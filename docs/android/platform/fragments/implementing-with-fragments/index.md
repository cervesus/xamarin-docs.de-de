---
title: 'Implementieren von Fragmenten: Exemplarische Vorgehensweise'
description: In diesem Artikel wird die Verwendung von Fragmenten zum Entwickeln von Xamarin.Android-Anwendungen erläutert.
ms.topic: tutorial
ms.prod: xamarin
ms.assetid: A71E9D87-CB69-10AB-CE51-357A05C76BCD
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 04/26/2018
ms.openlocfilehash: b601fc37cc75dcd43c3688de8d302f0a47a06b35
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2020
ms.locfileid: "73027420"
---
# <a name="implementing-fragments---walkthrough"></a>Implementieren von Fragmenten: Exemplarische Vorgehensweise

_Fragmente sind eigenständige, modulare Komponenten, die dabei helfen können, die Komplexität von Android-Apps zu verringern, die auf Geräte mit einer Vielzahl von Bildschirmgrößen abzielen. In diesem Artikel wird die Erstellung und Verwendung von Fragmenten zum Entwickeln von Xamarin.Android-Anwendungen erläutert._

## <a name="overview"></a>Übersicht

In diesem Abschnitt wird die Erstellung und Verwendung von Fragmenten in einer Xamarin.Android-Anwendung schrittweise erläutert. Diese Anwendung zeigt die Titel mehrerer Stücke von William Shakespeare in einer Liste an. Wenn der Benutzer auf den Titel eines Stücks tippt, wird in der App ein Zitat aus diesem Stück in einer separaten Aktivität angezeigt:

[![App, die auf einem Android-Smartphone im Hochformatmodus ausgeführt wird](./images/intro-screenshot-phone-sml.png)](./images/intro-screenshot-phone.png#lightbox)

Wenn das Smartphone in den Querformatmodus gedreht wird, ändert sich die Darstellung der App: Die Liste der Spiele und die Zitate werden in derselben Aktivität angezeigt. Wenn ein Stück ausgewählt wird, wird das Zitat in der gleichen Aktivität angezeigt:

[![App, die auf einem Android-Smartphone im Querformatmodus ausgeführt wird](./images/intro-screenshot-phone-land-sml.png)](./images/intro-screenshot-phone-land.png#lightbox)

Schließlich wird die App auf einem Tablet ausgeführt:

[![App, die auf einem Android-Tablet ausgeführt wird](./images/intro-screenshot-tablet-sml.png)](./images/intro-screenshot-tablet.png#lightbox)

Mithilfe von Fragmenten und [alternativen Layouts](/xamarin/android/app-fundamentals/resources-in-android/alternate-resources) können Sie diese Beispielanwendung mit minimalen Codeänderungen problemlos an die verschiedenen Formfaktoren und Ausrichtungen anpassen.

Die Daten für die Anwendung sind in zwei Zeichenfolgenarrays vorhanden, die in der App als C#-Zeichenfolgenarrays hartcodiert sind. Jedes der Arrays dient als Datenquelle für ein Fragment.  Ein Array enthält den Namen einiger Stücke von Shakespeare, das andere Array enthält ein Zitat aus dem Stück. Wenn die App gestartet wird, werden die Namen der Stücke in einem `ListFragment` angezeigt. Wenn der Benutzer auf ein Stück im `ListFragment` klickt, startet die App eine weitere Aktivität, die das Zitat anzeigt.

Die Benutzeroberfläche für die App besteht aus zwei Layouts: ein Layout für den Hochformat- und ein Layout für den Querformatmodus. Zur Laufzeit bestimmt Android, welches Layout basierend auf der Ausrichtung des Geräts geladen werden soll, und stellt das Layout für die zu rendernde Aktivität bereit. Die gesamte Logik für die Reaktion auf Benutzerklicks und die Anzeige der Daten ist in Fragmenten enthalten. Die Aktivitäten in der App sind nur als Container vorhanden, in denen die Fragmente gehostet werden.

Diese exemplarische Vorgehensweise wird in zwei Leitfäden aufgeteilt. Der [erste Teil](./walkthrough.md) konzentriert sich auf den Kern der Anwendung. Es wird ein einziger Satz von Layouts (optimiert für den Hochformatmodus) erstellt, zusammen mit zwei Fragmenten und zwei Aktivitäten:

1. `MainActivity` &nbsp; Dies ist die Startaktivität für die App.
1. `TitlesFragment` &nbsp; Dieses Fragment zeigt eine Liste mit Titeln von Stücken an, die von William Shakespeare geschrieben wurden. Es wird von `MainActivity` gehostet.
1. `PlayQuoteActivity` &nbsp; `TitlesFragment` startet die `PlayQuoteActivity` als Reaktion auf den Benutzer, der ein Stück in `TitlesFragment` auswählt.
1. `PlayQuoteFragment` &nbsp; Dieses Fragment zeigt ein Zitat aus einem Stück von William Shakespeare an. Es wird von `PlayQuoteActivity` gehostet.

Im [zweiten Teil dieser exemplarischen Vorgehensweise](./walkthrough-landscape.md) wird das Hinzufügen eines alternativen Layouts (optimiert für den Querformatmodus) erläutert, in dem beide Fragmente auf dem Bildschirm angezeigt werden. Außerdem werden einige geringfügige Änderungen am Code vorgenommen, sodass die App ihr Verhalten an die Anzahl von Fragmenten anpasst, die gleichzeitig auf dem Bildschirm angezeigt werden.

## <a name="related-links"></a>Verwandte Links

- [FragmentsWalkthrough (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/fragmentswalkthrough)
- [Übersicht über Designer](~/android/user-interface/android-designer/index.md)
- [Implementieren von Fragmenten](https://developer.android.com/guide/topics/fundamentals/fragments.html)
- [Supportpaket](https://developer.android.com/sdk/compatibility-library.html)
