---
title: Xamarin. Forms-Schalter
description: Der xamarin. Forms-Schalter ist ein Typ von Schaltfläche, der vom Benutzer geändert werden kann, um zwischen den Zuständen ein-und auszuschalten. In diesem Artikel wird erläutert, wie Sie die Switch-Klasse verwenden, um ein umschlendes Benutzeroberflächen Element anzuzeigen.
ms.prod: xamarin
ms.assetId: B2F9CC65-481B-4323-8E77-C6BE29C90DE9
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 07/18/2019
ms.openlocfilehash: 88655aabdbd32db63aaf3330a18b0ad8105ea26c
ms.sourcegitcommit: b751605179bef8eee2df92cb484011a7dceb6fda
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/20/2020
ms.locfileid: "77506540"
---
# <a name="xamarinforms-switch"></a>Xamarin. Forms-Schalter

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-switchdemos/)

Das xamarin. Forms [`Switch`](xref:Xamarin.Forms.Switch) -Steuerelement ist eine horizontale UMSCHALT Fläche, die vom Benutzer geändert werden kann, um zwischen einem-und einem-Zustand zu wechseln, die durch einen `boolean`-Wert dargestellt werden. Die `Switch`-Klasse erbt von [`View`](xref:Xamarin.Forms.View).

Die folgenden Screenshots zeigen ein `Switch`-Steuerelement in seinen **on** -und **Off** -UMSCHALT Zuständen unter IOS und Android:

![Screenshot von Switches in ein-und Ausschalten in IOS und Android](switch-images/switch-states-default.png "Switches unter IOS und Android")

Das `Switch`-Steuerelement definiert die folgenden Eigenschaften:

* [`IsToggled`](xref:Xamarin.Forms.Switch.IsToggled) ist ein `boolean` Wert, der angibt, ob die `Switch` **auf on**fest liegt.
* [`OnColor`](xref:Xamarin.Forms.Switch.OnColor) ist eine `Color`, die sich darauf auswirkt, wie die `Switch` im ein **-oder ausgeschaltet**wird.
* `ThumbColor` ist die `Color` des Zieh Punkts.

Diese Eigenschaften werden durch ein [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) Objekt gestützt. Dies bedeutet, dass die `Switch` formatiert und das Ziel von Daten Bindungen sein kann.

Das `Switch`-Steuerelement definiert ein `Toggled` Ereignis, das ausgelöst wird, wenn sich die `IsToggled`-Eigenschaft ändert, entweder durch Benutzer Manipulation oder wenn eine Anwendung die `IsToggled`-Eigenschaft festlegt. Das `ToggledEventArgs` Objekt, das das `Toggled` Ereignis begleitet, verfügt über eine einzelne Eigenschaft mit dem Namen `Value`vom Typ `bool`. Wenn das Ereignis ausgelöst wird, gibt der Wert der `Value`-Eigenschaft den neuen Wert der `IsToggled`-Eigenschaft wieder.

## <a name="create-a-switch"></a>Erstellen eines Schalters

Eine `Switch` kann in XAML instanziiert werden. Die `IsToggled`-Eigenschaft kann festgelegt werden, um die `Switch`zu wechseln. Standardmäßig ist die `IsToggled`-Eigenschaft `false`. Im folgenden Beispiel wird gezeigt, wie ein `Switch` in XAML mit dem optionalen `IsToggled` Eigenschaften Satz instanziiert wird:

```xaml
<Switch IsToggled="true"/>
```

Eine `Switch` kann auch im Code erstellt werden:

```csharp
Switch switchControl = new Switch { IsToggled = true };
```

## <a name="switch-appearance"></a>Darstellung wechseln

Zusätzlich zu den Eigenschaften, die [`Switch`](xref:Xamarin.Forms.Switch) von der [`View`](xref:Xamarin.Forms.View) -Klasse erbt, definiert `Switch` auch `OnColor`-und `ThumbColor`-Eigenschaften. Die `OnColor`-Eigenschaft kann festgelegt werden, um die `Switch` Farbe zu definieren, wenn Sie in Ihren **on** -Zustand gewechselt wird, und die `ThumbColor`-Eigenschaft kann festgelegt werden, um die `Color` des Zieh Punkts zu definieren. Im folgenden Beispiel wird gezeigt, wie ein `Switch` in XAML instanziiert wird, wobei diese Eigenschaften festgelegt werden:

```xaml
<Switch OnColor="Orange"
        ThumbColor="Green" />
```

Die Eigenschaften können auch beim Erstellen einer `Switch` im Code festgelegt werden:

```csharp
Switch switch = new Switch { OnColor = Color.Orange, ThumbColor = Color.Green };
```

Der folgende Screenshot zeigt die `Switch` in den **ein-** und **ausschalten** -Status, bei denen die Eigenschaften `OnColor` und `ThumbColor` festgelegt sind:

![Screenshot von Switches in ein-und Ausschalten in IOS und Android](switch-images/switch-states-colors.png "Switches unter IOS und Android")

## <a name="respond-to-a-switch-state-change"></a>Reagieren auf eine Änderung des Wechsel Status

Wenn die `IsToggled`-Eigenschaft geändert wird, entweder durch Benutzer Manipulation oder wenn eine Anwendung die `IsToggled`-Eigenschaft festlegt, wird das `Toggled`-Ereignis ausgelöst. Ein Ereignishandler für dieses Ereignis kann registriert werden, um auf die Änderung zu reagieren:

```xaml
<Switch Toggled="OnToggled" />
```

Die Code-Behind-Datei enthält den Handler für das `Toggled`-Ereignis:

```csharp
void OnToggled(object sender, ToggledEventArgs e)
{
    // Perform an action after examining e.Value
}
```

Das `sender`-Argument im-Ereignishandler ist der `Switch`, der für das Auslösen dieses Ereignisses verantwortlich ist. Sie können die `sender`-Eigenschaft verwenden, um auf das `Switch` Objekt zuzugreifen, oder um zwischen mehreren `Switch` Objekten zu unterscheiden, die denselben `Toggled` Ereignishandler verwenden.

Der `Toggled` Ereignishandler kann auch im Code zugewiesen werden:

```csharp
Switch switchControl = new Switch {...};
switchControl.Toggled += (sender, e) =>
{
    // Perform an action after examining e.Value
}
```

## <a name="data-bind-a-switch"></a>Daten binden eines Schalters

Der `Toggled`-Ereignishandler kann mithilfe von Datenbindung und Triggern gelöscht werden, um auf eine `Switch` ändernde UMSCHALT Zustände zu reagieren.

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

In diesem Beispiel verwendet die [`Label`](xref:Xamarin.Forms.Label) einen Bindungs Ausdruck in einer `DataTrigger`, um die `IsToggled`-Eigenschaft der `Switch` mit dem Namen `styleSwitch`zu überwachen. Wenn diese Eigenschaft `true`wird, werden die Eigenschaften `FontAttributes` und `FontSize` der `Label` geändert. Wenn die `IsToggled`-Eigenschaft zu `false`zurückkehrt, werden die Eigenschaften `FontAttributes` und `FontSize` der `Label` auf ihren ursprünglichen Zustand zurückgesetzt.

Weitere Informationen zu Triggern finden Sie unter [xamarin. Forms-Trigger](~/xamarin-forms/app-fundamentals/triggers.md).

## <a name="disable-a-switch"></a>Deaktivieren eines Schalters

Eine Anwendung kann einen Status eingeben, in dem die `Switch`, die ein-/ausgeschaltet wird, kein gültiger Vorgang ist. In solchen Fällen kann die `Switch` durch Festlegen der `IsEnabled`-Eigenschaft auf `false`deaktiviert werden. Dadurch wird verhindert, dass Benutzer die `Switch`bearbeiten können.

## <a name="related-links"></a>Verwandte Links

* [Switchdemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-switchdemos/)
* [Xamarin. Forms-Trigger](~/xamarin-forms/app-fundamentals/triggers.md)
