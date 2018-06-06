---
title: Suche mit NSUserActivity in Xamarin.iOS
description: Dieses Dokument beschreibt, wie ein NSUserActivity, Spotlight und Safari durchsuchbar machen zu indizieren. Es wird erläutert, wie auf die Auswahl einer NSUserActivity in den Suchergebnissen angezeigter reagieren.
ms.prod: xamarin
ms.assetid: 0B28B284-C7C9-4C0D-A782-D471FBBC4CAE
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: 4b053f66e9b6b7715cbe52c4e43d9db32db48f4c
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34788207"
---
# <a name="search-with-nsuseractivity-in-xamarinios"></a>Suche mit NSUserActivity in Xamarin.iOS

`NSUserActivity` iOS 8 eingeführt wurde, und wird verwendet, um die Daten für die Übergabe bereitstellen.
Sie können Sie Aktivitäten in bestimmte Teile Ihrer app zu erstellen, die dann in eine andere Instanz Ihrer App ausführen auf einem anderen iOS-Gerät deaktiviert übergeben werden kann. Das empfangende Gerät können Sie die Aktivität gestartet wird, auf dem vorherigen Gerät Entnahme oben rechts, in denen der Benutzer aufgehört fortsetzen. Weitere Informationen finden Sie unter Übergabe, finden Sie unter unsere [Einführung in die Übergabe](~/ios/platform/handoff.md) Dokumentation.

Für iOS 9, neue `NSUserActivity` (öffentlich und privat) indiziert und vom Spotlight-Suche und Safari durchsucht werden können. Markieren Sie zunächst eine `NSUserActivity` als indizierbaren durchsuchbar und Hinzufügen von Metadaten kann die Aktivität in den Suchergebnissen auf das iOS-Gerät aufgelistet werden.

[![](nsuseractivity-images/apphistory01.png "Übersicht über die App-Verlauf")](nsuseractivity-images/apphistory01.png#lightbox)

Wenn der Benutzer ein Suchergebnis, die an eine Aktivität aus Ihrer app gehört auswählt, die app wird gestartet, und die Aktivität von beschrieben die `NSUserActivity` neu gestartet wird, und dem Benutzer angezeigt.

Die folgenden Eigenschaften der `NSUserActivity` werden verwendet, um App-Suche zu unterstützen:

 - `EligibleForHandoff` – If `true`, diese Aktivität kann in einem Vorgang Übergabe verwendet werden.
 - `EligibleForSearch` – If `true`, diese Aktivität wird der Index auf dem Gerät hinzugefügt und in den Suchergebnissen angezeigt.
 - `EligibleForPublicIndexing` – If `true`, diese Aktivität wird der Apple-Cloud-basierten Index hinzugefügt werden und so präsentiert, Benutzer (über die Suche), die nicht bereits die app auf seinem iOS-Gerät installiert haben. Finden Sie unter der [öffentliche Indizierung](#Public-Search-Indexing) weiter unten für weitere Details.
 - `Title` – Stellt einen Titel für die Aktivität, und in den Suchergebnissen angezeigt. Benutzer können auch für den Text des Titels selbst suchen.
 - `Keywords` – Ist ein Array von Zeichenfolgen verwendet, um Ihre Aktivitäten zu beschreiben, die indiziert und vom Endbenutzer durchsuchbaren vorgenommen werden.
 - `ContentAttributeSet` – Ist eine `CSSearchableItemAttributeSet` verwendet, um weitere Aktivität im Detail beschreiben, und geben Sie Inhalte in den Suchergebnissen angezeigt.
 - `ExpirationDate` – Wenn Sie eine Aktivität nur zu einem bestimmten Datum angezeigt werden, Sie möchten, können Sie dieses Datum hier bereitstellen.
 - `WebpageURL` – Wenn die Aktivität im Web angezeigt werden kann, oder wenn Ihre app deep-Links des Safari unterstützt, können Sie den Link besuchen. hier festlegen.

## <a name="nsuseractivity-quickstart"></a>NSUserActivity-Schnellstart

Führen Sie diese Anweisungen, um eine durchsuchbare implementieren `NSUserActivity` in Ihrer app:

- [Aktivität Typ-IDs erstellen](#creatingtypeid)
- [Erstellen einer Aktivität](#createactivity)
- [Reagieren auf eine Aktivität](#respondactivity)
- [Öffentliche Indizierung](#indexing)

Es gibt einige [zusätzliche Vorteile](#benefits) mit `NSUserActivity` auf Ihre Inhalte durchsuchbar zu machen.

<a name="creatingtypeid" />

## <a name="creating-activity-type-identifiers"></a>Aktivität Typ-IDs erstellen

Vor dem Erstellen einer Aktivität suchen können, müssen Sie erstellen eine _Typbezeichner der Aktivität_ dienen. Der Typbezeichner der Aktivität ist eine kurze Zeichenfolge hinzugefügt, die `NSUserActivityTypes` Array von der app **"Info.plist"** Datei, die zur eindeutigen Identifizierung einer bestimmten Aktivität Benutzertyp verwendet. Im Array für jede Aktivität, die die app unterstützt, und macht App suchen wird ein einziger Eintrag vorhanden sein. 

Apple empfiehlt eine Notation Reverse-DNS-Format für den Typ Aktivitätsbezeichner Konflikte zu vermeiden. Zum Beispiel: `com.company-name.appname.activity` für bestimmte app-basierte Aktivitäten oder `com.company-name.activity` für Aktivitäten, die in mehreren apps ausgeführt werden können.

Der Typbezeichner der Aktivität wird verwendet, für die Erstellung einer `NSUserActivity` Instanz, um den Typ der Aktivität zu identifizieren. Wenn eine Aktivität, die als Ergebnis des Benutzers, tippen Sie auf ein Suchergebnis fortgesetzt wird, bestimmt den Aktivitätstyp (zusammen mit der app-Team-ID) der app zu starten, um die Aktivität fortgesetzt.

Zum Erstellen der erforderlichen Aktivität Typ-IDs zur Unterstützung dieses Verhalten Bearbeiten der **"Info.plist"** Datei, und wechseln Sie zu der **Quelle** anzeigen. Hinzufügen einer `NSUserActivityTypes` Taste, und Erstellen von Bezeichnern in folgendem Format:

[![](nsuseractivity-images/type01.png "Die NSUserActivityTypes Schlüssel und die erforderlichen Bezeichner in der Plist-editor")](nsuseractivity-images/type01.png#lightbox)

Im obigen Beispiel wird eine neue Aktivität Typbezeichner für die Suche Aktivität erstellt (`com.xamarin.platform`). Wenn Sie Ihre eigenen apps zu erstellen, ersetzen Sie den Inhalt von der `NSUserActivityTypes` array mit der Aktivität-Datentypbezeichner, die spezifisch für die Aktivitäten Ihrer app unterstützt.

<a name="createactivity" />

## <a name="creating-an-activity"></a>Erstellen einer Aktivität

Im folgenden finden ein Beispiel zum Erstellen einer Aktivität für die Indexsuche auf dem Gerät gehostet:

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

Wir konnten Sie weitere Details hinzufügen durch Festlegen der `ContentAttributeSet` Eigenschaft unsere `NSUserActivity` wie folgt:

[![](nsuseractivity-images/apphistory02.png "Außerdem Details suchen (Übersicht)")](nsuseractivity-images/apphistory02.png#lightbox)

Mithilfe einer `ContentAttributeSet` können Sie umfangreiche Suchergebnisse, die der Endbenutzer mit ihnen interagieren verleiten erstellen.

<a name="respondactivity" />

## <a name="responding-to-an-activity"></a>Reagieren auf eine Aktivität

So reagieren Sie auf den Benutzer Tippen auf ein Suchergebnis (`NSUserActivity`) für unsere app Bearbeiten der **AppDelegate.cs** Datei, und überschreiben die `ContinueUserActivity` Methode. Zum Beispiel:

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

Beachten Sie, dass dies dieselbe Methode Außerkraftsetzung zur Reaktion auf die Übergabe von Anforderungen verwendet wird. Jetzt klickt der Benutzer auf einen Link aus dieser app in den Ergebnissen der Spotlight-Suche unserer app wird in den Vordergrund gebracht (oder gestartet wurde, wenn nicht bereits ausgeführt werden) und der Inhalte, die Navigation oder die Funktion durch diesen Link dargestellt wird angezeigt werden:

[![](nsuseractivity-images/apphistory03.png "Wiederherstellen der vorherigen Zustand aus der Suche")](nsuseractivity-images/apphistory03.png#lightbox)

<a name="indexing" />

## <a name="public-search-indexing"></a>Öffentliche Indizierung

Wie oben veranschaulichte Filtermenü, vereinfacht die Verwendung iOS 9 Geben Sie Suche Zugriff auf app-Inhalte und Funktionen, die der Benutzer bereits ermittelt und auf einem bestimmten iOS-Gerät verwendet. Mit der öffentlichen Indizierung iOS 9 bietet eine Möglichkeit für Benutzer, die noch nicht Inhalt oder Funktionen noch ermittelt (oder die app noch nicht selbst installiert) zu abzurufenden diese Ergebnisse in die Suche.

Öffentliche Indizierung funktioniert wie folgt:

1. Wenn Sie eine Aktivität für Ihre app erstellen, können Sie es als öffentlich markieren.
2. Öffentliche Aktivitäten an Apple gesendet und in der Cloud indiziert.
3. Als weitere Benutzer mit der app auf einem Gerät interagieren und die spezifische Funktion oder von der Aktivität dargestellte Inhalt, steigt es Rang.
4. Beliebte öffentliche Ergebnisse werden anderen Benutzern zur Verfügung, auch wenn sie die app nicht installiert haben.
5. Diese öffentliche Ergebnisse werden angezeigt in Spotlight-Suche und Safari (wenn die Aktivität eine URL enthält).

Wir können dauern privaten-Aktivität, der wir zuvor erstellt haben, und erweitern, um die öffentlich sein:

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

Nur weil eine Aktivität festgelegt wurde, für die öffentliche Indizierung durch Festlegen von `EligibleForPublicIndexing = true`, es bedeutet nicht, dass es automatisch von Apple öffentlichen Cloud Index hinzugefügt werden. Die folgenden Bedingungen müssen erfüllt werden:

1. Es muss in den Suchergebnissen angezeigt werden und von vielen Benutzern ausgewählt werden. Die Ergebnisse bleiben private, bis ein Aktivität Engagement Schwellenwert erreicht wurde.
2. App-Bereitstellung wird verhindert, dass benutzerspezifische Daten indiziert und öffentlich gemacht.

<a name="benefits" />

## <a name="additional-benefits"></a>Weitere Vorteile

Durch die Annahme von App-Suche über `NSUserActivity` in Ihrer app erhalten Sie auch die folgenden Funktionen:

- **Übergabe** – da App-Suche von Inhalt, Navigation Verfügbarmachen ist und/oder Funktionen nutzen denselben Mechanismus wie Übergabe (`NSUserActivity`), Sie können problemlos Benutzern Ihrer app zu starten, eine Aktivität auf einem Gerät auf einem anderen fortgesetzt.
- **Siri Vorschläge** -zusammen mit den standard Siri Vorschläge normalerweise macht Vorschläge aktiv von der app automatisch vorgeschlagen werden können.
- **Siri intelligente Erinnerungen** -Benutzer werden in der Lage, bitten Sie Siri, um sie zu Aktivitäten aus Ihrer app daran zu erinnern.



## <a name="related-links"></a>Verwandte Links

- [iOS 9-Beispiele](https://developer.xamarin.com/samples/ios/iOS9/)
- [iOS 9 für Entwickler](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Programmierhandbuch für App-Suche](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/AppSearch/index.html#//apple_ref/doc/uid/TP40016308)
- [Blogbeitrag & Beispiel](https://blog.xamarin.com/improve-discoverability-with-search-in-ios-9/)
