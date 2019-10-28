---
title: Einfache Animationen in xamarin. Forms
description: Die viewextensions-Klasse stellt Erweiterungs Methoden bereit, die verwendet werden können, um einfache Animationen zu erstellen. Dieser Artikel veranschaulicht das Erstellen und Abbrechen von Animationen mithilfe der viewextensions-Klasse.
ms.prod: xamarin
ms.assetid: 4A6FAE5A-848F-4CE0-BFA1-22A6309B5225
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2019
ms.openlocfilehash: 116911787db128b103fb555554076704a0549db5
ms.sourcegitcommit: f8583585c501607fdfa061b95e9a9f385ed1d591
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/26/2019
ms.locfileid: "72959163"
---
# <a name="simple-animations-in-xamarinforms"></a>Einfache Animationen in xamarin. Forms

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-animation-basic)

_Die viewextensions-Klasse stellt Erweiterungs Methoden bereit, die verwendet werden können, um einfache Animationen zu erstellen. Dieser Artikel veranschaulicht das Erstellen und Abbrechen von Animationen mithilfe der viewextensions-Klasse._

Die [`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions) -Klasse stellt die folgenden Erweiterungs Methoden bereit, die verwendet werden können, um einfache Animationen zu erstellen:

- [`TranslateTo`](xref:Xamarin.Forms.ViewExtensions.TranslateTo(Xamarin.Forms.VisualElement,System.Double,System.Double,System.UInt32,Xamarin.Forms.Easing)) animiert die [`TranslationX`](xref:Xamarin.Forms.VisualElement.TranslationX) und [`TranslationY`](xref:Xamarin.Forms.VisualElement.TranslationY) Eigenschaften einer [`VisualElement`](xref:Xamarin.Forms.VisualElement).
- [`ScaleTo`](xref:Xamarin.Forms.ViewExtensions.ScaleTo*) animiert die [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) -Eigenschaft eines [`VisualElement`](xref:Xamarin.Forms.VisualElement).
- [`RelScaleTo`](xref:Xamarin.Forms.ViewExtensions.RelScaleTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) wendet eine animierte inkrementelle Zunahme oder Verringerung der [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) -Eigenschaft eines [`VisualElement`](xref:Xamarin.Forms.VisualElement)an.
- [`RotateTo`](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) animiert die [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation) -Eigenschaft eines [`VisualElement`](xref:Xamarin.Forms.VisualElement).
- [`RelRotateTo`](xref:Xamarin.Forms.ViewExtensions.RelRotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) wendet eine animierte inkrementelle Zunahme oder Verringerung der [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation) -Eigenschaft eines [`VisualElement`](xref:Xamarin.Forms.VisualElement)an.
- [`RotateXTo`](xref:Xamarin.Forms.ViewExtensions.RotateXTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) animiert die [`RotationX`](xref:Xamarin.Forms.VisualElement.RotationX) -Eigenschaft eines [`VisualElement`](xref:Xamarin.Forms.VisualElement).
- [`RotateYTo`](xref:Xamarin.Forms.ViewExtensions.RotateYTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) animiert die [`RotationY`](xref:Xamarin.Forms.VisualElement.RotationY) -Eigenschaft eines [`VisualElement`](xref:Xamarin.Forms.VisualElement).
- [`FadeTo`](xref:Xamarin.Forms.ViewExtensions.FadeTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) animiert die [`Opacity`](xref:Xamarin.Forms.VisualElement.Opacity) -Eigenschaft eines [`VisualElement`](xref:Xamarin.Forms.VisualElement).

Standardmäßig dauert jede Animation 250 Millisekunden. Beim Erstellen der Animation kann jedoch eine Dauer für jede Animation angegeben werden.

Die [`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions) -Klasse enthält auch eine [`CancelAnimations`](xref:Xamarin.Forms.ViewExtensions.CancelAnimations(Xamarin.Forms.VisualElement)) -Methode, die verwendet werden kann, um Animationen abzubrechen.

> [!NOTE]
> Die [`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions) -Klasse stellt eine [`LayoutTo`](xref:Xamarin.Forms.ViewExtensions.LayoutTo(Xamarin.Forms.VisualElement,Xamarin.Forms.Rectangle,System.UInt32,Xamarin.Forms.Easing)) Erweiterungsmethode bereit. Diese Methode soll jedoch von Layouts verwendet werden, um Übergänge zwischen layoutzuständen zu animieren, die Größen-und Positionsänderungen enthalten. Daher sollte Sie nur von [`Layout`](xref:Xamarin.Forms.Layout) -Unterklassen verwendet werden.

Die Animations Erweiterungs Methoden in der [`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions) -Klasse sind alle asynchron und geben ein `Task<bool>`-Objekt zurück. Der Rückgabewert ist `false`, wenn die Animation abgeschlossen ist, und `true`, wenn die Animation abgebrochen wird. Daher sollten die Animations Methoden in der Regel mit dem `await`-Operator verwendet werden, sodass Sie leicht feststellen können, wann eine Animation abgeschlossen wurde. Außerdem wird es möglich, sequenzielle Animationen mit nachfolgenden Animations Methoden zu erstellen, die nach Abschluss der vorherigen Methode ausgeführt werden. Weitere Informationen finden Sie unter zusammen [gesetzte Animationen](#compound).

Wenn die Anforderung besteht, eine Animation im Hintergrund abzuschließen, kann der `await` Operator ausgelassen werden. In diesem Szenario werden die Animations Erweiterungs Methoden nach dem Initiieren der Animation schnell zurückgegeben, wobei die Animation im Hintergrund stattfindet. Dieser Vorgang kann beim Erstellen von zusammengesetzten Animationen genutzt werden. Weitere Informationen finden Sie unter zusammen [gesetzte Animationen](#composite).

Weitere Informationen zum `await`-Operator finden Sie [unter async-Unterstützung: Übersicht](~/cross-platform/platform/async.md).

## <a name="single-animations"></a>Einzelne Animationen

Jede Erweiterungsmethode in der [`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions) implementiert einen einzelnen Animations Vorgang, der eine Eigenschaft in einem bestimmten Zeitraum progressiv von einem Wert in einen anderen Wert ändert. In diesem Abschnitt wird jeder Animations Vorgang erläutert.

### <a name="rotation"></a>Drehung

Im folgenden Codebeispiel wird die Verwendung der [`RotateTo`](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) -Methode zum Animieren der [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation) -Eigenschaft eines [`Image`](xref:Xamarin.Forms.Image)veranschaulicht:

```csharp
await image.RotateTo (360, 2000);
image.Rotation = 0;
```

Dieser Code animiert die [`Image`](xref:Xamarin.Forms.Image) Instanz, indem Sie um bis zu 360 Grad über 2 Sekunden (2000 Millisekunden) rotiert. Die [`RotateTo`](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) -Methode ruft den aktuellen [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation) -Eigenschafts Wert für den Start der Animation ab und rotiert dann von diesem Wert zum ersten Argument (360). Nachdem die Animation fertiggestellt ist, wird die [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation) -Eigenschaft des Bilds auf 0 zurückgesetzt. Dadurch wird sichergestellt, dass die `Rotation`-Eigenschaft nach Abschluss der Animation nicht bei 360 bleibt, wodurch zusätzliche Drehungen verhindert werden.

Die folgenden Screenshots zeigen die Drehung, die auf den einzelnen Plattformen ausgeführt wird:

![](simple-images/rotateto.png "Rotation Animation")

### <a name="relative-rotation"></a>Relative Drehung

Im folgenden Codebeispiel wird die Verwendung der [`RelRotateTo`](xref:Xamarin.Forms.ViewExtensions.RelRotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) -Methode zum inkrementellen vergrößern oder Verkleinern der [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation) -Eigenschaft eines [`Image`](xref:Xamarin.Forms.Image)veranschaulicht:

```csharp
await image.RelRotateTo (360, 2000);
```

Dieser Code animiert die [`Image`](xref:Xamarin.Forms.Image) Instanz, indem Sie 360 Grad von der Startposition über 2 Sekunden (2000 Millisekunden) rotiert. Die [`RelRotateTo`](xref:Xamarin.Forms.ViewExtensions.RelRotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) -Methode ruft den aktuellen [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation) -Eigenschafts Wert für den Start der Animation ab und rotiert dann von diesem Wert zu dem Wert plus dem ersten Argument (360). Dadurch wird sichergestellt, dass jede Animation immer eine 360 Grad-Drehung von der Anfangsposition aus ist. Wenn eine neue Animation aufgerufen wird, während eine Animation bereits ausgeführt wird, wird Sie daher von der aktuellen Position aus gestartet und kann an einer Position enden, die kein Inkrement von 360 Grad ist.

Die folgenden Screenshots zeigen die relative Drehung, die auf den einzelnen Plattformen ausgeführt wird:

![](simple-images/relrotateto.png "Relative Rotation Animation")

### <a name="scaling"></a>Skalieren

Im folgenden Codebeispiel wird die Verwendung der [`ScaleTo`](xref:Xamarin.Forms.ViewExtensions.ScaleTo*) -Methode zum Animieren der [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) -Eigenschaft eines [`Image`](xref:Xamarin.Forms.Image)veranschaulicht:

```csharp
await image.ScaleTo (2, 2000);
```

Mit diesem Code wird die [`Image`](xref:Xamarin.Forms.Image) Instanz animiert, indem eine Skalierung auf eine doppelte Größe über 2 Sekunden (2000 Millisekunden) ausgeführt wird. Die [`ScaleTo`](xref:Xamarin.Forms.ViewExtensions.ScaleTo*) -Methode ruft den aktuellen [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) -Eigenschafts Wert (Standardwert 1) für den Start der Animation ab und skaliert dann von diesem Wert auf das erste Argument (2). Dies hat den Effekt, dass die Größe des Bilds auf die doppelte Größe erweitert wird.

Die folgenden Screenshots zeigen, wie die Skalierung auf den einzelnen Plattformen ausgeführt wird:

![](simple-images/scaleto.png "Scaling Animation")

> [!NOTE]
> Die [`VisualElement`](xref:Xamarin.Forms.VisualElement)-Klasse definiert auch die Eigenschaften [`ScaleX`](xref:Xamarin.Forms.VisualElement.ScaleX) und [`ScaleY`](xref:Xamarin.Forms.VisualElement.ScaleY), die das `VisualElement` horizontal und vertikal skalieren können. Diese Eigenschaften können mit der [`Animation`](xref:Xamarin.Forms.Animation) -Klasse animiert werden. Weitere Informationen finden Sie unter [benutzerdefinierte Animationen in xamarin. Forms](custom.md).

### <a name="relative-scaling"></a>Relative Skalierung

Im folgenden Codebeispiel wird die Verwendung der [`RelScaleTo`](xref:Xamarin.Forms.ViewExtensions.RelScaleTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) -Methode zum Animieren der [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) -Eigenschaft eines [`Image`](xref:Xamarin.Forms.Image)veranschaulicht:

```csharp
await image.RelScaleTo (2, 2000);
```

Mit diesem Code wird die [`Image`](xref:Xamarin.Forms.Image) Instanz animiert, indem eine Skalierung auf eine doppelte Größe über 2 Sekunden (2000 Millisekunden) ausgeführt wird. Die [`RelScaleTo`](xref:Xamarin.Forms.ViewExtensions.RelScaleTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) -Methode ruft den aktuellen [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) -Eigenschafts Wert für den Start der Animation ab und skaliert dann von diesem Wert auf den Wert plus das erste Argument (2). Dadurch wird sichergestellt, dass jede Animation immer eine Skalierung von 2 von der Startposition aus ist.

### <a name="scaling-and-rotation-with-anchors"></a>Skalierung und Drehung mit Anker

Die Eigenschaften " [`AnchorX`](xref:Xamarin.Forms.VisualElement.AnchorX) " und " [`AnchorY`](xref:Xamarin.Forms.VisualElement.AnchorY) " legen den Mittelpunkt der Skalierung oder Drehung für die Eigenschaften [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation) und [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) fest. Daher wirken sich ihre Werte auch auf die [`RotateTo`](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) -und [`ScaleTo`](xref:Xamarin.Forms.ViewExtensions.ScaleTo*) Methoden aus.

Bei einer [`Image`](xref:Xamarin.Forms.Image) , die in der Mitte eines Layouts platziert wurde, veranschaulicht das folgende Codebeispiel, wie das Bild um die Mitte des Layouts gedreht wird, indem die [`AnchorY`](xref:Xamarin.Forms.VisualElement.AnchorY) -Eigenschaft festgelegt wird:

```csharp
double radius = Math.Min(absoluteLayout.Width, absoluteLayout.Height) / 2;
image.AnchorY = radius / image.Height;
await image.RotateTo(360, 2000);
```

Um die [`Image`](xref:Xamarin.Forms.Image) Instanz um die Mitte des Layouts zu drehen, müssen die Eigenschaften [`AnchorX`](xref:Xamarin.Forms.VisualElement.AnchorX) und [`AnchorY`](xref:Xamarin.Forms.VisualElement.AnchorY) auf Werte festgelegt werden, die relativ zur Breite und Höhe der `Image`sind. In diesem Beispiel ist der Mittelpunkt der `Image` so definiert, dass er sich in der Mitte des Layouts befindet, sodass der Standard `AnchorX` Wert 0,5 nicht geändert werden muss. Allerdings wird die `AnchorY`-Eigenschaft als Wert von der obersten Position des `Image` bis zum Mittelpunkt des Layouts neu definiert. Dadurch wird sichergestellt, dass die `Image` eine vollständige Drehung um 360 Grad um den Mittelpunkt des Layouts führt, wie in den folgenden Screenshots gezeigt:

![](simple-images/rotate-anchors.png "Rotation Animation with Anchors")

### <a name="translation"></a>Übersetzung

Im folgenden Codebeispiel wird die Verwendung der [`TranslateTo`](xref:Xamarin.Forms.ViewExtensions.TranslateTo(Xamarin.Forms.VisualElement,System.Double,System.Double,System.UInt32,Xamarin.Forms.Easing)) -Methode zum Animieren der [`TranslationX`](xref:Xamarin.Forms.VisualElement.TranslationX) und [`TranslationY`](xref:Xamarin.Forms.VisualElement.TranslationY) Eigenschaften einer [`Image`](xref:Xamarin.Forms.Image)veranschaulicht:

```csharp
await image.TranslateTo (-100, -100, 1000);
```

Mit diesem Code wird die [`Image`](xref:Xamarin.Forms.Image) Instanz animiert, indem Sie horizontal und vertikal über eine Sekunde (1000 Millisekunden) übersetzt wird. Die [`TranslateTo`](xref:Xamarin.Forms.ViewExtensions.TranslateTo(Xamarin.Forms.VisualElement,System.Double,System.Double,System.UInt32,Xamarin.Forms.Easing)) -Methode übersetzt gleichzeitig das Bild 100 Pixel nach links und 100 Pixel nach oben. Dies liegt daran, dass das erste und das zweite Argument negative Zahlen sind. Durch die Bereitstellung positiver Zahlen wird das Bild nach rechts und nach unten übersetzt.

Die folgenden Screenshots zeigen die Übersetzung, die auf jeder Plattform durchgeführt wird:

![](simple-images/translateto.png "Translation Animation")

> [!NOTE]
> Wenn ein Element anfänglich aus dem Bildschirm entfernt und dann auf den Bildschirm übersetzt wird, bleibt das Eingabe Layout des Elements nach der Übersetzung deaktiviert, sodass der Benutzer nicht mit dem Bildschirm interagieren kann. Daher empfiehlt es sich, eine Ansicht an der endgültigen Position und dann alle erforderlichen Übersetzungen zu überprüfen.

### <a name="fading"></a>Ausblenden

Im folgenden Codebeispiel wird die Verwendung der [`FadeTo`](xref:Xamarin.Forms.ViewExtensions.FadeTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) -Methode zum Animieren der [`Opacity`](xref:Xamarin.Forms.VisualElement.Opacity) -Eigenschaft eines [`Image`](xref:Xamarin.Forms.Image)veranschaulicht:

```csharp
image.Opacity = 0;
await image.FadeTo (1, 4000);
```

Mit diesem Code wird die [`Image`](xref:Xamarin.Forms.Image) Instanz animiert, indem Sie in mehr als 4 Sekunden (4000 Millisekunden) ausgeblendet wird. Die [`FadeTo`](xref:Xamarin.Forms.ViewExtensions.FadeTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) -Methode ruft den aktuellen [`Opacity`](xref:Xamarin.Forms.VisualElement.Opacity) -Eigenschafts Wert für den Start der Animation ab und wird dann von diesem Wert auf das erste Argument (1).

Die folgenden Screenshots zeigen, wie die einzelnen Plattformen ausgeblendet werden:

![](simple-images/fadeto.png "Fading Animation")

<a name="compound" />

## <a name="compound-animations"></a>Verbund Animationen

Eine Verbund Animation ist eine sequenzielle Kombination von Animationen und kann mit dem `await`-Operator erstellt werden, wie im folgenden Codebeispiel gezeigt:

```csharp
await image.TranslateTo (-100, 0, 1000);    // Move image left
await image.TranslateTo (-100, -100, 1000); // Move image up
await image.TranslateTo (100, 100, 2000);   // Move image diagonally down and right
await image.TranslateTo (0, 100, 1000);     // Move image left
await image.TranslateTo (0, 0, 1000);       // Move image up
```

In diesem Beispiel wird der [`Image`](xref:Xamarin.Forms.Image) über 6 Sekunden (6000 Millisekunden) übersetzt. Bei der Übersetzung des `Image` werden fünf Animationen verwendet, wobei der `await` Operator angibt, dass jede Animation sequenziell ausgeführt wird. Folglich werden nachfolgende Animations Methoden ausgeführt, nachdem die vorherige Methode abgeschlossen wurde.

<a name="composite" />

## <a name="composite-animations"></a>Zusammengesetzte Animationen

Eine zusammengesetzte Animation ist eine Kombination aus Animationen, bei denen zwei oder mehr Animationen gleichzeitig ausgeführt werden. Zusammengesetzte Animationen können erstellt werden, indem erwartete und nicht erwartete Animationen gemischt werden, wie im folgenden Codebeispiel gezeigt:

```csharp
image.RotateTo (360, 4000);
await image.ScaleTo (2, 2000);
await image.ScaleTo (1, 2000);
```

In diesem Beispiel wird der [`Image`](xref:Xamarin.Forms.Image) skaliert und gleichzeitig über 4 Sekunden gedreht (4000 Millisekunden). Bei der Skalierung des `Image` werden zwei sequenzielle Animationen verwendet, die zur gleichen Zeit wie die Drehung auftreten. Die [`RotateTo`](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) -Methode wird ohne `await` Operator ausgeführt und wird sofort zurückgegeben, wobei die erste [`ScaleTo`](xref:Xamarin.Forms.ViewExtensions.ScaleTo*) Animation beginnt. Der `await`-Operator für den ersten `ScaleTo` Methoden aufrufvorgangs verzögert den zweiten `ScaleTo` Methoden Aufruf, bis der erste `ScaleTo` Methoden Aufrufvorgang abgeschlossen ist. An diesem Punkt ist die `RotateTo` Animation halb Weise abgeschlossen, und die `Image` werden um 180 Grad gedreht. Während der letzten 2 Sekunden (2000 Millisekunden) sind die beiden `ScaleTo` Animation und die `RotateTo` Animation abgeschlossen.

### <a name="running-multiple-asynchronous-methods-concurrently"></a>Gleichzeitiges Ausführen mehrerer asynchroner Methoden

Die Methoden `static` `Task.WhenAny` und `Task.WhenAll` werden verwendet, um mehrere asynchrone Methoden gleichzeitig auszuführen. Sie können daher zum Erstellen von zusammengesetzten Animationen verwendet werden. Beide Methoden geben ein `Task` Objekt zurück und akzeptieren eine Auflistung von Methoden, die jeweils ein `Task` Objekt zurückgeben. Die `Task.WhenAny`-Methode wird abgeschlossen, wenn die Ausführung einer beliebigen Methode in der-Auflistung abgeschlossen ist, wie im folgenden Codebeispiel gezeigt:

```csharp
await Task.WhenAny<bool>
(
  image.RotateTo (360, 4000),
  image.ScaleTo (2, 2000)
);
await image.ScaleTo (1, 2000);
```

In diesem Beispiel enthält der `Task.WhenAny` Methoden Aufrufes zwei Aufgaben. Der erste Task dreht das Bild über 4 Sekunden (4000 Millisekunden), während die zweite Aufgabe das Bild über 2 Sekunden (2000 Millisekunden) skaliert. Wenn die zweite Aufgabe abgeschlossen ist, wird der `Task.WhenAny` Methoden aufrufsvorgang abgeschlossen. Obwohl die [`RotateTo`](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) -Methode noch ausgeführt wird, kann die zweite [`ScaleTo`](xref:Xamarin.Forms.ViewExtensions.ScaleTo*) Methode beginnen.

Die `Task.WhenAll`-Methode wird abgeschlossen, wenn alle Methoden in der-Auflistung abgeschlossen wurden, wie im folgenden Codebeispiel gezeigt:

```csharp
// 10 minute animation
uint duration = 10 * 60 * 1000;

await Task.WhenAll (
  image.RotateTo (307 * 360, duration),
  image.RotateXTo (251 * 360, duration),
  image.RotateYTo (199 * 360, duration)
);
```

In diesem Beispiel enthält der `Task.WhenAll` Methoden Aufrufes drei Aufgaben, die jeweils über 10 Minuten ausgeführt werden. Jede `Task` gibt eine andere Anzahl von 360-Grad-Drehungen – 307 Drehungen für [`RotateTo`](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)), 251-Drehungen für [`RotateXTo`](xref:Xamarin.Forms.ViewExtensions.RotateXTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing))und 199-Drehungen für [`RotateYTo`](xref:Xamarin.Forms.ViewExtensions.RotateYTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)). Bei diesen Werten handelt es sich um Primzahlen. Dadurch wird sichergestellt, dass die Rotationen nicht synchronisiert werden und daher nicht wiederholende Muster

Die folgenden Screenshots zeigen, wie viele Drehungen auf den einzelnen Plattformen ausgeführt werden:

![](simple-images/multiple-rotations.png "Composite Animation")

## <a name="canceling-animations"></a>Abbrechen von Animationen

Eine Anwendung kann eine oder mehrere Animationen mit einem aufzurufenden `static` [`ViewExtensions.CancelAnimations`](xref:Xamarin.Forms.ViewExtensions.CancelAnimations(Xamarin.Forms.VisualElement)) -Methode abbrechen, wie im folgenden Codebeispiel gezeigt:

```csharp
ViewExtensions.CancelAnimations (image);
```

Dadurch werden sofort alle Animationen abgebrochen, die derzeit auf der [`Image`](xref:Xamarin.Forms.Image) Instanz ausgeführt werden.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde das Erstellen und Abbrechen von Animationen mithilfe der [`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions) -Klasse veranschaulicht. Diese Klasse stellt Erweiterungs Methoden bereit, die verwendet werden können, um einfache Animationen zu erstellen, die [`VisualElement`](xref:Xamarin.Forms.VisualElement) Instanzen drehen, skalieren, übersetzen und ausblenden.

## <a name="related-links"></a>Verwandte Links

- [Async Support Overview (Übersicht über die asynchrone Unterstützung)](~/cross-platform/platform/async.md)
- [Grundlegende Animation (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-animation-basic)
- [Viewextensions](xref:Xamarin.Forms.ViewExtensions)
