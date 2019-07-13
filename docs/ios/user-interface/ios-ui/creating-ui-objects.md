---
title: Erstellen von Benutzeroberflächenobjekten in Xamarin.iOS
description: Dieses Dokument enthält eine Übersicht über eine Benutzeroberfläche in Xamarin.iOS zu erstellen. Es wird erläutert, die iOS-Designer, Xcode Interface Builder, C#, und Storyboards.
ms.prod: xamarin
ms.assetid: 4D6B136C-744A-4936-8655-A77E62BA7A60
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/21/2017
ms.openlocfilehash: a80bf876e239e1788a371a1f09d36d73247d4611
ms.sourcegitcommit: 7ccc7a9223cd1d3c42cd03ddfc28050a8ea776c2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/13/2019
ms.locfileid: "67865011"
---
# <a name="creating-user-interface-objects-in-xamarinios"></a>Erstellen von Benutzeroberflächenobjekten in Xamarin.iOS

Apple-Gruppen im Zusammenhang mit Funktionen in "Frameworks" die Xamarin.iOS-Namespaces entsprechen. `UIKit` ist der Namespace, der alle Steuerelemente der Benutzeroberfläche für iOS enthält.

Wenn Ihr Code auf ein Steuerelement der Benutzeroberfläche, z. B. eine Bezeichnung oder eine Schaltfläche muss, denken Sie daran, die folgenden using-Anweisung:

```csharp
using UIKit;
```

Alle Steuerelemente, die in diesem Kapitel erläutert im UIKit-Namespace sind und jede Benutzer-steuerelementklassenname der `UI` Präfix.

Sie können die UI-Steuerelemente und Layouts auf drei Arten bearbeiten:

-  **[Xamarin.IOS-Designer](~/ios/user-interface/designer/index.md)**  – Verwendung Xamarins integrierten Layout-Designer Entwerfen von Bildschirmen. Doppelklicken Sie auf Storyboard oder XIB-Dateien mit der integrierten Designer bearbeiten.
-  **Xcode Interface Builder** – ziehen Sie Steuerelemente auf Ihre Bildschirmlayouts mit Interface Builder. Öffnen Sie die Storyboard oder XIB-Datei in Xcode, mit der rechten Maustaste in die Datei in die **Lösungspad** und **Öffnen mit > Interface Builder von Xcode**.
-  **Mithilfe von C#**  – Steuerelemente können auch programmgesteuert mit Code erstellt und hinzugefügt werden, die Hierarchie von Inhaltsansichten.

Neue Storyboard und XIB-Dateien können hinzugefügt werden, indem mit der rechten Maustaste auf ein iOS-Projekt und **hinzufügen > neue Datei...** .

Welche Methode Sie verwenden, Eigenschaften und Ereignisse können jedoch geändert werden mit C# in Ihrer Anwendungslogik.

## <a name="using-xamarin-ios-designer"></a>Mithilfe von Xamarin.IOS-Designer

Um Ihre Benutzeroberfläche erstellen, in der iOS-Designer zu starten, doppelklicken Sie auf eine Storyboard-Datei. Steuerelemente gezogen werden können, auf die Entwurfsoberfläche, aus der **Toolbox** wie unten gezeigt:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

 [![](creating-ui-objects-images/image2b.png "Toolbox Pad")](creating-ui-objects-images/image2b.png#lightbox)
 
# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

 [![](creating-ui-objects-images/image2b-vs.png "Pad \"Toolbox\" – Visual Studio")](creating-ui-objects-images/image2b.png#lightbox)
 
-----

Wenn ein Steuerelement ausgewählt ist, auf der Entwurfsoberfläche der **Pad "Eigenschaften"** zeigt die Attribute für dieses Steuerelement. Die **Widget > Identität > Namen** Feld, das im folgenden Screenshot ausgefüllt wird, wird verwendet, als die *Outlet* Name. Dies ist, wie Sie das Steuerelement in verweisen können C#:

 [![](creating-ui-objects-images/image3b.png "Widget-Pad \"Eigenschaften\"")](creating-ui-objects-images/image3b.png#lightbox)

Ein tieferer Einblick in die Verwendung des iOS Designers werden soll, finden Sie in der [Einführung in iOS-Designer](~/ios/user-interface/designer/introduction.md) Guide.

## <a name="using-xcode-interface-builder"></a>Mithilfe von Xcode Interface Builder

Wenn Sie mit der Verwendung von Interface Builder nicht vertraut sind, finden Sie im Apple- [Interface Builder](https://developer.apple.com/xcode/interface-builder/) Dokumente.

Um ein Storyboard in Xcode öffnen, mit der rechten Maustaste das Kontextmenü für die Storyboard-Datei zugreifen, und wählen Sie öffnen die **Xcode Interface Builder**:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

 [![](creating-ui-objects-images/imagexcode.png "Kontextmenü für die Storyboard - Xcode")](creating-ui-objects-images/imagexcode.png#lightbox)
 
# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](creating-ui-objects-images/imagexcode-vs.png "Kontextmenü für die Storyboard - Xcode")](creating-ui-objects-images/imagexcode-vs.png#lightbox)

-----

Steuerelemente gezogen werden können, auf die Entwurfsoberfläche, aus der **Objektbibliothek** unten dargestellt:

 [![](creating-ui-objects-images/image5a.png "Xcode-Objektbibliothek")](creating-ui-objects-images/image5a.png#lightbox)

Beim Entwerfen der Benutzeroberflächenautomatisierungs mit Interface Builder müssen Sie erstellen eine **Outlet** für jedes Steuerelement, das Sie verweisen in möchten C#. Dies erfolgt durch das Aktivieren der **Assistant Editor** über das Center **Editor** Schaltfläche auf der Xcode-Symbolleisten-Schaltfläche:

 [![](creating-ui-objects-images/image6a.png "Assistenten-Editor-Schaltfläche")](creating-ui-objects-images/image6a.png#lightbox)

Klicken Sie auf ein Benutzerobjekt-Schnittstelle. Klicken Sie dann **Steuerelement Drag &** in der h-Datei. Um **Steuerelement Drag &** , halten Sie die STRG-Taste und halten Sie über die Benutzer-Schnittstellenobjekts, das Sie für den Ausgang (oder die Aktion) erstellen. Halten Sie gedrückt halten Sie die STRG-Taste und Sie in der Headerdatei ziehen. "Fertig stellen" unten ziehen die `@interface` Definition. Mit einer Beschriftung einfügen Outlet oder Outlet-Auflistung, sollte eine blaue Linie angezeigt werden, wie im folgenden Screenshot dargestellt.

Wenn Sie die auf freigeben, Sie aufgefordert werden, einen Namen für die Outlet angeben, die verwendet werden wird, zum Erstellen, einer C# -Eigenschaft, die im Code verwiesen werden kann:

 [![](creating-ui-objects-images/image8a.png "Erstellen eines outlets")](creating-ui-objects-images/image8a.png#lightbox)

Weitere Informationen dazu, wie Interface Builder von Xcode in Visual Studio für Mac integriert ist, finden Sie in der [Xib-Codegenerierung](~/ios/internals/xib-code-generation.md#generated) Dokument.

## <a name="using-c"></a>Mithilfe vonC#

Wenn Sie programmgesteuert erstellen ein Objekt mit User Interface C# (in einer Sicht oder der View-Controller, z. B.), gehen Sie folgendermaßen vor:

-  Deklarieren Sie ein Klassenfeld-Ebene für das Benutzerobjekt-Schnittstelle. Erstellen Sie das Steuerelement selbst einmal im `ViewDidLoad` z. B. Das Objekt kann dann in der gesamten die Lebenszyklusmethoden der View-Controller (z.B.) verwiesen werden
`ViewWillAppear`) angezeigt wird.
-  Erstellen Sie eine `CGRect` , die den Rahmen des Steuerelements (der X- und Y-Koordinaten auf dem Bildschirm "" sowie die Breite und Höhe) definiert. Sie müssen sicherstellen, dass eine `using CoreGraphics` für diese Direktive.
-  Rufen Sie den Konstruktor zum Erstellen, und weisen Sie das Steuerelement.
-  Legen Sie Eigenschaften oder Ereignishandler.
-  Rufen Sie `Add()` das Steuerelement auf die Hierarchie von Inhaltsansichten hinzugefügt.

Hier ist ein einfaches Beispiel zum Erstellen einer `UILabel` in einer View Controller mit C#:

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

## <a name="using-c-and-storyboards"></a>Mithilfe von C# und Storyboards

Wenn View Controller hinzugefügt werden, auf die Entwurfsoberfläche, in zwei entsprechenden C# Dateien im Projekt erstellt werden. In diesem Beispiel `ControlsViewController.cs` und `ControlsViewController.designer.cs` automatisch erstellt wurden:

 [![](creating-ui-objects-images/image9b.png "Partielle ViewController-Klasse")](creating-ui-objects-images/image9b.png#lightbox)

Die `MainViewController.cs` Datei soll *Code*. Hier kommt die `View` Lebenszyklusmethoden, z. B. `ViewDidLoad` und `ViewWillAppear` werden implementiert und in dem Sie Ihre eigenen Eigenschaften, Felder und Methoden hinzufügen können.

Die `ControlsViewController.designer.cs` ist generierter Code enthält eine partielle Klasse. Wenn der Name eines Steuerelements auf der Entwurfsoberfläche in Visual Studio für Mac oder ein Outlet oder eine Aktion in Xcode, eine entsprechende Eigenschaft oder partielle Methode erstellen, werden der Designer ("Designer.cs")-Datei hinzugefügt. Der folgende Code zeigt ein Beispiel für zwei Schaltflächen und einer Textansicht generiert Code, in denen eine der Schaltflächen verfügt auch über eine `TouchUpInside` Ereignis.

Diese Elemente der partiellen Klasse aktivieren Sie Ihren Code zum Verweisen auf die Steuerelemente, und reagieren auf die Aktionen, die auf der Entwurfsoberfläche deklariert werden:

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

Die `designer.cs` Datei sollte nicht manuell bearbeitet werden: die IDE (Visual Studio für Mac oder Visual Studio) ist verantwortlich dafür, die mit dem Storyboard synchronisiert.

Wenn der Benutzeroberflächenobjekte programmgesteuert hinzugefügt werden eine `View` oder `ViewController`Sie instanziieren und die Objektverweise zu verwalten und daher keine Designer-Datei erforderlich ist.



## <a name="related-links"></a>Verwandte Links

- [Steuerelemente (Beispiel)](https://developer.xamarin.com/samples/monotouch/Controls/)
