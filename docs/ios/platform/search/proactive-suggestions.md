---
title: Einführung in proaktives vorschlagen in xamarin. IOS
description: In diesem Artikel wird gezeigt, wie Sie proaktive Vorschläge in der xamarin. IOS-App verwenden, um Engagement zu fördern, indem Sie dem Benutzer das proaktive Anzeigen von hilfreichen Informationen für den Benutzer ermöglichen.
ms.prod: xamarin
ms.assetid: 8DDD084A-0D1E-4DF7-B686-6309DCEFF5D3
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: 49e1b382d711f3486782e9e8747ef422c6853979
ms.sourcegitcommit: 0df727caf941f1fa0aca680ec871bfe7a9089e7c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/19/2019
ms.locfileid: "69620978"
---
# <a name="introduction-to-proactive-suggestions-in-xamarinios"></a>Einführung in proaktives vorschlagen in xamarin. IOS

_In diesem Artikel wird gezeigt, wie Sie proaktive Vorschläge in der xamarin. IOS-App verwenden, um Engagement zu fördern, indem Sie dem Benutzer das proaktive Anzeigen von hilfreichen Informationen für den Benutzer ermöglichen._

Neu in ios 10, proaktive Vorschläge stellen News-Möglichkeiten zur Verfügung, mit denen Benutzer eine xamarin. IOS-App einbinden können, indem Sie den Benutzern proaktiv hilfreiche Informationen zu den richtigen Zeitpunkten präsentieren.

IOS 10 bietet neue Möglichkeiten, um die Einbindung in die APP zu fördern, indem es dem System ermöglicht, den Benutzern proaktiv nützliche Informationen zu den richtigen Zeiten zu präsentieren. Ebenso wie IOS 9 die Möglichkeit bot, der App mithilfe von Spotlight, Handoff und Siri-Vorschlägen Deep Search hinzuzufügen (siehe [neue Such-APIs](~/ios/platform/search/index.md)), kann eine APP Funktionen verfügbar machen, die dem Benutzer von dem System aus den folgenden Speicherorten angezeigt werden können. :

- Der APP-Switcher
- Der Sperrbildschirm
- Carplay
- Karten
- Siri-Interaktionen
- Quick Type-Vorschläge

Die APP macht diese Funktionalität für das System mit einer Sammlung von Technologien verfügbar, `NSUserActivity`wie z. b. Web Markup, Core Spotlight, MapKit, Media Player und UIKit. Durch die Bereitstellung einer proaktiven Vorschlags Unterstützung für die APP wird außerdem die Siri-Integration kostenlos unterstützt.

## <a name="location-based-suggestions"></a>Location based-Vorschläge

Neu in ios 10. die `NSUserActivity` -Klasse enthält `MapItem` eine-Eigenschaft, die es dem Entwickler ermöglicht, Standortinformationen bereitzustellen, die in anderen Kontexten verwendet werden können. Wenn die APP beispielsweise Restaurant Reviews anzeigt, kann der Entwickler die- `MapItem` Eigenschaft auf den Speicherort des Restaurants festlegen, das der Benutzer in der APP anzeigt. Wenn der Benutzer zur Maps-APP wechselt, ist der Speicherort des Restaurants automatisch verfügbar.

Wenn die APP die APP-Suche unterstützt, kann Sie die neuen Adress Komponenten `CSSearchableItemAttributesSet` der-Klasse verwenden, um Orte anzugeben, die der Benutzer möglicherweise besuchen möchte. Wenn Sie die `MapItem` -Eigenschaft festlegen, werden die anderen Eigenschaften automatisch ausgefüllt.

Zusätzlich zum Festlegen `Latitude` von und `Longitude` der Eigenschaften der Adress Komponente wird empfohlen, dass die APP auch die `NamedLocation` -Eigenschaft und `PhoneNumbers` die-Eigenschaft bereitstellt, damit Siri einen aufrufungsort initiieren kann.

## <a name="web-markup-based-suggestions"></a>Webmarkup basierte Vorschläge

IOS 9 wurde zur Fähigkeit hinzugefügt, strukturiertes Daten Markup in die Website einzubeziehen, mit dem der Inhalt, der Benutzern in Spotlight-und Safari-Suchergebnissen angezeigt wird, erweitert wird (siehe [Suchen mit webmarkup](~/ios/platform/search/web-markup.md)). IOS 10 bietet die Möglichkeit, Speicherort basiertes Markup (z. b. [PostalAddress](http://schema.org/PostalAddress) wie von [Schema.org](http://schema.org/)definiert) einzuschließen, um die Benutzerfreundlichkeit weiter zu verbessern. Wenn ein Benutzer beispielsweise eine auf der Website markierte Seite anzeigt, kann das System beim Öffnen von Zuordnungen denselben Speicherort vorschlagen.

## <a name="text-based-suggestions"></a>Text basierte Vorschläge

UIKit wurde in ios 10 erweitert und enthält die [textcontenttype](https://developer.apple.com/reference/uikit/uitextinputtraits/1649656-textcontenttype) -Eigenschaft der [uitextinputtrait](https://developer.apple.com/reference/uikit/uitextinputtraits) -Klasse, um die semantische Bedeutung der Inhalte in einem Textbereich anzugeben. Wenn diese Informationen vorhanden sind, kann das System in der Regel automatisch den richtigen Tastatur-Typ auswählen, Vorschläge für die automatische Korrektur verbessern und Informationen aus anderen apps und Websites proaktiv integrieren.

Wenn der Benutzer z. b. Text in ein Textfeld eingibt `UITextContentType.FullStreetAddress`, der als markiert ist, kann das System die automatische Füllung des Felds mit dem Speicherort vorschlagen, den der Benutzer vor kurzem angezeigt hat.

## <a name="media-based-suggestions"></a>Medien basierte Vorschläge

Wenn die APP Medien mithilfe der [mpplayablecontentmanager](xref:MediaPlayer.MPPlayableContentManager) -API wieder gibt, ermöglicht IOS 10 Benutzern das Anzeigen von albumgrafiken und das Abspielen von Medien über die APP auf dem Sperrbildschirm.

## <a name="contextual-siri-reminders"></a>Kontextabhängige Siri-Erinnerungen

Ermöglicht dem Benutzer die Verwendung von Siri, um schnell eine Erinnerung zum Anzeigen der Inhalte zu erhalten, die Sie zu einem späteren Zeitpunkt in der App anzeigen. Wenn Sie z. b. eine Restaurant Überprüfung in der App anzeigen, könnte Sie Siri aufrufen und sagen, *Wenn ich zu Hause komme.* Siri generiert die Erinnerung mit einem Link zur Überprüfung in der app.

## <a name="contact-based-suggestions"></a>Kontaktbasierte Vorschläge

Ermöglicht, dass die Kontakte (und Kontaktinformationen) der app in der **Kontakt** -App auf dem IOS-Gerät zusammen mit allen Benutzer Kontakten angezeigt werden, die im System gespeichert sind.

## <a name="ride-sharing-based-suggestions"></a>Auf Freigabe basierende Vorschläge Reiten

Wenn eine Fahrt Freigabe-APP die [mkdirectionsrequest](xref:MapKit.MKDirectionsRequest) -API verwendet, wird Sie von IOS 10 als Option in der APP-Umstellung angezeigt, wenn der Benutzer wahrscheinlich eine Fahrt wünscht. Die APP muss auch als APP `MKDirectionsModeRideShare` für die Fahrt Freigabe registriert werden, indem für den [mkdirectionsapplicationsupportedmodes](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Articles/iPhoneOSKeys.html) -Schlüssel in `Info.plist` der Datei angegeben wird.

Wenn die app nur Fahrt Freigaben unterstützt, würde der System Vorschlag mit *"Get a Ride to..."* beginnen. wenn andere Arten der Routing Richtung (z. b. "Walking" oder "Bike") unterstützt werden, verwendet das System *"get directions to...".*

> [!IMPORTANT]
> Das von der APP empfangene [mkmapitem](xref:MapKit.MKMapItem) -Objekt enthält möglicherweise keine Längen-und Breitengrad Informationen und erfordert Geocodierung.

## <a name="implementing-proactive-suggestions"></a>Implementieren von proaktiven Vorschlägen

Das Hinzufügen einer proaktiven Vorschlags Unterstützung zu einer xamarin. IOS-APP ist in der Regel genauso einfach wie das Implementieren einiger APIs oder das Erweitern von einigen APIs, die möglicherweise bereits von der APP implementiert werden.

Proaktive Vorschläge können auf drei Arten mit den apps verwendet werden:

- **`NSUserActivity`und**  -  Schema.org`NSUserActivity` unterstützt das System dabei, zu verstehen, welche Informationen der Benutzer zurzeit auf dem Bildschirm arbeitet. Schema.org fügt Webseiten ähnliche Möglichkeiten hinzu.
- **Vorschläge** für den Standort: Wenn die APP standortbasierte Informationen anbietet oder nutzt, bieten diese API-Erweiterungen neue Möglichkeiten, diese Informationen über apps hinweg gemeinsam zu nutzen.
- **Vorschläge** für die Medien-App: das System kann die APP und deren Medieninhalt basierend auf dem Kontext der Interaktion des Benutzers mit dem IOS-Gerät herauf Stufen.

Und werden in der App unterstützt, indem folgendes implementiert wird:

-  -  DerHandoff`NSUserActivity` wurde in ios 8 zur Unterstützung von Handoff hinzugefügt, sodass der Entwickler eine Aktivität auf einem Gerät starten und dann auf einem anderen Gerät fortsetzen kann (siehe [Einführung in die Übergabe](~/ios/platform/handoff.md)).
- **Spotlight-Suche** : IOS 9 hat die Möglichkeit hinzugefügt, App-Inhalte aus den Spotlight- `NSUserActivity` Suchergebnissen mithilfe von zu bewerben (siehe [Suchen mit Core Spotlight](~/ios/platform/search/corespotlight.md)).
- **Kontextabhängige Siri-Erinnerungen** : in ios `NSUserActivity` 10 wurde erweitert, damit Siri rasch eine Erinnerung zum Anzeigen der Inhalte erhalten kann, die der Benutzer zurzeit in der APP an einem späteren Zeitpunkt anzeigen kann.
- **Location-Vorschläge** : IOS 10 `NSUserActivity` verbessert die Erfassung von Standorten, die in der App angezeigt werden, und stuft sie an vielen Stellen im gesamten System herauf.
- **Kontextabhängige Siri-Anforderungen**  -  `NSUserActivity` stellen Kontextinformationen bereit, die in der APP für Siri bereitgestellt werden, damit der Benutzeranweisungen abrufen oder aufrufen kann, um Siri innerhalb der APP aufzurufen.
- **Interaktionen mit Interaktionen** : neu in ios 10 `NSUserActivity` ermöglicht das herauf Stufen von Kommunikations-apps von einer Kontaktkarte (in der App "Kontakte") als Alternative Kommunikationsmethode.

Alle diese Features haben eine gemeinsame Funktion, die alle in einer `NSUserActivity` Form oder einem anderen zum Bereitstellen Ihrer Funktionalität verwendet werden. 

## <a name="nsuseractivity"></a>NSUserActivity

Wie bereits erwähnt, `NSUserActivity` unterstützt das System dabei, zu verstehen, welche Informationen der Benutzer zurzeit auf dem Bildschirm arbeitet. `NSUserActivity`bei handelt es sich um einen einfachen Mechanismus zum Zwischenspeichern von Zuständen, um die Aktivität des Benutzers zu erfassen, während Sie durch die APP navigieren. Sehen Sie sich z. b. eine Restaurant-APP an:

[![](proactive-suggestions-images/activity02.png "Der Mechanismus zum Zwischenspeichern von nsuseractivity mit geringem Gewicht")](proactive-suggestions-images/activity02.png#lightbox)

Mit den folgenden Interaktionen:

1. Wenn der Benutzer mit der APP arbeitet, wird `NSUserActivity` eine erstellt, um den Status der APP später neu zu erstellen.
2. Wenn der Benutzer nach einem Restaurant sucht, wird das gleiche Muster zum Erstellen von Aktivitäten befolgt.
3. Und auch hier, wenn der Benutzer ein Ergebnis anzeigt. Im letzten Fall wird der Benutzer einen Speicherort anzeigen und in ios 10 bestimmte Konzepte (z. b. Standort-oder Kommunikations Interaktionen) besser kennen.

Sehen Sie sich den letzten Bildschirm genauer an:

[![](proactive-suggestions-images/activity03.png "Die nsuseractivity-Details")](proactive-suggestions-images/activity03.png#lightbox)

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

Der Entwickler muss sicherstellen, dass es sich hierbei um denselben Aktivitätstyp Bezeichner (`com.xamarin.platform`) wie die oben erstellte Aktivität handelt. Die APP verwendet die in `NSUserActivity` gespeicherten Informationen, um den Zustand wiederherzustellen, an dem der Benutzer aufgehört hat.

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
2. Wenn der Benutzer mit der Multitasking-App-Umleitung von der app Weg wechselt, zeigt das System automatisch einen Vorschlag an (unten auf dem Bildschirm), um mit seiner bevorzugten Navigations-App Anweisungen zum Restaurant zu erhalten.
3. Wenn der Benutzer zur Nachrichten-APP wechselt und mit der Eingabe von *"lassen Sie sich an"* beginnt, schlägt die quicktype-Tastatur automatisch vor, dass die Adresse des Restaurants eingefügt wird.
4. Wenn der Benutzer zur Maps-APP wechselt, wird die Adresse des Restaurants automatisch als Ziel vorgeschlagen.
5. Dies funktioniert auch für Drittanbieter-Apps (die unter `NSUserActivity`Stützung für), sodass der Benutzer zu einer Fahrt Freigabe-App wechseln kann, und die Adresse des Restaurants wird ebenfalls automatisch als Ziel vorgeschlagen.
6. Außerdem wird der Kontext für Siri bereitgestellt, sodass der Benutzer Siri innerhalb der Restaurant-App aufrufen und *"get directions..." (DIRECTIONS...* ) abrufen kann.

Alle oben genannten Funktionen haben eine gemeinsame Sache, Sie geben alle an, woher der Vorschlag ursprünglich stammt. Im Beispiel oben ist dies die fiktive Restaurant Review-app.

IOS 10 wurde verbessert, um diese Funktionalität für eine APP durch mehrere kleine Änderungen und Ergänzungen vorhandener Frameworks zu ermöglichen:

- `NSUserActivity`verfügt über zusätzliche Felder zum Erfassen von Standortinformationen, die in der App angezeigt werden.
- An MapKit und corespotlight wurden mehrere Ergänzungen vorgenommen, um den Speicherort zu erfassen.
- Die Funktion zum Erkennen von Standorten wurde Siri, Maps, Tastaturen, Multitasking und anderen apps im System hinzugefügt.

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

Anschließend ist die textbasierte Beschreibung des Speicher Orts für textbasierte Instanzen (z. b. die quicktype-Tastatur) erforderlich:

```csharp
attributes.SubThoroughfare = "1";
attributes.Thoroughfare = "Infinite Loop";
attributes.City = "Cupertino";
attributes.StateOrProvince = "CA";
attributes.Country = "United States";
```

Der breiten-und Längengrad ist optional, aber stellen Sie sicher, dass der Benutzer an den genauen Speicherort weitergeleitet wird, an den die APP Sie senden möchte. Daher sollte er eingeschlossen werden:

```csharp
attributes.Latitude = 37.33072;
attributes.Longitude = 122.029674;
```

Durch Festlegen der Telefonnummern kann die APP auf Siri zugreifen, sodass der Benutzer Siri von der APP aufrufen kann, indem er etwas wie *"Anruf an dieser Stelle"* sagt:

```csharp
attributes.PhoneNumbers = new string[]{"(800) 275-2273"};
```

Zum Schluss kann die APP angeben, ob die Instanz für Navigations-und Telefonanrufe geeignet ist:

```csharp
attributes.SupportsPhoneCalls = true;
attributes.SupportsNavigation = true;
```

### <a name="implementing-contact-interactions"></a>Implementieren von Kontakt Interaktionen

Neu in ios 10: Kommunikations-apps sind über die Kontaktkarte tief in die app "Kontakte" integriert. Für apps, die Interaktionen mit Kontakten implementiert haben, kann der Benutzer die Informationen der angegebenen app bestimmten Personen in Ihren Kontakten hinzufügen. Wenn Sie z. b. die Schaltfläche schnell Aktion am oberen Rand einer Karte drücken, um eine Nachricht zu senden, wird die angefügte App als Option zum Senden der Nachricht aufgeführt.

Wenn eine Drittanbieter-App ausgewählt ist, kann Sie gespeichert und als Standardmethode angezeigt werden, wenn der Benutzer das nächste Mal eine Verbindung mit Ihnen aufnehmen möchte.

Kontakt Interaktionen werden in der App mithilfe `NSUserActivity` von und dem neuen Intents-Framework implementiert, das in ios 10 eingeführt wird. Weitere Informationen zum Arbeiten mit Intents finden Sie Untergrund Legendes zu [Sirikit-Konzepten](~/ios/platform/sirikit/understanding-sirikit.md) und [Implementieren von Sirikit](~/ios/platform/sirikit/implementing-sirikit.md) -Handbüchern.

#### <a name="donating-interactions"></a>Spenden von Interaktionen

Sehen Sie sich an, wie die APP Interaktionen spenden kann:

[![](proactive-suggestions-images/activity04.png "Übersicht über das über Spenden")](proactive-suggestions-images/activity04.png#lightbox)

Die App erstellt ein `INInteraction` Objekt, das eine **Absicht** (`INIntent`), **Teilnehmer** und **Metadaten**enthält. Die **Absicht** stellt eine Benutzeraktion dar, z. b. das Erstellen eines Videos oder das Senden einer Textnachricht. Zu den **Teilnehmern** gehören die Personen, die die Kommunikation erhalten. Die **Metadaten** definieren zusätzliche Informationen, z. b. das erfolgreiche Senden der Nachricht usw.

Der Entwickler erstellt niemals direkt eine Instanz von `INIntent` oder `INIntentResponse`, er verwendet eine der spezifischen untergeordneten Klassen (basierend auf der Aufgabe, die die APP im Namen des Benutzers ausführt), die von diesen übergeordneten Klassen geerbt werden. Zum Beispiel `INSendMessageIntent` und `INSendMessageIntentResponse` zum Senden einer Textnachricht. 

Wenn die Interaktion vollständig aufgefüllt ist, wird `DonateInteraction` die-Methode aufgerufen, um das System zu informieren, dass die Interaktion zur Verwendung verfügbar ist.

Wenn der Benutzer von der Kontaktkarte mit der APP interagiert, wird die Interaktion mit einem `NSUserActivity`gebündelt, das dann zum Starten der APP verwendet wird:

[![](proactive-suggestions-images/activity05.png "Die Interaktion wird mit einer nsuseractivity gebündelt, die zum Starten der APP verwendet wird.")](proactive-suggestions-images/activity05.png#lightbox)

Sehen Sie sich das folgende Beispiel für eine Absicht der Sende Nachricht an:

```csharp
using System;
using Foundation;
using UIKit;
using Intents;

namespace MonkeyNotification
{
  public class DonateInteraction
  {
    #region Constructors
    public DonateInteraction ()
    {
    }
    #endregion

    #region Public Methods
    public void SendMessageIntent (string text, INPerson from, INPerson[] to)
    {

      // Create App Activity
      var activity = new NSUserActivity ("com.xamarin.message");

      // Define details
      var info = new NSMutableDictionary ();
      info.Add (new NSString ("message"), new NSString (text));

      // Populate Activity
      activity.Title = "Sent MonkeyChat Message";
      activity.UserInfo = info;

      // Enable capabilities
      activity.EligibleForSearch = true;
      activity.EligibleForHandoff = true;
      activity.EligibleForPublicIndexing = true;

      // Inform system of Activity
      activity.BecomeCurrent ();

      // Create message Intent
      var intent = new INSendMessageIntent (to, text, "", "MonkeyChat", from);

      // Create Intent Reaction
      var response = new INSendMessageIntentResponse (INSendMessageIntentResponseCode.Success, activity);

      // Create interaction
      var interaction = new INInteraction (intent, response);

      // Donate interaction to the system
      interaction.DonateInteraction ((err) => {
        // Handle donation error
        ...
      });
    }
    #endregion
  }
}
```

Wenn Sie diesen Code ausführlich betrachten, wird eine Instanz von `NSUserActivity` erstellt und aufgefüllt (wie im obigen Abschnitt [Erstellen einer Aktivität](#creating-an-activity) gezeigt). Anschließend wird eine Instanz von `INSendMessageIntent` erstellt (die von `INIntent`erbt) und mit den Details der gesendeten Nachricht aufgefüllt:

```csharp
var intent = new INSendMessageIntent (to, text, "", "MonkeyChat", from);
```

Eine `INSendMessageIntentResponse` wird erstellt und an die `NSUserActivity` oben erstellte weitergeleitet:

```csharp
var response = new INSendMessageIntentResponse (INSendMessageIntentResponseCode.Success, activity);
```

Eine `INInteraction` wird aus den soeben erstellten Sende Nachrichten Absicht`INSendMessageIntent`() und der`INSendMessageIntentResponse`Antwort () erstellt:

```csharp
var interaction = new INInteraction (intent, response);
```

Zum Schluss wird das System über die Interaktion benachrichtigt:

```csharp
// Donate interaction to the system
interaction.DonateInteraction ((err) => {
  // Handle donation error
  ...
});
```

Ein Vervollständigungs Handler wird an die Stelle geleitet, an der die APP auf die erfolgreiche oder fehlgeschlagene Spende reagieren kann.

### <a name="activities-best-practices"></a>Bewährte Methoden für Aktivitäten

Apple schlägt beim Arbeiten mit Aktivitäten die folgenden bewährten Methoden vor:

- Verwenden `NeedsSave` Sie für Lazy Payload-Updates.
- Stellen Sie sicher, dass Sie einen starken Verweis auf die aktuelle Aktivität erhalten.
- Übertragen Sie nur kleine Nutzlasten, die nur genügend Informationen enthalten, um den Status wiederherzustellen.
- Stellen Sie sicher, dass die Aktivitätstyp Bezeichner eindeutig und beschreibend sind, indem Sie die Reverse-DNS-Notation zum angeben verwenden. 

## <a name="schemaorg"></a>Schema.org

Wie oben gezeigt, `NSUserActivity` unterstützt das System das Verständnis der Informationen, mit denen der Benutzer zurzeit auf dem Bildschirm arbeitet. Schema.org fügt Webseiten ähnliche Möglichkeiten hinzu.

Schema.org kann die gleichen Typen von ortsbasierten Interaktionen mit der Website bereitstellen. Apple hat die neuen Location-Vorschläge so konzipiert, dass Sie genauso gut funktionieren, wenn Sie in Safari wie in einer nativen App angezeigt werden.

Einige Schema.org Hintergrund:

- Er bietet einen offenen webmarkup-vokabularstandard.
- Dies funktioniert, indem strukturierte Metadaten auf Webseiten eingeschlossen werden.
- Es gibt mehr als 500 Schemas, die verschiedene verfügbare Konzepte darstellen.
- Der Entwickler kann die Vorteile der Verwendung von in einer nativen App nutzen `NSUserActivity` , indem er ihn auf der Website implementiert.

Die Schemas werden in einer Struktur wie der Struktur angeordnet, in der bestimmte Typen, z. b. *Restaurant*, von generischen Typen wie z. b. dem *lokalen Unternehmen*erben. Weitere Informationen finden Sie unter [Schema.org](http://schema.org).

Wenn die Webseite z. b. die folgenden Daten enthält:

```html
<script type="application/ld+json">
{
  "@context":"http://schema.org",
  "@type":"Restaurant",
  "telephone":"(415) 781-1111",
  "url":"https://www.yanksing.com",
  "address":{
    "@type":"PostalAddress",
    "streetAddress":"101 Spear St",
    "addressLocality":"San Francisco",
    "postalCode":"94105",
    "addressRegion":"CA"
  },
  "aggregateRating":{
    "@type":"AggregateRating",
    "ratingValue":"3.5",
    "reviewCount":"2022"
  }
}
</script>
```

Wenn der Benutzer diese Seite in Safari besucht und dann zu einer anderen APP gewechselt hat, werden die Standortinformationen auf der Seite aufgezeichnet und in anderen Teilen des Systems in anderen Teilen des Systems angeboten (wie in den obigen Aktivitäten gezeigt).

Safari extrahiert alles auf einer Webseite, die den folgenden Schema Eigenschaften entspricht:

- **PostalAddress**
- **PIN**
- Eine Telefon Eigenschaft.

Weitere Informationen finden Sie im Leitfaden für die [Suche mit webmarkup](~/ios/platform/search/web-markup.md) .

## <a name="consuming-location-suggestions"></a>Nutzen von Location-Vorschlägen

Im nächsten Abschnitt werden die Vorschläge für den nutzenden Standort behandelt, die aus anderen Teilen des Systems (z. b. der Maps-APP) oder anderen Drittanbieter-apps stammen.

Es gibt zwei Hauptmethoden, mit denen die APP Location-Vorschläge nutzen kann:

- Über die quicktype-Tastatur.
- Direkt durch die Nutzung der Standortinformationen in Routing-apps.

### <a name="location-suggestions-and-the-quicktype-keyboard"></a>Location-Vorschläge und die quicktype-Tastatur

Wenn die APP Adressen in textbasierten Formaten behandelt, kann die APP über die quicktype-Benutzeroberfläche die Empfehlungen für den Standort nutzen. IOS 10 erweitert die quicktype-Benutzeroberfläche mit den folgenden Features:

- Die APP kann Hinweise zur Semantik Absicht für Textfelder in der Benutzeroberfläche hinzufügen.
- Die APP kann proaktive Vorschläge in der APP erhalten.
- Die APP kann von der verbesserten AutoKorrektur profitieren.

Die neue `TextContentType` Eigenschaft der Textfeld-Steuerelemente in ios 10 ermöglicht es dem Entwickler, die semantische Absicht für den Wert zu definieren, den der Benutzer in ein bestimmtes Feld eingegeben hat. Beispiel:

```csharp
var textField = new UITextField();
textField.TextContentType = UITextContentType.FullStreetAddress;
```

Teilt dem System mit, dass die APP erwartet, dass der Benutzer eine vollständige Straße in das angegebene Feld eingegeben hat. Dadurch kann die quicktype-Tastatur automatisch Positions Vorschläge auf der Tastatur bereitstellen, wenn der Benutzer einen Wert in diesem Feld eingibt.

Im folgenden sind einige der gängigeren Typen aufgeführt, die dem Entwickler in der `UITextContentType` statischen-Klasse zur Verfügung stehen:

* `Name`
* `GivenName`
* `FamilyName`
* `Location`
* `FullStreetAddress`
* `AddressCityAndState`
* `TelephoneNumber`
* `EmailAddress`

### <a name="routing-apps-and-locations-suggestions"></a>Vorschläge für Routing-apps und-Standorte

In diesem Abschnitt wird erläutert, wie Sie Orts Vorschläge direkt aus einer Routing-APP heraus nutzen. Damit die Routing-App Diese Funktionalität hinzufügen kann, nutzt der Entwickler das `MKDirectionsRequest` vorhandene Framework wie folgt:

- Herauf Stufen der APP im Multitasking
- Zum Registrieren der App als Routing-app.
- Um das Starten der APP mit einem MapKit `MKDirectionsRequest` -Objekt zu behandeln.
- , Um IOS zu ermöglichen, zu den entsprechenden Zeitpunkten auf der Grundlage von Benutzer Engagement zu erfahren, wie Sie dem Benutzer die APP hinzulegen können.

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

Neu in ios 10: der APP kann eine Adresse gesendet werden, die keine Geokoordinaten hat. Dies bewirkt, dass der Entwickler die Adresse codieren muss:

```csharp
var geocoder = new CLGeocoder();
geocoder.GeocodeAddress(address, (place, err)=> {
  // Handle the display of the address
  
});

```

## <a name="media-app-suggestions"></a>Vorschläge für Medien-App

Wenn die APP beliebige Medientypen wie z. b. eine Podcasts-APP oder Streamingmedieninhalte wie z. b. Audiodateien oder Videos mit IOS 10 verarbeitet, kann Sie die Media-Vorschläge im System nutzen.

Für apps, die Medien verarbeiten, unterstützt IOS die folgenden Verhaltensweisen:

- IOS fördert apps, die der Benutzer wahrscheinlich auf der Grundlage des vorherigen Verhaltens verwendet.
- Vorschläge im Zusammenhang mit der App werden im Spotlight und in der Ansicht "heute" angezeigt.
- Wenn die APP einen der folgenden Trigger erfüllt, kann Sie auf einen Sperrbildschirm Vorschlag herauf gestuft werden:
  - Nach dem Plug auf den Kopfhörern oder einem Bluetooth-Gerät stellt eine Verbindung her.
  - Nach dem Einstieg in ein Auto.
  - Nach dem Eintreffen zu Hause oder bei der Arbeit. 

Wenn Sie einen einfachen API-Befehl in ios 10 einschließen, kann der Entwickler für die Benutzer der Medien-App eine ansprechkräftere Sperrbildschirm Darstellung erstellen. Durch die Verwendung `MPPlayableContentManager` der-Klasse zum Verwalten der Medienwiedergabe werden die vollständigen mediensteuer Elemente (wie die von der Music-App dargestellten) auf dem Sperrbildschirm für die App angezeigt.


```csharp
using System;
using MediaPlayer;
using UIKit;

namespace MonkeyPlayer
{
  public class PlayableContentDelegate : MPPlayableContentDelegate
  {
    #region Constructors
    public PlayableContentDelegate ()
    {
    }
    #endregion

    #region Override methods
    public override void InitiatePlaybackOfContentItem (MPPlayableContentManager contentManager, Foundation.NSIndexPath indexPath, Action<Foundation.NSError> completionHandler)
    {
      // Access the media item to play
      var item = LoadMediaItem (indexPath);

      // Populate the lock screen
      PopulateNowPlayingItem (item);

      // Prep item to be played
      var status = PreparePlayback (item);

      // Call completion handler
      completionHandler (null);
    }
    #endregion

    #region Public Methods
    public MPMediaItem LoadMediaItem (Foundation.NSIndexPath indexPath)
    {
      var item = new MPMediaItem ();

      // Load item from media store
      ...

      return item;
    }

    public void PopulateNowPlayingItem (MPMediaItem item)
    {
      // Get Info Center and album art
      var infoCenter = MPNowPlayingInfoCenter.DefaultCenter;
      var albumArt = (item.Artwork == null) ? new MPMediaItemArtwork (UIImage.FromFile ("MissingAlbumArt.png")) : item.Artwork;

      // Populate Info Center
      infoCenter.NowPlaying.Title = item.Title;
      infoCenter.NowPlaying.Artist = item.Artist;
      infoCenter.NowPlaying.AlbumTitle = item.AlbumTitle;
      infoCenter.NowPlaying.PlaybackDuration = item.PlaybackDuration;
      infoCenter.NowPlaying.Artwork = albumArt;
    }

    public bool PreparePlayback (MPMediaItem item)
    {
      // Prepare media item for playback
      ...

      // Return results
      return true;
    }
    #endregion
  }
}
```

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden proaktive Vorschläge behandelt, und es wurde gezeigt, wie der Entwickler diese zum Steuern von Datenverkehr an die xamarin. IOS-App verwenden kann. Es wurden die Schritte zum Implementieren von proaktiven Vorschlägen und zur Verwendung von Verwendungs Richtlinien behandelt.



## <a name="related-links"></a>Verwandte Links

- [IOS 10-Beispiele](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS10)
- [Sirikit-Programmier Handbuch](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/index.html)
