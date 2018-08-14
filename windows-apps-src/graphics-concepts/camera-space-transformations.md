---
title: Kameraraumtransformationen
description: Scheitelpunkte im Kameraraum werden berechnet, indem die Scheitelpunkte des Objekts mit der globalen Ansichtsmatrix transformiert werden.
ms.assetid: 86EDEB95-8348-4FAA-897F-25251B32B076
keywords:
- Kameraraumtransformationen
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 032518fe793db99309d098b28f2f8e94bbb072ba
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2018
ms.locfileid: "1043929"
---
# <a name="camera-space-transformations"></a>Kameraraumtransformationen


Scheitelpunkte im Kameraraum werden berechnet, indem die Scheitelpunkte des Objekts mit der globalen Ansichtsmatrix transformiert werden.

V = V \* wvMatrix

Vertexspezifische Normalen im Kameraraum werden berechnet, indem die Normalen des Objekts mit der inversen Umsetzung der globalen Ansichtsmatrix transformiert werden. Eine globale Ansichtsmatrix kann entweder orthogonal sein oder nicht.

N = N \* (wvMatrix⁻¹)<sup>T</sup>

Die Matrixinversion und der Matrixaustausch werden auf einer 4x4-Matrix ausgeführt. Die Multiplikation kombiniert den normalen Teil mit dem 3x3-Teil der resultierenden 4x4-Matrix.

Wenn der Renderstatus so festgelegt ist, dass die Normalen normalisiert werden, werden Scheitelnormalenvektoren wie folgt nach der Transformation auf den Kameraraum normalisiert:

N = norm(N)

Die Beleuchtungsposition im Kameraraum wird durch die Transformation der Position der Lichtquelle mit der Ansichtsmatrix berechnet.

Lₚ = Lₚ \* vMatrix

Die Richtung des Lichts im Kameraraum für gerichtetes Licht wird durch die Multiplikation der Richtung der Lichtquellen mit der Ansichtsmatrix berechnet, normalisiert und das Resultat wird negiert.

L<sub>dir</sub> = -norm(L<sub>dir</sub> \* wvMatrix)

Für ein punktuelles Licht und ein Spotlight wird die Richtung der Lichtquelle wie folgt berechnet:

L<sub>dir</sub> = norm(V \* Lₚ). Dabei werden die Parameter in der folgenden Tabelle definiert.

| Parameter       | Standardwert | Typ                                          | Beschreibung                                               |
|-----------------|---------------|-----------------------------------------------|-----------------------------------------------------------|
| L<sub>dir</sub> | Nicht zutreffend           | 3D-Vektor (X-, Y- und Z-Gleitkommawerte) | Richtungsvektor vom Objekt-Vertex bis zur Lichtquelle          |
| V               | Nicht zutreffend           | 3D-Vektor (X-, Y- und Z-Gleitkommawerte) | Vertexposition im Kameraraum                           |
| wvMatrix        | Identität      | 4x4-Matrix der Gleitkommawerte           | Zusammengesetzte Matrix mit globaler und Ansichtstransformation |
| N               | Nicht zutreffend           | 3D-Vektor (X-, Y- und Z-Gleitkommawerte) | Vertexnormale                                             |
| Lₚ              | Nicht zutreffend           | 3D-Vektor (X-, Y- und Z-Gleitkommawerte) | Position der Lichtquelle im Kameraraum                            |
| vMatrix         | Identität      | 4x4-Matrix der Gleitkommawerte           | Matrix mit Ansichtstransformation                      |

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Beleuchtungsmathematik](mathematics-of-lighting.md)

 

 



