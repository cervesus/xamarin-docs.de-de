---
title: Erstellen von Benutzeroberflächen Objekten in xamarin. IOS
description: Dieses Dokument enthält eine Übersicht über die Erstellung einer Benutzeroberfläche in xamarin. IOS. Es werden die Storyboards IOS Designer, Xcode C#Interface Builder, und erläutert.
ms.prod: xamarin
ms.assetid: 4D6B136C-744A-4936-8655-A77E62BA7A60
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/21/2017
ms.openlocfilehash: 204bf087a51132fdd204990c3b92453ecce96a53
ms.sourcegitcommit: 20c645f41620d5124da75943de1b690261d00660
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2019
ms.locfileid: "72426581"
---
# <a name="creating-user-interface-objects-in-xamarinios"></a>Erstellen von Benutzeroberflächen Objekten in xamarin. IOS

Apple gruppiert verwandte Funktionen in "Frameworks", die xamarin. IOS-Namespaces entsprechen. `UIKit` ist der Namespace, der alle Benutzeroberflächen-Steuerelemente für IOS enthält.

Wenn Ihr Code auf ein Benutzeroberflächen Steuerelement verweisen muss, z. b. eine Bezeichnung oder Schaltfläche, sollten Sie die folgende using-Anweisung einschließen:

```csharp
using UIKit;
```

Alle in diesem Kapitel erörterten Steuerelemente sind im UIKit-Namespace und jeder Benutzer Steuerelement-Klassenname das Präfix "`UI`".

Sie können UI-Steuerelemente und Layouts auf drei Arten bearbeiten:

- **[Xamarin IOS-Designer](~/ios/user-interface/designer/index.md)** – verwenden Sie den integrierten layoutdesigner von xamarin, um Bildschirme zu entwerfen. Doppelklicken Sie auf Storyboard-oder XIb-Dateien, um Sie mit dem integrierten Designer zu bearbeiten.
- **Xcode Interface Builder** – ziehen Sie Steuerelemente mit Interface Builder auf die Bildschirmlayouts. Öffnen Sie die Storyboard-oder XIb-Datei in Xcode, indem Sie im **Lösungspad** mit der rechten Maustaste auf die Datei klicken und dann **mit > Xcode Interface Builder öffnen**auswählen.
- **Die C# Verwendung** von –-Steuerelementen kann auch Programm gesteuert mit Code erstellt und der Ansichts Hierarchie hinzugefügt werden.

Neue Storyboard-und XIb-Dateien können hinzugefügt werden, indem Sie mit der rechten Maustaste auf ein IOS-Projekt klicken und **Hinzufügen > neue Datei...** auswählen.

Unabhängig davon, welche Methode Sie verwenden, können die Steuerelement Eigenschaften und C# -Ereignisse in ihrer Anwendungslogik weiterhin mit manipuliert werden.

## <a name="using-xamarin-ios-designer"></a>Verwenden des xamarin IOS-Designers

Doppelklicken Sie auf eine storyboarddatei, um mit dem Erstellen Ihrer Benutzeroberfläche im IOS-Designer zu beginnen. Steuerelemente können aus der **Toolbox** auf die Entwurfs Oberfläche gezogen werden, wie unten dargestellt:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

 [![](creating-ui-objects-images/image2b.png "Toolbox Pad")](creating-ui-objects-images/image2b.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

 [![](creating-ui-objects-images/image2b-vs.png "Toolbox Pad - Visual Stuio")](creating-ui-objects-images/image2b.png#lightbox)

-----

Wenn ein Steuerelement auf der Entwurfs Oberfläche ausgewählt wird, werden die Attribute für das Steuerelement **Eigenschaftenpad** angezeigt. Das **Widget > Identity > Name** -Feld, das im folgenden Screenshot aufgefüllt wird, wird als Name des *Ausgabenamens* verwendet. Auf diese Weise können Sie auf das Steuerelement C#in verweisen:

 [![](creating-ui-objects-images/image3b.png "Properties Widget Pad")](creating-ui-objects-images/image3b.png#lightbox)

Einen tieferen Einblick in die Verwendung des IOS-Designers finden Sie unter [Einführung in den IOS-Designer](~/ios/user-interface/designer/introduction.md) -Leitfaden.

## <a name="using-xcode-interface-builder"></a>Verwenden von Xcode Interface Builder

Wenn Sie mit der Verwendung von Interface Builder nicht vertraut sind, lesen Sie die [Interface Builder](https://developer.apple.com/xcode/interface-builder/) Dokumente von Apple.

Um ein Storyboard in Xcode zu öffnen, klicken Sie mit der rechten Maustaste, um auf das Kontextmenü für die storyboarddatei zuzugreifen, und wählen Sie das Öffnen mit dem **Xcode-Interface Builder**aus:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

 [![](creating-ui-objects-images/imagexcode.png "Storyboard context menu - Xcode")](creating-ui-objects-images/imagexcode.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](creating-ui-objects-images/imagexcode-vs.png "Storyboard context menu - Xcode")](creating-ui-objects-images/imagexcode-vs.png#lightbox)

-----

Steuerelemente können aus der unten dargestellten **Objektbibliothek** auf die Designoberfläche gezogen werden:

 [![](creating-ui-objects-images/image5a.png "Xcode Object Library")](creating-ui-objects-images/image5a.png#lightbox)

Wenn Sie die Benutzeroberfläche mit Interface Builder entwerfen, müssen Sie für jedes Steuerelement, auf C#das Sie verweisen möchten, ein Outlet erstellen. Dies geschieht, indem Sie den **Assistenten-Editor** mithilfe der Schaltfläche Center- **Editor** auf der Xcode-Symbolleisten Schaltfläche einschalten:

 [![](creating-ui-objects-images/image6a.png "Assistant Editor button")](creating-ui-objects-images/image6a.png#lightbox)

Klicken Sie auf ein Benutzeroberflächen Objekt. Ziehen Sie dann das **Steuer** Element in die h-Datei. Halten Sie die STRG-Taste gedrückt, und klicken Sie dann auf das Benutzeroberflächen Objekt, für das Sie das Outlet (oder die Aktion) erstellen, um das **ziehen zu steuern**. Halten Sie die Steuerelement Taste gedrückt, während Sie in die Header Datei ziehen. Schließen Sie das ziehen unterhalb der Definition "`@interface`" ab. Eine blaue Linie sollte mit einer Beschriftung INSERT-oder Outlet-Auflistung angezeigt werden, wie im folgenden Screenshot dargestellt.

Wenn Sie den Klick freigeben, werden Sie aufgefordert, einen Namen für das Outlet anzugeben, der zum Erstellen einer C# Eigenschaft verwendet wird, auf die im Code verwiesen werden kann:

 [![](creating-ui-objects-images/image8a.png "Creating an outlet")](creating-ui-objects-images/image8a.png#lightbox)

Weitere Informationen zur Integration von Xcode Interface Builder in Visual Studio für Mac finden Sie im Dokument zur [XIb-Code Generierung](~/ios/internals/xib-code-generation.md#generated) .

## <a name="using-c"></a>Using C @ no__t-0

Wenn Sie sich entscheiden, ein Benutzeroberflächen Objekt mithilfe C# von (z. b. in einem Ansichts-oder Ansichts Controller) Programm gesteuert zu erstellen, führen Sie die folgenden Schritte aus:

- Deklarieren Sie ein Feld auf Klassenebene für das Benutzeroberflächen Objekt. Erstellen Sie das Steuerelement einmal in `ViewDidLoad` (z. b.). Auf das Objekt kann dann in den Lebenszyklus Methoden des Ansichts Controllers verwiesen werden (z. b.
`ViewWillAppear`) angezeigt wird.
- Erstellen Sie eine `CGRect`, die den Rahmen des Steuer Elements definiert (die X-und Y-Koordinaten auf dem Bildschirm sowie dessen Breite und Höhe). Hierfür müssen Sie sicherstellen, dass Sie über eine `using CoreGraphics`-Direktive verfügen.
- Rufen Sie den Konstruktor auf, um das Steuerelement zu erstellen und zuzuweisen.
- Legen Sie alle Eigenschaften oder Ereignishandler fest.
- Wenden Sie `Add()` an, um das Steuerelement der Ansichts Hierarchie hinzuzufügen.

Im folgenden finden Sie ein einfaches Beispiel für das Erstellen einer `UILabel` in einem C#Ansichts Controller unter Verwendung von:

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

## <a name="using-c-and-storyboards"></a>Verwenden C# von und Storyboards

Wenn dem Designoberfläche Ansichts Controller hinzugefügt werden, werden C# im Projekt zwei entsprechende Dateien erstellt. In diesem Beispiel wurden `ControlsViewController.cs` und `ControlsViewController.designer.cs` automatisch erstellt:

 [![](creating-ui-objects-images/image9b.png "ViewController partial class")](creating-ui-objects-images/image9b.png#lightbox)

Die Datei `ControlsViewController.cs` ist für *Ihren Code*bestimmt. An dieser Stelle werden die `View`-Lebenszyklus Methoden wie `ViewDidLoad` und `ViewWillAppear` implementiert, und Sie können Ihre eigenen Eigenschaften, Felder und Methoden hinzufügen.

Der `ControlsViewController.designer.cs` ist generierter Code, der eine partielle Klasse enthält. Wenn Sie ein-Steuerelement auf der Entwurfs Oberfläche in Visual Studio für Mac benennen oder ein Outlet oder eine Aktion in Xcode erstellen, wird der Designer-Datei (Designer.cs) eine entsprechende Eigenschaft oder partielle Methode hinzugefügt. Der folgende Code zeigt ein Beispiel für den Code, der für zwei Schaltflächen und eine Textansicht generiert wird, wobei eine der Schaltflächen auch ein `TouchUpInside`-Ereignis aufweist.

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

Die `designer.cs`-Datei sollte nicht manuell bearbeitet werden – die IDE (Visual Studio für Mac oder Visual Studio) ist dafür verantwortlich, Sie mit dem Storyboard zu synchronisieren.

Wenn Benutzeroberflächen Objekte Programm gesteuert zu einem `View` oder `ViewController` hinzugefügt werden, instanziieren und verwalten Sie die Objekt Verweise selbst. Daher ist keine Designer-Datei erforderlich.

## <a name="related-links"></a>Verwandte Links

- [Steuerelemente (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/controls)
