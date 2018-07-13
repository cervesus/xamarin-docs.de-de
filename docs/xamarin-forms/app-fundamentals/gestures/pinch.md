---
title: Hinzufügen einer Pinch-Stiftbewegungs-Erkennung
description: In diesem Artikel wird erläutert, wie die Pinch-Geste zum Ausführen von interaktiven Zoom eines Bilds an die Pinch-Speicherort verwendet wird.
ms.prod: xamarin
ms.assetid: 832F7810-F0CF-441A-B04A-3975F3FB8B29
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/21/2016
ms.openlocfilehash: 37befdcd4ccbcd49e3cebda92d55ae6f70da2ad6
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998699"
---
# <a name="adding-a-pinch-gesture-recognizer"></a>Hinzufügen einer Pinch-Stiftbewegungs-Erkennung

_Die Pinch-Geste wird zum Ausführen von interaktiven Zoom verwendet, und es wird mit der PinchGestureRecognizer-Klasse implementiert. Ein häufiges Szenario für die Pinch-Geste ist zum Ausführen von interaktiven Zoom eines Bilds an der Pinch-Position. Dies wird erreicht, indem Sie den Inhalt des Viewports skalieren und in diesem Artikel gezeigt wird._

## <a name="overview"></a>Übersicht

Um ein Element der Benutzeroberfläche mit der Pinch-Geste zoombare machen, erstellen eine [ `PinchGestureRecognizer` ](xref:Xamarin.Forms.PinchGestureRecognizer) -Instanz, behandeln Sie die [ `PinchUpdated` ](xref:Xamarin.Forms.PinchGestureRecognizer.PinchUpdated) -Ereignis, und fügen Sie der neuen stiftbewegungs-Erkennung, um die [ `GestureRecognizers` ](xref:Xamarin.Forms.View.GestureRecognizers) Auflistung in das Benutzeroberflächenelement. Das folgende Codebeispiel zeigt eine `PinchGestureRecognizer` angefügt, um eine [ `Image` ](xref:Xamarin.Forms.Image) Element:

```csharp
var pinchGesture = new PinchGestureRecognizer();
pinchGesture.PinchUpdated += (s, e) => {
  // Handle the pinch
};
image.GestureRecognizers.Add(pinchGesture);
```

Dies kann auch in XAML, erreicht werden, wie im folgenden Codebeispiel gezeigt:

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

Behandlung von der Pinch-Geste, um einem Zoomvorgang auszuführen, erfordert einiger Berechnungen aus, um die Benutzeroberfläche zu transformieren. Dieser Abschnitt enthält eine generalisierte Hilfsklasse zum Ausführen von der Mathematik, die verwendet werden kann, alle Benutzer-Schnittstellenelement interaktiv zu vergrößern. Das folgende Codebeispiel zeigt die `PinchToZoomContainer`-Klasse:

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

Diese Klasse kann ein Element der Benutzeroberfläche umbrochen werden, so, dass die Pinch-Geste das umschlossene Benutzeroberflächenelement vergrößert wird. Das folgende Beispiel zeigt für die XAML-Code der `PinchToZoomContainer` umschließen einer [ `Image` ](xref:Xamarin.Forms.Image) Element:

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

Das folgende Codebeispiel zeigt die `PinchToZoomContainer` dient als Wrapper für ein [ `Image` ](xref:Xamarin.Forms.Image) Element in einer C#-Seite:

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

Wenn die [ `Image` ](xref:Xamarin.Forms.Image) Element erhält eine zusammendrückbewegung, das angezeigte Bild wird werden vergrößert im bzw. Der Zoomfaktor wird ausgeführt, indem die `PinchZoomContainer.OnPinchUpdated` -Methode, die im folgenden Codebeispiel gezeigt wird:

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

Diese Methode aktualisiert die Zoomstufe des umschlossenen Element der Benutzeroberfläche anhand des Benutzers Pinch-Geste. Dies wird erreicht, indem Sie mithilfe der Werte der [ `Scale` ](xref:Xamarin.Forms.PinchGestureUpdatedEventArgs.Scale), [ `ScaleOrigin` ](xref:Xamarin.Forms.PinchGestureUpdatedEventArgs.ScaleOrigin) und [ `Status` ](xref:Xamarin.Forms.PinchGestureUpdatedEventArgs.Status) Eigenschaften der [ `PinchGestureUpdatedEventArgs` ](xref:Xamarin.Forms.PinchGestureUpdatedEventArgs) Instanz zum Berechnen des Skalierungsfaktor auf den Ursprung der Pinch-Geste angewendet werden soll. Das umschlossene Benutzer-Element wird durch Festlegen von klicken Sie dann auf den Ursprung der Pinch-Geste vergrößert die [ `TranslationX` ](xref:Xamarin.Forms.VisualElement.TranslationX), [ `TranslationY` ](xref:Xamarin.Forms.VisualElement.TranslationY), und [ `Scale` ](xref:Xamarin.Forms.VisualElement.Scale) Eigenschaften, die die berechneten Werte.

## <a name="summary"></a>Zusammenfassung

Die Pinch-Geste wird zum Ausführen von interaktiven Zoom verwendet und wird implementiert, mit der [ `PinchGestureRecognizer` ](xref:Xamarin.Forms.PinchGestureRecognizer) Klasse.


## <a name="related-links"></a>Verwandte Links

- [PinchGesture (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithGestures/PinchGesture/)
- [GestureRecognizer](xref:Xamarin.Forms.GestureRecognizer)
- [PinchGestureRecognizer](xref:Xamarin.Forms.PinchGestureRecognizer)
