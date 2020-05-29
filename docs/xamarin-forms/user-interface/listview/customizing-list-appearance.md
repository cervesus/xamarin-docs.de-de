---
title: ''
description: In diesem Artikel wird erläutert, wie ListViews in Xamarin.Forms Anwendungen mithilfe von Kopfzeilen, Fußzeilen, Gruppen und Zellen mit variabler Höhe angepasst werden.
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: c7fdecdb0ce209c88dbe9e6f4e6e6588ec4fd3fd
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2020
ms.locfileid: "84139918"
---
# <a name="listview-appearance"></a>ListView-Darstellung

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-grouping)

Mit Xamarin.Forms [`ListView`](xref:Xamarin.Forms.ListView) können Sie die Darstellung der Liste zusätzlich zu den [`ViewCell`](xref:Xamarin.Forms.ViewCell) Instanzen für jede Zeile in der Liste anpassen.

## <a name="grouping"></a>Gruppierung

Große Datenmengen können unhandlich werden, wenn Sie in einer fortlaufend scrollliste präsentiert werden. Das Aktivieren der Gruppierung kann die Benutzerfreundlichkeit in diesen Fällen verbessern, indem der Inhalt besser organisiert und plattformspezifische Steuerelemente aktiviert werden, die das Navigieren in Daten vereinfachen.

Wenn für eine Gruppierung aktiviert ist `ListView` , wird für jede Gruppe eine Kopfzeile hinzugefügt.

So aktivieren Sie die Gruppierung:

- Erstellen Sie eine Liste mit Listen (eine Liste mit Gruppen, wobei jede Gruppe eine Liste von Elementen ist).
- Legen `ListView` `ItemsSource` Sie auf die Liste fest.
- Legen Sie `IsGroupingEnabled` auf TRUE fest.
- Legen [`GroupDisplayBinding`](xref:Xamarin.Forms.ListView.GroupDisplayBinding) Sie fest, um eine Bindung an die-Eigenschaft der Gruppen herzustellen, die als Titel der Gruppe verwendet werden.
- Optionale Legen [`GroupShortNameBinding`](xref:Xamarin.Forms.ListView.GroupShortNameBinding) Sie fest, um eine Bindung an die-Eigenschaft der Gruppen herzustellen, die als Kurzname für die Gruppe verwendet wird. Der Kurzname wird für die Sprung Listen verwendet (Rechte Spalte auf IOS).

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

Im obigen Code `All` ist die Liste, die an "ListView" als Bindungs Quelle übergeben wird. `Title`und `ShortName` sind die Eigenschaften, die für Gruppen Überschriften verwendet werden.

In dieser Phase `All` ist eine leere Liste. Fügen Sie einen statischen Konstruktor hinzu, sodass die Liste beim Programmstart aufgefüllt wird:

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

Im obigen Code können auch `Add` für Elemente von aufgerufen `Groups` werden, bei denen es sich um Instanzen des Typs handelt `PageTypeGroup` . Diese Methode ist möglich, da `PageTypeGroup` von erbt `List<PageModel>` .

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

- Legen `GroupShortNameBinding` Sie die `ShortName` in der Group-Klasse definierte Eigenschaft fest.
- Legen `GroupDisplayBinding` Sie die `Title` in der Group-Klasse definierte Eigenschaft fest.
- Auf "true" festlegen `IsGroupingEnabled`
- Der wurde `ListView` `ItemsSource` in die gruppierte Liste geändert.

Der folgende Screenshot zeigt die resultierende Benutzeroberfläche:

![](customizing-list-appearance-images/grouping-depth.png "ListView Grouping Example")

### <a name="customizing-grouping"></a>Anpassen der Gruppierung

Wenn die Gruppierung in der Liste aktiviert wurde, kann der Gruppen Header auch angepasst werden.

Ähnlich wie bei einem `ListView` `ItemTemplate` zum Definieren der Art und Weise, in der Zeilen angezeigt werden, `ListView` verfügt über einen `GroupHeaderTemplate` .

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

Sie können den `Header` Wert für und/oder `Footer` auf einen `string` Wert festlegen, oder Sie können ihn auf ein komplexeres Layout festlegen. Außerdem gibt es `HeaderTemplate` die `FooterTemplate` Eigenschaften und, mit denen Sie komplexere Layouts für die Kopf-und Fußzeile erstellen können, die die Datenbindung unterstützen.

Zum Erstellen einer einfachen Kopfzeile/Fußzeile legen Sie die Kopf-oder Fußzeilen Eigenschaften auf den Text fest, den Sie anzeigen möchten. Im Code:

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

Die [`ListView`](xref:Xamarin.Forms.ListView) -Klasse verfügt über die `HorizontalScrollBarVisibility` -Eigenschaft und die-Eigenschaft `VerticalScrollBarVisibility` , die einen Wert, der [`ScrollBarVisibility`](xref:Xamarin.Forms.ScrollBarVisibility) angibt, wann die horizontale oder vertikale Schiebe Leiste sichtbar ist Beide Eigenschaften können auf die folgenden Werte festgelegt werden:

- [`Default`](xref:Xamarin.Forms.ScrollBarVisibility)Gibt das Standardverhalten der Bild Lauf Leiste für die Plattform an, und ist der Standardwert für die `HorizontalScrollBarVisibility` -Eigenschaft und die-Eigenschaft `VerticalScrollBarVisibility` .
- [`Always`](xref:Xamarin.Forms.ScrollBarVisibility)Gibt an, dass Scrollleisten sichtbar sind, auch wenn der Inhalt in die Ansicht passt.
- [`Never`](xref:Xamarin.Forms.ScrollBarVisibility)Gibt an, dass Scrollleisten nicht sichtbar sind, auch wenn der Inhalt nicht in die Ansicht passt.

## <a name="row-separators"></a>Zeilen Trennzeichen

Trennlinien werden unter `ListView` IOS und Android standardmäßig zwischen Elementen angezeigt. Wenn Sie die Trennlinien unter IOS und Android ausblenden möchten, legen Sie die- `SeparatorVisibility` Eigenschaft in der ListView fest. Die Optionen für lauten `SeparatorVisibility` :

- **Standard** -zeigt eine Trennlinie für IOS und Android an.
- **None** : Blendet das Trennzeichen auf allen Plattformen aus.

Standard Sichtbarkeit:

C#:

```csharp
SeparatorDemoListView.SeparatorVisibility = SeparatorVisibility.Default;
```

XAML:

```xaml
<ListView x:Name="SeparatorDemoListView" SeparatorVisibility="Default" />
```

![](customizing-list-appearance-images/separator-default.png "ListView with Default Row Separators")

Keine:

C#:

```csharp
SeparatorDemoListView.SeparatorVisibility = SeparatorVisibility.None;
```

XAML:

```xaml
<ListView x:Name="SeparatorDemoListView" SeparatorVisibility="None" />
```

![](customizing-list-appearance-images/separator-none.png "ListView without Row Separators")

Sie können die Farbe der Trennlinie auch über die- `SeparatorColor` Eigenschaft festlegen:

C#:

```csharp
SeparatorDemoListView.SeparatorColor = Color.Green;
```

XAML:

```xaml
<ListView x:Name="SeparatorDemoListView" SeparatorColor="Green" />
```

![](customizing-list-appearance-images/separator-custom.png "ListView with Green Row Separators")

> [!NOTE]
> Wenn Sie eine dieser Eigenschaften auf Android festlegen, nachdem das geladen wurde, kommt es zu `ListView` einer hohen Leistungs Einbuße.

## <a name="row-height"></a>Zeilenhöhe

Alle Zeilen in einem ListView-Steuer Punkt haben standardmäßig dieselbe Höhe. ListView verfügt über zwei Eigenschaften, die verwendet werden können, um dieses Verhalten zu ändern:

- `HasUnevenRows`&ndash; `true`/`false` Wert: Zeilen haben unterschiedliche Höhen, wenn auf festgelegt `true` . Dies ist standardmäßig `false`.
- `RowHeight`&ndash;legt die Höhe der einzelnen Zeilen fest, wenn den Wert hat `HasUnevenRows` `false` .

Sie können die Höhe aller Zeilen festlegen, indem Sie die- `RowHeight` Eigenschaft auf festlegen `ListView` .

### <a name="custom-fixed-row-height"></a>Benutzerdefinierte Zeilenhöhe

C#:

```csharp
RowHeightDemoListView.RowHeight = 100;
```

XAML:

```xaml
<ListView x:Name="RowHeightDemoListView" RowHeight="100" />
```

![](customizing-list-appearance-images/height-custom.png "ListView with Fixed Row Height")

### <a name="uneven-rows"></a>Ungleichmäßige Zeilen

Wenn Sie möchten, dass einzelne Zeilen unterschiedlich hoch sind, können Sie die- `HasUnevenRows` Eigenschaft auf festlegen `true` . Zeilen Höhen müssen nicht manuell festgelegt werden `HasUnevenRows` , wenn auf festgelegt wurde `true` , da die Höhe automatisch von berechnet wird Xamarin.Forms .

C#:

```csharp
RowHeightDemoListView.HasUnevenRows = true;
```

XAML:

```xaml
<ListView x:Name="RowHeightDemoListView" HasUnevenRows="true" />
```

![](customizing-list-appearance-images/height-uneven.png "ListView with Uneven Rows")

### <a name="resize-rows-at-runtime"></a>Ändern der Größe von Zeilen zur Laufzeit

`ListView`Die Größe einzelner Zeilen kann zur Laufzeit Programm gesteuert geändert werden, sofern die- `HasUnevenRows` Eigenschaft auf festgelegt ist `true` . Die [`Cell.ForceUpdateSize`](xref:Xamarin.Forms.Cell.ForceUpdateSize) -Methode aktualisiert die Größe einer Zelle, auch wenn Sie derzeit nicht sichtbar ist, wie im folgenden Codebeispiel gezeigt:

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

Der `OnImageTapped` Ereignishandler wird als Reaktion auf einen [`Image`](xref:Xamarin.Forms.Image) in einer zu gezapften Zelle ausgeführt und erhöht die Größe des `Image` in der Zelle angezeigten, sodass er problemlos angezeigt werden kann.

![](customizing-list-appearance-images/dynamic-row-resizing.png "ListView with Runtime Row Resizing")

> [!WARNING]
> Eine übermäßige Verwendung der Lauf Zeit Zeilengröße kann zu Leistungseinbußen führen.

## <a name="related-links"></a>Verwandte Links

- [Gruppierung (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-grouping)
- [Benutzerdefinierte rendereransicht (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithlistviewnative)
- [Dynamische Größe von Zeilen (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-dynamicunevenlistcells)
- [1,4 Anmerkungen zu dieser Version](https://forums.xamarin.com/discussion/35451/xamarin-forms-1-4-0-released/)
- [1,3 Anmerkungen zu dieser Version](https://forums.xamarin.com/discussion/29934/xamarin-forms-1-3-0-released/)
