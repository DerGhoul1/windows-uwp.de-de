---
Description: Spezielle Kachelvorlagen sind individuelle Vorlagen, die animiert sind oder mit denen Sie Vorgänge durchführen können, die mit adaptiven Kacheln nicht möglich sind.
title: Spezielle Kachelvorlagen
ms.assetid: 1322C9BA-D5B2-45E2-B813-865884A467FF
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 739abc139eabc9f773938f55c15d3e18aaf562ce
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66365964"
---
# <a name="special-tile-templates"></a>Spezielle Kachelvorlagen
 

Spezielle Kachelvorlagen sind individuelle Vorlagen, die animiert sind oder mit denen Sie Vorgänge durchführen können, die mit adaptiven Kacheln nicht möglich sind. Jede spezielle Tile-Vorlage wurde speziell für Windows 10, mit Ausnahme von der Vorlage iconic-Kachel eine klassische spezielle Vorlage erstellt, die für Windows 10 aktualisiert wurde. In diesem Artikel werden drei speziellen kachelvorlagen behandelt: Symbol, Fotos und Personen.

## <a name="iconic-tile-template"></a>Iconic-Kachelvorlage


Mit der Iconic-Vorlage (auch als „IconWithBadge“-Vorlage bezeichnet) können Sie ein kleines Bild in der Mitte der Kachel anzeigen. Die Vorlage wird von Windows 10 auf Smartphone und Tablet/Desktop unterstützt.

![Kleine und mittelgroße E-Mail-Kacheln](images/iconic-template-mail-2sizes.png)

### <a name="how-to-create-an-iconic-tile"></a>Erstellen einer ikonischen Kachel

Die folgenden Schritte behandelt alles, was Sie zum Erstellen von iconic-Kachel für Windows 10 wissen müssen. Auf hoher Ebene benötigen Sie Ihre Iconic-Bildressource. Dann senden Sie mithilfe der Iconic-Vorlage eine Benachrichtigung an die Kachel und senden schließlich eine Signalbenachrichtigung, die die auf der Kachel anzuzeigende Zahl bereitstellt.

![Entwicklerablauf der Iconic-Kachel](images/iconic-template-dev-flow.png)

**Schritt 1: Erstellen Sie Medienobjekte Bild im PNG-format**

Erstellen Sie die Symbolressourcen für die Kachel, und platzieren Sie diese mit den anderen Ressourcen in Ihren Projektressourcen. Erstellen Sie mindestens ein Symbol mit 200 x 200 Pixeln, das für kleine und mittelgroße Kacheln auf dem Telefon und dem Desktop funktioniert. Um die bestmögliche Benutzerfreundlichkeit bereitzustellen, erstellen Sie für jede Größe ein eigenes Symbol. Auf diese Ressourcen ist kein Abstand erforderlich. Informationen zu den Größen finden Sie im Bild unten.

Speichern Sie Symbolressourcen im PNG-Format und mit Transparenz. Unter Windows Phone wird jedes nicht transparente Pixel weiß (RGB-Wert 255, 255, 255) angezeigt. Verwenden Sie aus Gründen der Konsistenz und Einfachheit weiß auch für Desktopsymbole.

Windows 10 auf Tablet, Laptop und Desktop unterstützt nur quadratische Symbolressourcen. Windows Phone unterstützt quadratische Ressourcen und Ressourcen, die höher als breit sind, bis zu einem Breite-Höhe-Verhältnis von 2:3, was für Bilder, z. B. ein Telefonsymbol, nützlich ist.

![Symbolgrößen auf kleinen und mittelgroßen Kacheln auf Telefon und Desktop](images/iconic-template-sizing-info.png)

![Größen für Ressourcen mit und ohne Signal](images/assetguidance24.png)

Für quadratische Ressourcen erfolgt eine automatische Zentrierung innerhalb des Containers:

![Größe einer quadratischen Ressource, mit und ohne Signal](images/assetguidance25.png)

Bei nicht quadratischen Ressourcen erfolgt eine automatische horizontale/vertikale Zentrierung und ein Andocken an die Breite bzw. Höhe des Containers:

![Größe einer nicht quadratischen Ressource, mit und ohne Signal](images/assetguidance26a.png)

![Größe einer nicht quadratischen Ressource, mit und ohne Signal](images/assetguidance26b.png)

**Schritt 2: Erstellen Sie Ihre Basiskachel**

Sie können die Iconic-Vorlage für primäre und sekundäre Kacheln verwenden. Wenn Sie sie für eine sekundäre Kachel verwenden, müssen Sie zuerst die sekundäre Kachel erstellen oder eine bereits angeheftete sekundäre Kachel verwenden. Primäre Kacheln sind implizit angeheftet, und Sie können jederzeit Benachrichtigungen an sie senden.

**Schritt 3: Senden einer Benachrichtigung an die Kachel**

Obwohl dieser Schritt variieren kann, je nachdem, ob die Benachrichtigung lokal oder per Serverpush gesendet wird, bleibt die gesendete XML-Nutzlast identisch. Um eine lokale Kachelbenachrichtigung zu senden, erstellen Sie einen [**TileUpdater**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileUpdater) für die Kachel (entweder primäre oder sekundäre Kachel), und senden Sie dann eine Benachrichtigung an die Kachel, die die Iconic-Kachelvorlage verwendet, wie unten dargestellt. Im Idealfall sollten Sie auch Bindungen für breite und große Kachelgrößen mit [adaptiven Kachelvorlagen](create-adaptive-tiles.md) einschließen.

Hier sehen Sie einen Beispielcode für die XML-Nutzlast:

```xml
<tile>
  <visual>

    <binding template="TileSquare150x150IconWithBadge">
      <image id="1" src="Iconic.png" alt="alt text"/>
    </binding>
    
    <binding template="TileSquare71x71IconWithBadge">
      <image id="1" src="Iconic.png" alt="alt text"/>
    </binding>

  </visual>
</tile>
```

Die XML-Nutzlast dieser Iconic-Kachelvorlage verwendet ein Bildelement, das auf das Bild verweist, das Sie in Schritt 1 erstellt haben. Jetzt kann die Kachel das Signal neben Ihrem Symbol anzeigen, wenn Signalbenachrichtigungen gesendet werden.

**Schritt 4: Senden Sie eine signalbenachrichtigung an die Kachel**

Wie Schritt 3 kann dieser Schritt variieren, je nachdem, ob die Benachrichtigung lokal oder per Serverpush gesendet wird, aber die gesendete XML-Nutzlast bleibt identisch. Erstellen Sie zum Senden einer lokalen Signalbenachrichtigung einen [**BadgeUpdater**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.BadgeUpdater) für die Kachel (entweder primäre oder sekundäre Kachel), und senden Sie dann eine Signalbenachrichtigung mit dem gewünschten Wert (oder löschen Sie das Signal).

Hier sehen Sie einen Beispielcode für die XML-Nutzlast:

```xml
<badge value="2"/>
```

Das Signal der Kachel wird entsprechend aktualisiert.

**Schritt 5: Zusammenfassung**

Die folgende Abbildung zeigt, wie die verschiedenen APIs und Nutzlasten den einzelnen Aspekten der ikonischen Kachelvorlage zugeordnet sind. Ein [Kachelbenachrichtigung](https://docs.microsoft.com/previous-versions/windows/apps/hh779724(v=win.10)) (die diese &lt;binding&gt;-Elemente enthält) dient zum Angeben der ikonischen Vorlage und der Bildressource. Eine [Signalbenachrichtigung](https://docs.microsoft.com/previous-versions/windows/apps/hh779719(v=win.10)) gibt den numerischen Wert an. Kacheleigenschaften steuern den Anzeigenamen, die Farbe und vieles mehr der Kachel.

![APIs und Nutzlasten, die der Iconic-Kachelvorlage zugeordnet sind](images/iconic-template-properties-info.png)

## <a name="photos-tile-template"></a>Kachelvorlage „Fotos“


Mit der Kachelvorlage „Fotos“ können Sie eine Diashow von Fotos auf der Live-Kachel anzeigen. Die Vorlage wird für alle Kachelgrößen, einschließlich klein, unterstützt und verhält sich für jede Kachelgröße gleich. Das folgende Beispiel zeigt fünf Frames einer mittelgroßen Kachel, die die Vorlage „Fotos“ verwendet. Die Vorlage verfügt über eine Zoom- und Überblendanimation, die ausgewählte Fotos in einer Endlosschleife durchläuft.

![Bilddiashow mit Kachelvorlage „Fotos“](images/photo-tile-template-image01.jpg)

### <a name="how-to-use-the-photos-template"></a>Verwenden der Kachelvorlage „Fotos“

Das Verwenden der Vorlage „Fotos“ ist einfach, wenn Sie die [Benachrichtigungsbibliothek](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) installiert haben. Obwohl Sie unformatierte XML-Daten verwenden können, empfehlen wir nachdrücklich die Verwendung der Benachrichtigungsbibliothek, damit Sie sich keine Gedanken zum Generieren gültiger XML- oder XML-Escapesequenzinhalte machen müssen.

Windows Phone zeigt bis zu 9 Fotos in einer Diashow an. Auf Tablet, Laptop und Desktop werden bis zu 12 Fotos angezeigt.

Informationen zum Senden der Kachelbenachrichtigung finden Sie im [Artikel zum Senden von Benachrichtigungen](index.md).


```xml
<!--
 
To use the Photos template...
 
 - On any adaptive tile binding (like TileMedium or TileWide)
   - Set the hint-presentation attribute to "photos"
   - Add up to 12 images as children of the binding.
    
-->
 
<tile>
  <visual>
     
    <binding template="TileMedium" hint-presentation="photos">
       
      <image src="Assets/1.jpg" />
      <image src="ms-appdata:///local/Images/2.jpg"/>
      <image src="http://msn.com/images/3.jpg"/>
       
      <!--TODO: Can have 12 images total-->
       
    </binding>
     
    <!--TODO: Add bindings for other tile sizes-->
     
  </visual>
</tile>
```

```csharp
/*
 
To use the Photos template...
 
 - On any TileBinding object
   - Set Content property to new instance of TileBindingContentPhotos
   - Add up to 12 images to Images property of TileBindingContentPhotos.
 
*/
 
TileContent content = new TileContent()
{
    Visual = new TileVisual()
    {
        TileMedium = new TileBinding()
        {
            Content = new TileBindingContentPhotos()
            {
                Images =
                {
                    new TileBasicImage() { Source = "Assets/1.jpg" },
                    new TileBasicImage() { Source = "ms-appdata:///local/Images/2.jpg" },
                    new TileBasicImage() { Source = "http://msn.com/images/3.jpg" }
 
                    // TODO: Can have 12 images total
                }
            }
        }
 
        // TODO: Add other tile sizes
    }
};
```

## <a name="people-tile-template"></a>Kachelvorlage „Kontakte“


Die Kontakte-App in Windows 10 verwendet eine spezielle Kachelvorlage, die eine Sammlung von Bildern in Kreisen anzeigt, die sich auf der Kachel vertikal oder horizontal verschieben. Dieser Tile-Vorlage wurde seit Windows 10 Build 10572 verfügbar, und jeder Benutzer ist es in ihrer app verwenden.

Die Kachelvorlage „Kontakte“ funktioniert auf Kacheln folgender Größen:

**Mittelgroße Kachel** (TileMedium)

![Mittelgroße Kachel „Kontakte“](images/people-tile-medium.png)

 

**Breite Kachel** (TileWide)

![Breite Kachel „Kontakte“](images/people-tile-wide.png)

 

**Große Kachel (nur Desktop)** (TileLarge)

![Große Kachel „Kontakte“](images/people-tile-large.png)

 

Bei Verwendung der [Benachrichtigungsbibliothek](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) müssen Sie zum Verwenden der Kachelvorlage „Kontakte“ nur ein neues *TileBindingContentPeople*-Objekt für Ihren *TileBinding*-Inhalt erstellen. Die *TileBindingContentPeople*-Klasse verfügt über eine Bildereigenschaft, mit der Sie Ihre Bilder hinzufügen.

Wenn Sie unformatiertes XML verwenden, legen Sie *hint-presentation* auf „Kontakte“ fest, und fügen Sie die Bilder als untergeordnete Elemente des Bindungselements hinzu.

Das folgende C#-Codebeispiel nimmt an, dass Sie [Benachrichtigungsbibliothek](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) verwenden.

```csharp
TileContent content = new TileContent()
{
    Visual = new TileVisual()
    {
        TileMedium = new TileBinding()
        {
            Content = new TileBindingContentPeople()
            {
                Images =
                {
                    new TileBasicImage() { Source = "Assets/ProfilePics/1.jpg" },
                    new TileBasicImage() { Source = "Assets/ProfilePics/2.jpg" },
                    new TileBasicImage() { Source = "Assets/ProfilePics/3.jpg" },
                    new TileBasicImage() { Source = "Assets/ProfilePics/4.jpg" },
                    new TileBasicImage() { Source = "Assets/ProfilePics/5.jpg" },
                    new TileBasicImage() { Source = "Assets/ProfilePics/6.jpg" },
                    new TileBasicImage() { Source = "Assets/ProfilePics/7.jpg" },
                    new TileBasicImage() { Source = "Assets/ProfilePics/8.jpg" },
                    new TileBasicImage() { Source = "Assets/ProfilePics/9.jpg" }
                }
            }
        }
    }
};
```

```xml
<tile>
  <visual>
 
    <binding template="TileMedium" hint-presentation="people">
      <image src="Assets/ProfilePics/1.jpg"/>
      <image src="Assets/ProfilePics/2.jpg"/>
      <image src="Assets/ProfilePics/3.jpg"/>
      <image src="Assets/ProfilePics/4.jpg"/>
      <image src="Assets/ProfilePics/5.jpg"/>
      <image src="Assets/ProfilePics/6.jpg"/>
      <image src="Assets/ProfilePics/7.jpg"/>
      <image src="Assets/ProfilePics/8.jpg"/>
      <image src="Assets/ProfilePics/9.jpg"/>
    </binding>
 
  </visual>
</tile>
```

Um eine optimale Benutzererfahrung bereitzustellen, empfehlen wir die Bereitstellung der folgenden Anzahl von Fotos für die einzelnen Kachelgrößen:

-   Mittelgroße Kachel: 9 Fotos
-   Breite Kachel: 15 Fotos
-   Große Kachel: 20 Fotos

Durch die Anzahl der Fotos werden einige leere Kreise ermöglicht, was bedeutet, dass die Kachel visuell nicht zu ausgelastet ist. Sie können die Anzahl der Fotos anpassen, um das gewünschte Erscheinungsbild zu erhalten.

Informationen zum Senden der Benachrichtigung finden Sie unter [Auswählen einer Methode für die Übermittlung von Benachrichtigungen](choosing-a-notification-delivery-method.md).

## <a name="related-topics"></a>Verwandte Themen


* [Vollständige Codebeispiel auf GitHub](https://github.com/WindowsNotifications/quickstart-people-tile-template)
* [Benachrichtigungsbibliothek](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)
* [Kacheln, Signale und Benachrichtigungen](index.md)
* [Erstellen adaptiver Kacheln](create-adaptive-tiles.md)
* [Kachelinhaltsschema](../tiles-and-notifications/tile-schema.md)
 

 




