---
title: Introduction to Content Pipelines
description: "Inhaltspaket Pipelines Anwendungen oder Teile der Anwendung sind, dienen, die Dateien in ein Format zu konvertieren, die von Spiel Projekte geladen werden können. Pipeline Inhalt MonoGame handelt es sich um eine bestimmte Inhalte Pipeline-Implementierung zum Konvertieren von Dateien für CocosSharp und MonoGame-Projekte."
ms.topic: article
ms.prod: xamarin
ms.assetid: 40628B5F-FAF7-4FA7-A929-6C3FEA83F8EC
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/27/2017
ms.openlocfilehash: d51852924a4d909857659d38f8c19d520bb4c589
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="introduction-to-content-pipelines"></a>Introduction to Content Pipelines

_Inhaltspaket Pipelines Anwendungen oder Teile der Anwendung sind, dienen, die Dateien in ein Format zu konvertieren, die von Spiel Projekte geladen werden können. Pipeline Inhalt MonoGame handelt es sich um eine bestimmte Inhalte Pipeline-Implementierung zum Konvertieren von Dateien für CocosSharp und MonoGame-Projekte._

Dieser Artikel bietet ein über ein grundlegendes Verständnis von Inhalt Pipelines, wobei schwerpunktmäßig in erster Linie auf die die *MonoGame Inhalt Pipeline*, wobei es sich ein Content Pipeline-Implementierung mit CocosSharp und MonoGame verwendet.


# <a name="what-is-a-content-pipeline"></a>Was ist eine Content Pipeline?

Der Begriff *Content Pipeline* ist ein allgemeiner Begriff für eine Datei aus einem Format in eine andere konvertiert. Die *input* der Content-Pipeline ist in der Regel eine Datei, die von einem Erstellungstool, wie Bilddateien aus Photoshop ausgegeben. Die Content-Pipeline erstellt die *Ausgabe* Datei in einem Format, die direkt von einem Spiel Projekt geladen werden kann. In der Regel die Ausgabedateien für schnelles Laden optimiert sind und reduziert die Größe des Datenträgers.

Content-Pipelines verwenden möglicherweise eingebettete ausführbare Dateien, dedizierten GUI-basierte Anwendungen oder -Plug-Ins in einer anderen Anwendung wie z. B. Visual Studio. Content-Pipelines werden normalerweise auf, bevor das Spiel ausgeführt wird. Wenn die Content-Pipeline mit einer Anwendung wie Visual Studio verknüpft ist, können sie während der Kompilierung ausführen. Wenn die Content-Pipeline eine eigenständige Anwendung ist, kann diese ausgeführt werden wenn explizit vom Benutzer dazu angewiesen. Die Anwendung oder eine Logik, die verantwortlich für das Konvertieren einer bestimmten Eingabedatei (z. B. eine **PNG**) auf eine entsprechende Ausgabe wird bezeichnet als eine *Builder*. 

Wir können den Pfad visuell darstellen, den eine Datei von der Erstellung zur Laufzeit geladen werden akzeptiert:

![](introduction-images/image1.png "Der Pfad, den eine Datei verwendet wird, von der Erstellung, die zur Laufzeit geladen wird, wird in diesem Diagramm veranschaulicht.")

# <a name="why-use-a-content-pipeline"></a>Gründe für die Verwendung von Content-Pipeline

Content-Pipelines führen einen zusätzlichen Schritt zwischen der authoring Anwendung und das Spiel, die kompilierzeiten zu erhöhen und Komplexität des Entwicklungsprozesses hinzufügen können. Trotz dieser Faktoren stellen Content Pipelines eine Reihe von Vorteilen für 3D-Spielentwicklung an:


## <a name="converting-to-a-format-understood-by-the-game"></a>Konvertierung in ein Format, das Spiel verständlich

Geben Sie Methoden zum Laden von verschiedenen Typen von Inhalt, CocosSharp und MonoGame; Allerdings muss der Inhalt korrekt formatiert sein, bevor Sie geladen wird. Die meisten Typen von Inhalten erfordern einige Art der Konvertierung, bevor Sie geladen wird. Beispielsweise Soundeffekten in der **.wav** Format konvertiert werden muss, in eine **.xnb** Datei, die zur Laufzeit geladen werden, da CocosSharp und MonoGame laden unterstützen die **.wav** Dateiformat.


## <a name="converting-to-a-format-native-to-the-hardware"></a>Konvertieren in ein systemeigenes Format der Hardware

Unterschiedliche Hardware Inhalt möglicherweise zur Laufzeit anders behandeln. Beispielsweise können CocosSharp Spiele Bilddateien laden, beim Erstellen einer `CCSprite` Instanz. Obwohl der gleiche Code zum Laden der Dateien auf IOS- und Android verwendet werden kann, speichert jede Plattform die geladene Datei unterschiedlich. Daher formatiert die MonoGame Content Pipeline Textur **.xnb** Dateien anders abhängig von der Zielplattform.


## <a name="reducing-size-on-disk"></a>Reduzieren der Größe auf Datenträger 

Inhaltspaket Pipelines verwendet werden können, um Informationen zu entfernen, die nützlich zur Erstellungszeit, jedoch nicht zur Laufzeit erforderlich ist. Die ursprüngliche (Eingabedatei) kann alle Informationen, die Autoren vorhandene Inhalte verwalten kann, speichern, aber die Ausgabedatei kann abgespeckte insgesamt Spiele Datei gering gehalten werden. Diese Überlegungen sind besonders nützlich für mobile Spiele, die werden heruntergeladen, sondern auf den Installationsmedien verteilt.


## <a name="reducing-load-time"></a>Ladezeit reduzieren

Spiele erfordern möglicherweise Änderungen des Inhalts zur Verbesserung der Leistung zur Laufzeit, um visuelle Elemente zu verbessern oder neue Funktionen hinzufügen. Z. B. viele 3D-Spielen Beleuchtung einmal zu berechnen, verwenden Sie das Ergebnis der Berechnung bei komplexe Szenen zu rendern. Da diese Berechnungen ausführen, wenn beim Laden von Inhalt ungeheuer teuer sein kann kann die Berechnung stattdessen ausgeführt werden, wenn das Spiel erstellt wird. Die resultierenden Berechnungen können eingefügt werden, im Inhalt, aktivieren den Inhalt zu ladende viel schneller, als andernfalls möglich wäre. 


# <a name="xnb-file-extension"></a>XNB-Erweiterung

Die **.xnb** Erweiterung der Ausgabedatei lautet die Erweiterung für alle Dateien, die von der Monogame Content Pipeline ausgegeben. Dies entspricht der Erweiterung von Dateien, die von Microsoft XNA Content Pipeline ausgegeben.

Die **.xnb** Erweiterung wird unabhängig von dem ursprünglichen Dateityp verwendet. In anderen Worten: Bilddateien (**PNG**), audio-Dateien (**.wav**), und alle benutzerdefinierten Dateitypen werden alle als ausgegeben werden. **.xnb** -Dateien, wenn Sie über die Pipeline Content übergeben. Da die Erweiterung kann nicht verwendet werden, unterscheiden unterschiedliche Dateiformate dann CocosSharp und MonoGame Methoden, die laden **.xnb** Dateien erwarten nicht Erweiterungen beim Laden der Datei.

CocosSharp und MonoGame .xnb-Dateien können erstellt werden, mithilfe der Monogame pipelinetool der behandelt wird [in dieser exemplarischen Vorgehensweise](~/graphics-games/cocossharp/content-pipeline/walkthrough.md).


# <a name="summary"></a>Zusammenfassung

In diesem Artikel bereitgestellt eine Übersicht und Vorteile von Inhalt Pipelines im Allgemeinen sowie eine Einführung in die MonoGame Inhalt Pipeline.

## <a name="related-links"></a>Verwandte Links

- [MonoGame Pipeline-Dokumentation](http://www.monogame.net/documentation/?page=Pipeline)
