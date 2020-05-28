---
title: ''
description: ''
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 37d870509e034f4c23afba60fa055965ed9df4de
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2020
ms.locfileid: "84138863"
---
# <a name="passing-effect-parameters-as-common-language-runtime-properties"></a>Übergeben von Effect-Parametern als Eigenschaften der Common Language Runtime

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/effects-shadoweffect)

_CLR-Eigenschaften können verwendet werden, um Effect-Parameter zu definieren, die nicht auf Änderungen der Runtimeeigenschaften reagieren. In diesem Artikel wird veranschaulicht, wie CLR-Eigenschaften verwendet werden, um Parameter an einen Effekt zu übergeben._

Effect-Parameter, die nicht auf die Änderungen von Runtimeeigenschaften reagieren, werden folgendermaßen erstellt:

1. Erstellen Sie eine `public`-Klasse, die die [`RoutingEffect`](xref:Xamarin.Forms.RoutingEffect)-Klasse als Unterklasse implementiert. Die `RoutingEffect`-Klasse stellt einen plattformunabhängigen Effekt dar, der einen in der Regel plattformspezifischen inneren Effekt umschließt.
1. Erstellen Sie einen Konstruktor, der den Basisklassenkonstruktor aufruft, der eine Verkettung des Namens der Auflösungsgruppe übergibt, und die eindeutige ID, die in jeder plattformspezifischen Effect-Klasse angegeben wurde.
1. Fügen Sie der Klasse Eigenschaften für jeden Parameter hinzu, die an den Effekt übergeben werden sollen.

Parameter können anschließend an den Effekt übergeben werden, indem bei der Instanziierung des Effekts für jede Eigenschaft Werte angegeben werden.

Die Beispielanwendung veranschaulicht eine `ShadowEffect`-Klasse, die einen Schatten zu dem Text hinzufügt, der vom [`Label`](xref:Xamarin.Forms.Label)-Steuerelement angezeigt wird. Das folgende Diagramm veranschaulicht die Zuständigkeiten jedes Projekts in der Beispielanwendung sowie deren Beziehungen zueinander:

![](clr-properties-images/shadow-effect.png "Shadow Effect Project Responsibilities")

Ein [`Label`](xref:Xamarin.Forms.Label)-Steuerelement im `HomePage`-Element wird in jedem plattformspezifischen Projekt von `LabelShadowEffect` angepasst. Parameter werden über Eigenschaften in der `ShadowEffect`-Klasse an jeden `LabelShadowEffect` übergeben. Jede `LabelShadowEffect`-Klasse wird von der `PlatformEffect`-Klasse für jede Plattform abgeleitet. Dadurch wird wie in den folgenden Screenshots dargestellt ein Schatten zu dem Text hinzugefügt, der vom `Label`-Steuerelement angezeigt wird:

![](clr-properties-images/screenshots.png "Shadow Effect on each Platform")

## <a name="creating-effect-parameters"></a>Erstellen von Effect-Parametern

Eine `public`-Klasse, die die [`RoutingEffect`](xref:Xamarin.Forms.RoutingEffect)-Klasse als Unterklasse implementiert, sollte wie im folgenden Codebeispiel veranschaulicht zur Darstellung von Effect-Parametern erstellt werden:

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

Der `ShadowEffect` enthält vier Eigenschaften, die Parameter darstellen, die an jedes plattformspezifische `LabelShadowEffect`-Element übergeben werden sollen. Der Klassenkonstruktor ruft den Basisklassenkonstruktor auf, der einen Parameter bestehend aus einer Verkettung des Namens der Auflösungsgruppe übergibt, und die eindeutige ID, die in jeder plattformspezifischen Effect-Klasse angegeben wurde. Daher wird eine neue Instanz von `MyCompany.LabelShadowEffect` zur [`Effects`](xref:Xamarin.Forms.Element.Effects)-Sammlung des Steuerelements hinzugefügt, wenn `ShadowEffect` instanziiert wird.

## <a name="consuming-the-effect"></a>Nutzen des Effekts

Das folgende XAML-Codebeispiel stellt ein [`Label`](xref:Xamarin.Forms.Label)-Steuerelement dar, an das `ShadowEffect` angefügt ist:

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

Das äquivalente [`Label`](xref:Xamarin.Forms.Label)-Steuerelement in C# wird im folgenden Codebeispiel veranschaulicht:

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

In beiden Codebeispielen wird eine Instanz der `ShadowEffect`-Klasse vor dem Hinzufügen zur [`Effects`](xref:Xamarin.Forms.Element.Effects)-Sammlung des Steuerelements mit Werten instanziiert, die für jede Eigenschaft festgelegt wurden. Beachten Sie, dass die Eigenschaft `ShadowEffect.Color` plattformspezifische Farbwerte verwendet. Weitere Informationen finden Sie unter [Xamarin.Forms Device Class (Xamarin.Forms-Geräteklasse)](~/xamarin-forms/platform/device.md).

## <a name="creating-the-effect-on-each-platform"></a>Erstellen von Effect-Klassen auf verschiedenen Plattformen

In den folgenden Abschnitten wird die plattformspezifische Implementierung der `LabelShadowEffect`-Klasse erläutert.

### <a name="ios-project"></a>iOS-Projekt

Im folgenden Codebeispiel wird die Implementierung von `LabelShadowEffect` für das iOS-Projekt veranschaulicht:

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
                    Control.Layer.ShadowRadius = effect.Radius;
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

Die `OnAttached`-Methode ruft die `ShadowEffect`-Instanz ab und legt `Control.Layer`-Eigenschaften für die festgelegten Eigenschaftswerten fest, um den Schatteneffekt zu erstellen. Diese Funktion wird von einem `try`/`catch`-Block umschlossen, falls das Steuerelement, an das der Effekt angefügt ist, nicht über die `Control.Layer`-Eigenschaften verfügt. Von der `OnDetached`-Methode wird keine Implementierung bereitgestellt, da keine Bereinigung erforderlich ist.

### <a name="android-project"></a>Android-Projekt

Im folgenden Codebeispiel wird die Implementierung von `LabelShadowEffect` für das Android-Projekt veranschaulicht:

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

Die `OnAttached`-Methode ruft die `ShadowEffect`-Instanz ab und ruft die Methode [`TextView.SetShadowLayer`](xref:Android.Widget.TextView.SetShadowLayer*) auf, um mithilfe der bestimmten Eigenschaftswerte einen Schatteneffekt zu erstellen. Diese Funktion wird von einem `try`/`catch`-Block umschlossen, falls das Steuerelement, an das der Effekt angefügt ist, nicht über die `Control.Layer`-Eigenschaften verfügt. Von der `OnDetached`-Methode wird keine Implementierung bereitgestellt, da keine Bereinigung erforderlich ist.

### <a name="universal-windows-platform-project"></a>Projekt für die Universelle Windows-Plattform

Das folgende Codebeispiel veranschaulicht die Implementierung von `LabelShadowEffect` für ein Projekt für die Universelle Windows-Plattform (UWP):

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

Die Universelle Windows-Plattform stellt keinen Schatteneffekt zur Verfügung, sodass die Implementierung von `LabelShadowEffect` auf beiden Plattformen einen Effekt erzeugt, indem hinter der primären `Label`-Klasse eine zweite versetzte [`Label`](xref:Xamarin.Forms.Label)-Klasse hinzugefügt wird. Die `OnAttached`-Methode ruft die `ShadowEffect`-Instanz ab, erstellt das neue `Label`-Element und legt Layouteigenschaften für `Label` fest. Anschließend erstellt sie den Schatteneffekt durch das Festlegen der Eigenschaften [`TextColor`](xref:Xamarin.Forms.Label.TextColor), [`TranslationX`](xref:Xamarin.Forms.VisualElement.TranslationX) und [`TranslationY`](xref:Xamarin.Forms.VisualElement.TranslationY), um die Farbe und die Position von `Label` zu steuern. Das `shadowLabel`-Element wird dann versetzt hinter der primären `Label`-Klasse eingefügt. Diese Funktion wird von einem `try`/`catch`-Block umschlossen, falls das Steuerelement, an das der Effekt angefügt ist, nicht über die `Control.Layer`-Eigenschaften verfügt. Von der `OnDetached`-Methode wird keine Implementierung bereitgestellt, da keine Bereinigung erforderlich ist.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde veranschaulicht, wie CLR-Eigenschaften verwendet werden, um Parameter an einen Effekt zu übergeben. CLR-Eigenschaften können verwendet werden, um Effect-Parameter zu definieren, die nicht auf Änderungen von Runtimeeigenschaften reagieren.

## <a name="related-links"></a>Verwandte Links

- [Benutzerdefinierte Renderer](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
- [Effect class (Effect-Klasse)](xref:Xamarin.Forms.Effect)
- [PlatformEffect (PlatformEffect-Klasse)](xref:Xamarin.Forms.PlatformEffect`2)
- [RoutingEffect Class (RoutingEffect-Klasse)](xref:Xamarin.Forms.RoutingEffect)
- [Shadow Effect (Schatteneffekt (Beispiel))](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/effects-shadoweffect)
