---
title: Xamarin.FormsMarkieren
description: Das Xamarin.Forms Kontrollkästchen ist ein Typ von Schaltfläche, der entweder aktiviert oder leer ist. Wenn ein Kontrollkästchen aktiviert ist, gilt es als aktiviert. Wenn ein Kontrollkästchen leer ist, wird es als deaktiviert angesehen.
ms.prod: xamarin
ms.assetid: B8B9268B-BCB8-42B9-B08C-C0F22C137238
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/11/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 8399dde2e4e2c9fb53b38fca2923eb0e3bfc6ce3
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84136473"
---
# <a name="xamarinforms-checkbox"></a>Xamarin.FormsMarkieren

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-checkboxdemos/)

Der Xamarin.Forms `CheckBox` ist ein Typ von Schaltfläche, der entweder aktiviert oder leer sein kann. Wenn ein Kontrollkästchen aktiviert ist, gilt es als aktiviert. Wenn ein Kontrollkästchen leer ist, wird es als deaktiviert angesehen.

`CheckBox`definiert eine `bool` Eigenschaft mit dem Namen `IsChecked` , die angibt, ob die `CheckBox` aktiviert ist. Diese Eigenschaft wird auch durch ein- [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) Objekt unterstützt. Dies bedeutet, dass Sie formatiert werden kann und das Ziel von Daten Bindungen ist.

> [!NOTE]
> Die `IsChecked` bindbare Eigenschaft verfügt über einen Standard Bindungs Modus von [`BindingMode.TwoWay`](xref:Xamarin.Forms.BindingMode.TwoWay) .

`CheckBox`definiert ein `CheckedChanged` -Ereignis, das ausgelöst wird, wenn die- `IsChecked` Eigenschaft geändert wird, entweder durch Benutzer Bearbeitung oder wenn eine Anwendung die- `IsChecked` Eigenschaft festlegt. Das `CheckedChangedEventArgs` Objekt, das das `CheckedChanged` Ereignis begleitet, verfügt über eine einzelne Eigenschaft namens `Value` vom Typ `bool` . Wenn das Ereignis ausgelöst wird, wird der Wert der `Value` Eigenschaft auf den neuen Wert der Eigenschaft festgelegt `IsChecked` .

## <a name="create-a-checkbox"></a>Kontrollkästchen erstellen

Im folgenden Beispiel wird gezeigt, wie ein `CheckBox` in XAML instanziiert wird:

```xaml
<CheckBox />
```

Dieses XAML-Code Ergebnis wird in den folgenden Screenshots angezeigt:

![Screenshot eines leeren Kontrollkästchens unter IOS und Android](checkbox-images/checkbox-empty.png "Leeres Kontrollkästchen")

Standardmäßig `CheckBox` ist leer. `CheckBox`Kann durch Benutzer Bearbeitung oder durch Festlegen der- `IsChecked` Eigenschaft auf festgelegt werden `true` :

```xaml
<CheckBox IsChecked="true" />
```

Dieses XAML-Code Ergebnis wird in den folgenden Screenshots angezeigt:

![Screenshot eines aktivierten Kontrollkästchens unter IOS und Android](checkbox-images/checkbox-checked.png "Kontrollkästchen aktiviert")

Alternativ kann eine `CheckBox` im Code erstellt werden:

```csharp
CheckBox checkBox = new CheckBox { IsChecked = true };
```

## <a name="respond-to-a-checkbox-changing-state"></a>Reagieren auf das Kontrollkästchen zum Ändern des Status

Wenn sich die- `IsChecked` Eigenschaft ändert, entweder über die Benutzer Manipulation oder wenn eine Anwendung die- `IsChecked` Eigenschaft festlegt, wird das- `CheckedChanged` Ereignis ausgelöst. Ein Ereignishandler für dieses Ereignis kann registriert werden, um auf die Änderung zu reagieren:

```xaml
<CheckBox CheckedChanged="OnCheckBoxCheckedChanged" />
```

Die Code-Behind-Datei enthält den Handler für das- `CheckedChanged` Ereignis:

```csharp
void OnCheckBoxCheckedChanged(object sender, CheckedChangedEventArgs e)
{
    // Perform required operation after examining e.Value
}
```

Das- `sender` Argument ist der `CheckBox` Verantwortliche für dieses Ereignis. Sie können dies verwenden, um auf das `CheckBox` Objekt zuzugreifen, oder um zwischen mehreren Objekten zu unterscheiden, `CheckBox` die denselben `CheckedChanged` Ereignishandler verwenden.

Alternativ kann ein Ereignishandler für das- `CheckedChanged` Ereignis im Code registriert werden:

```csharp
CheckBox checkBox = new CheckBox { ... };
checkBox.CheckedChanged += (sender, e) =>
{
    // Perform required operation after examining e.Value
};
```

## <a name="data-bind-a-checkbox"></a>Kontrollkästchen "Datenbindung"

Der `CheckedChanged` Ereignishandler kann durch die Verwendung von Datenbindung und Triggern gelöscht werden, um auf ein zu über `CheckBox` prüfende oder leeres Ereignis zu reagieren:

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

In diesem Beispiel [`Label`](xref:Xamarin.Forms.Label) verwendet einen Bindungs Ausdruck in einem Daten-Triggern, um die-Eigenschaft von zu überwachen `IsChecked` `CheckBox` . Wenn diese Eigenschaft wird `true` , werden die `FontAttributes` -Eigenschaft und die-Eigenschaft `FontSize` der `Label` geändert. Wenn die `IsChecked` -Eigenschaft zurückkehrt `false` , `FontAttributes` werden die-Eigenschaft und die-Eigenschaft `FontSize` des `Label` auf den ursprünglichen Zustand zurückgesetzt.

In den folgenden Screenshots zeigt der IOS-Screenshot die [`Label`](xref:Xamarin.Forms.Label) Formatierung, wenn der `CheckBox` leer ist, während der Android-Screenshot die Formatierung anzeigt, `Label` Wenn das `CheckBox` aktiviert ist:

[![Screenshot des Kontrollkästchens "Daten gebunden" unter IOS und Android](checkbox-images/checkbox-databinding.png "Kontrollkästchen für Daten gebunden")](checkbox-images/checkbox-databinding-large.png#lightbox "Kontrollkästchen für Daten gebunden")

Weitere Informationen zu Triggern finden Sie unter [ Xamarin.Forms Trigger](~/xamarin-forms/app-fundamentals/triggers.md).

## <a name="disable-a-checkbox"></a>Kontrollkästchen deaktivieren

Manchmal wird eine Anwendung in einen Zustand versetzt, in dem eine über `CheckBox` prüfte keine gültige Operation ist. In solchen Fällen kann der deaktiviert werden, indem die zugehörige- `CheckBox` Eigenschaft auf festgelegt wird `IsEnabled` `false` .

## <a name="checkbox-appearance"></a>CheckBox-Darstellung

Zusätzlich zu den Eigenschaften, die `CheckBox` von der- [`View`](xref:Xamarin.Forms.View) Klasse erben, `CheckBox` definiert auch eine- `Color` Eigenschaft, die die Farbe auf festlegt [`Color`](xref:Xamarin.Forms.Color) :

```xaml
<CheckBox Color="Red" />
```

Die folgenden Screenshots zeigen eine Reihe von aktivierten `CheckBox` Objekten, wobei für jedes Objekt die- `Color` Eigenschaft auf einen anderen Wert festgelegt ist [`Color`](xref:Xamarin.Forms.Color) :

![Screenshot der farbigen Kontrollkästchen unter IOS und Android](checkbox-images/checkbox-colors.png "Farbiges Kontrollkästchen")

## <a name="checkbox-visual-states"></a>Kontrollkästchen visuelle Zustände

`CheckBox`verfügt über ein-Element `IsChecked` [`VisualState`](xref:Xamarin.Forms.VisualState) , das zum Initiieren einer visuellen Änderung am verwendet werden kann, `CheckBox` Wenn es aktiviert wird.

Das folgende XAML-Beispiel zeigt, wie Sie einen visuellen Zustand für den `IsChecked` Status definieren:

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

In diesem Beispiel gibt das an, dass die-Eigenschaft bei aktivierter- `IsChecked` [`VisualState`](xref:Xamarin.Forms.VisualState) `CheckBox` `Color` Eigenschaft auf grün festgelegt wird. Der `Normal` `VisualState` gibt an, dass die- `CheckBox` `Color` Eigenschaft auf "rot" festgelegt wird, wenn sich der in einem normalen Zustand befindet. Der Gesamteffekt besteht daher darin, dass das `CheckBox` rot ist, wenn es leer ist, und grün, wenn es aktiviert ist.

Weitere Informationen zu visuellen Zuständen finden Sie unter [Xamarin.Forms: Visual State-Manager](~/xamarin-forms/user-interface/visual-state-manager.md).

## <a name="related-links"></a>Verwandte Links

- [CheckBox-Demos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-checkboxdemos/)
- [Xamarin.Forms-Trigger](~/xamarin-forms/app-fundamentals/triggers.md)
- [Xamarin.Forms: Visual State-Manager](~/xamarin-forms/user-interface/visual-state-manager.md)
