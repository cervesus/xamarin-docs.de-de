---
title: Einführung in die ContentProviders
description: 'Android-Betriebssysteme verwendet Inhaltsanbieter, um Zugriff auf freigegebene Daten wie z. B. Mediendateien, Kontakte und Kalenderinformationen zu ermöglichen. Dieser Artikel führt die ContentProvider-Klasse und enthält zwei Beispiele für dessen Verwendung.'
ms.prod: xamarin
ms.assetid: 6E1810AA-EB70-9AD0-1B32-D9418908CC97
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/09/2018
---

# <a name="intro-to-contentproviders"></a>Einführung in die ContentProviders

_Android-Betriebssysteme verwendet Inhaltsanbieter, um Zugriff auf freigegebene Daten wie z. B. Mediendateien, Kontakte und Kalenderinformationen zu ermöglichen. Dieser Artikel führt die ContentProvider-Klasse und enthält zwei Beispiele für dessen Verwendung._


## <a name="content-providers-overview"></a>Übersicht über die Inhaltsanbieter

Ein *ContentProvider* kapselt ein Datenrepository und stellt eine API, um darauf zuzugreifen. Der Anbieter, die als Teil einer Android-Anwendung, die in der Regel auch eine Benutzeroberfläche zum Anzeigen von ermöglicht/Verwalten der Daten vorhanden ist. Der Hauptvorteil der Verwendung von Content-Anbieter besteht darin, einfach den gekapselten Daten mithilfe von einem Anbieter-Objekt Zugriff auf andere Anwendungen ermöglichen (Namens eine *ContentResolver*). Zusammen bieten einen Inhaltsanbieter und inhaltsresolvers für eine konsistente zwischen Anwendungen API für den Datenzugriff, die einfach erstellen und nutzen. Jede Anwendung kann auch verwenden `ContentProviders` zum Verwalten von Daten intern und sie für andere Anwendungen verfügbar machen.

Ein `ContentProvider` sind ebenso Voraussetzung für Ihre Anwendung zum unterbreiten von Vorschlägen für die benutzerdefinierte Suche, oder wenn Sie die Möglichkeit, komplexe Daten aus Ihrer Anwendung zum Einfügen in andere Anwendungen kopieren möchten. In diesem Dokument wird gezeigt, wie Zugriff auf und erstellen `ContentProviders` mit Xamarin.Android.

Die Struktur der in diesem Abschnitt lautet wie folgt aus:

- **Funktionsweise** &ndash; was eine Übersicht über die `ContentProvider` dient für, und wie es funktioniert.

- **Nutzen Inhaltsanbieter** &ndash; ein Beispiel für den Zugriff auf die Contacts-Liste.

- **Verwenden von ContentProvider zum Freigeben von Daten** &ndash; schreiben und Nutzung einer `ContentProvider` in der gleichen Anwendung.

`ContentProviders` und die Cursor, die für ihre Daten verwendet werden werden oft verwendet, um ListViews aufzufüllen. Finden Sie in der [Handbuchs ListViews und Adapter](~/android/user-interface/layouts/list-view/index.md) für Weitere Informationen zur Verwendung dieser Klassen.

`ContentProviders` von verfügbar gemachten Android (oder anderen Anwendungen) sind eine einfache Möglichkeit, Daten aus anderen Quellen in Ihre Anwendung einbinden. Diese es Ihnen ermöglichen, Zugriff und die Darstellung der Daten wie z. B. die Kontakte-Liste, Fotos oder Kalenderereignisse aus innerhalb Ihrer Anwendung, und lassen Sie den Benutzer, die Interaktion mit diesen Daten.

Benutzerdefinierte `ContentProviders` sind eine einfache Möglichkeit zum Packen Ihrer Daten für die Verwendung in Ihrer eigenen app oder für die Verwendung von anderen Anwendungen (einschließlich spezieller verwendet wie für die benutzerdefinierte Suche "und" Kopieren/Einfügen).

Die Themen in diesem Abschnitt enthalten einige einfache Beispiele für Nutzung und Schreiben von `ContentProvider` Code.



## <a name="related-links"></a>Verwandte Links

- [ContactsAdapter-Demo (Beispiel)](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/ContactsAdapterDemo/)
- [SimpleContentProvider (Beispiel)](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/SimpleContentProvider)
- [Entwicklerhandbuch für Inhaltsanbieter](https://developer.android.com/guide/topics/providers/content-providers.html)
- [Referenz für die ContentProvider](https://developer.xamarin.com/api/type/Android.Content.ContentProvider/)
- [Geben ContentResolver-Klassenreferenz](https://developer.xamarin.com/api/type/Android.Content.ContentResolver/)
- [ListView-Klassenreferenz](https://developer.xamarin.com/api/type/Android.Widget.ListView/)
- [CursorAdapter-Klassenreferenz](https://developer.xamarin.com/api/type/Android.Widget.CursorAdapter/)
- [UriMatcher-Klassenreferenz](https://developer.xamarin.com/api/type/Android.Content.UriMatcher/)
- [Android.Provider](https://developer.xamarin.com/api/namespace/Android.Provider/)
- [ContactsContract-Klassenreferenz](https://developer.xamarin.com/api/type/Android.Provider.ContactsContract/)
