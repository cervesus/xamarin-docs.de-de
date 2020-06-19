---
title: Xamarin.FormsBoxview
description: In diesem Artikel wird erläutert, wie Sie ein farbiges Rechteck für die Dekoration, Grafik und Interaktion in einer- Xamarin.Forms Anwendung verwenden.
ms.prod: xamarin
ms.assetid: 4CBF703D-84A0-4CDF-A433-5926B587782A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/26/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 06f1813bafb34a9c32603490e66f8caa6c6a6a22
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84573819"
---
# <a name="xamarinforms-boxview"></a>Xamarin.FormsBoxview

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-basicboxview)

[`BoxView`](xref:Xamarin.Forms.BoxView)Rendert ein einfaches Rechteck mit einer angegebenen Breite, Höhe und Farbe. Sie können `BoxView` für Dekorationen, rudimentäre Grafiken und für die Interaktion mit dem Benutzer über die Fingereingabe verwenden.

Da Xamarin.Forms nicht über ein integriertes Vektorgrafik System verfügt, hilft bei der `BoxView` Kompensation von. Einige der in diesem Artikel beschriebenen Beispiel Programme verwenden `BoxView` zum Rendern von Grafiken. Die `BoxView` Größe der Größe kann einer Linie mit einer bestimmten Breite und Stärke ähneln und dann mithilfe der-Eigenschaft durch einen beliebigen Winkel gedreht werden `Rotation` .

Obwohl `BoxView` einfache Grafiken imitieren kann, empfiehlt es sich, die [Verwendung von skiasharp Xamarin.Forms in](~/xamarin-forms/user-interface/graphics/skiasharp/index.md) für anspruchsvollere Grafik Anforderungen zu untersuchen.

## <a name="setting-boxview-color-and-size"></a>Festlegen der Farbe und Größe von boxview

In der Regel legen Sie die folgenden Eigenschaften von fest `BoxView` :

- [`Color`](xref:Xamarin.Forms.BoxView.Color), um die Farbe festzulegen.
- [`CornerRadius`](xref:Xamarin.Forms.BoxView.CornerRadius), um den Eckradius festzulegen.
- [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest), um die Breite von `BoxView` in geräteunabhängigen Einheiten festzulegen.
- [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest), um die Höhe des festzulegen `BoxView` .

Die- `Color` Eigenschaft ist vom Typ `Color` . die-Eigenschaft kann auf einen beliebigen Wert festgelegt werden `Color` , einschließlich der 141 static-schreibgeschützten Felder mit benannten Farben, die alphabetisch von `AliceBlue` bis reichen `YellowGreen` .

Die- `CornerRadius` Eigenschaft ist vom Typ [`CornerRadius`](xref:Xamarin.Forms.CornerRadius) . die-Eigenschaft kann auf einen einzelnen URI-Wert (Uniform Corner) oder auf eine Struktur festgelegt werden, die `double` durch vier Werte definiert wird, die oben `CornerRadius` `double` Links, oben rechts, unten links und unten rechts im angewendet werden `BoxView` .

Die `WidthRequest` -Eigenschaft und die-Eigenschaft `HeightRequest` spielen nur dann eine Rolle, wenn `BoxView` im Layout *nicht eingeschränkt* ist. Dies ist der Fall, wenn der Layoutcontainer die Größe des Kindes kennen muss, z. b. Wenn ein untergeordnetes Element `BoxView` einer Zelle mit automatischer Größe im `Grid` Layout ist. Eine `BoxView` ist auch nicht eingeschränkt, wenn deren `HorizontalOptions` -Eigenschaft und-Eigenschaft `VerticalOptions` auf andere Werte als festgelegt sind `LayoutOptions.Fill` . Wenn die nicht `BoxView` eingeschränkt ist, aber die `WidthRequest` -Eigenschaft und die-Eigenschaft `HeightRequest` nicht festgelegt sind, werden Breite oder Höhe auf die Standardwerte 40 Einheiten oder etwa 1/4 Zoll auf mobilen Geräten festgelegt.

Die `WidthRequest` -Eigenschaft und die-Eigenschaft `HeightRequest` werden ignoriert `BoxView` , wenn im Layout *eingeschränkt* ist. in diesem Fall erzwingt der Layoutcontainer eine eigene Größe auf dem `BoxView` .

Eine `BoxView` kann in einer Dimension eingeschränkt und in einer anderen uneingeschränkt sein. Wenn beispielsweise ein untergeordnetes Element `BoxView` eines vertikalen ist `StackLayout` , wird die vertikale Dimension der `BoxView` nicht eingeschränkt und die horizontale Dimension ist im allgemeinen beschränkt. Es gibt jedoch Ausnahmen für diese horizontale Dimension: Wenn die-Eigenschaft der- `BoxView` `HorizontalOptions` Eigenschaft auf einen anderen Wert als festgelegt ist `LayoutOptions.Fill` , wird die horizontale Dimension ebenfalls nicht eingeschränkt. Es ist auch möglich, dass der `StackLayout` selbst über eine nicht eingeschränkte horizontale Dimension verfügt. in diesem Fall `BoxView` wird der auch horizontal uneingeschränkt eingeschränkt.

Im [**basicboxview**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-basicboxview) -Beispiel wird ein ein-Zoll-Quadrat `BoxView` in der Mitte der Seite angezeigt:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:BasicBoxView"
             x:Class="BasicBoxView.MainPage">

    <BoxView Color="CornflowerBlue"
             CornerRadius="10"
             WidthRequest="160"
             HeightRequest="160"
             VerticalOptions="Center"
             HorizontalOptions="Center" />

</ContentPage>
```

So sieht das Ergebnis aus:

[![Grundlegende boxview](boxview-images/basicboxview-small.png "Grundlegende boxview")](boxview-images/basicboxview-large.png#lightbox "Basicboxview")

Wenn die `VerticalOptions` -Eigenschaft und die-Eigenschaft `HorizontalOptions` aus dem- `BoxView` Tag entfernt oder auf festgelegt werden `Fill` , `BoxView` wird der durch die Größe der Seite eingeschränkt und erweitert, um die Seite auszufüllen.

Ein `BoxView` kann auch ein untergeordnetes Element eines sein `AbsoluteLayout` . In diesem Fall werden sowohl der Speicherort als auch die Größe der `BoxView` mithilfe der `LayoutBounds` angefügten bindbaren Eigenschaft festgelegt. Die `AbsoluteLayout` wird im Artikel " [**AbsoluteLayout**](~/xamarin-forms/user-interface/layouts/absolute-layout.md)" erläutert.

Beispiele für alle diese Fälle finden Sie in den folgenden Beispielprogrammen.

## <a name="rendering-text-decorations"></a>Rendern von Text Dekorationen

Sie können das verwenden `BoxView` , um Ihren Seiten einige einfache Dekorationen in Form von horizontalen und vertikalen Linien hinzuzufügen. Dies wird im [**TextDecoration**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-textdecoration) -Beispiel veranschaulicht. Alle visuellen Elemente des Programms sind in der Datei " **MainPage. XAML** " definiert, die mehrere `Label` -und- `BoxView` Elemente in der `StackLayout` hier gezeigten enthält:

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

Alle folgenden Markup-Elemente sind untergeordnete Elemente von `StackLayout` . Dieses Markup besteht aus verschiedenen Typen von dekorativen `BoxView` Elementen, die mit dem-Element verwendet werden `Label` :

[![Text Dekoration](boxview-images/textdecoration-small.png "Text Dekoration")](boxview-images/textdecoration-large.png#lightbox "Text Dekoration")

Der stilvolle Header am oberen Rand der Seite wird mit einem erreicht `AbsoluteLayout` , dessen untergeordnete `BoxView` Elemente vier Elemente und ein sind `Label` , denen alle bestimmte Orte und Größen zugewiesen sind:

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

In der XAML-Datei `AbsoluteLayout` folgt ein `Label` mit formatiertem Text, der beschreibt `AbsoluteLayout` .

Sie können eine Text Zeichenfolge unterstreichen, indem Sie sowohl das `Label` als auch das `BoxView` in einem einschließen `StackLayout` , dessen `HorizontalOptions` Wert auf einen anderen Wert als festgelegt ist `Fill` . Die Breite des `StackLayout` wird dann von der Breite des bestimmt `Label` , das dann diese Breite auf dem-Wert festlegt `BoxView` . Dem `BoxView` wird nur eine explizite Höhe zugewiesen:

```xaml
<StackLayout HorizontalOptions="Center">
    <Label Text="Underlined Text"
           FontSize="24" />
    <BoxView HeightRequest="2" />
</StackLayout>
```

Sie können diese Technik nicht verwenden, um einzelne Wörter innerhalb von längeren Text Zeichenfolgen oder einem Absatz zu unterstreichen.

Es ist auch möglich, ein- `BoxView` Element zu verwenden, das einem HTML- `hr` Element (horizontaler Regel) ähnelt. Lassen Sie einfach die Breite des `BoxView` von seinem übergeordneten Container festgelegt, in diesem Fall `StackLayout` :

```xaml
<BoxView HeightRequest="3" />
```

Schließlich können Sie eine vertikale Linie auf einer Seite eines textabtexts zeichnen, indem Sie sowohl `BoxView` als auch `Label` in einen horizontalen einschließen `StackLayout` . In diesem Fall entspricht die Höhe von der Höhe `BoxView` von `StackLayout` , die durch die Höhe von gesteuert wird `Label` :

```xaml
<StackLayout Orientation="Horizontal">
    <BoxView WidthRequest="4"
             Margin="0, 0, 10, 0" />
    <Label>

        ···

    </Label>
</StackLayout>
```

## <a name="listing-colors-with-boxview"></a>Auflisten von Farben mit boxview

Der `BoxView` eignet sich zum Anzeigen von Farben. Dieses Programm verwendet eine `ListView` , um alle öffentlichen statischen schreibgeschützten Felder der Xamarin.Forms `Color` Struktur aufzulisten:

[![ListView-Farben](boxview-images/listviewcolors-small.png "ListView-Farben")](boxview-images/listviewcolors-large.png#lightbox "ListView-Farben")

Das [**listviewcolors**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-listviewcolors) -Programm enthält eine Klasse mit dem Namen `NamedColor` . Der statische Konstruktor verwendet Reflektion, um auf alle Felder der `Color` Struktur zuzugreifen und ein- `NamedColor` Objekt für jeden zu erstellen. Diese werden in der statischen- `All` Eigenschaft gespeichert:

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

Die visuellen Programmelemente werden in der XAML-Datei beschrieben. Die- `ItemsSource` Eigenschaft des `ListView` wird auf die statische- `NamedColor.All` Eigenschaft festgelegt, was bedeutet, dass `ListView` alle einzelnen `NamedColor` Objekte anzeigt:

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

Die- `NamedColor` Objekte werden durch das-Objekt formatiert `ViewCell` , das als Daten Vorlage von festgelegt wird `ListView` . Diese Vorlage enthält eine `BoxView` , deren- `Color` Eigenschaft an die- `Color` Eigenschaft des-Objekts gebunden ist `NamedColor` .

## <a name="playing-the-game-of-life-by-subclassing-boxview"></a>Spielen des Lebenszyklus nach dem Unterklassen-boxview

Das Spiel der Lebensdauer ist eine Mobil Funk Automat, die von der Mathematikerin John-Seite erfunden und in den 70er Jahren auf den Seiten von *Scientific American* populär gemacht wurde. Eine gute Einführung finden Sie im Wikipedia-Artikel [-Spiel der Lebensdauer](https://en.wikipedia.org/wiki/Conway%27s_Game_of_Life).

Das Xamarin.Forms [**gameoslife**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-gameoflife) -Programm definiert eine Klasse mit dem Namen `LifeCell` , die von abgeleitet wird `BoxView` . Diese Klasse kapselt die Logik einer einzelnen Zelle im Spiel der Lebensdauer:

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

`LifeCell`Fügt drei weitere Eigenschaften hinzu `BoxView` : die `Col` -Eigenschaft und die- `Row` Eigenschaft speichern die Position der Zelle innerhalb des Rasters, und die- `IsAlive` Eigenschaft gibt ihren Zustand an. Die `IsAlive` -Eigenschaft legt auch die `Color` -Eigenschaft von `BoxView` auf Schwarz fest, wenn die Zelle aktiv ist, und weiß, wenn die Zelle nicht aktiv ist.

`LifeCell`installiert auch ein `TapGestureRecognizer` , um dem Benutzer zu ermöglichen, den Zustand von Zellen durch Tippen zu wechseln. Die-Klasse übersetzt das- `Tapped` Ereignis von der Gestenerkennung in ein eigenes- `Tapped` Ereignis.

Das **gameoolife** -Programm umfasst auch eine `LifeGrid` Klasse, die einen Großteil der Logik des Spiels kapselt, und eine Klasse, die `MainPage` die visuellen Elemente des Programms behandelt. Diese umfassen ein Overlay, das die Regeln des Spiels beschreibt. Hier ist das Programm in Aktion, das ein paar hundert `LifeCell` Objekte auf der Seite anzeigt:

[![Lebenszyklus](boxview-images/gameoflife-small.png "Lebenszyklus")](boxview-images/gameoflife-large.png#lightbox "Lebenszyklus")

## <a name="creating-a-digital-clock"></a>Erstellen einer digitalen Uhr

Das [**dotmatrixclock**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-dotmatrixclock) -Programm erstellt 210 `BoxView` Elemente, um die Punkte einer alten 5-x-7-Punkt-Matrix-Anzeige zu simulieren. Sie können die Zeit im hoch-oder Querformat lesen, aber Sie ist größer im Querformat:

[![Punkt-Matrix-Uhr](boxview-images/dotmatrixclock-small.png "Punkt-Matrix-Uhr")](boxview-images/dotmatrixclock-large.png#lightbox "Punkt-Matrix-Uhr")

Die XAML-Datei führt nur wenig mehr aus, als die `AbsoluteLayout` für die Uhr verwendete zu instanziieren:

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

Alles andere wird in der Code Behind-Datei angezeigt. Die Anzeige Logik der Punktmatrix wird erheblich vereinfacht, indem mehrere Arrays definiert werden, die die Punkte beschreiben, die den 10 Ziffern und einem Doppelpunkt entsprechen:

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

Diese Felder schließen mit einem dreidimensionalen Array von- `BoxView` Elementen, um die Punktmuster für die sechs Ziffern zu speichern.

Der-Konstruktor erstellt alle- `BoxView` Elemente für die Ziffern und den Doppelpunkt und initialisiert außerdem die- `Color` Eigenschaft der- `BoxView` Elemente für den Doppelpunkt:

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

Dieses Programm verwendet das relative Positions-und Größen Anpassungs Feature von `AbsoluteLayout` . Breite und Höhe der einzelnen `BoxView` Werte werden auf Bruchzahlen festgelegt, insbesondere 85% von 1 dividiert durch die Anzahl der horizontalen und vertikalen Punkte. Die Positionen werden auch auf Bruch Komma Werte festgelegt.

Da alle Positionen und Größen relativ zur Gesamtgröße von sind `AbsoluteLayout` , `SizeChanged` muss der Handler für die Seite nur eine der folgenden festlegen `HeightRequest` `AbsoluteLayout` :

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

Die Breite des `AbsoluteLayout` wird automatisch festgelegt, da Sie auf die vollständige Breite der Seite reicht.

Der endgültige Code in der `MainPage` -Klasse verarbeitet den Timer-Rückruf und Farben die Punkte der einzelnen Ziffern. Die Definition der mehrdimensionalen Arrays am Anfang der Code-Behind-Datei trägt dazu bei, diese Logik zum einfachsten Teil des Programms zu machen:

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

## <a name="creating-an-analog-clock"></a>Erstellen einer analogen Uhr

Eine Punkt-Matrix-Uhr scheint eine offensichtliche Anwendung von zu sein `BoxView` , aber `BoxView` Elemente können auch eine analoge Uhr realisieren:

[![Boxview-Uhr](boxview-images/boxviewclock-small.png "Boxview-Uhr")](boxview-images/boxviewclock-large.png#lightbox "Boxview-Uhr")

Alle visuellen Elemente im [**boxviewclock**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-boxviewclock) -Programm sind untergeordnete Elemente eines `AbsoluteLayout` . Diese Elemente werden mit der `LayoutBounds` angefügten-Eigenschaft vergrößert und mithilfe der- `Rotation` Eigenschaft gedreht.

Die drei `BoxView` Elemente für die Uhrzeitangabe werden in der XAML-Datei instanziiert, jedoch nicht positioniert oder Größe:

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

Der Konstruktor der Code Behind-Datei instanziiert die 60 `BoxView` Elemente für die Teil Striche um den Umfang der Uhr:

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

Die Größenanpassung und Positionierung aller `BoxView` Elemente treten im- `SizeChanged` Handler für den auf `AbsoluteLayout` . Eine kleine Struktur, die für die Klasse aufgerufen wird, `HandParams` beschreibt die Größe der drei Hände in Bezug auf die Gesamtgröße der Uhr:

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

Der `SizeChanged` -Handler bestimmt den Mittelpunkt und den Radius von `AbsoluteLayout` und ordnet dann die Größe und Position der 60 Elemente an, die als Teil Striche `BoxView` verwendet werden. Die `for` Schleife wird beendet, indem die- `Rotation` Eigenschaft der einzelnen Elemente festgelegt wird `BoxView` . Am Ende des `SizeChanged` Handlers `LayoutHand` wird die-Methode aufgerufen, um die drei Hände der Uhr zu verkleinern und zu positionieren:

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

Die `LayoutHand` Methoden Größen und positionieren jede Hand, um direkt auf die 12:00-Position zu zeigen. Am Ende der Methode wird die- `AnchorY` Eigenschaft auf eine Position festgelegt, die der Mitte der Uhr entspricht. Dies gibt den Mittelpunkt der Drehung an.

Die Hände werden in der Timer-Rückruffunktion gedreht:

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

Die zweite Seite wird etwas anders behandelt: eine Animations Beschleunigungs Funktion wird angewendet, um die Bewegung als mechanisch und nicht als glatt zu gestalten. Bei jedem Tick Ruft die zweite Hand ein wenig ab und überschreitet dann das Ziel. Diese Menge an Code erhöht den Realismus der Bewegung.

## <a name="related-links"></a>Verwandte Links

- [Grundlegende boxview (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-basicboxview)
- [Text Dekoration (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-textdecoration)
- [ListView-Farben (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-listviewcolors/)
- [Lebenszyklus (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-gameoflife)
- [Punkt-Matrix-Uhr (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-dotmatrixclock)
- [Boxview-Uhr (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-boxviewclock)
- [BoxView](xref:Xamarin.Forms.BoxView)
