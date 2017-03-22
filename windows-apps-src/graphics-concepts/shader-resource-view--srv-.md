---
title: Shaderressourcenansicht (SRV) und Unsortierte Zugriffsansicht (UAV)
description: "Shaderressourcenansichten verpacken Texturen in der Regel in einem Format, sodass die Shader darauf zugreifen können. Eine unsortierte Zugriffsansicht bietet ähnlichen Funktionen, ermöglicht aber das Lesen und Schreiben der Textur (oder einer anderen Ressource) in beliebiger Reihenfolge."
ms.assetid: 4505BCD2-0EDA-40F2-887C-EC081FE32E8F
keywords:
- Shaderressourcenansicht (SRV)
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 4ce8e7a5d17656545cf047c2ab21cda5c5fbb4f0
ms.lasthandoff: 02/07/2017

---

# <a name="shader-resource-view-srv-and-unordered-access-view-uav"></a>Shaderressourcenansicht (SRV) und Unsortierte Zugriffsansicht (UAV)


Shaderressourcenansichten verpacken Texturen in der Regel in einem Format, sodass die Shader darauf zugreifen können. Eine unsortierte Zugriffsansicht bietet ähnlichen Funktionen, ermöglicht aber das Lesen und Schreiben der Textur (oder einer anderen Ressource) in beliebiger Reihenfolge.

Das Verpacken einer einzelnen Textur ist wahrscheinlich die einfachste Form der Shaderressourcenansicht. Komplexere Beispiele wären eine Sammlung von Unterressourcen (einzelne Arrays, Ebenen oder Farben aus einer Mipmap-Textur), 3D-Texturen, 1D-Texturfarbverläufe usw.

Unsortierte Zugriffsansichten sind in Bezug auf die Leistung etwas aufwändiger, ermöglichen aber z. B., dass in eine Textur geschrieben und diese gleichzeitig gelesen wird. Dadurch kann die aktualisierte Textur von der Grafikpipeline zu einem anderen Zweck wiederverwendet werden. Shaderressourcenansichten sind nur zum Lesen vorgesehen (das ist die gängigste Verwendung von Ressourcen).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Ansichten](views.md)

 

 




