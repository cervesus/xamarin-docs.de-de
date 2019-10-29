---
title: Android-Ressourcen
description: In diesem Artikel wird das Konzept der Android-Ressourcen in xamarin. Android vorgestellt, und es wird beschrieben, wie Sie verwendet werden. Es wird erläutert, wie Ressourcen in Ihrer Android-Anwendung zur Unterstützung der Lokalisierung von Anwendungen und mehrerer Geräte mit unterschiedlichen Bildschirmgrößen und dichten verwendet werden.
ms.prod: xamarin
ms.assetid: C0DCC856-FA36-04CD-443F-68D26075649E
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/01/2018
ms.openlocfilehash: 1ff4d9d896aaa5f290402a49aa4b4bd1f1e00aaf
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73025058"
---
# <a name="android-resources"></a>Android-Ressourcen

_In diesem Artikel wird das Konzept der Android-Ressourcen in xamarin. Android vorgestellt, und es wird beschrieben, wie Sie verwendet werden. Es wird erläutert, wie Ressourcen in Ihrer Android-Anwendung zur Unterstützung der Lokalisierung von Anwendungen und mehrerer Geräte mit unterschiedlichen Bildschirmgrößen und dichten verwendet werden._

## <a name="overview"></a>Übersicht

Eine Android-Anwendung ist nur selten Quellcode. Es gibt oft viele andere Dateien, aus denen eine Anwendung besteht: Video, Bilder, Schriftarten und Audiodateien, um nur einige zu nennen. Gemeinsam werden diese nicht-Quell Code Dateien als Ressourcen bezeichnet und während des Buildprozesses (zusammen mit dem Quellcode) kompiliert und als APK für die Verteilung und Installation auf Geräten verpackt:

![Verpackungs Diagramm](images/packaging-diagram.png)

Ressourcen bieten mehrere Vorteile für eine Android-Anwendung:

- **Code Trennung** &ndash; trennt Quellcode von Bildern, Zeichen folgen, Menüs, Animationen, Farben usw. Da solche Ressourcen bei der Lokalisierung erheblich helfen können.

- **Mehrere Geräte als Ziel** &ndash; bietet einfachere Unterstützung verschiedener Geräte Konfigurationen ohne Codeänderungen.

- Die über **Prüfung der Kompilierzeit** &ndash; Ressourcen ist statisch und wird in die Anwendung kompiliert. Dadurch kann die Verwendung der Ressourcen zum Zeitpunkt der Kompilierung geprüft werden, wenn die Fehler leicht abgefangen und korrigiert werden können, im Gegensatz zur Laufzeit, wenn Sie schwieriger zu finden und zu korrigieren ist.

Wenn ein neues xamarin. Android-Projekt gestartet wird, wird ein spezielles Verzeichnis mit dem Namen "Resources" (Ressourcen) zusammen mit einigen Unterverzeichnissen erstellt:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![Ordner und Inhalt von Ressourcen](images/resources-folder-vs.png)

In der obigen Abbildung werden die Anwendungs Ressourcen nach ihrem Typ in diesen Unterverzeichnissen organisiert: Bilder werden in das **drawable** -Verzeichnis geleitet. Sichten werden im **layoutunterverzeichnis** usw. angezeigt.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

![Ordner und Inhalt von Ressourcen](images/resources-folder-xs.png)

In der obigen Abbildung werden die Anwendungs Ressourcen nach ihrem Typ in diesen Unterverzeichnissen organisiert: Bilder werden in das **MipMap** -Verzeichnis geleitet. Sichten werden im **layoutunterverzeichnis** usw. angezeigt.

-----

Es gibt zwei Möglichkeiten, auf diese Ressourcen in einer xamarin. Android-Anwendung zuzugreifen: *Programm* gesteuert in Code und *deklarativ* in XML mithilfe einer speziellen XML-Syntax.

Diese Ressourcen werden als *Standard Ressourcen* bezeichnet und werden von allen Geräten verwendet, es sei denn, es wird eine spezifischere Übereinstimmung angegeben. Darüber hinaus kann jeder Ressourcentyp optional *Alternative Ressourcen* aufweisen, die von Android für bestimmte Geräte verwendet werden können. So können z. b. Ressourcen bereitgestellt werden, um das Gebiets Schema des Benutzers, die Bildschirmgröße oder die Größe des Geräts um 90 Grad von Hochformat in Querformat usw. zu richten. In jedem dieser Fälle lädt Android die Ressourcen, die von der Anwendung verwendet werden können, ohne zusätzlichen Codierungsaufwand durch den Entwickler.

Alternative Ressourcen werden angegeben, indem eine kurze Zeichenfolge, die als *Qualifizierer*bezeichnet wird, am Ende des Verzeichnisses mit einem bestimmten Typ von Ressourcen hinzugefügt wird.

Beispielsweise geben **Ressourcen/drawable-de** die Images für Geräte an, die auf ein deutsches Gebiets Schema festgelegt sind, während **Ressourcen/drawable-fr** Bilder für Geräte enthalten, die auf ein Französisch Gebiets Schema festgelegt sind. Ein Beispiel für die Bereitstellung alternativer Ressourcen finden Sie in der folgenden Abbildung, in der dieselbe Anwendung mit nur dem Gebiets Schema des Geräts ausgeführt wird, das sich ändert:

![Beispiel Bildschirme für verschiedene Gebiets Schemas](images/localized-screenshots.png)

Dieser Artikel bietet einen umfassenden Einblick in die Verwendung von Ressourcen und deckt die folgenden Themen ab:

- **Grundlagen der Android-Ressourcen** &ndash; die programmgesteuerte Verwendung von Standard Ressourcen und die deklarative Verwendung von Ressourcentypen wie Bildern und Schriftarten zu einer Anwendung.

- **Gerätespezifische Konfigurationen** &ndash; Unterstützung der verschiedenen Bildschirmauflösungen und dichten in einer Anwendung.

- **Lokalisierungs** &ndash; Verwenden von Ressourcen zur Unterstützung der verschiedenen Regionen, für die eine Anwendung verwendet werden kann.

## <a name="related-links"></a>Verwandte Links

- [Verwenden von Android-Ressourcen](~/android/app-fundamentals/resources-in-android/android-assets.md)
- [Application Fundamentals (Anwendungsgrundlagen)](https://developer.android.com/guide/topics/fundamentals.html)
- [Anwendungsressourcen](https://developer.android.com/guide/topics/resources/index.html)
- [Unterstützen mehrerer Bildschirme](https://developer.android.com/guide/practices/screens_support.html)
