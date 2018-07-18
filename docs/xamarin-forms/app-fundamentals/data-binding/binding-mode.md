---
title: Bindungsmodus für Xamarin.Forms
description: In diesem Artikel erläutert die zur Steuerung des Informationsflusses zwischen Quelle und Ziel mithilfe einer Bindungsmodus, der mit einem Member der Enumeration BindingMode angegeben wird. Jede bindbare Eigenschaft verfügt über einen Standardmodus für die Bindung, womit den Modus in Kraft, wenn diese Eigenschaft eine Datenbindung-Ziel ist.
ms.prod: xamarin
ms.assetid: D087C389-2E9E-47B9-A341-5B14AC732C45
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 05/01/2018
ms.openlocfilehash: a6eaf08d17f70c43f451361e27555a09c39f26a9
ms.sourcegitcommit: 3e980fbf92c69c3dd737554e8c6d5b94cf69ee3a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2018
ms.locfileid: "37935666"
---
# <a name="xamarinforms-binding-mode"></a>Bindungsmodus für Xamarin.Forms

In der [vorherigen Artikel](basic-bindings.md), wird die **Alternative Code binden** und **Alternative XAML-Bindung** Seiten, die wichtige eine `Label` mit seiner `Scale` Eigenschaft gebunden werden, um die `Value` Eigenschaft eine `Slider`. Da die `Slider` Anfangswert 0 ist, verursacht dies die `Scale` Eigenschaft der `Label` 0 statt 1 festgelegt werden und die `Label` nicht mehr vorhanden.

In der [ **DataBindingDemos** ](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/) Beispiel der **Reverse-Bindung** Seite ähnelt der Programme im vorgängerartikel, außer dass die Datenbindung für die definiertist`Slider` statt auf die `Label`:

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

Zunächst mag dies rückwärts: jetzt die `Label` ist die Source-Datenbindung und die `Slider` ist das Ziel. Bindungsverweise der `Opacity` Eigenschaft der `Label`, die über einen Standardwert von 1 verfügt.

Wie zu erwarten, die `Slider` auf den Wert 1 initialisiert wird, auf dem ersten `Opacity` Wert `Label`. Dies wird in der iOS-Screenshot auf der linken Seite gezeigt:

[![Reverse-Bindung](binding-mode-images/reversebinding-small.png "Reverse-Bindung")](binding-mode-images/reversebinding-large.png#lightbox "Reverse-Bindung")

Werden aber möglicherweise überrascht es, dass die `Slider` funktioniert weiterhin, wie die Android- und UWP-Screenshots zeigen. Dies weist darauf hin, dass die Datenbindung Situationen vorzuziehen funktioniert die `Slider` ist das Bindungsziel anstelle der `Label` da die Initialisierung verwendet werden kann, wie wir erwarten.

Der Unterschied zwischen der **Reverse-Bindung** Beispiel und den früheren Beispielen umfasst die *Bindungsmodus*.

## <a name="the-default-binding-mode"></a>Der Standardmodus für die Bindung

Der Bindungsmodus wird angegeben, mit einem Member mit dem [ `BindingMode` ](xref:Xamarin.Forms.BindingMode) Enumeration:

- [`Default`](xref:Xamarin.Forms.BindingMode.Default)
- [`TwoWay`](xref:Xamarin.Forms.BindingMode.TwoWay) &ndash; Daten gilt in beide Richtungen zwischen Quelle und Ziel
- [`OneWay`](xref:Xamarin.Forms.BindingMode.OneWay) &ndash; Daten gehen aus der Quelle zum Ziel
- [`OneWayToSource`](xref:Xamarin.Forms.BindingMode.OneWayToSource) &ndash; Daten gehen vom Ziel mit Quelle
- [`OneTime`](xref:Xamarin.Forms.BindingMode.OneWayToSource) &ndash; Daten gehen aus der Quelle zum Ziel, aber nur, wenn die `BindingContext` Änderungen (mit Xamarin.Forms 3.0 neu)

Jede bindbare Eigenschaft hat den Standardwert Bindungsmodus, der festgelegt wird, wenn die bindbare Eigenschaft erstellt wurde, und die verfügbar ist. die [ `DefaultBindingMode` ](xref:Xamarin.Forms.BindableProperty.DefaultBindingMode) Eigenschaft der `BindableProperty` Objekt. Dieser Standardmodus für die Bindung gibt den Modus in Kraft, wenn diese Eigenschaft eine Datenbindung-Ziel ist.

Der Standardmodus für die Bindung für die meisten Eigenschaften, z. B. `Rotation`, `Scale`, und `Opacity` ist `OneWay`. Wenn diese Eigenschaften über die Datenbindung Ziele sind, ist Klicken Sie dann die Zieleigenschaft aus der Quelle festgelegt.

Jedoch den Standardmodus für die Bindung für die `Value` Eigenschaft `Slider` ist `TwoWay`. Das bedeutet, wenn die `Value` -Eigenschaft ist ein Ziel für die Datenbindung, und klicken Sie dann das Ziel aus der Quelle (wie gewohnt) festgelegt ist, aber auch die Quelle vom Ziel festgelegt ist. Dies kann die `Slider` auf dem ersten festzulegenden `Opacity` Wert.

Diese bidirektionale Bindung scheint eine Endlosschleife zu erstellen, aber dies nicht der Fall. Bindbare Eigenschaften sind keine Eigenschaftenänderung signalisieren, solange die Eigenschaft nicht tatsächlich ändern. Dies verhindert eine unendliche Schleife.

### <a name="two-way-bindings"></a>Bidirektionale Bindungen

Die bindbare Eigenschaften haben einen Standardmodus für die Bindung der `OneWay` jedoch die folgenden Eigenschaften verfügen, einen Bindung Standardmodus des `TwoWay`:

- `Date` Eigenschaft `DatePicker`
- `Text` Eigenschaft des `Editor`, `Entry`, `SearchBar`, und `EntryCell`
- `IsRefreshing` Eigenschaft `ListView`
- `SelectedItem` Eigenschaft `MultiPage`
- `SelectedIndex` und `SelectedItem` Eigenschaften `Picker`
- `Value` Eigenschaft des `Slider` und `Stepper`
- `IsToggled` Eigenschaft `Switch`
- `On` Eigenschaft `SwitchCell`
- `Time` Eigenschaft `TimePicker`

Diese bestimmten Eigenschaften sind definiert als `TwoWay` für einen sehr guten Grund:

Wenn datenbindungen, die mit der Anwendungsarchitektur der Model-View-ViewModel (MVVM)-verwendet werden, die ViewModel-Klasse ist die Quelle für die Datenbindung und die Ansicht, die Ansichten besteht, z. B. `Slider`, sind die Datenbindung-Ziele. Bindungen mit MVVM ähneln den **Reverse-Bindung** Beispiel mehr als die Bindungen in den vorherigen Beispielen. Es ist sehr wahrscheinlich, dass Sie jeder Ansicht auf der Seite mit den Wert der entsprechenden Eigenschaft im ViewModel initialisiert werden, aber Änderungen in der Ansicht sollte auch Auswirkungen auf die Eigenschaft "ViewModel".

Die Eigenschaften mit Standardwert Bindungsmodi von `TwoWay` sind die Eigenschaften, die am wahrscheinlichsten in MVVM-Szenarien verwendet werden.

### <a name="one-way-to-source-bindings"></a>One-unidirektionale-to-Source-Bindungen

Nur-Lese bindbare Eigenschaften haben einen Standardmodus für die Bindung der `OneWayToSource`. Es gibt nur eine bindbare Lese-/Schreibzugriff-Eigenschaft, die einen Standardmodus für die Bindung der `OneWayToSource`:

- `SelectedItem` Eigenschaft `ListView`

Der Grund ist, die eine Bindung auf den `SelectedItem` Eigenschaft sollten dazu führen, indem Sie die Bindungsquelle. Ein Beispiel weiter unten in diesem Artikel wird dieses Verhalten überschrieben.

### <a name="one-time-bindings"></a>Einmalige Bindungen

Mehrere Eigenschaften haben einen Standardmodus für die Bindung der `OneTime`. Diese lauten wie folgt:

- `IsTextPredictionEnabled` Eigenschaft `Entry`
- `Text`, `BackgroundColor`, und `Style` Eigenschaften `Span`.

Eigenschaften mit einem Bindungsmodus, der als Ziel `OneTime` werden nur aktualisiert, wenn der Bindungskontext ändert. Für Bindungen an diese Eigenschaften als Ziel vereinfacht dies die bindungsinfrastruktur, da es nicht zum Überwachen von Änderungen in den Datenquelleneigenschaften erforderlich ist.

## <a name="viewmodels-and-property-change-notifications"></a>ViewModels und Benachrichtigungen für Eigenschaftsänderungen

Die **einfache Farbauswahl** Seite veranschaulicht die Verwendung eines einfachen "ViewModels". Datenbindungen, die dem Benutzer ermöglichen, wählen Sie eine Farbe, die drei `Slider` Elemente für den Farbton, Sättigung und Helligkeit.

Das "ViewModel" ist die Quelle für die Datenbindung. Ist das "ViewModel" *nicht* bindbare Eigenschaften zu definieren, aber sie implementiert einen Benachrichtigungsmechanismus, der ermöglicht die bindungsinfrastruktur benachrichtigt, wenn der Wert einer Eigenschaft ändert. Durch diesen Benachrichtigungsmechanismus ist die [ `INotifyPropertyChanged` ](xref:System.ComponentModel.INotifyPropertyChanged) Schnittstelle, die eine einzelne Eigenschaft namens definiert [ `PropertyChanged` ](xref:System.ComponentModel.INotifyPropertyChanged.PropertyChanged). Eine Klasse, die diese Schnittstelle implementiert, die in der Regel wird ausgelöst, das Ereignis, wenn eine von dessen öffentliche Eigenschaften ändert. Das Ereignis muss nicht ausgelöst werden, wenn die Eigenschaft sich nie ändert. (Die `INotifyPropertyChanged` Schnittstelle wird auch von implementiert `BindableObject` und `PropertyChanged` Ereignis wird ausgelöst, wenn sich eine bindbare Eigenschaft Wert ändert.)

Die `HslColorViewModel` -Klasse definiert fünf Eigenschaften: die `Hue`, `Saturation`, `Luminosity`, und `Color` Eigenschaften stehen in wechselseitiger Beziehung. Wenn eine der drei Farbwert Komponenten Änderungen, die `Color` Eigenschaft neu berechnet, und `PropertyChanged` Ereignisse werden ausgelöst, für alle vier Eigenschaften:

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

Bei der `Color` eigenschaftenänderungen, die statische `GetNearestColorName` -Methode in der der `NamedColor` Klasse (enthält auch die **DataBindingDemos** Lösung) Ruft die nächste benannte Farbe, und legt ihn fest der `Name` Eigenschaft. Dies `Name` Eigenschaft verfügt über ein privates `set` Zugriffsmethode, damit sie von außerhalb der Klasse festgelegt werden kann.

Wenn ein "ViewModel" als Bindungsquelle festgelegt ist, fügt die bindungsinfrastruktur einen Handler, der die `PropertyChanged` Ereignis. Auf diese Weise wird die Bindung von Änderungen an den Eigenschaften benachrichtigt werden und kann dann die Zieleigenschaften aus den geänderten Werten festlegen.

Jedoch bei einer Zieleigenschaft (oder die `Binding` Definition auf eine Zieleigenschaft) verfügt über eine `BindingMode` von `OneTime`, es ist nicht erforderlich, für die bindungsinfrastruktur zum Anfügen eines Handlers für die `PropertyChanged` Ereignis. Aktualisiert die Zieleigenschaft nur dann, wenn die `BindingContext` Änderungen und nicht, wenn die Source-Eigenschaft selbst ändert.

Die **einfache Farbauswahl** XAML-Datei instanziiert den `HslColorViewModel` im Ressourcenverzeichnis der Seite, und initialisiert die `Color` Eigenschaft. Die `BindingContext` Eigenschaft der `Grid` nastaven NA hodnotu eine `StaticResource` bindungserweiterung auf diese Ressource zu verweisen:

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

Die `BoxView`, `Label`, und drei `Slider` Ansichten erben den Bindungskontext aus der `Grid`. Die Sichten befinden sich alle Bindungsziele, die Eigenschaften der Quelle in das "ViewModel" verweisen. Für die `Color` Eigenschaft der `BoxView`, und die `Text` Eigenschaft der `Label`, die datenbindungen werden `OneWay`: die Eigenschaften in der Ansicht werden von den Eigenschaften in "ViewModel" festgelegt.

Die `Value` Eigenschaft der `Slider`, ist jedoch `TwoWay`. Dadurch kann jede `Slider` festgelegt werden, aus dem ViewModel sowie für das "ViewModel" aus jedem festzulegende `Slider`.

Wenn die Anwendung zuerst ausgeführt wird, die `BoxView`, `Label`, und drei `Slider` Elemente können aus "ViewModel" basierend auf dem ersten `Color` -Eigenschaft festgelegt wird, wenn das "ViewModel" instanziiert wurde. Dies wird in der iOS-Screenshot auf der linken Seite gezeigt:

[![Einfache Farbauswahl](binding-mode-images/simplecolorselector-small.png "einfache Farbauswahl")](binding-mode-images/simplecolorselector-large.png#lightbox "einfache Farbauswahl")

Wie Sie die Schieberegler, Ändern der `BoxView` und `Label` , wie die Android- und UWP-Screenshots dargestellt, entsprechend aktualisiert werden.

Instanziieren von ViewModel im Ressourcenverzeichnis ist eine gängige Methode. Es ist auch möglich, im Eigenschaftenelement-Tags für "ViewModel" instanziiert die `BindingContext` Eigenschaft. In der **einfache Farbauswahl** XAML-Datei, entfernen Sie die `HslColorViewModel` aus dem Ressourcenverzeichnis und legen ihn auf die `BindingContext` Eigenschaft der `Grid` wie folgt aus:

```xaml
<Grid>
    <Grid.BindingContext>
        <local:HslColorViewModel Color="MediumTurquoise" />
    </Grid.BindingContext>

    ···

</Grid>
```

Der Bindungskontext kann auf vielfältige Weise festgelegt werden. In einigen Fällen die Code-Behind-Datei instanziiert das "ViewModel" und legt es auf die `BindingContext` -Eigenschaft der Seite. Hierbei handelt es sich um gültige Ansätze.

## <a name="overriding-the-binding-mode"></a>Überschreiben den Bindungsmodus

Ist der standardbindungsmodus für die Zieleigenschaft nicht für die einer bestimmten Bindung geeignet ist, ist es möglich, überschreiben Sie sie durch Festlegen der [ `Mode` ](xref:Xamarin.Forms.BindingBase.Mode) Eigenschaft `Binding` (oder die [ `Mode` ](xref:Xamarin.Forms.Xaml.BindingExtension.Mode) Eigenschaft der `Binding` Markuperweiterung) zu einem Mitglied der `BindingMode` Enumeration.

Festlegen der `Mode` Eigenschaft `TwoWay` funktioniert nicht immer wie zu erwarten. Versuchen Sie es z. B. mit dem Ändern der **Alternative XAML-Bindung** XAML-Datei eingeschlossen `TwoWay` in der Bindungsdefinition:

```xaml
<Label Text="TEXT"
       FontSize="40"
       HorizontalOptions="Center"
       VerticalOptions="CenterAndExpand"
       Scale="{Binding Source={x:Reference slider},
                       Path=Value,
                       Mode=TwoWay}" />
```

Es erwartet werden kann, die die `Slider` initialisiert werden würde, auf den ursprünglichen Wert von der `Scale` -Eigenschaft, die ist-1, aber dies nicht der Fall. Wenn eine `TwoWay` Bindung initialisiert wird, ist das Ziel aus der Quelle zuerst, was bedeutet, dass die `Scale` -Eigenschaftensatz auf die `Slider` Standardwert 0. Wenn die `TwoWay` Bindung festgelegt wird, auf die `Slider`, die `Slider` zunächst aus der Quelle festgelegt ist.

Sie können den Bindungsmodus festlegen, um `OneWayToSource` in die **Alternative XAML-Bindung** Beispiel:

```xaml
<Label Text="TEXT"
       FontSize="40"
       HorizontalOptions="Center"
       VerticalOptions="CenterAndExpand"
       Scale="{Binding Source={x:Reference slider},
                       Path=Value,
                       Mode=OneWayToSource}" />
```

Jetzt die `Slider` mit 1 initialisiert wird (der Standardwert von `Scale`) aber Bearbeiten der `Slider` hat keine Auswirkungen auf die `Scale` -Eigenschaft, sodass dies nicht sehr nützlich ist.

Eine sehr nützliche Anwendung Überschreiben der Standardmodus für die Bindung mit `TwoWay` umfasst die `SelectedItem` Eigenschaft `ListView`. Der Standardmodus für die Bindung ist `OneWayToSource`. Wenn eine Bindung festgelegt ist, auf die `SelectedItem` -Eigenschaft zum Verweisen auf eine Quelleigenschaft in ein "ViewModel", und klicken Sie dann aus dieser Quelleigenschaft festgelegt ist die `ListView` Auswahl. Allerdings unter Umständen, Sie sollten auch die `ListView` aus dem ViewModel initialisiert werden.

Die **Beispieleinstellungen** Seite wird diese Technik veranschaulicht. Diese Seite stellt eine einfache Implementierung von Anwendungseinstellungen, die sehr häufig in einem ViewModel, wie diese definiert sind `SampleSettingsViewModel` Datei:

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

Jede anwendungseinstellung ist eine Eigenschaft, die im Eigenschaftenwörterbuch "Xamarin.Forms" in einer Methode namens gespeichert ist `SaveState` und von diesem Wörterbuch im Konstruktor geladen. Sind Sie im unteren Bereich der Klasse zwei Methoden, die Optimierung von ViewModels und weniger fehleranfällig zu machen. Die `OnPropertyChanged` Methode am unteren Rand hat einen optionalen Parameter, die an die aufrufende Eigenschaft festgelegt ist. Dies vermeidet Rechtschreibfehler, wenn Sie den Namen der Eigenschaft als Zeichenfolge angeben.

Die `SetProperty` Methode in der Klasse ist sogar noch mehr: sie vergleicht den Wert, der mit dem Wert, der als Feld gespeichert, die Eigenschaft festgelegt wird, und nur ruft `OnPropertyChanged` Wenn die beiden Werte ungleich sind.

Die `SampleSettingsViewModel` Klasse definiert zwei Eigenschaften für die Farbe des Hintergrunds: die `BackgroundNamedColor` Eigenschaft ist vom Typ `NamedColor`, eine Klasse auch im Lieferumfang der **DataBindingDemos** Lösung. Die `BackgroundColor` Eigenschaft ist vom Typ `Color`, und aus einer der `Color` Eigenschaft der `NamedColor` Objekt.

Die `NamedColor` Klasse verwendet .NET Reflection zum Aufzählen von statischen öffentlichen Felder in der Xamarin.Forms `Color` Struktur, und speichern Sie sie mit ihren Namen in einer Auflistung zugegriffen werden kann, aus der statischen `All` Eigenschaft:

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

Die `App` -Klasse in der **DataBindingDemos** Projekt definiert eine Eigenschaft namens `Settings` des Typs `SampleSettingsViewModel`. Diese Eigenschaft wird initialisiert bei der `App` Klasse instanziiert wird, und die `SaveState` Methode wird aufgerufen, wenn die `OnSleep` Methode wird aufgerufen:

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

Weitere Informationen zu den Application Lifecycle-Methoden finden Sie im Artikel [ **App-Lebenszyklus**](~/xamarin-forms/app-fundamentals/app-lifecycle.md).

Fast alles andere erfolgt der **SampleSettingsPage.xaml** Datei. Die `BindingContext` der Seite festgelegt ist, verwenden eine `Binding` Markuperweiterung: die Bindungsquelle handelt, die statische `Application.Current` -Eigenschaft, die die Instanz ist von der `App` Klasse im Projekt, und die `Path` nastaven NA hodnotu der `Settings` Eigenschaft, die die `SampleSettingsViewModel` Objekt:

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

Alle untergeordneten Elemente der Seite erbt den Bindungskontext. Die meisten anderen Bindungen auf dieser Seite sind Eigenschaften im `SampleSettingsViewModel`. Die `BackgroundColor` Eigenschaft dient zum Festlegen der `BackgroundColor` Eigenschaft der `StackLayout`, und die `Entry`, `DatePicker`, `Switch`, und `Stepper` Eigenschaften werden auf andere Eigenschaften in "ViewModel" gebunden.

Die `ItemsSource` Eigenschaft der `ListView` festgelegt ist, an die statische `NamedColor.All` Eigenschaft. Diese füllt die `ListView` mit allen dem `NamedColor` Instanzen. Für jedes Element in der `ListView`, der Bindungskontext für das Element festgelegt ist, um eine `NamedColor` Objekt. Die `BoxView` und `Label` in die `ViewCell` an Eigenschaften gebunden sind `NamedColor`.

Die `SelectedItem` Eigenschaft der `ListView` ist vom Typ `NamedColor`, gebunden ist, und der `BackgroundNamedColor` Eigenschaft `SampleSettingsViewModel`:

```xaml
SelectedItem="{Binding BackgroundNamedColor, Mode=TwoWay}"
```

Der Standardmodus für die Bindung für `SelectedItem` ist `OneWayToSource`, wodurch die ViewModel-Eigenschaft aus dem ausgewählten Element festgelegt. Die `TwoWay` -Modus ermöglicht die `SelectedItem` aus dem ViewModel initialisiert werden.

Jedoch, wenn die `SelectedItem` festgelegt ist, auf diese Weise die `ListView` wird nicht automatisch scrollen, um das ausgewählte Element anzuzeigen. Ein wenig Code in der CodeBehind-Datei ist erforderlich:

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

Der iOS-Screenshot auf der linken Seite zeigt das Programm aus, wenn er zuerst ausgeführt wird. Der Konstruktor in `SampleSettingsViewModel` initialisiert die Hintergrundfarbe auf weiß, und der Auswahl in der `ListView`:

[![Beispiel für Einstellungen](binding-mode-images/samplesettings-small.png "Beispiel Einstellungen")](binding-mode-images/samplesettings-large.png#lightbox "Beispiel-Einstellungen")

Geänderte Einstellungen werden von den anderen beiden Screenshots anzeigen. Wenn Sie mit dieser Seite zu experimentieren, denken Sie daran, platzieren Sie das Programm in den Ruhezustand versetzt oder beenden Sie sie auf dem Gerät oder Emulator, der er ausgeführt wird. Beenden die Anwendung aus Visual Studio-Debugger führt nicht dazu, dass die `OnSleep` außer Kraft setzen, der `App` Klasse aufgerufen wird.

Im nächsten Artikel erfahren Sie, wie angegeben [ **Formatierung von Zeichenfolgen** ](string-formatting.md) datenbindungen, die für festgelegt sind das `Text` Eigenschaft `Label`.


## <a name="related-links"></a>Verwandte Links

- [Binden von Daten-Demos (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Im Kapitel Daten-Bindung von Xamarin.Forms-Buch](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
