---
title: Anpassen der Zellendarstellung ListView
description: In diesem Artikel untersucht die Optionen zum Darstellen von Daten in Xamarin.Forms-Anwendungen, und nutzt die Vorteile des ListView-Steuerelements.
ms.prod: xamarin
ms.assetid: FD45CB91-1A8F-46FB-B432-6BC20492E456
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/07/2016
ms.openlocfilehash: 067ff4758ca78f7d706c7be96ffecd10e4e57965
ms.sourcegitcommit: d8edb1b9e7fd61979014d5f5f091ee135ab70e34
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/04/2019
ms.locfileid: "55712071"
---
# <a name="customizing-listview-cell-appearance"></a>Anpassen der Zellendarstellung ListView

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/CustomCells)

ListView stellt bildlauffähige Listen, die durch die Verwendung von angepasst werden können `ViewCell`s. `ViewCells` kann für Anzeigen von Text und Bilder an, einen True/False-Status und Empfangen von Benutzereingaben verwendet werden.

Es gibt zwei Ansätze zum Abrufen des Aussehens von ListView Zellen gewünschte:

- **[Anpassen von integrierten Zellen](#Built_in_Cells)**  &ndash; einfacheren Implementierung und eine bessere Leistung auf Kosten der Anpassungsfähigkeit.
- **[Erstellen von benutzerdefinierten Zellen](#customcells)**  &ndash; mehr Kontrolle über das Endergebnis, jedoch können Sie möglicherweise Leistungsprobleme auftreten, wenn nicht ordnungsgemäß implementiert.

<a name="Built_in_Cells" />

## <a name="built-in-cells"></a>Erstellt in Zellen
Xamarin.Forms verfügt über integrierte Zellen, die für viele einfache Anwendungen funktionieren:

- **TextCell** &ndash; zum Anzeigen von Text
- **ImageCell** &ndash; zum Anzeigen eines Bilds mit Text.

Zwei weitere Zellen, [ `SwitchCell` ](~/xamarin-forms/user-interface/tableview.md#switchcell) und [ `EntryCell` ](~/xamarin-forms/user-interface/tableview.md#entrycell) sind verfügbar, aber sie nicht häufig verwendet werden, mit `ListView`. Finden Sie unter [ `TableView` ](~/xamarin-forms/user-interface/tableview.md) für Weitere Informationen zu dieser Zellen.

<a name="TextCell" />

### <a name="textcell"></a>TextCell

[`TextCell`](xref:Xamarin.Forms.TextCell) ist eine Zelle für die Anzeige von Text, optional mit einer zweiten Zeile als Details Text.

TextCells als systemeigene Steuerelemente zur Laufzeit gerendert werden, damit die Leistung sehr gut ist im Vergleich zu einer benutzerdefinierten `ViewCell`. TextCells können angepasst werden, können Sie festlegen:

- `Text` &ndash; der Text, der auf der ersten Zeile, in großer Schriftart angezeigt wird.
- `Detail` &ndash; der Text, der unterhalb der ersten Zeile in einer kleineren Schriftart angezeigt wird.
- `TextColor` &ndash; die Farbe des Texts.
- `DetailColor` &ndash; die Farbe des Texts detail

![](customizing-cell-appearance-images/text-cell-default.png "Standard-TextCell-Beispiel")

![](customizing-cell-appearance-images/text-cell-custom.png "Benutzerdefinierte TextCell-Beispiel")

<a name="ImageCell" />

### <a name="imagecell"></a>ImageCell

[`ImageCell`](xref:Xamarin.Forms.ImageCell), z. B. `TextCell`, für die Anzeige von Text und sekundären Detail Text verwendet werden kann und er bietet hohe Leistung aus, indem Sie mit nativen Steuerelementen der Plattformen. `ImageCell` unterscheidet sich von `TextCell` , da es ein Bild auf der linken Seite des Texts anzeigt.

`ImageCell` ist nützlich, wenn Sie eine Liste der Daten mit der ein visueller Aspekt, z. B. eine Liste der Kontakte oder Filme anzeigen müssen. ImageCells können angepasst werden, können Sie festlegen:

- `Text` &ndash; der Text, der auf der ersten Zeile, in großer Schriftart angezeigt wird
- `Detail` &ndash; der Text, der unterhalb der ersten Zeile in einer kleineren Schriftart angezeigt wird
- `TextColor` &ndash; die Farbe des Texts
- `DetailColor` &ndash; die Farbe des Texts detail
- `ImageSource` &ndash; das Bild an neben dem Text angezeigt.

![](customizing-cell-appearance-images/image-cell-default.png "Standard-ImageCell-Beispiel")

![](customizing-cell-appearance-images/image-cell-custom.png "Benutzerdefinierte ImageCell-Beispiel")

<a name="customcells" />

## <a name="custom-cells"></a>Benutzerdefinierte Zellen
Die integrierten Zellen nicht das erforderliche Layout bereitstellen, implementiert benutzerdefinierte Zellen das Layout erforderliche. Beispielsweise empfiehlt es sich um eine Zelle mit zwei Bezeichnungen zu präsentieren, die gleich Gewichtung. Ein `TextCell` wäre nicht genügend, da die `TextCell` enthält eine Bezeichnung, die kleiner ist. Die meisten Anpassungen der Zelle hinzufügen zusätzliche schreibgeschützte Daten (z. B. zusätzliche Bezeichnungen, Bilder oder andere Anzeigeinformationen).

Alle benutzerdefinierte Zellen abgeleitet müssen [ `ViewCell` ](xref:Xamarin.Forms.ViewCell), der gleichen Basisklasse an, dass alle der Zelle integrierte Typen verwenden.

Xamarin.Forms 2 eingeführt, ein neues [Verhalten beim Zwischenspeichern](~/xamarin-forms/user-interface/listview/performance.md#cachingstrategy) auf die `ListView` steuern, welche zum Verbessern der bildlaufleistung für einige Typen von benutzerdefinierten Zellen festgelegt werden kann.

Dies ist ein Beispiel für eine benutzerdefinierte Zelle:

![](customizing-cell-appearance-images/custom-cell.png "Beispiel für die benutzerdefinierte Zelle")

### <a name="xaml"></a>XAML
Das XAML das obige Layout erstellen, lautet wie folgt:

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

Der obige XAML macht viel. Wir unterteilen:

- Die benutzerdefinierte Zelle darin geschachtelt ist eine `DataTemplate`, d.h. in `ListView.ItemTemplate`. Dies ist genauso wie mit der eine beliebige andere Zelle.
- `ViewCell` ist der Typ der benutzerdefinierten Zelle. Das untergeordnete Element die `DataTemplate` Element sein oder vom Typ abgeleitet werden muss `ViewCell`.
- Beachten Sie, dass dieses innerhalb der `ViewCell`, Layout wird verwaltet, indem eine `StackLayout`. Dieses Layout ermöglicht uns, die die Hintergrundfarbe anpassen. Beachten Sie, dass eine beliebige Eigenschaft des `StackLayout` , bindbare kann innerhalb einer benutzerdefinierten Zelle gebunden werden, obwohl, hier nicht angezeigt werden.
- In der `ViewCell`, Layout von Xamarin.Forms Layouts verwaltet werden kann. 

### <a name="cnum"></a>C&num;

Durch das Angeben einer benutzerdefinierten Zelle in C# geschrieben ist ausführlicher als die XAML-Entsprechung. Sehen wir uns das einmal genauer an:

Definieren Sie zuerst eine benutzerdefinierte Zellenklasse mit `ViewCell` als Basisklasse:

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

Im Konstruktor für die Seite mit den `ListView`, legen Sie das ListView `ItemTemplate` Eigenschaft, um ein neues `DataTemplate`:

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

Beachten Sie, dass der Konstruktor für `DataTemplate` akzeptiert einen Typ. Der Typeof-Operator Ruft die CLR-Typ `CustomCell`.

<a name="binding-context-changes" />

### <a name="binding-context-changes"></a>Binden von Kontextänderungen

Beim Binden an einen benutzerdefinierten Zellentyps [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) Instanzen, die Anzeige von UI-Steuerelemente der `BindableProperty` Werte verwenden, sollten die [ `OnBindingContextChanged` ](xref:Xamarin.Forms.Cell.OnBindingContextChanged) überschreiben, um die Daten angezeigt werden jede Zelle, anstatt die Cell-Konstruktor ein, wie im folgenden Codebeispiel gezeigt:

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

    public string Name {
        get { return(string)GetValue (NameProperty); }
        set { SetValue (NameProperty, value); }
    }

    public int Age {
        get { return(int)GetValue (AgeProperty); }
        set { SetValue (AgeProperty, value); }
    }

    public string Location {
        get { return(string)GetValue (LocationProperty); }
        set { SetValue (LocationProperty, value); }
    }
    ...

    protected override void OnBindingContextChanged ()
    {
        base.OnBindingContextChanged ();

        if (BindingContext != null) {
            nameLabel.Text = Name;
            ageLabel.Text = Age.ToString ();
            locationLabel.Text = Location;
        }
    }
}
```

Die [ `OnBindingContextChanged` ](xref:Xamarin.Forms.Cell.OnBindingContextChanged) Außerkraftsetzung wird aufgerufen, wenn die [ `BindingContextChanged` ](xref:Xamarin.Forms.BindableObject.BindingContextChanged) Ereignis wird ausgelöst, als Reaktion auf den Wert des der [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) Eigenschaft zu ändern. Aus diesem Grund, wenn die `BindingContext` geändert wird, die UI-Steuerelemente, die Anzeige der [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) Werte sollten ihre Daten festgelegt. Beachten Sie, dass die `BindingContext` sollte überprüft werden eine `null` -Wert fest, wie dies von Xamarin.Forms für die Garbagecollection festgelegt werden kann, die wiederum führt die `OnBindingContextChanged` außer Kraft setzen, die aufgerufen wird.

Sie können Benutzeroberflächen-Steuerelemente binden, um die [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) Instanzen zum Anzeigen von deren Werte, außer Kraft setzen müssen entfernt die `OnBindingContextChanged` Methode.

> [!NOTE]
> Beim Überschreiben von `OnBindingContextChanged`, stellen Sie sicher, dass der Basisklasse `OnBindingContextChanged` Methode wird aufgerufen, damit registrierte Delegaten empfangen die `BindingContextChanged` Ereignis.

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

Bindet die `Name`, `Age`, und `Location` bindbare Eigenschaften in der `CustomCell` Instanz, zu der `Name`, `Age`, und `Location` Eigenschaften jedes Objekts in der zugrunde liegenden Auflistung.

Die entsprechende Bindung im C# -Code wird im folgenden Codebeispiel dargestellt:

```csharp
var customCell = new DataTemplate (typeof(CustomCell));
customCell.SetBinding (CustomCell.NameProperty, "Name");
customCell.SetBinding (CustomCell.AgeProperty, "Age");
customCell.SetBinding (CustomCell.LocationProperty, "Location");

var listView = new ListView {
    ItemsSource = people,
    ItemTemplate = customCell
};
```

Unter iOS und Android Wenn die [ `ListView` ](xref:Xamarin.Forms.ListView) wirft Elemente und die benutzerdefinierte Zelle wird einen benutzerdefinierten Renderer verwendet, der benutzerdefinierte Renderer muss eigenschaftenänderungen ordnungsgemäß implementieren. Wenn Zellen wiederverwendet werden ihre Eigenschaftswerte ändert, wenn der Bindungskontext mit einer Zelle für zur Verfügung, mit aktualisiert wird `PropertyChanged` Ereignisse, die ausgelöst wird. Weitere Informationen finden Sie unter [Anpassen einer ViewCell](~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md). Weitere Informationen zu der Zelle wiederverwenden, finden Sie unter [Strategie für die Zwischenspeicherung](~/xamarin-forms/user-interface/listview/performance.md#cachingstrategy).

## <a name="related-links"></a>Verwandte Links

- [Erstellt in Zellen (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/BuiltInCells)
- [Benutzerdefinierte Zellen (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/CustomCells)
- [Bindung der Kontext geändert (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/BindingContextChanged)
