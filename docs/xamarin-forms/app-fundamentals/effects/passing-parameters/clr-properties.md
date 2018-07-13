---
title: Übergeben von Parametern Auswirkungen als Common Language Runtime-Eigenschaften
description: Common Language Runtime (CLR)-Eigenschaften können Auswirkungen Parameter zu definieren, die auf Änderungen zur Laufzeit reagieren nicht verwendet werden. Dieser Artikel veranschaulicht mithilfe von CLR-Eigenschaften zum Übergeben von Parametern in einen Effekt.
ms.prod: xamarin
ms.assetid: 4B50466C-5DBD-45DD-B1E6-BE9524C92F27
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/05/2016
ms.openlocfilehash: 1bb357b256a7cc6d52d1d92613f38cbf48400c4c
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995767"
---
# <a name="passing-effect-parameters-as-common-language-runtime-properties"></a>Übergeben von Parametern Auswirkungen als Common Language Runtime-Eigenschaften

_Common Language Runtime (CLR)-Eigenschaften können Auswirkungen Parameter zu definieren, die auf Änderungen zur Laufzeit reagieren nicht verwendet werden. Dieser Artikel veranschaulicht mithilfe von CLR-Eigenschaften zum Übergeben von Parametern in einen Effekt._

Der Prozess zum Erstellen von Auswirkungen-Parameter, die auf Änderungen zur Laufzeit reagieren nicht lautet wie folgt aus:

1. Erstellen Sie eine `public` Klasse diese Unterklassen der [ `RoutingEffect` ](xref:Xamarin.Forms.RoutingEffect) Klasse. Die `RoutingEffect` Klasse stellt eine plattformunabhängige ab, der einen inneren Effekt einschließt, die in der Regel plattformspezifisch ist.
1. Erstellen Sie einen Konstruktor, der ruft der Konstruktor der Basisklasse übergeben, die in einer Verkettung der Namen der Lösung und die eindeutige ID, die für jede Clientplattform-spezifische Auswirkungen-Klasse angegeben wurde.
1. Die Klasse für jeden Parameter an die Auswirkungen zu übergebenden fügen Sie Eigenschaften hinzu.

Parameter können dann mit dem Hinweis übergeben werden, durch Angeben von Werten für jede Eigenschaft, bei der Instanziierung des Effekts.

Die beispielanwendung zeigt eine `ShadowEffect` , die den Text durch ein Schatten hinzugefügt eine [ `Label` ](xref:Xamarin.Forms.Label) Steuerelement. Das folgende Diagramm veranschaulicht die Verantwortlichkeiten der einzelnen Projekte in der beispielanwendung, sowie die Beziehungen zwischen ihnen:

![](clr-properties-images/shadow-effect.png "Schatten Auswirkungen Project-Aufgaben")

Ein [ `Label` ](xref:Xamarin.Forms.Label) control für die `HomePage` angepasst wird, indem die `LabelShadowEffect` in den einzelnen plattformspezifischen Projekten. Parameter werden an die einzelnen übergeben `LabelShadowEffect` über Eigenschaften in der `ShadowEffect` Klasse. Jede `LabelShadowEffect` Klasse leitet sich von der `PlatformEffect` -Klasse für jede Plattform. Dadurch wird ein Schatten hinzugefügt wird, auf den Text angezeigt, indem die `Label` zu steuern, wie in den folgenden Screenshots gezeigt:

![](clr-properties-images/screenshots.png "Schatteneffekt auf jeder Plattform")

## <a name="creating-effect-parameters"></a>Erstellen von Parametern, wirksam

Ein `public` Klasse diese Unterklassen der [ `RoutingEffect` ](xref:Xamarin.Forms.RoutingEffect) Klasse sollte zur Darstellung von Auswirkungen Parameter erstellt werden, wie im folgenden Codebeispiel wird veranschaulicht:

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

Die `ShadowEffect` enthält vier Eigenschaften, die auf jedes plattformspezifische übergeben von Parametern an darstellen `LabelShadowEffect`. Der Klassenkonstruktor ruft der Konstruktor der Basisklasse, übergeben eines Parameters, bestehend aus einer Verkettung der Namen der Lösung und die eindeutige ID, die für jede Clientplattform-spezifische Auswirkungen-Klasse angegeben wurde. Aus diesem Grund eine neue Instanz der der `MyCompany.LabelShadowEffect` eines Steuerelements hinzugefügt [ `Effects` ](xref:Xamarin.Forms.Element.Effects) Auflistung bei einer `ShadowEffect` instanziiert wird.

## <a name="consuming-the-effect"></a>Nutzen die Auswirkungen

Das folgende XAML-Code-Beispiel zeigt eine [ `Label` ](xref:Xamarin.Forms.Label) Steuerelement, auf das die `ShadowEffect` angefügt ist:

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

Die entsprechende [ `Label` ](xref:Xamarin.Forms.Label) in c# wird im folgenden Codebeispiel dargestellt:

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

In beiden Codebeispielen eine Instanz der die `ShadowEffect` Klasse instanziiert wird, mit Werten, die für jede Eigenschaft angegeben wird, die vor dem Hinzufügen des Steuerelements [ `Effects` ](xref:Xamarin.Forms.Element.Effects) Auflistung. Beachten Sie, dass die `ShadowEffect.Color` Eigenschaft verwendet plattformspezifische Farbwerte. Weitere Informationen finden Sie unter [Geräteklasse](~/xamarin-forms/platform/device.md).

## <a name="creating-the-effect-on-each-platform"></a>Erstellen die Auswirkungen auf jeder Plattform

Die folgenden Abschnitte beschreiben die plattformspezifische Implementierung der `LabelShadowEffect` Klasse.

### <a name="ios-project"></a>iOS-Projekts

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

Die `OnAttached` Methode ruft die `ShadowEffect` -Instanz und legt `Control.Layer` Eigenschaften, die den angegebenen Eigenschaftswerten den Schatten zu erstellen. Diese Funktion umschlossen ist eine `try` / `catch` block für den Fall, dass das Steuerelement, das der Effekt angefügt ist keine der `Control.Layer` Eigenschaften. Keine Implementierung erfolgt über die `OnDetached` Methode, da kein Cleanup erforderlich ist.

### <a name="android-project"></a>Android-Projekt

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

Die `OnAttached` Methode ruft die `ShadowEffect` -Instanz, und ruft die [ `TextView.SetShadowLayer` ](https://developer.xamarin.com/api/member/Android.Widget.TextView.SetShadowLayer/p/System.Single/System.Single/System.Single/Android.Graphics.Color/) Methode, um einen Schatten mit den angegebenen Eigenschaftswerten erstellen. Diese Funktion umschlossen ist eine `try` / `catch` block für den Fall, dass das Steuerelement, das der Effekt angefügt ist keine der `Control.Layer` Eigenschaften. Keine Implementierung erfolgt über die `OnDetached` Methode, da kein Cleanup erforderlich ist.

### <a name="universal-windows-platform-project"></a>Universelle Windows-Plattform-Projekt

Das folgende Codebeispiel zeigt die `LabelShadowEffect` Implementierung für das Projekt für universelle Windows-Plattform (UWP):

```csharp
[assembly: ResolutionGroupName ("Xamarin")]
[assembly: ExportEffect (typeof(LabelShadowEffect), "LabelShadowEffect")]
namespace EffectsDemo.UWP
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

Die universelle Windows-Plattform nicht Geben Sie einen Schatteneffekt, daher wird der `LabelShadowEffect` Implementierung auf beiden Plattformen simuliert einen durch das Hinzufügen eines zweiten Offsets [ `Label` ](xref:Xamarin.Forms.Label) des primären Replikats `Label`. Der `OnAttached` Methode ruft die `ShadowEffect` Instanz ist, erstellt das neue `Label`, und legt einige Layouteigenschaften für das `Label`. Er erstellt dann Schattens durch Festlegen der [ `TextColor` ](xref:Xamarin.Forms.Label.TextColor), [ `TranslationX` ](xref:Xamarin.Forms.VisualElement.TranslationX), und [ `TranslationY` ](xref:Xamarin.Forms.VisualElement.TranslationY) Eigenschaften zum Steuern von Farbe und Position des der `Label`. Die `shadowLabel` wird eingefügt offset des primären Replikats `Label`. Diese Funktion umschlossen ist eine `try` / `catch` block für den Fall, dass das Steuerelement, das der Effekt angefügt ist keine der `Control.Layer` Eigenschaften. Keine Implementierung erfolgt über die `OnDetached` Methode, da kein Cleanup erforderlich ist.

## <a name="summary"></a>Zusammenfassung

Dieser Artikel hat veranschaulicht mittels CLR-Eigenschaften zum Übergeben von Parametern in einen Effekt. CLR-Eigenschaften können Auswirkungen Parameter zu definieren, die auf Änderungen zur Laufzeit reagieren nicht verwendet werden.


## <a name="related-links"></a>Verwandte Links

- [Benutzerdefinierte Renderer](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
- [Auswirkung](xref:Xamarin.Forms.Effect)
- [PlatformEffect](xref:Xamarin.Forms.PlatformEffect`2)
- [RoutingEffect](xref:Xamarin.Forms.RoutingEffect)
- [Schatteneffekt (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/effects/shadoweffect/)
