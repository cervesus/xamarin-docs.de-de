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
ms.sourcegitcommit: 699de58432b7da300ddc2c85842e5d9e129b0dc5
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2019
ms.locfileid: "68739148"
---
# <a name="xamarinforms-checkbox"></a>Xamarin. Forms (Kontrollkästchen)

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-checkboxdemos/)

Xamarin. Forms `CheckBox` ist ein Typ von Schaltfläche, der entweder aktiviert oder leer sein kann. Wenn ein Kontrollkästchen aktiviert ist, gilt es als aktiviert. Wenn ein Kontrollkästchen leer ist, wird es als deaktiviert angesehen.

`CheckBox`definiert eine `bool` Eigenschaft mit `IsChecked`dem Namen, die angibt `CheckBox` , ob die aktiviert ist. Diese Eigenschaft wird auch durch ein [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) -Objekt unterstützt. Dies bedeutet, dass Sie formatiert werden kann und das Ziel von Daten Bindungen ist.

> [!NOTE]
> Die `IsChecked` bindbare Eigenschaft verfügt über einen Standard Bindungs Modus [`BindingMode.TwoWay`](xref:Xamarin.Forms.BindingMode.TwoWay)von.

`CheckBox`definiert ein `CheckedChanged` -Ereignis, das ausgelöst wird `IsChecked` , wenn die-Eigenschaft geändert wird, entweder durch Benutzer Bearbeitung oder `IsChecked` wenn eine Anwendung die-Eigenschaft festlegt. Das `CheckedChangedEventArgs` Objekt, das das `CheckedChanged` Ereignis begleitet, verfügt über eine `Value`einzelne Eigenschaft namens `bool`vom Typ. Wenn das Ereignis ausgelöst wird, wird der Wert `Value` der Eigenschaft auf den neuen Wert `IsChecked` der Eigenschaft festgelegt.

## <a name="create-a-checkbox"></a>Kontrollkästchen erstellen

Das folgende Beispiel zeigt, wie Sie instanziieren ein `CheckBox` in XAML:

```xaml
<CheckBox />
```

Dieses XAML-Code Ergebnis wird in den folgenden Screenshots angezeigt:

![Screenshot eines leeren Kontrollkästchens unter IOS und Android](checkbox-images/checkbox-empty.png "Leeres Kontrollkästchen")

Standardmäßig `CheckBox` ist leer. Kann durch Benutzer Bearbeitung oder durch Festlegen der `IsChecked` -Eigenschaft auf `true`festgelegt werden: `CheckBox`

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

Wenn sich `IsChecked` die-Eigenschaft ändert, entweder über die Benutzer Manipulation oder wenn eine `IsChecked` Anwendung die- `CheckedChanged` Eigenschaft festlegt, wird das-Ereignis ausgelöst. Ein Ereignishandler für dieses Ereignis kann registriert werden, um auf die Änderung zu reagieren:

```xaml
<CheckBox CheckedChanged="OnCheckBoxCheckedChanged" />
```

Die Code-Behind-Datei enthält den Handler für die `CheckedChanged` Ereignis:

```csharp
void OnCheckBoxCheckedChanged(object sender, CheckedChangedEventArgs e)
{
    // Perform required operation after examining e.Value
}
```

Die `sender` Argument ist der `CheckBox` für dieses Ereignis verantwortlich. Hiermit können Sie den Zugriff auf die `CheckBox` Objekt oder zur Unterscheidung zwischen mehreren `CheckBox` Objekte, die gemeinsame Verwendung desselben `CheckedChanged` Ereignis.

Alternativ kann ein Ereignishandler für das `CheckedChanged` -Ereignis im Code registriert werden:

```csharp
CheckBox checkBox = new CheckBox { ... };
checkBox.CheckedChanged += (sender, e) =>
{
    // Perform required operation after examining e.Value
};
```

## <a name="data-bind-a-checkbox"></a>Kontrollkästchen "Datenbindung"

Der `CheckedChanged` Ereignishandler kann durch die Verwendung von Datenbindung und Triggern gelöscht werden, `CheckBox` um auf ein zu überprüfende oder leeres Ereignis zu reagieren:

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

In diesem Beispiel [`Label`](xref:Xamarin.Forms.Label) verwendet einen Bindungs Ausdruck in einem Daten-Triggern, um die `IsChecked` -Eigenschaft von `CheckBox`zu überwachen. Wenn diese Eigenschaft wird `true`, werden `FontAttributes` die-Eigenschaft und `Label` die- `FontSize` Eigenschaft der geändert. Wenn die `IsChecked` -Eigenschaft `false`zurückkehrt, `FontAttributes` werden`Label` die-Eigenschaft und die- `FontSize` Eigenschaft des auf den ursprünglichen Zustand zurückgesetzt.

In den folgenden Screenshots zeigt der IOS-Screenshot die [`Label`](xref:Xamarin.Forms.Label) Formatierung, wenn `CheckBox` der leer ist, während der Android-Screenshot `Label` die Formatierung anzeigt `CheckBox` , wenn das aktiviert ist:

[![Screenshot des Kontrollkästchens "Daten gebunden" unter IOS und Android](checkbox-images/checkbox-databinding.png "Kontrollkästchen für Daten gebunden")](checkbox-images/checkbox-databinding-large.png#lightbox "Kontrollkästchen für Daten gebunden")

Weitere Informationen zu Triggern finden Sie unter [xamarin. Forms-Trigger](~/xamarin-forms/app-fundamentals/triggers.md).

## <a name="disable-a-checkbox"></a>Kontrollkästchen deaktivieren

Manchmal wird eine Anwendung in einen Zustand versetzt `CheckBox` , in dem eine überprüfte keine gültige Operation ist. In solchen Fällen kann der `CheckBox` deaktiviert werden, indem die zugehörige- `false` `IsEnabled` Eigenschaft auf festgelegt wird.

## <a name="checkbox-appearance"></a>CheckBox-Darstellung

Zusätzlich zu den Eigenschaften `CheckBox` , die von der [`View`](xref:Xamarin.Forms.View) -Klasse erben, `CheckBox` definiert auch [`Color`](xref:Xamarin.Forms.Color)eine `Color` -Eigenschaft, die die Farbe auf festlegt:

```xaml
<CheckBox Color="Red" />
```

Die folgenden Screenshots zeigen eine Reihe von `CheckBox` aktivierten Objekten, wobei für jedes Objekt die- `Color` Eigenschaft auf einen [`Color`](xref:Xamarin.Forms.Color)anderen Wert festgelegt ist:

![Screenshot der farbigen Kontrollkästchen unter IOS und Android](checkbox-images/checkbox-colors.png "Farbiges Kontrollkästchen")

## <a name="checkbox-visual-states"></a>Kontrollkästchen visuelle Zustände

`CheckBox`verfügt über `IsChecked` ein [`VisualState`](xref:Xamarin.Forms.VisualState) -Element, das zum Initiieren einer visuellen Änderung am `CheckBox` verwendet werden kann, wenn es aktiviert wird.

Im folgende XAML-Beispiel zeigt, wie definieren Sie einen visuellen Zustand für die `IsChecked` Zustand:

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

In diesem Beispiel gibt das `IsChecked` [`VisualState`](xref:Xamarin.Forms.VisualState) an, dass die `CheckBox` - `Color` Eigenschaft bei aktivierter-Eigenschaft auf grün festgelegt wird. Der `Normal` `CheckBox` `Color` gibt an, dass die-Eigenschaft auf "rot" festgelegt wird, wenn sich der in einem normalen Zustand befindet. `VisualState` Der Gesamteffekt besteht daher darin, dass `CheckBox` das rot ist, wenn es leer ist, und grün, wenn es aktiviert ist.

Weitere Informationen zu visuellen Zuständen finden Sie unter [xamarin. Forms Visual State Manager](~/xamarin-forms/user-interface/visual-state-manager.md).

## <a name="related-links"></a>Verwandte Links

- [CheckBox-Demos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-checkboxdemos/)
- [Xamarin. Forms-Trigger](~/xamarin-forms/app-fundamentals/triggers.md)
- [Xamarin. Forms Visual State Manager](~/xamarin-forms/user-interface/visual-state-manager.md)
