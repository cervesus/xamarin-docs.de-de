---
title: Xamarin.Forms-Trigger für Dual-Screen-Geräte
description: In diesem Artikel wird beschrieben, wie Xamarin.Forms-Trigger für Dual-Screen-Geräte verwendet werden, um auf Änderungen der Benutzeroberfläche mit XAML zu reagieren.
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: df10327b2ac12d2b119f1ab558d7f27e8c319507
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2020
ms.locfileid: "84131195"
---
# <a name="xamarinforms-dual-screen-triggers"></a>Xamarin.Forms-Trigger für Dual-Screen-Geräte

![](~/media/shared/preview.png "This API is currently pre-release")

Der [`Xamarin.Forms.DualScreen`](xref:Xamarin.Forms.DualScreen)-Namespace enthält zwei Zustandstrigger:

- [`SpanModeStateTrigger`](xref:Xamarin.Forms.DualScreen.SpanModeStateTrigger) löst eine [`VisualState`](xref:Xamarin.Forms.VisualState)-Änderung aus, wenn sich der Ansichtsmodus des angefügten Layouts ändert.
- `WindowSpanModeStateTrigger` löst eine [`VisualState`](xref:Xamarin.Forms.VisualState)-Änderung aus, wenn sich der Ansichtsmodus des Fensters ändert.

Weitere Informationen zu Zustandstriggern finden Sie unter [Zustandstrigger](~/xamarin-forms/app-fundamentals/triggers.md#state-triggers).

## <a name="span-mode-state-trigger"></a>Span-Modus-Zustandstrigger

Ein [`SpanModeStateTrigger`](xref:Xamarin.Forms.DualScreen.SpanModeStateTrigger) löst eine [`VisualState`](xref:Xamarin.Forms.VisualState)-Änderung aus, wenn sich der Span-Modus des angefügten Layouts ändert. Dieser Trigger hat eine einzelne bindbare Eigenschaft:

- [`SpanMode`](xref:Xamarin.Forms.DualScreen.SpanModeStateTrigger.SpanMode) mit Typ [`TwoPaneViewMode`](xref:Xamarin.Forms.DualScreen.SpanModeStateTrigger.SpanMode), der den Span-Modus angibt, auf den [`VisualState`](xref:Xamarin.Forms.VisualState) angewendet werden soll.

> [!NOTE]
> [`SpanModeStateTrigger`](xref:Xamarin.Forms.DualScreen.SpanModeStateTrigger) ist von der Klasse [`StateTriggerBase`](xref:Xamarin.Forms.StateTriggerBase) abgeleitet und kann daher einen Ereignishandler an das Ereignis [`IsActiveChanged`](xref:Xamarin.Forms.StateTriggerBase.IsActiveChanged) anfügen.

Im folgenden XAML-Beispiel wird ein [`Grid`](xref:Xamarin.Forms.Grid) mit `SpanModeStateTrigger`-Objekten veranschaulicht:

```xaml
<Grid>
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup>
            <VisualState x:Name="GridSingle">
                <VisualState.StateTriggers>
                    <dualScreen:SpanModeStateTrigger SpanMode="SinglePane"/>
                </VisualState.StateTriggers>
                <VisualState.Setters>
                    <Setter Property="BackgroundColor" Value="Green" />
                </VisualState.Setters>
            </VisualState>
            <VisualState x:Name="GridWide">
                <VisualState.StateTriggers>
                    <dualScreen:SpanModeStateTrigger SpanMode="Wide" />
                </VisualState.StateTriggers>
                <VisualState.Setters>
                    <Setter Property="BackgroundColor" Value="Red" />
                </VisualState.Setters>
            </VisualState>
            <VisualState x:Name="GridTall">
                <VisualState.StateTriggers>
                    <dualScreen:SpanModeStateTrigger SpanMode="Tall" />
                </VisualState.StateTriggers>
                <VisualState.Setters>
                    <Setter Property="BackgroundColor" Value="Purple" />
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
    ...
</Grid>
```

In diesem Beispiel werden visuelle Zustände für ein [`Grid`](xref:Xamarin.Forms.Grid)-Objekt festgelegt. Die Hintergrundfarbe für das `Grid` ist grün, wenn nur ein Bereich angezeigt wird. Sie ist rot, wenn Bereiche nebeneinander angezeigt werden, und sie ist lila, wenn Bereiche übereinander angezeigt werden.

## <a name="window-span-mode-state-trigger"></a>Zustandstrigger für Span-Modus des Fensters

Ein `WindowSpanModeStateTrigger` löst eine [`VisualState`](xref:Xamarin.Forms.VisualState)-Änderung aus, wenn sich der Span-Modus des Fensters ändert. Dieser Trigger hat eine einzelne bindbare Eigenschaft:

- [`SpanMode`](xref:Xamarin.Forms.DualScreen.SpanModeStateTrigger.SpanMode) mit Typ [`TwoPaneViewMode`](xref:Xamarin.Forms.DualScreen.SpanModeStateTrigger.SpanMode), der den Span-Modus angibt, auf den [`VisualState`](xref:Xamarin.Forms.VisualState) angewendet werden soll.

> [!NOTE]
> `WindowSpanModeStateTrigger` ist von der Klasse [`StateTriggerBase`](xref:Xamarin.Forms.StateTriggerBase) abgeleitet und kann daher einen Ereignishandler an das Ereignis [`IsActiveChanged`](xref:Xamarin.Forms.StateTriggerBase.IsActiveChanged) anfügen.

Im folgenden XAML-Beispiel wird ein [`Grid`](xref:Xamarin.Forms.Grid) mit `WindowSpanModeStateTrigger`-Objekten veranschaulicht:

```xaml
<Grid>
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup>
            <VisualState x:Name="NotSpanned">
                <VisualState.StateTriggers>
                    <dualScreen:WindowSpanModeStateTrigger SpanMode="SinglePane"/>
                </VisualState.StateTriggers>
                <VisualState.Setters>
                    <Setter Property="BackgroundColor" Value="Red" />
                </VisualState.Setters>
            </VisualState>
            <VisualState x:Name="Spanned">
                <VisualState.StateTriggers>
                    <dualScreen:WindowSpanModeStateTrigger SpanMode="Wide" />
                </VisualState.StateTriggers>
                <VisualState.Setters>
                    <Setter Property="BackgroundColor" Value="Green" />
                </VisualState.Setters>
            </VisualState>
                <VisualState x:Name="Tall">
                    <VisualState.StateTriggers>
                        <dualScreen:WindowSpanModeStateTrigger SpanMode="Tall" />
                    </VisualState.StateTriggers>
                    <VisualState.Setters>
                        <Setter Property="BackgroundColor" Value="Yellow" />
                    </VisualState.Setters>
                </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
    ...
</Grid>    
```

In diesem Beispiel werden visuelle Zustände für ein [`Grid`](xref:Xamarin.Forms.Grid)-Objekt festgelegt. Die Hintergrundfarbe für das `Grid` ist rot, wenn nur ein Bereich angezeigt wird. Sie ist grün, wenn Bereiche nebeneinander angezeigt werden, und sie ist gelb, wenn Bereiche übereinander angezeigt werden.

## <a name="related-links"></a>Verwandte Links

- [Xamarin.Forms-Trigger](~/xamarin-forms/app-fundamentals/triggers.md)
- [Xamarin.Forms: Visual State-Manager](~/xamarin-forms/user-interface/visual-state-manager.md)
