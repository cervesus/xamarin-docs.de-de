---
title: Arbeiten mit WatchOS Bildschirmgrößen in Xamarin
description: Dieses Dokument beschreibt das Arbeiten mit verschiedenen Bildschirmgrößen von WatchOS. Es wird erläutert, die WatchOS-Benutzeroberflächen-Designer, der WatchOS-Simulator und Bildressourcen.
ms.prod: xamarin
ms.assetid: 840DF939-2F59-4ABA-87D8-92AAC8A92BC4
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
ms.openlocfilehash: b2f4cc71c1993e51ed55b51edd7c50d393e60873
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61412925"
---
# <a name="working-with-watchos-screen-sizes-in-xamarin"></a>Arbeiten mit WatchOS Bildschirmgrößen in Xamarin

Apple Watch ist in zwei Bildschirmgrößen verfügbar:

- **38mm**
  - 136 x 170 logischen Pixeln (272 340 x physische Pixel)

- **42mm**
  - 156 x 195 logischen Pixeln (312 x 390 physische Pixel).

Sie sollten die Größe des Bildschirms berücksichtigt beim Entwerfen und Testen Ihrer apps durchführen.

## <a name="watchos-interface-designer"></a>WatchOS-Benutzeroberflächen-Designer

Wird standardmäßig der Visual Studio für Mac-Designer angezeigt wird. sehen Sie sich Schnittstellencontroller am **Any Apple Watch-**.

![](screen-sizes-images/screen-any-sml.png "Der Designer zeigt ansehen Schnittstellencontroller an beliebige Apple Watch")

Verwenden Sie das Menü "Größe", um bearbeiten und Vorschau das Storyboard an eines der verfügbaren Bildschirmgrößen: **38mm** oder **42mm**:

![](screen-sizes-images/screen-menu-sml.png "Die Größe 38mm oder -42mm auswählen")

Die größeren Bildschirm wird manchmal Inhalt gerendert werden, die auf den kleineren Bildschirm abgeschnitten oder ausgeblendet ist.
Achten Sie darauf, dass Sie auf die beiden Größen zu testen.


### <a name="interface-design"></a>Schnittstellenentwurf

Ihre app sollte zeigt den gleichen Inhalt auf dem Bildschirm unabhängig von Größe, sollte und Trennzeichen erweitern oder verkleinern die Elemente nach Bedarf. Sie sollten in der Visual Studio für Mac-Designer im Attribute Inspector verwenden **relativ zum Container** oder **Größe an Inhalt anpassen** Adressregister feste Größen.

![](screen-sizes-images/sizeattributepanel-sml.png "Verwenden Sie relativ zum Container oder Größe an Inhalt anpassen Adressregister feste Größen")

Da auf der Bildschirm sehen Sie sich von einem schwarzen Rahmen umgeben ist, wird die Bereitstellung des Abstands um Ihre Schnittstelle nicht empfohlen. Können Sie Elemente rest für den Rand des Bildschirms, und lassen die Blende einen natürlichen Rahmen um die app bilden.


## <a name="watchos-simulator"></a>WatchOS-Simulator

Beim Testen auf dem Simulator kann einfach wechseln zwischen den zwei Bildschirmgrößen, die mit der **Hardware > Gerät** Menü.

![](screen-sizes-images/simulator.png "Der Simulator kann zwischen den zwei Bildschirmgrößen, die über das Menü Hardwaregerät wechseln.")


## <a name="image-resources"></a>Bildressourcen

Sie sollten mehrere Bildanlagen verwenden, wenn ein einziges Medienobjekt nicht gut für die verschiedenen Größen aussieht. Ressourcenkataloge Image zulassen für separate Bitmaps für jede Größe angegeben werden:

![](screen-sizes-images/images-xcassets.png "Bildbearbeitung für Asset-Katalog")

```csharp
// specify the asset name, the correct size will automatically be loaded
staticImage.SetImage(UIImage.FromBundle("Walkway"));
```

Alternativ verwenden Sie Code zum bestimmen die Größe des Bildschirms, und laden die verschiedene Bildern vollständig:

```csharp
bool large = WKInterfaceDevice.CurrentDevice.ScreenBounds.Size.Width > 136.0;
// Load image depending on screen size
using (var image = UIImage.FromBundle (large ? "42mm-Walkway" : "38mm-Walkway"))
{
   myImage.SetImage (image);

}
```

Weitere Informationen zum Verwenden der [image-Steuerelement](~/ios/watchos/user-interface/image.md).



## <a name="related-links"></a>Verwandte Links

- [Einführung in watchOS 3](~/ios/watchos/platform/introduction-to-watchos3/index.md)
