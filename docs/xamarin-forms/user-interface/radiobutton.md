---
title: Xamarin.FormsRadioButton
description: Xamarin.FormsRadioButton ist ein Typ von Schaltfläche, mit dem Benutzer eine Option aus einer Menge auswählen können. Jede Option wird durch ein Optionsfeld dargestellt, und Sie können nur ein Optionsfeld in einer Gruppe auswählen.
ms.prod: xamarin
ms.assetid: E2AA40E0-69A5-41DF-BFC4-C151CA657451
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/13/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: f7cbd11f98127cb73514112dae785102ff9c51c0
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84127622"
---
# <a name="xamarinforms-radiobutton"></a>Xamarin.FormsRadioButton

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-radiobuttondemos/)

Der Xamarin.Forms `RadioButton` ist ein Typ von Schaltfläche, mit dem Benutzer eine Option aus einer Menge auswählen können. Jede Option wird durch ein Optionsfeld dargestellt, und Sie können nur ein Optionsfeld in einer Gruppe auswählen. Die `RadioButton` Klasse erbt von der- [`Button`](xref:Xamarin.Forms.Button) Klasse.

Die folgenden Screenshots zeigen `RadioButton` Objekte in den gelöschten und ausgewählten Zuständen unter IOS und Android:

![Screenshot der Options Felder in den Status "ausgewählt" und "gelöscht" unter IOS und Android](radiobutton-images/radiobutton-states.png "Radiobuttons unter IOS und Android")

> [!IMPORTANT]
> `RadioButton`ist zurzeit experimentell und kann nur verwendet werden, indem das-Flag festgelegt wird `RadioButton_Experimental` . Weitere Informationen finden Sie unter [experimentelle Flags](~/xamarin-forms/internals/experimental-flags.md).

Das- `RadioButton` Steuerelement definiert die folgenden Eigenschaften:

- `IsChecked`vom Typ `bool` , der definiert, ob `RadioButton` ausgewählt ist. Diese Eigenschaft verwendet eine `TwoWay` -Bindung, und hat den Standardwert `false` .
- `GroupName`vom Typ `string` , der den Namen definiert, der angibt, welche Steuer `RadioButton` Elemente sich gegenseitig ausschließen. Diese Eigenschaft hat den Standardwert `null` .

Diese Eigenschaften werden von Objekten unterstützt [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . Dies bedeutet, dass Sie Ziele von Daten Bindungen und formatiert sein können.

Das `RadioButton` -Steuerelement definiert ein `CheckedChanged` -Ereignis, das ausgelöst wird, wenn die- `IsChecked` Eigenschaft geändert wird, entweder über Benutzer oder programmgesteuerte Bearbeitung. Das `CheckedChangedEventArgs` Objekt, das das `CheckedChanged` Ereignis begleitet, verfügt über eine einzelne Eigenschaft namens `Value` vom Typ `bool` . Wenn das Ereignis ausgelöst wird, wird der Wert der `Value` Eigenschaft auf den neuen Wert der Eigenschaft festgelegt `IsChecked` .

Außerdem `RadioButton` erbt die-Klasse die folgenden Eigenschaften, die normalerweise von der-Klasse verwendet werden [`Button`](xref:Xamarin.Forms.Button) :

- [`Command`](xref:Xamarin.Forms.Button.Command)vom Typ `ICommand` , der ausgeführt wird, wenn `RadioButton` ausgewählt wird.
- [`CommandParameter`](xref:Xamarin.Forms.Button.CommandParameter)vom Typ `object` , der dem-Parameter entspricht, der an den übergeben wird `Command` .
- [`FontAttributes`](xref:Xamarin.Forms.Button.FontAttributes)vom Typ [`FontAttributes`](xref:Xamarin.Forms.FontAttributes) , der den Text Stil bestimmt.
- [`FontFamily`](xref:Xamarin.Forms.Button.FontFamily)vom Typ `string` , der die Schriftfamilie definiert.
- [`FontSize`](xref:Xamarin.Forms.Button.FontSize)vom Typ `double` , der den Schrift Grad definiert.
- [`Text`](xref:Xamarin.Forms.Button.Text)vom Typ `string` , der den anzuzeigenden Text definiert.
- [`TextColor`](xref:Xamarin.Forms.Button.TextColor)vom Typ [`Color`](xref:Xamarin.Forms.Color) , der die Farbe des angezeigten Texts definiert.

Weitere Informationen zum-Steuerelement finden Sie unter [`Button`](xref:Xamarin.Forms.Button) [ Xamarin.Forms Schaltfläche](~/xamarin-forms/user-interface/button.md).

## <a name="create-radiobuttons"></a>Erstellen von Radiobuttons

Im folgenden Beispiel wird gezeigt, wie `RadioButton` Objekte in XAML instanziiert werden:

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

In diesem Beispiel `RadioButton` werden-Objekte implizit innerhalb desselben übergeordneten Containers gruppiert. Dieses XAML-Code Ergebnis wird in den folgenden Screenshots angezeigt:

![Screenshot der implizit gruppierten Radiobuttons unter IOS und Android](radiobutton-images/radiobuttons.png "Implizit gruppierte Radiobuttons unter IOS und Android")

Alternativ `RadioButton` können Objekte im Code erstellt werden:

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
- Durch Festlegen der- `GroupName` Eigenschaft für jedes Optionsfeld auf denselben Wert. Dies wird als explizite Gruppierung bezeichnet.

Das folgende XAML-Beispiel zeigt das explizite gruppieren `RadioButton` von Objekten durch Festlegen ihrer `GroupName` Eigenschaften:

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

Ein Optionsfeld hat zwei Zustände: aktiviert und deaktiviert. Wenn ein Optionsfeld ausgewählt wird, ist seine- `IsChecked` Eigenschaft `true` . Wenn ein Optionsfeld gelöscht wird, ist seine- `IsChecked` Eigenschaft `false` . Ein Optionsfeld kann durch Klicken auf ein anderes Optionsfeld in derselben Gruppe deaktiviert werden, jedoch nicht durch erneutes Klicken auf das Optionsfeld selbst. Sie können jedoch eine Options Schaltfläche Programm gesteuert löschen, indem Sie die zugehörige- `IsChecked` Eigenschaft auf festlegen `false` .

Wenn sich die- `IsChecked` Eigenschaft ändert, entweder über Benutzer-oder programmgesteuerte Bearbeitung, wird das- `CheckedChanged` Ereignis ausgelöst. Ein Ereignishandler für dieses Ereignis kann registriert werden, um auf die Änderung zu reagieren:

```xaml
<RadioButton Text="Red"
             TextColor="Red"
             GroupName="colors"
             CheckedChanged="OnColorsRadioButtonCheckedChanged" />
```

Der Code Behind enthält den Handler für das- `CheckedChanged` Ereignis:

```csharp
void OnColorsRadioButtonCheckedChanged(object sender, CheckedChangedEventArgs e)
{
    // Perform required operation
}
```

Das- `sender` Argument ist der `RadioButton` Verantwortliche für dieses Ereignis. Sie können dies verwenden, um auf das `RadioButton` Objekt zuzugreifen, oder um zwischen mehreren Objekten zu unterscheiden, `RadioButton` die denselben `CheckedChanged` Ereignishandler verwenden.

Alternativ kann ein Ereignishandler für das- `CheckedChanged` Ereignis im Code registriert werden:

```csharp
RadioButton radioButton = new RadioButton { ... };
radioButton.CheckedChanged += (sender, e) =>
{
    // Perform required operation
};
```

> [!NOTE]
> Eine alternative Methode, um auf eine `RadioButton` Statusänderung zu reagieren, besteht darin, ein zu definieren `ICommand` und es der- `RadioButton.Command` Eigenschaft zuzuweisen. Weitere Informationen finden Sie unter [Schaltfläche: Verwenden der Befehlsschnittstelle](~/xamarin-forms/user-interface/button.md#using-the-command-interface).

## <a name="radiobutton-visual-states"></a>Visuelle Zustände von RadioButton

`RadioButton`verfügt über einen `IsChecked` [`VisualState`](xref:Xamarin.Forms.VisualState) , der zum Initiieren einer visuellen Änderung verwendet werden kann, wenn eine `RadioButton` ausgewählt wird.

Das folgende XAML-Beispiel zeigt, wie Sie einen visuellen Zustand für den `IsChecked` Status definieren:

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

Bei diesem Beispiel gilt der implizite [`Style`](xref:Xamarin.Forms.Style) für `RadioButton`-Objekte. Der `IsChecked` [`VisualState`](xref:Xamarin.Forms.VisualState) gibt an, dass die- `RadioButton` Eigenschaft bei Auswahl `TextColor` von auf grün mit dem Wert 1 festgelegt wird `Opacity` . Der gibt an, dass die- `Normal` `VisualState` Eigenschaft, wenn sich ein `RadioButton` in einem gelöschten Zustand befindet, `TextColor` auf rot mit einem `Opacity` Wert von 0,5 festgelegt wird. Der Gesamteffekt besteht daher darin, dass beim `RadioButton` Löschen von rot und teilweise transparent ist und bei Auswahl von Grün ohne Transparenz angezeigt wird:

![Screenshot der RadioButton-Darstellung, die durch den visuellen Zustand festgelegt ist, unter IOS und Android](radiobutton-images/ischecked-visualstate.png "Visuelle Zustände von RadioButton unter IOS und Android")

Weitere Informationen zu visuellen Zuständen finden Sie unter [Xamarin.Forms: Visual State-Manager](~/xamarin-forms/user-interface/visual-state-manager.md).

## <a name="disable-a-radiobutton"></a>Optionsfeld deaktivieren

Manchmal wird eine Anwendung in einen Zustand versetzt, in dem eine über `RadioButton` prüfte keine gültige Operation ist. In solchen Fällen kann der deaktiviert werden, indem die zugehörige- `RadioButton` Eigenschaft auf festgelegt wird `IsEnabled` `false` .

## <a name="related-links"></a>Verwandte Links

- [RadioButton-Demos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-radiobuttondemos/)
- [Xamarin.Forms-Schaltfläche](~/xamarin-forms/user-interface/button.md)
- [Xamarin.Forms: Visual State-Manager](~/xamarin-forms/user-interface/visual-state-manager.md)
