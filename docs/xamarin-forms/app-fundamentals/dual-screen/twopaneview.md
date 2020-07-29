---
title: Xamarin.Forms-Layout für Dual-Screen-Geräte
description: In diesem Artikel wird beschrieben, wie Sie mit „TwoPaneView“ in Xamarin.Forms die Benutzeroberfläche Ihrer App für Dual-Screen-Geräte wie Surface Duo und Surface Neo optimieren.
ms.prod: xamarin
ms.assetid: 17ee8afa-5e7c-4a4f-a9b6-2aca03f30fe3
ms.technology: xamarin-forms
author: davidortinau
ms.author: daortin
ms.date: 02/08/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: fb5474c7436cb985a1404b662fcf842f22cfdc0d
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86937695"
---
# <a name="xamarinforms-twopaneview-layout"></a>TwoPaneView-Layout von Xamarin.Forms

![Vorabrelease der API](~/media/shared/preview.png "Diese API ist derzeit als Vorabversion erhältlich.")

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-dualscreendemos/)

Die `TwoPaneView`-Klasse stellt einen Container mit zwei Ansichten dar, die den Inhalt dem verfügbaren Platz entsprechend nebeneinander oder übereinander positionieren und in der Größe anpassen. `TwoPaneView` erbt von `Grid`. Diese Eigenschaften funktionieren also so, als würden sie auf ein Raster angewendet werden.

## <a name="set-up-twopaneview"></a>Einrichten von TwoPaneView

Führen Sie die folgenden Anweisungen aus, um ein Dual-Screen-Layout in Ihrer App zu erstellen:

1. Befolgen Sie die Anweisungen zu den [ersten Schritten](index.md), um NuGet hinzuzufügen und die Android-Klasse `MainActivity` zu konfigurieren.
1. Beginnen Sie mit einem einfachen `TwoPaneView`-Layout mithilfe des folgenden XAML-Codes:

    ```xaml
    <ContentPage 
        xmlns:dualScreen="clr-namespace:Xamarin.Forms.DualScreen;assembly=Xamarin.Forms.DualScreen">
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

> [!TIP]
> Im obigen XAML-Code werden viele allgemeine Attribute aus dem `ContentPage`-Element ausgelassen. Denken Sie daran, den `xmlns:dualScreen`-Namespace wie gezeigt zu deklarieren, wenn Sie ein `TwoPaneView`-Layout zu Ihrer App hinzufügen.

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
