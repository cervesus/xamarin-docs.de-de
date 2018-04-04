---
title: Suche mit Core Spotlight
ms.prod: xamarin
ms.assetid: 1374914C-0F63-41BF-BD97-EBCEE86E57B1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: d38d90ce460c7a93f8412baf372778443eb9d9e9
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="search-with-core-spotlight"></a>Suche mit Core Spotlight

Core Spotlight ist ein neues Framework für iOS 9, in dem eine Datenbank-ähnliche API zum Hinzufügen, bearbeiten oder Löschen von Links zu Inhalten innerhalb der app dargestellt. Elemente, die mit Core Spotlight hinzugefügt wurden, werden in der Spotlight-Suche auf dem iOS-Gerät verfügbar sein.

Ein Beispiel für die Typen von Inhalten, die mit Core Spotlight indiziert werden können, finden Sie in der Apple-Nachrichten, E-Mail, Kalender und Anmerkungen zu dieser apps. Sie verwenden derzeit Core Spotlight Suchergebnisse bereit.

## <a name="creating-an-item"></a>Erstellen ein Element

Im folgenden finden ein Beispiel für ein Element erstellen und deren Indizierung mit Core Spotlight:

```csharp
using CoreSpotlight;
...

// Create attributes to describe an item
var attributes = new CSSearchableItemAttributeSet();
attributes.Title = "Test Cloud";
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

[![](corespotlight-images/corespotlight01.png "Core Spotlight-Suche-Ergebnis (Übersicht)")](corespotlight-images/corespotlight01.png#lightbox)

## <a name="restoring-an-item"></a>Ein Element wiederherstellen

Beim Tippen auf ein Element hinzugefügt wird, auf das Suchergebnis über Core Spotlight für Ihre app die `AppDelegate` Methode `ContinueUserActivity` aufgerufen wird (diese Methode wird auch zum `NSUserActivity`). Zum Beispiel:

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

Beachten Sie, dass dieses Mal werden die Kontrollkästchen für die Aktivität mit einem `ActivityType` von `CSSearchableItem.ActionType`.

## <a name="updating-an-item"></a>Aktualisieren eines Elements

Es gibt möglicherweise vorkommen, dass ein Index-Element, das wir mit Core Spotlight erstellt müssen geändert werden, wie z. B. eine Änderung im Titel oder Miniaturbild erforderlich ist. Um diese Änderung vorzunehmen, verwenden wir die gleiche Methode verwendet wurde, um zunächst den Index zu erstellen.
Wir erstellen Sie ein neues `CSSearchableItem` mit derselben ID wie zum Erstellen des Artikels, und fügen Sie ein neues `CSSearchableItemAttributeSet` mit den geänderten Attributen:

[![](corespotlight-images/corespotlight02.png "Aktualisieren ein Element (Übersicht)")](corespotlight-images/corespotlight02.png#lightbox)

Wenn dieses Element in der durchsuchbaren Index geschrieben wird, wird das vorhandene Element mit den neuen Informationen aktualisiert.

## <a name="deleting-an-item"></a>Löschen eines Elements

Core Spotlight bietet mehrere Möglichkeiten, ein Index-Element zu löschen, wenn es nicht mehr benötigt wird.

Zunächst können Sie ein Element durch seinen Bezeichner, z. B. löschen:

```csharp
// Delete Items by ID
CSSearchableIndex.DefaultSearchableIndex.Delete(new string[]{"1","16"},(error) => {
    // Successful?
    if (error !=null) {
        Console.WriteLine(error.LocalizedDescription);
    }
});
```

Als Nächstes können Sie eine Gruppe von Elementen der Index den Namen ihrer Domäne löschen. Zum Beispiel:

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
## <a name="additional-core-spotlight-features"></a>Zusätzliche Spotlight-Kernfunktionen

Core Spotlight hat die folgenden Funktionen, mit denen Sie um den Index genau als auch auf dem neuesten Stand zu halten:

- **Batch-Unterstützung für die Aktualisierung** – Wenn Ihre app muss zum Erstellen oder Ändern einer großen Gruppe von Indizes zur gleichen Zeit, kann der gesamte Batch gesendet werden, auf die `Index` Methode der `CSSearchableIndex` Klasse in einem Aufruf.
- **Reagieren auf Änderungen des Index** – über das `CSSearchableIndexDelegate` Ihrer app auf Änderungen und Benachrichtigungen aus dem Index durchsuchbaren reagieren kann.
- **Anwenden von Data Protection** – verwenden den Schutz-Datenklassen, können Sie Sicherheit für die Elemente, die Sie der durchsuchbaren Index mit Core Spotlight hinzufügen implementieren.



## <a name="related-links"></a>Verwandte Links

- [iOS 9-Beispiele](https://developer.xamarin.com/samples/ios/iOS9/)
- [iOS 9 für Entwickler](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Programmierhandbuch für App-Suche](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/AppSearch/index.html#//apple_ref/doc/uid/TP40016308)
