---
ms.assetid: 24C5F796-5FB8-4B5D-B428-C3154B3098BD
description: Verwenden Sie diese Methode aus der Microsoft Store-Übermittlungs-API zur Aktualisierung einer vorhandenen Flight-Paketübermittlung.
title: Aktualisieren einer Flight-Paket-Übermittlung
ms.date: 04/17/2018
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store-Übermittlungs-API, Flight-Übermittlung, Aktualisieren
ms.localizationpriority: medium
ms.openlocfilehash: a06f341584c88be06e4f8c23a3b86bec9d1cec28
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66360896"
---
# <a name="update-a-package-flight-submission"></a>Aktualisieren einer Flight-Paket-Übermittlung


Verwenden Sie diese Methode aus der Microsoft Store-Übermittlungs-API zur Aktualisierung einer vorhandenen Flight-Paketübermittlung. Nachdem Sie mit dieser Methode eine Übermittlung erfolgreich aktualisiert haben, müssen Sie ein [Commit für die Übermittlung](commit-a-flight-submission.md) für Aufnahme und Veröffentlichung durchführen.

Weitere Informationen dazu, wie diese Methode in das Erstellen einer Flight-Paketübermittlung mit der Microsoft Store-Übermittlungs-API passt, finden Sie unter [Verwalten von Flight-Paketübermittlungen](manage-flight-submissions.md).

## <a name="prerequisites"></a>Vorraussetzungen

Zur Verwendung dieser Methode sind folgende Schritte erforderlich:

* Falls noch nicht geschehen, erfüllen Sie alle [Voraussetzungen](create-and-manage-submissions-using-windows-store-services.md#prerequisites) für die Microsoft Store-Übermittlungs-API.
* [Rufen Sie ein Azure AD-Zugriffstoken ab](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token), das im Anforderungsheader für diese Methode verwendet wird. Nach Erhalt eines Zugriffstokens können Sie es 60 Minuten lang verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.
* Erstellen Sie eine Paket-Flight-Eingabe für eine Ihrer apps. Sie können dies im Partner Center, oder Sie erreichen dies, indem die [erstellen Sie eine Paket-Flight-Eingabe](create-a-flight-submission.md) Methode.

## <a name="request"></a>Anforderung

Diese Methode hat die folgende Syntax. In den folgenden Abschnitten finden Sie Verwendungsbeispiele und Beschreibungen des Header und Anforderungstexts.

| Methode | Anforderungs-URI                                                      |
|--------|------------------------------------------------------------------|
| PUT    | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}``` |


### <a name="request-header"></a>Anforderungsheader

| Header        | Typ   | Beschreibung                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorisierung | String | Erforderlich. Die Azure AD-Zugriffstoken in der Form **Bearer** &lt; *token*&gt;. |


### <a name="request-parameters"></a>Anforderungsparameter

| Name        | Typ   | Beschreibung                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | String | Erforderlich. Die Store-ID der App, für die Sie eine Flight Paketübermittlung aktualisieren möchten. Weitere Informationen zur Store-ID finden Sie unter [Anzeigen von Details zur App-Identität](https://docs.microsoft.com/windows/uwp/publish/view-app-identity-details).  |
| flightId | String | Erforderlich. Die ID des Flight-Pakets, für das Sie eine Übermittlung aktualisieren möchten. Diese ID ist in den Antwortdaten für Anforderungen zum [Erstellen eines Flight-Pakets](create-a-flight.md) und zum [Abrufen von Flight-Paketen für eine App](get-flights-for-an-app.md) enthalten. Für einen Flug, der im Partner Center erstellt wurde, ist diese ID auch in der URL für die Seite "aktiv" im Partner Center verfügbar.  |
| submissionId | String | Erforderlich. Die ID der zu aktualisierenden Übermittlung. Diese ID ist in den Antwortdaten für Anforderungen zum [Erstellen einer Flight-Paket-Übermittlung](create-a-flight-submission.md) verfügbar. Für eine Eingabe, die im Partner Center erstellt wurde, ist diese ID auch in die URL für die Seite für die Auftragsübermittlung im Partner Center verfügbar.  |


### <a name="request-body"></a>Anforderungstext

Der Anforderungstext hat folgende Parameter.

| Wert      | Typ   | Beschreibung                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| flightPackages           | array  | Enthält Objekte, die Details über die einzelnen Pakete der Übermittlung bereitstellen. Weitere Informationen zu den Werten im Antworttext finden Sie unter [Flight-Paketressource](manage-flight-submissions.md#flight-package-object). Beim Aufruf dieser Methode zur Aktualisierung einer App-Übermittlung sind im Antworttext nur die *fileName*-, *fileStatus*-, *minimumDirectXVersion*- und *minimumSystemRam*-Werte dieser Objekte erforderlich. Die anderen Werte werden vom Partner Center aufgefüllt. |
| packageDeliveryOptions    | object  | Enthält den schrittweisen Paketrollout sowie erforderliche Einstellungen für die Übermittlung. Weitere Informationen finden Sie unter [options-Objekt für die Paketübermittlung](manage-flight-submissions.md#package-delivery-options-object).  |
| targetPublishMode           | String  | Der Veröffentlichungsmodus für die Übermittlung. Folgende Werte sind möglich: <ul><li>Immediate</li><li>Manuell</li><li>SpecificDate</li></ul> |
| targetPublishDate           | String  | Das Veröffentlichungsdatum der Übermittlung im ISO 8601-Format, wenn *TargetPublishMode* den Wert SpecificDate hat.  |
| notesForCertification           | String  |  Enthält zusätzliche Informationen für Zertifizierungstester wie Anmeldeinformationen für Testkonten und Schritte zum Zugriff auf und zur Überprüfung von Features. Weitere Informationen finden Sie unter [Hinweise zur Zertifizierung](https://docs.microsoft.com/windows/uwp/publish/notes-for-certification). |


### <a name="request-example"></a>Anforderungsbeispiel

Im folgenden Beispiel wird die Aktualisierung einer Flight-Paketübermittlung für eine App veranschaulicht.

```json
PUT https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/flights/43e448df-97c9-4a43-a0bc-2a445e736bcd/submissions/1152921504621243649 HTTP/1.1
Authorization: Bearer <your access token>
Content-Type: application/json
{
  "flightPackages": [
    {
      "fileName": "newPackage.appx",
      "fileStatus": "PendingUpload",
      "minimumDirectXVersion": "None",
      "minimumSystemRam": "None"
    }
  ],
  "packageDeliveryOptions": {
    "packageRollout": {
        "isPackageRollout": false,
        "packageRolloutPercentage": 0.0,
        "packageRolloutStatus": "PackageRolloutNotStarted",
        "fallbackSubmissionId": "0"
    },
    "isMandatoryUpdate": false,
    "mandatoryUpdateEffectiveDate": "1601-01-01T00:00:00.0000000Z"
  },
  "targetPublishMode": "Immediate",
  "targetPublishDate": "",
  "notesForCertification": "No special steps are required for certification of this app."
}
```

## <a name="response"></a>Antwort

Das folgende Beispiel veranschaulicht den JSON-Antworttext für einen erfolgreichen Aufruf dieser Methode. Der Antworttext enthält Informationen zur aktualisierten Übermittlung. Weitere Informationen zu den Werten im Antworttext finden Sie unter [Flight-Paketressource](manage-flight-submissions.md#flight-submission-object).

```json
{
  "id": "1152921504621243649",
  "flightId": "cd2e368a-0da5-4026-9f34-0e7934bc6f23",
  "status": "PendingCommit",
  "statusDetails": {
    "errors": [],
    "warnings": [],
    "certificationReports": []
  },
  "flightPackages": [
    {
      "fileName": "newPackage.appx",
      "fileStatus": "PendingUpload",
      "id": "",
      "version": "1.0.0.0",
      "languages": ["en-us"],
      "capabilities": [],
      "minimumDirectXVersion": "None",
      "minimumSystemRam": "None"
    }
  ],
  "packageDeliveryOptions": {
    "packageRollout": {
        "isPackageRollout": false,
        "packageRolloutPercentage": 0.0,
        "packageRolloutStatus": "PackageRolloutNotStarted",
        "fallbackSubmissionId": "0"
    },
    "isMandatoryUpdate": false,
    "mandatoryUpdateEffectiveDate": "1601-01-01T00:00:00.0000000Z"
  },
  "fileUploadUrl": "https://productingestionbin1.blob.core.windows.net/ingestion/8b389577-5d5e-4cbe-a744-1ff2e97a9eb8?sv=2014-02-14&sr=b&sig=wgMCQPjPDkuuxNLkeG35rfHaMToebCxBNMPw7WABdXU%3D&se=2016-06-17T21:29:44Z&sp=rwl",
  "targetPublishMode": "Immediate",
  "targetPublishDate": "",
  "notesForCertification": "No special steps are required for certification of this app."
}
```

## <a name="error-codes"></a>Fehlercodes

Wenn die Anforderung nicht erfolgreich abgeschlossen werden kann, enthält die Antwort einen der folgenden HTTP-Fehlercodes.

| Fehlercode |  Beschreibung   |
|--------|------------------|
| 400  | Die Flight-Paketübermittlung konnte nicht aktualisiert werden, da die Anforderung ungültig ist. |
| 409  | Die Paket-Flight-Übermittlung konnte aufgrund des aktuellen Status der app nicht aktualisiert werden, oder die app verwendet ein Partner Center-Feature, das [derzeit nicht durch die Übermittlung zum Microsoft Store-API unterstützt](create-and-manage-submissions-using-windows-store-services.md#not_supported). |   


## <a name="related-topics"></a>Verwandte Themen

* [Erstellen und Verwalten von Übermittlungen, die mithilfe von Microsoft Store services](create-and-manage-submissions-using-windows-store-services.md)
* [Verwalten von Paket-Flight-Übermittlungen](manage-flight-submissions.md)
* [Erhalten Sie eine Paket-Flight-Eingabe](get-a-flight-submission.md)
* [Erstellen Sie eine Paket-Flight-Eingabe](create-a-flight-submission.md)
* [Übernehmen Sie eine Paket-Flight-Eingabe](commit-a-flight-submission.md)
* [Löschen Sie eine Paket-Flight-Eingabe](delete-a-flight-submission.md)
* [Abrufen des Status einer Paket-Flight-Übermittlung](get-status-for-a-flight-submission.md)
