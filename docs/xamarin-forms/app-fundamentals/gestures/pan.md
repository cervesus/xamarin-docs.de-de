---
title: Hinzufügen einer Schwenken Gestenhandler-Erkennung
description: Schwenk Bewegung dient zum Erkennen von ziehen und wird mit der PanGestureRecognizer-Klasse implementiert. Ein übliches Szenario für die Aktion zum Schwenken ist, ein Bild horizontal und vertikal zu ziehen, sodass alle Inhalte Bild angezeigt werden können, wenn sie in einen Viewport, der kleiner als die Dimensionen des Bilds angezeigt wird. Dies wird erreicht, indem Sie das Bild innerhalb des Viewports verschieben und wird in diesem Artikel veranschaulicht.
ms.prod: xamarin
ms.assetid: 42CBD2CF-432D-4F19-A05E-D569BB7F8713
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/21/2016
ms.openlocfilehash: ed38f7ace9e11b009aae768cda2d4af0f36c337e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="adding-a-pan-gesture-recognizer"></a>Hinzufügen einer Schwenken Gestenhandler-Erkennung

_Schwenk Bewegung dient zum Erkennen von ziehen und wird mit der PanGestureRecognizer-Klasse implementiert. Ein übliches Szenario für die Aktion zum Schwenken ist, ein Bild horizontal und vertikal zu ziehen, sodass alle Inhalte Bild angezeigt werden können, wenn sie in einen Viewport, der kleiner als die Dimensionen des Bilds angezeigt wird. Dies wird erreicht, indem Sie das Bild innerhalb des Viewports verschieben und wird in diesem Artikel veranschaulicht._

## <a name="overview"></a>Übersicht

Damit ein Benutzeroberflächenelement draggable mit Pan Bewegung ist, erstellen eine [ `PanGestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PanGestureRecognizer/) -Instanz verwenden, behandeln die [ `PanUpdated` ](https://developer.xamarin.com/api/event/Xamarin.Forms.PanGestureRecognizer.PanUpdated/) Ereignis, und fügen Sie die neue Geste Erkennung der [ `GestureRecognizers` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.GestureRecognizers/) Auflistung auf das Element der Debuggerbenutzeroberfläche. Das folgende Codebeispiel zeigt eine `PanGestureRecognizer` angefügt, um eine [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) Element:

```csharp
var panGesture = new PanGestureRecognizer();
panGesture.PanUpdated += (s, e) => {
  // Handle the pan
};
image.GestureRecognizers.Add(panGesture);
```

Dies kann auch in XAML erreicht werden, wie im folgenden Codebeispiel gezeigt:

```xaml
<Image Source="MonoMonkey.jpg">
  <Image.GestureRecognizers>
    <PanGestureRecognizer PanUpdated="OnPanUpdated" />
  </Image.GestureRecognizers>
</Image>
```

Der Code für die `OnPanUpdated` -Ereignishandler wird dann in die CodeBehind-Datei hinzugefügt:

```csharp
void OnPanUpdated (object sender, PanUpdatedEventArgs e)
{
  // Handle the pan
}
```

> [!NOTE]
> Richtige Schwenken unter Android erfordert die [Xamarin.Forms 2.1.0-pre1 NuGet-Paket](https://www.nuget.org/packages/Xamarin.Forms/2.1.0.6501-pre1) mindestens.

## <a name="creating-a-pan-container"></a>Erstellen eines Containers zum Schwenken

Dieser Abschnitt enthält eine generalisierte Hilfsklasse, die Freihandform schwenken, führt dies in der Regel eignet sich für das Navigieren in Bildern oder Zuordnungen. Behandlung von Schwenken Bewegung zum Ausführen eines Ziehvorgangs erfordert einige mathematische Funktionen, um die Benutzeroberfläche zu transformieren. Diese mathematische wird verwendet, um nur innerhalb der Grenzen des umschlossenen Benutzeroberflächenelement ziehen. Das folgende Codebeispiel zeigt die `PanContainer`-Klasse:

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

Diese Klasse kann auf ein Benutzeroberflächenelement umbrochen werden, sodass zum Schwenken Bewegung umschlossene Benutzeroberflächenelement Ziehen wird. Das folgende Beispiel zeigt für die Verwendung von XAML-Code die `PanContainer` wrapping ein [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) Element:

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

Im folgenden Codebeispiel wird veranschaulicht wie die `PanContainer` dient als Wrapper für ein [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) Element in einer C#-Seite:

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

In beiden Beispielen die [ `WidthRequest` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.WidthRequest/) und [ `HeightRequest` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.HeightRequest/) Eigenschaften werden festgelegt, um die Werte für Breite und Höhe des Bilds angezeigt wird.

Wenn die [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) Element empfängt eine Geste zum Schwenken, wird das angezeigte Bild gezogen werden. Der Ziehvorgang erfolgt durch die `PanContainer.OnPanUpdated` -Methode, die im folgenden Codebeispiel gezeigt wird:

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

Diese Methode aktualisiert den sichtbaren Inhalt umbrochenen Element der Benutzeroberfläche, basierend auf der Benutzeraktion zum schwenken. Dies erfolgt mithilfe der Werte von der [ `TotalX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.PanUpdatedEventArgs.TotalX/) und [ `TotalY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.PanUpdatedEventArgs.TotalY/) Eigenschaften der [ `PanUpdatedEventArgs` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PanUpdatedEventArgs/) Instanz um die Richtung zu berechnen und Abstand von der schwenken. Die `App.ScreenWidth` und `App.ScreenHeight` Eigenschaften geben Sie die Höhe und Breite des Viewports und werden von den jeweiligen plattformspezifischen Projekten auf die Bildschirmbreite und Höhenwerte des Geräts Bildschirm festgelegt. Das umschlossene Element gezogen wird dann durch Festlegen seiner [ `TranslationX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationX/) und [ `TranslationY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationY/) Eigenschaften auf die berechneten Werte.

Wenn Inhalt in einem Element schwenken, die nicht den gesamten Bildschirm belegen, die Höhe und Breite des Viewports abgerufen werden können aus des Elements [ `Height` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Height/) und [ `Width` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Width/) Eigenschaften.

> [!NOTE]
> Hochauflösende Bilder anzeigen kann eine app Speicherbedarf erheblich erhöhen. Sie sollten daher nur erstellt, wenn erforderlich und freigegeben werden, sollte sobald die app nicht mehr benötigt. Weitere Informationen finden Sie unter [Optimieren von Bildressourcen](~/xamarin-forms/deploy-test/performance.md#optimizeimages).

## <a name="summary"></a>Zusammenfassung

Schwenk Bewegung dient zum Erkennen von ziehen und wird implementiert, mit der [ `PanGestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PanGestureRecognizer/) Klasse.



## <a name="related-links"></a>Verwandte Links

- [PanGesture (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithGestures/PanGesture/)
- [GestureRecognizer](https://developer.xamarin.com/api/type/Xamarin.Forms.GestureRecognizer/)
- [PanGestureRecognizer](https://developer.xamarin.com/api/type/Xamarin.Forms.PanGestureRecognizer/)
