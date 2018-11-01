---
title: Die Xamarin.Forms-Befehlsschnittstelle
description: In diesem Artikel wird erläutert, wie die Command-Eigenschaft mit Xamarin.Forms datenbindungen implementieren werden. Die Eingabeereignisse-Schnittstelle bietet eine alternative Methode zum Implementieren von Befehlen, die für die MVVM-Architektur viel besser geeignet ist.
ms.prod: xamarin
ms.assetid: 69922284-F398-45C3-B4CC-B8E29BB4C533
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
ms.openlocfilehash: 68c7869254ae861cef8307431d925368082be921
ms.sourcegitcommit: 729035af392dc60edb9d99d3dc13d1ef69d5e46c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2018
ms.locfileid: "50675254"
---
# <a name="the-xamarinforms-command-interface"></a>Die Xamarin.Forms-Befehlsschnittstelle

In der Model-View-ViewModel (MVVM)-Architektur von datenbindungen definiert sind, zwischen Eigenschaften in ViewModel, der in der Regel eine Klasse, die von abgeleitet `INotifyPropertyChanged`, und Eigenschaften in der Ansicht, der in der Regel die XAML-Datei. In einigen Fällen verfügt eine Anwendung Anforderungen, die diese Eigenschaft Bindungen hinausgehen, da des Benutzers Befehle zu initiieren, die ein Element in "ViewModel" betreffen. Diese Befehle werden in der Regel durch Klicken auf eine Schaltfläche signalisiert oder finger Taps und sie werden normalerweise verarbeitet, in der CodeBehind-Datei in einem Handler für die `Clicked` Ereignis die `Button` oder `Tapped` Ereignis eine `TapGestureRecognizer`.

Die Eingabeereignisse-Schnittstelle bietet eine alternative Methode zum Implementieren von Befehlen, die für die MVVM-Architektur viel besser geeignet ist. Das ViewModel selbst kann Befehle enthalten, die Methoden darstellen, die in Reaktion auf eine bestimmte Aktivität in der Ansicht, z. B. ausgeführt werden eine `Button` klicken Sie auf. Datenbindungen werden zwischen diesen Befehlen definiert und die `Button`.

Um eine Datenbindung zwischen ermöglichen eine `Button` und ein "ViewModel", die `Button` definiert zwei Eigenschaften:

- [`Command`](xref:Xamarin.Forms.Button.Command) Der Typ [`System.Windows.Input.ICommand`](xref:System.Windows.Input.ICommand)
- [`CommandParameter`](xref:Xamarin.Forms.Button.CommandParameter) Der Typ `Object`

Um die Befehlsschnittstelle verwenden zu können, definieren Sie eine Datenbindung, dessen Ziel die `Command` Eigenschaft der `Button` , in dem die Quelle ist eine Eigenschaft in "ViewModel" vom Typ `ICommand`. Das ViewModel-Code zugeordnet sind, enthält `ICommand` -Eigenschaft, die ausgeführt wird, wenn die Schaltfläche geklickt wird. Sie können festlegen, `CommandParameter` an beliebige Daten mehrere Schaltflächen unterscheiden, wenn sie alle sind gebunden werden soll, auf die gleiche `ICommand` -Eigenschaft in "ViewModel".

Die `Command` und `CommandParameter` Eigenschaften werden auch durch die folgenden Klassen definiert:

- [`MenuItem`](xref:Xamarin.Forms.MenuItem) und daher [ `ToolbarItem` ](xref:Xamarin.Forms.ToolbarItem), die sich daraus ableitet `MenuItem`
- [`TextCell`](xref:Xamarin.Forms.TextCell) und daher [ `ImageCell` ](xref:Xamarin.Forms.ImageCell), die sich daraus ableitet `TextCell`
- [`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer)

[`SearchBar`](xref:Xamarin.Forms.SearchBar) definiert eine [ `SearchCommand` ](xref:Xamarin.Forms.SearchBar.SearchCommand) Eigenschaft vom Typ `ICommand` und [ `SearchCommandParameter` ](xref:Xamarin.Forms.SearchBar.SearchCommandParameter) Eigenschaft. Die [ `RefreshCommand` ](xref:Xamarin.Forms.ListView.RefreshCommand) Eigenschaft [ `ListView` ](xref:Xamarin.Forms.ListView) wird auch vom Typ `ICommand`.

All diese Befehle können innerhalb eines "ViewModels" in einer Weise behandelt werden, die nicht von der bestimmten Benutzeroberflächen-Objekt in der Ansicht abhängig ist.

## <a name="the-icommand-interface"></a>Die ICommand-Schnittstelle

Die [ `System.Windows.Input.ICommand` ](xref:System.Windows.Input.ICommand) Schnittstelle ist nicht Teil von Xamarin.Forms. Es wird stattdessen definiert, der [System.Windows.Input](xref:System.Windows.Input) -Namespace und besteht aus zwei Methoden und ein Ereignis:

```csharp
public interface ICommand
{
    public void Execute (Object parameter);

    public bool CanExecute (Object parameter);

    public event EventHandler CanExecuteChanged;
}
```

Um die Befehlsschnittstelle verwenden zu können, Ihr ViewModel enthält Eigenschaften des Typs `ICommand`:

```csharp
public ICommand MyCommand { private set; get; }
```

Das "ViewModel" muss eine Klasse, implementiert auch verweisen die `ICommand` Schnittstelle. Diese Klasse wird in Kürze beschrieben. In der Ansicht der `Command` Eigenschaft eine `Button` an diese Eigenschaft gebunden ist:

```xaml
<Button Text="Execute command"
        Command="{Binding MyCommand}" />
```

Wenn der Benutzer drückt die `Button`, `Button` Aufrufe der `Execute` -Methode in der die `ICommand` -Objekt zu seine `Command` Eigenschaft. Das ist der einfachste Teil der Eingabeereignisse-Schnittstelle.

Die `CanExecute` Methode ist komplexer. Wenn die Bindung zuerst auf definiert ist die `Command` Eigenschaft der `Button`, und wenn die Datenbindung in irgendeiner Form, ändert der `Button` Aufrufe der `CanExecute` -Methode in der die `ICommand` Objekt. Wenn `CanExecute` gibt `false`, und klicken Sie dann die `Button` deaktiviert. Dies gibt an, dass der Befehl derzeit nicht verfügbar oder ungültig ist.

Die `Button` fügt außerdem einen Handler für die `CanExecuteChanged` Ereignis `ICommand`. Das Ereignis wird von in "ViewModel" ausgelöst. Wenn das Ereignis ausgelöst wird, die `Button` Aufrufe `CanExecute` erneut aus. Die `Button` können Sie selbst, wenn `CanExecute` gibt `true` und deaktiviert Sie sich an, wenn `CanExecute` gibt `false`.

> [!IMPORTANT]
> Verwenden Sie nicht die `IsEnabled` Eigenschaft `Button` bei Verwendung die Befehlsschnittstelle.  

## <a name="the-command-class"></a>Die Befehlsklasse

Wenn Ihre "ViewModel" definiert eine Eigenschaft des Typs `ICommand`, das "ViewModel" muss auch enthalten oder verweisen auf eine Klasse, implementiert die `ICommand` Schnittstelle. Diese Klasse enthalten oder darauf verweisen muss die `Execute` und `CanExecute` Methoden und Auslösen der `CanExecuteChanged` Ereignis immer die `CanExecute` Methode möglicherweise einen anderen Wert zurück.

Sie können eine solche Klasse selbst schreiben, oder können Sie eine Klasse, die eine andere Person geschrieben hat. Da `ICommand` ist Teil von Microsoft Windows, wurde es verwendet seit Jahren mit Windows-MVVM-Anwendungen. Verwenden einer Windows-Klasse, die implementiert `ICommand` können Sie Ihre ViewModels zwischen Windows-Anwendungen und Xamarin.Forms-Anwendungen zu nutzen.

Wenn Freigabe ViewModels zwischen Windows und Xamarin.Forms nicht relevant ist, können Sie die [ `Command` ](xref:Xamarin.Forms.Command) oder [ `Command<T>` ](xref:Xamarin.Forms.Command`1) Klasse, die in Xamarin.Forms, um die implementierenenthalten`ICommand`Schnittstelle. Diese Klassen ermöglichen Ihnen die Angabe des Hauptteils von der `Execute` und `CanExecute` Methoden in Konstruktoren. Verwenden Sie `Command<T>` bei Verwendung der `CommandParameter` -Eigenschaft zur Unterscheidung zwischen mehrere Ansichten gebunden, auf die gleiche `ICommand` Eigenschaft verwendet werden soll, und die einfachere `Command` Klasse bei, die eine Anforderung nicht.

## <a name="basic-commanding"></a>Grundlegende Befehle

Die **Person Eintrag** auf der Seite die [ **Data Binding Demos** ](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/) Programm zeigt einige einfache Befehle, die in ein "ViewModel" implementiert.

Die `PersonViewModel` definiert drei Eigenschaften, die mit dem Namen `Name`, `Age`, und `Skills` , die eine Person definieren. Diese Klasse kann *nicht* enthalten `ICommand` Eigenschaften:

```csharp
public class PersonViewModel : INotifyPropertyChanged
{
    string name;
    double age;
    string skills;

    public event PropertyChangedEventHandler PropertyChanged;

    public string Name
    {
        set { SetProperty(ref name, value); }
        get { return name; }
    }

    public double Age
    {
        set { SetProperty(ref age, value); }
        get { return age; }
    }

    public string Skills
    {
        set { SetProperty(ref skills, value); }
        get { return skills; }
    }

    public override string ToString()
    {
        return Name + ", age " + Age;
    }

    bool SetProperty<T>(ref T storage, T value, [CallerMemberName] string propertyName = null)
    {
        if (Object.Equals(storage, value))
            return false;

        storage = value;
        OnPropertyChanged(propertyName);
        return true;
    }

    protected void OnPropertyChanged([CallerMemberName] string propertyName = null)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

Die `PersonCollectionViewModel` gezeigt unten erstellt neue Objekte des Typs `PersonViewModel` und ermöglicht dem Benutzer, die in den Daten zu füllen. Zu diesem Zweck die Klasse definiert Eigenschaften `IsEditing` des Typs `bool` und `PersonEdit` des Typs `PersonViewModel`. Darüber hinaus die Klasse definiert drei Eigenschaften des Typs `ICommand` und eine Eigenschaft mit dem Namen `Persons` des Typs `IList<PersonViewModel>`:

```csharp
public class PersonCollectionViewModel : INotifyPropertyChanged
{
    PersonViewModel personEdit;
    bool isEditing;

    public event PropertyChangedEventHandler PropertyChanged;

    ···

    public bool IsEditing
    {
        private set { SetProperty(ref isEditing, value); }
        get { return isEditing; }
    }

    public PersonViewModel PersonEdit
    {
        set { SetProperty(ref personEdit, value); }
        get { return personEdit; }
    }

    public ICommand NewCommand { private set; get; }

    public ICommand SubmitCommand { private set; get; }

    public ICommand CancelCommand { private set; get; }

    public IList<PersonViewModel> Persons { get; } = new ObservableCollection<PersonViewModel>();

    bool SetProperty<T>(ref T storage, T value, [CallerMemberName] string propertyName = null)
    {
        if (Object.Equals(storage, value))
            return false;

        storage = value;
        OnPropertyChanged(propertyName);
        return true;
    }

    protected void OnPropertyChanged([CallerMemberName] string propertyName = null)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

Diese abgekürzte Auflistung schließt nicht den Konstruktor der Klasse, wofür die drei Eigenschaften des Typs `ICommand` definiert sind, werden die in Kürze angezeigt werden. Ändert sich auf die drei Eigenschaften des Typs `ICommand` und `Persons` Eigenschaft keinen unnötigen `PropertyChanged` Ereignisse ausgelöst werden. Diese Eigenschaften festgelegt sind, wenn die Klasse erstellt wird, und danach nicht ändern.

Bevor Sie den Konstruktor der Untersuchen der `PersonCollectionViewModel` Klasse, sehen wir uns die XAML-Datei für die **Person Eintrag** Programm. Diese Datei enthält eine `Grid` mit der `BindingContext` -Eigenschaftensatz auf die `PersonCollectionViewModel`. Die `Grid` enthält eine `Button` mit dem Text **neu** mit seine `Command` -Eigenschaft gebunden werden, um die `NewCommand` -Eigenschaft in "ViewModel", ein Anmeldeformular mit Eigenschaften gebunden, um die `IsEditing` -Eigenschaft, als auch als Eigenschaften des `PersonViewModel`, und zwei weitere Schaltflächen gebunden, um die `SubmitCommand` und `CancelCommand` Eigenschaften von "ViewModel". Die endgültige `ListView` zeigt die Auflistung von Personen, die bereits eingegeben:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.PersonEntryPage"
             Title="Person Entry">
    <Grid Margin="10">
        <Grid.BindingContext>
            <local:PersonCollectionViewModel />
        </Grid.BindingContext>

        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <!-- New Button -->
        <Button Text="New"
                Grid.Row="0"
                Command="{Binding NewCommand}"
                HorizontalOptions="Start" />

        <!-- Entry Form -->
        <Grid Grid.Row="1"
              IsEnabled="{Binding IsEditing}">

            <Grid BindingContext="{Binding PersonEdit}">
                <Grid.RowDefinitions>
                    <RowDefinition Height="Auto" />
                    <RowDefinition Height="Auto" />
                    <RowDefinition Height="Auto" />
                </Grid.RowDefinitions>

                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="Auto" />
                    <ColumnDefinition Width="*" />
                </Grid.ColumnDefinitions>

                <Label Text="Name: " Grid.Row="0" Grid.Column="0" />
                <Entry Text="{Binding Name}"
                       Grid.Row="0" Grid.Column="1" />

                <Label Text="Age: " Grid.Row="1" Grid.Column="0" />
                <StackLayout Orientation="Horizontal"
                             Grid.Row="1" Grid.Column="1">
                    <Stepper Value="{Binding Age}"
                             Maximum="100" />
                    <Label Text="{Binding Age, StringFormat='{0} years old'}"
                           VerticalOptions="Center" />
                </StackLayout>

                <Label Text="Skills: " Grid.Row="2" Grid.Column="0" />
                <Entry Text="{Binding Skills}"
                       Grid.Row="2" Grid.Column="1" />

            </Grid>
        </Grid>

        <!-- Submit and Cancel Buttons -->
        <Grid Grid.Row="2">
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="*" />
                <ColumnDefinition Width="*" />
            </Grid.ColumnDefinitions>

            <Button Text="Submit"
                    Grid.Column="0"
                    Command="{Binding SubmitCommand}"
                    VerticalOptions="CenterAndExpand" />

            <Button Text="Cancel"
                    Grid.Column="1"
                    Command="{Binding CancelCommand}"
                    VerticalOptions="CenterAndExpand" />
        </Grid>

        <!-- List of Persons -->
        <ListView Grid.Row="3"
                  ItemsSource="{Binding Persons}" />
    </Grid>
</ContentPage>
```

So funktioniert es: der erste Benutzer drückt die **neu** Schaltfläche. Dies ermöglicht es das Eintragsformular, deaktiviert aber die **neu** Schaltfläche. Der Benutzer gibt dann einen Namen, Alter und Fähigkeiten. Der Benutzer kann jederzeit während der Bearbeitung, drücken die **Abbrechen** zu beginnen. Nur wenn ein Name und ein gültiges Alter eingegeben wurden die **senden** Schaltfläche aktiviert. Drücken dies **senden** überträgt die Person, die der Auflistung angezeigt werden, indem die `ListView`. Entweder nach dem die **Abbrechen** oder **senden** gedrückt wird, die das Eintragsformular deaktiviert ist und die **neu** Schaltfläche erneut aktiviert wird.

IOS-Bildschirms auf der linken Seite zeigt das Layout an, bevor Sie ein gültiges Alter eingegeben wird. Die Android- und UWP Bildschirme zeigen die **senden** Schaltfläche aktiviert wird, nachdem ein Alter festgelegt wurde:

[![Person Eintrag](commanding-images/personentry-small.png "Person Eintrag")](commanding-images/personentry-large.png#lightbox "Person-Eintrag")

Die Anwendung verfügt nicht über keine Möglichkeit zum Bearbeiten vorhandener Einträge, und die Einträge werden nicht gespeichert werden, wenn Sie von der Seite Weg navigieren.

Gesamte Logik für die **neu**, **senden**, und **Abbrechen** Schaltflächen erfolgt im `PersonCollectionViewModel` über Definitionen der `NewCommand`, `SubmitCommand`, und `CancelCommand` Eigenschaften. Der Konstruktor, der die `PersonCollectionViewModel` legt diese drei Eigenschaften zu Objekten des Typs `Command`.  

Ein [Konstruktor](xref:Xamarin.Forms.Command.%23ctor(System.Action,System.Func{System.Boolean})) von der `Command` Klasse ermöglicht die Übergabe von Argumenten des Typs `Action` und `Func<bool>` entspricht der `Execute` und `CanExecute` Methoden. Es ist am einfachsten, diese Aktionen und Funktionen als Lambda-Funktionen direkt im Definieren der `Command` Konstruktor. Hier ist die Definition der `Command` -Objekt für die `NewCommand` Eigenschaft:

```csharp
public class PersonCollectionViewModel : INotifyPropertyChanged
{

    ···

    public PersonCollectionViewModel()
    {
        NewCommand = new Command(
            execute: () =>
            {
                PersonEdit = new PersonViewModel();
                PersonEdit.PropertyChanged += OnPersonEditPropertyChanged;
                IsEditing = true;
                RefreshCanExecutes();
            },
            canExecute: () =>
            {
                return !IsEditing;
            });

        ···

    }

    void OnPersonEditPropertyChanged(object sender, PropertyChangedEventArgs args)
    {
        (SubmitCommand as Command).ChangeCanExecute();
    }

    void RefreshCanExecutes()
    {
        (NewCommand as Command).ChangeCanExecute();
        (SubmitCommand as Command).ChangeCanExecute();
        (CancelCommand as Command).ChangeCanExecute();
    }

    ···

}
```

Klickt der Benutzer die **neu** Schaltfläche der `execute` Funktion übergeben, um die `Command` Konstruktor ausgeführt wird. Dies erstellt ein neues `PersonViewModel` Objekt, legt einen Handler für dieses Objekts fest `PropertyChanged` Ereignis legt `IsEditing` zu `true`, und ruft die `RefreshCanExecutes` Methode, die nach dem Konstruktor definiert.

Neben der Implementierung der `ICommand` -Schnittstelle, die `Command` -Klasse definiert außerdem eine Methode namens `ChangeCanExecute`. Sollte Ihr ViewModel Aufrufen `ChangeCanExecute` für eine `ICommand` Eigenschaft, wenn alles in diesem Fall kann den Rückgabewert von Ändern der `CanExecute` Methode. Ein Aufruf von `ChangeCanExecute` bewirkt, dass die `Command` Klasse ausgelöst werden die `CanExecuteChanged` Methode. Die `Button` verfügt über einen Handler für das Ereignis angefügt und antwortet durch Aufrufen `CanExecute` erneut, und die anschließende basiert auf den Rückgabewert dieser Methode selbst.

Wenn der `execute` -Methode der `NewCommand` Aufrufe `RefreshCanExecutes`, die `NewCommand` -Eigenschaft enthält einen Aufruf von `ChangeCanExecute`, und die `Button` Aufrufe der `canExecute` -Methode, die jetzt zurück `false` da die `IsEditing`Eigenschaft ist mittlerweile `true`.

Die `PropertyChanged` Ereignishandler für das neue `PersonViewModel` -Objekt ruft die `ChangeCanExecute` -Methode der `SubmitCommand`. Hier ist, wie diese Command-Eigenschaft implementiert wird:


```csharp
public class PersonCollectionViewModel : INotifyPropertyChanged
{

    ···

    public PersonCollectionViewModel()
    {

        ···

        SubmitCommand = new Command(
            execute: () =>
            {
                Persons.Add(PersonEdit);
                PersonEdit.PropertyChanged -= OnPersonEditPropertyChanged;
                PersonEdit = null;
                IsEditing = false;
                RefreshCanExecutes();
            },
            canExecute: () =>
            {
                return PersonEdit != null &&
                       PersonEdit.Name != null &&
                       PersonEdit.Name.Length > 1 &&
                       PersonEdit.Age > 0;
            });

        ···
    }

    ···

}
```

Die `canExecute` für Funktion `SubmitCommand` heißt, jedes Mal, wenn eine Eigenschaft geändert wird, der `PersonViewModel` Objekt bearbeitet wird. Es gibt `true` nur, wenn die `Name` -Eigenschaft ist mindestens ein Zeichen lang sein, und `Age` ist größer als 0. Zu diesem Zeitpunkt wird die **senden** Schaltfläche aktiviert wird.

Die `execute` für Funktion **senden** entfernt den Handler für eigenschaftenänderungen aus der `PersonViewModel`, fügt das Objekt, das die `Persons` Auflistung und gibt alle anfänglichen Bedingung zurück.

Die `execute` Funktion für die **Abbrechen** Schaltfläche trifft, die die **senden** Schaltfläche ausführen kann, außer das Objekt der Auflistung hinzufügen:

```csharp
public class PersonCollectionViewModel : INotifyPropertyChanged
{

    ···

    public PersonCollectionViewModel()
    {

        ···

        CancelCommand = new Command(
            execute: () =>
            {
                PersonEdit.PropertyChanged -= OnPersonEditPropertyChanged;
                PersonEdit = null;
                IsEditing = false;
                RefreshCanExecutes();
            },
            canExecute: () =>
            {
                return IsEditing;
            });
    }

    ···

}
```

Die `canExecute` Methodenrückgabe `true` zu einem beliebigen Zeitpunkt eine `PersonViewModel` bearbeitet wird.

Diese Techniken können für komplexere Szenarien angepasst werden,: eine Eigenschaft in `PersonCollectionViewModel` konnte gebunden werden, um die `SelectedItem` Eigenschaft der `ListView` zum Bearbeiten von vorhandener Elementen und eine **löschen** Schaltfläche hinzugefügt werden konnte, löschen Diese Elemente.

Es ist nicht erforderlich ist, definieren die `execute` und `canExecute` Methoden als Lambda-Funktionen. Kann so schreiben sie als reguläre private Methoden in "ViewModel" und verweisen sie in der `Command` Konstruktoren. Dieser Ansatz jedoch neigen dazu, um eine Vielzahl von Methoden zu erhalten, der nur einmal in das "ViewModel" verwiesen wird.

## <a name="using-command-parameters"></a>Über Parameter des Befehls

Manchmal ist es praktisch für eine oder mehrere Schaltflächen (oder andere Objekte der Benutzeroberfläche), die denselben `ICommand` -Eigenschaft in "ViewModel". In diesem Fall verwenden Sie die `CommandParameter` Eigenschaft zur Unterscheidung zwischen den Schaltflächen.

Sie können weiterhin verwenden die `Command` -Klasse für diese freigegebenen `ICommand` Eigenschaften. Die Klasse definiert ein [alternativen Konstruktor](xref:Xamarin.Forms.Command.%23ctor(System.Action{System.Object},System.Func{System.Object,System.Boolean})) , akzeptiert `execute` und `canExecute` Methoden mit Parametern vom Typ `Object`. Dies ist die `CommandParameter` an diese Methoden übergeben wird.

Allerdings bei Verwendung `CommandParameter`, es ist am einfachsten, die die generischen [ `Command<T>` ](xref:Xamarin.Forms.Command`1) Klasse, um den Typ des Objekts festgelegt werden, um anzugeben `CommandParameter`. Die `execute` und `canExecute` Methoden, die Sie angeben, verfügen über Parameter dieses Typs.

Die **Decimal Tastatur** Seite dieses Verfahren veranschaulicht, wie eine Zehnertastatur für die Eingabe von Dezimalzahlen zu implementieren. Die `BindingContext` für die `Grid` ist eine `DecimalKeypadViewModel`. Die `Entry` Eigenschaft von "ViewModel" gebunden ist die `Text` Eigenschaft eine `Label`. Alle der `Button` Objekte an verschiedene Befehle im ViewModel gebunden sind: `ClearCommand`, `BackspaceCommand`, und `DigitCommand`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.DecimalKeypadPage"
             Title="Decimal Keyboard">

    <Grid WidthRequest="240"
          HeightRequest="480"
          ColumnSpacing="2"
          RowSpacing="2"
          HorizontalOptions="Center"
          VerticalOptions="Center">

        <Grid.BindingContext>
            <local:DecimalKeypadViewModel />
        </Grid.BindingContext>

        <Grid.Resources>
            <ResourceDictionary>
                <Style TargetType="Button">
                    <Setter Property="FontSize" Value="32" />
                    <Setter Property="BorderWidth" Value="1" />
                    <Setter Property="BorderColor" Value="Black" />
                </Style>
            </ResourceDictionary>
        </Grid.Resources>

        <Label Text="{Binding Entry}"
               Grid.Row="0" Grid.Column="0" Grid.ColumnSpan="3"
               FontSize="32"
               LineBreakMode="HeadTruncation"
               VerticalTextAlignment="Center"
               HorizontalTextAlignment="End" />

        <Button Text="CLEAR"
                Grid.Row="1" Grid.Column="0" Grid.ColumnSpan="2"
                Command="{Binding ClearCommand}" />

        <Button Text="&#x21E6;"
                Grid.Row="1" Grid.Column="2"
                Command="{Binding BackspaceCommand}" />

        <Button Text="7"
                Grid.Row="2" Grid.Column="0"
                Command="{Binding DigitCommand}"
                CommandParameter="7" />

        <Button Text="8"
                Grid.Row="2" Grid.Column="1"
                Command="{Binding DigitCommand}"
                CommandParameter="8" />

        <Button Text="9"
                Grid.Row="2" Grid.Column="2"
                Command="{Binding DigitCommand}"
                CommandParameter="9" />

        <Button Text="4"
                Grid.Row="3" Grid.Column="0"
                Command="{Binding DigitCommand}"
                CommandParameter="4" />

        <Button Text="5"
                Grid.Row="3" Grid.Column="1"
                Command="{Binding DigitCommand}"
                CommandParameter="5" />

        <Button Text="6"
                Grid.Row="3" Grid.Column="2"
                Command="{Binding DigitCommand}"
                CommandParameter="6" />

        <Button Text="1"
                Grid.Row="4" Grid.Column="0"
                Command="{Binding DigitCommand}"
                CommandParameter="1" />

        <Button Text="2"
                Grid.Row="4" Grid.Column="1"
                Command="{Binding DigitCommand}"
                CommandParameter="2" />

        <Button Text="3"
                Grid.Row="4" Grid.Column="2"
                Command="{Binding DigitCommand}"
                CommandParameter="3" />

        <Button Text="0"
                Grid.Row="5" Grid.Column="0" Grid.ColumnSpan="2"
                Command="{Binding DigitCommand}"
                CommandParameter="0" />

        <Button Text="&#x00B7;"
                Grid.Row="5" Grid.Column="2"
                Command="{Binding DigitCommand}"
                CommandParameter="." />
    </Grid>
</ContentPage>
```

Die 11 Schaltflächen für die 10 Ziffern und das Dezimaltrennzeichen freigeben, eine Bindung an `DigitCommand`. Die `CommandParameter` unterscheidet zwischen dieser Schaltflächen. Der Wert festgelegt, `CommandParameter` entspricht im Allgemeinen den Text, der von der Schaltfläche außer dem Dezimaltrennzeichen an, die zur Veranschaulichung, mit einer mittleren Punkt angezeigt wird angezeigt.

So sieht die Anwendung in Aktion:

[![Decimal-Tastatur](commanding-images/decimalkeyboard-small.png "Decimal Tastatur")](commanding-images/decimalkeyboard-large.png#lightbox "Decimal Tastatur")

Beachten Sie, dass die Schaltfläche für das Dezimaltrennzeichen in allen drei Screenshots deaktiviert ist, da die eingegebene Zahl bereits ein Dezimaltrennzeichen enthält.

Die `DecimalKeypadViewModel` definiert eine `Entry` Eigenschaft vom Typ `string` (Dies ist die einzige Eigenschaft, die ausgelöst eine `PropertyChanged` Ereignis) und drei Eigenschaften des Typs `ICommand`:

```csharp
public class DecimalKeypadViewModel : INotifyPropertyChanged
{
    string entry = "0";

    public event PropertyChangedEventHandler PropertyChanged;

    ···

    public string Entry
    {
        private set
        {
            if (entry != value)
            {
                entry = value;
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Entry"));
            }
        }
        get
        {
            return entry;
        }
    }

    public ICommand ClearCommand { private set; get; }

    public ICommand BackspaceCommand { private set; get; }

    public ICommand DigitCommand { private set; get; }
}
```

Die Schaltfläche entspricht der `ClearCommand` ist immer aktiviert und einfach Eintrag wieder auf "0" festgelegt:

```csharp
public class DecimalKeypadViewModel : INotifyPropertyChanged
{

    ···

    public DecimalKeypadViewModel()
    {
        ClearCommand = new Command(
            execute: () =>
            {
                Entry = "0";
                RefreshCanExecutes();
            });

        ···

    }

    void RefreshCanExecutes()
    {
        ((Command)BackspaceCommand).ChangeCanExecute();
        ((Command)DigitCommand).ChangeCanExecute();
    }

    ···

}
```

Da die Schaltfläche immer aktiviert ist, ist es nicht notwendig, geben Sie einen `canExecute` -Argument in der `Command` Konstruktor.

Die Logik zum Eingeben von Zahlen, und Drücken der RÜCKTASTE ist ein wenig knifflig, da keine Ziffern eingegeben wurde, und klicken Sie dann die `Entry` -Eigenschaft ist die Zeichenfolge "0". Wenn der Benutzer weitere Nullen (0) gibt die `Entry` enthält jedoch nur eine 0 (null). Wenn der Benutzer eine andere Ziffer eingibt, wird diese Ziffer der 0 (null) ersetzt. Aber wenn der Benutzer ein Dezimaltrennzeichen, bevor Sie eine andere Ziffer, dann eingibt `Entry` ist die Zeichenfolge "0".

Die **RÜCKTASTE** -Schaltfläche ist nur, wenn die Länge des Eintrags größer als 1 ist, oder wenn `Entry` entspricht nicht der Zeichenfolge "0":

```csharp
public class DecimalKeypadViewModel : INotifyPropertyChanged
{

    ···

    public DecimalKeypadViewModel()
    {

        ···

        BackspaceCommand = new Command(
            execute: () =>
            {
                Entry = Entry.Substring(0, Entry.Length - 1);
                if (Entry == "")
                {
                    Entry = "0";
                }
                RefreshCanExecutes();
            },
            canExecute: () =>
            {
                return Entry.Length > 1 || Entry != "0";
            });

        ···

    }

    ···

}
```

Die Logik für die `execute` Funktion für die **RÜCKTASTE** Schaltfläche wird sichergestellt, dass die `Entry` ist mindestens eine Zeichenfolge mit "0".

Die `DigitCommand` Eigenschaft gebunden ist, um 11 Schaltflächen an, von denen jede identifiziert sich selbst bei der `CommandParameter` Eigenschaft. Die `DigitCommand` festgelegt werden auf einer Instanz von den regulären `Command` -Klasse, aber das einfacher zu verwenden, die `Command<T>` generische Klasse. Bei Verwendung der Eingabeereignisse Benutzeroberfläche mit XAML, die `CommandParameter` Eigenschaften sind in der Regel Zeichenfolgen, und das ist der Typ des generischen Argument. Die `execute` und `canExecute` Funktionen haben dann die Argumente des Typs `string`:

```csharp
public class DecimalKeypadViewModel : INotifyPropertyChanged
{

    ···

    public DecimalKeypadViewModel()
    {

        ···

        DigitCommand = new Command<string>(
            execute: (string arg) =>
            {
                Entry += arg;
                if (Entry.StartsWith("0") && !Entry.StartsWith("0."))
                {
                    Entry = Entry.Substring(1);
                }
                RefreshCanExecutes();
            },
            canExecute: (string arg) =>
            {
                return !(arg == "." && Entry.Contains("."));
            });
    }

    ···

}
```

Die `execute` Methode fügt das Zeichenfolgenargument für die `Entry` Eigenschaft. Jedoch, wenn das Ergebnis mit einer 0 (null) (jedoch keine 0 (null) und ein Dezimaltrennzeichen) beginnt dann dieser anfänglichen 0 (null) muss entfernt werden mithilfe der `Substring` Funktion.

Die `canExecute` Methodenrückgabe `false` nur dann, wenn das Argument ist dem Dezimaltrennzeichen an (gibt an, dass es sich bei dem Dezimaltrennzeichen betätigt wird) und `Entry` bereits ein Dezimaltrennzeichen enthält.

Alle der `execute` Methoden `RefreshCanExecutes`, wodurch wiederum `ChangeCanExecute` für beide `DigitCommand` und `ClearCommand`. Dadurch wird sichergestellt, dass dem Dezimalkomma und RÜCKTASTE Schaltflächen aktiviert oder deaktiviert auf der aktuellen Sequenz von eingegebenen Ziffern Grundlage.

## <a name="adding-commands-to-existing-views"></a>Hinzufügen von Befehlen zu vorhandenen Ansichten

Wenn Sie die Eingabeereignisse-Schnittstelle mit Ansichten verwenden, die sie nicht unterstützen möchten, ist es möglich, ein Xamarin.Forms-Verhalten zu verwenden, der ein Ereignis in einem Befehl konvertiert. Dies wird in diesem Artikel beschrieben [ **Wiederverwendbaren EventToCommandBehavior**](~/xamarin-forms/app-fundamentals/behaviors/reusable/event-to-command-behavior.md).

## <a name="asynchronous-commanding-for-navigation-menus"></a>Asynchrone Befehle für Navigationsmenüs

Befehle eignet sich für die Implementierung von Navigationsmenüs, wie z. B. in der [ **Data Binding Demos** ](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/) Programm selbst. Er ist Teil der **"MainPage.xaml"**:

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.MainPage"
             Title="Data Binding Demos"
             Padding="10">
    <TableView Intent="Menu">
        <TableRoot>
            <TableSection Title="Basic Bindings">

                <TextCell Text="Basic Code Binding"
                          Detail="Define a data-binding in code"
                          Command="{Binding NavigateCommand}"
                          CommandParameter="{x:Type local:BasicCodeBindingPage}" />

                <TextCell Text="Basic XAML Binding"
                          Detail="Define a data-binding in XAML"
                          Command="{Binding NavigateCommand}"
                          CommandParameter="{x:Type local:BasicXamlBindingPage}" />

                <TextCell Text="Alternative Code Binding"
                          Detail="Define a data-binding in code without a BindingContext"
                          Command="{Binding NavigateCommand}"
                          CommandParameter="{x:Type local:AlternativeCodeBindingPage}" />

                ···

            </TableSection>
        </TableRoot>
    </TableView>
</ContentPage>
```

Bei Verwendung der Befehle mit XAML, `CommandParameter` Eigenschaften in der Regel auf Zeichenfolgen festgelegt. Wird in diesem Fall jedoch eine XAML-Markuperweiterung verwendet, damit die `CommandParameter` ist vom Typ `System.Type`.

Jede `Command` Eigenschaft gebunden ist, auf eine Eigenschaft mit dem Namen `NavigateCommand`. Dass die Eigenschaft in der CodeBehind-Datei definiert ist **"MainPage.Xaml.cs"**:

```csharp
public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();

        NavigateCommand = new Command<Type>(
            async (Type pageType) =>
            {
                Page page = (Page)Activator.CreateInstance(pageType);
                await Navigation.PushAsync(page);
            });

        BindingContext = this;
    }

    public ICommand NavigateCommand { private set; get; }
}
```

Der Konstruktor legt die `NavigateCommand` Eigenschaft, um eine `execute` -Methode, die instanziiert die `System.Type` Parameter und wechselt anschließend darauf. Da die `PushAsync` Aufruf erfordert eine `await` -Operator, der `execute` Methode als asynchron gekennzeichnet sein muss. Dies erfolgt mit der `async` -Schlüsselwort vor der Parameterliste.

Der Konstruktor legt auch fest der `BindingContext` der Seite, um sich selbst so, dass die Bindungen verweisen auf die `NavigateCommand` in dieser Klasse.

Die Reihenfolge der der Code in diesem Konstruktor Unterschied macht sich bemerkbar: die `InitializeComponent` Aufruf führt dazu, dass das XAML analysiert werden, aber zu diesem Zeitpunkt die Bindung an eine Eigenschaft mit dem Namen `NavigateCommand` kann nicht aufgelöst werden, da `BindingContext` nastaven NA hodnotu `null`. Wenn die `BindingContext` wird im Konstruktor festgelegt *vor* `NavigateCommand` festgelegt ist, und klicken Sie dann die Bindung aufgelöst, wenn werden kann `BindingContext` festgelegt ist, aber zu diesem Zeitpunkt `NavigateCommand` ist immer noch `null`. Festlegen von `NavigateCommand` nach `BindingContext` hat keine Auswirkungen auf die Bindung, da eine Änderung an `NavigateCommand` wird nicht ausgelöst eine `PropertyChanged` Ereignis und die Bindung nicht kennen, die `NavigateCommand` ist jetzt gültig.

Einstellung `NavigateCommand` und `BindingContext` (in beliebiger Reihenfolge) vor dem Aufruf von `InitializeComponent` funktionieren, da beide Komponenten der Bindung festgelegt werden, wenn der XAML-Parser die Bindungsdefinition feststellt.

Datenbindungen können manchmal schwierig sein, aber wie Sie in dieser Artikelreihe gesehen haben, sie sind leistungsstark und vielseitig und helfen, die zum Organisieren von Code durch die Trennung der zugrunde liegende Logik, über die Benutzeroberfläche.

## <a name="related-links"></a>Verwandte Links

- [Binden von Daten-Demos (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Im Kapitel Daten-Bindung von Xamarin.Forms-Buch](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter18.md)
