---
title: Hinzufügen einer Pan stiftbewegungs-Erkennung
description: In diesem Artikel wird erläutert, wie eine Geste schwenken, horizontal und vertikal Schwenken ein Bild, sodass alle den Bildinhalt angezeigt werden können, wenn es kleiner als die Bildgröße eine relativ zum Viewport angezeigt wird.
ms.prod: xamarin
ms.assetid: 42CBD2CF-432D-4F19-A05E-D569BB7F8713
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/21/2016
ms.openlocfilehash: 59e9f4c61bda86faa5a55d70ef91411adb14da6d
ms.sourcegitcommit: 79313604ed68829435cfdbb530db36794d50858f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/18/2018
ms.locfileid: "38996805"
---
# <a name="adding-a-pan-gesture-recognizer"></a>Hinzufügen einer Pan stiftbewegungs-Erkennung

_Die Pan-Aktion für die Verschiebung der Finger auf dem Bildschirm zu erkennen und diese Verschiebung auf Inhalt angewendet werden und wird implementiert, mit der `PanGestureRecognizer` Klasse. Ein häufiges Szenario für die Pan-Aktion ist horizontal und vertikal Schwenken ein Bild, sodass alle den Bildinhalt angezeigt werden können, wenn es kleiner als die Bildgröße eine relativ zum Viewport angezeigt wird. Dies wird erreicht, indem Sie das Bild innerhalb des Viewports verschieben und in diesem Artikel gezeigt wird._

Um ein Element der Benutzeroberfläche mit dieser Bewegung Pan verschiebbar zu machen, erstellen eine [ `PanGestureRecognizer` ](xref:Xamarin.Forms.PanGestureRecognizer) -Instanz, behandeln Sie die [ `PanUpdated` ](xref:Xamarin.Forms.PanGestureRecognizer.PanUpdated) -Ereignis, und fügen Sie der neuen stiftbewegungs-Erkennung, um die [ `GestureRecognizers` ](xref:Xamarin.Forms.View.GestureRecognizers) Auflistung in das Benutzeroberflächenelement. Das folgende Codebeispiel zeigt eine `PanGestureRecognizer` angefügt, um eine [ `Image` ](xref:Xamarin.Forms.Image) Element:

```csharp
var panGesture = new PanGestureRecognizer();
panGesture.PanUpdated += (s, e) => {
  // Handle the pan
};
image.GestureRecognizers.Add(panGesture);
```

Dies kann auch in XAML, erreicht werden, wie im folgenden Codebeispiel gezeigt:

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

## <a name="creating-a-pan-container"></a>Erstellen eines Containers Schwenken

Dieser Abschnitt enthält eine generalisierte Hilfsklasse, die führt Freihandform schwenken, die in der Regel durch Navigieren in Bildern oder Zuordnungen geeignet ist. Behandeln die Pan-Geste zum Ausführen dieses Vorgangs erfordert einiger Berechnungen aus, um die Benutzeroberfläche zu transformieren. Diese mathematischen werden nur innerhalb der Grenzen des das umschlossene Benutzeroberflächenelement schwenken. Das folgende Codebeispiel zeigt die `PanContainer`-Klasse:

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

Diese Klasse kann ein Element der Benutzeroberfläche umbrochen werden, so, dass die Bewegung auf das umschlossene Benutzeroberflächenelement Schwenken wird. Das folgende Beispiel zeigt für die XAML-Code der `PanContainer` umschließen einer [ `Image` ](xref:Xamarin.Forms.Image) Element:

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

Das folgende Codebeispiel zeigt die `PanContainer` dient als Wrapper für ein [ `Image` ](xref:Xamarin.Forms.Image) Element in einer C#-Seite:

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

In beiden Beispielen die [ `WidthRequest` ](xref:Xamarin.Forms.VisualElement.WidthRequest) und [ `HeightRequest` ](xref:Xamarin.Forms.VisualElement.HeightRequest) Eigenschaften festgelegt werden, auf die Werte für Breite und Höhe des Bilds angezeigt wird.

Wenn die [ `Image` ](xref:Xamarin.Forms.Image) Element erhält eine Geste für ein schwenken, wird das angezeigte Bild verschoben werden. Die Pfanne erfolgt durch die `PanContainer.OnPanUpdated` -Methode, die im folgenden Codebeispiel gezeigt wird:

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

Diese Methode aktualisiert den sichtbaren Inhalt des umschlossenen Element der Benutzeroberfläche, basierend auf der Benutzeraktion schwenken. Dies erfolgt mithilfe der Werte von der [ `TotalX` ](xref:Xamarin.Forms.PanUpdatedEventArgs.TotalX) und [ `TotalY` ](xref:Xamarin.Forms.PanUpdatedEventArgs.TotalY) Eigenschaften der [ `PanUpdatedEventArgs` ](xref:Xamarin.Forms.PanUpdatedEventArgs) Instanz um die Richtung zu berechnen und die Entfernung von der Pfanne. Die `App.ScreenWidth` und `App.ScreenHeight` Eigenschaften geben Sie die Höhe und Breite des Viewports und werden von den jeweiligen plattformspezifischen Projekten auf die Bildschirmbreite und die Werte für die Höhe Bildschirm des Geräts festgelegt. Das umschlossene Benutzer-Element wird dann verschoben, durch Festlegen seiner [ `TranslationX` ](xref:Xamarin.Forms.VisualElement.TranslationX) und [ `TranslationY` ](xref:Xamarin.Forms.VisualElement.TranslationY) Eigenschaften, die die berechneten Werte.

Wenn der Inhalt in einem Element zu schwenken, die nicht den gesamten Bildschirm einnehmen wird, die Höhe und Breite des Viewports erhalten Sie von des Elements [ `Height` ](xref:Xamarin.Forms.VisualElement.Height) und [ `Width` ](xref:Xamarin.Forms.VisualElement.Width) Eigenschaften.

> [!NOTE]
> Anzeigen von Bildern mit hoher Auflösung kann den Speicherbedarf einer app erheblich erhöhen. Sie sollten daher nur erstellt, wenn erforderlich, und freigegeben werden soll, sobald die app nicht mehr benötigt. Weitere Informationen finden Sie unter [Optimieren von Bildressourcen](~/xamarin-forms/deploy-test/performance.md#optimizeimages).

## <a name="related-links"></a>Verwandte Links

- [PanGesture (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithGestures/PanGesture/)
- [GestureRecognizer](xref:Xamarin.Forms.GestureRecognizer)
- [PanGestureRecognizer](xref:Xamarin.Forms.PanGestureRecognizer)
