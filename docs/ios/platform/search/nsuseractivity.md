---
title: Suchen mit nsuseractivity in xamarin. IOS
description: In diesem Dokument wird beschrieben, wie Sie eine nsuseractivity indizieren, um Sie in Spotlight und Safari durchsuchbar zu gestalten. Es wird erläutert, wie auf die Auswahl einer nsuseractivity in den Suchergebnissen geantwortet wird.
ms.prod: xamarin
ms.assetid: 0B28B284-C7C9-4C0D-A782-D471FBBC4CAE
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/20/2017
ms.openlocfilehash: 314b341b7cfb40c62a5d6469ca22d2889f5a8057
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70291756"
---
# <a name="search-with-nsuseractivity-in-xamarinios"></a>Suchen mit nsuseractivity in xamarin. IOS

`NSUserActivity`wurde in ios 8 eingeführt und wird verwendet, um die Daten für die Übergabe bereitzustellen.
Sie ermöglicht es Ihnen, Aktivitäten in bestimmten Teilen Ihrer APP zu erstellen, die dann an eine andere Instanz Ihrer APP weitergeleitet werden können, die auf einem anderen ios-Gerät ausgeführt wird. Das empfangende Gerät kann dann die Aktivität fortsetzen, die auf dem vorherigen Gerät gestartet wurde, und dann nach rechts, wo der Benutzer aufgehört hat. Weitere Informationen zur Verwendung von Handoff finden Sie in unserer [Einführung in die Übergabe](~/ios/platform/handoff.md) Dokumentation.

Neu bei IOS 9, `NSUserActivity` kann indiziert werden (sowohl öffentlich als auch privat) und in Spotlight-Suche und Safari gesucht werden. Wenn Sie ein `NSUserActivity` als durchsuchbar markieren und indizierbare Metadaten hinzufügen, kann die Aktivität in den Suchergebnissen auf dem IOS-Gerät aufgeführt werden.

[![](nsuseractivity-images/apphistory01.png "Übersicht über den App-Verlauf")](nsuseractivity-images/apphistory01.png#lightbox)

Wenn der Benutzer ein Suchergebnis auswählt, das zu einer Aktivität in Ihrer APP gehört, wird die APP gestartet, und die von `NSUserActivity` beschriebene Aktivität wird neu gestartet und dem Benutzer angezeigt.

Die folgenden Eigenschaften von `NSUserActivity` werden verwendet, um die APP-Suche zu unterstützen:

- `EligibleForHandoff`– Wenn `true`, kann diese Aktivität in einem Handoff-Vorgang verwendet werden.
- `EligibleForSearch`– Gibt `true`an, dass diese Aktivität dem On-Device-Index hinzugefügt und in den Suchergebnissen angezeigt wird.
- `EligibleForPublicIndexing`– Gibt `true`an, dass diese Aktivität dem cloudbasierten Index von Apple hinzugefügt und Benutzern (über die Suche) angezeigt wird, die Ihre APP nicht bereits auf Ihrem IOS-Gerät installiert haben. Weitere Informationen finden Sie im Abschnitt zur [Indizierung der öffentlichen Suche](#public-search-indexing) weiter unten.
- `Title`– Stellt einen Titel für Ihre Aktivität bereit und wird in den Suchergebnissen angezeigt. Benutzer können auch nach dem Text des Titels selbst suchen.
- `Keywords`– Ist ein Array von Zeichen folgen, die zum Beschreiben der Aktivität verwendet werden, die indiziert und vom Endbenutzer durchsuchbar gemacht wird.
- `ContentAttributeSet`– Ist eine `CSSearchableItemAttributeSet` , die verwendet wird, um die Aktivität ausführlicher zu beschreiben und umfangreiche Inhalte in den Suchergebnissen bereitzustellen.
- `ExpirationDate`– Wenn Sie möchten, dass eine Aktivität nur bis zu einem bestimmten Datum angezeigt wird, können Sie dieses Datum hier angeben.
- `WebpageURL`– Wenn die Aktivität im Internet angezeigt werden kann oder wenn Ihre APP die Deep-Links von Safari unterstützt, können Sie den Link hier aufrufen.

## <a name="nsuseractivity-quickstart"></a>Nsuseractivity-Schnellstart

Befolgen Sie diese Anweisungen, um eine durch `NSUserActivity` Such Bare in Ihrer APP zu implementieren:

- [Erstellen von Aktivitätstyp bezeichlern](#creatingtypeid)
- [Erstellen einer Aktivität](#createactivity)
- [Reagieren auf eine Aktivität](#respondactivity)
- [Indizierung der öffentlichen Suche](#indexing)

Es gibt einige [zusätzliche Vorteile](#benefits) bei der `NSUserActivity` Verwendung von, um Ihre Inhalte durchsuchbar zu machen.

<a name="creatingtypeid" />

## <a name="creating-activity-type-identifiers"></a>Erstellen von Aktivitätstyp bezeichlern

Bevor Sie eine Such Aktivität erstellen können, müssen Sie einen _Aktivitätstyp Bezeichner_ erstellen, um ihn zu identifizieren. Der Aktivitätstyp Bezeichner ist eine kurze Zeichenfolge `NSUserActivityTypes` , die dem Array der **Info. plist** -Datei der app hinzugefügt wird, die zur eindeutigen Identifizierung eines bestimmten Benutzer Aktivitäts Typs verwendet wird. Es gibt einen Eintrag im-Array für jede Aktivität, die von der App unterstützt wird und die für die APP-Suche verfügbar gemacht wird. 

Apple schlägt vor, eine Notation im Reverse-DNS-Stil für den Aktivitätstyp Bezeichner zu verwenden, um Konflikte zu vermeiden. Beispiel: `com.company-name.appname.activity` für bestimmte App-basierte Aktivitäten oder `com.company-name.activity` für Aktivitäten, die über mehrere apps hinweg ausgeführt werden können.

Der Aktivitätstyp Bezeichner wird verwendet, `NSUserActivity` wenn eine-Instanz erstellt wird, um den Typ der Aktivität zu identifizieren. Wenn eine Aktivität durch den Benutzer, der auf ein Suchergebnis tippt, fortgesetzt wird, bestimmt der Aktivitätstyp (zusammen mit der Team-ID der APP), welche App gestartet werden soll, um die Aktivität fortzusetzen.

Um die erforderlichen Aktivitätstypen Bezeichner zu erstellen, um dieses Verhalten zu unterstützen, bearbeiten Sie die Datei **Info. plist** , und wechseln Sie zur **Quell** Ansicht. Fügen Sie `NSUserActivityTypes` einen Schlüssel hinzu, und erstellen Sie Bezeichner im folgenden Format:

[![](nsuseractivity-images/type01.png "Der nsuseractivitytypes-Schlüssel und erforderliche Bezeichner im plist-Editor")](nsuseractivity-images/type01.png#lightbox)

Im obigen Beispiel haben wir einen neuen Aktivitätstyp Bezeichner für die Such Aktivität (`com.xamarin.platform`) erstellt. Wenn Sie eigene Apps erstellen, ersetzen Sie den Inhalt des `NSUserActivityTypes` Arrays durch die Aktivitätstyp-IDs, die für die von Ihrer APP unterstützten Aktivitäten spezifisch sind.

<a name="createactivity" />

## <a name="creating-an-activity"></a>Erstellen einer Aktivität

Im folgenden finden Sie ein Beispiel für das Erstellen einer Aktivität für eine Suche nach einem auf dem Gerät gehosteten Index:

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

Wir könnten weitere Details hinzufügen, indem `ContentAttributeSet` wir die- `NSUserActivity` Eigenschaft von wie folgt festlegen:

[![](nsuseractivity-images/apphistory02.png "Übersicht über die Details der Such Details")](nsuseractivity-images/apphistory02.png#lightbox)

Mithilfe eines `ContentAttributeSet` können Sie umfassende Suchergebnisse erstellen, die den Endbenutzer für die Interaktion mit Ihnen in verleiten verwenden.

<a name="respondactivity" />

## <a name="responding-to-an-activity"></a>Reagieren auf eine Aktivität

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

Beachten Sie, dass es sich hierbei um dieselbe Methoden Überschreibung handelt, die für die Reaktion auf Übergabe Anforderungen verwendet wird Wenn der Benutzer nun in den Suchergebnissen des Spotlight auf einen Link aus unserer App klickt, wird unsere app in den Vordergrund gestellt (oder gestartet, wenn Sie nicht bereits ausgeführt wird), und der Inhalt, die Navigation oder die Funktion, die von diesem Link dargestellt wird, wird angezeigt:

[![](nsuseractivity-images/apphistory03.png "Vorherigen Zustand aus der Suche wiederherstellen")](nsuseractivity-images/apphistory03.png#lightbox)

<a name="indexing" />

## <a name="public-search-indexing"></a>Indizierung der öffentlichen Suche

Wie bereits erwähnt, vereinfacht IOS 9 das Bereitstellen von Such Zugriff auf App-Inhalte und-Features, die der Benutzer bereits erkannt und auf einem bestimmten IOS-Gerät verwendet hat. Mit der öffentlichen Indizierung bietet IOS 9 eine Möglichkeit für Benutzer, die noch keine Inhalte oder Features erkannt haben (oder die die APP noch nicht installiert haben), um diese Ergebnisse auch in ihren Suchergebnissen zu erhalten.

Die öffentliche Indizierung funktioniert wie folgt:

1. Wenn Sie eine Aktivität für die APP erstellen, können Sie Sie als öffentlich kennzeichnen.
2. Öffentliche Aktivitäten werden an Apple gesendet und in der Cloud indiziert.
3. Wenn mehr Benutzer mit Ihrer APP auf einem Gerät interagieren und die von der Aktivität dargestellte bestimmte Funktion oder den Inhalt verwenden, steigt Sie in Rang.
4. Beliebte öffentliche Ergebnisse werden anderen Benutzern zur Verfügung gestellt, auch wenn Sie die APP nicht installiert haben.
5. Diese öffentlichen Ergebnisse werden in Spotlight-Suche und Safari angezeigt (wenn die Aktivität eine URL enthält).

Wir können die zuvor erstellte private Such Aktivität durchführen und Sie als öffentlich erweitern:

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

Nur weil eine Aktivität durch Festlegen `EligibleForPublicIndexing = true`der öffentlichen Indizierung festgelegt wurde, bedeutet dies nicht, dass Sie automatisch dem Public Cloud Index von Apple hinzugefügt wird. Zuerst müssen die folgenden Bedingungen erfüllt sein:

1. Er muss in den Suchergebnissen angezeigt werden und von vielen Benutzern ausgewählt werden. Die Ergebnisse bleiben privat, bis ein Aktivitäts Engagement-Schwellenwert erreicht wurde.
2. Mithilfe der APP-Bereitstellung wird verhindert, dass benutzerspezifische Daten indiziert und öffentlich gemacht werden.

<a name="benefits" />

## <a name="additional-benefits"></a>Weitere Vorteile

Durch die Einführung der APP `NSUserActivity` -Suche über in Ihrer APP erhalten Sie auch die folgenden Features:

- **Handoff** : da bei der APP-Suche Inhalt, Navigation und/oder Features mithilfe desselben Mechanismus wie Handoff (`NSUserActivity`) verfügbar gemacht werden, können Sie den Benutzern Ihrer APP leicht gestatten, eine Aktivität auf einem Gerät zu starten und auf einem anderen Gerät fortzufahren.
- **Siri-Vorschläge** : zusammen mit den standardvorschlägen, die Siri-Vorschläge normalerweise vornimmt, können die aktiven Benutzer aus Ihrer APP automatisch vorgeschlagen werden.
- **Siri-intelligente Erinnerungen** : Benutzer können Siri bitten, Sie an Aktivitäten aus Ihrer APP zu erinnern.



## <a name="related-links"></a>Verwandte Links

- [IOS 9-Beispiele](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS9)
- [IOS 9 für Entwickler](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Programmier Handbuch für die APP-Suche](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/AppSearch/index.html#//apple_ref/doc/uid/TP40016308)
- [Blog Beitrag &-Beispiel](https://blog.xamarin.com/improve-discoverability-with-search-in-ios-9/)
