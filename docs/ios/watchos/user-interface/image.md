---
title: watchos-Image-Steuerelemente in xamarin
description: In diesem Dokument wird die Verwendung von Image-Steuerelementen in einer watchos-Anwendung beschrieben, die mit xamarin erstellt wurde. Es erläutert das wkinterfaceimage-Steuerelement, die Methode "-timage", das Hinzufügen von Bildern zu einer Watch-Erweiterung, Animationen und mehr.
ms.prod: xamarin
ms.assetid: B741C207-3427-46F3-9C90-A52BF8933FA4
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/17/2017
ms.openlocfilehash: f9367eda7651ca61a8a3cb0928ad11cb320faab6
ms.sourcegitcommit: 699de58432b7da300ddc2c85842e5d9e129b0dc5
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2019
ms.locfileid: "70769958"
---
# <a name="watchos-image-controls-in-xamarin"></a>watchos-Image-Steuerelemente in xamarin

WatchOS bietet eine [`WKInterfaceImage`](xref:WatchKit.WKInterfaceImage) -Steuerelement zum Anzeigen von Bildern und einfache Animationen. Einige Steuerelemente können auch über ein Hintergrundbild verfügen (z. b. Schaltflächen, Gruppen und Schnittstellen Controller).

![](image-images/image-walkway.png "Apple Watch Anzeigen eines Bildes") ![](image-images/image-animation.png "Apple Watch mit einfache Animation")
<!-- watch image courtesy of http://infinitapps.com/bezel/ -->

Verwenden Sie Asset catalog-images, um Images zu Watch Kit-apps hinzuzufügen.
Nur **@2x** Versionen sind erforderlich, da alle Geräte Retina wird ansehen.

![](image-images/asset-universal-sml.png "Nur 2X-Versionen sind erforderlich, da alle Überwachungsgeräte Retina-anzeigen aufweisen.")

Es wird empfohlen, sicherzustellen, dass die Bilder selbst die richtige Größe für die Anzeige Anzeige darstellen. *Vermeiden* Sie die Verwendung von Images mit falscher Größe (besonders große) und die Skalierung, um Sie auf dem Bildschirm anzuzeigen.

Sie können die Watch Kit-Größen (38mm und 42mm) in einem Asset Catalog-Image verwenden, um unterschiedliche Images für jede Anzeige Größe anzugeben.

![](image-images/asset-watch-sml.png "Sie können das Watch Kit mit der Größe 38mm und 42mm in einem Asset Catalog-Image verwenden, um unterschiedliche Images für jede Anzeige Größe anzugeben.")

## <a name="images-on-the-watch"></a>Bilder auf der Uhr

Die effizienteste Möglichkeit zum Anzeigen von Bildern besteht darin, *Sie in das Überwachungs-App-Projekt einzuschließen* und `SetImage(string imageName)` mithilfe der-Methode anzuzeigen.

Beispielsweise verfügt das [watchkitcatalog](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog/) -Beispiel über eine Reihe von Bildern, die einem Asset-Katalog im Überwachungs-App-Projekt hinzugefügt werden:

![](image-images/asset-whale-sml.png "Das watchkitcatalog-Beispiel enthält eine Reihe von Bildern, die einem Asset-Katalog im Überwachungs-App-Projekt hinzugefügt werden.")

Diese können mithilfe `SetImage` des Parameters "String Name" effizient geladen und auf der Überwachung angezeigt werden:

```csharp
myImageControl.SetImage("Whale");
myOtherImageControl.SetImage("Worry");
```

### <a name="background-images"></a>Hintergrundbilder

Die gleiche Logik gilt für die `SetBackgroundImage (string imageName)` in den `Button`Klassen `Group`, und `InterfaceController` . Die beste Leistung wird erzielt, indem die Images in der Watch-APP selbst gespeichert werden.

## <a name="images-in-the-watch-extension"></a>Bilder in der Watch-Erweiterung

Zusätzlich zum Laden von Bildern, die in der Watch-APP selbst gespeichert sind, können Sie Bilder aus dem Erweiterungspaket zur Anzeige an die Watch-App senden (oder Sie können Images von einem Remote Speicherort herunterladen und anzeigen).

Wenn Sie Bilder aus der Watch-Erweiterung laden `UIImage` möchten, erstellen Sie `SetImage` Instanzen, `UIImage` und rufen Sie dann mit dem Objekt auf.

Beispielsweise hat das [watchkitcatalog](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog) -Beispiel ein Image mit dem Namen **Bumblebee** im Überwachungs Erweiterungsprojekt:

![](image-images/asset-bumblebee-sml.png "Das watchkitcatalog-Beispiel enthält ein Image mit dem Namen \"Bumblebee\" im Überwachungs Erweiterungsprojekt.")

Der folgende Code führt zu folgendem:

- das Bild, das in den Arbeitsspeicher geladen wird, und
- wird auf der Überwachung angezeigt.

```csharp
using (var image = UIImage.FromBundle ("Bumblebee")) {
    myImageControl.SetImage (image);
}
```

## <a name="animations"></a>Animationen

Um einen Satz von Bildern zu animieren, sollten Sie alle mit dem gleichen Präfix beginnen und über ein numerisches Suffix verfügen.

Das [watchkitcatalog](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog) -Beispiel enthält eine Reihe nummerierter Images im Watch-App-Projekt mit dem **Bus** -Präfix:

![](image-images/asset-bus-animation-sml.png "Das watchkitcatalog-Beispiel enthält eine Reihe nummerierter Images im Watch-App-Projekt mit dem Bus-Präfix.")

Wenn Sie diese Bilder als Animation anzeigen möchten, laden Sie das Bild `SetImage` zuerst mit dem Präfix Namen, `StartAnimating`und klicken Sie dann auf:

```csharp
animatedImage.SetImage ("Bus");
animatedImage.StartAnimating ();
```

Ruft `StopAnimating` das Image-Steuerelement auf, um die Animations Schleife zu verhindern:

```csharp
animatedImage.StopAnimating ();
```

<a name="cache" />

## <a name="appendix-caching-images-watchos-1"></a>Anhang: Zwischenspeichern von Images (watchos 1)

> [!IMPORTANT]
> watchos 3-apps werden vollständig auf dem Gerät ausgeführt. Die folgenden Informationen sind nur für watchos 1-apps vorgesehen.

Wenn die Anwendung wiederholt ein Bild verwendet, das in der Erweiterung gespeichert ist (oder heruntergeladen wurde), ist es möglich, das Image im Speicher der Überwachung zwischenzuspeichern, um die Leistung für nachfolgende anzeigen zu erhöhen.

Verwenden Sie `WKInterfaceDevice`die `AddCachedImage` s-Methode, um das Bild auf die Überwachung zu über `SetImage` tragen, und verwenden Sie dann mit dem Image Name-Parameter als Zeichenfolge, um es anzuzeigen:

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

Sie können den Inhalt des Image Caches im Code mithilfe `WKInterfaceDevice.CurrentDevice.WeakCachedImages`von Abfragen.

### <a name="managing-the-cache"></a>Verwalten des Caches

Der Cache ist ungefähr 20 MB groß. Sie wird über APP-Neustarts hinweg beibehalten, und wenn Sie ausgefüllt wird, liegt es in ihrer Verantwortung, `RemoveCachedImage` Dateien `RemoveAllCachedImages` mit der- `WKInterfaceDevice.CurrentDevice` Methode oder der-Methode für das-Objekt zu löschen.

## <a name="related-links"></a>Verwandte Links

- [WatchKitCatalog (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog)
- [Image-Dokument von Apple](https://developer.apple.com/documentation/watchkit/wkinterfaceimage)
