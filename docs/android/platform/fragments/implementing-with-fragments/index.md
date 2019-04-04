---
title: Implementieren von Fragmenten – Exemplarische Vorgehensweise
description: In diesem Artikel erläutert, wie Fragmente zum Entwickeln von Xamarin.Android-Anwendungen verwenden.
ms.topic: tutorial
ms.prod: xamarin
ms.assetid: A71E9D87-CB69-10AB-CE51-357A05C76BCD
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/26/2018
ms.openlocfilehash: 2ff4729e68497391d41521da26917571c146b541
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2019
ms.locfileid: "57667264"
---
# <a name="implementing-fragments---walkthrough"></a>Implementieren von Fragmenten – Exemplarische Vorgehensweise

_Fragmente sind eigenständige, modulare Komponenten, mit deren Hilfe können die Komplexität der Android-apps zu beheben, die Geräte mit einer Vielzahl von Bildschirmgrößen abzielen. Dieser Artikel führt durch das Erstellen und verwenden Sie Fragmente, bei der Entwicklung von Xamarin.Android-Anwendungen._

## <a name="overview"></a>Übersicht

In diesem Abschnitt begleiten Sie durch die Schritte zum Erstellen und verwenden Fragmente in einer Xamarin.Android-Anwendung. Diese Anwendung zeigt mehrere spielt die Titel von William Shakespeare in einer Liste. Wenn der Benutzer auf den Titel der eine Play tippt, wird die app in einer separaten Aktivität ein Zitat aus dieser Play angezeigt:

[![App auf einem Android-Telefon im Hochformat](./images/intro-screenshot-phone-sml.png)](./images/intro-screenshot-phone.png#lightbox)

Wenn das Telefon in das Querformat gedreht wird, ändert das Erscheinungsbild der app: sowohl die Liste der spielt und Anführungszeichen wird in der gleichen Aktivität angezeigt. Wenn ein Spiel ausgewählt ist, werden die anführung wie in der gleichen Aktivität angezeigt:

[![App auf einem Android-Telefon im Querformat](./images/intro-screenshot-phone-land-sml.png)](./images/intro-screenshot-phone-land.png#lightbox)

Schließlich, wenn die app auf einem Tablet PC ausgeführt wird:

[![App auf einem Android-tablet](./images/intro-screenshot-tablet-sml.png)](./images/intro-screenshot-tablet.png#lightbox)

Diese beispielanwendung kann problemlos auf die verschiedenen Formfaktoren und Ausrichtungen mit minimalen codeänderungen mit Fragmenten anpassen und [alternative Layouts](/xamarin/android/app-fundamentals/resources-in-android/alternate-resources).

Die Daten für die Anwendung befindet sich in zwei Zeichenfolgenarrays, die in der app als hartcodiert sind C# Zeichenfolge des Arrays. Jede der Arrays wird als Datenquelle für ein Fragment dienen.  Ein Array enthält den Namen des einige spielt von Shakespeare und anderen Array enthält ein Angebot aus, Play. Wenn die app wird gestartet, zeigt es die Play-Namen in einem `ListFragment`. Wenn der Benutzer klickt auf eine Play in die `ListFragment`, die app wird gestartet, um eine andere Aktivität der das Angebot angezeigt wird.

Die Benutzeroberfläche für die app besteht aus zwei Layouts, für das Hochformat und eine für im Querformat. Zur Laufzeit Android wird bestimmt, welches Layout beim Laden auf die Ausrichtung des Geräts basieren und stellt das Layout der Aktivität zu rendern. Die gesamte Logik zum Reagieren auf Benutzerklicks und zum Anzeigen der Daten ist in Fragmenten enthalten. Die Aktivitäten in der app, die nur als Container, die als der Fragmente Host vorhanden sein.

In dieser exemplarischen Vorgehensweise wird in zwei Handbücher unterteilt werden. Die [erster Teil](./walkthrough.md) konzentriert sich auf die wichtigsten Bestandteile der Anwendung. Ein einzelner Satz von Layouts (optimiert für Hochformat) wird zusammen mit zwei Fragmente und zwei Aktivitäten erstellt werden:

1. `MainActivity` &nbsp; Dies ist der Start-Aktivität für die app.
1. `TitlesFragment` &nbsp; Dieses Fragment zeigt eine Liste der Titel der wiedergegeben wird, die von William Shakespeare geschrieben wurden. Es von gehosteten `MainActivity`.
1. `PlayQuoteActivity` &nbsp; `TitlesFragment` Startet die `PlayQuoteActivity` als Reaktion auf die Auswahl einer Play in `TitlesFragment`.
1. `PlayQuoteFragment` &nbsp; Dieses Fragment wird ein Zitat aus eine Play von William Shakespeare angezeigt. Es von gehosteten `PlayQuoteActivity`.

Die [zweiter Teil dieser exemplarischen Vorgehensweise](./walkthrough-landscape.md) besprechen hinzufügen ein alternatives Layout (optimiert für im Querformat) der beiden Fragmenten auf dem Bildschirm angezeigt wird. Einige kleinere Änderungen am Code werden außerdem am Code vorgenommen werden, damit die app das Verhalten, um die Anzahl der Fragmente angepasst wird, die gleichzeitig auf dem Bildschirm angezeigt werden.

## <a name="related-links"></a>Verwandte Links

- [FragmentsWalkthrough (Beispiel)](https://developer.xamarin.com/samples/monodroid/FragmentsWalkthrough/)
- [Übersicht über Designer](~/android/user-interface/android-designer/index.md)
- [Implementieren von Fragmenten](https://developer.android.com/guide/topics/fundamentals/fragments.html)
- [Supportpaket](https://developer.android.com/sdk/compatibility-library.html)
