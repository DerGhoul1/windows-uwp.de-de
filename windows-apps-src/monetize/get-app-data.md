---
ms.assetid: 8D4AE532-22EF-4743-9555-A828B24B8F16
description: Verwenden Sie diese Methoden in der Microsoft Store Übermittlungs-API zum Abrufen von Daten für apps, die bei Ihrem Partner Center-Konto registriert sind.
title: Abrufen von App-Daten
ms.date: 02/28/2018
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store-Übermittlungs-API, App-Daten
ms.localizationpriority: medium
ms.openlocfilehash: cfbe8df46f51b41ccdd840f609caf2c593735e1f
ms.sourcegitcommit: ca1b5c3ab905ebc6a5b597145a762e2c170a0d1c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/13/2020
ms.locfileid: "79210976"
---
# <a name="get-app-data"></a>Abrufen von App-Daten

Verwenden Sie die folgenden Methoden in der Microsoft Store Übermittlungs-API, um Daten für vorhandene apps in Ihrem Partner Center-Konto zu erhalten. Eine Einführung in die Microsoft Store-Übermittlungs-API einschließlich der Voraussetzungen für die Verwendung der API finden Sie unter [Erstellen und Verwalten von Übermittlungen mit Microsoft Store-Diensten](create-and-manage-submissions-using-windows-store-services.md).

Bevor Sie diese Methoden verwenden können, muss die APP bereits in Ihrem Partner Center-Konto vorhanden sein. Informationen zum Erstellen oder Verwalten von Übermittlungen für Apps finden Sie unter den Methoden in [Verwalten von App-Übermittlungen](manage-app-submissions.md).

| Methode | URI                                                                                             | Beschreibung                                                 |
|------- |------------------------------------------------------------------------------------------------ |------------------------------------------------------------ |
| GET    | `https://manage.devcenter.microsoft.com/v1.0/my/applications`                                   | [Daten für alle Ihre apps erhalten](get-all-apps.md)               |
| GET    | `https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}`                   | [Daten für eine bestimmte APP erhalten](get-an-app.md)                |
| GET    | `https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/listinappproducts` | [Add-ons für eine APP](get-add-ons-for-an-app.md)         |
| GET    | `https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/listflights`       | [Get Package Flights for an App](get-flights-for-an-app.md) |

## <a name="prerequisites"></a>Erforderliche Komponenten

Falls noch nicht geschehen, sorgen Sie vor der Verwendung dieser Methoden dafür, dass alle [Voraussetzungen](create-and-manage-submissions-using-windows-store-services.md#prerequisites) für die Microsoft Store-Übermittlungs-API erfüllt sind.

## <a name="data-resources"></a>Datenressourcen

Die Microsoft Store-Übermittlungs-API-Methoden für das Abrufen von App-Daten verwenden die folgenden JSON-Datenressourcen.

<span id="application_object" />

### <a name="application-resource"></a>Anwendungsressource

Diese Ressource steht für eine App, die in Ihrem Konto registriert ist.

```json
{
  "id": "9NBLGGH4R315",
  "primaryName": "ApiTestApp",
  "packageFamilyName": "30481DevCenterAPITester.ApiTestAppForDevbox_ng6try80pwt52",
  "packageIdentityName": "30481DevCenterAPITester.ApiTestAppForDevbox",
  "publisherName": "CN=…",
  "firstPublishedDate": "1601-01-01T00:00:00Z",
  "lastPublishedApplicationSubmission": {
    "id": "1152921504621086517",
    "resourceLocation": "applications/9NBLGGH4R315/submissions/1152921504621086517"
  },
  "pendingApplicationSubmission": {
    "id": "1152921504621243487",
    "resourceLocation": "applications/9NBLGGH4R315/submissions/1152921504621243487"
  },
  "hasAdvancedListingPermission": true
}
```

Die Ressource hat die folgenden Werte.

| Wert           | Typ    | Beschreibung       |
|-----------------|---------|---------------------|
| id            | string  | Die Store-ID der App. Weitere Informationen zur Store-ID finden Sie unter [Anzeigen von Details zur App-Identität](https://docs.microsoft.com/windows/uwp/publish/view-app-identity-details).   |
| primaryName   | string  | Der Primärname der App.      |
| packageFamilyName | string  | Der Paketfamilienname der App.      |
| packageIdentityName          | string  | Die Paketidentität der App.                       |
| publisherName       | string  | Die Windows-Herausgeber-ID, die mit der App verknüpft ist. Dies entspricht dem Wert für " **Package/Identity/Publisher** ", der auf der Seite " [App-Identität](https://docs.microsoft.com/windows/uwp/publish/view-app-identity-details) " für die APP im Partner Center angezeigt wird.       |
| firstPublishedDate      | string  | Das Datum, an dem die App erstmals im Format ISO 8601 veröffentlicht wurde.   |
| lastPublishedApplicationSubmission       | object | Eine [Übermittlungsressource](#submission_object) mit Informationen über die letzte veröffentlichte Übermittlung für die App.    |
| pendingApplicationSubmission        | object  |  Eine [Übermittlungsressource](#submission_object) mit Informationen über die aktuelle ausstehende Übermittlung für die App.   |   
| hasAdvancedListingPermission        | boolean  |  Gibt an, ob Sie die [gamingOptions](manage-app-submissions.md#gaming-options-object) oder [trailers](manage-app-submissions.md#trailer-object) für Übermittlungen für die App konfigurieren können. Dieser Wert gilt für Übermittlungen, die später als Mai 2017 erstellt wurden. |  |


<span id="add-on-object" />

### <a name="add-on-resource"></a>Add-On-Ressource

Diese Ressource enthält Informationen zu einem Add-On.

```json
{
    "inAppProductId": "9WZDNCRD7DLK"
}
```

Die Ressource hat die folgenden Werte.

| Wert           | Typ    | Beschreibung         |
|-----------------|---------|----------------------|
| inAppProductId            | string  | Die Store-ID des Add-Ons. Dieser Wert wird vom Store bereitgestellt. Beispiel für eine Store-ID: 9NBLGGH4TNMP.   |


<span id="flight-object" />

### <a name="flight-resource"></a>Flight-Ressource

Diese Ressource enthält Informationen zu einem Flight-Paket für eine App.

```json
{
    "flightId": "7bfc11d5-f710-47c5-8a98-e04bb5aad310",
    "friendlyName": "myflight",
    "lastPublishedFlightSubmission": {
        "id": "1152921504621086517",
        "resourceLocation": "flights/7bfc11d5-f710-47c5-8a98-e04bb5aad310/submissions/1152921504621086517"
    },
    "pendingFlightSubmission": {
        "id": "1152921504621215786",
        "resourceLocation": "flights/7bfc11d5-f710-47c5-8a98-e04bb5aad310/submissions/1152921504621215786"
    },
    "groupIds": [
        "1152921504606962205"
    ],
    "rankHigherThan": "Non-flighted submission"
}
```

Die Ressource hat die folgenden Werte.

| Wert           | Typ    | Beschreibung           |
|-----------------|---------|------------------------|
| flightId            | string  | Die ID für das Flight-Paket. Dieser Wert wird von Partner Center bereitgestellt.  |
| friendlyName           | string  | Der Name des Flight-Pakets nach Vorgabe des Entwicklers.   |
| lastPublishedFlightSubmission       | object | Eine [Übermittlungsressource](#submission_object) mit Informationen über die letzte veröffentlichte Übermittlung für das Flight-Paket.   |
| pendingFlightSubmission        | object  |  Eine [Übermittlungsressource](#submission_object) mit Informationen über die aktuelle ausstehende Übermittlung für das Flight-Paket.  |    
| groupIds           | array  | Ein Array von Zeichenfolgen, die die IDs der Test-Flight-Gruppen enthalten, die dem Flight-Paket zugeordnet sind. Weitere Informationen zu Test-Flight-Gruppen finden Sie unter [Flight-Pakete](https://docs.microsoft.com/windows/uwp/publish/package-flights).   |
| rankHigherThan           | string  | Der Anzeigename des Flight-Pakets, das den unmittelbar niedrigeren Rang als das aktuelle Flight-Paket erhält. Weitere Informationen zur Bewertung von Test-Flight-Gruppen finden Sie unter [Flight-Pakete](https://docs.microsoft.com/windows/uwp/publish/package-flights).  |


<span id="submission_object" />

### <a name="submission-resource"></a>Übermittlungsressource

Diese Ressource enthält Informationen zu einer Übermittlung. Das folgende Beispiel veranschaulicht das Format der Ressource.

```json
{
  "pendingApplicationSubmission": {
    "id": "1152921504621243487",
    "resourceLocation": "applications/9WZDNCRD9MMD/submissions/1152921504621243487"
  }
}
```

Die Ressource hat die folgenden Werte.

| Wert              | Typ   | Beschreibung               |
|--------------------|--------|---------------------------|
| id                 | string | Die ID der Übermittlung. |
| resourceLocation   | string | Ein relativer Pfad, den Sie an den Basisanforderungs-URI ```https://manage.devcenter.microsoft.com/v1.0/my/``` anfügen können, um die vollständigen Daten für die Übermittlung abzurufen. |

 
## <a name="related-topics"></a>Verwandte Themen

* [Erstellen und Verwalten von Übermittlungen mithilfe von Microsoft Store Services](create-and-manage-submissions-using-windows-store-services.md)
* [Verwalten von Microsoft Store App](manage-app-submissions.md)
* [Alle apps erhalten](get-all-apps.md)
* [APP erhalten](get-an-app.md)
* [Add-ons für eine APP](get-add-ons-for-an-app.md)
* [Get Package Flights for an App](get-flights-for-an-app.md)
