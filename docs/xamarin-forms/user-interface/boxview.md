---
title: Xamarin.Forms BoxView
description: In diesem Artikel wird erläutert, wie Sie ein farbiges Rechteck für Dekoration, Grafiken und Aktivität in einer Xamarin.Forms-Anwendung verwenden.
ms.prod: xamarin
ms.assetid: 4CBF703D-84A0-4CDF-A433-5926B587782A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/07/2017
ms.openlocfilehash: 813a913c2c2fb27456c9a489c73b16d5892c4b8d
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997051"
---
# <a name="xamarinforms-boxview"></a>Xamarin.Forms BoxView

[`BoxView`](xref:Xamarin.Forms.BoxView) Rendert ein einfaches Rechteck von einer angegebenen Breite, Höhe und Farbe. Sie können `BoxView` Dekoration, rudimentäre Grafiken, und für die Interaktion mit dem Benutzer über Touch.

Da Xamarin.Forms keine integrierte Vektor-Grafiksystem, verfügt die `BoxView` können, um dies zu kompensieren. Einige der in diesem Artikel beschriebenen Beispielprogramme `BoxView` für das Rendern von Grafiken. Die `BoxView` können die Größe eine Zeile eine bestimmte Breite und die Stärke ähneln, und klicken Sie dann mithilfe von jedem Winkel gedreht der `Rotation` Eigenschaft.

Obwohl `BoxView` können einfache Grafiken zu imitieren, die Sie untersuchen möchten [mithilfe von SkiaSharp in Xamarin.Forms](~/xamarin-forms/user-interface/graphics/skiasharp/index.md) für komplexere Grafiken-Anforderungen.

In diesem Artikel werden die folgenden Themen behandelt:

- **[Festlegen von BoxView Farbe und Größe](#colorandsize)**  &ndash; legen Sie die `BoxView` Eigenschaften.
- **[Rendering von Textdekorationen](#textdecorations)**  &ndash; verwenden eine `BoxView` für Rendering-Zeilen.
- **[Auflisten von Farben mit BoxView](#listingcolors)**  &ndash; zeigen alle die von in Systemfarben einem `ListView`.
- **[Spielen das Spiel-of-Life von Unterklassen BoxView](#subclassing)**  &ndash; eine berühmten Mobilfunk Automaton implementieren.
- **[Erstellen eine Digitaluhr](#digitalclock)**  &ndash; simulieren Sie eine Punkt-Matrix angezeigt.
- **[Erstellen einer analogen Uhr](#analogclock)**  &ndash; transformieren und animieren `BoxView` Elemente.

<a name="colorandsize" />

## <a name="setting-boxview-color-and-size"></a>Einstellung BoxView Farbe und Größe

Sehr häufig legen Sie die folgenden drei Eigenschaften des `BoxView`:

- [`Color`](xref:Xamarin.Forms.BoxView.Color) die Farbe fest.
- [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) die Breite der Festlegen der `BoxView` in geräteunabhängigen Einheiten.
- [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) die Höhe der Festlegen der `BoxView`.

Die `Color` Eigenschaft ist vom Typ `Color`; die Eigenschaft kann festgelegt werden, um eine `Color` Wert, einschließlich der 141 statische schreibgeschützte Felder mit der Bezeichnung alphabetisch zwischen Farben `AliceBlue` zu `YellowGreen`.

Die `WidthRequest` und `HeightRequest` Eigenschaften nur eine Rolle spielen, wenn die `BoxView` ist *uneingeschränkte* im Layout. Dies ist der Fall, wenn der Layoutcontainer muss wissen, das untergeordnete Element die Größe zu ändern, z. B. wenn die `BoxView` ist ein untergeordnetes Element einer automatischer Größenänderung Zelle in der `Grid` Layout. Ein `BoxView` ist auch uneingeschränkte wenn seine `HorizontalOptions` und `VerticalOptions` Eigenschaften nicht auf Werte festgelegt sind `LayoutOptions.Fill`. Wenn die `BoxView` uneingeschränkt ist, ist aber die `WidthRequest` und `HeightRequest` sind keine Eigenschaften festgelegt, und klicken Sie dann die Breite und Höhe werden mit Standardwerten von 40 Einheiten oder etwa 1/4 Zoll auf mobilen Geräten festgelegt.

Die `WidthRequest` und `HeightRequest` Eigenschaften werden ignoriert, wenn die `BoxView` ist *eingeschränkte* im Layout, in dem Fall der Layoutcontainer seine eigene Größe für legt die `BoxView`.

Ein `BoxView` in einer Dimension beschränkt und im anderen uneingeschränkt werden kann. Z. B. wenn die `BoxView` ist ein untergeordnetes Element eines vertikalen `StackLayout`, die vertikale Dimension der der `BoxView` ist uneingeschränkt und dessen horizontale Dimension wird im Allgemeinen eingeschränkt. Aber es gibt jedoch Ausnahmen für diese horizontale Dimension: Wenn die `BoxView` hat seine `HorizontalOptions` -Eigenschaft auf etwas anders als `LayoutOptions.Fill`, und klicken Sie dann die horizontale Abmessung auch uneingeschränkte ist. Es ist auch möglich, für die `StackLayout` selbst, um eine uneingeschränkte horizontale Dimension, in diesem Fall müssen die `BoxView` werden auch horizontal uneingeschränkt.

Die [ **BasicBoxView** ](https://developer.xamarin.com/samples/xamarin-forms/BoxView/BasicBoxView) Beispiel zeigt eine uneingeschränkte eine-Zoll-Square `BoxView` in der Mitte der Seite:

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

Wenn die `VerticalOptions` und `HorizontalOptions` Eigenschaften entfernt wurden die `BoxView` kennzeichnen oder auf festgelegt `Fill`, und klicken Sie dann die `BoxView` wird von der Größe der Seite beschränkt und wird erweitert, um das Ausfüllen der Seite.

Ein `BoxView` kann auch sein, ein untergeordnetes Element des ein `AbsoluteLayout`. In diesem Fall wird der Speicherort und die Größe der `BoxView` festgelegt werden, werden die `LayoutBounds` bindbare Eigenschaft angefügt. Die `AbsoluteLayout` ist in diesem Artikel erläuterten [ **von "AbsoluteLayout"**](~/xamarin-forms/user-interface/layouts/absolute-layout.md).

Beispiele für die all diesen Fällen in den Beispielprogrammen angezeigt, die folgen.

<a name="textdecorations" />

## <a name="rendering-text-decorations"></a>Rendern von Textdekorationen

Sie können die `BoxView` einige einfache Ergänzungen, die auf Ihren Seiten in Form von horizontale und vertikale Linien hinzufügen. Die [ **TextDecoration** ](https://developer.xamarin.com/samples/xamarin-forms/BoxView/TextDecoration) veranschaulicht dies. Alle des Programms Visuals werden definiert, der **"MainPage.xaml"** Datei, die mehrere enthält `Label` und `BoxView` Elemente in der `StackLayout` hier gezeigt:

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

All das Markup, das folgt sind untergeordnete Elemente von der `StackLayout`. Dieses Markup besteht aus mehrere Typen von dekorativen `BoxView` Elemente, die verwendet werden, mit der `Label` Element:

[![Text-Decoration](boxview-images/textdecoration-small.png "Textdekoration")](boxview-images/textdecoration-large.png#lightbox "Textdekoration")

Der elegante Header am oberen Rand der Seite erfolgt über eine `AbsoluteLayout` , deren untergeordnete Elemente sind vier `BoxView` Elemente und ein `Label`, werden alle von sind bestimmte Speicherorte und Größen zugewiesen:

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

In der XAML-Datei der `AbsoluteLayout` folgt eine `Label` mit formatierten Text zur Beschreibung der `AbsoluteLayout`.

Sie können eine Textzeichenfolge unterstreichen, schließen Sie beide die `Label` und `BoxView` in eine `StackLayout` , bei dem die `HorizontalOptions` festgelegt auf einen Wert als `Fill`. Die Breite des der `StackLayout` unterliegt klicken Sie dann die Breite des der `Label`, das dann erzwingt die Breite auf den `BoxView`. Die `BoxView` erhält nur eine explizite Höhe:

```xaml
<StackLayout HorizontalOptions="Center">
    <Label Text="Underlined Text"
           FontSize="24" />
    <BoxView HeightRequest="2" />
</StackLayout>
```

Diese Methode können keine um einzelnen Wörter in längere Textzeichenfolgen oder eines Absatzes zu unterstreichen.

Es ist auch möglich, verwenden Sie eine `BoxView` HTML wie folgt `hr` (horizontale Trennlinie)-Element. Lassen Sie einfach die Breite des der `BoxView` ermittelt werden, durch den übergeordneten Container, die in diesem Fall ist die `StackLayout`:

```xaml
<BoxView HeightRequest="3" />
```

Sie können schließlich eine vertikale Linie auf einer Seite des einen Textabsatz zeichnen, schließen Sie beide die `BoxView` und `Label` in einer horizontalen `StackLayout`. In diesem Fall die Höhe des der `BoxView` ist identisch mit die Höhe des `StackLayout`, unterliegt der die Höhe der `Label`:

```xaml
<StackLayout Orientation="Horizontal">
    <BoxView WidthRequest="4"
             Margin="0, 0, 10, 0" />
    <Label>

        ···

    </Label>
</StackLayout>
```
<a name="listingcolors" />

## <a name="listing-colors-with-boxview"></a>Auflisten von Farben mit BoxView

Die `BoxView` eignet sich zum Anzeigen von Farben. Dieses Programm verwendet eine `ListView` Listen Sie alle die öffentliche statische schreibgeschützte Felder von der Xamarin.Forms `Color` Struktur:

[![ListView-Farben](boxview-images/listviewcolors-small.png "ListView Farben")](boxview-images/listviewcolors-large.png#lightbox "ListView-Farben")

Die [ **ListViewColors** ](https://developer.xamarin.com/samples/xamarin-forms/BoxView/ListViewColors/) Programm enthält eine Klasse namens `NamedColor`. Der statische Konstruktor verwendet Reflektion, um alle Felder der Zugriff auf die `Color` Struktur und erstellen Sie eine `NamedColor` Objekt für jeden dieser. Diese befinden sich in der statischen `All` Eigenschaft:

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

Die Programm-Visualisierungen werden in der XAML-Datei beschrieben. Die `ItemsSource` Eigenschaft der `ListView` festgelegt ist, an die statische `NamedColor.All` -Eigenschaft, die das bedeutet, dass die `ListView` zeigt alle einzelnen `NamedColor` Objekte:

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

Die `NamedColor` Objekte werden formatiert, indem die `ViewCell` -Objekt, das die Datenvorlage der festgelegt ist die `ListView`. Diese Vorlage umfasst eine `BoxView` , deren `Color` Eigenschaft gebunden ist die `Color` Eigenschaft der `NamedColor` Objekt.

<a name="subclassing" />

## <a name="playing-the-game-of-life-by-subclassing-boxview"></a>Lebensdauer von Unterklassen BoxView Spiels des

Das Spiel-of-Life ist eine Mobilfunk Automaton von Mathematiker John Conway erfunden und auf den Seiten des bekannt gemacht *Scientific American-* in den 1970er Jahren. Eine gute Einführung wird bereitgestellt, indem Sie im Wikipedia-Artikel [Conways Spiel of Life](https://en.wikipedia.org/wiki/Conway%27s_Game_of_Life).

Die Xamarin.Forms [ **GameOfLife** ](https://developer.xamarin.com/samples/xamarin-forms/BoxView/GameOfLife/) Programm definiert eine Klasse namens `LifeCell` abgeleitet, die `BoxView`. Diese Klasse kapselt die Logik der eine einzelne Zelle im Spiel Leben:

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

`LifeCell` drei weitere Eigenschaften hinzugefügt `BoxView`: die `Col` und `Row` speichern die Position der Zelle innerhalb des Rasters, Eigenschaften und die `IsAlive` Eigenschaft gibt den Zustand an. Die `IsAlive` Eigenschaftensätze auch die `Color` Eigenschaft der `BoxView` auf Schwarz festgelegt, wenn die Zelle ist aktiv, und weiß, wenn die Zelle nicht aktiv ist.

`LifeCell` installiert außerdem eine `TapGestureRecognizer` um den Status der Zellen umschalten, indem Sie auf diese zu ermöglichen. Die Klasse übersetzt die `Tapped` Ereignis aus der gestenerkennung in ihrem eigenen `Tapped` Ereignis.

Die **GameOfLife** Programm enthält auch eine `LifeGrid` Klasse, die ein Großteil der Logik des Spiels, kapselt und `MainPage` Klasse, des Programms visuelle Elemente verarbeitet. Dazu gehören ein Overlay, das die Regeln des Spiels beschreibt. Hier ist die Anwendung in Aktion, die mit ein paar Hundert `LifeCell` Objekte auf der Seite:

[![Spiel Leben](boxview-images/gameoflife-small.png "Spiel Leben")](boxview-images/gameoflife-large.png#lightbox "Spiel Leben")

<a name="digitalclock" />

## <a name="creating-a-digital-clock"></a>Erstellen eine digitale Uhr

Die [ **DotMatrixClock** ](https://developer.xamarin.com/samples/xamarin-forms/BoxView/DotMatrixClock/) Programm erstellt 210 `BoxView` Elemente, die Punkte ein altmodisches 5-von-7-Punktmatrix-Anzeige zu simulieren. Sie können die Zeit in entweder hoch-oder Querformat lesen, es ist jedoch größere im Querformat:

[![Matrix-Uhr](boxview-images/dotmatrixclock-small.png "Matrix-Uhr")](boxview-images/dotmatrixclock-large.png#lightbox "Punktmatrix Uhr")

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

Alles andere erfolgt im Code-Behind-Datei. Der Anzeigelogik Matrix-wird durch die Definition von mehreren Arrays, die beschreiben, die Punkte für jede der 10 Ziffern und einem Doppelpunkt, erheblich vereinfacht:

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

Diese Felder und zuletzt ein dreidimensionales Array von `BoxView` Elemente für die Punkt-Muster für die sechs Ziffern speichern.

Der Konstruktor erstellt alle die `BoxView` Elemente für die Ziffern und Doppelpunkt und initialisiert die `Color` Eigenschaft der `BoxView` Elemente dem Doppelpunkt:

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

Dieses Programm verwendet die relative Positionierung und größenanpassung-Feature von `AbsoluteLayout`. Die Breite und Höhe der einzelnen `BoxView` auf Bruchzahlen, insbesondere 85 % 1, geteilt durch die Anzahl der horizontalen und vertikalen Punkte festgelegt sind. Die Positionen werden auch auf Bruchzahlen festgelegt.

Da die Positionen und Größen Bezug auf die Gesamtgröße der sind die `AbsoluteLayout`, wird die `SizeChanged` Handler für die Seite muss nur festgelegt werden. eine `HeightRequest` von der `AbsoluteLayout`:

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

Die Breite der `AbsoluteLayout` wird automatisch festgelegt werden, da sie auf der Seite die volle Breite gestreckt wird.

Der endgültige Code in die `MainPage` -Klasse den Timer-Rückruf verarbeitet und Farben, die Punkte der einzelnen Ziffern. Die Definition der mehrdimensionalen Arrays am Anfang des Code-Behind-Datei hilft dabei, diese Logik den einfachsten Teil des Programms:

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

Eine Uhr Matrix-scheinen sich in einer Anwendung offensichtlich sein `BoxView`, aber `BoxView` Elemente sind auch kann der Umsetzung einer analogen Uhr:

[![BoxView Uhr](boxview-images/boxviewclock-small.png "BoxView Uhr")](boxview-images/boxviewclock-large.png#lightbox "BoxView Uhr")

Alle visuellen Elemente in der [ **BoxViewClock** ](https://developer.xamarin.com/samples/xamarin-forms/BoxView/BoxViewClock/) Programm sind untergeordnete Elemente von einem `AbsoluteLayout`. Diese Elemente haben eine Größe von über die `LayoutBounds` angefügte Eigenschaft und gedreht, mit der `Rotation` Eigenschaft.

Die drei `BoxView` Elemente für die Zeiger der Uhr in der XAML-Datei instanziiert jedoch keine positioniert oder angepasst werden:

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

Die Größe und Positionierung der alle der `BoxView` Elemente erfolgt in der `SizeChanged` Handler für die `AbsoluteLayout`. Wird aufgerufen, für die Klasse intern ein wenig Struktur `HandParams` wird beschrieben, die Größe der einzelnen der drei Zeiger relativ zu die Gesamtgröße der Uhr:

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

Die `SizeChanged` Ereignishandler bestimmt den Mittelpunkt und den Radius der der `AbsoluteLayout`, und klicken Sie dann die Größen und positioniert die 60 `BoxView` Elemente, die als Teilstriche verwendet. Die `for` Schleife endet, mit dem Festlegen der `Rotation` Eigenschaft der einzelnen `BoxView` Elemente. Am Ende der `SizeChanged` Handler auf, die `LayoutHand` aufgerufen, um die Größe und position der drei Zeiger der Uhr:

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

Die `LayoutHand` Methode Größen an und setzt Sie Zeiger darauf direkt auf die 12:00 Uhr-Position. Am Ende der Methode die `AnchorY` -Eigenschaftensatz auf eine Position in der Mitte des Uhr entspricht. Dies gibt an, den Mittelpunkt der Drehung.

Die Hände werden in der Rückruffunktion Timer ausgetauscht:

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

Der zweite Zeiger wird jedoch etwas anders behandelt: eine Animation, die EasingFunction angewendet, um die Bewegung scheinen mechanischen anstelle von smooth vornehmen. Bei jedem der zweite Zeiger wieder ein wenig abruft und overshoots klicken Sie dann auf das Ziel. Diese wenig Code fügt viel zu die wirklichkeitsgetreuen Bilder der Bewegung.

## <a name="conclusion"></a>Schlussbemerkung

Die `BoxView` mag zunächst, aber Sie einfach auf gesehen haben, kann es Recht vielseitig sein und können fast reproduzieren von Visuals, die normalerweise nur mit möglich sind Vektorgrafiken. Für komplexere Grafiken, wenden Sie sich an [mithilfe von SkiaSharp in Xamarin.Forms](~/xamarin-forms/user-interface/graphics/skiasharp/index.md).


## <a name="related-links"></a>Verwandte Links

- [Grundlegende BoxView (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/BoxView/BasicBoxView)
- [Textdekoration (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/BoxView/TextDecoration)
- [Farbe ListBox (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/BoxView/ColorListBox)
- [Spiel Leben (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/BoxView/GameOfLife)
- [Punktmatrix Format (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/BoxView/DotMatrixClock)
- [BoxView-Format (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/BoxView/BoxViewClock)
- [BoxView](xref:Xamarin.Forms.BoxView)
