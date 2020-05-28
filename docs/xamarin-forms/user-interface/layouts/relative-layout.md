---
title: Xamarin.FormsRelativeLayout
description: In diesem Artikel wird erläutert, wie Sie die Xamarin.Forms relativelayout-Klasse verwenden, um Benutzeroberflächen zu erstellen, die an jede Bildschirmgröße angepasst werden können.
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: f250b109f759bcf6bb7fa4ac0573743ac12c4bc1
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2020
ms.locfileid: "84127191"
---
# <a name="xamarinforms-relativelayout"></a>Xamarin.FormsRelativeLayout

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout)

`RelativeLayout`wird verwendet, um Ansichten relativ zu Eigenschaften des Layout oder der gleich geordneten Sichten zu positionieren und zu verkleinern. Im `AbsoluteLayout` Gegensatz `RelativeLayout` zu verfügt nicht über das Konzept des Verschiebungs Ankers und verfügt nicht über Funktionen zum Positionieren von Elementen in Relation zum unteren oder rechten Rand des Layouts. `RelativeLayout`unterstützt das Positionieren von Elementen außerhalb ihrer eigenen Begrenzungen.

[![](relative-layout-images/layouts-sml.png "Xamarin.Forms Layouts")](relative-layout-images/layouts.png#lightbox "Xamarin.Forms Layouts")

## <a name="purpose"></a>Zweck

`RelativeLayout`kann verwendet werden, um Ansichten auf dem Bildschirm relativ zum Gesamt Layout oder zu anderen Sichten zu positionieren.

![](relative-layout-images/flag.png "RelativeLayout Exploration")

## <a name="usage"></a>Verbrauch

### <a name="understanding-constraints"></a>Grundlegendes zu Einschränkungen

Positionierung und Größe einer Ansicht in einem werden `RelativeLayout` mit Einschränkungen erreicht. Ein Einschränkungs Ausdruck kann die folgenden Informationen enthalten:

- **Typ** &ndash; Gibt an, ob die Einschränkung relativ zum übergeordneten Element oder zu einer anderen Ansicht ist.
- **Eigenschaft** &ndash; welche Eigenschaft als Grundlage für die Einschränkung verwendet werden soll.
- **Faktor** &ndash; der Faktor, der auf den Eigenschafts Wert angewendet werden soll.
- **Konstante** &ndash; der Wert, der als Offset des Werts verwendet werden soll.
- **ElementName** &ndash; der Name der Ansicht, zu der die Einschränkung relativ ist.

In XAML werden Einschränkungen als s ausgedrückt `ConstraintExpression` . Sehen Sie sich das folgende Beispiel an:

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

In c# werden Einschränkungen etwas anders ausgedrückt, wobei Funktionen anstelle von Ausdrücken in der Ansicht verwendet werden. Einschränkungen werden als Argumente für die-Methode des Layouts angegeben `Add` :

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

Beachten Sie die folgenden Aspekte des obigen Layouts:

- Die `x` -und- `y` Einschränkungen werden mit ihren eigenen Einschränkungen angegeben.
- In c# werden relative Einschränkungen als Funktionen definiert. Konzepte wie `Factor` sind nicht vorhanden, können aber manuell implementiert werden.
- Die Koordinaten des Felds `x` wird als halbe Breite des übergeordneten Elements-100 definiert.
- Die Koordinaten des Felds `y` wird als halbe Höhe des übergeordneten Elements (-100) definiert.

> [!NOTE]
> Aufgrund der Art und Weise, wie Einschränkungen definiert werden, ist es möglich, komplexere Layouts in c# zu erstellen, als mit XAML angegeben werden können.

In den beiden obigen Beispielen werden Einschränkungen definiert `RelativeToParent` &ndash; , das heißt, ihre Werte sind relativ zum übergeordneten Element. Es ist auch möglich, Einschränkungen in Bezug auf eine andere Sicht zu definieren. Dies ermöglicht eine intuitivere (für Entwickler-) Layouts und kann die Absicht ihres Layoutcodes leichter erkennbar machen.

Stellen Sie sich ein Layout vor, bei dem ein Element 20 Pixel kleiner als ein anderes sein muss. Wenn beide Elemente mit Konstanten Werten definiert sind, kann die niedrigere `Y` Einschränkung als Konstante definiert werden, die 20 Pixel größer als die `Y` Einschränkung des höheren Elements ist. Diese Vorgehensweise ist kurz, wenn das höhere Element mithilfe eines Anteils positioniert wird, sodass die Pixelgröße nicht bekannt ist. In diesem Fall ist die Beschränkung des Elements auf der Grundlage der Position eines anderen Elements robuster:

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

So erreichen Sie das gleiche Layout in c#:

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

Dadurch wird die folgende Ausgabe erzeugt, wobei die Position des blauen Felds _relativ_ zur Position des roten Felds festgelegt wird:

![](relative-layout-images/red-blue-box.png "RelativeLayout with Red and Blue BoxViews")

### <a name="sizing"></a>Festlegen der Größe

Sichten, die durch angeordnet `RelativeLayout` sind, haben zwei Optionen zum Angeben der Größe:

- `HeightRequest & WidthRequest`
- `RelativeLayout.WidthConstraint` & `RelativeLayout.HeightConstraint`

`HeightRequest`und `WidthRequest` geben die gewünschte Höhe und Breite der Ansicht an, können jedoch bei Bedarf von Layouts überschrieben werden. `WidthConstraint`und `HeightConstraint` unterstützen das Festlegen von Height und Width als Wert relativ zu den Eigenschaften des Layouts oder einer anderen Ansicht bzw. als Konstantenwert.

## <a name="exploring-a-complex-layout"></a>Untersuchen eines komplexen Layouts
Jedes der Layouts hat Stärken und Schwächen zum Erstellen bestimmter Layouts. In dieser Reihe von layoutartikeln wurde eine Beispiel-App mit dem gleichen Seitenlayout erstellt, das mithilfe von drei verschiedenen Layouts implementiert wurde.

Beachten Sie den folgenden XAML-Code:

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
                HeightRequest="50" RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Y, Factor=0, Constant=5}" RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width, Factor=0.5}" />
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

Der obige Code führt zum folgenden Layout:

![](relative-layout-images/relative.png "Complex RelativeLayout")

Beachten Sie, dass `RelativeLayouts` s geschachtelt sind, da in einigen Fällen die Schachtelung von Layouts einfacher sein kann, als alle Elemente innerhalb desselben Layouts darzustellen. Beachten Sie auch, dass einige Elemente sind `RelativeToView` , da dies ein einfacheres und intuitiveres Layout ermöglicht, wenn die Beziehungen zwischen Ansichten die Positionierung steuern.

## <a name="related-links"></a>Verwandte Links

- [Layout (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout)
- [Businesstumm Beispiel (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-businesstumble)
