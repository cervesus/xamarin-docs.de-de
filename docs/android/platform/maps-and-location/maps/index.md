---
title: Verwenden von Google Maps und dem Standort mit Xamarin.Android
description: In diesem Artikel wird erläutert, wie Sie Karten und Standortinformationen mit Xamarin.Android verwenden. Es werden verschiedene Aspekte behandelt – von der Nutzung der integrierten Kartenanwendung bis hin zur direkten Verwendung der Android-API-V2 von Google Maps.
ms.prod: xamarin
ms.assetid: 425E0ED2-5380-6EBE-7059-256B6E9128B8
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/25/2018
ms.openlocfilehash: d877f415bb96024bb41edc2be9aec108ae248e88
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2020
ms.locfileid: "73020032"
---
# <a name="how-to-use-google-maps-and-location-with-xamarinandroid"></a>Verwenden von Google Maps und dem Standort mit Xamarin.Android

_In diesem Artikel wird erläutert, wie Sie Karten und Standortinformationen mit Xamarin.Android verwenden. Es werden verschiedene Aspekte behandelt – von der Nutzung der integrierten Kartenanwendung bis hin zur direkten Verwendung der Android-API-V2 von Google Maps._

## <a name="maps-overview"></a>Übersicht über Karten

Kartentechnologien sind eine universelle Ergänzung für mobile Geräte. In Desktopcomputern und Laptops ist normalerweise keine Standortermittlung integriert. Mobile Geräte verwenden jedoch solche Anwendungen, um Geräte zu suchen und sich ändernde Standortinformationen anzuzeigen. Android verfügt über eine leistungsstarke integrierte Technologie, die Standortdaten auf Karten mithilfe einer Standortsoftware anzeigt, die auf dem Gerät verfügbar sein kann. Dieser Artikel behandelt eine ganzes Spektrum an Möglichkeiten, die Kartenanwendungen unter Xamarin.Android bieten können, einschließlich der folgenden: 

- Verwenden integrierter Kartenanwendungen zum schnellen Hinzufügen von Kartenfunktionen
- Arbeiten mit der Karten-API, um die Anzeige einer Karte zu steuern
- Verwenden verschiedener Techniken zum Hinzufügen grafischer Überlagerungen

Die Themen in diesem Abschnitt behandeln eine Vielzahl von Kartenfeatures.
Zunächst wird erläutert, wie die in Android integrierte Kartenanwendung genutzt wird und wie eine Panoramaansicht eines Orts angezeigt wird. Anschließend wird thematisiert, wie die Karten-API verwendet wird, um Kartenfeatures direkt in einer Anwendung zu integrieren, was sowohl die Steuerung der Position als auch die Kartenansicht abdeckt und auch, wie grafische Überlagerungen hinzugefügt werden.

## <a name="related-links"></a>Verwandte Links

- [MapsAndLocationDemo_v3 (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/mapsandlocationdemo-v3)
- [Aktivitätslebenszyklus](~/android/app-fundamentals/activity-lifecycle/index.md)
- [Abrufen eines API-Schlüssels für Google Maps](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md)
- [Liste der Absichten: Aufrufen von Google-Anwendungen auf Android-Geräten](https://developer.android.com/guide/appendix/g-app-intents.html)
- [Position und Karten](https://developer.android.com/guide/topics/location/index.html)
