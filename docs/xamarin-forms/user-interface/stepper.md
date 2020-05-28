---
title: Xamarin.FormsStänder
description: Der Xamarin.Forms Stepper ermöglicht einem Benutzer, einen numerischen Wert aus einem Wertebereich auszuwählen. Er besteht aus zwei Schaltflächen mit minus-und Pluszeichen. Beim Bearbeiten der beiden Schaltflächen wird der ausgewählte Wert inkrementell geändert.
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 4f071530fb17de44d8ede786ca1b42f5e11f4f7c
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2020
ms.locfileid: "84130545"
---
# <a name="xamarinforms-stepper"></a>Xamarin.FormsStänder

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-stepperdemos)

_Verwenden Sie einen Stepper zum Auswählen eines numerischen Werts aus einem Wertebereich._

Das Xamarin.Forms [`Stepper`](xref:Xamarin.Forms.Stepper) besteht aus zwei Schaltflächen mit minus-und Pluszeichen. Diese Schaltflächen können vom Benutzer so bearbeitet werden, dass ein `double` Wert aus einem Wertebereich inkrementell ausgewählt wird.

[`Stepper`](xref:Xamarin.Forms.Stepper)Definiert vier Eigenschaften vom Typ `double` :

- [`Increment`](xref:Xamarin.Forms.Stepper.Increment)der Betrag, um den der ausgewählte Wert geändert werden soll. der Standardwert ist 1.
- [`Minimum`](xref:Xamarin.Forms.Stepper.Minimum)der Mindestwert des Bereichs, der Standardwert ist 0 (null).
- [`Maximum`](xref:Xamarin.Forms.Stepper.Maximum)ist der Höchstwert des Bereichs. der Standardwert ist 100.
- [`Value`](xref:Xamarin.Forms.Stepper.Value)ist der Wert des Stepper, der zwischen und liegen `Minimum` kann `Maximum` und den Standardwert 0 aufweist.

Alle diese Eigenschaften werden von-Objekten unterstützt [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . Die- [`Value`](xref:Xamarin.Forms.Stepper.Value) Eigenschaft verfügt über einen Standard Bindungs Modus von [`BindingMode.TwoWay`](xref:Xamarin.Forms.BindingMode.TwoWay) . Dies bedeutet, dass Sie als Bindungs Quelle in einer Anwendung geeignet ist, die die [Model-View-ViewModel (MVVM)-](~/xamarin-forms/enterprise-application-patterns/mvvm.md) Architektur verwendet.

> [!WARNING]
> Intern [`Stepper`](xref:Xamarin.Forms.Stepper) stellt sicher, dass [`Minimum`](xref:Xamarin.Forms.Stepper.Minimum) kleiner als ist [`Maximum`](xref:Xamarin.Forms.Stepper.Maximum) . Wenn `Minimum` oder `Maximum` jemals festgelegt sind, sodass `Minimum` nicht kleiner als ist `Maximum` , wird eine Ausnahme ausgelöst. Weitere Informationen zum Festlegen der `Minimum` Eigenschaften und `Maximum` finden Sie im Abschnitt [Vorsichtsmaßnahmen](#precautions) .

Der wandelt [`Stepper`](xref:Xamarin.Forms.Stepper) die [`Value`](xref:Xamarin.Forms.Stepper.Value) -Eigenschaft so um, dass Sie zwischen und (einschließlich) liegt [`Minimum`](xref:Xamarin.Forms.Stepper.Minimum) [`Maximum`](xref:Xamarin.Forms.Stepper.Maximum) . Wenn die `Minimum` Eigenschaft auf einen Wert festgelegt ist, der größer als die- `Value` Eigenschaft ist, wird `Stepper` die- `Value` Eigenschaft von auf festgelegt `Minimum` . Wenn `Maximum` auf einen Wert kleiner als festgelegt wird `Value` , dann wird `Stepper` die- `Value` Eigenschaft auf festgelegt `Maximum` .

[`Stepper`](xref:Xamarin.Forms.Stepper)definiert ein- [`ValueChanged`](xref:Xamarin.Forms.Stepper.ValueChanged) Ereignis, das ausgelöst wird [`Value`](xref:Xamarin.Forms.Stepper.Value) , wenn die geändert werden, entweder über die Benutzer Bearbeitung von oder, wenn die- `Stepper` Eigenschaft von der Anwendung direkt festgelegt wird `Value` . Ein- `ValueChanged` Ereignis wird auch ausgelöst, wenn die- `Value` Eigenschaft wie im vorherigen Absatz beschrieben erzwungen wird.

Das [`ValueChangedEventArgs`](xref:Xamarin.Forms.ValueChangedEventArgs) Objekt, das das [`ValueChanged`](xref:Xamarin.Forms.Stepper.ValueChanged) Ereignis begleitet, verfügt über zwei Eigenschaften vom Typ `double` : [`OldValue`](xref:Xamarin.Forms.ValueChangedEventArgs.OldValue) und [`NewValue`](xref:Xamarin.Forms.ValueChangedEventArgs.NewValue) . Wenn das-Ereignis ausgelöst wird, ist der Wert von mit `NewValue` der- [`Value`](xref:Xamarin.Forms.Stepper.Value) Eigenschaft des-Objekts identisch [`Stepper`](xref:Xamarin.Forms.Stepper) .

## <a name="basic-stepper-code-and-markup"></a>Grundlegender Stepper-Code und Markup

Das Beispiel " [**stepperdemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-stepperdemos) " enthält drei Seiten, die funktionell identisch sind, jedoch auf unterschiedliche Weise implementiert werden. Die erste Seite verwendet nur c#-Code, die zweite verwendet XAML mit einem Ereignishandler im Code, und dritte kann den Ereignishandler mithilfe der Datenbindung in der XAML-Datei vermeiden.

### <a name="creating-a-stepper-in-code"></a>Erstellen eines Stepper in Code

Die **grundlegende Stepper-Codepage** im Beispiel [**stepperdemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-stepperdemos) zeigt, wie ein [`Stepper`](xref:Xamarin.Forms.Stepper) -Objekt und zwei- [`Label`](xref:Xamarin.Forms.Label) Objekte im Code erstellt werden:

```csharp
public class BasicStepperCodePage : ContentPage
{
    public BasicStepperCodePage()
    {
        Label rotationLabel = new Label
        {
            Text = "ROTATING TEXT",
            FontSize = Device.GetNamedSize(NamedSize.Large, typeof(Label)),
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.CenterAndExpand
        };

        Label displayLabel = new Label
        {
            Text = "(uninitialized)",
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.CenterAndExpand
        };

        Stepper stepper = new Stepper
        {
            Maximum = 360,
            Increment = 30,
            HorizontalOptions = LayoutOptions.Center
        };
        stepper.ValueChanged += (sender, e) =>
        {
            rotationLabel.Rotation = stepper.Value;
            displayLabel.Text = string.Format("The Stepper value is {0}", e.NewValue);
        };

        Title = "Basic Stepper Code";
        Content = new StackLayout
        {
            Margin = new Thickness(20),
            Children = { rotationLabel, stepper, displayLabel }
        };
    }
}
```

Die [`Stepper`](xref:Xamarin.Forms.Stepper) wird mit der [`Maximum`](xref:Xamarin.Forms.Stepper.Maximum) -Eigenschaft 360 und einer- [`Increment`](xref:Xamarin.Forms.Stepper.Increment) Eigenschaft von 30 initialisiert. Durch die Bearbeitung `Stepper` von ändert sich der ausgewählte Wert inkrementell zwischen [`Minimum`](xref:Xamarin.Forms.Stepper.Minimum) und `Maximum` basierend auf dem Wert der- `Increment` Eigenschaft. Der- [`ValueChanged`](xref:Xamarin.Forms.Stepper.ValueChanged) Handler des `Stepper` [`Value`](xref:Xamarin.Forms.Stepper.Value) -Objekts verwendet die-Eigenschaft des `stepper` -Objekts, um die [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation) -Eigenschaft des ersten festzulegen, [`Label`](xref:Xamarin.Forms.Label) und verwendet die- `string.Format` Methode mit der- `NewValue` Eigenschaft der Ereignis Argumente, um die- [`Text`](xref:Xamarin.Forms.Label.Text) Eigenschaft der zweiten festzulegen `Label` . Diese beiden Ansätze zum Abrufen des aktuellen Werts von `Stepper` sind austauschbar.

Die folgenden Screenshots zeigen die **grundlegende Stepper-Codepage** :

[![Grundlegender Stepper-Code](stepper-images/basic-stepper-code.png "Grundlegender Stepper-Code")](stepper-images/basic-stepper-code-large.png#lightbox)

Die zweite [`Label`](xref:Xamarin.Forms.Label) zeigt den Text "(nicht initialisiert)" [`Stepper`](xref:Xamarin.Forms.Stepper) an, bis der manipuliert wird, was bewirkt, dass das erste [`ValueChanged`](xref:Xamarin.Forms.Stepper.ValueChanged) Ereignis ausgelöst wird.

### <a name="creating-a-stepper-in-xaml"></a>Erstellen eines Stepper in XAML

Die **XAML-Basis** Seite des Standard-Stepper ist funktional identisch mit dem **einfachen Stepper-Code** , wird jedoch größtenteils in XAML implementiert:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="StepperDemo.BasicStepperXAMLPage"
             Title="Basic Stepper XAML">
    <StackLayout Margin="20">
        <Label x:Name="_rotatingLabel"
               Text="ROTATING TEXT"
               FontSize="Large"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />
        <Stepper Maximum="360"
                 Increment="30"
                 HorizontalOptions="Center"
                 ValueChanged="OnStepperValueChanged" />
        <Label x:Name="_displayLabel"
               Text="(uninitialized)"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />        
    </StackLayout>
</ContentPage>
```

Die Code-Behind-Datei enthält den Handler für das- [`ValueChanged`](xref:Xamarin.Forms.Stepper.ValueChanged) Ereignis:

```csharp
public partial class BasicStepperXAMLPage : ContentPage
{
    public BasicStepperXAMLPage()
    {
        InitializeComponent();
    }

    void OnStepperValueChanged(object sender, ValueChangedEventArgs e)
    {
        double value = e.NewValue;
        _rotatingLabel.Rotation = value;
        _displayLabel.Text = string.Format("The Stepper value is {0}", value);
    }
}
```

Der Ereignishandler kann auch den abrufen [`Stepper`](xref:Xamarin.Forms.Stepper) , der das Ereignis durch das- `sender` Argument auslöst. Die- [`Value`](xref:Xamarin.Forms.Stepper.Value) Eigenschaft enthält den aktuellen Wert:

```csharp
double value = ((Stepper)sender).Value;
```

Wenn dem [`Stepper`](xref:Xamarin.Forms.Stepper) Objekt ein Name in der XAML-Datei mit einem- `x:Name` Attribut (z. b. "Stepper") zugewiesen wurde, könnte der Ereignishandler direkt auf dieses Objekt verweisen:

```csharp
double value = stepper.Value;
```

### <a name="data-binding-the-stepper"></a>Datenbindung für den Stepper

Auf der Seite **grundlegende Stepper-Bindungen** wird gezeigt, wie eine nahezu äquivalente Anwendung geschrieben wird, die den [`Value`](xref:Xamarin.Forms.Stepper.Value) Ereignishandler mithilfe der [Datenbindung](~/xamarin-forms/app-fundamentals/data-binding/index.md)ausschließt:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="StepperDemo.BasicStepperBindingsPage"
             Title="Basic Stepper Bindings">
    <StackLayout Margin="20">
        <Label Text="ROTATING TEXT"
               Rotation="{Binding Source={x:Reference _stepper}, Path=Value}"
               FontSize="Large"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />
        <Stepper x:Name="_stepper"
                 Maximum="360"
                 Increment="30"
                 HorizontalOptions="Center" />
        <Label Text="{Binding Source={x:Reference _stepper}, Path=Value, StringFormat='The Stepper value is {0:F0}'}"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />
    </StackLayout>
</ContentPage>
```

Die- [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation) Eigenschaft der ersten [`Label`](xref:Xamarin.Forms.Label) ist an die- [`Value`](xref:Xamarin.Forms.Stepper.Value) Eigenschaft von gebunden [`Stepper`](xref:Xamarin.Forms.Stepper) , wie die- [`Text`](xref:Xamarin.Forms.Label.Text) Eigenschaft der zweiten `Label` mit einer- `StringFormat` Spezifikation. Die Seite " **grundlegende Stepper-Bindungen** " funktioniert ein wenig anders als die beiden vorherigen Seiten: Wenn die Seite zum ersten Mal angezeigt wird, zeigt die zweite `Label` die Text Zeichenfolge mit dem Wert an. Dies hat den Vorteil, dass die Datenbindung verwendet wird. Um Text ohne Datenbindung anzuzeigen, müssen Sie die- `Text` Eigenschaft des-Objekts explizit initialisieren `Label` oder ein Auslösen des-Ereignisses simulieren, [`ValueChanged`](xref:Xamarin.Forms.Stepper.ValueChanged) indem Sie den-Ereignishandler aus dem-Klassenkonstruktor aufrufen.

## <a name="precautions"></a>Treffen

Der Wert der- [`Minimum`](xref:Xamarin.Forms.Stepper.Minimum) Eigenschaft muss stets kleiner sein als der Wert der- [`Maximum`](xref:Xamarin.Forms.Stepper.Maximum) Eigenschaft. Der folgende Code Ausschnitt bewirkt, dass [`Stepper`](xref:Xamarin.Forms.Stepper) eine Ausnahme auslöst:

```csharp
// Throws an exception!
Stepper stepper = new Stepper
{
    Minimum = 180,
    Maximum = 360
};
```

Der c#-Compiler generiert Code, mit dem diese beiden Eigenschaften nacheinander festgelegt werden. wenn die- [`Minimum`](xref:Xamarin.Forms.Stepper.Minimum) Eigenschaft auf 180 festgelegt ist, ist sie größer als der Standard [`Maximum`](xref:Xamarin.Forms.Stepper.Maximum) Wert 100. In diesem Fall können Sie die Ausnahme vermeiden, indem Sie `Maximum` zuerst die-Eigenschaft festlegen:

```csharp
Stepper stepper = new Stepper
{
    Maximum = 360,
    Minimum = 180
};
```

[`Maximum`](xref:Xamarin.Forms.Stepper.Maximum)Die Einstellung auf 360 ist kein Problem, da Sie größer als der Standard [`Minimum`](xref:Xamarin.Forms.Stepper.Minimum) Wert 0 ist. Wenn `Minimum` festgelegt ist, ist der Wert kleiner als der `Maximum` Wert von 360.

Das gleiche Problem ist in XAML aufgetreten. Legen Sie die Eigenschaften in einer Reihenfolge fest, die sicherstellt, dass [`Maximum`](xref:Xamarin.Forms.Stepper.Maximum) immer größer ist als `Minimum` :

```xaml
<Stepper Maximum="360"
         Minimum="180" ... />
```

Sie können den [`Minimum`](xref:Xamarin.Forms.Stepper.Minimum) -Wert und den- [`Maximum`](xref:Xamarin.Forms.Stepper.Maximum) Wert auf negative Zahlen festlegen, aber nur in einer Reihenfolge, in der `Minimum` immer kleiner ist als `Maximum` :

```xaml
<Stepper Minimum="-360"
         Maximum="-180" ... />
```

Die [`Value`](xref:Xamarin.Forms.Stepper.Value) -Eigenschaft ist immer größer oder gleich dem [`Minimum`](xref:Xamarin.Forms.Stepper.Minimum) -Wert und kleiner als oder gleich [`Maximum`](xref:Xamarin.Forms.Stepper.Maximum) . Wenn `Value` auf einen Wert außerhalb dieses Bereichs festgelegt wird, wird der Wert in den Bereich umgewandelt, aber es wird keine Ausnahme ausgelöst. Beispielsweise wird mit diesem Code *keine* Ausnahme ausgelöst:

```csharp
Stepper stepper = new Stepper
{
    Value = 180
};
```

Stattdessen wird die- [`Value`](xref:Xamarin.Forms.Stepper.Value) Eigenschaft in den [`Maximum`](xref:Xamarin.Forms.Stepper.Maximum) Wert 100 umgewandelt.

Hier sehen Sie einen oben gezeigten Code Ausschnitt:

```csharp
Stepper stepper = new Stepper
{
    Maximum = 360,
    Minimum = 180
};
```

Wenn [`Minimum`](xref:Xamarin.Forms.Stepper.Minimum) auf 180 festgelegt ist, [`Value`](xref:Xamarin.Forms.Stepper.Value) wird auch auf 180 festgelegt.

Wenn ein- [`ValueChanged`](xref:Xamarin.Forms.Stepper.ValueChanged) Ereignishandler zu dem Zeitpunkt angefügt wurde, an dem die- [`Value`](xref:Xamarin.Forms.Stepper.Value) Eigenschaft in einen anderen als den Standardwert 0 (null) umgewandelt wird, wird ein- `ValueChanged` Ereignis ausgelöst. Hier ist ein Code Ausschnitt von XAML:

```xaml
<Stepper ValueChanged="OnStepperValueChanged"
         Maximum="360"
         Minimum="180" />
```

Wenn [`Minimum`](xref:Xamarin.Forms.Stepper.Minimum) auf 180 festgelegt ist, [`Value`](xref:Xamarin.Forms.Stepper.Value) wird auch auf 180 festgelegt, und das- [`ValueChanged`](xref:Xamarin.Forms.Stepper.ValueChanged) Ereignis wird ausgelöst. Dies kann vorkommen, bevor der Rest der Seite erstellt wurde, und der Handler versucht möglicherweise, auf andere Elemente auf der Seite zu verweisen, die noch nicht erstellt wurden. Möglicherweise möchten Sie dem Handler Code hinzufügen `ValueChanged` , der `null` auf Werte anderer Elemente auf der Seite prüft. Oder Sie können den `ValueChanged` Ereignishandler festlegen, nachdem die [`Stepper`](xref:Xamarin.Forms.Stepper) Werte initialisiert wurden.

## <a name="related-links"></a>Verwandte Links

- [Beispiel für Stepper-Demos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-stepperdemos)
- [Stepper-API](xref:Xamarin.Forms.Stepper)
