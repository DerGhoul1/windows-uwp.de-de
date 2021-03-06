---
description: Verwenden Sie diese Methode in der Microsoft Store-Analyse-API, um Xbox Live Erfolgsdaten abzurufen.
title: Abrufen von Xbox Live-Erfolgsdaten
ms.date: 06/04/2018
ms.topic: article
keywords: Windows 10, Uwp, Store-Diensten, Microsoft Store-Analyse-API, Xbox Live-Analyse, Erfolge
ms.localizationpriority: medium
ms.openlocfilehash: 422024445be4662aab0a47b5527369c8b7091446
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/21/2019
ms.locfileid: "67317767"
---
# <a name="get-xbox-live-achievements-data"></a>Abrufen von Xbox Live-Erfolgsdaten

Verwenden Sie diese Methode in der Microsoft Store-Analyse-API, um die Anzahl der Kunden zu erhalten, die jeden Erfolg für Ihr [Xbox Live-fähiges Spiel](https://docs.microsoft.com/gaming/xbox-live/index.md) freigeschaltet haben, und zwar während des letzten Tages, für den Erfolgsdaten verfügbar sind, für die vorherigen 30 Tage vor diesem Tag und über die gesamte Lebensdauer des Spiels bis zu diesem Tag. Diese Informationen sind auch verfügbar in der [Xbox-Analysebericht](../publish/xbox-analytics-report.md) im Partner Center.

> [!IMPORTANT]
> Diese Methode unterstützt nur Spiele für Xbox oder Spiele, die Xbox Live-Dienste verwenden. Diese Spiele müssen den [Konzeptgenehmigungsprozess](../gaming/concept-approval.md) durchlaufen, der Spiele umfasst, die von [Microsoft-Partnern](https://docs.microsoft.com/gaming/xbox-live/developer-program-overview.md#microsoft-partners) veröffentlicht wurden, sowie Spiele, die über das [ID@Xbox-Programm](https://docs.microsoft.com/gaming/xbox-live/developer-program-overview.md#id) übermittelt wurden. Diese Methode unterstützt derzeit keine Spiele, die über das [Xbox Live Creators-Programm](https://docs.microsoft.com/gaming/xbox-live/get-started-with-creators/get-started-with-xbox-live-creators.md) eingereicht wurden.

## <a name="prerequisites"></a>Vorraussetzungen

Zur Verwendung dieser Methode sind folgende Schritte erforderlich:

* Falls noch nicht geschehen, erfüllen Sie alle [Voraussetzungen](access-analytics-data-using-windows-store-services.md#prerequisites) für die Microsoft Store-Analyse-API.
* [Rufen Sie ein Azure AD-Zugriffstoken ab](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token), das im Anforderungsheader für diese Methode verwendet wird. Nach Erhalt eines Zugriffstokens können Sie es 60 Minuten lang verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.

## <a name="request"></a>Anforderung


### <a name="request-syntax"></a>Anforderungssyntax

| Methode | Anforderungs-URI       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics``` |


### <a name="request-header"></a>Anforderungsheader

| Header        | Typ   | Beschreibung                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Autorisierung | String | Erforderlich. Die Azure AD-Zugriffstoken in der Form **Bearer** &lt; *token*&gt;. |


### <a name="request-parameters"></a>Anforderungsparameter


| Parameter        | Typ   |  Beschreibung      |  Erforderlich  
|---------------|--------|---------------|------|
| applicationId | String | Die [Store-ID](in-app-purchases-and-trials.md#store-ids) des Spiels, für das Sie die Xbox Live Erfolgsdaten abrufen möchten.  |  Ja  |
| metricType | String | Eine Zeichenfolge, die den Typ der abzurufenden Xbox Live-Analysedaten angibt. Geben Sie für diese Methode den Wert **achievements** an.  |  Ja  |
| top | ssNoversion | Die Anzahl der Datenzeilen, die in der Anforderung zurückgegeben werden sollen. Der Maximal- und Standardwert ist 10.000, wenn nicht anders angegeben. Sind in der Abfrage keine weiteren Zeilen, enthält der Antworttext den Link „Weiter“, über den Sie die nächste Seite mit Daten anfordern können. |  Nein  |
| skip | ssNoversion | Die Anzahl der Zeilen, die in der Abfrage übersprungen werden sollen. Verwenden Sie diesen Parameter, um große Datensätze durchzublättern. Beispielsweise rufen „top=10000“ und „skip=0“ die ersten 10.000 Datenzeilen ab, „top=10000“ und „skip=10000“ die nächsten 10.000 Datenzeilen usw. |  Nein  |


### <a name="request-example"></a>Anforderungsbeispiel

Das folgende Beispiel zeigt eine Anforderung zum Abrufen von Erfolgsdaten für Kunden, die die ersten 10 Erfolge für Ihr Xbox Live-fähiges Spiel entsperrt haben. Ersetzen Sie den *applicationId*-Wert durch die Store-ID Ihres Spiels.


```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics?applicationId=9NBLGGGZ5QDR&metrictype=achievements&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Antwort

| Wert      | Typ   | Beschreibung                  |
|------------|--------|-------------------------------------------------------|
| Wert      | array  | Ein Array von Objekten, die Daten für jeden Erfolg in Ihrem Spiel enthalten. Weitere Informationen zu den Daten in den einzelnen Objekten finden Sie in der folgenden Tabelle.                                                                                                                      |
| @nextLink  | String | Wenn weitere Seiten mit Daten vorhanden sind, enthält diese Zeichenfolge einen URI, mit dem Sie die nächste Seite mit Daten anfordern können. Beispielsweise wird dieser Wert zurückgegeben, wenn der Parameter **top** der Anforderung auf 100 festgelegt ist, es jedoch mehr als 100 Zeilen mit Daten für die Abfrage gibt. |
| TotalCount | ssNoversion    | Die Gesamtzahl der Zeilen im Datenergebnis für die Abfrage.  |


Elemente im Array *Value* enthalten die folgenden Werte.

| Wert               | Typ   | Beschreibung                           |
|---------------------|--------|-------------------------------------------|
| applicationId       | String | Die Store-ID des Spiels, für das Sie Erfolgsdaten abrufen.     |
| reportDateTime     | String |  Das Datum für die Erfolgsdaten.    |
| achievementId          | number |  Die ID des Erfolges. |
| achievementName           | String | Der Name des Erfolges.  |
| gamerscore           | number |  Die Gamerscore-Prämie für den Erfolg.  |
| dailyUnlocks           | number |  Die Anzahl der Kunden, die den Erfolg für den Tag entsperrt haben, der durch *reportDateTime* angegeben wird.  |
| monthlyUnlocks              | number |  Die Anzahl der Kunden, die den Erfolg in den 30 Tagen vor dem Tag entsperrt haben, der durch *reportDateTime* angegeben wird.   |
| totalUnlocks | number |  Die Anzahl der Kunden, die den Erfolg während der Lebensdauer des Spiels bis zu dem Tag entsperrt haben, der durch *reportDateTime* angegeben wird.   |


### <a name="response-example"></a>Antwortbeispiel

Das folgende Beispiel zeigt ein Beispiel für einen JSON-Antworttext für diese Anforderung.

```json
{
  "Value": [
    {
      "applicationId": "9NBLGGGZ5QDR",
      "reportDateTime": "2018-01-30T00:00:00",
      "achievementId": 6,
      "achievementName": "Yoink!",
      "gamerscore": 10,
      "dailyUnlocks": 0,
      "monthlyUnlocks": 10310,
      "totalUnlocks": 1215360
    },
    {
      "applicationId": "9NBLGGGZ5QDR",
      "reportDateTime": "2018-01-30T00:00:00",
      "achievementId": 7,
      "achievementName": "Ding!",
      "gamerscore": 10,
      "dailyUnlocks": 0,
      "monthlyUnlocks": 10897,
      "totalUnlocks": 1282524
    },
    {
      "applicationId": "9NBLGGGZ5QDR",
      "reportDateTime": "2018-01-30T00:00:00",
      "achievementId": 8,
      "achievementName": "End of the Beginning",
      "gamerscore": 30,
      "dailyUnlocks": 0,
      "monthlyUnlocks": 9848,
      "totalUnlocks": 1105074
    }
  ],
  "@nextLink": null,
  "TotalCount": 3
}
```

## <a name="related-topics"></a>Verwandte Themen

* [Access-Analytics-Daten mithilfe von Microsoft Store services](access-analytics-data-using-windows-store-services.md)
* [Abrufen von Xbox Live-Analytics-Daten](get-xbox-live-analytics.md)
* [Abrufen von Xbox Live-Health-Daten](get-xbox-live-health-data.md)
* [Abrufen von Xbox Live-Spiel Hub-Daten](get-xbox-live-game-hub-data.md)
* [Abrufen von Xbox Live-Club-Daten](get-xbox-live-club-data.md)
* [Abrufen von Xbox Live Multiplayer-Daten](get-xbox-live-multiplayer-data.md)
