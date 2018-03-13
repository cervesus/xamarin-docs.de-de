---
title: RecyclerView
description: "RecyclerView ist eine Gruppe \"Ansicht\" für die Anzeige von Sammlungen. Es soll eine flexiblere Ersatz für ältere Ansicht-Gruppen wie z. B. ListView-Steuerelement und GridView.  Dieses Handbuch erläutert das verwenden und Anpassen von RecyclerView in Xamarin.Android Anwendungen."
ms.topic: article
ms.prod: xamarin
ms.assetid: 91EF0BD2-3306-47E1-9B39-627A1787762F
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 01/03/2018
ms.openlocfilehash: 028520742a84e717e28147f2fa1fafacfef34028
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/12/2018
---
# <a name="recyclerview"></a>RecyclerView

_RecyclerView ist eine Gruppe "Ansicht" für die Anzeige von Sammlungen. Es soll eine flexiblere Ersatz für ältere Ansicht-Gruppen wie z. B. ListView-Steuerelement und GridView.  Dieses Handbuch erläutert das verwenden und Anpassen von RecyclerView in Xamarin.Android Anwendungen._

## <a name="recyclerview"></a>RecyclerView

Viele apps müssen Sammlungen des gleichen Typs (z. B. Nachrichten, Kontakte, Bilder oder Titel); anzeigen Diese Auflistung wird häufig zu groß für auf dem Bildschirm, damit die Auflistung in einem kleinen Fenster angezeigt wird, das über alle Elemente in der Auflistung reibungslos gescrollt werden kann.
`RecyclerView` ist ein Android-Widget, die eine Auflistung von Elementen in einer Liste oder einem Raster, damit der Benutzer einen Bildlauf durch die Auflistung zeigt. Im folgenden finden Sie einen Screenshot der Beispiel-app, verwendet `RecyclerView` e-Mail-Posteingang Inhalt in einer Liste der vertikalen Bildlaufleiste angezeigt:

[![Beispiel-app mithilfe von RecyclerView in Liste Posteingangsnachrichten](images/01-recyclerview-example-sml.png)](images/01-recyclerview-example.png#lightbox)

`RecyclerView` bietet zwei interessante Funktionen:

-  Er verfügt über eine flexible Architektur, in dem Sie dessen Verhalten zu ändern, indem Sie unter Verwendung der in Ihren bevorzugten Komponenten kann.

-  Es ist effizienter mit großen Auflistungen, da Item Sichten wiederverwendet und die Verwendung eines erfordert *anzeigen Inhaber* Cache Sicht verweisen.

Diese Anleitung erläutert die Verwendung `RecyclerView` in Xamarin.Android Anwendungen; es erläutert das Hinzufügen der `RecyclerView` Paket Xamarin.Android Projekt, und es wird beschrieben, wie `RecyclerView` Funktionen in einer typischen Anwendung. Echte Codebeispiele sind dient zur Veranschaulichung der Vorgehensweise beim Integrieren von `RecyclerView` in Ihrer Anwendung, zum Implementieren von Elementansicht auf und Aktualisieren von `RecyclerView` Wenn sich die zugrunde liegenden Daten ändert. Dieses Handbuch setzt voraus, dass Sie bei der Entwicklung Xamarin.Android vertraut sind.


### <a name="requirements"></a>Anforderungen

Obwohl `RecyclerView` wird häufig mit Android 5.0 Lollipop verknüpft ist, wird er als Unterstützungsbibliothek angeboten &ndash; `RecyclerView` arbeitet mit apps, Ziel-API-Ebene 7 (Android 2.1) und höher. Folgendes ist erforderlich, um verwenden `RecyclerView` in Xamarin-basierten Anwendungen:

-  **Xamarin.Android** &ndash; Xamarin.Android 4.20 or later must be installed and configured with either Visual Studio or Visual Studio for Mac.

-  Ihr app-Projekt umfasst die **Xamarin.Android.Support.v7.RecyclerView** Paket. Weitere Informationen zum Installieren von NuGet-Pakete finden Sie unter [Exemplarische Vorgehensweise: einschließlich eines NuGet in Ihrem Projekt](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough).


### <a name="overview"></a>Übersicht

`RecyclerView` kann als Ersatz für betrachtet werden die `ListView` und `GridView` Widgets in Android. Wie die Vorgängerversionen `RecyclerView` dient zum Anzeigen eines großen Datasets in einem kleinen Fenster aber `RecyclerView` bieten weitere Layoutoptionen und eine bessere Leistung für die Anzeige von umfangreichen Sammlungen optimiert ist. Wenn Sie kennen `ListView`, es gibt mehrere wichtige Unterschiede zwischen `ListView` und `RecyclerView`:

-   `RecyclerView` ist etwas komplexer verwenden: Sie verwenden mehr Code schreiben müssen, `RecyclerView` im Vergleich zu `ListView`.

-   `RecyclerView` bietet einen vordefinierten Adapter keine; Sie müssen den Adaptercode implementieren, der Ihre Datenquelle zugreift. Android bietet jedoch mehrere vordefinierte Adapter, die mit arbeiten `ListView` und `GridView`.

-   `RecyclerView` bietet keine Element-Click-Ereignis, wenn ein Benutzer ein Element tippt; Stattdessen werden die Element-Click-Ereignisse vom Hilfsklassen behandelt. Im Gegensatz dazu `ListView` bietet eine Element-Click-Ereignis.

-   `RecyclerView` verbessert die Leistung durch die Wiederverwendung von Ansichten und erzwingen das Muster Ansicht Inhaber, das wodurch unnötige Layout Ressourcensuchen entfällt. Die Verwendung des Musters für den Inhaber der Sicht ist optional in `ListView`.

-   `RecyclerView` einen modularen Aufbau basiert auf, die leichter angepasst werden kann. Beispielsweise können Sie ein anderes Layoutrichtlinie ohne bedeutende codeänderungen an Ihre app anschließen.
    Im Gegensatz dazu `ListView` ist relativ monolithischen in Struktur.

-   `RecyclerView` umfasst integrierte Animationen an, für Artikel hinzufügen und entfernen. `ListView` Animationen erfordern einige zusätzlichen Aufwand seitens der app-Entwickler.


### <a name="sections"></a>Abschnitte

#### <a name="recyclerview-parts-and-functionalityandroiduser-interfacelayoutsrecycler-viewparts-and-functionalitymd"></a>[RecyclerView Teile und Funktionalität](~/android/user-interface/layouts/recycler-view/parts-and-functionality.md)

In diesem Thema wird erläutert, wie die `Adapter`, `LayoutManager`, und `ViewHolder` zusammenarbeiten als Hilfsklassen zur Unterstützung `RecyclerView`.
Es gibt einen Überblick über jedes dieser Hilfsklassen und erläutert, wie Sie diese in Ihrer app verwenden.

#### <a name="a-basic-recyclerview-exampleandroiduser-interfacelayoutsrecycler-viewrecyclerview-examplemd"></a>[Ein einfaches RecyclerView-Beispiel](~/android/user-interface/layouts/recycler-view/recyclerview-example.md)

Dieses Thema baut auf den Angaben in [RecyclerView Teile und Funktionalität](~/android/user-interface/layouts/recycler-view/parts-and-functionality.md) durch die Bereitstellung von realen Codebeispielen, wie die verschiedenen `RecyclerView` Elemente werden implementiert, um einen realen Durchsuchen von Foto-app zu erstellen.

#### <a name="extending-the-recyclerview-exampleandroiduser-interfacelayoutsrecycler-viewextending-the-examplemd"></a>[Erweitern Sie im RecyclerView-Beispiel](~/android/user-interface/layouts/recycler-view/extending-the-example.md)

In diesem Thema wird dargestellt, der Beispiel-app zusätzlichen Code hinzugefügt [ein einfaches RecyclerView Beispiel](~/android/user-interface/layouts/recycler-view/recyclerview-example.md) zu veranschaulichen, wie Sie die Element-Click-Ereignisse behandeln, und aktualisieren Sie `RecyclerView` Wenn die Änderungen die zugrunde liegenden Datenquelle.


### <a name="summary"></a>Zusammenfassung

Dieses Handbuch eingeführt, das Android `RecyclerView` Widget; es wird erläutert, wie zum Hinzufügen der `RecyclerView` Unterstützungsbibliothek Xamarin.Android-Projekte wie `RecyclerView` Ansichten wiederverwendet wird, wie das Muster Ansicht Inhaber Effizienzgründen Datenbankkonsistenz erzwungen werden und wie die verschiedenen Hilfsklassen, mit deren Hilfe `RecyclerView` zusammenarbeiten, um die Sammlungen angezeigt. Sie Beispielcode zur Veranschaulichung der Funktion bereitgestellt wie `RecyclerView` ist integriert in eine Anwendung, es wird erläutert, wie anpassen `RecyclerView`des Layoutrichtlinie von Plug-in-anderes Layout-Manager, und es beschrieben Element behandelt click-Ereignisse, und benachrichtigen Sie `RecyclerView`der Datenquelle geändert.

Weitere Informationen zu `RecyclerView`, finden Sie unter der [RecyclerView-Klassenreferenz](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html).


## <a name="related-links"></a>Verwandte Links

- [RecyclerViewer (Beispiel)](https://developer.xamarin.com/samples/monodroid/android5.0/RecyclerViewer)
- [Einführung in die Lollipop](~/android/platform/lollipop.md)
- [RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html)
