---
title: Systemeigene Ansichten in c#
description: "Systemeigene Ansichten von iOS-, Android- und uwp-können direkt von Xamarin.Forms-Seiten, die erstellt wurden, mithilfe von c# verwiesen werden. In diesem Artikel wird veranschaulicht, wie eine Xamarin.Forms-Layout mit c# erstellten systemeigene Ansichten hinzugefügt und wie das Layout an benutzerdefinierten Ansichten an, korrigieren Sie die Maßeinheit zur Verwendung der API überschrieben."
ms.topic: article
ms.prod: xamarin
ms.assetid: 230F937C-F914-4B21-8EA1-1A2A9E644769
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/27/2016
ms.openlocfilehash: 0c4014ecda0501e9309a17901c439444e4b48e86
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="native-views-in-c"></a>Systemeigene Ansichten in c#

_Systemeigene Ansichten von iOS-, Android- und uwp-können direkt von Xamarin.Forms-Seiten, die erstellt wurden, mithilfe von c# verwiesen werden. In diesem Artikel wird veranschaulicht, wie eine Xamarin.Forms-Layout mit c# erstellten systemeigene Ansichten hinzugefügt und wie das Layout an benutzerdefinierten Ansichten an, korrigieren Sie die Maßeinheit zur Verwendung der API überschrieben._

## <a name="overview"></a>Übersicht

Jedes Xamarin.Forms-Steuerelement, das ermöglicht `Content` , um die festgelegt werden, oder mit einer `Children` Auflistung können plattformspezifische Ansichten hinzufügen. Z. B. eine iOS `UILabel` können nicht direkt hinzugefügt werden die [ `ContentView.Content` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ContentView.Content/) -Eigenschaft, oder auf die [ `StackLayout.Children` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout%3CT%3E.Children/) Auflistung. Beachten Sie jedoch, dass diese Funktionalität über die Verwendung von erfordert `#if` in Xamarin.Forms freigegebenes Projekt Projektmappen definiert und von Xamarin.Forms Portable Klassenbibliothek (PCL)-Projektmappen verfügbar ist.

Führen Sie den folgenden Screenshots vor plattformspezifische Ansichten hinzugefügt, um eine Xamarin.Forms [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/):

[![](code-images/screenshots-sml.png "Enthält plattformspezifische Ansichten StackLayout")](code-images/screenshots.png#lightbox "StackLayout enthält plattformspezifische Ansichten")

Die Möglichkeit zum Hinzufügen von plattformspezifischen Ansichten zu einem Layout Xamarin.Forms wird durch zwei Erweiterungsmethoden auf jeder Plattform aktiviert:

- `Add` – Fügt eine plattformspezifische Ansicht für die [ `Children` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout%3CT%3E.Children/) Auflistung eines Layouts.
- `ToView` – akzeptiert eine plattformspezifische-Sicht und umschließt ihn als einen Xamarin.Forms [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) , die festgelegt werden können, als die `Content` Eigenschaft eines Steuerelements.

Verwenden diese Methoden in einem freigegebenen Xamarin.Forms-Projekt erfordert den entsprechenden plattformspezifischen Xamarin.Forms Namespace importiert:

- **iOS** – Xamarin.Forms.Platform.iOS
- **Android** – Xamarin.Forms.Platform.Android
- **Windows-Runtime** – Xamarin.Forms.Platform.WinRT
- **Universelle Windows-Plattform (UWP)** – Xamarin.Forms.Platform.UWP

## <a name="adding-platform-specific-views-on-each-platform"></a>Hinzufügen von plattformspezifischen Ansichten auf jeder Plattform

Die folgenden Abschnitte zeigen, wie ein Layout Xamarin.Forms auf jeder Plattform plattformspezifischen Sichten hinzugefügt wird.

### <a name="ios"></a>iOS

Das folgende Codebeispiel veranschaulicht das Hinzufügen einer `UILabel` auf eine [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) und ein [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/):

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

Das Beispiel setzt voraus, dass die `stackLayout` und `contentView` Instanzen zuvor in XAML oder c# erstellt wurden.

### <a name="android"></a>Android

Das folgende Codebeispiel veranschaulicht das Hinzufügen einer `TextView` auf eine [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) und ein [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/):

```csharp
var textView = new TextView (MainActivity.Instance) { Text = originalText, TextSize = 14 };
stackLayout.Children.Add (textView);
contentView.Content = textView.ToView();
```

Das Beispiel setzt voraus, dass die `stackLayout` und `contentView` Instanzen zuvor in XAML oder c# erstellt wurden.

### <a name="windows-runtime-and-universal-windows-platform"></a>Windows-Runtime und universelle Windows-Plattform

Das folgende Codebeispiel veranschaulicht das Hinzufügen einer `TextBlock` auf eine [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) und ein [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/):

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

Das Beispiel setzt voraus, dass die `stackLayout` und `contentView` Instanzen zuvor in XAML oder c# erstellt wurden.

## <a name="overriding-platform-measurements-for-custom-views"></a>Überschreiben die Plattform Messungen für benutzerdefinierte Ansichten

Häufig nur korrekt implementieren benutzerdefinierte Ansichten auf jeder Plattform Maßeinheit zur Ermittlung der Layoutszenario für das sie entwickelt wurden. Angenommen, eine benutzerdefinierte Ansicht möglicherweise ausgelegt sind, nur die Hälfte der verfügbaren Breite des Geräts einnehmen. Allerdings ist nach für andere Benutzer freigegeben, die benutzerdefinierte Sicht erforderlich, um die vollständige verfügbare Breite des Geräts belegen möglich. Aus diesem Grund kann es notwendig, eine benutzerdefinierte Ansichten Messung Implementierung überschrieben werden, wenn in einem Layout Xamarin.Forms wiederverwendeten sein. Aus diesem Grund die `Add` und `ToView` Erweiterungsmethoden bieten Außerkraftsetzungen, die Messung Delegaten angegeben werden, mit denen das benutzerdefinierte Ansichtslayout überschreiben können, wenn sie eine Xamarin.Forms-Layout hinzugefügt wird.

In den folgenden Abschnitten veranschaulichen, wie das Layout der benutzerdefinierte Ansichten, korrigieren Sie die Maßeinheit zur Verwendung der API zu überschreiben.

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

Eine Instanz dieser Sicht hinzugefügt wird eine [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/), wie im folgenden Codebeispiel gezeigt:

```csharp
var customControl = new CustomControl {
  MinimumFontSize = 14,
  Lines = 0,
  LineBreakMode = UILineBreakMode.WordWrap,
  Text = "This control has incorrect sizing - there's empty space above and below it."
};
stackLayout.Children.Add (customControl);
```

Jedoch, da die `CustomControl.SizeThatFits` immer Überschreibung eine Höhe von 150, die Ansicht mit den Leerraum oberhalb und unterhalb davon angezeigt, wie im folgenden Screenshot gezeigt:

![](code-images/ios-bad-measurement.png "iOS benutzerdefiniertes Steuerelement mit ungültigen SizeThatFits-Implementierung")

Eine Lösung für dieses Problem besteht darin, geben Sie einen `GetDesiredSizeDelegate` Implementierung, wie im folgenden Codebeispiel gezeigt:

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

Diese Methode verwendet die gebotenen Breite der `CustomControl.SizeThatFits` ersetzt die Methode, aber die Höhe des 150 für eine Höhe von 70. Wenn der `CustomControl` Instanz hinzugefügt der [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/), die `FixSize` Methode kann angegeben werden, als der `GetDesiredSizeDelegate` , korrigieren Sie die fehlerhafte Messung von bereitgestellten der `CustomControl` Klasse:

```csharp
stackLayout.Children.Add (customControl, FixSize);
```

Daraus ergibt sich der benutzerdefinierten Sicht ohne Leerraum oberhalb und unterhalb davon ordnungsgemäß angezeigt wird, wie im folgenden Screenshot gezeigt:

![](code-images/ios-good-measurement.png "iOS benutzerdefiniertes Steuerelement mit GetDesiredSize Außerkraftsetzung")

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

Eine Instanz dieser Sicht hinzugefügt wird eine [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/), wie im folgenden Codebeispiel gezeigt:

```csharp
var customControl = new CustomControl (MainActivity.Instance) {
  Text = "This control has incorrect sizing - it doesn't occupy the available width of the device.",
  TextSize = 14
};
stackLayout.Children.Add (customControl);
```

Jedoch, da die `CustomControl.OnMeasure` immer Überschreibung die Hälfte der Breite des angeforderten, wird die Ansicht angezeigt, belegen nur die Hälfte die verfügbare Breite des Geräts, wie im folgenden Screenshot gezeigt:

![](code-images/android-bad-measurement.png "Android benutzerdefiniertes Steuerelement mit ungültigen OnMeasure-Implementierung")

Eine Lösung für dieses Problem besteht darin, geben Sie einen `GetDesiredSizeDelegate` Implementierung, wie im folgenden Codebeispiel gezeigt:

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

Diese Methode verwendet die gebotenen Breite der `CustomControl.OnMeasure` -Methode, aber multipliziert zwei. Wenn der `CustomControl` Instanz hinzugefügt der [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/), die `FixSize` Methode kann angegeben werden, als der `GetDesiredSizeDelegate` , korrigieren Sie die fehlerhafte Messung von bereitgestellten der `CustomControl` Klasse:

```csharp
stackLayout.Children.Add (customControl, FixSize);
```

Dadurch wird die benutzerdefinierte Ansicht korrekt angezeigt werden, belegen die Breite des Geräts, wie im folgenden Screenshot gezeigt:

![](code-images/android-good-measurement.png "Android benutzerdefiniertes Steuerelement mit dem benutzerdefinierten GetDesiredSize Delegaten")

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

Eine Instanz dieser Sicht hinzugefügt wird eine [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/), wie im folgenden Codebeispiel gezeigt:

```csharp
var brokenControl = new CustomControl {
  Text = "This control has incorrect sizing - it doesn't occupy the available width of the device."
};
stackLayout.Children.Add(brokenControl);
```

Jedoch, da die `CustomControl.ArrangeOverride` immer Überschreibung die Hälfte der Breite des angeforderten, die Sicht auf die Hälfte der verfügbaren Breite des Geräts abgeschnitten, wie im folgenden Screenshot gezeigt:

![](code-images/winrt-bad-measurement.png "Universelle Windows-Plattform benutzerdefiniertes Steuerelement mit ungültigen "ArrangeOverride" Implementierung")

Eine Lösung für dieses Problem besteht darin, geben Sie eine `ArrangeOverrideDelegate` Implementierung, die beim Hinzufügen der Ansicht, um die [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/), wie im folgenden Codebeispiel wird gezeigt:

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

Diese Methode verwendet die gebotenen Breite der `CustomControl.ArrangeOverride` -Methode, aber multipliziert zwei. Dadurch wird die benutzerdefinierte Ansicht korrekt angezeigt werden, belegen die Breite des Geräts, wie im folgenden Screenshot gezeigt:

![](code-images/winrt-good-measurement.png "Universelle Windows-Plattform benutzerdefiniertes Steuerelement mit dem Delegaten "ArrangeOverride"")

## <a name="summary"></a>Zusammenfassung

Dieser Artikel wurde erläutert, wie eine Xamarin.Forms-Layout mit c# erstellten systemeigene Ansichten hinzugefügt und wie das Layout an benutzerdefinierten Ansichten an, korrigieren Sie die Maßeinheit zur Verwendung der API überschrieben.


## <a name="related-links"></a>Verwandte Links

- [NativeEmbedding (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/NativeEmbedding/)
- [Native Formulare](~/xamarin-forms/platform/native-forms.md)
