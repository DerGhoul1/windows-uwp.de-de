---
author: stevewhims
description: Ein Einzelwert muss in ein Referenzklassenobjekt gepackt werden, bevor er an eine Funktion übergeben wird, die **IInspectable** erwartet. Dieser Wrapping-Prozess wird als *Boxing* des Wertes bezeichnet.
title: Boxing und Unboxing von Einzelwerten für IInspectable mit C++/WinRT
ms.author: stwhi
ms.date: 04/10/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, standard, c++, cpp, winrt, projizierung, XAML, steuerelement, boxing, einzelwert
ms.localizationpriority: medium
ms.openlocfilehash: 61d5c7a35fb7a6ff9952f3fe768f4faa3f6c6347
ms.sourcegitcommit: ab92c3e0dd294a36e7f65cf82522ec621699db87
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/03/2018
ms.locfileid: "1832004"
---
# <a name="boxing-and-unboxing-scalar-values-to-iinspectable-with-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>Boxing und Unboxing von Einzelwerten für IInspectable mit [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 
Die [**IInspectable**](https://msdn.microsoft.com/library/windows/desktop/br205821)-Schnittstellen ist die Root-Schnittstelle jeder Laufzeitklasse in Windows-Runtime (WinRT). Dies ist eine Idee, die analoge zu [**IUnknown**](https://msdn.microsoft.com/library/windows/desktop/ms680509) am Stammknoten jeder COM-Schnittstelle und Klasse ist. **System.Object** ist der Stammknoten jeder [Common Type System](https://docs.microsoft.com/dotnet/standard/base-types/common-type-system)-Klasse.

Mit anderen Worten, eine Funktion, die **IInspectable** erwartet, kann eine Instanz einer beliebigen Laufzeitklasse übergeben werden. Sie können aber nicht direkt einen Einzelwert, wie z. B. einen Zahlen- oder Textwert, an eine solche Funktion übergeben. Stattdessen muss ein Einzelwert in ein Objekt der Referenzklasse gepackt werden. Dieser Wrapping-Prozess wird als *Boxing* des Wertes bezeichnet.

C++/WinRT stellt die Funktion [**winrt::box_value**](/uwp/cpp-ref-for-winrt/box-value) zur Verfügung, die einen Einzelwert entgegennimmt und den Boxed-Wert als **IInspectable** zurückgibt. Um ein **IInspectable** wieder in einen Einzelwert zu entpacken, gibt es die Funktionen [**winrt::unbox_value**](/uwp/cpp-ref-for-winrt/unbox-value) und [**winrt::unbox_value_or**](/uwp/cpp-ref-for-winrt/unbox-value-or).

## <a name="examples-of-boxing-a-value"></a>Beispiele für das Boxen eines Wertes
Die Zugriffsfunktion [**LaunchActivatedEventArgs::Arguments**](/uwp/api/windows.applicationmodel.activation.launchactivatedeventargs.Arguments) gibt einen [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) zurück, der ein Einzelwert ist. Wir können diesen **hstring**-Wert einpacken und an eine Funktion übergeben, die so ein **IInspectable** erwartet.

```cppwinrt
void App::OnLaunched(LaunchActivatedEventArgs const& e)
{
    ...
    rootFrame.Navigate(xaml_typename<BlankApp1::MainPage>(), winrt::box_value(e.Arguments()));
    ...
}
```

Um die Content-Eigenschaft eines XAML-[**Button**](/uwp/api/windows.ui.xaml.controls.button) festzulegen, rufen Sie die Zugriffsfunktion [**Button::Content**](/uwp/api/windows.ui.xaml.controls.contentcontrol.content?) auf. Um die Content-Eigenschaft auf einen String-Wert zu setzen, können Sie diesen Code verwenden.

```cppwinrt
Button().Content(winrt::box_value(L"Clicked"));
```

Zuerst wandelt der **hstring**-Konvertierungskonstruktor das String-Literal in einen **hstring** um. Dann wird die Überladung von **winrt::box_value**, die einen **hstring** benötigt, aufgerufen.

## <a name="examples-of-unboxing-an-iinspectable"></a>Beispiele für das Unboxing eines IInspectable
In Ihren eigenen Funktionen (die ein **IInspectable** erwarten) können Sie [**winrt::unbox_value**](/uwp/cpp-ref-for-winrt/unbox-value) zum Unboxing verwenden. Sie können [**winrt::unbox_value_or**](/uwp/cpp-ref-for-winrt/unbox-value-or) zum Unboxing mit einem Standardwert verwenden.

```cppwinrt
void Unbox(Windows::Foundation::IInspectable const& object)
{
    hstring hstringValue = unbox_value<hstring>(object); // Throws if object is not a boxed string.
    hstringValue = unbox_value_or<hstring>(object, L"Default"); // Returns L"Default" if object is not a boxed string.
    float floatValue = unbox_value_or<float>(object, 0.f); // Returns 0.0 if object is not a boxed float.
}
```

## <a name="important-apis"></a>Wichtige APIs
* [IInspectable-Schnittstelle](https://msdn.microsoft.com/library/windows/desktop/br205821)
* [winrt::box_value Funktionsvorlage](/uwp/cpp-ref-for-winrt/box-value)
* [winrt::unbox_value Funktionsvorlage](/uwp/cpp-ref-for-winrt/unbox-value)
* [winrt::unbox_value_or Funktionsvorlage](/uwp/cpp-ref-for-winrt/unbox-value-or)