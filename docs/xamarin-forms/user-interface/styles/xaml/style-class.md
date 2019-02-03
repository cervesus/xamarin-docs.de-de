---
title: Xamarin.Forms-Formatklassen
description: Xamarin.Forms-Formatklassen aktivieren mehrere Formate, die an ein Steuerelement angewendet werden, ohne auf stilvererbung zurückzugreifen.
ms.prod: xamarin
ms.assetid: 4762401E-2B48-48F1-B6E4-61F7AF8AA46F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/30/2019
ms.openlocfilehash: dd749a4a78adbab5317f1ae5ca6334caa009b9b3
ms.sourcegitcommit: 9dcb7377dc92ad921285fbb857b0be13030bbea3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/03/2019
ms.locfileid: "55668549"
---
# <a name="xamarinforms-style-classes"></a>Xamarin.Forms-Formatklassen

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/BasicStyles/)

_Xamarin.Forms-Formatklassen aktivieren mehrere Formate, die an ein Steuerelement angewendet werden, ohne auf stilvererbung zurückzugreifen._

## <a name="create-style-classes"></a>Style-Klassen erstellen

Eine Formatklasse erstellt werden, indem Sie die Einstellung der [ `Class` ](xref:Xamarin.Forms.Style.Class) Eigenschaft für eine [ `Style` ](xref:Xamarin.Forms.Style) auf eine `string` , die den Namen der Klasse darstellt. Der Vorteil dieser bietet, über das Definieren einer expliziter Stil mithilfe der `x:Key` Attribut, besteht darin, dass mehrere Klassen der Stil angewendet werden können eine [ `VisualElement` ](xref:Xamarin.Forms.VisualElement).

> [!IMPORTANT]
> Mehrere Formate können denselben Klassennamen, freigeben, vorausgesetzt sie verschiedene Arten ausgerichtet. Dadurch können mehrere Stil Klassen, die identisch, auf anderen Zieltypen benannt sind.

Das folgende Beispiel zeigt drei [ `BoxView` ](xref:Xamarin.Forms.BoxView) Klassen, formatieren und ein [ `VisualElement` ](xref:Xamarin.Forms.VisualElement) -Formatklasse:

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

Die `Separator`, `Rounded`, und `Circle` Stil Klassen jeden Satz [ `BoxView` ](xref:Xamarin.Forms.BoxView) Eigenschaften mit bestimmten Werten.

Die `Rotated` Style-Klasse verfügt über eine [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType) von [ `VisualElement` ](xref:Xamarin.Forms.VisualElement), was bedeutet, dass kann nur auf angewendet `VisualElement` Instanzen. Allerdings die [ `ApplyToDerivedTypes` ](xref:Xamarin.Forms.Style.ApplyToDerivedTypes) -Eigenschaftensatz auf `true`, die sicherstellt, dass es auf alle Steuerelemente angewendet werden kann, die von abgeleitet `VisualElement`, z. B. [ `BoxView` ](xref:Xamarin.Forms.BoxView). Weitere Informationen zum Anwenden einer Formatvorlage auf ein abgeleiteter Typ finden Sie unter [richtigen Stil für die abgeleiteten Typen](implicit.md#apply-a-style-to-derived-types).

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

## <a name="consume-style-classes"></a>Style-Klassen nutzen

Formatklassen können genutzt werden, durch Festlegen der [ `StyleClass` ](xref:Xamarin.Forms.VisualElement.StyleClass) Eigenschaft des Steuerelements, das vom Typ `IList<string>`, um eine Liste der Style-Klassennamen. Die Style-Klassen ausgeglichen werden, vorausgesetzt, dass der Typ des Steuerelements entspricht der [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType) der Style-Klassen.

Das folgende Beispiel zeigt drei [ `BoxView` ](xref:Xamarin.Forms.BoxView) Instanzen jeweils anderen Stil Klassen festgelegt:

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

In diesem Beispiel ist die erste [ `BoxView` ](xref:Xamarin.Forms.BoxView) formatiert wird, um ein Zeilentrennzeichen, während das dritte `BoxView` ist zirkulär. Die zweite `BoxView` verfügt über zwei Klassen von Style darauf angewendet, die erhalten It abgerundete Ecken und drehen ihn um 45 Grad:

![](style-class-images/boxviews.png "BoxViews erstellt, in denen Klassen style")

> [!IMPORTANT]
> Mehrere Klassen von Style können auf ein Steuerelement angewendet werden, da die [ `StyleClass` ](xref:Xamarin.Forms.VisualElement.StyleClass) Eigenschaft ist vom Typ `IList<string>`. In diesem Fall werden Formatklassen in aufsteigender Listenreihenfolge angewendet. Aus diesem Grund, wenn mehrere Formatklassen identische Eigenschaften festgelegt, hat die Eigenschaft in der Style-Klasse, die die höchsten Listenposition wird Vorrang.

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

- [Einfache Stile (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/BasicStyles/)
