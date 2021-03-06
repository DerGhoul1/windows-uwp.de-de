---
description: Erfahren Sie, wie Kontakte und Kalenderinformationen in Ihrer UWP-App verwendet werden.
title: Kontakte und Kalender
ms.assetid: b7e53ab5-2828-4fb7-8656-2bec70b3467f
ms.date: 05/18/2018
ms.topic: article
keywords: Windows 10, UWP, Kontakte, Kalender, Termine, E-Mail-Nachrichten
ms.localizationpriority: medium
ms.openlocfilehash: b2e3f0b1d93d2b2c32e117f61fd7514077ca3923
ms.sourcegitcommit: 90fe7a9a5bfa7299ad1b78bbef289850dfbf857d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2020
ms.locfileid: "84756546"
---
# <a name="contacts-my-people-and-calendar"></a>Kontakte, Meine Kontakte und Kalender


Sie können Benutzern den Zugriff auf ihre Kontakte und Termine ermöglichen, sodass sie Inhalte, E-Mails, Kalenderinformationen oder Nachrichten mit anderen Benutzern oder beliebiger, von Ihnen entwickelter Funktionalität teilen können.

Die folgenden Themen enthalten Informationen zu verschiedenen Verfahren, wie Ihre App auf Kontakte und Termine zugreifen kann:

| Thema | Beschreibung |
|-------|-------------|
| [Auswählen von Kontakten](selecting-contacts.md) | Mit dem [<strong>Windows.ApplicationModel.Contacts</strong>](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Contacts)-Namespace verfügen Sie über mehrere Optionen zum Auswählen von Kontakten. Wir zeigen Ihnen hier, wie Sie einen einzelnen Kontakt oder mehrere Kontakte auswählen und wie Sie die Kontaktauswahl so konfigurieren, dass nur die von der App benötigten Kontaktinformationen abgerufen werden. |
| [Senden von E-Mails](sending-email.md) | Hier erfahren Sie, wie Sie das Dialogfeld zum Verfassen einer E-Mail starten, damit Benutzer eine E-Mail senden können. Sie können die Felder der E-Mail vor dem Anzeigen des Dialogfelds mit Daten füllen. Die Nachricht wird erst gesendet, wenn Benutzer auf die Schaltfläche „Senden“ tippen. |
| [Senden einer SMS](sending-an-sms-message.md) | In diesem Thema erfahren Sie, wie Sie das Dialogfeld zum Verfassen einer SMS starten, damit Benutzer eine SMS senden können. Sie können die Felder der SMS vor dem Anzeigen des Dialogfelds mit Daten füllen. Die Nachricht wird erst gesendet, wenn Benutzer auf die Schaltfläche „Senden“ tippen. |
| [Verwalten von Terminen](managing-appointments.md) | Mit dem [<strong>Windows.ApplicationModel.Appointments</strong>](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Appointments)-Namespace können Sie in der Kalender-App eines Benutzers Termine erstellen und verwalten. Hier erfahren Sie, wie Sie einen Termin erstellen, einer Kalender-App hinzufügen, in der Kalender-App ersetzen und aus der Kalender-App entfernen. Außerdem wird erläutert, wie Sie eine Zeitspanne für eine Kalender-App anzeigen und ein Terminwiederholungsobjekt erstellen. |
| [Verbinden der App mit Aktionen auf einer Visitenkarte](integrating-with-contacts.md) | Zeigt, wie Ihre App neben Aktionen auf einer Visitenkarte oder einer kleinen Visitenkarte angezeigt werden kann. Benutzer können Ihre App auswählen, um eine Aktion auszuführen, z. B. eine Profilseite zu öffnen, einen Anruf zu tätigen oder eine Nachricht zu senden. |
| [Hinzufügen von Unterstützung für „Meine Kontakte“ zu einer Anwendung](my-people-support.md) | Hier wird gezeigt, wie Sie einer Anwendung Unterstützung für „Meine Kontakte” hinzufügen und wie Sie Kontakte auf der Taskleiste anheften und lösen. |
| [Freigeben von „Meine Kontakte”](my-people-sharing.md) | Hier wird gezeigt, wie Sie Unterstützung für die Freigabe von „Meine Kontakte” hinzufügen, sodass Benutzer Inhalte für ihre angehefteten Kontakte freigeben können, indem sie Dateien aus dem Datei-Explorer auf die angeheftete Instanz von „Meine Kontakte” ziehen. |
| [Benachrichtigungen für „Meine Kontakte“](my-people-notifications.md) | Hier erfahren Sie, wie Sie Benachrichtigungen für „Meine Kontakte“ erstellen und verwenden. Dies ist eine neue Art von Popupbenachrichtigungen, die von einem angehefteten Kontakt gesendet werden. |

 

## <a name="related-topics"></a>Zugehörige Themen

* [Beispiel zur Termin-API](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Appointments)
* [Beispiel zur Kontakt-Manager-API](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/master/Official%20Windows%20Platform%20Sample/Contact%20manager%20API%20sample)
* [Beispiel-App für die Kontaktauswahl](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/ContactPicker)
* [Beispiel zum Behandeln von Kontaktaktionen](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/master/Official%20Windows%20Platform%20Sample/Windows%208.1%20Store%20app%20samples/99866-Windows%208.1%20Store%20app%20samples/Handling%20Contact%20Actions)
* [Beispiel für die Visitenkartenintegration](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ContactCardIntegration)
