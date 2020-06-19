---
title: Anpassen der ListView-Zellen Darstellung
description: Dieser Artikel erläutert die Optionen für die Darstellung von Daten in Xamarin.Forms Anwendungen und nutzt dabei die Vorteile des ListView-Steuer Elements.
ms.prod: xamarin
ms.assetid: FD45CB91-1A8F-46FB-B432-6BC20492E456
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/12/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: cdede547e3ef7cf9f7b6d89751c7476a2ce66d3d
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84129011"
---
# <a name="customizing-listview-cell-appearance"></a>Anpassen der ListView-Zellen Darstellung

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-customcells)

Die- Xamarin.Forms [`ListView`](xref:Xamarin.Forms.ListView) Klasse wird verwendet, um scrollbare Listen darzustellen, die durch die Verwendung von-Elementen angepasst werden können `ViewCell` . Ein- `ViewCell` Element kann Text und Bilder anzeigen, den Zustand true/false angeben und Benutzereingaben empfangen.

## <a name="built-in-cells"></a>Integrierte Zellen
Xamarin.Formsenthält integrierte Zellen, die für viele Anwendungen funktionieren:

- [`TextCell`](#textcell)Steuerelemente werden zum Anzeigen von Text mit einer optionalen zweiten Zeile für Detail Text verwendet.
- [`ImageCell`](#imagecell)Steuerelemente ähneln `TextCell` s, enthalten aber ein Bild links neben dem Text.
- `SwitchCell`Steuerelemente werden verwendet, um den Status "ein/aus" oder "true/false" darzustellen und aufzuzeichnen.
- `EntryCell`Steuerelemente werden verwendet, um Textdaten darzustellen, die der Benutzer bearbeiten kann.

Die [`SwitchCell`](~/xamarin-forms/user-interface/tableview.md#switchcell) -und-Steuer [`EntryCell`](~/xamarin-forms/user-interface/tableview.md#entrycell) Elemente werden üblicherweise im Kontext einer verwendet [`TableView`](~/xamarin-forms/user-interface/tableview.md) .

### <a name="textcell"></a>Textcell

[`TextCell`](xref:Xamarin.Forms.TextCell)ist eine Zelle zum Anzeigen von Text, optional mit einer zweiten Zeile als Detailtext. Der folgende Screenshot zeigt `TextCell` Elemente unter IOS und Android:

![](customizing-cell-appearance-images/text-cell-default.png "Default TextCell Example")

Textcells werden zur Laufzeit als native Steuerelemente gerendert, sodass die Leistung im Vergleich zu einem benutzerdefinierten sehr gut ist `ViewCell` . Textcells können angepasst werden, sodass Sie die folgenden Eigenschaften festlegen können:

- `Text`&ndash;der Text, der in der ersten Zeile in der großen Schriftart angezeigt wird.
- `Detail`&ndash;der Text, der unterhalb der ersten Zeile angezeigt wird, in einer kleineren Schriftart.
- `TextColor`&ndash;die Textfarbe.
- `DetailColor`&ndash;die Farbe des Detail Texts.

Der folgende Screenshot zeigt `TextCell` Elemente mit angepassten Farbeigenschaften:

![](customizing-cell-appearance-images/text-cell-custom.png "Custom TextCell Example")

### <a name="imagecell"></a>ImageCell

[`ImageCell`](xref:Xamarin.Forms.ImageCell), wie `TextCell` , kann zum Anzeigen von Text und sekundärem Detail Text verwendet werden, und bietet eine hohe Leistung durch die Verwendung der nativen Steuerelemente der einzelnen Plattformen. `ImageCell`unterscheidet sich von `TextCell` insofern, dass ein Bild links neben dem Text angezeigt wird.

Der folgende Screenshot zeigt `ImageCell` Elemente unter IOS und Android: !["Default ImageCell example"](customizing-cell-appearance-images/image-cell-default.png "ImageCell-Standardbeispiel")

`ImageCell`ist nützlich, wenn Sie eine Liste von Daten mit einem visuellen Aspekt anzeigen müssen, z. b. eine Liste von Kontakten oder Filmen. `ImageCell`s sind anpassbar, sodass Sie Folgendes festlegen können:

- `Text`&ndash;der Text, der in der ersten Zeile in der großen Schriftart angezeigt wird.
- `Detail`&ndash;der Text, der unterhalb der ersten Zeile angezeigt wird, in einer kleineren Schriftart.
- `TextColor`&ndash;die Farbe des Texts.
- `DetailColor`&ndash;die Farbe des Detail Texts.
- `ImageSource`&ndash;das Bild, das neben dem Text angezeigt werden soll.

Der folgende Screenshot zeigt `ImageCell` Elemente mit angepassten Farbeigenschaften: !["angepasste ImageCell example"](customizing-cell-appearance-images/image-cell-custom.png "Beispiel für eine angepasste ImageCell")

## <a name="custom-cells"></a>Benutzerdefinierte Zellen
Mithilfe von benutzerdefinierten Zellen können Sie Zellen Layouts erstellen, die von den integrierten Zellen nicht unterstützt werden. Beispielsweise kann es vorkommen, dass Sie eine Zelle mit zwei Bezeichnungen mit gleicher Gewichtung präsentieren möchten. Ein `TextCell` ist unzureichend, weil eine `TextCell` Bezeichnung hat, die kleiner ist. Bei den meisten Zell Anpassungen werden zusätzliche schreibgeschützte Daten (z. b. zusätzliche Bezeichnungen, Bilder oder andere Anzeigeinformationen) hinzugefügt.

Alle benutzerdefinierten Zellen müssen von abgeleitet [`ViewCell`](xref:Xamarin.Forms.ViewCell) werden. Dies ist die Basisklasse, die von allen integrierten Zelltypen verwendet wird.

Xamarin.Formsbietet ein [Cachingverhalten](~/xamarin-forms/user-interface/listview/performance.md#caching-strategy) für das-Steuerelement, das die Bild Lauf `ListView` Leistung für einige Arten von benutzerdefinierten Zellen verbessern kann.

Der folgende Screenshot zeigt ein Beispiel für eine benutzerdefinierte Zelle:

![Beispiel für eine benutzerdefinierte Zelle](customizing-cell-appearance-images/custom-cell.png "Beispiel für benutzerdefinierte Zelle")

### <a name="xaml"></a>XAML
Die im vorherigen Screenshot gezeigte benutzerdefinierte Zelle kann mit dem folgenden XAML-Code erstellt werden:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="demoListView.ImageCellPage">
    <ContentPage.Content>
        <ListView  x:Name="listView">
            <ListView.ItemTemplate>
                <DataTemplate>
                    <ViewCell>
                        <StackLayout BackgroundColor="#eee"
                        Orientation="Vertical">
                            <StackLayout Orientation="Horizontal">
                                <Image Source="{Binding image}" />
                                <Label Text="{Binding title}"
                                TextColor="#f35e20" />
                                <Label Text="{Binding subtitle}"
                                HorizontalOptions="EndAndExpand"
                                TextColor="#503026" />
                            </StackLayout>
                        </StackLayout>
                    </ViewCell>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </ContentPage.Content>
</ContentPage>
```

Die XAML funktioniert wie folgt:

- Die benutzerdefinierte Zelle ist in einer geschachtelt `DataTemplate` , die sich in befindet `ListView.ItemTemplate` . Dies ist der gleiche Prozess wie die Verwendung einer beliebigen integrierten Zelle.
- `ViewCell`der Typ der benutzerdefinierten Zelle. Das untergeordnete Element des- `DataTemplate` Elements muss von der-Klasse oder davon abgeleitet sein `ViewCell` .
- Innerhalb `ViewCell` von kann das Layout durch ein beliebiges Xamarin.Forms Layout verwaltet werden. In diesem Beispiel wird das Layout von einem verwaltet `StackLayout` , wodurch die Hintergrundfarbe angepasst werden kann.

> [!NOTE]
> Jede Eigenschaft von `StackLayout` , die bindbar ist, kann innerhalb einer benutzerdefinierten Zelle gebunden werden. Diese Funktion wird im XAML-Beispiel jedoch nicht angezeigt.

### <a name="code"></a>Code

Eine benutzerdefinierte Zelle kann auch im Code erstellt werden. Zuerst muss eine benutzerdefinierte Klasse, die von abgeleitet `ViewCell` wird, erstellt werden:

```csharp
public class CustomCell : ViewCell
    {
        public CustomCell()
        {
            //instantiate each of our views
            var image = new Image ();
            StackLayout cellWrapper = new StackLayout ();
            StackLayout horizontalLayout = new StackLayout ();
            Label left = new Label ();
            Label right = new Label ();

            //set bindings
            left.SetBinding (Label.TextProperty, "title");
            right.SetBinding (Label.TextProperty, "subtitle");
            image.SetBinding (Image.SourceProperty, "image");

            //Set properties for desired design
            cellWrapper.BackgroundColor = Color.FromHex ("#eee");
            horizontalLayout.Orientation = StackOrientation.Horizontal;
            right.HorizontalOptions = LayoutOptions.EndAndExpand;
            left.TextColor = Color.FromHex ("#f35e20");
            right.TextColor = Color.FromHex ("503026");

            //add views to the view hierarchy
            horizontalLayout.Children.Add (image);
            horizontalLayout.Children.Add (left);
            horizontalLayout.Children.Add (right);
            cellWrapper.Children.Add (horizontalLayout);
            View = cellWrapper;
        }
    }
```

Im Seitenkonstruktor wird die-Eigenschaft von ListView `ItemTemplate` auf eine `DataTemplate` mit dem `CustomCell` angegebenen Typ festgelegt:

```csharp
public partial class ImageCellPage : ContentPage
    {
        public ImageCellPage ()
        {
            InitializeComponent ();
            listView.ItemTemplate = new DataTemplate (typeof(CustomCell));
        }
    }
```

### <a name="binding-context-changes"></a>Bindungs Kontext Änderungen

Beim Binden an die Instanzen eines benutzerdefinierten Zelltyps sollten die UI-Steuerelemente, die [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) die Werte anzeigen, die-Überschreibung `BindableProperty` verwenden, [`OnBindingContextChanged`](xref:Xamarin.Forms.Cell.OnBindingContextChanged) um die Daten festzulegen, die in jeder Zelle angezeigt werden sollen, und nicht den zellkonstruktor, wie im folgenden Codebeispiel gezeigt:

```csharp
public class CustomCell : ViewCell
{
    Label nameLabel, ageLabel, locationLabel;

    public static readonly BindableProperty NameProperty =
        BindableProperty.Create ("Name", typeof(string), typeof(CustomCell), "Name");
    public static readonly BindableProperty AgeProperty =
        BindableProperty.Create ("Age", typeof(int), typeof(CustomCell), 0);
    public static readonly BindableProperty LocationProperty =
        BindableProperty.Create ("Location", typeof(string), typeof(CustomCell), "Location");

    public string Name
    {
        get { return(string)GetValue (NameProperty); }
        set { SetValue (NameProperty, value); }
    }

    public int Age
    {
        get { return(int)GetValue (AgeProperty); }
        set { SetValue (AgeProperty, value); }
    }

    public string Location
    {
        get { return(string)GetValue (LocationProperty); }
        set { SetValue (LocationProperty, value); }
    }
    ...

    protected override void OnBindingContextChanged ()
    {
        base.OnBindingContextChanged ();

        if (BindingContext != null)
        {
            nameLabel.Text = Name;
            ageLabel.Text = Age.ToString ();
            locationLabel.Text = Location;
        }
    }
}
```

Die [`OnBindingContextChanged`](xref:Xamarin.Forms.Cell.OnBindingContextChanged) außer Kraft setzung wird aufgerufen, wenn das Ereignis ausgelöst wird [`BindingContextChanged`](xref:Xamarin.Forms.BindableObject.BindingContextChanged) , als Reaktion auf den Wert der geänderten [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) Eigenschaft. Daher sollten die Benutzeroberflächen-Steuerelemente, die `BindingContext` die Werte anzeigen, die Daten festlegen, wenn Sie geändert werden [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . Beachten Sie, dass `BindingContext` auf einen Wert geprüft werden muss `null` , da dieser von für Garbage Collection festgelegt werden kann Xamarin.Forms , was wiederum dazu führt, dass die `OnBindingContextChanged` außer Kraft Setzung aufgerufen wird.

Alternativ können UI-Steuerelemente an die- [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) Instanzen gebunden werden, um ihre Werte anzuzeigen. Dadurch entfällt die Notwendigkeit, die-Methode zu überschreiben `OnBindingContextChanged` .

> [!NOTE]
> Stellen Sie beim Überschreiben `OnBindingContextChanged` sicher, dass die-Methode der Basisklasse `OnBindingContextChanged` aufgerufen wird, damit registrierte Delegaten das `BindingContextChanged` Ereignis empfangen.

In XAML können Sie den benutzerdefinierten Zellentyp an Daten binden, wie im folgenden Codebeispiel gezeigt:

```xaml
<ListView x:Name="listView">
    <ListView.ItemTemplate>
        <DataTemplate>
            <local:CustomCell Name="{Binding Name}" Age="{Binding Age}" Location="{Binding Location}" />
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

Dadurch werden die `Name` `Age` `Location` bindbaren Eigenschaften, und in der- `CustomCell` Instanz an die `Name` Eigenschaften, `Age` und `Location` jedes-Objekts in der zugrunde liegenden Auflistung gebunden.

Die entsprechende Bindung in c# wird im folgenden Codebeispiel gezeigt:

```csharp
var customCell = new DataTemplate (typeof(CustomCell));
customCell.SetBinding (CustomCell.NameProperty, "Name");
customCell.SetBinding (CustomCell.AgeProperty, "Age");
customCell.SetBinding (CustomCell.LocationProperty, "Location");

var listView = new ListView
{
    ItemsSource = people,
    ItemTemplate = customCell
};
```

Wenn unter IOS und Android Elemente wieder [`ListView`](xref:Xamarin.Forms.ListView) verwendet und die benutzerdefinierte Zelle einen benutzerdefinierten Renderer verwendet, muss der benutzerdefinierte Renderer ordnungsgemäß eine Eigenschafts Änderungs Benachrichtigung implementieren. Wenn Zellen wieder verwendet werden, ändern sich die Eigenschaftswerte, wenn der Bindungs Kontext auf den einer verfügbaren Zelle aktualisiert wird, wobei `PropertyChanged` Ereignisse ausgelöst werden. Weitere Informationen finden Sie unter [Anpassen einer viewcell](~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md). Weitere Informationen zur Wiederverwendung von Zellen finden Sie unter [Caching-Strategie](~/xamarin-forms/user-interface/listview/performance.md#caching-strategy).

## <a name="related-links"></a>Verwandte Links

- [Integrierte Zellen (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-builtincells)
- [Benutzerdefinierte Zellen (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-customcells)
- [Bindungs Kontext geändert (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-bindingcontextchanged)
