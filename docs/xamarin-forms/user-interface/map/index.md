---
title: Xamarin. Forms-Karte
description: Das Karten Steuerelement zeigt eine Karte an und erfordert das nuget-Paket xamarin. Forms. Maps.
ms.prod: xamarin
ms.assetid: B669B5EE-D24C-4C69-93E1-2CA5CC9108B5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/29/2019
ms.openlocfilehash: 013e126b76de08442327707cd0502f207826dad8
ms.sourcegitcommit: 3ea19e3a51515b30349d03c70a5b3acd7eca7fe7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/01/2019
ms.locfileid: "73425598"
---
# <a name="xamarinforms-map"></a>Xamarin. Forms-Karte

## <a name="initialization-and-configurationsetupmd"></a>[Initialisierung und Konfiguration](setup.md)

Das [xamarin. Forms. Maps](https://www.nuget.org/packages/Xamarin.Forms.Maps/) -nuget-Paket ist erforderlich, um Maps-Funktionen in einer Anwendung zu verwenden. Außerdem müssen für den Zugriff auf den Speicherort des Benutzers Standort Berechtigungen für die Anwendung erteilt worden sein.

## <a name="map-controlmapmd"></a>[Kartensteuerelement](map.md)

Das [`Map`](xref:Xamarin.Forms.Maps.Map) -Steuerelement ist eine plattformübergreifende Ansicht zum Anzeigen und kommentieren von Zuordnungen. Es verwendet das Native Karten Steuerelement für jede Plattform und bietet Benutzern eine schnelle und vertraute Zuordnungs Darstellung.

## <a name="position-and-distanceposition-distancemd"></a>[Position und Entfernung](position-distance.md)

Die [`Position`](xref:Xamarin.Forms.Maps.Position) -Struktur wird normalerweise verwendet, wenn eine Karte und Ihre Pins positioniert werden, und die [`Distance`](xref:Xamarin.Forms.Maps.Distance) Struktur, die optional beim Positionieren einer Karte verwendet werden kann.

## <a name="pinspinsmd"></a>[Pins](pins.md)

Mit dem [`Map`](xref:Xamarin.Forms.Maps.Map) -Steuerelement können Standorte mit [`Pin`](xref:Xamarin.Forms.Maps.Pin) Objekten gekennzeichnet werden. Ein `Pin` ist ein Zuordnungs Marker, der ein Informationsfenster öffnet, wenn er getippt wird.

## <a name="polygons-and-polylinespolygonsmd"></a>[Polygone und Polylinien](polygons.md)

die Elemente `Polygon` und `Polyline` ermöglichen es Ihnen, bestimmte Bereiche auf einer Karte hervorzuheben. Ein `Polygon` ist eine vollständig eingeschlossene Form, die einen Strich und eine Füllfarbe aufweisen kann. Ein `Polyline` ist eine Zeile, die einen Bereich nicht vollständig umschließt.

## <a name="geocodinggeocodermd"></a>[Geocodierung](geocoder.md)

Die [`Geocoder`](xref:Xamarin.Forms.Maps.Geocoder) -Klasse konvertiert zwischen Zeichen folgen Adressen und breiten-und Längenkoordinaten, die in [`Position`](xref:Xamarin.Forms.Maps.Position) -Objekten gespeichert werden.

## <a name="launch-the-native-map-appnative-map-appmd"></a>[Starten Sie die native Map-app.](native-map-app.md)

Die native Map-App auf jeder Plattform kann von einer xamarin. Forms-Anwendung durch die xamarin. Essentials `Launcher`-Klasse gestartet werden.
