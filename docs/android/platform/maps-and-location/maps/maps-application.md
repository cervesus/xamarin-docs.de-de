---
title: Starten die Maps-Anwendung
description: Wie Sie die integrierten Maps-Anwendung in Ihrer Xamarin.Android-app zu starten.
ms.prod: xamarin
ms.assetid: 929EACB8-8950-50E1-093C-43FB5F1F1CD5
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/25/2018
ms.openlocfilehash: fa32783617fce99514560677184f17be904cd42d
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2019
ms.locfileid: "57670321"
---
# <a name="launching-the-maps-application"></a>Starten die Maps-Anwendung

Die einfachste Möglichkeit, das mit Karten in Xamarin.Android funktioniert ist die integrierte Karten-Anwendung, die unten gezeigten nutzen:

[![Beispielhafter Screenshot der integrierten app von Google Maps](maps-application-images/01-mapsapplication.png)](maps-application-images/01-mapsapplication.png#lightbox)

Wenn Sie die Maps-Anwendung verwenden, wird die Zuordnung Teil Ihrer Anwendung nicht. Stattdessen wird Ihre Anwendung starten Sie die Maps-Anwendung, und laden die Zuordnung extern. Im nächste Abschnitt wird untersucht, wie Xamarin.Android verwendet wird, um Zuordnungen wie oben zu starten.


## <a name="creating-the-intent"></a>Erstellen die Absicht

Arbeiten mit den Karten ist die Anwendung so einfach wie das Erstellen eines Intents mit einem entsprechenden URI, die Aktion auf ActionView festlegen und die StartActivity-Methode aufrufen. Der folgende Code startet z. B. die Maps-Anwendung, zentriert bei einer angegebenen Längen- und Breitengrad:

```csharp
var geoUri = Android.Net.Uri.Parse ("geo:42.374260,-71.120824");
var mapIntent = new Intent (Intent.ActionView, geoUri);
StartActivity (mapIntent);
```

Dieser Code ist alles, die was benötigt wird, um die Zuordnung, die im vorherigen Screenshot gezeigt zu starten. Zusätzlich zur Angabe von Längen- und Breitengrad, unterstützt das URI-Schema für Maps zahlreiche andere Optionen.


## <a name="geo-uri-scheme"></a>Geo-URI-Schema

Der obige Code verwendet die Geo-Schema, um einen URI zu erstellen. Dieser URI-Schema unterstützt verschiedene Formate, wie im folgenden aufgeführt:

-   `geo:latitude,longitude` &ndash; Öffnet die Maps-Anwendung, die am Lon Lat/zentriert. 

-   `geo:latitude,longitude?z=zoom` &ndash; Öffnet die Zuordnungen Anwendung Lon Lat/zentriert und vergrößert, auf die angegebene Stufe. Die Zoomstufe reichen von 1 bis 23: 1 zeigt die gesamte Erde und 23, wird die nächste Zoomstufe.

-   `geo:0,0?q=my+street+address` &ndash; Öffnet die Maps-Anwendung auf den Speicherort der eine Straße und Hausnummer. 

-   `geo:0,0?q=business+near+city` &ndash; Öffnet die Maps-Anwendung, und zeigt die mit Anmerkungen versehene Suchergebnisse. 


Die Versionen des URIS, die eine Abfrage (d. h. die Straßen suchen oder die Adresse-Bedingungen) in Anspruch nehmen Verwenden Googles Geocoder Dienst, um den Speicherort abzurufen, der Sie dann auf der Karte angezeigt wird. Z. B. der URI `geo:0,0?q=coop+Cambridge` in der unten gezeigten Zuordnung führt:

[![Beispielscreenshot Google Maps mit dem Suchbegriff anzeigen](maps-application-images/02-mapsearch.png)](maps-application-images/02-mapsearch.png#lightbox)



Weitere Informationen über Geo-URI-Schemas finden Sie unter [Anzeigen von einem Speicherort auf einer Karte](https://developer.android.com/guide/components/intents-common.html#Maps).


## <a name="street-view"></a>Straße anzeigen

Zusätzlich zu der Geo-Schema unterstützt Android auch Straße und Ansichten aus ein Intent-Objekt geladen. Ein Beispiel für die Straße und Ansicht-Anwendung, die von Xamarin.Android gestartet ist unten dargestellt:

[![Beispielscreenshot, der eine Straße anzeigen](maps-application-images/03-streetview.png)](maps-application-images/03-streetview.png#lightbox)

Um eine Straße Ansicht zu starten, verwenden Sie einfach die `google.streetview` URI-Schema, wie im folgenden Code gezeigt:

```csharp
var streetViewUri = Android.Net.Uri.Parse (
       "google.streetview:cbll=42.374260,-71.120824&cbp=1,90,,0,1.0&mz=20");  
var streetViewIntent = new Intent (Intent.ActionView, streetViewUri);  
StartActivity (streetViewIntent);
```

Die oben verwendete google.streetview-URI-Schema weist folgende Form:

```csharp
google.streetview:cbll=lat,lng&cbp=1,yaw,,pitch,zoom&mz=mapZoom
```

Wie Sie sehen können, stehen mehrere Parameter unterstützt, wie im folgenden aufgeführt:

-   `lat` &ndash; Der Breitengrad des Standorts in der Straße Ansicht angezeigt werden.

-   `lng` &ndash; Der Längengrad des Standorts in der Straße Ansicht angezeigt werden.

-   `pitch` &ndash; Winkel der Straße anzeigen Panorama, gemessen in Grad, um 90 Grad wird direkt nach unten, und-90 Grad aus dem Center ist gerade nach oben.

-   `yaw` &ndash; Center-der-Ansicht der Straße anzeigen Panorama, gemessen im Uhrzeigersinn in Grad von Norden.

-   `zoom` &ndash; Zoom Multiplikator für die Straße anzeigen Panorama, wobei 1,0 = normal Zoom 2.0 vergrößerten 2 = x "," 3.0 = vergrößerten 4 x usw.

-   `mz` &ndash; Die Zoomstufe, die verwendet wird, wenn an die Maps-Anwendung aus der Straße an.


Arbeiten mit der integrierten ordnet die Anwendung oder die Straße Ansicht ist eine einfache Möglichkeit, schnell Unterstützung hinzuzufügen. Android-API von Maps bietet jedoch eine präzisere Kontrolle über die Benutzeroberfläche für die Zuordnung.
