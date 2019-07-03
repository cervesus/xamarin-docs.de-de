---
title: Xamarin.Forms-Kontrollkästchen
description: Das Kontrollkästchen für die Xamarin.Forms ist eine Art von Schaltfläche, die entweder geprüft werden kann oder leer sein. Wenn ein Kontrollkästchen aktiviert ist, gilt dies als auf. Wenn ein Kontrollkästchen leer ist, gilt dies als deaktiviert werden.
ms.prod: xamarin
ms.assetid: B8B9268B-BCB8-42B9-B08C-C0F22C137238
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/11/2019
ms.openlocfilehash: 42631b1b67dc1d342e9f8666916604e68ee158d8
ms.sourcegitcommit: 0fd04ea3af7d6a6d6086525306523a5296eec0df
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2019
ms.locfileid: "67517919"
---
# <a name="xamarinforms-checkbox"></a>Xamarin.Forms-Kontrollkästchen

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/CheckBoxDemos)

Die Xamarin.Forms `CheckBox` ist eine Art von Schaltfläche, die können entweder überprüfte oder leer sein. Wenn ein Kontrollkästchen aktiviert ist, gilt dies als auf. Wenn ein Kontrollkästchen leer ist, gilt dies als deaktiviert werden.

`CheckBox` definiert eine `bool` Eigenschaft mit dem Namen `IsChecked`, das angibt, ob die `CheckBox` aktiviert ist. Diese Eigenschaft wird auch unterstützt von einem [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) -Objekt, das bedeutet, dass es formatiert werden kann, und das Ziel von datenbindungen.

> [!NOTE]
> Die `IsChecked` bindbare Eigenschaft verfügt über einen Standardmodus für die Bindung der [ `BindingMode.TwoWay` ](xref:Xamarin.Forms.BindingMode.TwoWay).

`CheckBox` definiert eine `CheckedChanged` -Ereignisses, wenn ausgelöst hat, die `IsChecked` eigenschaftsänderungen, entweder durch Bearbeitung durch Benutzer oder wenn eine Anwendung festlegt der `IsChecked` Eigenschaft. Die `CheckedChangedEventArgs` Objekt an, die mit der `CheckedChanged` Ereignis verfügt über eine einzelne Eigenschaft namens `Value`, des Typs `bool`. Wenn das Ereignis ausgelöst wird, wird den Wert des der `Value` -Eigenschaftensatz auf den neuen Wert der `IsChecked` Eigenschaft.

## <a name="create-a-checkbox"></a>Erstellen Sie ein Kontrollkästchen

Das folgende Beispiel zeigt, wie Sie instanziieren ein `CheckBox` in XAML:

```xaml
<CheckBox />
```

Dieses XAML ergibt das in den folgenden Screenshots gezeigt aussehen:

![Screenshot des ist das Kontrollkästchen, unter iOS und Android](checkbox-images/checkbox-empty.png "ist das Kontrollkästchen")

In der Standardeinstellung die `CheckBox` ist leer. Die `CheckBox` kann überprüft werden, von der Bearbeitung durch Benutzer oder durch Festlegen der `IsChecked` Eigenschaft `true`:

```xaml
<CheckBox IsChecked="true" />
```

Dieses XAML ergibt das in den folgenden Screenshots gezeigt aussehen:

![Screenshot der ein aktiviertes Kontrollkästchen an, unter iOS und Android](checkbox-images/checkbox-checked.png "aktiviert Kontrollkästchen")

Sie können auch eine `CheckBox` im Code erstellt werden können:

```csharp
CheckBox checkBox = new CheckBox { IsChecked = true };
```

## <a name="respond-to-a-checkbox-changing-state"></a>Reagieren Sie auf eine Zustandsänderung Kontrollkästchen

Wenn die `IsChecked` eigenschaftsänderungen, entweder durch Bearbeitung durch Benutzer oder wenn eine Anwendung festlegt der `IsChecked` -Eigenschaft, die `CheckedChanged` Ereignis wird ausgelöst. Ein Ereignishandler für dieses Ereignis kann registriert werden, um auf die Änderung reagieren:

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

Sie können auch einen Ereignishandler für die `CheckedChanged` Ereignis registriert werden kann, im Code:

```csharp
CheckBox checkBox = new CheckBox { ... };
checkBox.CheckedChanged += (sender, e) =>
{
    // Perform required operation after examining e.Value
};
```

## <a name="data-bind-a-checkbox"></a>Ein Kontrollkästchen zum Binden von Daten

Die `CheckedChanged` -Ereignishandler kann behoben werden, mithilfe von Datenbindung und Trigger reagieren auf eine `CheckBox` aktiviert bzw. leer:

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

In diesem Beispiel die [ `Label` ](xref:Xamarin.Forms.Label) verwendet einen Bindungsausdruck in einem Datentrigger zum Überwachen der `IsChecked` Eigenschaft der `CheckBox`. Wenn diese Eigenschaft wird `true`, `FontAttributes` und `FontSize` Eigenschaften der `Label` ändern. Wenn der `IsChecked` -Eigenschaft gibt, `false`, wird die `FontAttributes` und `FontSize` Eigenschaften der `Label` auf ihren ursprünglichen Status zurückgesetzt.

In den folgenden Screenshots der iOS-Screenshot zeigt die [ `Label` ](xref:Xamarin.Forms.Label) Formatierung verwenden, wenn die `CheckBox` leer ist, während der Android-Screenshot zeigt die `Label` Formatierung verwenden, wenn die `CheckBox` aktiviert ist:

[![Screenshot eines gebundenen Kontrollkästchen unter iOS und Android](checkbox-images/checkbox-databinding.png "datengebundene Kontrollkästchen")](checkbox-images/checkbox-databinding-large.png#lightbox "datengebundene Kontrollkästchen")

Weitere Informationen zu Triggern finden Sie unter [Xamarin.Forms Trigger](~/xamarin-forms/app-fundamentals/triggers.md).

## <a name="disable-a-checkbox"></a>Deaktivieren Sie ein Kontrollkästchen

Manchmal eine Anwendung in einen Zustand wechselt, in denen eine `CheckBox` überprüft wird, ist kein gültiger Vorgang. In solchen Fällen die `CheckBox` können deaktiviert werden, indem die `IsEnabled` Eigenschaft `false`.

## <a name="checkbox-appearance"></a>Kontrollkästchen-Darstellung

Zusätzlich zu den Eigenschaften, die `CheckBox` erbt von der [ `View` ](xref:Xamarin.Forms.View) -Klasse, `CheckBox` definiert auch eine `Color` -Eigenschaft, die die Farbe wird auf eine [ `Color` ](xref:Xamarin.Forms.Color):

```xaml
<CheckBox Color="Red" />
```

Die folgenden Screenshots zeigen eine Reihe von aktivierten `CheckBox` Objekte, wobei jedes Objekt verfügt über seine `Color` -Eigenschaft auf einen anderen [ `Color` ](xref:Xamarin.Forms.Color):

![Screenshot der farbige Kontrollkästchen an, unter iOS und Android](checkbox-images/checkbox-colors.png "farbige Kontrollkästchen")

## <a name="checkbox-visual-states"></a>Visuelle Zustände von Kontrollkästchen

`CheckBox` verfügt über eine `IsChecked` [ `VisualState` ](xref:Xamarin.Forms.VisualState) , die verwendet werden kann, initiieren Sie eine visuelle Änderung an der `CheckBox` Wenn diese Option aktiviert wird.

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

In diesem Beispiel die `IsChecked` [ `VisualState` ](xref:Xamarin.Forms.VisualState) gibt an, dass die `CheckBox` aktiviert ist, wird die `Color` Eigenschaft wird auf Grün festgelegt werden. Die `Normal` `VisualState` gibt an, dass die `CheckBox` befindet sich in einem normalen Status, seine `Color` Eigenschaft wird auf Rot festgelegt. Aus diesem Grund der Gesamteffekt besteht, die die `CheckBox` ist rot, wenn es leer ist, und Grün ist, wenn diese Option aktiviert ist.

Weitere Informationen zu visuellen Status finden Sie unter [Xamarin.Forms Visual State Manager](~/xamarin-forms/user-interface/visual-state-manager.md).

## <a name="related-links"></a>Verwandte Links

- [Kontrollkästchen-Demos (Beispiel)](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/CheckBoxDemos)
- [Xamarin.Forms-Trigger](~/xamarin-forms/app-fundamentals/triggers.md)
- [Xamarin.Forms visuellen Zustands-Manager](~/xamarin-forms/user-interface/visual-state-manager.md)
