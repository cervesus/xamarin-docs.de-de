---
title: XIb-Dateien in xamarin. Mac
description: In diesem Artikel wird das Arbeiten mit XIb-Dateien erläutert, die in Xcode-Interface Builder erstellt wurden, um Benutzeroberflächen für eine xamarin. Mac-Anwendung zu erstellen und zu verwalten.
ms.prod: xamarin
ms.assetid: 6AF3D216-448D-4B2D-9026-74E4FFF5923A
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 03/14/2017
ms.openlocfilehash: 6d40dd3cc994ef8ab21ffb9658f226d36cd97913
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73021771"
---
# <a name="xib-files-in-xamarinmac"></a>XIb-Dateien in xamarin. Mac

_In diesem Artikel wird das Arbeiten mit XIb-Dateien erläutert, die in Xcode-Interface Builder erstellt wurden, um Benutzeroberflächen für eine xamarin. Mac-Anwendung zu erstellen und zu verwalten._

> [!NOTE]
> Die bevorzugte Methode zum Erstellen einer Benutzeroberfläche für eine xamarin. Mac-app ist die Verwendung von Storyboards. Diese Dokumentation wurde aus historischen Gründen und für die Arbeit mit älteren xamarin. Mac-Projekten noch nicht ausgeführt. Weitere Informationen finden Sie [in unserer Einführung in die Storyboards](~/mac/platform/storyboards/index.md) -Dokumentation.

## <a name="overview"></a>Übersicht

Wenn Sie mit C# und .net in einer xamarin. Mac-Anwendung arbeiten, haben Sie Zugriff auf dieselben Elemente und Tools wie die Benutzeroberfläche, die ein Entwickler in *Ziel-C* und *Xcode* verwendet. Da xamarin. Mac direkt in Xcode integriert ist, können Sie die _Interface Builder_ von Xcode verwenden, um die Benutzeroberflächen zu erstellen und zu verwalten (oder C# Sie optional direkt im Code zu erstellen).

Eine XIb-Datei wird von macOS zum Definieren von Elementen der Benutzeroberfläche Ihrer Anwendung (z. b. Menüs, Fenster, Sichten, Bezeichnungen, Text Felder) verwendet, die in der Interface Builder von Xcode grafisch erstellt und verwaltet werden.

[![Ein Beispiel für die laufende App](xib-images/intro01.png "Ein Beispiel für die laufende App")](xib-images/intro01-large.png#lightbox)

In diesem Artikel werden die Grundlagen der Arbeit mit XIb-Dateien in einer xamarin. Mac-Anwendung behandelt. Es wird dringend empfohlen, dass Sie zuerst mit dem Artikel [Hello, Mac](~/mac/get-started/hello-mac.md) arbeiten, da er wichtige Konzepte und Techniken behandelt, die wir in diesem Artikel verwenden werden.

Sie können sich auch den Abschnitt verfügbar machen von [ C# Klassen/Methoden zu "Ziel-c](~/mac/internals/how-it-works.md) " im Dokument " [xamarin. Mac](~/mac/internals/how-it-works.md) " ansehen. darin werden die `Register` und `Export` Attribute erläutert, die zum Verknüpfen der C# Klassen mit "Ziel-c" verwendet werden. Objekte und UI-Elemente.

## <a name="introduction-to-xcode-and-interface-builder"></a>Einführung in Xcode und Interface Builder

Als Teil von Xcode hat Apple ein Tool namens Interface Builder erstellt, mit dem Sie die Benutzeroberfläche in einem Designer visuell erstellen können. Xamarin. Mac ist nahtlos in Interface Builder integriert, sodass Sie Ihre Benutzeroberfläche mit denselben Tools erstellen können, die von den Ziel-C-Benutzern Unternehmen werden.

### <a name="components-of-xcode"></a>Komponenten von Xcode

Wenn Sie eine XIb-Datei in Xcode aus Visual Studio für Mac öffnen, wird Sie mit einem **Projekt Navigator** auf der linken Seite, der **Schnittstellen Hierarchie** und dem Schnittstellen- **Editor** in der Mitte und den **Eigenschaften &** hilfsprogrammbereich auf der rechten Seite geöffnet:

[![Die Komponenten der Xcode-Benutzeroberfläche](xib-images/xcode03.png "Die Komponenten der Xcode-Benutzeroberfläche")](xib-images/xcode03-large.png#lightbox)

Sehen wir uns einmal an, was die einzelnen Xcode-Abschnitte tun und wie Sie Sie verwenden können, um die Schnittstelle für Ihre xamarin. Mac-Anwendung zu erstellen.

#### <a name="project-navigation"></a>Projekt Navigation

Wenn Sie eine XIb-Datei zur Bearbeitung in Xcode öffnen, erstellt Visual Studio für Mac im Hintergrund eine Xcode-Projektdatei, um Änderungen zwischen sich selbst und Xcode zu kommunizieren. Wenn Sie später von Xcode zurück zu Visual Studio für Mac wechseln, werden alle an diesem Projekt vorgenommenen Änderungen mithilfe Visual Studio für Mac mit Ihrem xamarin. Mac-Projekt synchronisiert.

Im Abschnitt **Project Navigation (Projekt Navigation** ) können Sie zwischen allen Dateien navigieren, aus denen dieses _Shim_ -Xcode-Projekt besteht. In der Regel interessieren Sie sich nur für die XIb-Dateien in dieser Liste, z. b. " **MainMenu. XIb** " und " **MainWindow. XIb**".

#### <a name="interface-hierarchy"></a>Schnittstellen Hierarchie

Der Abschnitt **Schnittstellen Hierarchie** ermöglicht den einfachen Zugriff auf verschiedene Schlüsseleigenschaften der Benutzeroberfläche, z. b. auf **Platzhalter** und Haupt **Fenster**. Mithilfe dieses Abschnitts können Sie auch auf die einzelnen Elemente (Ansichten) zugreifen, aus denen Ihre Benutzeroberfläche besteht, und die Art und Weise anpassen, wie Sie geschachtelt werden, indem Sie Sie in der Hierarchie ziehen.

#### <a name="interface-editor"></a>Schnittstellen-Editor

Der **Schnitt** stellen-Editor-Abschnitt enthält die Oberfläche, auf der Sie Ihre Benutzeroberfläche grafisch darstellen. Sie ziehen Elemente aus dem Abschnitt **Library** des Abschnitts **Properties & Utilities** , um den Entwurf zu erstellen. Wenn Sie der Entwurfs Oberfläche Benutzeroberflächen Elemente (Ansichten) hinzufügen, werden diese dem **Schnittstellen Hierarchie** Abschnitt in der Reihenfolge hinzugefügt, in der Sie im Schnittstellen- **Editor**angezeigt werden.

#### <a name="properties--utilities"></a>Eigenschaften & Hilfsprogramme

Der Abschnitt **Properties & Utilities** ist in zwei Hauptabschnitte unterteilt, mit denen wir arbeiten werden, **Eigenschaften** (auch als Inspektoren bezeichnet) und die **Bibliothek**:

![Der Eigenschaften Inspektor](xib-images/xcode04.png "Der Eigenschaften Inspektor")

Anfänglich ist dieser Abschnitt fast leer. Wenn Sie jedoch ein Element im Schnittstellen- **Editor** oder in der **Schnittstellen Hierarchie**auswählen, wird der Abschnitt " **Properties** " mit Informationen über das angegebene Element und die Eigenschaften aufgefüllt, die Sie Einstellung.

Im Abschnitt **Eigenschaften** gibt es wie in der folgenden Abbildung gezeigt acht verschiedene *Inspektorregisterkarten*:

[![Übersicht über alle Inspektoren](xib-images/xcode05.png "Übersicht über alle Inspektoren")](xib-images/xcode05-large.png#lightbox)

Es gibt die folgenden Registerkarten (von links nach rechts):

- **Dateiinspektor**: Der Dateiinspektor zeigt Dateiinformationen an, wie z.B. den Dateinamen der XIB-Datei, die bearbeitet wird.
- **Schnellhilfe**: Die Registerkarte „Schnellhilfe“ bietet Kontexthilfe, je nachdem, was in Xcode ausgewählt ist.
- **Identitätsinspektor**: Der Identitätsinspektor bietet Informationen zu dem ausgewählten Steuerelement bzw. der ausgewählten Ansicht.
- **Attribut Inspektor** – mit dem Attribut Inspektor können Sie verschiedene Attribute des ausgewählten Steuer Elements/der ausgewählten Ansicht anpassen.
- **Größen Inspektor** – mit dem Größen Inspektor können Sie die Größe und das Größenänderung des ausgewählten Steuer Elements/der ausgewählten Ansicht steuern.
- **Verbindungs Inspektor** – der Verbindungs Inspektor zeigt die Ausgangs-und Aktions Verbindungen der ausgewählten Steuerelemente an. Wir untersuchen die Outlets und Aktionen in Kürze.
- **Bindungs Inspektor** – mit dem Bindungs Inspektor können Sie Steuerelemente so konfigurieren, dass ihre Werte automatisch an Datenmodelle gebunden werden.
- **View Effects Inspector** – mit dem View Effects Inspector können Sie Auswirkungen auf die Steuerelemente, z. b. Animationen, angeben.

Im Abschnitt **Library** finden Sie Steuerelemente und Objekte, die Sie im Designer platzieren können, um Ihre Benutzeroberfläche grafisch zu erstellen:

![Ein Beispiel für den Library Inspector](xib-images/xcode06.png "Ein Beispiel für den Library Inspector")

Nun, da Sie mit der Xcode-IDE und der Interface Builder vertraut sind, betrachten wir die Verwendung zum Erstellen einer Benutzeroberfläche.

## <a name="creating-and-maintaining-windows-in-xcode"></a>Erstellen und Verwalten von Fenstern in Xcode

Die bevorzugte Methode zum Erstellen der Benutzeroberfläche einer xamarin. Mac-app ist die Verwendung von Storyboards (Weitere Informationen finden Sie in unserer [Einführung in die Storyboards](~/mac/platform/storyboards/index.md) -Dokumentation). Folglich werden alle neuen Projekte, die in xamarin. Mac gestartet werden, Storyboards von vorgegebene.

Gehen Sie folgendermaßen vor, um zur Verwendung einer XIb-basierten Benutzeroberfläche zu wechseln:

1. Öffnen Sie Visual Studio für Mac, und starten Sie ein neues xamarin. Mac-Projekt.
2. Klicken Sie im **Lösungspad**mit der rechten Maustaste auf das Projekt, und wählen Sie  > **neue Datei** **Hinzufügen** ... aus.
3. Wählen Sie **Mac**  > **Windows-Controller**aus:

    ![Hinzufügen eines neuen Fenster Controllers](xib-images/setup00.png "Hinzufügen eines neuen Fenster Controllers")
4. Geben Sie `MainWindow` als Namen ein, und klicken Sie auf die Schaltfläche **neu** :

    ![Hinzufügen eines neuen Hauptfensters](xib-images/setup01.png "Hinzufügen eines neuen Hauptfensters")
5. Klicken Sie erneut mit der rechten Maustaste auf das Projekt, und wählen Sie  > **neue Datei** **Hinzufügen** ...
6. Wählen Sie **Mac**  > **Hauptmenü**aus:

    ![Hinzufügen eines neuen Hauptmenüs](xib-images/setup02.png "Hinzufügen eines neuen Hauptmenüs")
7. Belassen Sie den Namen `MainMenu`, und klicken Sie auf die Schaltfläche **neu** .
8. Wählen Sie im **Lösungspad** die Datei **Main. Storyboard** aus, klicken Sie mit der rechten Maustaste, und wählen Sie **Entfernen**aus:

    ![Auswählen des Haupt Storyboards](xib-images/setup03.png "Auswählen des Haupt Storyboards")
9. Klicken Sie im Dialog Feld entfernen auf die Schaltfläche **Löschen** :

    ![Bestätigen des Löschvorgangs](xib-images/setup04.png "Bestätigen des Löschvorgangs")
10. Doppelklicken Sie im **Lösungspad**auf die Datei **Info. plist** , um Sie zur Bearbeitung zu öffnen.
11. Wählen Sie `MainMenu` aus der Dropdown Liste **Hauptschnittstelle** aus:

    [![Festlegen des Hauptmenü](xib-images/setup05.png "Festlegen des Hauptmenü")](xib-images/setup05-large.png#lightbox)
12. Doppelklicken Sie im **Lösungspad**auf die Datei " **MainMenu. XIb** ", um Sie für die Bearbeitung in der Interface Builder von Xcode zu öffnen.
13. Geben Sie im **Bibliotheks Inspektor**`object` in das Suchfeld ein, und ziehen Sie dann ein neues **Objekt** auf die Entwurfs Oberfläche:

    [![Bearbeiten des Hauptmenü](xib-images/setup06.png "Bearbeiten des Hauptmenü")](xib-images/setup06-large.png#lightbox)
14. Geben Sie im **Identitäts Inspektor**`AppDelegate` für die- **Klasse**ein:

    [![Auswählen des App-Delegaten](xib-images/setup07.png "Auswählen des App-Delegaten")](xib-images/setup07-large.png#lightbox)
15. Wählen Sie den **Dateibesitzer** aus der **Schnittstellen Hierarchie**aus, wechseln Sie zum **Verbindungs Inspektor** , und ziehen Sie eine Linie vom Delegaten auf das `AppDelegate` **Objekt** , das dem Projekt soeben hinzugefügt wurde:

    [![Verbinden des App-Delegaten](xib-images/setup08.png "Verbinden des App-Delegaten")](xib-images/setup08-large.png#lightbox)
16. Speichern Sie die Änderungen, und kehren Sie zu Visual Studio für Mac zurück.

Wenn alle diese Änderungen vorhanden sind, bearbeiten Sie die Datei **AppDelegate.cs** , und führen Sie Sie wie folgt aus:

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

Nun wird das Hauptfenster der app in einer **XIb** -Datei definiert, die beim Hinzufügen eines Fenster Controllers automatisch im Projekt enthalten ist. Um den Windows-Entwurf zu bearbeiten, doppelklicken Sie im **Lösungspad**auf die Datei " **MainWindow. XIb** ":

![Auswählen der Datei "MainWindow. XIb"](xib-images/edit01.png "Auswählen der Datei "MainWindow. XIb"")

Dadurch wird das Fensterdesign in der Interface Builder von Xcode geöffnet:

[![Bearbeiten der Datei "MainWindow. XIb"](xib-images/edit02.png "Bearbeiten der Datei "MainWindow. XIb"")](xib-images/edit02-large.png#lightbox)

### <a name="standard-window-workflow"></a>Standard Fenster Workflow

Für jedes Fenster, das Sie in ihrer xamarin. Mac-Anwendung erstellen und mit Ihnen arbeiten, ist der Prozess im Grunde das gleiche:

1. Fügen Sie für neue Fenster, die dem Projekt nicht automatisch hinzugefügt werden, eine neue Fenster Definition zum Projekt hinzu.
2. Doppelklicken Sie auf die XIb-Datei, um den Fenster Entwurf für die Bearbeitung in der Interface Builder von Xcode zu öffnen.
3. Legen Sie alle erforderlichen Fenster Eigenschaften in der **Attribut** Prüfung und der **Größe Inspector**fest.
4. Ziehen Sie die Steuerelemente, die zum Erstellen der Schnittstelle erforderlich sind, und konfigurieren Sie Sie im **Attribut Inspektor**.
5. Verwenden Sie den **Größen Inspektor** , um die Größenänderung für Ihre Benutzeroberflächen Elemente zu verarbeiten.
6. Machen Sie die Benutzeroberflächen Elemente des C# Fensters für den Code über Outlets und Aktionen verfügbar.
7. Speichern Sie die Änderungen, und wechseln Sie zurück zu Visual Studio für Mac, um mit Xcode zu synchronisieren.

### <a name="designing-a-window-layout"></a>Entwerfen eines Fensterlayouts

Der Vorgang zum Festlegen einer Benutzeroberfläche im Schnittstellen Generator ist im Grunde für jedes Element identisch, das Sie hinzufügen:

1. Suchen Sie das gewünschte Steuerelement im **Bibliotheks Inspektor** , ziehen Sie es in den Schnittstellen- **Editor** , und positionieren Sie es.
2. Legen Sie alle erforderlichen Fenster Eigenschaften im **Attribut Inspektor**fest.
3. Verwenden Sie den **Größen Inspektor** , um die Größenänderung für Ihre Benutzeroberflächen Elemente zu verarbeiten.
4. Wenn Sie eine benutzerdefinierte Klasse verwenden, legen Sie Sie im **Identitäts Inspektor**fest.
5. Machen Sie die Benutzeroberflächen C# Elemente für Code über Outlets und Aktionen verfügbar.
6. Speichern Sie die Änderungen, und wechseln Sie zurück zu Visual Studio für Mac, um mit Xcode zu synchronisieren.

Beispiel:

1. Ziehen Sie in Xcode eine **Befehlsschaltfläche** aus dem **Bibliotheksbereich**:

    [![Auswählen einer Schaltfläche aus der Bibliothek](xib-images/xcode07.png "Auswählen einer Schaltfläche aus der Bibliothek")](xib-images/xcode07-large.png#lightbox)
2. Legen Sie die Schaltfläche auf dem **Fenster** im Schnittstellen- **Editor**ab:

    [![Hinzufügen einer Schaltfläche zum Fenster](xib-images/xcode08.png "Hinzufügen einer Schaltfläche zum Fenster")](xib-images/xcode08-large.png#lightbox)
3. Klicken Sie auf die Eigenschaft **Titel** im **Attributinspektor**, und ändern Sie den Titel der Schaltfläche in `Click Me`:

    ![Festlegen der Schaltflächen Attribute](xib-images/xcode09.png "Festlegen der Schaltflächen Attribute")
4. Ziehen Sie eine **Bezeichnung** aus dem **Bibliotheksbereich**:

    [![Auswählen einer Bezeichnung in der Bibliothek](xib-images/xcode10.png "Auswählen einer Bezeichnung in der Bibliothek")](xib-images/xcode10-large.png#lightbox)
5. Fügen Sie die Bezeichnung im **Fenster** neben der Schaltfläche im **Schnittstellen-Editor** ein:

    [![Hinzufügen einer Bezeichnung zum Fenster](xib-images/xcode11.png "Hinzufügen einer Bezeichnung zum Fenster")](xib-images/xcode11-large.png#lightbox)
6. Klicken Sie auf den rechten Ziehpunkt der Bezeichnung, und ziehen Sie diesen nach rechts, bis der Rand des Fensters erreicht ist:

    [![Ändern der Größe der Bezeichnung](xib-images/xcode12.png "Ändern der Größe der Bezeichnung")](xib-images/xcode12-large.png#lightbox)
7. Wechseln Sie im Schnittstellen- **Editor**, während die Bezeichnung noch ausgewählt ist, in den **Größen Inspektor**:

    ![Auswählen des Größen Inspektors](xib-images/xcode13.png "Auswählen des Größen Inspektors")
8. Klicken Sie im **Feld Automatische Größen** Anpassung auf die **Dim rote eckige Klammer** rechts und den Pfeil mit dem **Pfeil "dunkel** " in der Mitte:

    ![Bearbeiten der Eigenschaften für die automatische Größenanpassung](xib-images/xcode14.png "Bearbeiten der Eigenschaften für die automatische Größenanpassung")
9. Dadurch wird sichergestellt, dass die Bezeichnung gestreckt wird, um vergrößert und verkleinert zu werden, wenn die Größe des Fensters in der laufenden Anwendung geändert wird. Die **Rote eckige Klammer** und die obere und linke Ecke des Felds Feld für die **Automatische Größen** Anpassung geben an, dass die Bezeichnung an den angegebenen X-und Y-Speicherorten hängen bleibt.
10. Speichern Sie die Änderungen an der Benutzeroberfläche.

Wenn Sie die Größe der Steuerelemente geändert haben, sollten Sie bemerkt haben, dass Interface Builder Ihnen hilfreiche Snap-Ins bietet, die auf [OS X-Richtlinien für die Benutzeroberfläche](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)basieren. Diese Richtlinien helfen Ihnen, qualitativ hochwertige Anwendungen zu erstellen, die ein bekanntes Aussehen und Gefühl für Mac-Benutzer haben.

Wenn Sie im Abschnitt **Schnittstellen Hierarchie** nachschauen, sehen Sie, wie das Layout und die Hierarchie der Elemente, aus denen die Benutzeroberfläche besteht, angezeigt werden:

![Auswählen eines Elements in der Schnittstellen Hierarchie](xib-images/xcode15.png "Auswählen eines Elements in der Schnittstellen Hierarchie")

Von hier aus können Sie Elemente auswählen, die Sie bearbeiten oder ziehen, um Benutzeroberflächen Elemente bei Bedarf neu anzuordnen. Wenn z. b. ein Benutzeroberflächen Element von einem anderen Element abgedeckt wird, können Sie es an das Ende der Liste ziehen, um es zum obersten Element im Fenster zu machen.

Weitere Informationen zum Arbeiten mit Windows in einer xamarin. Mac-Anwendung finden Sie in unserer [Windows](~/mac/user-interface/window.md) -Dokumentation.

## <a name="exposing-ui-elements-to-c-code"></a>Verfügbar machen von UI C# -Elementen für Code

Nachdem Sie das Erscheinungsbild der Benutzeroberfläche in Interface Builder festgelegt haben, müssen Sie die Elemente der Benutzeroberfläche verfügbar machen, damit Sie über C# Code darauf zugreifen können. Zu diesem Zweck verwenden Sie Aktionen und Outlets.

### <a name="setting-a-custom-main-window-controller"></a>Festlegen eines benutzerdefinierten Hauptfenster Controllers

Damit Outlets und Aktionen erstellt werden können, um Benutzeroberflächen Elemente für C# Code verfügbar zu machen, muss die xamarin. Mac-app einen benutzerdefinierten Fenster Controller verwenden.

Führen Sie folgende Schritte aus:

1. Öffnen Sie das Storyboard der app in der Interface Builder von Xcode.
2. Wählen Sie die `NSWindowController` im Designoberfläche aus.
3. Wechseln Sie zur Ansicht " **Identitäts Inspektor** ", und geben Sie `WindowController` als **Klassennamen**ein:

    [![Bearbeiten des Klassen namens](xib-images/windowcontroller01.png "Bearbeiten des Klassen namens")](xib-images/windowcontroller01-large.png#lightbox)
4. Speichern Sie die Änderungen, und kehren Sie zur Synchronisierung Visual Studio für Mac zurück.
5. Eine **WindowController.cs** -Datei wird dem Projekt im **Lösungspad** in Visual Studio für Mac hinzugefügt:

    ![Der neue Klassenname in Visual Studio für Mac](xib-images/windowcontroller02.png "Der neue Klassenname in Visual Studio für Mac")
6. Öffnen Sie das Storyboard erneut in der Interface Builder von Xcode.
7. Die Datei " **windowcontroller. h** " steht zur Verwendung zur Verfügung:

    [![Die entsprechende h-Datei in Xcode](xib-images/windowcontroller03.png "Die entsprechende h-Datei in Xcode")](xib-images/windowcontroller03-large.png#lightbox)

### <a name="outlets-and-actions"></a>Outlets und Aktionen

Was sind Outlets und Aktionen? Bei der herkömmlichen .NET-Benutzeroberflächenprogrammierung wird ein Steuerelement auf der Benutzeroberfläche automatisch als Eigenschaft verfügbar gemacht, wenn es hinzugefügt wird. Unter Mac ist dies anders. Wenn Sie ein Steuerelement zu einer Ansicht hinzufügen, wird es dadurch nicht automatisch für Code zugänglich. Der Entwickler muss die UI-Elemente explizit für Code verfügbar machen. In der richtigen Reihenfolge gibt Apple zwei Optionen:

- **Outlets**: Outlets sind vergleichbar mit Eigenschaften. Wenn Sie ein Steuerelement an ein Outlet anschließen, wird es über eine Eigenschaft für den Code verfügbar gemacht, sodass Sie z. b. Anfügen von Ereignis Handlern, Methodenaufrufe usw. durchführen können.
- **Aktionen**: Aktionen sind vergleichbar mit dem Befehlsmuster in WPF. Wenn z. b. eine Aktion für ein Steuerelement ausgeführt wird, z. b. durch Klicken auf eine Schaltfläche, ruft das Steuerelement automatisch eine Methode in Ihrem Code auf. Aktionen sind leistungsstark und praktisch, da Sie viele Steuerelemente an dieselbe Aktion übertragen können.

In Xcode werden Outlets und Aktionen direkt im Code über das *ziehen von Steuer*Elementen hinzugefügt. Genauer gesagt bedeutet dies, dass Sie zum Erstellen eines Outlets oder einer Aktion ein Steuerelement auswählen, das Sie ein Outlet oder eine Aktion hinzufügen möchten, indem Sie die Schaltfläche **Control** auf der Tastatur gedrückt halten und das Steuerelement direkt in den Code ziehen.

Für xamarin. Mac-Entwickler bedeutet dies, dass Sie in die Ziel-C-Stubdateien ziehen, die der C# Datei entsprechen, in der Sie das Outlet oder die Aktion erstellen möchten. Visual Studio für Mac eine Datei namens " **MainWindow. h** " als Teil des Shim-Xcode-Projekts erstellt, das für die Verwendung der Interface Builder generiert wurde:

[![Ein Beispiel für eine h-Datei in Xcode](xib-images/xcode16.png "Ein Beispiel für eine h-Datei in Xcode")](xib-images/xcode16-large.png#lightbox)

Diese Stub. h-Datei spiegelt die **MainWindow.Designer.cs** , die einem xamarin. Mac-Projekt automatisch hinzugefügt wird, wenn ein neuer `NSWindow` erstellt wird. Diese Datei wird verwendet, um die von Interface Builder vorgenommenen Änderungen zu synchronisieren, und ist der Ort, an dem wir Ihre Outlets und Aktionen erstellen, C# damit Benutzeroberflächen Elemente für Code verfügbar gemacht werden.

#### <a name="adding-an-outlet"></a>Hinzufügen eines Outlets

Ein grundlegendes Verständnis der Outlets und Aktionen besteht darin, ein Outlet zu erstellen, um ein Benutzeroberflächen Element für Ihren C# Code verfügbar zu machen.

Führen Sie folgende Schritte aus:

1. Klicken Sie in der rechten oberen Ecke in Xcode auf die Schaltfläche mit den **zwei Kreisen**, um den **Assistenten-Editor** zu öffnen:

    [![Auswählen des Assistenten-Editors](xib-images/outlet01.png "Auswählen des Assistenten-Editors")](xib-images/outlet01-large.png#lightbox)
2. Xcode wechselt in eine geteilte Ansicht mit dem **Schnittstellen-Editor** auf der einen und dem **Code-Editor** auf der anderen Seite.
3. Beachten Sie, dass Xcode automatisch die Datei " **mainwindowcontroller. m** " im **Code-Editor**ausgewählt hat, was falsch ist. Wenn Sie wissen, welche Outlets und Aktionen oben vorliegen, müssen Sie die " **MainWindow. h** "-Option auswählen.
4. Klicken Sie am oberen Rand des **Code-Editors** auf den **automatischen Link** , und wählen Sie die Datei " **MainWindow. h** " aus:

    [![Auswählen der richtigen h-Datei](xib-images/outlet02.png "Auswählen der richtigen h-Datei")](xib-images/outlet02-large.png#lightbox)
5. Xcode sollte jetzt die korrekte Datei ausgewählt haben:

    [![Die richtige Datei ist ausgewählt.](xib-images/outlet03.png "Die richtige Datei ist ausgewählt.")](xib-images/outlet03-large.png#lightbox)
6. **Der letzte Schritt war sehr wichtig!** Wenn Sie die richtige Datei nicht ausgewählt haben, können Sie keine Outlets und Aktionen erstellen, oder Sie werden in C#der falschen Klasse verfügbar gemacht.
7. Halten Sie im **Schnitt**stellen-Editor die STRG **-Taste gedrückt** , und klicken Sie auf die oben erstellte Bezeichnung auf den Code-Editor direkt unterhalb des `@interface MainWindow : NSWindow { }` Codes:

    [![Ziehen, um ein neues Outlet zu erstellen](xib-images/outlet04.png "Ziehen, um ein neues Outlet zu erstellen")](xib-images/outlet04-large.png#lightbox)
8. Es wird ein Dialogfeld geöffnet. Belassen Sie den **Verbindungs** Satz auf "Outlet", und geben Sie `ClickedLabel` für den **Namen**ein:

    [![Festlegen der Outlet-Eigenschaften](xib-images/outlet05.png "Festlegen der Outlet-Eigenschaften")](xib-images/outlet05-large.png#lightbox)
9. Klicken Sie auf die Schaltfläche **verbinden** , um das Outlet zu erstellen:

    ![Der abgeschlossene Ausgang](xib-images/outlet06.png "Der abgeschlossene Ausgang")
10. Speichern Sie die Änderungen in der Datei.

#### <a name="adding-an-action"></a>Hinzufügen einer Aktion

Als nächstes sehen wir uns das Erstellen einer Aktion an, um eine Benutzerinteraktion mit dem Benutzeroberflächen C# Element für Ihren Code verfügbar zu machen.

Führen Sie folgende Schritte aus:

1. Stellen Sie sicher, dass Sie sich noch im **Assistenten-Editor** befinden und dass die Datei " **MainWindow. h** " im **Code-Editor**angezeigt wird.
2. Halten Sie im **Schnitt**stellen-Editor die STRG-Taste gedrückt, und klicken Sie auf die Schaltfläche **, die wir** oben erstellt haben, auf den Code-Editor direkt unterhalb des `@property (assign) IBOutlet NSTextField *ClickedLabel;` Codes:

    [![Ziehen zum Erstellen einer Aktion](xib-images/action01.png "Ziehen zum Erstellen einer Aktion")](xib-images/action01-large.png#lightbox)
3. Ändern Sie den **Verbindungstyp** in Aktion:

    [![Aktionstyp auswählen](xib-images/action02.png "Aktionstyp auswählen")](xib-images/action02-large.png#lightbox)
4. Geben Sie `ClickedButton` als **Namen** ein:

    [![Konfigurieren der Aktion](xib-images/action03.png "Konfigurieren der Aktion")](xib-images/action03-large.png#lightbox)
5. Klicken Sie zum Erstellen einer Aktion auf die Schaltfläche **verbinden** :

    ![Die abgeschlossene Aktion](xib-images/action04.png "Die abgeschlossene Aktion")
6. Speichern Sie die Änderungen in der Datei.

Wenn Ihre Benutzeroberfläche verbunden und für den C# Code verfügbar gemacht wurde, wechseln Sie zurück zu Visual Studio für Mac, und lassen Sie die Änderungen von Xcode und Interface Builder synchronisieren.

### <a name="writing-the-code"></a>Schreiben des Codes

Nachdem Sie Ihre Benutzeroberfläche erstellt und ihre UI-Elemente im Code über Outlets und Aktionen verfügbar gemacht haben, können Sie den Code schreiben, um das Programm in den Lebenszyklus zu bringen. Öffnen Sie z. b. die **MainWindow.cs** -Datei zur Bearbeitung, indem Sie in der **Lösungspad**auf die Datei doppelklicken:

[![Die MainWindow.cs-Datei](xib-images/code01.png "Die MainWindow.cs-Datei")](xib-images/code01-large.png#lightbox)

Fügen Sie der `MainWindow`-Klasse den folgenden Code hinzu, um mit dem Beispiel-Outlet zu arbeiten, das Sie oben erstellt haben:

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

Beachten Sie, dass der Zugriff auf C# die `NSLabel` über den direkten Namen erfolgt, den Sie in Xcode zugewiesen haben, als Sie das Outlet in Xcode erstellt haben. in diesem Fall wird es als `ClickedLabel` bezeichnet. Sie können auf jede Methode oder Eigenschaft des verfügbar gemachten Objekts auf die gleiche Weise wie jede normale C# Klasse zugreifen.

> [!IMPORTANT]
> Sie müssen anstelle einer anderen Methode, wie z. b. `Initialize`, `AwakeFromNib` verwenden, da `AwakeFromNib` aufgerufen wird, _nachdem_ das Betriebssystem die Benutzeroberfläche aus der XIb-Datei geladen und instanziiert hat. Wenn Sie versucht haben, auf das Label-Steuerelement zuzugreifen, bevor die XIb-Datei vollständig geladen und instanziiert wurde, erhalten Sie einen `NullReferenceException` Fehler, weil das Label-Steuerelement noch nicht erstellt wurde.

Fügen Sie als nächstes die folgende partielle Klasse der `MainWindow`-Klasse hinzu:

```csharp
partial void ClickedButton (Foundation.NSObject sender) {

    // Update counter and label
    ClickedLabel.StringValue = string.Format("The button has been clicked {0} time{1}.",++numberOfTimesClicked, (numberOfTimesClicked < 2) ? "" : "s");
}
```

Dieser Code wird an die Aktion angefügt, die Sie in Xcode erstellt haben, und Interface Builder und wird aufgerufen, wenn der Benutzer auf die Schaltfläche klickt.

Einige Elemente der Benutzeroberfläche haben automatisch integrierte Aktionen, z. b. Elemente in der Standardmenü Leiste, z. b. das Menü Element **Öffnen...** (`openDocument:`). Doppelklicken Sie im **Lösungspad**auf die Datei **AppDelegate.cs** , um Sie zur Bearbeitung zu öffnen, und fügen Sie den folgenden Code unterhalb der `DidFinishLaunching`-Methode hinzu:

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

Die Schlüssel Zeile hier ist `[Export ("openDocument:")]`. Sie teilt `NSMenu`, dass der **appdelegat** über eine Methode `void OpenDialog (NSObject sender)` verfügt, die auf die `openDocument:` Aktion antwortet.

Weitere Informationen zum Arbeiten mit Menüs finden Sie in unserer [Menüs](~/mac/user-interface/menu.md) -Dokumentation.

## <a name="synchronizing-changes-with-xcode"></a>Synchronisieren von Änderungen mit Xcode

Wenn Sie von Xcode zurück zu Visual Studio für Mac wechseln, werden alle Änderungen, die Sie in Xcode vorgenommen haben, automatisch mit Ihrem xamarin. Mac-Projekt synchronisiert.

Wenn Sie im Lösungspad die **MainWindow.Designer.cs** auswählen , können Sie sehen, wie unser Outlet und ihre Aktion im C# Code verknüpft wurden:

[![Synchronisieren von Änderungen mit Xcode](xib-images/sync01.png "Synchronisieren von Änderungen mit Xcode")](xib-images/sync01-large.png#lightbox)

Beachten Sie, dass die beiden Definitionen in der **MainWindow.Designer.cs** -Datei:

```csharp
[Outlet]
AppKit.NSTextField ClickedLabel { get; set; }

[Action ("ClickedButton:")]
partial void ClickedButton (Foundation.NSObject sender);
```

Richten Sie die Definitionen in der Datei " **MainWindow. h** " in Xcode ein:

```objc
@property (assign) IBOutlet NSTextField *ClickedLabel;
- (IBAction)ClickedButton:(id)sender;
```

Wie Sie sehen, werden Visual Studio für Mac Änderungen an der h-Datei abhört und diese Änderungen dann automatisch in der entsprechenden **. Designer.cs** -Datei synchronisieren, um Sie für Ihre Anwendung verfügbar zu machen. Sie können auch bemerken, dass **MainWindow.Designer.cs** eine partielle Klasse ist, sodass Visual Studio für Mac nicht ändern muss **MainWindow.cs** was alle Änderungen überschreibt, die wir an der Klasse vorgenommen haben.

Normalerweise müssen Sie das **MainWindow.Designer.cs** nicht selbst öffnen, sondern nur zu Schulungszwecken.

> [!IMPORTANT]
> In den meisten Fällen sehen Visual Studio für Mac automatisch alle Änderungen, die in Xcode vorgenommen werden, und synchronisieren Sie mit Ihrem xamarin. Mac-Projekt. Sollte die Synchronisierung nicht automatisch durchgeführt werden – was sehr selten passiert –, wechseln Sie zurück zu Xcode und dann erneut zu Visual Studio für Mac. Dadurch wird normalerweise der Synchronisierungsprozess gestartet.

## <a name="adding-a-new-window-to-a-project"></a>Hinzufügen eines neuen Fensters zu einem Projekt

Abgesehen vom Hauptdokument Fenster muss eine xamarin. Mac-Anwendung möglicherweise andere Windows-Typen für den Benutzer anzeigen, z. b. Einstellungen oder Inspektor-Panels. Wenn Sie dem Projekt ein neues Fenster hinzufügen, sollten Sie immer das **Cocoa-Fenster mit der Controller** -Option verwenden, da dadurch das Laden des Fensters aus der XIb-Datei vereinfacht wird.

Gehen Sie folgendermaßen vor, um ein neues Fenster hinzuzufügen:

1. Klicken Sie im **Lösungspad**mit der rechten Maustaste auf das Projekt, und wählen Sie  > **neue Datei** **Hinzufügen** ... aus.
2. Wählen Sie im Dialogfeld neue Datei die Option **xamarin. Mac**  > **Cocoa-Fenster mit Controller**:

    ![Hinzufügen eines neuen Fenster Controllers](xib-images/new01.png "Hinzufügen eines neuen Fenster Controllers")
3. Geben Sie für den **Namen** `PreferencesWindow` ein, und klicken Sie auf **Neu**.
4. Doppelklicken Sie auf die Datei " **preferenceswindow. XIb** ", um Sie für die Bearbeitung in Interface Builder zu öffnen:

    [![Bearbeiten des Fensters in Xcode](xib-images/new02.png "Bearbeiten des Fensters in Xcode")](xib-images/new02-large.png#lightbox)
5. Entwerfen Sie Ihre Schnittstelle:

    [![Entwerfen des Windows-Layouts](xib-images/new03.png "Entwerfen des Windows-Layouts")](xib-images/new03-large.png#lightbox)
6. Speichern Sie die Änderungen, und kehren Sie zu Visual Studio für Mac zurück, um mit Xcode zu synchronisieren.

Fügen Sie **AppDelegate.cs** den folgenden Code hinzu, um das neue Fenster anzuzeigen:

```csharp
[Export("applicationPreferences:")]
void ShowPreferences (NSObject sender)
{
    var preferences = new PreferencesWindowController ();
    preferences.Window.MakeKeyAndOrderFront (this);
}
```

Die `var preferences = new PreferencesWindowController ();` Zeile erstellt eine neue Instanz des Fenster Controllers, der das Fenster aus der XIb-Datei lädt und vergrößert. Die `preferences.Window.MakeKeyAndOrderFront (this);` Zeile zeigt dem Benutzer das neue Fenster an.

Wenn Sie den Code ausführen und die **Einstellungen** im **Anwendungsmenü**auswählen, wird das Fenster angezeigt:

![Ausführen der Beispiel-App](xib-images/new04.png "Ausführen der Beispiel-App")

Weitere Informationen zum Arbeiten mit Windows in einer xamarin. Mac-Anwendung finden Sie in unserer [Windows](~/mac/user-interface/window.md) -Dokumentation.

## <a name="adding-a-new-view-to-a-project"></a>Hinzufügen einer neuen Ansicht zu einem Projekt

Es gibt Zeiten, in denen es einfacher ist, den Entwurf Ihres Fensters in mehrere, besser verwaltbare XIb-Dateien zu unterbrechen. Beispielsweise das Auslagern des Inhalts des Hauptfensters bei der Auswahl eines Symbolleisten Elements in einem **Einstellungsfenster** oder das Austauschen von Inhalten als Reaktion auf eine **Quell Listen** Auswahl.

Wenn Sie dem Projekt eine neue Ansicht hinzufügen, sollten Sie immer die Option **Cocoa-Ansicht mit Controller** verwenden, da dadurch das Laden der Sicht aus der XIb-Datei vereinfacht wird.

Gehen Sie folgendermaßen vor, um eine neue Ansicht hinzuzufügen:

1. Klicken Sie im **Lösungspad**mit der rechten Maustaste auf das Projekt, und wählen Sie  > **neue Datei** **Hinzufügen** ... aus.
2. Wählen Sie im Dialogfeld neue Datei die Option **xamarin. Mac**  > **Cocoa-Ansicht mit Controller**:

    ![Hinzufügen einer neuen Ansicht](xib-images/view01.png "Hinzufügen einer neuen Ansicht")
3. Geben Sie für den **Namen** `SubviewTable` ein, und klicken Sie auf **Neu**.
4. Doppelklicken Sie auf die Datei **subviewtable. XIb** , um Sie für die Bearbeitung in Interface Builder zu öffnen und die Benutzeroberfläche zu entwerfen:

    [![Entwerfen der neuen Ansicht in Xcode](xib-images/view02.png "Entwerfen der neuen Ansicht in Xcode")](xib-images/view02-large.png#lightbox)
5. Richten Sie alle erforderlichen Aktionen und Outlets ein.
6. Speichern Sie die Änderungen, und kehren Sie zu Visual Studio für Mac zurück, um mit Xcode zu synchronisieren.

Bearbeiten Sie als nächstes die **SubviewTable.cs** , und fügen Sie den folgenden Code in die Datei " **awakeFromNib** " ein, um die neue Ansicht beim Laden aufzufüllen:

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

Fügen Sie dem Projekt eine Aufzählung hinzu, um zu verfolgen, welche Ansicht aktuell angezeigt wird. Beispiel: **SubviewType.cs**:

```csharp
public enum SubviewType
{
    None,
    TableView,
    OutlineView,
    ImageView
}
```

Bearbeiten Sie die XIb-Datei des Fensters, in dem die Ansicht verwendet wird, und zeigen Sie Sie an. Fügen Sie eine **benutzerdefinierte Ansicht** hinzu, die als Container für die Ansicht fungiert, nachdem Sie durch Code C# in den Arbeitsspeicher geladen wurde, und machen Sie Sie in einem Outlet mit dem Namen `ViewContainer` verfügbar:

[![Erstellen des erforderlichen Outlets](xib-images/view03.png "Erstellen des erforderlichen Outlets")](xib-images/view03-large.png#lightbox)

Speichern Sie die Änderungen, und kehren Sie zu Visual Studio für Mac zurück, um mit Xcode zu synchronisieren.

Bearbeiten Sie als nächstes die CS-Datei des Fensters, in dem die neue Ansicht angezeigt wird (z. b. **MainWindow.cs**), und fügen Sie den folgenden Code hinzu:

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

Wenn wir eine neue Ansicht anzeigen müssen, die aus einer XIb-Datei im Container des Fensters geladen wurde (die oben hinzugefügte **benutzerdefinierte Ansicht** ), behandelt dieser Code das Entfernen vorhandener Sichten und das austauschen für die neue Ansicht. Sie sehen, dass Sie bereits eine Ansicht angezeigt haben, wenn dies der Fall ist, wird Sie aus dem Bildschirm entfernt. Im nächsten Schritt wird die von Ihnen weiter gegebene Ansicht (wie von einem Ansichts Controller geladen) angepasst, sodass Sie in den Inhalts Bereich passt, und Sie wird dem Inhalt für die Anzeige hinzugefügt.

Verwenden Sie den folgenden Code, um eine neue Ansicht anzuzeigen:

```csharp
DisplaySubview(new SubviewTableController(), SubviewType.TableView);
```

Dadurch wird eine neue Instanz des Ansichts Controllers für die anzuzeigende neue Ansicht erstellt, der zugehörige Typ (wie durch die dem Projekt hinzugefügte Enumeration angegeben) festgelegt und die `DisplaySubview` Methode verwendet, die der Klasse des Fensters hinzugefügt wurde, um die Ansicht tatsächlich anzuzeigen. Beispiel:

[![Ausführen der Beispiel-App](xib-images/view04.png "Ausführen der Beispiel-App")](xib-images/view04-large.png#lightbox)

Weitere Informationen zum Arbeiten mit Windows in einer xamarin. Mac-Anwendung finden Sie in der Dokumentation zu [Windows](~/mac/user-interface/window.md) und [Dialog](~/mac/user-interface/dialog.md) Feldern.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde die Arbeit mit XIb-Dateien in einer xamarin. Mac-Anwendung ausführlich erläutert. Wir haben die unterschiedlichen Typen und Verwendungsmöglichkeiten von XIb-Dateien zum Erstellen der Benutzeroberfläche Ihrer Anwendung, zum Erstellen und Verwalten von XIb-Dateien in Xcode-Interface Builder und zum Arbeiten mit XIb-Dateien C# im Code gesehen.

## <a name="related-links"></a>Verwandte Links

- [MacImages (Beispiel)](https://docs.microsoft.com/samples/xamarin/mac-samples/macimages)
- [Hello, Mac (Hallo, Mac)](~/mac/get-started/hello-mac.md)
- [Windows](~/mac/user-interface/window.md)
- [Menüs](~/mac/user-interface/menu.md)
- [Dialogfelder](~/mac/user-interface/dialog.md)
- [Arbeiten mit Bildern](~/mac/app-fundamentals/image.md)
- [macOS-Eingaberichtlinien](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
