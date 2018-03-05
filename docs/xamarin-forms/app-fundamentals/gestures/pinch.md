---
title: "Hinzufügen einer verkleinern Gestenhandler-Erkennung"
description: "Zwei-Finger Bewegung wird zum Ausführen von interaktiven Zoom verwendet und ist mit der PinchGestureRecognizer-Klasse implementiert. Ein übliches Szenario für die zwei-Finger-Geste wird, interaktive Zoom eines Bilds an der Position von zwei-Finger durchzuführen. Dies wird erreicht, indem Sie den Inhalt des Viewports skalieren und wird in diesem Artikel veranschaulicht."
ms.topic: article
ms.prod: xamarin
ms.assetid: 832F7810-F0CF-441A-B04A-3975F3FB8B29
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/21/2016
ms.openlocfilehash: 38e46af1d928a3d4e5dc33e2a46fe04cd169ed60
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="adding-a-pinch-gesture-recognizer"></a>Hinzufügen einer verkleinern Gestenhandler-Erkennung

_Zwei-Finger Bewegung wird zum Ausführen von interaktiven Zoom verwendet und ist mit der PinchGestureRecognizer-Klasse implementiert. Ein übliches Szenario für die zwei-Finger-Geste wird, interaktive Zoom eines Bilds an der Position von zwei-Finger durchzuführen. Dies wird erreicht, indem Sie den Inhalt des Viewports skalieren und wird in diesem Artikel veranschaulicht._

## <a name="overview"></a>Übersicht

Damit ein Benutzeroberflächenelement zoombarem mit zwei-Finger Bewegung ist, erstellen eine [ `PinchGestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PinchGestureRecognizer/) -Instanz verwenden, behandeln die [ `PinchUpdated` ](https://developer.xamarin.com/api/event/Xamarin.Forms.PinchGestureRecognizer.PinchUpdated/) Ereignis, und fügen Sie die neue Geste Erkennung der [ `GestureRecognizers` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.GestureRecognizers/) Auflistung auf das Element der Debuggerbenutzeroberfläche. Das folgende Codebeispiel zeigt eine `PinchGestureRecognizer` angefügt, um eine [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) Element:

```csharp
var pinchGesture = new PinchGestureRecognizer();
pinchGesture.PinchUpdated += (s, e) => {
  // Handle the pinch
};
image.GestureRecognizers.Add(pinchGesture);
```

Dies kann auch in XAML erreicht werden, wie im folgenden Codebeispiel gezeigt:

```xaml
<Image Source="waterfront.jpg">
  <Image.GestureRecognizers>
    <PinchGestureRecognizer PinchUpdated="OnPinchUpdated" />
  </Image.GestureRecognizers>
</Image>
```

Der Code für die `OnPinchUpdated` -Ereignishandler wird dann in die CodeBehind-Datei hinzugefügt:

```csharp
void OnPinchUpdated (object sender, PinchGestureUpdatedEventArgs e)
{
  // Handle the pinch
}
```

## <a name="creating-a-pinchtozoom-container"></a>Erstellen eines Containers PinchToZoom

Behandlung von zwei-Finger Bewegung zum Ausführen eines Vorgangs Zoom erfordert einige mathematische Funktionen, um die Benutzeroberfläche zu transformieren. Dieser Abschnitt enthält eine generalisierte Hilfsklasse zum Ausführen von der Mathematik, der verwendet werden kann, Benutzer-Schnittstellenelement interaktiv zu vergrößern. Das folgende Codebeispiel zeigt die `PinchToZoomContainer`-Klasse:

```csharp
public class PinchToZoomContainer : ContentView
{
  ...

  public PinchToZoomContainer ()
  {
    var pinchGesture = new PinchGestureRecognizer ();
    pinchGesture.PinchUpdated += OnPinchUpdated;
    GestureRecognizers.Add (pinchGesture);
  }

  void OnPinchUpdated (object sender, PinchGestureUpdatedEventArgs e)
  {
    ...
  }
}
```

Diese Klasse kann auf ein Benutzeroberflächenelement umbrochen werden, sodass verkleinern Bewegung umschlossene Benutzeroberflächenelement vergrößert wird. Das folgende Beispiel zeigt für die Verwendung von XAML-Code die `PinchToZoomContainer` wrapping ein [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) Element:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:PinchGesture;assembly=PinchGesture"
             x:Class="PinchGesture.HomePage">
    <ContentPage.Content>
        <Grid Padding="20">
            <local:PinchToZoomContainer>
                <local:PinchToZoomContainer.Content>
                    <Image Source="waterfront.jpg" />
                </local:PinchToZoomContainer.Content>
            </local:PinchToZoomContainer>
        </Grid>
    </ContentPage.Content>
</ContentPage>
```

Im folgenden Codebeispiel wird veranschaulicht wie die `PinchToZoomContainer` dient als Wrapper für ein [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) Element in einer C#-Seite:

```csharp
public class HomePageCS : ContentPage
{
  public HomePageCS ()
  {
    Content = new Grid {
      Padding = new Thickness (20),
      Children = {
        new PinchToZoomContainer {
          Content = new Image { Source = ImageSource.FromFile ("waterfront.jpg") }
        }
      }
    };
  }
}
```

Wenn die [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) Element empfängt eine zwei-Finger-Geste, das Bild wird werden vergrößertes oder out. Der Zoomfaktor wird ausgeführt, indem die `PinchZoomContainer.OnPinchUpdated` -Methode, die im folgenden Codebeispiel gezeigt wird:

```csharp
void OnPinchUpdated (object sender, PinchGestureUpdatedEventArgs e)
{
  if (e.Status == GestureStatus.Started) {
    // Store the current scale factor applied to the wrapped user interface element,
    // and zero the components for the center point of the translate transform.
    startScale = Content.Scale;
    Content.AnchorX = 0;
    Content.AnchorY = 0;
  }
  if (e.Status == GestureStatus.Running) {
    // Calculate the scale factor to be applied.
    currentScale += (e.Scale - 1) * startScale;
    currentScale = Math.Max (1, currentScale);

    // The ScaleOrigin is in relative coordinates to the wrapped user interface element,
    // so get the X pixel coordinate.
    double renderedX = Content.X + xOffset;
    double deltaX = renderedX / Width;
    double deltaWidth = Width / (Content.Width * startScale);
    double originX = (e.ScaleOrigin.X - deltaX) * deltaWidth;

    // The ScaleOrigin is in relative coordinates to the wrapped user interface element,
    // so get the Y pixel coordinate.
    double renderedY = Content.Y + yOffset;
    double deltaY = renderedY / Height;
    double deltaHeight = Height / (Content.Height * startScale);
    double originY = (e.ScaleOrigin.Y - deltaY) * deltaHeight;

    // Calculate the transformed element pixel coordinates.
    double targetX = xOffset - (originX * Content.Width) * (currentScale - startScale);
    double targetY = yOffset - (originY * Content.Height) * (currentScale - startScale);

    // Apply translation based on the change in origin.
    Content.TranslationX = targetX.Clamp (-Content.Width * (currentScale - 1), 0);
    Content.TranslationY = targetY.Clamp (-Content.Height * (currentScale - 1), 0);

    // Apply scale factor.
    Content.Scale = currentScale;
  }
  if (e.Status == GestureStatus.Completed) {
    // Store the translation delta's of the wrapped user interface element.
    xOffset = Content.TranslationX;
    yOffset = Content.TranslationY;
  }
}
```

Diese Methode aktualisiert die Zoomstufe des umschlossenen Element der Benutzeroberfläche basierend auf der Benutzeraktion verkleinern. Dies erfolgt mithilfe der Werte von der [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.PinchGestureUpdatedEventArgs.Scale/), [ `ScaleOrigin` ](https://developer.xamarin.com/api/property/Xamarin.Forms.PinchGestureUpdatedEventArgs.ScaleOrigin/) und [ `Status` ](https://developer.xamarin.com/api/property/Xamarin.Forms.PinchGestureUpdatedEventArgs.Status/) Eigenschaften der [ `PinchGestureUpdatedEventArgs` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PinchGestureUpdatedEventArgs/) Instanz berechnen Sie den Skalierungsfaktor in der Ursprung der Bewegung verkleinern angewendet wird. Das umschlossene Benutzer-Element wird dann am ursprünglichen Speicherort der Bewegung verkleinern vergrößert, durch Festlegen seiner [ `TranslationX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationX/), [ `TranslationY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationY/), und [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) Eigenschaften, die die berechneten Werte.

## <a name="summary"></a>Zusammenfassung

Zwei-Finger Bewegung wird zum Ausführen von interaktiven Zoom verwendet und wird implementiert, mit der [ `PinchGestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PinchGestureRecognizer/) Klasse.


## <a name="related-links"></a>Verwandte Links

- [PinchGesture (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithGestures/PinchGesture/)
- [GestureRecognizer](https://developer.xamarin.com/api/type/Xamarin.Forms.GestureRecognizer/)
- [PinchGestureRecognizer](https://developer.xamarin.com/api/type/Xamarin.Forms.PinchGestureRecognizer/)
