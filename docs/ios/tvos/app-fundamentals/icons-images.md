---
title: Arbeiten mit TvOS Symbole und Bilder in Xamarin
description: Dieses Dokument beschreibt das Arbeiten mit Symbolen und Bildern in einer TvOS-app mit Xamarin erstellt wurde. Es wird erläutert, startbilder, mehrschichtiger Abbilder, das app-Symbol und vieles mehr.
ms.prod: xamarin
ms.assetid: A2DA4347-0563-4C72-A8D7-5B9DE9E28712
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: 96af7fab366c3fd3493cf5adbf183d80b7c1ee26
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61418194"
---
# <a name="working-with-tvos-icons-and-images-in-xamarin"></a>Arbeiten mit TvOS Symbole und Bilder in Xamarin

Erstellen ansprechender Symbole und Bilder sind ein wichtiger Bestandteil der Entwicklung eine beeindruckende benutzererfahrung für Ihre Apple TV-apps. Dieses Handbuch befasst sich die erforderlichen Schritte zum Erstellen und integrieren Sie die erforderlichen grafischen Ressourcen für Ihre Xamarin.tvOS-apps:

- [Startbild](#Launch-Image) -ein startbilds, das angezeigt wird, wenn Ihre app zum ersten Mal gestartet, und wird nach Abschluss der Start vom ersten Bildschirm der app ersetzt.
- [Mehrstufige Images](#Layered-Images) – speziell für Apple TV, Apple neue Images mit Ebenen Arbeit mit den Parallaxeneffekt, um eine 3D-Effekt für ausgewählte Elemente zu erstellen. Es gibt mehrere Möglichkeiten, [Mehrschicht-Images erstellen](#Creating-Layered-Images).
- [Symbol "App"](#App-Icons) -Symbole erforderlich sind, nicht nur Apple TV Home-Bildschirm jedoch für den App Store. Sie müssen als Mehrschicht-Image bereitgestellt werden.
- [Top-Shelf-Bild](#Top-Shelf-Image) – Wenn Ihre app auf der obersten Zeile des Startbildschirms platziert wird, benötigen sie einen Top Shelf Image, um Ihrer app Funktionen zu markieren. Optional können Sie angeben [dynamischen Inhalt der Top-Shelf](#Dynamic-Top-Shelf-Content) , markieren Sie den Inhalt in Ihrer app.
- [Game Center-Images](#Game-Center-Images) – Wenn Ihre app ein Spiel ist und Game Center verwendet, werden verschiedene weitere Images erforderlich sein.
- [Festlegen Xamarin.tvOS-Projekt Bilder](#Setting-Xamarin.tvOS-Project-Images) -enthält die erforderlichen Schritte für das Image zu starten und die App-Symbol für Ihre Xamarin.tvOS-app einzurichten.

> [!IMPORTANT]
> Alle Bilder auf Apple TV zu werden, in der 1 X-Auflösung (`@1x`) und Sie sollten _nur_ Images dieser Größe verwenden. Einschließlich größer, Grafiken mit höherer Auflösung dauern, nicht nur zum Herunterladen und Verwenden von mehr Arbeitsspeicher und Speicher, aber sie müssen zur Laufzeit dynamisch neu skaliert werden und wirkt sich negativ auf die Rendergeschwindigkeit.

<a name="Launch-Image" />

## <a name="launch-image"></a>Startbild

Das Image starten wird als erstes, das angezeigt wird, wenn Ihre app Xamarin.tvOS anfänglich auf Apple TV gestartet wird, und daher jede TvOS-app muss angeben, ein Image zu starten. 

Das Image zu starten, erscheint schnell und den Eindruck entstehen lässt, dass Ihre app schnell und reaktionsfähig ist. Apple TV ersetzt das Abbild starten mit dem ersten Bildschirm Ihrer App in Kürze es nach.

Startbilder nicht Gelegenheit für Werbung oder künstlerische Ausdruck, sie dienen lediglich der Eindruck entstehen, dass Ihre app schnell gestartet und verwendet.

|Starten Sie die Größe des Abbilds|Hinweise|
|---|---|
|1920x1080px|Nur die Ebenen nicht PNG-Dateien|

Apple stellt die folgenden Vorschläge für das Entwerfen von Startbildgruppe Ihrer app:

- **Fast identisch mit dem ersten Bildschirm** -der Startbildschirm sollte wie in der Nähe Ihrer app ersten Bildschirm wie möglich sein. Z. B. verschiedene Grafiken oder Element kann eine lästige führen "flash", wenn der erste Bildschirm angezeigt wird.
- **Vermeiden Sie Text mithilfe von** -Startbilder sind statisch und wird daher nicht vor seiner Anzeige lokalisiert werden.
- **Starten Sie herunterspielen** -da Apple TV-Benutzer werden häufig apps wechseln, sollten nicht an den Prozess der app-Start aufgezeigt.
- **Weder anzeigen noch Branding** -Ihre Startbildgruppe sollte nicht verwendet werden, wie ein Bildschirm "Info" oder sind branding, es sei denn, es sich um statischen Teil des ersten Bildschirm Ihrer app ist. ADS sind streng untersagt.

<a name="Setting-the-Launch-Image" />

### <a name="setting-the-launch-image"></a>Das Startbild, das Festlegen

Führen Sie zum Festlegen der starten-Image für Ihr Projekt TvOS folgende:

1. In der **Projektmappen-Explorer**, doppelklicken Sie auf `Assets.xcassets` um ihn zur Bearbeitung zu öffnen: 

    [![](icons-images-images/asset01.png "Die Datei Assets.xcassets")](icons-images-images/asset01.png#lightbox)
2. In der **Ressourcen-Editor**, klicken Sie auf die `LaunchImages` Medienobjekt: 

    [![](icons-images-images/asset02.png "Das Medienobjekt LaunchImages")](icons-images-images/asset02.png#lightbox)
3. Klicken Sie auf die **1 x Apple TV** Eintrag, und wählen Sie das Image zu starten, oder ziehen Sie optional ein neues Image in aus dem Dateisystem: 

    [![](icons-images-images/asset03.png "Wählen Sie ein Startbilds, das")](icons-images-images/asset03.png#lightbox)
4. Speichern Sie die Änderungen.

<a name="Layered-Images" />

## <a name="layered-images"></a>Mehrschichtige Abbilder

Noch nicht mit Apple TV Mehrschicht-Images-arbeiten mit den Parallaxeneffekt, um eine 3D-Effekt zu erstellen, mit dem der Benutzer auf dem Sofa, die auf den Inhalt auf dem Bildschirm mental verbunden werden, durch das Zimmer gehalten wird.

Mehrschichtige Abbilder enthalten von zwei (2) auf fünf (5) Trennen von Ebenen, die kombiniert werden, um ein vollständiges Image zu erstellen. Mit Ausnahme der Hintergrundebene verwendet jede Ebene der Z-Reihenfolge zusammen mit Transparenz, um eine Illusion von Tiefe zu erstellen. Wenn der Benutzer mit einem Image Layered interagiert, sind höhere Z geordnete Ebenen skaliert und überlappende, um diesen Effekt zu erstellen.

[![](icons-images-images/layered01.png "Überlappende Images Z geordnete-Diagramm")](icons-images-images/layered01.png#lightbox)

> [!IMPORTANT]
> Mehrschichtige Abbilder für Ihre app Symbole erforderlich sind, und sind optional, für andere [Fokussierbare Elemente](~/ios/tvos/app-fundamentals/navigation-focus.md#Focus-and-Selection) (z. B. die Top Shelf Image). Allerdings schlägt Apple vor, die Verwendung von Images mit Ebenen für jedes Bild, das in Ihrer app den Fokus erhalten kann.




Apple stellt die folgenden Vorschläge zum Entwerfen Ihrer Ebenen Images:

- **Die Hintergrund-Ebene nicht transparent machen** -der Hintergrundebene (Ebene 1) **müssen** nicht transparent sein, oder Sie erhalten einen Fehler auf, wenn Sie versuchen, das Image mit Ebenen auf Apple TV zu verwenden. Alle anderen Ebenen können mehrere Ebenen von Transparenz zur Verbesserung des 3D-Effekt enthalten.
- **Zu isolieren, Vordergrund, mittleren und Elemente im Hintergrund** -gut sichtbaren Elemente (z. B. Spiele Zeichen) in den Vordergrund zu platzieren, verwenden Sie die Mitte für sekundäre Elemente oder Schatten. Schließlich enthalten Sie einen neutralen Hintergrund um eine Stufe für die oberen Ebenen bereitzustellen.
- **Behalten Sie Text im Vordergrund** – es sei denn, Ihr Text von höheren Ebenen verdeckt werden soll, in der Regel wird auf der obersten Ebene.
- **Verwenden Sie einfache Schichten** -die Parallaxeneffekt wurde entworfen, um geringfügige werden halten Sie also Ihre Ebenen, eine minimale bedürfen, unrealistisch Auswirkungen vermieden werden.
- **Eine sichere Zone enthalten** -da obere Schichten während einer Parallaxeneffekt abgeschnitten werden können, müssen Sie einen Rahmen für die sichere Zone in jeder Ebene zu erstellen. Wenn Sie Ihre Inhalte zu nah am Rand des Ebenen erhalten, kann es abgeschnitten werden. Obere Schichten treten weitere Skalierung und als unteren Schichten zuschneiden. Finden Sie unter den [Größenanpassung Imageebenen](#Sizing-Image-Layers) Abschnitt weiter unten.
- **Vorschau häufig** -Mehrschicht-Images sollten regelmäßig, um sicherzustellen, dass der gewünschte 3D-Effekt tritt ein, und keine des Inhalts auf den einzelnen Ebenen zugeschnitten wird, wird Vorschau angezeigt werden. Sie sollten Vorschaubilder Ihrer Ebenen auf echter Apple TV-Hardware, stellen Sie sicher, dass sie werden wie erwartet gerendert.

Wann immer möglich, verwenden Sie immer die integrierte `UIKit` Steuerelemente Ihre Ebenen Bilder anzuzeigen, wie sie automatisch den Parallaxeneffekt erhalten werden, wenn sie in den Fokus fallen.

<a name="Sizing-Image-Layers" />

### <a name="sizing-image-layers"></a>Größenanpassung Imageebenen

Es ist wichtig, denken Sie daran, eine _sichere Zone_ Rahmen in jeder Schicht, die Ihr Image Ebenen erstellen. Da die einzelnen Ebenen skaliert und, die bei den Parallaxeneffekt zugeschnitten werden können, kann der Inhalt der Ebenen zugeschnitten ist dies nah an der Schicht Edge:

[![](icons-images-images/layered02.png "35 Pixel-Rahmen")](icons-images-images/layered02.png#lightbox)

<a name="Creating-Layered-Images" />

### <a name="creating-layered-images"></a>Erstellen von mehrstufige Images

TvOS funktioniert mit Ebenen von Bildern in den folgenden Formaten:

- **Auto-Dateien** – Dies ist ein proprietäres Asset-Katalog-Format, das von Apple entwickelt wurde. Auto-Dateien wird nicht direkt erstellen, werden sie zum Zeitpunkt der Kompilierung aus beliebigen LSR-Dateien erstellt und in Ihres app-Bundles eingeschlossen.
- **LSR Images** – Dies ist ein proprietäres Bildformat, das von Apple entwickelt wurde. Verwenden der [Parallax Ausführer Adobe Photoshop-Plug-in](https://itunespartner.apple.com/assets/downloads/ParallaxExporter_Apps.zip) oder [Parallax-Vorschau](http://itunespartner.apple.com/assets/downloads/Parallax%20Previewer.dmg) erstellen und Vorschaubilder mit Ebenen im LSR Format.
- **Assets.xcassets** – aus zwei (2) auf fünf (5) standard `.png` formatierte Bildern in ein Asset-Katalog, der in ein Auto oder LSR kompiliert wird formatiert Mehrschicht-Image zum Zeitpunkt der Kompilierung.
- **LCR-Dateien** – Dies ist ein proprietäres Format von Apple entwickelt wurde. LCR-Dateien dienen als zusätzliche Inhalte, die von einem der Content-Server heruntergeladen verwendet werden soll. LCR-Datei sollte nie in Ihrer app-Bündel enthalten sein. LCR-Dateien werden generiert, aus LSR oder Photoshop-Dateien, die mithilfe der `layerutil` -Befehlszeilentools, die in Xcode enthalten.

<a name="The-Parallax-Previewer" />

### <a name="the-parallax-previewer"></a>Der Parallax-Vorschau

Apple erstellt die [Parallax-Vorschau](http://itunespartner.apple.com/assets/downloads/Parallax%20Previewer.dmg) zu Vorschau- und erstellten Mehrschicht-Images für App-Symbole und optional den Fokus erhalten kann Elemente erforderlich sind. Der Previewer wird jeder Ebene, die den abgeschlossenen Images von Ebenen bildet:

[![](icons-images-images/layered03.png "Der Parallax-Vorschau")](icons-images-images/layered03.png#lightbox)

Während der Vorschau eines Mehrschicht-Images, können Sie die Maus verwenden, zum Drehen eines Bilds und Vorschau den Parallaxeneffekt. Verwenden der **+** (Pluszeichen) und **-** (Schaltflächen zum Hinzufügen und Entfernen von Ebenen minus).

Wenn Sie ein neues Image mit Ebenen zu erstellen, können Sie in dem LSR-Format exportiert und in Ihrer app Bundle eingeschlossen werden.

Weitere Informationen zum Erstellen und Anzeigen einer Vorschau Mehrschicht-Images finden Sie unter Apple [Parallax-Grafik erstellen](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/CreatingParallaxArtwork.html#//apple_ref/doc/uid/TP40015241-CH19-SW1) Teil der [App-Programmierhandbuch für TvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/).

<a name="App-Icons" />

## <a name="app-icons"></a>App-Symbole

Ihre Xamarin.tvOS-app wird nicht nur App-Symbol für den Apple TV-Home-Bildschirm, sondern auch ein Symbol für die Store-App benötigen. Das App-Symbol ist das Ihre erste ändern, um eine hervorragende Eindruck für Ihre potenziellen Benutzer ist, und sprechen Zweck von Ihrer app auf einen Blick.

[![](icons-images-images/icon01.png "Das App-Symbol")](icons-images-images/icon01.png#lightbox)

Jede app muss sowohl eine kleine als auch eine große Version der App-Symbol angeben. Das kleine Symbol wird auf dem Bildschirm Apple TV-Startseite verwendet werden, wenn die app installiert ist. Die große Version wird von der App Store verwendet. Die großen App-Symbol sollte es sich um das Aussehen und Verhalten der Version Minisymbols imitieren.

|Kleines Symbol||Großes Symbol||
|---|---|---|---|
|Tatsächliche Größe|400x240px|Größe|1280x768px|
|Sichere Zone Größe|370x222px|||
|Ohne Fokus Größe|300x180px|||
|Fokussierte Größe|370x222px|||

> [!IMPORTANT]
> Ihre App-Symbole muss angegeben werden, als **Layered Images**. Informieren Sie sich die [Mehrschicht-Image](#Layered-Images) Abschnitt Weitere Informationen.




Apple bietet die folgenden Vorschläge zum Erstellen Ihrer App-Symbole:

- **Geben Sie einen einzelnen Fokuspunkt** – Entwurf Ihrer Symbol mit einem einzelnen Fokuspunkt platziert wird, direkt in der Mitte des Symbols.
- **Verwenden Sie einfache Hintergrund** – Ihre Symbolhintergrund einfach halten, damit die oberen Schichten abhebt. Erwägen Sie eine einfache Farbe oder einen feinen Farbverlauf.
- **Begrenzen der Menge von Text** – da es sich bei den Namen der app unterhalb des Symbols angezeigt wird, wenn sie vom Benutzer ausgewählt wird, Sie sollten nur Text beim einschließen ist es wichtig, für das Design des Symbols.
- **Verwenden Sie keine Screenshots** – Screenshots sind zu komplex für ein Symbol und nicht den Zweck der app auf einen Blick anzeigen können.
- **Keep Symbole Quadrat** – TvOS wendet automatisch eine Maske, das die Ecken der Ihre Symbole leicht rundet. Nehmen Sie nicht durch diese Rundung selbst.
- **Trennen der Ebenen sorgfältig** – sollte oben angezeigt werden, die meisten Schicht, sekundäre Elemente in der Mitte und einem neutralen Hintergrund, die die oberen Schichten zu bringen kann.
- **Verwenden Sie Verläufe und Schatten sorgfältig** – Farbverläufe und Schatten können miteinander in Konflikt geraten mit den Parallaxeneffekt sollte sorgfältig verwendet werden. Einfache oben-nach-unten, funktionieren am besten, Licht-dunkel-Farbverlauf-Formate. Zeichnen von Schatten arbeiten normalerweise am besten als scharfe, mit harten Kanten Farbtöne.
- **Variieren Sie die Transparenz** – verwenden Sie unterschiedliche Zugriffsebenen Transparenz für die oberen Ebenen des Ihr App-Symbol, um der 3D-Effekt zu erhöhen. Die Hintergrundebene muss nicht transparent sein oder zu einem Fehler führt.

<a name="Setting-the-App-Icons" />

### <a name="setting-the-app-icons"></a>Festlegen der App-Symbole

Führen Sie zum Festlegen der App-Symbole, die für Ihr Projekt TvOS erforderlich sind folgende:

1. In der **Projektmappen-Explorer**, doppelklicken Sie auf `Assets.xcassets` um ihn zur Bearbeitung zu öffnen: 

    [![](icons-images-images/asset01.png "Die Assets.xcassets fileg")](icons-images-images/asset01.png#lightbox)
2. In der **Ressourcen-Editor**, erweitern Sie die `App Icon & Top Shelf Image` Medienobjekt: 

    [![](icons-images-images/asset04.png "Erweitern Sie das Medienobjekt Top Shelf Image")](icons-images-images/asset04.png#lightbox)
3. Erweitern Sie als Nächstes die `App Icon - Small` Medienobjekt: 

    [![](icons-images-images/asset05.png "Erweitern Sie das Symbol der App - kleine asset")](icons-images-images/asset05.png#lightbox)
4. Erweitern Sie dann die `Back` Bestand, und klicken Sie auf die `Contents` Eintrag: 

    [![](icons-images-images/asset06.png "Erweitern Sie dann auf das Back-Medienobjekt")](icons-images-images/asset06.png#lightbox)
5. Klicken Sie auf die **1 x Apple TV-Eintrag** , und wählen Sie eine Bilddatei.
6. Wiederholen Sie die oben genannten Schritte für die `Front` und `Middle` Bestand.
7. Wiederholen Sie dann die gleichen Schritte zum Definieren der `App Icon - Large` Asset.
4. Speichern Sie die Änderungen.

<a name="Top-Shelf-Image" />

## <a name="top-shelf-image"></a>Top-Shelf-Bild

Wenn der Benutzer Ihre Xamarin.tvOS-app auf der obersten Zeile, auf dem Bildschirm Apple TV-Startseite aufgegeben hat, wird eine große Top Shelf Image angezeigt, wenn es sich bei Ihrer app vom Benutzer ausgewählt wird. Dieses Image sollte markieren Sie die Funktionen Ihrer App oder enthalten direkte Links zum jeweiligen Inhalt.

[![](icons-images-images/topshelf01.png "Top-Shelf-Bild-Beispiel")](icons-images-images/topshelf01.png#lightbox)

Die Top Shelf Image kann entweder als eine einzelne statische bereitgestellt `.png` oder `.lsr` Datei (finden Sie unter [Mehrschicht-Images erstellen](#Creating-Layered-Images)) oder es werden dynamisch erstellt zur Laufzeit als eine einzelne Zeile den Fokus erhalten kann Elemente (finden Sie unter [ Dynamische Oberes Regal Inhalt](#Dynamic-Top-Shelf-Content) unten).

|Oberes Regal-Bildgröße|Hinweise|
|---|---|
|1920x720px|Statische PNG- oder überlappende .lsr-Datei|

Apple bietet die folgenden Vorschläge für Ihre Top-Shelf-Images erstellen:

- **Umfangreiche statische Bilder verwenden** – Wenn Ihre app keinen dynamischen Inhalt bereitstellt, wird die Top Shelf Image nicht erhalten werden. Verwenden Sie dieses Bild, um die Features der app oder Ihre Marke zu markieren.
- **Link zur App-Inhalte** – dynamische Layouts der Top-Shelf Geben Sie einen Quicklink zu den Inhalt, den Ihre Benutzer in Ihrer app die wichtigsten findet. Verwenden Sie diesen Bereich, um ein quick Link zu Ihrer app zu starten und sofort in den angegebenen Inhalt wechseln.
- **Präsentieren Sie die neuesten Inhalte** – umfassender-Top-Shelf-Inhalte kann zeichnen Sie einen Benutzer in Ihrer app und machen es verwendet werden soll. Verwenden Sie diese als ein Bereich, um die höchsten Bewertung oder neueste Inhalt zu veranschaulichen.
- **Personalisierte Inhalte** – Benutzer vorhanden, die am häufigsten verwendeten, oder bevorzugten apps in der obersten Zeile des Startbildschirms. Verwenden Sie die Top-Shelf, um den Inhalt anzuzeigen, die, dem Sie am meisten interessiert wäre.
- **ADS unzulässig** – Werbeeinblendungen dürfen ausschließlich in der Top-Shelf angezeigt wird. Sie können den neuesten kostenpflichtige Inhalt anzeigen, aber keine Preisinformationen angezeigt werden soll.

### <a name="setting-the-top-shelf-image"></a>Festlegen der Top-Shelf-Bild

Führen Sie zum Festlegen der Top-Shelf-Image für Ihr Projekt TvOS erforderlich sind folgende:

1. In der **Projektmappen-Explorer**, doppelklicken Sie auf `Assets.xcassets` um ihn zur Bearbeitung zu öffnen: 

    [![](icons-images-images/asset01.png "Die Datei Assets.xcassets")](icons-images-images/asset01.png#lightbox)
2. In der **Ressourcen-Editor**, erweitern Sie die `App Icon & Top Shelf Image` Medienobjekt: 

    [![](icons-images-images/asset04.png "Erweitern Sie das Medienobjekt Top Shelf Image")](icons-images-images/asset04.png#lightbox)
3. Klicken Sie auf die `Top Shelf Image` Medienobjekt: 

    [![](icons-images-images/asset07.png "Das Medienobjekt Top Shelf Image")](icons-images-images/asset07.png#lightbox)
5. Klicken Sie auf die **1 x Apple TV-Eintrag** , und wählen Sie eine Bilddatei.
6. Speichern Sie die Änderungen.

<a name="Dynamic-Top-Shelf-Content" />

### <a name="dynamic-top-shelf-content"></a>Dynamische Oberes Regal Inhalt

Anstatt eine statische Top Shelf Image zu verwenden, darf der Top-Shelf eine dynamischen Zeile [Fokussierbare Elemente](~/ios/tvos/app-fundamentals/navigation-focus.md#Focus-and-Selection) oder einen dynamischen Satz von Durchführen eines Bildlaufs Banner. Beide dieser dynamische Format können Sie markieren Sie den Inhalt von Ihrer app oder ein Sprung in die am häufigsten verwendeten Funktionen bereitgestellt.

<a name="Sectioned-Content-Row" />

#### <a name="sectioned-content-row"></a>Schottwänden Inhalt Zeile

Dieser dynamische Top Shelf-Inhaltstyp stellt eine einzelne Zeile des Bildlaufs an, den Fokus erhalten kann Elemente, die optional in Abschnitte unterteilt. Es wird normalerweise verwendet, um neue, Favoriten zu markieren oder die zuletzt angezeigte app-Inhalte.

Der Inhalt wird als einzelne, einem horizontalen Bildlauf Liste von Inhalten mit einer Bezeichnung, die unter der aktuellen Teil des ausgewählten Inhalts dargestellt (die gerade den Fokus besitzt). Wenn der Benutzer einen bestimmten Inhalt auswählt, wird Ihre app wird gestartet, und sie direkt in diese Inhalte entnommen werden soll.

Die folgenden Inhalte Größen werden benötigt:

||Poster (2:3)|Square (1:1)|HDTV (16:9)|
|---|---|---|---|
|Tatsächliche Größe|404x608px|608x608px|908x512px|
|Sichere Zone Größe|380x570px|570x570px|852x479px|
|Ohne Fokus Größe|333x500px|500x500px|782x440px|
|Fokussierte Größe|380x570px|570x570px|852x479px|

Apple bietet die folgenden Vorschläge für die Inhalte geschnitten Zeile:

- **Führen Sie die Zeile** – sollten Sie genug Daten umfassen die gesamte Breite des Bildschirms bereitstellen.
- **Skalieren von Bildern mit gemischten** – die geschnitten Content Zeile wurde entworfen, um eine Mischung aus Image-Größe (in der Liste oben aufgeführten) enthalten. Wenn Sie jedoch Bildgrößen mischen, denken Sie daran, dass zusätzliche Skalierung angewendet wird, um die Inhaltsanzeige zu normalisieren.

<a name="Scrolling-Inset-Banners" />

#### <a name="scrolling-inset-banners"></a>Durchführen eines Bildlaufs Inset Banner

Optional kann Ihre Xamarin.tvOS-app den Inhalt darstellen, in der Top-Shelf als automatisch durchführen eines Bildlaufs und Schleifen Auflistung von Banner, die nahezu den Bildschirm zu füllen. Dieses Format wird normalerweise verwendet, um featurereiche, neue Inhalte wie neue Fernsehsendungen vorzustellen.

Zusätzlich zu den automatischen Bildlauf aus, kann der Benutzer Kontrolle über das Banner und führen Sie einen Bildlauf in beide Richtungen mithilfe von Siri Remote. Sie haben eine kleine, werden den Parallaxeneffekt für dieses Banner zirkuläre Bewegung auf dem Remotecomputer Siri, wenn ein Banner im Fokus ist aktiviert.

**Bannerbild (zusätzliche breit)**

|   |   |
|---|---|
|Tatsächliche Größe|1940x624px|
|Sichere Zone Größe|1740x620px|
|Ohne Fokus Größe|1740x560px|
|Fokussierte Größe|1740x620px|

Durchführen eines Bildlaufs Inset Banner kann entweder bereitgestellt werden als statisch `.png` oder Ebenen `.lsr` Datei.

Apple bietet die folgenden Vorschläge für das Durchführen eines Bildlaufs Inset Banner:

- **Inhalt der Menge** -sollten Sie mindestens drei (3) Banner für den Bildlauf damit können Sie natürliche bereitstellen. Sie sollten nicht mehr als acht (8) Banner einschließen oder erleichtern Navigation schwer für den Endbenutzer.
- **Inhalt der Text** : Wenn Ihr Banner erfordert Text in das Bannerbild eingeschlossen werden soll. Wenn Sie Abbilder mit Ebenen verwendet werden, sollte der Text auf der obersten Ebene sein.

Informieren Sie sich von Apple [TVServices Frameworkverweis](https://developer.apple.com/library/prerelease/tvos/documentation/TVServices/Reference/TVServices_Ref/index.html#//apple_ref/doc/uid/TP40016412) für Weitere Informationen zu Ihrer app zum Bereitstellen von dynamischen Top Shelf-Inhalt eine Top-Shelf-Erweiterung hinzufügen.

<a name="Game-Center-Images" />

## <a name="game-center-images"></a>Gamecenter-Images

Wenn Ihre Xamarin.tvOS-app ein Spiel ist, und Sie Game Center-Unterstützung enthält, werden einige weitere Bildanlagen benötigt:

- **Auszeichnung Symbole** – ein nicht transparenter Image ist erforderlich, damit jede Auszeichnung an, die automatisch in einen Kreis zugeschnitten wird. Leistungen sind nicht den Fokus erhalten kann Elemente vorhanden.
- **Dashboard-Bildmaterial** -bereitgestellt, die am oberen Rand der app-Dashboard im Game Center angezeigt wird, kann ein optionales Bild sein. Diese Images werden nicht erhalten.
- **Leaderboard-Bildmaterial** -Geben Sie zwischen einem (1) auf drei (3) Seitenverhältnis 16:9-Images für die einzelnen Bestenliste anzeigen, die Ihre app unterstützt. Diese können entweder statisch sein `.png` oder Ebenen `.lsr` Dateien. Das Leaderboard-Bildmaterial ist den Fokus erhalten kann.

||Auszeichnung-Symbole|Dashboard-Bildmaterial|Leaderboard-Bildmaterial|
|---|---|---|---|
|Sichtbare Größe|200x200px|923x150px|n/v|
|Tatsächliche Größe|320x320px|n/v|659x371px|
|Sichere Zone Größe|n/v|n/v|618x348px|
|Ohne Fokus Größe|n/v|n/v|548x309px|
|Fokussierte Größe|n/v|n/v|618x348px|

Weitere Informationen zum Arbeiten mit Game Center finden Sie unter Apple [Game Center-Programmierhandbuch](https://developer.apple.com/library/prerelease/tvos/documentation/NetworkingInternet/Conceptual/GameKit_Guide/Introduction/Introduction.html).

<a name="Working-with-Images" />

## <a name="working-with-images"></a>Working with Images (Arbeiten mit Bildern)

Da TvOS 9 eine Teilmenge von iOS 9 ist und Anzeigen von Bildern in einer Xamarin.iOS-app verwendeten Techniken funktionieren Sie auch für eine Xamarin.tvOS-app. Informieren Sie sich unsere [Anzeigen eines Bilds](~/ios/app-fundamentals/images-icons/displaying-an-image.md) Dokumentation zu informieren.

<a name="Setting-Xamarin.tvOS-Project-Images" />

## <a name="setting-xamarintvos-project-images"></a>Festlegen der Bilder mit Xamarin.tvOS-Projekt

Wie bereits erwähnt, erfordern alle TvOS-apps eine [Startbildgruppe](#Launch-Image), und [App-Symbol](#App-Icons). Dieser Abschnitt enthält das Image zu starten und die App-Symbol für Ihre Xamarin.tvOS-app-Projekt auswählen, nachdem sie in einem Ressourcenkatalog festgelegt wurden.

Führen Sie folgende Schritte aus:

1. In der **Projektmappen-Explorer**, doppelklicken Sie auf die `Info.plist` um ihn zur Bearbeitung zu öffnen: 

    [![](icons-images-images/info01.png "Die Datei \"Info.plist\"")](icons-images-images/info01.png#lightbox)
2. In der **Info.Plist-Editor**, wählen Sie den Katalog der Ressourcen (Konfiguration oben in der [Festlegen der App-Symbole](#Setting-the-App-Icons) Abschnitt) für die **-App-Symbole**: 

    [![](icons-images-images/info02.png "Die Datei \"Info.plist\"-Editor")](icons-images-images/info02.png#lightbox)
3. Wählen Sie als Nächstes die Ressourcenkatalogs (Konfiguration oben in der [Festlegen des Bildformats starten](#Setting-the-Launch-Image) Abschnitt) für die **Startbilder**.
4. Speichern Sie die Änderungen.

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden alle imagetypen und Größen, die in einer Xamarin.tvOS-app verwendet behandelt. Es behandelt zunächst Startbilder, Mehrschicht-Images, App-Symbole, Top-Shelf-Images und Game Center-Images. Klicken Sie dann konnte damit die Arbeit mit Bildern in der Xamarin.tvOS-app.

## <a name="related-links"></a>Verwandte Links

- [tvOS-Beispiele](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [TvOS Human Interface-Handbücher](https://developer.apple.com/tvos/human-interface-guidelines/)
- [App-Programmierhandbuch für tvos verwendet.](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
