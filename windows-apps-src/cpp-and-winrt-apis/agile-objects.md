---
author: stevewhims
description: Ein agiles Objekt ist ein Objekt, auf das von jedem Thread aus zugegriffen werden kann. Ihre C++/WinRT-Typen sind standardmäßig agil, aber Sie können diese Option deaktivieren.
title: Agile Objekte mit C++/WinRT
ms.author: stwhi
ms.date: 04/19/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, standard, c++, cpp, winrt, projektion, agil, objekt, agilität, IAgileObject
ms.localizationpriority: medium
ms.openlocfilehash: a9d65fad2b093b64166a6a1b5be033e23bdd905b
ms.sourcegitcommit: 91511d2d1dc8ab74b566aaeab3ef2139e7ed4945
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/30/2018
ms.locfileid: "1818364"
---
# <a name="agile-objects-in-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>Agile Objekte in [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)
In den meisten Fällen kann von jedem Thread aus auf eine Instanz einer Windows-Runtime-Klasse zugegriffen werden (wie auf ein Standard-C++ Objekt). Eine solche Klasse ist *agil*. Nur eine kleine Anzahl von Windows-Runtime-Klassen, die mit Windows bereitgestellt werden, sind nicht agil. Wenn Sie sie nutzen, müssen Sie ihr Threading-Modell und ihr Marshaling-Verhalten berücksichtigen (Marshaling ist die Weitergabe von Daten über eine Thread- oder eine Prozessgrenze). Es ist ein guter Standard für jedes Windows-Runtime-Objekt, agil zu sein, sodass Ihre eigenen C++/WinRT-Typen standardmäßig agil sind.

Sie können jedoch auch von dieser Regel abweichen. Möglicherweise haben Sie einen zwingenden Grund, ein Objekt Ihres Typs zu beschränken (zum Beispiel in einer Anwendung mit einem einzelnen Thread). Dies hat typischerweise mit Wiedereinsprungsvoraussetzungen zu tun. Aber auch die UI-APIs (User Interface, Benutzerschnittstelle) bieten zunehmend agile Objekte. Im Allgemeinen ist Agilität die einfachste und leistungsfähigste Option. Auch wenn Sie eine Aktivierungs-Factory implementieren, muss diese agil sein (auch, wenn Ihre entsprechende Laufzeitklasse es nicht ist).

> [!NOTE]
> Windows-Runtime basiert auf COM. Im Sinne von COM ist eine agile Klasse mit `ThreadingModel` = *Both* registriert. Weitere Informationen über COM-Threading-Modelle finden Sie unter [Verstehen und Verwenden von COM-Threading-Modellen](https://msdn.microsoft.com/library/ms809971).

## <a name="code-examples"></a>Codebeispiele
Lassen Sie uns anhand einer Beispielimplementierung veranschaulichen, wie C++/WinRT Agilität unterstützt.

```cppwinrt
#include <winrt/Windows.Foundation.h>

using namespace winrt;
using namespace Windows::Foundation;

struct MyType : implements<MyType, IStringable>
{
    winrt::hstring ToString(){ ... }
};
```

Da wir dies nicht explizit ausschließen, ist diese Implementierung agil. Die [**winrt::implementiert**](/uwp/cpp-ref-for-winrt/implements) Basisstruktur implementiert [**IAgileObject**](https://msdn.microsoft.com/library/windows/desktop/hh802476) und [**IMarshal**](https://docs.microsoft.com/previous-versions/windows/embedded/ms887993). Die **IMarshal**-Implementierung verwendet **CoCreateFreeThreadedMarshaler**, um das Legacy-Code zu unterstützen, der nichts über **IAgileObject** weiß.

Dieser Code prüft ein Objekt auf Agilität. Der Aufruf von [**IUnknown::as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function) löst eine Ausnahme aus, wenn `myimpl` nicht agil ist.

```cppwinrt
winrt::com_ptr<MyType> myimpl = winrt::make_self<MyType>();
winrt::com_ptr<IAgileObject> iagileobject = myimpl.as<IAgileObject>();
```

Anstatt eine Ausnahme zu verarbeiten, können Sie [**IUnknown::try_as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntryas-function) aufrufen.

```cppwinrt
winrt::com_ptr<IAgileObject> iagileobject = myimpl.try_as<IAgileObject>();
if (iagileobject) { /* myimpl is agile. */ }
```

**IAgileObject** hat keine eigenen Methoden, sodass man damit nicht viel anfangen kann. Diese nächste Variante ist eher typisch.

```cppwinrt
if (myimpl.try_as<IAgileObject>()) { /* myimpl is agile. */ }
```

**IAgileObject** ist eine *Marker-Schnittstelle*. Der Nutzen der Abfrage von **IAgileObject** besteht aus der Informationen zum Erfolg oder Misserfolg.

## <a name="opting-out-of-agile-object-support"></a>Vermeiden der Unterstützung von agilen Objekten
Sie können die Unterstützung für agile Objekte explizit deaktivieren, indem Sie die [**winrt::non_agile**](/uwp/cpp-ref-for-winrt/non_agile)-Markerstruktur als template-Argument an Ihre Basisklasse übergeben.

Wenn Sie direkt von **winrt::implements** ableiten.

```cppwinrt
struct MyImplementation: implements<MyImplementation, IStringable, non_agile>
{
    ...
}
```

Wenn Sie eine Laufzeitklasse schreiben.

```cppwinrt
struct MyRuntimeClass: MyRuntimeClassT<MyRuntimeClass, non_agile>
{
    ...
}
```

Dabei spielt es keine Rolle, wo im Variadic-Parameterpaket die Markerstruktur erscheint.

Unabhängig davon, ob Sie die Agilität verhindern, können Sie IMarshal selbst implementieren. Beispielsweise können Sie den Marker [**non_agile**] verwenden, um die Standard-Agilitäts-Implementierung zu vermeiden, und IMarshal selbst implementieren (um die Marshal-by-value-Semantik zu unterstützen).

## <a name="agile-references-winrtagileref"></a>Agile Referenzen (winrt::agile_ref)
Wenn Sie ein Objekt verwenden, das nicht agil ist, aber Sie es in einem potentiell agilen Kontext weitergeben müssen, dann ist eine Option die Verwendung der [**winrt::agile_ref**](/uwp/cpp-ref-for-winrt/agile-ref)-Strukturvorlage. So erhalten Sie eine agile Referenz auf eine Instanz eines nicht agilen Typs oder auf eine Schnittstelle eines nicht agilen Objekts.

```cppwinrt
NonAgileType nonagile_obj;
winrt::agile_ref<NonAgileType> agile{ nonagile_obj };
```
Oder Sie können die Hilfsfunktion [**winrt::make_agile**](/uwp/cpp-ref-for-winrt/make-agile) verwenden.

```cppwinrt
NonAgileType nonagile_obj;
auto agile = winrt::make_agile(nonagile_obj);
```

In beiden Fällen kann `agile` nun frei an einen Thread in einem anderen Apartment übergeben und dort verwendet werden.

```cppwinrt
co_await resume_background();
NonAgileType nonagile_obj_again = agile.get();
winrt::hstring message = nonagile_obj_again.Message();
```

Der Aufruf [**agile_ref::get**](/uwp/cpp-ref-for-winrt/agile-ref#agilerefget-function) gibt einen Proxy zurück, der in dem Thread-Kontext, in dem **get** aufgerufen wird, sicher verwendet werden kann.

## <a name="important-apis"></a>Wichtige APIs
* [IAgileObject Schnittstelle](https://msdn.microsoft.com/library/windows/desktop/hh802476)
* [IMarshal Schnittstelle](https://docs.microsoft.com/previous-versions/windows/embedded/ms887993)
* [winrt::agile_ref Strukturvorlage](/uwp/cpp-ref-for-winrt/agile-ref)
* [winrt::implements Strukturvorlage](/uwp/cpp-ref-for-winrt/implements)
* [winrt::make_agile Funktionsvorlage](/uwp/cpp-ref-for-winrt/make-agile)
* [winrt::non_agile Markerstruktur](/uwp/cpp-ref-for-winrt/non_agile)
* [winrt::Windows::Foundation::IUnknown::as Funktion](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function)
* [winrt::Windows::Foundation::IUnknown::try_as Funktion](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntryas-function)

## <a name="related-topics"></a>Verwandte Themen
* [Verstehen und Verwenden von COM-Threadingmodellen](https://msdn.microsoft.com/library/ms809971)