---
title: iOS-Designer-Grundlagen
description: Dieser Leitfaden beschreibt die Xamarin-Designer für iOS. Es wird veranschaulicht, wie Sie iOS-Designer zu verwenden, um das visuelle Layout von Steuerelementen, wie Sie diese Steuerelemente im Code zugreifen und wie Eigenschaften bearbeitet.
ms.prod: xamarin
ms.assetid: E7045E41-0DEF-416B-BCDB-52502350F61C
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 01/31/2018
ms.openlocfilehash: e9c2a42b9108c04f18252a410d40dbc03013f6dd
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50123751"
---
# <a name="ios-designer-basics"></a>iOS-Designer-Grundlagen

_Dieser Leitfaden beschreibt die Xamarin-Designer für iOS. Es wird veranschaulicht, wie Sie iOS-Designer zu verwenden, um das visuelle Layout von Steuerelementen, wie Sie diese Steuerelemente im Code zugreifen und wie Eigenschaften bearbeitet._

Der Xamarin-Designer für iOS ist eine visuelle Schnittstelle ähnelt Interface Builder von Xcode und dem Android Designer. Einige seiner zahlreichen Features umfassen nahtlose Integration in Visual Studio für Mac und Visual Studio 2015 und 2017, Drag & Drop-Bearbeitung, eine Schnittstelle für das Einrichten von Ereignishandlern und die Möglichkeit, benutzerdefinierte Steuerelemente darzustellen.

## <a name="requirements"></a>Anforderungen

IOS ist Designer, die auf Windows in Visual Studio für Mac und Visual Studio 2015 und 2017 verfügbar. In Visual Studio 2015 oder 2017 erfordert der iOS-Designer eine Verbindung mit einem ordnungsgemäß konfigurierten Mac-buildhost, wenn Xcode nicht ausgeführt werden muss.

Dieses Handbuch setzt voraus, Vertrautheit mit dem Inhalt in behandelt die [Einstieg führt](~/ios/get-started/index.md).

<a name="how-it-works" />

## <a name="how-the-ios-designer-works"></a>Funktionsweise von iOS-Designer

In diesem Abschnitt wird beschrieben, wie der iOS-Designer erleichtert das Erstellen einer Benutzeroberfläche und mit Code verbinden.

Der iOS-Designer kann Entwickler Benutzeroberfläche einer Anwendung visuell entwerfen. Wie in der [Einführung in Storyboards](~/ios/user-interface/storyboards/index.md) Leitfaden beschreibt die Bildschirme (View Controller), aus denen eine app, die Elemente der Benutzeroberfläche (Ansichten) auf die View-Controller und die app Navigation Gesamtablauf platziert, ein Storyboard . 

Ein ansichtscontroller besteht aus zwei Teilen: eine visuelle Darstellung in der iOS-Designer und eine zugeordnete C#-Klasse:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

[![Einen ansichtscontroller im iOS-Designer](introduction-images/1-storyboardwithviewcontroller-vsmac.png "einen ansichtscontroller im iOS-Designer")](introduction-images/1-storyboardwithviewcontroller-vsmac-large.png#lightbox)

[![Der Code für einen ansichtscontroller](introduction-images/2-viewcontrollercode-vsmac.png "den Code für ein View-Controller")](introduction-images/2-viewcontrollercode-vsmac-large.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![Einen ansichtscontroller im iOS-Designer](introduction-images/1-storyboardwithviewcontroller-vs.png "einen ansichtscontroller im iOS-Designer")](introduction-images/1-storyboardwithviewcontroller-vs-large.png#lightbox)

[![Der Code für einen ansichtscontroller](introduction-images/2-viewcontrollercode-vs.png "den Code für ein View-Controller")](introduction-images/2-viewcontrollercode-vs-large.png#lightbox)

-----

Ein View Controller bereit keine Funktionalität, in seinem Standardzustand; Es muss mit Steuerelementen aktualisiert werden. Diese Steuerelemente werden in der Ansicht des ansichtscontrollers, der rechteckige Bereich platziert, die alle des Bildschirms Inhalte enthält. Die meisten View-Controller enthalten allgemeine Steuerelemente wie Schaltflächen, Bezeichnungen und Textfeldern, wie im folgenden Screenshot dargestellt einen ansichtscontroller, das eine Schaltfläche angezeigt: 

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

[![Einen ansichtscontroller, das eine Schaltfläche](introduction-images/3-viewcontrollerwithbutton-vsmac.png "einen View Controller-Schaltfläche enthält.")](introduction-images/3-viewcontrollerwithbutton-vsmac-large.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![Einen ansichtscontroller, das eine Schaltfläche](introduction-images/3-viewcontrollerwithbutton-vs.png "einen View Controller-Schaltfläche enthält.")](introduction-images/3-viewcontrollerwithbutton-vs-large.png#lightbox)

-----

Einige Steuerelemente, z.B. Bezeichnungen, die mit statischen Text, mit dem ansichtscontroller hinzugefügt, und sich selbst überlassen werden können. Allerdings müssen meistens, Steuerelemente programmgesteuert angepasst werden. Z. B. tun etwas beim Tippen auf, die Schaltfläche Weiter oben hinzugefügt, daher muss ein Ereignishandler im Code hinzugefügt werden.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

Um Zugriff auf und ändern die Schaltfläche im Code, müssen sie einen eindeutigen Bezeichner verfügen. Geben Sie einen eindeutigen Bezeichner durch Auswählen der Schaltfläche, die Sie öffnen die **Pad "Eigenschaften"**, und legen seine **Name** Feld einen Wert wie "SubmitButton":

[![Festlegen einer Schaltfläche namens im Bereich "Eigenschaften"](introduction-images/4-settingbuttonname-vsmac.png "Einstellungsname einer Schaltfläche im Bereich \"Eigenschaften\"")](introduction-images/4-settingbuttonname-vsmac-large.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Um Zugriff auf und ändern die Schaltfläche im Code, müssen sie einen eindeutigen Bezeichner verfügen. Geben Sie einen eindeutigen Bezeichner durch Auswählen der Schaltfläche, die Sie öffnen die **Fenster "Eigenschaften"**, und legen seine **Name** Feld einen Wert wie "SubmitButton":

[![Festlegen einer Schaltfläche Namen im Eigenschaftenfenster](introduction-images/4-settingbuttonname-vs.png "einer Schaltfläche-Namen im Fenster Eigenschaften festlegen")](introduction-images/4-settingbuttonname-vs-large.png#lightbox)

-----

Nun, dass die Schaltfläche einen Namen besitzt, kann er im Code zugegriffen werden. Aber wie funktioniert das?

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

In der **Lösungspad**, Navigation zu **ViewController.cs** und durch Klicken auf den Indikator für die Veröffentlichung von zeigt, dass des ansichtscontrollers `ViewController` Spannen von Klasse-Definition zwei Dateien, von denen jede enthält eine [Teilklasse](https://docs.microsoft.com/dotnet/csharp/programming-guide/classes-and-structs/partial-classes-and-methods) Definition:

[![Die beiden Dateien, die der ViewController Klasse bilden: ViewController.cs und ViewController.designer.cs](introduction-images/5-twoviewcontrollerfiles-vsmac.png "die beiden Dateien, die der ViewController Klasse bilden: ViewController.cs und ViewController.designer.cs")](introduction-images/5-twoviewcontrollerfiles-vsmac-large.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

In der **Projektmappen-Explorer**, Navigation zu **ViewController.cs** und durch Klicken auf den Indikator für die Veröffentlichung von zeigt, dass des ansichtscontrollers `ViewController` Klassendefinition umfasst zwei Dateien, von denen jede enthält eine [Teilklasse](https://docs.microsoft.com/dotnet/csharp/programming-guide/classes-and-structs/partial-classes-and-methods) Definition:

[![Die beiden Dateien, die der ViewController Klasse bilden: ViewController.cs und ViewController.designer.cs](introduction-images/5-twoviewcontrollerfiles-vs.png "die beiden Dateien, die der ViewController Klasse bilden: ViewController.cs und ViewController.designer.cs")](introduction-images/5-twoviewcontrollerfiles-vs-large.png#lightbox)

-----

- **ViewController.cs** gefüllt werden soll, mit benutzerdefiniertem Code im Zusammenhang mit der `ViewController` Klasse. In dieser Datei die `ViewController` Klasse kann Antworten auf verschiedenen iOS View Controller-Lebenszyklusmethoden, Anpassen der Benutzeroberfläche und reagieren auf Benutzereingaben, z. B. Schaltfläche tippt.

- **ViewController.designer.cs** ist eine generierte Datei, die von iOS-Designer, um die Schnittstelle visuell erstellten Code zuzuordnen erstellt. Da Änderungen an dieser Datei überschrieben werden, sollte es nicht geändert werden. Eigenschaftendeklarationen in dieser Datei können sie Code in die `ViewController` Klasse für den Zugriff auf, indem **Namen**, steuert Satz nach oben in der iOS-Designer. Öffnen von **ViewController.designer.cs** zeigt den folgenden Code:

```csharp
namespace Designer
{
    [Register ("ViewController")]
    partial class ViewController
    {
        [Outlet]
        [GeneratedCode ("iOS Designer", "1.0")]
        UIKit.UIButton SubmitButton { get; set; }

        void ReleaseDesignerOutlets ()
        {
            if (SubmitButton != null) {
                SubmitButton.Dispose ();
                SubmitButton = null;
            }
        }
    }
}
```

Die `SubmitButton` Eigenschaftendeklaration verbindet die gesamte `ViewController` Klasse – nicht nur die **ViewController.designer.cs** auf die Schaltfläche definiert, in dem Storyboard-Datei. Da **ViewController.cs** definiert Teil der `ViewController` -Klasse, hat Sie Zugriff auf `SubmitButton`.

Der folgende Screenshot veranschaulicht, dass IntelliSense jetzt erkennt die `SubmitButton` verweisen in **ViewController.cs**:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

[![IntelliSense erkennt den Verweis SubmitButton](introduction-images/6-submitbuttonintellisense-vsmac.png "IntelliSense erkennt die SubmitButton-Referenz")](introduction-images/6-submitbuttonintellisense-vsmac-large.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![IntelliSense erkennt den Verweis SubmitButton](introduction-images/6-submitbuttonintellisense-vs.png "IntelliSense erkennt die SubmitButton-Referenz")](introduction-images/6-submitbuttonintellisense-vs-large.png#lightbox)

-----

In diesem Abschnitt haben Sie gesehen, wie eine Schaltfläche "erstellen" im iOS-Designer und Zugriff auf diese Schaltfläche im Code.

Im weiteren Verlauf dieses Dokuments bietet einen weiteren Überblick über den iOS-Designer.

## <a name="ios-designer-basics"></a>iOS-Designer-Grundlagen

Dieser Abschnitt führt die Teile der iOS-Designer und bietet einen Überblick über die Funktionen.

### <a name="launching-the-ios-designer"></a>Starten den iOS-Designer

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

Xamarin.iOS-Projekte, die mit Visual Studio für Mac erstellt enthalten ein Storyboard. Um den Inhalt eines Storyboards anzuzeigen, doppelklicken Sie auf die Storyboard-Datei in die **Lösungspad**:

[![Öffnen Sie ein Storyboard in der iOS Designer](introduction-images/7-storyboardopen-vsmac.png "öffnen Sie ein Storyboard im iOS-Designer")](introduction-images/7-storyboardopen-vsmac-large.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Die meisten Xamarin.iOS-Projekte, die mit Visual Studio 2015 oder 2017 erstellt wurden, enthalten ein Storyboard. Um den Inhalt eines Storyboards anzuzeigen, doppelklicken Sie auf die Storyboard-Datei in die **Projektmappen-Explorer**:

[![Öffnen Sie ein Storyboard in der iOS Designer](introduction-images/7-storyboardopen-vs.png "öffnen Sie ein Storyboard im iOS-Designer")](introduction-images/7-storyboardopen-vs-large.png#lightbox)

-----

<a name="iOS_Designer_features"/>

### <a name="ios-designer-features"></a>iOS-Designer-Funktionen

Der iOS-Designer verfügt über sechs Hauptabschnitte:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

[![Teile des iOS Designers](introduction-images/8-sixpartsofiosdesigner-vsmac.png "Teile der iOS-Designer")](introduction-images/8-sixpartsofiosdesigner-vsmac-large.png#lightbox)

1. **Entwurfsoberfläche** – primäre Arbeitsbereich für die iOS-Designer. Im Bereich "Dokument" gezeigt, ermöglicht es der grafischen Erstellung von Benutzeroberflächen.
2. **Symbolleiste für Einschränkungen** – für das Wechseln zwischen Rahmen und der Einschränkung bearbeiten Modus zwei unterschiedliche Arten zum Positionieren von Elementen in einer Benutzeroberfläche bearbeiten können.
3. **Toolbox** – führt den Controller, Objekte, Steuerelemente, Datenansichten, Geste Erkennungen, Windows und Balken, der auf die Entwurfsoberfläche gezogen und eine Benutzeroberfläche hinzugefügt werden können.
4. **Pad "Eigenschaften"** – zeigt die Eigenschaften des ausgewählten Steuerelements, einschließlich der Identität, visuelle Stile, Barrierefreiheit, Layout und Verhalten.
5. **Dokumentgliederung** – zeigt die Struktur der Steuerelemente, aus denen das Layout für die Schnittstelle, die bearbeitet wird. Durch Klicken auf ein Element in der Struktur wird in der iOS-Designer ausgewählt, und zeigt die Eigenschaften in der **Pad "Eigenschaften"**. Dies ist praktisch für die Auswahl eines bestimmten Steuerelements in einer Benutzeroberfläche mit tief geschachtelt.
6. **Untere Symbolleiste** – enthält Optionen zum Ändern der Anzeige von iOS-Designer auf der Storyboard oder XIB-Datei, einschließlich der Geräte, Ausrichtung und einen Zoom.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![Teile des iOS Designers](introduction-images/8-sixpartsofiosdesigner-vs.png "Teile der iOS-Designer")](introduction-images/8-sixpartsofiosdesigner-vs-large.png#lightbox)

1. **Entwurfsoberfläche** – primäre Arbeitsbereich für die iOS-Designer. Im Bereich "Dokument" gezeigt, ermöglicht es der grafischen Erstellung von Benutzeroberflächen.
2. **Symbolleiste für Einschränkungen** – für das Wechseln zwischen Rahmen und der Einschränkung bearbeiten Modus zwei unterschiedliche Arten zum Positionieren von Elementen in einer Benutzeroberfläche bearbeiten können.
3. **Toolbox** – führt den Controller, Objekte, Steuerelemente, Datenansichten, Geste Erkennungen, Windows und Balken, der auf die Entwurfsoberfläche gezogen und eine Benutzeroberfläche hinzugefügt werden können.
4. **Fenster "Eigenschaften"** – zeigt die Eigenschaften des ausgewählten Steuerelements, einschließlich der Identität, visuelle Stile, Barrierefreiheit, Layout und Verhalten.
5. **Dokumentgliederung** – zeigt die Struktur der Steuerelemente, aus denen das Layout für die Schnittstelle, die bearbeitet wird. Durch Klicken auf ein Element in der Struktur wird in der iOS-Designer ausgewählt, und zeigt die Eigenschaften in der **Fenster "Eigenschaften"**. Dies ist praktisch für die Auswahl eines bestimmten Steuerelements in einer Benutzeroberfläche mit tief geschachtelt.
6. **Untere Symbolleiste** – enthält Optionen zum Ändern der Anzeige von iOS-Designer auf der Storyboard oder XIB-Datei, einschließlich der Geräte, Ausrichtung und einen Zoom.

-----

### <a name="design-workflow"></a>Entwerfen von Workflows

#### <a name="adding-a-control-to-the-interface"></a>Hinzufügen eines Steuerelements auf die-Schnittstelle

Um eine Schnittstelle ein Steuerelement hinzuzufügen, ziehen Sie es aus der **Toolbox** und legen Sie sie auf der Entwurfsoberfläche angezeigt. Beim Hinzufügen oder ein Steuerelement zu positionieren, markieren Sie vertikale und horizontale Richtlinien häufig verwendete Layouts Positionen, z. B. VERTICALCENTER, horizontalen Mitte und Ränder:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)
 
![Markieren Sie auf der Entwurfsoberfläche Richtlinien häufig verwendete Layouts Positionen](introduction-images/9-layoutguides-vsmac.png "markieren Sie auf der Entwurfsoberfläche Richtlinien Positionen für häufig verwendete Layouts")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![Markieren Sie auf der Entwurfsoberfläche Richtlinien häufig verwendete Layouts Positionen](introduction-images/9-layoutguides-vs.png "markieren Sie auf der Entwurfsoberfläche Richtlinien Positionen für häufig verwendete Layouts")

-----

Die blaue gepunktete Linie im obigen Beispiel bietet eine horizontalen Mitte visuellen Ausrichtung-Richtlinie für die Unterstützung bei der Platzierung der Schaltfläche.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

#### <a name="context-menu-commands"></a>Befehle im Kontextmenü

Ein Kontextmenü zur Verfügung steht, sowohl auf der Entwurfsoberfläche als auch in der **Dokumentgliederung**. Dieses Menü enthält Befehle für das ausgewählte Steuerelement und seinem übergeordneten Element, dies hilfreich ist bei der Verwendung von Ansichten in einer geschachtelten Hierarchie:

[![Im Kontextmenü auf der Entwurfsoberfläche](introduction-images/10-contextmenudesignsurface-vsmac.png "im Kontextmenü auf der Entwurfsoberfläche")](introduction-images/10-contextmenudesignsurface-vsmac-large.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

-----

### <a name="constraints-toolbar"></a>Symbolleiste für Einschränkungen

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)
 
[![Die Symbolleiste für Einschränkungen](introduction-images/11-constraintstoolbar-vsmac.png "die Symbolleiste für Einschränkungen")](introduction-images/11-constraintstoolbar-vsmac-large.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![Die Symbolleiste für Einschränkungen](introduction-images/11-constraintstoolbar-vs.png "die Symbolleiste für Einschränkungen")](introduction-images/11-constraintstoolbar-vs-large.png#lightbox)

-----

Die Symbolleiste für Einschränkungen aktualisiert wurde und jetzt besteht aus zwei Steuerelementen: der Frame im Bearbeitungsmodus / Einschränkung bearbeiten umschalten und die Einschränkungen für die Update / Frames Schaltfläche "Aktualisieren".

#### <a name="frame-editing-mode--constraint-editing-mode-toggle"></a>Frame im Bearbeitungsmodus / Einschränkung bearbeiten umschalten

In früheren Versionen von iOS-Designer ein-/ausgeschaltet klicken Sie auf ein bereits ausgewähltes Ansicht auf der Entwurfsoberfläche zwischen Frame-Modus und Bearbeitungsmodus Einschränkung bearbeiten. Nun, wechselt das Umschalten-Steuerelement in der Symbolleiste für Einschränkungen zwischen diesen Bearbeitungsmodi ein.

- Framebearbeitungsmodus:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

![Bearbeiten die Schaltfläche für Hilfemodus Frame](introduction-images/12a-frameeditingmode-vsmac.png "Frame-Schaltfläche \"Bearbeiten\" \"")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![Bearbeiten die Schaltfläche für Hilfemodus Frame](introduction-images/12a-frameeditingmode-vs.png "Frame-Schaltfläche \"Bearbeiten\" \"")

-----

- Bearbeitungsmodus der Einschränkung:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

![Bearbeiten die Schaltfläche für Hilfemodus Einschränkung](introduction-images/12b-constrainteditingmode-vsmac.png "Einschränkung Bearbeiten Schaltfläche für Hilfemodus")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![Bearbeiten die Schaltfläche für Hilfemodus Einschränkung](introduction-images/12b-constrainteditingmode-vs.png "Einschränkung Bearbeiten Schaltfläche für Hilfemodus")

-----

#### <a name="update-constraints--update-frames-button"></a>Aktualisieren Sie Einschränkungen / aktualisieren Sie Frames Schaltfläche ""

Die Einschränkungen für die Update / Update frames Schaltfläche befindet sich rechts neben der Frame im Bearbeitungsmodus / Einschränkung bearbeiten umschalten.

- Im Frame im Bearbeitungsmodus passt klicken auf diese Schaltfläche die Frames, der alle ausgewählten Elemente entsprechend ihren Einschränkungen aus.
- Im Bearbeitungsmodus der Einschränkung passt den Einschränkungen für alle ausgewählten Elemente entsprechend ihrer Frames an, wenn Sie auf diese Schaltfläche klicken.

### <a name="bottom-toolbar"></a>Untere Symbolleiste

Die unteren Symbolleiste bietet eine Möglichkeit zur Auswahl der Geräte, Ausrichtung und einen Zoom verwendet, um ein Storyboard oder XIB-Datei im iOS-Designer anzeigen:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

[![Der unteren Symbolleiste verwendet, um ein Gerät und die Ausrichtung für die Entwurfsoberfläche auszuwählen](introduction-images/13-bottomtoolbar-vsmac.png "der unteren Symbolleiste verwendet, um ein Gerät und die Ausrichtung für die Entwurfsoberfläche auswählen")](introduction-images/13-bottomtoolbar-vsmac-large.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![Der unteren Symbolleiste verwendet, um ein Gerät und die Ausrichtung für die Entwurfsoberfläche auszuwählen](introduction-images/13-bottomtoolbar-vs.png "der unteren Symbolleiste verwendet, um ein Gerät und die Ausrichtung für die Entwurfsoberfläche auswählen")](introduction-images/13-bottomtoolbar-vs-large.png#lightbox)

-----

#### <a name="device-and-orientation"></a>Gerät und Ausrichtung

Wenn die Kategorie erweitert ist, zeigt die unteren Symbolleiste an alle Geräte, Ausrichtungen und/oder Anpassungen, die für das aktuelle Dokument. Klicken Sie auf diese Änderungen in der Ansicht auf der Entwurfsoberfläche angezeigt. 

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

[![Die unteren Symbolleiste erweitert, sodass Geräte und -Ausrichtungen](introduction-images/14-bottomtoolbarexpanded-vsmac.png "der unteren Symbolleiste erweitert, um Geräte und -Ausrichtungen anzuzeigen.")](introduction-images/14-bottomtoolbarexpanded-vsmac-large.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![Die unteren Symbolleiste erweitert, sodass Geräte und -Ausrichtungen](introduction-images/14-bottomtoolbarexpanded-vs.png "der unteren Symbolleiste erweitert, um Geräte und -Ausrichtungen anzuzeigen.")](introduction-images/14-bottomtoolbarexpanded-vs-large.png#lightbox)

-----

Beachten Sie, dass durch Auswahl von einem Gerät und die Ausrichtung ändert sich nur wie iOS-Designer das Design zeigt in der Vorschau. Unabhängig von der aktuellen Auswahl neu hinzugefügte Einschränkungen gelten für alle Geräte und -Ausrichtungen, es sei denn, die **bearbeiten "traits"** Schaltfläche wurde verwendet, um nichts anderes angeben.

Wenn [Größenklassen](~/ios/user-interface/storyboards/unified-storyboards.md#size-classes) sind [aktiviert](~/ios/user-interface/storyboards/unified-storyboards.md#enabling-size-classes), **bearbeiten "traits"** Schaltfläche in der erweiterten unteren Symbolleiste angezeigt.  Klicken auf die **bearbeiten "traits"** Schaltfläche zeigt Optionen für das Erstellen einer Schnittstelle Variation basierend auf der Größenklasse, die durch das ausgewählte Gerät und Ausrichtung dargestellt wird. Betrachten Sie die folgenden Beispiele:

- Wenn **iPhone SE** / **Hochformat**, wird ausgewählt, im Popover bietet Optionen, um eine Variante Schnittstelle für die compact Breite Größenklasse regulären Höhe zu erstellen. 
- Wenn **iPad Pro 9,7"** / **Querformat** / **Vollbild** wird ausgewählt, im Popover bietet Optionen zum Erstellen von einer Variante Schnittstelle für die reguläre Breite und die regulären Höhe Größenklasse.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

[![Der unteren Symbolleiste wird, eine Schnittstelle variieren nach Größenklasse](introduction-images/15-edittraitsbutton-vsmac.png "der unteren Symbolleiste, um eine Schnittstelle variieren nach Größenklasse verwendet wird")](introduction-images/15-edittraitsbutton-vsmac-large.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![Der unteren Symbolleiste wird, eine Schnittstelle variieren nach Größenklasse](introduction-images/15-edittraitsbutton-vs.png "der unteren Symbolleiste, um eine Schnittstelle variieren nach Größenklasse verwendet wird")](introduction-images/15-edittraitsbutton-vs-large.png#lightbox)

-----

#### <a name="zoom-controls"></a>Zoom-Steuerelemente

Die Entwurfsoberfläche unterstützt das Zoomen über mehrere Steuerelemente:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)
 
![Die Zoom-Steuerelemente in der unteren Symbolleiste](introduction-images/16-zoomcontrols-vsmac.png "die Zoom-Steuerelemente in der unteren Symbolleiste")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![Die Zoom-Steuerelemente in der unteren Symbolleiste](introduction-images/16-zoomcontrols-vs.png "die Zoom-Steuerelemente in der unteren Symbolleiste")

-----

Die Steuerelemente umfassen Folgendes:

1. Zoom anpassen
2. Verkleinern
3. Vergrößern
4. Tatsächliche Größe (1:1-Pixel-Größe)

Diese Steuerelemente passen Sie den Zoom auf der Entwurfsoberfläche angezeigt. Sie wirken sich nicht auf die Benutzeroberfläche der Anwendung zur Laufzeit aus.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

### <a name="properties-pad"></a>Pad "Eigenschaften"

Verwenden der **Pad "Eigenschaften"** so bearbeiten Sie die Identität, visuelle Stile, Barrierefreiheit und Verhalten eines Steuerelements. Der folgende Screenshot veranschaulicht die **Pad "Eigenschaften"** Optionen für eine Schaltfläche:

[![Das Pad "Eigenschaften" für eine Schaltfläche](introduction-images/17-buttonpropertiespad-vsmac.png "das Pad \"Eigenschaften\" für eine Schaltfläche")](introduction-images/17-buttonpropertiespad-vsmac-large.png#lightbox)
#### <a name="properties-pad-sections"></a>Eigenschaften-Pad-Abschnitte

Die **Pad "Eigenschaften"** enthält drei Abschnitte:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

### <a name="properties-window"></a>Eigenschaftenfenster

Verwenden der **Fenster "Eigenschaften"** so bearbeiten Sie die Identität, visuelle Stile, Barrierefreiheit und Verhalten eines Steuerelements. Der folgende Screenshot veranschaulicht die **Fenster "Eigenschaften"** Optionen für eine Schaltfläche:

[![Fenster "Eigenschaften" für eine Schaltfläche](introduction-images/17-buttonpropertieswindow-vs.png "das Fenster \"Eigenschaften\" für eine Schaltfläche")](introduction-images/17-buttonpropertieswindow-vs-large.png#lightbox)

#### <a name="properties-window-sections"></a>Eigenschaften-Fenster-Abschnitte

Die **Fenster "Eigenschaften"** enthält drei Abschnitte:

-----

1.  **Widget** – die Haupteigenschaften des Steuerelements, z. B. Name, Klasse, Eigenschaften usw. Eigenschaften für die Verwaltung von Inhalt des Steuerelements werden normalerweise hier eingefügt.
2.  **Layout** – Eigenschaften, die nachverfolgen die Position und Größe des Steuerelements, einschließlich Einschränkungen und Frames sind hier aufgeführt.
3.  **Ereignisse** – Ereignisse und Ereignishandler werden hier angegeben. Nützlich zum Behandeln von Ereignissen, z. B. Touch, tippen, ziehen Sie usw. Ereignisse können auch direkt in Code behandelt werden.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

#### <a name="editing-properties-in-the-properties-pad"></a>Bearbeiten von Eigenschaften im Bereich "Eigenschaften"

Zusätzlich zu visuelle Bearbeitung auf der Entwurfsoberfläche, der iOS-Designer unterstützt die Bearbeitung von Eigenschaften in der **Pad "Eigenschaften"**. Die verfügbaren Eigenschaften ändern sich entsprechend das ausgewählte Steuerelement, wie in den folgenden Screenshots dargestellt:

[![Schaltfläche Eigenschaften](introduction-images/18a-buttonpropertiespad-vsmac.png "Schaltfläche Eigenschaften")](introduction-images/18a-buttonpropertiespad-vsmac-large.png#lightbox)

[![Anzeigen der Eigenschaften von Buildcontroller](introduction-images/18b-viewcontrollerpropertiespad-vsmac.png "Anzeigen der Eigenschaften von Buildcontroller")](introduction-images/18b-viewcontrollerpropertiespad-vsmac-large.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

#### <a name="editing-properties-in-the-properties-window"></a>Bearbeiten von Eigenschaften im Eigenschaftenfenster

Zusätzlich zu visuelle Bearbeitung auf der Entwurfsoberfläche, der iOS-Designer unterstützt die Bearbeitung von Eigenschaften in der **Fenster "Eigenschaften"**. Die verfügbaren Eigenschaften ändern sich entsprechend das ausgewählte Steuerelement, wie in den folgenden Screenshots dargestellt:

[![Schaltfläche Eigenschaften](introduction-images/18a-buttonpropertieswindow-vs.png "Schaltfläche Eigenschaften")](introduction-images/18a-buttonpropertieswindow-vs-large.png#lightbox)

[![Anzeigen der Eigenschaften von Buildcontroller](introduction-images/18b-viewcontrollerpropertieswindow-vs.png "Anzeigen der Eigenschaften von Buildcontroller")](introduction-images/18b-viewcontrollerpropertieswindow-vs-large.png#lightbox)

-----

> [!IMPORTANT]
> Der Identitätsabschnitt des das Pad "Eigenschaften" jetzt zeigt eine **Modul** Feld. Es ist erforderlich, die in diesem Abschnitt geben Sie nur bei der Interaktion mit Swift-Klassen. Verwenden Sie, um den Modulnamen für Swift-Klassen, geben Sie die Namespaces sind.

#### <a name="default-values"></a>Standardwerte

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

Viele Eigenschaften in der **Pad "Eigenschaften"** zeigen kein Wert oder einen Standardwert zurück. Den Code der Anwendung kann diese Werte jedoch immer noch ändern. Die **Pad "Eigenschaften"** zeigt keine Werte im Code festlegen.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Viele Eigenschaften in der **Fenster "Eigenschaften"** zeigen kein Wert oder einen Standardwert zurück. Den Code der Anwendung kann diese Werte jedoch immer noch ändern. Die **Fenster "Eigenschaften"** zeigt keine Werte im Code festlegen.

-----

#### <a name="event-handlers"></a>Ereignishandler

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

Verwenden Sie zum Angeben benutzerdefinierter Ereignishandler für die verschiedenen Ereignisse der **Ereignisse** Registerkarte die **Pad "Eigenschaften"**. Z. B. im folgenden Screenshot eine `HandleClick` Methode verarbeitet der Schaltfläche " **berühren sich innerhalb von** Ereignis:

[![Das Pad "Eigenschaften", mit einem Ereignishandler festzulegen, die für eine Schaltfläche](introduction-images/19-buttonpropertiespadevents-vsmac.png "das Pad \"Eigenschaften\", mit einem Ereignishandler für eine Schaltfläche festlegen")](introduction-images/19-buttonpropertiespadevents-vsmac-large.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Verwenden Sie zum Angeben benutzerdefinierter Ereignishandler für die verschiedenen Ereignisse der **Ereignisse** Registerkarte die **Fenster "Eigenschaften"**. Z. B. im folgenden Screenshot eine `HandleClick` Methode verarbeitet der Schaltfläche " **berühren sich innerhalb von** Ereignis:

[![Eigenschaftenfenster mit einem Ereignishandler festzulegen, die für eine Schaltfläche](introduction-images/19-buttonpropertieswindowevents-vs.png "das Fenster \"Eigenschaften\", mit einem Ereignishandler für eine Schaltfläche festlegen")](introduction-images/19-buttonpropertieswindowevents-vs-large.png#lightbox)

-----

Sobald ein Ereignishandler angegeben wurde, muss die entsprechende Ansicht-Controller-Klasse eine Methode mit dem gleichen Namen hinzugefügt werden. Andernfalls ein `unrecognized selector` Ausnahme tritt auf, wenn die Schaltfläche getippt wird:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

[![Eine Ausnahme unbekannten Selektor](introduction-images/20-unrecognizedselector-vsmac.png "eine unbekannte Selector-Ausnahme")](introduction-images/20-unrecognizedselector-vsmac-large.png#lightbox)

Beachten Sie, die nach der ein Ereignishandler in angegeben wurde, die **Pad "Eigenschaften"**, des iOS Designers sofort die entsprechende Codedatei zu öffnen und bieten die Methodendeklaration eingefügt wird. 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![Eine Ausnahme unbekannten Selektor](introduction-images/20-unrecognizedselector-vs.png "eine unbekannte Selector-Ausnahme")](introduction-images/20-unrecognizedselector-vs-large.png#lightbox)

-----

Ein Beispiel, benutzerdefinierte Ereignishandler verwendet, finden Sie in der [Hallo, iOS, erste-Schritte-Handbuch](~/ios/get-started/hello-ios/index.md).

### <a name="outline-view"></a>Gliederungsansicht

Der iOS-Designer können auch eine Schnittstelle für die Hierarchie der Steuerelemente als Gliederung anzeigen. Die Gliederung finden Sie dazu die **Dokumentgliederung** Registerkarte wie folgt:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

[![Die Dokumentgliederung](introduction-images/21-buttonoutlineview-vsmac.png "der Dokumentgliederung")](introduction-images/21-buttonoutlineview-vsmac-large.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![Die Dokumentgliederung](introduction-images/21-buttonoutlineview-vs.png "der Dokumentgliederung")](introduction-images/21-buttonoutlineview-vs-large.png#lightbox)

-----

Das ausgewählte Steuerelement in der Gliederungsansicht bleibt synchron mit dem ausgewählten Steuerelement auf der Entwurfsoberfläche angezeigt.  Dieses Feature eignet sich für ein Element aus einer tief verschachtelten Schnittstellenhierarchie ausgewählt.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

## <a name="revert-to-xcode"></a>Wiederherstellen mit Xcode

Es ist möglich, die iOS-Designer und Interface Builder von Xcode austauschbar verwenden. Ein Storyboard oder eine XIB-Datei in Xcode Interface Builder öffnen, mit der Maustaste auf die Datei, und wählen Sie **Öffnen mit > Interface Builder von Xcode**, wie im folgenden Screenshot dargestellt:

[![Öffnen ein Storyboard in Xcode Interface Builder](introduction-images/22-openinxcodeinterfacebuilder-vsmac.png "ein Storyboard in Xcode Interface Builder öffnen")](introduction-images/22-openinxcodeinterfacebuilder-vsmac-large.png#lightbox)

Nach dem vornehmen von Änderungen in Xcode Interface Builder, die Datei speichern und zurück zu Visual Studio für Mac. Die Änderungen werden mit Xamarin.iOS-Projekt synchronisiert.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

-----

## <a name="xib-support"></a>XIB-Unterstützung

Der iOS-Designer unterstützt das Erstellen, bearbeiten und Verwalten von XIB-Dateien. Diese sind XML-Dateien, geschlossenes einzelne, benutzerdefinierte Ansichten der Anwendung der Hierarchie von Inhaltsansichten hinzugefügt werden können. Eine XIB-Datei stellt die Schnittstelle für eine einzelne Ansicht oder einen Bildschirm in einer Anwendung, in der Regel dar, während ein Storyboard viele Bildschirme und die Übergänge zwischen ihnen darstellt.

Es gibt viele meinungen über die Lösung – XIB-Dateien, Storyboards oder Code – am besten für Sie erstellen und verwalten eine Benutzeroberfläche ist. Es gibt keine perfekte Lösung, und es ist immer in Betracht ziehen das beste Tool für den Auftrag zur Verfügung, in der Praxis. Allerdings XIB-Dateien in der Regel leistungsstärksten Wenn verwendet, um eine benutzerdefinierte Ansicht, die erforderlich sind, an mehreren Stellen in eine app, wie z. B. eine benutzerdefinierte tabellenansichtszelle darzustellen sind. 

Weitere Informationen zur Verwendung von XIB-Dateien finden Sie in der folgenden Anleitungen:

- [Mithilfe der Ansicht XIB-Vorlage](https://github.com/xamarin/recipes/tree/master/Recipes/ios/general/templates/using_the_ios_view_xib_template)
- [Erstellen eine benutzerdefinierte TableViewCell mithilfe einer XIB](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/tables/custom-tableviewcell)
- [Erstellen einen Startbildschirm mithilfe einer XIB](https://github.com/xamarin/recipes/tree/master/Recipes/ios/general/templates/launchscreen-xib)

Weitere Informationen zur Verwendung von Storyboards, finden Sie in der [Einführung in Storyboards](~/ios/user-interface/storyboards/index.md).

Diese und andere iOS-Designer-bezogene Anleitungen finden Sie auf die Verwendung von Storyboards als die Standardmethode zum Erstellen von Benutzeroberflächen, da die meisten Xamarin.iOS neue Projektvorlagen ein Storyboard standardmäßig bereit.

## <a name="summary"></a>Zusammenfassung

Diese Anleitung enthält einer Einführung in die Beschreibung der Features und Gliedern die Tools, die er bietet für das Entwerfen schicke Benutzeroberflächen-Designer für IOS.

## <a name="related-links"></a>Verwandte Links

- [Einführung in Storyboards](~/ios/user-interface/storyboards/index.md)
- [iOS entworfen Steuerelemente Exemplarische Vorgehensweise](~/ios/user-interface/designer/ios-designable-controls-walkthrough.md)
- [Hello, iOS](~/ios/get-started/hello-ios/index.md)
- [Hello, iOS Multiscreen](~/ios/get-started/hello-ios-multiscreen/index.md)
- [Übersicht über Android Designer](~/android/user-interface/android-designer/index.md)
- [Partielle Klassen und Methoden](https://docs.microsoft.com/dotnet/csharp/programming-guide/classes-and-structs/partial-classes-and-methods)
- [Einstieg in die Xamarin-Designer für iOS – weiterentwickelt 2014 (video)](https://www.youtube.com/watch?v=W4H9uLjoEjM)
- [Verwendung des iOS Designers einen Startbildschirm (video) erstellen.](https://university.xamarin.com/lightninglectures/using-the-ios-designer-to-create-a-launch-screen)
