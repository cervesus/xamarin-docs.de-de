---
title: Windows in Xamarin.Mac
description: In diesem Artikel wird das Arbeiten mit Fenstern und Bereichen in einer Xamarin.Mac-Anwendung behandelt. Erstellen von Windows und Bereiche in Xcode und Interface Builder, die sie von Storyboards und XIB-Dateien geladen und Programmgesteuertes Arbeiten mit ihnen beschrieben.
ms.prod: xamarin
ms.assetid: 4F6C67E9-BBFF-44F7-B29E-AB47D7F44287
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/14/2017
ms.openlocfilehash: ec907e71074a97bd5d1714e79dd504013f5c8a4b
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61240844"
---
# <a name="windows-in-xamarinmac"></a>Windows in Xamarin.Mac

_In diesem Artikel wird das Arbeiten mit Fenstern und Bereichen in einer Xamarin.Mac-Anwendung behandelt. Erstellen von Windows und Bereiche in Xcode und Interface Builder, die sie von Storyboards und XIB-Dateien geladen und Programmgesteuertes Arbeiten mit ihnen beschrieben._

Bei der Arbeit mit C# und .NET in einer Xamarin.Mac-Anwendung, Sie haben Zugriff auf die gleiche Windows, und die Panel-Elemente, die ein Entwickler *Objective-C-* und *Xcode* ist. Da Xamarin.Mac direkt in Xcode integriert ist, können Sie von Xcode _Interface Builder_ erstellen und Verwaltung Ihrer Windows und Bereiche (oder erstellen sie optional direkt in c#-Code).

Basierend auf ihren Zweck, kann eine Xamarin.Mac-Anwendung stellen ein oder mehrere Windows auf dem Bildschirm zum Verwalten und koordinieren die Informationen wird angezeigt, und arbeitet mit. Die Hauptfunktionen eines Fensters sind:

1. Um einen Bereich bereitstellen, in dem Ansichten und Steuerelemente gespeichert und verwaltet werden können.
2. Um zu akzeptieren und reagieren auf Ereignisse als Reaktion auf Benutzerinteraktion mit Tastatur und Maus.

Windows kann sein, in einem nicht modalen Zustand (z. B. in einem Text-Editor, der mehrere Dokumente gleichzeitig geöffnet sein kann) verwendeten "oder" Modal (z. B. ein Dialogfeld "Exportieren", die geschlossen werden muss, bevor die Anwendung fortgesetzt werden kann).

Bereiche sind eine besondere Art von Fenster (eine Unterklasse des basistexts `NSWindow` Klasse), in der Regel dienen, die eine zusätzliche Funktion in einer Anwendung, z. B. Hilfsprogramm Windows wie Inspektoren für Text-Format und das System Farbauswahl.

[![](window-images/intro01.png "Bearbeiten ein Fenster in Xcode")](window-images/intro01.png#lightbox)

In diesem Artikel werden die Grundlagen der Arbeit mit Windows und Bereiche in einer Xamarin.Mac-Anwendung beschrieben. Es wird dringend empfohlen, dass Sie über arbeiten die [Hallo, Mac](~/mac/get-started/hello-mac.md) Artikel zuerst, insbesondere die [Einführung in Xcode und Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) und [Outlets und Aktionen](~/mac/get-started/hello-mac.md#outlets-and-actions) Abschnitte, wie sie behandelt wichtige Konzepte und Techniken, die wir in diesem Artikel verwenden.

Empfiehlt es sich um einen Blick auf die [Verfügbarmachen von c#-Klassen / Methoden mit Objective-C](~/mac/internals/how-it-works.md) Teil der [Xamarin.Mac-Interna](~/mac/internals/how-it-works.md) dokumentieren, es wird erläutert, die `Register` und `Export` Befehle verwendet, um Ihre Klassen in c# für Objective-C-Objekte und Elemente der Benutzeroberfläche anschließen.

<a name="Introduction_to_Windows" />

## <a name="introduction-to-windows"></a>Einführung in Windows

Wie bereits erwähnt, wird ein Fenster enthält einen Bereich in dem Ansichten und Steuerelemente gespeichert und verwaltet werden können, und reagiert auf Ereignisse, die je nach Benutzerinteraktion (entweder über die Tastatur oder Maus).

Gemäß den Apple gibt es fünf Haupttypen von Windows in einem MacOS-App:

- **Dokumentfenster** -ein Dokumentfenster enthält dateibasierte Benutzerdaten, z. B. eine Tabelle oder ein Textdokument.
- **App-Fenster** – einem app-Fenster wird das Hauptfenster der Anwendung nicht dokumentbasierte (z. B. die Kalender-app auf einem Mac) ist.
- **Bereich** – ein Bereich über andere Fenster gleitet, und stellt Tools bereit, oder öffnen Sie die Steuerelemente, die Benutzer mit arbeiten können, wenn Dokumente sind. In einigen Fällen kann ein Panel lichtdurchlässiger sein (z. B. beim Arbeiten mit großen Grafiken).
- **Dialogfeld** -ein Dialogfeld angezeigt, die als Reaktion auf eine Benutzeraktion wird und in der Regel stellt Möglichkeiten Benutzer können die Aktion ausgeführt. Ein Dialogfeld ist eine Antwort des Benutzers erforderlich, bevor sie geschlossen werden kann. (Finden Sie unter [arbeiten mit Dialogen](~/mac/user-interface/dialog.md))
- **Warnungen** – eine Warnung ist eine besondere Art von Dialogfeld, das angezeigt wird, tritt ein schwerwiegendes Problem (z. B. ein Fehler) oder als eine Warnung (z. B. die Vorbereitung zum Löschen einer Datei). Da eine Warnung ein Dialog ist, benötigen sie auch eine, bevor sie geschlossen werden kann. (Finden Sie unter [arbeiten mit Warnungen](~/mac/user-interface/alert.md))

Weitere Informationen finden Sie unter den [zu Windows](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowAppearanceBehavior.html#//apple_ref/doc/uid/20000957-CH33-SW1) Abschnitt des Apple [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)

<a name="Main_Key_and_Inactive_Windows" />

### <a name="main-key-and-inactive-windows"></a>Main, Schlüssel und inaktive Windows

Windows in einer Xamarin.Mac-Anwendung können Aussehen und Verhalten sich anders als Grundlage, wie der Benutzer derzeit mit ihnen interagiert. Wird aufgerufen, das wichtigste Dokument oder die App-Fenster, die derzeit der Fokus der Aufmerksamkeit des Benutzers wird die _Hauptfenster_. In den meisten Fällen in diesem Fenster werden auch die _Fenster Key_ (das Fenster, die gegenwärtig Benutzereingaben akzeptiert). Aber dies ist nicht immer der Fall, beispielsweise ein Farbwähler geöffnet sein und das Schlüssel-Fenster, dem der Benutzer mit den Zustand eines Elements im Dokumentfenster zu ändern (der immer noch das Hauptfenster sein würde) interagiert werden kann.

Die Haupt- und den Schlüssel Windows (Wenn sie separate sind) sind immer aktiv, _inaktiv Windows_ Fenster geöffnet, die nicht im Vordergrund sind. Angenommen, eine Text-Editor-Anwendung verfügen über mehr als ein Dokument geöffnet ist jeweils nur das Hauptfenster aktiv sein würde, alle anderen wäre inaktiv. 

Weitere Informationen finden Sie unter den [zu Windows](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowAppearanceBehavior.html#//apple_ref/doc/uid/20000957-CH33-SW1) Abschnitt des Apple [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)

<a name="Naming_Windows" />

### <a name="naming-windows"></a>Benennen von Windows

Ein Fenster kann eine Titelleiste und beim Titel angezeigt wird, ist es in der Regel den Namen der Anwendung, die den Namen des Dokuments, bearbeitet oder die Funktion des Fensters (z. B. Inspektor). Einige Anwendungen keine Titelleiste angezeigt werden, da sie von der Sicht erkannt und nicht mit Dokumenten arbeiten.

Apple empfehlen die folgenden Richtlinien:

- Verwenden Sie für den Titel eines Fensters main, kein Dokument ist der Name Ihrer Anwendung. 
- Benennen Sie ein neues Dokumentfenster `untitled`. Für das erste neue Dokument, keine Zahl auf den Titel anfügen (z. B. `untitled 1`). Wenn der Benutzer ein neues Dokument vor dem Speichern und Großbuchstaben für die erste erstellt werden, rufen Sie das Fenster `untitled 2`, `untitled 3`usw.

Weitere Informationen finden Sie unter den [Benennung Windows](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowNaming.html#//apple_ref/doc/uid/20000957-CH35-SW1) Abschnitt des Apple [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)

<a name="Full-Screen_Windows" />

### <a name="full-screen-windows"></a>Vollbild-Windows

MacOS, das Fenster einer Anwendung kann wechseln Sie im Vollbildmodus ausblenden Alles einschließlich der Menüleiste (die durch den Cursor auf den oberen Rand des Bildschirms bewegen aufgedeckt werden können) zu störende kostenlose Interaktion mit der Anwendung ist ein Inhaltselement.

Apple empfiehlt es sich um die folgenden Richtlinien:

- Bestimmen Sie, ob es für ein Fenster sinnvoll, um den Vollbildmodus zu wechseln. Anwendungen, die kurze Interaktionen (z. B. einen Rechner) ermöglichen dürfen nicht Vollbildmodus bereitstellen.
- Zeigen Sie der Symbolleiste aus, wenn die Vollbild-Aufgabe erforderlich ist. In der Regel wird die Symbolleiste im Vollbildmodus ausgeblendet.
- Das Vollbild-Fenster müssen alle Features Benutzer müssen zum Ausführen die Aufgabe.
- Vermeiden Sie wenn möglich, Finder Interaktion während der Benutzer ein Vollbild-Fenster wird.
- Nutzen Sie die erhöhte Bildschirmbereich ohne den Fokus von der wichtigste verschieben.

Weitere Informationen finden Sie unter den [Vollbildmodus Windows](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/FullScreen.html#//apple_ref/doc/uid/20000957-CH61-SW1) Abschnitt des Apple [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)

<a name="Panels" />

### <a name="panels"></a>Panels

Ein Bereich ist eine zusätzliche Fenster enthält, Steuerelemente und Optionen, die das aktive Dokument oder die Auswahl (z. B. das System Farbauswahl) betreffen:

[![](window-images/panel01.png "Ein Farbbereich")](window-images/panel01.png#lightbox)

Bereiche können es sich um _App-spezifische_ oder _systemweiten_. App-spezifischen Bereichen "float", über den oberen Rand der Anwendung Dokumentfenster und nicht mehr angezeigt, wenn die Anwendung im Hintergrund ist. Systemweiten Panels (z. B. die **Schriftarten** Bereich), "float", über alle offenen Fenster unabhängig von der Anwendung. 

Apple empfiehlt es sich um die folgenden Richtlinien:

- Im Allgemeinen verwenden Sie einen standard-Bereich, transparente Bereiche sollte nur sparsam und für grafisch intensive Vorgänge verwendet werden.
- Erwägen Sie einen Bereich, um Benutzern ermöglichen einfachen Zugriff auf wichtige Steuerelemente oder Informationen, die ihre Aufgabe direkt betroffen sind.
- Ausblenden und Anzeigen von Bereichen wie erforderlich.
- Bereichen sollten immer Titelleiste enthalten.
- Bereiche sollten nicht mit eine aktiven Minimieren-Schaltfläche enthalten.

#### <a name="inspectors"></a>Inspektoren

Die meisten moderne Anwendungen für MacOS stellen zusätzliche Steuerelemente und Optionen, die das aktive Dokument oder die Auswahl als betreffen _Inspektoren_ werden, die Teil des Hauptfensters (wie die **Seiten** app siehe unten), anstelle von Windows für Bereich:

[![](window-images/panel02.png "Ein Beispiel-Inspektor")](window-images/panel02.png#lightbox)

Weitere Informationen finden Sie unter den [Bereiche](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowPanels.html#//apple_ref/doc/uid/20000957-CH42-SW1) Abschnitt des Apple [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/) und unseren [MacInspector](https://developer.xamarin.com/samples/mac/MacInspector/) Beispiel-app für eine vollständige Implementierung von einem **Inspector-Schnittstelle** in einer Xamarin.Mac-app.

<a name="Creating_and_Maintaining_Windows_in_Xcode" />

## <a name="creating-and-maintaining-windows-in-xcode"></a>Erstellen und Verwalten von Windows in Xcode

Wenn Sie eine neue Xamarin.Mac-Cocoa-Anwendung erstellen, erhalten Sie ein Standardfenster leer ist, wird standardmäßig an. Dieses Windows wird definiert, einem `.storyboard` Datei automatisch im Projekt enthalten ist. So bearbeiten Sie Ihre Windows-Design, in der **Projektmappen-Explorer**, Doppelklick der `Main.storyboard` Datei:

[![](window-images/edit01.png "Wählen die Haupt-storyboard")](window-images/edit01.png#lightbox)

Dies öffnet das Fenster Design in Interface Builder von Xcode:

[![](window-images/edit02.png "Bearbeiten die Benutzeroberfläche in Xcode")](window-images/edit02.png#lightbox)

In der **Attributinspektor**, es gibt mehrere Eigenschaften, die Sie zum Definieren und steuern das Fenster verwenden können:

- **Titel** – Dies ist der Text, der in der Titelleiste des Fensters angezeigt wird.
- **Automatisches Speichern** – Dies ist die _Schlüssel_ , wird verwendet, um das Fenster ID, wenn sie die Position und Einstellungen automatisch gespeichert werden.
- **Titelleiste** -zeigt das Fenster eine Titelleiste.
- **Einheitliche Funktionen für Titel und die Symbolleiste** : Wenn das Fenster eine Symbolleiste, enthält, muss es Teil der Titelleiste angezeigt.
- **Vollständige Größe Inhalt anzeigen** -den Inhaltsbereich des Fensters, um unter der Titelleiste werden können.
- **Schatten** -verfügt über das Fenster einen Schatten.
- **Texturen versehenen** -strukturierte Windows können haben (z. B. Dynamik) und können durch Ziehen an einer beliebigen Stelle in ihrem Textkörper verschoben werden.
- **Schließen Sie** -verfügt über das Fenster eine Schaltfläche "Schließen".
- **Minimieren Sie** -verfügt über das Fenster eine Minimierungsschaltfläche.
- **Ändern der Größe** -verfügt über das Fenster eine Größe des Steuerelements.
- **Symbolleisten-Schaltfläche** -verfügt über das Fenster eine ein-/ausblenden-Symbolleisten-Schaltfläche.
- **Wiederherstellbare** -ist die Position des Fensters und die Einstellungen automatisch gespeichert und wiederhergestellt.
- **Starten Sie sichtbar an** -wird das Fenster automatisch angezeigt wird, wenn die `.xib` -Datei geladen wird.
- **Ausblenden, die bei Deaktivierung** -Fenster wird ausgeblendet werden, wenn die Anwendung den Hintergrund wechselt.
- **Release geschlossen** -Fenster aus dem Arbeitsspeicher gelöscht werden, wenn es geschlossen wird.
- **Immer anzeigen von QuickInfos** -die QuickInfo, die ständig angezeigt werden.
- **Berechnet die Ansicht Schleife** -ist die Ansicht Reihenfolge neu berechnet, bevor das Fenster gezeichnet wird.
- **Leerzeichen**, **Zurschaustellung** und **Cycling** – definiert das Verhalten des Fensters in diesen Umgebungen MacOS. 
- **Vollbild** -bestimmt, ob dieses Fenster im Vollbildmodus eingeben kann. 
- **Animation** -steuert den Typ der Animation, die für das Fenster zur Verfügung.
- **Darstellung** – steuert die Darstellung des Fensters. Derzeit besteht nur eine Darstellung, Aqua.

Finden Sie unter Apple [Einführung in Windows](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1) und [NSWindow](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSWindow_Class/index.html#//apple_ref/occ/cl/NSWindow) Dokumentation.

<a name="Setting_the_Default_Size_and_Location" />

### <a name="setting-the-default-size-and-location"></a>Festlegen der Standardgröße und Position

Wechseln Sie zu, um die ursprüngliche Position des Fensters festgelegt und dessen Größe zu steuern, die **Größeninspektor**:

[![](window-images/edit07.png "Die standardmäßige Größe und Position")](window-images/edit07.png#lightbox)

Von hier aus können Sie die Anfangsgröße des Fensters, geben sie eine minimale und maximale Größe, legen Sie die ursprüngliche Position auf dem Bildschirm und steuern die Rahmen um das Fenster.

<a name="Setting-a-Custom-Main-Window-Controller" />

### <a name="setting-a-custom-main-window-controller"></a>Festlegen eines benutzerdefinierten Hauptfenster-Controllers

Zum Erstellen von Outlets und Aktionen, um Benutzeroberflächenelemente für c#-Code verfügbar zu machen können, müssen die Xamarin.Mac-app einen benutzerdefinierten Fenstercontroller verwenden.

Führen Sie folgende Schritte aus:

1. Öffnen Sie die app Storyboard in Interface Builder von Xcode an.
2. Wählen Sie die `NSWindowController` in der Entwurfsoberfläche.
3. Wechseln Sie zu der **Identitätsinspektor** anzuzeigen, und geben Sie `WindowController` als die **Klassenname**: 

    [![](window-images/windowcontroller01.png "Den Namen der Klasse festlegen")](window-images/windowcontroller01.png#lightbox)
4. Die Änderungen zu speichern und zurück zu Visual Studio für Mac, um zu synchronisieren.
5. Ein `WindowController.cs` Datei wird hinzugefügt werden, mit Ihrem Projekt in der **Projektmappen-Explorer** in Visual Studio für Mac: 

    [![](window-images/windowcontroller02.png "Wählen die Windows-Domänencontroller")](window-images/windowcontroller02.png#lightbox)
6. Öffnen Sie erneut das Storyboard in Interface Builder von Xcode.
7. Die `WindowController.h` Datei wird für die Verwendung verfügbar sein: 

    [![](window-images/windowcontroller03.png "Bearbeiten der Datei WindowController.h")](window-images/windowcontroller03.png#lightbox)

<a name="Adding_UI_Elements" />

### <a name="adding-ui-elements"></a>Hinzufügen von UI-Elemente

Ziehen Sie Steuerelemente aus, um den Inhalt eines Fensters zu definieren, die **Bibliotheksinspektor** auf die **Schnittstellen-Editor**. Informieren Sie sich unsere [Einführung in Xcode und Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) Dokumentation für Weitere Informationen zur Verwendung von Interface Builder erstellen und Steuerelemente zu aktivieren.

Beispielsweise können wir ziehen Sie die Symbolleiste aus der **Bibliotheksinspektor** in das Fenster in der **Schnittstellen-Editor**:

[![](window-images/edit03.png "Eine Symbolleiste auswählen aus der Bibliothek")](window-images/edit03.png#lightbox)

Ziehen Sie anschließend eine **Textansicht** und ihre Größe, um die Fläche unter der Symbolleiste aufgefüllt:

[![](window-images/edit04.png "Hinzufügen einer Textansicht")](window-images/edit04.png#lightbox)

Da wir möchten die **Textansicht** verkleinert und vergrößert sich die Größe des Fensters ändert, wechseln wir zu den **Einschränkung Editor** und fügen Sie die folgenden Einschränkungen:

[![](window-images/edit05.png "Bearbeiten von Einschränkungen")](window-images/edit05.png#lightbox)

Durch Klicken auf die für **roten Balken** am oben im Editor und durch Klicken auf **4 Einschränkungen hinzufügen**, weisen wir die Textansicht zu halten, den angegebenen X-, Y-Koordinaten und vergrößert oder verkleinert werden, horizontal und vertikal als die Größe des Fensters wird geändert.

Lassen Sie uns zum Schluss verfügbar zu machen die **Textansicht** codieren, verwenden eine **Outlet** (sichergestellt wird, wählen Sie die `ViewController.h` Datei):

[![](window-images/edit06.png "Konfigurieren eines Outlets")](window-images/edit06.png#lightbox)

Speichern Sie die Änderungen zu, und wechseln Sie zurück zu Visual Studio für Mac mit Xcode synchronisiert.

Weitere Informationen zum Arbeiten mit **Outlets** und **Aktionen**, informieren Sie sich unsere [Outlets und Aktionen](~/mac/get-started/hello-mac.md#outlets-and-actions) Dokumentation.

<a name="Standard_Window_Workflow" />

### <a name="standard-window-workflow"></a>Standardfenster-Workflow

Für alle Fenster, das Sie erstellen und in Ihre Xamarin.Mac-Anwendung verwenden, ist der Prozess im Grunde identisch mit was wir einfach oben getan haben:

1. Fügen Sie für neue Windows, die nicht die Standardeinstellung, die dem Projekt automatisch hinzugefügt werden eine neue Fensterdefinition zum Projekt hinzu. Dies wird weiter unten ausführlich erläutert werden.
2. Doppelklicken Sie auf die `Main.storyboard` Datei, um das Fenster-Design für die Bearbeitung in Interface Builder von Xcode zu öffnen.
3. Ziehen Sie ein neues Fenster in der Benutzeroberfläche entwerfen und verknüpfen Sie das Fenster, in die Verwendung des Hauptfenster _Segues_ (Weitere Informationen finden Sie unter den [Segues](~/mac/platform/storyboards/indepth.md#Segues) Teil unserer [arbeiten mit Storyboards](~/mac/platform/storyboards/indepth.md) Dokumentation).
3. Legen Sie alle erforderlichen Eigenschaften in der **Attributinspektor** und **Größeninspektor**.
4. Ziehen Sie in den Steuerelementen erforderlich, um Ihre Schnittstelle erstellen und konfigurieren sie in der **Attributinspektor**.
5. Verwenden der **Größeninspektor** , behandeln das Ändern der Größe für Ihre UI-Elemente.
6. Machen Sie das Fenster des UI-Elemente für C#-Code über **Outlets** und **Aktionen**.
7. Speichern Sie die Änderungen zu, und wechseln Sie zurück zu Visual Studio für Mac mit Xcode synchronisiert.

Nun, da wir einen grundlegenden Windows erstellt haben, betrachten die typische Prozesse ein Xamarin.Mac wir Anwendung wird bei der Arbeit mit Windows. 

<a name="Displaying_the_Default_Window" />

## <a name="displaying-the-default-window"></a>Anzeigen des Standard-Fensters

Standardmäßig eine neue Xamarin.Mac-Anwendung zeigt automatisch das Fenster definiert, der `MainWindow.xib` -Datei, wenn es gestartet wird:

[![](window-images/display01.png "Ein Beispiel-Fenster ausgeführt wird")](window-images/display01.png#lightbox)

Da wir den Entwurf, oben im Fenster geändert haben, enthält es nun einen Standard-Symbolleiste und **Textansicht** Steuerelement. Im folgenden Abschnitt, der `Info.plist` -Datei für die Anzeige dieses Fensters verantwortlich ist:

[![](window-images/display00.png "Bearbeiten die Datei \"Info.plist\"")](window-images/display00.png#lightbox)

Die **Hauptschnittstelle** Dropdownliste wird verwendet, um das Storyboard auswählen, die als die Haupt-app-Benutzeroberfläche verwendet werden soll (in diesem Fall `Main.storyboard`).

Eine View-Controller wird automatisch hinzugefügt, auf das Projekt, Main Windows zu steuern, der (zusammen mit der primären Ansicht) angezeigt wird. Es definiert ist die `ViewController.cs` und angefügt die **Besitzer der Datei** in Interface Builder unter der **Identitätsinspektor**:

[![](window-images/display02.png "Der Besitzer der Datei festlegen")](window-images/display02.png#lightbox)

Für unser Fenster, wir möchten damit haben einen beliebigen Titel `untitled` beim ersten Öffnen wir also außer Kraft setzen der `ViewWillAppear` -Methode in der die `ViewController.cs` zu wie folgt aussehen:

```csharp
public override void ViewWillAppear ()
{
    base.ViewWillAppear ();

    // Set Window Title
    this.View.Window.Title = "untitled";
}
``` 

> [!NOTE]
> Legen wir den Wert des Fensters `Title` -Eigenschaft in der `ViewWillAppear` -Methode anstelle der `ViewDidLoad` Methode daran, während die Ansicht in den Arbeitsspeicher geladen werden, es nicht noch vollständig instanziiert wird. Wenn wir versuchen, den Zugriff auf die `Title` -Eigenschaft in der `ViewDidLoad` Methode wir erhalten eine `null` Ausnahme, da das Fenster noch nicht wurde erstellt und wired oben auf die Eigenschaft noch.

<a name="Programmatically_Closing_a_Window" />

## <a name="programmatically-closing-a-window"></a>Programmgesteuertes Schließen eines Fensters

Gibt es möglicherweise Fälle, in denen Sie programmgesteuert ein Fenster in einer Xamarin.Mac-Anwendung schließen möchten, klicken Sie neben der Verwendung der Benutzer auf des Fensters **schließen** Schaltfläche oder mithilfe eines Menüelements. MacOS bietet zwei verschiedene Verfahren zum Schließen einer `NSWindow` programmgesteuert: `PerformClose` und `Close`.

<a name="PerformClose" />

### <a name="performclose"></a>PerformClose

Aufrufen der `PerformClose` Methode eine `NSWindow` simuliert, den Benutzer des Fensters auf **schließen** Schaltfläche vorübergehend Hervorhebung der Schaltfläche aus, und klicken Sie dann das Fenster geschlossen.

Wenn die Anwendung implementiert die `NSWindow`des `WillClose` Ereignis wird ausgelöst werden, bevor das Fenster geschlossen wird. Wenn das Ereignis zurückgibt `false`, und klicken Sie dann das Fenster nicht geschlossen wird. Wenn das Fenster nicht verfügt eine **schließen** Schaltfläche oder kann nicht geschlossen werden aus irgendeinem Grund das Betriebssystem die akustisches Signal ausgegeben wird.

Zum Beispiel:

```csharp
MyWindow.PerformClose(this);
```

Würde versuchen, schließen Sie die `MyWindow` `NSWindow` Instanz. Wenn der Vorgang erfolgreich abgeschlossen wurde, wird das Fenster geschlossen werden, andernfalls wird die akustisches Signal ausgegeben werden, sodass die geöffnet bleibt.

<a name="Close" />

### <a name="close"></a>Schließen

Aufrufen der `Close` Methode ein `NSWindow` ist nicht den Benutzer des Fensters auf simuliert **schließen** Schaltfläche durch vorübergehend Hervorhebung der Schaltfläche, schließt sie einfach das Fenster.

Ein Fenster muss nicht geschlossen werden, angezeigt werden und ein `NSWindowWillCloseNotification` Benachrichtigung wird in den Standardmittelpunkt der Benachrichtigung für das Fenster geschlossen wird gebucht werden.

Die `Close` Methode unterscheidet sich in zwei wichtigen Punkten von der `PerformClose` Methode:

1. Er versucht nicht, lösen die `WillClose` Ereignis.
2. Es wird nicht simuliert, das Benutzer klicken auf die **schließen** Schaltfläche vorübergehend Hervorhebung der Schaltfläche.

Zum Beispiel:

```csharp
MyWindow.Close();
```

Würden Sie zum Schließen der `MyWindow` `NSWindow` Instanz.

<a name="Modified-Windows-Content" />

## <a name="modified-windows-content"></a>Geänderte Windows-Inhalt

Apple stellt unter MacOS, eine Möglichkeit, den Benutzer zu informieren, die den Inhalt eines Fensters (`NSWindow`) wurde vom Benutzer geändert und gespeichert werden muss. Wenn das Fenster geänderten Inhalt enthält, wird ein kleiner schwarzer Punkt angezeigt werden, in dessen **schließen** Widgets:

[![](window-images/close01.png "Ein Fenster mit den geänderten marker")](window-images/close01.png#lightbox)

Wenn der Benutzer versucht, das Fenster schließen oder beenden Sie die Mac-App, während es nicht gespeicherte sind, der Inhalt des Fensters geändert wird, sollten Sie zur Verfügung stellen eine [im Dialogfeld](~/mac/user-interface/dialog.md) oder [modale Blatt](~/mac/user-interface/dialog.md) und dem Benutzer ermöglichen, ihre Änderungen zu speichern erste:

[![](window-images/close02.png "Ein Blatt angezeigt wird, wenn das Fenster geschlossen wird gespeichert")](window-images/close02.png#lightbox)

### <a name="marking-a-window-as-modified"></a>Ein Fenster als geändert markieren

Um ein Fenster als wenn Inhalt geändert zu markieren, verwenden Sie den folgenden Code:

```csharp
// Mark Window content as modified
Window.DocumentEdited = true;
```

Und sobald die Änderung gespeichert wurde, deaktivieren Sie das Änderungsflag mit:

```csharp
// Mark Window content as not modified
Window.DocumentEdited = false;
```

### <a name="saving-changes-before-closing-a-window"></a>Speichern von Änderungen vor dem Schließen eines Fensters

Um dem Benutzer das Schließen eines Fensters, und ihnen ermöglicht, geänderte Inhalt im Voraus zu speichern überwachen, müssen Sie eine Unterklasse von erstellen `NSWindowDelegate` und überschreiben seine `WindowShouldClose` Methode. Zum Beispiel:

```csharp
using System;
using AppKit;
using System.IO;
using Foundation;

namespace SourceWriter
{
    public class EditorWindowDelegate : NSWindowDelegate
    {
        #region Computed Properties
        public NSWindow Window { get; set;}
        #endregion

        #region constructors
        public EditorWindowDelegate (NSWindow window)
        {
            // Initialize
            this.Window = window;

        }
        #endregion

        #region Override Methods
        public override bool WindowShouldClose (Foundation.NSObject sender)
        {
            // is the window dirty?
            if (Window.DocumentEdited) {
                var alert = new NSAlert () {
                    AlertStyle = NSAlertStyle.Critical,
                    InformativeText = "Save changes to document before closing window?",
                    MessageText = "Save Document",
                };
                alert.AddButton ("Save");
                alert.AddButton ("Lose Changes");
                alert.AddButton ("Cancel");
                var result = alert.RunSheetModal (Window);

                // Take action based on result
                switch (result) {
                case 1000:
                    // Grab controller
                    var viewController = Window.ContentViewController as ViewController;

                    // Already saved?
                    if (Window.RepresentedUrl != null) {
                        var path = Window.RepresentedUrl.Path;

                        // Save changes to file
                        File.WriteAllText (path, viewController.Text);
                        return true;
                    } else {
                        var dlg = new NSSavePanel ();
                        dlg.Title = "Save Document";
                        dlg.BeginSheet (Window, (rslt) => {
                            // File selected?
                            if (rslt == 1) {
                                var path = dlg.Url.Path;
                                File.WriteAllText (path, viewController.Text);
                                Window.DocumentEdited = false;
                                viewController.View.Window.SetTitleWithRepresentedFilename (Path.GetFileName(path));
                                viewController.View.Window.RepresentedUrl = dlg.Url;
                                Window.Close();
                            }
                        });
                        return true;
                    }
                    return false;
                case 1001:
                    // Lose Changes
                    return true;
                case 1002:
                    // Cancel
                    return false;
                }
            }

            return true;
        }
        #endregion
    }
}
```

Verwenden Sie den folgenden Code, um eine Instanz dieses Delegaten an das Fenster anzufügen:

```csharp
// Set delegate
Window.Delegate = new EditorWindowDelegate(Window);
```

### <a name="saving-changes-before-closing-the-app"></a>Speichern von Änderungen vor dem Schließen der App

Schließlich sollten einer Xamarin.Mac-App überprüfen, um festzustellen, ob die Windows geänderte Inhalt enthalten und dem Benutzer ermöglicht, die Änderungen vor dem Beenden speichern. Zu diesem Zweck Bearbeiten Ihrer `AppDelegate.cs` Datei außer Kraft, indem die `ApplicationShouldTerminate` Methode und legen Sie ihn wie folgt aussehen:

```csharp
public override NSApplicationTerminateReply ApplicationShouldTerminate (NSApplication sender)
{
    // See if any window needs to be saved first
    foreach (NSWindow window in NSApplication.SharedApplication.Windows) {
        if (window.Delegate != null && !window.Delegate.WindowShouldClose (this)) {
            // Did the window terminate the close?
            return NSApplicationTerminateReply.Cancel;
        }
    }

    // Allow normal termination
    return NSApplicationTerminateReply.Now;
}
```

<a name="Working_with_Multiple_Windows" />

## <a name="working-with-multiple-windows"></a>Arbeiten mit mehreren Windows

Die meisten basierend Dokuments Macintosh-Anwendungen können mehrere Dokumente gleichzeitig bearbeiten. Beispielsweise kann ein Text-Editor mehrere Textdateien, die für die Bearbeitung zur gleichen Zeit geöffnet haben. Standardmäßig verfügt die neue Xamarin.Mac-Anwendung eine **Datei** Menü mit einer **neu** Element automatisch verbunden – bis zu der `newDocument:` **Aktion**.

Wir werden dieses neue Element zu aktivieren und ermöglicht dem Benutzer, mehrere Kopien von unserem Hauptfenster so bearbeiten Sie mehrere Dokumente gleichzeitig öffnen.

Ermöglicht das Bearbeiten von unserem `AppDelegate.cs` Datei, und fügen Sie die folgenden berechneten Eigenschaft hinzu:

```csharp
public int UntitledWindowCount { get; set;} =1;
```

Wir verwenden dies, um die Anzahl der nicht gespeicherte Dateien nachzuverfolgen, daher geben wir Feedback an den Benutzer (pro Apple Richtlinien wie oben beschrieben) können.

Als Nächstes fügen Sie die folgende Methode hinzu:

```csharp
[Export ("newDocument:")]
void NewDocument (NSObject sender) {
    // Get new window
    var storyboard = NSStoryboard.FromName ("Main", null);
    var controller = storyboard.InstantiateControllerWithIdentifier ("MainWindow") as NSWindowController;

    // Display
    controller.ShowWindow(this);

    // Set the title
    controller.Window.Title = (++UntitledWindowCount == 1) ? "untitled" : string.Format ("untitled {0}", UntitledWindowCount);
}
```

Dieser Code erstellt eine neue Version des Controllers Fenster, lädt das neue Fenster, können sie die Haupt- und das Fenster "Schlüssel" und Titel wird. Wenn wir unsere Anwendung auszuführen, und wählen Sie jetzt **neu** aus der **Datei** Menü ein neues Editor-Fenster wird geöffnet und angezeigt werden:

[![](window-images/display04.png "Ein neues unbenanntes Fenster wurde hinzugefügt.")](window-images/display04.png#lightbox)

Wenn wir öffnen den **Windows** Menü, sehen Sie die Anwendung automatisch nachverfolgen und Behandeln von unserem geöffneten Fenster:

[![](window-images/display05.png "Klicken Sie im Windows-Menü")](window-images/display05.png#lightbox)

Weitere Informationen zum Arbeiten mit Menüs in einer Xamarin.Mac-Anwendung finden Sie unserem [arbeiten mit Menüs](~/mac/user-interface/menu.md) Dokumentation.

<a name="Getting_the_Currently_Active_Window" />

### <a name="getting-the-currently-active-window"></a>Das derzeit aktive Fenster abrufen

In einer Xamarin.Mac-Anwendung, die mehrere Fenster (Dokumente) öffnen können, sind Sie müssen manchmal werden zum Abrufen der aktuellen, obersten Fensters (wichtigsten Fenster). Der folgende Code gibt die wichtigsten Fenster zurück:

```csharp
var window = NSApplication.SharedApplication.KeyWindow;
```

Sie können in einer beliebigen Klasse oder Methode, greifen Sie auf die aktuelle, Schlüssel-Fenster muss, aufgerufen werden. Wenn kein Fenster derzeit geöffnet ist, wird zurückgegeben, `null`.

<a name="Accessing-All-App-Windows" />

### <a name="accessing-all-app-windows"></a>Zugriff auf alle App-Windows

Es gibt möglicherweise Zeiten müssen Sie alle Fenster zugegriffen werden soll, einer Xamarin.Mac-app aktuell geöffnet hat. Z. B. ist erkundigen, wenn eine Datei, die die Benutzer öffnen möchte noch in einem vorhandenen Fenster geöffnet.

Die `NSApplication.SharedApplication` verwaltet eine `Windows` -Eigenschaft, die ein Array von alle offenen Fenster in Ihrer app enthält. Sie können dieses Array für Zugriff auf alle aktuellen Windows von der app durchlaufen. Zum Beispiel:

```csharp
// Is the file already open?
for(int n=0; n<NSApplication.SharedApplication.Windows.Length; ++n) {
    var content = NSApplication.SharedApplication.Windows[n].ContentViewController as ViewController;
    if (content != null && path == content.FilePath) {
        // Bring window to front
        NSApplication.SharedApplication.Windows[n].MakeKeyAndOrderFront(this);
        return true;
    }
}
```

Im Beispielcode wandeln wir jedes zurückgegebene Fenster mit den benutzerdefinierten `ViewController` -Klasse in unserer app und den Wert eines benutzerdefinierten Tests `Path` Eigenschaft für den Pfad einer Datei, die die Benutzer öffnen möchte. Wenn die Datei bereits geöffnet ist, bringen wir das Fenster im Vordergrund.

<a name="Adjusting_the_Window_Size_in_Code" />

## <a name="adjusting-the-window-size-in-code"></a>Anpassen der Größe des Fensters im Code

Es gibt Situationen, wenn die Anwendung zum Ändern der Größe eines Fensters im Code muss. Zum Ändern der Größe und Position eines Fensters, passen Sie es der `Frame` Eigenschaft. Wenn Sie die Größe eines Fensters anpassen zu können, müssen Sie in der Regel auch anpassen, dessen Ursprung, damit das Fenster am gleichen Speicherort aufgrund des MacOS-Koordinatensystem bleibt.

Im Gegensatz zu iOS, wobei die linke obere Ecke (0,0) darstellt, wird MacOS ein mathematischer Koordinatensystem verwendet, in der unteren linken Ecke des Bildschirms (0,0) darstellt. In iOS erhöhen die Koordinaten, wie Sie auf der rechten Seite nach unten zu verschieben. Unter MacOS erhöhen die Koordinaten in Wert nach oben rechts aus. 

Im folgenden Beispielcode wird die Größe ein Fensters geändert:

```csharp
nfloat y = 0;

// Calculate new origin
y = Frame.Y - (768 - Frame.Height);

// Resize and position window
CGRect frame = new CGRect (Frame.X, y, 1024, 768);
SetFrame (frame, true);
```

> [!IMPORTANT]
> Wenn Sie eine Windows-Größe und Position im Code anpassen, müssen Sie sicherstellen, dass Sie die minimale und maximale Größe berücksichtigt, die Sie in Interface Builder festgelegt haben. Dies wird nicht automatisch berücksichtigt, und um das Fenster größer oder kleiner als diese Grenzwerte können.

<a name="Monitoring-Window-Size-Changes" />

## <a name="monitoring-window-size-changes"></a>Überwachen die Fenstergröße ändert

Es gibt möglicherweise Zeiten, in dem Sie Änderungen an der Größe eines Fensters, in einer Xamarin.Mac-app überwachen müssen. Geben Sie beispielsweise Folgendes ein, um Inhalt auf die neue Größe neu zeichnen.

Um Änderungen zu überwachen, stellen Sie zunächst sicher, dass Sie eine benutzerdefinierte Klasse für den Fenster-Controller in Interface Builder von Xcode zugewiesen haben. Z. B. `MasterWindowController` in der folgenden:

[![](window-images/resize01.png "Der Identitätsinspektor")](window-images/resize01.png#lightbox)

Als Nächstes bearbeiten Sie die benutzerdefinierte Fenstercontroller-Klasse und den Monitor der `DidResize` Ereignis des Controllers im Fenster größenänderungen live benachrichtigt werden sollen. Zum Beispiel:

```csharp
public override void WindowDidLoad ()
{
    base.WindowDidLoad ();

    Window.DidResize += (sender, e) => {
        // Do something as the window is being live resized
    };
}
```

Optional können Sie die `DidEndLiveResize` Ereignis nur benachrichtigt werden, nachdem der Benutzer abgeschlossen ist, ändern die Größe des Fensters. Beispiel:

```csharp
public override void WindowDidLoad ()
{
    base.WindowDidLoad ();

        Window.DidEndLiveResize += (sender, e) => {
        // Do something after the user's finished resizing
        // the window
    };
}
```

<a name="Setting_a_Window’s_Title_and_Represented_File" />

## <a name="setting-a-windows-title-and-represented-file"></a>Der Titel und Datei dargestellt ein Fenster festlegen

Bei der Verwendung von Windows, die Dokumente darstellen `NSWindow` verfügt über eine `DocumentEdited` , auch wenn-Eigenschaftensatz auf `true` kleinen Punkt angezeigt, in der Schaltfläche "Schließen", um dem Benutzer ein Hinweis darauf, dass die Datei geändert wurde, und vor dem Schließen gespeichert werden soll.

Ermöglicht das Bearbeiten von unserem `ViewController.cs` Datei, und nehmen Sie die folgenden Änderungen:

```csharp
public bool DocumentEdited {
    get { return View.Window.DocumentEdited; }
    set { View.Window.DocumentEdited = value; }
}
...

public override void ViewWillAppear ()
{
    base.ViewWillAppear ();

    // Set Window Title
    this.View.Window.Title = "untitled";

    View.Window.WillClose += (sender, e) => {
        // is the window dirty?
        if (DocumentEdited) {
            var alert = new NSAlert () {
                AlertStyle = NSAlertStyle.Critical,
                InformativeText = "We need to give the user the ability to save the document here...",
                MessageText = "Save Document",
            };
            alert.RunModal ();
        }
    };
}

public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Show when the document is edited
    DocumentEditor.TextDidChange += (sender, e) => {
        // Mark the document as dirty
        DocumentEdited = true;
    };

    // Overriding this delegate is required to monitor the TextDidChange event
    DocumentEditor.ShouldChangeTextInRanges += (NSTextView view, NSValue[] values, string[] replacements) => {
        return true;
    };

}
```

Wir sind auch Überwachung der `WillClose` Ereignis in das Fenster, und Überprüfen des Zustands der `DocumentEdited` Eigenschaft. Ist dies `true` müssen wir dem Benutzer die Möglichkeit, die Änderungen an der Datei speichern bieten. Wenn wir unsere app ausführen, und geben Sie Text ein, wird der Punkt angezeigt:

[![](window-images/file01.png "Ein Fenster geändert")](window-images/file01.png#lightbox)

Wenn wir versuchen, das Fenster zu schließen, erhalten wir eine Warnung aus:

[![](window-images/file02.png "Anzeigen eines Speichervorgangs Dialogfeld")](window-images/file02.png#lightbox)

Wenn wir ein Dokument aus einer Datei laden können wir den Titel des Fensters festlegen, zu der Datei mithilfe der `window.SetTitleWithRepresentedFilename (Path.GetFileName(path));` Methode (angesichts der Tatsache, dass `path` ist eine Zeichenfolge, das die Datei geöffnet wird). Darüber hinaus können wir legen Sie die URL der Datei mit den `window.RepresentedUrl = url;` Methode.

Wenn die URL in einen Dateityp vom Betriebssystem bezeichnet verweist, wird dessen Symbol in der Titelleiste angezeigt. Wenn der rechten Maustaste auf das Symbol klickt, wird der Pfad zur Datei angezeigt.

Ermöglicht das Bearbeiten der `AppDelegate.cs` Datei, und fügen Sie die folgende Methode hinzu:

```csharp
[Export ("openDocument:")]
void OpenDialog (NSObject sender)
{
    var dlg = NSOpenPanel.OpenPanel;
    dlg.CanChooseFiles = true;
    dlg.CanChooseDirectories = false;

    if (dlg.RunModal () == 1) {
        // Nab the first file
        var url = dlg.Urls [0];

        if (url != null) {
            var path = url.Path;

            // Get new window
            var storyboard = NSStoryboard.FromName ("Main", null);
            var controller = storyboard.InstantiateControllerWithIdentifier ("MainWindow") as NSWindowController;

            // Display
            controller.ShowWindow(this);

            // Load the text into the window
            var viewController = controller.Window.ContentViewController as ViewController;
            viewController.Text = File.ReadAllText(path);
                    viewController.View.Window.SetTitleWithRepresentedFilename (Path.GetFileName(path));
            viewController.View.Window.RepresentedUrl = url;

        }
    }
}
```

Wählen Sie jetzt, wenn wir unsere app ausführen, **öffnen...**  aus der **Datei** wählen Sie im Menü, eine Textdatei, die von, der **öffnen** Dialogfeld ein, und öffnen Sie sie:

[![](window-images/file03.png "Ein geöffnetes Dialogfenster")](window-images/file03.png#lightbox)

Die Datei wird angezeigt, und der Titel wird mit dem Symbol der Datei festgelegt werden:

[![](window-images/file04.png "Der Inhalt einer Datei geladen")](window-images/file04.png#lightbox)

<a name="Adding_a_New_Window_to_a_Project" />

## <a name="adding-a-new-window-to-a-project"></a>Hinzufügen von einem neuen Fenster zu einem Projekt

Abgesehen von der wichtigsten Dokumentfenster müssen eine Xamarin.Mac-Anwendung möglicherweise andere Typen von Windows für den Benutzer, z. B. Einstellungen oder Inspektor-Fenster angezeigt.

Um ein neues Fenster hinzuzufügen, führen Sie folgende Schritte aus:

1. In der **Projektmappen-Explorer**, doppelklicken Sie auf die `Main.storyboard` Datei, die sie zur Bearbeitung in Xcode Interface Builder zu öffnen.
2. Ziehen Sie ein neues **Fenstercontroller** aus der **Bibliothek** und legen Sie diese auf die **Entwurfsoberfläche**:

    [![](window-images/new01.png "Auswählen eines neuen Fensters Controllers in der Bibliothek")](window-images/new01.png#lightbox)
3. In der **Identitätsinspektor**, geben Sie `PreferencesWindow` für die **Storyboard-ID**: 

    [![](window-images/new02.png "Festlegen der Storyboard-ID")](window-images/new02.png#lightbox)
5. Entwerfen Sie Ihre Schnittstelle: 

    [![](window-images/new03.png "Entwerfen der Benutzeroberfläche")](window-images/new03.png#lightbox)
6. Öffnen Sie im Menü der App (`MacWindows`) Option **Einstellungen...** , Strg + Klick und ziehen Sie auf das neue Fenster: 

    [![](window-images/new05.png "Erstellen eines segues")](window-images/new05.png#lightbox)
7. Wählen Sie **anzeigen** im Popupmenü.
6. Die Änderungen zu speichern und zurück zu Visual Studio für Mac mit Xcode synchronisiert.

Wenn wir den Code auszuführen, und wählen Sie die **Einstellungen...**  aus der **Anwendungsmenü**, das Fenster wird angezeigt:

[![](window-images/new04.png "Eine Beispiel-Menü \"Einstellungen\"")](window-images/new04.png#lightbox)

<a name="Working_with_Panels" />

## <a name="working-with-panels"></a>Verwenden von Bereichen

Wie zu Beginn dieses Artikels erwähnt, wird ein Panel über andere Fenster gleitet, und stellt Sie bereit, Tools oder Steuerelemente, denen Benutzer mit arbeiten können, wenn Dokumente geöffnet sind. 

Genau wie jede andere Art von Fenster, das Sie erstellen und in Ihre Xamarin.Mac-Anwendung verwenden, ist der Prozess im Wesentlichen identisch:

1. Fügen Sie eine neue Fensterdefinition zum Projekt hinzu.
2. Doppelklicken Sie auf die `.xib` Datei, um das Fenster-Design für die Bearbeitung in Interface Builder von Xcode zu öffnen.
2. Legen Sie alle erforderlichen Eigenschaften in der **Attributinspektor** und **Größeninspektor**.
4. Ziehen Sie in den Steuerelementen erforderlich, um Ihre Schnittstelle erstellen und konfigurieren sie in der **Attributinspektor**.
5. Verwenden der **Größeninspektor** , behandeln das Ändern der Größe für Ihre UI-Elemente.
6. Machen Sie das Fenster des UI-Elemente für C#-Code über **Outlets** und **Aktionen**.
7. Speichern Sie die Änderungen zu, und wechseln Sie zurück zu Visual Studio für Mac mit Xcode synchronisiert.

In der **Attributinspektor**, Sie haben die folgenden Optionen, die bestimmten Bereichen:

[![](window-images/panel03.png "Attribute Inspector")](window-images/panel03.png#lightbox)

- **Stil** -können Sie das Format des Bereichs von anpassen: Reguläre Bereich (sieht wie ein Standardfenster), Utility Bereich (hat eine kleinere Titelleiste), HUD-Bereich (transparent ist und die Titelleiste ist Teil des Hintergrunds).
- **Nicht aktiviert** -bestimmt in der Bereich zum wichtigsten Fenster wird.
- **Dokumentieren Sie modale** -wenn Dokument modales, im Bereich wird nur "float" oberhalb der Anwendung Windows, ansonsten gleitet sie überzeugt.


Um einen neuen Bereich hinzuzufügen, führen Sie folgende Schritte aus:

1. In der **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und wählen Sie **hinzufügen** > **neue Datei...** .
2. Wählen Sie in das Dialogfeld "neue Datei" **Xamarin.Mac** > **Cocoa-Fenster mit Controller**:

    [![](window-images/panels00.png "Hinzufügen eines neuen Fensters-Controllers")](window-images/panels00.png#lightbox)
3. Geben Sie für den **Namen** `DocumentPanel` ein, und klicken Sie auf **Neu**.
4. Doppelklicken Sie auf die `DocumentPanel.xib` Datei, die sie zur Bearbeitung in Interface Builder zu öffnen: 

    [![](window-images/new02.png "Bearbeiten den Zugriffsbereich")](window-images/new02.png#lightbox)
5. Löschen Sie das vorhandene Fenster, und ziehen Sie ein Panel aus dem **Bibliotheksinspektor** in die **Schnittstellen-Editor**: 

    [![](window-images/panels01.png "Löschen von vorhandenen Fenster")](window-images/panels01.png#lightbox)
6. Verknüpfen Sie den Bereich bis zu der **Besitzer der Datei** - **Fenster** - **Outlet**: 

    [![](window-images/panels02.png "Ziehen verknüpfen, um den Bereich")](window-images/panels02.png#lightbox)
7. Wechseln Sie zu der **Identitätsinspektor** , und legen Sie im Bereich der Klasse auf `DocumentPanel`: 

    [![](window-images/panels03.png "Festlegen des Bereichs-Klasse")](window-images/panels03.png#lightbox)
6. Die Änderungen zu speichern und zurück zu Visual Studio für Mac mit Xcode synchronisiert.
7. Bearbeiten der `DocumentPanel.cs` Datei, und ändern Sie die Definition der Klasse auf den folgenden: 

    `public partial class DocumentPanel : NSPanel`
8. Speichern Sie die Änderungen in der Datei.

Bearbeiten der `AppDelegate.cs` Datei, und stellen die `DidFinishLaunching` Methode sehen wie folgt:

```csharp
public override void DidFinishLaunching (NSNotification notification)
{
        // Display panel
    var panel = new DocumentPanelController ();
    panel.Window.MakeKeyAndOrderFront (this);
}

```

Wenn wir unsere Anwendung ausführen, wird im Bereich angezeigt werden:

[![](window-images/panels04.png "Der Bereich in einer ausgeführten app")](window-images/panels04.png#lightbox)

> [!IMPORTANT]
> Bereich Windows von Apple veraltet und sollte ersetzt werden durch **Nachrichtenüberprüfer-Schnittstellen**. Ein vollständiges Beispiel zum Erstellen einer **Inspektor** in einer Xamarin.Mac-app finden Sie unsere [MacInspector](https://developer.xamarin.com/samples/mac/MacInspector/) Beispiel-app.

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wird eine ausführliche Übersicht über das Arbeiten mit Windows und Bereiche in einer Xamarin.Mac-Anwendung verwendet. Wir haben gesehen, die verschiedenen Typen und Verwendung von Windows und Bereichen, wie Sie Windows und Bereiche in Xcode Interface Builder erstellen und verwalten und wie Sie mit Windows und Bereiche in C#-Code zu arbeiten.

## <a name="related-links"></a>Verwandte Links

- [MacWindows (Beispiel)](https://developer.xamarin.com/samples/mac/MacWindows/)
- [MacInspector (Beispiel)](https://developer.xamarin.com/samples/mac/MacInspector/)
- [Hello, Mac (Hallo, Mac)](~/mac/get-started/hello-mac.md)
- [Arbeiten mit Menüs](~/mac/user-interface/menu.md)
- [Eingaberichtlinien für OS X](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Einführung in Windows](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
