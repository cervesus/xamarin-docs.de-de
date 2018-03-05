---
title: "Proaktive Vorschläge"
description: "In diesem Artikel wird gezeigt, wie proaktive Vorschläge in einer WatchOS 3-app auf Laufwerk Engagement verwendet werden, durch den Wechsel des Systems proaktiv hilfreiche Informationen automatisch für den Benutzer anzuzeigen."
ms.topic: article
ms.prod: xamarin
ms.assetid: 4E1FF652-28F0-4566-B383-9D12664401A4
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: ca2476eef120c7d86b939934ec4b286e871d6a78
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="proactive-suggestions"></a>Proaktive Vorschläge

_In diesem Artikel wird gezeigt, wie proaktive Vorschläge in einer WatchOS 3-app auf Laufwerk Engagement verwendet werden, durch den Wechsel des Systems proaktiv hilfreiche Informationen automatisch für den Benutzer anzuzeigen._


Noch nicht mit WatchOS 3, proaktive Vorschläge vorhanden News Möglichkeiten für Benutzer mit Xamarin.iOS app durch proaktiv vorhanden hilfreiche Informationen zur richtigen Zeit jeweils automatisch an den Benutzer in Verbindung setzen.


## <a name="about-proactive-suggestions"></a>Zum proaktiven Vorschläge

Neu bei WatchOS 3, `NSUserActivity` enthält eine `MapItem` Clustereigenschaft, mit der app, Informationen zum Speicherort bereitstellen, die in anderen Kontexten verwendet werden kann. Beispielsweise, wenn der app angezeigten Hotel überprüft und bietet eine `MapItem` Speicherort, wenn der Benutzer an die Maps-app übergeben hat, der Speicherort des gerade angezeigten waren Hotels steht zur Verfügung.

Die app verfügbar macht, diese Funktion in das System für eine Sammlung von Technologien wie z. B. über `NSUserActivity`, MapKit, Media Player und UIKit. Darüber hinaus wird durch proaktive Vorschlag-Unterstützung für die app, eine umfassendere Integration der Siri kostenlos.

## <a name="location-based-suggestions"></a>Standortbasierte Vorschläge

Neu bei WatchOS 3, die `NSUserActivity` Klasse enthält eine `MapItem` Clustereigenschaft, mit dem Entwickler, die Informationen zum Speicherort bereitstellen, die in anderen Kontexten verwendet werden kann. Beispielsweise, wenn die app Restaurantkritiken angezeigt wird, können Sie festlegen der `MapItem` Eigenschaft, um den Speicherort des Restaurants, die der Benutzer in der app angezeigt wird. Wenn der Benutzer der Maps-App gewechselt wird, ist das Restaurant Speicherort automatisch verfügbar.

Wenn die app App-Suche unterstützt, können sie die neue Adresse Bestandteile der `CSSearchableItemAttributesSet` Klasse, um Speicherorte anzugeben, die die Benutzer besuchen können. Durch Festlegen der `MapItem` -Eigenschaft, die anderen Eigenschaften sind, automatisch ausgefüllt.

Zusätzlich zu den `Latitude` und `Longitude` von die Adresseigenschaften für die Komponente, wird empfohlen, dass die app Bereitstellen der `NamedLocation` und `PhoneNumbers` Eigenschaften zu, sodass Siri einen Aufruf an den Speicherort initiieren kann.

## <a name="contextual-siri-reminders"></a>Kontextabhängige Siri Erinnerungen

Ermöglicht den Benutzer Siri verwenden, um schnell eine Erinnerung, um den Inhalt anzuzeigen, dass sie derzeit in der app zu einem späteren Zeitpunkt anzeigen. Z. B. wenn eine Restaurantkritik in der app angezeigten waren, sie können aufrufen Siri und sagen *"Erinnern dazu an, wenn ich Startseite gelangen."* Siri würde die Erinnerung durch einen Link zur Überprüfung in der app generieren.

## <a name="implementing-proactive-suggestions"></a>Proaktive Vorschläge

Proaktive Vorschlag hinzufügen ist-Support, um die app Xamarin.iOS in der Regel so einfach wie einige APIs implementieren oder Erweitern auf einige APIs, die die app bereits implementiert werden kann.

Proaktive Vorschläge arbeiten mit den apps in drei verschiedene Arten:

- **`NSUserActivity`** -Hilft das System an, welche Informationen zu verstehen, der Benutzer gerade mit Bildschirm arbeitet.
- **Speicherort Vorschläge** – Wenn die app bietet oder basierend Standortinformationen, diese neuen Möglichkeiten für API-Erweiterung Angebot nutzt auf diese Informationen in apps gemeinsam nutzen.

Und wird in der app unterstützt, durch Folgendes implementieren:

- **Kontextabhängige Siri Erinnerungen** – In iOS 10 `NSUserActivity` wurde erweitert, um schnell eine Erinnerung um den Inhalt anzuzeigen, die aktuell angezeigt in der app zu einem späteren Zeitpunkt stellen Siri zulassen.
- **Speicherort Vorschläge** -iOS 10 verbessert `NSUserActivity` zum Erfassen von Standorten innerhalb der app angezeigt, und Stufen sie an vielen Stellen im gesamten System.
- **Anforderungen für kontextabhängige Siri**  -  `NSUserActivity` bietet Kontext, um die Informationen, die Siri innerhalb der app angezeigt, damit der Benutzer Anweisungen oder ein Aufruf direkt aufrufen Siri aus innerhalb der app abrufen kann.

Alle diese Funktionen haben eines gemeinsam, verwenden jedoch alle `NSUserActivity` in einem Formular oder einer anderen, ihre Funktionalität bereitzustellen. 

## <a name="nsuseractivity"></a>NSUserActivity

Wie bereits erwähnt, `NSUserActivity` hilft das System, welche Informationen zu verstehen, der Benutzer aktuell mit auf dem Bildschirm arbeitet. `NSUserActivity` ist ein Lightweight-Status-caching-Mechanismus, um die Aktivität des Benutzers zu erfassen, während sie durch die app navigieren. Betrachten z. B. die Restaurant-app ein:

[ ![](proactive-suggestions-images/activity02.png "Die Restaurant-app")](proactive-suggestions-images/activity02.png)

Mit der folgenden Aktivitäten:

1. Wie der Benutzer mit der app funktioniert eine `NSUserActivity` wird erstellt, um den Status der app später neu zu erstellen.
2. Wenn der Benutzer ein Restaurant sucht, folgt demselben Muster zum Erstellen von Aktivitäten.
3. Und erneut, wenn der Benutzer zeigt ein Ergebnis. In diesem Fall einen Ort anzeigen und in iOS-10, das System mehr bestimmte Konzepte (z. B. Speicherort oder die Kommunikation Interaktionen) bekannt ist.

Führen Sie eine genauere Betrachtung der letzten Seite:

[ ![](proactive-suggestions-images/activity03.png "Die Nutzlast NSUserActivity")](proactive-suggestions-images/activity03.png)

Hier wird die app erstellen eine `NSUserActivity` und verfügt über wurde mit Informationen, um den Status später neu zu erstellen. Die app ist auch einige Metadaten wie Name und Adresse der Standort enthalten. Mit dieser Aktivität erstellt wird kann die app iOS wissen, dass es den aktuellen Status des Benutzers darstellt.

Die app entscheidet sich dann, wenn die Aktivität für die Übergabe drahtlose angekündigt, als temporären Wert für Speicherort Vorschläge gespeichert oder dem auf dem Gerät Spotlight-Index für die Anzeige in den Suchergebnissen hinzugefügt.

Weitere Informationen zu Übergabe und Spotlight-Suche, finden Sie unter unsere [Einführung in die Übergabe](~/ios/platform/handoff.md) und [iOS 9 neue Such-APIs](~/ios/platform/search/index.md) Handbüchern.

### <a name="creating-an-activity"></a>Erstellen einer Aktivität

Vor dem Erstellen einer Aktivität, ist ein Typbezeichner der Aktivität erstellt werden, um sie leichter identifizieren erforderlich. Der Typbezeichner der Aktivität ist eine kurze Zeichenfolge hinzugefügt, die `NSUserActivityTypes` Array von der app `Info.plist` Datei, die zur eindeutigen Identifizierung einer bestimmten Aktivität Benutzertyp verwendet. Im Array für jede Aktivität, die die app unterstützt, und macht App suchen wird ein einziger Eintrag vorhanden sein. Finden Sie in unserer [erstellen Aktivität-IDs Typverweis](~/ios/platform/search/nsuseractivity.md) Weitere Details.

Betrachten Sie ein Beispiel einer Aktivität ein:

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

Eine neue Aktivität wird mithilfe von einer Aktivität Typbezeichner erstellt. Als Nächstes wird einige Metadaten zur Definition der Aktivitäts erstellt, damit dieser Status zu einem späteren Zeitpunkt wiederhergestellt werden kann. Die Aktivität ist anschließend erhält einen aussagekräftigen Titel und an die Benutzerinformationen angefügt. Schließlich sind einige Funktionen aktiviert und die Aktivität an das System gesendet wird.

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

Verfügt der Entwickler eine Website, die die gleiche Informationen wie die app anzeigen kann, die app schließen Sie die URL und die Inhalte können angezeigt werden, auf anderen Geräten, die die app (über Übergabe) nicht installiert haben:

```csharp
// Restore on the web
activity.WebPageUrl = new NSUrl("http://xamarin.com/platform");
```

### <a name="restoring-an-activity"></a>Wiederherstellen einer Aktivität

So reagieren Sie auf den Benutzer Tippen auf ein Suchergebnis (`NSUserActivity`) für die app Bearbeiten der **AppDelegate.cs** Datei, und überschreiben die `ContinueUserActivity` Methode. Zum Beispiel:

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

Sicherstellen, dass dies ist der gleichen Aktivität Typbezeichner (`com.xamarin.platform`) als die oben erstellte Aktivität. Die app verwendet den Informationen in den `NSUserActivity` zum Wiederherstellen des Zustands an, in denen der Benutzer wurde unterbrochen.

### <a name="benefits-of-creating-an-activity"></a>Vorteile der Erstellen einer Aktivität

Mit minimalem Zeitaufwand oben dargestellten Code kann die app nun drei neue iOS 10-Funktionen nutzen:

- **Handoff**
- **Spotlight-Suche**
- **Kontextabhängige Siri Erinnerungen**

Im folgende Abschnitt wird ein Blick auf zwei weitere neue iOS 10-Funktionen zu aktivieren:

- **Speicherort-Vorschläge**
- **Kontextabhängige Siri-Anforderungen**

### <a name="location-based-suggestions"></a>Standortbasierte Vorschläge 

Nehmen Sie als Beispiel der oben genannten Restaurant Suche app. Wenn sie implementiert hat `NSUserActivity` und ordnungsgemäß aufgefüllt aller Metadaten, Attribute und der Benutzer kann jetzt die folgenden Aktionen ausführen:

1. Suchen Sie ein Restaurant in der app, der sie, um einen "Friend" am zu erfüllen möchten.
4. Wenn der Benutzer der Maps-App gewechselt wird, wird das Restaurant-Adresse als Ziel automatisch vorgeschlagen.
5. Dies funktioniert sogar für 3. apps von Drittanbietern (diese Unterstützung `NSUserActivity`), sodass der Benutzer zu einer fuhr-Freigabe-app wechseln kann und das Restaurant Adresse wird automatisch als auch als Ziel es vorgeschlagen.
6. Es bietet darüber hinaus Kontext auf Siri, damit der Benutzer Siri innerhalb der app Restaurant aufrufen kann, und bitten Sie *"Get Richtungen..."* und Siri gebe Richtungen an das Restaurant anzeigen.

Alle oben genannten Funktionen gemeinsam hat dabei, alle anzugeben, wo der Vorschlag ursprünglich von stammt. Bei dem obigen Beispiel ist es der fiktiven Restaurant-app überprüfen.

WatchOS 3 wurde verbessert, um die Funktionalität für eine app über mehrere kleinere Änderungen und Ergänzungen für vorhandene Frameworks aktivieren:

- `NSUserActivity` verfügt über zusätzliche Felder für das Erfassen von Informationen zum Speicherort, die innerhalb der app angezeigt wird.
- Mehrere Erweiterungen MapKit und CoreSpotlight wurden um Speicherort zu erfassen.
- Beachten Sie Speicherort-Funktionalität wurde um Siri, Karten, Multitasking und anderen apps innerhalb des Systems hinzugefügt.

Um Position basierend Vorschläge zu implementieren, starten Sie mit der gleichen Aktivität oben dargestellt:

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

Wenn die app MapKit verwendet wird, ist so einfach wie das Hinzufügen der aktuellen Zuordnung `MKMapItem` an die Aktivität:

```csharp
// Save MKMapItem location
activity.MapItem = myMapItem;
```

Wenn von der app MapKit wird nicht verwendet, kann es übernehmen der App suchen und die folgenden neuen Attribute für Speicherort angeben:

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

Betrachten Sie den oben aufgeführten Code im Detail. Zunächst ist der Name des Orts in jeder Instanz erforderlich:

```csharp
attributes.NamedLocation = "Apple Inc.";
```

Der Text anhand der Beschreibung im Text erforderlich Basis, Instanzen (z. B. die Tastatur QuickType):

```csharp
attributes.SubThoroughfare = "1";
attributes.Thoroughfare = "Infinite Loop";
attributes.City = "Cupertino";
attributes.StateOrProvince = "CA";
attributes.Country = "United States";
```

Breiten- und Längengrade sind optional, aber stellen Sie sicher, dass der Benutzer auf den genauen Speicherort weitergeleitet wird die app zum Senden endversatz ist:

```csharp
attributes.Latitude = 37.33072;
attributes.Longitude = 122.029674;
```

Durch Festlegen der Telefonnummern an, kann die app Zugriff auf Siri erhalten, damit der Benutzer Siri aus der app aufrufen kann, indem Sie darauf hingewiesen werden etwa, * "Platzieren aufrufen":

```csharp
attributes.PhoneNumbers = new string[]{"(800) 275-2273"};
```

Schließlich kann die app darauf hinweisen, ob die Instanz für die Navigation und Anrufe geeignet ist:

```csharp
attributes.SupportsPhoneCalls = true;
attributes.SupportsNavigation = true;
```
## <a name="activities-best-practices"></a>Bewährte Methoden von Aktivitäten

Apple empfiehlt die folgenden bewährten Methoden bei der Arbeit mit Aktivitäten:

- Verwendung `NeedsSave` für verzögerte Nutzlast Updates.
- Stellen Sie sicher, um einen starken Verweis auf die aktuelle Aktivität zu halten.
- Übertragen Sie nur kleine Nutzlasten, die gerade ausreicht, Informationen zum Wiederherstellen des Zustands enthalten.
- Sicherstellen Sie, dass die Aktivität-Datentypbezeichner eindeutigen und aussagekräftigen mithilfe von Reverse-DNS-Notation, anzugeben. 

## <a name="consuming-location-suggestions"></a>Nutzen Speicherort-Vorschläge

In diesem Abschnitt weiter befasst sich mit Nutzung Speicherort Vorschlag, die von anderen Teilen des Systems (z. B. die Maps-app) oder andere 3rd Party apps kommen.

## <a name="routing-apps-and-locations-suggestions"></a>Routing-Apps und Speicherorte-Vorschläge

In diesem Abschnitt wird ein Blick auf Speicherort Vorschläge aus dem direkt innerhalb einer routing-app nutzen. Für das routing app diese Funktionalität hinzufügen, wird der Entwickler nutzen, die vorhandene `MKDirectionsRequest` Framework wie folgt:

- Um die app im Multitasking höher zu stufen.
- Zum Registrieren der app als routing-app.
- Starten der Anwendung mit einem MapKit behandeln `MKDirectionsRequest` Objekt.
- Geben Sie WatchOS die Möglichkeit zu erfahren, wie die app, die basierend auf Benutzer empfohlen.

Wenn die app gestartet wird, mit einem MapKit `MKDirectionsRequest` -Objekt, sollte er automatisch starten, erteilen die Benutzer-Anweisungen auf den angeforderten Pfad oder stellen eine Benutzeroberfläche, die für den Benutzer, erste Richtungen erleichtert. Zum Beispiel:


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

Sehen Sie sich diesen Code im Detail an. Er überprüft, um festzustellen, ob es sich um ein gültiges Ziel-Anforderung ist:

```csharp
if (MKDirectionsRequest.IsDirectionsRequestUrl(url)) {
```

Wenn es ist, erstellt er eine `MKDirectionsRequest` aus der URL:

```csharp
var request = new MKDirectionsRequest(url);
```

Neue kann in WatchOS 3, die app eine Adresse gesendet werden, die nicht Geo-Koordinaten in, aufgrund derer der Entwickler die Adresse codieren muss:

```csharp
var geocoder = new CLGeocoder();
geocoder.GeocodeAddress(address, (place, err)=> {
    // Handle the display of the address
    
});

```

## <a name="summary"></a>Zusammenfassung

Dieser Artikel hat abgedeckt proaktive Vorschläge und wurde gezeigt, wie der Entwickler diese Laufwerk Datenverkehr an eine app Xamarin.iOS für WatchOS verwenden kann. Er behandelt die Schritte, um die proaktive Vorschläge implementieren und Verwendungsrichtlinien dargestellt.


## <a name="related-links"></a>Verwandte Links

- [watchOS-Beispiele](https://developer.xamarin.com/samples/watchos/all/)
- [Programmierhandbuch SiriKit](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/index.html)
