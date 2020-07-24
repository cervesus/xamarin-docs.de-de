---
title: Suche mit Core Spotlight in xamarin. IOS
description: In diesem Dokument wird beschrieben, wie Sie Core Spotlight in einer xamarin. IOS-Anwendung verwenden können, um Links zu in-App-Inhalten bereitzustellen. Darin wird erläutert, wie durchsuchbare Elemente erstellt, wieder hergestellt, aktualisiert und gelöscht werden.
ms.prod: xamarin
ms.assetid: 1374914C-0F63-41BF-BD97-EBCEE86E57B1
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/20/2017
ms.openlocfilehash: 285243c832d080d93e557deada5bb824e03f8d89
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86939424"
---
# <a name="search-with-core-spotlight-in-xamarinios"></a>Suche mit Core Spotlight in xamarin. IOS

Core Spotlight ist ein neues Framework für IOS 9, das eine Daten Bank ähnliche API zum Hinzufügen, bearbeiten oder Löschen von Links zu Inhalten in Ihrer APP darstellt. Elemente, die mit Core Spotlight hinzugefügt wurden, stehen in Spotlight-Suche auf dem IOS-Gerät zur Verfügung.

Ein Beispiel für die Inhaltstypen, die mithilfe von Core Spotlight indiziert werden können, finden Sie unter Apple Messages, Mail, Calendar und Notes apps. Sie verwenden derzeit Kern Spotlight zum Bereitstellen von Suchergebnissen.

## <a name="creating-an-item"></a>Erstellen eines Elements

Im folgenden finden Sie ein Beispiel für das Erstellen eines Elements und das Indizieren mithilfe von Core Spotlight:

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

Diese Informationen werden in einem Suchergebnis wie folgt angezeigt:

[![Übersicht über das Core Spotlight-Suchergebnis](corespotlight-images/corespotlight01.png)](corespotlight-images/corespotlight01.png#lightbox)

## <a name="restoring-an-item"></a>Wiederherstellen eines Elements

Wenn der Benutzer auf ein Element tippt, das dem Suchergebnis über Core Spotlight für Ihre APP hinzugefügt wurde, wird die- `AppDelegate` Methode `ContinueUserActivity` aufgerufen (diese Methode wird auch für verwendet `NSUserActivity` ). Beispiel:

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

Beachten Sie, dass dieses Mal überprüft wird, ob die Aktivität über einen `ActivityType` von verfügt `CSSearchableItem.ActionType` .

## <a name="updating-an-item"></a>Aktualisieren eines Elements

Es kann vorkommen, dass ein Index Element, das wir mit Core Spotlight erstellt haben, geändert werden muss, z. b. Wenn eine Änderung des Titels oder des Miniatur Bilds erforderlich ist. Um diese Änderung vorzunehmen, verwenden wir dieselbe Methode wie zum anfänglichen Erstellen des Indexes.
Wir erstellen eine neue `CSSearchableItem` mit derselben ID wie zum Erstellen des Elements und Anfügen eines neuen mit `CSSearchableItemAttributeSet` den geänderten Attributen:

[![Aktualisieren eines Element Übersichts](corespotlight-images/corespotlight02.png)](corespotlight-images/corespotlight02.png#lightbox)

Wenn dieses Element in den durchsuchbaren Index geschrieben wird, wird das vorhandene Element mit den neuen Informationen aktualisiert.

## <a name="deleting-an-item"></a>Löschen eines Elements

Core Spotlight bietet mehrere Möglichkeiten zum Löschen eines Index Elements, wenn es nicht mehr benötigt wird.

Zuerst können Sie ein Element anhand seines Bezeichners löschen, z. b.:

```csharp
// Delete Items by ID
CSSearchableIndex.DefaultSearchableIndex.Delete(new string[]{"1","16"},(error) => {
    // Successful?
    if (error !=null) {
        Console.WriteLine(error.LocalizedDescription);
    }
});
```

Als nächstes können Sie eine Gruppe von Index Elementen anhand ihres Domänen namens löschen. Beispiel:

```csharp
// Delete by Domain Name
CSSearchableIndex.DefaultSearchableIndex.DeleteWithDomain(new string[]{"domain-name"},(error) => {
    // Successful?
    if (error !=null) {
        Console.WriteLine(error.LocalizedDescription);
    }
});
```

Schließlich können Sie alle Index Elemente mit folgendem Code löschen:

```csharp
// Delete all index items
CSSearchableIndex.DefaultSearchableIndex.DeleteAll((error) => {
    // Successful?
    if (error !=null) {
        Console.WriteLine(error.LocalizedDescription);
    }
});
```

## <a name="additional-core-spotlight-features"></a>Zusätzliche kernspotlight-Features

Core Spotlight verfügt über die folgenden Features, die dabei helfen, den Index genau und auf dem neuesten Stand zu halten:

- **Unterstützung für Batch Updates** – Wenn Ihre APP eine große Gruppe von Indizes gleichzeitig erstellen oder ändern muss, kann der gesamte Batch `Index` `CSSearchableIndex` in einem Aufruf an die-Methode der-Klasse gesendet werden.
- **Reagieren auf Index Änderungen** – die Verwendung `CSSearchableIndexDelegate` Ihrer APP kann auf Änderungen und Benachrichtigungen vom durchsuchbaren Index Antworten.
- **Anwenden von Datenschutz** – mithilfe der Datenschutz Klassen können Sie die Sicherheit für die Elemente implementieren, die Sie mithilfe von Core Spotlight dem durchsuchbaren Index hinzufügen.

## <a name="related-links"></a>Verwandte Links

- [IOS 9-Beispiele](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS9)
- [IOS 9 für Entwickler](https://developer.apple.com/ios/pre-release/)
- [IOS 9,0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Programmier Handbuch für die APP-Suche](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/AppSearch/index.html#//apple_ref/doc/uid/TP40016308)
