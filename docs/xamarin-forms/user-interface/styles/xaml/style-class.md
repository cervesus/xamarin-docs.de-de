---
title: Xamarin.Forms-Formatklassen
description: Xamarin.FormsStil Klassen ermöglichen das Anwenden mehrerer Stile auf ein Steuerelement, ohne dass auf eine Format Vererbung zurückgegriffen wird.
ms.prod: xamarin
ms.assetid: 4762401E-2B48-48F1-B6E4-61F7AF8AA46F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/30/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 2afb06c2d97e6f15c2041b9c2e9cad092b13d90d
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84138761"
---
# <a name="xamarinforms-style-classes"></a>Xamarin.Forms-Formatklassen

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-basicstyles)

_Xamarin. Forms-Klassen Klassen ermöglichen das Anwenden mehrerer Stile auf ein Steuerelement, ohne dass auf eine Formatierungs Vererbung zurückgegriffen wird._

## <a name="create-style-classes"></a>Erstellen von Stil Klassen

Eine Style-Klasse kann erstellt werden, indem die-Eigenschaft eines auf einen festgelegt wird [`Class`](xref:Xamarin.Forms.Style.Class) [`Style`](xref:Xamarin.Forms.Style) `string` , der den Klassennamen darstellt. Der Vorteil, den diese bietet, wenn Sie einen expliziten Stil mithilfe des- `x:Key` Attributs definieren, besteht darin, dass mehrere Formatklassen auf einen angewendet werden können [`VisualElement`](xref:Xamarin.Forms.VisualElement) .

> [!IMPORTANT]
> Mehrere Stile können denselben Klassennamen verwenden, sofern Sie unterschiedliche Typen als Ziel haben. Dies ermöglicht es, dass mehrere Formatklassen, die identisch benannt sind, verschiedene Typen als Ziel haben.

Das folgende Beispiel zeigt drei [`BoxView`](xref:Xamarin.Forms.BoxView) Stil Klassen und eine Format [`VisualElement`](xref:Xamarin.Forms.VisualElement) Klasse:

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <Style TargetType="BoxView"
               Class="Separator">
            <Setter Property="BackgroundColor"
                    Value="#CCCCCC" />
            <Setter Property="HeightRequest"
                    Value="1" />
        </Style>

        <Style TargetType="BoxView"
               Class="Rounded">
            <Setter Property="BackgroundColor"
                    Value="#1FAECE" />
            <Setter Property="HorizontalOptions"
                    Value="Start" />
            <Setter Property="CornerRadius"
                    Value="10" />
        </Style>    

        <Style TargetType="BoxView"
               Class="Circle">
            <Setter Property="BackgroundColor"
                    Value="#1FAECE" />
            <Setter Property="WidthRequest"
                    Value="100" />
            <Setter Property="HeightRequest"
                    Value="100" />
            <Setter Property="HorizontalOptions"
                    Value="Start" />
            <Setter Property="CornerRadius"
                    Value="50" />
        </Style>

        <Style TargetType="VisualElement"
               Class="Rotated"
               ApplyToDerivedTypes="true">
            <Setter Property="Rotation"
                    Value="45" />
        </Style>        
    </ContentPage.Resources>
</ContentPage>
```

Die `Separator` `Rounded` `Circle` Stil Klassen, und legen jeweils [`BoxView`](xref:Xamarin.Forms.BoxView) Eigenschaften auf bestimmte Werte fest.

Die `Rotated` Style-Klasse verfügt über eine [`TargetType`](xref:Xamarin.Forms.Style.TargetType) von [`VisualElement`](xref:Xamarin.Forms.VisualElement) , was bedeutet, dass Sie nur auf-Instanzen angewendet werden kann `VisualElement` . Die- [`ApplyToDerivedTypes`](xref:Xamarin.Forms.Style.ApplyToDerivedTypes) Eigenschaft ist jedoch auf festgelegt `true` , wodurch sichergestellt wird, dass Sie auf alle Steuerelemente angewendet werden kann, die von abgeleitet werden `VisualElement` , z [`BoxView`](xref:Xamarin.Forms.BoxView) . b.. Weitere Informationen zum Anwenden eines Stils auf einen abgeleiteten Typ finden Sie unter [Anwenden eines Stils auf abgeleitete Typen](implicit.md#apply-a-style-to-derived-types).

Der entsprechende C#-Code lautet:

```csharp
var separatorBoxViewStyle = new Style(typeof(BoxView))
{
    Class = "Separator",
    Setters =
    {
        new Setter
        {
            Property = VisualElement.BackgroundColorProperty,
            Value = Color.FromHex("#CCCCCC")
        },
        new Setter
        {
            Property = VisualElement.HeightRequestProperty,
            Value = 1
        }
    }
};

var roundedBoxViewStyle = new Style(typeof(BoxView))
{
    Class = "Rounded",
    Setters =
    {
        new Setter
        {
            Property = VisualElement.BackgroundColorProperty,
            Value = Color.FromHex("#1FAECE")
        },
        new Setter
        {
            Property = View.HorizontalOptionsProperty,
            Value = LayoutOptions.Start
        },
        new Setter
        {
            Property = BoxView.CornerRadiusProperty,
            Value = 10
        }
    }
};

var circleBoxViewStyle = new Style(typeof(BoxView))
{
    Class = "Circle",
    Setters =
    {
        new Setter
        {
            Property = VisualElement.BackgroundColorProperty,
            Value = Color.FromHex("#1FAECE")
        },
        new Setter
        {
            Property = VisualElement.WidthRequestProperty,
            Value = 100
        },
        new Setter
        {
            Property = VisualElement.HeightRequestProperty,
            Value = 100
        },
        new Setter
        {
            Property = View.HorizontalOptionsProperty,
            Value = LayoutOptions.Start
        },
        new Setter
        {
            Property = BoxView.CornerRadiusProperty,
            Value = 50
        }
    }
};

var rotatedVisualElementStyle = new Style(typeof(VisualElement))
{
    Class = "Rotated",
    ApplyToDerivedTypes = true,
    Setters =
    {
        new Setter
        {
            Property = VisualElement.RotationProperty,
            Value = 45
        }
    }
};

Resources = new ResourceDictionary
{
    separatorBoxViewStyle,
    roundedBoxViewStyle,
    circleBoxViewStyle,
    rotatedVisualElementStyle
};
```

## <a name="consume-style-classes"></a>Stil Klassen verwenden

Stil Klassen können verwendet werden, indem die- [`StyleClass`](xref:Xamarin.Forms.NavigableElement.StyleClass) Eigenschaft des-Steuer Elements, die vom Typ ist `IList<string>` , auf eine Liste von Formatklassen Namen festgelegt wird. Die Formatklassen werden angewendet, vorausgesetzt, dass der Typ des Steuer Elements [`TargetType`](xref:Xamarin.Forms.Style.TargetType) mit dem der Stil Klassen übereinstimmt.

Im folgenden Beispiel werden drei- [`BoxView`](xref:Xamarin.Forms.BoxView) Instanzen gezeigt, die jeweils auf verschiedene Formatklassen festgelegt sind:

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        ...
    </ContentPage.Resources>
    <StackLayout Margin="20">
        <BoxView StyleClass="Separator" />       
        <BoxView WidthRequest="100"
                 HeightRequest="100"
                 HorizontalOptions="Center"
                 StyleClass="Rounded, Rotated" />
        <BoxView HorizontalOptions="Center"
                 StyleClass="Circle" />
    </StackLayout>
</ContentPage>    
```

In diesem Beispiel wird das erste [`BoxView`](xref:Xamarin.Forms.BoxView) als Zeilen Trennzeichen formatiert, während das dritte `BoxView` zirkulär ist. Die zweite `BoxView` Klasse verfügt über zwei auf Sie angewendete Formatklassen, die ihr gerundete Ecken und um 45 Grad drehen:

![Boxviews mit Stil Klassen formatieren](style-class-images/boxviews.png)

> [!IMPORTANT]
> Mehrere Formatklassen können auf ein-Steuerelement angewendet werden, da die- [`StyleClass`](xref:Xamarin.Forms.NavigableElement.StyleClass) Eigenschaft vom Typ ist `IList<string>` . Wenn dies auftritt, werden Stil Klassen in aufsteigender Listen Reihenfolge angewendet. Wenn also mehrere Formatklassen identische Eigenschaften festlegen, hat die Eigenschaft in der Style-Klasse, die sich in der höchsten Listenposition befindet, Vorrang.

Der entsprechende C#-Code lautet:

```csharp
...
Content = new StackLayout
{
    Children =
    {
        new BoxView { StyleClass = new [] { "Separator" } },
        new BoxView { WidthRequest = 100, HeightRequest = 100, HorizontalOptions = LayoutOptions.Center, StyleClass = new [] { "Rounded", "Rotated" } },
        new BoxView { HorizontalOptions = LayoutOptions.Center, StyleClass = new [] { "Circle" } }
    }
};
```

## <a name="related-links"></a>Verwandte Links

- [Grundlegende Stile (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-basicstyles)
