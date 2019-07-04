---
title: WatchOS-Image-Steuerelemente in Xamarin
description: Dieses Dokument beschreibt, wie Sie Image-Steuerelemente in eine Anwendung für WatchOS mit Xamarin verwenden. Es wird erläutert, das Steuerelement WKInterfaceImage SetImage-Methode, eine Watch-Erweiterung, Animationen und weitere Bilder hinzugefügt.
ms.prod: xamarin
ms.assetid: B741C207-3427-46F3-9C90-A52BF8933FA4
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
ms.openlocfilehash: 30bb8f096384dd9f76e208fbd3dbef73cf53bb33
ms.sourcegitcommit: 8ecfa339d0f3e7687977bfe4fc96448942690183
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2019
ms.locfileid: "67558702"
---
# <a name="watchos-image-controls-in-xamarin"></a>WatchOS-Image-Steuerelemente in Xamarin

WatchOS bietet eine [`WKInterfaceImage`](xref:WatchKit.WKInterfaceImage) -Steuerelement zum Anzeigen von Bildern und einfache Animationen. Einige Steuerelemente können auch ein Bild als Hintergrund (z. B. Schaltflächen, Gruppen und Schnittstellencontroller) verfügen.

![](image-images/image-walkway.png "Apple Watch Anzeigen eines Bildes") ![](image-images/image-animation.png "Apple Watch mit einfache Animation")
<!-- watch image courtesy of http://infinitapps.com/bezel/ -->

Verwenden Sie Asset-Katalog-Images, um Bilder Watch-Kit-apps hinzufügen.
Nur **@2x** Versionen sind erforderlich, da alle Geräte Retina wird ansehen.

![](image-images/asset-universal-sml.png "Nur 2 X-Versionen sind erforderlich, da alle Geräte Retina wird ansehen")

Es ist empfiehlt sich, um sicherzustellen, dass die Bilder selbst die richtige Größe für die Überwachungsanzeige sind. *Vermeiden Sie* falsch große Bildern (insbesondere für große) verwenden und die Skalierung, um sie auf der Apple Watch anzuzeigen.

Sie können die Watch-Kit-Größen (38 mm und 42 mm) in einem Asset-Katalog-Image verwenden, verschiedene Bilder für jede Größe angegeben.

![](image-images/asset-watch-sml.png "Die Watch-Kit Größe 38 mm \"und\" 42 mm können in einem Asset-Katalog-Image Sie verschiedene Abbilder für jede Größe angeben")


## <a name="images-on-the-watch"></a>Bilder auf der Apple Watch

Ist die effizienteste Methode zum Anzeigen von Bildern *nehmen sie in der Watch-app-Projekt* und zeigen Sie sie mithilfe der `SetImage(string imageName)` Methode.

Z. B. die [WatchKitCatalog](https://developer.xamarin.com/samples/WatchKitCatalog/) Beispiel bietet eine Reihe von Bildern, die ein Asset-Katalog in der Watch-app-Projekt hinzugefügt:

![](image-images/asset-whale-sml.png "Das Beispiel WatchKitCatalog bietet eine Reihe von Images, die ein Asset-Katalog in der Watch-app-Projekt hinzugefügt")

Diese können effizient geladen und angezeigt werden, auf dem Dashboard sehen Sie sich mit `SetImage` mit dem Namen Zeichenfolgenparameter:

```csharp
myImageControl.SetImage("Whale");
myOtherImageControl.SetImage("Worry");
```

### <a name="background-images"></a>Hintergrundbilder

Die gleiche Logik gilt für die `SetBackgroundImage (string imageName)` auf die `Button`, `Group`, und `InterfaceController` Klassen. Bewährte Leistung wird erzielt, indem Sie die Images speichern, in der Watch-app selbst.


## <a name="images-in-the-watch-extension"></a>Bilder in der Watch-Erweiterung

Zusätzlich zum Laden von Bildern, die in der Watch-app selbst gespeichert sind, können Sie Bilder aus dem Paket für die Erweiterung an die Watch-app für die Anzeige senden (oder Sie könnten Herunterladen von Images von einem Remotestandort aus und zeigt diese).

Erstellen Sie zum Laden von Bildern aus der Watch-Erweiterung `UIImage` Instanzen aus, und rufen Sie anschließend `SetImage` mit der `UIImage` Objekt.

Z. B. die [WatchKitCatalog](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/) Beispiel verfügt über ein Image namens **Bumblebee** im Erweiterungsprojekt überwachen:

![](image-images/asset-bumblebee-sml.png "Das WatchKitCatalog-Beispiel enthält ein Image namens Bumblebee in der Watch-Erweiterungsprojekt")

Der folgende Code führt zu:

- das Bild in den Arbeitsspeicher geladen wird und
- Klicken Sie auf der Apple Watch angezeigt.

```csharp
using (var image = UIImage.FromBundle ("Bumblebee")) {
    myImageControl.SetImage (image);
}
```


## <a name="animations"></a>Animationen

Um einen Satz von Bildern zu animieren, sollten sie alle mit dem gleichen Präfix beginnen und ein numerisches Suffix haben.

Die [WatchKitCatalog](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/) Beispiel enthält eine Reihe von nummerierten Bildern in der Watch-app-Projekt mit der **Bus** Präfix:

![](image-images/asset-bus-animation-sml.png "Das WatchKitCatalog-Beispiel enthält eine Reihe von nummerierten Bildern im app-Projekt sehen Sie sich mit dem Bus-Präfix")

Zum Anzeigen dieser Images als eine Animation zuerst laden das Image mit `SetImage` mit dem Präfixnamen an und rufen Sie dann `StartAnimating`:

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

Wenn die Anwendung wiederholt ein Bild verwendet, die in der Erweiterung gespeichert (oder heruntergeladen wurde), ist es möglich, das Bild in der Apple Watch Speicher, zum Erhöhen der Leistung für nachfolgende zeigt zwischengespeichert.

Verwenden Sie die `WKInterfaceDevice`s `AddCachedImage` Methode, um das Bild auf der Apple Watch übertragen, und klicken Sie dann verwenden `SetImage` mit dem Image-Name-Parameter als Zeichenfolge angezeigt:

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


### <a name="managing-the-cache"></a>Verwalten des Caches

Der Cache ca. 20 MB. Es bleibt nach einem Neustart und wenn es voll ist es, ist Ihre Aufgabe können Sie Dateien löschen, `RemoveCachedImage` oder `RemoveAllCachedImages` Methoden für die `WKInterfaceDevice.CurrentDevice` Objekt.



## <a name="related-links"></a>Verwandte Links

- [WatchKitCatalog (Beispiel)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)
- [Apple Image-doc](https://developer.apple.com/documentation/watchkit/wkinterfaceimage)
