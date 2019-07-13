---
title: WatchOS proaktive Vorschläge in Xamarin
description: In diesem Artikel veranschaulicht die proaktive Vorschläge in einer WatchOS 3-app zum fördern der Interaktion zu verwenden, indem Sie dem System proaktiv hilfreiche Informationen automatisch an den Benutzer vorhanden.
ms.prod: xamarin
ms.assetid: 10CC9F16-963C-44F1-8B98-F09FB2310DFF
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
ms.openlocfilehash: 0c0bf6058b2ec7a8e3ef606bef9f725a476abffe
ms.sourcegitcommit: 7ccc7a9223cd1d3c42cd03ddfc28050a8ea776c2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/13/2019
ms.locfileid: "67865920"
---
# <a name="watchos-proactive-suggestions-in-xamarin"></a>WatchOS proaktive Vorschläge in Xamarin

_In diesem Artikel veranschaulicht die proaktive Vorschläge in einer WatchOS 3-app zum fördern der Interaktion zu verwenden, indem Sie dem System proaktiv hilfreiche Informationen automatisch an den Benutzer vorhanden._


Die neue watchos 3, proaktive Vorschläge vorhanden News Möglichkeiten für Benutzer von proaktiv vorhanden hilfreiche Informationen automatisch an den Benutzer zur richtigen Zeit jeweils mit einer Xamarin.iOS-app in Verbindung setzen.


## <a name="about-proactive-suggestions"></a>Informationen zu proaktive Vorschläge

Neu in WatchOS 3, `NSUserActivity` umfasst eine `MapItem` -Eigenschaft, die ermöglicht der app, Informationen zum Speicherort bereitstellen, die in anderen Kontexten verwendet werden kann. Wenn die Hotel-app angezeigt werden und bietet beispielsweise eine `MapItem` Speicherort, wenn der Benutzer an die Maps-app übergeben hat, der Speicherort des Hotels, die sie gerade angezeigt haben, verfügbar sein würde.

Die app stellt diese Funktionen an das System eine Sammlung von Technologien wie z. B. über `NSUserActivity`, MapKit, Media Player und UIKit. Darüber hinaus durch die proaktive Vorschläge Unterstützung für die app, ruft sie ab engere Integration von Siri kostenlos.

## <a name="location-based-suggestions"></a>Standortbasierte Vorschläge

Neu in WatchOS 3, die `NSUserActivity` Klasse enthält eine `MapItem` -Eigenschaft, die ermöglicht es dem Entwickler, um Informationen bereitzustellen, die in anderen Kontexten verwendet werden kann. Z. B. wenn die app Restaurantkritiken angezeigt wird, können Sie festlegen der `MapItem` Eigenschaft, um den Speicherort des Restaurants, die dem Benutzer in der app angezeigt wird. Wenn der Benutzer an die Maps-app wechselt, ist das Restaurant Speicherort automatisch verfügbar.

Wenn die app die App-Suche unterstützt, können sie die neue Adresskomponenten der `CSSearchableItemAttributesSet` Klasse Speicherorte an, die der Benutzer möglicherweise besuchen möchte. Durch Festlegen der `MapItem` -Eigenschaft, die anderen Eigenschaften sind, automatisch ausgefüllt.

Zusätzlich zum Festlegen der `Latitude` und `Longitude` der Eigenschaften der Komponente, wird empfohlen, geben Sie die app die `NamedLocation` und `PhoneNumbers` Eigenschaften, also Siri einen Aufruf an den Speicherort initiieren kann.

## <a name="contextual-siri-reminders"></a>Kontextbezogene Siri Erinnerungen

Ermöglicht dem Benutzer um Siri verwenden, um schnell eine Erinnerung, um den Inhalt anzuzeigen, dass sie zu einem späteren Zeitpunkt gerade in der app angezeigt werden. Z. B. Wenn sie eine Restaurantkritik in der app angezeigt haben, sie könnten aufrufen Siri und sagen *"Erinnern dazu beim Start mir."* Siri würde die Erinnerung mit einem Link zur Überprüfung in der app generieren.

## <a name="implementing-proactive-suggestions"></a>Proaktive Vorschläge

Proaktive Vorschlag hinzufügen, ist Unterstützung für die Xamarin.iOS-app in der Regel ganz einfach einige APIs implementieren oder Erweitern auf einige wenige APIs, die die app bereits implementiert werden kann.

Proaktive Vorschläge arbeiten mit den apps auf drei Arten:

- **`NSUserActivity`** – Kann das System aus, welche Informationen, die der Benutzer derzeit mit Bildschirm arbeitet.
- **Speicherort-Vorschläge** – Wenn die app bietet oder diese neuen Möglichkeiten für API-Erweiterung Angebot positionsbasierten Informationen nutzt diese Informationen über apps hinweg zu teilen.

Und wird in der app unterstützt, durch die folgende Implementierung:

- **Kontextbezogene Siri Erinnerungen** – In iOS 10 `NSUserActivity` wurde erweitert, um Siri schnell stellen eine Erinnerung, die den Inhalt anzuzeigen, die aktuell angezeigt werden in der app zu einem späteren Zeitpunkt zu ermöglichen.
- **Speicherort-Vorschläge** -iOS 10 erhöht `NSUserActivity` zum Erfassen von Standorten, die innerhalb der app angezeigt, und Stufen sie an vielen Stellen im gesamten System.
- **Kontextbezogene Siri-Anforderungen**  -  `NSUserActivity` stellt Kontext für die Informationen in der app siri, damit der Benutzer Anweisungen oder ein Aufruf direkt aufrufen Siri innerhalb der app abrufen kann.

Alle diese Funktionen haben eine Gemeinsamkeit, dass alle verwenden `NSUserActivity` in einem Formular oder eine andere, um die jeweiligen Funktionen zur Verfügung. 

## <a name="nsuseractivity"></a>NSUserActivity

Wie bereits erwähnt, `NSUserActivity` kann das System aus, welche Informationen, die der Benutzer zurzeit mit auf dem Bildschirm arbeitet. `NSUserActivity` ist ein Lightweight-Zwischenspeicherung des Mechanismus für die Aktivität des Benutzers zu erfassen, wie sie durch die app navigieren. Betrachten z. B. die Restaurant-app ein:

[![](proactive-suggestions-images/activity02.png "Die Restaurant-app")](proactive-suggestions-images/activity02.png#lightbox)

Mit dem folgenden Interaktionen:

1. Da der Benutzer mit der app arbeitet ein `NSUserActivity` wird erstellt, um den Zustand der app später neu erstellen.
2. Wenn der Benutzer für ein Restaurant sucht, folgt das gleiche Muster zum Erstellen von Aktivitäten.
3. Und Sie erneut, wenn der Benutzer zeigt ein Ergebnis. Im letzten Fall einem Ort anzeigen und in iOS 10, das System bessere Unterstützung für bestimmte Konzepte (z. B. Speicherort oder die Kommunikation Interaktionen).

Führen Sie einen genaueren Blick auf dem letzten Bildschirm:

[![](proactive-suggestions-images/activity03.png "Die Nutzlast NSUserActivity")](proactive-suggestions-images/activity03.png#lightbox)

Hier die app ist das Erstellen einer `NSUserActivity` und es aufgefüllt wurde mit Informationen, um den Status später neu zu erstellen. Die app hat auch einige Metadaten wie Name und Adresse des Speicherorts enthalten. Mit dieser Aktivität erstellt wird informiert die app für iOS, wissen, dass sie den aktuellen Status des Benutzers darstellt.

Die app entscheidet dann, wenn die Aktivität für die Übergabe drahtlose angekündigt, als ein temporärer Wert für Speicherort Vorschläge gespeichert oder dem auf dem Gerät Spotlight-Index für die Anzeige in den Suchergebnissen hinzugefügt.

Weitere Informationen zu Übergabeanimationen und Spotlight-Suche, finden Sie unter unserem [Einführung in die Übergabe](~/ios/platform/handoff.md) und [iOS 9-Suche-APIs neue](~/ios/platform/search/index.md) Anleitungen.

### <a name="creating-an-activity"></a>Erstellen einer Aktivität

Vor dem Erstellen einer Aktivitäts an, ist ein Aktivitätsbezeichner für den Typ erforderlich um erstellt werden, um es zu identifizieren. Typ des Bezeichners der Aktivität ist eine kurze Zeichenfolge hinzugefügt, die `NSUserActivityTypes` Array von der app `Info.plist` Datei, die zur eindeutigen Identifizierung eines bestimmten Typs des Benutzer-Aktivität verwendet. Es werden ein Eintrag im Array für jede Aktivität, die die app unterstützt, und App-Suche verfügbar macht. Finden Sie unter unserem [erstellen Aktivität-IDs Typverweis](~/ios/platform/search/nsuseractivity.md) Weitere Details.

Sehen Sie sich ein Beispiel für eine Aktivität:

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

Eine neue Aktivität wird erstellt unter Verwendung eines Bezeichners der Aktivität geben. Als Nächstes wird einige Metadaten zur Definition der Aktivitäts erstellt, sodass diesen Zustand zu einem späteren Zeitpunkt wiederhergestellt werden kann. Anschließend wird die Aktivität erhalten einen aussagekräftigen Titel und die Benutzerinformationen angefügt. Schließlich sind einige Funktionen aktiviert, und die Aktivität an das System gesendet wird.

Der obige Code kann weiter verbessert werden, um die Metadaten enthalten, die Kontext für die Aktivität bietet die folgenden Änderungen vornehmen:

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

Verfügt der Entwickler auf eine Website, die die gleiche Informationen wie die app angezeigt werden, wird die app schließen Sie die URL, und der Inhalt angezeigt werden kann, auf anderen Geräten, die die app (über Übergabe) nicht installiert haben:

```csharp
// Restore on the web
activity.WebPageUrl = new NSUrl("http://xamarin.com/platform");
```

### <a name="restoring-an-activity"></a>Wiederherstellen einer Aktivitäts

Für den Benutzer Tippen auf ein Suchergebnis reagieren (`NSUserActivity`) für die app Bearbeiten der **Datei "appdelegate.cs"** Datei, und überschreiben die `ContinueUserActivity` Methode. Beispiel:

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

Sicher, dass dem gleichen Aktivitätsbezeichner-Typ ist (`com.xamarin.platform`) als die oben erstellte Aktivität. Die app verwendet den Informationen in den `NSUserActivity` zum Wiederherstellen des Zustands an, wo der Benutzer unterbrochen.

### <a name="benefits-of-creating-an-activity"></a>Vorteile der Erstellung einer Aktivitäts

Mit minimalem Code oben angeführte kann die app nun drei neue iOS 10-Features nutzen:

- **Handoff**
- **Spotlight-Suche**
- **Kontextbezogene Siri Erinnerungen**

Im folgende Abschnitt wird einen Blick auf zwei weitere neue iOS 10-Funktionen aktivieren:

- **Speicherort-Vorschläge**
- **Kontextbezogene Siri-Anforderungen**

### <a name="location-based-suggestions"></a>Standortbasierte Vorschläge 

Betrachten Sie beispielsweise das Restaurant-Suchanwendung, die oben genannten. Wenn sie implementiert hat `NSUserActivity` und korrekt alle Metadaten und Attribute, die Benutzer können die folgenden Schritte ausführen:

1. Finden Sie ein Restaurant, in der app, der sie einen Freund am zu erfüllen möchten.
2. Wenn der Benutzer an die Maps-app wechselt, wird das Restaurant die Adresse als Ziel automatisch vorgeschlagen.
3. Dies funktioniert sogar für 3. apps von Drittanbietern (, unterstützen `NSUserActivity`), sodass der Benutzer für eine Fahrt-Freigabe-app wechseln kann und das Restaurant die Adresse wird automatisch auch als Ziel es empfohlen.
4. Es stellt auch Kontext siri, kann der Benutzer Aufrufen von Siri innerhalb der app Restaurant und bitten Sie *"Abrufen einer Wegbeschreibung..."* und Siri bietet Anweisungen für das Restaurant anzeigen.

Alle oben genannten Funktionen gemeinsam ist eine Sache, alle anzugeben, in dem der Vorschlag ursprünglich stammt. Im Fall der obigen Beispiel ist es die fiktiven Restaurant-app überprüfen.

WatchOS 3 wurde verbessert, um diese Funktion für eine app über mehrere kleine Änderungen und Ergänzungen zu den vorhandenen Frameworks zu aktivieren:

- `NSUserActivity` enthält zusätzliche Felder für die Erfassung von Speicherortinformationen, die innerhalb der app angezeigt wird.
- Mehrere Erweiterungen MapKit und CoreSpotlight wurden Ort zu erfassen.
- Siri, Zuordnungen, Multitasking und andere apps innerhalb des Systems wurde bewusst speicherortfunktion hinzugefügt.

Um positionsbasierten Vorschläge zu implementieren, beginnen Sie mit der gleichen Aktivität angezeigt, über:

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

Wenn die app MapKit verwendet wird, ist es so einfach wie das Hinzufügen der aktuellen Zuordnung `MKMapItem` mit der Aktivität:

```csharp
// Save MKMapItem location
activity.MapItem = myMapItem;
```

Wenn die app MapKit verwendet, kann App-Suche zu übernehmen und angeben, die folgenden neuen Attribute für Ort:

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

Sehen Sie sich den obigen Code im Detail an. Zunächst ist der Name des Speicherorts in jedem Fall erforderlich:

```csharp
attributes.NamedLocation = "Apple Inc.";
```

-Basierte klicken Sie dann der Text basierend Beschreibung im Text erforderlich Instanzen (z. B. die Tastatur QuickType):

```csharp
attributes.SubThoroughfare = "1";
attributes.Thoroughfare = "Infinite Loop";
attributes.City = "Cupertino";
attributes.StateOrProvince = "CA";
attributes.Country = "United States";
```

Den Breiten- und Längengrad sind optional, aber stellen Sie sicher, dass der Benutzer an die richtige Stelle weitergeleitet wird, die app senden möchten, ist:

```csharp
attributes.Latitude = 37.33072;
attributes.Longitude = 122.029674;
```

Durch Festlegen von Telefonnummern, kann die app Zugriff auf Siri erhalten, damit der Benutzer Siri aus der app aufrufen kann, indem sagen etwas wie * "Call folgendem":

```csharp
attributes.PhoneNumbers = new string[]{"(800) 275-2273"};
```

Schließlich kann die app darauf hinweisen, ob die Instanz für die Navigation und Telefonanrufe geeignet ist:

```csharp
attributes.SupportsPhoneCalls = true;
attributes.SupportsNavigation = true;
```
## <a name="activities-best-practices"></a>Bewährte Methoden von Aktivitäten

Apple empfiehlt die folgenden bewährten Methoden bei der Arbeit mit Aktivitäten:

- Verwendung `NeedsSave` für verzögerte Nutzlast-Updates.
- Achten Sie darauf, um einen starken Verweis auf die aktuelle Aktivität zu halten.
- Übertragen Sie nur kleine Nutzlasten, die gerade genug Informationen zum Wiederherstellen des Zustands enthalten.
- Sicherstellen Sie, dass der Typbezeichner der Aktivität eindeutigen und aussagekräftigen mit Reverse-DNS-Notation angeben. 

## <a name="consuming-location-suggestions"></a>Nutzen die Speicherort-Vorschläge

In diesem nächsten Abschnitt behandelt, nutzen die Speicherort-Vorschlag, die aus anderen Teilen des Systems (z. B. der Karten-app) oder andere 3. apps von Drittanbietern stammen.

## <a name="routing-apps-and-locations-suggestions"></a>Routing-Apps und Speicherorte Vorschläge

In diesem Abschnitt dauert einen Blick auf die Speicherort-Vorschläge aus dem direkt innerhalb einer routing-app nutzen. Für die routing-app diese Funktionalität hinzufügen, wird der Entwickler nutzen, die vorhandene `MKDirectionsRequest` Framework wie folgt:

- Um die app im Multitasking höher zu stufen.
- Die app als eine routing-app zu registrieren.
- Behandeln, beim Starten der app mit einem MapKit `MKDirectionsRequest` Objekt.
- Geben Sie die Möglichkeit, erfahren Sie, wie die app basierend auf Benutzerinteraktion vorschlagen WatchOS.

Wenn die app gestartet wird, mit einem MapKit `MKDirectionsRequest` -Objekt, sollten sie automatisch gestartet, sodass die Benutzer-Anweisungen auf den angeforderten Pfad oder stellen eine Benutzeroberfläche, die für den Benutzer zu Anfordern einer Wegbeschreibung vereinfacht. Beispiel:


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

Sehen Sie sich diesen Code im Detail an. Es überprüft, um festzustellen, ob es sich um eine gültige Ziel-Anforderung ist:

```csharp
if (MKDirectionsRequest.IsDirectionsRequestUrl(url)) {
```

Wenn es erstellt. anschließend eine `MKDirectionsRequest` aus der URL:

```csharp
var request = new MKDirectionsRequest(url);
```

Neue kann in WatchOS 3, die app eine Adresse gesendet werden, die nicht-geografische Koordinaten im, aufgrund derer der Entwickler die Adresse zu codieren muss:

```csharp
var geocoder = new CLGeocoder();
geocoder.GeocodeAddress(address, (place, err)=> {
    // Handle the display of the address
    
});

```

## <a name="summary"></a>Zusammenfassung

In diesem Artikel verfügt über proaktive Vorschläge beschrieben und gezeigt, wie die Entwickler sie Datenverkehr an eine Xamarin.iOS-app für WatchOS verwenden kann. Es erläutert die Schritte, um proaktive Vorschläge zu implementieren und Nutzungsrichtlinien angezeigt.


## <a name="related-links"></a>Verwandte Links

- [watchOS-Beispiele](https://developer.xamarin.com/samples/watchos/all/)
- [SiriKit-Programmierhandbuch](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/index.html)
