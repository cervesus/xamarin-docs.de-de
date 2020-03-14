---
title: macOS-APIs für xamarin. Mac-Entwickler
description: In diesem Dokument wird beschrieben, wie Sie die Ziel-C-Selektoren lesen und C# die entsprechenden Methoden in einer xamarin. Mac-app finden.
ms.prod: xamarin
ms.assetid: 9F7451FA-E07E-4C7B-B5CF-27AFC157ECDA
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 03/02/2017
ms.openlocfilehash: cd427d13bb79fd31e1e814726aaaf61788ae10ec
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/13/2020
ms.locfileid: "79306272"
---
# <a name="macos-apis-for-xamarinmac-developers"></a>macOS-APIs für xamarin. Mac-Entwickler

## <a name="overview"></a>Übersicht

Für einen Großteil ihrer Zeit mit xamarin. Mac können Sie sich mit den zugrunde liegenden Ziel-C- C# APIs in Gedanken machen, lesen und schreiben. Manchmal müssen Sie jedoch die API-Dokumentation von Apple lesen, eine Antwort von Stack Overflow auf eine Lösung für Ihr Problem übersetzen oder eine vorhandene Stichprobe vergleichen.

## <a name="reading-enough-objective-c-to-be-dangerous"></a>Lesen von ausreichendem Ziel-C als gefährlich

In manchen Fällen ist es erforderlich, eine Definition oder einen Methodenaufrufe von "Ziel C" zu lesen und C# diese in die entsprechende Methode zu übersetzen. Werfen wir einen Blick auf die Definition einer Ziel-C-Funktion und unterteilen die Teile. Diese Methode (eine *Auswahl* in Ziel-C) befindet sich auf `NSTableView`:

```objc
- (BOOL)canDragRowsWithIndexes:(NSIndexSet *)rowIndexes atPoint:(NSPoint)mouseDownPoint
```

Die Deklaration kann von links nach rechts gelesen werden:

- Das `-` Präfix bedeutet, dass es sich um eine Instanzmethode (nicht statisch) handelt. + bedeutet, dass es sich um eine Klassenmethode (statisch) handelt.
- `(BOOL)` ist der Rückgabetyp (bool in C#).
- `canDragRowsWithIndexes` ist der erste Teil des Namens.
- `(NSIndexSet *)rowIndexes` ist der erste Parameter und mit dem Typ. Der erste Parameter weist das folgende Format auf: `(Type) pararmName`
- `atPoint:(NSPoint)mouseDownPoint` ist der zweite Parameter und sein Typ. Jeder Parameter nach dem ersten ist das folgende Format: `selectorPart:(Type) pararmName`
- Der vollständige Name dieses Nachrichtenselektor lautet: `canDragRowsWithIndexes:atPoint:`. Beachten Sie den `:` am Ende, was wichtig ist.
- Die tatsächliche xamarin. Mac C# -Bindung lautet: `bool CanDragRows (NSIndexSet rowIndexes, PointF mouseDownPoint)`

Dieser Auswahl Aufruf kann auf die gleiche Weise gelesen werden:

```objc
[v canDragRowsWithIndexes:set atPoint:point];
```

- Bei der Instanz `v` wird die `canDragRowsWithIndexes:atPoint` Auswahl mit zwei Parametern aufgerufen, `set` und `point`.
- In C#sieht der Methodenaufruf wie folgt aus: `x.CanDragRows (set, point);`

<a name="finding_selector" />

## <a name="finding-the-c-member-for-a-given-selector"></a>Suchen des C# Members für eine bestimmte Auswahl

Nachdem Sie den Ziel-C-Selektor gefunden haben, den Sie aufrufen müssen, ist der nächste Schritt das Mapping des C# entsprechenden Elements. Es gibt vier Ansätze, die Sie ausprobieren können (Fortfahren mit dem `NSTableView CanDragRows` Beispiel):

1. Verwenden Sie die Liste automatische Vervollständigung, um schnell nach einem beliebigen Namen zu suchen. Da wir wissen, dass es sich um eine Instanz von handelt `NSTableView` können Sie Folgendes eingeben:

    - `NSTableView x;`
    - `x.` [STRG + LEERTASTE, wenn die Liste nicht angezeigt wird).
    - `CanDrag` [Eingabe]
    - Klicken Sie mit der rechten Maustaste auf die Methode, und wechseln Sie zu Deklaration, um den assemblybrowser zu öffnen, in dem Sie das `Export` Attribut mit dem fraglichen

2. Durchsuchen Sie die gesamte Klassen Bindung. Da wir wissen, dass es sich um eine Instanz von handelt `NSTableView` können Sie Folgendes eingeben:

    - `NSTableView x;`
    - Klicken Sie mit der rechten Maustaste auf `NSTableView`, gehen Sie zu Deklaration in Assembly
    - Nach dem fraglichen Selektor suchen

3. Sie können die [xamarin. Mac-API Online-Dokumentation](https://docs.microsoft.com/dotnet/api/?view=xamarinmac-3.0) verwenden.

4. Miguel stellt [hier](https://tirania.org/tmp/rosetta.html) eine "Rosetta Stone"-Ansicht der xamarin. Mac-APIs bereit, die Sie für eine bestimmte API durchsuchen können. Wenn Ihre API nicht AppKit oder macOS-spezifisch ist, finden Sie es möglicherweise dort.

<!--
Note: In some cases, the assembly browser can hit a bug where it will open but not jump to the right definition. Keep that tab open, switch back to your source code and try again.
Note: The assembly browser tricks currently only works with Xamarin.Mac Classic. This will be fixed in a future version.
-->
