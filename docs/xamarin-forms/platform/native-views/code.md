---
title: Native Ansichten in C#
description: Native Ansichten von IOS, Android und UWP können direkt von Xamarin.Forms mit c# erstellten Seiten referenziert werden. In diesem Artikel wird veranschaulicht, wie Sie systemeigene Ansichten zu einem Xamarin.Forms mit c# erstellten Layout hinzufügen und wie Sie das Layout von benutzerdefinierten Ansichten überschreiben, um die Verwendung der Messungs-API zu korrigieren.
ms.prod: xamarin
ms.assetid: 230F937C-F914-4B21-8EA1-1A2A9E644769
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/27/2016
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 4cad46bdee1b49c316947bc56bdb69a3b9e9a270
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86938209"
---
# <a name="native-views-in-c"></a>Native Ansichten in C\#

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-nativeembedding)

_Native Ansichten von IOS, Android und UWP können direkt von Xamarin.Forms mit c# erstellten Seiten referenziert werden. In diesem Artikel wird veranschaulicht, wie Sie systemeigene Ansichten zu einem Xamarin.Forms mit c# erstellten Layout hinzufügen und wie Sie das Layout von benutzerdefinierten Ansichten überschreiben, um die Verwendung der Messungs-API zu korrigieren._

## <a name="overview"></a>Übersicht

Jedes Xamarin.Forms Steuerelement, das `Content` festgelegt werden kann oder das über eine Auflistung verfügt `Children` , kann plattformspezifische Ansichten hinzufügen. Beispielsweise kann ein IOS `UILabel` direkt der-Eigenschaft oder der-Auflistung hinzugefügt werden [`ContentView.Content`](xref:Xamarin.Forms.ContentView.Content) [`StackLayout.Children`](xref:Xamarin.Forms.Layout`1.Children) . Beachten Sie jedoch, dass diese Funktionalität die Verwendung von `#if` Definitionen in frei Xamarin.Forms gegebenen Projektlösungen erfordert und nicht in Xamarin.Forms .NET Standard Bibliotheks Lösungen verfügbar ist.

Die folgenden Screenshots veranschaulichen plattformspezifische Sichten, die zu einem hinzugefügt wurden Xamarin.Forms [`StackLayout`](xref:Xamarin.Forms.StackLayout) :

[![Stacklayout mit plattformspezifischen Ansichten](code-images/screenshots-sml.png)](code-images/screenshots.png#lightbox "Stacklayout mit plattformspezifischen Ansichten")

Die Möglichkeit, plattformspezifische Ansichten einem Layout hinzuzufügen, Xamarin.Forms wird auf jeder Plattform durch zwei Erweiterungs Methoden ermöglicht:

- `Add`– Fügt der Auflistung eines Layouts eine plattformspezifische Ansicht hinzu [`Children`](xref:Xamarin.Forms.Layout`1.Children) .
- `ToView`– nimmt eine plattformspezifische Ansicht und umschließt Sie als einen Xamarin.Forms [`View`](xref:Xamarin.Forms.View) , der als- `Content` Eigenschaft eines Steuer Elements festgelegt werden kann.

Wenn Sie diese Methoden in einem frei Xamarin.Forms gegebenen Projekt verwenden, müssen Sie den entsprechenden plattformspezifischen Xamarin.Forms Namespace importieren:

- **iOS**: Xamarin.Forms.Platform.iOS
- **Android**: Xamarin.Forms.Platform.Android
- **Universelle Windows-Plattform (UWP)** : Xamarin.Forms.Platform.UWP

## <a name="adding-platform-specific-views-on-each-platform"></a>Hinzufügen von plattformspezifischen Ansichten auf jeder Plattform

In den folgenden Abschnitten wird gezeigt, wie plattformspezifische Ansichten einem Xamarin.Forms Layout auf jeder Plattform hinzugefügt werden.

### <a name="ios"></a>iOS

Im folgenden Codebeispiel wird veranschaulicht, wie ein `UILabel` zu einem [`StackLayout`](xref:Xamarin.Forms.StackLayout) und einem hinzugefügt wird [`ContentView`](xref:Xamarin.Forms.ContentView) :

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

Im Beispiel wird davon ausgegangen, dass die `stackLayout` -Instanz und die- `contentView` Instanz in XAML oder c# erstellt wurden.

### <a name="android"></a>Android

Im folgenden Codebeispiel wird veranschaulicht, wie ein `TextView` zu einem [`StackLayout`](xref:Xamarin.Forms.StackLayout) und einem hinzugefügt wird [`ContentView`](xref:Xamarin.Forms.ContentView) :

```csharp
var textView = new TextView (MainActivity.Instance) { Text = originalText, TextSize = 14 };
stackLayout.Children.Add (textView);
contentView.Content = textView.ToView();
```

Im Beispiel wird davon ausgegangen, dass die `stackLayout` -Instanz und die- `contentView` Instanz in XAML oder c# erstellt wurden.

### <a name="universal-windows-platform"></a>Universelle Windows-Plattform

Im folgenden Codebeispiel wird veranschaulicht, wie ein `TextBlock` zu einem [`StackLayout`](xref:Xamarin.Forms.StackLayout) und einem hinzugefügt wird [`ContentView`](xref:Xamarin.Forms.ContentView) :

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

Im Beispiel wird davon ausgegangen, dass die `stackLayout` -Instanz und die- `contentView` Instanz in XAML oder c# erstellt wurden.

## <a name="overriding-platform-measurements-for-custom-views"></a>Überschreiben von Platt Formmessungen für benutzerdefinierte Ansichten

Benutzerdefinierte Ansichten auf den einzelnen Plattformen implementieren häufig nur die Maßeinheit für das Layoutszenario, für das Sie entworfen wurden. Beispielsweise kann eine benutzerdefinierte Ansicht so entworfen werden, dass Sie nur die Hälfte der verfügbaren Breite des Geräts einnimmt. Nachdem Sie jedoch für andere Benutzer freigegeben wurde, ist möglicherweise die benutzerdefinierte Ansicht erforderlich, um die gesamte verfügbare Breite des Geräts zu belegen. Daher kann es erforderlich sein, eine benutzerdefinierte Ansichts Messungs Implementierung zu überschreiben, wenn Sie in einem Layout wieder verwendet werden Xamarin.Forms . Aus diesem Grund stellen die `Add` -und- `ToView` Erweiterungs Methoden außer Kraft setzungen bereit, mit denen messdelegaten angegeben werden können. Dies kann das benutzerdefinierte Ansichts Layout überschreiben, wenn es einem Layout hinzugefügt wird Xamarin.Forms .

In den folgenden Abschnitten wird gezeigt, wie Sie das Layout von benutzerdefinierten Ansichten außer Kraft setzen, um die Verwendung von Maßeinheiten zu korrigieren.

### <a name="ios"></a>iOS

Das folgende Codebeispiel zeigt die- `CustomControl` Klasse, die von erbt `UILabel` :

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

Eine Instanz dieser Sicht wird einem hinzugefügt [`StackLayout`](xref:Xamarin.Forms.StackLayout) , wie im folgenden Codebeispiel gezeigt:

```csharp
var customControl = new CustomControl {
  MinimumFontSize = 14,
  Lines = 0,
  LineBreakMode = UILineBreakMode.WordWrap,
  Text = "This control has incorrect sizing - there's empty space above and below it."
};
stackLayout.Children.Add (customControl);
```

Da die Überschreibung jedoch `CustomControl.SizeThatFits` immer die Höhe 150 zurückgibt, wird die Ansicht mit einem leeren Bereich oberhalb und darunter angezeigt, wie im folgenden Screenshot zu sehen:

![IOS CustomControl mit fehlerhafter size-fits-Implementierung](code-images/ios-bad-measurement.png)

Eine Lösung für dieses Problem besteht darin, eine-Implementierung bereitzustellen `GetDesiredSizeDelegate` , wie im folgenden Codebeispiel gezeigt:

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

Diese Methode verwendet die von der-Methode bereitgestellte Breite `CustomControl.SizeThatFits` , ersetzt jedoch die Höhe von 150 für eine Höhe von 70. Wenn die- `CustomControl` Instanz hinzugefügt wird [`StackLayout`](xref:Xamarin.Forms.StackLayout) , kann die- `FixSize` Methode als angegeben werden, `GetDesiredSizeDelegate` um die von der-Klasse bereitgestellte fehlerhafte Messung zu beheben `CustomControl` :

```csharp
stackLayout.Children.Add (customControl, FixSize);
```

Dies führt dazu, dass die benutzerdefinierte Ansicht ordnungsgemäß angezeigt wird, ohne dass ein leerer Leerraum oberhalb und unterhalb angezeigt wird, wie im folgenden Screenshot zu sehen:

![IOS CustomControl mit getdesiredsize-außer Kraft Setzung](code-images/ios-good-measurement.png)

### <a name="android"></a>Android

Das folgende Codebeispiel zeigt die- `CustomControl` Klasse, die von erbt `TextView` :

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

Eine Instanz dieser Sicht wird einem hinzugefügt [`StackLayout`](xref:Xamarin.Forms.StackLayout) , wie im folgenden Codebeispiel gezeigt:

```csharp
var customControl = new CustomControl (MainActivity.Instance) {
  Text = "This control has incorrect sizing - it doesn't occupy the available width of the device.",
  TextSize = 14
};
stackLayout.Children.Add (customControl);
```

Da die Überschreibung jedoch `CustomControl.OnMeasure` immer die Hälfte der angeforderten Breite zurückgibt, wird die Ansicht nur die Hälfte der verfügbaren Breite des Geräts angezeigt, wie im folgenden Screenshot zu sehen:

![Android CustomControl mit fehlerhafter onmeasure-Implementierung](code-images/android-bad-measurement.png)

Eine Lösung für dieses Problem besteht darin, eine-Implementierung bereitzustellen `GetDesiredSizeDelegate` , wie im folgenden Codebeispiel gezeigt:

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

Diese Methode verwendet die von der-Methode bereitgestellte Breite `CustomControl.OnMeasure` , multipliziert Sie jedoch mit zwei. Wenn die- `CustomControl` Instanz hinzugefügt wird [`StackLayout`](xref:Xamarin.Forms.StackLayout) , kann die- `FixSize` Methode als angegeben werden, `GetDesiredSizeDelegate` um die von der-Klasse bereitgestellte fehlerhafte Messung zu beheben `CustomControl` :

```csharp
stackLayout.Children.Add (customControl, FixSize);
```

Dies führt dazu, dass die benutzerdefinierte Ansicht ordnungsgemäß angezeigt wird, wobei die Breite des Geräts belegt wird, wie im folgenden Screenshot zu sehen:

![Android CustomControl mit benutzerdefiniertem getdesiredsize-Delegaten](code-images/android-good-measurement.png)

### <a name="universal-windows-platform"></a>Universelle Windows-Plattform

Das folgende Codebeispiel zeigt die- `CustomControl` Klasse, die von erbt `Panel` :

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

Eine Instanz dieser Sicht wird einem hinzugefügt [`StackLayout`](xref:Xamarin.Forms.StackLayout) , wie im folgenden Codebeispiel gezeigt:

```csharp
var brokenControl = new CustomControl {
  Text = "This control has incorrect sizing - it doesn't occupy the available width of the device."
};
stackLayout.Children.Add(brokenControl);
```

Da die Überschreibung jedoch `CustomControl.ArrangeOverride` immer die Hälfte der angeforderten Breite zurückgibt, wird die Ansicht auf die Hälfte der verfügbaren Breite des Geräts zugeschnitten, wie im folgenden Screenshot zu sehen:

![UWP CustomControl mit fehlerhafter ArrangeOverride-Implementierung](code-images/winrt-bad-measurement.png)

Eine Lösung für dieses Problem besteht darin, eine-Implementierung bereitzustellen `ArrangeOverrideDelegate` , wenn die Ansicht hinzugefügt wird [`StackLayout`](xref:Xamarin.Forms.StackLayout) , wie im folgenden Codebeispiel gezeigt:

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

Diese Methode verwendet die von der-Methode bereitgestellte Breite `CustomControl.ArrangeOverride` , multipliziert Sie jedoch mit zwei. Dies führt dazu, dass die benutzerdefinierte Ansicht ordnungsgemäß angezeigt wird, wobei die Breite des Geräts belegt wird, wie im folgenden Screenshot zu sehen:

![UWP CustomControl mit ArrangeOverride-Delegaten](code-images/winrt-good-measurement.png)

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde erläutert, wie systemeigene Ansichten zu einem Xamarin.Forms mit c# erstellten Layout hinzugefügt werden, und wie Sie das Layout von benutzerdefinierten Ansichten überschreiben, um die Verwendung der Messungs-API zu korrigieren.

## <a name="related-links"></a>Verwandte Links

- [Nativeeinbettungen (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-nativeembedding)
- [Native Formulare](~/xamarin-forms/platform/native-forms.md)
