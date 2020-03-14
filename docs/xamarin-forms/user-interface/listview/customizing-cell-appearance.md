---
title: Anpassen der Zellendarstellung ListView
description: In diesem Artikel untersucht die Optionen zum Darstellen von Daten in Xamarin.Forms-Anwendungen, und nutzt die Vorteile des ListView-Steuerelements.
ms.prod: xamarin
ms.assetid: FD45CB91-1A8F-46FB-B432-6BC20492E456
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/12/2019
ms.openlocfilehash: ab54b54c9f2f7d6d7748137ea079439b7c3ddfca
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/13/2020
ms.locfileid: "79306536"
---
# <a name="customizing-listview-cell-appearance"></a>Anpassen der Zellendarstellung ListView

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-customcells)

Die xamarin. Forms [`ListView`](xref:Xamarin.Forms.ListView) -Klasse wird verwendet, um scrollbare Listen darzustellen, die durch die Verwendung von `ViewCell` Elementen angepasst werden können. Ein `ViewCell`-Element kann Text und Bilder anzeigen, den Zustand true/false angeben und Benutzereingaben empfangen.

## <a name="built-in-cells"></a>Erstellt in Zellen
Xamarin. Forms verfügt über integrierte Zellen, die für viele Anwendungen funktionieren:

- [`TextCell`](#textcell) Steuerelemente werden zum Anzeigen von Text mit einer optionalen zweiten Zeile für Detail Text verwendet.
- [`ImageCell`](#imagecell) Steuerelemente ähneln `TextCell`s, enthalten aber ein Bild links neben dem Text.
- `SwitchCell` Steuerelemente werden verwendet, um den Status "ein/aus" oder "true/false" darzustellen und aufzuzeichnen.
- `EntryCell` Steuerelemente werden verwendet, um Textdaten darzustellen, die der Benutzer bearbeiten kann.

Die [`SwitchCell`](~/xamarin-forms/user-interface/tableview.md#switchcell) -und [`EntryCell`](~/xamarin-forms/user-interface/tableview.md#entrycell) -Steuerelemente werden üblicherweise im Kontext einer [`TableView`](~/xamarin-forms/user-interface/tableview.md)verwendet.

### <a name="textcell"></a>TextCell

[`TextCell`](xref:Xamarin.Forms.TextCell) ist eine Zelle zum Anzeigen von Text, optional mit einer zweiten Zeile als Detailtext. Der folgende Screenshot zeigt `TextCell` Elemente unter IOS und Android:

![](customizing-cell-appearance-images/text-cell-default.png "Default TextCell Example")

Textcells werden zur Laufzeit als native Steuerelemente gerendert, sodass die Leistung im Vergleich zu einer benutzerdefinierten `ViewCell`sehr gut ist. Textcells können angepasst werden, sodass Sie die folgenden Eigenschaften festlegen können:

- `Text` &ndash; den Text, der in der ersten Zeile angezeigt wird, in großer Schrift.
- `Detail` &ndash; den Text, der unterhalb der ersten Zeile angezeigt wird, in einer kleineren Schriftart.
- `TextColor` &ndash; die Farbe des Texts.
- `DetailColor` &ndash; die Farbe des Detail Texts.

Der folgende Screenshot zeigt `TextCell` Elemente mit angepassten Farbeigenschaften:

![](customizing-cell-appearance-images/text-cell-custom.png "Custom TextCell Example")

### <a name="imagecell"></a>ImageCell

[`ImageCell`](xref:Xamarin.Forms.ImageCell), wie z. b. `TextCell`, können zum Anzeigen von Text und sekundärem Detail Text verwendet werden, und die Leistung wird durch die Verwendung der nativen Steuerelemente jeder Plattform hervorragend `ImageCell` unterscheidet sich von `TextCell` darin, dass ein Bild links neben dem Text angezeigt wird.

Der folgende Screenshot zeigt `ImageCell` Elemente unter IOS und Android: !["Default ImageCell example"](customizing-cell-appearance-images/image-cell-default.png "ImageCell-Standardbeispiel")

`ImageCell` ist nützlich, wenn Sie eine Liste von Daten mit einem visuellen Aspekt anzeigen müssen, z. b. eine Liste von Kontakten oder Filmen. `ImageCell`s sind anpassbar, sodass Sie Folgendes festlegen können:

- `Text` &ndash; den Text, der in der ersten Zeile in der großen Schriftart angezeigt wird.
- `Detail` &ndash; den Text, der unterhalb der ersten Zeile angezeigt wird, in einer kleineren Schriftart.
- `TextColor` &ndash; die Textfarbe
- `DetailColor` &ndash; die Farbe des Detail Texts.
- `ImageSource` &ndash; das Bild, das neben dem Text angezeigt werden soll.

Der folgende Screenshot zeigt `ImageCell` Elemente mit angepassten Farbeigenschaften: !["angepasste ImageCell example"](customizing-cell-appearance-images/image-cell-custom.png "Beispiel für eine angepasste ImageCell")

## <a name="custom-cells"></a>Benutzerdefinierte Zellen
Mithilfe von benutzerdefinierten Zellen können Sie Zellen Layouts erstellen, die von den integrierten Zellen nicht unterstützt werden. Beispielsweise empfiehlt es sich um eine Zelle mit zwei Bezeichnungen zu präsentieren, die gleich Gewichtung. Ein `TextCell` wäre unzureichend, weil die `TextCell` eine Bezeichnung hat, die kleiner ist. Die meisten Anpassungen der Zelle hinzufügen zusätzliche schreibgeschützte Daten (z. B. zusätzliche Bezeichnungen, Bilder oder andere Anzeigeinformationen).

Alle benutzerdefinierten Zellen müssen von [`ViewCell`](xref:Xamarin.Forms.ViewCell)abgeleitet werden, der gleichen Basisklasse, die von allen integrierten Zelltypen verwendet wird.

Xamarin. Forms bietet ein [Cachingverhalten](~/xamarin-forms/user-interface/listview/performance.md#caching-strategy) für das `ListView` Steuerelement, das die Bild Laufleistung für einige Arten von benutzerdefinierten Zellen verbessern kann.

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

- Die benutzerdefinierte Zelle ist in einem `DataTemplate`geschachtelt, das sich innerhalb `ListView.ItemTemplate`befindet. Dies ist der gleiche Prozess wie die Verwendung einer beliebigen integrierten Zelle.
- `ViewCell` ist der Typ der benutzerdefinierten Zelle. Das untergeordnete Element des `DataTemplate` Elements muss die `ViewCell` Klasse aufweisen oder davon abgeleitet sein.
- Innerhalb des `ViewCell`kann das Layout von jedem xamarin. Forms-Layout verwaltet werden. In diesem Beispiel wird das Layout von einem `StackLayout`verwaltet, wodurch die Hintergrundfarbe angepasst werden kann.

> [!NOTE]
> Jede Eigenschaft `StackLayout`, die gebunden werden kann, kann innerhalb einer benutzerdefinierten Zelle gebunden werden. Diese Funktion wird im XAML-Beispiel jedoch nicht angezeigt.

### <a name="code"></a>Code

Eine benutzerdefinierte Zelle kann auch im Code erstellt werden. Zuerst muss eine benutzerdefinierte Klasse erstellt werden, die von `ViewCell` abgeleitet wird:

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

Im Seitenkonstruktor wird die `ItemTemplate`-Eigenschaft von ListView auf eine `DataTemplate` mit dem angegebenen `CustomCell` Typ festgelegt:

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

### <a name="binding-context-changes"></a>Binden von Kontextänderungen

Beim Binden an die [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) Instanzen eines benutzerdefinierten Zelltyps sollten die Benutzeroberflächen-Steuerelemente, die die `BindableProperty` Werte anzeigen, die [`OnBindingContextChanged`](xref:Xamarin.Forms.Cell.OnBindingContextChanged) -Überschreibung verwenden, um die Daten festzulegen, die in jeder Zelle angezeigt werden sollen, und nicht den zellkonstruktor, wie im folgenden Codebeispiel gezeigt:

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

Die [`OnBindingContextChanged`](xref:Xamarin.Forms.Cell.OnBindingContextChanged) Überschreibung wird aufgerufen, wenn das [`BindingContextChanged`](xref:Xamarin.Forms.BindableObject.BindingContextChanged) -Ereignis ausgelöst wird, als Reaktion auf den Wert der [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) -Eigenschaft, die geändert wird. Wenn sich der `BindingContext` ändert, sollten daher die Benutzeroberflächen-Steuerelemente, die die [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) Werte anzeigen, Ihre Daten festlegen. Beachten Sie, dass die `BindingContext` auf einen `null` Wert geprüft werden muss, da dieser von xamarin. Forms für Garbage Collection festgelegt werden kann, was wiederum dazu führt, dass das `OnBindingContextChanged`-überschreiben aufgerufen wird.

Alternativ können UI-Steuerelemente an die [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) Instanzen gebunden werden, um ihre Werte anzuzeigen. Dadurch entfällt die Notwendigkeit, die `OnBindingContextChanged`-Methode zu überschreiben.

> [!NOTE]
> Wenn Sie `OnBindingContextChanged`überschreiben, stellen Sie sicher, dass die `OnBindingContextChanged`-Methode der Basisklasse aufgerufen wird, damit registrierte Delegaten das `BindingContextChanged` Ereignis empfangen.

In XAML kann das Binden von benutzerdefinierten Zellentyps an Daten erreicht werden wie im folgenden Codebeispiel gezeigt:

```xaml
<ListView x:Name="listView">
    <ListView.ItemTemplate>
        <DataTemplate>
            <local:CustomCell Name="{Binding Name}" Age="{Binding Age}" Location="{Binding Location}" />
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

Dadurch werden die Eigenschaften `Name`, `Age`und `Location` bindbare in der `CustomCell` Instanz an die Eigenschaften `Name`, `Age`und `Location` der einzelnen Objekte in der zugrunde liegenden Auflistung gebunden.

Die entsprechende Bindung im C# -Code wird im folgenden Codebeispiel dargestellt:

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

Wenn der [`ListView`](xref:Xamarin.Forms.ListView) unter IOS und Android Elemente wieder verwendet und die benutzerdefinierte Zelle einen benutzerdefinierten Renderer verwendet, muss der benutzerdefinierte Renderer ordnungsgemäß eine Eigenschafts Änderungs Benachrichtigung implementieren. Wenn Zellen wieder verwendet werden, ändern sich Ihre Eigenschaftswerte, wenn der Bindungs Kontext auf den einer verfügbaren Zelle aktualisiert wird, wobei `PropertyChanged` Ereignisse ausgelöst werden. Weitere Informationen finden Sie unter [Anpassen einer viewcell](~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md). Weitere Informationen zur Wiederverwendung von Zellen finden Sie unter [Caching-Strategie](~/xamarin-forms/user-interface/listview/performance.md#caching-strategy).

## <a name="related-links"></a>Verwandte Links

- [Integrierte Zellen (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-builtincells)
- [Benutzerdefinierte Zellen (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-customcells)
- [Bindungs Kontext geändert (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-bindingcontextchanged)
