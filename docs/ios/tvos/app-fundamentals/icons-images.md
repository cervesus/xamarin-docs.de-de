---
title: Arbeiten mit tvos-Symbolen und-Bildern in xamarin
description: In diesem Dokument wird beschrieben, wie Sie mit Symbolen und Bildern in einer tvos-App arbeiten, die mit xamarin erstellt wurde. Es werden Start Bilder, geschichtete Bilder, das App-Symbol und mehr erläutert.
ms.prod: xamarin
ms.assetid: A2DA4347-0563-4C72-A8D7-5B9DE9E28712
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/16/2017
ms.openlocfilehash: 72c10d10e65194171479d66845d597e313281cdf
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2020
ms.locfileid: "84573780"
---
# <a name="working-with-tvos-icons-and-images-in-xamarin"></a>Arbeiten mit tvos-Symbolen und-Bildern in xamarin

Das Erstellen von faszinierenden Symbolen und Bildern ist ein wichtiger Bestandteil der Entwicklung eines immersiven Benutzer Erlebnisses für Ihre Apple TV-apps. In diesem Leitfaden werden die erforderlichen Schritte zum Erstellen und einschließen der erforderlichen Grafik Ressourcen für Ihre xamarin. tvos-apps behandelt:

- [Startbild](#Launch-Image) : ein Start Bild wird angezeigt, wenn Ihre APP zum ersten Mal gestartet wird, und wird durch den ersten Bildschirm der APP ersetzt, sobald der Startvorgang abgeschlossen ist.
- Überlappende [Bilder](#Layered-Images) , die spezifisch für das Apple TV sind, können die neuen geschichteten Images von Apple mit dem Paramet-Effekt zusammenarbeiten, um einen 3D-Effekt für ausgewählte Elemente zu erstellen Es gibt mehrere Möglichkeiten zum [Erstellen von geschichteten Bildern](#Creating-Layered-Images).
- [App-Symbol](#App-Icons) : Symbole sind nur für den Apple TV-Startbildschirm, sondern für den App Store erforderlich. Sie müssen als geschichtetes Bild bereitgestellt werden.
- [Top-Regal Bild](#Top-Shelf-Image) : Wenn Ihre APP in der obersten Zeile des Startbildschirms platziert wird, benötigt Sie ein Top-Regal Bild, um die Features Ihrer APP hervorzuheben. Optional können Sie [dynamischen Top-Shelf-Inhalt](#Dynamic-Top-Shelf-Content) bereitstellen, um den Inhalt in Ihrer APP hervorzuheben.
- [Game Center Images](#Game-Center-Images) : Wenn Ihre APP ein Spiel ist und Game Center verwendet, sind mehrere weitere Images erforderlich.
- Festlegen von [xamarin. tvos-Projekt Images](#Setting-Xamarin.tvOS-Project-Images) : Hier werden die erforderlichen Schritte zum Festlegen des Start Images und App-Symbols für Ihre xamarin. tvos-App behandelt.

> [!IMPORTANT]
> Alle Images auf dem Apple TV befinden sich in der 1-x-Auflösung ( `@1x` ), und Sie sollten _nur_ Images dieser Größe verwenden. Das einschließen größerer Grafiken mit höherer Auflösung benötigt nicht nur Zeit zum herunterladen und Verwenden von mehr Arbeitsspeicher und Speicher, sondern muss zur Laufzeit dynamisch neu erstellt werden, was sich negativ auf die Zeichnungs Leistung auswirkt.

<a name="Launch-Image"></a>

## <a name="launch-image"></a>Bild starten

Das Startimage ist das erste, was angezeigt wird, wenn Ihre xamarin. tvos-App anfänglich im Apple TV gestartet wird. Daher muss jede tvos-App ein Launch-Image bereitstellen. 

Das Startbild erscheint schnell und gibt den Eindruck, dass Ihre APP schnell und reaktionsfähig ist. Das Apple TV ersetzt das Startbild durch den ersten Bildschirm der app in Kürze.

Start Bilder sind keine Gelegenheit für Werbeeinblendungen oder Kunst Ausdrücke, Sie sind nur vorhanden, um den Eindruck zu erwecken, dass Ihre APP schnell gestartet werden kann und einsatzbereit ist.

|Bild Größe starten|Notizen|
|---|---|
|1920 × 1080px|Nur nicht geschichtete PNG-Dateien|

Apple gibt die folgenden Vorschläge zum Entwerfen des Start Images Ihrer APP an:

- **Nahezu identisch mit dem ersten Bildschirm** : Ihr Startbildschirm sollte so nah wie möglich mit dem ersten Bildschirm Ihrer APP sein. Wenn Sie verschiedene Grafiken oder Elemente einschließen, kann dies zu einem lästigen "Flash" führen, wenn der erste Bildschirm angezeigt wird.
- **Vermeiden Sie die Verwendung von Text** Start Bildern, die statisch sind und daher nicht lokalisiert werden, bevor Sie angezeigt werden.
- **Downplay-Start** : da Apple TV-Benutzer häufig Anwendungen wechseln, sollten Sie nicht auf den App-Startprozess achten.
- **Keine anzeigen oder Brandings** : das Start Image darf nicht als Info Bildschirm verwendet werden oder ein Branding enthalten, es sei denn, es ist ein statischer Teil des ersten Bildschirms Ihrer APP. ADS sind streng unzulässig.

<a name="Setting-the-Launch-Image"></a>

### <a name="setting-the-launch-image"></a>Festlegen des Start Abbilds

Gehen Sie folgendermaßen vor, um das Start Image für Ihr tvos-Projekt festzulegen:

1. Doppelklicken Sie im **Projektmappen-Explorer**, `Assets.xcassets` um es zur Bearbeitung zu öffnen: 

    [![](icons-images-images/asset01.png "The Assets.xcassets file")](icons-images-images/asset01.png#lightbox)
2. Klicken Sie im **Asset-Editor**auf das Medien `LaunchImages` Objekt: 

    [![](icons-images-images/asset02.png "The LaunchImages asset")](icons-images-images/asset02.png#lightbox)
3. Klicken Sie auf den Eintrag **1X Apple TV** , und wählen Sie das Startbild aus, oder ziehen Sie optional ein neues Bild aus dem Dateisystem: 

    [![](icons-images-images/asset03.png "Select a Launch Image")](icons-images-images/asset03.png#lightbox)
4. Speichern Sie die Änderungen.

<a name="Layered-Images"></a>

## <a name="layered-images"></a>Geschichtete Bilder

In der Abbildung von Apple TV haben mehrstufige Bilder mit den Parametern der einzelnen Inhalte zusammengearbeitet, um einen 3D-Effekt zu erzielen, der dabei hilft, den Benutzer auf dem Bildschirm über das gesamte Raum mit dem Inhalt auf dem Bildschirm zu verbinden.

Geschichtete Bilder enthalten zwei (2) bis fünf (5) separate Ebenen, die kombiniert werden, um ein Abbild zu bilden. Mit Ausnahme der Hintergrund Ebene verwendet jede Schicht die Z-Reihenfolge zusammen mit Transparenz, um eine Illusion von Tiefe zu erzeugen. Wenn der Benutzer mit einem geschichteten Bild interagiert, werden höhere Z-geordnete Ebenen skaliert und überlappen, um diesen Effekt zu erzeugen.

[![](icons-images-images/layered01.png "Layered Images Z-ordered diagram")](icons-images-images/layered01.png#lightbox)

> [!IMPORTANT]
> Für die Symbole Ihrer APP sind geschichtete Bilder erforderlich und für andere [Fokussier Bare Elemente](~/ios/tvos/app-fundamentals/navigation-focus.md#Focus-and-Selection) (z. b. das Top-Regal Bild) optional. Apple schlägt jedoch vor, dass Sie für jedes Bild, das in Ihrer APP den Fokus erhält, geschichtete Bilder verwenden können.

Apple gibt die folgenden Vorschläge zum Entwerfen Ihrer geschichteten Images an:

- **Legen Sie die Hintergrund Ebene undurchsichtig** fest: Ihre Hintergrund Ebene (Schicht 1) **muss** nicht transparent sein, oder Sie erhalten eine Fehlermeldung, wenn Sie versuchen, das geschichtete Bild in Apple TV zu verwenden. Alle anderen Ebenen können mehrere Ebenen der Transparenz enthalten, um den 3D-Effekt zu verbessern.
- **Isolieren Sie Vordergrund-, Mittel-und Hintergrundelemente** : Platzieren Sie wichtige Elemente (z. b. Spiel Zeichen) im Vordergrund, und verwenden Sie die Mitte für sekundäre Elemente oder Schatten. Fügen Sie abschließend einen neutralen Hintergrund ein, um eine Phase für die oberen Ebenen bereitzustellen.
- **Behalten Sie Text im Vordergrund bei** , es sei denn, Sie möchten, dass der Text durch eine höhere Ebene verdeckt wird. in der Regel sollte er sich auf der obersten Ebene befinden.
- **Einfache** ebenenweise verwenden: der Teil des Parametern ist so konzipiert, dass er sehr gering ist, sodass Sie die Ebenen möglichst gering halten können, um jarrings, unrealistische Effekte zu vermeiden.
- **Schließen Sie eine sichere Zone ein** . da obere Ebenen bei einem Teil des Effekts zugeschnitten werden können, müssen Sie einen sicheren Zonen Rahmen in jeder Schicht erstellen. Wenn Sie Ihren Inhalt zu einem zu schließende Ebenenrand bringen, kann er entfernt werden. Obere Ebenen werden mehr skalieren und Zuschneiden als niedrigere Ebenen. Weitere Informationen finden Sie unten im Abschnitt Anpassen der [Bildebenen](#Sizing-Image-Layers) .
- **Vorschau von häufig** geschichteten Bildern sollten häufig in der Vorschau angezeigt werden, um sicherzustellen, dass der gewünschte 3D-Effekt auftritt und der Inhalt auf den einzelnen Ebenen nicht abgeschnitten wird. Sie sollten eine Vorschau der überlappenden Images auf echter Apple TV-Hardware anzeigen, um sicherzustellen, dass Sie erwartungsgemäß angezeigt werden

Wenn möglich, sollten Sie immer die integrierten Steuerelemente verwenden, um die überlappenden `UIKit` Bilder anzuzeigen, da Sie automatisch den Teil des Parametern erhalten, wenn Sie den Fokus erhalten.

<a name="Sizing-Image-Layers"></a>

### <a name="sizing-image-layers"></a>Anpassen von Bildebenen

Beachten Sie, dass Sie einen _sicheren Zonen_ Rahmen in jede Schicht einschließen müssen, die ihr geschichtetes Bild bilden wird. Da die einzelnen Ebenen während des-Parametern skaliert und zugeschnitten werden können, kann der Inhalt der Ebenen abgeschnitten werden, wenn er zu nah am Rand der Ebene ist:

[![](icons-images-images/layered02.png "35 pixel border")](icons-images-images/layered02.png#lightbox)

<a name="Creating-Layered-Images"></a>

### <a name="creating-layered-images"></a>Erstellen von geschichteten Bildern

tvos funktioniert mit mehrschichtigen Bildern in den folgenden Formaten:

- **Auto-Dateien** : Dies ist ein proprietäres, von Apple erstelltes Objektkatalog Format. Sie erstellen keine autodateien direkt, Sie werden zur Kompilierzeit aus beliebigen LSR-Dateien erstellt und in Ihrer APP Bundle enthalten.
- **LSR-Images** : Dies ist ein proprietäres Bildformat, das von Apple erstellt wurde. Verwenden Sie das [Adobe Photoshop-Plug](https://itunespartner.apple.com/assets/downloads/ParallaxExporter_Apps.zip) -in "Parser" oder " [Parser](https://itunespartner.apple.com/assets/downloads/Parallax%20Previewer.dmg) ", um überlappende Bilder im LSR-Format zu erstellen und in der Vorschau anzuzeigen.
- **Assets. xcassets** : zwischen zwei (2) und fünf (5) Standard `.png` formatierten Bildern, die in einem Asset-Katalog enthalten sind, die zur Kompilierzeit in ein mit einem Auto formatiertes oder LSR-formatiertes Bild umgewandelt werden.
- **LCR-Dateien** : Dies ist ein proprietäres Dateiformat, das von Apple erstellt wurde. LCR-Dateien sind für die Verwendung als zusätzlicher Inhalt gedacht, der von einem ihrer Inhalts Server heruntergeladen wird. Die LCR-Datei sollte nie in ihren App Bundle eingeschlossen werden. LCR-Dateien werden aus LSR-oder Photoshop-Dateien mithilfe des `layerutil` Befehlszeilen Tools generiert, das in Xcode enthalten ist.

<a name="The-Parallax-Previewer"></a>

### <a name="the-parallax-previewer"></a>Die "Parser"-Vorschau

Apple hat die " [Parser](https://itunespartner.apple.com/assets/downloads/Parallax%20Previewer.dmg) "-Vorschau für die Vorschau und Erstellung von geschichteten Bildern erstellt, die für App-Symbole und optionale Fokussier Bare Elemente erforderlich sind. Der Vorschau zeigt jede Ebene, die das abgeschlossene geschichtete Bild bildet:

[![](icons-images-images/layered03.png "The Parallax Previewer")](icons-images-images/layered03.png#lightbox)

Beim Anzeigen einer Vorschau eines geschichteten Bilds können Sie die Maus verwenden, um das Bild zu drehen und die Vorschau des Parametern anzuzeigen. Verwenden **+** Sie die Schaltflächen (plus) und **-** (minus) zum Hinzufügen und Entfernen von Ebenen.

Beim Erstellen eines neuen geschichteten Bilds kann es im LSR-Format exportiert und in das Paket Ihrer APP eingeschlossen werden.

Weitere Informationen zum Erstellen und in der Vorschau von geschichteten Bildern finden Sie im Abschnitt [Erstellen von para](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/CreatingParallaxArtwork.html#//apple_ref/doc/uid/TP40015241-CH19-SW1) Metern für die Apple-Programmierung des App- [Programmier Handbuchs für tvos](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/).

<a name="App-Icons"></a>

## <a name="app-icons"></a>App-Symbole

Ihre xamarin. tvos-App benötigt nicht nur ein App-Symbol für den Apple TV-Startbildschirm, sondern auch ein Symbol für den App Store. Das App-Symbol ist die erste Änderung, mit der Sie sich einen guten Eindruck von Ihrem potenziellen Benutzer verschaffen und den Zweck Ihrer APP auf einen Blick vermitteln sollten.

[![](icons-images-images/icon01.png "The App Icon")](icons-images-images/icon01.png#lightbox)

Jede APP muss sowohl eine kleine als auch eine große Version des App-Symbols bereitstellen. Das kleine Symbol wird bei der Installation der APP auf dem Apple TV-Startbildschirm verwendet. Die umfangreiche Version wird vom App Store verwendet. Das Symbol für große apps sollte das Aussehen und Gefühl der kleinen Symbol Version imitieren.

|Kleines Symbol||Großes Symbol||
|---|---|---|---|
|Tatsächliche Größe|400x240 px|Size|1280x768px|
|Größe der sicheren Zone|370x222px|||
|Größe ohne Fokus|300 x 180px|||
|Fokus Größe|370x222px|||

> [!IMPORTANT]
> Ihre APP-Symbole müssen als **geschichtete Bilder**bereitgestellt werden. Weitere Informationen finden Sie im Abschnitt überlappende [Images](#Layered-Images) .

Apple bietet die folgenden Vorschläge zum Erstellen Ihrer APP-Symbole:

- **Stellen Sie einen einzelnen Fokuspunkt bereit** – entwerfen Sie das Symbol mit einem einzelnen Schwerpunkt Punkt, der direkt in der Mitte des Symbols platziert wird.
- **Verwenden Sie einen einfachen Hintergrund** – halten Sie den Symbol Hintergrund so einfach, dass die oberen Ebenen ausstehen. Verwenden Sie eine einfache Farbe oder einen subtilen Farbverlauf.
- **Begrenzen Sie die Menge an Text** – da der Name der APP unter dem Symbol angezeigt wird, wenn er vom Benutzer ausgewählt wird, sollten Sie nur Text einschließen, wenn dies für den Entwurf des Symbols von Bedeutung ist.
- **Verwenden Sie keine Screenshots** – Screenshots sind zu komplex für ein Symbol und ermöglichen dem Benutzer nicht, den Zweck der APP auf einen Blick anzuzeigen.
- **Symbole speichern** – tvos wendet automatisch eine Maske an, mit der die Ecken der Symbole auf eine beliebige Ecke gerundet werden. Schließen Sie diese Rundung nicht ein.
- **Trennen Sie Ihre Ebenen sorgfältig** – der Text sollte sich auf der oberen Ebene, sekundären Elementen in der Mitte und einem neutralen Hintergrund befinden, der die Obergrenzen des oberen Ebenen ermöglicht.
- **Verwenden Sie Farbverläufe und Schatten sorgfältig** – Farbverläufe und Schatten können mit den Parametern in Konflikt stehen, sodass Sie sorgfältig verwendet werden sollten. Einfache, helle Farbverlaufs Stile von oben nach unten funktionieren am besten. Schatten funktionieren in der Regel am besten als Spitze, hart über schneidige tints.
- **Variieren der Ebenentransparenz** – verwenden Sie unterschiedliche Transparenz Ebenen auf den oberen Ebenen des App-Symbols, um den 3D-Effekt zu erhöhen. Die Hintergrund Ebene muss undurchsichtig sein, da Sie zu einem Fehler führt.

<a name="Setting-the-App-Icons"></a>

### <a name="setting-the-app-icons"></a>Festlegen der APP-Symbole

Gehen Sie folgendermaßen vor, um die für Ihr tvos-Projekt erforderlichen App-Symbole festzulegen:

1. Doppelklicken Sie im **Projektmappen-Explorer**, `Assets.xcassets` um es zur Bearbeitung zu öffnen: 

    [![](icons-images-images/asset01.png "The Assets.xcassets fileg")](icons-images-images/asset01.png#lightbox)
2. Erweitern Sie im **Asset-Editor**das `App Icon & Top Shelf Image` Objekt: 

    [![](icons-images-images/asset04.png "Expand the Top Shelf Image asset")](icons-images-images/asset04.png#lightbox)
3. Erweitern Sie als nächstes das Medien `App Icon - Small` Objekt: 

    [![](icons-images-images/asset05.png "Expand the App Icon - Small asset")](icons-images-images/asset05.png#lightbox)
4. Erweitern Sie dann das `Back` Objekt, und klicken Sie auf den `Contents` Eintrag: 

    [![](icons-images-images/asset06.png "Then expand the Back asset")](icons-images-images/asset06.png#lightbox)
5. Klicken Sie auf den **Eintrag 1X Apple TV** , und wählen Sie eine Bilddatei aus.
6. Wiederholen Sie die obigen Schritte für die `Front` `Middle` Objekte und.
7. Wiederholen Sie dann die gleichen Schritte, um das Medienobjekt zu definieren `App Icon - Large` .
8. Speichern Sie die Änderungen.

<a name="Top-Shelf-Image"></a>

## <a name="top-shelf-image"></a>Top-Regal Bild

Wenn der Benutzer die xamarin. tvos-app in der obersten Zeile des Apple TV-Startbildschirms abgelegt hat, wird ein großes hoch Regal Bild angezeigt, wenn die APP vom Benutzer ausgewählt wird. Dieses Bild sollte die Features Ihrer APP hervorheben oder direkte Links zu den Inhalten bereitstellen.

[![](icons-images-images/topshelf01.png "Top Shelf Image example")](icons-images-images/topshelf01.png#lightbox)

Das Top-Regal Bild kann entweder als einzelne statische `.png` Datei oder Datei bereitgestellt werden `.lsr` (siehe [Erstellen von geschichteten Bildern](#Creating-Layered-Images)), oder es kann zur Laufzeit dynamisch als einzelne Zeile mit Fokus nutzbaren Elementen erstellt werden (siehe [dynamischer Top-Shelf-Inhalt](#Dynamic-Top-Shelf-Content) unten).

|Größe des oberen Regal Bilds|Notizen|
|---|---|
|1920 x 720px|Statische PNG-oder geschichtete LSR-Datei|

Apple bietet die folgenden Vorschläge zum Erstellen Ihrer Top-Regal-Images:

- **Verwenden reichhaltiger statischer Bilder** – Wenn Ihre APP keinen dynamischen Inhalt bereitstellt, kann das oberste Regal Bild nicht verwendet werden. Verwenden Sie dieses Image, um die Features der APP oder Ihrer Marke hervorzuheben.
- **Verknüpfung mit App-Inhalten** – dynamische Top-Regal-Layouts bieten einen schnellen Link zu den Inhalten, die Ihr Benutzer in Ihrer APP am wichtigsten findet. Verwenden Sie diesen Bereich, um einen schnellen Link zum Starten der APP bereitzustellen und sofort in den gegebenen Inhalt zu springen.
- **Präsentieren Sie den neuesten Inhalt** – umfassende Inhalte mit Top-Shelf können einen Benutzer in Ihre APP ziehen und ihn mehr verwenden. Verwenden Sie diese als Bereich, um den höchsten bewerteten oder neuesten Inhalt zu präsentieren.
- **Personalisierter Inhalt** – Benutzer platzieren Ihre am häufigsten verwendeten oder bevorzugten apps in der obersten Zeile des Startbildschirms. Verwenden Sie das obere Regal, um den Inhalt anzuzeigen, den Sie am meisten interessiert.
- **ADS nicht zulässig** – Werbeeinblendungen dürfen im oberen Regal nicht angezeigt werden. Sie können den neuesten, von purchgbaren Inhalt anzeigen, es sollten jedoch keine Preisinformationen angezeigt werden.

### <a name="setting-the-top-shelf-image"></a>Festlegen des oberen Regal Bilds

Gehen Sie folgendermaßen vor, um das oberste Regal Bild festzulegen, das für Ihr tvos-Projekt erforderlich ist:

1. Doppelklicken Sie im **Projektmappen-Explorer**, `Assets.xcassets` um es zur Bearbeitung zu öffnen: 

    [![](icons-images-images/asset01.png "The Assets.xcassets file")](icons-images-images/asset01.png#lightbox)
2. Erweitern Sie im **Asset-Editor**das `App Icon & Top Shelf Image` Objekt: 

    [![](icons-images-images/asset04.png "Expand the Top Shelf Image asset")](icons-images-images/asset04.png#lightbox)
3. Klicken Sie auf das Medien `Top Shelf Image` Objekt: 

    [![](icons-images-images/asset07.png "The Top Shelf Image asset")](icons-images-images/asset07.png#lightbox)
4. Klicken Sie auf den **Eintrag 1X Apple TV** , und wählen Sie eine Bilddatei aus.
5. Speichern Sie die Änderungen.

<a name="Dynamic-Top-Shelf-Content"></a>

### <a name="dynamic-top-shelf-content"></a>Dynamischer Top-Shelf-Inhalt

Anstatt ein statisches Top-Regal Bild anzuzeigen, kann der obere Regal eine dynamische Zeile von [Fokus verwendbaren Elementen](~/ios/tvos/app-fundamentals/navigation-focus.md#Focus-and-Selection) oder einen dynamischen Satz von scrollbanner enthalten. Beide dynamischen Stile ermöglichen es Ihnen, den von der APP bereitgestellten Inhalt hervorzuheben oder zu den am häufigsten verwendeten Features zu springen.

<a name="Sectioned-Content-Row"></a>

#### <a name="sectioned-content-row"></a>Zeile mit Abschnitts Inhalt

Dieser dynamische Top-Regal-Inhaltstyp zeigt eine einzelne Zeile mit Bild Lauf baren Elementen, die optional in Abschnitte unterteilt sind. Sie wird in der Regel verwendet, um neue, bevorzugte oder zuletzt angezeigte App-Inhalte hervorzuheben.

Der Inhalt wird als einzelne, horizontale Bildlauf-Inhaltsliste dargestellt, wobei eine Bezeichnung unter dem aktuell ausgewählten Inhalt angezeigt wird (der derzeit den Fokus besitzt). Wenn der Benutzer ein bestimmtes Inhalts Element auswählt, wird die APP gestartet und sollte direkt in diesen Inhalt übernommen werden.

Die folgenden Inhalts Größen sind erforderlich:

||Poster (2:3)|Square (1:1)|HDTV (16:9)|
|---|---|---|---|
|Tatsächliche Größe|404x608px|608x608px|908x512 px|
|Größe der sicheren Zone|380x570px|570x570px|852x479px|
|Größe ohne Fokus|333x500.000|500.000 × 500.000 Pixel|782x440px|
|Fokus Größe|380x570px|570x570px|852x479px|

Apple bietet die folgenden Vorschläge für die Zeile mit den Zeilen mit Inhalt:

- **Vervollständigen Sie die Zeile** – Sie sollten genügend Inhalte bereitstellen, um die gesamte Bildschirmbreite zu überspannen.
- **Skalieren gemischter Images** – die Zeile mit den Zeilen mit dem Inhalt wurde so entworfen, dass Sie eine Mischung aus Bildgrößen (aus der oben angegebenen Liste) enthält. Wenn Sie jedoch Bildgrößen kombinieren, beachten Sie, dass zusätzliche Skalierung angewendet wird, um die Inhalts Anzeige zu normalisieren.

<a name="Scrolling-Inset-Banners"></a>

#### <a name="scrolling-inset-banners"></a>Scrollen in INSET-Banner

Optional kann Ihre xamarin. tvos-App ihren Inhalt im oberen Regal als automatische Bildlauf-und Schleifen Auflistung von transparenten präsentieren, die den Bildschirm fast ausfüllen. Dieser Stil wird in der Regel verwendet, um umfangreiche, neue Inhalte wie neue Fernsehsendungen zu präsentieren.

Zusätzlich zum automatischen Bildlauf kann der Benutzer die Kontrolle über die Banner und den Bildlauf in beide Richtungen mithilfe von Siri Remote durchführen. Wenn ein Banner im Fokus ist, wird eine kleine, kreisförmige Geste auf der Siri-Remote Bewegung aktiviert.

**Banner Bild (extra breit)**

|   |   |
|---|---|
|Tatsächliche Größe|1940× 624px|
|Größe der sicheren Zone|1740x620px|
|Größe ohne Fokus|1740x560px|
|Fokus Größe|1740x620px|

Bildlauf-INSET-Banner können als statische `.png` oder geschichtete Datei bereitgestellt werden `.lsr` .

Apple bietet die folgenden Vorschläge für die scrollinset-Banner:

- **Inhalts Betrag** : Sie sollten mindestens drei Banner für den Bildlauf angeben, damit der Bildlauf naturgemäß angezeigt wird. Sie sollten nicht mehr als acht (8) Banner einschließen oder die Navigation für den Endbenutzer hart machen.
- **Inhalts Text** : Wenn für Ihr Banner Text in erforderlich ist, sollte das Banner Bild enthalten sein. Wenn Sie geschichtete Bilder verwenden, sollte sich der Text auf der obersten Ebene befinden.

Weitere Informationen zum Hinzufügen einer Top-Regal-Erweiterung zu ihrer App finden Sie in der [Referenz zum tvservices-Framework](https://developer.apple.com/library/prerelease/tvos/documentation/TVServices/Reference/TVServices_Ref/index.html#//apple_ref/doc/uid/TP40016412) von Apple, um dynamische Top-Regal Inhalte bereitzustellen.

<a name="Game-Center-Images"></a>

## <a name="game-center-images"></a>Game Center Bilder

Wenn Ihre xamarin. tvos-App ein Spiel ist und Sie Game Center Support eingeschlossen haben, sind mehrere Bild Ressourcen erforderlich:

- **Erfolgs Symbole** : ein undurchsichtiges Bild ist für jede der einzelnen Leistungen erforderlich, die automatisch in einen Kreis zugeschnitten werden. Die Ergebnisse sind nicht Fokus nutzbare Elemente.
- **Dashboard-Grafik** : Sie können ein optionales Bild bereitstellen, das im oberen Bereich des App-dashGame Center Boards angezeigt wird. Diese Bilder können nicht verwendet werden.
- **Leaderboard-Grafik** : Sie müssen zwischen einem (1) und drei (3) 16:9-Seitenverhältnis-Bild für jede Bestenlisten bereitstellen, die ihre App unterstützt. Dabei kann es sich entweder um statische `.png` oder geschichtete `.lsr` Dateien handeln. Die Leaderboard-Grafik kann als Fokus verwendet werden.

||Symbol "Erfolge"|Dashboard-Grafik|Leaderboard-Grafik|
|---|---|---|---|
|Sichtbare Größe|200x200px|923x150px|–|
|Tatsächliche Größe|320x320 px|–|659x371px|
|Größe der sicheren Zone|–|–|618x348px|
|Größe ohne Fokus|–|–|548x309px|
|Fokus Größe|–|–|618x348px|

Weitere Informationen zum Arbeiten mit Game Center finden Sie im [Game Center-Programmier Handbuch](https://developer.apple.com/library/prerelease/tvos/documentation/NetworkingInternet/Conceptual/GameKit_Guide/Introduction/Introduction.html)von Apple.

<a name="Working-with-Images"></a>

## <a name="working-with-images"></a>Working with Images (Arbeiten mit Bildern)

Da tvos 9 eine Teilmenge von IOS 9 ist, funktionieren die gleichen Techniken zum einschließen und Anzeigen von Bildern in einer xamarin. IOS-APP auch für eine xamarin. tvos-app. Weitere Informationen finden Sie in unserer [Bild](~/ios/app-fundamentals/images-icons/displaying-an-image.md) Dokumentation.

<a name="Setting-Xamarin.tvOS-Project-Images"></a>

## <a name="setting-xamarintvos-project-images"></a>Festlegen von xamarin. tvos-Projekt Images

Wie bereits erwähnt, erfordern alle tvos-apps ein [Start Image](#Launch-Image)und ein [App-Symbol](#App-Icons). In diesem Abschnitt wird beschrieben, wie Sie das Symbol Start Bild und App für Ihr xamarin. tvos-App-Projekt auswählen, nachdem Sie in einem Ressourcen Katalog festgelegt wurden.

Gehen Sie folgendermaßen vor:

1. Doppelklicken Sie in der **Projektmappen-Explorer**auf das, `Info.plist` um es zur Bearbeitung zu öffnen: 

    [![](icons-images-images/info01.png "The Info.plist file")](icons-images-images/info01.png#lightbox)
2. Wählen Sie im **Info. plist-Editor**den Ressourcen Katalog (oben im Abschnitt [Festlegen der APP-Symbole](#Setting-the-App-Icons) konfiguriert) für die **App-Symbole**aus: 

    [![](icons-images-images/info02.png "The Info.Plist Editor")](icons-images-images/info02.png#lightbox)
3. Wählen Sie als nächstes den Ressourcen Katalog (oben im Abschnitt [Festlegen des Start Abbilds](#Setting-the-Launch-Image) konfiguriert) für die **Start Images**aus.
4. Speichern Sie die Änderungen.

<a name="Summary"></a>

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden alle in einer xamarin. tvos-App verwendeten Abbild Typen und-Größen behandelt. Zuerst wurden Start Bilder, geschichtete Bilder, App-Symbole, Top-Shelf-Bilder und Game Center Bilder abgedeckt. Anschließend wird die Arbeit mit Bildern in ihrer xamarin. tvos-App behandelt.

## <a name="related-links"></a>Verwandte Links

- [tvOS-Beispiele](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [tvOS](https://developer.apple.com/tvos/)
- [tvos Human Interface Guides](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Leitfaden zur APP-Programmierung für tvos](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
