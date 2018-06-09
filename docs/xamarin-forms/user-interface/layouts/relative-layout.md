---
title: Xamarin.Forms RelativeLayout
description: In diesem Artikel wird erläutert, wie die Xamarin.Forms-RelativeLayout-Klasse verwendet wird, Benutzeroberflächen zu erstellen, die an jede Bildschirmgröße anpassen skaliert wird.
ms.prod: xamarin
ms.assetid: 2530BCB8-01B8-4C4F-BF14-CA53659F1B5A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/25/2015
ms.openlocfilehash: 712092e58a7a7358ba1fa808614822c7988e6105
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/08/2018
ms.locfileid: "35245054"
---
# <a name="xamarinforms-relativelayout"></a>Xamarin.Forms RelativeLayout

`RelativeLayout` wird auf die Position und Größe von Ansichten relativ zu den Eigenschaften der Ansichten Layout oder gleichgeordnetes Element verwendet werden. Im Gegensatz zu `AbsoluteLayout`, `RelativeLayout` verfügt nicht über das Konzept der gleitenden Anker und keine Funktionen zum Positionieren von Elementen relativ zum unteren oder rechten Rands des Layouts. `RelativeLayout` Positionieren von Elementen außerhalb seiner eigenen Grenzen unterstützt.

[![](relative-layout-images/layouts-sml.png "Xamarin.Forms Layouts")](relative-layout-images/layouts.png#lightbox "Xamarin.Forms Layouts")

## <a name="purpose"></a>Zweck

`RelativeLayout` kann verwendet werden, um Ansichten auf Bildschirm relativ zum das allgemeine Layout oder mit anderen Ansichten einzufügen.

![](relative-layout-images/flag.png "RelativeLayout durchsuchen")

## <a name="usage"></a>Verwendung

### <a name="understanding-constraints"></a>Grundlegendes zu Einschränkungen

Position und Größe einer Ansicht innerhalb einer `RelativeLayout` erfolgt mit Einschränkungen. Ein Einschränkungsausdruck zählen die folgende Informationen:

- **Typ** &ndash; gibt an, ob die Einschränkung relativ zum übergeordneten oder zu einer anderen Ansicht ist.
- **Eigenschaft** &ndash; , dessen Eigenschaft als Grundlage für die Einschränkung zu verwenden.
- **Faktor** &ndash; der Faktor, um auf den Wert der Eigenschaft anzuwenden.
- **Konstante** &ndash; der Wert, der als Offset des Werts verwenden.
- **ElementName** &ndash; den Namen der Sicht, die die Einschränkung liegt.

Einschränkungen werden in XAML als ausgedrückt `ConstraintExpression`s. Betrachten Sie das folgende Beispiel:

```xaml
<BoxView Color="Green" WidthRequest="50" HeightRequest="50"
    RelativeLayout.XConstraint =
      "{ConstraintExpression Type=RelativeToParent,
                             Property=Width,
                             Factor=0.5,
                             Constant=-100}"
    RelativeLayout.YConstraint =
      "{ConstraintExpression Type=RelativeToParent,
                             Property=Height,
                             Factor=0.5,
                             Constant=-100}" />
```

In c# werden Einschränkungen etwas anders ausgedrückt mit Funktionen und nicht als Ausdrücke für die Sicht. Einschränkungen werden als Argumente für des Layouts angegeben `Add` Methode:

```csharp
layout.Children.Add(box, Constraint.RelativeToParent((parent) =>
    {
      return (.5 * parent.Width) - 100;
    }),
    Constraint.RelativeToParent((parent) =>
    {
        return (.5 * parent.Height) - 100;
    }),
    Constraint.Constant(50), Constraint.Constant(50));
```

Beachten Sie die folgenden Aspekte des oben genannten Layouts aus:

- Die `x` und `y` Einschränkungen werden mit ihren eigenen Einschränkungen angegeben.
- In c# werden die relative Einschränkungen als Funktionen definiert. Konzepte wie `Factor` nicht vorhanden sind, jedoch manuell implementiert werden können.
- Des Feld `x` Koordinate als halbe Breite des übergeordneten Elements,-100 definiert ist.
- Des Feld `y` Koordinate ist definiert als die Hälfte der Höhe des übergeordneten Elements, und 100 an.

> [!NOTE]
> Aufgrund der Art und Weise, die Einschränkungen definiert sind, ist es möglich, stellen Sie komplexe Layouts in c# als durch XAML-Code angegeben werden können.

Sowohl der obigen Beispiele Definieren von Einschränkungen als `RelativeToParent` &ndash; , also ihre Werte sind relativ zum übergeordneten Element. Es ist auch möglich, relativ zu einer anderen Ansicht Einschränkungen zu definieren. Dies ermöglicht eine intuitivere (für den Entwickler) Layouts und den Zweck des Codes Layout mehr sofort erkennbar machen kann.

Betrachten Sie ein Layout, in denen ein Element 20 Pixel, die niedriger ist als eine andere werden muss. Wenn beide Elemente mit konstanten Werten definiert sind, gibt die untere möglicherweise seine `Y` Einschränkung definiert als eine Konstante, die größer als 20 Pixel ist die `Y` Einschränkung des Elements höher. Dieser Ansatz fällt kurz, wenn eine Menge mit höhere Element positioniert ist, damit die Größe in Pixel bekannt ist. In diesem Fall wird das Einschränken des Elements auf der Grundlage der Position des Elements unter Umständen stabiler:

```xaml
<RelativeLayout>
    <BoxView Color="Red" x:Name="redBox"
        RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent,
            Property=Height,Factor=.15,Constant=0}"
        RelativeLayout.WidthConstraint="{ConstraintExpression
            Type=RelativeToParent,Property=Width,Factor=1,Constant=0}"
        RelativeLayout.HeightConstraint="{ConstraintExpression
            Type=RelativeToParent,Property=Height,Factor=.8,Constant=0}" />
    <BoxView Color="Blue"
        RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToView,
            ElementName=redBox,Property=Y,Factor=1,Constant=20}"
        RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToView,
            ElementName=redBox,Property=X,Factor=1,Constant=20}"
        RelativeLayout.WidthConstraint="{ConstraintExpression
            Type=RelativeToParent,Property=Width,Factor=.5,Constant=0}"
        RelativeLayout.HeightConstraint="{ConstraintExpression
            Type=RelativeToParent,Property=Height,Factor=.5,Constant=0}" />
</RelativeLayout>
```

Dasselbe Layout in c# erreichen Sie folgendermaßen:

```csharp
layout.Children.Add (redBox, Constraint.RelativeToParent ((parent) => {
        return parent.X;
    }), Constraint.RelativeToParent ((parent) => {
        return parent.Y * .15;
    }), Constraint.RelativeToParent((parent) => {
        return parent.Width;
    }), Constraint.RelativeToParent((parent) => {
        return parent.Height * .8;
    }));
layout.Children.Add (blueBox, Constraint.RelativeToView (redBox, (Parent, sibling) => {
        return sibling.X + 20;
    }), Constraint.RelativeToView (blueBox, (parent, sibling) => {
        return sibling.Y + 20;
    }), Constraint.RelativeToParent((parent) => {
        return parent.Width * .5;
    }), Constraint.RelativeToParent((parent) => {
        return parent.Height * .5;
    }));
```

Dies erzeugt die folgende Ausgabe, mit dem blauen Position bestimmt _relative_ auf die Position neben dem roten Feld:

![](relative-layout-images/red-blue-box.png "RelativeLayout mit roten und blauen BoxViews")

### <a name="sizing"></a>Anpassen der Größe

Ansichten von vorgegebene `RelativeLayout` stehen zwei Optionen zur Angabe ihrer Größe:

- `HeightRequest & WidthRequest`
- `RelativeLayout.WidthConstraint` & `RelativeLayout.HeightConstraint`

`HeightRequest` und `WidthRequest` Geben Sie die gewünschten Höhe und Breite der Sicht, jedoch möglicherweise von Layouts überschrieben werden, nach Bedarf. `WidthConstraint` und `HeightConstraint` unterstützt das Festlegen der Höhe und Breite als ein Wert in Bezug auf des Layouts oder einer anderen Ansicht Eigenschaften oder als einen konstanten Wert.

## <a name="exploring-a-complex-layout"></a>Untersuchen ein komplexes Layout
Jedes der Layouts aufweisen Stärken und Schwächen für bestimmte Layouts erstellen. In der gesamten diese Artikelreihe Layout wurde eine Beispiel-app mit dem gleichen Seitenlayout mithilfe von drei verschiedenen Layouts implementiert erstellt.

Betrachten Sie das folgende XAML-Code ein:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="TheBusinessTumble.RelativeLayoutPage"
BackgroundColor="Maroon"
Title="RelativeLayout">
    <ContentPage.Content>
    <ScrollView>
      <RelativeLayout>
        <BoxView Color="Gray" HeightRequest="100"
            RelativeLayout.WidthConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=1}" />
        <Button BorderRadius="35" x:Name="imageCircleBack"
            BackgroundColor="Maroon" HeightRequest="70" WidthRequest="70" RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToParent,Property=Width, Factor=.5, Constant = -35}" RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Factor=0, Property=Y, Constant=70}" />
        <Button BorderRadius="30" BackgroundColor="Red" HeightRequest="60"
            WidthRequest="60" RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToView, ElementName=imageCircleBack, Property=X, Factor=1,Constant=5}" RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Factor=0, Property=Y, Constant=75}" />
        <Label Text="User Name" FontAttributes="Bold" FontSize="26"
            HorizontalTextAlignment="Center" RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Y, Factor=0, Constant=140}" RelativeLayout.WidthConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=1}" />
        <Entry Text="Bio + Hashtags" TextColor="White" BackgroundColor="Maroon"
            RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Y, Factor=0, Constant=180}" RelativeLayout.WidthConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=1}" />
        <RelativeLayout BackgroundColor="White" RelativeLayout.YConstraint="
            {ConstraintExpression Type=RelativeToParent, Property=Y, Factor=0, Constant=220}" HeightRequest="60" RelativeLayout.WidthConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=1}" >
            <BoxView BackgroundColor="Black" WidthRequest="50"
                HeightRequest="50" RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Y, Factor=0, Constant=5}" RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToParent, Property=X, Factor=0, Constant=5}" />
            <BoxView BackgroundColor="Maroon" WidthRequest="50"
                HeightRequest="50" RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Y, Factor=0, Constant=5}" RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=0.5, Constant=}" />
            <Label FontSize="14" TextColor="Black" Text="Accent Color"
                RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Y, Factor=0, Constant=20}" RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToParent, Property=X, Factor=0, Constant=60}" />
            <Label FontSize="14" TextColor="Black" Text="Primary Color"
                RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Y, Factor=0, Constant=20}" RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=0.5, Constant=55}" />
        </RelativeLayout>
        <RelativeLayout Padding="5,0,0,0">
          <Label FontSize="14" Text="Age:" TextColor="White"
            RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=0,Constant=305}"
            RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=0, Constant=10}"
            RelativeLayout.WidthConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width,Factor=.25,Constant=0}"
            RelativeLayout.HeightConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=0,Constant=50}" />
          <Entry Text="35" TextColor="White" BackgroundColor="Maroon"
            RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=0,Constant=280}"
            RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=0.3, Constant=0}"
            RelativeLayout.WidthConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width,Factor=0.75,Constant=0}"
            RelativeLayout.HeightConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=0,Constant=50}" />
        </RelativeLayout>
        <RelativeLayout  Padding="5,0,0,0">
          <Label FontSize="14" Text="Interests:" TextColor="White"
            RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=0,Constant=345}"
            RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=0, Constant=10}"
            RelativeLayout.WidthConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width,Factor=.25,Constant=0}"
            RelativeLayout.HeightConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=0,Constant=50}" />
          <Entry Text="Xamarin.Forms" TextColor="White" BackgroundColor="Maroon"
            RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=0,Constant=320}"
            RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=0.3, Constant=0}"
            RelativeLayout.WidthConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width,Factor=0.75,Constant=0}"
            RelativeLayout.HeightConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=0,Constant=50}" />
        </RelativeLayout>
        <RelativeLayout  Padding="5,0,0,0">
          <Label FontSize="14" Text="Ask me about:" TextColor="White"
            LineBreakMode="WordWrap"
            RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=0,Constant=395}"
            RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=0, Constant=10}"
            RelativeLayout.WidthConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width,Factor=.25,Constant=0}"
            RelativeLayout.HeightConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=0,Constant=50}" />
          <Entry Text="Xamarin, C#, .NET, Mono" TextColor="White"
            BackgroundColor="Maroon"
            RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=0,Constant=370}"
            RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=0.3, Constant=0}"
            RelativeLayout.WidthConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width,Factor=0.75,Constant=0}"
            RelativeLayout.HeightConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=0,Constant=50}" />
        </RelativeLayout>
      </RelativeLayout>
    </ScrollView>
  </ContentPage.Content>
</ContentPage>
```

Der obige Code erhalten Sie im folgenden dargestellt:

![](relative-layout-images/relative.png "Komplexe RelativeLayout")

Beachten Sie, dass `RelativeLayouts`s geschachtelt werden, da in einigen Fällen Schachteln von Layouts einfacher, als vertrauenswürdig sind alle Elemente innerhalb des gleichen Layouts sein kann. Beachten Sie auch, die einige Elemente sind `RelativeToView`, da, die für das Layout von einfacher und intuitiver gestattet, wenn die Beziehungen zwischen den Ansichten Positionierung geführt.


## <a name="related-links"></a>Verwandte Links

- [Layout (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)
- [BusinessTumble-Beispiel (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BusinessTumble/)
