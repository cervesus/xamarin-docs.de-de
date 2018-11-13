---
title: Verwenden benutzerdefinierter Steuerelemente im IOS-Designer
description: Dieses Dokument beschreibt, wie Sie ein benutzerdefiniertes Steuerelement erstellen und verwenden sie für iOS mit Xamarin-Designer. Es zeigt, wie das Steuerelement in der iOS-Designer-Toolbox verfügbar zu machen, das Steuerelement zu implementieren, damit sie richtig gerendert und Entwerfen von Zeit und vieles mehr.
ms.prod: xamarin
ms.assetid: 9032B32E-97BD-4DA6-9955-811B84682578
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/22/2017
ms.openlocfilehash: 0401c2c05677c719bbe4914cc7e008b650fdd198
ms.sourcegitcommit: 7eed80186e23e6aff3ddbbf7ce5cd1fa20af1365
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/11/2018
ms.locfileid: "51526240"
---
# <a name="using-custom-controls-with-the-ios-designer"></a>Verwenden benutzerdefinierter Steuerelemente im IOS-Designer

## <a name="requirements"></a>Anforderungen

Xamarin für iOS Designer ist in Visual Studio für Mac und Visual Studio 2015 und 2017 unter Windows verfügbar.

Dieses Handbuch setzt voraus, Vertrautheit mit dem Inhalt in behandelt die [Einstieg führt](~/ios/get-started/index.md).

## <a name="walkthrough"></a>Exemplarische Vorgehensweise

> [!IMPORTANT]
> Beginnen in xamarin.Studio die Datei Exchange 5.5, ist die Möglichkeit, die in der benutzerdefinierten Steuerelemente erstellt werden Unterschied zu früheren Versionen. Um ein benutzerdefiniertes Steuerelement erstellen die `IComponent` Schnittstelle ist erforderlich (mit den zugehörigen Implementierungsmethoden) oder die Klasse kann angemerkt werden, mit `[DesignTimeVisible(true)]`. Die zweite Methode wird im folgenden Beispiel für die exemplarische Vorgehensweise verwendet.


1. Erstellen einer neuen Projektmappe aus der **iOS > App > Single View Application > c#** Vorlage, nennen Sie sie `ScratchTicket`, und durchlaufen Sie den Assistenten für neue Projekte:

    [![](ios-designable-controls-walkthrough-images/01new.png "Erstellen einer neuen Projektmappe")](ios-designable-controls-walkthrough-images/01new.png#lightbox)

1. Erstellen Sie eine neue leere Klassendatei mit dem Namen `ScratchTicketView`:

    [![](ios-designable-controls-walkthrough-images/02new.png "Erstellen Sie eine neue ScratchTicketView-Klasse")](ios-designable-controls-walkthrough-images/02new.png#lightbox)

1. Fügen Sie den folgenden Code für `ScratchTicketView` Klasse:

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


1. Hinzufügen der `FillTexture.png`, `FillTexture2.png` und `Monkey.png` Dateien (verfügbaren [aus GitHub](https://github.com/xamarin/ios-samples/blob/master/ScratchTicket/Resources/images.zip?raw=true)) auf die **Ressourcen** Ordner.
    
1. Doppelklicken Sie auf die `Main.storyboard` Datei, die sie im Designer zu öffnen:

    [![](ios-designable-controls-walkthrough-images/03new.png "Der iOS-Designer")](ios-designable-controls-walkthrough-images/03new.png#lightbox)


1. Drag & Drop eine **Image View** aus der **Toolbox** auf die Ansicht im Storyboard.

    [![](ios-designable-controls-walkthrough-images/04new.png "Ein Image-Ansicht hinzugefügt werden, auf das layout")](ios-designable-controls-walkthrough-images/04new.png#lightbox)


1. Wählen Sie die **Image View** , und ändern Sie seine **Image** Eigenschaft `Monkey.png`.

    [![](ios-designable-controls-walkthrough-images/05new.png "Monkey.png Bild Image-Eigenschaft")](ios-designable-controls-walkthrough-images/05new.png#lightbox)

    
1. Wie wir Größenklassen verwenden, müssen wir diese Bildansicht zu beschränken. Klicken Sie auf das Bild zweimal, um diese Einschränkung Modus versetzen. Lassen Sie uns schränken Sie ihn durch Klicken auf das Handle Center anheften an das Rechenzentrum aus, und sie vertikal und horizontal ausrichten:

    [![](ios-designable-controls-walkthrough-images/06new.png "Das Bild zentrieren")](ios-designable-controls-walkthrough-images/06new.png#lightbox)

1. Um die Höhe und Breite zu beschränken, klicken Sie auf die Handles Größe anheften (die "Knochenarbeit" strukturiert Steuerpunkte), und wählen Sie Breite und Höhe:

    [![](ios-designable-controls-walkthrough-images/07new.png "Hinzufügen von Einschränkungen")](ios-designable-controls-walkthrough-images/07new.png#lightbox)


1. Aktualisieren Sie den Frame basierend auf Einschränkungen, indem Sie auf die Schaltfläche "Aktualisieren" auf der Symbolleiste:

    [![](ios-designable-controls-walkthrough-images/08new.png "Die Symbolleiste für Einschränkungen")](ios-designable-controls-walkthrough-images/08new.png#lightbox)


1. Als Nächstes erstellen Sie das Projekt, damit die **Scratch-Ticket Ansicht** erscheint unter **benutzerdefinierter Komponenten** in der Toolbox:

    [![](ios-designable-controls-walkthrough-images/09new.png "Der benutzerdefinierte Komponenten-toolbox")](ios-designable-controls-walkthrough-images/09new.png#lightbox)


1. Drag & drop eine **Scratch-Ticket Ansicht** , damit es auf das Monkey-Bild angezeigt wird. Passen Sie den Handles, damit die Scratch-Ticket-Ansicht das Monkey-Objekt, wie unten dargestellt, völlig bedeckt:

    [![](ios-designable-controls-walkthrough-images/10new.png "Eine Ansicht Scratch-Ticket für die Image-Sicht")](ios-designable-controls-walkthrough-images/10new.png#lightbox)

1. Schränken Sie die Ansicht, um das Image View Scratch-Ticket, durch ein umschließendes Rechteck zum Auswählen der beiden Ansichten zu zeichnen. Wählen Sie die Optionen, um die Breite, Höhe, Center und mittleren und Update-Frames, die basierend auf Einschränkungen zu beschränken, wie unten dargestellt:

    [![](ios-designable-controls-walkthrough-images/11new.png "Zentrieren und Hinzufügen von Einschränkungen")](ios-designable-controls-walkthrough-images/11new.png#lightbox)


1. Führen Sie die Anwendung, und "scratch-" das Bild, um das Monkey-Objekt anzuzeigen.

    [![](ios-designable-controls-walkthrough-images/10-app.png "Führen Sie eine Beispiel-app")](ios-designable-controls-walkthrough-images/10-app.png#lightbox)

## <a name="adding-design-time-properties"></a>Hinzufügen von Entwurfszeit-Eigenschaften

Der Designer enthält außerdem Unterstützung für benutzerdefinierte Steuerelemente der Eigenschaft Datentyps Numeric-Enumeration, String, "bool", CGSize, UIColor und UIImage während der Entwurfszeit. Um zu demonstrieren, fügen Sie eine Eigenschaft, um die `ScratchTicketView` das Bild festlegen, die "deaktiviert angeschnitten ist."

Fügen Sie den folgenden Code der `ScratchTicketView` -Klasse für die Eigenschaft:

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

Wir könnten auch eine null-Überprüfung zum Hinzufügen der `Draw` -Methode wie folgt:

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

Einschließlich ein `ExportAttribute` und ein `BrowsableAttribute` mit dem Argument festgelegt `true` führt in der Eigenschaft angezeigt wird, im Designer des **Eigenschaft** Bereich. Ändern die Eigenschaft in ein anderes Bild enthalten das Projekt, z. B. `FillTexture2.png`, führt das Steuerelement zur Entwurfszeit aktualisiert wie unten dargestellt:

 [![](ios-designable-controls-walkthrough-images/11-customproperty.png "Entwurfszeit-Eigenschaften bearbeiten")](ios-designable-controls-walkthrough-images/10-app.png#lightbox)

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde erläutert, wie ein benutzerdefiniertes Steuerelement erstellen sowie zur anschließenden Verwendung in einer iOS-Anwendung, die mit dem iOS-Designer. Erläutert, wie zum Erstellen und das Steuerelement, um es zu einer Anwendung in des Designers verfügbar **Toolbox**. Darüber hinaus erläutert, wie Sie das Steuerelement implementieren, damit es ordnungsgemäß zur Entwurfszeit und Laufzeit gerendert wird, sowie wie Sie Eigenschaften für benutzerdefinierte Steuerelement im Designer verfügbar zu machen.



## <a name="related-links"></a>Verwandte Links

- [ScratchTicket (Beispiel)](https://developer.xamarin.com/samples/monotouch/ScratchTicket/)
- [erforderlichen Bilder (Beispiel)](https://github.com/xamarin/ios-samples/blob/master/ScratchTicket/Resources/images.zip?raw=true)
