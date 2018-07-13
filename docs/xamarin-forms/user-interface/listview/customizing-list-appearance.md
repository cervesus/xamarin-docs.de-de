---
title: Anpassen der Darstellung der ListView
description: In diesem Artikel wird erläutert, wie zum Anpassen von Listenansichten in Xamarin.Forms-Anwendungen mithilfe von Kopfzeilen, Fußzeilen, Gruppen und Zellen mit variabler Höhe werden.
ms.prod: xamarin
ms.assetid: DC8009B0-4371-4D60-885A-5362FC7EE3E5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: 1326a1326b4a88459e4e0a01ef590e770e3a88c0
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997347"
---
# <a name="customizing-listview-appearance"></a>Anpassen der Darstellung der ListView

`ListView` enthält Optionen zum Steuern der Darstellung der gesamten Liste zusätzlich zu den zugrunde liegenden `ViewCell`s. Zu den Optionen gehören:

- [**Gruppieren von** ](#Grouping) &ndash; gruppieren Sie Elemente in ListView für einfachere Navigation und verbesserte Organisation.
- [**Kopf- und Fußzeilen** ](#Headers_and_Footers) &ndash; Informationen am Anfang und Ende der Ansicht, die mit den anderen Elementen führt einen Bildlauf angezeigt.
- [**Zeile Trennzeichen** ](#Row_Separators) &ndash; Trennzeichen Linien zwischen den Elementen ein- oder auszublenden.
- [**Zeilen mit Variablen Höhe** ](#Row_Heights) &ndash; standardmäßig alle Zeilen, die die gleiche Höhe sind, aber dies kann geändert werden, um Zeilen mit unterschiedlichen Größen angezeigt werden können.

<a name="Grouping" />

## <a name="grouping"></a>Gruppieren
Häufig können große Mengen von Daten unhandlich, wenn in einer scrollliste fortlaufend dargestellt werden. Aktivierung Gruppierung verbessert die benutzerfreundlichkeit in diesen Fällen durch besser organisieren des Inhalts, und aktivieren plattformspezifische-Steuerelemente, die Daten der Navigation zu vereinfachen.

Wenn der Gruppierung für aktiviert ist eine `ListView`, eine Headerzeile für jede Gruppe hinzugefügt wird.

So aktivieren Sie die Gruppierung:

- Erstellen Sie eine Liste mit Listen (eine Liste der Gruppen, jede Gruppe wird eine Liste von Elementen).
- Legen Sie die `ListView`des `ItemsSource` an.
- Legen Sie `IsGroupingEnabled` auf "true".
- Legen Sie [ `GroupDisplayBinding` ](xref:Xamarin.Forms.ListView.GroupDisplayBinding) an die Eigenschaft der Gruppen gebunden werden soll, der als Titel der Gruppe verwendet wird.
- [Optional] Legen Sie [ `GroupShortNameBinding` ](xref:Xamarin.Forms.ListView.GroupShortNameBinding) an die Eigenschaft der Gruppen gebunden werden soll, die als den Kurznamen für die Gruppe verwendet wird. Der kurze Name wird für die Sprunglisten (rechten Spalte unter iOS) verwendet.

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

Im obigen Code `All` ist die Liste, die unsere ListView als Bindungsquelle zugewiesen wird. `Title` und `ShortName` sind die Eigenschaften, die für die Gruppenüberschriften verwendet werden.

In dieser Phase `All` ist eine leere Liste. Fügen Sie einen statischen Konstruktor aus, sodass beim Start der Anwendung die Liste aufgefüllt wird:

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

Wir können auch aufrufen, in dem Code oben `Add` Elemente eines `groups`, die Instanzen des Typs sind `PageTypeGroup`. Dies ist möglich, da `PageTypeGroup` erbt `List<PageModel>`. Dies ist ein Beispiel für die Liste der Listen-Muster, die wie oben beschrieben.

Hier ist die XAML zur Anzeige der gruppierten Liste:

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

Dies hat folgende Auswirkungen:

![](customizing-list-appearance-images/grouping-depth.png "ListView-Grouping-Beispiels")

Beachten Sie, dass wir:

- Legen Sie `GroupShortNameBinding` auf die `ShortName` in unserer Gruppenklasse definierte Eigenschaft
- Legen Sie `GroupDisplayBinding` auf die `Title` in unserer Gruppenklasse definierte Eigenschaft
- Legen Sie `IsGroupingEnabled` auf "true"
- Geändert der `ListView`des `ItemsSource` der gruppierte Liste

### <a name="customizing-grouping"></a>Anpassen der Gruppierung

Wenn die Gruppierung in der Liste aktiviert wurde, kann der der Gruppenheader auch angepasst werden.

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
Es ist möglich, für eine ListView zur Darstellung von Kopf- und Fußzeilen, die Scrollen mit den Elementen der Liste. Die Kopf- und Fußzeilen können Zeichenfolgen oder ein etwas komplizierteres Layout sein. Beachten Sie, dass dies unabhängig vom ist [im Abschnitt Gruppen](#Grouping).

Sie können festlegen, die `Header` und/oder `Footer` auf eine einfache Zeichenfolge-Wert, oder Sie festlegen können sie zu einem komplexeren Layout.
Es gibt auch `HeaderTemplate` und `FooterTemplate` Eigenschaften, mit denen Sie erstellen eine komplexere Layouts für die Kopf- und Fußzeile Datenbindung unterstützen.

Um eine einfache Kopfzeile/Fußzeile zu erstellen, müssen festlegen Sie die Kopf- oder Fußzeile Eigenschaften nur auf den Text an, die, den Sie anzeigen möchten. In Code:

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

![](customizing-list-appearance-images/header-default.png "ListView mit Header und Footer")

Um eine benutzerdefinierte Kopf- und Fußzeile zu erstellen, definieren Sie die Kopf- und Fußzeile Ansichten:

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
Trennzeichen für Zeilen werden angezeigt, zwischen `ListView` Elemente werden standardmäßig unter iOS und Android. Wenn Sie die Trennlinien unter iOS und Android ausblenden möchten, legen Sie die `SeparatorVisibility` Eigenschaft Ihre ListView. Die Optionen für `SeparatorVisibility` sind:

* **Standard** -zeigt eine Trennlinie unter iOS und Android.
* **Keine** -Blendet das Trennzeichen auf allen Plattformen.

Standardmäßige Sichtbarkeit:

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

Sie können auch festlegen, die Farbe der Trennlinie über die `SeparatorColor` Eigenschaft:

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
> Eine dieser Eigenschaften unter Android festlegen, nach dem Laden der `ListView` einem großen Leistungsverlust.

<a name="Row_Heights" />

## <a name="row-heights"></a>Zeilenhöhe
Alle Zeilen in einer ListView nutzen, gilt die gleiche Höhe. ListView verfügt über zwei Eigenschaften, die verwendet werden können, um dieses Verhalten zu ändern:

- `HasUnevenRows` &ndash; `true`/`false` Wert Zeilen haben unterschiedliche Höhen, wenn auf festgelegt `true`. Wird standardmäßig auf `false` festgelegt.
- `RowHeight` &ndash; Legt die Höhe der einzelnen Datenzeile, wenn `HasUnevenRows` ist `false`.

Sie können die Höhe aller Zeilen festlegen, durch Festlegen der `RowHeight` Eigenschaft für die `ListView`.

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

Wenn Sie einzelne Zeilen anhand unterschiedlicher Höhe haben möchten, legen Sie die `HasUnevenRows` Eigenschaft `true`.
Beachten Sie, dass die Zeilenhöhe manuell festgelegt werden, sobald keine `HasUnevenRows` festgelegt wurde `true`, da die Höhe von Xamarin.Forms automatisch berechnet werden.


C#:

```csharp
RowHeightDemoListView.HasUnevenRows = true;
```

XAML:

```xaml
<ListView x:Name="RowHeightDemoListView" HasUnevenRows="true" />
```

![](customizing-list-appearance-images/height-uneven.png "ListView mit ungleiche Zeilen")

### <a name="runtime-resizing-of-rows"></a>Common Language Runtime Ändern der Größe von Zeilen

Einzelne `ListView` Zeilen programmgesteuert zur Laufzeit bereitgestellt, die geändert werden können die `HasUnevenRows` -Eigenschaftensatz auf `true`. Die [ `Cell.ForceUpdateSize` ](xref:Xamarin.Forms.Cell.ForceUpdateSize) Methode aktualisiert die Größe einer Zelle, auch wenn es derzeit nicht angezeigt wird wie im folgenden Codebeispiel wird veranschaulicht:

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

Die `OnImageTapped` Ereignishandler wird ausgeführt, als Reaktion auf eine [ `Image` ](xref:Xamarin.Forms.Image) in einer Zelle getippt wird, und erhöht die Größe des der `Image` in der Zelle angezeigt wird, sodass sie ganz einfach betrachtet wird.

![](customizing-list-appearance-images/dynamic-row-resizing.png "ListView mit Common Language Runtime-Zeile Größenänderung")

Beachten Sie, dass ist es sehr wahrscheinlich, dass eine Verringerung der Leistung aus, wenn dieses Feature übermäßig beansprucht wird.



## <a name="related-links"></a>Verwandte Links

- [Gruppierung (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/Grouping)
- [Benutzerdefinierte Renderer-Ansicht (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithListviewNative/)
- [Dynamische Ändern der Größe von Zeilen (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/DynamicUnevenListCells/)
- [die Anmerkungen zu dieser Version 1.4](http://forums.xamarin.com/discussion/35451/xamarin-forms-1-4-0-released/)
- [1.3 – Anmerkungen zu dieser](http://forums.xamarin.com/discussion/29934/xamarin-forms-1-3-0-released/)
