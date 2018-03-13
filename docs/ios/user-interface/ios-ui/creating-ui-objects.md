---
title: "Erstellen von Benutzeroberflächenobjekten"
ms.topic: article
ms.prod: xamarin
ms.assetid: 4D6B136C-744A-4936-8655-A77E62BA7A60
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: e7a61dcf2cf2fabf575e30ef402121db3bea7912
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="creating-user-interface-objects"></a>Erstellen von Benutzeroberflächenobjekten

Apple-Gruppen im Zusammenhang mit Funktionen in "Frameworks" die Xamarin.iOS Namespaces entsprechen. `UIKit` ist der Namespace, der alle Steuerelemente der Benutzeroberfläche für iOS enthält.

Wenn Code ein Steuerelement der Benutzeroberfläche, z. B. eine Bezeichnung oder eine Schaltfläche verweisen muss Denken Sie daran, und schließen Sie folgende Anweisung:

```csharp
using UIKit;
```


Alle Steuerelemente, die in diesem Kapitel beschriebenen sind im UIKit-Namespace und Namen der Steuerelementklasse jeder Benutzer hat die `UI` Präfix.

Sie können Steuerelemente der Benutzeroberfläche und Layouts auf drei Arten bearbeiten:

-  **[Xamarin iOS Designer](~/ios/user-interface/designer/index.md)**  – verwenden die Xamarin integrierten Layout-Designers können Sie Bildschirme entwerfen. Doppelklicken Sie auf Storyboard oder XIB-Dateien mit der integrierten Designer bearbeitet werden.
-  **Xcode-Schnittstelle-Generator** – ziehen Sie Steuerelemente auf Ihre Bildschirmlayouts mit Schnittstelle-Generator. Die Storyboard oder XIB-Datei in Xcode geöffnet, mit der rechten Maustaste in die Datei in die **Lösung Pad** auswählen und **Öffnen mit > Xcode Schnittstelle-Generator**.
-  **Mithilfe von c#** – Steuerelemente auch programmgesteuert mit Code erstellt und hinzugefügt werden können, die Hierarchie anzeigen.

Neue Storyboard und XIB Dateien können hinzugefügt werden, indem Sie mit der rechten Maustaste auf ein iOS-Projekt und **hinzufügen > neuen Datei...** .

Steuerelementeigenschaften, unabhängig davon, welche Methode Sie verwenden, und Ereignisse können immer noch mit c# bearbeitet werden, in Ihrer Anwendungslogik.

## <a name="using-xamarin-ios-designer"></a>Mithilfe von Xamarin iOS-Designer

Um eine Benutzeroberfläche erstellen, in der iOS-Designer zu starten, doppelklicken Sie auf eine Storyboarddatei. Steuerelemente auf der Entwurfsoberfläche aus gezogen werden können die **Toolbox** wie unten gezeigt:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

 [![](creating-ui-objects-images/image2b.png "Toolbox mit Leerstellen auffüllen")](creating-ui-objects-images/image2b.png#lightbox)
 
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

 [![](creating-ui-objects-images/image2b-vs.png "Toolbox Pad - Visual Studio")](creating-ui-objects-images/image2b.png#lightbox)
 
-----

Wenn ein Steuerelement aktiviert ist, auf der Entwurfsoberfläche der **Eigenschaften Pad** zeigt die Attribute für dieses Steuerelement. Die **Widget > Identität > Name** Feld, das im folgenden Screenshot aufgefüllt wird, dient als die *Steckdose* Name. Wie Sie das Steuerelement in C#-verweisen können sieht folgendermaßen aus:

 [![](creating-ui-objects-images/image3b.png "Eigenschaften-Widget mit Leerstellen auffüllen")](creating-ui-objects-images/image3b.png#lightbox)

Eine Vertiefung in der iOS-Designer verwenden, finden Sie in der [Einführung in die iOS-Designer](~/ios/user-interface/designer/introduction.md) Handbuch.

## <a name="using-xcode-interface-builder"></a>Mithilfe von Xcode-Schnittstelle-Generator

Wenn Sie mit der Verwendung von Benutzeroberflächen-Generator nicht vertraut sind, finden Sie in der Apple- [Schnittstelle-Generator](https://developer.apple.com/xcode/interface-builder/) Dokumente.

Um ein Storyboard in Xcode zu öffnen, mit der rechten Maustaste das Kontextmenü für die Storyboarddatei zugreifen, und wählen zum Öffnen der **Xcode Schnittstelle-Generator**:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

 [![](creating-ui-objects-images/imagexcode.png "Kontextmenü für die Storyboard - Xcode")](creating-ui-objects-images/imagexcode.png#lightbox)
 
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](creating-ui-objects-images/imagexcode-vs.png "Kontextmenü für die Storyboard - Xcode")](creating-ui-objects-images/imagexcode-vs.png#lightbox)

-----

Steuerelemente auf der Entwurfsoberfläche aus gezogen werden können die **Objektbibliothek** unten dargestellt:

 [![](creating-ui-objects-images/image5a.png "Xcode-Objektbibliothek")](creating-ui-objects-images/image5a.png#lightbox)

Beim Entwerfen der Benutzeroberflächenautomatisierungs mit dem Benutzeroberflächen-Generator müssen Sie erstellen eine **Nachrichtenplattform** für jedes Steuerelement, das Sie in c# verweisen möchten. Dies erfolgt durch das Einschalten der **Assistant Editor** über das Center **Editor** Schaltfläche auf der Xcode-Symbolleisten-Schaltfläche:

 [![](creating-ui-objects-images/image6a.png "Assistenten-Editor-Schaltfläche")](creating-ui-objects-images/image6a.png#lightbox)

Klicken Sie auf ein Benutzerobjekt-Schnittstelle. Klicken Sie dann **Steuerelement Drag &** in die .h-Datei. Um ** Steuerelement ziehen **, halten Sie die STRG-Taste und halten Sie über die Benutzer-Schnittstellenobjekt, das Sie für den Ausgang (oder die Aktion) erstellen. Halten Sie STRG gedrückt und Sie in der Headerdatei ziehen. Fertig stellen unten ziehen die `@interface` Definition. Mit einer Beschriftung Steckdose einfügen oder Steckdose-Auflistung, sollte eine blaue Linie angezeigt werden, wie im folgenden Screenshot dargestellt.

Beim Freigeben des auf werden Sie aufgefordert, einen Namen für den Ausgang anzugeben, die verwendet werden soll, erstellen Sie eine C#-Eigenschaft, die auf die verwiesen werden kann, im Code:

 [![](creating-ui-objects-images/image8a.png "Erstellen eine Steckdose")](creating-ui-objects-images/image8a.png#lightbox)

Weitere Informationen wie Xcodes Benutzeroberflächen-Generator in Visual Studio für Mac integriert, finden Sie in der [Xib Codegenerierung](~/ios/internals/xib-code-generation.md#generated) Dokument.

##  <a name="using-c"></a>Mithilfe von c#

Wenn Sie programmgesteuert ein Benutzerobjekt-Schnittstelle mithilfe von c# (in einer Sicht oder die View-Controller, z. B.) erstellen möchten, gehen Sie folgendermaßen vor:

-  Deklarieren Sie eine Ebene Klassenfeld für das Benutzerobjekt-Schnittstelle. Erstellen des Steuerelements selbst einmal in `ViewDidLoad` beispielsweise. Das Objekt kann dann in der gesamten Lebenszyklusmethoden des Controllers anzeigen (z. b. verwiesen werden
`ViewWillAppear`) angezeigt wird.
-  Erstellen einer `CGRect` , die den Frame des Steuerelements (seine X- und Y-Koordinaten auf dem Bildschirm "" sowie die Breite und Höhe) definiert. Sie müssen sicherstellen, dass eine `using CoreGraphics` für diese Richtlinie.
-  Rufen Sie den Konstruktor zum Erstellen, und weisen Sie das Steuerelement.
-  Legen Sie Eigenschaften oder Ereignishandler.
-  Rufen Sie `Add()` der Hierarchie anzeigen des Steuerelements hinzu.

Hier ist ein einfaches Beispiel zum Erstellen einer `UILabel` in einem Controller Sicht mithilfe von c#:

```csharp
UILabel label1;
public override void ViewDidLoad () {
    base.ViewDidLoad ();
    var frame = new CGRect(10, 10, 300, 30);
    label1 = new UILabel(frame);
    label1.Text = "New Label";
    View.Add (label1);
}
```

<a name="partial_classes" />

## <a name="using-c-and-storyboards"></a>Verwenden von C#- und Storyboards

Wenn auf die Entwurfsoberfläche View-Controller hinzugefügt werden, werden zwei entsprechende C#-Dateien im Projekt erstellt. In diesem Beispiel `ControlsViewController.cs` und `ControlsViewController.designer.cs` automatisch erstellt wurden:

 [![](creating-ui-objects-images/image9b.png "Partielle ViewController-Klasse")](creating-ui-objects-images/image9b.png#lightbox)

Die `MainViewController.cs` Datei soll *Codes*. Dies ist, wenn die `View` Lebenszyklusmethoden wie z. B. `ViewDidLoad` und `ViewWillAppear` werden implementiert und in dem Sie Ihre eigenen Eigenschaften, Felder und Methoden hinzufügen können.

Die `ControlsViewController.designer.cs` ist generierter Code enthält eine partielle Klasse. Name eines Steuerelements auf der Entwurfsoberfläche Entwurfsoberfläche in Visual Studio für Mac, oder eine Steckdose oder die Aktion in Xcode, eine entsprechende Eigenschaft oder die partielle Methode erstellen, werden der Designer (designer.cs)-Datei hinzugefügt. Der folgende Code zeigt ein Beispiel für zwei Schaltflächen und ein Textansicht generiert Code, in denen eine der Schaltflächen verfügt auch über eine `TouchUpInside` Ereignis.

Aktivieren Sie diese Elemente der partiellen Klasse Code zum Verweisen auf die Steuerelemente und reagieren auf die Aktionen, die auf der Entwurfsoberfläche deklariert werden:

```csharp
[Register ("ControlsViewController")]
    partial class ControlsViewController
    {
        [Outlet]
        [GeneratedCodeAttribute ("iOS Designer", "1.0")]
        UIKit.UIButton Button1 { get; set; }

        [Outlet]
        [GeneratedCodeAttribute ("iOS Designer", "1.0")]
        UIKit.UIButton Button2 { get; set; }

        [Outlet]
        [GeneratedCodeAttribute ("iOS Designer", "1.0")]
        UIKit.UITextField textfield1 { get; set; }

        [Action ("button2_TouchUpInside:")]
        [GeneratedCodeAttribute ("iOS Designer", "1.0")]
        partial void button2_TouchUpInside (UIButton sender);

        void ReleaseDesignerOutlets ()
        {
            if (Button1 != null) {
                Button1.Dispose ();
                Button1 = null;
            }
            if (Button2 != null) {
                Button2.Dispose ();
                Button2 = null;
            }
            if (textfield1 != null) {
                textfield1.Dispose ();
                textfield1 = null;
            }
        }
    }
}
```

Die `designer.cs` Datei sollte nicht manuell bearbeitet werden – die IDE (Visual Studio für Mac oder Visual Studio) dient, halten sie mit dem Storyboard synchronisiert.

Wenn der Benutzeroberflächenobjekte programmgesteuert hinzugefügt werden eine `View` oder `ViewController`, instanziieren und Objektverweise zu verwalten, und daher keine-Designer-Datei erforderlich ist.



## <a name="related-links"></a>Verwandte Links

- [Steuerelemente (Beispiel)](https://developer.xamarin.com/samples/Controls/)
