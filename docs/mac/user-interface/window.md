---
title: Windows
description: In diesem Artikel wird das Arbeiten mit Windows- und Bereiche in einer Anwendung Xamarin.Mac behandelt. Erstellen von Windows und Bereiche in Xcode und Schnittstelle-Generator Laden aus dem Storyboards und .xib Dateien und Arbeiten mit diesen programmgesteuerten beschrieben.
ms.topic: article
ms.prod: xamarin
ms.assetid: 4F6C67E9-BBFF-44F7-B29E-AB47D7F44287
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 4b8de30cecb738fecb13616a3b796c0b4fa5a51a
ms.sourcegitcommit: 7b88081a979381094c771421253d8a388b2afc16
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2018
---
# <a name="windows"></a>Windows

_In diesem Artikel wird das Arbeiten mit Windows- und Bereiche in einer Anwendung Xamarin.Mac behandelt. Erstellen von Windows und Bereiche in Xcode und Schnittstelle-Generator Laden aus dem Storyboards und .xib Dateien und Arbeiten mit diesen programmgesteuerten beschrieben._

Bei der Arbeit mit c# und .NET in einer Anwendung Xamarin.Mac Sie haben Zugriff auf den gleichen Windows und Bilder, die ein Entwickler arbeiten in unter *Objective-C* und *Xcode* verfügt. Da Xamarin.Mac direkt mit Xcode integriert ist, können Sie die Xcode _Schnittstelle-Generator_ erstellen und Verwalten von Windows und -Bereiche (oder erstellen sie optional direkt im C#-Code).

Basierend auf ihren Zweck, kann eine Anwendung Xamarin.Mac stellen ein oder mehrere Fenster auf dem Bildschirm zum Verwalten und koordinieren die Informationen, die er zeigt an, und arbeitet mit. Die Hauptfunktionen des Fensters werden:

1. Einen Bereich angeben, in dem Ansichten und Steuerelemente gespeichert und verwaltet werden können.
2. Um zu übernehmen, und reagieren auf Ereignisse als Reaktion auf Benutzerinteraktion mit Tastatur und Maus.

Windows kann, modale (z. B. ein Dialogfeld "Export", die verworfen werden muss, bevor die Anwendung fortgesetzt werden kann) oder in einem nicht modalen Status (z. B. einem Text-Editor, der mehrere Dokumente gleichzeitig geöffnet sein kann) verwendet werden.

Bereiche sind eine besondere Art von Fenster (eine Unterklasse des basistexts `NSWindow` Klasse), in der Regel eine zusätzliche Funktion in einer Anwendung, z. B. Dienstprogramm Windows wie Text-Format Inspektoren und System-Farbauswahl dienen.

[![](window-images/intro01.png "Bearbeiten eines Fensters in Xcode")](window-images/intro01.png#lightbox)

In diesem Artikel werden die Grundlagen der Arbeit mit Windows und Bereiche in einer Anwendung Xamarin.Mac eingegangen. Wird mit hoher vorgeschlagen, dass Sie über arbeiten die [Hello, Mac](~/mac/get-started/hello-mac.md) Artikel zuerst, insbesondere die [Einführung in Xcode und Benutzeroberflächen-Generator](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) und [Steckdosen und Aktionen](~/mac/get-started/hello-mac.md#Outlets_and_Actions) Abschnitte, wie sie behandelt wichtige Konzepte und Techniken, die in diesem Artikel verwendet werden.

Sie möchten einen Blick auf die [Verfügbarmachen von C#-Klassen / Methoden für Objective-C-](~/mac/internals/how-it-works.md) Teil der [Xamarin.Mac Internals](~/mac/internals/how-it-works.md) dokumentieren, es wird erläutert, die `Register` und `Export` Befehle verwendet, um über das Netzwerk Ihrer C#-Klassen für Objective-C-Objekte und Elemente der Benutzeroberfläche von Volltextkatalogen.

<a name="Introduction_to_Windows" />

## <a name="introduction-to-windows"></a>Einführung in Windows

Wie bereits erwähnt, wird ein Fenster stellt einen Bereich in dem Sichten und die Steuerelemente platziert und verwaltet werden können und reagiert auf Ereignisse basierend auf Benutzerinteraktionen (entweder über die Tastatur oder Maus).

Gemäß den Apple an gibt es fünf Haupttypen von Windows in einem MacOS App:

- **Dokumentfenster** -ein Dokumentfenster dateibasierte Benutzerdaten, z. B. einem Arbeitsblatt oder einem Textdokument enthält.
- **App-Fenster** -ein app-Fenster wird das Hauptfenster der Anwendung kein Dokument basiert (z. B. die Kalender-app auf einem Mac) ist.
- **Bereich** – ein Panel gleitet über anderen Fenstern und stellt Tools bereit, oder öffnen Sie die Steuerelemente, die Benutzer mit arbeiten können, wenn Dokumente sind. In einigen Fällen kann ein Panel transparent sein (z. B. beim Arbeiten mit großen Grafiken).
- **Dialogfeld** -ein Dialogfeld angezeigt, die als Antwort auf eine Benutzeraktion wird und in der Regel stellt Möglichkeiten Benutzer können die Aktion ausgeführt. Ein Dialogfeld erfordert eine Antwort des Benutzers an, bevor sie geschlossen werden kann. (Siehe [arbeiten mit Dialogen](~/mac/user-interface/dialog.md))
- **Warnungen** -eine Warnung ist eine Sonderform des Dialogfelds ", der angezeigt wird, tritt ein schwerwiegendes Problem (z. B. Fehler) oder als Warnung (z. B. Vorbereitung zum Löschen einer Datei). Ein Dialogfeld wird eine Warnung Quellformat, sodass es auch eine Benutzerantwort, bevor sie geschlossen werden kann. (Siehe [arbeiten mit Warnungen](~/mac/user-interface/alert.md))

Weitere Informationen finden Sie unter der [zu Windows](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowAppearanceBehavior.html#//apple_ref/doc/uid/20000957-CH33-SW1) Abschnitt der Apple [OS X-Richtlinien für menschliche-Schnittstelle](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)

<a name="Main_Key_and_Inactive_Windows" />

### <a name="main-key-and-inactive-windows"></a>Main, Schlüssel und der inaktiven Fenstern

Windows in einer Anwendung Xamarin.Mac können Aussehen und Verhalten auf wie derzeit der Benutzer mit ihnen interagiert auf verschiedene Weise basierend. -Sicherheitsverhaltens Dokument oder App-Fenster, die aktuell den Fokus der Aufmerksamkeit des Benutzers wird aufgerufen, die _Hauptfenster_. In den meisten Fällen in diesem Fenster werden auch die _Fenster Key_ (das Fenster, die derzeit eine Benutzereingabe akzeptiert). Aber nicht immer die Groß-/Kleinschreibung, z. B. ein Farbwähler geöffnet sein, und der Schlüssel-Fenster, dem der Benutzer zur Änderung des Zustands eines Elements im Dokumentfenster (was immer noch im Hauptfenster wäre) mit interagiert werden konnte.

Der Haupt- und der Schlüssel Windows (Wenn sie separate befinden) sind immer aktiv, _inaktiven Fenstern_ geöffneten Fenstern, die nicht in den Vordergrund gestellt werden. Eine Text-Editor-Anwendung könnte z. B. mehr als ein Dokument besitzen gleichzeitig geöffnet, nur das Hauptfenster dann aktiv wäre, alle anderen würde inaktiv sein. 

Weitere Informationen finden Sie unter der [zu Windows](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowAppearanceBehavior.html#//apple_ref/doc/uid/20000957-CH33-SW1) Abschnitt der Apple [OS X-Richtlinien für menschliche-Schnittstelle](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)

<a name="Naming_Windows" />

### <a name="naming-windows"></a>Benennen von Windows

Ein Fenster mit einer Titelleiste öffnen kann, und wenn der Titel angezeigt wird, ist dies normalerweise den Namen der Anwendung, die den Namen des Dokuments, bearbeitet oder die Funktion des Fensters (z. B. Inspektor). Einige Anwendungen keine Titelleiste angezeigt werden, da sie nicht mit Dokumenten arbeiten und von Sight erkannt werden.

Apple empfehlen die folgenden Richtlinien:

- Verwenden Sie den Anwendungsnamen für den Titel eines Fensters main, kein Dokument ist. 
- Benennen Sie ein neuen Dokumentfenster `untitled`. Für die erste neue Dokument keine Zahl für den Titel der anfügen (z. B. `untitled 1`). Wenn der Benutzer ein neues Dokument vor dem Speichern und das erste anspruchsvolle erstellt, rufen Sie dieses Fenster `untitled 2`, `untitled 3`usw.

Weitere Informationen finden Sie unter der [Naming Windows](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowNaming.html#//apple_ref/doc/uid/20000957-CH35-SW1) Abschnitt der Apple [OS X-Richtlinien für menschliche-Schnittstelle](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)

<a name="Full-Screen_Windows" />

### <a name="full-screen-windows"></a>Vollbildmodus

MacOS, Fenster einer Anwendung kann wechseln Sie in der Vollbildmodus ausblenden Alles einschließlich der Menüleiste der Anwendung (die durch das Verschieben des Cursors an den oberen Rand des Bildschirms identifizierte werden können) störende bereitstellen freien Interaktion mit der Anwendung ist ein Inhaltselement.

Apple empfiehlt es sich um die folgenden Richtlinien:

- Bestimmen Sie, ob es sinnvoll ist für ein Fenster in den Vollbildmodus zu wechseln. Anwendungen, die kurze Aktivitäten (z. B. einen Rechner) darf keinen Vollbild-Modus bereit.
- Blenden Sie die Symbolleiste aus, wenn die Vollbild-Aufgabe erforderlich ist. In der Regel wird die Symbolleiste im Vollbildmodus ausgeblendet.
- Fenster Vollbild-sollten alle Features Benutzer müssen die Aufgabe abgeschlossen haben.
- Finder-Interaktion möglichst nicht, während der Benutzer, geben Sie im Vollbildmodus ist.
- Nutzen Sie die erhöhte Bildschirmbereich ohne verlagern den Fokus von der Hauptaufgabe Weg.

Weitere Informationen finden Sie unter der [Vollbildmodus Windows](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/FullScreen.html#//apple_ref/doc/uid/20000957-CH61-SW1) Abschnitt der Apple [OS X-Richtlinien für menschliche-Schnittstelle](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)

<a name="Panels" />

### <a name="panels"></a>Panels

Ein Bereich ist ein zusätzliches Fenster, das Steuerelemente und Optionen, die Einfluss auf das aktive Dokument oder die Auswahl (z. B. das System Farbauswahl) enthält:

[![](window-images/panel01.png "Eine Farbpalette")](window-images/panel01.png#lightbox)

Bereiche können es sich um _App-spezifisches_ oder _systemweiten_. App-spezifisches Bereiche über den oberen Rand der Anwendung Dokumentfenster float und nicht mehr angezeigt, wenn die Anwendung im Hintergrund ist. Systemweiten Bereiche (z. B. die **Schriftarten** Bereich), "float", zusätzlich zu allen geöffneten Fenstern unabhängig von der Anwendung. 

Apple empfiehlt es sich um die folgenden Richtlinien:

- Im Allgemeinen verwenden eines Standardbereichs, transparente Bereiche sollte nur selten und für grafisch anspruchsvolle Aufgaben verwendet werden.
- Erwägen Sie ein Panel Benutzern gewähren einfachen Zugriff auf wichtige Steuerelemente oder Informationen, die direkt auf ihre Aufgabe auswirkt.
- Ausblenden und Anzeigen von Bereichen nach Bedarf.
- Bereiche sollten immer Titelleiste enthalten.
- Bereiche sollten nicht mit eine aktiven Minimieren-Schaltfläche enthalten.

#### <a name="inspectors"></a>Inspektoren

Die meisten moderne MacOS Anwendungen vorhanden, zusätzliche Steuerelemente und Optionen, die Einfluss auf das aktive Dokument oder die Auswahl als _Inspektoren_ , sind Teil des Hauptfensters (z. B. die **Seiten** app siehe unten), Anstatt Bereich Windows:

[![](window-images/panel02.png "Ein Beispiel-Inspektor")](window-images/panel02.png#lightbox)

Weitere Informationen finden Sie unter der [Bereiche](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowPanels.html#//apple_ref/doc/uid/20000957-CH42-SW1) Abschnitt der Apple [OS X-Richtlinien für menschliche Benutzeroberfläche](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/) und unseren [MacInspector](https://developer.xamarin.com/samples/mac/MacInspector/) Beispiel-app für eine vollständige Implementierung der ein **Inspektor Schnittstelle** in einer Xamarin.Mac-app.

<a name="Creating_and_Maintaining_Windows_in_Xcode" />

## <a name="creating-and-maintaining-windows-in-xcode"></a>Erstellen und Verwalten von Windows in Xcode

Wenn Sie eine neue Xamarin.Mac Kakao-Anwendung erstellen, erhalten Sie Standardfensters leer ist, wird standardmäßig an. Dieses Windows wird definiert, einem `.storyboard` automatisch im Projekt enthaltene Datei. So bearbeiten Sie die Windows-Design in der **Projektmappen-Explorer**, doppelklicken klicken Sie auf die `Main.storyboard` Datei:

[![](window-images/edit01.png "Die Haupt-Storyboard auswählen")](window-images/edit01.png#lightbox)

Dies öffnet das Fenster Design in Xcodes Benutzeroberflächen-Generator:

[![](window-images/edit02.png "Bearbeiten die Benutzeroberfläche in Xcode")](window-images/edit02.png#lightbox)

In der **Attribut Inspektor**, es gibt mehrere Eigenschaften, die Sie zum Definieren und steuern die Fenster verwenden können:

- **Titel** -Dies ist der Text, der in der Titelleiste des Fensters angezeigt wird.
- **AutoSpeichern** -Dies ist die _Schlüssel_ , wird verwendet, um das Fenster-ID, wenn die Position und Einstellungen automatisch gespeichert werden.
- **Titelleiste** -zeigt das Fenster eine Titelleiste.
- **Vereinheitlichte Titel und Symbolleiste** – Wenn das Fenster eine Symbolleiste enthält, muss es Teil der Titelleiste.
- **Vollständige Größe Inhalt anzeigen** -Inhaltsbereichs des Fensters, um unter der Titelleiste angezeigt werden können.
- **Schatten** -verfügt über das Fenster einen Schatten.
- **Strukturierten** -texturierten Windows können Auswirkungen (z. B. Dynamik) und können durch Ziehen an einer beliebigen Stelle im Textkörper verschoben werden.
- **Schließen Sie** -verfügt über das Fenster eine Schaltfläche "Schließen".
- **Minimieren Sie** -verfügt über das Fenster eine Schaltfläche "Minimieren".
- **Ändern der Größe** -verfügt über das Fenster eines Steuerelements zum Ändern der Größe.
- **Symbolleisten-Schaltfläche** -verfügt über das Fenster eine Symbolleisten-Schaltfläche einblenden/ausblenden.
- **Wiederherstellbare** -Position des Fensters und die Einstellungen automatisch gespeichert und wiederhergestellt wird.
- **Starten Sie sichtbar an** -wird das Fenster automatisch angezeigt, wenn die `.xib` Datei geladen wird.
- **Bei Deaktivierung ausblenden** -Fenster wird ausgeblendet werden, wenn die Anwendung in den Hintergrund wechselt.
- **Freigeben, wenn geschlossen** -ist das Fenster aus dem Arbeitsspeicher gelöscht werden, wenn er geschlossen wurde.
- **Immer Anzeige QuickInfos** -QuickInfos konstant angezeigt werden.
- **Ansicht Schleife berechnet** -ist die Anzeigereihenfolge, die neu berechnet, bevor das Fenster gezeichnet wird.
- **Leerzeichen**, **Exposé** und **Cycling** -alle definieren, wie das Fenster in diesen Umgebungen MacOS verhält. 
- **Vollbild** -bestimmt, ob dieses Fenster im Vollbildmodus eingeben kann. 
- **Animation** -steuert den Typ der Animation für das Fenster verfügbar.
- **Darstellung** -steuert die Darstellung des Fensters. Es ist jetzt nur eine Darstellung des Aqua.

Finden Sie in der Apple- [Einführung in Windows](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1) und [NSWindow](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSWindow_Class/index.html#//apple_ref/occ/cl/NSWindow) Weitere Einzelheiten s. Dokumentation.

<a name="Setting_the_Default_Size_and_Location" />

### <a name="setting-the-default-size-and-location"></a>Die standardmäßige Größe und Position festlegen

Um die Anfangsposition des Fensters festgelegt und dessen Größe zu steuern, wechseln Sie zu der **Größe Inspektor**:

[![](window-images/edit07.png "Die standardmäßige Größe und Position")](window-images/edit07.png#lightbox)

Von hier aus können die Anfangsgröße des Fensters, weisen Sie ihm eine minimale und maximale Größe, legen Sie die ursprüngliche Position auf dem Bildschirm und steuern die Rahmen um das Fenster.

<a name="Setting-a-Custom-Main-Window-Controller" />

### <a name="setting-a-custom-main-window-controller"></a>Festlegen eines benutzerdefinierten Hauptfenster-Controllers

Um Steckdosen und Aktionen zum Verfügbarmachen von Benutzeroberflächenelementen für C#-Code erstellt werden, müssen die Xamarin.Mac app einen benutzerdefinierten Fenster Controller verwenden.

Führen Sie folgende Schritte aus:

1. Öffnen Sie die app-Storyboard in Xcode des Benutzeroberflächen-Generator.
2. Wählen Sie die `NSWindowController` in der Entwurfsoberfläche angezeigt.
3. Wechseln Sie zu der **Identität Inspektor** anzuzeigen, und geben Sie `WindowController` als die **Klassenname**: 

    [![](window-images/windowcontroller01.png "Der Name der Klasse festlegen")](window-images/windowcontroller01.png#lightbox)
4. Die Änderungen zu speichern und zurück zu Visual Studio für Mac synchronisiert werden.
5. Ein `WindowController.cs` Datei wird hinzugefügt werden, um das Projekt in der **Projektmappen-Explorer** in Visual Studio für Mac: 

    [![](window-images/windowcontroller02.png "Auswählen der Windows-Domänencontroller")](window-images/windowcontroller02.png#lightbox)
6. Öffnen Sie das Storyboard in Xcode des Benutzeroberflächen-Generator.
7. Die `WindowController.h` -Datei wird für die Verwendung verfügbar sein: 

    [![](window-images/windowcontroller03.png "Bearbeiten der Datei WindowController.h")](window-images/windowcontroller03.png#lightbox)

<a name="Adding_UI_Elements" />

### <a name="adding-ui-elements"></a>Hinzufügen von UI-Elementen

Ziehen Sie Steuerelemente aus, um den Inhalt eines Fensters zu definieren, die **Bibliothek Inspektor** auf die **Benutzeroberflächen-Editors**. Finden Sie in unserer [Einführung in Xcode und Benutzeroberflächen-Generator](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) Dokumentation weitere Informationen zur Verwendung von Benutzeroberflächen-Generator zum Erstellen und aktivieren die Steuerelemente.

Beispielsweise können wir ziehen Sie die Symbolleiste aus der **Bibliothek Inspektor** auf das Fenster in der **Benutzeroberflächen-Editors**:

[![](window-images/edit03.png "Auswählen einer Symbolleiste aus der Bibliothek")](window-images/edit03.png#lightbox)

Ziehen Sie anschließend eine **Textansicht** und seine Größe ändern, um die Fläche unter der Symbolleiste aufgefüllt:

[![](window-images/edit04.png "Hinzufügen einer Textansicht")](window-images/edit04.png#lightbox)

Da wir möchten die **Textansicht** wir zum Verkleinern und vergrößern die Fenstergröße ändert, wechseln Sie zu der **Einschränkung Editor** und fügen Sie die folgenden Einschränkungen:

[![](window-images/edit05.png "Bearbeiten von Einschränkungen")](window-images/edit05.png#lightbox)

Durch Klicken auf die für **I-Balken rot** am oberen des Editors und durch Klicken auf **4 Einschränkungen hinzufügen**, sagen wir Textansicht Speicherstick auf den angegebenen X-und Y-Koordinaten und vergrößert oder verkleinert werden horizontal und vertikal als die Fenstergröße geändert wird.

Wir schließlich verfügbar zu machen die **Textansicht** codieren, verwenden eine **Steckdose** (vornehmen möchten, wählen Sie die `ViewController.h` Datei):

[![](window-images/edit06.png "Konfigurieren von einer Steckdose")](window-images/edit06.png#lightbox)

Speichern Sie die Änderungen zu, und wechseln Sie zurück zu Visual Studio für Mac mit Xcode zu synchronisieren.

Weitere Informationen zum Arbeiten mit **Steckdosen** und **Aktionen**, finden Sie in unserer [Ausgang und Aktion](~/mac/get-started/hello-mac.md#Outlets_and_Actions) Dokumentation.

<a name="Standard_Window_Workflow" />

### <a name="standard-window-workflow"></a>Standardfenster-Workflow

Für ein Fenster, das Sie erstellen und in der Anwendung Xamarin.Mac verwenden, ist der Vorgang im Grunde identisch mit der nur über wir haben:

1. Für neue Fenster, die nicht die Standardeinstellung, die dem Projekt automatisch hinzugefügt werden, wird dem Projekt eine neue Fensterdefinition hinzugefügt. Dies wird nachfolgend detailliert erläutert werden.
2. Doppelklicken Sie auf die `Main.storyboard` Datei, um den Entwurf der Fenster zur Bearbeitung in Xcodes Benutzeroberflächen-Generator zu öffnen.
3. Ziehen Sie ein neues Fenster in der Benutzeroberfläche entwerfen und verknüpfen Sie das Fenster in Hauptfenster mit _Segues_ (Weitere Informationen finden Sie unter der [Segues](~/mac/platform/storyboards/indepth.md#Segues) Teil unserer [arbeiten mit Storyboards](~/mac/platform/storyboards/indepth.md) Dokumentation).
3. Legen Sie alle erforderlichen Eigenschaften der **Attribut Inspektor** und **Größe Inspektor**.
4. Ziehen Sie in den Steuerelementen, die erforderlich sind, damit Ihre Schnittstelle erstellen und konfigurieren sie in der **Attribut Inspektor**.
5. Verwenden der **Größe Inspektor** behandelt, die größenanpassung von UI-Elementen.
6. Verfügbar machen die Elemente der Benutzeroberfläche des Fensters für C#-Code über **Steckdosen** und **Aktionen**.
7. Speichern Sie die Änderungen zu, und wechseln Sie zurück zu Visual Studio für Mac mit Xcode zu synchronisieren.

Nun, da wir einen grundlegenden Windows erstellt haben, betrachten die typische Prozesse eine Xamarin.Mac wir Anwendung ist, bei der Arbeit mit Windows. 

<a name="Displaying_the_Default_Window" />

## <a name="displaying-the-default-window"></a>Anzeigen des Standard-Fensters

Standardmäßig eine neue Xamarin.Mac-Anwendung zeigt automatisch das Fenster definiert, der `MainWindow.xib` Datei gestartet wird:

[![](window-images/display01.png "Ein Beispiel-Fenster Ausführen")](window-images/display01.png#lightbox)

Da wir das Design dieser oben im Fenster geändert haben, enthält es jetzt einen Standard-Symbolleiste und **Textansicht** Steuerelement. Im folgenden Abschnitt die `Info.plist` Datei ist verantwortlich für die Anzeige dieses Fensters:

[![](window-images/display00.png "Bearbeiten der Datei "Info.plist"")](window-images/display00.png#lightbox)

Die **Hauptbenutzeroberfläche** Dropdownliste wird verwendet, um das Storyboard auswählen, die als die Haupt-app-Benutzeroberfläche verwendet wird (in diesem Fall `Main.storyboard`).

Eine View-Controller wird automatisch dem Projekt hinzugefügt, die Windows-Main steuern, der (zusammen mit seiner Hauptansicht) angezeigt wird. Ist im definiert die `ViewController.cs` Datei und angefügt werden die **des Dateibesitzer** in Benutzeroberflächen-Generator unter der **Identität Inspektor**:

[![](window-images/display02.png "Der Besitzer der Datei festlegen")](window-images/display02.png#lightbox)

Für unsere Fenster möchten wir, sie haben einen beliebigen Titel `untitled` beim ersten Öffnen wir also Außerkraftsetzung der `ViewWillAppear` Methode in der `ViewController.cs` wie folgt aussehen:

```csharp
public override void ViewWillAppear ()
{
    base.ViewWillAppear ();

    // Set Window Title
    this.View.Window.Title = "untitled";
}
``` 

> [!NOTE]
> Legen wir den Wert im Fenster `Title` Eigenschaft in der `ViewWillAppear` -Methode anstelle der `ViewDidLoad` Methode daran, während die Ansicht in den Arbeitsspeicher geladen werden, es nicht noch vollständig instanziiert wird. Wenn es wurde versucht, den Zugriff auf die `Title` Eigenschaft in der `ViewDidLoad` Methode wir erhalten einen `null` Ausnahme, da das Fenster noch nicht wurde erstellt und wired-Up für die Eigenschaft noch.

<a name="Programmatically_Closing_a_Window" />

## <a name="programmatically-closing-a-window"></a>Programmgesteuertes Schließen eines Fensters

Möglicherweise Fälle, in denen Sie programmgesteuert ein Fenster in einer Xamarin.Mac Anwendung schließen möchten, als dass der Benutzer klicken Sie auf des Fensters **schließen** Schaltfläche oder ein Menüelement zu verwenden. MacOS bietet zwei verschiedene Methoden zum Schließen einer `NSWindow` programmgesteuert: `PerformClose` und `Close`.

<a name="PerformClose" />

### <a name="performclose"></a>PerformClose

Aufrufen der `PerformClose` Methode von einer `NSWindow` wird simuliert, in der Benutzer auf des Fensters **schließen** Schaltfläche Hervorhebung vorübergehend die Schaltfläche "", und schließen Sie das Fenster.

Wenn die Anwendung implementiert die `NSWindow`des `WillClose` -Ereignis wird ausgelöst werden, bevor das Fenster geschlossen wird. Wenn das Ereignis zurückgibt `false`, und klicken Sie dann das Fenster wird nicht geschlossen werden. Wenn das Fenster keine **schließen** Schaltfläche "oder" kann nicht geschlossen werden aus irgendeinem Grund das Betriebssystem akustisches Signal ausgegeben wird.

Zum Beispiel:

```csharp
MyWindow.PerformClose(this);
```

Versuchen, schließen Sie die `MyWindow` `NSWindow` Instanz. Wenn er erfolgreich war, wird das Fenster geschlossen werden, sonst akustisches Signal ausgegeben werden, und wird bleibt geöffnet.

<a name="Close" />

### <a name="close"></a>Schließen

Aufrufen der `Close` Methode von einer `NSWindow` den Benutzer auf des Fensters wird nicht simuliert **schließen** Schaltfläche, da Sie die Schaltfläche "" vorübergehend hervorgehoben, schließt er einfach das Fenster.

Ein Fenster muss nicht geschlossen werden, sichtbar und ein `NSWindowWillCloseNotification` Benachrichtigung werden im Verteilungsverlauf auf die Standard-Mitteilungszentrale für das Fenster geschlossen wird.

Die `Close` Methoden unterscheiden sich in zwei wichtigen Aspekten von der `PerformClose` Methode:

1. Nicht erneut ausgelöst werden soll die `WillClose` Ereignis.
2. Nicht simuliert das Benutzer klicken auf die **schließen** Schaltfläche, da Sie die Schaltfläche "" vorübergehend hervorgehoben.

Zum Beispiel:

```csharp
MyWindow.Close();
```

Würden Sie zum Schließen der `MyWindow` `NSWindow` Instanz.

<a name="Modified-Windows-Content" />

## <a name="modified-windows-content"></a>Geänderte Windows-Inhalt

Apple hat in MacOS, eine Möglichkeit, den Benutzer zu informieren bereitgestellt, die den Inhalt eines Fensters (`NSWindow`) wurde vom Benutzer geändert und gespeichert werden muss. Wenn das Fenster geänderten Inhalt enthält, ein kleiner schwarzer Punkt erscheint in der **schließen** Widget:

[![](window-images/close01.png "Ein Fenster mit den geänderten marker")](window-images/close01.png#lightbox)

Wenn der Benutzer versucht, das Fenster schließen oder beenden Sie die Mac-App sind nicht gespeicherte Inhalt des Fensters ändert, sollten Sie präsentieren einer [Dialogfeld](~/mac/user-interface/dialog.md) oder [modale Blatt](~/mac/user-interface/dialog.md) und ermöglicht dem Benutzer ihre Änderungen zu speichern erste:

[![](window-images/close02.png "Ein Blatt wird angezeigt, wenn das Fenster geschlossen wird gespeichert")](window-images/close02.png#lightbox)

### <a name="marking-a-window-as-modified"></a>Markieren ein Fenster geändert

Um ein Fenster, als dass änderte Inhalte zu kennzeichnen, verwenden Sie den folgenden Code:

```csharp
// Mark Window content as modified
Window.DocumentEdited = true;
```

Und nachdem die Änderung gespeichert wurde, löschen Sie die geänderte Flag mit:

```csharp
// Mark Window content as not modified
Window.DocumentEdited = false;
```

### <a name="saving-changes-before-closing-a-window"></a>Speichern von Änderungen vor dem Schließen eines Fensters

Um für den Benutzer ein Fenster zu schließen, und sie zulassen, dass geänderte Inhalte vorab speichern zu beobachten, müssen Sie eine Unterklasse von erstellen `NSWindowDelegate` und überschreiben die `WindowShouldClose` Methode. Zum Beispiel:

```csharp
using System;
using AppKit;
using System.IO;
using Foundation;

namespace SourceWriter
{
    public class EditorWidowDelegate : NSWindowDelegate
    {
        #region Computed Properties
        public NSWindow Window { get; set;}
        #endregion

        #region constructors
        public EditorWidowDelegate (NSWindow window)
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

                // Take action based on resu;t
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

Verwenden Sie den folgenden Code eine Instanz dieses Delegaten an die Fenster angefügt:

```csharp
// Set delegate
Window.Delegate = new EditorWidowDelegate(Window);
```

### <a name="saving-changes-before-closing-the-app"></a>Speichern von Änderungen vor dem Schließen der App

Schließlich sollten Ihre Xamarin.Mac App überprüfen, um festzustellen, ob alle Fenster geänderte Inhalt enthalten, und dem Benutzer die Änderungen vor dem Beenden speichern ermöglicht. Bearbeiten Sie hierzu die `AppDelegate.cs` Datei außer Kraft, indem die `ApplicationShouldTerminate` Methode, und stellen sie wie folgt aussehen:

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

## <a name="working-with-multiple-windows"></a>Arbeiten mit mehreren Fenstern

Die meisten basierendes Dokument Macintosh-Anwendungen können mehrere Dokumente gleichzeitig bearbeiten. Beispielsweise kann ein Text-Editor mehrere Textdateien, die zur Bearbeitung zur gleichen Zeit geöffnet haben. Unsere neue Xamarin.Mac-Anwendung verfügt standardmäßig eine **Datei** Menü mit einer **neu** Element automatisch wired-bis zu der `newDocument:` **Aktion**.

Kegel zum Aktivieren dieses neuen Elements, und ermöglicht dem Benutzer mehrere Kopien von unseren Hauptfenster mehrere Dokumente auf einmal bearbeiten zu öffnen.

Ermöglicht das Bearbeiten unsere `AppDelegate.cs` Datei, und fügen Sie die folgenden berechneten Eigenschaft hinzu:

```csharp
public int UntitledWindowCount { get; set;} =1;
```

Wir verwenden dies, um die Anzahl der nicht gespeicherte Dateien nachverfolgen, sodass wir Feedback, für den Benutzer (pro Apple Richtlinien erhalten können wie oben erläutert).

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

Dieser Code erstellt eine neue Version des unserer Fenster Controller, lädt das neue Fenster der Haupt- und der Schlüssel Fenster vereinfacht und Titel wird. Wenn wir unsere Anwendung auszuführen, und wählen Sie nun **neu** aus der **Datei** Menü ein neues Editorfenster geöffnet und angezeigt werden:

[![](window-images/display04.png "Es wurde ein neues unbenanntes Fenster hinzugefügt.")](window-images/display04.png#lightbox)

Wenn wir öffnen die **Windows** Menü, sehen Sie die Anwendung automatisch nachverfolgen und Behandeln von unseren geöffneten Fenstern:

[![](window-images/display05.png "Klicken Sie im Windows-Menü")](window-images/display05.png#lightbox)

Weitere Informationen zum Arbeiten mit Menüs in einer Anwendung Xamarin.Mac finden Sie unter unsere [arbeiten mit Menüs](~/mac/user-interface/menu.md) Dokumentation.

<a name="Getting_the_Currently_Active_Window" />

### <a name="getting-the-currently-active-window"></a>Das aktuell aktive Fenster abrufen

In einer Xamarin.Mac-Anwendung, die mehrere Fenster (Dokumente) öffnen können, gibt es gelegentlich Sie die aktuelle, oberste Fenster (der Schlüssel) zu erhalten müssen. Der folgende Code gibt die wichtigsten Fenster zurück:

```csharp
var window = NSApplication.SharedApplication.KeyWindow;
```

Es kann in jeder Klasse oder Methode, die auf den aktuellen Schlüssel Fenster zugreifen muss aufgerufen werden. Wenn kein Fenster geöffnet ist, gibt es zurück `null`.

<a name="Accessing-All-App-Windows" />

### <a name="accessing-all-app-windows"></a>Zugriff auf alle App-Fenster

Es gibt möglicherweise Zeiten, in denen Sie müssen auf allen Fenstern, Ihre app Xamarin.Mac aktuell geöffnet hat. Beispielsweise ist um festzustellen, wenn eine Datei, die der Benutzer möchte Öffnen bereits in ein bestehendes Fenster zu öffnen.

Die `NSApplication.SharedApplication` verwaltet eine `Windows` Eigenschaft, die ein Array mit allen geöffneten Fenstern in der app enthält. Sie können dieses Array den Zugriff auf alle aktuellen Windows die app durchlaufen. Zum Beispiel:

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

Im Beispielcode werden wir jedes zurückgegebene Fenster der benutzerdefinierten Umwandlung `ViewController` in dieser app und der Test des Werts einer benutzerdefinierten Klasse `Path` Eigenschaft für den Pfad einer Datei, die der Benutzer möchte öffnen. Wenn die Datei bereits geöffnet ist, werden wir dieses Fenster in den Vordergrund bringen.

<a name="Adjusting_the_Window_Size_in_Code" />

## <a name="adjusting-the-window-size-in-code"></a>Anpassen der Größe des Fensters im Code

Es gibt Situationen, wenn die Anwendung benötigt, um die Größe eines Fensters im Code. Um die Größe und Position eines Fensters, passen Sie es der `Frame` Eigenschaft. Beim Anpassen der Größe des Fensters müssen Sie in der Regel auch dessen Ursprung, damit das Fenster am gleichen Speicherort aufgrund des MacOS Koordinatensystem bleibt anpassen.

Im Gegensatz zu iOS, wobei die linke obere Ecke (0,0) darstellt, wird MacOS eine mathematische Koordinatensystem verwendet, in der unteren linken Ecke des Bildschirms (0,0) darstellt. In iOS-erhöhen Sie die Koordinaten, wie Sie auf der rechten Seite nach unten zu verschieben. In MacOS erhöhen die Koordinaten in Wert nach oben rechts aus. 

Im folgenden Codebeispiel wird ein Fenster:

```csharp
nfloat y = 0;

// Calculate new origin
y = Frame.Y - (768 - Frame.Height);

// Resize and position window
CGRect frame = new CGRect (Frame.X, y, 1024, 768);
SetFrame (frame, true);
```

> [!IMPORTANT]
> Wenn Sie eine Windows-Größe und Position im Code anpassen, müssen Sie sicherstellen, dass Sie die minimale und maximale Größe berücksichtigen, die Sie in der Benutzeroberflächen-Generator festgelegt haben. Dies wird nicht automatisch berücksichtigt und Sie Fenster größer oder kleiner als diese Grenzwerte vornehmen.

<a name="Monitoring-Window-Size-Changes" />

## <a name="monitoring-window-size-changes"></a>Überwachen die Fenstergröße ändert

Es gibt möglicherweise Zeiten, in dem Sie Änderungen an der ein Fenster Größe innerhalb Ihrer app Xamarin.Mac überwachen müssen. Geben Sie beispielsweise Folgendes ein, um den Inhalt auf die neue Größe neu gezeichnet werden.

Informationen zum Überwachen der Fenstergröße ändert zunächst sicherstellen Sie, dass Sie eine benutzerdefinierte Klasse für den Controller Fenster in Xcodes Benutzeroberflächen-Generator zugewiesen haben. Z. B. `MasterWindowController` in der folgenden:

[![](window-images/resize01.png "Der Identity-Inspektor")](window-images/resize01.png#lightbox)

Als Nächstes Bearbeiten des benutzerdefinierten Fenster Controllerklasse und zum Überwachen der `DidResize` Ereignis auf der Controller Fenster live größenveränderung benachrichtigt zu werden. Zum Beispiel:

```csharp
public override void WindowDidLoad ()
{
    base.WindowDidLoad ();

    Window.DidResize += (sender, e) => {
        // Do something as the window is being live resized
    };
}
```

Optional können Sie die `DidEndLiveResize` Ereignis nur benachrichtigt werden, nachdem der Benutzer wurde die Größe des Fensters ändern. Beispiel:

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

## <a name="setting-a-windows-title-and-represented-file"></a>Ein Fenster Festlegen des Titels und dargestellten Datei

Bei der Arbeit mit Windows, die Dokumente darstellen `NSWindow` verfügt über eine `DocumentEdited` , auch wenn-Eigenschaftensatz auf `true` zeigt einen kleineren Punkt in der Schaltfläche "Schließen", um dem Benutzer ein Hinweis darauf, dass die Datei geändert wurde, und vor dem Schließen gespeichert werden soll.

Ermöglicht das Bearbeiten unsere `ViewController.cs` Datei, und nehmen Sie die folgenden Änderungen:

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

Wir sind auch Überwachung der `WillClose` Ereignis in das Fenster und Überprüfung des Status der `DocumentEdited` Eigenschaft. Ist er `true` muss dem Benutzer Zugriff auf die Änderungen zu speichern, um die Datei zu gewähren. Wenn wir unsere app ausführen und Text eingeben, wird der Punkt angezeigt:

[![](window-images/file01.png "Eine geänderte Fenster")](window-images/file01.png#lightbox)

Wenn Sie versuchen, das Fenster schließen, erhalten wir eine Warnung aus:

[![](window-images/file02.png "Anzeigen eines Speichervorgangs Dialogfeld")](window-images/file02.png#lightbox)

Wenn wir ein Dokument aus einer Datei laden können wir den Titel des Fensters festlegen, auf der Datei mithilfe der `window.SetTitleWithRepresentedFilename (Path.GetFileName(path));` Methode (davon ausgehend, dass `path` ist eine Zeichenfolge, die zu öffnende Datei darstellt). Wir können darüber hinaus legen Sie die URL der Datei unter Verwendung der `window.RepresentedUrl = url;` Methode.

Wenn die URL in einen Dateityp vom Betriebssystem bekannt verweist, wird dessen Symbol in der Titelleiste angezeigt. Wenn der Benutzer mit der rechten Maustaste auf das Symbol klickt, wird der Pfad zur Datei angezeigt werden.

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

Wählen Sie jetzt beim Ausführen dieser app, **öffnen...**  aus der **Datei** klicken Sie im Menü eine Textdatei aus der **öffnen** Dialogfeld Feld, und öffnen Sie ihn:

[![](window-images/file03.png "Ein geöffnetes Dialogfenster")](window-images/file03.png#lightbox)

Die Datei wird angezeigt, und der Titel wird durch das Symbol der Datei festgelegt werden:

[![](window-images/file04.png "Der Inhalt einer Datei geladen.")](window-images/file04.png#lightbox)

<a name="Adding_a_New_Window_to_a_Project" />

## <a name="adding-a-new-window-to-a-project"></a>Hinzufügen eines neuen Fensters zu einem Projekt

Abgesehen von der main-Dokumentfenster müssen eine Xamarin.Mac-Anwendung möglicherweise andere Arten von Fenstern, die der Benutzer, z. B. Voreinstellungen oder Inspektor-Fenstern angezeigt.

Um ein neues Fenster hinzuzufügen, führen Sie folgende Schritte aus:

1. In der **Projektmappen-Explorer**, doppelklicken Sie auf die `Main.storyboard` Datei, um ihn zur Bearbeitung in Xcodes Benutzeroberflächen-Generator zu öffnen.
2. Ziehen Sie ein neues **Fenster Controller** aus der **Bibliothek** und legen sie auf der **Entwurfsoberfläche**:

    [![](window-images/new01.png "Auswählen von einem neuen Fenster Controller in der Bibliothek")](window-images/new01.png#lightbox)
3. In der **Identität Inspektor**, geben Sie `PreferencesWindow` für die **Storyboard-ID**: 

    [![](window-images/new02.png "Festlegen der Storyboard-ID")](window-images/new02.png#lightbox)
5. Entwerfen Sie Ihre Schnittstelle: 

    [![](window-images/new03.png "Entwerfen der Benutzeroberflächenautomatisierungs")](window-images/new03.png#lightbox)
6. Öffnen Sie das App-Menü (`MacWindows`), wählen **Einstellungen...** , Steuerelement klicken und ziehen, um das neue Fenster: 

    [![](window-images/new05.png "Erstellen eine segue")](window-images/new05.png#lightbox)
7. Wählen Sie **anzeigen** im Popupmenü.
6. Die Änderungen zu speichern und zurück zu Visual Studio für Mac mit Xcode zu synchronisieren.

Wenn wir führen Sie den Code, und wählen Sie die **Einstellungen...**  aus der **Anwendungsmenü**, das Fenster wird angezeigt:

[![](window-images/new04.png "Eine Beispiel-Menü "Einstellungen"")](window-images/new04.png#lightbox)

<a name="Working_with_Panels" />

## <a name="working-with-panels"></a>Arbeiten mit Bereichen

Wie zu Beginn dieses Artikels erwähnt, wird ein Panel gleitet über anderen Fenstern und bietet Tools oder Steuerelemente, denen Benutzer arbeiten können, wenn Dokumente geöffnet sind. 

Genau wie jeder andere Typ von Fenster, das Sie erstellen und in der Anwendung Xamarin.Mac verwenden, ist der Prozess im Grunde identisch:

1. Fügen Sie eine neue Fensterdefinition zum Projekt hinzu.
2. Doppelklicken Sie auf die `.xib` Datei, um den Entwurf der Fenster zur Bearbeitung in Xcodes Benutzeroberflächen-Generator zu öffnen.
2. Legen Sie alle erforderlichen Eigenschaften der **Attribut Inspektor** und **Größe Inspektor**.
4. Ziehen Sie in den Steuerelementen, die erforderlich sind, damit Ihre Schnittstelle erstellen und konfigurieren sie in der **Attribut Inspektor**.
5. Verwenden der **Größe Inspektor** behandelt, die größenanpassung von UI-Elementen.
6. Verfügbar machen die Elemente der Benutzeroberfläche des Fensters für C#-Code über **Steckdosen** und **Aktionen**.
7. Speichern Sie die Änderungen zu, und wechseln Sie zurück zu Visual Studio für Mac mit Xcode zu synchronisieren.

In der **Attribut Inspektor**, Sie haben die folgenden Optionen, die spezifisch für Bereiche:

[![](window-images/panel03.png "Die Attribut-Inspektor")](window-images/panel03.png#lightbox)

- **Stil** -können Sie das Format des Bereichs von anpassen: reguläre Bereich (anscheinend Standardfensters), Utility Bereich (hat eine kleinere Titelleiste), HUD-Bereich (transparent ist und die Titelleiste ist Bestandteil des Hintergrunds).
- **Nicht aktiviert** -bestimmt in das Panel wird das Fenster.
- **Dokumentieren Sie modale** -wenn Dokument modales, im Bereich wird nur float oberhalb der Anwendung Windows, ansonsten gleitet es oberhalb aller.


Um einen neuen Bereich hinzuzufügen, führen Sie folgende Schritte aus:

1. In der **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und wählen Sie **hinzufügen** > **neue Datei...** .
2. Wählen Sie im Dialogfeld "neue Datei" **Xamarin.Mac** > **Kakao-Fenster mit Controller**:

    [![](window-images/panels00.png "Hinzufügen eines neuen Fensters-Controllers")](window-images/panels00.png#lightbox)
3. Geben Sie für den **Namen** `DocumentPanel` ein, und klicken Sie auf **Neu**.
4. Doppelklicken Sie auf die `DocumentPanel.xib` Datei, um ihn zur Bearbeitung in der Benutzeroberflächen-Generator zu öffnen: 

    [![](window-images/new02.png "Bearbeiten die pannel")](window-images/new02.png#lightbox)
5. Löschen Sie das vorhandene Fenster, und ziehen Sie ein Panel aus der **Bibliothek Inspektor** in der die **Benutzeroberflächen-Editors**: 

    [![](window-images/panels01.png "Löschen von vorhandenen Fenster")](window-images/panels01.png#lightbox)
6. Verknüpfen Sie den Bereich bis zu der **des Dateibesitzer*-**Fenster*- **Nachrichtenplattform**: 

    [![](window-images/panels02.png "Ziehen Weise wird das panel")](window-images/panels02.png#lightbox)
7. Wechseln Sie zu der **Identität Inspektor** und legen Sie im Bereich-Klasse den `DocumentPanel`: 

    [![](window-images/panels03.png "Festlegen des Bereichs-Klasse")](window-images/panels03.png#lightbox)
6. Die Änderungen zu speichern und zurück zu Visual Studio für Mac mit Xcode zu synchronisieren.
7. Bearbeiten der `DocumentPanel.cs` Datei, und ändern Sie die Klassendefinition in Folgendes: 

    `public partial class DocumentPanel : NSPanel`
8. Speichern Sie die Änderungen in der Datei.

Bearbeiten der `AppDelegate.cs` Datei, und stellen die `DidFinishLaunching` Methode aussehen wie folgt:

```csharp
public override void DidFinishLaunching (NSNotification notification)
{
        // Display panel
    var panel = new DocumentPanelController ();
    panel.Window.MakeKeyAndOrderFront (this);
}

```

Wenn wir unsere Anwendung ausführen, wird das Panel angezeigt:

[![](window-images/panels04.png "Der Bereich in einer ausgeführten app")](window-images/panels04.png#lightbox)

> [!IMPORTANT]
> Bereich Windows von Apple veraltet und sollten ersetzt werden, mit **Inspektor Schnittstellen**. Ein vollständiges Beispiel zum Erstellen einer **Inspektor** in einer app Xamarin.Mac finden Sie unter unsere [MacInspector](https://developer.xamarin.com/samples/mac/MacInspector/) Beispiel-app.

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

Dieser Artikel hat eine ausführliche Übersicht über das Arbeiten mit Windows- und Bereiche in einer Anwendung Xamarin.Mac übernommen. Wir gesehen haben die verschiedenen Typen und von Windows und Bereiche, zum Erstellen und Verwalten von Windows und Bereiche in Xcodes Benutzeroberflächen-Generator und zum Arbeiten mit Windows- und Bereiche in C#-Code verwendet.

## <a name="related-links"></a>Verwandte Links

- [MacWindows (Beispiel)](https://developer.xamarin.com/samples/mac/MacWindows/)
- [MacInspector (Beispiel)](https://developer.xamarin.com/samples/mac/MacInspector/)
- [Hello, Mac (Hallo, Mac)](~/mac/get-started/hello-mac.md)
- [Arbeiten mit Menüs](~/mac/user-interface/menu.md)
- [Eingaberichtlinien für OS X](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Einführung in Windows](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
