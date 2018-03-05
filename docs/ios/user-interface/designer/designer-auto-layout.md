---
title: "Automatisches Layout mit dem Xamarin-Designer für iOS"
description: "Dieses Handbuch enthält iOS Automatisches Layout und den neuen Einschränkungen Workflow in die Xamarin-Designer für iOS verfügbar."
ms.topic: article
ms.prod: xamarin
ms.assetid: CAC7A715-55BB-45E2-BB6D-2168D36D428F
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 040a5979339ed12f212f932f3b7e51cf48a9d382
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="auto-layout-with-the-xamarin-designer-for-ios"></a>Automatisches Layout mit dem Xamarin-Designer für iOS

_Dieses Handbuch enthält iOS Automatisches Layout und den neuen Einschränkungen Workflow in die Xamarin-Designer für iOS verfügbar._

Automatisches Layout (auch als "adaptive Layout" bezeichnet) ist ein reaktionsfähiges Design-Ansatz. Im Gegensatz zu den transitional Layoutsystem, in dem jedes Element Speicherort hartcodiert bis zu einem Zeitpunkt auf dem Bildschirm ist, automatisches Layout geht *Beziehungen* -die Positionen von Elementen relativ zu anderen Elementen auf der Entwurfsoberfläche angezeigt. Das Herzstück des Automatisches Layout ist die Einschränkungen und Regeln, die die Platzierung eines Elements oder einer Gruppe von Elementen im Kontext von anderen Elementen auf dem Bildschirm zu definieren. Da die Elemente nicht an einer bestimmten Position auf dem Bildschirm gebunden sind, können Einschränkungen eine adaptive Layout zu erstellen, die für verschiedene Bildschirmgrößen und Gerät Ausrichtungen gut aussieht.

In diesem Handbuch werden Einschränkungen und zum Arbeiten mit ihnen in der Xamarin iOS-Designer eingeführt. Programmgesteuertes Arbeiten mit Einschränkungen wird von diesem Handbuch nicht behandelt. Informationen zum Verwenden von Auto-Layout programmgesteuert finden Sie unter der der [Apple-Dokumentation](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/AutolayoutPG/ProgrammaticallyCreatingConstraints.html).

## <a name="requirements"></a>Anforderungen

Die Xamarin-Designer für iOS ist in Visual Studio für Mac in Visual Studio 2015 und 2017 unter Windows verfügbar.

Dieses Handbuch setzt Kenntnisse der Designer-Komponenten aus der [Einführung in die iOS-Designer](~/ios/user-interface/designer/introduction.md) Handbuch.

## <a name="introduction-to-constraints"></a>Einführung in die Einschränkungen

Eine Einschränkung ist eine mathematische Darstellung der Beziehung zwischen zwei Elementen auf dem Bildschirm. Eine Benutzeroberfläche darstellt löst die Position des Elements als eine mathematische Beziehung mehrere Probleme im Zusammenhang mit Hardcodieren ein Benutzeroberflächenelement Position. Beispielsweise würden wir eine Schaltfläche 20px vom unteren Rand des Bildschirms im Hochformat platzieren, wäre die Position der Schaltfläche außerhalb des Bildschirms im Querformat. Um dies zu vermeiden, konnten wir eine Einschränkung festlegen, die den unteren Rand der Schaltfläche 20px vom unteren Rand der Ansicht positioniert. Die Position der Schaltfläche Edge würde dann berechnet als *button.bottom = view.bottom - 20px*, würde die Hochformat- und querformatausrichtung Modus der Schaltfläche 20px vom unteren Rand der Ansicht versehen. Die Möglichkeit zum Berechnen der Platzierung basierend auf eine mathematische Beziehung ist, was Einschränkungen im UI-Entwurf daher macht.

Wenn es eine Einschränkung festgelegt haben, erstellen wir eine `NSLayoutConstraint` Objekt, das als Argumente verwendet werden sollen, die Objekte eingeschränkt wird und die Eigenschaften oder *Attribute*, die die Einschränkung auf fungiert. In der iOS-Designer enthalten Attribute Kanten z. B. die *linken*, *rechten*, *oben*, und *unteren* eines Elements. Dazu zählen auch Größenattribute wie z. B. *Höhe* und *Breite*, und der Mittelpunkt Speicherort *CenterX* und *CenterY*. Wenn es eine Einschränkung für die Position der linken Begrenzung der zwei Schaltflächen hinzufügen, wird der Designer z. B. den folgenden Code im Hintergrund generiert:

```csharp
View.AddConstraint (NSLayoutConstraint.Create (Button1, NSLayoutAttribute.Left, NSLayoutRelation.Equal, Button2, NSLayoutAttribute.Left, 1, 10));
```

Im nächste Abschnitt wird das Arbeiten mit Einschränkungen mit der iOS-Designer, einschließlich aktivieren und deaktivieren Automatisches Layout und über diese Symbolleiste Einschränkungen behandelt.

## <a name="enable-auto-layout"></a>Automatisches Layout aktivieren

Die Standardkonfiguration für iOS-Designer hat Einschränkung-Modus aktiviert ist. Allerdings müssen Sie Sie aktivieren oder deaktivieren es manuell, können Sie dies tun, in zwei Schritten:

1.  Klicken Sie auf einen leeren Bereich auf der Entwurfsoberfläche angezeigt. Dies hebt alle Elemente und die Eigenschaften für das Storyboard-Dokument wird geöffnet.
1.  Aktivieren oder deaktivieren Sie die **verwenden Autolayout** CheckBox-Element im Eigenschaftenbereich:

    ![](designer-auto-layout-images/image01.png "Das Kontrollkästchen verwenden Autolayout im Eigenschaftenbereich")


Standardmäßig sind keine Einschränkungen auf der Oberfläche angezeigt oder erstellt. Stattdessen werden sie automatisch die Frame-Informationen zum Zeitpunkt der Kompilierung abgeleitet. Wenn Einschränkungen hinzufügen möchten, müssen wir wählen Sie ein Element auf der Entwurfsoberfläche und -Einschränkungen hinzugefügt. Es möglich, dass die Verwendung der **Einschränkung Symbolleiste**.

## <a name="constraints-toolbar"></a>Einschränkungen-Symbolleiste

 [ ![](designer-auto-layout-images/toolbarnew.png "Die Kontextmenübefehle")](designer-auto-layout-images/toolbarnew.png)

Die Einschränkungen-Symbolleiste aktualisiert wurde und jetzt besteht aus zwei Hauptkomponenten:

- **Eine Schaltfläche Umschalten Einschränkungen**: zuvor eingegebene Einschränkungen-Modus durch erneutes Klicken auf eine ausgewählte Ansicht auf der Entwurfsoberfläche angezeigt. Sie sollten diese Schaltfläche zum ein-/ausschalten jetzt in der Leiste Einschränkungen verwenden:

  ![der Aufrufattribute Modi zum ein-/ausschalten](designer-auto-layout-images/constraints.png)

- **Eine Schaltfläche "Einschränkungen":** es ist wichtig zu beachten, dass die Änderungen, je nachdem, ob Sie im Bearbeitungsmodus Einschränkungen sind.
  - Im Bearbeitungsmodus Einschränkung wird diese Schaltfläche, die Einschränkungen den Element-Frame entsprechend angepasst.
  - Im Bearbeitungsmodus Frame passt diese Schaltfläche den Element-Frame entsprechend die Position, die die Einschränkungen definieren.


## <a name="surface-based-constraint-editing"></a>Oberfläche basierende Einschränkung bearbeiten

Im vorherigen Abschnitt zum Hinzufügen von Default-Einschränkungen und Entfernen von Einschränkungen mithilfe der Symbolleiste Einschränkungen gelernt. Für weitere Berechtigungsumfang Einschränkung bearbeiten, können wir mit Einschränkungen auf der Entwurfsoberfläche direkt interagieren. In diesem Abschnitt werden die Grundlagen der Oberfläche basierende Einschränkung zu bearbeiten, einschließlich Pin Zwischenraum-Steuerelemente, ablegebereiche und Arbeiten mit verschiedenen Typen von Einschränkungen.

### <a name="creating-constraints"></a>Erstellen von Einschränkungen

Das Designer-Tool für iOS bietet zwei Arten von Steuerelementen zum Bearbeiten von Elementen auf der Entwurfsoberfläche angezeigt. *Ziehen von Steuerelementen* und *Pin Zwischenraum Steuerelemente*, wie in der folgenden Abbildung dargestellt:

![-Steuerelemente](designer-auto-layout-images/controls.png)

Diese werden ein-/ausgeschaltet, durch Auswählen der Schaltfläche mit den Einschränkungen Modus in der Leiste Einschränkungen.

4 T geformten Handles auf jeder Seite des Elements definieren die *oben*, *rechten*, *unteren*, und *linken* Kanten des Elements für eine Einschränkung. Definieren Sie die beiden ich geformten Handles am rechten und unteren Rand des Elements *Höhe* und *Breite* Einschränkung bzw. Der mittlere quadratische handles beide *CenterX* und *CenterY* Einschränkungen.

Um eine Einschränkung erstellen, wählen Sie ein Handle, und ziehen Sie es an einer beliebigen Stelle auf der Entwurfsoberfläche angezeigt. Beim Starten des Ziehvorgangs erscheint auf der Oberfläche, die mitzuteilen, was eine Reihe von grünen Zeilen/Felder können eingeschränkt werden. Im folgenden Screenshot sind wir z. B. den oberen Rand die mittlere Schaltfläche einschränken:

 [ ![](designer-auto-layout-images/image07.png "Einschränken der Oberseite der mittleren")](designer-auto-layout-images/image07.png)

Beachten Sie die drei gestrichelten grünen Linien für die anderen zwei Schaltflächen. Die grünen Linien geben *löschen Bereiche*, oder die Attribute aus anderen Elementen, wir eingeschränkt werden können. In der oben dargestellten Screenshot bieten die anderen zwei Schaltflächen 3 vertikale ablegebereiche ( *unteren*, *CenterY*, *oben*), die Schaltfläche zu beschränken. Die gestrichelte grüne Linie am oberen Rand der Ansicht bedeutet, dass der Controller Ansicht bietet eine Einschränkung am oberen Rand der Ansicht und durchgehende grünen Kasten bedeutet, dass die View-Controller eine Einschränkung unterhalb der obersten Layout Handbuch bietet.

> [!IMPORTANT]
> **Hinweis**: Layout Handbücher sind spezielle Arten von Einschränkung-Ziele, die können wir die oberen und unteren Einschränkungen erstellen, die das Vorhandensein von System Balken, z. B. Statusleisten oder Symbolleisten berücksichtigen. Einer der wichtigsten ist eine app, die zwischen iOS 6 und iOS 7 kompatibel, da die aktuelle Version der Containeransicht unterhalb der Statusleiste verfügt. Weitere Informationen zu den oberen Layout-Handbuch, finden Sie in der [Apple-Dokumentation](https://developer.apple.com/library/ios/documentation/userexperience/conceptual/transitionguide/AppearanceCustomization.html#//apple_ref/doc/uid/TP40013174-CH15-SW2).



In den nächsten drei Abschnitten einführen arbeiten mit verschiedenen Typen von Einschränkungen.

### <a name="size-constraints"></a>Einschränkungen der Datenkapazität

Mit Einschränkungen der Datenkapazität - *Höhe* und *Breite* -Ihnen zwei Optionen zur Verfügung. Die erste Möglichkeit besteht, am Ziehpunkt, um auf eine Nachbar-Element-Größe eingeschränkt zu ziehen, wie im obigen Beispiel gezeigt. Die andere Option ist am Ziehpunkt, um eine Self-Einschränkung erstellen doppelklicken. Dadurch können wir einen Konstante Größenwert angeben, wie der folgende Screenshot veranschaulicht:

 [ ![](designer-auto-layout-images/sizec.png "Ziehen Sie am Ziehpunkt, um auf eine Größe Nachbar-Element zu beschränken, wie hier dargestellt")](designer-auto-layout-images/sizec.png)

### <a name="center-constraints"></a>Center-Einschränkungen

Die quadratische Handles erstellt eine *CenterX* oder *CenterY* Einschränkung, je nach Kontext. Ziehen die quadratische Handles leuchtet die anderen Elemente sowohl vertikale und horizontale ablegebereiche anbieten, wie der folgende Screenshot veranschaulicht:

 [ ![](designer-auto-layout-images/centerc.png "Center-Einschränkungen")](designer-auto-layout-images/centerc.png)

Eine vertikale Ablegebereich auf Wunsch eine *CenterY* Einschränkung erstellt werden. Wenn Sie eine horizontale Ablegebereich auswählen, wird die Einschränkung basierend auf *CenterX*.

### <a name="combinational-constraints"></a>Combinational Einschränkungen

Um Ausrichtung und Einschränkungen der Datenkapazität Gleichheit zwischen zwei Elementen zu erstellen, können Sie Elemente aus einer oben in der Symbolleiste an – in der Reihenfolge - horizontale Ausrichtung, die vertikale Ausrichtung und die Größe Gleichheitsprädikate, auswählen, wie der folgende Screenshot veranschaulicht:

 [ ![](designer-auto-layout-images/image06.png "Combinational Einschränkungen")](designer-auto-layout-images/image06.png)

### <a name="visualizing-and-editing-constraints"></a>Visualisieren und Bearbeiten von Einschränkungen

Wenn Sie eine Einschränkung hinzufügen, wird es angezeigt auf der Entwurfsoberfläche als blaue Linie dargestellt, wenn Sie ein Element auswählen:

 [ ![](designer-auto-layout-images/image09.png "Visualisieren von Einschränkungen")](designer-auto-layout-images/image09.png)

Sie können eine Einschränkung auswählen, indem Sie durch Klicken auf eine blaue Linie und die Einschränkungswerte direkt im Eigenschaftenbereich bearbeiten. Alternativ wird durch Doppelklicken auf eine blaue Linie eine Popover angezeigt, in dem Sie die Werte direkt auf der Entwurfsoberfläche bearbeitet werden können:

 [ ![](designer-auto-layout-images/image08.png "Bearbeiten von Einschränkungen")](designer-auto-layout-images/image08.png)

## <a name="constraint-issues"></a>Einschränkung Probleme

Verschiedene Arten von Problemen können Einschränkungen auftreten:

-  **In Konflikt stehenden Einschränkungen** – dies geschieht, wenn mehrere Einschränkungen erzwingen, das Element in Konflikt stehenden Werte für ein Attribut verfügt, und das Modul für die Einschränkung kann nicht auf sie abstimmen.
-  **Elemente unterbestimmt** – Eigenschaften eines Elements, (Speicherort + Größe) müssen vollständig abgedeckt werden, durch einen Satz von Einschränkungen und systeminterne Größen für die Einschränkungen gültig ist. Wenn diese Werte nicht eindeutig sind, wird das Element als unterbestimmt bezeichnet.
-  **Frame-Verlust** – dies geschieht, wenn ein Element Rahmen und einen Satz von Einschränkungen zwei verschiedene resultierende Rechtecke definieren.


In diesem Abschnitt werden die drei oben genannten Probleme näher erläutert und enthält Details zu deren Behandlung.

### <a name="conflicting-constraints"></a>In Konflikt stehenden Einschränkungen

In Konflikt stehenden Einschränkungen in rot gekennzeichnet sind und ein Warnsymbol haben. Mit dem Mauszeiger auf die Symbole für die Warnung wird eine Popover mit Informationen über den Konflikt geöffnet:

 [ ![](designer-auto-layout-images/image11.png "In Konflikt stehenden Einschränkungen Warnung")](designer-auto-layout-images/image11.png)

### <a name="underconstrained-items"></a>Unterbestimmt Elemente

Unterbestimmt Elemente sind Orange hervorgehoben und die Darstellung eines Symbols in der Ansicht Controller Objektleiste orange Marker auslösen:

 [ ![](designer-auto-layout-images/image02.png "Unterbestimmt Elemente sind Orange hervorgehoben")](designer-auto-layout-images/image02.png)

Wenn Sie auf dieses Symbol "Marker" klicken, können Sie abrufen von Informationen zu unterbestimmt Elemente in der Szene und lösen die Probleme, indem Sie entweder vollständig typisiert werden oder deren Einschränkungen zu entfernen, wie der folgende Screenshot veranschaulicht:

 [ ![](designer-auto-layout-images/image10.png "Beheben von unterbestimmt Elemente")](designer-auto-layout-images/image10.png)

### <a name="frame-misplacement"></a>Frame-Verlust

Frame-Verlust wird den gleiche Code für die Farbe als unterbestimmt Elemente verwendet. Das Element wird immer auf die Oberfläche, die unter Verwendung der systemeigenen Rahmen gerendert werden, aber im Falle einer Frame Verlust wird ein rotes Rechteck kennzeichnen, in dem das Element am Ende wird beim Ausführen der Anwendung, wie der folgende Screenshot veranschaulicht:

 [ ![](designer-auto-layout-images/image05.png "Beispielansicht der Frame-Verlust")](designer-auto-layout-images/image05.png)

Um Frames Verlust Fehler zu beheben, wählen Sie die **Update Frames basierend auf Einschränkungen** Schaltfläche auf der Symbolleiste Einschränkungen (Schaltfläche ganz rechts):

 [ ![](designer-auto-layout-images/image03.png "Aktualisieren von Frames, die basierend auf Einschränkungen Symbolleisten-Schaltfläche")](designer-auto-layout-images/image03.png)

Dadurch wird den Element-Frame entsprechend die von den Steuerelementen definierten Positionen automatisch angepasst.

<a name="modifying-in-code" />

## <a name="modifying-constraints-in-code"></a>Ändern von Einschränkungen in Code

Basierend auf den Anforderungen der app, gibt es möglicherweise vorkommen, dass Sie eine Einschränkung im Code ändern müssen. Beispielsweise ist zum Ändern der Größe oder Position der Ansicht eine Einschränkung zugeordnet zum Ändern der Priorität einer Einschränkung oder eine Einschränkung zu deaktivieren.

Für den Zugriff auf eine Einschränkung in Code, müssen Sie zuerst sie in der iOS-Designer verfügbar machen, wie folgt:

1. Erstellen Sie die Einschränkung als normale (mithilfe einer der oben aufgeführten Methoden).
2. In der **Document Gliederung Explorer**, suchen Sie die gewünschte Einschränkung, und wählen Sie ihn:

    [ ![](designer-auto-layout-images/modify01.png "Gliederung Document Explorer")](designer-auto-layout-images/modify01.png)
3. Als Nächstes weisen Sie einem **Namen** der Einschränkung in der **Widget** auf der Registerkarte die **Eigenschaften-Explorer**:

    [ ![](designer-auto-layout-images/modify02.png "Die Registerkarte "Widget"")](designer-auto-layout-images/modify02.png)
4. Speichern Sie die Änderungen.

Mit den oben beschriebenen Änderungen vorhanden können Sie die Einschränkung im Code zugreifen und ihre Eigenschaften ändern. Die folgenden können Sie z. B. die Höhe der angefügte Ansicht auf 0 (null) festgelegt:

```csharp
ViewInfoHeight.Constant = 0;
```

Betrachten Sie die folgende Einstellung für die Einschränkung in der iOS-Designer:

[ ![](designer-auto-layout-images/modify03.png "Bearbeiten eine Einschränkung in den Eigenschaften-Explorer")](designer-auto-layout-images/modify03.png)

### <a name="the-deferred-layout-pass"></a>Die verzögerte Layoutdurchlauf

Anstelle der angefügte Ansicht als Reaktion auf Änderungen der Einschränkung wird sofort aktualisiert, die automatische Layoutmodul plant eine _zurückgestellt Layoutdurchlauf_ für die naher Zukunft nicht mehr. Während dieses Durchgangs verzögerte ist nicht nur die angegebene Ansicht Einschränkung aktualisiert, die Einschränkungen für jede Ansicht in der Hierarchie werden neu berechnet und aktualisiert, um für das neue Layout angepasst werden müssen.

Sie können eigene zurückgestellt Layoutdurchlauf zu einem beliebigen Zeitpunkt planen, durch Aufrufen der `SetNeedsLayout` oder `SetNeedsUpdateConstraints` Methoden der übergeordneten Ansicht. 

Die verzögerte Layoutdurchlauf besteht aus zwei eindeutige durchläuft die Hierarchie anzeigen:

- **Die Aktualisierung erfolgreich** -In diesem Durchgang das automatische Layout-Modul durchsucht die Hierarchie anzeigen, und ruft die `UpdateViewConstraints` Methode auf alle View-Controller und die `UpdateConstraints` Methode auf alle Ansichten.
- **Die Layoutdurchlauf** – erneut, das automatische Layout-Modul durchsucht die Hierarchie anzeigen, aber dieses Mal ruft der `ViewWillLayoutSubviews` Methode auf alle View-Controller und die `LayoutSubviews` Methode auf alle Ansichten. Die `LayoutSubviews` Methode Updates der `Frame` Eigenschaft jeder Unteransicht mit dem Rechteck berechnet, indem das automatische Layout-Modul.

### <a name="animating-constraint-changes"></a>Animieren von Änderungen der Einschränkung

Core-Animation können Sie zusätzlich zum Ändern von Einschränkungseigenschaften, um Änderungen an einer Ansicht Einschränkungen zu animieren. Zum Beispiel:

```csharp
UIView.BeginAnimations("OpenInfo");
UIView.SetAnimationDuration(1.0f);
ViewInfoHeight.Constant = 237;
View.LayoutIfNeeded();

//Execute Animation
UIView.CommitAnimations();
```

Hier der Schlüssel ist das Aufrufen der `LayoutIfNeeded` Methode der übergeordneten Ansicht innerhalb der Animation-Block. Das weist die Sicht, jedes "Rahmen" der animierten Speicherort oder die Änderung der Textgröße gezeichnet werden soll. Ohne diese Zeile würde die Ansicht einfach an die endgültige Version ohne Animation ausgerichtet.

## <a name="summary"></a>Zusammenfassung

Dieses Handbuch eingeführt wurden, automatisch des iOS (oder "adaptive") Layout und das Konzept der Einschränkungen als mathematische Darstellungen von Beziehungen zwischen Elementen auf der Entwurfsoberfläche angezeigt. Er beschreibt, wie Sie im iOS-Designer arbeiten mit Automatisches Layout aktivieren die **Einschränkungen Symbolleiste**, und Bearbeiten von Einschränkungen einzeln auf der Entwurfsoberfläche angezeigt. Als Nächstes wurde es erläutert, wie drei allgemeine Einschränkungen Probleme beheben. Schließlich wurde es gezeigt, wie Einschränkungen im Code zu ändern.

## <a name="related-links"></a>Verwandte Links

- [Einführung in Storyboards](~/ios/user-interface/storyboards/index.md)
- [iOS entworfen Steuerelemente Exemplarische Vorgehensweise](~/ios/user-interface/designer/ios-designable-controls-walkthrough.md)
- [Übersicht über Android-Designer](~/android/user-interface/android-designer/index.md)
- [Programmgesteuerte Einschränkungen](~/ios/user-interface/programmatic-layout-constraints.md)
- [Apple - Auto-Layout-Handbuch](https://developer.apple.com/library/ios/documentation/userexperience/conceptual/AutolayoutPG/Introduction/Introduction.html#/apple_ref/doc/uid/TP40010853-CH13-SW1)
