---
Title: " Xamarin.Forms Switch" Description: "der Xamarin.Forms Schalter ist eine Art von Schaltfläche, die vom Benutzer geändert werden kann, um zwischen den Zuständen" ein "und" aus "zu wechseln. In diesem Artikel wird erläutert, wie Sie die Switch-Klasse verwenden, um ein umschlendes Benutzeroberflächen Element anzuzeigen.
ms. Prod: xamarin ms. assetid: B2F9CC65-481B-4323-8E77-C6BE29C90DE9 ms. Technology: xamarin-Forms Author: profexorgeek ms. Author: jusjohns ms. Date: 07/18/2019 NO-LOC: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="xamarinforms-switch"></a>Xamarin.FormsNot

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-switchdemos/)

Das- Xamarin.Forms [`Switch`](xref:Xamarin.Forms.Switch) Steuerelement ist eine horizontale UMSCHALT Fläche, die vom Benutzer geändert werden kann, um zwischen den Zuständen "ein" und "aus" zu wechseln, die durch einen-Wert dargestellt werden `boolean` . Die `Switch` Klasse erbt von [`View`](xref:Xamarin.Forms.View) .

Die folgenden Screenshots zeigen ein `Switch` Steuerelement in seinen **on** -und **Off** -UMSCHALT Zuständen unter IOS und Android:

![Screenshot von Switches in ein-und Ausschalten in IOS und Android](switch-images/switch-states-default.png "Switches unter IOS und Android")

Das- `Switch` Steuerelement definiert die folgenden Eigenschaften:

* [`IsToggled`](xref:Xamarin.Forms.Switch.IsToggled)ein `boolean` Wert, der angibt, ob `Switch` **auf on**fest liegt.
* [`OnColor`](xref:Xamarin.Forms.Switch.OnColor)ist eine `Color` , die sich darauf auswirkt, wie das-Objekt `Switch` im ein- **on**oder ausgeschaltet wird.
* `ThumbColor`ist der `Color` des Zieh Punkts.

Diese Eigenschaften werden durch ein- [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) Objekt unterstützt. Dies bedeutet, dass formatiert `Switch` werden kann und das Ziel von Daten Bindungen ist.

Das- `Switch` Steuerelement definiert ein- `Toggled` Ereignis, das ausgelöst wird, wenn die- `IsToggled` Eigenschaft geändert wird, entweder über eine Benutzer Bearbeitung oder eine Anwendung die-Eigenschaft festgelegt `IsToggled` . Das `ToggledEventArgs` Objekt, das das `Toggled` Ereignis begleitet, verfügt über eine einzelne Eigenschaft namens `Value` vom Typ `bool` . Wenn das Ereignis ausgelöst wird, gibt der Wert der `Value` Eigenschaft den neuen Wert der Eigenschaft an `IsToggled` .

## <a name="create-a-switch"></a>Erstellen eines Schalters

Eine `Switch` kann in XAML instanziiert werden. `IsToggled`Die-Eigenschaft kann festgelegt werden, um die zu aktivieren `Switch` . Standardmäßig ist die- `IsToggled` Eigenschaft `false` . Im folgenden Beispiel wird gezeigt, wie ein `Switch` in XAML mit dem optionalen Eigenschaften Satz instanziiert wird `IsToggled` :

```xaml
<Switch IsToggled="true"/>
```

Ein `Switch` kann auch im Code erstellt werden:

```csharp
Switch switchControl = new Switch { IsToggled = true };
```

## <a name="switch-appearance"></a>Darstellung wechseln

Zusätzlich zu den Eigenschaften, die [`Switch`](xref:Xamarin.Forms.Switch) von der- [`View`](xref:Xamarin.Forms.View) Klasse erben, `Switch` definiert auch die `OnColor` -Eigenschaft und die-Eigenschaft `ThumbColor` . Die `OnColor` -Eigenschaft kann festgelegt werden, um die Farbe zu definieren `Switch` , wenn Sie in Ihren **on** -Zustand gewechselt wird, und die- `ThumbColor` Eigenschaft kann festgelegt werden, um den `Color` des Zieh Punkts zu definieren. Im folgenden Beispiel wird gezeigt, wie ein in XAML instanziiert wird, `Switch` wobei diese Eigenschaften festgelegt werden:

```xaml
<Switch OnColor="Orange"
        ThumbColor="Green" />
```

Die Eigenschaften können auch beim Erstellen einer im Code festgelegt werden `Switch` :

```csharp
Switch switch = new Switch { OnColor = Color.Orange, ThumbColor = Color.Green };
```

Der folgende Screenshot zeigt den `Switch` in seinen **on** -und **Off** -UMSCHALT Zuständen, bei denen die `OnColor` -Eigenschaft und die-Eigenschaft `ThumbColor` festgelegt sind:

![Screenshot von Switches in ein-und Ausschalten in IOS und Android](switch-images/switch-states-colors.png "Switches unter IOS und Android")

## <a name="respond-to-a-switch-state-change"></a>Reagieren auf eine Änderung des Wechsel Status

Wenn sich die- `IsToggled` Eigenschaft ändert, entweder über die Benutzer Manipulation oder wenn eine Anwendung die- `IsToggled` Eigenschaft festlegt, wird das- `Toggled` Ereignis ausgelöst. Ein Ereignishandler für dieses Ereignis kann registriert werden, um auf die Änderung zu reagieren:

```xaml
<Switch Toggled="OnToggled" />
```

Die Code-Behind-Datei enthält den Handler für das- `Toggled` Ereignis:

```csharp
void OnToggled(object sender, ToggledEventArgs e)
{
    // Perform an action after examining e.Value
}
```

Das- `sender` Argument im-Ereignishandler ist der `Switch` Verantwortliche für das Auslösen dieses Ereignisses. Sie können die- `sender` Eigenschaft verwenden, um auf das- `Switch` Objekt zuzugreifen, oder um zwischen mehreren Objekten zu unterscheiden, `Switch` die denselben `Toggled` Ereignishandler verwenden.

Der `Toggled` Ereignishandler kann auch im Code zugewiesen werden:

```csharp
Switch switchControl = new Switch {...};
switchControl.Toggled += (sender, e) =>
{
    // Perform an action after examining e.Value
}
```

## <a name="data-bind-a-switch"></a>Daten binden eines Schalters

Der `Toggled` Ereignishandler kann mithilfe von Datenbindung und Triggern gelöscht werden, um auf geänderte Statusänderungen zu reagieren `Switch` .

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

In diesem Beispiel [`Label`](xref:Xamarin.Forms.Label) verwendet einen Bindungs Ausdruck in einer `DataTrigger` , um die- `IsToggled` Eigenschaft der mit dem Namen zu überwachen `Switch` `styleSwitch` . Wenn diese Eigenschaft wird `true` , `FontAttributes` werden die-Eigenschaft und die-Eigenschaft des-Objekts `FontSize` `Label` geändert. Wenn die `IsToggled` -Eigenschaft zurückkehrt `false` , `FontAttributes` werden die-Eigenschaft und die-Eigenschaft `FontSize` des `Label` auf den ursprünglichen Zustand zurückgesetzt.

Weitere Informationen zu Triggern finden Sie unter [ Xamarin.Forms Trigger](~/xamarin-forms/app-fundamentals/triggers.md).

## <a name="disable-a-switch"></a>Deaktivieren eines Schalters

Eine Anwendung kann einen Status eingeben, in dem ein-/ausgeschaltet `Switch` ist, der kein gültiger Vorgang ist. In solchen Fällen kann der deaktiviert werden, indem die zugehörige- `Switch` Eigenschaft auf festgelegt wird `IsEnabled` `false` . Dadurch wird verhindert, dass Benutzer die bearbeiten können `Switch` .

## <a name="related-links"></a>Verwandte Links

* [Switchdemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-switchdemos/)
* [Xamarin.Forms-Trigger](~/xamarin-forms/app-fundamentals/triggers.md)
