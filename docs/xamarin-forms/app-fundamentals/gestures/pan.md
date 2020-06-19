---
title: 'title: "Hinzufügen der Gestenerkennung für Schwenkbewegungen" description: "In diesem Artikel wird erläutert, wie Sie mit einer Schwenkbewegung ein Bild horizontal und vertikal schwenken, sodass der gesamte Bildinhalt in einem Anzeigebereich angezeigt werden kann, der kleiner als die Bildabmessungen ist."'
description: 'ms.prod: xamarin ms.assetid: 42CBD2CF-432D-4F19-A05E-D569BB7F8713 ms.technology: xamarin-forms author: davidbritch ms.author: dabritch ms.date: 01/21/2016 no-loc: [Xamarin.Forms, Xamarin.Essentials]'
ms.prod: xamarin
ms.assetid: 42CBD2CF-432D-4F19-A05E-D569BB7F8713
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/21/2016
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 53122991811c06360e8d015a753096cb35c1cca0
ms.sourcegitcommit: ea9269b5d9e3d68b61bb428560a10034117ee457
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2020
ms.locfileid: "84137630"
---
# <a name="adding-a-pan-gesture-recognizer"></a>Hinzufügen einer Gestenerkennung für Schwenkbewegungen

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithgestures-pangesture)

_Die Schwenkbewegung wird verwendet, um die Bewegung von Fingern um den Bildschirm zu erkennen und diese Bewegung auf den Inhalt zu übertragen. Sie wird mit der `PanGestureRecognizer`-Klasse implementiert. Ein häufiges Szenario für die Schwenkbewegung ist das horizontale und vertikale Schwenken eines Bilds, sodass der gesamte Bildinhalt in einem Anzeigebereich angezeigt werden kann, der kleiner als die Bildabmessungen ist. Dies wird durch Bewegen des Bilds innerhalb des Anzeigebereichs erreicht und in diesem Artikel veranschaulicht._

Erstellen Sie eine Instanz [`PanGestureRecognizer`](xref:Xamarin.Forms.PanGestureRecognizer), bearbeiten Sie das Ereignis [`PanUpdated`](xref:Xamarin.Forms.PanGestureRecognizer.PanUpdated), und fügen Sie die neue Funktion zur Gestenerkennung der Collection [`GestureRecognizers`](xref:Xamarin.Forms.View.GestureRecognizers) im Benutzeroberflächenelement hinzu, damit ein Benutzeroberflächenelement mit der Schwenkbewegung bewegt werden kann. Das folgende Codebeispiel zeigt die Instanz `PanGestureRecognizer`, die an das Element [`Image`](xref:Xamarin.Forms.Image) angefügt ist:

```csharp
var panGesture = new PanGestureRecognizer();
panGesture.PanUpdated += (s, e) => {
  // Handle the pan
};
image.GestureRecognizers.Add(panGesture);
```

Dies kann auch wie im folgenden Codebeispiel dargestellt in XAML erreicht werden:

```xaml
<Image Source="MonoMonkey.jpg">
  <Image.GestureRecognizers>
    <PanGestureRecognizer PanUpdated="OnPanUpdated" />
  </Image.GestureRecognizers>
</Image>
```

Der Code für den Ereignishandler `OnPanUpdated` wird dann zur CodeBehind-Datei hinzugefügt:

```csharp
void OnPanUpdated (object sender, PanUpdatedEventArgs e)
{
  // Handle the pan
}
```

## <a name="creating-a-pan-container"></a>Erstellen eines Schwenkcontainers

Dieser Abschnitt enthält eine generalisierte Hilfsklasse, die das Schwenken mit Freihandformen ausführt. Dies eignet sich in der Regel gut für das Navigieren in Bildern oder Karten. Für das Bearbeiten der Schwenkbewegung zum Ausführen dieser Operation ist Mathematik erforderlich, um die Benutzeroberfläche zu transformieren. Diese Mathematik wird verwendet, um nur innerhalb der Begrenzung des umschlossenen Benutzeroberflächenelements zu schwenken. Das folgende Codebeispiel zeigt die `PanContainer`-Klasse:

```csharp
public class PanContainer : ContentView
{
  double x, y;

  public PanContainer ()
  {
    // Set PanGestureRecognizer.TouchPoints to control the
    // number of touch points needed to pan
    var panGesture = new PanGestureRecognizer ();
    panGesture.PanUpdated += OnPanUpdated;
    GestureRecognizers.Add (panGesture);
  }

  void OnPanUpdated (object sender, PanUpdatedEventArgs e)
  {
    ...
  }
}
```

Diese Klasse kann das Benutzeroberflächenelement umschließen, sodass die Bewegung das umschlossene Benutzeroberflächenelement schwenkt. Das folgende XAML-Codebeispiel zeigt die Klasse `PanContainer`, die ein [`Image`](xref:Xamarin.Forms.Image)-Element umschließt:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:PanGesture"
             x:Class="PanGesture.HomePage">
    <ContentPage.Content>
        <AbsoluteLayout>
            <local:PanContainer>
                <Image Source="MonoMonkey.jpg" WidthRequest="1024" HeightRequest="768" />
            </local:PanContainer>
        </AbsoluteLayout>
    </ContentPage.Content>
</ContentPage>
```

Das folgende Codebeispiel veranschaulicht, wie die Klasse `PanContainer` ein [`Image`](xref:Xamarin.Forms.Image)-Element auf einer C#-Seite umschließt:

```csharp
public class HomePageCS : ContentPage
{
  public HomePageCS ()
  {
    Content = new AbsoluteLayout {
      Padding = new Thickness (20),
      Children = {
        new PanContainer {
          Content = new Image {
            Source = ImageSource.FromFile ("MonoMonkey.jpg"),
            WidthRequest = 1024,
            HeightRequest = 768
          }
        }
      }
    };
  }
}
```

In beiden Beispielen werden die Eigenschaften [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) und [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) auf die Werte für Breite und Höhe des angezeigten Bilds festgelegt.

Das angezeigte Bild wird geschwenkt, wenn das [`Image`](xref:Xamarin.Forms.Image)-Element eine Schwenkbewegung empfängt. Das Schwenken wird von der Methode `PanContainer.OnPanUpdated` ausgeführt, die im folgenden Codebeispiel veranschaulicht wird:

```csharp
void OnPanUpdated (object sender, PanUpdatedEventArgs e)
{
  switch (e.StatusType) {
  case GestureStatus.Running:
    // Translate and ensure we don't pan beyond the wrapped user interface element bounds.
    Content.TranslationX =
      Math.Max (Math.Min (0, x + e.TotalX), -Math.Abs (Content.Width - App.ScreenWidth));
    Content.TranslationY =
      Math.Max (Math.Min (0, y + e.TotalY), -Math.Abs (Content.Height - App.ScreenHeight));
    break;

  case GestureStatus.Completed:
    // Store the translation applied during the pan
    x = Content.TranslationX;
    y = Content.TranslationY;
    break;
  }
}
```

Diese Methode aktualisiert den sichtbaren Inhalt des umschlossenen Benutzeroberflächenelements basierend auf der Schwenkbewegung des Benutzers. Durch die Verwendung der Werte der Eigenschaften [`TotalX`](xref:Xamarin.Forms.PanUpdatedEventArgs.TotalX) und [`TotalY`](xref:Xamarin.Forms.PanUpdatedEventArgs.TotalY) der Instanz [`PanUpdatedEventArgs`](xref:Xamarin.Forms.PanUpdatedEventArgs) werden die Richtung und der Abstand der Schwenkbewegung berechnet. Die Eigenschaften `App.ScreenWidth` und `App.ScreenHeight` geben die Höhe und Breite des Anzeigebereichs an und werden durch die jeweiligen plattformspezifischen Projekte auf die Werte von Breite und Höhe des Gerätebildschirms festgelegt. Das umschlossene Benutzeroberflächenelement wird anschließend geschwenkt, indem die Eigenschaften [`TranslationX`](xref:Xamarin.Forms.VisualElement.TranslationX) und [`TranslationY`](xref:Xamarin.Forms.VisualElement.TranslationY) auf die berechneten Werte festgelegt werden.

Wenn Inhalt in einem Element geschwenkt wird, das nicht den gesamten Bildschirm einnimmt, können die Höhe und Breite des Anzeigebereichs von den Eigenschaften [`Height`](xref:Xamarin.Forms.VisualElement.Height) und [`Width`](xref:Xamarin.Forms.VisualElement.Width) des Elements abgerufen werden.

> [!NOTE]
> Das Anzeigen von Bildern mit hoher Auflösung kann den Speicherbedarf einer App erheblich erhöhen. Sie sollten daher nur erstellt werden, wenn dies erforderlich ist. Sie sollten freigegeben werden, sobald die App sie nicht mehr benötigt. Weitere Informationen finden Sie unter [Optimieren von Bildressourcen](~/xamarin-forms/deploy-test/performance.md#optimize-image-resources).

## <a name="related-links"></a>Verwandte Links

- [PanGesture (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithgestures-pangesture)
- [GestureRecognizer](xref:Xamarin.Forms.GestureRecognizer)
- [PanGestureRecognizer](xref:Xamarin.Forms.PanGestureRecognizer)
