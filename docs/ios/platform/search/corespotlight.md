---
title: Suche mit Core Spotlight in Xamarin.iOS
description: Dieses Dokument beschreibt, wie Core Spotlight in einer Xamarin.iOS-Anwendung zu verwenden, um Links zu in-app-Inhalte bereitzustellen. Es wird erläutert, wie erstellen, wiederherstellen, aktualisieren und löschen Sie durchsuchbaren Elemente.
ms.prod: xamarin
ms.assetid: 1374914C-0F63-41BF-BD97-EBCEE86E57B1
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/20/2017
ms.openlocfilehash: fb9ddcc39bd33199dc370897250cd0d74597612f
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61248472"
---
# <a name="search-with-core-spotlight-in-xamarinios"></a>Suche mit Core Spotlight in Xamarin.iOS

Core Spotlight ist ein neues Framework für iOS 9, in dem eine datenbankähnliche-API hinzufügen, bearbeiten oder Löschen von Links zu Inhalten innerhalb Ihrer app dargestellt. Elemente, die mit Core Spotlight hinzugefügt wurden, werden in Spotlight-Suche auf iOS-Geräten verfügbar sein.

Ein Beispiel für die Arten von Inhalten, die mit Core Spotlight indiziert werden können, finden Sie in der Apple Nachrichten, E-Mail, Kalender und Anmerkungen zu dieser apps. Sie verwenden derzeit Core Spotlight Suchergebnisse liefern.

## <a name="creating-an-item"></a>Erstellen eines Elements

Im folgenden finden ein Beispiel für ein Element erstellen und Indizierung mit Core Spotlight:

```csharp
using CoreSpotlight;
...

// Create attributes to describe an item
var attributes = new CSSearchableItemAttributeSet();
attributes.Title = "App Center Test";
attributes.ContentDescription = "Automatically test your app on 1,000 devices in the cloud.";

// Create item
var item = new CSSearchableItem ("1", "products", attributes);

// Index item
CSSearchableIndex.DefaultSearchableIndex.Index (new CSSearchableItem[]{ item }, (error) => {
    // Successful?
    if (error !=null) {
        Console.WriteLine(error.LocalizedDescription);
    }
});
```

Diese Informationen würde wie folgt in einem Suchergebnis angezeigt:

[![](corespotlight-images/corespotlight01.png "Core Spotlight-Suche Ergebnis (Übersicht)")](corespotlight-images/corespotlight01.png#lightbox)

## <a name="restoring-an-item"></a>Wiederherstellen eines Elements

Wenn der Benutzer, auf ein Element hinzugefügt wird, auf das Suchergebnis über Core Schwerpunktthema für Ihre app tippt die `AppDelegate` Methode `ContinueUserActivity` aufgerufen wird (diese Methode wird auch zum `NSUserActivity`). Zum Beispiel:

```csharp
public override bool ContinueUserActivity (UIApplication application,
   NSUserActivity userActivity, UIApplicationRestorationHandler completionHandler)
{

    // Take action based on the activity type
    switch (userActivity.ActivityType) {
    case "com.xamarin.platform":
        // Restore the state of the app here...
        break;
    default:
        if (userActivity.ActivityType == CSSearchableItem.ActionType.ToString ()) {
            // Display content for searchable item...
        }
        break;
    }

    return true;
}
```

Beachten Sie, dass dieses Mal werden die Kontrollkästchen für die Aktivität mit einer `ActivityType` von `CSSearchableItem.ActionType`.

## <a name="updating-an-item"></a>Aktualisieren eines Elements

Es gibt möglicherweise Zeiten, wenn ein Index-Element, das wir mit Core Spotlight erstellt müssen geändert werden, wie z. B. eine Änderung im Titel oder Miniaturansicht erforderlich ist. Um diese Änderung vorzunehmen, verwenden wir die gleiche Methode wie zu Beginn den Index erstellt.
Wir erstellen eine neue `CSSearchableItem` mit derselben ID, wie zum Erstellen des Elements, und fügen Sie einen neuen `CSSearchableItemAttributeSet` mit den geänderten Attributen:

[![](corespotlight-images/corespotlight02.png "Übersicht über die ein Element aktualisieren")](corespotlight-images/corespotlight02.png#lightbox)

Wenn dieses Element in der durchsuchbaren Index geschrieben wird, wird das vorhandene Element mit den neuen Informationen aktualisiert.

## <a name="deleting-an-item"></a>Löschen eines Elements

Core Spotlight bietet Ihnen mehrere Möglichkeiten, eine Index-Element zu löschen, wenn es nicht mehr benötigt wird.

Erstens können Sie ein Element durch seinen Bezeichner, z. B. löschen:

```csharp
// Delete Items by ID
CSSearchableIndex.DefaultSearchableIndex.Delete(new string[]{"1","16"},(error) => {
    // Successful?
    if (error !=null) {
        Console.WriteLine(error.LocalizedDescription);
    }
});
```

Als Nächstes können Sie eine Gruppe von Elementen der Index nach ihren Domänennamen löschen. Zum Beispiel:

```csharp
// Delete by Domain Name
CSSearchableIndex.DefaultSearchableIndex.DeleteWithDomain(new string[]{"domain-name"},(error) => {
    // Successful?
    if (error !=null) {
        Console.WriteLine(error.LocalizedDescription);
    }
});
```

Schließlich können Sie alle Elemente von Index durch den folgenden Code löschen:

```csharp
// Delete all index items
CSSearchableIndex.DefaultSearchableIndex.DeleteAll((error) => {
    // Successful?
    if (error !=null) {
        Console.WriteLine(error.LocalizedDescription);
    }
});
```
## <a name="additional-core-spotlight-features"></a>Zusätzliche Spotlight-Kernfeatures

Schwerpunktthema in Core weist folgende Merkmale, mit denen Sie um den Index präzise und auf dem neuesten Stand zu halten:

- **Batch-Unterstützung für die Aktualisierung** – Wenn Ihre app muss zum Erstellen oder eine große Gruppe von Indizes zur gleichen Zeit ändern, kann der gesamte Batch gesendet werden, auf die `Index` -Methode der der `CSSearchableIndex` Klasse in einem einzigen Aufruf.
- **Reagieren auf Änderungen des Index** – über die `CSSearchableIndexDelegate` Ihrer app von Änderungen und Benachrichtigungen aus dem durchsuchbaren Index reagieren kann.
- **Anwenden von Schutz von Daten** – verwenden die Schutz-Datenklassen, können Sie Sicherheit für die Elemente, die Sie hinzufügen, auf den durchsuchbaren Index mithilfe von Core Spotlight implementieren.



## <a name="related-links"></a>Verwandte Links

- [iOS 9-Beispiele](https://developer.xamarin.com/samples/ios/iOS9/)
- [iOS 9 für Entwickler](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Programmierhandbuch für App-Suche](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/AppSearch/index.html#//apple_ref/doc/uid/TP40016308)
