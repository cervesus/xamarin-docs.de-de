---
title: Automatisches Layout mit dem Xamarin Designer für IOS
description: Dieses Handbuch enthält eine Einführung in das automatische IOS-Layout und beschreibt die Verwendung der Xamarin Designer für IOS zum Erstellen und Bearbeiten von Layouts mithilfe von Einschränkungen. Außerdem wird erläutert, wie Einschränkungen im Code geändert werden, Einschränkungs Änderungen animiert werden und vieles mehr.
ms.prod: xamarin
ms.assetid: CAC7A715-55BB-45E2-BB6D-2168D36D428F
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/21/2017
ms.openlocfilehash: 35a8d3aeb00ac73f944712cb31f913f98bd3b6e8
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2020
ms.locfileid: "76725471"
---
# <a name="auto-layout-with-the-xamarin-designer-for-ios"></a>Automatisches Layout mit dem Xamarin Designer für IOS

Das automatische Layout (auch als "Adaptive Layout" bezeichnet) ist ein reaktionsfähiges Entwurfskonzept. Anders als das Übergangs Layoutsystem, bei dem die Position jedes Elements an einem Punkt auf dem Bildschirm hart codiert ist, bezieht sich das automatische Layout auf *Beziehungen* -die Positionen von Elementen relativ zu anderen Elementen auf der Entwurfs Oberfläche. Das Herzstück des automatischen Layouts ist die Idee von Einschränkungen oder Regeln, die die Platzierung eines Elements oder einer Gruppe von Elementen im Kontext anderer Elemente auf dem Bildschirm definieren. Da die Elemente nicht an eine bestimmte Position auf dem Bildschirm gebunden sind, helfen Einschränkungen bei der Erstellung eines adaptiven Layouts, das in verschiedenen Bildschirmgrößen und Geräte Ausrichtungen gut aussieht.

In dieser Anleitung werden Einschränkungen eingeführt und erläutert, wie Sie im xamarin IOS-Designer verwendet werden. Dieses Handbuch behandelt nicht Programm gesteuert das Arbeiten mit Einschränkungen. Informationen zur programmgesteuerten Verwendung des automatischen Layouts finden Sie in der [Apple-Dokumentation](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/AutolayoutPG/ProgrammaticallyCreatingConstraints.html).

## <a name="requirements"></a>-Anforderungen

Der Xamarin Designer für IOS ist in Visual Studio für Mac in Visual Studio 2017 und höher unter Windows verfügbar.

In diesem Leitfaden wird davon ausgegangen, dass die Komponenten des Designers von der [Einführung in den IOS-Designer](~/ios/user-interface/designer/introduction.md) Handbuch kennen.

## <a name="introduction-to-constraints"></a>Einführung in Einschränkungen

Eine Einschränkung ist eine mathematische Darstellung der Beziehung zwischen zwei Elementen auf dem Bildschirm. Das darstellen der Position eines Benutzeroberflächen Elements als mathematische Beziehung löst verschiedene Probleme, die mit der hart Codierung der Position eines Benutzeroberflächen Elements einhergehen. Wenn Sie z. b. eine Schaltfläche 20px vom unteren Bildschirmrand im Hochformat platzieren, befindet sich die Position der Schaltfläche im Querformat außerhalb des Bildschirms. Um dies zu vermeiden, können wir eine Einschränkung festlegen, die den unteren Rand der Schaltfläche 20px vom unteren Rand der Ansicht platziert. Die Position für den Schaltflächen Rand wird dann als *Button. Bottom = View. Bottom-20px*berechnet, wodurch die Schaltfläche 20px vom unteren Rand der Ansicht im hoch-und Querformat platziert wird. Die Möglichkeit, die Platzierung auf der Grundlage einer mathematischen Beziehung zu berechnen, ist das, was Einschränkungen für den Entwurf der Benutzeroberfläche ermöglicht.

Wenn eine Einschränkung festgelegt wird, wird ein `NSLayoutConstraint` Objekt erstellt, das die zu Beschränkungs enden Objekte und die Eigenschaften oder *Attribute*, auf die die Einschränkung reagieren soll, als Argumente annimmt. Im IOS-Designer enthalten Attribute z. b. den *linken*, *rechten*, *oberen*und *unteren Rand* eines Elements. Dazu zählen auch Größen Attribute wie *height* und *Width*sowie die Mittelpunkt Position, *CenterX* und *CenterY*. Wenn Sie z. b. eine Einschränkung an der Position der linken Begrenzung von zwei Schaltflächen hinzufügen, erzeugt der Designer den folgenden Code im Bereich:

```csharp
View.AddConstraint (NSLayoutConstraint.Create (Button1, NSLayoutAttribute.Left, NSLayoutRelation.Equal, Button2, NSLayoutAttribute.Left, 1, 10));
```

Der nächste Abschnitt behandelt das Arbeiten mit Einschränkungen mithilfe des IOS-Designers, einschließlich aktivieren und Deaktivieren des automatischen Layouts und Verwenden der Einschränkungs Symbolleiste.

## <a name="enable-auto-layout"></a>Automatisches Layout aktivieren

Die Standardkonfiguration für den IOS-Designer hat den Einschränkungs Modus aktiviert. Wenn Sie es jedoch manuell aktivieren oder deaktivieren müssen, können Sie dies in zwei Schritten tun:

1. Klicken Sie auf einen leeren Bereich auf der Entwurfs Oberfläche. Dadurch werden alle Elemente deaktiviert und die Eigenschaften für das Storyboard-Dokument angezeigt.
1. Aktivieren oder deaktivieren Sie das Kontrollkästchen " **AutoLayout verwenden** " im Eigenschaften Panel:

    ![](designer-auto-layout-images/image01.png "The Use Autolayout checkbox in the property panel")

Standardmäßig werden keine Einschränkungen auf der-Oberfläche erstellt oder sichtbar. Stattdessen werden Sie zum Zeitpunkt der Kompilierung automatisch aus den Frame Informationen abgeleitet. Zum Hinzufügen von Einschränkungen müssen wir ein Element auf der Entwurfs Oberfläche auswählen und Einschränkungen hinzufügen. Dazu können Sie die Einschränkungs **Symbolleiste**verwenden.

## <a name="constraints-toolbar"></a>Einschränkungs Symbolleiste

 [![](designer-auto-layout-images/toolbarnew.png "The Context Menu Commands")](designer-auto-layout-images/toolbarnew.png#lightbox)

Die Einschränkungs Symbolleiste wurde aktualisiert und besteht nun aus zwei Hauptteilen:

- **Schaltfläche "Einschränkungs Modus**": zuvor haben Sie den Einschränkungs Modus aktiviert, indem Sie auf der Entwurfs Oberfläche erneut auf eine ausgewählte Ansicht geklickt haben. Sie sollten nun diese UMSCHALT Fläche in der Einschränkungs Leiste verwenden:

  ![Schalter für Verbindungs Modi](designer-auto-layout-images/constraints.png)

- **Schaltfläche "Einschränkungen aktualisieren":** Es ist wichtig zu beachten, dass die Änderungen abhängig davon, ob Sie sich im Bearbeitungsmodus für Einschränkungen befinden, geändert werden.
  - Im Einschränkungs Bearbeitungsmodus passt diese Schaltfläche die Einschränkungen so an, dass Sie dem Element Rahmen entsprechen.
  - Im Frame Bearbeitungsmodus passt diese Schaltfläche den Element Rahmen an die Position an, die die Einschränkungen definieren.

## <a name="constraints-editing-popover"></a>Einschränkungen bei der Bearbeitung von popover

Mit dem Editor für Einschränkungs-Editor können Sie mehrere Einschränkungen gleichzeitig für eine ausgewählte Ansicht hinzufügen und aktualisieren. Wir können mehrere Abstände, Seitenverhältnis und Ausrichtungs Einschränkungen erstellen, wie z. b. das Ausrichten einer Ansicht an den linken Rand zweier Ansichten.

Zum Bearbeiten von Einschränkungen für die ausgewählte Ansicht klicken Sie auf das Auslassungs Zeichen, um die popover-![Einschränkungen für die Einschränkungen anzuzeigen](designer-auto-layout-images/constraints-popup.png)

Beim Öffnen des popover "Einschränkungen" werden alle vordefinierten Einschränkungen für die Sicht angezeigt. Wir können alle Abstands Einschränkungen festlegen, indem wir **alle Seiten** aus dem Kombinations Feld in der oberen rechten Ecke auswählen und **Alle löschen** auswählen, um Sie zu entfernen.

Der Wert von **W** legt Width fest, und **H** legt die Height-Einschränkung fest. Wenn Sie das **Seitenverhältnis**überprüfen, werden die Höhe und Breite der Ansichten auf unterschiedlichen Bildschirmgrößen gesteuert, die Breite der Ansicht wird als Zähler für die Ration und die Höhe als Nenner verwendet.

![Einschränkungs Abstände](designer-auto-layout-images/constraints-spacing.png)

Vier Kombinations Felder für Abstands Einschränkungen listet die benachbarten Sichten auf, um die Einschränkung zu verankern.

## <a name="surface-based-constraint-editing"></a>Bearbeitung von Oberflächen basierter Einschränkung

Für eine präzisere Einschränkungs Bearbeitung können wir direkt auf der Entwurfs Oberfläche mit Einschränkungen interagieren. In diesem Abschnitt werden die Grundlagen der oberflächenbasierten Einschränkungs Bearbeitung erläutert, einschließlich der Steuerelemente für PIN-Abstände, Ablage Bereiche und der Arbeit mit unterschiedlichen Einschränkungs Typen.

### <a name="creating-constraints"></a>Erstellen von Einschränkungen

Das IOS-Designer-Tool bietet zwei Arten von Steuerelementen zum Bearbeiten von Elementen auf der Entwurfs Oberfläche. *Ziehen* von Steuerelementen und Steuer *Elementen für PIN-Abstände*, wie in der folgenden Abbildung dargestellt:

![Steuerelemente anzeigen](designer-auto-layout-images/controls.png)

Diese werden ein-und ausgeschaltet, indem die Schaltfläche Einschränkungs Modus in der Einschränkungs Leiste ausgewählt wird.

Die vier T-förmigen Handles auf jeder Seite des Elements definieren den *oberen*, *rechten*, *unteren*und *linken* Rand des Elements für eine Einschränkung. Die zwei I-förmigen Handles am rechten und unteren Rand des Elements definieren die *Höhen* -und *breiten* Einschränkung. Das mittlere Quadrat übernimmt sowohl *CenterX* -als auch *CenterY* -Einschränkungen.

Wählen Sie zum Erstellen einer Einschränkung ein Handle aus, und ziehen Sie es an eine beliebige Stelle auf der Entwurfs Oberfläche. Wenn Sie den Zieh Vorgang starten, werden auf der Oberfläche eine Reihe von grünen Linien/Feldern angezeigt, die Sie darüber informieren, was Sie einschränken können. Beispielsweise beschränken wir im folgenden Screenshot die obere Seite der mittleren Schaltfläche:

 [![](designer-auto-layout-images/image07.png "Constraining the top side of the middle button")](designer-auto-layout-images/image07.png#lightbox)

Beachten Sie die drei gestrichelten grünen Linien für die anderen beiden Schaltflächen. Die grünen Linien geben Ablage *Bereiche*oder die Attribute anderer Elemente an, die eingeschränkt werden können. Im obigen Screenshot bieten die anderen beiden Schaltflächen drei vertikale Ablage Bereiche ( *unten*, *CenterY*, *Top*), um die Schaltfläche einzuschränken. Die gestrichelte grüne Linie am oberen Rand der Ansicht bedeutet, dass der Ansichts Controller am oberen Rand der Ansicht eine Einschränkung bietet, und das solide grüne Feld bedeutet, dass der Ansichts Controller eine Einschränkung unterhalb des oberen layouthandbuchs bietet.

> [!IMPORTANT]
> Layouthandbücher sind spezielle Einschränkungs Ziele, die es uns ermöglichen, obere und untere Einschränkungen zu erstellen, die das vorhanden sein von System leisten, z. b. Status leisten oder Symbolleisten, berücksichtigen. Einer der Haupt Verwendungsmöglichkeiten besteht darin, dass eine APP zwischen IOS 6 und IOS 7 kompatibel ist, da in der neuesten Version die Container Ansicht unter der Statusleiste angezeigt wird. Weitere Informationen finden Sie in der [Apple-Dokumentation](https://developer.apple.com/library/ios/documentation/userexperience/conceptual/transitionguide/AppearanceCustomization.html#//apple_ref/doc/uid/TP40013174-CH15-SW2).

In den nächsten drei Abschnitten wird die Arbeit mit verschiedenen Einschränkungs Typen eingeführt.

### <a name="size-constraints"></a>Größen Einschränkungen

Mit Größenbeschränkungen ( *Höhe* und *Breite* ) haben Sie zwei Möglichkeiten. Die erste Option besteht darin, das Handle zu ziehen, um eine benachbarte Elementgröße einzuschränken, wie im obigen Beispiel veranschaulicht. Die andere Option besteht darin, auf das Handle zu doppelklicken, um eine selbst Einschränkung zu erstellen. Auf diese Weise können wir einen konstanten Größen Wert angeben, wie im folgenden Screenshot veranschaulicht:

 [![](designer-auto-layout-images/sizec.png "Drag the handle to constrain to a neighbor element size, as illustrated here")](designer-auto-layout-images/sizec.png#lightbox)

### <a name="center-constraints"></a>Center-Einschränkungen

Das quadratische Handle erstellt eine *CenterX* -oder *CenterY* -Einschränkung, abhängig vom Kontext. Wenn Sie den quadratischen Zieh Punkt ziehen, werden die anderen Elemente so angezeigt, dass Sie sowohl vertikale als auch horizontale Ablage Bereiche bieten, wie im folgenden Screenshot veranschaulicht:

 [![](designer-auto-layout-images/centerc.png "Center Constraints")](designer-auto-layout-images/centerc.png#lightbox)

Wenn Sie einen vertikalen Ablage Bereich auswählen, wird eine *CenterY* -Einschränkung erstellt. Wenn Sie einen horizontalen Ablage Bereich auswählen, wird die Einschränkung auf " *CenterX*" basieren.

### <a name="combinational-constraints"></a>Combinational-Einschränkungen

Zum Erstellen von Gleichheits-und Größen Gleichheits Einschränkungen zwischen zwei Elementen können Sie Elemente aus einer oberen Symbolleiste auswählen, um die Reihenfolge der Reihenfolge der horizontalen Ausrichtung, die vertikale Ausrichtung und Größe von equalitäten anzugeben, wie im folgenden Screenshot veranschaulicht:

 [![](designer-auto-layout-images/image06.png "Combinational Constraints")](designer-auto-layout-images/image06.png#lightbox)

### <a name="visualizing-and-editing-constraints"></a>Visualisieren und Bearbeiten von Einschränkungen

Wenn Sie eine Einschränkung hinzufügen, wird Sie auf der Entwurfs Oberfläche als blaue Linie angezeigt, wenn Sie ein Element auswählen:

 [![](designer-auto-layout-images/image09.png "Visualizing Constraints")](designer-auto-layout-images/image09.png#lightbox)

Sie können eine Einschränkung auswählen, indem Sie auf eine blaue Linie klicken und die Einschränkungs Werte direkt im Eigenschaften Panel bearbeiten. Wenn Sie auf eine blaue Linie doppelklicken, wird ein popover angezeigt, mit dem Sie die Werte direkt auf der Entwurfs Oberfläche bearbeiten können:

 [![](designer-auto-layout-images/image08.png "Editing Constraints")](designer-auto-layout-images/image08.png#lightbox)

## <a name="constraint-issues"></a>Einschränkungs Probleme

Bei der Verwendung von Einschränkungen können verschiedene Arten von Problemen auftreten:

- **Widersprüchliche Einschränkungen** – Dies ist der Fall, wenn mehrere Einschränkungen erzwingen, dass das Element in Konflikt stehende Werte für ein Attribut aufweist und die Einschränkungs-Engine diese nicht abstimmen kann.
- **Unter beschränkte Elemente** – die Eigenschaften eines Elements (Location + size) müssen vollständig durch den Satz von Einschränkungen und systeminternen Größen abgedeckt werden, damit die Einschränkungen gültig sind. Wenn diese Werte mehrdeutig sind, wird das Element als nicht eingeschränkt bezeichnet.
- **Frame-fehllacement** – Dies tritt auf, wenn ein Element Rahmen und sein Satz von Einschränkungen zwei verschiedene resultierende Rechtecke definieren.

In diesem Abschnitt werden die drei oben aufgeführten Probleme erläutert, und es wird erläutert, wie Sie behandelt werden.

### <a name="conflicting-constraints"></a>Widersprüchliche Einschränkungen

Widersprüchliche Einschränkungen sind rot markiert und weisen ein Warnsymbol auf. Wenn Sie mit dem Mauszeiger auf die Warn Symbole zeigen, wird ein Popup mit Informationen über den Konflikt angezeigt:

 [![](designer-auto-layout-images/image11.png "Conflicting Constraints warning")](designer-auto-layout-images/image11.png#lightbox)

### <a name="underconstrained-items"></a>Unter beschränkte Elemente

Unter beschränkte Elemente werden Orange angezeigt und zeigen die Darstellung eines orangefarbenen markersymbols in der Ansichts Controller-Objekt Leiste an:

 [![](designer-auto-layout-images/image02.png "Underconstrained items appear in orange")](designer-auto-layout-images/image02.png#lightbox)

Wenn Sie auf dieses Markierungs Symbol klicken, können Sie Informationen zu unter beschränkten Elementen in der Szene erhalten und die Probleme lösen, indem Sie Sie entweder vollständig einschränken oder indem Sie Ihre Einschränkungen entfernen, wie im folgenden Screenshot veranschaulicht:

 [![](designer-auto-layout-images/image10.png "Fixing Underconstrained Items")](designer-auto-layout-images/image10.png#lightbox)

### <a name="frame-misplacement"></a>Frame-fehl-Element

Der Frame-"-fehlpunkt" verwendet denselben Farbcode wie unter beschränkte Elemente. Das Element wird immer auf der-Oberfläche gerendert, indem es seinen systemeigenen Frame verwendet, aber im Fall eines Frame-fehl Zeichens wird ein rotes Rechteck gekennzeichnet, wo das Element am Ende der Anwendung ausgeführt wird, wie im folgenden Screenshot veranschaulicht:

 [![](designer-auto-layout-images/image05.png "Sample Frame Misplacement view")](designer-auto-layout-images/image05.png#lightbox)

Wählen Sie zum Auflösen von Frame-Fehler behebe Fehlern auf der Symbolleiste Einschränkungen die Schaltfläche **Frames basierend auf Einschränkungen aktualisieren** aus (ganz rechts auf die Schaltfläche):

 [![](designer-auto-layout-images/image03.png "Update Frames based on Constraints toolbar button")](designer-auto-layout-images/image03.png#lightbox)

Dadurch wird der Element Rahmen automatisch so angepasst, dass er den von den Steuerelementen definierten Positionen entspricht.

<a name="modifying-in-code" />

## <a name="modifying-constraints-in-code"></a>Ändern von Einschränkungen in Code

Abhängig von den Anforderungen Ihrer APP kann es vorkommen, dass Sie eine Einschränkung im Code ändern müssen. Wenn Sie z. b. die Größe der Sicht ändern oder neu positionieren möchten, wird eine Einschränkung an angefügt, um die Priorität einer Einschränkung zu ändern oder eine Einschränkung vollständig zu deaktivieren.

Um auf eine Einschränkung im Code zuzugreifen, müssen Sie Sie zunächst im IOS-Designer verfügbar machen, indem Sie die folgenden Schritte ausführen:

1. Erstellen Sie die Einschränkung wie gewohnt (mit einer der oben aufgeführten Methoden).
2. Suchen Sie im Dokument Gliederungs- **Explorer**die gewünschte Einschränkung, und wählen Sie Sie aus:

    [![](designer-auto-layout-images/modify01.png "The Document Outline Explorer")](designer-auto-layout-images/modify01.png#lightbox)
3. Weisen Sie der Einschränkung im nächsten Schritt im **Eigenschaften-Explorer**auf der Registerkarte **Widget** einen **Namen** zu:

    [![](designer-auto-layout-images/modify02.png "The Widget Tab")](designer-auto-layout-images/modify02.png#lightbox)
4. Speichern Sie die Änderungen.

Nachdem Sie die obigen Änderungen vorgenommen haben, können Sie im Code auf die Einschränkung zugreifen und ihre Eigenschaften ändern. Beispielsweise können Sie Folgendes verwenden, um die Höhe der angefügten Ansicht auf NULL festzulegen:

```csharp
ViewInfoHeight.Constant = 0;
```

Mit der folgenden Einstellung für die Einschränkung im IOS-Designer:

[![](designer-auto-layout-images/modify03.png "Editing a Constraint in the Property Explorer")](designer-auto-layout-images/modify03.png#lightbox)

### <a name="the-deferred-layout-pass"></a>Der verzögerte Layoutdurchlauf.

Anstatt die angefügte Ansicht als Reaktion auf Einschränkungs Änderungen zu aktualisieren, plant das Modul für automatisches Layout in naher Zukunft eine _verzögerte layoutdurchführung_ . Bei diesem verzögerten Durchlauf wird nicht nur die Einschränkung der angegebenen Ansicht aktualisiert, sondern die Einschränkungen für jede Ansicht in der Hierarchie werden neu berechnet und aktualisiert, um das neue Layout anzupassen.

Sie können zu jedem Zeitpunkt ihren eigenen verzögerten Layoutdurchlauf planen, indem Sie die Methoden `SetNeedsLayout` oder `SetNeedsUpdateConstraints` der übergeordneten Ansicht aufrufen.

Der verzögerte Layoutdurchlauf besteht aus zwei eindeutigen Durchläufen der Ansichts Hierarchie:

- **Der Update Pass** : in diesem Durchlauf durchsucht das Modul für automatisches Layout die Ansichts Hierarchie und ruft die `UpdateViewConstraints`-Methode für alle Ansichts Controller und die `UpdateConstraints`-Methode für alle Ansichten auf.
- **Das Layout** wird erneut ausgeführt, die Auto Layout-Engine durchläuft die Ansichts Hierarchie, aber dieses Mal Ruft die `ViewWillLayoutSubviews`-Methode für alle Ansichts Controller und die `LayoutSubviews`-Methode für alle Ansichten auf. Die `LayoutSubviews`-Methode aktualisiert die `Frame`-Eigenschaft jeder unter Ansicht mit dem Rechteck, das von der Auto Layout-Engine berechnet wird.

### <a name="animating-constraint-changes"></a>Animieren von Einschränkungs Änderungen

Zusätzlich zum Ändern von Einschränkungs Eigenschaften können Sie die Core-Animation verwenden, um Änderungen an den Einschränkungen einer Ansicht zu animieren. Beispiel:

```csharp
UIView.BeginAnimations("OpenInfo");
UIView.SetAnimationDuration(1.0f);
ViewInfoHeight.Constant = 237;
View.LayoutIfNeeded();

//Execute Animation
UIView.CommitAnimations();
```

Der Schlüssel hier ist das Aufrufen der `LayoutIfNeeded`-Methode der übergeordneten Sicht innerhalb des Animations Blocks. Dadurch wird die Ansicht aufgefordert, jeden "Frame" der animierten Position oder Größenänderung zu zeichnen. Ohne diese Zeile würde die Ansicht einfach an die endgültige Version andocken, ohne eine Animation durchlaufen zu müssen.

## <a name="summary"></a>Summary

In diesem Handbuch wurden das automatische IOS-Layout (oder das adaptive Layout) und das Konzept der Einschränkungen als mathematische Darstellungen von Beziehungen zwischen Elementen auf der Entwurfs Oberfläche vorgestellt. Es wurde beschrieben, wie Sie das automatische Layout im IOS-Designer aktivieren, mit der **Einschränkungs Symbolleiste**arbeiten und Einschränkungen einzeln auf der Entwurfs Oberfläche bearbeiten. Im nächsten Schritt wurde erläutert, wie drei häufige Einschränkungs Probleme behoben werden. Schließlich wurde gezeigt, wie Einschränkungen im Code geändert werden.

## <a name="related-links"></a>Verwandte Themen

- [Einführung in Storyboards](~/ios/user-interface/storyboards/index.md)
- [Exemplarische Vorgehensweise für Steuerelemente für IOS](~/ios/user-interface/designer/ios-designable-controls-walkthrough.md)
- [Übersicht über Android Designer](~/android/user-interface/android-designer/index.md)
- [Programmgesteuerte Einschränkungen](~/ios/user-interface/programmatic-layout-constraints.md)
