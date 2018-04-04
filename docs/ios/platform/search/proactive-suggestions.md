---
title: Einführung in die proaktive Vorschläge
description: In diesem Artikel wird gezeigt, wie proaktive Vorschläge in die app Xamarin.iOS Laufwerk Engagement verwendet werden, durch den Wechsel des Systems proaktiv hilfreiche Informationen automatisch für den Benutzer anzuzeigen.
ms.prod: xamarin
ms.assetid: 8DDD084A-0D1E-4DF7-B686-6309DCEFF5D3
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 5b06dbf0e8e108616adb4f77910267aaa1ac71f4
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="introduction-to-proactive-suggestions"></a>Einführung in die proaktive Vorschläge

_In diesem Artikel wird gezeigt, wie proaktive Vorschläge in die app Xamarin.iOS Laufwerk Engagement verwendet werden, durch den Wechsel des Systems proaktiv hilfreiche Informationen automatisch für den Benutzer anzuzeigen._

Neue für iOS-10, proaktive Vorschläge vorhanden News Möglichkeiten für Benutzer von proaktiv vorhanden hilfreiche Informationen zur richtigen Zeit jeweils automatisch an den Benutzer mit einem Xamarin.iOS-app in Verbindung setzen.

iOS 10 präsentiert neue Methoden für die steuernde Engagement an die app nach Wechsel des Systems proaktiv hilfreiche Informationen automatisch für den Benutzer zur richtigen Zeit jeweils anzuzeigen. Ebenso wie iOS 9 die Möglichkeit zum Hinzufügen von umfassenden Suche der App mit Spotlight, Übergabe und Siri Vorschläge bereitgestellt (finden Sie unter [neue Such-APIs](~/ios/platform/search/index.md)), mit iOS 10 eine app verfügbar machen Funktionen, die für dem Benutzer durch das System innerhalb angezeigt werden kann die folgenden Speicherorten:

- Der App Switcher
- Dem Sperrbildschirm
- CarPlay
- Karten
- Siri Interaktionen
- QuickType-Vorschläge

Die app verfügbar macht, diese Funktion in das System für eine Sammlung von Technologien wie z. B. über `NSUserActivity`, Web-Markup, Core Spotlight, MapKit und UIKit Media Player. Darüber hinaus wird durch proaktive Vorschlag-Unterstützung für die app, eine umfassendere Integration der Siri kostenlos.

## <a name="location-based-suggestions"></a>Standortbasierte Vorschläge

Neue für iOS-10, die `NSUserActivity` Klasse enthält eine `MapItem` Clustereigenschaft, mit dem Entwickler, die Informationen zum Speicherort bereitstellen, die in anderen Kontexten verwendet werden kann. Beispielsweise, wenn die app Restaurantkritiken angezeigt wird, können Sie festlegen der `MapItem` Eigenschaft, um den Speicherort des Restaurants, die der Benutzer in der app angezeigt wird. Wenn der Benutzer der Maps-App gewechselt wird, ist das Restaurant Speicherort automatisch verfügbar.

Wenn die app App-Suche unterstützt, können sie die neue Adresse Bestandteile der `CSSearchableItemAttributesSet` Klasse, um Speicherorte anzugeben, die die Benutzer besuchen können. Durch Festlegen der `MapItem` -Eigenschaft, die anderen Eigenschaften sind, automatisch ausgefüllt.

Zusätzlich zu den `Latitude` und `Longitude` von die Adresseigenschaften für die Komponente, wird empfohlen, dass die app Bereitstellen der `NamedLocation` und `PhoneNumbers` Eigenschaften zu, sodass Siri einen Aufruf an den Speicherort initiieren kann.

## <a name="web-markup-based-suggestions"></a>Web-Markup basierende Vorschläge

iOS 9 hinzugefügt, um strukturierte Daten Markup in der Website zu verwenden, die den Inhalt zu verbessern, die Benutzern im Spotlight und Safari Suchergebnisse angezeigt (finden Sie unter [Suche mit Web Markup](~/ios/platform/search/web-markup.md)). iOS 10 fügt die Möglichkeit, speicherortbasierte Markup enthalten (z. B. [PostalAddress](http://schema.org/PostalAddress) gemäß der Definition von [Schema.org](http://schema.org/)), um die benutzerfreundlichkeit weiter zu verbessern. Z. B. wenn ein Benutzer Ansichten einen Speicherort Seite auf der Website markiert ist, kann das System am gleichen Speicherort vorschlagen beim Öffnen von Zuordnungen.

## <a name="text-based-suggestions"></a>Vorschläge für textbasierte

UIKit in iOS 10 eingeschlossen wurde erweitert die [TextContentType](https://developer.apple.com/reference/uikit/uitextinputtraits/1649656-textcontenttype) Eigenschaft von der [UITextInputTraits](https://developer.apple.com/reference/uikit/uitextinputtraits) Klasse, um die semantische Bedeutung für den Inhalt in einem Textbereich anzugeben. Mit diesen Informationen vorhanden sind können in der Regel automatisch die entsprechende Tastenkombination auswählen, AutoKorrektur Vorschläge verbessern und proaktiv Informationen aus anderen apps und Websites integrieren.

Wenn der Benutzer Text in ein Feld "Text" markiert eintritt, z. B. `UITextContentType.FullStreetAddress`, das System kann das Feld mit dem Speicherort der Benutzer wurde vor kurzem anzeigen automatisch ausfüllen vorschlagen.

## <a name="media-based-suggestions"></a>Medien basierende Vorschläge

Wenn die app mithilfe von Medien spielt die [MPPlayableContentManager](https://developer.xamarin.com/api/type/MediaPlayer.MPPlayableContentManager/) API iOS 10 ermöglicht Benutzern das Anzeigen der Album Kunst und Wiedergeben von Medien über die app auf dem Sperrbildschirm.

## <a name="contextual-siri-reminders"></a>Kontextabhängige Siri Erinnerungen

Ermöglicht den Benutzer Siri verwenden, um schnell eine Erinnerung, um den Inhalt anzuzeigen, dass sie derzeit in der app zu einem späteren Zeitpunkt anzeigen. Z. B. wenn eine Restaurantkritik in der app angezeigten waren, sie können aufrufen Siri und sagen *"Erinnern dazu an, wenn ich Startseite gelangen."* Siri würde die Erinnerung durch einen Link zur Überprüfung in der app generieren.

## <a name="contact-based-suggestions"></a>Wenden Sie sich an basierende Vorschläge

Ermöglicht der app Kontakte (und zugehörige Kontaktinformationen) in angezeigt werden die **wenden Sie sich an** app auf dem iOS-Gerät zusammen mit allen Benutzern Kontakte im System gespeichert.

## <a name="ride-sharing-based-suggestions"></a>Außer Kraft setzen und Freigabe je Vorschläge

Wenn eine fuhr-Freigabe-app verwendet die [MKDirectionsRequest](https://developer.xamarin.com/api/type/MapKit.MKDirectionsRequest/) -API, iOS 10 bietet es als Option in der app-Switcher vorkommen, wenn der Benutzer eine fuhr möchten wahrscheinlich ist. Die app muss auch als fuhr-Freigabe-app registriert werden, durch Angeben der `MKDirectionsModeRideShare` für die [MKDirectionsApplicationSupportedModes](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Articles/iPhoneOSKeys.html) -Schlüssel in seiner `Info.plist` Datei.

Wenn die app nur bei Stromausfällen unterstützt, würde der Vorschlag System beginnen, mit *"Eine fuhr auf Get..."*, wenn andere Typen von routing Richtung (z. B. Walking oder Fahrrad) unterstützt werden, wird das System verwenden *"Anweisungen zum Abrufen..."*

> [!IMPORTANT]
> Die [MKMapItem](https://developer.xamarin.com/api/type/MapKit.MKMapItem/) Objekt, das die Anwendung empfängt möglicherweise keinen Längen- und Breitengrad Informationen und geocodierung benötigen.

## <a name="implementing-proactive-suggestions"></a>Proaktive Vorschläge

Proaktive Vorschlag hinzufügen ist-Support, um eine app Xamarin.iOS in der Regel so einfach wie einige APIs implementieren oder Erweitern auf einige APIs, die die app bereits implementiert werden kann.

Proaktive Vorschläge arbeiten mit den apps in drei verschiedene Arten:

- **`NSUserActivity` und Schema.org**  -  `NSUserActivity` hilft das System, welche Informationen zu verstehen, der Benutzer aktuell mit auf dem Bildschirm arbeitet. Schema.org fügt ähnlich wie die Zugehörigkeit zu Webseiten hinzu.
- **Speicherort Vorschläge** – Wenn die app bietet oder basierend Standortinformationen, diese neuen Möglichkeiten für API-Erweiterung Angebot nutzt auf diese Informationen in apps gemeinsam nutzen.
- **Media-App Vorschläge** – das System kann die app heraufstufen und die Medieninhalte auf Grundlage des Kontexts der Interaktion mit dem iOS-Gerät des Benutzers.

Und wird in der app unterstützt, durch Folgendes implementieren:

- **Übergabe**  -  `NSUserActivity` in iOS 8-Übergabe, zu unterstützen, die ermöglicht dem Entwickler, starten eine Aktivität auf einem Gerät, und klicken Sie dann auf einem anderen fortgesetzt hinzugefügt wurde (finden Sie unter [Einführung in die Übergabe](~/ios/platform/handoff.md)).
- **Spotlight-Suche** -iOS 9 hinzugefügt, die Möglichkeit, app-Inhalte innerhalb der Spotlight-Suche Ergebnisse mithilfe höher stufen `NSUserActivity` (finden Sie unter [Suche mit Core Spotlight](~/ios/platform/search/corespotlight.md)).
- **Kontextabhängige Siri Erinnerungen** – In iOS 10 `NSUserActivity` wurde erweitert, um schnell eine Erinnerung um den Inhalt anzuzeigen, die der Benutzer ist gerade in der app anzeigt, zu einem späteren Zeitpunkt stellen Siri zulassen.
- **Speicherort Vorschläge** -iOS 10 verbessert `NSUserActivity` zum Erfassen von Standorten innerhalb der app angezeigt, und Stufen sie an vielen Stellen im gesamten System.
- **Anforderungen für kontextabhängige Siri**  -  `NSUserActivity` bietet Kontext, um die Informationen, die Siri innerhalb der app angezeigt, damit der Benutzer Anweisungen oder ein Aufruf direkt aufrufen Siri aus innerhalb der app abrufen kann.
- **Wenden Sie sich an Interaktionen** – neu in iOS 10 `NSUserActivity` Communications-apps, die von einer Kontakt-Karte (in der Kontakte-app) als ein alternative Kommunikationsmethode höher gestuft werden können.

Alle diese Funktionen haben eines gemeinsam, verwenden jedoch alle `NSUserActivity` in einem Formular oder einer anderen, ihre Funktionalität bereitzustellen. 

## <a name="nsuseractivity"></a>NSUserActivity

Wie bereits erwähnt, `NSUserActivity` hilft das System, welche Informationen zu verstehen, der Benutzer aktuell mit auf dem Bildschirm arbeitet. `NSUserActivity` ist ein Lightweight-Status-caching-Mechanismus, um die Aktivität des Benutzers zu erfassen, während sie durch die app navigieren. Betrachten z. B. einem Restaurant-app ein:

[![](proactive-suggestions-images/activity02.png "Der NSUserActivity Lightweight-Status-caching-Mechanismus")](proactive-suggestions-images/activity02.png#lightbox)

Mit der folgenden Aktivitäten:

1. Wie der Benutzer mit der app funktioniert eine `NSUserActivity` wird erstellt, um den Status der app später neu zu erstellen.
2. Wenn der Benutzer ein Restaurant sucht, folgt demselben Muster zum Erstellen von Aktivitäten.
3. Und erneut, wenn der Benutzer zeigt ein Ergebnis. In diesem Fall einen Ort anzeigen und in iOS-10, das System mehr bestimmte Konzepte (z. B. Speicherort oder die Kommunikation Interaktionen) bekannt ist.

Führen Sie eine genauere Betrachtung der letzten Seite:

[![](proactive-suggestions-images/activity03.png "Die NSUserActivity-details")](proactive-suggestions-images/activity03.png#lightbox)

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

Entwickler müssen sicherstellen, dies ist der gleiche Typ Aktivitätsbezeichner (`com.xamarin.platform`) als die oben erstellte Aktivität. Die app verwendet den Informationen in den `NSUserActivity` zum Wiederherstellen des Zustands an, in denen der Benutzer wurde unterbrochen.

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
2. Wenn der Benutzer von der app mithilfe der Multitasking app Switcher weg bewegt, wird das System automatisch einen Vorschlag (am unteren Rand des Bildschirms) angezeigt, um Richtungen an das Restaurant mit ihren bevorzugten Navigations-app abrufen.
3. Wenn der Benutzer in den Nachrichten-app wechselt und startet eingeben *"Treffen wir uns"*, die Tastatur QuickType schlägt automatisch einfügen in die Adresse des Restaurants.
4. Wenn der Benutzer der Maps-App gewechselt wird, wird das Restaurant-Adresse als Ziel automatisch vorgeschlagen.
5. Dies funktioniert sogar für 3. apps von Drittanbietern (diese Unterstützung `NSUserActivity`), sodass der Benutzer zu einer fuhr-Freigabe-app wechseln kann und das Restaurant Adresse wird automatisch als auch als Ziel es vorgeschlagen.
6. Es bietet darüber hinaus Kontext auf Siri, damit der Benutzer Siri innerhalb der app Restaurant aufrufen kann, und bitten Sie *"Get Richtungen..."* und Siri gebe Richtungen an das Restaurant anzeigen.

Alle oben genannten Funktionen gemeinsam hat dabei, alle anzugeben, wo der Vorschlag ursprünglich von stammt. Bei dem obigen Beispiel ist es der fiktiven Restaurant-app überprüfen.

iOS 10 wurde verbessert, um die Funktionalität für eine app über mehrere kleinere Änderungen und Ergänzungen für vorhandene Frameworks aktivieren:

- `NSUserActivity` verfügt über zusätzliche Felder für das Erfassen von Informationen zum Speicherort, die innerhalb der app angezeigt wird.
- Mehrere Erweiterungen MapKit und CoreSpotlight wurden um Speicherort zu erfassen.
- Beachten Sie Speicherort-Funktionalität wurde um Siri, Karten, Tastaturen, Multitasking und anderen apps innerhalb des Systems hinzugefügt.

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

Klicken Sie dann, ist die textbasierte Beschreibung des Speicherorts für textbasierte Instanzen (z. B. die Tastatur QuickType) erforderlich:

```csharp
attributes.SubThoroughfare = "1";
attributes.Thoroughfare = "Infinite Loop";
attributes.City = "Cupertino";
attributes.StateOrProvince = "CA";
attributes.Country = "United States";
```

Breiten- und Längengrade sind optional, aber stellen Sie sicher, dass der Benutzer den genauen Speicherort weitergeleitet wird, senden die app endversatz ist, muss enthalten sein:

```csharp
attributes.Latitude = 37.33072;
attributes.Longitude = 122.029674;
```

Durch Festlegen der Telefonnummern an, kann die app Zugriff auf Siri erhalten, damit der Benutzer Siri aus der app aufrufen kann, indem Sie darauf hingewiesen werden etwa, *"Rufen Sie diesen Ort"*:

```csharp
attributes.PhoneNumbers = new string[]{"(800) 275-2273"};
```

Schließlich kann die app darauf hinweisen, ob die Instanz für die Navigation und Anrufe geeignet ist:

```csharp
attributes.SupportsPhoneCalls = true;
attributes.SupportsNavigation = true;
```

### <a name="implementing-contact-interactions"></a>Wenden Sie sich an Aktivitäten implementieren

Neu sind in iOS-10, Kommunikation apps direkt in die Kontakte-app aus integriert. Für apps, die Kontakt-Interaktionen implementiert haben, kann die Benutzer der angegebenen app-Informationen an bestimmte Personen in ihrer Kontakte hinzufügen. Und wenn angenommen, sie die schnellaktions-Schaltfläche am oberen Rand einer Karte zum Senden einer Nachricht auftritt, wird die angefügte app als eine Option zum Senden der Nachricht aus aufgeführt.

Wenn a 3rd Party app aktiviert ist, können Sie gespeichert und als die angegebene Person auf das nächste Mal Nachricht, das möchte, dass der Benutzer sie wenden Sie sich an, die Standardmethode dargestellt werden.

Wenden Sie sich an Aktivitäten werden in der Anwendung mit implementiert `NSUserActivity` und das neue Intents-Framework in iOS 10 eingeführt. Weitere Informationen zum Arbeiten mit Intents finden Sie unter unsere [Grundlegendes zu SiriKit Konzepten](~/ios/platform/sirikit/understanding-sirikit.md) und [SiriKit implementieren](~/ios/platform/sirikit/implementing-sirikit.md) Handbüchern.

#### <a name="donating-interactions"></a>Spenden Interaktionen

Sehen Sie sich wie die app Interaktionen threadingdienst kann:

[![](proactive-suggestions-images/activity04.png "Übersicht über die Interaktionen Spenden")](proactive-suggestions-images/activity04.png#lightbox)

Die app erstellt eine `INInteraction` -Objekt, enthält ein **Absicht** (`INIntent`), **Teilnehmer** und **Metadaten**. Die **Absicht** stellt eine Benutzeraktion, z. B. ein video Aufruf oder eine Textnachricht zu senden. Die **Teilnehmer** enthalten die Personen, die die Kommunikation zu empfangen. Die **Metadaten** zusätzliche Informationen wie z. B. erfolgreichen Senden der Nachricht usw. definiert.

Der Entwickler erstellt nie direkt eine Instanz des `INIntent` oder `INIntentResponse`, sie verwenden eine der bestimmten untergeordneten Klassen (basierend auf den Task, der die app wird im Auftrag des Benutzers zum Ausführen), die von diesen übergeordneten Klassen erben. Beispielsweise `INSendMessageIntent` und `INSendMessageIntentResponse` für eine SMS gesendet. 

Nach dem die Aktivität vollständig aufgefüllt ist, rufen Sie die `DonateInteraction` Methode, um das System zu informieren, die die Interaktion mit verfügbar ist.

Wenn der Benutzer mit der app aus der Karte Kontakt interagiert, ruft die Interaktion mit gebündelt eine `NSUserActivity`, dieser wird dann verwendet, um die app starten:

[![](proactive-suggestions-images/activity05.png "Die Interaktion Ruft mit einem NSUserActivity gebündelt, die verwendet wird, um die app starten")](proactive-suggestions-images/activity05.png#lightbox)

Betrachten Sie das folgende Beispiel für eine gesendet Nachrichtenabsicht:

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

Betrachten diesen Code im Detail, erstellt und füllt eine Instanz der `NSUserActivity` (entsprechend der [Erstellen einer Aktivität](#Creating-an-Activity) obigen Abschnitt ""). Anschließend erstellt er eine Instanz des `INSendMessageIntent` (geerbt von `INIntent`) und füllt sie mit den Details der Nachricht gesendet werden:

```csharp
var intent = new INSendMessageIntent (to, text, "", "MonkeyChat", from);
```

Ein `INSendMessageIntentResponse` erstellt und übergeben der `NSUserActivity` oben erstellten:

```csharp
var response = new INSendMessageIntentResponse (INSendMessageIntentResponseCode.Success, activity);
```

Ein `INInteraction` wird aus der Nachricht senden Absicht erstellt (`INSendMessageIntent`) und die Antwort (`INSendMessageIntentResponse`) gerade erstellt haben:

```csharp
var interaction = new INInteraction (intent, response);
```

Schließlich wird das System Benachrichtigung über die Interaktion:

```csharp
// Donate interaction to the system
interaction.DonateInteraction ((err) => {
    // Handle donation error
    ...
});
```

Eine Abschlusshandler übergeben wird, in dem die app auf die Spende Erfolg oder Fehlschlagen reagieren kann.

### <a name="activities-best-practices"></a>Bewährte Methoden von Aktivitäten

Apple empfiehlt die folgenden bewährten Methoden bei der Arbeit mit Aktivitäten:

- Verwendung `NeedsSave` für verzögerte Nutzlast Updates.
- Stellen Sie sicher, um einen starken Verweis auf die aktuelle Aktivität zu halten.
- Übertragen Sie nur kleine Nutzlasten, die gerade ausreicht, Informationen zum Wiederherstellen des Zustands enthalten.
- Sicherstellen Sie, dass die Aktivität-Datentypbezeichner eindeutigen und aussagekräftigen mithilfe von Reverse-DNS-Notation, anzugeben. 

## <a name="schemaorg"></a>Schema.org

Wie oben gezeigt `NSUserActivity` hilft das System, welche Informationen zu verstehen, der Benutzer aktuell mit auf dem Bildschirm arbeitet. Schema.org fügt ähnlich wie die Zugehörigkeit zu Webseiten hinzu.

Schema.org bieten die gleichen Typen von Interaktionen an Position basierend auf der Website. Apple entwickelt Vorschläge für die neue Position genauso gut funktioniert, bei der Anzeige in Safari, wie in eine systemeigene app.

Einige Schema.org-Hintergrund:

- Er bietet einen Web öffnen Markup Vokabular-Standard.
- Es funktioniert, indem einschließlich strukturierten Metadaten auf Webseiten.
- Es gibt mehr als 500 Schemas, die verschiedene Konzepte verfügbar darstellt.
- Durch die Implementierung auf der Website, kann der Entwickler abrufen zu den Vorteilen der Verwendung von `NSUserActivity` in eine systemeigene app.

Die Schemas werden in einer Struktur wie die Struktur, in denen bestimmte wie Typen angeordnet *Restaurant*, erben Sie die mehr generische Typen z. B. *lokalen Business*. Weitere Informationen finden Sie unter [Schema.org](http://schema.org).

Angenommen, wenn die Webseite die folgenden Daten enthalten:

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

Wenn der Benutzer Safari auf diese Seite besucht, und klicken Sie dann auf eine andere app gewechselt, würde die Informationen zum Dateispeicherort aus der Seite aufgezeichnet und in anderen Teilen des Systems (wie in der oben genannten Aktivitäten dargestellt) als einen Vorschlag Speicherort angeboten werden.

Safari wird alles auf einer Webseite extrahieren, die die folgenden Eigenschaften für Schema entspricht:

- **PostalAddress**
- **GeoCoordinates**
- Eine Telefon-Eigenschaft.

Weitere Informationen finden Sie unter unsere [Suche mit Web Markup](~/ios/platform/search/web-markup.md) Handbuch.

## <a name="consuming-location-suggestions"></a>Nutzen Speicherort-Vorschläge

In diesem Abschnitt weiter befasst sich mit Nutzung Speicherort Vorschlag, die von anderen Teilen des Systems (z. B. die Maps-app) oder andere 3rd Party apps kommen.

Es gibt zwei Hauptmethoden, die die app Speicherort Vorschläge nutzen kann:

- Über die Tastatur QuickType.
- Direkt durch die Nutzung von Informationen zum Speicherort in routing-apps.

### <a name="location-suggestions-and-the-quicktype-keyboard"></a>Speicherort Vorschläge und die Tastatur QuickType

Wenn die app mit Adressen in textbasierte Formate behandelt wird, kann die app Speicherort Vorschläge über die QuickType UI nutzen. iOS 10 wird die QuickType UI mit den folgenden Features erweitert:

- Die app kann Hinweise zu den semantischen Zweck für Textfelder auf der Benutzeroberfläche hinzufügen.
- Die app kann proaktive Vorschläge in der app zu erhalten.
- Verbesserte AutoKorrektur kann die app profitieren.

Die neue `TextContentType` Eigenschaft der Text-Feld-Steuerelemente in iOS 10 ermöglicht dem Entwickler, die die semantische Absicht für den Wert fest, die der Benutzer in einem bestimmten Feld eingeben. Zum Beispiel:

```csharp
var textField = new UITextField();
textField.TextContentType = UITextContentType.FullStreetAddress;
```

Dem System würde Teilen Sie, dass die app erwartet, dass den Benutzer eine vollständige Adresse im angegebenen Feld eingeben. Dadurch können die Tastatur QuickType Speicherort Vorschläge auf der Tastatur automatisch bereitstellen, wenn der Benutzer einen Wert in dieses Feld eingeben, wird.

Im folgenden werden einige der häufigeren verfügbar, für den Entwickler in die `UITextContentType` statische Klasse:

* `Name`
* `GivenName`
* `FamilyName`
* `Location`
* `FullStreetAddress`
* `AddressCityAndState`
* `TelephoneNumber`
* `EmailAddress`

### <a name="routing-apps-and-locations-suggestions"></a>Routing-Apps und Speicherorte-Vorschläge

In diesem Abschnitt wird ein Blick auf Speicherort Vorschläge aus dem direkt innerhalb einer routing-app nutzen. Für das routing app diese Funktionalität hinzufügen, wird der Entwickler nutzen, die vorhandene `MKDirectionsRequest` Framework wie folgt:

- Um die app im Multitasking höher zu stufen.
- Zum Registrieren der app als routing-app.
- Starten der Anwendung mit einem MapKit behandeln `MKDirectionsRequest` Objekt.
- Auf iOS zu erfahren, wie die app dem Benutzer zu geeigneten Zeitpunkten und vorschlagen gewähren basierend auf Benutzer.

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

Neue kann in iOS 10, die app eine Adresse gesendet werden, die nicht Geo-Koordinaten in, aufgrund derer der Entwickler die Adresse codieren muss:

```csharp
var geocoder = new CLGeocoder();
geocoder.GeocodeAddress(address, (place, err)=> {
    // Handle the display of the address
    
});

```

## <a name="media-app-suggestions"></a>Vorschläge für Media-App

Wenn die app einen beliebigen Typ von Medien wie eine app Podcast oder eine Streamingmedieninhalte z. B. Audio oder Video mit iOS-10, behandelt können sie Medien Vorschläge im System nutzen.

Für apps, die Medien verarbeiten, unterstützt iOS folgenden Verhaltensweisen:

- iOS stuft apps, die der Benutzer wahrscheinlich ist, basierend auf ihren vorherigen Verhalten.
- Vorschläge im Zusammenhang mit der app werden im Spotlight und die Ansicht "heute" angezeigt.
- Wenn die app eine der folgende Trigger erfüllt, kann es auf eine Sperre Bildschirm Vorschlag hochgestuft werden:
    - Nachdem eine Verbindung unter Verwendung der Kopfhörer oder einem Bluetooth-Gerät hergestellt hat.
    - Nach Eingang eines Auto.
    - Nach dem zu Hause ankommen, oder klicken Sie mit der Arbeit. 

Einen einfache API-Aufruf in iOS 10 einschließen, kann der Entwickler eine mehr einbeziehen Lock-Bildschirm-Erfahrung für Benutzer von der Media-app erstellen. Mithilfe der `MPPlayableContentManager` Klasse zum Verwalten der Wiedergabe von Medien, vollständige Medien-Steuerelemente (z. B. die von der app Musik dargestellt) wird auf dem Sperrbildschirm für die app angezeigt werden.


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

Dieser Artikel hat abgedeckt proaktive Vorschläge und wurde gezeigt, wie der Entwickler diese Laufwerk-Datenverkehr an die Xamarin.iOS-app verwenden kann. Er behandelt die Schritte, um die proaktive Vorschläge implementieren und Verwendungsrichtlinien dargestellt.



## <a name="related-links"></a>Verwandte Links

- [iOS-10-Beispiele](https://developer.xamarin.com/samples/ios/iOS10/)
- [Programmierhandbuch SiriKit](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/index.html)
