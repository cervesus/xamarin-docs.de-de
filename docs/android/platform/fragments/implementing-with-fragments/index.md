---
title: Mit Fragmenten implementieren
description: "Fragmente, Android 3.0 eingeführt. Fragmente sind eigenständige, modulare Komponenten, mit denen die Komplexität beim Schreiben von Anwendungen, die auf Bildschirmen mit unterschiedlichen Größen ausgeführt werden können, bewältigt werden kann. Dieser Artikel führt Sie durch wie Fragmente verwenden, um Xamarin.Android Anwendungen zu entwickeln und wie Fragmente auf vorab Android 3.0-Geräten unterstützt."
ms.topic: article
ms.prod: xamarin
ms.assetid: A71E9D87-CB69-10AB-CE51-357A05C76BCD
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: ebb53398edba64e255f1a534556836df8734ba6f
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="implementing-with-fragments"></a>Mit Fragmenten implementieren

_Fragmente, Android 3.0 eingeführt. Fragmente sind eigenständige, modulare Komponenten, mit denen die Komplexität beim Schreiben von Anwendungen, die auf Bildschirmen mit unterschiedlichen Größen ausgeführt werden können, bewältigt werden kann. Dieser Artikel führt Sie durch wie Fragmente verwenden, um Xamarin.Android Anwendungen zu entwickeln und wie Fragmente auf vorab Android 3.0-Geräten unterstützt._

<a name="Overview" />

## <a name="overview"></a>Übersicht

In diesem Abschnitt werden protokollsuchen Gewusst wie: Erstellen einer Anwendung, die eine Liste der hohen spielt und ein Anführungszeichen in jeder ausgewählten Play angezeigt werden. Dieser app nutzt Fragmente, damit wir unsere UI-Komponenten zentral definieren können, aber verwenden diese auf verschiedenen Formfaktoren arbeiten. Die folgenden Screenshots zeigen z. B. die Anwendung auf einem Tablet 10" sowie auf einem Telefon ausgeführt:

[![Screenshots der Beispiel-app, die unter dem Tablet oder Telefon](images/intro-screenshot-sml.png)](images/intro-screenshot.png)

Dieser Abschnitt befasst sich mit die folgenden Themen:

- **Erstellen von Fragmenten** &ndash; wird gezeigt, wie ein Fragment, um eine Liste der hohen spielt anzuzeigen und ein anderes Fragment anzuzeigenden ein Angebot aus jeder Play zu erstellen.

- **Unterstützen von unterschiedlichen Bildschirmgrößen** &ndash; zeigt das Layout der Anwendung zu größeren Bildschirmgrößen nutzen.

- **Verwenden das Android Supportpaket** &ndash; implementiert das Paket für Android unterstützt, und klicken Sie dann einige kleinere Änderungen an die Aktivitäten in der Anwendung ermöglicht unter älteren Versionen von Android ausgeführt wird.

<a name="Requirements" />

## <a name="requirements"></a>Anforderungen

Diese exemplarische Vorgehensweise erfordert Xamarin.Android 4.0 oder höher. Es sind auch erforderlich, um das Android Supportpaket zu installieren, da in der Fragmente-Dokumentation beschrieben.

<a name="Introduction" />

## <a name="introduction"></a>Einführung

In dem Beispiel in diesem Abschnitt erstellen müssen, enthalten die Aktivitäten keine Logik zum Laden der Liste, reagieren auf die Auswahl des Benutzers oder das Angebot für die ausgewählten Play anzeigen. Diese Logik, die in den einzelnen Fragmenten vorhanden ist.
Platzieren Sie diese Logik in den Fragmenten selbst, können wir die Anwendung zur Unterstützung von großer Bildschirmen mit einer Aktivität oder kleine Bildschirme mit mehreren Aktivitäten ohne abweichender Logik für jede Aktivität Schreiben des Workflows aufteilen. Auf einem Tablet-PC werden die beiden Fragmenten in einer Aktivität. Auf einem Telefon werden die Fragmente in unterschiedlichen Aktivitäten gehostet werden.

Diese Anwendung besteht aus folgenden Teilen:

 **Verwendung des Layoutnamens** – zeigt eine oder beide der Fragmente, abhängig von der Größe des Bildschirms. Hierbei handelt es sich um den Start-Aktivität.

 **TitlesFragment** – zeigt eine Liste der hohen spielt in dem der Benutzer auswählen kann.

 **DetailsFragment** – zeigt die anführung wie in den ausgewählten wiedergeben.

 **DetailsActivity** – hostet, und zeigt die DetailsFragment.
Diese Aktivität wird von Geräten mit kleine Bildschirme, z. B. Telefone.



## <a name="related-links"></a>Verwandte Links

- [FragmentsWalkthrough (Beispiel)](https://developer.xamarin.com/samples/monodroid/FragmentsWalkthrough/)
- [Übersicht über-Designer](~/android/user-interface/android-designer/index.md)
- [Xamarin.Android Beispiele: Wabe-Katalog](https://developer.xamarin.com/samples/HoneycombGallery/)
- [Implementieren von Fragmenten](http://developer.android.com/guide/topics/fundamentals/fragments.html)
- [Supportpaket](http://developer.android.com/sdk/compatibility-library.html)
