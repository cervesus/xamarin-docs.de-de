---
title: 'title: "Hinzufügen der Gestenerkennung für Zusammendrückbewegungen" description: "In diesem Artikel wird erläutert, wie Sie die Zusammendrückbewegung verwenden, um den interaktiven Zoom eines Bildes an der Position der Zusammendrückbewegung auszuführen."'
description: 'ms.prod: xamarin ms.assetid: 832F7810-F0CF-441A-B04A-3975F3FB8B29 ms.technology: xamarin-forms author: davidbritch ms.author: dabritch ms.date: 01/21/2016 no-loc: [Xamarin.Forms, Xamarin.Essentials]'
ms.prod: xamarin
ms.assetid: 832F7810-F0CF-441A-B04A-3975F3FB8B29
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/21/2016
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: da4a8bc66a7986efd3683de6dce1f6af618b85cc
ms.sourcegitcommit: ea9269b5d9e3d68b61bb428560a10034117ee457
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2020
ms.locfileid: "84137851"
---
# <a name="adding-a-pinch-gesture-recognizer"></a>Hinzufügen einer Gestenerkennung für Zusammendrückbewegungen

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithgestures-pinchgesture)

_Die Zusammendrückbewegung wird zum Ausführen des interaktiven Zooms verwendet und mit der Klasse „PinchGestureRecognizer“ implementiert. Allgemein wird die Zusammendrückbewegung verwendet, um einen interaktiven Zoom eines Bildes im Speicherort der Zusammendrückbewegung auszuführen. Dies wird durch Skalierung des Anzeigebereichs erreicht und in diesem Artikel veranschaulicht._

Erstellen Sie eine Instanz [`PinchGestureRecognizer`](xref:Xamarin.Forms.PinchGestureRecognizer), bearbeiten Sie das Ereignis [`PinchUpdated`](xref:Xamarin.Forms.PinchGestureRecognizer.PinchUpdated), und fügen Sie die neue Funktion zur Gestenerkennung der Sammlung [`GestureRecognizers`](xref:Xamarin.Forms.View.GestureRecognizers) im Benutzeroberflächenelement hinzu, damit ein Benutzeroberflächenelement mit der Zusammendrückbewegung vergrößert werden kann. Das folgende Codebeispiel stellt die Instanz `PinchGestureRecognizer` dar, die an das Element [`Image`](xref:Xamarin.Forms.Image) angefügt ist:

```csharp
var pinchGesture = new PinchGestureRecognizer();
pinchGesture.PinchUpdated += (s, e) => {
  // Handle the pinch
};
image.GestureRecognizers.Add(pinchGesture);
```

Dies kann auch wie im folgenden Codebeispiel dargestellt in XAML erreicht werden:

```xaml
<Image Source="waterfront.jpg">
  <Image.GestureRecognizers>
    <PinchGestureRecognizer PinchUpdated="OnPinchUpdated" />
  </Image.GestureRecognizers>
</Image>
```

Der Code für den Ereignishandler `OnPinchUpdated` wird dann zur CodeBehind-Datei hinzugefügt:

```csharp
void OnPinchUpdated (object sender, PinchGestureUpdatedEventArgs e)
{
  // Handle the pinch
}
```

## <a name="creating-a-pinchtozoom-container"></a>Erstellen des Containers „PinchToZoom“

Für das Verarbeiten der Zusammendrückbewegung zum Ausführen eines Zooms sind Berechnungen erforderlich, um die Benutzeroberfläche zu transformieren. Dieser Abschnitt enthält eine Hilfsprogrammklasse zum Ausführen der Berechnungen, damit jedes Benutzeroberflächenelement interaktiv vergrößert werden kann. Das folgende Codebeispiel zeigt die `PinchToZoomContainer`-Klasse:

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

Diese Klasse kann das Benutzeroberflächenelement umschließen, sodass die Zusammendrückbewegung das umschlossene Benutzeroberflächenelement vergrößert. Das folgende XAML-Codebeispiel stellt die Klasse `PinchToZoomContainer` dar, die ein [`Image`](xref:Xamarin.Forms.Image)-Element umschließt:

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

Das folgende Codebeispiel veranschaulicht, wie die Klasse `PinchToZoomContainer` ein [`Image`](xref:Xamarin.Forms.Image)-Element auf einer C#-Seite umschließt:

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

Das angezeigte Bild wird vergrößert oder verkleinert, wenn das [`Image`](xref:Xamarin.Forms.Image)-Element eine Zusammendrückbewegung empfängt. Der Zoom wird von der Methode `PinchZoomContainer.OnPinchUpdated` ausgeführt, die im folgenden Codebeispiel veranschaulicht wird:

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

Diese Methode aktualisiert die Zoomstufe des umschlossenen Benutzeroberflächenelements basierend auf der Zusammendrückbewegung des Benutzers. Durch die Verwendung der Werte der Eigenschaften [`Scale`](xref:Xamarin.Forms.PinchGestureUpdatedEventArgs.Scale), [`ScaleOrigin`](xref:Xamarin.Forms.PinchGestureUpdatedEventArgs.ScaleOrigin) und [`Status`](xref:Xamarin.Forms.PinchGestureUpdatedEventArgs.Status) der Instanz [`PinchGestureUpdatedEventArgs`](xref:Xamarin.Forms.PinchGestureUpdatedEventArgs) wird der Skalierungsfaktor berechnet, der am Anfang der Zusammendrückbewegung angewendet werden soll. Das umschlossene Benutzeroberflächenelement wird anschließend am Anfang der Zusammendrückbewegung vergrößert, indem die Eigenschaften [`TranslationX`](xref:Xamarin.Forms.VisualElement.TranslationX), [`TranslationY`](xref:Xamarin.Forms.VisualElement.TranslationY) und [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) auf die berechneten Werte eingestellt werden.

## <a name="related-links"></a>Verwandte Links

- [PinchGesture (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithgestures-pinchgesture)
- [GestureRecognizer](xref:Xamarin.Forms.GestureRecognizer)
- [PinchGestureRecognizer](xref:Xamarin.Forms.PinchGestureRecognizer)
