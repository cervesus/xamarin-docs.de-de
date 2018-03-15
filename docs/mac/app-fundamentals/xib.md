---
title: .XIB-Dateien
description: "In diesem Artikel wird das Arbeiten mit .xib erstellten Dateien in Xcodes Benutzeroberflächen-Generator erstellen und Verwalten von Benutzeroberflächen für eine Anwendung Xamarin.Mac behandelt."
ms.topic: article
ms.prod: xamarin
ms.assetid: 6AF3D216-448D-4B2D-9026-74E4FFF5923A
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 92ca65409dd82806278885bb03efd7b04ab1827d
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/15/2018
---
# <a name="xib-files"></a>.XIB-Dateien

_In diesem Artikel wird das Arbeiten mit .xib erstellten Dateien in Xcodes Benutzeroberflächen-Generator erstellen und Verwalten von Benutzeroberflächen für eine Anwendung Xamarin.Mac behandelt._

> [!NOTE]
> Die bevorzugte Methode zum Erstellen einer Benutzeroberfläche für eine app Xamarin.Mac ist mit Storyboards. In dieser Dokumentation wurde aus Verlaufsgründen und zum Arbeiten mit älteren Xamarin.Mac Projekten beibehalten wurde. Weitere Informationen finden Sie unter unsere [Einführung in Storyboards](~/mac/platform/storyboards/index.md) Dokumentation.

## <a name="overview"></a>Übersicht

Bei der Arbeit mit c# und .NET in einer Anwendung Xamarin.Mac Sie haben Zugriff auf die gleichen Elemente der Benutzeroberfläche und tools, die ein Entwickler arbeiten in *Objective-C* und *Xcode* verfügt. Da Xamarin.Mac direkt mit Xcode integriert ist, können Sie die Xcode _Schnittstelle-Generator_ zu erstellen und Verwalten Ihrer Benutzerschnittstellen (oder erstellen sie optional direkt im C#-Code).

Eine .xib-Datei wird von MacOS verwendet, um Elemente von Ihrer Anwendung Benutzeroberfläche (z. B. Menüs, Windows, Sichten, Bezeichnungen, Textfelder), die erstellt und verwaltet werden grafisch in Xcodes Benutzeroberflächen-Generator zu definieren.

[![Ein Beispiel für die ausgeführte app](xib-images/intro01.png "ein Beispiel für die ausgeführte app")](xib-images/intro01-large.png#lightbox)

In diesem Artikel werden die Grundlagen der Arbeit mit .xib-Dateien in einer Anwendung Xamarin.Mac eingegangen. Wird mit hoher vorgeschlagen, dass Sie über arbeiten die [Hello, Mac](~/mac/get-started/hello-mac.md) Artikel zuerst behandelt wichtige Konzepte und Techniken, die in diesem Artikel verwendet werden.

Sie möchten einen Blick auf die [Verfügbarmachen von C#-Klassen / Methoden für Objective-C-](~/mac/internals/how-it-works.md) Teil der [Xamarin.Mac Internals](~/mac/internals/how-it-works.md) dokumentieren, es wird erläutert, die `Register` und `Export` Attribute zum Einrichten Ihrer C#-Klassen für Objective-C-Objekte und UI Elemente von Netzwerkdaten verwendet.


## <a name="introduction-to-xcode-and-interface-builder"></a>Einführung in Xcode und Interface Builder

Im Rahmen von Xcode hat Apple erstellt ein Tool namens Schnittstelle-Generator, mit dem Sie Ihre Benutzeroberfläche visuell in einem Designer erstellen können. Xamarin.Mac integriert Schrift fließend Schnittstelle-Generator, und Sie können die Benutzeroberfläche mit den gleichen Tools zu erstellen, die Objective-C-Benutzer ausführen.


### <a name="components-of-xcode"></a>Komponenten von Xcode

Wenn Sie eine Datei .xib in Xcode aus Visual Studio für Mac öffnen, öffnet Sie mit einem **Projektnavigators** auf der linken Seite der **Schnittstellenhierarchie** und **Benutzeroberflächen-Editors** in der Mitte , und ein **Eigenschaften und Energieversorgung** Abschnitt auf der rechten Seite:

[![Die Bestandteile der Xcode UI](xib-images/xcode03.png "die Bestandteile der Xcode UI")](xib-images/xcode03-large.png#lightbox)

Werfen wir einen Blick auf, wofür jedes der folgenden Abschnitte Xcode wird und wie Sie mit werden die Benutzeroberfläche für Ihre Anwendung Xamarin.Mac zu erstellen.


#### <a name="project-navigation"></a>Projektnavigation

Wenn Sie eine .xib-Datei zur Bearbeitung in Xcode öffnen, erstellt Visual Studio für Mac eine Projektdatei Xcode im Hintergrund, um die Änderungen zwischen sich selbst und Xcode zu kommunizieren. Später, wenn Sie wieder zu Visual Studio für Mac von Xcode wechseln, werden alle Änderungen an diesem Projekt mit Xamarin.Mac Projekt von Visual Studio für Mac synchronisiert

Die **Projektnavigation** Abschnitt können Sie zum Navigieren zwischen aller Dateien, die diese bilden _Shim_ Xcode-Projekt. In der Regel wird nur sichtbar, die .xib-Dateien in dieser Liste, z. B. interessiert **MainMenu.xib** und **MainWindow.xib**.


#### <a name="interface-hierarchy"></a>Hierarchie der Benutzeroberfläche

Die **Schnittstellenhierarchie** Abschnitt können Sie problemlos mehrere wichtige Eigenschaften der Benutzeroberfläche wie z. B. den Zugriff auf seine **Platzhalter** und main **Fenster**. In diesem Abschnitt können auch den Zugriff auf einzelne Elemente (Views), die Stellen Einrichten der Benutzeroberfläche und passen Sie die die Möglichkeit, dass sie geschachtelt sind, indem Sie sie um innerhalb der Hierarchie ziehen.


#### <a name="interface-editor"></a>Benutzeroberflächen-Editors

Die **Benutzeroberflächen-Editors** Abschnitt enthält die Oberfläche, auf dem Sie grafisch Layout der Benutzeroberfläche. Ziehen Sie Elemente aus der **Bibliothek** Teil der **Eigenschaften und Energieversorgung** Abschnitt aus, um den Entwurf zu erstellen. Während Sie Elemente der Benutzeroberfläche (Ansichten), die der Entwurfsoberfläche hinzufügen, werden hinzugefügt die **Schnittstellenhierarchie** Abschnitt in der Reihenfolge, die sie in angezeigt werden die **Benutzeroberflächen-Editors**.


#### <a name="properties--utilities"></a>Eigenschaften und Energieversorgung

Die **Eigenschaften und Energieversorgung** Abschnitt ist in zwei Hauptabschnitte, die wir arbeiten mit unterteilt **Eigenschaften** (auch als Inspektoren bezeichnet) und die **Bibliothek**:

![Die Eigenschaftenanalyse](xib-images/xcode04.png "der Eigenschaftenanalyse")

Ursprünglich in diesem Abschnitt ist fast leer, jedoch bei der Auswahl eines Elements in der **Benutzeroberflächen-Editors** oder **Schnittstellenhierarchie**, **Eigenschaften** Abschnitt gefüllt mit Informationen zu den angegebenen Element und die Eigenschaften, die Sie anpassen können.

Im Abschnitt **Eigenschaften** gibt es wie in der folgenden Abbildung gezeigt acht verschiedene *Inspektorregisterkarten*:

[![Eine Übersicht über alle Inspektoren](xib-images/xcode05.png "einen Überblick über alle Inspektoren")](xib-images/xcode05-large.png#lightbox)

Es gibt die folgenden Registerkarten (von links nach rechts):

-   **Dateiinspektor**: Der Dateiinspektor zeigt Dateiinformationen an, wie z.B. den Dateinamen der XIB-Datei, die bearbeitet wird.
-   **Schnellhilfe**: Die Registerkarte „Schnellhilfe“ bietet Kontexthilfe, je nachdem, was in Xcode ausgewählt ist.
-   **Identitätsinspektor**: Der Identitätsinspektor bietet Informationen zu dem ausgewählten Steuerelement bzw. der ausgewählten Ansicht.
-   **Attribute Inspektor** – die Attribute Inspector können Sie verschiedene Attribute der ausgewählten Steuerelementansicht anpassen.
-   **Size-Inspektor** – die Größe Inspektor können Sie die Größe und Ändern der Größe der ausgewählten Steuerelementansicht Verhalten zu steuern.
-   **Verbindungen Inspektor** – der Verbindungen Inspektor zeigt den Ausgang und Aktion Verbindungen der ausgewählten Steuerelemente. Ausgänge und Aktionen untersucht in nur wenigen Augenblicken.
-   **Bindungen-Inspektor** – die Bindungen Inspector können Sie Steuerelemente so konfigurieren, dass ihre Werte automatisch mit Datenmodellen gebunden sind.
-   **Anzeigen der Auswirkungen Inspektor** – der Ansicht Effekte Inspector können Sie Auswirkungen auf die Steuerelemente, z. B. Animationen angeben.

In der **Bibliothek** Abschnitt können Sie Steuerelemente suchen und Objekte grafisch an in den Designer zum Erstellen der Benutzeroberfläche:

![Ein Beispiel für die Bibliothek Inspektor](xib-images/xcode06.png "ein Beispiel für die Bibliothek-Inspektor")

Nun, dass Sie mit dem Xcode IDE und Benutzeroberflächen-Generator vertraut sind, betrachten wir dazu verwenden, um eine Benutzeroberfläche erstellen.


## <a name="creating-and-maintaining-windows-in-xcode"></a>Erstellen und Verwalten von Windows in Xcode

Die bevorzugte Methode zum Erstellen einer Xamarin.Mac-app-Benutzeroberfläche ist mit Storyboards (finden Sie in unserer [Einführung in Storyboards](~/mac/platform/storyboards/index.md) Dokumentation weitere Informationen) und daher neues-Projekt gestartet hat, Xamarin.Mac wird Storyboards wird standardmäßig verwendet.

Um zum Wechseln mit einem .xib-basierte Benutzeroberfläche, gehen Sie folgendermaßen vor:

1. Öffnen Sie Visual Studio für Mac, und starten Sie ein neues Xamarin.Mac-Projekt.
2. In der **Lösung Pad**mit der rechten Maustaste auf das Projekt, und wählen Sie **hinzufügen** > **neue Datei...**
3. Wählen Sie **Mac** > **Windows Domänencontroller**:

    ![Hinzufügen eines neuen Fensters Controllers](xib-images/setup00.png "Hinzufügen eines neuen Fensters-Controllers")
4. Geben Sie `MainWindow` für den Namen und klicken Sie auf die **neu** Schaltfläche:

    ![Hinzufügen eines neuen Hauptfensters](xib-images/setup01.png "Hinzufügen einer neuen Hauptfenster")
5. Mit der rechten Maustaste auf das Projekt erneut, und wählen Sie **hinzufügen** > **neue Datei...**
6. Wählen Sie **Mac** > **Hauptmenü**:

    ![Hinzufügen einer neuen Hauptmenü](xib-images/setup02.png "Hinzufügen eines neuen Main-Menüs")
7. Lassen Sie den Namen als `MainMenu` , und klicken Sie auf die **neu** Schaltfläche.
8. In der **Lösung Pad** wählen Sie die **Main.storyboard** Datei, mit der rechten Maustaste, und wählen Sie **entfernen**:

    ![Auswählen der Haupt-Storyboards](xib-images/setup03.png "Haupt-Storyboard auswählen")
9. Klicken Sie im Dialogfeld "entfernen" auf die **löschen** Schaltfläche:

    ![Bestätigen den Löschvorgang](xib-images/setup04.png "den Löschvorgang bestätigen")
10. In der **Lösung Pad**, doppelklicken Sie auf die **"Info.plist"** Datei zur Bearbeitung zu öffnen.
11. Wählen Sie `MainMenu` aus der **Hauptbenutzeroberfläche** Dropdownliste:

    [![Festlegen der Hauptmenü](xib-images/setup05.png "im Hauptmenü festlegen")](xib-images/setup05-large.png#lightbox)
12. In der **Lösung Pad**, doppelklicken Sie auf die **MainMenu.xib** Datei, um ihn zur Bearbeitung in Xcodes Benutzeroberflächen-Generator zu öffnen.
13. In der **Bibliothek Inspektor**, Typ `object` im Feld "Suchen" ziehen Sie dann ein neues **Objekt** auf die Entwurfsoberfläche:

    [![Bearbeiten Sie im Hauptmenü](xib-images/setup06.png "bearbeiten Sie im Hauptmenü")](xib-images/setup06-large.png#lightbox)
14. In der **Identität Inspektor**, geben Sie `AppDelegate` für die **Klasse**:

    [![Auswählen der App-Delegat](xib-images/setup07.png "Auswählen der App-Delegat")](xib-images/setup07-large.png#lightbox)
15. Wählen Sie **des Dateibesitzer** aus der **Schnittstellenhierarchie**, wechseln Sie zu der **Verbindung Inspektor** und ziehen Sie eine Zeile aus der Delegat, der die `AppDelegate` **Objekt** soeben dem Projekt hinzugefügt:

    [![Herstellen einer Verbindung der App-Delegat](xib-images/setup08.png "Herstellen einer Verbindung der App-Delegat")](xib-images/setup08-large.png#lightbox)
16. Die Änderungen zu speichern und zurück zu Visual Studio für Mac.

Mit diesen Änderungen direktes Bearbeiten der **AppDelegate.cs** Datei, und stellen sie wie folgt aussehen:

```csharp
using AppKit;
using Foundation;

namespace MacXib
{
    [Register ("AppDelegate")]
    public class AppDelegate : NSApplicationDelegate
    {
        public MainWindowController mainWindowController { get; set; }

        public AppDelegate ()
        {
        }

        public override void DidFinishLaunching (NSNotification notification)
        {
            // Insert code here to initialize your application
            mainWindowController = new MainWindowController ();
            mainWindowController.Window.MakeKeyAndOrderFront (this);
        }

        public override void WillTerminate (NSNotification notification)
        {
            // Insert code here to tear down your application
        }
    }
}
```

Jetzt im Hauptfenster der Anwendung definiert ist ein **.xib** beim Hinzufügen eines Controllers Fenster automatisch im Projekt enthaltene Datei. So bearbeiten Sie die Windows-Design in der **Lösung Pad**, doppelklicken klicken Sie auf die **MainWindow.xib** Datei:

![Auswählen der Datei MainWindow.xib](xib-images/edit01.png "MainWindow.xib auswählen")

Dies öffnet das Fenster Design in Xcodes Benutzeroberflächen-Generator:

[![Bearbeiten die MainWindow.xib](xib-images/edit02.png "der MainWindow.xib bearbeiten")](xib-images/edit02-large.png#lightbox)


### <a name="standard-window-workflow"></a>Standardfenster-workflow

Für ein Fenster, das Sie erstellen und in der Anwendung Xamarin.Mac verwenden, ist der Prozess im Wesentlichen identisch:

1. Für neue Fenster, die nicht die Standardeinstellung, die dem Projekt automatisch hinzugefügt werden, wird dem Projekt eine neue Fensterdefinition hinzugefügt.
2. Doppelklicken Sie auf die .xib-Datei, um den Entwurf der Fenster zur Bearbeitung in Xcodes Benutzeroberflächen-Generator zu öffnen.
3. Legen Sie alle erforderlichen Eigenschaften der **Attribut Inspektor** und **Größe Inspektor**.
4. Ziehen Sie in den Steuerelementen, die erforderlich sind, damit Ihre Schnittstelle erstellen und konfigurieren sie in der **Attribut Inspektor**.
5. Verwenden der **Größe Inspektor** behandelt, die größenanpassung von UI-Elementen.
6. Verfügbar machen Sie die Elemente der Benutzeroberfläche des Fensters für C#-Code über die Steckdosen und Aktionen.
7. Speichern Sie die Änderungen zu, und wechseln Sie zurück zu Visual Studio für Mac mit Xcode zu synchronisieren.


### <a name="designing-a-window-layout"></a>Entwerfen einer Fensterlayout

Der Prozess für das Layout einer Benutzeroberfläche im Benutzeroberflächen-Generator entspricht im Wesentlichen dem für jedes Element, das Sie hinzufügen:

1. Suchen Sie das gewünschte Steuerelement in der **Bibliothek Inspektor** , und ziehen Sie ihn in die **Benutzeroberflächen-Editors** und positionieren Sie ihn.
2. Legen Sie alle erforderlichen Eigenschaften der **Attribut Inspektor**.
3. Verwenden der **Größe Inspektor** behandelt, die größenanpassung von UI-Elementen.
4. Wenn Sie eine benutzerdefinierte Klasse verwenden, legen Sie sie der **Identität Inspektor**.
5. Verfügbar machen Sie die Elemente der Benutzeroberfläche für C#-Code über die Steckdosen und Aktionen.
6. Speichern Sie die Änderungen zu, und wechseln Sie zurück zu Visual Studio für Mac mit Xcode zu synchronisieren.

Beispiel:

1. Ziehen Sie in Xcode eine **Befehlsschaltfläche** aus dem **Bibliotheksbereich**:

    [![Eine Schaltfläche auswählen, aus der Bibliothek](xib-images/xcode07.png "Sie eine Schaltfläche aus der Bibliothek auswählen.")](xib-images/xcode07-large.png#lightbox)
2. Legen Sie das Steuerelement auf die **Fenster** in der **Benutzeroberflächen-Editors**:

    [![Hinzufügen einer Schaltfläche zum Fenster](xib-images/xcode08.png "Fenster eine Schaltfläche hinzufügen")](xib-images/xcode08-large.png#lightbox)
3. Klicken Sie auf die Eigenschaft **Titel** im **Attributinspektor**, und ändern Sie den Titel der Schaltfläche in `Click Me`:

    ![Festlegen der Attribute für die Schaltfläche](xib-images/xcode09.png "die Schaltfläche Attribute festlegen")
4. Ziehen Sie eine **Bezeichnung** aus dem **Bibliotheksbereich**:

    [![Auswählen einer Bezeichnung in der Bibliothek](xib-images/xcode10.png "eine Bezeichnung in der Bibliothek auswählen")](xib-images/xcode10-large.png#lightbox)
5. Fügen Sie die Bezeichnung im **Fenster** neben der Schaltfläche im **Schnittstellen-Editor** ein:

    [![Hinzufügen einer Bezeichnung in das Fenster](xib-images/xcode11.png "Fenster eine Bezeichnung hinzugefügt")](xib-images/xcode11-large.png#lightbox)
6. Klicken Sie auf den rechten Ziehpunkt der Bezeichnung, und ziehen Sie diesen nach rechts, bis der Rand des Fensters erreicht ist:

    [![Ändern der Größe der Bezeichnung](xib-images/xcode12.png "Ändern der Größe der Bezeichnung")](xib-images/xcode12-large.png#lightbox)
7. Mit der Bezeichnung ausgewählter der **Benutzeroberflächen-Editors**, wechseln Sie zu der **Größe Inspektor**:

    ![Auswählen des Größe Inspektors](xib-images/xcode13.png "den Inspektor Größe auswählen")
8. In der **Automatisches Anpassen der Größe im** klicken Sie auf die **Dim Rot Klammer** rechts und die **Dim rote, horizontale Pfeil** in der Mitte:

    ![Bearbeiten der Eigenschaften von Automatisches Anpassen der Größe](xib-images/xcode14.png "Bearbeiten der Eigenschaften von Automatisches Anpassen der Größe")
9. Dadurch wird sichergestellt, dass die Bezeichnung gestreckt wird, vergrößert und verkleinert werden, während das Fenster in der laufenden Anwendung angepasst wird. Die **Rot Klammern** und die oberen und linken Rand der **Automatisches Anpassen der Größe im** Feld Teilen Sie die Bezeichnung, die auf den angegebenen X- und Y Speicherorte hängen bleiben.
10. Speichern Sie die Änderungen an der Benutzeroberfläche

Beim Ändern der Größe und Verschieben von Steuerelementen, um wurden, Sie sollten aufgefallen, dass Schnittstelle-Generator Sie Snap hilfreiche Hinweise können auf der Grundlage von [OS X-Richtlinien für menschliche Benutzeroberfläche](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/). Diese Richtlinien hilft Ihnen hochwertige Anwendungen zu erstellen, die ein vertraut Aussehen und Verhalten für Mac-Benutzer verfügen sollen.

Wenn Sie, in Suchen der **Schnittstellenhierarchie** Abschnitt, beachten Sie, wie das Layout und die Hierarchie der Elemente, aus denen unsere Benutzeroberfläche angezeigt werden:

![Auswählen eines Elements in der Schnittstellenhierarchie](xib-images/xcode15.png "Auswählen eines Elements in der Schnittstellenhierarchie")

Von hier aus können Sie auswählen, Elemente bearbeiten, oder ziehen Elemente der Benutzeroberfläche neu anordnen, bei Bedarf. Wenn ein Element der Benutzeroberfläche von einem anderen Element abgedeckt wird, wurde, können Sie es z. B. an das Ende der Liste, um erleichtern das oberste Element im Fenster ziehen.

Weitere Informationen zum Arbeiten mit Windows in einer Anwendung Xamarin.Mac finden Sie unter unsere [Windows](~/mac/user-interface/window.md) Dokumentation.


## <a name="exposing-ui-elements-to-c-code"></a>Verfügbarmachen von Benutzeroberflächenelementen für C#-code

Nachdem Sie das Aussehen und Verhalten der Benutzeroberfläche im Benutzeroberflächen-Generator Layouts abgeschlossen haben, müssen Sie die Elemente der Benutzeroberfläche verfügbar zu machen, damit diese von C#-Code zugegriffen werden kann. Zu diesem Zweck müssen Sie Aktionen und Ausgänge verwenden.


### <a name="setting-a-custom-main-window-controller"></a>Festlegen eines benutzerdefinierten Hauptfenster-Controllers

Um Steckdosen und Aktionen zum Verfügbarmachen von Benutzeroberflächenelementen für C#-Code erstellt werden, müssen die Xamarin.Mac app einen benutzerdefinierten Fenster Controller verwenden.

Führen Sie folgende Schritte aus:

1. Öffnen Sie die app-Storyboard in Xcode des Benutzeroberflächen-Generator.
2. Wählen Sie die `NSWindowController` in der Entwurfsoberfläche angezeigt.
3. Wechseln Sie zu der **Identität Inspektor** anzuzeigen, und geben Sie `WindowController` als die **Klassenname**:

    [![Bearbeiten den Klassennamen](xib-images/windowcontroller01.png "bearbeiten den Klassennamen abrufen")](xib-images/windowcontroller01-large.png#lightbox)
4. Die Änderungen zu speichern und zurück zu Visual Studio für Mac synchronisiert werden.
5. Ein **WindowController.cs** Datei wird hinzugefügt werden, um das Projekt in der **Lösung Pad** in Visual Studio für Mac:

    ![Der neue Klassenname in Visual Studio für Mac](xib-images/windowcontroller02.png "dem neuen Klassennamen in Visual Studio für Mac")
6. Öffnen Sie das Storyboard in Xcode des Benutzeroberflächen-Generator.
7. Die **WindowController.h** -Datei wird für die Verwendung verfügbar sein:

    [![Die übereinstimmenden .h-Datei in Xcode](xib-images/windowcontroller03.png "der übereinstimmenden .h-Datei in Xcode")](xib-images/windowcontroller03-large.png#lightbox)


### <a name="outlets-and-actions"></a>Ausgänge und Aktionen

Was sind, Steckdosen und Aktionen? Bei der herkömmlichen .NET-Benutzeroberflächenprogrammierung wird ein Steuerelement auf der Benutzeroberfläche automatisch als Eigenschaft verfügbar gemacht, wenn es hinzugefügt wird. Unter Mac ist dies anders. Wenn Sie ein Steuerelement zu einer Ansicht hinzufügen, wird es dadurch nicht automatisch für Code zugänglich. Der Entwickler muss die UI-Elemente explizit für Code verfügbar machen. In der Reihenfolge hierzu Apple erhalten Sie zwei Optionen:

-  **Outlets**: Outlets sind vergleichbar mit Eigenschaften. Wenn Sie ein Steuerelement an einer Steckdose anzuschließen, zum Code über eine Eigenschaft bereitgestellt, damit Sie Aufgaben wie das Anfügen von Ereignishandlern ausführen können, rufen Methoden auf, usw.
-  **Aktionen**: Aktionen sind vergleichbar mit dem Befehlsmuster in WPF. Z. B. wenn eine Aktion für ein Steuerelement ausgeführt wird, die z. B. eine Schaltfläche klicken, die das Steuerelement wird automatisch eine Methode im Code aufrufen. Aktionen sind leistungsstark und praktische, da Sie viele Steuerelemente auf der gleichen Aktion verbinden können.

In Xcode Steckdosen und Aktionen hinzukommen direkt im Code über *Steuerelement ziehen*. Genauer gesagt, bedeutet dies, um einen Ausgang oder Aktion zu erstellen, welches Steuerelement Element auszuwählen und Hinzufügen eines Steckdose oder Aktion, halten Sie möchten die **Steuerelement** Schaltfläche auf der Tastatur, und ziehen Sie das Steuerelement direkt in den Code.

Für Entwickler, Xamarin.Mac bedeutet dies, dass Sie auf der C#-Datei in die Objective-C-Stub-Dateien, die entsprechen ziehen, in der Sie den Ausgang oder die Aktion erstellen möchten. Visual Studio für Mac erstellt eine Datei namens **MainWindow.h** als Teil der Shim Xcode-Projekt, das sie zum Verwenden des Generators für Schnittstelle generiert:

[![Ein Beispiel für eine .h-Datei in Xcode](xib-images/xcode16.png "ein Beispiel für eine .h-Datei in Xcode")](xib-images/xcode16-large.png#lightbox)

Dieser Stub .h-Datei spiegelt die **MainWindow.designer.cs** , wird automatisch hinzugefügt, zu einem Projekt Xamarin.Mac, wenn ein neuer `NSWindow` erstellt wird. Diese Datei wird verwendet, um die vom Generator für Schnittstelle vorgenommenen Änderungen zu synchronisieren und, in denen wir die Steckdosen und Aktionen erstellen, damit Elemente der Benutzeroberfläche für C#-Code verfügbar gemacht werden.


#### <a name="adding-an-outlet"></a>Hinzufügen von einer Steckdose

Mit der ein grundlegendes Verständnis dafür, welche Ausgänge und Aktionen sind, betrachten wir dazu erstellen eine Steckdose, um ein Element der Benutzeroberfläche in den C#-Code verfügbar zu machen.

Führen Sie folgende Schritte aus:

1. Klicken Sie in der rechten oberen Ecke in Xcode auf die Schaltfläche mit den **zwei Kreisen**, um den **Assistenten-Editor** zu öffnen:

    [![Auswählen des Assistenten-Editors](xib-images/outlet01.png "der Assistant-Editor auswählen")](xib-images/outlet01-large.png#lightbox)
2. Xcode wechselt in eine geteilte Ansicht mit dem **Schnittstellen-Editor** auf der einen und dem **Code-Editor** auf der anderen Seite.
3. Beachten Sie die Xcode automatisch übernommen hat die **MainWindowController.m** in der Datei die **Code-Editor**, falsch. Wenn Sie, was wissen oben Steckdosen und Aktionen werden der Beschreibung, müssen wir die **MainWindow.h** ausgewählten.
4. Am oberen Rand der **Code-Editor** klicken Sie auf die **automatische Verknüpfung** , und wählen Sie die **MainWindow.h** Datei:

    [![Auswählen der richtigen .h-Datei](xib-images/outlet02.png "Auswählen der richtigen .h-Datei")](xib-images/outlet02-large.png#lightbox)
5. Xcode sollte jetzt die korrekte Datei ausgewählt haben:

    [![Die richtige Datei ausgewählt](xib-images/outlet03.png "die richtige Datei ausgewählt")](xib-images/outlet03-large.png#lightbox)
6. **Der letzte Schritt war sehr wichtig!** Wenn Sie die richtige Datei ausgewählt haben, kann nicht zum Erstellen von Steckdosen und Aktionen oder sie werden verfügbar gemacht werden auf die falschen Klasse in c#!
7. In der **Benutzeroberflächen-Editors**, halten Sie die **Steuerelement** Taste auf der Tastatur, und klicken und ziehen die Bezeichnung wird oben auf den Code-Editor erstellten direkt unterhalb der `@interface MainWindow : NSWindow { }` Code:

    [![Ziehen Sie zum Erstellen einer neuen Steckdose](xib-images/outlet04.png "ziehen, um einen neuen Anschluss erstellen")](xib-images/outlet04-large.png#lightbox)
8. Es wird ein Dialogfeld geöffnet. Lassen Sie die **Verbindung** Steckdose festgelegt, und geben Sie `ClickedLabel` für die **Namen**:

    [![Festlegen der Eigenschaften der Steckdose](xib-images/outlet05.png "Festlegen der Eigenschaften")](xib-images/outlet05-large.png#lightbox)
9. Klicken Sie auf die **verbinden** Schaltfläche, um den Ausgang zu erstellen:

    ![Der abgeschlossene Ausgang](xib-images/outlet06.png "der abgeschlossenen Ausgang")
10. Speichern Sie die Änderungen in der Datei.


#### <a name="adding-an-action"></a>Hinzufügen einer Aktion

Als Nächstes sehen wir uns, erstellen eine Aktion aus, um eine Interaktion mit UI-Elements in den C#-Code verfügbar zu machen.

Führen Sie folgende Schritte aus:

1. Sicherstellen, dass wir sind noch in der **Assistant Editor** und die **MainWindow.h** Datei wird in der **Code-Editor**.
2. In der **Benutzeroberflächen-Editors**, halten Sie die **Steuerelement** Taste auf der Tastatur, und klicken und ziehen die Schaltfläche mit den oben auf der Code-Editor erstellten direkt unterhalb der `@property (assign) IBOutlet NSTextField *ClickedLabel;` Code:

    [![Ziehen Sie zum Erstellen einer Aktion](xib-images/action01.png "ziehen, die zum Erstellen einer Aktion")](xib-images/action01-large.png#lightbox)
3. Ändern der **Verbindung** Typ in Aktion:

    [![Wählen Sie eine Aktion](xib-images/action02.png "wählen Sie eine Aktion")](xib-images/action02-large.png#lightbox)
4. Geben Sie `ClickedButton` als **Namen** ein:

    [![Konfigurieren der Aktion](xib-images/action03.png "Konfigurieren der Aktion")](xib-images/action03-large.png#lightbox)
5. Klicken Sie auf die **verbinden** Schaltfläche, um die Aktion zu erstellen:

    ![Die abgeschlossene Aktion](xib-images/action04.png "der abgeschlossenen Aktivität")
6. Speichern Sie die Änderungen in der Datei.

Mit der Benutzeroberfläche wired von Volltextkatalogen und für C#-Code verfügbar gemacht wechseln Sie zurück zu Visual Studio für Mac aus, und lassen sie die Änderungen von Xcode und Schnittstelle-Generator synchronisiert.


### <a name="writing-the-code"></a>Das Schreiben des Codes

Die Benutzeroberfläche erstellt und die Elemente der Benutzeroberfläche für einen Überprüfungscode Steckdosen und Aktionen verfügbar gemacht sind Sie bereit, schreiben den Code, um das Programm lebendig werden lassen. Öffnen Sie beispielsweise die **MainWindow.cs** Datei für die Bearbeitung durch Doppelklick im die **Lösung Pad**:

[![Die Datei MainWindow.cs](xib-images/code01.png "der MainWindow.cs-Datei")](xib-images/code01-large.png#lightbox)

Fügen Sie den folgenden Code hinzu, und die `MainWindow` -Klasse mit der Beispiel-Ausgang verwendet, die Sie soeben erstellt haben:

```csharp
private int numberOfTimesClicked = 0;
...

public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Set the initial value for the label
    ClickedLabel.StringValue = "Button has not been clicked yet.";
}
```

Beachten Sie, dass die `NSLabel` erfolgt in c# durch direkte Name, dass Sie es in Xcode zugewiesen, wenn Sie die Steckdose in Xcode erstellt, in diesem Fall heißt es `ClickedLabel`. Sie können Zugriff auf eine beliebige Methode oder Eigenschaft des verfügbar gemachten Objekts die gleiche Weise alle normale C#-Klasse.

> [!IMPORTANT]
> Benötigen Sie `AwakeFromNib`, anstelle einer anderen Methode wie z. B. `Initialize`, da `AwakeFromNib` heißt _nach_ das Betriebssystem geladen und der Benutzeroberfläche aus der Datei .xib instanziiert wurde. Wenn Sie versucht, auf das Label-Steuerelement, bevor Sie die Datei .xib wurde vollständig geladen und instanziiert, erhalten Sie eine `NullReferenceException` Fehler, da das Label-Steuerelement noch nicht erstellt werden würde.

Fügen Sie die folgende partielle Klasse, die `MainWindow` Klasse:

```csharp
partial void ClickedButton (Foundation.NSObject sender) {

    // Update counter and label
    ClickedLabel.StringValue = string.Format("The button has been clicked {0} time{1}.",++numberOfTimesClicked, (numberOfTimesClicked < 2) ? "" : "s");
}
```

Dieser Code fügt an die Aktion, die Sie in Xcode und Benutzeroberflächen-Generator erstellt und jedes Mal, die der Benutzer auf die Schaltfläche klickt aufgerufen werden.

Einige Elemente der Benutzeroberfläche automatisch haben, für die Aktionen, z. B. Elemente in der Standard-Menüleiste wie z. B. die **öffnen...**  Menüelement (`openDocument:`). In der **Lösung Pad**, doppelklicken Sie auf die **AppDelegate.cs** Datei zur Bearbeitung öffnen, und fügen Sie den folgenden Code unter der `DidFinishLaunching` Methode:

```csharp
[Export ("openDocument:")]
void OpenDialog (NSObject sender)
{
    var dlg = NSOpenPanel.OpenPanel;
    dlg.CanChooseFiles = false;
    dlg.CanChooseDirectories = true;

    if (dlg.RunModal () == 1) {
        var alert = new NSAlert () {
            AlertStyle = NSAlertStyle.Informational,
            InformativeText = "At this point we should do something with the folder that the user just selected in the Open File Dialog box...",
            MessageText = "Folder Selected"
        };
        alert.RunModal ();
    }
}
```

Ist der entscheidende `[Export ("openDocument:")]`, teilt `NSMenu` , die die **AppDelegate** verfügt über eine Methode `void OpenDialog (NSObject sender)` , die beantwortet die `openDocument:` Aktion.

Weitere Informationen zum Arbeiten mit Menüs finden Sie unter unsere [Menüs](~/mac/user-interface/menu.md) Dokumentation.


## <a name="synchronizing-changes-with-xcode"></a>Synchronisieren der Änderungen mit Xcode

Wenn Sie wieder zu Visual Studio für Mac von Xcode wechseln, werden alle Änderungen, die Sie in Xcode vorgenommen haben in Ihrem Projekt Xamarin.Mac automatisch synchronisiert.

Bei Auswahl der **MainWindow.designer.cs** in der **Lösung Pad** können, um festzustellen, wie unsere Ausgang und Aktion oben im C#-Code wired wurde haben:

[![Synchronisieren der Änderungen mit Xcode](xib-images/sync01.png "Änderungen werden mit Xcode synchronisiert.")](xib-images/sync01-large.png#lightbox)

Beachten Sie, dass wie die beiden Definitionen in der **MainWindow.designer.cs** Datei:

```csharp
[Outlet]
AppKit.NSTextField ClickedLabel { get; set; }

[Action ("ClickedButton:")]
partial void ClickedButton (Foundation.NSObject sender);
```

Richten Sie sich mit den Definitionen in der **MainWindow.h** Datei in Xcode befinden:

```objc
@property (assign) IBOutlet NSTextField *ClickedLabel;
- (IBAction)ClickedButton:(id)sender;
```

Wie Sie sehen können, Visual Studio für Mac überwacht Änderungen an der .h-Datei, und klicken Sie dann automatisch synchronisiert diese Änderungen in der jeweiligen **. designer.cs** Datei, die sie für Ihre Anwendung verfügbar gemacht werden. Möglicherweise auch feststellen, dass **MainWindow.designer.cs** ist eine partielle Klasse so, dass Visual Studio für Mac nicht ändern **MainWindow.cs** würde die, die wir auf die Klasse vorgenommenen Änderungen überschrieben.

Normalerweise werden nie müssen Sie öffnen die **MainWindow.designer.cs** selbst, es wurde hier vorgestellten nur zu Lernzwecken.

> [!IMPORTANT]
> Visual Studio für Mac wird in den meisten Fällen finden Sie in Xcode vorgenommenen Änderungen und dem Projekt Xamarin.Mac zu synchronisieren. Sollte die Synchronisierung nicht automatisch durchgeführt werden – was sehr selten passiert –, wechseln Sie zurück zu Xcode und dann erneut zu Visual Studio für Mac. Dadurch wird normalerweise der Synchronisierungsprozess gestartet.


## <a name="adding-a-new-window-to-a-project"></a>Hinzufügen eines neuen Fensters zu einem Projekt

Abgesehen von der main-Dokumentfenster müssen eine Xamarin.Mac-Anwendung möglicherweise andere Arten von Fenstern, die der Benutzer, z. B. Voreinstellungen oder Inspektor-Fenstern angezeigt. Wenn Sie ein neues Fenster zu Ihrem Projekt hinzufügen sollten Sie immer verwenden die **Kakao-Fenster mit Controller** option, da dadurch der Prozess zum Laden des Fensters aus der .xib einfacher Datei.

Um ein neues Fenster hinzuzufügen, führen Sie folgende Schritte aus:

1. In der **Lösung Pad**mit der rechten Maustaste auf das Projekt, und wählen Sie **hinzufügen** > **neue Datei...** .
2. Wählen Sie im Dialogfeld "neue Datei" **Xamarin.Mac** > **Kakao-Fenster mit Controller**:

    ![Hinzufügen eines neuen Fensters Controllers](xib-images/new01.png "ein neues Fenster Controller hinzufügen")
3. Geben Sie für den **Namen** `PreferencesWindow` ein, und klicken Sie auf **Neu**.
4. Doppelklicken Sie auf die **PreferencesWindow.xib** Datei, um ihn zur Bearbeitung in der Benutzeroberflächen-Generator zu öffnen:

    [![Bearbeiten das Fenster in Xcode](xib-images/new02.png "Bearbeitungsfenster in Xcode")](xib-images/new02-large.png#lightbox)
5. Entwerfen Sie Ihre Schnittstelle:

    [![Entwerfen des Layouts Windows](xib-images/new03.png "Entwerfen des Layouts für Windows")](xib-images/new03-large.png#lightbox)
6. Die Änderungen zu speichern und zurück zu Visual Studio für Mac mit Xcode zu synchronisieren.

Fügen Sie folgenden Code zum **AppDelegate.cs** das neue Fenster angezeigt:

```csharp
[Export("applicationPreferences:")]
void ShowPreferences (NSObject sender)
{
    var preferences = new PreferencesWindowController ();
    preferences.Window.MakeKeyAndOrderFront (this);
}
```

Die `var preferences = new PreferencesWindowController ();` Zeile erstellt eine neue Instanz des Controllers Fenster, die aus der Datei .xib Fenster geladen und vergrößert ihn. Die `preferences.Window.MakeKeyAndOrderFront (this);` Zeile dem Benutzer im neue Fenster angezeigt.

Wenn Sie den Code auszuführen, und wählen Sie die **Einstellungen...**  aus der **Anwendungsmenü**, das Fenster wird angezeigt:

![Ausführen der Beispielapp](xib-images/new04.png "Ausführen der Beispielapp")

Weitere Informationen zum Arbeiten mit Windows in einer Anwendung Xamarin.Mac finden Sie unter unsere [Windows](~/mac/user-interface/window.md) Dokumentation.


## <a name="adding-a-new-view-to-a-project"></a>Hinzufügen einer neuen Ansicht zu einem Projekt

Es gibt Situationen, wenn einfacher Entwurf des Fensters in mehrere, besser verwaltbare .xib Dateien unterteilt werden. Beispielsweise wie das Auslagern aus den Inhalt des Hauptfensters ein, bei der Auswahl von Symbolleistenelement in einem **Fenster Voreinstellungen** oder Austauschen von Inhalten als Antwort auf eine **Quellliste** Auswahl.

Wenn Sie eine neue Ansicht zu Ihrem Projekt hinzufügen sollten Sie immer verwenden die **Kakao Sicht mit Controller** option, da dadurch den Prozess des Ladens der Sicht aus der .xib einfacher Datei.

Um eine neue Sicht hinzuzufügen, führen Sie folgende Schritte aus:

1. In der **Lösung Pad**mit der rechten Maustaste auf das Projekt, und wählen Sie **hinzufügen** > **neue Datei...** .
2. Wählen Sie im Dialogfeld "neue Datei" **Xamarin.Mac** > **Kakao Sicht mit Controller**:

    ![Hinzufügen einer neuen Ansicht](xib-images/view01.png "Hinzufügen einer neuen Ansicht")
3. Geben Sie für den **Namen** `SubviewTable` ein, und klicken Sie auf **Neu**.
4. Doppelklicken Sie auf die **SubviewTable.xib** Datei zur Bearbeitung in der Benutzeroberflächen-Generator zu öffnen und Entwerfen der Benutzeroberfläche:

    [![Entwerfen die neue Ansicht in Xcode](xib-images/view02.png "entwerfen die neue Ansicht in Xcode")](xib-images/view02-large.png#lightbox)
5. Alle erforderlichen Aktionen und Ausgänge verknüpfen.
6. Die Änderungen zu speichern und zurück zu Visual Studio für Mac mit Xcode zu synchronisieren.

Als Nächstes Bearbeiten der **SubviewTable.cs** und fügen Sie den folgenden Code hinzu der **AwakeFromNib** Datei, die die neue Sicht auffüllen, wenn es geladen wird:

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Create the Product Table Data Source and populate it
    var DataSource = new ProductTableDataSource ();
    DataSource.Products.Add (new Product ("Xamarin.iOS", "Allows you to develop native iOS Applications in C#"));
    DataSource.Products.Add (new Product ("Xamarin.Android", "Allows you to develop native Android Applications in C#"));
    DataSource.Products.Add (new Product ("Xamarin.Mac", "Allows you to develop Mac native Applications in C#"));
    DataSource.Sort ("Title", true);

    // Populate the Product Table
    ProductTable.DataSource = DataSource;
    ProductTable.Delegate = new ProductTableDelegate (DataSource);

    // Auto select the first row
    ProductTable.SelectRow (0, false);
}
```

Hinzufügen einer Enumeration dem Projekt zum Nachverfolgen, welche Ansicht derzeit angezeigt wird. Beispielsweise **SubviewType.cs**:

```csharp
public enum SubviewType
{
    None,
    TableView,
    OutlineView,
    ImageView
}
```

Bearbeiten Sie die .xib-Datei des Fensters, die Nutzung der Ansicht und angezeigt werden. Hinzufügen einer **benutzerdefinierte Sicht** , wird die fungieren als Container für die Ansicht, in den Arbeitsspeicher geladen wurden, indem C#-Code und verfügbar gemacht, die mit einer Netzsteckdose namens `ViewContainer`:

[![Erstellen den erforderlichen Ausgang](xib-images/view03.png "erstellen den erforderlichen Ausgang")](xib-images/view03-large.png#lightbox)

Die Änderungen zu speichern und zurück zu Visual Studio für Mac mit Xcode zu synchronisieren.

Als Nächstes bearbeiten die CS-Datei des Fensters, das die neue Ansicht anzeigen werden (z. B. **MainWindow.cs**) und fügen Sie den folgenden Code hinzu:

```csharp
private SubviewType ViewType = SubviewType.None;
private NSViewController SubviewController = null;
private NSView Subview = null;
...

private void DisplaySubview(NSViewController controller, SubviewType type) {

    // Is this view already displayed?
    if (ViewType == type) return;

    // Is there a view already being displayed?
    if (Subview != null) {
        // Yes, remove it from the view
        Subview.RemoveFromSuperview ();

        // Release memory
        Subview = null;
        SubviewController = null;
    }

    // Save values
    ViewType = type;
    SubviewController = controller;
    Subview = controller.View;

    // Define frame and display
    Subview.Frame = new CGRect (0, 0, ViewContainer.Frame.Width, ViewContainer.Frame.Height);
    ViewContainer.AddSubview (Subview);
}
```

Wenn wir eine neue Ansicht angezeigt werden, müssen aus einer .xib-Datei in das Fenster Container geladen wird (die **benutzerdefinierte Sicht** oben hinzugefügten), diese behandelt jede vorhandene Ansicht entfernen, und tauschen Sie ihn für das neue Projekt. Er überprüft, es bereits eine Ansicht angezeigt wird, stehen Ihnen also sie es aus dem Bildschirm entfernt. Als Nächstes dauert die Sicht, die übergeben wurde in der (wie aus einem View-Controller geladen) ändert sich in den Inhaltsbereich passt, und fügt es den Inhalt für die Anzeige hinzu.

Um eine neue Ansicht anzuzeigen, verwenden Sie den folgenden Code:

```csharp
DisplaySubview(new SubviewTableController(), SubviewType.TableView);
```

Dies erstellt eine neue Instanz des Controllers für die neue Ansicht anzeigen, die angezeigt werden, legt den Typ (entsprechend den Angaben von der Enumeration, die dem Projekt hinzugefügt werden soll) und verwendet die `DisplaySubview` Methode hinzugefügt, die Fensterklasse tatsächlich in der Ansicht angezeigt. Zum Beispiel:

[![Ausführen der Beispielapp](xib-images/view04.png "Ausführen der Beispielapp")](xib-images/view04-large.png#lightbox)

Weitere Informationen zum Arbeiten mit Windows in einer Anwendung Xamarin.Mac finden Sie unter unsere [Windows](~/mac/user-interface/window.md) und [Dialoge](~/mac/user-interface/dialog.md) Dokumentation.


## <a name="summary"></a>Zusammenfassung

Dieser Artikel hat eine ausführliche Übersicht über das Arbeiten mit .xib-Dateien in einer Anwendung Xamarin.Mac übernommen. Wir haben gesehen, die verschiedene Typen und Verwendungen von .xib-Dateien, erstellen die Benutzeroberfläche Ihrer Anwendung, wie Dateien erstellen und Verwalten von .xib in Xcode Schnittstelle-Generator und zum Arbeiten mit .xib-Dateien in C#-Code.


## <a name="related-links"></a>Verwandte Links

- [MacImages (Beispiel)](https://developer.xamarin.com/samples/mac/MacImages/)
- [Hello, Mac (Hallo, Mac)](~/mac/get-started/hello-mac.md)
- [Windows](~/mac/user-interface/window.md)
- [Menüs](~/mac/user-interface/menu.md)
- [Dialogfelder](~/mac/user-interface/dialog.md)
- [Arbeiten mit Bildern](~/mac/app-fundamentals/image.md)
- [MacOS von menschlichen Richtlinien zur Benutzeroberfläche](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
