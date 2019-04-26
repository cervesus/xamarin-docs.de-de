---
title: Symbolleisten in Xamarin.Mac
description: Dieser Artikel beschreibt das Arbeiten mit Symbolleisten in einer Xamarin.Mac-Anwendung. Hierin sind erstellen und Verwalten von Symbolleisten in Xcode und Interface Builder, für Code verfügbar zu machen und Programmgesteuertes Arbeiten mit ihnen.
ms.prod: xamarin
ms.assetid: C8D228CE-C860-47E1-85FD-69864BF91F20
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/14/2017
ms.openlocfilehash: 6cb17ae0f60390564a8aa6bdb64ea612aae51b55
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61295330"
---
# <a name="toolbars-in-xamarinmac"></a>Symbolleisten in Xamarin.Mac

_Dieser Artikel beschreibt das Arbeiten mit Symbolleisten in einer Xamarin.Mac-Anwendung. Hierin sind erstellen und Verwalten von Symbolleisten in Xcode und Interface Builder, für Code verfügbar zu machen und Programmgesteuertes Arbeiten mit ihnen._

Xamarin.Mac-Entwickler, die Arbeit mit Visual Studio für Mac haben Zugriff auf die gleiche UI-Steuerelemente, die für MacOS-Entwickler arbeiten mit Xcode, einschließlich das Symbolleisten-Steuerelement. Da Xamarin.Mac direkt in Xcode integriert ist, ist es möglich, verwenden Interface Builder von Xcode erstellen und verwalten die Elemente der Symbolleiste. Diese Elemente der Symbolleiste können auch in c# erstellt werden.

Symbolleisten in MacOS werden im oberen Abschnitt eines Fensters hinzugefügt und bieten einfachen Zugriff auf Befehle, die sich auf die Funktionalität beziehen. Symbolleisten ausgeblendet, angezeigt oder von einer Anwendung Benutzer angepasst werden können, und sie können Elemente der Symbolleiste auf verschiedene Weise präsentieren.

Dieser Artikel behandelt die Grundlagen der Arbeit mit der Symbolleisten und Symbolleistenelementen in einer Xamarin.Mac-Anwendung. 

Bevor Sie fortfahren, lesen Sie die [Hallo, Mac](~/mac/get-started/hello-mac.md) Artikel – insbesondere die [Einführung in Xcode und Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) und [Outlets und Aktionen](~/mac/get-started/hello-mac.md#outlets-and-actions) Abschnitte – wie behandelt wichtige Konzepte und Techniken, die in dieser Anleitung verwendet werden.

Außerdem sehen Sie sich die [Verfügbarmachen von c#-Klassen / Methoden mit Objective-C](~/mac/internals/how-it-works.md) Teil der [Xamarin.Mac-Interna](~/mac/internals/how-it-works.md) Dokument. Es wird erläutert, die `Register` und `Export` Attribute, die zur Verbindung von Klassen in c# mit Objective-C-Klassen.

## <a name="introduction-to-toolbars"></a>Einführung in Symbolleisten

Ein Fenster in einer MacOS-Anwendung kann es sich um eine Symbolleiste enthalten:

![Ein Beispiel-Fenster mit einer Symbolleiste](toolbar-images/info01.png "ein Beispiel-Fenster mit einer Symbolleiste")

Symbolleisten bieten eine einfache Möglichkeit für Benutzer Ihrer Anwendung, um schnell auf wichtige zuzugreifen oder häufig verwendete Funktionen. Z. B. gewährleistet eine Anwendung dokumentbearbeitung Elemente der Symbolleiste für die Textfarbe festlegen, Ändern der Schriftart oder das aktuelle Dokument zu drucken.

Symbolleisten können Elemente auf drei Arten angezeigt werden:

1. **Symbol und Text** 

     ![Eine Symbolleiste mit Symbolen und Text](toolbar-images/info02.png "eine Symbolleiste mit Symbolen und Text")

2. **Symbol "nur"** 

     ![Ein Symbol nur Symbolleiste](toolbar-images/info03.png "eine nur-Symbol-Symbolleiste")

3. **Nur Text** 

     ![Eine nur-Text-Symbolleiste](toolbar-images/info04.png "eine nur-Text-Symbolleiste")

Wechseln Sie zwischen diesen Modi, indem Sie mit der rechten Maustaste in der Symbolleiste und einen Anzeigemodus aus dem Kontextmenü auswählen:

![Kontextmenü für eine Symbolleiste](toolbar-images/info05.png "im Kontextmenü auf einer Symbolleiste")

Verwenden Sie das gleiche Menü auf der Symbolleiste auf eine kleinere Größe angezeigt:

![Eine Symbolleiste mit kleinen Symbolen](toolbar-images/info06.png "eine Symbolleiste mit kleinen Symbolen")

Klicken Sie im Menü kann auch zum Anpassen der Symbolleiste:

![Das Dialogfeld zum Anpassen einer Symbolleiste](toolbar-images/info07.png "das Dialogfeld zum Anpassen einer Symbolleiste")

Bei der Einrichtung einer Symbolleiste in Interface Builder von Xcode kann ein Entwickler zusätzliche Symbolleistenelemente bereitstellen, die nicht Teil der standardmäßigen Konfiguration sind. Benutzer der Anwendung können dann die Symbolleiste anpassen, hinzufügen und entfernen diese vordefinierte Elemente nach Bedarf. Natürlich kann die Symbolleiste auf die Standardkonfiguration zurückgesetzt werden.

Die Symbolleiste zum automatischen Verbindung mit der **Ansicht** -Menü, das ermöglicht es Benutzern, die sie ausblenden, anzeigen und anpassen:

![Symbolleiste-bezogene Elemente im Menü Ansicht](toolbar-images/info08.png "Symbolleistenelemente im Menü Ansicht")

Finden Sie unter den [Funktionen der integrierten Menüs](~/mac/user-interface/menu.md) Dokumentation.

Wenn die Symbolleiste in Interface Builder ordnungsgemäß konfiguriert ist, wird darüber hinaus benutzerdefinierten Symbolleisten von die Anwendung automatisch über mehrere Starts der Anwendung beibehalten.

In den nächsten Abschnitten dieses Handbuchs beschrieben, wie zum Erstellen und Verwalten von Symbolleisten mit Interface Builder von Xcode und im Code mit ihnen arbeiten.

## <a name="setting-a-custom-main-window-controller"></a>Festlegen eines benutzerdefinierten Hauptfenster-Controllers

Eine benutzerdefinierte fenstercontroller muss verwenden, um Benutzeroberflächenelemente für C#-Code mit Outlets und Aktionen, die Xamarin.Mac-app verfügbar zu machen:

1. Öffnen Sie die app Storyboard in Interface Builder von Xcode an.
2. Wählen Sie den fenstercontroller im auf der Entwurfsoberfläche angezeigt.
3. Wechseln Sie zu der **Identitätsinspektor** und geben Sie "WindowController" als die **Klassenname**: 

    [![Einstellungsname eine benutzerdefinierte Klasse für den fenstercontroller](toolbar-images/windowcontroller01.png "Einstellung einen Namen der benutzerdefinierten Klasse für den fenstercontroller")](toolbar-images/windowcontroller01-large.png#lightbox) 

4. Die Änderungen zu speichern und zurück zu Visual Studio für Mac, um zu synchronisieren.
5. Ein **WindowController.cs** Datei wird hinzugefügt werden, mit Ihrem Projekt in der **Lösungspad** in Visual Studio für Mac: 

    ![Wählen im Projektmappenpad WindowController.cs](toolbar-images/windowcontroller02.png "WindowController.cs im Projektmappenpad auswählen")

6. Öffnen Sie erneut das Storyboard in Interface Builder von Xcode.
7. Die **WindowController.h** Datei wird für die Verwendung verfügbar sein: 

    [![Die Datei WindowController.h](toolbar-images/windowcontroller03.png "der WindowController.h-Datei")](toolbar-images/windowcontroller03-large.png#lightbox)

## <a name="creating-and-maintaining-toolbars-in-xcode"></a>Erstellen und Verwalten von Symbolleisten in Xcode

Symbolleisten erstellt und verwaltet mit Interface Builder von Xcode. Um eine Symbolleiste, eine Anwendung hinzufügen, Bearbeiten der app primären Storyboard (in diesem Fall **"Main.Storyboard"**) durch Doppelklick im der **Lösungspad**:

![Öffnen "Main.Storyboard" im Projektmappenpad](toolbar-images/edit01.png "\"Main.Storyboard\" im Projektmappenpad öffnen")

In der **Bibliotheksinspektor**, geben Sie "Tools" in der **Suchfeld** zu erleichtern, damit alle die verfügbaren Symbolleistenelemente angezeigt:

![Im Bibliotheksinspektor festlegen, gefiltert, um Symbolleistenelemente anzeigen](toolbar-images/edit02.png "der Bibliotheksinspektor, gefiltert, um Elemente der Symbolleiste")

Ziehen Sie die Symbolleiste auf das Fenster in der **Schnittstellen-Editor**. Konfigurieren Sie mit der Symbolleiste ausgewählt ist, dessen Verhalten durch Festlegen von Eigenschaften der **Attributes Inspector**:

![Attributes Inspector für eine Symbolleiste](toolbar-images/edit04.png "dem Attributinspektor einer Symbolleiste")

Die folgenden Eigenschaften sind verfügbar:

1. **Anzeige** -steuert, ob die Symbolleiste Symbole, Text oder beides anzeigt.
2. **Angezeigt beim Start** -Wenn ausgewählt, für die Symbolleiste standardmäßig sichtbar ist.
3. **Anpassbare** -Wenn ausgewählt, Benutzer bearbeiten und Anpassen die Symbolleiste können.
4. **Trennzeichen** -Wenn ausgewählt, trennt eine schlanke horizontale Linie die Symbolleiste vom Inhalt des Fensters.
5. **Größe** – Festlegen der Größe der Symbolleiste
6. **Automatisches Speichern** -ausgewählt, die Anwendung bleibt bestehen, wenn Änderungen an der Konfiguration von eines Benutzers-Symbolleiste für die Anwendung gestartet wird.

Wählen Sie die **Autosave** aus, und lassen Sie alle anderen Eigenschaften die Standardeinstellungen. 

Nach dem Öffnen der Symbolleiste in der **Schnittstellenhierarchie**, dazu ein Symbolleistenelement das Dialogfeld für kartenanpassung öffnen:

![Anpassen der Symbolleiste](toolbar-images/edit05.png "Anpassen der Symbolleiste")

Verwenden Sie dieses Dialogfeld zum Festlegen von Eigenschaften für Elemente, die bereits Teil der Symbolleiste auf der Standardsymbolleiste für die Anwendung, und geben Sie zusätzliche Symbolleistenelemente für einen Benutzer auswählen, wenn die Symbolleiste anpasst. Um Elemente auf der Symbolleiste hinzuzufügen, ziehen Sie diese aus der **Bibliotheksinspektor**:

![Der Bibliotheksinspektor](toolbar-images/edit06.png "im Bibliotheksinspektor festlegen")

Die folgenden Symbolleistenelemente können hinzugefügt werden:

- **Abbildung Symbolleistenelement** -ein Symbolleistenelement mit einem benutzerdefinierten Image als Symbol.
- **Flexible Speicherplatz Symbolleistenelement** -Flexible Speicherplatz verwendet, um nachfolgende Symbolleistenelemente zu rechtfertigen. Z. B. eine oder mehrere Elemente der Symbolleiste gefolgt ein Symbolleistenelement flexible Speicherplatz, und Heften Sie eine andere Symbolleistenelement würde das letzte Element der rechten Seite der Symbolleiste.
- **Leerzeichen Symbolleistenelement** -feste Leerzeichen zwischen Elementen auf der Symbolleiste
- **Trennzeichen-Symbolleistenelement** -sichtbar Trennzeichen zwischen zwei oder mehr Elemente der Symbolleiste, für die Gruppierung
- **Anpassen des Symbolleistenelements** – ermöglicht das Anpassen die Symbolleiste
- **Drucken von Symbolleistenelement** -können Benutzer das geöffnete Dokument drucken
- **Anzeigen von Farben Symbolleistenelement** -wird die standardmäßige Farbauswahl angezeigt: 

     ![Die System-Farbauswahl](toolbar-images/edit07.png "der System-Farbauswahl")

- **Anzeigen der Schriftart Symbolleistenelement** -zeigt das Standarddialogfeld Schriftart: 

     ![Die Auswahl der Schriftart](toolbar-images/edit08.png "die Schriftart-Auswahl")

> [!IMPORTANT]
> Wie weiter unten angezeigt wird, können viele standard-Cocoa-UI-Steuerelemente wie z. B. Suchfelder und segmentierte Steuerelemente horizontalen Schieberegler auch eine Symbolleiste hinzugefügt werden.

### <a name="adding-an-item-to-a-toolbar"></a>Hinzufügen eines Elements zu einer Symbolleiste

Um ein Element zu einer Symbolleiste hinzufügen, wählen Sie auf die Symbolleiste in der **Schnittstellenhierarchie** , und klicken Sie auf eines seiner Elemente, sodass im Dialogfeld "anpassen" angezeigt werden. Ziehen Sie jetzt ein neues Element aus der **Bibliotheksinspektor** auf die **Symbolleistenelemente zulässig** Bereich:

![Die zulässige Symbolleistenelemente im Dialogfeld "Symbolleiste anpassen"](toolbar-images/add01.png "die zulässigen Elemente der Symbolleiste im Dialogfeld \"Symbolleiste anpassen\"")

Um sicherzustellen, dass ein neues Element Teil der Standardsymbolleiste ist, ziehen Sie dann auf die **Symbolleiste Standardelemente** Bereich: 

![Neuanordnen von ein Symbolleistenelement durch Ziehen von](toolbar-images/add02.png "Neuanordnung ein Symbolleistenelement durch Ziehen")

Um Standardelemente für die Symbolleiste neu anzuordnen, ziehen Sie sie nach links oder rechts.

Verwenden Sie als Nächstes die **Attributes Inspector** Standardeigenschaften für das Element festlegen:

![Anpassen eines Symbolleistenelements mit Attributes Inspector](toolbar-images/add03.png "ein Symbolleistenelement mit Attributes Inspector anpassen")

Die folgenden Eigenschaften sind verfügbar:

- **ImageName** -Bild, das als Symbol für das Element verwendet wird
- **Bezeichnung** -Text, der für das Element in der Symbolleiste angezeigt.
- **Palette Bezeichnung** -Text für das Element in der **Symbolleistenelemente zulässig** Bereich
- **Tag** – ein optionaler, eindeutiger Bezeichner, mit denen das Element im Code identifiziert.
- **Bezeichner** -definiert den Typ der Symbolleisten-Element. Ein benutzerdefinierter Wert kann verwendet werden, um ein Symbolleistenelement im Code auszuwählen.
- **Auswählbare** – Wenn aktiviert, wird das Element fungiert, wie ein/aus-Schaltfläche.

> [!IMPORTANT]
> Fügen Sie ein Element, das die **Symbolleistenelemente zulässig** Bereich vorhanden, aber nicht der Standardsymbolleiste, um die Anpassungsoptionen für Benutzer bereitstellen. 

### <a name="adding-other-ui-controls-to-a-toolbar"></a>Andere UI-Steuerelemente hinzufügen zu einer Symbolleiste.

Mehrere Cocoa-UI-Elemente wie z. B. Suchfelder und segmentierte Steuerelemente können auch eine Symbolleiste hinzugefügt werden.

Um dies zu testen, öffnen Sie die Symbolleiste in der **Schnittstellenhierarchie** , und wählen Sie ein Symbolleistenelement, um das Dialogfeld für kartenanpassung öffnen. Ziehen Sie eine **suchen (Feld)** aus der **Bibliotheksinspektor** auf die **Symbolleistenelemente zulässig** Bereich:

![Verwenden das Dialogfeld zum Anpassen der Symbolleiste](toolbar-images/add05.png "mithilfe der Symbolleiste-Dialogfeld für kartenanpassung")

Verwenden Sie Interface Builder hier konfigurieren das Suchfeld ein, und machen ihn Code über eine Aktion oder Outlet verfügbar.

## <a name="built-in-toolbar-item-support"></a>Unterstützung für integrierte Symbolleiste-Element

Mehrere Cocoa-UI-Elemente interagieren Sie mit der Standardsymbolleiste Elemente in der Standardeinstellung. Ziehen Sie beispielsweise eine **Textansicht** auf das Fenster der Anwendung und positionieren Sie ihn, um den Inhaltsbereich zu füllen:

[![Hinzufügen einer Textansicht an die Anwendung](toolbar-images/edit09.png "eine Textansicht an die Anwendung hinzufügen")](toolbar-images/edit09-large.png#lightbox)

Speichern Sie das Dokument, wechseln Sie zurück zur Visual Studio für Mac mit Xcode synchronisieren, die Anwendung auszuführen, geben Text ein, wählen Sie ihn und klicken Sie auf die **Farben** Symbolleistenelement. Beachten Sie, dass die Textansicht automatisch mit dem Farbwähler funktioniert:

![Integrierte Symbolleiste-Funktionalität mit einer Text anzeigen und die Farbe der Auswahl](toolbar-images/edit10.png "integrierte Symbolleiste-Funktionalität mit einer Ansicht und die Farbe Auswahl von Text")

## <a name="using-images-with-toolbar-items"></a>Verwenden von Bildern mit Symbolleistenelemente

Mit einer **Image Symbolleistenelement**, alle Bitmap-Bild hinzugefügt der **Ressourcen** Ordner (und eine Buildaktion von **Bundle-Ressource**) können auf der Symbolleiste als Symbol angezeigt werden:

1. In Visual Studio für Mac in der **Lösungspad**, mit der rechten Maustaste die **Ressourcen** Ordner, und wählen **hinzufügen** > **Hinzufügen von Dateien** .
2. Von der **Hinzufügen von Dateien** Dialogfeld, navigieren Sie zu den gewünschten Bildern, wählen Sie sie aus, und auf die **öffnen** Schaltfläche: 

    [![Auswählen von Images hinzufügen](toolbar-images/edit11.png "auswählen von Images hinzufügen")](toolbar-images/edit11-large.png#lightbox)

3. Wählen Sie **Kopie**, überprüfen Sie **verwenden die gleiche Aktion für alle ausgewählten Dateien**, und klicken Sie auf **OK**:

    ![Bei Auswahl der Kopieraktion für die hinzugefügten Bilder](toolbar-images/edit12.png "des Kopiervorgangs für die hinzugefügten Bilder auswählen")

4. In der **Lösungspad**, doppelklicken Sie auf **MainWindow.xib** , die sie in Xcode geöffnet.

5. Wählen Sie die Symbolleiste in der **Schnittstellenhierarchie** , und klicken Sie auf eines seiner Elemente, um das Dialogfeld für kartenanpassung öffnen.

6. Ziehen Sie ein **Image Symbolleistenelement** aus der **Bibliotheksinspektor** auf der Symbolleiste auf die **Symbolleistenelemente zulässig** Bereich: 

    ![Ein Image-Symbolleiste-Element hinzugefügt wird, in den Bereich der zulässigen Symbolleistenelemente](toolbar-images/edit14.png "ein Bildelement Symbolleiste hinzugefügt wird, in den Bereich der Symbolleistenelemente zulässig")

7. In der **Attributes Inspector**, wählen Sie das Bild, das nur in Visual Studio für Mac hinzugefügt wurde: 

    ![Festlegen eines benutzerdefinierten Images für ein Symbolleistenelement](toolbar-images/edit15.png "Festlegen eines benutzerdefinierten Images für ein Symbolleistenelement")

8. Legen Sie die **Bezeichnung** in "Den Papierkorb legen" und die **Palette Bezeichnung** auf "Dokument löschen": 

    ![Festlegen der Symbolleiste des Elements, Bezeichnung und Palette Bezeichnung](toolbar-images/edit16.png "Element Festlegen der Symbolleiste, Bezeichnung und Palette Bezeichnung")

9. Ziehen Sie eine **Trennzeichen Symbolleistenelement** aus der **Bibliotheksinspektor** auf der Symbolleiste auf die **Symbolleistenelemente zulässig** Bereich: 

    [![Ein Trennzeichenelement. Symbolleiste hinzugefügt wird, in den Bereich der Symbolleistenelemente zulässig](toolbar-images/edit17.png "ein Trennzeichenelement. Symbolleiste hinzugefügt wird, in den Bereich der Symbolleistenelemente zulässig")](toolbar-images/edit17-large.png#lightbox)

10. Ziehen Sie das trennzeichenelement und das "Papierkorb"-Element, das die **Symbolleistenelemente Standard** Bereichs- und Menge von aus die Reihenfolge der Symbolleiste Elementen von links nach rechts, die wie folgt (Farben, Schriftarten, Trennzeichen, den Papierkorb, Flexible Speicherplatz, Drucken): 

    ![Die Standardelemente für die Symbolleiste](toolbar-images/edit18.png "die Standardelemente für die Symbolleiste")

11. Speichert Änderungen und zurück zu Visual Studio für Mac mit Xcode synchronisiert.

Führen Sie die Anwendung aus, um sicherzustellen, dass die neue Symbolleiste standardmäßig angezeigt wird:

![Eine Symbolleiste mit angepassten Standard-Elementen](toolbar-images/edit19.png "eine Symbolleiste mit angepassten Standard-Elemente")

## <a name="exposing-toolbar-items-with-outlets-and-actions"></a>Verfügbarmachen von Symbolleistenelemente mit Outlets und Aktionen

Um eine Symbolleiste oder Symbolleistenelement im Code zugreifen, müssen sie eines outlets oder eine Aktion zugeordnet werden:

1. In der **Lösungspad**, doppelklicken Sie auf **"Main.Storyboard"** , die sie in Xcode geöffnet.
2. Stellen Sie sicher, dass die benutzerdefinierte Klasse, die den Controller im Hauptfenster auf "WindowController" zugewiesen wurde die **Identitätsinspektor**:

    [![Legen Sie eine benutzerdefinierte Klasse für den fenstercontroller mithilfe der Identitätsinspektor](toolbar-images/edit20a.png "mit der Identity-Inspektor eine benutzerdefinierte Klasse für den fenstercontroller festlegen")](toolbar-images/edit20a-large.png#lightbox)

3. Wählen Sie als Nächstes Symbolleistenelement der **Schnittstellenhierarchie**: 

    ![Wählen in der Schnittstellenhierarchie Symbolleistenelement](toolbar-images/edit20.png "Symbolleistenelement in der Hierarchie der Benutzeroberfläche auswählen")  

4. Öffnen der **Assistant Ansicht**, wählen die **WindowController.h** Datei- und Steuerelement ziehen aus dem Symbolleistenelement auf die **WindowController.h** Datei.
5. Festlegen der **Verbindung** Geben Sie auf **Aktion**, geben Sie "TrashDocument" für die **Namen**, und klicken Sie auf der **verbinden** Schaltfläche: 

    [![Konfigurieren eine Aktion für ein Symbolleistenelement](toolbar-images/edit23.png "konfigurieren eine Aktion für ein Symbolleistenelement")](toolbar-images/edit23-large.png#lightbox)

6. Machen die **Textansicht** als eines outlets namens "DocumentEditor" in der **"viewcontroller.h"** Datei: 

    [![Konfigurieren eines outlets für die Textansicht](toolbar-images/edit24.png "Konfigurieren eines outlets für die Textansicht")](toolbar-images/edit24-large.png#lightbox)

7. Die Änderungen zu speichern und zurück zu Visual Studio für Mac mit Xcode synchronisiert.

Bearbeiten in Visual Studio für Mac die **ViewController.cs** Datei, und fügen Sie den folgenden Code hinzu:

```csharp
public void EraseDocument() {
    documentEditor.Value = "";
}
```

Als Nächstes bearbeiten Sie die **WindowController.cs** Datei, und fügen Sie den folgenden Code am Ende der `WindowController` Klasse:

```csharp
[Export ("trashDocument:")]
void TrashDocument (NSObject sender) {

    var controller = ContentViewController as ViewController;
    controller.EraseDocument ();
}
```

Beim Ausführen der Anwendung, die **Papierkorb** Symbolleistenelement aktiv sein:

![Eine Symbolleiste mit einem aktiven Papierkorb Element](toolbar-images/edit25.png "eine Symbolleiste mit einem aktiven Papierkorb-Element")

Beachten Sie, dass die **Papierkorb** Symbolleistenelement kann jetzt verwendet werden, um Text zu löschen.

## <a name="disabling-toolbar-items"></a>Deaktivieren die Elemente der Symbolleiste

Um ein Element in einer Symbolleiste zu deaktivieren, erstellen Sie eine benutzerdefinierte `NSToolbarItem` Klasse, und überschreiben die `Validate` Methode. Weisen Sie anschließend in Interface Builder, des benutzerdefinierten Typs des Elements, die Sie zum Aktivieren/deaktivieren möchten.

Erstellen eine benutzerdefinierten `NSToolbarItem` Klasse, mit der rechten Maustaste auf das Projekt, und wählen Sie **hinzufügen** > **neue Datei...** . Wählen Sie **allgemeine** > **leere Klasse**, geben Sie "ActivatableItem" für die **Namen**, und klicken Sie auf die **neu** Schaltfläche: 

![Hinzufügen einer leeren Klasse in Visual Studio für Mac](toolbar-images/custom01.png "hinzufügen eine leere Klasse in Visual Studio für Mac")

Als Nächstes bearbeiten Sie die **ActivatableItem.cs** Datei wie folgt:

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

Doppelklicken Sie auf **"Main.Storyboard"** , die sie in Xcode geöffnet. Wählen Sie die **Papierkorb** Symbolleistenelement oben erstellt haben, und ändern Sie die Klasse in "ActivatableItem", der **Identitätsinspektor**:

![Festlegen einer benutzerdefinierten Klasse für ein Symbolleistenelement](toolbar-images/custom02.png "Festlegen einer benutzerdefinierten Klasse für ein Symbolleistenelement")

Erstellen ein outlets namens `trashItem` für die **Papierkorb** Symbolleistenelement. Speichert Änderungen und zurück zu Visual Studio für Mac mit Xcode synchronisiert. Öffnen Sie abschließend **MainWindow.cs** und aktualisieren Sie die `AwakeFromNib` Methode wie folgt:

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Disable trash
    trashItem.Active = false;
}
```

Führen Sie die Anwendung, und beachten Sie, dass die **Papierkorb** Element ist jetzt in der Symbolleiste deaktiviert:

![Eine Symbolleiste mit einem inaktiven Papierkorb Element](toolbar-images/custom03.png "eine Symbolleiste mit einem inaktiven Papierkorb-Element")

## <a name="summary"></a>Zusammenfassung

Dieser Artikel hat es sich um einen detaillierten Einblick in die Arbeit mit der Symbolleisten und Symbolleistenelementen in einer Xamarin.Mac-Anwendung geführt. Es wurde beschrieben, wie zum Erstellen und Verwalten von Symbolleisten in Interface Builder von Xcode, einige Benutzeroberflächen-Steuerelemente automatisch Symbolleistenelemente Funktionsweise mit, wie Sie mit der Symbolleisten im C#-Code arbeiten und wie beim Aktivieren und deaktivieren die Elemente der Symbolleiste.


## <a name="related-links"></a>Verwandte Links

- [MacToolbar (Beispiel)](https://developer.xamarin.com/samples/mac/MacToolbar/)
- [Hello, Mac (Hallo, Mac)](~/mac/get-started/hello-mac.md)
- [Human Interface Guidelines für Symbolleisten](https://developer.apple.com/macos/human-interface-guidelines/windows-and-views/toolbars/)
- [Einführung in Symbolleisten](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/Toolbars/Toolbars.html)
