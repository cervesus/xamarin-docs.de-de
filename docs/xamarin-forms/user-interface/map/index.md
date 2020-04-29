---
title: Xamarin. Forms-Karte
description: Das Karten Steuerelement zeigt eine Karte an und erfordert das nuget-Paket xamarin. Forms. Maps.
ms.prod: xamarin
ms.assetid: B669B5EE-D24C-4C69-93E1-2CA5CC9108B5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/29/2019
ms.openlocfilehash: ffd8f7cc31707d09bb3442c180a867d31afcef0f
ms.sourcegitcommit: 8d13d2262d02468c99c4e18207d50cd82275d233
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "82517496"
---
# <a name="xamarinforms-map"></a>Xamarin. Forms-Karte

## <a name="initialization-and-configuration"></a>[Initialisierung und Konfiguration](setup.md)

Das [xamarin. Forms. Maps](https://www.nuget.org/packages/Xamarin.Forms.Maps/) -nuget-Paket ist erforderlich, um Maps-Funktionen in einer Anwendung zu verwenden. Außerdem müssen für den Zugriff auf den Speicherort des Benutzers Standort Berechtigungen für die Anwendung erteilt worden sein.

## <a name="map-control"></a>[Kartensteuerelement](map.md)

Das [`Map`](xref:Xamarin.Forms.Maps.Map) -Steuerelement ist eine plattformübergreifende Ansicht zum Anzeigen und kommentieren von Zuordnungen. Es verwendet das Native Karten Steuerelement für jede Plattform und bietet Benutzern eine schnelle und vertraute Zuordnungs Darstellung.

## <a name="position-and-distance"></a>[Position und Entfernung](position-distance.md)

Die [`Position`](xref:Xamarin.Forms.Maps.Position) Struktur wird in der Regel verwendet, wenn eine Karte und Ihre Pins positioniert [`Distance`](xref:Xamarin.Forms.Maps.Distance) werden und die Struktur, die optional beim Positionieren einer Karte verwendet werden kann.

## <a name="pins"></a>[Ösen](pins.md)

Mit [`Map`](xref:Xamarin.Forms.Maps.Map) dem-Steuerelement können Standorte mit [`Pin`](xref:Xamarin.Forms.Maps.Pin) -Objekten gekennzeichnet werden. Ein `Pin` ist ein Karten Marker, der ein Informationsfenster öffnet, wenn er getippt wird.

## <a name="polygons-polylines-and-circles"></a>[Polygone, Polylines und Kreise](polygons.md)

`Polygon`die `Polyline`Elemente, `Circle` und ermöglichen es Ihnen, bestimmte Bereiche auf einer Karte hervorzuheben. Eine `Polygon` ist eine vollständig eingeschlossene Form, die einen Strich und eine Füllfarbe aufweisen kann. Ein `Polyline` ist eine Zeile, die einen Bereich nicht vollständig umschließt. Ein `Circle` markiert einen kreisförmigen Bereich der Karte.

## <a name="geocoding"></a>[Geocodierung](geocoder.md)

Die [`Geocoder`](xref:Xamarin.Forms.Maps.Geocoder) -Klasse konvertiert zwischen Zeichen folgen Adressen und breiten-und Längenkoordinaten, die [`Position`](xref:Xamarin.Forms.Maps.Position) in-Objekten gespeichert werden.

## <a name="launch-the-native-map-app"></a>[Starten Sie die native Map-app.](native-map-app.md)

Die native Map-App auf jeder Plattform kann von einer xamarin. Forms-Anwendung durch die xamarin. Essentials `Launcher` -Klasse gestartet werden.
