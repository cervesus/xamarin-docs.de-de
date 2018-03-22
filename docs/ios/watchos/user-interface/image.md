---
title: Image-Steuerelement
ms.topic: article
ms.prod: xamarin
ms.assetid: B741C207-3427-46F3-9C90-A52BF8933FA4
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 4184d7babc396a6b6179e6876dced34b773474b4
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/22/2018
---
# <a name="image-control"></a>Image-Steuerelement

WatchOS bietet eine [ `WKInterfaceImage` ](https://developer.xamarin.com/api/type/WatchKit.WKInterfaceImage/) -Steuerelement zum Anzeigen von Bildern und einfache Animationen. Einige Steuerelemente können auch ein Hintergrundbild (z. B. Schaltflächen, Gruppen und Schnittstelle Controller) aufweisen.

![](image-images/image-walkway.png "Apple Watch Anzeigen eines Bildes") ![ ] (image-images/image-animation.png "Apple Watch mit einfache Animation")
<!-- watch image courtesy of http://infinitapps.com/bezel/ -->

Verwenden Sie Asset-Katalog-Images, um Bilder Überwachungsfenster Kit apps hinzufügen.
Nur  **@2x**  Versionen sind erforderlich, da sehen Sie sich alle Geräte Retina angezeigt haben.

![](image-images/asset-universal-sml.png "Nur 2 X-Versionen sind erforderlich, da sehen Sie sich alle Geräte Retina zeigt haben")

Es ist ratsam, um sicherzustellen, dass die Bilder selbst die richtige Größe für die Überwachung angezeigt werden. *Vermeiden Sie* Verwendung falsch Größe Images (besonders großen Werten), und skalieren, um sie auf der Apple Watch anzuzeigen.

Die Überwachung Kit Größen (38 mm und 42 mm) können in einem Asset-Katalog-Image verschiedene Bilder für jede Anzeigegröße angegeben.

![](image-images/asset-watch-sml.png "Die Überwachung Kit Größen 38 mm und 42 mm können in einem Asset-Katalog-Image Sie verschiedene Bilder für die einzelnen Anzeigegröße angeben")


## <a name="images-on-the-watch"></a>Bilder auf der Apple Watch

Anzeige von Bildern die effizienteste Möglichkeit besteht darin *Watch-app-Projekt einschließen* und zeigen sie mithilfe der `SetImage(string imageName)` Methode.

Z. B. die [WatchKitCatalog](https://developer.xamarin.com/samples/WatchKitCatalog/) Beispiel bietet eine Reihe von Bildern, die eine Asset-Katalog im Watch-app-Projekt hinzugefügt:

![](image-images/asset-whale-sml.png "Das Beispiel WatchKitCatalog bietet eine Reihe von Images, die eine Asset-Katalog im Watch-app-Projekt hinzugefügt")

Diese können effizient geladen und angezeigt, auf die Überwachung mit `SetImage` mit der Namensparameter der Zeichenfolge:

```csharp
myImageControl.SetImage("Whale");
myOtherImageControl.SetImage("Worry");
```

### <a name="background-images"></a>Hintergrundbilder

Die gleiche Logik gilt für die `SetBackgroundImage (string imageName)` auf die `Button`, `Group`, und `InterfaceController` Klassen. Optimale Leistung wird erreicht, indem Sie die Bilder in Watch-app selbst zu speichern.


## <a name="images-in-the-watch-extension"></a>Bilder in der Watch-Erweiterung

Zusätzlich zum Laden von Images, die in der Watch-app selbst gespeichert sind, können Sie Bilder aus dem Paket für die Erweiterung in die Watch-app für die Anzeige senden (oder Sie könnten Bilder von remote-Speicherorten herunterladen und diejenigen anzeigen).

Erstellen Sie zum Laden von Bildern aus der Watch-Erweiterung `UIImage` Instanzen, und rufen Sie anschließend `SetImage` mit der `UIImage` Objekt.

Z. B. die [WatchKitCatalog](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/) Beispiel enthält ein Bild, das mit dem Namen **Bumblebee** im Erweiterungsprojekt überwachen:

![](image-images/asset-bumblebee-sml.png "Das Beispiel WatchKitCatalog enthält ein Bild, das mit dem Namen Bumblebee im Überwachungsfenster Erweiterungsprojekt")

Der folgende Code führt zu:

- das Bild wird in den Arbeitsspeicher geladen und
- Klicken Sie auf der Apple Watch angezeigt.

```csharp
using (var image = UIImage.FromBundle ("Bumblebee")) {
    myImageControl.SetImage (image);
}
```


## <a name="animations"></a>Animationen

Um einen Satz von Bildern zu animieren, sollten sie alle mit dem gleichen Präfix beginnen und ein numerisches Suffix aufweisen.

Die [WatchKitCatalog](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/) Beispiel verfügt über eine Reihe von nummerierten Bildern im Watch-app-Projekt mit der **Bus** Präfix:

![](image-images/asset-bus-animation-sml.png "Im Beispiel WatchKitCatalog hat eine Reihe von nummerierten Bildern im Watch-app-Projekt mit dem Bus-Präfix")

Zum Anzeigen dieser Bilder als eine Animation zum ersten Mal laden das Bild mithilfe `SetImage` mit dem Präfixnamen und rufen Sie dann `StartAnimating`:

```csharp
animatedImage.SetImage ("Bus");
animatedImage.StartAnimating ();
```

Rufen Sie `StopAnimating` auf das Image-Steuerelement, um die Animation Schleife zu beenden:

```csharp
animatedImage.StopAnimating ();
```


<a name="cache" />

## <a name="appendix-caching-images-watchos-1"></a>Anhang: Zwischenspeichern von Bildern (WatchOS 1)

> [!IMPORTANT]
> WatchOS 3-apps, die vollständig auf dem Gerät ausgeführt werden. Die folgenden Informationen sind WatchOS 1 nur für apps.

Wenn die Anwendung wiederholt ein Bild verwendet, die in der Erweiterung gespeichert (oder heruntergeladen wurde), ist es möglich, das Bild im Speicher für die Überwachung, zum Erhöhen der Leistung für nachfolgende zeigt zwischengespeichert.

Verwenden Sie die `WKInterfaceDevice`s `AddCachedImage` Methode, um das Bild in der Apple Watch übertragen und dann `SetImage` mit dem Image-Name-Parameter als Zeichenfolge angezeigt:

```csharp
var device = WKInterfaceDevice.CurrentDevice;
using (var image = UIImage.FromBundle ("Bumblebee")) {
    if (!device.AddCachedImage (image, "Bumblebee")) {
            Console.WriteLine ("Image cache full.");
        } else {
            cachedImage.SetImage ("Bumblebee");
        }
    }
}
```

Sie können den Inhalt des Caches für Images in Code mit Abfragen `WKInterfaceDevice.CurrentDevice.WeakCachedImages`.


### <a name="managing-the-cache"></a>Cacheverwaltung

Der Cache-Größe von ca. 20 MB. Es über app-Neustart beibehalten wird, und wenn es voll ist es, bleibt Ihnen überlassen löschen, Dateien mithilfe von `RemoveCachedImage` oder `RemoveAllCachedImages` Methoden für die `WKInterfaceDevice.CurrentDevice` Objekt.



## <a name="related-links"></a>Verwandte Links

- [WatchKitCatalog (Beispiel)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)
- [Apple Image doc](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/Images.html)
