---
title: Arbeiten mit WatchOS Layout in Xamarin
description: Dieses Dokument beschreibt, wie ein WatchOS Layout mithilfe von Xamarin erstellt wird. Es wird erläutert, Schnittstelle Controller, Gruppen, Trennzeichen und Content-Steuerelemente.
ms.prod: xamarin
ms.assetid: BEDB62A1-2249-4459-986F-413A41E63DF0
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 11ff5ec2fc8fe99a780a3d728d3d84af59794cea
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34790613"
---
# <a name="working-with-watchos-layout-in-xamarin"></a>Arbeiten mit WatchOS Layout in Xamarin

Entwerfen des Layouts für das Apple Watch- [Bildschirm Größen](~/ios/watchos/app-fundamentals/screen-sizes.md) stellt eine besondere Herausforderung dar.

## <a name="design-tips"></a>Tipps für den Entwurf

Wichtig ist: Stellen Sie Ihre Benutzeroberfläche lesbar und auf einem Bildschirm small überwachen, der mit einem großen Finger verwendbar. Nicht fallen in der Trap des Entwurfs für die **iOS-Simulator** (die angezeigt wird sehr große) und ein **Mauszeiger** (das funktioniert mit kleinen Touch-Ziele)!

- Schwarzen Hintergrund verwenden – die Illusion von einem größeren Bildschirm mit schwarzen Rahmen für die Überwachung erstellt.

- Werden nicht aufgefüllt, um das Bildschirmlayout - Blende bildet eine natürliche visual Auffüllung.

- Lesbarkeit darauf konzentrieren. Verwenden Sie Schriftart Größen und Farben, stellen Sie sicher, dass der Text gelesen werden kann. Verwenden Sie die integrierte Text-Formate, um automatische dynamischen Typ-Unterstützung zu erhalten.

![](layout-images/type.png "Beispiel für die Unterstützung von dynamischen Typen")

- Konzentrieren Sie Touch Ziel Größen aus. Schaltflächen/tappable Tabellenzeilen mit Beschriftungen sollte es sich um den gesamten Bildschirm umfassen. Apple sagt "nie mehr als drei Elemente-Seite-an-Seite platzieren", und wenn Sie Symbole und keine Bezeichnungen verwenden.

- Verwenden der [ `Menu` Steuerelement](~/ios/watchos/user-interface/menu.md) zu machen, die weniger häufig verwendete Funktionen des app-Entwurfs deaktivieren und präzise beibehalten.


## <a name="implementation"></a>Implementierung

Überwachen-Kit enthält die folgenden Steuerelemente, damit Sie attraktive Überwachungsfenster app Layouts erstellen können:

### <a name="interface-controller"></a>Schnittstellen-Controller

Die `WKInterfaceController` ist die Basisklasse aller Ihrer Szenen.

Die Entwurfsoberfläche für die Schnittstelle Controller verhält sich wie eine vertikale **Gruppe**: können Sie andere Steuerelemente in der Schnittstelle Controller ziehen und sie werden automatisch Layout übereinander:

![](layout-images/controller-scene.png "Steuerelemente werden automatisch Layout übereinander")

Sie können festlegen, die **Position** und **Größe** Eigenschaften für jedes Steuerelement, um ihre Darstellung zu steuern:

![](layout-images/positionsize-attributes.png "Festlegen Sie die Position und Größe Eigenschaften für jedes Steuerelement")

Wenn die Größe festgelegt ist, um **relativ zum Container** können Sie proportionaler Wert und einen Offset Anpassung bereitstellen. Diese bildschirmabbildung zeigt eine Schaltfläche, die festgelegt wurde, verwenden von 80 % der Breite des Bildschirms überwachen (**0,8**):

![](layout-images/button-attributes.png "Geben Sie proportionaler Wert und einen Offset Anpassung")


### <a name="group"></a>Gruppieren

`WKInterfaceGroup` ist ein einfache Layoutcontainer, der konfiguriert werden kann, um den Stapel vertikal oder horizontal steuert. Er enthält Abstand zwischen den Steuerelementen in der Standardeinstellung, aber Sie können den Abstand (und die Abstände) ändern, der **Attribute** Inspektor.

![](layout-images/group-attributes.png "Ändern der Abstände und der Abstände in der Attribute-Inspektor")

Gruppen können selbst angepasst und relativ zu den Steuerelementen darum positioniert und Gruppen können geschachtelt werden, um komplexe Layouts zu erstellen.

![](layout-images/group-scene.png "Gruppen können geschachtelt werden, um komplexe Layouts zu erstellen.")


### <a name="separator"></a>Trennzeichen

Das Separator-Steuerelement ist vorgesehen, um visuelle Führung im Layout bereitzustellen. Verwenden Sie als Trennzeichen (oder Hintergrundfarben oder Bilder) besser verstehen, welche Inhalte auf dem Bildschirm verknüpft ist.

![](layout-images/separator-scene.png "Beispiel für die Verwendung von Trennzeichen")

Beachten Sie, die blauen und grünen als Trennzeichen, die nicht die gesamte Breite des Bildschirms verwenden entweder mit konfiguriert wurden **Fixed** oder **relativ zum Container** Größen.

### <a name="content-controls"></a>Inhaltssteuerelemente

Kein Layout wäre vollständig ohne die `Label`, `Image`, `Button`, `Switch`, `Slider`, `Map`, und [andere Steuerelemente](~/ios/watchos/user-interface/index.md).
Diese können in Ihrer Layouts mit positioniert werden **Gruppen** oder die Position und Größe Einstellungen auf jedes Steuerelement.



## <a name="related-links"></a>Verwandte Links

- [WatchKitCatalog (Beispiel)](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchKitCatalog/)
- [Apple Layout-Referenz](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/WatchHumanInterfaceGuidelines/Layout.html)
- [Apple Farbe & Typografie verweisen](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/WatchHumanInterfaceGuidelines/ColorandTypography.html)
