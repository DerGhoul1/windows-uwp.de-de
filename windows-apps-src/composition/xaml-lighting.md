---
title: XAML-Beleuchtung
description: Helle Objekte werden zusammen mit SceneLightingEffect verwendet, um dynamische Beleuchtung und Reflexionsvermögen zu simulieren.
ms.date: 06/28/2018
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
dev_langs:
- csharp
- vb
- cppcx
- cppwinrt
ms.openlocfilehash: 768509b3b22eef32e26d9e5423c00e2ee65f4b81
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/21/2019
ms.locfileid: "67318032"
---
# <a name="xaml-lighting"></a>XAML-Beleuchtung

[**CompositionLight** ](/uwp/api/Windows.UI.Composition.CompositionLight) Objekte werden verwendet, in Verbindung mit [ **SceneLightingEffect** ](/uwp/api/Windows.UI.Composition.Effects.SceneLightingEffect) , dynamische Beleuchtung und Reflexionsvermögen zu simulieren.

Sie können Lichter auf [**visuelle Elemente**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.Visual) und XAML-[**UI-Elemente**](/uwp/api/Windows.UI.Xaml.UIElement) anwenden.

## <a name="applying-lights-to-xaml-uielements"></a>Anwenden von Lichtern auf XAML-UI-Elemente

[**XamlLight** ](/uwp/api/windows.ui.xaml.media.xamllight) Objekte werden zum Anwenden [ **CompositionLights** ](/uwp/api/Windows.UI.Composition.CompositionLight) auf XAML-UIElements dynamisch light. XamlLight bietet Methoden für die Zielgruppenadressierung von UIElements oder XAML-Pinsel, Anwenden von Beleuchtung auf Strukturen von UIElements, und hilft, Verwalten der Lebensdauer von CompositionLight Ressourcen basierend auf der gibt an, ob sie sich gerade befinden.

- Wenn Sie XamlLight auf einen **Pinsel** anwenden, werden damit alle UI-Elemente beleuchtet, die den Pinsel nutzen.
- Wenn Sie XamlLight auf ein **UI-Element** anwenden, wird damit das gesamte UI-Element einschließlich seiner Unterelemente beleuchtet.

## <a name="creating-and-using-a-xamllight"></a>Erstellen und Verwenden von XamlLight

[**XamlLight** ](/uwp/api/windows.ui.xaml.media.xamllight) ist eine Basisklasse, die zum Erstellen von benutzerdefinierten Lichter verwendet werden kann.

Dieses Beispiel zeigt die Definition für eine benutzerdefinierte XamlLight, der einen mehrfarbigen Spotlight für gezielte UIElements und Pinsel gilt.

```csharp
public sealed class OrangeSpotLight : XamlLight
{
    // Register an attached property that lets you set a UIElement
    // or Brush as a target for this light type in markup.
    public static readonly DependencyProperty IsTargetProperty =
        DependencyProperty.RegisterAttached(
        "IsTarget",
        typeof(bool),
        typeof(OrangeSpotLight),
        new PropertyMetadata(null, OnIsTargetChanged)
    );

    public static void SetIsTarget(DependencyObject target, bool value)
    {
        target.SetValue(IsTargetProperty, value);
    }

    public static Boolean GetIsTarget(DependencyObject target)
    {
        return (bool)target.GetValue(IsTargetProperty);
    }

    // Handle attached property changed to automatically target and untarget UIElements and Brushes.
    private static void OnIsTargetChanged(DependencyObject obj, DependencyPropertyChangedEventArgs e)
    {
        var isAdding = (bool)e.NewValue;

        if (isAdding)
        {
            if (obj is UIElement)
            {
                XamlLight.AddTargetElement(GetIdStatic(), obj as UIElement);
            }
            else if (obj is Brush)
            {
                XamlLight.AddTargetBrush(GetIdStatic(), obj as Brush);
            }
        }
        else
        {
            if (obj is UIElement)
            {
                XamlLight.RemoveTargetElement(GetIdStatic(), obj as UIElement);
            }
            else if (obj is Brush)
            {
                XamlLight.RemoveTargetBrush(GetIdStatic(), obj as Brush);
            }
        }
    }

    protected override void OnConnected(UIElement newElement)
    {
        if (CompositionLight == null)
        {
            // OnConnected is called when the first target UIElement is shown on the screen.
            // This lets you delay creation of the composition object until it's actually needed.
            var spotLight = Window.Current.Compositor.CreateSpotLight();
            spotLight.InnerConeColor = Colors.Orange;
            spotLight.OuterConeColor = Colors.Yellow;
            spotLight.InnerConeAngleInDegrees = 30;
            spotLight.OuterConeAngleInDegrees = 45;
            CompositionLight = spotLight;
        }
    }

    protected override void OnDisconnected(UIElement oldElement)
    {
        // OnDisconnected is called when there are no more target UIElements on the screen.
        // The CompositionLight should be disposed when no longer required.
        // For SDK 15063, see Remarks in the XamlLight class documentation.
        if (CompositionLight != null)
        {
            CompositionLight.Dispose();
            CompositionLight = null;
        }
    }

    protected override string GetId()
    {
        return GetIdStatic();
    }

    private static string GetIdStatic()
    {
        // This specifies the unique name of the light.
        // In most cases you should use the type's FullName.
        return typeof(OrangeSpotLight).FullName;
    }
}
```

```vb
Public NotInheritable Class OrangeSpotLight
    Inherits XamlLight

    ' Register an attached property that lets you set a UIElement
    ' or Brush as a target for this light type in markup.
    Public Shared ReadOnly IsTargetProperty As DependencyProperty = DependencyProperty.RegisterAttached(
            "IsTarget",
            GetType(Boolean),
            GetType(OrangeSpotLight),
            New PropertyMetadata(Nothing, New PropertyChangedCallback(AddressOf OnIsTargetChanged)
            )
        )

    Public Shared Sub SetIsTarget(target As DependencyObject, value As Boolean)
        target.SetValue(IsTargetProperty, value)
    End Sub

    Public Shared Function GetIsTarget(target As DependencyObject) As Boolean
        Return DirectCast(target.GetValue(IsTargetProperty), Boolean)
    End Function

    ' Handle attached property changed to automatically target And untarget UIElements And Brushes.
    Public Shared Sub OnIsTargetChanged(obj As DependencyObject, e As DependencyPropertyChangedEventArgs)
        Dim isAdding = DirectCast(e.NewValue, Boolean)

        If isAdding Then
            If TypeOf obj Is UIElement Then
                XamlLight.AddTargetElement(GetIdStatic(), TryCast(obj, UIElement))
            ElseIf TypeOf obj Is Brush Then
                XamlLight.AddTargetBrush(GetIdStatic(), TryCast(obj, Brush))
            End If
        Else
            If TypeOf obj Is UIElement Then
                XamlLight.RemoveTargetElement(GetIdStatic(), TryCast(obj, UIElement))
            ElseIf TypeOf obj Is Brush Then
                XamlLight.RemoveTargetBrush(GetIdStatic(), TryCast(obj, Brush))
            End If
        End If
    End Sub

    Protected Overrides Sub OnConnected(newElement As UIElement)
        If CompositionLight Is Nothing Then
            ' OnConnected Is called when the first target UIElement Is shown on the screen.
            ' This lets you delay creation of the composition object until it's actually needed.
            Dim spotLight = Window.Current.Compositor.CreateSpotLight()
            spotLight.InnerConeColor = Colors.Orange
            spotLight.OuterConeColor = Colors.Yellow
            spotLight.InnerConeAngleInDegrees = 30
            spotLight.OuterConeAngleInDegrees = 45
            CompositionLight = spotLight
        End If
    End Sub

    Protected Overrides Sub OnDisconnected(oldElement As UIElement)
        ' OnDisconnected Is called when there are no more target UIElements on the screen.
        ' The CompositionLight should be disposed when no longer required.
        If CompositionLight IsNot Nothing Then
            CompositionLight.Dispose()
            CompositionLight = Nothing
        End If
    End Sub

    Protected Overrides Function GetId() As String
        Return GetIdStatic()
    End Function

    Private Shared Function GetIdStatic() As String
        ' This specifies the unique name of the light.
        ' In most cases you should use the type's FullName.
        Return GetType(OrangeSpotLight).FullName
    End Function
End Class
```

```cppwinrt
// For the C++/WinRT code example below, you'll need to add a Midl File (.idl) file to your project.

// OrangeSpotLight.idl
namespace MyApp
{
    [default_interface]
    runtimeclass OrangeSpotLight : Windows.UI.Xaml.Media.XamlLight
    {
        OrangeSpotLight();
        static Windows.UI.Xaml.DependencyProperty IsTargetProperty{ get; };
        static Boolean GetIsTarget(Windows.UI.Xaml.DependencyObject target);
        static void SetIsTarget(Windows.UI.Xaml.DependencyObject target, Boolean value);
    }
}

// OrangeSpotLight.h
struct OrangeSpotLight : OrangeSpotLightT<OrangeSpotLight>
{
    OrangeSpotLight() = default;

    winrt::hstring GetId();

    static Windows::UI::Xaml::DependencyProperty IsTargetProperty() { return m_isTargetProperty; }

    static bool GetIsTarget(Windows::UI::Xaml::DependencyObject const& target)
    {
        return winrt::unbox_value<bool>(target.GetValue(m_isTargetProperty));
    }

    static void SetIsTarget(Windows::UI::Xaml::DependencyObject const& target, bool value)
    {
        target.SetValue(m_isTargetProperty, winrt::box_value(value));
    }

    void OnConnected(Windows::UI::Xaml::UIElement const& newElement);
    void OnDisconnected(Windows::UI::Xaml::UIElement const& oldElement);

    static void OnIsTargetChanged(Windows::UI::Xaml::DependencyObject const& d, Windows::UI::Xaml::DependencyPropertyChangedEventArgs const& e);

    inline static winrt::hstring GetIdStatic()
    {
        // This specifies the unique name of the light. In most cases you should use the type's full name.
        return winrt::xaml_typename<MyApp::OrangeSpotLight>().Name;
    }

private:
    static Windows::UI::Xaml::DependencyProperty m_isTargetProperty;
};

// OrangeSpotLight.cpp
Windows::UI::Xaml::DependencyProperty OrangeSpotLight::m_isTargetProperty =
    Windows::UI::Xaml::DependencyProperty::RegisterAttached(
        L"IsTarget",
        winrt::xaml_typename<bool>(),
        winrt::xaml_typename<MyApp::OrangeSpotLight>(),
        Windows::UI::Xaml::PropertyMetadata{ winrt::box_value(false), Windows::UI::Xaml::PropertyChangedCallback{ &OrangeSpotLight::OnIsTargetChanged } }
);

void OrangeSpotLight::OnConnected(Windows::UI::Xaml::UIElement const& /* newElement */)
{
    if (!CompositionLight())
    {
        // OnConnected is called when the first target UIElement is shown on the screen. This enables delaying composition object creation until it's actually necessary.
        auto spotLight{ Windows::UI::Xaml::Window::Current().Compositor().CreateSpotLight() };
        spotLight.InnerConeColor(Windows::UI::Colors::Orange());
        spotLight.OuterConeColor(Windows::UI::Colors::Yellow());
        spotLight.InnerConeAngleInDegrees(30);
        spotLight.OuterConeAngleInDegrees(45);
        CompositionLight(spotLight);
    }
}

void OrangeSpotLight::OnDisconnected(Windows::UI::Xaml::UIElement const& /* oldElement */)
{
    // OnDisconnected is called when there are no more target UIElements on the screen.
    // Dispose of composition resources when no longer in use.
    if (CompositionLight())
    {
        CompositionLight(nullptr);
    }
}

winrt::hstring OrangeSpotLight::GetId()
{
    return OrangeSpotLight::GetIdStatic();
}

void OrangeSpotLight::OnIsTargetChanged(Windows::UI::Xaml::DependencyObject const& d, Windows::UI::Xaml::DependencyPropertyChangedEventArgs const& e)
{
    auto uie{ d.try_as<Windows::UI::Xaml::UIElement>() };
    auto brush{ d.try_as<Windows::UI::Xaml::Media::Brush>() };

    auto isAdding = winrt::unbox_value<bool>(e.NewValue());
    if (isAdding)
    {

        if (uie)
        {
            Windows::UI::Xaml::Media::XamlLight::AddTargetElement(OrangeSpotLight::GetIdStatic(), uie);
        }
        else if (brush)
        {
            Windows::UI::Xaml::Media::XamlLight::AddTargetBrush(OrangeSpotLight::GetIdStatic(), brush);
        }
    }
    else
    {
        if (uie)
        {
            Windows::UI::Xaml::Media::XamlLight::RemoveTargetElement(OrangeSpotLight::GetIdStatic(), uie);
        }
        else if (brush)
        {
            Windows::UI::Xaml::Media::XamlLight::RemoveTargetBrush(OrangeSpotLight::GetIdStatic(), brush);
        }
    }
}

// MainPage.h
...
#include "OrangeSpotLight.h"
...
struct MainPage : MainPageT<MainPage>
{
    MainPage()
    {
        InitializeComponent();

        OrangeSpotLight::SetIsTarget(spotlitBrush(), true);
        OrangeSpotLight::SetIsTarget(spotlitUIElement(), true);
    }
...
};
```

```cppcx
// OrangeSpotLight.h:
public ref class OrangeSpotLight sealed :
    public Windows::UI::Xaml::Media::XamlLight
{
public:
    OrangeSpotLight();

    static property Windows::UI::Xaml::DependencyProperty^ IsTargetProperty
    {
        Windows::UI::Xaml::DependencyProperty^ get() { return m_isTargetProperty; }
    };
    static void SetIsTarget(Windows::UI::Xaml::DependencyObject^ target, bool value);
    static bool GetIsTarget(Windows::UI::Xaml::DependencyObject^ target);

protected:
    virtual void OnConnected(Windows::UI::Xaml::UIElement^ newElement) override;
    virtual void OnDisconnected(Windows::UI::Xaml::UIElement^ oldElement) override;
    virtual Platform::String^ GetId() override;

private:
    static Windows::UI::Xaml::DependencyProperty^ m_isTargetProperty;
    static void OnIsTargetChanged(Windows::UI::Xaml::DependencyObject^ obj, Windows::UI::Xaml::DependencyPropertyChangedEventArgs^ e);

    inline static Platform::String^ GetIdStatic()
    {
        // This specifies the unique name of the light. In most cases you should use the type's FullName.
        return OrangeSpotLight::typeid->FullName;
    }
};

//OrangeSpotLight.cpp:

// Register an attached property that lets you set a UIElement
// or Brush as a target for this light type in markup.
DependencyProperty^ OrangeSpotLight::m_isTargetProperty = DependencyProperty::RegisterAttached(
    "IsTarget",
    bool::typeid,
    OrangeSpotLight::typeid,
    ref new PropertyMetadata(0.0, ref new PropertyChangedCallback(OnIsTargetChanged))
);

OrangeSpotLight::OrangeSpotLight()
{
}

void OrangeSpotLight::SetIsTarget(DependencyObject^ target, bool value)
{
    target->SetValue(IsTargetProperty, value);
}

bool OrangeSpotLight::GetIsTarget(DependencyObject^ target)
{
    return static_cast<bool>(target->GetValue(IsTargetProperty));
}

// Handle attached property changed to automatically target and untarget UIElements and Brushes.
void OrangeSpotLight::OnIsTargetChanged(DependencyObject^ obj, DependencyPropertyChangedEventArgs^ e)
{
    auto isAdding = static_cast<bool>(e->NewValue);

    if (isAdding)
    {
        if (dynamic_cast<UIElement^>(obj))
        {
            XamlLight::AddTargetElement(GetIdStatic(), static_cast<UIElement^>(obj));
        }
        else if (dynamic_cast<Brush^>(obj))
        {
            XamlLight::AddTargetBrush(GetIdStatic(), static_cast<Brush^>(obj));
        }
    }
    else
    {
        if (dynamic_cast<UIElement^>(obj))
        {
            XamlLight::RemoveTargetElement(GetIdStatic(), static_cast<UIElement^>(obj));
        }
        else if (dynamic_cast<Brush^>(obj))
        {
            XamlLight::RemoveTargetBrush(GetIdStatic(), static_cast<Brush^>(obj));
        }
    }
}
void OrangeSpotLight::OnConnected(UIElement^ newElement)
{
    if (CompositionLight == nullptr)
    {
        // OnConnected is called when the first target UIElement is shown on the screen.
        // This lets you delay creation of the composition object until it's actually needed.
        auto spotLight = Window::Current->Compositor->CreateSpotLight();
        spotLight->InnerConeColor = Colors::Orange;
        spotLight->OuterConeColor = Colors::Yellow;
        spotLight->InnerConeAngleInDegrees = 30;
        spotLight->OuterConeAngleInDegrees = 45;
        CompositionLight = spotLight;
    }
}

void OrangeSpotLight::OnDisconnected(UIElement^ oldElement)
{
    // OnDisconnected is called when there are no more target UIElements on the screen.
    // The CompositionLight should be disposed when no longer required.
    // For SDK 15063, see Remarks in the XamlLight class documentation.
    if (CompositionLight != nullptr)
    {
        delete CompositionLight;
        CompositionLight = nullptr;
    }
}

Platform::String^ OrangeSpotLight::GetId()
{
    return GetIdStatic();
}
```

Sie können dann dieses Licht auf einem beliebigen XAML "UIElement" oder den Pinsel, um diese light anwenden. Dieses Beispiel zeigt verschiedene mögliche Verwendungen.

> [!Important]
> Für [C++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt), entfernen Sie die zwei Vorkommen des `local:OrangeSpotLight.IsTarget="True"` aus dem folgenden Markup. Die angefügten Eigenschaften werden im Code-Behind bereits festgelegt.

```xaml
<StackPanel Width="100">
    <StackPanel.Lights>
        <local:OrangeSpotLight/>
    </StackPanel.Lights>

    <!-- This border is lit by the OrangeSpotLight, but its content is not. -->
    <Border BorderThickness="4" Margin="2">
        <Border.BorderBrush>
            <SolidColorBrush x:Name="spotlitBrush" Color="White" local:OrangeSpotLight.IsTarget="True"/>
        </Border.BorderBrush>
        <Rectangle Fill="LightGray" Height="20"/>
    </Border>

    <!-- This border and its content are lit by the OrangeSpotLight. -->
    <Border x:Name="spotlitUIElement" BorderThickness="4" BorderBrush="PaleGreen" Margin="2"
            local:OrangeSpotLight.IsTarget="True">
        <Rectangle Fill="LightGray" Height="20"/>
    </Border>

    <!-- This border and its content are not lit by the OrangeSpotLight. -->
    <Border BorderThickness="4" BorderBrush="PaleGreen" Margin="2">
        <Rectangle Fill="LightGray" Height="20"/>
    </Border>
</StackPanel>
```

Die Ergebnisse der diese XAML werden wie folgt aussehen.

![Beispiele für Elemente, die von einem Xaml-Licht leuchtet](images/orange-spot-light.png)

> [!Important]
> Die Einstellung UIElement.Lights im Markup des obigen Beispiels wird nur für Apps mit mindestens gleicher oder höherer Version des Windows 10 Creators-Updates unterstützt. Für Apps, die ältere Versionen verwenden, müssen die Lichter in CodeBehind erstellt werden.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* Erweiterte Beispiele für Benutzeroberfläche und Komposition finden Sie im [WindowsUIDevLabs-GitHub](https://github.com/microsoft/WindowsCompositionSamples).
