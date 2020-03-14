---
title: Schaltfläche "Xamarin.Forms"
description: Die Schaltfläche mit der reagiert auf ein tippen oder klicken Sie auf, die eine Anwendung, um eine bestimmte Aufgabe durchzuführen weiterleitet.
ms.prod: xamarin
ms.assetid: 62CAEB63-0800-44F4-9B8C-EE632138C2F5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/04/2019
ms.openlocfilehash: f82d590213076f349b21ebdee2832f2bf474d2f2
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/13/2020
ms.locfileid: "79305846"
---
# <a name="xamarinforms-button"></a>Schaltfläche "Xamarin.Forms"

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-buttondemos)

_Die Schaltfläche antwortet auf eine Tap-oder Klick-Schaltfläche, die eine Anwendung anweist, eine bestimmte Aufgabe auszuführen._

Der [`Button`](xref:Xamarin.Forms.Button) ist das grundlegendste interaktive Steuerelement in allen xamarin. Forms. In der `Button` wird normalerweise eine kurze Text Zeichenfolge angezeigt, die einen Befehl anzeigt, aber es kann auch ein Bitmap-Bild oder eine Kombination aus Text und einem Bild angezeigt werden. Der Benutzer drückt den `Button` mit einem Finger oder klickt mit der Maus darauf, um diesen Befehl zu initiieren.

Die meisten der unten erläuterten Themen entsprechen den Seiten im [**buttondemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-buttondemos) -Beispiel.

## <a name="handling-button-clicks"></a>Schaltfläche "klickt

`Button` definiert ein [`Clicked`](xref:Xamarin.Forms.Button.Clicked) Ereignis, das ausgelöst wird, wenn der Benutzer mit einem Finger oder Mauszeiger auf die `Button` tippt. Das Ereignis wird ausgelöst, wenn der Finger oder die Maustaste von der Oberfläche des `Button`losgelassen wird. Für die `Button` muss die [`IsEnabled`](xref:Xamarin.Forms.VisualElement.IsEnabled) -Eigenschaft auf `true` festgelegt sein, damit Sie auf Abzweigungen reagieren kann.

Die **Schaltfläche "einfache Schaltfläche** " im [**buttondemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-buttondemos) -Beispiel veranschaulicht, wie ein `Button` in XAML instanziiert und das `Clicked` Ereignis behandelt wird. Die Datei " **basicbuttonclickpage. XAML** " enthält eine `StackLayout` mit einem `Label` und einem `Button`:

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

Der `Button` in der Regel den gesamten Platz belegen, der für ihn zulässig ist. Wenn Sie die `HorizontalOptions`-Eigenschaft von `Button` z. b. nicht auf `Fill`festgelegt haben, belegt der `Button` die gesamte Breite des übergeordneten Elements.

Standardmäßig ist die `Button` rechteckig, Sie können Sie jedoch mit der [`CornerRadius`](xref:Xamarin.Forms.Button.CornerRadius) -Eigenschaft gerundet werden, wie unten im Abschnitt Darstellung der [**Schaltfläche**](#button-appearance)beschrieben.

Die [`Text`](xref:Xamarin.Forms.Button.Text)-Eigenschaft gibt den Text an, der auf dem `Button`-Element angezeigt wird. Das [`Clicked`](xref:Xamarin.Forms.Button.Clicked) Ereignis ist auf einen Ereignishandler mit dem Namen `OnButtonClicked`festgelegt. Dieser Handler befindet sich in der Code-Behind-Datei **BasicButtonClickPage.XAML.cs**:

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

Wenn die `Button` getippt wird, wird die `OnButtonClicked`-Methode ausgeführt. Das `sender`-Argument ist das `Button` Objekt, das für dieses Ereignis verantwortlich ist. Sie können dies verwenden, um auf das `Button` Objekt zuzugreifen oder um zwischen mehreren `Button` Objekten zu unterscheiden, die dasselbe `Clicked` Ereignis verwenden.

Dieser bestimmte `Clicked` Handler Ruft eine Animations Funktion auf, die die `Label` 360 Grad in 1000 Millisekunden rotiert. So sieht das Programm ausgeführt wird, auf IOS- und Android-Geräte und als universelle Windows-Plattform (UWP)-Anwendung auf dem Windows 10-Desktop aus:

[![Einfache Schaltflächen-Click](button-images/BasicButtonClick.png "Einfache Schaltflächen-Click")](button-images/BasicButtonClick-Large.png#lightbox "Einfache Schaltflächen-Click")

Beachten Sie, dass die `OnButtonClicked`-Methode den `async`-Modifizierer enthält, da `await` im-Ereignishandler verwendet wird. Ein `Clicked` Ereignishandler erfordert den `async`-Modifizierer nur, wenn der Text des Handlers `await`verwendet.

Jede Plattform rendert die `Button` auf eigene Weise. Im Abschnitt " [**Schalt**](#button-appearance) Flächen Darstellung" erfahren Sie, wie Sie Farben festlegen und den `Button` Rahmen für stärker angepasste Auftritte sichtbar machen. `Button` implementiert die [`IFontElement`](xref:Xamarin.Forms.Internals.IFontElement) -Schnittstelle und enthält daher die Eigenschaften [`FontFamily`](xref:Xamarin.Forms.Button.FontFamily), [`FontSize`](xref:Xamarin.Forms.Button.FontSize)und [`FontAttributes`](xref:Xamarin.Forms.Button.FontAttributes) .

## <a name="creating-a-button-in-code"></a>Erstellen eine Schaltfläche in code

Es ist üblich, eine `Button` in XAML zu instanziieren, aber Sie können auch eine `Button` im Code erstellen. Dies kann praktisch sein, wenn Ihre Anwendung mehrere Schaltflächen auf der Grundlage von Daten erstellen muss, die mit einer `foreach` Schleife Aufzähl Bar sind.

Die **Schaltfläche für die Code Schaltfläche** zeigt, wie Sie eine Seite erstellen, die der **einfachen Schalt** Flächen-Schalt C#Flächen-Klick Seite, aber vollständig entspricht,

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

Alles wird in den Konstruktor der Klasse ausgeführt. Da der `Clicked` Handler nur eine Anweisung lang ist, kann er einfach an das Ereignis angefügt werden:

```csharp
button.Clicked += async (sender, args) => await label.RelRotateTo(360, 1000);
```

Natürlich können Sie den Ereignishandler auch als separate Methode definieren (genau wie die `OnButtonClick`-Methode in **einfachen Schaltflächen Klick**) und diese Methode an das-Ereignis anfügen:

```csharp
button.Clicked += OnButtonClicked;
```

## <a name="disabling-the-button"></a>Deaktivieren der Schaltfläche

Manchmal befindet sich eine Anwendung in einem bestimmten Zustand, in dem ein bestimmter `Button` Click kein gültiger Vorgang ist. In diesen Fällen sollte die `Button` durch Festlegen der Eigenschaft `IsEnabled` auf `false`deaktiviert werden. Das klassische Beispiel ist ein `Entry` Steuerelement für einen Dateinamen, der von einer Datei geöffneten `Button`begleitet wird: die `Button` sollte nur aktiviert werden, wenn ein Text in den `Entry`eingegeben wurde.
Sie können eine `DataTrigger` für diese Aufgabe verwenden, wie im Artikel [**Daten Trigger**](~/xamarin-forms/app-fundamentals/triggers.md#data-triggers) gezeigt.

## <a name="using-the-command-interface"></a>Verwenden die Befehlsschnittstelle

Es ist möglich, dass eine Anwendung auf `Button` TAPS antwortet, ohne das `Clicked`-Ereignis zu behandeln. Der `Button` implementiert einen alternativen Benachrichtigungs Mechanismus, der als _Befehl_ _oder Befehls_ Schnittstelle bezeichnet wird. Dies besteht aus zwei Eigenschaften:

- [`Command`](xref:Xamarin.Forms.Button.Command) vom Typ [`ICommand`](xref:System.Windows.Input.ICommand), eine Schnittstelle, die im [`System.Windows.Input`](xref:System.Windows.Input) Namespace definiert ist.
- [`CommandParameter`](xref:Xamarin.Forms.Button.CommandParameter) Eigenschaft vom Typ [`Object`](xref:System.Object).

Dieser Ansatz eignet sich besonders im Zusammenhang mit Datenbindung und insbesondere dann, wenn die Model-View-ViewModel (MVVM)-Architektur zu implementieren. Diese Themen werden in den Artikeln [Datenbindung](~/xamarin-forms/app-fundamentals/data-binding/index.md), [Daten Bindungen an MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)und [MVVM](~/xamarin-forms/enterprise-application-patterns/mvvm.md)erläutert.

In einer MVVM-Anwendung definiert das ViewModel Eigenschaften vom Typ `ICommand`, die dann mit den XAML-`Button` Elementen mit Daten Bindungen verbunden werden. Xamarin. Forms definiert auch [`Command`](xref:Xamarin.Forms.Command) -und [`Command<T>`](xref:Xamarin.Forms.Command`1) Klassen, die die `ICommand`-Schnittstelle implementieren und das ViewModel beim Definieren von Eigenschaften vom Typ `ICommand`unterstützen.

Der Befehl wird im Artikel über [**die Befehlsschnittstelle**](~/xamarin-forms/app-fundamentals/data-binding/commanding.md) ausführlicher beschrieben, aber auf der **Befehls Seite grundlegende Schalt** Flächen im [**buttondemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-buttondemos) -Beispiel wird der grundlegende Ansatz veranschaulicht.

Die `CommandDemoViewModel`-Klasse ist ein sehr einfaches ViewModel, das eine Eigenschaft des Typs `double` mit dem Namen `Number`definiert, und zwei Eigenschaften vom Typ `ICommand` mit dem Namen `MultiplyBy2Command` und `DivideBy2Command`:

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

Die beiden `ICommand` Eigenschaften werden im Konstruktor der Klasse mit zwei Objekten des Typs "`Command`" initialisiert. Die `Command`-Konstruktoren enthalten eine kleine Funktion (als `execute`-Konstruktorargument bezeichnet), die die `Number` Eigenschaft verdoppelt oder halbiert.

Die Datei " **basicbuttoncommand. XAML** " legt die `BindingContext` auf eine Instanz von `CommandDemoViewModel`fest. Das `Label`-Element und zwei `Button`-Elemente enthalten Bindungen zu den drei Eigenschaften in `CommandDemoViewModel`:

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

Es ist auch möglich, dass die `Command`-Objekte das Aktivieren und Deaktivieren der `Button` Elemente steuern. Nehmen Sie beispielsweise an, Sie möchten den Bereich von Zahlenwerten zwischen 2<sup>10</sup> und 2<sup>&ndash;10</sup>begrenzen. Sie können dem Konstruktor eine weitere Funktion hinzufügen (als `canExecute` Argument bezeichnet), die `true` zurückgibt, wenn die `Button` aktiviert werden soll. Im folgenden finden Sie die Änderungen am `CommandDemoViewModel`-Konstruktor:

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

Die Aufrufe der `ChangeCanExecute`-Methode von `Command` sind erforderlich, damit die `Command`-Methode die `canExecute`-Methode aufrufen und ermitteln kann, ob die `Button` deaktiviert werden soll oder nicht. Bei dieser Codeänderung wird die `Button` deaktiviert, da die Zahl den Grenzwert erreicht:

[![Grundlegende Schaltflächen Befehle-geändert](button-images/BasicButtonCommandModified.png "Grundlegende Schaltflächen Befehle-geändert")](button-images/BasicButtonCommandModified-Large.png#lightbox)

Es ist möglich, dass zwei oder mehr `Button` Elemente an dieselbe `ICommand` Eigenschaft gebunden werden. Die `Button` Elemente können mithilfe der [`CommandParameter`](xref:Xamarin.Forms.Button.CommandParameter) -Eigenschaft von `Button`unterschieden werden. In diesem Fall möchten Sie die generische [`Command<T>`](xref:Xamarin.Forms.Command`1) -Klasse verwenden. Das `CommandParameter` Objekt wird dann als Argument an die Methoden `execute` und `canExecute` weitergegeben. Dieses Verfahren wird im Artikel " [**grundlegende**](~/xamarin-forms/app-fundamentals/data-binding/commanding.md#basic-commanding) Befehls [**Schnittstelle**](~/xamarin-forms/app-fundamentals/data-binding/commanding.md#basic-commanding) " ausführlich erläutert.

Das [**buttondemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-buttondemos) -Beispiel verwendet dieses Verfahren auch in der `MainPage`-Klasse. Die Datei " **MainPage. XAML** " enthält eine `Button` für jede Seite des Beispiels:

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

Für jede `Button` ist die `Command`-Eigenschaft an eine Eigenschaft mit dem Namen `NavigateCommand`gebunden, und der `CommandParameter` wird auf ein [`Type`](xref:System.Type) Objekt festgelegt, das einer der Seiten Klassen im Projekt entspricht.

Diese `NavigateCommand` Eigenschaft hat den Typ `ICommand` und wird in der Code Behind-Datei definiert:

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

Der-Konstruktor initialisiert die `NavigateCommand`-Eigenschaft mit einem `Command<Type>`-Objekt, da `Type` der Typ des `CommandParameter` Objekts ist, das in der XAML-Datei festgelegt ist. Dies bedeutet, dass die `execute`-Methode über ein Argument vom Typ `Type` verfügt, das diesem `CommandParameter`-Objekt entspricht. Die Funktion die Seite instanziiert und wechselt anschließend darauf.

Beachten Sie, dass der Konstruktor beendet wird, indem er seine `BindingContext` auf sich selbst festlegt. Dies ist erforderlich, damit Eigenschaften in der XAML-Datei an die `NavigateCommand`-Eigenschaft gebunden werden können.

## <a name="pressing-and-releasing-the-button"></a>Drücken und Loslassen der Taste

Neben dem `Clicked`-Ereignis definiert das `Button`-Element auch die Ereignisse [`Pressed`](xref:Xamarin.Forms.Button.Pressed) und [`Released`](xref:Xamarin.Forms.Button.Released). Das `Pressed` Ereignis tritt auf, wenn ein Finger auf einen `Button`drückt, oder wenn eine Maustaste gedrückt wird, während der Zeiger auf die `Button`positioniert ist. Das `Released` Ereignis tritt auf, wenn der Finger oder die Maustaste losgelassen wird. Im Allgemeinen wird ein `Clicked` Ereignis auch zur gleichen Zeit ausgelöst wie das Ereignis `Released`, aber wenn der Finger oder Mauszeiger vor der Freigabe von der Oberfläche des `Button` bewegt wird, tritt das `Clicked` Ereignis möglicherweise nicht auf.

Die `Pressed`-und `Released` Ereignisse werden nicht häufig verwendet, Sie können jedoch für besondere Zwecke verwendet werden, wie auf der Schaltflächen Seite " **drücken und freigeben** " gezeigt. Die XAML-Datei enthält eine `Label` und eine `Button` mit Handlern, die für die Ereignisse `Pressed` und `Released` angefügt sind:

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

Die Code-Behind-Datei animiert die `Label`, wenn ein `Pressed` Ereignis auftritt, hält aber die Drehung an, wenn ein `Released` Ereignis auftritt:

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

Das Ergebnis ist, dass die `Label` nur rotiert, während sich der Finger in Kontakt mit dem `Button`befindet, und beendet, wenn der Finger losgelassen wird:

[![Schaltfläche "drücken und freigeben"](button-images/PressAndReleaseButton.png "Schaltfläche drücken und freigeben")](button-images/PressAndReleaseButton-Large.png)

Diese Art von Verhalten hat Anwendungen für Spiele: ein Finger, der auf einem `Button` gehalten wird, kann ein Bild auf dem Bildschirm in eine bestimmte Richtung verschieben.

<a name="button-appearance" />

## <a name="button-appearance"></a>Darstellung der Schaltfläche

Der `Button` erbt oder definiert mehrere Eigenschaften, die sich auf seine Darstellung auswirken:

- [`TextColor`](xref:Xamarin.Forms.Button.TextColor) ist die Farbe des `Button` Texts.
- [`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor) die Farbe des Hintergrunds für diesen Text.
- [`BorderColor`](xref:Xamarin.Forms.Button.BorderColor) ist die Farbe eines Bereichs, der die `Button`
- [`FontFamily`](xref:Xamarin.Forms.Button.FontFamily) ist die für den Text verwendete Schriftfamilie.
- [`FontSize`](xref:Xamarin.Forms.Button.FontSize) ist die Textgröße.
- [`FontAttributes`](xref:Xamarin.Forms.Button.FontAttributes) gibt an, ob der Text kursiv oder fett formatiert ist.
- [`BorderWidth`](xref:Xamarin.Forms.Button.BorderWidth) ist die Breite des Rahmens.
- [`CornerRadius`](xref:Xamarin.Forms.Button.CornerRadius) ist der Eckradius des `Button`
- `CharacterSpacing` ist der Abstand zwischen Zeichen des `Button` Texts.

> [!NOTE]
> Die `Button`-Klasse verfügt auch über [`Margin`](xref:Xamarin.Forms.View.Margin) -und [`Padding`](xref:Xamarin.Forms.Button.Padding) -Eigenschaften, die das Layoutverhalten der `Button`steuern. Weitere Informationen finden Sie unter [Ränder und Abstände](~/xamarin-forms/user-interface/layouts/margin-and-padding.md).

Die Auswirkungen von sechs dieser Eigenschaften (ausgenommen `FontFamily` und `FontAttributes`) werden auf der Seite "Darstellung der **Schaltfläche** " veranschaulicht. Eine weitere Eigenschaft, [`Image`](xref:Xamarin.Forms.Button.ImageSource), wird im Abschnitt [**Verwenden von Bitmaps mit Schaltfläche**](#image-button)erläutert.

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

Der `Button` am oberen Rand der Seite verfügt über die drei `Color` Eigenschaften, die an `Picker` Elemente am unteren Rand der Seite gebunden sind. Die Elemente in den `Picker` Elementen sind Farben aus der `NamedColor` Klasse, die im Projekt enthalten ist. Drei `Slider` Elemente enthalten bidirektionale Bindungen zu den Eigenschaften `FontSize`, `BorderWidth`und `CornerRadius` der `Button`.

Dieses Programm ermöglicht Ihnen das Experimentieren mit Kombinationen von all diese Eigenschaften:

[![Darstellung der Schaltfläche](button-images/ButtonAppearance.png "Darstellung der Schaltfläche")](button-images/ButtonAppearance-Large.png)

Um den `Button` Rahmen anzuzeigen, müssen Sie eine `BorderColor` auf einen anderen Wert als `Default`festlegen und auf einen positiven Wert `BorderWidth`.

Unter IOS werden Sie feststellen, dass große Grenzwerte in das Innere der `Button` eindringen und die Anzeige von Text stören. Wenn Sie sich für die Verwendung eines Rahmens mit einem IOS-`Button`entscheiden, sollten Sie die `Text`-Eigenschaft wahrscheinlich mit Leerzeichen versehen und beenden, um die Sichtbarkeit beizubehalten.

Bei UWP wird beim Auswählen einer `CornerRadius`, die die Hälfte der Höhe des `Button` überschreitet, eine Ausnahme ausgelöst.

## <a name="button-visual-states"></a>Visuelle Zustände der Schaltfläche

[`Button`](xref:Xamarin.Forms.Button) verfügt über eine `Pressed` [`VisualState`](xref:Xamarin.Forms.VisualState) , die zum Initiieren einer visuellen Änderung am `Button` verwendet werden kann, wenn der Benutzer darauf klickt, sofern diese aktiviert ist.

Das folgende XAML-Beispiel zeigt, wie ein visueller Zustand für den `Pressed` Status definiert wird:

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

Die `Pressed` [`VisualState`](xref:Xamarin.Forms.VisualState) gibt an, dass die [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) Eigenschaft beim Drücken der [`Button`](xref:Xamarin.Forms.Button) von ihrem Standardwert von 1 in 0,8 geändert wird. Die `Normal` `VisualState` gibt an, dass die `Scale` Eigenschaft auf 1 festgelegt wird, wenn sich die `Button` in einem normalen Zustand befindet. Der Gesamteffekt ist, dass die `Button` gedrückt wird, dass Sie etwas kleiner ist, und wenn die `Button` freigegeben wird, wird Sie auf Ihre Standardgröße umgestellt.

Weitere Informationen zu visuellen Zuständen finden Sie [unter xamarin. Forms Visual State Manager](~/xamarin-forms/user-interface/visual-state-manager.md).

## <a name="creating-a-toggle-button"></a>Erstellen eine Umschaltfläche

Es ist möglich, die Unterklasse `Button`, sodass Sie wie ein on-off-Switch funktioniert: Tippen Sie einmal auf die Schaltfläche, um die Schaltfläche ein-und auszuschalten, um Sie erneut zu aktivieren.

Die folgende `ToggleButton` Klasse wird von `Button` abgeleitet und definiert ein neues Ereignis namens `Toggled` und eine boolesche Eigenschaft mit dem Namen `IsToggled`. Dabei handelt es sich um die gleichen zwei Eigenschaften, die vom xamarin. Forms- [`Switch`](xref:Xamarin.Forms.Switch)definiert werden:

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

Der `ToggleButton`-Konstruktor fügt einen Handler an das `Clicked` Ereignis an, sodass der Wert der `IsToggled`-Eigenschaft geändert werden kann. Die `OnIsToggledChanged`-Methode löst das `Toggled`-Ereignis aus.

In der letzten Zeile der `OnIsToggledChanged`-Methode wird die statische `VisualStateManager.GoToState`-Methode mit den beiden Text Zeichenfolgen "toggledon" und "toggledoff" aufgerufen. Informationen zu dieser Methode und dazu, wie Ihre Anwendung auf visuelle Zustände reagieren kann, finden Sie im Artikel [**xamarin. Forms Visual State Manager**](~/xamarin-forms/user-interface/visual-state-manager.md).

Da `ToggleButton` den `VisualStateManager.GoToState`aufruft, muss die Klasse selbst keine zusätzlichen Einrichtungen einschließen, um die Darstellung der Schaltfläche auf der Grundlage des `IsToggled` Zustands zu ändern. Dies liegt in der Verantwortung der XAML, die die `ToggleButton`hostet.

Die Demo Seite zum Umschalten der **Schaltfläche** enthält zwei Instanzen von `ToggleButton`, einschließlich des visuellen Zustands-Manager-Markups, mit dem die `Text`, `BackgroundColor`und `TextColor` der Schaltfläche auf der Grundlage des visuellen Zustands festgelegt werden:

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

Die `Toggled` Ereignishandler befinden sich in der Code Behind-Datei. Sie sind dafür verantwortlich, die `FontAttributes`-Eigenschaft des `Label` basierend auf dem Zustand der Schaltflächen festzulegen:

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

So sieht das Programm ausgeführt wird, auf iOS-, Android- und UWP aus:

[![Demo zum Umschalten der Schaltfläche](button-images/ToggleButtonDemo.png "Demo zum Umschalten der Schaltfläche")](button-images/ToggleButtonDemo-Large.png#lightbox)

<a name="image-button" />

## <a name="using-bitmaps-with-buttons"></a>Mithilfe von Bitmaps mit Schaltflächen

Die `Button`-Klasse definiert eine [`ImageSource`](xref:Xamarin.Forms.Button.Image) -Eigenschaft, mit der Sie ein Bitmap-Bild auf dem `Button`anzeigen können (entweder allein oder in Kombination mit Text). Sie können auch angeben, wie dem Text und Bild angeordnet sind.

Die `ImageSource`-Eigenschaft ist vom Typ [`ImageSource`](xref:Xamarin.Forms.ImageSource). Dies bedeutet, dass die Bitmaps aus einer Datei, einer eingebetteten Ressource, einem URI oder einem Stream geladen werden können.

> [!NOTE]
> Während ein `Button` ein animiertes GIF laden kann, wird nur der erste Frame der gif angezeigt.

Jeder von Xamarin.Forms unterstützte Plattform können Images in mehreren Größen für unterschiedliche Pixel-Lösungen von verschiedenen Geräten gespeichert werden, die auf die Anwendung möglicherweise ausgeführt. Diese, die mehrerer Bitmaps werden mit dem Namen oder so, dass das Betriebssystem die beste Übereinstimmung auswählen kann, für das Gerät Bild des gespeicherten, Bildschirmauflösung.

Bei einer Bitmap auf einem `Button`liegt die beste Größe normalerweise zwischen 32 und 64 geräteunabhängigen Einheiten, je nachdem, wie groß Sie sein sollen. Die in diesem Beispiel verwendeten Images basieren auf einer Größe von 48 geräteunabhängige Einheiten.

Im IOS-Projekt enthält der Ordner " **Resources** " drei Größen dieses Bilds:

- Eine quadratische Bitmap mit 48 Pixel, die als **/Resources/MonkeyFace.png** gespeichert ist
- Eine quadratische Bitmap mit 96 Pixel, die als **/Resource/MonkeyFace@2x.png** gespeichert ist
- Eine quadratische Bitmap mit 144 Pixel, die als **/Resource/MonkeyFace@3x.png** gespeichert ist

Alle drei Bitmaps erhielten eine **Buildaktion** von **bundleresource**.

Für das Android-Projekt haben die Bitmaps alle denselben Namen, aber Sie werden in verschiedenen Unterordnern des Ordners " **Resources** " gespeichert:

- Eine quadratische Bitmap mit 72 Pixel, die als **/Resources/drawable-hdpi/MonkeyFace.png** gespeichert ist
- Eine quadratische Bitmap mit 96 Pixel, die als **/Resources/drawable-xhdpi/MonkeyFace.png** gespeichert ist
- Eine quadratische Bitmap mit 144 Pixel, die als **/Resources/drawable-xxhdpi/MonkeyFace.png** gespeichert ist
- Eine quadratische Bitmap mit 192 Pixel, die als **/Resources/drawable-xxxhdpi/MonkeyFace.png** gespeichert ist

Diese **wurden als** Buildvorgang von " **androidresource**" festgestellt.

Im UWP-Projekt können Bitmaps an einer beliebigen Stelle im Projekt gespeichert werden, Sie werden jedoch im Allgemeinen in einem benutzerdefinierten Ordner oder dem vorhandenen **Assets** -Ordner gespeichert. Das UWP-Projekt enthält diese Bitmaps:

- Eine quadratische Bitmap mit 48 Pixel, die als **/Assets/MonkeyFace.Scale-100.png** gespeichert ist
- Eine quadratische Bitmap mit 96 Pixel, die als **/Assets/MonkeyFace.Scale-200.png** gespeichert ist
- Eine quadratische Bitmap mit 192 Pixel, die als **/Assets/MonkeyFace.Scale-400.png** gespeichert ist

Sie wurden alle eine **Buildaktion** von **Inhalten**erhalten.

Mithilfe der [`ContentLayout`](xref:Xamarin.Forms.Button.ContentLayout) -Eigenschaft von `Button`können Sie angeben, wie die Eigenschaften `Text` und `ImageSource` auf dem `Button` angeordnet werden. Diese Eigenschaft hat den Typ [`ButtonContentLayout`](xref:Xamarin.Forms.Button.ButtonContentLayout), bei dem es sich um eine eingebettete Klasse in `Button`handelt. Der [Konstruktor](xref:Xamarin.Forms.Button.ButtonContentLayout.%23ctor(Xamarin.Forms.Button.ButtonContentLayout.ImagePosition,System.Double)) verfügt über zwei Argumente:

- Ein Member der [`ImagePosition`](xref:Xamarin.Forms.Button.ButtonContentLayout.ImagePosition) Enumeration: `Left`, `Top`, `Right`oder `Bottom`, der angibt, wie die Bitmap relativ zum Text angezeigt wird.
- Ein `double` Wert für den Abstand zwischen der Bitmap und dem Text.

Die Standardwerte sind `Left` und 10 Einheiten. Zwei schreibgeschützte Eigenschaften von `ButtonContentLayout` mit dem Namen [`Position`](xref:Xamarin.Forms.Button.ButtonContentLayout.Position) und [`Spacing`](xref:Xamarin.Forms.Button.ButtonContentLayout.Spacing) die Werte dieser Eigenschaften bereitstellen.

Im Code können Sie eine `Button` erstellen und die `ContentLayout`-Eigenschaft wie folgt festlegen:

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

In XAML müssen Sie nur den Enumerationsmember oder den Abstand angeben, oder beides in beliebiger Reihenfolge durch Kommas getrennt:

```xaml
<Button Text="button text"
        ImageSource="image filename"
        ContentLayout="Right, 20" />
```

Die **Demo Seite Bild Schaltfläche** verwendet `OnPlatform`, um unterschiedliche Dateinamen für die Bitmapdateien der IOS-, Android-und UWP-Dateien anzugeben. Wenn Sie den gleichen Dateinamen für jede Plattform verwenden und die Verwendung von `OnPlatform`vermeiden möchten, müssen Sie die UWP-Bitmaps im Stammverzeichnis des Projekts speichern.

Auf der ersten `Button` auf der **Bild Schaltfläche "Bild Schaltfläche** " wird die Eigenschaft `Image` festgelegt, aber nicht die Eigenschaft `Text`:

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

Wenn die Bitmaps für UWP im Stammverzeichnis des Projekts gespeichert sind, kann dieses Markup erheblich vereinfacht werden:

```xaml
<Button ImageSource="MonkeyFace.png" />
```

Um in der **imagebuttondemo. XAML** -Datei eine Menge wiederholter Markup zu vermeiden, wird auch ein implizites `Style` definiert, um die `ImageSource`-Eigenschaft festzulegen. Diese `Style` wird automatisch auf fünf andere `Button` Elemente angewendet. Hier ist die vollständige XAML-Datei ein:

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

Die letzten vier `Button` Elemente verwenden die `ContentLayout`-Eigenschaft, um eine Position und einen Abstand von Text und Bitmap anzugeben:

[![Demo für Bild Schaltfläche](button-images/ImageButtonDemo.png "Demo für Bild Schaltfläche")](button-images/ImageButtonDemo-Large.png#lightbox)

Sie haben nun die verschiedenen Möglichkeiten gesehen, `Button` Ereignisse zu behandeln und die `Button` Darstellung zu ändern.

## <a name="related-links"></a>Verwandte Links

- [Buttondemos-Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-buttondemos)
- [Button-API](xref:Xamarin.Forms.Button)
