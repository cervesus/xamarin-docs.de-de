---
title: Einführung zu contentproviders
description: Das Android-Betriebssystem verwendet Inhaltsanbieter, um den Zugriff auf freigegebene Daten (z. b. Mediendateien, Kontakte und Kalenderinformationen) zu vereinfachen. In diesem Artikel wird die Contentprovider-Klasse vorgestellt, und es werden zwei Beispiele für deren Verwendung bereitstellt.
ms.prod: xamarin
ms.assetid: 6E1810AA-EB70-9AD0-1B32-D9418908CC97
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/09/2018
ms.openlocfilehash: 496e5c092c79f4f71bddaad30bea6acd1d58d375
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73027539"
---
# <a name="intro-to-contentproviders"></a>Einführung zu contentproviders

_Das Android-Betriebssystem verwendet Inhaltsanbieter, um den Zugriff auf freigegebene Daten (z. b. Mediendateien, Kontakte und Kalenderinformationen) zu vereinfachen. In diesem Artikel wird die Contentprovider-Klasse vorgestellt, und es werden zwei Beispiele für deren Verwendung bereitstellt._

## <a name="content-providers-overview"></a>Übersicht über Inhaltsanbieter

Ein *Contentprovider* kapselt ein Datenrepository und stellt eine API für den Zugriff bereit. Der Anbieter ist als Teil einer Android-Anwendung vorhanden, die in der Regel auch eine Benutzeroberfläche zum Anzeigen und Verwalten der Daten bereitstellt. Der Hauptvorteil der Verwendung eines Inhalts Anbieters besteht darin, dass andere Anwendungen mithilfe eines Anbieter Client Objekts (als " *contentresolver*" bezeichnet) problemlos auf die gekapselten Daten zugreifen können. Ein Inhaltsanbieter und eine Inhalts Auflösung bieten eine konsistente Anwendungsübergreifende API für den Datenzugriff, die einfach zu erstellen und zu nutzen ist. Jede Anwendung kann `ContentProviders` verwenden, um Daten intern zu verwalten und Sie auch anderen Anwendungen zur Verfügung zu stellen.

Eine `ContentProvider` ist auch erforderlich, damit Ihre Anwendung benutzerdefinierte Suchvorschläge bereitstellen kann, oder wenn Sie komplexe Daten aus Ihrer Anwendung kopieren möchten, um Sie in andere Anwendungen einzufügen. In diesem Dokument wird gezeigt, wie Sie mit xamarin. Android auf `ContentProviders` zugreifen und diese erstellen.

Die Struktur dieses Abschnitts lautet wie folgt:

- **Funktionsweise** &ndash; eine Übersicht darüber, wofür das `ContentProvider` konzipiert ist und wie es funktioniert.

- Verwenden **eines Inhalts Anbieters** &ndash; ein Beispiel für den Zugriff auf die Liste Kontakte.

- **Verwenden von Contentprovider zum Freigeben von Daten** &ndash; schreiben und Nutzen eines `ContentProvider` in derselben Anwendung.

`ContentProviders` und die Cursor, die mit Ihren Daten arbeiten, werden häufig verwendet, um ListViews aufzufüllen. Weitere Informationen zur Verwendung dieser Klassen finden Sie im [Handbuch zu ListViews und Adaptern](~/android/user-interface/layouts/list-view/index.md) .

`ContentProviders`, die von Android (oder anderen Anwendungen) verfügbar gemacht werden, sind eine einfache Möglichkeit, Daten aus anderen Quellen in Ihre Anwendung einzubeziehen. Sie ermöglichen Ihnen den Zugriff auf und die Darstellung von Daten, wie z. b. die Liste Kontakte, Fotos oder Kalenderveranstaltungen innerhalb Ihrer Anwendung, und ermöglichen dem Benutzer die Interaktion mit diesen Daten.

Benutzerdefinierte `ContentProviders` sind eine bequeme Möglichkeit zum Verpacken Ihrer Daten für die Verwendung in ihrer eigenen App oder zur Verwendung durch andere Anwendungen (z. b. für die benutzerdefinierte Suche und das Kopieren/Einfügen).

Die Themen in diesem Abschnitt enthalten einige einfache Beispiele für das verarbeiten und Schreiben von `ContentProvider`-Code.

## <a name="related-links"></a>Verwandte Links

- [Contacttadapter-Demo (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/platformfeatures-contactsadapterdemo)
- [Simplecontentprovider (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/platformfeatures-simplecontentprovider)
- [Entwicklerhandbuch zu Inhaltsanbietern](https://developer.android.com/guide/topics/providers/content-providers.html)
- [Contentprovider-Klassenreferenz](xref:Android.Content.ContentProvider)
- [Contentresolver-Klassenreferenz](xref:Android.Content.ContentResolver)
- [ListView-Klassen Verweis](xref:Android.Widget.ListView)
- [CursorAdapter-Klassenreferenz](xref:Android.Widget.CursorAdapter)
- [Urimatcher-Klassenreferenz](xref:Android.Content.UriMatcher)
- [Android. Provider](xref:Android.Provider)
- [Contaczcontract-Klassenreferenz](xref:Android.Provider.ContactsContract)
