---
title: Bindungsmodus in Xamarin.Forms
description: ''
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: c14215071a1d9d3ec804c307fa6edbbe4ddcf8e9
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2020
ms.locfileid: "84139775"
---
# <a name="xamarinforms-binding-mode"></a>Bindungsmodus in Xamarin.Forms

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/databindingdemos)

Im [vorherigen Artikel](basic-bindings.md) hatten die Seiten **Alternative Code Binding** (Alternative Codebindung) und **Alternative XAML Binding** (Alternative XAML-Bindung) ein `Label`, dessen `Scale`-Eigenschaft an die `Value`-Eigenschaft eines `Slider` gebunden war. Da der anfängliche Wert des `Slider` 0 (null) ist, wurde die `Scale`-Eigenschaft des `Label` auf 0 (null) statt auf 1 festgelegt, und das `Label` wurde nicht mehr angezeigt.

Im Beispiel [**Data Binding Demos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/databindingdemos) (Demos zur Datenbindung) ähnelt die Seite **Reverse Binding** (Umgekehrte Bindung) den Programmen im vorherigen Artikel. Der einzige Unterschied besteht darin, dass die Datenbindung im `Slider`-Objekt statt im `Label`-Objekt definiert wird:

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

Das scheint zunächst vielleicht falsch herum: Jetzt ist das `Label` die Quelle der Datenbindung und der `Slider` das Ziel. Die Bindung verweist auf die `Opacity`-Eigenschaft des `Label`, deren Standardwert 1 ist.

Sie konnten es sich vielleicht schon denken: Der `Slider` wird über den anfänglichen `Opacity`-Wert des `Label` mit einem Wert von 1 initialisiert. Dies sehen Sie auf dem iOS-Screenshot auf der linken Seite:

[![Umgekehrte Bindung](binding-mode-images/reversebinding-small.png "Umgekehrte Bindung")](binding-mode-images/reversebinding-large.png#lightbox "Umgekehrte Bindung")

Möglicherweise wären Sie aber überrascht, dass der `Slider` weiterhin funktioniert, wie Sie auf dem Android-Screenshot sehen können. Die Datenbindung scheint sogar besser zu funktionieren, wenn der `Slider` und nicht das `Label` das Bindungsziel ist, da die Initialisierung wie erwartet funktioniert.

Der Unterschied zwischen dem Beispiel für die **umgekehrte Bindung** und den vorherigen Beispielen besteht im *Bindungsmodus*.

## <a name="the-default-binding-mode"></a>Der Standardbindungsmodus

Der Bindungsmodus wird mit einem Member der [`BindingMode`](xref:Xamarin.Forms.BindingMode)-Enumeration angegeben:

- [`Default`](xref:Xamarin.Forms.BindingMode.Default)
- [`TwoWay`](xref:Xamarin.Forms.BindingMode.TwoWay): Daten fließen zwischen Quelle und Ziel in beide Richtungen
- [`OneWay`](xref:Xamarin.Forms.BindingMode.OneWay): Daten fließen von der Quelle zum Ziel
- [`OneWayToSource`](xref:Xamarin.Forms.BindingMode.OneWayToSource): Daten fließen vom Ziel zur Quelle
- [`OneTime`](xref:Xamarin.Forms.BindingMode.OneWayToSource): Daten fließen nur dann von der Quelle zum Ziel, wenn sich `BindingContext` ändert (neu in Xamarin.Forms 3.0).

Jede bindbare Eigenschaft hat einen Standardbindungsmodus, der beim Erstellen der Eigenschaft festgelegt wird und der über die [`DefaultBindingMode`](xref:Xamarin.Forms.BindableProperty.DefaultBindingMode)-Eigenschaft des `BindableProperty`-Objekts verfügbar ist. Dieser Standardbindungsmodus gibt den geltenden Modus an, wenn diese Eigenschaft das Ziel einer Datenbindung ist.

Der Standardbindungsmodus für die meisten Eigenschaften ist `OneWay`, z.B. für `Rotation`, `Scale` und `Opacity`. Wenn diese Eigenschaften Ziele von Datenbindungen sind, wird die Zieleigenschaft über die Quelle festgelegt.

Der Standardbindungsmodus für die `Value`-Eigenschaft des `Slider` ist `TwoWay`. Das bedeutet, dass das Ziel von der Quelle aus festgelegt wird (wie es auch sonst immer geschieht), aber die Quelle auch vom Ziel aus festgelegt wird, wenn die `Value`-Eigenschaft als Datenbindungsziel angegeben ist. Deshalb kann der `Slider` über den anfänglichen `Opacity`-Wert festgelegt werden.

Die bidirektionale Bindung führt scheinbar zu einer Endlosschleife. Das ist jedoch nicht der Fall. Bindbare Eigenschaften melden nur dann Eigenschaftenänderungen, wenn sich die Eigenschaft tatsächlich ändert. Dies verhindert eine Endlosschleife.

### <a name="two-way-bindings"></a>Bidirektionale Bindungen

Die meisten bindbaren Eigenschaften haben den Standardbindungsmodus `OneWay`. Die folgenden Eigenschaften haben jedoch den Bindungsmodus `TwoWay`:

- die `Date`-Eigenschaft von `DatePicker`
- die `Text`-Eigenschaft von `Editor`, `Entry`, `SearchBar` und `EntryCell`
- die `IsRefreshing`-Eigenschaft von `ListView`
- die `SelectedItem`-Eigenschaft von `MultiPage`
- die Eigenschaften `SelectedIndex` und `SelectedItem` von `Picker`
- die `Value`-Eigenschaft von `Slider` und `Stepper`
- die `IsToggled`-Eigenschaft von `Switch`
- die `On`-Eigenschaft von `SwitchCell`
- die `Time`-Eigenschaft von `TimePicker`

Diese Eigenschaften weisen aus einem guten Grund den Modus `TwoWay` auf:

Wenn Datenbindungen mit der MVVM-Anwendungsarchitektur verwendet werden, ist die ViewModel-Klasse die Quelle der Datenbindung und die View-Klasse ist das Ziel der Datenbindung. Diese Klasse besteht aus Ansichten wie `Slider`. MVVM-Bindungen entsprechen am ehesten dem Beispiel für **umgekehrte Bindungen**. Es ist sehr wahrscheinlich, dass Sie jede Ansicht auf der Seite mit dem Wert der entsprechenden Eigenschaft in der ViewModel-Klassen initialisieren möchten. Änderungen der Ansicht sollten sich jedoch auch auf die ViewModel-Eigenschaft auswirken.

Die Eigenschaften mit dem Standardbindungsmodus `TwoWay` werden am wahrscheinlichsten in MVVM-Szenarios verwendet.

### <a name="one-way-to-source-bindings"></a>Unidirektionale Bindungen in Richtung der Quelle

Schreibgeschützte bindbare Eigenschaften weisen den Standardbindungsmodus `OneWayToSource` auf. Es gibt nur eine bindbare Lese/Schreib-Eigenschaft, die den Standardbindungsmodus `OneWayToSource` aufweist:

- die `SelectedItem`-Eigenschaft von `ListView`

Dies liegt daran, dass eine Bindung der `SelectedItem`-Eigenschaft dazu führen soll, dass die Bindungsquelle festgelegt wird. Weiter unten in diesem Artikel besprechen wir ein Beispiel, das dieses Verhalten überschreibt.

### <a name="one-time-bindings"></a>Einmalige Bindungen

Einige Eigenschaften wie `IsTextPredictionEnabled` von `Entry` nutzen den Standardbindungsmodus `OneTime`.

Zieleigenschaften mit dem Bindungsmodus `OneTime` werden nur dann aktualisiert, wenn der Bindungskontext sich ändert. Für Bindungen dieser Zieleigenschaften vereinfacht das die Bindungsinfrastruktur, weil es nicht erforderlich ist, die Änderungen der Quelleigenschaften zu überwachen.

## <a name="viewmodels-and-property-change-notifications"></a>ViewModel-Klassen und Benachrichtigungen zu Änderungen von Eigenschaften

Die Seite **Simple Color Selector** (Einfache Farbauswahl) veranschaulicht die Verwendung einer einfachen ViewModel-Klasse. Durch Datenbindungen kann ein Benutzer die Farbe über drei `Slider`-Elemente für den Farbton, die Sättigung und die Helligkeit auswählen.

Die ViewModel-Klasse ist die Datenbindungsquelle. Die ViewModel-Klasse definiert *keine* bindbaren Eigenschaften. Sie implementiert aber einen Benachrichtigungsmechanismus, über den die Bindungsinfrastruktur benachrichtigt wird, wenn sich der Wert einer Eigenschaft ändert. Diese Benachrichtigungsmechanismus ist die [`INotifyPropertyChanged`](xref:System.ComponentModel.INotifyPropertyChanged)-Schnittstelle, die ein einziges Ereignis mit dem Namen [`PropertyChanged`](xref:System.ComponentModel.INotifyPropertyChanged.PropertyChanged) definiert. Eine Klasse, die diese Schnittstelle implementiert, löst das Ereignis aus, wenn sich der Wert einer ihrer öffentlichen Eigenschaften ändert. Das Ereignis muss nicht ausgelöst werden, wenn sich die Eigenschaft nicht ändert. (Die `INotifyPropertyChanged`-Schnittstelle wird auch von `BindableObject` implementiert, und ein `PropertyChanged`-Ereignis wird ausgelöst, wenn sich der Wert einer bindbaren Eigenschaft ändert.)

Die `HslColorViewModel`-Klasse definiert fünf Eigenschaften: Die Eigenschaften `Hue`, `Saturation`, `Luminosity`und `Color` sind miteinander verwandt. Wenn sich der Wert einer der drei Farbkomponenten ändert, wird die `Color`-Eigenschaft neu berechnet, und für alle vier Eigenschaften werden `PropertyChanged`-Ereignisse werden ausgelöst:

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

Wenn sich die `Color`-Eigenschaft ändert, ruft die `GetNearestColorName`-Methode in der `NamedColor`-Klasse (die auch im Beispiel **Data Binding Demos** (Demos zur Datenbindung) enthalten ist) die nächste benannte Farbe ab und legt die `Name`-Eigenschaft fest. Diese `Name`-Eigenschaft hat einen privaten `set`-Accessor, weshalb sie innerhalb der Klasse festgelegt werden muss.

Wenn eine ViewModel-Klasse als Bindungsquelle festgelegt wird, fügt die Bindungsinfrastruktur einen Handler an das `PropertyChanged`-Ereignis an. So kann die Bindung informiert werden, wenn sich die Eigenschaften ändern, um die Zieleigenschaften der geänderten Werte entsprechend anzupassen.

Wenn die Zieleigenschaft (oder die `Binding`-Definition einer Zieleigenschaft) jedoch den `BindingMode``OneTime` aufweist, muss die Bindungsinfrastruktur keinen Handler an das `PropertyChanged`-Ereignis anfügen. Die Zieleigenschaft wird nur dann aktualisiert, wenn sich der `BindingContext` ändert, und nicht, wenn sich die Quelleigenschaft ändert.

Die XAML-Datei **Simple Color Selector** (Einfache Farbauswahl) instanziiert die Klasse `HslColorViewModel` im Ressourcenverzeichnis der Seite und initialisiert die `Color`-Eigenschaft. Die `BindingContext`-Eigenschaft des `Grid`-Objekts wird auf die Bindungserweiterung `StaticResource` festgelegt, um auf diese Ressource zu verweisen:

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

Die Ansichten `BoxView`, `Label` und `Slider` erben den Bindungskontext von `Grid`. All diese Ansichten sind Bindungsziele, die auf Quelleigenschaften in ViewModel verweisen. Für die `Color`-Eigenschaft der `BoxView`-Klasse und die `Text`-Eigenschaft des `Label` sind die Datenbindungen `OneWay`: Die Eigenschaften in der Ansicht werden mithilfe der Eigenschaften in der ViewModel-Klasse festgelegt.

Die `Value`-Eigenschaft des `Slider` ist jedoch `TwoWay`. Dadurch kann jedes `Slider`-Steuerelement über die ViewModel-Klasse festgelegt werden, und die ViewModel-Klasse kann über jedes `Slider`-Steuerelement festgelegt werden.

Wenn das Programm zum ersten Mal ausgeführt wird, werden die Elemente `BoxView` und `Label` sowie die drei `Slider`-Elemente basierend auf der ursprünglichen `Color`-Eigenschaft, die bei der Instanziierung von ViewModel festgelegt wurde, über die ViewModel-Klasse festgelegt. Dies sehen Sie auf dem iOS-Screenshot auf der linken Seite:

[![Einfache Farbauswahl](binding-mode-images/simplecolorselector-small.png "Einfache Farbauswahl")](binding-mode-images/simplecolorselector-large.png#lightbox "Einfache Farbauswahl")

Wenn Sie die Schieberegler anpassen, werden `BoxView` und `Label` entsprechend aktualisiert, wie in dem Android-Screenshot veranschaulicht.

Ein gängiger Ansatz ist das Instanziieren der ViewModel-Klasse im Ressourcenverzeichnis. Zudem ist es möglich, die ViewModel-Klasse innerhalb von Eigenschaftenelementtags für die `BindingContext`-Eigenschaft zu instanziieren. Versuchen Sie in der Datei **Simple Color Selector** (Einfache Farbauswahl) `HslColorViewModel` aus dem Ressourcenverzeichnis zu entfernen, und legen Sie das Objekt wie folgt auf die `BindingContext`-Eigenschaft des `Grid`-Objekts fest:

```xaml
<Grid>
    <Grid.BindingContext>
        <local:HslColorViewModel Color="MediumTurquoise" />
    </Grid.BindingContext>

    ···

</Grid>
```

Der Bindungskontext kann auf unterschiedliche Art und Weise festgelegt werden. Manchmal instanziiert die CodeBehind-Datei die ViewModel-Klasse und legt diese auf die `BindingContext`-Eigenschaft der Seite fest. Das sind alles geeignete Vorgehensweisen.

## <a name="overriding-the-binding-mode"></a>Überschreiben des Bindungsmodus

Wenn der Standardbindungsmodus der Zieleigenschaft sich für eine bestimmte Datenbindung nicht eignet, können Sie ihn überschreiben, indem Sie die [`Mode`](xref:Xamarin.Forms.BindingBase.Mode)-Eigenschaft des `Binding`-Objekts (oder die [`Mode`](xref:Xamarin.Forms.Xaml.BindingExtension.Mode)-Eigenschaft der `Binding`-Markuperweiterung) auf einen der Member der `BindingMode`-Enumeration festlegen.

Wenn Sie die `Mode`-Eigenschaft auf `TwoWay` festlegen, kann dies zu einem unerwarteten Ergebnis führen. Versuchen Sie die XAML-Datei **Alternative XAML Binding** (Alternative XAML-Bindung) so anzupassen, dass sie `TwoWay` in der Bindungsdefinition enthält:

```xaml
<Label Text="TEXT"
       FontSize="40"
       HorizontalOptions="Center"
       VerticalOptions="CenterAndExpand"
       Scale="{Binding Source={x:Reference slider},
                       Path=Value,
                       Mode=TwoWay}" />
```

Das erwartete Verhalten (`Slider` wird mit dem anfänglichen Wert der `Scale`-Eigenschaft, 1, initialisiert) tritt nicht ein. Wenn eine `TwoWay`-Bindung initialisiert wird, wird das Ziel zunächst über die Quelle festgelegt, d.h., die `Scale`-Eigenschaft wird auf den `Slider`-Standardwert 0 (null) festgelegt. Wenn die `TwoWay`-Bindung auf dem `Slider` festgelegt wird, wird der `Slider` zunächst über die Quelle festgelegt.

Im Beispiel **Alternative XAML Binding** (Alternative XAML-Bindung) können Sie den Bindungsmodus auf `OneWayToSource` festlegen:

```xaml
<Label Text="TEXT"
       FontSize="40"
       HorizontalOptions="Center"
       VerticalOptions="CenterAndExpand"
       Scale="{Binding Source={x:Reference slider},
                       Path=Value,
                       Mode=OneWayToSource}" />
```

Jetzt wird der `Slider` mit dem Wert 1 initialisiert (der Standardwert von `Scale`). Wenn Sie den `Slider` anpassen, wirkt sich dies jedoch nicht auf die `Scale`-Eigenschaft aus. Das ist also nicht sehr nützlich.

> [!NOTE]
> Die [`VisualElement`](xref:Xamarin.Forms.VisualElement)-Klasse definiert auch die Eigenschaften [`ScaleX`](xref:Xamarin.Forms.VisualElement.ScaleX) und [`ScaleY`](xref:Xamarin.Forms.VisualElement.ScaleY), die das `VisualElement` horizontal und vertikal skalieren können.

Das Überschreiben des Standardbindungsmodus mit `TwoWay` kann nützlich sein, wenn die `SelectedItem`-Eigenschaft von `ListView` involviert ist. Der Standardbindungsmodus ist `OneWayToSource`. Wenn eine Datenbindung auf der `SelectedItem`-Eigenschaft festgelegt wird, die auf eine Quelleigenschaft in einer ViewModel-Klasse verweist, wird diese Quelleigenschaft über die `ListView`-Auswahl festgelegt. Unter einigen Umständen sollte auch das `ListView`-Objekt von der ViewModel-Klasse initialisiert werden.

Dieser Ansatz wird auf der Seite **Sample Settings** (Beispieleinstellungen) veranschaulicht. Diese Seite stellt eine einfache Implementierung der Anwendungseinstellungen dar, die häufig in einer ViewModel-Klasse definiert wird, wie z.B. in dieser `SampleSettingsViewModel`-Klasse:

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
        if (object.Equals(storage, value))
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

Jede Anwendungseinstellung ist eine Eigenschaft, die im Xamarin.Forms-Eigenschaftenwörterbuch in einer Methode namens `SaveState` gespeichert ist und im Konstruktor aus diesem Wörterbuch geladen wird. Gegen Ende der Klasse gibt es zwei Methoden, die ViewModel-Klassen optimieren und weniger fehleranfällig machen. Die `OnPropertyChanged`-Methode weist einen optionalen Parameter auf, der auf die aufrufende Eigenschaft festgelegt ist. Dadurch können Sie Tippfehler vermeiden, wenn Sie den Namen der Eigenschaft als Zeichenfolge angeben.

Die `SetProperty`-Methode in der Klasse macht noch mehr: Sie vergleicht den Wert, auf den die Eigenschaft festgelegt wird, mit dem Wert, der als Feld gespeichert wurde, und ruft `OnPropertyChanged` nur dann auf, wenn die Werte nicht übereinstimmen.

Die `SampleSettingsViewModel`-Klasse definiert zwei Eigenschaften für die Hintergrundfarbe: Die `BackgroundNamedColor`-Eigenschaft weist den Typ `NamedColor` auf. Dies ist eine Klasse, die auch im Beispiel **Data Binding Demos** (Demos zur Datenbindung) enthalten ist. Die `BackgroundColor`-Eigenschaft weist den Typ `Color` auf und kann von der `Color`-Eigenschaft des `NamedColor`-Objekts abgerufen werden.

Die Klasse `NamedColor` nutzt die .NET-Reflexion, um alle statischen öffentlichen Felder in der `Color`-Struktur von Xamarin.Forms aufzuführen und sie mitsamt ihrer Namen in einer Sammlung zu speichern, auf die über die statische Eigenschaft `All` zugegriffen werden kann:

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

Die `App`-Klasse im **Data Binding Demos**-Projekt definiert eine Eigenschaft mit dem Namen `Settings`, die den Typ `SampleSettingsViewModel` aufweist. Diese Eigenschaft wird initialisiert, wenn die `App`-Klasse instanziiert wird, und die `SaveState`-Methode wird aufgerufen, wenn die `OnSleep`-Methode aufgerufen wird:

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

Weitere Informationen zu Anwendungslebenszyklusmethoden finden Sie im Artikel zum [**App-Lebenszyklus**](~/xamarin-forms/app-fundamentals/app-lifecycle.md).

Das meiste andere wird in der Datei **SampleSettingsPage.xaml** behandelt. Der `BindingContext` der Seite wird mit einer `Binding`-Markuperweiterung festgelegt: Die Bindungsquelle ist die statische Eigenschaft `Application.Current`, die eine Instanz der `App`-Klasse im Projekt ist, und das `Path`-Objekt wird auf die `Settings`-Eigenschaft festgelegt, das `SampleSettingsViewModel`-Objekt:

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

Alle untergeordneten Elemente der Seite erben den Bindungskontext. Die meisten anderen Bindungen auf dieser Seite bestehen zu den Eigenschaften in `SampleSettingsViewModel`. Die `BackgroundColor`-Eigenschaft wird verwendet, um die `BackgroundColor`-Eigenschaft des `StackLayout`-Objekts festzulegen, und die Eigenschaften `Entry`, `DatePicker`, `Switch` und `Stepper` werden alle an andere Eigenschaften in der ViewModel-Klasse gebunden.

Für die `ItemsSource`-Eigenschaft von `ListView` wird die statische Eigenschaft `NamedColor.All` festgelegt. Dadurch wird das `ListView`-Objekt mit allen Instanzen von `NamedColor` aufgefüllt. Der Bindungskontext aller Elemente in der `ListView`-Klasse wird auf ein `NamedColor`-Objekt festgelegt. Die Elemente `BoxView` und `Label` in der `ViewCell`-Klasse werden an Eigenschaften in `NamedColor` gebunden.

Die `SelectedItem`-Eigenschaft der `ListView`-Klasse weist den Typ `NamedColor` auf und wird an die `BackgroundNamedColor`-Eigenschaft von `SampleSettingsViewModel` gebunden:

```xaml
SelectedItem="{Binding BackgroundNamedColor, Mode=TwoWay}"
```

Der Standardbindungsmodus von `SelectedItem` ist `OneWayToSource`, wodurch die ViewModel-Eigenschaft über die ausgewählten Elemente festlegt wird. Der Modus `TwoWay` lässt zu, dass das `SelectedItem`-Objekt von ViewModel aus initialisiert wird.

Wenn `SelectedItem` so festgelegt wird, scrollt `ListView` jedoch nicht automatisch an die Stelle des ausgewählten Elements. Dazu sind einige Codezeilen in der CodeBehind-Datei notwendig:

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

Der iOS-Screenshot auf der linken Seite zeigt das Programm bei der ersten Ausführung. Der Konstruktor in `SampleSettingsViewModel` initialisiert die Hintergrundfarbe in weiß, was so in `ListView` ausgewählt wurde:

[![Beispieleinstellung](binding-mode-images/samplesettings-small.png "Beispieleinstellungen")](binding-mode-images/samplesettings-large.png#lightbox "Beispieleinstellungen")

Der andere Screenshot zeigt die geänderten Einstellungen. Denken Sie daran, das Programm auf dem ausgeführten Gerät oder Emulator in den Energiesparmodus zu versetzen oder es zu beenden, wenn Sie mit dieser Seite experimentieren. Wenn Sie das Programm über den Visual Studio-Debugger beenden, wird die `OnSleep`-Überschreibung in der `App`-Klasse nicht aufgerufen.

Im nächsten Artikel erfahren Sie, wie Sie die [**Zeichenfolgenformatierung**](string-formatting.md) von Datenbindungen festlegen, die auf der `Text`-Eigenschaft des `Label` festgelegt sind.

## <a name="related-links"></a>Verwandte Links

- [Data Binding Demos (Demos zur Datenbindung (Beispiel))](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/databindingdemos)
- [Zusammenfassung von Kapitel 16.: Datenbindung](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
