---
title: Arbeiten mit WatchOS Layout in Xamarin
description: In diesem Dokument wird beschrieben, wie ein WatchOS-Layout mithilfe von Xamarin erstellt wird. Es wird erläutert, Schnittstellencontroller, Gruppen, Trennzeichen und ContentControl-Elemente.
ms.prod: xamarin
ms.assetid: BEDB62A1-2249-4459-986F-413A41E63DF0
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
ms.openlocfilehash: 9a9bec364990f683a59e6ddce1a536950cdf3861
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/07/2018
ms.locfileid: "53059612"
---
# <a name="working-with-watchos-layout-in-xamarin"></a>Arbeiten mit WatchOS Layout in Xamarin

Entwerfen von Layouts für die Apple Watch- [Bildschirmgrößen](~/ios/watchos/app-fundamentals/screen-sizes.md) besondere Herausforderungen bereit.

## <a name="design-tips"></a>Tipps für den Entwurf

Der wichtigste Punkt ist: treffen Sie Ihre Benutzeroberfläche lesbar und auf einem Bildschirm kleine sehen Sie sich mit einem großen Finger verwendet. Tappen Sie nicht in der Trap beim Entwurf für die **iOS-Simulator** (wird angezeigt, sehr groß) und ein **Mauszeiger** (das funktioniert mit kleinen Touch-Ziele)!

- Erhält einen schwarzen Hintergrund verwenden – die Illusion von einem größeren Bildschirm mit der Apple Watch schwarzen Rahmen erstellt.

- Führen Sie für Ihre Bildschirmlayouts nicht aufgefüllt: die Blende bildet natürlich visual Auffüllung.

- Konzentrieren sich auf die Lesbarkeit. Verwenden Sie Schriftgrößen und Farben mit Bedacht, stellen Sie sicher, dass der Text lesbar ist. Verwenden Sie die integrierte Text-Formate, um automatische Typ "Dynamic"-Unterstützung zu erhalten.

![](layout-images/type.png "Beispiel für die Unterstützung von dynamischen Typen")

- Touch-zielgrößen darauf konzentrieren. Schaltflächen/tappable Tabellenzeilen mit Beschriftungen, die den gesamten Bildschirm umfassen soll. Apple sagt "nie mehr als drei Elemente-Seite-an-Seite platzieren", und wenn Sie Symbole und nicht die Beschriftungen verwenden.

- Verwenden der [ `Menu` Steuerelement](~/ios/watchos/user-interface/menu.md) zu machen, die weniger häufig verwendete Funktionen, Ihr app-Design klaren und präzisen zu halten.


## <a name="implementation"></a>Implementierung

Sehen Sie sich Kit umfasst die folgenden Steuerelemente, damit Sie attraktive sehen Sie sich app-Layouts erstellen können:

### <a name="interface-controller"></a>Schnittstellencontroller

Die `WKInterfaceController` ist die Basisklasse aller den Szenen.

Die Entwurfsoberfläche für die schnittstellencontrollers verhält sich wie ein vertikales **Gruppe**: können Sie andere Steuerelemente in der schnittstellencontrollers ziehen und sie werden automatisch gelayoutet eine über der anderen:

![](layout-images/controller-scene.png "Steuerelemente sind automatisch gelayoutet eine über der anderen")

Sie können festlegen, die **Position** und **Größe** Eigenschaften für jedes Steuerelement, um ihre Darstellung zu steuern:

![](layout-images/positionsize-attributes.png "Legen Sie die Position und Größe-Eigenschaften für jedes Steuerelement")

Wenn die Größe festgelegt ist, um **relativ zum Container** können Sie angeben, proportionaler Wert und eine Anpassung der Offset. Dieser Screenshot zeigt eine Schaltfläche, die festgelegt wurde, verwenden Sie 80 % der Breite für die Überwachung des Bildschirms (**0,8**):

![](layout-images/button-attributes.png "Geben Sie proportionaler Wert und eine Offset-Anpassung")


### <a name="group"></a>Gruppieren

`WKInterfaceGroup` ist ein einfaches Layout-Container, der konfiguriert werden kann, um die Aufrufliste vertikal oder horizontal steuert. Es enthält Abstand zwischen den Steuerelementen in der Standardeinstellung, aber Sie können den Abstand (und die Abstände) ändern, der **Attribute** Inspector.

![](layout-images/group-attributes.png "Ändern der Abstände und der Abstände im Attributes inspector")

Gruppen können selbst werden Größe und relativ zu den Steuerelementen zu positioniert, und Servergruppen können geschachtelt sein, um komplexe Layouts zu erstellen.

![](layout-images/group-scene.png "Gruppen können geschachtelt werden, um komplexe Layouts zu erstellen.")


### <a name="separator"></a>Trennzeichen

Das Separator-Steuerelement soll darstellungsleitfaden im Layout zu bieten. Verwenden Sie die Trennzeichen (oder Hintergrundfarben oder Bilder) können Sie die Benutzer wissen, welcher Inhalt auf dem Bildschirm zusammenhängt.

![](layout-images/separator-scene.png "Beispiel für die Verwendung von Trennzeichen")

Beachten Sie die blauen und grünen Trennzeichen, die nicht die gesamte Breite des Bildschirms verwenden entweder konfiguriert wurden **Fixed** oder **relativ zum Container** Größen.

### <a name="content-controls"></a>Inhaltssteuerelemente

Kein Layout wäre vollständig ohne die `Label`, `Image`, `Button`, `Switch`, `Slider`, `Map`, und [andere Steuerelemente](~/ios/watchos/user-interface/index.md).
Diese können in Ihre Layouts mit positioniert werden **Gruppen** oder die Position und Größe auf jedes Steuerelement.



## <a name="related-links"></a>Verwandte Links

- [WatchKitCatalog (Beispiel)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)
- [Apple Layout-Referenz](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/WatchHumanInterfaceGuidelines/Layout.html)
- [Farb & Typografie von Apple zu verweisen.](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/WatchHumanInterfaceGuidelines/ColorandTypography.html)
