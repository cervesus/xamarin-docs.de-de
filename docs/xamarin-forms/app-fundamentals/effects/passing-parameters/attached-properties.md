---
title: Übergeben von Parametern Auswirkungen als angefügte Eigenschaften
description: Angefügte Eigenschaften können verwendet werden, Auswirkungen Parameter zu definieren, die auf Änderungen zur Laufzeit reagieren. In diesem Artikel wird veranschaulicht, angefügte Eigenschaften zum Übergeben von Parametern einen Effekt, und ändern einen Parameter zur Laufzeit zu verwenden.
ms.prod: xamarin
ms.assetid: DFCDCB9F-17DD-4117-BD53-B4FB206BB387
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/05/2016
ms.openlocfilehash: 9483e424a74a88ce3f0eb49624bb5315551f2062
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996450"
---
# <a name="passing-effect-parameters-as-attached-properties"></a>Übergeben von Parametern Auswirkungen als angefügte Eigenschaften

_Angefügte Eigenschaften können verwendet werden, Auswirkungen Parameter zu definieren, die auf Änderungen zur Laufzeit reagieren. In diesem Artikel wird veranschaulicht, angefügte Eigenschaften zum Übergeben von Parametern einen Effekt, und ändern einen Parameter zur Laufzeit zu verwenden._

Der Prozess zum Erstellen von Auswirkungen-Parameter, die Reaktion auf Änderungen der Common Language Runtime-Eigenschaft lautet wie folgt aus:

1. Erstellen Sie eine `static` Klasse enthält eine angefügte Eigenschaft für jeden Parameter mit dem Hinweis übergeben werden.
1. Hinzufügen einer weiteren angefügten Eigenschaft der Klasse, die verwendet wird, steuern Sie das Hinzufügen oder entfernen Sie die Auswirkungen auf das Steuerelement, dem die Klasse zugeordnet wird. Stellen Sie sicher, dass diese Eigenschaft Register angefügt eine `propertyChanged` Delegat, der ausgeführt wird, wenn der Wert der Eigenschaft geändert wird.
1. Erstellen Sie `static` Getter und Setter für jedes angefügte Eigenschaft.
1. Implementieren Sie die Logik in die `propertyChanged` Delegaten hinzufügen und entfernen Sie den Effekt.
1. Implementieren Sie eine geschachtelte Klasse innerhalb der `static` Klasse, mit dem Namen nach dem die Auswirkungen, die Unterklassen der `RoutingEffect` Klasse. Rufen Sie für den Konstruktor der Basisklasse-Konstruktors und übergibt eine Verkettung der Namen der Lösung und die eindeutige ID, die für jede Clientplattform-spezifische Auswirkungen-Klasse angegeben wurde. aus.

Parameter können dann mit dem Hinweis durch Hinzufügen von angefügten Eigenschaften und Eigenschaftswerte an das entsprechende Steuerelement übergeben werden. Darüber hinaus können Parameter zur Laufzeit geändert werden, indem Sie einen neuen Wert der angefügten Eigenschaft angeben.

> [!NOTE]
> Eine angefügte Eigenschaft ist eine besondere Art von bindbare Eigenschaft, die in einer Klasse, aber mit anderen Objekten verbunden und erkennbaren in XAML definiert wird, als Attribute, die eine Klasse und einen Eigenschaftsnamen, die durch einen Punkt getrennt enthalten. Weitere Informationen finden Sie unter [angefügte Eigenschaften](~/xamarin-forms/xaml/attached-properties.md).

Die beispielanwendung zeigt eine `ShadowEffect` , die den Text durch ein Schatten hinzugefügt eine [ `Label` ](xref:Xamarin.Forms.Label) Steuerelement. Darüber hinaus kann die Farbe des Schattens zur Laufzeit geändert werden. Das folgende Diagramm veranschaulicht die Verantwortlichkeiten der einzelnen Projekte in der beispielanwendung, sowie die Beziehungen zwischen ihnen:

![](attached-properties-images/shadow-effect.png "Schatten Auswirkungen Project-Aufgaben")

Ein [ `Label` ](xref:Xamarin.Forms.Label) control für die `HomePage` angepasst wird, indem die `LabelShadowEffect` in den einzelnen plattformspezifischen Projekten. Parameter werden an die einzelnen übergeben `LabelShadowEffect` über angefügte Eigenschaften in der `ShadowEffect` Klasse. Jede `LabelShadowEffect` Klasse leitet sich von der `PlatformEffect` -Klasse für jede Plattform. Dadurch wird ein Schatten hinzugefügt wird, auf den Text angezeigt, indem die `Label` zu steuern, wie in den folgenden Screenshots gezeigt:

![](attached-properties-images/screenshots.png "Schatteneffekt auf jeder Plattform")

## <a name="creating-effect-parameters"></a>Erstellen von Parametern, wirksam

Ein `static` Klasse sollte zur Darstellung von Auswirkungen Parameter erstellt werden, wie im folgenden Codebeispiel wird veranschaulicht:

```csharp
public static class ShadowEffect
{
  public static readonly BindableProperty HasShadowProperty =
    BindableProperty.CreateAttached ("HasShadow", typeof(bool), typeof(ShadowEffect), false, propertyChanged: OnHasShadowChanged);
  public static readonly BindableProperty ColorProperty =
    BindableProperty.CreateAttached ("Color", typeof(Color), typeof(ShadowEffect), Color.Default);
  public static readonly BindableProperty RadiusProperty =
    BindableProperty.CreateAttached ("Radius", typeof(double), typeof(ShadowEffect), 1.0);
  public static readonly BindableProperty DistanceXProperty =
    BindableProperty.CreateAttached ("DistanceX", typeof(double), typeof(ShadowEffect), 0.0);
  public static readonly BindableProperty DistanceYProperty =
    BindableProperty.CreateAttached ("DistanceY", typeof(double), typeof(ShadowEffect), 0.0);

  public static bool GetHasShadow (BindableObject view)
  {
    return (bool)view.GetValue (HasShadowProperty);
  }

  public static void SetHasShadow (BindableObject view, bool value)
  {
    view.SetValue (HasShadowProperty, value);
  }
  ...

  static void OnHasShadowChanged (BindableObject bindable, object oldValue, object newValue)
  {
    var view = bindable as View;
    if (view == null) {
      return;
    }

    bool hasShadow = (bool)newValue;
    if (hasShadow) {
      view.Effects.Add (new LabelShadowEffect ());
    } else {
      var toRemove = view.Effects.FirstOrDefault (e => e is LabelShadowEffect);
      if (toRemove != null) {
        view.Effects.Remove (toRemove);
      }
    }
  }

  class LabelShadowEffect : RoutingEffect
  {
    public LabelShadowEffect () : base ("MyCompany.LabelShadowEffect")
    {
    }
  }
}
```

Die `ShadowEffect` enthält fünf angefügte Eigenschaften, mit `static` Getter und Setter für jedes angefügte Eigenschaft. Vier dieser Eigenschaften darstellen, jedes plattformspezifische übergeben von Parametern an `LabelShadowEffect`. Die `ShadowEffect` -Klasse definiert außerdem eine `HasShadow` angefügten Eigenschaft, die verwendet wird, um das Hinzufügen oder entfernen Sie die Auswirkungen auf das Steuerelement zu steuern, die die `ShadowEffect` Klasse, die angefügt wird. Diese angefügte Eigenschaft registriert die `OnHasShadowChanged` -Methode, die ausgeführt werden, wenn der Wert der Eigenschaft geändert wird. Diese Methode fügt hinzu oder entfernt den Effekt, der anhand des Werts von der `HasShadow` angefügte Eigenschaft.

Die geschachtelte `LabelShadowEffect` Klasse, die Unterklassen der [ `RoutingEffect` ](xref:Xamarin.Forms.RoutingEffect) Klasse unterstützt Effekt hinzufügen und entfernen. Die `RoutingEffect` Klasse stellt eine plattformunabhängige ab, der einen inneren Effekt einschließt, die in der Regel plattformspezifisch ist. Dies vereinfacht das Effekt entfernen, da kein während der Kompilierung auf die Typinformationen für eine plattformspezifische-Effekt Zugriff. Die `LabelShadowEffect` ruft der Konstruktor der Basisklasse-Konstruktors und übergibt einen Parameter, bestehend aus einer Verkettung der Namen der Lösung und die eindeutige ID, die für jede Clientplattform-spezifische Auswirkungen-Klasse angegeben wurde. Dadurch wirksam das Hinzufügen und entfernen Sie in der `OnHasShadowChanged` -Methode wie folgt:

- **Außerdem Auswirkungen** – eine neue Instanz der dem `LabelShadowEffect` des Steuerelements hinzugefügt [ `Effects` ](xref:Xamarin.Forms.Element.Effects) Auflistung. Dies ersetzt die Verwendung der [ `Effect.Resolve` ](xref:Xamarin.Forms.Effect.Resolve(System.String)) Methode, um den Effekt hinzufügen.
- **Auswirkungen der Entfernung** – die erste Instanz von der `LabelShadowEffect` in des Steuerelements [ `Effects` ](xref:Xamarin.Forms.Element.Effects) Sammlung abgerufen und entfernt.

## <a name="consuming-the-effect"></a>Nutzen die Auswirkungen

Jedes plattformspezifische `LabelShadowEffect` können genutzt werden, durch die angefügten Eigenschaften zum Hinzufügen einer [ `Label` ](xref:Xamarin.Forms.Label) zu steuern, wie in den folgenden XAML-Codebeispiel veranschaulicht:

```xaml
<Label Text="Label Shadow Effect" ...
       local:ShadowEffect.HasShadow="true" local:ShadowEffect.Radius="5"
       local:ShadowEffect.DistanceX="5" local:ShadowEffect.DistanceY="5">
  <local:ShadowEffect.Color>
    <OnPlatform x:TypeArguments="Color">
        <On Platform="iOS" Value="Black" />
        <On Platform="Android" Value="White" />
        <On Platform="UWP" Value="Red" />
    </OnPlatform>
  </local:ShadowEffect.Color>
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

ShadowEffect.SetHasShadow (label, true);
ShadowEffect.SetRadius (label, 5);
ShadowEffect.SetDistanceX (label, 5);
ShadowEffect.SetDistanceY (label, 5);
ShadowEffect.SetColor (label, color));
```

Festlegen der `ShadowEffect.HasShadow` angefügten Eigenschaft, um `true` führt die `ShadowEffect.OnHasShadowChanged` -Methode, die hinzugefügt oder entfernt die `LabelShadowEffect` auf die [ `Label` ](xref:Xamarin.Forms.Label) Steuerelement. In beiden Codebeispielen die `ShadowEffect.Color` angefügte Eigenschaft enthält plattformspezifische Farbwerte. Weitere Informationen finden Sie unter [Geräteklasse](~/xamarin-forms/platform/device.md).

Darüber hinaus eine [ `Button` ](xref:Xamarin.Forms.Button) die Schattenfarbe zur Laufzeit geändert werden können. Wenn die `Button` geklickt wird, wird die folgenden codeänderungen, die Farbe des Schattens, durch Festlegen der `ShadowEffect.Color` angefügte Eigenschaft:

```csharp
ShadowEffect.SetColor (label, Color.Teal);
```

### <a name="consuming-the-effect-with-a-style"></a>Verwenden den Effekt mit einem Stil

Effekte, die genutzt werden können, durch das Hinzufügen von angefügten Eigenschaften auf ein Steuerelement können auch von einem Format genutzt werden. Das folgende XAML-Code-Beispiel zeigt eine *explizite* Stil für den Schatteneffekt, die auf angewendet werden kann [ `Label` ](xref:Xamarin.Forms.Label) Steuerelemente:

```xaml
<Style x:Key="ShadowEffectStyle" TargetType="Label">
  <Style.Setters>
    <Setter Property="local:ShadowEffect.HasShadow" Value="True" />
    <Setter Property="local:ShadowEffect.Radius" Value="5" />
    <Setter Property="local:ShadowEffect.DistanceX" Value="5" />
    <Setter Property="local:ShadowEffect.DistanceY" Value="5" />
  </Style.Setters>
</Style>
```

Die [ `Style` ](xref:Xamarin.Forms.Style) angewendet werden können, um eine [ `Label` ](xref:Xamarin.Forms.Label) durch Festlegen der [ `Style` ](xref:Xamarin.Forms.VisualElement.Style) Eigenschaft, um die `Style` -Instanz unter Verwendung der `StaticResource`Markuperweiterung, wie im folgenden Codebeispiel gezeigt:

```xaml
<Label Text="Label Shadow Effect" ... Style="{StaticResource ShadowEffectStyle}" />
```

Weitere Informationen zu Stilen finden Sie unter [Stile](~/xamarin-forms/user-interface/styles/index.md).

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
                UpdateRadius ();
                UpdateColor ();
                UpdateOffset ();
                Control.Layer.ShadowOpacity = 1.0f;
            } catch (Exception ex) {
                Console.WriteLine ("Cannot set property on attached control. Error: ", ex.Message);
            }
        }

        protected override void OnDetached ()
        {
        }
        ...

        void UpdateRadius ()
        {
            Control.Layer.CornerRadius = (nfloat)ShadowEffect.GetRadius (Element);
        }

        void UpdateColor ()
        {
            Control.Layer.ShadowColor = ShadowEffect.GetColor (Element).ToCGColor ();
        }

        void UpdateOffset ()
        {
            Control.Layer.ShadowOffset = new CGSize (
                (double)ShadowEffect.GetDistanceX (Element),
                (double)ShadowEffect.GetDistanceY (Element));
        }
    }
```

Die `OnAttached` -Methode ruft Methoden, die die angefügten Eigenschaftswerte, die mithilfe von Abrufen der `ShadowEffect` Getter und festgelegt `Control.Layer` Eigenschaften, die die Eigenschaftswerte den Schatten zu erstellen. Diese Funktion umschlossen ist eine `try` / `catch` block für den Fall, dass das Steuerelement, das der Effekt angefügt ist keine der `Control.Layer` Eigenschaften. Keine Implementierung erfolgt über die `OnDetached` Methode, da kein Cleanup erforderlich ist.

#### <a name="responding-to-property-changes"></a>Reagieren auf Änderungen der Eigenschaften

Wenn die `ShadowEffect` angefügte Eigenschaft geändert werden zur Laufzeit, die Auswirkungen Anforderungen zu reagieren, indem Sie die Änderungen anzeigen. Eine außer Kraft gesetzte Version von der `OnElementPropertyChanged` -Methode, in der plattformspezifischen Auswirkungen-Klasse ist der Ort, an die Reaktion auf Änderungen der bindbare Eigenschaft, wie im folgenden Codebeispiel wird veranschaulicht:

```csharp
public class LabelShadowEffect : PlatformEffect
{
  ...
  protected override void OnElementPropertyChanged (PropertyChangedEventArgs args)
  {
    if (args.PropertyName == ShadowEffect.RadiusProperty.PropertyName) {
      UpdateRadius ();
    } else if (args.PropertyName == ShadowEffect.ColorProperty.PropertyName) {
      UpdateColor ();
    } else if (args.PropertyName == ShadowEffect.DistanceXProperty.PropertyName ||
               args.PropertyName == ShadowEffect.DistanceYProperty.PropertyName) {
      UpdateOffset ();
    }
  }
  ...
}
```

Die `OnElementPropertyChanged` Methode aktualisiert die Radius, Farbe oder Offset des Schattens bereitgestellt, die die entsprechende `ShadowEffect` Wert der angefügten Eigenschaft geändert wurde. Überprüft, ob die geänderte Eigenschaft sollten immer vorgenommen werden, wie diese Außerkraftsetzung oft aufgerufen werden kann.

### <a name="android-project"></a>Android-Projekt

Das folgende Codebeispiel zeigt die `LabelShadowEffect` Implementierung für das Android-Projekt:

```csharp
[assembly:ResolutionGroupName ("MyCompany")]
[assembly:ExportEffect (typeof(LabelShadowEffect), "LabelShadowEffect")]
namespace EffectsDemo.Droid
{
    public class LabelShadowEffect : PlatformEffect
    {
        Android.Widget.TextView control;
        Android.Graphics.Color color;
        float radius, distanceX, distanceY;

        protected override void OnAttached ()
        {
            try {
                control = Control as Android.Widget.TextView;
                UpdateRadius ();
                UpdateColor ();
                UpdateOffset ();
                UpdateControl ();
            } catch (Exception ex) {
                Console.WriteLine ("Cannot set property on attached control. Error: ", ex.Message);
            }
        }

        protected override void OnDetached ()
        {
        }
        ...

        void UpdateControl ()
        {
            if (control != null) {
                control.SetShadowLayer (radius, distanceX, distanceY, color);
            }
        }

        void UpdateRadius ()
        {
            radius = (float)ShadowEffect.GetRadius (Element);
        }

        void UpdateColor ()
        {
            color = ShadowEffect.GetColor (Element).ToAndroid ();
        }

        void UpdateOffset ()
        {
            distanceX = (float)ShadowEffect.GetDistanceX (Element);
            distanceY = (float)ShadowEffect.GetDistanceY (Element);
        }
    }
```

Die `OnAttached` -Methode ruft Methoden, die die angefügten Eigenschaftswerte mit Abrufen der `ShadowEffect` Getter, und ruft eine Methode, die Aufrufe der [ `TextView.SetShadowLayer` ](https://developer.xamarin.com/api/member/Android.Widget.TextView.SetShadowLayer/p/System.Single/System.Single/System.Single/Android.Graphics.Color/) Methode, um einen Schatten unter Verwendung der Eigenschaftswerte zu erstellen. Diese Funktion umschlossen ist eine `try` / `catch` block für den Fall, dass das Steuerelement, das der Effekt angefügt ist keine der `Control.Layer` Eigenschaften. Keine Implementierung erfolgt über die `OnDetached` Methode, da kein Cleanup erforderlich ist.

#### <a name="responding-to-property-changes"></a>Reagieren auf Änderungen der Eigenschaften

Wenn die `ShadowEffect` angefügte Eigenschaft geändert werden zur Laufzeit, die Auswirkungen Anforderungen zu reagieren, indem Sie die Änderungen anzeigen. Eine außer Kraft gesetzte Version von der `OnElementPropertyChanged` -Methode, in der plattformspezifischen Auswirkungen-Klasse ist der Ort, an die Reaktion auf Änderungen der bindbare Eigenschaft, wie im folgenden Codebeispiel wird veranschaulicht:

```csharp
public class LabelShadowEffect : PlatformEffect
{
  ...
  protected override void OnElementPropertyChanged (PropertyChangedEventArgs args)
  {
    if (args.PropertyName == ShadowEffect.RadiusProperty.PropertyName) {
      UpdateRadius ();
      UpdateControl ();
    } else if (args.PropertyName == ShadowEffect.ColorProperty.PropertyName) {
      UpdateColor ();
      UpdateControl ();
    } else if (args.PropertyName == ShadowEffect.DistanceXProperty.PropertyName ||
               args.PropertyName == ShadowEffect.DistanceYProperty.PropertyName) {
      UpdateOffset ();
      UpdateControl ();
    }
  }
  ...
}
```

Die `OnElementPropertyChanged` Methode aktualisiert die Radius, Farbe oder Offset des Schattens bereitgestellt, die die entsprechende `ShadowEffect` Wert der angefügten Eigenschaft geändert wurde. Überprüft, ob die geänderte Eigenschaft sollten immer vorgenommen werden, wie diese Außerkraftsetzung oft aufgerufen werden kann.

### <a name="universal-windows-platform-project"></a>Universelle Windows-Plattform-Projekt

Das folgende Codebeispiel zeigt die `LabelShadowEffect` Implementierung für das Projekt für universelle Windows-Plattform (UWP):

```csharp
[assembly: ResolutionGroupName ("MyCompany")]
[assembly: ExportEffect (typeof(LabelShadowEffect), "LabelShadowEffect")]
namespace EffectsDemo.UWP
{
    public class LabelShadowEffect : PlatformEffect
    {
        Label shadowLabel;
        bool shadowAdded = false;

        protected override void OnAttached ()
        {
            try {
                if (!shadowAdded) {
                    var textBlock = Control as Windows.UI.Xaml.Controls.TextBlock;

                    shadowLabel = new Label ();
                    shadowLabel.Text = textBlock.Text;
                    shadowLabel.FontAttributes = FontAttributes.Bold;
                    shadowLabel.HorizontalOptions = LayoutOptions.Center;
                    shadowLabel.VerticalOptions = LayoutOptions.CenterAndExpand;

                    UpdateColor ();
                    UpdateOffset ();

                    ((Grid)Element.Parent).Children.Insert (0, shadowLabel);
                    shadowAdded = true;
                }
            } catch (Exception ex) {
                Debug.WriteLine ("Cannot set property on attached control. Error: ", ex.Message);
            }
        }

        protected override void OnDetached ()
        {
        }
        ...

        void UpdateColor ()
        {
            shadowLabel.TextColor = ShadowEffect.GetColor (Element);
        }

        void UpdateOffset ()
        {
            shadowLabel.TranslationX = ShadowEffect.GetDistanceX (Element);
            shadowLabel.TranslationY = ShadowEffect.GetDistanceY (Element);
        }
    }
}
```

Die universelle Windows-Plattform nicht Geben Sie einen Schatteneffekt, daher wird der `LabelShadowEffect` Implementierung auf beiden Plattformen simuliert einen durch das Hinzufügen eines zweiten Offsets [ `Label` ](xref:Xamarin.Forms.Label) des primären Replikats `Label`. Die `OnAttached` Methode erstellt die neue `Label` und legt einige Layouteigenschaften für das `Label`. Es ruft dann die Methoden zum Abrufen der Werte der angefügten Eigenschaft, die mit der `ShadowEffect` Getter, und erstellt die Schattenkopie durch Festlegen der [ `TextColor` ](xref:Xamarin.Forms.Label.TextColor), [ `TranslationX` ](xref:Xamarin.Forms.VisualElement.TranslationX), und [ `TranslationY` ](xref:Xamarin.Forms.VisualElement.TranslationY) Eigenschaften fest, die Farbe und Position des der `Label`. Die `shadowLabel` wird eingefügt offset des primären Replikats `Label`. Diese Funktion umschlossen ist eine `try` / `catch` block für den Fall, dass das Steuerelement, das der Effekt angefügt ist keine der `Control.Layer` Eigenschaften. Keine Implementierung erfolgt über die `OnDetached` Methode, da kein Cleanup erforderlich ist.

#### <a name="responding-to-property-changes"></a>Reagieren auf Änderungen der Eigenschaften

Wenn die `ShadowEffect` angefügte Eigenschaft geändert werden zur Laufzeit, die Auswirkungen Anforderungen zu reagieren, indem Sie die Änderungen anzeigen. Eine außer Kraft gesetzte Version von der `OnElementPropertyChanged` -Methode, in der plattformspezifischen Auswirkungen-Klasse ist der Ort, an die Reaktion auf Änderungen der bindbare Eigenschaft, wie im folgenden Codebeispiel wird veranschaulicht:

```csharp
public class LabelShadowEffect : PlatformEffect
{
  ...
  protected override void OnElementPropertyChanged (PropertyChangedEventArgs args)
  {
    if (args.PropertyName == ShadowEffect.ColorProperty.PropertyName) {
      UpdateColor ();
    } else if (args.PropertyName == ShadowEffect.DistanceXProperty.PropertyName ||
                      args.PropertyName == ShadowEffect.DistanceYProperty.PropertyName) {
      UpdateOffset ();
    }
  }
  ...
}
```

Die `OnElementPropertyChanged` Methode aktualisiert wird, der Farbe oder der Offset des Schattens bereitgestellt, die die entsprechende `ShadowEffect` Wert der angefügten Eigenschaft geändert wurde. Überprüft, ob die geänderte Eigenschaft sollten immer vorgenommen werden, wie diese Außerkraftsetzung oft aufgerufen werden kann.

## <a name="summary"></a>Zusammenfassung

Dieser Artikel hat veranschaulicht angefügte Eigenschaften zum Übergeben von Parametern einen Effekt, und ändern einen Parameter zur Laufzeit zu verwenden. Angefügte Eigenschaften können verwendet werden, Auswirkungen Parameter zu definieren, die auf Änderungen zur Laufzeit reagieren.


## <a name="related-links"></a>Verwandte Links

- [Benutzerdefinierte Renderer](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
- [Auswirkung](xref:Xamarin.Forms.Effect)
- [PlatformEffect](xref:Xamarin.Forms.PlatformEffect`2)
- [RoutingEffect](xref:Xamarin.Forms.RoutingEffect)
- [Schatteneffekt (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/effects/shadoweffectruntimechange/)
