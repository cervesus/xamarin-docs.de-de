---
title: Verwenden benutzerdefinierter Steuerelemente im iOS-Designer
description: In diesem Dokument wird beschrieben, wie Sie ein benutzerdefiniertes Steuerelement erstellen und es mit dem Xamarin Designer für IOS verwenden. Es wird gezeigt, wie das Steuerelement in der Toolbox des IOS-Designers verfügbar gemacht wird, das Steuerelement implementiert wird, damit es ordnungsgemäß und Entwurfszeit und vieles mehr gerendert wird.
ms.prod: xamarin
ms.assetid: 9032B32E-97BD-4DA6-9955-811B84682578
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/22/2017
ms.openlocfilehash: 4652497aa6a7819afe7224617a429b2852566255
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86934692"
---
# <a name="using-custom-controls-with-the-ios-designer"></a>Verwenden benutzerdefinierter Steuerelemente im iOS-Designer

## <a name="requirements"></a>Requirements (Anforderungen)

Der Xamarin Designer für IOS ist in Visual Studio für Mac und Visual Studio 2017 und höher unter Windows verfügbar.

In diesem Leitfaden wird davon ausgegangen, dass Sie sich mit den Inhalten vertraut machen, die in den [Leitfäden zu](~/ios/get-started/index.md)

## <a name="walkthrough"></a>Exemplarische Vorgehensweise

> [!IMPORTANT]
> Ab xamarin. Studio 5,5 unterscheidet sich die Art und Weise, in der benutzerdefinierte Steuerelemente erstellt werden, geringfügig von früheren Versionen. Zum Erstellen eines benutzerdefinierten Steuer Elements ist entweder die- `IComponent` Schnittstelle erforderlich (mit den zugehörigen Implementierungs Methoden), oder die-Klasse kann mit kommentiert werden `[DesignTimeVisible(true)]` . Die letztere Methode wird in der folgenden exemplarischen Vorgehensweise verwendet.

1. Erstellen Sie eine neue Projekt Mappe aus der **IOS-> App > Einzelansicht Anwendung > c#** -Vorlage, benennen Sie Sie `ScratchTicket` , und fahren Sie mit dem Assistenten für neue Projekte fort:

    [![Erstellen einer neuen Projekt Mappe](ios-designable-controls-walkthrough-images/01new.png)](ios-designable-controls-walkthrough-images/01new.png#lightbox)

1. Erstellen Sie eine neue leere Klassendatei mit dem Namen `ScratchTicketView` :

    [![Erstellen einer neuen scratchticketview-Klasse](ios-designable-controls-walkthrough-images/02new.png)](ios-designable-controls-walkthrough-images/02new.png#lightbox)

1. Fügen Sie den folgenden Code für die- `ScratchTicketView` Klasse hinzu:

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

1. Fügen Sie `FillTexture.png` die `FillTexture2.png` Dateien, und `Monkey.png` (die über [GitHub](https://github.com/xamarin/ios-samples/blob/master/ScratchTicket/Resources/images.zip?raw=true)verfügbar sind) dem **Ressourcen** Ordner hinzu.

1. Doppelklicken Sie `Main.storyboard` auf die Datei, um Sie im Designer zu öffnen:

    [![Der iOS-Designer](ios-designable-controls-walkthrough-images/03new.png)](ios-designable-controls-walkthrough-images/03new.png#lightbox)

1. Ziehen Sie eine **Bildansicht** aus der **Toolbox** auf die Ansicht im Storyboard, und legen Sie Sie dort ab.

    [![Eine Bildansicht, die dem Layout hinzugefügt wurde.](ios-designable-controls-walkthrough-images/04new.png)](ios-designable-controls-walkthrough-images/04new.png#lightbox)

1. Wählen Sie die **Bildansicht** aus, und ändern Sie deren **Bild** Eigenschaft in `Monkey.png` .

    [![Image View Image-Eigenschaft wird auf Monkey.pngfestgelegt](ios-designable-controls-walkthrough-images/05new.png)](ios-designable-controls-walkthrough-images/05new.png#lightbox)

1. Bei der Verwendung von Größenklassen muss diese Bildansicht eingeschränkt werden. Klicken Sie zweimal auf das Bild, um es in den Einschränkungs Modus zu versetzen. Wir beschränken ihn auf den Mittelpunkt, indem wir auf das zentrierende Handle klicken und ihn sowohl vertikal als auch horizontal ausrichten:

    [![Zentrieren des Bilds](ios-designable-controls-walkthrough-images/06new.png)](ios-designable-controls-walkthrough-images/06new.png#lightbox)

1. Um die Höhe und Breite einzuschränken, klicken Sie auf die Steuerpunkte mit der Größenbegrenzung (die "mit dem Strich" formatierte Handles), und wählen Sie "width" und "Height" aus:

    [![Hinzufügen von Einschränkungen](ios-designable-controls-walkthrough-images/07new.png)](ios-designable-controls-walkthrough-images/07new.png#lightbox)

1. Aktualisieren Sie den Frame basierend auf Einschränkungen, indem Sie auf der Symbolleiste auf die Schaltfläche Aktualisieren klicken:

    [![Die Einschränkungs Symbolleiste](ios-designable-controls-walkthrough-images/08new.png)](ios-designable-controls-walkthrough-images/08new.png#lightbox)

1. Erstellen Sie als nächstes das Projekt, sodass die **Ticket Ansicht Scratch** unter **benutzerdefinierte Komponenten** in der Toolbox angezeigt wird:

    [![Die Toolbox für benutzerdefinierte Komponenten](ios-designable-controls-walkthrough-images/09new.png)](ios-designable-controls-walkthrough-images/09new.png#lightbox)

1. Ziehen Sie eine **Scratch-Ticket Ansicht** per Drag & Drop, damit Sie über dem affenbild angezeigt wird. Passen Sie die Zieh Punkte so an, dass die Scratch-Ticket Ansicht den Affen vollständig abdeckt, wie unten dargestellt:

    [![Eine Scratch-Ticket Ansicht in der Bildansicht](ios-designable-controls-walkthrough-images/10new.png)](ios-designable-controls-walkthrough-images/10new.png#lightbox)

1. Schränken Sie die Ansicht für das Scratch-Ticket auf die Bildansicht ein, indem Sie ein umgebenden Rechteck zeichnen, um beide Sichten auszuwählen. Wählen Sie die Optionen aus, um Sie auf die breiten-, Höhen-, Mittel-und Mittel-und Update Rahmen zu beschränken, wie unten dargestellt:

    [![Zentrieren und Hinzufügen von Einschränkungen](ios-designable-controls-walkthrough-images/11new.png)](ios-designable-controls-walkthrough-images/11new.png#lightbox)

1. Führen Sie die Anwendung aus, und deaktivieren Sie das Bild, um den Affen zu verdeutlichen.

    [![Eine Beispiel-App-Run](ios-designable-controls-walkthrough-images/10-app.png)](ios-designable-controls-walkthrough-images/10-app.png#lightbox)

## <a name="adding-design-time-properties"></a>Hinzufügen von Entwurfszeit Eigenschaften

Der Designer umfasst auch Entwurfszeit Unterstützung für benutzerdefinierte Steuerelemente des Eigenschaftentyps numeric, Enumeration, String, bool, CGSize, uicolor und uiimage. Um dies zu veranschaulichen, fügen Sie eine Eigenschaft hinzu, um `ScratchTicketView` das Bild festzulegen, das "kratzt" ist.

Fügen Sie der-Klasse den folgenden Code `ScratchTicketView` für die-Eigenschaft hinzu:

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

Möglicherweise möchten Sie der-Methode auch eine NULL-Überprüfung hinzufügen, wie im folgenden Beispiel `Draw` :

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

Wenn ein `ExportAttribute` und ein `BrowsableAttribute` mit dem-Argument auf festgelegt `true` sind, wird die-Eigenschaft im **Eigenschaften** Panel des Designers angezeigt. Wenn Sie die-Eigenschaft in ein anderes Bild ändern, das im Projekt enthalten ist, z. b. `FillTexture2.png` , wird das Steuerelement zur Entwurfszeit aktualisiert, wie unten dargestellt:

 [![Bearbeiten von Entwurfszeit Eigenschaften](ios-designable-controls-walkthrough-images/11-customproperty.png)](ios-designable-controls-walkthrough-images/10-app.png#lightbox)

## <a name="summary"></a>Zusammenfassung

In diesem Artikel haben wir erläutert, wie Sie ein benutzerdefiniertes Steuerelement erstellen und es in einer IOS-Anwendung mit dem IOS-Designer nutzen. Wir haben gesehen, wie das Steuerelement erstellt und erstellt wird, um es für eine Anwendung in der **Toolbox**des Designers verfügbar zu machen. Außerdem wurde erläutert, wie das Steuerelement implementiert wird, sodass es sowohl zur Entwurfszeit als auch zur Laufzeit ordnungsgemäß gerendert wird, und wie benutzerdefinierte Steuerelement Eigenschaften im Designer verfügbar gemacht werden.

## <a name="related-links"></a>Ähnliche Themen

- [Scratchticket (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/scratchticket)
- [erforderliche Images (Beispiel)](https://github.com/xamarin/ios-samples/blob/master/ScratchTicket/Resources/images.zip?raw=true)
