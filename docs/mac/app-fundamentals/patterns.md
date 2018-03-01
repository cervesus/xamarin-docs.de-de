---
title: "Allgemeine Muster und Ausdrücke"
description: Dieses Dokument beschreibt das Model View Controller-Muster, Quelle und des Delegats Datenmuster und Protokolle.
ms.topic: article
ms.prod: xamarin
ms.assetid: BF0A3517-17D8-453D-87F7-C8A34BEA8FF5
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/17/2016
ms.openlocfilehash: 4926670a0cd0960c9a390bd388d3530929634153
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="common-patterns-and-idioms"></a>Allgemeine Muster und Ausdrücke

In der gesamten die Apple-APIs über c# verfügbar gemacht sind bestimmte Idiome und Muster immer wieder. Wenn Sie Erfahrung mit der Programmierung mit Xamarin.iOS haben, können diese bekannt vorkommen. Dokumentation wird häufig auf diese Muster und Idiome wiederholt, verwiesen, damit über ein gutes Verständnis davon können Sie der Dokumentation sinnvoll sein, das sich befindet.

## <a name="mvc---model-view-controller"></a>MVC - Model View Controller

Model View Controller oder kurz, MVC ist ein sehr übliches Muster in der gesamten Kakao gefunden. Eine detaillierte Erläuterung ist nicht Gegenstand dieses Dokuments allerdings in Kürze handelt es sich um eine Möglichkeit zum Strukturieren Ihrer Anwendung in Komponenten:

- **Modell** Objekte darstellen zugrunde liegenden Daten, angezeigt und bearbeitet (z. B. Adressen in einem Adressbuch)
- **Ansicht** Objekte behandeln das Zeichnen eines angegebenen Objekts auf dem Bildschirm und Verarbeiten von Benutzerinteraktionen (ein Textfeld mit der Adresse auf dem Bildschirm)
- **Controller** Objekte behandeln die Interaktion zwischen Modell und anzeigen. Drücken Sie modelländerungen "bis", um die Ansicht aktualisieren, und "nach unten" Änderungen in der Ansicht verschoben wird, wenn Benutzer in der Benutzeroberfläche ändern.

Wenn Sie von anderen Bibliotheken wie WPF mit MVVM (Model-View-ViewModel) vertraut sind, wird der Controller fungiert ViewModel ähnelt jedoch häufig genauer an bestimmte Elemente der Benutzeroberfläche gebunden ist.

Weitere Informationen finden Sie hier:

- [Erlernen von MVC auf der Website von Apple](https://developer.apple.com/library/ios/documentation/general/conceptual/devpedia-cocoacore/MVC.html)

- [Model View Controller in Objective-C](https://developer.apple.com/library/ios/documentation/general/conceptual/CocoaEncyclopedia/Model-View-Controller/Model-View-Controller.html)
- [Arbeiten mit Fenstern](~/mac/user-interface/window.md)

## <a name="data-source--delegate--subclassing"></a>Datenquelle / delegieren / Unterklasse erstellen

Eine andere sehr übliches Muster in Kakao befasst sich mit dem Bereitstellen von Daten für die Elemente der Benutzeroberfläche und das Reagieren auf Benutzerinteraktionen. Mithilfe von `NSTableView` beispielsweise müssen Sie die Daten für jede Zeile aus irgendeinem Grund angeben, einige von UI-Elemente darstellen, die, einige Zeile, festgelegt Satz von Verhaltensweisen, auf Benutzerinteraktionen und möglicherweise bestimmte Werte in der Anpassung zu reagieren. Quelle und des Delegats Datenmuster Funktionen können Sie den meisten Fällen zu verarbeiten, ohne Erstellung von Unterklassen von verwenden `NSTableView` selbst.

- Die `DataSource` Eigenschaft ist eine Instanz einer benutzerdefinierten Unterklasse zugewiesen `NSTableViewDataSource` die aufgerufen wird, um die Tabelle mit Daten füllen (über `GetRowCount` und `GetObjectValue`).

- Die `Delegate` Eigenschaft ist eine Instanz einer benutzerdefinierten Unterklasse zugewiesen `NSTableViewDelegate` bietet die Ansicht für ein Objekt des angegebenen Modell (über `GetViewForItem`) und verarbeitet die UI-Interaktion (über `DidClickTableColumn`, `MouseDownInHeaderOfTableColumn`usw.).

In einigen Fällen sollten Sie zum Anpassen des Steuerelements auf eine Weise hinter die Hooks in das Delegaten oder die Datenquelle angegeben und können Sie direkt Unterklasse der Ansicht. Achten Sie jedoch, in vielen Fällen Überschreiben der Standardeinstellung Verhalten dann erfordert, dass Sie zur Behandlung aller von diesem Verhalten selbst (das Verhalten der Funktionsauswahl anpassen müssen Sie möglicherweise alle der Auswahlverhalten selbst implementieren).

In Xamarin.iOS, einige APIs wie z. B. `UITableView` haben, wurde durch eine Eigenschaft, die der Delegat und die Datenquelle implementiert erweitert (`UITableViewSource`). Dies erfolgt auf die allgemeine Beschränkung umgehen, die eine einzelne C#-Klasse nur eine Basisklasse und unsere Einbringen der Protokolle kann über Basisklassen.

Weitere Informationen zum Arbeiten mit Tabellensichten in einer Anwendung Xamarin.Mac finden Sie unter unsere [Tabellenansicht](~/mac/user-interface/table-view.md) Dokumentation.

## <a name="protocols"></a>Protokolle

Protokolle in Objective-C-Schnittstelle in c# verglichen werden können und in vielen Fällen werden in ähnlichen Situationen verwendet. Zum Beispiel die `NSTableView` Beispiel oben, Delegaten und der Datenquelle sind tatsächlich Protokolle. Xamarin.Mac macht diese als Basis-Klassen mit virtuellen Methoden, die Sie überschreiben können. Der Hauptunterschied zwischen C#-Schnittstellen und Protokolle für Objective-C ist, dass einige Methoden in ein Protokoll optional implementieren können. Sie müssen Zugriff auf die Dokumentation und/oder die Definition einer API, um zu bestimmen, was optional ist.

Weitere Informationen finden Sie in unserem [Delegaten, Protokolle und Ereignisse](~/ios/app-fundamentals/delegates-protocols-and-events.md) Dokumentation.



## <a name="related-links"></a>Verwandte Links

- [Tabellenansichten](~/mac/user-interface/table-view.md)
- [Arbeiten mit Fenstern](~/mac/user-interface/window.md)
- [Delegaten, Protokolle und Ereignisse](~/ios/app-fundamentals/delegates-protocols-and-events.md)
- [Model-View-Controller](https://developer.apple.com/library/ios/documentation/general/conceptual/CocoaEncyclopedia/Model-View-Controller/Model-View-Controller.html)
