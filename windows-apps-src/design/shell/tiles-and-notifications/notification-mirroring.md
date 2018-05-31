---
author: anbare
Description: Learn how to use notification mirroring on your toast notifications.
title: Spiegelung der Benachrichtigung
label: Notification mirroring
template: detail.hbs
ms.author: mijacobs
ms.date: 12/15/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP, Popup, Info-Center in der Cloud, Spiegelung der Benachrichtigung, Benachrichtigung, geräteübergreifend
ms.localizationpriority: medium
ms.openlocfilehash: 6eda26c37bc95943691063c3d21d39a6c7a1d7b2
ms.sourcegitcommit: eead3c00b27d9f66f79ec08c81a97e91dc1fdb3c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/18/2018
ms.locfileid: "1523268"
---
# <a name="notification-mirroring"></a>Spiegelung der Benachrichtigung

Mit der Spiegelung der Benachrichtigung, die vom Info-Center in der Cloud unterstützt wird, können Sie auf Ihrem Telefon Benachrichtigungen auf Ihrem PC anzeigen.

> [!IMPORTANT]
> **Benötigt Anniversary Update**: Sie müssen Build 14393 oder höher ausführen, um die Spiegelung der Benachrichtigung verwenden zu können. Wenn Sie die Spiegelung der Benachrichtigung aus Ihrer App entfernen möchten, müssen Sie als Ziel die SDK-Version 14393 für den Zugriff auf die gespiegelten APIs verwenden.

Benutzer können mit der Spiegelung Benachrichtigung und Cortana Handy-Benachrichtigungen empfangen und darauf reagieren (Windows Mobile und Android) und zwar bequem von ihrem PC aus. Als Entwickler müssen Sie nichts unternehmen, um Benachrichtigung der Spiegelung zu aktivieren, die Spiegelung funktioniert automatisch! Wenn Sie Schaltflächen auf dem gespiegelten Popup anklicken, z. B. Schnellantworten, werden diese zurück an das Telefon geleitet, und rufen eine Hintergrundaufgabe hervor oder starten die Vordergrund-App.

<img alt="Notification mirroring diagram" src="images/toast-mirroring.gif" width="350"/>

Entwickler haben zwei Vorteile bei der Spiegelung von Benachrichtigung: die gespiegelten Benachrichtigungen führen zu weiterem Interesse der Benutzer mit dem Dienst, und darüber hinaus ermöglichen Sie den Benutzern, Ihre Windows Store-Desktop-Apps zu entdecken. Ihre Benutzer wissen möglicherweise nicht einmal, was für eine bemerkenswerte UWP-App für ihren Windows10-Desktop verfügbar ist. Wenn Benutzer die gespiegelte Benachrichtigung auf dem Handy empfangen, können sie auf die Popupbenachrichtigung klicken und werden zum Windows Store weitergeleitet, von dem sie Ihre UWP-Desktop-App installieren können.

Die Spiegelung funktioniert mit den Windows Phone und Android. Benutzer müssen bei Cortana und auf ihrem Handy und dem Desktop angemeldet sein, damit die Spiegelung der Benachrichtigung funktioniert.


## <a name="what-if-the-app-is-installed-on-both-devices"></a>Was geschieht, wenn die App auf beiden Geräten installiert ist?

Wenn der Benutzer Ihre App bereits auf dem PC installiert hat, werden wir die gespiegelte Phone-Benachrichtigung automatisch stumm schalten, damit keine doppelte Benachrichtigung nicht angezeigt wird. Gespiegelte Benachrichtigungen werden automatisch anhand der folgenden Kriterien stummgeschaltet...

1. Eine App auf dem PC existiert entweder mit dem **gleichen Anzeigenamen oder dem gleichen PFN** (Paketfamilienname)
2. Diese PC-App hat eine Popupbenachrichtigung gesendet.

Wenn die PC-App noch kein Popup gesendet hat, zeigen wir weiterhin die Phone-Benachrichtigungen, dass sind, da der Benutzer wahrscheinlich die PC-App noch nicht gestartet hat.


## <a name="how-to-opt-out-of-mirroring"></a>Deaktivieren der Spiegelung

UWP-App-Entwickler, Unternehmen und Benutzer können die Benachrichtigung der Spiegelung deaktivieren.

> [!NOTE]
> Das Deaktivieren der Spiegelung deaktiviert außerdem [Universal Dismiss](universal-dismiss.md).


### <a name="as-a-developer-opt-out-an-individual-notification"></a>Deaktivieren Sie als Entwickler eine einzelne Benachrichtigung

Sie haben möglicherweise gelegentlich eine Benachrichtigung für bestimmte Geräte, die nicht auf anderen Geräten gespiegelt werden soll. Sie können verhindern, dass eine Benachrichtigung gespiegelt wird, indem Sie die **Spiegelungs**-Eigenschaft für die Popupbenachrichtigung festlegen. Diese Eigenschaft der Spiegelung kann derzeit nur auf lokale Benachrichtigungen festgelegt werden (es kann nicht angegeben werden, wenn eine WNS-Pushbenachrichtigung gesendet wird).

**Bekanntes Problem**: Das Abrufen der Spiegelungseigenschaften über die `ToastNotificationHistory.GetHistory()`-API gibt immer den Standardwert zurück (**Zugelassen**) und nicht die Option, die Sie angegeben haben. Keine Sorge, alles funktioniert – es wird nur der Wert abgerufen, der beschädigt ist.

```csharp
var toast = new ToastNotification(xml)
{
    // Disable mirroring of this notification
    Mirroring = NotificationMirroring.Disabled
};
  
ToastNotificationManager.CreateToastNotifier().Show(toast);
```


### <a name="as-a-developer-opt-out-completely"></a>Als Entwickler sollten Sie diese Option vollkommen ablehnen

Entwickler können die Spiegelung der Benachrichtigung vollständig in der App deaktivieren. Während wir glauben, dass alle Apps von der Spiegelung profitieren würde, ist ein Deaktivieren ganz einfach. Rufen Sie die folgende Methode nur einmal auf, damit die Option in Ihrer App deaktiviert wird. Sie können z.B. diesen Aufruf im App-Konstruktor in `App.xaml.cs` platzieren...

```csharp
public App()
{
    this.InitializeComponent();
    this.Suspending += OnSuspending;
 
    // Disable notification mirroring for entire app
    ToastNotificationManager.ConfigureNotificationMirroring(NotificationMirroring.Disabled);
}
```


### <a name="as-an-enterprise-how-do-i-opt-out"></a>Wie entferne ich die Option als Unternehmen?

Unternehmen können die Spiegelung der Benachrichtigung vollständig deaktivieren. Bearbeiten sie einfach die Gruppenrichtlinie, um die zu spiegelnde Benachrichtigungen zu deaktivieren.


### <a name="as-a-user-how-do-i-opt-out"></a>Wie entferne ich die Option als Benutzer?

Benutzer können eine einzelne App vollständig deaktivieren, indem Sie das Feature deaktivieren. Wenn keine bestimmten App-Benachrichtigungen auf Ihren Desktop gespiegelt werden sollen, können Sie einfach diese bestimmte App deaktivieren. Diese Optionen finden Sie in den Cortana Einstellungen auf dem Handy und dem PC.