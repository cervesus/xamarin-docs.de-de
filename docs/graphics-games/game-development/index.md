---
title: Einführung in die Spieleentwicklung mit xamarin
description: Dieses Dokument enthält eine allgemeine Übersicht über die Spieleentwicklung mit xamarin, die beschreibt, wie Spiele erstellt werden, und eine Stichprobe der Technologien, die für die Verwendung mit xamarin. IOS und xamarin. Android verfügbar sind.
ms.prod: xamarin
ms.assetid: 0E3CDCD2-FBE4-49F5-A70E-8A7B937BAF1D
author: conceptdev
ms.author: crdun
ms.date: 03/24/2017
ms.openlocfilehash: 5992e8df3080bb35fd123483e5ffb5e64f268b1a
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2020
ms.locfileid: "76724759"
---
# <a name="introduction-to-game-development-with-xamarin"></a>Einführung in die Spieleentwicklung mit xamarin

Das Entwickeln von spielen kann sehr spannend sein, vor allem, wie einfach es ist, ihre Arbeit auf mobilen Plattformen zu veröffentlichen. In diesem Artikel werden Konzepte und Technologien im Zusammenhang mit der Spieleentwicklung erläutert, die Ihnen dabei helfen, Spiele zu erstellen, ganz gleich, ob Sie ein qualitativ hochwertiges AAA-Spiel erstellen oder einfach programmieren möchten.

In diesem Artikel werden die folgenden Themen behandelt:

- **Konzepte für Spiele und nicht-Spielprogrammierung** – wir untersuchen einige Konzepte, die für die Spieleentwicklung spezifisch sind oder mit anderen Entwicklungs Typen gemeinsam genutzt werden, aber aufgrund ihrer Wichtigkeit den Schwerpunkt erhalten.
- **Spiele Entwicklungsteam** – dieser Abschnitt befasst sich mit den verschiedenen Rollen in einem Team von Spiel Entwicklern.
- **Erstellen einer Spiel Idee** – dieser Abschnitt unterstützt Sie beim Erstellen einer neuen Spiel Idee – dem ersten Schritt bei der Erstellung eines neuen Spiels.
- **Spiele Entwicklungstechnologie** – hier werden einige der verfügbaren plattformübergreifenden Technologien aufgelistet, die Ihre Produktivität als Spielentwickler verbessern können.

## <a name="game-vs-non-game-programming-concepts"></a>Konzepte von Spielen im Vergleich zu nicht Spiel Programmen

Programmierer, die sich in die Spieleentwicklung verlagern, werden häufig mit neuen Konzepten und Entwicklungsmustern konfrontiert. Dieser Abschnitt enthält eine allgemeine Übersicht über einige dieser Konzepte.

### <a name="the-game-loop"></a>Die Game-Schleife

Ein typisches Spiel erfordert ein konstantes verschieben oder ändern, das als Reaktion auf Benutzerinteraktion und automatische Spiellogik auf dem Bildschirm stattfindet. Dies wird durch das, was in der Regel als *Spiel Schleife*bezeichnet wird, erreicht. Eine Game-Schleife ist eine Art von Schleifen Anweisung (z. b. eine while-Schleife), die mit sehr hoher Häufigkeit (z. b. 30 oder 60 *Frames pro Sekunde*) ausgeführt wird.

Im folgenden finden Sie ein Diagramm einer einfachen Spiel Schleife:

![](images/image1.png "This is a diagram of a simple game loop")

Die im folgenden erläuterten Technologien abstrahieren die tatsächliche while-Schleife, aber trotz dieser Abstraktion ist das Konzept der Updates für jedes Frame vorhanden.

Die Code Leistung kann auch in den einfachsten spielen Priorität haben. Beispiel: eine Funktion, die für die Ausführung 10 Millisekunden benötigt, könnte eine erhebliche Auswirkung auf die Leistung eines Spiels haben – insbesondere, wenn Sie mehr als einmal pro Frame aufgerufen wird. Wenn das Spiel bei 30 Frames pro Sekunde ausgeführt wird, bedeutet dies, dass jeder Frame in unter 33 Millisekunden ausgeführt werden muss. Im Gegensatz dazu ist eine solche Funktion möglicherweise nicht einmal bemerkbar, wenn Sie nur als Reaktion auf einen Klick auf eine Schaltfläche in einer Anwendung ausgeführt wird, die keine Spiele Anwendung ist.

Zu den gängigen Logik Typen, die möglicherweise für jeden Frame ausgeführt werden, gehören:

- **Lesen der Eingabe** – das Spiel muss möglicherweise überprüfen, ob der Benutzer mit dem Spiel interagiert hat, indem er die Eingabe Hardware (z. b. den Touchbildschirm, die Tastatur, die Maus oder den Spielcontroller) überprüft.
- **Bewegung** – Objekte, die von einem Ort zu einem anderen verschoben werden, verschieben in der Regel einen sehr kleinen Anteil jedes Frames, um die Illusion von fließendem Bewegungs Aufwand zu erwecken.
- **Kollision** – viele Spiele erfordern das häufige testen, ob verschiedene Objekte sich überlappen oder sich überschneiden. Der Konflikt wird in einem späteren Abschnitt in diesem Artikel ausführlicher behandelt. Bewegung und Kollision können von einem dedizierten Physik Simulationssystem behandelt werden.
- Die **Überprüfung auf Spiel spezifische Bedingungen** – der Status eines Spiels kann durch bestimmte Bedingungen gesteuert werden, z. b. ob der Spieler genügend Punkte verdient hat oder ob die zugewiesene Zeit abgelaufen ist.
- **AI-Verhalten** – jede Frame-Logik, die verwendet werden kann, um das Verhalten von Objekten zu steuern, die nicht vom Player gesteuert werden, wie z. b. das patken eines Feindes oder die Bewegung von Gegner Treibern um eine Rennstrecke.
- **Rendering** – die meisten Spiele aktualisieren, was auf dem Bildschirm jedes Frame angezeigt wird. Dies kann als Reaktion auf Änderungen erfolgen, die Auswirkungen auf Spiel Spiele haben (z. b. ein Zeichen, das sich durch eine Ebene bewegt), oder um visuelle Politur (z. b. Schnee-oder animierte Symbole) bereitzustellen.

Beachten Sie, dass viele der oben aufgeführten Aktivitäten den Zustand der gesamten Anwendung ändern können, während viele nicht-Spiel-apps tendenziell den Status als Reaktion auf Ereignisse ändern, die ausgelöst werden.

### <a name="content-loading-and-unloading"></a>Laden und Entladen von Inhalten

Das manuelle Laden und Entladen (oder verwerfen) von Inhalten ist abhängig von der in der Entwicklung verwendeten Technologie möglicherweise erforderlich. Das manuelle Laden und Entladen von Assets kann aus verschiedenen Gründen erforderlich sein:

- Das Laden von Assets in Relation zur Länge eines einzelnen Frames kann viel Zeit in Anspruch nehmen. Es kann sogar Sekunden dauern, bis einige Ressourcen geladen sind. Dies würde die Leistung bei der geladenen Mitte des Spiels erheblich stören. Wenn die Ladezeit besonders lange dauert (z. b. mehr als ein Sekunde), können Sie einen animierten Ladebildschirm oder eine Statusanzeige anzeigen.
- Assets können viel RAM beanspruchen, sodass eine aktive Verwaltung der Inhalte erforderlich ist, die in die von den Zielplattformen des Spiels bereitgestellten Funktionen integriert werden.
- Spiele müssen möglicherweise mehr Ressourcen anzeigen, als in den RAM passen. "Offene Welt" Spiele enthalten häufig große Umgebungen, in denen Spieler nahtlos navigieren können – ohne Ladebildschirm. In diesem Fall müssen Sie möglicherweise ein benutzerdefiniertes System zum Streamen von Inhalten in erstellen und die Speicherauslastung verwalten.

Benutzerdefinierte Dateiformate müssen möglicherweise zur Ladezeit verarbeitet werden, sodass benutzerdefinierter Lade Code erforderlich ist.

### <a name="math"></a>Mathematik

Viele Spiele erfordern eine erweiterte Mathematik als nicht-Spielanwendungen. Natürlich hängt der Grad der Mathematik von der Komplexität des Spiels ab. Im Allgemeinen erfordern 3D-Spiele mehr Mathematik als 2D. Glücklicherweise können Sie jederzeit mit einfachen spielen und lernen, wie Sie loslegen. Die Spieleentwicklung ist eine hervorragend geeignet, um Mathematik zu erlernen.

Wenn Sie mit der kartesischen Ebene vertraut sind –, die X-und Y-Koordinaten zum Positionieren von Objekten verwendet –, sind Sie für den Einstieg in die Spieleentwicklung ausreichend. Das folgende Beispiel zeigt eine kartesische Ebene mit dem positiven Y, das nach oben zeigt:

![](images/image2.png "This shows a Cartesian plane with positive Y pointing upward")

> [!IMPORTANT]
> Einige Engines/APIs verwenden ein Koordinatensystem, in dem der Y-Wert eines Objekts durch Vergrößern des y-Werts eines Objekts verschoben wird, während andere Systeme ein Koordinatensystem verwenden, bei dem positive Y-Werte in der Denken Sie daran, wenn Sie zwischen Systemen wechseln.
Trigonometrische Funktionen (z. b. Sinus und Kosinus) werden häufig in 2D-Spielen verwendet, die eine beliebige Form der Drehung implementieren.

Wenn Sie beabsichtigen, ein 3D-Spiel zu erstellen, müssen Sie wahrscheinlich mit Konzepten aus linearer Algebra (für Drehung und Bewegung in 3D-Raum) und einem gewissen Kalkül (für die Beschleunigung der Beschleunigung) vertraut sein.

### <a name="content-pipelines"></a>Inhalts Pipelines

Der Begriff *Inhalts Pipeline* bezieht sich auf den Prozess, der von einer Datei bei der Erstellung des Formats (z. b. einer PNG-Bilddatei) in das endgültige Format bei der Verwendung in einem Spiel benötigt wird. Das Endformat hängt davon ab, welcher Inhaltstyp verwendet wird und welche Technologie zur Darstellung des Inhalts verwendet wird.

Einige Inhalts Pipelines sind möglicherweise sehr schnell und erfordern keinen manuellen Aufwand. Beispielsweise können die meisten Spiel-Engines und APIs das PNG-Dateiformat im nicht verarbeiteten Format laden. Andererseits müssen komplexere Formate (z. b. 3D-Modelle) möglicherweise in ein anderes Format verarbeitet werden, bevor Sie geladen werden. diese Verarbeitung kann je nach Größe und Komplexität des Assets einige Zeit in Anspruch nehmen.

## <a name="game-development-teams"></a>Spiele Entwicklungs Teams

Bei der Spieleentwicklung werden neue Rollen und Titel für Personen eingeführt, die am Prozess beteiligt sind. Die meisten Spieleentwickler sind nicht in der Lage, die breite Palette an Kenntnissen zu erfüllen, die zum Freigeben eines vollständigen Spiels erforderlich sind, sodass eine Reihe von Disziplinen vorhanden ist. Beachten Sie, dass es sich hierbei nicht um eine vollständige Liste der Entwicklungsbereiche handelt – nur einige der gängigeren.

- **Programmierer** – die meisten Personen, die diesen Artikel lesen, fallen in diese Kategorie. Die Rolle eines Programmierers bei der Spieleentwicklung ähnelt der Rolle eines Programmierers in einer Anwendung ohne Spiel. Zu den Zuständigkeiten gehören das Schreiben von Logik zum Steuern des spielflusses, das Entwickeln von Systemen für allgemeine Aufgaben im Kontext eines bestimmten Projekts, das Hinzufügen und Anzeigen von Inhalten und – natürlich – das Beheben von Fehlern.
- **2D-Künstlerin** – 2D-Künstler sind für das Erstellen von *2D-Assets*verantwortlich. Dazu zählen Bilddateien für die Benutzeroberfläche des Spiels, Partikel, Umgebungen und Zeichen. Wenn das Spiel, das Sie entwickeln, 3D ist, sind 2D-Künstler möglicherweise nicht für Umgebungen und Zeichen verantwortlich. Die kostenlose Kunst für Ihr Spiel finden Sie unter [http://opengameart.org/](http://opengameart.org/) .
- **3D-Künstler** – 3D-Künstler sind für das Erstellen von *3D-Assets*verantwortlich. Hierzu zählen 3D-Modelle für Umgebungen, Zeichen und Eigenschaften (Möbel, Werke und andere inanimieren-Objekte). Einige Teams unterscheiden sich abhängig von der Größe des Teams zwischen 3D-Künstlern und 3D-Animatoren. Auf [http://opengameart.org/](http://opengameart.org/) finden Sie Kostenlose 3D-Grafiken für Ihr Spiel.
- **Game Designer** – Spiel-Designer sind dafür verantwortlich, wie das Spiel wiedergegeben wird. Dies kann allgemeine Entscheidungen einschließen, wie z. b. die Einstellung des Spiels, das Gesamtziel des Spiels und die Art und Weise, wie ein Spieler das Spiel durchläuft. Spiele-Designer können auch an sehr detaillierten Entscheidungen beteiligt sein, z. b. bei der Zuordnung von Eingaben zu Aktionen, beim Definieren von Koeffizienten für Verschiebungen oder ebenenups und beim Entwerfen des Layouts. Beachten Sie, dass der Begriff- *Designer* je nach Kontext auf einen Spiel-Designer oder einen visuellen Designer verweisen kann.
- **Sounddesigner** – Sounddesigner sind für die Audioressourcen eines Spiels verantwortlich. Einige Teams unterscheiden sich möglicherweise zwischen Personen, die für das Erstellen von Soundeffekten und Komponisten zuständig sind, während kleinere Teams möglicherweise eine einzelne Person für alle Audiodaten besitzen.

## <a name="creating-a-game-idea"></a>Erstellen einer Spiel Idee

Das Entwerfen eines Spiels ist möglicherweise leicht zu erledigen – nachdem die einzige Voraussetzung ist, dass Sie etwas Spaß machen. Leider finden viele Entwickler einen Verlust, wenn es an der Zeit ist, eine Idee zu erstellen, mit der die Entwicklung gestartet werden kann.

Die Disziplin des Spiel Entwurfs ist nicht leicht zu erläutern und erfordert, dass Sie genau wie die Kunst oder Programmierung verbessern, aber dieser Abschnitt hilft Ihnen beim Einstieg in den Pfad.

Neue Entwickler sollten klein anfangen. Es kann schwierig sein, die Versuchung zu widerstehen, ein großes, modernes Videospiel neu zu gestalten, aber kleinere Spiele können eine bessere Lernumgebung sein, und der schnellere Fortschritt sorgt für eine lohnendere Umgebung.

Viele Spiele, sowohl für Lernzwecke als auch für kommerzielle Spiele, beginnen als Verbesserung oder Änderung an einem vorhandenen Spiel. Eine Möglichkeit, Ideen zu generieren, besteht darin, andere Spiele auf Inspiration zu prüfen. Beispielsweise können Sie ein Spiel in Erwägung nehmen, das Ihnen persönlich gefällt, und herauszufinden, welche Merkmale im Spiel spielen. Es ist möglicherweise eine Untersuchung, das beherrschen der Spielmechanismen oder das Fortschreiten einer Story. Vergessen Sie nicht, bei der Suche nach neuen Ideen auch "Retro"-Spiele in Erwägung zu nehmen.

Ein weiteres Verfahren zum Erstellen neuer Ideen ist die Berücksichtigung eines bestimmten Genres, z. b. von Rätsel spielen, Strategie spielen oder platformern. Ein Genre, das dem Entwickler vertraut ist, stellt möglicherweise einen guten Ausgangspunkt dar.

Die Wiederherstellung vorhandener Spiele ist auch eine Bildungseinrichtung, obwohl dies die kommerzielle Leistung des fertigen Produkts einschränken kann. Der Prozess der Erstellung eines Spiels, selbst wenn es sich um einen exakten Klon handelt, bietet ein nützliches Schulungs Verfahren.

## <a name="game-development-technology"></a>Spiele Entwicklungstechnologie

Entwickler, die xamarin. Android und xamarin. IOS verwenden, verfügen über eine Vielzahl von Technologien zur Unterstützung bei der Entwicklung von spielen. In diesem Abschnitt werden einige der beliebtesten plattformübergreifenden Lösungen erläutert.

### <a name="monogame"></a>MonoGame

Monogame ist eine plattformübergreifende Open-Source-Version der XNA-API von Microsoft. Monogame kann verwendet werden, um Spiele für IOS, Android, Mac OS X, Linux, Windows, Windows RT, PS4, psvita, Xbox One und Switch zu erstellen.

Monogame ist technisch gesehen keine Spiel-Engine, sondern eher eine Spielentwicklungs-API. Dies bedeutet, dass das Arbeiten mit monogame das direkte Verwalten von Spielobjekten, das manuelle Zeichnen von Objekten und das Implementieren von gängigen Objekten wie Kameras und *Szenen Diagrammen* (die übergeordnete untergeordnete Hierarchie Zwischenspiel Objekten) erfordert.

Monogame bietet keine standardmäßige visuelle Entwicklungsumgebung, sodass das Arbeiten mit monogame Programmierkenntnisse erfordert.

Wichtige Beispiele für Spiele, die monogame verwenden, sind:

FEZ:

![](images/image7.png "FEZ")

Basti

![](images/image8.jpg "Bastion")

Um mit monogame zu arbeiten, besuchen Sie die [monogame](~/graphics-games/monogame/index.md)-Handbücher.

### <a name="urhosharp"></a>UrhoSharp

Urhosharp ist ein plattformübergreifendes 3D-und 2D-Modul auf hoher Ebene, mit dem animierte 3D-und 2D-Szenen für Ihre Anwendungen mithilfe von Geometrien, Materialien, Lichtern und Kameras erstellt werden können.

![](images/urhosharp.gif "UrhoSharp is a cross-platform high-level 3D and 2D engine that can be used to create animated 3D and 2D scenes")

Sehen Sie sich die [urhusharp-Leitfäden](~/graphics-games/urhosharp/index.md) an, um zu beginnen.

### <a name="additional-technology"></a>Zusätzliche Technologie

Die oben markierten Technologien sind nur ein Beispiel für die verfügbaren Technologien. Weitere wichtige Technologien sind:

- **Sprite Kit** – xamarin bietet Unterstützung für das Sprite Kit-Spiel Framework von Apple, das Ihnen den Zugriff auf die gesamte Funktionalität der systemeigenen API ermöglicht. Da das Sprite Kit von Apple erstellte Technologien umfasst, bietet es eine umfassende Integration in den Rest des IOS-Ökosystems. Natürlich ist das Sprite Kit nicht plattformübergreifend, sodass es nicht auf Android verwendet werden kann. Weitere Informationen zur Verwendung von Sprite Kit finden Sie in diesem Beitrag: [https://blog.xamarin.com/make-games-with-xamarin.ios-and-sprite-kit/](https://blog.xamarin.com/make-games-with-xamarin.ios-and-sprite-kit/)
- **Scene Kit** – xamarin bietet auch Unterstützung für das Scene Kit-Framework von Apple, das die Implementierung von 3D-Grafiken in ios-apps vereinfacht. Das Scene Kit ist auch von Apple bereitgestellte Technologie, sodass es sowohl über die Integrations-als auch über die plattformspezifischen Überlegungen für Sprite Kit verfügt. Weitere Informationen zum Scene Kit finden Sie in diesem Beitrag: [https://blog.xamarin.com/3d-in-ios-8-with-scene-kit/](https://blog.xamarin.com/3d-in-ios-8-with-scene-kit/)
- **Opentk –** Opentk (steht für Open Tool Kit) bietet OpenGL-Zugriff auf niedriger Ebene auf Ios-, Apple-und Mac-Hardware. Weitere Informationen zu opentk finden Sie auf der Hauptseite unter: [https://opentk.net/](https://opentk.net/)

## <a name="related-links"></a>Verwandte Links

- [Monogame-Führungslinien](~/graphics-games/monogame/index.md)
- [Urhusharp-Führungslinien](~/graphics-games/urhosharp/index.md)
