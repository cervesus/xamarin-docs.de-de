---
title: MacOS-APIs für Xamarin.Mac-Entwickler
description: In diesem Dokument wird beschrieben, wie Objective-C-Selektoren gelesen und wie Sie die entsprechenden C#-Methoden in einer Xamarin.Mac-app zu finden.
ms.prod: xamarin
ms.assetid: 9F7451FA-E07E-4C7B-B5CF-27AFC157ECDA
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/02/2017
ms.openlocfilehash: 6dfaa3c7bf988228bfbacefe7c8e7268edc8117a
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994311"
---
# <a name="macos-apis-for-xamarinmac-developers"></a>MacOS-APIs für Xamarin.Mac-Entwickler

## <a name="overview"></a>Übersicht

Für einen Großteil Ihrer Zeit für die Entwicklung mit Xamarin.Mac können Sie denken, lesen und ohne viel Beachtung mit den zugrunde liegenden Objective-C-APIs in c# schreiben. Jedoch werden manchmal müssen, lesen die API-Dokumentation von Apple, eine Antwort von Stack Overflow zu einer Lösung für Ihr Problem zu übersetzen, oder vergleichen, um ein Beispiel für eine vorhandene.

## <a name="reading-enough-objective-c-to-be-dangerous"></a>Lesen Sie genügend Objective-C gefährlich sein

Manchmal wird es auf eine Objective-C-Definition lesen erforderlich sein, oder Methodenaufruf, und übersetzt, die an die entsprechende C#-Methode. Wir sehen Sie sich die Definition einer Objective-C-Funktion, und die einzelnen Teile unterteilen. Diese Methode (eine *Selektor* in Objective-C) befinden sich `NSTableView`:

```objc
- (BOOL)canDragRowsWithIndexes:(NSIndexSet *)rowIndexes atPoint:(NSPoint)mouseDownPoint
```

Die Deklaration kann links nach rechts gelesen werden:

- Die `-` Präfix bedeutet, dass sie eine Instanzmethode (nicht statisch). + bedeutet, dass es sich um eine Klassenmethode (statisch) ist
- `(BOOL)` ist der Rückgabetyp (Bool in c#)
- `canDragRowsWithIndexes` ist der erste Teil des Namens an.
- `(NSIndexSet *)rowIndexes` der erste Parameter ist, und mit ihm vom Typ. Der erste Parameter ist im Format: `(Type) pararmName`
- `atPoint:(NSPoint)mouseDownPoint` ist der zweite Parameter und seinen Typ. Jeder Parameter nach dem ersten ist das Format: `selectorPart:(Type) pararmName`
- Der vollständige Name der dieser Nachrichtenselektor: `canDragRowsWithIndexes:atPoint:`. Beachten Sie die `:` am Ende – es ist wichtig.
- Die tatsächliche Xamarin.Mac C#-Bindung ist: `bool CanDragRows (NSIndexSet rowIndexes, PointF mouseDownPoint)`

Dieser Aufruf Selektor kann die gleiche Weise gelesen werden:

```objc
[v canDragRowsWithIndexes:set atPoint:point];
```

- Die Instanz `v` hat seine `canDragRowsWithIndexes:atPoint` Auswahl, die mit zwei Parametern aufgerufen `set` und `point`, übernommen.
- In c# sieht der Methodenaufruf folgendermaßen aus: `x.CanDragRows (set, point);`

<a name="finding_selector" />

## <a name="finding-the-c-member-for-a-given-selector"></a>Suchen das C#-Element für einen angegebenen Selektor

Nun, dass Sie die Objective-C-Auswahl, die Sie aufrufen möchten gefunden haben, ist der nächste Schritt, die den entsprechenden C#-Member zugeordnet. Es gibt vier Möglichkeiten, Sie versuchen können, (anhand der `NSTableView CanDragRows` Beispiel):

1. Verwenden Sie die Vervollständigungsliste automatisch, um schnell nach etwas mit dem gleichen Namen zu suchen. Da wir wissen, ist eine Instanz der `NSTableView` können Sie eingeben:

    - `NSTableView x;`
    - `x.` [STRG + LEERTASTE, wenn die Liste nicht angezeigt wird).
    - `CanDrag` [EINGABETASTE]
    - Mit der rechten Maustaste in der Methode, Gehe zu Deklaration zu der Assembly-Browser öffnen, in denen Sie vergleichen können, die `Export` -Attribut auf die betreffende-Auswahl

2. Suchen Sie die gesamte Klasse-Bindung. Da wir wissen, ist eine Instanz der `NSTableView` können Sie eingeben:

    - `NSTableView x;`
    - Mit der rechten Maustaste `NSTableView`, Gehe zu Deklaration an Assembly-Browser
    - Suchen Sie nach der betreffenden Auswahl

3. Sie können die [Xamarin.Mac-API-Onlinedokumentation](https://docs.microsoft.com/dotnet/api/?view=xamarinmac-3.0) .

4. Miguel bietet einen Überblick über "Rosettastein" die Xamarin.Mac-APIs [hier](http://tirania.org/tmp/rosetta.html) , die Sie für eine bestimmte API durchsuchen können. Wenn Ihre API nicht AppKit oder MacOS-spezifische ist, es gibt es möglicherweise.

<!--
Note: In some cases, the assembly browser can hit a bug where it will open but not jump to the right definition. Keep that tab open, switch back to your source code and try again.
Note: The assembly browser tricks currently only works with Xamarin.Mac Classic. This will be fixed in a future version.
-->
