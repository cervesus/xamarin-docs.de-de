---
title: Fenster in xamarin. Mac
description: In diesem Artikel wird das Arbeiten mit Fenstern und Bereichen in einer xamarin. Mac-Anwendung behandelt. Es beschreibt das Erstellen von Fenstern und Bereichen in Xcode und Interface Builder, das Laden von Fenstern aus Storyboards und XIb-Dateien und die programmgesteuerte Arbeit mit diesen.
ms.prod: xamarin
ms.assetid: 4F6C67E9-BBFF-44F7-B29E-AB47D7F44287
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 03/14/2017
ms.openlocfilehash: c3355b2696b5c40ee11a289e700239900698dcf1
ms.sourcegitcommit: 55167ad9db910c5c0eb5a84c0923cb07acd2530e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/02/2020
ms.locfileid: "75606709"
---
# <a name="windows-in-xamarinmac"></a>Fenster in xamarin. Mac

_In diesem Artikel wird das Arbeiten mit Fenstern und Bereichen in einer xamarin. Mac-Anwendung behandelt. Es beschreibt das Erstellen von Fenstern und Bereichen in Xcode und Interface Builder, das Laden von Fenstern aus Storyboards und XIb-Dateien und die programmgesteuerte Arbeit mit diesen._

Wenn Sie mit C# und .net in einer xamarin. Mac-Anwendung arbeiten, haben Sie Zugriff auf dieselben Fenster und Panels wie ein Entwickler, der in *Ziel-C* und *Xcode* arbeitet. Da xamarin. Mac direkt in Xcode integriert ist, können Sie die _Interface Builder_ von Xcode verwenden, um Windows und Panels zu erstellen und zu verwalten (oder Sie C# optional direkt im Code zu erstellen).

Basierend auf dem Zweck kann eine xamarin. Mac-Anwendung ein oder mehrere Fenster auf dem Bildschirm darstellen, um die angezeigten Informationen zu verwalten und zu koordinieren. Die Hauptfunktionen eines Fensters sind:

1. Um einen Bereich bereitzustellen, in dem Sichten und Steuerelemente platziert und verwaltet werden können.
2. Akzeptieren und reagieren auf Ereignisse als Reaktion auf Benutzerinteraktion mit Tastatur und Maus.

Windows kann in einem nicht modalen Zustand (z. b. in einem Text-Editor, in dem mehrere Dokumente gleichzeitig geöffnet werden können) oder Modal verwendet werden (z. b. ein Export Dialogfeld, das vor dem Fortsetzen der Anwendung verworfen werden muss)

Panels sind eine besondere Art von Fenster (eine Unterklasse der Basis `NSWindow`-Klasse), die in der Regel eine Zusatzfunktion in einer Anwendung bereitstellt, z. b. Hilfsprogrammfenster wie Text Format Inspektoren und System Farbauswahl.

[![Bearbeiten eines Fensters in Xcode](window-images/intro01.png)](window-images/intro01.png#lightbox)

In diesem Artikel werden die Grundlagen der Arbeit mit Fenstern und Bereichen in einer xamarin. Mac-Anwendung behandelt. Es wird dringend empfohlen, dass Sie zunächst den Artikel [Hello, Mac](~/mac/get-started/hello-mac.md) , insbesondere die [Einführung in Xcode und die Abschnitte zu Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) und Outlets und [Aktionen](~/mac/get-started/hello-mac.md#outlets-and-actions) , durcharbeiten, da er wichtige Konzepte und Techniken behandelt, die wir in diesem Artikel verwenden werden.

Sie können sich auch den Abschnitt verfügbar machen von [ C# Klassen/Methoden zu "Ziel-c](~/mac/internals/how-it-works.md) " im Dokument " [xamarin. Mac](~/mac/internals/how-it-works.md) " ansehen. darin werden die `Register`-und `Export` Befehle erläutert, die verwendet werden, um C# Ihre Klassen mit Ziel-c-Objekten und UI-Elementen zu verknüpfen.

## <a name="introduction-to-windows"></a>Einführung in Windows

Wie bereits erwähnt, stellt ein Fenster einen Bereich bereit, in dem Sichten und Steuerelemente platziert und verwaltet werden können, und antwortet auf Ereignisse, die auf der Benutzerinteraktion basieren (entweder über Tastatur oder Maus).

Gemäß Apple gibt es fünf Haupttypen von Windows in einer macOS-App:

- **Dokument Fenster** : ein Dokument Fenster enthält dateibasierte Benutzerdaten, z. b. eine Kalkulations Tabelle oder ein Textdokument.
- **App-Fenster** : ein App-Fenster ist das Hauptfenster einer Anwendung, die nicht Dokument basiert ist (z. b. die Kalender-APP auf einem Mac).
- Bereich **: ein Panel schwebt** oberhalb von anderen Fenstern und stellt Tools oder Steuerelemente bereit, mit denen Benutzer arbeiten können, während Dokumente geöffnet sind. In einigen Fällen kann ein Bereich durchsichtig sein (z. b. bei der Arbeit mit großen Grafiken).
- **Dialog** : ein Dialogfeld wird als Reaktion auf eine Benutzeraktion angezeigt und bietet in der Regel Möglichkeiten, wie Benutzer die Aktion durchführen können. Ein Dialogfeld erfordert eine Antwort vom Benutzer, bevor es geschlossen werden kann. (Siehe [Arbeiten mit Dialog](~/mac/user-interface/dialog.md)Feldern)
- **Warnungen** : eine Warnung ist eine besondere Art von Dialogfeld, das angezeigt wird, wenn ein schwerwiegendes Problem auftritt (z. b. ein Fehler) oder eine Warnung (z. b. das Löschen einer Datei). Da es sich bei einer Warnung um ein Dialogfeld handelt, ist auch eine Benutzer Antwort erforderlich, bevor Sie geschlossen werden kann. (Siehe [Arbeiten mit Warnungen](~/mac/user-interface/alert.md))

Weitere Informationen finden Sie im Abschnitt [about Windows](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowAppearanceBehavior.html#//apple_ref/doc/uid/20000957-CH33-SW1) des [macOS-Entwurfs Designs](https://developer.apple.com/design/human-interface-guidelines/macos/overview/themes/)von Apple.

### <a name="main-key-and-inactive-windows"></a>Haupt-, Schlüssel-und inaktive Fenster

Windows in einer xamarin. Mac-Anwendung kann abhängig davon, wie der Benutzer derzeit mit Ihnen interagiert, anders aussehen und sich Verhalten. Das wichtigste Dokument oder App-Fenster, das sich derzeit auf die Aufmerksamkeit des Benutzers konzentriert, wird als _Hauptfenster_bezeichnet. In den meisten Fällen handelt es sich bei diesem Fenster ebenfalls um das _Schlüssel Fenster_ (das Fenster, das derzeit Benutzereingaben annimmt). Dies ist jedoch nicht immer der Fall, z. b. kann eine Farbauswahl geöffnet werden, und es handelt sich um das Schlüssel Fenster, mit dem der Benutzer interagiert, um den Zustand eines Elements im Dokument Fenster zu ändern (was immer noch das Hauptfenster ist).

Die Haupt-und Schlüssel Fenster (wenn Sie getrennt sind) sind immer aktiv. _inaktive Fenster_ sind geöffnete Fenster, die nicht im Vordergrund stehen. Beispielsweise könnte eine Text-Editor-Anwendung mehr als ein Dokument gleichzeitig geöffnet haben, nur das Hauptfenster ist aktiv, alle anderen wären inaktiv. 

Weitere Informationen finden Sie im Abschnitt [about Windows](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowAppearanceBehavior.html#//apple_ref/doc/uid/20000957-CH33-SW1) des [macOS-Entwurfs Designs](https://developer.apple.com/design/human-interface-guidelines/macos/overview/themes/)von Apple.

### <a name="naming-windows"></a>Benennen von Fenstern

Ein Fenster kann eine Titelleiste anzeigen, und wenn der Titel angezeigt wird, ist es in der Regel der Name der Anwendung, der Name des Dokuments, an dem gearbeitet wird, oder die Funktion des Fensters (z. b. Inspektor). Einige Anwendungen zeigen keine Titelleiste an, da Sie von Sight erkannt werden und nicht mit Dokumenten arbeiten können.

Apple empfiehlt die folgenden Richtlinien:

- Verwenden Sie den Anwendungsnamen für den Titel eines Hauptfensters, das kein Dokument ist. 
- Benennen Sie ein neues Dokument Fenster `untitled`. Fügen Sie für das erste neue Dokument keine Zahl an den Titel an (z. b. `untitled 1`). Wenn der Benutzer ein neues Dokument erstellt, bevor es gespeichert wird, und das erste speichert, rufen Sie dieses Fenster `untitled 2`, `untitled 3`usw.

Weitere Informationen finden Sie im Abschnitt [Benennen von Windows](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowNaming.html#//apple_ref/doc/uid/20000957-CH35-SW1) in den [macOS-Designthemen](https://developer.apple.com/design/human-interface-guidelines/macos/overview/themes/)von Apple.

### <a name="full-screen-windows"></a>Voll Bildfenster

In macOS kann das Fenster der Anwendung den Vollbildmodus für alle Elemente ausblenden, einschließlich der Menüleiste der Anwendung (die angezeigt werden kann, indem der Cursor an den oberen Bildschirmrand verschoben wird), um eine ablenkungsfreie Interaktion mit dem Inhalt der Inhalte bereitzustellen.

Apple schlägt die folgenden Richtlinien vor:

- Legen Sie fest, ob es sinnvoll ist, dass ein Fenster in den Vollbildmodus wechselt. Anwendungen, die kurze Interaktionen (z. b. einen Rechner) bereitstellen, sollten keinen Vollbildmodus bereitstellen.
- Zeigen Sie die Symbolleiste an, wenn Sie von der voll Bild Aufgabe benötigt wird. In der Regel wird die Symbolleiste im Vollbildmodus ausgeblendet.
- Das voll Bildfenster muss alle Features enthalten, die Benutzer zum Ausführen der Aufgabe benötigen.
- Vermeiden Sie, wenn möglich, die Suche nach Findern, während sich der Benutzer in einem voll Bildfenster befindet.
- Profitieren Sie von den erweiterten bildschirmflächen, ohne den Fokus von der Hauptaufgabe zu verlagern.

Weitere Informationen finden Sie im Abschnitt zum [Windows-voll Bild](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/FullScreen.html#//apple_ref/doc/uid/20000957-CH61-SW1) Abschnitt der [macOS-Designthemen](https://developer.apple.com/design/human-interface-guidelines/macos/overview/themes/)von Apple.

### <a name="panels"></a>Panels

Ein Panel ist ein Hilfsfenster, das Steuerelemente und Optionen enthält, die sich auf das aktive Dokument oder die aktive Auswahl (z. b. die System Farbauswahl) auswirken:

[![eines Farb Panels](window-images/panel01.png)](window-images/panel01.png#lightbox)

Panels können _App-spezifisch_ oder _systemweit_sein. App-spezifische Panels schweben über den oberen Rand der Dokument Fenster der Anwendung und verschwinden, wenn sich die Anwendung im Hintergrund befindet. Systemweite Bereiche (z. b. das **Schriftarten** Panel) werden über alle geöffneten Fenster unabhängig von der Anwendung schwebt. 

Apple schlägt die folgenden Richtlinien vor:

- Im Allgemeinen sollten Sie einen Standardbereich verwenden, transparente Panels sollten nur sparsam und für grafisch intensive Aufgaben verwendet werden.
- Verwenden Sie einen Bereich, um Benutzern den einfachen Zugriff auf wichtige Steuerelemente oder Informationen zu gestatten, die sich direkt auf Ihre Aufgabe auswirken.
- Ausblenden und Anzeigen von Panels nach Bedarf.
- Panels sollten immer die Titelleiste einschließen.
- Panels sollten keine aktive Minimieren-Schaltfläche enthalten.

#### <a name="inspectors"></a>Inspectors

Die meisten modernen macOS-Anwendungen bieten zusätzliche Steuerelemente und Optionen, die sich auf das aktive Dokument oder die Auswahl als _Inspektoren_ auswirken, die Teil des Hauptfensters sind (wie die unten gezeigte **pages** -APP), anstatt Fenster Fenster zu verwenden:

[![eines Beispiel Inspektors](window-images/panel02.png)](window-images/panel02.png#lightbox)

Weitere Informationen finden Sie im Abschnitt [Panels](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowPanels.html#//apple_ref/doc/uid/20000957-CH42-SW1) der [macOS-Entwurfs Designs](https://developer.apple.com/design/human-interface-guidelines/macos/overview/themes/) von Apple und in der Beispiel-App für [macinspector](https://docs.microsoft.com/samples/xamarin/mac-samples/macinspector) , um eine vollständige Implementierung einer **inspektorschnittstelle** in einer xamarin. Mac-app zu erhalten.

## <a name="creating-and-maintaining-windows-in-xcode"></a>Erstellen und Verwalten von Fenstern in Xcode

Wenn Sie eine neue xamarin. Mac-Cocoa-Anwendung erstellen, erhalten Sie standardmäßig ein Standardmäßiges leeres Standardfenster. Dieses Fenster wird in einer `.storyboard` Datei definiert, die automatisch im Projekt enthalten ist. Um den Windows-Entwurf zu bearbeiten, doppelklicken Sie im **Projektmappen-Explorer**auf die `Main.storyboard` Datei:

[![auswählen des Haupt Storyboards](window-images/edit01.png)](window-images/edit01.png#lightbox)

Dadurch wird das Fensterdesign in der Interface Builder von Xcode geöffnet:

[![Bearbeiten der Benutzeroberfläche in Xcode](window-images/edit02.png)](window-images/edit02.png#lightbox)

Im **Attribut Inspektor**gibt es mehrere Eigenschaften, die Sie verwenden können, um das Fenster zu definieren und zu steuern:

- **Title** : Dies ist der Text, der in der Titlebar des Fensters angezeigt wird.
- **Autosave** : Dies ist der _Schlüssel_ , der verwendet wird, um das Fenster zu ID zu verwenden, wenn seine Position und Einstellungen automatisch gespeichert werden.
- **Titelleiste** : bewirkt, dass im Fenster eine Titelleiste angezeigt wird.
- **Einheitlicher Titel und Symbolleiste** : Wenn das Fenster eine Symbolleiste enthält, sollte es Teil der Titelleiste sein.
- **Inhaltsansicht mit voller Größen** Anpassung: der Inhalts Bereich des Fensters kann in der Titelleiste angezeigt werden.
- **Shadow** : das Fenster hat einen Schatten.
- In **texturierten** Fenstern können Effekte (wie vibrancy) verwendet werden, die durchziehen an eine beliebige Stelle in Ihren Text verschoben werden können.
- **Schließen** : das Fenster weist eine Schaltfläche zum Schließen auf.
- **Minimieren** : bewirkt, dass das Fenster die Schaltfläche Minimieren hat.
- **Größe** : bewirkt, dass das Fenster über ein Steuerelement zur Größenänderung verfügt.
- **Symbolleisten Schaltfläche** : führt das Fenster zum Ausblenden/Anzeigen der Symbolleisten Schaltfläche.
- Wieder **herstellbar** : die Position und Einstellungen des Fensters werden automatisch gespeichert und wieder hergestellt.
- **Sichtbar beim Start** : das Fenster wird automatisch angezeigt, wenn die `.xib`-Datei geladen wird.
- **Beim Deaktivieren ausblenden** : das Fenster wird ausgeblendet, wenn die Anwendung in den Hintergrund wechselt.
- **Release, wenn geschlossen** : das Fenster wird aus dem Arbeitsspeicher gelöscht, wenn es geschlossen wird.
- Quick Infos **immer anzeigen** : die Quick Infos werden ständig angezeigt.
- **Neuberechnen der Ansichts Schleife** : die Ansichts Reihenfolge wird neu berechnet, bevor das Fenster gezeichnet wird.
- **Spaces**, **Exposé** und **Cycling** -all definieren, wie sich das Fenster in diesen macOS-Umgebungen verhält. 
- **Vollbild** : legt fest, ob dieses Fenster in den Vollbildmodus wechseln kann. 
- **Animation** : steuert den Typ der für das Fenster verfügbaren Animation.
- Darstellung **: steuert** die Darstellung des Fensters. Für den Moment gibt es nur eine Darstellung, Aqua.

Weitere Informationen finden [Sie in der Dokumentation zu Windows](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1) und [nswindow](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSWindow_Class/index.html#//apple_ref/occ/cl/NSWindow) in der Apple-Dokumentation.

### <a name="setting-the-default-size-and-location"></a>Festlegen der Standardgröße und der Standardposition

Wechseln Sie zur **Größen**Prüfung, um die anfängliche Position des Fensters festzulegen und seine Größe zu steuern:

[![Standardgröße und-Speicherort](window-images/edit07.png)](window-images/edit07.png#lightbox)

Von hier aus können Sie die anfängliche Größe des Fensters festlegen, eine minimale und maximale Größe festlegen, die ursprüngliche Position auf dem Bildschirm festlegen und die Rahmen um das Fenster steuern.

### <a name="setting-a-custom-main-window-controller"></a>Festlegen eines benutzerdefinierten Hauptfenster Controllers

Damit Outlets und Aktionen erstellt werden können, um Benutzeroberflächen Elemente für C# Code verfügbar zu machen, muss die xamarin. Mac-app einen benutzerdefinierten Fenster Controller verwenden.

Gehen Sie wie folgt vor:

1. Öffnen Sie das Storyboard der app in der Interface Builder von Xcode.
2. Wählen Sie die `NSWindowController` im Designoberfläche aus.
3. Wechseln Sie zur Ansicht " **Identitäts Inspektor** ", und geben Sie `WindowController` als **Klassennamen**ein: 

    [![Festlegen des Klassen namens](window-images/windowcontroller01.png)](window-images/windowcontroller01.png#lightbox)
4. Speichern Sie die Änderungen, und kehren Sie zur Synchronisierung Visual Studio für Mac zurück.
5. Eine `WindowController.cs`-Datei wird dem Projekt im **Projektmappen-Explorer** in Visual Studio für Mac hinzugefügt: 

    [![auswählen des Windows-Controllers](window-images/windowcontroller02.png)](window-images/windowcontroller02.png#lightbox)
6. Öffnen Sie das Storyboard erneut in der Interface Builder von Xcode.
7. Die `WindowController.h` Datei steht zur Verwendung zur Verfügung: 

    [![Bearbeiten der Datei "windowcontroller. h"](window-images/windowcontroller03.png)](window-images/windowcontroller03.png#lightbox)

### <a name="adding-ui-elements"></a>Fügen von UI-Elementen

Um den Inhalt eines Fensters zu definieren, ziehen Sie die Steuerelemente aus der **Bibliothek** Prüfung auf den **Schnittstellen-Editor**. Weitere Informationen zur Verwendung Interface Builder zum Erstellen und Aktivieren von Steuerelementen finden Sie in unserer [Einführung in Xcode und Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) Dokumentation.

Ziehen Sie als Beispiel eine Symbolleiste aus dem **Bibliotheks Inspektor** in das Fenster im Schnittstellen- **Editor**:

[![Auswählen einer Symbolleiste aus der Bibliothek](window-images/edit03.png)](window-images/edit03.png#lightbox)

Ziehen Sie als nächstes eine **Text Ansicht** , und legen Sie die Größe ab, um den Bereich unter der Symbolleiste auszufüllen:

[![Hinzufügen einer Text Ansicht](window-images/edit04.png)](window-images/edit04.png#lightbox)

Da die **Text Ansicht** verkleinert und vergrößert werden soll, wenn sich die Fenstergröße ändert, wechseln wir zum Einschränkungs- **Editor** und fügen die folgenden Einschränkungen hinzu:

[![Einschränkungen für die Bearbeitung](window-images/edit05.png)](window-images/edit05.png#lightbox)

Indem Sie auf die vier **roten I-Balken** am oberen Rand des Editors klicken und auf **4 Einschränkungen hinzufügen**klicken, wird die Textansicht an die angegebene X-und Y-Koordinate angefügt und horizontal und vertikal vergrößert oder verkleinert, wenn die Größe des Fensters geändert wird.

Stellen Sie schließlich die **Text Ansicht** mithilfe eines **Outlets** für Code bereit (stellen Sie sicher, dass Sie die `ViewController.h` Datei auswählen):

[![Konfigurieren eines Outlets](window-images/edit06.png)](window-images/edit06.png#lightbox)

Speichern Sie die Änderungen, und wechseln Sie zurück zu Visual Studio für Mac, um mit Xcode zu synchronisieren.

Weitere Informationen zum Arbeiten mit **Outlets** und **Aktionen**finden Sie in unserer Dokumentation zu [Outlet und Aktion](~/mac/get-started/hello-mac.md#outlets-and-actions) .

### <a name="standard-window-workflow"></a>Standard Fenster Workflow

Für jedes Fenster, das Sie in ihrer xamarin. Mac-Anwendung erstellen und mit Ihnen arbeiten, ist der Prozess im Grunde das gleiche wie oben beschrieben:

1. Fügen Sie für neue Fenster, die dem Projekt nicht automatisch hinzugefügt werden, eine neue Fenster Definition zum Projekt hinzu. Dies wird im folgenden ausführlich erläutert.
1. Doppelklicken Sie auf die Datei `Main.storyboard`, um den Fenster Entwurf für die Bearbeitung in der Interface Builder von Xcode zu öffnen.
1. Ziehen _Sie ein_ neues Fenster in das Design der Benutzeroberfläche, und schließen Sie das Fenster mithilfe von Listen in das Hauptfenster ein (Weitere Informationen finden Sie [im Abschnitt "](~/mac/platform/storyboards/indepth.md#Segues) Abschnitte" in unserer Dokumentation zum [Arbeiten mit Storyboards](~/mac/platform/storyboards/indepth.md) ).
1. Legen Sie alle erforderlichen Fenster Eigenschaften in der **Attribut** Prüfung und der **Größe Inspector**fest.
1. Ziehen Sie die Steuerelemente, die zum Erstellen der Schnittstelle erforderlich sind, und konfigurieren Sie Sie im **Attribut Inspektor**.
1. Verwenden Sie den **Größen Inspektor** , um die Größenänderung für Ihre Benutzeroberflächen Elemente zu verarbeiten.
1. Machen Sie die Benutzeroberflächen Elemente des C# Fensters für den Code über **Outlets** und **Aktionen**verfügbar.
1. Speichern Sie die Änderungen, und wechseln Sie zurück zu Visual Studio für Mac, um mit Xcode zu synchronisieren.

Nachdem wir nun ein einfaches Fenster erstellt haben, sehen wir uns die typischen Prozesse an, die eine xamarin. Mac-Anwendung bei der Arbeit mit Windows vornimmt. 

## <a name="displaying-the-default-window"></a>Anzeigen des Standard Fensters

Standardmäßig wird in einer neuen xamarin. Mac-Anwendung automatisch das Fenster angezeigt, das in der `MainWindow.xib`-Datei definiert ist, wenn Sie gestartet wird:

[![ein Beispiel Fenster mit](window-images/display01.png)](window-images/display01.png#lightbox)

Da wir das Design dieses Fensters oben geändert haben, enthält es nun ein Standard Symbol für Symbolleiste und **Text Ansicht** . Der folgende Abschnitt in der `Info.plist`-Datei ist für die Anzeige dieses Fensters zuständig:

[![Bearbeiten von Info. plist](window-images/display00.png)](window-images/display00.png#lightbox)

Mithilfe der Dropdown Liste **Hauptschnittstelle** können Sie das Storyboard auswählen, das als Hauptbenutzer Oberfläche der APP verwendet wird (in diesem Fall `Main.storyboard`).

Ein Ansichts Controller wird dem Projekt automatisch hinzugefügt, um die angezeigten Hauptfenster (zusammen mit der primären Ansicht) zu steuern. Sie wird in der `ViewController.cs` Datei definiert und in Interface Builder unter dem **Identitäts Inspektor**an den **Besitzer der Datei** angefügt:

[![Festlegen des Besitzers der Datei](window-images/display02.png)](window-images/display02.png#lightbox)

In unserem Fenster soll der Titel `untitled` sein, wenn er zum ersten Mal geöffnet wird. überschreiben Sie die `ViewWillAppear`-Methode im `ViewController.cs` so, dass Sie wie folgt aussieht:

```csharp
public override void ViewWillAppear ()
{
    base.ViewWillAppear ();

    // Set Window Title
    this.View.Window.Title = "untitled";
}
```    

> [!NOTE]
> Die `Title`-Eigenschaft des Fensters wird in der `ViewWillAppear`-Methode anstelle der `ViewDidLoad`-Methode festgelegt, da die Sicht möglicherweise noch nicht vollständig instanziiert ist, während die Sicht in den Arbeitsspeicher geladen wird. Beim Zugriff auf die `Title`-Eigenschaft in der `ViewDidLoad`-Methode wird eine `null`-Ausnahme ausgelöst, da das Fenster noch nicht erstellt wurde und noch nicht mit der-Eigenschaft verknüpft ist.

## <a name="programmatically-closing-a-window"></a>Programm gesteuertes Schließen eines Fensters

Es kann vorkommen, dass Sie ein Fenster in einer xamarin. Mac-Anwendung Programm gesteuert schließen möchten, außer wenn der Benutzer auf die Schaltfläche **Schließen** oder ein Menü Element klickt. macOS bietet zwei verschiedene Möglichkeiten, eine `NSWindow` Programm gesteuert zu schließen: `PerformClose` und `Close`.

### <a name="performclose"></a>Performclose

Wenn Sie die `PerformClose`-Methode einer `NSWindow` aufrufen, wird der Benutzer durch das Klicken auf die Schaltfläche **Schließen** des Fensters simuliert, indem die Schaltfläche vorübergehend hervorgehoben und das Fenster geschlossen wird.

Wenn die Anwendung das `WillClose`-Ereignis der `NSWindow`implementiert, wird Sie ausgelöst, bevor das Fenster geschlossen wird. Wenn das Ereignis `false`zurückgibt, wird das Fenster nicht geschlossen. Wenn das Fenster keine Schaltfläche " **Schließen** " hat oder aus irgendeinem Grund nicht geschlossen werden kann, gibt das Betriebssystem den Ton der Warnung aus.

Beispiel:

```csharp
MyWindow.PerformClose(this);
```

Versucht, die `MyWindow` `NSWindow`-Instanz zu schließen. Wenn der Vorgang erfolgreich war, wird das Fenster geschlossen, und der Warnungs Sound wird ausgegeben, und der wird geöffnet.

### <a name="close"></a>Schließen

Wenn Sie die `Close`-Methode einer `NSWindow` aufrufen, wird der Benutzer nicht simuliert, indem er auf die Schließ Schaltfläche des Fensters klickt, indem er die Schaltfläche **kurz** hervorhebt. er schließt das Fenster einfach.

Ein Fenster muss nicht sichtbar sein, um geschlossen zu werden, und es wird eine `NSWindowWillCloseNotification` Benachrichtigung an das Standard Benachrichtigungs Center gesendet, um das Fenster zu schließen.

Die `Close`-Methode unterscheidet sich in zwei wichtigen Punkten von der `PerformClose`-Methode:

1. Es wird nicht versucht, das `WillClose`-Ereignis zu erhöhen.
2. Er simuliert den Benutzer nicht, indem er auf die Schaltfläche **Schließen** klickt, indem er die Schaltfläche vorübergehend hervorhebt.

Beispiel:

```csharp
MyWindow.Close();
```

Soll die `MyWindow` `NSWindow`-Instanz schließen.

## <a name="modified-windows-content"></a>Geänderte Windows-Inhalte

In macOS hat Apple eine Möglichkeit bereitgestellt, den Benutzer darüber zu informieren, dass der Inhalt eines Fensters (`NSWindow`) vom Benutzer geändert und gespeichert werden muss. Wenn das Fenster geänderten Inhalt enthält, wird ein kleiner schwarzer Punkt in seinem **Close** -Widget angezeigt:

[![eines Fensters mit dem geänderten Marker](window-images/close01.png)](window-images/close01.png#lightbox)

Wenn der Benutzer versucht, das Fenster zu schließen oder die Mac-app zu beenden, während nicht gespeicherte Änderungen am Inhalt des Fensters vorhanden sind, sollten Sie ein [Dialog Feld](~/mac/user-interface/dialog.md) oder ein [modales Blatt](~/mac/user-interface/dialog.md) darstellen und dem Benutzer die Möglichkeit geben, die Änderungen zuerst zu speichern:

[![ein Speicher Blatt, das angezeigt wird, wenn das Fenster geschlossen wird.](window-images/close02.png)](window-images/close02.png#lightbox)

### <a name="marking-a-window-as-modified"></a>Markieren eines Fensters als geändert

Verwenden Sie den folgenden Code, um ein Fenster als geänderten Inhalt zu markieren:

```csharp
// Mark Window content as modified
Window.DocumentEdited = true;
```

Nachdem die Änderung gespeichert wurde, löschen Sie das geänderte Flag mithilfe von:

```csharp
// Mark Window content as not modified
Window.DocumentEdited = false;
```

### <a name="saving-changes-before-closing-a-window"></a>Speichern von Änderungen vor dem Schließen eines Fensters

Wenn Sie überwachen möchten, dass der Benutzer ein Fenster schließt und es Ihnen ermöglicht, geänderte Inhalte vorher zu speichern, müssen Sie eine Unterklasse von `NSWindowDelegate` erstellen und dessen `WindowShouldClose`-Methode überschreiben. Beispiel:

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

### <a name="saving-changes-before-closing-the-app"></a>Speichern von Änderungen vor dem Schließen der APP

Schließlich sollte Ihre xamarin. Mac-app überprüfen, ob eines ihrer Fenster geänderte Inhalte enthält, und dass der Benutzer die Änderungen vor dem Beenden speichern kann. Bearbeiten Sie hierzu die `AppDelegate.cs` Datei, überschreiben Sie die `ApplicationShouldTerminate`-Methode, und führen Sie Sie wie folgt aus:

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

## <a name="working-with-multiple-windows"></a>Arbeiten mit mehreren Fenstern

Die meisten Dokument basierten Macintosh-Anwendungen können mehrere Dokumente gleichzeitig bearbeiten. Ein Text-Editor kann z. b. mehrere Textdateien gleichzeitig zum Bearbeiten geöffnet haben. Standardmäßig verfügt eine neue xamarin. Mac-Anwendung über ein Menü **Datei** mit einem **neuen** Element, das automatisch mit der `newDocument:` **Aktion**verknüpft ist.

Der folgende Code aktiviert dieses neue Element und ermöglicht es dem Benutzer, mehrere Kopien des Hauptfensters zu öffnen, um mehrere Dokumente gleichzeitig zu bearbeiten.

Bearbeiten Sie die Datei `AppDelegate.cs`, und fügen Sie die folgende berechnete Eigenschaft hinzu:

```csharp
public int UntitledWindowCount { get; set;} =1;
```

Verwenden Sie dieses Verfahren, um die Anzahl der nicht gespeicherten Dateien zu verfolgen, damit wir dem Benutzer Feedback geben können (gemäß den oben beschriebenen Richtlinien von Apple).

Fügen Sie als nächstes die folgende Methode hinzu:

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

Mit diesem Code wird eine neue Version des Fenster Controllers erstellt, das neue Fenster geladen, das Hauptfenster und das Schlüssel Fenster erstellt und der Titel festgelegt. Wenn wir nun die Anwendung ausführen und im Menü " **Datei** " die Option " **neu** " auswählen, wird ein neues Editor-Fenster geöffnet und angezeigt:

[![wurde ein neues unbenanntes Fenster hinzugefügt.](window-images/display04.png)](window-images/display04.png#lightbox)

Wenn wir das **Windows** -Menü öffnen, können Sie sehen, dass die Anwendung automatisch nachverfolgt und die geöffneten Fenster verarbeitet:

[![des Menüs "Windows"](window-images/display05.png)](window-images/display05.png#lightbox)

Weitere Informationen zum Arbeiten mit Menüs in einer xamarin. Mac-Anwendung finden Sie in der Dokumentation [Arbeiten mit Menüs](~/mac/user-interface/menu.md) .

### <a name="getting-the-currently-active-window"></a>Das aktuell aktive Fenster wird angezeigt.

In einer xamarin. Mac-Anwendung, die mehrere Fenster (Dokumente) öffnen kann, gibt es Zeiten, in denen Sie das aktuelle, oberste Fenster (das Schlüssel Fenster) erhalten müssen. Mit dem folgenden Code wird das Schlüssel Fenster zurückgegeben:

```csharp
var window = NSApplication.SharedApplication.KeyWindow;
```

Sie kann in jeder Klasse oder Methode aufgerufen werden, die Zugriff auf das aktuelle Schlüssel Fenster benötigt. Wenn derzeit kein Fenster geöffnet ist, wird `null`zurückgegeben.

### <a name="accessing-all-app-windows"></a>Zugreifen auf alle App-Fenster

Es kann vorkommen, dass Sie auf alle Fenster zugreifen müssen, die ihre xamarin. Mac-app derzeit geöffnet hat. Wenn Sie z. b. feststellen möchten, ob eine vom Benutzer geöffnete Datei bereits in einem beendenden Fenster geöffnet ist.

Der `NSApplication.SharedApplication` verwaltet eine `Windows` Eigenschaft, die ein Array aller geöffneten Fenster in der app enthält. Sie können dieses Array durchlaufen, um auf alle aktuellen Fenster der APP zuzugreifen. Beispiel:

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

Im Beispielcode wandeln wir jedes zurückgegebene Fenster in die benutzerdefinierte `ViewController` Klasse in unserer App um und testen den Wert einer benutzerdefinierten `Path` Eigenschaft anhand des Pfads einer Datei, die der Benutzer öffnen möchte. Wenn die Datei bereits geöffnet ist, wird das Fenster in den Vordergrund gebracht.

## <a name="adjusting-the-window-size-in-code"></a>Anpassen der Fenstergröße im Code

Es gibt Zeiten, in denen die Anwendung die Größe eines Fensters im Code ändern muss. Um die Größe und Position eines Fensters zu ändern, passen Sie die `Frame`-Eigenschaft an. Beim Anpassen der Größe eines Fensters müssen Sie in der Regel auch den Ursprung anpassen, damit das Fenster aufgrund des macOS-Koordinatensystems am gleichen Speicherort bleibt.

Anders als bei IOS, bei dem die obere linke Ecke (0,0) darstellt, verwendet macOS ein mathemkoordinaten System, bei dem die untere linke Ecke des Bildschirms (0,0) darstellt. In ios erhöhen sich die Koordinaten, wenn Sie nach unten nach rechts verschieben. In macOS erhöhen die Koordinaten den Wert nach oben nach rechts. 

Im folgenden Beispielcode wird die Größe eines Fensters geändert:

```csharp
nfloat y = 0;

// Calculate new origin
y = Frame.Y - (768 - Frame.Height);

// Resize and position window
CGRect frame = new CGRect (Frame.X, y, 1024, 768);
SetFrame (frame, true);
```

> [!IMPORTANT]
> Wenn Sie eine Windows-Größe und-Position im Code anpassen, müssen Sie sicherstellen, dass Sie die minimale und maximale Größe beachten, die Sie in Interface Builder festgelegt haben. Dies wird nicht automatisch berücksichtigt, und Sie können das Fenster vergrößern oder verkleinern als diese Grenzwerte.

## <a name="monitoring-window-size-changes"></a>Änderungen der Fenstergröße überwachen

Es kann vorkommen, dass Sie Änderungen an der Größe eines Fensters in ihrer xamarin. Mac-app überwachen müssen. So können Sie beispielsweise den Inhalt neu zeichnen, damit er der neuen Größe entspricht.

Stellen Sie zum Überwachen von Größenänderungen zunächst sicher, dass Sie eine benutzerdefinierte Klasse für den Fenster Controller in der Interface Builder von Xcode zugewiesen haben. `MasterWindowController` z. b. wie folgt aus:

[![](window-images/resize01.png "The Identity Inspector")](window-images/resize01.png#lightbox)

Bearbeiten Sie anschließend die benutzerdefinierte Fenster Controller-Klasse, und überwachen Sie das `DidResize`-Ereignis im Fenster des Controllers, um über Änderungen an der Live Größe benachrichtigt zu werden. Beispiel:

```csharp
public override void WindowDidLoad ()
{
    base.WindowDidLoad ();

    Window.DidResize += (sender, e) => {
        // Do something as the window is being live resized
    };
}
```

Optional können Sie das `DidEndLiveResize` Ereignis verwenden, um nur benachrichtigt zu werden, nachdem der Benutzer das Ändern der Fenstergröße abgeschlossen hat. Beispiel:

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

## <a name="setting-a-windows-title-and-represented-file"></a>Festlegen des Titels und der dargestellten Datei eines Fensters

Wenn Sie mit Windows arbeiten, das Dokumente darstellt, verfügt `NSWindow` über eine `DocumentEdited`-Eigenschaft, die bei Festlegung auf `true` einen kleinen Punkt in der Schaltfläche Schließen anzeigt, um dem Benutzer zu zeigen, dass die Datei geändert wurde und vor dem Schließen gespeichert werden muss.

Nun bearbeiten wir unsere `ViewController.cs` Datei und nehmen die folgenden Änderungen vor:

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

Wir überwachen außerdem das `WillClose`-Ereignis im Fenster und überprüfen den Status der `DocumentEdited`-Eigenschaft. Wenn es `true` ist, müssen Sie dem Benutzer die Möglichkeit einräumen, die Änderungen an der Datei zu speichern. Wenn wir unsere app ausführen und Text eingeben, wird der Punkt angezeigt:

[![](window-images/file01.png "A changed window")](window-images/file01.png#lightbox)

Wenn Sie versuchen, das Fenster zu schließen, erhalten Sie eine Warnung:

[![](window-images/file02.png "Displaying a save dialog")](window-images/file02.png#lightbox)

Wenn Sie ein Dokument aus einer Datei laden, legen Sie den Titel des Fensters mithilfe der `window.SetTitleWithRepresentedFilename (Path.GetFileName(path));`-Methode auf den Dateinamen fest (da `path` eine Zeichenfolge ist, die die Datei darstellt, die geöffnet wird). Außerdem können Sie die URL der Datei mit der `window.RepresentedUrl = url;`-Methode festlegen.

Wenn die URL auf einen vom Betriebssystem bekannten Dateityp verweist, wird das zugehörige Symbol in der Titelleiste angezeigt. Wenn der Benutzer mit der rechten Maustaste auf das Symbol klickt, wird der Pfad zur Datei angezeigt.

Bearbeiten Sie die Datei `AppDelegate.cs`, und fügen Sie die folgende Methode hinzu:

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

Wenn wir nun unsere app ausführen, wählen Sie im Menü **Datei** die Option **Öffnen...** aus, wählen Sie im Dialog Feld **Öffnen** eine Textdatei aus, und öffnen Sie Sie:

[Dialogfeld "Öffnen" ![](window-images/file03.png)](window-images/file03.png#lightbox)

Die Datei wird angezeigt, und der Titel wird mit dem Symbol der Datei festgelegt:

[![des Inhalts einer geladenen Datei](window-images/file04.png)](window-images/file04.png#lightbox)

## <a name="adding-a-new-window-to-a-project"></a>Hinzufügen eines neuen Fensters zu einem Projekt

Abgesehen vom Hauptdokument Fenster muss eine xamarin. Mac-Anwendung möglicherweise andere Windows-Typen für den Benutzer anzeigen, z. b. Einstellungen oder Inspektor-Panels.

Gehen Sie folgendermaßen vor, um ein neues Fenster hinzuzufügen:

1. Doppelklicken Sie im **Projektmappen-Explorer**auf die `Main.storyboard` Datei, um Sie in der Interface Builder von Xcode zur Bearbeitung zu öffnen.
2. Ziehen Sie einen neuen **Fenster Controller** aus der **Bibliothek** , und legen Sie ihn auf dem **Designoberfläche**:

    [![auswählen eines neuen Fenster Controllers in der Bibliothek](window-images/new01.png)](window-images/new01.png#lightbox)
3. Geben Sie im **Identitäts Inspektor**`PreferencesWindow` für die **Storyboard-ID**ein: 

    [![Festlegen der Storyboard-ID](window-images/new02.png)](window-images/new02.png#lightbox)
4. Entwerfen Sie Ihre Schnittstelle: 

    [![Entwerfen der Benutzeroberfläche](window-images/new03.png)](window-images/new03.png#lightbox)
5. Öffnen Sie das App-Menü (`MacWindows`), klicken Sie auf **Einstellungen...** , klicken Sie auf das neue Fenster, und ziehen Sie es in das Fenster: 

    [![Erstellen eines "*"](window-images/new05.png)](window-images/new05.png#lightbox)
6. Wählen Sie im Popupmenü die Option **anzeigen** aus.
7. Speichern Sie die Änderungen, und kehren Sie zu Visual Studio für Mac zurück, um mit Xcode zu synchronisieren.

Wenn wir den Code ausführen und die **Einstellungen** im **Anwendungsmenü**auswählen, wird das Fenster angezeigt:

[![Sie ein Beispiel Menü "Einstellungen"](window-images/new04.png)](window-images/new04.png#lightbox)

## <a name="working-with-panels"></a>Verwenden von Bereichen

Wie zu Beginn dieses Artikels erwähnt, schwebt ein Panel oberhalb der anderen Fenster und stellt Tools oder Steuerelemente bereit, mit denen Benutzer arbeiten können, während Dokumente geöffnet sind. 

Ebenso wie alle anderen Fenster, die Sie in ihrer xamarin. Mac-Anwendung erstellen und mit Ihnen arbeiten, ist der Prozess im Grunde das gleiche:

1. Fügen Sie dem Projekt eine neue Fenster Definition hinzu.
2. Doppelklicken Sie auf die Datei `.xib`, um den Fenster Entwurf für die Bearbeitung in der Interface Builder von Xcode zu öffnen.
3. Legen Sie alle erforderlichen Fenster Eigenschaften in der **Attribut** Prüfung und der **Größe Inspector**fest.
4. Ziehen Sie die Steuerelemente, die zum Erstellen der Schnittstelle erforderlich sind, und konfigurieren Sie Sie im **Attribut Inspektor**.
5. Verwenden Sie den **Größen Inspektor** , um die Größenänderung für Ihre Benutzeroberflächen Elemente zu verarbeiten.
6. Machen Sie die Benutzeroberflächen Elemente des C# Fensters für den Code über **Outlets** und **Aktionen**verfügbar.
7. Speichern Sie die Änderungen, und wechseln Sie zurück zu Visual Studio für Mac, um mit Xcode zu synchronisieren.

Im **Attribut Inspektor**verfügen Sie über die folgenden Optionen, die für Panels spezifisch sind:

[![des Attribut Inspektors](window-images/panel03.png)](window-images/panel03.png#lightbox)

- **Stil** : Hiermit können Sie den Stil des Panels anpassen: regulärer Bereich (sieht wie ein Standardfenster aus), Utility Panel (hat eine kleinere Titelleiste), HUD-Panel (ist durchlässig und die Titelleiste ist Teil des Hintergrunds).
- **Nicht aktivieren** : legt fest, dass im Panel zum Schlüssel Fenster wird.
- **Modales Dokument** : Wenn es sich um ein modales Dokument handelt, wird der Bereich nur über das Fenster der Anwendung bewegt, andernfalls schwebt er über alle Fenster.

Gehen Sie folgendermaßen vor, um einen neuen Bereich hinzuzufügen:

1. Klicken Sie im **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und wählen Sie > **neue Datei** **Hinzufügen** ... aus.
2. Wählen Sie im Dialogfeld neue Datei die Option **xamarin. Mac** > **Cocoa-Fenster mit Controller**:

    [![Hinzufügen eines neuen Fenster Controllers](window-images/panels00.png)](window-images/panels00.png#lightbox)

3. Geben Sie für den **Namen**`DocumentPanel` ein, und klicken Sie auf **Neu**.
4. Doppelklicken Sie auf die Datei `DocumentPanel.xib`, um Sie für die Bearbeitung in Interface Builder zu öffnen: 

    [![Bearbeiten des Panels](window-images/new02.png)](window-images/new02.png#lightbox)

5. Löschen Sie das vorhandene Fenster, und ziehen Sie ein Panel aus dem **Bibliotheks Inspektor** in den Schnittstellen- **Editor**: 

    [![Löschen des vorhandenen Fensters](window-images/panels01.png)](window-images/panels01.png#lightbox)

6. Schließen Sie den Bereich für den **Besitzer der Datei** - **Fenster** - **Outlets**ab: 

    [![ziehen, um den Bereich zu übertragen](window-images/panels02.png)](window-images/panels02.png#lightbox)

7. Wechseln Sie zum **Identitäts Inspektor** , und legen Sie die Klasse des Panels auf `DocumentPanel`fest: 

    [![Festlegen der Panel-Klasse](window-images/panels03.png)](window-images/panels03.png#lightbox)

8. Speichern Sie die Änderungen, und kehren Sie zu Visual Studio für Mac zurück, um mit Xcode zu synchronisieren.
9. Bearbeiten Sie die Datei `DocumentPanel.cs`, und ändern Sie die Klassendefinition wie folgt: 

    `public partial class DocumentPanel : NSPanel`

10. Speichern Sie die Änderungen in der Datei.

Bearbeiten Sie die Datei `AppDelegate.cs`, und legen Sie die `DidFinishLaunching`-Methode wie folgt an:

```csharp
public override void DidFinishLaunching (NSNotification notification)
{
        // Display panel
    var panel = new DocumentPanelController ();
    panel.Window.MakeKeyAndOrderFront (this);
}

```

Wenn wir die Anwendung ausführen, wird der Bereich angezeigt:

[![des Bereichs in einer laufenden app](window-images/panels04.png)](window-images/panels04.png#lightbox)

> [!IMPORTANT]
> Panel Fenster wurden von Apple als veraltet markiert und sollten durch Inspektor- **Schnittstellen**ersetzt werden. Ein vollständiges Beispiel für das Erstellen eines **Inspektors** in einer xamarin. Mac-app finden Sie in der Beispiel-App für [macinspector](https://docs.microsoft.com/samples/xamarin/mac-samples/macinspector) .

## <a name="summary"></a>Summary

In diesem Artikel wurde die Arbeit mit Fenstern und Bereichen in einer xamarin. Mac-Anwendung ausführlich erläutert. Wir haben die verschiedenen Typen und Verwendungsmöglichkeiten von Fenstern und Bereichen, das Erstellen und Verwalten von Fenstern und Bereichen in der Interface Builder von Xcode und das Arbeiten mit Fenstern und Bereichen C# im Code gesehen.

## <a name="related-links"></a>Verwandte Links

- [Macwindows (Beispiel)](https://docs.microsoft.com/samples/xamarin/mac-samples/macwindows)
- [Macinspector (Beispiel)](https://docs.microsoft.com/samples/xamarin/mac-samples/macinspector)
- [Hello, Mac (Hallo, Mac)](~/mac/get-started/hello-mac.md)
- [Arbeiten mit Menüs](~/mac/user-interface/menu.md)
- [macOS-Entwurfs Designs (Apple)](https://developer.apple.com/design/human-interface-guidelines/macos/overview/themes/)
- [Fenster, Panels und Bildschirme (Apple)](https://developer.apple.com/documentation/appkit/windows_panels_and_screens)
