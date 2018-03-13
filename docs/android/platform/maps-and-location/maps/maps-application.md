---
title: Maps-Anwendung
ms.topic: article
ms.prod: xamarin
ms.assetid: 929EACB8-8950-50E1-093C-43FB5F1F1CD5
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/05/2017
ms.openlocfilehash: 4bcbd14b88f19dc48dc9d0694fb30aed31708153
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="maps-application"></a>Maps-Anwendung

Die einfachste Methode zum Arbeiten mit Karten in Xamarin.Android ist die unten gezeigte integrierten Karten-Anwendung zu nutzen:

[![Beispiel-Screenshot der integrierten app von Google Maps](maps-application-images/01-mapsapplication.png)](maps-application-images/01-mapsapplication.png#lightbox)

Wenn Sie die Maps-Anwendung verwenden, werden die Zuordnung nicht Teil Ihrer Anwendung. Stattdessen wird die Anwendung starten Sie die Maps-Anwendung und laden die Zuordnung extern. Im nächste Abschnitt untersucht, wie mit Xamarin.Android verwenden, um Zuordnungen wie oben zu starten.


## <a name="creating-the-intent"></a>Die Absicht erstellen

Arbeiten mit den Karten ist die Anwendung so einfach wie die Priorität mit einem entsprechenden URI erstellen, das die Aktion zum ActionView festlegen und das Aufrufen der StartActivity-Methode. Beispielsweise startet der folgende Code die Maps-Anwendung, die an einen bestimmten Breiten- und Längengrad zentriert:

```csharp
var geoUri = Android.Net.Uri.Parse ("geo:42.374260,-71.120824");
var mapIntent = new Intent (Intent.ActionView, geoUri);
StartActivity (mapIntent);
```

Dieser Code ist erforderlich, starten Sie die Zuordnung, die in der vorherigen Bildschirmaufnahme dargestellt. Zusätzlich zum Angeben von Breiten- und Längengrad, unterstützt das URI-Schema für Maps einige andere Optionen.


## <a name="geo-uri-scheme"></a>Geo-URI-Schema

Der obige Code verwendet die Geo-Schema, um einen URI zu erstellen. Dieser URI-Schema unterstützt mehrere Formate, wie im folgenden aufgeführt:

-   `geo:latitude,longitude` &ndash; Öffnet die Maps-Anwendung auf Lon Lat/zentriert. 

-   `geo:latitude,longitude?z=zoom` &ndash; Öffnet die Zuordnungen, die Anwendung auf Lon Lat/zentriert und gezoomt, auf die angegebene Stufe. Die Zoomstufe kann reichen von 1 bis 23: 1 zeigt die gesamte Erde und 23, wird die nächstgelegenen Zoomstufe.

-   `geo:0,0?q=my+street+address` &ndash; Öffnet die Maps-Anwendung auf den Speicherort der eine Anschrift an. 

-   `geo:0,0?q=business+near+city` &ndash; Die Maps-Anwendung geöffnet und zeigt die Suchergebnisse mit Anmerkungen. 


Die Versionen des URIS, die eine Abfrage (d. h. die Straße suchen oder die Adresse-Begriffe) annehmen verwenden Google Geocoder Dienst um den Speicherort abzurufen, der Sie dann auf der Karte angezeigt wird. Z. B. der URI `geo:0,0?q=coop+Cambridge` führt zu den unten gezeigten Zuordnung:

[![Beispiel für ein Screenshot, Google Maps mit einen Suchbegriff anzeigt](maps-application-images/02-mapsearch.png)](maps-application-images/02-mapsearch.png#lightbox)



Weitere Informationen über Geo-URI-Schemas finden Sie unter [einen Speicherort auf einer Karte anzeigen](http://developer.android.com/guide/components/intents-common.html#Maps).


## <a name="street-view"></a>Straße anzeigen

Zusätzlich zu den Geo-Schema unterstützt Android auch das Laden von Straße Ansichten von Priorität. Ein Beispiel für die Straße Ansicht-Anwendung, die von Xamarin.Android gestartet ist unten dargestellt:

[![Beispiel-Screenshot einer Straße Sicht](maps-application-images/03-streetview.png)](maps-application-images/03-streetview.png#lightbox)

Um eine Straße Ansicht zu starten, verwenden Sie einfach die `google.streetview` URI-Schema, wie im folgenden Code gezeigt:

```csharp
var streetViewUri = Android.Net.Uri.Parse (
       "google.streetview:cbll=42.374260,-71.120824&cbp=1,90,,0,1.0&mz=20");  
var streetViewIntent = new Intent (Intent.ActionView, streetViewUri);  
StartActivity (streetViewIntent);
```

Das oben verwendete google.streetview-URI-Schema weist folgende Form auf:

```csharp
google.streetview:cbll=lat,lng&cbp=1,yaw,,pitch,zoom&mz=mapZoom
```

Wie Sie sehen können, stehen mehrere Parameter unterstützt, wie im folgenden aufgeführt:

-   `lat` &ndash; Der Breitengrad des Speicherorts, in der Straße Ansicht angezeigt werden soll.

-   `lng` &ndash; Der Längengrad des Standorts in der Straße Ansicht angezeigt werden.

-   `pitch` &ndash; Drehwinkel Straße Ansicht Panorama, gemessen in Grad um 90 Grad ist, in dem gerade nach unten und-90 Grad aus dem Center werden gerade aktiv ist.

-   `yaw` &ndash; Center-des-Ansicht des Panorama Straße anzeigen, werden in Grad von North im Uhrzeigersinn gemessener.

-   `zoom` &ndash; Vergrößern/Verkleinern Multiplikator für Straße Ansicht Panorama, wobei 1,0 = normal Zoom 2.0 = vergrößerte 2 x 3.0 vergrößerte 4 = x usw.

-   `mz` &ndash; Die Zoomstufe, die verwendet wird, wenn Sie an die Maps-Anwendung aus der Straße Ansicht wechseln.


Arbeiten mit den integrierten ordnet die Anwendung oder die Straße Ansicht ist eine einfache Möglichkeit zum schnellen Hinzufügen von Unterstützung. Android Maps-API bietet jedoch eine genauere Steuerung der Zuordnung.
