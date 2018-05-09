---
title: Bindungsmodus
description: Steuern des Informationsflusses zwischen Quelle und Ziel
ms.prod: xamarin
ms.assetid: D087C389-2E9E-47B9-A341-5B14AC732C45
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 05/01/2018
ms.openlocfilehash: 1aa612d8b855158f09bc0aeaad1520a44b3d9637
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
---
# <a name="binding-mode"></a>Bindungsmodus

In der [vorherigen Artikel](basic-bindings.md), die **Alternative Code binden** und **Alternative Verwendung von XAML-Bindung** Seiten Funktionsumfang einer `Label` mit seiner `Scale` Eigenschaft gebunden an die `Value` Eigenschaft eine `Slider`. Da die `Slider` Anfangswert gleich 0 ist, die Ursache der `Scale` Eigenschaft von der `Label` 0 statt 1 festgelegt werden und die `Label` verschwunden.

In der [ **DataBindingDemos** ](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/) Beispiel wird die **Reverse-Bindung** Seite ähnelt der Programme in der vorherigen Artikel mit dem Unterschied, dass die Datenbindung für die definiertist`Slider` statt auf die `Label`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DataBindingDemos.ReverseBindingPage"
             Title="Reverse Binding">
    <StackLayout Padding="10, 0">

        <Label x:Name="label" 
               Text="TEXT"
               FontSize="80"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />

        <Slider x:Name="slider"
                VerticalOptions="CenterAndExpand"
                Value="{Binding Source={x:Reference label},
                                Path=Opacity}" />
    </StackLayout>
</ContentPage>
```

Zunächst mag dies Abwärtskompatibilität: jetzt die `Label` der Datenbindungsquelle ist und die `Slider` ist das Ziel. Die Bindungsverweise der `Opacity` Eigenschaft von der `Label`, dem hat des Standardwert 1.

Wie zu erwarten, die `Slider` auf den Wert 1 initialisiert wird, aus dem ersten `Opacity` Wert `Label`. Dies wird im auf der linken Seite der iOS-Screenshot gezeigt:

[![Reverse-Bindung](binding-mode-images/reversebinding-small.png "Reverse-Bindung")](binding-mode-images/reversebinding-large.png#lightbox "Reverse-Bindung")

Werden aber möglicherweise überrascht es, dass die `Slider` weiterhin funktionsfähig sind, wie in der Android- und uwp-Screenshots veranschaulicht. Dies wird empfohlen, dass die Datenbindung optimiert, wenn funktioniert scheint der `Slider` ist das Bindungsziel statt über das `Label` daran, dass die Initialisierung funktioniert, wie wir erwarten.

Der Unterschied zwischen der **umkehren Bindung** umfasst das Beispiel und in den früheren Beispielen die *Bindungsmodus*.

## <a name="the-default-binding-mode"></a>Der Standardmodus für die Bindung

Der Bindungsmodus ist angegeben, mit einem Mitglied der [ `BindingMode` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindingMode/) Enumeration: 

- [`Default`](https://developer.xamarin.com/api/field/Xamarin.Forms.BindingMode.Default/) 
- [`TwoWay`](https://developer.xamarin.com/api/field/Xamarin.Forms.BindingMode.TwoWay/) &ndash; Daten gehen beide Richtungen zwischen Quelle und Ziel
- [`OneWay`](https://developer.xamarin.com/api/field/Xamarin.Forms.BindingMode.OneWay/) &ndash; verspätet Daten aus der Quelle zum Ziel
- [`OneWayToSource`](https://developer.xamarin.com/api/field/Xamarin.Forms.BindingMode.OneWayToSource/) &ndash; Daten wechselt vom Ziel zu Quelle
- [`OneTime`](https://developer.xamarin.com/api/field/Xamarin.Forms.BindingMode.OneWayToSource/) &ndash; verspätet Daten aus der Quelle zum Ziel, aber nur, wenn die `BindingContext` ändert sich (mit Xamarin.Forms 3.0 neu)

Jede bindbare Eigenschaft hat den Standardwert Bindungsmodus, die festgelegt wird, wenn die bindbare Eigenschaft erstellt wird, und die verfügbar ist die [ `DefaultBindingMode` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableProperty.DefaultBindingMode/) Eigenschaft von der `BindableProperty` Objekt. Dieser Standardmodus für die Bindung gibt den Modus wirksam, wenn diese Eigenschaft eine Datenbindung-Ziel ist.

Der Standardmodus für die Bindung für die meisten Eigenschaften, z. B. `Rotation`, `Scale`, und `Opacity` ist `OneWay`. Wenn diese Eigenschaften Datenbindungsfunktionen Ziele sind, wird die Zieleigenschaft aus der Quelldatenbank festgelegt.

Allerdings der Standardmodus für die Bindung für die `Value` Eigenschaft `Slider` ist `TwoWay`. Dies bedeutet, dass bei der `Value` Eigenschaft ist ein Ziel, das die Datenbindung wird das Ziel aus der Quelle (wie gewohnt) festgelegt ist, jedoch die Quelle ist auch aus der Zieldatenbank festzulegen. Dadurch wird ermöglicht die `Slider` aus dem ersten festzulegende `Opacity` Wert.

Diese bidirektionale Bindung scheint eine Endlosschleife zu erstellen, aber, die nicht der Fall sein. Bindbare Eigenschaften signal Änderung einer Eigenschaft verwenden nicht, es sei denn, die Eigenschaft tatsächlich ändert. Dies verhindert, dass eine Endlosschleife.

### <a name="two-way-bindings"></a>Bidirektionale Bindungen

Die meisten Bindungseigenschaften einen Standardmodus für die Bindung des `OneWay` jedoch die folgenden Eigenschaften aufweisen, einen Bindung Standardmodus `TwoWay`:

- `Date` Eigenschaft `DatePicker`
- `Text` Eigenschaft des `Editor`, `Entry`, `SearchBar`, und `EntryCell`
- `IsRefreshing` Eigenschaft `ListView`
- `SelectedItem` Eigenschaft `MultiPage`
- `SelectedIndex` und `SelectedItem` Eigenschaften `Picker`
- `Value` Eigenschaft des `Slider` und `Stepper`
- `IsToggled` Eigenschaft `Switch` 
- `On` Eigenschaft `SwitchCell`
- `Time` Eigenschaft `TimePicker`

Diese bestimmten Eigenschaften sind definiert als `TwoWay` für eine sehr gute Gründe: 

Wenn datenbindungen mit Model-View-ViewModel (MVVM)-Anwendungsarchitektur verwendet werden, ist das ViewModel-Klasse der Datenbindungsquelle und der Ansicht, die z. B. aus Sichten besteht `Slider`, sind die Datenbindung Ziele. MVVM Bindungen ähneln den **umkehren binden** Beispiel mehr als die Bindungen in den vorherigen Beispielen. Es ist sehr wahrscheinlich, dass jede Ansicht auf der Seite mit den Wert der entsprechenden Eigenschaft im ViewModel initialisiert werden, jedoch die Änderungen in der Ansicht sollten auch Auswirkungen auf das ViewModel-Eigenschaft.

Die Eigenschaften für standardmäßige Bindung Betriebsmodi `TwoWay` sind die Eigenschaften, die höchstwahrscheinlich in MVVM-Szenarien verwendet werden.

### <a name="one-way-to-source-bindings"></a>One-Wege-zu-Source-Bindungen

Nur-Lese Bindungseigenschaften einen Standardmodus für die Bindung des `OneWayToSource`. Ist nur eine bindbare Lese-/Schreibzugriff-Eigenschaft, die einen Bindung Standardmodus `OneWayToSource`:

- `SelectedItem` Eigenschaft `ListView`

Der Grund ist, die eine Bindung auf den `SelectedItem` Eigenschaft sollten dazu führen, indem Sie die Bindungsquelle. Ein Beispiel weiter unten in diesem Artikel wird dieses Verhalten überschrieben.

### <a name="one-time-bindings"></a>Einmalige Bindungen

Mehrere Eigenschaften aufweisen, einen Bindung Standardmodus `OneTime`. Diese lauten wie folgt:

- `IsTextPredictionEnabled` Eigenschaft `Entry`
- `Text`, `BackgroundColor`, und `Style` Eigenschaften des `Span`.

Eigenschaften mit dem Bindungsmodus als Ziel `OneTime` werden nur aktualisiert, wenn der Bindungskontext ändert. Für Bindungen an diese Zieleigenschaften vereinfacht dies die bindungsinfrastruktur, da sie nicht zum Überwachen von Änderungen in den Datenquelleneigenschaften ist.

## <a name="viewmodels-and-property-change-notifications"></a>ViewModels und-Eigenschaftenänderung Benachrichtigungen

Die **einfache Farbauswahl** Seite veranschaulicht die Verwendung von einem einfachen ViewModel. Datenbindungen ermöglicht dem Benutzer das auswählen eine Farbe mit drei `Slider` Elemente für den Farbton, Sättigung und Helligkeit.

Das ViewModel ist der Datenbindungsquelle. Ist das ViewModel *nicht* bindbare Eigenschaften definieren, aber er implementiert einen Benachrichtigungsmechanismus, der ermöglicht die bindungsinfrastruktur benachrichtigt, wenn der Wert einer Eigenschaft ändert. Dieser Benachrichtigungsmechanismus ist die [ `INotifyPropertyChanged` ](https://developer.xamarin.com/api/type/System.ComponentModel.INotifyPropertyChanged/) -Schnittstelle, die eine einzelne Eigenschaft mit dem Namen definiert [ `PropertyChanged` ](https://developer.xamarin.com/api/event/System.ComponentModel.INotifyPropertyChanged.PropertyChanged/). Eine Klasse, die diese Schnittstelle implementiert, die in der Regel wird ausgelöst, das Ereignis, wenn eine der öffentlichen Eigenschaften Wert ändert. Das Ereignis muss nicht ausgelöst werden, wenn die Eigenschaft nicht geändert. (Die `INotifyPropertyChanged` Schnittstelle wird auch von implementiert `BindableObject` und ein `PropertyChanged` Ereignis wird ausgelöst, wenn eine bindbare Eigenschaft Wert ändert.)

Die `HslColorViewModel` Klasse definiert fünf Eigenschaften: die `Hue`, `Saturation`, `Luminosity`, und `Color` sind Eigenschaften verknüpft. Wenn eine der drei Komponenten Farb-Änderungen-Wert, der `Color` Eigenschaft ist neu berechnet, und `PropertyChanged` Ereignisse werden ausgelöst, für alle vier Eigenschaften:

```csharp
public class HslColorViewModel : INotifyPropertyChanged
{
    Color color;
    string name; 

    public event PropertyChangedEventHandler PropertyChanged;

    public double Hue
    {
        set
        {
            if (color.Hue != value)
            {
                Color = Color.FromHsla(value, color.Saturation, color.Luminosity);
            }
        }
        get 
        {
            return color.Hue;
        }
    }

    public double Saturation
    {
        set
        {
            if (color.Saturation != value)
            {
                Color = Color.FromHsla(color.Hue, value, color.Luminosity);
            }
        }
        get
        {
            return color.Saturation;
        }
    }

    public double Luminosity
    {
        set
        {
            if (color.Luminosity != value)
            {
                Color = Color.FromHsla(color.Hue, color.Saturation, value);
            }
        }
        get
        {
            return color.Luminosity;
        }
    }

    public Color Color
    {
        set
        {
            if (color != value)
            {
                color = value;
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Hue"));
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Saturation"));
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Luminosity"));
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Color"));

                Name = NamedColor.GetNearestColorName(color);
            }
        }
        get
        {
            return color;
        }
    }

    public string Name
    {
        private set
        {
            if (name != value)
            {
                name = value;
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Name"));
            }
        }
        get
        {
            return name;
        }
    }
}
```

Bei der `Color` eigenschaftenänderungen, die statische `GetNearestColorName` Methode in der `NamedColor` Klasse (auch in enthalten die **DataBindingDemos** Lösung) erhält die nächstgelegene benannte Farbe, und legt sie fest die `Name` Eigenschaft. Dies `Name` Eigenschaft verfügt über einen privaten `set` Accessor, damit sie von außerhalb der Klasse festgelegt werden kann.

Wenn ein ViewModel als Bindungsquelle festgelegt ist, fügt die bindungsinfrastruktur einen Handler, der `PropertyChanged` Ereignis. Auf diese Weise wird die Bindung benachrichtigt werden, Änderungen an den Eigenschaften und kann dann die Zieleigenschaften aus die geänderten Werte festlegen.

Jedoch wenn eine Zieleigenschaft (oder die `Binding` Definition auf eine Zieleigenschaft) hat eine `BindingMode` von `OneTime`, es ist nicht notwendig, für die bindungsinfrastruktur an einen Handler für die `PropertyChanged` Ereignis. Die Zieleigenschaft aktualisiert nur, wenn die `BindingContext` Änderungen und nicht wenn die Source-Eigenschaft selbst ändert. 

Die **einfache Farbauswahl** XAML-Datei instanziiert den `HslColorViewModel` im Ressourcenverzeichnis und initialisiert der Seite die `Color` Eigenschaft. Die `BindingContext` Eigenschaft der `Grid` festgelegt ist, um eine `StaticResource` bindungserweiterung auf diese Ressource verweisen:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.SimpleColorSelectorPage">

    <ContentPage.Resources>
        <ResourceDictionary>
            <local:HslColorViewModel x:Key="viewModel" 
                                     Color="MediumTurquoise" />

            <Style TargetType="Slider">
                <Setter Property="VerticalOptions" Value="CenterAndExpand" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>
        
    <Grid BindingContext="{StaticResource viewModel}">
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <BoxView Color="{Binding Color}"
                 Grid.Row="0" />

        <StackLayout Grid.Row="1"
                     Margin="10, 0">

            <Label Text="{Binding Name}"
                   HorizontalTextAlignment="Center" />

            <Slider Value="{Binding Hue}" />
    
            <Slider Value="{Binding Saturation}" />

            <Slider Value="{Binding Luminosity}" />
        </StackLayout>
    </Grid>
</ContentPage>
```

Die `BoxView`, `Label`, und drei `Slider` Ansichten erben den Bindungskontext aus der `Grid`. Die Sichten befinden sich alle Bindungsziele, die Datenquelleneigenschaften im ViewModel verweisen. Für die `Color` Eigenschaft der `BoxView`, und die `Text` Eigenschaft der `Label`, sind die datenbindungen `OneWay`: die Eigenschaften in der Sicht werden von den Eigenschaften in das ViewModel festgelegt.

Die `Value` Eigenschaft von der `Slider`, ist jedoch `TwoWay`. Dadurch kann jede `Slider` ViewModel und auch für das ViewModel aus allen festzulegende festgelegt werden `Slider`. 

Wenn das Programm zuerst ausgeführt wird, der `BoxView`, `Label`, und drei `Slider` Elemente sind alle Satz von ViewModel basierend auf dem ersten `Color` -Eigenschaft festgelegt wird, wenn das ViewModel instanziiert wurde. Dies wird im auf der linken Seite der iOS-Screenshot gezeigt:

[![Einfache Farbauswahl](binding-mode-images/simplecolorselector-small.png "einfache Farbauswahl")](binding-mode-images/simplecolorselector-large.png#lightbox "einfache Farbauswahl")

Wie Sie die Schieberegler Bearbeiten der `BoxView` und `Label` werden wie in der Android- und uwp-Screenshots veranschaulicht entsprechend aktualisiert.

Instanziieren das ViewModel im Ressourcenverzeichnis ist ein gebräuchliches Verfahren. Es ist auch möglich, instanziieren das ViewModel innerhalb Eigenschaftselementtags für die `BindingContext` Eigenschaft. In der **einfache Farbauswahl** XAML Datei, entfernen Sie die `HslColorViewModel` aus dem Ressourcenverzeichnis und legen Sie sie auf die `BindingContext` Eigenschaft von der `Grid` wie folgt:

```xaml
<Grid>
    <Grid.BindingContext>
        <local:HslColorViewModel Color="MediumTurquoise" />
    </Grid.BindingContext>
        
    ···

</Grid>
```

Der Bindungskontext kann eine Vielzahl von Möglichkeiten festgelegt werden. In einigen Fällen, die Code-Behind-Datei ViewModel instanziiert und legt es auf die `BindingContext` -Eigenschaft der Seite. Hierbei handelt es sich um gültige Ansätze.

## <a name="overriding-the-binding-mode"></a>Überschreiben den Bindungsmodus

Ist der Standardmodus für die Bindung für die Eigenschaft nicht für eine bestimmte Bindung geeignet, es ist möglich, überschreiben Sie es durch Festlegen der [ `Mode` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindingBase.Mode/) Eigenschaft `Binding` (oder die [ `Mode` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Xaml.BindingExtension.Mode/) Eigenschaft von der `Binding` Markuperweiterung) zu einem Mitglied der `BindingMode` Enumeration.

Festlegen der `Mode` Eigenschaft `TwoWay` immer funktioniert nicht, wie zu erwarten. Versuchen Sie z. B. Ändern der **Alternative Verwendung von XAML-Bindung** XAML-Datei einschließen `TwoWay` in der Bindungsdefinition:

```xaml
<Label Text="TEXT"
       FontSize="40"
       HorizontalOptions="Center"
       VerticalOptions="CenterAndExpand"
       Scale="{Binding Source={x:Reference slider},
                       Path=Value,
                       Mode=TwoWay}" />
```

Es kann davon ausgegangen, die die `Slider` würde initialisiert werden, um den anfänglichen Wert des der `Scale` Eigenschaft ist 1, aber nicht, die zieht. Wenn eine `TwoWay` Bindung initialisiert wird, das Ziel ist aus der Quelle legen Sie zuerst fest, was bedeutet, dass die `Scale` -Eigenschaftensatz auf die `Slider` Standardwert 0. Wenn die `TwoWay` Bindung festgelegt ist, auf die `Slider`, die `Slider` wird anfänglich von der Quelle festgelegt.

Sie können den Bindungsmodus festlegen, um `OneWayToSource` in der **Alternative Verwendung von XAML-Bindung** Beispiel:

```xaml
<Label Text="TEXT"
       FontSize="40"
       HorizontalOptions="Center"
       VerticalOptions="CenterAndExpand"
       Scale="{Binding Source={x:Reference slider},
                       Path=Value,
                       Mode=OneWayToSource}" />
```

Jetzt die `Slider` mit 1 initialisiert wird (der Standardwert `Scale`) jedoch Bearbeiten von der `Slider` keinen Einfluss auf die `Scale` -Eigenschaft, damit diese nicht sehr hilfreich ist.

Eine sehr nützliche Anwendung Überschreiben des Standardmodus für die Bindung mit `TwoWay` umfasst die `SelectedItem` Eigenschaft `ListView`. Der Standardmodus für die Bindung ist `OneWayToSource`. Wann wird eine Datenbindung festzulegen, auf die `SelectedItem` Eigenschaft, um die Quelleigenschaft in einem ViewModel verweisen, und klicken Sie dann aus dieser Quelleigenschaft festgelegt ist die `ListView` Auswahl. Jedoch unter bestimmten Umständen, Sie sollten auch die `ListView` von ViewModel initialisiert werden.

Die **Beispiel Einstellungen** Seite wird diese Technik veranschaulicht. Diese Seite stellt eine einfache Implementierung von Anwendungseinstellungen, die sehr häufig in einem ViewModel, wie diese definiert sind `SampleSettingsViewModel` Datei:

```csharp
public class SampleSettingsViewModel : INotifyPropertyChanged
{
    string name;
    DateTime birthDate;
    bool codesInCSharp;
    double numberOfCopies;
    NamedColor backgroundNamedColor;

    public event PropertyChangedEventHandler PropertyChanged;

    public SampleSettingsViewModel(IDictionary<string, object> dictionary)
    {
        Name = GetDictionaryEntry<string>(dictionary, "Name");
        BirthDate = GetDictionaryEntry(dictionary, "BirthDate", new DateTime(1980, 1, 1));
        CodesInCSharp = GetDictionaryEntry<bool>(dictionary, "CodesInCSharp");
        NumberOfCopies = GetDictionaryEntry(dictionary, "NumberOfCopies", 1.0);
        BackgroundNamedColor = NamedColor.Find(GetDictionaryEntry(dictionary, "BackgroundNamedColor", "White"));
    }

    public string Name
    {
        set { SetProperty(ref name, value); }
        get { return name; }
    }

    public DateTime BirthDate
    {
        set { SetProperty(ref birthDate, value); }
        get { return birthDate; }
    }

    public bool CodesInCSharp
    {
        set { SetProperty(ref codesInCSharp, value); }
        get { return codesInCSharp; }
    }

    public double NumberOfCopies
    {
        set { SetProperty(ref numberOfCopies, value); }
        get { return numberOfCopies; }
    }

    public NamedColor BackgroundNamedColor
    {
        set
        {
            if (SetProperty(ref backgroundNamedColor, value))
            {
                OnPropertyChanged("BackgroundColor");
            }
        }
        get { return backgroundNamedColor; }
    }

    public Color BackgroundColor
    {
        get { return BackgroundNamedColor?.Color ?? Color.White; }
    }

    public void SaveState(IDictionary<string, object> dictionary)
    {
        dictionary["Name"] = Name;
        dictionary["BirthDate"] = BirthDate;
        dictionary["CodesInCSharp"] = CodesInCSharp;
        dictionary["NumberOfCopies"] = NumberOfCopies;
        dictionary["BackgroundNamedColor"] = BackgroundNamedColor.Name;
    }

    T GetDictionaryEntry<T>(IDictionary<string, object> dictionary, string key, T defaultValue = default(T))
    {
        return dictionary.ContainsKey(key) ? (T)dictionary[key] : defaultValue;
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

Jede anwendungseinstellung ist eine Eigenschaft, die im Eigenschaftenwörterbuch Xamarin.Forms in einer Methode namens gespeichert ist `SaveState` und aus diesem Wörterbuch im Konstruktor geladen. Sind Sie im unteren Teil der Klasse zwei Methoden, mit deren Hilfe ViewModels optimieren, und stellen sie weniger fehleranfällig. Die `OnPropertyChanged` Methode im unteren Bereich ist einen optionalen Parameter, die an die aufrufende-Eigenschaft festgelegt wird. Dies vermeidet Rechtschreibfehler, wenn Sie den Namen der Eigenschaft als Zeichenfolge angeben. 

Die `SetProperty` Methode in der Klasse hat noch mehr: Es vergleicht den Wert, der mit dem Wert gespeichert als Feld für die Eigenschaft festgelegt wird, und nur ruft `OnPropertyChanged` Wenn die beiden Werte ungleich sind. 

Die `SampleSettingsViewModel` Klasse definiert zwei Eigenschaften für die Farbe des Hintergrunds: die `BackgroundNamedColor` -Eigenschaft ist vom Typ `NamedColor`, eine Klasse auch im Lieferumfang der **DataBindingDemos** Lösung. Die `BackgroundColor` -Eigenschaft ist vom Typ `Color`, und stammt von der `Color` Eigenschaft von der `NamedColor` Objekt.

Die `NamedColor` Klasse verwendet .NET Reflektion zum Aufzählen aller statische öffentliche Felder in der Xamarin.Forms `Color` -Struktur, und sie mit ihren Namen in einer Auflistung zugegriffen werden kann, aus der statischen speichern `All` Eigenschaft:

```csharp
public class NamedColor : IEquatable<NamedColor>, IComparable<NamedColor>
{
    // Instance members
    private NamedColor()
    {
    }

    public string Name { private set; get; }

    public string FriendlyName { private set; get; }

    public Color Color { private set; get; }

    public string RgbDisplay { private set; get; }

    public bool Equals(NamedColor other)
    {
        return Name.Equals(other.Name);
    }

    public int CompareTo(NamedColor other)
    {
        return Name.CompareTo(other.Name);
    }

    // Static members
    static NamedColor()
    {
        List<NamedColor> all = new List<NamedColor>();
        StringBuilder stringBuilder = new StringBuilder();

        // Loop through the public static fields of the Color structure.
        foreach (FieldInfo fieldInfo in typeof(Color).GetRuntimeFields())
        {
            if (fieldInfo.IsPublic &&
                fieldInfo.IsStatic &&
                fieldInfo.FieldType == typeof(Color))
            {
                // Convert the name to a friendly name.
                string name = fieldInfo.Name;
                stringBuilder.Clear();
                int index = 0;

                foreach (char ch in name)
                {
                    if (index != 0 && Char.IsUpper(ch))
                    {
                        stringBuilder.Append(' ');
                    }
                    stringBuilder.Append(ch);
                    index++;
                }

                // Instantiate a NamedColor object.
                Color color = (Color)fieldInfo.GetValue(null);

                NamedColor namedColor = new NamedColor
                {
                    Name = name,
                    FriendlyName = stringBuilder.ToString(),
                    Color = color,
                    RgbDisplay = String.Format("{0:X2}-{1:X2}-{2:X2}",
                                                (int)(255 * color.R),
                                                (int)(255 * color.G),
                                                (int)(255 * color.B))
                };

                // Add it to the collection.
                all.Add(namedColor);
            }
        }
        all.TrimExcess();
        all.Sort();
        All = all;
    }

    public static IList<NamedColor> All { private set; get; }

    public static NamedColor Find(string name)
    {
        return ((List<NamedColor>)All).Find(nc => nc.Name == name);
    }

    public static string GetNearestColorName(Color color)
    {
        double shortestDistance = 1000;
        NamedColor closestColor = null;

        foreach (NamedColor namedColor in NamedColor.All)
        {
            double distance = Math.Sqrt(Math.Pow(color.R - namedColor.Color.R, 2) +
                                        Math.Pow(color.G - namedColor.Color.G, 2) +
                                        Math.Pow(color.B - namedColor.Color.B, 2));

            if (distance < shortestDistance)
            {
                shortestDistance = distance;
                closestColor = namedColor;
            }
        }
        return closestColor.Name;
    }
}
```

Die `App` -Klasse in der **DataBindingDemos** Projekt definiert eine Eigenschaft namens `Settings` vom Typ `SampleSettingsViewModel`. Diese Eigenschaft wird initialisiert bei der `App` Klasse instanziiert, und die `SaveState` Methode wird aufgerufen, wenn die `OnSleep` Methode wird aufgerufen:

```csharp
public partial class App : Application
{
    public App()
    {
        InitializeComponent();

        Settings = new SampleSettingsViewModel(Current.Properties);

        MainPage = new NavigationPage(new MainPage());
    }

    public SampleSettingsViewModel Settings { private set; get; }

    protected override void OnStart()
    {
        // Handle when your app starts
    }

    protected override void OnSleep()
    {
        // Handle when your app sleeps
        Settings.SaveState(Current.Properties);
    }

    protected override void OnResume()
    {
        // Handle when your app resumes
    }
}
```

Weitere Informationen zu der Anwendung Lebenszyklusmethoden, finden Sie im Artikel [ **App-Lebenszyklus**](~/xamarin-forms/app-fundamentals/app-lifecycle.md).

Fast alles erfolgt der **SampleSettingsPage.xaml** Datei. Die `BindingContext` von der Seite "festgelegt ist, verwenden eine `Binding` Markuperweiterung: Bindungsquelle ist die statische `Application.Current` -Eigenschaft, die die Instanz ist von der `App` Klasse im Projekt, und die `Path` auf festgelegt ist der `Settings` Eigenschaft, die die `SampleSettingsViewModel` Objekt:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.SampleSettingsPage"
             Title="Sample Settings"
             BindingContext="{Binding Source={x:Static Application.Current},
                                      Path=Settings}">

    <StackLayout BackgroundColor="{Binding BackgroundColor}"
                 Padding="10"
                 Spacing="10">

        <StackLayout Orientation="Horizontal">
            <Label Text="Name: "
                   VerticalOptions="Center" />

            <Entry Text="{Binding Name}"
                   Placeholder="your name"
                   HorizontalOptions="FillAndExpand"
                   VerticalOptions="Center" />
        </StackLayout>

        <StackLayout Orientation="Horizontal">
            <Label Text="Birth Date: "
                   VerticalOptions="Center" />

            <DatePicker Date="{Binding BirthDate}"
                        HorizontalOptions="FillAndExpand"
                        VerticalOptions="Center" />
        </StackLayout>

        <StackLayout Orientation="Horizontal">
            <Label Text="Do you code in C#? "
                   VerticalOptions="Center" />

            <Switch IsToggled="{Binding CodesInCSharp}"
                    VerticalOptions="Center" />
        </StackLayout>

        <StackLayout Orientation="Horizontal">
            <Label Text="Number of Copies: "
                   VerticalOptions="Center" />

            <Stepper Value="{Binding NumberOfCopies}"
                     VerticalOptions="Center" />

            <Label Text="{Binding NumberOfCopies}"
                   VerticalOptions="Center" />
        </StackLayout>

        <Label Text="Background Color:" />

        <ListView x:Name="colorListView"
                  ItemsSource="{x:Static local:NamedColor.All}"
                  SelectedItem="{Binding BackgroundNamedColor, Mode=TwoWay}"
                  VerticalOptions="FillAndExpand"
                  RowHeight="40">
            <ListView.ItemTemplate>
                <DataTemplate>
                    <ViewCell>
                        <StackLayout Orientation="Horizontal">
                            <BoxView Color="{Binding Color}"
                                     HeightRequest="32"
                                     WidthRequest="32"
                                     VerticalOptions="Center" />

                            <Label Text="{Binding FriendlyName}"
                                   FontSize="24"
                                   VerticalOptions="Center" />
                        </StackLayout>                        
                    </ViewCell>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </StackLayout>
</ContentPage>
```

Die untergeordneten Elemente der Seite erben den Bindungskontext. Die meisten anderen Bindungen auf dieser Seite sind Eigenschaften im `SampleSettingsViewModel`. Die `BackgroundColor` Eigenschaft wird zum Festlegen der `BackgroundColor` Eigenschaft der `StackLayout`, und die `Entry`, `DatePicker`, `Switch`, und `Stepper` Eigenschaften sind mit anderen Eigenschaften im ViewModel gebunden.

Die `ItemsSource` Eigenschaft von der `ListView` festgelegt ist, um die statische `NamedColor.All` Eigenschaft. Dies füllt die `ListView` mit allen die `NamedColor` Instanzen. Für jedes Element in der `ListView`, der Bindungskontext für das Element festgelegt ist, um eine `NamedColor` Objekt. Die `BoxView` und `Label` in der `ViewCell` gebunden sind, um Eigenschaften im `NamedColor`.

Die `SelectedItem` Eigenschaft von der `ListView` ist vom Typ `NamedColor`, und gebunden ist die `BackgroundNamedColor` Eigenschaft `SampleSettingsViewModel`:

```xaml
SelectedItem="{Binding BackgroundNamedColor, Mode=TwoWay}"
```

Der Standardmodus für die Bindung für `SelectedItem` ist `OneWayToSource`, wodurch das ViewModel-Eigenschaft aus dem ausgewählten Element. Die `TwoWay` Modus ermöglicht die `SelectedItem` von ViewModel initialisiert werden. 

Jedoch, wenn die `SelectedItem` festgelegt ist, auf diese Weise die `ListView` automatisch Bildlauf nicht um das ausgewählte Element anzuzeigen. Ein wenig Code in der Code-Behind-Datei ist erforderlich:

```csharp
public partial class SampleSettingsPage : ContentPage
{
    public SampleSettingsPage()
    {
        InitializeComponent();

        if (colorListView.SelectedItem != null)
        {
            colorListView.ScrollTo(colorListView.SelectedItem, 
                                   ScrollToPosition.MakeVisible, 
                                   false);
        }
    }
}
``` 

Auf der linken Seite der iOS-Screenshot zeigt das Programm bei der ersten Ausführung. Der Konstruktor in `SampleSettingsViewModel` initialisiert die Hintergrundfarbe weiß, und der Auswahl in der `ListView`:

[![Beispiel-Einstellungen](binding-mode-images/samplesettings-small.png "Beispiel Einstellungen")](binding-mode-images/samplesettings-large.png#lightbox "Beispiel-Einstellungen")

Die anderen zwei Screenshots zeigen geänderte Einstellungen an. Denken Sie daran, Ihnen das Experimentieren mit dieser Seite, setzen das Programm, in den Energiesparmodus oder beenden Sie sie auf dem Gerät oder Emulator, der er ausgeführt wird. Beenden die Anwendung aus Visual Studio-Debugger führt nicht dazu, dass die `OnSleep` außer Kraft setzen, der `App` Klasse aufgerufen werden.

Im nächsten Artikel sehen Sie, wie angegeben [ **Zeichenfolgenformatierung** ](string-formatting.md) von datenbindungen, die für festgelegt werden die `Text` Eigenschaft `Label`.


## <a name="related-links"></a>Verwandte Links

- [Binden von Daten-Demos (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Daten Bindung Kapitel Xamarin.Forms Buchs](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
