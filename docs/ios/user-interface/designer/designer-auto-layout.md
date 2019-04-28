---
title: Automatisches Layout mit dem Xamarin-Designer für iOS
description: Dieses Handbuch stellt iOS Automatisches Layout, und es wird beschrieben, wie der Xamarin-Designer zum Erstellen und Bearbeiten von Layouts, die mithilfe von Einschränkungen für iOS zu verwenden. Außerdem wird erläutert, Ändern von Einschränkungen im Code wird das Animieren von Änderungen der Einschränkung, und vieles mehr.
ms.prod: xamarin
ms.assetid: CAC7A715-55BB-45E2-BB6D-2168D36D428F
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/21/2017
ms.openlocfilehash: 41a9ec90b4b734dde7a982ac3d4b2e7b2082321c
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61413073"
---
# <a name="auto-layout-with-the-xamarin-designer-for-ios"></a>Automatisches Layout mit dem Xamarin-Designer für iOS

Automatisches Layout (auch als "adaptive Layout" bezeichnet) ist ein reaktionsfähiges Design-Ansatz. Im Gegensatz zu transitional Layoutsystem, in dem Speicherort des Elements zu einem Zeitpunkt auf dem Bildschirm hartcodiert ist, automatisches Layout geht es um *Beziehungen* -die Positionen der Elemente relativ zu anderen Elementen auf der Entwurfsoberfläche angezeigt. Den Kern des automatischen Layouts bildet das Konzept der Einschränkungen oder Regeln, die die Platzierung eines Elements oder einer Gruppe von Elementen im Kontext von anderen Elementen auf dem Bildschirm zu definieren. Da die Elemente nicht auf eine bestimmte Position auf dem Bildschirm gebunden sind, können Einschränkungen an, ein Layout für die adaptive zu erstellen, die für verschiedene Bildschirmgrößen und geräteausrichtungen in Ordnung ist.

In diesem Leitfaden führen wir Einschränkungen und wie Sie in der Xamarin.IOS-Designer verwendet werden. Dieses Handbuch deckt die Programmgesteuertes Arbeiten mit Einschränkungen nicht. Informationen finden Sie unter Automatisches Layout programmgesteuert, finden Sie in der [Apple-Dokumentation](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/AutolayoutPG/ProgrammaticallyCreatingConstraints.html).

## <a name="requirements"></a>Anforderungen

Xamarin für iOS Designer ist in Visual Studio für Mac in Visual Studio 2017 und höher auf Windows verfügbar.

Dieses Handbuch setzt Kenntnisse der Designer Komponenten unter den [Einführung in iOS Designer](~/ios/user-interface/designer/introduction.md) Guide.

## <a name="introduction-to-constraints"></a>Einführung in die Einschränkungen

Eine Einschränkung ist eine mathematische Darstellung der Beziehung zwischen zwei Elementen, auf dem Bildschirm. Position des Elements als eine mathematische Beziehung ist, das eine Benutzeroberfläche darstellt löst mehrere Probleme im Zusammenhang mit der ein Benutzeroberflächenelement Position fest zu programmieren. Beispielsweise würden wir eine Schaltfläche 20px vom unteren Rand des Bildschirms im Hochformat platzieren, wäre die Position der Schaltfläche außerhalb des Bildschirms im Querformat. Um dies zu vermeiden, können wir eine Einschränkung festlegen, die den unteren Rand der Schaltfläche 20px zwischen dem unteren Rand der Ansicht platziert. Die Position für den Rand der Schaltfläche wird dann als berechnet *button.bottom = view.bottom - 20px*, wäre der der Schaltfläche 20px zwischen dem unteren Rand der Ansicht im Hoch-und Querformat platziert. Die Möglichkeit zum Berechnen der Platzierung basierend auf einer mathematischen Beziehung ist, was Einschränkungen in UI-Entwurf so nützlich ist.

Wenn es sich um eine Einschränkung festzulegen, erstellen wir eine `NSLayoutConstraint` Objekt, das als Argumente verwendet werden sollen, die Objekte eingeschränkt werden und die Eigenschaften oder *Attribute*, die auf die die Einschränkung angewendet wird. In der iOS-Designer enthalten Attribute Kanten z. B. die *linken*, *rechten*, *oben*, und *unten* eines Elements. Dazu gehören auch Attribute wie z. B. *Höhe* und *Breite*, und zeigen Sie den Speicherort, Center *CenterX* und *CenterY*. Wenn es eine Einschränkung für die Position der linken Begrenzung von zwei Schaltflächen hinzufügen, wird der Designer beispielsweise den folgenden Code im Hintergrund generieren:

```csharp
View.AddConstraint (NSLayoutConstraint.Create (Button1, NSLayoutAttribute.Left, NSLayoutRelation.Equal, Button2, NSLayoutAttribute.Left, 1, 10));
```

Im nächste Abschnitt wird das Arbeiten mit Einschränkungen, die mit dem iOS-Designer, einschließlich aktivieren und deaktivieren Automatisches Layout und die Verwendung der Symbolleiste für Einschränkungen behandelt.

## <a name="enable-auto-layout"></a>Automatisches Layout aktivieren

Die Standardkonfiguration für iOS-Designer hat die Einschränkung-Modus aktiviert ist. Allerdings müssen Sie zum Aktivieren oder deaktivieren sie manuell, können Sie dies tun, in zwei Schritten:

1.  Klicken Sie auf einen leeren Bereich auf der Entwurfsoberfläche angezeigt. Dies hebt alle Elemente auf und werden die Eigenschaften für das Storyboard-Dokument.
1.  Aktivieren oder deaktivieren Sie die **verwenden Autolayout** Kontrollkästchen im Eigenschaftenbereich:

    ![](designer-auto-layout-images/image01.png "Das verwenden Autolayout-Kontrollkästchen im Eigenschaftenbereich")


Standardmäßig sind keine Einschränkungen erstellt oder auf der Oberfläche angezeigt. Stattdessen werden sie automatisch aus den Frame-Informationen zur Kompilierzeit abgeleitet. Wenn Einschränkungen hinzufügen möchten, müssen wir wählen Sie ein Element auf der Entwurfsoberfläche aus, und fügen Sie Einschränkungen hinzu. Dies ist möglich, dass die Verwendung der **Einschränkung Symbolleiste**.

## <a name="constraints-toolbar"></a>Symbolleiste für Einschränkungen

 [![](designer-auto-layout-images/toolbarnew.png "Die Kontextmenübefehle")](designer-auto-layout-images/toolbarnew.png#lightbox)

Die Symbolleiste für Einschränkungen aktualisiert wurde, und es besteht nun aus zwei Hauptkomponenten:

- **Eine Schaltfläche Umschalten Einschränkungen**: Bisher haben Sie durch erneutes Klicken auf eine ausgewählte Ansicht auf der Entwurfsoberfläche der einschränkungsmodus eingegeben. Sie sollten jetzt in der Leiste Einschränkungen diese Umschaltfläche verwenden:

  ![Einschränkungen Modi umschalten](designer-auto-layout-images/constraints.png)

- **Eine Schaltfläche "Update-Einschränkungen":** Es ist wichtig zu beachten, dass die Änderungen, je nachdem, ob Einschränkungen, die im Bearbeitungsmodus ist.
  - Im Bearbeitungsmodus Einschränkung wird diese Schaltfläche, die Einschränkungen den Element-Frame entsprechend angepasst.
  - Im Bearbeitungsmodus Frame passt diese Schaltfläche, die Element-Frame entsprechend die Position, die die Einschränkungen definieren.


## <a name="surface-based-constraint-editing"></a>Surface-basierte Einschränkung bearbeiten

Im vorherigen Abschnitt haben Sie erfahren, Default-Einschränkungen hinzufügen und Entfernen von Einschränkungen mithilfe der Symbolleiste für Einschränkungen. Für weitere Berechtigungsumfang Einschränkung bearbeiten, können wir mit Einschränkungen auf der Entwurfsoberfläche direkt interagieren. Dieser Abschnitt enthält die Grundlagen Oberfläche basierende Einschränkung zu bearbeiten, einschließlich der Pin-Abstand Steuerelemente-Ablagebereiche, und Arbeiten mit verschiedenen Typen von Einschränkungen.

### <a name="creating-constraints"></a>Erstellen von Einschränkungen

Das iOS-Designer-Tool bietet zwei Arten von Steuerelementen zum Bearbeiten von Elementen auf der Entwurfsoberfläche angezeigt. *Ziehen von Steuerelementen* und *Pin Zwischenraum Steuerelemente*, wie in der folgenden Abbildung dargestellt:

![-Steuerelemente](designer-auto-layout-images/controls.png)

Diese werden ein-/ausgeschaltet, durch die Schaltfläche "Einschränkungen" in der Leiste Einschränkungen auswählen.

Definieren Sie die Steuerpunkte der 4 T gestalteten auf jeder Seite des Elements der *oben*, *rechten*, *unten*, und *linken* Kanten des Elements für eine Einschränkung. Definieren Sie die beiden ich geformten Handles am rechten und unteren Rand des Elements *Höhe* und *Breite* Einschränkung bzw. Das Quadrat mittlere behandelt sowohl *CenterX* und *CenterY* Einschränkungen.

Um eine Einschränkung zu erstellen, wählen Sie ein Handle, und ziehen Sie es an einer beliebigen Stelle auf der Entwurfsoberfläche angezeigt. Beim Starten des Ziehvorgangs wird eine Reihe von grünen Linien/Felder angezeigt, auf der Oberfläche, die mitzuteilen, was Sie einschränken können. Im folgenden Screenshot sind wir z. B. den oberen Rand die mittlere Schaltfläche einschränken:

 [![](designer-auto-layout-images/image07.png "Einschränken der Oberseite der mittleren Taste")](designer-auto-layout-images/image07.png#lightbox)

Beachten Sie die drei gestrichelten grünen Linien für die anderen zwei Schaltflächen an. Die grünen Linien geben *ablegebereiche*, oder die Attribute anderer Elemente, die auf die wir einschränken können. Im obigen Screenshot, bieten die anderen zwei Schaltflächen 3 vertikale ablegebereiche ( *unten*, *CenterY*, *oben*) auf die Schaltfläche zu beschränken. Die gestrichelte grüne Linie am oberen Rand der Ansicht bedeutet, dass der ansichtscontroller stellt eine Einschränkung am oberen Rand der Ansicht und das durchgehende grüne Kästchen bedeutet, dass der ansichtscontroller eine Einschränkung unter die obere Layoutführungslinie bietet.

> [!IMPORTANT]
> Layoutführungslinien sind bestimmte Arten von Einschränkung-Ziele, die wir oben erstellen können und nach unten-Einschränkungen, die das Vorhandensein von System Balken, z. B. Statusleisten oder Symbolleisten berücksichtigen. Eine der wichtigsten Einsatzzwecke ist verfügen über eine app, die zwischen iOS 6 und iOS 7 kompatibel, da die neueste Version die Containeransicht unterhalb der Statusleiste verfügt. Weitere Informationen zu die obere Layoutführungslinie, finden Sie in der [Apple-Dokumentation](https://developer.apple.com/library/ios/documentation/userexperience/conceptual/transitionguide/AppearanceCustomization.html#//apple_ref/doc/uid/TP40013174-CH15-SW2).



In den nächsten drei Abschnitten stellen die Arbeit mit verschiedenen Typen von Einschränkungen vor.

### <a name="size-constraints"></a>Einschränkungen der Datenkapazität

Größenbeschränkungen - *Höhe* und *Breite* -haben Sie zwei Möglichkeiten. Die erste Option ist am Ziehpunkt, um auf eine benachbarte Element-Größe beschränken ziehen, wie im obigen Beispiel gezeigt. Die andere Option ist auf den Ziehpunkt, um eine Self-Einschränkung erstellen. Dadurch können wir keinen Wert Konstanten Größe angeben, wie im folgenden Screenshot dargestellt:

 [![](designer-auto-layout-images/sizec.png "Ziehen Sie den Ziehpunkt, um auf eine benachbarte Element-Größe beschränken, wie hier gezeigt")](designer-auto-layout-images/sizec.png#lightbox)

### <a name="center-constraints"></a>Center-Einschränkungen

Die quadratischen Handles erstellt eine *CenterX* oder *CenterY* -Einschränkung, je nach Kontext. Ziehen die quadratischen Handles leuchtet nach den anderen Elementen beide Dropbereiche vertikale und horizontale &-zu bieten, wie im folgenden Screenshot dargestellt:

 [![](designer-auto-layout-images/centerc.png "Center-Einschränkungen")](designer-auto-layout-images/centerc.png#lightbox)

Bei Auswahl einer vertikalen Ablagebereich, eine *CenterY* Einschränkung erstellt werden. Wenn Sie eine horizontale Ablegebereich auswählen, wird die Einschränkung basierend auf *CenterX*.

### <a name="combinational-constraints"></a>Combinational Einschränkungen

Um sowohl Ausrichtung und größenbeschränkungen für Gleichheit zwischen zwei Elementen zu erstellen, können Sie Elemente aus einer oberen Symbolleiste an – in der Reihenfolge - horizontale Ausrichtung, vertikaler Ausrichtung und Größe Gleichheitsprädikate, auswählen, wie im folgenden Screenshot dargestellt:

 [![](designer-auto-layout-images/image06.png "Combinational Einschränkungen")](designer-auto-layout-images/image06.png#lightbox)

### <a name="visualizing-and-editing-constraints"></a>Visualisieren und Bearbeiten von Einschränkungen

Wenn Sie eine Einschränkung hinzufügen, wird es angezeigt, auf der Entwurfsoberfläche als eine blaue Linie Wenn Sie ein Element auswählen:

 [![](designer-auto-layout-images/image09.png "Visualisieren von Einschränkungen")](designer-auto-layout-images/image09.png#lightbox)

Sie können eine Einschränkung auswählen, indem Sie durch Klicken auf eine blaue Linie, und bearbeiten die Einschränkungswerte direkt in den Eigenschaftenbereich. Alternativ wird durch Doppelklicken auf eine blaue Linie ein Popover angezeigt, die die Werte direkt auf der Entwurfsoberfläche bearbeitet werden kann:

 [![](designer-auto-layout-images/image08.png "Bearbeiten von Einschränkungen")](designer-auto-layout-images/image08.png#lightbox)

## <a name="constraint-issues"></a>Einschränkung Probleme

Verschiedene Arten von Problemen können auftreten, wenn Sie Einschränkungen verwenden:

-  **In Konflikt stehenden Einschränkungen** – dies geschieht, wenn mehrere Einschränkungen erzwingen, dass das Element in Konflikt stehende Werte für ein Attribut verfügt, und die Einschränkung-Engine nicht möglich ist, Konflikte zu lösen.
-  **Elemente unterbestimmt** – eines Elements-Eigenschaften ("Speicherort" + "Größe") müssen durch einen Satz von Einschränkungen und systeminterne Größen für die Einschränkungen gültig ist vollständig abgedeckt werden. Wenn diese Werte nicht eindeutig sind, wird das Element als unterbestimmt bezeichnet.
-  **Frame-Verlust** – dieses Problem tritt bei eines Elements Rahmen und einen Satz von Einschränkungen zwei verschiedene resultierende Rechtecke definieren.


In diesem Abschnitt werden auf die drei oben aufgeführten Probleme, und enthält Details zu deren Behandlung.

### <a name="conflicting-constraints"></a>In Konflikt stehenden Einschränkungen

In Konflikt stehenden Einschränkungen sind rot markiert und haben ein Warnsymbol angezeigt wird. Mit dem Mauszeiger auf die Symbole für die Warnung wird ein Popover mit Informationen über den Konflikt:

 [![](designer-auto-layout-images/image11.png "In Konflikt stehenden Einschränkungen Warnung")](designer-auto-layout-images/image11.png#lightbox)

### <a name="underconstrained-items"></a>Unterbestimmt Elemente

Unterbestimmt Elemente sind Orange hervorgehoben, und lösen die Darstellung eines Symbols orangefarbenen Marker in der Ansichtsleiste des Controller-Objekt:

 [![](designer-auto-layout-images/image02.png "Unterbestimmt Elemente sind Orange hervorgehoben")](designer-auto-layout-images/image02.png#lightbox)

Wenn Sie auf das Markensymbol klicken, können Sie abrufen von Informationen zu unterbestimmt Elemente in der Szene und Lösen der Probleme entweder vollständig Einschränken von ihnen oder durch das Entfernen von deren Einschränkungen, wie im folgenden Screenshot dargestellt:

 [![](designer-auto-layout-images/image10.png "Unterbestimmt Elemente korrigiert")](designer-auto-layout-images/image10.png#lightbox)

### <a name="frame-misplacement"></a>Frame-Verlust

Frame-Verlust wird den gleiche Code für die Farbe als unterbestimmt Elemente verwendet. Das Element wird immer auf der Oberfläche, die mithilfe der systemeigenen Rahmen gerendert, aber bei einem Verlust der Frame ein rotes Rechteck kennzeichnet, in dem das Element am Ende wird beim Ausführen der Anwendung, wie im folgenden Screenshot dargestellt:

 [![](designer-auto-layout-images/image05.png "Beispielansicht für die Frame-Verlust")](designer-auto-layout-images/image05.png#lightbox)

Wählen Sie zum Beheben von Fehlern der Frame-Verlust der **Update Frames auf Einschränkungen basieren** Schaltfläche der Symbolleiste für Einschränkungen (ganz rechts-Taste):

 [![](designer-auto-layout-images/image03.png "Aktualisieren des Frames, die basierend auf Einschränkungen Symbolleisten-Schaltfläche")](designer-auto-layout-images/image03.png#lightbox)

Dies passt sich automatisch auf den Element-Frame entsprechend die von den Steuerelementen definierten Positionen aus.

<a name="modifying-in-code" />

## <a name="modifying-constraints-in-code"></a>Ändern von Einschränkungen in Code

Je nach den Anforderungen Ihrer App möglicherweise vorkommen, dass Sie benötigen eine Einschränkung im Code zu ändern. Beispielsweise zum Ändern der Größe oder Position der Sicht ist eine Einschränkung, angefügt eine Einschränkung für die Priorität ändern oder eine Einschränkung vollständig zu deaktivieren.

Für den Zugriff auf eine Einschränkung in Code, müssen Sie zuerst es im iOS-Designer verfügbar machen, indem Sie wie folgt vorgehen:

1. Erstellen Sie die Einschränkung wie gewohnt (mithilfe einer der oben aufgeführten Methoden).
2. In der **Gliederung-Dokument-Explorer**, suchen Sie die gewünschte Einschränkung, und wählen Sie ihn:

    [![](designer-auto-layout-images/modify01.png "Der Dokument-Explorer Kontur")](designer-auto-layout-images/modify01.png#lightbox)
3. Als Nächstes weisen eine **Namen** für die Einschränkung in der **Widget** Registerkarte die **Eigenschaften-Explorer**:

    [![](designer-auto-layout-images/modify02.png "Die Registerkarte \"Widget\"")](designer-auto-layout-images/modify02.png#lightbox)
4. Speichern Sie die Änderungen.

Mit den oben genannten Änderungen vorhanden können Sie Zugriff auf die Einschränkung im Code und ihre Eigenschaften ändern. Beispielsweise können Sie Folgendes, um die Höhe der angefügten Ansicht auf NULL festzulegen:

```csharp
ViewInfoHeight.Constant = 0;
```

Betrachten Sie die folgende Einstellung für die Einschränkung in der iOS-Designer:

[![](designer-auto-layout-images/modify03.png "Bearbeiten eine Einschränkung in den Eigenschaften-Explorer")](designer-auto-layout-images/modify03.png#lightbox)

### <a name="the-deferred-layout-pass"></a>Der verzögerte Layoutdurchlauf

Plant die anstelle der angefügten Ansicht als Reaktion auf Änderungen der Einschränkung wird sofort aktualisiert, die Auto-Layout-Engine eine _verzögerten Layoutdurchlauf_ für die nahe Zukunft. Während dieses Durchgangs verzögerte ist die angegebene Ansicht Einschränkung aktualisiert, die Einschränkungen nicht nur über für jede Ansicht in der Hierarchie neu berechnet werden, und aktualisiert, um für das neue Layout passen.

Sie können Ihre eigenen verzögerten Layoutdurchlauf zu jedem Zeitpunkt planen, durch Aufrufen der `SetNeedsLayout` oder `SetNeedsUpdateConstraints` Methoden der übergeordneten Ansicht. 

Der Layoutdurchlauf verzögerten besteht aus zwei eindeutige durchläuft die Hierarchie von Inhaltsansichten:

- **Der Update-Pass** – In diesem Schritt, der Auto-Layout-Engine durchsucht die Hierarchie von Inhaltsansichten und ruft die `UpdateViewConstraints` Methode für alle View-Controller und die `UpdateConstraints` Methode auf alle Sichten.
- **Der Layoutdurchlauf** – in diesem Fall die Auto-Layout-Engine durchsucht die Hierarchie von Inhaltsansichten, aber dieses Mal ruft der `ViewWillLayoutSubviews` Methode für alle View-Controller und die `LayoutSubviews` Methode für alle Ansichten. Die `LayoutSubviews` -Methode aktualisiert die `Frame` Eigenschaft der einzelnen Unteransicht mit dem Rechteck von der Layout-Engine automatisch berechnet.

### <a name="animating-constraint-changes"></a>Animieren von Änderungen der Einschränkung

Zusätzlich zum Ändern von Eigenschaften, können Sie Core Animation Animieren von Änderungen an eine Ansicht der Einschränkungen. Zum Beispiel:

```csharp
UIView.BeginAnimations("OpenInfo");
UIView.SetAnimationDuration(1.0f);
ViewInfoHeight.Constant = 237;
View.LayoutIfNeeded();

//Execute Animation
UIView.CommitAnimations();
```

Der Schlüssel ist der Aufruf der `LayoutIfNeeded` Methode der übergeordneten Ansicht innerhalb der Animation-Block. Dies teilt die Ansicht zum Zeichnen der einzelnen "Frame" von der animierten Speicherort oder die Änderung der Dateigröße. Ohne diese Zeile wird die Ansicht einfach zur abschließenden Version ohne Animation ausrichten.

## <a name="summary"></a>Zusammenfassung

In diesem Leitfaden wurden automatisch des iOS (oder "adaptive") Layout und das Konzept der Einschränkungen als mathematische Darstellung der Beziehungen zwischen den Elementen auf der Entwurfsoberfläche angezeigt. Es wurde beschrieben, wie Automatisches Layout in der iOS-Designer, und Arbeiten mit aktivieren die **Symbolleiste für Einschränkungen**, und Bearbeiten von Einschränkungen einzeln auf der Entwurfsoberfläche angezeigt. Als Nächstes wurde es erklärt, wie Sie drei allgemeine Einschränkungen Probleme behandeln. Zum Schluss wird aufgezeigt, wie Einschränkungen im Code geändert.

## <a name="related-links"></a>Verwandte Links

- [Einführung in Storyboards](~/ios/user-interface/storyboards/index.md)
- [iOS entworfen Steuerelemente Exemplarische Vorgehensweise](~/ios/user-interface/designer/ios-designable-controls-walkthrough.md)
- [Übersicht über Android Designer](~/android/user-interface/android-designer/index.md)
- [Programmgesteuerte Einschränkungen](~/ios/user-interface/programmatic-layout-constraints.md)
- [Apple – automatische Layoutführungslinie](https://developer.apple.com/library/ios/documentation/userexperience/conceptual/AutolayoutPG/Introduction/Introduction.html#/apple_ref/doc/uid/TP40010853-CH13-SW1)
