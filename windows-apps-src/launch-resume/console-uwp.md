---
author: TylerMSFT
title: Erstellen einer universellen Windows-Plattform-Konsolen-App
description: In diesem Thema erfahren Sie, wie Sie eine UWP-App erstellen, die in einem Konsolenfenster ausgeführt wird.
keywords: console uwp
ms.author: twhitney
ms.date: 03/30/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 413f4db427557460c267370f477e16d84c8af29c
ms.sourcegitcommit: 91511d2d1dc8ab74b566aaeab3ef2139e7ed4945
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/30/2018
ms.locfileid: "1815535"
---
# <a name="create-a-universal-windows-platform-console-app"></a>Erstellen einer universellen Windows-Plattform-Konsolen-App

In diesem Thema erfahren Sie, wie Sie eine C++ /WinRT oder /CX Universelle Windows-Plattform (UWP)-Konsolen-App erstellen.

Sie können jetzt unter Windows 10, Version 1803, C++/WinRT oder /CX UWP Konsolen-Apps erstellen, die in einem Konsolenfenster wie z.B. einem DOS oder PowerShell-Konsolenfenster ausgeführt werden. Konsolen-Apps verwenden das Konsolenfenster für die Eingabe und Ausgabe und können Win32-APIs, die für UWP-Apps wie z.B. **Printf** oder **Getchar** verfügbar sind, nutzen. UWP-Konsolen-Apps können im Microsoft Store veröffentlicht werden. Sie haben einen Eintrag in der App-Liste und eine primäre Kachel, die an das Startmenü angeheftet werden kann. UWP-Konsolen-Apps werden normalerweise über die Befehlszeile gestartet werden, können aber auch aus dem Startmenü gestartet werden.

Hier können Sie sich ein Video über das Erstellen einer UWP-Konsolen-App ansehen:
> [!VIDEO https://www.youtube.com/embed/bwvfrguY20s]

## <a name="use-a-uwp-console-app-template"></a>Verwenden einer UWP-Konsolen-App-Vorlage 

Um eine UWP-Konsolen-App zu erstellen, installieren Sie zuerst die **Konsolen-App (Universal)-Projektvorlagen**, die Sie im [Visual Studio Marketplace](https://aka.ms/E2nzbv) erhalten. Anschließend stehen die installierten Vorlagen unter **Neues Projekt** > **Installiert** > **Andere Sprachen** > **Visual C++** > **Windows Universal** als **Console App C++/CX (Universal Windows)** und **Console App C++/WinRT (Universal Windows)** zur Verfügung.

## <a name="add-your-code-to-main"></a>Ihren Code „main()” hinzufügen

Die Vorlagen fügen **Program.cpp** hinzu, die die `main()`-Funktion enthält. Hier beginnt die Ausführung in einer UWP-App-Konsole. Greifen Sie mit den Parametern `__argc` und `__argv` auf die Befehlszeilenargumente zu. Die UWP-Konsolen-App wird beendet, wenn die Steuerung von `main()` zurückgegeben wird.

Das folgende Beispiel von **Program.cpp** wird von der **Console App C++ /WinRT**-Vorlage hinzugefügt:

```cpp
#include "pch.h"

using namespace winrt;

// This example code shows how you could implement the required main function
// for a Console UWP Application. You can replace all the code inside main
// with your own custom code.

int __cdecl main()
{
    // You can get parsed command-line arguments from the CRT globals.
    wprintf(L"Parsed command-line arguments:\n");
    for (int i = 0; i < __argc; i++)
    {
        wprintf(L"__argv[%d] = %S\n", i, __argv[i]);
    }

    // Keep the console window alive in case you want to see console output when running from within Visual Studio
      wprintf(L"Press 'Enter' to continue: ");
    getchar();
}
```

## <a name="uwp-console-app-behavior"></a>Verhalten der UWP-Konsolen-App

Eine UWP-Konsolen-App kann sowohl aus dem Verzeichnis auf das Dateisystem zugreifen, aus dem sie ausgeführt wird, sowie aus Unterverzeichnissen dessen. Dies ist möglich, da die Vorlage der Datei „Package.appxmanifest” der App die [AppExecutionAlias](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap5-appexecutionalias)-Erweiterung hinzufügt. Diese Erweiterung ermöglicht auch, dass der Benutzer den Alias in einem Konsolenfenster eingeben kann, um die App zu starten. Die App muss sich für den Start nicht im Systempfad befinden.

Darüber hinaus können Sie Ihrer UWP-Konsolen-App umfassenden Zugriff auf das Dateisystem erteilen, indem Sie die eingeschränkte Funktion `broadFileSystemAccess`, wie in [Berechtigungen für den Dateizugriff](https://docs.microsoft.com/windows/uwp/files/file-access-permissions) beschrieben, hinzufügen. Diese Funktion funktioniert mit APIs im [**Windows.Storage**](https://msdn.microsoft.com/library/windows/apps/BR227346)-Namespace.

Zu jedem beliebigen Zeitpunkt kann mehr als eine Instanz einer UWP-Konsolen-App ausgeführt werden, da die Vorlage der Datei „Package.appxmanifest” Ihrer App die [SupportsMultipleInstances](multi-instance-uwp.md)-Funktion hinzufügt.

Außerdem fügt die Vorlage der Datei „Package.appxmanifest” die `Subsystem="console"`-Funktion hinzu, die angibt, dass es sich bei dieser UWP-App um eine Konsolen-App handelt. Beachten Sie die Präfixe `desktop4` und iot2 `namespace`. UWP-Konsolen-Apps werden nur für Desktop- und Internet der Dinge (IoT)-Projekte unterstützt.

```xml
<Package
  ...
  xmlns:desktop4="http://schemas.microsoft.com/appx/manifest/desktop/windows10/4" 
  xmlns:iot2="http://schemas.microsoft.com/appx/manifest/iot/windows10/2" 
  IgnorableNamespaces="uap mp uap5 desktop4 iot2">
  ...
  <Applications>
    <Application Id="App"
      ...
      desktop4:Subsystem="console" 
      desktop4:SupportsMultipleInstances="true" 
      iot2:Subsystem="console" 
      iot2:SupportsMultipleInstances="true"  >
      ...
      <Extensions>
          <uap5:Extension 
            Category="windows.appExecutionAlias" 
            Executable="YourApp.exe" 
            EntryPoint="YourApp.App">
            <uap5:AppExecutionAlias desktop4:Subsystem="console">
              <uap5:ExecutionAlias Alias="YourApp.exe" />
            </uap5:AppExecutionAlias>
          </uap5:Extension>
      </Extensions>
    </Application>
  </Applications>
    ...
</Package>
```

## <a name="additional-considerations-for-uwp-console-apps"></a>Weitere Überlegungen zu UWP-Konsolen-Apps

- Nur C++ /WinRT und C++-/CX UWP-Apps können Konsolen-Apps sein.
- UWP-Konsolen-Apps müssen auf den Projekttyp Desktop oder IoT ausgerichtet sein.
- UWP-Konsolen-Apps dürfen keine Fenster erstellen. So dürfen sie „MessageBox()” nicht verwenden, weil damit ein Fenster erstellt wird.
- UWP-Konsolen-Apps darf nicht Hintergrundaufgaben nutzen oder als Hintergrundaufgabe dienen.
- Mit Ausnahme der [Befehlszeilenaktivierung](https://blogs.windows.com/buildingapps/2017/07/05/command-line-activation-universal-windows-apps/#5YJUzjBoXCL4MhAe.97) unterstützen UWP-Konsolen-Apps keine Support-Aktivierungsverträge, einschließlich Dateizuordnung, Protokollzuordnung usw.
- Obwohl UWP-Konsolen-Apps die Multiinstanzerstellung unterstützen, bieten Sie keine Unterstützung für die [Umleitung bei der Multiinstanzerstellung](multi-instance-uwp.md)
- Eine Liste von Win32-APIs, die für UWP-Apps verfügbar sind, finden Sie unter [Win32- und COM-APIs für UWP-Apps](https://docs.microsoft.com/uwp/win32-and-com/win32-and-com-for-uwp-apps)