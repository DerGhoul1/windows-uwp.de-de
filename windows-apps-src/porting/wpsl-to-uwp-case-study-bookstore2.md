---
ms.assetid: 333f67f5-f012-4981-917f-c6fd271267c6
description: Diese Fallstudie, die auf den in Bookstore gegebenen Informationen aufbaut, beginnt mit einer Windows Phone Silverlight-APP, die gruppierte Daten in einem longlistselector anzeigt.
title: Windows Phone von Silverlight zur UWP-Fallstudie, Bookstore2
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 1d1440bf3cfded6b50eb58feffd322ea484e488a
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74260105"
---
# <a name="windowsphone-silverlight-to-uwp-case-study-bookstore2"></a>Windows Phone von Silverlight zur UWP-Fallstudie: Bookstore2


Diese Fallstudie –, die auf den in [Bookstore1](wpsl-to-uwp-case-study-bookstore1.md)angegebenen Informationen aufbaut, beginnt mit einer Windows Phone Silverlight-APP, die gruppierte Daten in einem **longlistselector**anzeigt. Im Ansichtsmodell stellt jede Instanz der **Author**-Klasse die Gruppe der vom betreffenden Autor verfassten Titel dar. In **LongList Selector** können wir dann entweder die Bücherliste nach Autoren gruppiert anzeigen oder die Liste verkleinern, um eine Sprungliste der Autoren zu erhalten. Die Sprungliste ermöglicht eine wesentlich schnellere Navigation im Vergleich zum Blättern in der Bücherliste. Wir führen Sie durch die Schritte zum Portieren der APP auf eine Windows 10 universelle Windows-Plattform-app (UWP).

**Beachten Sie**   Wenn Sie Bookstore2Universal\_10 in Visual Studio öffnen, wenn die Meldung "Visual Studio-Update erforderlich" angezeigt wird, führen Sie die Schritte zum Festlegen der Ziel Platt Form Version in [targetplatformversion](w8x-to-uwp-troubleshooting.md)aus.

## <a name="downloads"></a>Downloads

[Laden Sie die Bookstore2WPSL8 Windows Phone Silverlight-APP herunter](https://codeload.github.com/MicrosoftDocs/windows-topic-specific-samples/zip/Bookstore2WPSL8).

[Laden Sie die Bookstore2Universal\_10 Windows 10-APP herunter](https://codeload.github.com/MicrosoftDocs/windows-topic-specific-samples/zip/Bookstore2Universal_10).

##  <a name="the-windowsphone-silverlight-app"></a>Die Windows Phone Silverlight-App

Die folgende Abbildung zeigt, wie die zu portierende App namens Bookstore2WPSL8 aussieht. Es handelt sich um einen vertikal scrollbaren **LongListSelector** mit Buchtiteln, die nach Autor gruppiert sind. Sie können die Liste auf die Sprungliste verkleinern und von dort aus wieder zurück zu einer beliebigen Gruppe navigieren. Die App besteht aus zwei Hauptteilen: dem Ansichtsmodell, das die gruppierte Datenquelle bereitstellt, und der Benutzeroberfläche, die an dieses Ansichtsmodell gebunden ist. Wie wir sehen werden, sind beide Teile leicht von Windows Phone Silverlight-Technologie zum universelle Windows-Plattform (UWP) portieren.

![Erscheinungsbild von „bookstore2wpsl8“](images/wpsl-to-uwp-case-studies/c02-01-wpsl-how-the-app-looks.png)

##  <a name="porting-to-a-windows10-project"></a>Portieren auf ein Windows 10-Projekt

Das Erstellen eines neuen Projekts in Visual Studio geht schnell: Kopieren Sie Dateien aus Bookstore2WPSL8, und fügen Sie die kopierten Dateien in das neue Projekt ein. Erstellen Sie zunächst ein neues Projekt vom Typ „Leere Anwendung“ (Windows Universal). Nennen Sie Sie Bookstore2Universal\_10. Dabei handelt es sich um die Dateien, die von Bookstore2WPSL8 zu Bookstore2Universal\_10 kopiert werden.

-   Kopieren Sie den Ordner, der das Buch enthält, das das Buch enthält, das das Buch enthält (der Ordner ist \\\\coverimages). Vergewissern Sie sich nach dem Kopieren des Ordners im **Projektmappen-Explorer**, dass **Alle Dateien anzeigen** aktiviert ist. Klicken Sie mit der rechten Maustaste auf den kopierten Ordner, und klicken Sie dann auf **Zu Projekt hinzufügen**. Mit „Einschließen“ von Dateien oder Ordnern in einem Projekt meinen wir diesen Befehl. Klicken Sie jedes Mal, wenn Sie eine Datei oder einen Ordner kopieren, im **Projektmappen-Explorer** auf **Aktualisieren**, und schließen Sie dann die Datei oder den Ordner in das Projekt ein. Dies ist nicht für Dateien erforderlich, die Sie am Ziel ersetzen.
-   Kopieren Sie den Ordner mit der Quelldatei für das Ansichts Modell (der Ordner ist \\ViewModel).
-   Kopieren Sie „MainPage.xaml“, und ersetzen Sie die Datei am Ziel.

Wir können die Datei "App. XAML" und App.XAML.cs, die Visual Studio für uns generiert hat, im Windows 10-Projekt aufbewahren.

Bearbeiten Sie den Quell Code und die soeben kopierten Markup Dateien, und ändern Sie alle Verweise auf den Bookstore2WPSL8-Namespace in Bookstore2Universal\_10. Eine schnelle Möglichkeit dafür ist die Verwendung des Features **In Dateien ersetzen**. Im imperativen Code in der Ansichtsmodell-Quelldatei sind die folgenden Portierungsänderungen erforderlich:

-   Ändern Sie `System.ComponentModel.DesignerProperties` in `DesignMode`, und wenden Sie dann den Befehl **Auflösen** darauf an. Löschen Sie die `IsInDesignTool`-Eigenschaft, und fügen Sie mithilfe von IntelliSense den richtigen Eigenschaftsnamen hinzu: `DesignModeEnabled`.
-   Wenden Sie den Befehl **Auflösen** auf `ImageSource` an.
-   Wenden Sie den Befehl **Auflösen** auf `BitmapImage` an.
-   Löschen Sie `using System.Windows.Media;` und `using System.Windows.Media.Imaging;`.
-   Ändern Sie den Wert, der von der **Bookstore2Universal\_10. bookstoreviewmodel. appname** -Eigenschaft von "BOOKSTORE2WPSL8" in "Bookstore2Universal" zurückgegeben wird.
-   Wie zuvor bei [Bookstore1](wpsl-to-uwp-case-study-bookstore1.md) aktualisieren Sie die Implementierung der **BookSku.CoverImage**-Eigenschaft (siehe [Binden eines Bilds an ein Ansichtsmodell](wpsl-to-uwp-case-study-bookstore1.md)).

In „MainPage.xaml“ müssen Sie die folgenden anfänglichen Portierungsänderungen vornehmen:

-   Ändern Sie `phone:PhoneApplicationPage` in `Page` (einschließlich der Vorkommen in der Eigenschaftselementsyntax).
-   Löschen Sie die Namespacepräfix-Deklarationen `phone` und `shell`.
-   Ändern Sie „clr-namespace“ in der verbleibenden Namespacepräfix-Deklaration in „using“.
-   Löschen Sie `SupportedOrientations="Portrait"`und `Orientation="Portrait"`, und konfigurieren Sie **Portrait** im App-Paketmanifest des neuen Projekts.
-   Löschen Sie `shell:SystemTray.IsVisible="True"`.
-   Die Konvertertypen für Sprunglistenelemente (die im Markup als Ressourcen enthalten sind) wurden in den [**Windows.UI.Xaml.Controls.Primitives**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives)-Namespace verschoben. Fügen Sie die Namespace-Präfix Deklaration Windows\_UI\_XAML-\_Steuerelemente\_primitiven hinzu, und ordnen Sie Sie **Windows. UI. XAML. Controls. Primitives**zu. Ändern Sie das Präfix in den Konverterressourcen für Sprunglistenelemente von `phone:` in `Windows_UI_Xaml_Controls_Primitives:`.
-   Ersetzen Sie wie zuvor bei [Bookstore1](wpsl-to-uwp-case-study-bookstore1.md) alle Verweise auf den `PhoneTextExtraLargeStyle` **TextBlock**-Stil durch einen Verweis auf `SubtitleTextBlockStyle`, und ersetzen Sie `PhoneTextSubtleStyle` durch `SubtitleTextBlockStyle`, `PhoneTextNormalStyle` durch `CaptionTextBlockStyle` und `PhoneTextTitle1Style` durch `HeaderTextBlockStyle`.
-   Einzige Ausnahme ist `BookTemplate`. Der Stil des zweiten **TextBlock**-Elements sollte auf `CaptionTextBlockStyle` verweisen.
-   Entfernen Sie das FontFamily-Attribut aus **TextBlock** innerhalb von `AuthorGroupHeaderTemplate`, und legen Sie den Hintergrund von **Border** so fest, dass nicht auf `SystemControlBackgroundAccentBrush` verwiesen wird, sondern auf `PhoneAccentBrush`.
-   Überprüfen Sie aufgrund von [Änderungen im Zusammenhang mit Anzeigepixeln](wpsl-to-uwp-porting-xaml-and-ui.md) das Markup, und multiplizieren Sie alle Dimensionen mit fester Größe (Ränder, Breite, Höhe usw.) mit 0,8.

## <a name="replacing-the-longlistselector"></a>Ersetzen von „LongListSelector“


Um **LongListSelector** durch ein [**SemanticZoom**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom)-Steuerelement zu ersetzen, sind mehrere Schritte erforderlich. Los geht’s. **LongListSelector** wird direkt an die gruppierte Datenquelle gebunden, während **SemanticZoom**[**ListView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView)- oder [**GridView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView)-Steuerelemente enthält, die über einen [**CollectionViewSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.CollectionViewSource)-Adapter indirekt an die Daten gebunden werden. **CollectionViewSource** muss als Ressource im Markup vorhanden sein. Deshalb fügen wir sie zunächst dem Markup in „MainPage.xaml“ innerhalb von `<Page.Resources>` hinzu.

```xml
    <CollectionViewSource
        x:Name="AuthorHasACollectionOfBookSku"
        Source="{Binding Authors}"
        IsSourceGrouped="true"/>
```

Beachten Sie, dass die Bindung an **LongListSelector.ItemsSource** zum Wert **CollectionViewSource.Source** und **LongListSelector.IsGroupingEnabled** zu **CollectionViewSource.IsSourceGrouped** wird. **CollectionViewSource** weist einen Namen (und nicht wie möglicherweise erwartet einen Schlüssel) auf, sodass wir eine Bindung daran vornehmen können.

Als Nächstes ersetzen Sie `phone:LongListSelector` durch das folgende Markup. Dadurch erhalten wir ein vorläufiges **SemanticZoom**-Element, mit dem wir arbeiten können.

```xml
    <SemanticZoom>
        <SemanticZoom.ZoomedInView>
            <ListView
                ItemsSource="{Binding Source={StaticResource AuthorHasACollectionOfBookSku}}"
                ItemTemplate="{StaticResource BookTemplate}">
                <ListView.GroupStyle>
                    <GroupStyle
                        HeaderTemplate="{StaticResource AuthorGroupHeaderTemplate}"
                        HidesIfEmpty="True"/>
                </ListView.GroupStyle>
            </ListView>
        </SemanticZoom.ZoomedInView>
        <SemanticZoom.ZoomedOutView>
            <ListView
                ItemsSource="{Binding CollectionGroups, Source={StaticResource AuthorHasACollectionOfBookSku}}"
                ItemTemplate="{StaticResource ZoomedOutAuthorTemplate}"/>
        </SemanticZoom.ZoomedOutView>
    </SemanticZoom>
```

Den flachen Listen und Sprunglisten von **LongListSelector** stehen die vergrößerten und verkleinerten Ansichten von **SemanticZoom** gegenüber. Die vergrößerte Ansicht ist eine Eigenschaft, die Sie auf eine Instanz von **ListView** festlegen. In diesem Fall ist die verkleinerte Ansicht auch auf **ListView** festgelegt, und beide **ListView**-Steuerelemente sind an **CollectionViewSource** gebunden. Die vergrößerte Ansicht verwendet die gleiche Elementvorlage, Gruppenkopfvorlage und **HideEmptyGroups**-Einstellung (dann allerdings mit dem Namen **HidesIfEmpty**) wie die flache Liste von **LongListSelector**. Darüber hinaus verwendet die verkleinerte Ansicht eine Elementvorlage, die weitestgehend der Vorlage im Sprunglistenstil ( **) von** LongListSelector`AuthorNameJumpListStyle` entspricht. Beachten Sie auch, dass die verkleinerte Ansicht an eine bestimmte **CollectionViewSource**-Eigenschaft namens **CollectionGroups** gebunden wird. Dabei handelt es sich um eine Auflistung, die anstelle der Elemente Gruppen enthält.

`AuthorNameJumpListStyle` wird größtenteils nicht mehr benötigt. Wir benötigen nur die Datenvorlage für die Gruppen in der verkleinerten Ansicht (d. h. die Autoren in dieser App). Hierzu löschen wir den `AuthorNameJumpListStyle`-Stil und ersetzen ihn durch die folgende Datenvorlage.

```xml
   <DataTemplate x:Key="ZoomedOutAuthorTemplate">
        <Border Margin="9.6,0.8" Background="{Binding Converter={StaticResource JumpListItemBackgroundConverter}}">
            <TextBlock Margin="9.6,0,9.6,4.8" Text="{Binding Group.Name}" Style="{StaticResource SubtitleTextBlockStyle}"
            Foreground="{Binding Converter={StaticResource JumpListItemForegroundConverter}}" VerticalAlignment="Bottom"/>
        </Border>
    </DataTemplate>
```

Da der Datenkontext dieser Datenvorlage eine Gruppe und kein Element ist, binden wir ihn an eine spezielle Eigenschaft namens **Group**.

Sie können die App jetzt erstellen und ausführen. Im Mobile-Emulator sieht sie wie hier dargestellt aus.

![UWP-App auf dem Mobilgerät mit Änderungen am ursprünglichen Quellcode](images/wpsl-to-uwp-case-studies/c02-02-mob10-initial-source-code-changes.png)

Das Ansichtsmodell und die vergrößerten sowie verkleinerten Ansichten arbeiten korrekt zusammen. Ein Problem ist aber, dass der Aufwand in Bezug auf die Formatierungen und Vorlagen etwas erhöht ist. Beispielsweise werden die richtigen Stile und Pinsel noch nicht verwendet, sodass der Text in den Gruppen Headern unsichtbar ist, auf die Sie klicken können, um zu verkleinern. Wenn Sie die APP auf einem Desktop Gerät ausführen, wird ein zweites Problem angezeigt, das heißt, dass die APP die Benutzeroberfläche noch nicht anpasst, um die beste Leistung und Verwendung von Speicherplatz auf größeren Geräten zu ermöglichen, auf denen Windows potenziell viel größer ist als der Bildschirm eines mobilen Geräts. In den nächsten Abschnitten ([Anfängliches Erstellen von Formatierungen und Vorlagen](#initial-styling-and-templating), [Adaptive UI](#adaptive-ui) und [Abschließende Formatierung](#final-styling)) beheben wir diese Probleme.

## <a name="initial-styling-and-templating"></a>Anfängliches Erstellen von Formatierungen und Vorlagen

Zum richtigen Festlegen der Abstände für die Gruppenköpfe bearbeiten Sie `AuthorGroupHeaderTemplate` und legen für **Margin** unter `"0,0,0,9.6"`Border**den Wert** fest.

Zum richtigen Festlegen der Abstände für die Buchelemente bearbeiten Sie `BookTemplate` und legen **Margin** für beide `"9.6,0"`TextBlock **-Elemente auf den Wert**  fest.

Um das Layout des App-Namens und des Seitentitels zu verbessern, entfernen Sie unter `TitlePanel` das **Margin**-Element für den oberen Rand im zweiten **TextBlock**, indem Sie den Wert auf `"7.2,0,0,0"` festlegen. Für das `TitlePanel`-Element selbst legen Sie den Rand auf `0` (oder einen anderen gewünschten Wert) fest.

Ändern Sie den Hintergrund von `LayoutRoot` in `"{ThemeResource ApplicationPageBackgroundThemeBrush}"`.

## <a name="adaptive-ui"></a>Adaptive UI

Da wir mit einer Telefon-App begonnen haben, ist es nicht überraschend, dass das UI-Layout unserer portierten App zu diesem Zeitpunkt eigentlich nur für kleine Geräte und schmale Fenster sinnvoll ist. Wir möchten aber erreichen, dass das UI-Layout sich selbst anpasst und den Platz besser nutzt, wenn die App in einem breiten Fenster ausgeführt wird (nur möglich auf einem Gerät mit großem Bildschirm). Außerdem soll die aktuelle UI nur verwendet werden, wenn das Fenster der App schmal ist (möglich auf einem kleinen Gerät und auch auf einem großen Gerät).

Wir können dazu auch das adaptive Visual State-Manager-Feature verwenden. Wir werden Eigenschaften für visuelle Elemente festlegen, damit für die Benutzeroberfläche mit den Vorlagen, die wir derzeit verwenden, standardmäßig der schmale Zustand genutzt wird. Dann werden wir erkennen, wann das Fenster der App breiter als eine bestimmte Größe ist bzw. dieser entspricht (gemessen in der Einheit [effektive Pixel](wpsl-to-uwp-porting-xaml-and-ui.md)), und als Reaktion darauf die Eigenschaften der visuelle Elemente so ändern, dass wir ein größeres und breiteres Layout erhalten. Wir versetzen diese Eigenschaftsänderungen in einen visuellen Zustand und verwenden einen adaptiven Auslöser, um fortlaufend zu überwachen und zu bestimmen, ob der visuelle Zustand abhängig von der Breite des Fensters in effektiven Pixeln angewendet werden soll. Die Auslösung erfolgt hierbei anhand der Fensterbreite, aber sie kann auch anhand der Fensterhöhe erfolgen.

Eine minimale Fensterbreite von 548 Epx eignet sich in diesem Anwendungsfall, da dies der Größe des kleinsten Geräts entspricht, auf dem wir das breite Layout anzeigen können. Telefone sind in der Regel kleiner als 548 Epx, sodass wir auf solch einem kleinen Gerät das schmale Layout erhalten würden. Auf einem PC wird das Fenster standardmäßig breit genug gestartet, um den Wechsel zum breiten Zustand auszulösen, in dem Elemente der Größe 250 x 250 angezeigt werden. Dort können Sie das Fenster so schmal machen, dass mindestens zwei Spalten für Elemente der Größe 250 x 250 angezeigt werden können. Ist es schmaler, wird der Auslöser deaktiviert, der breite Ansichtszustand wird gelöscht, und das schmale Standardlayout wird aktiviert.

Bevor wir das Problem mit dem adaptiven Visual State-Manager angehen, müssen wir zuerst den breiten Zustand entwerfen. Dies bedeutet, dass wir unserem Markup einige neue visuelle Elemente und Vorlagen hinzufügen müssen. Die entsprechende Vorgehensweise wird in den folgenden Schritten beschrieben. Mit Benennungskonventionen für visuelle Elemente und Vorlagen fügen wir das Wort „wide“ (breit) in den Namen aller Elemente oder Vorlagen für den breiten Zustand ein. Wenn ein Element oder eine Vorlage nicht das Wort „wide“ enthält, können Sie davon ausgehen, dass es für den schmalen Zustand bestimmt ist. Dies ist der Standardzustand, und die Eigenschaftswerte werden als lokale Werte der visuellen Elemente einer Seite festgelegt. Nur die Eigenschaftswerte für den breiten Zustand werden im Markup über einen tatsächlichen visuellen Zustand festgelegt.

-   Erstellen Sie eine Kopie des [**SemanticZoom**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom)-Steuerelements im Markup, und legen Sie in der Kopie `x:Name="narrowSeZo"` fest. Legen Sie im Original `x:Name="wideSeZo"` und `Visibility="Collapsed"` so fest, dass der breite Zustand standardmäßig nicht sichtbar ist.
-   Ändern Sie in `wideSeZo` die **ListView**-Elemente in **GridView**-Elemente sowohl für die vergrößerte als auch für die verkleinerte Ansicht.
-   Erstellen Sie eine Kopie der drei Ressourcen `AuthorGroupHeaderTemplate`, `ZoomedOutAuthorTemplate` und `BookTemplate`, und fügen Sie das Wort `Wide` an die Schlüssel der Kopien an. Aktualisieren Sie außerdem `wideSeZo`, damit darin auf die Schlüssel dieser neuen Ressourcen verwiesen wird.
-   Ersetzen Sie den Inhalt von `AuthorGroupHeaderTemplateWide` durch `<TextBlock Style="{StaticResource SubheaderTextBlockStyle}" Text="{Binding Name}"/>`.
-   Ersetzen Sie den Inhalt von `ZoomedOutAuthorTemplateWide` durch:

```xml
    <Grid HorizontalAlignment="Left" Width="250" Height="250" >
        <Border Background="{StaticResource ListViewItemPlaceholderBackgroundThemeBrush}"/>
        <StackPanel VerticalAlignment="Bottom" Background="{StaticResource ListViewItemOverlayBackgroundThemeBrush}">
          <TextBlock Foreground="{StaticResource ListViewItemOverlayForegroundThemeBrush}"
              Style="{StaticResource SubtitleTextBlockStyle}"
            Height="80" Margin="15,0" Text="{Binding Group.Name}"/>
        </StackPanel>
    </Grid>
```

-   Ersetzen Sie den Inhalt von `BookTemplateWide` durch:

```xml
    <Grid HorizontalAlignment="Left" Width="250" Height="250">
        <Border Background="{StaticResource ListViewItemPlaceholderBackgroundThemeBrush}"/>
        <Image Source="{Binding CoverImage}" Stretch="UniformToFill"/>
        <StackPanel VerticalAlignment="Bottom" Background="{StaticResource ListViewItemOverlayBackgroundThemeBrush}">
            <TextBlock Style="{StaticResource SubtitleTextBlockStyle}"
                Foreground="{StaticResource ListViewItemOverlaySecondaryForegroundThemeBrush}"
                TextWrapping="NoWrap" TextTrimming="CharacterEllipsis"
                Margin="12,0,24,0" Text="{Binding Title}"/>
            <TextBlock Style="{StaticResource CaptionTextBlockStyle}" Text="{Binding Author.Name}"
                Foreground="{StaticResource ListViewItemOverlaySecondaryForegroundThemeBrush}" TextWrapping="NoWrap"
                TextTrimming="CharacterEllipsis" Margin="12,0,12,12"/>
        </StackPanel>
    </Grid>
```

-   Für den breiten Zustand benötigen die Gruppen in der vergrößerten Ansicht mehr vertikalen Platz. Wir erhalten die gewünschten Ergebnisse, indem wir eine ItemsPanel-Vorlage erstellen und darauf verweisen. Das Markup sieht wie folgt aus:

```xml
   <ItemsPanelTemplate x:Key="ZoomedInItemsPanelTemplate">
        <ItemsWrapGrid Orientation="Horizontal" GroupPadding="0,0,0,20"/>
    </ItemsPanelTemplate>
    ...

    <SemanticZoom x:Name="wideSeZo" ... >
        <SemanticZoom.ZoomedInView>
            <GridView
            ...
            ItemsPanel="{StaticResource ZoomedInItemsPanelTemplate}">
            ...
```

-   Fügen Sie zuletzt das richtige Visual State Manager-Markup als erstes untergeordnetes Element von `LayoutRoot` hinzu.

```xml
    <Grid x:Name="LayoutRoot" ... >
        <VisualStateManager.VisualStateGroups>
            <VisualStateGroup>
                <VisualState x:Name="WideState">
                    <VisualState.StateTriggers>
                        <AdaptiveTrigger MinWindowWidth="548"/>
                    </VisualState.StateTriggers>
                    <VisualState.Setters>
                        <Setter Target="wideSeZo.Visibility" Value="Visible"/>
                        <Setter Target="narrowSeZo.Visibility" Value="Collapsed"/>
                    </VisualState.Setters>
                </VisualState>
            </VisualStateGroup>
        </VisualStateManager.VisualStateGroups>

    ...
```

## <a name="final-styling"></a>Abschließende Formatierung

Nun müssen wir nur noch einige abschließende Formatierungsoptimierungen vornehmen.

-   Legen Sie unter `AuthorGroupHeaderTemplate` das `Foreground="White"`-Element für **TextBlock** so fest, dass sich bei der Ausführung für die Familie der Mobilgeräte die richtige Darstellung ergibt.
-   Fügen Sie `FontWeight="SemiBold"` sowohl in **als auch in** dem `AuthorGroupHeaderTemplate`TextBlock`ZoomedOutAuthorTemplate`-Element hinzu.
-   In `narrowSeZo`sind die Gruppenköpfe und die Autoren in der verkleinerten Ansicht nicht gestreckt, sondern linksbündig ausgerichtet. Dies müssen wir also ändern. Wir erstellen [**HeaderContainerStyle**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.groupstyle.headercontainerstyle) für die vergrößerte Ansicht, wobei [**HorizontalContentAlignment**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.horizontalcontentalignment) auf `Stretch` festgelegt ist. Anschließend erstellen wir [**ItemContainerStyle**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.itemscontrol.itemcontainerstyle) für die verkleinerte Ansicht, die denselben [**Setter**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Setter) enthält. Dies sieht wie folgt aus:

```xml
   <Style x:Key="AuthorGroupHeaderContainerStyle" TargetType="ListViewHeaderItem">
        <Setter Property="HorizontalContentAlignment" Value="Stretch"/>
    </Style>

    <Style x:Key="ZoomedOutAuthorItemContainerStyle" TargetType="ListViewItem">
        <Setter Property="HorizontalContentAlignment" Value="Stretch"/>
    </Style>

    ...

    <SemanticZoom x:Name="narrowSeZo" ... >
        <SemanticZoom.ZoomedInView>
            <ListView
            ...
                <ListView.GroupStyle>
                    <GroupStyle
                    ...
                    HeaderContainerStyle="{StaticResource AuthorGroupHeaderContainerStyle}"
                    ...
        <SemanticZoom.ZoomedOutView>
            <ListView
                ...
                ItemContainerStyle="{StaticResource ZoomedOutAuthorItemContainerStyle}"
                ...
```

Nach den letzten Formatierungsvorgängen sieht die App folgendermaßen aus.

![Die portierte Windows 10-App auf einem Desktopgerät, vergrößerte Ansicht, zwei Fenstergrößen](images/w8x-to-uwp-case-studies/c02-07-desk10-zi-ported.png)

Die portierte Windows 10-APP, die auf einem Desktop Gerät ausgeführt wird, vergrößert die Ansicht, zwei Größen von Fenstern  
![der portierten Windows 10-APP, die auf einem Desktop Gerät ausgeführt wird, Zoomansicht, zwei Fenstergrößen](images/w8x-to-uwp-case-studies/c02-08-desk10-zo-ported.png)

Die portierte Windows 10-APP, die auf einem Desktop Gerät ausgeführt wird, Zoomansicht, zwei Fenstergrößen

![Die portierte Windows 10-App auf einem mobilen Gerät, vergrößerte Ansicht](images/w8x-to-uwp-case-studies/c02-09-mob10-zi-ported.png)

Die portierte Windows 10-APP, die auf einem mobilen Gerät ausgeführt wird, Zoomansicht

![Die portierte Windows 10-App auf einem mobilen Gerät, verkleinerte Ansicht](images/w8x-to-uwp-case-studies/c02-10-mob10-zo-ported.png)

Die portierte Windows 10-APP, die auf einem mobilen Gerät ausgeführt wird, und Zoomansicht

## <a name="making-the-view-model-more-flexible"></a>Entwickeln eines flexibleren Ansichtsmodells

In diesem Abschnitt wird demonstriert, welche Möglichkeiten sich durch das Verschieben unserer App und die Verwendung der UWP ergeben. Hier erläutern wir optionale Schritte, mit denen Sie Ihr Ansichtsmodell flexibler gestalten können, wenn über **CollectionViewSource** darauf zugegriffen wird. Das Ansichts Modell (die Quelldatei befindet sich in ViewModel\\BookstoreViewModel.cs), das wir aus der Windows Phone Silverlight-App portiert haben Bookstore2WPSL8 enthält eine Klasse mit dem Namen Author, die von **List&lt;t&gt;** abgeleitet ist, wobei **t** für booksku steht. Dies bedeutet, dass die Author-Klasse einer Gruppe von BookSku-Objekten *entspricht*.

Wenn wir **CollectionViewSource.Source** an „Authors“ binden, zeigen wir dadurch lediglich an, dass jeder Autor in „Authors“ eine Gruppe von *etwas* ist. Wir überlassen es **CollectionViewSource** zu bestimmen, dass „Author“ in diesem Fall eine BookSku-Gruppe ist. Diese Lösung funktioniert zwar, ist jedoch nicht sehr flexibel. Was wäre, wenn „Author“ *beides* sein soll, eine Gruppe von BookSku-Objekten *und* eine Gruppe der Adressen, unter denen der Autor gewohnt hat? „Author“ kann *nicht* beide dieser Gruppen darstellen. „Author“ *kann* jedoch eine beliebige Anzahl von Gruppen enthalten. Unsere Lösung sieht also wie folgt aus: Verwenden Sie das *has-a-group*-Muster anstelle oder zusätzlich zum aktuell verwendeten *is-a-group*-Muster. Gehen Sie dazu wie folgt vor:

-   Ändern Sie „Author“, sodass das Element nicht mehr von **List&lt;T&gt;** abgeleitet wird.
-   Dieses Feld hinzufügen 
-   Diese Eigenschaft hinzufügen zu 
-   Selbstverständlich können wir die beiden vorangehenden Schritte auch wiederholen, um „Author“ so viele Gruppen wie nötig hinzuzufügen.
-   Ändern Sie die Implementierung der AddBookSku-Methode in `this.BookSkus.Add(bookSku);`.
-   Nachdem „Author“ jetzt über mindestens eine Gruppe *verfügt*, müssen wir **CollectionViewSource** mitteilen, welche dieser Gruppen verwendet werden soll. Fügen Sie diese Eigenschaft hierzu **CollectionViewSource** hinzu: `ItemsPath="BookSkus"`

Die Funktionalität dieser App wird durch diese Änderungen nicht verändert, aber Sie wissen jetzt, wie Sie „Author“ und **CollectionViewSource** im Bedarfsfall erweitern können. Wir nehmen nun eine letzte Änderung an „Author“ vor, damit eine Standardgruppe unserer Wahl verwendet wird, wenn wir „Author“ *ohne* Angabe von **CollectionViewSource.ItemsPath** verwenden:

```csharp
    public class Author : IEnumerable<BookSku>
    {
        ...

        public IEnumerator<BookSku> GetEnumerator()
        {
            return this.BookSkus.GetEnumerator();
        }
        System.Collections.IEnumerator System.Collections.IEnumerable.GetEnumerator()
        {
            return this.BookSkus.GetEnumerator();
        }
    }
```

Jetzt können wir `ItemsPath="BookSkus"` entfernen, ohne dass sich das Verhalten der App ändert.

## <a name="conclusion"></a>Fazit

In dieser Fallstudie haben wir es mit einer aufwändigeren Benutzeroberfläche als im vorherigen Beispiel zu tun. Alle Funktionen und Konzepte der Windows Phone Silverlight **longlistselector**– und mehr – sind für eine UWP-app in Form von **semanticzoom**, **ListView**, **GridView**und **CollectionViewSource**verfügbar. Sie haben erfahren, wie Sie sowohl imperativen Code als auch Markup in einer UWP-App wiederverwenden oder kopieren und bearbeiten, um Funktionen, Benutzeroberflächenelemente und Interaktionen speziell für die schmalsten und breitesten Formfaktoren von Windows-Geräten und alle Größen dazwischen umzusetzen.
