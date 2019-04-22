---
title: Einführung in die Entwicklung von Spielen mit Xamarin
description: Dieses Dokument enthält einen allgemeinen Überblick über die Entwicklung von Spielen mithilfe von Xamarin, beschreibt, wie Spiele vorgenommen werden und eine Stichprobe der Technologien für die Verwendung mit Xamarin.iOS und Xamarin.Android zur Verfügung stehen.
ms.prod: xamarin
ms.assetid: 0E3CDCD2-FBE4-49F5-A70E-8A7B937BAF1D
author: conceptdev
ms.author: crdun
ms.date: 03/24/2017
ms.openlocfilehash: 314bedcb6bb2d7ebf9d8f98428b6a7cad059f73b
ms.sourcegitcommit: 3489c281c9eb5ada2cddf32d73370943342a1082
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/18/2019
ms.locfileid: "59509952"
---
# <a name="introduction-to-game-development-with-xamarin"></a>Einführung in die Entwicklung von Spielen mit Xamarin

Entwickeln von Spielen kann sehr aufregend insbesondere in Anbetracht wie einfach es sein kann, um Ihre Arbeit auf mobilen Plattformen zu veröffentlichen. In diesem Artikel erläutert die Begriffe aus, und Technologien, die im Zusammenhang mit der Entwicklung von Spielen, mit denen Sie erstellen Spiele, ob das Ziel ist es, um eine qualitativ hochwertige AAA Spiel oder eine so zum Spaß-Programm zu erstellen.

In diesem Artikel werden die folgenden Themen behandelt:

- **Spiel im Vergleich zu nicht-Game-Programmierkonzepte** – lernen wir einige Konzepte, die entweder nur für die Entwicklung von Spielen, oder Sie freigegeben wurden andere Entwicklungsszenarien einsetzen, aber verdienen Schwerpunkt hier aufgrund ihrer Wichtigkeit.
- **Entwicklung von Spielen Team** – dieser Abschnitt befasst sich mit den verschiedenen Rollen in einem Team von Spielentwickler dar.
- **Erstellen eine spielidee in Angriff** – in diesem Abschnitt können Sie eine neue spielidee in Angriff zu erstellen – der erste Schritt bei der Erstellung eines neuen Spiels.
- **Spielen entwicklungstechnologie** – hier führen wir einige der plattformübergreifenden Technologien zur Verfügung, die Ihre Produktivität als Entwickler von Spielen verbessern können.

## <a name="game-vs-non-game-programming-concepts"></a>Spiel im Vergleich zu Nicht-Game-Programmierkonzepte

Programmierer, die in der Entwicklung von Spielen verschoben werden häufig mit neuen Konzepte und den Entwicklungsmustern konfrontiert. In diesem Abschnitt werden eine allgemeine Ansicht der einige dieser Konzepte.

### <a name="the-game-loop"></a>Die Spielschleife

Ein normales Spiel erfordert Konstante verschieben oder ändern, sodass Sie auf dem Bildschirm als Reaktion auf Benutzerinteraktion und automatische Spiellogik sowohl passiert. Hierzu wird in der Regel als bezeichnet ein *spielschleife*. Eine Spiele-Schleife ist eine Art von Schleife Anweisung (z. B. eine While-Schleife), die sehr häufig, z. B. 30 oder 60 ausgeführt wird *Frames pro Sekunde*.

Im folgenden finden ein Diagramm einer einfachen Spiel-Schleife:

![](images/image1.png "Dies ist ein Diagramm einer einfachen Spiel-Schleife")

Die Technologien, die im folgenden erläutert werden der tatsächliche While-Schleife abstrahieren, aber trotz dieser Abstraktion wird das Konzept der every-Frame-Updates vorhanden sein.

Leistung von Code kann auch ganz einfach Spiele Vorrang. Zum Beispiel: eine Funktion, die dauert 10 Millisekunden zum ausführen kann einen erheblichen Einfluss auf die Leistung für ein Spiel – haben, insbesondere dann, wenn sie mehr als einmal pro Frame aufgerufen wird. Wenn Ihr Spiel mit 30 Frames pro läuft bedeutet dies Zweitens klicken Sie dann, dass jeder Frame in unter 33 Millisekunden ausgeführt werden muss. Im Gegensatz dazu kann eine solche Funktion nicht einmal bemerkt werden, wenn sie nur als Reaktion auf einen Klick auf die in einer nicht-Game-Anwendung ausführt.

Allgemeine Logik, die möglicherweise ausgeführten every-Frame gehören:

- **Lesen von Eingabe** – das Spiel zu überprüfen, ob der Benutzer das Spiel interagiert hat, durch Eingabe Hardware, wie z. B. den Touchscreen, Tastatur, Maus oder Gamecontroller aktivieren müssen.
- **Datenverschiebung** – Objekte, die die Verschiebung von einem zentralen Ort zu einem anderen in der Regel eine sehr kleine wechselt Betrag jedes Bild, um die Illusion von fließend während der Übertragung zu vermitteln.
- **Konflikt** – viele Spiele erfordern häufig testen, ob verschiedene Objekte sich überschneidenden überlappende oder werden. Kollisionen in einem späteren Abschnitt in diesem Artikel ausführlicher erläutern. Verschieben und Konflikte können von einem dedizierten Physik Simulation System behandelt werden.
- **Prüfung von spezifischen Bedingungen** – der Status für ein Spiel kann gesteuert werden, indem bestimmten Bedingungen wie z. B., ob der Spieler erhalten hat, ausreichende Anzahl von Punkten oder, ob der vorgesehenen Zeit ausgeführt wurde, sich.
- **AI-Verhalten** – jeder Frame-Logik, die verwendet werden kann, das Verhalten von Objekten zu steuern, die nicht von den Player verwenden, z. B. die patrolling ein Gegner oder die Übertragung der protokollsicherungsdaten Gegner Treiber um eine Oval kontrolliert werden.
- **Rendern von** – die meisten Spiele aktualisiert die Anzeige Bildschirm auf jedem Frame. Dies kann erfolgen, als Reaktion auf Änderungen, die Auswirkungen auf Spiels (z. B. ein Zeichen, die eine Ebene durchsuchen) oder einfach bereitzustellenden visuellen Ausgereiftheit (z. B. fallender Schnee oder animierter Symbole).

Sollten Sie bedenken, dass viele der oben aufgeführten Aktivitäten den Status der in der gesamten Anwendung ändern können, während viele nicht-Game-apps weisen tendenziell statusänderung als Reaktion auf Ereignisse, die ausgelöst wird.

### <a name="content-loading-and-unloading"></a>Inhalt laden und entladen

Manuell laden und Entladen (oder freigegeben) Inhalt kann erforderlich sein, je nachdem welche Technologie Sie bei der Entwicklung verwenden. Manuell laden und Entladen von Ressourcen können eine Reihe von Gründen erforderlich sein:

 - Ressourcen dauert sehr viel Zeit in Bezug auf die Länge eines einzelnen Frames zu laden. Einige Ressourcen dauert sogar Sekunden zum Laden, die die Oberfläche erheblich stören würde, wenn mid Gameplay geladen. Wenn die Ladezeit sehr lange (z. B. mehr als ein oder zwei Sekunden) ist Sie möglicherweise einen animierten anzeigen möchten Bildschirm oder die Bearbeitung geladen.
 - Assets können viel Arbeitsspeicher, die eine aktive Verwaltung von was geladen wird, entsprechend der in die vom des Spiels Zielplattformen bereitgestellten erfordern nutzen.
 - Spiele müssen möglicherweise mehr Ressourcen als in den Arbeitsspeicher passen anzuzeigen. "Open World" Spiele enthalten häufig große Umgebungen, die der Spieler können nahtlos durch Navigieren –, die keine Bildschirme geladen ist. In diesem Fall müssen Sie zum Erstellen eines benutzerdefinierten System für die streaming-Inhalten in und zum Verwalten von speicherauslastung.

Benutzerdefinierte Dateiformate möglicherweise Verarbeitung zur Ladezeit benutzerdefinierte Laden Code erfordern.

### <a name="math"></a>Mathematik

Viele Spiele erfordern mehr Mathematik als nicht-Game-Anwendungen. Die Ebene der mathematischen hängt natürlich die Komplexität des Spiels. Im Allgemeinen benötigt 3D-Spiele Weitere mathematische als 2D. Zum Glück Sie immer Einstieg in einfache Spiele und erfahren Sie, wie Sie fortfahren. Entwicklung von Spielen kann es sich um eine hervorragende Möglichkeit, erfahren, Mathematik sein!

Wenn Sie mit der kartesischen Ebene – vertraut sind, die verwendet X und Y-Koordinaten um Objekte zu positionieren, dann wissen Sie genug, um den ersten Schritten mit der Entwicklung von Spielen. Das folgende Beispiel zeigt eine kartesischen Ebene mit nach oben positive Y-verweisen:

![](images/image2.png "Dadurch wird eine kartesischen Ebene mit nach oben positive Y-verweisen")

> [!IMPORTANT]
> Einige Engines/APIs verwenden ein Koordinatensystem, in dem Y-Werts eines Objekts zu erhöhen sie nach unten, wechselt während andere Systeme auf einem Koordinatensystem verwenden, in denen positive Y aktiv ist. Beachten Sie, dass wenn Sie zwischen Systemen.
Trigonometrische Funktionen (z. B. Sinus und Kosinus) werden häufig in 2D-Spiele verwendet, die jede Art von Rotation zu implementieren.

Wenn Sie beabsichtigen, für das Erstellen eines 3D-Spiels wahrscheinlich müssen Sie mit den Konzepten von lineare Algebra (für Rotation und Verschieben von Daten in 3D-Bereich) sowie einige berechnen (für die Implementierung von Acceleration) vertraut sein.

### <a name="content-pipelines"></a>Inhaltspipelines

Der Begriff *Pipeline für Bildinhalte* bezieht sich auf den Prozess, der eine Datei verwendet, um aus dem Format, die beim Erstellen von (z. B. eine PNG-Bilddatei) zu erhalten, in das endgültige Format, wenn in einem Spiel verwendet. Das abschließende Format hängt ab, auf dem Typ des Inhalts verwendet wird sowie die Technologie verwendet wird, um den Inhalt darstellen.

Einige inhaltspipelines können sehr schnell sein und keinen manuellen Aufwand erfordern. Beispielsweise können die meisten Spiele-Engines und APIs das PNG-Dateiformat in das nicht verarbeitete Format geladen werden. Auf der anderen Seite möglicherweise mehr komplizierte Formate (z. B.-3D-Modelle) in ein anderes Format verarbeitet werden, bevor Sie geladen wird, und diese Verarbeitung kann je nach Größe und Komplexität des Medienobjekts einige Zeit dauern.

## <a name="game-development-teams"></a>Entwicklung von Spielen-Teams

Entwicklung von Spielen führt neue Rollen und Titel für Einzelpersonen, die der Prozess an. Die meisten Spiele-Entwickler sind nicht erfüllen die Vielzahl von Fähigkeiten erforderlich, um eine vollständige Spiel, freigeben, sodass der eine Anzahl von Disziplinen vorhanden. Bedenken Sie, dass es sich nicht um eine vollständige Liste der Bereiche der Entwicklung – nur einige der häufigeren handelt.

- **Programmierer** – die meisten Personen, die in diesem Artikel wird fallen in diese Kategorie zu lesen. Die Rolle der Programmierer bei der Spielentwicklung ähnelt der Programmierer-Rolle in einer nicht-Game-Anwendung. Verantwortlich für das Schreiben von Logik zum Steuern des Datenflusses ein Spiel, das Entwickeln von Systemen für allgemeine Aufgaben im Kontext eines bestimmten Projekts, hinzufügen und Anzeigen von Inhalt und – natürlich – Beheben von Fehlern.
- **2D Interpreten** – 2D Künstler sind verantwortlich für das Erstellen von *2D-Assets*. Dazu gehören die Bilddateien, für des Spiels GUI, Partikel, Umgebungen und Zeichen. Wenn das Spiel, das Sie entwickeln 3D ist, können 2D Künstler nicht für Umgebungen und Zeichen verantwortlich sein. Finden Sie kostenlose Art für Ihr Spiel auf [ http://opengameart.org/ ](http://opengameart.org/) .
- **3D Künstler** – 3D Künstler sind verantwortlich für das Erstellen von *3D-Objekten*. Dazu gehören 3D-Modelle für Umgebungen, Zeichen und Eigenschaftendateien (Möbel, Pflanzen und andere Objekte einem Informationstechnologie-(IT-)Unternehmen). Einige Teams unterscheiden zwischen 3D Interpreten und 3D-Animatoren abhängig von der Größe des Teams. Finden Sie kostenlose 3D Art für Ihr Spiel auf [ http://opengameart.org/ ](http://opengameart.org/) .
- **Spielen Sie Designer** – Spiele Designer sind dafür verantwortlich zu definieren, wie das Spiel gespielt wird. Dazu gehören allgemeine Entscheidungen wie z. B. die Einstellung des Spiels, das Gesamtziel des Spiels, und wie sich ein Spieler das Spiel durch, wechselt. Spiele-Designern können auch sehr detaillierte Entscheidungen wie z. B. die Zuordnung von Eingabe auf Aktionen, beteiligt sein, definieren die Koeffizienten für das Verschieben oder auf Sicherungen und den Entwurf von Level-Layout. Beachten Sie, dass der Begriff *Designer* bezieht sich möglicherweise auf eine Spiele-Designer oder einem visuellen Designer je nach Kontext.
- **Klingt Designer** – Sound Designer für eine audio spielobjekt verantwortlich sind. Einige Teams möglicherweise unterscheiden zwischen Personen, die verantwortlich für das Erstellen von Soundeffekte und Komponisten, während kleinere Teams eine einzelne Person, die für alle Audio verantwortlich sein können.

## <a name="creating-a-game-idea"></a>Erstellen eine Spielidee in Angriff

Entwerfen eines Spiels scheint sich ganz einfach – schließlich die einzige Anforderung ist "etwas lustiges stellen". Leider viele Entwickler finden sich selbst mit Verlust bei Zeit, erstellen eine Vorstellung von der Entwicklung zu starten.

Die Disziplin der game-Entwurf ist einfach nicht erklärt, und muss es sich, wie die Kunst zu verbessern oder Programmierung ist, aber in diesem Abschnitt können Sie den Pfad beginnen.

Neue Entwickler sollte klein gestartet werden. Es kann schwierig sein, widerstehen die Versuchung, ein Videospiel große, moderne, Neuerstellen können jedoch kleinere spielen eine bessere Learning-Umgebung und das schneller durchgeführt wird, für eine weitere lohnende Erfahrung.

Viele Spiele sowohl für learning als auch kommerzieller Spiele, als Verbesserung oder Änderung ein existierendes Spiel beginnen. Eine Möglichkeit, Ideen zu generieren ist anderen Spiele für Inspiration ansehen. Beispielsweise können Sie erwägen, dass ein Spiel, das Sie persönlich gefällt und versuchen Sie, welche Eigenschaften über die Ausführung des Spiels identifizieren Spaß machen. Es kann die Untersuchung, beherrschen des Spiels Mechanismen oder bis zu einer Story sein. Vergessen Sie nicht "retro" spielen auch bei der Suche nach neuen Ideen zu berücksichtigen.

Ein weiteres Verfahren zum Generieren von neuen Ideen ist zu einem bestimmten Genre, wie z. B. Rätsel Spiele, Strategiespiele und sprungbewegungen zu berücksichtigen. Ein Genre vertraut sind, für den Entwickler möglicherweise einen guten Ausgangspunkt bereit.

Die Neugestaltung der vorhandene Spiele ist auch eine Bildungseinrichtung-Erfahrung, obwohl dies dem Endprodukt kommerziellen Lebensfähigkeit eingeschränkt werden kann. Das Verfahren zum Erstellen eines Spiels, selbst eine wird eine genaue klonen, bietet es sich um eine wertvolle Bildungseinrichtungen Erfahrung.

## <a name="game-development-technology"></a>Entwicklung von Spielen-Technologie

Entwickler, die mit Xamarin.Android und Xamarin.iOS haben eine Vielzahl von Technologien, die ihnen bei der Entwicklung von Spielen zur Verfügung. In diesem Abschnitt werden einige der am häufigsten verwendeten plattformübergreifende Lösungen erläutert.

### <a name="cocossharp"></a>CocosSharp

CocosSharp ist ein Open Source- und plattformübergreifende Version die Cocos-2D-Spiele-Engine. Die Engine bietet Zugriff auf Android-, iOS-, Mac OS X, Windows-Desktop, Windows RT und Windows Phone.

CocosSharp konzentriert sich auf einfachen Programmierer API für die Entwicklung von 2D spielen. Das Wachstum der Spiele auf Mobilgeräten hat dazu beigetragen, um die Beliebtheit der Entwicklung von 2D spielen CocosSharp geeignete Technologie für Hobby und für kommerzielle Projekte, die gleichermaßen reignite. Es wird als Quelle Code oder DLL-Dateien (die über NuGet abgerufen werden können) bereitgestellt, aber es bietet einen visuellen Editor keine; aus diesem Grund erfordert Interaktion mit der CocosSharp-Engine Kenntnisse in der Programmierung.

Um den ersten Schritten mit CocosSharp sehen Sie sich unsere [CocosSharp-Handbücher](~/graphics-games/cocossharp/index.md).

Das Spiel verärgerter Ninjas mit CocosSharp erstellt, und es kann ein guter Ausgangspunkt sein, wenn Sie ein bereits ausgeführten-Spiel für mehrere Plattformen suchen:

![](images/image3.png "Das Spiel erstellten verärgerter Ninjas mit CocosSharp")

Sie können es herunterladen und erhalten Sie weitere Informationen unter den [AngryNinjas-Github-Seite](https://github.com/xamarin/AngryNinjas).

### <a name="monogame"></a>MonoGame

MonoGame ist ein Open-Source, cross-Platform-Version von Microsoft XNA-API. MonoGame kann verwendet werden, um Spiele für iOS, Android, Mac OS X, Linux, Windows, Windows RT, PS4, PSVita, Xbox One und Switch zu erstellen.

Im Gegensatz zu CocosSharp ist MonoGame, technisch keine Spiele-Engine, sondern vielmehr eine Spieleentwicklung API. Dies bedeutet, dass die Arbeit mit MonoGame erfordert direkt Spielobjekte zu verwalten, manuell Zeichnen von Objekten und implementieren die allgemeine Objekte wie Kameras und *Szene Diagramme* (der über-/ unterordnungshierarchie zwischen Spielobjekte). Um den Unterschied zu verstehen, sollten erwägen Sie, dass CocosSharp zusätzlich MonoGame erstellt wird. MonoGame generalisiert einiger der Clientplattform-spezifische Technologie, wie Grafiken, Rendering und Audio, während CocosSharp Code zum Organisieren und Implementieren von Spiellogik bereitstellt.

MonoGame bietet keine standard-visuelle Entwicklungsumgebung, arbeiten mit MonoGame erforderlich, dass eine mit programmieren.

Wichtige Beispiele für Spiele mithilfe von MonoGame sind:

FEZ:

![](images/image7.png "FEZ")

Bastion:

![](images/image8.jpg "Bastion")

Zum Arbeiten mit MonoGame, navigieren Sie zum unsere [MonoGame Handbücher](~/graphics-games/monogame/index.md).

### <a name="urhosharp"></a>UrhoSharp

Von UrhoSharp ist eine plattformübergreifende auf hoher Ebene 3D- und 2D-spielen-Engine, die zum Erstellen von animierten im Hintergrund, 2D und 3D für Ihre Anwendungen mit Geometrien, Materialien, Lichter und Kameras verwendet werden kann.

![](images/urhosharp.gif "Von UrhoSharp ist eine plattformübergreifende auf hoher Ebene 3D- und 2D-spielen-Engine, die verwendet werden kann, um animierte 3D- und 2D-spielen Szenen erstellen")

Sehen Sie sich die [von UrhoSharp Handbücher](~/graphics-games/urhosharp/index.md) für den Einstieg.

### <a name="additional-technology"></a>Zusätzliche Technologie

Die oben hervorgehobenen Technologien ist nur ein Beispiel für die Technologien zur Verfügung stehen. Andere wichtigen Technologien gehören:

- **Spritekit** : Xamarin bietet Unterstützung für Apple Spritekit game-Framework, das Sie Zugriff auf alle Funktionen der systemeigenen API erhalten. Spritekit-Technologie von Apple erstellt ist, bietet umfassende Integration in den Rest des iOS-Ökosystems. Spritekit ist natürlich nicht plattformübergreifende, damit es unter Android verwendet werden kann. Weitere Informationen zur Verwendung von Spritekit Siehe diesen Beitrag:  [https://blog.xamarin.com/make-games-with-xamarin.ios-and-sprite-kit/](https://blog.xamarin.com/make-games-with-xamarin.ios-and-sprite-kit/)
- **Szene Kit** : Xamarin bietet auch Unterstützung für Apple Szene Kit Framework vereinfacht die Implementierung von 3D-Grafiken in iOS-apps. Szene Kit ist auch die Technologie, die von Apple bereitgestellt werden, es gelten somit die Integration und plattformspezifischen Überlegungen für Spritekit erwähnt. Weitere Informationen zu Szene Kit Siehe diesen Beitrag: [https://blog.xamarin.com/3d-in-ios-8-with-scene-kit/](https://blog.xamarin.com/3d-in-ios-8-with-scene-kit/)
- **OpenTK –** OpenTK (die für das Öffnen Tool Kit steht) stellt Low-Level OpenGL-Zugriff auf iOS, Mac und Apple Hardware. Weitere Informationen zu OpenTK finden Sie unter der Hauptseite auf:  [http://www.opentk.com/](http://www.opentk.com/)

## <a name="related-links"></a>Verwandte Links

- [CocosSharp-Handbücher](~/graphics-games/cocossharp/index.md)
- [MonoGame-Handbücher](~/graphics-games/monogame/index.md)
- [Von UrhoSharp-Handbücher](~/graphics-games/urhosharp/index.md)
