---
title: Tiefenschablonenansicht (Depth Stencil View, DSV)
description: Eine Tiefenschablonenansicht stellt das Format und den Puffer zur Speicherung von Tiefen- und Schabloneninformationen bereit.
ms.assetid: 2E8BFF7F-76F8-408E-B8E6-A71B9DF46281
keywords:
- Tiefenschablonenansicht (Depth Stencil View, DSV)
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: eee30882373513bd352777aff0ae6399f047bbec
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2018
ms.locfileid: "1044149"
---
# <a name="depth-stencil-view-dsv"></a>Tiefenschablonenansicht (Depth Stencil View, DSV)


Eine Tiefenschablonenansicht stellt das Format und den Puffer zur Speicherung von Tiefen- und Schabloneninformationen bereit. Der Tiefenpuffer dient dazu, das Zeichnen von Pixeln zu vermeiden, die von einem näher liegenden Objekt verdeckt werden und so für den Betrachter nicht sichtbar wären. Der Schablonenpuffer kann zur Aussortierung (Culling) aller Zeichnungsaktivitäten außerhalb einer definierten Form genutzt werden.

Abgesehen von der Definition eines bestimmten Renderingbereiches gibt es noch eine Reihe erweiterter Einsatzmöglichkeiten für Schablonenpuffer. Schablonenpufferwerte können so verändert werden, dass Effekte wie Ausblenden, Silhouetten, Decaling, Auflösen, Umrisse, Schattenvolumen, usw. möglich werden.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Ansichten](views.md)

 

 



