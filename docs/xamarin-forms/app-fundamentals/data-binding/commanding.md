---
title: Die Befehlsschnittstelle
description: Implementieren der `Command` Eigenschaft mit dem Datenbindung
ms.topic: article
ms.prod: xamarin
ms.assetid: 69922284-F398-45C3-B4CC-B8E29BB4C533
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
ms.openlocfilehash: 0bc039385a6b2077c3b5fa5114b35b586a14a150
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="the-command-interface"></a>Die Befehlsschnittstelle

In der Model-View-ViewModel (MVVM)-Architektur datenbindungen definiert sind, zwischen Eigenschaften im ViewModel, in der Regel ist eine Klasse, die abgeleitet `INotifyPropertyChanged`, und Eigenschaften in der Sicht, die sich im Allgemeinen die Verwendung von XAML-Datei handelt. In einigen Fällen weist eine Anwendung Anforderungen, die diese Eigenschaft Bindungen hinausgehen, durch das Anfordern des Benutzers Befehle ausführen, die ein Element im ViewModel beeinflussen. Diese Befehle sind in der Regel durch Klicken auf eine Schaltfläche signalisiert oder finger Taps und sie werden normalerweise verarbeitet, in der CodeBehind-Datei in einen Handler für die `Clicked` -Ereignis für die `Button` oder `Tapped` -Ereignis für eine `TapGestureRecognizer`. 

Die Befehle Schnittstelle bietet eine alternative Methode zum Implementieren von Befehlen, die der Architektur MVVM wesentlich besser geeignet ist. Das ViewModel selbst darf Befehle, die Methoden, die in Reaktion auf eine bestimmte Aktivität in der Ansicht, z. B. ausgeführt werden eine `Button` klicken Sie auf. Datenbindungen zwischen diesen Befehlen definiert werden und die `Button`.

Ermöglicht eine Datenbindung zwischen einer `Button` und ein ViewModel, die `Button` definiert zwei Eigenschaften:

- [`Command`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.Command/) vom Typ [`ICommand`](https://developer.xamarin.com/api/type/System.Windows.Input.ICommand/)
- [`CommandParameter`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.CommandParameter/) vom Typ `Object`

Um die Befehlsschnittstelle verwenden zu können, definieren Sie eine Bindung, die als Ziel verwendet die `Command` Eigenschaft von der `Button` , in dem die Quelle ist eine Eigenschaft im ViewModel vom Typ `ICommand`. Das ViewModel enthält Code zugeordnet sind, `ICommand` -Eigenschaft, die ausgeführt wird, wenn die Schaltfläche geklickt wird. Sie können festlegen, `CommandParameter` an mehrere Schaltflächen unterscheiden, wenn sie alle sind beliebige Daten gebunden werden soll, auf das gleiche `ICommand` Eigenschaft im ViewModel.

Die `Command` und `CommandParameter` Eigenschaften werden auch von den folgenden Klassen definiert:

- [`MenuItem`](https://developer.xamarin.com/api/type/Xamarin.Forms.MenuItem/) und daher [ `ToolbarItem` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ToolbarItem/), die sich daraus ableitet `MenuItem`
- [`TextCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.TextCell/) und daher [ `ImageCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageCell/), die sich daraus ableitet `TextCell`
- [`TapGestureRecognizer`](https://developer.xamarin.com/api/type/Xamarin.Forms.TapGestureRecognizer/)

[`SearchBar`](https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/) definiert eine [ `SearchCommand` ](https://developer.xamarin.com/api/property/Xamarin.Forms.SearchBar.SearchCommand/) Eigenschaft vom Typ `ICommand` und ein [ `SearchCommandParameter` ](https://developer.xamarin.com/api/property/Xamarin.Forms.SearchBar.SearchCommandParameter/) Eigenschaft. Die [ `RefreshCommand` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.RefreshCommand/) Eigenschaft [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) wird auch vom Typ `ICommand`. 

All diese Befehle können in einem ViewModel in einer Weise behandelt werden, die nicht von der bestimmten Benutzeroberflächen-Objekt in der Sicht abhängig ist.

## <a name="the-icommand-interface"></a>Der ICommand-Schnittstelle

Die [ `ICommand` ](https://developer.xamarin.com/api/type/System.Windows.Input.ICommand/) Schnittstelle ist nicht Teil des Xamarin.Forms. Es wird stattdessen definiert, der [ `System.Windows.Input` ](https://developer.xamarin.com/api/namespace/System.Windows.Input/) -Namespace und besteht aus zwei Methoden und ein Ereignis:

```csharp
public interface ICommand
{
    public void Execute (Object parameter);

    public bool CanExecute (Object parameter);

    public event EventHandler CanExecuteChanged;
}
```

Um die Befehlsschnittstelle verwenden zu können, enthält Ihre ViewModel Eigenschaften vom Typ `ICommand`:

```csharp
public ICommand MyCommand { private set; get; }
```

Eine Klasse, die implementiert muss auch auf das ViewModel verweisen die `ICommand` Schnittstelle. Diese Klasse wird in Kürze beschrieben werden. In der Ansicht der `Command` Eigenschaft von einem `Button` an diese Eigenschaft gebunden ist: 

```xaml
<Button Text="Execute command"
        Command="{Binding MyCommand}" />
```

Wenn der Benutzer drückt die `Button`, die `Button` Aufrufe der `Execute` Methode in der `ICommand` -Objekt gebunden werden, um seine `Command` Eigenschaft. Das die einfachste Teil der Befehle-Schnittstelle ist.

Die `CanExecute` Methode ist komplexer. Die Bindung wird beim ersten definiert, auf die `Command` Eigenschaft von der `Button`, und, wenn die Datenbindung in irgendeiner Form ändert die `Button` Aufrufe der `CanExecute` Methode in der `ICommand` Objekt. Wenn `CanExecute` gibt `false`, und klicken Sie dann die `Button` deaktiviert. Dies gibt an, dass der Befehl derzeit nicht verfügbar oder ungültig ist.

Die `Button` auch Fügt einen Handler für das `CanExecuteChanged` -Ereignis `ICommand`. Das Ereignis wird von innerhalb der ViewModel ausgelöst. Wenn das Ereignis ausgelöst wird, die `Button` Aufrufe `CanExecute` erneut aus. Die `Button` können selbst, wenn `CanExecute` gibt `true` und sich selbst deaktiviert, wenn `CanExecute` gibt `false`.

> [!IMPORTANT]
> Verwenden Sie nicht die `IsEnabled` Eigenschaft `Button` bei Verwendung der Befehlsschnittstelle.  

## <a name="the-command-class"></a>Die Befehlsklasse

Wenn Ihre ViewModel definiert eine Eigenschaft vom Typ `ICommand`, ViewModel muss auch enthalten oder verweisen auf eine Klasse, implementiert die `ICommand` Schnittstelle. Diese Klasse enthalten oder darauf verweisen muss die `Execute` und `CanExecute` Methoden und Auslösen der `CanExecuteChanged` Ereignis immer die `CanExecute` Methode möglicherweise einen anderen Wert zurück.

Sie können eine solche Klasse selbst schreiben, oder können Sie eine Klasse, die eine andere Person geschrieben wurde. Da `ICommand` ist Bestandteil von Microsoft Windows, wurde es verwendet Jahre mit MVVM Windows-Anwendungen. Die Verwendung einer Windows-Klasse, die implementiert `ICommand` ermöglicht es Ihnen, Ihre ViewModels zwischen Windows und Xamarin.Forms-Anwendungen freizugeben.

Wenn ViewModels zwischen Windows und Xamarin.Forms Freigabe ist nicht relevant ist, können Sie die [ `Command` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command/) oder [ `Command<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command%3CT%3E/) in Xamarin.Forms, um die implementierenenthaltenenKlasse`ICommand`Schnittstelle. Diese Klassen ermöglichen Ihnen das Festlegen des Texts für die `Execute` und `CanExecute` Methoden in Konstruktoren-Klasse. Verwenden Sie `Command<T>` bei Verwendung der `CommandParameter` Eigenschaft zur Unterscheidung zwischen mehreren Ansichten auf die gleiche gebunden `ICommand` -Eigenschaft und die einfachere `Command` Klasse bei, die eine Anforderung nicht.

## <a name="basic-commanding"></a>Grundlegende Befehle

Die **Person Eintrag** auf der Seite der [ **Data Binding Demos** ](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/) Programm veranschaulicht einige einfache Befehle in einem ViewModel implementiert.

Die `PersonViewModel` definiert drei Eigenschaften, die mit dem Namen `Name`, `Age`, und `Skills` , die eine Person definieren. Diese Klasse ist *nicht* enthalten `ICommand` Eigenschaften:

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

Die `PersonCollectionViewModel` gezeigt unten erstellt neue Objekte des Typs `PersonViewModel` und ermöglicht dem Benutzer, die in den Daten zu füllen. Zu diesem Zweck definiert die Klasse Eigenschaften `IsEditing` des Typs `bool` und `PersonEdit` vom Typ `PersonViewModel`. Darüber hinaus die Klasse definiert drei Eigenschaften des Typs `ICommand` und eine Eigenschaft namens `Persons` des Typs `IList<PersonViewModel>`: 

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

Dieses Angebot abgekürzte enthält keinen Konstruktor der Klasse, die ist, die drei Eigenschaften des Typs `ICommand` definiert sind, in Kürze angezeigt wird. Beachten Sie die Änderungen an der drei Eigenschaften des Typs `ICommand` und `Persons` Eigenschaft führen nicht `PropertyChanged` Ereignisse ausgelöst werden. Diese Eigenschaften festgelegt sind, wenn die Klasse zum ersten Mal erstellt wird, und danach nicht ändern.

Bevor Sie den Konstruktor der Untersuchen der `PersonCollectionViewModel` Klasse, betrachten wir die Verwendung von XAML-Datei für die **Person Eintrag** Programm. Dieses Objekt enthält eine `Grid` mit seiner `BindingContext` -Eigenschaftensatz auf die `PersonCollectionViewModel`. Die `Grid` enthält eine `Button` mit dem Text **neu** mit seiner `Command` Eigenschaft gebunden wird, um die `NewCommand` -Eigenschaft in das ViewModel, ein Eintragsformular mit Eigenschaften gebunden, um die `IsEditing` -Eigenschaft, als auch als Eigenschaften des `PersonViewModel`, und zwei weitere Schaltflächen gebunden werden, um die `SubmitCommand` und `CancelCommand` Eigenschaften für das ViewModel. Der endgültige `ListView` zeigt die Auflistung der Personen, die bereits eingegeben:

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

Hier wird die Funktionsweise erläutert: der erste Benutzer drückt die **neu** Schaltfläche. Dies ermöglicht es das Eintragsformular, aber deaktiviert die **neu** Schaltfläche. Der Benutzer gibt dann einen Namen, Alter und Fähigkeiten. Der Benutzer kann zu einem beliebigen Zeitpunkt während der Bearbeitung, drücken die **"Abbrechen"** Schaltfläche beginnen. Nur wenn ein Name und ein gültiges Alter eingegeben wurden die **Absenden** Schaltfläche aktiviert. Drücken dies **Absenden** überträgt die Person, die auf die Auflistung, die angezeigt werden, indem Sie die `ListView`. Entweder nach dem die **"Abbrechen"** oder **Absenden** gedrückt wird, wird das Eintragsformular gelöscht und die **neu** Schaltfläche erneut aktiviert wird.

Die iOS-Bildschirm auf der linken Seite zeigt das Layout, bevor ein gültiges Alter eingegeben wird. Der Android- und uwp-Bildschirme zeigen die **Absenden** Schaltfläche aktiviert, nachdem ein Alter festgelegt wurde:

[![Person Eintrag](commanding-images/personentry-small.png "Person Eintrag")](commanding-images/personentry-large.png "Person Eintrag")

Die Anwendung verfügt nicht über Funktion zum Bearbeiten vorhandener Einträge und die Einträge werden nicht gespeichert werden, wenn Sie die Seite verlassen.

Die Logik für die **neu**, **Absenden**, und **"Abbrechen"** Schaltflächen erfolgt `PersonCollectionViewModel` über Definitionen von der `NewCommand`, `SubmitCommand`, und `CancelCommand` Eigenschaften. Der Konstruktor, der die `PersonCollectionViewModel` legt diese drei Eigenschaften auf Objekte vom Typ `Command`.  

Ein [Konstruktor](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Command.Command/p/System.Action/System.Func%7BSystem.Boolean%7D/) von der `Command` -Klasse ermöglicht Ihnen das Übergeben von Argumenten des Typs `Action` und `Func<bool>` entspricht der `Execute` und `CanExecute` Methoden. Es ist am einfachsten so definieren Sie diese Aktionen und Funktionen als Lambda-Ausdruck Funktionen direkt in die `Command` Konstruktor. Hier ist die Definition der `Command` -Objekt für die `NewCommand` Eigenschaft:

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

Klickt der Benutzer die **neu** Schaltfläche, die `execute` Funktion übergeben, um die `Command` Konstruktor ausgeführt wird. Dies erstellt ein neues `PersonViewModel` Objekt, legt einen Handler für dieses Objekts `PropertyChanged` Ereignis legt `IsEditing` auf `true`, und ruft die `RefreshCanExecutes` Methode nach dem Konstruktor definiert.

Neben dem Implementieren der `ICommand` -Schnittstelle, die `Command` -Klasse definiert außerdem eine Methode namens `ChangeCanExecute`. Sollten Ihre ViewModel Aufrufen `ChangeCanExecute` für eine `ICommand` Eigenschaft immer alles in diesem Fall kann den Rückgabewert der ändern die `CanExecute` Methode. Ein Aufruf von `ChangeCanExecute` bewirkt, dass die `Command` -Klasse zum Auslösen der `CanExecuteChanged` Methode. Die `Button` einen Handler für dieses Ereignis wurde angefügt und durch den Aufruf antwortet `CanExecute` erneut, und aktivieren basierend auf den Rückgabewert der Methode.

Wenn die `execute` Methode `NewCommand` Aufrufe `RefreshCanExecutes`, die `NewCommand` Eigenschaft ruft einen Aufruf von `ChangeCanExecute`, und die `Button` Aufrufe der `canExecute` Methode, die jetzt zurückgibt `false` da die `IsEditing`Eigenschaft ist mittlerweile `true`.

Die `PropertyChanged` Handler für das neue `PersonViewModel` -Objekt ruft die `ChangeCanExecute` Methode `SubmitCommand`. Hier ist, wie diese Befehlseigenschaft implementiert wird:


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

Die `canExecute` für Funktion `SubmitCommand` wird jedes Mal aufgerufen, ist eine Eigenschaft geändert, der `PersonViewModel` Objekt bearbeitet wird. Es gibt `true` nur, wenn die `Name` Eigenschaft ist mindestens ein Zeichen lang sein, und `Age` ist größer als 0. Zu diesem Zeitpunkt wird die **Absenden** Schaltfläche aktiviert wird. 

Die `execute` für Funktion **senden** entfernt den durch geänderte Eigenschaften ausgelöste Handler aus der `PersonViewModel`, fügt das Objekt, das die `Persons` -Auflistung, und alles, was zum anfänglichen Bedingungen zurückgegeben.

Die `execute` -Funktion für die **"Abbrechen"** Schaltfläche wird alles, was das der **Absenden** Schaltfläche tut Execept das Objekt der Sammlung hinzufügen:

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

Die `canExecute` -Methode zurückkehrt `true` zu einem beliebigen Zeitpunkt eine `PersonViewModel` bearbeitet wird.

Dieser Techniken konnte für komplexere Szenarien angepasst werden: eine Eigenschaft in `PersonCollectionViewModel` konnte gebunden werden, um die `SelectedItem` Eigenschaft der `ListView` für die Bearbeitung von vorhandener Elementen und ein **löschen** Schaltfläche hinzugefügt werden konnte, löschen Diese Elemente.

Es ist nicht erforderlich, Sie definieren die `execute` und `canExecute` Methoden als Lambda-Funktionen. Schreiben sie als reguläre private Methoden im ViewModel können die und verweisen sie in der `Command` Konstruktoren. Dieser Ansatz jedoch tendieren dazu, zu viele Methoden, die nur einmal in das ViewModel verwiesen wird.

## <a name="using-command-parameters"></a>Verwenden die Befehlsparameter

Dies kann manchmal praktisch sein, für eine oder mehrere Schaltflächen (oder andere Objekte der Benutzeroberfläche), die denselben `ICommand` Eigenschaft im ViewModel. In diesem Fall verwenden Sie die `CommandParameter` Eigenschaft zur Unterscheidung zwischen den Schaltflächen. 

Sie können weiterhin die `Command` Klasse für diese freigegebenen `ICommand` Eigenschaften. Die Klasse definiert ein [alternative Konstruktor](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Command.Command/p/System.Action%7BSystem.Object%7D/System.Func%7BSystem.Object,System.Boolean%7D/) nimmt `execute` und `canExecute` Methoden mit Parametern vom Typ `Object`. Dies ist wie die `CommandParameter` wird an diese Methoden übergeben.

Allerdings bei Verwendung `CommandParameter`, es ist am einfachsten, die generischen [ `Command<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command%3CT%3E/) Klasse, um den Typ des Objekts festgelegt werden, um anzugeben `CommandParameter`. Die `execute` und `canExecute` Methoden, die Sie angeben, Parameter dieses Typs aufweisen.

Die **Decimal Tastatur** Seite veranschaulicht dieses Verfahren erfahren, wie eine Zehnertastatur zum Eingeben von Dezimalzahlen implementieren. Die `BindingContext` für die `Grid` ist eine `DecimalKeypadViewModel`. Die `Entry` Eigenschaft von diesem ViewModel gebunden ist die `Text` Eigenschaft von einem `Label`. Alle der `Button` Objekte auf verschiedenen Befehle im ViewModel gebunden sind: `ClearCommand`, `BackspaceCommand`, und `DigitCommand`:

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

Die 11 Schaltflächen für die 10 Ziffern und das Dezimaltrennzeichen gemeinsam nutzen, eine Bindung an `DigitCommand`. Die `CommandParameter` unterscheidet zwischen dieser Schaltflächen. Der Wert auf `CommandParameter` sind im Allgemeinen als Text angezeigt, die für die Schaltfläche außer dem Dezimaltrennzeichen, die zum Zweck der Übersichtlichkeit mit einem Punkt in der Mitte angezeigt wird.

Hier wird die Anwendung in Aktion:

[![Decimal Tastatur](commanding-images/decimalkeyboard-small.png "Decimal Tastatur")](commanding-images/decimalkeyboard-large.png "Decimal Tastatur")

Beachten Sie, dass die Schaltfläche für das Dezimaltrennzeichen in alle drei Screenshots deaktiviert ist, da die eingegebene Nummer bereits ein Dezimaltrennzeichen enthält. 

Die `DecimalKeypadViewModel` definiert ein `Entry` Eigenschaft vom Typ `string` (Dies ist die einzige Eigenschaft, die Trigger ein `PropertyChanged` Ereignis) und drei Eigenschaften des Typs `ICommand`:

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

Da die Schaltfläche "" immer aktiviert ist, ist es nicht notwendig, geben Sie einen `canExecute` Argument in der `Command` Konstruktor.

Die Logik zum Eingeben von Zahlen ein, und Drücken der RÜCKTASTE ist ein bisschen, da keine Ziffern eingegeben wurde, und klicken Sie dann die `Entry` Eigenschaft ist die Zeichenfolge "0". Wenn der Benutzer weitere Nullen (0) gibt die `Entry` noch enthält nur eine 0 (null). Wenn der Benutzer eine beliebige andere Ziffer eingibt, wird diese Ziffer der 0 (null) ersetzt. Aber wenn der Benutzer dann einem Dezimaltrennzeichen, bevor Sie eine beliebige andere Ziffer eingibt `Entry` ist die Zeichenfolge "0".

Die **RÜCKTASTE** Schaltfläche aktiviert ist, nur, wenn die Länge des Eintrags größer als 1 ist, oder wenn `Entry` stimmt nicht mit der Zeichenfolge "0":

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

Die Logik für die `execute` -Funktion für die **RÜCKTASTE** Schaltfläche wird sichergestellt, dass die `Entry` ist mindestens eine Zeichenfolge mit "0".

Die `DigitCommand` Eigenschaft gebunden ist 11 Schaltflächen, von denen jede identifiziert sich selbst bei der `CommandParameter` Eigenschaft. Die `DigitCommand` konnte mit einer Instanz von normalen festgelegt werden `Command` -Klasse, sondern die einfacher zu verwenden, die `Command<T>` generische Klasse. Bei Verwendung von XAML, die Befehle Schnittstelle mit dem `CommandParameter` Eigenschaften werden in der Regel Zeichenfolgen, und das ist der Typ, der dem generischen Argument. Die `execute` und `canExecute` dann haben Funktionen Argumente des Typs `string`:

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

Die `execute` Methode fügt das Zeichenfolgenargument für die `Entry` Eigenschaft. Jedoch wenn das Ergebnis mit einer NULL (aber keine 0 (null) und ein Dezimaltrennzeichen) beginnt klicken Sie dann diese anfängliche 0 (null) entfernt werden, mithilfe der `Substring` Funktion.

Die `canExecute` -Methode zurückkehrt `false` nur, wenn das Argument das Dezimaltrennzeichen ist (angibt, dass das Dezimaltrennzeichen gedrückt wird) und `Entry` enthält bereits ein Dezimaltrennzeichen. 

Alle der `execute` -Methode `RefreshCanExecutes`, wodurch wiederum `ChangeCanExecute` für beide `DigitCommand` und `ClearCommand`. Dadurch wird sichergestellt, dass das Dezimaltrennzeichen und RÜCKTASTE Schaltflächen aktiviert werden oder deaktiviert auf der aktuellen Sequenz von eingegebenen Ziffern Grundlage.

## <a name="adding-commands-to-existing-views"></a>Hinzufügen von Befehlen an vorhandenen Ansichten

Wenn Sie die Befehle Schnittstelle mit Ansichten verwenden, die er nicht unterstützen möchten, ist es möglich, ein Verhalten Xamarin.Forms verwenden, die ein Ereignis in einem Befehl konvertiert. Dies wird im Artikel beschriebenen [ **Wiederverwendbaren EventToCommandBehavior**](~/xamarin-forms/app-fundamentals/behaviors/reusable/event-to-command-behavior.md).

## <a name="asynchronous-commanding-for-navigation-menus"></a>Asynchrone Befehle für Navigationsmenüs

Befehle eignet sich für die Implementierung von Navigationsmenüs, z. B., dass in der [ **Data Binding Demos** ](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/) Programm selbst. Hier ist Bestandteil des **"MainPage.xaml"**:


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

Bei Verwendung der Befehle, die mit XAML `CommandParameter` Eigenschaften werden in der Regel auf Zeichenfolgen festgelegt. Wird in diesem Fall jedoch eine XAML-Markuperweiterung verwendet, damit die `CommandParameter` ist vom Typ `System.Type`.

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

Der Standardkonstruktor legt die `NavigateCommand` Eigenschaft, um eine `execute` -Methode, die instanziiert die `System.Type` Parameter und wechselt anschließend darauf. Da die `PushAsync` Aufruf erfordert eine `await` -Operator, der `execute` Methode muss als asynchrones gekennzeichnet werden. Dies wird erreicht, mit der `async` -Schlüsselwort vor der Parameterliste. 

Zudem wird der Konstruktor der `BindingContext` Rand der Seite auf sich selbst, damit die Bindungen verweisen die `NavigateCommand` in dieser Klasse.

Die Reihenfolge des Codes in diesem Konstruktor macht einen Unterschied: die `InitializeComponent` Aufruf bewirkt, dass den XAML-Code analysiert werden, jedoch zu diesem Zeitpunkt die Bindung an eine Eigenschaft mit dem Namen `NavigateCommand` kann nicht aufgelöst werden, da `BindingContext` auf festgelegt ist `null`. Wenn die `BindingContext` wird im Konstruktor festgelegt *vor* `NavigateCommand` festgelegt ist, und klicken Sie dann die Bindung aufgelöst, wenn werden kann `BindingContext` festgelegt ist, jedoch zu diesem Zeitpunkt `NavigateCommand` ist immer noch `null`. Festlegen von `NavigateCommand` nach `BindingContext` hat keine Auswirkungen auf die Bindung aus, da eine Änderung an `NavigateCommand` wird nicht ausgelöst eine `PropertyChanged` Ereignis und die Bindung nicht kennen, `NavigateCommand` ist jetzt gültig.

Festlegen von sowohl `NavigateCommand` und `BindingContext` (in beliebiger Reihenfolge) vor dem Aufruf von `InitializeComponent` funktionieren, da beide Komponenten der Bindung festgelegt werden, wenn der XAML-Parser die Bindungsdefinition feststellt. 

Datenbindungen können manchmal schwierig sein, wie Sie in dieser Reihe von Artikeln gesehen haben, sie werden jedoch leistungsfähigen und vielseitigen, und helfen, die zum Organisieren von Code durch die Trennung der zugrunde liegende Logik von der Benutzeroberfläche.



## <a name="related-links"></a>Verwandte Links

- [Binden von Daten-Demos (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Daten Bindung Kapitel Xamarin.Forms Buchs](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter18.md)
