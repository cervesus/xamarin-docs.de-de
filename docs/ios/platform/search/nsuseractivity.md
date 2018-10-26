---
title: Suche mit NSUserActivity in Xamarin.iOS
description: In diesem Dokument wird beschrieben, wie ein NSUserActivity, und sie durchsuchbar in Spotlight und Safari indiziert wird. Es wird erläutert, wie auf die Auswahl einer NSUserActivity in den Suchergebnissen zu reagieren.
ms.prod: xamarin
ms.assetid: 0B28B284-C7C9-4C0D-A782-D471FBBC4CAE
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/20/2017
ms.openlocfilehash: acfff90b4b983f92718bb9af1f587a73ec0f8da7
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50104257"
---
# <a name="search-with-nsuseractivity-in-xamarinios"></a>Suche mit NSUserActivity in Xamarin.iOS

`NSUserActivity` iOS 8 eingeführtes wurde, und wird verwendet, um die Übergabe die Daten bereit.
Sie können zum Erstellen von Aktivitäten in bestimmten Teilen der app, die dann in eine andere Instanz Ihrer App auf ein iOS-Gerät aus übergeben werden kann. Der empfangende Gerät kann weiterhin die Aktivität gestartet wird, auf das vorherige Gerät, und wählen oben rechts, wo der Benutzer unterbrochen. Weitere Informationen zur Verwendung von Übergabeanimationen finden Sie unserem [Einführung in die Übergabe](~/ios/platform/handoff.md) Dokumentation.

Noch nicht mit iOS 9, `NSUserActivity` indiziert (öffentlich und privat) und über die Spotlight-Suche und Safari durchsucht werden können. Durch das Markieren einer `NSUserActivity` als durchsuchbar und Hinzufügen von indizierbaren Metadaten kann die Aktivität in den Suchergebnissen auf das iOS-Gerät aufgeführt werden.

[![](nsuseractivity-images/apphistory01.png "Übersicht über die App-Verlauf")](nsuseractivity-images/apphistory01.png#lightbox)

Wenn der Benutzer auf ein Suchergebnis, zu dem eine Aktivität aus Ihrer app gehört auswählt, die app wird gestartet, und die Aktivität beschrieben wird, indem die `NSUserActivity` neu gestartet und dem Benutzer angezeigt wird.

Die folgenden Eigenschaften der `NSUserActivity` dienen zur Unterstützung von App-Suche:

 - `EligibleForHandoff` – If `true`, diese Aktivität kann in einem Vorgang für die Übergabe verwendet werden.
 - `EligibleForSearch` – If `true`, diese Aktivität wird auf den Index auf dem Gerät hinzugefügt und in den Suchergebnissen angezeigt.
 - `EligibleForPublicIndexing` – If `true`, diese Aktivität wird von Apple cloudbasierte Index hinzugefügt und angezeigt, die für Benutzer (über die Suche), die nicht bereits Ihre app auf ihrem iOS-Gerät installiert haben. Finden Sie unter den [öffentliche Suchindizierung](#Public-Search-Indexing) Informationen weiter unten im Abschnitt.
 - `Title` – Stellt einen Titel für die Aktivität bereit und wird in den Suchergebnissen angezeigt. Benutzer können auch für den Text des Titels selbst suchen.
 - `Keywords` – Ist ein Array von Zeichenfolgen verwendet, um Ihre Aktivität zu beschreiben, die indiziert und durchsucht werden vorgenommen, durch den Endbenutzer.
 - `ContentAttributeSet` – Ist eine `CSSearchableItemAttributeSet` verwendet, um weitere Ihrer Aktivität ausführlich zu beschreiben, und geben Sie umfangreiche Inhalte in den Suchergebnissen angezeigt.
 - `ExpirationDate` – Wenn Sie eine Aktivität, die nur zu einem bestimmten Datum angezeigt werden, Sie möchten, können Sie dieses Datum hier bereitstellen.
 - `WebpageURL` – Wenn die Aktivität im Web angezeigt werden kann, oder wenn Ihre app mit deep-Links von Safari unterstützt, können Sie den Link, um hier finden Sie auf festlegen.

## <a name="nsuseractivity-quickstart"></a>NSUserActivity-Schnellstart

Befolgen Sie diese Anweisungen zum Implementieren von einem durchsuchbaren `NSUserActivity` in Ihrer app:

- [Aktivität-Typ-IDs erstellen](#creatingtypeid)
- [Erstellen einer Aktivität](#createactivity)
- [Reagieren auf eine Aktivität](#respondactivity)
- [Öffentliche Search-Indizierung](#indexing)

Es gibt einige [zusätzliche Vorteile](#benefits) mit `NSUserActivity` auf Ihre Inhalte durchsuchbar zu machen.

<a name="creatingtypeid" />

## <a name="creating-activity-type-identifiers"></a>Aktivität-Typ-IDs erstellen

Bevor Sie eine Suche Aktivität erstellen können, müssen Sie erstellen eine _Typbezeichner der Aktivität_ identifizieren. Typ des Bezeichners der Aktivität ist eine kurze Zeichenfolge hinzugefügt, die `NSUserActivityTypes` Array von der app **"Info.plist"** Datei, die zur eindeutigen Identifizierung eines bestimmten Typs des Benutzer-Aktivität verwendet. Es werden ein Eintrag im Array für jede Aktivität, die die app unterstützt, und App-Suche verfügbar macht. 

Apple empfiehlt eine Notation Reverse-DNS-Format für den Typ des Bezeichners der Aktivität verwenden, um Konflikte zu vermeiden. Zum Beispiel: `com.company-name.appname.activity` für bestimmte app-basierte Aktivitäten oder `com.company-name.activity` für Aktivitäten, die für mehrere apps ausgeführt werden können.

Typ des Bezeichners der Aktivität wird verwendet, beim Erstellen einer `NSUserActivity` Instanz, um den Typ der Aktivität zu identifizieren. Wenn eine Aktivität, die als Ergebnis des Benutzers durch Tippen auf ein Suchergebnis fortgesetzt wird, bestimmt der Aktivitätstyp (zusammen mit der app-Team-ID) der app zu starten, um die Aktivität den Vorgang fortzusetzen.

Um die erforderliche Aktivität Typ-IDs zur Unterstützung dieses Verhaltens zu erstellen, bearbeiten die **"Info.plist"** Datei, und wechseln Sie zu der **Quelle** anzeigen. Hinzufügen einer `NSUserActivityTypes` Schlüssel, und erstellen Sie Bezeichner im folgenden Format:

[![](nsuseractivity-images/type01.png "Die NSUserActivityTypes Schlüssel und die erforderlichen-IDs in der Plist-editor")](nsuseractivity-images/type01.png#lightbox)

Im obigen Beispiel, die wir erstellt eine neue Aktivität Typ-ID für die Search-Aktivität (`com.xamarin.platform`). Wenn Sie Ihre eigenen apps zu erstellen, ersetzen Sie den Inhalt von der `NSUserActivityTypes` array mit der Aktivität-Typ-IDs für die Aktivitäten Ihrer app unterstützt.

<a name="createactivity" />

## <a name="creating-an-activity"></a>Erstellen einer Aktivität

Im folgenden finden ein Beispiel zum Erstellen einer Aktivität für eine Indexsuche auf dem Gerät gehostet:

```csharp
// Create App Search Activity
var activity = new NSUserActivity ("com.xamarin.platform");

// Define details
var info = new NSMutableDictionary ();
info.Add(new NSString("link"),new NSString("http://xamarin.com/platform"));

// Populate Activity
activity.Title = "The Xamarin Platform";
activity.UserInfo = info;

// Add App Search ability
activity.EligibleForSearch = true;
activity.BecomeCurrent();
```

Wir können Sie weitere Details hinzufügen durch Festlegen der `ContentAttributeSet` Eigenschaft unserer `NSUserActivity` wie folgt:

[![](nsuseractivity-images/apphistory02.png "Übersicht über die Details der Suche hinzufügen")](nsuseractivity-images/apphistory02.png#lightbox)

Mithilfe einer `ContentAttributeSet` können Sie umfassende Suchergebnisse, die dem Benutzer interagieren mit ihnen zu verleiten erstellen.

<a name="respondactivity" />

## <a name="responding-to-an-activity"></a>Reagieren auf eine Aktivität

Für den Benutzer Tippen auf ein Suchergebnis reagieren (`NSUserActivity`) für unsere app Bearbeiten der **Datei "appdelegate.cs"** Datei, und überschreiben die `ContinueUserActivity` Methode. Zum Beispiel:

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

Beachten Sie, dass dies die gleichen methodenüberschreibung zum Reagieren auf Anforderungen von Übergabeanimationen verwendet wird. Jetzt klickt der Benutzer auf einen Link, über die app in den Ergebnissen der Spotlight-Suche, unsere app wird in den Vordergrund gesetzt (oder gestartet wurde, wenn nicht bereits ausgeführt werden) und die Inhalte, die Navigation oder die Funktion durch diesen Link dargestellt wird angezeigt:

[![](nsuseractivity-images/apphistory03.png "Wiederherstellen der vorherigen Zustand aus der Suche")](nsuseractivity-images/apphistory03.png#lightbox)

<a name="indexing" />

## <a name="public-search-indexing"></a>Öffentliche Search-Indizierung

Wie wir oben gesehen haben, erleichtert iOS 9 zu, den Suchzugriff auf app-Inhalte und Features, die der Benutzer bereits ermittelt und auf einem Gerät verwendet. Mit öffentlichen Indizierung iOS 9 bietet eine Möglichkeit für Benutzer, die noch nicht Inhalte oder Features noch ermittelt (oder die app, die nicht selbst installiert haben), die Ergebnisse in die Suche zu erhalten.

Öffentliche Indizierung funktioniert wie folgt:

1. Wenn Sie eine Aktivität für Ihre app erstellen, können Sie es als öffentlich markieren.
2. Öffentliche Aktivitäten an Apple gesendet und in der Cloud indiziert.
3. Wenn weitere Benutzer mit Ihrer app auf einem Gerät interagieren und verwenden Sie die bestimmte Funktion oder den Inhalt, die von der Aktivität dargestellt, steigt es Rang.
4. Gängige öffentliche Ergebnisse stehen an andere Benutzer, auch wenn sie die app nicht installiert haben.
5. Diese öffentliche Ergebnisse werden angezeigt in Spotlight-Suche und Safari (wenn die Aktivität eine URL enthält).

Wir können werden die privaten-Aktivität, der wir zuvor erstellt haben, und erweitern, um die öffentlich sein:

```csharp
// Create App Search Activity
var activity = new NSUserActivity ("com.xamarin.platform");

// Define details
var info = new NSMutableDictionary ();
info.Add(new NSString("link"),new NSString("http://xamarin.com/platform"));

// Populate Activity
activity.Title = "The Xamarin Platform";
activity.UserInfo = info;

// Add App Search ability
activity.EligibleForSearch = true;
activity.EligibleForPublicIndexing = true;
activity.BecomeCurrent();
```

Nur weil eine Aktivität festgelegt wurde, für die Indizierung durch Festlegen von öffentlichen `EligibleForPublicIndexing = true`, es bedeutet nicht, dass es automatisch von Apple öffentliche Cloud Index hinzugefügt werden. Zunächst müssen die folgenden Bedingungen erfüllt sein:

1. Es muss in den Suchergebnissen angezeigt werden und von vielen Benutzern ausgewählt werden. Die Ergebnisse bleiben private, bis ein Aktivität Engagement-Schwellenwert erreicht wurde.
2. App-Bereitstellung wird verhindert, dass alle benutzerspezifischen Daten indiziert und öffentlich gemacht.

<a name="benefits" />

## <a name="additional-benefits"></a>Zusätzliche Vorteile

Mit App-Suche über `NSUserActivity` in Ihrer app erhalten Sie auch die folgenden Funktionen:

- **Übergabe** – da App-Suche von Inhalt, Navigation Verfügbarmachen ist und/oder-Funktionen mit denselben Mechanismus wie Handoff (`NSUserActivity`), können Sie einfach die Benutzer Ihrer app zu starten, eine Aktivität auf einem Gerät auf einem anderen fortgesetzt.
- **Siri-Vorschläge** -zusammen mit der standard-Vorschläge, die von Siri-Vorschläge, normalerweise unterbreitet, aktive aus Ihrer app automatisch vorgeschlagen werden können.
- **Siri intelligente Erinnerungen** -Benutzer werden in der Lage, bitten Sie Siri, um sie zu Aktivitäten in Ihrer app erinnern.



## <a name="related-links"></a>Verwandte Links

- [iOS 9-Beispiele](https://developer.xamarin.com/samples/ios/iOS9/)
- [iOS 9 für Entwickler](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Programmierhandbuch für App-Suche](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/AppSearch/index.html#//apple_ref/doc/uid/TP40016308)
- [Blogbeitrag und Beispiel](https://blog.xamarin.com/improve-discoverability-with-search-in-ios-9/)
