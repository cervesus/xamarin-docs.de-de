---
title: Implementieren von Fragmenten - Exemplarische Vorgehensweise
description: In diesem Artikel erläutert Fragmente verwenden, um Xamarin.Android Anwendungen zu entwickeln.
ms.topic: tutorial
ms.prod: xamarin
ms.assetid: A71E9D87-CB69-10AB-CE51-357A05C76BCD
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/26/2018
ms.openlocfilehash: 92c68298d7abd2570efd89e12d7cfb6364e90972
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
ms.locfileid: "33798355"
---
# <a name="implementing-fragments---walkthrough"></a>Implementieren von Fragmenten - Exemplarische Vorgehensweise

_Fragmente handelt es sich um eigenständige, modularen Komponenten, mit deren Hilfe können die Komplexität der Android-apps zu befassen, die mit unterschiedlichen Bildschirmgrößen-Geräte abzielen. Dieser Artikel führt durch die Schritte zum Erstellen und verwenden Sie beim Entwickeln von Anwendungen Xamarin.Android Fragmente._

## <a name="overview"></a>Übersicht

In diesem Abschnitt fügen Sie Exemplarische Vorgehensweise zum Erstellen und Verwenden von Fragmenten in einer Anwendung Xamarin.Android. Diese Anwendung zeigt mehrere spielt die Titel von William Shakespeare in einer Liste. Beim Tippen auf den Titel des wiedergeben wird die app ein Angebot aus dieser Play in einer separaten Aktivität angezeigt:

[![App auf einem Android-Telefon im Hochformat](./images/intro-screenshot-phone-sml.png)](./images/intro-screenshot-phone.png#lightbox)

Wenn das Telefon Modus Querformat gedreht wird, wird das Erscheinungsbild der app zu ändern: sowohl die Liste der spielt und Anführungszeichen in der gleichen Aktivität angezeigt. Wenn eine Wiedergabe aktiviert ist, wird das Angebot Anzeige in der gleichen Aktivität sein:

[![App auf einem Android-Telefon im Querformat](./images/intro-screenshot-phone-land-sml.png)](./images/intro-screenshot-phone-land.png#lightbox)

Abschließend, wenn die app auf einem Tablet-PC ausgeführt wird:

[![Auf einem Android-Tablet Ausführen einer App](./images/intro-screenshot-tablet-sml.png)](./images/intro-screenshot-tablet.png#lightbox)

Diese beispielanwendung kann problemlos auf die verschiedenen Formfaktoren arbeiten und Ausrichtungen mit minimalen codeänderungen mit Fragmenten anpassen und [alternative Layouts](/xamarin/android/app-fundamentals/resources-in-android/alternate-resources).

Die Daten für die Anwendung werden in zwei Zeichenfolgenarrays vorhanden sein, die in der Anwendung hartcodiert als C#-Zeichenfolgenarrays sind. Jede der Arrays wird als Datenquelle für ein Fragment dienen.  Ein Array, in den Namen des einige spielt von Shakespeare gespeichert, und anderen Arrays wird ein Angebot aus dieser Play halten. Nach dem Start der app zeigt es die wiedergeben-Namen in einem `ListFragment`. Wenn der Benutzer klickt auf eine Wiedergabe in die `ListFragment`, startet die app von einer anderen Aktivität der das Angebot angezeigt wird.

Die Benutzeroberfläche für die app besteht aus zwei Layouts, eine für Hochformat und eine für im Querformat. Zur Laufzeit Android wird bestimmt, welches Layout beim Laden der Ausrichtung des Geräts anhand und stellt das Layout der Aktivität zum Rendern bereit. Alle von der Logik zum Reagieren auf Benutzer und zum Anzeigen der Daten wird in Fragmente enthalten sein. Die Aktivitäten in der app, die nur als Container, die als der Fragmente Host vorhanden sein.

In dieser exemplarischen Vorgehensweise wird in zwei Führungslinien unterteilt werden. Die [erster Teil](./walkthrough.md) konzentriert sich auf die wichtigsten Bestandteile der Anwendung. Ein einzelner Satz von Layouts (optimiert für Hochformat) wird zusammen mit zwei Fragmente und zwei Aktivitäten erstellt werden:

1. `MainActivity` &nbsp; Hierbei handelt es sich um den Start-Aktivität für die app.
1. `TitlesFragment` &nbsp; Dieses Fragment zeigt eine Liste der Titel wiedergegeben wird, die von William Shakespeare geschrieben wurden. Es wird von gehostet werden `MainActivity`.
1. `PlayQuoteActivity` &nbsp; `TitlesFragment` Startet die `PlayQuoteActivity` als Antwort auf die Benutzer wählen ein Spiel in `TitlesFragment`.
1. `PlayQuoteFragment` &nbsp; Dieses Fragment wird ein Angebot aus einer Prüfung von William Shakespeare angezeigt. Es wird von gehostet werden `PlayQuoteActivity`.

Die [zweiter Teil dieser exemplarischen Vorgehensweise](./walkthrough-landscape.md) besprechen Hinzufügen einer alternativen Layout (optimiert für im Querformat) der beiden Fragmenten auf dem Bildschirm angezeigt wird. Darüber hinaus werden einige kleinere Änderungen am Code an den Code erfolgen, damit die app sein Verhalten an die Anzahl der Fragmente angepasst wird, die gleichzeitig auf dem Bildschirm angezeigt werden.

## <a name="related-links"></a>Verwandte Links

- [FragmentsWalkthrough (Beispiel)](https://developer.xamarin.com/samples/monodroid/FragmentsWalkthrough/)
- [Übersicht über-Designer](~/android/user-interface/android-designer/index.md)
- [Implementieren von Fragmenten](http://developer.android.com/guide/topics/fundamentals/fragments.html)
- [Supportpaket](http://developer.android.com/sdk/compatibility-library.html)
