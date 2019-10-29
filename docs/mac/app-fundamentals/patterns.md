---
title: Allgemeine Muster und Idiome in xamarin. Mac
description: In diesem Dokument werden allgemeine Entwurfsmuster beschrieben, die beim Erstellen von xamarin. Mac-Apps verwendet werden. Es erläutert das Model-View-Controller-Muster, die Datenquelle und die delegatmuster sowie Protokolle.
ms.prod: xamarin
ms.assetid: BF0A3517-17D8-453D-87F7-C8A34BEA8FF5
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 06/17/2016
ms.openlocfilehash: b508cc12f468e5b9dfef91718585f61bfd633816
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73030060"
---
# <a name="common-patterns-and-idioms-in-xamarinmac"></a>Allgemeine Muster und Idiome in xamarin. Mac

In den Apple-APIs, C#die über verfügbar gemacht werden, werden bestimmte Idiome und Muster wieder immer wieder hochskalieren. Wenn Sie Erfahrungen mit der Programmierung mit xamarin. IOS haben, sind diese möglicherweise vertraut. Die Dokumentation bezieht sich häufig auf diese Muster und Idiome, sodass ein solides Verständnis der Dokumente Ihnen hilft, die von Ihnen festzulegende Dokumentation zu verstehen.

## <a name="mvc---model-view-controller"></a>MVC-Modell Ansichts Controller

Der Modell Ansichts Controller (oder MVC) ist ein sehr gängiges Muster, das in Cocoa enthalten ist. Eine ausführliche Erläuterung geht über den Rahmen dieses Dokuments hinaus, aber kurz gesagt, es handelt sich um eine Möglichkeit, Ihre Anwendung in Komponenten zu strukturieren:

- **Modell** Objekte repräsentieren die zugrunde liegenden Daten, die angezeigt und bearbeitet werden (z. b. Adressen in einem Adressbuch).
- Objekte **anzeigen** behandelt die Zeichnung eines bestimmten Objekts auf dem Bildschirm und behandelt die Benutzerinteraktion (ein Textfeld, das die Adresse auf dem Bildschirm anzeigt).
- **Controller** Objekte behandeln die Interaktion zwischen dem Modell und der Ansicht. Sie überschreiben die Modelländerungen nach oben, um die Ansicht zu aktualisieren und "nach unten" Änderungen aus der Ansicht zu verschieben, wenn Benutzer Änderungen an der Benutzeroberfläche vornehmen.

Wenn Sie mit MVVM (Model View ViewModel) aus anderen Bibliotheken, wie z. b. WPF, vertraut sind, verhält sich der Controller ähnlich wie "ViewModel", ist aber oft enger an die spezifischen Benutzeroberflächen Elemente gebunden.

Weitere Informationen finden Sie hier:

- [Erlernen von MVC auf der Apple-Website](https://developer.apple.com/library/ios/documentation/general/conceptual/devpedia-cocoacore/MVC.html)

- [Modell Ansichts Controller in Ziel-C](https://developer.apple.com/library/ios/documentation/general/conceptual/CocoaEncyclopedia/Model-View-Controller/Model-View-Controller.html)
- [Arbeiten mit Windows](~/mac/user-interface/window.md)

## <a name="data-source--delegate--subclassing"></a>Datenquelle/Delegat/Unterklassen

Ein weiteres sehr häufiges Muster in Cocoa ist das Bereitstellen von Daten für Benutzeroberflächen Elemente und das reagieren auf Benutzerinteraktionen. Wenn Sie `NSTableView` als Beispiel verwenden, müssen Sie die Daten für jede Zeile, eine Reihe von Elementen der Benutzeroberfläche, die diese Zeile darstellen, einige Verhaltensweisen für die Reaktion auf Benutzerinteraktionen und ggf. eine gewisse Anpassung bereitstellen. Mit den Datenquellen-und delegatmustern können Sie die meisten Fälle verarbeiten, ohne sich auf die Unterklassen `NSTableView` selbst zurückgreifen zu müssen.

- Der `DataSource`-Eigenschaft wird eine Instanz einer benutzerdefinierten Unterklasse von zugewiesen `NSTableViewDataSource` die aufgerufen wird, um die Tabelle mit Daten (über `GetRowCount` und `GetObjectValue`) aufzufüllen.

- Der `Delegate`-Eigenschaft wird eine Instanz einer benutzerdefinierten Unterklasse von zugewiesen `NSTableViewDelegate` die die Ansicht für ein bestimmtes Modell Objekt (über `GetViewForItem`) bereitstellt und Interaktionen mit der Benutzeroberfläche verarbeitet (über `DidClickTableColumn`, `MouseDownInHeaderOfTableColumn`usw.).

In einigen Fällen möchten Sie ein Steuerelement auf eine Weise anpassen, die über die im Delegaten oder der Datenquelle angegebenen Hooks hinausgeht, und Sie können die Ansicht direkt Unterklassen. Achten Sie jedoch darauf, dass Sie in vielen Fällen, in denen das Überschreiben von Standardverhalten erforderlich ist, das gesamte Verhalten selbst verarbeiten müssen. (beim Anpassen des Auswahl Verhaltens ist es möglicherweise erforderlich, dass Sie alle Auswahl Verhalten selbst implementieren.)

In xamarin. IOS wurden einige APIs, z. b. `UITableView`, mit einer Eigenschaft erweitert, die sowohl den Delegaten als auch die Datenquelle (`UITableViewSource`) implementiert. Dadurch wird die allgemeine Einschränkung umgangen, dass eine einzelne C# Klasse nur eine Basisklasse aufweisen kann, und die Oberfläche von Protokollen wird über Basisklassen erstellt.

Weitere Informationen zum Arbeiten mit Tabellen Sichten in einer xamarin. Mac-Anwendung finden Sie in der Dokumentation zur [Tabellenansicht](~/mac/user-interface/table-view.md) .

## <a name="protocols"></a>Protokolle

Protokolle in Ziel-C können mit Schnittstellen in C#verglichen werden, und in vielen Fällen werden Sie in ähnlichen Situationen verwendet. Beispielsweise handelt es sich bei dem obigen `NSTableView` Beispiel um den Delegaten und die Datenquelle. Xamarin. Mac macht diese als Basisklassen mit virtuellen Methoden verfügbar, die Sie außer Kraft setzen können. Der Hauptunterschied zwischen C# Schnittstellen und Ziel-C-Protokollen besteht darin, dass einige Methoden in einem Protokoll optional für die Implementierung von optional sein können. Sie müssen sich die Dokumentation und/oder Definition einer API ansehen, um zu bestimmen, was optional ist.

Weitere Informationen finden Sie in unserer Dokumentation zu Delegaten [, Protokollen und Ereignissen](~/ios/app-fundamentals/delegates-protocols-and-events.md) .

## <a name="related-links"></a>Verwandte Links

- [Tabellen Sichten](~/mac/user-interface/table-view.md)
- [Arbeiten mit Windows](~/mac/user-interface/window.md)
- [Delegaten, Protokolle und Ereignisse](~/ios/app-fundamentals/delegates-protocols-and-events.md)
- [Model-View-Controller](https://developer.apple.com/library/ios/documentation/general/conceptual/CocoaEncyclopedia/Model-View-Controller/Model-View-Controller.html)
