---
title: Einfache Animationen inXamarin.Forms
description: Die viewextensions-Klasse stellt Erweiterungs Methoden bereit, die verwendet werden können, um einfache Animationen zu erstellen. Dieser Artikel veranschaulicht das Erstellen und Abbrechen von Animationen mithilfe der viewextensions-Klasse.
ms.prod: xamarin
ms.assetid: 4A6FAE5A-848F-4CE0-BFA1-22A6309B5225
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/05/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: b13ec7ab079dcf7069b5f4b0dccbb52faf25f927
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86933795"
---
# <a name="simple-animations-in-xamarinforms"></a>Einfache Animationen inXamarin.Forms

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-animation-basic)

_Die viewextensions-Klasse stellt Erweiterungs Methoden bereit, die verwendet werden können, um einfache Animationen zu erstellen. Dieser Artikel veranschaulicht das Erstellen und Abbrechen von Animationen mithilfe der viewextensions-Klasse._

Die- [`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions) Klasse stellt die folgenden Erweiterungs Methoden bereit, die verwendet werden können, um einfache Animationen zu erstellen:

- [ `TranslateTo` ] (Xref: Xamarin.Forms . Viewextensions. translateto ( Xamarin.Forms . Visualelement, System. Double, System. Double, System. UInt32, Xamarin.Forms . Beschleunigung)) animiert die [`TranslationX`](xref:Xamarin.Forms.VisualElement.TranslationX) -Eigenschaft und die-Eigenschaft [`TranslationY`](xref:Xamarin.Forms.VisualElement.TranslationY) eines [`VisualElement`](xref:Xamarin.Forms.VisualElement) .
- [`ScaleTo`](xref:Xamarin.Forms.ViewExtensions.ScaleTo*)animiert die- [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) Eigenschaft eines [`VisualElement`](xref:Xamarin.Forms.VisualElement) .
- `ScaleXTo`animiert die- [`ScaleX`](xref:Xamarin.Forms.VisualElement.ScaleX) Eigenschaft eines [`VisualElement`](xref:Xamarin.Forms.VisualElement) .
- `ScaleYTo`animiert die- [`ScaleY`](xref:Xamarin.Forms.VisualElement.ScaleY) Eigenschaft eines [`VisualElement`](xref:Xamarin.Forms.VisualElement) .
- [ `RelScaleTo` ] (Xref: Xamarin.Forms . Viewextensions. relscaleto ( Xamarin.Forms . Visualelement, System. Double, System. UInt32, Xamarin.Forms . Beschleunigung)) wendet eine animierte inkrementelle Erhöhung oder Verringerung der- [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) Eigenschaft eines an [`VisualElement`](xref:Xamarin.Forms.VisualElement) .
- [ `RotateTo` ] (Xref: Xamarin.Forms . Viewextensions. rotateTo ( Xamarin.Forms . Visualelement, System. Double, System. UInt32, Xamarin.Forms . Beschleunigung)) animiert die- [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation) Eigenschaft eines [`VisualElement`](xref:Xamarin.Forms.VisualElement) .
- [ `RelRotateTo` ] (Xref: Xamarin.Forms . Viewextensions. relrotateto ( Xamarin.Forms . Visualelement, System. Double, System. UInt32, Xamarin.Forms . Beschleunigung)) wendet eine animierte inkrementelle Erhöhung oder Verringerung der- [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation) Eigenschaft eines an [`VisualElement`](xref:Xamarin.Forms.VisualElement) .
- [ `RotateXTo` ] (Xref: Xamarin.Forms . Viewextensions. rotatexto ( Xamarin.Forms . Visualelement, System. Double, System. UInt32, Xamarin.Forms . Beschleunigung)) animiert die- [`RotationX`](xref:Xamarin.Forms.VisualElement.RotationX) Eigenschaft eines [`VisualElement`](xref:Xamarin.Forms.VisualElement) .
- [ `RotateYTo` ] (Xref: Xamarin.Forms . Viewextensions. rotateyto ( Xamarin.Forms . Visualelement, System. Double, System. UInt32, Xamarin.Forms . Beschleunigung)) animiert die- [`RotationY`](xref:Xamarin.Forms.VisualElement.RotationY) Eigenschaft eines [`VisualElement`](xref:Xamarin.Forms.VisualElement) .
- [ `FadeTo` ] (Xref: Xamarin.Forms . Viewextensions. Fade to ( Xamarin.Forms . Visualelement, System. Double, System. UInt32, Xamarin.Forms . Beschleunigung)) animiert die- [`Opacity`](xref:Xamarin.Forms.VisualElement.Opacity) Eigenschaft eines [`VisualElement`](xref:Xamarin.Forms.VisualElement) .

Standardmäßig dauert jede Animation 250 Millisekunden. Beim Erstellen der Animation kann jedoch eine Dauer für jede Animation angegeben werden.

Die- [`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions) Klasse enthält auch ein [ `CancelAnimations` ] (Xref:) Xamarin.Forms . Viewextensions. cancelanimationen ( Xamarin.Forms . Visualelement))-Methode, die zum Abbrechen von Animationen verwendet werden kann.

> [!NOTE]
> Die- [`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions) Klasse stellt einen [ `LayoutTo` ] (Xref:) bereit Xamarin.Forms . Viewextensions. layoutto ( Xamarin.Forms . Visualelement, Xamarin.Forms . Rechteck, System. UInt32, Xamarin.Forms . Beschleunigung)), Erweiterungsmethode. Diese Methode soll jedoch von Layouts verwendet werden, um Übergänge zwischen layoutzuständen zu animieren, die Größen-und Positionsänderungen enthalten. Daher sollte Sie nur von [`Layout`](xref:Xamarin.Forms.Layout) Unterklassen verwendet werden.

Die Animations Erweiterungs Methoden in der [`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions) -Klasse sind alle asynchron und geben ein- `Task<bool>` Objekt zurück. Der Rückgabewert ist `false` , wenn die Animation abgeschlossen ist, und, `true` Wenn die Animation abgebrochen wird. Daher sollten die Animations Methoden in der Regel mit dem- `await` Operator verwendet werden, sodass Sie problemlos bestimmen können, wann eine Animation abgeschlossen wurde. Außerdem wird es möglich, sequenzielle Animationen mit nachfolgenden Animations Methoden zu erstellen, die nach Abschluss der vorherigen Methode ausgeführt werden. Weitere Informationen finden Sie unter zusammen [gesetzte Animationen](#compound-animations).

Wenn die Anforderung besteht, eine Animation im Hintergrund abzuschließen, `await` kann der Operator ausgelassen werden. In diesem Szenario werden die Animations Erweiterungs Methoden nach dem Initiieren der Animation schnell zurückgegeben, wobei die Animation im Hintergrund stattfindet. Dieser Vorgang kann beim Erstellen von zusammengesetzten Animationen genutzt werden. Weitere Informationen finden Sie unter zusammen [gesetzte Animationen](#composite-animations).

Weitere Informationen zum- `await` Operator finden Sie [unter async-Unterstützung: Übersicht](~/cross-platform/platform/async.md).

## <a name="single-animations"></a>Einzelne Animationen

Jede Erweiterungsmethode in [`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions) implementiert einen einzelnen Animations Vorgang, der eine Eigenschaft in einem bestimmten Zeitraum progressiv von einem Wert in einen anderen Wert ändert. In diesem Abschnitt wird jeder Animations Vorgang erläutert.

### <a name="rotation"></a>Rotation

Im folgenden Codebeispiel wird die Verwendung von [ `RotateTo` ] (Xref:) veranschaulicht Xamarin.Forms . Viewextensions. rotateTo ( Xamarin.Forms . Visualelement, System. Double, System. UInt32, Xamarin.Forms . Beschleunigung)) Methode zum Animieren der [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation) Eigenschaft eines [`Image`](xref:Xamarin.Forms.Image) :

```csharp
await image.RotateTo (360, 2000);
image.Rotation = 0;
```

Dieser Code animiert die [`Image`](xref:Xamarin.Forms.Image) Instanz, indem Sie um bis zu 360 Grad über 2 Sekunden (2000 Millisekunden) rotiert. [ `RotateTo` ] (Xref: Xamarin.Forms . Viewextensions. rotateTo ( Xamarin.Forms . Visualelement, System. Double, System. UInt32, Xamarin.Forms . Beschleunigung)) die-Methode ruft den aktuellen [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation) -Eigenschafts Wert für den Beginn der Animation ab und rotiert dann von diesem Wert zum ersten Argument (360). Nachdem die Animation fertiggestellt wurde, wird die-Eigenschaft des Bilds [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation) auf 0 zurückgesetzt. Dadurch wird sichergestellt, dass die- `Rotation` Eigenschaft nach dem Abschluss der Animation nicht bei 360 bleibt, was zusätzliche Rotationen verhindern würde.

Die folgenden Screenshots zeigen die Drehung, die auf den einzelnen Plattformen ausgeführt wird:

![Rotations Animation](simple-images/rotateto.png)

> [!NOTE]
> Zusätzlich zu [ `RotateTo` ] (Xref: Xamarin.Forms . Viewextensions. rotateTo ( Xamarin.Forms . Visualelement, System. Double, System. UInt32, Xamarin.Forms . Beschleunigung))-Methode, es gibt auch [ `RotateXTo` ] (Xref: Xamarin.Forms . Viewextensions. rotatexto ( Xamarin.Forms . Visualelement, System. Double, System. UInt32, Xamarin.Forms . Beschleunigung)) und [ `RotateYTo` ] (Xref: Xamarin.Forms . Viewextensions. rotateyto ( Xamarin.Forms . Visualelement, System. Double, System. UInt32, Xamarin.Forms . Beschleunigung)) Methoden, die die [`RotationX`](xref:Xamarin.Forms.VisualElement.RotationX) -Eigenschaft bzw. die-Eigenschaft animieren [`RotationY`](xref:Xamarin.Forms.VisualElement.RotationY) .

### <a name="relative-rotation"></a>Relative Drehung

Im folgenden Codebeispiel wird die Verwendung von [ `RelRotateTo` ] (Xref:) veranschaulicht Xamarin.Forms . Viewextensions. relrotateto ( Xamarin.Forms . Visualelement, System. Double, System. UInt32, Xamarin.Forms . Beschleunigung))-Methode, um die-Eigenschaft eines inkrementell zu vergrößern oder zu verkleinern [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation) [`Image`](xref:Xamarin.Forms.Image) :

```csharp
await image.RelRotateTo (360, 2000);
```

Dieser Code animiert die [`Image`](xref:Xamarin.Forms.Image) Instanz, indem Sie 360 Grad von der Startposition über 2 Sekunden (2000 Millisekunden) rotiert. [ `RelRotateTo` ] (Xref: Xamarin.Forms . Viewextensions. relrotateto ( Xamarin.Forms . Visualelement, System. Double, System. UInt32, Xamarin.Forms . Beschleunigung)) die Methode ruft den aktuellen [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation) Eigenschafts Wert für den Beginn der Animation ab und rotiert dann von diesem Wert zu dem Wert plus dem ersten Argument (360). Dadurch wird sichergestellt, dass jede Animation immer eine 360 Grad-Drehung von der Anfangsposition aus ist. Wenn eine neue Animation aufgerufen wird, während eine Animation bereits ausgeführt wird, wird Sie daher von der aktuellen Position aus gestartet und kann an einer Position enden, die kein Inkrement von 360 Grad ist.

Die folgenden Screenshots zeigen die relative Drehung, die auf den einzelnen Plattformen ausgeführt wird:

![Relative Rotations Animation](simple-images/relrotateto.png)

### <a name="scaling"></a>Skalierung

Im folgenden Codebeispiel wird veranschaulicht, wie die-Methode verwendet wird [`ScaleTo`](xref:Xamarin.Forms.ViewExtensions.ScaleTo*) , um die- [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) Eigenschaft eines zu animieren [`Image`](xref:Xamarin.Forms.Image) :

```csharp
await image.ScaleTo (2, 2000);
```

Mit diesem Code wird die- [`Image`](xref:Xamarin.Forms.Image) Instanz animiert, indem eine Skalierung auf eine doppelte Größe über 2 Sekunden (2000 Millisekunden) ausgeführt wird. Die [`ScaleTo`](xref:Xamarin.Forms.ViewExtensions.ScaleTo*) -Methode ruft den aktuellen- [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) Eigenschafts Wert (Standardwert 1) für den Start der Animation ab und skaliert dann von diesem Wert auf das erste Argument (2). Dies hat den Effekt, dass die Größe des Bilds auf die doppelte Größe erweitert wird.

Die folgenden Screenshots zeigen, wie die Skalierung auf den einzelnen Plattformen ausgeführt wird:

![Skalieren der Animation](simple-images/scaleto.png)

> [!NOTE]
> Zusätzlich zur [`ScaleTo`](xref:Xamarin.Forms.ViewExtensions.ScaleTo*) -Methode gibt es auch `ScaleXTo` -und- `ScaleYTo` Methoden, die die [`ScaleX`](xref:Xamarin.Forms.VisualElement.ScaleX) -Eigenschaft und die-Eigenschaft animieren [`ScaleY`](xref:Xamarin.Forms.VisualElement.ScaleY) .

### <a name="relative-scaling"></a>Relative Skalierung

Im folgenden Codebeispiel wird die Verwendung von [ `RelScaleTo` ] (Xref:) veranschaulicht Xamarin.Forms . Viewextensions. relscaleto ( Xamarin.Forms . Visualelement, System. Double, System. UInt32, Xamarin.Forms . Beschleunigung)) Methode zum Animieren der [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) Eigenschaft eines [`Image`](xref:Xamarin.Forms.Image) :

```csharp
await image.RelScaleTo (2, 2000);
```

Mit diesem Code wird die- [`Image`](xref:Xamarin.Forms.Image) Instanz animiert, indem eine Skalierung auf eine doppelte Größe über 2 Sekunden (2000 Millisekunden) ausgeführt wird. [ `RelScaleTo` ] (Xref: Xamarin.Forms . Viewextensions. relscaleto ( Xamarin.Forms . Visualelement, System. Double, System. UInt32, Xamarin.Forms . Beschleunigung)) die Methode ruft den aktuellen [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) Eigenschafts Wert für den Start der Animation ab und skaliert dann von diesem Wert auf den Wert plus das erste Argument (2). Dadurch wird sichergestellt, dass jede Animation immer eine Skalierung von 2 von der Startposition aus ist.

### <a name="scaling-and-rotation-with-anchors"></a>Skalierung und Drehung mit Anker

Die [`AnchorX`](xref:Xamarin.Forms.VisualElement.AnchorX) [`AnchorY`](xref:Xamarin.Forms.VisualElement.AnchorY) Eigenschaften und legen den Mittelpunkt der Skalierung oder Drehung für die-Eigenschaft und die-Eigenschaft fest [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation) [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) . Daher wirken sich ihre Werte auch auf [ `RotateTo` ] (Xref:) aus Xamarin.Forms . Viewextensions. rotateTo ( Xamarin.Forms . Visualelement, System. Double, System. UInt32, Xamarin.Forms . Beschleunigung)) und [`ScaleTo`](xref:Xamarin.Forms.ViewExtensions.ScaleTo*) Methoden.

Wenn ein- [`Image`](xref:Xamarin.Forms.Image) Objekt in der Mitte eines Layouts platziert wurde, veranschaulicht das folgende Codebeispiel, wie das Bild um den Mittelpunkt des Layouts gedreht wird, indem die zugehörige-Eigenschaft festgelegt wird [`AnchorY`](xref:Xamarin.Forms.VisualElement.AnchorY) :

```csharp
double radius = Math.Min(absoluteLayout.Width, absoluteLayout.Height) / 2;
image.AnchorY = radius / image.Height;
await image.RotateTo(360, 2000);
```

Um die [`Image`](xref:Xamarin.Forms.Image) Instanz um die Mitte des Layouts zu drehen, [`AnchorX`](xref:Xamarin.Forms.VisualElement.AnchorX) müssen die-Eigenschaft und die-Eigenschaft auf- [`AnchorY`](xref:Xamarin.Forms.VisualElement.AnchorY) Werte festgelegt werden, die relativ zur Breite und Höhe von sind `Image` . In diesem Beispiel wird der Mittelpunkt des `Image` -Werts so definiert, dass er sich in der Mitte des Layouts befindet, sodass der Standard `AnchorX` Wert 0,5 nicht geändert werden muss. Die- `AnchorY` Eigenschaft wird jedoch neu definiert, sodass Sie ein Wert von der obersten Position des `Image` bis zum Mittelpunkt des Layouts ist. Dadurch wird sichergestellt, dass die `Image` eine vollständige Drehung um 360 Grad um den Mittelpunkt des Layouts führt, wie in den folgenden Screenshots gezeigt:

![Rotations Animation mit Anker](simple-images/rotate-anchors.png)

### <a name="translation"></a>Sprachübersetzung

Im folgenden Codebeispiel wird die Verwendung von [ `TranslateTo` ] (Xref:) veranschaulicht Xamarin.Forms . Viewextensions. translateto ( Xamarin.Forms . Visualelement, System. Double, System. Double, System. UInt32, Xamarin.Forms . Beschleunigung))-Methode, um die-Eigenschaft und die-Eigenschaft eines zu animieren [`TranslationX`](xref:Xamarin.Forms.VisualElement.TranslationX) [`TranslationY`](xref:Xamarin.Forms.VisualElement.TranslationY) [`Image`](xref:Xamarin.Forms.Image) :

```csharp
await image.TranslateTo (-100, -100, 1000);
```

Mit diesem Code wird die [`Image`](xref:Xamarin.Forms.Image) -Instanz animiert, indem Sie horizontal und vertikal über eine Sekunde (1000 Millisekunden) übersetzt wird. [ `TranslateTo` ] (Xref: Xamarin.Forms . Viewextensions. translateto ( Xamarin.Forms . Visualelement, System. Double, System. Double, System. UInt32, Xamarin.Forms . Beschleunigungs)) die Methode übersetzt gleichzeitig das Bild 100 Pixel nach links und 100 Pixel nach oben. Dies liegt daran, dass das erste und das zweite Argument negative Zahlen sind. Durch die Bereitstellung positiver Zahlen wird das Bild nach rechts und nach unten übersetzt.

Die folgenden Screenshots zeigen die Übersetzung, die auf jeder Plattform durchgeführt wird:

![Übersetzungs Animation](simple-images/translateto.png)

> [!NOTE]
> Wenn ein Element anfänglich aus dem Bildschirm entfernt und dann auf den Bildschirm übersetzt wird, bleibt das Eingabe Layout des Elements nach der Übersetzung deaktiviert, sodass der Benutzer nicht mit dem Bildschirm interagieren kann. Daher empfiehlt es sich, eine Ansicht an der endgültigen Position und dann alle erforderlichen Übersetzungen zu überprüfen.

### <a name="fading"></a>Ausblenden

Im folgenden Codebeispiel wird die Verwendung von [ `FadeTo` ] (Xref:) veranschaulicht Xamarin.Forms . Viewextensions. Fade to ( Xamarin.Forms . Visualelement, System. Double, System. UInt32, Xamarin.Forms . Beschleunigung)) Methode zum Animieren der [`Opacity`](xref:Xamarin.Forms.VisualElement.Opacity) Eigenschaft eines [`Image`](xref:Xamarin.Forms.Image) :

```csharp
image.Opacity = 0;
await image.FadeTo (1, 4000);
```

Mit diesem Code wird die-Instanz animiert, [`Image`](xref:Xamarin.Forms.Image) indem Sie in mehr als 4 Sekunden (4000 Millisekunden) ausgeblendet wird. [ `FadeTo` ] (Xref: Xamarin.Forms . Viewextensions. Fade to ( Xamarin.Forms . Visualelement, System. Double, System. UInt32, Xamarin.Forms . Beschleunigung)) die Methode ruft den aktuellen [`Opacity`](xref:Xamarin.Forms.VisualElement.Opacity) Eigenschafts Wert für den Beginn der Animation ab und wird dann von diesem Wert auf das erste Argument (1).

Die folgenden Screenshots zeigen, wie die einzelnen Plattformen ausgeblendet werden:

![Ausblenden der Animation](simple-images/fadeto.png)

## <a name="compound-animations"></a>Verbund Animationen

Eine Verbund Animation ist eine sequenzielle Kombination von Animationen und kann mit dem- `await` Operator erstellt werden, wie im folgenden Codebeispiel gezeigt:

```csharp
await image.TranslateTo (-100, 0, 1000);    // Move image left
await image.TranslateTo (-100, -100, 1000); // Move image up
await image.TranslateTo (100, 100, 2000);   // Move image diagonally down and right
await image.TranslateTo (0, 100, 1000);     // Move image left
await image.TranslateTo (0, 0, 1000);       // Move image up
```

In diesem Beispiel wird der [`Image`](xref:Xamarin.Forms.Image) über 6 Sekunden (6000 Millisekunden) übersetzt. Bei der Übersetzung von `Image` werden fünf Animationen verwendet, wobei der `await` Operator angibt, dass jede Animation sequenziell ausgeführt wird. Folglich werden nachfolgende Animations Methoden ausgeführt, nachdem die vorherige Methode abgeschlossen wurde.

## <a name="composite-animations"></a>Zusammengesetzte Animationen

Eine zusammengesetzte Animation ist eine Kombination aus Animationen, bei denen zwei oder mehr Animationen gleichzeitig ausgeführt werden. Zusammengesetzte Animationen können erstellt werden, indem erwartete und nicht erwartete Animationen gemischt werden, wie im folgenden Codebeispiel gezeigt:

```csharp
image.RotateTo (360, 4000);
await image.ScaleTo (2, 2000);
await image.ScaleTo (1, 2000);
```

In diesem Beispiel wird der [`Image`](xref:Xamarin.Forms.Image) skaliert und gleichzeitig über 4 Sekunden gedreht (4000 Millisekunden). Die Skalierung des `Image` verwendet zwei sequenzielle Animationen, die zur gleichen Zeit wie die Drehung auftreten. [ `RotateTo` ] (Xref: Xamarin.Forms . Viewextensions. rotateTo ( Xamarin.Forms . Visualelement, System. Double, System. UInt32, Xamarin.Forms . Beschleunigung)) die Methode wird ohne `await` Operator ausgeführt und wird sofort zurückgegeben, wobei die erste [`ScaleTo`](xref:Xamarin.Forms.ViewExtensions.ScaleTo*) Animation beginnt. Der- `await` Operator für den ersten `ScaleTo` Methodenaufrufe verzögert den zweiten `ScaleTo` Methoden Aufrufvorgang, bis der erste `ScaleTo` Methoden Aufrufvorgang abgeschlossen ist. An diesem Punkt `RotateTo` ist die Animation halb Weise abgeschlossen, und die `Image` wird um 180 Grad gedreht. Während der letzten 2 Sekunden (2000 Millisekunden) werden die zweite `ScaleTo` Animation und die `RotateTo` Animation abgeschlossen.

### <a name="running-multiple-asynchronous-methods-concurrently"></a>Gleichzeitiges Ausführen mehrerer asynchroner Methoden

Die `static` `Task.WhenAny` -Methode und die- `Task.WhenAll` Methode werden zum gleichzeitigen Ausführen mehrerer asynchroner Methoden verwendet und können daher zum Erstellen von zusammengesetzten Animationen verwendet werden. Beide Methoden geben ein `Task` -Objekt zurück und akzeptieren eine Auflistung von Methoden, die jeweils ein-Objekt zurückgeben `Task` . Die- `Task.WhenAny` Methode wird abgeschlossen, wenn die Ausführung einer Methode in der-Auflistung abgeschlossen ist, wie im folgenden Codebeispiel gezeigt:

```csharp
await Task.WhenAny<bool>
(
  image.RotateTo (360, 4000),
  image.ScaleTo (2, 2000)
);
await image.ScaleTo (1, 2000);
```

In diesem Beispiel enthält der `Task.WhenAny` Methodenaufrufe zwei Aufgaben. Der erste Task dreht das Bild über 4 Sekunden (4000 Millisekunden), während die zweite Aufgabe das Bild über 2 Sekunden (2000 Millisekunden) skaliert. Wenn die zweite Aufgabe abgeschlossen ist, wird der `Task.WhenAny` Methoden Aufrufvorgang abgeschlossen. Obwohl [ `RotateTo` ] (Xref: Xamarin.Forms . Viewextensions. rotateTo ( Xamarin.Forms . Visualelement, System. Double, System. UInt32, Xamarin.Forms . Beschleunigung)) die Methode wird noch ausgeführt, die zweite [`ScaleTo`](xref:Xamarin.Forms.ViewExtensions.ScaleTo*) Methode kann beginnen.

Die- `Task.WhenAll` Methode wird abgeschlossen, wenn alle Methoden in der-Auflistung abgeschlossen wurden, wie im folgenden Codebeispiel gezeigt:

```csharp
// 10 minute animation
uint duration = 10 * 60 * 1000;

await Task.WhenAll (
  image.RotateTo (307 * 360, duration),
  image.RotateXTo (251 * 360, duration),
  image.RotateYTo (199 * 360, duration)
);
```

In diesem Beispiel enthält der `Task.WhenAll` Methodenaufrufe drei Aufgaben, die jeweils über 10 Minuten ausgeführt werden. Jede `Task` gibt eine andere Anzahl von 360-Grad-Drehungen – 307 Drehungen für [ `RotateTo` ] (Xref:) aus Xamarin.Forms . Viewextensions. rotateTo ( Xamarin.Forms . Visualelement, System. Double, System. UInt32, Xamarin.Forms . Beschleunigung)), 251 Drehungen für [ `RotateXTo` ] (Xref: Xamarin.Forms . Viewextensions. rotatexto ( Xamarin.Forms . Visualelement, System. Double, System. UInt32, Xamarin.Forms . Beschleunigung)) und 199 Drehungen für [ `RotateYTo` ] (Xref: Xamarin.Forms . Viewextensions. rotateyto ( Xamarin.Forms . Visualelement, System. Double, System. UInt32, Xamarin.Forms . Beschleunigung)). Bei diesen Werten handelt es sich um Primzahlen. Dadurch wird sichergestellt, dass die Rotationen nicht synchronisiert werden und daher nicht wiederholende Muster

Die folgenden Screenshots zeigen, wie viele Drehungen auf den einzelnen Plattformen ausgeführt werden:

![Zusammengesetzte Animation](simple-images/multiple-rotations.png)

## <a name="canceling-animations"></a>Abbrechen von Animationen

Eine Anwendung kann eine oder mehrere Animationen durch einen []-Aufrufvorgang Abbrechen `static` `ViewExtensions.CancelAnimations` (Xref:) Xamarin.Forms . Viewextensions. cancelanimationen ( Xamarin.Forms . Visualelement))-Methode, wie im folgenden Codebeispiel gezeigt:

```csharp
ViewExtensions.CancelAnimations (image);
```

Dadurch werden sofort alle Animationen abgebrochen, die derzeit auf der-Instanz ausgeführt werden [`Image`](xref:Xamarin.Forms.Image) .

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde das Erstellen und Abbrechen von Animationen mit der- [`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions) Klasse veranschaulicht. Diese Klasse stellt Erweiterungs Methoden bereit, die verwendet werden können, um einfache Animationen zu erstellen, mit denen Instanzen gedreht, skaliert, übersetzt und ausgeblendet werden [`VisualElement`](xref:Xamarin.Forms.VisualElement) .

## <a name="related-links"></a>Verwandte Links

- [Async Support Overview (Übersicht über die asynchrone Unterstützung)](~/cross-platform/platform/async.md)
- [Grundlegende Animation (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-animation-basic)
- [Viewextensions](xref:Xamarin.Forms.ViewExtensions)
