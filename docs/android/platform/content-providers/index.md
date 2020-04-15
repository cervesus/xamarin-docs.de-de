---
title: Einführung zu ContentProviders
description: Das Android-Betriebssystem verwendet Inhaltsanbieter, um den Zugriff auf freigegebene Daten (z. B. Mediendateien, Kontakte und Kalenderinformationen) zu unterstützen. In diesem Artikel wird die ContentProvider-Klasse vorgestellt, und es werden zwei Beispiele für deren Verwendung gezeigt.
ms.prod: xamarin
ms.assetid: 6E1810AA-EB70-9AD0-1B32-D9418908CC97
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/09/2018
ms.openlocfilehash: 496e5c092c79f4f71bddaad30bea6acd1d58d375
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2020
ms.locfileid: "73027539"
---
# <a name="intro-to-contentproviders"></a>Einführung in ContentProviders

_Das Android-Betriebssystem verwendet Inhaltsanbieter, um den Zugriff auf freigegebene Daten (z. B. Mediendateien, Kontakte und Kalenderinformationen) zu unterstützen. In diesem Artikel wird die ContentProvider-Klasse vorgestellt, und es werden zwei Beispiele für deren Verwendung gezeigt._

## <a name="content-providers-overview"></a>Inhaltsanbieter: Übersicht

Ein *ContentProvider* kapselt ein Datenrepository und stellt eine API für den Zugriff darauf bereit. Der Anbieter ist als Teil einer Android-Anwendung vorhanden, die in der Regel auch eine Benutzeroberfläche zum Anzeigen und Verwalten der Daten bereitstellt. Der Hauptvorteil der Verwendung eines Inhaltsanbieters besteht darin, dass andere Anwendungen mithilfe eines Anbieterclientobjekts (als *ContentResolver* bezeichnet) problemlos auf die gekapselten Daten zugreifen können. Ein Inhaltsanbieter und eine Inhaltsauflösung bieten zusammen eine konsistente anwendungsübergreifende API für den Datenzugriff, die einfach zu erstellen und zu nutzen ist. Jede Anwendung kann `ContentProviders` verwenden, um Daten intern zu verwalten und sie auch für andere Anwendungen zur Verfügung zu stellen.

Ein `ContentProvider` ist auch erforderlich, damit Ihre Anwendung benutzerdefinierte Suchvorschläge bereitstellen kann, oder wenn Sie eine Möglichkeit zum Kopieren komplexer Daten aus Ihrer Anwendung bereitstellen möchten, um sie in andere Anwendungen einzufügen. In diesem Dokument wird gezeigt, wie Sie mit Xamarin.Android auf `ContentProviders` zugreifen und diese erstellen.

Dieser Abschnitt ist wie folgt gegliedert:

- **Funktionsweise** &ndash; Eine Übersicht darüber, welchem Zweck der `ContentProvider` dient und wie er funktioniert.

- **Nutzen eines Inhaltsanbieters** &ndash; Ein Beispiel für den Zugriff auf die Kontaktliste.

- **Verwenden von ContentProvider zum Freigeben von Daten** &ndash; Schreiben und Nutzen eines `ContentProvider` in der gleichen Anwendung.

`ContentProviders` und die Cursor, die mit deren Daten arbeiten, werden häufig verwendet, um ListViews mit Daten aufzufüllen. Weitere Informationen zur Verwendung dieser Klassen finden Sie im [Leitfaden zu ListViews und Adaptern](~/android/user-interface/layouts/list-view/index.md).

Von Android (oder anderen Anwendungen) bereitgestellte `ContentProviders` bieten eine einfache Möglichkeit, Daten aus anderen Quellen in Ihre Anwendung einzubeziehen. Sie ermöglichen Ihnen den Zugriff auf und die Darstellung von Daten (etwa von Kontaktlisten, Fotos oder Kalenderinformationen aus Ihrer Anwendung) und Benutzern die Interaktion mit diesen Daten.

Benutzerdefinierte `ContentProviders` sind eine bequeme Möglichkeit zum Verpacken Ihrer Daten für die Verwendung in Ihrer eigenen App oder durch andere Anwendungen (z. B. Sonderanwendungen wie benutzerdefinierte Suche und Kopieren/Einfügen).

Die Themen in diesem Abschnitt enthalten einige einfache Beispiele zum Nutzen und Schreiben von `ContentProvider`-Code.

## <a name="related-links"></a>Verwandte Links

- [ContactsAdapter-Demo (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/platformfeatures-contactsadapterdemo)
- [SimpleContentProvider (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/platformfeatures-simplecontentprovider)
- [Inhaltsanbieter: Entwicklerleitfaden](https://developer.android.com/guide/topics/providers/content-providers.html)
- [ContentProvider-Klassenreferenz](xref:Android.Content.ContentProvider)
- [ContentResolver-Klassenreferenz](xref:Android.Content.ContentResolver)
- [ListView-Klassenreferenz](xref:Android.Widget.ListView)
- [CursorAdapter-Klassenreferenz](xref:Android.Widget.CursorAdapter)
- [UriMatcher-Klassenreferenz](xref:Android.Content.UriMatcher)
- [Android.Provider](xref:Android.Provider)
- [ContactsContract-Klassenreferenz](xref:Android.Provider.ContactsContract)
