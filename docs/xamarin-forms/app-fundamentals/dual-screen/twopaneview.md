---
title: 'Xamarin.Forms: TwoPaneView'
description: In diesem Artikel wird erläutert, wie Sie mit TwoPaneView in Xamarin.Forms die Benutzeroberfläche Ihrer App für Dual-Screen-Geräte wie Surface Duo und Surface Neo optimieren.
ms.prod: xamarin
ms.assetid: 17ee8afa-5e7c-4a4f-a9b6-2aca03f30fe3
ms.technology: xamarin-forms
author: davidortinau
ms.author: daortin
ms.date: 02/08/2020
ms.openlocfilehash: 1992a714774dba79e42c82e71af0bfe6aed7feb7
ms.sourcegitcommit: 07941cf9704ff88cf4087de5ebdea623ff54edb1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/11/2020
ms.locfileid: "77145504"
---
# <a name="xamarinforms-twopaneview"></a>Xamarin.Forms: TwoPaneView

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://github.com/xamarin/xamarin-forms-samples/tree/pre-release/UserInterface/DualScreenDemos)

Die Eigenschaft stellt einen Container mit zwei Ansichten dar, die den Inhalt dem verfügbaren Platz entsprechend nebeneinander oder übereinander positionieren und in der Größe anpassen. TwoPaneView erbt von „Grid“. Diese Eigenschaften funktionieren also so, als würden sie auf ein Raster angewendet werden.

## <a name="set-up-twopaneview"></a>Einrichten von TwoPaneView

Die `TwoPaneView`-Eigenschaft `Source` kann einen URI oder einen lokalen Dateipfad annehmen. Die Wiedergabe beginnt unmittelbar, nachdem das Medium geöffnet wurde.

```xaml
<ContentPage xmlns:dualScreen="clr-namespace:Xamarin.Forms.DualScreen;assembly=Xamarin.Forms.DualScreen">
    <dualScreen:TwoPaneView>
        <dualScreen:TwoPaneView.Pane1>
            <StackLayout>
                <Label Text="Pane1 Content"></Label>
            </StackLayout>
        </dualScreen:TwoPaneView.Pane1>
        <dualScreen:TwoPaneView.Pane2>
            <StackLayout>
                <Label Text="Pane2 Content"></Label>
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

Die folgenden Eigenschaften sind nur relevant, wenn TwoPaneView auf einem einzelnen Bildschirm angewendet wird. Wenn TwoPaneView auf zwei Bildschirmen eingesetzt wird, sind diese Eigenschaften wirkungslos.

- `MinTallModeHeight`: gibt die minimale Höhe an, die das Steuerelement aufweisen muss, um in den hohen Modus wechseln zu können
- `MinWideModeWidth`: gibt die minimale Breite an, die das Steuerelement aufweisen muss, um in den breiten Modus wechseln zu können
- `Pane1Length`: legt die Breite von Pane1 im breiten Modus sowie die Höhe von Pane1 im hohen Modus fest. Die Eigenschaft hat keine Auswirkungen im SinglePane-Modus.
- `Pane2Length`: legt die Breite von Pane2 im breiten Modus sowie die Höhe von Pane2 im hohen Modus fest. Die Eigenschaft hat keine Auswirkungen im SinglePane-Modus.

## <a name="properties-that-apply-when-on-one-screen-or-two"></a>Eigenschaften, die unabhängig der Bildschirmanzahl gelten

- `TallModeConfiguration`: Im hohen Modus gibt diese Eigenschaft an, welcher Bereich links und welcher rechts angeordnet ist oder, falls von TwoPaneViewPriority so definiert, ob nur ein einzelner Bereich angezeigt werden soll.
- `WideModeConfiguration`: Im hohen Modus gibt diese Eigenschaft an, welcher Bereich oben und welcher unten angeordnet ist oder, falls von TwoPaneViewPriority so definiert, ob nur ein einzelner Bereich angezeigt werden soll.
- `PanePriority`: Diese Eigenschaft gibt an, ob im SinglePane-Modus Pane1 oder Pane2 angezeigt werden soll.

## <a name="related-links"></a>Verwandte Links

- [DualScreen (Beispiel)](https://github.com/xamarin/xamarin-forms-samples/tree/pre-release/UserInterface/DualScreenDemos)