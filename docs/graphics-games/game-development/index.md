---
title: "Einführung in die 3D-Spielentwicklung mit Xamarin"
description: "Die Art der 3D-Spielentwicklung kann von der Entwicklung von anderen Typen von apps erheblich abweichen. Dieser Artikel ist eine Einführung in die Entwicklung mit Technologien, die mit Xamarin.iOS und Xamarin.Android verwendet werden können. Es bietet eine allgemeine Erläuterung der wie Spiele vorgenommen werden und eine Stichprobe von Technologien für die Verwendung mit Xamarin.iOS und Xamarin.Android."
ms.topic: article
ms.prod: xamarin
ms.assetid: 0E3CDCD2-FBE4-49F5-A70E-8A7B937BAF1D
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/24/2017
ms.openlocfilehash: 9d1ce2da87d6f169efb5431f734695f6876cf3f0
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="introduction-to-game-development-with-xamarin"></a>Einführung in die 3D-Spielentwicklung mit Xamarin

_Die Art der 3D-Spielentwicklung kann von der Entwicklung von anderen Typen von apps erheblich abweichen. Dieser Artikel ist eine Einführung in die Entwicklung mit Technologien, die mit Xamarin.iOS und Xamarin.Android verwendet werden können. Es bietet eine allgemeine Erläuterung der wie Spiele vorgenommen werden und eine Stichprobe von Technologien für die Verwendung mit Xamarin.iOS und Xamarin.Android._

Entwickeln von Spielen kann sehr spannend insbesondere angegeben, wie einfach es sein kann, um Ihre Arbeit auf mobilen Plattformen zu veröffentlichen. Dieser Artikel erläutert die Konzepte, und Technologien, die im Zusammenhang mit der Entwicklung, die Ihnen helfen erstellen Spiele, Ihr Ziel ist, um ein qualitativ hochwertiges AAA Spiel oder auf eine Anwendung für Fun zu erstellen.

In diesem Artikel werden die folgenden Themen behandelt:

- **Im Vergleich zu nicht-Spiel Programmierkonzepte Spiel** – Artikelserie einige Konzepte, die entweder für 3D-Spielentwicklung, eindeutig oder gemeinsam mit anderen Typen von Entwicklung jedoch verdienen Schwerpunkt hier aufgrund ihrer Wichtigkeit.
- **Spiel Entwicklungsteam** – in diesem Abschnitt untersucht die verschiedenen Rollen in einem Team von Entwicklern Spiel.
- **Erstellen eine Spiele Idee** – in diesem Abschnitt können Sie eine neue game Idee Erstellen – der erste Schritt darin, ein neues Spiel.
- **Game Development Technologie** – wir müssen hier sind einige der plattformübergreifenden Technologien zur Verfügung, die Ihre Produktivität als Spiel Entwickler verbessert werden können.


# <a name="game-vs-non-game-programming-concepts"></a>Spiel im Vergleich zu Programmierkonzepte für die nicht-Spiel

Programmierer in 3D-Spielentwicklung verschoben werden häufig mit neuen Konzepte und Entwicklungsmustern konfrontiert. In diesem Abschnitt werden eine allgemeine Ansicht der einige dieser Konzepte.


## <a name="the-game-loop"></a>Das Spiel Schleife

Ein typisches Spiel erfordert Konstante verschieben oder ändern Sie auf dem Bildschirm als Antwort auf eine Benutzerinteraktion und automatische Spiellogik Fall zu sein. Dies erfolgt über die in der Regel als "" bezeichnet wird eine *Spiel Schleife*. Eine Spiele Schleife ist eine Art von Schleifen-Anweisung (z. B. einer While-Schleife) die mit einer sehr hohe Frequenz, z. B. 30 oder 60 *Frames pro Sekunde*.

Im folgenden finden ein Diagramm einer einfachen Spiel Schleife:

![](images/image1.png "Dies ist ein Diagramm einer einfachen game-Schleife")

Die Technologien, die im folgenden erörtert werden der tatsächliche While-Schleife Abstraktion, jedoch trotz diese Abstraktion wird das Konzept von jedem Frame Updates vorhanden sein.

Codeleistung kann sogar ganz einfach gesagt Spiele Vorrang. Zum Beispiel: eine Funktion, die auszuführende 10 Millisekunden dauert möglicherweise einen erheblichen Einfluss auf die Leistung eines Spiels – insbesondere dann, wenn sie mehr als einmal pro Frame aufgerufen wird. Wenn ein Spiel mit 30 Frames pro ausgeführt wird also Zweitens klicken Sie dann, dass jeder Rahmen in unter 33 Millisekunden ausgeführt werden muss. Im Gegensatz dazu, eine solche Funktion selbst möglicherweise nicht erkennbar, wenn sie nur als Reaktion auf eine Schaltfläche klicken Sie in einer nicht-Game-Anwendung ausgeführt wird.

Häufig verwendete Typen von Logik, die möglicherweise ausgeführten jeden-Frame gehören:

- **Eingabe lesen** – das Spiel möglicherweise überprüfen, ob der Benutzer mit dem Spiel durch Überprüfen der Eingabe Hardware, z. B. die Touchscreen, Tastatur, Maus oder Gamecontroller interagiert hat.
- **Verschieben von** – welche Verschieben von einem zentralen Ort zu einem anderen wird in der Regel eine sehr kleine verschieben Objekte Betrag jeder Frame, um den Eindruck flüssigen Bewegung zu gewähren.
- **Kollisionen** – viele Spiele erfordern häufig testen gibt an, ob verschiedene Objekte sich überschneidenden oder werden überlappende. Kollisionen in einem späteren Abschnitt in diesem Artikel ausführlicher beschrieben. Verschieben und Konflikte möglicherweise von einem dedizierten physikalische Simulation System behandelt werden.
- **Prüfung von Game spezifische Bedingungen** – der Status eines Spiels kann gesteuert werden, indem bestimmte Bedingungen wie z. B., ob der Spieler erworben hat, ausreichende Anzahl von Punkten oder gibt an, ob der vorgesehenen Zeit ausgeführt wurde, aus.
- **AI-Verhalten** – jeder Frame-Logik, die verwendet werden kann, das Verhalten von Objekten zu steuern, die nicht von den Player verwenden, z. B. die patrolling ein Feind oder die Übertragung der protokollsicherungsdaten Gegner Treiber um eine Oval kontrolliert werden.
- **Rendern von** – die meisten Spiele aktualisiert die Anzeige auf Bildschirm jeder Frame. Dies kann in Reaktion auf Änderungen, die Auswirkungen auf Spiels (z. B. ein Zeichen, die über eine Ebene verschieben) oder einfach um visual Polnisch (z. B. Abnehmend Schneefall oder animierte Symbole) bereitzustellen.

Sollten Sie bedenken, dass viele der oben aufgeführten Aktivitäten den Status der gesamten Anwendung ändern können, während viele nicht-Game-apps sind tendenziell Statuswechsel als Antwort auf Ereignisse, die ausgelöst wird.


## <a name="content-loading-and-unloading"></a>Inhalt laden und entladen

Manuell laden und Entladen (oder disposing) Inhalt kann erforderlich sein, je nachdem welche Technologie Sie bei der Entwicklung verwenden. Manuell laden und Entladen von Ressourcen können eine Reihe von Gründen erforderlich sein:

 - Bestand können viel Zeit in Bezug auf die Länge eines einzelnen Frames laden dauern. Einige Objekte dauert sogar Sekunden zu laden, die eine schwerwiegende Beschädigung die Erfahrung stören würde, wenn mid Spielverlauf geladen. Wenn die Ladezeit besonders lange (z. B. mehr als ein oder zwei Sekunden) ist Sie möglicherweise eine animierte anzeigen möchten Bildschirm oder die Bearbeitung zu laden.
 - Medienobjekte können viel RAM erfordern active Verwaltung von was geladen wird, entsprechend innerhalb von Zielplattformen für das Spiel bereitgestellt werden.
 - Spiele müssen möglicherweise mehr Ressourcen als im Arbeitsspeicher passen anzuzeigen. "Öffnen Sie World" Spiele gehören häufig große Umgebungen, die der Spieler nahtlos navigieren können – also keine laden Bildschirme mit. In diesem Fall müssen Sie zum Erstellen einer benutzerdefinierten System für Streaminginhalte im und Verwalten von speicherauslastung.

Benutzerdefinierte Dateiformate möglicherweise Verarbeitung zur Ladezeit, jeglichen Code benutzerdefiniert zu laden.


## <a name="math"></a>Mathematik

Viele Spiele erfordern erweiterte Mathematik als nicht-Game-Anwendungen. Die Komplexität des Spiels natürlich abhängig das Maß an mathematische. Im Allgemeinen erfordern 3D-Spielen Weitere mathematische als 2D. Glücklicherweise können immer Einstieg einfache Spiele und erfahren Sie, wie Sie fortfahren. 3D-Spielentwicklung Anzeigename kann eine hervorragende Möglichkeit, um zu erfahren, Mathematik sein.

Wenn Sie mit der kartesischen Ebene – vertraut sind, die verwendet X und Y-Koordinaten um Objekte zu positionieren, und Sie genug Informationen zum Einstieg in die Entwicklung von. Das folgende Beispiel zeigt eine kartesischen Ebene mit steigender positive Y-verweist:

![](images/image2.png "Dies zeigt eine kartesischen Ebene mit steigender positive Y-verweist")

> [!IMPORTANT]
> Einige Datenbankmodule-APIs verwenden ein Koordinatensystems, in denen wird ein Objekt Y-Wert erhöhen es nach unten, verschieben, während andere Systeme einem Koordinatensystem, wobei positive Y verwenden einrichten. Beachten Sie dies, wenn Sie zwischen Systemen verschoben werden.
Trigonometrische Funktionen (z. B. Sinus- und Kosinuswert) werden häufig in 2D Spiele verwendet, die keiner Form eines Drehung implementieren.



Wenn Sie beabsichtigen, zu der ein 3D Spiel vornehmen, müssen Sie wahrscheinlich von lineare Algebra (zur Bandrotation und die Verschiebung im 3D-Bereich) als auch einige Analysis (für die Implementierung Acceleration) mit Konzepten vertraut sein.


## <a name="content-pipelines"></a>Content-Pipelines

Der Begriff *Content Pipeline* bezieht sich auf den Prozess, der eine Datei verwendet wird, aus dem Format (z. B. eine PNG-Bilddatei) beim Abrufen in das endgültige Format bei Verwendung in ein Spiel. Das letzte Format hängt ab, auf dem Typ des Inhalts verwendet wird sowie die Technologie verwendet wird, um den Inhalt darstellen.

Einige Inhalte Pipelines sind sehr schnell und erfordern keine manuellen Aufwand. Beispielsweise können die meisten Spiel Module und APIs das PNG-Dateiformat in seiner unverarbeiteten Format geladen werden. Andererseits, müssen möglicherweise etwas komplizierteren Formate (z. B. 3D-Modelle) in ein anderes Format verarbeitet werden, bevor Sie geladen wird, und diese Verarbeitung kann je nach Größe und Komplexität des Medienobjekts einige Zeit dauern.


# <a name="game-development-teams"></a>Spiel Entwicklungsteams

3D-Spielentwicklung führt neue Rollen und Titel für Einzelpersonen, die in den Prozess einbezogen. Die meisten Entwickler können nicht zum Erfüllen der Breite Palette an maßgeblichen Qualifikationen für eine vollständige Spiel freigeben, damit eine Anzahl von Disziplinen vorhanden. Bedenken Sie, dass dies eine vollständige Liste der Bereiche der Entwicklung – nur einige der häufigeren nicht ist.

- **Programmierer** – die meisten Personen, die in diesem Artikel wird fallen in diese Kategorie zu lesen. Die Rolle der Programmierer in 3D-Spielentwicklung ähnelt ein Programmierer-Rolle in einer Anwendung nicht Spiel. Aufgaben zählen das Schreiben von Logik, um den Ablauf eines Spiels, Entwickeln von Systemen für allgemeine Aufgaben im Kontext eines bestimmten Projekts, hinzufügen und Anzeigen des Inhalts und des Kurses Beheben von Fehlern zu steuern.
- **2D Interpreten** – 2D Künstler sind verantwortlich für das Erstellen von *2D Bestand*. Dazu gehören Bilddateien, für des Spiels GUI, Partikel, Umgebungen und Zeichen. Wenn das Spiel, das Sie entwickeln 3D ist, können 2D Künstler nicht verantwortlich für Umgebungen und Zeichen sein. Finden Sie kostenlose ClipArt für das Spiel am [http://opengameart.org/](http://opengameart.org/) .
- **3D Künstler** – 3D Künstler sind verantwortlich für das Erstellen von *3D Bestand*. Dazu gehören 3D-Modelle für Umgebungen, die Zeichen und Eigenschaftendateien enthalten (Möbel Pflanzen und andere unbelebte Objekte). Einige Teams unterscheiden zwischen 3D Künstler und 3D-Animatoren abhängig von der Größe des Teams. Sie finden die kostenlose 3D Grafiken für das Spiel am [http://opengameart.org/](http://opengameart.org/) .
- **Spiel Designer** – Spiel Designer sind dafür verantwortlich zu definieren, wie das Spiel wiedergegeben wird. Dies kann auf hoher Ebene Entscheidungen wie z. B. die Einstellung für das Spiel, das Ziel des Spiels und Fortschritt von ein Player über das Spiel einschließen. Spiel-Designer können auch Koeffizienten für Bewegung oder Ebene USV definieren und Entwerfen Ebene Layouts sehr detaillierte Entscheidungen dienen, z. B. Zuordnung Eingabe für Aktionen, beteiligt sein. Beachten Sie, dass der Begriff *Designer* bezieht sich möglicherweise auf ein Spiel oder eine visual-Designer, je nach Kontext.
- **Sound Designer** – Sound Designer sind verantwortlich für ein Spiel-audio-Objekte. Einige Teams möglicherweise zwischen Personen, die verantwortlich für das Erstellen von Soundeffekten und Komponisten, während kleinere Teams eine Einzelperson verantwortlich für alle Audio möglicherweise unterscheiden.


# <a name="creating-a-game-idea"></a>Erstellen eine Spiele Idee

Entwerfen ein Spiel möglicherweise erleichtert, – nachdem alle die einzige Anforderung ist "etwas Arbeitserleichterung stellen." Leider finden Sie viele Entwickler selbst zu einem Verlust bei der Erstellung eine Vorstellung von der Entwicklung zu starten.

Die Disziplin des Spiels Entwurf nicht problemlos erläutert und erfordert bewährt, ebenso wie die Art zu verbessern oder Programmierung ist, aber in diesem Abschnitt können Sie die nach-unten den Pfad zu starten.

Neue Entwickler sollte klein gestartet werden. Es kann schwierig sein, die Versuchung, Neuerstellen ein großes, modernes Video-Spiels widerstehen können jedoch kleinere Spiele eine bessere Learning-Umgebung und schneller ausgeführt wird, mehr lohnende zu vereinfachen.

Viele Spiele, die beide zum Lernen als auch für gewerbliche Spiele als Verbesserung oder Änderung einer vorhandenen Spiel beginnen. Eine Möglichkeit zum Generieren von Ideen ist am anderen Spielen für Inspiration gesucht werden soll. Beispielsweise können Sie erwägen, ein Spiel, die Sie persönlich gefällt, und versuchen Sie, welche Eigenschaften über das Spiel identifizieren Arbeitserleichterung stellen. Untersuchung von, beherrschen Mechanismen für das Spiel oder über eine Story hängen möglicherweise. Vergessen Sie nicht "retro" Spiele sowie bei der Suche nach neuen Ideen zu berücksichtigen.

Ein weiteres Verfahren zum Generieren von neue Ideen wird zu einem bestimmten "Genre" z. B. Puzzlespiele, Strategiespiele oder Platformers berücksichtigen. "Genre" und vertraut dem Entwickler möglicherweise einen guten Ausgangspunkt bereit.

Vorhandene Spiele erneut zu erstellen ist auch eine Bildungseinrichtung Erfahrung, obwohl dies das fertige Produkt kommerziellen Durchführbarkeit eingeschränkt werden kann. Das Verfahren zum Erstellen eines Spiels, sogar eines, das ein genau Klon ist bietet wertvolle Lernzwecken.


# <a name="game-development-technology"></a>3D-Spielentwicklung-Technologie

Entwickler, die mit Xamarin.Android und Xamarin.iOS haben eine Vielzahl von Technologien, die Ihnen bei der Entwicklung zu unterstützen. In diesem Abschnitt werden einige der am häufigsten verwendeten plattformübergreifende Lösungen erläutert.


## <a name="cocossharp"></a>CocosSharp

CocosSharp ist ein Open Source- und plattformübergreifender Version der Cocos 2D Spiel-Engine. Das Modul bietet Zugriff auf Android-, iOS-, Mac OS X, Windows-Desktop, Windows RT und Windows Phone.

CocosSharp konzentriert sich auf einfachen Programmierer API für die Entwicklung von 2D. Die Vergrößerung in mehrere Spieler auf mobilen Geräten hat dazu beigetragen, um die Beliebtheit der Entwicklung von 2D ausführenden CocosSharp geeignete Technologie für Hobbys und kommerziellen Projekten gleichermaßen reignite. Es wird als Quelle Code oder DLL-Dateien (die über NuGet abgerufen werden können) bereitgestellt, aber er bietet einen visuellen Editor nicht; Daher erfordert Interaktion mit dem Modul CocosSharp Kenntnisse in der Programmierung.

Um mit CocosSharp beginnen, sehen Sie sich unsere [CocosSharp Handbücher](~/graphics-games/cocossharp/index.md).

Das Spiel verärgerten Ninjas mit CocosSharp erstellt, und es kann ein guter Ausgangspunkt, wenn Sie für eine bereits ausgeführte Spiel für mehrere Plattformen suchen:

![](images/image3.png "Das Spiel verärgerten Ninjas wurde mit CocosSharp erstellt werden.")

Sie können die Software hier herunterladen und erhalten Sie weitere Informationen zur den [AngryNinjas Github-Seite](https://github.com/xamarin/AngryNinjas).


## <a name="monogame"></a>MonoGame

MonoGame ist eine Open-Source cross-Platform-Version des Microsoft XNA-API. MonoGame kann verwendet werden, um Spiele für iOS, Android, Mac OS X, Linux, Windows, Windows RT und Windows Phone zu erstellen.

Im Gegensatz zu CocosSharp ist MonoGame, technisch keine Spiel-Engine, sondern vielmehr eine 3D-Spielentwicklung API. Dies bedeutet, dass die Arbeit mit MonoGame erfordert direkt Verwalten von game-Objekten, manuell Zeichnen von Objekten und gemeinsame Objekte wie z. B. Kameras implementieren und *Szene Diagramme* (die über-/ unterordnungshierarchie zwischen Spiel Objekten). Um den Unterschied zu verstehen, berücksichtigen Sie, dass CocosSharp auf MonoGame aufsetzt. MonoGame generalisiert Clientplattform-spezifische Technologie, wie z. B. Grafiken, Rendering und Audiodateien während CocosSharp Code zu organisieren und Implementieren von Spiellogik bereitstellt.

MonoGame bietet keine standard-visuelle Entwicklungsumgebung, damit die Arbeit mit MonoGame erfordert programmieren.

Wichtige Beispiele für Spiele, die mit MonoGame:

FEZ:

![](images/image7.png "FEZ")

Geschützte:

![](images/image8.jpg "Bastion")

Informationen zur unmittelbaren Verwendung mit MonoGame, rufen Sie über unsere [MonoGame Handbücher](~/graphics-games/monogame/index.md).


## <a name="urhosharp"></a>UrhoSharp

UrhoSharp ist eine plattformübergreifende auf hoher Ebene 3D und 2D Engine, die zum Erstellen der animierter 3D und 2D Szenen für Ihre Anwendungen mithilfe von Geometrien, Materialien, Leuchten und Kameras verwendet werden kann.

![](images/urhosharp.gif "UrhoSharp ist eine plattformübergreifende auf hoher Ebene 3D und 2D, die mit der animierte 3D und 2D Szenen erstellt werden können")

Sehen Sie sich die [UrhoSharp Handbücher](~/graphics-games/urhosharp/index.md) um zu beginnen.

## <a name="additional-technology"></a>Zusätzliche Technologie

Die markierten Technologien ist nur ein Beispiel für den verfügbaren Technologien. Andere wichtigen Technologien zählen:

- **Sprite Kit** – Xamarin bietet Unterstützung für Apple Sprite Kit Spiel Framework, die Sie Zugriff auf alle Funktionen der systemeigenen API gewährt. Da Sprite Kit Technologie, die von Apple erstellt ist, wird die enge Integration mit dem Rest des iOS-Ökosystem. Sprite Kit ist natürlich nicht plattformübergreifende auf Android-Geräten kann deshalb nicht verwendet werden. Weitere Informationen finden Sie unter Sprite Kit, finden Sie in diesen Beitrag: [http://blog.xamarin.com/make-games-with-xamarin.ios-and-sprite-kit/](http://blog.xamarin.com/make-games-with-xamarin.ios-and-sprite-kit/)
- **Szene Kit** – Xamarin bietet auch Unterstützung für Apple Szene Kit Framework implementieren 3D-Grafiken in iOS-apps zu vereinfachen. Szene Kit ist auch Technologie, die von Apple, bereitgestellt werden, sodass er die Integration und plattformspezifischen Hinweise für Sprite Kit oben genannten verfügt. Weitere Informationen zu Szene Kit, finden Sie in diesen Beitrag: [http://blog.xamarin.com/3d-in-ios-8-with-scene-kit/](http://blog.xamarin.com/3d-in-ios-8-with-scene-kit/)
- **OpenTK –** OpenTK (die für das Öffnen Tool Kit steht) bietet auf niedriger Ebene OpenGL-Zugriff auf iOS und Apple Macintosh Hardware. Weitere Informationen zu OpenTK, finden Sie unter der Hauptseite auf: [http://www.opentk.com/](http://www.opentk.com/)


# <a name="summary"></a>Zusammenfassung

Dieser Artikel behandelt die wichtigsten Konzepte der Entwicklung und enthält Informationen zu den ersten Schritten machen Ihr erste Spiel. Sobald Sie in diesem Artikel abgeschlossen haben, sind in den nächsten Schritten wählen Sie die Technologie und Einstieg durch unsere Reihe von Lernprogrammen, die in den entsprechenden Abschnitten oben genannten verknüpft.

## <a name="related-links"></a>Verwandte Links

- [CocosSharp-Handbücher](~/graphics-games/cocossharp/index.md)
- [MonoGame-Handbücher](~/graphics-games/monogame/index.md)
- [UrhoSharp-Handbücher](~/graphics-games/urhosharp/index.md)
