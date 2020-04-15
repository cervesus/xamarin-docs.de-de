---
title: Die Befehlsschnittstelle in Xamarin.Forms
description: In diesem Artikel wird das Implementieren der Command-Eigenschaft mit Xamarin.Forms-Datenbindungen erläutert. Über die Befehlsschnittstelle kann jedoch ein alternativer Ansatz für das Implementieren von Befehlen verwendet werden, der für die MVVM-Architektur besser geeignet ist.
ms.prod: xamarin
ms.assetid: 69922284-F398-45C3-B4CC-B8E29BB4C533
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
ms.openlocfilehash: 185aebf48b24a6abbdd8f56dbbfc32f6e99f6e63
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2020
ms.locfileid: "75545594"
---
# <a name="the-xamarinforms-command-interface"></a>Die Befehlsschnittstelle in Xamarin.Forms

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/databindingdemos)

In der MVVM-Architektur (Model View ViewModel) werden Datenbindungen zwischen den Eigenschaften in ViewModel (üblicherweise eine von `INotifyPropertyChanged` abgeleitete Klasse) und den Eigenschaften in View (üblicherweise die XAML-Datei) definiert. Manchmal ist für eine Anwendung mehr als Eigenschaftenbindungen erforderlich, beispielsweise, wenn der Benutzer Befehle initiieren muss, die Auswirkungen auf ViewModel haben. Diese Befehle werden in der Regel ausgeführt, indem Sie auf Schaltflächen klicken oder auf den Bildschirm tippen. Normalerweise werden sie in der CodeBehind-Datei in einem Handler für das `Clicked`-Ereignis von `Button` oder das `Tapped`-Ereignis von `TapGestureRecognizer` verarbeitet.

Über die Befehlsschnittstelle kann jedoch ein alternativer Ansatz für das Implementieren von Befehlen verwendet werden, der für die MVVM-Architektur besser geeignet ist. Die ViewModel-Klasse kann Befehle enthalten, bei denen es sich um Methoden handelt, die als Reaktion auf eine bestimmte Aktivität in der View-Klasse (z.B. ein Klick auf ein `Button`-Element) ausgeführt werden. Datenbindungen werden zwischen diesen Befehlen und `Button` definiert.

Damit eine Datenbindung zwischen `Button` und einer ViewModel-Klasse möglich ist, definiert `Button` zwei Eigenschaften:

- [`Command`](xref:Xamarin.Forms.Button.Command) vom Typ [`System.Windows.Input.ICommand`](xref:System.Windows.Input.ICommand)
- [`CommandParameter`](xref:Xamarin.Forms.Button.CommandParameter) vom Typ `Object`

Wenn Sie die Befehlsschnittstelle verwenden möchten, müssen Sie eine Datenbindung definieren, die die `Command`-Eigenschaft von `Button` anzielt. Hierbei ist die Quelle eine Eigenschaft vom Typ `ICommand` in der ViewModel-Klasse. Die ViewModel-Klasse enthält Code, der der `ICommand`-Eigenschaft zugeordnet ist, die beim Klicken der Schaltfläche ausgeführt wird. Sie können `CommandParameter` auf beliebige Daten festlegen, um zwischen mehreren Schaltflächen zu unterscheiden, wenn diese an die gleiche `ICommand`-Eigenschaft in der ViewModel-Klasse gebunden sind.

Die Eigenschaften `Command` und `CommandParameter` werden ebenfalls durch folgende Klassen definiert:

- [`MenuItem`](xref:Xamarin.Forms.MenuItem) und daher [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem), die aus `MenuItem` abgeleitet wird
- [`TextCell`](xref:Xamarin.Forms.TextCell) und daher [`ImageCell`](xref:Xamarin.Forms.ImageCell), die aus `TextCell` abgeleitet wird
- [`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer)

[`SearchBar`](xref:Xamarin.Forms.SearchBar) definiert eine [`SearchCommand`](xref:Xamarin.Forms.SearchBar.SearchCommand)-Eigenschaft vom Typ `ICommand` und eine [`SearchCommandParameter`](xref:Xamarin.Forms.SearchBar.SearchCommandParameter)-Eigenschaft. Die [`RefreshCommand`](xref:Xamarin.Forms.ListView.RefreshCommand)-Eigenschaft von [`ListView`](xref:Xamarin.Forms.ListView) weist ebenfalls den Typ `ICommand` auf.

Diese Befehle können innerhalb einer ViewModel-Klasse auf eine Weise verarbeitet werden, die nicht vom entsprechenden Benutzeroberflächenobjekt der View-Klasse abhängig ist.

## <a name="the-icommand-interface"></a>Die ICommand-Schnittstelle

Die [`System.Windows.Input.ICommand`](xref:System.Windows.Input.ICommand)-Schnittstelle ist nicht in Xamarin.Forms enthalten. Sie wird stattdessen im Namespace [System.Windows.Input](xref:System.Windows.Input) definiert und besteht aus zwei Methoden und einem Ereignis:

```csharp
public interface ICommand
{
    public void Execute (Object parameter);

    public bool CanExecute (Object parameter);

    public event EventHandler CanExecuteChanged;
}
```

Ihre ViewModel-Klasse enthält Eigenschaften vom Typ `ICommand`, damit die Befehlsschnittstelle verwendet werden kann:

```csharp
public ICommand MyCommand { private set; get; }
```

Die ViewModel-Klasse muss zudem auf eine Klasse verweisen, die die `ICommand`-Schnittstelle implementiert. Diese Klasse wird noch erläutert. In der View-Klasse ist die `Command`-Eigenschaft von `Button` an diese Eigenschaft gebunden:

```xaml
<Button Text="Execute command"
        Command="{Binding MyCommand}" />
```

Wenn der Benutzer das `Button`-Element anklickt, ruft `Button` die `Execute`-Methode im `ICommand`-Objekt auf, das an dessen `Command`-Eigenschaft gebunden ist. Das ist der einfachste Teil der Befehlsschnittstelle.

Die `CanExecute`-Methode ist komplexer. Wenn die Bindung zuerst in der `Command`-Eigenschaft von `Button` definiert wird und die Datenbindung sich ändert, ruft `Button` die `CanExecute`-Methode im `ICommand`-Objekt auf. Wenn `CanExecute``false` zurückgibt, deaktiviert `Button` sich selbst. Das bedeutet, dass der entsprechende Befehl aktuell nicht verfügbar oder ungültig ist.

`Button` fügt ebenfalls einen Handler an das `CanExecuteChanged`-Ereignis von `ICommand` an. Das Ereignis wird innerhalb der ViewModel-Klasse ausgelöst. Wenn das Ereignis ausgelöst wird, ruft `Button``CanExecute` erneut auf. Das `Button`-Element aktiviert sich selbst, wenn `CanExecute``true` zurückgibt, und es deaktiviert sich selbst, wenn `CanExecute``false` zurückgibt.

> [!IMPORTANT]
> Verwenden Sie die `IsEnabled`-Eigenschaft von `Button` nicht, wenn Sie die Befehlsschnittstelle verwenden.  

## <a name="the-command-class"></a>Die Command-Klasse

Wenn Ihre ViewModel-Klasse eine Eigenschaft vom Typ `ICommand` definiert, muss diese ebenfalls eine Klasse enthalten oder auf eine Klasse verweisen, die die `ICommand`-Schnittstelle implementiert. Diese Klasse muss die `Execute`- und `CanExecute`-Methoden enthalten oder auf diese verweisen und das `CanExecuteChanged`-Ereignis auslösen, wenn die `CanExecute`-Methode einen unterschiedlichen Wert zurückgibt.

Sie können diese Klasse selbst schreiben oder eine Klasse verwenden, die von einer anderen Person erstellt wurde. Da die `ICommand`-Schnittstelle in Microsoft Windows enthalten ist, wird sie seit Jahren mit MVVM-Anwendungen für Windows verwendet. Wenn Sie eine Windows-Klasse verwenden, die `ICommand` implementiert, können Sie Ihre ViewModel-Klassen für Windows- und Xamarin.Forms-Anwendungen freigeben.

Wenn das Freigeben von ViewModel-Klassen zwischen Windows und Xamarin.Forms für Sie nicht relevant ist, können Sie die [`Command`](xref:Xamarin.Forms.Command)- oder [`Command<T>`](xref:Xamarin.Forms.Command`1)-Klasse verwenden, die in Xamarin.Forms enthalten ist, um die `ICommand`-Schnittstelle zu implementieren. Durch diese Klassen können Sie den Text der `Execute`- und `CanExecute`-Methoden in den Konstruktoren der Klassen festlegen. Verwenden Sie `Command<T>`, wenn Sie die `CommandParameter`-Eigenschaft verwenden, um zwischen mehreren Ansichten zu unterscheiden, die an die gleiche `ICommand`-Eigenschaft gebunden sind. Verwenden Sie die einfachere `Command`-Klasse, wenn dies nicht erforderlich ist.

## <a name="basic-commanding"></a>Grundlegende Befehle

Die Seite **Person Entry** im Beispielprogramm für [**Datenbindungen**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/databindingdemos) veranschaulicht einige einfache Befehle, die in eine ViewModel-Klasse implementiert werden.

Die `PersonViewModel`-Klasse definiert drei Eigenschaften namens `Name`, `Age` und `Skills`, über die eine Person definiert wird. Diese Klasse enthält *keine*`ICommand`-Eigenschaften:

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

Die unten dargestellte `PersonCollectionViewModel`-Klasse erstellt neue Objekte vom Typ `PersonViewModel` und ermöglicht es dem Benutzer, Daten einzugeben. Hierfür definiert die Klasse`IsEditing`-Eigenschaften vom Typ `bool` und `PersonEdit`-Eigenschaften vom Typ `PersonViewModel`. Darüber hinaus definiert die Klasse drei Eigenschaften vom Typ `ICommand` und eine Eigenschaft namens `Persons` vom Typ `IList<PersonViewModel>`:

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

In dieser abgekürzten Auflistung ist der Konstruktor der Klasse nicht enthalten. In diesem werden drei Eigenschaften vom Typ `ICommand` definiert, die noch erläutert werden. Beachten Sie, dass Änderungen an den drei Eigenschaften vom Typ `ICommand` und an der `Persons`-Eigenschaft nicht dazu führen, dass `PropertyChanged`-Ereignisse ausgelöst werden. Diese Eigenschaften werden festgelegt, wenn die Klasse erstellt wird und ändern sich nicht mehr.

Bevor Sie den Konstruktor der `PersonCollectionViewModel`-Klasse untersuchen, sollten Sie sich die XAML-Datei des Programms **Person Entry** ansehen. Diese enthält ein `Grid`-Objekt mit einer `BindingContext`-Eigenschaft, die auf `PersonCollectionViewModel` festgelegt ist. `Grid` enthält ein `Button`-Element mit dem Text **New** (Neu), dessen `Command`-Eigenschaft an die `NewCommand`-Eigenschaft in ViewModel gebunden ist, ein Eingabeformular mit Eigenschaften, die an die `IsEditing`-Eigenschaft gebunden sind, Eigenschaften von `PersonViewModel` und zwei weitere Schaltflächen, die an die `SubmitCommand`- und `CancelCommand`-Eigenschaften von ViewModel gebunden sind. Die endgültige `ListView`-Klasse zeigt eine Collection der Personen an, die bereits eingegeben wurden:

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

Dieser Vorgang funktioniert folgendermaßen: Der Benutzer drückt zunächst auf die Schaltfläche **New** (Neu). Dadurch wird das Eingabeformular aktiviert, aber die Schaltfläche **New** (Neu) wird deaktiviert. Der Benutzer gibt dann einen Namen, ein Alter und seine Qualifikationen ein. Während der Bearbeitung kann der Benutzer jederzeit auf **Cancel** (Abbrechen) drücken, um von vorne zu beginnen. Die Schaltfläche **Submit** (Senden) wird nur aktiviert, wenn ein Name und ein gültiges Alter eingegeben wurden. Wenn auf die Schaltfläche **Submit** (Senden) gedrückt wird, wird die Person an die Collection übertragen, die von `ListView` angezeigt wird. Nachdem auf **Cancel** (Abbrechen) oder **Submit** (Senden) gedrückt wurde, wird das Eingabeformular zurückgesetzt, und die Schaltfläche **New** (Neu) wird wieder aktiviert.

Auf dem iOS-Bildschirm auf der linken Seite sehen Sie das Layout, bevor ein gültiges Alter eingegeben wurde. Auf dem Android-Bildschirm wird die Schaltfläche **Submit** (Senden) angezeigt, die aktiviert wurde, nachdem ein Alter festgelegt wurde:

[![Personeneintrag](commanding-images/personentry-small.png "Personeneintrag")](commanding-images/personentry-large.png#lightbox "Personeneintrag")

Über das Programm können vorhandene Einträge nicht bearbeitet werden, und die Einträge werden nicht gespeichert, wenn Sie zu einer anderen Seite navigieren.

Die Logik für die Schaltflächen **New**, **Submit** und **Cancel** wird in `PersonCollectionViewModel` über die Definitionen der `NewCommand`-, `SubmitCommand`- und `CancelCommand`-Eigenschaften verarbeitet. Der Konstruktor von `PersonCollectionViewModel` legt diese drei Eigenschaften auf Objekte vom Typ `Command` fest.  

Ein [Konstruktor](xref:Xamarin.Forms.Command.%23ctor(System.Action,System.Func{System.Boolean})) der `Command`-Klasse ermöglicht das Übergeben von Argumenten vom Typ `Action` und `Func<bool>` den `Execute`- und `CanExecute`-Methoden entsprechend. Es ist am einfachsten, diese Aktionen und Funktionen als Lambdafunktionen im `Command`-Konstruktor zu definieren. Hier finden Sie die Definition des `Command`-Objekts für die `NewCommand`-Eigenschaft:

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

Wenn der Benutzer auf die Schaltfläche **New** klickt, wird die `execute`-Funktion ausgeführt, die an den `Command`-Konstruktor übergeben wurde. Dadurch wird ein neues `PersonViewModel`-Objekt erstellt, ein Handler für das `PropertyChanged`-Ereignis des Objekts festgelegt, `IsEditing` wird auf `true` festgelegt, und die `RefreshCanExecutes`-Methode wird aufgerufen, die nach dem Konstruktor definiert wurde.

Die `Command`-Klasse implementiert nicht nur die `ICommand`-Schnittstelle, sondern definiert auch eine Methode namens `ChangeCanExecute`. Ihre ViewModel-Klasse sollte `ChangeCanExecute` für eine `ICommand`-Eigenschaft aufrufen, wenn ein Vorgang den Rückgabewert der `CanExecute`-Methode ändern könnte. Durch einen Aufruf von `ChangeCanExecute` löst die `Command`-Klasse die `CanExecuteChanged`-Methode aus. An das `Button`-Element ist ein Handler für dieses Ereignis angefügt, und das Element reagiert, indem `CanExecute` erneut aufgerufen wird. Anschließend aktiviert es sich je nach Rückgabewert der Methode selbst.

Wenn die `execute`-Methode von `NewCommand``RefreshCanExecutes` aufruft, erhält die `NewCommand`-Eigenschaft einen Aufruf von `ChangeCanExecute`, und `Button` ruft die `canExecute`-Methode auf, die nun `false` zurückgibt, da die `IsEditing`-Eigenschaft nun den Wert `true` aufweist.

Der `PropertyChanged`-Handler für das neue `PersonViewModel`-Objekt ruft die `ChangeCanExecute`-Methode von `SubmitCommand` auf. Hier sehen Sie, wie diese Befehlseigenschaft implementiert wird:

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

Die `canExecute`-Funktion für `SubmitCommand` wird immer dann aufgerufen, wenn sich eine Eigenschaft in dem `PersonViewModel`-Objekt ändert, das bearbeitet wird. `true` wird nur zurückgegeben, wenn die `Name`-Eigenschaft mindestens ein Zeichen lang und `Age` größer als 0 ist. In diesem Fall wird die Schaltfläche **Submit** (Senden) aktiviert.

Die `execute`-Funktion für **Submit** entfernt den Handler mit der geänderten Eigenschaft aus `PersonViewModel`, fügt das Objekt zur `Persons`-Collection hinzu und setzt die Oberfläche auf die anfänglichen Bedingungen zurück.

Die `execute`-Funktion für die Schaltfläche **Cancel** (Abbrechen) funktioniert wie die Schaltfläche **Submit** (Senden), das Objekt wird allerdings nicht zur Sammlung hinzugefügt:

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

Die `canExecute`-Methode gibt `true` immer dann zurück, wenn `PersonViewModel` bearbeitet wird.

Diese Technik kann auch auf komplexere Szenarios angewendet werden: Eine Eigenschaft in `PersonCollectionViewModel` könnte an die `SelectedItem`-Eigenschaft von `ListView` gebunden werden, wenn vorhandene Elemente bearbeitet werden. Zudem könnte die Schaltfläche **Delete** (Löschen) hinzugefügt werden, um diese Elemente zu löschen.

Es ist nicht erforderlich, die `execute`- und `canExecute`-Methoden als Lambdafunktionen zu definieren. Sie können diese als reguläre private Methoden in ViewModel schreiben und in den `Command`-Konstruktoren auf diese verweisen. Dieser Ansatz führt jedoch häufig dazu, dass viele Methoden in ViewModel vorhanden sind, auf die nur einmal verwiesen wird.

## <a name="using-command-parameters"></a>Verwenden von CommandParameter-Eigenschaften

Häufig erweist es sich als praktisch, wenn mehrere Schaltflächen (oder andere Benutzeroberflächenobjekte) die gleiche `ICommand`-Eigenschaft in ViewModel verwenden. In diesem Fall verwenden Sie die `CommandParameter`-Eigenschaft, um zwischen diesen Schaltflächen zu unterscheiden.

Sie können weiterhin die `Command`-Klasse für diese freigegebenen `ICommand`-Eigenschaften verwenden. Die Klasse definiert einen [alternativen Konstruktor](xref:Xamarin.Forms.Command.%23ctor(System.Action{System.Object},System.Func{System.Object,System.Boolean})), der `execute`- und `canExecute`-Methoden mit Parametern vom Typ `Object` akzeptiert. Auf diese Weise wird `CommandParameter` an diese Methoden übergeben.

Wenn Sie `CommandParameter` verwenden, ist es jedoch einfacher, die generische [`Command<T>`](xref:Xamarin.Forms.Command`1)-Klasse zu verwenden, um den Typ des Objekts anzugeben, das auf `CommandParameter` festgelegt ist. Die von Ihnen angegebenen `execute`- und `canExecute`-Methoden enthalten Parameter dieses Typs.

Auf der Seite **Decimal Keyboard** wird diese Technik anhand der Implementierung einer Zehnertastatur für die Eingabe von Dezimalzahlen veranschaulicht. Die `BindingContext`-Klasse für `Grid` entspricht `DecimalKeypadViewModel`. Die `Entry`-Eigenschaft dieser ViewModel-Klasse ist an die `Text`-Eigenschaft von `Label` gebunden. Alle `Button`-Objekte sind an verschiedene Befehle in der ViewModel-Klasse gebunden: `ClearCommand`, `BackspaceCommand` und `DigitCommand`:

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

Die 11 Schaltflächen für die 10 Ziffern und das Dezimaltrennzeichen sind an `DigitCommand` gebunden. `CommandParameter` unterscheidet zwischen diesen Schaltflächen. Der auf `CommandParameter` festgelegte Wert entspricht in der Regel dem Text, der auf der Schaltfläche angezeigt wird. Das Dezimaltrennzeichen stellt eine Ausnahme dar, denn es wird aus Gründen der Übersichtlichkeit als Punkt in der Mitte dargestellt.

Hier sehen Sie das Programm bei der Ausführung:

[![Dezimaltastatur](commanding-images/decimalkeyboard-small.png "Dezimaltastatur")](commanding-images/decimalkeyboard-large.png#lightbox "Dezimaltastatur")

Beachten Sie, dass die Schaltfläche für das Dezimaltrennzeichen auf allen drei Screenshots deaktiviert ist, weil die eingegebene Zahl bereits ein Dezimaltrennzeichen enthält.

`DecimalKeypadViewModel` definiert eine `Entry`-Eigenschaft vom Typ `string` (die einzige Eigenschaft, die ein `PropertyChanged`-Ereignis auslöst) und drei Eigenschaften vom Typ `ICommand`:

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

Die Schaltfläche für `ClearCommand` ist immer aktiviert und setzt den Eintrag auf „0“ zurück:

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

Da diese Schaltfläche immer aktiviert ist, ist es nicht notwendig, ein `canExecute`-Argument im `Command`-Konstruktor anzugeben.

Die Logik für das Eingeben von Zahlen und das Drücken der RÜCKTASTE ist ein wenig kompliziert, denn wenn keine Ziffern eingegeben wurden, enthält die `Entry`-Eigenschaft die Zeichenfolge „0“. Wenn der Benutzer weitere Nullen eingibt, enthält `Entry` weiterhin nur eine Null. Wenn der Benutzer eine weitere Ziffer eingibt, ersetzt diese die Null. Wenn der Benutzer jedoch ein Dezimaltrennzeichen vor einer weiteren Ziffer eingibt, entspricht `Entry` der Zeichenfolge „0.“.

Die **RÜCKTASTE** wird nur aktiviert, wenn die Länge des Eintrags größer als 1 ist oder wenn `Entry` nicht der Zeichenfolge „0“ entspricht:

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

Die Logik für die `execute`-Funktion der **RÜCKTASTE** stellt sicher, dass `Entry` mindestens die Zeichenfolge „0“ enthält.

Die `DigitCommand`-Eigenschaft ist an 11 Schaltflächen gebunden, von denen jede über die `CommandParameter`-Eigenschaft identifiziert wird. `DigitCommand` könnte auf eine Instanz der regulären `Command`-Klasse festgelegt werden. Es ist jedoch einfacher, die generische `Command<T>`-Klasse zu verwenden. Wenn Sie die Befehlsschnittstelle mit XAML verwenden, handelt es sich bei den `CommandParameter`-Eigenschaften üblicherweise um Zeichenfolgen. Diese stellen auch den Typ des generischen Arguments dar. Die Funktionen `execute` und `canExecute` enthalten dann Argumente vom Typ `string`:

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

Die `execute`-Methode fügt das Zeichenfolgenargument zur `Entry`-Eigenschaft hinzu. Wenn das Ergebnis jedoch mit einer Null beginnt (nicht jedoch mit einer Null und einem Dezimaltrennzeichen), muss diese erste Null mithilfe der `Substring`-Funktion entfernt werden.

Die `canExecute`-Methode gibt `false` nur zurück, wenn das Argument das Dezimaltrennzeichen ist (d.h. es gibt an, dass das Dezimaltrennzeichen betätigt wird) und `Entry` bereits ein Dezimaltrennzeichen enthält.

Alle `execute`-Methoden rufen die `RefreshCanExecutes`-Methode auf, die dann `ChangeCanExecute` für `DigitCommand` und `ClearCommand` aufruft. Dadurch wird sichergestellt, dass das Dezimaltrennzeichen und die RÜCKTASTE je nachdem, welche Ziffern eingegeben wurden, aktiviert oder deaktiviert werden.

## <a name="adding-commands-to-existing-views"></a>Hinzufügen von Befehlen zu vorhandenen Ansichten

Wenn Sie die Befehlsschnittstelle für Ansichten verwenden möchten, die diese nicht unterstützen, können Sie ein Xamarin.Forms-Verhalten verwenden, das ein Ereignis in einen Befehl konvertiert. Dieser Vorgang wird im Artikel [**Reusable EventToCommandBehavior (Wiederverwendbare EventToCommandBehavior-Klasse)** ](~/xamarin-forms/app-fundamentals/behaviors/reusable/event-to-command-behavior.md) erläutert.

## <a name="asynchronous-commanding-for-navigation-menus"></a>Asynchrone Befehle für Navigationsmenüs

Befehle sind für die Implementierung von Navigationsmenüs gut geeignet und werden auch im [**Beispielprogramm für Datenbindungen**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/databindingdemos) verwendet. Hier sehen Sie einen Teil von **MainPage.xaml**:

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

Wenn Sie Befehle mit XAML verwenden, sind `CommandParameter`-Eigenschaften in der Regel auf Zeichenfolgen festgelegt. In diesem Fall wird jedoch eine XAML-Markuperweiterung verwendet, damit `CommandParameter` dem Typ `System.Type` entspricht.

Jede `Command`-Eigenschaft ist an eine Eigenschaft namens `NavigateCommand` gebunden. Diese Eigenschaft wird in der CodeBehind-Datei (**MainPage.xaml.cs**) definiert:

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

Der Konstruktor legt die `NavigateCommand`-Eigenschaft auf eine `execute`-Methode fest, die den `System.Type`-Parameter instanziiert und dann zu diesem navigiert. Da für den Aufruf von `PushAsync` ein `await`-Operator erforderlich ist, muss die `execute`-Methode als asynchron gekennzeichnet sein. Dies erfolgt über das `async`-Schlüsselwort, das vor der Parameterliste eingefügt wird.

Der Konstruktor legt die `BindingContext`-Klasse der Seite auf sich selbst fest, sodass die Bindungen in dieser Klasse auf `NavigateCommand` verweisen.

Die Reihenfolge des Codes in diesem Konstruktor macht einen wichtigen Unterschied: Der Aufruf von `InitializeComponent` führt dazu, dass XAML analysiert wird. Zu diesem Zeitpunkt kann die Bindung zu einer Eigenschaft namens `NavigateCommand` jedoch nicht aufgelöst werden, da `BindingContext` auf `null` festgelegt ist. Wenn `BindingContext` im Konstruktor festgelegt wird, *bevor* `NavigateCommand` festgelegt wird, kann die Bindung aufgelöst werden, wenn `BindingContext` festgelegt ist. Zu diesem Zeitpunkt entspricht `NavigateCommand` jedoch immer noch `null`. Wenn Sie `NavigateCommand` nach `BindingContext` festlegen, wirkt sich dies nicht auf die Bindung aus, da durch eine Änderung von `NavigateCommand` kein `PropertyChanged`-Ereignis ausgelöst wird und der Bindung nicht bekannt ist, dass `NavigateCommand` nun gültig ist.

Das Festlegen von `NavigateCommand` und `BindingContext` (in beliebiger Reihenfolge) vor dem Aufruf von `InitializeComponent` funktioniert, da beide Komponenten der Bindung festgelegt werden, wenn der XAML-Parser die Bindungsdefinition ermittelt.

Datenbindungen können manchmal kompliziert sein. In dieser Artikelreihe wurde jedoch verdeutlicht, dass diese leistungsstark und vielseitig sind und dafür eingesetzt werden können, den Code zu organisieren, indem die zugrunde liegende Logik von der Benutzeroberfläche getrennt wird.

## <a name="related-links"></a>Verwandte Links

- [Data Binding Demos (Demos zur Datenbindung (Beispiel))](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/databindingdemos)
- [Kapitel zu Datenbindung aus dem Xamarin.Forms-Buch](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter18.md)
