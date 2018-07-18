---
title: iOS-Designer-Grundlagen
description: Dieses Handbuch enthält die Xamarin-Designer für iOS an. Es zeigt, wie Sie den iOS-Designer verwenden, um das visuelle Layout Steuerelemente, wie Zugriff auf diese Steuerelemente im Code und Bearbeiten von Eigenschaften.
ms.prod: xamarin
ms.assetid: E7045E41-0DEF-416B-BCDB-52502350F61C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 01/31/2018
ms.openlocfilehash: 7e36a402619813214e821f3060e053d76c99cfb7
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
ms.locfileid: "30784257"
---
# <a name="ios-designer-basics"></a>iOS-Designer-Grundlagen

_Dieses Handbuch enthält die Xamarin-Designer für iOS an. Es zeigt, wie Sie den iOS-Designer verwenden, um das visuelle Layout Steuerelemente, wie Zugriff auf diese Steuerelemente im Code und Bearbeiten von Eigenschaften._

Die Xamarin-Designer für iOS ist eine visuelle Oberfläche ähnlich Xcodes Benutzeroberflächen-Generator und dem Android-Designer. Einige der zahlreichen Funktionen enthalten die nahtlose Integration in Visual Studio für Mac und Visual Studio 2015 und 2017, Drag & Drop-Bearbeitung, eine Schnittstelle zum Einrichten der Ereignishandler und die Möglichkeit, benutzerdefinierte Steuerelemente darzustellen.

## <a name="requirements"></a>Anforderungen

IOS ist auf Windows-Designer in Visual Studio für Mac und in Visual Studio 2015 und 2017 verfügbar. In Visual Studio 2015 oder 2017 erfordert iOS-Designer eine Verbindung mit einem ordnungsgemäß konfigurierten Mac-Build-Host an, obwohl Xcode nicht ausgeführt werden muss.

Dieses Handbuch setzt voraus, Vertrautheit mit den Inhalt in behandelt die [Einstieg führt](~/ios/get-started/index.md).

<a name="how-it-works" />

## <a name="how-the-ios-designer-works"></a>Wie funktioniert die iOS-Designer

In diesem Abschnitt wird beschrieben, wie die iOS-Designer erleichtert das Erstellen einer Benutzeroberfläche und mit dem Code verbunden.

Die iOS-Designer kann Entwickler Benutzeroberfläche einer Anwendung visuell zu entwerfen. Wie in der [Einführung in Storyboards](~/ios/user-interface/storyboards/index.md) Guide ein Storyboard beschreibt die Bildschirme (View Controller), aus denen eine app, die Elemente der Benutzeroberfläche (Views) belastet die View-Controller und die app Gesamtablauf Navigation . 

Eine modellansichtcontroller besteht aus zwei Teilen: eine visuelle Darstellung in der iOS-Designer und eine zugeordnete C#-Klasse:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Eine Ansicht Controller in der iOS-Designer](introduction-images/1-storyboardwithviewcontroller-vsmac.png "einen Controller Ansicht in der iOS-Designer")](introduction-images/1-storyboardwithviewcontroller-vsmac-large.png#lightbox)

[![Der Code für einen Controller Ansicht](introduction-images/2-viewcontrollercode-vsmac.png "den Code für einen Controller anzeigen")](introduction-images/2-viewcontrollercode-vsmac-large.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Eine Ansicht Controller in der iOS-Designer](introduction-images/1-storyboardwithviewcontroller-vs.png "einen Controller Ansicht in der iOS-Designer")](introduction-images/1-storyboardwithviewcontroller-vs-large.png#lightbox)

[![Der Code für einen Controller Ansicht](introduction-images/2-viewcontrollercode-vs.png "den Code für einen Controller anzeigen")](introduction-images/2-viewcontrollercode-vs-large.png#lightbox)

-----

Ein Controller für die Sicht bereit keine Funktionalität, in seinem Standardzustand; Es muss mit Steuerelementen aufgefüllt werden. Diese Steuerelemente befinden sich in der Ansicht Controller Ansicht der rechteckige Bereich, die gesamte Inhalt des Bildschirms enthält. Die meisten View-Controller enthalten allgemeine Steuerelemente wie Schaltflächen, Bezeichnungen und Textfelder, wie im folgenden Screenshot dargestellt, die eine Schaltfläche enthält modellansichtcontroller dargestellt wird: 

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Eine Schaltfläche enthält modellansichtcontroller](introduction-images/3-viewcontrollerwithbutton-vsmac.png "eine modellansichtcontroller Schaltfläche enthält.")](introduction-images/3-viewcontrollerwithbutton-vsmac-large.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Eine Schaltfläche enthält modellansichtcontroller](introduction-images/3-viewcontrollerwithbutton-vs.png "eine modellansichtcontroller Schaltfläche enthält.")](introduction-images/3-viewcontrollerwithbutton-vs-large.png#lightbox)

-----

Einige Steuerelemente, z. B. Bezeichnungen mit statischen Text, können mit dem View-Controller hinzugefügt und gelassen werden. Allerdings müssen mehr häufig als nicht Steuerelemente programmgesteuert angepasst werden. Die Schaltfläche mit der oben hinzugefügten sollte z. B. etwas beim Tippen, erfolgen, sodass ein Ereignishandler im Code hinzugefügt werden muss.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Um zugreifen und diese Bearbeiten der Schaltfläche im Code, müssen sie einen eindeutigen Bezeichner verfügen. Geben Sie einen eindeutigen Bezeichner durch Auswählen der Schaltfläche aus, öffnen die **Eigenschaften Pad**, verwendet wird und seine **Namen** Feld auf einen Wert wie "SubmitButton":

[![Eine Schaltfläche Einstellungsname Eigenschaften Pad](introduction-images/4-settingbuttonname-vsmac.png "Einstellungsname eine Schaltfläche in den Eigenschaften mit Leerstellen auffüllen")](introduction-images/4-settingbuttonname-vsmac-large.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Um zugreifen und diese Bearbeiten der Schaltfläche im Code, müssen sie einen eindeutigen Bezeichner verfügen. Geben Sie einen eindeutigen Bezeichner durch Auswählen der Schaltfläche aus, öffnen die **Fenster "Eigenschaften"**, verwendet wird und seine **Namen** auf einen Wert wie "SubmitButton" Feld:

[![Eine Schaltfläche Einstellungsname im Eigenschaftenfenster](introduction-images/4-settingbuttonname-vs.png "Einstellungsname eine Schaltfläche im Eigenschaftenfenster")](introduction-images/4-settingbuttonname-vs-large.png#lightbox)

-----

Die Schaltfläche "" einen Namen besitzt, kann er im Code zugegriffen werden. Wie funktioniert dies jedoch?

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

In der **Lösung Pad**, navigieren Sie zu **ViewController.cs** , und klicken auf das Symbol für die Offenlegung von, die die View-Controller `ViewController` Klasse Definition Spannen zwei Dateien, von denen jede enthält eine [Teilklasse](https://docs.microsoft.com/dotnet/csharp/programming-guide/classes-and-structs/partial-classes-and-methods) Definition:

[![Die beiden Dateien, bilden die ViewController-Klasse: ViewController.cs und ViewController.designer.cs](introduction-images/5-twoviewcontrollerfiles-vsmac.png "die beiden Dateien, bilden die ViewController-Klasse: ViewController.cs und ViewController.designer.cs")](introduction-images/5-twoviewcontrollerfiles-vsmac-large.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

In der **Projektmappen-Explorer**, navigieren Sie zu **ViewController.cs** , und klicken auf das Symbol für die Offenlegung von, die die View-Controller `ViewController` Klassendefinition umfasst zwei Dateien, jede enthält eine [Teilklasse](https://docs.microsoft.com/dotnet/csharp/programming-guide/classes-and-structs/partial-classes-and-methods) Definition:

[![Die beiden Dateien, bilden die ViewController-Klasse: ViewController.cs und ViewController.designer.cs](introduction-images/5-twoviewcontrollerfiles-vs.png "die beiden Dateien, bilden die ViewController-Klasse: ViewController.cs und ViewController.designer.cs")](introduction-images/5-twoviewcontrollerfiles-vs-large.png#lightbox)

-----

- **ViewController.cs** aufgefüllt werden soll, mit benutzerdefiniertem Code im Zusammenhang mit der `ViewController` Klasse. In dieser Datei den `ViewController` Klasse kann für verschiedene iOS-View-Controller Lebenszyklusmethoden reagieren, Anpassen der Benutzeroberfläche und reagieren auf Benutzereingaben, z. B. Schaltfläche tippt.

- **ViewController.designer.cs** wird eine generierte Datei, die erstellt werden, von dem iOS-Designer können Sie die Schnittstelle visuell erstellt Code zuzuordnen. Da Änderungen an dieser Datei überschrieben werden, sollten sie nicht geändert werden. Eigenschaftendeklarationen in dieser Datei können sie Code in der `ViewController` -Klasse für den Zugriff auf, indem **Namen**, steuert die Menge von in der iOS-Designer. Öffnen von **ViewController.designer.cs** den folgenden Code zeigt:

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

Die `SubmitButton` Eigenschaftendeklaration verbindet die gesamte `ViewController` Klasse – nicht nur die **ViewController.designer.cs** Datei – auf die Schaltfläche "" in das Storyboard definiert. Da **ViewController.cs** definiert Teil der `ViewController` -Klasse, hat sie Zugriff auf `SubmitButton`.

Der folgende Screenshot zeigt, dass IntelliSense jetzt erkennt die `SubmitButton` verweisen in **ViewController.cs**:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![IntelliSense, erkennen den Verweis SubmitButton](introduction-images/6-submitbuttonintellisense-vsmac.png "IntelliSense erkennen die SubmitButton-Referenz")](introduction-images/6-submitbuttonintellisense-vsmac-large.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![IntelliSense, erkennen den Verweis SubmitButton](introduction-images/6-submitbuttonintellisense-vs.png "IntelliSense erkennen die SubmitButton-Referenz")](introduction-images/6-submitbuttonintellisense-vs-large.png#lightbox)

-----

In diesem Abschnitt gezeigten hat wie eine Schaltfläche in der iOS-Designer erstellen und Zugriff auf die Schaltfläche im Code.

Im weiteren Verlauf dieses Dokuments bietet eine weitere Übersicht des iOS-Designer.

## <a name="ios-designer-basics"></a>iOS-Designer-Grundlagen

In diesem Abschnitt werden die Teile des iOS-Designer eingeführt und bietet einen Überblick über die zugehörigen Funktionen.

### <a name="launching-the-ios-designer"></a>Starten die iOS-Designer

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Xamarin.iOS-Projekte, die mit Visual Studio erstellt wurden, für Mac enthalten ein Storyboard. Um den Inhalt eines Drehbuchs anzuzeigen, doppelklicken Sie auf die .storyboard-Datei in die **Lösung Pad**:

[![Öffnen Sie ein Storyboard in der iOS-Designer](introduction-images/7-storyboardopen-vsmac.png "ein Storyboard in der iOS-Designer zu öffnen.")](introduction-images/7-storyboardopen-vsmac-large.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Die meisten Xamarin.iOS Projekte mit Visual Studio 2015 oder 2017 erstellt einschließen ein storyboard Um den Inhalt eines Drehbuchs anzuzeigen, doppelklicken Sie auf die .storyboard-Datei in die **Projektmappen-Explorer**:

[![Öffnen Sie ein Storyboard in der iOS-Designer](introduction-images/7-storyboardopen-vs.png "ein Storyboard in der iOS-Designer zu öffnen.")](introduction-images/7-storyboardopen-vs-large.png#lightbox)

-----

<a name="iOS_Designer_features"/>

### <a name="ios-designer-features"></a>iOS-Designer-Funktionen

Die iOS-Designer verfügt über sechs Hauptabschnitten:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Abschnitte des iOS-Designer](introduction-images/8-sixpartsofiosdesigner-vsmac.png "Abschnitte des iOS-Designer")](introduction-images/8-sixpartsofiosdesigner-vsmac-large.png#lightbox)

1. **Entwurfsoberfläche** – primäre Arbeitsbereich für die iOS-Designer. Im Bereich "Dokument" gezeigt, kann der grafischen Erstellung von Benutzeroberflächen.
2. **Einschränkungen Symbolleiste** – für den Wechsel zwischen Rahmen und Einschränkung bearbeiten Modus, zwei unterschiedliche Arten zum Positionieren von Elementen in einer Benutzeroberfläche bearbeiten können.
3. **Toolbox** – Listet die Controller, Objekte, Steuerelemente, Datenansichten, Geste Prüfer, Windows und Steuerleisten, die auf die Entwurfsoberfläche gezogen und eine Benutzeroberfläche hinzugefügt werden können.
4. **Eigenschaften mit Leerstellen auffüllen** – zeigt die Eigenschaften des ausgewählten Steuerelements, einschließlich der Identität, visuelle Stile, Barrierefreiheit, Layout und Verhalten.
5. **Dokumentgliederung** – zeigt die Struktur von Steuerelementen, die das Layout für die Schnittstelle, die zu bearbeitenden bilden. Durch Klicken auf ein Element in der Struktur wird in der iOS-Designer ausgewählt und gezeigt, seiner Eigenschaften in der **Eigenschaften Pad**. Dies ist praktisch für die Auswahl eines bestimmten Steuerelements in einer Benutzeroberfläche mit tief geschachtelt.
6. **Symbolleiste unten** – enthält Optionen zum Ändern der Anzeige von iOS-Designer auf der Datei .storyboard oder .xib Gerät, Ausrichtung und einen Zoom einschließlich.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Abschnitte des iOS-Designer](introduction-images/8-sixpartsofiosdesigner-vs.png "Abschnitte des iOS-Designer")](introduction-images/8-sixpartsofiosdesigner-vs-large.png#lightbox)

1. **Entwurfsoberfläche** – primäre Arbeitsbereich für die iOS-Designer. Im Bereich "Dokument" gezeigt, kann der grafischen Erstellung von Benutzeroberflächen.
2. **Einschränkungen Symbolleiste** – für den Wechsel zwischen Rahmen und Einschränkung bearbeiten Modus, zwei unterschiedliche Arten zum Positionieren von Elementen in einer Benutzeroberfläche bearbeiten können.
3. **Toolbox** – Listet die Controller, Objekte, Steuerelemente, Datenansichten, Geste Prüfer, Windows und Steuerleisten, die auf die Entwurfsoberfläche gezogen und eine Benutzeroberfläche hinzugefügt werden können.
4. **Fenster "Eigenschaften"** – zeigt die Eigenschaften des ausgewählten Steuerelements, einschließlich der Identität, visuelle Stile, Barrierefreiheit, Layout und Verhalten.
5. **Dokumentgliederung** – zeigt die Struktur von Steuerelementen, die das Layout für die Schnittstelle, die zu bearbeitenden bilden. Durch Klicken auf ein Element in der Struktur wird in der iOS-Designer ausgewählt und gezeigt, seiner Eigenschaften in der **Fenster "Eigenschaften"**. Dies ist praktisch für die Auswahl eines bestimmten Steuerelements in einer Benutzeroberfläche mit tief geschachtelt.
6. **Symbolleiste unten** – enthält Optionen zum Ändern der Anzeige von iOS-Designer auf der Datei .storyboard oder .xib Gerät, Ausrichtung und einen Zoom einschließlich.

-----

### <a name="design-workflow"></a>Entwerfen von Workflows

#### <a name="adding-a-control-to-the-interface"></a>Hinzufügen eines Steuerelements auf die Schnittstelle

Um ein Steuerelement zu einer Schnittstelle hinzuzufügen, ziehen Sie es aus der **Toolbox** und legen Sie sie auf der Entwurfsoberfläche angezeigt. Beim Hinzufügen oder positionieren ein Steuerelement, markieren Sie Richtlinien für vertikale und horizontale häufig verwendete Layout Positionen wie vertikal zentrieren, horizontal zentrieren und Ränder:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)
 
![Markieren Sie am häufigsten verwendeten Layout Positionen auf der Entwurfsoberfläche Richtlinien](introduction-images/9-layoutguides-vsmac.png "markieren Sie am häufigsten verwendeten Layout Positionen auf der Entwurfsoberfläche Richtlinien")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Markieren Sie am häufigsten verwendeten Layout Positionen auf der Entwurfsoberfläche Richtlinien](introduction-images/9-layoutguides-vs.png "markieren Sie am häufigsten verwendeten Layout Positionen auf der Entwurfsoberfläche Richtlinien")

-----

Die blaue gepunktete Linie im obigen Beispiel bietet Richtlinie visual Ausrichtung horizontal zentrieren, das Befehlsschaltflächen-Platzierung verwenden können.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

#### <a name="context-menu-commands"></a>Kontextmenübefehle

Ein Kontextmenü steht sowohl auf der Entwurfsoberfläche als auch in der **Dokumentgliederung**. Dieses Menü enthält Befehle, für die das ausgewählte Steuerelement und seinem übergeordneten Element, was beim Arbeiten mit Sichten in einer geschachtelten Hierarchie hilfreich ist:

[![Im Kontextmenü den Befehl auf der Entwurfsoberfläche](introduction-images/10-contextmenudesignsurface-vsmac.png "im Kontextmenü den Befehl auf der Entwurfsoberfläche")](introduction-images/10-contextmenudesignsurface-vsmac-large.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

-----

### <a name="constraints-toolbar"></a>Einschränkungen-Symbolleiste

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)
 
[![Die Symbolleiste Aufrufattribute](introduction-images/11-constraintstoolbar-vsmac.png "der Einschränkungen-Symbolleiste")](introduction-images/11-constraintstoolbar-vsmac-large.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Die Symbolleiste Aufrufattribute](introduction-images/11-constraintstoolbar-vs.png "der Einschränkungen-Symbolleiste")](introduction-images/11-constraintstoolbar-vs-large.png#lightbox)

-----

Die Einschränkungen-Symbolleiste aktualisiert wurde und jetzt besteht aus zwei Steuerelemente: Bearbeitungsmodus Frames / Einschränkung Modus ein-/ausschalten und die Update-Einschränkungen Bearbeiten / Aktualisieren der Frames-Schaltfläche.

#### <a name="frame-editing-mode--constraint-editing-mode-toggle"></a>Frame Vorlagenbearbeitungsmodus / Einschränkung bearbeiten Modus ein-/ausschalten

In früheren Versionen des iOS-Designer ein-/ausgeschaltet zwischen Rahmen und Einschränkung bearbeiten Modus bearbeiten Sie einen bereits ausgewählte Ansicht auf der Entwurfsoberfläche auf. Jetzt, schaltet ein umschaltbaren Steuerelements auf der Symbolleiste Einschränkungen zwischen diesen Modi bearbeiten.

- Frame-Modus:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

![Schaltfläche Bearbeiten Frame](introduction-images/12a-frameeditingmode-vsmac.png "Frame-Schaltfläche \"Bearbeiten\" \"")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Schaltfläche Bearbeiten Frame](introduction-images/12a-frameeditingmode-vs.png "Frame-Schaltfläche \"Bearbeiten\" \"")

-----

- Einschränkung Modus:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

![Einschränkung Bearbeiten Schaltfläche Modus](introduction-images/12b-constrainteditingmode-vsmac.png "Einschränkung bearbeiten-Modus-Schaltfläche")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Einschränkung Bearbeiten Schaltfläche Modus](introduction-images/12b-constrainteditingmode-vs.png "Einschränkung bearbeiten-Modus-Schaltfläche")

-----

#### <a name="update-constraints--update-frames-button"></a>Einschränkungen bei der Aktualisierung / Frames Schaltfläche "Aktualisieren"

Die Einschränkungen für die Update / Update frames Schaltfläche befindet sich rechts neben den Frame Vorlagenbearbeitungsmodus / Einschränkung bearbeiten Modus ein-/ausschalten.

- Im Frame Vorlagenbearbeitungsmodus passt klicken auf diese Schaltfläche die Frames alle ausgewählten Elemente werden entsprechend ihren Einschränkungen aus.
- Im Bearbeitungsmodus Einschränkung passt das Klicken auf diese Schaltfläche die Einschränkungen alle ausgewählten Elemente werden entsprechend ihrer Frames aus.

### <a name="bottom-toolbar"></a>Unteren Symbolleiste

Die unteren Symbolleiste bietet die Möglichkeit, wählen Sie das Gerät, Ausrichtung und einen Zoom verwendet, um ein Storyboard oder .xib-Datei in der iOS-Designer anzeigen:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Der unteren Symbolleiste verwendet, um ein Gerät und die Ausrichtung für die Entwurfsoberfläche auswählen](introduction-images/13-bottomtoolbar-vsmac.png "der unteren Symbolleiste verwendet, um ein Gerät und die Ausrichtung für die Entwurfsoberfläche auswählen")](introduction-images/13-bottomtoolbar-vsmac-large.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Der unteren Symbolleiste verwendet, um ein Gerät und die Ausrichtung für die Entwurfsoberfläche auswählen](introduction-images/13-bottomtoolbar-vs.png "der unteren Symbolleiste verwendet, um ein Gerät und die Ausrichtung für die Entwurfsoberfläche auswählen")](introduction-images/13-bottomtoolbar-vs-large.png#lightbox)

-----

#### <a name="device-and-orientation"></a>Geräte- und Ausrichtung

Wenn die Kategorie erweitert ist, zeigt die unteren Symbolleiste alle Geräte, Ausrichtungen und/oder Anpassung gilt für das aktuelle Dokument. Klicken Sie auf diese Änderungen in der Ansicht auf der Entwurfsoberfläche angezeigt. 

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Die Symbolleiste unten erweitert, sodass Geräte und Ausrichtungen](introduction-images/14-bottomtoolbarexpanded-vsmac.png "der unteren Symbolleiste erweitert, um Geräte und Ausrichtungen anzuzeigen.")](introduction-images/14-bottomtoolbarexpanded-vsmac-large.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Die Symbolleiste unten erweitert, sodass Geräte und Ausrichtungen](introduction-images/14-bottomtoolbarexpanded-vs.png "der unteren Symbolleiste erweitert, um Geräte und Ausrichtungen anzuzeigen.")](introduction-images/14-bottomtoolbarexpanded-vs-large.png#lightbox)

-----

Hinweis: Auswählen eines Geräts und die Ausrichtung ändert nur wie iOS-Designer den Entwurf in der Vorschau anzeigt. Unabhängig von der aktuellen Auswahl neu hinzugefügten Einschränkungen gelten für alle Geräte sowie Ausrichtungen, es sei denn, die **bearbeiten Traits** Schaltfläche wurde verwendet, um nichts anderes angeben.

Wenn [Größe Klassen](~/ios/user-interface/storyboards/unified-storyboards.md#size-classes) sind [aktiviert](~/ios/user-interface/storyboards/unified-storyboards.md#enabling-size-classes), die **bearbeiten Traits** Schaltfläche erscheint in der erweiterten unteren Symbolleiste.  Klicken auf die **bearbeiten Traits** Schaltfläche zeigt Optionen für das Erstellen einer Schnittstelle Variation basierend auf der Größe-Klasse, die durch das ausgewählte Gerät und Ausrichtung dargestellt. Betrachten Sie die folgenden Beispiele:

- Wenn **iPhone SE** / **Hochformat**, wird ausgewählt, die Popover bietet Optionen zum Erstellen einer Schnittstelle Variation compact Breite, Höhe reguläre Größe-Klasse. 
- Wenn **iPhone Pro 9.7"** / **Querformat** / **Vollbild** ist ausgewählt, die Popover bietet Optionen zum Erstellen einer Schnittstelle Variation für der normale Breite, Höhe reguläre Größe-Klasse.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Der unteren Symbolleiste, um eine Schnittstelle zu variieren von Größe Klasse verwendeten](introduction-images/15-edittraitsbutton-vsmac.png "der unteren Symbolleiste, um eine Schnittstelle zu variieren von Größe-Klasse verwendet wird")](introduction-images/15-edittraitsbutton-vsmac-large.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Der unteren Symbolleiste, um eine Schnittstelle zu variieren von Größe Klasse verwendeten](introduction-images/15-edittraitsbutton-vs.png "der unteren Symbolleiste, um eine Schnittstelle zu variieren von Größe-Klasse verwendet wird")](introduction-images/15-edittraitsbutton-vs-large.png#lightbox)

-----

#### <a name="zoom-controls"></a>Zoom-Steuerelementen

Die Entwurfsoberfläche unterstützt Zoomen über mehrere Steuerelemente:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)
 
![Die Zoomsteuerelemente in der unteren Symbolleiste](introduction-images/16-zoomcontrols-vsmac.png "die Zoomsteuerelemente in der unteren Symbolleiste")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Die Zoomsteuerelemente in der unteren Symbolleiste](introduction-images/16-zoomcontrols-vs.png "die Zoomsteuerelemente in der unteren Symbolleiste")

-----

Die Steuerelemente umfassen Folgendes:

1. Zoom anpassen
2. Verkleinern
3. Vergrößern
4. Tatsächliche Größe (Größe in Pixel für 1:1)

Diese Steuerelemente passen Sie den Zoom auf der Entwurfsoberfläche angezeigt. Sie wirken sich nicht auf die Benutzeroberfläche der Anwendung zur Laufzeit aus.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

### <a name="properties-pad"></a>Eigenschaften mit Leerstellen auffüllen

Verwenden der **Eigenschaften Pad** so bearbeiten Sie die Identität, visuelle Stile Eingabehilfen und das Verhalten eines Steuerelements. Der folgende Screenshot veranschaulicht die **Eigenschaften Pad** Optionen für eine Schaltfläche:

[![Das Auffüllzeichen Eigenschaften für eine Schaltfläche](introduction-images/17-buttonpropertiespad-vsmac.png "der Eigenschaften Pad für eine Schaltfläche")](introduction-images/17-buttonpropertiespad-vsmac-large.png#lightbox)
#### <a name="properties-pad-sections"></a>Eigenschaften Pad-Abschnitte

Die **Eigenschaften Pad** enthält drei Abschnitte:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

### <a name="properties-window"></a>Eigenschaftenfenster

Verwenden der **Fenster "Eigenschaften"** so bearbeiten Sie die Identität, visuelle Stile Eingabehilfen und das Verhalten eines Steuerelements. Der folgende Screenshot veranschaulicht die **Fenster "Eigenschaften"** Optionen für eine Schaltfläche:

[![Das Eigenschaftenfenster für eine Schaltfläche](introduction-images/17-buttonpropertieswindow-vs.png "das Eigenschaftenfenster für eine Schaltfläche")](introduction-images/17-buttonpropertieswindow-vs-large.png#lightbox)

#### <a name="properties-window-sections"></a>Eigenschaften-Fenster-Abschnitte

Die **Fenster "Eigenschaften"** enthält drei Abschnitte:

-----

1.  **Widget** – die Haupteigenschaften des Steuerelements, wie Name, Klasse, Stileigenschaften. Eigenschaften für die Verwaltung von Inhalt des Steuerelements werden hier in der Regel platziert.
2.  **Layout** – Eigenschaften, die nachverfolgen die Position und Größe des Steuerelements, einschließlich Einschränkungen und Frames, werden hier aufgelistet.
3.  **Ereignisse** – Ereignisse und Ereignishandler sind hier angegeben. Für die Behandlung von Ereignissen wie Toucheingabe, tippen, ziehen Sie nützlich. Ereignisse können auch direkt im Code verarbeitet werden.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

#### <a name="editing-properties-in-the-properties-pad"></a>Bearbeiten von Eigenschaften in den Eigenschaften mit Leerstellen auffüllen

Neben dem visuelle Bearbeitung auf der Entwurfsoberfläche angezeigt, die iOS-Designer unterstützt Bearbeiten von Eigenschaften in der **Eigenschaften Pad**. Die verfügbaren Eigenschaften ändern sich des ausgewählten Steuerelements abhängig, wie in den nachstehenden Screenshots veranschaulicht:

[![Schaltfläche Eigenschaften](introduction-images/18a-buttonpropertiespad-vsmac.png "Schaltfläche Eigenschaften")](introduction-images/18a-buttonpropertiespad-vsmac-large.png#lightbox)

[![Anzeigen der Eigenschaften von Domänencontroller](introduction-images/18b-viewcontrollerpropertiespad-vsmac.png "Controller Eigenschaften anzeigen")](introduction-images/18b-viewcontrollerpropertiespad-vsmac-large.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

#### <a name="editing-properties-in-the-properties-window"></a>Bearbeiten von Eigenschaften im Eigenschaftenfenster

Neben dem visuelle Bearbeitung auf der Entwurfsoberfläche angezeigt, die iOS-Designer unterstützt Bearbeiten von Eigenschaften in der **Fenster "Eigenschaften"**. Die verfügbaren Eigenschaften ändern sich des ausgewählten Steuerelements abhängig, wie in den nachstehenden Screenshots veranschaulicht:

[![Schaltfläche Eigenschaften](introduction-images/18a-buttonpropertieswindow-vs.png "Schaltfläche Eigenschaften")](introduction-images/18a-buttonpropertieswindow-vs-large.png#lightbox)

[![Anzeigen der Eigenschaften von Domänencontroller](introduction-images/18b-viewcontrollerpropertieswindow-vs.png "Controller Eigenschaften anzeigen")](introduction-images/18b-viewcontrollerpropertieswindow-vs-large.png#lightbox)

-----

> [!IMPORTANT]
> Der Identitätsabschnitt des das Eigenschaften Pad jetzt zeigt eine **Modul** Feld. Es ist notwendig, füllen in diesem Abschnitt nur bei der Interaktion mit Swift-Klassen. Verwenden sie den Namen eines Moduls für Swift Klassen Geben Sie die Serversteuerelement sind.

#### <a name="default-values"></a>Standardwerte

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Viele Eigenschaften in der **Eigenschaften Pad** zeigen kein Wert oder einen Standardwert zurück. Allerdings kann der Code der Anwendung weiterhin diese Werte ändern. Die **Eigenschaften Pad** zeigt nicht die Werte, die im Code festlegen.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Viele Eigenschaften in der **Fenster "Eigenschaften"** zeigen kein Wert oder einen Standardwert zurück. Allerdings kann der Code der Anwendung weiterhin diese Werte ändern. Die **Fenster "Eigenschaften"** zeigt nicht die Werte, die im Code festlegen.

-----

#### <a name="event-handlers"></a>Ereignishandler

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Um benutzerdefinierte Ereignishandler für die verschiedenen Ereignisse anzugeben, verwenden die **Ereignisse** auf der Registerkarte die **Eigenschaften Pad**. Z. B. in folgender Screenshot einer `HandleClick` Methode verarbeitet der Schaltfläche **berühren sich innerhalb** Ereignis:

[![Die Auffüllzeichen Eigenschaften mit einem Ereignishandler für eine Schaltfläche festlegen](introduction-images/19-buttonpropertiespadevents-vsmac.png "der Eigenschaften Pad mit einem Ereignishandler für eine Schaltfläche festlegen")](introduction-images/19-buttonpropertiespadevents-vsmac-large.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Verwenden Sie zum Angeben benutzerdefinierter Ereignishandler für verschiedene Ereignisse aus der **Ereignisse** auf der Registerkarte die **Fenster "Eigenschaften"**. Z. B. in folgender Screenshot einer `HandleClick` Methode verarbeitet der Schaltfläche **berühren sich innerhalb** Ereignis:

[![Eigenschaftenfenster mit einem Ereignishandler für eine Schaltfläche festlegen](introduction-images/19-buttonpropertieswindowevents-vs.png "das Eigenschaftenfenster, einem Ereignishandler für eine Schaltfläche festlegen")](introduction-images/19-buttonpropertieswindowevents-vs-large.png#lightbox)

-----

Sobald ein Ereignishandler angegeben wurde, muss der entsprechende View Controller-Klasse eine Methode mit dem gleichen Namen hinzugefügt werden. Andernfalls ein `unrecognized selector` Ausnahme tritt auf, wenn die Schaltfläche "" abgerufen werden:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Eine Ausnahme nicht erkannten Selektor](introduction-images/20-unrecognizedselector-vsmac.png "eine unbekannte Auswahlzeiger-Ausnahme")](introduction-images/20-unrecognizedselector-vsmac-large.png#lightbox)

Beachten Sie, die nach einem Ereignishandler in angegeben wurde die **Eigenschaften aufgefüllt**, die iOS-Designer wird sofort öffnen Sie die entsprechenden Codedatei und bieten die Methodendeklaration eingefügt. 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Eine Ausnahme nicht erkannten Selektor](introduction-images/20-unrecognizedselector-vs.png "eine unbekannte Auswahlzeiger-Ausnahme")](introduction-images/20-unrecognizedselector-vs-large.png#lightbox)

-----

Ein Beispiel, benutzerdefinierte Ereignishandler verwendet, finden Sie in der [Hello, iOS-erste Schritte](~/ios/get-started/hello-ios/index.md).

### <a name="outline-view"></a>Gliederungsansicht

Die iOS-Designer können auch eine Schnittstellenhierarchie von Steuerelementen als Gliederung anzeigen. Die Gliederung ist verfügbar durch Auswahl der **Dokumentgliederung** Registerkarte wie folgt:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Die Dokumentgliederung](introduction-images/21-buttonoutlineview-vsmac.png "der Dokumentgliederung")](introduction-images/21-buttonoutlineview-vsmac-large.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Die Dokumentgliederung](introduction-images/21-buttonoutlineview-vs.png "der Dokumentgliederung")](introduction-images/21-buttonoutlineview-vs-large.png#lightbox)

-----

Das ausgewählte Steuerelement in der Gliederungsansicht bleibt synchron mit dem ausgewählten Steuerelement auf der Entwurfsoberfläche angezeigt.  Diese Funktion ist nützlich für das Auswählen eines Elements aus einer Schnittstellenhierarchie in der Schachtelungshierarchie.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

## <a name="revert-to-xcode"></a>Kehren Sie zu Xcode

Es ist möglich, den iOS-Designer und Xcode Schnittstelle-Generator Synonym zu verwenden. Um ein Storyboard oder eine .xib-Datei in Xcode Schnittstelle-Generator zu öffnen, mit der Maustaste, auf die Datei, und wählen **Öffnen mit > Xcode Schnittstelle-Generator**, wie der folgende Screenshot veranschaulicht:

[![Öffnen ein Storyboard im Xcode Schnittstelle-Generator](introduction-images/22-openinxcodeinterfacebuilder-vsmac.png "ein Storyboard in Xcode Schnittstelle-Generator öffnen")](introduction-images/22-openinxcodeinterfacebuilder-vsmac-large.png#lightbox)

Nach Änderungen in Xcode Schnittstelle-Generator vornehmen, die Datei speichern und zurück zu Visual Studio für Mac. Die Änderungen werden dem Projekt Xamarin.iOS synchronisiert.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

-----

## <a name="xib-support"></a>.XIB-Unterstützung

Die iOS-Designer unterstützt erstellen, bearbeiten und Verwalten von .xib-Dateien. Diese sind XML-Dateien, geschlossenes einzelne, benutzerdefinierte Ansichten, die Hierarchie von einer Anwendung anzeigen hinzugefügt werden können. Eine .xib-Datei stellt die Schnittstelle für eine einzelne Ansicht oder einen Bildschirm in einer Anwendung in der Regel dar, während ein Drehbuchs viele Bildschirme und die Übergänge zwischen diesen darstellt.

Es gibt viele Meinung dazu, die welche Lösung – .xib-Dateien, Storyboards oder Code – am besten zum Erstellen und verwalten eine neue Benutzeroberfläche funktioniert. In Wirklichkeit besteht keine perfekten Lösung, und es ist immer das beste Tool für den Auftrag zur hand Überlegung Wert. Dies bedeutet, dass .xib Dateien im Allgemeinen leistungsstärksten beim zur Darstellung von einer benutzerdefinierten Ansicht an mehreren Stellen in eine app, wie z. B. eine benutzerdefinierte Ansicht Tabellenzelle erforderlich sind. 

Weitere Dokumentation zur Verwendung von .xib-Dateien kann in den folgenden Rezepte gefunden werden:

- [Verwenden Sie die Ansicht .xib-Vorlage](https://developer.xamarin.com/recipes/ios/general/templates/using_the_ios_view_xib_template/)
- [Erstellen eine benutzerdefinierte TableViewCell mithilfe einer .xib](https://developer.xamarin.com/recipes/ios/content_controls/tables/custom-tableviewcell/)
- [Erstellen eines mit einem .xib starten-Bildschirms](https://developer.xamarin.com/recipes/ios/general/templates/launchscreen-xib/)

Weitere Informationen zur Verwendung von Storyboards finden Sie in der [Einführung in Storyboards](~/ios/user-interface/storyboards/index.md).

Diese und andere iOS-Designer-bezogene Handbücher finden Sie in die Verwendung von Storyboards als standard Ansatz zum Erstellen von Benutzeroberflächen, da neue Projektvorlagen für die meisten Xamarin.iOS ein Storyboard standardmäßig enthalten.

## <a name="summary"></a>Zusammenfassung

Dieses Handbuch bereitgestellt, eine Einführung in die iOS-Designer, beschreiben seine Funktionen und Gliedern die Tools zum Entwerfen von überzeugender Benutzeroberfläche bietet.

## <a name="related-links"></a>Verwandte Links

- [Einführung in Storyboards](~/ios/user-interface/storyboards/index.md)
- [iOS entworfen Steuerelemente Exemplarische Vorgehensweise](~/ios/user-interface/designer/ios-designable-controls-walkthrough.md)
- [Hello, iOS](~/ios/get-started/hello-ios/index.md)
- [Hello, iOS Multiscreen](~/ios/get-started/hello-ios-multiscreen/index.md)
- [Übersicht über Android-Designer](~/android/user-interface/android-designer/index.md)
- [Partielle Klassen und Methoden](https://docs.microsoft.com/dotnet/csharp/programming-guide/classes-and-structs/partial-classes-and-methods)
- [Einstieg in die Xamarin-Designer für iOS - weiterentwickelt 2014 (video)](https://www.youtube.com/watch?v=W4H9uLjoEjM)
- [Verwenden die iOS-Designer zum eines Bildschirms starten (video) erstellen](https://university.xamarin.com/lightninglectures/using-the-ios-designer-to-create-a-launch-screen)
