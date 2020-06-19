---
title: Xamarin.Forms-Karte
description: Das Karten Steuerelement zeigt eine Karte an und erfordert Xamarin.Forms . Ordnet das nuget-Paket zu.
ms.prod: xamarin
ms.assetid: B669B5EE-D24C-4C69-93E1-2CA5CC9108B5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/29/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 2461ffa8168207e6a57fae005f752be48772a34a
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84139827"
---
# <a name="xamarinforms-map"></a>Xamarin.Forms-Karte

## <a name="initialization-and-configuration"></a>[Initialisierung und Konfiguration](setup.md)

Die [ Xamarin.Forms . Das Karten](https://www.nuget.org/packages/Xamarin.Forms.Maps/) -nuget-Paket ist erforderlich, um Maps-Funktionen in einer Anwendung zu verwenden. Außerdem müssen für den Zugriff auf den Speicherort des Benutzers Standort Berechtigungen für die Anwendung erteilt worden sein.

## <a name="map-control"></a>[Kartensteuerelement](map.md)

Das [`Map`](xref:Xamarin.Forms.Maps.Map) -Steuerelement ist eine plattformübergreifende Ansicht zum Anzeigen und kommentieren von Zuordnungen. Es verwendet das Native Karten Steuerelement für jede Plattform und bietet Benutzern eine schnelle und vertraute Zuordnungs Darstellung.

## <a name="position-and-distance"></a>[Position und Entfernung](position-distance.md)

Die [`Position`](xref:Xamarin.Forms.Maps.Position) Struktur wird in der Regel verwendet, wenn eine Karte und Ihre Pins positioniert werden und die Struktur, die [`Distance`](xref:Xamarin.Forms.Maps.Distance) optional beim Positionieren einer Karte verwendet werden kann.

## <a name="pins"></a>[Markierungen](pins.md)

Mit dem- [`Map`](xref:Xamarin.Forms.Maps.Map) Steuerelement können Standorte mit-Objekten gekennzeichnet werden [`Pin`](xref:Xamarin.Forms.Maps.Pin) . Ein `Pin` ist ein Karten Marker, der ein Informationsfenster öffnet, wenn er getippt wird.

## <a name="polygons-polylines-and-circles"></a>[Polygone, Polylinien und Kreise](polygons.md)

`Polygon``Polyline` `Circle` die Elemente, und ermöglichen es Ihnen, bestimmte Bereiche auf einer Karte hervorzuheben. Eine `Polygon` ist eine vollständig eingeschlossene Form, die einen Strich und eine Füllfarbe aufweisen kann. Ein `Polyline` ist eine Zeile, die einen Bereich nicht vollständig umschließt. Ein `Circle` markiert einen kreisförmigen Bereich der Karte.

## <a name="geocoding"></a>[Geocodierung](geocoder.md)

Die [`Geocoder`](xref:Xamarin.Forms.Maps.Geocoder) -Klasse konvertiert zwischen Zeichen folgen Adressen und breiten-und Längenkoordinaten, die in-Objekten gespeichert werden [`Position`](xref:Xamarin.Forms.Maps.Position) .

## <a name="launch-the-native-map-app"></a>[Starten der nativen Karten-App](native-map-app.md)

Die native Map-App auf jeder Plattform kann von einer- Xamarin.Forms Anwendung durch die-Klasse gestartet werden Xamarin.Essentials `Launcher` .
