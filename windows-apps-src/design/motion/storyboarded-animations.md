---
ms.assetid: 0CBCEEA0-2B0E-44A1-A09A-F7A939632F3A
title: Storyboardanimationen
description: Storyboardanimationen sind nicht nur Animationen visueller Art.
ms.date: 07/13/2018
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 1107670e837dff294739e9ba38c7dea9004d1d62
ms.sourcegitcommit: a20457776064c95a74804f519993f36b87df911e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2019
ms.locfileid: "71340346"
---
# <a name="storyboarded-animations"></a>Storyboardanimationen

Storyboardanimationen sind nicht nur Animationen visueller Art. Eine Storyboardanimation bietet die Möglichkeit, den Wert einer Abhängigkeitseigenschaft zeitabhängig zu ändern. Einer der Hauptgründe für eine Storyboardanimation, die nicht aus der Animationsbibliothek stammt, ist die Definition des visuellen Zustands für ein Steuerelement als Teil einer Steuerelementvorlage oder Seitendefinition.

## <a name="differences-with-silverlight-and-wpf"></a>Unterschiede zu Silverlight und WPF

Lesen Sie diesen Abschnitt, wenn Sie mit Microsoft Silverlight or Windows Presentation Foundation (WPF) vertraut sind. Ansonsten können Sie diesen Abschnitt überspringen.

Grundsätzlich werden Storyboardanimationen in Windows-Runtime-Apps wie in Silverlight oder WPF erstellt. Es gibt jedoch einige wichtige Unterschiede:

-   Storyboardanimationen sind nicht die einzige Möglichkeit zum visuellen Animieren einer UI und stellen in dieser Beziehung auch nicht unbedingt die einfachste Möglichkeit für Entwickler dar. Anstelle von Storyboardanimationen empfiehlt sich als bessere Entwurfsalternative häufig die Verwendung von Design- und Übergangsanimationen. Dabei können die empfohlenen UI-Animationen schnell erstellt werden, ohne sich mit den Details der Zielgruppenadressierung für Animationseigenschaften auseinandersetzen zu müssen. Weitere Informationen finden Sie unter [Übersicht über Animationen](xaml-animation.md).
-   In der Windows-Runtime enthalten viele XAML-Steuerelemente im Rahmen des integrierten Verhaltens Design- und Übergangsanimationen. WPF- und Silverlight-Steuerelemente haben bisher meist nicht über ein Standardverhalten für Animationen verfügt.
-   Nicht alle von Ihnen erstellten benutzerdefinierten Animationen können standardmäßig in einer Windows-Runtime-App ausgeführt werden, wenn das Animationssystem ermittelt, dass die Animation die Leistung Ihrer Benutzeroberfläche beeinträchtigen kann. Animationen, für die das System eine mögliche Leistungsbeeinträchtigung erkennt, werden als *abhängige Animationen* bezeichnet. Sie sind abhängig, weil die Taktung Ihrer Animationen direkt mit dem UI-Thread arbeitet, wo auch aktive Benutzereingaben und andere Aktualisierungen versuchen, die Laufzeitänderungen auf die Benutzeroberfläche anzuwenden. Eine abhängige Animation, die sehr viele Systemressourcen im UI-Thread verbraucht, kann dafür sorgen, dass eine App in bestimmten Situationen scheinbar nicht mehr reagiert. Wenn eine Animation Layoutänderungen bedingt oder die Leistung im UI-Thread potenziell auf andere Weise beeinträchtigen kann, müssen Sie die Animation häufig explizit aktivieren, um die Ausführung zu ermöglichen. Hierzu gibt es die **EnableDependentAnimation**-Eigenschaft für bestimmte Animationsklassen. Weitere Informationen finden Sie unter [Abhängige und unabhängige Animationen](./storyboarded-animations.md#dependent-and-independent-animations).
-   Benutzerdefinierte Beschleunigungsfunktionen werden in der Windows-Runtime derzeit nicht unterstützt.

## <a name="defining-storyboarded-animations"></a>Definieren von Storyboardanimationen

Eine Storyboardanimation bietet die Möglichkeit, den Wert einer Abhängigkeitseigenschaft zeitabhängig zu ändern. Die Eigenschaft, die Sie animieren, ist nicht immer eine Eigenschaft, die sich direkt auf die UI der App auswirkt. Da es bei XAML jedoch um das Definieren einer UI für eine App geht, ist es in der Regel eine UI-bezogene Eigenschaft, die Sie animieren. Beispielsweise können Sie den Winkel eines [**RotateTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.RotateTransform)-Elements oder den Farbwert eines Schaltflächenhintergrunds animieren.

Einer der Hauptgründe für die Definition einer Storyboardanimation ist, wenn Sie als Steuerelementautor oder beim Erstellen einer neuen Vorlage für ein Steuerelement visuelle Zustände definieren. Weitere Informationen finden Sie unter [Storyboardanimationen für visuelle Zustände](https://docs.microsoft.com/previous-versions/windows/apps/jj819808(v=win.10)).

Die Konzepte und APIs für Storyboardanimationen, die in diesem Thema beschrieben werden, gelten unabhängig davon, ob Sie visuelle Zustände oder eine benutzerdefinierte Animation für eine App definieren.

Damit eine Eigenschaft animiert werden kann, muss es sich bei der Eigenschaft, auf die Sie mit einer Storyboardanimation abzielen, um eine *Abhängigkeitseigenschaft* handeln. Eine Abhängigkeitseigenschaft ist ein wichtiges Feature der Windows-Runtime-XAML-Implementierung. Die schreibbaren Eigenschaften der häufigsten UI-Elemente werden normalerweise als Abhängigkeitseigenschaften implementiert, damit Sie diese animieren, datengebundene Werte anwenden oder ein [**Style**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Style)-Element anwenden und mit einem [**Setter**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Setter)-Element auf die Eigenschaft verweisen können. Weitere Informationen zur Funktionsweise von Abhängigkeitseigenschaften finden Sie unter [Übersicht über Abhängigkeitseigenschaften](https://docs.microsoft.com/windows/uwp/xaml-platform/dependency-properties-overview).

In den meisten Fällen definieren Sie eine Storyboardanimation, indem Sie XAML-Code schreiben. Wenn Sie ein Tool wie Microsoft Visual Studio verwenden, können Sie den XAML-Code vom Tool erzeugen lassen. Es ist auch möglich, eine Storyboardanimation mithilfe von normalem Code zu definieren. Dies ist jedoch keine gängige Lösung.

Sehen wir uns dazu ein einfaches Beispiel an. In diesem XAML-Beispiel wird die [**Opacity**](/uwp/api/Windows.UI.Xaml.UIElement.Opacity)-Eigenschaft auf einem bestimmten [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle)-Objekt animiert.

```xaml
<Page ...>
  <Page.Resources>
    <!-- Storyboard resource: Animates a rectangle's opacity. -->
    <Storyboard x:Name="myStoryboard">
      <DoubleAnimation
        Storyboard.TargetName="MyAnimatedRectangle"
        Storyboard.TargetProperty="Opacity"
        From="1.0" To="0.0" Duration="0:0:1"/>
    </Storyboard>
  </Page.Resources>

  <!--Page root element, UI definition-->
  <Grid>
    <Rectangle x:Name="MyAnimatedRectangle"
      Width="300" Height="200" Fill="Blue"/>
  </Grid>
</Page>
```

### <a name="identifying-the-object-to-animate"></a>Angeben des zu animierenden Objekts

Im vorherigen Beispiel hat das Storyboard die [**Opacity**](/uwp/api/Windows.UI.Xaml.UIElement.Opacity)-Eigenschaft eines [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle)-Objekts animiert. Dabei deklarieren Sie nicht die Animationen des Objekts selbst. Stattdessen führen Sie dies in der Animationsdefinition eines Storyboards durch. Storyboards werden normalerweise in XAML-Code definiert, der sich nicht in unmittelbarer Nähe der XAML-UI-Definition des zu animierenden Objekts befindet. Stattdessen werden sie meist als XAML-Ressource eingerichtet.

Um eine Animation mit einem Ziel zu verbinden, verweisen Sie anhand des identifizierenden Programmierungsnamens auf das Ziel. Sie sollten stets das [x:Name-Attribut](https://docs.microsoft.com/windows/uwp/xaml-platform/x-name-attribute) in der XAML-UI-Definition anwenden, um das Objekt zu benennen, das Sie animieren möchten. Anschließend geben Sie das zu animierende Objekt als Ziel an, indem Sie [**Storyboard.TargetName**](https://docs.microsoft.com/dotnet/api/system.windows.media.animation.storyboard.targetname) in der Animationsdefinition festlegen. Für den Wert von **Storyboard.TargetName** verwenden Sie die Namenszeichenfolge des Zielobjekts. Dies ist die Zeichenfolge, die Sie bereits an anderer Stelle mit dem x:Name-Attribut festgelegt haben.

### <a name="targeting-the-dependency-property-to-animate"></a>Angeben der zu animierenden Abhängigkeitseigenschaft

Sie legen einen Wert für [**Storyboard.TargetProperty**](https://docs.microsoft.com/previous-versions/windows/silverlight/dotnet-windows-silverlight/ms616983(v=vs.95)) in der Animation fest. Damit wird bestimmt, welche spezifische Eigenschaft des Zielobjekts animiert wird.

Es kann vorkommen, dass Sie eine Eigenschaft als Ziel angeben müssen, bei der es sich nicht um eine direkte Eigenschaft des Zielobjekts handelt, sondern die tiefer in einer Beziehung zwischen Objekt und Eigenschaft geschachtelt ist. Dies ist häufig erforderlich, um Detailinformationen für eine Gruppe von beitragenden Objekt- und Eigenschaftswerten anzuzeigen, bis Sie auf einen Eigenschaftstyp verweisen können, der animiert werden kann ([**Double**](https://docs.microsoft.com/dotnet/api/system.double), [**Point**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Point), [**Color**](https://docs.microsoft.com/uwp/api/Windows.UI.Color)). Dieses Konzept wird als *indirekte Zielbestimmung* bezeichnet. Um eine Eigenschaft auf diese Art als Ziel auszuwählen, wird ein *Eigenschaftspfad* verwendet.

Hier sehen Sie ein Beispiel. Ein häufiges Szenario für eine Storyboardanimation ist eine Farbänderung für einen Teil einer App-UI oder eines Steuerelements, um anzuzeigen, dass sich das Steuerelement in einem bestimmten Zustand befindet. Angenommen, Sie möchten das [**Foreground**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.foreground)-Element eines [**TextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock)-Elements so animieren, dass sich die Farbe von Rot in Grün ändert. Wenn Sie erwarten, dass hierbei ein [**ColorAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.ColorAnimation)-Element verwendet wird, liegen Sie richtig. Keine der Eigenschaften von UI-Elementen, die sich auf die Farbe des Objekts auswirken, weisen jedoch den Typ [**Color**](https://docs.microsoft.com/uwp/api/Windows.UI.Color) auf. Stattdessen weisen sie den Typ [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush) auf. Das eigentliche Ziel der Animation sollte also die [**Color**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush.Color)-Eigenschaft der [**SolidColorBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush)-Klasse sein. Dabei handelt es sich um einen von **Brush** abgeleiteten Typ, der normalerweise für diese farbbezogenen UI-Eigenschaften verwendet wird. In Bezug auf die Erstellung eines Eigenschaftspfads für die Eigenschaftsangabe der Animation sieht dies wie folgt aus:

```xaml
<Storyboard x:Name="myStoryboard">
  <ColorAnimation
    Storyboard.TargetName="tb1"
    Storyboard.TargetProperty="(TextBlock.Foreground).(SolidColorBrush.Color)"
    From="Red" To="Green"/>
</Storyboard>
```

Stellen Sie sich die Syntax hinsichtlich ihrer Komponenten folgendermaßen vor:

- Jedes Klammerpaar "()" umschließt jeweils einen Eigenschaftsnamen.
- Der Eigenschaftsname enthält einen Punkt zur Trennung eines Typnamens und eines Eigenschaftsnamens, damit die angegebene Eigenschaft eindeutig ist.
- Der Punkt in der Mitte, der sich nicht innerhalb der Klammern befindet, ist ein Schritt. Dies wird von der Syntax wie folgt interpretiert: Verwende den Wert der ersten Eigenschaft (ein Objekt), greife auf sein Objektmodell zu und gib eine bestimmte Untereigenschaft des Werts der ersten Eigenschaft an.

Unten ist eine Liste mit Szenarien zum Angeben von Animationszielen aufgeführt, bei denen Sie Eigenschaften normalerweise indirekt angeben. Außerdem sind einige Eigenschaftspfadzeichenfolgen aufgeführt, die an die zu verwendende Syntax angenähert sind:

- Animation des [**X**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.translatetransform.x)-Werts eines [**TranslateTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.TranslateTransform)-Elements bei Anwendung auf [**RenderTransform**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.rendertransform): `(UIElement.RenderTransform).(TranslateTransform.X)`
- Animation eines [**Color**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush.Color)-Elements innerhalb eines [**GradientStop**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.GradientStop)-Elements eines [**LinearGradientBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.LinearGradientBrush)-Elements bei Anwendung auf [**Fill**](/uwp/api/Windows.UI.Xaml.Shapes.Shape.Fill): `(Shape.Fill).(GradientBrush.GradientStops)[0].(GradientStop.Color)`
- Animation des [**X**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.translatetransform.x)-Werts eines [**TranslateTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.TranslateTransform)-Elements, das eine von vier Transformationen in einem [**TransformGroup**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.TransformGroup)-Element darstellt, bei Anwendung auf [**RenderTransform**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.rendertransform): `(UIElement.RenderTransform).(TransformGroup.Children)[3].(TranslateTransform.X)`

Sie sehen, dass Zahlen in einigen Beispielen in eckige Klammern gesetzt sind. Dabei handelt es sich um einen Indexer. Damit wird angegeben, dass der davor stehende Eigenschaftsname als Wert eine Sammlung aufweist und dass Sie ein Element dieser Sammlung (Angabe per nullbasiertem Index) verwenden möchten.

Sie können auch angefügte XAML-Eigenschaften animieren. Setzen Sie den vollständigen Namen der angefügten Eigenschaft stets in Klammern, z. B. `(Canvas.Left)`. Weitere Informationen finden Sie unter [Animieren von angefügten XAML-Eigenschaften](./storyboarded-animations.md#animating-xaml-attached-properties).

Weitere Informationen zur Verwendung eines Eigenschaftenpfads für die indirekte Auswahl der zu animierenden Eigenschaft finden Sie unter [Property-path-Syntax](https://docs.microsoft.com/windows/uwp/xaml-platform/property-path-syntax) oder [**Angefügte Storyboard.TargetProperty-Eigenschaft**](https://docs.microsoft.com/previous-versions/windows/silverlight/dotnet-windows-silverlight/ms616983(v=vs.95)).

### <a name="animation-types"></a>Animationstypen

Das Windows-Runtime-Animationssystem verfügt über drei spezielle Typen für Storyboardanimationen:

-   [**Double**](https://docs.microsoft.com/dotnet/api/system.double), kann mit jeder [ **DoubleAnimation** animiert werden.](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.DoubleAnimation)
-   [**Point**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Point), kann mit jeder [ **pointanimung** animiert werden.](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PointAnimation)
-   [**Farbe**](https://docs.microsoft.com/uwp/api/Windows.UI.Color), kann mit allen [ **ColorAnimation** animiert werden](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.ColorAnimation)

Es ist auch ein generalisierter [**Object**](https://docs.microsoft.com/dotnet/api/system.object)-Animationstyp für Objektverweiswerte vorhanden, der später erläutert wird.

### <a name="specifying-the-animated-values"></a>Angeben der animierten Werte

Bisher wurde beschrieben, wie Sie das Objekt und die Eigenschaft für die Animation angeben, aber es wurde noch nicht erläutert, wie sich die Animation während der Ausführung auf den Eigenschaftswert auswirkt.

Die beschriebenen Animationstypen werden auch als **From**/**To**/**By**-Animationen bezeichnet. Dies bedeutet, dass die Animation den Wert einer Eigenschaft in Abhängigkeit der Zeit ändert, indem eine oder mehrere dieser Eingaben aus der Animationsdefinition verwendet werden:

-   Der Wert beginnt beim **From**-Wert. Wenn Sie keinen **From**-Wert angeben, wird als Startwert der Wert genutzt, den die animierte Eigenschaft jeweils vor dem Ausführen der Animation aufweist. Dies kann ein Standardwert, ein Wert aus einem Stil oder einer Vorlage oder ein Wert sein, der speziell von einer XAML-UI-Definition oder einem App-Code angewendet wird.
-   Am Ende der Animation ist der Wert der **To**-Wert.
-   Sie können auch die **By**-Eigenschaft festlegen, um einen Endwert relativ zum Startwert anzugeben. Legen Sie diese Eigenschaft dazu anstelle der **To**-Eigenschaft fest.
-   Wenn Sie keinen **To**-Wert oder einen **By**-Wert angeben, wird als Endwert der Wert verwendet, den die animierte Eigenschaft vor dem Ausführen der Animation aufweist. In diesem Fall empfiehlt sich die Nutzung eines **From**-Werts, weil die Animation den Wert ansonsten gar nicht ändert, da der Start- und Endwert identisch sind.
-   Eine Animation verfügt normalerweise über mindestens ein **From**-, **By**- oder **To**-Element, aber nicht über alle drei.

Sehen wir uns das vorherige XAML-Beispiel und die **From**- und **To**-Werte sowie **Duration** erneut an. Im Beispiel wird die [**Opacity**](/uwp/api/Windows.UI.Xaml.UIElement.Opacity)-Eigenschaft animiert, und der Eigenschaftstyp von **Opacity** lautet [**Double**](https://docs.microsoft.com/dotnet/api/system.double). Als Animation muss hier also [**DoubleAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.DoubleAnimation) verwendet werden.

Mit `From="1.0" To="0.0"` wird angegeben, dass die [**Opacity**](/uwp/api/Windows.UI.Xaml.UIElement.Opacity)-Eigenschaft während der Ausführung der Animation mit dem Wert 1 beginnt und zu 0 animiert wird. Anders ausgedrückt: Hinsichtlich der Bedeutung dieser [**Double**](https://docs.microsoft.com/dotnet/api/system.double)-Werte für die **Opacity**-Eigenschaft bewirkt diese Animation, dass das Objekt undurchsichtig gestartet wird und dann transparent wird.

```xaml
...
<Storyboard x:Name="myStoryboard">
  <DoubleAnimation
    Storyboard.TargetName="MyAnimatedRectangle"
    Storyboard.TargetProperty="Opacity"
    From="1.0" To="0.0" Duration="0:0:1"/>
</Storyboard>
...
```

`Duration="0:0:1"` gibt die Dauer der Animation an, also die Geschwindigkeit, mit der das Rechteck verblasst. Eine [**Duration**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.timeline.duration)-Eigenschaft wird in der Form *Stunden*:*Minuten*:*Sekunden* angegeben. Die Zeitdauer in diesem Beispiel beträgt eine Sekunde.

Weitere Informationen zu [**Duration**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Duration)-Werten und der XAML-Syntax finden Sie unter [**Duration**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Duration).

> [!NOTE]
> Wenn Sie sich in Bezug auf das gezeigte Beispiel sicher sind, dass der Startzustand des animierten Objekts für [**Opacity**](/uwp/api/Windows.UI.Xaml.UIElement.Opacity) stets den Wert „1“ aufweist (durch standardmäßige oder explizite Festlegung), können Sie den **From**-Wert auslassen. Die Animation verwendet dann den impliziten Startwert, und das Ergebnis ist identisch.

### <a name="fromtoby-are-nullable"></a>From/To/By akzeptieren NULL-Werte

Es wurde bereits erwähnt, dass Sie **From**, **To** oder **By** weglassen können und so aktuelle nicht animierte Werte als Ersatzwerte für einen fehlenden Wert verwenden können. **From**, **To** oder **By**-Eigenschaften einer Animation sind möglicherweise von einem anderen Typ, als Sie vermuten. Der Typ der [**DoubleAnimation.To**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.doubleanimation.easingfunction)-Eigenschaft lautet beispielsweise nicht [**Double**](https://docs.microsoft.com/dotnet/api/system.double). Stattdessen gilt [**Nullable**](https://docs.microsoft.com/dotnet/api/system.nullable-1) für **Double**. Der Standardwert lautet **null**, nicht 0. Anhand dieses **null**-Werts kann das Animationssystem unterscheiden, dass Sie keinen spezifischen Wert für eine **From**-, **To**- oder **By**-Eigenschaft festgelegt haben. Für C++ Visual Component ExtensionsC++(/CX) gibt es keinen Typ, der **NULL-Werte** zulässt, weshalb stattdessen [IReference](https://docs.microsoft.com/uwp/api/Windows.Foundation.IReference_T_) verwendet wird.

### <a name="other-properties-of-an-animation"></a>Andere Eigenschaften einer Animation

Die nächsten in diesem Abschnitt beschriebenen Eigenschaften sind alle optional, da sie Standardwerte aufweisen, die für die meisten Animationen geeignet sind.

### <a name="autoreverse"></a>**AutoReverse**

Wenn Sie für eine Animation entweder [**AutoReverse**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.timeline.autoreverse) oder [**RepeatBehavior**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.timeline.repeatbehavior) angeben, wird die Animation einmal ausgeführt, und als Dauer wird die Angabe unter [**Duration**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.timeline.duration) verwendet.

Mit der [**AutoReverse**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.timeline.autoreverse)-Eigenschaft wird angegeben, ob eine Zeitachse rückwärts wiedergegeben wird, nachdem das Ende von [**Duration**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.timeline.duration) erreicht wurde. Wenn Sie diese Eigenschaft auf **true** festlegen, wird die Animation nach dem Erreichen des Endes ihrer deklarierten Dauer ([**Duration**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.timeline.duration)) umgekehrt, sodass der Wert von seinem Endwert (**To**) wieder in den Startwert (**From**) geändert wird. Dies bedeutet, dass die Animation praktisch doppelt so lange wie im [**Duration**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.timeline.duration)-Element angegeben ausgeführt wird.

### <a name="repeatbehavior"></a>**RepeatBehavior**

Mit der [**RepeatBehavior**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.timeline.repeatbehavior)-Eigenschaft wird entweder festgelegt, wie häufig eine Zeitachse wiedergegeben wird, oder eine längere Dauer, während der die Zeitachse wiederholt wird. Standardmäßig gilt für eine Zeitachse ein Durchlaufwert von "1x". Dies bedeutet, dass sie gemäß dem [**Duration**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.timeline.duration)-Element einmal ausgeführt und nicht wiederholt wird.

Sie können die Animation dazu veranlassen, mehrere Durchläufe auszuführen. Ein Wert von „3x” bewirkt beispielsweise, dass die Animation dreimal ausgeführt wird. Alternativ dazu können Sie eine andere Dauer ([**Duration**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Duration)) für [**RepeatBehavior**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.timeline.repeatbehavior) angeben. Es ist ratsam, diesen **Duration**-Wert auf einen längeren Wert als den **Duration**-Wert der Animation selbst festzulegen. Wenn Sie z. B. für **RepeatBehavior** den Wert "0:0:10" festlegen und die Animation für [**Duration**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.timeline.duration) den Wert "0:0:2" aufweist, wird die Animation fünfmal wiederholt. Falls diese Werte nicht ohne Rest teilbar sind, wird die Animation an dem Punkt abgeschnitten, an dem der **RepeatBehavior**-Zeitpunkt erreicht wird, also z. B. auch nach der Hälfte der Animation. Außerdem haben Sie noch die Möglichkeit, den Spezialwert „Forever” anzugeben, bei dem die Animation endlos lange ausgeführt wird, bis sie explizit beendet wird.

Weitere Informationen zu [**RepeatBehavior**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.RepeatBehavior)-Werten und zur XAML-Syntax finden Sie unter [**RepeatBehavior**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.RepeatBehavior).

### <a name="fillbehaviorstop"></a>**FillBehavior = "Ende"**

Wenn eine Animation endet, belässt die Animation den Eigenschaftswert standardmäßig auf dem letzten per **To** oder **By** geänderten Wert. Dies gilt auch, wenn die Dauer abgelaufen ist. Wenn Sie den Wert der [**FillBehavior**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.timeline.fillbehavior)-Eigenschaft jedoch auf [**FillBehavior.Stop**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.FillBehavior) festlegen, wird der Wert des animierten Werts auf den Stand vor dem Anwenden der Animation zurückgesetzt. Genauer gesagt: Er wird auf den aktuell geltenden Wert zurückgesetzt, der vom Abhängigkeitseigenschaftensystem bestimmt wird. (Weitere Informationen zu dieser Unterscheidung finden Sie unter [Übersicht über Abhängigkeitseigenschaften](https://docs.microsoft.com/windows/uwp/xaml-platform/dependency-properties-overview).)

### <a name="begintime"></a>**BeginTime**

Standardmäßig lautet die [**BeginTime**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.timeline.begintime) einer Animation „0:0:0“. Die Animation beginnt daher, sobald das enthaltende [**Storyboard**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.Storyboard) ausgeführt wird. Sie können dies ändern, falls das **Storyboard** mehr als eine Animation enthält und Sie die Startzeiten der anderen Animationen gegenüber der ersten Animation staffeln möchten. Außerdem können Sie auch eine absichtliche kurze Verzögerung einbauen.

### <a name="speedratio"></a>**SpeedRatio**

Wenn Sie mehr als eine Animation in einem [**Storyboard**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.Storyboard) verwenden, können Sie die Zeitrate für eine oder mehrere Animationen relativ zum **Storyboard** ändern. Letztendlich wird über das übergeordnete **Storyboard** gesteuert, wie die Zeitdauer ([**Duration**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Duration)) während der Ausführung der Animationen verstreicht. Diese Eigenschaft wird nicht sehr häufig verwendet. Weitere Informationen finden Sie unter [**SpeedRatio**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.timeline.speedratio).

## <a name="defining-more-than-one-animation-in-a-storyboard"></a>Definieren mehrerer Animationen in einem **Storyboard**

Ein [**Storyboard**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.Storyboard) kann mehr als eine Animationsdefinition enthalten. Möglicherweise verwenden Sie z. B. mehr als eine Animation, wenn Sie verwandte Animationen auf zwei Eigenschaften desselben Zielobjekts anwenden. Sie können zum Beispiel die [**TranslateX**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.compositetransform.translatex)- und [**TranslateY**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.compositetransform.translatey)-Eigenschaft eines [**TranslateTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.TranslateTransform)-Elements ändern, das als [**RenderTransform**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.rendertransform)-Element eines Steuerelements der UI verwendet wird. Dies führt dazu, dass das Element diagonal übersetzt wird. Dafür benötigen Sie zwei unterschiedliche Animationen. Es kann jedoch ratsam sein, Animationen zu nutzen, die Teil desselben **Storyboard**-Elements sind, weil die beiden Animationen immer zusammen ausgeführt werden sollen.

Die Animationen müssen nicht denselben Typ oder dasselbe Objekt als Ziel aufweisen. Ihre Dauer kann unterschiedlich sein, und es ist keine gemeinsame Nutzung von Eigenschaftswerten erforderlich.

Wenn das übergeordnete [**Storyboard**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.Storyboard) ausgeführt wird, werden auch die beiden Animationen ausgeführt.

Die [**Storyboard**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.Storyboard)-Klasse verfügt zudem über viele gleiche Animationseigenschaften wie die Animationstypen, weil von beiden die [**Timeline**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.Timeline)-Basisklasse verwendet wird. Ein **Storyboard** kann also ein [**RepeatBehavior**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.timeline.repeatbehavior)-Element oder ein [**BeginTime**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.timeline.begintime)-Element aufweisen. Sie legen diese Elemente jedoch normalerweise nicht in einem **Storyboard** fest, es sei denn, alle darin enthaltenen Animationen sollen das entsprechende Verhalten aufweisen. Es gilt die allgemeine Regel, dass jede **Timeline**-Eigenschaft, die in einem **Storyboard** festgelegt wird, für alle untergeordneten Animationen gilt. Wenn hierfür nichts festgelegt wird, hat das **Storyboard** eine implizite Dauer, die aus dem längsten [**Duration**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Duration)-Wert der enthaltenen Animationen berechnet wird. Ein explizit festgelegtes [**Duration**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.timeline.duration)-Element in einem **Storyboard**, das kürzer als eine seiner untergeordneten Animationen ist, führt dazu, dass die Animation abgeschnitten wird. Dies ist normalerweise nicht wünschenswert.

Ein Storyboard kann nicht zwei Animationen enthalten, die jeweils versuchen, dieselbe Eigenschaft desselben Objekts anzusprechen und zu animieren. Wenn Sie dies versuchen, tritt beim Ausführen des Storyboards ein Laufzeitfehler auf. Diese Einschränkung gilt auch, wenn sich die Animationen zeitlich nicht überlappen, weil absichtlich unterschiedliche [**BeginTime**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.timeline.begintime)-Werte und Dauern verwendet werden. Wenn Sie in einem einzelnen Storyboard eine komplexere Animationszeitachse auf die gleiche Eigenschaft anwenden möchten, können Sie eine Keyframeanimation nutzen. Siehe [Keyframeanimationen und Animationen für Beschleunigungsfunktionen](key-frame-and-easing-function-animations.md).

Das Animationssystem kann mehr als eine Animation auf den Wert einer Eigenschaft anwenden, wenn diese Eingaben von mehreren Storyboards stammen. Dieses Verhalten wird für gleichzeitig ausgeführte Storyboards in der Regel nicht verwendet. Mithilfe einer von einer App definierten Animation, die Sie auf eine Steuerelementeigenschaft anwenden, kann jedoch der **HoldEnd**-Wert einer Animation geändert werden, die als Teil des Modells für den visuellen Zustand des Steuerelements bereits ausgeführt wurde.

## <a name="defining-a-storyboard-as-a-resource"></a>Definieren eines Storyboards als Ressource

Ein [**Storyboard**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.Storyboard) ist der Container, in den Sie Animationsobjekte einschließen. Normalerweise definieren Sie das **Storyboard** als Ressource, die für das zu animierende Objekt verfügbar ist, und zwar entweder unter [**Resources**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.resources) auf Seitenebene oder unter [**Application.Resources**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.resources).

Im nächsten Beispiel wird gezeigt, wie das [**Storyboard**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.Storyboard) aus dem vorherigen Beispiel in eine [**Resources**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.resources)-Definition auf Seitenebene eingeschlossen wird, bei der das **Storyboard** eine Ressource mit Schlüssel des [**Page**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page)-Stammelements darstellt. Beachten Sie das [x:Name-Attribut](https://docs.microsoft.com/windows/uwp/xaml-platform/x-name-attribute). Mit diesem Attribut definieren Sie einen Variablennamen für das **Storyboard**, damit andere Elemente im XAML-Code und in normalem Code später auf das **Storyboard** verweisen können.

```xaml
<Page ...>
  <Page.Resources>
    <!-- Storyboard resource: Animates a rectangle's opacity. -->
    <Storyboard x:Name="myStoryboard">
      <DoubleAnimation
        Storyboard.TargetName="MyAnimatedRectangle"
        Storyboard.TargetProperty="Opacity"
        From="1.0" To="0.0" Duration="0:0:1"/>
    </Storyboard>
  </Page.Resources>
  <!--Page root element, UI definition-->
  <Grid>
    <Rectangle x:Name="MyAnimatedRectangle"
      Width="300" Height="200" Fill="Blue"/>
  </Grid>
</Page>
```

Das Definieren von Ressourcen auf der XAML-Stammebene einer XAML-Datei, z. B. page.xaml oder app.xaml, ist eine übliche Vorgehensweise zum Organisieren von Ressourcen mit Schlüsseln in XAML. Sie können Ressourcen auch auf separate Dateien aufteilen und diese in Apps oder Seiten zusammenführen. Weitere Informationen finden Sie unter [ResourceDictionary- und XAML-Ressourcenreferenzen](https://docs.microsoft.com/windows/uwp/controls-and-patterns/resourcedictionary-and-xaml-resource-references).

> [!NOTE]
> Windows-Runtime-XAML unterstützt die Identifizierung von Ressourcen unter Verwendung des [x:Key-Attributs](https://docs.microsoft.com/windows/uwp/xaml-platform/x-key-attribute) oder des [x:Name-Attributs](https://docs.microsoft.com/windows/uwp/xaml-platform/x-name-attribute). Das x:Name-Attribut wird für ein [**Storyboard**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.Storyboard) häufiger verwendet, weil darauf später anhand des Variablennamens verwiesen werden soll, damit Sie dessen [**Begin**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.storyboard.begin)-Methode aufrufen und die Animationen ausführen können. Wenn Sie dagegen das [x:Key-Attribut](https://docs.microsoft.com/windows/uwp/xaml-platform/x-key-attribute) verwenden, müssen Sie [**ResourceDictionary**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.ResourceDictionary)-Methoden wie den [**Item**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.resourcedictionary.item)-Indexer zum Abrufen als Ressource mit Schlüssel nutzen und das abgerufene Objekt dann für das **Storyboard** umwandeln, um die **Storyboard**-Methoden zu verwenden.

### <a name="storyboards-for-visual-states"></a>Storyboards für Animationen

Sie können Ihre Animationen auch in einer [**Storyboard**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.Storyboard)-Einheit platzieren, wenn Sie die Ansichtszustandsanimationen für die visuelle Gestaltung eines Steuerelements deklarieren. In diesem Fall kommen die definierten **Storyboard**-Elemente in einen [**VisualState**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.VisualState)-Container, der tiefer in einem [**Style**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Style) verschachtelt ist (der **Style** ist die Schlüsselressource). Sie benötigen in diesem Fall keinen Schlüssel oder Namen für **Storyboard**, da **VisualState** einen Zielnamen besitzt, den [**VisualStateManager**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.visualstatemanager) aufrufen kann. Die Stile für Steuerelemente werden häufig auf separate XAML-[**ResourceDictionary**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.ResourceDictionary)-Dateien aufgeteilt, statt dass sie in eine Seiten- oder App-**Ressources**-Sammlung platziert werden. Weitere Informationen finden Sie unter [Storyboardanimationen für visuelle Zustände](https://docs.microsoft.com/previous-versions/windows/apps/jj819808(v=win.10)).

## <a name="dependent-and-independent-animations"></a>Abhängige und unabhängige Animationen

An diesem Punkt ist es sinnvoll, einige wichtige Aspekte zur Funktionsweise des Animationssystems zu erläutern. Besonders wichtig ist, dass sich die grundlegende Interaktion der Animation danach richtet, wie eine Windows-Runtime-App auf dem Bildschirm gerendert wird und wie beim Rendern Verarbeitungsthreads eingesetzt werden. Eine Windows-Runtime-App verfügt immer über einen UI-Hauptthread, der für die Aktualisierung des Bildschirms mit den aktuellen Informationen zuständig ist. Außerdem verfügt eine Windows-Runtime-App über einen Kompositionsthread, der zum Vorabberechnen von Layouts unmittelbar vor dem Anzeigen verwendet wird. Wenn Sie die UI animieren, kann dies für den UI-Thread einen erhöhten Arbeitsaufwand bedeuten. Das System muss große Bereiche des Bildschirms neu zeichnen, wobei relativ kurze Zeitintervalle zwischen den Aktualisierungen verwendet werden. Dies ist erforderlich, um den aktuellen Eigenschaftswert der animierten Eigenschaft zu erfassen. Wenn Sie dabei nicht sorgfältig vorgehen, besteht das Risiko, dass die Reaktionsfähigkeit der UI aufgrund einer Animation sinkt. Außerdem kann die Leistung anderer App-Features beeinträchtigt werden, die sich ebenfalls auf demselben UI-Thread befinden.

Die Animationsvariante, für die das Risiko einer Verlangsamung des UI-Threads ermittelt wird, wird als *abhängige Animation* bezeichnet. Eine Animation, für die dieses Risiko nicht besteht, ist eine *unabhängige Animation*. Die Unterscheidung zwischen abhängigen und unabhängigen Animationen basiert nicht nur auf den Animationstypen ([**DoubleAnimation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.DoubleAnimation) usw.), wie bereits beschrieben. Stattdessen wird untersucht, welche speziellen Eigenschaften Sie animieren, und es wird auf andere Faktoren wie die Vererbung und die Zusammensetzung der Steuerelemente geachtet. Es gibt Fälle, in denen eine Animation auch dann nur eine geringe Auswirkung auf den UI-Thread haben kann, wenn die UI von der Animation geändert wird. Sie kann stattdessen vom Kompositionsthread als unabhängige Animation behandelt werden.

Eine Animation ist unabhängig, wenn Sie diese Merkmale aufweist:

-   Das [**Duration**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.timeline.duration)-Element der Animation hat einen Wert von 0 Sekunden (siehe Warnhinweis).
-   Die Animation hat [**UIElement.Opacity**](/uwp/api/Windows.UI.Xaml.UIElement.Opacity) als Ziel.
-   Die Animation zielt auf einen untergeordneten Eigenschafts Wert dieser [**UIElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement) -Eigenschaften ab: [**Transform3D**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.transform3d), [**RenderTransform**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.rendertransform), [**Projektion**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.projection), [**Clip**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.clip)
-   Die Animation hat [**Canvas.Left**](https://docs.microsoft.com/dotnet/api/system.windows.controls.canvas.left) oder [**Canvas.Top**](https://docs.microsoft.com/dotnet/api/system.windows.controls.canvas.top) als Ziel.
-   Die Animation hat einen [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush)-Wert als Ziel und verwendet ein [**SolidColorBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush)-Element, für das [**Color**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush.Color) animiert wird.
-   Die Animation ist ein [**ObjectAnimationUsingKeyFrames**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.ObjectAnimationUsingKeyFrames)-Element.

> [!WARNING]
> Damit Ihre Animation als unabhängige Animation behandelt wird, müssen Sie `Duration="0"` explizit festlegen. Wenn Sie z. B. `Duration="0"` aus diesem XAML-Beispiel entfernen, wird die Animation als abhängige Animation behandelt, obwohl die [**KeyTime**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.doublekeyframe.keytime) des Frames "0:0:0" ist.

```xaml
<Storyboard>
  <DoubleAnimationUsingKeyFrames
    Duration="0"
    Storyboard.TargetName="Button2"
    Storyboard.TargetProperty="Width">
    <DiscreteDoubleKeyFrame KeyTime="0:0:0" Value="200"/>
  </DoubleAnimationUsingKeyFrames>
</Storyboard>
```

Wenn Ihre Animation diese Kriterien nicht erfüllt, handelt es sich wahrscheinlich um eine abhängige Animation. Vom Animationssystem wird eine abhängige Animation standardmäßig nicht ausgeführt. Es kann also sein, dass Sie die Ausführung der Animation während der Entwicklungs- und Testphase nicht beobachten können. Sie können die Animation trotzdem verwenden, aber Animationen dieser Art müssen einzeln aktiviert werden. Zum Aktivieren der Animation legen Sie die **EnableDependentAnimation**-Eigenschaft des Animationsobjekts auf **true** fest. (Jede [**Timeline**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.Timeline)-Unterklasse, die eine Animation darstellt, weist eine andere Implementierung der Eigenschaft auf, aber alle tragen den Namen `EnableDependentAnimation`.)

Die erforderliche Aktivierung abhängiger Animationen durch den App-Entwickler ist ein bewusst integrierter Entwurfsaspekt des Animationssystems und der Entwicklungsumgebung. Damit soll Entwicklern bewusst gemacht werden, dass Animationen einen Leistungsaufwand bedeuten, der die Reaktionsfähigkeit der UI beeinträchtigen kann. In einer kompletten App lassen sich Animationen mit schlechter Leistung nur schwer isolieren und debuggen. Daher ist es besser, nur die abhängigen Animationen zu aktivieren, die Sie für die UI der App wirklich benötigen. Damit sollte es den Entwicklern nicht zu einfach gemacht werden, die Leistung von Apps aufgrund von dekorativen Animationen, für die viele Zyklen erforderlich sind, zu verschlechtern. Weitere Informationen zu Leistungstipps für Animationen finden Sie unter [Optimieren von Animationen und Medien](https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-animations-and-media).

Als App-Entwickler können Sie sich auch für die Anwendung einer App-weiten Einstellung entscheiden, mit der abhängige Animationen stets deaktiviert werden – auch Animationen, für die **EnableDependentAnimation** auf **true** festgelegt ist. Informationen finden Sie unter [**Timeline.AllowDependentAnimations**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.timeline.allowdependentanimations).

> [!TIP]
> Wenn Sie den Animations Bereich in Blend für Visual Studio 2019 verwenden, werden Warnungen im Designer angezeigt, wenn Sie versuchen, eine abhängige Animation auf eine Eigenschaft des visuellen Zustands anzuwenden. Warnungen werden nicht in der Buildausgabe oder Fehlerliste angezeigt. Wenn Sie XAML per Hand bearbeiten, wird im Designer keine Warnung angezeigt. Zum Zeitpunkt der debugerstellung zeigt die Debug-Ausgabe im Ausgabebereich eine Warnung an, dass die Animation nicht unabhängig ist und übersprungen wird.


## <a name="starting-and-controlling-an-animation"></a>Starten und Steuern einer Animation

Bisher haben wir Ihnen noch nicht gezeigt, wie eine Animation wirklich ausgeführt oder angewendet wird. Bis die Animation gestartet und ausgeführt wird, sind die Wertänderungen, die von einer Animation in XAML deklariert werden, lediglich latent vorhanden und werden noch nicht durchgeführt. Sie müssen eine Animation explizit auf eine Art und Weise starten, die sich auf die App-Lebensdauer oder die Benutzererfahrung bezieht. Auf der einfachsten Ebene starten Sie eine App, indem Sie die [**Begin**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.storyboard.begin)-Methode im [**Storyboard**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.Storyboard) aufrufen, das als übergeordnetes Element der Animation fungiert. Sie können Methoden nicht direkt in XAML aufrufen. Jedwede Aktivierung von Animationen muss also über den Code durchgeführt werden. Dies ist entweder CodeBehind für die Seiten oder Komponenten der App oder die Logik des Steuerelements, wenn Sie eine benutzerdefinierte Steuerelementklasse definieren.

Normalerweise rufen Sie [**Begin**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.storyboard.begin) auf und führen die Animation einfach bis zum Abschluss ihrer Zeitdauer aus. Sie können jedoch auch die Methoden [**Pause**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.storyboard.pause), [**Resume**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.storyboard.resume) und [**Stop**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.storyboard.stop) verwenden, um das [**Storyboard**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.Storyboard) zur Laufzeit zu steuern, sowie andere APIs, die für anspruchsvollere Animationssteuerungsszenarien verwendet werden.

Wenn Sie [**Begin**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.storyboard.begin) für ein Storyboard mit Animationen aufrufen, die unendlich wiederholt werden (`RepeatBehavior="Forever"`), wird die Animation ausgeführt, bis die enthaltende Seite entladen wird oder bis Sie explizit [**Pause**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.storyboard.pause) oder [**Stop**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.storyboard.stop) aufrufen.

### <a name="starting-an-animation-from-app-code"></a>Starten einer Animation aus dem App-Code

Sie können Animationen entweder automatisch oder als Reaktion auf Benutzeraktionen starten. Beim automatischen Starten nutzen Sie in der Regel ein Objektlebensdauerereignis wie [**Loaded**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.loaded), das als Auslöser der Animation dient. Das **Loaded**-Ereignis ist hierfür gut geeignet, da die UI an diesem Punkt zur Interaktion bereit ist und die Animation am Anfang nicht abgeschnitten wird, weil noch ein anderer Teil der UI geladen werden muss.

In diesem Beispiel wird das [**PointerPressed**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerpressed)-Ereignis an das Rechteck angefügt, sodass die Animation startet, wenn der Benutzer auf das Rechteck klickt.

```xaml
<Rectangle PointerPressed="Rectangle_Tapped"
  x:Name="MyAnimatedRectangle"
  Width="300" Height="200" Fill="Blue"/>
```

Der Ereignishandler startet das [**Storyboard**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.Storyboard) (die Animation), indem die [**Begin**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.storyboard.begin)-Methode des **Storyboard**-Elements verwendet wird.

```csharp
myStoryboard.Begin();
```

```cppwinrt
myStoryboard().Begin();
```

```cpp
myStoryboard->Begin();
```

```vb
myStoryBoard.Begin()
```

Sie können das [**Completed**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.timeline.completed)-Ereignis behandeln, wenn Sie andere Logik ausführen möchten, nachdem die Animation das Anwenden von Werten abgeschlossen hat. Für die Problembehandlung bei Eigenschaftssystem-/Animationsinteraktionen kann auch die [**GetAnimationBaseValue**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyobject.getanimationbasevalue)-Methode hilfreich sein.

> [!TIP]
> Wenn Sie den Code für ein App-Szenario erstellen, bei dem Sie eine Animation aus dem App-Code starten, sollten Sie noch einmal überprüfen, ob in der Animationsbibliothek für Ihr UI-Szenario nicht bereits eine geeignete Animation oder ein Übergang enthalten ist. Die Bibliotheksanimationen ermöglichen eine einheitlichere UI-Erfahrung in allen Windows-Runtime-Apps und sind einfacher zu verwenden.

 

### <a name="animations-for-visual-states"></a>Animationen für visuelle Zustände

Das Ausführungsverhalten für ein [**Storyboard**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.Storyboard), das zum Definieren des visuellen Zustands eines Steuerelements verwendet wird, unterscheidet sich davon, wie eine App ein Storyboard ggf. direkt ausführt. Bei der Anwendung auf eine Definition des visuellen Zustands in XAML ist das **Storyboard** ein Element eines enthaltenden [**VisualState**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.VisualState)-Elements, und der gesamte Zustand wird mithilfe der [**VisualStateManager**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.visualstatemanager)-API gesteuert. Alle enthaltenen Animationen werden gemäß ihren Animationswerten und [**Timeline**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.Timeline)-Eigenschaften ausgeführt, wenn das enthaltende **VisualState**-Element von einem Steuerelement verwendet wird. Weitere Informationen finden Sie unter [Storyboardanimationen für visuelle Zustände](https://docs.microsoft.com/previous-versions/windows/apps/jj819808(v=win.10)). Für visuelle Zustände unterscheidet sich das [**FillBehavior**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.timeline.fillbehavior)-Element. Wenn ein visueller Zustand in einen anderen Zustand geändert wird, werden alle auf den vorherigen visuellen Zustand angewendeten Eigenschaftsänderungen und die zugehörigen Animationen abgebrochen – auch dann, wenn der neue visuelle Zustand nicht ausdrücklich eine neue Animation auf eine Eigenschaft anwendet.

### <a name="storyboard-and-eventtrigger"></a>**Storyboard** und **EventTrigger**

Es gibt eine Möglichkeit zum Starten einer Animation, bei der die Deklaration vollständig in XAML durchgeführt werden kann. Dieses Verfahren wird jedoch nicht mehr häufig angewendet. Es handelt sich um alte Syntax aus WPF und frühen Versionen von Silverlight, bevor die Unterstützung für [**VisualStateManager**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.visualstatemanager) hinzugefügt wurde. Diese [**EventTrigger**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.EventTrigger)-Syntax funktioniert aus Import- und Kompatibilitätsgründen in Windows-Runtime-XAML weiterhin. Dies gilt jedoch nur für ein Auslöserverhalten basierend auf dem [**FrameworkElement.Loaded**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.loaded)-Ereignis. Wenn versucht wird, andere Ereignisse auszulösen, werden Ausnahmen ausgelöst, oder die Kompilierung schlägt fehl. Weitere Informationen finden Sie unter [**EventTrigger**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.EventTrigger) oder [**BeginStoryboard**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.BeginStoryboard).

## <a name="animating-xaml-attached-properties"></a>Animieren angeschlossener XAML-Eigenschaften

Auch wenn es sich nicht um ein gängiges Szenario handelt, können Sie einen animierten Wert auf eine angefügte XAML-Eigenschaft anwenden. Weitere Informationen zu angefügten Eigenschaften und ihrer Funktionsweise finden Sie unter [Übersicht über angefügte Eigenschaften](https://docs.microsoft.com/windows/uwp/xaml-platform/attached-properties-overview). Für die Zielbestimmung einer angefügten Eigenschaft benötigen Sie eine [Property-path-Syntax](https://docs.microsoft.com/windows/uwp/xaml-platform/property-path-syntax), bei der der Eigenschaftenname in Klammern gesetzt ist. Sie können die integrierten angefügten Eigenschaften wie beispielsweise [**Canvas.ZIndex**](https://docs.microsoft.com/previous-versions/windows/silverlight/dotnet-windows-silverlight/cc190397(v=vs.95)) animieren, indem Sie ein [**ObjectAnimationUsingKeyFrames**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.ObjectAnimationUsingKeyFrames)-Element verwenden, das diskrete Ganzzahlwerte anwendet. Allerdings sieht eine bestehende Einschränkung der XAML-Implementierung der Windows-Runtime vor, dass Sie eine benutzerdefinierte angefügte Eigenschaft nicht animieren können.

## <a name="more-animation-types-and-next-steps-for-learning-about-animating-your-ui"></a>Weitere Animationstypen und nächste Schritte zum Erlernen der UI-Animation

Bisher wurden die benutzerdefinierten Animationen vorgestellt, bei denen die Animation zwischen zwei Werten erfolgt und diese Werte während der Ausführung der Animation dann je nach Bedarf linear interpoliert werden. Diese Animationen werden als **From**/**To**/**By**-Animationen bezeichnet. Es gibt jedoch noch einen anderen Animationstyp, bei dem Sie Zwischenwerte deklarieren können, die zwischen dem Start- und Endwert liegen. Diese Animationen werden als *Keyframeanimationen* bezeichnet. Außerdem kann die Interpolationslogik für eine **From**/**To**/**By**-Animation oder eine Keyframeanimation geändert werden. Dazu muss eine Beschleunigungsfunktion angewendet werden. Weitere Informationen zu diesen Konzepten finden Sie unter [Keyframeanimationen und Animationen für Beschleunigungsfunktionen](key-frame-and-easing-function-animations.md).

## <a name="related-topics"></a>Verwandte Themen

* [Eigenschafts Pfad Syntax](https://docs.microsoft.com/windows/uwp/xaml-platform/property-path-syntax)
* [Übersicht über Abhängigkeitseigenschaften](https://docs.microsoft.com/windows/uwp/xaml-platform/dependency-properties-overview)
* [Keyframe-und Beschleunigungsfunktionen-Animationen](key-frame-and-easing-function-animations.md)
* [Storyboarded Animationen für visuelle Zustände](https://docs.microsoft.com/previous-versions/windows/apps/jj819808(v=win.10))
* [Steuerelementvorlagen](https://docs.microsoft.com/windows/uwp/controls-and-patterns/control-templates)
* [**Storyboard**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.Storyboard)
* [**Storyboard. TargetProperty**](https://docs.microsoft.com/previous-versions/windows/silverlight/dotnet-windows-silverlight/ms616983(v=vs.95))
 

 




