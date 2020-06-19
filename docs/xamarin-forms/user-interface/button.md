---
title: Xamarin.FormsGedrückt
description: Die Schaltfläche antwortet auf eine Tap-oder Klick-Schaltfläche, die eine Anwendung anweist, eine bestimmte Aufgabe auszuführen.
ms.prod: xamarin
ms.assetid: 62CAEB63-0800-44F4-9B8C-EE632138C2F5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/04/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 595367d94bcc4ac384763e915a0a19db7517341d
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84573390"
---
# <a name="xamarinforms-button"></a>Xamarin.FormsGedrückt

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-buttondemos)

_Die Schaltfläche antwortet auf eine Tap-oder Klick-Schaltfläche, die eine Anwendung anweist, eine bestimmte Aufgabe auszuführen._

Das [`Button`](xref:Xamarin.Forms.Button) ist das grundlegendste interaktive Steuerelement in allen Xamarin.Forms . `Button`In wird normalerweise eine kurze Text Zeichenfolge angezeigt, die einen Befehl anzeigt, aber es kann auch ein Bitmap-Bild oder eine Kombination aus Text und einem Bild angezeigt werden. Der Benutzer drückt den `Button` mit einem Finger oder klickt mit der Maus darauf, um diesen Befehl zu initiieren.

Die meisten der unten erläuterten Themen entsprechen den Seiten im [**buttondemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-buttondemos) -Beispiel.

## <a name="handling-button-clicks"></a>Behandeln von Schaltflächen Klicks

`Button`definiert ein- [`Clicked`](xref:Xamarin.Forms.Button.Clicked) Ereignis, das ausgelöst wird, wenn der Benutzer `Button` mit einem Finger oder Mauszeiger auf das tippt. Das-Ereignis wird ausgelöst, wenn der Finger oder die Maustaste von der-Oberfläche der losgelassen wird `Button` . Die- `Button` Eigenschaft muss [`IsEnabled`](xref:Xamarin.Forms.VisualElement.IsEnabled) für die auf tippen festgelegt sein, damit Sie auf Abzweigungen `true` antwortet.

Die **Schaltfläche "einfache Schaltfläche** " im [**buttondemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-buttondemos) -Beispiel veranschaulicht, wie ein `Button` in XAML instanziiert und das zugehörige-Ereignis behandelt wird `Clicked` . Die **basicbuttonclickpage. XAML** -Datei enthält eine `StackLayout` mit `Label` und einem `Button` :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="ButtonDemos.BasicButtonClickPage"
             Title="Basic Button Click">
    <StackLayout>

        <Label x:Name="label"
               Text="Click the Button below"
               FontSize="Large"
               VerticalOptions="CenterAndExpand"
               HorizontalOptions="Center" />

        <Button Text="Click to Rotate Text!"
                VerticalOptions="CenterAndExpand"
                HorizontalOptions="Center"
                Clicked="OnButtonClicked" />

    </StackLayout>
</ContentPage>
```

Der `Button` neigt den gesamten Platz, der für ihn zulässig ist. Wenn Sie z. b. die- `HorizontalOptions` Eigenschaft von nicht `Button` auf einen anderen Wert als festlegen `Fill` , nimmt die `Button` vollständige Breite des übergeordneten Elements an.

Standardmäßig ist der `Button` rechteckig, aber Sie können ihm mithilfe der-Eigenschaft abgerundete Ecken übergeben [`CornerRadius`](xref:Xamarin.Forms.Button.CornerRadius) , wie unten im Abschnitt Darstellung der [**Schaltfläche**](#button-appearance)beschrieben.

Die- [`Text`](xref:Xamarin.Forms.Button.Text) Eigenschaft gibt den Text an, der in der angezeigt wird `Button` . Das- [`Clicked`](xref:Xamarin.Forms.Button.Clicked) Ereignis wird auf einen Ereignishandler mit dem Namen festgelegt `OnButtonClicked` . Dieser Handler befindet sich in der Code-Behind-Datei **BasicButtonClickPage.XAML.cs**:

```csharp
public partial class BasicButtonClickPage : ContentPage
{
    public BasicButtonClickPage ()
    {
        InitializeComponent ();
    }

    async void OnButtonClicked(object sender, EventArgs args)
    {
        await label.RelRotateTo(360, 1000);
    }
}
```

Wenn die `Button` angetippt wird, wird die `OnButtonClicked`-Methode ausgeführt. Das- `sender` Argument ist das `Button` für dieses Ereignis verantwortliche Objekt. Sie können dies verwenden, um auf das `Button` Objekt zuzugreifen, oder um zwischen mehreren Objekten zu unterscheiden, `Button` die dasselbe `Clicked` Ereignis verwenden.

Dieser bestimmte `Clicked` Handler Ruft eine Animations Funktion auf, die die `Label` 360 Grad in 1000 Millisekunden rotiert. Hier ist das Programm, das auf IOS-und Android-Geräten ausgeführt wird, und als universelle Windows-Plattform (UWP)-Anwendung auf dem Windows 10-Desktop:

[![Einfache Schaltflächen-Click](button-images/BasicButtonClick.png "Einfache Schaltflächen-Click")](button-images/BasicButtonClick-Large.png#lightbox "Einfache Schaltflächen-Click")

Beachten Sie, dass die- `OnButtonClicked` Methode den- `async` Modifizierer enthält, da `await` im-Ereignishandler verwendet wird. Ein- `Clicked` Ereignishandler erfordert den- `async` Modifizierer nur, wenn der Text des Handlers verwendet `await` .

Jede Plattform rendert den `Button` auf eigene Weise. Im Abschnitt " [**Schalt**](#button-appearance) Flächen Darstellung" erfahren Sie, wie Farben festgelegt und der Rahmen `Button` für stärker angepasste Auftritte sichtbar gemacht wird. `Button`implementiert die- [`IFontElement`](xref:Xamarin.Forms.Internals.IFontElement) Schnittstelle, sodass Sie die [`FontFamily`](xref:Xamarin.Forms.Button.FontFamily) [`FontSize`](xref:Xamarin.Forms.Button.FontSize) Eigenschaften, und enthält [`FontAttributes`](xref:Xamarin.Forms.Button.FontAttributes) .

## <a name="creating-a-button-in-code"></a>Erstellen einer Schaltfläche im Code

Es ist üblich, eine `Button` in XAML zu instanziieren, aber Sie können auch eine `Button` im Code erstellen. Dies kann praktisch sein, wenn Ihre Anwendung mehrere Schaltflächen auf der Grundlage von Daten erstellen muss, die mit einer-Schleife aufgelistet werden können `foreach` .

Die **Schaltfläche für die Code Schaltfläche** zeigt, wie Sie eine Seite erstellen, die der **einfachen Schalt** Flächen-Klick Seite, aber vollständig in c# entspricht:

```csharp
public class CodeButtonClickPage : ContentPage
{
    public CodeButtonClickPage ()
    {
        Title = "Code Button Click";

        Label label = new Label
        {
            Text = "Click the Button below",
            FontSize = Device.GetNamedSize(NamedSize.Large, typeof(Label)),
            VerticalOptions = LayoutOptions.CenterAndExpand,
            HorizontalOptions = LayoutOptions.Center
        };

        Button button = new Button
        {
            Text = "Click to Rotate Text!",
            VerticalOptions = LayoutOptions.CenterAndExpand,
            HorizontalOptions = LayoutOptions.Center
        };
        button.Clicked += async (sender, args) => await label.RelRotateTo(360, 1000);

        Content = new StackLayout
        {
            Children =
            {
                label,
                button
            }
        };
    }
}
```

Alles erfolgt im Konstruktor der Klasse. Da der `Clicked` Handler nur eine Anweisung lang ist, kann er ganz einfach an das Ereignis angefügt werden:

```csharp
button.Clicked += async (sender, args) => await label.RelRotateTo(360, 1000);
```

Natürlich können Sie den Ereignishandler auch als separate Methode definieren (genau wie die- `OnButtonClick` Methode in der **einfachen Schaltfläche**) und diese Methode an das-Ereignis anfügen:

```csharp
button.Clicked += OnButtonClicked;
```

## <a name="disabling-the-button"></a>Deaktivieren der Schaltfläche

Manchmal befindet sich eine Anwendung in einem bestimmten Zustand, in dem ein bestimmter `Button` Klick kein gültiger Vorgang ist. In diesen Fällen sollte der deaktiviert werden, indem die zugehörige- `Button` Eigenschaft auf festgelegt wird `IsEnabled` `false` . Beim klassischen Beispiel handelt es sich um ein `Entry` Steuerelement für einen Dateinamen, der von einer Datei geöffnet wird `Button` : die `Button` sollte nur aktiviert werden, wenn ein Text in das eingegeben wurde `Entry` .
Sie können eine `DataTrigger` für diese Aufgabe verwenden, wie im Artikel [**Daten Trigger**](~/xamarin-forms/app-fundamentals/triggers.md#data-triggers) gezeigt.

## <a name="using-the-command-interface"></a>Verwenden der Befehlsschnittstelle

Es ist möglich, dass eine Anwendung auf TAPS reagiert, `Button` ohne das `Clicked` Ereignis zu behandeln. `Button`Implementiert einen alternativen Benachrichtigungs Mechanismus, der als _Befehl_ oder Befehlsschnittstelle bezeichnet wird. _commanding_ Dies umfasst zwei Eigenschaften:

- [`Command`](xref:Xamarin.Forms.Button.Command)vom Typ [`ICommand`](xref:System.Windows.Input.ICommand) , eine im-Namespace definierte-Schnittstelle [`System.Windows.Input`](xref:System.Windows.Input) .
- [`CommandParameter`](xref:Xamarin.Forms.Button.CommandParameter)-Eigenschaft vom Typ [`Object`](xref:System.Object) .

Dieser Ansatz eignet sich besonders für die Verbindung mit der Datenbindung und insbesondere bei der Implementierung der Model-View-ViewModel (MVVM)-Architektur. Diese Themen werden in den Artikeln [Datenbindung](~/xamarin-forms/app-fundamentals/data-binding/index.md), [Daten Bindungen an MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)und [MVVM](~/xamarin-forms/enterprise-application-patterns/mvvm.md)erläutert.

In einer MVVM-Anwendung definiert das ViewModel Eigenschaften vom Typ `ICommand` , die dann mit den XAML- `Button` Elementen mit Daten Bindungen verbunden werden. Xamarin.Formsdefiniert auch [`Command`](xref:Xamarin.Forms.Command) die [`Command<T>`](xref:Xamarin.Forms.Command`1) Klassen und, die die- `ICommand` Schnittstelle implementieren und dem ViewModel beim Definieren von Eigenschaften vom Typ unterstützen `ICommand` .

Der Befehl wird im Artikel über [**die Befehlsschnittstelle**](~/xamarin-forms/app-fundamentals/data-binding/commanding.md) ausführlicher beschrieben, aber auf der **Befehls Seite grundlegende Schalt** Flächen im [**buttondemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-buttondemos) -Beispiel wird der grundlegende Ansatz veranschaulicht.

Die `CommandDemoViewModel` -Klasse ist ein sehr einfaches ViewModel, das eine Eigenschaft vom Typ mit dem `double` Namen und `Number` zwei Eigenschaften vom Typ mit dem `ICommand` Namen `MultiplyBy2Command` und definiert `DivideBy2Command` :

```csharp
class CommandDemoViewModel : INotifyPropertyChanged
{
    double number = 1;

    public event PropertyChangedEventHandler PropertyChanged;

    public CommandDemoViewModel()
    {
        MultiplyBy2Command = new Command(() => Number *= 2);

        DivideBy2Command = new Command(() => Number /= 2);
    }

    public double Number
    {
        set
        {
            if (number != value)
            {
                number = value;
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Number"));
            }
        }
        get
        {
            return number;
        }
    }

    public ICommand MultiplyBy2Command { private set; get; }

    public ICommand DivideBy2Command { private set; get; }
}
```

Die beiden `ICommand` Eigenschaften werden im Konstruktor der Klasse mit zwei Objekten des Typs initialisiert `Command` . Die `Command` Konstruktoren enthalten eine kleine Funktion (als `execute` Konstruktorargument bezeichnet), die die Eigenschaft verdoppelt oder halbiert `Number` .

Mit der Datei **basicbuttoncommand. XAML** wird `BindingContext` auf eine Instanz von festgelegt `CommandDemoViewModel` . Das `Label` -Element und zwei- `Button` Elemente enthalten Bindungen zu den drei Eigenschaften in `CommandDemoViewModel` :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:ButtonDemos"
             x:Class="ButtonDemos.BasicButtonCommandPage"
             Title="Basic Button Command">

    <ContentPage.BindingContext>
        <local:CommandDemoViewModel />
    </ContentPage.BindingContext>

    <StackLayout>
        <Label Text="{Binding Number, StringFormat='Value is now {0}'}"
               FontSize="Large"
               VerticalOptions="CenterAndExpand"
               HorizontalOptions="Center" />

        <Button Text="Multiply by 2"
                VerticalOptions="CenterAndExpand"
                HorizontalOptions="Center"
                Command="{Binding MultiplyBy2Command}" />

        <Button Text="Divide by 2"
                VerticalOptions="CenterAndExpand"
                HorizontalOptions="Center"
                Command="{Binding DivideBy2Command}" />
    </StackLayout>
</ContentPage>
```

Wenn die beiden `Button` Elemente abgetippt werden, werden die Befehle ausgeführt, und der Wert für die Anzahl ändert sich:

[![Grundlegender Schaltflächen Befehl](button-images/BasicButtonCommand.png "Grundlegender Schaltflächen Befehl")](button-images/BasicButtonCommand-Large.png#lightbox)

Der Vorteil dieses Ansatzes gegenüber `Clicked` Handlern besteht darin, dass sich die gesamte Logik, die die Funktionalität dieser Seite beinhaltet, im ViewModel und nicht in der Code Behind-Datei befindet, sodass die Benutzeroberfläche besser von der Geschäftslogik getrennt wird.

Es ist auch möglich, dass die `Command` -Objekte das Aktivieren und Deaktivieren der Elemente steuern `Button` . Nehmen Sie beispielsweise an, Sie möchten den Bereich von Zahlenwerten zwischen 2<sup>10</sup> und 2<sup> &ndash; 10</sup>begrenzen. Sie können dem Konstruktor (als Argument bezeichnet) eine weitere Funktion hinzufügen `canExecute` , die zurückgibt, `true` Wenn das `Button` aktiviert werden soll. Hier ist die Änderung am `CommandDemoViewModel` Konstruktor:

```csharp
class CommandDemoViewModel : INotifyPropertyChanged
{
    ···
    public CommandDemoViewModel()
    {
        MultiplyBy2Command = new Command(
            execute: () =>
            {
                Number *= 2;
                ((Command)MultiplyBy2Command).ChangeCanExecute();
                ((Command)DivideBy2Command).ChangeCanExecute();
            },
            canExecute: () => Number < Math.Pow(2, 10));

        DivideBy2Command = new Command(
            execute: () =>
            {
                Number /= 2;
                ((Command)MultiplyBy2Command).ChangeCanExecute();
                ((Command)DivideBy2Command).ChangeCanExecute();
            },
            canExecute: () => Number > Math.Pow(2, -10));
    }
    ···
}
```

Die Aufrufe der- `ChangeCanExecute` Methode von `Command` sind erforderlich, damit die `Command` -Methode die `canExecute` -Methode aufrufen und ermitteln kann, ob die `Button` deaktiviert werden soll oder nicht. Bei dieser Codeänderung ist die Zahl deaktiviert, da die Zahl den Grenzwert erreicht `Button` :

[![Grundlegende Schaltflächen Befehle-geändert](button-images/BasicButtonCommandModified.png "Grundlegende Schaltflächen Befehle-geändert")](button-images/BasicButtonCommandModified-Large.png#lightbox)

Es ist möglich, dass zwei oder mehr `Button` Elemente an dieselbe Eigenschaft gebunden werden `ICommand` . Die `Button` Elemente können mithilfe der-Eigenschaft von unterschieden werden [`CommandParameter`](xref:Xamarin.Forms.Button.CommandParameter) `Button` . In diesem Fall möchten Sie die generische [`Command<T>`](xref:Xamarin.Forms.Command`1) Klasse verwenden. Das `CommandParameter` -Objekt wird dann als Argument an die `execute` -Methode und die-Methode weitergegeben `canExecute` . Dieses Verfahren wird im Artikel " [**grundlegende**](~/xamarin-forms/app-fundamentals/data-binding/commanding.md#basic-commanding) Befehls [**Schnittstelle**](~/xamarin-forms/app-fundamentals/data-binding/commanding.md#basic-commanding) " ausführlich erläutert.

Das [**buttondemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-buttondemos) -Beispiel verwendet dieses Verfahren auch in der- `MainPage` Klasse. Die Datei " **MainPage. XAML** " enthält eine `Button` für jede Seite des Beispiels:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:ButtonDemos"
             x:Class="ButtonDemos.MainPage"
             Title="Button Demos">
    <ScrollView>
        <FlexLayout Direction="Column"
                    JustifyContent="SpaceEvenly"
                    AlignItems="Center">

            <Button Text="Basic Button Click"
                    Command="{Binding NavigateCommand}"
                    CommandParameter="{x:Type local:BasicButtonClickPage}" />

            <Button Text="Code Button Click"
                    Command="{Binding NavigateCommand}"
                    CommandParameter="{x:Type local:CodeButtonClickPage}" />

            <Button Text="Basic Button Command"
                    Command="{Binding NavigateCommand}"
                    CommandParameter="{x:Type local:BasicButtonCommandPage}" />

            <Button Text="Press and Release Button"
                    Command="{Binding NavigateCommand}"
                    CommandParameter="{x:Type local:PressAndReleaseButtonPage}" />

            <Button Text="Button Appearance"
                    Command="{Binding NavigateCommand}"
                    CommandParameter="{x:Type local:ButtonAppearancePage}" />

            <Button Text="Toggle Button Demo"
                    Command="{Binding NavigateCommand}"
                    CommandParameter="{x:Type local:ToggleButtonDemoPage}" />

            <Button Text="Image Button Demo"
                    Command="{Binding NavigateCommand}"
                    CommandParameter="{x:Type local:ImageButtonDemoPage}" />

        </FlexLayout>
    </ScrollView>
</ContentPage>
```

Jeder `Button` hat seine- `Command` Eigenschaft an eine Eigenschaft mit dem Namen `NavigateCommand` , und die `CommandParameter` ist auf ein-Objekt festgelegt, das [`Type`](xref:System.Type) einer der Seiten Klassen im Projekt entspricht.

Diese `NavigateCommand` Eigenschaft ist vom Typ `ICommand` und wird in der Code Behind-Datei definiert:

```csharp
public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();

        NavigateCommand = new Command<Type>(async (Type pageType) =>
        {
            Page page = (Page)Activator.CreateInstance(pageType);
            await Navigation.PushAsync(page);
        });

        BindingContext = this;
    }

    public ICommand NavigateCommand { private set; get; }
}
```

Der-Konstruktor initialisiert die- `NavigateCommand` Eigenschaft für ein- `Command<Type>` Objekt, da `Type` der Typ des Objekts ist, das `CommandParameter` in der XAML-Datei festgelegt ist. Dies bedeutet, dass die `execute` Methode über ein Argument vom Typ verfügt `Type` , das diesem- `CommandParameter` Objekt entspricht. Die-Funktion instanziiert die Seite und navigiert zu ihr.

Beachten Sie, dass der Konstruktor beendet wird `BindingContext` , indem er auf sich selbst festlegt. Dies ist erforderlich, damit Eigenschaften in der XAML-Datei an die-Eigenschaft gebunden werden können `NavigateCommand` .

## <a name="pressing-and-releasing-the-button"></a>Drücken und Freigeben der Schaltfläche

Neben dem `Clicked` -Ereignis `Button` definiert auch [`Pressed`](xref:Xamarin.Forms.Button.Pressed) -und- [`Released`](xref:Xamarin.Forms.Button.Released) Ereignisse. Das- `Pressed` Ereignis tritt auf, wenn ein Finger auf ein drückt `Button` , oder wenn eine Maustaste gedrückt wird, während der Zeiger auf dem positioniert ist `Button` . Das `Released` Ereignis tritt auf, wenn der Finger oder die Maustaste losgelassen wird. Im allgemeinen `Clicked` wird auch ein-Ereignis zur gleichen Zeit ausgelöst wie das- `Released` Ereignis, aber wenn der Finger oder Mauszeiger vor der Freigabe von der-Oberfläche der bewegt wird `Button` , `Clicked` tritt das Ereignis möglicherweise nicht auf.

Das `Pressed` -Ereignis und das- `Released` Ereignis werden nicht häufig verwendet, Sie können jedoch für besondere Zwecke verwendet werden, wie auf der Schaltflächen Seite " **drücken und freigeben** " gezeigt. Die XAML-Datei enthält eine `Label` und eine `Button` mit Handlern, die für die Ereignisse und angefügt sind `Pressed` `Released` :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="ButtonDemos.PressAndReleaseButtonPage"
             Title="Press and Release Button">
    <StackLayout>

        <Label x:Name="label"
               Text="Press and hold the Button below"
               FontSize="Large"
               VerticalOptions="CenterAndExpand"
               HorizontalOptions="Center" />

        <Button Text="Press to Rotate Text!"
                VerticalOptions="CenterAndExpand"
                HorizontalOptions="Center"
                Pressed="OnButtonPressed"
                Released="OnButtonReleased" />

    </StackLayout>
</ContentPage>
```

Die Code-Behind-Datei animiert den `Label` , wenn ein- `Pressed` Ereignis auftritt, hält aber die Drehung an, wenn ein- `Released` Ereignis auftritt:

```csharp
public partial class PressAndReleaseButtonPage : ContentPage
{
    bool animationInProgress = false;
    Stopwatch stopwatch = new Stopwatch();

    public PressAndReleaseButtonPage ()
    {
        InitializeComponent ();
    }

    void OnButtonPressed(object sender, EventArgs args)
    {
        stopwatch.Start();
        animationInProgress = true;

        Device.StartTimer(TimeSpan.FromMilliseconds(16), () =>
        {
            label.Rotation = 360 * (stopwatch.Elapsed.TotalSeconds % 1);

            return animationInProgress;
        });
    }

    void OnButtonReleased(object sender, EventArgs args)
    {
        animationInProgress = false;
        stopwatch.Stop();
    }
}
```

Das Ergebnis ist, dass `Label` nur rotiert, wenn sich ein Finger in der Kontaktaufnahme mit dem befindet `Button` , und beendet, wenn der Finger losgelassen wird:

[![Schaltfläche "drücken und freigeben"](button-images/PressAndReleaseButton.png "Schaltfläche "drücken und freigeben"")](button-images/PressAndReleaseButton-Large.png)

Diese Art von Verhalten hat Anwendungen für Spiele: ein Finger, der auf einem-Objekt gehalten wird, `Button` kann dazu führen, dass ein Objekt auf dem Bildschirm in eine bestimmte Richtung verschoben

## <a name="button-appearance"></a>Darstellung der Schaltfläche

Das `Button` erbt oder definiert mehrere Eigenschaften, die sich auf seine Darstellung auswirken:

- [`TextColor`](xref:Xamarin.Forms.Button.TextColor)die `Button` Textfarbe.
- [`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor)die Farbe des Hintergrunds für diesen Text.
- [`BorderColor`](xref:Xamarin.Forms.Button.BorderColor)die Farbe eines Bereichs, der das`Button`
- [`FontFamily`](xref:Xamarin.Forms.Button.FontFamily)die für den Text verwendete Schriftfamilie.
- [`FontSize`](xref:Xamarin.Forms.Button.FontSize)die Textgröße.
- [`FontAttributes`](xref:Xamarin.Forms.Button.FontAttributes)Gibt an, ob der Text kursiv oder fett formatiert ist.
- [`BorderWidth`](xref:Xamarin.Forms.Button.BorderWidth)die Breite des Rahmens.
- [`CornerRadius`](xref:Xamarin.Forms.Button.CornerRadius)ist der Eckradius des`Button`
- `CharacterSpacing`der Abstand zwischen den Zeichen des Texts. `Button`

> [!NOTE]
> Die `Button` -Klasse verfügt auch über [`Margin`](xref:Xamarin.Forms.View.Margin) -und- [`Padding`](xref:Xamarin.Forms.Button.Padding) Eigenschaften, die das Layoutverhalten von Steuern `Button` . Weitere Informationen finden Sie unter [Ränder und Abstände](~/xamarin-forms/user-interface/layouts/margin-and-padding.md).

Die Auswirkungen von sechs dieser Eigenschaften (ausgenommen `FontFamily` und `FontAttributes` ) werden auf der Seite "Darstellung der **Schaltfläche** " veranschaulicht. Eine weitere Eigenschaft, [`Image`](xref:Xamarin.Forms.Button.ImageSource) , wird im Abschnitt [**Verwenden von Bitmaps mit Schaltfläche**](#using-bitmaps-with-buttons)erläutert.

Alle Ansichten und Daten Bindungen auf der Seite "Darstellung der **Schaltfläche** " werden in der XAML-Datei definiert:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:ButtonDemos"
             x:Class="ButtonDemos.ButtonAppearancePage"
             Title="Button Appearance">
    <StackLayout>
        <Button x:Name="button"
                Text="Button"
                VerticalOptions="CenterAndExpand"
                HorizontalOptions="Center"
                TextColor="{Binding Source={x:Reference textColorPicker},
                                    Path=SelectedItem.Color}"
                BackgroundColor="{Binding Source={x:Reference backgroundColorPicker},
                                          Path=SelectedItem.Color}"
                BorderColor="{Binding Source={x:Reference borderColorPicker},
                                      Path=SelectedItem.Color}" />

        <StackLayout BindingContext="{x:Reference button}"
                     Padding="10">

            <Slider x:Name="fontSizeSlider"
                    Maximum="48"
                    Minimum="1"
                    Value="{Binding FontSize}" />

            <Label Text="{Binding Source={x:Reference fontSizeSlider},
                                  Path=Value,
                                  StringFormat='FontSize = {0:F0}'}"
                   HorizontalTextAlignment="Center" />

            <Slider x:Name="borderWidthSlider"
                    Minimum="-1"
                    Maximum="12"
                    Value="{Binding BorderWidth}" />

            <Label Text="{Binding Source={x:Reference borderWidthSlider},
                                  Path=Value,
                                  StringFormat='BorderWidth = {0:F0}'}"
                   HorizontalTextAlignment="Center" />

            <Slider x:Name="cornerRadiusSlider"
                    Minimum="-1"
                    Maximum="24"
                    Value="{Binding CornerRadius}" />

            <Label Text="{Binding Source={x:Reference cornerRadiusSlider},
                                  Path=Value,
                                  StringFormat='CornerRadius = {0:F0}'}"
                   HorizontalTextAlignment="Center" />

            <Grid>
                <Grid.RowDefinitions>
                    <RowDefinition Height="Auto" />
                    <RowDefinition Height="Auto" />
                    <RowDefinition Height="Auto" />
                </Grid.RowDefinitions>

                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="*" />
                    <ColumnDefinition Width="*" />
                </Grid.ColumnDefinitions>

                <Grid.Resources>
                    <Style TargetType="Label">
                        <Setter Property="VerticalOptions" Value="Center" />
                    </Style>
                </Grid.Resources>

                <Label Text="Text Color:"
                       Grid.Row="0" Grid.Column="0" />

                <Picker x:Name="textColorPicker"
                        ItemsSource="{Binding Source={x:Static local:NamedColor.All}}"
                        ItemDisplayBinding="{Binding FriendlyName}"
                        SelectedIndex="0"
                        Grid.Row="0" Grid.Column="1" />

                <Label Text="Background Color:"
                       Grid.Row="1" Grid.Column="0" />

                <Picker x:Name="backgroundColorPicker"
                        ItemsSource="{Binding Source={x:Static local:NamedColor.All}}"
                        ItemDisplayBinding="{Binding FriendlyName}"
                        SelectedIndex="0"
                        Grid.Row="1" Grid.Column="1" />

                <Label Text="Border Color:"
                       Grid.Row="2" Grid.Column="0" />

                <Picker x:Name="borderColorPicker"
                        ItemsSource="{Binding Source={x:Static local:NamedColor.All}}"
                        ItemDisplayBinding="{Binding FriendlyName}"
                        SelectedIndex="0"
                        Grid.Row="2" Grid.Column="1" />
            </Grid>
        </StackLayout>
    </StackLayout>
</ContentPage>
```

Die `Button` drei Eigenschaften am oberen Rand der Seite sind an `Color` `Picker` Elemente am unteren Rand der Seite gebunden. Die Elemente in den `Picker` Elementen sind Farben der- `NamedColor` Klasse, die im Projekt enthalten ist. Drei `Slider` -Elemente enthalten bidirektionale Bindungen zu den `FontSize` `BorderWidth` Eigenschaften, und `CornerRadius` von `Button` .

Mit diesem Programm können Sie mit Kombinationen aller folgenden Eigenschaften experimentieren:

[![Darstellung der Schaltfläche](button-images/ButtonAppearance.png "Darstellung der Schaltfläche")](button-images/ButtonAppearance-Large.png)

Um den Rahmen anzuzeigen `Button` , müssen Sie einen `BorderColor` auf einen anderen Wert als festlegen `Default` und `BorderWidth` auf einen positiven Wert festlegen.

Unter IOS werden Sie feststellen, dass große Rahmen breiten in das Innere der Eindringen `Button` und die Anzeige von Text stören. Wenn Sie sich für die Verwendung eines Rahmens mit IOS entscheiden `Button` , sollten Sie die-Eigenschaft wahrscheinlich mit Leerzeichen versehen und beenden, `Text` um die Sichtbarkeit beizubehalten.

Bei UWP wird durch die Auswahl einer, die die `CornerRadius` Hälfte der Höhe von überschreitet, `Button` eine Ausnahme ausgelöst.

## <a name="button-visual-states"></a>Visuelle Schaltflächen für Flächen

[`Button`](xref:Xamarin.Forms.Button)verfügt über ein `Pressed` [`VisualState`](xref:Xamarin.Forms.VisualState) -Element, das zum Initiieren einer visuellen Änderung am verwendet werden kann `Button` , wenn es vom Benutzer gedrückt wird, vorausgesetzt, dass es aktiviert ist.

Das folgende XAML-Beispiel zeigt, wie Sie einen visuellen Zustand für den `Pressed` Status definieren:

```xaml
<Button Text="Click me!"
        ...>
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">
            <VisualState x:Name="Normal">
                <VisualState.Setters>
                    <Setter Property="Scale"
                            Value="1" />
                </VisualState.Setters>
            </VisualState>

            <VisualState x:Name="Pressed">
                <VisualState.Setters>
                    <Setter Property="Scale"
                            Value="0.8" />
                </VisualState.Setters>
            </VisualState>

        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Button>
```

Der `Pressed` [`VisualState`](xref:Xamarin.Forms.VisualState) gibt an, dass die- [`Button`](xref:Xamarin.Forms.Button) Eigenschaft beim Drücken [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) von von ihrem Standardwert von 1 in 0,8 geändert wird. Der `Normal` `VisualState` gibt an, dass die- `Button` `Scale` Eigenschaft auf 1 festgelegt wird, wenn sich der in einem normalen Zustand befindet. Der Gesamteffekt besteht daher darin, dass beim `Button` drücken von eine etwas geringere Größe erreicht wird, und wenn der freigegeben wird, wird `Button` er auf seine Standardgröße umgestellt.

Weitere Informationen zu visuellen Zuständen finden Sie [unter Xamarin.Forms Visual State Manager](~/xamarin-forms/user-interface/visual-state-manager.md).

## <a name="creating-a-toggle-button"></a>Erstellen einer UMSCHALT Fläche

Es ist möglich, die Unterklasse so zu unterteilen `Button` , dass Sie wie ein on-off-Switch funktioniert: Tippen Sie einmal auf die Schaltfläche, um die Schaltfläche einzublenden, und tippen Sie erneut, um Sie zu deaktivieren.

Die folgende `ToggleButton` Klasse wird von abgeleitet `Button` und definiert ein neues Ereignis mit dem Namen `Toggled` und eine boolesche Eigenschaft mit dem Namen `IsToggled` . Dabei handelt es sich um die gleichen zwei Eigenschaften, die von definiert werden Xamarin.Forms [`Switch`](xref:Xamarin.Forms.Switch) :

```csharp
class ToggleButton : Button
{
    public event EventHandler<ToggledEventArgs> Toggled;

    public static BindableProperty IsToggledProperty =
        BindableProperty.Create("IsToggled", typeof(bool), typeof(ToggleButton), false,
                                propertyChanged: OnIsToggledChanged);

    public ToggleButton()
    {
        Clicked += (sender, args) => IsToggled ^= true;
    }

    public bool IsToggled
    {
        set { SetValue(IsToggledProperty, value); }
        get { return (bool)GetValue(IsToggledProperty); }
    }

    protected override void OnParentSet()
    {
        base.OnParentSet();
        VisualStateManager.GoToState(this, "ToggledOff");
    }

    static void OnIsToggledChanged(BindableObject bindable, object oldValue, object newValue)
    {
        ToggleButton toggleButton = (ToggleButton)bindable;
        bool isToggled = (bool)newValue;

        // Fire event
        toggleButton.Toggled?.Invoke(toggleButton, new ToggledEventArgs(isToggled));

        // Set the visual state
        VisualStateManager.GoToState(toggleButton, isToggled ? "ToggledOn" : "ToggledOff");
    }
}
```

Der- `ToggleButton` Konstruktor fügt einen Handler an das- `Clicked` Ereignis an, sodass er den Wert der-Eigenschaft ändern kann `IsToggled` . Die- `OnIsToggledChanged` Methode löst das-Ereignis aus `Toggled` .

In der letzten Zeile der- `OnIsToggledChanged` Methode wird die statische- `VisualStateManager.GoToState` Methode mit den beiden Text Zeichenfolgen "toggledon" und "toggledoff" aufgerufen. Informationen zu dieser Methode und dazu, wie Ihre Anwendung auf visuelle Zustände reagieren kann, finden Sie im Artikel " [** Xamarin.Forms Visual State Manager**](~/xamarin-forms/user-interface/visual-state-manager.md)".

Da `ToggleButton` den-Befehl aufruft `VisualStateManager.GoToState` , muss die Klasse selbst keine zusätzlichen Funktionen einschließen, um die Darstellung der Schaltfläche auf der Grundlage Ihres `IsToggled` Zustands zu ändern. Dies liegt in der Verantwortung der XAML, die hostet `ToggleButton` .

Die Demo Seite zum Umschalten der **Schaltfläche** enthält zwei Instanzen von `ToggleButton` , einschließlich des visuellen Zustands-Manager-Markups, das `Text` , `BackgroundColor` und `TextColor` der Schaltfläche auf der Grundlage des visuellen Zustands festlegt:

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:ButtonDemos"
             x:Class="ButtonDemos.ToggleButtonDemoPage"
             Title="Toggle Button Demo">

    <ContentPage.Resources>
        <Style TargetType="local:ToggleButton">
            <Setter Property="VerticalOptions" Value="CenterAndExpand" />
            <Setter Property="HorizontalOptions" Value="Center" />
        </Style>
    </ContentPage.Resources>

    <StackLayout Padding="10, 0">
        <local:ToggleButton Toggled="OnItalicButtonToggled">
            <VisualStateManager.VisualStateGroups>
                <VisualStateGroup Name="ToggleStates">
                    <VisualState Name="ToggledOff">
                        <VisualState.Setters>
                            <Setter Property="Text" Value="Italic Off" />
                            <Setter Property="BackgroundColor" Value="#C0C0C0" />
                            <Setter Property="TextColor" Value="Black" />
                        </VisualState.Setters>
                    </VisualState>

                    <VisualState Name="ToggledOn">
                        <VisualState.Setters>
                            <Setter Property="Text" Value=" Italic On " />
                            <Setter Property="BackgroundColor" Value="#404040" />
                            <Setter Property="TextColor" Value="White" />
                        </VisualState.Setters>
                    </VisualState>
                </VisualStateGroup>
            </VisualStateManager.VisualStateGroups>
        </local:ToggleButton>

        <local:ToggleButton Toggled="OnBoldButtonToggled">
            <VisualStateManager.VisualStateGroups>
                <VisualStateGroup Name="ToggleStates">
                    <VisualState Name="ToggledOff">
                        <VisualState.Setters>
                            <Setter Property="Text" Value="Bold Off" />
                            <Setter Property="BackgroundColor" Value="#C0C0C0" />
                            <Setter Property="TextColor" Value="Black" />
                        </VisualState.Setters>
                    </VisualState>

                    <VisualState Name="ToggledOn">
                        <VisualState.Setters>
                            <Setter Property="Text" Value=" Bold On " />
                            <Setter Property="BackgroundColor" Value="#404040" />
                            <Setter Property="TextColor" Value="White" />
                        </VisualState.Setters>
                    </VisualState>
                </VisualStateGroup>
            </VisualStateManager.VisualStateGroups>
        </local:ToggleButton>

        <Label x:Name="label"
               Text="Just a little passage of some sample text that can be formatted in italic or boldface by toggling the two buttons."
               FontSize="Large"
               HorizontalTextAlignment="Center"
               VerticalOptions="CenterAndExpand" />

    </StackLayout>
</ContentPage>
```

Die `Toggled` Ereignishandler befinden sich in der Code-Behind-Datei. Sie sind verantwortlich für das Festlegen der- `FontAttributes` Eigenschaft von `Label` basierend auf dem Zustand der Schaltflächen:

```csharp
public partial class ToggleButtonDemoPage : ContentPage
{
    public ToggleButtonDemoPage ()
    {
        InitializeComponent ();
    }

    void OnItalicButtonToggled(object sender, ToggledEventArgs args)
    {
        if (args.Value)
        {
            label.FontAttributes |= FontAttributes.Italic;
        }
        else
        {
            label.FontAttributes &= ~FontAttributes.Italic;
        }
    }

    void OnBoldButtonToggled(object sender, ToggledEventArgs args)
    {
        if (args.Value)
        {
            label.FontAttributes |= FontAttributes.Bold;
        }
        else
        {
            label.FontAttributes &= ~FontAttributes.Bold;
        }
    }
}
```

Hier ist das Programm, das unter IOS, Android und UWP ausgeführt wird:

[![Demo zum Umschalten der Schaltfläche](button-images/ToggleButtonDemo.png "Demo zum Umschalten der Schaltfläche")](button-images/ToggleButtonDemo-Large.png#lightbox)

## <a name="using-bitmaps-with-buttons"></a>Verwenden von Bitmaps mit Schaltflächen

Die- `Button` Klasse definiert eine- [`ImageSource`](xref:Xamarin.Forms.Button.Image) Eigenschaft, mit der Sie ein Bitmap-Bild auf dem anzeigen können `Button` , entweder allein oder in Kombination mit Text. Sie können auch angeben, wie der Text und das Bild angeordnet werden.

Die- `ImageSource` Eigenschaft ist vom Typ [`ImageSource`](xref:Xamarin.Forms.ImageSource) . Dies bedeutet, dass die Bitmaps aus einer Datei, einer eingebetteten Ressource, einem URI oder einem Stream geladen werden können.

> [!NOTE]
> Während ein `Button` animiertes GIF laden kann, wird nur der erste Frame der gif angezeigt.

Jede von unterstützte Plattform Xamarin.Forms ermöglicht die Speicherung von Bildern in mehreren Größen für verschiedene Pixel Auflösungen der verschiedenen Geräte, auf denen die Anwendung möglicherweise ausgeführt wird. Diese mehrere Bitmaps werden so benannt oder gespeichert, dass das Betriebssystem die beste Entsprechung für die Video Bildschirmauflösung des Geräts auswählen kann.

Bei einer Bitmap in einem `Button` liegt die beste Größe normalerweise zwischen 32 und 64 geräteunabhängigen Einheiten, je nachdem, wie groß Sie sein sollen. Die in diesem Beispiel verwendeten Images basieren auf einer Größe von 48 geräteunabhängigen Einheiten.

Im IOS-Projekt enthält der Ordner " **Resources** " drei Größen dieses Bilds:

- Eine quadratische Bitmap mit 48 Pixel, die als/Resources/gespeichert ist **MonkeyFace.png**
- Eine quadratische Bitmap mit 96 Pixel, gespeichert als**/Resource/MonkeyFace@2x.png**
- Eine quadratische Bitmap mit 144 Pixel, gespeichert als**/Resource/MonkeyFace@3x.png**

Alle drei Bitmaps erhielten eine **Buildaktion** von **bundleresource**.

Für das Android-Projekt haben die Bitmaps alle denselben Namen, aber Sie werden in verschiedenen Unterordnern des Ordners " **Resources** " gespeichert:

- Eine quadratische Bitmap mit 72 Pixel, die als/Resources/drawable-hdpi/gespeichert ist **MonkeyFace.png**
- Eine quadratische Bitmap mit 96 Pixel, die als/Resources/drawable-xhdpi/gespeichert ist **MonkeyFace.png**
- Eine quadratische Bitmap mit 144 Pixel, die als/Resources/drawable-xxhdpi/gespeichert ist **MonkeyFace.png**
- Eine quadratische Bitmap mit 192 Pixel, die als/Resources/drawable-xxxhdpi/gespeichert ist **MonkeyFace.png**

Diese **wurden als** Buildvorgang von " **androidresource**" festgestellt.

Im UWP-Projekt können Bitmaps an einer beliebigen Stelle im Projekt gespeichert werden, Sie werden jedoch im Allgemeinen in einem benutzerdefinierten Ordner oder dem vorhandenen **Assets** -Ordner gespeichert. Das UWP-Projekt enthält diese Bitmaps:

- Eine quadratische Bitmap mit 48 Pixel, die als/Assets/gespeichert ist **MonkeyFace.scale-100.png**
- Eine quadratische Bitmap mit 96 Pixel, die als/Assets/gespeichert ist **MonkeyFace.scale-200.png**
- Eine quadratische Bitmap mit 192 Pixel, die als/Assets/gespeichert ist **MonkeyFace.scale-400.png**

Sie wurden alle eine **Buildaktion** von **Inhalten**erhalten.

Mithilfe der-Eigenschaft von können Sie angeben, wie die `Text` -Eigenschaft und die- `ImageSource` Eigenschaft auf dem angeordnet werden `Button` [`ContentLayout`](xref:Xamarin.Forms.Button.ContentLayout) `Button` . Diese Eigenschaft [`ButtonContentLayout`](xref:Xamarin.Forms.Button.ButtonContentLayout) hat den Typ, bei dem es sich um eine eingebettete Klasse in handelt `Button` . Der [Konstruktor] (Xref: Xamarin.Forms . Button. buttoncontentlayout .% 23ctor ( Xamarin.Forms . Button. buttoncontentlayout. imageposition, System. Double)) hat zwei Argumente:

- Ein Member der- [`ImagePosition`](xref:Xamarin.Forms.Button.ButtonContentLayout.ImagePosition) Enumeration: `Left` , `Top` , `Right` oder, `Bottom` der angibt, wie die Bitmap relativ zum Text angezeigt wird.
- Ein `double` -Wert für den Abstand zwischen der Bitmap und dem Text.

Die Standardwerte sind `Left` und 10 Einheiten. Zwei schreibgeschützte Eigenschaften von mit `ButtonContentLayout` [`Position`](xref:Xamarin.Forms.Button.ButtonContentLayout.Position) dem Namen und [`Spacing`](xref:Xamarin.Forms.Button.ButtonContentLayout.Spacing) stellen die Werte dieser Eigenschaften bereit.

Im Code können Sie einen erstellen `Button` und die- `ContentLayout` Eigenschaft wie folgt festlegen:

```csharp
Button button = new Button
{
    Text = "button text",
    ImageSource = new FileImageSource
    {
        File = "image filename"
    },
    ContentLayout = new Button.ButtonContentLayout(Button.ButtonContentLayout.ImagePosition.Right, 20)
};
```

In XAML müssen Sie nur den Enumerationsmember oder den Abstand oder beides in beliebiger Reihenfolge angeben, die durch Kommas getrennt ist:

```xaml
<Button Text="button text"
        ImageSource="image filename"
        ContentLayout="Right, 20" />
```

Die **Demo Seite Bild Schaltfläche** verwendet `OnPlatform` , um unterschiedliche Dateinamen für die Bitmapdateien der IOS-, Android-und UWP-Datei anzugeben. Wenn Sie den gleichen Dateinamen für jede Plattform verwenden und die Verwendung von vermeiden möchten `OnPlatform` , müssen Sie die UWP-Bitmaps im Stammverzeichnis des Projekts speichern.

Mit dem ersten `Button` auf der **Bild Schaltfläche "Demo** " wird die Eigenschaft festgelegt, `Image` jedoch nicht die `Text` Eigenschaft:

```xaml
<Button>
    <Button.ImageSource>
        <OnPlatform x:TypeArguments="ImageSource">
            <On Platform="iOS, Android" Value="MonkeyFace.png" />
            <On Platform="UWP" Value="Assets/MonkeyFace.png" />
        </OnPlatform>
    </Button.ImageSource>
</Button>
```

Wenn die UWP-Bitmaps im Stammverzeichnis des Projekts gespeichert werden, kann dieses Markup erheblich vereinfacht werden:

```xaml
<Button ImageSource="MonkeyFace.png" />
```

Um in der **imagebuttondemo. XAML** -Datei eine Menge wiederholter Markup zu vermeiden, `Style` wird auch eine implizite definiert, um die-Eigenschaft festzulegen `ImageSource` . Dies `Style` wird automatisch auf fünf weitere `Button` Elemente angewendet. Hier ist die vollständige XAML-Datei:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="ButtonDemos.ImageButtonDemoPage">

    <FlexLayout Direction="Column"
                JustifyContent="SpaceEvenly"
                AlignItems="Center">

        <FlexLayout.Resources>
            <Style TargetType="Button">
                <Setter Property="ImageSource">
                    <OnPlatform x:TypeArguments="ImageSource">
                        <On Platform="iOS, Android" Value="MonkeyFace.png" />
                        <On Platform="UWP" Value="Assets/MonkeyFace.png" />
                    </OnPlatform>
                </Setter>
            </Style>
        </FlexLayout.Resources>

        <Button>
            <Button.ImageSource>
                <OnPlatform x:TypeArguments="ImageSource">
                    <On Platform="iOS, Android" Value="MonkeyFace.png" />
                    <On Platform="UWP" Value="Assets/MonkeyFace.png" />
                </OnPlatform>
            </Button.ImageSource>
        </Button>

        <Button Text="Default" />

        <Button Text="Left - 10"
                ContentLayout="Left, 10" />

        <Button Text="Top - 10"
                ContentLayout="Top, 10" />

        <Button Text="Right - 20"
                ContentLayout="Right, 20" />

        <Button Text="Bottom - 20"
                ContentLayout="Bottom, 20" />
    </FlexLayout>
</ContentPage>
```

Die letzten vier `Button` Elemente verwenden die `ContentLayout` -Eigenschaft, um eine Position und einen Abstand von Text und Bitmap anzugeben:

[![Demo für Bild Schaltfläche](button-images/ImageButtonDemo.png "Demo für Bild Schaltfläche")](button-images/ImageButtonDemo-Large.png#lightbox)

Sie haben nun die verschiedenen Methoden zum Behandeln von `Button` Ereignissen und Ändern der Darstellung gesehen `Button` .

## <a name="related-links"></a>Verwandte Links

- [Buttondemos-Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-buttondemos)
- [Button-API](xref:Xamarin.Forms.Button)
