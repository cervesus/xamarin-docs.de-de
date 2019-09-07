---
title: Arbeiten mit watchos-Bildschirmgrößen in xamarin
description: In diesem Dokument wird beschrieben, wie Sie mit verschiedenen watchos-Bildschirmgrößen arbeiten. Er erläutert den watchos-Schnittstellen-Designer, den watchos-Simulator und Bild Ressourcen.
ms.prod: xamarin
ms.assetid: 840DF939-2F59-4ABA-87D8-92AAC8A92BC4
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/17/2017
ms.openlocfilehash: e9c87b76dc6845962450b8cb6fab921ea1748832
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70768328"
---
# <a name="working-with-watchos-screen-sizes-in-xamarin"></a>Arbeiten mit watchos-Bildschirmgrößen in xamarin

Apple Watch ist in zwei Bildschirmgrößen verfügbar:

- **38mm**
  - 136 x 170 logische Pixel (272 x 340 physische Pixel)

- **42 mm**
  - 156 x 195 logische Pixel (312 x 390 physische Pixel).

Beim Entwerfen und Testen Ihrer Apps sollten Sie die Bildschirmgröße berücksichtigen.

## <a name="watchos-interface-designer"></a>watchos-Schnittstellen-Designer

Standardmäßig zeigt der Visual Studio für Mac-Designer in **jeder Apple Watch**Überwachungsschnittstellen Controller an.

![](screen-sizes-images/screen-any-sml.png "Der Designer zeigt die Überwachungsschnittstellen Controller an allen Apple Watch")

Verwenden Sie das Menü Größe, um das Storyboard in einer der verfügbaren Bildschirmgrößen zu bearbeiten und in der Vorschau anzuzeigen: **38mm** oder **42 mm**:

![](screen-sizes-images/screen-menu-sml.png "Auswählen der Größe von 38mm oder 42 mm")

In der größeren Bildschirmgröße werden manchmal Inhalte angezeigt, die auf dem kleineren Bildschirm abgeschnitten/ausgeblendet werden.
Achten Sie darauf, beide Größen zu testen.

### <a name="interface-design"></a>Schnittstellenentwurf

Ihre APP sollte denselben Inhalt unabhängig von der Größe auf dem Bildschirm anzeigen und die Elemente nach Bedarf erweitern oder verkleinern. Im Visual Studio für Mac-Designer sollten Sie im Attribut Inspektor **relativ zum Container** oder zur Größe verwenden, **um den Inhalt** bevorzugt an die Größe fester Größe anzupassen.

![](screen-sizes-images/sizeattributepanel-sml.png "Verwenden Sie relativ zum Container oder zur Größe, um Inhalte für die Größe von fester Größe anzupassen.")

Da der Bildschirm "überwachen" von einem schwarzen Bezel umgeben ist, wird die Bereitstellung der Oberfläche nicht empfohlen. Lassen Sie die Elemente sich am Bildschirmrand befinden, und lassen Sie das Bild einen natürlichen Rahmen um die APP bilden.

## <a name="watchos-simulator"></a>watchos-Simulator

Beim Testen auf dem Simulator können Sie problemlos zwischen den beiden Bildschirmgrößen wechseln, indem Sie das Menü **Hardware > Gerät** verwenden.

![](screen-sizes-images/simulator.png "Der Simulator kann mit dem Menü Hardware Gerät zwischen den beiden Bildschirmgrößen wechseln.")

## <a name="image-resources"></a>Bild Ressourcen

Sie sollten mehrere Bildmedien Objekte verwenden, wenn ein einzelnes Medienobjekt nicht in unterschiedlichen Größen gut aussieht. Mit Image-Asset-Katalogen können separate Bitmaps für jede Größe angegeben werden:

![](screen-sizes-images/images-xcassets.png "Bild-Asset-Katalog-Editor")

```csharp
// specify the asset name, the correct size will automatically be loaded
staticImage.SetImage(UIImage.FromBundle("Walkway"));
```

Alternativ können Sie Code verwenden, um die Bildschirmgröße zu ermitteln und verschiedene Bilder zu laden:

```csharp
bool large = WKInterfaceDevice.CurrentDevice.ScreenBounds.Size.Width > 136.0;
// Load image depending on screen size
using (var image = UIImage.FromBundle (large ? "42mm-Walkway" : "38mm-Walkway"))
{
   myImage.SetImage (image);

}
```

Weitere Informationen finden Sie unter Verwenden des [Image-Steuer](~/ios/watchos/user-interface/image.md)Elements.

## <a name="related-links"></a>Verwandte Links

- [Einführung in watchOS 3](~/ios/watchos/platform/introduction-to-watchos3/index.md)
