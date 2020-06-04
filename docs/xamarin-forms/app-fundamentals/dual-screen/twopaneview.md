---
title: Xamarin.Forms-Layout für Dual-Screen-Geräte
description: In diesem Artikel wird beschrieben, wie Sie mit „TwoPaneView“ in Xamarin.Forms die Benutzeroberfläche Ihrer App für Dual-Screen-Geräte wie Surface Duo und Surface Neo optimieren.
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 28d4b3da44cc1a022b70c0de0720be747e047f9f
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2020
ms.locfileid: "84138891"
---
# <a name="xamarinforms-dual-screen-layout"></a>Xamarin.Forms-Layout für Dual-Screen-Geräte

![](~/media/shared/preview.png "This API is currently pre-release")

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-dualscreendemos/)

Die `TwoPaneView`-Klasse stellt einen Container mit zwei Ansichten dar, die den Inhalt dem verfügbaren Platz entsprechend nebeneinander oder übereinander positionieren und in der Größe anpassen. `TwoPaneView` erbt von `Grid`. Diese Eigenschaften funktionieren also so, als würden sie auf ein Raster angewendet werden.

## <a name="set-up-twopaneview"></a>Einrichten von TwoPaneView

Die `TwoPaneView.Source`-Eigenschaft kann einen URI oder einen lokalen Dateipfad annehmen. Die Wiedergabe beginnt unmittelbar, nachdem das Medium geöffnet wurde:

```xaml
<ContentPage xmlns:dualScreen="clr-namespace:Xamarin.Forms.DualScreen;assembly=Xamarin.Forms.DualScreen">
    <dualScreen:TwoPaneView>
        <dualScreen:TwoPaneView.Pane1>
            <StackLayout>
                <Label Text="Pane1 Content" />
            </StackLayout>
        </dualScreen:TwoPaneView.Pane1>
        <dualScreen:TwoPaneView.Pane2>
            <StackLayout>
                <Label Text="Pane2 Content" />
            </StackLayout>
        </dualScreen:TwoPaneView.Pane2>
    </dualScreen:TwoPaneView>
</ContentPage>
```

## <a name="understand-twopaneview-modes"></a>Grundlegendes zu den TwoPaneView-Modi

Nur einer der folgenden Modi kann aktiv sein:

- `SinglePane`: Nur ein Bereich ist sichtbar.
- `Wide`: Beide Bereiche werden horizontal angeordnet. Ein Bereich befindet sich links, der andere rechts. Bei zwei Bildschirmen und einem Gerät im Hochformat wird dieser Modus verwendet.
- `Tall`: Beide Bereiche werden vertikal angeordnet. Ein Bereich befindet sich oben und der andere unten. Bei zwei Bildschirmen und einem Gerät im Querformat wird dieser Modus verwendet.

## <a name="control-twopaneview-when-its-only-on-one-screen"></a>Steuern von TwoPaneView bei einem Bildschirm

Die folgenden Eigenschaften werden angewendet, wenn `TwoPaneView` auf einem einzelnen Bildschirm verwendet wird:

- `MinTallModeHeight`: gibt die minimale Höhe an, die das Steuerelement aufweisen muss, um in den Tall-Modus wechseln zu können.
- `MinWideModeWidth`: gibt die minimale Breite an, die das Steuerelement aufweisen muss, um in den Wide-Modus wechseln zu können.
- `Pane1Length`: legt die Breite von Pane1 im Wide-Modus sowie die Höhe von Pane1 im Tall-Modus fest. Die Eigenschaft hat keine Auswirkungen im SinglePane-Modus.
- `Pane2Length`: legt die Breite von Pane2 im Wide-Modus sowie die Höhe von Pane2 im Tall-Modus fest. Die Eigenschaft hat keine Auswirkungen im SinglePane-Modus.

> [!IMPORTANT]
> Wenn `TwoPaneView` auf zwei Bildschirmen eingesetzt wird, sind diese Eigenschaften wirkungslos.

## <a name="properties-that-apply-when-on-one-screen-or-two"></a>Eigenschaften, die unabhängig der Bildschirmanzahl gelten

Die folgenden Eigenschaften werden angewendet, wenn `TwoPaneView` auf einem einzelnen Bildschirm oder auf zwei Bildschirmen verwendet wird:

- `TallModeConfiguration`: Im Tall-Modus gibt diese Eigenschaft an, welcher Bereich links und welcher rechts angeordnet ist oder, falls von TwoPaneViewPriority so definiert, ob nur ein einzelner Bereich angezeigt werden soll.
- `WideModeConfiguration`: Im Wide-Modus gibt diese Eigenschaft an, welcher Bereich oben und welcher unten angeordnet ist oder, falls von TwoPaneViewPriority so definiert, ob nur ein einzelner Bereich angezeigt werden soll.
- `PanePriority`: Diese Eigenschaft gibt an, ob im SinglePane-Modus Pane1 oder Pane2 angezeigt werden soll.

## <a name="related-links"></a>Verwandte Links

- [DualScreen (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-dualscreendemos/)
