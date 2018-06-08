---
title: Xamarin.Forms-Schaltfläche
description: Die Schaltfläche "" reagiert auf eine tippen oder klicken Sie auf, die eine Anwendung zum Ausführen einer bestimmten Aufgabe weiterleitet.
ms.prod: xamarin
ms.assetid: 62CAEB63-0800-44F4-9B8C-EE632138C2F5
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 06/01/2018
ms.openlocfilehash: 1fed439ecb4bd79bd84974ea1397ca0ed1336b62
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/07/2018
ms.locfileid: "34847952"
---
# <a name="xamarinforms-button"></a>Xamarin.Forms-Schaltfläche

_Die Schaltfläche "" reagiert auf eine tippen oder klicken Sie auf, die eine Anwendung zum Ausführen einer bestimmten Aufgabe weiterleitet._ 

Die [ `Button` ](xref:Xamarin.Forms.Button) ist das grundlegendste interaktive-Steuerelement in allen Xamarin.Forms. Die `Button` zeigt eine kurze Textzeichenfolge, der angibt, einen Befehl, aber sie auch können in der Regel angezeigt wird, ein Bitmap-Bild oder eine Kombination von Text und ein Bild. Der Benutzer drückt die `Button` mit einem Finger oder darauf klickt mit der Maus auf den Befehl zu initiieren.

Die meisten der unten erläuterten Themen entsprechen Seiten in der [ **ButtonDemos** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ButtonDemos) Beispiel.

## <a name="handling-button-clicks"></a>Schaltfläche "klickt

`Button` definiert eine [ `Clicked` ](xref:Xamarin.Forms.Button.Clicked) Ereignis, das ausgelöst wird, wenn der Benutzer tippt der `Button` mit einem Finger oder mit dem Mauszeiger Zeiger. Das Ereignis wird ausgelöst, wenn die Finger oder mit dem Mauszeiger über die Oberfläche des losgelassen wird die `Button`. Die `Button` benötigen ihre [ `IsEnabled` ](xref:Xamarin.Forms.VisualElement.IsEnabled) -Eigenschaftensatz auf `true` dafür auf Klicks reagiert. 

Die **grundlegende Schaltfläche klicken,** auf der Seite der [ **ButtonDemos** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ButtonDemos) Beispiel zeigt, wie beim Instanziieren einer `Button` in XAML und das Handle seine `Clicked` Ereignis. Die **BasicButtonClickPage.xaml** -Datei enthält eine `StackLayout` mit einer `Label` und ein `Button`:

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

Die `Button` tendenziell der gesamte Speicherplatz belegen, die für sie zulässig ist. Z. B., wenn Sie nicht Festlegen der `HorizontalOptions` Eigenschaft `Button` auf einen anderen Wert als `Fill`, die `Button` beanspruchen, die gesamte Breite des übergeordneten Objekts.

Wird standardmäßig die `Button` rechteckig, ist, aber Sie können die It abgerundete Ecken erteilen, mit der [ `CornerRadius` ](xref:Xamarin.Forms.Button.CornerRadius) Eigenschaft, die im Abschnitt unten beschrieben [ **Schaltfläche Darstellung** ](#button-appearance).

Die [ `Text` ](xref:Xamarin.Forms.Button.Text) Eigenschaft gibt den angezeigten Text in der `Button`. Die [ `Clicked` ](xref:Xamarin.Forms.Button.Clicked) Ereignis festgelegt ist, um einen Ereignishandler namens `OnButtonClicked`. Mit diesem Handler befindet sich in der Code-Behind-Datei **BasicButtonClickPage.xaml.cs**:

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

Wenn die `Button` abgerufen wird, die `OnButtonClicked` Methode ausgeführt wird. Die `sender` Argument ist der `Button` Objekt für dieses Ereignis verantwortlich. Hiermit können Sie den Zugriff auf die `Button` -Objekt, oder zur Unterscheidung zwischen mehreren `Button` Objekte, die gemeinsame Nutzung der gleichen `Clicked` Ereignis.

Dieser spezielle `Clicked` Handler aufruft, eine Animationsfunktion, die dreht die `Label` 360 Grad in 1000 Millisekunden. Hier wird das Programm auf IOS- und Android-Geräte, und eine universelle Windows-Plattform (UWP)-Anwendung auf dem Windows 10 Desktop ausgeführt:

[![Grundlegende Schaltflächen-Klickereignis](button-images/BasicButtonClick.png "grundlegende Schaltflächen-Klickereignis")](button-images/BasicButtonClick-Large.png#lightbox "grundlegende Schaltflächen-Klickereignis")

Beachten Sie, dass die `OnButtonClicked` Methode enthält die `async` Modifizierer da `await` innerhalb der Ereignishandler verwendet wird. Ein `Clicked` Ereignishandler erfordert die `async` Modifizierer nur, wenn der Text des Handlers verwendet `await`.

Jede Plattform rendert die `Button` in einen eigenen bestimmten Weise. In der [ **Schaltfläche Darstellung** ](#button-appearance) Abschnitt sehen Sie das Festlegen von Farben, und stellen die `Button` Rahmen für stärker angepassten eindeutigkeitsmetrik sichtbar. `Button` implementiert die [ `IFontElement` ](xref:Xamarin.Forms.Internals.IFontElement) Schnittstelle, damit er enthält [ `FontFamily` ](xref:Xamarin.Forms.Button.FontFamily), [ `FontSize` ](xref:Xamarin.Forms.Button.FontSize), und [ `FontAttributes` ](xref:Xamarin.Forms.Button.FontAttributes)Eigenschaften.

## <a name="creating-a-button-in-code"></a>Erstellen eine Schaltfläche in code

Es ist üblich, instanziieren Sie ein `Button` in XAML, aber Sie können auch erstellen eine `Button` im Code. Dies kann zweckmäßig sein, wenn die Anwendung muss so erstellen mehrere Schaltflächen, die basierend auf Daten, die mit aufzählbar ist eine `foreach` Schleife.

Die **Code auf die Schaltfläche klicken** Seite veranschaulicht, wie eine Seite, die ist funktionell gleichwertig mit der **grundlegende klicken** Seite jedoch vollständig in c#:

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

Alles wird im Konstruktor der Klasse durchgeführt. Da die `Clicked` Handler ist nur eine Anweisung, die lange, können Sie sehr einfach an das Ereignis angehängt werden:

```csharp
button.Clicked += async (sender, args) => await label.RelRotateTo(360, 1000);
```

Natürlich können Sie den Ereignishandler als eine separate Methode definieren (ebenso wie die `OnButtonClick` Methode im **grundlegende klicken**), und fügen Sie diese Methode auf das Ereignis:

```csharp
button.Clicked += OnButtonClicked;
```

## <a name="disabling-the-button"></a>Deaktivieren die Schaltfläche ""

Manchmal ist eine Anwendung einen bestimmten Status, wobei ein bestimmter `Button` auf ist kein gültiger Vorgang. In diesen Fällen die `Button` sollte deaktiviert werden, indem seine `IsEnabled` Eigenschaft `false`. Das klassische Beispiel ist ein `Entry` -Steuerelement für einen Dateinamen, die zusammen mit einer geöffneten Datei `Button`: die `Button` sollten nur aktiviert, wenn Text in typisiert wurde die `Entry`. Können Sie eine `DataTrigger` für diese Aufgabe, die entsprechend der [ **Trigger Data** ](~/xamarin-forms/app-fundamentals/triggers.md#data-triggers) Artikel.

## <a name="using-the-command-interface"></a>Mithilfe der Befehls-Benutzeroberfläche

Es ist möglich, dass eine Anwendung so reagieren Sie auf `Button` Taps ohne Behandlung der `Clicked` Ereignis. Die `Button` implementiert eine alternative Benachrichtigungsmechanismus aufgerufen, die _Befehl_ oder _Befehle_ Schnittstelle. Dies umfasst zwei Eigenschaften:

- [`Command`](xref:Xamarin.Forms.Button.Command) Der Typ [ `ICommand` ](xref:System.Windows.Input.ICommand), eine Schnittstelle definiert, der [ `System.Windows.Input` ](xref:System.Windows.Input) Namespace.
- [`CommandParameter`](xref:Xamarin.Forms.Button.CommandParameter) Eigenschaft vom Typ [ `Object` ](xref:System.Object).

Dieser Ansatz eignet sich insbesondere im Zusammenhang mit Datenbindung und insbesondere dann, wenn die Model-View-ViewModel (MVVM)-Architektur implementiert. Diese Themen werden erörtert, in den Artikeln [Datenbindung](~/xamarin-forms/app-fundamentals/data-binding/index.md), [aus Datenbindungen, MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md), und [MVVM](~/xamarin-forms/enterprise-application-patterns/mvvm.md).

In einer Anwendung MVVM definiert das ViewModel Eigenschaften vom Typ `ICommand` , klicken Sie dann in der XAML verbunden sind `Button` Elemente mit datenbindungen. Xamarin.Forms definiert außerdem [ `Command` ]((xref:Xamarin.Forms.Command`1)) und [ `Command<T>` ](xref:Xamarin.Forms.Command`1) Klassen implementiert, die `ICommand` Schnittstelle, und bei der Definition von Eigenschaften des Typs ViewModel`ICommand`.

Befehle wird ausführlicher im Artikel beschrieben [ **die Befehlsschnittstelle** ](~/xamarin-forms/app-fundamentals/data-binding/commanding.md) aber die **grundlegende Schaltfläche Befehl** auf der Seite der [  **ButtonDemos** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ButtonDemos) Beispiel zeigt die grundlegende Vorgehensweise.

Die `CommandDemoViewModel` Klasse ist eine sehr einfache ViewModel, die eine Eigenschaft des Typs definiert `double` mit dem Namen `Number`, sowie zwei Eigenschaften vom Typ `ICommand` mit dem Namen `MultiplyBy2Command` und `DivideBy2Command`:

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

Die beiden `ICommand` Eigenschaften werden im Konstruktor der Klasse mit zwei Objekte des Typs initialisiert `Command`. Die `Command` Konstruktoren enthalten eine kleine Funktion (aufgerufen, die `execute` Konstruktorargument), verdoppelt oder halbiert die `Number` Eigenschaft.

Die **BasicButtonCommand.xaml** legt seine `BindingContext` mit einer Instanz von `CommandDemoViewModel`. Die `Label` Element und zwei `Button` Elemente enthalten, Bindungen, die drei Eigenschaften in `CommandDemoViewModel`:

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

Wie die beiden `Button` Elemente abgerufen werden, die Befehle werden ausgeführt, und die Anzahl ändert, Wert:

[![Schaltfläche "Standard"-Befehl](button-images/BasicButtonCommand.png "grundlegende Schaltfläche-Befehl")](button-images/BasicButtonCommand-Large.png#lightbox)

Der Vorteil dieses Ansatzes über `Clicked` Handler ist, dass die Logik, die im Zusammenhang mit der Funktionalität von dieser Seite in der Code-Behind-Datei, anstatt das ViewModel befindet erzielen eine bessere Trennung der Benutzeroberfläche von der Geschäftslogik.

Es ist auch möglich, für die `Command` -Objekten, das Aktivieren und Deaktivieren von steuern die `Button` Elemente. Angenommen, Sie möchten zum Begrenzen des Bereichs der numerischen Werte zwischen 2<sup>10</sup> und 2<sup>&ndash;10</sup>. Sie können eine andere Funktion hinzufügen, an den Konstruktor (aufgerufen der `canExecute` Argument) zurückgibt `true` Wenn die `Button` aktiviert werden soll. Hier wird die Änderung an der `CommandDemoViewModel` Konstruktor:

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

Die Aufrufe an die `ChangeCanExecute` Methode `Command` sind erforderlich, damit die `Command` Methode aufrufen kann die `canExecute` Methode und bestimmen, ob die `Button` sollte deaktiviert werden, oder nicht. Mit dieser Änderung des Codes, als die Anzahl erreicht den Grenzwert der `Button` deaktiviert wird:

[![Schaltfläche "Standard"-Befehl - geändert](button-images/BasicButtonCommandModified.png "grundlegende Schaltfläche Befehl - geändert")](button-images/BasicButtonCommandModified-Large.png#lightbox)

Es ist möglich, dass mindestens zwei `Button` Elemente auf das gleiche gebunden werden `ICommand` Eigenschaft. Die `Button` Elemente mit unterschieden werden können die [ `CommandParameter` ](xref:Xamarin.Forms.Button.CommandParameter) Eigenschaft `Button`. In diesem Fall sollten die generischen [ `Command<T>` ](xref:Xamarin.Forms.Command`1) Klasse. Die `CommandParameter` Objekt wird dann als Argument übergeben der `execute` und `canExecute` Methoden. Dieses Verfahren wird gezeigt, im Detail in der [ **grundlegende Befehle** ](~/xamarin-forms/app-fundamentals/data-binding/commanding.md#basic-commanding) Teil der [ **Befehlsschnittstelle** ](~/xamarin-forms/app-fundamentals/data-binding/commanding.md#basic-commanding) Artikel.

Die [ **ButtonDemos** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ButtonDemos) Beispiel verwendet auch diese Technik in seiner `MainPage` Klasse. Die **"MainPage.xaml"** -Datei enthält eine `Button` für jede Seite des Beispiels:

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

Jede `Button` hat seine `Command` Eigenschaft gebunden wird, um eine Eigenschaft namens `NavigateCommand`, und die `CommandParameter` auf festgelegt ist ein [ `Type` ](xref:System.Type) -Objekt, eine der Seitenklassen im Projekt entspricht.

Dass `NavigateCommand` -Eigenschaft ist vom Typ `ICommand` und im Code-Behind-Datei definiert ist:

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

Der Konstruktor initialisiert die `NavigateCommand` Eigenschaft, um eine `Command<Type>` Objekt, da der `Type` ist der Typ des der `CommandParameter` Objektsatz in der XAML-Datei. Dies bedeutet, dass die `execute` Methode verfügt über ein Argument des Typs `Type` , entspricht dies `CommandParameter` Objekt. Die Funktion instanziiert die Seite, und wechselt anschließend darauf.

Beachten Sie, der durch Festlegen der Konstruktor abgeschlossen ist seine `BindingContext` auf sich selbst. Dies ist notwendig, damit Eigenschaften in der XAML-Datei zum Binden an die `NavigateCommand` Eigenschaft.

## <a name="pressing-and-releasing-the-button"></a>Drücken und Loslassen der

Neben den `Clicked` Ereignis `Button` definiert auch [ `Pressed` ](xref:Xamarin.Forms.Button.Pressed) und [ `Released` ](xref:Xamarin.Forms.Button.Released) Ereignisse. Die `Pressed` Ereignis tritt auf, wenn ein Finger drückt, auf eine `Button`, oder eine Maustaste gedrückt wird mit dem Mauszeiger auf positioniert das `Button`. Die `Released` Ereignis tritt auf, wenn die Finger oder der Maustaste losgelassen wird. Im Allgemeinen eine `Clicked` wird auch ausgelöst, zur gleichen Zeit wie der `Released` Ereignis jedoch wenn der Zeiger Finger oder Maus Weg von der Oberfläche des Folien der `Button` vor freigegeben wird, der `Clicked` Ereignis kann nicht auftreten.

Die `Pressed` und `Released` Ereignisse werden nicht oft verwendet, aber sie können für spezielle Zwecke verwendet werden, wie in der **drücken und Release-Schaltfläche** Seite. Die Verwendung von XAML-Datei enthält eine `Label` und ein `Button` Handler angefügten für die `Pressed` und `Released` Ereignisse:

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

Die Code-Behind-Datei erstellt eine Animation der `Label` beim eine `Pressed` Ereignis tritt auf, aber die Drehung hält bei der ein `Released` Ereignis tritt auf:

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

Das Ergebnis ist, die `Label` nur dreht, während ein Finger in Kontakt mit ist der `Button`, und beendet, wenn sich der Finger freigegeben wird:

[![Drücken Sie und Los](button-images/PressAndReleaseButton.png "drücken und los")](button-images/PressAndReleaseButton-Large.png)

Diese Art von Verhalten weist eine Anwendung für Spiele: ein Finger belegt eine `Button` kann, ein auf Bildschirmobjekt, das in eine bestimmte Richtung zu verschieben. 

<a name="button-appearance" />

## <a name="button-appearance"></a>Schaltfläche Darstellung

Die `Button` erbt oder definiert verschiedene Eigenschaften, die seine Darstellung beeinflussen:

- [`TextColor`](xref:Xamarin.Forms.Button.TextColor) entspricht der Farbe der `Button` Text
- [`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor) ist die Farbe des Hintergrunds auf den ausgewählten text
- [`BorderColor`](xref:Xamarin.Forms.Button.BorderColor) entspricht der Farbe ein Bereich umgibt die `Button`
- [`FontFamily`](xref:Xamarin.Forms.Button.FontFamily) die Schriftfamilie ist für das Textfeld verwendet werden
- [`FontSize`](xref:Xamarin.Forms.Button.FontSize) ist die Größe des Texts
- [`FontAttributes`](xref:Xamarin.Forms.Button.FontAttributes) Gibt an, ob der Text kursiv oder fett formatiert ist.
- [`BorderWidth`](xref:Xamarin.Forms.Button.BorderWidth) ist die Breite des Rahmens 
- [`CornerRadius`](xref:Xamarin.Forms.Button.CornerRadius) Rundet die Ecken

Die Auswirkungen von sechs dieser Eigenschaften (außer `FontFamily` und `FontAttributes`) werden veranschaulicht die **Darstellung Schaltfläche** Seite. Eine andere Eigenschaft [ `Image` ](xref:Xamarin.Forms.Button.Image), wird im Abschnitt erläutert [ **Schaltfläche Bitmaps mit**](#image-button).

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

Die `Button` am oberen Rand der Seite verfügt über die drei `Color` Eigenschaften gebunden werden, um `Picker` Elemente am unteren Rand der Seite. Die Elemente in der `Picker` Elemente sind Farben aus der `NamedColor` Klasse im Projekt enthalten. Drei `Slider` Elemente enthalten, bidirektionale Bindungen, die `FontSize`, `BorderWidth`, und `CornerRadius` Eigenschaften der `Button`.

Dieses Programm können Sie zum Experimentieren mit Kombinationen von all diese Eigenschaften:

[![Schaltfläche Darstellung](button-images/ButtonAppearance.png "Schaltfläche Darstellung")](button-images/ButtonAppearance-Large.png)

Anzeigen der `Button` Rahmen, müssen Sie die festlegen eine `BorderColor` auf einen anderen Wert als `Default`, und die `BorderWidth` in einen positiven Wert.

IOS, werden Sie feststellen, dass in das Innere eines großen Rahmenbreiten Eindringen der `Button` und bei der Anzeige von Text in Konflikt. Wenn Sie einen Rahmen mit einem iOS verwenden `Button`, wahrscheinlich sollten zum Starten und Beenden der `Text` Eigenschaft mit Leerzeichen seine Sichtbarkeit beibehalten werden sollen.

Auf UWP Auswählen einer `CornerRadius` , die Hälfte die Höhe des überschreitet die `Button` löst eine Ausnahme aus.

## <a name="creating-a-toggle-button"></a>Erstellen einer Schaltfläche zum ein-/ausschalten

Es ist möglich, Unterklasse `Button` so, dass sie z. B. einen-aus-Schalter funktioniert: Tippen Sie auf die Schaltfläche "Hilfe", um die Umschaltfläche auf, und tippen sie erneut, um es deaktivieren.

Die folgenden `ToggleButton` Klasse abgeleitet `Button` und ein neues Ereignis mit dem Namen definiert `Toggled` und eine boolesche Eigenschaft, die mit dem Namen `IsToggled`. Hierbei handelt es sich um die gleichen zwei Eigenschaften definiert, indem Sie das Xamarin.Forms [ `Switch` ](xref:Xamarin.Forms.Switch):

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

Die `ToggleButton` Konstruktor fügt einen Handler, der die `Clicked` Ereignis so, dass die It den Wert ändern, kann die `IsToggled` Eigenschaft. Die `OnIsToggledChanged` Methode ausgelöst wird die `Toggled` Ereignis. 

Die letzte Zeile des der `OnIsToggledChanged` Methode ruft die statische `VisualStateManager.GoToState` Methode mit dem Text der beiden Zeichenfolgen "ToggledOn" und "ToggledOff". Informieren Sie sich über diese Methode und wie Ihre Anwendung auf visuelle Zustände im Artikel reagieren kann [ **der Xamarin.Forms visuellen Status-Manager**](~/xamarin-forms/user-interface/visual-state-manager.md). 

Da `ToggleButton` führt den Aufruf für `VisualStateManager.GoToState`, muss die Klasse selbst enthalten zusätzlichen Funktionen so ändern Sie die Darstellung der Schaltfläche basierend auf seiner `IsToggled` Zustand. D. h. die Zuständigkeit für den XAML-Code, der hostet die `ToggleButton`. 

Die **ein-/ausschalten Schaltflächendemo** Seite enthält zwei Instanzen des `ToggleButton`, einschließlich Markup visuellen Status-Manager, der festlegt der `Text`, `BackgroundColor`, und `TextColor` der Schaltfläche basierend auf den visuellen Status: 

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

Die `Toggled` Ereignishandler sind in der CodeBehind-Datei. Sie sind verantwortlich für die Einstellung der `FontAttributes` Eigenschaft von der `Label` basierend auf dem Status der Schaltflächen:

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

Hier wird das Programm auf iOS-, Android- und die universelle Windows-Plattform ausgeführt wird:

[![Schalten Sie die Schaltflächendemo](button-images/ToggleButtonDemo.png "Schaltflächendemo umschalten")](button-images/ToggleButtonDemo-Large.png#lightbox)

<a name="image-button" />

## <a name="using-bitmaps-with-buttons"></a>Verwenden von Bitmaps mit Schaltflächen

Die `Button` Klasse definiert ein [ `Image` ](xref:Xamarin.Forms.Button.Image) -Eigenschaft, die auf ein Bitmapbild Anzeige ermöglicht die `Button`, entweder allein oder in Kombination mit Text. Sie können auch angeben, wie die Text- und Bilddateien angeordnet sind.

Die `Image` -Eigenschaft ist vom Typ [ `FileImageSource` ](xref:Xamarin.Forms.FileImageSource), was bedeutet, dass die Bitmaps als Ressourcen in den einzelnen plattformprojekten und nicht in das .NET Standard-Steuerelementbibliothek-Projekt gespeichert werden müssen. 

Jede Plattform unterstützt, indem Sie Xamarin.Forms kann Bilder in verschiedenen Größen für unterschiedliche Pixel-Lösungen von verschiedenen Geräten gespeichert werden, die auf die Anwendung möglicherweise ausgeführt. Diese Komponenten, die mehrere Bitmaps werden mit dem Namen oder auf eine Weise, dass das Betriebssystem die beste Übereinstimmung ausgewählt werden kann, für das Gerät Videoadapter des gespeicherten, Bildschirmauflösung. 

Für eine Bitmap auf eine `Button`, die optimale Größe wird in der Regel zwischen 32 und 64 geräteunabhängigen Einheiten, je nach Umfang Sie ihn haben möchten. Die Bilder in diesem Beispiel verwendete basieren auf eine Größe von 48 geräteunabhängigen Einheiten.

In der iOS-Projekt die **Ressourcen** Ordner enthält drei Größen dieses Bilds:

- Eine quadratische 48 Pixel-Bitmap gespeichert als **/Resources/MonkeyFace.png**
- Eine quadratische 96 Pixel-Bitmap gespeichert als **/Resource/MonkeyFace@2x.png**
- Eine quadratische 144-Pixel-Bitmap, die als gespeichert **/Resource/MonkeyFace@3x.png**

Alle drei Bitmaps ausgehändigten eine **Buildvorgang** von **BundleResource**.

Für das Android-Projekt die Bitmaps, die alle den gleichen Namen aufweisen, aber sie werden in verschiedenen Unterordnern gespeichert die **Ressourcen** Ordner:

- Eine quadratische 72-Pixel-Bitmap, die als gespeicherte **/Resources/drawable-hdpi/MonkeyFace.png**
- Eine quadratische 96 Pixel-Bitmap gespeichert als **/Resources/drawable-xhdpi/MonkeyFace.png**
- Eine quadratische 144-Pixel-Bitmap, die als gespeicherte **/Resources/drawable-xxhdpi/MonkeyFace.png**
- Eine quadratische 192-Pixel-Bitmap, die als gespeicherte **/Resources/drawable-xxxhdpi/MonkeyFace.png**

Diese ausgehändigten eine **Buildvorgang** von **AndroidResource**.

In der uwp-Projekt Bitmaps können an einer beliebigen Stelle im Projekt gespeichert werden, aber sie werden in der Regel in einem benutzerdefinierten Ordner gespeichert oder die **Bestand** vorhandenen Ordner. Uwp-Projekt enthält diese Bitmaps:

- Eine quadratische 48 Pixel-Bitmap gespeichert als **/Assets/MonkeyFace.scale-100.png**
- Eine quadratische 96 Pixel-Bitmap gespeichert als **/Assets/MonkeyFace.scale-200.png**
- Eine quadratische 192-Pixel-Bitmap, die als gespeicherte **/Assets/MonkeyFace.scale-400.png**

Alle erhielten eine **Buildvorgang** von **Content**.

Können Sie angeben, wie die `Text` und `Image` Eigenschaften angeordnet sind, auf die `Button` mithilfe der [ `ContentLayout` ](xref:Xamarin.Forms.Button.ContentLayout) Eigenschaft `Button`. Diese Eigenschaft ist vom Typ [ `ButtonContentLayout` ](xref:Xamarin.Forms.Button.ButtonContentLayout), dies ist eine eingebettete Klasse in `Button`. Die [Konstruktor](xref:Xamarin.Forms.Button.ButtonContentLayout.%23ctor(Xamarin.Forms.Button.ButtonContentLayout.ImagePosition,System.Double)) verfügt über zwei Argumente:

- Ein Mitglied der [ `ImagePosition` ](xref:Xamarin.Forms.Button.ButtonContentLayout.ImagePosition) Enumeration: `Left`, `Top`, `Right`, oder `Bottom` , der angibt, wie die Bitmap relativ zu dem Text angezeigt wird.
- Ein `double` Wert für den Abstand zwischen der Bitmap und dem Text.

Die Standardwerte sind `Left` und 10 Einheiten. Zwei schreibgeschützte Eigenschaften des `ButtonContentLayout` mit dem Namen [ `Position` ](xref:Xamarin.Forms.Button.ButtonContentLayout.Position) und [ `Spacing` ](xref:Xamarin.Forms.Button.ButtonContentLayout.Spacing) Geben Sie die Werte dieser Eigenschaften.

In Code, erstellen Sie eine `Button` und legen Sie die `ContentLayout` Eigenschaft wie folgt:

```csharp
Button button = new Button
{
    Text = "button text",
    Image = new FileImageSource
    {
        File = "image filename"
    },
    ContentLayout = new Button.ButtonContentLayout(Button.ButtonContentLayout.ImagePosition.Right, 20)
};
```

In XAML müssen Sie nur die Enumerationsmember oder den Abstand angeben, oder beide Typen in beliebiger Reihenfolge durch Kommas getrennt:

```xaml
<Button Text="button text"
        Image="image filename"
        ContentLayout="Right, 20" />
```

Die **Bild Schaltflächendemo** Seite verwendet `OnPlatform` einen anderen Dateinamen an, für die iOS, Android und uwp--Dateien Bitmap. Wenn Sie verwenden möchten, verwenden den gleichen Dateinamen für alle drei Plattformen und vermeiden Sie die Verwendung von `OnPlatform`, Sie müssen die Bitmaps für universelle Windows-Plattform im Stammverzeichnis des Projekts zu speichern.

Die erste `Button` auf die **Bild Schaltflächendemo** Seite legt die `Image` Eigenschaft jedoch nicht die `Text` Eigenschaft:

```xaml
<Button>
    <Button.Image>
        <OnPlatform x:TypeArguments="FileImageSource">
            <On Platform="iOS, Android" Value="MonkeyFace.png" />
            <On Platform="UWP" Value="Assets/MonkeyFace.png" />
        </OnPlatform>
    </Button.Image>
</Button>
```

Wenn die Bitmaps für universelle Windows-Plattform im Stammverzeichnis des Projekts gespeichert sind, kann dieses Markup erheblich vereinfacht werden:

```xaml
<Button Image="MonkeyFace.png" />
```

Zur Vermeidung von viele sich wiederholende Markup in der **ImageButtonDemo.xaml** Datei, einen impliziten `Style` wird auch zum Festlegen von definiert die `Image` Eigenschaft. Dies `Style` wird automatisch auf andere fünf angewendet `Button` Elemente. So sieht die vollständige Verwendung von XAML-Datei aus:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="ButtonDemos.ImageButtonDemoPage">

    <FlexLayout Direction="Column"
                JustifyContent="SpaceEvenly"
                AlignItems="Center">
        
        <FlexLayout.Resources>
            <Style TargetType="Button">
                <Setter Property="Image">
                    <OnPlatform x:TypeArguments="FileImageSource">
                        <On Platform="iOS, Android" Value="MonkeyFace.png" />
                        <On Platform="UWP" Value="Assets/MonkeyFace.png" />
                    </OnPlatform>
                </Setter>
            </Style>
        </FlexLayout.Resources>

        <Button>
            <Button.Image>
                <OnPlatform x:TypeArguments="FileImageSource">
                    <On Platform="iOS, Android" Value="MonkeyFace.png" />
                    <On Platform="UWP" Value="Assets/MonkeyFace.png" />
                </OnPlatform>
            </Button.Image>
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

Die letzten vier `Button` -Elemente verwenden, der die `ContentLayout` Eigenschaft, um eine Position und Abstand für Text und Bitmap anzugeben:

[![Bild der Schaltflächendemo](button-images/ImageButtonDemo.png "Bild der Schaltflächendemo")](button-images/ImageButtonDemo-Large.png#lightbox)

Sie haben nun gesehen, die verschiedenen Möglichkeiten, die Sie behandeln können `Button` Ereignisse und Ändern der `Button` Darstellung.

## <a name="related-links"></a>Verwandte Links

- [ButtonDemos-Beispiel](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ButtonDemos)
- [API-Schaltfläche](xref:Xamarin.Forms.Button)
