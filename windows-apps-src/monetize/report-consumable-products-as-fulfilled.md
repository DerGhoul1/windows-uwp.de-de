---
ms.assetid: E9BEB2D2-155F-45F6-95F8-6B36C3E81649
description: Verwenden Sie diese Methode aus der Microsoft Store Collection-API, um den Kauf eines Verbrauchsprodukt für einen bestimmten Kunden als abgewickelt zu melden. Damit ein Benutzer ein Verbrauchsprodukt erneut erwerben kann, muss Ihre App oder Ihr Dienst das Verbrauchsprodukt für den betreffenden Benutzer als abgewickelt melden.
title: Melden von konsumierbaren Produkten als erfüllt
ms.date: 03/19/2018
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store-Sammlungs-API, erfüllen, Verbrauchsprodukt
ms.localizationpriority: medium
ms.openlocfilehash: 994113abc34a0a5f7905bff00aa77c6785409927
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372775"
---
# <a name="report-consumable-products-as-fulfilled"></a>Melden von konsumierbaren Produkten als erfüllt

Verwenden Sie diese Methode aus der Microsoft Store Collection-API, um den Kauf eines Verbrauchsprodukt für einen bestimmten Kunden als abgewickelt zu melden. Damit ein Benutzer ein Verbrauchsprodukt erneut erwerben kann, muss Ihre App oder Ihr Dienst das Verbrauchsprodukt für den betreffenden Benutzer als abgewickelt melden.

Sie können diese Methode auf zwei Weisen verwenden, um ein Verbrauchsprodukt als erfüllt zu melden:

* Geben Sie die (im **itemId**-Parameter einer [Produktabfrage](query-for-products.md)) zurückgegebene) Artikelkennung des Verbrauchsprodukts und eine von Ihnen bereitgestellte eindeutige Tracking-ID an. Wenn die gleiche Tracking-ID für mehrere Versuche verwendet wird, wird auch dann das gleiche Ergebnis zurückgegeben, wenn der Artikel bereits in Anspruch genommen wurde. Wenn Sie nicht sicher sind, ob eine Verbrauchsanforderung erfolgreich war, sollte Ihr Dienst Verbrauchsanforderungen mit derselben Tracking-ID erneut übermitteln. Die Tracking-ID ist immer mit der jeweiligen Verbrauchsanforderung verknüpft und kann beliebig oft erneut übermittelt werden.
* Geben Sie die (im **productId**-Parameter einer [Produktabfrage](query-for-products.md) zurückgegebene) Produkt-ID und eine Transaktions-ID an, die aus einer der Quellen abgerufen wird, die in der Beschreibung für den **transactionId**-Parameter im Abschnitt „Anforderungstext“ unten aufgeführt sind.

## <a name="prerequisites"></a>Vorraussetzungen


Zur Verwendung dieser Methode benötigen Sie:

* Ein Azure AD-Zugriffstoken, das mit dem Zielgruppen-URI `https://onestore.microsoft.com` erstellt wurde.
* Ein Microsoft Store-ID-Schlüssel, der die Identität des Benutzers darstellt, für den Sie ein Verbrauchsprodukt als erfüllt melden möchten.

Weitere Informationen finden Sie unter [Verwalten von Produktansprüchen aus einem Dienst](view-and-grant-products-from-a-service.md).

## <a name="request"></a>Anforderung


### <a name="request-syntax"></a>Anforderungssyntax

| Methode | Anforderungs-URI                                                   |
|--------|---------------------------------------------------------------|
| POST   | ```https://collections.mp.microsoft.com/v6.0/collections/consume``` |


### <a name="request-header"></a>Anforderungsheader

| Header         | Typ   | Beschreibung                                                                                           |
|----------------|--------|-------------------------------------------------------------------------------------------------------|
| Autorisierung  | String | Erforderlich. Die Azure AD-Zugriffstoken in der Form **Bearer** &lt; *token*&gt;.                           |
| Host           | String | Muss auf den Wert **collections.mp.microsoft.com** festgelegt werden.                                            |
| Content-Length | number | Die Länge des Anforderungstexts.                                                                       |
| Content-Type   | String | Gibt den Anforderungs- und Antworttyp an. Derzeit wird als einziger Wert **application/json** unterstützt. |


### <a name="request-body"></a>Anforderungstext

| Parameter     | Typ         | Beschreibung         | Erforderlich |
|---------------|--------------|---------------------|----------|
| beneficiary   | UserIdentity | Der Benutzer, für den dieser Artikel genutzt wird. Weitere Informationen finden Sie in der folgenden Tabelle.        | Ja      |
| itemId        | String       | Der Wert *itemId*, der von einer [Produktanfrage](query-for-products.md) zurückgegeben wird. Verwenden Sie diesen Parameter mit *trackingId*.      | Nein       |
| trackingId    | guid         | Eine eindeutige, vom Entwickler angegebene Tracking-ID. Verwenden Sie diesen Parameter mit *itemId*.         | Nein       |
| productId     | String       | Der Wert *productId*, den eine [Produktabfrage](query-for-products.md) zurückgibt. Verwenden Sie diesen Parameter mit *transactionId*.   | Nein       |
| transactionId | guid         | Ein Transaktions-ID-Wert, der aus einer der folgenden Quellen abgerufen wird. Verwenden Sie diesen Parameter mit *productId*.<ul><li>Die [TransactionID](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.purchaseresults.transactionid)-Eigenschaft der [PurchaseResults](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.PurchaseResults)-Klasse.</li><li>Der von [RequestProductPurchaseAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync), [RequestAppPurchaseAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.requestapppurchaseasync) oder [GetAppReceiptAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.getappreceiptasync) zurückgegebene App- oder Produktbeleg.</li><li>Der Parameter *transactionId*, der von einer [Produktabfrage](query-for-products.md) zurückgegeben wird.</li></ul>   | Nein       |


Das UserIdentity-Objekt enthält die folgenden Parameter.

| Parameter            | Typ   | Beschreibung       | Erforderlich |
|----------------------|--------|-------------------|----------|
| identityType         | String | Gibt den Zeichenfolgenwert **b2b** an.    | Ja      |
| identityValue        | String | Der [Microsoft Store-ID-Schlüssel](view-and-grant-products-from-a-service.md#step-4), der die Identität des Benutzers darstellt, für den Sie ein Verbrauchsprodukt als erfüllt melden möchten.      | Ja      |
| localTicketReference | String | Der angeforderte Bezeichner für die zurückgegebene Antwort. Es wird empfohlen, die Sie verwenden den gleichen Wert wie die *"UserID"* [Anspruch](view-and-grant-products-from-a-service.md#claims-in-a-microsoft-store-id-key) in den Microsoft Store-ID-Schlüssel. | Ja      |


### <a name="request-examples"></a>Anforderungsbeispiele

Im folgenden Beispiel werden *itemId* und *trackingId* verwendet.

```syntax
POST https://collections.mp.microsoft.com/v6.0/collections/consume HTTP/1.1
Authorization: Bearer eyJ0eXAiOiJKV1…..
Host: collections.mp.microsoft.com
Content-Length: 2050
Content-Type: application/json

{
    "beneficiary": {
        "localTicketReference": "testreference",
        "identityValue": "eyJ0eXAiOi…..",
        "identityType": "b2b"
    },
    "itemId": "44c26106-4979-457b-af34-609ae97a084f",
    "trackingId": "44db79ca-e31d-49e9-8896-fa5c7f892b40"
}
```

Im folgenden Beispiel werden *productId* und *transactionId* verwendet.

```syntax
POST https://collections.mp.microsoft.com/v6.0/collections/consume HTTP/1.1
Authorization: Bearer eyJ0eXAiOiJKV1……
Content-Length: 1880
Content-Type: application/json
Host: collections.md.mp.microsoft.com

{
    "beneficiary" : {
        "localTicketReference" : "testReference",
        "identityValue" : "eyJ0eXAiOiJ…..",
        "identitytype" : "b2b"
    },
    "productId" : "9NBLGGH5WVP6",
    "transactionId" : "08a14c7c-1892-49fc-9135-190ca4f10490"
}
```


## <a name="response"></a>Antwort

Bei erfolgreicher Nutzung wird kein Inhalt zurückgegeben.

### <a name="response-example"></a>Antwortbeispiel

```syntax
HTTP/1.1 204 No Content
Content-Length: 0
MS-CorrelationId: 386f733d-bc66-4bf9-9b6f-a1ad417f97f0
MS-RequestId: e488cd0a-9fb6-4c2c-bb77-e5100d3c15b1
MS-CV: 5.1
MS-ServerId: 030011326
Date: Tue, 22 Sep 2015 20:40:55 GMT
```

## <a name="error-codes"></a>Fehlercodes


| Code | Fehler        | Interner Fehlercode           | Beschreibung           |
|------|--------------|----------------------------|-----------------------|
| 401  | Nicht autorisiert | AuthenticationTokenInvalid | Das Azure AD-Zugriffstoken ist ungültig. In einigen Fällen enthalten die Details zu ServiceError weitere Informationen, z. B. wenn das Token abgelaufen ist oder der *appid*-Anspruch fehlt. |
| 401  | Nicht autorisiert | PartnerAadTicketRequired   | An den Dienst wurde im Autorisierungsheader kein Azure AD-Zugriffstoken übergeben.                                                                                                   |
| 401  | Nicht autorisiert | InconsistentClientId       | Der *clientId*-Anspruch im Microsoft Store-ID-Schlüssel im Anforderungstext und der *appid*-Anspruch im Azure AD-Zugriffstoken im Autorisierungsheader stimmen nicht überein.                     |

<span/> 

## <a name="related-topics"></a>Verwandte Themen

* [Verwalten von Product-Berechtigungen von einem Dienst](view-and-grant-products-from-a-service.md)
* [Abfrage von Produkten](query-for-products.md)
* [Gewähren Sie kostenlose Produkte](grant-free-products.md)
* [Erneuern Sie einen Microsoft Store-ID-Schlüssel](renew-a-windows-store-id-key.md)
