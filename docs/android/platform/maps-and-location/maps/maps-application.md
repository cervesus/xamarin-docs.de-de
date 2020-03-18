---
title: Starten der Maps-Anwendung
description: Hier finden Sie Informationen zum Starten der integrierten Maps-Anwendung aus Ihrer Xamarin.Android-App.
ms.prod: xamarin
ms.assetid: 929EACB8-8950-50E1-093C-43FB5F1F1CD5
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/25/2018
ms.openlocfilehash: 7b74f564f2b6e9613874a774258a7e999002e61a
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2020
ms.locfileid: "73027078"
---
# <a name="launching-the-maps-application"></a>Starten der Maps-Anwendung

Die einfachste Möglichkeit zum Arbeiten mit Karten in Xamarin.Android ist die Nutzung der unten gezeigten integrierten Maps-Anwendung:

[![Beispielscreenshot der integrierten Google Maps-App](maps-application-images/01-mapsapplication.png)](maps-application-images/01-mapsapplication.png#lightbox)

Wenn Sie die Maps-Anwendung verwenden, ist die Karte selbst kein Bestandteil Ihrer App. Stattdessen startet Ihre App die Maps-Anwendung und lädt die Karte extern. Im nächsten Abschnitt wird untersucht, wie Sie Xamarin.Android verwenden, um Karten wie die oben gezeigte zu starten.

## <a name="creating-the-intent"></a>Erstellen der Absicht

Die Arbeit mit der Maps-Anwendung ist ganz einfach: Sie erstellen eine Absicht mit einem geeigneten URI, legen die Aktion auf „ActionView“ fest und rufen die Methode „StartActivity“ auf. Der folgende Code beispielsweise startet die Maps-Anwendung zentriert auf einem bestimmten Breiten- und Längengrad:

```csharp
var geoUri = Android.Net.Uri.Parse ("geo:42.374260,-71.120824");
var mapIntent = new Intent (Intent.ActionView, geoUri);
StartActivity (mapIntent);
```

Mehr als dieser Code ist nicht erforderlich, um die im vorherigen Screenshot gezeigte Karte zu starten. Zusätzlich zur Angabe von Breiten- und Längengrad unterstützt das URI-Schema für Karten verschiedene weitere Optionen.

## <a name="geo-uri-scheme"></a>Geografisches URI-Schema

Der Code oben verwendet das geografische Schema, um einen URI zu erstellen. Dieses URI-Schema unterstützt verschiedene Formate, wie im Folgenden aufgeführt:

- `geo:latitude,longitude` &ndash; öffnet die Maps-Anwendung zentriert auf einem bestimmten Breiten- und Längengrad. 

- `geo:latitude,longitude?z=zoom` &ndash; öffnet die Maps-Anwendung zentriert auf einem bestimmten Breiten- und Längengrad und mit einem angegebenen Zoomfaktor. Der Zoomfaktor kann von 1 bis 23 reichen: 1 zeigt die gesamte Erde an, 23 ist die detaillierteste Stufe.

- `geo:0,0?q=my+street+address` &ndash; öffnet die Maps-Anwendung zentriert auf den Standort einer bestimmten Postanschrift. 

- `geo:0,0?q=business+near+city` &ndash; öffnet die Maps-Anwendung und zeigt die Suchergebnisse als Anmerkung an. 

Die Versionen der URI, die eine Abfrage akzeptieren (also eine Postanschrift oder einen Suchbegriff), verwenden den Geocoder-Dienst von Google, um den Standort abzurufen, der dann auf der Karte angezeigt wird. Der URI `geo:0,0?q=coop+Cambridge` beispielsweise resultiert in der folgenden Karte:

[![Beispielscreenshot von Google Maps mit einem Suchbegriff](maps-application-images/02-mapsearch.png)](maps-application-images/02-mapsearch.png#lightbox)

Weitere Informationen zu geografischen URI-Schemas finden Sie unter [Show a location on a map](https://developer.android.com/guide/components/intents-common.html#Maps) (Anzeigen eines Standorts auf einer Karte).

## <a name="street-view"></a>Straßenansicht

Zusätzlich zum geografischen Schema unterstützt Android auch das Laden von Straßenansichten aus einer Absicht. Im Folgenden sehen Sie eine Beispiel der von Xamarin.Android gestarteten Anwendung mit einer Straßenansicht:

[![Beispielscreenshot einer Straßenansicht](maps-application-images/03-streetview.png)](maps-application-images/03-streetview.png#lightbox)

Um eine Straßenansicht zu starten, verwenden Sie einfach das URI-Schema `google.streetview`, wie im folgenden Code gezeigt:

```csharp
var streetViewUri = Android.Net.Uri.Parse (
       "google.streetview:cbll=42.374260,-71.120824&cbp=1,90,,0,1.0&mz=20");  
var streetViewIntent = new Intent (Intent.ActionView, streetViewUri);  
StartActivity (streetViewIntent);
```

Das oben verwendete URI-Schema „google.streetview“ weist die folgende Form auf:

```csharp
google.streetview:cbll=lat,lng&cbp=1,yaw,,pitch,zoom&mz=mapZoom
```

Wie Sie sehen, werden verschiedene Parameter unterstützt:

- `lat` &ndash; der Breitengrad des Standorts, der in der Straßenansicht angezeigt werden soll.

- `lng` &ndash; der Längengrad des Standorts, der in der Straßenansicht angezeigt werden soll.

- `pitch` &ndash; Panorama der Straßenansicht mit veränderlicher vertikaler Ansicht, gemessen vom Zentrum und mit Gradangaben, wobei 90 Grad senkrecht nach unten und -90 Grad senkrecht nach oben bedeutet.

- `yaw` &ndash; Panorama der Straßenansicht mit veränderlicher horizontaler Ansicht, gemessen im Uhrzeigersinn, Ausgangspunkt ist Norden.

- `zoom` &ndash; Zoommultiplikator für das Straßenansichtspanorama, wobei 1,0 normalem Zoom, 2,0 zweifachem Zoom, 3,0 vierfachem Zoom usw. entspricht.

- `mz` &ndash; der Zoomfaktor der Karte, die beim Wechsel zur Maps-Anwendung aus der Straßenansicht verwendet wird.

Der Einsatz der integrierten Maps-Anwendung oder der Straßenansicht ist eine einfache Möglichkeit, schnell Unterstützung für Karten hinzuzufügen. Die Maps-API von Android bietet allerdings eine genauere Steuerung der Kartenfunktionen.
