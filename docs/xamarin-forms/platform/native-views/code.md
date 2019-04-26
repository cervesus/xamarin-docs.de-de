---
title: Native Ansichten in c#
description: Native Ansichten von iOS-, Android- und UWP können direkt von Xamarin.Forms-Seiten, die mit c# erstellte verwiesen werden. In diesem Artikel wird veranschaulicht, wie eine Xamarin.Forms-Layout mit c# erstellte native Ansichten hinzugefügt, und wie Sie das Layout der benutzerdefinierte Ansichten, korrigieren Sie die Messung API-Nutzung außer Kraft zu setzen.
ms.prod: xamarin
ms.assetid: 230F937C-F914-4B21-8EA1-1A2A9E644769
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/27/2016
ms.openlocfilehash: 8587cade1c5b4a6882f21603ee869f94f38fd04a
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61086004"
---
# <a name="native-views-in-c"></a>Native Ansichten in c#

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/NativeEmbedding/)

_Native Ansichten von iOS-, Android- und UWP können direkt von Xamarin.Forms-Seiten, die mit c# erstellte verwiesen werden. In diesem Artikel wird veranschaulicht, wie eine Xamarin.Forms-Layout mit c# erstellte native Ansichten hinzugefügt, und wie Sie das Layout der benutzerdefinierte Ansichten, korrigieren Sie die Messung API-Nutzung außer Kraft zu setzen._

## <a name="overview"></a>Übersicht

Alle Xamarin.Forms-Steuerelement, das ermöglicht `Content` festgelegt werden, oder mit einem `Children` Auflistung können plattformspezifische Ansichten hinzufügen. Z. B. eine iOS `UILabel` können direkt hinzugefügt werden die [ `ContentView.Content` ](xref:Xamarin.Forms.ContentView.Content) -Eigenschaft oder die [ `StackLayout.Children` ](xref:Xamarin.Forms.Layout`1.Children) Auflistung. Beachten Sie jedoch, dass diese Funktion erfordert `#if` definiert in Xamarin.Forms freigegebenes Projekt Lösungen und Lösungen für Xamarin.Forms .NET Standard-Bibliothek verfügbar ist.

Die folgenden Screenshots veranschaulichen plattformspezifische hinzugefügt, um eine Xamarin.Forms-Ansichten [ `StackLayout` ](xref:Xamarin.Forms.StackLayout):

[![](code-images/screenshots-sml.png "Plattformspezifische Ansichten mit StackLayout")](code-images/screenshots.png#lightbox "StackLayout mit plattformspezifischen Ansichten")

Die Möglichkeit, eine Xamarin.Forms-Layouts plattformspezifische Ansichten hinzufügen, wird durch zwei Erweiterungsmethoden auf jeder Plattform aktiviert:

- `Add` – Fügt eine plattformspezifische-Ansicht für die [ `Children` ](xref:Xamarin.Forms.Layout`1.Children) Auflistung eines Layouts.
- `ToView` – wird eine bestimmte Ansicht und umschließt ihn als eine Xamarin.Forms [ `View` ](xref:Xamarin.Forms.View) , die festgelegt werden können, als die `Content` Eigenschaft eines Steuerelements.

Mithilfe dieser Methoden in einem freigegebenen Xamarin.Forms-Projekt benötigt den entsprechenden plattformspezifischen Xamarin.Forms-Namespace importiert:

- **iOS** – Xamarin.Forms.Platform.iOS
- **Android** – Xamarin.Forms.Platform.Android
- **Universelle Windows-Plattform (UWP)** : Xamarin.Forms.Platform.UWP

## <a name="adding-platform-specific-views-on-each-platform"></a>Hinzufügen von plattformspezifischen Ansichten auf jeder Plattform

Die folgenden Abschnitte zeigen, wie eine Xamarin.Forms-Layouts auf jeder Plattform plattformspezifische Ansichten hinzugefügt wird.

### <a name="ios"></a>iOS

Im folgenden Codebeispiel wird veranschaulicht, wie Hinzufügen einer `UILabel` auf eine [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) und [ `ContentView` ](xref:Xamarin.Forms.ContentView):

```csharp
var uiLabel = new UILabel {
  MinimumFontSize = 14f,
  Lines = 0,
  LineBreakMode = UILineBreakMode.WordWrap,
  Text = originalText,
};
stackLayout.Children.Add (uiLabel);
contentView.Content = uiLabel.ToView();
```

Im Beispiel wird vorausgesetzt, dass die `stackLayout` und `contentView` Instanzen zuvor in XAML oder c# erstellt wurden.

### <a name="android"></a>Android

Im folgenden Codebeispiel wird veranschaulicht, wie Hinzufügen einer `TextView` auf eine [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) und [ `ContentView` ](xref:Xamarin.Forms.ContentView):

```csharp
var textView = new TextView (MainActivity.Instance) { Text = originalText, TextSize = 14 };
stackLayout.Children.Add (textView);
contentView.Content = textView.ToView();
```

Im Beispiel wird vorausgesetzt, dass die `stackLayout` und `contentView` Instanzen zuvor in XAML oder c# erstellt wurden.

### <a name="universal-windows-platform"></a>Universelle Windows-Plattform

Im folgenden Codebeispiel wird veranschaulicht, wie Hinzufügen einer `TextBlock` auf eine [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) und [ `ContentView` ](xref:Xamarin.Forms.ContentView):

```csharp
var textBlock = new TextBlock
{
    Text = originalText,
    FontSize = 14,
    FontFamily = new FontFamily("HelveticaNeue"),
    TextWrapping = TextWrapping.Wrap
};
stackLayout.Children.Add(textBlock);
contentView.Content = textBlock.ToView();
```

Im Beispiel wird vorausgesetzt, dass die `stackLayout` und `contentView` Instanzen zuvor in XAML oder c# erstellt wurden.

## <a name="overriding-platform-measurements-for-custom-views"></a>Überschreiben die Eigenschaften der Plattform für benutzerdefinierte Ansichten

Benutzerdefinierte Ansichten auf jeder Plattform implementieren häufig nur ordnungsgemäß Maßeinheit für das Layoutszenario für das sie entwickelt wurden. Z. B. eine benutzerdefinierte Ansicht möglicherweise ausgelegt sind, nur die Hälfte der verfügbaren Breite des Geräts einnimmt. Nach dem für andere Benutzer freigegeben wird, kann die benutzerdefinierte Ansicht jedoch belegt die vollständigen verfügbare Breite des Geräts erforderlich sein. Aus diesem Grund kann es erforderlich, eine Messung-Implementierung von benutzerdefinierten Ansichten zu überschreiben, wenn in einer Xamarin.Forms-Layouts wiederverwendeten sein. Aus diesem Grund die `Add` und `ToView` Erweiterungsmethoden Geben Sie Außerkraftsetzungen, die Messung Delegaten angegeben werden, ermöglichen es, die das Layout für die benutzerdefinierte Ansicht überschreiben können, wenn es zu einer Xamarin.Forms-Layout hinzugefügt wird.

Die folgenden Abschnitte zeigen, wie Sie das Layout der benutzerdefinierten Ansichten, korrigieren Sie die Messung API-Nutzung außer Kraft zu setzen.

### <a name="ios"></a>iOS

Das folgende Codebeispiel zeigt die `CustomControl` -Klasse, die erbt `UILabel`:

```csharp
public class CustomControl : UILabel
{
  public override string Text {
    get { return base.Text; }
    set { base.Text = value.ToUpper (); }
  }

  public override CGSize SizeThatFits (CGSize size)
  {
    return new CGSize (size.Width, 150);
  }
}
```

Eine Instanz der in dieser Ansicht wird hinzugefügt, um eine [ `StackLayout` ](xref:Xamarin.Forms.StackLayout), wie im folgenden Codebeispiel gezeigt:

```csharp
var customControl = new CustomControl {
  MinimumFontSize = 14,
  Lines = 0,
  LineBreakMode = UILineBreakMode.WordWrap,
  Text = "This control has incorrect sizing - there's empty space above and below it."
};
stackLayout.Children.Add (customControl);
```

Aber da die `CustomControl.SizeThatFits` Außerkraftsetzung gibt immer eine Höhe von 150 zurück, die Ansicht mit Leerraum oberhalb und unterhalb, wie im folgenden Screenshot dargestellt angezeigt:

![](code-images/ios-bad-measurement.png "iOS CustomControl mit ungültigen SizeThatFits-Implementierung")

Eine Lösung für dieses Problem besteht darin, geben Sie einen `GetDesiredSizeDelegate` Implementierung wie im folgenden Codebeispiel gezeigt:

```csharp
SizeRequest? FixSize (NativeViewWrapperRenderer renderer, double width, double height)
{
  var uiView = renderer.Control;

  if (uiView == null) {
    return null;
  }

  var constraint = new CGSize (width, height);

  // Let the CustomControl determine its size (which will be wrong)
  var badRect = uiView.SizeThatFits (constraint);

  // Use the width and substitute the height
  return new SizeRequest (new Size (badRect.Width, 70));
}
```

Diese Methode verwendet die Breite von bereitgestellten der `CustomControl.SizeThatFits` -Methode, ersetzt jedoch die Höhe von 150 für eine Höhe von 70. Bei der `CustomControl` Instanz hinzugefügt der [ `StackLayout` ](xref:Xamarin.Forms.StackLayout), wird die `FixSize` Methode kann angegeben werden, als die `GetDesiredSizeDelegate` , beheben Sie die fehlerhafte Messung von bereitgestellten der `CustomControl` Klasse:

```csharp
stackLayout.Children.Add (customControl, FixSize);
```

Dies führt in der benutzerdefinierten Ansicht ohne Leerraum oberhalb und unterhalb, ordnungsgemäß angezeigt wird, wie im folgenden Screenshot gezeigt:

![](code-images/ios-good-measurement.png "iOS CustomControl mit GetDesiredSize außer Kraft setzen")

### <a name="android"></a>Android

Das folgende Codebeispiel zeigt die `CustomControl` -Klasse, die erbt `TextView`:

```csharp
public class CustomControl : TextView
{
  public CustomControl (Context context) : base (context)
  {
  }

  protected override void OnMeasure (int widthMeasureSpec, int heightMeasureSpec)
  {
    int width = MeasureSpec.GetSize (widthMeasureSpec);

    // Force the width to half of what's been requested.
    // This is deliberately wrong to demonstrate providing an override to fix it with.
    int widthSpec = MeasureSpec.MakeMeasureSpec (width / 2, MeasureSpec.GetMode (widthMeasureSpec));

    base.OnMeasure (widthSpec, heightMeasureSpec);
  }
}
```

Eine Instanz der in dieser Ansicht wird hinzugefügt, um eine [ `StackLayout` ](xref:Xamarin.Forms.StackLayout), wie im folgenden Codebeispiel gezeigt:

```csharp
var customControl = new CustomControl (MainActivity.Instance) {
  Text = "This control has incorrect sizing - it doesn't occupy the available width of the device.",
  TextSize = 14
};
stackLayout.Children.Add (customControl);
```

Aber da die `CustomControl.OnMeasure` außer Kraft setzen immer die Hälfte der die angeforderte Breite zurück, die Ansicht angezeigt, belegt nur die Hälfte die verfügbare Breite des Geräts, wie im folgenden Screenshot gezeigt:

![](code-images/android-bad-measurement.png "Android CustomControl mit ungültigen OnMeasure-Implementierung")

Eine Lösung für dieses Problem besteht darin, geben Sie einen `GetDesiredSizeDelegate` Implementierung wie im folgenden Codebeispiel gezeigt:

```csharp
SizeRequest? FixSize (NativeViewWrapperRenderer renderer, int widthConstraint, int heightConstraint)
{
  var nativeView = renderer.Control;

  if ((widthConstraint == 0 && heightConstraint == 0) || nativeView == null) {
    return null;
  }

  int width = Android.Views.View.MeasureSpec.GetSize (widthConstraint);
  int widthSpec = Android.Views.View.MeasureSpec.MakeMeasureSpec (
    width * 2, Android.Views.View.MeasureSpec.GetMode (widthConstraint));
  nativeView.Measure (widthSpec, heightConstraint);
  return new SizeRequest (new Size (nativeView.MeasuredWidth, nativeView.MeasuredHeight));
}
```

Diese Methode verwendet die Breite von bereitgestellten der `CustomControl.OnMeasure` -Methode, aber multipliziert zwei. Bei der `CustomControl` Instanz hinzugefügt der [ `StackLayout` ](xref:Xamarin.Forms.StackLayout), wird die `FixSize` Methode kann angegeben werden, als die `GetDesiredSizeDelegate` , beheben Sie die fehlerhafte Messung von bereitgestellten der `CustomControl` Klasse:

```csharp
stackLayout.Children.Add (customControl, FixSize);
```

Dadurch wird die benutzerdefinierte Ansicht wird korrekt angezeigt werden, belegt die Breite des Geräts, wie im folgenden Screenshot gezeigt:

![](code-images/android-good-measurement.png "Android CustomControl mit benutzerdefinierten GetDesiredSize Delegaten")

### <a name="universal-windows-platform"></a>Universelle Windows-Plattform

Das folgende Codebeispiel zeigt die `CustomControl` -Klasse, die erbt `Panel`:

```csharp
public class CustomControl : Panel
{
  public static readonly DependencyProperty TextProperty =
    DependencyProperty.Register(
      "Text", typeof(string), typeof(CustomControl), new PropertyMetadata(default(string), OnTextPropertyChanged));

  public string Text
  {
    get { return (string)GetValue(TextProperty); }
    set { SetValue(TextProperty, value.ToUpper()); }
  }

  readonly TextBlock textBlock;

  public CustomControl()
  {
    textBlock = new TextBlock
    {
      MinHeight = 0,
      MaxHeight = double.PositiveInfinity,
      MinWidth = 0,
      MaxWidth = double.PositiveInfinity,
      FontSize = 14,
      TextWrapping = TextWrapping.Wrap,
      VerticalAlignment = VerticalAlignment.Center
    };

    Children.Add(textBlock);
  }

  static void OnTextPropertyChanged(DependencyObject dependencyObject, DependencyPropertyChangedEventArgs args)
  {
    ((CustomControl)dependencyObject).textBlock.Text = (string)args.NewValue;
  }

  protected override Size ArrangeOverride(Size finalSize)
  {
      // This is deliberately wrong to demonstrate providing an override to fix it with.
      textBlock.Arrange(new Rect(0, 0, finalSize.Width/2, finalSize.Height));
      return finalSize;
  }

  protected override Size MeasureOverride(Size availableSize)
  {
      textBlock.Measure(availableSize);
      return new Size(textBlock.DesiredSize.Width, textBlock.DesiredSize.Height);
  }
}
```

Eine Instanz der in dieser Ansicht wird hinzugefügt, um eine [ `StackLayout` ](xref:Xamarin.Forms.StackLayout), wie im folgenden Codebeispiel gezeigt:

```csharp
var brokenControl = new CustomControl {
  Text = "This control has incorrect sizing - it doesn't occupy the available width of the device."
};
stackLayout.Children.Add(brokenControl);
```

Aber da die `CustomControl.ArrangeOverride` außer Kraft setzen immer die Hälfte der die angeforderte Breite zurück, die Ansicht wird auf die Hälfte der verfügbaren Breite des Geräts, abgeschnitten werden, wie im folgenden Screenshot gezeigt:

![](code-images/winrt-bad-measurement.png "UWP-CustomControl mit ungültigen ArrangeOverride-Implementierung")

Eine Lösung für dieses Problem besteht darin, geben Sie eine `ArrangeOverrideDelegate` Implementierung, die beim Hinzufügen der Ansicht, um die [ `StackLayout` ](xref:Xamarin.Forms.StackLayout), wie im folgenden Codebeispiel wird veranschaulicht:

```csharp
stackLayout.Children.Add(fixedControl, arrangeOverrideDelegate: (renderer, finalSize) =>
{
    if (finalSize.Width <= 0 || double.IsInfinity(finalSize.Width))
    {
        return null;
    }
    var frameworkElement = renderer.Control;
    frameworkElement.Arrange(new Rect(0, 0, finalSize.Width * 2, finalSize.Height));
    return finalSize;
});
```

Diese Methode verwendet die Breite von bereitgestellten der `CustomControl.ArrangeOverride` -Methode, aber multipliziert zwei. Dadurch wird die benutzerdefinierte Ansicht wird korrekt angezeigt werden, belegt die Breite des Geräts, wie im folgenden Screenshot gezeigt:

![](code-images/winrt-good-measurement.png "UWP-CustomControl mit ArrangeOverride-Delegaten")

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wird erläutert, wie eine Xamarin.Forms-Layout mit c# erstellte native Ansichten hinzugefügt, und wie Sie das Layout der benutzerdefinierte Ansichten, korrigieren Sie die Messung API-Nutzung außer Kraft zu setzen.


## <a name="related-links"></a>Verwandte Links

- [NativeEmbedding (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/NativeEmbedding/)
- [Native Formulare](~/xamarin-forms/platform/native-forms.md)
