---
title: Unterstützen von Schattenmaps für unterschiedliche Hardware
description: Rendern Sie Schatten in noch besserer Qualität auf schnelleren Geräten und schnellere Schatten auf weniger leistungsfähigen Geräten.
ms.assetid: d97c0544-44f2-4e29-5e02-54c45e0dff4e
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Spiele, Schattenmaps, DirectX
ms.localizationpriority: medium
ms.openlocfilehash: 1087a063fa19bea716b86143c10097711cef9205
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66367898"
---
# <a name="support-shadow-maps-on-a-range-of-hardware"></a>Unterstützen von Schattenmaps für unterschiedliche Hardware




Rendern Sie Schatten in noch besserer Qualität auf schnelleren Geräten und schnellere Schatten auf weniger leistungsfähigen Geräten. Teil 4 von [Exemplarische Vorgehensweise: Implementieren Sie mithilfe von Tiefenpuffern in Direct3D 11 Schattenvolumen](implementing-depth-buffers-for-shadow-mapping.md).

## <a name="comparison-filter-types"></a>Arten von Vergleichsfiltern


Verwenden Sie die lineare Filterung nur, wenn das Gerät die Leistungseinbußen verkraften kann. Im Allgemeinen Direct3D-Funktionsebene 9\_1 Geräte nicht ausreichend Leistung, für die lineare Filterung auf Schatten übrig haben. Nutzen Sie auf diesen Geräten stattdessen die Punktfilterung. Passen Sie beim Verwenden der linearen Filterung den Pixelshader so an, dass die Schattenkanten ineinander verlaufen.

Erstellen Sie den Vergleichssampler für die Punktfilterung:

```cpp
D3D11_SAMPLER_DESC comparisonSamplerDesc;
ZeroMemory(&comparisonSamplerDesc, sizeof(D3D11_SAMPLER_DESC));
comparisonSamplerDesc.AddressU = D3D11_TEXTURE_ADDRESS_BORDER;
comparisonSamplerDesc.AddressV = D3D11_TEXTURE_ADDRESS_BORDER;
comparisonSamplerDesc.AddressW = D3D11_TEXTURE_ADDRESS_BORDER;
comparisonSamplerDesc.BorderColor[0] = 1.0f;
comparisonSamplerDesc.BorderColor[1] = 1.0f;
comparisonSamplerDesc.BorderColor[2] = 1.0f;
comparisonSamplerDesc.BorderColor[3] = 1.0f;
comparisonSamplerDesc.MinLOD = 0.f;
comparisonSamplerDesc.MaxLOD = D3D11_FLOAT32_MAX;
comparisonSamplerDesc.MipLODBias = 0.f;
comparisonSamplerDesc.MaxAnisotropy = 0;
comparisonSamplerDesc.ComparisonFunc = D3D11_COMPARISON_LESS_EQUAL;
comparisonSamplerDesc.Filter = D3D11_FILTER_COMPARISON_MIN_MAG_MIP_POINT;

// Point filtered shadows can be faster, and may be a good choice when
// rendering on hardware with lower feature levels. This sample has a
// UI option to enable/disable filtering so you can see the difference
// in quality and speed.

DX::ThrowIfFailed(
    pD3DDevice->CreateSamplerState(
        &comparisonSamplerDesc,
        &m_comparisonSampler_point
        )
    );
```

Erstellen Sie anschließend einen Sampler für die lineare Filterung:

```cpp
comparisonSamplerDesc.Filter = D3D11_FILTER_COMPARISON_MIN_MAG_MIP_LINEAR;
DX::ThrowIfFailed(
    pD3DDevice->CreateSamplerState(
        &comparisonSamplerDesc,
        &m_comparisonSampler_linear
        )
    );
```

Wählen Sie einen Sampler aus:

```cpp
ID3D11PixelShader* pixelShader;
ID3D11SamplerState** comparisonSampler;
if (m_useLinear)
{
    pixelShader = m_shadowPixelShader_linear.Get();
    comparisonSampler = m_comparisonSampler_linear.GetAddressOf();
}
else
{
    pixelShader = m_shadowPixelShader_point.Get();
    comparisonSampler = m_comparisonSampler_point.GetAddressOf();
}

// Attach our pixel shader.
context->PSSetShader(
    pixelShader,
    nullptr,
    0
    );

context->PSSetSamplers(0, 1, comparisonSampler);
context->PSSetShaderResources(0, 1, m_shadowResourceView.GetAddressOf());
```

Schattenkanten mit linearer Filterung vermischen:

```cpp
// Blends the shadow area into the lit area.
float3 light = lighting * (ambient + DplusS(N, L, NdotL, input.view));
float3 shadow = (1.0f - lighting) * ambient;
return float4(input.color * (light + shadow), 1.f);
```

## <a name="shadow-buffer-size"></a>Schattenpuffergröße


Größere Schattenmaps sehen weniger "eckig" aus, aber sie nehmen mehr Speicherplatz im Grafikspeicher ein. Experimentieren Sie im Spiel mit unterschiedlichen Größen von Schattenmaps, und beobachten Sie, welche Ergebnisse Sie für unterschiedliche Arten von Geräten und unterschiedliche Anzeigegrößen erzielen. Erwägen Sie eine Optimierung, z. B. mithilfe von kaskadierenden Schattenmaps, um bessere Ergebnisse mit geringerem Grafikspeicheraufwand zu erhalten. Weitere Informationen finden Sie unter [Häufig verwendete Methoden zur Verbesserung von Tiefenmaps für Schatten](https://docs.microsoft.com/windows/desktop/DxTechArts/common-techniques-to-improve-shadow-depth-maps).

## <a name="shadow-buffer-depth"></a>Schattenpuffertiefe


Eine höhere Präzision im Schattenpuffer führt zu genaueren Ergebnissen bei Tiefentests. Dies trägt zur Verhinderung von Problemen bei, z. B. von Z-Bufferkonflikten. Wie bei größeren Schattenmaps auch wird bei einer höheren Präzision mehr Speicher belegt. Experimentieren Sie mit verschiedenen Tiefe Genauigkeit Typen in Ihrem Spiel - DXGI\_FORMAT\_R24G8\_TYPELESS im Vergleich zur DXGI\_FORMAT\_R16\_TYPELESS - und beobachten Sie die Geschwindigkeit und Qualität auf verschiedene Funktionsebenen.

## <a name="optimizing-precompiled-shaders"></a>Optimieren vorkompilierter Shader


UWP-Apps (Universelle Windows-Plattform) können eine dynamische Shaderkompilierung nutzen, die Verwendung einer dynamischen Shaderverknüpfung ist jedoch schneller. Sie können auch Compilerdirektiven und `#ifdef`-Blöcke verwenden, um unterschiedliche Versionen von Shadern zu laden. Dazu wird die Visual Studio-Projektdatei in einem Text-Editor geöffnet, und es werden mehrere `<FxcCompiler>`-Einträge für den HLSL-Code hinzugefügt (jeweils mit den passenden Präprozessordefinitionen). Beachten Sie, dass dies auf einen andere Dateinamen aktualisiert. In diesem Fall fügt Visual Studio \_zeigen und \_linear zu den verschiedenen Versionen des Shaders.

Im Projektdateieintrag für die linear gefilterte Version des Shaders wird LINEAR definiert:

```xml
<FxCompile Include="Content\ShadowPixelShader.hlsl">
  <ShaderType Condition="'$(Configuration)|$(Platform)'=='Debug|ARM'">Pixel</ShaderType>
  <ShaderType Condition="'$(Configuration)|$(Platform)'=='Release|ARM'">Pixel</ShaderType>
  <ShaderType Condition="'$(Configuration)|$(Platform)'=='Debug|Win32'">Pixel</ShaderType>
  <ShaderType Condition="'$(Configuration)|$(Platform)'=='Release|Win32'">Pixel</ShaderType>
  <ShaderType Condition="'$(Configuration)|$(Platform)'=='Debug|x64'">Pixel</ShaderType>
  <ShaderType Condition="'$(Configuration)|$(Platform)'=='Release|x64'">Pixel</ShaderType>
  <DisableOptimizations Condition="'$(Configuration)|$(Platform)'=='Debug|ARM'">false</DisableOptimizations>
  <EnableDebuggingInformation Condition="'$(Configuration)|$(Platform)'=='Debug|ARM'">true</EnableDebuggingInformation>
  <DisableOptimizations Condition="'$(Configuration)|$(Platform)'=='Debug|Win32'">false</DisableOptimizations>
  <DisableOptimizations Condition="'$(Configuration)|$(Platform)'=='Debug|x64'">false</DisableOptimizations>
  <ObjectFileOutput Condition="'$(Configuration)|$(Platform)'=='Debug|ARM'">$(OutDir)%(Filename)_linear.cso</ObjectFileOutput>
  <ObjectFileOutput Condition="'$(Configuration)|$(Platform)'=='Release|ARM'">$(OutDir)%(Filename)_linear.cso</ObjectFileOutput>
  <ObjectFileOutput Condition="'$(Configuration)|$(Platform)'=='Debug|Win32'">$(OutDir)%(Filename)_linear.cso</ObjectFileOutput>
  <ObjectFileOutput Condition="'$(Configuration)|$(Platform)'=='Release|Win32'">$(OutDir)%(Filename)_linear.cso</ObjectFileOutput>
  <ObjectFileOutput Condition="'$(Configuration)|$(Platform)'=='Debug|x64'">$(OutDir)%(Filename)_linear.cso</ObjectFileOutput>
  <ObjectFileOutput Condition="'$(Configuration)|$(Platform)'=='Release|x64'">$(OutDir)%(Filename)_linear.cso</ObjectFileOutput>
  <PreprocessorDefinitions Condition="'$(Configuration)|$(Platform)'=='Debug|ARM'">LINEAR</PreprocessorDefinitions>
  <PreprocessorDefinitions Condition="'$(Configuration)|$(Platform)'=='Release|ARM'">LINEAR</PreprocessorDefinitions>
  <PreprocessorDefinitions Condition="'$(Configuration)|$(Platform)'=='Debug|Win32'">LINEAR</PreprocessorDefinitions>
  <PreprocessorDefinitions Condition="'$(Configuration)|$(Platform)'=='Release|Win32'">LINEAR</PreprocessorDefinitions>
  <PreprocessorDefinitions Condition="'$(Configuration)|$(Platform)'=='Debug|x64'">LINEAR</PreprocessorDefinitions>
  <PreprocessorDefinitions Condition="'$(Configuration)|$(Platform)'=='Release|x64'">LINEAR</PreprocessorDefinitions>
</FxCompile>
```

Der Projektdateieintrag für die linear gefilterte Version des Shaders enthält keine Präprozessordefinitionen:

```xml
<FxCompile Include="Content\ShadowPixelShader.hlsl">
  <ShaderType Condition="'$(Configuration)|$(Platform)'=='Debug|ARM'">Pixel</ShaderType>
  <ShaderType Condition="'$(Configuration)|$(Platform)'=='Release|ARM'">Pixel</ShaderType>
  <ShaderType Condition="'$(Configuration)|$(Platform)'=='Debug|Win32'">Pixel</ShaderType>
  <ShaderType Condition="'$(Configuration)|$(Platform)'=='Release|Win32'">Pixel</ShaderType>
  <ShaderType Condition="'$(Configuration)|$(Platform)'=='Debug|x64'">Pixel</ShaderType>
  <ShaderType Condition="'$(Configuration)|$(Platform)'=='Release|x64'">Pixel</ShaderType>
  <DisableOptimizations Condition="'$(Configuration)|$(Platform)'=='Debug|ARM'">false</DisableOptimizations>
  <EnableDebuggingInformation Condition="'$(Configuration)|$(Platform)'=='Debug|ARM'">true</EnableDebuggingInformation>
  <DisableOptimizations Condition="'$(Configuration)|$(Platform)'=='Debug|Win32'">false</DisableOptimizations>
  <DisableOptimizations Condition="'$(Configuration)|$(Platform)'=='Debug|x64'">false</DisableOptimizations>
  <ObjectFileOutput Condition="'$(Configuration)|$(Platform)'=='Debug|ARM'">$(OutDir)%(Filename)_point.cso</ObjectFileOutput>
  <ObjectFileOutput Condition="'$(Configuration)|$(Platform)'=='Release|ARM'">$(OutDir)%(Filename)_point.cso</ObjectFileOutput>
  <ObjectFileOutput Condition="'$(Configuration)|$(Platform)'=='Debug|Win32'">$(OutDir)%(Filename)_point.cso</ObjectFileOutput>
  <ObjectFileOutput Condition="'$(Configuration)|$(Platform)'=='Release|Win32'">$(OutDir)%(Filename)_point.cso</ObjectFileOutput>
  <ObjectFileOutput Condition="'$(Configuration)|$(Platform)'=='Debug|x64'">$(OutDir)%(Filename)_point.cso</ObjectFileOutput>
  <ObjectFileOutput Condition="'$(Configuration)|$(Platform)'=='Release|x64'">$(OutDir)%(Filename)_point.cso</ObjectFileOutput>
</FxCompile>
```

 

 




