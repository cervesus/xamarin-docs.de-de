---
title: Einführung zu contentproviders
description: Das Android-Betriebssystem verwendet Inhaltsanbieter, um den Zugriff auf freigegebene Daten (z. b. Mediendateien, Kontakte und Kalenderinformationen) zu vereinfachen. In diesem Artikel wird die Contentprovider-Klasse vorgestellt, und es werden zwei Beispiele für deren Verwendung bereitstellt.
ms.prod: xamarin
ms.assetid: 6E1810AA-EB70-9AD0-1B32-D9418908CC97
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/09/2018
ms.openlocfilehash: 2533ad80571e2c8fe94cb4a2dcb0ec0ff0dd68cb
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68643589"
---
# <a name="intro-to-contentproviders"></a>Einführung zu contentproviders

_Das Android-Betriebssystem verwendet Inhaltsanbieter, um den Zugriff auf freigegebene Daten (z. b. Mediendateien, Kontakte und Kalenderinformationen) zu vereinfachen. In diesem Artikel wird die Contentprovider-Klasse vorgestellt, und es werden zwei Beispiele für deren Verwendung bereitstellt._


## <a name="content-providers-overview"></a>Übersicht über Inhaltsanbieter

Ein *Contentprovider* kapselt ein Datenrepository und stellt eine API für den Zugriff bereit. Der Anbieter ist als Teil einer Android-Anwendung vorhanden, die in der Regel auch eine Benutzeroberfläche zum Anzeigen und Verwalten der Daten bereitstellt. Der Hauptvorteil der Verwendung eines Inhalts Anbieters besteht darin, dass andere Anwendungen mithilfe eines Anbieter Client Objekts (als " *contentresolver*" bezeichnet) problemlos auf die gekapselten Daten zugreifen können. Ein Inhaltsanbieter und eine Inhalts Auflösung bieten eine konsistente Anwendungsübergreifende API für den Datenzugriff, die einfach zu erstellen und zu nutzen ist. Jede Anwendung kann für die interne `ContentProviders` Verwaltung von Daten und auch für andere Anwendungen verwenden.

Außerdem `ContentProvider` ist ein erforderlich, damit Ihre Anwendung benutzerdefinierte Suchvorschläge bereitstellen kann, oder wenn Sie komplexe Daten aus Ihrer Anwendung kopieren möchten, um Sie in andere Anwendungen einzufügen. In diesem Dokument wird gezeigt, wie Sie `ContentProviders` mit xamarin. Android auf Sie zugreifen und diese erstellen.

Die Struktur dieses Abschnitts lautet wie folgt:

- **Funktionsweise** Eine Übersicht über das `ContentProvider` , wofür entworfen wurde und wie es funktioniert. &ndash;

- Nutzen **eines Inhalts Anbieters** &ndash; Ein Beispiel für den Zugriff auf die Liste Kontakte.

- **Verwenden von Contentprovider zum Freigeben von Daten** Schreiben und Verarbeiten eines `ContentProvider` in derselben Anwendung. &ndash;

`ContentProviders`und die Cursor, die mit Ihren Daten arbeiten, werden häufig verwendet, um ListViews aufzufüllen. Weitere Informationen zur Verwendung dieser Klassen finden Sie im [Handbuch zu ListViews und Adaptern](~/android/user-interface/layouts/list-view/index.md) .

`ContentProviders`verfügbar gemacht von Android (oder anderen Anwendungen) ist eine einfache Möglichkeit, Daten aus anderen Quellen in Ihre Anwendung einzubeziehen. Sie ermöglichen Ihnen den Zugriff auf und die Darstellung von Daten, wie z. b. die Liste Kontakte, Fotos oder Kalenderveranstaltungen innerhalb Ihrer Anwendung, und ermöglichen dem Benutzer die Interaktion mit diesen Daten.

Benutzer `ContentProviders` definiert sind eine bequeme Möglichkeit, Ihre Daten für die Verwendung in ihrer eigenen App zu verpacken, oder für die Verwendung durch andere Anwendungen (einschließlich spezieller Verwendung, z. b. benutzerdefinierte Suche und kopieren/einfügen).

Die Themen in diesem Abschnitt enthalten einige einfache Beispiele für das verarbeiten und `ContentProvider` Schreiben von Code.



## <a name="related-links"></a>Verwandte Links

- [Contacttadapter-Demo (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/platformfeatures-contactsadapterdemo)
- [Simplecontentprovider (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/platformfeatures-simplecontentprovider)
- [Entwicklerhandbuch zu Inhaltsanbietern](https://developer.android.com/guide/topics/providers/content-providers.html)
- [Contentprovider-Klassenreferenz](xref:Android.Content.ContentProvider)
- [Contentresolver-Klassenreferenz](xref:Android.Content.ContentResolver)
- [ListView-Klassen Verweis](xref:Android.Widget.ListView)
- [CursorAdapter-Klassenreferenz](xref:Android.Widget.CursorAdapter)
- [Urimatcher-Klassenreferenz](xref:Android.Content.UriMatcher)
- [Android.Provider](xref:Android.Provider)
- [Contaczcontract-Klassenreferenz](xref:Android.Provider.ContactsContract)
