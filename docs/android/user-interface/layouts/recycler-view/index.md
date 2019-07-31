---
title: RecyclerView
description: "\"Recyclerview\" ist eine Ansichts Gruppe zum Anzeigen von Sammlungen. Es ist für eine flexiblere Ersetzung für ältere Ansichts Gruppen wie ListView und GridView konzipiert.  In diesem Handbuch wird erläutert, wie Sie in xamarin. Android-Anwendungen recyclerview verwenden und anpassen."
ms.prod: xamarin
ms.assetid: 91EF0BD2-3306-47E1-9B39-627A1787762F
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 01/03/2018
ms.openlocfilehash: 95d71beff2bd5219712494deb43f1f9fb4b082ec
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68646311"
---
# <a name="recyclerview"></a>RecyclerView

_"Recyclerview" ist eine Ansichts Gruppe zum Anzeigen von Sammlungen. Es ist für eine flexiblere Ersetzung für ältere Ansichts Gruppen wie ListView und GridView konzipiert.  In diesem Handbuch wird erläutert, wie Sie in xamarin. Android-Anwendungen recyclerview verwenden und anpassen._

## <a name="recyclerview"></a>RecyclerView

Viele apps müssen Auflistungen desselben Typs anzeigen (z. b. Nachrichten, Kontakte, Bilder oder Lieder); Diese Sammlung ist zu groß, um auf den Bildschirm zu passen, sodass die Auflistung in einem kleinen Fenster dargestellt wird, das reibungslos durch alle Elemente in der Auflistung Scrollen kann.
`RecyclerView`ist ein Android-Widget, das eine Auflistung von Elementen in einer Liste oder einem Raster anzeigt, sodass der Benutzer einen Bildlauf durch die Auflistung durchführen kann. Der folgende Screenshot zeigt eine Beispiel-APP, die verwendet `RecyclerView` , um den Inhalt von e-Mail-Posteingang in einer vertikalen scrollliste anzuzeigen:

[![Beispiel-App mit "recyclerview" zum Auflisten von Posteingangs Nachrichten](images/01-recyclerview-example-sml.png)](images/01-recyclerview-example.png#lightbox)

`RecyclerView`bietet zwei überzeugende Features:

-  Es verfügt über eine flexible Architektur, mit der Sie das Verhalten ändern können, indem Sie Ihre bevorzugten Komponenten anbinden.

-  Sie ist mit großen Auflistungen effizient, da Sie Element Sichten wieder verwendet und die Verwendung von *Ansichts Besitzern* zum Zwischenspeichern von Ansichts verweisen erfordert.

In diesem Handbuch wird erläutert, `RecyclerView` wie Sie in xamarin. Android-Anwendungen verwenden. es wird `RecyclerView` erläutert, wie Sie das Paket zu Ihrem xamarin. Android- `RecyclerView` Projekt hinzufügen, und es wird beschrieben, wie Funktionen in einer typischen Anwendung verwendet werden. Echte Codebeispiele werden bereitgestellt, um Ihnen zu zeigen `RecyclerView` , wie Sie in Ihre Anwendung integrieren, wie Sie die Element Ansicht implementieren und wie `RecyclerView` Sie aktualisiert werden, wenn sich die zugrunde liegenden Daten ändern. In dieser Anleitung wird davon ausgegangen, dass Sie mit der xamarin. Android-Entwicklung vertraut sind.


### <a name="requirements"></a>Anforderungen

Obwohl `RecyclerView` häufig mit Android 5,0 Lollipop verknüpft ist, wird es als Support Bibliothek &ndash; `RecyclerView` für apps angeboten, die auf API Level 7 (Android 2,1) und höher ausgerichtet sind. Folgendes ist erforderlich, um in `RecyclerView` xamarin-basierten Anwendungen zu verwenden:

-  **Xamarin.Android** &ndash; Xamarin.Android 4.20 or later must be installed and configured with either Visual Studio or Visual Studio for Mac.

-  Das App-Projekt muss das **xamarin. Android. Support. V7. recyclerview** -Paket enthalten. Weitere Informationen zum Installieren von nuget-Paketen finden [Sie unter Exemplarische Vorgehensweise: Einschließen eines nuget-Projekts in](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)Ihr Projekt.


### <a name="overview"></a>Übersicht

`RecyclerView`kann als Ersatz für die `ListView` Widgets und `GridView` in Android angesehen werden. Wie die Vorgänger `RecyclerView` ist so konzipiert, dass ein großes Dataset in einem kleinen Fenster angezeigt wird `RecyclerView` , aber es werden Weitere Layoutoptionen angeboten, die für die Anzeige von großen Auflistungen optimiert sind. Wenn Sie mit `ListView`vertraut sind, gibt es einige wichtige Unterschiede `ListView` zwischen `RecyclerView`und:

-   `RecyclerView`ist etwas komplexer zu verwenden: Sie müssen mehr Code schreiben, der im Vergleich `RecyclerView` zu `ListView`verwendet werden kann.

-   `RecyclerView`stellt keinen vordefinierten Adapter bereit. Sie müssen den Adapter Code implementieren, der auf Ihre Datenquelle zugreift. Android enthält jedoch mehrere vordefinierte Adapter, die mit `ListView` und `GridView`funktionieren.

-   `RecyclerView`bietet kein Element-Click-Ereignis, wenn ein Benutzer auf ein Element tippt. Stattdessen werden Element-Click-Ereignisse von Hilfsklassen behandelt. Im Gegensatz dazu `ListView` bietet ein Element-Click-Ereignis.

-   `RecyclerView`verbessert die Leistung durch die Wiederverwendung von Sichten und durch Erzwingen des Ansichts Inhaber-Musters, das unnötige layoutressourcenlookups eliminiert. Die Verwendung des Ansichts Inhaber Musters ist in `ListView`optional.

-   `RecyclerView`basiert auf einem modularen Entwurf, der die Anpassung erleichtert. Beispielsweise können Sie eine andere Layoutrichtlinie ohne wesentliche Codeänderungen an Ihrer APP einbinden.
    Im Gegensatz dazu `ListView` ist in der-Struktur relativ monolithischer.

-   `RecyclerView`enthält integrierte Animationen für das Hinzufügen und Entfernen von Elementen. `ListView`Animationen erfordern einen zusätzlichen Aufwand für den App-Entwickler.


### <a name="sections"></a>Abschnitte

#### <a name="recyclerview-parts-and-functionalityandroiduser-interfacelayoutsrecycler-viewparts-and-functionalitymd"></a>[Recyclerview-Teile und-Funktionalität](~/android/user-interface/layouts/recycler-view/parts-and-functionality.md)

In diesem Thema `Adapter`wird erläutert, wie, `ViewHolder` und zusammen als Hilfsklassen zur unter `RecyclerView`Stützung von verwendet werden `LayoutManager`.
Sie bietet eine allgemeine Übersicht über jede dieser Hilfsklassen und erläutert, wie Sie Sie in Ihrer APP verwenden.

#### <a name="a-basic-recyclerview-exampleandroiduser-interfacelayoutsrecycler-viewrecyclerview-examplemd"></a>[Ein grundlegendes recyclerview-Beispiel](~/android/user-interface/layouts/recycler-view/recyclerview-example.md)

Dieses Thema baut auf den Informationen in den [teilen und Funktionen von recyclerview auf und](~/android/user-interface/layouts/recycler-view/parts-and-functionality.md) bietet echte Codebeispiele für die `RecyclerView` Implementierung der verschiedenen Elemente zum Erstellen einer realen Foto-Browser-app.

#### <a name="extending-the-recyclerview-exampleandroiduser-interfacelayoutsrecycler-viewextending-the-examplemd"></a>[Erweitern des recyclerview-Beispiels](~/android/user-interface/layouts/recycler-view/extending-the-example.md)

In diesem Thema wird der Beispiel-APP, die in [einem grundlegenden recyclerview-Beispiel](~/android/user-interface/layouts/recycler-view/recyclerview-example.md) dargestellt wird, zusätzlicher Code hinzugefügt, um `RecyclerView` zu veranschaulichen, wie Elemente mit Klick Ereignissen behandelt und wann die zugrunde liegende Datenquelle geändert wird


### <a name="summary"></a>Zusammenfassung

In diesem Leitfaden wurde das `RecyclerView` Android-Widget vorgestellt. es wurde erläutert `RecyclerView` , wie die Unterstützungs Bibliothek zu xamarin. Android `RecyclerView` -Projekten hinzugefügt wird, wie Ansichten wieder verwendet werden, wie das Ansichts Inhaber-Muster für Effizienz und wie die verschiedenen Hilfsklassen, die zusammen `RecyclerView` arbeiten, um Auflistungen anzuzeigen. Es wurde ein Beispielcode bereitgestellt `RecyclerView` , um zu veranschaulichen, wie in eine Anwendung integriert ist `RecyclerView`. es wurde erläutert, wie Sie die Layoutrichtlinie anpassen können, indem Sie in verschiedene LayoutManager eingebunden werden, und es wurde beschrieben, wie Sie Click-Ereignisse `RecyclerView`von Datenquellen Änderungen.

Weitere Informationen zu `RecyclerView`finden Sie in der " [recyclerview"-Klassenreferenz](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html).


## <a name="related-links"></a>Verwandte Links

- [Recyclerviewer (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android50-recyclerviewer)
- [Einführung in Lollipop](~/android/platform/lollipop.md)
- [RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html)
