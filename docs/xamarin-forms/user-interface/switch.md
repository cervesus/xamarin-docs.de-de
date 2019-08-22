---
title: Xamarin. Forms-Schalter
description: Der xamarin. Forms-Schalter ist ein Typ von Schaltfläche, der vom Benutzer geändert werden kann, um zwischen den Zuständen ein-und auszuschalten. In diesem Artikel wird erläutert, wie Sie die Switch-Klasse verwenden, um ein umschlendes Benutzeroberflächen Element anzuzeigen.
ms.prod: xamarin
ms.assetId: B2F9CC65-481B-4323-8E77-C6BE29C90DE9
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 07/18/2019
ms.openlocfilehash: 1f2ef838287e32df5df42f73e4b43816d618552d
ms.sourcegitcommit: 5f972a757030a1f17f99177127b4b853816a1173
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/21/2019
ms.locfileid: "69887887"
---
# <a name="xamarinforms-switch"></a>Xamarin. Forms-Schalter

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-switchdemos/)

Das xamarin. Forms [`Switch`](xref:Xamarin.Forms.Switch) -Steuerelement ist eine horizontale UMSCHALT Fläche, die vom Benutzer geändert werden kann, um zwischen den Zuständen "ein" und "aus" zu `boolean` wechseln, die durch einen-Wert dargestellt werden. Die `Switch` Klasse erbt von [`View`](xref:Xamarin.Forms.View).

Die folgenden Screenshots zeigen ein `Switch` Steuerelement in seinen **on** -und **Off** -UMSCHALT Zuständen unter IOS und Android:

![Screenshot von Switches in ein-und Ausschalten in IOS und Android](switch-images/switch-states-default.png "Switches unter IOS und Android")

Das `Switch` -Steuerelement definiert zwei Eigenschaften:

* [`IsToggled`](xref:Xamarin.Forms.Switch.IsToggled)ein `boolean` Wert, der `Switch` angibt, ob **auf on**fest liegt.
* [`OnColor`](xref:Xamarin.Forms.Switch.OnColor)ist eine `Color` , die sich darauf `Switch` auswirkt, wie das-Objekt im ein-oder ausgeschaltet wird.
* `ThumbColor`ist der `Color` des Zieh Punkts.

Diese Eigenschaften werden durch ein [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) -Objekt unterstützt. dies `Switch` bedeutet, dass formatiert werden kann und das Ziel von Daten Bindungen ist.

Das `Switch` -Steuerelement `Toggled` definiert ein-Ereignis, das `IsToggled` ausgelöst wird, wenn die-Eigenschaft geändert wird, entweder über eine `IsToggled` Benutzer Bearbeitung oder eine Anwendung die-Eigenschaft festgelegt. Das `ToggledEventArgs` Objekt, das das `Toggled` Ereignis begleitet, verfügt über eine `Value`einzelne Eigenschaft namens `bool`vom Typ. Wenn das Ereignis ausgelöst wird, gibt der Wert `Value` der Eigenschaft den neuen Wert der `IsToggled` Eigenschaft an.

## <a name="create-a-switch"></a>Erstellen eines Schalters

Eine `Switch` kann in XAML instanziiert werden. Die- `Switch`Eigenschaft kann festgelegt werden, um die zu aktivieren. `IsToggled` Standardmäßig ist `false`die `IsToggled` -Eigenschaft. Im folgenden Beispiel wird gezeigt, wie ein `Switch` in XAML mit dem optionalen `IsToggled` Eigenschaften Satz instanziiert wird:

```xaml
<Switch IsToggled="true"/>
```

Ein `Switch` kann auch im Code erstellt werden:

```csharp
Switch switchControl = new Switch { IsToggled = true };
```

## <a name="switch-appearance"></a>Darstellung wechseln

Zusätzlich zu den Eigenschaften [`Switch`](xref:Xamarin.Forms.Switch) , die von der [`View`](xref:Xamarin.Forms.View) -Klasse erben, `Switch` definiert `OnColor` auch die `ThumbColor` -Eigenschaft und die-Eigenschaft. Die `OnColor` -Eigenschaft kann festgelegt werden, `Switch` um die Farbe zu definieren, wenn Sie in Ihren **on** -Zustand gewechselt `ThumbColor` wird, und die-Eigenschaft kann `Color` festgelegt werden, um den des Zieh Punkts zu definieren. Im folgenden Beispiel wird gezeigt, wie ein `Switch` in XAML instanziiert wird, wobei diese Eigenschaften festgelegt werden:

```xaml
<Switch OnColor="Orange"
        ThumbColor="Green" />
```

Die Eigenschaften können auch beim Erstellen einer `Switch` im Code festgelegt werden:

```csharp
Switch switch = new Switch { OnColor = Color.Orange, ThumbColor = Color.Green };
```

Der folgende Screenshot zeigt den `Switch` in seinen **on** -und **Off** -UMSCHALT Zuständen, bei `OnColor` denen `ThumbColor` die-Eigenschaft und die-Eigenschaft festgelegt sind:

![Screenshot von Switches in ein-und Ausschalten in IOS und Android](switch-images/switch-states-colors.png "Switches unter IOS und Android")

## <a name="respond-to-a-switch-state-change"></a>Reagieren auf eine Änderung des Wechsel Status

Wenn sich `IsToggled` die-Eigenschaft ändert, entweder über die Benutzer Manipulation oder wenn eine `IsToggled` Anwendung die- `Toggled` Eigenschaft festlegt, wird das-Ereignis ausgelöst. Ein Ereignishandler für dieses Ereignis kann registriert werden, um auf die Änderung zu reagieren:

```xaml
<Switch Toggled="OnToggled" />
```

Die Code-Behind-Datei enthält den Handler für die `Toggled` Ereignis:

```csharp
void OnToggled(object sender, ToggledEventArgs e)
{
    // Perform an action after examining e.Value
}
```

Das `sender` -Argument im-Ereignishandler ist `Switch` der Verantwortliche für das Auslösen dieses Ereignisses. Sie `sender` können die-Eigenschaft verwenden, um `Switch` auf das-Objekt zuzugreifen, oder `Switch` um zwischen mehreren Objekten `Toggled` zu unterscheiden, die denselben Ereignishandler verwenden.

Der `Toggled` Ereignishandler kann auch im Code zugewiesen werden:

```csharp
Switch switchControl = new Switch {...};
switchControl.Toggled += (sender, e) =>
{
    // Perform an action after examining e.Value
}
```

## <a name="data-bind-a-switch"></a>Daten binden eines Schalters

Der `Toggled` Ereignishandler kann mithilfe von Datenbindung und Triggern gelöscht werden, um `Switch` auf geänderte Statusänderungen zu reagieren.

```xaml
<Switch x:Name="styleSwitch" />
<Label Text="Lorem ipsum dolor sit amet, elit rutrum, enim hendrerit augue vitae praesent sed non, lorem aenean quis praesent pede.">
    <Label.Triggers>
        <DataTrigger TargetType="Label"
                     Binding="{Binding Source={x:Reference styleSwitch}, Path=IsToggled}"
                     Value="true">
            <Setter Property="FontAttributes"
                    Value="Italic, Bold" />
            <Setter Property="FontSize"
                    Value="Large" />
        </DataTrigger>
    </Label.Triggers>
</Label>
```

[`Label`](xref:Xamarin.Forms.Label) In diesem Beispiel verwendet einen Bindungs Ausdruck in einer `IsToggled` `DataTrigger` , um die-Eigenschaft der mit dem `Switch` Namen `styleSwitch`zu überwachen. Wenn diese Eigenschaft wird `true`, `Label` werden `FontAttributes` die `FontSize` -Eigenschaft und die-Eigenschaft des-Objekts geändert. Wenn die `IsToggled` -Eigenschaft `false`zurückkehrt, `FontAttributes` werden`Label` die-Eigenschaft und die- `FontSize` Eigenschaft des auf den ursprünglichen Zustand zurückgesetzt.

Weitere Informationen zu Triggern finden Sie unter [xamarin. Forms-Trigger](~/xamarin-forms/app-fundamentals/triggers.md).

## <a name="disable-a-switch"></a>Deaktivieren eines Schalters

Eine Anwendung kann einen Status eingeben, in `Switch` dem ein-/ausgeschaltet ist, der kein gültiger Vorgang ist. In solchen Fällen kann der `Switch` deaktiviert werden, indem die zugehörige- `false` `IsEnabled` Eigenschaft auf festgelegt wird. Dadurch wird verhindert, dass Benutzer die `Switch`bearbeiten können.

## <a name="related-links"></a>Verwandte Links

* [Switchdemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-switchdemos/)
* [Xamarin. Forms-Trigger](~/xamarin-forms/app-fundamentals/triggers.md)
