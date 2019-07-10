---
title: Xamarin.Forms-Schalter
description: Der Xamarin.Forms-Schalter ist eine Art von Schaltfläche, die vom Benutzer die zwischen ein- und ausschalten Zustände bearbeitet werden kann. In diesem Artikel wird erläutert, wie mit der Switch-Klasse ein Umschaltelemente Element der Benutzeroberfläche angezeigt wird.
ms.prod: xamarin
ms.assetId: B2F9CC65-481B-4323-8E77-C6BE29C90DE9
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 07/03/2019
ms.openlocfilehash: 22a17f9a916d94a3a0f44a451512de43c943e95a
ms.sourcegitcommit: 58d8bbc19ead3eb535fb8248710d93ba0892e05d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2019
ms.locfileid: "67675039"
---
# <a name="xamarinforms-switch"></a>Xamarin.Forms-Schalter

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/SwitchDemos)

Die Xamarin.Forms [ `Switch` ](xref:Xamarin.Forms.Switch) eine horizontale Umschaltfläche, die bearbeitet werden können, durch den Benutzer, die zwischen ein- und ausschalten Zustände, die dargestellt werden. durch eine `boolean` Wert. Die `Switch` Klasse erbt von [ `View` ](xref:Xamarin.Forms.View).

Der folgende Screenshot zeigt eine `Switch` steuern, dessen **auf** und **aus** Zustände unter iOS und Android zu wechseln:

![Screenshot der Schalter in ein- und Ausschalten der Zustände, die unter iOS und Android](switch-images/switch-states-default.png "unter iOS und Android-Switches")

Die `Switch` -Steuerelement definiert zwei Eigenschaften:

* [`OnColor`](xref:Xamarin.Forms.Switch.OnColor) ist eine `Color` , beeinflusst die `Switch` ist im umgeschalteten, gerendert oder **auf**, Status.
* [`IsToggled`](xref:Xamarin.Forms.Switch.IsToggled) ist eine `boolean` Wert, der angibt, ob die `Switch` ist **auf**.

Diese Eigenschaften verfügen über eine [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) -Objekt, das bedeutet, dass die `Switch` formatiert werden können und das Ziel von datenbindungen verwendet werden.

Die `Switch` -Steuerelement definiert eine `Toggled` Ereignis, wenn ausgelöst wird, die `IsToggled` eigenschaftsänderungen, entweder durch Bearbeitung durch Benutzer oder wenn eine Anwendung festlegt der `IsToggled` Eigenschaft. Die `ToggledEventArgs` Objekt an, die mit der `Toggled` Ereignis verfügt über eine einzelne Eigenschaft namens `Value`, des Typs `bool`. Wenn das Ereignis ausgelöst wird, wird den Wert des der `Value` Eigenschaft spiegelt wider, den neuen Wert der `IsToggled` Eigenschaft.

## <a name="create-a-switch"></a>Erstellen Sie einen Switch

Ein `Switch` in XAML instanziiert werden. Die `IsToggled` Eigenschaft kann festgelegt werden, um das Umschalten der `Switch`. In der Standardeinstellung die `IsToggled` Eigenschaft `false`. Das folgende Beispiel zeigt, wie Sie instanziieren ein `Switch` in XAML mit dem optionalen `IsToggled` Eigenschaftensatz:

```xaml
<Switch IsToggled="true"/>
```

Ein `Switch` können auch im Code erstellt werden:

```csharp
Switch switch = new Switch { IsToggled = true };
```

### <a name="switch-style-properties"></a>Wechseln Sie Stileigenschaften

Die `OnColor` Eigenschaft kann festgelegt werden, definieren die `Switch` Farbe, wenn es um umgeschaltet wird seine **auf** Zustand. Das folgende Beispiel zeigt, wie Sie instanziieren ein `Switch` in XAML mit dem `OnColor` Eigenschaftensatz:

```xaml
<Switch OnColor="Orange" />
```

Die `OnColor` Eigenschaft kann auch festgelegt werden, beim Erstellen einer `Switch` im Code:

```csharp
Switch switch = new Switch { OnColor = Color.Orange };
```

Der folgende Screenshot zeigt die `Switch` in seine **auf** und **aus** wechseln von Zuständen, mit der `OnColor` -Eigenschaftensatz auf `Color.Orange` unter iOS und Android:

![Screenshot der Schalter in ein- und Ausschalten der Zustände, die unter iOS und Android](switch-images/switch-states-oncolor.png "unter iOS und Android-Switches")

## <a name="respond-to-a-switch-state-change"></a>Reagieren Sie auf eine Zustandsänderung Switch

Wenn die `IsToggled` eigenschaftsänderungen, entweder durch Bearbeitung durch Benutzer oder wenn eine Anwendung festlegt der `IsToggled` -Eigenschaft, die `Toggled` Ereignis wird ausgelöst. Ein Ereignishandler für dieses Ereignis kann registriert werden, um auf die Änderung reagieren:

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

Die `sender` Argument im Ereignishandler ist der `Switch` verantwortlich für das Auslösen dieses Ereignisses. Können Sie die `sender` Eigenschaft, die Zugriff auf die `Switch` Objekt oder zur Unterscheidung zwischen mehreren `Switch` Objekte, die gemeinsame Verwendung desselben `Toggled` -Ereignishandler.

Die `Toggled` -Ereignishandler kann auch im Code zugewiesen werden:

```csharp
Switch switch = new Switch {...};
switch.Toggled += (sender, e) =>
{
    // Perform an action after examining e.Value
}
```

## <a name="data-bind-a-switch"></a>Binden von Daten einen Switch

Die `Toggled` -Ereignishandler kann behoben werden, mithilfe von Datenbindung und Trigger reagieren auf eine `Switch` Umschaltzustände ändern.

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

In diesem Beispiel die [ `Label` ](xref:Xamarin.Forms.Label) verwendet einen Bindungsausdruck in einer `DataTrigger` zum Überwachen der `IsToggled` Eigenschaft der `Switch` mit dem Namen `styleSwitch`. Wenn diese Eigenschaft wird `true`, `FontAttributes` und `FontSize` Eigenschaften der `Label` geändert werden. Wenn der `IsToggled` -Eigenschaft gibt, `false`, wird die `FontAttributes` und `FontSize` Eigenschaften der `Label` auf ihren ursprünglichen Status zurückgesetzt.

Weitere Informationen zu Triggern finden Sie unter [Xamarin.Forms Trigger](~/xamarin-forms/app-fundamentals/triggers.md).

## <a name="disable-a-switch"></a>Deaktivieren Sie einen Switch

Eine Anwendung kann einen versetzt, in denen die `Switch` ein-oder ausgeschaltet ist kein gültiger Vorgang. In solchen Fällen die `Switch` können deaktiviert werden, indem die `IsEnabled` Eigenschaft `false`. Dies hindert Benutzer nicht bearbeiten können die `Switch`.

## <a name="related-links"></a>Verwandte Links

* [Switch-Demos](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/SwitchDemos)
* [Xamarin.Forms-Trigger](~/xamarin-forms/app-fundamentals/triggers.md)
