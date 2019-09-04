---
title: Klassen im xamarin. Forms-Stil
description: Xamarin. Forms-Klassen Klassen ermöglichen das Anwenden mehrerer Stile auf ein Steuerelement, ohne dass auf eine Formatierungs Vererbung zurückgegriffen wird.
ms.prod: xamarin
ms.assetid: 4762401E-2B48-48F1-B6E4-61F7AF8AA46F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/30/2019
ms.openlocfilehash: 4a353d64f0e7e29da6c64f93b8554c3661f4d389
ms.sourcegitcommit: c9651cad80c2865bc628349d30e82721c01ddb4a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/03/2019
ms.locfileid: "70228130"
---
# <a name="xamarinforms-style-classes"></a>Klassen im xamarin. Forms-Stil

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-basicstyles)

_Xamarin. Forms-Klassen Klassen ermöglichen das Anwenden mehrerer Stile auf ein Steuerelement, ohne dass auf eine Formatierungs Vererbung zurückgegriffen wird._

## <a name="create-style-classes"></a>Erstellen von Stil Klassen

Eine Style-Klasse kann erstellt werden, indem [`Class`](xref:Xamarin.Forms.Style.Class) die-Eigenschaft [`Style`](xref:Xamarin.Forms.Style) eines auf `string` einen festgelegt wird, der den Klassennamen darstellt. Der Vorteil, den diese bietet, wenn Sie einen expliziten `x:Key` Stil mithilfe des-Attributs definieren, besteht darin, dass [`VisualElement`](xref:Xamarin.Forms.VisualElement)mehrere Formatklassen auf einen angewendet werden können.

> [!IMPORTANT]
> Mehrere Stile können denselben Klassennamen verwenden, sofern Sie unterschiedliche Typen als Ziel haben. Dies ermöglicht es, dass mehrere Formatklassen, die identisch benannt sind, verschiedene Typen als Ziel haben.

Das folgende Beispiel zeigt drei [`BoxView`](xref:Xamarin.Forms.BoxView) Stil Klassen und eine [`VisualElement`](xref:Xamarin.Forms.VisualElement) Formatklasse:

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

Die `Separator`Stil `Rounded`Klassen, `Circle` und legen jeweils Eigenschaften [`BoxView`](xref:Xamarin.Forms.BoxView) auf bestimmte Werte fest.

Die `Rotated` Style-Klasse verfügt [`TargetType`](xref:Xamarin.Forms.Style.TargetType) über [`VisualElement`](xref:Xamarin.Forms.VisualElement)eine von, was bedeutet, dass Sie nur `VisualElement` auf-Instanzen angewendet werden kann. Die- `true` `VisualElement`Eigenschaft ist jedoch auf festgelegt, wodurch sichergestellt wird, dass Sie auf alle Steuerelemente angewendet werden kann, [`BoxView`](xref:Xamarin.Forms.BoxView)die von abgeleitet werden, z. b. [`ApplyToDerivedTypes`](xref:Xamarin.Forms.Style.ApplyToDerivedTypes) Weitere Informationen zum Anwenden eines Stils auf einen abgeleiteten Typ finden Sie unter [Anwenden eines Stils auf abgeleitete Typen](implicit.md#apply-a-style-to-derived-types).

Der entsprechende C#-Code ist:

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

Stil Klassen können verwendet werden, indem die [`StyleClass`](xref:Xamarin.Forms.NavigableElement.StyleClass) -Eigenschaft des-Steuer Elements, die vom `IList<string>`Typ ist, auf eine Liste von Formatklassen Namen festgelegt wird. Die Formatklassen werden angewendet, vorausgesetzt, dass der Typ des Steuer Elements mit [`TargetType`](xref:Xamarin.Forms.Style.TargetType) dem der Stil Klassen übereinstimmt.

Im folgenden Beispiel werden drei [`BoxView`](xref:Xamarin.Forms.BoxView) -Instanzen gezeigt, die jeweils auf verschiedene Formatklassen festgelegt sind:

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
> Mehrere Formatklassen können auf ein-Steuerelement angewendet werden [`StyleClass`](xref:Xamarin.Forms.NavigableElement.StyleClass) , da die- `IList<string>`Eigenschaft vom Typ ist. Wenn dies auftritt, werden Stil Klassen in aufsteigender Listen Reihenfolge angewendet. Wenn also mehrere Formatklassen identische Eigenschaften festlegen, hat die Eigenschaft in der Style-Klasse, die sich in der höchsten Listenposition befindet, Vorrang.

Der entsprechende C#-Code ist:

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

- [Einfache Stile (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-basicstyles)
