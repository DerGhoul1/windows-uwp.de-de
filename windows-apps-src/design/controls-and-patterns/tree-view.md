---
author: Jwmsft
description: Verwenden Sie den Beispielcode zur Strukturansicht, um eine ausklappbare Struktur (einen ausklappbaren Baum) zu erzeugen.
title: Strukturansicht
label: Tree view
template: detail.hbs
ms.author: jimwalk
ms.localizationpriority: medium
pm-contact: predavid
design-contact: ksulliv
dev-contact: joyate
doc-status: Published
ms.openlocfilehash: c92d0c6517456f180dcc84b60cbc6ca3a53ea282
ms.sourcegitcommit: 346b5c9298a6e9e78acf05944bfe13624ea7062e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/05/2018
ms.locfileid: "1707145"
---
# <a name="treeview"></a>TreeView

> [!IMPORTANT]
> In diesem Artikel werden Funktionen beschrieben, die noch nicht veröffentlicht wurden und vor der kommerziellen Freigabe evtl. geändert werden. Microsoft übernimmt keine Garantie, weder ausdrücklich noch stillschweigend, für die hier bereitgestellten Informationen.

Das Steuerelement „XAML-Strukturansicht“ ermöglicht eine Hierarchieauflistung mit Knoten, die das Aus- und Einblenden von geschachtelten Elementen erlauben. Es kann verwendet werden, um eine Ordnerstruktur oder geschachtelte Beziehungen zwischen Elementen in der Benutzeroberfläche zu veranschaulichen.

> **Wichtige APIs**: [TreeView-Klasse](/uwp/api/windows.ui.xaml.controls.treeview), [TreeViewNode-Klasse](/uwp/api/windows.ui.xaml.controls.treeviewnode)

Die TreeView-APIs unterstützen die folgenden Features:

- Schachtelung von N Ebenen
- Aus-/Einblenden von Knoten
- Auswahl einzelner oder mehreren Knoten

## <a name="is-this-the-right-control"></a>Ist dies das richtige Steuerelement?

- Verwenden Sie eine Strukturansicht, wenn Elemente geschachtelte Elemente haben, und dann, wenn es wichtig ist, die hierarchische Beziehung zwischen Elementen und ihren gleichgestellten Elementen und Knoten zu veranschaulichen.

- Vermeiden Sie Strukturansicht, wenn die geschachtelte Beziehung eines Elements nicht im Vordergrund steht. Für die meisten Drilldown-Szenarios eignet sich eine normale ListView

## <a name="treeview-ui"></a>TreeView-UI

Die Strukturansicht verwendet eine Kombination aus Einzügen und Symbolen, um die geschachtelte Beziehung zwischen Ordnern und übergeordneten Knoten bzw. Nicht-Ordnern und untergeordneten Knoten darzustellen. Ausgeblendete Knoten verwenden ein Chevron, welches nach rechts zeigt, während eingeblendete Knoten ein Chevron verwenden, das nach unten zeigt.

![Das Chevronsymbol in der Strukturansicht](images/treeview_chevron.png)

Sie können ein Symbol in die Datenvorlage für Strukturansichtselemente einschließen, um Knoten darzustellen. In diesem Fall sollten Sie ein Ordnersymbol nur für Knoten verwenden, die literale Ordner wie etwa die Ordnerstruktur auf einem Datenträger darstellen.

![Das Chevron- und Ordnersymbol zusammen in einer Strukturansicht](images/treeview_chevron_folder.png)

## <a name="create-a-tree-view"></a>Erstellen einer Strukturansicht

Um eine Strukturansicht zu erstellen, verwenden Sie ein [Strukturansicht](/uwp/api/windows.ui.xaml.controls.treeview)-Steuerelement und eine Hierarchie von [TreeViewNode](/uwp/api/windows.ui.xaml.controls.treeviewnode)-Objekten. Sie erstellen die Kontenhierarchie, indem Sie der RootNodes-Sammlung des Strukturansicht-Steuerelements mindestens einen Stammknoten hinzufügen. Der Sammlung der untergeordneten Elemente der einzelnen TreeViewNodes können dann weitere Knoten hinzugefügt haben. Sie können Knoten der Strukturansicht auf jede beliebigen Tiefe schachteln.

Im Folgenden finden Sie ein Beispiel für eine einfache in XAML deklarierte Strukturansicht. Sie fügen die Knoten in der Regel im Code hinzu, aber hier wird die XAML-Hierarchie angezeigt, da sie beim Visualisieren der Erstellung der Knotenhierarchie hilfreich sein kann.

```xaml
<TreeView>
    <TreeView.RootNodes>
        <TreeViewNode Content="Flavors" IsExpanded="True">
            <TreeViewNode.Children>
                <TreeViewNode Content="Vanilla"/>
                <TreeViewNode Content="Strawberry"/>
                <TreeViewNode Content="Chocolate"/>
            </TreeViewNode.Children>
        </TreeViewNode>
    </TreeView.RootNodes>
</TreeView>
```

In den meisten Fällen zeigt die Strukturansicht Daten aus einer Datenquelle an. Daher deklarieren Sie das Steuerelement der Stammstrukturansicht in der Regel in XAML, fügen die TreeViewNode-Objekte jedoch im Code hinzu.

Diese Strukturansicht ist identisch mit der zuvor in XAML erstellten, aber die Knoten werden stattdessen im Code erstellt.

```xaml
<TreeView x:Name="sampleTreeView"/>
```

```csharp
private void InitializeTreeView()
{
    TreeViewNode rootNode = new TreeViewNode() { Content = "Flavors" };
    rootNode.IsExpanded = true;
    rootNode.Children.Add(new TreeViewNode() { Content = "Vanilla" });
    rootNode.Children.Add(new TreeViewNode() { Content = "Strawberry" });
    rootNode.Children.Add(new TreeViewNode() { Content = "Chocolate" });

    sampleTreeView.RootNodes.Add(rootNode);
}
```

Diese APIs sind für die Verwaltung der Datenhierarchie der Strukturansicht verfügbar.

| **[TreeView](/uwp/api/windows.ui.xaml.controls.treeview)** | |
| - | - |
| [RootNodes](/uwp/api/windows.ui.xaml.controls.treeview.rootnodes) | Eine Strukturansicht kann einen oder mehrere Stammknoten aufweisen. Fügen Sie der RootNodes-Sammlung ein TreeViewNode-Objekt hinzu, um einen Stammknoten zu erstellen. Das **übergeordnete Element** eines Stammknotens hat immer den Wert **null**. Die **Tiefe** eines Stammknotens beträgt 0. |

| **[TreeViewNode](/uwp/api/windows.ui.xaml.controls.treeviewnode)** | |
| - | - |
| [Untergeordnete Elemente](/uwp/api/windows.ui.xaml.controls.treeviewnode.children) | Fügen Sie der Sammlung der untergeordneten Elemente des TreeViewNode-Objekts einen übergeordneten Knoten hinzu, um eine Knotenhierarchie zu erstellen. Ein Knoten ist das **übergeordnete Element** aller Knoten in der Sammlung der **untergeordneten Elemente**. |
| [HasChildren](/uwp/api/windows.ui.xaml.controls.treeviewnode.haschildren) | Ist **true**, wenn der Knoten untergeordnete Elemente erkannt hat. **false** gibt einen leeren Ordner oder ein Element an. |
| [HasUnrealizedChildren](/uwp/api/windows.ui.xaml.controls.treeviewnode.hasunrealizedchildren) | Verwenden Sie diese Eigenschaft, wenn Sie Knoten beim Erweitern ausgefüllt werden. Weitere Informationen finden Sie unter _Ausfüllen eines Knotens, wenn er erweitert wird_ weiter unten in diesem Artikel. |
| [Tiefe](/uwp/api/windows.ui.xaml.controls.treeviewnode.depth) | Gibt an, wie weit der Stammknoten von einem untergeordneten Knoten entfernt ist. |
| [Parent](/uwp/api/windows.ui.xaml.controls.treeviewnode.parent) | Ruft den TreeViewNode ab, der die Sammlung der **untergeordneten Elemente** besitzt, zu der dieser Knoten gehört. |

Die Strukturansicht verwendet die Eigenschaften **HasChildren** und **HasUnrealizedChildren**, um zu ermitteln, ob das Symbol „Erweitern/Reduzieren“ angezeigt wird. Wenn eine der Eigenschaften den Wert **true** hat, wird das Symbol angezeigt, andernfalls nicht.

## <a name="tree-view-node-content"></a>Inhalt des Knotens der Strukturansicht

Sie können das Datenelement, das ein Knoten darstellt, in dessen [Inhalte](/uwp/api/windows.ui.xaml.controls.treeviewnode.content)-Eigenschaft speichern.

Im vorherigen Beispielen war der Inhalt ein einfachen Zeichenfolgenwert. Hier stellt ein Knoten der Strukturansicht den Ordner „Bilder“ des Benutzers dar. Daher ist die Bildbibliothek [StorageFolder](/uwp/api/windows.storage.storagefolder) der Inhaltseigenschaft des Knotens zugewiesen.

```csharp
StorageFolder picturesFolder = KnownFolders.PicturesLibrary;
TreeViewNode pictureNode = new TreeViewNode();
pictureNode.Content = picturesFolder;
```

Sie können eine [DataTemplate](/uwp/api/windows.ui.xaml.datatemplate) angeben, um festzulegen, wie das Datenelement in der Strukturansicht angezeigt wird.

> [!NOTE]
> In Windows10, Version 1803, müssen Sie Vorlage für das TreeView-Steuerelement umschreiben und eine benutzerdefinierte ItemTemplate festlegen, falls der Inhalt keine Zeichenfolge ist. Weitere Informationen finden Sie im vollständigen Beispiel am Anfang dieses Artikels.

## <a name="interacting-with-a-tree-view"></a>Interagieren mit einer Strukturansicht

Sie können eine Strukturansicht konfigurieren, damit Benutzer auf verschiedene Weise mit ihr interagieren können:

- Knoten erweitern oder reduzieren
- Einfach- oder Mehrfachauswahlelemente
- Element durch Klicken aufrufen

### <a name="expandcollapse"></a>Erweitern/Reduzieren

Jeder Strukturknoten, der über untergeordnete Elementen verfügt, kann durch Klicken auf das Erweitern/Reduzieren-Symbol erweitert oder reduziert werden. Knoten lassen sich auch programmgesteuert erweitern oder reduzieren und reagieren, wenn der Zustand eines Knotens geändert wird.

#### <a name="expandcollapse-a-node-programmatically"></a>Programmgesteuertes Erweitern/Reduzieren von Knoten

Es gibt 2 Methoden den Knoten einer Strukturansicht in Ihrem Code zu erweitern oder zu reduzieren.

- Die [TreeView](/uwp/api/windows.ui.xaml.controls.treeview)-Klasse verfügt über die Methoden [Reduzieren](/uwp/api/windows.ui.xaml.controls.treeview.collapse) und [Erweitern](/uwp/api/windows.ui.xaml.controls.treeview.expand). Wenn Sie diese Methoden aufrufen, übergeben Sie den TreeViewNode, den Sie erweitern oder reduzieren möchten.

- Jeder [TreeViewNode](/uwp/api/windows.ui.xaml.controls.treeviewnode) verfügt über die Eigenschaft [IsExpanded](/uwp/api/windows.ui.xaml.controls.treeviewnode.isexpanded). Mit dieser Eigenschaft können Sie den Status eines Knotens überprüfen oder ihn so festlegen, dass sein Zustand geändert wird. Diese Eigenschaft lässt sich auch in XAML einstellen, um den Anfangszustand eines Knotens festzulegen.

### <a name="fill-a-node-when-its-expanding"></a>Ausfüllen eines Knotens, wenn er erweitert wird

Sie müssen möglicherweise eine große Anzahl von Knoten in der Strukturansicht anzeigen, oder Sie wissen möglicherweise nicht vorab, wie viele Knoten diese aufweisen wird. Das TreeView-Steuerelement ist nicht virtualisiert, damit Sie Ressourcen verwalten können, indem Sie jeden Knoten ausfüllen, wenn er erweitert wird, und die untergeordneten Knoten entfernen, wenn sie reduziert werden.

Behandeln Sie das [Erweitern](/uwp/api/windows.ui.xaml.controls.treeview.expand)-Ereignis und verwenden Sie die [HasUnrealizedChildren](/uwp/api/windows.ui.xaml.controls.treeviewnode.hasunrealizedchildren) -Eigenschaft, um einem Knoten untergeordnete Elemente hinzuzufügen, wenn er erweitert wird. Die HasUnrealizedChildren-Eigenschaft gibt an, ob der Knoten ausgefüllt werden muss oder ob dessen Sammlung der untergeordneten Elemente bereits aufgefüllt wurde. Es ist wichtig, dass dieser Wert nicht durch den TreeViewNode festgelegt, sondern von Ihnen in Ihrem App-Code verwaltet wird.

Im Folgenden finden Sie ein Beispiel für diese verwendeten APIs. Der vollständige Beispielcode ist am Ende dieses Artikels aufgeführt, darunter die Implementierung des „FillTreeNode”.

```csharp
private void SampleTreeView_Expanding(TreeView sender, TreeViewExpandingEventArgs args)
{
    if (args.Node.HasUnrealizedChildren)
    {
        FillTreeNode(args.Node);
    }
}
```

Es ist nicht erforderlich, Sie können jedoch auch das [Reduziert](/uwp/api/windows.ui.xaml.controls.treeview.collapsed)-Ereignis behandeln und die untergeordneten Knoten entfernen, wenn der übergeordnete Knoten geschlossen wird. Dies kann wichtig sein, wenn die Strukturansicht viele Knoten aufweist oder die Knotendaten viele Ressourcen nutzen. Berücksichtigen Sie potenzielle Leistungseinbußen durch das Ausfüllen eines Knotens bei jedem Öffnen im Vergleich zur Beibehaltung der untergeordneten Elemente an einem geschlossenen Knoten. Welche Option jeweils geeignet ist, hängt von Ihrer App ab.

Nachfolgend finden Sie ein Beispiel für einen Handler für das Reduziert-Ereignis.

```csharp
private void SampleTreeView_Collapsed(TreeView sender, TreeViewCollapsedEventArgs args)
{
    args.Node.Children.Clear();
    args.Node.HasUnrealizedChildren = true;
}
```

### <a name="invoking-an-item"></a>Aufrufen eines Elements

Ein Benutzer kann eine Aktion (behandelt das Element wie eine Schaltfläche) aufrufen, statt das Element auszuwählen. Sie behandeln das [ItemInvoked](/uwp/api/windows.ui.xaml.controls.treeview.iteminvoked)-Ereignis, um auf diese Benutzerinteraktion zu reagieren.

> [!NOTE]
> Im Gegensatz zur ListView, die über die [IsItemClickEnabled](/uwp/api/windows.ui.xaml.controls.listviewbase.isitemclickenabled)-Eigenschaft verfügt, ist das Aufrufen eines Elements für die Strukturansicht immer aktiviert. Sie können weiterhin auswählen, ob das Ereignis behandelt werden soll oder nicht.

**[TreeViewItemInvokedEventArgs](/uwp/api/windows.ui.xaml.controls.treeviewiteminvokedeventargs)-Klasse**

Die ItemInvoked-Ereignisargumente ermöglichen Ihnen den Zugriff auf das aufgerufene Element. Die [InvokedItem](/uwp/api/windows.ui.xaml.controls.treeviewiteminvokedeventargs.invokeditem)-Eigenschaft verfügt über den Knoten, der aufgerufen wurde. Sie können eine Umwandlung in TreeViewNode durchführen, um das Datenelement aus der TreeViewNode.Content-Eigenschaft abzurufen.

Nachfolgend finden Sie ein Beispiel für einen ItemInvoked-Ereignishandler. Das Datenelement ist ein [IStorageItem](/uwp/api/windows.storage.istorageitem), und in diesem Beispiel werden lediglich einige Informationen über die Datei und die Struktur angezeigt. Wenn der Knoten ein Ordnerknoten ist, erweitert oder reduziert er den Knoten gleichzeitig. Andernfalls wird der Knoten nur dann erweitert oder reduziert, wenn auf die Chevron-Schaltfläche geklickt wird.

```csharp
private void SampleTreeView_ItemInvoked(TreeView sender, TreeViewItemInvokedEventArgs args)
{
    var node = args.InvokedItem as TreeViewNode;
    if (node.Content is IStorageItem item)
    {
        FileNameTextBlock.Text = item.Name;
        FilePathTextBlock.Text = item.Path;
        TreeDepthTextBlock.Text = node.Depth.ToString();

        if (node.Content is StorageFolder)
        {
            node.IsExpanded = !node.IsExpanded;
        }
    }
}
```

### <a name="item-selection"></a>Elementauswahl

Das TreeView-Steuerelement unterstützt sowohl die Einzelauswahl als auch die Mehrfachauswahl. Die Auswahl von Knoten ist standardmäßig deaktiviert, Sie können die [TreeView.SelectionMode](/uwp/api/windows.ui.xaml.controls.treeview.selectionmode)-Eigenschaft jedoch festlegen, um die Auswahl von Knoten zu ermöglichen. Die [TreeViewSelectionMode](/uwp/api/windows.ui.xaml.controls.treeviewselectionmode)-Werte sind **Keine**, **Einzelne** und **Mehrere**.

Wenn die Auswahl aktiviert ist, wird ein Kontrollkästchen neben jedem Knoten einer Strukturansicht angezeigt und ausgewählte Elemente werden hervorgehoben. Ein Benutzer kann ein Element mithilfe eines Kontrollkästchens aktivieren oder deaktivieren. Das Element kann weiterhin aufgerufen werden, indem darauf geklickt wird.

Ausgewählte Knoten werden der [SelectedNodes](/uwp/api/windows.ui.xaml.controls.treeview.selectednodes)-Sammlung der Strukturansicht hinzugefügt. Sie können die [SelectAll](/uwp/api/windows.ui.xaml.controls.treeview.selectall)-Methode aufrufen, um alle Knoten in einer Strukturansicht auszuwählen.

> [!NOTE]
> Wenn Sie **SelectAll** aufrufen, werden alle realisierten Knoten ungeachtet der SelectionMode ausgewählt. Um ein einheitliches Benutzererlebnis zu ermöglichen, sollten Sie SelectAll nur dann aufrufen, wenn SelectionMode auf **Mehrere** festgelegt ist.

#### <a name="selection-and-realizedunrealized-nodes"></a>Auswahl und realisierte/nicht realisierte Knoten

Wenn die Strukturansicht nicht realisierte Knoten aufweist, werden sie bei der Auswahl nicht berücksichtigt. Beachten Sie bei der Auswahl mit nicht realisierten Knoten Folgendes.

- Wenn ein Benutzer einen übergeordneten Knoten auswählt, werden auch alle realisierten untergeordneten Elemente ausgewählt, die sich unter diesem übergeordneten Element befinden. Wenn alle untergeordneten Knoten ausgewählt werden, wird der übergeordnete Knoten entsprechend ebenfalls ausgewählt.
- Bei der SelectAll-Methode werden der SelectedNodes-Sammlung nur realisierte Knoten hinzugefügt.
- Wenn ein übergeordneter Knoten mit nicht realisierten untergeordneten Elementen ausgewählt wird, werden die untergeordneten Elemente ausgewählt, sobald sie realisiert werden.

## <a name="code-examples"></a>Codebeispiele

### <a name="tree-view-with-selection-enabled"></a>Strukturansicht mit aktivierter Auswahl

Dieses Beispiel zeigt, wie in XAML eine einfache Struktur für eine Strukturansicht erstellt wird. Die Strukturansicht zeigt in Kategorien angeordnete Eissorten und Garnierungen, die der Benutzer auswählen kann. Die Mehrfachauswahl ist aktiviert, und wenn der Benutzer auf eine Schaltfläche klickt, werden die SelectedItems in der Haupt-App-UI angezeigt.

```xaml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}" Padding="100">
    <SplitView IsPaneOpen="True"
               DisplayMode="Inline"
               OpenPaneLength="296">
        <SplitView.Pane>
            <TreeView x:Name="DessertTree" SelectionMode="Multiple">
                <TreeView.RootNodes>
                    <TreeViewNode Content="Flavors" IsExpanded="True">
                        <TreeViewNode.Children>
                            <TreeViewNode Content="Vanilla"/>
                            <TreeViewNode Content="Strawberry"/>
                            <TreeViewNode Content="Chocolate"/>
                        </TreeViewNode.Children>
                    </TreeViewNode>

                    <TreeViewNode Content="Toppings">
                        <TreeViewNode.Children>
                            <TreeViewNode Content="Candy">
                                <TreeViewNode.Children>
                                    <TreeViewNode Content="Chocolate"/>
                                    <TreeViewNode Content="Mint"/>
                                    <TreeViewNode Content="Sprinkles"/>
                                </TreeViewNode.Children>
                            </TreeViewNode>
                            <TreeViewNode Content="Fruits">
                                <TreeViewNode.Children>
                                    <TreeViewNode Content="Mango"/>
                                    <TreeViewNode Content="Peach"/>
                                    <TreeViewNode Content="Kiwi"/>
                                </TreeViewNode.Children>
                            </TreeViewNode>
                            <TreeViewNode Content="Berries">
                                <TreeViewNode.Children>
                                    <TreeViewNode Content="Strawberry"/>
                                    <TreeViewNode Content="Blueberry"/>
                                    <TreeViewNode Content="Blackberry"/>
                                </TreeViewNode.Children>
                            </TreeViewNode>
                        </TreeViewNode.Children>
                    </TreeViewNode>
                </TreeView.RootNodes>
            </TreeView>
        </SplitView.Pane>

        <StackPanel Grid.Column="1" Margin="12,0">
            <Button Content="Select all" Click="SelectAllButton_Click"/>
            <Button Content="Create order" Click="OrderButton_Click" Margin="0,12"/>
            <TextBlock Text="Your flavor selections:" Style="{StaticResource CaptionTextBlockStyle}"/>
            <TextBlock x:Name="FlavorList" Margin="0,0,0,12"/>
            <TextBlock Text="Your topping selections:" Style="{StaticResource CaptionTextBlockStyle}"/>
            <TextBlock x:Name="ToppingList"/>
        </StackPanel>
    </SplitView>
</Grid>
```

```csharp
private void OrderButton_Click(object sender, RoutedEventArgs e)
{
    FlavorList.Text = string.Empty;
    ToppingList.Text = string.Empty;

    foreach (TreeViewNode node in DessertTree.SelectedNodes)
    {
        if (node.Parent.Content?.ToString() == "Flavors")
        {
            FlavorList.Text += node.Content + "; ";
        }
        else if (node.HasChildren == false)
        {
            ToppingList.Text += node.Content + "; ";
        }
    }
}

private void SelectAllButton_Click(object sender, RoutedEventArgs e)
{
    if (DessertTree.SelectionMode == TreeViewSelectionMode.Multiple)
    {
        DessertTree.SelectAll();
    }
}
```

### <a name="pictures-and-music-library-tree-view"></a>Baumstruktur für Bild- und Musikbibliotheken

Dieses Beispiel zeigt, wie eine Strukturansicht erstellt wird, die den Inhalt und die Struktur der Bild- und Musikbibliotheken von Benutzern anzeigt. Da die Anzahl der Elemente vorab nicht bekannt sein kann, wird jeder Knoten beim Erweitern gefüllt und beim Reduzieren geleert.

Eine benutzerdefinierte Elementvorlage wird verwendet, um die Datenelemente vom Typ [IStorageItem](/uwp/api/windows.storage.istorageitem) anzuzeigen.

> [!IMPORTANT]
> Für den Code in diesem Beispiel sind die Funktionen PicturesLibrary und MusicLibrary erforderlich. Weitere Informationen zum Dateizugriff finden Sie unter [Berechtigungen für den Dateizugriff](../../files/file-access-permissions.md), [Aufzählen und Abfragen von Dateien und Ordnern](../../files/quickstart-listing-files-and-folders.md) und [Dateien und Ordner in den Musik-, Bild- und Videobibliotheken](../../files/quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md).

```xaml
<Page
    x:Class="TreeViewApp1.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:TreeViewApp1"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">
    <Page.Resources>
        <DataTemplate x:Key="TreeViewItemDataTemplate">
            <Grid Height="44">
                <TextBlock
                    Text="{Binding Content.DisplayName}"
                    HorizontalAlignment="Left"
                    VerticalAlignment="Center"
                    Style="{ThemeResource BodyTextBlockStyle}"/>
            </Grid>
        </DataTemplate>

        <Style TargetType="TreeView">
            <Setter Property="IsTabStop" Value="False" />
            <Setter Property="Template">
                <Setter.Value>
                    <ControlTemplate TargetType="TreeView">
                        <TreeViewList x:Name="ListControl"
                                      ItemTemplate="{StaticResource TreeViewItemDataTemplate}"
                                      ItemContainerStyle="{StaticResource TreeViewItemStyle}"
                                      CanDragItems="True"
                                      AllowDrop="True"
                                      CanReorderItems="True">
                            <TreeViewList.ItemContainerTransitions>
                                <TransitionCollection>
                                    <ContentThemeTransition />
                                    <ReorderThemeTransition />
                                    <EntranceThemeTransition IsStaggeringEnabled="False" />
                                </TransitionCollection>
                            </TreeViewList.ItemContainerTransitions>
                        </TreeViewList>
                    </ControlTemplate>
                </Setter.Value>
            </Setter>
        </Style>
    </Page.Resources>

    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <SplitView IsPaneOpen="True"
                   DisplayMode="Inline"
                   OpenPaneLength="296">
            <SplitView.Pane>
                <Grid>
                    <Grid.RowDefinitions>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition/>
                    </Grid.RowDefinitions>
                    <Button Content="Refresh tree" Click="RefreshButton_Click" Margin="24,12"/>
                    <TreeView x:Name="sampleTreeView" Grid.Row="1" SelectionMode="Single"
                              Expanding="SampleTreeView_Expanding"
                              Collapsed="SampleTreeView_Collapsed"
                              ItemInvoked="SampleTreeView_ItemInvoked"/>
                </Grid>
            </SplitView.Pane>

            <StackPanel Grid.Column="1" Margin="12,72">
                <TextBlock Text="File name:" Style="{StaticResource CaptionTextBlockStyle}"/>
                <TextBlock x:Name="FileNameTextBlock" Margin="0,0,0,12"/>

                <TextBlock Text="File path:" Style="{StaticResource CaptionTextBlockStyle}"/>
                <TextBlock x:Name="FilePathTextBlock" Margin="0,0,0,12"/>

                <TextBlock Text="Tree depth:" Style="{StaticResource CaptionTextBlockStyle}"/>
                <TextBlock x:Name="TreeDepthTextBlock" Margin="0,0,0,12"/>
            </StackPanel>
        </SplitView>
    </Grid>
</Page>
```

```csharp
public MainPage()
{
    this.InitializeComponent();
    InitializeTreeView();
}

private void InitializeTreeView()
{
    // A TreeView can have more than 1 root node. The Pictures library
    // and the Music library will each be a root node in the tree.
    // Get Pictures library.
    StorageFolder picturesFolder = KnownFolders.PicturesLibrary;
    TreeViewNode pictureNode = new TreeViewNode();
    pictureNode.Content = picturesFolder;
    pictureNode.IsExpanded = true;
    pictureNode.HasUnrealizedChildren = true;
    sampleTreeView.RootNodes.Add(pictureNode);
    FillTreeNode(pictureNode);

    // Get Music library.
    StorageFolder musicFolder = KnownFolders.MusicLibrary;
    TreeViewNode musicNode = new TreeViewNode();
    musicNode.Content = musicFolder;
    musicNode.IsExpanded = true;
    musicNode.HasUnrealizedChildren = true;
    sampleTreeView.RootNodes.Add(musicNode);
    FillTreeNode(musicNode);
}

private async void FillTreeNode(TreeViewNode node)
{
    // Get the contents of the folder represented by the current tree node.
    // Add each item as a new child node of the node that's being expanded.

    // Only process the node if it's a folder and has unrealized children.
    StorageFolder folder = null;
    if (node.Content is StorageFolder && node.HasUnrealizedChildren == true)
    {
        folder = node.Content as StorageFolder;
    }
    else
    {
        // The node isn't a folder, or it's already been filled.
        return;
    }

    IReadOnlyList<IStorageItem> itemsList = await folder.GetItemsAsync();

    if (itemsList.Count == 0)
    {
        // The item is a folder, but it's empty. Leave HasUnrealizedChildren = true so
        // that the chevron appears, but don't try to process children that aren't there.
        return;
    }

    foreach (var item in itemsList)
    {
        var newNode = new TreeViewNode();
        newNode.Content = item;

        if (item is StorageFolder)
        {
            // If the item is a folder, set HasUnrealizedChildren to true. 
            // This makes the collapsed chevron show up.
            newNode.HasUnrealizedChildren = true;
        }
        else
        {
            // Item is StorageFile. No processing needed for this scenario.
        }
        node.Children.Add(newNode);
    }
    // Children were just added to this node, so set HasUnrealizedChildren to false.
    node.HasUnrealizedChildren = false;
}

private void SampleTreeView_Expanding(TreeView sender, TreeViewExpandingEventArgs args)
{
    if (args.Node.HasUnrealizedChildren)
    {
        FillTreeNode(args.Node);
    }
}

private void SampleTreeView_Collapsed(TreeView sender, TreeViewCollapsedEventArgs args)
{
    args.Node.Children.Clear();
    args.Node.HasUnrealizedChildren = true;
}

private void SampleTreeView_ItemInvoked(TreeView sender, TreeViewItemInvokedEventArgs args)
{
    var node = args.InvokedItem as TreeViewNode;
    if (node.Content is IStorageItem item)
    {
        FileNameTextBlock.Text = item.Name;
        FilePathTextBlock.Text = item.Path;
        TreeDepthTextBlock.Text = node.Depth.ToString();

        if (node.Content is StorageFolder)
        {
            node.IsExpanded = !node.IsExpanded;
        }
    }
}

private void RefreshButton_Click(object sender, RoutedEventArgs e)
{
    sampleTreeView.RootNodes.Clear();
    InitializeTreeView();
}
```

## <a name="related-articles"></a>Verwandte Artikel

- [TreeView-Klasse](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.treeview)
- [ListView-Klasse](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listview.aspx)
- [ListView und GridView](listview-and-gridview.md)