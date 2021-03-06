---
title: Exemplarische Vorgehensweise -- Portieren von Direct3D 9 zu DirectX 11 und UWP
description: In dieser Portierungsübung wird veranschaulicht, wie Sie ein einfaches Renderingframework von Direct3D 9 auf Direct3D 11 und die universelle Windows-Plattform (UWP) umstellen.
ms.assetid: d4467e1f-929b-a4b8-b233-e142a8714c96
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Spiele, Directx, Port, direct3d 9, direct3d 11
ms.localizationpriority: medium
ms.openlocfilehash: 5d4aef73b9b28d631a492436ff90761541134220
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66367426"
---
# <a name="walkthrough-port-a-simple-direct3d-9-app-to-directx-11-and-universal-windows-platform-uwp"></a>Exemplarische Vorgehensweise: Portieren Sie eine einfache Direct3D 9-app, die DirectX 11 und die universelle Windows-Plattform (UWP)



In dieser Portierungsübung wird veranschaulicht, wie Sie ein einfaches Renderingframework von Direct3D 9 auf Direct3D 11 und die universelle Windows-Plattform (UWP) umstellen.
## 
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Thema</th>
<th align="left">Beschreibung</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="simple-port-from-direct3d-9-to-11-1-part-1--initializing-direct3d.md">Initialisieren von Direct3D 11</a></p></td>
<td align="left"><p>Hier wird veranschaulicht, wie Sie Direct3D 9-Initialisierungscode in Direct3D 11 konvertieren, und Sie erfahren, wie Sie Handles zum Direct3D-Gerät und zum Gerätekontext abrufen und DXGI zum Einrichten einer Swapchain verwenden.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="simple-port-from-direct3d-9-to-11-1-part-2--rendering.md">Konvertieren Sie die Rendering-framework</a></p></td>
<td align="left"><p>Hier wird veranschaulicht, wie Sie ein einfaches Renderingframework von Direct3D 9 in Direct3D 11 konvertieren. Sie erfahren, wie Sie Geometriepuffer portieren, HLSL-Shaderprogramme kompilieren und laden und die Renderkette in Direct3D 11 implementieren.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="simple-port-from-direct3d-9-to-11-1-part-3--viewport-and-game-loop.md">Die spielschleife Port</a></p></td>
<td align="left"><p>In diesem Thema wird veranschaulicht, wie Sie ein Fenster für ein UWP-Spiel (Universelle Windows-Plattform) implementieren und die Spielschleife übertragen. Außerdem wird die Erstellung eines <a href="https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.IFrameworkView"><strong>IFrameworkView</strong></a>-Elements zum Steuern eines <a href="https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow"><strong>CoreWindow</strong></a>-Vollbilds erläutert.</p></td>
</tr>
</tbody>
</table>

 

Es werden zwei Codepfade durchgegangen, mit denen jeweils die gleiche grundlegende Grafikaufgabe durchgeführt wird: das Anzeigen eines sich drehenden Würfels mit Vertexschattierung. In beiden Fällen wird mit dem Code der folgende Prozess abgedeckt:

1.  Erstellen eines Direct3D-Geräts und einer Swapchain
2.  Erstellen eines Vertexpuffers und eines Indexpuffers zum Darstellen eines farbigen Würfelgitters
3.  Erstellen eines Vertexshaders, mit dem Vertizes in Bildschirmbereiche umgewandelt werden, eines Pixelshaders, mit dem Farbwerte vermischt werden, Kompilieren der Shader und Laden der Shader als Direct3D-Ressourcen
4.  Implementieren der Renderkette und Darstellen des gezeichneten Würfels auf dem Bildschirm
5.  Erstellen eines Fensters, Starten einer Hauptschleife und Durchführen der Verarbeitung von Bildschirmmeldungen

Nach der Durcharbeitung dieser exemplarischen Vorgehensweise sollten Ihnen die folgenden grundlegenden Unterschiede zwischen Direct3D 9 und Direct3D 11 bekannt sein:

-   Trennung von Gerät, Gerätekontext und Grafikinfrastruktur
-   Kompilierungsprozess für Shader und Laden von Shaderbytecode zur Laufzeit
-   Konfigurieren von Daten pro Vertex für den Eingabeassemblerzustand
-   Verwenden eines [**IFrameworkView**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.IFrameworkView)-Elements zum Erstellen einer CoreWindow-Ansicht

Beachten Sie, dass in dieser exemplarischen Vorgehensweise der Einfachheit halber [**CoreWindow**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow) verwendet wird. Die XAML-Interoperabilität wird nicht behandelt.

## <a name="prerequisites"></a>Vorraussetzungen


Führen Sie die Schritte unter [Vorbereiten der Entwicklungsumgebung für die Entwicklung von UWP-DirectX-Spielen](prepare-your-dev-environment-for-windows-store-directx-game-development.md) aus. Ist nicht erforderlich, eine Vorlage noch, jedoch benötigen Sie Microsoft Visual Studio 2015, um die Codebeispiele in dieser exemplarischen Vorgehensweise zu laden.

Lesen Sie sich die Informationen unter [Konzepte und Aspekte der Portierung](porting-considerations.md) durch, um ein besseres Verständnis der in dieser exemplarischen Vorgehensweise verwendeten Programmierkonzepte für DirectX 11 und UWP zu entwickeln.

## <a name="related-topics"></a>Verwandte Themen

**Direct3D**

* [Schreiben von HLSL-Shadern in Direct3D 9](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-writing-shaders-9)
* [DirectX-Spiele-Projektvorlagen](user-interface.md)

**Microsoft Store**

* [**Microsoft::WRL::ComPtr**](https://docs.microsoft.com/cpp/windows/comptr-class)
* [**Handle für Objekt (^)** ](https://docs.microsoft.com/cpp/windows/handle-to-object-operator-hat-cpp-component-extensions)

