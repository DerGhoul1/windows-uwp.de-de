---
author: jwmsft
Description: Command bar flyouts give users inline access to your app's most common tasks.
title: Befehlsleiste flyout
label: Command bar flyout
template: detail.hbs
ms.author: jimwalk
ms.date: 07/19/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows10, UWP
pm-contact: abarlow
design-contact: ksulliv
dev-contact: llongley
doc-status: Draft
ms.localizationpriority: medium
ms.openlocfilehash: ec532749fc2dacfc56e80ee2830da36f71c75b2f
ms.sourcegitcommit: 5dda01da4702cbc49c799c750efe0e430b699502
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/21/2018
ms.locfileid: "4118498"
---
# <a name="command-bar-flyout"></a>Befehlsleiste flyout

> [!IMPORTANT]
> In diesem Artikel werden Funktionen beschrieben, die noch nicht veröffentlicht wurden und vor der kommerziellen Freigabe evtl. geändert werden. Microsoft übernimmt keine Garantie, weder ausdrücklich noch stillschweigend, für die hier bereitgestellten Informationen. Preview-Features benötigen die [neuesten Windows 10 Insider Preview-Build und SDK](https://insider.windows.com/for-developers/) oder der [Windows-UI-Bibliothek](https://docs.microsoft.com/uwp/toolkits/winui/).

Das Befehlsleiste Flyout können Sie Benutzer mit einfachen Zugriff auf allgemeine Aufgaben bereitstellen, indem Befehle in einem schwebenden Symbolleisten im Zusammenhang mit eines Elements auf die UI-Canvas angezeigt.

![Eine erweiterte Text Befehlsleiste-flyout](images/command-bar-flyout-text-full.png)

> Verwandte Informationen finden Sie unter [Flyouts](../controls-and-patterns/dialogs-and-flyouts/flyouts.md), [Menüs und Kontextmenüs](menus.md)und [Befehlsleisten](app-bars.md).

Wie [CommandBar](app-bars.md)hat CommandBarFlyout **PrimaryCommands** und **"secondarycommands"** Eigenschaften, mit denen Sie Befehle hinzufügen. Sie können entweder Sammlung oder beide Befehle versehen. Wann und wie die primären und sekundären Befehle angezeigt werden, hängt von den Anzeigemodus.

Das Befehlsleiste Flyout verfügt über zwei Anzeigemodi: *reduziert* und *Erweitert*.

- Im Modus "Collapsed" werden nur die primären Befehle angezeigt. Hat der Befehlsleiste Flyout primären und sekundären Befehle, eine "Schaltfläche" Weitere, dargestellt durch Auslassungspunkte \ [• • •] wird angezeigt. Dadurch wird den Benutzer den Zugriff auf die sekundären Befehle Abrufen von erweiterten Modus wechselt.
- Im erweiterten Modus werden die primären und sekundären Befehle angezeigt. (Wenn das Steuerelement nur sekundäre Elemente verfügt, werden sie in einer Weise ähnelt dem MenuFlyout-Steuerelement angezeigt.)

| **Windows-UI-Bibliothek herunterladen** |
| - |
| Dieses Steuerelement ist Bestandteil der Windows-UI-Bibliothek, NuGet-Paket, das neue Steuerelemente und UI-Features für UWP-apps enthält. Weitere Informationen, einschließlich installationsanweisungen finden Sie die [Übersicht über die Windows-UI-Bibliothek](https://docs.microsoft.com/uwp/toolkits/winui/). |

| **Plattform-APIs** | **Windows-UI-Bibliothek APIs** |
| - | - |
| [CommandBarFlyout-Klasse](/uwp/api/windows.ui.xaml.controls.commandbarflyout), [TextCommandBarFlyout-Klasse](/uwp/api/windows.ui.xaml.controls.textcommandbarflyout), [AppBarButton-Klasse](/uwp/api/windows.ui.xaml.controls.appbarbutton), [Klasse "appbartogglebutton"](/uwp/api/windows.ui.xaml.controls.appbartogglebutton), [Klasse "appbarseparator"](/uwp/api/windows.ui.xaml.controls.appbarseparator) | [CommandBarFlyout-Klasse](/uwp/api/microsoft.ui.xaml.controls.commandbarflyout), [TextCommandBarFlyout-Klasse](/uwp/api/microsoft.ui.xaml.controls.textcommandbarflyout) |

## <a name="is-this-the-right-control"></a>Ist dies das richtige Steuerelement?

Verwenden Sie das CommandBarFlyout-Steuerelement, um eine Sammlung von Befehle für dem Benutzer, z. B. Schaltflächen und Menüelementen im Kontext eines Elements auf der app-Canvas anzeigen.

Die TextCommandBarFlyout zeigt Textbefehle im TextBlock, TextBox, RichEditBox, RichTextBlock und PasswordBox-Steuerelemente. Die Befehle werden automatisch auf die aktuelle Textauswahl entsprechend konfiguriert. Verwenden Sie eine CommandBarFlyout, um die Standard-Text-Befehle auf Text-Steuerelemente ersetzen.

Um kontextbezogene anzeigen folgen Befehle auf Listenelemente der Anleitung im [Contextual Befehle für Sammlungen und Listen](collection-commanding.md).

### <a name="commandbarflyout-vs-menuflyout"></a>CommandBarFlyout Vs MenuFlyout

Um Befehle in einem Kontextmenü angezeigt werden, können Sie CommandBarFlyout oder MenuFlyout verwenden. CommandBarFlyout wird empfohlen, da es mehr Funktionen als MenuFlyout bereitstellt. Sie können CommandBarFlyout mit nur sekundäre Befehle verwenden, erhalten das Verhalten und von einem MenuFlyout, oder verwenden Sie das vollständige Befehlsleiste Flyout mit primären und sekundären Befehlen.

## <a name="examples"></a>Beispiele

<table>
<th align="left">XAML-Steuerelementekatalog<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Wenn Sie die <strong style="font-weight: semi-bold">XAML-Steuerelementekatalog</strong> -app installiert haben, klicken Sie hier, um <a href="xamlcontrolsgallery:/item/CommandBarFlyout">die app zu öffnen und finden Sie unter der CommandBarFlyout in Aktion zu sehen</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Erwerben Sie die XAML-Steuerelementekatalog-App (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Erwerben Sie den Quellcode (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="proactive-vs-reactive-invocation"></a>Im Vergleich zu Zeiten Aufruf proaktive

Es gibt in der Regel zwei Möglichkeiten zum Aufrufen eines Flyouts oder dem Menü ", die mit einem Element auf Ihrer Benutzeroberfläche Canvas verknüpft ist: _proaktive aufrufen_ und das _reaktive Aufruf_.

Unter proaktive Aufruf werden Befehle automatisch angezeigt, wenn der Benutzer mit dem Element interagiert, die die Befehle zugeordnet sind. Z. B. möglicherweise Formatierungsbefehle Text eingeblendet, wenn der Benutzer Text in einem Textfeld auswählt. In diesem Fall wird das Befehlsleiste Flyout Fokus nicht verwendet. Stattdessen stellt es geeignete Befehle nahe dem Element, dem der Benutzer mit interagiert. Wenn der Benutzer mit den Befehlen interagieren nicht, werden sie geschlossen.

In Zeiten Aufruf werden Befehle in Reaktion auf eine explizite Benutzeraktion angezeigt, die Befehle anfordern. z. B. eine mit der rechten Maustaste. Dies entspricht dem herkömmliche Konzept eines [Kontextmenüs](menus.md).

Sie können die CommandBarFlyout in Weise oder sogar eine Mischung der beiden verwenden.

## <a name="create-a-command-bar-flyout"></a>Erstellen Sie ein Befehlsleiste flyout

> **Vorschau**: CommandBarFlyout erfordert die [neuesten Windows 10 Insider Preview-Build und SDK](https://insider.windows.com/for-developers/) oder der [Windows-UI-Bibliothek](https://docs.microsoft.com/uwp/toolkits/winui/).

In diesem Beispiel wird veranschaulicht, wie ein Befehlsleiste Flyout erstellen und verwenden sie proaktiv und reaktiv. Wenn das Bild getippt wird, wird das Flyout im Modus "Collapsed" angezeigt. Wenn als Kontextmenü angezeigt wird, wird das Flyout im erweiterten Modus angezeigt. In beiden Fällen kann der Benutzer erweitern oder reduzieren das Flyout, nachdem er geöffnet wird.

:::row:::
    :::column:::
        Ein Flyout "Collapsed" Befehlsleiste<br/>
        !["Collapsed" Befehlsleiste Flyout-Beispiel](images/command-bar-flyout-img-collapsed.png)
    :::column-end:::
    :::column:::
        Eine erweiterte Befehlsleiste flyout<br/>
        ![Beispiel für eine erweiterte Befehlsleiste flyout](images/command-bar-flyout-img-expanded.png)
    :::column-end:::
:::row-end:::

```xaml
<Grid>
    <Grid.Resources>
        <CommandBarFlyout x:Name="ImageCommandsFlyout">
            <AppBarButton Icon="OutlineStar" ToolTipService.ToolTip="Favorite"/>
            <AppBarButton Icon="Copy" ToolTipService.ToolTip="Copy"/>
            <AppBarButton Icon="Share" ToolTipService.ToolTip="Share"/>
            <CommandBarFlyout.SecondaryCommands>
                <AppBarButton Label="Rotate" Icon="Rotate"/>
                <AppBarButton Label="Delete" Icon="Delete"/>
            </CommandBarFlyout.SecondaryCommands>
        </CommandBarFlyout>
    </Grid.Resources>

    <Image Source="Assets/licorice.png" Width="300"
           Tapped="Image_Tapped" FlyoutBase.AttachedFlyout="{x:Bind ImageCommandsFlyout}"
           ContextFlyout="{x:Bind ImageCommandsFlyout}"/>
</Grid>
```

```csharp
private void Image_Tapped(object sender, TappedRoutedEventArgs e)
{
    var flyout = FlyoutBase.GetAttachedFlyout((FrameworkElement)sender);
    var options = new FlyoutShowOptions()
    {
        // Position shows the flyout next to the pointer.
        // "Transient" ShowMode makes the flyout open in its collapsed state.
        Position = e.GetPosition((FrameworkElement)sender),
        ShowMode = FlyoutShowMode.Transient
    };
    flyout?.ShowAt((FrameworkElement)sender, options);
}
```

### <a name="show-commands-proactively"></a>Befehle proaktiv anzeigen

Wenn Sie Kontextbefehlen proaktiv angezeigt wird, sollte nur die primären Befehle standardmäßig angezeigt werden (die Leiste-Flyout der Befehl sollte reduziert werden). Platzieren Sie die wichtigsten Befehle in der Auflistung primäre Befehle und weitere Befehle, die normalerweise in einem Kontextmenü in die sekundären Befehle Auflistung würde.

Wenn proaktiv Befehle anzeigen möchten, behandeln Sie in der Regel das Ereignis [Klicken](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) oder [Tapped](/uwp/api/windows.ui.xaml.uielement.tapped) , um das Befehlsleiste Flyout anzuzeigen. Legen Sie das Flyout [ShowMode](/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.showmode) **vorübergehende** oder **TransientWithDismissOnPointerMoveAway** , um das Flyout im Modus "Collapsed" zu öffnen, ohne den Fokus fest.

Ab Windows 10 Insider Preview, Textsteuerelemente verfügen über eine **SelectionFlyout** -Eigenschaft. Wenn Sie diese Eigenschaft ein Flyout zuordnen, wird es automatisch angezeigt, wenn Text markiert ist.

### <a name="show-commands-reactively"></a>Befehle reaktiv anzeigen

Wenn Sie Kontextbefehlen reaktiv, als Kontextmenü angezeigt wird, sind die sekundären Befehle standardmäßig angezeigt (die Leiste-Flyout der Befehl sollte erweitert werden). In diesem Fall wird möglicherweise das Flyout Befehlsleiste primären und sekundären Befehlen oder nur sekundäre Befehle.

Um Befehle in einem Kontextmenü anzuzeigen, weisen Sie in der Regel das Flyout der [ContextFlyout](/uwp/api/windows.ui.xaml.uielement.contextflyout) -Eigenschaft eines Benutzeroberflächenelements. Auf diese Weise wird das Flyout Öffnen des Elements behandelt, und Sie müssen nichts weiter tun.

Wenn Sie verarbeiten das Flyout (z. B. auf ein Ereignis [RightTapped](/uwp/api/windows.ui.xaml.uielement.righttapped) ) angezeigt, die das Flyout [ShowMode](/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.showmode) in **Standard** , öffnen Sie das Flyout im erweiterten Modus und weisen Sie ihm den Fokus.

> [!TIP]
> Weitere Informationen zu den Optionen beim Anzeigen von einem Flyout und zur Platzierung von das Flyout zu steuern finden Sie unter [Flyouts](../controls-and-patterns/dialogs-and-flyouts/flyouts.md).

## <a name="commands-and-content"></a>Befehle und Inhalt

Das CommandBarFlyout-Steuerelement verfügt über 2 Eigenschaften Sie Befehle und Inhalte hinzufügen können: [PrimaryCommands](/uwp/api/windows.ui.xaml.controls.commandbarflyout.primarycommands) und ["secondarycommands"](/uwp/api/windows.ui.xaml.controls.commandbarflyout.secondarycommands).

Befehlsleistenelemente werden standardmäßig der **PrimaryCommands**-Sammlung hinzugefügt. Diese Befehle werden angezeigt, in der Befehlsleiste und sowohl die "Collapsed" auch im erweiterten Modus sichtbar sind. Im Gegensatz zu CommandBar primäre Befehle nicht automatisch Überlauf auf sekundäre Befehle und möglicherweise abgeschnitten.

Sie können auch Befehle zur Auflistung **"secondarycommands"** hinzufügen. Sekundäre Befehle auf das Menü Teil des Steuerelements angezeigt werden und sind nur im erweiterten Modus sichtbar.

### <a name="app-bar-buttons"></a>App-Leistenschaltflächen

Sie können die PrimaryCommands und "secondarycommands" direkt mit Steuerelementen [AppBarButton](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbarbutton.aspx), [AppBarToggleButton](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbartogglebutton.aspx)und [AppBarSeparator](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.appbarseparator.aspx) auffüllen.

Die Steuerelemente für die App-Leistenschaltfläche zeichnen sich durch ein Symbol und eine Textbeschriftung aus. Diese Steuerelemente sind für die Verwendung in Befehlsleisten optimiert, und ihre Darstellung ändert, je nachdem, ob das Steuerelement in der Befehlsleiste oder im Überlaufmenü angezeigt wird.

- App-Leistenschaltflächen als primäre Befehle verwendet werden in der Befehlsleiste mit nur ihre Symbol angezeigt. die Beschriftung wird nicht angezeigt. Es wird empfohlen, dass Sie eine QuickInfo verwenden, um einen beschreibenden Text für den Befehl anzuzeigen wie hier gezeigt.
    ```xaml
    <AppBarButton Icon="Copy" ToolTipService.ToolTip="Copy"/>
    ```
- App-Leistenschaltflächen als sekundäre Befehle verwendet werden Sie im Menü mit der Bezeichnung und die Symbol sichtbar angezeigt.

### <a name="other-content"></a>Andere Inhalte

Sie können ein Befehlsleiste Flyout eine AppBarElementContainer umschließen andere Steuerelemente hinzufügen. Auf diese Weise können Sie die Steuerelemente wie [DropDownButton]() oder [SplitButton]()hinzugefügt oder Containern wie [StackPanel]() um komplexere Benutzeroberfläche zu erstellen.

> [!NOTE]
> Um die primären oder sekundären Befehl Sammlungen Befehlsleiste Flyout hinzugefügt werden, muss ein Element die [ICommandBarElement](/uwp/api/windows.ui.xaml.controls.icommandbarelement) -Schnittstelle implementieren. AppBarElementContainer ist ein Wrapper, der diese Schnittstelle implementiert, sodass Sie ein Element zu einer Befehlsleiste hinzufügen können, auch wenn es nicht die Schnittstelle selbst implementiert.

Hier wird ein AppBarElementContainer verwendet, um zusätzliche Elemente ein Befehlsleiste Flyout hinzufügen. Die primären Befehle Auswahl an Farben ermöglicht wird ein SplitButton hinzugefügt. StackPanel wird die sekundären Befehle ein komplexeres Layouts für Zoomsteuerelemente zulassen hinzugefügt.

> [!NOTE]
> Dieses Beispiel zeigt nur Befehlsleiste Flyout der UI, sie keine Befehle implementiert, die angezeigt werden. Weitere Informationen zur Implementierung der Befehle finden Sie unter [Schaltflächen](buttons.md) und [befehlsdesigngrundlagen](../basics/commanding-basics.md).

:::row:::
    :::column:::
        Ein "Collapsed" Befehlsleiste Flyout mit einer offenen SplitButton<br/>
        ![Ein Befehlsleiste Flyout mit einer Split-Schaltfläche](images/command-bar-flyout-split-button.png)
    :::column-end:::
    :::column:::
        Eine erweiterte Befehlsleiste Flyout mit benutzerdefinierten Zoom UI im Menü ""<br/>
        ![Ein Befehlsleiste Flyout mit komplexen UI](images/command-bar-flyout-complex-ui.png)
    :::column-end:::
:::row-end:::

```xaml
<CommandBarFlyout>
    <AppBarButton Icon="Cut" ToolTipService.ToolTip="Cut"/>
    <AppBarButton Icon="Copy" ToolTipService.ToolTip="Copy"/>
    <AppBarButton Icon="Paste" ToolTipService.ToolTip="Paste"/>
    <!-- Color controls -->
    <AppBarElementContainer>
        <SplitButton Height="Auto" Margin="0,4,0,0"
                     ToolTipService.ToolTip="Colors"
                     Background="{ThemeResource AppBarItemBackgroundThemeBrush}">
            <SplitButton.Content>
                <Rectangle Width="20" Height="20">
                    <Rectangle.Fill>
                        <SolidColorBrush Color="Red"/>
                    </Rectangle.Fill>
                </Rectangle>
            </SplitButton.Content>
            <SplitButton.Flyout>
                <MenuFlyout>
                    <MenuFlyoutItem Text="Red"/>
                    <MenuFlyoutItem Text="Yellow"/>
                    <MenuFlyoutItem Text="Green"/>
                    <MenuFlyoutItem Text="Blue"/>
                </MenuFlyout>
            </SplitButton.Flyout>
        </SplitButton>
    </AppBarElementContainer>
    <!-- end Color controls -->
    <CommandBarFlyout.SecondaryCommands>
        <!-- Zoom controls -->
        <AppBarElementContainer>
            <AppBarElementContainer.Resources>
                <Style TargetType="Button">
                    <Setter Property="Background"
                            Value="{ThemeResource AppBarItemBackgroundThemeBrush}"/>
                </Style>
                <Style TargetType="TextBlock">
                    <Setter Property="VerticalAlignment" Value="Center"/>
                </Style>
            </AppBarElementContainer.Resources>
            <Grid Margin="12,0">
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="86"/>
                    <ColumnDefinition Width="Auto"/>
                </Grid.ColumnDefinitions>
                <TextBlock Text="Zoom"/>
                <StackPanel Orientation="Horizontal" Grid.Column="1">
                    <Button>
                        <SymbolIcon Symbol="Remove"/>
                    </Button>
                    <TextBlock Text="50%" Width="40"
                               HorizontalTextAlignment="Center"/>
                    <Button>
                        <SymbolIcon Symbol="Add"/>
                    </Button>
                </StackPanel>
            </Grid>
        </AppBarElementContainer>
        <!-- end Zoom controls -->
        <AppBarSeparator/>
        <AppBarButton Label="Undo" Icon="Undo"/>
        <AppBarButton Label="Redo" Icon="Redo"/>
        <AppBarButton Label="Select all"/>
    </CommandBarFlyout.SecondaryCommands>
</CommandBarFlyout>
```

## <a name="create-a-context-menu-with-secondary-commands-only"></a>Erstellen Sie ein Kontextmenü mit nur sekundäre Befehle

Sie können eine CommandBarFlyout mit nur sekundäre Befehle als ein [Kontextmenü](menus.md)anstelle einer MenuFlyout verwenden.

![Ein Befehlsleiste Flyout mit nur sekundäre Befehle](images/command-bar-flyout-context-menu.png)

```xaml
<Grid>
    <Grid.Resources>
        <!-- A command bar flyout with only secondary commands. -->
        <CommandBarFlyout x:Name="ContextMenu">
            <CommandBarFlyout.SecondaryCommands>
                <AppBarButton Label="Pin" Icon="Pin"/>
                <AppBarButton Label="Unpin" Icon="UnPin"/>
                <AppBarButton Label="Copy" Icon="Copy"/>
                <AppBarSeparator />
                <AppBarButton Label="Properties"/>
            </CommandBarFlyout.SecondaryCommands>
        </CommandBarFlyout>
    </Grid.Resources>

    <Image Source="Assets/licorice.png" Width="300"
           ContextFlyout="{x:Bind ContextMenu}"/>
</Grid>
```

Sie können auch eine CommandBarFlyout mit einer DropDownButton verwenden, zum Erstellen eines Standardmenüs.

![Ein Befehlsleiste Flyout mit als ein Dropdown-Menü "Schaltfläche"](images/command-bar-flyout-button-menu.png)

```xaml
<DropDownButton Content="Mail">
    <DropDownButton.Flyout>
        <CommandBarFlyout Placement="BottomEdgeAlignedLeft">
            <CommandBarFlyout.SecondaryCommands>
                <AppBarButton Icon="MailForward" Label="Forward"/>
                <AppBarButton Icon="MailReply" Label="Reply"/>
                <AppBarButton Icon="MailReplyAll" Label="Reply all"/>
            </CommandBarFlyout.SecondaryCommands>
        </CommandBarFlyout>
    </DropDownButton.Flyout>
</DropDownButton>
```

## <a name="command-bar-flyouts-for-text-controls"></a>Flyouts auf einer Befehlsleiste für Textsteuerelemente

Die [TextCommandBarFlyout](/uwp/api/microsoft.ui.xaml.controls.textcommandbarflyout) ist ein spezielles Befehlsleiste Flyout, die Befehle zum Bearbeiten von Text enthält. Jedes Textsteuerelement zeigt die TextCommandBarFlyout automatisch als Kontextmenü (Rechtsklick), oder wenn Text markiert ist. Die Leiste-Flyout der Text-Befehl passt sich die Textauswahl nur relevante Befehle angezeigt.

:::row:::
    :::column:::
        Ein Text Befehlsleiste Flyout für Textauswahl<br/>
        ![Ein "Collapsed" Text Befehlsleiste-flyout](images/command-bar-flyout-text-selection.png)
    :::column-end:::
    :::column:::
        Eine erweiterte Text Befehlsleiste-flyout<br/>
        ![Eine erweiterte Text Befehlsleiste-flyout](images/command-bar-flyout-text-full.png)
    :::column-end:::
:::row-end:::

### <a name="available-commands"></a>Verfügbaren Befehle

Diese Tabelle zeigt die Befehle in einer TextCommandBarFlyout, und wenn diese angezeigt werden.

| Befehl | Dargestellt … |
| ------- | -------- |
| Fett | Wenn das Text-Steuerelement keine schreibgeschützt (RichEditBox nur) ist. |
| Kursiv | Wenn das Text-Steuerelement keine schreibgeschützt (RichEditBox nur) ist. |
| Unterstreichen | Wenn das Text-Steuerelement keine schreibgeschützt (RichEditBox nur) ist. |
| Nachweis von Manipulation | Wenn IsSpellCheckEnabled **"true ist"** und falsch geschrieben wird Text ausgewählt. |
| Ausschneiden | Wenn das Text-Steuerelement ist nicht schreibgeschützt, und Text ausgewählt ist. |
| Kopieren | Wenn Text markiert ist. |
| Einfügen | Wenn das Steuerelement nicht schreibgeschützt und der Zwischenablage Inhalte. |
| Rückgängig machen | Wenn eine Aktion, die rückgängig gemacht werden kann. |
| Alle auswählen | Wenn Text ausgewählt werden können. |

### <a name="custom-text-command-bar-flyouts"></a>Benutzerdefinierter Text, der Flyouts auf einer Befehlsleiste

TextCommandBarFlyout nicht angepasst werden kann, und von jedes Textsteuerelement automatisch verwaltet wird. Allerdings können Sie die standardmäßige TextCommandBarFlyout mit benutzerdefinierten Befehlen ersetzen.

- Um die Standard-TextCommandBarFlyout ersetzen, die auf Textauswahl angezeigt wird, können Sie erstellen Sie eine benutzerdefinierte CommandBarFlyout (oder andere Flyout) und weisen sie die **SelectionFlyout** -Eigenschaft. Wenn Sie SelectionFlyout auf **null**festlegen, werden keine Befehle auf der Auswahl angezeigt.
- Um die Standard-TextCommandBarFlyout ersetzen, die im Kontextmenü angezeigt wird, weisen Sie eine benutzerdefinierte CommandBarFlyout (oder andere Flyout-Typ) für die **ContextFlyout** -Eigenschaft für ein Text-Steuerelement. Wenn Sie ContextFlyout auf **null**festgelegt, wird das Flyout "Menü" angezeigt, in früheren Versionen des Textsteuerelements anstelle der TextCommandBarFlyout angezeigt.

## <a name="get-the-sample-code"></a>Beispielcode herunterladen

- [Beispiel eines XAML-Steuerelementekatalogs](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) – Hier werden alle XAML-Steuerelemente in einem interaktiven Format dargestellt.
- [Beispiel für XAML-Befehle](http://go.microsoft.com/fwlink/p/?LinkId=620019)

## <a name="related-articles"></a>Verwandte Artikel

- [Befehlsdesigngrundlagen für UWP-Apps](../basics/commanding-basics.md)
- [CommandBar-Klasse](https://msdn.microsoft.com/library/windows/apps/dn279427)