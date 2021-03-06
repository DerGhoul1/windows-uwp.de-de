---
title: Unity – Versionskontrolle für Ihr UWP-Projekt
description: Versionsverwaltung für Ihr Unity-UWP.
ms.localizationpriority: medium
ms.topic: article
ms.date: 02/08/2017
ms.openlocfilehash: b98fba394fb326d60451f07938504e99a92d764d
ms.sourcegitcommit: 3e7a4f7605dfb4e87bac2d10b6d64f8b35229546
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/08/2020
ms.locfileid: "77089486"
---
# <a name="unity-version-control-your-uwp-project"></a>Unity: Versionskontrolle für Ihr UWP-Projekt

Sie haben Ihr Unity-Spiel für Xbox noch immer nicht in die universelle Windows-Plattform (UWP) portiert?  Lesen Sie zunächst [Portieren von Unity-Spielen für Xbox auf die UWP](development-lanes-unity.md).

Es gibt verschiedene Gründe dafür, Teile Ihres generierten UWP-Verzeichnisses der Versionskontrolle hinzuzufügen. Hierzu zählt unter anderem das Hinzufügen von Abhängigkeiten (z. B. Xbox Live SDK).  Dieses Szenario wird in diesem Lernprogramm als Beispiel herangezogen und hilft Ihnen hoffentlich dabei, die individuellen Anforderungen Ihres Projekts zu erfüllen.

***Haftungsausschluss: Wir verwenden git als Lösung für die Versionskontrolle.  Wenn Sie sich unterscheiden, sollten die Konzepte weiterhin übersetzt werden.***

Zur Erinnerung: Das Verzeichnis für unser Spiel ***ScrapyardPhoenix*** sieht aktuell wie folgt aus:

![Build-Zielordner](images/build-destination.png)

Und unser UWP-Verzeichnis sieht wie folgt aus:

![UWP-VS-Lösung](images/uwp-vs-solution.png)

In diesem Verzeichnis interessiert uns nur der Ordner ***ScrapyardPhoenix*** (Name Ihres Spiels).  Alles andere ist für unsere Versionskontrolle nicht relevant.

***Sie sind nicht vertraut, was eine gitignore-Datei ist?  Weitere Informationen finden Sie unter [gitignore](https://git-scm.com/docs/gitignore).***

    ##################################################################
    # The original .gitignore file can be found at
    # https://github.com/github/gitignore/blob/master/Unity.gitignore
    ##################################################################

    # standard ignores for a Unity Project
    ...

    # ignore the whole UWP directory
    /UWP/**

    # except we want to keep... (this line will be modified and removed further down)
    !/UWP/ScrapyardPhoenix/

Wir möchten einige unterschiedliche Dateien und Ordner aus dem Ordner **UWP/ScrapyardPhoenix** auswählen und unserer Versionskontrolle hinzufügen.  Sehen wir uns das Ganze erst einmal im Detail an:

![UWP-Buildverzeichnis](images/uwp-build-directory.png)  

## <a name="folders"></a>Ordner  

`Assets` | ***einschließen*** | Enthält Microsoft Store Bilder  
`Data`   | ***ignorieren*** | Wenn Unity das Projekt in kompiliert (Szenen, Shader, Skripts, Prefabs usw.)  
`Dependencies` | ***einschließen*** | Dieser Ordner wurde erstellt, um alle UWP-Abhängigkeiten beizubehalten (z. b. "xboxlivesdk. dll").  
`Properties` | ***einschließen*** | Enthält erweiterte Einstellungen, die vom Entwickler geändert werden können.  
`Unprocessed` | ***ignorieren*** | Enthält Unity-`.dll` und `.pdb` Dateien  

## <a name="files"></a>Dateien  

`App.cs` | ***einschließen*** | Einstiegspunkt für Ihre UWP-Anwendung Diese kann geändert und mit anderen Quelldateien erweitert werden.  
`Package.appxmanifest` | ***einschließen*** | App-Paket Manifest-Quelldatei für das msix-oder AppX-Paket  
`project.json` | ***einschließen*** | Beschreibt die nuget-Pakete, von denen Ihre `*.csproj` abhängig ist.  
`ScrapyardPhoenix.csproj` | ***einschließen*** | Beschreibt das UWP-Buildziel. Wenn Sie dem UWP-Projekt weitere Abhängigkeiten hinzufügen, enthält diese `*.csproj` Datei diese Informationen.  
`ScrapyardPhoenix.csproj.user` | ***ignorieren*** | Diese Datei enthält lokale Benutzerinformationen.

## <a name="resulting-gitignore"></a>Resultierende GITIGNORE-Datei

    ##################################################################
    # The original .gitignore file can be found at
    # https://github.com/github/gitignore/blob/master/Unity.gitignore
    ##################################################################

    # standard ignores for a Unity Project
    ...

    # ignore the whole UWP directory
    /UWP/**

    # except we want to keep...
    !/UWP/ScrapyardPhoenix/Assets/*
    !/UWP/ScrapyardPhoenix/Dependencies/*
    !/UWP/ScrapyardPhoenix/Properties/*
    !/UWP/ScrapyardPhoenix/App.cs
    !/UWP/ScrapyardPhoenix/Package.appxmanifest
    !/UWP/ScrapyardPhoenix/project.json
    !/UWP/ScrapyardPhoenix/ScrapyardPhoenix.csproj

Geschafft: Ihre Teammitglieder sind nun mit dem von Ihnen generierten UWP-Projekt synchronisiert. Nun können Sie Ihrem UWP-Projekt problemlos zusätzliche Ressourcen, Quellen und Abhängigkeiten hinzufügen.

Einige weitere Versionsverwaltungsbeispiele für den UWP-Ordner finden Sie [hier](https://bitbucket.org/Unity-Technologies/windowsstoreappssamples/overview).

## <a name="adding-dependencies-to-your-uwp-app"></a>Hinzufügen von Abhängigkeiten zu Ihrer UWP-App

Fügen Sie Abhängigkeiten zu DLL- und WINMD-Dateien hinzu, indem Sie diese im Unterordner **Unity Assets** des Ordners **Plug-Ins** ablegen und auswählen. Legen Sie anschließend im Paketprüfungstool die Plattform-Zieleinstellungen fest.

![UWP-Lösung](images/uwp-solution.PNG)

***ScrapyardPhoenix (Universal Windows)*** ist das Projekt, dem Sie einen Verweis (etwa auf das Xbox Live SDK) hinzufügen würden.

## <a name="see-also"></a>Siehe auch
- [Portieren vorhandener Spiele zu Xbox](development-lanes-landing.md)
- [UWP auf Xbox One](index.md)
