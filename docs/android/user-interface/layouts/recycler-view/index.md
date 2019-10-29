---
title: RecyclerView
description: "\"Recyclerview\" ist eine Ansichts Gruppe zum Anzeigen von Sammlungen. Es ist für eine flexiblere Ersetzung für ältere Ansichts Gruppen wie ListView und GridView konzipiert.  In diesem Handbuch wird erläutert, wie Sie in xamarin. Android-Anwendungen recyclerview verwenden und anpassen."
ms.prod: xamarin
ms.assetid: 91EF0BD2-3306-47E1-9B39-627A1787762F
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 01/03/2018
ms.openlocfilehash: bce89be8bec512ac70ca40015521c7d56f3460d3
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73028810"
---
# <a name="recyclerview"></a>RecyclerView

_recyclerview eine Ansichts Gruppe zum Anzeigen von Sammlungen ist. Es ist für eine flexiblere Ersetzung für ältere Ansichts Gruppen wie ListView und GridView konzipiert.  In diesem Handbuch wird erläutert, wie Sie in xamarin. Android-Anwendungen recyclerview verwenden und anpassen._

## <a name="recyclerview"></a>RecyclerView

Viele apps müssen Auflistungen desselben Typs anzeigen (z. b. Nachrichten, Kontakte, Bilder oder Lieder); Diese Sammlung ist zu groß, um auf den Bildschirm zu passen, sodass die Auflistung in einem kleinen Fenster dargestellt wird, das reibungslos durch alle Elemente in der Auflistung Scrollen kann.
`RecyclerView` ist ein Android-Widget, das eine Auflistung von Elementen in einer Liste oder einem Raster anzeigt, sodass der Benutzer einen Bildlauf durch die Sammlung durchführen kann. Der folgende Screenshot zeigt eine Beispiel-APP, die `RecyclerView` verwendet, um den Inhalt von e-Mail-Posteingang in einer vertikalen scrollliste anzuzeigen:

[![Beispiel-App mit "recyclerview" zum Auflisten von Posteingangs Nachrichten](images/01-recyclerview-example-sml.png)](images/01-recyclerview-example.png#lightbox)

`RecyclerView` bietet zwei überzeugende Features:

- Es verfügt über eine flexible Architektur, mit der Sie das Verhalten ändern können, indem Sie Ihre bevorzugten Komponenten anbinden.

- Sie ist mit großen Auflistungen effizient, da Sie Element Sichten wieder verwendet und die Verwendung von *Ansichts Besitzern* zum Zwischenspeichern von Ansichts verweisen erfordert.

In diesem Handbuch wird erläutert, wie `RecyclerView` in xamarin. Android-Anwendungen verwendet wird. Es wird erläutert, wie das `RecyclerView`-Paket Ihrem xamarin. Android-Projekt hinzugefügt wird, und es wird beschrieben, wie `RecyclerView` in einer typischen Anwendung funktioniert. Echte Codebeispiele werden bereitgestellt, um Ihnen zu zeigen, wie Sie `RecyclerView` in Ihre Anwendung integrieren, die Element Ansicht implementieren und `RecyclerView` aktualisieren, wenn sich die zugrunde liegenden Daten ändern. In dieser Anleitung wird davon ausgegangen, dass Sie mit der xamarin. Android-Entwicklung vertraut sind.

### <a name="requirements"></a>Voraussetzungen

Obwohl `RecyclerView` häufig mit Android 5,0 Lollipop verknüpft ist, wird es als Support Bibliothek angeboten &ndash; `RecyclerView` die mit apps arbeiten, die auf API Level 7 (Android 2,1) und höher ausgerichtet sind. Folgendes ist erforderlich, um `RecyclerView` in xamarin-basierten Anwendungen zu verwenden:

- **Xamarin. Android** &ndash; xamarin. Android 4,20 oder höher muss entweder mit Visual Studio oder mit Visual Studio für Mac installiert und konfiguriert werden.

- Das App-Projekt muss das **xamarin. Android. Support. V7. recyclerview** -Paket enthalten. Weitere Informationen zum Installieren von nuget-Paketen finden Sie unter [Exemplarische Vorgehensweise: Das Einschließen eines nuget-](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)in Ihr Projekt.

### <a name="overview"></a>Übersicht

`RecyclerView` können als Ersatz für die `ListView`-und `GridView` Widgets in Android angesehen werden. Wie seine Vorgänger ist `RecyclerView` so konzipiert, dass ein großes Dataset in einem kleinen Fenster angezeigt wird. `RecyclerView` bietet jedoch Weitere Layoutoptionen und ist für die Anzeige großer Sammlungen besser optimiert. Wenn Sie mit `ListView`vertraut sind, gibt es einige wichtige Unterschiede zwischen `ListView` und `RecyclerView`:

- `RecyclerView` ist etwas komplexer zu verwenden: Sie müssen mehr Code schreiben, um `RecyclerView` im Vergleich zu `ListView`zu verwenden.

- `RecyclerView` stellt keinen vordefinierten Adapter bereit. Sie müssen den Adapter Code implementieren, der auf Ihre Datenquelle zugreift. Android enthält jedoch mehrere vordefinierte Adapter, die mit `ListView` und `GridView`funktionieren.

- `RecyclerView` bietet kein Element Klick Ereignis, wenn ein Benutzer auf ein Element tippt. Stattdessen werden Element-Click-Ereignisse von Hilfsklassen behandelt. Im Gegensatz dazu bietet `ListView` ein Element Klick-Ereignis.

- `RecyclerView` verbessern die Leistung durch die Wiederverwendung von Sichten und durch Erzwingen des Ansichts Inhaber Musters, wodurch unnötige layoutressourcenlookups vermieden werden. Die Verwendung des Ansichts Inhaber Musters ist in `ListView`optional.

- `RecyclerView` basiert auf einem modularen Design, das die Anpassung erleichtert. Beispielsweise können Sie eine andere Layoutrichtlinie ohne wesentliche Codeänderungen an Ihrer APP einbinden.
    Im Gegensatz dazu ist `ListView` relativ monolithischer in der Struktur.

- `RecyclerView` enthält integrierte Animationen für das Hinzufügen und Entfernen von Elementen. `ListView` Animationen erfordern einen zusätzlichen Aufwand für den App-Entwickler.

### <a name="sections"></a>Abschnitte

#### <a name="recyclerview-parts-and-functionalityandroiduser-interfacelayoutsrecycler-viewparts-and-functionalitymd"></a>[Recyclerview-Teile und-Funktionalität](~/android/user-interface/layouts/recycler-view/parts-and-functionality.md)

In diesem Thema wird erläutert, wie die `Adapter`, `LayoutManager`und `ViewHolder` als Hilfsklassen zur Unterstützung von `RecyclerView`zusammenarbeiten.
Sie bietet eine allgemeine Übersicht über jede dieser Hilfsklassen und erläutert, wie Sie Sie in Ihrer APP verwenden.

#### <a name="a-basic-recyclerview-exampleandroiduser-interfacelayoutsrecycler-viewrecyclerview-examplemd"></a>[Ein grundlegendes recyclerview-Beispiel](~/android/user-interface/layouts/recycler-view/recyclerview-example.md)

Dieses Thema baut auf den Informationen in den [teilen und Funktionen von recyclerview auf und](~/android/user-interface/layouts/recycler-view/parts-and-functionality.md) bietet echte Codebeispiele für die Implementierung der verschiedenen `RecyclerView` Elemente zum Erstellen einer realen Foto-Browser-app.

#### <a name="extending-the-recyclerview-exampleandroiduser-interfacelayoutsrecycler-viewextending-the-examplemd"></a>[Erweitern des recyclerview-Beispiels](~/android/user-interface/layouts/recycler-view/extending-the-example.md)

In diesem Thema wird der Beispiel-APP, die in [einem grundlegenden recyclerview-Beispiel](~/android/user-interface/layouts/recycler-view/recyclerview-example.md) dargestellt wird, zusätzlicher Code hinzugefügt, um zu veranschaulichen, wie Elemente mit Element-und Update `RecyclerView` behandelt werden, wenn sich die zugrunde liegende

### <a name="summary"></a>Zusammenfassung

In diesem Leitfaden wurde das Android-`RecyclerView` Widget eingeführt. Es wurde erläutert, wie Sie die `RecyclerView` Unterstützungs Bibliothek zu xamarin. Android-Projekten hinzufügen, wie `RecyclerView` Sichten wieder verwendet werden, wie Sie das Ansichts Inhaber-Muster für Effizienz erzwingt und wie die verschiedenen Hilfsklassen, die `RecyclerView` zusammenarbeiten, zum Anzeigen von Sammlungen zusammenarbeiten. Es wurde ein Beispielcode bereitgestellt, um zu veranschaulichen, wie `RecyclerView` in eine Anwendung integriert ist. es wurde erläutert, wie Sie die Layoutrichtlinie `RecyclerView`anpassen können, indem Sie unterschiedliche LayoutManager einrichtet. Außerdem wurde beschrieben, wie Sie Klick Ereignisse für Elemente und `RecyclerView` von Daten Quell Änderungen.

Weitere Informationen zu `RecyclerView`finden Sie in der " [recyclerview"-Klassenreferenz](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html).

## <a name="related-links"></a>Verwandte Links

- [Recyclerviewer (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android50-recyclerviewer)
- [Einführung in Lollipop](~/android/platform/lollipop.md)
- [RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html)
