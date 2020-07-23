---
title: Erstellen von Benutzeroberflächen Objekten in xamarin. IOS
description: Dieses Dokument enthält eine Übersicht über die Erstellung einer Benutzeroberfläche in xamarin. IOS. Er erläutert den IOS-Designer, Xcode Interface Builder, c# und Storyboards.
ms.prod: xamarin
ms.assetid: 4D6B136C-744A-4936-8655-A77E62BA7A60
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/21/2017
ms.openlocfilehash: 729289c1764746f9777ef3d720e77865c9a71389
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86937253"
---
# <a name="creating-user-interface-objects-in-xamarinios"></a>Erstellen von Benutzeroberflächen Objekten in xamarin. IOS

Apple gruppiert verwandte Funktionen in "Frameworks", die xamarin. IOS-Namespaces entsprechen. `UIKit`der Namespace, der alle Benutzeroberflächen-Steuerelemente für IOS enthält.

Wenn Ihr Code auf ein Benutzeroberflächen Steuerelement verweisen muss, z. b. eine Bezeichnung oder Schaltfläche, sollten Sie die folgende using-Anweisung einschließen:

```csharp
using UIKit;
```

Alle in diesem Kapitel erörterten Steuerelemente sind im UIKit-Namespace und jeder Benutzer Steuerelement-Klassenname das `UI` Präfix.

Sie können UI-Steuerelemente und Layouts auf drei Arten bearbeiten:

- **[Xamarin IOS-Designer](~/ios/user-interface/designer/index.md)** – verwenden Sie den integrierten layoutdesigner von xamarin, um Bildschirme zu entwerfen. Doppelklicken Sie auf Storyboard-oder XIb-Dateien, um Sie mit dem integrierten Designer zu bearbeiten.
- **Xcode Interface Builder** – ziehen Sie Steuerelemente mit Interface Builder auf die Bildschirmlayouts. Öffnen Sie die Storyboard-oder XIb-Datei in Xcode, indem Sie im **Lösungspad** mit der rechten Maustaste auf die Datei klicken und dann **mit > Xcode Interface Builder öffnen**auswählen.
- Die **Verwendung von c#** -–-Steuerelementen kann auch Programm gesteuert mit Code erstellt und der Ansichts Hierarchie hinzugefügt werden.

Neue Storyboard-und XIb-Dateien können hinzugefügt werden, indem Sie mit der rechten Maustaste auf ein IOS-Projekt klicken und **Hinzufügen > neue Datei...** auswählen.

Unabhängig davon, welche Methode Sie verwenden, können Steuerelement Eigenschaften und-Ereignisse in ihrer Anwendungslogik weiterhin mit c# manipuliert werden.

## <a name="using-xamarin-ios-designer"></a>Verwenden des xamarin IOS-Designers

Doppelklicken Sie auf eine storyboarddatei, um mit dem Erstellen Ihrer Benutzeroberfläche im IOS-Designer zu beginnen. Steuerelemente können aus der **Toolbox** auf die Entwurfs Oberfläche gezogen werden, wie unten dargestellt:

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

 [![Toolbox-Pad](creating-ui-objects-images/image2b.png)](creating-ui-objects-images/image2b.png#lightbox)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

 [![Toolbox-Pad-visuelle STUIO](creating-ui-objects-images/image2b-vs.png)](creating-ui-objects-images/image2b.png#lightbox)

-----

Wenn ein Steuerelement auf der Entwurfs Oberfläche ausgewählt wird, werden die Attribute für das Steuerelement **Eigenschaftenpad** angezeigt. Das **Widget > Identity > Name** -Feld, das im folgenden Screenshot aufgefüllt wird, wird als Name des *Ausgabenamens* verwendet. Auf diese Weise können Sie in c# auf das-Steuerelement verweisen:

 [![Eigenschaften-Widget-Pad](creating-ui-objects-images/image3b.png)](creating-ui-objects-images/image3b.png#lightbox)

Einen tieferen Einblick in die Verwendung des IOS-Designers finden Sie unter [Einführung in den IOS-Designer](~/ios/user-interface/designer/introduction.md) -Leitfaden.

## <a name="using-xcode-interface-builder"></a>Verwenden von Xcode Interface Builder

Wenn Sie mit der Verwendung von Interface Builder nicht vertraut sind, lesen Sie die [Interface Builder](https://developer.apple.com/xcode/interface-builder/) Dokumente von Apple.

Um ein Storyboard in Xcode zu öffnen, klicken Sie mit der rechten Maustaste, um auf das Kontextmenü für die storyboarddatei zuzugreifen, und wählen Sie das Öffnen mit dem **Xcode-Interface Builder**aus:

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

 [![Storyboard-Kontextmenü-Xcode](creating-ui-objects-images/imagexcode.png)](creating-ui-objects-images/imagexcode.png#lightbox)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[![Storyboard-Kontextmenü-Xcode](creating-ui-objects-images/imagexcode-vs.png)](creating-ui-objects-images/imagexcode-vs.png#lightbox)

-----

Steuerelemente können aus der unten dargestellten **Objektbibliothek** auf die Designoberfläche gezogen werden:

 [![Xcode-Objektbibliothek](creating-ui-objects-images/image5a.png)](creating-ui-objects-images/image5a.png#lightbox)

Wenn Sie die Benutzeroberfläche mit Interface Builder entwerfen, müssen Sie für jedes Steuerelement, auf das Sie in c# verweisen möchten, ein **Outlet** erstellen. Dies geschieht, indem Sie den **Assistenten-Editor** mithilfe der Schaltfläche Center- **Editor** auf der Xcode-Symbolleisten Schaltfläche einschalten:

 [![Schaltfläche für den Assistenten](creating-ui-objects-images/image6a.png)](creating-ui-objects-images/image6a.png#lightbox)

Klicken Sie auf ein Benutzeroberflächen Objekt. Ziehen Sie dann das **Steuer** Element in die h-Datei. Halten Sie die STRG-Taste gedrückt, und klicken Sie dann auf das Benutzeroberflächen Objekt, für das Sie das Outlet (oder die Aktion) erstellen, um das **ziehen zu steuern**. Halten Sie die Steuerelement Taste gedrückt, während Sie in die Header Datei ziehen. Beenden Sie den Zieh Abschnitt unterhalb der `@interface` Definition. Eine blaue Linie sollte mit einer Beschriftung INSERT-oder Outlet-Auflistung angezeigt werden, wie im folgenden Screenshot dargestellt.

Wenn Sie den Klick freigeben, werden Sie aufgefordert, einen Namen für das Outlet anzugeben, der zum Erstellen einer c#-Eigenschaft verwendet wird, auf die im Code verwiesen werden kann:

 [![Erstellen eines Outlets](creating-ui-objects-images/image8a.png)](creating-ui-objects-images/image8a.png#lightbox)

Weitere Informationen zur Integration von Xcode Interface Builder in Visual Studio für Mac finden Sie im Dokument zur [XIb-Code Generierung](~/ios/internals/xib-code-generation.md#generated) .

## <a name="using-c"></a>Verwenden von C\#

Wenn Sie sich entscheiden, ein Benutzeroberflächen Objekt Programm gesteuert mithilfe von c# (z. b. in einem Ansichts-oder Ansichts Controller) zu erstellen, führen Sie die folgenden Schritte aus:

- Deklarieren Sie ein Feld auf Klassenebene für das Benutzeroberflächen Objekt. Erstellen Sie das Steuerelement selbst einmal, `ViewDidLoad` z. b.. Auf das Objekt kann dann in den Lebenszyklus Methoden des Ansichts Controllers verwiesen werden (z. b.
`ViewWillAppear`).
- Erstellen Sie einen `CGRect` , der den Rahmen des Steuer Elements definiert (seine X-und Y-Koordinaten auf dem Bildschirm sowie seine Breite und Höhe). Sie müssen sicherstellen, dass Sie über eine- `using CoreGraphics` Direktive für diese verfügen.
- Rufen Sie den Konstruktor auf, um das Steuerelement zu erstellen und zuzuweisen.
- Legen Sie alle Eigenschaften oder Ereignishandler fest.
- Wird aufgerufen `Add()` , um der Ansichts Hierarchie das Steuerelement hinzuzufügen.

Im folgenden finden Sie ein einfaches Beispiel für das Erstellen einer `UILabel` in einem Ansichts Controller mit c#:

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

<a name="partial_classes"></a>

## <a name="using-c-and-storyboards"></a>Verwenden von c# und Storyboards

Beim Hinzufügen von Ansichts Controllern zum Designoberfläche werden zwei entsprechende c#-Dateien im Projekt erstellt. In diesem Beispiel wurden `ControlsViewController.cs` und `ControlsViewController.designer.cs` automatisch erstellt:

 [![Partielle ViewController-Klasse](creating-ui-objects-images/image9b.png)](creating-ui-objects-images/image9b.png#lightbox)

Die `ControlsViewController.cs` Datei ist für *Ihren Code*bestimmt. An dieser Stelle `View` werden die Lebenszyklus Methoden wie `ViewDidLoad` und `ViewWillAppear` implementiert, und Sie können Ihre eigenen Eigenschaften, Felder und Methoden hinzufügen.

Der `ControlsViewController.designer.cs` ist generierter Code, der eine partielle Klasse enthält. Wenn Sie ein-Steuerelement auf der Entwurfs Oberfläche in Visual Studio für Mac benennen oder ein Outlet oder eine Aktion in Xcode erstellen, wird der Designer-Datei (Designer.cs) eine entsprechende Eigenschaft oder partielle Methode hinzugefügt. Der folgende Code zeigt ein Beispiel für den Code, der für zwei Schaltflächen und eine Textansicht generiert wird, wobei eine der Schaltflächen auch ein- `TouchUpInside` Ereignis aufweist.

Diese Elemente der partiellen Klasse ermöglichen dem Code das verweisen auf die Steuerelemente und das reagieren auf die Aktionen, die auf der Entwurfs Oberfläche deklariert sind:

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

Die `designer.cs` Datei sollte nicht manuell bearbeitet werden – die IDE (Visual Studio für Mac oder Visual Studio) ist dafür verantwortlich, Sie mit dem Storyboard zu synchronisieren.

Wenn Benutzeroberflächen Objekte Programm gesteuert zu einem oder hinzugefügt werden `View` `ViewController` , instanziieren und verwalten Sie die Objekt Verweise selbst. Daher ist keine Designer-Datei erforderlich.

## <a name="related-links"></a>Ähnliche Themen

- [Steuerelemente (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/controls)
