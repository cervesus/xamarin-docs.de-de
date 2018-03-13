---
title: Android-Ressourcen
description: "Dieser Artikel führt das Konzept von Android-Ressourcen in Xamarin.Android und deren Verwendung, dokumentieren. Sie erfahren, wie auf Ressourcen in der Android-Anwendung verwenden, um die anwendungslokalisierung und mehrere Geräte, die unter unterschiedlichen Bildschirmgrößen und Dichte zu unterstützen."
ms.topic: article
ms.prod: xamarin
ms.assetid: C0DCC856-FA36-04CD-443F-68D26075649E
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/01/2018
ms.openlocfilehash: 6546870d85f7b77e60dff0cb9e6075f982c9cb8e
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="android-resources"></a>Android-Ressourcen

_Dieser Artikel führt das Konzept von Android-Ressourcen in Xamarin.Android und deren Verwendung, dokumentieren. Sie erfahren, wie auf Ressourcen in der Android-Anwendung verwenden, um die anwendungslokalisierung und mehrere Geräte, die unter unterschiedlichen Bildschirmgrößen und Dichte zu unterstützen._


## <a name="overview"></a>Übersicht

Eine Android-Anwendung ist selten nur Quellcode. Es gibt oftmals viele andere Dateien, die sich eine Anwendung zusammensetzt: Video, Bilder, Schriftarten und audio-Dateien, um nur einige zu nennen. Zusammen, sind diese Dateien nicht Quellcode werden als Ressourcen bezeichnet und (zusammen mit den Quellcode) während des Erstellungsprozesses kompiliert und als ein APK für die Verteilung und Installation auf Geräten verpackt:

![Diagramm der Verpackung](images/packaging-diagram.png)

Ressourcen bieten eine Android-Anwendung bietet mehrere Vorteile:

-  **Getrenntem Code** &ndash; Quellcode vom Bildern, Zeichenfolgen, Menüs, Animationen, Farben usw. trennt. Daher können Ressourcen deutlich beim Lokalisieren von.

-  **Mehrere Zielgeräte** &ndash; bietet einfachere Unterstützung für verschiedene Gerätekonfigurationen ohne codeänderungen.

-  **Zeitpunkt der Kompilierung Checking** &ndash; Ressourcen sind statisch und kompilierten der Anwendung. Dadurch wird die Verwendung der Ressourcen, die zum Zeitpunkt der Kompilierung überprüft werden, wenn er kann einfach zu erkennen und Korrigieren von Fehlern, im Gegensatz zur Laufzeit, wenn eine höhere schwer zu lokalisieren und zu korrigieren ist.

Wenn ein neues Xamarin.Android-Projekt gestartet wird, wird ein spezielles Verzeichnis mit dem Namen der Ressourcen zusammen mit einigen Unterverzeichnisse erstellt:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Ressourcenordner und Inhalt](images/resources-folder-vs.png)

In der obigen Abbildung sind die Ressourcen der Anwendung entsprechend ihrem Typ in diese Unterverzeichnisse organisiert: Images geht die **zeichenbaren** Verzeichnis, Ansichten, wechseln Sie der **Layout** Unterverzeichnis usw.
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

![Ressourcenordner und Inhalt](images/resources-folder-xs.png)

In der obigen Abbildung sind die Ressourcen der Anwendung entsprechend ihrem Typ in diese Unterverzeichnisse organisiert: Images geht die **Mipmap** Verzeichnis, Ansichten, wechseln Sie der **Layout** Unterverzeichnis usw.
 
-----

Es gibt zwei Möglichkeiten, den Zugriff auf diese Ressourcen in einer Anwendung Xamarin.Android: *programmgesteuert* im Code und *deklarativ* in XML-Datei, die eine spezielle XML-Syntax verwenden.

Diese Ressourcen werden mit dem Namen *Standardressourcen* und werden von allen Geräten verwendet, es sei denn, eine genauere Übereinstimmung angegeben wird. Darüber hinaus kann jede Art von Ressource optional haben *alternativen Ressourcen* , dass Android verwenden kann, um bestimmte Geräte abzielen. Beispielsweise können Ressourcen bereitgestellt werden, um Gebietsschema des Benutzers, die Größe des Bildschirms, als Ziel usw. oder wenn das Gerät aus auf Querformat, Hochformat um 90 Grad gedreht wird. In jedem dieser Fälle wird die Ressourcen für die Verwendung in Android von der Anwendung ohne zusätzlichen Codierungsaufwand vom Entwickler geladen werden.

Alternative Ressourcen durch Hinzufügen einer kurzen Zeichenfolge mit dem Namen angegeben sind eine *Qualifizierer*, bis zum Ende des Verzeichnisses einen bestimmten Typ von Ressourcen enthalten.

Beispielsweise **Ressourcen/zeichenbaren-de** wird Festlegen der Bilder für Geräte, die auf einem deutschen Gebietsschema festgelegt sind während **Ressourcen/zeichenbaren-fr** würde Bilder enthalten, für Geräte auf einer deutschen gebietsschemaeinstellung festgelegt. Ein Beispiel für das Bereitstellen alternativer Ressourcen kann angezeigt werden, in der Abbildung unten mit nur das Gebietsschema, das Gerät ändern, in dem dieselbe Anwendung ausgeführt wurde:

![Beispiel-Bildschirme für verschiedene Gebietsschemas](images/localized-screenshots.png)

In diesem Artikel wird nutzen einen umfassenden Einblick in die Verwendung von Ressourcen und behandelt die folgenden Themen:

-  **Android Ressource Grundlagen** &ndash; mit Standardressourcen programmgesteuert und deklarativ Ressourcentypen wie Bilder und Schriftarten zu einer Anwendung hinzufügen.

-  **Bestimmte Konfigurationen zu gerätesicherheit** &ndash; Unterstützung der verschiedenen bildschirmauflösungen und Dichte in einer Anwendung.

-  **Lokalisierung** &ndash; Ressourcen verwenden, um die verschiedenen Regionen zu unterstützen, eine Anwendung verwendet werden kann.


## <a name="related-links"></a>Verwandte Links

- [Verwenden von Android-Ressourcen](~/android/app-fundamentals/resources-in-android/android-assets.md)
- [Application Fundamentals (Anwendungsgrundlagen)](http://developer.android.com/guide/topics/fundamentals.html)
- [Anwendungsressourcen](http://developer.android.com/guide/topics/resources/index.html)
- [Unterstützung von mehreren Bildschirmen](http://developer.android.com/guide/practices/screens_support.html)
