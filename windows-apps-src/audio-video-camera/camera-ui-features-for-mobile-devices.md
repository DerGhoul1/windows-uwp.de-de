---
ms.assetid: c43d4af3-9a1a-4eae-a137-1267c293c1b5
description: Dieser Artikel beschreibt, wie Sie spezielle Kamera-UI-Features nutzen, die nur auf mobilen Geräten vorhanden sind.
title: Kamera-UI-Features für mobile Geräte
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: a6e72aea74c4aed092cab450c05dc0982e838f09
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66358970"
---
# <a name="camera-ui-features-for-mobile-devices"></a>Kamera-UI-Features für mobile Geräte

Dieser Artikel beschreibt, wie Sie spezielle Kamera-UI-Features nutzen, die nur auf mobilen Geräten vorhanden sind. 

## <a name="add-the-mobile-extension-to-your-project"></a>Hinzufügen der mobilen Erweiterung zu Ihrem Projekt 

Um diese Features zu verwenden, müssen Sie einen Verweis auf das Microsoft Mobile Extension SDK für die Universelle App-Plattform zu Ihrem Projekt hinzufügen.

**Um einen Verweis auf das mobile Erweiterungs-SDK für die Unterstützung für Serverhardware Kamera Schaltfläche hinzufügen**

1.  Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf **Verweise**, und wählen Sie **Verweis hinzufügen** aus.

2.  Erweitern Sie den Knoten für die **universelle Windows-App**, und wählen Sie **Erweiterungen** aus.

3.  Aktivieren Sie das Kontrollkästchen **Microsoft Mobile Extension SDK für Universelle App-Plattform**.

## <a name="hide-the-status-bar"></a>Ausblenden der Statusleiste

Mobile Geräte verfügen über ein [**StatusBar**](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.StatusBar)-Steuerelement, das dem Benutzer Statusinformationen zum Gerät liefert. Dieses Steuerelement benötigt Speicherplatz auf dem Bildschirm, der die Medienaufnahme-UI beeinträchtigen kann. Sie können die Statusleiste ausblenden, indem Sie [**HideAsync**](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.statusbar.hideasync) aufrufen. Dieser Aufruf muss jedoch innerhalb eines Bedingungsblocks ausgeführt werden, in dem Sie die [**ApiInformation.IsTypePresent**](https://docs.microsoft.com/uwp/api/windows.foundation.metadata.apiinformation.istypepresent)-Methode verwenden, um festzustellen, ob die API verfügbar ist. Diese Methode gibt nur „true“ auf mobilen Geräten zurück, die die Statusleiste unterstützen. Sie sollten die Statusleiste ausblenden, wenn die App gestartet wird oder wenn Sie die Vorschau von der Kamera beginnen.

[!code-cs[HideStatusBar](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetHideStatusBar)]

Wenn die App beendet wird oder der Benutzer die Medienaufnahmeseite der App verlässt, können Sie das Steuerelement wieder einblenden.

[!code-cs[ShowStatusBar](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetShowStatusBar)]

## <a name="use-the-hardware-camera-button"></a>Verwenden der Kamera-Hardwaretaste

Einige mobile Geräte verfügen über eine dedizierte Kamera-Hardwaretaste, die einige Benutzer einem Steuerelement auf dem Bildschirm vorziehen. Um benachrichtigt zu werden, wenn die Hardwaretaste für die Kamera gedrückt wird, registrieren Sie einen Handler für das [**HardwareButtons.CameraPressed**](https://docs.microsoft.com/uwp/api/windows.phone.ui.input.hardwarebuttons.camerapressed)-Ereignis. Da diese API nur auf mobilen Geräten verfügbar ist, müssen Sie erneut **IsTypePresent** verwenden, um sicherzustellen, dass die API auf dem aktuellen Gerät unterstützt wird, bevor Sie versuchen, darauf zuzugreifen.

[!code-cs[PhoneUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetPhoneUsing)]

[!code-cs[RegisterCameraButtonHandler](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetRegisterCameraButtonHandler)]

Im Handler für das **CameraPressed**-Ereignis können Sie die Aufnahme eines Fotos initiieren.

[!code-cs[CameraPressed](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCameraPressed)]

Wenn die App heruntergefahren wird oder der Benutzer die Medienaufnahmeseite der App verlässt, heben Sie die Registrierung des Hardwaretastenhandlers auf.

[!code-cs[UnregisterCameraButtonHandler](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetUnregisterCameraButtonHandler)]

## <a name="related-topics"></a>Verwandte Themen

* [Kamera](camera.md)
* [Erfassen Sie grundlegende Foto, Video- und Audiodateien mit MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)





