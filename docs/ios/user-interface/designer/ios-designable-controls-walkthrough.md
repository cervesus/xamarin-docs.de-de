---
title: Verwenden benutzerdefinierter Steuerelemente im iOS-Designer
description: In diesem Dokument wird beschrieben, wie Sie ein benutzerdefiniertes Steuerelement erstellen und es mit dem Xamarin Designer für IOS verwenden. Es wird gezeigt, wie das Steuerelement in der Toolbox des IOS-Designers verfügbar gemacht wird, das Steuerelement implementiert wird, damit es ordnungsgemäß und Entwurfszeit und vieles mehr gerendert wird.
ms.prod: xamarin
ms.assetid: 9032B32E-97BD-4DA6-9955-811B84682578
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/22/2017
ms.openlocfilehash: 4b8c72da1e280b83e215bca9316bc0b9de99402c
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73003805"
---
# <a name="using-custom-controls-with-the-ios-designer"></a>Verwenden benutzerdefinierter Steuerelemente im iOS-Designer

## <a name="requirements"></a>Anforderungen

Der Xamarin Designer für IOS ist in Visual Studio für Mac und Visual Studio 2017 und höher unter Windows verfügbar.

In diesem Leitfaden wird davon ausgegangen, dass Sie sich mit den Inhalten vertraut machen, die in den [Leitfäden zu](~/ios/get-started/index.md)

## <a name="walkthrough"></a>Exemplarische Vorgehensweise

> [!IMPORTANT]
> Ab xamarin. Studio 5,5 unterscheidet sich die Art und Weise, in der benutzerdefinierte Steuerelemente erstellt werden, geringfügig von früheren Versionen. Um ein benutzerdefiniertes Steuerelement zu erstellen, ist entweder die `IComponent`-Schnittstelle erforderlich (mit den zugehörigen Implementierungs Methoden), oder die-Klasse kann mit `[DesignTimeVisible(true)]`kommentiert werden. Die letztere Methode wird in der folgenden exemplarischen Vorgehensweise verwendet.

1. Erstellen Sie eine neue Projekt Mappe aus der **IOS-> app > Vorlage C# für die Einzelansicht-Anwendungs >** , benennen Sie Sie`ScratchTicket`, und fahren Sie mit dem Assistenten für neue Projekte fort:

    [![](ios-designable-controls-walkthrough-images/01new.png "Create a new solution")](ios-designable-controls-walkthrough-images/01new.png#lightbox)

1. Erstellen Sie eine neue leere Klassendatei mit dem Namen `ScratchTicketView`:

    [![](ios-designable-controls-walkthrough-images/02new.png "Create a new ScratchTicketView class")](ios-designable-controls-walkthrough-images/02new.png#lightbox)

1. Fügen Sie den folgenden Code für `ScratchTicketView`-Klasse hinzu:

    ```csharp
    using System;
    using System.ComponentModel;
    using CoreGraphics;
    using Foundation;
    using UIKit;

    namespace ScratchTicket
    {
        [Register("ScratchTicketView"), DesignTimeVisible(true)]
        public class ScratchTicketView : UIView
        {
            CGPath path;
            CGPoint initialPoint;
            CGPoint latestPoint;
            bool startNewPath = false;
            UIImage image;

            [Export("Image"), Browsable(true)]
            public UIImage Image
            {
                get { return image; }
                set
                {
                    image = value;
                    SetNeedsDisplay();
                }
            }

            public ScratchTicketView(IntPtr p)
                : base(p)
            {
                Initialize();
            }

            public ScratchTicketView()
            {
                Initialize();
            }

            void Initialize()
            {
                initialPoint = CGPoint.Empty;
                latestPoint = CGPoint.Empty;
                BackgroundColor = UIColor.Clear;
                Opaque = false;
                path = new CGPath();
                SetNeedsDisplay();
            }

            public override void TouchesBegan(NSSet touches, UIEvent evt)
            {
                base.TouchesBegan(touches, evt);

                var touch = touches.AnyObject as UITouch;

                if (touch != null)
                {
                    initialPoint = touch.LocationInView(this);
                }
            }

            public override void TouchesMoved(NSSet touches, UIEvent evt)
            {
                base.TouchesMoved(touches, evt);

                var touch = touches.AnyObject as UITouch;

                if (touch != null)
                {
                    latestPoint = touch.LocationInView(this);
                    SetNeedsDisplay();
                }
            }

            public override void TouchesEnded(NSSet touches, UIEvent evt)
            {
                base.TouchesEnded(touches, evt);
                startNewPath = true;
            }

            public override void Draw(CGRect rect)
            {
                base.Draw(rect);

                using (var g = UIGraphics.GetCurrentContext())
                {
                    if (image != null)
                        g.SetFillColor((UIColor.FromPatternImage(image).CGColor));
                    else
                        g.SetFillColor(UIColor.LightGray.CGColor);
                    g.FillRect(rect);

                    if (!initialPoint.IsEmpty)
                    {
                        g.SetLineWidth(20);
                        g.SetBlendMode(CGBlendMode.Clear);
                        UIColor.Clear.SetColor();

                        if (path.IsEmpty || startNewPath)
                        {
                            path.AddLines(new CGPoint[] { initialPoint, latestPoint });
                            startNewPath = false;
                        }
                        else
                        {
                            path.AddLineToPoint(latestPoint);
                        }

                        g.SetLineCap(CGLineCap.Round);
                        g.AddPath(path);
                        g.DrawPath(CGPathDrawingMode.Stroke);
                    }
                }
            }
        }
    }
    ```

1. Fügen Sie die `FillTexture.png`-, `FillTexture2.png`-und `Monkey.png` Dateien (verfügbar über [GitHub](https://github.com/xamarin/ios-samples/blob/master/ScratchTicket/Resources/images.zip?raw=true)) dem **Ressourcen** Ordner hinzu.

1. Doppelklicken Sie auf die Datei `Main.storyboard`, um Sie im Designer zu öffnen:

    [![](ios-designable-controls-walkthrough-images/03new.png "The iOS Designer")](ios-designable-controls-walkthrough-images/03new.png#lightbox)

1. Ziehen Sie eine **Bildansicht** aus der **Toolbox** auf die Ansicht im Storyboard, und legen Sie Sie dort ab.

    [![](ios-designable-controls-walkthrough-images/04new.png "An Image View added to the layout")](ios-designable-controls-walkthrough-images/04new.png#lightbox)

1. Wählen Sie die **Bildansicht** aus, und ändern Sie die **Image** -Eigenschaft in `Monkey.png`.

    [![](ios-designable-controls-walkthrough-images/05new.png "Setting Image View Image property to Monkey.png")](ios-designable-controls-walkthrough-images/05new.png#lightbox)

1. Bei der Verwendung von Größenklassen muss diese Bildansicht eingeschränkt werden. Klicken Sie zweimal auf das Bild, um es in den Einschränkungs Modus zu versetzen. Wir beschränken ihn auf den Mittelpunkt, indem wir auf das zentrierende Handle klicken und ihn sowohl vertikal als auch horizontal ausrichten:

    [![](ios-designable-controls-walkthrough-images/06new.png "Centering the image")](ios-designable-controls-walkthrough-images/06new.png#lightbox)

1. Um die Höhe und Breite einzuschränken, klicken Sie auf die Steuerpunkte mit der Größenbegrenzung (die "mit dem Strich" formatierte Handles), und wählen Sie "width" und "Height" aus:

    [![](ios-designable-controls-walkthrough-images/07new.png "Adding Constraints")](ios-designable-controls-walkthrough-images/07new.png#lightbox)

1. Aktualisieren Sie den Frame basierend auf Einschränkungen, indem Sie auf der Symbolleiste auf die Schaltfläche Aktualisieren klicken:

    [![](ios-designable-controls-walkthrough-images/08new.png "The Constraints toolbar")](ios-designable-controls-walkthrough-images/08new.png#lightbox)

1. Erstellen Sie als nächstes das Projekt, sodass die **Ticket Ansicht Scratch** unter **benutzerdefinierte Komponenten** in der Toolbox angezeigt wird:

    [![](ios-designable-controls-walkthrough-images/09new.png "The Custom Components toolbox")](ios-designable-controls-walkthrough-images/09new.png#lightbox)

1. Ziehen Sie eine **Scratch-Ticket Ansicht** per Drag & Drop, damit Sie über dem affenbild angezeigt wird. Passen Sie die Zieh Punkte so an, dass die Scratch-Ticket Ansicht den Affen vollständig abdeckt, wie unten dargestellt:

    [![](ios-designable-controls-walkthrough-images/10new.png "A Scratch Ticket View over the Image View")](ios-designable-controls-walkthrough-images/10new.png#lightbox)

1. Schränken Sie die Ansicht für das Scratch-Ticket auf die Bildansicht ein, indem Sie ein umgebenden Rechteck zeichnen, um beide Sichten auszuwählen. Wählen Sie die Optionen aus, um Sie auf die breiten-, Höhen-, Mittel-und Mittel-und Update Rahmen zu beschränken, wie unten dargestellt:

    [![](ios-designable-controls-walkthrough-images/11new.png "Centering and adding Constraints")](ios-designable-controls-walkthrough-images/11new.png#lightbox)

1. Führen Sie die Anwendung aus, und deaktivieren Sie das Bild, um den Affen zu verdeutlichen.

    [![](ios-designable-controls-walkthrough-images/10-app.png "A sample app run")](ios-designable-controls-walkthrough-images/10-app.png#lightbox)

## <a name="adding-design-time-properties"></a>Hinzufügen von Entwurfszeit Eigenschaften

Der Designer umfasst auch Entwurfszeit Unterstützung für benutzerdefinierte Steuerelemente des Eigenschaftentyps numeric, Enumeration, String, bool, CGSize, uicolor und uiimage. Um dies zu veranschaulichen, fügen Sie dem `ScratchTicketView` eine Eigenschaft hinzu, um das Bild festzulegen, das "kratzt" ist.

Fügen Sie der `ScratchTicketView`-Klasse den folgenden Code für die-Eigenschaft hinzu:

```csharp
[Export("Image"), Browsable(true)]
public UIImage Image
{
    get { return image; }
    set {
            image = value;
              SetNeedsDisplay ();
        }
}
```

Möglicherweise möchten Sie der `Draw`-Methode auch eine NULL-Überprüfung hinzufügen, wie im folgenden Beispiel:

```csharp
public override void Draw(CGRect rect)
{
    base.Draw(rect);

    using (var g = UIGraphics.GetCurrentContext())
    {
        if (image != null)
            g.SetFillColor ((UIColor.FromPatternImage (image).CGColor));
        else
            g.SetFillColor (UIColor.LightGray.CGColor);

        g.FillRect(rect);

        if (!initialPoint.IsEmpty)
        {
             g.SetLineWidth(20);
             g.SetBlendMode(CGBlendMode.Clear);
             UIColor.Clear.SetColor();

             if (path.IsEmpty || startNewPath)
             {
                 path.AddLines(new CGPoint[] { initialPoint, latestPoint });
                 startNewPath = false;
             }
             else
             {
                 path.AddLineToPoint(latestPoint);
             }

             g.SetLineCap(CGLineCap.Round);
             g.AddPath(path);
             g.DrawPath(CGPathDrawingMode.Stroke);
        }
    }
}
```

Das Einschließen eines `ExportAttribute` und eines `BrowsableAttribute`, bei dem das-Argument auf `true` festgelegt ist, führt dazu, dass die-Eigenschaft im **Eigenschaften** Panel des Designers angezeigt wird. Das Ändern der Eigenschaft in ein anderes Bild, wie z. b. `FillTexture2.png`, führt dazu, dass das Steuerelement zur Entwurfszeit aktualisiert wird, wie unten dargestellt:

 [![](ios-designable-controls-walkthrough-images/11-customproperty.png "Editing Design Time properties")](ios-designable-controls-walkthrough-images/10-app.png#lightbox)

## <a name="summary"></a>Zusammenfassung

In diesem Artikel haben wir erläutert, wie Sie ein benutzerdefiniertes Steuerelement erstellen und es in einer IOS-Anwendung mit dem IOS-Designer nutzen. Wir haben gesehen, wie das Steuerelement erstellt und erstellt wird, um es für eine Anwendung in der **Toolbox**des Designers verfügbar zu machen. Außerdem wurde erläutert, wie das Steuerelement implementiert wird, sodass es sowohl zur Entwurfszeit als auch zur Laufzeit ordnungsgemäß gerendert wird, und wie benutzerdefinierte Steuerelement Eigenschaften im Designer verfügbar gemacht werden.

## <a name="related-links"></a>Verwandte Links

- [Scratchticket (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/scratchticket)
- [erforderliche Images (Beispiel)](https://github.com/xamarin/ios-samples/blob/master/ScratchTicket/Resources/images.zip?raw=true)
