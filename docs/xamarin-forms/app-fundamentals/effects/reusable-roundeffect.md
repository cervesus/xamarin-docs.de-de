---
title: Wiederverwendbarer RoundEffect in Xamarin.Forms
description: RoundEffect ist ein wiederverwendbarer Effekt, der auf jedes aus VisualElement stammende Steuerelement angewendet werden kann, um das Steuerelement als Kreis zu rendern.
ms.prod: xamarin
ms.assetid: B5DE7507-B565-4EE5-9897-27E5733FD173
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 10/25/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: fc3776934a4c109b2527132b11c6c6a93b7d9f9e
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84138852"
---
# <a name="xamarinforms-reusable-roundeffect"></a>Wiederverwendbarer RoundEffect in Xamarin.Forms

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/effects-roundeffect/)

RoundEffect vereinfacht das Rendering jedes Steuerelements, das von VisualElement als Kreis abgeleitet wird. Dieser Effekt kann verwendet werden, um kreisförmige Bilder, Schaltflächen oder andere Steuerelemente zu erstellen:

[![Screenshots von RoundEffect in iOS und Android](example-roundeffect-images/round-effect-cropped.png)](example-roundeffect-images/round-effect.png#lightbox)

## <a name="create-a-shared-routingeffect"></a>Erstellen einer freigegebenen RoutingEffect-Klasse

Im freigegebenen Projekt muss eine Effektklasse angelegt werden, um einen plattformübergreifenden Effekt zu erzeugen. Die Beispielanwendung erstellt eine leere `RoundEffect`-Klasse, die sich von der Klasse `RoutingEffect` ableitet:

```csharp
public class RoundEffect : RoutingEffect
{
    public RoundEffect() : base($"Xamarin.{nameof(RoundEffect)}")
    {
    }
}
```

Diese Klasse ermöglicht es dem freigegebenen Projekt, die Referenzen auf den Effekt im Code oder XAML aufzulösen, bietet aber keine Funktionalität. Der Effekt muss für jede Plattform Implementierungen enthalten.

## <a name="implement-the-android-effect"></a>Implementieren des Android-Effekts

Das Android-Plattformprojekt definiert eine Klasse `RoundEffect`, die sich von `PlatformEffect` ableitet. Diese Klasse ist mit `assembly`-Attributen versehen, die es Xamarin.Forms ermöglichen, die Effektklasse aufzulösen:

```csharp
[assembly: ResolutionGroupName("Xamarin")]
[assembly: ExportEffect(typeof(RoundEffectDemo.Droid.RoundEffect), nameof(RoundEffectDemo.Droid.RoundEffect))]
namespace RoundEffectDemo.Droid
{
    public class RoundEffect : PlatformEffect
    {
        // ...
    }
}
```

Die Android-Plattform verwendet das Konzept eines `OutlineProvider`, um die Edges eines Steuerelements zu definieren. Das Beispielprojekt beinhaltet eine `CornerRadiusProvider`-Klasse, die sich von der `ViewOutlineProvider`-Klasse ableitet:

```csharp
class CornerRadiusOutlineProvider : ViewOutlineProvider
{
    Element element;

    public CornerRadiusOutlineProvider(Element formsElement)
    {
        element = formsElement;
    }

    public override void GetOutline(Android.Views.View view, Outline outline)
    {
        float scale = view.Resources.DisplayMetrics.Density;
        double width = (double)element.GetValue(VisualElement.WidthProperty) * scale;
        double height = (double)element.GetValue(VisualElement.HeightProperty) * scale;
        float minDimension = (float)Math.Min(height, width);
        float radius = minDimension / 2f;
        Rect rect = new Rect(0, 0, (int)width, (int)height);
        outline.SetRoundRect(rect, radius);
    }
}
```

Diese Klasse verwendet die Eigenschaften `Width` und `Height` der Instanz `Element` von Xamarin.Forms, um einen Radius zu berechnen, der die Hälfte der kürzesten Dimension beträgt.

Sobald eine OutlineProvider-Eigenschaft definiert ist, kann die Klasse `RoundEffect` ihn nutzen, um den Effekt zu implementieren:

```csharp
public class RoundEffect : PlatformEffect
{
    ViewOutlineProvider originalProvider;
    Android.Views.View effectTarget;

    protected override void OnAttached()
    {
        try
        {
            effectTarget = Control ?? Container;
            originalProvider = effectTarget.OutlineProvider;
            effectTarget.OutlineProvider = new CornerRadiusOutlineProvider(Element);
            effectTarget.ClipToOutline = true;
        }
        catch (Exception ex)
        {
            Console.WriteLine($"Failed to set corner radius: {ex.Message}");
        }
    }

    protected override void OnDetached()
    {
        if(effectTarget != null)
        {
            effectTarget.OutlineProvider = originalProvider;
            effectTarget.ClipToOutline = false;
        }
    }
}
```

Die `OnAttached`-Methode wird aufgerufen, wenn der Effekt an ein Element angehängt ist. Das vorhandene `OutlineProvider`-Objekt wird gespeichert, sodass es wiederhergestellt werden kann, wenn der Effekt entfernt wird. Eine neue Instanz von `CornerRadiusOutlineProvider` wird verwendet, wenn `OutlineProvider` und `ClipToOutline` auf „true“ festgelegt sind, um überfließende Elemente an den Umrandungen auszuschneiden.

Die `OnDetatched`-Methode wird aufgerufen, wenn der Effekt aus einem Element entfernt wird und den ursprünglichen `OutlineProvider`-Wert wiederherstellt.

> [!NOTE]
> Abhängig vom Elementtyp kann die Eigenschaft `Control` NULL oder nicht NULL sein. Wenn die Eigenschaft `Control` nicht NULL ist, können die abgerundeten Ecken direkt auf das Steuerelement angewendet werden. Wenn sie jedoch NULL ist, müssen die abgerundeten Ecken auf das Objekt `Container` angewendet werden. Das Feld `effectTarget` ermöglicht es, den Effekt auf das entsprechende Objekt anzuwenden.

## <a name="implement-the-ios-effect"></a>Implementieren des iOS-Effekts

Das iOS-Plattformprojekt definiert eine Klasse `RoundEffect`, die sich von `PlatformEffect` ableitet. Diese Klasse ist mit `assembly`-Attributen versehen, die es Xamarin.Forms ermöglichen, die Effektklasse aufzulösen:

```csharp
[assembly: ResolutionGroupName("Xamarin")]
[assembly: ExportEffect(typeof(RoundEffectDemo.iOS.RoundEffect), nameof(RoundEffectDemo.iOS.RoundEffect))]
namespace RoundEffectDemo.iOS
{
    public class RoundEffect : PlatformEffect
    {
        // ...
    }
```

Unter iOS haben Steuerelemente eine `Layer`-Eigenschaft, die eine `CornerRadius`-Eigenschaft enthält. Die Klassenimplementierung von `RoundEffect` unter iOS berechnet den entsprechenden Eckenradius und aktualisiert die Eigenschaft `CornerRadius` der Ebene:

```csharp
public class RoundEffect : PlatformEffect
{
    nfloat originalRadius;
    UIKit.UIView effectTarget;

    protected override void OnAttached()
    {
        try
        {
            effectTarget = Control ?? Container;
            originalRadius = effectTarget.Layer.CornerRadius;
            effectTarget.ClipsToBounds = true;
            effectTarget.Layer.CornerRadius = CalculateRadius();
        }
        catch (Exception ex)
        {
            Console.WriteLine($"Failed to set corner radius: {ex.Message}");
        }
    }

    protected override void OnDetached()
    {
        if (effectTarget != null)
        {
            effectTarget.ClipsToBounds = false;
            if (effectTarget.Layer != null)
            {
                effectTarget.Layer.CornerRadius = originalRadius;
            }
        }
    }

    float CalculateRadius()
    {
        double width = (double)Element.GetValue(VisualElement.WidthRequestProperty);
        double height = (double)Element.GetValue(VisualElement.HeightRequestProperty);
        float minDimension = (float)Math.Min(height, width);
        float radius = minDimension / 2f;

        return radius;
    }
}
```

Die `CalculateRadius`-Methode berechnet einen Radius basierend auf der Mindestdimension der Xamarin.Forms `Element`. Die `OnAttached`-Methode wird aufgerufen, wenn der Effekt an ein Steuerelement angehängt ist, und aktualisiert die `CornerRadius`-Eigenschaft der Ebene. Sie legt die `ClipToBounds`-Eigenschaft auf `true` fest, sodass überfließende Elemente an den Grenzen des Steuerelements abgeschnitten werden. Die `OnDetatched`-Methode wird aufgerufen, wenn der Effekt aus einem Steuerelement entfernt wird und diese Änderungen rückgängig macht, und den ursprünglichen Eckenradius wiederherstellt.

> [!NOTE]
> Abhängig vom Elementtyp kann die Eigenschaft `Control` NULL oder nicht NULL sein. Wenn die Eigenschaft `Control` nicht NULL ist, können die abgerundeten Ecken direkt auf das Steuerelement angewendet werden. Wenn sie jedoch NULL ist, müssen die abgerundeten Ecken auf das Objekt `Container` angewendet werden. Das Feld `effectTarget` ermöglicht es, den Effekt auf das entsprechende Objekt anzuwenden.

## <a name="consume-the-effect"></a>Nutzung des Effekts

Sobald der Effekt plattformübergreifend implementiert ist, kann er von Xamarin.Forms-Steuerelementen genutzt werden. Eine gängige Anwendung von `RoundEffect` besteht darin, ein `Image`-Objekt kreisförmig zu machen. Die folgende XAML zeigt den Effekt, der auf eine `Image`-Instanz angewendet wird:

```xaml
<Image Source=outdoors"
       HeightRequest="100"
       WidthRequest="100">
    <Image.Effects>
        <local:RoundEffect />
    </Image.Effects>
</Image>
```

Der Effekt kann auch in Code angewendet werden:

```csharp
var image = new Image
{
    Source = ImageSource.FromFile("outdoors"),
    HeightRequest = 100,
    WidthRequest = 100
};
image.Effects.Add(new RoundEffect());
```

Die Klasse `RoundEffect` kann auf jedes Steuerelement angewendet werden, das sich von `VisualElement` ableitet.

> [!NOTE]
> Damit der Effekt den richtigen Radius berechnen kann, muss das Steuerelement, auf das er angewendet wird, eine explizite Größenanpassung aufweisen. Daher sollten die Eigenschaften `HeightRequest` und `WidthRequest` definiert werden. Wenn das betroffene Steuerelement in einem `StackLayout` erscheint, sollte seine `HorizontalOptions`-Eigenschaft keinen der **Expand**-Werte (Erweitern) wie `LayoutOptions.CenterAndExpand` verwenden, sonst hat es keine genauen Dimensionen.

## <a name="related-links"></a>Verwandte Links

- [RoundEffect Beispielanwendung](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/effects-roundeffect/)
- [Einführung in Effekte](~/xamarin-forms/app-fundamentals/effects/introduction.md)
- [Erstellen eines Effekts](~/xamarin-forms/app-fundamentals/effects/creating.md)
