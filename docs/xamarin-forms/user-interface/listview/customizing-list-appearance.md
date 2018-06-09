---
title: Anpassen der Darstellung von ListView
description: In diesem Artikel erläutert die Listenansichten in Xamarin.Forms Anwendungen mithilfe von Kopfzeilen, Fußzeilen, Gruppen und Zellen variabler Höhe anpassen.
ms.prod: xamarin
ms.assetid: DC8009B0-4371-4D60-885A-5362FC7EE3E5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: febf712848b81c09a4e25c824acc097e8b65e409
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/08/2018
ms.locfileid: "35245139"
---
# <a name="customizing-listview-appearance"></a>Anpassen der Darstellung von ListView

`ListView` enthält Optionen zum Steuern der Darstellung von der gesamten Liste zusätzlich zu den zugrunde liegenden `ViewCell`s. Zu den Optionen gehören:

- [**Gruppieren von** ](#Grouping) &ndash; Elemente im ListView für einfachere Navigation und verbesserte Organisation gruppiert werden sollen.
- [**Kopf- und Fußzeilen** ](#Headers_and_Footers) &ndash; Anzeigeinformationen am Anfang und Ende der Sicht, die mit den anderen Elementen einen Bildlauf durchführt.
- [**Zeile Trennzeichen** ](#Row_Separators) &ndash; Trennzeichen Linien zwischen Elementen ein- oder auszublenden.
- [**Zeilen mit Variablen Höhe** ](#Row_Heights) &ndash; Standardmäßig werden alle Zeilen die gleiche Höhe, aber dies kann geändert werden, damit Zeilen mit unterschiedlichen Höhen angezeigt werden können.

<a name="Grouping" />

## <a name="grouping"></a>Gruppieren
Häufig können große Datenmengen unhandlich, wenn in einer fortlaufend bildlauffähigen Liste dargestellt werden. Aktivieren des Gruppierung kann die erforderlichen Erfahrungsgrad des Benutzers in diesen Fällen verbessern, indem besser organisieren des Inhalts, und aktivieren die plattformspezifischen-Steuerelemente, die Navigieren durch Daten zu vereinfachen.

Wenn Gruppierung aktiviert ist, für eine `ListView`, eine Kopfzeile wird für jede Gruppe hinzugefügt.

So aktivieren Sie die Gruppierung:

- Erstellen Sie eine Liste mit Listen (eine Liste der Gruppen, jede Gruppe wird eine Liste von Elementen).
- Legen Sie die `ListView`des `ItemsSource` auf diese Liste.
- Legen Sie `IsGroupingEnabled` auf "true".
- Legen Sie [ `GroupDisplayBinding` ](http://developer.xamarin.com/api/property/Xamarin.Forms.ListView.GroupDisplayBinding/) zum Binden an die Eigenschaft der Gruppen, die als Titel der Gruppe verwendet wird.
- [Optional] Legen Sie [ `GroupShortNameBinding` ](http://developer.xamarin.com/api/property/Xamarin.Forms.ListView.GroupShortNameBinding/) zum Binden an die Eigenschaft der Gruppen, die als den Kurznamen für die Gruppe verwendet wird. Der kurze Name wird für den Sprunglisten (rechten Spalte auf iOS) verwendet.

Starten Sie durch Erstellen einer Klasse für die Gruppen:

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

Im obigen Code `All` ist die Liste, die an unsere ListView als Bindungsquelle zugewiesen wird. `Title` und `ShortName` sind die Eigenschaften, die für Gruppenköpfen verwendet werden.

In diesem Stadium `All` ist eine leere Liste. Fügen Sie einen statischen Konstruktor aus, sodass beim Start der Anwendung die Liste aufgefüllt wird:

```csharp
static PageTypeGroup()
{
    List<PageTypeGroup> Groups = new List<PageTypeGroup> {
            new PageTypeGroup ("Alfa", "A"){
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
        }
        All = Groups; //set the publicly accessible list
}
```

Im obigen Code kann es auch aufrufen `Add` für Elemente eines `groups`, wobei es sich um Instanzen eines Typs `PageTypeGroup`. Dies ist möglich, da `PageTypeGroup` erbt von `List<PageModel>`. Dies ist ein Beispiel für die Liste der Listen Muster wie oben beschrieben.

Hier wird der XAML-Code für die Anzeige von gruppierte Liste:

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

Daraus ergibt sich Folgendes:

![](customizing-list-appearance-images/grouping-depth.png "Beispiel für das ListView gruppieren")

Beachten Sie, dass wir:

- Legen Sie `GroupShortNameBinding` auf die `ShortName` -Eigenschaft in der Gruppenklasse definiert
- Legen Sie `GroupDisplayBinding` auf die `Title` -Eigenschaft in der Gruppenklasse definiert
- Legen Sie `IsGroupingEnabled` auf "true"
- Geändert die `ListView`des `ItemsSource` der gruppierte Liste

### <a name="customizing-grouping"></a>Anpassen der Gruppierung

Wenn in der Liste gruppieren aktiviert wurde, kann auch dem Gruppenheader angepasst werden.

Ähnlich wie die `ListView` verfügt über eine `ItemTemplate` zum definieren, wie Zeilen angezeigt werden, `ListView` verfügt über eine `GroupHeaderTemplate`.

Ein Beispiel für das Anpassen der Gruppenkopfzeile in XAML wird hier gezeigt:

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

<a name="Headers_and_Footers" />

## <a name="headers-and-footers"></a>Kopf- und Fußzeilen
Es ist möglich, dass eine Listenansicht zum Präsentieren von Kopf- und Fußzeilen, die einen Bildlauf durch die Elemente der Liste. Kopf- und Fußzeilen können Zeichenfolgen oder ein etwas komplizierteren Layout sein. Beachten Sie, dass dies getrennt von [im Abschnitt Gruppen](#Grouping).

Sie können festlegen, die `Header` und/oder `Footer` in eine einfache Zeichenfolge Wert, oder Sie können sie festlegen, eine komplexere Layout.
Es gibt auch `HeaderTemplate` und `FooterTemplate` Eigenschaften, mit denen Sie komplexe Layouts für Kopf- und Fußzeilen erstellen Datenbindung unterstützen.

Um eine einfache Kopf-/Fußzeile zu erstellen, legen Sie nur die Kopf- oder Fußzeile-Eigenschaften, auf den Text, die, den Sie anzeigen möchten. In Code:

```csharp
ListView HeaderList = new ListView() {
    Header = "Header",
    Footer = "Footer"
    };
```

In XAML:

```xaml
<ListView  x:Name="HeaderList"  Header="Header" Footer="Footer"></ListView>
```

![](customizing-list-appearance-images/header-default.png "ListView mit Kopf- und Fußzeilen")

Um eine benutzerdefinierte Kopf- und Fußzeile zu erstellen, definieren Sie Ansichten für Kopf- und Fußzeilen:

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

![](customizing-list-appearance-images/header-custom.png "ListView mit benutzerdefinierten Header und Footer")

<a name="Row_Separators" />

## <a name="row-separators"></a>Zeilentrennzeichen
Trennlinien zwischen abfragedarstellung `ListView` Elemente standardmäßig auf IOS- und Android. Wenn Sie lieber die Trennlinien auf IOS- und Android ausblenden möchten, legen Sie die `SeparatorVisibility` Eigenschaft für die Listenansicht. Die Optionen für `SeparatorVisibility` sind:

* **Standard** -zeigt eine Trennlinie auf IOS- und Android.
* **Keine** -Blendet das Trennzeichen auf allen Plattformen.

Standardsichtbarkeit:

C#:

```csharp
SepratorDemoListView.SeparatorVisibility = SeparatorVisibility.Default;
```

XAML:

```xaml
<ListView x:Name="SeparatorDemoListView" SeparatorVisibility="Default" />
```

![](customizing-list-appearance-images/separator-default.png "ListView mit Standard-Zeilentrennzeichen")

None:

C#:

```csharp
SepratorDemoListView.SeparatorVisibility = SeparatorVisibility.None;
```

XAML:

```xaml
<ListView x:Name="SeparatorDemoListView" SeparatorVisibility="None" />
```

![](customizing-list-appearance-images/separator-none.png "ListView ohne Zeilentrennzeichen")

Sie können auch festlegen, die Farbe der Linie als Trennzeichen über die `SeparatorColor` Eigenschaft:

C#:

```csharp
SepratorDemoListView.SeparatorColor = Color.Green;
```

XAML:

```xaml
<ListView x:Name="SeparatorDemoListView" SeparatorColor="Green" />
```

![](customizing-list-appearance-images/separator-custom.png "ListView mit Grün-Zeilentrennzeichen")

> [!NOTE]
> Durch Festlegen dieser Eigenschaften auf Android-Geräten nach dem Laden der `ListView` einem großen Leistungsverlust.

<a name="Row_Heights" />

## <a name="row-heights"></a>Zeilenhöhe
Alle Zeilen in einem ListView haben die gleiche Höhe standardmäßig. ListView verfügt über zwei Eigenschaften, die verwendet werden können, um dieses Verhalten zu ändern:

- `HasUnevenRows` &ndash; `true`/`false` Wert Zeilen haben unterschiedliche Höhen, wenn auf festgelegt `true`. Wird standardmäßig auf `false` festgelegt.
- `RowHeight` &ndash; Legt die Höhe jeder Zeile wann `HasUnevenRows` ist `false`.

Sie können die Höhe aller Zeilen festlegen, durch Festlegen der `RowHeight` Eigenschaft auf die `ListView`.

### <a name="custom-fixed-row-height"></a>Benutzerdefinierte feste Zeilenhöhe

C#:

```csharp
RowHeightDemoListView.RowHeight = 100;
```

XAML:

```xaml
<ListView x:Name="RowHeightDemoListView" RowHeight="100" />
```

![](customizing-list-appearance-images/height-custom.png "ListView mit feste Zeilenhöhe")


### <a name="uneven-rows"></a>Ungleiche Zeilen

Wenn Sie einzelne Zeilen haben unterschiedliche Höhen verwenden möchten, legen Sie die `HasUnevenRows` Eigenschaft `true`.
Beachten Sie, dass die Zeilenhöhe manuell einmal festgelegt werden keine `HasUnevenRows` vorsieht `true`, da die Höhe von Xamarin.Forms automatisch berechnet werden.


C#:

```csharp
RowHeightDemoListView.HasUnevenRows = true;
```

XAML:

```xaml
<ListView x:Name="RowHeightDemoListView" HasUnevenRows="true" />
```

![](customizing-list-appearance-images/height-uneven.png "ListView mit ungleiche Zeilen")

### <a name="runtime-resizing-of-rows"></a>Common Language Runtime-Größenänderung von Zeilen

Einzelne `ListView` Zeilen programmgesteuert zur Laufzeit bereitgestellt, die geändert werden können die `HasUnevenRows` -Eigenschaftensatz auf `true`. Die [ `Cell.ForceUpdateSize` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Cell.ForceUpdateSize()/) Methode aktualisiert die Größe für eine Zelle, selbst wenn es derzeit nicht angezeigt wird wie im folgenden Codebeispiel wird veranschaulicht:

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

Die `OnImageTapped` -Ereignishandler ausgeführt wird, als Antwort auf eine [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) in einer Zelle wird abgerufen, und nimmt die Größe der der `Image` in der Zelle angezeigt wird, sodass es ganz einfach angezeigt wird.

![](customizing-list-appearance-images/dynamic-row-resizing.png "ListView mit Common Language Runtime Zeilengröße")

Beachten Sie, dass sichere Möglichkeit, dass eine Verringerung der Leistung, wenn diese Funktion verwendet wird.



## <a name="related-links"></a>Verwandte Links

- [Gruppierung (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/Grouping)
- [Benutzerdefinierte Ansicht der Renderer (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithListviewNative/)
- [Dynamisches Ändern der Größe von Zeilen (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/DynamicUnevenListCells/)
- [Anmerkungen zu dieser Version 1.4](http://forums.xamarin.com/discussion/35451/xamarin-forms-1-4-0-released/)
- [Anmerkungen zu dieser Version 1.3](http://forums.xamarin.com/discussion/29934/xamarin-forms-1-3-0-released/)
