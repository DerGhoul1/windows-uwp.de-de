---
Description: Reagieren Sie in Ihren Apps auf Mauseingaben, indem Sie die gleichen einfachen Zeigerereignisse behandeln, die Sie für Touch- und Stifteingaben verwenden.
title: Mausinteraktionen
ms.assetid: C8A158EF-70A9-4BA2-A270-7D08125700AC
label: Mouse
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 2d6ddf03541e94f89d0950a4f4c03eebaa0e396e
ms.sourcegitcommit: 87fd0ec1e706a460832b67f936a3014f0877a88c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/12/2020
ms.locfileid: "83234476"
---
# <a name="mouse-interactions"></a>Mausinteraktionen

Optimieren Sie Ihren Windows-App-Entwurf für die Fingereingabe, und erhalten Sie standardmäßig grundlegende Mausunterstützung. 

![Maus](images/input-patterns/input-mouse.jpg)

Die Mauseingabe eignet sich am besten für Benutzerinteraktionen, die Präzision beim Zeigen und Klicken erfordern. Die Benutzeroberfläche von Windows unterstützt diese Präzision, auch wenn sie für die ungenauere Toucheingabe optimiert wurde.

Die Maus- und Toucheingabe unterscheiden sich dahingehend, dass bei der Toucheingabe die direkte Manipulation von UI-Elementen auf dem Bildschirm durch physische Gesten für diese Objekte (z. B. Streifen, Ziehen, Drehen usw.) emuliert werden kann. Manipulationen mit der Maus erfordern in der Regel einigen UI-Aufwand, wie z. B. die Verwendung von Handles für das Anpassen der Größe oder Drehen eines Objekts.

In diesem Thema werden Designüberlegungen für Mausinteraktionen behandelt.

## <a name="the-uwp-app-mouse-language"></a>Die UWP-App-Sprache für Mauseingaben

Ein kompakter Satz von Mausinteraktionen wird durchgängig im ganzen System verwendet.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Begriff</th>
<th align="left">BESCHREIBUNG</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>Lernen durch Zeigen</p></td>
<td align="left"><p>Zeigen Sie auf ein Element, um weitere Informationen oder visuelle Hinweise (wie etwa QuickInfos) aufzurufen, ohne eine Aktion auszuführen.</p></td>
</tr>
<tr class="even">
<td align="left"><p>Linksklick, um primäre Aktion auszuführen</p></td>
<td align="left"><p>Klicken Sie mit der linken Maustaste auf ein Element, um dessen primäre Aktion aufzurufen (z. B. das Starten einer App oder das Ausführen eines Befehls).</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Bildlauf, um Ansicht zu ändern</p></td>
<td align="left"><p>Zeigen Sie Bildlaufleisten an, damit Benutzer in einem Inhaltsbereich nach oben, unten, links und rechts navigieren können. Benutzer können durch Klicken auf Bildlaufleisten oder Drehen des Mausrads einen Bildlauf durchführen. Auf Bildlaufleisten kann die Position der aktuellen Ansicht innerhalb des Inhaltsbereichs angezeigt werden (durch das Schwenken bei der Fingereingabe wird eine ähnliche Benutzeroberfläche angezeigt).</p></td>
</tr>
<tr class="even">
<td align="left"><p>Rechtsklick, um Auswahl zu treffen und Befehl auszuwählen</p></td>
<td align="left"><p>Klicken Sie mit der rechten Maustaste, um die Navigationsleiste (sofern verfügbar) und die App-Leiste mit globalen Befehlen anzuzeigen. Klicken Sie mit der rechten Maustaste auf ein Element, um es auszuwählen und die App-Leiste mit Kontextbefehlen für das ausgewählte Element anzuzeigen.</p>
<div class="alert">
<strong>Hinweis</strong>    Klicken Sie mit der rechten Maustaste, um ein Kontextmenü anzuzeigen, wenn Auswahl-oder App-leisten-Befehle keine passenden UI-Verhalten darstellen Wir empfehlen jedoch ausdrücklich, die App-Leiste für alle Befehlsverhalten zu verwenden.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>Benutzeroberflächenbefehle zum Zoomen</p></td>
<td align="left"><p>Zeigen Sie Benutzeroberflächenbefehle auf der App-Leiste an (z. B. "+" und "-"), oder drücken Sie STRG und drehen Sie das Mausrad, um Zusammendrück- und Aufziehbewegungen zum Zoomen zu emulieren.</p></td>
</tr>
<tr class="even">
<td align="left"><p>Benutzeroberflächenbefehle zum Drehen</p></td>
<td align="left"><p>Zeigen Sie Benutzeroberflächenbefehle auf der App-Leiste an, oder drücken Sie STRG+UMSCHALTTASTE, und drehen Sie das Mausrad, um die Drehbewegung zum Drehen zu emulieren. Drehen Sie das Gerät selbst, um den ganzen Bildschirm zu drehen.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Linksklick und ziehen, um neu anzuordnen</p></td>
<td align="left"><p>Klicken Sie mit der linken Maustaste auf ein Element, und ziehen Sie, um es zu bewegen.</p></td>
</tr>
<tr class="even">
<td align="left"><p>Linksklick und ziehen, um Text auszuwählen</p></td>
<td align="left"><p>Klicken Sie mit der linken Maustaste auf auswählbaren Text, und ziehen Sie, um Text auszuwählen. Doppelklicken Sie, um ein Wort auszuwählen.</p></td>
</tr>
</tbody>
</table>

## <a name="mouse-input-events"></a>Mauseingabe Ereignisse

Die meisten Maus Eingaben können durch die allgemeinen Routing Eingabeereignisse behandelt werden, die von allen [**UIElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement) -Objekten unterstützt werden. Dazu gehören:

- [**Bringingeviewangeforderten**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.bringintoviewrequested)
- [**Charakteristik empfangen**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.characterreceived)
- [**Contextabgeb Rochen**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.contextcanceled)
- [**Contextrequessiert**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.contextrequested)
- [**DoubleTapped**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.doubletapped)
- [**DragEnter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.dragenter)
- [**DragLeave**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.dragleave)
- [**DragOver**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.dragover)
- [**DragStart**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.dragstarting)
- [**Dropdown**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.drop)
- [**Dropdown**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.dropcompleted)
- [**Gettingfocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.gettingfocus)
- [**GotFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.gotfocus)
- [**Holding**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.holding)
- [**KeyDown**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keydown)
- [**KeyUp**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keyup)
- [**Losingfocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.losingfocus)
- [**LostFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.lostfocus)
- [**ManipulationCompleted**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.manipulationcompleted)
- [**ManipulationDelta**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.manipulationdelta)
- [**ManipulationInertiaStarting**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.manipulationinertiastarting)
- [**ManipulationStarted**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.manipulationstarted)
- [**ManipulationStarting**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.manipulationstarting)
- [**Nofocus candidatefound**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.nofocuscandidatefound)
- [**PointerCanceled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointercanceled)
- [**PointerCaptureLost**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointercapturelost)
- [**PointerEntered**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerentered)
- [**PointerExited**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerexited)
- [**PointerMoved**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointermoved)
- [**PointerPressed**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerpressed)
- [**PointerReleased**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerreleased)
- [**PointerWheelChanged**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerwheelchanged)
- [**PreviewKeyDown**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.previewkeydown)
- [**PreviewKeyUp**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.previewkeyup)
- [**RightTapped**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.righttapped)
- [**Tapped**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.tapped)

Sie können jedoch die spezifischen Funktionen der einzelnen Geräte (z. b. Mausrad Ereignisse) mithilfe der Zeiger-, Gesten-und Bearbeitungs Ereignisse in [Windows. UI. Input](https://docs.microsoft.com/uwp/api/windows.ui.input)nutzen.

**Beispiele:** Weitere Informationen finden Sie in unserem [basicinput-Beispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput)für.

## <a name="guidelines-for-visual-feedback"></a>Richtlinien für visuelles Feedback

- Wenn eine Maus erkannt wird (durch Bewegungs- oder Daraufzeigen-Ereignisse), zeigen Sie eine für Mausinteraktionen spezifische Benutzeroberfläche an, um auf vom Element verfügbar gemachte Funktionen hinzuweisen. Wenn die Maus für eine bestimmte Zeit nicht bewegt wird oder der Benutzer eine Fingereingabeinteraktion auslöst, blenden Sie die für Mausinteraktionen spezifische Benutzeroberfläche schrittweise aus. Somit bleibt die Benutzeroberfläche sauber und aufgeräumt.
- Verwenden Sie nicht den Cursor für Zeigefeedback, das Feedback des Elements reicht aus (siehe Cursor unten).
- Lassen Sie kein visuelles Feedback anzeigen, wenn ein Element keine Interaktionen unterstützt (z. B. statischer Text).
- Verwenden Sie keine Fokusrechtecke für Mausinteraktionen. Diese sind ausschließlich für Tastaturinteraktionen vorgesehen.
- Zeigen Sie für alle Elemente, die das gleiche Eingabeziel darstellen, das gleiche visuelle Feedback an.
- Stellen Sie Schaltflächen (z. B. „+“ und „-“) zur Verfügung, um fingereingabebasierte Manipulationen wie etwa Schwenken, Drehen, Zoomen usw. zu emulieren.

Allgemeine Informationen zum visuellen Feedback finden Sie unter [Richtlinien für visuelles Feedback](guidelines-for-visualfeedback.md).

## <a name="cursors"></a>Cursor

Für einen Mauszeiger ist eine Reihe von Standardcursor verfügbar. Diese Cursor werden verwendet, um die primäre Aktion eines Elements anzugeben.

Jedem Standardcursor ist ein entsprechendes Standardbild zugewiesen. Benutzer einer App können das einem Standardcursor zugewiesene Standardbild jederzeit ändern. Geben Sie über die [**PointerCursor**](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.pointercursor)-Funktion die Abbildung eines Cursors an.

Beachten Sie beim Anpassen des Mauszeigers Folgendes:

- Verwenden Sie immer den Pfeilcursor (![Pfeilcursor](images/cursor-arrow.png)) für klickbare Elemente. Verwenden Sie den Zeigefingercursor (![Zeigefingercursor](images/cursor-pointinghand.png)) nicht für Links oder andere interaktive Elemente. Verwenden Sie stattdessen Zeigeeffekte (wie bereits beschrieben).
- Verwenden Sie den Textcursor (![Textcursor](images/cursor-text.png)) für auswählbaren Text.
- Verwenden Sie den Bewegungscursor (![Bewegungscursor](images/cursor-move.png)), wenn die primäre Aktion eine Bewegung ist (etwa beim Ziehen oder Zuschneiden). Verwenden Sie den Bewegungscursor nicht, wenn die primäre Aktion eine Navigationsaktion ist (etwa bei Start-Kacheln).
- Verwenden Sie die Cursor für horizontale, vertikale und diagonale Größenänderung (![Cursor für vertikale Größenänderung](images/cursor-vertical.png), ![Cursor für horizontale Größenänderung](images/cursor-horizontal.png), ![Cursor für diagonale Größenänderung (unten links, oben rechts)](images/cursor-diagonal2.png), ![Cursor für diagonale Größenänderung (oben links, unten rechts)](images/cursor-diagonal1.png)), wenn die Größe eines Objekts geändert werden kann.
- Verwenden Sie den Handcursor (![Handcursor (offen)](images/cursor-pan1.png), ![Handcursor (geschlossen)](images/cursor-pan2.png)) beim Verschieben von Inhalt innerhalb einer Canvas (etwa bei einer Karte).

## <a name="related-articles"></a>Verwandte Artikel

- [Behandeln von Zeigereingaben](handle-pointer-input.md)
- [Identifizieren von Eingabegeräten](identify-input-devices.md)
- [Übersicht über Ereignisse und Routingereignisse](https://docs.microsoft.com/windows/uwp/xaml-platform/events-and-routed-events-overview)

### <a name="samples"></a>Beispiele

- [Einfaches Eingabebeispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput)
- [Eingabebeispiel mit geringer Latenz](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LowLatencyInput)
- [Beispiel für den Benutzerinteraktionsmodus](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/UserInteractionMode)
- [Beispiel für visuelle Fokuselemente](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals)
