---
title: Symbolleisten
description: "Dieser Artikel beschreibt das Arbeiten mit Symbolleisten in einer Anwendung Xamarin.Mac. Außerdem werden beim Erstellen und Verwalten von Symbolleisten in Xcode und Benutzeroberflächen-Generator, diese in Code verfügbar machen und Arbeiten mit diesen programmgesteuert behandelt."
ms.topic: article
ms.prod: xamarin
ms.assetid: C8D228CE-C860-47E1-85FD-69864BF91F20
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 641f62d5e646607b2ff61db412a5defce63a211d
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2018
---
# <a name="toolbars"></a>Symbolleisten

_Dieser Artikel beschreibt das Arbeiten mit Symbolleisten in einer Anwendung Xamarin.Mac. Außerdem werden beim Erstellen und Verwalten von Symbolleisten in Xcode und Benutzeroberflächen-Generator, diese in Code verfügbar machen und Arbeiten mit diesen programmgesteuert behandelt._

Xamarin.Mac-Entwickler, die Arbeit mit Visual Studio für Mac haben Zugriff auf die gleiche UI-Steuerelemente MacOS Entwickler arbeiten mit Xcode, einschließlich das Symbolleisten-Steuerelement zur Verfügung. Da Xamarin.Mac direkt mit Xcode integriert ist, ist es möglich, Xcodes Benutzeroberflächen-Generator erstellen und Verwalten der Symbolleistenelemente zu verwenden. Diese Symbolleistenelemente können auch in c# erstellt werden.

Symbolleisten in MacOS im oberen Abschnitt eines Fensters hinzugefügt werden und bieten einfachen Zugriff auf Befehle, die im Zusammenhang mit seiner Funktionalität. Symbolleisten ausgeblendet, angezeigt oder durch eine Anwendung Benutzer angepasst werden können, und sie können Symbolleistenelemente auf verschiedene Weise darstellen.

Dieser Artikel behandelt die Grundlagen der Arbeit mit Symbolleisten und Symbolleistenelementen in einer Anwendung Xamarin.Mac. 

Bevor Sie fortfahren, lesen die [Hello, Mac](~/mac/get-started/hello-mac.md) Artikel – insbesondere die [Einführung in Xcode und Benutzeroberflächen-Generator](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) und [Steckdosen und Aktionen](~/mac/get-started/hello-mac.md#Outlets_and_Actions) Abschnitte – wie er behandelt wichtige Konzepte und Techniken, die in diesem Handbuch verwendet werden.

Außerdem sehen Sie sich die [Verfügbarmachen von C#-Klassen / Methoden für Objective-C-](~/mac/internals/how-it-works.md) Teil der [Xamarin.Mac Internals](~/mac/internals/how-it-works.md) Dokument. Es wird erläutert, die `Register` und `Export` Attribute, die zur Verbindung von C#-Klassen mit Objective-C-Klassen.

## <a name="introduction-to-toolbars"></a>Einführung in Symbolleisten

Jedes Fenster in einer Anwendung MacOS kann es sich um eine Symbolleiste enthalten:

![Ein Beispiel-Fenster mit einer Symbolleiste](toolbar-images/info01.png "eine Beispiel-Fenster mit einer Symbolleiste")

Symbolleisten bieten eine einfache Möglichkeit für häufig verwendete Features oder den Benutzern Ihrer Anwendung, rasch auf wichtige zuzugreifen. Z. B. bereitstellen eine Anwendung Dokument bearbeiten Symbolleistenelemente für die Textfarbe festlegen, Ändern der Schriftart oder das aktuelle Dokument zu drucken.

Symbolleisten können Elemente auf drei Arten anzeigen:

1. **Symbol und Text** 

     ![Eine Symbolleiste mit Symbole und Text](toolbar-images/info02.png "eine Symbolleiste mit Symbole und Text")

2. **Symbol "nur"** 

     ![Ein Symbol nur Symbolleiste](toolbar-images/info03.png "ein Symbol nur-Symbolleiste")

3. **Nur Text** 

     ![Eine nur-Text-Symbolleiste](toolbar-images/info04.png "eine nur-Text-Symbolleiste")

Wechseln Sie zwischen diesen Modi über Rechtsklick auf der Symbolleiste und einen Anzeigemodus aus dem Kontextmenü auswählen:

![Kontextmenü für eine Symbolleiste](toolbar-images/info05.png "das Kontextmenü für eine Symbolleiste")

Verwenden Sie das gleiche Menü, um die Symbolleiste an eine geringere Größe anzuzeigen:

![Eine Symbolleiste mit kleinen Symbolen](toolbar-images/info06.png "eine Symbolleiste mit kleinen Symbolen")

Klicken Sie im Menü kann auch zum Anpassen der Symbolleiste:

![Das Dialogfeld "verwendet, um eine Symbolleiste anzupassen](toolbar-images/info07.png "das Dialogfeld" verwendet, um eine Symbolleiste anzupassen.")

Beim Einrichten einer Symbolleiste in Xcodes Benutzeroberflächen-Generator kann ein Entwickler zusätzliche Symbolleistenelemente bereitstellen, die nicht Teil der standardmäßigen Konfiguration sind. Benutzer der Anwendung können dann die Symbolleiste anpassen, hinzufügen und Entfernen dieser vordefinierten Elemente nach Bedarf. Natürlich kann die Symbolleiste auf die Standardkonfiguration zurückgesetzt werden.

Die Symbolleiste zum automatischen Verbindung mit dem **Ansicht** Menü können Benutzer zu verbergen, anzeigen und anpassen:

![Symbolleistenelemente im Menü "Ansicht"](toolbar-images/info08.png "Symbolleistenelemente im Menü "Ansicht"")

Finden Sie unter der [integrierte Funktionen für Menüs](~/mac/user-interface/menu.md) Dokumentation.

Darüber hinaus, wenn die Symbolleiste im Benutzeroberflächen-Generator ordnungsgemäß konfiguriert ist, bleiben die Anwendung automatisch Symbolleiste Anpassungen über mehrere Starts der Anwendung erhalten.

In den nächsten Abschnitten dieses Handbuchs wird beschrieben, wie zum Erstellen und Verwalten von Symbolleisten mit Xcodes Benutzeroberflächen-Generator und im Code arbeiten.

## <a name="setting-a-custom-main-window-controller"></a>Festlegen eines benutzerdefinierten Hauptfenster-Controllers

Zum Verfügbarmachen von Benutzeroberflächenelementen für C#-Code durch Steckdosen und Aktionen, die app Xamarin.Mac muss einen benutzerdefinierte Fenster Controller verwenden:

1. Öffnen Sie die app-Storyboard in Xcodes Benutzeroberflächen-Generator.
2. Wählen Sie den Controller Fenster auf der Entwurfsoberfläche angezeigt.
3. Wechseln Sie zu der **Identität Inspektor** und geben Sie "WindowController" als die **Klassenname**: 

    [![Eine benutzerdefinierte Klasse Einstellungsname für den Controller Fenster](toolbar-images/windowcontroller01.png "Einstellungsname eine benutzerdefinierte Klasse für den Fenster-Controller")](toolbar-images/windowcontroller01-large.png) 

4. Die Änderungen zu speichern und zurück zu Visual Studio für Mac synchronisiert werden.
5. Ein **WindowController.cs** Datei wird hinzugefügt werden, um das Projekt in der **Lösung Pad** in Visual Studio für Mac: 

    ![WindowController.cs auswählen, in der Projektmappe Pad](toolbar-images/windowcontroller02.png "WindowController.cs Pad Lösung auswählen")

6. Öffnen Sie das Storyboard in Xcodes Benutzeroberflächen-Generator.
7. Die **WindowController.h** -Datei wird für die Verwendung verfügbar sein: 

    [![Die Datei WindowController.h](toolbar-images/windowcontroller03.png "der WindowController.h-Datei")](toolbar-images/windowcontroller03-large.png)

## <a name="creating-and-maintaining-toolbars-in-xcode"></a>Erstellen und Verwalten von Symbolleisten in Xcode

Symbolleisten werden erstellt und mit Xcodes Benutzeroberflächen-Generator verwaltet. Um eine Anwendung eine Symbolleiste hinzugefügt haben, bearbeiten Sie die app primären Storyboard (in diesem Fall **Main.storyboard**) durch Doppelklick im die **Lösung Pad**:

![Öffnen im Projektmappen-Pad Main.storyboard](toolbar-images/edit01.png "Main.storyboard in das Auffüllzeichen Projektmappe öffnen")

In der **Bibliothek Inspektor**, geben Sie "Tools" in der **Suchfeld** Anzeigen aller verfügbaren Symbolleistenelemente erleichtern:

![Der Inspektor Bibliothek gefiltert, um Symbolleistenelemente anzeigen](toolbar-images/edit02.png "die Bibliothek-Inspektors gefiltert, um Symbolleistenelemente anzeigen")

Ziehen Sie eine Symbolleiste auf das Fenster in der **Benutzeroberflächen-Editors**. Über die Symbolleiste ausgewählt haben, konfigurieren Sie dessen Verhalten durch Festlegen von Eigenschaften der **Attribute Inspektor**:

![Der Inspektor Attribute für eine Symbolleiste](toolbar-images/edit04.png "der Attribute-Inspektors für eine Symbolleiste")

Die folgenden Eigenschaften sind verfügbar:

1. **Anzeige** -steuert, ob die Symbolleiste angezeigt wird, Symbole, Text oder beides
2. **Bei Start sichtbar** -ausgewählt, die Symbolleiste wird standardmäßig angezeigt wird.
3. **Anpassbare** – Wenn aktiviert, Benutzer bearbeiten und die Symbolleiste anpassen können.
4. **Trennzeichen** – Wenn aktiviert, trennt eine schlanke horizontale Linie die Symbolleiste vom Inhalt des Fensters.
5. **Größe** -legt die Größe der Symbolleiste
6. **AutoSpeichern** -Wenn ausgewählt, wird die Anwendung des Benutzers Symbolleiste konfigurationsänderungen beibehalten, über die Anwendung gestartet wird.

Wählen Sie die **AutoSpeichern** aus, und behalten Sie die Standardeinstellungen für alle anderen Eigenschaften. 

Nach dem Öffnen der Symbolleiste in der **Schnittstellenhierarchie**, schalten Sie das Dialogfeld "anpassen", indem Symbolleistenelement auswählen:

![Anpassen der Symbolleiste](toolbar-images/edit05.png "Anpassen der Symbolleiste")

Verwenden Sie dieses Dialogfeld, um Eigenschaften für Elemente festlegen, die bereits Teil der Symbolleiste auf der Standardsymbolleiste in der Anwendung sind und zusätzliche Symbolleistenelemente für einen Benutzer auswählen, wenn die Symbolleiste anpassen bereitzustellen. Um Elemente auf der Symbolleiste hinzuzufügen, ziehen Sie sie aus der **Bibliothek Inspektor**:

![Der Inspektor Bibliothek](toolbar-images/edit06.png "der Bibliothek Inspektor")

Die folgenden Elemente in der Symbolleiste können hinzugefügt werden:

- **Abbildung Symbolleistenelement** -Symbolleistenelement durch ein benutzerdefiniertes Bild als Symbol.
- **Flexible Speicherplatz Symbolleistenelement** -Flexible Speicherplatz verwendet, um nachfolgende Symbolleistenelemente zu rechtfertigen. Z. B. eine oder mehrere Symbolleistenelemente gefolgt Symbolleistenelement flexible Speicherplatz, und Heften Sie ein anderes Symbolleistenelement würde das letzte Element auf der rechten Seite der Symbolleiste.
- **Speicherplatz Symbolleistenelement** -festen Abstand zwischen Elementen auf der Symbolleiste
- **Trennzeichen Symbolleistenelement** -sichtbar Trennzeichen zwischen zwei oder mehr Symbolleistenelemente, für die Gruppierung
- **Anpassen von Symbolleistenelement** – ermöglicht Benutzern das Anpassen der Symbolleiste
- **Drucken von Symbolleistenelement** -ermöglicht es Benutzern, die das geöffnete Dokument drucken
- **Anzeigen von Farben Symbolleistenelement** -zeigt die Farbauswahl Standardbetriebssystem: 

     ![Der System-Farbauswahl](toolbar-images/edit07.png "System-Farbauswahl")

- **Anzeigen der Schriftart Symbolleistenelement** -zeigt das Dialogfeld "Schriftart Standardbetriebssystem": 

     ![Die Schriftart](toolbar-images/edit08.png "der Schriftart")

> [!IMPORTANT]
> Später angezeigt wird, können viele standard Kakao UI-Steuerelemente wie z. B. Suchfelder segmentierte Steuerelemente und horizontaler Schieberegler auch eine Symbolleiste hinzugefügt werden.

### <a name="adding-an-item-to-a-toolbar"></a>Hinzufügen eines Elements zu einer Symbolleiste

Um ein Element zu einer Symbolleiste hinzuzufügen, wählen Sie die Symbolleiste in der **Schnittstellenhierarchie** , und klicken Sie auf eines seiner Elemente, verursacht im Dialogfeld "anpassen" angezeigt werden. Ziehen Sie anschließend ein neues Element aus der **Bibliothek Inspektor** auf die **Symbolleistenelemente zulässig** Bereich:

![Im Dialogfeld "Symbolleiste anpassen" der Symbolleistenelemente zulässig](toolbar-images/add01.png "die zulässige Symbolleistenelemente im Dialogfeld "Symbolleiste anpassen"")

Um sicherzustellen, dass ein neues Element Teil der Standardsymbolleiste ist, ziehen Sie es auf die **Symbolleiste Standardelemente** Bereich: 

![Neuanordnen von Symbolleistenelement durch Ziehen von](toolbar-images/add02.png "Neuanordnen von Symbolleistenelement durch Ziehen")

Um Standardelemente für die Symbolleiste neu anzuordnen, ziehen Sie sie nach links oder rechts.

Verwenden Sie als Nächstes die **Attribute Inspektor** Standardeigenschaften für das Element festlegen:

![Anpassen von mit den Attributen Inspektor Symbolleistenelement](toolbar-images/add03.png "mithilfe der Attribute Inspektor Symbolleistenelement anpassen")

Die folgenden Eigenschaften sind verfügbar:

- **Abbildname** -Bild, das als Symbol für das Element verwenden
- **Bezeichnung** -Text, der für das Element in der Symbolleiste angezeigt.
- **Palette Bezeichnung** -anzuzeigenden Text für das Element in der **Symbolleistenelemente zulässig** Bereich
- **Tag** – eine optionale, eindeutigen Bezeichners, die der Identifikation Identifizieren des Elements im Code.
- **Bezeichner** -definiert, die den Artikel "Toolbar". Ein benutzerdefinierter Wert kann verwendet werden, um ein Symbolleistenelement im Code auszuwählen.
- **Auswählbare** – Wenn dieses Kontrollkästchen aktiviert, wird das Element verhält Sie sich wie ein/aus-Schaltfläche.

> [!IMPORTANT]
> Fügen Sie ein Element aus, um die **Symbolleistenelemente zulässig** Bereich vorhanden, aber nicht der Standardsymbolleiste, um die Anpassungsoptionen für Benutzer bereitstellen. 

### <a name="adding-other-ui-controls-to-a-toolbar"></a>Hinzufügen von anderen UI-Steuerelementen zu einer Symbolleiste

Mehrere Kakao-Benutzeroberflächenautomatisierungs-Elemente wie z. B. die Suchfelder und segmentierte Steuerelemente können auch eine Symbolleiste hinzugefügt werden.

Um dies zu testen, öffnen Sie die Symbolleiste in der **Schnittstellenhierarchie** und wählen Sie ein Symbolleistenelement, um das Dialogfeld "Anpassung" zu öffnen. Ziehen Sie eine **suchen (Feld)** aus der **Bibliothek Inspektor** auf die **Symbolleistenelemente zulässig** Bereich:

![Verwenden das Dialogfeld "Symbolleiste-Anpassung"](toolbar-images/add05.png "verwenden das Dialogfeld "Symbolleiste-Anpassung"")

Hier verwenden Sie Schnittstelle-Generator zu suchen (Feld) konfigurieren und Verfügbarmachen von es Code über eine Aktion oder eine Steckdose.

## <a name="built-in-toolbar-item-support"></a>Unterstützung für integrierte Symbolleiste

Mehrere Kakao-Benutzeroberflächenautomatisierungs-Elementen interagieren Standardsymbolleiste Elemente standardmäßig. Ziehen Sie beispielsweise eine **Textansicht** auf das Fenster der Anwendung, und positionieren Sie ihn zum Auffüllen des Inhaltsbereichs:

[![Hinzufügen einer Textansicht zur Anwendung](toolbar-images/edit09.png "eine Textansicht zur Anwendung hinzufügen")](toolbar-images/edit09-large.png)

Speichern Sie das Dokument, zurück zu Visual Studio für Mac mit Xcode zu synchronisieren, führen Sie die Anwendung, Text eingeben, wählen Sie sie und klicken Sie auf die **Farben** Symbolleistenelement. Beachten Sie, dass der Textansicht automatisch mit dem Farbwähler funktioniert:

![Integrierte Symbolleiste-Funktionalität mit einer Text anzeigen und die Farbe der Datumsauswahl](toolbar-images/edit10.png "integrierte Symbolleiste-Funktionalität mit einer Text anzeigen und die Farbe der Auswahl")

## <a name="using-images-with-toolbar-items"></a>Verwenden von Bildern mit Symbolleistenelemente

Mithilfe einer **Image Symbolleistenelement**, Bitmap-Bild hinzugefügt der **Ressourcen** Ordner (und erhält einen Buildvorgang von **Bundle-Ressource**) können auf der Symbolleiste als Symbol angezeigt werden:

1. In Visual Studio für Mac in der **Lösung Pad**, mit der rechten Maustaste die **Ressourcen** Ordner, und wählen **hinzufügen** > **Hinzufügen von Dateien** .
2. Aus der **Hinzufügen von Dateien** Dialogfeld Feld, navigieren Sie zu der gewünschten Bilder, wählen Sie sie aus und klicken Sie auf die **öffnen** Schaltfläche: 

    [![Auswählen von Bildern hinzuzufügende](toolbar-images/edit11.png "Bilder hinzufügen auswählen")](toolbar-images/edit11-large.png)

3. Wählen Sie **Kopie**, überprüfen Sie **verwenden die gleiche Aktion für alle ausgewählten Dateien**, und klicken Sie auf **OK**:

    ![Auswählen des Kopiervorgangs für die hinzugefügten Bilder](toolbar-images/edit12.png "des Kopiervorgangs für die hinzugefügten Bilder auswählen")

4. In der **Lösung Pad**, doppelklicken Sie auf **MainWindow.xib** , die sie in Xcode geöffnet.

5. Wählen Sie die Symbolleiste in der **Schnittstellenhierarchie** , und klicken Sie auf eines seiner Elemente, um das Dialogfeld "Anpassung" zu öffnen.

6. Ziehen Sie ein **Image Symbolleistenelement** aus der **Bibliothek Inspektor** auf der Symbolleiste **Symbolleistenelemente zulässig** Bereich: 

    ![Ein Bild Symbolleistenelement dem Bereich Symbolleistenelemente zulässig](toolbar-images/edit14.png "Image Symbolleistenelement dem Bereich Symbolleistenelemente zulässig")

7. In der **Attribute Inspektor**, wählen Sie das Bild, das nur in Visual Studio für Mac hinzugefügt wurde: 

    ![Festlegen eines benutzerdefinierten Abbilds für Symbolleistenelement](toolbar-images/edit15.png "Festlegen eines benutzerdefinierten Abbilds für Symbolleistenelement")

8. Legen Sie die **Bezeichnung** in "Papierkorb" und der **Palette Bezeichnung** "Dokument löschen": 

    ![Festlegen der Symbolleiste Element Label und Palette Bezeichnung](toolbar-images/edit16.png "Element Festlegen der Symbolleiste, Beschriftung und die Palette-Bezeichnung")

9. Ziehen Sie eine **Trennzeichen Symbolleistenelement** aus der **Bibliothek Inspektor** auf der Symbolleiste **Symbolleistenelemente zulässig** Bereich: 

    [![Trennzeichen Symbolleistenelement hinzugefügt, in den Bereich der zulässigen Symbolleistenelemente](toolbar-images/edit17.png "ein Trennzeichen Symbolleistenelement dem Bereich Symbolleistenelemente zulässig")](toolbar-images/edit17-large.png)

10. Ziehen Sie das Trennzeichen-Element und das "Papierkorb"-Element, das die **Symbolleistenelemente Standard** Bereichs- und Menge von aus die Reihenfolge der Symbolleiste Elementen von links nach rechts wie folgt (Farben, Schriftarten, Trennzeichen, Papierkorb, Flexible Speicherplatz, Drucken): 

    ![Die Standardelemente für die Symbolleiste](toolbar-images/edit18.png "die Standardelemente für die Symbolleiste")

11. Speichert Änderungen und zurück zu Visual Studio für Mac mit Xcode zu synchronisieren.

Führen Sie die Anwendung aus, um sicherzustellen, dass die neue Symbolleiste wird standardmäßig angezeigt wird:

![Eine Symbolleiste mit benutzerdefinierten Standardelemente](toolbar-images/edit19.png "eine Symbolleiste mit angepassten Standard-Elemente")

## <a name="exposing-toolbar-items-with-outlets-and-actions"></a>Verfügbarmachen von Symbolleistenelemente mit Steckdosen und Aktionen

Um eine Symbolleiste oder ein Symbolleistenelement im Code zuzugreifen, muss es eine Steckdose oder eine Aktion zugeordnet werden:

1. In der **Lösung Pad**, doppelklicken Sie auf **Main.storyboard** , die sie in Xcode geöffnet.
2. Stellen Sie sicher, dass die benutzerdefinierte Klasse "WindowController", mit dem Controller im Hauptfenster in zugewiesen wurde der **Identität Inspektor**:

    [![Legen Sie eine benutzerdefinierte Klasse für den Controller Fenster mithilfe der Identity-Inspektor](toolbar-images/edit20a.png "legen Sie eine benutzerdefinierte Klasse für den Controller Fenster mithilfe der Identity-Inspektor")](toolbar-images/edit20a-large.png)

3. Wählen Sie als Nächstes das Symbolleistenelement in der **Schnittstellenhierarchie**: 

    ![Auswählen der Symbolleistenelement in der Schnittstellenhierarchie](toolbar-images/edit20.png "Symbolleistenelement in der Schnittstellenhierarchie auswählen")  

4. Öffnen der **Assistant Ansicht**, wählen die **WindowController.h** Datei- und Steuerelement ziehen Sie aus der Symbolleistenelement die **WindowController.h** Datei.
5. Festlegen der **Verbindung** Geben Sie auf **Aktion**, geben Sie "TrashDocument" für die **Namen**, und klicken Sie auf die **verbinden** Schaltfläche: 

    [![Konfigurieren eine Aktion für ein Symbolleistenelement](toolbar-images/edit23.png "eine Aktion für ein Symbolleistenelement konfigurieren")](toolbar-images/edit23-large.png)

6. Verfügbar machen die **Textansicht** als eine Steckdose namens "DocumentEditor" in der **ViewController.h** Datei: 

    [![Konfigurieren von einer Steckdose für Textansicht](toolbar-images/edit24.png "eine Steckdose für Textansicht konfigurieren")](toolbar-images/edit24-large.png)

7. Die Änderungen zu speichern und zurück zu Visual Studio für Mac mit Xcode zu synchronisieren.

In Visual Studio für Mac Bearbeiten der **ViewController.cs** Datei, und fügen Sie den folgenden Code hinzu:

```csharp
public void EraseDocument() {
    documentEditor.Value = "";
}
```

Als Nächstes Bearbeiten der **WindowController.cs** Datei, und fügen Sie den folgenden Code am Ende der `WindowController` Klasse:

```csharp
[Export ("trashDocument:")]
void TrashDocument (NSObject sender) {

    var controller = ContentViewController as ViewController;
    controller.EraseDocument ();
}
```

Beim Ausführen der Anwendung die **Papierkorb** Symbolleistenelement aktiv sein:

![Eine Symbolleiste mit einem aktiven Papierkorb Element](toolbar-images/edit25.png "eine Symbolleiste mit einem aktiven Papierkorb-Element")

Beachten Sie, dass die **Papierkorb** Symbolleistenelement kann jetzt verwendet werden, um Text zu löschen.

## <a name="disabling-toolbar-items"></a>Deaktivieren der Symbolleistenelemente

Um ein Element auf einer Symbolleiste zu deaktivieren, erstellen Sie eine benutzerdefinierte `NSToolbarItem` Klasse, und überschreiben die `Validate` Methode. Weisen Sie anschließend im Benutzeroberflächen-Generator, des benutzerdefinierten Typs das Element, das Sie aktivieren bzw. deaktivieren möchten.

Erstellen eine benutzerdefinierten `NSToolbarItem` Klasse, mit der rechten Maustaste auf das Projekt, und wählen Sie **hinzufügen** > **neue Datei...** . Wählen Sie **allgemeine** > **leere Klasse**, geben Sie "ActivatableItem" für die **Namen**, und klicken Sie auf die **neu** Schaltfläche: 

![Hinzufügen eine leere Klasse ist in Visual Studio für Mac](toolbar-images/custom01.png "hinzufügen eine leere Klasse ist in Visual Studio für Mac")

Als Nächstes Bearbeiten der **ActivatableItem.cs** Datei wie folgt:

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

Doppelklicken Sie auf **Main.storyboard** , die sie in Xcode geöffnet. Wählen Sie die **Papierkorb** Symbolleistenelement oben erstellt, und ändern Sie die Klasse in "ActivatableItem" in der **Identität Inspektor**:

![Festlegen einer benutzerdefinierten Klasse für ein Symbolleistenelement](toolbar-images/custom02.png "eine benutzerdefinierte Klasse für ein Symbolleistenelement festlegen")

Erstellen Sie eine Steckdose aufgerufen `trashItem` für die **Papierkorb** Symbolleistenelement. Speichert Änderungen und zurück zu Visual Studio für Mac mit Xcode zu synchronisieren. Öffnen Sie abschließend **MainWindow.cs** und aktualisieren Sie die `AwakeFromNib` Methode wie folgt:

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Disable trash
    trashItem.Active = false;
}
```

Führen Sie die Anwendung, und beachten Sie, dass die **Papierkorb** Element ist jetzt in der Symbolleiste deaktiviert:

![Eine Symbolleiste mit einem Element inaktiv Papierkorb](toolbar-images/custom03.png "eine Symbolleiste mit einem inaktiven Papierkorb-Element")

## <a name="summary"></a>Zusammenfassung

Dieser Artikel hat eine ausführliche Übersicht über das Arbeiten mit Symbolleisten und Symbolleistenelementen in einer Anwendung Xamarin.Mac übernommen. Dieser Mechanismus zum Erstellen und Verwalten von Symbolleisten in Xcodes Benutzeroberflächen-Generator, wie einige UI-Steuerelementen automatisch mit Symbolleistenelemente funktionieren, zum Arbeiten mit Symbolleisten in C#-Code und zum Aktivieren und Deaktivieren der Symbolleistenelemente beschrieben.


## <a name="related-links"></a>Verwandte Links

- [MacToolbar (Beispiel)](https://developer.xamarin.com/samples/mac/MacToolbar/)
- [Hello, Mac (Hallo, Mac)](~/mac/get-started/hello-mac.md)
- [Richtlinien für menschliche Benutzeroberfläche für Symbolleisten](https://developer.apple.com/macos/human-interface-guidelines/windows-and-views/toolbars/)
- [Einführung in Symbolleisten](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/Toolbars/Toolbars.html)
