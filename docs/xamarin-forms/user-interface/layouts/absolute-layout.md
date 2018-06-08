---
title: AbsoluteLayout
description: Verwenden Sie AbsoluteLayout Pixel perfekte Benutzeroberflächen zu erstellen.
ms.prod: xamarin
ms.assetid: 01A5CCE0-AD45-4806-84FD-72C007005B38
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/25/2015
ms.openlocfilehash: 506a9a4916cf2cf9105d59f56648e339d664a3d2
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/07/2018
ms.locfileid: "34848387"
---
# <a name="absolutelayout"></a>AbsoluteLayout

[`AbsoluteLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.AbsoluteLayout/) Position und Größe untergeordnete-Elemente, die Größe und Position oder Absolute Werte proportional. Untergeordnete Ansichten möglicherweise positioniert und Größe mit proportional oder statische Werte und proportional und statische Werte kombiniert werden können.

[![](absolute-layout-images/layouts-sml.png "Xamarin.Forms Layouts")](absolute-layout-images/layouts.png#lightbox "Xamarin.Forms Layouts")

Dieser Artikel umfasst folgende Themen:

- **[Zweck](#Purpose)**  &ndash; Allgemeine Verwendungsmöglichkeiten für `AbsoluteLayout`.
- **[Verwendung](#Usage)**  &ndash; wie `AbsoluteLayout` auf den gewünschten Entwurf zu erzielen.
  - **[Layouts proportional](#Proportional_Layouts)**  &ndash; verstehen, wie proportional Werte funktionieren in einer `AbsoluteLayout`.
  - **[Angeben von Werten](#Specifying_Values)**  &ndash; zu verstehen, wie Proportional und Absolute Werte angegeben werden.
  - **[Proportional Werte](#Proportional_Values)**  &ndash; verstehen, wie proportionale Werten arbeiten.
    - **[Absolute Werte](#Absolute_Values)**  &ndash; Arbeitsweise Absolute Werte.

<a name="Purpose" />

## <a name="purpose"></a>Zweck

Aufgrund der Positionierung Objektmodell `AbsoluteLayout`, das Layout ist es relativ einfach zu Elemente zu positionieren, sodass sie eine Seite des Layouts ausgerichtet oder zentriert werden. Mit proportional Größen und Positionen, die Elemente in einer `AbsoluteLayout` können automatisch skalieren, um eine beliebige Größe anzeigen. Für Elemente, in dem nur die Position jedoch nicht die Größe skaliert werden sollen, können Werte für absolute als auch proportional gemischt werden.

`AbsoluteLayout` kann verwendet werden überall Elemente müssen innerhalb einer Ansicht positioniert werden und ist besonders nützlich, wenn Elemente unterhalb der ausrichten.

<a name="Usage" />

## <a name="usage"></a>Verwendung

<a name="Proportional_Layouts" />

### <a name="proportional-layouts"></a>Proportional Layouts

`AbsoluteLayout` verfügt über eine eindeutige Anker-Modell, bei dem Anker des Elements relativ zu seinem Element positioniert ist, wie das Element in Bezug auf das Layout positioniert ist, wenn proportional Positionierung verwendet wird. Wenn absolute Positionierung verwendet wird, wird der Anker bei (0,0) innerhalb der Ansicht. Dies hat zwei wichtige Folgen im Klaren sein:

- Elemente können nicht außerhalb des Bildschirms proportional Werte mit positioniert werden.
- Elemente können an jeder Seite des Layouts oder in der Mitte, unabhängig von der Größe des Layouts oder Geräts zuverlässig positioniert werden.

`AbsoluteLayout`, z. B. `RelativeLayout`, kann Elemente zu positionieren, sodass sie sich überlappen.

Notieren Sie sich im folgenden Screenshot ist dem Anker des Felds ist ein weißer Punkt. Beachten Sie die Beziehung zwischen dem Anker und das Feld aus, wie sie das Layout durchlaufen:

![](absolute-layout-images/anchor-start.png "Anchor beim Start")
![](absolute-layout-images/anchor-center.png "Anker zentriert")
![](absolute-layout-images/anchor-end.png "Anker am Ende")

<a name="Specifying_Values" />

### <a name="specifying-values"></a>Angeben von Werten

Ansichten in einer `AbsoluteLayout` positioniert sind mit vier Werten:

- **X** &ndash; die (horizontale) X-Position von der Ansicht Anker
- **Y** &ndash; die (vertikale) y-Position des Anker mit der Ansicht
- **Breite** &ndash; die Breite der Sicht
- **Höhe** &ndash; die Höhe der Ansicht

Jeder dieser Werte kann festgelegt werden, als ein [proportional](#Proportional_Values) Wert oder ein [absolute](#Absolute_Values) Wert.

Werte werden als eine Kombination von Grenzen und ein Flag angegeben. `LayoutBounds` ist eine [ `Rectangle` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Rectangle/) bestehend aus vier Werten: `x`, `y`, `width`, `height`.

### <a name="absolutelayoutflags"></a>AbsoluteLayoutFlags

[`AbsoluteLayoutFlags`](https://developer.xamarin.com/api/type/Xamarin.Forms.AbsoluteLayoutFlags/) Gibt an, wie die Werte interpretiert werden, und enthält die folgenden vordefinierten Optionen aus:

- **Keine** &ndash; interpretiert alle Werte als Absolute. Dies ist der Standardwert, wenn keine Layout Flags angegeben werden.
- **Alle** &ndash; interpretiert alle Werte als proportional.
- **WidthProportional** &ndash; interpretiert die `Width` Proportional sowie alle anderen Werte als absoluter Wert.
- **HeightProportional** &ndash; nur dem Höhenwert als proportional und alle anderen absolute Werte interpretiert.
- **XProportional** &ndash; interpretiert die `X` -Wert wie proportional, während alle anderen Werte als Absolute behandelt.
- **YProportional** &ndash; interpretiert die `Y` -Wert wie proportional, während alle anderen Werte als Absolute behandelt.
- **PositionProportional** &ndash; interpretiert die `X` und `Y` Werte als proportional, während die Größenwerte als Absolute interpretiert werden.
- **SizeProportional** &ndash; interpretiert die `Width` und `Height` -Werte als proportional, während die Positionswerte absolute sind.

In XAML, Grenzen und Flags werden festgelegt als Teil der Definition von Ansichten innerhalb des Layouts mit den `AbsoluteLayout.LayoutBounds` Eigenschaft. Grenzen werden festgelegt, als eine durch Trennzeichen getrennte Liste von Werten, `X`, `Y`, `Width`, und `Height`in dieser Reihenfolge. Flags werden auch angegeben, in der Deklaration von Ansichten im Layout mit der `AbsoluteLayout.LayoutFlags` Eigenschaft. Beachten Sie, dass die Flags in XAML, die mit einer durch Trennzeichen getrennte Liste kombiniert werden können. Betrachten Sie das folgende Beispiel:

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

![](absolute-layout-images/exploration.png "AbsoluteLayout-Beispiele")

Beachten Sie Folgendes:

- Die Bezeichnung in der Mitte befindet mit absoluten Werten von Größe und Position. Aus diesem Grund wird es auf iPhone 4 s zentriert und niedriger, aber nicht auf mehr Geräten zentriert angezeigt.
- Der Text am unteren Rand des Layouts befindet sich proportional Größe und Position Werte verwenden. Erscheint immer in der unteren Mitte des Layouts, aber seine Größe wächst mit größere Layout.
- Drei farbig `BoxView`s werden an den Kanten oberen linken und rechten Rand des Bildschirms mithilfe von proportional Position und die absolute Größe positioniert.

Die folgenden erzielt dasselbe Layout in C# geschrieben wird:

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
<a name="Proportional_Values" />

### <a name="proportional-values"></a>Proportional Werte

Proportional Werte definieren eine Beziehung zwischen einem Layout und einer Ansicht. Diese Beziehung definiert einer untergeordnete Ansicht Position oder Skalierungswert als Teile des entsprechenden Wertes des übergeordneten Layouts an. Diese Werte werden als ausgedrückt `double`s mit Werten zwischen 0 und 1.

Proportional Werte werden an die Position und Größe von Ansichten innerhalb des Layouts verwendet. Wenn eine Sicht Breite als Verhältnis festgelegt ist, ist die der resultierenden Breitenwert also das Größenverhältnis multipliziert mit der `AbsoluteLayout`des Breite. Z. B. mit einem `AbsoluteLayout` Breite `500` und eine Sicht, die auf einer proportional Breite von.5, die gerenderte Breite der Sicht festgelegt, werden 250 (500 x.5 ergeben

Um Werte proportional zu nutzen, setzen `LayoutBounds` (X, y) mit Proportionen und proportional Größen verfügbar, und legen Sie dann `LayoutFlags` auf `All`.

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

<a name="Absolute_Values" />

### <a name="absolute-values"></a>Absolute Werte

Absolute Werte definieren explizit an, wie Ansichten innerhalb des Layouts positioniert werden soll. Im Gegensatz zu proportional Werte werden Absolute Werte können Position und Größe einer Sicht, die nicht innerhalb der Grenzen des Layouts passt.

Absolute Werte können für die Positionierung gefährlich sein, wenn die Größe des Layouts nicht bekannt ist. Bei Verwendung von absolute Positionen konnte ein Element in der Mitte des Bildschirms auf eine Größe an eine beliebige andere Größe versetzt werden. Es ist wichtig, Ihre app auf verschiedenen Bildschirmgrößen der unterstützten Geräte zu testen.

Legen Sie für die Verwendung absoluter layoutwerten `LayoutBounds` verwenden (X, y) Koordinaten und explizite Größen verfügbar, und legen Sie dann `LayoutFlags` auf `None`.

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

## <a name="exploring-a-complex-layout"></a>Untersuchen ein komplexes Layout

Jedes der Layouts aufweisen Stärken und Schwächen für bestimmte Layouts erstellen. In der gesamten diese Artikelreihe Layout wurde eine Beispiel-app mit dem gleichen Seitenlayout mithilfe von drei verschiedenen Layouts implementiert erstellt.

Betrachten Sie das folgende XAML-Code ein:

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

Der obige Code erhalten Sie im folgenden dargestellt:

![](absolute-layout-images/abs.png "Komplexe AbsoluteLayout")

Beachten Sie, dass `AbsoluteLayout`s geschachtelt werden, da in einigen Fällen Schachteln von Layouts einfacher, als vertrauenswürdig sind alle Elemente innerhalb des gleichen Layouts sein kann.

## <a name="related-links"></a>Verwandte Links

- [Beim Erstellen mobiler Apps mit Xamarin.Forms, Kapitel 14](https://developer.xamarin.com/r/xamarin-forms/book/chapter14.pdf)
- [AbsoluteLayout](https://developer.xamarin.com/api/type/Xamarin.Forms.AbsoluteLayout/)
- [Layout (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)
- [BusinessTumble-Beispiel (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BusinessTumble/)
