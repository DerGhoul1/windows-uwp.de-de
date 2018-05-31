---
author: stevewhims
description: Das Windows SDK enthält C++/WinRT. Dies ist eine Standard C++ Sprachprojektion für Windows-Runtime-APIs, die ausschließlich in Header-Dateien implementiert ist und Ihnen einen erstklassigen Zugriff auf die moderne Windows API bietet. Mit C++/WinRT können Sie Windows-Runtime-APIs mit jedem standardkonformen C++ Compiler erstellen und verwenden.
title: C++/WinRT
ms.author: stwhi
ms.date: 04/10/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, standard, c++, cpp, winrt, projizierung
ms.localizationpriority: medium
ms.openlocfilehash: e364283c998179264fc3aa4cb8581d511608f251
ms.sourcegitcommit: ab92c3e0dd294a36e7f65cf82522ec621699db87
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/03/2018
ms.locfileid: "1831814"
---
# [<a name="cwinrt"></a>C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)
<BR/>
> [!VIDEO https://www.youtube.com/embed/TLSul1XxppA]

Das in Version 10.0.17134.0 (Windows 10, Version 1803) eingeführte Windows SDK enthält nun C++/WinRT. C++/WinRT ist eine vollständig standardisierte, moderne C++17 Sprachprojektion für Windows-Runtime-(WinRT)-APIs, die ausschließlich in Header-Dateien implementiert ist und Ihnen einen erstklassigen Zugriff auf die moderne Windows-API bietet. Mit C++/WinRT können Sie Windows-Runtime-APIs mit jedem standardkonformen C++17-Compiler erstellen und verwenden.

C++/WinRT ist für jeden Entwickler gedacht, der daran interessiert ist, schönen und schnellen Code für Windows zu schreiben. Dies sind die Gründe.

## <a name="the-case-for-cwinrt"></a>Der Zweck von C++/WinRT
Die Programmiersprache C++ wird sowohl im Enterprise- als *auch* im Independent Software Vendor (ISV)-Segment für Anwendungen eingesetzt, bei denen ein hohes Maß an Korrektheit, Qualität und Leistung geschätzt wird. Diese sind zum Beispiel: Systemprogrammierung; ressourcenbeschränkte integrierte und mobile Systeme; Spiele und Grafiken; Gerätetreiber, und industrielle, wissenschaftliche und medizinische Anwendungen.

Aus Sicht der Sprache ging es in C++ immer darum, Abstraktionen zu erstellen und zu nutzen, die sowohl typenreich als auch leichtgewichtig sind. Aber die Sprache hat sich seit den grundlegenden Zeigern und Schleifen und der mühsamen Speicherzuweisung und -freigabe von C++ aus dem Jahr 1998 radikal verändert. Modernes C++ (ab C++11) ist ideenreicher, einfacher, lesbarer und viel weniger fehleranfällig.

Für die Erstellung und Nutzung von Windows-Runtime-APIs mit C++ gibt es C++/WinRT. C++/WinRT ist der von Microsoft empfohlene Ersatz für die [C++-Vorlagenbibliothek für Windows-Runtime (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl?branch=live) und [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx?branch=live).

Mit C++/WinRT verwenden Sie Standard-C++ Datentypen, -Algorithmen und -Schlüsselwörter. Die Projektion hat ihre eigenen benutzerdefinierten Datentypen, aber in den meisten Fällen müssen Sie sie nicht kennen, da es entsprechende Konvertierungen zu und von Standardtypen gibt. Auf diese Weise können Sie weiterhin die gewohnten Standardfunktionen von C++ und den bereits vorhandenen Quellcode verwenden. C++/WinRT macht es extrem einfach, Windows-Runtime-APIs in jeder C++ Anwendung, von Win32 bis UWP, aufzurufen.

C++/WinRT arbeitet besser und erzeugt kleinere Binärdateien als jede andere Sprachoption für die Windows-Runtime. Es übertrifft sogar handgeschriebenen Code, der die ABI-Schnittstellen direkt nutzt. Das liegt daran, dass die Abstraktionen moderne C++ Idiome verwenden, die der Visual C++ Compiler optimiert. Dazu gehören magische Statics, leere Basisklassen, **strlen**-Elision, sowie viele neuere Optimierungen in der neuesten Version von Visual C++, die speziell auf die Verbesserung der Performance von C++/WinRT abzielen.

| Thema | Beschreibung |
| - | - |
| [Einführung in C++/WinRT](intro-to-using-cpp-with-winrt.md) | Eine Einführung in C++/WinRT – einer Standard C++ Sprachprojektion für Windows-Runtime-APIs. |
| [Häufig gestellte Fragen](faq.md) | Antworten auf Fragen zur Erstellung und Nutzung von Windows-Runtime-APIs mit C++/WinRT. |
| [Problembehandlung](troubleshooting.md) | Die Tabelle mit den Symptomen und Problembehandlungen in diesem Thema kann für Sie hilfreich sein, egal ob Sie neuen Code schreiben oder eine bestehende App portieren. |
| [String-Verarbeitung](strings.md) | Mit C++/WinRT können Sie Windows-Runtime-APIs mit Standard-C++ String-Typen aufrufen, oder Sie können den Typ [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) verwenden. |
| [Standard C++ Datentypen und C++/WinRT](std-cpp-data-types.md) | Mit C++/WinRT können Sie Windows-Runtime-APIs über Standard-C++ Datentypen aufrufen. |
| [Boxing und Unboxing von Einzelwerten für IInspectable](boxing.md) | Ein Einzelwert muss in ein Referenzklassenobjekt gepackt werden, bevor er an eine Funktion übergeben wird, die **IInspectable** erwartet. Dieser Wrapping-Prozess wird als *Boxing* des Wertes bezeichnet. |
| [Verwenden von APIs mit C++/WinRT](consume-apis.md) | Dieses Thema zeigt, wie man selbst, von Windows oder von Drittanbietern implementierte C++/WinRT-APIs verwendet. |
| [Erstellen von APIs mit C++/WinRT](author-apis.md) | Dieses Thema zeigt, wie man C++/WinRT-APIs mit Hilfe der **winrt::implements**-Basisstruktur direkt oder indirekt erstellt. |
| [Verarbeiten von Ereignissen über Delegaten](handle-events.md) | Dieses Thema zeigt, wie man Event-Handling-Delegaten mit C++/WinRT registriert und widerruft. |
| [Erstellen von Ereignissen](author-events.md) | Dieses Thema zeigt, wie man eine Komponente für Windows-Runtime erstellt, die eine Laufzeitklasse enthält, die Ereignisse auslöst. Es zeigt außerdem eine App, die die Komponente nutzt und die Ereignisse verarbeitet. |
| [Parallelität und asynchrone Vorgänge](concurrency.md) | Dieses Thema zeigt, wie Sie asynchrone Windows-Runtime-Objekte mit C++/WinRT erstellen und nutzen können. |
| [XAML-Steuerelemente; Binden an eine C++/WinRT-Eigenschaft](binding-property.md) | Eine Eigenschaft, die effektiv an ein XAML-Steuerelement gebunden werden kann, wird als *Observable*-Eigenschaft bezeichnet. Dieses Thema zeigt, wie man eine Observable-Eigenschaft implementiert und nutzt und wie man ein XAML-Steuerelement daran bindet. |
| [XAML-Items-Steuerelemente; Binden an eine C++/WinRT-Collection](binding-collection.md) | Eine Collection, die effektiv an ein XAML-Items-Steuerelement gebunden werden kann, wird als *Observable*-Collection bezeichnet. Dieses Thema zeigt, wie man eine Observable-Collection implementiert und nutzt und wie man ein XAML-Items-Steuerelement daran bindet. |
| [Interoperabilität zwischen C++/WinRT und C++/CX](interop-winrt-cx.md) | In diesem Thema werden zwei Hilfsfunktionen gezeigt, die verwendet werden können, um zwischen [C++ / CX](/cpp/cppcx/visual-c-language-reference-c-cx?branch=live)- und C++ / WinRT-Objekten zu konvertieren. |
| [Interoperabilität zwischen C++/WinRT und der ABI](interop-winrt-abi.md) | Dieses Thema zeigt, wie man zwischen Application Binary Interface (ABI) und C++/WinRT-Objekten konvertiert. |
| [Schwache Referenzen](weak-references.md) | Die Unterstützung von schwachen Referenzen in C++/WinRT kostet Sie nichts. Es sei denn Ihr Objekt wird auf [**IWeakReferenceSource**](https://msdn.microsoft.com/library/br224609) abgefragt. |
| [Agile Objekte](agile-objects.md) | Ein agiles Objekt ist ein Objekt, auf das von jedem Thread aus zugegriffen werden kann. Ihre C++/WinRT-Typen sind standardmäßig agil, aber Sie können diese Option deaktivieren. |

## <a name="important-apis"></a>Wichtige APIs
* [winrt Namespace](/uwp/cpp-ref-for-winrt/winrt)

## <a name="related-topics"></a>Verwandte Themen
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [C++-Vorlagenbibliothek für Windows-Runtime (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl)