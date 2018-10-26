---
title: XIB-Dateien in Xamarin.Mac
description: Dieser Artikel behandelt die Arbeit mit XIB-Dateien, die in Xcode Interface Builder erstellen und Verwalten von Benutzeroberflächen für eine Xamarin.Mac-Anwendung erstellt.
ms.prod: xamarin
ms.assetid: 6AF3D216-448D-4B2D-9026-74E4FFF5923A
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/14/2017
ms.openlocfilehash: 45eeee745b133646aef0f775bc879fa6a5d867c7
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50109431"
---
# <a name="xib-files-in-xamarinmac"></a>XIB-Dateien in Xamarin.Mac

_Dieser Artikel behandelt die Arbeit mit XIB-Dateien, die in Xcode Interface Builder erstellen und Verwalten von Benutzeroberflächen für eine Xamarin.Mac-Anwendung erstellt._

> [!NOTE]
> Die bevorzugte Methode zum Erstellen einer Benutzeroberfläche für eine Xamarin.Mac-app ist mit Storyboards. Diese Dokumentation ist historische Gründe und für die Arbeit mit älteren Xamarin.Mac-Projekte beibehalten wurde. Weitere Informationen finden Sie unserem [Einführung in Storyboards](~/mac/platform/storyboards/index.md) Dokumentation.

## <a name="overview"></a>Übersicht

Bei der Arbeit mit C# und .NET in einer Xamarin.Mac-Anwendung, Sie haben Zugriff auf die gleichen Elemente der Benutzeroberfläche und tools, die ein Entwickler *Objective-C-* und *Xcode* ist. Da Xamarin.Mac direkt in Xcode integriert ist, können Sie von Xcode _Interface Builder_ erstellen und verwalten Ihren Benutzeroberflächen (oder erstellen sie optional direkt in c#-Code).

Eine XIB-Datei wird von Mac OS verwendet, um Elemente der Benutzeroberfläche Ihrer Anwendung die (z. B. Menüs "," Windows "," Ansichten "," Bezeichnungen "," Text-Felder "), die erstellt und verwaltet werden grafisch in Interface Builder von Xcode zu definieren.

[![Ein Beispiel für die ausgeführte app](xib-images/intro01.png "ein Beispiel für die ausgeführte app")](xib-images/intro01-large.png#lightbox)

In diesem Artikel wird die Grundlagen der Arbeit mit XIB-Dateien in einer Xamarin.Mac-Anwendung behandelt. Es wird dringend empfohlen, dass Sie über arbeiten die [Hallo, Mac](~/mac/get-started/hello-mac.md) Artikel zuerst, wie sie erfahren, wichtige Konzepte und Techniken, die wir in diesem Artikel verwenden.

Empfiehlt es sich um einen Blick auf die [Verfügbarmachen von c#-Klassen / Methoden mit Objective-C](~/mac/internals/how-it-works.md) Teil der [Xamarin.Mac-Interna](~/mac/internals/how-it-works.md) dokumentieren, es wird erläutert, die `Register` und `Export` Attribute verwendet, um Ihre Klassen in c# für Objective-C-Objekte und Benutzeroberfläche Elemente zu verknüpfen.


## <a name="introduction-to-xcode-and-interface-builder"></a>Einführung in Xcode und Interface Builder

Im Rahmen von Xcode hat die Apple ein Tool namens Interface Builder, in dem Sie Ihre Benutzeroberfläche visuell im Designer erstellen können. Xamarin.Mac integriert sich nahtlos mit Interface Builder, die Ihnen ermöglichen, Ihre Benutzeroberfläche mit den gleichen Tools zu erstellen, die Objective-C-Benutzer ausführen.


### <a name="components-of-xcode"></a>Komponenten von Xcode

Wenn Sie eine XIB-Datei in Xcode über Visual Studio für Mac öffnen, wird eine **Projektnavigator** auf der linken Seite die **Schnittstellenhierarchie** und **Schnittstellen-Editor** in der Mitte , und ein **Eigenschaften & Dienstprogramme** Abschnitt auf der rechten Seite:

[![Die Komponenten von der Xcode-UI](xib-images/xcode03.png "die Komponenten von der Xcode-UI")](xib-images/xcode03-large.png#lightbox)

Werfen wir einen Blick auf jedes dieser Abschnitte Xcode wird und wie Sie diese verwendet, um die Benutzeroberfläche für die Xamarin.Mac-Anwendung zu erstellen.


#### <a name="project-navigation"></a>Projektnavigation

Wenn Sie eine XIB-Datei zur Bearbeitung in Xcode öffnen, erstellt Visual Studio für Mac eine Xcode-Projektdatei im Hintergrund, um Xcode Änderungen auszutauschen. Später, wenn Sie zurück zu Visual Studio für Mac in Xcode zu wechseln, werden an diesem Projekt vorgenommenen Änderungen mit dem Xamarin.Mac-Projekt von Visual Studio für Mac synchronisiert

Die **Projektnavigation** Abschnitt können Sie zwischen den aller Dateien zu navigieren, aus denen diese _Shim_ Xcode-Projekt. In der Regel wird nur sichtbar, interessiert die XIB-Dateien in dieser Liste, z. B. **MainMenu.xib** und **MainWindow.xib**.


#### <a name="interface-hierarchy"></a>Schnittstellenhierarchie

Die **Schnittstellenhierarchie** Abschnitt können Sie ganz einfach verschiedene Schlüsseleigenschaften der Benutzeroberfläche zugreifen, z.B. die **Platzhalter** und main **Fenster**. Sie können auch in diesem Abschnitt verwenden, auf die einzelnen Elemente (Ansichten), die besteht die Benutzeroberfläche und anzupassen die Möglichkeit, dass sie geschachtelt sind, indem Sie sie innerhalb der Hierarchie ziehen.


#### <a name="interface-editor"></a>Schnittstellen-editor

Die **Schnittstellen-Editor** Abschnitt finden Sie die Oberfläche auf dem Sie grafisch Layout der Benutzeroberfläche. Ziehen Sie Elemente aus der **Bibliothek** Teil der **Eigenschaften & Dienstprogramme** Abschnitt, um Ihren Entwurf zu erstellen. Während Sie Elemente der Benutzeroberfläche (Ansichten) auf die Entwurfsoberfläche hinzufügen, werden sie zu hinzugefügt werden die **Schnittstellenhierarchie** Abschnitt in der Reihenfolge, sie in der **Schnittstellen-Editor**.


#### <a name="properties--utilities"></a>Eigenschaften & Dienstprogramme

Die **Eigenschaften & Dienstprogramme** Abschnitt ist in zwei Hauptabschnitte, die wir mit dem Sie arbeiten werden unterteilt **Eigenschaften** (auch als Inspektoren bezeichnet) und die **Bibliothek**:

![Die Eigenschaftenanalyse](xib-images/xcode04.png "Eigenschafteninspektor")

Zunächst in diesem Abschnitt nahezu leer ist, jedoch bei der Auswahl eines Elements in der **Schnittstellen-Editor** oder **Schnittstellenhierarchie**, **Eigenschaften** Abschnitt gefüllt Informationen zu diesem Element und Eigenschaften, die Sie anpassen können.

Im Abschnitt **Eigenschaften** gibt es wie in der folgenden Abbildung gezeigt acht verschiedene *Inspektorregisterkarten*:

[![Eine Übersicht aller Inspektoren](xib-images/xcode05.png "eine Übersicht aller Inspektoren")](xib-images/xcode05-large.png#lightbox)

Es gibt die folgenden Registerkarten (von links nach rechts):

-   **Dateiinspektor**: Der Dateiinspektor zeigt Dateiinformationen an, wie z.B. den Dateinamen der XIB-Datei, die bearbeitet wird.
-   **Schnellhilfe**: Die Registerkarte „Schnellhilfe“ bietet Kontexthilfe, je nachdem, was in Xcode ausgewählt ist.
-   **Identitätsinspektor**: Der Identitätsinspektor bietet Informationen zu dem ausgewählten Steuerelement bzw. der ausgewählten Ansicht.
-   **Attributinspektor** – dem Attributinspektor kann verschiedene Attribute der ausgewählten Steuerelements bzw. Ansicht anpassen.
-   **Größeninspektor** – dem Größeninspektor kann Sie die Größe und das Größenänderungsverhalten des dem ausgewählten Steuerelement bzw. Ansicht steuern.
-   **Verbindungsinspektor** : der Verbindungsinspektor zeigt die Verbindungen outlets und Aktionen der ausgewählten Steuerelemente. Outlets und Aktionen untersucht gleich.
-   **Bindungsinspektor** – dem Bindungsinspektor können Sie Steuerelemente so konfigurieren, dass deren Werte automatisch an Datenmodelle gebunden werden.
-   **Ansichteffektinspektor** – dem Ansichteffektinspektor können Sie die Auswirkungen auf die Steuerelemente, z. B. Animationen angeben.

In der **Bibliothek** Abschnitt finden Sie Steuerelemente und Objekte in den Designer, platzieren Sie grafisch erstellen Ihre Benutzeroberfläche:

![Ein Beispiel für die Bibliotheksinspektor](xib-images/xcode06.png "ein Beispiel für im Bibliotheksinspektor festlegen")

Nun, dass Sie mit der Xcode-IDE und Interface Builder vertraut sind, sehen wir uns mit, dass Sie eine Benutzeroberfläche erstellen.


## <a name="creating-and-maintaining-windows-in-xcode"></a>Erstellen und Verwalten von Windows in Xcode

Die bevorzugte Methode zum Erstellen einer Xamarin.Mac-app-Benutzeroberfläche ist mit Storyboards (informieren Sie sich unsere [Einführung in Storyboards](~/mac/platform/storyboards/index.md) Dokumentation für Weitere Informationen) und, folglich neuen Projekten, die im Xamarin.Mac wird gestartet Storyboards werden standardmäßig verwendet.

Wechseln Sie zu einer XIB mit-basierte Benutzeroberfläche, gehen Sie folgendermaßen vor:

1. Öffnen Sie Visual Studio für Mac, und Starten eines neuen Xamarin.Mac-Projekts.
2. In der **Lösungspad**mit der rechten Maustaste auf das Projekt, und wählen Sie **hinzufügen** > **neue Datei...**
3. Wählen Sie **Mac** > **Windows Controller**:

    ![Hinzufügen eines neuen Fensters Controllers](xib-images/setup00.png "Hinzufügen eines neuen Fensters-Controllers")
4. Geben Sie `MainWindow` ein, und klicken Sie auf die **neu** Schaltfläche:

    ![Hinzufügen eines neuen Hauptfensters](xib-images/setup01.png "hinzufügen ein neues Fenster für die Main")
5. Mit der rechten Maustaste auf das Projekt erneut, und wählen Sie **hinzufügen** > **neue Datei...**
6. Wählen Sie **Mac** > **Hauptmenü**:

    ![Hinzufügen einer neuen Hauptmenü](xib-images/setup02.png "Hinzufügen eines neuen Main-Menüs")
7. Lassen Sie den Namen als `MainMenu` , und klicken Sie auf die **neu** Schaltfläche.
8. In der **Lösungspad** wählen Sie die **"Main.Storyboard"** , mit der rechten Maustaste und wählen Sie **entfernen**:

    ![Wählen die Haupt-Storyboard](xib-images/setup03.png "der Haupt-Storyboard auswählen")
9. Das Dialogfeld zu entfernen, klicken Sie auf die **löschen** Schaltfläche:

    ![Bestätigen den Löschvorgang](xib-images/setup04.png "den Löschvorgang bestätigen")
10. In der **Lösungspad**, doppelklicken Sie auf die **"Info.plist"** Datei, die sie für die Bearbeitung zu öffnen.
11. Wählen Sie `MainMenu` aus der **Hauptschnittstelle** Dropdownliste:

    [![Festlegen von im Hauptmenü](xib-images/setup05.png "im Hauptmenü festlegen")](xib-images/setup05-large.png#lightbox)
12. In der **Lösungspad**, doppelklicken Sie auf die **MainMenu.xib** Datei, die sie zur Bearbeitung in Xcode Interface Builder zu öffnen.
13. In der **Bibliotheksinspektor**, Typ `object` in das Suchfeld ein ziehen Sie ein neues **Objekt** auf die Entwurfsoberfläche:

    [![Bearbeiten im Hauptmenü](xib-images/setup06.png "bearbeiten im Hauptmenü")](xib-images/setup06-large.png#lightbox)
14. In der **Identitätsinspektor**, geben Sie `AppDelegate` für die **Klasse**:

    [![Auswählen der App-Delegat](xib-images/setup07.png "Auswählen der App-Delegat")](xib-images/setup07-large.png#lightbox)
15. Wählen Sie **Besitzer der Datei** aus der **Schnittstellenhierarchie**, wechseln Sie zu der **Verbindung Inspektor** und ziehen Sie eine Zeile aus der Delegat, der die `AppDelegate` **Objekt** nur dem Projekt hinzugefügt:

    [![Verbinden der App-Delegat](xib-images/setup08.png "Verbinden der App-Delegat")](xib-images/setup08-large.png#lightbox)
16. Die Änderungen zu speichern und zurück zu Visual Studio für Mac.

Bearbeiten Sie mit der alle diese Änderungen vorgenommen, die **Datei "appdelegate.cs"** Datei, und stellen sie wie folgt aussehen:

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

Jetzt im Hauptfenster der Anwendung definiert ist eine **XIB** Datei automatisch im Projekt enthalten, wenn Sie einen Fenster-Controller hinzufügen. So bearbeiten Sie Ihre Windows-Design, in der **Lösungspad**, Doppelklick der **MainWindow.xib** Datei:

![Auswählen der Datei MainWindow.xib](xib-images/edit01.png "MainWindow.xib auswählen")

Dies öffnet das Fenster Design in Interface Builder von Xcode:

[![Bearbeiten die MainWindow.xib](xib-images/edit02.png "der MainWindow.xib bearbeiten")](xib-images/edit02-large.png#lightbox)


### <a name="standard-window-workflow"></a>Standardfenster-workflow

Für alle Fenster, das Sie erstellen und in Ihre Xamarin.Mac-Anwendung verwenden, ist der Prozess im Wesentlichen identisch:

1. Fügen Sie für neue Windows, die nicht die Standardeinstellung, die dem Projekt automatisch hinzugefügt werden eine neue Fensterdefinition zum Projekt hinzu.
2. Doppelklicken Sie auf die XIB-Datei, um das Fenster-Design für die Bearbeitung in Interface Builder von Xcode zu öffnen.
3. Legen Sie alle erforderlichen Eigenschaften in der **Attributinspektor** und **Größeninspektor**.
4. Ziehen Sie in den Steuerelementen erforderlich, um Ihre Schnittstelle erstellen und konfigurieren sie in der **Attributinspektor**.
5. Verwenden der **Größeninspektor** , behandeln das Ändern der Größe für Ihre UI-Elemente.
6. Machen Sie die Elemente der Benutzeroberfläche des Fensters auf C# Code über Outlets und Aktionen.
7. Speichern Sie die Änderungen zu, und wechseln Sie zurück zu Visual Studio für Mac mit Xcode synchronisiert.


### <a name="designing-a-window-layout"></a>Entwerfen ein Layout des Toolfensters

Der Prozess für das eine Benutzeroberfläche in Interface Builder Layout ist im Grunde für jedes Element, das Sie hinzufügen:

1. Suchen Sie das gewünschte Steuerelement in der **Bibliotheksinspektor** , und ziehen Sie ihn in das **Schnittstellen-Editor** und positionieren Sie sie.
2. Legen Sie alle erforderlichen Eigenschaften in der **Attributinspektor**.
3. Verwenden der **Größeninspektor** , behandeln das Ändern der Größe für Ihre UI-Elemente.
4. Wenn Sie eine benutzerdefinierte Klasse verwenden, legen Sie es in der **Identitätsinspektor**.
5. Machen Sie die UI-Elemente für C# Code über Outlets und Aktionen.
6. Speichern Sie die Änderungen zu, und wechseln Sie zurück zu Visual Studio für Mac mit Xcode synchronisiert.

Beispiel:

1. Ziehen Sie in Xcode eine **Befehlsschaltfläche** aus dem **Bibliotheksbereich**:

    [![Auswählen einer Schaltfläche aus der Bibliothek](xib-images/xcode07.png "eine Schaltfläche auswählt, aus der Bibliothek")](xib-images/xcode07-large.png#lightbox)
2. Fügen Sie die Schaltfläche auf der **Fenster** in die **Schnittstellen-Editor**:

    [![Hinzufügen einer Schaltfläche an das Fenster](xib-images/xcode08.png "Hinzufügen einer Schaltfläche an das Fenster")](xib-images/xcode08-large.png#lightbox)
3. Klicken Sie auf die Eigenschaft **Titel** im **Attributinspektor**, und ändern Sie den Titel der Schaltfläche in `Click Me`:

    ![Die Attribute der Schaltfläche festlegen](xib-images/xcode09.png "die Attribute der Schaltfläche festlegen")
4. Ziehen Sie eine **Bezeichnung** aus dem **Bibliotheksbereich**:

    [![Auswählen einer Bezeichnung in der Bibliothek](xib-images/xcode10.png "Auswählen einer Bezeichnung in der Bibliothek")](xib-images/xcode10-large.png#lightbox)
5. Fügen Sie die Bezeichnung im **Fenster** neben der Schaltfläche im **Schnittstellen-Editor** ein:

    [![Hinzufügen einer Bezeichnung für das Fenster](xib-images/xcode11.png "eine Bezeichnung für das Fenster hinzufügen")](xib-images/xcode11-large.png#lightbox)
6. Klicken Sie auf den rechten Ziehpunkt der Bezeichnung, und ziehen Sie diesen nach rechts, bis der Rand des Fensters erreicht ist:

    [![Ändern der Größe der Bezeichnungsfelds](xib-images/xcode12.png "Ändern der Größe der Bezeichnungsfelds")](xib-images/xcode12-large.png#lightbox)
7. Mit der Bezeichnung ausgewählt die **Schnittstellen-Editor**, wechseln Sie zu der **Größeninspektor**:

    ![Auswählen der Größeninspektor](xib-images/xcode13.png "dem Größeninspektor auswählen")
8. In der **Automatisches Anpassen der Größe im** klicken Sie auf die **Dim Red Klammer** auf der rechten Seite und die **Dim rote, horizontale Pfeil** in der Mitte:

    ![Bearbeiten der Eigenschaften von Automatisches Anpassen der Größe](xib-images/xcode14.png "Bearbeiten der Eigenschaften von Automatisches Anpassen der Größe")
9. Dadurch wird sichergestellt, dass die Bezeichnung gestreckt wird, vergrößert und verkleinert werden, wie in der laufenden Anwendung die Größe des Fensters geändert wird. Die **Red Klammern** und dem oberen und linken Rand der **Automatisches Anpassen der Größe im** Feld Teilen Sie die Bezeichnung, die auf die angegebenen X- und Y Speicherorte hängen bleiben.
10. Speichern Sie die Änderungen an der Benutzeroberfläche

Beim Bewegen von Steuerelementen und Ändern der Größe wurden, Sie sollten aufgefallen, dass Interface Builder Sie hilfreiche Hinweise gibt auf der Grundlage von [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/). Diese Richtlinien können Sie hochwertige Anwendungen zu erstellen, die eine vertraute Aussehen und Verhalten für Mac-Benutzer verfügen sollen.

Wenn Sie, in Suchen der **Schnittstellenhierarchie** Abschnitt, beachten Sie, wie das Layout und die Hierarchie der Elemente, aus denen unser Benutzeroberfläche dargestellt werden:

![Auswählen eines Elements in der Schnittstellenhierarchie](xib-images/xcode15.png "Auswählen eines Elements in der Schnittstellenhierarchie")

Von hier aus können Sie auswählen, Elemente bearbeiten, oder ziehen, um die UI-Elemente neu anordnen, falls erforderlich. Wenn ein Element der Benutzeroberfläche von einem anderen Element verdeckt wird, können Sie es z. B. am Ende der Liste, um erleichtern das oberste Element im Fenster ziehen.

Weitere Informationen zum Arbeiten mit Windows in einer Xamarin.Mac-Anwendung finden Sie unserem [Windows](~/mac/user-interface/window.md) Dokumentation.


## <a name="exposing-ui-elements-to-c-code"></a>Verfügbarmachen von UI-Elemente für C# Code

Nachdem Sie das Aussehen und Verhalten Ihrer Benutzeroberfläche in Interface Builder Anordnung haben, müssen Sie die Elemente der Benutzeroberfläche verfügbar zu machen, damit sie aus zugegriffen werden kann C# Code. Zu diesem Zweck müssen Sie Aktionen und Ergebnisdaten verwenden.


### <a name="setting-a-custom-main-window-controller"></a>Festlegen eines benutzerdefinierten Hauptfenster-Controllers

Zum Erstellen von Outlets und Aktionen, um Benutzeroberflächenelemente für c#-Code verfügbar zu machen können, müssen die Xamarin.Mac-app einen benutzerdefinierten Fenstercontroller verwenden.

Führen Sie folgende Schritte aus:

1. Öffnen Sie die app Storyboard in Interface Builder von Xcode an.
2. Wählen Sie die `NSWindowController` in der Entwurfsoberfläche.
3. Wechseln Sie zu der **Identitätsinspektor** anzuzeigen, und geben Sie `WindowController` als die **Klassenname**:

    [![Bearbeiten den Namen der Klasse](xib-images/windowcontroller01.png "bearbeiten den Namen der Klasse")](xib-images/windowcontroller01-large.png#lightbox)
4. Die Änderungen zu speichern und zurück zu Visual Studio für Mac, um zu synchronisieren.
5. Ein **WindowController.cs** Datei wird hinzugefügt werden, mit Ihrem Projekt in der **Lösungspad** in Visual Studio für Mac:

    ![Dem neuen Klassennamen in Visual Studio für Mac](xib-images/windowcontroller02.png "dem neuen Klassennamen in Visual Studio für Mac")
6. Öffnen Sie erneut das Storyboard in Interface Builder von Xcode.
7. Die **WindowController.h** Datei wird für die Verwendung verfügbar sein:

    [![Die entsprechende h-Datei in Xcode](xib-images/windowcontroller03.png "der entsprechenden h-Datei in Xcode")](xib-images/windowcontroller03-large.png#lightbox)


### <a name="outlets-and-actions"></a>Outlets und Aktionen

Was sind also Outlets und Aktionen? Bei der herkömmlichen .NET-Benutzeroberflächenprogrammierung wird ein Steuerelement auf der Benutzeroberfläche automatisch als Eigenschaft verfügbar gemacht, wenn es hinzugefügt wird. Unter Mac ist dies anders. Wenn Sie ein Steuerelement zu einer Ansicht hinzufügen, wird es dadurch nicht automatisch für Code zugänglich. Der Entwickler muss die UI-Elemente explizit für Code verfügbar machen. Dafür, Apple bietet zwei Optionen:

-  **Outlets**: Outlets sind vergleichbar mit Eigenschaften. Wenn Sie ein Steuerelement mit einem Outlet verbinden, sie wird verfügbar gemacht zu Ihrem Code über eine Eigenschaft, sodass Sie z. B. Ereignishandler tun können, rufen Methoden usw.
-  **Aktionen**: Aktionen sind vergleichbar mit dem Befehlsmuster in WPF. Beispielsweise wenn eine Aktion für ein Steuerelement ausgeführt wird, die z. B. eine Schaltfläche klicken, die das Steuerelement wird automatisch eine Methode im Code aufrufen. Aktionen sind leistungsstark und praktisch, da Sie viele Steuerelemente mit der gleichen Aktion verbinden können.

In Xcode Outlets und Aktionen direkt im Code über hinzugefügt werden *Ziehen von Steuerelementen*. Genauer gesagt bedeutet dies, um ein Outlet oder eine Aktion zu erstellen, die Control-Element auszuwählen und Hinzufügen eines outlets oder Aktion, halten Sie möchten die **Steuerelement** Schaltfläche auf der Tastatur, und ziehen Sie das Steuerelement direkt in Ihren Code.

Für die Xamarin.Mac-Entwickler, bedeutet dies, dass Sie in der Objective-C-Stub-Dateien ziehen, die entsprechen, den C# Datei, in dem Sie die Outlet oder eine Aktion erstellen möchten. Visual Studio für Mac hat eine Datei namens **MainWindow.h** als Teil des Shim-Xcode-Projekt, das sie zum Verwenden von Interface Builder generiert:

[![Ein Beispiel für eine h-Datei in Xcode](xib-images/xcode16.png "ein Beispiel für eine h-Datei in Xcode")](xib-images/xcode16-large.png#lightbox)

Diese Stub-h-Datei spiegelt die **MainWindow.designer.cs** , wird automatisch hinzugefügt, um ein Xamarin.Mac-Projekt, wenn ein neues `NSWindow` erstellt wird. Diese Datei wird verwendet, um die von Interface Builder vorgenommenen Änderungen zu synchronisieren und, in dem wir Ihre Outlets und Aktionen erstellen, damit Elemente der Benutzeroberfläche verfügbar gemacht werden, um C# Code.


#### <a name="adding-an-outlet"></a>Hinzufügen eines outlets

Sehen wir uns mit der ein grundlegendes Verständnis der Outlets und Aktionen, über das Erstellen eines outlets um ein Benutzeroberflächenelement verfügbar zu machen Ihre C# Code.

Führen Sie folgende Schritte aus:

1. Klicken Sie in der rechten oberen Ecke in Xcode auf die Schaltfläche mit den **zwei Kreisen**, um den **Assistenten-Editor** zu öffnen:

    [![Wählen die Assistenten-Editor](xib-images/outlet01.png "Assistant Editor auswählen")](xib-images/outlet01-large.png#lightbox)
2. Xcode wechselt in eine geteilte Ansicht mit dem **Schnittstellen-Editor** auf der einen und dem **Code-Editor** auf der anderen Seite.
3. Beachten Sie, dass Xcode automatisch ausgewählt hat, die die **MainWindowController.m** Datei die **Code-Editor**, was nicht korrekt ist. Wenn Sie, aus der Beschreibung was Outlets und Aktionen vergessen über sind, müssen wir haben die **MainWindow.h** ausgewählten.
4. Am oberen Rand der **Code-Editor** klicken Sie auf die **automatische Verknüpfung** , und wählen Sie die **MainWindow.h** Datei:

    [![Auswählen der richtigen h-Datei](xib-images/outlet02.png "Auswählen der richtigen h-Datei")](xib-images/outlet02-large.png#lightbox)
5. Xcode sollte jetzt die korrekte Datei ausgewählt haben:

    [![Die richtige Datei auswählt](xib-images/outlet03.png "die korrekte Datei ausgewählt")](xib-images/outlet03-large.png#lightbox)
6. **Der letzte Schritt war sehr wichtig!** Wenn Sie die korrekte Datei ausgewählt haben, nicht möglich, erstellen Sie Outlets und Aktionen oder zur Verfügung gestellt, die falsche Klasse in C#!
7. In der **Schnittstellen-Editor**, halten Sie die **Steuerelement** auf der Tastatur gedrückt, und klicken Sie auf die Bezeichnung, die wir oben, die Code-Editor erstellt ziehen direkt unterhalb der `@interface MainWindow : NSWindow { }` Code:

    [![Ziehen Sie zum Erstellen einer neuen Outlets](xib-images/outlet04.png "Ziehen zum Erstellen einer neuen Outlets")](xib-images/outlet04-large.png#lightbox)
8. Es wird ein Dialogfeld geöffnet. Lassen Sie die **Verbindung** Outlet festgelegt, und geben Sie `ClickedLabel` für die **Namen**:

    [![Festlegen der Eigenschaften Outlet](xib-images/outlet05.png "Festlegen der Eigenschaften des Outlets")](xib-images/outlet05-large.png#lightbox)
9. Klicken Sie auf die **Connect** klicken, um die Outlet zu erstellen:

    ![Die abgeschlossene Outlet](xib-images/outlet06.png "des abgeschlossenen Outlets")
10. Speichern Sie die Änderungen in der Datei.


#### <a name="adding-an-action"></a>Hinzufügen einer Aktion

Als Nächstes sehen wir uns das Erstellen einer Aktion zum Verfügbarmachen von einer Benutzerinteraktion mit UI-Element, Ihre C# Code.

Führen Sie folgende Schritte aus:

1. Stellen Sie sicher, dass es nach wie vor in den **Assistant Editor** und **MainWindow.h** Datei wird angezeigt, in der **Code-Editor**.
2. In der **Schnittstellen-Editor**, halten Sie die **Steuerelement** auf der Tastatur gedrückt, und klicken Sie auf die Schaltfläche, die wir oben, die Code-Editor erstellt ziehen direkt unterhalb der `@property (assign) IBOutlet NSTextField *ClickedLabel;` Code:

    [![Ziehen Sie zum Erstellen einer Aktion](xib-images/action01.png "Ziehen zum Erstellen einer Aktion")](xib-images/action01-large.png#lightbox)
3. Ändern der **Verbindung** Typ in Aktion:

    [![Wählen Sie einen neuen Aktionstyp](xib-images/action02.png "einen Aktionstyp auswählen")](xib-images/action02-large.png#lightbox)
4. Geben Sie `ClickedButton` als **Namen** ein:

    [![Konfigurieren die Aktion](xib-images/action03.png "Konfigurieren der Aktion")](xib-images/action03-large.png#lightbox)
5. Klicken Sie auf die **Connect** Schaltfläche, um die Aktion zu erstellen:

    ![Die fertige Aktion](xib-images/action04.png "abgeschlossene Aktionen")
6. Speichern Sie die Änderungen in der Datei.

Mit der Benutzeroberfläche verbunden und verfügbar gemacht werden, um C# programmieren, wechseln Sie zurück zu Visual Studio für Mac und synchronisieren sie die Änderungen aus Xcode und Interface Builder zu ermöglichen.


### <a name="writing-the-code"></a>Das Schreiben des Codes

Die Benutzeroberfläche erstellt und die Elemente der Benutzeroberfläche für einen Überprüfungscode Outlets und Aktionen verfügbar gemacht können Sie schreiben den Code, um das Programm zum Leben zu erwecken. Öffnen Sie z. B. die **MainWindow.cs** Datei zur Bearbeitung durch Doppelklick im der **Lösungspad**:

[![Die Datei MainWindow.cs](xib-images/code01.png "der MainWindow.cs-Datei")](xib-images/code01-large.png#lightbox)

Fügen Sie den folgenden Code hinzu, und die `MainWindow` -Klasse zum Arbeiten mit der Beispiel ab, die Sie soeben erstellt haben:

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

Beachten Sie, dass die `NSLabel` erfolgt C# mit dem direkten Namen, den Sie es in Xcode zugewiesen, wenn Sie die Outlet in Xcode erstellt, in diesem Fall heißt es `ClickedLabel`. Sie können Zugriff auf eine beliebige Methode oder Eigenschaft des verfügbar gemachten Objekts die gleiche Weise möchten, dass alle Normal C# Klasse.

> [!IMPORTANT]
> Sie verwenden müssen `AwakeFromNib`, statt einer anderen Methode wie z. B. `Initialize`, da `AwakeFromNib` heißt _nach_ das Betriebssystem geladen und instanziiert die Benutzeroberfläche aus der XIB-Datei hat. Wenn Sie versuchen auf das Label-Steuerelement, bevor die XIB-Datei wurden vollständig geladen und instanziiert hat, würde ein `NullReferenceException` Fehler, da das Bezeichnungssteuerelement noch nicht erstellt werden sollen.

Fügen Sie die folgende partielle Klasse der `MainWindow` Klasse:

```csharp
partial void ClickedButton (Foundation.NSObject sender) {

    // Update counter and label
    ClickedLabel.StringValue = string.Format("The button has been clicked {0} time{1}.",++numberOfTimesClicked, (numberOfTimesClicked < 2) ? "" : "s");
}
```

Dieser Code fügt an die Aktion, die Sie in Xcode und Interface Builder erstellt und werden jedes Mal, die auf die Schaltfläche, aufgerufen werden.

Einige Elemente der Benutzeroberfläche automatisch über eine integrierte Aktionen, z. B. Elemente in der Menüleiste Standardwert wie z. B. die **öffnen...**  Menüelement (`openDocument:`). In der **Lösungspad**, doppelklicken Sie auf die **Datei "appdelegate.cs"** Datei zur Bearbeitung öffnen, und fügen den folgenden Code unter der `DidFinishLaunching` Methode:

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

Hier die wichtigsten Zeile ist `[Export ("openDocument:")]`, weist `NSMenu` , die die **AppDelegate** verfügt über eine Methode `void OpenDialog (NSObject sender)` , die reagiert, auf die `openDocument:` Aktion.

Weitere Informationen zum Arbeiten mit Menüs, informieren Sie sich unsere [Menüs](~/mac/user-interface/menu.md) Dokumentation.


## <a name="synchronizing-changes-with-xcode"></a>Synchronisieren von Änderungen mit Xcode

Wenn Sie zurück zu Visual Studio für Mac in Xcode wechseln, werden alle Änderungen, die Sie in Xcode vorgenommenen automatisch mit dem Xamarin.Mac-Projekt synchronisiert werden.

Bei Auswahl der **MainWindow.designer.cs** in die **Lösungspad** Sie werden sehen, wie unsere Outlet und Aktion haben in vernetzt wurden unserem C# Code:

[![Synchronisieren von Änderungen mit Xcode](xib-images/sync01.png "Änderungen mit Xcode synchronisieren")](xib-images/sync01-large.png#lightbox)

Beachten Sie, dass wie die beiden Definitionen aus dem **MainWindow.designer.cs** Datei:

```csharp
[Outlet]
AppKit.NSTextField ClickedLabel { get; set; }

[Action ("ClickedButton:")]
partial void ClickedButton (Foundation.NSObject sender);
```

Richten Sie sich mit den Definitionen in der **MainWindow.h** -Datei in Xcode:

```objc
@property (assign) IBOutlet NSTextField *ClickedLabel;
- (IBAction)ClickedButton:(id)sender;
```

Wie Sie sehen können, Visual Studio für Mac Lauscht auf Änderungen an der h-Datei, und klicken Sie dann synchronisiert diese automatisch der entsprechenden **. designer.cs** Datei, die sie für Ihre Anwendung verfügbar zu machen. Sie können auch feststellen, dass **MainWindow.designer.cs** wird von eine partielle Klasse sind, damit Visual Studio für Mac nicht so ändern Sie **MainWindow.cs** überschreibt die Änderungen, die wir auf die Klasse vorgenommen haben.

Sie normalerweise müssen nie zum Öffnen der **MainWindow.designer.cs** selbst, es wurde hier vorgestellten zum besseren.

> [!IMPORTANT]
> In den meisten Fällen Visual Studio für Mac automatisch finden Sie in Xcode vorgenommenen Änderungen und synchronisiert diese mit dem Xamarin.Mac-Projekt. Sollte die Synchronisierung nicht automatisch durchgeführt werden – was sehr selten passiert –, wechseln Sie zurück zu Xcode und dann erneut zu Visual Studio für Mac. Dadurch wird normalerweise der Synchronisierungsprozess gestartet.


## <a name="adding-a-new-window-to-a-project"></a>Hinzufügen von einem neuen Fenster zu einem Projekt

Abgesehen von der wichtigsten Dokumentfenster müssen eine Xamarin.Mac-Anwendung möglicherweise andere Typen von Windows für den Benutzer, z. B. Einstellungen oder Inspektor-Fenster angezeigt. Wenn Sie ein neues Fenster zu Ihrem Projekt hinzufügen sollten Sie immer eine verwenden die **Cocoa-Fenster mit Controller** option, da dies beim Laden des Fensters aus der XIB einfacher Datei ist.

Um ein neues Fenster hinzuzufügen, führen Sie folgende Schritte aus:

1. In der **Lösungspad**mit der rechten Maustaste auf das Projekt, und wählen Sie **hinzufügen** > **neue Datei...** .
2. Wählen Sie in das Dialogfeld "neue Datei" **Xamarin.Mac** > **Cocoa-Fenster mit Controller**:

    ![Hinzufügen eines neuen Fensters Controllers](xib-images/new01.png "Hinzufügen eines neuen Fensters-Controllers")
3. Geben Sie für den **Namen** `PreferencesWindow` ein, und klicken Sie auf **Neu**.
4. Doppelklicken Sie auf die **PreferencesWindow.xib** Datei, die sie zur Bearbeitung in Interface Builder zu öffnen:

    [![Bearbeiten das Fenster in Xcode](xib-images/new02.png "bearbeiten das Fenster in Xcode")](xib-images/new02-large.png#lightbox)
5. Entwerfen Sie Ihre Schnittstelle:

    [![Entwerfen des Layouts Windows](xib-images/new03.png "Entwerfen des Layouts für Windows")](xib-images/new03-large.png#lightbox)
6. Die Änderungen zu speichern und zurück zu Visual Studio für Mac mit Xcode synchronisiert.

Fügen Sie den folgenden Code **Datei "appdelegate.cs"** Ihr neue Fenster angezeigt:

```csharp
[Export("applicationPreferences:")]
void ShowPreferences (NSObject sender)
{
    var preferences = new PreferencesWindowController ();
    preferences.Window.MakeKeyAndOrderFront (this);
}
```

Die `var preferences = new PreferencesWindowController ();` Zeile erstellt eine neue Instanz des Controllers Fenster, das Fenster aus der XIB-Datei geladen und vergrößert ihn. Die `preferences.Window.MakeKeyAndOrderFront (this);` Zeile dem Benutzer im neue Fenster angezeigt.

Wenn Sie den Code auszuführen, und wählen Sie die **Einstellungen...**  aus der **Anwendungsmenü**, das Fenster wird angezeigt:

![Ausführen der Beispielapp](xib-images/new04.png "Ausführen der Beispielapp")

Weitere Informationen zum Arbeiten mit Windows in einer Xamarin.Mac-Anwendung finden Sie unserem [Windows](~/mac/user-interface/window.md) Dokumentation.


## <a name="adding-a-new-view-to-a-project"></a>Hinzufügen einer neuen Ansicht zu einem Projekt

Es gibt Zeiten, in denen es einfacher, die von Fenstern Entwurf in mehrere, besser verwaltbare XIB-Dateien zu unterteilen. Beispielsweise, wie Sie den Inhalt des Hauptfensters wechseln, bei der Auswahl eines Symbolleistenelements in einer **Fenster "Einstellungen"** oder Austauschen von Inhalten in Reaktion auf eine **Quellliste** Auswahl.

Wenn Sie eine neue Ansicht zu Ihrem Projekt hinzufügen sollten Sie immer verwenden die **Cocoa-Ansicht mit Controller** option, da dies beim Laden der Ansicht aus der XIB einfacher Datei ist.

Um eine neue Sicht hinzuzufügen, führen Sie folgende Schritte aus:

1. In der **Lösungspad**mit der rechten Maustaste auf das Projekt, und wählen Sie **hinzufügen** > **neue Datei...** .
2. Wählen Sie in das Dialogfeld "neue Datei" **Xamarin.Mac** > **Cocoa-Ansicht mit Controller**:

    ![Hinzufügen einer neuen Ansicht](xib-images/view01.png "Hinzufügen einer neuen Ansicht")
3. Geben Sie für den **Namen** `SubviewTable` ein, und klicken Sie auf **Neu**.
4. Doppelklicken Sie auf die **SubviewTable.xib** Datei zur Bearbeitung in Interface Builder öffnen, und Entwerfen der Benutzeroberfläche:

    [![Entwerfen die neue Ansicht in Xcode](xib-images/view02.png "entwerfen die neue Ansicht in Xcode")](xib-images/view02-large.png#lightbox)
5. Verknüpfen Sie alle erforderlichen Aktionen und Ergebnisdaten.
6. Die Änderungen zu speichern und zurück zu Visual Studio für Mac mit Xcode synchronisiert.

Als Nächstes bearbeiten Sie die **SubviewTable.cs** und fügen Sie den folgenden Code der **"AwakeFromNib"** Datei, die die neue Ansicht zu füllen, wenn es geladen wird:

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

Hinzufügen einer Enumeration auf das Projekt nachverfolgen, welche Ansicht aktuell angezeigt wird. Z. B. **SubviewType.cs**:

```csharp
public enum SubviewType
{
    None,
    TableView,
    OutlineView,
    ImageView
}
```

Bearbeiten Sie die XIB-Datei des Fensters, das die Ansicht nutzen und anzuzeigen. Hinzufügen einer **benutzerdefinierte Sicht** , wird die fungieren als Container für die Ansicht, die im Speicher geladen wird C# code, und machen ihn eines outlets namens verfügbar `ViewContainer`:

[![Erstellen die erforderlichen Outlet](xib-images/view03.png "Erstellen des erforderlichen Outlets")](xib-images/view03-large.png#lightbox)

Die Änderungen zu speichern und zurück zu Visual Studio für Mac mit Xcode synchronisiert.

Als Nächstes bearbeiten Sie die CS-Datei des Fensters, das die neue Ansicht anzeigen lassen (z. B. **MainWindow.cs**), und fügen Sie den folgenden Code hinzu:

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

Wenn wir eine neue Ansicht anzeigen müssen aus einer XIB-Datei in das Fenster des Containers geladen (die **benutzerdefinierte Sicht** oben hinzugefügt), dieser Code behandelt jede vorhandene Ansicht entfernen, und tauschen Sie ihn für die neue Ressourcengruppe ein. Es sieht aus, um dieses zu sehen Sie eine Ansicht angezeigt wird, bereits verfügen, wenn also diese vom Bildschirm entfernt. Als Nächstes benötigt die Ansicht, die übergeben wurde (wie aus einem Ansichtscontroller geladen) ändert sich im Bereich Inhalt anpassen und den Inhalt für die Anzeige hinzugefügt.

Um eine neue Ansicht anzuzeigen, verwenden Sie den folgenden Code:

```csharp
DisplaySubview(new SubviewTableController(), SubviewType.TableView);
```

Dies erstellt eine neue Instanz des Ansichtscontrollers für die neue Ansicht, die angezeigt werden, legt den Typ (wie durch die Enumeration, die dem Projekt hinzugefügt) und verwendet die `DisplaySubview` Methode tatsächlich anzeigen die Ansicht des Fensters-Klasse hinzugefügt. Zum Beispiel:

[![Ausführen der Beispielapp](xib-images/view04.png "Ausführen der Beispielapp")](xib-images/view04-large.png#lightbox)

Weitere Informationen zum Arbeiten mit Windows in einer Xamarin.Mac-Anwendung finden Sie unserem [Windows](~/mac/user-interface/window.md) und [Dialogfelder](~/mac/user-interface/dialog.md) Dokumentation.


## <a name="summary"></a>Zusammenfassung

Dieser Artikel hat es sich um einen detaillierten Einblick in die Arbeit mit XIB-Dateien in einer Xamarin.Mac-Anwendung geführt. Wir haben gesehen, die verschiedene Arten und XIB-Dateien verwendet, auf der Benutzeroberfläche Ihrer Anwendungsverzeichnis zu erstellen, wie zum Erstellen und Verwalten von XIB-Dateien in Xcode Interface-Generator und das Arbeiten mit XIB-Dateien in C# Code.


## <a name="related-links"></a>Verwandte Links

- [MacImages (Beispiel)](https://developer.xamarin.com/samples/mac/MacImages/)
- [Hello, Mac (Hallo, Mac)](~/mac/get-started/hello-mac.md)
- [Windows](~/mac/user-interface/window.md)
- [Menüs](~/mac/user-interface/menu.md)
- [Dialogfelder](~/mac/user-interface/dialog.md)
- [Arbeiten mit Bildern](~/mac/app-fundamentals/image.md)
- [MacOS Human Interface Guidelines](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
