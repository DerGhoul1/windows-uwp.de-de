---
title: PointOfService-Gerätefunktion
description: Die PointOfService-Gerätefunktion wird für die Verwendung des Windows.Devices.PointOfService-Namespace benötigt.
ms.date: 05/02/2018
ms.topic: article
keywords: Windows 10, UWP, Point Of Service, POS
ms.localizationpriority: medium
ms.openlocfilehash: f120e093ab65224ca4c32b64640100b6abd1a36a
ms.sourcegitcommit: 0301f794f994e604ffc131de7e40ffcede3530c9
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/08/2019
ms.locfileid: "72036261"
---
# <a name="pointofservice-device-capability"></a>PointOfService-Gerätefunktion
Sie fordern Zugriff auf die PointOfService-APIs an, indem Sie die Funktion in Ihrem Anwendungspaketmanifest deklarieren. Sie können die meisten Funktionen mithilfe des Manifest-Designers in Microsoft Visual Studio deklarieren, oder Sie können sie manuell hinzufügen.  

> [!Important]
> Sie erhalten die Fehlermeldung **System. UnauthorizedAccessException** , wenn Sie versuchen, eine API im Namespace Windows. Devices. pointfservice zu verwenden, wenn Sie die Funktion **pointfservice** nicht im Anwendungs Manifest deklarieren. 

## <a name="declare-capability-using-manifest-designer"></a>Deklarieren von Funktionen mithilfe des Manifest-Designers

1. Erweitern Sie im **Projektmappen-Explorer** den Projektknoten Ihrer UWP-Anwendung.
2. Doppelklicken Sie auf die Datei **Package.appxmanifest**.  
*Wenn die Manifest-Datei bereits in der XML-Code Ansicht geöffnet ist, werden Sie von Visual Studio aufgefordert, die Datei zu schließen.*
3. Öffnen Sie die Registerkarte **Funktionen**.
4. Klicken Sie auf das Kontrollkästchen neben **Point of Service** in der Liste der Funktionen, um die Funktionalität des Point-of-Service-Geräts zu aktivieren.


## <a name="declare-capability-manually"></a>Manuelles Deklarieren eines Funktion

1. Erweitern Sie im **Projektmappen-Explorer** den Projektknoten Ihrer UWP-Anwendung.
2. Klicken Sie mit der rechten Maustaste auf die Datei **Package.appxmanifest**, und wählen Sie die Option **Code anzeigen** aus.
3. Fügen Sie das PointOfService DeviceCapability-Element zum Abschnitt mit den Funktionen Ihres Anwendungsmanifests hinzu.  

```xml
  <Capabilities>
    <DeviceCapability Name="pointOfService" />
  </Capabilities>
   ```
