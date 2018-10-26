---
title: Einführung in inhaltspipelines
description: Inhalt, dass Pipelines Anwendungen oder Teile der Anwendungen sind, dienen, um Dateien in ein Format zu konvertieren, die von Spielen Projekte geladen werden kann. MonoGame Content Pipeline handelt es sich um eine bestimmte Pipeline für Bildinhalte-Implementierung für die Konvertierung von Dateien für Projekte von CocosSharp "und" MonoGame ".
ms.prod: xamarin
ms.assetid: 40628B5F-FAF7-4FA7-A929-6C3FEA83F8EC
author: conceptdev
ms.author: crdun
ms.date: 03/27/2017
ms.openlocfilehash: 712c430fb6309ba0f5c3e573267c59e422de8ad2
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50104140"
---
# <a name="introduction-to-content-pipelines"></a>Einführung in inhaltspipelines

_Inhalt, dass Pipelines Anwendungen oder Teile der Anwendungen sind, dienen, um Dateien in ein Format zu konvertieren, die von Spielen Projekte geladen werden kann. MonoGame Content Pipeline handelt es sich um eine bestimmte Pipeline für Bildinhalte-Implementierung für die Konvertierung von Dateien für Projekte von CocosSharp "und" MonoGame "._

Dieser Artikel bietet einen konzeptionellen Überblick über die inhaltspipelines, beschäftigt sich hauptsächlich mit der *MonoGame Content Pipeline*, d.h., dass eine Pipeline für Bildinhalte-Implementierung mit CocosSharp "und" MonoGame "verwendet.


## <a name="what-is-a-content-pipeline"></a>Was ist eine Pipeline für Bildinhalte?

Der Begriff *Pipeline für Bildinhalte* ist ein allgemeiner Begriff für den Prozess der Konvertierung von einer Datei von einem Format in eine andere. Die *Eingabe* von der Pipeline für Bildinhalte ist in der Regel eine Datei, die von einem Erstellungstool, z. B. Bilddateien aus Photoshop ausgegeben. Das von der Pipeline erstellt den *Ausgabe* -Datei in ein Format, das direkt von einem Spiel Projekt geladen werden kann. In der Regel der Ausgabedateien sind optimiert für schnelles Laden und reduziert die Größe des Datenträgers.

Inhalt, dass Pipelines möglicherweise Befehlszeile ausführbare Dateien, dedizierte GUI-basierte Anwendungen oder -Plug-Ins eingebettet, in einer anderen Anwendung wie z.B. Visual Studio. Inhaltspipelines führen in der Regel auf, bevor das Spiel ausgeführt wird. Wenn das von der Pipeline mit einer Anwendung wie Visual Studio verknüpft ist, kann es während der Kompilierung führen. Ist die Pipeline für Bildinhalte eine eigenständige Anwendung, kann er ausgeführt, wenn explizit dazu angewiesen, durch den Benutzer. Die Anwendung oder die Logik für die Konvertierung von einer bestimmten Eingabedatei verantwortlich (z. B. eine **PNG**) auf einer zugeordneten Datei wird als bezeichnet ein *Builder*. 

Wir können den Pfad visuell darstellen, den eine Datei verwendet wird, von der Erstellung wie folgt zur Laufzeit geladen werden:

![](introduction-images/image1.png "Der Pfad, den eine Datei verwendet wird, vom erstellen, die zur Laufzeit geladen wird, wird in diesem Diagramm veranschaulicht.")

## <a name="why-use-a-content-pipeline"></a>Warum verwenden Sie eine Pipeline für Bildinhalte?

Inhaltspipelines führen einen zusätzlichen Schritt zwischen entwicklungsanwendung und das Spiel die Kompilierdauer zu erhöhen und den Entwicklungsprozess Komplexität hinzufügen können. Trotz dieser Überlegungen stellen eine Reihe von Vorteilen von inhaltspipelines zur Spieleentwicklung vor:


### <a name="converting-to-a-format-understood-by-the-game"></a>Das Spiel verständlich in ein Format konvertieren

Geben Methoden zum Laden von verschiedenen Arten von Inhalten, CocosSharp "und" MonoGame "; jedoch muss der Inhalt korrekt formatiert sein, bevor Sie geladen wird. Die meisten Arten von Inhalten benötigen eine Art der Konvertierung, bevor Sie geladen wird. Z. B. Soundeffekten in die **WAV** Format konvertiert werden muss, in eine **.xnb** Datei, die zur Laufzeit geladen werden, da CocosSharp "und" MonoGame "nicht laden unterstützen die **WAV** Dateiformat.


### <a name="converting-to-a-format-native-to-the-hardware"></a>Konvertierung in ein Format, die in der hardware

Unterschiedliche Hardware Inhalte möglicherweise zur Laufzeit anders behandeln. CocosSharp-Spiele können Laden Sie z. B. Bilddateien beim Erstellen einer `CCSprite` Instanz. Zwar der gleiche Code zum Laden der Dateien für iOS und Android verwendet werden kann, werden die geladene Datei in jede Plattform anders speichert. Infolgedessen wird die Pipeline für Bildinhalte MonoGame Textur formatiert **.xnb** Dateien unterschiedlich je nach Zielplattform.


### <a name="reducing-size-on-disk"></a>Reduziert die Größe auf Datenträger 

Inhalt, dass Pipelines verwendet werden können, um Informationen zu entfernen, d.h. nützlich zum Zeitpunkt der Autor, aber nicht zur Laufzeit erforderlich sind. Die ursprüngliche (Eingabedatei) speichert alle Informationen dem Inhaltsersteller vorhandene Inhalte verwalten kann, die Ausgabedatei kann jedoch sein reduzierte, um die Datei des Spiels gering zu halten. Dieser Aspekt ist besonders nützlich für mobile Spiele, die werden heruntergeladen, sondern auf den Installationsmedien verteilt.


### <a name="reducing-load-time"></a>Reduzierung der Ladezeit

Spiele erfordern möglicherweise Änderungen des Inhalts zur Verbesserung der Leistung zur Laufzeit, um visuelle Elemente zu verbessern oder neue Funktionen hinzuzufügen. Z. B. viele 3D-Spiele Beleuchtung einmal zu berechnen, und verwenden das Ergebnis dieser Berechnung aus, wenn komplexe Szenen rendern. Da diese Berechnungen ausführen, wenn nur sehr teuer, Laden von Inhalten sein kann kann stattdessen die Berechnung ausgeführt werden, wenn das Spiel erstellt wird. Die resultierenden Berechnungen können enthalten sein, den Inhalt, und aktivieren den Inhalt zu ladende viel schneller, als andernfalls möglich wäre. 


## <a name="xnb-file-extension"></a>Xnb-Erweiterung

Die **.xnb** Dateierweiterung ist die Erweiterung für alle Dateien, die von der Pipeline für Bildinhalte Monogame ausgegeben. Dies entspricht die Erweiterung der Dateien, die von Microsoft XNA Pipeline für Bildinhalte ausgegeben.

Die **.xnb** Erweiterung wird unabhängig von dem ursprünglichen Dateityp verwendet. Das heißt, Bilddateien (**PNG**), Audiodateien (**WAV**), und alle benutzerdefinierten Dateitypen werden alle als ausgegeben werden **.xnb** -Dateien, wenn Sie über die Pipeline für Bildinhalte übergeben. Da die Erweiterung kann nicht verwendet werden, unterscheiden unterschiedliche Dateiformate, klicken Sie dann sowohl "CocosSharp" und "MonoGame" Methoden die laden **.xnb** Dateien erwarten Sie nicht nur Erweiterungen beim Laden der Datei.

CocosSharp "und" MonoGame ".xnb-Dateien können erstellt werden, mit dem Monogame-Pipeline-Tool die beschrieben wird [in dieser exemplarischen Vorgehensweise](~/graphics-games/cocossharp/content-pipeline/walkthrough.md).


## <a name="summary"></a>Zusammenfassung

Dieser Artikel enthält eine Übersicht und Vorteile von inhaltspipelines im Allgemeinen sowie eine Einführung in die MonoGame Content Pipeline.

## <a name="related-links"></a>Verwandte Links

- [MonoGame-Pipeline-Dokumentation](http://www.monogame.net/documentation/?page=Pipeline)
