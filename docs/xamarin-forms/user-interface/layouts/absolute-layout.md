---
title: Xamarin.Forms von "AbsoluteLayout"
description: In diesem Artikel wird erläutert, wie Sie die Xamarin.Forms von "AbsoluteLayout"-Klasse, um einer präzisen Benutzeroberflächen zu erstellen. Diese Klasse passt Position und Größe von untergeordneten Elementen, die proportional zu seiner eigenen Größe und Position oder Absolute Werte.
ms.prod: xamarin
ms.assetid: 01A5CCE0-AD45-4806-84FD-72C007005B38
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/25/2015
ms.openlocfilehash: 0389587b2abffa751349a66eedc4a3800aa2a99d
ms.sourcegitcommit: 5f972a757030a1f17f99177127b4b853816a1173
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/21/2019
ms.locfileid: "69888528"
---
# <a name="xamarinforms-absolutelayout"></a>Xamarin.Forms von "AbsoluteLayout"

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout)

[`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout) passt Position und Größe der untergeordneten Elemente, die proportional zu seiner eigenen Größe und Position oder Absolute Werte. Untergeordnete Ansichten können und Größe mit proportional oder statische Werte, und ein proportional und statische Werte können kombiniert werden.

[![](absolute-layout-images/layouts-sml.png "Xamarin.Forms-Layouts")](absolute-layout-images/layouts.png#lightbox "Xamarin.Forms-Layouts")

Dieser Artikel behandelt:

- **[Zweck](#Purpose)**  &ndash; Allgemeine Verwendungsmöglichkeiten für `AbsoluteLayout`.
- **[Nutzung](#Usage)**  &ndash; mit `AbsoluteLayout` Erreichen des gewünschten Entwurfs.
  - **[Proportionale Layouts](#Proportional_Layouts)**  &ndash; verstehen, wie proportional Werte funktionieren in einer `AbsoluteLayout`.
  - **[Angeben von Werten](#Specifying_Values)**  &ndash; zu verstehen, wie die proportionale und absoluten Werten angegeben werden.
  - **[Proportionale Werte](#Proportional_Values)**  &ndash; verstehen, wie proportionale Werten arbeiten.
    - **[Absolute Werte](#Absolute_Values)**  &ndash; Arbeitsweise Absolute Werte.

<a name="Purpose" />

## <a name="purpose"></a>Zweck

Aufgrund der Positionierung-Objektmodell `AbsoluteLayout`, das Layout ist es relativ einfach, um Elemente zu positionieren, sodass sie jede Seite des Layouts ausgerichtet oder zentriert ausgerichtet sind. Mit proportional Größen und Positionen, Elemente in einem `AbsoluteLayout` können auf eine beliebige Ansichtsgröße automatisch skalieren. Für Elemente, in dem nur die Position, aber nicht die Größe skaliert werden soll, können die absoluten und proportional Werte gemischt werden.

`AbsoluteLayout` kann überall dort verwendet werden Elemente müssen innerhalb einer Ansicht positioniert werden soll, und ist besonders nützlich, wenn es sich bei Elementen, die Kanten ausrichten.

<a name="Usage" />

## <a name="usage"></a>Verwendung

<a name="Proportional_Layouts" />

### <a name="proportional-layouts"></a>Proportionale Layouts

`AbsoluteLayout` verfügt über ein eindeutiges Anker-Modell, bei dem die Verankerung des Elements relativ zu seinem Element positioniert wird, wie das Element relativ zu das Layout positioniert wird, wenn proportional Positionierung verwendet wird. Wenn absolute Positionierung verwendet wird, wird der Anker bei (0,0), in der Ansicht. Dies hat zwei wichtige Konsequenzen:

- Elemente können nicht aus dem Sie proportional Werte positioniert werden.
- Elemente können zuverlässig auf jeder Seite des Layouts oder in der Mitte, unabhängig von der Größe des Layouts oder Geräts positioniert werden.

`AbsoluteLayout`, z. B. `RelativeLayout`, kann sich um Elemente zu positionieren, sodass sie sich überlappen.

Notieren Sie sich im folgenden Screenshot ist der Anker des Felds ist ein weißer Punkt. Beachten Sie die Beziehung zwischen dem Anker und das Feld aus, wie sie das Layout durchläuft:

![](absolute-layout-images/anchor-start.png "Anker zu Beginn")
![](absolute-layout-images/anchor-center.png "Anker zentriert")
![](absolute-layout-images/anchor-end.png "Anker am Ende")

<a name="Specifying_Values" />

### <a name="specifying-values"></a>Angeben von Werten

Die Ansichten in einer `AbsoluteLayout` positioniert sind, mithilfe von vier Werten:

- **X** &ndash; die (horizontale) X-Position des Ankers mit der Ansicht
- **Y** &ndash; die (vertikale) y-Position des Ankers mit der Ansicht
- **Breite** &ndash; die Breite der Sicht
- **Höhe** &ndash; die Höhe der Ansicht

Jeder dieser Werte kann festgelegt werden, als eine [proportional](#Proportional_Values) Wert oder ein [absolute](#Absolute_Values) Wert.

Werte werden als eine Kombination von Grenzen und ein Flag angegeben. `LayoutBounds` ist eine [ `Rectangle` ](xref:Xamarin.Forms.Rectangle) bestehend aus vier Werten: `x`, `y`, `width`, `height`.

### <a name="absolutelayoutflags"></a>AbsoluteLayoutFlags

[`AbsoluteLayoutFlags`](xref:Xamarin.Forms.AbsoluteLayoutFlags) Gibt an, wie Werte interpretiert werden, und enthält die folgenden vordefinierten Optionen aus:

- **Keine** &ndash; alle Werte als Absolute interpretiert. Dies ist der Standardwert, wenn keine Layout-Flags angegeben werden.
- **Alle** &ndash; interpretiert alle Werte als proportional.
- **WidthProportional** &ndash; interpretiert die `Width` Proportional und alle anderen Werte als absoluter Wert.
- **HeightProportional** &ndash; nur Wert für die Höhe als proportionaler und alle anderen absolute Werte interpretiert.
- **XProportional** &ndash; interpretiert die `X` als proportional und zum Behandeln von allen anderen Werten als absoluter Wert.
- **YProportional** &ndash; interpretiert die `Y` als proportional und zum Behandeln von allen anderen Werten als absoluter Wert.
- **PositionProportional** &ndash; interpretiert die `X` und `Y` Werte als proportional ist, während der Größenwerte als Absolute interpretiert werden.
- **SizeProportional** &ndash; interpretiert die `Width` und `Height` Werte als proportional, während die Positionswerte absolut sind.

In XAML, Grenzen und die Flags werden festgelegt als Teil der Definition der Ansichten in das Layout an, mit der `AbsoluteLayout.LayoutBounds` Eigenschaft. Grenzen werden als eine durch Trennzeichen getrennte Liste von Werten festgelegt `X`, `Y`, `Width`, und `Height`in dieser Reihenfolge. Flags werden ebenfalls angegeben, in der Deklaration von Ansichten in das Layout mithilfe der `AbsoluteLayout.LayoutFlags` Eigenschaft. Beachten Sie, dass die Flags, die in XAML, die mit einer durch Trennzeichen getrennte Liste kombiniert werden können. Betrachten Sie das folgende Beispiel:

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

![](absolute-layout-images/exploration.png "Beispiele für die von \"AbsoluteLayout\"")

Beachten Sie Folgendes:

- Die Bezeichnung in der Mitte befindet sich unter Verwendung absoluter Werte für Größe und Position. Aus diesem Grund wird er auf iPhone 4 s zentriert und niedriger, aber nicht zentriert auf größeren Geräten angezeigt.
- Der Text am Ende das Layout befindet sich proportionale Größe und Position Werte verwenden. Es wird immer angezeigt, in der Mitte der Unterseite des Layouts, aber wächst die Größe mit größere Layout.
- Drei farbig `BoxView`s werden an den oberen, linken und rechten Rändern des Bildschirms mithilfe von proportional Position und Größe von absolut positioniert.

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

### <a name="proportional-values"></a>Proportionale Werte

Proportionale Werte definieren Sie eine Beziehung zwischen einem Layout und einer Ansicht. Diese Beziehung definiert einer untergeordnete Ansicht der Position oder Skalierungswert als Teile der entsprechende Wert des übergeordneten Layouts. Diese Werte werden als ausgedrückt `double`mit Werten zwischen 0 und 1.

Proportionale Werte werden auf die Position und Größe von Ansichten innerhalb des Layouts verwendet. Wenn also eine Ansicht der Breite eines Anteils festgelegt ist, den Wert für die resultierenden Breite ist der Zeitanteil, multipliziert mit der `AbsoluteLayout`der Breite. Z. B. mit einem `AbsoluteLayout` Breite `500` und eine Ansicht, die auf die proportionale Breite der.5, die gerenderte Breite der Sicht festgelegt, werden 250 (500 x.5 zwei

Um proportionale Werte zu verwenden, legen `LayoutBounds` verwenden (X, y) Proportionen und proportionale Größe, und legen Sie dann `LayoutFlags` zu `All`.

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

Absolute Werte definieren explizit die, in denen Sichten innerhalb des Layouts positioniert werden soll. Im Gegensatz zu proportional Werte sind Absolute Werte funktionsfähig positionieren und Anpassen der Größe einer Ansicht, die nicht innerhalb der Grenzen des Layouts passen.

Verwenden Absolute Werte für die Positionierung kann gefährlich sein, wenn die Größe des Layouts nicht bekannt ist. Wenn Sie absolute Positionen verwenden zu können, kann ein Element in der Mitte des Bildschirms auf eine Größe bei anderen versetzt werden. Es ist wichtig zum Testen Ihrer app für die verschiedenen Bildschirmgrößen von Ihre unterstützten Geräte.

Um absolute layoutwerten zu verwenden, legen `LayoutBounds` (X, y) mithilfe von Koordinaten und explizite Größe, und legen Sie dann `LayoutFlags` zu `None`.

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

## <a name="exploring-a-complex-layout"></a>Erkunden ein komplexes Layout

Jede der Layouts hat Stärken und Schwächen für bestimmte Layouts erstellen. In dieser Artikelreihe Layout eine Beispiel-app mit der gleichen Layout mit drei verschiedenen Layouts implementiert wurde.

Betrachten Sie das folgende XAML:

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

Der obige Code führt das folgende Layout:

![](absolute-layout-images/abs.png "Komplexe von \"AbsoluteLayout\"")

Beachten Sie, dass `AbsoluteLayout`s geschachtelt sind, da in einigen Fällen Schachteln von Layouts einfacher als die Darstellung aller Elemente innerhalb des gleichen Layouts sein kann.

## <a name="related-links"></a>Verwandte Links

- [Erstellen von mobilen Apps mit Xamarin.Forms Kapitel 14](https://developer.xamarin.com/r/xamarin-forms/book/chapter14.pdf)
- [AbsoluteLayout](xref:Xamarin.Forms.AbsoluteLayout)
- [Layout (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout)
- [BusinessTumble-Beispiel (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-businesstumble)
