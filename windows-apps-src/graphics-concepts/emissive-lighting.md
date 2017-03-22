---
title: Emissive-Lighting
description: "Beim Emissive-Lighting handelt es sich um eine Beleuchtung, die von einem Objekt ausgeht (z. B. ein Glühen)."
ms.assetid: 262EB9E2-DF96-401F-93D6-51A7BB60074B
keywords:
- Emissive-Lighting
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 50bc904e5810340846b3fd84ffca214d07d38750
ms.lasthandoff: 02/07/2017

---

# <a name="emissive-lighting"></a>Emissive-Lighting


Beim *Emissive-Lighting* handelt es sich um eine Beleuchtung, die von einem Objekt ausgeht (z. B. ein Glühen). Das Emissive-Lighting sorgt dafür, dass ein gerendertes Objekt leuchtet. Die Emissionen wirken sich auf die Farbe eines Objektes aus. Sie können beispielsweise ein dunkles Material heller und teilweise in der emittierten Farbe erscheinen lassen.

Das Emissive-lighting wird durch einen einzigen Faktor beschrieben.

Emissive-Lighting = Cₑ

Dabei gilt Folgendes:

| Parameter | Standardwert | Typ                                                                 | Beschreibung     |
|-----------|---------------|----------------------------------------------------------------------|-----------------|
| Cₑ        | (0,0,0,0)     | Rot, Grün, Blau und Alphatransparenz (alle Gleitkommawerte) | Emissionsfarbe. |

 

Der Wert für Cₑ ist entweder Farbe 1 oder Farbe 2. Wenn die Vertex-Farbe nicht angegeben wird, wird das Emissive-Materialfarbe verwendet.

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>Beispiel


In diesem Beispiel wird das Objekt über die Ambiente-Beleuchtung der Szene und die Ambiente-Farbe des Materials eingefärbt.

Entsprechend der Gleichung ist die resultierende Farbe der Objekt-Vertizes die Materialfarbe.

Die folgende Abbildung zeigt die Materialfarbe (Grün). Die Emissive-Beleuchtung beleuchtet alle Objekt-Vertizes mit derselben Farbe. Sie ist nicht von der Vertex-Normalen oder der Beleuchtungsrichtung abhängig. Daher sieht die Kugel wie ein 2D-Kreis aus – denn es gibt keine Schattierung für die Oberfläche des Objekts.

![Abbildung einer grünen Kugel](images/lighte.jpg)

Die folgende Abbildung zeigt, wie sich die Emissive-Beleuchtung mit den anderen drei Beleuchtungstypen mischt. Auf der rechten Seite der Kugel ist eine Mischung aus grüne Emissive- und roter Ambient-Beleuchtung sichtbar. Auf der linken Seite der Kugel mischt sich die grüne Emissive-Beleuchtung mit einer Diffuse-Beleuchtung und erzeugt so einen roten Farbverlauf. Das Glanzlicht in der Mitte ist Weiß. Es sorgt für einen gelben Ring, denn der Wert der Glanz-Beleuchtung fällt stark ab. Dies sorgt dafür, dass die Werte für die Ambiente-, Diffuse- und Emissions-Beleuchtung gemischt werden. So entsteht das Gelb.

![Abbildung einer grüne Kugel mit Emissive-Beleuchtung](images/lightadse.jpg)

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Beleuchtungsmathematik](mathematics-of-lighting.md)

 

 




