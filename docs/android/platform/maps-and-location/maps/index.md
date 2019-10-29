---
title: Verwenden von Google Maps und Location mit xamarin. Android
description: In diesem Artikel wird erläutert, wie Maps und Location mit xamarin. Android verwendet werden. Dabei wird alles von der Verwendung der integrierten Maps-Anwendung zur direkten Verwendung der Google Maps Android-API v2 behandelt.
ms.prod: xamarin
ms.assetid: 425E0ED2-5380-6EBE-7059-256B6E9128B8
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/25/2018
ms.openlocfilehash: d877f415bb96024bb41edc2be9aec108ae248e88
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73020032"
---
# <a name="how-to-use-google-maps-and-location-with-xamarinandroid"></a>Verwenden von Google Maps und Location mit xamarin. Android

_In diesem Artikel wird erläutert, wie Maps und Location mit xamarin. Android verwendet werden. Dabei wird alles von der Verwendung der integrierten Maps-Anwendung zur direkten Verwendung der Google Maps Android-API v2 behandelt._

## <a name="maps-overview"></a>Übersicht über Maps

Mapping-Technologien sind eine universelle Ergänzung für mobile Geräte. Desktop Computer und Laptops sind nicht in der Lage, die Standortinformationen integriert zu haben. Auf der anderen Seite verwenden mobile Geräte solche Anwendungen, um Geräte zu suchen und sich ändernde Standortinformationen anzuzeigen. Android verfügt über eine leistungsstarke, integrierte Technologie, die Standortdaten in Maps mithilfe von Standort Hardware anzeigt, die auf dem Gerät verfügbar sein kann. In diesem Artikel wird ein Spektrum der Zuordnungen von Anwendungen unter xamarin. Android behandelt, einschließlich: 

- Mithilfe der integrierten Maps-Anwendung können Sie schnell Zuordnungs Funktionen hinzufügen.
- Arbeiten mit der Maps-API, um die Anzeige einer Karte zu steuern.
- Verwenden verschiedener Techniken zum Hinzufügen grafischer Überlagerungen.

Die Themen in diesem Abschnitt behandeln eine Vielzahl von Karten Features.
Zuerst wird erläutert, wie die integrierte Maps-Anwendung von Android genutzt wird und wie eine Panoramaansicht eines Orts angezeigt wird. Anschließend wird erläutert, wie Sie mit der Maps-API Zuordnungs Features direkt in eine Anwendung integrieren. Außerdem wird erläutert, wie Sie die Position und Anzeige einer Karte Steuern und grafische Überlagerungen hinzufügen.

## <a name="related-links"></a>Verwandte Links

- [MapsAndLocationDemo_v3 (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/mapsandlocationdemo-v3)
- [Aktivitätslebenszyklus](~/android/app-fundamentals/activity-lifecycle/index.md)
- [Abrufen eines API-Schlüssels für Google Maps](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md)
- [Intents-Liste: Aufrufen von Google-Anwendungen auf Android-Geräten](https://developer.android.com/guide/appendix/g-app-intents.html)
- [Speicherort und Zuordnungen](https://developer.android.com/guide/topics/location/index.html)
