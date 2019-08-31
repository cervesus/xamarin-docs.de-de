---
title: proaktive watchos-Vorschläge in xamarin
description: In diesem Artikel wird gezeigt, wie Sie proaktive Vorschläge in einer watchos 3-App verwenden, um Engagement zu fördern, indem Sie dem Benutzer das proaktive Anzeigen von hilfreichen Informationen für den Benutzer ermöglichen.
ms.prod: xamarin
ms.assetid: 10CC9F16-963C-44F1-8B98-F09FB2310DFF
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
ms.openlocfilehash: 95e0eb77719a9bcc642dfb1385d7371652943155
ms.sourcegitcommit: 1e3a0d853669dcc57d5dee0894d325d40c7d8009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2019
ms.locfileid: "70198802"
---
# <a name="watchos-proactive-suggestions-in-xamarin"></a>proaktive watchos-Vorschläge in xamarin

_In diesem Artikel wird gezeigt, wie Sie proaktive Vorschläge in einer watchos 3-App verwenden, um Engagement zu fördern, indem Sie dem Benutzer das proaktive Anzeigen von hilfreichen Informationen für den Benutzer ermöglichen._


Neu bei watchos 3: proaktive Vorschläge stellen News-Möglichkeiten zur Verfügung, damit Benutzer mit einer xamarin. IOS-app in Kontakt treten, indem Sie den Benutzern proaktiv hilfreiche Informationen zu den richtigen Zeiten präsentieren.


## <a name="about-proactive-suggestions"></a>Informationen zu proaktiven Vorschlägen

Neu bei watchos 3: `NSUserActivity` enthält eine `MapItem` Eigenschaft, die es der App ermöglicht, Standortinformationen bereitzustellen, die in anderen Kontexten verwendet werden können. Wenn die APP z. b. "Hotel Reviews" angezeigt `MapItem` wird und einen Speicherort bereitstellt, wäre der Speicherort des Hotels, das Sie gerade anzeigen, verfügbar, wenn der Benutzer zur Maps-APP gewechselt hat.

Die APP macht diese Funktionalität dem System mithilfe einer Sammlung von Technologien wie `NSUserActivity`, MapKit, Media Player und UIKit verfügbar. Durch die Bereitstellung einer proaktiven Vorschlags Unterstützung für die APP wird außerdem die Siri-Integration kostenlos unterstützt.

## <a name="location-based-suggestions"></a>Location based-Vorschläge

Neu bei watchos 3: die `NSUserActivity` -Klasse enthält `MapItem` eine-Eigenschaft, die es dem Entwickler ermöglicht, Standortinformationen bereitzustellen, die in anderen Kontexten verwendet werden können. Wenn die APP beispielsweise Restaurant Reviews anzeigt, kann der Entwickler die- `MapItem` Eigenschaft auf den Speicherort des Restaurants festlegen, das der Benutzer in der APP anzeigt. Wenn der Benutzer zur Maps-APP wechselt, ist der Speicherort des Restaurants automatisch verfügbar.

Wenn die APP die APP-Suche unterstützt, kann Sie die neuen Adress Komponenten `CSSearchableItemAttributesSet` der-Klasse verwenden, um Orte anzugeben, die der Benutzer möglicherweise besuchen möchte. Wenn Sie die `MapItem` -Eigenschaft festlegen, werden die anderen Eigenschaften automatisch ausgefüllt.

Zusätzlich zum Festlegen `Latitude` von und `Longitude` der Eigenschaften der Adress Komponente wird empfohlen, dass die APP auch die `NamedLocation` -Eigenschaft und `PhoneNumbers` die-Eigenschaft bereitstellt, damit Siri einen aufrufungsort initiieren kann.

## <a name="contextual-siri-reminders"></a>Kontextabhängige Siri-Erinnerungen

Ermöglicht dem Benutzer die Verwendung von Siri, um schnell eine Erinnerung zum Anzeigen der Inhalte zu erhalten, die Sie zu einem späteren Zeitpunkt in der App anzeigen. Wenn Sie z. b. eine Restaurant Überprüfung in der App anzeigen, könnte Sie Siri aufrufen und sagen, *Wenn ich zu Hause komme.* Siri generiert die Erinnerung mit einem Link zur Überprüfung in der app.

## <a name="implementing-proactive-suggestions"></a>Implementieren von proaktiven Vorschlägen

Die Unterstützung für proaktives vorschlagen der xamarin. IOS-APP ist in der Regel genauso einfach wie das Implementieren einiger APIs oder das Erweitern einiger APIs, die die APP möglicherweise bereits implementiert.

Proaktive Vorschläge können auf drei Arten mit den apps verwendet werden:

- **`NSUserActivity`** : Unterstützt das System dabei, zu verstehen, welche Informationen der Benutzer zurzeit auf dem Bildschirm arbeitet.
- **Vorschläge** für den Standort: Wenn die APP standortbasierte Informationen anbietet oder nutzt, bieten diese API-Erweiterungen neue Möglichkeiten, diese Informationen über apps hinweg gemeinsam zu nutzen.

Und werden in der App unterstützt, indem folgendes implementiert wird:

- **Kontextbezogene Siri-Erinnerungen** : in ios `NSUserActivity` 10 wurde erweitert, damit Siri rasch eine Erinnerung zum Anzeigen der Inhalte, die Sie zurzeit in der App anzeigen, zu einem späteren Zeitpunkt erhalten.
- **Location-Vorschläge** : IOS 10 `NSUserActivity` verbessert die Erfassung von Standorten, die in der App angezeigt werden, und stuft sie an vielen Stellen im gesamten System herauf.
- **Kontextabhängige Siri-Anforderungen**  -  `NSUserActivity` stellen Kontextinformationen bereit, die in der APP für Siri bereitgestellt werden, damit der Benutzeranweisungen abrufen oder aufrufen kann, um Siri innerhalb der APP aufzurufen.

Alle diese Features haben eine gemeinsame Funktion, die alle in einer `NSUserActivity` Form oder einem anderen zum Bereitstellen Ihrer Funktionalität verwendet werden. 

## <a name="nsuseractivity"></a>NSUserActivity

Wie bereits erwähnt, `NSUserActivity` unterstützt das System dabei, zu verstehen, welche Informationen der Benutzer zurzeit auf dem Bildschirm arbeitet. `NSUserActivity`bei handelt es sich um einen einfachen Mechanismus zum Zwischenspeichern von Zuständen, um die Aktivität des Benutzers zu erfassen, während Sie durch die APP navigieren. Sehen Sie sich z. b. die Restaurant-APP an:

[![](proactive-suggestions-images/activity02.png "Die Restaurant-App")](proactive-suggestions-images/activity02.png#lightbox)

Mit den folgenden Interaktionen:

1. Wenn der Benutzer mit der APP arbeitet, wird `NSUserActivity` eine erstellt, um den Status der APP später neu zu erstellen.
2. Wenn der Benutzer nach einem Restaurant sucht, wird das gleiche Muster zum Erstellen von Aktivitäten befolgt.
3. Und auch hier, wenn der Benutzer ein Ergebnis anzeigt. Im letzten Fall wird der Benutzer einen Speicherort anzeigen und in ios 10 bestimmte Konzepte (z. b. Standort-oder Kommunikations Interaktionen) besser kennen.

Sehen Sie sich den letzten Bildschirm genauer an:

[![](proactive-suggestions-images/activity03.png "Die nsuseractivity-Nutzlast")](proactive-suggestions-images/activity03.png#lightbox)

Hier erstellt die APP einen `NSUserActivity` und wurde mit Informationen aufgefüllt, um den Zustand später neu zu erstellen. Die app enthält außerdem einige Metadaten, z. b. den Namen und die Adresse des Speicher Orts. Wenn diese Aktivität erstellt wurde, informiert die APP IOS, dass Sie den aktuellen Zustand des Benutzers darstellt.

Die APP entscheidet dann, ob die Aktivität für die Übergabe per Funk angekündigt, als temporärer Wert für Location-Vorschläge gespeichert oder zum Spotlight-Index auf dem Gerät hinzugefügt wird, um Sie in den Suchergebnissen anzuzeigen.

Weitere Informationen zur Übergabe und Spotlight-Suche finden Sie [in unserer Einführung in Handoff](~/ios/platform/handoff.md) und IOS 9-Anleitungen für [neue Such-APIs](~/ios/platform/search/index.md) .

### <a name="creating-an-activity"></a>Erstellen einer Aktivität

Vor dem Erstellen einer Aktivität muss eine Aktivitätstyp Kennung erstellt werden, um Sie zu identifizieren. Der Aktivitätstyp Bezeichner ist eine kurze Zeichenfolge `NSUserActivityTypes` , die dem Array der `Info.plist` App-Datei hinzugefügt wird, die zur eindeutigen Identifizierung eines bestimmten Benutzer Aktivitäts Typs verwendet wird. Es gibt einen Eintrag im-Array für jede Aktivität, die von der App unterstützt wird und die für die APP-Suche verfügbar gemacht wird. Weitere Informationen finden Sie in der [Referenz zum Erstellen von Aktivitätstyp bezeichlern](~/ios/platform/search/nsuseractivity.md) .

Sehen Sie sich ein Beispiel für eine Aktivität an:

```csharp
// Create App Activity
var activity = new NSUserActivity ("com.xamarin.platform");

// Define details
var info = new NSMutableDictionary ();
info.Add(new NSString("link"),new NSString("http://xamarin.com/platform"));

// Populate Activity
activity.Title = "The Xamarin Platform";
activity.UserInfo = info;

// Enable capabilities
activity.EligibleForSearch = true;
activity.EligibleForHandoff = true;
activity.EligibleForPublicIndexing = true;

// Inform system of Activity
activity.BecomeCurrent();
```

Eine neue Aktivität wird mit einem Aktivitätstyp Bezeichner erstellt. Als nächstes werden einige Metadaten erstellt, die die Aktivität definieren, sodass dieser Zustand zu einem späteren Zeitpunkt wieder hergestellt werden kann. Anschließend erhält die Aktivität einen sinnvollen Titel und wird an die Benutzerinformationen angefügt. Schließlich sind einige Funktionen aktiviert, und die Aktivität wird an das System gesendet.

Der obige Code kann erweitert werden, um Metadaten zu enthalten, die den Kontext für die Aktivität bereitstellen, indem Sie die folgenden Änderungen vornehmen:

```csharp
...

// Provide context
var attributes = new CSSearchableItemAttributeSet ("com.xamarin.location");
attributes.ThumbnailUrl = myThumbnailURL;
attributes.Keywords = new string [] { "software", "mobile", "language" }; 
activity.ContentAttributeSet = attributes;

// Inform system of Activity
activity.BecomeCurrent();
```

Wenn der Entwickler über eine Website verfügt, die die gleichen Informationen wie die App anzeigen kann, kann die APP die URL enthalten, und der Inhalt kann auf anderen Geräten angezeigt werden, auf denen die APP nicht installiert ist (via Handoff):

```csharp
// Restore on the web
activity.WebPageUrl = new NSUrl("http://xamarin.com/platform");
```

### <a name="restoring-an-activity"></a>Wiederherstellen einer Aktivität

Bearbeiten Sie die Datei **AppDelegate.cs** , und überschreiben Sie `ContinueUserActivity` die`NSUserActivity`-Methode, um darauf zu reagieren, dass der Benutzer auf ein Suchergebnis () für die APP tippt. Beispiel:

```csharp
public override bool ContinueUserActivity (UIApplication application, NSUserActivity userActivity, UIApplicationRestorationHandler completionHandler)
{

    // Take action based on the activity type
    switch (userActivity.ActivityType) {
    case "com.xamarin.platform":
        // Restore the state of the app here...
        break;
    }

    return true;
}
```

Stellen Sie sicher, dass es sich um denselben`com.xamarin.platform`Aktivitätstyp Bezeichner () wie die oben erstellte Aktivität handelt. Die APP verwendet die in `NSUserActivity` gespeicherten Informationen, um den Zustand wiederherzustellen, an dem der Benutzer aufgehört hat.

### <a name="benefits-of-creating-an-activity"></a>Vorteile der Erstellung einer Aktivität

Mit der minimalen Menge an Code, der oben dargestellt wird, kann die App nun drei neue IOS 10-Funktionen nutzen:

- **Handoff**
- **Spotlight-Suche**
- **Kontextabhängige Siri-Erinnerungen**

Im folgenden Abschnitt wird beschrieben, wie Sie zwei weitere neue IOS 10-Features aktivieren:

- **Location-Vorschläge**
- **Kontextabhängige Siri-Anforderungen**

### <a name="location-based-suggestions"></a>Location based-Vorschläge 

Sehen Sie sich das Beispiel für die app "Restaurant Suche" an. Wenn alle Metadaten und `NSUserActivity` Attribute implementiert und korrekt ausgefüllt wurden, kann der Benutzer folgende Aktionen ausführen:

1. Suchen Sie in der APP nach einem Restaurant, das Sie einem Friend begegnen möchten.
2. Wenn der Benutzer zur Maps-APP wechselt, wird die Adresse des Restaurants automatisch als Ziel vorgeschlagen.
3. Dies funktioniert auch für Drittanbieter-Apps (die unter `NSUserActivity`Stützung für), sodass der Benutzer zu einer Fahrt Freigabe-App wechseln kann, und die Adresse des Restaurants wird ebenfalls automatisch als Ziel vorgeschlagen.
4. Außerdem wird der Kontext für Siri bereitgestellt, sodass der Benutzer Siri innerhalb der Restaurant-App aufrufen und *"get directions..." (DIRECTIONS...* ) abrufen kann.

Alle oben genannten Funktionen haben eine gemeinsame Sache, Sie geben alle an, woher der Vorschlag ursprünglich stammt. Im Beispiel oben ist dies die fiktive Restaurant Review-app.

watchos 3 wurde verbessert, um diese Funktionalität für eine APP durch mehrere kleine Änderungen und Ergänzungen vorhandener Frameworks zu ermöglichen:

- `NSUserActivity`verfügt über zusätzliche Felder zum Erfassen von Standortinformationen, die in der App angezeigt werden.
- An MapKit und corespotlight wurden mehrere Ergänzungen vorgenommen, um den Speicherort zu erfassen.
- Die Funktion zum Erkennen von Standorten wurde Siri, Maps, Multitasking und anderen apps im System hinzugefügt.

Um Location based-Vorschläge zu implementieren, beginnen Sie mit dem oben dargestellten Aktivitäts Code:

```csharp
// Create App Activity
var activity = new NSUserActivity ("com.xamarin.platform");

// Define details
var info = new NSMutableDictionary ();
info.Add(new NSString("link"),new NSString("http://xamarin.com/platform"));

// Populate Activity
activity.Title = "The Xamarin Platform";
activity.UserInfo = info;

// Enable capabilities
activity.EligibleForSearch = true;
activity.EligibleForHandoff = true;
activity.EligibleForPublicIndexing = true;

// Provide context
var attributes = new CSSearchableItemAttributeSet ("com.xamarin.location");
attributes.ThumbnailUrl = myThumbnailURL;
attributes.Keywords = new string [] { "software", "mobile", "language" }; 
activity.ContentAttributeSet = attributes;

// Restore on the web
activity.WebPageUrl = new NSUrl("http://xamarin.com/platform");

// Inform system of Activity
activity.BecomeCurrent();
```

Wenn die APP MapKit verwendet, ist es so einfach wie das Hinzufügen der aktuellen `MKMapItem` Karte zur Aktivität:

```csharp
// Save MKMapItem location
activity.MapItem = myMapItem;
```

Wenn die APP MapKit nicht verwendet, kann Sie die APP-Suche übernehmen und die folgenden neuen Attribute für Location angeben:

```csharp
// Provide context
var attributes = new CSSearchableItemAttributeSet ("com.xamarin.location");
...

attributes.NamedLocation = "Apple Inc.";
attributes.SubThoroughfare = "1";
attributes.Thoroughfare = "Infinite Loop";
attributes.City = "Cupertino";
attributes.StateOrProvince = "CA";
attributes.Country = "United States";
attributes.Latitude = 37.33072;
attributes.Longitude = 122.029674;
attributes.PhoneNumbers = new string[]{"(800) 275-2273"};
attributes.SupportsPhoneCalls = true;
attributes.SupportsNavigation = true;
```

Sehen Sie sich den obigen Code ausführlich an. Zuerst ist der Name des Speicher Orts in jeder Instanz erforderlich:

```csharp
attributes.NamedLocation = "Apple Inc.";
```

Anschließend wird die textbasierte Beschreibung in für textbasierte Instanzen (z. b. die quicktype-Tastatur) benötigt:

```csharp
attributes.SubThoroughfare = "1";
attributes.Thoroughfare = "Infinite Loop";
attributes.City = "Cupertino";
attributes.StateOrProvince = "CA";
attributes.Country = "United States";
```

Der breiten-und Längengrad ist optional, aber stellen Sie sicher, dass der Benutzer an den genauen Speicherort weitergeleitet wird, an den die APP Sie senden möchte:

```csharp
attributes.Latitude = 37.33072;
attributes.Longitude = 122.029674;
```

Durch Festlegen der Telefonnummern kann die APP auf Siri zugreifen, sodass der Benutzer Siri von der APP aus aufrufen kann, indem er etwas wie "Aufrufen dieses Orts" sagt:

```csharp
attributes.PhoneNumbers = new string[]{"(800) 275-2273"};
```

Zum Schluss kann die APP angeben, ob die Instanz für Navigations-und Telefonanrufe geeignet ist:

```csharp
attributes.SupportsPhoneCalls = true;
attributes.SupportsNavigation = true;
```

## <a name="activities-best-practices"></a>Bewährte Methoden für Aktivitäten

Apple schlägt beim Arbeiten mit Aktivitäten die folgenden bewährten Methoden vor:

- Verwenden `NeedsSave` Sie für Lazy Payload-Updates.
- Stellen Sie sicher, dass Sie einen starken Verweis auf die aktuelle Aktivität erhalten.
- Übertragen Sie nur kleine Nutzlasten, die nur genügend Informationen enthalten, um den Status wiederherzustellen.
- Stellen Sie sicher, dass die Aktivitätstyp Bezeichner eindeutig und beschreibend sind, indem Sie die Reverse-DNS-Notation zum angeben verwenden. 

## <a name="consuming-location-suggestions"></a>Nutzen von Location-Vorschlägen

Im nächsten Abschnitt werden die Vorschläge für den nutzenden Standort behandelt, die aus anderen Teilen des Systems (z. b. der Maps-APP) oder anderen Drittanbieter-apps stammen.

## <a name="routing-apps-and-locations-suggestions"></a>Vorschläge für Routing-apps und-Standorte

In diesem Abschnitt wird erläutert, wie Sie Orts Vorschläge direkt aus einer Routing-APP heraus nutzen. Damit die Routing-App Diese Funktionalität hinzufügen kann, nutzt der Entwickler das `MKDirectionsRequest` vorhandene Framework wie folgt:

- Herauf Stufen der APP im Multitasking
- Zum Registrieren der App als Routing-app.
- Um das Starten der APP mit einem MapKit `MKDirectionsRequest` -Objekt zu behandeln.
- Stellen Sie watchos die Möglichkeit zur Verfügung, die App basierend auf der Benutzer Einbindung vorzuschlagen.

Wenn die APP mit einem MapKit `MKDirectionsRequest` -Objekt gestartet wird, sollte Sie automatisch damit beginnen, den Benutzer zum angeforderten Speicherort zu bringen, oder eine Benutzeroberfläche bereitstellen, die es dem Benutzer ermöglicht, mit der Einführung von Anweisungen zu beginnen. Beispiel:


```csharp
using System;
using Foundation;
using UIKit;
using MapKit;
using CoreLocation;

namespace MonkeyChat
{
    [Register ("AppDelegate")]
    public class AppDelegate : UIApplicationDelegate, IUISplitViewControllerDelegate
    {
        ...

        public override bool OpenUrl (UIApplication app, NSUrl url, NSDictionary options)
        {
            if (MKDirectionsRequest.IsDirectionsRequestUrl (url)) {
                var request = new MKDirectionsRequest (url);
                var coordinate = request.Destination?.Placemark.Location?.Coordinate;
                var address = request.Destination.Placemark.AddressDictionary;
                if (coordinate.IsValid()) {
                    var geocoder = new CLGeocoder ();
                    geocoder.GeocodeAddress (address, (place, err) => {
                        // Handle the display of the address

                    });
                }
            }

            return true;
        }
    }
}
```

Sehen Sie sich diesen Code ausführlich an. Es testet, ob es sich um eine gültige Ziel Anforderung handelt:

```csharp
if (MKDirectionsRequest.IsDirectionsRequestUrl(url)) {
```

Wenn dies der Fall ist, wird aus `MKDirectionsRequest` der URL ein erstellt:

```csharp
var request = new MKDirectionsRequest(url);
```

Neu in watchos 3: der APP kann eine Adresse gesendet werden, die keine geografischen Koordinaten hat. Dies bewirkt, dass der Entwickler die Adresse codieren muss:

```csharp
var geocoder = new CLGeocoder();
geocoder.GeocodeAddress(address, (place, err)=> {
    // Handle the display of the address

});

```

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden proaktive Vorschläge behandelt, und es wurde gezeigt, wie der Entwickler diese zum Steuern von Datenverkehr an eine xamarin. IOS-App für watchos verwenden kann. Es wurden die Schritte zum Implementieren von proaktiven Vorschlägen und zur Verwendung von Verwendungs Richtlinien behandelt.


## <a name="related-links"></a>Verwandte Links

- [watchOS-Beispiele](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+watchOS)
- [Sirikit-Programmier Handbuch](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/index.html)
