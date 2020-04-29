---
title: Optionsfeld "xamarin. Forms"
description: Die Options Schaltfläche xamarin. Forms ist eine Art von Schaltfläche, mit der Benutzer eine Option aus einer Menge auswählen können. Jede Option wird durch ein Optionsfeld dargestellt, und Sie können nur ein Optionsfeld in einer Gruppe auswählen.
ms.prod: xamarin
ms.assetid: E2AA40E0-69A5-41DF-BFC4-C151CA657451
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/13/2020
ms.openlocfilehash: 128ab4f6f00adaf86afc08eba37bcb81d3a04a90
ms.sourcegitcommit: 8d13d2262d02468c99c4e18207d50cd82275d233
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "82533000"
---
# <a name="xamarinforms-radiobutton"></a>Optionsfeld "xamarin. Forms"

[![Beispiel](~/media/shared/download.png) herunterladen herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-radiobuttondemos/)

Xamarin. Forms `RadioButton` ist ein Typ von Schaltfläche, mit dem Benutzer eine Option aus einer Menge auswählen können. Jede Option wird durch ein Optionsfeld dargestellt, und Sie können nur ein Optionsfeld in einer Gruppe auswählen. Die `RadioButton` Klasse erbt von der [`Button`](xref:Xamarin.Forms.Button) -Klasse.

Die folgenden Screenshots zeigen `RadioButton` Objekte in den gelöschten und ausgewählten Zuständen unter IOS und Android:

![Screenshot der Options Felder in den Status "ausgewählt" und "gelöscht" unter IOS und Android](radiobutton-images/radiobutton-states.png "Radiobuttons unter IOS und Android")

> [!IMPORTANT]
> `RadioButton`ist zurzeit experimentell und kann nur verwendet werden, indem `RadioButton_Experimental` das-Flag festgelegt wird. Weitere Informationen finden Sie unter [experimentelle Flags](~/xamarin-forms/internals/experimental-flags.md).

Das `RadioButton` -Steuerelement definiert die folgenden Eigenschaften:

- `IsChecked`vom Typ `bool`, der definiert, ob ausgewählt `RadioButton` ist. Diese Eigenschaft verwendet eine `TwoWay` -Bindung, und hat den Standardwert `false`.
- `GroupName`vom Typ `string`, der den Namen definiert, der angibt, `RadioButton` welche Steuerelemente sich gegenseitig ausschließen. Diese Eigenschaft hat den Standardwert `null`.

Diese Eigenschaften werden von [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) Objekten unterstützt. Dies bedeutet, dass Sie Ziele von Daten Bindungen und formatiert sein können.

Das `RadioButton` -Steuerelement `CheckedChanged` definiert ein-Ereignis, das `IsChecked` ausgelöst wird, wenn die-Eigenschaft geändert wird, entweder über Benutzer oder programmgesteuerte Bearbeitung. Das `CheckedChangedEventArgs` Objekt, das das `CheckedChanged` Ereignis begleitet, verfügt über eine `Value`einzelne Eigenschaft namens `bool`vom Typ. Wenn das Ereignis ausgelöst wird, wird der Wert der `Value` Eigenschaft auf den neuen Wert der `IsChecked` Eigenschaft festgelegt.

Außerdem erbt die `RadioButton` -Klasse die folgenden Eigenschaften, die normalerweise von der [`Button`](xref:Xamarin.Forms.Button) -Klasse verwendet werden:

- [`Command`](xref:Xamarin.Forms.Button.Command)vom Typ `ICommand`, der ausgeführt wird, wenn ausgewählt `RadioButton` wird.
- [`CommandParameter`](xref:Xamarin.Forms.Button.CommandParameter)vom Typ `object`, der dem-Parameter entspricht, der an den `Command`übergeben wird.
- [`FontAttributes`](xref:Xamarin.Forms.Button.FontAttributes)vom Typ [`FontAttributes`](xref:Xamarin.Forms.FontAttributes), der den Text Stil bestimmt.
- [`FontFamily`](xref:Xamarin.Forms.Button.FontFamily)vom Typ `string`, der die Schriftfamilie definiert.
- [`FontSize`](xref:Xamarin.Forms.Button.FontSize)vom Typ `double`, der den Schrift Grad definiert.
- [`Text`](xref:Xamarin.Forms.Button.Text)vom Typ, `string`der den anzuzeigenden Text definiert.
- [`TextColor`](xref:Xamarin.Forms.Button.TextColor)vom Typ [`Color`](xref:Xamarin.Forms.Color), der die Farbe des angezeigten Texts definiert.

Weitere Informationen zum- [`Button`](xref:Xamarin.Forms.Button) Steuerelement finden Sie unter [xamarin. Forms-Schaltfläche](~/xamarin-forms/user-interface/button.md).

## <a name="create-radiobuttons"></a>Erstellen von Radiobuttons

Im folgenden Beispiel wird gezeigt, wie Objekte in `RadioButton` XAML instanziiert werden:

```xaml
<StackLayout>
    <Label Text="What's your favorite animal?" />
    <RadioButton Text="Cat" />
    <RadioButton Text="Dog" />
    <RadioButton Text="Elephant" />
    <RadioButton Text="Monkey"
                 IsChecked="true" />
</StackLayout>
```

In diesem Beispiel werden `RadioButton` -Objekte implizit innerhalb desselben übergeordneten Containers gruppiert. Dieses XAML-Code Ergebnis wird in den folgenden Screenshots angezeigt:

![Screenshot der implizit gruppierten Radiobuttons unter IOS und Android](radiobutton-images/radiobuttons.png "Implizit gruppierte Radiobuttons unter IOS und Android")

Alternativ können `RadioButton` Objekte im Code erstellt werden:

```csharp
StackLayout stackLayout = new StackLayout
{
    Children =
    {
        new Label { Text = "What's your favorite animal?" },
        new RadioButton { Text = "Cat" },
        new RadioButton { Text = "Dog" },
        new RadioButton { Text = "Elephant" },
        new RadioButton { Text = "Monkey", IsChecked = true }
    }
};
```

## <a name="group-radiobuttons"></a>Radiobuttons gruppieren

Options Felder können in Gruppen verwendet werden, und es gibt zwei Ansätze, um Options Felder zu gruppieren:

- Indem Sie Sie im gleichen übergeordneten Container platzieren. Dies wird als implizite Gruppierung bezeichnet.
- Durch Festlegen der `GroupName` -Eigenschaft für jedes Optionsfeld auf denselben Wert. Dies wird als explizite Gruppierung bezeichnet.

Das folgende XAML-Beispiel zeigt das `RadioButton` explizite Gruppieren von Objekten `GroupName` durch Festlegen ihrer Eigenschaften:

```xaml
<Label Text="What's your favorite color?" />
<RadioButton Text="Red"
             TextColor="Red"
             GroupName="colors" />
<RadioButton Text="Green"
             TextColor="Green"
             GroupName="colors" />
<RadioButton Text="Blue"
             TextColor="Blue"
             GroupName="colors" />
<RadioButton Text="Other"
             GroupName="colors" />
```

In diesem Beispiel schließen sich `RadioButton` beide gegenseitig aus, da Sie denselben `GroupName` Wert haben. Dieses XAML-Code Ergebnis wird in den folgenden Screenshots angezeigt:

![Screenshot der explizit gruppierten Radiobuttons unter IOS und Android](radiobutton-images/grouped-radiobuttons.png "Explizites Gruppieren von Radiobuttons unter IOS und Android")

## <a name="respond-to-a-radiobutton-state-change"></a>Reaktion auf eine RadioButton-Statusänderung

Ein Optionsfeld hat zwei Zustände: aktiviert und deaktiviert. Wenn ein Optionsfeld ausgewählt wird, ist `IsChecked` `true`seine-Eigenschaft. Wenn ein Optionsfeld gelöscht wird, ist `IsChecked` `false`seine-Eigenschaft. Ein Optionsfeld kann durch Klicken auf ein anderes Optionsfeld in derselben Gruppe deaktiviert werden, jedoch nicht durch erneutes Klicken auf das Optionsfeld selbst. Sie können jedoch eine Options Schaltfläche Programm gesteuert löschen, indem `IsChecked` Sie die `false`zugehörige-Eigenschaft auf festlegen.

Wenn sich `IsChecked` die-Eigenschaft ändert, entweder über Benutzer-oder Programm `CheckedChanged` gesteuerte Bearbeitung, wird das-Ereignis ausgelöst. Ein Ereignishandler für dieses Ereignis kann registriert werden, um auf die Änderung zu reagieren:

```xaml
<RadioButton Text="Red"
             TextColor="Red"
             GroupName="colors"
             CheckedChanged="OnColorsRadioButtonCheckedChanged" />
```

Der Code Behind enthält den Handler für das `CheckedChanged` -Ereignis:

```csharp
void OnColorsRadioButtonCheckedChanged(object sender, CheckedChangedEventArgs e)
{
    // Perform required operation
}
```

Das `sender` -Argument ist `RadioButton` der Verantwortliche für dieses Ereignis. Sie können dies verwenden, um auf `RadioButton` das Objekt zuzugreifen, oder um zwischen `RadioButton` mehreren Objekten zu unter `CheckedChanged` scheiden, die denselben Ereignishandler verwenden.

Alternativ kann ein Ereignishandler für das `CheckedChanged` -Ereignis im Code registriert werden:

```csharp
RadioButton radioButton = new RadioButton { ... };
radioButton.CheckedChanged += (sender, e) =>
{
    // Perform required operation
};
```

> [!NOTE]
> Eine alternative Methode, um auf eine `RadioButton` Statusänderung zu reagieren, besteht `ICommand` darin, ein zu definieren `RadioButton.Command` und es der-Eigenschaft zuzuweisen. Weitere Informationen finden Sie unter [Schaltfläche: Verwenden der Befehlsschnittstelle](~/xamarin-forms/user-interface/button.md#using-the-command-interface).

## <a name="radiobutton-visual-states"></a>Visuelle Zustände von RadioButton

`RadioButton`verfügt über `IsChecked` [`VisualState`](xref:Xamarin.Forms.VisualState) einen, der zum Initiieren einer visuellen Änderung verwendet werden kann `RadioButton` , wenn eine ausgewählt wird.

Das folgende XAML-Beispiel zeigt, wie Sie einen visuellen Zustand für `IsChecked` den Status definieren:

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <Style TargetType="RadioButton">
            <Setter Property="VisualStateManager.VisualStateGroups">
                <VisualStateGroupList>
                    <VisualStateGroup x:Name="CommonStates">
                        <VisualState x:Name="Normal">
                            <VisualState.Setters>
                                <Setter Property="TextColor"
                                        Value="Red" />
                                <Setter Property="Opacity"
                                        Value="0.5" />
                            </VisualState.Setters>
                        </VisualState>
                        <VisualState x:Name="IsChecked">
                            <VisualState.Setters>
                                <Setter Property="TextColor"
                                        Value="Green" />
                                <Setter Property="Opacity"
                                        Value="1" />
                            </VisualState.Setters>
                        </VisualState>
                    </VisualStateGroup>
                </VisualStateGroupList>
            </Setter>
        </Style>
    </ContentPage.Resources>
    <StackLayout>
        <Label Text="What's your favorite mode of transport?" />
        <RadioButton Text="Car"
                     CheckedChanged="OnRadioButtonCheckedChanged" />
        <RadioButton Text="Bike"
                     CheckedChanged="OnRadioButtonCheckedChanged" />
        <RadioButton Text="Train"
                     CheckedChanged="OnRadioButtonCheckedChanged" />
        <RadioButton Text="Walking"
                     CheckedChanged="OnRadioButtonCheckedChanged" />
    </StackLayout>
</ContentPage>
```

In diesem Beispiel sind die impliziten [`Style`](xref:Xamarin.Forms.Style) Targets `RadioButton` -Objekte. Der `IsChecked` [`VisualState`](xref:Xamarin.Forms.VisualState) gibt an, dass `RadioButton` die- `TextColor` Eigenschaft bei Auswahl von auf grün mit dem `Opacity` Wert 1 festgelegt wird. Der `Normal` `VisualState` gibt an, dass `RadioButton` die- `TextColor` Eigenschaft, wenn sich ein in einem gelöschten Zustand befindet, auf `Opacity` rot mit einem Wert von 0,5 festgelegt wird. Der Gesamteffekt besteht daher darin, dass beim `RadioButton` Löschen von rot und teilweise transparent ist und bei Auswahl von Grün ohne Transparenz angezeigt wird:

![Screenshot der RadioButton-Darstellung, die durch den visuellen Zustand festgelegt ist, unter IOS und Android](radiobutton-images/ischecked-visualstate.png "Visuelle Zustände von RadioButton unter IOS und Android")

Weitere Informationen zu visuellen Zuständen finden Sie unter [xamarin. Forms Visual State Manager](~/xamarin-forms/user-interface/visual-state-manager.md).

## <a name="disable-a-radiobutton"></a>Optionsfeld deaktivieren

Manchmal wird eine Anwendung in einen Zustand versetzt `RadioButton` , in dem eine überprüfte keine gültige Operation ist. In solchen Fällen kann der `RadioButton` deaktiviert werden, indem die zugehörige `IsEnabled` - `false`Eigenschaft auf festgelegt wird.

## <a name="related-links"></a>Verwandte Links

- [RadioButton-Demos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-radiobuttondemos/)
- [Schaltfläche "xamarin. Forms"](~/xamarin-forms/user-interface/button.md)
- [Xamarin. Forms Visual State Manager](~/xamarin-forms/user-interface/visual-state-manager.md)
