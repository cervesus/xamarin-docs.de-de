---
title: Teil 5. Von Datenbindungen zu MVVM
description: Das MVVM-Muster erzwingt eine Trennung zwischen drei Software Ebenen – der XAML-Benutzeroberfläche, die als Ansicht bezeichnet wird. die zugrunde liegenden Daten, die als Modell bezeichnet werden. und ein Vermittler zwischen der Ansicht und dem Modell, die als ViewModel bezeichnet wird.
ms.prod: xamarin
ms.custom: video
ms.assetid: 48B37D44-4FB1-41B2-9A5E-6D383B041F81
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/25/2017
ms.openlocfilehash: 80386780d52f9a28d421d2a83981085956d06ea5
ms.sourcegitcommit: efbc69acf4ea484d8815311b058114379c9db8a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73842950"
---
# <a name="part-5-from-data-bindings-to-mvvm"></a>Teil 5. Von Datenbindungen zu MVVM

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xamlsamples)

_Das Model-View-ViewModel (MVVM)-Architekturmuster wurde mit XAML im Hinterkopf erfunden. Das Muster erzwingt eine Trennung zwischen drei Software Ebenen – der XAML-Benutzeroberfläche, die als Ansicht bezeichnet wird. die zugrunde liegenden Daten, die als Modell bezeichnet werden. und ein Vermittler zwischen der Ansicht und dem Modell, die als ViewModel bezeichnet wird. Die Ansicht und ViewModel sind häufig über Daten Bindungen verbunden, die in der XAML-Datei definiert sind. Der BindingContext für die Sicht ist in der Regel eine Instanz von "ViewModel"._

## <a name="a-simple-viewmodel"></a>Ein einfaches ViewModel

Betrachten wir als Einführung in ViewModels zunächst ein Programm ohne eins.
Früher haben Sie erfahren, wie Sie eine neue XML-Namespace Deklaration definieren, damit eine XAML-Datei auf Klassen in anderen Assemblys verweisen kann. Hier ist ein Programm, das eine XML-Namespace Deklaration für den `System`-Namespace definiert:

```csharp
xmlns:sys="clr-namespace:System;assembly=mscorlib"
```

Das Programm kann `x:Static` verwenden, um das aktuelle Datum und die aktuelle Uhrzeit aus der Eigenschaft static `DateTime.Now` abzurufen, und diesen `DateTime` Wert auf die `BindingContext` auf einem `StackLayout`festzulegen:

```xaml
<StackLayout BindingContext="{x:Static sys:DateTime.Now}" …>
```

`BindingContext` ist eine spezielle Eigenschaft: Wenn Sie die `BindingContext` für ein Element festlegen, wird Sie von allen untergeordneten Elementen dieses Elements geerbt. Dies bedeutet, dass alle untergeordneten Elemente des `StackLayout` denselben `BindingContext`haben und einfache Bindungen zu Eigenschaften dieses Objekts enthalten können.

Im **One-Shot-DateTime** -Programm enthalten zwei der untergeordneten Elemente Bindungen zu Eigenschaften dieses `DateTime` Werts, aber zwei andere untergeordnete Elemente enthalten Bindungen, für die ein Bindungs Pfad fehlt. Dies bedeutet, dass der `DateTime` Wert selbst für die `StringFormat`verwendet wird:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:sys="clr-namespace:System;assembly=mscorlib"
             x:Class="XamlSamples.OneShotDateTimePage"
             Title="One-Shot DateTime Page">

    <StackLayout BindingContext="{x:Static sys:DateTime.Now}"
                 HorizontalOptions="Center"
                 VerticalOptions="Center">

        <Label Text="{Binding Year, StringFormat='The year is {0}'}" />
        <Label Text="{Binding StringFormat='The month is {0:MMMM}'}" />
        <Label Text="{Binding Day, StringFormat='The day is {0}'}" />
        <Label Text="{Binding StringFormat='The time is {0:T}'}" />

    </StackLayout>
</ContentPage>
```

Das Problem besteht darin, dass das Datum und die Uhrzeit einmal festgelegt werden, wenn die Seite erstmalig erstellt wird, und sich nie ändern:

[![](data-bindings-to-mvvm-images/oneshotdatetime.png "View Displaying Date and Time")](data-bindings-to-mvvm-images/oneshotdatetime-large.png#lightbox "View Displaying Date and Time")

In einer XAML-Datei kann eine Uhr angezeigt werden, die immer die aktuelle Zeit anzeigt, aber Sie benötigt etwas Code, um Sie zu unterstützen. Bei der Betrachtung von MVVM sind Model und ViewModel Klassen, die vollständig im Code geschrieben sind. Die Sicht ist häufig eine XAML-Datei, die auf Eigenschaften verweist, die im ViewModel durch Daten Bindungen definiert werden.

Ein richtiges Modell ignoriert das ViewModel, und ein richtiges ViewModel kennt die Sicht nicht. Häufig schneidet ein Programmierer jedoch die Datentypen, die vom ViewModel verfügbar gemacht werden, den Datentypen zu, die bestimmten Benutzeroberflächen zugeordnet sind. Wenn ein Modell beispielsweise auf eine Datenbank zugreift, die 8-Bit-ASCII-Zeichen folgen enthält, muss das ViewModel zwischen diesen Zeichen folgen in Unicode-Zeichen folgen konvertieren, um die ausschließliche Verwendung von Unicode in der Benutzeroberfläche zu ermöglichen.

In einfachen Beispielen von MVVM (z. b. den hier gezeigten) gibt es häufig kein Modell, und das Muster umfasst lediglich eine Ansicht und ein ViewModel, die mit Daten Bindungen verknüpft sind.

Hier ist ein ViewModel für eine Uhr mit nur einer Eigenschaft mit dem Namen `DateTime`, die diese `DateTime` Eigenschaft jede Sekunde aktualisiert:

```csharp
using System;
using System.ComponentModel;
using Xamarin.Forms;

namespace XamlSamples
{
    class ClockViewModel : INotifyPropertyChanged
    {
        DateTime dateTime;

        public event PropertyChangedEventHandler PropertyChanged;

        public ClockViewModel()
        {
            this.DateTime = DateTime.Now;

            Device.StartTimer(TimeSpan.FromSeconds(1), () =>
                {
                    this.DateTime = DateTime.Now;
                    return true;
                });
        }

        public DateTime DateTime
        {
            set
            {
                if (dateTime != value)
                {
                    dateTime = value;

                    if (PropertyChanged != null)
                    {
                        PropertyChanged(this, new PropertyChangedEventArgs("DateTime"));
                    }
                }
            }
            get
            {
                return dateTime;
            }
        }
    }
}
```

ViewModels implementieren im Allgemeinen die `INotifyPropertyChanged`-Schnittstelle. Dies bedeutet, dass die-Klasse ein `PropertyChanged`-Ereignis auslöst, wenn sich eine ihrer Eigenschaften ändert. Der Daten Bindungs Mechanismus in xamarin. Forms fügt einen Handler an dieses `PropertyChanged` Ereignis an, damit er benachrichtigt werden kann, wenn eine Eigenschaft geändert wird, und das Ziel mit dem neuen Wert aktualisiert wird.

Eine Uhr, die auf diesem ViewModel basiert, kann so einfach sein:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:XamlSamples;assembly=XamlSamples"
             x:Class="XamlSamples.ClockPage"
             Title="Clock Page">

    <Label Text="{Binding DateTime, StringFormat='{0:T}'}"
           FontSize="Large"
           HorizontalOptions="Center"
           VerticalOptions="Center">
        <Label.BindingContext>
            <local:ClockViewModel />
        </Label.BindingContext>
    </Label>
</ContentPage>
```

Beachten Sie, dass die `ClockViewModel` auf die `BindingContext` der `Label` festgelegt wird, die Eigenschaften Element Tags verwendet. Alternativ dazu können Sie die `ClockViewModel` in einer `Resources` Auflistung instanziieren und Sie über eine `StaticResource` Markup Erweiterung auf die `BindingContext` festlegen. Oder die Code-Behind-Datei kann ViewModel instanziieren.

Die `Binding` Markup Erweiterung auf der `Text`-Eigenschaft des `Label` formatiert die `DateTime`-Eigenschaft. Hier sehen Sie die Anzeige:

[![](data-bindings-to-mvvm-images/clock.png "View Displaying Date and Time via ViewModel")](data-bindings-to-mvvm-images/clock-large.png#lightbox "View Displaying Date and Time via ViewModel")

Es ist auch möglich, auf einzelne Eigenschaften der `DateTime`-Eigenschaft von "ViewModel" zuzugreifen, indem die Eigenschaften durch Zeiträume getrennt werden:

```xaml
<Label Text="{Binding DateTime.Second, StringFormat='{0}'}" … >
```

## <a name="interactive-mvvm"></a>Interaktiver MVVM

MVVM wird häufig mit bidirektionalen Daten Bindungen für eine interaktive Ansicht verwendet, die auf einem zugrunde liegenden Datenmodell basiert.

Hier ist eine Klasse mit dem Namen `HslViewModel`, die einen `Color`-Wert in die Werte `Hue`, `Saturation`und `Luminosity` konvertiert und umgekehrt:

```csharp
using System;
using System.ComponentModel;
using Xamarin.Forms;

namespace XamlSamples
{
    public class HslViewModel : INotifyPropertyChanged
    {
        double hue, saturation, luminosity;
        Color color;

        public event PropertyChangedEventHandler PropertyChanged;

        public double Hue
        {
            set
            {
                if (hue != value)
                {
                    hue = value;
                    OnPropertyChanged("Hue");
                    SetNewColor();
                }
            }
            get
            {
                return hue;
            }
        }

        public double Saturation
        {
            set
            {
                if (saturation != value)
                {
                    saturation = value;
                    OnPropertyChanged("Saturation");
                    SetNewColor();
                }
            }
            get
            {
                return saturation;
            }
        }

        public double Luminosity
        {
            set
            {
                if (luminosity != value)
                {
                    luminosity = value;
                    OnPropertyChanged("Luminosity");
                    SetNewColor();
                }
            }
            get
            {
                return luminosity;
            }
        }

        public Color Color
        {
            set
            {
                if (color != value)
                {
                    color = value;
                    OnPropertyChanged("Color");

                    Hue = value.Hue;
                    Saturation = value.Saturation;
                    Luminosity = value.Luminosity;
                }
            }
            get
            {
                return color;
            }
        }

        void SetNewColor()
        {
            Color = Color.FromHsla(Hue, Saturation, Luminosity);
        }

        protected virtual void OnPropertyChanged(string propertyName)
        {
            PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
        }
    }
}
```

Änderungen an den Eigenschaften `Hue`, `Saturation`und `Luminosity` bewirken, dass die `Color` Eigenschaft geändert wird, und Änderungen an `Color` bewirken, dass die anderen drei Eigenschaften geändert werden. Dies mag eine Endlosschleife sein, mit dem Unterschied, dass die-Klasse das `PropertyChanged`-Ereignis nur dann aufruft, wenn sich die-Eigenschaft geändert hat. Dadurch wird ein Ende der ansonsten unkontrollierbaren Feedback Schleife angezeigt.

Die folgende XAML-Datei enthält eine `BoxView`, deren `Color`-Eigenschaft an die `Color`-Eigenschaft von ViewModel gebunden ist, sowie drei `Slider` und drei `Label` Sichten, die an die Eigenschaften `Hue`, `Saturation`und `Luminosity` gebunden sind:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:XamlSamples;assembly=XamlSamples"
             x:Class="XamlSamples.HslColorScrollPage"
             Title="HSL Color Scroll Page">
    <ContentPage.BindingContext>
        <local:HslViewModel Color="Aqua" />
    </ContentPage.BindingContext>

    <StackLayout Padding="10, 0">
        <BoxView Color="{Binding Color}"
                 VerticalOptions="FillAndExpand" />

        <Label Text="{Binding Hue, StringFormat='Hue = {0:F2}'}"
               HorizontalOptions="Center" />

        <Slider Value="{Binding Hue, Mode=TwoWay}" />

        <Label Text="{Binding Saturation, StringFormat='Saturation = {0:F2}'}"
               HorizontalOptions="Center" />

        <Slider Value="{Binding Saturation, Mode=TwoWay}" />

        <Label Text="{Binding Luminosity, StringFormat='Luminosity = {0:F2}'}"
               HorizontalOptions="Center" />

        <Slider Value="{Binding Luminosity, Mode=TwoWay}" />
    </StackLayout>
</ContentPage>
```

Die Bindung der einzelnen `Label` ist die Standard `OneWay`. Der Wert muss nur angezeigt werden. Die Bindung der einzelnen `Slider` ist jedoch `TwoWay`. Dadurch kann der `Slider` aus ViewModel initialisiert werden. Beachten Sie, dass die `Color`-Eigenschaft auf `Aqua` festgelegt wird, wenn das ViewModel-Objekt instanziiert wird. Eine Änderung in der `Slider` muss jedoch auch einen neuen Wert für die-Eigenschaft in ViewModel festlegen, der dann eine neue Farbe berechnet.

[![](data-bindings-to-mvvm-images/hslcolorscroll.png "MVVM using Two-Way Data Bindings")](data-bindings-to-mvvm-images/hslcolorscroll-large.png#lightbox "MVVM using Two-Way Data Bindings")

## <a name="commanding-with-viewmodels"></a>Befehls mit "ViewModels"

In vielen Fällen ist das MVVM-Muster auf die Bearbeitung von Datenelementen beschränkt: Benutzeroberflächen Objekte in der Ansicht "parallele Datenobjekte" in "ViewModel".

Manchmal muss die Sicht jedoch Schaltflächen enthalten, die verschiedene Aktionen im ViewModel auslöst. Das ViewModel darf jedoch keine `Clicked` Handler für die Schaltflächen enthalten, da das ViewModel mit einem bestimmten Benutzeroberflächen Paradigma verknüpft würde.

Um zu ermöglichen, dass ViewModels von bestimmten Benutzeroberflächen Objekten unabhängig sind, aber immer noch Methoden innerhalb von ViewModel aufgerufen werden können, ist eine *Befehls* Schnittstelle vorhanden. Diese Befehlsschnittstelle wird von den folgenden Elementen in xamarin. Forms unterstützt:

- `Button`
- `MenuItem`
- `ToolbarItem`
- `SearchBar`
- `TextCell` (und damit auch `ImageCell`)
- `ListView`
- `TapGestureRecognizer`

Mit Ausnahme der `SearchBar`-und `ListView`-Elements definieren diese Elemente zwei Eigenschaften:

- `Command` vom Typ `System.Windows.Input.ICommand`
- `CommandParameter` vom Typ `Object`

Der-`SearchBar` definiert `SearchCommand` und `SearchCommandParameter` Eigenschaften, während das `ListView` eine `RefreshCommand`-Eigenschaft des Typs `ICommand`definiert.

Die `ICommand`-Schnittstelle definiert zwei Methoden und ein Ereignis:

- `void Execute(object arg)`
- `bool CanExecute(object arg)`
- `event EventHandler CanExecuteChanged`

Das ViewModel kann Eigenschaften vom Typ `ICommand`definieren. Anschließend können Sie diese Eigenschaften an die `Command`-Eigenschaft der einzelnen `Button` oder anderen Elemente oder eventuell an eine benutzerdefinierte Ansicht binden, die diese Schnittstelle implementiert. Optional können Sie die `CommandParameter`-Eigenschaft festlegen, um einzelne `Button` Objekte (oder andere Elemente) zu identifizieren, die an diese ViewModel-Eigenschaft gebunden sind. Intern ruft der `Button` die `Execute`-Methode immer dann auf, wenn der Benutzer auf die `Button`tippt und die `Execute` Methode an die `CommandParameter`übergibt.

Die `CanExecute`-Methode und das `CanExecuteChanged` Ereignis werden in Fällen verwendet, in denen ein `Button` Tap derzeit ungültig sein kann. in diesem Fall sollten die `Button` sich selbst deaktivieren. Der `Button` ruft `CanExecute` auf, wenn die `Command`-Eigenschaft zum ersten Mal festgelegt wird, und wenn das `CanExecuteChanged` Ereignis ausgelöst wird. Wenn `CanExecute` `false`zurückgibt, wird der `Button` deaktiviert, und es werden keine `Execute` Aufrufe generiert.

Um Hilfe beim Hinzufügen von Befehle zu ViewModels zu erhalten, definiert xamarin. Forms zwei Klassen, die `ICommand`implementieren: `Command` und `Command<T>`, wobei `T` der Typ der Argumente ist, die `Execute` und `CanExecute`werden. Diese beiden Klassen definieren mehrere Konstruktoren sowie eine `ChangeCanExecute` Methode, die das ViewModel aufzurufen kann, um das Auslösen des `CanExecuteChanged` Ereignisses durch das `Command` Objekt zu erzwingen.

Hier ist ein ViewModel für eine einfache Tastatur, die für die Eingabe von Telefonnummern vorgesehen ist. Beachten Sie, dass die `Execute`-und `CanExecute`-Methode als Lambda-Funktionen direkt im Konstruktor definiert sind:

```csharp
using System;
using System.ComponentModel;
using System.Windows.Input;
using Xamarin.Forms;

namespace XamlSamples
{
    class KeypadViewModel : INotifyPropertyChanged
    {
        string inputString = "";
        string displayText = "";
        char[] specialChars = { '*', '#' };

        public event PropertyChangedEventHandler PropertyChanged;

        // Constructor
        public KeypadViewModel()
        {
            AddCharCommand = new Command<string>((key) =>
                {
                    // Add the key to the input string.
                    InputString += key;
                });

            DeleteCharCommand = new Command(() =>
                {
                    // Strip a character from the input string.
                    InputString = InputString.Substring(0, InputString.Length - 1);
                },
                () =>
                {
                    // Return true if there's something to delete.
                    return InputString.Length > 0;
                });
        }

        // Public properties
        public string InputString
        {
            protected set
            {
                if (inputString != value)
                {
                    inputString = value;
                    OnPropertyChanged("InputString");
                    DisplayText = FormatText(inputString);

                    // Perhaps the delete button must be enabled/disabled.
                    ((Command)DeleteCharCommand).ChangeCanExecute();
                }
            }

            get { return inputString; }
        }

        public string DisplayText
        {
            protected set
            {
                if (displayText != value)
                {
                    displayText = value;
                    OnPropertyChanged("DisplayText");
                }
            }
            get { return displayText; }
        }

        // ICommand implementations
        public ICommand AddCharCommand { protected set; get; }

        public ICommand DeleteCharCommand { protected set; get; }

        string FormatText(string str)
        {
            bool hasNonNumbers = str.IndexOfAny(specialChars) != -1;
            string formatted = str;

            if (hasNonNumbers || str.Length < 4 || str.Length > 10)
            {
            }
            else if (str.Length < 8)
            {
                formatted = String.Format("{0}-{1}",
                                          str.Substring(0, 3),
                                          str.Substring(3));
            }
            else
            {
                formatted = String.Format("({0}) {1}-{2}",
                                          str.Substring(0, 3),
                                          str.Substring(3, 3),
                                          str.Substring(6));
            }
            return formatted;
        }

        protected void OnPropertyChanged(string propertyName)
        {
            PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
        }
    }
}
```

Dieses ViewModel geht davon aus, dass die `AddCharCommand`-Eigenschaft an die `Command`-Eigenschaft mehrerer Schaltflächen (oder beliebiger anderer Benutzer mit einer Befehlsschnittstelle) gebunden ist, die jeweils durch die `CommandParameter`identifiziert werden. Mit diesen Schaltflächen können Sie einer `InputString`-Eigenschaft Zeichen hinzufügen, die dann als Telefonnummer für die `DisplayText`-Eigenschaft formatiert wird.

Es gibt auch eine zweite Eigenschaft des Typs `ICommand` mit dem Namen `DeleteCharCommand`. Diese ist an eine Schaltfläche backabstand gebunden, aber die Schaltfläche sollte deaktiviert werden, wenn keine zu löschenden Zeichen vorhanden sind.

Der folgende Tastatur ist nicht so visuell ausgereift wie möglich. Stattdessen wurde das Markup auf ein Mindestmaß reduziert, um die Verwendung der Befehlsschnittstelle deutlicher zu veranschaulichen:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:XamlSamples;assembly=XamlSamples"
             x:Class="XamlSamples.KeypadPage"
             Title="Keypad Page">

    <Grid HorizontalOptions="Center"
          VerticalOptions="Center">
        <Grid.BindingContext>
            <local:KeypadViewModel />
        </Grid.BindingContext>

        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="80" />
            <ColumnDefinition Width="80" />
            <ColumnDefinition Width="80" />
        </Grid.ColumnDefinitions>

        <!-- Internal Grid for top row of items -->
        <Grid Grid.Row="0" Grid.Column="0" Grid.ColumnSpan="3">
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="*" />
                <ColumnDefinition Width="Auto" />
            </Grid.ColumnDefinitions>

            <Frame Grid.Column="0"
                   OutlineColor="Accent">
                <Label Text="{Binding DisplayText}" />
            </Frame>

            <Button Text="&#x21E6;"
                    Command="{Binding DeleteCharCommand}"
                    Grid.Column="1"
                    BorderWidth="0" />
        </Grid>

        <Button Text="1"
                Command="{Binding AddCharCommand}"
                CommandParameter="1"
                Grid.Row="1" Grid.Column="0" />

        <Button Text="2"
                Command="{Binding AddCharCommand}"
                CommandParameter="2"
                Grid.Row="1" Grid.Column="1" />

        <Button Text="3"
                Command="{Binding AddCharCommand}"
                CommandParameter="3"
                Grid.Row="1" Grid.Column="2" />

        <Button Text="4"
                Command="{Binding AddCharCommand}"
                CommandParameter="4"
                Grid.Row="2" Grid.Column="0" />

        <Button Text="5"
                Command="{Binding AddCharCommand}"
                CommandParameter="5"
                Grid.Row="2" Grid.Column="1" />

        <Button Text="6"
                Command="{Binding AddCharCommand}"
                CommandParameter="6"
                Grid.Row="2" Grid.Column="2" />

        <Button Text="7"
                Command="{Binding AddCharCommand}"
                CommandParameter="7"
                Grid.Row="3" Grid.Column="0" />

        <Button Text="8"
                Command="{Binding AddCharCommand}"
                CommandParameter="8"
                Grid.Row="3" Grid.Column="1" />

        <Button Text="9"
                Command="{Binding AddCharCommand}"
                CommandParameter="9"
                Grid.Row="3" Grid.Column="2" />

        <Button Text="*"
                Command="{Binding AddCharCommand}"
                CommandParameter="*"
                Grid.Row="4" Grid.Column="0" />

        <Button Text="0"
                Command="{Binding AddCharCommand}"
                CommandParameter="0"
                Grid.Row="4" Grid.Column="1" />

        <Button Text="#"
                Command="{Binding AddCharCommand}"
                CommandParameter="#"
                Grid.Row="4" Grid.Column="2" />
    </Grid>
</ContentPage>
```

Die `Command`-Eigenschaft des ersten `Button`, die in diesem Markup angezeigt wird, ist an die `DeleteCharCommand`gebunden. der Rest ist an das `AddCharCommand` mit einem `CommandParameter` gebunden, der mit dem Zeichen identisch ist, das auf dem `Button` Gesicht angezeigt wird. Hier ist das Programm in Aktion:

[![](data-bindings-to-mvvm-images/keypad.png "Calculator using MVVM and Commands")](data-bindings-to-mvvm-images/keypad-large.png#lightbox "Calculator using MVVM and Commands")

### <a name="invoking-asynchronous-methods"></a>Aufrufen von asynchronen Methoden

Befehle können auch asynchrone Methoden aufrufen. Dies wird erreicht, indem die `async`-und `await` Schlüsselwörter beim Angeben der `Execute`-Methode verwendet werden:

```csharp
DownloadCommand = new Command (async () => await DownloadAsync ());
```

Dies weist darauf hin, dass die `DownloadAsync`-Methode ein `Task` ist und erwartet werden sollte:

```csharp
async Task DownloadAsync ()
{
    await Task.Run (() => Download ());
}

void Download ()
{
    ...
}
```

## <a name="implementing-a-navigation-menu"></a>Implementieren eines Navigationsmenüs

Das [xamlsamples](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xamlsamples) -Programm, das den gesamten Quellcode in dieser Artikel Reihe enthält, verwendet ein ViewModel für die zugehörige Startseite. Dieses ViewModel ist eine Definition einer kurzen Klasse mit drei Eigenschaften namens `Type`, `Title`und `Description`, die den Typ der einzelnen Beispielseiten, einen Titel und eine kurze Beschreibung enthalten. Außerdem definiert ViewModel eine statische Eigenschaft mit dem Namen `All`, die eine Auflistung aller Seiten im Programm ist:

```csharp
public class PageDataViewModel
{
    public PageDataViewModel(Type type, string title, string description)
    {
        Type = type;
        Title = title;
        Description = description;
    }

    public Type Type { private set; get; }

    public string Title { private set; get; }

    public string Description { private set; get; }

    static PageDataViewModel()
    {
        All = new List<PageDataViewModel>
        {
            // Part 1. Getting Started with XAML
            new PageDataViewModel(typeof(HelloXamlPage), "Hello, XAML",
                                  "Display a Label with many properties set"),

            new PageDataViewModel(typeof(XamlPlusCodePage), "XAML + Code",
                                  "Interact with a Slider and Button"),

            // Part 2. Essential XAML Syntax
            new PageDataViewModel(typeof(GridDemoPage), "Grid Demo",
                                  "Explore XAML syntax with the Grid"),

            new PageDataViewModel(typeof(AbsoluteDemoPage), "Absolute Demo",
                                  "Explore XAML syntax with AbsoluteLayout"),

            // Part 3. XAML Markup Extensions
            new PageDataViewModel(typeof(SharedResourcesPage), "Shared Resources",
                                  "Using resource dictionaries to share resources"),

            new PageDataViewModel(typeof(StaticConstantsPage), "Static Constants",
                                  "Using the x:Static markup extensions"),

            new PageDataViewModel(typeof(RelativeLayoutPage), "Relative Layout",
                                  "Explore XAML markup extensions"),

            // Part 4. Data Binding Basics
            new PageDataViewModel(typeof(SliderBindingsPage), "Slider Bindings",
                                  "Bind properties of two views on the page"),

            new PageDataViewModel(typeof(SliderTransformsPage), "Slider Transforms",
                                  "Use Sliders with reverse bindings"),

            new PageDataViewModel(typeof(ListViewDemoPage), "ListView Demo",
                                  "Use a ListView with data bindings"),

            // Part 5. From Data Bindings to MVVM
            new PageDataViewModel(typeof(OneShotDateTimePage), "One-Shot DateTime",
                                  "Obtain the current DateTime and display it"),

            new PageDataViewModel(typeof(ClockPage), "Clock",
                                  "Dynamically display the current time"),

            new PageDataViewModel(typeof(HslColorScrollPage), "HSL Color Scroll",
                                  "Use a view model to select HSL colors"),

            new PageDataViewModel(typeof(KeypadPage), "Keypad",
                                  "Use a view model for numeric keypad logic")
        };
    }

    public static IList<PageDataViewModel> All { private set; get; }
}
```

In der XAML-Datei für `MainPage` wird ein `ListBox` definiert, dessen `ItemsSource`-Eigenschaft auf diese `All` Eigenschaft festgelegt ist und eine `TextCell` zum Anzeigen der `Title`-und `Description` Eigenschaften jeder Seite enthält:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:XamlSamples"
             x:Class="XamlSamples.MainPage"
             Padding="5, 0"
             Title="XAML Samples">

    <ListView ItemsSource="{x:Static local:PageDataViewModel.All}"
              ItemSelected="OnListViewItemSelected">
        <ListView.ItemTemplate>
            <DataTemplate>
                <TextCell Text="{Binding Title}"
                          Detail="{Binding Description}" />
            </DataTemplate>
        </ListView.ItemTemplate>
    </ListView>
</ContentPage>
```

Die Seiten werden in einer Bild lauffähigen Liste angezeigt:

[![](data-bindings-to-mvvm-images/mainpage.png "Scrollable list of pages")](data-bindings-to-mvvm-images/mainpage-large.png#lightbox "Scrollable list of pages")

Der Handler in der Code-Behind-Datei wird ausgelöst, wenn der Benutzer ein Element auswählt. Der Handler legt die `SelectedItem`-Eigenschaft des `ListBox` auf `null` zurück, instanziiert die ausgewählte Seite und navigiert zu ihr:

```csharp
private async void OnListViewItemSelected(object sender, SelectedItemChangedEventArgs args)
{
    (sender as ListView).SelectedItem = null;

    if (args.SelectedItem != null)
    {
        PageDataViewModel pageData = args.SelectedItem as PageDataViewModel;
        Page page = (Page)Activator.CreateInstance(pageData.Type);
        await Navigation.PushAsync(page);
    }
}
```

## <a name="video"></a>Video

> [!VIDEO https://youtube.com/embed/DYRLcqG2BAY]

**Xamarin weiterentwickelt 2016: MVVM wurde mit xamarin. Forms und Prism einfach gemacht**

## <a name="summary"></a>Zusammenfassung

XAML ist ein leistungsfähiges Tool zum Definieren von Benutzeroberflächen in xamarin. Forms-Anwendungen, insbesondere wenn die Datenbindung und MVVM verwendet werden. Das Ergebnis ist eine saubere, elegante und potenziell toolbare Darstellung einer Benutzeroberfläche mit der gesamten hintergrundunterstützung im Code.

## <a name="related-links"></a>Verwandte Links

- [Xamlsamples](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xamlsamples)
- [Teil 1: Einstieg in XAML](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md)
- [Teil 2. Wichtige XAML-Syntax](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md)
- [Teil 3: XAML-Markup Erweiterungen](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Teil 4. Grundlagen der Datenbindung](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)

## <a name="related-videos"></a>Verwandte Videos

> [!Video https://channel9.msdn.com/Series/Xamarin-101/XamarinForms-MVVM-with-XAML-6-of-11/player]

> [!Video https://channel9.msdn.com/Series/Xamarin-101/XamarinForms-Navigation-with-XAML-7-of-11/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
