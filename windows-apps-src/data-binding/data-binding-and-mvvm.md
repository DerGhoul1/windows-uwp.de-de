---
author: KarlErickson
ms.assetid: F46306EC-DFF3-4FF0-91A8-826C1F8C4A52
title: Datenbindungen und MVVM
description: Die Datenbindung ist das Herzstück des Musters Architektur Model-View-ViewModel (MVVM)-UI und ermöglicht die Kopplung zwischen UI und nicht-UI-Code.
ms.author: karler
ms.date: 10/02/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: eda370db8b68232066052cca3d0abfa6e3876167
ms.sourcegitcommit: e16c9845b52d5bd43fc02bbe92296a9682d96926
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/19/2018
ms.locfileid: "4949236"
---
# <a name="data-binding-and-mvvm"></a>Datenbindungen und MVVM

Model-View-ViewModel (MVVM) ist ein UI-Architektur-Muster für das Entkoppeln von Benutzeroberfläche und nicht-UI-Code. Das MVVM deklarativ definieren die Benutzeroberfläche in XAML und bindungsmarkup Daten für die Verbindung zu anderen Ebenen, die Daten und Befehle verwenden. Die Datenbindung Infrastruktur bietet eine Kopplung, die die Benutzeroberfläche hält und die verknüpften Daten synchronisiert und leitet Benutzereingaben an die entsprechenden Befehle weiter. 

Da es Kopplung bereitstellt, wird die Verwendung der Datenbindung schwer Abhängigkeiten zwischen verschiedenen Arten von Code verringert. Dies erleichtert die einzelnen Code Einheiten (Methoden, Klassen, Steuerelemente usw.) zu ändern, ohne dass unerwünschte Nebenwirkungen in anderen Einheiten. Dieser Entkopplung ist ein Beispiel für die *Trennung von Bereichen*ein wichtiges Konzept in vielen Entwurfsmuster. 

## <a name="benefits-of-mvvm"></a>Vorteile von MVVM

Entkoppeln Ihren Code hat viele Vorteile, darunter:

* Aktivieren eine iterative, explorativen Codierungsstil. Änderung, die isoliert ist weniger fehleranfällig und leichter zu experimentieren.
* Vereinfachung Komponententests. Code-Einheiten, die voneinander isoliert sind können einzeln und außerhalb produktionsumgebungen getestet werden.
* Unterstützung der Teamzusammenarbeit. Getrennt Code, die gut durchdachte Schnittstellen entspricht, kann durch separate Personen oder Teams entwickelt und später integriert werden.
* Verbesserung der Wartungsfreundlichkeit. Beheben von Fehlern im getrennt Code wahrscheinlich weniger Funktionalität in einem anderen Code verursachen.

Im Gegensatz zu MVVM eine app mit einer konventionelle "CodeBehind"-Struktur in der Regel verwendet für die Anzeige nur Daten binden von Daten und auf Benutzereingaben reagiert, indem direkt behandeln von Ereignissen von Steuerelementen verfügbar gemacht werden. Die Ereignishandler im Code-Behind-Dateien (z. B. "MainPage.Xaml.cs") implementiert sind, und werden häufig eng an die Steuerelemente in der Regel mit Code, der die Benutzeroberfläche direkt bearbeitet. Dadurch wird es schwierig oder sogar unmöglich, um ein Steuerelement zu ersetzen, ohne den Ereignisbehandlungscode aktualisieren. Mit dieser Architektur wiederholenden CodeBehind-Dateien häufig Code, der sich direkt auf die Benutzeroberfläche, z. B. Datenbankzugriffs Code mit ist nicht der Clientmethode dupliziert und für die Verwendung mit anderen Seiten geändert wird.

## <a name="app-layers"></a>App-Schichten

Wenn Sie das MVVM-Muster zu verwenden, ist eine app in den folgenden Ebenen unterteilt:

* Die **Modell** -Ebene definiert die Typen, die Ihre Geschäftsdaten darstellen. Dies enthält alles, was erforderlich, um die Core app Domäne modellieren und umfasst häufig zentralen app-Logik. Diese Ebene ist vollständig unabhängig von der Ansicht und der Ansichtsmodell Ebene und befindet sich häufig teilweise in der Cloud. Eine vollständig implementierten Modellebene können Sie mehrere verschiedene Client apps erstellen, wenn Sie also, z. B. UWP- und Web-apps auswählen, die mit den gleichen Daten funktionieren.
* Die **Ansicht** -Ebene definiert die Benutzeroberfläche mit XAML-Markup. Das Markup enthält Datenbindungsausdrücke (z. B. [X: Bind](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension)), die die Verbindung zwischen bestimmten UI-Komponenten und verschiedenen Ansichtsmodell und Modell Member zu definieren. Code-Behind-Dateien werden manchmal als Teil der Ansicht Ebene verwendet, enthalten zusätzlichen Code erforderlich, die Benutzeroberfläche zu anpassen oder Extrahieren von Daten aus Ereignisargumente Handler vor dem Aufrufen einer Ansichtsmodell-Methode, die die Arbeit ausführt. 
* Die **Ansichtsmodell** Ebene bereitstellt Bindungsziele Daten für die Ansicht. In vielen Fällen das Ansichtsmodell verfügbar macht das Modell direkt oder enthält Member für die spezifische Modell Mitglieder umschließen. Das Ansichtsmodell kann auch die Mitglieder definieren, Verfolgen von Daten, die relevant für die Benutzeroberfläche aber nicht für das Modell, z. B. die Anzeigereihenfolge der eine Liste von Elementen. Das Ansichtsmodell dient auch als Point Integration mit anderen Diensten wie z. B. Datenbankzugriffs Code. Bei einfachen Projekten benötigen Sie möglicherweise keine separate Modellebene, aber nur ein Ansichtsmodell, das alle Daten kapselt, die Sie benötigen. 

## <a name="basic-and-advanced-mvvm"></a>Grundlegenden und erweiterten MVVM

Wie bei allen Entwurfsmuster, es ist mehr als eine Möglichkeit zum Implementieren von MVVM und viele verschiedene Techniken, die als Teil des MVVM. Aus diesem Grund stehen mehrere verschiedene Drittanbieter-MVVM-Frameworks unterstützen die verschiedenen XAML-basierte Plattformen, einschließlich UWP. Diese Frameworks umfassen jedoch in der Regel mehrere Dienste für die Implementierung von getrennt Architektur, die genaue Definition von MVVM nicht eindeutig machen. 

Obwohl anspruchsvolle MVVM-Frameworks sehr nützlich sein können, insbesondere bei einer Skalierung von Enterprise-Projekte wird in der Regel eine Aufwands für alle bestimmtes Muster oder Technik Übernahme und die Vorteile sind nicht immer klar, je nach der Skalierung und Größe des Ihr Projekt. Glücklicherweise können Sie nur diese Techniken, die einen klare und konkreten Vorteil bereitstellen übernehmen und andere ignorieren, bis Sie sie benötigen. 

Genauer gesagt erhalten Sie einen Großteil Vorteil einfach durch Verständnis und die leistungsstarken Daten binden und die Aufteilung Ihrer app-Logik in der oben beschriebenen Ebenen anwenden. Dies kann mit nur die Funktionen von Windows SDK, und ohne alle externe Frameworks erzielt werden. Insbesondere [{X: Bind}-Markuperweiterung](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension) erleichtert die Datenbindung und leistungsfähigere als in vorherigen XAML-Plattformen-Steuerelementen für einen Großteil der Standardcode früheren Versionen erforderlich.

Weitere Informationen zur Verwendung von einfachen, Out-of-the-Box MVVM sehen Sie sich das [Kundenauftragsdatenbank-Beispiel](https://github.com/Microsoft/Windows-appsample-customers-orders-database) auf GitHub. Viele der anderen [UWP: Beispiele](https://github.com/Microsoft?q=windows-appsample
) verwenden auch eine grundlegende MVVM-Architektur und der [Datenverkehr App-Beispiel](https://github.com/Microsoft/Windows-appsample-trafficapp) enthält Code-Behind und MVVM-Versionen mit Notizen, beschreiben die [MVVM-Konvertierung](https://github.com/Microsoft/Windows-appsample-trafficapp/blob/MVVM/MVVM.md). 

## <a name="see-also"></a>Weitere Informationen:

### <a name="topics"></a>Themen

[Datenbindung im Detail](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-in-depth)  
[{x:Bind}-Markuperweiterung](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension)  

### <a name="samples"></a>Beispiele

[Beispiel für Kunden-Datenbank für Kundenaufträge](https://github.com/Microsoft/Windows-appsample-customers-orders-database)  
[VanArsdel Inventar-Beispiel](https://github.com/Microsoft/InventorySample)  
[Beispiel für App mit Verkehrsinformationen](https://github.com/Microsoft/Windows-appsample-trafficapp)  