---
title: Glanzlichtzuordnungen
description: Wenn glänzende Objekte aus stark reflektierenden Materialien von einer Lichtquelle beleuchtet werden, erhalten sie Glanzlichter.
ms.assetid: 9B3AC5EC-DDAA-4671-BC81-0E3C79D87A81
keywords:
- Glanzlichtzuordnungen
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 6da5292396a334e50c61fb4638334aae27581d7d
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2018
ms.locfileid: "1043109"
---
# <a name="specular-light-maps"></a>Glanzlichtzuordnungen


Glänzende Objekte aus hochreflektieRendern Materialien erhalten durch das Beleuchten mit einer Lichtquelle Glanzlichter. In einigen Fällen erhalten Sie exaktere Glanzlichter, wenn Sie Glanzlichtzuordnungen auf Grundtypen anwenden, anstatt die vom Beleuchtungsmodul erzeugten Glanzlichter zu verwenden.

Um die Glanzlichtzuordnung durchzuführen, fügen Sie der Textur des Grundtyps die Glanzlichtzuordnung hinzu und modulieren dann die RGB-Lichtzuordnung (multiplizieren Sie das Ergebnis mit der RGB-Lichtzuordnung).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Lichtzuordnung mit Texturen](light-mapping-with-textures.md)

 

 



