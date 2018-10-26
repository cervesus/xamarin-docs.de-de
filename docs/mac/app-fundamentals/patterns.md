---
title: Allgemeine Muster und Idiome in Xamarin.Mac
description: Dieses Dokument beschreibt gängige Entwurfsmuster beim Erstellen von Xamarin.Mac-apps verwendet. Es wird erläutert, das Model-View-Controller-Muster, das Datenmuster Quell- und Delegaten und Protokolle.
ms.prod: xamarin
ms.assetid: BF0A3517-17D8-453D-87F7-C8A34BEA8FF5
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 06/17/2016
ms.openlocfilehash: b4266582ce0cec522e207cd06987f2d0eeff8b2d
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50104335"
---
# <a name="common-patterns-and-idioms-in-xamarinmac"></a>Allgemeine Muster und Idiome in Xamarin.Mac

In der Apple-APIs über C# -Code verfügbar gemacht wird aktiviert bestimmte Ausdrücke und Muster immer wieder. Wenn Sie keine Erfahrung mit Programmierung mit Xamarin.iOS verfügen, können dies vertraut sein. Dokumentation verweist oft diese Muster und Idiome wiederholt wird, auf daher müssen solide Kenntnisse über diese können Sie der Dokumentation sinnvoll sein, die Sie zu finden.

## <a name="mvc---model-view-controller"></a>MVC - Model-View-Controller

Model View Controller oder MVC kurz, ist ein häufig verwendetes Muster, die in der gesamten Cocoa. Eine ausführliche Erörterung sprengen den Rahmen dieses Dokuments, aber kurz gesagt ist es eine Möglichkeit zum Strukturieren Ihrer Anwendung in Komponenten:

- **Modell** Objekte angezeigt und bearbeitet (z. B. Adressen in einem Adressbuch) darstellen die zugrunde liegenden Daten,
- **Ansicht** Objekte zu verarbeiten, die Zeichnung eines bestimmten Objekts auf dem Bildschirm und Verarbeiten von Benutzerinteraktionen (ein Textfeld mit der Adresse auf dem Bildschirm)
- **Controller** Objekte verarbeiten, die Interaktion zwischen Modell und anzeigen. Diese Änderungen des Datenmodells push "oben" zum Aktualisieren der Ansicht "" und "down" Änderungen aus der Sicht übertragen wird, wenn Benutzer in der Benutzeroberfläche ändern.

Wenn Sie mit MVVM (Model-View-ViewModel) von anderen Bibliotheken wie WPF vertraut sind, wird der Controller verhält sich ähnlich wie das "ViewModel", aber häufig enger an die spezifischen Elemente der Benutzeroberfläche gebunden ist.

Weitere Informationen finden Sie hier:

- [Mehr über MVC auf der Website von Apple](https://developer.apple.com/library/ios/documentation/general/conceptual/devpedia-cocoacore/MVC.html)

- [Model View Controller in Objective-C](https://developer.apple.com/library/ios/documentation/general/conceptual/CocoaEncyclopedia/Model-View-Controller/Model-View-Controller.html)
- [Arbeiten mit Windows](~/mac/user-interface/window.md)

## <a name="data-source--delegate--subclassing"></a>Datenquelle / delegieren / Erstellen von Unterklassen für

Ein weiteres sehr häufiges Muster in Cocoa befasst sich mit Daten an Benutzeroberflächenelemente bereitstellt und die Reaktion auf Benutzerinteraktionen. Mithilfe von `NSTableView` beispielsweise müssen Sie die Daten für jede Zeile bieten, einige von UI-Elementen, die darstellen, die, einige Zeile, festgelegt Satz von Verhaltensweisen, die auf Interaktionen der Benutzer, und möglicherweise gewisse Anpassungen zu reagieren. Die Quell- und Delegaten Muster von Daten können Sie die meisten Fälle verarbeiten, ohne auf den Unterklassen zurückgreifen `NSTableView` selbst.

- Die `DataSource` -Eigenschaft zugewiesen wird eine Instanz einer benutzerdefinierten Unterklasse von `NSTableViewDataSource` die zum Auffüllen der Tabelle mit Daten aufgerufen wird (über `GetRowCount` und `GetObjectValue`).

- Die `Delegate` -Eigenschaft zugewiesen wird eine Instanz einer benutzerdefinierten Unterklasse von `NSTableViewDelegate` die Ansicht für ein Objekt des angegebenen Modellelements bereitstellt (über `GetViewForItem`) und verarbeitet die UI-Interaktion (über `DidClickTableColumn`, `MouseDownInHeaderOfTableColumn`usw.).

Klicken Sie in einigen Fällen wird ein Steuerelement auf eine Weise über die Hooks in das Delegaten oder die Datenquelle angegebenen anpassen möchten, und Sie können direkt Unterklasse der Ansicht. Achten Sie jedoch, die in vielen Fällen überschreiben Verhalten dann erfordert, dass Sie alle dieses Verhalten wird automatisch ausgeführt (Anpassen des Auswahlverhaltens müssen Sie alle von der Auswahlverhalten selbst implementieren).

In Xamarin.iOS, einige APIs, wie z. B. `UITableView` erweitert wurden mit einer Eigenschaft, die der Delegat und der Datenquelle implementiert (`UITableViewSource`). Dies, die allgemeine Einschränkung zu umgehen, die eine einzelne C# Klasse kann nur von einer Basisklasse verfügen, und unsere Anzeigen der Protokolle erfolgt über Basis-Klassen.

Weitere Informationen zum Arbeiten mit Tabellenansichten in einer Xamarin.Mac-Anwendung finden Sie unserem [Tabellenansicht](~/mac/user-interface/table-view.md) Dokumentation.

## <a name="protocols"></a>Protokolle

Protokolle in Objective-C-Schnittstellen in verglichen werden können C#, und in vielen Fällen in ähnlichen Situationen verwendet werden. Zum Beispiel die `NSTableView` Beispiel oben wird der Delegat und der Datenquelle sind tatsächlich Protokolle. Xamarin.Mac macht diese als Basis-Klassen mit virtuellen Methoden, die Sie überschreiben können. Der Hauptunterschied zwischen C# Schnittstellen und Protokolle für Objective-C ist, dass einige Methoden in einem Protokoll optional implementiert werden können. Sie müssen sehen Sie sich die Dokumentation und/oder die Definition einer API, um zu bestimmen, was optional ist.

Weitere Informationen finden Sie unserem [Delegaten, Protokolle und Ereignisse](~/ios/app-fundamentals/delegates-protocols-and-events.md) Dokumentation.



## <a name="related-links"></a>Verwandte Links

- [Tabellenansichten](~/mac/user-interface/table-view.md)
- [Arbeiten mit windows](~/mac/user-interface/window.md)
- [Delegaten, Protokolle und Ereignisse](~/ios/app-fundamentals/delegates-protocols-and-events.md)
- [Model-View-Controller](https://developer.apple.com/library/ios/documentation/general/conceptual/CocoaEncyclopedia/Model-View-Controller/Model-View-Controller.html)
