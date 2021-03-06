---
description: Hier erfahren Sie, wie Sie Ihre App aktualisieren, damit Sie die neuesten unterstützten Microsoft Advertising-Bibliotheken verwenden können und Ihre App weiterhin Banneranzeigen erhält.
title: Verwenden Sie die neuesten Werbe Bibliotheken für Bannerwerbung.
ms.date: 02/18/2020
ms.topic: article
keywords: Windows 10, UWP, Anzeigen, Werbung, AdControl, AdMediatorControl, Migrieren
ms.assetid: f8d5b2ad-fcdb-4891-bd68-39eeabdf799c
ms.localizationpriority: medium
ms.openlocfilehash: 2fa57e02574850704b6c43853980dde32c388557
ms.sourcegitcommit: 71f9013c41fc1038a9d6c770cea4c5e481c23fbc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/20/2020
ms.locfileid: "77507084"
---
# <a name="update-your-app-to-the-latest-advertising-libraries-for-banner-ads"></a>Aktualisieren Ihrer App auf die neuesten Advertising-Bibliotheken für Banneranzeigen

>[!WARNING]
> Ab dem 1. Juni 2020 wird die Microsoft AD-Monetarisierungsplattform für Windows UWP-apps heruntergefahren. [Weitere Informationen](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

Am dem 1. April 2017 werden Banneranzeigen nicht mehr für Apps bereitgestellt, die nicht unterstützte Advertising-SDK-Versionen verwenden. Bei Verwendung von **AdControl** zum Anzeigen von Banneranzeigen in Ihrer universellen Windows-Plattform-App (UWP), verwenden Sie die Informationen in diesem Artikel, um herauszufinden, ob Sie eine nicht unterstützte Advertising-SDK verwenden und migrieren Sie Ihre App zu einem unterstützten SDK.

## <a name="overview"></a>Übersicht

UWP-Apps, die Banneranzeige verwenden, müssen **AdControl** aus der Advertising-Bibliotheken verwenden, die in [Microsoft Advertising-SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK) verteilt werden. Diese SDK-Version unterstützt einen mindestens erforderlichen Satz von Funktionen. Dies umfasst auch die Möglichkeit, HTML5-Rich-Media über die [Mobile Rich Media Ad Interface Definitions (MRAID) 1.0-Spezifikation](https://www.iab.com/wp-content/uploads/2015/08/IAB_MRAID_VersionOne.pdf) vom Interactive Advertising Bureau (IAB) zu verarbeiten. Viele unserer Werbekunden wünschen sich diese Funktionen und wir setzen voraus, dass App-Entwickler eine dieser SDK-Versionen verwenden, damit unser App-Ökosystem für Werbekunden attraktiver wird und Ihr Umsatz wächst.

Vor der Freigabe dieses SDK wurde die **AdControl**-Klasse in mehreren älteren Advertising-SDK-Versionen angeboten. Diese älteren Advertising-SDK-Versionen werden nicht mehr unterstützt, da sie den oben beschriebenen mindestens erforderlichen Satz von Funktionen nicht unterstützen. Am dem 1. April 2017 werden Banneranzeigen nicht mehr für Apps bereitgestellt, die nicht unterstützte Advertising-SDK-Versionen verwenden. Wenn Sie eine App besitzen, die weiterhin eine nicht unterstützte Advertising-SDK-Version verwendet, geschieht Folgendes:

* Banneranzeigen werden nicht mehr für **AdControl** in Ihrer App bereitgestellt, und Sie erhalten keinen Anzeigenumsatz mehr aus diesen Steuerelementen.

* Wenn **AdControl** in Ihrer App eine neue Anzeige anfordert, wird ein **ErrorOccurred**-Ereignis des Steuerelements ausgegeben, und die **ErrorCode**-Eigenschaft der Ereignisargumente hat den Wert **NoAdAvailable**.

* Alle Anzeigeneinheiten, die Ihrer App zugeordnet sind, werden deaktiviert. Sie können diese deaktivierten Ad-Einheiten nicht aus Ihrem departnerv Center-Konto entfernen. Wenn Sie Ihre App aktualisieren, damit Sie ein unterstütztes [Microsoft Advertising-SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK) nutzen können, ignorieren Sie diese Anzeigeneinheiten und erstellen Sie neue.

* Banneranzeigen werden nicht mehr für Anzeigeneinheiten bereitgestellt, die in mehreren Apps verwendet werden. Stellen Sie sicher, dass Ihre Anzeigeneinheiten jeweils nur in einer App verwendet werden.

Wenn Sie über eine App verfügen (bereits im Store oder noch in Entwicklung), die Banneranzeigen mithilfe von **AdControl** anzeigt, und Sie nicht sicher sind, welches Advertising-SDK von Ihrer App verwendet wird, ermitteln Sie anhand der Anweisungen in diesem Artikel, ob Sie Ihre App auf ein unterstütztes SDK aktualisieren müssen. Wenn Schwierigkeiten auftreten oder Sie Hilfe benötigen, [wenden Sie sich an den Support](https://support.microsoft.com/getsupport/hostpage.aspx?locale=EN-US&supportregion=EN-US&ccfcode=US&ln=EN-US&pesid=14654&oaspworkflow=start_1.0.0.0&tenant=store&supporttopic_L1=32136151).

> [!NOTE]
> Wenn Ihre App bereits [Microsoft Advertising-SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK) (für UWP-Apps) verwendet müssen für Ihre App keine weiteren Änderungen vorgenommen werden.

## <a name="prerequisites"></a>Erforderliche Komponenten

* Der komplette Quellcode und Visual Studio-Projektdateien für Ihre App, die **AdControl** verwendet.
* Das APPX-Paket für Ihre App.

> [!NOTE]
> Wenn Sie über das APPX-Paket für Ihre App nicht mehr verfügen, Ihnen aber noch immer ein Entwicklungscomputer mit der Version von Visual Studio und dem Advertising-SDK, das für die App-Erstellung eingesetzt wurde, zur Verfügung steht, können Sie das APPX- Paket in Visual Studio neu generieren.

<span id="part-1" />

## <a name="part-1-determine-whether-you-need-to-update-your-uwp-app"></a>Teil 1: Ermitteln, ob Ihre UWP-App aktualisiert werden muss

Befolgen Sie die Anweisungen in den folgenden Abschnitten, um festzustellen, ob Sie Ihre App aktualisieren müssen.

1. Erstellen Sie eine Kopie des APPX-Pakets für Ihre App, damit das Original nicht beeinträchtigt wird, benennen Sie die Kopie um, sodass sie die Erweiterung „.zip“ erhält, und extrahieren Sie den Inhalt der Datei.

2. Überprüfen Sie den extrahierten Inhalt Ihres App-Pakets:
  * Wenn die Datei „Microsoft.Advertising.dll“ vorhanden ist, verwendet Ihre App ein altes SDK, und Sie müssen Ihr Projekt aktualisieren, indem Sie die Anweisungen in den folgenden Abschnitten ausführen. Fahren Sie mit [Teil 2](update-your-app-to-the-latest-advertising-libraries.md#part-2) fort.
  * Wenn die Datei „Microsoft.Advertising.dll“ nicht vorhanden ist, verwendet Ihre UWP-App bereits das neueste verfügbare Advertising SDK, und Sie müssen keine Änderungen an Ihrem Projekt vornehmen.


<span id="part-2" />

## <a name="part-2-install-the-latest-sdk"></a>Teil 2: Installieren der aktuellen SDK-Version

Wenn Ihre App eine alte SDK-Version verwendet, führen Sie diese Anweisungen aus, um sicherzustellen, dass Sie das aktuelle SDK auf Ihrem Entwicklungscomputer verwenden.

1. Stellen Sie sicher, dass auf Ihrem Entwicklungscomputer Visual Studio 2015 oder eine spätere Version installiert ist.
    > [!NOTE]
    > Wenn Visual Studio auf Ihrem Entwicklungscomputer geöffnet ist, schließen Sie es, bevor Sie die folgenden Schritte ausführen.

1.  Deinstallieren Sie alle früheren Versionen des Microsoft Advertising SDK und Ad Mediator SDK auf Ihrem Entwicklungscomputer.

2.  Öffnen Sie ein **Eingabeaufforderungsfenster**, und führen Sie diese Befehle aus, um alle SDK-Versionen zu löschen, die möglicherweise mit Visual Studio installiert wurden und nicht in der Liste der installierten Programme auf Ihrem Computer angezeigt werden:
    ```syntax
    MsiExec.exe /x{5C87A4DB-31C7-465E-9356-71B485B69EC8}
    MsiExec.exe /x{6AB13C21-C3EC-46E1-8009-6FD5EBEE515B}
    MsiExec.exe /x{6AC81125-8485-463D-9352-3F35A2508C11}
    ```

3.  Installieren des [Microsoft Advertising-SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK).

## <a name="part-3-update-your-project"></a>Teil 3: Aktualisieren Ihres Projekts

Entfernen Sie alle vorhandenen Verweise auf Microsoft Advertising-Bibliotheken aus dem Projekt, und führen Sie zum Hinzufügen der erforderlichen Verweise [diese Anweisungen](install-the-microsoft-advertising-libraries.md#reference) aus. Dadurch wird sichergestellt, dass Ihr Projekt die richtigen Bibliotheken verwendet. Sie können Ihr vorhandenes Markup und den Code beibehalten.

## <a name="part-4-test-and-republish-your-app"></a>Teil 4: Testen und erneutes Veröffentlichen Ihrer App

Testen Sie Ihre App, um sicherzustellen, dass sie Banneranzeigen korrekt anzeigt.

Wenn die vorherige Version der App bereits im Store verfügbar ist, erstellen Sie [eine neue Übermittlung](../publish/app-submissions.md) für Ihre aktualisierte APP im Partner Center, um Ihre APP erneut zu veröffentlichen.
