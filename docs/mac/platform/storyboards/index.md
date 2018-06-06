---
title: Einführung in die Storyboards in Xamarin.Mac
description: Dieser Artikel bietet eine Einführung in das Arbeiten mit Storyboards in einer app Xamarin.Mac. Sie erfahren, wie Sie die UI der App mit Storyboards und Interface Builder von Xcode erstellen und verwalten.
ms.prod: xamarin
ms.assetid: F37BA503-0B25-489F-80A8-58C493291A55
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 027998d6aff8aba4e5621b1cde51a24e18821ff9
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34792664"
---
# <a name="introduction-to-storyboards-in-xamarinmac"></a>Einführung in die Storyboards in Xamarin.Mac

_Dieser Artikel bietet eine Einführung in das Arbeiten mit Storyboards in einer app Xamarin.Mac. Es umfasst das Erstellen und Verwalten der Benutzeroberfläche der Anwendung mit Storyboards und Xcodes Benutzeroberflächen-Generator._

Storyboards ermöglichen es Ihnen, eine Benutzeroberfläche für Ihre app Xamarin.Mac entstehen, enthält nicht nur die Definitionen und Steuerelemente, aber enthält auch die Links zwischen verschiedenen Fenstern (über segues) und Ansichtszustände.

[![](images/intro01.png "Ein Beispiel für die Benutzeroberfläche in Xcode")](images/intro01.png#lightbox)

Dieser Artikel bietet eine Einführung in das Storyboards verwenden, um eine Xamarin.Mac-app-Benutzeroberfläche zu definieren.

<a name="What-are-Storyboards" />

## <a name="what-are-storyboards"></a>Was sind Storyboards?

Mithilfe von Storyboards können aller eine Xamarin.Mac-app-Benutzeroberfläche in einem zentralen Ort mit allen der Navigation zwischen den einzelnen Elementen und Benutzeroberflächen definiert werden. Storyboards für Xamarin.Mac, funktionieren auf sehr ähnliche Weise mit Storyboards für Xamarin.iOS. Sie enthalten jedoch einen anderen Satz von _Segue Typen_ aufgrund der anderen Schnittstelle Idiome.

<a name="Working-with-Scenes" />

### <a name="working-with-scenes"></a>Arbeiten mit Szenen

Wie bereits erwähnt, ein Storyboard definiert alle für die einer bestimmten app aufgeschlüsselt nach Funktionsbereich eine Übersicht über die Benutzeroberfläche der _View Controller_. In Xcodes Benutzeroberflächen-Generator, jedes dieser Controller befindet sich in einem eigenen _Szene_.

[![](images/intro02.png "Ein Beispiel-View-controller")](images/intro02.png#lightbox)

Jede Szene stellt einer angegebenen Ansicht und Controller-Paar Ansicht mit einem Satz von Linien (so genannte Segues), die jede Szene in der Benutzeroberfläche, also mit ihren Beziehungen zu verbinden. Einige Segues definieren, wie eine View-Controller enthält eine oder mehrere untergeordnete Ansichten oder View-Controller. Andere Segues Übergänge zwischen View-Controller (z. B. das Anzeigen eines Popover oder eines Dialogfelds) definieren. 

[![](images/intro03.png "Ein Beispiel segue")](images/intro03.png#lightbox)

Der wichtigste zu beachten ist, dass jede Segue den Datenfluss eine Form von Daten zwischen dem angegebenen Element der Benutzeroberfläche der Anwendung darstellt.

<a name="Working-with-View-Controllers" />

### <a name="working-with-view-controllers"></a>Arbeiten mit View-Controller

View-Controller definieren die Beziehungen zwischen einer bestimmten Ansicht der Informationen innerhalb einer app für Mac und des Datenmodells, die diese Informationen bereitstellt. Jedes oberster Ebene Szene in das Storyboard stellt eine View-Controller in der Xamarin.Mac-app-Code dar.

[![](images/intro04.png "Ein Beispiel für verzögert View-controller")](images/intro04.png#lightbox)

Auf diese Weise ist jeder View-Controller eine unabhängige, wiederverwendbare Kopplung visuelle Darstellung von den Informationen (Ansicht) und die Logik und die Informationen zu steuern.

Innerhalb einer bestimmten Szene erreichen Sie, alle der Aufgaben, die normalerweise von einzelnen bearbeitet werden würde `.xib` Dateien: 

 - Subviews und (z. B. Schaltflächen und Textfeldern) steuert.
 - Element Positionen und Auto-Layout-Einschränkungen zu definieren.
 - Über das Netzwerk von Aktionen und Ausgänge Benutzeroberflächenelementen für Code verfügbar machen.

<a name="Working-with-Segues" />

### <a name="working-with-segues"></a>Arbeiten mit Segues

Wie bereits erwähnt, Segues Geben Sie die Beziehungen zwischen allen die Szenen, die Benutzeroberfläche Ihrer Anwendung zu definieren. Wenn Sie mit der Arbeit im Storyboards in iOS vertraut sind, wissen Sie, die Segues für iOS in der Regel Übergänge zwischen den gesamten Bildschirmansichten definieren. Dies unterscheidet sich von MacOS, wenn Segues definieren in der Regel "Aufnahme" (wobei eine Szene für das untergeordnete Element eines übergeordneten Elements Szene ist).

Die meisten apps tendenziell in MacOS ihre Ansichten in das gleiche Fenster über Benutzeroberflächenelemente, z. B. Split-Sichten und Registerkarten zu gruppieren. Zeigen Sie im Gegensatz zu iOS, in denen Ansichten ein- und ausschalten Bildschirm gewechselt werden müssen, aufgrund der begrenzten physische Speicherplatz.

Angesichts des MacOS Tendenzen für Containment, es gibt Situationen, in denen _Präsentation Segues_ verwendet werden, z. B. modale Fenster, Ansichten und Popovers.

Wenn Segues Präsentation verwenden, können Sie überschreiben die `PrepareForSegue` Methode des übergeordneten Elements View Controller für die Präsentation zum Initialisieren und Variablen, und geben Sie alle Daten, die View-Controller angezeigt wird.

<a name="Design-and-Run-Times" />

### <a name="design-and-run-times"></a>Entwerfen und Ausführen von Zeiten

Zur Entwurfszeit (wenn Layout, die Benutzeroberfläche in Xcodes Benutzeroberflächen-Generator), wird jedes Element der Benutzeroberfläche der Anwendung in der enthaltenen Elemente unterteilt:

- **Szenen** – welche bestehen aus:
    - **Anzeigen von Controller** -, definieren die Beziehungen zwischen den Ansichten und die Daten, die sie unterstützen.
    - **Sichten und Unteransichten** – die tatsächlichen Elemente, aus denen die Benutzeroberfläche besteht.
    - **Kapselung Segues** -, die die über-/ unterordnungsbeziehungen zwischen Szenen definieren.
- **Präsentation Segues** -, die einzelne Präsentationsmodus definieren. 

Jedes Element auf diese Weise definieren, können für das verzögerte Laden jedes Element nur verwendet werden, da sie während der Laufzeit erforderlich ist. Der gesamte Prozess wurde in MacOS, ermöglichen den Entwickler, komplexe, flexible Benutzeroberflächen zu erstellen, müssen ein absolutes Minimum von Code, damit Sie sie arbeiten, sichern, entwickelt und wird mit Systemressourcen so effizient wie möglich.

<a name="Storyboard-Quick-Start" />

## <a name="storyboard-quick-start"></a>Storyboard-Schnellstart

In der [Storyboard Schnellstart](~/mac/platform/storyboards/quickstart.md) Handbuch, wir erstellen eine einfache Xamarin.Mac-app, die die grundlegenden Konzepte der Arbeit mit Storyboards erstellen Sie eine neue Benutzeroberfläche eingeführt werden. Die Beispiel-app besteht aus einer Sicht aufgeteilt, enthält eine _Inhaltsbereichs_ und ein _Inspektor Bereich_ und er bietet ein einfaches Fenster mit Voreinstellungen-Dialogfeld. Wir müssen Segues verwenden, um alle Elemente der Benutzeroberfläche zu verbinden.

<a name="Working-with-Storyboards" />

## <a name="working-with-storyboards"></a>Arbeiten mit Storyboards

Dieser Abschnitt enthält ausführliche Details [arbeiten mit Storyboards](~/mac/platform/storyboards/indepth.md) in einer Xamarin.Mac-app. Wir nehmen eine ausführliche Betrachtung Szenen und wie sie der Ansicht Controller und Ansicht zusammengesetzt sind. Klicken Sie dann, müssen wir sehen Sie sich wie Szenen zusammen mit Segues gebunden sind. Schalten Sie schließlich einen Blick auf das Arbeiten mit benutzerdefinierten Segue-Typen. 

<a name="Complex-Storyboard-Example" />

## <a name="complex-storyboard-example"></a>Komplexe Storyboard-Beispiel

Ein Beispiel eines komplexen Beispiels mit Storyboards in einer app Xamarin.Mac zu arbeiten, finden Sie unter der [SourceWriter Beispiel-App](https://developer.xamarin.com/samples/mac/SourceWriter/). SourceWriter ist ein einfacher Quellcode-Editor, der Unterstützung für die Codevervollständigung und die einfache Syntaxhervorhebung bietet.

Der SourceWriter-Code wurde vollständig kommentiert und es wurden, wenn möglich, Links der Schlüsseltechnologien und -methoden zu wichtigen Informationen in den Leitfäden der Xamarin.Mac-Dokumentation bereitgestellt.

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

Dieser Artikel hat einen kurzen Blick auf das Arbeiten mit Storyboards in einer app Xamarin.Mac übernommen. Wir haben gesehen, Gewusst wie: Erstellen einer neue app, die mithilfe von Storyboards und wie Sie eine Benutzeroberfläche zu definieren. Auch wurde erläutert, wie zum Navigieren zwischen Fenstern und segues Ansichtszustände verwenden.


## <a name="related-links"></a>Verwandte Links

- [„Hallo, Mac“-Beispiel](https://developer.xamarin.com/samples/mac/Hello_Mac/)
- [Hello, Mac (Hallo, Mac)](~/mac/get-started/hello-mac.md)
- [Arbeiten mit Fenstern](~/mac/user-interface/window.md)
- [Eingaberichtlinien für OS X](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Einführung in Windows](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
