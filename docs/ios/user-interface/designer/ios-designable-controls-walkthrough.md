---
title: "Exemplarische Vorgehensweise – verwenden benutzerdefinierte Steuerelemente mit dem Xamarin-Designer für iOS"
description: "Dieser Artikel bietet eine schrittweise exemplarische Vorgehensweise zeigt, wie ein benutzerdefiniertes Steuerelement zu erstellen und in die Xamarin-Designer für iOS verwenden. Es wird gezeigt, wie ein Steuerelement zur Verfügung zu stellen in der Toolbox des Designers daher Ziehen/auf eine Sicht nicht gelöscht kann werden. Es zeigt darüber hinaus, wie ein Steuerelement zu implementieren, damit es ordnungsgemäß zur Entwurfs- und Laufzeit gerendert wird, sowie zum Erstellen von Eigenschaften, die zur Entwurfszeit festgelegt werden können."
ms.topic: article
ms.prod: xamarin
ms.assetid: 9032B32E-97BD-4DA6-9955-811B84682578
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: e78b76a531e9f8ea88adca46fc59b2063fce14cc
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="walkthrough---using-custom-controls-with-the-xamarin-designer-for-ios"></a>Exemplarische Vorgehensweise – verwenden benutzerdefinierte Steuerelemente mit dem Xamarin-Designer für iOS

_Dieser Artikel bietet eine schrittweise exemplarische Vorgehensweise zeigt, wie ein benutzerdefiniertes Steuerelement zu erstellen und in die Xamarin-Designer für iOS verwenden. Es wird gezeigt, wie ein Steuerelement zur Verfügung zu stellen in der Toolbox des Designers daher Ziehen/auf eine Sicht nicht gelöscht kann werden. Es zeigt darüber hinaus, wie ein Steuerelement zu implementieren, damit es ordnungsgemäß zur Entwurfs- und Laufzeit gerendert wird, sowie zum Erstellen von Eigenschaften, die zur Entwurfszeit festgelegt werden können._

## <a name="requirements"></a>Anforderungen

Die Xamarin-Designer für iOS ist in Visual Studio für Mac und Visual Studio 2015 und 2017 unter Windows verfügbar.

Dieses Handbuch setzt voraus, Vertrautheit mit den Inhalt in behandelt die [Einstieg führt](~/ios/get-started/index.md).

## <a name="walkthrough"></a>Exemplarische Vorgehensweise

> [!IMPORTANT]
> Xamarin.Studio 5.5 ab, unterscheidet sich die Möglichkeit, die in der benutzerdefinierten Steuerelemente erstellt werden für frühere Versionen. So erstellen ein benutzerdefiniertes Steuerelement entweder die `IComponent` Schnittstelle ist erforderlich (mit den zugehörigen Implementierungsmethoden) oder die Klasse kann als Anmerkung `[DesignTimeVisible(true)]`. Die zweite Methode wird in der folgenden exemplarischen Vorgehensweise wird verwendet.


1. Erstellen Sie eine neue Projektmappe aus der **iOS > App > einzelne Ansicht Anwendung > c#** Vorlage, nennen Sie sie `ScratchTicket`, und folgen Sie der Assistent für neue:


    [![](ios-designable-controls-walkthrough-images/01new.png "Erstellen einer neuen Projektmappe")](ios-designable-controls-walkthrough-images/01new.png)


1. Erstellen Sie eine neue leere Klassendatei mit dem Namen `ScratchTicketView`:


    [![](ios-designable-controls-walkthrough-images/02new.png "Erstellen Sie eine neue ScratchTicketView-Klasse")](ios-designable-controls-walkthrough-images/02new.png)


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


1. Hinzufügen der `FillTexture.png`, `FillTexture2.png` und `Monkey.png` Dateien (verfügbaren [von GitHub](https://github.com/xamarin/ios-samples/blob/master/ScratchTicket/Resources/images.zip?raw=true)), die **Ressourcen** Ordner.
    
1. Doppelklicken Sie auf die `Main.storyboard` Datei im Designer zu öffnen:

    
    [![](ios-designable-controls-walkthrough-images/03new.png "Die iOS-Designer")](ios-designable-controls-walkthrough-images/03new.png)



1. Drag & Drop ein **Image Ansicht** aus der **Toolbox** auf die Ansicht in das Storyboard.

    
    [![](ios-designable-controls-walkthrough-images/04new.png "Ein Image-Ansicht des Layouts hinzugefügt")](ios-designable-controls-walkthrough-images/04new.png)


1. Wählen Sie die **Bild anzeigen** , und ändern Sie seine **Image** Eigenschaft `Monkey.png`.

    
    [![](ios-designable-controls-walkthrough-images/05new.png "Monkey.png Bild Image-Eigenschaft")](ios-designable-controls-walkthrough-images/05new.png)

    
1. Wie wir Größenklassen verwenden, müssen wir hier Bild zu beschränken. Klicken Sie auf das Bild zweimal, um diese Einschränkung Modus versetzen. Wir schränken Sie ihn an das Rechenzentrum durch Klicken auf das Handle Center anheften, und richten Sie es sowohl vertikal und horizontal:
    
    
    [![](ios-designable-controls-walkthrough-images/06new.png "Zentrieren das Bild")](ios-designable-controls-walkthrough-images/06new.png)

    
1. Um die Höhe und Breite zu beschränken, klicken Sie auf die Handles Größe anheften (die "Bone" geformten Handles) auf, und wählen Sie die Breite und Höhe:

    
    [![](ios-designable-controls-walkthrough-images/07new.png "Hinzufügen von Einschränkungen")](ios-designable-controls-walkthrough-images/07new.png)


1. Aktualisieren Sie den Frame basierend auf Einschränkungen, indem Sie auf die Schaltfläche "Aktualisieren" auf der Symbolleiste:


    [![](ios-designable-controls-walkthrough-images/08new.png "Die Einschränkungen-Symbolleiste")](ios-designable-controls-walkthrough-images/08new.png)


1. Als Nächstes erstellen Sie das Projekt, damit die **Scratch Ticket Ansicht** erscheint unter **benutzerdefinierte Komponenten** in der Toolbox:

    
    [![](ios-designable-controls-walkthrough-images/09new.png "Der benutzerdefinierte Komponenten-toolbox")](ios-designable-controls-walkthrough-images/09new.png)


1. Drag & drop eine **Scratch Ticket Ansicht** , damit es auf das Bild Affe angezeigt wird. Passen Sie den Ziehpunkten so, dass der Speicherbereich Ticket-Ansicht der Affe abdeckt, die vollständig, wie unten dargestellt:

    
    [![](ios-designable-controls-walkthrough-images/10new.png "Eine Sicht Scratch Ticket für die Image-Sicht")](ios-designable-controls-walkthrough-images/10new.png)


1. Schränken Sie die Scratch Ticket Ansicht anzuzeigende Bild durch ein umschließendes Rechteck zum Auswählen der beiden Ansichten zu zeichnen. Wählen Sie die Optionen, die in der Breite, Höhe, Center und mittleren und Update-Frames, die basierend auf Einschränkungen zu beschränken, wie unten dargestellt:
 
    
    [![](ios-designable-controls-walkthrough-images/11new.png "Zentrieren und Hinzufügen von Einschränkungen")](ios-designable-controls-walkthrough-images/11new.png)


1. Führen Sie die Anwendung, und "scratch deaktiviert" das Bild, um die Affe anzuzeigen.


 [ ![](ios-designable-controls-walkthrough-images/10-app.png "Eine Beispiel-app ausführen")](ios-designable-controls-walkthrough-images/10-app.png)

## <a name="adding-design-time-properties"></a>Hinzufügen von Eigenschaften zur Entwurfszeit

Der Designer umfasst auch zur Entwurfszeit Unterstützung für benutzerdefinierte Steuerelemente des Eigenschaft-Datentyps Numeric, Enumeration, String, Bool, CGSize, UIColor und UIImage. Um zu demonstrieren, fügen Sie eine Eigenschaft an, die `ScratchTicketView` das Bild festlegen, die "deaktiviert berührt werden."

Fügen Sie folgenden Code, der `ScratchTicketView` Klasse für die Eigenschaft:

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

Wir können auch eine Überprüfung auf null, Hinzufügen der `Draw` -Methode, wie folgt:

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

Z. B. ein `ExportAttribute` und ein `BrowsableAttribute` mit dem Argument festgelegt `true` führt in der Eigenschaft, die im Designer angezeigt wird **Eigenschaft** Bereich. Ändern der Eigenschaft zu einem anderen Bild mit dem Projekt enthalten sind, z. B. `FillTexture2.png`, führt das Steuerelement aktualisieren zur Entwurfszeit wie unten dargestellt:

 [ ![](ios-designable-controls-walkthrough-images/11-customproperty.png "Entwurfszeit-Eigenschaften bearbeiten")](ios-designable-controls-walkthrough-images/10-app.png)

## <a name="summary"></a>Zusammenfassung

Durchlaufen in diesem Artikel wird erläutert, wie ein benutzerdefiniertes Steuerelement erstellen, als auch dafür in einer iOS-Anwendung mit dem iOS-Designer. Wurde erläutert, wie erstellen, und erstellen Sie das Steuerelement, um es in des Designers für eine Anwendung verfügbar zu machen **Toolbox**. Darüber hinaus haben wir, wie das Steuerelement implementiert wird, dass er ordnungsgemäß zur Entwurfs- und Laufzeit gerendert wird und wie Eigenschaften für benutzerdefinierte Steuerelement im Designer verfügbar machen.



## <a name="related-links"></a>Verwandte Links

- [ScratchTicket (Beispiel)](https://developer.xamarin.com/samples/monotouch/ScratchTicket/)
- [erforderliche Bilder (Beispiel)](https://github.com/xamarin/ios-samples/blob/master/ScratchTicket/Resources/images.zip?raw=true)
