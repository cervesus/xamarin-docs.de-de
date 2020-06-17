---
Title: " Xamarin.Forms AbsoluteLayout" Description: "in diesem Artikel wird erläutert, wie Sie die Xamarin.Forms Klasse" AbsoluteLayout "verwenden, um Pixel perfekte Benutzeroberflächen zu erstellen. Diese Klasse positioniert und passt die Größe untergeordneter Elemente proportional zu ihrer eigenen Größe und Position oder mit absoluten Werten an. "
ms. Prod: xamarin ms. assetid: 01a5cce0-ad45-4806-84td-72c007005b38 ms. Technology: xamarin-Forms Author: davidbritch ms. Author: dabritch ms. Date: 11/25/2015 NO-LOC: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="xamarinforms-absolutelayout"></a>Xamarin.FormsAbsoluteLayout

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout)

[`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout)Positionen und Größen untergeordnete Elemente sind proportional zu ihrer eigenen Größe und Position oder mit absoluten Werten. Untergeordnete Ansichten können mithilfe von proportionalen Werten oder statischen Werten positioniert und vergrößert werden, und proportionale und statische Werte können gemischt werden.

[![](absolute-layout-images/layouts-sml.png "Xamarin.Forms Layouts")](absolute-layout-images/layouts.png#lightbox "Xamarin.Forms Layouts")

In diesem Artikel wird Folgendes behandelt:

- **[Zweck](#purpose)** &ndash; Allgemeine Verwendungsmöglichkeiten für `AbsoluteLayout` .
- **[Verwendung](#usage)** &ndash; wie Sie verwenden `AbsoluteLayout` , um den gewünschten Entwurf zu erzielen.
  - **[Proportionale Layouts](#proportional-layouts)** &ndash; verstehen, wie proportionale Werte in einem funktionieren `AbsoluteLayout` .
  - **[Angeben von Werten](#specifying-values)** &ndash; verstehen, wie proportionale und absolute Werte angegeben werden.
  - **[Proportionale Werte](#proportional-values)** &ndash; verstehen, wie proportionale Werte funktionieren.
    - **[Absolute Werte](#absolute-values)** &ndash; verstehen, wie absolute Werte funktionieren.

## <a name="purpose"></a>Zweck

Aufgrund des Positionierungs Modells von `AbsoluteLayout` ist das Layout relativ einfach, Elemente so zu positionieren, dass Sie mit einer beliebigen Seite des Layouts oder zentriert sind. Bei proportionalen Größen und Positionen können Elemente in einem `AbsoluteLayout` automatisch auf jede beliebige Ansichts Größe skaliert werden. Bei Elementen, bei denen nur die Position, aber nicht die Größe skaliert werden soll, können absolute und proportionale Werte gemischt werden.

`AbsoluteLayout`kann überall dort verwendet werden, wo Elemente in einer Ansicht positioniert werden müssen, und ist besonders nützlich, wenn Elemente an Kanten ausgerichtet werden.

## <a name="usage"></a>Verwendung

### <a name="proportional-layouts"></a>Proportionale Layouts

`AbsoluteLayout`verfügt über ein eindeutiges Anker Modell, bei dem der Anker des Elements relativ zu seinem Element positioniert ist, da das Element relativ zum Layout positioniert wird, wenn eine proportionale Positionierung verwendet wird. Wenn absolute Positionierung verwendet wird, befindet sich der Anker in der Ansicht an (0,0). Dies hat zwei wichtige Konsequenzen:

- Elemente können nicht mithilfe von proportionalen Werten auf dem Bildschirm positioniert werden.
- Elemente können unabhängig von der Größe des Layouts oder Geräts zuverlässig entlang einer beliebigen Seite des Layouts oder in der Mitte positioniert werden.

`AbsoluteLayout`, wie `RelativeLayout` , kann Elemente so positionieren, dass Sie sich überlappen.

Beachten Sie im folgenden Screenshot, dass der Anker des Felds ein weißer Punkt ist. Beachten Sie die Beziehung zwischen dem Anker und dem Feld beim Durchlaufen des Layouts:

![](absolute-layout-images/anchor-start.png "Anchor at Start")
![](absolute-layout-images/anchor-center.png "Anchor at Center")
![](absolute-layout-images/anchor-end.png "Anchor at End")

### <a name="specifying-values"></a>Angeben von Werten

Sichten innerhalb eines `AbsoluteLayout` werden mit vier Werten positioniert:

- **X** &ndash; die x-Position (horizontal) des Ansichts Ankers.
- **Y** &ndash; (vertikal) Position des Ansichts Ankers
- **Breite** &ndash; die Breite der Ansicht.
- **Höhe** &ndash; die Höhe der Ansicht.

Jeder dieser Werte kann als [proportionaler](#proportional-values) Wert oder als [absoluter](#absolute-values) Wert festgelegt werden.

Werte werden als eine Kombination aus Begrenzungen und einem-Flag angegeben. `LayoutBounds`besteht [`Rectangle`](xref:Xamarin.Forms.Rectangle) aus vier Werten: `x` , `y` , `width` , `height` .

### <a name="absolutelayoutflags"></a>Absolutelayoutflags

[`AbsoluteLayoutFlags`](xref:Xamarin.Forms.AbsoluteLayoutFlags)Gibt an, wie Werte interpretiert werden, und verfügt über die folgenden vordefinierten Optionen:

- **Keine** &ndash; interpretiert alle Werte als absolut. Dies ist der Standardwert, wenn keine LayoutFlags angegeben werden.
- **Alle** &ndash; interpretiert alle Werte als proportional.
- **Widthproportional** &ndash; interpretiert den `Width` Wert als proportional und alle anderen Werte als absolut.
- **Heightproportional** &ndash; interpretiert nur den height-Wert als proportional zu allen anderen Werten, die absolut sind.
- **Xproportional** &ndash; interpretiert den `X` Wert als proportional, während alle anderen Werte als absolut behandelt werden.
- **Yproportional** &ndash; interpretiert den `Y` Wert als proportional, während alle anderen Werte als absolut behandelt werden.
- **Positionproportional** &ndash; interpretiert den `X` -Wert und den- `Y` Wert als proportional, während die Größen Werte als absolut interpretiert werden.
- **Sizeproportional** &ndash; interpretiert den `Width` -Wert und den- `Height` Wert als proportional, während die Positionswerte absolut sind.

In XAML werden Begrenzungen und Flags als Teil der Definition von Sichten innerhalb des Layouts mithilfe der-Eigenschaft festgelegt `AbsoluteLayout.LayoutBounds` . Grenzen werden als durch Trennzeichen getrennte Liste von Werten, `X` ,, `Y` `Width` und `Height` in dieser Reihenfolge festgelegt. Flags werden auch in der Deklaration von Sichten im Layout mithilfe der- `AbsoluteLayout.LayoutFlags` Eigenschaft angegeben. Beachten Sie, dass Flags in XAML mithilfe einer durch Trennzeichen getrennten Liste kombiniert werden können. Betrachten Sie das folgende Beispiel:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="LayoutSamples.AbsoluteLayoutExploration"
Title="Absolute Layout Exploration">
    <ContentPage.Content>
        <AbsoluteLayout>
            <Label Text="I'm centered on iPhone 4 but no other device"
                AbsoluteLayout.LayoutBounds="115,150,100,100" LineBreakMode="WordWrap"  />
            <Label Text="I'm bottom center on every device."
                AbsoluteLayout.LayoutBounds=".5,1,.5,.1" AbsoluteLayout.LayoutFlags="All"
                LineBreakMode="WordWrap"  />
            <BoxView Color="Olive"  AbsoluteLayout.LayoutBounds="1,.5, 25, 100"
                AbsoluteLayout.LayoutFlags="PositionProportional" />
            <BoxView Color="Red" AbsoluteLayout.LayoutBounds="0,.5,25,100"
                AbsoluteLayout.LayoutFlags="PositionProportional" />
            <BoxView Color="Blue" AbsoluteLayout.LayoutBounds=".5,0,100,25"
                AbsoluteLayout.LayoutFlags="PositionProportional" />
        </AbsoluteLayout>
    </ContentPage.Content>
</ContentPage>
```

![](absolute-layout-images/exploration.png "AbsoluteLayout Examples")

Beachten Sie Folgendes:

- Die Bezeichnung in der Mitte wird mit den Werten der absoluten Größe und Position positioniert. Aus diesem Grund wird er auf iPhone 4S und niedriger zentriert, aber nicht auf größeren Geräten zentriert.
- Der Text am unteren Rand des Layouts wird mithilfe von proportionalen Größen-und Positions Werten positioniert. Sie wird immer unten in der Mitte des Layouts angezeigt, aber ihre Größe vergrößert sich mit größeren layoutgrößen.
- Dreifarbige `BoxView` e werden am oberen, linken und rechten Rand des Bildschirms mithilfe von proportionaler Position und absoluter Größe positioniert.

Folgendes erreicht das gleiche Layout in c#:

```csharp
public class AbsoluteLayoutExplorationCode : ContentPage
{
    public AbsoluteLayoutExplorationCode ()
    {
        Title = "Absolute Layout Exploration - Code";
        var layout = new AbsoluteLayout();

        var centerLabel = new Label {
        Text = "I'm centered on iPhone 4 but no other device.",
        LineBreakMode = LineBreakMode.WordWrap};

        AbsoluteLayout.SetLayoutBounds (centerLabel, new Rectangle (115, 159, 100, 100));
        // No need to set layout flags, absolute positioning is the default

        var bottomLabel = new Label { Text = "I'm bottom center on every device.", LineBreakMode = LineBreakMode.WordWrap };
        AbsoluteLayout.SetLayoutBounds (bottomLabel, new Rectangle (.5, 1, .5, .1));
        AbsoluteLayout.SetLayoutFlags (bottomLabel, AbsoluteLayoutFlags.All);

        var rightBox = new BoxView{ Color = Color.Olive };
        AbsoluteLayout.SetLayoutBounds (rightBox, new Rectangle (1, .5, 25, 100));
        AbsoluteLayout.SetLayoutFlags (rightBox, AbsoluteLayoutFlags.PositionProportional);

        var leftBox = new BoxView{ Color = Color.Red };
        AbsoluteLayout.SetLayoutBounds (leftBox, new Rectangle (0, .5, 25, 100));
        AbsoluteLayout.SetLayoutFlags (leftBox, AbsoluteLayoutFlags.PositionProportional);

        var topBox = new BoxView{ Color = Color.Blue };
        AbsoluteLayout.SetLayoutBounds (topBox, new Rectangle (.5, 0, 100, 25));
        AbsoluteLayout.SetLayoutFlags (topBox, AbsoluteLayoutFlags.PositionProportional);

        layout.Children.Add (bottomLabel);
        layout.Children.Add (centerLabel);
        layout.Children.Add (rightBox);
        layout.Children.Add (leftBox);
        layout.Children.Add (topBox);

        Content = layout;
    }
}
```

### <a name="proportional-values"></a>Proportionale Werte

Proportional Werte definieren eine Beziehung zwischen einem Layout und einer Ansicht. Diese Beziehung definiert die Position oder den Skalierungs Wert einer untergeordneten Ansicht als Anteil des entsprechenden Werts des übergeordneten Layouts. Diese Werte werden als `double` s mit Werten zwischen 0 und 1 ausgedrückt.

Proportional Werte werden verwendet, um Sichten innerhalb des Layouts zu positionieren und zu verkleinern. Wenn die Breite einer Sicht als proportional festgelegt wird, ist der resultierende Breitenwert der Anteil multipliziert mit der `AbsoluteLayout` Breite. Beispielsweise ist `AbsoluteLayout` `500` die gerenderte Breite der Ansicht 250 (500 x. 5), wenn die Breite und die Ansicht auf eine proportionale Breite von 0,5 festgelegt sind.

Um proportionale Werte zu verwenden, legen `LayoutBounds` Sie mit (x, y) Proportionen und proportionalen Größen fest, und legen Sie dann `LayoutFlags` auf fest `All` .

In XAML:

```xaml
<Label Text="I'm bottom center on every device."
  AbsoluteLayout.LayoutBounds=".5,1,.5,.1" AbsoluteLayout.LayoutFlags="All"  />
```

In C#:

```csharp
var label = new Label {Text = "I'm bottom center on every device."};
AbsoluteLayout.SetLayoutBounds(label, new Rectangle(.5,1,.5,.1));
AbsoluteLayout.SetLayoutFlags(label, AbsoluteLayoutFlags.All);
```

### <a name="absolute-values"></a>Absolute Werte

Absolute Werte definieren explizit, wo Sichten innerhalb des Layouts positioniert werden sollen. Im Gegensatz zu proportionalen Werten können absolute Werte eine Ansicht positionieren und anpassen, die nicht innerhalb der Grenzen des Layouts liegt.

Die Verwendung absoluter Werte für die Positionierung kann gefährlich sein, wenn die Größe des Layouts nicht bekannt ist. Wenn absolute Positionen verwendet werden, kann ein Element in der Mitte des Bildschirms auf einer beliebigen anderen Größe versetzt werden. Es ist wichtig, dass Sie Ihre APP über die verschiedenen Bildschirmgrößen der unterstützten Geräte hinweg testen.

Wenn Sie absolute layoutwerte verwenden möchten, legen `LayoutBounds` Sie using (x, y)-Koordinaten und explizite Größen fest, und legen Sie dann `LayoutFlags` auf fest `None`

In XAML:

```xaml
<Label Text="I'm centered on iPhone 4 but no other device."
  AbsoluteLayout.LayoutBounds="115,150,100,100"  />
```

In C#:

```csharp
var label = new Label {Text = "I'm centered on iPhone 4 but no other device."};
AbsoluteLayout.SetLayoutBounds(label, new Rectangle(115,150,100,100));
```

## <a name="exploring-a-complex-layout"></a>Untersuchen eines komplexen Layouts

Jedes der Layouts hat Stärken und Schwächen zum Erstellen bestimmter Layouts. In dieser Reihe von layoutartikeln wurde eine Beispiel-App mit dem gleichen Seitenlayout erstellt, das mithilfe von drei verschiedenen Layouts implementiert wurde.

Beachten Sie den folgenden XAML-Code:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="TheBusinessTumble.AbsoluteLayoutPage"
Title="AbsoluteLayout">
    <ContentPage.ToolbarItems>
        <ToolbarItem Text="Save" />
    </ContentPage.ToolbarItems>
    <ContentPage.Content>
        <ScrollView>
            <AbsoluteLayout BackgroundColor="Maroon">
                <BoxView BackgroundColor="Gray" AbsoluteLayout.LayoutBounds="0
                    0,1,100" AbsoluteLayout.LayoutFlags="XProportional,YProportional,WidthProportional" />
                <Button BackgroundColor="Maroon"
                    AbsoluteLayout.LayoutBounds=".5,55,70,70" AbsoluteLayout.LayoutFlags="XProportional" BorderRadius="35" />
                <Button BackgroundColor="Red" AbsoluteLayout.LayoutBounds=".5
                    60,60,60" AbsoluteLayout.LayoutFlags="XProportional" BorderRadius="30" />
                <Label Text="User Name" FontAttributes="Bold" FontSize="26"
                    TextColor="Black" HorizontalTextAlignment="Center" AbsoluteLayout.LayoutBounds=".5,140,1,40" AbsoluteLayout.LayoutFlags="XProportional,WidthProportional" />
                <Entry Text="Bio + Hashtags" TextColor="White"
                    BackgroundColor="Maroon" AbsoluteLayout.LayoutBounds=".5,180,1,40" AbsoluteLayout.LayoutFlags="XProportional,WidthProportional" />
                <AbsoluteLayout BackgroundColor="White"
                        AbsoluteLayout.LayoutBounds="0, 220, 1, 50" AbsoluteLayout.LayoutFlags="XProportional,WidthProportional">
                    <AbsoluteLayout AbsoluteLayout.LayoutBounds="0,0,.5,1" AbsoluteLayout.LayoutFlags="WidthProportional,HeightProportional">
                        <Button BackgroundColor="Black" BorderRadius="20"
                            AbsoluteLayout.LayoutBounds="5,.5,40,40" AbsoluteLayout.LayoutFlags="YProportional" />
                        <Label Text="Accent Color" TextColor="Black"
                            AbsoluteLayout.LayoutBounds="50,.55,1,25" AbsoluteLayout.LayoutFlags="YProportional,WidthProportional" />
                    </AbsoluteLayout>
                    <AbsoluteLayout AbsoluteLayout.LayoutBounds="1,0,.5,1" AbsoluteLayout.LayoutFlags="WidthProportional,HeightProportional,XProportional">
                        <Button BackgroundColor="Maroon" BorderRadius="20"
                            AbsoluteLayout.LayoutBounds="5,.5,40,40" AbsoluteLayout.LayoutFlags="YProportional" />
                        <Label Text="Primary Color" TextColor="Black"
                            AbsoluteLayout.LayoutBounds="50,.55,1,25" AbsoluteLayout.LayoutFlags="YProportional,WidthProportional" />
                    </AbsoluteLayout>
                </AbsoluteLayout>
                <AbsoluteLayout AbsoluteLayout.LayoutBounds="0,270,1,50" AbsoluteLayout.LayoutFlags="WidthProportional" Padding="5,0,0,0">
                    <Label Text="Age:" TextColor="White"
                        AbsoluteLayout.LayoutBounds="0,25,.25,50" AbsoluteLayout.LayoutFlags="WidthProportional" />
                    <Entry Text="35" TextColor="White" BackgroundColor="Maroon"
                        AbsoluteLayout.LayoutBounds="1,10,.75,50" AbsoluteLayout.LayoutFlags="XProportional,WidthProportional" />
                </AbsoluteLayout>
                <AbsoluteLayout AbsoluteLayout.LayoutBounds="0,320,1,50" AbsoluteLayout.LayoutFlags="WidthProportional" Padding="5,0,0,0">
                    <Label Text="Interests:" TextColor="White"
                        AbsoluteLayout.LayoutBounds="0,25,.25,50" AbsoluteLayout.LayoutFlags="WidthProportional" />
                    <Entry Text="Xamarin.Forms" TextColor="White"
                        BackgroundColor="Maroon" AbsoluteLayout.LayoutBounds="1,10,.75,50" AbsoluteLayout.LayoutFlags="XProportional,WidthProportional" />
                </AbsoluteLayout>
                <AbsoluteLayout AbsoluteLayout.LayoutBounds="0,370,1,50" AbsoluteLayout.LayoutFlags="WidthProportional" Padding="5,0,0,0">
                    <Label Text="Ask me about:" TextColor="White"
                        AbsoluteLayout.LayoutBounds="0,25,.25,50" AbsoluteLayout.LayoutFlags="WidthProportional" />
                    <Entry Text="Xamarin, C#, .NET, Mono" TextColor="White"
                        BackgroundColor="Maroon" AbsoluteLayout.LayoutBounds="1,10,.75,50" AbsoluteLayout.LayoutFlags="XProportional,WidthProportional" />
                </AbsoluteLayout>
            </AbsoluteLayout>
        </ScrollView>
    </ContentPage.Content>
</ContentPage>
```

Der obige Code führt zum folgenden Layout:

![](absolute-layout-images/abs.png "Complex AbsoluteLayout")

Beachten Sie, dass `AbsoluteLayout` s geschachtelt sind, da in einigen Fällen die Schachtelung von Layouts einfacher sein kann, als alle Elemente innerhalb desselben Layouts darzustellen.

## <a name="related-links"></a>Verwandte Links

- [Erstellen von Mobile Apps mit Xamarin.Forms , Kapitel 14](https://developer.xamarin.com/r/xamarin-forms/book/chapter14.pdf)
- [AbsoluteLayout](xref:Xamarin.Forms.AbsoluteLayout)
- [Layout (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout)
- [Businesstumm Beispiel (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-businesstumble)
