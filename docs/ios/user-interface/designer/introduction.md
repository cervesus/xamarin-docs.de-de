---
title: IOS Designer-Grundlagen
description: In diesem Leitfaden werden die Xamarin Designer für IOS vorgestellt. Es wird veranschaulicht, wie der IOS-Designer verwendet wird, um Steuerelemente visuell zu gestalten, um auf diese Steuerelemente im Code zuzugreifen und um Eigenschaften zu bearbeiten.
ms.prod: xamarin
ms.assetid: E7045E41-0DEF-416B-BCDB-52502350F61C
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 01/31/2018
ms.openlocfilehash: 6b02a0f8476cf47ca6df279653095fe0845b36c9
ms.sourcegitcommit: eedc6032eb5328115cb0d99ca9c8de48be40b6fa
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/07/2020
ms.locfileid: "78914750"
---
# <a name="ios-designer-basics"></a>IOS Designer-Grundlagen

_In diesem Leitfaden werden die Xamarin Designer für IOS vorgestellt. Es wird veranschaulicht, wie der IOS-Designer verwendet wird, um Steuerelemente visuell zu gestalten, um auf diese Steuerelemente im Code zuzugreifen und um Eigenschaften zu bearbeiten._

Der Xamarin Designer für IOS ist ein visueller Schnittstellen Designer, der dem Interface Builder von Xcode und dem Android Designer ähnelt. Zu den zahlreichen Features zählen die nahtlose Integration in Visual Studio für Windows und Mac, Drag & Drop-Bearbeitung, eine Schnittstelle zum Einrichten von Ereignis Handlern und die Möglichkeit, benutzerdefinierte Steuerelemente zu Rendering.

## <a name="requirements"></a>Requirements (Anforderungen)

Der IOS-Designer ist in Visual Studio für Mac und Visual Studio 2017 und höher unter Windows verfügbar. In Visual Studio für Windows benötigt der IOS-Designer eine Verbindung mit einem ordnungsgemäß konfigurierten Mac-buildhost, obwohl Xcode nicht ausgeführt werden muss.

In diesem Leitfaden wird davon ausgegangen, dass Sie sich mit den Inhalten vertraut machen, die in den [Leitfäden zu](~/ios/get-started/index.md)

<a name="how-it-works" />

## <a name="how-the-ios-designer-works"></a>Funktionsweise des IOS-Designers

In diesem Abschnitt wird beschrieben, wie der IOS-Designer das Erstellen einer Benutzeroberfläche und das Verbinden mit dem Code erleichtert.

Der IOS-Designer ermöglicht es Entwicklern, die Benutzeroberfläche einer Anwendung visuell zu entwerfen. Wie im Leitfaden [Introduction to Storyboards (Einführung in Storyboards](~/ios/user-interface/storyboards/index.md) ) erläutert, beschreibt ein Storyboard die Bildschirme (Ansichts Controller), aus denen eine APP besteht, die Schnittstellen Elemente (Ansichten), die auf diesen Ansichts Controllern platziert werden, und den gesamten Navigations Fluss der app. 

Ein Ansichts Controller besteht aus zwei Teilen: einer visuellen Darstellung im IOS-Designer und C# einer zugeordneten Klasse:

<!-- markdownlint-disable MD001 -->

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

[![Einen Ansichts Controller im IOS-Designer](introduction-images/1-storyboardwithviewcontroller-vsmac.png "Einen Ansichts Controller im IOS-Designer")](introduction-images/1-storyboardwithviewcontroller-vsmac-large.png#lightbox)

[![Der Code für einen Ansichts Controller.](introduction-images/2-viewcontrollercode-vsmac.png "Der Code für einen Ansichts Controller.")](introduction-images/2-viewcontrollercode-vsmac-large.png#lightbox)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[![Einen Ansichts Controller im IOS-Designer](introduction-images/1-storyboardwithviewcontroller-vs.png "Einen Ansichts Controller im IOS-Designer")](introduction-images/1-storyboardwithviewcontroller-vs-large.png#lightbox)

[![Der Code für einen Ansichts Controller.](introduction-images/2-viewcontrollercode-vs.png "Der Code für einen Ansichts Controller.")](introduction-images/2-viewcontrollercode-vs-large.png#lightbox)

-----

In seinem Standardzustand stellt ein Ansichts Controller keine Funktionen bereit. Es muss mit-Steuerelementen aufgefüllt werden. Diese Steuerelemente werden in der Ansicht des Ansichts Controllers platziert, der rechteckige Bereich, der den gesamten Inhalt des Bildschirms enthält. Die meisten Ansichts Controller enthalten allgemeine Steuerelemente, wie z. b. Schaltflächen, Bezeichnungen und Textfelder, wie im folgenden Screenshot gezeigt, der einen Ansichts Controller mit einer Schaltfläche anzeigt: 

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

[![Ein Ansichts Controller, der eine Schaltfläche enthält](introduction-images/3-viewcontrollerwithbutton-vsmac.png "Ein Ansichts Controller, der eine Schaltfläche enthält")](introduction-images/3-viewcontrollerwithbutton-vsmac-large.png#lightbox)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[![Ein Ansichts Controller, der eine Schaltfläche enthält](introduction-images/3-viewcontrollerwithbutton-vs.png "Ein Ansichts Controller, der eine Schaltfläche enthält")](introduction-images/3-viewcontrollerwithbutton-vs-large.png#lightbox)

-----

Einige Steuerelemente, wie z. b. Bezeichnungen mit statischem Text, können dem Ansichts Controller und der linken Seite hinzugefügt werden. In den meisten Fällen müssen Steuerelemente jedoch Programm gesteuert angepasst werden. Beispielsweise sollte die oben hinzugefügte Schaltfläche etwas tun, wenn Sie getippt wird, sodass ein Ereignishandler im Code hinzugefügt werden muss.

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

Um auf die Schaltfläche im Code zuzugreifen und Sie zu bearbeiten, muss Sie über einen eindeutigen Bezeichner verfügen. Geben Sie einen eindeutigen Bezeichner an, indem Sie die Schaltfläche auswählen, die **Eigenschaftenpad**öffnen und das Feld **Name** auf einen Wert wie "SubmitButton" festlegen:

[![Festlegen des Namens einer Schaltfläche im Eigenschaftenpad](introduction-images/4-settingbuttonname-vsmac.png "Festlegen des Namens einer Schaltfläche im Eigenschaftenpad")](introduction-images/4-settingbuttonname-vsmac-large.png#lightbox)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Um auf die Schaltfläche im Code zuzugreifen und Sie zu bearbeiten, muss Sie über einen eindeutigen Bezeichner verfügen. Geben Sie einen eindeutigen Bezeichner an, indem Sie die Schaltfläche auswählen, das **Eigenschaften Fenster**öffnen und das **Name** -Feld auf einen Wert wie "SubmitButton" festlegen:

[![Festlegen des Namens einer Schaltfläche im Eigenschaften Fenster](introduction-images/4-settingbuttonname-vs.png "Festlegen des Namens einer Schaltfläche im Eigenschaften Fenster")](introduction-images/4-settingbuttonname-vs-large.png#lightbox)

-----

Nachdem die Schaltfläche nun über einen Namen verfügt, können Sie im Code darauf zugreifen. Aber wie funktioniert das?

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

Im **Lösungspad**wird durch Navigieren zu **ViewController.cs** und durch Klicken auf den Offenlegungs Indikator angezeigt, dass die `ViewController` Klassendefinition des Ansichts Controllers zwei Dateien umfasst, die jeweils eine [partielle Klassen](https://docs.microsoft.com/dotnet/csharp/programming-guide/classes-and-structs/partial-classes-and-methods) Definition enthalten:

[![Die beiden Dateien, die die ViewController-Klasse bilden: ViewController.cs und ViewController.Designer.cs](introduction-images/5-twoviewcontrollerfiles-vsmac.png "Die beiden Dateien, die die ViewController-Klasse bilden: ViewController.cs und ViewController.Designer.cs")](introduction-images/5-twoviewcontrollerfiles-vsmac-large.png#lightbox)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Im **Projektmappen-Explorer**wird durch Navigieren zu **ViewController.cs** und durch Klicken auf den Offenlegungs Indikator angezeigt, dass die `ViewController` Klassendefinition des Ansichts Controllers zwei Dateien umfasst, die jeweils eine [partielle Klassen](https://docs.microsoft.com/dotnet/csharp/programming-guide/classes-and-structs/partial-classes-and-methods) Definition enthalten:

[![Die beiden Dateien, die die ViewController-Klasse bilden: ViewController.cs und ViewController.Designer.cs](introduction-images/5-twoviewcontrollerfiles-vs.png "Die beiden Dateien, die die ViewController-Klasse bilden: ViewController.cs und ViewController.Designer.cs")](introduction-images/5-twoviewcontrollerfiles-vs-large.png#lightbox)

-----

- **ViewController.cs** sollte mit benutzerdefiniertem Code aufgefüllt werden, der mit der `ViewController`-Klasse verknüpft ist. In dieser Datei kann die `ViewController`-Klasse auf verschiedene IOS-Ansichts Controller-Lebenszyklus Methoden reagieren, die Benutzeroberfläche anpassen und auf Benutzereingaben, wie z. b. Schaltflächen tippen, reagieren.

- **ViewController.Designer.cs** ist eine generierte Datei, die vom IOS-Designer erstellt wurde, um die visuell erstellte Schnittstelle dem Code zuzuordnen. Da Änderungen an dieser Datei überschrieben werden, sollte Sie nicht geändert werden. Eigenschafts Deklarationen in dieser Datei ermöglichen es dem Code in der `ViewController`-Klasse **, über die**im IOS-Designer eingerichteten Steuerelemente auf die Steuerelemente zuzugreifen. Öffnen **ViewController.Designer.cs** zeigt den folgenden Code:

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

Die `SubmitButton`-Eigenschafts Deklaration verbindet die gesamte `ViewController`-Klasse, nicht nur die **ViewController.Designer.cs** -Datei – mit der im Storyboard definierten Schaltfläche. Da **ViewController.cs** einen Teil der `ViewController`-Klasse definiert, hat er Zugriff auf `SubmitButton`.

Der folgende Screenshot zeigt, dass IntelliSense jetzt den `SubmitButton` Verweis in **ViewController.cs**erkennt:

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

[![IntelliSense, das den SubmitButton-Verweis erkennt](introduction-images/6-submitbuttonintellisense-vsmac.png "IntelliSense, das den SubmitButton-Verweis erkennt")](introduction-images/6-submitbuttonintellisense-vsmac-large.png#lightbox)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[![IntelliSense, das den SubmitButton-Verweis erkennt](introduction-images/6-submitbuttonintellisense-vs.png "IntelliSense, das den SubmitButton-Verweis erkennt")](introduction-images/6-submitbuttonintellisense-vs-large.png#lightbox)

-----

In diesem Abschnitt wurde gezeigt, wie Sie eine Schaltfläche im IOS-Designer erstellen und auf diese Schaltfläche im Code zugreifen.

Im restlichen Teil dieses Dokuments finden Sie eine Übersicht über den IOS-Designer.

## <a name="ios-designer-basics"></a>IOS Designer-Grundlagen

Dieser Abschnitt enthält eine Einführung in die Bestandteile des IOS-Designers und bietet eine Einführung in seine Features.

### <a name="launching-the-ios-designer"></a>Starten des IOS-Designers

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

Xamarin. IOS-Projekte, die mit Visual Studio für Mac erstellt wurden, enthalten ein Storyboard. Um den Inhalt eines Storyboards anzuzeigen, doppelklicken Sie auf die Storyboard-Datei im **Lösungspad**:

[![Ein Storyboard im IOS-Designer geöffnet](introduction-images/7-storyboardopen-vsmac.png "Ein Storyboard im IOS-Designer geöffnet")](introduction-images/7-storyboardopen-vsmac-large.png#lightbox)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Die meisten xamarin. IOS-Projekte, die mit Visual Studio erstellt wurden, enthalten ein Storyboard. Um den Inhalt eines Storyboards anzuzeigen, doppelklicken Sie auf die Storyboard-Datei im **Projektmappen-Explorer**:

[![Ein Storyboard im IOS-Designer geöffnet](introduction-images/7-storyboardopen-vs.png "Ein Storyboard im IOS-Designer geöffnet")](introduction-images/7-storyboardopen-vs-large.png#lightbox)

-----

<a name="iOS_Designer_features"/>

### <a name="ios-designer-features"></a>IOS-Designer-Funktionen

Der IOS-Designer hat sechs primäre Abschnitte:

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

[![Abschnitte des IOS-Designers](introduction-images/8-sixpartsofiosdesigner-vsmac.png "Abschnitte des IOS-Designers")](introduction-images/8-sixpartsofiosdesigner-vsmac-large.png#lightbox)

1. **Designoberfläche** – der primäre Arbeitsbereich des IOS-Designers. Dies wird im Dokumentbereich angezeigt und ermöglicht die visuelle Erstellung von Benutzeroberflächen.
2. **Einschränkungs Symbolleiste** – ermöglicht das Wechseln zwischen dem Frame Bearbeitungsmodus und dem Einschränkungs Bearbeitungsmodus, zwei unterschiedliche Möglichkeiten zum Positionieren von Elementen in einer Benutzeroberfläche.
3. **Toolbox** – listet die Controller, Objekte, Steuerelemente, Datenansichten, Gesten erkenungen, Fenster und Balken auf, die auf die Entwurfs Oberfläche gezogen und einer Benutzeroberfläche hinzugefügt werden können.
4. **Eigenschaftenpad** – zeigt die Eigenschaften des ausgewählten Steuer Elements an, einschließlich Identität, visueller Stile, Barrierefreiheit, Layout und Verhalten.
5. **Dokument** Gliederung – zeigt die Struktur der Steuerelemente an, die das Layout für die zu bearbeitende Schnittstelle bilden. Wenn Sie auf ein Element in der Struktur klicken, wird es im IOS-Designer ausgewählt und seine Eigenschaften in der **Eigenschaftenpad**angezeigt. Dies ist praktisch, wenn Sie ein bestimmtes Steuerelement in einer tief geschachtelten Benutzeroberfläche auswählen.
6. **Untere Symbolleiste** – enthält Optionen zum Ändern der Anzeige der Storyboard-oder XIb-Datei durch den IOS-Designer, einschließlich Gerät, Ausrichtung und Zoom.

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[![Abschnitte des IOS-Designers](introduction-images/8-sixpartsofiosdesigner-vs.png "Abschnitte des IOS-Designers")](introduction-images/8-sixpartsofiosdesigner-vs-large.png#lightbox)

1. **Designoberfläche** – der primäre Arbeitsbereich des IOS-Designers. Dies wird im Dokumentbereich angezeigt und ermöglicht die visuelle Erstellung von Benutzeroberflächen.
2. **Einschränkungs Symbolleiste** – ermöglicht das Wechseln zwischen dem Frame Bearbeitungsmodus und dem Einschränkungs Bearbeitungsmodus, zwei unterschiedliche Möglichkeiten zum Positionieren von Elementen in einer Benutzeroberfläche.
3. **Toolbox** – listet die Controller, Objekte, Steuerelemente, Datenansichten, Gesten erkenungen, Fenster und Balken auf, die auf die Entwurfs Oberfläche gezogen und einer Benutzeroberfläche hinzugefügt werden können.
4. **Eigenschaften Fenster** – zeigt die Eigenschaften des ausgewählten Steuer Elements an, einschließlich Identität, visueller Stile, Barrierefreiheit, Layout und Verhalten.
5. **Dokument** Gliederung – zeigt die Struktur der Steuerelemente an, die das Layout für die zu bearbeitende Schnittstelle bilden. Wenn Sie auf ein Element in der Struktur klicken, wird es im IOS-Designer ausgewählt und seine Eigenschaften im **Fenster Eigenschaften**angezeigt. Dies ist praktisch, wenn Sie ein bestimmtes Steuerelement in einer tief geschachtelten Benutzeroberfläche auswählen.
6. **Untere Symbolleiste** – enthält Optionen zum Ändern der Anzeige der Storyboard-oder XIb-Datei durch den IOS-Designer, einschließlich Gerät, Ausrichtung und Zoom.

-----

### <a name="design-workflow"></a>Workflow entwerfen

#### <a name="adding-a-control-to-the-interface"></a>Hinzufügen eines Steuer Elements zur Schnittstelle

Wenn Sie einer Schnittstelle ein Steuerelement hinzufügen möchten, ziehen Sie es aus der **Toolbox** , und legen Sie es auf der Entwurfs Oberfläche ab. Beim Hinzufügen oder Positionieren eines Steuer Elements werden durch vertikale und horizontale Richtlinien häufig verwendete Layoutpositionen hervorgehoben, wie z. b. vertikale Mitte, horizontale Mitte und Ränder:

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

![Auf der Entwurfs Oberfläche markieren Richtlinien häufig verwendete Layoutpositionen.](introduction-images/9-layoutguides-vsmac.png "Auf der Entwurfs Oberfläche markieren Richtlinien häufig verwendete Layoutpositionen.")

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

![Auf der Entwurfs Oberfläche markieren Richtlinien häufig verwendete Layoutpositionen.](introduction-images/9-layoutguides-vs.png "Auf der Entwurfs Oberfläche markieren Richtlinien häufig verwendete Layoutpositionen.")

-----

Die blaue gepunktete Linie im obigen Beispiel bietet eine horizontale Ausrichtung der visuellen Ausrichtung auf die Schaltfläche.

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

#### <a name="context-menu-commands"></a>Kontextmenü Befehle

Ein Kontextmenü ist sowohl auf der Entwurfs Oberfläche als auch in der **Dokument**Gliederung verfügbar. Dieses Menü enthält Befehle für das ausgewählte Steuerelement und das zugehörige übergeordnete Element, das beim Arbeiten mit Ansichten in einer Hierarchie mit einer Hierarchie hilfreich ist:

[![Das Kontextmenü auf der Entwurfs Oberfläche](introduction-images/10-contextmenudesignsurface-vsmac.png "Das Kontextmenü auf der Entwurfs Oberfläche")](introduction-images/10-contextmenudesignsurface-vsmac-large.png#lightbox)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

-----

### <a name="constraints-toolbar"></a>Einschränkungs Symbolleiste

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

[![Die Symbolleiste "Configuration Manager"](introduction-images/11-constraintstoolbar-vsmac.png "Die Einschränkungs Symbolleiste")](introduction-images/11-constraintstoolbar-vsmac-large.png#lightbox)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[![Die Symbolleiste "Configuration Manager"](introduction-images/11-constraintstoolbar-vs.png "Die Einschränkungs Symbolleiste")](introduction-images/11-constraintstoolbar-vs-large.png#lightbox)

-----

Die Einschränkungs Symbolleiste wurde aktualisiert und umfasst nun zwei Steuerelemente: den Umschalter Frame-Bearbeitungsmodus/Einschränkungs Bearbeitungsmodus und Update Einschränkungen/Aktualisierungs Rahmen.

#### <a name="frame-editing-mode--constraint-editing-mode-toggle"></a>Frame Bearbeitungsmodus/Einschränkungs Bearbeitungsmodus umschalten

In früheren Versionen des IOS-Designers wurde durch Klicken auf eine bereits ausgewählte Ansicht auf der Entwurfs Oberfläche zwischen dem Frame Bearbeitungsmodus und dem Einschränkungs Bearbeitungsmodus gewechselt. Nun wechselt ein UMSCHALT Steuerelement auf der Einschränkungs Symbolleiste zwischen diesen Bearbeitungsmodi.

- Frame Bearbeitungsmodus:

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

![Schaltfläche "Frame Bearbeitungsmodus"](introduction-images/12a-frameeditingmode-vsmac.png "Schaltfläche Frame Bearbeitungsmodus")

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

![Schaltfläche "Frame Bearbeitungsmodus"](introduction-images/12a-frameeditingmode-vs.png "Schaltfläche Frame Bearbeitungsmodus")

-----

- Einschränkungs Bearbeitungsmodus:

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

![Schaltfläche für Einschränkungs Bearbeitungsmodus](introduction-images/12b-constrainteditingmode-vsmac.png "Schaltfläche für Einschränkungs Bearbeitungsmodus")

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

![Schaltfläche für Einschränkungs Bearbeitungsmodus](introduction-images/12b-constrainteditingmode-vs.png "Schaltfläche für Einschränkungs Bearbeitungsmodus")

-----

#### <a name="update-constraints--update-frames-button"></a>Schaltfläche "Update Einschränkungen/Frames aktualisieren"

Die Schaltfläche Update Einschränkungen/Update Frames befindet sich rechts neben der UMSCHALT Fläche für den Bearbeitungsmodus für Frame/Einschränkung.

- Wenn Sie im Frame Bearbeitungsmodus auf diese Schaltfläche klicken, werden die Frames ausgewählter Elemente an ihre Einschränkungen angepasst.
- Wenn Sie im Bearbeitungsmodus der Einschränkung auf diese Schaltfläche klicken, werden die Einschränkungen aller ausgewählten Elemente an die entsprechenden Frames angepasst.

### <a name="bottom-toolbar"></a>Untere Symbolleiste

Die untere Symbolleiste bietet eine Möglichkeit, das Gerät, die Ausrichtung und den Zoom auszuwählen, die zum Anzeigen einer Storyboard-oder XIb-Datei im IOS-Designer verwendet werden:

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

[![Die untere Symbolleiste, die zum Auswählen eines Geräts und der Ausrichtung für die Entwurfs Oberfläche verwendet wird.](introduction-images/13-bottomtoolbar-vsmac.png "Die untere Symbolleiste, die zum Auswählen eines Geräts und der Ausrichtung für die Entwurfs Oberfläche verwendet wird.")](introduction-images/13-bottomtoolbar-vsmac-large.png#lightbox)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[![Die untere Symbolleiste, die zum Auswählen eines Geräts und der Ausrichtung für die Entwurfs Oberfläche verwendet wird.](introduction-images/13-bottomtoolbar-vs.png "Die untere Symbolleiste, die zum Auswählen eines Geräts und der Ausrichtung für die Entwurfs Oberfläche verwendet wird.")](introduction-images/13-bottomtoolbar-vs-large.png#lightbox)

-----

#### <a name="device-and-orientation"></a>Gerät und Ausrichtung

Wenn Sie erweitert ist, werden auf der unteren Symbolleiste alle Geräte, Ausrichtungen und/oder Anpassungen angezeigt, die für das aktuelle Dokument gelten. Wenn Sie darauf klicken, wird die auf der Entwurfs Oberfläche angezeigte Ansicht geändert. 

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

[![Die untere Symbolleiste, erweitert zum Anzeigen von Geräten und Ausrichtungen](introduction-images/14-bottomtoolbarexpanded-vsmac.png "Die untere Symbolleiste, erweitert zum Anzeigen von Geräten und Ausrichtungen")](introduction-images/14-bottomtoolbarexpanded-vsmac-large.png#lightbox)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[![Die untere Symbolleiste, erweitert zum Anzeigen von Geräten und Ausrichtungen](introduction-images/14-bottomtoolbarexpanded-vs.png "Die untere Symbolleiste, erweitert zum Anzeigen von Geräten und Ausrichtungen")](introduction-images/14-bottomtoolbarexpanded-vs-large.png#lightbox)

-----

Beachten Sie, dass beim Auswählen eines Geräts und einer Ausrichtung nur die Vorschau des Entwurfs durch den IOS-Designer geändert wird. Unabhängig von der aktuellen Auswahl werden neu hinzugefügte Einschränkungen auf alle Geräte und Ausrichtungen angewendet, es sei denn, die Schaltfläche zum **Bearbeiten von Merkmalen** wurde verwendet, um anderweitig anzugeben.

Wenn [Größenklassen](~/ios/user-interface/storyboards/unified-storyboards.md#size-classes) [aktiviert](~/ios/user-interface/storyboards/unified-storyboards.md#enabling-size-classes)sind, wird die Schaltfläche zum **Bearbeiten von Merkmalen** in der erweiterten unteren Symbolleiste angezeigt.  Wenn Sie auf die Schaltfläche " **Merkmale bearbeiten** " klicken, werden Optionen zum Erstellen einer Schnittstellen Variation basierend auf der Größenklasse angezeigt, die vom ausgewählten Gerät und der ausgewählten Ausrichtung Betrachten Sie die folgenden Beispiele:

- Wenn **iPhone SE** / Hochformat ausgewählt **ist, stellt**das popover Optionen zum Erstellen einer Schnittstellen Variation für die Klasse Compact Width, Regular Height size bereit. 
- Wenn **iPad pro 9,7 "**  / **Landscape** / **Vollbild** ausgewählt ist, stellt das popover Optionen zum Erstellen einer Schnittstellen Variation für die reguläre Breite, reguläre Height size-Klasse bereit.

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

[![Die untere Symbolleiste, die zur Variation einer Schnittstelle nach Größenklasse verwendet wird.](introduction-images/15-edittraitsbutton-vsmac.png "Die untere Symbolleiste, die zur Variation einer Schnittstelle nach Größenklasse verwendet wird.")](introduction-images/15-edittraitsbutton-vsmac-large.png#lightbox)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[![Die untere Symbolleiste, die zur Variation einer Schnittstelle nach Größenklasse verwendet wird.](introduction-images/15-edittraitsbutton-vs.png "Die untere Symbolleiste, die zur Variation einer Schnittstelle nach Größenklasse verwendet wird.")](introduction-images/15-edittraitsbutton-vs-large.png#lightbox)

-----

#### <a name="zoom-controls"></a>Zoom Steuerelemente

Die Entwurfs Oberfläche unterstützt das Zoomen über mehrere Steuerelemente:

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

![Die Zoom Steuerelemente in der unteren Symbolleiste](introduction-images/16-zoomcontrols-vsmac.png "Die Zoom Steuerelemente in der unteren Symbolleiste")

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

![Die Zoom Steuerelemente in der unteren Symbolleiste](introduction-images/16-zoomcontrols-vs.png "Die Zoom Steuerelemente in der unteren Symbolleiste")

-----

Die Steuerelemente umfassen Folgendes:

1. Zoom anpassen
2. Verkleinern
3. Vergrößern
4. Tatsächliche Größe (1:1 Pixelgröße)

Diese Steuerelemente passen den Zoomfaktor auf der Entwurfs Oberfläche an. Sie wirken sich nicht auf die Benutzeroberfläche der Anwendung zur Laufzeit aus.

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

### <a name="properties-pad"></a>Eigenschaftenpad

Verwenden Sie die **Eigenschaftenpad** , um die Identität, die visuellen Stile, die Barrierefreiheit und das Verhalten eines Steuer Elements zu bearbeiten. Der folgende Screenshot veranschaulicht die **Eigenschaftenpad** Optionen für eine Schaltfläche:

[![Die Eigenschaftenpad für eine Schaltfläche.](introduction-images/17-buttonpropertiespad-vsmac.png "Die Eigenschaftenpad für eine Schaltfläche.")](introduction-images/17-buttonpropertiespad-vsmac-large.png#lightbox)
#### <a name="properties-pad-sections"></a>Eigenschaftenpad Abschnitte

Die **Eigenschaftenpad** enthält drei Abschnitte:

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

### <a name="properties-window"></a>Eigenschaftenfenster

Verwenden Sie das **Fenster Eigenschaften** , um die Identität, die visuellen Stile, die Barrierefreiheit und das Verhalten eines Steuer Elements zu bearbeiten. Der folgende Screenshot veranschaulicht die **Eigenschaften Fenster** Optionen für eine Schaltfläche:

[![Das Eigenschaften Fenster für eine Schaltfläche](introduction-images/17-buttonpropertieswindow-vs.png "Das Eigenschaften Fenster für eine Schaltfläche")](introduction-images/17-buttonpropertieswindow-vs-large.png#lightbox)

#### <a name="properties-window-sections"></a>Abschnitte der Eigenschaften Fenster

Das **Fenster Eigenschaften** enthält drei Abschnitte:

-----

1. **Widget** – die Haupteigenschaften des Steuer Elements, z. b. Name, Klasse, Stileigenschaften usw. Eigenschaften zum Verwalten des Inhalts des Steuer Elements werden in der Regel hier abgelegt.
2. **Layout** – Eigenschaften, die die Position und Größe des Steuer Elements nachverfolgen, einschließlich Einschränkungen und Frames, werden hier aufgelistet.
3. **Ereignisse** – Ereignisse und Ereignishandler werden hier angegeben. Nützlich für die Behandlung von Ereignissen wie z. b. berühren, tippen, ziehen usw. Ereignisse können auch direkt im Code behandelt werden.

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

#### <a name="editing-properties-in-the-properties-pad"></a>Bearbeiten von Eigenschaften im Eigenschaftenpad

Zusätzlich zur visuellen Bearbeitung auf der Entwurfs Oberfläche unterstützt der IOS-Designer das Bearbeiten von Eigenschaften in der **Eigenschaftenpad**. Die verfügbaren Eigenschaften ändern sich basierend auf dem ausgewählten Steuerelement, wie in den folgenden Screenshots veranschaulicht:

[![Schaltflächen Eigenschaften](introduction-images/18a-buttonpropertiespad-vsmac.png "Schaltflächen Eigenschaften")](introduction-images/18a-buttonpropertiespad-vsmac-large.png#lightbox)

[![Controller Eigenschaften anzeigen](introduction-images/18b-viewcontrollerpropertiespad-vsmac.png "Controller Eigenschaften anzeigen")](introduction-images/18b-viewcontrollerpropertiespad-vsmac-large.png#lightbox)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

#### <a name="editing-properties-in-the-properties-window"></a>Bearbeiten von Eigenschaften im Eigenschaften Fenster

Zusätzlich zur visuellen Bearbeitung auf der Entwurfs Oberfläche unterstützt der IOS-Designer das Bearbeiten von Eigenschaften im **Eigenschaften Fenster**. Die verfügbaren Eigenschaften ändern sich basierend auf dem ausgewählten Steuerelement, wie in den folgenden Screenshots veranschaulicht:

[![Schaltflächen Eigenschaften](introduction-images/18a-buttonpropertieswindow-vs.png "Schaltflächen Eigenschaften")](introduction-images/18a-buttonpropertieswindow-vs-large.png#lightbox)

[![Controller Eigenschaften anzeigen](introduction-images/18b-viewcontrollerpropertieswindow-vs.png "Controller Eigenschaften anzeigen")](introduction-images/18b-viewcontrollerpropertieswindow-vs-large.png#lightbox)

-----

> [!IMPORTANT]
> Der Identitäts Abschnitt der Eigenschaftenpad zeigt nun ein **Modulfeld** an. Es ist erforderlich, diesen Abschnitt nur bei der Interaktion mit SWIFT-Klassen auszufüllen. Verwenden Sie ihn, um einen Modulnamen für SWIFT-Klassen einzugeben, die Namespaces sind.

#### <a name="default-values"></a>Standardwerte

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

Viele Eigenschaften im **Eigenschaftenpad** zeigen keinen Wert oder einen Standardwert an. Der Code der Anwendung kann diese Werte jedoch trotzdem ändern. In der **Eigenschaftenpad** werden keine Werte angezeigt, die im Code festgelegt sind.

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Viele Eigenschaften im **Eigenschaften Fenster** zeigen keinen Wert oder einen Standardwert an. Der Code der Anwendung kann diese Werte jedoch trotzdem ändern. Im **Fenster Eigenschaften** werden keine Werte angezeigt, die im Code festgelegt sind.

-----

#### <a name="event-handlers"></a>Ereignishandler

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

Zum Angeben von benutzerdefinierten Ereignis Handlern für verschiedene Ereignisse verwenden Sie die Registerkarte **Ereignisse** der **Eigenschaftenpad**. Im folgenden Screenshot behandelt eine `HandleClick` Methode z. b. den Fingerabdruck der Schaltfläche **innerhalb** des Ereignisses:

[![Der Eigenschaftenpad, bei dem ein Ereignishandler für eine Schaltfläche festgelegt ist.](introduction-images/19-buttonpropertiespadevents-vsmac.png "Der Eigenschaftenpad, bei dem ein Ereignishandler für eine Schaltfläche festgelegt ist.")](introduction-images/19-buttonpropertiespadevents-vsmac-large.png#lightbox)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Zum Angeben von benutzerdefinierten Ereignis Handlern für verschiedene Ereignisse verwenden Sie die Registerkarte **Ereignisse** des **Fensters Eigenschaften**. Im folgenden Screenshot behandelt eine `HandleClick` Methode z. b. den Fingerabdruck der Schaltfläche **innerhalb** des Ereignisses:

[![Das Eigenschaften Fenster, bei dem ein Ereignishandler für eine Schaltfläche festgelegt ist.](introduction-images/19-buttonpropertieswindowevents-vs.png "Das Eigenschaften Fenster, bei dem ein Ereignishandler für eine Schaltfläche festgelegt ist.")](introduction-images/19-buttonpropertieswindowevents-vs-large.png#lightbox)

-----

Nachdem ein Ereignishandler angegeben wurde, muss der entsprechenden Ansichts Controller Klasse eine Methode mit dem gleichen Namen hinzugefügt werden. Andernfalls tritt eine `unrecognized selector` Ausnahme auf, wenn die Schaltfläche getippt wird:

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

[![Eine unbekannte Selektor-Ausnahme.](introduction-images/20-unrecognizedselector-vsmac.png "Eine unbekannte Selektor-Ausnahme.")](introduction-images/20-unrecognizedselector-vsmac-large.png#lightbox)

Beachten Sie, dass der IOS-Designer nach dem Angeben eines Ereignis Handlers im **Eigenschaftenpad**sofort die entsprechende Codedatei öffnet und das Angebot zum Einfügen der Methoden Deklaration bereitstellt. 

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[![Eine unbekannte Selektor-Ausnahme.](introduction-images/20-unrecognizedselector-vs.png "Eine unbekannte Selektor-Ausnahme.")](introduction-images/20-unrecognizedselector-vs-large.png#lightbox)

-----

Ein Beispiel für die Verwendung von benutzerdefinierten Ereignis Handlern finden Sie im Leitfaden für die ersten Schritte mit [Hello, IOS](~/ios/get-started/hello-ios/index.md).

### <a name="outline-view"></a>Gliederungsansicht

Der IOS-Designer kann auch die Hierarchie von Steuerelementen einer Schnittstelle als Kontur anzeigen. Die Gliederung ist verfügbar, indem Sie die Registerkarte **Dokument** Gliederung auswählen, wie unten dargestellt:

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

[![Die Dokument Gliederung](introduction-images/21-buttonoutlineview-vsmac.png "Die Dokument Gliederung")](introduction-images/21-buttonoutlineview-vsmac-large.png#lightbox)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[![Die Dokument Gliederung](introduction-images/21-buttonoutlineview-vs.png "Die Dokument Gliederung")](introduction-images/21-buttonoutlineview-vs-large.png#lightbox)

-----

Das ausgewählte Steuerelement in der Gliederungs Ansicht bleibt synchron mit dem ausgewählten Steuerelement auf der Entwurfs Oberfläche.  Diese Funktion ist nützlich, um ein Element aus einer tief geschachtelten Schnittstellen Hierarchie auszuwählen.

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

## <a name="revert-to-xcode"></a>Xcode zurücksetzen

Der IOS-Designer und Xcode-Interface Builder können austauschbar verwendet werden. Zum Öffnen eines Storyboards oder einer XIb-Datei in Xcode Interface Builder klicken Sie mit der rechten Maustaste auf die Datei, und wählen Sie **Öffnen mit > Xcode Interface Builder**aus, wie im folgenden Screenshot veranschaulicht:

[![Öffnen eines Storyboards in Xcode Interface Builder](introduction-images/22-openinxcodeinterfacebuilder-vsmac.png "Öffnen eines Storyboards in Xcode Interface Builder")](introduction-images/22-openinxcodeinterfacebuilder-vsmac-large.png#lightbox)

Nachdem Sie die Änderungen in Xcode Interface Builder vorgenommen haben, speichern Sie die Datei, und kehren Sie zu Visual Studio für Mac zurück. Die Änderungen werden mit dem xamarin. IOS-Projekt synchronisiert.

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

## <a name="revert-to-xcode"></a>Xcode zurücksetzen

Es ist möglich, den IOS-Designer und Xcode Interface Builder austauschbar zu verwenden, aber Xcode Interface Builder ist nur auf dem Mac verfügbar. Öffnen Sie die Projekt Mappe mit dem xamarin. IOS-Projekt in [Visual Studio für Mac](/visualstudio/mac/), klicken Sie mit der rechten Maustaste auf die Datei, und wählen Sie **Öffnen mit > Xcode Interface Builder**aus, wie im folgenden Screenshot veranschaulicht, um eine Storyboard-oder XIb-Datei in Xcode Interface Builder auf einem Mac zu öffnen:

[![Öffnen eines Storyboards in Xcode Interface Builder](introduction-images/22-openinxcodeinterfacebuilder-vsmac.png "Öffnen eines Storyboards in Xcode Interface Builder")](introduction-images/22-openinxcodeinterfacebuilder-vsmac-large.png#lightbox)

Nachdem Sie die Änderungen in Xcode Interface Builder vorgenommen haben, speichern Sie die Datei, und kehren Sie zu Visual Studio für Mac zurück. Die Änderungen werden mit dem xamarin. IOS-Projekt synchronisiert.

-----

## <a name="xib-support"></a>. XIb-Unterstützung

Der IOS-Designer unterstützt das Erstellen, bearbeiten und Verwalten von XIb-Dateien. Dabei handelt es sich um XML-Dateien, die einzelne benutzerdefinierte Ansichten erneut senden, die der Ansichts Hierarchie einer Anwendung hinzugefügt werden können. Eine XIb-Datei stellt in der Regel die Schnittstelle für eine einzelne Ansicht oder einen Bildschirm in einer Anwendung dar, während ein Storyboard viele Bildschirme und die Übergänge zwischen Ihnen darstellt.

Es gibt viele Meinungen darüber, welche Lösung – XIb-Dateien, Storyboards oder Code – am besten zum Erstellen und Verwalten einer Benutzeroberfläche verwendet werden. In Wirklichkeit gibt es keine perfekte Lösung, und es lohnt sich immer, das beste Tool für die jeweilige Aufgabe zu erwägen. Das heißt, XIb-Dateien sind in der Regel leistungsfähiger, wenn Sie eine benutzerdefinierte Ansicht darstellen, die an mehreren Stellen in einer APP benötigt wird, z. b. eine benutzerdefinierte Tabellen Ansichts Zelle. 

Weitere Dokumentationen zur Verwendung von XIb-Dateien finden Sie in den folgenden Rezepten:

- [Verwenden der Vorlage "View. XIb"](https://github.com/xamarin/recipes/tree/master/Recipes/ios/general/templates/using_the_ios_view_xib_template)
- [Erstellen einer benutzerdefinierten tableviewcell mithilfe von ". XIb"](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/tables/custom-tableviewcell)
- [Erstellen eines Startbildschirms mithilfe von ". XIb"](https://github.com/xamarin/recipes/tree/master/Recipes/ios/general/templates/launchscreen-xib)

Weitere Informationen zur Verwendung von Storyboards finden Sie unter [Introduction to Storyboards (Einführung in Storyboards](~/ios/user-interface/storyboards/index.md)).

Diese und andere IOS-Designer-bezogene Anleitungen beziehen sich auf die Verwendung von Storyboards als Standardmethode zum Erstellen von Benutzeroberflächen, da die meisten xamarin. IOS-Vorlagen für neue Projekte standardmäßig ein Storyboard bereitstellen.

## <a name="summary"></a>Zusammenfassung

Dieses Handbuch bietet eine Einführung in den IOS-Designer, in dem die Features beschrieben und die Tools beschrieben werden, die für das Entwerfen von Benutzeroberflächen angeboten werden.

## <a name="related-links"></a>Verwandte Links

- [Einführung in Storyboards](~/ios/user-interface/storyboards/index.md)
- [Exemplarische Vorgehensweise für Steuerelemente für IOS](~/ios/user-interface/designer/ios-designable-controls-walkthrough.md)
- [Hello, iOS](~/ios/get-started/hello-ios/index.md)
- [Hello, iOS Multiscreen](~/ios/get-started/hello-ios-multiscreen/index.md)
- [Übersicht über Android Designer](~/android/user-interface/android-designer/index.md)
- [Partielle Klassen und Methoden](https://docs.microsoft.com/dotnet/csharp/programming-guide/classes-and-structs/partial-classes-and-methods)
- [Eintauchen in den Xamarin Designer für IOS-Evolve 2014 (Video)](https://www.youtube.com/watch?v=W4H9uLjoEjM)
- [Verwenden des IOS-Designers zum Erstellen eines Startbildschirms (Video)](https://university.xamarin.com/lightninglectures/using-the-ios-designer-to-create-a-launch-screen)
