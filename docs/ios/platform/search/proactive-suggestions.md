---
title: Einführung in die proaktive Vorschläge in Xamarin.iOS
description: In diesem Artikel zeigt, wie auf proaktive Vorschläge in der Xamarin.iOS-app zum fördern der Interaktion zu verwenden, indem Sie dem System proaktiv hilfreiche Informationen automatisch, die Benutzer angezeigt wird.
ms.prod: xamarin
ms.assetid: 8DDD084A-0D1E-4DF7-B686-6309DCEFF5D3
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: 7b7564e3b94062c2294919121f32c4f830346bda
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50105336"
---
# <a name="introduction-to-proactive-suggestions-in-xamarinios"></a>Einführung in die proaktive Vorschläge in Xamarin.iOS

_In diesem Artikel zeigt, wie auf proaktive Vorschläge in der Xamarin.iOS-app zum fördern der Interaktion zu verwenden, indem Sie dem System proaktiv hilfreiche Informationen automatisch, die Benutzer angezeigt wird._

Noch nicht mit iOS 10, proaktive Vorschläge vorhanden News Möglichkeiten für Benutzer von proaktiv vorhanden hilfreiche Informationen automatisch an den Benutzer zur richtigen Zeit jeweils mit einer Xamarin.iOS-app in Verbindung setzen.

iOS 10 stellt neue Methoden für die treibende Engagement für die app dem System proaktiv hilfreiche Informationen automatisch an den Benutzer zur richtigen Zeit jeweils vorhanden. Wie iOS 9 die Möglichkeit, die detaillierte Suche der App mit Spotlight, Übergabeanimationen und Siri Vorschläge hinzuzufügen bereitgestellt (finden Sie unter [neuen Such-APIs](~/ios/platform/search/index.md)), mit iOS 10 kann eine app-Funktionen, die für den Benutzer durch das System aus angezeigt werden kann machen die folgende Speicherorte:

- Der App-Switcher
- Der Sperrbildschirm
- CarPlay
- Karten
- Siri-Interaktionen
- QuickType Vorschläge

Die app stellt diese Funktionen an das System eine Sammlung von Technologien wie z. B. über `NSUserActivity`, Markup im Web, Core Spotlight, MapKit, Media Player und UIKit. Darüber hinaus durch die proaktive Vorschläge Unterstützung für die app, ruft sie ab engere Integration von Siri kostenlos.

## <a name="location-based-suggestions"></a>Standortbasierte Vorschläge

Noch nicht mit iOS 10, die `NSUserActivity` Klasse enthält eine `MapItem` -Eigenschaft, die ermöglicht es dem Entwickler, um Informationen bereitzustellen, die in anderen Kontexten verwendet werden kann. Z. B. wenn die app Restaurantkritiken angezeigt wird, können Sie festlegen der `MapItem` Eigenschaft, um den Speicherort des Restaurants, die dem Benutzer in der app angezeigt wird. Wenn der Benutzer an die Maps-app wechselt, ist das Restaurant Speicherort automatisch verfügbar.

Wenn die app die App-Suche unterstützt, können sie die neue Adresskomponenten der `CSSearchableItemAttributesSet` Klasse Speicherorte an, die der Benutzer möglicherweise besuchen möchte. Durch Festlegen der `MapItem` -Eigenschaft, die anderen Eigenschaften sind, automatisch ausgefüllt.

Zusätzlich zum Festlegen der `Latitude` und `Longitude` der Eigenschaften der Komponente, wird empfohlen, geben Sie die app die `NamedLocation` und `PhoneNumbers` Eigenschaften, also Siri einen Aufruf an den Speicherort initiieren kann.

## <a name="web-markup-based-suggestions"></a>Webmarkup basierende Vorschläge

iOS 9, die Möglichkeit, strukturierte Daten Markup auf der Website zu enthalten, die den Inhalt ergänzt, die Benutzern, in den Suchergebnissen für Spotlight und Safari angezeigt hinzugefügt (finden Sie unter [Suche mit Webmarkup](~/ios/platform/search/web-markup.md)). iOS 10 fügt die Möglichkeit, standortbasierte Markup enthalten (z. B. [PostalAddress](http://schema.org/PostalAddress) gemäß [Schema.org](http://schema.org/)), um die benutzerfreundlichkeit weiter zu verbessern. Z. B. wenn ein Benutzer Ansichten einen Speicherort auf der Website markiert ist, kann das System am gleichen Speicherort vorschlagen beim Öffnen von Zuordnungen.

## <a name="text-based-suggestions"></a>Textbasiertes Vorschläge

UIKit wurde erweitert, unter iOS 10 sollen die [TextContentType](https://developer.apple.com/reference/uikit/uitextinputtraits/1649656-textcontenttype) Eigenschaft der [UITextInputTraits](https://developer.apple.com/reference/uikit/uitextinputtraits) Klasse, um die semantische Bedeutung für den Inhalt in einem Textbereich anzugeben. Mit diesen Informationen vorhanden das System kann wählen Sie in der Regel automatisch die entsprechende Tastenkombination-Typ, AutoKorrektur Vorschlägen verbessern und proaktiv Informationen aus anderen apps und Websites integrieren.

Z. B., wenn der Benutzer Text in einem Textfeld markiert eingibt `UITextContentType.FullStreetAddress`, das System kann das Feld mit dem Speicherort, die der Benutzer wurde vor kurzem anzeigen automatische Ausfüllen vorschlagen.

## <a name="media-based-suggestions"></a>Media-basierte Vorschläge

Wenn die app Medien verwenden, spielt die [MPPlayableContentManager](https://developer.xamarin.com/api/type/MediaPlayer.MPPlayableContentManager/) -API, iOS 10 kann Benutzer das Albumcover anzeigen und Wiedergeben von Medien über die app auf dem Sperrbildschirm angezeigt.

## <a name="contextual-siri-reminders"></a>Kontextbezogene Siri Erinnerungen

Ermöglicht dem Benutzer um Siri verwenden, um schnell eine Erinnerung, um den Inhalt anzuzeigen, dass sie zu einem späteren Zeitpunkt gerade in der app angezeigt werden. Z. B. Wenn sie eine Restaurantkritik in der app angezeigt haben, sie könnten aufrufen Siri und sagen *"Erinnern dazu beim Start mir."* Siri würde die Erinnerung mit einem Link zur Überprüfung in der app generieren.

## <a name="contact-based-suggestions"></a>Wenden Sie sich an basierende Vorschläge

Ermöglicht der app Kontakte (und verwandte Kontaktinformationen) in angezeigt werden die **wenden Sie sich an** app auf iOS-Geräten sowie alle Benutzer Kontakte im System gespeichert.

## <a name="ride-sharing-based-suggestions"></a>Tour Freigabe basierende Vorschläge

Wenn eine app carsharinganbietern verwendet die [MKDirectionsRequest](https://developer.xamarin.com/api/type/MapKit.MKDirectionsRequest/) -API, iOS 10 bietet es als Option in der app-Switcher gelegentlich verwendet werden, wenn der Benutzer soll eine Fahrt wahrscheinlich ist. Die app muss auch als carsharinganbietern-app registriert werden, durch Angabe der `MKDirectionsModeRideShare` für die [MKDirectionsApplicationSupportedModes](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Articles/iPhoneOSKeys.html) -Schlüssel in der `Info.plist` Datei.

Wenn die app nur bei Stromausfällen freigeben unterstützt, würde der Vorschlag System beginnen, mit *"Eine Tour zum Abrufen..."*, wenn andere Typen von routing Richtung (z. B. Walking "oder" Bike ") unterstützt werden, verwendet das System *"Anweisungen zum Abrufen..."*

> [!IMPORTANT]
> Die [MKMapItem](https://developer.xamarin.com/api/type/MapKit.MKMapItem/) -Objekt, das die app erhält möglicherweise keine Längen- und Breitengrad-Informationen und geocodierung benötigen.

## <a name="implementing-proactive-suggestions"></a>Proaktive Vorschläge

Proaktive Vorschlag hinzufügen, ist Unterstützung für eine Xamarin.iOS-app in der Regel ganz einfach einige APIs implementieren oder Erweitern auf einige wenige APIs, die die app bereits implementiert werden kann.

Proaktive Vorschläge arbeiten mit den apps auf drei Arten:

- **`NSUserActivity` und Schema.org**  -  `NSUserActivity` kann das System aus, welche Informationen, die der Benutzer zurzeit mit auf dem Bildschirm arbeitet. Schema.org hinzugefügt Webseiten ähnliche Möglichkeiten.
- **Speicherort-Vorschläge** – Wenn die app bietet oder diese neuen Möglichkeiten für API-Erweiterung Angebot positionsbasierten Informationen nutzt diese Informationen über apps hinweg zu teilen.
- **Medien-App-Vorschläge** – das System kann die app bewerben und die Medieninhalte auf Grundlage des Kontexts der Interaktion mit dem iOS-Gerät des Benutzers.

Und wird in der app unterstützt, durch die folgende Implementierung:

- **Übergabe**  -  `NSUserActivity` wurde hinzugefügt, in iOS 8 verwenden, um die Übergabe, zu unterstützen, die kann der Entwickler eine Aktivität auf einem Gerät starten, und klicken Sie dann auf einem anderen fortgesetzt (finden Sie unter [Einführung in die Übergabe](~/ios/platform/handoff.md)).
- **Spotlight-Suche** -iOS 9 hinzugefügt, die Möglichkeit, app-Inhalte innerhalb der Spotlight-Suche gibt Ergebnisse mit heraufstufen `NSUserActivity` (finden Sie unter [Suche mit Core Spotlight](~/ios/platform/search/corespotlight.md)).
- **Kontextbezogene Siri Erinnerungen** – In iOS 10 `NSUserActivity` wurde erweitert, um erlauben Siri schnell stellen eine Erinnerung um den Inhalt anzuzeigen, die der Benutzer wird zu einem späteren Zeitpunkt gerade in der app anzeigt.
- **Speicherort-Vorschläge** -iOS 10 erhöht `NSUserActivity` zum Erfassen von Standorten, die innerhalb der app angezeigt, und Stufen sie an vielen Stellen im gesamten System.
- **Kontextbezogene Siri-Anforderungen**  -  `NSUserActivity` stellt Kontext für die Informationen in der app siri, damit der Benutzer Anweisungen oder ein Aufruf direkt aufrufen Siri innerhalb der app abrufen kann.
- **Wenden Sie sich an Interaktionen** – unter iOS 10, neue `NSUserActivity` Communications-apps, die von einer Kontaktkarte (in der Kontakte-app) als eine alternative Kommunikationsmethode höher gestuft werden können.

Alle diese Funktionen haben eine Gemeinsamkeit, dass alle verwenden `NSUserActivity` in einem Formular oder eine andere, um die jeweiligen Funktionen zur Verfügung. 

## <a name="nsuseractivity"></a>NSUserActivity

Wie bereits erwähnt, `NSUserActivity` kann das System aus, welche Informationen, die der Benutzer zurzeit mit auf dem Bildschirm arbeitet. `NSUserActivity` ist ein Lightweight-Zwischenspeicherung des Mechanismus für die Aktivität des Benutzers zu erfassen, wie sie durch die app navigieren. Betrachten z. B. eine Restaurant-app ein:

[![](proactive-suggestions-images/activity02.png "Der Cachemechanismus NSUserActivity leicht-Status")](proactive-suggestions-images/activity02.png#lightbox)

Mit dem folgenden Interaktionen:

1. Da der Benutzer mit der app arbeitet ein `NSUserActivity` wird erstellt, um den Zustand der app später neu erstellen.
2. Wenn der Benutzer für ein Restaurant sucht, folgt das gleiche Muster zum Erstellen von Aktivitäten.
3. Und Sie erneut, wenn der Benutzer zeigt ein Ergebnis. Im letzten Fall einem Ort anzeigen und in iOS 10, das System bessere Unterstützung für bestimmte Konzepte (z. B. Speicherort oder die Kommunikation Interaktionen).

Führen Sie einen genaueren Blick auf dem letzten Bildschirm:

[![](proactive-suggestions-images/activity03.png "Die NSUserActivity-details")](proactive-suggestions-images/activity03.png#lightbox)

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

Für den Benutzer Tippen auf ein Suchergebnis reagieren (`NSUserActivity`) für die app Bearbeiten der **Datei "appdelegate.cs"** Datei, und überschreiben die `ContinueUserActivity` Methode. Zum Beispiel:

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

Der Entwickler muss sicher, dass dem gleichen Aktivitätsbezeichner-Typ ist (`com.xamarin.platform`) als die oben erstellte Aktivität. Die app verwendet den Informationen in den `NSUserActivity` zum Wiederherstellen des Zustands an, wo der Benutzer unterbrochen.

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
2. Wie der Benutzer außerhalb der app mithilfe der app-Switcher Multitasking verschiebt, zeigt das System automatisch einen Vorschlag (am unteren Rand des Bildschirms) um Anweisungen an das Restaurant, die mit ihrer bevorzugten Navigation-app zu erhalten.
3. Wenn der Benutzer in den Nachrichten-app wechselt, und mit der Eingabe beginnt *"Treffen wir uns an"*, die Tastatur QuickType schlägt automatisch die Adresse des Restaurants einfügen.
4. Wenn der Benutzer an die Maps-app wechselt, wird das Restaurant die Adresse als Ziel automatisch vorgeschlagen.
5. Dies funktioniert sogar für 3. apps von Drittanbietern (, unterstützen `NSUserActivity`), sodass der Benutzer für eine Fahrt-Freigabe-app wechseln kann und das Restaurant die Adresse wird automatisch auch als Ziel es empfohlen.
6. Es stellt auch Kontext siri, kann der Benutzer Aufrufen von Siri innerhalb der app Restaurant und bitten Sie *"Abrufen einer Wegbeschreibung..."* und Siri bietet Anweisungen für das Restaurant anzeigen.

Alle oben genannten Funktionen gemeinsam ist eine Sache, alle anzugeben, in dem der Vorschlag ursprünglich stammt. Im Fall der obigen Beispiel ist es die fiktiven Restaurant-app überprüfen.

iOS 10 wurde verbessert, um diese Funktion für eine app über mehrere kleine Änderungen und Ergänzungen zu den vorhandenen Frameworks zu aktivieren:

- `NSUserActivity` enthält zusätzliche Felder für die Erfassung von Speicherortinformationen, die innerhalb der app angezeigt wird.
- Mehrere Erweiterungen MapKit und CoreSpotlight wurden Ort zu erfassen.
- Siri, Zuordnungen, Tastaturen, Multitasking und andere apps innerhalb des Systems wurde bewusst speicherortfunktion hinzugefügt.

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

Klicken Sie dann ist die textbasierten Beschreibung des Speicherorts für textbasierten-Instanzen (z. B. die Tastatur QuickType) erforderlich:

```csharp
attributes.SubThoroughfare = "1";
attributes.Thoroughfare = "Infinite Loop";
attributes.City = "Cupertino";
attributes.StateOrProvince = "CA";
attributes.Country = "United States";
```

Den Breiten- und Längengrad sind optional, aber stellen Sie sicher, dass der Benutzer an die richtige Stelle weitergeleitet wird, die app so, dass es eingeschlossen werden soll, senden möchten:

```csharp
attributes.Latitude = 37.33072;
attributes.Longitude = 122.029674;
```

Durch Festlegen von Telefonnummern, kann die app Zugriff auf Siri erhalten, damit der Benutzer Siri aus der app aufrufen kann, indem sagen etwas wie *"Call folgendem"*:

```csharp
attributes.PhoneNumbers = new string[]{"(800) 275-2273"};
```

Schließlich kann die app darauf hinweisen, ob die Instanz für die Navigation und Telefonanrufe geeignet ist:

```csharp
attributes.SupportsPhoneCalls = true;
attributes.SupportsNavigation = true;
```

### <a name="implementing-contact-interactions"></a>Wenden Sie sich an Interaktionen implementieren

Neue sind in iOS 10 Kommunikations-apps tief in die Kontakte-app aus der Kontaktkarte integriert. Für apps, die wenden Sie sich an Interaktionen implementiert haben, kann der Benutzer die angegebene app Informationen an bestimmte Personen in ihrer Kontakte hinzufügen. Und wenn sie z. B. drücken Sie die schnelle Aktion-Schaltfläche am oberen Rand einer Karte zum Senden einer Nachricht, die angefügten-app, wird er als eine Option zum Senden der Nachricht aus.

Wenn a 3rd Party-app ausgewählt ist, können Sie gespeichert und als die Standardmethode für die angegebene Person, die das nächste Mal Nachricht, das möchte, dass der Benutzer an diesen Partner wenden, dargestellt werden.

Wenden Sie sich an Interaktionen werden implementiert, in der app mit `NSUserActivity` und das neue Intents-Framework, die unter iOS 10 eingeführt. Weitere Informationen zum Arbeiten mit Intent-Elemente finden Sie unserem [Grundlegendes zu SiriKit-Konzepten](~/ios/platform/sirikit/understanding-sirikit.md) und [Implementieren von SiriKit](~/ios/platform/sirikit/implementing-sirikit.md) Anleitungen.

#### <a name="donating-interactions"></a>Das Spenden Interaktionen

Sehen Sie sich wie die app Interaktionen Spenden kann:

[![](proactive-suggestions-images/activity04.png "Übersicht über das Spenden von Interaktionen")](proactive-suggestions-images/activity04.png#lightbox)

Die app erstellt ein `INInteraction` -Objekt, enthält ein **Absicht** (`INIntent`), **Teilnehmer** und **Metadaten**. Die **Absicht** stellt eine Benutzeraktion wie z. B. video Anruf oder eine Textnachricht senden. Die **Teilnehmer** enthalten die Personen, die die Kommunikation empfangen. Die **Metadaten** Informationen wie z. B. erfolgreichen Senden der Nachricht usw. definiert.

Der Entwickler erstellt nicht direkt eine Instanz des `INIntent` oder `INIntentResponse`, sie verwenden eine der bestimmten untergeordneten Klassen (basierend auf der Task wird die app im Auftrag des Benutzers ausführen), die von diesen übergeordneten Klassen erben. Z. B. `INSendMessageIntent` und `INSendMessageIntentResponse` zum Senden einer Textnachricht. 

Sobald die Interaktion vollständig gefüllt ist, rufen Sie die `DonateInteraction` Methode, um das System zu informieren, die die Interaktion mit verfügbar ist.

Wenn die app auf der Karte wenden Sie sich an der Benutzer interagiert, ruft die Interaktion mit gebündelt eine `NSUserActivity`, die wird dann verwendet, um die app zu starten:

[![](proactive-suggestions-images/activity05.png "Die Interaktion Ruft mit einem NSUserActivity gebündelt, die verwendet wird, um die app starten")](proactive-suggestions-images/activity05.png#lightbox)

Sehen Sie sich im folgenden Beispiel eine gesendet Nachrichtenabsicht:

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

Betrachten diesen Code im Detail, wird erstellt und füllt eine Instanz der `NSUserActivity` (siehe die [Erstellen einer Aktivität](#Creating-an-Activity) weiter oben). Als Nächstes erstellt er eine Instanz des `INSendMessageIntent` (erbt von `INIntent`) und füllt sie mit den Details der Nachricht gesendet werden:

```csharp
var intent = new INSendMessageIntent (to, text, "", "MonkeyChat", from);
```

Ein `INSendMessageIntentResponse` erstellt wird, und übergeben die `NSUserActivity` oben erstellt haben:

```csharp
var response = new INSendMessageIntentResponse (INSendMessageIntentResponseCode.Success, activity);
```

Ein `INInteraction` basiert auf dem Ziel-Nachricht zu senden (`INSendMessageIntent`) und die Antwort (`INSendMessageIntentResponse`) gerade erstellt haben:

```csharp
var interaction = new INInteraction (intent, response);
```

Schließlich ist das System die Benachrichtigung über die Interaktion:

```csharp
// Donate interaction to the system
interaction.DonateInteraction ((err) => {
    // Handle donation error
    ...
});
```

Ein Abschlusshandler übergeben, in denen die Spende erfolgreich war oder die app reagieren kann.

### <a name="activities-best-practices"></a>Bewährte Methoden von Aktivitäten

Apple empfiehlt die folgenden bewährten Methoden bei der Arbeit mit Aktivitäten:

- Verwendung `NeedsSave` für verzögerte Nutzlast-Updates.
- Achten Sie darauf, um einen starken Verweis auf die aktuelle Aktivität zu halten.
- Übertragen Sie nur kleine Nutzlasten, die gerade genug Informationen zum Wiederherstellen des Zustands enthalten.
- Sicherstellen Sie, dass der Typbezeichner der Aktivität eindeutigen und aussagekräftigen mit Reverse-DNS-Notation angeben. 

## <a name="schemaorg"></a>Schema.org

Wie oben gezeigt `NSUserActivity` kann das System aus, welche Informationen, die der Benutzer zurzeit mit auf dem Bildschirm arbeitet. Schema.org hinzugefügt Webseiten ähnliche Möglichkeiten.

Schema.org können dieselben Arten von Aktivitäten, die Position basierend auf der Website bereitstellen. Apple entwickelt, die neue Position Vorschläge genauso gut funktioniert in Safari angezeigt werden soll, wie in einer nativen app.

Einige Schema.org-Hintergrund:

- Es bietet einen Web öffnen Markup Vokabular-Standard.
- Dies funktioniert mit strukturierten Metadaten auf Webseiten.
- Es gibt mehr als 500 Schemas, die, die verschiedenen Konzepte zur Verfügung.
- Implementieren Sie sie auf der Website, kann der Entwickler abrufen zu den Vorteilen der Verwendung von `NSUserActivity` in einer nativen app.

Die Schemas werden in einer Struktur wie Struktur, in denen bestimmte wie Typen angeordnet *Restaurant*, erben von generischen Typen wie z. B. *Ort Geschäfte*. Weitere Informationen finden Sie unter [Schema.org](http://schema.org).

Angenommen, wenn die Webseite über die folgenden Daten enthalten:

```xml
<script type="application/ld+json>
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

Wenn der Benutzer diese Seite in Safari besucht, und klicken Sie dann in einer anderen app gewechselt, würde die Informationen zum Speicherort auf der Seite aufgezeichnet und in anderen Teilen des Systems (wie in der Aktivitäten, die oben dargestellt) als Speicherort Vorschlag angeboten werden.

Safari wird alles auf einer Webseite extrahiert, die die folgenden Eigenschaften für Schema entspricht:

- **PostalAddress**
- **Karten**
- Eine Telefon-Eigenschaft.

Weitere Informationen finden Sie unter unserem [Suche mit Webmarkup](~/ios/platform/search/web-markup.md) Guide.

## <a name="consuming-location-suggestions"></a>Nutzen die Speicherort-Vorschläge

In diesem nächsten Abschnitt behandelt, nutzen die Speicherort-Vorschlag, die aus anderen Teilen des Systems (z. B. der Karten-app) oder andere 3. apps von Drittanbietern stammen.

Es gibt zwei Hauptmethoden, die die app Speicherort Vorschläge nutzen kann:

- Über die Tastatur QuickType.
- Direkt, durch die Nutzung der Speicherortinformationen in routing-apps.

### <a name="location-suggestions-and-the-quicktype-keyboard"></a>Speicherort-Vorschläge und die Tastatur QuickType

Wenn die app mit den Adressen im textbasierten Format behandelt wird, kann die app Speicherort Vorschläge über die QuickType UI nutzen. iOS 10 wird die QuickType UI mit den folgenden Features erweitert:

- Die app kann Hinweise gibt, die semantische Bedeutung für Textfelder auf der Benutzeroberfläche hinzufügen.
- Proaktive Vorschläge kann von die app in der app abrufen.
- Verbesserte AutoKorrektur kann die app nutzen.

Die neue `TextContentType` Eigenschaft von Steuerelementen für das Feld unter iOS 10 ermöglicht dem Entwickler, die die semantische Bedeutung für den Wert fest, die der Benutzer in einem bestimmten Feld eingeben. Zum Beispiel:

```csharp
var textField = new UITextField();
textField.TextContentType = UITextContentType.FullStreetAddress;
```

Würde das System darauf hinzuweisen, dass die app erwartet, dass den Benutzer eine vollständige Adresse in das angegebene Feld eingeben. Dadurch wird die QuickType-Tastatur, um automatisch Vorschläge Speicherort auf der Tastatur, wenn der Benutzer in diesem Feld einen Wert eingibt.

Im folgenden sind einige der häufigeren für Entwickler in die `UITextContentType` statische Klasse:

* `Name`
* `GivenName`
* `FamilyName`
* `Location`
* `FullStreetAddress`
* `AddressCityAndState`
* `TelephoneNumber`
* `EmailAddress`

### <a name="routing-apps-and-locations-suggestions"></a>Routing-Apps und Speicherorte Vorschläge

In diesem Abschnitt dauert einen Blick auf die Speicherort-Vorschläge aus dem direkt innerhalb einer routing-app nutzen. Für die routing-app diese Funktionalität hinzufügen, wird der Entwickler nutzen, die vorhandene `MKDirectionsRequest` Framework wie folgt:

- Um die app im Multitasking höher zu stufen.
- Die app als eine routing-app zu registrieren.
- Behandeln, beim Starten der app mit einem MapKit `MKDirectionsRequest` Objekt.
- Grundlage Engagement der Benutzer auf iOS zu erfahren, wie Sie die app für den Benutzer zu geeigneten Zeitpunkten und vorschlagen gewähren.

Wenn die app gestartet wird, mit einem MapKit `MKDirectionsRequest` -Objekt, sollten sie automatisch gestartet, sodass die Benutzer-Anweisungen auf den angeforderten Pfad oder stellen eine Benutzeroberfläche, die für den Benutzer zu Anfordern einer Wegbeschreibung vereinfacht. Zum Beispiel:


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

Neue kann in iOS 10, die app eine Adresse gesendet werden, die nicht-geografische Koordinaten im, aufgrund derer der Entwickler die Adresse zu codieren muss:

```csharp
var geocoder = new CLGeocoder();
geocoder.GeocodeAddress(address, (place, err)=> {
    // Handle the display of the address
    
});

```

## <a name="media-app-suggestions"></a>Medien-App-Vorschläge

Wenn die app alle Arten von Medien wie eine Podcasts-app oder eine Streamingmedieninhalte wie z. B. Audio und Video – mit iOS 10, verarbeitet können sie Medien-Vorschläge im System nutzen.

Für apps, die Medien unterstützt iOS folgenden Verhalten:

- iOS fördert apps, die der Benutzer wahrscheinlich verwenden, wird basierend auf ihren vorherigen Verhalten.
- Vorschläge, die im Zusammenhang mit der app werden in Spotlight und in der Ansicht "heute" angezeigt.
- Wenn die app eine der folgenden Trigger erfüllt, kann es auf einen Vorschlag der Sperre-Bildschirm mit erweiterten Berechtigungen werden:
    - Nach dem Anschließen Kopfhörer oder einem Bluetooth-Gerät eine Verbindung herstellt.
    - Nach dem Abrufen von in einem Auto.
    - Nach dem Eintreffen von zu Hause, oder klicken Sie mit der Arbeit. 

Einen einfache API-Aufruf in iOS 10 einschließen, kann der Entwickler eine ansprechendere Erfahrung der Sperre-Bildschirm für die Benutzer der Media-app erstellen. Mithilfe der `MPPlayableContentManager` Klasse zum Verwalten der Wiedergabe von Medien, vollständige Medien-Steuerelemente (wie die von der Music-app angezeigt werden) wird auf dem Sperrbildschirm für die app angezeigt werden.


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

In diesem Artikel verfügt über proaktive Vorschläge beschrieben und gezeigt, wie die Entwickler sie Datenverkehr an die Xamarin.iOS-app verwenden kann. Es erläutert die Schritte, um proaktive Vorschläge zu implementieren und Nutzungsrichtlinien angezeigt.



## <a name="related-links"></a>Verwandte Links

- [iOS 10-Beispiele](https://developer.xamarin.com/samples/ios/iOS10/)
- [SiriKit-Programmierhandbuch](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/index.html)
