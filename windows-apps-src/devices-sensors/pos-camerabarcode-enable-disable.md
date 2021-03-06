---
title: Kamera-Strichcodescannerkonfiguration
description: Aktivieren oder Deaktivieren des Kamera-Strichcodescanners
ms.date: 04/08/2019
ms.topic: article
keywords: Windows 10, UWP, Point Of Service, POS
ms.localizationpriority: medium
ms.openlocfilehash: 8394f79e9581101d6def0f1568cb000ffdee5987
ms.sourcegitcommit: 0dec04de501a3db6b22dfd4a320fc09b5c4a21b5
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70243314"
---
# <a name="enable-or-disable-the-software-decoder-that-ships-with-windows"></a>Enthält Informationen zum aktivieren oder deaktivieren der Standard-Software-Decoder, der mit im Lieferumfang von Windows enthalten ist

In Windows 10, Version 1803, ist der Software-Decoder installiert und standardmäßig aktiviert.  Sie können den im Lieferumfang von Windows enthaltenen Software-Decoder deaktivieren, wenn Sie den Kamera-Strichcodescanner nicht wünschen oder einen Decoder von einem Drittanbieter haben, der mit Windows.Devices.PointOfService.BarcodeScanner-APIs verwendet werden kann und nicht beide verwenden möchten.

## <a name="enable-or-disable-using-the-system-registry"></a>Aktivieren Sie oder deaktivieren Sie die Verwendung der Registrierung

Der Softwaredecoder, der im Lieferumfang von Windows enthalten ist, kann über die Registrierung durch Hinzufügen des Registrierungsschlüssels *InboxDecoder* unter *HKLM\Software\ Microsoft \PointOfService\ BarcodeScanner* aktiviert oder deaktiviert werden, indem Sie den Wert *Aktivieren* wie folgt festlegen.

| Wertname  | Werttyp | Wert | Status |
| ----------- | --------- | -------|--------|
| Aktivieren      | DWORD     | 1 (Standard)<br/>0 |  Enthält Informationen zum aktivieren des Standard-Software-Decoders, der im Lieferumfang von Windows enthalten ist <br/> Enthält Informationen zum deaktivieren des Standard-Software-Decoders, der im Lieferumfang von Windows enthalten ist |

Hier ist eine Beispiel-Registrierungsdatei, die Sie zum **deaktivieren** des Softwaredecoders verwenden können, der im Lieferumfang von Windows enthalten ist:

```text
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\PointOfService\BarcodeScanner\InboxDecoder]
"Enable"=dword:0000000
```  

Verwenden Sie die Beispiel-Registrierungsdatei zum **aktivieren** des Softwaredecoders, der im Lieferumfang von Windows enthalten ist:

```text
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\PointOfService\BarcodeScanner\InboxDecoder]
"Enable"=dword:0000001
```  

> [!Warning]
> Wird die Registrierung falsch angepasst, können schwerwiegende Probleme auftreten.  Für den zusätzlichen Schutz, sichern Sie die Registrierung, bevor Sie diese ändern.  Sie können dann die Registrierung wiederherstellen, falls ein Problem auftritt.  Weitere Informationen zur Sicherung und zum Wiederherstellen der Registrierung erhalten Sie in der nachstehende Artikelnummer des Microsoft Knowledge Base-Artikels: <br/><br/> [322756](https://support.microsoft.com/help/322756/how-to-back-up-and-restore-the-registry-in-windows) Sichern und Wiederherstellen der Registrierung in Windows.

> [!NOTE]
> Der in Windows 10 integrierte Softwaredecoder stammt von [**Digimarc Corporation**](https://www.digimarc.com/).

## <a name="see-also"></a>Siehe auch

### <a name="samples"></a>Proben

- [Beispiel für Barcode Scanner](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner)
