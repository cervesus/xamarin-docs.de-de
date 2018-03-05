---
title: "Einführung in ContentProviders"
description: "Die Android-Betriebssystem verwendet Inhaltsanbieter, um Zugriff auf freigegebene Daten wie Mediendateien, Kontakte und Kalenderinformationen zu vereinfachen. Dieser Artikel führt die ContentProvider-Klasse und enthält zwei Beispiele für die Art der Verwendung."
ms.topic: article
ms.prod: xamarin
ms.assetid: 6E1810AA-EB70-9AD0-1B32-D9418908CC97
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.openlocfilehash: 42a20f4f0bc16cf42aa54fd0758db8a5db7c9048
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="intro-to-contentproviders"></a>Einführung in ContentProviders

_Die Android-Betriebssystem verwendet Inhaltsanbieter, um Zugriff auf freigegebene Daten wie Mediendateien, Kontakte und Kalenderinformationen zu vereinfachen. Dieser Artikel führt die ContentProvider-Klasse und enthält zwei Beispiele für die Art der Verwendung._


## <a name="content-providers-overview"></a>Inhaltsanbieter (Übersicht)

Ein *ContentProvider* kapselt ein Datenrepository und stellt eine API, um darauf zuzugreifen. Der Anbieter, die als Teil einer Android-Anwendung, die in der Regel auch eine Benutzeroberfläche enthält, die zum Anzeigen von/Verwalten der Daten vorhanden ist. Der wichtigste Vorteil der Verwendung von Content-Anbieter ermöglicht anderen Anwendungen einfach die gekapselten Zugriff auf Daten mit einer Client-Anbieterobjekt (bezeichnet eine *ContentResolver*). Zusammen bieten einen Inhaltsanbieter und inhaltsresolvers für eine konsistente zwischen Anwendung-API für den Datenzugriff, die einfach zu erstellen und nutzen. Jede Anwendung verwenden können `ContentProviders` Daten intern zu verwalten und ihn für andere Anwendungen verfügbar zu machen.

Ein `ContentProvider` sind ebenso Voraussetzung für die Anwendung benutzerdefinierte Suchvorschläge, bereitstellen oder wenn Sie die Möglichkeit bieten, komplexe Daten aus Ihrer Anwendung in andere Anwendungen Einfügen kopieren möchten. Dieses Dokument zeigt, wie zugreifen, und erstellen Sie `ContentProviders` mit Xamarin.Android.

Die Struktur der in diesem Abschnitt ist wie folgt aus:

- **Informationen zur Funktionsweise** &ndash; eine Übersicht über die `ContentProvider` dient für, und wie es funktioniert.

- **Verarbeiten einer Inhaltsanbieter** &ndash; ein Beispiel für den Zugriff auf die Liste "Kontakte".

- **Freigeben von Daten über ContentProvider** &ndash; schreiben und nutzen einen `ContentProvider` in der gleichen Anwendung.

`ContentProviders` und der Cursor, die ihre Daten bearbeitet werden dienen häufig Listenansichten aufzufüllen. Finden Sie in der [Listenansichten und Adaptern Handbuch](~/android/user-interface/layouts/list-view/index.md) für Weitere Informationen zur Verwendung dieser Klassen.

`ContentProviders` von verfügbar gemachten Android (oder anderen Anwendungen) sind eine einfache Möglichkeit zum Einschließen von Daten aus anderen Quellen in Ihrer Anwendung. Sie können Sie zugreifen und die Darstellung der Daten z. B. die Kontakte-Liste, Fotos oder Kalenderereignisse innerhalb der Anwendung und der Benutzer, die bei der Interaktion mit diesen Daten können.

Benutzerdefinierte `ContentProviders` sind eine einfache Möglichkeit, Ihre Daten für die Verwendung in Ihrer eigenen app oder für die Verwendung von anderen Anwendungen (einschließlich spezieller Einsatzzwecke wie benutzerdefinierte Such- und Kopieren/Einfügen) verpacken.

Die Themen in diesem Abschnitt enthalten einige einfache Beispiele nutzen und das Schreiben von `ContentProvider` Code.



## <a name="related-links"></a>Verwandte Links

- [ContactsAdapter Demo (Beispiel)](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/ContactsAdapterDemo/)
- [SimpleContentProvider (sample)](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/SimpleContentProvider)
- [Inhaltsanbieter Developers-Guide](http://developer.android.com/guide/topics/providers/content-providers.html)
- [ContentProvider-Klassenreferenz](https://developer.xamarin.com/api/type/Android.Content.ContentProvider/)
- [ContentResolver-Klassenreferenz](https://developer.xamarin.com/api/type/Android.Content.ContentResolver/)
- [ListView-Klassenreferenz](https://developer.xamarin.com/api/type/Android.Widget.ListView/)
- [CursorAdapter Klassenreferenz](https://developer.xamarin.com/api/type/Android.Widget.CursorAdapter/)
- [UriMatcher-Klassenreferenz](https://developer.xamarin.com/api/type/Android.Content.UriMatcher/)
- [Android.Provider](https://developer.xamarin.com/api/namespace/Android.Provider/)
- [ContactsContract-Klassenreferenz](https://developer.xamarin.com/api/type/Android.Provider.ContactsContract/)
