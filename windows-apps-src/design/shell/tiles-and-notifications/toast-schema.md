---
Description: Im folgenden Artikel werden alle Eigenschaften und Elemente im Popupinhalt beschrieben.
title: Popupinhaltsschema
ms.assetid: 7CBC3BD5-D9C3-4781-8BD0-1F28039E1FA8
label: Toast content schema
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 57ddb366964c259fccddc3f905c6a03a382d0f17
ms.sourcegitcommit: ca1b5c3ab905ebc6a5b597145a762e2c170a0d1c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/13/2020
ms.locfileid: "79210116"
---
# <a name="toast-content-schema"></a>Popupinhaltsschema

 

Im Folgenden werden alle Eigenschaften und Elemente im Popupinhalt beschrieben.

Wenn Sie anstelle der [Benachrichtigungsbibliothek](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) lieber unformatiertes XML verwenden, finden Sie im [XML-Schema](toast-xml-schema.md) weitere Informationen.

[Inhalts Inhalt](#toastcontent)
* [-Visualisierung](#toastvisual)
  * ["Deastbindinggeneric"](#toastbindinggeneric)
    * [Ideastbindinggenericchild](#itoastbindinggenericchild)
    * ["Ein"-Logo](#toastgenericapplogo)
    * [Verzeichnis Image](#toastgenericheroimage)
    * ["" "" "" "".](#toastgenericattributiontext)
* [Ideastactions](#itoastactions)
* [Ein-und ausgeschaltet](#toastaudio)
* [Verzeichnis Leser](#toastheader)


## <a name="toastcontent"></a>ToastContent
ToastContent ist das Objekt der obersten Ebene, das den Inhalt einer Benachrichtigung beschreibt, einschließlich der visuellen Elemente, Aktionen und Audio.

| Eigenschaft | Typ | Erforderlich | Beschreibung |
|---|---|---|---|
| **Gestartet**| string | false | Eine Zeichenfolge, die an die Anwendung übergeben wird, wenn sie durch das Popup aktiviert wird. Das Format und der Inhalt dieser Zeichenfolge werden von der App für eigene Zwecke definiert. Wenn der Benutzer zum Starten der zugeordneten App auf das Popup tippt oder klickt, stellt die Startzeichenfolge der App den Kontext bereit, um dem Benutzer eine Ansicht passend zum Popupinhalt zu ermöglichen, anstatt sie in der Standardgröße starten. |
| **Störungen** | [-Visualisierung](#toastvisual) | true | Beschreibt den visuellen Teil der Popupbenachrichtigung. |
| **Aktionen** | [Ideastactions](#itoastactions) | false | Erstellen Sie optional benutzerdefinierte Aktionen mit Schaltflächen und Eingaben. |
| **Audio** | [Ein-und ausgeschaltet](#toastaudio) | false | Beschreibt den Audioteil der Popupbenachrichtigung. |
| **ActivationType** | ["Deastactivationtype"](#toastactivationtype) | false | Gibt an, welche Art der Aktivierung verwendet wird, wenn der Benutzer auf den Text dieser Popupbenachrichtigung klickt. |
| **Activationoptions** | ["Deastactivationoptions"](#toastactivationoptions) | false | Neu im Creators Update: zusätzliche Optionen zur Aktivierung der Popupbenachrichtigung. |
| **Szenario** | [Ein-/ausgeschaltet](#toastscenario) | false | Gibt das Szenario an, für das Ihr Popup verwendet wird, wie ein Weckton oder eine Erinnerung. |
| **Display Timestamp** | DateTimeOffset? | false | Neu im Creators Update: Überschreiben Sie den Standardzeitstempel mit einem benutzerdefiniertem Zeitstempel, der anstelle des Zeitpunkts, an dem die Benachrichtigung bei der Windows-Plattform eingegangen ist, den Zeitpunkt der tatsächlichen Übermittlung des Benachrichtigungsinhalts angibt. |
| **Header** | [Verzeichnis Leser](#toastheader) | false | Neu im Creators Update: Fügen Sie Ihrer Benachrichtigung eine benutzerdefinierte Kopfzeile hinzu, um mehrere Benachrichtigungen im Info-Center zu gruppieren. |


### <a name="toastscenario"></a>ToastScenario
Gibt an, welches Szenario das Popup darstellt.

| Wert | Bedeutung |
|---|---|
| **Default (Standard)** | Das normale Popupverhalten. |
| **Erneut** | Eine Erinnerungsbenachrichtigung. Diese wird vorab erweitert angezeigt und verbleibt auf dem Bildschirm des Benutzers, bis sie geschlossen wird. |
| **Ruf** | Eine Wecktonbenachrichtigung. Diese wird vorab erweitert angezeigt und verbleibt auf dem Bildschirm des Benutzers, bis sie geschlossen wird. Es wird standardmäßig ein Weckton in einer Audioschleife wiedergegeben. |
| **IncomingCall** | Benachrichtigung über einen eingehenden Anruf. Diese wird in einem speziellen Anrufformat vorab erweitert angezeigt und verbleibt auf dem Bildschirm des Benutzers, bis sie geschlossen wird. Es wird standardmäßig ein Klingelton in einer Audioschleife wiedergegeben. |


## <a name="toastvisual"></a>ToastVisual
Der visuelle Teil von Popups enthält die Bindungen, die Text, Bilder, adaptiven Inhalt und vieles mehr enthalten.

| Eigenschaft | Typ | Erforderlich | Beschreibung |
|---|---|---|---|
| **Bindinggeneric** | ["Deastbindinggeneric"](#toastbindinggeneric) | true | Die allgemeine Popupbindung, die auf allen Geräten gerendert werden kann. Diese Bindung ist erforderlich und kann nicht null sein. |
| **BaseUri** | URI | false | Eine grundlegende Standard-URL, die mit relativen URLs in Bildquellattributen kombiniert wird. |
| **Addimagequery** | bool? | false | Legen Sie den Wert auf „Wahr“ fest, um an die in der Popupbenachrichtigung angegebene Bild-URL eine Abfragezeichenfolge anzufügen. Verwenden Sie dieses Attribut, wenn Ihr Server Bilder hostet und Abfragezeichenfolgen verarbeiten kann, indem er entweder basierend auf den Abfragezeichenfolgen eine Bildvariante abruft oder die Abfragezeichenfolge ignoriert und das Bild wie angegeben ohne die Abfragezeichenfolge zurückgibt. Diese Abfragezeichenfolge gibt Skalierung, Kontrasteinstellung und Sprache an. So wird beispielsweise ein in der Benachrichtigung angegebener Wert „www.website.com/images/hello.png“ zu „www.website.com/images/hello.png?ms-scale=100&ms-contrast=standard&ms-lang=en-us“ |
| **Sprache**| string | false | Das Zielgebietsschema der visuellen Nutzlast bei Verwendung lokalisierter Ressourcen, angegeben als BCP-47-Sprachtags wie z. B. „en-US“ oder „fr-FR“. Dieses Gebietsschema wird von jedem in der Bindung oder dem Text angegebenen Gebietsschema überschrieben. Falls nicht angegeben, wird stattdessen das Gebietsschema des Systems verwendet. |


## <a name="toastbindinggeneric"></a>ToastBindingGeneric
Die allgemeine Bindung ist die Standardbindung für Popups, und der Ort, an dem Sie den Text, Bilder, adaptiven Inhalt und mehr angeben.

| Eigenschaft | Typ | Erforderlich | Beschreibung |
|---|---|---|---|
| **Children** | IList<[IToastBindingGenericChild](#itoastbindinggenericchild)> | false | Der Inhalt des Texts des Popups, der Text, Bilder und Gruppen enthalten kann (im Anniversary Update hinzugefügt). Textelemente müssen vor allen anderen Elementen stehen, und nur 3 Textelemente werden unterstützt. Wenn ein Textelement nach einem anderen Element platziert wird, wird es nach oben verschoben oder gelöscht. Außerdem werden bestimmte Texteigenschaften wie HintStyle für die untergeordneten Stammtextelemente nicht unterstützt und können nur in einer AdaptiveSubgroup verwendet werden. Wenn Sie AdaptiveGroup auf Geräten ohne das Anniversary Update verwenden, wird der Gruppeninhalt einfach gelöscht. |
| **Applogooverride** | ["Ein"-Logo](#toastgenericapplogo) | false | Ein optionales Logo zum Überschreiben des App-Logos. |
| **Heroimage** | [Verzeichnis Image](#toastgenericheroimage) | false | Eine optionales ausgewähltes Favoritenbild, das auf dem Popup und im Info-Center angezeigt wird. |
| **Nung** | ["" "" "" "".](#toastgenericattributiontext) | false | Optionaler Zuordnungstext, der am unteren Rand der Popupbenachrichtigung angezeigt wird. |
| **BaseUri** | URI | false | Eine grundlegende Standard-URL, die mit relativen URLs in Bildquellattributen kombiniert wird. |
| **Addimagequery** | bool? | false | Legen Sie den Wert auf „Wahr“ fest, um an die in der Popupbenachrichtigung angegebene Bild-URL eine Abfragezeichenfolge anzufügen. Verwenden Sie dieses Attribut, wenn Ihr Server Bilder hostet und Abfragezeichenfolgen verarbeiten kann, indem er entweder basierend auf den Abfragezeichenfolgen eine Bildvariante abruft oder die Abfragezeichenfolge ignoriert und das Bild wie angegeben ohne die Abfragezeichenfolge zurückgibt. Diese Abfragezeichenfolge gibt Skalierung, Kontrasteinstellung und Sprache an. So wird beispielsweise ein in der Benachrichtigung angegebener Wert „www.website.com/images/hello.png“ zu „www.website.com/images/hello.png?ms-scale=100&ms-contrast=standard&ms-lang=en-us“ |
| **Sprache**| string | false | Das Zielgebietsschema der visuellen Nutzlast bei Verwendung lokalisierter Ressourcen, angegeben als BCP-47-Sprachtags wie z. B. „en-US“ oder „fr-FR“. Dieses Gebietsschema wird von jedem in der Bindung oder dem Text angegebenen Gebietsschema überschrieben. Falls nicht angegeben, wird stattdessen das Gebietsschema des Systems verwendet. |


## <a name="itoastbindinggenericchild"></a>IToastBindingGenericChild
Markierungsschnittstelle für untergeordnete Popupelemente, die Text, Bilder, Gruppen und mehr enthalten.

| Implementierungen |
| --- |
| [Adaptivetext](#adaptivetext) |
| [Adaptiveimage](#adaptiveimage) |
| [Adaptivegroup](#adaptivegroup) |
| [Adaptiveprogressbar](#adaptiveprogressbar) |


## <a name="adaptivetext"></a>AdaptiveText
Ein adaptives Textelement. Wenn in ToastBindingGeneric.Children der obersten Ebene platziert, wird nur HintMaxLines angewendet. Wenn dies als untergeordnetes Element einer Gruppe/Untergruppe platziert wird, werden umfassende Textstile unterstützt.

| Eigenschaft | Typ | Erforderlich |Beschreibung |
|---|---|---|---|
| **Text** | Zeichenfolge oder [BindableString](#bindablestring) | false | Der anzuzeigende Text. Unterstützung für Datenbindung, die im Creators Update hinzugefügt wurde, kann aber nur für Elemente der obersten Ebene verwendet werden. |
| **Hintstyle** | [Adaptivetextstyle](#adaptivetextstyle) | false | Der Stil steuert den Schriftgrad, die Schriftbreite und die Deckkraft des Texts. Kann nur für Textelemente innerhalb einer Gruppe/Untergruppe verwendet werden. |
| **Hintwrap** | bool? | false | Legen Sie diese Option auf „Wahr“ fest, um Textumbruch zu aktivieren. Textelemente der obersten Ebene ignorieren diese Eigenschaft und verwenden immer Textumbruch (Sie können HintMaxLines = 1 verwenden, um den Textumbruch für Textelemente der obersten Ebene zu deaktivieren). Bei Textelementen in Gruppen/Untergruppen ist für Textumbruch standardmäßig „Falsch“ festgelegt. |
| **Hintmaxlines** | int? | false | Die maximale Anzahl der Zeilen, die das Textelement anzeigen darf. |
| **Hintminlines** | int? | false | Die Mindestanzahl der Zeilen, die das Textelement anzeigen muss. Kann nur für Textelemente innerhalb einer Gruppe/Untergruppe verwendet werden. |
| **Hintalign** | [Adaptivetextalign](#adaptivetextalign) | false | Die horizontale Ausrichtung des Texts. Kann nur für Textelemente innerhalb einer Gruppe/Untergruppe verwendet werden. |
| **Sprache** | string | false | Das Zielgebietsschema der XML-Nutzlast, angegeben als BCP-47-Sprachtags wie z. B. „ en-US“ oder „fr-FR“. Das hier angegebene Gebietsschema überschreibt jedes andere – etwa in der Bindung oder im visuellen Element – angegebene Gebietsschema. Wenn dieser Wert eine Literalzeichenfolge ist, wird für dieses Attribut standardmäßig die Sprache der Benutzeroberfläche des Benutzers verwendet. Wenn dieser Wert ein Zeichenfolgenverweis ist, wird für dieses Attribut standardmäßig das Gebietsschema verwendet, das beim Auflösen der Zeichenfolge von Windows-Runtime ausgewählt wurde. |


### <a name="bindablestring"></a>BindableString
Ein Bindungswert für Zeichenfolgen.

| Eigenschaft | Typ | Erforderlich | Beschreibung |
|---|---|---|---|
| **BindingName** | string | true | Ruft den Namen auf, der dem Bindungsdatenwert zugeordnet ist, oder legt diesen fest. |


### <a name="adaptivetextstyle"></a>AdaptiveTextStyle
Textstil steuert Schriftgrad, Schriftbreite und Deckkraft. Dezente Deckkraft ist 60 % undurchsichtig.

| Wert | Bedeutung |
|---|---|
| **Default (Standard)** | Standardwert Stil wird durch den Renderer bestimmt. |
| **Unter** | Kleiner als Schriftgrad des Absatzes. |
| **Captionfeine** | Identisch mit „Untertitel für Hörgeschädigte“, aber mit dezenter Deckkraft. |
| **Instanz** | Schriftgrad des Absatzes. |
| **Bodyfein** | Identisch mit „Textkörper“, aber mit dezenter Deckkraft. |
| **Sock** | Schriftgrad des Absatzes, fette Schriftbreite. Im Wesentlichen die fettgedruckte Version des Textkörpers. |
| **Basesubtle** | Identisch mit „Basis“, aber mit dezenter Deckkraft. |
| **Titel** | H4-Schriftgrad. |
| **Subtitlefeine** | Identisch mit „Untertitel“, aber mit dezenter Deckkraft. |
| **Title** | H3-Schriftgrad. |
| **Titlefeine** | Identisch mit „Titel“, aber mit dezenter Deckkraft. |
| **Titleneneral** | Identisch mit „Titel“, aber ohne Abstand nach oben/unten. |
| **Subheader** | H2-Schriftgrad. |
| **Subheaderfein** | Identisch mit „Unterüberschrift“, aber mit dezenter Deckkraft. |
| **Subheadernumeral** | Identisch mit „Unterüberschrift“, aber ohne Abstand nach oben/unten. |
| **Header** | H1-Schriftgrad. |
| **Headerfein** | Identisch mit „Kopfzeile“, aber mit dezenter Deckkraft. |
| **Headernumeral** | Identisch mit „Kopfzeile“, aber ohne Abstand nach oben/unten. |


### <a name="adaptivetextalign"></a>AdaptiveTextAlign
Steuert die horizontale Ausrichtung des Texts.

| Wert | Bedeutung |
|---|---|
| **Default (Standard)** | Standardwert Ausrichtung wird automatisch vom Renderer bestimmt. |
| **Auto** | Ausrichtung durch die aktuelle Sprache und Kultur bestimmt. |
| **Linken** | Den Text horizontal linksbündig ausrichten. |
| **Tagesstätte** | Den Text horizontal zentrieren. |
| **Richting** | Den Text horizontal rechtsbündig ausrichten. |


## <a name="adaptiveimage"></a>AdaptiveImage
Ein Inlinebild.

| Eigenschaft | Typ | Erforderlich |Beschreibung |
|---|---|---|---|
| **Quelle** | string | true | Die URL zum Bild. ms-appx, ms-appdata und http werden unterstützt. Im Fall Creators Update kann die Größe der Webbilder 3 MB für normale Verbindungen und 1 MB für getaktete Verbindungen betragen. Auf Geräten, die noch nicht das Fall Creators Update haben, dürfen Webbilder nicht größer als 200 KB sein. |
| **Hintcrop** | [Adaptiveimagecrop](#adaptiveimagecrop) | false | Neu im Anniversary Update: den gewünschten Zuschnitt des Bilds festlegen. |
| **Hintremovemargin** | bool? | false | Bilder in Gruppen/Untergruppen verfügen standardmäßig über einen Rand von 8 Pixel. Sie können diesen Rand entfernen, indem Sie diese Eigenschaft auf „Wahr“ festlegen. |
| **Hintalign** | [Adaptiveimagealign](#adaptiveimagealign) | false | Die horizontale Ausrichtung des Bilds. Kann nur für Bilder in einer Gruppe/Untergruppe verwendet werden. |
| **AlternateText** | string | false | Alternativtext, der das Bild beschreibt; wird für Bedienungshilfen verwendet. |
| **Addimagequery** | bool? | false | Legen Sie den Wert auf „Wahr“ fest, um an die in der Popupbenachrichtigung angegebene Bild-URL eine Abfragezeichenfolge anzufügen. Verwenden Sie dieses Attribut, wenn Ihr Server Bilder hostet und Abfragezeichenfolgen verarbeiten kann, indem er entweder basierend auf den Abfragezeichenfolgen eine Bildvariante abruft oder die Abfragezeichenfolge ignoriert und das Bild wie angegeben ohne die Abfragezeichenfolge zurückgibt. Diese Abfragezeichenfolge gibt Skalierung, Kontrasteinstellung und Sprache an. So wird beispielsweise ein in der Benachrichtigung angegebener Wert „www.website.com/images/hello.png“ zu „www.website.com/images/hello.png?ms-scale=100&ms-contrast=standard&ms-lang=en-us“ |


### <a name="adaptiveimagecrop"></a>AdaptiveImageCrop
Gibt den gewünschten Zuschnitt des Bilds an.

| Wert | Bedeutung |
|---|---|
| **Default (Standard)** | Standardwert Zuschneideverhalten wird vom Renderer bestimmt. |
| **Keine** | Bild wird nicht zugeschnitten. |
| **Kreisen** | Bild wird kreisförmig zugeschnitten. |


### <a name="adaptiveimagealign"></a>AdaptiveImageAlign
Gibt die horizontale Ausrichtung für ein Bild an.

| Wert | Bedeutung |
|---|---|
| **Default (Standard)** | Standardwert Ausrichtungsverhalten vom Renderer bestimmt. |
| **Tre** | Bild wird gestreckt, um die verfügbare Breite (und möglicherweise auch die verfügbare Höhe, je nachdem, wo das Bild platziert wird) auszufüllen. |
| **Linken** | Das Bild links ausrichten, wobei das Bild mit der systemeigenen Auflösung angezeigt wird. |
| **Tagesstätte** | Das Bild zentrieren, wobei das Bild mit der systemeigenen Auflösung angezeigt wird. |
| **Richting** | Das Bild rechts ausrichten, wobei das Bild mit der systemeigenen Auflösung angezeigt wird. |


## <a name="adaptivegroup"></a>AdaptiveGroup
Neu im Anniversary Update: Gruppen geben semantisch an, dass der Inhalt in der Gruppe entweder als Ganzes oder nicht angezeigt werden soll, wenn nicht genügend Platz vorhanden ist. Gruppen können auch mehrere Spalten erstellen.

| Eigenschaft | Typ | Erforderlich |Beschreibung |
|---|---|---|---|
| **Children** | IList<[AdaptiveSubgroup](#adaptivesubgroup)> | false | Untergruppen werden als vertikale Spalten angezeigt. Sie müssen Untergruppen verwenden, um Inhalt innerhalb einer AdaptiveGroup bereitzustellen. |


## <a name="adaptivesubgroup"></a>AdaptiveSubgroup
Neu im Anniversary Update: Untergruppen sind vertikale Spalten, die Text und Bilder enthalten können.

| Eigenschaft | Typ | Erforderlich |Beschreibung |
|---|---|---|---|
| **Children** | IList<[IAdaptiveSubgroupChild](#iadaptivesubgroupchild)> | false | [AdaptiveText](#adaptivetext) und [AdaptiveImage](#adaptiveimage) gültige untergeordnete Elemente von Untergruppen. |
| **Hintweight** | int? | false | Steuern Sie die Breite dieser Untergruppenspalte, indem Sie die Breite im Verhältnis zu den anderen Untergruppen angeben. |
| **Hinttextstapel** | [Adaptivesubgrouptextstacking](#adaptivesubgrouptextstacking) | false | Steuern Sie die vertikale Ausrichtung des Inhalts dieser Untergruppe. |


### <a name="iadaptivesubgroupchild"></a>IAdaptiveSubgroupChild
Markierungsschnittstelle für untergeordnete Untergruppenelemente.

| Implementierungen |
| --- |
| [Adaptivetext](#adaptivetext) |
| [Adaptiveimage](#adaptiveimage) |


### <a name="adaptivesubgrouptextstacking"></a>AdaptiveSubgroupTextStacking
TextStacking gibt die vertikale Ausrichtung des Inhalts an.

| Wert | Bedeutung |
|---|---|
| **Default (Standard)** | Standardwert Renderer wählt automatisch die standardmäßige vertikale Ausrichtung aus. |
| **Nach oben** | Vertikal oben ausrichten. |
| **Tagesstätte** | Vertikal zentrieren. |
| **Unten** | Vertikal unten ausrichten. |


## <a name="adaptiveprogressbar"></a>AdaptiveProgressBar
Neu im Creators Update: die Statusanzeige. Wird nur von Popups auf Desktop, Build 15063 oder höher unterstützt.

| Eigenschaft | Typ | Erforderlich | Beschreibung |
|---|---|---|---|
| **Title** | Zeichenfolge oder [BindableString](#bindablestring) | false | Ruft einen optionalen Zeichenfolgetitel auf oder legt diesen fest. Unterstützt die Datenbindung. |
| **Wert** | doppelt oder [AdaptiveProgressBarValue](#adaptiveprogressbarvalue) oder [BindableProgressBarValue](#bindableprogressbarvalue) | false | Ruft den Wert der Statusanzeige auf oder legt diesen fest. Unterstützt die Datenbindung. Die Standardeinstellung ist 0. |
| **Valuestringoverride** | Zeichenfolge oder [BindableString](#bindablestring) | false | Ruft eine optionale Zeichenfolge auf oder legt diese fest, damit sie anstelle der Standardzeichenfolge in Prozent angezeigt wird. Wenn dies nicht bereitgestellt ist, wird etwas wie "70 %" angezeigt. |
| **Status** | Zeichenfolge oder [BindableString](#bindablestring) | true | Ruft eine Statuszeichenfolge auf oder legt diese fest (erforderlich), die unter der Statusanzeige auf der linken Seite angezeigt wird. Diese Zeichenfolge sollte den Status des Vorgangs widerspiegeln, z. B. "Herunterladen..." oder "Installieren von..." |


### <a name="adaptiveprogressbarvalue"></a>AdaptiveProgressBarValue
Eine Klasse, die den Fortschritt der Statusanzeige anzeigt

| Eigenschaft | Typ | Erforderlich | Beschreibung |
|---|---|---|---|
| **Wert** | double | false | Ruft den Wert (0,0 – 1,0) ab oder legt diese fest, der den abgeschlossenen Prozentsatz darstellt. |
| **IsIndeterminate** | bool | false | Ruft den Wert ab, der angibt, ob die Statusanzeige unbestimmt ist, oder legt diesen fest. Wenn dies der Fall ist, wird **Wert** ignoriert. |


### <a name="bindableprogressbarvalue"></a>BindableProgressBarValue
Ein bindender Wert der Statusanzeige.

| Eigenschaft | Typ | Erforderlich | Beschreibung |
|---|---|---|---|
| **BindingName** | string | true | Ruft den Namen auf, der dem Bindungsdatenwert zugeordnet ist, oder legt diesen fest. |


## <a name="toastgenericapplogo"></a>ToastGenericAppLogo
Ein Logo, das anstelle des App-Logos angezeigt werden soll.

| Eigenschaft | Typ | Erforderlich |Beschreibung |
|---|---|---|---|
| **Quelle** | string | true | Die URL zum Bild. ms-appx, ms-appdata und http werden unterstützt. HTTP-Bilder müssen kleiner als 200 KB sein. |
| **Hintcrop** | ["Anastgenericapplogocrop"](#toastgenericapplogocrop) | false | Geben Sie an, wie das Bild zugeschnitten werden soll. |
| **AlternateText** | string | false | Alternativtext, der das Bild beschreibt; wird für Bedienungshilfen verwendet. |
| **Addimagequery** | bool? | false | Legen Sie den Wert auf „Wahr“ fest, um an die in der Popupbenachrichtigung angegebene Bild-URL eine Abfragezeichenfolge anzufügen. Verwenden Sie dieses Attribut, wenn Ihr Server Bilder hostet und Abfragezeichenfolgen verarbeiten kann, indem er entweder basierend auf den Abfragezeichenfolgen eine Bildvariante abruft oder die Abfragezeichenfolge ignoriert und das Bild wie angegeben ohne die Abfragezeichenfolge zurückgibt. Diese Abfragezeichenfolge gibt Skalierung, Kontrasteinstellung und Sprache an. So wird beispielsweise ein in der Benachrichtigung angegebener Wert „www.website.com/images/hello.png“ zu „www.website.com/images/hello.png?ms-scale=100&ms-contrast=standard&ms-lang=en-us“ |


### <a name="toastgenericapplogocrop"></a>ToastGenericAppLogoCrop
Steuert das Zuschneiden des App-Logo-Bilds.

| Wert | Bedeutung |
|---|---|
| **Default (Standard)** | Beim Zuschneiden wird das Standardverhalten des Renderers verwendet. |
| **Keine** | Bild wird nicht zugeschnitten, wird quadratisch angezeigt. |
| **Kreisen** | Bild wird kreisförmig zugeschnitten. |


## <a name="toastgenericheroimage"></a>ToastGenericHeroImage
Ein ausgewähltes Favoritenbild, das auf dem Popup und im Info-Center angezeigt wird.

| Eigenschaft | Typ | Erforderlich |Beschreibung |
|---|---|---|---|
| **Quelle** | string | true | Die URL zum Bild. ms-appx, ms-appdata und http werden unterstützt. HTTP-Bilder müssen kleiner als 200 KB sein. |
| **AlternateText** | string | false | Alternativtext, der das Bild beschreibt; wird für Bedienungshilfen verwendet. |
| **Addimagequery** | bool? | false | Legen Sie den Wert auf „Wahr“ fest, um an die in der Popupbenachrichtigung angegebene Bild-URL eine Abfragezeichenfolge anzufügen. Verwenden Sie dieses Attribut, wenn Ihr Server Bilder hostet und Abfragezeichenfolgen verarbeiten kann, indem er entweder basierend auf den Abfragezeichenfolgen eine Bildvariante abruft oder die Abfragezeichenfolge ignoriert und das Bild wie angegeben ohne die Abfragezeichenfolge zurückgibt. Diese Abfragezeichenfolge gibt Skalierung, Kontrasteinstellung und Sprache an. So wird beispielsweise ein in der Benachrichtigung angegebener Wert „www.website.com/images/hello.png“ zu „www.website.com/images/hello.png?ms-scale=100&ms-contrast=standard&ms-lang=en-us“ |


## <a name="toastgenericattributiontext"></a>ToastGenericAttributionText
Zuschreibungstext, der am unteren Rand der Popupbenachrichtigung angezeigt wird.

| Eigenschaft | Typ | Erforderlich | Beschreibung |
|---|---|---|---|
| **Text** | string | true | Der anzuzeigende Text. |
| **Sprache** | string | false | Das Zielgebietsschema der visuellen Nutzlast bei Verwendung lokalisierter Ressourcen, angegeben als BCP-47-Sprachtags wie z. B. „en-US“ oder „fr-FR“. Falls nicht angegeben, wird stattdessen das Gebietsschema des Systems verwendet. |


## <a name="itoastactions"></a>IToastActions
Markierungsschnittstelle für Popupaktionen/-eingaben.

| Implementierungen |
| --- |
| [Ein-und ausgeschaltet](#toastactionscustom) |
| ["", "", "", ""](#toastactionssnoozeanddismiss) |


## <a name="toastactionscustom"></a>ToastActionsCustom
*Implementiert [ideastactions](#itoastactions)*

Erstellen Sie Ihre eigenen benutzerdefinierten Aktionen und Eingaben unter Verwendung von Steuerelementen wie Schaltflächen, Textfeldern und Auswahleingaben.

| Eigenschaft | Typ | Erforderlich | Beschreibung |
|---|---|---|---|
| **Ungs** | IList<[IToastInput](#itoastinput)> | false | Eingaben wie Textfelder und Auswahleingaben. Es sind nur bis zu 5 Eingaben zulässig. |
| **Schaltflächen** | IList<[IToastButton](#itoastbutton)> | false | Schaltflächen werden nach all den Eingaben (oder neben einer Eingabe, wenn die Schaltfläche als schnelle Antwortschaltfläche verwendet wird) angezeigt. Es sind nur bis zu 5 Eingaben zulässig (oder weniger, wenn Sie auch Kontextmenüelemente besitzen). |
| **Contextmenuitems** | IList<[ToastContextMenuItem](#toastcontextmenuitem)> | false | Neu im Anniversary Update: benutzerdefinierte Kontextmenüelemente, die zusätzliche Aktionen bereitstellen, wenn der Benutzer mit der rechten Maustaste auf die Benachrichtigung klickt. Es sind nur bis zu 5 Schaltflächen und Kontextmenüelemente *in Kombination* zulässig. |


## <a name="itoastinput"></a>IToastInput
Markierungsschnittstelle für Popupeingaben.

| Implementierungen |
| --- |
| [-Textfeld](#toasttextbox) |
| ["-Selectionbox"](#toastselectionbox) |


## <a name="toasttextbox"></a>ToastTextBox
*Implementiert [ionastinput](#itoastinput)*

Ein Textfeld-Steuerelement, in das der Benutzer Text eingeben kann.

| Eigenschaft | Typ | Erforderlich | Beschreibung |
|---|---|---|---|
| **Name** | string | true | Die ID ist erforderlich und wird verwendet, um dem vom Benutzer eingegebenen Text einem Schlüssel-Wert-Paar von ID/Wert zuzuordnen, das Ihre App später verwendet. |
| **Title** | string | false | Titeltext, der über dem Textfeld angezeigt werden soll. |
| **Placeholdercontent** | string | false | Platzhaltertext kann im Textfeld angezeigt werden, wenn der Benutzer noch keinen Text eingegeben hat. |
| **Defaultinput** | string | false | Der erste Text, der im Textfeld platziert werden soll. Lassen Sie diesen Wert bei einem leeren Textfeld auf Null. |


## <a name="toastselectionbox"></a>ToastSelectionBox
*Implementiert [ionastinput](#itoastinput)*

Ein Auswahl-Steuerelement, mit dem Benutzer aus einer Dropdown-Liste von Optionen auswählen können.

| Eigenschaft | Typ | Erforderlich | Beschreibung |
|---|---|---|---|
| **Name** | string | true | Die ID muss angegeben werden. Wenn der Benutzer dieses Element ausgewählt hat, wird diese ID wieder an den Code Ihrer App übergeben und gibt an, welche Auswahl der Benutzer getroffen hat. |
| **Inhalt** | string | true | Inhalt ist erforderlich und ist eine Zeichenfolge, die für das Auswahlelement angezeigt wird. |


### <a name="toastselectionboxitem"></a>ToastSelectionBoxItem
Ein Auswahlfeldelement (ein Element, das der Benutzer aus der Dropdown-Liste auswählen kann).

| Eigenschaft | Typ | Erforderlich | Beschreibung |
|---|---|---|---|
| **Name** | string | true | Die ID ist erforderlich und wird verwendet, um dem vom Benutzer eingegebenen Text einem Schlüssel-Wert-Paar von ID/Wert zuzuordnen, das Ihre App später verwendet. |
| **Title** | string | false | Titeltext, der über dem Auswahlfeld angezeigt werden soll. |
| **Defaultselectionboxitemid** | string | false | Hiermit wird gesteuert, welches Element standardmäßig ausgewählt wird, und auf die ID-Eigenschaft von [ToastSelectionBoxItem](#toastselectionboxitem) Bezug genommen. Wenn Sie diese nicht angeben, wird die Standardauswahl leer sein (Benutzer wird nichts angezeigt). |
| **Elemente** | IList<[ToastSelectionBoxItem](#toastselectionboxitem)> | false | Die Auswahlelemente, die der Benutzer in dieser SelectionBox auswählen kann. Es können nur 5 Elemente hinzugefügt werden. |


## <a name="itoastbutton"></a>IToastButton
Markierungsschnittstelle für Popupschaltflächen.

| Implementierungen |
| --- |
| [Umschalten](#toastbutton) |
| ["Onastbuttonsnooze"](#toastbuttonsnooze) |
| [Ein-und ausblenden](#toastbuttondismiss) |


## <a name="toastbutton"></a>ToastButton
*Implementiert [Iper astbutton](#itoastbutton) .*

Eine Schaltfläche, auf die der Benutzer klicken kann.

| Eigenschaft | Typ | Erforderlich | Beschreibung |
|---|---|---|---|
| **Inhalt** | string | true | Erforderlich Der Text, der auf der Schaltfläche angezeigt werden soll. |
| **Argumentation** | string | true | Erforderlich Von der App definierte Zeichenfolge mit Argumenten, welche die App später empfängt, wenn der Benutzer auf diese Schaltfläche klickt. |
| **ActivationType** | ["Deastactivationtype"](#toastactivationtype) | false | Steuert, welche Art der Aktivierung beim Klicken auf diese Schaltfläche verwendet wird. Der Standardwert ist Vordergrund. |
| **Activationoptions** | ["Deastactivationoptions"](#toastactivationoptions) | false | Neu im Creators Update: Ruft eine zusätzliche Optionen zur Aktivierung der Popupschaltfläche auf oder legt diese fest. |


### <a name="toastactivationtype"></a>ToastActivationType
Legt die Art der Aktivierung fest, die verwendet wird, wenn der Benutzer mit einer bestimmten Aktion interagiert.

| Wert | Bedeutung |
|---|---|
| **Grundes** | Standardwert Ihre Vordergrund-App wird gestartet. |
| **Hintergrund** | Die entsprechende Hintergrundaufgabe (vorausgesetzt, dass Sie alles eingerichtet haben) wird ausgelöst, und Sie können Code im Hintergrund ausführen (z. B. die Schnellantwortnachricht des Benutzers senden), ohne den Benutzer zu unterbrechen. |
| **Protocol** | Starten Sie eine andere App mit Protokollaktivierung. |


### <a name="toastactivationoptions"></a>ToastActivationOptions
Neu im Creators Update: zusätzliche Optionen für Aktivierung.

| Eigenschaft | Typ | Erforderlich | Beschreibung |
|---|---|---|---|
| **Afteractivationbehavior** | ["Deastafteractivationbehavior"](#toastafteractivationbehavior) | false | Neu im Fall Creators Update: Ruft das Verhalten auf, das das Popup verwenden soll, wenn der Benutzer diese Aktion aufruft oder legt es fest. Dies funktioniert nur auf Desktop für [ToastButton](#toastbutton) und [ToastContextMenuItem](#toastcontextmenuitem). |
| **Protocolactivationtargetapplicationpfn** | string | false | Bei Verwendung von *ToastActivationType.Protocol* können Sie optional die Ziel-PFN angeben, sodass die gewünschte App immer gestartet wird, unabhängig davon, ob mehrere Apps zur Behandlung des gleichen Protokoll-Uri registriert sind. |


### <a name="toastafteractivationbehavior"></a>ToastAfterActivationBehavior
Gibt das Verhalten an, das das Popup verwenden soll, wenn der Benutzer eine Aktion auf dem Popup ausführt.

| Wert | Bedeutung |
|---|---|
| **Default (Standard)** | Standardverhalten. Das Popup wird geschlossen, wenn der Benutzer eine Aktion auf das Popup ausführt. |
| **"Pendingupdate"** | Nachdem der Benutzer auf eine Schaltfläche im Popup klickt, bleibt die Benachrichtigung erhalten, im Ansichtszustand "Ausstehendes Update". Sie sollten Ihr Popup über eine Hintergrundaufgabe sofort aktualisieren, damit der Benutzer den visuellen Zustand "Ausstehendes Update" nicht zu lange sieht. |


## <a name="toastbuttonsnooze"></a>ToastButtonSnooze
*Implementiert [Iper astbutton](#itoastbutton) .*

Eine systemgesteuerte Erneut erinnern-Schaltfläche, die automatisch das erneute Erinnern an die Benachrichtigung verarbeitet.

| Eigenschaft | Typ | Erforderlich | Beschreibung |
|---|---|---|---|
| **Customcontent** | string | false | Optionaler benutzerdefinierter Text, der auf der Schaltfläche angezeigt wird und den standardmäßigen lokalisierten „Erneut erinnern“-Text überschreibt. |


## <a name="toastbuttondismiss"></a>ToastButtonDismiss
*Implementiert [Iper astbutton](#itoastbutton) .*

Eine systemgesteuerte Schließen-Schaltfläche, welche die Benachrichtigung schließt, wenn sie angeklickt wird.

| Eigenschaft | Typ | Erforderlich | Beschreibung |
|---|---|---|---|
| **Customcontent** | string | false | Optionaler benutzerdefinierter Text, der auf der Schaltfläche angezeigt wird und den standardmäßigen lokalisierten „Schließen“-Text überschreibt. |


## <a name="toastactionssnoozeanddismiss"></a>ToastActionsSnoozeAndDismiss
*Implementiert [IToastActions](#itoastactions)

Erstellt automatisch ein Auswahlfeld für Erneut erinnern-Intervalle und Erneut erinnern-/Schließen-Schaltflächen, die alle automatisch lokalisiert werden, und Erneut erinnern-Logik wird vom System automatisch behandelt.

| Eigenschaft | Typ | Erforderlich | Beschreibung |
|---|---|---|---|
| **Contextmenuitems** | IList<[ToastContextMenuItem](#toastcontextmenuitem)> | false | Neu im Anniversary Update: benutzerdefinierte Kontextmenüelemente, die zusätzliche Aktionen bereitstellen, wenn der Benutzer mit der rechten Maustaste auf die Benachrichtigung klickt. Sie können nur bis zu 5 Elemente haben. |


## <a name="toastcontextmenuitem"></a>ToastContextMenuItem
Ein Kontextmenüelement-Eintrag.

| Eigenschaft | Typ | Erforderlich | Beschreibung |
|---|---|---|---|
| **Inhalt** | string | true | Erforderlich Der anzuzeigende Text. |
| **Argumentation** | string | true | Erforderlich Von der App definierte Zeichenfolge mit Argumenten, welche die App später nach deren Aktivierung abrufen kann, wenn der Benutzer auf das Menüelement klickt. |
| **ActivationType** | ["Deastactivationtype"](#toastactivationtype) | false | Steuert, welche Art der Aktivierung beim Klicken auf dieses Menüelement verwendet wird. Der Standardwert ist Vordergrund. |
| **Activationoptions** | ["Deastactivationoptions"](#toastactivationoptions) | false | Neu im Creators Update: zusätzliche Optionen im Zusammenhang mit der Aktivierung des Kontextmenüelements des Popups. |


## <a name="toastaudio"></a>ToastAudio
Geben Sie die Audiodaten an, die beim Empfang der Popupbenachrichtigung wiedergegeben werden sollen.

| Eigenschaft | Typ | Erforderlich | Beschreibung |
|---|---|---|---|
| **Src** | uri | false | Die Mediendatei, die anstelle des Standardsounds wiedergegeben werden soll. Es werden nur ms-appx und ms-appdata unterstützt. |
| **ESE** | boolean | false | Legen Sie den Wert auf „Wahr“ fest, wen der Sound so lange wiederholt werden soll, wie das Popup angezeigt wird. Wählen Sie „Falsch“ aus, wenn der Sound nur einmal wiedergegeben werden soll (Standardeinstellung). |
| **Me** | boolean | false | „Wahr“, um den Sound stummzuschalten; „Falsch“, um die Wiedergabe des Popupbenachrichtigungs-Sounds zu erlauben (Standardeinstellung). |


## <a name="toastheader"></a>ToastHeader
Neu im Creators Update: Eine benutzerdefinierte Kopfzeile, die mehrere Benachrichtigungen im Info-Center gruppiert.

| Eigenschaft | Typ | Erforderlich | Beschreibung |
|---|---|---|---|
| **Name** | string | true | Ein vom Entwickler erstellter Bezeichner, der diese Kopfzeile eindeutig identifiziert. Wenn zwei Benachrichtigungen die gleiche Kopfzeilen-ID haben, werden sie im Info-Center unter der gleichen Kopfzeile angezeigt. |
| **Title** | string | true | Ein Titel für die Kopfzeile. |
| **Argumentation**| string | true | Ruft oder legt eine vom Entwickler definierte Zeichenfolge mit Argumenten fest, die an die App zurückgegeben wird, wenn der Benutzer diesen Header anklickt. Darf nicht NULL sein. |
| **ActivationType** | ["Deastactivationtype"](#toastactivationtype) | false | Steuert, welche Art der Aktivierung beim Klicken auf diesen Header verwendet wird. Der Standardwert ist Vordergrund. Beachten Sie, dass nur Vordergrund und Protokoll unterstützt werden. |
| **Activationoptions** | ["Deastactivationoptions"](#toastactivationoptions) | false | Ruft eine zusätzliche Optionen zur Aktivierung der Popup-Header auf oder legt diese fest. |


## <a name="related-topics"></a>Verwandte Themen

* [Schnellstart: Senden eines lokalen Popups und behandeln der Aktivierung](https://blogs.msdn.microsoft.com/tiles_and_toasts/2015/07/08/quickstart-sending-a-local-toast-notification-and-handling-activations-from-it-windows-10/)
* [Benachrichtigungs Bibliothek auf GitHub](https://github.com/windows-toolkit/WindowsCommunityToolkit/tree/dev/Notifications)