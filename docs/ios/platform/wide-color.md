---
title: Breite Farbskala in Xamarin.iOS
description: In diesem Dokument wird erläutert, Breite Farbskala und wie es in einer Xamarin.iOS oder Xamarin.Mac-app verwendet werden kann. Darüber hinaus einen allgemeinen Überblick über viele wichtige farbbezogene Konzepte wie z. B. Farbräume, Kanäle und primären Replikaten.
ms.prod: xamarin
ms.assetid: 576E978A-F182-489A-83E4-D8CDC6890B24
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
ms.openlocfilehash: f139bcceda12752e43a3a8330fa0a0e038e539f9
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50121307"
---
# <a name="wide-color-in-xamarinios"></a>Breite Farbskala in Xamarin.iOS

_Dieser Artikel behandelt die Breite Farbskala, und wie es in einer Xamarin.iOS oder Xamarin.Mac-app verwendet werden kann._

iOS 10 und MacOS Sierra verbessert die Unterstützung für erweiterte-Range-Pixelformate "und" Wide-Farbskala Farbräume im gesamten System einschließlich Frameworks wie Core Graphics "," Core-Image ","-Metal-Computern "und" AVFoundation. Unterstützung für Geräte mit Breite Farbskala zeigt wird weiter durch die Bereitstellung dieses Verhalten in der gesamten Grafikstapel geringer.

## <a name="about-wide-color"></a>Informationen zu Breite Farbskala

Wie bereits erwähnt, verbessert mit iOS 10 und MacOS Sierra die Unterstützung für erweiterte-Range-Pixelformate "und" Wide-Farbskala Farbräume im gesamten System, einschließlich des Frameworks wie Core Graphics "," Core-Image ","-Metal-Computern "und" AVFoundation. Unterstützung für Geräte mit Breite Farbskala zeigt wird weiter durch die Bereitstellung dieses Verhalten in der gesamten Grafikstapel geringer.

In der 90er Jahre erstellt Apple ColorSync zum Behandeln von Farbe, die Verarbeitung auf dem Mac. Außerdem hat das Unternehmen die International Color Consortium (Chipkarte) erstellen, und Stufen einen Satz von Standards für die Behandlung von Farbe auf Computerhardware gefunden. Apple-arbeiten mit Chipkarte im ColorSync enthalten war, und sie in den Kern von OS X (jetzt als MacOS bezeichnet) erstellt wurde.

Apple wurde auch an der Spitze der Technologie der Anzeige in der Hardware wie z. B. die Retina anzuzeigen, die neue P3 anzuzeigen und anzeigen P3-Farbraum (in der iMac 2015 veröffentlicht) und die TrueTone zeigt an, in der iPad-Experten, iPhone 7 und iPhone 7 Plus.

Breite Farbskala unter MacOS Sierra und iOS 10 verändert Apple die Art, MacOS und iOS Farbe verarbeitet, um dieser neuen Technologien für die Anzeige zu nutzen.

## <a name="core-color-concepts"></a>Farbe der Kernkonzepte

Die folgenden grundlegenden Farbe Konzepte benötigen, die behandelt werden, bevor Sie einen tieferen Einblick in die Farbe in MacOS und iOS:

### <a name="color-space"></a>Farbraum

Ein Farbraum ist eine Umgebung, in der Farben dargestellt und verglichen werden können. Es kann eine bis zu vier-dimensionalen Raum sein, die durch die Intensität der Farbe Komponenten definiert ist. 

[![](wide-color-images/color00.png "Ein Farbraum")](wide-color-images/color00.png#lightbox)

### <a name="color-channels"></a>Farbkanäle

Die Farbe-Komponenten können auch als Farbkanäle bezeichnet werden. Einige vertrauten Darstellungen wäre die RGB-Leerzeichen, grau Leerzeichen, CMYK Leerzeichen oder Gerät unabhängig Leerzeichen. 

[![](wide-color-images/color02.png "Die Farbe-Komponenten können auch als Farbkanäle bezeichnet werden")](wide-color-images/color02.png#lightbox)

### <a name="color-primaries"></a>Grundfarben

Grundfarben bieten das Koordinatensystem, das verwendet wird, zu vergleichen und die Farben zu berechnen. Grundfarben gehören normalerweise auf der höchsten Intensives Version der angegebenen Farbe, die innerhalb der Farbkanal generiert werden kann.

[![](wide-color-images/color01.png "Grundfarben Geben Sie das Koordinatensystem, mit dem verglichen werden soll, und compute-Farben")](wide-color-images/color01.png#lightbox)

Im Fall der RGB-Farbspektrum oben dargestellt, sind die Grundfarben Where der `1.0` Koordinaten verankert sind (z. B. `[1.0, 0.0, 0.0]` für Rot).

### <a name="color-gamut"></a>Farbskala

Farbskala bezieht sich auf alle Farben, die als Kombination aus den einzelnen-Kanal innerhalb einer bieten Farbraum definiert werden können.

[![](wide-color-images/color03.png "Beispiel für die gesamte Farbe")](wide-color-images/color03.png#lightbox)

## <a name="what-is-wide-color"></a>Was ist eine Breite Farbskala

Vor dem behandelt des Themas der Breite Farbskala, sollte eine Diskussion über der aktuellen Industriestandard für Farbe, den standardmäßigen RGB-Farbraum (sRGB) mussten werden. Dabei handelt es sich die am häufigsten verwendeten Farbraum bei der Berechnung von heute ist der Standard-Farbraum für iOS und MacOS.

Der sRGB-Farbraum hat die folgenden Eigenschaften:

- Er basiert auf dem Standard der ITU-R-BT.709.
- Er verfügt über eine ungefähre dessen Gamma 2.2.
- Es stellt die typische Lichtverhältnissen (D65) dar.

Da sRGB wird häufig verwendet, in der Branche, ein Entwickler bestimmte Annahmen, die vornehmen können wird die angegebene Farbe enthalten auf allen Geräten dargestellt werden, die er angezeigt wird. Allerdings kann dies immer die Groß-/Kleinschreibung nicht. Darüber hinaus stehen verschiedene Farben, die passen nicht in den sRGB-Farbraums und Verlaufsebene, nicht darin dargestellt werden können.

Beispielsweise werden viele Textilien mit Threads, die viele Tinten und außerhalb von sRGB fallen Farbstoffe entworfen. Auch werden viele Produkte, die eine Person in ihrer täglichen Lebens trifft mit hellen, lebendige Farben erstellt, die außerhalb der sRGB-Farbraum liegen. Einige der interessantesten Beispiele der Farben, die im sRGB dargestellt werden können Dinge Natur wie Sonnenuntergänge, Autumn lässt, außergewöhnlichen Blumen und tropischen vermeiden.

Benutzer, die Erfassen von digitalen Bildern im RAW-Format möglicherweise Images auf ihren Geräten, die alle diese Farbe Daten enthalten, obwohl es nicht ordnungsgemäß in den sRGB-Farbraum dargestellt werden, und deshalb nicht ordnungsgemäß auf dem Bildschirm angezeigt werden.

### <a name="the-display-p3-color-space"></a>Das Display P3-Farbraum

In 2015 veröffentlicht Apple neue Produkte (iMac und iPad Pro 9,7"), die den neuen Display P3-Farbraum zum Behandeln der Probleme, die von den sRGB-Farbraum erstellt bereitstellen.

[![](wide-color-images/color04.png "Die neue Display P3-Farbraum")](wide-color-images/color04.png#lightbox)

Das Display P3-Farbraum hat die folgenden Eigenschaften:

- Unterstützt eine Breite Farbskala Speicherplatz für moderne Hardwareplattformen.
- Basierend auf dem SMPTE DCI-P3-Standard. DCI-P3 digitale Projektoren konzipiert wurde, aber es wurde von Apple zur Unterstützung von Monitoren geändert.
- Er verfügt über eine ungefähre dessen Gamma 2.2.
- Es stellt die typische Lichtverhältnissen (D65) dar.

Gemäß den Apple verschieben Benutzer ihre Workflows für ihre mobilen Plattformen. Der sRGB in den professionellen mobilen Geräten (iPad Experten) erforderlich sind, einschließlich der mehr als nur eine Breite Farbskala anzeigen präsentierte Farbe Presentation-Probleme lösen können. Eine der Lösungen bestand darin, die Kalibrierung der Factory zu aktualisieren, sodass jedes Gerät im Einzelfall kalibriert wurde die Factory an, die verhindert, dass von einem Gerät zu Gerät Farbanzeige präzise und einheitlich sind.

Eine andere Lösung ist die Einbeziehung von vollständigen, systemweite farbverwaltung, die mit iOS 10 und MacOS Sierra Apple integriert wurde. 

### <a name="the-extended-range-srgb-color-space"></a>Der Extended Range sRGB-Farbraum

Die neue iOS 10 systemweite farbverwaltung muss für alle vorhandenen iOS-apps zu berücksichtigen, die erstellt und optimiert für sRGB. Sie wurde entwickelt, um sicherzustellen, dass es entweder Farbe-Darstellung oder app-Leistung, der diese vorhandenen apps auswirken, nicht.

Um diese Situation zu behandeln, hat Apple die Extended Range sRGB-Farbraum unter iOS 10 (und auch MacOS Sierra) enthalten.

Der Extended Range sRGB-Farbraum hat die folgenden Eigenschaften:

- Verfügt über alle den gleichen sRGB primären Replikaten.
- Er verfügt über eine ungefähre dessen Gamma 2.2.
- Es stellt die typische Lichtverhältnissen (D65) dar.
- Es unterstützt die negative Werte und Werte größer als eins (1).

Von kann für Werte, die kleiner als 0 (null) und ermöglicht und größer als eins, die Extended Range sRGB-Farbraum nicht nur für vorhandene apps vorhanden Farben im sRGB ohne Leistungseinbußen oder Verzerrung zu darzustellen, aber es können des Farbraums jede Farbe in den sichtbaren darstellen Bandbreite. All dies wird erreicht, und dennoch die Sicherheit der gleichen Ankerpunkte als den sRGB-Farbraum.

### <a name="extended-range-srgb-in-action"></a>Extended Range sRGB in Aktion

Um anzuzeigen, wie Werte außerhalb von 0 (null) und eine in den Extended Range sRGB-Farbraum arbeiten, nutzen Sie das folgende Beispiel der von der höchsten Sättigung entspricht Rot im Display P3-Farbraum:

[![](wide-color-images/color05.png "Funktionsweise von Werten außerhalb von 0 (null) und eine in den Extended Range sRGB-Farbraum")](wide-color-images/color05.png#lightbox)

Im Display P3, wäre dies die Farbe als dargestellt werden `[1.0, 0.0, 0.0]` und Extended Range sRGB es wäre `[1.358, -0.074, -0.012]`. Da sRGB-Werten gefüllt sind Erstellen des Layouts innerhalb Display P3 geschlossen und die Display P3-Werte "außerhalb" der sRGB-Bereiche.

Für die physische Hardware, der Pixelwerte von äußerst positiv zu extreme negative Werte wechseln kann, es kann eine beliebige Farbe, die innerhalb des sichtbaren Spektrums verfügbar anzeigen, und diese Werte in den Extended Range sRGB-Farbraum dargestellt werden können.

### <a name="device-pixel-formats"></a>Gerät-Pixelformate 

Der sRGB-Farbraum wurde hauptsächlich als Standard ein 8-Bit-Pixelformat verwenden, da es sich bei 8 Bits pro Farbkanal ist größtenteils genug, um die Farben im sRGB beschreiben. Dies ist nicht perfekt, aber gut genug, und sie erhalten einen guten Kompromiss zwischen Arbeitsspeicher und Prozessorzeit Verwendung zum Anzeigen von Bildern.

Da Display P3 Koordinaten der Farbe außerhalb des sRGB-Farbraums darstellen kann, muss 16 Bit pro Kanal Farbe, um Farben mit den Extended Range sRGB-Farbraum richtig darzustellen.

## <a name="system-wide-wide-color-support"></a>Unterstützung für eine systemweite Breite Farbskala

Um große Farbe und Breite Farbskala in iOS 10 und MacOS Sierra vollständig unterstützen zu können, hat Apple die folgenden Frameworks nutzen die Extended Range sRGB-Farbraums und Display P3 wird erweitert:

- UIKit (nur für iOS)
- SceneKit
- Wichtigste Grafik
- ImageIO
- Core-Image
- WebKit
- SpriteKit
- Core-Animation
- AppKit (für MacOS)

Darüber hinaus zeigt Retina-Display, die Unterstützung für Extended Range sRGB-Farbraum verbessert hat und Display P3.

Breite Farbskala wird unterstützt und kann in die folgenden Inhaltstypen der Anwendung verwendet werden:

- Statisches Bild-Ressourcen, die in der app-Bundles eingeschlossen.
- Dokument und netzwerkbasierten Bildressourcen.
- Erweiterte Medien wie z. B. Live Fotos oder Bilder in das RAW-Format.
- 3D Shader Textur Grafiken.

## <a name="solving-the-color-problem"></a>Zur Lösung des Problems Farbe

Der Inhalt in einer app angezeigt, kann aus einer Vielzahl von Farbe mit vielen Quellen stammen. Darüber hinaus kann diese Inhalte auf einer breiten Palette von Geräten, die jeweils über einen eigenen Adressbereich der Color-Anzeigefunktionen angezeigt werden.

Eine iOS 10-app verbindet den Unterschied zwischen diesen zwei Probleme mit der neuen integrierten, systemweite Farbe zu verwalten. Dieses System wird sichergestellt, dass ein Bild auf alle iOS-Gerät sieht gleich aus unabhängig davon, welche Farbraum das Bild in codiert wurde.

Farbverwaltung beginnt mit dem jedes Bild, das mit einer zugeordneten-Farbraum (oder Farbprofil). Diese Informationen werden verwendet, der _Farbe übereinstimmenden Prozess_ , bei denen werden Farben im Quellbild übereinstimmt, für die Farben in das Ausgabegerät. Da jedes Pixel in der Abbildung Farbe zugeordnet sein muss, kann zeitaufwändig sein, und platzieren Sie einen Hinweis auf die Belastung auf das Gerät die CPU.

Aufgrund der Art der der _Farbe übereinstimmenden Prozess_, diese Konvertierung kann potenziell "verlustbehaftete" angezeigt, wenn das Ausgabegerät eine kleinere Skala als dem Quellimage verfügt sein.

Glücklicherweise die Berechnungen, die näher betrachten die _Farbe übereinstimmenden Prozess_ kann problemlos Hardware beschleunigt werden (entweder auf der CPU oder GPU) und Apple wird sichergestellt, dass dies automatisch erfolgt durch das Integrieren in Basis-Systemen wie z. B. Quartz 2D, ColorSync und Core-Animation. Inhalte, ordnungsgemäß mit Tags ist keine Codierung erforderlich, um diese Funktionen nutzen.

Farbverwaltung wurde wie folgt auf den einzelnen Plattformen unterstützt:

- **MacOS** -MacOS wurde eine Farbe, die seit Einführung verwaltet.
- **iOS** -iOS unterstützt automatische farbverwaltung seit iOS 9.3 (oder höher).

### <a name="designing-for-wide-gamut"></a>Entwerfen für Breite Farbskala

Apple hat die folgenden Vorschläge für das Entwerfen und mit breiten, Farbe, Breite Farbskala Bildinhalt in iOS und MacOS-apps:

- Nur Vielzahl Inhalte verwenden, an welcher Stelle in den Stellen in der app zu ermitteln, sie sollten nicht automatisch überall verwendet werden.
- Verwenden Sie nur Vielzahl Inhalt lebendige Farben werden, in denen die benutzerfreundlichkeit verbessern.
- In ist nicht erforderlich, um alle Inhalte in P3 für vorhandene apps zu ändern.

Apple toolkette ist die Vielzahl Bildinhalt ein schrittweise opt-in, Unterstützung für, so unterstützen in einer app Breite Farbskala nichts Situation nicht ist.

### <a name="upgrading-existing-content-to-wide-color"></a>Aktualisieren vorhandene Inhalt auf Breite Farbskala

Apple hat die folgenden Vorschläge zum Aktualisieren von vorhandenen Inhalt auf Breite Farbskala:

- Weisen Sie nicht nur zu"" ein Profil P3 auf den Inhalt in der Abbildung app bearbeiten. Auf diese Weise wird einfach die vorhandene Farbe Inhalt in den neuen Farbraum mit unerwartete Ergebnisse, wie z. B. die Farben, die in den neuen Bereich so ändern das Bild passt Strecken zuzuordnen.
- Den Inhalt des müssen "klicken Sie auf das Display P3-Profil, das mithilfe eines Images bearbeiten die app konvertiert werden".

### <a name="file-formats-and-color-profiles"></a>Dateiformate und die Farbprofile

Apple hat die folgenden Vorschläge für die Dateiformate und die Farbprofile, die in der app Breite Farbskala Bildinhalt verwendet:

- Verwenden Sie das Farbprofil "Display P3" bei Speicherplätzen mit RGB-arbeiten.
- Verwenden Sie eine 16-Bit pro Kanal Farbmodus.
- Verwenden Sie einen iMac Late 2015 (oder höher) um genau den Inhalt der Vorschau anzuzeigen.
- Exportieren Sie Bildanlagen als 16-Bit-PNG-Dateien mit einem eingebetteten "Display P3" Chipkarte-Profil ein.

> [!IMPORTANT]
> Mithilfe der **für das Web speichern** oder **Objekte exportieren** Features finden Sie in der am häufigsten verwendeten Bildbearbeitungssoftware _nicht_ für Breite Farbskala Bilder, da diese Funktionen nicht wurden aktualisiert, um die erforderliche Datei Formatangaben noch unterstützt.

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

## <a name="colors-in-user-interfaces"></a>Farben in Benutzeroberflächen

Bei der Arbeit mit den Farben in einer Benutzeroberfläche sind die meisten der Pixel auf dem Bildschirm in einer Volltonfarbe. Darüber hinaus wird die meisten dieser Pixel nicht stammen aus Images Bestand jedoch stammen direkt von der app (oder durch das Betriebssystem im Auftrag einer app).

Farbe der Vielzahl kann mehrere zu Herausforderungen bei der Arbeit auf der UI-Ebene:

- **Kommunikation von Farben** – bei der Kommunikation über Farbe zwischen Designern und Entwicklern, die in der Regel besteht eine _davon ausgegangen, dass_ sRGB-Farbraum beteiligt. Damit eine Farbe als übermittelt werden kann `rgb(128, 45, 56)` oder `#FF0456`. In einem Entwurf Vielzahl diesen Darstellungen bieten keine genügend Informationen, um genau die angegebene Farbe darstellt, Farbraum arbeiten, muss auch eingeschlossen werden. Apple empfiehlt mit `P3(128, 45, 56)` und `P3#FF0456` stattdessen. 
- **Auswählen von Farben** – die meisten der gängigen Bildformat zu bearbeiten und entwerfen, die Software auf dieselben Einschränkungen wie bei den sRGB-Farbraum leiden, wenn Sie ihre integrierte Farbwähler verwenden. Der Designer sorgen dafür, dass sie im Bereich "Display P3" Farbe in der Farbauswahl sind, bei der Arbeit mit Breite Farbskala Entwürfe.
- **Codierung von Farben** – beide `NSColor` (MacOS) und `UIColor` (iOS und TvOS) haben Sie neue Hilfsmethoden zum Generieren von P3-Farben direkt und haben wurden erweitert, um Farben in den Extended Range sRGB-Farbraum ebenfalls unterstützen.
- **Speichern von Farben** -Sorgfalt geboten, wenn das Speichern von Breite Farbskala-, in der app-Dokument Farben. Bei 8-Bit-pro-Farbkanal funktionierte gut für sRGB-Farbraum, sollte die 16-Bit-pro-Farbkanal für große Farben verwendet werden. Der Entwickler muss außerdem auf Instanzen von angenommenen-Farbraum (da alles, was normalerweise nur sRGB war).

## <a name="colors-on-the-web"></a>Farben im Web

Bei der Arbeit mit Breite Farbskala in Webseiten und auf Geräten, die die Breite Farbskala anzeigen zu unterstützen, sollte vorsichtig vorgenommen werden. Wenn alle den Bildinhalt, der auf der Website einbezogen wurde entsprechend markiert ist, iOS und MacOS automatisch color-Übereinstimmung und korrekt angezeigt wird.

Medienabfragen sind ebenfalls verfügbar, um Ressourcen in beiden P3 und sRGB-fähige Geräte zu lösen:

```xml
<picture>
    <source srcset="monkey-p3.jpg" media="(color-gamut: p3)">
    <source srcset="monkey-rpg.jpg">
</picture>
```

Apple hat auch ein WebKit-Angebot, mit dem CSS in anderen Farbräume neben dem Bereich der angenommenen sRGB angegeben werden kann.

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
- SRGB
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

## <a name="summary"></a>Zusammenfassung

Breite Farbskala und, wie sie implementiert und innerhalb einer Xamarin.iOS oder Xamarin.Mac-app verwendet werden kann, wurden in diesem Artikel behandelt.



## <a name="related-links"></a>Verwandte Links

- [iOS 10-Beispiele](https://developer.xamarin.com/samples/ios/iOS10/)
