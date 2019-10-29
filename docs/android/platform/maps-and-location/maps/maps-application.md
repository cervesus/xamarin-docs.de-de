---
title: Starten der Maps-Anwendung
description: Starten der integrierten Maps-Anwendung in ihrer xamarin. Android-App.
ms.prod: xamarin
ms.assetid: 929EACB8-8950-50E1-093C-43FB5F1F1CD5
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/25/2018
ms.openlocfilehash: 7b74f564f2b6e9613874a774258a7e999002e61a
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73027078"
---
# <a name="launching-the-maps-application"></a>Starten der Maps-Anwendung

Die einfachste Methode zum Arbeiten mit Maps in xamarin. Android besteht darin, die im folgenden gezeigte integrierte Maps-Anwendung zu nutzen:

[Screenshot der integrierten Google Maps-APP![Beispiel](maps-application-images/01-mapsapplication.png)](maps-application-images/01-mapsapplication.png#lightbox)

Wenn Sie die Maps-Anwendung verwenden, ist die Zuordnung nicht Teil Ihrer Anwendung. Stattdessen startet die Anwendung die Maps-Anwendung und lädt die Karte extern. Im nächsten Abschnitt wird erläutert, wie mit xamarin. Android Zuordnungen wie oben beschrieben gestartet werden.

## <a name="creating-the-intent"></a>Erstellen der Absicht

Das Arbeiten mit der Maps-Anwendung ist so einfach wie das Erstellen einer Absicht mit einem geeigneten URI, das Festlegen der Aktion auf Action View und das Aufrufen der startactivity-Methode. Mit dem folgenden Code wird z. b. die Maps-Anwendung mit einem bestimmten breiten-und Längengrad gestartet:

```csharp
var geoUri = Android.Net.Uri.Parse ("geo:42.374260,-71.120824");
var mapIntent = new Intent (Intent.ActionView, geoUri);
StartActivity (mapIntent);
```

Dieser Code ist alles, was zum Starten der im vorherigen Screenshot gezeigten Karte erforderlich ist. Zusätzlich zur Angabe von breiten-und Längengrad unterstützt das URI-Schema für Maps verschiedene andere Optionen.

## <a name="geo-uri-scheme"></a>Geografisches URI-Schema

Der obige Code hat das geografieschema verwendet, um einen URI zu erstellen. Dieses URI-Schema unterstützt verschiedene Formate, wie unten aufgeführt:

- `geo:latitude,longitude` &ndash; öffnet die Maps-Anwendung, die sich in einem lat/lon zentriert. 

- `geo:latitude,longitude?z=zoom` &ndash; öffnet die Maps-Anwendung, die sich in einem lat/lon zentriert und auf die angegebene Ebene vergrößert. Der Zoomfaktor liegt zwischen 1 und 23:1 zeigt die gesamte Erde an, und 23 ist die nächstgelegene Zoomstufe.

- `geo:0,0?q=my+street+address` &ndash; öffnet die Anwendung Maps am Speicherort einer Straße. 

- `geo:0,0?q=business+near+city` &ndash; öffnet die Maps-Anwendung und zeigt die mit Anmerkungen versehene Suchergebnisse an. 

Die Versionen des URIs, die eine Abfrage ausführen (d. h. die Adresse der Straße oder die Suchbegriffe), verwenden den Geocoder-Dienst von Google, um den Speicherort abzurufen, der dann auf der Karte angezeigt wird. Der URI `geo:0,0?q=coop+Cambridge` beispielsweise in der folgenden Abbildung dargestellt:

[Screenshot des![Beispiels, der Google Maps mit einem Suchbegriff anzeigt](maps-application-images/02-mapsearch.png)](maps-application-images/02-mapsearch.png#lightbox)

Weitere Informationen zu den URI-Schemas finden Sie unter [Show a Location on a map](https://developer.android.com/guide/components/intents-common.html#Maps).

## <a name="street-view"></a>Straße

Zusätzlich zum geografieschema unterstützt Android auch das Laden von Straßenansichten von einer Absicht. Im folgenden finden Sie ein Beispiel für die aus xamarin. Android gestartete Anwendung "Street View":

[Screenshot einer Straße![Beispiel](maps-application-images/03-streetview.png)](maps-application-images/03-streetview.png#lightbox)

Um eine Straßenansicht zu starten, verwenden Sie einfach das `google.streetview` URI-Schema, wie im folgenden Code gezeigt:

```csharp
var streetViewUri = Android.Net.Uri.Parse (
       "google.streetview:cbll=42.374260,-71.120824&cbp=1,90,,0,1.0&mz=20");  
var streetViewIntent = new Intent (Intent.ActionView, streetViewUri);  
StartActivity (streetViewIntent);
```

Das oben verwendete Google. Streetview-URI-Schema hat die folgende Form:

```csharp
google.streetview:cbll=lat,lng&cbp=1,yaw,,pitch,zoom&mz=mapZoom
```

Wie Sie sehen können, werden mehrere Parameter unterstützt, wie unten aufgeführt:

- `lat` &ndash; den Breitengrad des Orts, der in der Ansicht "Straße" angezeigt werden soll.

- `lng` &ndash; den Längengrad der Position, die in der Ansicht "Straße" angezeigt werden soll.

- `pitch` &ndash; Winkel des Straßen Ansichts Panoramas, gemessen von der Mitte in Grad, bei der 90 Grad direkt nach unten und-90 Grad gerade ist.

- `yaw` &ndash; Mitte der Ansicht des Straßen Ansichts Panoramas, gemessen im Uhrzeigersinn in Grad von Nord.

- `zoom` &ndash; Zoom-Multiplikator für das Straßen Ansichts Panorama, wobei 1,0 = normaler Zoom Wert, 2,0 = vergrößert 2x, 3,0 = Zoom 4X usw.

- `mz` &ndash; den kartenzoomfaktor, der verwendet wird, wenn Sie die Maps-Anwendung aus der Ansicht "Straße" wechseln.

Das Arbeiten mit der integrierten Maps-Anwendung oder der Straßenansicht ist eine einfache Möglichkeit, um Unterstützung für die Zuordnung schnell hinzuzufügen. Allerdings bietet die Android Maps-API eine präzisere Steuerung der Zuordnungs Funktionen.
