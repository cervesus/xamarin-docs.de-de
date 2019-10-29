---
title: Arbeiten mit dem watchos-Layout in xamarin
description: In diesem Dokument wird beschrieben, wie Sie ein watchos-Layout mithilfe von xamarin erstellen. Hier werden Schnittstellen Controller, Gruppen, Trennzeichen und Inhalts Steuerelemente erläutert.
ms.prod: xamarin
ms.assetid: BEDB62A1-2249-4459-986F-413A41E63DF0
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/17/2017
ms.openlocfilehash: 568d1e354d0ee840aeed980d6e8cc6b83068a1c8
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73001547"
---
# <a name="working-with-watchos-layout-in-xamarin"></a>Arbeiten mit dem watchos-Layout in xamarin

Das Entwerfen von Layouts für die Apple Watch [Bildschirmgrößen](~/ios/watchos/app-fundamentals/screen-sizes.md) stellt besondere Herausforderungen dar.

## <a name="design-tips"></a>Entwurfs Tipps

Der wichtigste Punkt ist: machen Sie Ihre Benutzeroberfläche auf einem kleinen Bildschirm mit einem großen Finger lesbar und nutzbar. Gehen Sie beim Entwerfen für den **IOS-Simulator** (der sehr groß erscheint) und einem **Mauszeiger** (der mit kleinen Berührungs Zielen funktioniert) nicht in den Trap.

- Verwenden Sie einen schwarzen Hintergrund. Dadurch wird die Illusion eines größeren Bildschirms mit dem schwarzen Bild der Uhr angezeigt.

- Nicht um das Bildschirmlayout herum Auffüllen: das Ecke bildet eine natürliche visuelle Auffüllung.

- Konzentrieren Sie sich auf die Lesbarkeit. Verwenden Sie Schriftart Größen und Farben mit bedacht, um sicherzustellen, dass der Text lesbar ist. Verwenden Sie die integrierten Text Stile, um die automatische Unterstützung dynamischer Typen zu erhalten.

![](layout-images/type.png "Example of Dynamic Type support")

- Konzentrieren Sie sich auf die Berührungs Zielgrößen. Schaltflächen/Einfügbare Tabellenzeilen mit Text Bezeichnungen sollten den gesamten Bildschirm überspannen. Apple sagt, dass Sie nicht mehr als drei Elemente nebeneinander platzieren können, und wenn Sie Symbole und keine Text Bezeichnungen verwenden.

- Verwenden Sie das [`Menu`-Steuer](~/ios/watchos/user-interface/menu.md) Element, um weniger häufig verwendete Funktionen verfügbar zu machen, um Ihren app-Entwurf klar und präzise zu halten.

## <a name="implementation"></a>Implementierung

Das Watch Kit umfasst die folgenden Steuerelemente, die Ihnen bei der Erstellung ansprechender App-Layouts helfen:

### <a name="interface-controller"></a>Schnittstellen Controller

Der `WKInterfaceController` ist die Basisklasse in ihren Kulissen.

Die Entwurfs Oberfläche für den Schnittstellen Controller verhält sich wie eine vertikale **Gruppe**: Sie können andere Steuerelemente auf den Schnittstellen Controller ziehen, und Sie werden automatisch über den anderen festgelegt:

![](layout-images/controller-scene.png "Controls are automatically laid-out one above the other")

Sie können die Eigenschaften **Position** und **Größe** jedes Steuer Elements festlegen, um die Darstellung zu steuern:

![](layout-images/positionsize-attributes.png "Set the Position and Size properties on each control")

Wenn die Größe **relativ zum Container** auf festgelegt ist, können Sie einen proportionalen Wert und eine Offset Anpassung bereitstellen. Dieser Screenshot zeigt eine Schaltfläche, die für die Verwendung von 80% der Breite des Bildschirms (**0,8**) festgelegt wurde:

![](layout-images/button-attributes.png "Provide a proportional value and an offset adjustment")

### <a name="group"></a>Gruppieren

`WKInterfaceGroup` ist ein einfacher Layoutcontainer, der so konfiguriert werden kann, dass Steuerelemente vertikal oder horizontal gestapelt werden. Sie enthält standardmäßig Abstände zwischen den einzelnen Steuerelementen, aber Sie können den Abstand (und die Insets) im **Attribut** Inspektor ändern.

![](layout-images/group-attributes.png "Modify the spacing and insets in the Attributes inspector")

Gruppen können selbst angepasst und in Relation zu den Steuerelementen herum positioniert werden, und Gruppen können zum Erstellen komplexer Layouts gruppiert werden.

![](layout-images/group-scene.png "Groups can be nested to create complex layouts")

### <a name="separator"></a>Trennzeichen

Das Trennzeichen Steuerelement soll Ihnen helfen, visuelle Anleitungen in Ihrem Layout bereitzustellen. Verwenden Sie Trennzeichen (oder Hintergrundfarben oder Bilder), um dem Benutzer zu helfen, zu verstehen, welcher Inhalt auf dem Bildschirm verknüpft ist.

![](layout-images/separator-scene.png "Example of Separator usage")

Beachten Sie, dass die blauen und grünen Trennzeichen, die nicht die vollständige Breite des Bildschirms verwenden, mit **fester** oder **relativ zu Container** Größen konfiguriert wurden.

### <a name="content-controls"></a>Inhaltssteuerelemente

Es ist kein Layout ohne die `Label`, `Image`, `Button`, `Switch`, `Slider`, `Map`und anderen Steuer [Elementen](~/ios/watchos/user-interface/index.md)fertig.
Diese können in ihren Layouts mithilfe von **Gruppen** oder den Positions-und Größen Einstellungen der einzelnen Steuerelemente positioniert werden.

## <a name="related-links"></a>Verwandte Links

- [Watchkitcatalog (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog)
- [Apple-layoutverweis](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/WatchHumanInterfaceGuidelines/Layout.html)
- [Farb & typografiereferenz von Apple](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/WatchHumanInterfaceGuidelines/ColorandTypography.html)
