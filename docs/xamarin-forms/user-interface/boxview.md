---
title: Xamarin.Forms BoxView
description: In diesem Artikel wird erläutert, wie ein farbiges Rechteck für die Ergänzung, Grafiken und Interaktion in einer Xamarin.Forms-Anwendung verwendet wird.
ms.prod: xamarin
ms.assetid: 4CBF703D-84A0-4CDF-A433-5926B587782A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/07/2017
ms.openlocfilehash: edb2785362f2cc7377d9adb0c1a89a6fa2b08111
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/08/2018
ms.locfileid: "35244314"
---
# <a name="xamarinforms-boxview"></a>Xamarin.Forms BoxView

[`BoxView`](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/) Rendert ein einfaches Rechteck angegebenen Breite, Höhe und Farbe. Sie können `BoxView` für Decoration rudimentäre Grafiken und Interaktion mit dem Benutzer durch berühren.

Da Xamarin.Forms keine integrierte Vektor-Grafiksystem verfügt die `BoxView` hilft Ihnen bei der kompensieren. Einige der in diesem Artikel beschriebenen Beispielprogramme `BoxView` für das Rendern von Grafiken. Die `BoxView` Größe angepasst, um eine Zeile mit einer bestimmten Breite und die Stärke ähneln und klicken Sie dann alle Winkel mit gedreht werden kann die `Rotation` Eigenschaft.

Obwohl `BoxView` können einfache Grafiken imitieren, die Sie möglicherweise untersuchen möchten [verwenden SkiaSharp in Xamarin.Forms](~/xamarin-forms/user-interface/graphics/skiasharp/index.md) für komplexere Grafiken Anforderungen.

In diesem Artikel werden die folgenden Themen erläutert:

- **[Festlegen von BoxView Farbe und Größe](#colorandsize)**  &ndash; legen Sie die `BoxView` Eigenschaften.
- **[Rendering von Textdekorationen](#textdecorations)**  &ndash; verwenden eine `BoxView` für Rendering-Zeilen.
- **[Auflisten von Farben mit BoxView](#listingcolors)**  &ndash; zeigen alle im System in Farben einer `ListView`.
- **[Wiedergabe der Spiel Lebensdauer von Unterklassen BoxView](#subclassing)**  &ndash; einer bekannten Mobilfunk Automaton implementieren.
- **[Erstellen eine Digitaluhr](#digitalclock)**  &ndash; eine Matrix-Anzeige simulieren.
- **[Erstellen einer analogen Uhr gleichgesetzt](#analogclock)**  &ndash; transformieren und animieren `BoxView` Elemente.

<a name="colorandsize" />

## <a name="setting-boxview-color-and-size"></a>Einstellung BoxView Farbe und Größe

Sehr häufig legen Sie die folgenden drei Eigenschaften des `BoxView`:

- [`Color`](https://developer.xamarin.com/api/property/Xamarin.Forms.BoxView.Color/) die Farbe fest.
- [`WidthRequest`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.WidthRequest/) die Breite des Festlegen der `BoxView` in geräteunabhängigen Einheiten.
- [`HeightRequest`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.HeightRequest/) die Höhe der Festlegen der `BoxView`.

Die `Color` -Eigenschaft ist vom Typ `Color`; die Eigenschaft kann festgelegt werden, an ein beliebiges `Color` Wert, einschließlich der 141 statische schreibgeschützte Felder von Farben in alphabetischer Reihenfolge angefangen mit dem Namen `AliceBlue` auf `YellowGreen`.

Die `WidthRequest` und `HeightRequest` Eigenschaften spielen eine Rolle nur, wenn die `BoxView` ist *uneingeschränkte* im Layout. Dies ist der Fall, wenn der Layoutcontainer muss wissen, das untergeordnete Element festlegen, z. B. wenn die `BoxView` ist ein untergeordnetes Element einer Zelle automatisch angepasst, in der `Grid` Layout. Ein `BoxView` ist auch nicht eingeschränkte wenn seine `HorizontalOptions` und `VerticalOptions` Eigenschaften außer auf Werte festgelegt sind `LayoutOptions.Fill`. Wenn die `BoxView` ist, dass nicht eingeschränkte, aber die `WidthRequest` und `HeightRequest` sind keine Eigenschaften festgelegt, und die Breite und Höhe werden mit Standardwerten von 40 Einheiten oder ungefähr 1/4-Zoll auf mobilen Geräten festgelegt.

Die `WidthRequest` und `HeightRequest` Eigenschaften werden ignoriert, wenn die `BoxView` ist *eingeschränkte* im Layout, in dem Fall der Layoutcontainer legt eine eigene Größe für die `BoxView`.

Ein `BoxView` eingeschränkt, die in einer Dimension und in den anderen uneingeschränkten werden können. Z. B. wenn die `BoxView` ist ein untergeordnetes Element von einem vertikalen `StackLayout`, die vertikale Dimension von der `BoxView` ist uneingeschränkten und die horizontale Abmessung wird im Allgemeinen eingeschränkt. Es sind jedoch Ausnahmen für diese Dimension horizontale: Wenn die `BoxView` hat seine `HorizontalOptions` -Eigenschaft auf einen anderen Wert festgelegt wurde, außer `LayoutOptions.Fill`, und klicken Sie dann die horizontale Abmessung auch uneingeschränkte ist. Es ist auch möglich, für die `StackLayout` selbst über eine uneingeschränkte horizontale Abmessung in diesem Fall müssen die `BoxView` werden auch horizontal nicht eingeschränkte.

Die [ **BasicBoxView** ](https://developer.xamarin.com/samples/xamarin-forms/BoxView/BasicBoxView) Beispiel zeigt einen uneingeschränkten One-Zoll-Quadrat `BoxView` in der Mitte der entsprechenden Seite:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:BasicBoxView"
             x:Class="BasicBoxView.MainPage">

    <BoxView Color="CornflowerBlue"
             WidthRequest="160"
             HeightRequest="160"
             VerticalOptions="Center"
             HorizontalOptions="Center" />

</ContentPage>
```

Hier ist das Ergebnis:

[![Grundlegende BoxView](boxview-images/basicboxview-small.png "grundlegende BoxView")](boxview-images/basicboxview-large.png#lightbox "BasicBoxView")

Wenn die `VerticalOptions` und `HorizontalOptions` Eigenschaften daraus die `BoxView` kennzeichnen oder auf festgelegt `Fill`, und klicken Sie dann die `BoxView` wird durch die Größe der Seite eingeschränkt, und wird erweitert, um die Seite auszufüllen.

Ein `BoxView` kann auch ein untergeordnetes Element von einem `AbsoluteLayout`. In diesem Fall wird der Speicherort und die Größe der `BoxView` festgelegt werden, werden die `LayoutBounds` angefügte bindbare Eigenschaft. Die `AbsoluteLayout` wird erläutert, in dem Artikel [ **AbsoluteLayout**](~/xamarin-forms/user-interface/layouts/absolute-layout.md).

Sie finden Beispiele für all diesen Fällen Beispielprogramme, die folgen.

<a name="textdecorations" />

## <a name="rendering-text-decorations"></a>Rendern von Textdekorationen

Sie können die `BoxView` einige einfache Ergänzungen, die auf den Seiten in Form von horizontalen und vertikalen Linien hinzufügen. Die [ **TextDecoration** ](https://developer.xamarin.com/samples/xamarin-forms/BoxView/TextDecoration) Beispiel veranschaulicht dies. Alle Visualisierungen des Programms sind definiert, der **"MainPage.xaml"** Datei, die mehrere enthält `Label` und `BoxView` Elemente in der `StackLayout` hier gezeigten:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:TextDecoration"
             x:Class="TextDecoration.MainPage">
    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">
            <On Platform="iOS" Value="0, 20, 0, 0" />
        </OnPlatform>
    </ContentPage.Padding>

    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="BoxView">
                <Setter Property="Color" Value="Black" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>

    <ScrollView Margin="15">
        <StackLayout>

            ···

        </StackLayout>
    </ScrollView>
</ContentPage>
```

Alle das Markup, das folgt die untergeordneten Elemente sind die `StackLayout`. Dieses Markup besteht aus mehrere Typen von dekorativen `BoxView` Elementen, die verwendet wird, mit der `Label` Element:

[![Text-Decoration](boxview-images/textdecoration-small.png "Textdekoration")](boxview-images/textdecoration-large.png#lightbox "Textdekoration")

Der elegante Header am oberen Rand der Seite erfolgt mit einer `AbsoluteLayout` , deren untergeordnete Elemente sind vier `BoxView` Elemente und eine `Label`, dass alle sind bestimmten Positionen und Größen zugewiesen:

```xaml
<AbsoluteLayout>
    <BoxView AbsoluteLayout.LayoutBounds="0, 10, 200, 5" />
    <BoxView AbsoluteLayout.LayoutBounds="0, 20, 200, 5" />
    <BoxView AbsoluteLayout.LayoutBounds="10, 0, 5, 65" />
    <BoxView AbsoluteLayout.LayoutBounds="20, 0, 5, 65" />
    <Label Text="Stylish Header"
           FontSize="24"
           AbsoluteLayout.LayoutBounds="30, 25, AutoSize, AutoSize"/>
</AbsoluteLayout>
```

In der XAML-Datei die `AbsoluteLayout` gefolgt von einer `Label` mit formatierten Text zur Beschreibung der `AbsoluteLayout`.

Sie können eine Textzeichenfolge unterstreichen, schließen Sie beide die `Label` und `BoxView` in eine `StackLayout` mit dem seine `HorizontalOptions` Wert festgelegt auf einen anderen Wert als `Fill`. Die Breite des der `StackLayout` unterliegt klicken Sie dann die Breite des der `Label`, die wiederum erzwingt, Breite, auf die `BoxView`. Die `BoxView` wird nur eine explizite Höhe zugewiesen:

```xaml
<StackLayout HorizontalOptions="Center">
    <Label Text="Underlined Text"
           FontSize="24" />
    <BoxView HeightRequest="2" />
</StackLayout>
```

Diese Technik können einzelne Wörter in längere Textzeichenfolgen oder eines Absatzes.

Es ist auch möglich, verwenden Sie eine `BoxView` HTML gleichkommt `hr` (horizontale Regelelementen). Lassen Sie einfach die Breite des der `BoxView` bestimmt werden, durch den übergeordneten Container, die in diesem Fall ist die `StackLayout`:

```xaml
<BoxView HeightRequest="3" />
```

Sie können schließlich eine vertikale Linie auf einer Seite des einen Textabsatz zeichnen, schließen Sie beide die `BoxView` und `Label` in einem horizontalen `StackLayout`. In diesem Fall wird die Höhe des der `BoxView` ist identisch mit die Höhe des `StackLayout`, unterliegt der die Höhe des der `Label`:

```xaml
<StackLayout Orientation="Horizontal">
    <BoxView WidthRequest="4"
             Margin="0, 0, 10, 0" />
    <Label>

        ···

    </Label>
/StackLayout>
```
<a name="listingcolors" />

## <a name="listing-colors-with-boxview"></a>Auflisten von Farben mit BoxView

Die `BoxView` eignet sich zum Anzeigen von Farben. Das Programm erstellt mithilfe einer `ListView` alle öffentlichen statischen nur-Lese Felder von der Xamarin.Forms auflisten `Color` Struktur:

[![ListView-Farben](boxview-images/listviewcolors-small.png "ListView Farben")](boxview-images/listviewcolors-large.png#lightbox "ListView-Farben")

Die [ **ListViewColors** ](https://developer.xamarin.com/samples/xamarin-forms/BoxView/ListViewColors/) Programm enthält eine Klasse namens `NamedColor`. Der statische Konstruktor verwendet Reflektion, um den Zugriff auf alle Felder des der `Color` -Struktur, und erstellen Sie eine `NamedColor` für jedes einzelne Objekt. Diese werden in der statischen gespeichert `All` Eigenschaft:

```csharp
public class NamedColor
{
    // Instance members.
    private NamedColor()
    {
    }

    public string Name { private set; get; }

    public string FriendlyName { private set; get; }

    public Color Color { private set; get; }

    public string RgbDisplay { private set; get; }

    // Static members.
    static NamedColor()
    {
        List<NamedColor> all = new List<NamedColor>();
        StringBuilder stringBuilder = new StringBuilder();

        // Loop through the public static fields of the Color structure.
        foreach (FieldInfo fieldInfo in typeof(Color).GetRuntimeFields ())
        {
            if (fieldInfo.IsPublic &&
                fieldInfo.IsStatic &&
                fieldInfo.FieldType == typeof (Color))
            {
                // Convert the name to a friendly name.
                string name = fieldInfo.Name;
                stringBuilder.Clear();
                int index = 0;

                foreach (char ch in name)
                {
                    if (index != 0 && Char.IsUpper(ch))
                    {
                        stringBuilder.Append(' ');
                    }
                    stringBuilder.Append(ch);
                    index++;
                }

                // Instantiate a NamedColor object.
                Color color = (Color)fieldInfo.GetValue(null);

                NamedColor namedColor = new NamedColor
                {
                    Name = name,
                    FriendlyName = stringBuilder.ToString(),
                    Color = color,
                    RgbDisplay = String.Format("{0:X2}-{1:X2}-{2:X2}",
                                               (int)(255 * color.R),
                                               (int)(255 * color.G),
                                               (int)(255 * color.B))
                };

                // Add it to the collection.
                all.Add(namedColor);
            }
        }
        all.TrimExcess();
        All = all;
    }

    public static IList<NamedColor> All { private set; get; }
}
```

Das Programm visuelle Elemente werden in der XAML-Datei beschrieben. Die `ItemsSource` Eigenschaft von der `ListView` festgelegt ist, um die statische `NamedColor.All` -Eigenschaft, d., die h. die `ListView` zeigt alle einzelnen `NamedColor` Objekte:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:ListViewColors"
             x:Class="ListViewColors.MainPage">
    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">
            <On Platform="iOS" Value="10, 20, 10, 0" />
            <On Platform="Android, UWP" Value="10, 0" />
        </OnPlatform>
    </ContentPage.Padding>

    <ListView SeparatorVisibility="None"
              ItemsSource="{x:Static local:NamedColor.All}">
        <ListView.RowHeight>
            <OnPlatform x:TypeArguments="x:Int32">
                <On Platform="iOS, Android" Value="80" />
                <On Platform="UWP" Value="90" />
            </OnPlatform>
        </ListView.RowHeight>

        <ListView.ItemTemplate>
            <DataTemplate>
                <ViewCell>
                    <ContentView Padding="5">
                        <Frame OutlineColor="Accent"
                               Padding="10">
                            <StackLayout Orientation="Horizontal">
                                <BoxView Color="{Binding Color}"
                                         WidthRequest="50"
                                         HeightRequest="50" />
                                <StackLayout>
                                    <Label Text="{Binding FriendlyName}"
                                           FontSize="22"
                                           VerticalOptions="StartAndExpand" />
                                    <Label Text="{Binding RgbDisplay, StringFormat='RGB = {0}'}"
                                           FontSize="16"
                                           VerticalOptions="CenterAndExpand" />
                                </StackLayout>
                            </StackLayout>
                        </Frame>
                    </ContentView>
                </ViewCell>
            </DataTemplate>
        </ListView.ItemTemplate>
    </ListView>
</ContentPage>
```

Die `NamedColor` Objekte werden formatiert, indem die `ViewCell` -Objekt, das als die Datenvorlage der festzulegen ist die `ListView`. Diese Vorlage enthält einen `BoxView` , deren `Color` Eigenschaft gebunden ist die `Color` Eigenschaft von der `NamedColor` Objekt.

<a name="subclassing" />

## <a name="playing-the-game-of-life-by-subclassing-boxview"></a>Lebensdauer von Unterklassen BoxView Spiels des

Das Spiel of Life ist ein Mobiltelefon Automaton von Mathematiker John Conways erfunden und wurde auf den Seiten des *wissenschaftlichen American* in der 1970s. Eine gute Einführung wird bereitgestellt, indem Sie im Wikipedia-Artikel [Conways Spiel of Life](https://en.wikipedia.org/wiki/Conway%27s_Game_of_Life).

Der Xamarin.Forms [ **GameOfLife** ](https://developer.xamarin.com/samples/xamarin-forms/BoxView/GameOfLife/) Programm definiert eine Klasse namens `LifeCell` abgeleitet, die `BoxView`. Diese Klasse kapselt die Logik der eine einzelne Zelle im Spiel Leben:

```csharp
class LifeCell : BoxView
{
    bool isAlive;

    public event EventHandler Tapped;

    public LifeCell()
    {
        BackgroundColor = Color.White;

        TapGestureRecognizer tapGesture = new TapGestureRecognizer();
        tapGesture.Tapped += (sender, args) =>
        {
            Tapped?.Invoke(this, EventArgs.Empty);
        };
        GestureRecognizers.Add(tapGesture);
    }

    public int Col { set; get; }

    public int Row { set; get; }

    public bool IsAlive
    {
        set
        {
            if (isAlive != value)
            {
                isAlive = value;
                BackgroundColor = isAlive ? Color.Black : Color.White;
            }
        }
        get
        {
            return isAlive;
        }
    }
}
```

`LifeCell` fügt drei weitere Eigenschaften `BoxView`: die `Col` und `Row` speichern die Position der Zelle innerhalb des Rasters Eigenschaften und die `IsAlive` Eigenschaft gibt an, den Zustand. Die `IsAlive` Eigenschaft setzt auch die `Color` Eigenschaft von der `BoxView` auf Schwarz festgelegt, wenn die Zelle ist aktiv und weiß, wenn die Zelle nicht aktiv ist.

`LifeCell` installiert außerdem eine `TapGestureRecognizer` damit der Benutzer den Status von Zellen aktivieren bzw. deaktivieren, tippen Sie auf diese kann. Die Klasse übersetzt die `Tapped` Ereignis aus der Erkennung Gestenhandler in seiner eigenen `Tapped` Ereignis.

Die **GameOfLife** Programm enthält auch eine `LifeGrid` Klasse, die ein Großteil der Logik des Spiels, kapselt und eine `MainPage` -Klasse, die das Programm visuelle Elemente verarbeitet. Dazu gehören Overlay, die die Regeln des Spiels beschreibt. Hier ist die Anwendung in Aktion zeigt einige hundert `LifeCell` Objekte auf der Seite:

[![Spiel abgelaufenen](boxview-images/gameoflife-small.png "Spiel abgelaufenen")](boxview-images/gameoflife-large.png#lightbox "Spiel abgelaufenen")

<a name="digitalclock" />

## <a name="creating-a-digital-clock"></a>Erstellen eine digitale Uhr

Die [ **DotMatrixClock** ](https://developer.xamarin.com/samples/xamarin-forms/BoxView/DotMatrixClock/) Programm erstellt 210 `BoxView` Elemente, die Punkte einer altmodisch Matrix-5-7-Anzeige zu simulieren. Informieren Sie sich die Zeit in hoch-oder Querformat, es ist jedoch größere im Querformat:

[![Matrix-Uhr](boxview-images/dotmatrixclock-small.png "Matrix-Uhr")](boxview-images/dotmatrixclock-large.png#lightbox "Matrix-Uhr")

Die XAML-Datei etwas mehr als Instanziieren der `AbsoluteLayout` für die Uhr verwendet:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DotMatrixClock"
             x:Class="DotMatrixClock.MainPage"
             Padding="10"
             SizeChanged="OnPageSizeChanged">

    <AbsoluteLayout x:Name="absoluteLayout"
                    VerticalOptions="Center" />
</ContentPage>
```

Ansonsten tritt ein, in der Code-Behind-Datei. Die Matrix-Anzeige Logik wird durch die Definition von mehreren Arrays, die beschreiben, die Punkte, die jeweils 10 Ziffern und einem Doppelpunkt, erheblich vereinfacht:

```csharp
public partial class MainPage : ContentPage
{
    // Total dots horizontally and vertically.
    const int horzDots = 41;
    const int vertDots = 7;

    // 5 x 7 dot matrix patterns for 0 through 9.
    static readonly int[, ,] numberPatterns = new int[10, 7, 5]
    {
        {
            { 0, 1, 1, 1, 0}, { 1, 0, 0, 0, 1}, { 1, 0, 0, 1, 1}, { 1, 0, 1, 0, 1},
            { 1, 1, 0, 0, 1}, { 1, 0, 0, 0, 1}, { 0, 1, 1, 1, 0}
        },
        {
            { 0, 0, 1, 0, 0}, { 0, 1, 1, 0, 0}, { 0, 0, 1, 0, 0}, { 0, 0, 1, 0, 0},
            { 0, 0, 1, 0, 0}, { 0, 0, 1, 0, 0}, { 0, 1, 1, 1, 0}
        },
        {
            { 0, 1, 1, 1, 0}, { 1, 0, 0, 0, 1}, { 0, 0, 0, 0, 1}, { 0, 0, 0, 1, 0},
            { 0, 0, 1, 0, 0}, { 0, 1, 0, 0, 0}, { 1, 1, 1, 1, 1}
        },
        {
            { 1, 1, 1, 1, 1}, { 0, 0, 0, 1, 0}, { 0, 0, 1, 0, 0}, { 0, 0, 0, 1, 0},
            { 0, 0, 0, 0, 1}, { 1, 0, 0, 0, 1}, { 0, 1, 1, 1, 0}
        },
        {
            { 0, 0, 0, 1, 0}, { 0, 0, 1, 1, 0}, { 0, 1, 0, 1, 0}, { 1, 0, 0, 1, 0},
            { 1, 1, 1, 1, 1}, { 0, 0, 0, 1, 0}, { 0, 0, 0, 1, 0}
        },
        {
            { 1, 1, 1, 1, 1}, { 1, 0, 0, 0, 0}, { 1, 1, 1, 1, 0}, { 0, 0, 0, 0, 1},
            { 0, 0, 0, 0, 1}, { 1, 0, 0, 0, 1}, { 0, 1, 1, 1, 0}
        },
        {
            { 0, 0, 1, 1, 0}, { 0, 1, 0, 0, 0}, { 1, 0, 0, 0, 0}, { 1, 1, 1, 1, 0},
            { 1, 0, 0, 0, 1}, { 1, 0, 0, 0, 1}, { 0, 1, 1, 1, 0}
        },
        {
            { 1, 1, 1, 1, 1}, { 0, 0, 0, 0, 1}, { 0, 0, 0, 1, 0}, { 0, 0, 1, 0, 0},
            { 0, 1, 0, 0, 0}, { 0, 1, 0, 0, 0}, { 0, 1, 0, 0, 0}
        },
        {
            { 0, 1, 1, 1, 0}, { 1, 0, 0, 0, 1}, { 1, 0, 0, 0, 1}, { 0, 1, 1, 1, 0},
            { 1, 0, 0, 0, 1}, { 1, 0, 0, 0, 1}, { 0, 1, 1, 1, 0}
        },
        {
            { 0, 1, 1, 1, 0}, { 1, 0, 0, 0, 1}, { 1, 0, 0, 0, 1}, { 0, 1, 1, 1, 1},
            { 0, 0, 0, 0, 1}, { 0, 0, 0, 1, 0}, { 0, 1, 1, 0, 0}
        },
    };

    // Dot matrix pattern for a colon.
    static readonly int[,] colonPattern = new int[7, 2]
    {
        { 0, 0 }, { 1, 1 }, { 1, 1 }, { 0, 0 }, { 1, 1 }, { 1, 1 }, { 0, 0 }
    };

    // BoxView colors for on and off.
    static readonly Color colorOn = Color.Red;
    static readonly Color colorOff = new Color(0.5, 0.5, 0.5, 0.25);

    // Box views for 6 digits, 7 rows, 5 columns.
    BoxView[, ,] digitBoxViews = new BoxView[6, 7, 5];

    ···

}
```

Schließen Sie diese Felder mit ein dreidimensionales Array vom `BoxView` Elemente für die Punkt-Muster für die sechs Ziffern speichern.

Der Konstruktor erstellt alle der `BoxView` Elemente für die Ziffern und Doppelpunkt, und außerdem initialisiert der `Color` Eigenschaft von der `BoxView` Elemente für den Doppelpunkt:

```csharp
public partial class MainPage : ContentPage
{

    ···

    public MainPage()
    {
        InitializeComponent();

        // BoxView dot dimensions.
        double height = 0.85 / vertDots;
        double width = 0.85 / horzDots;

        // Create and assemble the BoxViews.
        double xIncrement = 1.0 / (horzDots - 1);
        double yIncrement = 1.0 / (vertDots - 1);
        double x = 0;

        for (int digit = 0; digit < 6; digit++)
        {
            for (int col = 0; col < 5; col++)
            {
                double y = 0;

                for (int row = 0; row < 7; row++)
                {
                    // Create the digit BoxView and add to layout.
                    BoxView boxView = new BoxView();
                    digitBoxViews[digit, row, col] = boxView;
                    absoluteLayout.Children.Add(boxView,
                                                new Rectangle(x, y, width, height),
                                                AbsoluteLayoutFlags.All);
                    y += yIncrement;
                }
                x += xIncrement;
            }
            x += xIncrement;

            // Colons between the hours, minutes, and seconds.
            if (digit == 1 || digit == 3)
            {
                int colon = digit / 2;

                for (int col = 0; col < 2; col++)
                {
                    double y = 0;

                    for (int row = 0; row < 7; row++)
                    {
                        // Create the BoxView and set the color.
                        BoxView boxView = new BoxView
                            {
                                Color = colonPattern[row, col] == 1 ?
                                            colorOn : colorOff
                            };
                        absoluteLayout.Children.Add(boxView,
                                                    new Rectangle(x, y, width, height),
                                                    AbsoluteLayoutFlags.All);
                        y += yIncrement;
                    }
                    x += xIncrement;
                }
                x += xIncrement;
            }
        }

        // Set the timer and initialize with a manual call.
        Device.StartTimer(TimeSpan.FromSeconds(1), OnTimer);
        OnTimer();
    }

    ···

}
```

Dieses Programm verwendet werden, die relative Positionierung und Sizing-Funktion von `AbsoluteLayout`. Die Breite und Höhe der einzelnen `BoxView` Bruchzahlen, insbesondere 85 % 1, dividiert durch die Anzahl der horizontalen und vertikalen Punkte festgelegt werden. Die Positionen werden auch auf Bruchzahlen festgelegt.

Da sind die Positionen und Größen relativ zu der Gesamtgröße der der `AbsoluteLayout`, die `SizeChanged` Handler für die Seite muss nur festgelegt werden. eine `HeightRequest` von der `AbsoluteLayout`:

```csharp
public partial class MainPage : ContentPage
{

    ···

    void OnPageSizeChanged(object sender, EventArgs args)
    {
        // No chance a display will have an aspect ratio > 41:7
        absoluteLayout.HeightRequest = vertDots * Width / horzDots;
    }

    ···

}
```

Die Breite der `AbsoluteLayout` wird automatisch festgelegt werden, da es gestreckt wird, um die gesamte Breite der Seite.

Der endgültige Code in die `MainPage` -Klasse verarbeitet den Zeitgeberrückruf und Farben, die Punkte der einzelnen Ziffer. Die Definition der mehrdimensionalen Arrays am Anfang des Code-Behind-Datei hilft dabei, diese Logik den einfachsten Teil des Programms abzuwickeln:

```csharp
public partial class MainPage : ContentPage
{

    ···

    bool OnTimer()
    {
        DateTime dateTime = DateTime.Now;

        // Convert 24-hour clock to 12-hour clock.
        int hour = (dateTime.Hour + 11) % 12 + 1;

        // Set the dot colors for each digit separately.
        SetDotMatrix(0, hour / 10);
        SetDotMatrix(1, hour % 10);
        SetDotMatrix(2, dateTime.Minute / 10);
        SetDotMatrix(3, dateTime.Minute % 10);
        SetDotMatrix(4, dateTime.Second / 10);
        SetDotMatrix(5, dateTime.Second % 10);
        return true;
    }

    void SetDotMatrix(int index, int digit)
    {
        for (int row = 0; row < 7; row++)
            for (int col = 0; col < 5; col++)
            {
                bool isOn = numberPatterns[digit, row, col] == 1;
                Color color = isOn ? colorOn : colorOff;
                digitBoxViews[index, row, col].Color = color;
            }
    }
}
```
<a name="analogclock" />

## <a name="creating-an-analog-clock"></a>Erstellen einer analogen Uhr

Eine Matrix-Uhr scheinen einer Anwendung offensichtlich sein `BoxView`, aber `BoxView` Elemente sind auch in der Lage, eine analoge Uhr bemerken:

[![BoxView Uhr](boxview-images/boxviewclock-small.png "BoxView Uhr")](boxview-images/boxviewclock-large.png#lightbox "BoxView Uhr")

Alle visuellen Elemente in der [ **BoxViewClock** ](https://developer.xamarin.com/samples/xamarin-forms/BoxView/BoxViewClock/) Programm sind untergeordnete Elemente des ein `AbsoluteLayout`. Diese Elemente haben eine Größe von mithilfe der `LayoutBounds` -Eigenschaft, und gedreht, mit der `Rotation` Eigenschaft.

Die drei `BoxView` Elemente für die Zeiger der Uhr instanziiert die XAML-Datei, jedoch keine positioniert oder angepasst werden:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:BoxViewClock"
             x:Class="BoxViewClock.MainPage">
    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">
            <On Platform="iOS" Value="0, 20, 0, 0" />
        </OnPlatform>
    </ContentPage.Padding>

    <AbsoluteLayout x:Name="absoluteLayout"
                    SizeChanged="OnAbsoluteLayoutSizeChanged">

        <BoxView x:Name="hourHand"
                 Color="Black" />

        <BoxView x:Name="minuteHand"
                 Color="Black" />

        <BoxView x:Name="secondHand"
                 Color="Black" />
    </AbsoluteLayout>
</ContentPage>
```

Der Konstruktor der CodeBehind-Datei instanziiert den 60 `BoxView` Elemente für die Teilstriche, um den Umfang der Uhr:

```csharp
public partial class MainPage : ContentPage
{

    ···

    BoxView[] tickMarks = new BoxView[60];

    public MainPage()
    {
        InitializeComponent();

        // Create the tick marks (to be sized and positioned later).
        for (int i = 0; i < tickMarks.Length; i++)
        {
            tickMarks[i] = new BoxView { Color = Color.Black };
            absoluteLayout.Children.Add(tickMarks[i]);
        }

        Device.StartTimer(TimeSpan.FromSeconds(1.0 / 60), OnTimerTick);
    }

    ···

}
```

Die Größe und Positionierung der alle der `BoxView` Elemente tritt auf, der `SizeChanged` Handler für die `AbsoluteLayout`. Ein wenig Struktur für die Klasse intern aufgerufen `HandParams` beschreibt die Größe jedes der drei Hände relativ zu der Gesamtgröße der Uhr:

```csharp
public partial class MainPage : ContentPage
{
    // Structure for storing information about the three hands.
    struct HandParams
    {
        public HandParams(double width, double height, double offset) : this()
        {
            Width = width;
            Height = height;
            Offset = offset;
        }

        public double Width { private set; get; }   // fraction of radius
        public double Height { private set; get; }  // ditto
        public double Offset { private set; get; }  // relative to center pivot
    }

    static readonly HandParams secondParams = new HandParams(0.02, 1.1, 0.85);
    static readonly HandParams minuteParams = new HandParams(0.05, 0.8, 0.9);
    static readonly HandParams hourParams = new HandParams(0.125, 0.65, 0.9);

    ···

 }
```

Die `SizeChanged` Ereignishandler bestimmt den Mittelpunkt und den Radius des der `AbsoluteLayout`, und klicken Sie dann die Größe ein und platziert den 60 `BoxView` Elementen, die als Teilstriche verwendet. Die `for` Schleife endet, durch Festlegen der `Rotation` Eigenschaft der einzelnen `BoxView` Elemente. Am Ende der `SizeChanged` Handler auf, die `LayoutHand` Methode wird aufgerufen, um die Größe und position der drei Zeiger der Uhr:

```csharp
public partial class MainPage : ContentPage
{

    ···

    void OnAbsoluteLayoutSizeChanged(object sender, EventArgs args)
    {
        // Get the center and radius of the AbsoluteLayout.
        Point center = new Point(absoluteLayout.Width / 2, absoluteLayout.Height / 2);
        double radius = 0.45 * Math.Min(absoluteLayout.Width, absoluteLayout.Height);

        // Position, size, and rotate the 60 tick marks.
        for (int index = 0; index < tickMarks.Length; index++)
        {
            double size = radius / (index % 5 == 0 ? 15 : 30);
            double radians = index * 2 * Math.PI / tickMarks.Length;
            double x = center.X + radius * Math.Sin(radians) - size / 2;
            double y = center.Y - radius * Math.Cos(radians) - size / 2;
            AbsoluteLayout.SetLayoutBounds(tickMarks[index], new Rectangle(x, y, size, size));
            tickMarks[index].Rotation = 180 * radians / Math.PI;
        }

        // Position and size the three hands.
        LayoutHand(secondHand, secondParams, center, radius);
        LayoutHand(minuteHand, minuteParams, center, radius);
        LayoutHand(hourHand, hourParams, center, radius);
    }

    void LayoutHand(BoxView boxView, HandParams handParams, Point center, double radius)
    {
        double width = handParams.Width * radius;
        double height = handParams.Height * radius;
        double offset = handParams.Offset;

        AbsoluteLayout.SetLayoutBounds(boxView,
            new Rectangle(center.X - 0.5 * width,
                          center.Y - offset * height,
                          width, height));

        // Set the AnchorY property for rotations.
        boxView.AnchorY = handParams.Offset;
    }

    ···

}
```

Die `LayoutHand` Methode Größen und Positionen jeder Runde, zeigen Sie gerade bis zu der 12:00 Uhr-Position. Am Ende der Methode die `AnchorY` Eigenschaftensatz an eine Position in der Mitte der Uhr entspricht. Hiermit wird der Mittelpunkt der Drehung.

Die Hände geraten sind in der Rückruffunktion Timer gedreht:

```csharp
public partial class MainPage : ContentPage
{

    ···

    bool OnTimerTick()
    {
        // Set rotation angles for hour and minute hands.
        DateTime dateTime = DateTime.Now;
        hourHand.Rotation = 30 * (dateTime.Hour % 12) + 0.5 * dateTime.Minute;
        minuteHand.Rotation = 6 * dateTime.Minute + 0.1 * dateTime.Second;

        // Do an animation for the second hand.
        double t = dateTime.Millisecond / 1000.0;

        if (t < 0.5)
        {
            t = 0.5 * Easing.SpringIn.Ease(t / 0.5);
        }
        else
        {
            t = 0.5 * (1 + Easing.SpringOut.Ease((t - 0.5) / 0.5));
        }

        secondHand.Rotation = 6 * (dateTime.Second + t);
        return true;
    }
}
```

Der zweite Zeiger ist ein wenig anders behandelt: eine Animation Beschleunigungsfunktionen Funktion wird angewendet, um die Bewegung scheint mechanische anstelle von smooth vornehmen. Der zweite Zeiger zieht etwas jedem Takt und overshoots klicken Sie dann das Ziel. Diese wenig Code hinzugefügt viel realistische der Bewegung.

## <a name="conclusion"></a>Schlussbemerkung

Die `BoxView` scheinen zunächst, sondern als Sie auf einfache kennen gelernt haben, kann es sehr vielseitig sein und können nahezu reproduzieren visuelle Elemente, die normalerweise nur mit möglich sind Vektorgrafiken. Für komplexere Grafiken, wenden Sie sich an [verwenden SkiaSharp in Xamarin.Forms](~/xamarin-forms/user-interface/graphics/skiasharp/index.md).


## <a name="related-links"></a>Verwandte Links

- [Grundlegende BoxView (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/BoxView/BasicBoxView)
- [Text-Decoration (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/BoxView/TextDecoration)
- [Farbe ListBox (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/BoxView/ColorListBox)
- [Spiel abgelaufenen (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/BoxView/GameOfLife)
- [Matrix-Uhr (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/BoxView/DotMatrixClock)
- [BoxView Uhr (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/BoxView/BoxViewClock)
- [BoxView](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/)
