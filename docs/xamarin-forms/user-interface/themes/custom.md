---
title: Erstellen ein benutzerdefiniertes Design
ms.topic: article
ms.prod: xamarin
ms.assetid: 4FE08ADC-093F-47FA-B33C-20CF08B5D7E0
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/01/2017
ms.openlocfilehash: b981e6a076c8ab35d9c8be43ffd5cdc68b4a59bd
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2018
---
# <a name="creating-a-custom-theme"></a>Erstellen ein benutzerdefiniertes Design

![](~/media/shared/preview.png "Diese API ist derzeit als Vorschau verfügbar")

Zusätzlich zum Hinzufügen von ein Design aus einem NuGet-Paket (z. B. die [Licht](~/xamarin-forms/user-interface/themes/light.md) und [dunkel](~/xamarin-forms/user-interface/themes/dark.md) Designs), Ihre eigenen Ressourcen Wörterbuch Designs erstellen, auf die verwiesen werden, können in Ihrer app.

## <a name="example"></a>Beispiel

Die drei `BoxView`s gezeigt auf die [Designs Seite](~/xamarin-forms/user-interface/themes/index.md) sind entsprechend drei Klassen, die in zwei herunterladbaren Designs definiert formatiert.

Um zu verstehen, wie diese funktionieren, erstellt das folgende Markup eine entsprechende-Formatvorlage, die Sie direkt zum Hinzufügen von konnte Ihre **App.xaml**.

Beachten Sie die `Class` -Attribut für `Style` (im Gegensatz zu der [ `x:Key` ](~/xamarin-forms/user-interface/styles/inheritance.md) Attribut, die in früheren Versionen von Xamarin.Forms verfügbar).

```xml
<ResourceDictionary>
  <!-- DEFINE ANY CONSTANTS -->
  <Color x:Key="SeparatorLineColor">#CCCCCC</Color>
  <Color x:Key="iOSDefaultTintColor">#007aff</Color>
  <Color x:Key="AndroidDefaultAccentColorColor">#1FAECE</Color>
  <OnPlatform x:TypeArguments="Color" x:Key="AccentColor">
    <On Platform="iOS" Value="{StaticResource iOSDefaultTintColor}" />
    <On Platform="Android" Value="{StaticResource AndroidDefaultAccentColorColor}" />
  </OnPlatform>
  <!--  BOXVIEW CLASSES -->
  <Style TargetType="BoxView" Class="HorizontalRule">
    <Setter Property="BackgroundColor" Value="{ StaticResource SeparatorLineColor }" />
    <Setter Property="HeightRequest" Value="1" />
  </Style>

  <Style TargetType="BoxView" Class="Circle">
    <Setter Property="BackgroundColor" Value="{ StaticResource AccentColor }" />
    <Setter Property="WidthRequest" Value="34"/>
    <Setter Property="HeightRequest" Value="34"/>
    <Setter Property="HorizontalOptions" Value="Start" />

    <Setter Property="local:ThemeEffects.Circle" Value="True" />
  </Style>

  <Style TargetType="BoxView" Class="Rounded">
    <Setter Property="BackgroundColor" Value="{ StaticResource AccentColor }" />
    <Setter Property="HorizontalOptions" Value="Start" />
    <Setter Property="BackgroundColor" Value="{ StaticResource AccentColor }" />

    <Setter Property="local:ThemeEffects.CornerRadius" Value="4" />
  </Style>
</ResourceDictionary>
```

Sie werden feststellen, dass die `Rounded` -Klasse verweist auf einen benutzerdefinierten Effekt `CornerRadius`.
Der Code für diesen Effekt erhält-mit eine benutzerdefinierten ordnungsgemäß darauf verweisen `xmlns` muss hinzugefügt werden, um die **App.xaml**des "Root"-Element:

```csharp
xmlns:local="clr-namespace:ThemesDemo;assembly=ThemesDemo"
```

### <a name="c-code-in-the-pcl-or-shared-project"></a>C#-Code in der PCL oder ein freigegebenes Projekt

Der Code zum Erstellen einer Round-Ecke `BoxView` verwendet [Effekte](~/xamarin-forms/app-fundamentals/effects/index.md).
Die Eckradius mithilfe der angewendet wird eine `BindableProperty` und wird implementiert, durch Anwenden einer [wirksam](~/xamarin-forms/app-fundamentals/effects/index.md). Der Effekt erfordert plattformspezifischen Code in der [iOS](#ios) und [Android](#android) Projekte (siehe unten).

```csharp
namemspace ThemesDemo
{
  public static class ThemeEffects
  {
  public static readonly BindableProperty CornerRadiusProperty =
    BindableProperty.CreateAttached("CornerRadius", typeof(double), typeof(ThemeEffects), 0.0, propertyChanged: OnChanged<CornerRadiusEffect, double>);
    private static void OnChanged<TEffect, TProp>(BindableObject bindable, object oldValue, object newValue)
              where TEffect : Effect, new()
    {
        var view = bindable as View;
        if (view == null)
        {
            return;
        }

        if (EqualityComparer<TProp>.Equals(newValue, default(TProp)))
        {
            var toRemove = view.Effects.FirstOrDefault(e => e is TEffect);
            if (toRemove != null)
            {
                view.Effects.Remove(toRemove);
            }
        }
        else
        {
            view.Effects.Add(new TEffect());
        }

    }
    public static void SetCornerRadius(BindableObject view, double radius)
    {
        view.SetValue(CornerRadiusProperty, radius);
    }

    public static double GetCornerRadius(BindableObject view)
    {
        return (double)view.GetValue(CornerRadiusProperty);
    }

    private class CornerRadiusEffect : RoutingEffect
    {
        public CornerRadiusEffect()
            : base("Xamarin.CornerRadiusEffect")
        {
        }
    }
  }
}
```

<a name="ios" />

### <a name="c-code-in-the-ios-project"></a>C#-Code in der iOS-Projekt

```csharp
using System;
using Xamarin.Forms;
using Xamarin.Forms.Platform.iOS;
using CoreGraphics;
using Foundation;
using XFThemes;

namespace ThemesDemo.iOS
{
    public class CornerRadiusEffect : PlatformEffect
    {
        private nfloat _originalRadius;

        protected override void OnAttached()
        {
            if (Container != null)
            {
                _originalRadius = Container.Layer.CornerRadius;
                Container.ClipsToBounds = true;

                UpdateCorner();
            }
        }

        protected override void OnDetached()
        {
            if (Container != null)
            {
                Container.Layer.CornerRadius = _originalRadius;
                Container.ClipsToBounds = false;
            }
        }

        protected override void OnElementPropertyChanged(System.ComponentModel.PropertyChangedEventArgs args)
        {
            base.OnElementPropertyChanged(args);

            if (args.PropertyName == ThemeEffects.CornerRadiusProperty.PropertyName)
            {
                UpdateCorner();
            }
        }

        private void UpdateCorner()
        {
            Container.Layer.CornerRadius = (nfloat)ThemeEffects.GetCornerRadius(Element);
        }
    }
}
```

<a name="android" />

### <a name="c-code-in-the-android-project"></a>C#-Code in der Android-Projekt

```csharp
using System;
using Xamarin.Forms.Platform;
using Xamarin.Forms.Platform.Android;
using Android.Views;
using Android.Graphics;

namespace ThemesDemo.Droid
{
    public class CornerRadiusEffect : BaseEffect
    {
        private ViewOutlineProvider _originalProvider;

        protected override bool CanBeApplied()
        {
            return Container != null && (int)Android.OS.Build.VERSION.SdkInt >= 21;
        }

        protected override void OnAttachedInternal()
        {
            _originalProvider = Container.OutlineProvider;
            Container.OutlineProvider = new CornerRadiusOutlineProvider(Element);
            Container.ClipToOutline = true;
        }

        protected override void OnDetachedInternal()
        {
            Container.OutlineProvider = _originalProvider;
            Container.ClipToOutline = false;
        }

        protected override void OnElementPropertyChanged(System.ComponentModel.PropertyChangedEventArgs args)
        {
            base.OnElementPropertyChanged(args);

            if (!Attached)
            {
                return;
            }

            if (args.PropertyName == ThemeEffects.CornerRadiusProperty.PropertyName)
            {
                Container.Invalidate();
            }
        }

        private class CornerRadiusOutlineProvider : ViewOutlineProvider
        {
            private Xamarin.Forms.Element _element;

            public CornerRadiusOutlineProvider(Xamarin.Forms.Element element)
            {
                _element = element;
            }

            public override void GetOutline(Android.Views.View view, Outline outline)
            {
                var pixles =
                    (float)ThemeEffects.GetCornerRadius(_element) *
                    view.Resources.DisplayMetrics.Density;

                outline.SetRoundRect(new Rect(0, 0, view.Width, view.Height), (int)pixles);
            }
        }
    }
}
```


## <a name="summary"></a>Zusammenfassung

Durch definieren Stile für jedes Steuerelement, das benutzerdefinierten Darstellung erfordert, kann ein benutzerdefiniertes Design erstellt werden. Mehrere Formate für ein Steuerelement sollte von verschiedenen unterschieden werden `Class` im Ressourcenverzeichnis Attribute und dann angewendet, indem die `StyleClass` Attribut für das Steuerelement.

Ein Format kann auch nutzen [Effekte](~/xamarin-forms/app-fundamentals/effects/index.md) weiter anpassen, die Darstellung eines Steuerelements.

[Impliziten Stilen](~/xamarin-forms/user-interface/styles/implicit.md) (ohne eine `x:Key` oder `Style` Attribut) weiterhin auf alle Steuerelemente angewendet werden, die entsprechen den `TargetType`.
