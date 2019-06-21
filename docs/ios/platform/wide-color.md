---
title: Breite Farbskala in Xamarin.iOS
description: In diesem Dokument wird erläutert, Breite Farbskala und wie es in einer Xamarin.iOS oder Xamarin.Mac-app verwendet werden kann.
ms.prod: xamarin
ms.assetid: 576E978A-F182-489A-83E4-D8CDC6890B24
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
ms.openlocfilehash: 523aa1a97e1c536b5afbdb66c52f605382fa338d
ms.sourcegitcommit: a153623a69b5cb125f672df8007838afa32e9edf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2019
ms.locfileid: "67268808"
---
# <a name="wide-color-in-xamarinios"></a>Breite Farbskala in Xamarin.iOS

_Dieser Artikel behandelt die Breite Farbskala, und wie es in einer Xamarin.iOS oder Xamarin.Mac-app verwendet werden kann._

iOS 10 und MacOS Sierra verbessert die Unterstützung für erweiterte-Range-Pixelformate "und" Wide-Farbskala Farbräume im gesamten System einschließlich Frameworks wie z. B. Core Graphics, Core-Image, -Metal-Computern und AVFoundation. Unterstützung für Geräte mit Breite Farbskala zeigt wird weiter durch die Bereitstellung dieses Verhalten in der gesamten Grafikstapel geringer.

## <a name="asset-catalogs"></a>Ressourcenkataloge

### <a name="supporting-wide-color-with-asset-catalogs"></a>Breite Farbskala mit Ressourcenkataloge unterstützen

In iOS 10 und MacOS Sierra hat Apple Ressourcenkataloge und kategorisieren statischen Inhalt in der app-Paket verwendet, um Breite Farbskala unterstützen erweitert.

Mithilfe von Ressourcenkataloge bieten folgende Vorteile für eine app:

- Sie bieten die beste Bereitstellungsoption für statisches Bild Assets.
- Automatische Farbkorrektur unterstützt.
- Optimierung der automatische Pixel-Format unterstützt.
- Sie unterstützen App Aufteilen in Slices und Einhaltung der Regeln für App Dadurch wird sichergestellt, dass nur der Inhalt des relevanten Get an Gerät des Benutzers übermittelt.

Apple hat Ressourcenkataloge die folgenden Verbesserungen für die Breite Farbskala-Unterstützung vorgenommen:

- Inhalt der 16-Bit-(pro Farbkanal) unterstützt.
- Sie unterstützen dieses Inhalts durch Anzeige Farbskala. Inhalte kann für das sRGB oder Display P3 Farbumfänge gekennzeichnet werden.

Der Entwickler hat drei Optionen für die Unterstützung von Breite Farbskala Inhalt in ihre apps:

1. **Nichts Unternehmen** -da Breite Farbskala Inhalt nur in Situationen verwendet werden soll, in dem die app muss lebendige Farben angezeigt (wobei wird erhöht, wenn Sie die benutzerfreundlichkeit), sollte auch außerhalb dieser Anforderung Inhalte bleiben als-ist. Diese Funktion bleibt weiterhin in allen Situationen von Hardware ordnungsgemäß gerendert werden soll.
2. **Aktualisieren Sie vorhandene Inhalt auf Display P3** -dies erfordert, dass den Entwickler, ersetzen Sie den Inhalt des vorhandenen in ihrer Asset-Katalog durch eine neue, aktualisierte Datei im Format P3, und markieren es als solches im Ressourcen-Editor. Zur Erstellungszeit wird ein abgeleitetes sRGB-Image aus diesen Ressourcen generiert werden.
3. **Optimierte Asset Inhalt bereitstellen** -Entwickler werden In diesem Fall geben Sie eine 8-Bit-sRGB und eine 16-Bit-Display P3 Vision jedes Bilds in den Asset-Katalog (ordnungsgemäß gekennzeichnet in den Ressourcen-Editor).

### <a name="asset-catalog-deployment"></a>Asset-Katalog-Bereitstellung

Folgendes geschieht, wenn der Entwickler eine app in der App-Store mit Ressourcenkataloge übermittelt, die Breite Farbskala Inhalt enthalten:

- Wenn die app für den Endbenutzer bereitgestellt wird, werden App Aufteilen in Slices stellen Sie sicher, dass nur die entsprechenden Inhalte Variante an das Gerät des Benutzers gesendet wird.
- Auf Gerät, das breite Farbskala nicht unterstützen, fallen es keine Kosten Nutzlast für Breite Farbskala Inhalt, einschließlich, wie sie nie auf dem Gerät geliefert wird.
- `NSImage` unter MacOS Sierra (und höher) wählt automatisch die beste Darstellung des Inhalte für den Hardware Anzeige.
- Der angezeigte Inhalt wird automatisch aktualisiert werden, wann oder ob die Geräte-Hardware Ändern der Eigenschaften anzuzeigen.

### <a name="asset-catalog-storage"></a>Asset-Katalog-Speicher

Asset-Katalog-Speicher hat die folgenden Eigenschaften und die Auswirkungen auf die für eine app:

- Apple versucht, beim Erstellen der Speicherung von den Bildinhalt über hohe Qualität Image Konvertierungen zu optimieren.
- 16 Bits werden pro Farbkanal für Breite Farbskala Inhalt verwendet.
- Inhalt der abhängigen bildkomprimierung wird verwendet, lieferleistungen Inhaltsgrößen zu senken. Neue "verlustbehaftete" protokollkomprimierungen wurden hinzugefügt, um Inhaltsgrößen weiter zu optimieren.

## <a name="rendering-off-screen-images-in-app"></a>Rendern von außerhalb des Bildschirms Bildern in-App

Basierend auf den Typ der app, die erstellt wird, kann es ermöglicht dem Benutzer den Inhalt einschließen, sie über das Internet erfasst haben, oder erstellen Sie den Inhalt direkt innerhalb der app (z. B. einen Vektor, zeichnen die app z. B.).

In beiden Fällen kann die app die erforderlichen Bilder außerhalb des Bildschirms in Breite Farbskala Verwenden erweiterter Features hinzugefügt, die für IOS- und MacOS rendern.

### <a name="drawing-wide-color-in-ios"></a>Zeichnen Breite Farbskala in iOS

Vor dem erörtert, wie Sie eine Breite Farbskala-Bild unter iOS 10 ordnungsgemäß zu zeichnen, sehen Sie sich die folgenden allgemeinen iOS-Zeichencode:

```csharp
public UIImage DrawWideColorImage ()
{
    var size = new CGSize (250, 250);
    UIGraphics.BeginImageContext (size);

    ...

    UIGraphics.EndImageContext ();
    return image = UIGraphics.GetImageFromCurrentImageContext ();
    }
```

Es gibt Probleme mit der standard-Code, die berücksichtigt werden müssen, _vor_ dienen zum Zeichnen eines Bilds Breite Farbskala. Die `UIGraphics.BeginImageContext (size)` Methode, die zum Zeichnen der iOS-Image starten, gelten die folgenden Einschränkungen:

- Es kann nicht Image-Kontexten mit mehr als 8 Bits pro Farbkanal erstellen.
- Es kann keine Farben in den Extended Range sRGB-Farbraum dar.
- Es muss nicht die Möglichkeit, eine Schnittstelle zum Erstellen von Kontexten in einer nicht-sRGB-Farbraums aufgrund der aufgerufen wird, im Hintergrund C-Routine auf niedriger Ebene bereitzustellen.

Um diese Einschränkungen zu verarbeiten, und zeichnen ein Image Breite Farbskala unter iOS 10, verwenden Sie stattdessen den folgenden Code:

```csharp
public UIImage DrawWideColorImage ()
{
    var size = new CGSize (250, 250);
    var render = new UIGraphicsImageRenderer (size);

    var image = render.CreateImage ((UIGraphicsImageRendererContext context) => {
        var bounds = context.Format.Bounds;
        CGRect slice;
        CGRect remainder;
        bounds.Divide (bounds.Width / 2, CGRectEdge.MinXEdge, out slice, out remainder);

        var redP3 = UIColor.FromDisplayP3 (1.0F, 0.0F, 0.0F, 1.0F);
        redP3.SetColor ();
        context.FillRect (slice);

        var redRGB = UIColor.Red;
        redRGB.SetColor ();
        context.FillRect (remainder);
    });

    // Return results
    return image;
}
```

Die neue `UIGraphicsImageRenderer` Klasse erstellt einen neuen Image-Kontext, der den Extended Range sRGB-Farbraum verarbeiten kann, und es bietet die folgenden Features:

- Er ist vollständig Farbe, die standardmäßig verwaltet.
- Der Extended Range sRGB-Farbraum standardmäßig unterstützt.
- Es legt auf intelligente Weise auf, wenn es im sRGB oder Extended Range sRGB-Farbraum basierend auf den Funktionen des iOS-Geräts, die die app ausgeführt wird, auf gerendert werden soll.
- Vollständig und automatisch verwaltet den Image-Kontext (`CGContext`) Lebensdauer, sodass der Entwickler nicht um Aufruf kümmern Begin- und end-Befehle im Kontext.
- Es ist kompatibel mit der `UIGraphics.GetCurrentContext()` Methode.

Die `CreateImage` Methode der `UIGraphicsImageRenderer` Klasse aufgerufen, um eine Breite Farbskala-Image zu erstellen ist, und übergeben Sie einen Abschlusshandler mit dem Image-Kontext zum Zeichnen in. Alle der Zeichnung erfolgt innerhalb dieser Abschlusshandler wie folgt:

- Die `UIColor.FromDisplayP3` -Methode erstellt eine neue gesättigte rote Farbe in der Breite Farbskala Display P3-Farbraum und es wird verwendet, um die erste Hälfte des Rechtecks gezeichnet werden soll. 
- Die zweite Hälfte des Rechtecks gezeichnet wird in den normalen sRGB vollständig ausgelastet rote Farbe für den Vergleich.

### <a name="drawing-wide-color-in-macos"></a>Zeichnen Breite Farbskala in macOS

Die `NSImage` Klasse wurde erweitert, unter MacOS Sierra, um die Anzeige der Breite Farbskala Images zu unterstützen. Zum Beispiel:

```csharp
var size = CGSize(250,250);
var wideColorImage = new NSImage(size, false, (drawRect) =>{
    var rects = drawRect.Divide(drawRect.Size.Width/2,CGRect.MinXEdge);

    var color = new NSColor(1.0, 0.0, 0.0, 1.0).Set();
    var path = new NSBezierPath(rects.Slice).Fill();

    color = new NSColor(1.0, 0.0, 0.0, 1.0).Set();
    path = new NSBezierPath(rects.Remainder).Fill();

    // Return modified
    return true;
});
```

## <a name="rendering-on-screen-images-in-app"></a>In-App-Images auf dem Bildschirm rendern

Um Breite Farbskala Images auf dem Bildschirm zu rendern, verläuft der Prozess ähnelt, Zeichnen eines Bilds außerhalb des Bildschirms Breite Farbskala für MacOS und iOS oben angezeigt.

### <a name="rendering-on-screen-in-ios"></a>Rendern von auf dem Bildschirm in iOS

Überschreiben, wenn die app ein Bild in der Breite Farbskala in iOS auf dem Bildschirm gerendert werden soll muss, die `Draw` Methode der `UIView` fraglichen wie gewohnt. Zum Beispiel:

```csharp
using System;
using UIKit;
using CoreGraphics;

namespace MonkeyTalk
{
    public class MonkeyView : UIView
    {
        public MonkeyView ()
        {
        }

        public override void Draw (CGRect rect)
        {
            base.Draw (rect);

            // Draw the image to screen
        ...
        }
    }
}
```

IOS 10 ebenso mit der `UIGraphicsImageRenderer` Klasse, die oben dargestellte Intelligent entscheidet, wenn es im sRGB oder Extended Range sRGB-Farbraum basierend auf den Funktionen des iOS-Geräts, die die app, wann ausgeführt wird gerendert werden sollen die `Draw` Methode wird aufgerufen. Darüber hinaus die `UIImageView` Farbe, die seit iOS 9.3 auch verwaltet wurde.

Wenn die app muss wissen, wie Rendering auf erfolgt eine `UIView` oder `UIViewController`, sehen sie die neue `DisplayGamut` Eigenschaft der `UITraitCollection` Klasse. Dieser Wert eine `UIDisplayGamut` Enumeration der folgenden:

- P3
- Srgb
- Nicht angegeben.

Wenn die app steuern möchte, die Farbraum verwendet wird dass, um ein Bild zu zeichnen, können sie ein neues verwenden `ContentsFormat` Eigenschaft der `CALayer` an die gewünschte-Farbraum. Dieser Wert kann eine `CAContentsFormat` Enumeration der folgenden:

- Gray8Uint
- Rgba16Float
- Rgba8Uint

### <a name="rendering-on-screen-in-macos"></a>Rendern von auf dem Bildschirm in macOS

Überschreiben, wenn die app ein Bild in der Breite Farbskala in MacOS auf dem Bildschirm gerendert werden soll muss, die `DrawRect` Methode der `NSView` fraglichen wie gewohnt. Zum Beispiel:

```csharp
using System;
using AppKit;
using CoreGraphics;
using CoreAnimation;

namespace MonkeyTalkMac
{
    public class MonkeyView : NSView
    {
        public MonkeyView ()
        {
        }

        public override void DrawRect (CGRect dirtyRect)
        {
            base.DrawRect (dirtyRect);

            // Draw graphics into the view
            ...
        }
    }
}
```

In diesem Fall es auf intelligente Weise entscheidet, ob es rendern soll, im sRGB oder Extended Range sRGB-Farbraum basierend auf den Funktionen der Mac-Hardware, die die app, wann ausgeführt wird die `DrawRect` Methode wird aufgerufen.

Wenn die app steuern möchte, die Farbraum verwendet wird dass, um ein Bild zu zeichnen, können sie ein neues verwenden `DepthLimit` Eigenschaft der `NSWindow` Klasse, um den gewünschten Farbraum anzugeben. Dieser Wert kann eine `NSWindowDepth` Enumeration der folgenden:

- OneHundredTwentyEightBitRgb
- SixtyfourBitRgb
- TwentyfourBitRgb
