---
author: WilliamsJason
title: Geräteportal – API-Referenz für Fiddler
description: Erfahren Sie, wie Sie die Fiddler-Ablaufverfolgung programmgesteuert aktivieren/deaktivieren.
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.assetid: e7d4225e-ac2c-41dc-aca7-9b1a95ec590b
ms.localizationpriority: medium
ms.openlocfilehash: 819f039f04d1e0a7fd035b10e3cbe408645e8f4d
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2018
ms.locfileid: "409946"
---
# <a name="fiddler-settings-api-reference"></a>Fiddler-Einstellungen – API-Referenz   
Sie können die Fiddler-Netzwerkablaufverfolgung für Ihr Dev Kit mittels dieser REST-API aktivieren und deaktivieren.

## <a name="determine-if-fiddler-tracing-is-enabled"></a>Bestimmen Sie, ob Fiddler Tracing aktiviert ist

**Anforderung**

Sie können überprüfen, um festzustellen, ob auf dem Gerät mithilfe der folgenden Anforderung Fiddler Tracing aktiviert ist.

Methode      | Anforderungs-URI
:------     | :-----
GET | /ext/fiddler
<br />
**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**   

- Keine

**Antwort**   

- JSON Bool-Eigenschaft IsProxyEnabled welche Bezeichner gibt an, ob der Proxy aktiviert ist oder nicht.

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

HTTP-Statuscode      | Beschreibung
:------     | :-----
200 | Erfolg
4XX | Fehlercodes
5XX | Fehlercodes

## <a name="enable-fiddler-tracing"></a>Aktivieren der Fiddler-Ablaufverfolgung

**Anforderung**

Sie können die Fiddler-Ablaufverfolgung mittels der folgenden Anforderung für das Dev Kit verwenden.  Beachten Sie, dass das Gerät neu gestartet werden muss, bevor dies wirksam wird.

Methode      | Anforderungs-URI
:------     | :-----
POST | /ext/fiddler
<br />
**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

| URI-Parameter      | Beschreibung     | 
| ------------------ |-----------------|
| proxyAddress       | Die IP-Adresse oder der Hostnamen des Geräts, auf dem Fiddler ausgeführt wird. |
| proxyPort          | Der Port, den Fiddler für die Überwachung des Datenverkehrs verwendet. Der Standardport ist 8888. |
| updateCert (optional)| Ein boolescher Wert, der angibt, ob das Fiddler-Stammzertifikat angegeben wird. Dieser Wert muss true sein, wenn Fiddler nie zuvor für dieses Dev Kit oder für einen anderen Host konfiguriert wurde.  |
<br>

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keiner, wenn updateCert false ist oder nicht angegeben wird. Andernfalls mehrteiliger konformer http-Text, der die Datei FiddlerRoot.cer enthält.

**Antwort**   

- Keine  

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

HTTP-Statuscode      | Beschreibung
:------     | :-----
204 | Die Anforderung für die Aktivierung von Fiddler wurde akzeptiert. Fiddler wird aktiviert, wenn das Gerät das nächste Mal neu gestartet wird.
4XX | Fehlercodes
5XX | Fehlercodes

## <a name="disable-fiddler-tracing-on-the-devkit"></a>Deaktivieren Sie die Fiddler-Ablaufverfolgung für das Dev Kit.

**Anforderung**

Sie können die Fiddler-Ablaufverfolgung mithilfe der folgenden Anforderung für das Gerät deaktivieren. Beachten Sie, dass das Gerät neu gestartet werden muss, bevor dies wirksam wird.

Methode      | Anforderungs-URI
:------     | :-----
DELETE | /ext/fiddler
<br />
**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**   

- Keine

**Antwort**   

- Keine 

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

HTTP-Statuscode      | Beschreibung
:------     | :-----
204 | Die Anforderung zum Deaktivieren der Fiddler-Ablaufverfolgung war erfolgreich. Die Nachverfolgung wird deaktiviert, wenn das Gerät das nächste Mal neu gestartet wird.
4XX | Fehlercodes
5XX | Fehlercodes

<br />
**Verfügbare Gerätefamilien**

* Windows Xbox

## <a name="see-also"></a>Siehe auch
- [Konfigurieren von Fiddler für UWP auf Xbox](uwp-fiddler.md)
