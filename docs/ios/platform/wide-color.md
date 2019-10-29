---
title: Breite Farbe in xamarin. IOS
description: In diesem Dokument wird die Breite und die Verwendung in einer xamarin. IOS-oder xamarin. Mac-app erläutert.
ms.prod: xamarin
ms.assetid: 576E978A-F182-489A-83E4-D8CDC6890B24
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/17/2017
ms.openlocfilehash: e7240a271de1f0199c2c9fc045f5c95745eb98c5
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73031245"
---
# <a name="wide-color-in-xamarinios"></a>Breite Farbe in xamarin. IOS

_Dieser Artikel befasst sich mit der breiten Farbe und der Verwendung in einer xamarin. IOS-oder xamarin. Mac-app._

IOS 10 und macOS Sierra verbessern die Unterstützung für erweiterte Pixel Formate und große Farbbereiche im gesamten System, einschließlich Frameworks wie Kern Grafiken, Core-Bild, Metal und AVFoundation. Die Unterstützung für Geräte mit breit Farbanzeige wird weiter vereinfacht, indem dieses Verhalten im gesamten Grafik Stapel bereitgestellt wird.

## <a name="asset-catalogs"></a>Asset-Kataloge

### <a name="supporting-wide-color-with-asset-catalogs"></a>Unterstützen von breit Farben mit Asset-Katalogen

In ios 10 und macOS Sierra hat Apple die Asset-Kataloge erweitert, die zum einschließen und Kategorisieren von statischem Bildinhalt im Paket der APP verwendet werden, um die Breite der Farbe zu unterstützen.

Die Verwendung von Asset-Katalogen bietet die folgenden Vorteile für eine APP:

- Sie bieten die beste Bereitstellungs Option für statische Image Ressourcen.
- Sie unterstützen die automatische Farbkorrektur.
- Sie unterstützen die automatische Pixel Format Optimierung.
- Sie unterstützen das Aufteilen von apps und die APP-Verdünnung, wodurch sichergestellt wird, dass nur die relevanten Inhalte an das Gerät des Endbenutzers übermittelt werden.

Apple hat die folgenden Verbesserungen an Asset-Katalogen für die Unterstützung von breit Farben vorgenommen:

- Sie unterstützen den Inhalt von 16-Bit-Quellen (pro Farbkanal).
- Sie unterstützen das Katalogisieren von Inhalten nach Anzeige Gamut. Der Inhalt kann entweder für den sRGB-oder den Display P3-Gamuts gekennzeichnet werden.

Der Entwickler bietet drei Optionen für die Unterstützung von breit Farbinhalten in ihren apps:

1. **Nichts Unternehmen** : da Breite Farb Inhalte nur in Situationen verwendet werden sollen, in denen die APP anschauliche Farben anzeigen muss (wo Sie die Benutzer Funktionalität verbessern), sollte der Inhalt außerhalb dieser Anforderung unverändert bleiben. Sie wird in allen Hardware Situationen weiterhin ordnungsgemäß gerendert.
2. **Aktualisieren vorhandener Inhalte zum Anzeigen von P3** : Dies erfordert, dass der Entwickler den vorhandenen Bildinhalt in seinem Ressourcen Katalog durch eine neue, aktualisierte Datei im P3-Format ersetzt und ihn im Asset-Editor als solche kennzeichnen muss. Zum Zeitpunkt der Erstellung wird ein von diesen Assets abgeleitetes sRGB-Abbild generiert.
3. **Bereitstellen von optimierten Asset-Inhalten** : in diesem Fall stellt der Entwickler sowohl eine 8-Bit-sRGB als auch eine 16-Bit-Display-P3-Vision für jedes Image im Asset-Katalog bereit (im Asset-Editor ordnungsgemäß gekennzeichnet).

### <a name="asset-catalog-deployment"></a>Asset Catalog-Bereitstellung

Folgendes wird angezeigt, wenn der Entwickler eine APP mit Asset-Katalogen an den App Store übermittelt, die Bildinhalte für Breite Farben enthalten:

- Wenn die APP für den Endbenutzer bereitgestellt wird, stellt die APP-slizierung sicher, dass nur die entsprechende Inhalts Variante an das Gerät des Benutzers übermittelt wird.
- Auf Geräten, die keine Breite Farbe unterstützen, gibt es keine Nutz Last Kosten für die Einbeziehung von breit Farbinhalten, da diese nie an das Gerät ausgeliefert werden.
- bei der `NSImage` auf macOS Sierra (und höher) wird automatisch die beste Inhalts Darstellung für die Hardware Anzeige ausgewählt.
- Der angezeigte Inhalt wird automatisch aktualisiert, wenn oder die Merkmale der Hardware Anzeige der Geräte geändert werden.

### <a name="asset-catalog-storage"></a>Asset Catalog-Speicher

Der Asset Catalog-Speicher verfügt über die folgenden Eigenschaften und Implikationen für eine APP:

- Zum Zeitpunkt der Erstellung versucht Apple, den Speicher des Bildinhalts über qualitativ hochwertige Bild Konvertierungen zu optimieren.
- 16 Bits werden pro Farbkanal für Breite Farb Inhalte verwendet.
- Die Inhalts abhängige Bildkomprimierung wird verwendet, um die Größe des Inhalts zu verringern. Zur weiteren Optimierung der Inhalts Größen wurden neue "Lossy"-Komprimierungen hinzugefügt.

## <a name="rendering-off-screen-images-in-app"></a>Rendern von Offscreen-Bildern in der APP

Basierend auf dem Typ der zu erstellenden App kann der Benutzer möglicherweise Bildinhalte einschließen, die er aus dem Internet gesammelt hat, oder Bildinhalte direkt in der APP erstellen (z. b. eine Vector Drawing APP).

In beiden Fällen kann die APP die erforderlichen Bilder in der breiten Farbe von einem Bildschirm mit erweiterten Features, die IOS-und macOS-Funktionen hinzugefügt werden

### <a name="drawing-wide-color-in-ios"></a>Zeichnen von breit Farben in ios

Sehen Sie sich den folgenden gängigen IOS-Zeichnungs Code an, bevor Sie erfahren, wie Sie ein breit farbiges Bild in ios 10 korrekt zeichnen:

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

Es gibt Probleme mit dem Standard Code, der behandelt werden muss, _bevor_ er zum Zeichnen eines breiten Farbbilds verwendet werden kann. Die zum Starten der IOS-Image Zeichnung verwendete `UIGraphics.BeginImageContext (size)` Methode hat die folgenden Einschränkungen:

- Es können keine Bild Kontexte mit mehr als 8 Bits pro Farbkanal erstellt werden.
- Sie kann keine Farben im erweiterten Bereich der sRGB-Farbraum darstellen.
- Sie verfügt nicht über die Möglichkeit, eine Schnittstelle bereitzustellen, um Kontexte in einem nicht-sRGB-Farbraum zu erstellen, da die C-Routinen auf niedriger Ebene im Hintergrund aufgerufen werden.

Verwenden Sie stattdessen den folgenden Code, um diese Einschränkungen zu behandeln und ein breit farbiges Bild in ios 10 zu zeichnen:

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

Die neue `UIGraphicsImageRenderer`-Klasse erstellt einen neuen Bildkontext, der den erweiterten Bereich von sRGB-Farbraum verarbeiten kann und über die folgenden Funktionen verfügt:

- Der Standardwert ist vollständig Farben verwaltet.
- Der erweiterte Bereich von sRGB-Farbraum wird standardmäßig unterstützt.
- Basierend auf den Funktionen des IOS-Geräts, auf dem die app ausgeführt wird, entscheidet es intelligent, ob es im sRGB-oder erweiterten Bereich der sRGB-farbliche Größe renderert werden sollte.
- Die Lebensdauer des Bild Kontexts (`CGContext`) wird vollständig und automatisch verwaltet, sodass sich der Entwickler nicht um das Aufrufen von BEGIN-und End-Kontext Befehlen kümmern muss.
- Es ist mit der `UIGraphics.GetCurrentContext()`-Methode kompatibel.

Die `CreateImage`-Methode der `UIGraphicsImageRenderer`-Klasse wird aufgerufen, um ein breit Farbbild zu erstellen und einen Vervollständigungs Handler mit dem Bildkontext zu übertragen, in den gezeichnet werden soll. Alle Zeichnungen werden innerhalb dieses Vervollständigungs Handlers wie folgt durchgeführt:

- Die `UIColor.FromDisplayP3`-Methode erstellt eine neue vollständig satte rote Farbe im großen Bereich, um P3-Farbraum anzuzeigen, und Sie wird verwendet, um die erste Hälfte des Rechtecks zu zeichnen. 
- Die zweite Hälfte des Rechtecks wird in der normalen sRGB vollständig satte rote Farbe für den Vergleich gezeichnet.

### <a name="drawing-wide-color-in-macos"></a>Zeichnen von breit Farben in macOS

Die `NSImage`-Klasse wurde in macOS Sierra erweitert, um das Zeichnen von breit Farbbildern zu unterstützen. Beispiel:

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

## <a name="rendering-on-screen-images-in-app"></a>Rendern von Bildschirmbildern in der APP

Um breite Farbbilder auf dem Bildschirm zu Rendering, funktioniert der Prozess ähnlich wie das Zeichnen eines Bildschirms für die Bildschirmbreite für macOS und IOS.

### <a name="rendering-on-screen-in-ios"></a>Rendering auf dem Bildschirm in ios

Wenn die APP ein Bild auf der Bildschirm Breite auf dem Bildschirm in ios Rendering muss, überschreiben Sie die `Draw`-Methode des fraglichen `UIView` wie üblich. Beispiel:

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

Da IOS 10 mit der oben gezeigten `UIGraphicsImageRenderer`-Klasse ausgeführt wird, entscheidet es intelligent, ob es im sRGB-oder Extended Range sRGB-Farbraum renderingbasiert, abhängig von den Funktionen des IOS-Geräts, auf dem die app ausgeführt wird, wenn die `Draw`-Methode aufgerufen wird. Außerdem wurde die `UIImageView` seit IOS 9,3 farblich verwaltet.

Wenn die APP wissen muss, wie das Rendering bei einem `UIView` oder `UIViewController`erfolgt, kann die neue `DisplayGamut`-Eigenschaft der `UITraitCollection`-Klasse überprüft werden. Dieser Wert ist eine `UIDisplayGamut` Enumeration der folgenden:

- P3
- SRGB
- Nicht angegeben.

Wenn die App steuern möchte, welcher Farbraum zum Zeichnen eines Bilds verwendet wird, kann eine neue `ContentsFormat`-Eigenschaft des `CALayer` verwendet werden, um den gewünschten Farbraum anzugeben. Dieser Wert kann eine `CAContentsFormat` Enumeration der folgenden sein:

- Gray8Uint
- Rgba16Float
- Rgba8Uint

### <a name="rendering-on-screen-in-macos"></a>Rendering auf dem Bildschirm unter macOS

Wenn die APP ein Bild auf der Bildschirm Breite auf dem Bildschirm in macOS darstellen muss, überschreiben Sie die `DrawRect`-Methode des fraglichen `NSView` wie üblich. Beispiel:

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

Es wird auch hier Intelligent entschieden, ob der sRGB-oder erweiterte sRGB-Farbraum basierend auf den Funktionen der Macintosh-Hardware, auf der die app ausgeführt wird, wenn die `DrawRect`-Methode aufgerufen wird, renderert werden soll.

Wenn die App steuern möchte, welcher Farbraum zum Zeichnen eines Bilds verwendet wird, kann eine neue `DepthLimit`-Eigenschaft der `NSWindow`-Klasse verwendet werden, um den gewünschten Farbraum anzugeben. Dieser Wert kann eine `NSWindowDepth` Enumeration der folgenden sein:

- Onehundredtwentyeightbitrgb
- Sixtyfourbitrgb
- Twentyfourbitrgb
