---
title: Punktelisten
description: "Eine Punkteliste ist eine Sammlung von Scheitelpunkten, die als isolierte Punkte gerendert werden. Die Anwendung kann Punktelisten in 3D-Szenen für Sternenfelder oder gepunktete Linien auf der Oberfläche eines Polygons verwenden."
ms.assetid: 332954AE-019F-4A91-B773-E3A7C92F3297
keywords:
- Punktelisten
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: da7fd4dfd4835e39a76daddfa8855415f0e3234b
ms.lasthandoff: 02/07/2017

---

# <a name="point-lists"></a>Punktelisten


Eine Punkteliste ist eine Sammlung von Scheitelpunkten, die als isolierte Punkte gerendert werden. Die Anwendung kann Punktelisten in 3D-Szenen für Sternenfelder oder gepunktete Linien auf der Oberfläche eines Polygons verwenden.

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>Beispiel


Die folgende Abbildung zeigt eine gerenderte Punkteliste.

![Abbildung einer Punkteliste](images/pointlst.png)

Die Anwendung kann Materialien und Texturen auf eine Punkteliste anwenden. Die Farben im Material oder der Textur werden nur bei dem gezeichneten Punkt angezeigt, nicht an anderen Stellen zwischen den Punkten.

Der folgende Code zeigt, wie Scheitelpunkte für diese Punkteliste erstellt werden.

```
struct CUSTOMVERTEX
{
    float x,y,z;
};

CUSTOMVERTEX Vertices[] = 
{
    {-5.0, -5.0, 0.0},
    { 0.0,  5.0, 0.0},
    { 5.0, -5.0, 0.0},
    {10.0,  5.0, 0.0},
    {15.0, -5.0, 0.0},
    {20.0,  5.0, 0.0}
};
```

Im folgenden Codebeispiel wird veranschaulicht, wie Sie diese Punkteliste in Direct3D rendern.

```
//
// It is assumed that d3dDevice is a valid
// pointer to an IDirect3DDevice interface.
//
d3dDevice->DrawPrimitive( D3DPT_POINTLIST, 0, 6 );
```

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Grundtypen](primitives.md)

 

 




