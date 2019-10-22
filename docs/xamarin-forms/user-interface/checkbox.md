---
title: Xamarin. Forms (Kontrollkästchen)
description: Das Kontrollkästchen xamarin. Forms ist ein Typ von Schaltfläche, der entweder aktiviert oder leer ist. Wenn ein Kontrollkästchen aktiviert ist, gilt es als aktiviert. Wenn ein Kontrollkästchen leer ist, wird es als deaktiviert angesehen.
ms.prod: xamarin
ms.assetid: B8B9268B-BCB8-42B9-B08C-C0F22C137238
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/11/2019
ms.openlocfilehash: f78ca9d2cf7a9e57b81c5d923c64b36a7982c4b0
ms.sourcegitcommit: 9bfedf07940dad7270db86767eb2cc4007f2a59f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2019
ms.locfileid: "68739148"
---
# <a name="xamarinforms-checkbox"></a>Xamarin. Forms (Kontrollkästchen)

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-checkboxdemos/)

Der xamarin. Forms-`CheckBox` ist ein Typ von Schaltfläche, der entweder aktiviert oder leer sein kann. Wenn ein Kontrollkästchen aktiviert ist, gilt es als aktiviert. Wenn ein Kontrollkästchen leer ist, wird es als deaktiviert angesehen.

`CheckBox` definiert eine `bool` Eigenschaft mit dem Namen `IsChecked`, die angibt, ob die `CheckBox` aktiviert ist. Diese Eigenschaft wird auch durch ein [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) Objekt unterstützt. Dies bedeutet, dass Sie formatiert werden kann und das Ziel von Daten Bindungen ist.

> [!NOTE]
> Die `IsChecked` bindbare-Eigenschaft verfügt über einen Standard Bindungs Modus von [`BindingMode.TwoWay`](xref:Xamarin.Forms.BindingMode.TwoWay).

`CheckBox` definiert ein `CheckedChanged` Ereignis, das ausgelöst wird, wenn sich die `IsChecked`-Eigenschaft ändert, entweder durch Benutzer Manipulation oder wenn eine Anwendung die `IsChecked`-Eigenschaft festlegt. Das `CheckedChangedEventArgs` Objekt, das das `CheckedChanged` Ereignis begleitet, verfügt über eine einzelne Eigenschaft mit dem Namen `Value` vom Typ `bool`. Wenn das Ereignis ausgelöst wird, wird der Wert der `Value`-Eigenschaft auf den neuen Wert der `IsChecked`-Eigenschaft festgelegt.

## <a name="create-a-checkbox"></a>Kontrollkästchen erstellen

Im folgenden Beispiel wird gezeigt, wie ein `CheckBox` in XAML instanziiert wird:

```xaml
<CheckBox />
```

Dieses XAML-Code Ergebnis wird in den folgenden Screenshots angezeigt:

![Screenshot eines leeren Kontrollkästchens unter IOS und Android](checkbox-images/checkbox-empty.png "Leeres Kontrollkästchen")

Standardmäßig ist das `CheckBox` leer. Der `CheckBox` kann bei der Benutzer Bearbeitung oder durch Festlegen der Eigenschaft `IsChecked` auf `true` aktiviert werden:

```xaml
<CheckBox IsChecked="true" />
```

Dieses XAML-Code Ergebnis wird in den folgenden Screenshots angezeigt:

![Screenshot eines aktivierten Kontrollkästchens unter IOS und Android](checkbox-images/checkbox-checked.png "Kontrollkästchen aktiviert")

Alternativ kann ein `CheckBox` im Code erstellt werden:

```csharp
CheckBox checkBox = new CheckBox { IsChecked = true };
```

## <a name="respond-to-a-checkbox-changing-state"></a>Reagieren auf das Kontrollkästchen zum Ändern des Status

Wenn die `IsChecked`-Eigenschaft geändert wird, entweder durch Benutzer Manipulation oder wenn eine Anwendung die `IsChecked`-Eigenschaft festlegt, wird das `CheckedChanged`-Ereignis ausgelöst. Ein Ereignishandler für dieses Ereignis kann registriert werden, um auf die Änderung zu reagieren:

```xaml
<CheckBox CheckedChanged="OnCheckBoxCheckedChanged" />
```

Die Code-Behind-Datei enthält den Handler für das `CheckedChanged`-Ereignis:

```csharp
void OnCheckBoxCheckedChanged(object sender, CheckedChangedEventArgs e)
{
    // Perform required operation after examining e.Value
}
```

Das `sender`-Argument ist der `CheckBox`, der für dieses Ereignis verantwortlich ist. Sie können dies verwenden, um auf das `CheckBox` Objekt zuzugreifen oder um zwischen mehreren `CheckBox` Objekten zu unterscheiden, die dasselbe `CheckedChanged` Ereignis verwenden.

Alternativ kann ein Ereignishandler für das `CheckedChanged` Ereignis im Code registriert werden:

```csharp
CheckBox checkBox = new CheckBox { ... };
checkBox.CheckedChanged += (sender, e) =>
{
    // Perform required operation after examining e.Value
};
```

## <a name="data-bind-a-checkbox"></a>Kontrollkästchen "Datenbindung"

Der `CheckedChanged` Ereignishandler kann mithilfe von Datenbindung und Triggern gelöscht werden, um auf eine `CheckBox` zu antworten, die aktiviert oder leer ist:

```xaml
<CheckBox x:Name="checkBox" />
<Label Text="Lorem ipsum dolor sit amet, elit rutrum, enim hendrerit augue vitae praesent sed non, lorem aenean quis praesent pede.">
    <Label.Triggers>
        <DataTrigger TargetType="Label"
                     Binding="{Binding Source={x:Reference checkBox}, Path=IsChecked}"
                     Value="true">
            <Setter Property="FontAttributes"
                    Value="Italic, Bold" />
            <Setter Property="FontSize"
                    Value="Large" />
        </DataTrigger>
    </Label.Triggers>
</Label>
```

In diesem Beispiel verwendet die [`Label`](xref:Xamarin.Forms.Label) einen Bindungs Ausdruck in einem Daten-Triggern, um die `IsChecked`-Eigenschaft der `CheckBox` zu überwachen. Wenn diese Eigenschaft `true` wird, ändern sich die Eigenschaften `FontAttributes` und `FontSize` des `Label`. Wenn die `IsChecked`-Eigenschaft zu `false` zurückkehrt, werden die Eigenschaften `FontAttributes` und `FontSize` der `Label` auf ihren ursprünglichen Zustand zurückgesetzt.

In den folgenden Screenshots zeigt der IOS-Screenshot die [`Label`](xref:Xamarin.Forms.Label) Formatierung, wenn die `CheckBox` leer ist, während der Android-Screenshot die `Label` Formatierung anzeigt, wenn die `CheckBox` aktiviert ist:

[![Screenshot des Kontrollkästchens "Daten gebunden" unter IOS und Android](checkbox-images/checkbox-databinding.png "Kontrollkästchen für Daten gebunden")](checkbox-images/checkbox-databinding-large.png#lightbox "Kontrollkästchen für Daten gebunden")

Weitere Informationen zu Triggern finden Sie unter [xamarin. Forms-Trigger](~/xamarin-forms/app-fundamentals/triggers.md).

## <a name="disable-a-checkbox"></a>Kontrollkästchen deaktivieren

Manchmal wird eine Anwendung in einen Zustand versetzt, in dem ein `CheckBox` geprüft wird, der kein gültiger Vorgang ist. In solchen Fällen kann die `CheckBox` durch Festlegen der `IsEnabled`-Eigenschaft auf `false` deaktiviert werden.

## <a name="checkbox-appearance"></a>CheckBox-Darstellung

Zusätzlich zu den Eigenschaften, die `CheckBox` von der [`View`](xref:Xamarin.Forms.View) -Klasse erbt, definiert `CheckBox` auch eine `Color` Eigenschaft, die die Farbe auf einen [`Color`](xref:Xamarin.Forms.Color)festlegt:

```xaml
<CheckBox Color="Red" />
```

Die folgenden Screenshots zeigen eine Reihe von aktivierten `CheckBox` Objekten, wobei für jedes Objekt die `Color`-Eigenschaft auf einen anderen [`Color`](xref:Xamarin.Forms.Color)festgelegt ist:

![Screenshot der farbigen Kontrollkästchen unter IOS und Android](checkbox-images/checkbox-colors.png "Farbiges Kontrollkästchen")

## <a name="checkbox-visual-states"></a>Kontrollkästchen visuelle Zustände

`CheckBox` verfügt über eine `IsChecked` [`VisualState`](xref:Xamarin.Forms.VisualState) , die zum Initiieren einer visuellen Änderung an der `CheckBox` verwendet werden kann, wenn diese aktiviert wird.

Das folgende XAML-Beispiel zeigt, wie ein visueller Zustand für den `IsChecked` Status definiert wird:

```xaml
<CheckBox ...>
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">
            <VisualState x:Name="Normal">
                <VisualState.Setters>
                    <Setter Property="Color"
                            Value="Red" />
                </VisualState.Setters>
            </VisualState>

            <VisualState x:Name="IsChecked">
                <VisualState.Setters>
                    <Setter Property="Color"
                            Value="Green" />
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</CheckBox>
```

In diesem Beispiel gibt die `IsChecked` [`VisualState`](xref:Xamarin.Forms.VisualState) an, dass die `Color` Eigenschaft bei aktivierter `CheckBox` auf grün festgelegt wird. Die `Normal` `VisualState` gibt an, dass die `Color` Eigenschaft auf "rot" festgelegt wird, wenn sich die `CheckBox` in einem normalen Zustand befindet. Der Gesamteffekt besteht daher darin, dass das `CheckBox` rot ist, wenn es leer ist, und grün, wenn es aktiviert ist.

Weitere Informationen zu visuellen Zuständen finden Sie unter [xamarin. Forms Visual State Manager](~/xamarin-forms/user-interface/visual-state-manager.md).

## <a name="related-links"></a>Verwandte Links

- [CheckBox-Demos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-checkboxdemos/)
- [Xamarin. Forms-Trigger](~/xamarin-forms/app-fundamentals/triggers.md)
- [Xamarin. Forms Visual State Manager](~/xamarin-forms/user-interface/visual-state-manager.md)
