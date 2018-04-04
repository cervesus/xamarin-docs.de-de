---
title: Mac-APIs
description: Dieses Dokument beschreibt, wie Objective-C-Selektoren gelesen und wie Sie die entsprechenden C#-Methoden zu finden.
ms.prod: xamarin
ms.assetid: 9F7451FA-E07E-4C7B-B5CF-27AFC157ECDA
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/02/2017
ms.openlocfilehash: 0344fecb9a8d64a680bb11689f56cf074d952f4e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="mac-apis"></a>Mac-APIs

## <a name="overview"></a>Übersicht

Für einen Großteil der Arbeitszeit mit Xamarin.Mac entwickeln können Sie denken, lesen und in c# schreiben, ohne dabei die zugrunde liegenden Objective-C-APIs. Allerdings müssen manchmal Sie zum Lesen der API-Dokumentation von Apple, übersetzt eine Antwort von Stack Overflow zu einer Lösung für Ihr Problem, oder im Vergleich mit einer vorhandenen Beispiel.

## <a name="reading-enough-objective-c-to-be-dangerous"></a>Lesen Sie genügend Objective-C um gefährlich sein

In einigen Fällen wird es zum Lesen der Definition einer Objective-C erforderlich sein, oder -Methodenaufruf, und übersetzen, die an die entsprechende C#-Methode. Wir sehen Sie sich die Definition einer Objective-C-Funktion und die einzelnen Teile aufgliedern. Diese Methode (eine *Selektor* in Objective-C) finden Sie in der `NSTableView`:

```objc
- (BOOL)canDragRowsWithIndexes:(NSIndexSet *)rowIndexes atPoint:(NSPoint)mouseDownPoint
```

Die Deklaration kann links nach rechts gelesen werden:

- Die `-` Präfix bedeutet, dass es eine Instanzmethode (nicht statische). + bedeutet, dass es sich um eine Klassenmethode (statisch) ist
- `(BOOL)` ist der Rückgabetyp (Bool in c#)
- `canDragRowsWithIndexes` ist der erste Teil des Namens an.
- `(NSIndexSet *)rowIndexes` der erste Parameter ist und mit ihm vom Typ. Der erste Parameter ist im Format: `(Type) pararmName`
- `atPoint:(NSPoint)mouseDownPoint` ist der zweite Parameter und dessen Typ. Jeder Parameter nach dem ersten ist das Format: `selectorPart:(Type) pararmName`
- Der vollständige Name der dieser Nachrichtenselektor: `canDragRowsWithIndexes:atPoint:`. Beachten Sie die `:` am Ende - es ist wichtig.
- Die tatsächliche Xamarin.Mac C#-Bindung ist: `bool CanDragRows (NSIndexSet rowIndexes, PointF mouseDownPoint)`

Dieser Aufruf Selektor kann die gleiche Weise gelesen werden:

```objc
[v canDragRowsWithIndexes:set atPoint:point];
```

- Die Instanz `v` hat seine `canDragRowsWithIndexes:atPoint` Selektor mit zwei Parametern aufgerufen `set` und `point`, übergeben.
- In c# sieht der Methodenaufruf: `x.CanDragRows (set, point);`

<a name="finding_selector" />

## <a name="finding-the-c-member-for-a-given-selector"></a>Suchen das C#-Element für einen angegebenen Selektor

Nun, dass Sie die Objective-C-Auswahl, die Sie aufrufen möchten gefunden haben, ist der nächste Schritt, die auf den entsprechenden C#-Member zuordnen. Es gibt vier Möglichkeiten Sie versuchen können, (Fortsetzen der `NSTableView CanDragRows` Beispiel):

1. Verwenden Sie die Vervollständigungsliste automatisch, um schnell für ein Element mit demselben Namen zu durchsuchen. Da wir, es wissen ist eine Instanz der `NSTableView` können Sie eingeben:

    - `NSTableView x;`
    - `x.` [STRG + LEERTASTE, wenn die Liste nicht angezeigt wird).
    - `CanDrag` [enter]
    - Mit der rechten Maustaste in der Methodennamens, Gehe zu Deklaration zu der Assembly-Browser öffnen, in dem Sie vergleichen können, die `Export` -Attribut auf die betreffende-Auswahl

2. Suchen Sie die Bindung für die gesamte Klasse. Da wir, es wissen ist eine Instanz der `NSTableView` können Sie eingeben:

    - `NSTableView x;`
    - Mit der rechten Maustaste `NSTableView`, Gehe zu Deklaration an Assembly-Browser
    - Suchen Sie nach dem Selektor fraglichen

3. Sie können die [Xamarin.Mac-API-Onlinedokumentation](https://developer.xamarin.com/api/root/monomac-lib/) .

4. Miguel bietet einen Überblick über "Rosettastein" Xamarin.Mac-APIs [hier](http://tirania.org/tmp/rosetta.html) , die Sie für eine bestimmte API durchsuchen können. Ihre API nicht AppKit oder MacOS bestimmte ist, kann es es gefunden werden.

<!--
Note: In some cases, the assembly browser can hit a bug where it will open but not jump to the right definition. Keep that tab open, switch back to your source code and try again.
Note: The assembly browser tricks currently only works with Xamarin.Mac Classic. This will be fixed in a future version.
-->
