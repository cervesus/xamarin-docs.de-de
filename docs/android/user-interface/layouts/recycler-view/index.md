---
title: RecyclerView
description: RecyclerView ist eine Gruppe anzeigen für die Anzeige von Sammlungen. Es soll eine flexiblere Ersatz für ältere anzeigen Gruppen wie z. B. ListView und GridView zu werden.  Dieses Handbuch wird erläutert, wie verwenden und RecyclerView in Xamarin.Android-Anwendungen anpassen können.
ms.prod: xamarin
ms.assetid: 91EF0BD2-3306-47E1-9B39-627A1787762F
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 01/03/2018
ms.openlocfilehash: 1332a7ef5b8e5bb2f1178582bcf058123f1e177c
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50110146"
---
# <a name="recyclerview"></a>RecyclerView

_RecyclerView ist eine Gruppe anzeigen für die Anzeige von Sammlungen. Es soll eine flexiblere Ersatz für ältere anzeigen Gruppen wie z. B. ListView und GridView zu werden.  Dieses Handbuch wird erläutert, wie verwenden und RecyclerView in Xamarin.Android-Anwendungen anpassen können._

## <a name="recyclerview"></a>RecyclerView

Viele apps müssen zum Anzeigen von Sammlungen des gleichen Typs (z. B. Nachrichten, Kontakte, Bilder oder Titel); Häufig ist dieser Auflistung zu groß für auf dem Bildschirm, damit die Auflistung in einem kleinen Fenster angezeigt wird, die reibungslos über alle Elemente in der Auflistung einen Bildlauf durchführen kann.
`RecyclerView` ist ein Android-Widget, das eine Auflistung von Elementen in einer Liste oder einem Raster, sodass der Benutzer einen Bildlauf durch die Auflistung anzeigt. Im folgenden finden Sie einen Screenshot einer Beispiel-App, die verwendet `RecyclerView` e-Mail-Posteingang-Inhalt in einer Liste mit vertikalem Bildlauf angezeigt:

[![Beispiel-app, die Nachrichten im Posteingang Liste RecyclerView mit](images/01-recyclerview-example-sml.png)](images/01-recyclerview-example.png#lightbox)

`RecyclerView` bietet zwei interessante Features:

-  Es verfügt über eine flexible Architektur, in dem Sie ihr Verhalten zu ändern, indem Sie Ihre bevorzugte Komponenten anschließen kann.

-  Es ist mit großen Sammlungen effizient, da Item Sichten wiederverwendet und die Verwendung von erfordert *anzeigen Platzhalter* zu Cache Ansicht verweisen.

Dieses Handbuch erläutert, wie `RecyclerView` in Xamarin.Android-Anwendungen; es erläutert das Hinzufügen der `RecyclerView` Paket in Ihr Xamarin.Android-Projekt, und es wird beschrieben, wie `RecyclerView` Funktionen in einer typischen Anwendung. Echte Codebeispiele werden bereitgestellt, um eine Anleitung zum Integrieren von `RecyclerView` in Ihrer Anwendung, zum Implementieren der Elements-Ansicht klicken Sie auf, und Aktualisieren von `RecyclerView` Wenn die zugrunde liegenden Daten ändern. Dieses Handbuch wird davon ausgegangen, dass Sie mit der Xamarin.Android-Entwicklung vertraut sind.


### <a name="requirements"></a>Anforderungen

Obwohl `RecyclerView` wird häufig in Verbindung mit Android 5.0 Lollipop, wird es als ein Support-Bibliothek angeboten &ndash; `RecyclerView` funktioniert mit apps, Ziel-API-Ebene 7 (Android 2.1) und höher. Folgendes ist erforderlich, um verwenden `RecyclerView` in Xamarin-basierten Anwendungen:

-  **Xamarin.Android** &ndash; Xamarin.Android 4.20 or later must be installed and configured with either Visual Studio or Visual Studio for Mac.

-  Ihr app-Projekt muss enthalten der **Xamarin.Android.Support.v7.RecyclerView** Paket. Weitere Informationen zum Installieren von NuGet-Pakete finden Sie unter [Exemplarische Vorgehensweise: Einschließen eines NuGet in Ihrem Projekt](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough).


### <a name="overview"></a>Übersicht

`RecyclerView` kann als Ersatz für betrachtet werden die `ListView` und `GridView` Widgets in Android. Wie die Vorgängerversionen `RecyclerView` dient zur Anzeige eines großen Datasets in einem kleinen Fenster, jedoch `RecyclerView` bieten weitere Layoutoptionen und besser für die Anzeige von großen Auflistungen optimiert ist. Wenn Sie mit auskennen `ListView`, es gibt mehrere wichtige Unterschiede zwischen `ListView` und `RecyclerView`:

-   `RecyclerView` ist etwas komplexer verwenden: Sie verwenden mehr Code schreiben müssen, `RecyclerView` im Vergleich zu `ListView`.

-   `RecyclerView` bietet einen vordefinierten Adapter keine. Sie müssen den Adaptercode implementieren, der auf Ihre Datenquelle zugreift. Android bietet jedoch mehrere vordefinierte Adapter, die Arbeit mit `ListView` und `GridView`.

-   `RecyclerView` ein Element-Click-Ereignis, wenn ein Benutzer ein Element tippt werden nicht bereitgestellt werden; Stattdessen werden die Element-Click-Ereignisse von Hilfsklassen behandelt. Im Gegensatz dazu `ListView` bietet eine Element-Click-Ereignis.

-   `RecyclerView` verbessert die Leistung durch Wiederverwenden von Ansichten und erzwingen das View-Inhaber-Muster, das wodurch unnötige Layout Ressourcensuchen entfällt. Mit dem Ansicht-Inhaber der Muster ist in optional `ListView`.

-   `RecyclerView` auf einem modularen Design basiert, die leicht anpassen können. Beispielsweise können Sie zu Ihrer app in ein anderes Layoutrichtlinie ohne erhebliche codeänderungen anschließen.
    Im Gegensatz dazu `ListView` relativ monolithische Struktur ist.

-   `RecyclerView` enthält integrierte Animationen, Element hinzufügen und entfernen. `ListView` Animationen müssen einige zusätzlichen Aufwand seitens der app-Entwickler.


### <a name="sections"></a>Abschnitte

#### <a name="recyclerview-parts-and-functionalityandroiduser-interfacelayoutsrecycler-viewparts-and-functionalitymd"></a>[RecyclerView-Komponenten und Funktionen](~/android/user-interface/layouts/recycler-view/parts-and-functionality.md)

In diesem Thema wird erläutert, wie die `Adapter`, `LayoutManager`, und `ViewHolder` arbeiten zusammen als Hilfsklassen zur Unterstützung `RecyclerView`.
Es enthält eine allgemeine Übersicht über jede dieser Hilfsklassen und erläutert, wie Sie sie in Ihrer app verwenden.

#### <a name="a-basic-recyclerview-exampleandroiduser-interfacelayoutsrecycler-viewrecyclerview-examplemd"></a>[Ein einfaches RecyclerView-Beispiel](~/android/user-interface/layouts/recycler-view/recyclerview-example.md)

Dieses Thema basiert auf den Angaben in [RecyclerView-Komponenten und Funktionen](~/android/user-interface/layouts/recycler-view/parts-and-functionality.md) durch die Bereitstellung von echten Codebeispiele, wie die verschiedenen `RecyclerView` Elemente werden implementiert, um eine reale zum Durchsuchen von Fotos-app erstellen.

#### <a name="extending-the-recyclerview-exampleandroiduser-interfacelayoutsrecycler-viewextending-the-examplemd"></a>[Erweitern Sie im RecyclerView-Beispiel](~/android/user-interface/layouts/recycler-view/extending-the-example.md)

In diesem Thema werden die Beispiel-app angezeigt, zusätzlicher Code hinzugefügt [ein einfaches RecyclerView-Beispiel](~/android/user-interface/layouts/recycler-view/recyclerview-example.md) zu veranschaulichen, wie Sie die Element-Click-Ereignisse behandeln, und aktualisieren Sie `RecyclerView` Wenn die Änderungen die zugrunde liegenden Datenquelle.


### <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden die Android `RecyclerView` Widget; es wurde erklärt, wie zum Hinzufügen der `RecyclerView` Unterstützungsbibliothek für Xamarin.Android-Projekte wie `RecyclerView` Ansichten wiederverwendet wird, wie das View-Inhaber-Muster für Effizienz erzwungen werden und wie die verschiedenen Hilfsklassen, aus denen `RecyclerView` zusammenarbeiten, um die Sammlungen angezeigt. Er Beispielcode zur Veranschaulichung bereitgestellt wie `RecyclerView` integriert ist für eine Anwendung wurde er erklärt, wie anpassen `RecyclerView`des Layoutrichtlinie von Plug-in-anderes Layout-Manager, und es wurde beschrieben, wie Element behandelt click-Ereignisse, und benachrichtigen Sie `RecyclerView`der Datenquelle geändert.

Weitere Informationen zu `RecyclerView`, finden Sie unter den [RecyclerView-Klassenreferenz](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html).


## <a name="related-links"></a>Verwandte Links

- [RecyclerViewer (Beispiel)](https://developer.xamarin.com/samples/monodroid/android5.0/RecyclerViewer)
- [Einführung in die Lollipop](~/android/platform/lollipop.md)
- [RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html)
