---
description: Verwenden Sie diese Methode aus der Microsoft Store-Analyse-API, um die CAB-Datei für einen Fehler in der Desktopanwendung herunterzuladen.
title: Herunterladen der CAB-Datei bei einem Fehler in der Desktopanwendung
ms.date: 03/06/2018
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store-Analyse-API, CAB herunterladen, Desktopanwendung
ms.localizationpriority: medium
ms.openlocfilehash: 7a0c6203b3a55ecf8ca5e9473a41a7e6fb233000
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66371294"
---
# <a name="download-the-cab-file-for-an-error-in-your-desktop-application"></a>Herunterladen der CAB-Datei bei einem Fehler in der Desktopanwendung

Verwenden Sie diese Methode aus der Microsoft Store-Analyse-API, um die CAB-Datei herunterzuladen, die einem bestimmten Fehler einer Desktopanwendung zugeordnet ist, die Sie zum [Windows-Desktopanwendungsprogramm](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program) hinzugefügt haben. Diese Methode kann nur die CAB-Datei für einen App-Fehler herunterladen, die in den letzten 30 Tagen aufgetreten ist. Download der CAB-Dateien sind auch in der [Integritätsbericht](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program) für desktop-Anwendungen im Partner Center.

Bevor Sie diese Methode verwenden können, müssen Sie zuerst anhand der Methode zum [Abrufen von Informationen zu einem Fehler in der Desktopanwendung](get-details-for-an-error-in-your-desktop-application.md) den ID-Hash der CAB-Datei abrufen, die Sie herunterladen möchten.

## <a name="prerequisites"></a>Vorraussetzungen


Zur Verwendung dieser Methode sind folgende Schritte erforderlich:

* Falls noch nicht geschehen, erfüllen Sie alle [Voraussetzungen](access-analytics-data-using-windows-store-services.md#prerequisites) für die Microsoft Store-Analyse-API.
* [Rufen Sie ein Azure AD-Zugriffstoken ab](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token), das im Anforderungsheader für diese Methode verwendet wird. Nach Erhalt eines Zugriffstokens können Sie es 60 Minuten lang verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.
* Rufen Sie den ID-Hash der CAB-Datei ab, die Sie herunterladen möchten. Verwenden Sie zum Abrufen dieses Wertes die Methode zum [Abrufen von Details zu einem Fehler in Ihrer Desktopanwendung](get-details-for-an-error-in-your-desktop-application.md), um Details zu einem bestimmten Fehler in Ihrer App abzurufen, und verwenden Sie den **cabIdHash**-Wert im Antworttext dieser Methode.

## <a name="request"></a>Anforderung


### <a name="request-syntax"></a>Anforderungssyntax

| Methode | Anforderungs-URI                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/cabdownload``` |


### <a name="request-header"></a>Anforderungsheader

| Header        | Typ   | Beschreibung                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorisierung | String | Erforderlich. Die Azure AD-Zugriffstoken in der Form **Bearer** &lt; *token*&gt;. |


### <a name="request-parameters"></a>Anforderungsparameter

| Parameter        | Typ   |  Beschreibung      |  Erforderlich  |
|---------------|--------|---------------|------|
| applicationId | String | Die Produkt-ID der Desktopanwendung, für die Sie eine CAB-Datei herunterladen möchten. Rufen Sie die Produkt-ID, einer Desktopanwendung öffnen [Partner Center-Analyse zu melden, für die desktop-Anwendung](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program) (z. B. die **Integritätsbericht**) und die Produkt-ID aus der URL abzurufen. |  Ja  |
| cabIdHash | String | Der eindeutige ID-Hash der CAB-Datei ab, die Sie herunterladen möchten. Verwenden Sie zum Abrufen dieses Wertes die Methode zum [Abrufen von Details zu einem Fehler in Ihrer Desktopanwendung](get-details-for-an-error-in-your-desktop-application.md), um Details zu einem bestimmten Fehler in Ihrer Anwendung abzurufen, und verwenden Sie den **cabIdHash**-Wert im Antworttext dieser Methode. |  Ja  |


### <a name="request-example"></a>Anforderungsbeispiel

Im folgenden Beispiel wird gezeigt, wie Sie mit dieser Methode eine CAB-Datei herunterladen. Ersetzen Sie die Parameter *applicationId* und *cabIdHash* durch die entsprechende Werte für Ihre Desktopanwendung.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/desktop/cabdownload?applicationId=10238467886765136388&cabIdHash=54ffb83a-e159-41d2-8158-f36f306cc01e HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Antwort

Bei dieser Methode wird ein 302-Antwortcode (Umleitung) zurückgegeben, und der **Speicherort**-Header in der Antwort wird der Shared Access Signature (SAS)-URI der CAB-Datei zugewiesen. Der Aufrufer wird zu dieser URI für den automatischen Download der CAB-Datei weitergeleitet.

## <a name="related-topics"></a>Verwandte Themen

* [Bericht zur Integrität](../publish/health-report.md)
* [Access-Analytics-Daten mithilfe von Microsoft Store services](access-analytics-data-using-windows-store-services.md)
* [Abrufen von Daten für die desktop-Anwendung für die Fehlerberichterstattung](get-desktop-application-error-reporting-data.md)
* [Abrufen von Details für einen Fehler in die desktop-Anwendung](get-details-for-an-error-in-your-desktop-application.md)
* [Die stapelüberwachung für einen Fehler in die desktop-Anwendung abrufen.](get-the-stack-trace-for-an-error-in-your-desktop-application.md)
