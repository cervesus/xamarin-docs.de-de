---
title: Teil 5. Von Datenbindungen zu MVVM
description: Das MVVM-Muster erzwingt eine Trennung zwischen den drei Softwareebenen, die XAML-Benutzeroberfläche, die Namen der Ansicht befindet die zugrunde liegenden Daten, das als Modell bezeichnet; und ein Vermittler zwischen der Ansicht und das Modell als das "ViewModel" bezeichnet.
ms.prod: xamarin
ms.assetid: 48B37D44-4FB1-41B2-9A5E-6D383B041F81
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/25/2017
ms.openlocfilehash: c7bf7ca28200004e2383631c68cdaa4299348ecb
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61174581"
---
# <a name="part-5-from-data-bindings-to-mvvm"></a>Teil 5. Von Datenbindungen zu MVVM

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://developer.xamarin.com/samples/xamarin-forms/XamlSamples/)

_Beachten Sie das Architekturmuster Model-View-ViewModel (MVVM) mit XAML sollte. Das Muster erzwingt eine Trennung zwischen den drei Softwareebenen, die XAML-Benutzeroberfläche, die Namen der Ansicht befindet die zugrunde liegenden Daten, das als Modell bezeichnet; und ein Vermittler zwischen der Ansicht und das Modell als das "ViewModel" bezeichnet. Die Ansicht und ViewModel sind häufig über datenbindungen, die in der XAML-Datei definierten verbunden. BindingContext für die Ansicht ist normalerweise eine Instanz von "ViewModel"._

## <a name="a-simple-viewmodel"></a>Ein einfaches ViewModel

Als Einführung in ViewModels sehen wir uns zunächst an ein Programm ohne eine.
Zuvor wurde erläutert, wie Sie eine neue XML-Namespacedeklaration zum ermöglichen einer XAML-Datei, die als Verweisklassen in anderen Assemblys zu definieren. Hier ist ein Programm, das eine XML-Namespacedeklaration für definiert die `System` Namespace:

```csharp
xmlns:sys="clr-namespace:System;assembly=mscorlib"
```

Das Programm verwenden kann `x:Static` zum aktuellen Datum und Uhrzeit aus der statischen abrufen `DateTime.Now` Eigenschaft fest, und legen `DateTime` Wert der `BindingContext` auf eine `StackLayout`:

```xaml
<StackLayout BindingContext="{x:Static sys:DateTime.Now}" …>
```

`BindingContext` ist eine sehr spezielle Eigenschaft: Beim Festlegen der `BindingContext` auf ein Element, wird es von den untergeordneten Elementen dieses Elements geerbt. Dies bedeutet, dass alle untergeordneten Elemente der `StackLayout` haben diese gleiche `BindingContext`, und sie können einfache Bindung an Eigenschaften dieses Objekts enthalten.

In der **One-Shot "DateTime"** Programm zwei untergeordnete Knoten enthalten die Bindung an Eigenschaften, `DateTime` Wert, aber zwei andere untergeordnete Elemente enthalten Bindungen, die fehlen ein Bindungspfad erscheinen. Dies bedeutet, dass die `DateTime` Wert selbst wird verwendet, für die `StringFormat`:

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

Natürlich ist das große Problem, dass das Datum und Uhrzeit sind Satz, sobald bei die Seite zuerst erstellt wird, und nie ändern:

[![](data-bindings-to-mvvm-images/oneshotdatetime.png "Ansicht zum Anzeigen von Datums- /")](data-bindings-to-mvvm-images/oneshotdatetime-large.png#lightbox "Ansicht zum Anzeigen von Datum und Uhrzeit")

Eine XAML-Datei kann eine Uhr, die die aktuelle Uhrzeit immer zeigt anzeigen, jedoch benötigt etwas Code zu helfen. Wenn Denkweise von MVVM, das Modell und ViewModel Klassen, die vollständig in Code geschrieben sind. Die Ansicht ist häufig eine XAML-Datei, die Eigenschaften, die in "ViewModel" definiert ist, über datenbindungen verweist.

Ein entsprechendes Modells kennen muss, das "ViewModel" ist, und eine ordnungsgemäße "ViewModel" ist der Ansicht ignorierende. Allerdings schneidet eingeschränkt werden sehr häufig ein Programmierer die Datentypen, die von "ViewModel" in die Datentypen, die bestimmte Benutzeroberflächen zugeordneten verfügbar gemacht werden. Wenn ein Modell auf eine Datenbank, die 8-Bit-ASCII-Zeichenfolgen enthält zugreift, würde z. B. "ViewModel" müssen, für die Konvertierung zwischen diesen Zeichenfolgen, Unicode-Zeichenfolgen, um die exklusive Verwendung von Unicode in der Benutzeroberfläche zu berücksichtigen.

In einfachen Beispielen von MVVM (z. B. die hier gezeigte) häufig besteht kein Modell, und das Muster umfasst nur eine Ansicht aus, und "ViewModel" mit datenbindungen verknüpft.

Hier ist ein "ViewModel" für eine Uhr mit nur einer einzelnen Eigenschaft mit dem Namen `DateTime`, aber die aktualisiert werden, die `DateTime` Eigenschaft pro Sekunde:

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

ViewModels in der Regel implementiert die `INotifyPropertyChanged` Schnittstelle, was bedeutet, dass die Klasse ausgelöst werden, eine `PropertyChanged` Ereignis, wenn sich eine seiner Eigenschaften ändert. Der Datenbindungsmechanismus in Xamarin.Forms Fügt einen Handler an diese `PropertyChanged` Ereignis, sodass es bei Änderung einer Eigenschaft benachrichtigt werden kann, und lassen das Ziel mit dem neuen Wert aktualisiert.

Eine Uhr basierend auf "ViewModel" kann so einfach wie folgt sein:

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

Beachten Sie, dass wie die `ClockViewModel` nastaven NA hodnotu der `BindingContext` von der `Label` mithilfe von Eigenschaftenelement-Tags. Alternativ können Sie instanziieren die `ClockViewModel` in eine `Resources` Auflistung und legen Sie dafür die `BindingContext` über eine `StaticResource` Markuperweiterung. Alternativ dazu können Sie die CodeBehind Datei kann "ViewModel" instanziieren.

Die `Binding` Markuperweiterung für die `Text` Eigenschaft der `Label` Formate die `DateTime` Eigenschaft. So sieht die Anzeige aus:

[![](data-bindings-to-mvvm-images/clock.png "Ansicht zum Anzeigen von Datums- / über \"ViewModel\"")](data-bindings-to-mvvm-images/clock-large.png#lightbox "Ansicht zum Anzeigen von Datums- / über \"ViewModel\"")

Es ist auch möglich, den Zugriff auf einzelne Eigenschaften von den `DateTime` Eigenschaft von "ViewModel", indem Sie die Eigenschaften durch Punkte trennen:

```xaml
<Label Text="{Binding DateTime.Second, StringFormat='{0}'}" … >
```

## <a name="interactive-mvvm"></a>Interaktive MVVM

MVVM ist recht häufig mit bidirektionale datenbindungen für eine interaktive Ansicht basierend auf einem zugrunde liegenden Datenmodell verwendet.

Hier ist eine Klasse namens `HslViewModel` , konvertiert ein `Color` -Wert in `Hue`, `Saturation`, und `Luminosity` Werte und umgekehrt:

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

Änderungen an der `Hue`, `Saturation`, und `Luminosity` Eigenschaften Ursache der `Color` ändern, und Änderungen an `Color` bewirkt, dass die anderen drei Eigenschaften ändern. Dies mag wie eine unendliche Schleife, außer dass die Klasse aufrufen, nicht die `PropertyChanged` Ereignis, wenn die Eigenschaft tatsächlich geändert hat. Dies hilft dabei, die andernfalls durch unkontrollierbare Rückmeldungsschleife.

Der folgende XAML-Datei enthält eine `BoxView` , deren `Color` Eigenschaft gebunden ist die `Color` Eigenschaft von "ViewModel" und drei `Slider` und drei `Label` Ansichten gebunden werden, um die `Hue`, `Saturation`, und `Luminosity` Eigenschaften:

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

Die Bindung auf den einzelnen `Label` ist die Standardeinstellung `OneWay`. Es muss nur den Wert anzuzeigen. Aber die Bindung auf den einzelnen `Slider` ist `TwoWay`. Dadurch wird die `Slider` aus dem ViewModel initialisiert werden. Beachten Sie, dass die `Color` -Eigenschaftensatz auf `Aqua` Wenn "ViewModel" instanziiert wird. Nur eine Änderung in der `Slider` muss auch einen neuen Wert für die Eigenschaft im ViewModel festgelegt, die dann eine neue Farbe berechnet.

[![](data-bindings-to-mvvm-images/hslcolorscroll.png "Bidirektionale Datenbindungen mit MVVM")](data-bindings-to-mvvm-images/hslcolorscroll-large.png#lightbox "bidirektionale Datenbindungen mit MVVM")

## <a name="commanding-with-viewmodels"></a>Befehle mit ViewModels

In vielen Fällen ist das MVVM-Muster beschränkt ist, an der Bearbeitung von Datenelementen: Objekte der Benutzeroberfläche in der Ansicht parallele Datenobjekte in "ViewModel".

Allerdings muss manchmal die Ansicht Schaltflächen enthalten, die verschiedene Aktionen in "ViewModel" auslösen. Aber das "ViewModel" darf keinen `Clicked` Ereignishandler für die Schaltflächen da, die das "ViewModel" ein Paradigmenwechsel zu bestimmten Benutzeroberfläche gebunden würde.

Können Sie die ViewModels, die mehr unabhängig von bestimmten Benutzeroberflächenobjekte jedoch weiterhin Methoden in "ViewModel", aufgerufen werden eine *Befehl* Schnittstelle vorhanden ist. Dieser Befehlsschnittstelle wird durch die folgenden Elemente in Xamarin.Forms unterstützt:

-  `Button`
-  `MenuItem`
-  `ToolbarItem`
-  `SearchBar`
-  `TextCell` (und somit auch `ImageCell`)
-  `ListView`
-  `TapGestureRecognizer`

Mit Ausnahme von der `SearchBar` und `ListView` Elements definieren dieser Elemente zwei Eigenschaften:

-  `Command` Der Typ  `System.Windows.Input.ICommand`
-  `CommandParameter` Der Typ  `Object`

Die `SearchBar` definiert `SearchCommand` und `SearchCommandParameter` Eigenschaften während der `ListView` definiert eine `RefreshCommand` Eigenschaft vom Typ `ICommand`.

Die `ICommand` Schnittstelle definiert Methoden und ein Ereignis:

-  `void Execute(object arg)`
-  `bool CanExecute(object arg)`
-  `event EventHandler CanExecuteChanged`

Das "ViewModel" kann die Eigenschaften des Typs definieren `ICommand`. Anschließend können Sie diese Eigenschaften zu binden der `Command` Eigenschaft der einzelnen `Button` oder andere Elemente oder vielleicht eine benutzerdefinierte Ansicht, die diese Schnittstelle implementiert. Sie können optional festlegen, die `CommandParameter` Eigenschaft, um einzelne identifizieren `Button` Objekte (oder andere Elemente), die an diese Eigenschaft "ViewModel" gebunden werden. Intern die `Button` Aufrufe der `Execute` -Methode auf, wenn der Benutzer tippt der `Button`, und übergeben Sie zum die `Execute` Methode seine `CommandParameter`.

Die `CanExecute` Methode und `CanExecuteChanged` Ereignis werden Fälle verwendet, in denen eine `Button` Tap möglicherweise derzeit ungültig ist, in diesem Fall die `Button` sollten sich selbst deaktivieren. Die `Button` Aufrufe `CanExecute` bei der `Command` -Eigenschaft festgelegt ist und jedes Mal, wenn die `CanExecuteChanged` Ereignis wird ausgelöst. Wenn `CanExecute` gibt `false`, `Button` deaktiviert und nicht generieren `Execute` aufrufen.

Um Hilfe beim Hinzufügen der Befehle, um Ihre ViewModels, Xamarin.Forms definiert zwei Klassen, implementieren `ICommand`: `Command` und `Command<T>` , in denen `T` ist der Typ der Argumente für `Execute` und `CanExecute`. Diese beiden Klassen definieren mehrere Konstruktoren zuzüglich einer `ChangeCanExecute` -Methode, die das "ViewModel" aufrufen kann, um zu erzwingen die `Command` Objekt ausgelöst werden die `CanExecuteChanged` Ereignis.

Hier ist ein "ViewModel" für eine einfache Zehnertastatur, die für die Eingabe von Telefonnummern vorgesehen ist. Beachten Sie, dass die `Execute` und `CanExecute` Methode als Lambda-Funktionen direkt in den Konstruktor definiert sind:

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

Dieses "ViewModel" setzt voraus, dass die `AddCharCommand` Eigenschaft gebunden ist die `Command` Eigenschaft mehrere Schaltflächen (oder etwas anderes, das eine Befehlsschnittstelle verfügt), von denen jeder durch identifiziert die `CommandParameter`. Diese Schaltflächen hinzufügen, eine `InputString` -Eigenschaft, die dann als eine Telefonnummer für formatiert ist die `DisplayText` Eigenschaft.

Es gibt auch eine zweite Eigenschaft des Typs `ICommand` mit dem Namen `DeleteCharCommand`. Dies ist ein Zwischenraum zurück-Schaltfläche gebunden, aber die Schaltfläche mit der sollte deaktiviert werden, wenn es keine sind zu löschenden Zeichen.

Die folgende Zehnertastatur ist nicht als visuell anspruchsvolle, wie sie sein könnte. Stattdessen wurde das Markup auf ein Minimum, genauer auf die Verwendung der Befehlsschnittstelle gezeigt verringert:

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

Der `Command` Eigenschaft des ersten `Button` , angezeigt wird, in diesem Markup gebunden ist der `DeleteCharCommand`; an der Rest gebunden sind die `AddCharCommand` mit eine `CommandParameter` , identisch mit dem Zeichen, die auf die `Button` Gesicht. So sieht die Anwendung in Aktion:

[![](data-bindings-to-mvvm-images/keypad.png "Rechner mit Befehlen und MVVM")](data-bindings-to-mvvm-images/keypad-large.png#lightbox "Rechner mit Befehlen und MVVM")

### <a name="invoking-asynchronous-methods"></a>Aufrufen von asynchronen Methoden

Befehle können auch asynchrone Methoden aufrufen. Dies erfolgt mithilfe der `async` und `await` Schlüsselwörter, die beim Angeben der `Execute` Methode:

```csharp
DownloadCommand = new Command (async () => await DownloadAsync ());
```

Dies bedeutet, dass die `DownloadAsync` Methode ist eine `Task` und gewartet werden soll:

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

## <a name="implementing-a-navigation-menu"></a>Implementieren ein Navigationsmenü

Die [XamlSamples](https://developer.xamarin.com/samples/xamarin-forms/XamlSamples/) Programm, das den Quellcode in dieser Artikelreihe enthält ein "ViewModel" für die Startseite verwendet. Dieses "ViewModel" ist eine Definition einer kurzen Klasse mit drei Eigenschaften, die mit dem Namen `Type`, `Title`, und `Description` , die den Typ der einzelnen die Beispielseiten, einen Titel und eine kurze Beschreibung enthalten. Darüber hinaus definiert das "ViewModel" eine statische Eigenschaft namens `All` , d. h. eine Sammlung aller Seiten im Programm:

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

Die XAML-Datei für `MainPage` definiert eine `ListBox` , deren `ItemsSource` -Eigenschaftensatz auf, die `All` -Eigenschaft und die enthält eine `TextCell` für die Anzeige der `Title` und `Description` Eigenschaften jeder Seite:

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

Die Seiten werden in einer bildlauffähigen Liste angezeigt:

[![](data-bindings-to-mvvm-images/mainpage.png "Scrollbare Liste der Seiten")](data-bindings-to-mvvm-images/mainpage-large.png#lightbox "bildlauffähige Liste von Seiten")

Der Handler im Code-Behind-Datei wird ausgelöst, wenn der Benutzer ein Element auswählt. Der Handler legt die `SelectedItem` Eigenschaft der `ListBox` an `null` , und klicken Sie dann die ausgewählte Seite instanziiert und navigiert zu ihr:

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

**Xamarin Evolve 2016: MVVM, der mit Xamarin.Forms und Prism**

## <a name="summary"></a>Zusammenfassung

XAML ist ein leistungsstarkes Tool zum Definieren von Benutzeroberflächen in Xamarin.Forms-Anwendungen, insbesondere dann, wenn die Datenbindung und MVVM verwendet werden. Das Ergebnis ist eine saubere, elegante und potenziell lässt Darstellung einer Benutzeroberfläche mit der Hintergrund-Unterstützung im Code.


## <a name="related-links"></a>Verwandte Links

- [XamlSamples](https://developer.xamarin.com/samples/xamarin-forms/XamlSamples/)
- [Teil 1. Erste Schritte mit XAML](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md)
- [Teil 2. Grundlegende XAML-Syntax](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md)
- [Teil 3. XAML-Markuperweiterungen](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Teil 4. Grundlagen der Datenbindung](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)
