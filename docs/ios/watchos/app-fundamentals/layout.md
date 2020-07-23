---
title: Arbeiten mit dem watchos-Layout in xamarin
description: In diesem Dokument wird beschrieben, wie Sie ein watchos-Layout mithilfe von xamarin erstellen. Hier werden Schnittstellen Controller, Gruppen, Trennzeichen und Inhalts Steuerelemente erläutert.
ms.prod: xamarin
ms.assetid: BEDB62A1-2249-4459-986F-413A41E63DF0
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/17/2017
ms.openlocfilehash: 345c05a439423474644ac64ef86f9adc580ab0b1
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86937721"
---
# <a name="working-with-watchos-layout-in-xamarin"></a>Arbeiten mit dem watchos-Layout in xamarin

Das Entwerfen von Layouts für die Apple Watch [Bildschirmgrößen](~/ios/watchos/app-fundamentals/screen-sizes.md) stellt besondere Herausforderungen dar.

## <a name="design-tips"></a>Entwurfs Tipps

Der wichtigste Punkt ist: machen Sie Ihre Benutzeroberfläche auf einem kleinen Bildschirm mit einem großen Finger lesbar und nutzbar. Gehen Sie beim Entwerfen für den **IOS-Simulator** (der sehr groß erscheint) und einem **Mauszeiger** (der mit kleinen Berührungs Zielen funktioniert) nicht in den Trap.

- Verwenden Sie einen schwarzen Hintergrund. Dadurch wird die Illusion eines größeren Bildschirms mit dem schwarzen Bild der Uhr angezeigt.

- Nicht um das Bildschirmlayout herum Auffüllen: das Ecke bildet eine natürliche visuelle Auffüllung.

- Konzentrieren Sie sich auf die Lesbarkeit. Verwenden Sie Schriftart Größen und Farben mit bedacht, um sicherzustellen, dass der Text lesbar ist. Verwenden Sie die integrierten Text Stile, um die automatische Unterstützung dynamischer Typen zu erhalten.

![Beispiel für die Unterstützung dynamischer Typen](layout-images/type.png)

- Konzentrieren Sie sich auf die Berührungs Zielgrößen. Schaltflächen/Einfügbare Tabellenzeilen mit Text Bezeichnungen sollten den gesamten Bildschirm überspannen. Apple sagt, dass Sie nicht mehr als drei Elemente nebeneinander platzieren können, und wenn Sie Symbole und keine Text Bezeichnungen verwenden.

- Verwenden Sie das- [ `Menu` Steuer](~/ios/watchos/user-interface/menu.md) Element, um weniger häufig verwendete Funktionen verfügbar zu machen, um Ihren app-Entwurf klar und präzise zu halten.

## <a name="implementation"></a>Implementierung

Das Watch Kit umfasst die folgenden Steuerelemente, die Ihnen bei der Erstellung ansprechender App-Layouts helfen:

### <a name="interface-controller"></a>Schnittstellen Controller

Die `WKInterfaceController` ist die Basisklasse all Ihre Szenen.

Die Entwurfs Oberfläche für den Schnittstellen Controller verhält sich wie eine vertikale **Gruppe**: Sie können andere Steuerelemente auf den Schnittstellen Controller ziehen, und Sie werden automatisch über den anderen festgelegt:

![Steuerelemente werden automatisch über die andere festgelegt.](layout-images/controller-scene.png)

Sie können die Eigenschaften **Position** und **Größe** jedes Steuer Elements festlegen, um die Darstellung zu steuern:

![Festlegen der Positions-und Größen Eigenschaften für jedes Steuerelement](layout-images/positionsize-attributes.png)

Wenn die Größe **relativ zum Container** auf festgelegt ist, können Sie einen proportionalen Wert und eine Offset Anpassung bereitstellen. Dieser Screenshot zeigt eine Schaltfläche, die für die Verwendung von 80% der Breite des Bildschirms (**0,8**) festgelegt wurde:

![Angeben eines proportionalen Werts und einer Offset Anpassung](layout-images/button-attributes.png)

### <a name="group"></a>Group

`WKInterfaceGroup`ein einfacher Layoutcontainer, der so konfiguriert werden kann, dass Steuerelemente vertikal oder horizontal gestapelt werden. Sie enthält standardmäßig Abstände zwischen den einzelnen Steuerelementen, aber Sie können den Abstand (und die Insets) im **Attribut** Inspektor ändern.

![Ändern des Abstands und der insets im Attribute Inspector](layout-images/group-attributes.png)

Gruppen können selbst angepasst und in Relation zu den Steuerelementen herum positioniert werden, und Gruppen können zum Erstellen komplexer Layouts gruppiert werden.

![Gruppen können zum Erstellen komplexer Layouts eingefügt werden.](layout-images/group-scene.png)

### <a name="separator"></a>Trennzeichen

Das Trennzeichen Steuerelement soll Ihnen helfen, visuelle Anleitungen in Ihrem Layout bereitzustellen. Verwenden Sie Trennzeichen (oder Hintergrundfarben oder Bilder), um dem Benutzer zu helfen, zu verstehen, welcher Inhalt auf dem Bildschirm verknüpft ist.

![Beispiel für die Verwendung von Trennzeichen](layout-images/separator-scene.png)

Beachten Sie, dass die blauen und grünen Trennzeichen, die nicht die vollständige Breite des Bildschirms verwenden, mit **fester** oder **relativ zu Container** Größen konfiguriert wurden.

### <a name="content-controls"></a>Inhaltssteuerelemente

Ohne das `Label` -,-,-, `Image` `Button` `Switch` `Slider` `Map` -,-und andere Steuer [Elemente](~/ios/watchos/user-interface/index.md)wäre kein Layout ohne das Layout.
Diese können in ihren Layouts mithilfe von **Gruppen** oder den Positions-und Größen Einstellungen der einzelnen Steuerelemente positioniert werden.

## <a name="related-links"></a>Ähnliche Themen

- [Watchkitcatalog (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog)
- [Apple-layoutverweis](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/WatchHumanInterfaceGuidelines/Layout.html)
- [Farb & typografiereferenz von Apple](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/WatchHumanInterfaceGuidelines/ColorandTypography.html)
