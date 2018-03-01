---
title: "Übergeben von Parametern Auswirkungen als angefügte Eigenschaften"
description: "Angefügte Eigenschaften können verwendet werden, Auswirkungen Parameter zu definieren, die auf Änderungen zur Laufzeit reagieren. Dieser Artikel beschreibt die Eigenschaften zum Übergeben von Parametern eines Effekts, und ändern einen Parameter zur Laufzeit mit angefügt werden."
ms.topic: article
ms.prod: xamarin
ms.assetid: DFCDCB9F-17DD-4117-BD53-B4FB206BB387
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/05/2016
ms.openlocfilehash: df1287d2389d7645ee3f17b166af790f79aa70e1
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="passing-effect-parameters-as-attached-properties"></a>Übergeben von Parametern Auswirkungen als angefügte Eigenschaften

_Angefügte Eigenschaften können verwendet werden, Auswirkungen Parameter zu definieren, die auf Änderungen zur Laufzeit reagieren. Dieser Artikel beschreibt die Eigenschaften zum Übergeben von Parametern eines Effekts, und ändern einen Parameter zur Laufzeit mit angefügt werden._

Der Prozess zum Erstellen von Effekt-Parameter, die auf Änderungen zur Laufzeit reagieren lautet wie folgt:

1. Erstellen einer `static` Klasse enthält eine angefügte Eigenschaft für jeden Parameter der Effekt übergeben werden soll.
1. Hinzufügen einer zusätzlichen angefügten Eigenschaft auf die Klasse, die verwendet wird, zum Steuern der hinzufügen oder Entfernen von der Auswirkung auf das Steuerelement, dem die Klasse an angefügt wird. Stellen Sie sicher, dass diese Eigenschaft Register angefügt eine `propertyChanged` Delegat, der ausgeführt wird, wenn der Wert der Eigenschaft ändert.
1. Erstellen Sie `static` Getter und Setter für jede angefügte Eigenschaft.
1. Implementieren Sie die Logik in der `propertyChanged` Delegaten hinzufügen und entfernen Sie den Effekt.
1. Implementieren Sie eine geschachtelte Klasse innerhalb der `static` Klasse, mit dem Namen nach dem die Auswirkungen, die Unterklassen der `RoutingEffect` Klasse. Rufen Sie für den Konstruktor der Basisklasse-Konstruktors und übergibt eine Verkettung der Name der Lösung und die eindeutige ID, die für jede Clientplattform-spezifische Auswirkungen Klasse angegeben wurde.

Parameter können dann mit Ergebnis übergeben werden, indem Sie das entsprechende Steuerelement angefügte Eigenschaften und Eigenschaftswerte, hinzufügen. Darüber hinaus können Parameter zur Laufzeit geändert werden, indem Sie einen neuen Wert der angefügten Eigenschaft angeben.

> [!NOTE]
> **Hinweis**: eine angefügte Eigenschaft ist eine besondere Art von bindbare Eigenschaft, die in eine Klasse jedoch auf andere Objekte angefügt und erkennbaren in XAML definiert wird, als Attribute, die eine Klasse und einen Eigenschaftsnamen, die durch einen Punkt getrennt enthalten. Weitere Informationen finden Sie unter [angefügte Eigenschaften](~/xamarin-forms/xaml/attached-properties.md).

Die beispielanwendung für veranschaulicht eine `ShadowEffect` , der vom angezeigte Text einen Schatten hinzugefügt eine [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) Steuerelement. Darüber hinaus kann die Farbe des Schattens zur Laufzeit geändert werden. Das folgende Diagramm veranschaulicht die Zuständigkeiten aller Projekte in der beispielanwendung, sowie die Beziehungen zwischen ihnen:

![](attached-properties-images/shadow-effect.png "Schatten Auswirkungen Projekt Zuständigkeiten")

Ein [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) control für die `HomePage` angepasst wird, indem die `LabelShadowEffect` in jedem Projekt plattformspezifischen. Parameter werden für jede übergeben `LabelShadowEffect` über angefügte Eigenschaften in der `ShadowEffect` Klasse. Jede `LabelShadowEffect` Klasse leitet sich von der `PlatformEffect` Klasse für jede Plattform. Dies führt zu einem Schatten angezeigten Elementtexts hinzugefügt wird die `Label` zu steuern, wie in den folgenden Screenshots dargestellt:

![](attached-properties-images/screenshots.png "Schatteneffekt auf jeder Plattform")

## <a name="creating-effect-parameters"></a>Erstellen von Effekt-Parametern

Ein `static` Klasse sollte zur Darstellung von Effekt-Parameter erstellt werden, wie im folgenden Codebeispiel wird veranschaulicht:

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

Die `ShadowEffect` enthält fünf angefügte Eigenschaften, mit `static` Getter und Setter für jede angefügte Eigenschaft. Vier dieser Eigenschaften darstellen, jede Clientplattform-spezifische zu übergebenden Parameter an `LabelShadowEffect`. Die `ShadowEffect` -Klasse definiert außerdem eine `HasShadow` -Eigenschaft, die verwendet wird, um das Hinzufügen oder Entfernen von den Auswirkungen auf das Steuerelement zu steuern, die die `ShadowEffect` Klasse angefügt ist. Dies angefügte Eigenschaft registriert die `OnHasShadowChanged` Methode, die ausgeführt wird, wenn der Wert der Eigenschaft ändert. Diese Methode fügt hinzu oder entfernt die Auswirkungen, die basierend auf dem Wert von der `HasShadow` -Eigenschaft.

Die geschachtelte `LabelShadowEffect` Klasse, die Unterklassen der [ `RoutingEffect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.RoutingEffect/) Klasse unterstützt Effekt hinzufügen und entfernen. Die `RoutingEffect` Klasse stellt einen plattformunabhängigen Effekt, der einen inneren Effekt umschließt, die in der Regel plattformspezifischen ist. Dies vereinfacht das Effekt entfernen, da es keinen Zeitpunkt der Kompilierung Zugriff auf die Typinformationen für eine plattformspezifische Auswirkung gibt. Die `LabelShadowEffect` Konstruktor ruft die Basisklasse-Konstruktors und übergibt einen Parameter, bestehend aus einer Verkettung der Name der Lösung und die eindeutige ID, die für jede Clientplattform-spezifische Auswirkungen Klasse angegeben wurde. Dadurch können Auswirkungen Hinzufügung und Entfernung in der `OnHasShadowChanged` -Methode wie folgt:

- **Außerdem Auswirkungen** – eine neue Instanz der der `LabelShadowEffect` das Steuerelement hinzugefügt wird [ `Effects` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.Effects/) Auflistung. Dies ersetzt mithilfe der [ `Effect.Resolve` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Effect.Resolve/p/System.String/) Methode, um die Auswirkung hinzuzufügen.
- **Auswirkung der Entfernung** – die erste Instanz von der `LabelShadowEffect` in des Steuerelements [ `Effects` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.Effects/) Auflistung abgerufen und entfernt.

## <a name="consuming-the-effect"></a>Nutzen die Auswirkung

Jede Clientplattform-spezifische `LabelShadowEffect` genutzt werden können, durch den angefügten Eigenschaften zum Hinzufügen einer [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) zu steuern, wie im folgenden Beispiel der Verwendung von XAML-Code veranschaulicht:

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

ShadowEffect.SetHasShadow (label, true);
ShadowEffect.SetRadius (label, 5);
ShadowEffect.SetDistanceX (label, 5);
ShadowEffect.SetDistanceY (label, 5);
ShadowEffect.SetColor (label, color));
```

Festlegen der `ShadowEffect.HasShadow` angefügten Eigenschaft, um `true` führt die `ShadowEffect.OnHasShadowChanged` Methode, die hinzugefügt oder entfernt die `LabelShadowEffect` auf die [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) Steuerelement. In beiden Codebeispielen die `ShadowEffect.Color` angefügte Eigenschaft enthält plattformspezifische Farbwerte anzeigt. Weitere Informationen finden Sie unter [Geräteklasse](~/xamarin-forms/platform/device.md).

Darüber hinaus eine [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) ermöglicht die Schattenfarbe zur Laufzeit geändert werden. Wenn die `Button` geklickt haben, werden die folgenden codeänderungen Schattens Farbe durch Festlegen der `ShadowEffect.Color` -Eigenschaft:

```csharp
ShadowEffect.SetColor (label, Color.Teal);
```

### <a name="consuming-the-effect-with-a-style"></a>Nutzen den Effekt mit einer Formatvorlage

Effekte, die genutzt werden können, indem Sie ein Steuerelement angefügte Eigenschaften hinzufügen, können auch durch einen Stil genutzt werden. Das folgende Beispiel zeigt für die Verwendung von XAML-Code ein *explizite* Stil für den Schatteneffekt, der übernommen werden kann [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) Steuerelemente:

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

Die [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) angewendet werden können, um eine [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) durch Festlegen seiner [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) Eigenschaft, um die `Style` -Zielinstanz aus, die `StaticResource`Markuperweiterung, wie im folgenden Codebeispiel gezeigt:

```xaml
<Label Text="Label Shadow Effect" ... Style="{StaticResource ShadowEffectStyle}" />
```

Weitere Informationen zu Stilen finden Sie unter [Stile](~/xamarin-forms/user-interface/styles/index.md).

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

Die `OnAttached` Methodenaufrufe Methoden zum Abrufen der Werte der angefügten Eigenschaft, die mit der `ShadowEffect` Getter und die festgelegt `Control.Layer` Eigenschaften die Eigenschaftswerte an den Schatten zu erstellen. Diese Funktion umschlossen ist ein `try` / `catch` blockieren, für den Fall, dass das Steuerelement, das der Effekt angefügt ist keine der `Control.Layer` Eigenschaften. Keine Implementierung erfolgt durch die `OnDetached` Methode, da kein Cleanup erforderlich ist.

#### <a name="responding-to-property-changes"></a>Reagieren auf Eigenschaftenänderungen

Wenn keines der `ShadowEffect` angefügte Eigenschaft Werte ändern zur Laufzeit den Effekt Anforderungen reagieren, indem Sie die Änderungen anzeigen. Eine überschriebene Version von den `OnElementPropertyChanged` Methode, in der Klasse plattformspezifischen Auswirkungen bietet die Möglichkeit zum Beantworten bindbare Eigenschaft geändert wird, aus, wie im folgenden Codebeispiel wird veranschaulicht:

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

Die `OnElementPropertyChanged` -Methode aktualisiert den Radius, Farbe oder Offset des Schattens bereitgestellt, die auf den entsprechenden `ShadowEffect` Wert der angefügten Eigenschaft geändert wurde. Ein Kontrollkästchen für die Eigenschaft, die geändert werden sollten immer vorgenommen werden, wie diese Außerkraftsetzung oft aufgerufen werden kann.

### <a name="android-project"></a>Android Project

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

Die `OnAttached` Methodenaufrufe Methoden zum Abrufen der Werte der angefügten Eigenschaft, die mit der `ShadowEffect` Getter, und ruft eine Methode, die aufruft der [ `TextView.SetShadowLayer` ](https://developer.xamarin.com/api/member/Android.Widget.TextView.SetShadowLayer/p/System.Single/System.Single/System.Single/Android.Graphics.Color/) Methode, um einen Schatten mit den Eigenschaftswerten erstellen. Diese Funktion umschlossen ist ein `try` / `catch` blockieren, für den Fall, dass das Steuerelement, das der Effekt angefügt ist keine der `Control.Layer` Eigenschaften. Keine Implementierung erfolgt durch die `OnDetached` Methode, da kein Cleanup erforderlich ist.

#### <a name="responding-to-property-changes"></a>Reagieren auf Eigenschaftenänderungen

Wenn keines der `ShadowEffect` angefügte Eigenschaft Werte ändern zur Laufzeit den Effekt Anforderungen reagieren, indem Sie die Änderungen anzeigen. Eine überschriebene Version von den `OnElementPropertyChanged` Methode, in der Klasse plattformspezifischen Auswirkungen bietet die Möglichkeit zum Beantworten bindbare Eigenschaft geändert wird, aus, wie im folgenden Codebeispiel wird veranschaulicht:

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

Die `OnElementPropertyChanged` -Methode aktualisiert den Radius, Farbe oder Offset des Schattens bereitgestellt, die auf den entsprechenden `ShadowEffect` Wert der angefügten Eigenschaft geändert wurde. Ein Kontrollkästchen für die Eigenschaft, die geändert werden sollten immer vorgenommen werden, wie diese Außerkraftsetzung oft aufgerufen werden kann.

### <a name="windows-phone--universal-windows-platform-projects"></a>Windows Phone & universelle Windows-Plattformprojekten

Das folgende Codebeispiel zeigt die `LabelShadowEffect` Implementierung für die Projekte für Windows Phone und universelle Windows-Plattform (UWP):

```csharp
[assembly: ResolutionGroupName ("MyCompany")]
[assembly: ExportEffect (typeof(LabelShadowEffect), "LabelShadowEffect")]
namespace EffectsDemo.WinPhone81
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

Windows-Runtime und universelle Windows-Plattform einen Schatteneffekt stellen keine muss das `LabelShadowEffect` Implementierung auf beiden Plattformen simuliert ein Datum durch Hinzufügen eines zweiten Offsets [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) hinter dem primären `Label`. Die `OnAttached` Methode erstellt den neuen `Label` und einige Eigenschaften für das Layout für die `Label`. Er ruft dann die Methoden zum Abrufen der Werte der angefügten Eigenschaft, die mit der `ShadowEffect` Getter, und die Schattenkopie erstellt, durch Festlegen der [ `TextColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.TextColor/), [ `TranslationX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationX/), und [ `TranslationY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationY/) zu steuern, die Farbe und den Speicherort der Eigenschaften der `Label`. Die `shadowLabel` wird eingefügt hinter dem primären offset `Label`. Diese Funktion umschlossen ist ein `try` / `catch` blockieren, für den Fall, dass das Steuerelement, das der Effekt angefügt ist keine der `Control.Layer` Eigenschaften. Keine Implementierung erfolgt durch die `OnDetached` Methode, da kein Cleanup erforderlich ist.

#### <a name="responding-to-property-changes"></a>Reagieren auf Eigenschaftenänderungen

Wenn keines der `ShadowEffect` angefügte Eigenschaft Werte ändern zur Laufzeit den Effekt Anforderungen reagieren, indem Sie die Änderungen anzeigen. Eine überschriebene Version von den `OnElementPropertyChanged` Methode, in der Klasse plattformspezifischen Auswirkungen bietet die Möglichkeit zum Beantworten bindbare Eigenschaft geändert wird, aus, wie im folgenden Codebeispiel wird veranschaulicht:

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

Die `OnElementPropertyChanged` Methode aktualisiert die Farbe oder den Offset des Schattens bereitgestellt, die auf den entsprechenden `ShadowEffect` Wert der angefügten Eigenschaft geändert wurde. Ein Kontrollkästchen für die Eigenschaft, die geändert werden sollten immer vorgenommen werden, wie diese Außerkraftsetzung oft aufgerufen werden kann.

## <a name="summary"></a>Zusammenfassung

Dieser Artikel hat gezeigt, mithilfe von Eigenschaften zum Übergeben von Parametern eines Effekts, und ändern einen Parameter zur Laufzeit zugeordnet. Angefügte Eigenschaften können verwendet werden, Auswirkungen Parameter zu definieren, die auf Änderungen zur Laufzeit reagieren.


## <a name="related-links"></a>Verwandte Links

- [Benutzerdefinierte Renderer](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
- [Auswirkung](https://developer.xamarin.com/api/type/Xamarin.Forms.Effect/)
- [PlatformEffect](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformEffect%3CTContainer,TControl%3E/)
- [RoutingEffect](https://developer.xamarin.com/api/type/Xamarin.Forms.RoutingEffect/)
- [Schatten (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/effects/shadoweffectruntimechange/)
