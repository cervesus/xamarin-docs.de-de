---
title: Breite Farbskala in Xamarin.iOS
description: Dieses Dokument beschreibt die Breite Farbskala und wie es in einem Xamarin.iOS oder Xamarin.Mac verwendet werden kann. Darüber hinaus einen allgemeinen Überblick über viele wichtige farbbezogene Konzepte wie Farbräumen, Kanäle und Grundfarben.
ms.prod: xamarin
ms.assetid: 576E978A-F182-489A-83E4-D8CDC6890B24
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 173919e0d5feda6ab7d34895cc834c5f36d737a8
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34788780"
---
# <a name="wide-color-in-xamarinios"></a>Breite Farbskala in Xamarin.iOS

_Dieser Artikel behandelt die Breite Farbskala und wie es in einem Xamarin.iOS oder Xamarin.Mac verwendet werden kann._

iOS 10 und MacOS Sierra verbessert die Unterstützung für erweiterte Schlüsselbereich Pixelformate und Breite Farbskala Farbräumen im gesamten System einschließlich Frameworks wie z. B. Grafiken Core und Core-Image, Metall AVFoundation. Unterstützung für Geräte mit Breite Farbskala zeigt wird weiter Dienstverweise durch dieses Verhalten im ganzen Grafikstapel gesamte bereitstellen.

## <a name="about-wide-color"></a>Informationen zu Breite Farbskala

Wie bereits erwähnt, verbessert iOS 10 und MacOS Sierra die Unterstützung für erweiterte Schlüsselbereich Pixelformate und Breite Farbskala Farbräumen im gesamten System, darunter Frameworks wie z. B. Grafiken Core und Core-Image, Metall AVFoundation. Unterstützung für Geräte mit Breite Farbskala zeigt wird weiter Dienstverweise durch dieses Verhalten im ganzen Grafikstapel gesamte bereitstellen.

In den 90er Jahre erstellt Apple ColorSync um Farbe, die auf dem Mac Verarbeitung zu behandeln. Sie haben auch International Farbe Consortium (ICC) zum Erstellen und einen Satz von Standards für die Behandlung der Farbe auf Computerhardware heraufstufen gefunden. Apple arbeiten mit ICC wurde im ColorSync eingefügt, und es in den Kern der OS X (jetzt MacOS genannt) erstellt wurde.

Apple wurde auch bei der Anzeige Technologie mit Hardware wie z. B. die Retina anzuzeigen, die neue P3 anzuzeigen und die anzuzeigen P3 Farbe-Speicherplatz (in der iMac 2015 veröffentlicht) und die TrueTone im Experten iPad, iPhone 7 und iPhone 7 zeigt Plus.

Mit Breite Farbskala in MacOS Sierra und iOS 10 ändert Apple die Möglichkeit, MacOS und iOS Farbe, um diese neue Anzeige Technologien voll ausnützen behandeln.

## <a name="core-color-concepts"></a>Farbe Kernkonzepte

Die folgenden Farbe Kernkonzepte benötigen abgedeckt werden, bevor Sie einen tieferen Einblick in Farbe im MacOS und iOS:

### <a name="color-space"></a>Farbraum

Ein Farbraum ist eine Umgebung, in der Farben dargestellt und verglichen werden können. Es kann eine bis zu vier-dimensionalen Raum sein, die durch die Intensität der Farbe Komponenten definiert ist. 

[![](wide-color-images/color00.png "Ein Farbraum")](wide-color-images/color00.png#lightbox)

### <a name="color-channels"></a>Farbkanälen

Die Farbkomponenten können auch als Farbkanälen bezeichnet werden. Einige bekannten Darstellungen wäre die RGB-Leerzeichen, den grau Leerzeichen, die CMYK-Leerzeichen oder Gerät unabhängig Leerzeichen. 

[![](wide-color-images/color02.png "Die Farbkomponenten können auch als Farbkanälen bezeichnet werden")](wide-color-images/color02.png#lightbox)

### <a name="color-primaries"></a>Farbe Grundfarben

Farbe Grundfarben bieten das Koordinatensystem, das verwendet wird, vergleichen und Farben zu berechnen. Farbe Grundfarben fallen in der Regel das am häufigsten ressourcenintensive Version der angegebenen Farbe, die innerhalb des Kanals Farbe generiert werden können.

[![](wide-color-images/color01.png "Farbe Grundfarben Geben Sie das Koordinatensystem, das verwendet wird, vergleichen und Berechnen von Farben")](wide-color-images/color01.png#lightbox)

Im Fall der RGB-Farbspektrum oben dargestellt, sind die primäre Farbe Where der `1.0` Koordinaten verankert sind (z. B. `[1.0, 0.0, 0.0]` für Rot).

### <a name="color-gamut"></a>Farbskala

Farbskala bezieht sich auf alle Farben, die als eine Kombination aus den einzelnen Farbkanälen innerhalb einer angegebenen Farbraum definiert werden können.

[![](wide-color-images/color03.png "Farbe Farbskala-Beispiel")](wide-color-images/color03.png#lightbox)

## <a name="what-is-wide-color"></a>Was ist eine Breite Farbskala

Bevor auf das Thema Breite Farbskala, sollte eine Erläuterung der aktuellen Industriestandard für Farbe, die standardmäßige RGB-Farbspektrum (sRGB) hatte werden. Es ist die am häufigsten verwendeten Farbraum berechnen von heute und den Standard-Farbraum für IOS- und Mac OS.

Die sRGB Farbraum hat die folgenden Eigenschaften:

- Es basiert auf dem BT.709 ITU-R-Standard.
- Er verfügt über eine ungefähre Gammawert 2.2.
- Typische Beleuchtung Bedingungen (D65) dar.

Da sRGB wird häufig verwendet, in der Branche, ein Entwickler bestimmte Annahmen Kriterien kann wird angegebene Farbe nutzungsvereinbarungen auf jedem Gerät dargestellt werden, die auf dem er angezeigt wird. Kann dies jedoch nicht immer der Fall sein. Darüber hinaus sind mehrere Farben, die nicht in den sRGB Farbraum passen und nicht Verlaufsebene, darin dargestellt werden.

Beispielsweise sind viele Textilien mit Threads verwenden viele Farben und Farbstoffe außerhalb sRGB fallen vorgesehen. Außerdem werden viele Produkte, die eine Person in ihrer täglichen Leben vorfindet mit hell, lebendige Farben erstellt, die außerhalb der sRGB Farbraum liegen. Einige der wichtigsten Beispiele der Farben, die im sRGB dargestellt werden können Dinge wie Sonnenuntergänge, Herbst bewirkt, dass, exotische Blumen und tropische Wasser Natur.

Benutzer, die Erfassen von digitalen Bilder im RAW-Format möglicherweise Bilder auf ihren Geräten, die dieser Farbe Daten enthalten, obwohl er nicht ordnungsgemäß in den sRGB Farbraum dargestellt werden kann und Verlaufsebene kann nicht ordnungsgemäß auf Bildschirm angezeigt werden.

### <a name="the-display-p3-color-space"></a>Die Anzeige P3-Farbraum

Im 2015 veröffentlicht Apple neue Produkte (iMac und iPad Pro 9.7"), die den neuen Anzeige P3-Farbraum um erstellt, indem die sRGB Farbraum Probleme behandeln bereitstellen.

[![](wide-color-images/color04.png "Die neue Anzeige P3-Farbraum")](wide-color-images/color04.png#lightbox)

Die Anzeige P3-Farbraum hat die folgenden Eigenschaften:

- Unterstützt einen Breite Farbraum für moderne Hardwareplattformen.
- Basierend auf dem SMPTE DCI-P3-Standard. DCI P3 digitale Projektoren konzipiert wurde jedoch von Apple zur Unterstützung von Monitoren geändert wurde.
- Er verfügt über eine ungefähre Gammawert 2.2.
- Typische Beleuchtung Bedingungen (D65) dar.

Gemäß den Apple verschieben Benutzer ihre Workflows für ihre mobilen Plattformen. Lösen die Farbe Präsentation Probleme vorgelegte sRGB in der professional mobile Geräte (iPad IT-Spezialisten) erforderlich sind, z. B. mehr als nur eine Breite Farbskala anzeigen. Eine der Lösungen wurde die Factory-Kalibrierung zu aktualisieren, sodass jedes Gerät im Einzelfall kalibriert wurde an die Factory sichergestellt wird, dass vom Gerät, richtige und konsistente Farbwiedergabe unterstützt wird.

Eine andere Lösung ist die Angabe des vollständigen, systemweite Farbe-Verwaltung, die Apple iOS 10 und MacOS Sierra erstellt wurde. 

### <a name="the-extended-range-srgb-color-space"></a>Der erweiterte Bereich sRGB Farbraum

Die neue iOS 10 systemweite Color Management muss alle vorhandenen iOS-apps berücksichtigen, die erstellt und für sRGB abgestimmt sind. Es wurde entwickelt, um sicherzustellen, dass er entweder Farbe Darstellung oder app diese vorhandenen Apps Leistungseinbußen haben nicht.

Um diese Situation zu behandeln, ist Apple der erweiterte Bereich sRGB Farbraum in iOS 10 (und MacOS Sierra sowie) enthalten.

Der erweiterte Bereich sRGB Farbraum hat die folgenden Eigenschaften:

- Verfügt über alle den gleichen sRGB Grundfarben.
- Er verfügt über eine ungefähre Gammawert 2.2.
- Typische Beleuchtung Bedingungen (D65) dar.
- Es unterstützt die negative Werte und Werte größer als eins (1).

Durch ermöglicht lässt für Werte, die kleiner als 0 (null) und größer als 1, der erweiterte Bereich sRGB, den Farbraum nicht nur für vorhandene apps vorhanden Farben in sRGB ohne Leistungseinbußen oder Verzerrung, sondern ermöglicht, den Farbraum zum Darstellen einer beliebigen Farbe innerhalb der sichtbar Spektrum. All dies erfolgt gleichzeitig die gleichen Ankerpunkte als den sRGB Farbraum.

### <a name="extended-range-srgb-in-action"></a>Erweiterte Bereich sRGB in Aktion

Um anzuzeigen, wie Werte außerhalb von 0 (null) und eine in den erweiterten Bereich sRGB Farbraum funktionieren, nehmen Sie im folgende Beispiel die von den meisten wiederum überlastete roten in den Farbraum der anzeigen-P3:

[![](wide-color-images/color05.png "Funktionsweise von Werten außerhalb 0 (null) und eine in den erweiterten Bereich sRGB Farbraum")](wide-color-images/color05.png#lightbox)

Im Anzeigebereich P3, würde dieser Farbe dargestellt werden als `[1.0, 0.0, 0.0]` und im erweiterten Bereich sRGB es wäre `[1.358, -0.074, -0.012]`. Da sRGB Werten gefüllt sind werden in der Anzeige P3 enthaltenen und die Anzeige P3-Werte "außerhalb" der Bereiche sRGB anordnen.

Für physische Hardware, der Pixelwerte von äußerst positiv zu negativen Extremwerte wechseln kann, es kann eine beliebige Farbe verfügbar innerhalb des sichtbaren Spektrums angezeigt, und diese Werte in den erweiterten Bereich sRGB Farbraum dargestellt werden können.

### <a name="device-pixel-formats"></a>Gerät Pixelformate 

Die sRGB Farbraum größtenteils wurde auf eine 8-Bit-Pixelformat verwenden, da 8 Bits pro Kanal Farbe ist größtenteils standardisieren genug, um Farben im sRGB beschreiben. Dies ist nicht perfekt jedoch ausreichend, und sie erhalten einen guten Kompromiss zwischen Arbeitsspeicher und Prozessor Verbrauch Anzeige von Bildern.

Da Anzeige P3 Farbe Koordinaten außerhalb der Farbraum sRGB darstellen kann, muss 16 Bits pro Kanal Farbe, um Farben mit den erweiterten Bereich sRGB Farbraum richtig darzustellen.

## <a name="system-wide-wide-color-support"></a>Systemweite Breite Farbskala-Unterstützung

Apple erweitert um wide Farbe und Breite Farbskala in iOS 10 und MacOS Sierra vollständig unterstützen zu können, um vollständige der erweiterte Bereich sRGB Farbraum und Anzeige P3 nutzen die folgenden Frameworks:

- UIKit (nur für iOS)
- SceneKit
- Core-Grafiken
- ImageIO
- Core-Image
- WebKit
- SpriteKit
- Core-Animation
- AppKit (für MacOS)

Darüber hinaus zeigt Retina-Display, die Unterstützung für erweiterte Bereich sRGB Farbraum erweitert wurde und die Anzeige P3.

Breite Farbskala wird unterstützt und kann in die folgenden Inhaltstypen der Anwendung verwendet werden:

- Statisches Bildressourcen in app-Bundles eingeschlossen.
- Dokument und netzwerkbasierten Bildressourcen.
- Erweiterte Medien wie Fotos Live oder Bilder im RAW-Format.
- 3D Shader Textur Grafiken.

## <a name="solving-the-color-problem"></a>Zur Lösung des Problems Farbe

In einer app angezeigte Inhalt kann aus einer Vielzahl von Farbe Rich-Quellen stammen. Darüber hinaus kann diese Inhalte auf einer breiten Auswahl an Geräten, jeweils über eigene Wertebereich Anzeigefunktionen Farbe angezeigt werden.

Eine app für iOS 10 verbindet den Unterschied zwischen diesen zwei Probleme mit der neue integrierte, systemweite Farbe verwalten. Dieses System wird sichergestellt, dass ein Bild gleich auf alle iOS-Gerät dargestellt unabhängig davon, welche Farbraum das Bild in codiert wurde.

Farbe Verwaltung beginnt mit jedes Bild mit einem zugeordneten Farbraum (oder einem Farbprofil). Diese Informationen werden verwendet, der _Farbe übereinstimmenden Prozess_ , in denen Farben im Quellbild Farben in das Ausgabegerät zugeordnet sind. Da jedes einzelnen Pixels im Bild Farbe zugeordnet sein muss, kann zeitaufwändig sein, und platzieren Sie ein Stamm auf dem Gerät CPU.

Aufgrund der Natur der der _Farbe übereinstimmenden Prozess_, diese Konvertierung kann potenziell "lossy", verfügt das Ausgabegerät eine kleinere Skala als Quellbilds sein.

Glücklicherweise, die Berechnungen, die in der _Farbe übereinstimmenden Prozess_ kann problemlos Hardware beschleunigt werden (entweder auf der CPU oder GPU) und Apple wird sichergestellt, dass es automatisch funktioniert, indem Unterstützung in Basis Systemen wie z. B. Quartz 2D, erstellen ColorSync und Core-Animation. Für ordnungsgemäß markierten Inhalt ist keine Codierung erforderlich, um diese Funktionen nutzen.

Color Management wurde wie folgt auf den verschiedenen Plattformen unterstützt:

- **MacOS** -MacOS wurde Farbe Cacheobjekts seit Beginn verwaltet.
- **iOS** -iOS automatische Färbung Management seit iOS 9.3 (und höher) unterstützt.

### <a name="designing-for-wide-gamut"></a>Entwerfen für Breite Farbskala

Apple hat die folgenden Vorschläge für das Entwerfen und mithilfe der Breite, Farbe, Breite Farbskala Bildinhalt in apps für IOS- und Mac OS:

- Verwenden Sie Breite Farbskala Inhalt nur in stellen Sinn, in der app und sollten nicht automatisch verwendet werden überall.
- Nur verwenden Sie Breite Farbskala Inhalt, in denen werden lebendige Farben Verbesserung der benutzerfreundlichkeit.
- In ist nicht erforderlich, um alle Inhalte in P3 für vorhandene apps zu ändern.

Apple toolkette macht so unterstützen Breite Farbskala in eine app naheliegend Situation nicht ist für Breite Farbskala Bildinhalt eine allmähliche teilnehmen können, unterstützen.

### <a name="upgrading-existing-content-to-wide-color"></a>Aktualisieren von vorhandenen Inhalt auf Breite Farbskala

Apple hat die folgenden Vorschläge zum Aktualisieren von vorhandenen Bildinhalt auf Breite Farbskala:

- Weisen Sie nicht nur zu"" ein P3-Profil auf den Inhalt in der Abbildung app bearbeiten. Auf diese Weise wird der vorhandene Inhalt auf den neuen Farbraum mit unerwarteten Ergebnissen führen, z. B. die Farben in der neue Speicherplatz daher ändern das Bild passt Strecken Farbe einfach Analysepunkte.
- Der Inhalt des Betriebssystemabbilds müssen "klicken Sie in der Anzeige P3-Profil unter Verwendung eines Bildes bearbeiten app konvertiert werden".

### <a name="file-formats-and-color-profiles"></a>Dateiformate und die Farbe Profile

Apple hat die folgenden Vorschläge für die Dateiformate und die Farbe-Profile in der app Breite Farbskala Bildinhalt verwendet:

- Verwenden Sie das Farbprofil "Anzeige P3" für RGB-arbeiten Leerzeichen ein.
- Verwenden Sie eine 16-Bit pro Färbung Kanal aktiviert.
- Verwenden Sie eine späte 2015 iMac (oder höher) genau Bildinhalt Vorschau anzuzeigen.
- Exportieren Sie Bildanlagen als 16-Bit-PNG-Dateien mit eingebetteten "Anzeige P3" ICC-Profil ein.

> [!IMPORTANT]
> Mithilfe der **für das Web speichern** oder **exportieren Bestand** Funktionen finden Sie in der am häufigsten verwendeten Bildbearbeitungssoftware _nicht_ für Breite Farbskala Bilder, da diese Funktionen nicht wurden aktualisiert die erforderliche Datei Formatangaben noch unterstützt.

### <a name="supporting-wide-color-with-asset-catalogs"></a>Unterstützung der Breite Farbskala mit Asset-Katalogen

Apple hat in iOS 10 und MacOS Sierra zum einschließen und kategorisieren statisches Bildinhalt in der app-Bündel Asset Kataloge Breite Farbskala Unterstützung erweitert.

Mit Asset Kataloge bieten die folgenden Vorteile auf eine app:

- Sie bieten die beste Bereitstellungsoption für statisches Bild Bestand.
- Sie unterstützen die automatische Färbung Korrektur.
- Sie unterstützen die automatische Pixel Format Optimierung.
- Sie unterstützen App Aufteilen in Slices und App Ausdünnung, der sicherstellt, dass nur der Inhalt, der relevanten Get an das Gerät übermittelt werden.

Apple hat die folgenden Verbesserungen Asset-Katalog für Breite Farbskala-Unterstützung vorgenommen:

- Sie unterstützen Quellinhalt von 16-Bit (pro Kanal).
- Sie unterstützen Katalogisierung Inhalt, indem der Farbskala anzeigen. Inhalte kann für die sRGB oder die Anzeige P3 Farbumfänge gekennzeichnet werden.

Der Entwickler hat drei Optionen für die Unterstützung der Breite Farbskala Inhalt in ihren apps:

1. **Nichts Unternehmen** -seit Breite Farbskala Inhalt nur in Situationen verwendet werden soll, in denen die app muss lebendige Farben anzeigen (wobei sie werden erleichtern des Benutzers), sollte auch außerhalb dieser Anforderung Inhalte als Links-ist. Es werden weiterhin in allen Situationen von Hardware korrekt gerendert werden.
2. **Aktualisieren von vorhandenen Inhalt auf Anzeige P3** -dies erfordert, dass den Entwickler, ersetzen den vorhandenen Inhalt des Betriebssystemabbilds in ihrer Asset-Katalog mit einer neuen, aktualisierten Datei im Format P3 und markieren es als solches im Ressourcen-Editor. Zur Buildzeit wird eine Ableitung sRGB-Image aus diesen Objekten generiert werden.
3. **Bereitzustellen Anlage Content optimiert** – In diesem Fall wird der Entwickler eine 8-Bit-sRGB und einer 16-Bit-Anzeige P3 Vision jedes Bilds in den Asset-Katalog (im Ressourcen-Editor ordnungsgemäß gekennzeichnet) bereitstellen.

### <a name="asset-catalog-deployment"></a>Asset-Katalog-Bereitstellung

Folgendes geschieht, wenn der Entwickler eine app im App Store mit Asset Katalogen übermittelt, die Breite Farbskala Bildinhalt enthalten:

- Wenn für den Endbenutzer die app bereitgestellt wird, wird App Aufteilen in Slices stellen Sie sicher, dass nur die entsprechenden Content Variante an das Gerät des Benutzers übermittelt wird.
- Für Geräte, die Breite Farbskala nicht unterstützen, wird es keine Kosten für die Nutzlast für Breite Farbskala Inhalt, einschließlich, da er nie auf dem Gerät geliefert wird.
- `NSImage` auf MacOS Sierra (und höher) wählt automatisch die optimale Darstellung des Inhalte für die Anzeige der Hardware.
- Der angezeigte Inhalt wird automatisch aktualisiert werden, oder wenn die Hardware der Geräte Merkmale Änderung anzuzeigen.

### <a name="asset-catalog-storage"></a>Asset-Katalog-Speicher

Asset-Katalog Speicher verfügt über die folgenden Eigenschaften und die Implikationen für eine app:

- Apple versucht, zur Buildzeit die Speicherung des Inhalts Bild über qualitativ hochwertigen Image Konvertierungen zu optimieren.
- für Breite Farbskala Inhalt werden 16 Bits pro Kanal verwendet.
- Abhängige paketabbild Komprimierung dient zum Lieferumfang Inhaltsgrößen senken. Neue "lossy" Kompressionen wurden hinzugefügt, um Inhaltsgrößen weiter zu optimieren.

## <a name="colors-in-user-interfaces"></a>Farben in Benutzeroberflächen

Bei der Arbeit mit der Farben in einer Benutzeroberfläche sind die meisten der Pixel auf dem Bildschirm in einer Volltonfarbe aus. Darüber hinaus die meisten dieser Pixel nicht vom Bildern Bestand stammen jedoch direkt von der app (oder vom Betriebssystem für die app) gezeichnet werden.

Breite Farbskala Farbe kann mehrere zu Herausforderungen bei der Arbeit auf der UI-Ebene:

- **Kommunikation von Farben** – bei der Kommunikation über Farbe zwischen Designer und Entwickler normalerweise besteht eine _davon ausgegangen, dass_ sRGB Farbraum beteiligt. Damit eine Farbe als mitgeteilt werden möglicherweise `rgb(128, 45, 56)` oder `#FF0456`. In eine Breite Farbskala Entwurf diesen Darstellungen nicht genügend Informationen bereitstellt zu einwandfrei an die angegebene Farbe, die Arbeit Farbraum auch eingeschlossen werden muss. Apple empfiehlt die Verwendung `P3(128, 45, 56)` und `P3#FF0456` stattdessen. 
- **Kommissionieren Farben** – die meisten der beliebten Bild zu bearbeiten und entwerfen, die Software von dieselben Einschränkungen wie bei den sRGB Farbraum leiden, wenn er mit ihrer integrierten Farbwähler. Der Designer sollten sicherstellen, dass sie in den Farbraum "Anzeige P3" in die Farbauswahl beim Arbeiten mit Breite Farbskala Designs.
- **Codierung von Farben** – beide `NSColor` (MacOS) und `UIColor` (iOS & tvos. außerdem wurden) haben neue Hilfsmethoden zum Generieren von P3 Farben direkt und beide wurde zur Unterstützung von Farben in den erweiterten Bereich sRGB Farbraum ebenfalls erweitert.
- **Speichern von Farben** -sollte darauf geachtet werden, wenn das Speichern von Breite Farbskala in eine app Dokument Farben. Bei 8-Bit pro Kanal für den Farbraum sRGB funktionierten, sollten 16 Bit pro Kanal für wide Farben verwendet werden. Außerdem muss der Entwickler der zu überwachenden Instanzen von angenommenen Farbraum (da alles, was normalerweise nur sRGB wurde).

## <a name="colors-on-the-web"></a>Farben im Web

Bei der Arbeit mit Breite Farbskala in Webseiten und auf Geräten, die Breite Farbskala Anzeige unterstützen, sollte vorsichtig vorgenommen werden. Wenn der gesamte Bildinhalt, der auf der Website eingeschlossen wurde entsprechend markiert wurde, IOS- und Mac OS automatisch Farbe Übereinstimmung und ordnungsgemäß angezeigt werden können.

Außerdem stehen Medienabfragen Lösung bestand zwischen P3 und sRGB-fähige Geräte zur Verfügung:

```xml
<picture>
    <source srcset="monkey-p3.jpg" media="(color-gamut: p3)">
    <source srcset="monkey-rpg.jpg">
</picture>
```

Apple hat auch ein WebKit-Angebot, mit dem CSS in anderen Farbräumen neben der angenommenen sRGB Speicherplatz angegeben werden kann.

## <a name="rendering-off-screen-images-in-app"></a>Rendern von außerhalb des Bildschirmbereichs befindet Bilder in-App

Basierend auf den Typ der app erstellt wird, könnte dies den Benutzer Bildinhalt enthalten, diese über das Internet erfasst haben oder Erstellen von Bildinhalt direkter Verweis innerhalb der app (z. B. einen Vektor, zeichnen die app z. B.) ermöglichen.

In beiden Fällen kann die app erforderlichen Bilder in Breite Farbskala Verwenden erweiterter Features hinzugefügt, die für IOS- und Mac OS außerhalb des Bildschirms gerendert werden.

### <a name="drawing-wide-color-in-ios"></a>Zeichnen Breite Farbskala in iOS

Erläutert, wie eine Breite Farbskala Bild in iOS 10 korrekt gezeichnet werden soll, sehen Sie sich die folgenden allgemeinen iOS-Zeichencode:

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

Es gibt Probleme mit dem Standardcode, die behoben werden müssen _vor_ können verwendet werden, eine Breite Farbskala Bild gezeichnet werden soll. Die `UIGraphics.BeginImageContext (size)` Methode zum Starten von iOS-Image-Zeichnung gelten folgende Einschränkungen:

- Es kann nicht mit mehr als 8 Bits pro Kanal Farbe Image Kontexten erstellt.
- Es kann keine Farben in den erweiterten Bereich sRGB Farbraum dar.
- Er verfügt nicht über die Möglichkeit, eine Schnittstelle zum Erstellen von Kontexten in einer nicht-sRGB-Farbraum aufgrund der im Hintergrund aufgerufen werden C-Routine auf niedriger Ebene bereitzustellen.

Um diese Einschränkungen zu behandeln, und zeichnen eine Breite Farbskala Image in iOS-10, verwenden Sie stattdessen den folgenden Code:

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

Die neue `UIGraphicsImageRenderer` Klasse erstellt einen neuen Image-Kontext, der den erweiterten Bereich sRGB Farbraum verarbeiten kann, und es hat die folgenden Funktionen:

- Es ist vollständig Farbe standardmäßig verwaltet.
- Der erweiterte Bereich sRGB Farbraum standardmäßig unterstützt.
- Er entscheidet sich intelligent auf, wenn in der sRGB oder der erweiterte Bereich sRGB Farbraum basierend auf den Funktionen des iOS-Geräts, das die app ausgeführt wird, auf gerendert werden soll.
- Er vollständig und automatisch verwaltet wird das Image-Kontext (`CGContext`) Lebensdauer, sodass der Entwickler nicht kümmern aufrufen beginnen und enden Kontextbefehlen.
- Es ist kompatibel mit der `UIGraphics.GetCurrentContext()` Methode.

Die `CreateImage` Methode der `UIGraphicsImageRenderer` Klasse aufgerufen, um eine Breite Farbskala-Image zu erstellen und übergeben Sie einen Abschlusshandler mit dem Image-Kontext, in dem gezeichnet werden soll. ist. Alle der Zeichnung erfolgt innerhalb dieser Abschlusshandler wie folgt:

- Die `UIColor.FromDisplayP3` -Methode erstellt eine neue vollständig wiederum überlastete rote Farbe in der Breite Farbskala anzeigen P3-Farbraum und es wird verwendet, um die erste Hälfte des Rechtecks zeichnen. 
- Die zweite Hälfte des Rechtecks gezeichnet wird in den normalen sRGB vollständig ausgelastet rote Farbe für den Vergleich.

### <a name="drawing-wide-color-in-macos"></a>Zeichnen Breite Farbskala in macOS

Die `NSImage` Klasse wurde in MacOS Sierra zur Unterstützung des Zeichnens des Breite Farbskala Bilder erweitert. Zum Beispiel:

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

## <a name="rendering-on-screen-images-in-app"></a>Rendern auf dem Bildschirm in App-Images

Um eine Breite Farbskala Bilder auf dem Bildschirm gerendert wird, verläuft der Prozess ähnelt, Zeichnen eines Bilds außerhalb des Bildschirmbereichs befindet Breite Farbskala für MacOS und iOS, die oben aufgeführten.

### <a name="rendering-on-screen-in-ios"></a>Rendern von auf dem Bildschirm in iOS

Überschreiben, wenn die app ein Bild in Breite Farbskala in iOS auf dem Bildschirm gerendert werden soll muss, die `Draw` Methode der `UIView` fraglichen wie gewohnt. Zum Beispiel:

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

Wie iOS 10 mit der `UIGraphicsImageRenderer` Klasse, die oben dargestellte Intelligent entscheidet, wenn in der sRGB oder der erweiterte Bereich sRGB Farbraum basierend auf den Funktionen des iOS-Geräts, das dazu, wann die app ausgeführt wird gerendert werden soll die `Draw` -Methode aufgerufen wird. Darüber hinaus die `UIImageView` wurde seit iOS 9.3 verwaltet.

Wenn die app muss wissen, wie Rendering auf erfolgt eine `UIView` oder `UIViewController`, können Sie die neue überprüfen `DisplayGamut` Eigenschaft von der `UITraitCollection` Klasse. Dieser Wert eine `UIDisplayGamut` Enum Folgendes:

- P3
- SRGB
- Nicht angegeben.

Wenn die app steuern die Farbraum verwendet wird möchte, ein Bild gezeichnet werden soll, kann ein neues verwenden `ContentsFormat` Eigenschaft von der `CALayer` an den gewünschten Farbraum. Dieser Wert kann eine `CAContentsFormat` Enum Folgendes:

- Gray8Uint
- Rgba16Float
- Rgba8Uint

### <a name="rendering-on-screen-in-macos"></a>Auf dem Bildschirm Rendern in macOS

Überschreiben, wenn die app ein Bild in Breite Farbskala in MacOS auf dem Bildschirm gerendert werden soll muss, die `DrawRect` Methode der `NSView` fraglichen wie gewohnt. Zum Beispiel:

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

Erneut, es auf intelligente Weise entscheidet, ob in der sRGB oder der erweiterte Bereich sRGB Farbraum basierend auf den Funktionen der Mac-Hardware, die die app, dazu, wann ausgeführt wird gerendert werden sollen die `DrawRect` -Methode aufgerufen wird.

Wenn die app steuern die Farbraum verwendet wird möchte, ein Bild gezeichnet werden soll, kann ein neues verwenden `DepthLimit` Eigenschaft von der `NSWindow` Klasse, um den gewünschten Farbraum anzugeben. Dieser Wert kann eine `NSWindowDepth` Enum Folgendes:

- OneHundredTwentyEightBitRgb
- SixtyfourBitRgb
- TwentyfourBitRgb

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden behandelt, Breite, Farbe und den Verfahren, die sie implementiert und innerhalb einer Xamarin.iOS oder Xamarin.Mac-app verwendet werden kann.



## <a name="related-links"></a>Verwandte Links

- [iOS-10-Beispiele](https://developer.xamarin.com/samples/ios/iOS10/)
