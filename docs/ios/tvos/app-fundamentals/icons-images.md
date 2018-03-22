---
title: Arbeiten mit Symbolen und Bilder
description: Dieser Artikel umfasst das Entwerfen von und Arbeiten mit Symbolen und Bilder innerhalb einer Xamarin.tvOS-app.
ms.topic: article
ms.prod: xamarin
ms.assetid: A2DA4347-0563-4C72-A8D7-5B9DE9E28712
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 927a77d5671e877e93e5375b61220ac595891179
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/22/2018
---
# <a name="working-with-icons-and-images"></a>Arbeiten mit Symbolen und Bilder

_Dieser Artikel umfasst das Entwerfen von und Arbeiten mit Symbolen und Bilder innerhalb einer Xamarin.tvOS-app._

Erstellen von fesselnden Symbole und Bilder sind ein wichtiger Bestandteil der Entwicklung einer faszinierend benutzerfreundlichkeit für Ihre apps für Apple TV. Dieses Handbuch befasst sich die erforderlichen Schritte zum Erstellen und schließen Sie die Grafiken erforderlichen Ressourcen für Ihre apps Xamarin.tvOS:

- [Starten Sie Image](#Launch-Image) -ein Start-Image wird angezeigt, wenn Ihre app zum ersten Mal gestartet und durch die app ersten Bildschirm ersetzt wird, nach Abschluss der Start.
- [In den Ebenen Bilder](#Layered-Images) – speziell für die Apple TV, Apple neuer Bilder in den Ebenen-Anwendungen mit dem Parallax Effekt einen 3D-Effekt für ausgewählte Elemente zu erstellen. Es gibt mehrere Möglichkeiten, [in den Ebenen-Images erstellen](#Creating-Layered-Images).
- [Symbol "App"](#App-Icons) -Symbole erforderlich sind, nicht nur die Apple TV Home Bildschirm aber für den App Store. Sie müssen als ein Bild in den Ebenen bereitgestellt werden.
- [Top-Regal Image](#Top-Shelf-Image) – Wenn Ihre app auf der obersten Zeile des Home-Bildschirms platziert wird, benötigen sie ein oben Regal-Bild für Ihre app Funktionen markieren. Optional können Sie angeben [dynamische oben Regal Inhalte](#Dynamic-Top-Shelf-Content) , markieren Sie den Inhalt in Ihrer app.
- [Game Center-Images](#Game-Center-Images) – Wenn Ihre Anwendung ein Spiel ist und Game Center verwendet, werden mehrere zusätzliche Bilder erforderlich sein.
- [Festlegen von Xamarin.tvOS Projekt Bilder](#Setting-Xamarin.tvOS-Project-Images) -umfasst die erforderlichen Schritte für das Bild zu starten und das Symbol "App" für Ihre app Xamarin.tvOS festgelegt.

> [!IMPORTANT]
> Alle Images auf dem Apple TV sind in der 1 X-Auflösung (`@1x`) sollten _nur_ Bilder dieser Größe verwenden. Größere einschließlich höherer Auflösung Grafiken nicht nur Zeit benötigt herunterzuladen und mehr Arbeitsspeicher und Speicher, aber sie müssen zur Laufzeit dynamisch skaliert werden und wirkt sich negativ auf die Zeichnung Leistung.

<a name="Launch-Image" />

## <a name="launch-image"></a>Bild zu starten

Das Bild starten erfolgt als erstes, der angezeigt wird, wenn Ihre app Xamarin.tvOS anfänglich auf den Apple TV gestartet wird, und daher muss jede app tvos. außerdem wurden ein Image starten angeben. 

Das Bild starten schnell angezeigt und vermittelt den Eindruck, dass die app schnell und reaktionsfähig ist. Die Apple TV ersetzt der starten-Image mit dem ersten Bildschirm Ihrer App in Kürze es nach.

Start-Images sind keine Gelegenheit für Werbung oder künstlerischen Ausdruck, sie dienen lediglich der Eindruck entstehen, dass Ihre app schnell gestartet und kann jetzt verwendet.

|Starten der Bildgröße|Hinweise|
|---|---|
|1920x1080px|Non-layered .png files only|

Apple macht die folgenden Vorschläge zum Starten der app-Image entwerfen:

- **Mit dem ersten Bildschirm fast identisch** -Ihre starten Bildschirm sollte so nahe bei Ihrer app ersten Bildschirm wie möglich sein. Z. B. verschiedene Grafiken oder Element kann in eine störende führen "flash", wenn der erste Bildschirm angezeigt wird.
- **Vermeiden Sie Text mithilfe von** -starten Sie Bilder sind statisch und wird daher nicht vor seiner Anzeige lokalisiert werden.
- **Liegen, sondern verstärkt starten** -da Apple TV-Benutzern häufig apps wechseln, sollten nicht an den Prozess der app-Start Aufmerksamkeit.
- **Keine Werbung oder Branding** -Your-starten-Bild sollte nicht verwendet werden, wie ein Bildschirm "Info" oder umfassen alle branding, es sei denn, sie statische Teil Ihrer app ersten Bildschirm ist. Werbeeinblendungen sind unzulässig.

<a name="Setting-the-Launch-Image" />

### <a name="setting-the-launch-image"></a>Festlegen der starten-Bild

Führen Sie zum Festlegen der starten-Image für das Projekt tvos. außerdem wurden die folgenden:

1. In der **Projektmappen-Explorer**, doppelklicken Sie auf `Assets.xcassets` um ihn zur Bearbeitung zu öffnen: 

    [![](icons-images-images/asset01.png "Die Datei Assets.xcassets")](icons-images-images/asset01.png#lightbox)
2. In der **Asset Editor**, klicken Sie auf der `LaunchImages` Asset: 

    [![](icons-images-images/asset02.png "Das Medienobjekt LaunchImages")](icons-images-images/asset02.png#lightbox)
3. Klicken Sie auf die **1 x Apple TV** Eintrag, und wählen Sie das Abbild zu starten, oder ziehen Sie optional ein neues Image in aus dem Dateisystem: 

    [![](icons-images-images/asset03.png "Wählen Sie ein Image starten")](icons-images-images/asset03.png#lightbox)
4. Speichern Sie die Änderungen.

<a name="Layered-Images" />

## <a name="layered-images"></a>Bilder mit Ebenen

Noch nicht mit der Apple-TV-Images in den Ebenen arbeiten mit den Effekt Parallax einen 3D-Effekt zu erzeugen, der den Benutzer auf die Couch auf über den Raum auf den Inhalt auf dem Bildschirm verbunden bleiben.

Überlappende Bilder enthalten aus zwei (2) auf fünf (5) Trennen von Ebenen, die kombiniert werden, um ein Abschließen des Images zu bilden. Mit Ausnahme der Hintergrundebene verwendet jeder Ebene die Z-Reihenfolge zusammen mit Transparenz, um eine Illusion von Tiefe zu erstellen. Wenn der Benutzer mit einem Abbild in den Ebenen interagiert, werden höhere Z sortiert Ebenen skaliert und den überlappenden um diesen Effekt zu erstellen.

[![](icons-images-images/layered01.png "Überlappende Bilder Z sortiert-Diagramm")](icons-images-images/layered01.png#lightbox)

> [!IMPORTANT]
> Bilder mit Ebenen für Ihre app Symbole erforderlich sind, und sind optional, für andere [den Fokus erhalten kann Elemente](~/ios/tvos/app-fundamentals/navigation-focus.md#Focus-and-Selection) (z. B. dem Anfang Regal-Image). Allerdings wird Apple vorgeschlagen, die Verwendung von Images in den Ebenen für jedes Bild, das Fokus in der app abrufen kann.




Apple macht die folgenden Vorschläge zum Entwerfen von Bildern in den Ebenen:

- **Stellen Sie die nicht transparenten Hintergrund-Ebene** -Ihre Hintergrundebene (Ebene 1) **müssen** nicht transparent sein, und Sie erhalten einen Fehler auf, wenn Sie versuchen, verwenden Sie das Bild in den Ebenen auf Apple TV. Alle anderen Ebenen können mehrere Ebenen Grad an Transparenz für die 3D-Effekt verbessern enthalten.
- **Zu isolieren, Vordergrund, mittleren und Elemente im Hintergrund** -gut sichtbaren Elemente (z. B. Spiel Zeichen) im Vordergrund platzieren, verwenden Sie die Mitte für sekundäre Elemente oder Schatten. Schließlich enthalten Sie einen neutralen Hintergrund aus, um eine Stufe für die oberen Ebenen bereitzustellen.
- **Beibehalten von Text im Vordergrund** – es sei denn, Ihr Text von höheren Ebenen verdeckt werden soll, im Allgemeinen sollten sie auf der obersten Ebene werden.
- **Verwenden, einfacher Schichten** -der Parallax Effekt wurde entworfen, um feine werden daher Ebenen mit minimalen zu berücksichtigen, unrealistisch Auswirkungen vermieden werden beibehalten.
- **Eine sichere Zone umfassen** -da obere Schichten während eines Effekts Parallax abgeschnitten werden können, müssen Sie einen Rahmen für die sichere Zone in jeder Ebene zu erstellen. Wenn Sie Ihre Inhalte zu nah am Rand von Ebenen angezeigt werden, kann es abgeschnitten werden. Obere Ebenen treten weitere Skalierung und als unteren Schichten zuschneiden. Finden Sie unter der [Größenanpassung Imageebenen](#Sizing-Image-Layers) Abschnitt weiter unten.
- **In der Vorschau anzeigen häufig** -Images in den Ebenen sollten werden in der Vorschau angezeigt häufig, um sicherzustellen, dass die gewünschte 3D-Effekt tritt auf, und keine des Inhalts auf den einzelnen Ebenen zugeschnitten wird, wird. Führen Sie eine Vorschau der Bilder in den Ebenen auf echter Hardware für Apple TV, stellen Sie sicher, dass sie erwartungsgemäß gerendert.

Wann immer möglich, verwenden Sie immer die integrierte `UIKit` Steuerelementen zum Anzeigen der Bilder in den Ebenen, wie Benutzer automatisch die Auswirkungen Parallax erhalten, wenn sie den Fokus eintreffen.

<a name="Sizing-Image-Layers" />

### <a name="sizing-image-layers"></a>Sizing Imageebenen

Es ist wichtig, denken Sie daran, eine _Safe Zone_ Rahmen in jeder Ebene, die das Bild in den Ebenen erstellen. Da die einzelnen Ebenen skaliert und während der Effekt Parallax zugeschnitten werden können, kann der Inhalt der Ebenen zugeschnitten ist nah an den Rand der Ebene:

[![](icons-images-images/layered02.png "35 Pixel Rahmen")](icons-images-images/layered02.png#lightbox)

<a name="Creating-Layered-Images" />

### <a name="creating-layered-images"></a>Erstellen von Images in den Ebenen

tvos. außerdem wurden funktioniert mit den Ebenen-Images in den folgenden Formaten:

- **Auto-Dateien** -Dies ist ein proprietäres Asset-Katalog-Format erstellt, die von Apple. Auto-Dateien wird nicht direkt erstellen, sie sind aus LSR Dateien zum Zeitpunkt der Kompilierung erstellt und in Ihrer app-Bundles eingeschlossen.
- **LSR Bilder** -Dies ist eine proprietäre Bildformat erstellt, die von Apple. Verwenden der [Parallax Exportprogramm Adobe Photoshop-Plug-Ins](https://itunespartner.apple.com/assets/downloads/ParallaxExporter_Apps.zip) oder [Parallax-Vorschau](http://itunespartner.apple.com/assets/downloads/Parallax%20Previewer.dmg) erstellen und Bilder in den Ebenen in der Vorschau anzeigen, im Format LSR.
- **Assets.xcassets** – aus zwei (2) auf fünf (5) standard `.png` formatierte bildkomprimierung in eine Asset-Katalog, die in einer Auto oder LSR kompiliert werden überlappende Bild zum Zeitpunkt der Kompilierung formatiert.
- **LCR Dateien** -Dies ist ein proprietäres Dateiformat, das von Apple erstellt. LCR-Dateien dienen als zusätzliche aus einem der Inhaltsserver heruntergeladene Inhalt verwendet werden soll. LCR-Datei sollte nie in Ihrer app-Bündel enthalten sein. LCR-Dateien werden generiert, von LSR oder Photoshop-Dateien über die `layerutil` -Befehlszeilentool mit Xcode enthalten.

<a name="The-Parallax-Previewer" />

### <a name="the-parallax-previewer"></a>Die Parallax-Vorschau

Apple erstellt die [Parallax-Vorschau](http://itunespartner.apple.com/assets/downloads/Parallax%20Previewer.dmg) zu Vorschau- und erstellte überlappende Bilder für App-Symbole und optional den Fokus erhalten kann Elemente erforderlich sind. Die Vorschau zeigt die jeder Ebene, die den abgeschlossenen Images in den Ebenen bildet:

[![](icons-images-images/layered03.png "Die Parallax-Vorschau")](icons-images-images/layered03.png#lightbox)

Während der Vorschau auf ein Bild in den Ebenen, können Sie die Maus verwenden, drehen das Bild und eine Vorschau der Parallax wirksam. Verwenden der  **+**  (plus) und  **-**  (Minuszeichen) hinzufügen und Entfernen von Ebenen.

Wenn Sie ein neues Image mit den Ebenen zu erstellen, können Sie in der LSR-Format exportiert und in Ihrer app-Bundles eingeschlossen werden.

Weitere Informationen zum Erstellen und Anzeigen einer Vorschau in den Ebenen-Images finden Sie unter der Apple- [erstellen Parallax Bildmaterial](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/CreatingParallaxArtwork.html#//apple_ref/doc/uid/TP40015241-CH19-SW1) Teil der [App Programmierhandbuch für tvos. außerdem wurden](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/).

<a name="App-Icons" />

## <a name="app-icons"></a>App-Symbole

Ihre app Xamarin.tvOS müssen nicht nur ein App-Symbol für die Apple TV-Startseite, sondern auch ein Symbol für den App Store. Das Symbol "App" wird die Ihres ersten ändern, um eine hervorragende Eindruck für Ihre potenziellen Benutzer und sollte Ihre app Zweck auf einen Blick vermitteln.

[![](icons-images-images/icon01.png "Das Symbol "App"")](icons-images-images/icon01.png#lightbox)

Alle Apps muss eine kleine und eine große Version von dessen Symbol "App" angeben. Des kleinen Symbols wird auf dem Bildschirm Apple TV-Startseite verwendet werden, wenn die app installiert wird. Die große Version wird von dem App Store. "Große Symbole" App sollte imitieren, die das Aussehen und Verhalten von der Version des kleinen Symbols an.

|Symbol "Small"||"Große Symbole"||
|---|---|---|---|
|Tatsächliche Größe|400x240px|Größe|1280x768px|
|Größe der sicheren Zone|370x222px|||
|Ohne Fokus Größe|300x180px|||
|Mit Fokus Größe|370x222px|||

> [!IMPORTANT]
> Ihre App-Symbole muss angegeben werden, als **Bilder in den Ebenen**. Finden Sie unter der [Bild in den Ebenen](#Layered-Images) im Abschnitt oben für weitere Details.




Apple bietet die folgenden Vorschläge zum Erstellen Ihrer App-Symbole:

- **Bereitstellen eines einzelnen Fokus Punkts** – Entwurf Ihrer Symbol mit einem einzelnen Fokuspunkt platziert wird, direkt in der Mitte des Symbols.
- **Verwenden Sie einen einfachen Hintergrund** – Ihre Symbolhintergrund jedoch auch einfach halten, damit die oberen Schichten hervorzuheben. Erwägen Sie eine einfache Farbe oder geringfügige Farbverlauf.
- **Begrenzen der Menge Text** –, da die app-Namen unterhalb des Symbols angezeigt wird, wenn es vom Benutzer ausgewählt wird, Sie sollten nur Text beim einschließen ist entscheidend dafür, dass der Entwurf des Symbols.
- **Verwenden Sie keine Screenshots** – Screenshots für ein Symbol zu komplex sind und nicht dem Benutzer ermöglichen, den Zweck der app auf einen Blick finden Sie unter.
- **Keep Symbole Quadrat** – tvos. außerdem wurden automatisch eine Maske, die die Ecken der Symbole leicht rundet angewendet. Schließen Sie nicht durch diese Rundung selbst.
- **Trennen Sie sorgfältig Ihre Ebenen** – Text muss auf der oberen Ebene die meisten, sekundären Elemente in der Mitte und eine neutrale Hintergrund aus, die die oberen Ebenen scheinen ermöglicht.
- **Verwenden, Farbverläufe und Schatten sorgfältig** – Farbverläufe und Schatten können miteinander in Konflikt geraten mit dem Effekt Parallax sollte sorgfältig verwendet werden. Einfache oben-nach-unten, funktionieren am besten Hell-Dunkel-Farbverlauf Stile. Zeichnen von Schatten verwendet arbeiten normalerweise am besten als Spitze, die mit harten Kanten Farbtöne.
- **Verändern Sie die Transparenz** – verwenden unterschiedliche Zugriffsebenen Transparenz für die oberen Ebenen des Ihrer App-Symbol, um den 3D-Effekt zu erhöhen. Die Hintergrundebene muss nicht transparent sein oder zu einem Fehler führt.

<a name="Setting-the-App-Icons" />

### <a name="setting-the-app-icons"></a>Festlegen von App-Symbole

Führen Sie zum Festlegen von App-Symbole, die erforderlich sind, für das Projekt tvos. außerdem wurden die folgenden:

1. In der **Projektmappen-Explorer**, doppelklicken Sie auf `Assets.xcassets` um ihn zur Bearbeitung zu öffnen: 

    [![](icons-images-images/asset01.png "Die Assets.xcassets fileg")](icons-images-images/asset01.png#lightbox)
2. In der **Asset Editor**, erweitern Sie die `App Icon & Top Shelf Image` Asset: 

    [![](icons-images-images/asset04.png "Erweitern Sie den oberen Regal-Standardimage-Medienobjekt")](icons-images-images/asset04.png#lightbox)
3. Erweitern Sie als Nächstes die `App Icon - Small` Asset: 

    [![](icons-images-images/asset05.png "Erweitern Sie das App-Symbol - kleine asset")](icons-images-images/asset05.png#lightbox)
4. Erweitern Sie dann die `Back` Bestand, und klicken Sie auf die `Contents` Eintrag: 

    [![](icons-images-images/asset06.png "Erweitern Sie dann das Back-Medienobjekt")](icons-images-images/asset06.png#lightbox)
5. Klicken Sie auf die **1 x Apple TV-Eintrag** , und wählen Sie eine Bilddatei.
6. Wiederholen Sie die oben genannten Schritte für die `Front` und `Middle` Bestand.
7. Wiederholen Sie dann die gleichen Schritte zum Definieren der `App Icon - Large` Asset.
4. Speichern Sie die Änderungen.

<a name="Top-Shelf-Image" />

## <a name="top-shelf-image"></a>Oberes Regal-Bild

Wenn der Benutzer die app Xamarin.tvOS auf die Zeile nach oben auf dem Bildschirm Apple TV-Startseite aufgegeben hat, wird ein großes oben Regal Bild angezeigt, wenn es sich bei Ihrer app vom Benutzer ausgewählt wird. Dieses Bild sollte markieren Sie die Funktionen der app, oder geben Sie direkte Links zum jeweiligen Inhalt.

[![](icons-images-images/topshelf01.png "Beispiel mit Top Regal-Bild")](icons-images-images/topshelf01.png#lightbox)

Die oberen Regal Image kann entweder als eine einzelne statische bereitgestellt `.png` oder `.lsr` Datei (finden Sie unter [in den Ebenen-Images erstellen](#Creating-Layered-Images)) oder erstellt werden, dynamisch zur Laufzeit als eine einzelne Zeile mit den Fokus erhalten kann Elemente (finden Sie unter [ Dynamische Oberes Regal Inhalt](#Dynamic-Top-Shelf-Content) unten).

|Oberes Regal Bildgröße|Hinweise|
|---|---|
|1920x720px|Statische .png "oder" geschichteten .lsr-Datei|

Apple bietet die folgenden Vorschläge zum Erstellen der Top-Regal Bilder an:

- **Umfangreiche statische Bilder verwenden** – Wenn Ihre app nicht mit einen dynamischen Inhalt erbringt, Top Regal Bild werden nicht den Fokus erhalten kann. Verwenden Sie dieses Bild, um die Funktionen von der app oder Ihre Marke zu markieren.
- **Link zur App-Inhalte** – dynamische oben Regal Layouts bieten einen schnellen Link zum Inhalt, die Ihre Benutzer in Ihrer app die wichtigsten findet. Verwenden Sie diesen Bereich, um einen schnellen Link, um Ihre app starten und sofort Einsprung in den angegebenen Inhalt bereitzustellen.
- **Präsentieren Sie die neuesten Inhalte** – Rich oben Regal Inhalt kann einen Benutzer in Ihrer app zu zeichnen und machen sie weitere verwenden möchten. Verwenden Sie diese als einen Bereich zur Veranschaulichung des höchste bewerteten oder der neuesten Inhalts.
- **Personalisierten Inhalt** – Benutzer ihren am häufigsten verwendeten oder den bevorzugten apps in der oberen Zeile der Startseite angezeigt. Verwenden Sie im oberen Regal, um den Inhalt anzuzeigen, dem sie besonders interessant an wäre.
- **ADS unzulässig** – Werbeeinblendungen sind unzulässig im Regal oben angezeigt werden. Sie können die neuesten käuflichen Inhalt angezeigt werden, allerdings keine Preisinformationen angezeigt werden soll.

### <a name="setting-the-top-shelf-image"></a>Das Bild Oberes Regal festlegen

Führen Sie zum Festlegen der oberen Regal Image erforderlich für das Projekt tvos. außerdem wurden die folgenden:

1. In der **Projektmappen-Explorer**, doppelklicken Sie auf `Assets.xcassets` um ihn zur Bearbeitung zu öffnen: 

    [![](icons-images-images/asset01.png "Die Datei Assets.xcassets")](icons-images-images/asset01.png#lightbox)
2. In der **Asset Editor**, erweitern Sie die `App Icon & Top Shelf Image` Asset: 

    [![](icons-images-images/asset04.png "Erweitern Sie den oberen Regal-Standardimage-Medienobjekt")](icons-images-images/asset04.png#lightbox)
3. Klicken Sie auf der `Top Shelf Image` Asset: 

    [![](icons-images-images/asset07.png "Die Top-Regal-Standardimage-Medienobjekt")](icons-images-images/asset07.png#lightbox)
5. Klicken Sie auf die **1 x Apple TV-Eintrag** , und wählen Sie eine Bilddatei.
6. Speichern Sie die Änderungen.

<a name="Dynamic-Top-Shelf-Content" />

### <a name="dynamic-top-shelf-content"></a>Dynamische Oberes Regal Inhalt

Anstatt ein statisches Bild der Top-Regal darf die Top-Regal eine dynamische Zeile des [den Fokus erhalten kann Elemente](~/ios/tvos/app-fundamentals/navigation-focus.md#Focus-and-Selection) oder einen dynamischen Satz von Durchführen eines Bildlaufs Banner. Sowohl diese dynamische Format können Sie markieren Sie den Inhalt von Ihrer app oder ein Sprung in die am häufigsten verwendeten Funktionen bereitgestellt.

<a name="Sectioned-Content-Row" />

#### <a name="sectioned-content-row"></a>Unterteilten Inhalt Zeile

Diese dynamische oben Regal Inhaltstyp stellt eine einzelne Zeile mit scrollen, den Fokus erhalten kann Elemente, die optional in Abschnitte unterteilt. Es dient normalerweise zum neuen Favoriten markieren oder vor kurzem angezeigte app-Inhalte.

Inhalt wird als einzelne, einem horizontalen Bildlauf Liste von Inhalten mit einer Bezeichnung, die unterhalb der aktuellen Inhaltselement ausgewählt angezeigt (die gerade den Fokus besitzt). Wenn der Benutzer einen bestimmten Inhalt auswählt, Ihre app wird gestartet, und sie direkt in dieses Inhalts ergriffen werden sollten.

Die folgenden Inhaltsgrößen werden benötigt:

||Poster (2:3)|Square (1:1)|HDTV (16:9)|
|---|---|---|---|
|Tatsächliche Größe|404x608px|608x608px|908x512px|
|Größe der sicheren Zone|380x570px|570x570px|852x479px|
|Ohne Fokus Größe|333x500px|500x500px|782x440px|
|Mit Fokus Größe|380x570px|570x570px|852x479px|

Apple bietet die folgenden Vorschläge für die Inhalte geschnitten Zeile:

- **Führen Sie die Zeile** – sollten Sie genügend Inhalte, die die gesamte Breite des Bildschirms umfassen bereitstellen.
- **Skalieren von Bildern gemischten** – die geschnitten Content Zeile wurde entworfen, um eine Mischung aus Bildgrößen (aus der oben angezeigten Liste) enthalten. Wenn Sie jedoch Bildgrößen mischen, Bedenken Sie darauf, dass zusätzliche Skalierung angewendet werden, um die Inhaltsanzeige normalisieren.

<a name="Scrolling-Inset-Banners" />

#### <a name="scrolling-inset-banners"></a>Durchführen eines Bildlaufs Inset Banner

Optional kann Ihre app Xamarin.tvOS in einem Regal oben als automatisch durchführen eines Bildlaufs und Auflistung von Banner, die nahezu Bildschirmgröße Schleifen seinen Inhalt dargestellt. Dieses Format wird normalerweise verwendet, um umfangreiche, wird neuer Inhalt wie neue Fernsehsendungen veranschaulichen.

Zusätzlich zu den automatischen Bildlauf kann der Benutzer die Kontrolle über das Banner und führen Sie einen Bildlauf in beide Richtungen mithilfe von Siri Remote. Machen eine kleine, wird die zirkuläre Geste auf der Remoteinstanz Siri, wenn ein Banner besteht aus den Fokus der Parallax-Effekt für diese Banner aktiviert.

**Banner-Image (Extra breit)**

|   |   |
|---|---|
|Tatsächliche Größe|1940x624px|
|Größe der sicheren Zone|1740x620px|
|Ohne Fokus Größe|1740x560px|
|Mit Fokus Größe|1740x620px|

Durchführen eines Bildlaufs Inset Banner kann entweder bereitgestellt werden, als statisch `.png` oder überlappende `.lsr` Datei.

Apple bietet die folgenden Vorschläge für das Durchführen eines Bildlaufs Inset Banner:

- **Inhalts-Menge** -sollten Sie mindestens drei (3) Banner für den Bildlauf um natürliche gerne bereitstellen. Sie sollte nicht mehr als acht (8) Banner enthalten oder erleichtern Navigation Festplatte für den Endbenutzer.
- **Inhalts-Text** – Wenn Ihr Banner erfordert Text in das Banner Image eingeschlossen werden soll. Bei Verwendung von Images in den Ebenen sollte der Text auf der obersten Ebene sein.

Finden Sie in der Apple- [TVServices Frameworkverweis](https://developer.apple.com/library/prerelease/tvos/documentation/TVServices/Reference/TVServices_Ref/index.html#//apple_ref/doc/uid/TP40016412) Weitere Informationen zum Hinzufügen von sinnvolles Regal nach oben zu Ihrer app dynamische oben Regal Inhalte bereit.

<a name="Game-Center-Images" />

## <a name="game-center-images"></a>Game Center-Images

Wenn Ihre app Xamarin.tvOS ein Spiel ist, und Sie haben Game Center-Unterstützung enthalten, werden einige weitere Bildanlagen benötigt:

- **Erfolg Symbole** – ein nicht transparenter Image ist erforderlich, damit jede Leistung, die automatisch in einen Kreis zugeschnitten wird. Erfolge werden Elemente nicht den Fokus erhalten kann.
- **Dashboard-Bildmaterial** – ein optionales Bild bereitgestellt, die am oberen Rand der app-Dashboard in Game Center angezeigt werden. Diese Images stellen nicht den Fokus erhalten kann.
- **Leaderboard Bildmaterial** -Geben Sie zwischen eins (1) auf drei (3) 16:9-Seitenverhältnis Bilder für jede Leaderboard, die Ihrer app unterstützt werden. Dies ist möglicherweise entweder eine statische `.png` oder überlappende `.lsr` Dateien. Grafik Leaderboard ist den Fokus erhalten kann.

||Leistung-Symbole|Dashboard-Bildmaterial|Leaderboard-Bildmaterial|
|---|---|---|---|
|Sichtbare Größe|200x200px|923x150px|n/v|
|Tatsächliche Größe|320x320px|n/v|659x371px|
|Größe der sicheren Zone|n/v|n/v|618x348px|
|Ohne Fokus Größe|n/v|n/v|548x309px|
|Mit Fokus Größe|n/v|n/v|618x348px|

Weitere Informationen zum Arbeiten mit Game Center finden Sie im Apple [Game Center-Programmierhandbuch](https://developer.apple.com/library/prerelease/tvos/documentation/NetworkingInternet/Conceptual/GameKit_Guide/Introduction/Introduction.html).

<a name="Working-with-Images" />

## <a name="working-with-images"></a>Working with Images (Arbeiten mit Bildern)

Da tvos. außerdem wurden 9 eine Teilmenge von iOS 9 ist und Anzeigen von Bildern in einer app Xamarin.iOS verwendeten Techniken funktionieren Sie auch für eine Xamarin.tvOS-app. Wenden Sie sich unsere [Anzeigen eines Bilds](~/ios/app-fundamentals/images-icons/displaying-an-image.md) Dokumentation weitere Informationen.

<a name="Setting-Xamarin.tvOS-Project-Images" />

## <a name="setting-xamarintvos-project-images"></a>Festlegen der Bilder Xamarin.tvOS-Projekt

Wie bereits erwähnt, müssen alle apps für tvos. außerdem wurden ein [starten Image](#Launch-Image), und [Symbol "App"](#App-Icons). Dieser Abschnitt enthält die für Ihr Xamarin.tvOS-app-Projekt das Bild zu starten und das Symbol "App" auswählen, nachdem sie in eine Asset-Katalog festgelegt wurden.

Führen Sie folgende Schritte aus:

1. In der **Projektmappen-Explorer**, doppelklicken Sie auf die `Info.plist` um ihn zur Bearbeitung zu öffnen: 

    [![](icons-images-images/info01.png "Die Datei "Info.plist"")](icons-images-images/info01.png#lightbox)
2. In der **"Info.plist" Editor**, wählen Sie den Katalog Bestand (konfigurierten oben in der [Festlegen von App-Symbole](#Setting-the-App-Icons) Abschnitt) für die **App-Symbole**: 

    [![](icons-images-images/info02.png "Die Datei "Info.plist"-Editor")](icons-images-images/info02.png#lightbox)
3. Wählen Sie als Nächstes den Bestand-Katalog (konfigurierten oben in der [Einstellen des Bildes starten](#Setting-the-Launch-Image) Abschnitt) für die **starten Bilder**.
4. Speichern Sie die Änderungen.

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden alle Bildtypen und in einer app Xamarin.tvOS verwendeten Volumegrößen behandelt. Es behandelt zuerst starten Bilder, Bilder mit Ebenen, App-Symbole, Top Regal Bilder und Game Center-Images. Klicken Sie dann behandelt sie arbeiten mit Bildern in Ihrer app Xamarin.tvOS.

## <a name="related-links"></a>Verwandte Links

- [tvOS-Beispiele](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvos. außerdem wurden Handbücher für interaktive Workflowdienste-Schnittstelle](https://developer.apple.com/tvos/human-interface-guidelines/)
- [App-Programmierhandbuch für tvos. außerdem wurden](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
