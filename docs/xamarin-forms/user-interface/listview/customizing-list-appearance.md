---
title: ListView-Darstellung
description: In diesem Artikel wird erläutert, wie ListViews in xamarin. Forms-Anwendungen mithilfe von Kopfzeilen, Fußzeilen, Gruppen und Zellen mit variabler Höhe angepasst werden.
ms.prod: xamarin
ms.assetid: DC8009B0-4371-4D60-885A-5362FC7EE3E5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/13/2018
ms.openlocfilehash: f6576f1e7a0ed4aa27a40c7610b42e942c7923b4
ms.sourcegitcommit: fbccdade677a805d842ec054e44bed3d01356e93
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2019
ms.locfileid: "72798621"
---
# <a name="listview-appearance"></a>ListView-Darstellung

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-grouping)

Mit dem xamarin. Forms- [`ListView`](xref:Xamarin.Forms.ListView) können Sie die Darstellung der Liste zusätzlich zu den [`ViewCell`](xref:Xamarin.Forms.ViewCell) -Instanzen für jede Zeile in der Liste anpassen.

## <a name="grouping"></a>Gruppieren

Große Datenmengen können unhandlich werden, wenn Sie in einer fortlaufend scrollliste präsentiert werden. Das Aktivieren der Gruppierung kann die Benutzerfreundlichkeit in diesen Fällen verbessern, indem der Inhalt besser organisiert und plattformspezifische Steuerelemente aktiviert werden, die das Navigieren in Daten vereinfachen.

Wenn eine Gruppierung für eine `ListView`aktiviert ist, wird für jede Gruppe eine Kopfzeile hinzugefügt.

So aktivieren Sie die Gruppierung:

- Erstellen Sie eine Liste mit Listen (eine Liste mit Gruppen, wobei jede Gruppe eine Liste von Elementen ist).
- Legen Sie die `ItemsSource` des `ListView`auf diese Liste fest.
- Legen Sie `IsGroupingEnabled` auf true fest.
- Legen Sie [`GroupDisplayBinding`](xref:Xamarin.Forms.ListView.GroupDisplayBinding) fest, das an die-Eigenschaft der Gruppen gebunden werden soll, die als Titel der Gruppe verwendet werden.
- Optionale Legen Sie [`GroupShortNameBinding`](xref:Xamarin.Forms.ListView.GroupShortNameBinding) für die Bindung an die-Eigenschaft der Gruppen fest, die als Kurzname für die Gruppe verwendet werden. Der Kurzname wird für die Sprung Listen verwendet (Rechte Spalte auf IOS).

Beginnen Sie mit dem Erstellen einer Klasse für die Gruppen:

```csharp
public class PageTypeGroup : List<PageModel>
    {
        public string Title { get; set; }
        public string ShortName { get; set; } //will be used for jump lists
        public string Subtitle { get; set; }
        private PageTypeGroup(string title, string shortName)
        {
            Title = title;
            ShortName = shortName;
        }

        public static IList<PageTypeGroup> All { private set; get; }
    }
```

Im obigen Code ist `All` die Liste, die an "ListView" als Bindungs Quelle übergeben wird. `Title` und `ShortName` sind die Eigenschaften, die für Gruppen Überschriften verwendet werden.

In dieser Phase ist `All` eine leere Liste. Fügen Sie einen statischen Konstruktor hinzu, sodass die Liste beim Programmstart aufgefüllt wird:

```csharp
static PageTypeGroup()
{
    List<PageTypeGroup> Groups = new List<PageTypeGroup> {
            new PageTypeGroup ("Alpha", "A"){
                new PageModel("Amelia", "Cedar", new switchCellPage(),""),
                new PageModel("Alfie", "Spruce", new switchCellPage(), "grapefruit.jpg"),
                new PageModel("Ava", "Pine", new switchCellPage(), "grapefruit.jpg"),
                new PageModel("Archie", "Maple", new switchCellPage(), "grapefruit.jpg")
            },
            new PageTypeGroup ("Bravo", "B"){
                new PageModel("Brooke", "Lumia", new switchCellPage(),""),
                new PageModel("Bobby", "Xperia", new switchCellPage(), "grapefruit.jpg"),
                new PageModel("Bella", "Desire", new switchCellPage(), "grapefruit.jpg"),
                new PageModel("Ben", "Chocolate", new switchCellPage(), "grapefruit.jpg")
            }
        };
        All = Groups; //set the publicly accessible list
}
```

Im obigen Code können auch `Add` für Elemente `Groups`aufgerufen werden, bei denen es sich um Instanzen des Typs `PageTypeGroup`handelt. Diese Methode ist möglich, da `PageTypeGroup` von `List<PageModel>`erbt.

Dies ist der XAML-Code zum Anzeigen der gruppierten Liste:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DemoListView.GroupingViewPage"
    <ContentPage.Content>
        <ListView  x:Name="GroupedView"
                   GroupDisplayBinding="{Binding Title}"
                   GroupShortNameBinding="{Binding ShortName}"
                   IsGroupingEnabled="true">
            <ListView.ItemTemplate>
                <DataTemplate>
                    <TextCell Text="{Binding Title}"
                              Detail="{Binding Subtitle}" />
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </ContentPage.Content>
</ContentPage>
```

Dieser XAML-Code führt die folgenden Aktionen aus:

- Legen Sie `GroupShortNameBinding` auf die in der Group-Klasse definierte `ShortName` Eigenschaft fest.
- Legen Sie `GroupDisplayBinding` auf die in der Group-Klasse definierte `Title` Eigenschaft fest.
- `IsGroupingEnabled` auf "true" festlegen
- Die `ItemsSource` der `ListView`wurde in die gruppierte Liste geändert.

Der folgende Screenshot zeigt die resultierende Benutzeroberfläche:

![](customizing-list-appearance-images/grouping-depth.png "ListView Grouping Example")

### <a name="customizing-grouping"></a>Anpassen der Gruppierung

Wenn die Gruppierung in der Liste aktiviert wurde, kann der Gruppen Header auch angepasst werden.

Ähnlich wie die `ListView` über eine `ItemTemplate` zum Definieren der Anzeige von Zeilen verfügt `ListView` eine `GroupHeaderTemplate`.

Ein Beispiel für die Anpassung der Gruppen Kopfzeile in XAML finden Sie hier:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DemoListView.GroupingViewPage">
    <ContentPage.Content>
        <ListView x:Name="GroupedView"
                  GroupDisplayBinding="{Binding Title}"
                  GroupShortNameBinding="{Binding ShortName}"
                  IsGroupingEnabled="true">
            <ListView.ItemTemplate>
                <DataTemplate>
                    <TextCell Text="{Binding Title}"
                              Detail="{Binding Subtitle}"
                              TextColor="#f35e20"
                              DetailColor="#503026" />
                </DataTemplate>
            </ListView.ItemTemplate>
            <!-- Group Header Customization-->
            <ListView.GroupHeaderTemplate>
                <DataTemplate>
                    <TextCell Text="{Binding Title}"
                              Detail="{Binding ShortName}"
                              TextColor="#f35e20"
                              DetailColor="#503026" />
                </DataTemplate>
            </ListView.GroupHeaderTemplate>
            <!-- End Group Header Customization -->
        </ListView>
    </ContentPage.Content>
</ContentPage>
```

## <a name="headers-and-footers"></a>Kopf- und Fußzeilen

Eine ListView kann eine Kopfzeile und eine Fußzeile darstellen, die mit den Elementen der Liste scrollen. Die Kopf-und Fußzeile kann Text Zeichenfolgen oder ein komplizierteres Layout sein. Dieses Verhalten ist von [Abschnitts Gruppen](#grouping)getrennt.

Sie können die `Header` und/oder `Footer` auf einen `string` Wert festlegen, oder Sie können Sie auf ein komplexeres Layout festlegen. Außerdem gibt es `HeaderTemplate` und `FooterTemplate` Eigenschaften, mit denen Sie komplexere Layouts für die Kopf-und Fußzeile erstellen können, die die Datenbindung unterstützen.

Zum Erstellen einer einfachen Kopfzeile/Fußzeile legen Sie die Kopf-oder Fußzeilen Eigenschaften auf den Text fest, den Sie anzeigen möchten. In Code:

```csharp
ListView HeaderList = new ListView()
{
    Header = "Header",
    Footer = "Footer"
};
```

In XAML:

```xaml
<ListView x:Name="HeaderList" 
          Header="Header"
          Footer="Footer">
    ...
</ListView>
```

![](customizing-list-appearance-images/header-default.png "ListView with Header and Footer")

Um eine angepasste Kopfzeile und Fußzeile zu erstellen, definieren Sie die Kopf-und Fußzeilen Ansichten:

```xaml
<ListView.Header>
    <StackLayout Orientation="Horizontal">
        <Label Text="Header"
               TextColor="Olive"
               BackgroundColor="Red" />
    </StackLayout>
</ListView.Header>
<ListView.Footer>
    <StackLayout Orientation="Horizontal">
        <Label Text="Footer"
               TextColor="Gray"
               BackgroundColor="Blue" />
    </StackLayout>
</ListView.Footer>
```

![](customizing-list-appearance-images/header-custom.png "ListView with Customized Header and Footer")

## <a name="scrollbar-visibility"></a>Sichtbarkeit der Scrollleiste

Die [`ListView`](xref:Xamarin.Forms.ListView) -Klasse verfügt über `HorizontalScrollBarVisibility`-und `VerticalScrollBarVisibility`-Eigenschaften, die einen [`ScrollBarVisibility`](xref:Xamarin.Forms.ScrollBarVisibility) Wert erhalten oder festlegen, der angibt, wann die horizontale oder vertikale Schiebe Leiste sichtbar ist. Beide Eigenschaften können auf die folgenden Werte festgelegt werden:

- [`Default`](xref:Xamarin.Forms.ScrollBarVisibility) gibt das Standardverhalten der Bild Lauf Leiste für die Plattform an, und ist der Standardwert für die Eigenschaften `HorizontalScrollBarVisibility` und `VerticalScrollBarVisibility`.
- [`Always`](xref:Xamarin.Forms.ScrollBarVisibility) gibt an, dass Scrollleisten sichtbar sind, auch wenn der Inhalt in die Ansicht passt.
- [`Never`](xref:Xamarin.Forms.ScrollBarVisibility) gibt an, dass Scrollleisten nicht sichtbar sind, auch wenn der Inhalt nicht in die Ansicht passt.

## <a name="row-separators"></a>Zeilen Trennzeichen

Zwischen `ListView` Elementen werden standardmäßig Trennlinien unter IOS und Android angezeigt. Wenn Sie die Trennlinien unter IOS und Android ausblenden möchten, legen Sie die `SeparatorVisibility`-Eigenschaft in der ListView fest. Die Optionen für `SeparatorVisibility` lauten wie folgt:

- **Standard** -zeigt eine Trennlinie für IOS und Android an.
- **None** : Blendet das Trennzeichen auf allen Plattformen aus.

Standard Sichtbarkeit:

C#:

```csharp
SeparatorDemoListView.SeparatorVisibility = SeparatorVisibility.Default;
```

XAML

```xaml
<ListView x:Name="SeparatorDemoListView" SeparatorVisibility="Default" />
```

![](customizing-list-appearance-images/separator-default.png "ListView with Default Row Separators")

Gar

C#:

```csharp
SeparatorDemoListView.SeparatorVisibility = SeparatorVisibility.None;
```

XAML

```xaml
<ListView x:Name="SeparatorDemoListView" SeparatorVisibility="None" />
```

![](customizing-list-appearance-images/separator-none.png "ListView without Row Separators")

Sie können die Farbe der Trennlinie auch über die `SeparatorColor`-Eigenschaft festlegen:

C#:

```csharp
SeparatorDemoListView.SeparatorColor = Color.Green;
```

XAML

```xaml
<ListView x:Name="SeparatorDemoListView" SeparatorColor="Green" />
```

![](customizing-list-appearance-images/separator-custom.png "ListView with Green Row Separators")

> [!NOTE]
> Wenn Sie eine dieser Eigenschaften nach dem Laden der `ListView` auf Android festlegen, kommt es zu einer hohen Leistungs Einbuße.

## <a name="row-height"></a>Zeilenhöhe

Alle Zeilen in einem ListView-Steuer Punkt haben standardmäßig dieselbe Höhe. ListView verfügt über zwei Eigenschaften, die verwendet werden können, um dieses Verhalten zu ändern:

- `HasUnevenRows` &ndash; `true`/`false` Wert werden die Zeilen unterschiedlich sein, wenn `true`festgelegt ist. Wird standardmäßig auf `false` festgelegt.
- `RowHeight` &ndash; legt die Höhe der einzelnen Zeilen fest, wenn `HasUnevenRows` `false`ist.

Sie können die Höhe aller Zeilen festlegen, indem Sie die `RowHeight`-Eigenschaft des `ListView`festlegen.

### <a name="custom-fixed-row-height"></a>Benutzerdefinierte Zeilenhöhe

C#:

```csharp
RowHeightDemoListView.RowHeight = 100;
```

XAML

```xaml
<ListView x:Name="RowHeightDemoListView" RowHeight="100" />
```

![](customizing-list-appearance-images/height-custom.png "ListView with Fixed Row Height")

### <a name="uneven-rows"></a>Ungleichmäßige Zeilen

Wenn Sie möchten, dass einzelne Zeilen unterschiedlich hoch sind, können Sie die `HasUnevenRows`-Eigenschaft auf `true`festlegen. Die Zeilenhöhe muss nicht manuell festgelegt werden, wenn `HasUnevenRows` auf `true`festgelegt wurde, da die Höhen automatisch von xamarin. Forms berechnet werden.

C#:

```csharp
RowHeightDemoListView.HasUnevenRows = true;
```

XAML

```xaml
<ListView x:Name="RowHeightDemoListView" HasUnevenRows="true" />
```

![](customizing-list-appearance-images/height-uneven.png "ListView with Uneven Rows")

### <a name="resize-rows-at-runtime"></a>Ändern der Größe von Zeilen zur Laufzeit

Einzelne `ListView` Zeilen können zur Laufzeit Programm gesteuert angepasst werden, vorausgesetzt, die `HasUnevenRows`-Eigenschaft ist auf `true`festgelegt. Die [`Cell.ForceUpdateSize`](xref:Xamarin.Forms.Cell.ForceUpdateSize) -Methode aktualisiert die Größe einer Zelle, auch wenn Sie derzeit nicht sichtbar ist, wie im folgenden Codebeispiel gezeigt:

```csharp
void OnImageTapped (object sender, EventArgs args)
{
    var image = sender as Image;
    var viewCell = image.Parent.Parent as ViewCell;

    if (image.HeightRequest < 250) {
        image.HeightRequest = image.Height + 100;
        viewCell.ForceUpdateSize ();
    }
}
```

Der `OnImageTapped`-Ereignishandler wird als Reaktion auf eine [`Image`](xref:Xamarin.Forms.Image) in einer gezapften Zelle ausgeführt und erhöht die Größe des `Image`, das in der Zelle angezeigt wird, sodass es problemlos angezeigt wird.

![](customizing-list-appearance-images/dynamic-row-resizing.png "ListView with Runtime Row Resizing")

> [!WARNING]
> Eine übermäßige Verwendung der Lauf Zeit Zeilengröße kann zu Leistungseinbußen führen.

## <a name="related-links"></a>Verwandte Links

- [Gruppierung (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-grouping)
- [Benutzerdefinierte rendereransicht (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithlistviewnative)
- [Dynamische Größe von Zeilen (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-dynamicunevenlistcells)
- [1,4 Anmerkungen zu dieser Version](http://forums.xamarin.com/discussion/35451/xamarin-forms-1-4-0-released/)
- [1,3 Anmerkungen zu dieser Version](http://forums.xamarin.com/discussion/29934/xamarin-forms-1-3-0-released/)
