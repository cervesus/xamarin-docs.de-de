---
title: Symbolleisten in xamarin. Mac
description: Dieser Artikel beschreibt das Arbeiten mit Symbolleisten in einer xamarin. Mac-Anwendung. Es behandelt das Erstellen und Verwalten von Symbolleisten in Xcode und Interface Builder, die Bereitstellung für Code und die programmgesteuerte Arbeit mit Ihnen.
ms.prod: xamarin
ms.assetid: C8D228CE-C860-47E1-85FD-69864BF91F20
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 03/14/2017
ms.openlocfilehash: 5cf86adf07043a60c6fe445265e14591692e365b
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73008355"
---
# <a name="toolbars-in-xamarinmac"></a>Symbolleisten in xamarin. Mac

_Dieser Artikel beschreibt das Arbeiten mit Symbolleisten in einer xamarin. Mac-Anwendung. Es behandelt das Erstellen und Verwalten von Symbolleisten in Xcode und Interface Builder, die Bereitstellung für Code und die programmgesteuerte Arbeit mit Ihnen._

Xamarin. Mac-Entwickler, die mit Visual Studio für Mac arbeiten, haben Zugriff auf dieselben UI-Steuerelemente, die für macOS-Entwickler mit Xcode verfügbar sind, einschließlich des Toolbar-Steuer Elements. Da xamarin. Mac direkt in Xcode integriert ist, ist es möglich, die Interface Builder von Xcode zu verwenden, um Symbolleisten Elemente zu erstellen und zu verwalten. Diese Symbolleisten Elemente können auch in C#erstellt werden.

Symbolleisten in macOS werden dem oberen Abschnitt eines Fensters hinzugefügt und ermöglichen einen einfachen Zugriff auf Befehle im Zusammenhang mit der Funktionalität. Symbolleisten können von den Benutzern einer Anwendung ausgeblendet, angezeigt oder angepasst werden, und Sie können Symbolleisten Elemente auf verschiedene Weise darstellen.

In diesem Artikel werden die Grundlagen der Arbeit mit Symbolleisten und Symbolleisten Elementen in einer xamarin. Mac-Anwendung behandelt. 

Bevor Sie fortfahren, lesen Sie den Artikel [Hello, Mac](~/mac/get-started/hello-mac.md) – insbesondere die [Einführung in Xcode und Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) und [Outlets und Aktionen](~/mac/get-started/hello-mac.md#outlets-and-actions) –, da er wichtige Konzepte und Techniken behandelt, die in diesem Handbuch verwendet werden.

Sehen Sie sich auch den Abschnitt verfügbar machen von [ C# Klassen/Methoden zu Ziel-C](~/mac/internals/how-it-works.md) im Dokument " [xamarin. Mac Internals](~/mac/internals/how-it-works.md) " an. Es werden die `Register`-und `Export` Attribute erläutert, C# die zum Verbinden von Klassen mit Ziel-C-Klassen verwendet werden.

## <a name="introduction-to-toolbars"></a>Einführung in Symbolleisten

Jedes Fenster in einer macOS-Anwendung kann eine Symbolleiste enthalten:

![Ein Beispiel Fenster mit einer Symbolleiste](toolbar-images/info01.png "Ein Beispiel Fenster mit einer Symbolleiste")

Symbolleisten bieten den Benutzern Ihrer Anwendung eine einfache Möglichkeit, schnell auf wichtige oder häufig verwendete Features zuzugreifen. Beispielsweise kann eine Anwendung zur Dokument Bearbeitung Symbolleisten Elemente zum Festlegen der Textfarbe, zum Ändern der Schriftart oder zum Drucken des aktuellen Dokuments enthalten.

Symbolleisten können Elemente auf drei Arten anzeigen:

1. **Symbol und Text** 

     ![Eine Symbolleiste mit Symbolen und Text](toolbar-images/info02.png "Eine Symbolleiste mit Symbolen und Text")

2. **Nur Symbol** 

     ![Eine Symbol geschützte Symbolleiste](toolbar-images/info03.png "Eine Symbol geschützte Symbolleiste")

3. **Nur Text** 

     ![Eine nur-Text-Symbolleiste](toolbar-images/info04.png "Eine nur-Text-Symbolleiste")

Wechseln Sie zwischen diesen Modi, indem Sie mit der rechten Maustaste auf die Symbolleiste klicken und im Kontextmenü einen Anzeigemodus auswählen:

![Das Kontextmenü für eine Symbolleiste](toolbar-images/info05.png "Das Kontextmenü für eine Symbolleiste")

Verwenden Sie das gleiche Menü, um die Symbolleiste in einer kleineren Größe anzuzeigen:

![Eine Symbolleiste mit kleinen Symbolen](toolbar-images/info06.png "Eine Symbolleiste mit kleinen Symbolen")

Das Menü ermöglicht außerdem das Anpassen der Symbolleiste:

![Dialogfeld zum Anpassen einer Symbolleiste](toolbar-images/info07.png "Dialogfeld zum Anpassen einer Symbolleiste")

Beim Einrichten einer Symbolleiste in der Interface Builder von Xcode kann ein Entwickler zusätzliche Symbolleisten Elemente bereitstellen, die nicht Teil der Standardkonfiguration sind. Benutzer der Anwendung können dann die Symbolleiste anpassen und diese vordefinierten Elemente bei Bedarf hinzufügen und entfernen. Natürlich kann die Symbolleiste auf die Standardkonfiguration zurückgesetzt werden.

Die Symbolleiste stellt automatisch eine Verbindung mit dem Menü **Ansicht** her, sodass Benutzer Sie ausblenden, anzeigen und anpassen können:

![Symbolleisten bezogene Elemente im Menü "Ansicht"](toolbar-images/info08.png "Symbolleisten bezogene Elemente im Menü "Ansicht"")

Weitere Informationen finden Sie in der Dokumentation zur [integrierten Menü Funktionalität](~/mac/user-interface/menu.md) .

Wenn die Symbolleiste in Interface Builder ordnungsgemäß konfiguriert ist, werden die Symbolleisten Anpassungen von der Anwendung automatisch über mehrere Starts der Anwendung beibehalten.

In den nächsten Abschnitten dieses Handbuchs wird beschrieben, wie Symbolleisten mit dem Interface Builder von Xcode erstellt und verwaltet werden und wie Sie mit Ihnen im Code arbeiten können.

## <a name="setting-a-custom-main-window-controller"></a>Festlegen eines benutzerdefinierten Hauptfenster Controllers

Um UI-Elemente für C# den Code über Outlets und Aktionen verfügbar zu machen, muss die xamarin. Mac-app einen benutzerdefinierten Fenster Controller verwenden:

1. Öffnen Sie das Storyboard der app in der Interface Builder von Xcode.
2. Wählen Sie auf der Entwurfs Oberfläche den Fenster Controller aus.
3. Wechseln Sie zur **Identitäts** Prüfung, und geben Sie "windowcontroller" als **Klassennamen**ein: 

    [![Festlegen eines benutzerdefinierten Klassen namens für den Fenster Controller](toolbar-images/windowcontroller01.png "Festlegen eines benutzerdefinierten Klassen namens für den Fenster Controller")](toolbar-images/windowcontroller01-large.png#lightbox) 

4. Speichern Sie die Änderungen, und kehren Sie zur Synchronisierung Visual Studio für Mac zurück.
5. Eine **WindowController.cs** -Datei wird dem Projekt im **Lösungspad** in Visual Studio für Mac hinzugefügt: 

    ![Auswählen von WindowController.cs im Lösungspad](toolbar-images/windowcontroller02.png "Auswählen von WindowController.cs im Lösungspad")

6. Öffnen Sie das Storyboard erneut in der Interface Builder von Xcode.
7. Die Datei " **windowcontroller. h** " steht zur Verwendung zur Verfügung: 

    [![Die Datei "windowcontroller. h"](toolbar-images/windowcontroller03.png "Die Datei "windowcontroller. h"")](toolbar-images/windowcontroller03-large.png#lightbox)

## <a name="creating-and-maintaining-toolbars-in-xcode"></a>Erstellen und Verwalten von Symbolleisten in Xcode

Symbolleisten werden mit dem Interface Builder von Xcode erstellt und verwaltet. Wenn Sie einer Anwendung eine Symbolleiste hinzufügen möchten, bearbeiten Sie das primäre Storyboard der APP (in diesem Fall " **Main. Storyboard**"), indem Sie in der **Lösungspad**auf die Symbolleiste doppelklicken:

![Öffnen von "Main. Storyboard" im Lösungspad](toolbar-images/edit01.png "Öffnen von "Main. Storyboard" im Lösungspad")

Geben Sie im **Bibliotheks Inspektor**"Tool" in das **Suchfeld** ein, damit alle verfügbaren Symbolleisten Elemente leichter angezeigt werden können:

![Der Bibliothek Inspektor, gefiltert zum Anzeigen von Symbolleisten Elementen.](toolbar-images/edit02.png "Der Bibliothek Inspektor, gefiltert zum Anzeigen von Symbolleisten Elementen.")

Ziehen Sie eine Symbolleiste auf das Fenster im Schnittstellen- **Editor**. Wenn die Symbolleiste ausgewählt ist, konfigurieren Sie das Verhalten, indem Sie die Eigenschaften im **Attribut Inspektor**festlegen:

![Der-Attribut Inspektor für eine Symbolleiste.](toolbar-images/edit04.png "Der-Attribut Inspektor für eine Symbolleiste.")

Die folgenden Eigenschaften sind verfügbar:

1. **Anzeigen** : steuert, ob die Symbolleiste Symbole, Text oder beides anzeigt.
2. **Beim Start sichtbar** : Wenn diese Option ausgewählt ist, wird die Symbolleiste standardmäßig angezeigt.
3. **Anpassbar** : Wenn diese Option ausgewählt ist, können Benutzer die Symbolleiste bearbeiten und anpassen.
4. **Trenn** Zeichen: Wenn diese Option ausgewählt ist, wird die Symbolleiste von einer dünnen horizontalen Linie vom Inhalt des Fensters getrennt.
5. **Größe** : legt die Größe der Symbolleiste fest.
6. **Automatisch speichern** : Wenn diese Option ausgewählt ist, werden die Änderungen an der Symbolleisten Konfiguration eines Benutzers von der Anwendung über Anwendungs Starts hinweg beibehalten.

Wählen Sie die Option **automatisch speichern** aus, und überlassen Sie alle anderen Eigenschaften ihren Standardeinstellungen. 

Öffnen Sie nach dem Öffnen der Symbolleiste in der **Schnittstellen Hierarchie**das Anpassungs Dialogfeld, indem Sie ein Symbolleisten Element auswählen:

![Anpassen der Symbolleiste](toolbar-images/edit05.png "Anpassen der Symbolleiste")

Verwenden Sie dieses Dialogfeld, um Eigenschaften für Elemente festzulegen, die bereits Teil der Symbolleiste sind, um die Standard Symbolleiste für die Anwendung zu entwerfen und um zusätzliche Symbolleisten Elemente anzugeben, die ein Benutzer auswählen kann, wenn er die Symbolleiste anpasst. Um der Symbolleiste Elemente hinzuzufügen, ziehen Sie Sie aus der **Bibliothek Inspector**:

![Der Bibliotheks Inspektor](toolbar-images/edit06.png "Der Bibliotheks Inspektor")

Die folgenden Symbolleisten Elemente können hinzugefügt werden:

- **Bildsymbol leisten Element** -ein Symbolleisten Element mit einem benutzerdefinierten Bild als Symbol.
- **Flexibles Leerzeichen-Symbolleisten Element** : flexibler Bereich zum begründen der nachfolgenden Symbolleisten Elemente. Beispielsweise wird in einem oder mehreren Symbolleisten Elementen, gefolgt von einem flexiblen Leerzeichen-Symbolleisten Element und einem weiteren Symbolleisten Element, das letzte Element an der rechten Seite der Symbolleiste angeheftet.
- **Leerzeichen-Symbolleisten Element** -fester Bereich zwischen Elementen auf der Symbolleiste
- **Trenn** Zeichen-Symbolleisten Element-ein sichtbares Trennzeichen zwischen mindestens zwei Symbolleisten Elementen für die Gruppierung
- **Symbolleisten Element anpassen** : ermöglicht Benutzern das Anpassen der Symbolleiste.
- **Symbolleisten Element "Drucken** ": ermöglicht Benutzern das Drucken des geöffneten Dokuments.
- **Symbolleisten Element Farben anzeigen** : zeigt die standardmäßige System Farbauswahl an: 

     ![Die System Farbauswahl](toolbar-images/edit07.png "Die System Farbauswahl")

- **Schriftart-Symbolleisten Element anzeigen** : zeigt das standardmäßige System Schriftart Dialogfeld an: 

     ![Die Schriftart Auswahl](toolbar-images/edit08.png "Die Schriftart Auswahl")

> [!IMPORTANT]
> Wie Sie später sehen werden, können auch viele standardmäßige Cocoa-UI-Steuerelemente wie Suchfelder, segmentierte Steuerelemente und horizontale Schieberegler zu einer Symbolleiste hinzugefügt werden.

### <a name="adding-an-item-to-a-toolbar"></a>Hinzufügen eines Elements zu einer Symbolleiste

Wenn Sie einer Symbolleiste ein Element hinzufügen möchten, wählen Sie die Symbolleiste in der **Schnittstellen Hierarchie** aus, und klicken Sie auf eines ihrer Elemente, damit das Dialogfeld Anpassung angezeigt wird. Ziehen Sie als nächstes ein neues Element aus der **Bibliothek Inspektor** in den Bereich **zulässige Symbolleisten Elemente** :

![Zulässige Symbolleisten Elemente im Dialogfeld für die Symbolleisten Anpassung](toolbar-images/add01.png "Zulässige Symbolleisten Elemente im Dialogfeld für die Symbolleisten Anpassung")

Um sicherzustellen, dass ein neues Element Teil der Standard Symbolleiste ist, ziehen Sie es in den Standardbereich der **Symbolleisten Elemente** : 

![Neuanordnen eines Symbolleisten Elements durchziehen](toolbar-images/add02.png "Neuanordnen eines Symbolleisten Elements durchziehen")

Wenn Sie Standard Symbolleisten Elemente neu anordnen möchten, ziehen Sie Sie nach links oder rechts.

Verwenden Sie als nächstes den **Attribute Inspector** , um die Standardeigenschaften für das Element festzulegen:

![Anpassen eines Toolbar-Elements mit dem Attribute Inspector](toolbar-images/add03.png "Anpassen eines Toolbar-Elements mit dem Attribute Inspector")

Die folgenden Eigenschaften sind verfügbar:

- **Bildname** : Bild, das als Symbol für das Element verwendet werden soll.
- **Bezeichnungs** Text, der für das Element in der Symbolleiste angezeigt werden soll.
- **Palettenbezeichnung** -Text, der für das Element im Bereich der **zulässigen Symbolleisten Elemente** angezeigt werden soll.
- **Tag** : ein optionaler eindeutiger Bezeichner, der die Identifizierung des Elements im Code erleichtert.
- **Identifier** : definiert den Typ der Symbolleisten Elemente. Ein benutzerdefinierter Wert kann verwendet werden, um ein Symbolleisten Element im Code auszuwählen.
- **Auswählbar** : Wenn das Element aktiviert ist, verhält es sich wie eine Schaltfläche ein/aus.

> [!IMPORTANT]
> Fügen Sie dem Bereich **zulässige Symbolleisten Elemente** ein Element hinzu, aber nicht die Standard Symbolleiste, um Anpassungsoptionen für Benutzer bereitzustellen. 

### <a name="adding-other-ui-controls-to-a-toolbar"></a>Hinzufügen anderer UI-Steuerelemente zu einer Symbolleiste

Einer Symbolleiste können auch mehrere Cocoa-Elemente der Benutzeroberfläche hinzugefügt werden, z. b. Suchfelder und segmentierte Steuerelemente

Öffnen Sie dazu die Symbolleiste in der **Schnittstellen Hierarchie** , und wählen Sie ein Symbolleisten Element aus, um das Anpassungs Dialogfeld zu öffnen. Ziehen Sie ein **Suchfeld** aus dem **Bibliotheks Inspektor** in den Bereich **zulässige Symbolleisten Elemente** :

![Verwenden des Dialog Felds zur Symbolleisten Anpassung](toolbar-images/add05.png "Verwenden des Dialog Felds zur Symbolleisten Anpassung")

Verwenden Sie hier Interface Builder, um das Suchfeld zu konfigurieren, und machen Sie es für den Code über eine Aktion oder ein Outlet verfügbar.

## <a name="built-in-toolbar-item-support"></a>Unterstützung integrierter Symbolleisten Elemente

Mehrere Cocoa-UI-Elemente interagieren standardmäßig mit Standard Symbolleisten Elementen. Ziehen Sie beispielsweise eine **Text Ansicht** in das Fenster der Anwendung, und positionieren Sie Sie, um den Inhalts Bereich auszufüllen:

[![Hinzufügen einer Textansicht zur Anwendung](toolbar-images/edit09.png "Hinzufügen einer Textansicht zur Anwendung")](toolbar-images/edit09-large.png#lightbox)

Speichern Sie das Dokument, kehren Sie zu Visual Studio für Mac um mit Xcode zu synchronisieren, führen Sie die Anwendung aus, geben Sie Text ein, wählen Sie ihn aus, und klicken Sie auf das Symbolleisten Element **Farben** Beachten Sie, dass die Textansicht automatisch mit der Farbauswahl verwendet wird:

![Integrierte Funktionen der Symbolleiste mit einer Textansicht und einer Farbauswahl](toolbar-images/edit10.png "Integrierte Funktionen der Symbolleiste mit einer Textansicht und einer Farbauswahl")

## <a name="using-images-with-toolbar-items"></a>Verwenden von Bildern mit Symbolleisten Elementen

Mithilfe eines **Bildsymbol leisten Elements**können alle dem **Ressourcen** Ordner hinzugefügten Bitmap-Bilder (und eine Buildaktion der **Bundle-Ressource**) als Symbol auf der Symbolleiste angezeigt werden:

1. Klicken Sie in Visual Studio für Mac im **Lösungspad**mit der rechten Maustaste auf den Ordner **Ressourcen** , und wählen Sie **Hinzufügen**  > **Dateien hinzufügen**aus.
2. Navigieren Sie im Dialogfeld **Dateien hinzufügen** zu den gewünschten Bildern, wählen Sie Sie aus, und klicken Sie auf die Schaltfläche **Öffnen** : 

    [![Hinzu zufügende Bilder auswählen](toolbar-images/edit11.png "Hinzu zufügende Bilder auswählen")](toolbar-images/edit11-large.png#lightbox)

3. Wählen Sie **Kopieren**aus, aktivieren Sie **dieselbe Aktion für alle ausgewählten Dateien verwenden**, und klicken Sie auf **OK**:

    ![Auswählen der Kopier Aktion für die hinzugefügten Images](toolbar-images/edit12.png "Auswählen der Kopier Aktion für die hinzugefügten Images")

4. Doppelklicken Sie im **Lösungspad**auf die Datei **MainWindow. XIb** , um Sie in Xcode zu öffnen.

5. Wählen Sie die Symbolleiste in der **Schnittstellen Hierarchie** aus, und klicken Sie auf ein Element, um das Dialogfeld Anpassung zu öffnen.

6. Ziehen Sie ein **Bildsymbol leisten Element** aus dem **Bibliotheks Inspektor** in den Bereich **zulässige Symbolleisten Elemente** der Symbolleiste: 

    ![Ein Bildsymbol leisten Element, das dem Bereich zulässige Symbolleisten Elemente hinzugefügt wurde](toolbar-images/edit14.png "Ein Bildsymbol leisten Element, das dem Bereich zulässige Symbolleisten Elemente hinzugefügt wurde")

7. Wählen Sie im **Attribut Inspektor**das Image aus, das soeben in Visual Studio für Mac hinzugefügt wurde: 

    ![Festlegen eines benutzerdefinierten Bilds für ein Symbolleisten Element](toolbar-images/edit15.png "Festlegen eines benutzerdefinierten Bilds für ein Symbolleisten Element")

8. Legen Sie die **Bezeichnung** auf "Papierkorb" und die **palettenbezeichnung** auf "Dokument löschen" fest: 

    ![Festlegen der Symbolleisten-Element Bezeichnung und der palettenbezeichnung](toolbar-images/edit16.png "Festlegen der Symbolleisten-Element Bezeichnung und der palettenbezeichnung")

9. Ziehen Sie ein **Symbolleisten Element** der Symbolleiste aus dem **Bibliotheks Inspektor** in den Bereich **zulässige Symbolleisten Elemente** der Symbolleiste: 

    [![Ein Trennzeichen-Symbolleisten Element, das zum Bereich zulässige Symbolleisten Elemente](toolbar-images/edit17.png "Ein Trennzeichen-Symbolleisten Element, das zum Bereich zulässige Symbolleisten Elemente")](toolbar-images/edit17-large.png#lightbox)

10. Ziehen Sie das Trennzeichen und das Element "Papierkorb" in den Standardbereich der **Symbolleisten Elemente** , und legen Sie die Reihenfolge der Symbolleisten Elemente von links nach rechts fest (Farben, Schriftarten, Trennzeichen, Papierkorb, flexibler Raum, Druck): 

    ![Die standardmäßigen Symbolleisten Elemente](toolbar-images/edit18.png "Die standardmäßigen Symbolleisten Elemente")

11. Speichern Sie die Änderungen, und kehren Sie zu Visual Studio für Mac um mit Xcode zu synchronisieren.

Führen Sie die Anwendung aus, um sicherzustellen, dass die neue Symbolleiste standardmäßig angezeigt wird:

![Eine Symbolleiste mit angepassten Standardelementen](toolbar-images/edit19.png "Eine Symbolleiste mit angepassten Standardelementen")

## <a name="exposing-toolbar-items-with-outlets-and-actions"></a>Verfügbar machen von Symbolleisten Elementen mit Outlets und Aktionen

Wenn Sie im Code auf eine Symbolleiste oder ein Symbolleisten Element zugreifen möchten, muss Sie an ein Outlet oder eine Aktion angefügt werden:

1. Doppelklicken Sie im **Lösungspad**auf **Main. Storyboard** , um es in Xcode zu öffnen.
2. Stellen Sie sicher, dass die benutzerdefinierte Klasse "windowcontroller" dem Hauptfenster Controller im **Identitäts Inspektor**zugewiesen wurde:

    [![Verwenden des Identitäts Inspektors zum Festlegen einer benutzerdefinierten Klasse für den Fenster Controller](toolbar-images/edit20a.png "Verwenden des Identitäts Inspektors zum Festlegen einer benutzerdefinierten Klasse für den Fenster Controller")](toolbar-images/edit20a-large.png#lightbox)

3. Wählen Sie als nächstes das Symbolleisten Element in der **Schnittstellen Hierarchie**aus: 

    ![Auswählen des Symbolleisten Elements in der Schnittstellen Hierarchie](toolbar-images/edit20.png "Auswählen des Symbolleisten Elements in der Schnittstellen Hierarchie")  

4. Öffnen Sie die **Ansicht "Assistant**", wählen Sie die Datei " **windowcontroller. h** " aus, und ziehen Sie die Steuerung von der Symbolleiste in die Datei " **windowcontroller. h** ".
5. Legen Sie den **Verbindungstyp** auf **Action**fest, geben Sie als **Name**"trashdocument" ein, und klicken Sie auf die Schaltfläche " **verbinden** ": 

    [![Konfigurieren einer Aktion für ein Symbolleisten Element](toolbar-images/edit23.png "Konfigurieren einer Aktion für ein Symbolleisten Element")](toolbar-images/edit23-large.png#lightbox)

6. Machen Sie die **Text Ansicht** als Outlet mit dem Namen "documenteditor" in der Datei " **ViewController. h** " verfügbar: 

    [![Konfigurieren eines Outlets für die Textansicht](toolbar-images/edit24.png "Konfigurieren eines Outlets für die Textansicht")](toolbar-images/edit24-large.png#lightbox)

7. Speichern Sie die Änderungen, und kehren Sie zu Visual Studio für Mac zurück, um mit Xcode zu synchronisieren.

Bearbeiten Sie in Visual Studio für Mac die Datei **ViewController.cs** , und fügen Sie den folgenden Code hinzu:

```csharp
public void EraseDocument() {
    documentEditor.Value = "";
}
```

Bearbeiten Sie als nächstes die Datei **WindowController.cs** , und fügen Sie am Ende der `WindowController`-Klasse den folgenden Code hinzu:

```csharp
[Export ("trashDocument:")]
void TrashDocument (NSObject sender) {

    var controller = ContentViewController as ViewController;
    controller.EraseDocument ();
}
```

Beim Ausführen der Anwendung wird das **Papierkorb** -Symbolleisten Element aktiv:

![Eine Symbolleiste mit einem aktiven Papierkorb Element](toolbar-images/edit25.png "Eine Symbolleiste mit einem aktiven Papierkorb Element")

Beachten Sie, dass das **Papierkorb** -Symbolleisten Element jetzt zum Löschen von Text verwendet werden kann.

## <a name="disabling-toolbar-items"></a>Deaktivieren von Symbolleisten Elementen

Um ein Element auf einer Symbolleiste zu deaktivieren, erstellen Sie eine benutzerdefinierte `NSToolbarItem` Klasse, und überschreiben Sie die `Validate`-Methode. Weisen Sie dann in Interface Builder den benutzerdefinierten Typ dem Element zu, das Sie aktivieren/deaktivieren möchten.

Um eine benutzerdefinierte `NSToolbarItem` Klasse zu erstellen, klicken Sie mit der rechten Maustaste auf das Projekt, und wählen Sie  > **neue Datei** **Hinzufügen** ... aus. Wählen Sie **Allgemein**  > **leere Klasse**aus, geben Sie "activatableitem" als **Name**ein, und klicken Sie auf die Schaltfläche " **neu** ": 

![Hinzufügen einer leeren Klasse in Visual Studio für Mac](toolbar-images/custom01.png "Hinzufügen einer leeren Klasse in Visual Studio für Mac")

Bearbeiten Sie anschließend die **ActivatableItem.cs** -Datei wie folgt:

```csharp
using System;

using Foundation;
using AppKit;

namespace MacToolbar
{
    [Register("ActivatableItem")]
    public class ActivatableItem : NSToolbarItem
    {
        public bool Active { get; set;} = true;

        public ActivatableItem ()
        {
        }

        public ActivatableItem (IntPtr handle) : base (handle)
        {
        }

        public ActivatableItem (NSObjectFlag  t) : base (t)
        {
        }

        public ActivatableItem (string title) : base (title)
        {
        }

        public override void Validate ()
        {
            base.Validate ();
            Enabled = Active;
        }
    }
}
```

Doppelklicken Sie auf **Main. Storyboard** , um es in Xcode zu öffnen. Wählen Sie das oben erstellte **Papierkorb** -Symbolleisten Element aus, und ändern Sie die Klasse in "activatableitem" im **Identitäts Inspektor**:

![Festlegen einer benutzerdefinierten Klasse für ein Symbolleisten Element](toolbar-images/custom02.png "Festlegen einer benutzerdefinierten Klasse für ein Symbolleisten Element")

Erstellen Sie ein Outlet namens `trashItem` für das **Papierkorb** -Symbolleisten Element. Speichern Sie die Änderungen, und kehren Sie zu Visual Studio für Mac um mit Xcode zu synchronisieren. Öffnen Sie abschließend **MainWindow.cs** , und aktualisieren Sie die `AwakeFromNib`-Methode wie folgt:

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Disable trash
    trashItem.Active = false;
}
```

Führen Sie die Anwendung aus, und beachten Sie, dass das **Papierkorb** Element jetzt auf der Symbolleiste deaktiviert ist:

![Eine Symbolleiste mit einem inaktiven Papierkorb Element](toolbar-images/custom03.png "Eine Symbolleiste mit einem inaktiven Papierkorb Element")

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde die Arbeit mit Symbolleisten und Symbolleisten Elementen in einer xamarin. Mac-Anwendung ausführlich erläutert. Es wurde beschrieben, wie Symbolleisten in der Interface Builder von Xcode erstellt und verwaltet werden, wie einige UI-Steuerelemente automatisch mit Symbolleisten Elementen arbeiten, wie C# Sie mit Symbolleisten im Code arbeiten und wie Sie Symbolleisten Elemente aktivieren und deaktivieren.

## <a name="related-links"></a>Verwandte Links

- [Mactoolbar (Beispiel)](https://docs.microsoft.com/samples/xamarin/mac-samples/mactoolbar)
- [Hello, Mac (Hallo, Mac)](~/mac/get-started/hello-mac.md)
- [Richtlinien für die Benutzeroberfläche für Symbolleisten](https://developer.apple.com/macos/human-interface-guidelines/windows-and-views/toolbars/)
- [Einführung in Symbolleisten](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/Toolbars/Toolbars.html)
