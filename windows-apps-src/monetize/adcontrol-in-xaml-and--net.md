---
ms.assetid: 4e7c2388-b94e-4828-a104-14fa33f6eb2d
description: Hier erfahren Sie, wie Sie die AdControl-Klasse nutzen können, um Werbebanner in einer XAML-App für Windows 10 (UWP) anzuzeigen.
title: „AdControl“ in XAML und .NET
ms.date: 02/18/2020
ms.topic: article
keywords: Windows 10, UWP, Anzeigen, Werbung, AdControl, Anzeigensteuerelement, XAML, .NET, exemplarische Vorgehensweise
ms.localizationpriority: medium
ms.openlocfilehash: 7472343dcff4ac7f80d3bcea917e38849f513be6
ms.sourcegitcommit: 71f9013c41fc1038a9d6c770cea4c5e481c23fbc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/20/2020
ms.locfileid: "77507204"
---
# <a name="adcontrol-in-xaml-and-net"></a>„AdControl“ in XAML und .NET

>[!WARNING]
> Ab dem 1. Juni 2020 wird die Microsoft AD-Monetarisierungsplattform für Windows UWP-apps heruntergefahren. [Weitere Informationen](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

In dieser exemplarischen Vorgehensweise wird veranschaulicht, wie Sie die [AdControl](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol)-Klasse nutzen können, um Werbebanner in einer Universellen Windows-Plattform-, XAML-App für Windows 10 anzuzeigen, die in C++ implementiert wurden.

> [!NOTE]
> Das Microsoft Advertising-SDK unterstützt auch XAML-Apps, die mit C++ implementiert werden. Ein vollständiges Beispielprojekt finden Sie unter den [Anzeigenbeispielen auf GitHub](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising).

## <a name="prerequisites"></a>Erforderliche Komponenten

* Installieren Sie das [Microsoft Advertising-SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK) mit Visual Studio 2015 oder einer neueren Version von Visual Studio. Installationsanweisungen finden Sie in [diesem Artikel](install-the-microsoft-advertising-libraries.md).

## <a name="integrate-a-banner-ad-into-your-app"></a>Banneranzeige in Ihrer App integrieren

1. Öffnen Sie in Visual Studio Ihr Projekt, oder erstellen Sie ein neues Projekt.

    > [!NOTE]
    > Wenn Sie ein vorhandenes Projekt verwenden, öffnen Sie die Datei "Package.appxmanifest" in Ihrem Projekt, und stellen sicher, dass die **Internet (Client)** -Funktion aktiviert ist. Ihre App benötigt diese Funktion, um Testanzeigen und Live-Werbung zu erhalten.

2. Sollte in Ihrem Projekt die Zielplattform **ANYCPU** definiert sein, müssen Sie eine architekturspezifische Buildausgabe verwenden (z. B. **X86**) und das Projekt entsprechend aktualisieren. Sollte in Ihrem Projekt die Zielplattform **ANYCPU** definiert sein, können Sie bei den folgenden Schritten keinen Verweis auf die Microsoft Advertising-Bibliotheken hinzufügen. Weitere Informationen finden Sie unter [Referenzfehler, die durch die Ausrichtung auf eine beliebige CPU (Any CPU) in Ihrem Projekt verursacht werden](known-issues-for-the-advertising-libraries.md#reference_errors).

3. Hinzufügen eines Verweises auf die Microsoft Advertising-SDK in Ihrem Projekt:

    1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf **Verweise**, und wählen Sie **Verweis hinzufügen...** aus.
    2.  Erweitern Sie im **Verweis-Manager** den Knoten **Universal Windows**, klicken Sie auf **Erweiterungen**, und wählen Sie dann das Kontrollkästchen neben **Microsoft Advertising SDK für XAML** (Version 10.0).
    3.  Klicken Sie im **Verweis-Manager** auf „OK“.

4.  Ändern Sie das XAML für die Seite, in die Sie Werbung einbetten möchten, um den **Microsoft.Advertising.WinRT.UI**-Namespace miteinzubeziehen. Beispiel: Bei der Standard-Beispiel-App, die von Visual Studio generiert wird (in dieser App mit MyAdFundedWindows10AppXAML bezeichnet), heißt die XAML-Seite **"MainPage.xaml"** .

    Der Bereich **Seite** der von Visual Studio generierten Datei „MainPage.xaml“ hat den folgenden Code.

    ``` xml
    <Page
      x:Class="MyAdFundedWindows10AppXAML.MainPage"
      xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
      xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
      xmlns:local="using:MyAdFundedWindows10AppXAML"
      xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
      xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
      mc:Ignorable="d">
      <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
      </Grid>
    </Page>
    ```

    Fügen Sie den Namespace-Verweis **Microsoft.Advertising.WinRT.UI** ein, sodass der Abschnitt **Seite** der Datei „MainPage.xaml“ den folgenden Code hat.

    ``` xml
    <Page
      x:Class="MyAdFundedWindows10AppXAML.MainPage"
      xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
      xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
      xmlns:local="using:MyAdFundedWindows10AppXAML"
      xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
      xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
      xmlns:UI="using:Microsoft.Advertising.WinRT.UI"
      mc:Ignorable="d">
      <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
      </Grid>
    </Page>
    ```

5. Fügen Sie im **Raster**-Tag den Code für die **AdControl** ein. Weisen Sie die Eigenschaften [AdUnitId](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.adunitid) und [ApplicationId](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.applicationid) dem [Testanzeigen-Einheitenwert](set-up-ad-units-in-your-app.md#test-ad-units) hinzu. Passen Sie zudem **Höhe** und **Breite** des Steuerelements an, damit es einer der [unterstützten Anzeigengrößen für Werbebanner](supported-ad-sizes-for-banner-ads.md) entspricht.

    > [!NOTE]
    > Jedes **AdControl** verfügt über ein entsprechendes *Anzeigeneinheit*, die von unseren Diensten verwendet wird, um Werbung auf das Steuerelement zu übertragen, und jede Anzeigeneinheit besteht aus einer *Anzeigeneinheits-ID* und *Anwendungs-ID*. In den folgenden Schritten weisen Sie dem Steuerelement eine Anzeigeneinheits-ID und Anwendungs-ID zu. Dieser Test Werte können nur in einer Testversion Ihrer App verwendet werden. Bevor Sie Ihre APP im Store veröffentlichen, müssen Sie [diese Testwerte durch livewerte](#release) aus Partner Center ersetzen.

    Das vollständige **Raster**-Tag sieht aus wie dieser Code.

    ``` xml
    <Grid Background="{StaticResource ApplicationPageBackgroundThemeBrush}">
        <UI:AdControl ApplicationId="3f83fe91-d6be-434d-a0ae-7351c5a997f1"
            AdUnitId="test"
            HorizontalAlignment="Left"
            Height="250"
            VerticalAlignment="Top"
            Width="300"/>
    </Grid>
    ```

    Der vollständige Code für die Datei „MainPage.xaml“ sollte wie folgt aussehen.

    ``` xml
    <Page
      x:Class="MyAdFundedWindows10AppXAML.MainPage"
      xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
      xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
      xmlns:local="using:MyAdFundedWindows10AppXAML"
      xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
      xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
      xmlns:UI="using:Microsoft.Advertising.WinRT.UI"
      mc:Ignorable="d">
      <Grid Background="{StaticResource ApplicationPageBackgroundThemeBrush}">
            <UI:AdControl ApplicationId="3f83fe91-d6be-434d-a0ae-7351c5a997f1"
                  AdUnitId="test"
                  HorizontalAlignment="Left"
                  Height="250"
                  VerticalAlignment="Top"
                  Width="300"/>
      </Grid>
    </Page>
    ```

6.  Kompilieren und Ausführen der App, um sie mit einer Anzeige zu sehen

<span id="release" />

## <a name="release-your-app-with-live-ads"></a>Veröffentlichen Ihrer App mit Live-Anzeigen

1. Stellen Sie sicher, dass die Verwendung von Werbebannern in Ihrer App unseren [Richtlinien für das Anzeigen von Werbebannern](ui-and-user-experience-guidelines.md#guidelines-for-banner-ads) entspricht.

2.  Wechseln Sie in Partner Center zur Seite [in-App-ADS](../publish/in-app-ads.md) , und [Erstellen Sie eine Ad-Einheit](set-up-ad-units-in-your-app.md#live-ad-units). Geben Sie als Einheitentyp **Banner** an. Notieren Sie sich die Anzeigeneinheits-ID und die Anwendungs-ID.
    > [!NOTE]
    > Die Anwendungs-IDs für Test-Anzeigeneinheiten und Live-UWP-Anzeigeneinheiten besitzen unterschiedliche Formate. Testanwendungs-ID sind GUIDs. Wenn Sie eine Live-UWP-Ad-Einheit in Partner Center erstellen, entspricht der Wert der Anwendungs-ID für die Ad-Einheit immer der Speicher-ID für Ihre APP (ein Beispiel für eine Store-ID sieht wie 9nblggh4r315 aus).

3. Sie können optional die Anzeigenvermittlung für **AdControl** durch Konfigurieren der [Vermittlungseinstellungen](../publish/in-app-ads.md#mediation) auf der Seite [In-App-Anzeigen](../publish/in-app-ads.md) aktivieren. Mit der Anzeigenvermittlung können Sie Ihre Anzeigenumsätze maximieren und Werbefunktionen optimal nutzen, indem Sie Anzeigen aus mehreren Anzeigennetzwerken anzeigen, einschließlich Anzeigen aus anderen kostenpflichtigen Anzeigennetzwerken wie Taboola und Smaato sowie Anzeigen zu Werbekampagnen für Microsoft-Apps.

4.  Ersetzen Sie in Ihrem Code die Ad-Einheiten Werte (**ApplicationId** und **adunitid**) der Test durch die im Partner Center generierten livewerte.

5.  Über [Mitteln Sie Ihre APP](../publish/app-submissions.md) mithilfe von Partner Center an den Store.

6.  Überprüfen Sie Ihre [Leistungsberichte](../publish/advertising-performance-report.md) in Partner Center.

<span id="manage" />

## <a name="manage-ad-units-for-multiple-ad-controls-in-your-app"></a>Verwalten von Anzeigeeinheiten für mehrere Steuerelemente von Anzeigen in Ihrer App

Sie können mehrere **AdControl** Objekte in einer einzelnen App nutzen (z. B. jede Seite in Ihrer App hostet ein anderes **AdControl** Objekt). In diesem Fall wird empfohlen, dass Sie jedem Steuerelement eine andere Anzeigeneinheit zuweisen. Durch das Verwenden verschiedener Anzeigeeinheiten für jedes Steuerelement können Sie für jedes Steuerelement [die Einstellungen für die Anzeigenvermittlung konfigurieren](../publish/in-app-ads.md#mediation) und diskrete [Berichtsdaten](../publish/advertising-performance-report.md). Außerdem können unsere Dienste so die Werbung optimieren, die wir in Ihrer App anzeigen.

> [!IMPORTANT]
> Sie können jede Anzeigeneinheit in nur einer App verwenden. Wenn Sie eine Anzeigeneinheit in mehr als einer App verwenden, werden für die Ad-Einheit keine Anzeigen platziert.

## <a name="related-topics"></a>Verwandte Themen

* [Richtlinien für Bannerwerbung](ui-and-user-experience-guidelines.md#guidelines-for-banner-ads)
* [Exemplarische Vorgehensweise zur Fehlerbehandlung in XAML/C#](error-handling-in-xamlc-walkthrough.md).
* [Anzeigenbeispiele auf GitHub](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising)
* [Einrichten von Ad-Einheiten für Ihre APP](set-up-ad-units-in-your-app.md)
