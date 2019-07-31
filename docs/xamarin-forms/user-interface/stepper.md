---
title: Xamarin.Forms zugeordnetem
description: Die Xamarin.Forms-zugeordnetem können Benutzer einen numerischen Wert aus einem Bereich von Werten auswählen. Es besteht aus zwei Schaltflächen, die mit der Bezeichnung Minuszeichen und Pluszeichen. Bearbeiten die beiden Schaltflächen ändert den ausgewählten Wert inkrementell.
ms.prod: xamarin
ms.assetid: 62571B3E-D84B-4F52-9FC7-C105D6733B16
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/17/2018
ms.openlocfilehash: 6c89f04b1d1d87fed8d86d50cb68527391a7f317
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68656202"
---
# <a name="xamarinforms-stepper"></a>Xamarin.Forms zugeordnetem

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-stepperdemos)

_Verwenden Sie eine zugeordnetem für einen numerischen Wert aus einen Bereich von Werten auswählen._

Die Xamarin.Forms [ `Stepper` ](xref:Xamarin.Forms.Stepper) besteht aus zwei Schaltflächen, die mit der Bezeichnung Minuszeichen und Pluszeichen. Diese Schaltflächen können bearbeitet werden, indem der Benutzer inkrementell Auswählen einer `double` Wert aus einem Wertebereich.

Die [ `Stepper` ](xref:Xamarin.Forms.Stepper) definiert vier Eigenschaften vom Typ `double`:

- [`Increment`](xref:Xamarin.Forms.Stepper.Increment) ist der Betrag, um den ausgewählten Wert durch, mit einem Standardwert von 1 zu ändern.
- [`Minimum`](xref:Xamarin.Forms.Stepper.Minimum) ist der Mindestwert des Bereichs, hat den Standardwert 0.
- [`Maximum`](xref:Xamarin.Forms.Stepper.Maximum) ist der Höchstwert des Bereichs, hat den Standardwert von 100.
- [`Value`](xref:Xamarin.Forms.Stepper.Value) die zugeordnetem Wert, der zwischen liegen kann `Minimum` und `Maximum` und hat den Standardwert 0.

Alle diese Eigenschaften verfügen über [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) Objekte. Die [ `Value` ](xref:Xamarin.Forms.Stepper.Value) Eigenschaft verfügt über einen Standardmodus für die Bindung der [ `BindingMode.TwoWay` ](xref:Xamarin.Forms.BindingMode.TwoWay), was bedeutet, dass es als Bindungsquelle in einer Anwendung geeignet ist, verwendet der [ Model-View-ViewModel (MVVM)](~/xamarin-forms/enterprise-application-patterns/mvvm.md) Architektur.

> [!WARNING]
> Intern wird die [ `Stepper` ](xref:Xamarin.Forms.Stepper) wird sichergestellt, dass [ `Minimum` ](xref:Xamarin.Forms.Stepper.Minimum) ist kleiner als [ `Maximum` ](xref:Xamarin.Forms.Stepper.Maximum). Wenn `Minimum` oder `Maximum` einmal festgelegt werden, damit `Minimum` ist nicht kleiner als `Maximum`, wird eine Ausnahme ausgelöst. Weitere Informationen zum Festlegen der `Minimum` und `Maximum` Eigenschaften finden Sie [Vorsichtsmaßnahmen](#precautions) Abschnitt.

Die [ `Stepper` ](xref:Xamarin.Forms.Stepper) wandelt die [ `Value` ](xref:Xamarin.Forms.Stepper.Value) Eigenschaft, damit es zwischen ist [ `Minimum` ](xref:Xamarin.Forms.Stepper.Minimum) und [ `Maximum` ](xref:Xamarin.Forms.Stepper.Maximum)(inklusiv) enthalten. Wenn die `Minimum` -Eigenschaftensatz auf einen Wert größer als die `Value` -Eigenschaft, die `Stepper` legt diese fest der `Value` Eigenschaft `Minimum`. Auf ähnliche Weise Wenn `Maximum` auf einen Wert festgelegt ist kleiner als `Value`, klicken Sie dann `Stepper` legt die `Value` Eigenschaft `Maximum`.

[`Stepper`](xref:Xamarin.Forms.Stepper) definiert eine [ `ValueChanged` ](xref:Xamarin.Forms.Stepper.ValueChanged) Ereignis, wenn ausgelöst wird, die [ `Value` ](xref:Xamarin.Forms.Stepper.Value) ändert, entweder durch Bearbeitung der durch den Benutzer der `Stepper` oder wenn die Anwendung festlegt der `Value` direkt-Eigenschaft. Ein `ValueChanged` Ereignis wird auch ausgelöst, wenn die `Value` Eigenschaft wird erzwungen, wie im vorherigen Absatz beschrieben.

Die [ `ValueChangedEventArgs` ](xref:Xamarin.Forms.ValueChangedEventArgs) Objekt an, die mit der [ `ValueChanged` ](xref:Xamarin.Forms.Stepper.ValueChanged) Ereignis verfügt über zwei Eigenschaften, die beide vom Typ `double`: [ `OldValue` ](xref:Xamarin.Forms.ValueChangedEventArgs.OldValue) und [ `NewValue`](xref:Xamarin.Forms.ValueChangedEventArgs.NewValue). Zum Zeitpunkt das Ereignis ausgelöst wird, den Wert der `NewValue` ist identisch mit der [ `Value` ](xref:Xamarin.Forms.Stepper.Value) Eigenschaft der [ `Stepper` ](xref:Xamarin.Forms.Stepper) Objekt.

## <a name="basic-stepper-code-and-markup"></a>Grundlegende zugeordnetem Code und markup

Die [ **StepperDemos** ](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-stepperdemos) Beispiel enthält drei Seiten, die funktionell identisch sind, aber auf unterschiedliche Weise implementiert werden. Die erste Seite wird nur verwendet, C# Code, die zweite verwendet XAML mit einem Ereignishandler im Code, und die dritte den Ereignishandler zu vermeiden, indem Sie die Datenbindung in der XAML-Datei kann.

### <a name="creating-a-stepper-in-code"></a>Erstellen eine zugeordnetem in code

Die **grundlegenden zugeordnetem Code** auf der Seite die [ **StepperDemos** ](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-stepperdemos) Beispiel zeigt, wie zum Erstellen einer [ `Stepper` ](xref:Xamarin.Forms.Stepper) und zwei [ `Label` ](xref:Xamarin.Forms.Label) -Objekte im Code:

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

Die [ `Stepper` ](xref:Xamarin.Forms.Stepper) initialisiert wird, dass eine [ `Maximum` ](xref:Xamarin.Forms.Stepper.Maximum) Eigenschaft 360, und ein [ `Increment` ](xref:Xamarin.Forms.Stepper.Increment) Eigenschaft von 30. Bearbeiten der `Stepper` ändert sich den ausgewählten Wert inkrementell zwischen [ `Minimum` ](xref:Xamarin.Forms.Stepper.Minimum) zu `Maximum` basierend auf den Wert des der `Increment` Eigenschaft. Die [ `ValueChanged` ](xref:Xamarin.Forms.Stepper.ValueChanged) Handler die `Stepper` verwendet die [ `Value` ](xref:Xamarin.Forms.Stepper.Value) Eigenschaft der `stepper` Objekt zum Festlegen von der [ `Rotation` ](xref:Xamarin.Forms.VisualElement.Rotation) -Eigenschaft der ersten [ `Label` ](xref:Xamarin.Forms.Label) und verwendet die `string.Format` -Methode mit der `NewValue` Eigenschaft des Ereignisarguments festgelegt die [ `Text` ](xref:Xamarin.Forms.Label.Text) Eigenschaft der zweite `Label`. Diese beiden Ansätze zum Abrufen des aktuellen Werts von der `Stepper` sind austauschbar.

Die folgenden Screenshots zeigen die **grundlegenden zugeordnetem Code** Seite:

[![Grundlegende zugeordnetem Code](stepper-images/basic-stepper-code.png "grundlegende zugeordnetem Code")](stepper-images/basic-stepper-code-large.png#lightbox)

Die zweite [ `Label` ](xref:Xamarin.Forms.Label) zeigt den Text "(nicht initialisierte)", bis die [ `Stepper` ](xref:Xamarin.Forms.Stepper) bearbeitet wird, der bewirkt, dass der erstes [ `ValueChanged` ](xref:Xamarin.Forms.Stepper.ValueChanged) Ereignis ausgelöst werden.

### <a name="creating-a-stepper-in-xaml"></a>Erstellen eine zugeordnetem in XAML

Die **grundlegende zugeordnetem XAML** Seite ist funktionell identisch mit **grundlegenden zugeordnetem Code** jedoch meist in XAML implementiert:

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

Die Code-Behind-Datei enthält den Handler für die [ `ValueChanged` ](xref:Xamarin.Forms.Stepper.ValueChanged) Ereignis:

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

Es ist auch möglich, für den Ereignishandler zum Abrufen der [ `Stepper` ](xref:Xamarin.Forms.Stepper) ist, die das Ereignis durch Auslösen der `sender` Argument. Die [ `Value` ](xref:Xamarin.Forms.Stepper.Value) Eigenschaft enthält den aktuellen Wert:

```csharp
double value = ((Stepper)sender).Value;
```

Wenn die [ `Stepper` ](xref:Xamarin.Forms.Stepper) Objekt die erhaltenen eines Namens in der XAML-Datei mit einer `x:Name` -Attribut (z. B. "zugeordnetem"), und klicken Sie dann auf den Ereignishandler kann dieses Objekt direkt verweisen:

```csharp
double value = stepper.Value;
```

### <a name="data-binding-the-stepper"></a>Die Datenbindung der zugeordnetem

Die **Standardbindungen zugeordnetem** Seite zeigt, wie Sie eine nahezu gleichwertig Anwendung schreiben, bei denen keine das [ `Value` ](xref:Xamarin.Forms.Stepper.Value) -Ereignishandler mit [Datenbindung](~/xamarin-forms/app-fundamentals/data-binding/index.md):

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

Die [ `Rotation` ](xref:Xamarin.Forms.VisualElement.Rotation) Eigenschaft des ersten [ `Label` ](xref:Xamarin.Forms.Label) gebunden ist die [ `Value` ](xref:Xamarin.Forms.Stepper.Value) Eigenschaft der [ `Stepper` ](xref:Xamarin.Forms.Stepper), da die [ `Text` ](xref:Xamarin.Forms.Label.Text) -Eigenschaft der zweiten `Label` mit einem `StringFormat` Spezifikation. Die Seite " **grundlegende Stepper-Bindungen** " funktioniert ein wenig anders als die beiden vorherigen Seiten: Wenn die Seite zum ersten Mal angezeigt wird `Label` , zeigt die zweite die Text Zeichenfolge mit dem Wert an. Dies ist ein Vorteil der Verwendung der Datenbindung. Um Text ohne Datenbindung anzuzeigen, müssen Sie explizit Initialisieren der `Text` Eigenschaft der `Label` oder simulieren Sie eine Auslösung des der [ `ValueChanged` ](xref:Xamarin.Forms.Stepper.ValueChanged) Ereignis durch Aufrufen des ereignishandlers aus dem Klassenkonstruktor .

## <a name="precautions"></a>Vorsichtsmaßnahmen bei der

Der Wert des der [ `Minimum` ](xref:Xamarin.Forms.Stepper.Minimum) Eigenschaft muss immer kleiner als der Wert, der die [ `Maximum` ](xref:Xamarin.Forms.Stepper.Maximum) Eigenschaft. Im folgenden Codebeispiel Codeausschnitt bewirkt, dass die [ `Stepper` ](xref:Xamarin.Forms.Stepper) zum Auslösen einer Ausnahme:

```csharp
// Throws an exception!
Stepper stepper = new Stepper
{
    Minimum = 180,
    Maximum = 360
};
```

Die C# Compiler generiert Code, der diese beiden Eigenschaften in der Sequenz ist, legt diese fest und wann die [ `Minimum` ](xref:Xamarin.Forms.Stepper.Minimum) -Eigenschaftensatz auf 180, er ist größer als die standardmäßige [ `Maximum` ](xref:Xamarin.Forms.Stepper.Maximum) Wert von 100. Sie können die Ausnahme in diesem Fall vermeiden, durch Festlegen der `Maximum` Eigenschaft erste:

```csharp
Stepper stepper = new Stepper
{
    Maximum = 360,
    Minimum = 180
};
```

Festlegen von [ `Maximum` ](xref:Xamarin.Forms.Stepper.Maximum) auf 360 ist kein Problem, da sie größer als der Standardwert ist [ `Minimum` ](xref:Xamarin.Forms.Stepper.Minimum) Wert 0. Wenn `Minimum` festgelegt ist, wird der Wert ist kleiner als der `Maximum` Wert auf 360.

Das gleiche Problem tritt in XAML. Legen Sie die Eigenschaften in der Reihenfolge, die wird, dass sichergestellt [ `Maximum` ](xref:Xamarin.Forms.Stepper.Maximum) ist immer größer als `Minimum`:

```xaml
<Stepper Maximum="360"
         Minimum="180" ... />
```

Sie können festlegen, die [ `Minimum` ](xref:Xamarin.Forms.Stepper.Minimum) und [ `Maximum` ](xref:Xamarin.Forms.Stepper.Maximum) Werte in negative Zahlen, aber nur in der Reihenfolge, in denen `Minimum` ist immer kleiner als `Maximum`:

```xaml
<Stepper Minimum="-360"
         Maximum="-180" ... />
```

Die [ `Value` ](xref:Xamarin.Forms.Stepper.Value) Eigenschaft ist immer größer als oder gleich der [ `Minimum` ](xref:Xamarin.Forms.Stepper.Minimum) Wert und kleiner als oder gleich [ `Maximum` ](xref:Xamarin.Forms.Stepper.Maximum). Wenn `Value` festgelegt ist auf einen Wert außerhalb dieses Bereichs liegt, wird der Wert im Bereich liegen zwingend werden, jedoch wird keine Ausnahme ausgelöst. Dieser Code beispielsweise wird *nicht* eine Ausnahme ausgelöst:

```csharp
Stepper stepper = new Stepper
{
    Value = 180
};
```

Stattdessen die [ `Value` ](xref:Xamarin.Forms.Stepper.Value) Eigenschaft wird erzwungen, um die [ `Maximum` ](xref:Xamarin.Forms.Stepper.Maximum) Wert von 100.

Hier ist eine der oben aufgeführten Codeausschnitt aus:

```csharp
Stepper stepper = new Stepper
{
    Maximum = 360,
    Minimum = 180
};
```

Wenn [ `Minimum` ](xref:Xamarin.Forms.Stepper.Minimum) nastaven NA hodnotu 180 aufweist, klicken Sie dann [ `Value` ](xref:Xamarin.Forms.Stepper.Value) auch auf 180 festgelegt ist.

Wenn eine [ `ValueChanged` ](xref:Xamarin.Forms.Stepper.ValueChanged) Ereignishandler wurde angefügt, die zum Zeitpunkt, der [ `Value` ](xref:Xamarin.Forms.Stepper.Value) Eigenschaft wird in einen anderen Wert als den Standardwert 0 (null) umgewandelt wird eine `ValueChanged` Ereignis wird ausgelöst. Hier ist ein Ausschnitt des XAML aus:

```xaml
<Stepper ValueChanged="OnStepperValueChanged"
         Maximum="360"
         Minimum="180" />
```

Wenn [ `Minimum` ](xref:Xamarin.Forms.Stepper.Minimum) nastaven NA hodnotu 180 aufweist, [ `Value` ](xref:Xamarin.Forms.Stepper.Value) wird ebenfalls auf 180, festgelegt und die [ `ValueChanged` ](xref:Xamarin.Forms.Stepper.ValueChanged) Ereignis wird ausgelöst. Dies kann auftreten, bevor der Rest der Seite erstellt wurden wurde und der Handler könnte versuchen, auf andere Elemente auf der Seite verweisen, die noch nicht erstellt wurden. Möglicherweise möchten Sie Code zum Hinzufügen der `ValueChanged` Handler, der prüft, ob `null` Werte der anderen Elemente auf der Seite. Oder Sie können festlegen, die `ValueChanged` Ereignishandler nach der [ `Stepper` ](xref:Xamarin.Forms.Stepper) Werte initialisiert wurden.

## <a name="related-links"></a>Verwandte Links

- [Zugeordnetem Demos-Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-stepperdemos)
- [Zugeordnetem API](xref:Xamarin.Forms.Stepper)
