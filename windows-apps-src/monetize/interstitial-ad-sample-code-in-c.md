---
ms.assetid: 7a16b0ca-6b8e-4ade-9853-85690e06bda6
description: Erfahren Sie, wie Sie mithilfe von C# Interstitialwerbung veröffentlichen.
title: Beispielcode für Interstitialwerbung in C#
ms.date: 02/18/2020
ms.topic: article
keywords: Windows 10, UWP, Anzeigen, Werbung, Interstitial, C#, Beispielcode
ms.localizationpriority: medium
ms.openlocfilehash: 9d908151e30510977669f2bc101575754a946560
ms.sourcegitcommit: 71f9013c41fc1038a9d6c770cea4c5e481c23fbc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/20/2020
ms.locfileid: "77507044"
---
# <a name="interstitial-ad-sample-code-in-c"></a>Beispielcode für die Interaktion mit Interstitial in C\# #  

>[!WARNING]
> Ab dem 1. Juni 2020 wird die Microsoft AD-Monetarisierungsplattform für Windows UWP-apps heruntergefahren. [Weitere Informationen](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

Dieses Thema enthält den vollständigen Beispielcode für eine grundlegende C#- und XAML-App für die universelle Windows-Plattform (UWP), in der Videointerstitialwerbung angezeigt wird. Eine schrittweise Anleitung dazu, wie Sie Ihr Projekt zur Verwendung dieses Codes konfigurieren, finden Sie unter [Interstitialwerbung](interstitial-ads.md). Ein vollständiges Beispielprojekt finden Sie unter den [Anzeigenbeispielen auf GitHub](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising).

## <a name="code-example"></a>Codebeispiel

In diesem Abschnitt wird der Inhalt der Dateien „MainPage.xaml“ und „MainPage.xaml.cs“ in einer einfachen App veranschaulicht, in der eine Interstitialwerbung angezeigt wird. Um die Beispiele zu verwenden, kopieren Sie den Code in ein Visual C#-Projekt **Leere App (Universelle Windows-App)** in Visual Studio.

Diese Beispiel-App verwendet zwei Schaltflächen, um eine Interstitialwerbung anzufordern und dann zu starten. Ersetzen Sie die Werte der Felder ```myAppId``` und ```myAdUnitId``` durch livewerte aus Partner Center, bevor Sie Ihre APP an den Store übermitteln. Weitere Informationen finden Sie unter [Einrichten von Anzeigeneinheiten in der App](set-up-ad-units-in-your-app.md#live-ad-units).

> [!NOTE]
> Wenn Sie dieses Beispiel ändern möchten, um eine Interstitialwerbung mit Bannern anstelle einer Videointerstitialanzeige anzuzeigen, übergeben Sie den Wert **AdType.Display** an den ersten Parameter der [RequestAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad)-Methode anstelle von **AdType.Video**. Weitere Informationen finden Sie unter [Interstitialanzeigen](interstitial-ads.md).

### <a name="mainpagexaml"></a>MainPage.xaml

> [!div class="tabbedCodeSnippets"]
[!code-xml[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml#L1-L13)]

### <a name="mainpagexamlcs"></a>MainPage.xaml.cs

> [!div class="tabbedCodeSnippets"]
[!code-csharp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#CompleteSample)]

 
## <a name="related-topics"></a>Verwandte Themen

* [Anzeigenbeispiele auf GitHub](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising)
 
