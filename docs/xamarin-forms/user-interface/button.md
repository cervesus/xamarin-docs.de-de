---
title: Schaltfläche "Xamarin.Forms"
description: Die Schaltfläche mit der reagiert auf ein tippen oder klicken Sie auf, die eine Anwendung, um eine bestimmte Aufgabe durchzuführen weiterleitet.
ms.prod: xamarin
ms.assetid: 62CAEB63-0800-44F4-9B8C-EE632138C2F5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/19/2018
ms.openlocfilehash: 8c55fecc8605b8bb7312e658e5edf46008f6b6ce
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68651351"
---
# <a name="xamarinforms-button"></a>Schaltfläche "Xamarin.Forms"

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-buttondemos)

_Die Schaltfläche mit der reagiert auf ein tippen oder klicken Sie auf, die eine Anwendung, um eine bestimmte Aufgabe durchzuführen weiterleitet._

Die [ `Button` ](xref:Xamarin.Forms.Button) ist das wichtigste über interaktive Steuerung aller Xamarin.Forms. Die `Button` werden angezeigt, die eine kurze Textzeichenfolge, der angibt, einen Befehl, aber es auch kann in der Regel angezeigt wird, ein Bitmap-Bild oder eine Kombination aus Text und ein Bild. Der Benutzer drückt die `Button` mit einem Finger oder darauf klickt, mit der Maus auf diesen Befehl zu initiieren.

Die meisten der nachstehend erläuterten Themen entsprechen zu Seiten in der [ **ButtonDemos** ](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-buttondemos) Beispiel.

## <a name="handling-button-clicks"></a>Schaltfläche "klickt

`Button` definiert eine [ `Clicked` ](xref:Xamarin.Forms.Button.Clicked) Ereignis, das ausgelöst wird, wenn der Benutzer tippt der `Button` mit einem Finger oder der Maus-Zeiger. Das Ereignis wird ausgelöst, wenn der Finger oder der Maus über die Oberfläche des losgelassen wird, die `Button`. Die `Button` müssen die [ `IsEnabled` ](xref:Xamarin.Forms.VisualElement.IsEnabled) -Eigenschaftensatz auf `true` dafür auf tippen zu reagieren.

Die **einfachen Klick mit der** auf der Seite die [ **ButtonDemos** ](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-buttondemos) Beispiel veranschaulicht, wie zum Instanziieren einer `Button` in XAML und behandeln die `Clicked` Ereignis. Die **BasicButtonClickPage.xaml** -Datei enthält eine `StackLayout` mit einer `Label` und `Button`:

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

Die `Button` tendenziell der gesamte Speicherplatz belegen, die für diese zulässig ist. Z. B., wenn Sie nicht Festlegen der `HorizontalOptions` Eigenschaft `Button` auf einen anderen Wert als `Fill`, `Button` die volle Breite seines übergeordneten Elements belegt.

Standardmäßig die `Button` rechteckig, aber Sie können die It abgerundete Ecken erteilen, mit der [ `CornerRadius` ](xref:Xamarin.Forms.Button.CornerRadius) -Eigenschaft, wie im Abschnitt unten beschrieben [ **Schaltfläche Darstellung** ](#button-appearance).

Die [ `Text` ](xref:Xamarin.Forms.Button.Text) Eigenschaft gibt an, der Text erscheint in der `Button`. Die [ `Clicked` ](xref:Xamarin.Forms.Button.Clicked) Ereignis festgelegt ist, um einen Ereignishandler namens `OnButtonClicked`. Dieser Handler befindet sich in der CodeBehind-Datei **BasicButtonClickPage.xaml.cs**:

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

Wenn die `Button` getippt wird, die `OnButtonClicked` Methode ausgeführt wird. Die `sender` Argument ist der `Button` Objekt für dieses Ereignis verantwortlich. Hiermit können Sie den Zugriff auf die `Button` Objekt oder zur Unterscheidung zwischen mehreren `Button` Objekte, die gemeinsame Verwendung desselben `Clicked` Ereignis.

Dieser spezielle `Clicked` Handler Ruft eine Animationsfunktion, die dreht das `Label` 360 Grad in 1000 Millisekunden. So sieht das Programm ausgeführt wird, auf IOS- und Android-Geräte und als universelle Windows-Plattform (UWP)-Anwendung auf dem Windows 10-Desktop aus:

[![Grundlegende Schaltflächenklick](button-images/BasicButtonClick.png "einfachen Klick auf eine Schaltfläche")](button-images/BasicButtonClick-Large.png#lightbox "einfachen Klick auf die Schaltfläche")

Beachten Sie, dass die `OnButtonClicked` Methode enthält die `async` Modifizierer da `await` wird innerhalb des ereignishandlers verwendet. Ein `Clicked` Ereignishandler erfordert die `async` Modifizierer nur, wenn der Text des Handlers verwendet `await`.

Jede Plattform rendert die `Button` in eine eigene bestimmte Art und Weise. In der [ **Schaltfläche Darstellung** ](#button-appearance) Abschnitt erfahren Sie, wie Farben und die `Button` Rahmen bei stärker angepassten Darstellungen angezeigt. `Button` implementiert die [ `IFontElement` ](xref:Xamarin.Forms.Internals.IFontElement) Schnittstelle, damit er enthält [ `FontFamily` ](xref:Xamarin.Forms.Button.FontFamily), [ `FontSize` ](xref:Xamarin.Forms.Button.FontSize), und [ `FontAttributes` ](xref:Xamarin.Forms.Button.FontAttributes)Eigenschaften.

## <a name="creating-a-button-in-code"></a>Erstellen eine Schaltfläche in code

Es ist üblich, instanziieren Sie ein `Button` in XAML, aber Sie können auch erstellen, eine `Button` im Code. Dies kann zwar zweckmäßig sein, wenn Ihre Anwendung erstellen Sie mehrere Schaltflächen, die basierend auf Daten, die mit aufzählbar ist, muss ein `foreach` Schleife.

Die **Klick mit der Code** Seite erläutert, wie eine Seite erstellen, die funktionell gleichwertig mit der **einfachen Klick mit der** Seite jedoch vollständig in C#:

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

Alles wird in den Konstruktor der Klasse ausgeführt. Da die `Clicked` Handler ist nur eine Anweisung, die lange, kann Sie sehr einfach auf das Ereignis angefügt werden:

```csharp
button.Clicked += async (sender, args) => await label.RelRotateTo(360, 1000);
```

Natürlich können Sie den Ereignishandler als eine separate Methode definieren (wie die `OnButtonClick` -Methode in der **einfachen Klick mit der**), und fügen Sie diese Methode an das Ereignis:

```csharp
button.Clicked += OnButtonClicked;
```

## <a name="disabling-the-button"></a>Deaktivieren der Schaltfläche

Manchmal ist eine Anwendung einen bestimmten Zustand, bei einer bestimmten `Button` auf ist kein gültiger Vorgang. In diesen Fällen die `Button` sollte deaktiviert werden, indem die `IsEnabled` Eigenschaft `false`. Das klassische Beispiel ist ein `Entry` Steuerelement für einen Dateinamen, der mit einer `Button`Datei geöffnet ist: Sollte nur aktiviert werden, wenn ein Text in das `Entry`eingegeben wurde. `Button`
Können Sie eine `DataTrigger` für diesen Task, siehe die [ **Datentrigger** ](~/xamarin-forms/app-fundamentals/triggers.md#data-triggers) Artikel.

## <a name="using-the-command-interface"></a>Verwenden die Befehlsschnittstelle

Es ist möglich, dass eine Anwendung auf reagieren `Button` Taps ohne Behandlung der `Clicked` Ereignis. Die `Button` implementiert eine alternative Benachrichtigungsmechanismus, der Namen der _Befehl_ oder _Befehle_ Schnittstelle. Dies besteht aus zwei Eigenschaften:

- [`Command`](xref:Xamarin.Forms.Button.Command) Der Typ [ `ICommand` ](xref:System.Windows.Input.ICommand), eine Schnittstelle definiert, der [ `System.Windows.Input` ](xref:System.Windows.Input) Namespace.
- [`CommandParameter`](xref:Xamarin.Forms.Button.CommandParameter) Eigenschaft des Typs [ `Object` ](xref:System.Object).

Dieser Ansatz eignet sich besonders im Zusammenhang mit Datenbindung und insbesondere dann, wenn die Model-View-ViewModel (MVVM)-Architektur zu implementieren. In diesen Themen werden in den Artikeln erläutert [Datenbindung](~/xamarin-forms/app-fundamentals/data-binding/index.md), [von Datenbindungen zu MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md), und [MVVM](~/xamarin-forms/enterprise-application-patterns/mvvm.md).

In einer MVVM-Anwendung, definiert das "ViewModel" Eigenschaften des Typs `ICommand` klicken Sie dann das XAML angeschlossenen `Button` Elemente mit datenbindungen. Xamarin.Forms definiert auch [ `Command` ]((xref:Xamarin.Forms.Command)) und [ `Command<T>` ](xref:Xamarin.Forms.Command`1) Klassen, in denen die `ICommand` Schnittstelle, und bei der Definition von Eigenschaften des Typs "ViewModel"`ICommand`.

Befehle in diesem Artikel ausführlicher beschrieben wird [ **die Befehlsschnittstelle** ](~/xamarin-forms/app-fundamentals/data-binding/commanding.md) jedoch **grundlegende Schaltflächenbefehl** auf der Seite die [  **ButtonDemos** ](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-buttondemos) Beispiel zeigt die grundlegende Vorgehensweise.

Die `CommandDemoViewModel` -Klasse ist ein sehr einfaches "ViewModel", die eine Eigenschaft des Typs definiert `double` mit dem Namen `Number`, und zwei Eigenschaften vom Typ `ICommand` mit dem Namen `MultiplyBy2Command` und `DivideBy2Command`:

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

Die beiden `ICommand` Eigenschaften werden in den Konstruktor der Klasse mit zwei Objekte des Typs initialisiert `Command`. Die `Command` Konstruktoren sind eine nützliche Funktion (bezeichnet das `execute` Konstruktorargument), verdoppelt oder halbiert der `Number` Eigenschaft.

Die **BasicButtonCommand.xaml** legt die `BindingContext` mit einer Instanz von `CommandDemoViewModel`. Die `Label` -Element und zwei `Button` Elemente enthalten Bindungen in den drei Eigenschaften im `CommandDemoViewModel`:

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

Wie die beiden `Button` Elemente abgerufen werden, die Befehle ausgeführt werden, und die Anzahl ändert, Wert:

[![Grundlegende Schaltflächenbefehl](button-images/BasicButtonCommand.png "grundlegende Schaltflächenbefehl")](button-images/BasicButtonCommand-Large.png#lightbox)

Der Vorteil dieses Ansatzes über `Clicked` Handler ist, dass die gesamte Logik, die im Zusammenhang mit der Funktionalität von dieser Seite in "ViewModel" anstatt den Code-Behind-Datei befindet, erreichen eine bessere Trennung der Benutzeroberfläche von der Geschäftslogik.

Es ist auch möglich, für die `Command` zu steuern, das Aktivieren und Deaktivieren der Objekte die `Button` Elemente. Nehmen wir beispielsweise an, die Sie den Bereich der numerischen Werte zwischen 2 beschränken möchten<sup>10</sup> und 2<sup>&ndash;10</sup>. Sie können eine andere Funktion hinzufügen, an den Konstruktor (wird aufgerufen, die `canExecute` Argument) zurückgibt `true` Wenn die `Button` aktiviert werden soll. Hier ist die Änderung der `CommandDemoViewModel` Konstruktor:

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

Die Aufrufe an die `ChangeCanExecute` -Methode der `Command` sind erforderlich, damit die `Command` Methode aufrufen kann die `canExecute` Methode und bestimmen Sie, ob der `Button` deaktiviert werden sollen, oder nicht. Mit dieser Änderung des Codes, wenn die Anzahl die Grenze dessen erreicht, die `Button` deaktiviert ist:

[![Schaltfläche "Standard"-Befehl – geändert](button-images/BasicButtonCommandModified.png "grundlegende Schaltflächenbefehl - geändert")](button-images/BasicButtonCommandModified-Large.png#lightbox)

Es ist möglich, dass zwei oder mehr `Button` Elemente, die auf die gleiche gebunden werden `ICommand` Eigenschaft. Die `Button` Elemente können unterschieden werden, mit der [ `CommandParameter` ](xref:Xamarin.Forms.Button.CommandParameter) Eigenschaft `Button`. In diesem Fall sollten Sie die generischen [ `Command<T>` ](xref:Xamarin.Forms.Command`1) Klasse. Die `CommandParameter` Objekt wird dann als Argument für das Übergeben der `execute` und `canExecute` Methoden. Dieses Verfahren ist ausführlich gezeigt die [ **grundlegende Befehle** ](~/xamarin-forms/app-fundamentals/data-binding/commanding.md#basic-commanding) Teil der [ **Befehlsschnittstelle** ](~/xamarin-forms/app-fundamentals/data-binding/commanding.md#basic-commanding) Artikel.

Die [ **ButtonDemos** ](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-buttondemos) Beispiel verwendet auch diese Technik in seine `MainPage` Klasse. Die **"MainPage.xaml"** -Datei enthält eine `Button` für jede Seite des Beispiels:

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

Jede `Button` hat seine `Command` -Eigenschaft gebunden werden, um eine Eigenschaft namens `NavigateCommand`, und die `CommandParameter` nastaven NA hodnotu eine [ `Type` ](xref:System.Type) Objekt entspricht, der auf einen der Page-Klassen im Projekt.

Dass `NavigateCommand` Eigenschaft ist vom Typ `ICommand` und im Code-Behind-Datei definiert ist:

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

Der Konstruktor initialisiert die `NavigateCommand` Eigenschaft, um eine `Command<Type>` Objekt, da der `Type` ist der Typ des der `CommandParameter` Objekt in der XAML-Datei festgelegt. Dies bedeutet, dass die `execute` Methode hat ein Argument des Typs `Type` , entspricht dies `CommandParameter` Objekt. Die Funktion die Seite instanziiert und wechselt anschließend darauf.

Beachten Sie, die der Konstruktor wird, durch Festlegen abgeschlossen seiner `BindingContext` auf sich selbst. Dies ist notwendig für Eigenschaften in der XAML-Datei zum Binden an die `NavigateCommand` Eigenschaft.

## <a name="pressing-and-releasing-the-button"></a>Drücken und Loslassen der Taste

Neben der `Clicked` Ereignis `Button` definiert auch [ `Pressed` ](xref:Xamarin.Forms.Button.Pressed) und [ `Released` ](xref:Xamarin.Forms.Button.Released) Ereignisse. Die `Pressed` Ereignis tritt auf, wenn ein Finger auf drückt eine `Button`, oder eine Maustaste gedrückt wird mit dem Mauszeiger auf positioniert die `Button`. Die `Released` Ereignis tritt auf, wenn die Finger oder der Maustaste losgelassen wird. Im Allgemeinen eine `Clicked` Ereignis wird auch ausgelöst, zur gleichen Zeit wie die `Released` -Ereignis, aber die Folien Wenn des Finger oder der Maus Zeigers von der Oberfläche der `Button` freigegeben wird, bevor Sie die `Clicked` Ereignis erfolgen möglicherweise keine.

Die `Pressed` und `Released` Ereignisse werden nicht oft verwendet, aber sie können für besondere Zwecke verwendet werden, wie in der **drücken und die Schaltfläche "Release"** Seite. Die XAML-Datei enthält eine `Label` und `Button` mit angefügte Handler für die `Pressed` und `Released` Ereignisse:

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

Code-Behind-Datei erstellt eine Animation die `Label` beim eine `Pressed` Ereignis auftritt, aber es hält die Drehung bei einer `Released` Ereignis tritt auf:

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

Das Ergebnis ist, die `Label` nur dreht, während es sich bei ein Finger in Kontakt stehen ist die `Button`, und endet, wenn der Finger veröffentlicht wird:

[![Drücken Sie die EINGABETASTE, und lassen Sie los](button-images/PressAndReleaseButton.png "drücken und lassen Sie los")](button-images/PressAndReleaseButton-Large.png)

Diese Art von Verhalten hat Anwendungen für Spiele: Ein auf einem `Button` gehaltene Finger kann eine auf dem Bildschirm liegende Objekt in eine bestimmte Richtung verschieben.

<a name="button-appearance" />

## <a name="button-appearance"></a>Darstellung der Schaltfläche

Die `Button` erbt oder definiert mehrere Eigenschaften, die die Darstellung auswirken:

- [`TextColor`](xref:Xamarin.Forms.Button.TextColor) entspricht der Farbe der `Button` Text
- [`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor) ist die Farbe des Hintergrunds auf den ausgewählten text
- [`BorderColor`](xref:Xamarin.Forms.Button.BorderColor) entspricht der Farbe ein Bereich, umgibt die `Button`
- [`FontFamily`](xref:Xamarin.Forms.Button.FontFamily) die Schriftfamilie ist für das Textfeld verwendet werden
- [`FontSize`](xref:Xamarin.Forms.Button.FontSize) ist die Größe des Texts
- [`FontAttributes`](xref:Xamarin.Forms.Button.FontAttributes) Gibt an, ob der Text kursiv und fett ist.
- [`BorderWidth`](xref:Xamarin.Forms.Button.BorderWidth) ist die Breite des Rahmens
- [`CornerRadius`](xref:Xamarin.Forms.Button.CornerRadius) ist der Eckradius eines der `Button`

> [!NOTE]
> Die `Button` -Klasse verfügt auch über [ `Margin` ](xref:Xamarin.Forms.View.Margin) und [ `Padding` ](xref:Xamarin.Forms.Button.Padding) Eigenschaften zum Steuern der Layoutverhalten der `Button`. Weitere Informationen finden Sie unter [Ränder und Abstände](~/xamarin-forms/user-interface/layouts/margin-and-padding.md).

Die Auswirkungen von sechs dieser Eigenschaften (mit Ausnahme `FontFamily` und `FontAttributes`) werden veranschaulicht die **Darstellung Schaltfläche** Seite. Eine andere Eigenschaft, [ `Image` ](xref:Xamarin.Forms.Button.ImageSource), wird im Abschnitt erläuterten [ **mithilfe von Bitmaps mit der Schaltfläche**](#image-button).

Alle Ansichten und Bindungen in der **Darstellung Schaltfläche** Seite in der XAML-Datei definiert sind:

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

Die `Button` am oberen Rand der Seite hat drei `Color` Eigenschaften gebunden, um `Picker` Elemente am unteren Rand der Seite. Die Elemente in der `Picker` Elemente sind Farben aus der `NamedColor` Klasse im Projekt enthalten ist. Drei `Slider` Elemente enthalten, bidirektionale Bindungen, die `FontSize`, `BorderWidth`, und `CornerRadius` Eigenschaften der `Button`.

Dieses Programm ermöglicht Ihnen das Experimentieren mit Kombinationen von all diese Eigenschaften:

[![Schaltfläche Darstellung](button-images/ButtonAppearance.png "Schaltfläche Darstellung")](button-images/ButtonAppearance-Large.png)

Um finden Sie unter der `Button` Rahmen, Sie müssen eine `BorderColor` auf einen anderen Wert als `Default`, und die `BorderWidth` auf einen positiven Wert.

Unter iOS, sehen Sie, dass große Rahmenbreiten in das Innere des Eindringen der `Button` und die Anzeige von Text in Konflikt geraten. Wenn Sie einen Rahmen mit einem iOS-verwenden `Button`, Sie sollten beginnen und enden die `Text` Eigenschaft Leerzeichen beibehalten die Sichtbarkeit.

Auf UWP Auswählen einer `CornerRadius` , die Hälfte die Höhe des überschreitet die `Button` löst eine Ausnahme aus.

## <a name="button-visual-states"></a>Visuelle Zustände der Schaltfläche

[`Button`](xref:Xamarin.Forms.Button) verfügt über eine `Pressed` [ `VisualState` ](xref:Xamarin.Forms.VisualState) , die verwendet werden kann, initiieren Sie eine visuelle Änderung an der `Button` beim Klicken auf vom Benutzer aufgerufen werden, vorausgesetzt, dass es aktiviert ist.

Im folgende XAML-Beispiel zeigt, wie definieren Sie einen visuellen Zustand für die `Pressed` Zustand:

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

Die `Pressed` [ `VisualState` ](xref:Xamarin.Forms.VisualState) gibt an, dass die [ `Button` ](xref:Xamarin.Forms.Button) befindet, gedrückt wird seine [ `Scale` ](xref:Xamarin.Forms.VisualElement.Scale) eigenschaftsänderungen werden von der Der Standardwert von 1 auf 0,8. Die `Normal` `VisualState` gibt an, dass die `Button` befindet sich in einem normalen Status, seine `Scale` Eigenschaft wird auf 1 festgelegt werden. Aus diesem Grund der Gesamteffekt besteht, die bei der `Button` ist gedrückt, sie entsprechend neu skaliert etwas kleiner, und wenn die `Button` wird veröffentlicht, wird es auf der Standardgröße angepasst.

Weitere Informationen zu visuellen Status finden Sie unter [die Xamarin.Forms Visual State Manager](~/xamarin-forms/user-interface/visual-state-manager.md).

## <a name="creating-a-toggle-button"></a>Erstellen eine Umschaltfläche

Die Unterklasse `Button` ist möglich, sodass Sie wie ein on-off-Switch funktioniert: Tippen Sie einmal auf die Schaltfläche, um die Schaltfläche ein-und auszuschalten, und tippen Sie erneut, um Sie zu deaktivieren.

Die folgenden `ToggleButton` Klasse leitet sich von `Button` und definiert ein neues Ereignis mit dem Namen `Toggled` und eine boolesche Eigenschaft namens `IsToggled`. Hierbei handelt es sich um die gleichen zwei Eigenschaften, die durch die Xamarin.Forms definiert [ `Switch` ](xref:Xamarin.Forms.Switch):

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

Die `ToggleButton` Konstruktor fügt einen Handler, der die `Clicked` Ereignis so, dass die It den Wert ändern, kann die `IsToggled` Eigenschaft. Die `OnIsToggledChanged` Methode löst die `Toggled` Ereignis.

Die letzte Zeile der `OnIsToggledChanged` Methode ruft die statische `VisualStateManager.GoToState` Methode mit dem Text der beiden Zeichenfolgen "ToggledOn" und "ToggledOff". Erhalten Sie Informationen zu dieser Methode und wie Ihre Anwendung auf visuelle Zustände in diesem Artikel reagieren kann [ **die Xamarin.Forms Visual State Manager**](~/xamarin-forms/user-interface/visual-state-manager.md).

Da `ToggleButton` führt den Aufruf für `VisualStateManager.GoToState`, die Klasse selbst nicht unbedingt enthalten zusätzlichen Einrichtungen so ändern Sie die Darstellung der Schaltfläche auf der Grundlage der `IsToggled` Zustand. D. h. die Verantwortung für die XAML, die hostet die `ToggleButton`.

Die **ein-/ausschalten Schaltflächendemo** Seite enthält zwei Instanzen von `ToggleButton`, einschließlich Markup Visual State-Manager, der festlegt der `Text`, `BackgroundColor`, und `TextColor` der Schaltfläche basierend auf den visuellen Zustand:

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

Die `Toggled` Ereignishandler sind in der CodeBehind-Datei. Sie sind zuständig für die Einstellung der `FontAttributes` Eigenschaft der `Label` abhängig vom Status der Schaltflächen:

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

[![Wechseln Sie Schaltflächendemo](button-images/ToggleButtonDemo.png "Schaltflächendemo umschalten")](button-images/ToggleButtonDemo-Large.png#lightbox)

<a name="image-button" />

## <a name="using-bitmaps-with-buttons"></a>Mithilfe von Bitmaps mit Schaltflächen

Die `Button` -Klasse definiert ein [ `ImageSource` ](xref:Xamarin.Forms.Button.Image) -Eigenschaft, die Ihnen ermöglicht, die auf ein Bitmapbild Anzeigen der `Button`, entweder allein oder in Kombination mit dem Text. Sie können auch angeben, wie dem Text und Bild angeordnet sind.

Die `ImageSource` -Eigenschaft ist vom [`ImageSource`](xref:Xamarin.Forms.ImageSource)Typ. Dies bedeutet, dass die Bitmaps aus einer Datei, einer eingebetteten Ressource, einem URI oder einem Stream geladen werden können.

Jeder von Xamarin.Forms unterstützte Plattform können Images in mehreren Größen für unterschiedliche Pixel-Lösungen von verschiedenen Geräten gespeichert werden, die auf die Anwendung möglicherweise ausgeführt. Diese, die mehrerer Bitmaps werden mit dem Namen oder so, dass das Betriebssystem die beste Übereinstimmung auswählen kann, für das Gerät Bild des gespeicherten, Bildschirmauflösung.

Für eine Bitmap für einen `Button`, die optimale Größe ist in der Regel zwischen 32 und 64 geräteunabhängige Einheiten, je nachdem, wie groß Sie ihn haben möchten. Die in diesem Beispiel verwendeten Images basieren auf einer Größe von 48 geräteunabhängige Einheiten.

In der iOS-Projekt das **Ressourcen** Ordner enthält drei Größen dieses Bilds:

- Eine quadratische 48 Pixel-Bitmap, die als gespeicherte **/Resources/MonkeyFace.png**
- Eine quadratische 96 Pixel-Bitmap, die als gespeichert **/Resource/MonkeyFace@2x.png**
- Eine quadratische 144-Pixel-Bitmap, die als gespeichert **/Resource/MonkeyFace@3x.png**

Es wurden alle drei Bitmaps übergeben, eine **Buildvorgang** von **BundleResource**.

Für das Android-Projekt die Bitmaps, die alle den gleichen Namen aufweisen, aber sie werden in verschiedenen Unterordnern gespeichert der **Ressourcen** Ordner:

- Eine quadratische 72 Pixel-Bitmap, die als gespeicherte **/Resources/drawable-hdpi/MonkeyFace.png**
- Eine quadratische 96 Pixel-Bitmap, die als gespeicherte **/Resources/drawable-xhdpi/MonkeyFace.png**
- Eine quadratische 144-Pixel-Bitmap, die als gespeicherte **/Resources/drawable-xxhdpi/MonkeyFace.png**
- Eine quadratische 192-Pixel-Bitmap, die als gespeicherte **/Resources/drawable-xxxhdpi/MonkeyFace.png**

Diese wurden übergeben, eine **Buildvorgang** von **AndroidResource**.

In der UWP-Projekt Bitmaps können gespeichert werden an einer beliebigen Stelle im Projekt, aber sie werden in der Regel in einem benutzerdefinierten Ordner gespeichert oder **Assets** vorhandenen Ordner. Das UWP-Projekt enthält diese Bitmaps:

- Eine quadratische 48 Pixel-Bitmap, die als gespeicherte **/Assets/MonkeyFace.scale-100.png**
- Eine quadratische 96 Pixel-Bitmap, die als gespeicherte **/Assets/MonkeyFace.scale-200.png**
- Eine quadratische 192-Pixel-Bitmap, die als gespeicherte **/Assets/MonkeyFace.scale-400.png**

Sie wurden alle übergeben eine **Buildvorgang** von **Content**.

Können Sie angeben, wie die `Text` und `ImageSource` Eigenschaften angeordnet sind, auf die `Button` mithilfe der [ `ContentLayout` ](xref:Xamarin.Forms.Button.ContentLayout) Eigenschaft `Button`. Diese Eigenschaft ist vom Typ [ `ButtonContentLayout` ](xref:Xamarin.Forms.Button.ButtonContentLayout), dies ist in eine eingebettete Klasse `Button`. Die [Konstruktor](xref:Xamarin.Forms.Button.ButtonContentLayout.%23ctor(Xamarin.Forms.Button.ButtonContentLayout.ImagePosition,System.Double)) weist zwei Argumente:

- Ein Mitglied der [ `ImagePosition` ](xref:Xamarin.Forms.Button.ButtonContentLayout.ImagePosition) Enumeration: `Left`, `Top`, `Right`, oder `Bottom` , der angibt, wie die Bitmap relativ zum Text angezeigt wird.
- Ein `double` Wert für den Abstand zwischen der Bitmap und dem Text.

Die Standardwerte sind `Left` und 10 Einheiten. Zwei schreibgeschützte Eigenschaften des `ButtonContentLayout` mit dem Namen [ `Position` ](xref:Xamarin.Forms.Button.ButtonContentLayout.Position) und [ `Spacing` ](xref:Xamarin.Forms.Button.ButtonContentLayout.Spacing) Geben Sie die Werte dieser Eigenschaften.

Erstellen Sie im Code eine `Button` und legen Sie die `ContentLayout` Eigenschaft wie folgt:

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

Die **Image Schaltflächendemo** verwendet `OnPlatform` einen anderen Dateinamen an, für die iOS-, Android- und UWP-Dateien Bitmap. Wenn Sie verwenden möchten, verwenden den gleichen Dateinamen für jede Plattform und vermeiden Sie die Verwendung von `OnPlatform`, Sie müssen die Bitmaps für UWP im Stammverzeichnis des Projekts zu speichern.

Die erste `Button` auf die **Image Schaltflächendemo** Seite legt die `Image` Eigenschaft jedoch nicht die `Text` Eigenschaft:

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

Um viele wiederkehrende Markup zu vermeiden der **ImageButtonDemo.xaml** -Datei, eine implizite `Style` entsprechend definiert die `ImageSource` Eigenschaft. Dies `Style` wird automatisch auf anderen fünf angewendet `Button` Elemente. Hier ist die vollständige XAML-Datei ein:

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

Die letzten vier `Button` Elemente machen verwenden die `ContentLayout` Eigenschaft, um eine Position und Abstand von Text und Bitmap anzugeben:

[![Abbildung Schaltflächendemo](button-images/ImageButtonDemo.png "Schaltflächendemo Image")](button-images/ImageButtonDemo-Large.png#lightbox)

Sie haben nun gesehen, die verschiedenen Möglichkeiten, die Sie behandeln können `Button` Ereignissen und Änderungen der `Button` Darstellung.

## <a name="related-links"></a>Verwandte Links

- [ButtonDemos-Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-buttondemos)
- [Schaltfläche "API"](xref:Xamarin.Forms.Button)
