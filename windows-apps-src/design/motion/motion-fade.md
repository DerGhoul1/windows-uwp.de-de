---
author: mijacobs
Description: Use fade animations to bring items into a view or to take items out of a view. The two common fade animations are fade-in and fade-out.
title: Ein-und Ausblendungsanimationen in UWP-Apps
ms.assetid: 975E5EE3-EFBE-4159-8D10-3C94143DD07F
label: Motion--fades
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 211e0b6dc2c6e2872ca20d4711ef6d41d4974e5c
ms.sourcegitcommit: 0ab8f6fac53a6811f977ddc24de039c46c9db0ad
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/15/2018
ms.locfileid: "1652459"
---
# <a name="fade-animations"></a>Ein- und Ausblendungsanimationen



Verwenden Sie Ein- und Ausblendungsanimationen, um Elemente anzuzeigen oder nicht anzuzeigen. Die beiden üblichen Animationen dieser Art sind das Einblenden und das Ausblenden.

> **Wichtige APIs**: [**FadeInThemeAnimation class**](https://msdn.microsoft.com/library/windows/apps/br210298), [**FadeOutThemeAnimation class**](https://msdn.microsoft.com/library/windows/apps/br210302)


## <a name="dos-and-donts"></a>Empfohlene und nicht empfohlene Vorgehensweisen


-   Wenn die App zwischen nicht verbundenen oder textlastigen Elementen wechselt, sollten Sie eine Ausblendungsanimation gefolgt von einer Einblendungsanimation verwenden. Auf diese Weise kann das ausgehende Objekt vollständig ausgeblendet werden, bevor das eingehende Objekt sichtbar wird.
-   Blenden Sie die eingehenden Elemente über den ausgehenden Elementen ein, wenn die Größe der Elemente konstant bleibt und die Benutzer den Eindruck haben sollen, das gleiche Element zu sehen. Sobald die Einblendung abgeschlossen ist, kann das ausgehende Element entfernt werden. Diese Vorgehensweise ist nur dann geeignet, wenn das ausgehende Element vom eingehenden Element vollständig verdeckt wird.
-   Vermeiden Sie es, dass bei Ein- und Ausblendungsanimationen Elemente einer Liste hinzugefügt oder daraus gelöscht werden sollen. Verwenden Sie stattdessen die zu diesem Zweck erstellten Listenanimationen.
-   Verwenden Sie Ein- und Ausblendungsanimationen möglichst nicht, um den gesamten Inhalt einer Seite zu ändern. Verwenden Sie stattdessen die zu diesem Zweck erstellten Seitenübergangsanimationen.
-   Ausblenden ist eine gute Möglichkeit, um ein Element zu entfernen.
## <a name="related-articles"></a>Verwandte Artikel

* [Übersicht über Animationen](https://msdn.microsoft.com/library/windows/apps/mt187350)
* [Animieren von Ein- und Ausblendungen](https://msdn.microsoft.com/library/windows/apps/xaml/jj649429)
* [Schnellstart: Animieren der Benutzeroberfläche mithilfe von Bibliotheksanimationen](https://msdn.microsoft.com/library/windows/apps/xaml/hh452703)
* [**FadeInThemeAnimation-Klasse**](https://msdn.microsoft.com/library/windows/apps/br210298)
* [**FadeOutThemeAnimation-Klasse**](https://msdn.microsoft.com/library/windows/apps/br210302)

 

 



