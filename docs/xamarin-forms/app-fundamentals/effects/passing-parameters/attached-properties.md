---
title: Übergeben von Effect-Parametern als angefügte Eigenschaften
description: Angefügte Eigenschaften können verwendet werden, um Effect-Parameter zu definieren, die auf Änderungen der Runtimeeigenschaften reagieren. In diesem Artikel wird veranschaulicht, wie Sie angefügte Eigenschaften verwenden können, um Parameter an einen Effekt zu übergeben, und wie Sie einen Parameter zur Laufzeit anpassen können.
ms.prod: xamarin
ms.assetid: DFCDCB9F-17DD-4117-BD53-B4FB206BB387
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/05/2016
ms.openlocfilehash: d40e1657eb39543023490892b8765ee1fe956ec4
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68645374"
---
# <a name="passing-effect-parameters-as-attached-properties"></a>Übergeben von Effect-Parametern als angefügte Eigenschaften

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/effects-shadoweffectruntimechange)

_Angefügte Eigenschaften können verwendet werden, um Effect-Parameter zu definieren, die auf Änderungen der Runtimeeigenschaften reagieren. In diesem Artikel wird veranschaulicht, wie Sie angefügte Eigenschaften verwenden können, um Parameter an einen Effekt zu übergeben, und wie Sie einen Parameter zur Laufzeit anpassen können._

Effect-Parameter, die auf die Änderungen von Runtimeeigenschaften reagieren, werden folgendermaßen erstellt:

1. Erstellen Sie eine `static`-Klasse, die eine angefügte Eigenschaft für jeden Parameter enthält, die an den Effekt übergeben werden soll.
1. Fügen Sie der Klasse eine zusätzliche angefügte Eigenschaft hinzu, die steuert, ob der Effekt dem Steuerelement hinzugefügt oder von diesem entfernt wird, an das die Klasse angefügt werden soll. Stellen Sie sicher, dass die angefügte Eigenschaft einen `propertyChanged`-Delegaten registriert, der ausgeführt wird, wenn der Wert der Eigenschaft geändert wird.
1. Erstellen Sie `static`-Getter und -Setter für jede angefügte Eigenschaft.
1. Implementieren Sie Logik zum Hinzufügen und Entfernen des Effekts im `propertyChanged`-Delegaten.
1. Implementieren Sie eine geschachtelte Klasse innerhalb der `static`-Klasse, die nach dem Effekt benannt ist, der als Unterklasse der `RoutingEffect`-Klasse fungiert. Rufen Sie den Basisklassenkonstruktor für den Konstruktor auf, der eine Verkettung des Namens der Auflösungsgruppe übergibt, und die eindeutige ID, die in jeder plattformspezifischen Effect-Klasse angegeben wurde.

Parameter können dann an den Effekt übergeben werden, indem die angefügten Eigenschaften und die Eigenschaftswerte an das entsprechende Steuerelement übergeben werden. Parameter können außerdem zur Laufzeit geändert werden, indem ein neuer Wert für die angefügte Eigenschaft festgelegt wird.

> [!NOTE]
> Eine angefügte Eigenschaft stellt eine besondere Form von bindbaren Eigenschaften dar. Sie wird in einer Klasse definiert, jedoch an andere Objekte angefügt, und ist in XAML als Attribut erkennbar, das eine Klasse und einen Eigenschaftsnamen enthält, die durch einen Punkt voneinander getrennt sind. Weitere Informationen finden Sie unter [Angefügte Eigenschaften](~/xamarin-forms/xaml/attached-properties.md).

Die Beispielanwendung veranschaulicht eine `ShadowEffect`-Klasse, die einen Schatten zu dem Text hinzufügt, der vom [`Label`](xref:Xamarin.Forms.Label)-Steuerelement angezeigt wird. Die Farbe des Schattens kann darüber hinaus zur Laufzeit geändert werden. Das folgende Diagramm veranschaulicht die Zuständigkeiten jedes Projekts in der Beispielanwendung sowie deren Beziehungen zueinander:

![](attached-properties-images/shadow-effect.png "Projektzuständigkeiten des Schatteneffekts")

Ein [`Label`](xref:Xamarin.Forms.Label)-Steuerelement im `HomePage`-Element wird in jedem plattformspezifischen Projekt von `LabelShadowEffect` angepasst. Parameter werden über angefügte Eigenschaften in der `ShadowEffect`-Klasse an jede `LabelShadowEffect`-Klasse übergeben. Jede `LabelShadowEffect`-Klasse wird von der `PlatformEffect`-Klasse für jede Plattform abgeleitet. Dadurch wird wie in den folgenden Screenshots dargestellt ein Schatten zu dem Text hinzugefügt, der vom `Label`-Steuerelement angezeigt wird:

![](attached-properties-images/screenshots.png "Schatteneffekt auf den verschiedenen Plattformen")

## <a name="creating-effect-parameters"></a>Erstellen von Effect-Parametern

Eine `static`-Klasse sollte wie im folgenden Codebeispiel gezeigt zur Darstellung von Effect-Parametern erstellt werden:

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

Die `ShadowEffect`-Klasse enthält fünf angefügte Eigenschaften mit `static`-Gettern und -Settern für jede angefügte Eigenschaft. Vier dieser Eigenschaften stellen die Parameter dar, die an jede plattformspezifischen `LabelShadowEffect`-Klasse übergeben werden sollen. Die `ShadowEffect`-Klasse definiert darüber hinaus eine angefügte Eigenschaft `HasShadow`, die steuert, ob der Effekt dem Steuerelement hinzugefügt oder von diesem entfernt wird, an das die `ShadowEffect`-Klasse angefügt ist. Diese angefügte Eigenschaft registriert die `OnHasShadowChanged`-Methode, die ausgeführt wird, wenn der Wert der Eigenschaft geändert wird. Basierend auf dem Wert der angefügten Eigenschaft `HasShadow` fügt diese Methode den Effekt an oder entfernt ihn.

Die geschachtelte `LabelShadowEffect`-Klasse, die als Unterklasse der [`RoutingEffect`](xref:Xamarin.Forms.RoutingEffect)-Klasse implementiert wird, unterstützt das Hinzufügen und Entfernen von Effekten. Die `RoutingEffect`-Klasse stellt einen plattformunabhängigen Effekt dar, der einen in der Regel plattformspezifischen inneren Effekt umschließt. Dadurch wird das Entfernen eines Effekts vereinfacht, da während der Kompilierzeit nicht auf die Typinformationen für einen plattformspezifischen Effekt zugegriffen werden kann. Der `LabelShadowEffect`-Konstruktor ruft den Basisklassenkonstruktor auf, der einen Parameter bestehend aus einer Verkettung des Namens der Auflösungsgruppe übergibt, und die eindeutige ID, die in jeder plattformspezifischen Effect-Klasse angegeben wurde. Dadurch können Effekte in der `OnHasShadowChanged`-Methode folgendermaßen hinzugefügt oder entfernt werden:

- **Hinzufügen von Effekten:** Eine neue Instanz der `LabelShadowEffect`-Klasse wird zur [`Effects`](xref:Xamarin.Forms.Element.Effects)-Collection des Steuerelements hinzugefügt. Dadurch wird mithilfe der [`Effect.Resolve`](xref:Xamarin.Forms.Effect.Resolve(System.String))-Methode eine Ersetzung durchgeführt, um den Effekt hinzuzufügen.
- **Entfernen von Effekten:** Die erste Instanz der `LabelShadowEffect`-Klasse in der [`Effects`](xref:Xamarin.Forms.Element.Effects)-Collection des Steuerelements wird abgerufen und entfernt.

## <a name="consuming-the-effect"></a>Nutzen des Effekts

Jede plattformspezifische `LabelShadowEffect`-Klasse kann (wie im folgenden XAML-Codebeispiel veranschaulicht) verwendet werden, indem die angefügten Eigenschaften zu einem [`Label`](xref:Xamarin.Forms.Label)-Steuerelement hinzugefügt werden:

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

Das entsprechende [`Label`](xref:Xamarin.Forms.Label)-Steuerelement in C# wird im folgenden Codebeispiel veranschaulicht:

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

Wenn Sie die angefügte Eigenschaft `ShadowEffect.HasShadow` auf `true` festlegen, wird die `ShadowEffect.OnHasShadowChanged`-Methode ausgeführt, die `LabelShadowEffect` zum [`Label`](xref:Xamarin.Forms.Label)-Steuerelement hinzufügt oder aus diesem entfernt. In beiden Codebeispielen enthält die angefügte Eigenschaft `ShadowEffect.Color` plattformspezifische Farbwerte. Weitere Informationen finden Sie unter [Xamarin.Forms Device Class (Xamarin.Forms-Geräteklasse)](~/xamarin-forms/platform/device.md).

Darüber hinaus wird durch [`Button`](xref:Xamarin.Forms.Button) ermöglicht, die Farbe des Schattens zur Laufzeit zu ändern. Wenn Sie auf `Button` klicken, ändert folgender Code die Schattenfarbe, indem die angefügte Eigenschaft `ShadowEffect.Color` festgelegt wird:

```csharp
ShadowEffect.SetColor (label, Color.Teal);
```

### <a name="consuming-the-effect-with-a-style"></a>Nutzen von Effect-Klassen mit einer Style-Klasse

Effect-Klassen können genutzt werden, indem Sie angefügte Eigenschaften zu einem Steuerelement hinzufügen, das auch von einer Formatvorlage verwendet werden kann. In folgendem XAML-Codebeispiel wird eine *explizite* Formatvorlage für den Schatteneffekt veranschaulicht, die auf [`Label`](xref:Xamarin.Forms.Label)-Steuerelemente angewendet werden kann:

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

Die [`Style`](xref:Xamarin.Forms.Style)-Klasse kann auf eine [`Label`](xref:Xamarin.Forms.Label)-Klasse angewendet werden, indem ihre [`Style`](xref:Xamarin.Forms.NavigableElement.Style)-Eigenschaft wie im folgenden Codebeispiel gezeigt mithilfe der Markuperweiterung `StaticResource` für die `Style`-Instanz festgelegt wird:

```xaml
<Label Text="Label Shadow Effect" ... Style="{StaticResource ShadowEffectStyle}" />
```

Weitere Informationen zu Formatvorlagen finden Sie unter [Styles (Formatvorlagen)](~/xamarin-forms/user-interface/styles/index.md).

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

Die `OnAttached`-Methode ruft Methoden auf, die die Werte der angefügten Eigenschaft mithilfe von `ShadowEffect`-Gettern abrufen und `Control.Layer`-Eigenschaften auf die Eigenschaftswerte festlegen, um den Schatten zu erstellen. Diese Funktion wird von einem `try`/`catch`-Block umschlossen, falls das Steuerelement, an das der Effekt angefügt ist, nicht über die `Control.Layer`-Eigenschaften verfügt. Von der `OnDetached`-Methode wird keine Implementierung bereitgestellt, da keine Bereinigung erforderlich ist.

#### <a name="responding-to-property-changes"></a>Reaktion auf Änderungen bei den Eigenschaften

Wenn der Wert der angefügten `ShadowEffect`-Eigenschaft sich zur Laufzeit ändert, muss der Effekt darauf reagieren und die Änderungen darstellen. Auf Änderungen in bindbaren Eigenschaften muss in einer überschriebenen Version der `OnElementPropertyChanged`-Methode in der plattformspezifischen Effect-Klasse reagiert werden. Dies wird in folgendem Codebeispiel dargestellt:

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

Die `OnElementPropertyChanged`-Methode aktualisiert den Radius, die Farbe und das Offset des Schattens, wenn der Wert der entsprechenden angefügten `ShadowEffect`-Eigenschaft sich geändert hat. Eine geänderte Eigenschaft sollte immer überprüft werden, da die Möglichkeit besteht, dass diese Überschreibung häufig aufgerufen wird.

### <a name="android-project"></a>Android-Projekt

Im folgenden Codebeispiel wird die Implementierung von `LabelShadowEffect` für das Android-Projekt veranschaulicht:

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

Die `OnAttached`-Methode ruft Methoden auf, die die Werte der angefügten Eigenschaft mithilfe von `ShadowEffect`-Gettern abrufen. Außerdem wird eine Methode aufgerufen, die die [`TextView.SetShadowLayer`](xref:Android.Widget.TextView.SetShadowLayer*)-Methode aufruft, um einen Schatten auf Grundlage der Eigenschaftswerte zu erstellen. Diese Funktion wird von einem `try`/`catch`-Block umschlossen, falls das Steuerelement, an das der Effekt angefügt ist, nicht über die `Control.Layer`-Eigenschaften verfügt. Von der `OnDetached`-Methode wird keine Implementierung bereitgestellt, da keine Bereinigung erforderlich ist.

#### <a name="responding-to-property-changes"></a>Reaktion auf Änderungen bei den Eigenschaften

Wenn der Wert der angefügten `ShadowEffect`-Eigenschaft sich zur Laufzeit ändert, muss der Effekt darauf reagieren und die Änderungen darstellen. Auf Änderungen in bindbaren Eigenschaften muss in einer überschriebenen Version der `OnElementPropertyChanged`-Methode in der plattformspezifischen Effect-Klasse reagiert werden. Dies wird in folgendem Codebeispiel dargestellt:

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

Die `OnElementPropertyChanged`-Methode aktualisiert den Radius, die Farbe und das Offset des Schattens, wenn der Wert der entsprechenden angefügten `ShadowEffect`-Eigenschaft sich geändert hat. Eine geänderte Eigenschaft sollte immer überprüft werden, da die Möglichkeit besteht, dass diese Überschreibung häufig aufgerufen wird.

### <a name="universal-windows-platform-project"></a>Projekt für die Universelle Windows-Plattform

Das folgende Codebeispiel veranschaulicht die Implementierung von `LabelShadowEffect` für ein Projekt für die Universelle Windows-Plattform (UWP):

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

Die Universelle Windows-Plattform stellt keinen Schatteneffekt zur Verfügung, sodass die Implementierung von `LabelShadowEffect` auf beiden Plattformen einen Effekt erzeugt, indem hinter der primären `Label`-Klasse eine zweite versetzte [`Label`](xref:Xamarin.Forms.Label)-Klasse hinzugefügt wird. Die `OnAttached`-Methode erstellt die neue `Label`-Klasse und legt Layouteigenschaften für `Label` fest. Anschließend ruft diese Methoden auf, die die Werte der angefügten Eigenschaften mithilfe von `ShadowEffect`-Gettern abrufen, und erstellt den Schatten, indem die Eigenschaften [`TextColor`](xref:Xamarin.Forms.Label.TextColor), [`TranslationX`](xref:Xamarin.Forms.VisualElement.TranslationX) und [`TranslationY`](xref:Xamarin.Forms.VisualElement.TranslationY) darauf festgelegt werden, die Farbe und die Position von `Label` zu steuern. Das `shadowLabel`-Element wird dann versetzt hinter der primären `Label`-Klasse eingefügt. Diese Funktion wird von einem `try`/`catch`-Block umschlossen, falls das Steuerelement, an das der Effekt angefügt ist, nicht über die `Control.Layer`-Eigenschaften verfügt. Von der `OnDetached`-Methode wird keine Implementierung bereitgestellt, da keine Bereinigung erforderlich ist.

#### <a name="responding-to-property-changes"></a>Reaktion auf Änderungen bei den Eigenschaften

Wenn der Wert der angefügten `ShadowEffect`-Eigenschaft sich zur Laufzeit ändert, muss der Effekt darauf reagieren und die Änderungen darstellen. Auf Änderungen in bindbaren Eigenschaften muss in einer überschriebenen Version der `OnElementPropertyChanged`-Methode in der plattformspezifischen Effect-Klasse reagiert werden. Dies wird in folgendem Codebeispiel dargestellt:

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

Die `OnElementPropertyChanged`-Methode aktualisiert die Farbe und das Offset des Schattens, wenn der Wert der entsprechenden angefügten `ShadowEffect`-Eigenschaft sich geändert hat. Eine geänderte Eigenschaft sollte immer überprüft werden, da die Möglichkeit besteht, dass diese Überschreibung häufig aufgerufen wird.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde veranschaulicht, wie Sie angefügte Eigenschaften verwenden können, um Parameter an einen Effekt zu übergeben, und wie Sie einen Parameter zur Laufzeit anpassen können. Angefügte Eigenschaften können verwendet werden, um Effect-Parameter zu definieren, die auf Änderungen der Runtimeeigenschaften reagieren.


## <a name="related-links"></a>Verwandte Links

- [Benutzerdefinierte Renderer](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
- [Effect class (Effect-Klasse)](xref:Xamarin.Forms.Effect)
- [PlatformEffect (PlatformEffect-Klasse)](xref:Xamarin.Forms.PlatformEffect`2)
- [RoutingEffect Class (RoutingEffect-Klasse)](xref:Xamarin.Forms.RoutingEffect)
- [Shadow Effect (Schatteneffekt (Beispiel))](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/effects-shadoweffectruntimechange)
