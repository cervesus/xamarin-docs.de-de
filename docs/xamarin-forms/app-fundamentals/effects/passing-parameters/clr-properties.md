---
title: Übergeben von Parametern Auswirkungen als Common Language Runtime-Eigenschaften
description: Common Language Runtime (CLR)-Eigenschaften können Auswirkungen Parameter zu definieren, die auf Änderungen zur Laufzeit reagieren nicht verwendet werden. Dieser Artikel beschreibt mittels CLR-Eigenschaften zum Übergeben von Parametern mit einem Ergebnis.
ms.prod: xamarin
ms.assetid: 4B50466C-5DBD-45DD-B1E6-BE9524C92F27
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/05/2016
ms.openlocfilehash: 78d14b9764ab0c7cafb9f09fa1c8acea3f45afde
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="passing-effect-parameters-as-common-language-runtime-properties"></a>Übergeben von Parametern Auswirkungen als Common Language Runtime-Eigenschaften

_Common Language Runtime (CLR)-Eigenschaften können Auswirkungen Parameter zu definieren, die auf Änderungen zur Laufzeit reagieren nicht verwendet werden. Dieser Artikel beschreibt mittels CLR-Eigenschaften zum Übergeben von Parametern mit einem Ergebnis._

Der Prozess zum Erstellen von Effekt, auf die Änderungen zur Laufzeit reagiert nicht lautet wie folgt:

1. Erstellen einer `public` Klasse diese Unterklassen der [ `RoutingEffect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.RoutingEffect/) Klasse. Die `RoutingEffect` Klasse stellt einen plattformunabhängigen Effekt, der einen inneren Effekt umschließt, die in der Regel plattformspezifischen ist.
1. Erstellen Sie einen Konstruktor, der Aufrufe der Basisklasse-Konstruktors und übergibt eine Verkettung der Name der Lösung und die eindeutige ID, die für jede Clientplattform-spezifische Auswirkungen Klasse angegeben wurde.
1. Fügen Sie Eigenschaften der Klasse für jeden Parameter der Effekt übergeben werden soll.

Parameter können dann die auszuführenden übergeben werden, indem Sie Werte für jede Eigenschaft angeben, bei der Instanziierung des Effekts.

Die beispielanwendung für veranschaulicht eine `ShadowEffect` , der vom angezeigte Text einen Schatten hinzugefügt eine [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) Steuerelement. Das folgende Diagramm veranschaulicht die Zuständigkeiten aller Projekte in der beispielanwendung, sowie die Beziehungen zwischen ihnen:

![](clr-properties-images/shadow-effect.png "Schatten Auswirkungen Projekt Zuständigkeiten")

Ein [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) control für die `HomePage` angepasst wird, indem die `LabelShadowEffect` in jedem Projekt plattformspezifischen. Parameter werden für jede übergeben `LabelShadowEffect` über Eigenschaften in der `ShadowEffect` Klasse. Jede `LabelShadowEffect` Klasse leitet sich von der `PlatformEffect` Klasse für jede Plattform. Dies führt zu einem Schatten angezeigten Elementtexts hinzugefügt wird die `Label` zu steuern, wie in den folgenden Screenshots dargestellt:

![](clr-properties-images/screenshots.png "Schatteneffekt auf jeder Plattform")

## <a name="creating-effect-parameters"></a>Erstellen von Effekt-Parametern

Ein `public` Klasse diese Unterklassen der [ `RoutingEffect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.RoutingEffect/) Klasse sollte zur Darstellung von Effekt-Parameter erstellt werden, wie im folgenden Codebeispiel wird veranschaulicht:

```csharp
public class ShadowEffect : RoutingEffect
{
  public float Radius { get; set; }

  public Color Color { get; set; }

  public float DistanceX { get; set; }

  public float DistanceY { get; set; }

  public ShadowEffect () : base ("MyCompany.LabelShadowEffect")
  {         
  }
}
```

Die `ShadowEffect` enthält vier Eigenschaften, die an jede Clientplattform-spezifische zu übergebenden Parameter darstellen `LabelShadowEffect`. Der Klassenkonstruktor ruft den Basisklassenkonstruktor übergeben eines Parameters, bestehend aus einer Verkettung der Name der Lösung und die eindeutige ID, die für jede Clientplattform-spezifische Auswirkungen Klasse angegeben wurde. Daher eine neue Instanz der der `MyCompany.LabelShadowEffect` werden hinzugefügt werden, um ein Steuerelement [ `Effects` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.Effects/) Auflistung beim eine `ShadowEffect` instanziiert wird.

## <a name="consuming-the-effect"></a>Nutzen die Auswirkung

Das folgende Beispiel zeigt für die Verwendung von XAML-Code eine [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) Steuerelement, das `ShadowEffect` angefügt ist:

```xaml
<Label Text="Label Shadow Effect" ...>
  <Label.Effects>
    <local:ShadowEffect Radius="5" DistanceX="5" DistanceY="5">
      <local:ShadowEffect.Color>
        <OnPlatform x:TypeArguments="Color">
            <On Platform="iOS" Value="Black" />
            <On Platform="Android" Value="White" />
            <On Platform="UWP" Value="Red" />
        </OnPlatform>
      </local:ShadowEffect.Color>
    </local:ShadowEffect>
  </Label.Effects>
</Label>
```

Die Entsprechung [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) in c# ist im folgenden Codebeispiel gezeigt:

```csharp
var label = new Label {
  Text = "Label Shadow Effect",
  ...
};

Color color = Color.Default;
switch (Device.RuntimePlatform)
{
    case Device.iOS:
        color = Color.Black;
        break;
    case Device.Android:
        color = Color.White;
        break;
    case Device.UWP:
        color = Color.Red;
        break;
}

label.Effects.Add (new ShadowEffect {
  Radius = 5,
  Color = color,
  DistanceX = 5,
  DistanceY = 5
});
```

In beiden Codebeispielen eine Instanz von der `ShadowEffect` Klasse instanziiert mit Werten für jede Eigenschaft angegeben wird, bevor Sie dem Steuerelement hinzugefügt werden [ `Effects` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.Effects/) Auflistung. Beachten Sie, dass die `ShadowEffect.Color` Eigenschaft verwendet plattformspezifischen Farbwerte anzeigt. Weitere Informationen finden Sie unter [Geräteklasse](~/xamarin-forms/platform/device.md).

## <a name="creating-the-effect-on-each-platform"></a>Erstellen die Auswirkungen auf jeder Plattform

Die folgenden Abschnitte beschreiben die Clientplattform-spezifische Implementierung der `LabelShadowEffect` Klasse.

### <a name="ios-project"></a>iOS Project

Das folgende Codebeispiel zeigt die `LabelShadowEffect` Implementierung für das iOS-Projekt:

```csharp
[assembly:ResolutionGroupName ("MyCompany")]
[assembly:ExportEffect (typeof(LabelShadowEffect), "LabelShadowEffect")]
namespace EffectsDemo.iOS
{
    public class LabelShadowEffect : PlatformEffect
    {
        protected override void OnAttached ()
        {
            try {
                var effect = (ShadowEffect)Element.Effects.FirstOrDefault (e => e is ShadowEffect);
                if (effect != null) {
                    Control.Layer.CornerRadius = effect.Radius;
                    Control.Layer.ShadowColor = effect.Color.ToCGColor ();
                    Control.Layer.ShadowOffset = new CGSize (effect.DistanceX, effect.DistanceY);
                    Control.Layer.ShadowOpacity = 1.0f;
                }
            } catch (Exception ex) {
                Console.WriteLine ("Cannot set property on attached control. Error: ", ex.Message);
            }
        }

        protected override void OnDetached ()
        {
        }
    }
}
```

Die `OnAttached` Methode ruft die `ShadowEffect` Instanz und legt `Control.Layer` Eigenschaften auf den angegebenen Eigenschaftswerten den Schatten zu erstellen. Diese Funktion umschlossen ist ein `try` / `catch` blockieren, für den Fall, dass das Steuerelement, das der Effekt angefügt ist keine der `Control.Layer` Eigenschaften. Keine Implementierung erfolgt durch die `OnDetached` Methode, da kein Cleanup erforderlich ist.

### <a name="android-project"></a>Android Project

Das folgende Codebeispiel zeigt die `LabelShadowEffect` Implementierung für das Android-Projekt:

```csharp
[assembly:ResolutionGroupName ("MyCompany")]
[assembly:ExportEffect (typeof(LabelShadowEffect), "LabelShadowEffect")]
namespace EffectsDemo.Droid
{
    public class LabelShadowEffect : PlatformEffect
    {
        protected override void OnAttached ()
        {
            try {
                var control = Control as Android.Widget.TextView;
                var effect = (ShadowEffect)Element.Effects.FirstOrDefault (e => e is ShadowEffect);
                if (effect != null) {
                    float radius = effect.Radius;
                    float distanceX = effect.DistanceX;
                    float distanceY = effect.DistanceY;
                    Android.Graphics.Color color = effect.Color.ToAndroid ();
                    control.SetShadowLayer (radius, distanceX, distanceY, color);
                }
            } catch (Exception ex) {
                Console.WriteLine ("Cannot set property on attached control. Error: ", ex.Message);
            }
        }

        protected override void OnDetached ()
        {
        }
    }
}
```

Die `OnAttached` Methode ruft die `ShadowEffect` -Instanz und ruft die [ `TextView.SetShadowLayer` ](https://developer.xamarin.com/api/member/Android.Widget.TextView.SetShadowLayer/p/System.Single/System.Single/System.Single/Android.Graphics.Color/) Methode, um einen Schatten mit den angegebenen Eigenschaftswerten erstellen. Diese Funktion umschlossen ist ein `try` / `catch` blockieren, für den Fall, dass das Steuerelement, das der Effekt angefügt ist keine der `Control.Layer` Eigenschaften. Keine Implementierung erfolgt durch die `OnDetached` Methode, da kein Cleanup erforderlich ist.

### <a name="windows-phone--universal-windows-platform-projects"></a>Windows Phone & universelle Windows-Plattformprojekten

Das folgende Codebeispiel zeigt die `LabelShadowEffect` Implementierung für die Projekte für Windows Phone und universelle Windows-Plattform (UWP):

```csharp
[assembly: ResolutionGroupName ("Xamarin")]
[assembly: ExportEffect (typeof(LabelShadowEffect), "LabelShadowEffect")]
namespace EffectsDemo.WinPhone81
{
    public class LabelShadowEffect : PlatformEffect
    {
        bool shadowAdded = false;

        protected override void OnAttached ()
        {
            try {
                if (!shadowAdded) {
                    var effect = (ShadowEffect)Element.Effects.FirstOrDefault (e => e is ShadowEffect);
                    if (effect != null) {
                        var textBlock = Control as Windows.UI.Xaml.Controls.TextBlock;
                        var shadowLabel = new Label ();
                        shadowLabel.Text = textBlock.Text;
                        shadowLabel.FontAttributes = FontAttributes.Bold;
                        shadowLabel.HorizontalOptions = LayoutOptions.Center;
                        shadowLabel.VerticalOptions = LayoutOptions.CenterAndExpand;
                        shadowLabel.TextColor = effect.Color;
                        shadowLabel.TranslationX = effect.DistanceX;
                        shadowLabel.TranslationY = effect.DistanceY;

                        ((Grid)Element.Parent).Children.Insert (0, shadowLabel);
                        shadowAdded = true;
                    }
                }
            } catch (Exception ex) {
                Debug.WriteLine ("Cannot set property on attached control. Error: ", ex.Message);
            }
        }

        protected override void OnDetached ()
        {
        }
    }
}
```

Windows-Runtime und der universellen Windows-Plattform einen Schatteneffekt stellen keine muss das `LabelShadowEffect` Implementierung auf beiden Plattformen simuliert ein Datum durch Hinzufügen eines zweiten Offsets [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) hinter dem primären `Label`. Die `OnAttached` Methode ruft die `ShadowEffect` Instanz ist, erstellt das neue `Label`, und einige Eigenschaften für das Layout für die `Label`. Klicken Sie dann wird die Schattenkopie erstellt, durch Festlegen der [ `TextColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.TextColor/), [ `TranslationX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationX/), und [ `TranslationY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationY/) Eigenschaften zum Steuern der Farbe und Position von der `Label`. Die `shadowLabel` wird eingefügt hinter dem primären offset `Label`. Diese Funktion umschlossen ist ein `try` / `catch` blockieren, für den Fall, dass das Steuerelement, das der Effekt angefügt ist keine der `Control.Layer` Eigenschaften. Keine Implementierung erfolgt durch die `OnDetached` Methode, da kein Cleanup erforderlich ist.

## <a name="summary"></a>Zusammenfassung

Dieser Artikel hat mittels CLR-Eigenschaften zum Übergeben von Parametern mit einem Ergebnis gezeigt. CLR-Eigenschaften können verwendet werden, Auswirkungen Parameter zu definieren, auf die Änderungen zur Laufzeit reagiert nicht.


## <a name="related-links"></a>Verwandte Links

- [Benutzerdefinierte Renderer](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
- [Auswirkung](https://developer.xamarin.com/api/type/Xamarin.Forms.Effect/)
- [PlatformEffect](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformEffect%3CTContainer,TControl%3E/)
- [RoutingEffect](https://developer.xamarin.com/api/type/Xamarin.Forms.RoutingEffect/)
- [Schatten (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/effects/shadoweffect/)
