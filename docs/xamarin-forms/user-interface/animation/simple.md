---
title: Einfache Animationen in Xamarin.Forms
description: Die ViewExtensions-Klasse bietet Erweiterungsmethoden, die zum Erstellen von einfacher Animationen verwendet werden können. Dieser Artikel veranschaulicht das Erstellen und Abbrechen von Animationen mithilfe der ViewExtensions-Klasse.
ms.prod: xamarin
ms.assetid: 4A6FAE5A-848F-4CE0-BFA1-22A6309B5225
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/27/2017
ms.openlocfilehash: c0e2383d152db0b5055558a22386cafd8d27a059
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/08/2018
ms.locfileid: "35243004"
---
# <a name="simple-animations-in-xamarinforms"></a>Einfache Animationen in Xamarin.Forms

_Die ViewExtensions-Klasse bietet Erweiterungsmethoden, die zum Erstellen von einfacher Animationen verwendet werden können. Dieser Artikel veranschaulicht das Erstellen und Abbrechen von Animationen mithilfe der ViewExtensions-Klasse._


Die [ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/) Klasse enthält die folgenden Erweiterungsmethoden, die zum Erstellen von einfacher Animationen verwendet werden können:

- [`TranslateTo`](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.TranslateTo/p/Xamarin.Forms.VisualElement/System.Double/System.Double/System.UInt32/Xamarin.Forms.Easing/) erstellt eine Animation der [ `TranslationX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationX/) und [ `TranslationY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationY/) Eigenschaften einer [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/).
- [`ScaleTo`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) erstellt eine Animation der [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) Eigenschaft von einem [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/).
- [`RelScaleTo`](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RelScaleTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) Wendet eine animierte stufenweisen Erhöhung oder Verringerung der [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) Eigenschaft eine [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/).
- [`RotateTo`](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) erstellt eine Animation der [ `Rotation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/) Eigenschaft von einem [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/).
- [`RelRotateTo`](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RelRotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) Wendet eine animierte stufenweisen Erhöhung oder Verringerung der [ `Rotation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/) Eigenschaft eine [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/).
- [`RotateXTo`](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateXTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) erstellt eine Animation der [ `RotationX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.RotationX/) Eigenschaft von einem [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/).
- [`RotateYTo`](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateYTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) erstellt eine Animation der [ `RotationY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.RotationY/) Eigenschaft von einem [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/).
- [`FadeTo`](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.FadeTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) erstellt eine Animation der [ `Opacity` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Opacity/) Eigenschaft von einem [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/).

Standardmäßig wird jede Animation 250 Millisekunden dauert. Allerdings kann eine Dauer für jede Animation angegeben werden, für die Erstellung die Animation.

Die [ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/) Klasse enthält auch eine [ `CancelAnimations` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.CancelAnimations/p/Xamarin.Forms.VisualElement/) -Methode, die verwendet werden kann, um alle Animationen abzubrechen.

> [!NOTE]
> Die [ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/) Klasse bietet einen [ `LayoutTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.LayoutTo/p/Xamarin.Forms.VisualElement/Xamarin.Forms.Rectangle/System.UInt32/Xamarin.Forms.Easing/) Erweiterungsmethode. Diese Methode soll jedoch Übergänge zwischen Zuständen Layout animieren, die Größe enthalten, und positionieren Sie die Änderungen durch Layouts verwendet werden. Sollte daher nur von verwendet werden [ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/) Unterklassen.

Die Animation Erweiterungsmethoden in den [ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/) Klasse sind, alle asynchron und return ein `Task<bool>` Objekt. Der Rückgabewert ist `false` Wenn die Animation ausgeführt wird, und `true` , wenn die Animation abgebrochen wird. Daher die Methoden für die Animation sollte in der Regel verwendet werden, mit der `await` -Operator, der es ermöglicht, nur schwer bestimmen, wann eine Animation abgeschlossen ist. Darüber hinaus wird es dann möglich, sequenzielle Animationen mit nachfolgenden Animation-Methoden, die Ausführung nach Abschluss die vorherige Methode zu erstellen. Weitere Informationen finden Sie unter [Animationen zusammengesetzte](#compound).

Es ist erforderlich, können eine Animation im Hintergrund abgeschlossen und dann die `await` Operator kann ausgelassen werden. In diesem Szenario werden die Erweiterungsmethoden für die Animation schnell zurückgegeben, nach dem Initiieren der Animation, mit der Animation, die im Hintergrund ausgeführt. Dieser Vorgang kann genutzt werden beim Erstellen von zusammengesetzter Animationen. Weitere Informationen finden Sie unter [zusammengesetzten Animationen](#composite).

Weitere Informationen zu den `await` -Operator, finden Sie unter [Überblick über den asynchronen](~/cross-platform/platform/async.md).

## <a name="single-animations"></a>Einzelne Animationen

Jede Erweiterungsmethode in der [ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/) implementiert einen einzelne Animation-Vorgang, der schrittweise eine Eigenschaft von einem Wert in einen anderen Wert über einen Zeitraum geändert wird. In diesem Abschnitt wird erklärt, jeden Animation-Vorgang.

### <a name="rotation"></a>Drehung

Das folgende Codebeispiel veranschaulicht die Verwendung der [ `RotateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) Methode zum Animieren der [ `Rotation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/) Eigenschaft ein [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/):

```csharp
await image.RotateTo (360, 2000);
image.Rotation = 0;
```

Dieser Code erstellt eine Animation der [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) Instanz, indem Sie bis zu 360 Grad drehen, mehr als 2 Sekunden (2000 Millisekunden). Die [ `RotateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) Methode ruft die aktuelle [ `Rotation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/) Eigenschaft-Wert für den Beginn der Animation, und klicken Sie dann auf das erste Argument (360) von diesem Wert dreht. Sobald die Animation wird abgeschlossen haben, des Bilds [ `Rotation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/) Eigenschaft auf 0 zurückgesetzt. Dadurch wird sichergestellt, dass die `Rotation` Eigenschaft nicht auf 360 bleibt, nachdem die Animation endet mit dem zusätzliche Drehungen verhindern würden,.

Die folgenden Screenshots zeigen die Rotation auf jeder Plattform ausgeführt:

![](simple-images/rotateto.png "Drehungsanimation")

### <a name="relative-rotation"></a>Relative Drehung

Das folgende Codebeispiel veranschaulicht die Verwendung der [ `RelRotateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RelRotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) Methode, um schrittweise erhöhen oder verringern Sie die [ `Rotation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/) Eigenschaft ein [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/):

```csharp
await image.RelRotateTo (360, 2000);
```

Dieser Code erstellt eine Animation der [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) Instanz, indem Sie drehen 360 Grad von der Startposition mehr als 2 Sekunden (2000 Millisekunden). Die [ `RelRotateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RelRotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) Methode ruft die aktuelle [ `Rotation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/) Eigenschaft Wert für den Beginn der Animation, und klicken Sie dann auf den Wert plus erstes Argument (360) von diesem Wert dreht. Dadurch wird sichergestellt, dass jede Animation immer eine Drehung im 360 Grad von der Startposition. Daher, wenn eine neue Animation aufgerufen wird, während eine Animation bereits ausgeführt wird, sie wird von der aktuellen Position gestartet und kann an eine Position, die keinem Inkrement von 360 Grad beenden.

Die folgenden Screenshots zeigen die relative Rotation auf jeder Plattform ausgeführt:

![](simple-images/relrotateto.png "Relative Drehungsanimation")

### <a name="scaling"></a>Skalieren

Das folgende Codebeispiel veranschaulicht die Verwendung der [ `ScaleTo` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) Methode zum Animieren der [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) Eigenschaft ein [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/):

```csharp
await image.ScaleTo (2, 2000);
```

Dieser Code erstellt eine Animation der [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) Instanz durch zentrales Skalieren auf zwei Mal die Größe über 2 Sekunden (2000 Millisekunden). Die [ `ScaleTo` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) Methode ruft die aktuelle [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) Eigenschaftswert (Standardwert 1) für den Beginn der Animation, und klicken Sie dann Skalierung von diesem Wert auf das erste Argument (2). Dies wirkt sich die von der Größe des Bilds auf die doppelte Größe erweitert.

Die folgenden Screenshots zeigen die Skalierung auf jeder Plattform ausgeführt:

![](simple-images/scaleto.png "Animation-Skalierung")

### <a name="relative-scaling"></a>Relative Skalierung

Das folgende Codebeispiel veranschaulicht die Verwendung der [ `RelScaleTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RelScaleTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) Methode zum Animieren der [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) Eigenschaft ein [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/):

```csharp
await image.RelScaleTo (2, 2000);
```

Dieser Code erstellt eine Animation der [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) Instanz durch zentrales Skalieren auf zwei Mal die Größe über 2 Sekunden (2000 Millisekunden). Die [ `RelScaleTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RelScaleTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) Methode ruft die aktuelle [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) Eigenschaftswert für den Beginn der Animation, und klicken Sie dann Skalen von diesem Wert dem Wert plus erstes Argument (2). Dadurch wird sichergestellt, dass jede Animation immer eine Skalierung von 2 von der Startposition.

### <a name="scaling-and-rotation-with-anchors"></a>Skalierung und Drehung mit Anker

Die [ `AnchorX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.AnchorX/) und [ `AnchorY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.AnchorY/) Eigenschaften festlegen, die Mitte der Skalierung oder Drehung für die [ `Rotation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/) und [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) Eigenschaften. Deshalb deren Werte auch Einfluss auf die [ `RotateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) und [ `ScaleTo` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) Methoden.

Erhält ein [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) in der Mitte eines Layouts platziert wurde, wird im folgenden Codebeispiel wird veranschaulicht, drehen das Bild, um den Mittelpunkt des Layouts durch Festlegen seiner [ `AnchorY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.AnchorY/) Eigenschaft:

```csharp
image.AnchorY = (Math.Min (absoluteLayout.Width, absoluteLayout.Height) / 2) / image.Height;
await image.RotateTo (360, 2000);
```

Drehen der [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) Instanz um den Mittelpunkt der das Layout der [ `AnchorX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.AnchorX/) und [ `AnchorY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.AnchorY/) Eigenschaften müssen auf Werte festgelegt werden relativ zur Breite und Höhe der `Image`. In diesem Beispiel das Zentrum des der `Image` ist definiert werden, in der Mitte des Layouts, weshalb der Standard `AnchorX` Wert von 0,5 nicht geändert werden muss. Allerdings die `AnchorY` Eigenschaft neu definiert wird, einen Wert vom oberen Rand der `Image` für den Mittelpunkt des Layouts. Dadurch wird sichergestellt, dass die `Image` stellt eine vollständige Drehung von 360 Grad, um den Mittelpunkt des Layouts, wie in den folgenden Screenshots dargestellt:

![](simple-images/rotate-anchors.png "Drehungsanimation Anker")

### <a name="translation"></a>Übersetzung

Das folgende Codebeispiel veranschaulicht die Verwendung der [ `TranslateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.TranslateTo/p/Xamarin.Forms.VisualElement/System.Double/System.Double/System.UInt32/Xamarin.Forms.Easing/) Methode zum Animieren der [ `TranslationX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationX/) und [ `TranslationY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationY/) Eigenschaften einer [ `Image`](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/):

```csharp
await image.TranslateTo (-100, -100, 1000);
```

Dieser Code erstellt eine Animation der [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) Instanz durch Übersetzen sie horizontal und vertikal über 1 Sekunde (1000 Millisekunden). Die [ `TranslateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.TranslateTo/p/Xamarin.Forms.VisualElement/System.Double/System.Double/System.UInt32/Xamarin.Forms.Easing/) Methode gleichzeitig übersetzt, die 100 Pixel auf der linken Seite und nach oben 100 Pixel. Dies ist, da der erste und das zweite Argument beide negativen Zahlen sind. Positive Zahlen bereitstellen, würde das Bild nach rechts und nach-unten übersetzen.

Die folgenden Screenshots zeigen die Übersetzung auf jeder Plattform ausgeführt:

![](simple-images/translateto.png "Translation-Animation")

> [!NOTE]
> Wenn ein Element wird zunächst außerhalb des Bildschirms angeordnet, und klicken Sie dann auf dem Bildschirm übersetzt, nach der Übersetzung bleibt die Eingabe Elementlayouts außerhalb des Bildschirms, und der Benutzer interagieren kann nicht. Aus diesem Grund wird empfohlen, dass eine Ansicht, in der letzten Position angeordnet werden soll, und klicken Sie dann alle Übersetzungen ausgeführt erforderlichen.

### <a name="fading"></a>Ausblenden

Das folgende Codebeispiel veranschaulicht die Verwendung der [ `FadeTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.FadeTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) Methode zum Animieren der [ `Opacity` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Opacity/) Eigenschaft ein [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/):

```csharp
image.Opacity = 0;
await image.FadeTo (1, 4000);
```

Dieser Code erstellt eine Animation der [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) Instanz durch Ausblenden es in mehr als 4 Sekunden (4000 in Millisekunden). Die [ `FadeTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.FadeTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) Methode ruft die aktuelle [ `Opacity` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Opacity/) Eigenschaftswert für den Beginn der Animation, und klicken Sie dann einblendet von diesem Wert an das erste Argument (1).

Die folgenden Screenshots zeigen das einblenden auf jeder Plattform ausgeführt:

![](simple-images/fadeto.png "Animation ausblenden")

<a name="compound" />

## <a name="compound-animations"></a>Zusammengesetzte Animationen

Eine zusammengesetzte Animation ist eine sequenzielle Kombination von Animationen und erstellt werden können, mit der `await` -Operator, wie im folgenden Codebeispiel gezeigt:

```csharp
await image.TranslateTo (-100, 0, 1000);    // Move image left
await image.TranslateTo (-100, -100, 1000); // Move image up
await image.TranslateTo (100, 100, 2000);   // Move image diagonally down and right
await image.TranslateTo (0, 100, 1000);     // Move image left
await image.TranslateTo (0, 0, 1000);       // Move image up
```

In diesem Beispiel wird die [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) wird mehr als 6 Sekunden (6000 in Millisekunden) übersetzt. Die Übersetzung der `Image` verwendet fünf Animationen mit der `await` Operator, der angibt, dass jede Animation sequenziell ausgeführt wird. Führen daher nachfolgende Animation Methoden auf, nach dem Abschluss der vorherigen Methode.

<a name="composite" />

## <a name="composite-animations"></a>Zusammengesetzte Animationen

Eine zusammengesetzte Animation ist eine Kombination von Animationen, in denen zwei oder mehr Animationen gleichzeitig ausgeführt werden. Zusammengesetzte Animationen können durch das Mischen von erwarteten und nicht erwartet Animationen erstellt werden, wie im folgenden Codebeispiel wird veranschaulicht:

```csharp
image.RotateTo (360, 4000);
await image.ScaleTo (2, 2000);
await image.ScaleTo (1, 2000);
```

In diesem Beispiel wird die [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) wird skaliert, und gleichzeitig gedreht über 4 Sekunden (4000 in Millisekunden). Die Skalierung der `Image` verwendet zwei aufeinanderfolgende Animationen, die zur gleichen Zeit wie die Drehung auftreten. Die [ `RotateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) Methode ausgeführt wird, ohne eine `await` Operator und gibt sofort zurück, mit dem ersten [ `ScaleTo` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) Animation dann ab. Die `await` Operator auf der ersten `ScaleTo` Methodenaufruf verzögert die zweite `ScaleTo` -Methodenaufruf, bis die erste `ScaleTo` Methodenaufruf abgeschlossen wurde. An diesem Punkt der `RotateTo` Animation ist die Hälfte der Weise abgeschlossen und die `Image` 180 Grad gedreht werden. In den letzten 2 Sekunden (2000 Millisekunden) die zweite `ScaleTo` Animation und dem `RotateTo` Animation beide abgeschlossen.

### <a name="running-multiple-asynchronous-methods-concurrently"></a>Mehrere asynchrone Methoden gleichzeitig ausgeführt werden

Die `static` `Task.WhenAny` und `Task.WhenAll` Methoden werden verwendet, um mehrere asynchrone Methoden gleichzeitig ausgeführt werden und kann daher verwendet werden, um zusammengesetzte Animationen erstellen können. Beide Methoden zurückgeben einer `Task` Objekt und eine Auflistung von Methoden akzeptieren, dass jedes return ein `Task` Objekt. Die `Task.WhenAny` Methode abgeschlossen wird, Abschluss einer beliebigen Methode in der Auflistung Ausführung, wie im folgenden Codebeispiel gezeigt:

```csharp
await Task.WhenAny<bool>
(
  image.RotateTo (360, 4000),
  image.ScaleTo (2, 2000)
);
await image.ScaleTo (1, 2000);
```

In diesem Beispiel wird die `Task.WhenAny` Methodenaufruf enthält zwei Aufgaben. Die erste Aufgabe dreht das Bild über 4 Sekunden (4000 in Millisekunden), und die zweite Aufgabe wird das Bild skaliert mehr als 2 Sekunden (2000 Millisekunden). Wenn die zweite Aufgabe abgeschlossen ist, die `Task.WhenAny` Methodenaufruf abgeschlossen ist. Allerdings, obwohl die [ `RotateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) Methode noch ausgeführt wird, das zweite [ `ScaleTo` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) Methode beginnen kann.

Die `Task.WhenAll` Methode abgeschlossen wird, wenn alle Methoden in der Auflistung abgeschlossen haben, wie im folgenden Codebeispiel gezeigt:

```csharp
// 10 minute animation
uint duration = 10 * 60 * 1000;

await Task.WhenAll (
  image.RotateTo (307 * 360, duration),
  image.RotateXTo (251 * 360, duration),
  image.RotateYTo (199 * 360, duration)
);
```

In diesem Beispiel wird die `Task.WhenAll` Methodenaufruf enthält drei Tasks, von denen jedes über 10 Minuten lang ausgeführt. Jede `Task` macht eine unterschiedliche Anzahl von 360-Grad-Drehungen – 307 Drehungen für [ `RotateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/), 251 Drehungen für [ `RotateXTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateXTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/), und 199 Drehungen für [ `RotateYTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateYTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/). Diese Werte sind Primzahlen, daher sicherstellen, dass die Drehungen sind nicht synchron und daher wird nicht, sich wiederholenden Muster dazu an.

Die folgenden Screenshots zeigen mehrere Drehungen auf jeder Plattform ausgeführt:

![](simple-images/multiple-rotations.png "Zusammengesetzte Animation")

## <a name="canceling-animations"></a>Abbrechen von Animationen

Eine Anwendung kann eine oder mehrere Animationen mit einem Aufruf von "Abbrechen" die `static` [ `ViewExtensions.CancelAnimations` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.CancelAnimations/p/Xamarin.Forms.VisualElement/) Methode, wie im folgenden Codebeispiel gezeigt:

```csharp
ViewExtensions.CancelAnimations (image);
```

Hiermit wird sofort abgebrochen. alle Animationen, die zurzeit ausgeführt werden, auf die [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) Instanz.

## <a name="summary"></a>Zusammenfassung

Dieser Artikel veranschaulicht, erstellen und Abbrechen von Animationen mithilfe der [ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/) Klasse. Diese Klasse bietet Erweiterungsmethoden, mit der einfache Animationen erstellen, die drehen, zu skalieren, zu übersetzen und zur menüausblendung [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/) Instanzen.


## <a name="related-links"></a>Verwandte Links

- [Async Support Overview (Übersicht über die asynchrone Unterstützung)](~/cross-platform/platform/async.md)
- [Grundlegende Animation (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/animation/basic/)
- [ViewExtensions](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/)
