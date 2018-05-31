---
author: stevewhims
Description: With the package resource indexing (PRI) APIs, you can develop a custom build system for your UWP app's resources. The build system will be able to create, version, and dump PRI files to whatever level of complexity your UWP app needs.
title: APIs zur Paketressourcenindizierung (PRI) und benutzerdefinierte Buildsysteme
template: detail.hbs
ms.author: stwhi
ms.date: 02/20/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows10, UWP, Ressourcen, Bild, Element, MRT, Qualifizierer
ms.localizationpriority: medium
ms.openlocfilehash: 8947f9305829e5045a418ac2fdd2d6c327acb2a1
ms.sourcegitcommit: 346b5c9298a6e9e78acf05944bfe13624ea7062e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/05/2018
ms.locfileid: "1707351"
---
# <a name="package-resource-indexing-pri-apis-and-custom-build-systems"></a>APIs zur Paketressourcenindizierung (PRI) und benutzerdefinierte Buildsysteme
> [!NOTE]
> **Einige Informationen beziehen sich auf die Vorabversion, die vor der kommerziellen Freigabe möglicherweise wesentlichen Änderungen unterliegt. Microsoft übernimmt keine Garantie, weder ausdrücklich noch stillschweigend, für die hier bereitgestellten Informationen.**

Mit den [APIs zur Paketressourcenindizierung (PRI)](https://msdn.microsoft.com/library/windows/desktop/mt845690) können Sie ein benutzerdefiniertes Buildsystem für die Ressourcen Ihrer UWP-App entwickeln. Das Buildsystem kann Paketressourcenindexdateien (PRI) erstellen, versionieren und sichern (als XML), und zwar für jedes Maß an Komplexität, das Ihre UWP-App benötigt. Wenn Sie ein benutzerdefiniertes Buildsystem besitzen, das derzeit das Befehlszeilentool MakePri.exe verwendet (siehe [Manuelles Kompilieren von Ressourcen mit MakePri.exe](makepri-exe-command-options.md)), empfiehlt es sich zur Erzielung einer höheren Leistung und für mehr Steuerungsmöglichkeiten, die PRI-APIs anstelle von MakePri.exe aufzurufen.

Die PRI-APIs wurden mit dem Windows SDK für Windows10, Version 1803, eingeführt. Die APIs weisen die Form von Win32-Windows-APIs auf, was bedeutet, dass Ihnen mehrere Optionen für deren Aufruf zur Verfügung stehen. Sie können sie direkt von einer Win32-App oder über [Plattformaufrufe](/dotnet/framework/interop/consuming-unmanaged-dll-functions?branch=live) von einer .NET-App oder auch von einer UWP-App aus aufrufen.

Die Szenarien in diesem Thema zeigen den Aufruf von PRI-APIs von einem Anwendungsprojekt der Win32-Visual C++-Windows-Konsole. Hintergrundinformationen finden Sie unter [Ressourcenverwaltungssystem](resource-management-system.md).

Die maximale Größe für eine PRI-Datei beträgt 64KB.

> [!NOTE]
> Diese Einschränkung ist wahrscheinlich kein Problem, da Sie Ihre benutzerdefinierte Buildsystem-App vermutlich nicht an den Microsoft Store übermitteln möchten. Wenn Sie aber die Option zum Entwickeln des benutzerdefinierten Buildsystems in Form einer UWP-App wählen, könnten Sie diese UWP-App nicht an den Microsoft Store übermitteln. Dies liegt daran, dass für eine UWP-App, die Plattformaufrufe verwendet, die Microsoft Store-Zertifizierung fehlschlägt. Beachten Sie, dass in diesem Fall Plattformaufrufe *nur in Ihrem benutzerdefinierten Buildsystem vorhanden sind*, *nicht* aber in Ihrer versandten UWP-App (diejenige, für die Sie PRI-Dateien erstellen).

## <a name="scenario-walkthroughs"></a>Exemplarische Vorgehensweisen für die Szenarien
|Thema|Beschreibung|
|-|-|
|[Szenario 1: Generieren einer PRI-Datei aus Zeichenfolgenressourcen und Ressourcendateien](pri-apis-scenario-1.md)|In diesem Szenario erstellen wir eine neue App zur Darstellung unseres benutzerdefinierten Buildsystems. Wir erstellen einen Ressourceindexer und fügen diesem Zeichenfolgen und andere Arten von Ressourcen hinzu. Dann generieren und sichern wir eine PRI-Datei.|

## <a name="important-apis"></a>Wichtige APIs
* [Referenz für die Paketressourcenindizierung (PRI)](https://msdn.microsoft.com/library/windows/desktop/mt845690)

## <a name="related-topics"></a>Verwandte Themen
* [Manuelles Kompilieren von Ressourcen mit MakePri.exe](makepri-exe-command-options.md)
* [Verwenden nicht verwalteter DLL-Funktionen](/dotnet/framework/interop/consuming-unmanaged-dll-functions?branch=live)
* [Ressourcenverwaltungssystem](resource-management-system.md)