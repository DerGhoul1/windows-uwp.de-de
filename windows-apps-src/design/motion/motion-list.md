---
Description: Mit Listenanimationen können Sie einzelne oder mehrere Elemente in einer Sammlung wie z. B. einem Fotoalbum oder einer Liste mit Suchergebnissen einfügen oder entfernen.
title: Hinzufügen und Löschen von Animationen
ms.assetid: A85006AE-4992-457a-B514-500B8BEF5DC8
label: Motion--add and delete animations
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 4264a9a3a75c076fc033bb98dad45ff8dc99f2ef
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970285"
---
# <a name="add-and-delete-animations"></a>Hinzufügen und Löschen von Animationen



Mit Listenanimationen können Sie einzelne oder mehrere Elemente in einer Sammlung wie z. B. einem Fotoalbum oder einer Liste mit Suchergebnissen einfügen oder entfernen.

> **Wichtige APIs**: [ **adddeletedermetransition-Klasse**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.adddeletethemetransition)


## <a name="dos-and-donts"></a>Empfohlene und nicht empfohlene Vorgehensweisen


-   Verwenden Sie Listenanimationen, um einer bestehenden Menge von Elementen ein einzelnes neues Element hinzuzufügen. Verwenden Sie diese Animationen beispielsweise, wenn eine neue E-Mail eingeht oder ein neues Foto in eine vorhandene Serie importiert wird.
-   Verwenden Sie Listenanimationen, um einer bestehenden Menge von Elementen mehrere Elemente gleichzeitig hinzuzufügen. Verwenden Sie diese Animationen beispielsweise, wenn Sie eine neue Fotoserie in eine vorhandene Sammlung importieren. Wenn mehrere Elemente hinzugefügt oder gelöscht werden, sollte der Vorgang für alle Elemente gleichzeitig erfolgen, ohne dass es zwischen den Aktionen für die einzelnen Objekte zu Verzögerungen kommt.
-   Verwenden Sie Listenanimationen für das Hinzufügen und Löschen als Paar. Verwenden Sie immer, wenn Sie eine dieser Animationen verwenden, die zugehörige Animation für die entgegengesetzte Aktion.
-   Verwenden Sie Listenanimationen mit einer Liste von Elementen, in der Sie ein Element oder eine Gruppe von Elementen gleichzeitig hinzufügen oder löschen können.
-   Verwenden Sie Listenanimationen nicht, um einen Container anzuzeigen oder zu entfernen. Diese Animationen sind für Elemente einer bereits angezeigten Auflistung oder Menge vorgesehen. Verwenden Sie Popupanimationen, um einen kurzlebigen Container auf der App-Oberfläche ein- oder auszublenden. Verwenden Sie Inhaltsübergangsanimationen, um einen Container, der Teil der App-Oberfläche ist, anzuzeigen oder zu ersetzen.
-   Verwenden Sie Listenanimationen nicht für eine vollständige Menge von Elementen. Verwenden Sie die Inhaltsübergangsanimationen, um eine vollständige Auflistung innerhalb des Containers hinzuzufügen oder zu entfernen.



## <a name="related-articles"></a>Verwandte Artikel

* [Übersicht über Animationen](https://docs.microsoft.com/windows/uwp/graphics/animations-overview)
* [Animieren von Hinzufügungen und Löschungen in Listen](https://docs.microsoft.com/previous-versions/windows/apps/jj649430(v=win.10))
* [Schnellstart: Animieren der Benutzeroberfläche mithilfe von Bibliotheksanimationen](https://docs.microsoft.com/previous-versions/windows/apps/hh452703(v=win.10))
* [**AddDeleteThemeTransition-Klasse**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.adddeletethemetransition)

 

 




