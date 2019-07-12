---
title: Einführung in Storyboards in Xamarin.Mac
description: Dieser Artikel enthält eine Einführung in die Arbeit mit Storyboards in einer Xamarin.Mac-app. Sie erfahren, wie Sie die UI der App mit Storyboards und Interface Builder von Xcode erstellen und verwalten.
ms.prod: xamarin
ms.assetid: F37BA503-0B25-489F-80A8-58C493291A55
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/14/2017
ms.openlocfilehash: 8a5a2f87c16a5dd040cefb2fbc615b01431ebcf5
ms.sourcegitcommit: 654df48758cea602946644d2175fbdfba59a64f3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2019
ms.locfileid: "67832293"
---
# <a name="introduction-to-storyboards-in-xamarinmac"></a>Einführung in Storyboards in Xamarin.Mac

_Dieser Artikel enthält eine Einführung in die Arbeit mit Storyboards in einer Xamarin.Mac-app. Hierin sind erstellen und verwalten die Benutzeroberfläche der app mit Storyboards und Interface Builder von Xcode._

Storyboards können Sie eine Benutzeroberfläche für die Xamarin.Mac-app zu entwickeln, die enthält nicht nur die Definitionen für Fenster und Steuerelemente, sondern enthält auch die Verknüpfungen zwischen verschiedenen Fenstern (über segues) und Ansichtszustand.

[![](images/intro01.png "Ein Beispiel-Benutzeroberfläche in Xcode")](images/intro01.png#lightbox)

Dieser Artikel bietet eine Einführung zur Verwendung von Storyboards einer Xamarin.Mac-app-Benutzeroberfläche zu definieren.

<a name="What-are-Storyboards" />

## <a name="what-are-storyboards"></a>Was sind Storyboards?

Mithilfe von Storyboards können alle einer Xamarin.Mac-app-Benutzeroberfläche an einem zentralen Ort mit allen die Navigation zwischen die einzelnen Elemente und Benutzeroberflächen definiert werden. Storyboards für Xamarin.Mac, funktionieren auf sehr ähnliche Weise zu Storyboards für Xamarin.iOS. Allerdings enthalten sie einen anderen Satz von _Segue Typen_ aufgrund der anderen Schnittstelle-Sprachen.

<a name="Working-with-Scenes" />

### <a name="working-with-scenes"></a>Arbeiten mit im Hintergrund

Wie bereits erwähnt, ein Storyboard definiert die Benutzeroberfläche für eine bestimmte app in eine Funktionsübersicht von unterteilt die _Ansichtscontroller_. In Interface Builder von Xcode, jede dieser Controller befindet sich in eine eigene _Szene_.

[![](images/intro02.png "Eine Beispiel-ansichtscontroller")](images/intro02.png#lightbox)

Jede Szene stellt eine angegebene Ansicht und die View-Controller-Paar mit einem Satz von Zeilen (Segues genannt), die jeder Szene in der Benutzeroberfläche, also mit ihren Beziehungen zu verbinden. Einige Übergänge definieren, wie ein View-Controller enthält eine oder mehrere untergeordnete Ansichten oder View Controller. Andere Übergänge, Definieren von Übergängen zwischen View-Controller (z. B. das Anzeigen eines Popover oder eines Dialogfelds). 

[![](images/intro03.png "Eine Beispiel-segue")](images/intro03.png#lightbox)

Sehr wichtig zu beachten ist, dass jede Segue den Fluss der Daten zwischen dem angegebenen Element von der Benutzeroberfläche der Anwendung darstellt.

<a name="Working-with-View-Controllers" />

### <a name="working-with-view-controllers"></a>Arbeiten mit Ansichtscontrollern

Ansichtscontroller definieren die Beziehungen zwischen einer bestimmten Ansicht der Informationen in einer Mac-app und das Datenmodell, das die Informationen bereitstellt. Jede Szene auf oberster Ebene im Storyboard stellt einen Ansichtscontroller im Code der Xamarin.Mac-app dar.

[![](images/intro04.png "Ein Beispiel für schlüpft ansichtscontroller")](images/intro04.png#lightbox)

Auf diese Weise wird jedes View Controller an, eine unabhängige, wiederverwendbare Kopplung von den Informationen, die visuelle Darstellung (Ansicht) und die Logik und die Informationen zu steuern.

Innerhalb einer bestimmten Szene, erreichen Sie, alle Dinge, die normalerweise von Person übernommen hätte `.xib` Dateien: 

- Subviews und (z. B. Schaltflächen und Textfelder) steuert.
- Definieren Sie Position von Elementen und automatisches Layout Einschränkungen.
- Über das Netzwerk von Aktionen und Ergebnisdaten um UI-Elemente für Code verfügbar zu machen.

<a name="Working-with-Segues" />

### <a name="working-with-segues"></a>Arbeiten mit Segues

Wie bereits erwähnt, bieten Segues die Beziehungen zwischen allen im Hintergrund, die UIs Ihrer app zu definieren. Wenn Sie mit der Arbeiten in Storyboards unter iOS vertraut sind, beachten Sie, dass Segues für iOS die Übergänge zwischen Vollbild-Ansichten in der Regel definieren. Dies unterscheidet sich von Mac OS, wenn Übergänge werden in der Regel "Aufnahme" (wobei eine Szene für das untergeordnete Element eines übergeordneten Elements Szene ist) definieren.

Die meisten apps tendenziell unter MacOS ihre Ansichten zusammen im gleichen Fenster mithilfe von UI-Elemente wie z. B. geteilten Ansichten und Registerkarten gruppiert. Zeigen Sie im Gegensatz zu iOS, in denen Sichten ein- und ausschalten Bildschirm umgestellt werden müssen, aufgrund der begrenzten physische Speicherplatz.

Angesichts des MacOS-Tendenzen zu Kapselung, es gibt Situationen, in denen _Präsentation Segues_ verwendet werden, z. B. modalen Windows, Tabellenansichten und Popovers.

Wenn Segues Präsentation verwenden, können Sie überschreiben die `PrepareForSegue` -Methode des übergeordneten Ansichtscontroller für Präsentationen zum Initialisieren und Variablen, und geben Sie alle Daten auf den View-Controller, der angezeigt wird.

<a name="Design-and-Run-Times" />

### <a name="design-and-run-times"></a>Entwerfen und Ausführungszeiten

Zur Entwurfszeit (wenn Layout der Benutzeroberfläche in Interface Builder von Xcode), wird jedes Element der Benutzeroberfläche der Anwendung in der enthaltenen Elemente unterteilt:

- **Im Hintergrund** – welche bestehen aus:
    - **Anzeigen von Controller** -, definieren die Beziehungen zwischen Ansichten und die Daten, die sie unterstützen.
    - **Ansichten und Unteransichten** – die tatsächlichen Elemente, aus denen die Benutzeroberfläche besteht.
    - **Kapselung Segues** -, definieren die über-/ unterordnungsbeziehungen zwischen Szenen.
- **Präsentation Segues** -, die einzelne Präsentationsmodus definieren. 

Durch die Definition jedes Element auf diese Weise können ermöglicht es der Lazy jedes Elements nur verwendet werden, da sie während der Laufzeit benötigt wird. Unter MacOS, der gesamte Prozess soll ermöglichen es Entwicklern, komplexe und flexible Benutzeroberflächen zu erstellen, müssen ein absolutes Minimum von Code, um die Arbeit zu sichern, und wird mit Systemressourcen so effizient wie möglich.

<a name="Storyboard-Quick-Start" />

## <a name="storyboard-quick-start"></a>Storyboard-Schnellstart

In der [Storyboard Schnellstart](~/mac/platform/storyboards/quickstart.md) Anleitung erstellen wir eine einfache Xamarin.Mac-app, die die wichtigsten Konzepte für die Arbeit mit Storyboards zum Erstellen einer Benutzeroberfläche eingeführt werden. Die Beispiel-app besteht aus einer Ansicht aufgeteilt, enthält eine _Inhaltsbereich_ und ein _Inspektor Bereich_ und es bietet ein einfaches Dialogfeld "Einstellungen"-Fenster. Wir müssen Übergänge verwenden, um alle Elemente der Benutzeroberfläche zu verbinden.

<a name="Working-with-Storyboards" />

## <a name="working-with-storyboards"></a>Arbeiten mit Storyboards

In diesem Abschnitt finden Sie ausführliche Details zur [arbeiten mit Storyboards](~/mac/platform/storyboards/indepth.md) in einer Xamarin.Mac-app. Wir nehmen einen detaillierten Einblick in die Szenen, sowie die View-Controller und Ansicht bestehen. Dann werden wir sehen Sie sich wie Szenen zusammen mit Segues gebunden sind. Schließlich wird mit der Verwendung von benutzerdefinierten Segue Typen eingegangen. 

<a name="Complex-Storyboard-Example" />

## <a name="complex-storyboard-example"></a>Komplexe Storyboard-Beispiel

Ein Beispiel eines komplexen Beispiels arbeiten mit Storyboards in einer Xamarin.Mac-app, finden Sie unter den [SourceWriter-Beispiel-App](https://developer.xamarin.com/samples/mac/SourceWriter/). SourceWriter ist ein einfacher Quellcode-Editor, der Unterstützung für die Codevervollständigung und die einfache Syntaxhervorhebung bietet.

Der SourceWriter-Code wurde vollständig kommentiert und es wurden, wenn möglich, Links der Schlüsseltechnologien und -methoden zu wichtigen Informationen in den Leitfäden der Xamarin.Mac-Dokumentation bereitgestellt.

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde auf einen Blick auf die Arbeit mit Storyboards in einer Xamarin.Mac-app unternommen. Wir haben gesehen, wie Sie eine neue app mit Storyboards erstellen und wie Sie eine Benutzeroberfläche zu definieren. Außerdem wurde erläutert, wie zum Navigieren zwischen verschiedenen Fenstern und Ansichtszustände mit segues.


## <a name="related-links"></a>Verwandte Links

- [„Hallo, Mac“-Beispiel](https://developer.xamarin.com/samples/mac/Hello_Mac/)
- [Hello, Mac (Hallo, Mac)](~/mac/get-started/hello-mac.md)
- [Arbeiten mit Windows](~/mac/user-interface/window.md)
- [Eingaberichtlinien für OS X](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Einführung in Windows](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
