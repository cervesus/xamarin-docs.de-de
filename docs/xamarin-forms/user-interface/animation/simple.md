---
title: Einfache Animationen in Xamarin.Forms
description: Die ViewExtensions-Klasse enthält Erweiterungsmethoden, die verwendet werden können, um einfache Animationen zu erstellen. In diesem Artikel veranschaulicht das Erstellen und Abbrechen von Animationen mithilfe der ViewExtensions-Klasse.
ms.prod: xamarin
ms.assetid: 4A6FAE5A-848F-4CE0-BFA1-22A6309B5225
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/27/2017
ms.openlocfilehash: 71972f13f991bc5ad3ddf3c1c631fa7413290204
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70759907"
---
# <a name="simple-animations-in-xamarinforms"></a>Einfache Animationen in Xamarin.Forms

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-animation-basic)

_Die ViewExtensions-Klasse enthält Erweiterungsmethoden, die verwendet werden können, um einfache Animationen zu erstellen. In diesem Artikel veranschaulicht das Erstellen und Abbrechen von Animationen mithilfe der ViewExtensions-Klasse._

Die [ `ViewExtensions` ](xref:Xamarin.Forms.ViewExtensions) -Klasse bietet die folgenden Erweiterungsmethoden, die verwendet werden können, um einfache Animationen zu erstellen:

- [`TranslateTo`](xref:Xamarin.Forms.ViewExtensions.TranslateTo(Xamarin.Forms.VisualElement,System.Double,System.Double,System.UInt32,Xamarin.Forms.Easing)) Animiert die [ `TranslationX` ](xref:Xamarin.Forms.VisualElement.TranslationX) und [ `TranslationY` ](xref:Xamarin.Forms.VisualElement.TranslationY) Eigenschaften eine [ `VisualElement` ](xref:Xamarin.Forms.VisualElement).
- [`ScaleTo`](xref:Xamarin.Forms.VisualElement.Scale) Animiert die [ `Scale` ](xref:Xamarin.Forms.VisualElement.Scale) Eigenschaft eine [ `VisualElement` ](xref:Xamarin.Forms.VisualElement).
- [`RelScaleTo`](xref:Xamarin.Forms.ViewExtensions.RelScaleTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) gilt eine animierte stufenweisen Erhöhung oder Verringerung um die [ `Scale` ](xref:Xamarin.Forms.VisualElement.Scale) Eigenschaft eine [ `VisualElement` ](xref:Xamarin.Forms.VisualElement).
- [`RotateTo`](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) Animiert die [ `Rotation` ](xref:Xamarin.Forms.VisualElement.Rotation) Eigenschaft eine [ `VisualElement` ](xref:Xamarin.Forms.VisualElement).
- [`RelRotateTo`](xref:Xamarin.Forms.ViewExtensions.RelRotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) gilt eine animierte stufenweisen Erhöhung oder Verringerung um die [ `Rotation` ](xref:Xamarin.Forms.VisualElement.Rotation) Eigenschaft eine [ `VisualElement` ](xref:Xamarin.Forms.VisualElement).
- [`RotateXTo`](xref:Xamarin.Forms.ViewExtensions.RotateXTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) Animiert die [ `RotationX` ](xref:Xamarin.Forms.VisualElement.RotationX) Eigenschaft eine [ `VisualElement` ](xref:Xamarin.Forms.VisualElement).
- [`RotateYTo`](xref:Xamarin.Forms.ViewExtensions.RotateYTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) Animiert die [ `RotationY` ](xref:Xamarin.Forms.VisualElement.RotationY) Eigenschaft eine [ `VisualElement` ](xref:Xamarin.Forms.VisualElement).
- [`FadeTo`](xref:Xamarin.Forms.ViewExtensions.FadeTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) Animiert die [ `Opacity` ](xref:Xamarin.Forms.VisualElement.Opacity) Eigenschaft eine [ `VisualElement` ](xref:Xamarin.Forms.VisualElement).

Standardmäßig wird jede Animation 250 Millisekunden dauert. Allerdings kann eine Dauer für jede Animation angegeben werden, wenn Sie die Animation zu erstellen.

Die [ `ViewExtensions` ](xref:Xamarin.Forms.ViewExtensions) -Klasse enthält auch eine [ `CancelAnimations` ](xref:Xamarin.Forms.ViewExtensions.CancelAnimations(Xamarin.Forms.VisualElement)) -Methode, die verwendet werden kann, um alle Animationen abzubrechen.

> [!NOTE]
> Die [ `ViewExtensions` ](xref:Xamarin.Forms.ViewExtensions) -Klasse stellt eine [ `LayoutTo` ](xref:Xamarin.Forms.ViewExtensions.LayoutTo(Xamarin.Forms.VisualElement,Xamarin.Forms.Rectangle,System.UInt32,Xamarin.Forms.Easing)) -Erweiterungsmethode. Diese Methode soll jedoch Übergänge zwischen Zuständen für Layout zu animieren, die Größe enthält, und positionieren Sie die Änderungen durch Layouts verwendet werden. Aus diesem Grund, es sollte nur verwendet werden von [ `Layout` ](xref:Xamarin.Forms.Layout) Unterklassen.

Die Animation-Erweiterungsmethoden in den [ `ViewExtensions` ](xref:Xamarin.Forms.ViewExtensions) Klasse sind asynchron und Zurückgeben einer `Task<bool>` Objekt. Der Rückgabewert ist `false` , wenn die Animation abgeschlossen wird, und `true` , wenn die Animation abgebrochen wird. Aus diesem Grund die Animation-Methoden in der Regel mit verwendet werden sollte die `await` -Operator, der es ermöglicht auf einfache Weise bestimmen, wann eine Animation abgeschlossen ist. Darüber hinaus wird es möglich, Erstellen von sequenziellen Animationen mit nachfolgenden Animation-Methoden, die Ausführung nach Abschluss die vorherige Methode. Weitere Informationen finden Sie unter [zusammengesetzte Animationen](#compound).

Liegt eine Anforderung, um eine Animation können vollständig im Hintergrund die `await` Operator kann ausgelassen werden. In diesem Szenario werden die Animation-Erweiterungsmethoden schnell zurückgegeben, nach dem Auslösen der Animation, mit der Animation, die im Hintergrund ausgeführt wird. Dieser Vorgang kann genutzt werden beim Erstellen von zusammengesetzter Animationen. Weitere Informationen finden Sie unter [zusammengesetzten Animationen](#composite).

Weitere Informationen zu den `await` -Operator, finden Sie unter [Async-Unterstützung (Übersicht)](~/cross-platform/platform/async.md).

## <a name="single-animations"></a>Einzelne Animationen

Jede Erweiterungsmethode in der [ `ViewExtensions` ](xref:Xamarin.Forms.ViewExtensions) implementiert einen einzelne Animation-Vorgang, der immer eine Eigenschaft von einem Wert in einen anderen Wert über einen Zeitraum ändert. In diesem Abschnitt werden die einzelnen Animation-Vorgänge.

### <a name="rotation"></a>Drehung

Das folgende Codebeispiel veranschaulicht die Verwendung der [ `RotateTo` ](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) Methode zum Animieren der [ `Rotation` ](xref:Xamarin.Forms.VisualElement.Rotation) Eigenschaft eine [ `Image` ](xref:Xamarin.Forms.Image):

```csharp
await image.RotateTo (360, 2000);
image.Rotation = 0;
```

Dieser Code erstellt eine Animation die [ `Image` ](xref:Xamarin.Forms.Image) Instanz, indem Sie bis 360 Grad drehen, mehr als 2 Sekunden (2000 Millisekunden). Die [ `RotateTo` ](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) Methode ruft die aktuelle [ `Rotation` ](xref:Xamarin.Forms.VisualElement.Rotation) Eigenschaft Wert für den Beginn der Animation, und klicken Sie dann auf das erste Argument (360) von diesem Wert dreht. Nachdem die Animation ist, das Image Abschließen des [ `Rotation` ](xref:Xamarin.Forms.VisualElement.Rotation) Eigenschaft auf 0 zurückgesetzt. Dadurch wird sichergestellt, dass die `Rotation` Eigenschaft nicht auf 360, nachdem die Animation abgeschlossen, die zusätzliche Rotationen verhindern würden ist, bleiben.

Die folgenden Screenshots zeigen die Drehung auf jeder Plattform ausgeführt:

![](simple-images/rotateto.png "Drehungsanimation")

### <a name="relative-rotation"></a>Relative Drehung

Das folgende Codebeispiel veranschaulicht die Verwendung der [ `RelRotateTo` ](xref:Xamarin.Forms.ViewExtensions.RelRotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) Methode schrittweise erhöhen oder Verringern der [ `Rotation` ](xref:Xamarin.Forms.VisualElement.Rotation) Eigenschaft eine [ `Image` ](xref:Xamarin.Forms.Image):

```csharp
await image.RelRotateTo (360, 2000);
```

Dieser Code erstellt eine Animation die [ `Image` ](xref:Xamarin.Forms.Image) Instanz durch das Drehen von 360 Grad von der Startposition mehr als 2 Sekunden (2000 Millisekunden). Die [ `RelRotateTo` ](xref:Xamarin.Forms.ViewExtensions.RelRotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) Methode ruft die aktuelle [ `Rotation` ](xref:Xamarin.Forms.VisualElement.Rotation) Eigenschaft Wert für den Beginn der Animation, und klicken Sie dann von diesem Wert um den Wert und als erstes Argument (360) dreht. Dadurch wird sichergestellt, dass jede Animation immer eine 360-Grad-Drehung, ab der Startposition. Aus diesem Grund, wenn eine neue Animation aufgerufen wird, während bereits eine Animation ausgeführt wird, er beginnt mit der aktuellen Position und möglicherweise enden an der Position, die keinem Inkrement von 360 Grad.

Die folgenden Screenshots zeigen die relative Drehung auf jeder Plattform ausgeführt:

![](simple-images/relrotateto.png "Relative Drehungsanimation")

### <a name="scaling"></a>Skalieren

Das folgende Codebeispiel veranschaulicht die Verwendung der [ `ScaleTo` ](xref:Xamarin.Forms.VisualElement.Scale) Methode zum Animieren der [ `Scale` ](xref:Xamarin.Forms.VisualElement.Scale) Eigenschaft eine [ `Image` ](xref:Xamarin.Forms.Image):

```csharp
await image.ScaleTo (2, 2000);
```

Dieser Code erstellt eine Animation die [ `Image` ](xref:Xamarin.Forms.Image) Instanz durch den Umstieg auf die doppelte Größe über 2 Sekunden (2000 Millisekunden). Die [ `ScaleTo` ](xref:Xamarin.Forms.VisualElement.Scale) Methode ruft die aktuelle [ `Scale` ](xref:Xamarin.Forms.VisualElement.Scale) Eigenschaftswert (Standardwert 1) für den Beginn der Animation und skaliert von diesem Wert, der als erstes Argument (2). Dies wirkt sich die Größe des Bilds erweitern, um das Doppelte der Größe.

Die folgenden Screenshots zeigen die Skalierung auf jeder Plattform ausgeführt:

![](simple-images/scaleto.png "Skalieren der Animation")

> [!NOTE]
> Die [ `VisualElement` ](xref:Xamarin.Forms.VisualElement) -Klasse definiert außerdem [ `ScaleX` ](xref:Xamarin.Forms.VisualElement.ScaleX) und [ `ScaleY` ](xref:Xamarin.Forms.VisualElement.ScaleY) Eigenschaften, die skaliert werden können, die `VisualElement` anders als in der horizontaler bzw. vertikaler Richtung. Diese Eigenschaften können animiert werden, mit der [ `Animation` ](xref:Xamarin.Forms.Animation) Klasse. Weitere Informationen finden Sie unter [benutzerdefinierte Animationen in Xamarin.Forms](custom.md).

### <a name="relative-scaling"></a>Relative Skalierung

Das folgende Codebeispiel veranschaulicht die Verwendung der [ `RelScaleTo` ](xref:Xamarin.Forms.ViewExtensions.RelScaleTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) Methode zum Animieren der [ `Scale` ](xref:Xamarin.Forms.VisualElement.Scale) Eigenschaft eine [ `Image` ](xref:Xamarin.Forms.Image):

```csharp
await image.RelScaleTo (2, 2000);
```

Dieser Code erstellt eine Animation die [ `Image` ](xref:Xamarin.Forms.Image) Instanz durch den Umstieg auf die doppelte Größe über 2 Sekunden (2000 Millisekunden). Die [ `RelScaleTo` ](xref:Xamarin.Forms.ViewExtensions.RelScaleTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) Methode ruft die aktuelle [ `Scale` ](xref:Xamarin.Forms.VisualElement.Scale) Eigenschaftswert für den Beginn der Animation und skaliert von diesem Wert um den Wert und als erstes Argument (2). Dadurch wird sichergestellt, dass jede Animation immer eine Skalierung von 2 von der Anfangsposition.

### <a name="scaling-and-rotation-with-anchors"></a>Skalierung und Drehung mit Ankern

Die [ `AnchorX` ](xref:Xamarin.Forms.VisualElement.AnchorX) und [ `AnchorY` ](xref:Xamarin.Forms.VisualElement.AnchorY) Eigenschaften legen Sie den Mittelpunkt der Skalierung oder Drehung, für die [ `Rotation` ](xref:Xamarin.Forms.VisualElement.Rotation) und [ `Scale` ](xref:Xamarin.Forms.VisualElement.Scale) Eigenschaften. Aus diesem Grund, deren Werte auch Auswirkungen auf die [ `RotateTo` ](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) und [ `ScaleTo` ](xref:Xamarin.Forms.VisualElement.Scale) Methoden.

Erhält ein [ `Image` ](xref:Xamarin.Forms.Image) in der Mitte eines Layouts platziert wurde, wird im folgenden Codebeispiel wird veranschaulicht, drehen das Bild, um den Mittelpunkt des Layouts durch Festlegen seiner [ `AnchorY` ](xref:Xamarin.Forms.VisualElement.AnchorY) Eigenschaft:

```csharp
image.AnchorY = (Math.Min (absoluteLayout.Width, absoluteLayout.Height) / 2) / image.Height;
await image.RotateTo (360, 2000);
```

Zum Drehen der [ `Image` ](xref:Xamarin.Forms.Image) Instanz das Layout an, um den Mittelpunkt der [ `AnchorX` ](xref:Xamarin.Forms.VisualElement.AnchorX) und [ `AnchorY` ](xref:Xamarin.Forms.VisualElement.AnchorY) Eigenschaften müssen auf Werte festgelegt werden relativ zur Breite und Höhe der `Image`. In diesem Beispiel die Mitte des der `Image` definiert in der Mitte des Layouts, und daher die Standardeinstellung `AnchorX` Wert von 0,5 nicht geändert werden muss. Allerdings die `AnchorY` Eigenschaft einen Wert zwischen dem oberen Rand der Neudefinition der `Image` auf den Mittelpunkt des Layouts. Dadurch wird sichergestellt, dass die `Image` können Sie eine vollständige Drehung von 360 Grad, um den Mittelpunkt des Layouts, wie in den folgenden Screenshots gezeigt:

![](simple-images/rotate-anchors.png "Drehungsanimation mit Ankern")

### <a name="translation"></a>Übersetzung

Das folgende Codebeispiel veranschaulicht die Verwendung der [ `TranslateTo` ](xref:Xamarin.Forms.ViewExtensions.TranslateTo(Xamarin.Forms.VisualElement,System.Double,System.Double,System.UInt32,Xamarin.Forms.Easing)) Methode zum Animieren der [ `TranslationX` ](xref:Xamarin.Forms.VisualElement.TranslationX) und [ `TranslationY` ](xref:Xamarin.Forms.VisualElement.TranslationY) Eigenschaften eine [ `Image`](xref:Xamarin.Forms.Image):

```csharp
await image.TranslateTo (-100, -100, 1000);
```

Dieser Code erstellt eine Animation die [ `Image` ](xref:Xamarin.Forms.Image) Instanz übersetzt er horizontal und vertikal über eine Sekunde (1000 Millisekunden). Die [ `TranslateTo` ](xref:Xamarin.Forms.ViewExtensions.TranslateTo(Xamarin.Forms.VisualElement,System.Double,System.Double,System.UInt32,Xamarin.Forms.Easing)) Methode übersetzt gleichzeitig die Bildpixel 100 auf der linken Seite, und 100 Pixel nach oben. Dies ist, da es sich bei der ersten und zweiten Argument negativen Zahlen sind. Positive Zahlen bereitstellen, wird das Bild nach rechts und nach-unten übersetzt.

Die folgenden Screenshots zeigen die Übersetzung auf jeder Plattform ausgeführt:

![](simple-images/translateto.png "Translation-Animation")

> [!NOTE]
> Wenn ein Element aus dem ursprünglich angeordnet und klicken Sie dann auf den Bildschirm übersetzt, nach der Übersetzung des Elements eingabelayout bleibt, außerhalb des Bildschirms, und der Benutzer kann nicht mit ihm interagieren. Aus diesem Grund empfiehlt es sich, dass eine Ansicht, in der letzten Position angeordnet werden soll, und klicken Sie dann alle Übersetzungen ausgeführt erforderlichen.

### <a name="fading"></a>Ausblenden

Das folgende Codebeispiel veranschaulicht die Verwendung der [ `FadeTo` ](xref:Xamarin.Forms.ViewExtensions.FadeTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) Methode zum Animieren der [ `Opacity` ](xref:Xamarin.Forms.VisualElement.Opacity) Eigenschaft eine [ `Image` ](xref:Xamarin.Forms.Image):

```csharp
image.Opacity = 0;
await image.FadeTo (1, 4000);
```

Dieser Code erstellt eine Animation die [ `Image` ](xref:Xamarin.Forms.Image) Instanz, indem es in mehr als 4 Sekunden (4000 in Millisekunden) eingeblendet. Die [ `FadeTo` ](xref:Xamarin.Forms.ViewExtensions.FadeTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) Methode ruft die aktuelle [ `Opacity` ](xref:Xamarin.Forms.VisualElement.Opacity) Eigenschaftswert für den Beginn der Animation, und klicken Sie dann einblendet, von diesem Wert, der als erstes Argument (1).

Die folgenden Screenshots zeigen das Ausblenden auf jeder Plattform ausgeführt:

![](simple-images/fadeto.png "Ausblenden von Fenstern-Animation")

<a name="compound" />

## <a name="compound-animations"></a>Zusammengesetzte Animationen

Eine zusammengesetzte Animation ist eine sequenzielle Kombination von Animationen und können erstellt werden, mit der `await` -Operator verwenden, wie im folgenden Codebeispiel gezeigt:

```csharp
await image.TranslateTo (-100, 0, 1000);    // Move image left
await image.TranslateTo (-100, -100, 1000); // Move image up
await image.TranslateTo (100, 100, 2000);   // Move image diagonally down and right
await image.TranslateTo (0, 100, 1000);     // Move image left
await image.TranslateTo (0, 0, 1000);       // Move image up
```

In diesem Beispiel die [ `Image` ](xref:Xamarin.Forms.Image) wird mehr als 6 Sekunden (6000 in Millisekunden) übersetzt. Die Übersetzung von der `Image` verwendet fünf Animationen, mit der `await` Operator, der angibt, dass jede Animation wird sequenziell ausgeführt. Methoden der nachfolgenden Animation führen Sie nach Abschluss die vorherige Methode.

<a name="composite" />

## <a name="composite-animations"></a>Zusammengesetzte Animationen

Eine zusammengesetzte Animation ist eine Kombination von Animationen, in denen zwei oder mehr Animationen gleichzeitig ausgeführt werden. Zusammengesetzte Animationen können durch das Mischen von sehnsüchtigsten erwarteten und nicht erwartete Animationen erstellt werden, wie im folgenden Codebeispiel wird veranschaulicht:

```csharp
image.RotateTo (360, 4000);
await image.ScaleTo (2, 2000);
await image.ScaleTo (1, 2000);
```

In diesem Beispiel die [ `Image` ](xref:Xamarin.Forms.Image) skaliert wird, und gleichzeitig gedreht 4 Sekunden (4000 in Millisekunden). Die Skalierung der `Image` verwendet zwei aufeinanderfolgende Animationen, die zur gleichen Zeit wie die Drehung auftreten. Die [ `RotateTo` ](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) Methode ausgeführt wird, ohne eine `await` Operator und gibt sofort zurück, mit dem ersten [ `ScaleTo` ](xref:Xamarin.Forms.VisualElement.Scale) Animation dann ab. Die `await` Operator auf der ersten `ScaleTo` Methodenaufruf wird verzögert, die zweite `ScaleTo` Aufruf bis der ersten Methode `ScaleTo` Methodenaufruf abgeschlossen wurde. An diesem Punkt die `RotateTo` Animation ist die Hälfte der Weise abgeschlossen und die `Image` um 180 Grad gedreht werden. In den letzten 2 Sekunden (2000 Millisekunden) die zweite `ScaleTo` Animation und `RotateTo` Animation beide abgeschlossen.

### <a name="running-multiple-asynchronous-methods-concurrently"></a>Mehrere asynchrone Methoden gleichzeitig ausgeführt werden

Die `static` `Task.WhenAny` und `Task.WhenAll` Methoden werden verwendet, um mehrere asynchrone Methoden gleichzeitig ausgeführt werden und daher beim Erstellen von zusammengesetzter Animationen verwendet werden können. Beide Methoden zurückgeben einer `Task` Objekt aus, und eine Auflistung von Methoden akzeptieren, dass jeder Rückgabe eine `Task` Objekt. Die `Task.WhenAny` Methode abgeschlossen wird, wenn der Abschluss der Ausführung einer beliebigen Methode in der Sammlung wie im folgenden Codebeispiel veranschaulicht:

```csharp
await Task.WhenAny<bool>
(
  image.RotateTo (360, 4000),
  image.ScaleTo (2, 2000)
);
await image.ScaleTo (1, 2000);
```

In diesem Beispiel die `Task.WhenAny` Methodenaufruf enthält zwei Aufgaben. Die erste Aufgabe dreht das Bild 4 Sekunden (4000 in Millisekunden), und der zweite Task skaliert das Bild mehr als 2 Sekunden (2000 Millisekunden). Wenn der zweite Task abgeschlossen ist, die `Task.WhenAny` Methodenaufruf abgeschlossen ist. Allerdings, obwohl die [ `RotateTo` ](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) Methode wird weiterhin ausgeführt, die zweite [ `ScaleTo` ](xref:Xamarin.Forms.VisualElement.Scale) Methode beginnen kann.

Die `Task.WhenAll` Methode abgeschlossen wird, wenn alle Methoden in einer Auflistung abgeschlossen haben, wie im folgenden Codebeispiel gezeigt:

```csharp
// 10 minute animation
uint duration = 10 * 60 * 1000;

await Task.WhenAll (
  image.RotateTo (307 * 360, duration),
  image.RotateXTo (251 * 360, duration),
  image.RotateYTo (199 * 360, duration)
);
```

In diesem Beispiel die `Task.WhenAll` Methodenaufruf enthält drei Aufgaben, von denen jeder die mehr als 10 Minuten ausgeführt. Jede `Task` macht eine unterschiedliche Anzahl von 360-Grad-Drehungen – 307 Drehungen für [ `RotateTo` ](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)), 251 Drehungen für [ `RotateXTo` ](xref:Xamarin.Forms.ViewExtensions.RotateXTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)), und 199 Rotationen für [ `RotateYTo` ](xref:Xamarin.Forms.ViewExtensions.RotateYTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)). Diese Werte sind Primzahlen, damit stellen Sie sicher, dass die Rotationen sind nicht synchron und daher sich wiederholende Muster zu wird nicht an.

Die folgenden Screenshots zeigen die mehrere Rotationen auf jeder Plattform ausgeführt:

![](simple-images/multiple-rotations.png "Zusammengesetzte Animation")

## <a name="canceling-animations"></a>Abbrechen von Animationen

Eine Anwendung kann eine oder mehrere Animationen mit einem Aufruf von Abbrechen der `static` [ `ViewExtensions.CancelAnimations` ](xref:Xamarin.Forms.ViewExtensions.CancelAnimations(Xamarin.Forms.VisualElement)) -Methode, wie im folgenden Codebeispiel gezeigt:

```csharp
ViewExtensions.CancelAnimations (image);
```

Dies wird sofort abzubrechen, alle Animationen, die derzeit ausgeführt werden, auf die [ `Image` ](xref:Xamarin.Forms.Image) Instanz.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel gezeigt werden, erstellen und Abbrechen von Animationen mit der [ `ViewExtensions` ](xref:Xamarin.Forms.ViewExtensions) Klasse. Diese Klasse stellt die Erweiterungsmethoden, die verwendet werden können, um einfache Animationen erstellen, die drehen, skalieren, übersetzen und fade [ `VisualElement` ](xref:Xamarin.Forms.VisualElement) Instanzen.

## <a name="related-links"></a>Verwandte Links

- [Async Support Overview (Übersicht über die asynchrone Unterstützung)](~/cross-platform/platform/async.md)
- [Grundlegende Animation (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-animation-basic)
- [ViewExtensions](xref:Xamarin.Forms.ViewExtensions)
