---
title: Android-Ressourcen
description: Dieser Artikel führt das Konzept der Android-Ressourcen in Xamarin.Android und wird deren Verwendung zu dokumentieren. Es wird beschrieben, wie Ressourcen in Ihrer Android-Anwendung verwenden, um anwendungslokalisierung und mehrere Geräte aus unterschiedlichen Bildschirmgrößen und dichten zu unterstützen.
ms.prod: xamarin
ms.assetid: C0DCC856-FA36-04CD-443F-68D26075649E
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/01/2018
ms.openlocfilehash: d9cd6bf3ae51c6e27be88481e412995bd4113c17
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50117257"
---
# <a name="android-resources"></a>Android-Ressourcen

_Dieser Artikel führt das Konzept der Android-Ressourcen in Xamarin.Android und wird deren Verwendung zu dokumentieren. Es wird beschrieben, wie Ressourcen in Ihrer Android-Anwendung verwenden, um anwendungslokalisierung und mehrere Geräte aus unterschiedlichen Bildschirmgrößen und dichten zu unterstützen._


## <a name="overview"></a>Übersicht

Eine Android-Anwendung ist selten einfach Quellcode. Es gibt viele andere Dateien, aus denen eine Anwendung häufig: Video, Bilder, Schriftarten und Audiodateien, um nur einige zu nennen. Zusammen, werden diese Dateien ohne Quellcode werden als Ressourcen bezeichnet (zusammen mit dem Quellcode) während des Erstellungsprozesses kompiliert und als ein APK für die Verteilung und Installation auf Geräten:

![Packaging-Diagramm](images/packaging-diagram.png)

Ressourcen bieten eine Android-Anwendung bietet mehrere Vorteile:

-  **Codetrennung** &ndash; Quellcode von Bildern, Zeichenfolgen, Menüs, Animationen, Farben usw. getrennt. Daher können Ressourcen erheblich beim Lokalisieren von.

-  **Mehrere Zielgeräte schreiben** &ndash; bietet einfachere Unterstützung unterschiedlicher Gerätekonfigurationen ohne Änderungen am Code.

-  **Während der Kompilierung Checking** &ndash; Ressourcen sind statisch und kompilierten bei der Anwendung. Dies ermöglicht die Nutzung der Ressourcen, die zum Zeitpunkt der Kompilierung überprüft werden, wenn es sich handelt, abfangen und beheben Sie die Fehler, im Gegensatz zur Laufzeit, wenn sie schwer zu lokalisieren und korrigieren teuer ist einfach.

Wenn ein neues Xamarin.Android-Projekt gestartet wird, wird ein speziellen Verzeichnisses namens Ressourcen sowie einige Unterverzeichnisse erstellt:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![Ordner "Resources" und Inhalt](images/resources-folder-vs.png)

In der Abbildung oben sind die Ressourcen der Anwendung entsprechend ihrem Typ in diese Unterverzeichnisse organisiert: Images geht die **drawable** Verzeichnis, Ansichten, wechseln Sie der **Layout** Unterverzeichnis usw.
 
# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

![Ordner "Resources" und Inhalt](images/resources-folder-xs.png)

In der Abbildung oben sind die Ressourcen der Anwendung entsprechend ihrem Typ in diese Unterverzeichnisse organisiert: Images geht die **Mipmap** Verzeichnis, Ansichten, wechseln Sie der **Layout** Unterverzeichnis usw.
 
-----

Es gibt zwei Möglichkeiten, den Zugriff auf diese Ressourcen in einer Xamarin.Android-Anwendung: *programmgesteuert* im Code und *deklarativ* im XML-Format mit einer speziellen XML-Syntax.

Diese Ressourcen benannt sind *Standardressourcen* und von allen Geräten verwendet werden, es sei denn, eine genauere Übereinstimmung angegeben wird. Darüber hinaus kann jede Art von Ressource optional haben *alternative Ressourcen* , dass Android verwenden, um bestimmte Geräte als Ziele. Beispielsweise können Ressourcen angegeben werden, das Gebietsschema des Benutzers, die Größe des Bildschirms, als Ziel usw., oder wenn das Gerät vom Hochformat zum Querformat, um 90 Grad gedreht wird. In jedem dieser Fälle lädt Android den Ressourcen für die Verwendung durch die Anwendung, ohne alle zusätzlichen Aufwand beim Codeschreiben vom Entwickler.

Alternative Ressourcen werden durch das Hinzufügen einer kurzen Zeichenfolge, mit dem Namen angegeben, dass eine *Qualifizierer*, am Ende des Verzeichnisses, das eine bestimmte Art von Ressourcen enthält.

Z. B. **Ressourcen/drawable-de** wird Festlegen der Bilder für Geräte, die auf einem deutschen Gebietsschema festgelegt sind zwar **Ressourcen/drawable-fr** würde Bilder enthalten, für Geräte auf einer deutschen gebietsschemaeinstellung festgelegt. Ein Beispiel für die Bereitstellung von anderer Ressourcen kann angezeigt werden, in der Abbildung unten nur auf dem Gebietsschema des Geräts ändern, in dem dieselbe Anwendung ausgeführt wird:

![Beispiel-Bildschirme für verschiedene Gebietsschemas](images/localized-screenshots.png)

In diesem Artikel übernimmt einen umfassenden Überblick über die Verwendung von Ressourcen und in den folgenden Themen behandelt:

-  **Grundlagen der Android-Ressource** &ndash; mit Standardressourcen und zwar programmgesteuert sowie deklarativ, Hinzufügen von Ressourcentypen, z. B. Bilder und Schriftarten zu einer Anwendung.

-  **Bestimmte Gerätekonfigurationen** &ndash; verschiedene bildschirmauflösungen und dichten in einer Anwendung unterstützt.

-  **Lokalisierung** &ndash; Ressourcen verwenden, um die verschiedenen Regionen zu unterstützen, eine Anwendung verwendet werden kann.


## <a name="related-links"></a>Verwandte Links

- [Verwenden von Android-Ressourcen](~/android/app-fundamentals/resources-in-android/android-assets.md)
- [Application Fundamentals (Anwendungsgrundlagen)](http://developer.android.com/guide/topics/fundamentals.html)
- [Anwendungsressourcen](http://developer.android.com/guide/topics/resources/index.html)
- [Unterstützung von mehreren Bildschirmen](http://developer.android.com/guide/practices/screens_support.html)
