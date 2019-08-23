---
title: Xamarin.Forms-Trigger
description: In diesem Artikel wird erläutert, wie Xamarin.Forms-Trigger verwendet werden, um auf Änderungen der Benutzeroberfläche mit XAML zu reagieren. Mit Triggern können Sie Aktionen deklarativ in XAML ausdrücken, die die Darstellung von Steuerelementen basierend auf Ereignissen oder Eigenschaftenänderungen ändern.
ms.prod: xamarin
ms.assetid: 60460F57-63C6-4916-BBB5-A870F1DF53D7
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/01/2016
ms.openlocfilehash: e21ae2c335a1ffe410317ef8870ee074a3a5ebe2
ms.sourcegitcommit: 3434624a36a369986b6aeed7959dae60f7112a14
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/20/2019
ms.locfileid: "69629629"
---
# <a name="xamarinforms-triggers"></a>Xamarin.Forms-Trigger

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithtriggers)

Mit Triggern können Sie Aktionen deklarativ in XAML ausdrücken, die die Darstellung von Steuerelementen basierend auf Ereignissen oder Eigenschaftenänderungen ändern.

Sie können einen Trigger direkt zu einem Steuerelement zuweisen oder zu einem Ressourcenverzeichnis auf Seitenebene oder Anwendungsebene hinzufügen, das auf mehrere Steuerelemente angewendet wird.

Es gibt vier Typen von Triggern:

* [Eigenschaftstrigger:](#property) tritt auf, wenn eine Eigenschaft eines Steuerelements auf einen bestimmten Wert festgelegt wird.

* [Datentrigger:](#data) nutzt Datenbindung, um auf Grundlage der Eigenschaften eines anderen Steuerelements auszulösen.

* [Ereignistrigger:](#event) tritt auf, wenn ein Ereignis auf dem Steuerelement auftritt.

* [Multitrigger:](#multi) ermöglicht das Festlegen mehrerer Triggerbedingungen, unter denen eine Aktion auftritt.

<a name="property" />

## <a name="property-triggers"></a>Eigenschaftstrigger

Ein einfacher Trigger kann ausschließlich in XAML ausgedrückt werden, indem ein `Trigger`-Element in die Triggersammlung eines Steuerelements hinzugefügt wird.
In diesem Beispiel wird ein Trigger veranschaulicht, der eine `Entry`-Hintergrundfarbe ändert, wenn sie den Fokus erhält:

```xaml
<Entry Placeholder="enter name">
    <Entry.Triggers>
        <Trigger TargetType="Entry"
             Property="IsFocused" Value="True">
            <Setter Property="BackgroundColor" Value="Yellow" />
        </Trigger>
    </Entry.Triggers>
</Entry>
```

Die folgenden Bestandteile der Deklaration des Triggers sind wichtig:

* **TargetType:** der Steuerelementtyp, für den der Trigger gilt.

* **Property:** die Eigenschaft des Steuerelements, das überwacht wird.

* **Value:** der Wert, der die Aktivierung des Triggers auslöst, wenn er bei der überwachten Eigenschaft auftritt.

* **Setter:** eine Sammlung von `Setter`-Elementen kann hinzugefügt werden, wenn die Bedingung des Triggers erfüllt wird. Sie müssen `Property` und `Value` angeben, um diese Eigenschaft festzulegen.

* **EnterActions und ExitActions:** werden im Code geschrieben und können zusätzlich zu (oder anstelle von) `Setter`-Elementen verwendet werden (nicht gezeigt). Sie werden [unten beschrieben](#enterexit).

### <a name="applying-a-trigger-using-a-style"></a>Anwenden eines Triggers mit einer Formatvorlage

Trigger können auch zu einer `Style`-Deklaration in einem Steuerelement, auf einer Seite oder in ein `ResourceDictionary` (Ressourcenverzeichnis) einer Anwendung hinzugefügt werden. In diesem Beispiel wird eine implizite Formatvorlage deklariert (d.h. `Key` ist nicht festgelegt), weshalb sie für alle `Entry`-Steuerelemente auf der Seite gilt.

```xaml
<ContentPage.Resources>
    <ResourceDictionary>
        <Style TargetType="Entry">
                        <Style.Triggers>
                <Trigger TargetType="Entry"
                         Property="IsFocused" Value="True">
                    <Setter Property="BackgroundColor" Value="Yellow" />
                </Trigger>
            </Style.Triggers>
        </Style>
    </ResourceDictionary>
</ContentPage.Resources>
```

<a name="data" />

## <a name="data-triggers"></a>Datentrigger

Datentrigger nutzen die Datenbindung, um ein andere Steuerelement zu überwachen, damit die `Setter`-Elemente aufgerufen werden können. Legen Sie anstelle des `Property`-Attributs in einem Eigenschaftentrigger das Attribut `Binding` fest, um den angegebenen Wert zu überwachen.

Im folgenden Beispiel wird die Datenbindungssyntax `{Binding Source={x:Reference entry}, Path=Text.Length}` verwendet,
um auf die Eigenschaften eines anderen Steuerelements zu verweisen. Wenn die Länge von `entry` 0 (null) ist, wird der Trigger aktiviert. In diesem Beispiel wird die Schaltfläche durch den Trigger deaktiviert, wenn die Eingabe leer ist.

```xaml
<!-- the x:Name is referenced below in DataTrigger-->
<!-- tip: make sure to set the Text="" (or some other default) -->
<Entry x:Name="entry"
       Text=""
       Placeholder="required field" />

<Button x:Name="button" Text="Save"
        FontSize="Large"
        HorizontalOptions="Center">
    <Button.Triggers>
        <DataTrigger TargetType="Button"
                     Binding="{Binding Source={x:Reference entry},
                                       Path=Text.Length}"
                     Value="0">
            <Setter Property="IsEnabled" Value="False" />
        </DataTrigger>
    </Button.Triggers>
</Button>
```

> [!TIP]
> Geben Sie beim Auswerten von `Path=Text.Length` immer einen Standardwert für die Zieleigenschaft an (z.B. `Text=""`), da der Wert sonst `null` wäre und der Trigger nicht erwartungsgemäß funktionieren würde.

Zusätzlich zur Angabe von `Setter`-Eigenschaften können Sie auch [`EnterActions` und `ExitActions`](#enterexit) bereitstellen.

<a name="event" />

## <a name="event-triggers"></a>Ereignistrigger

Für das `EventTrigger`-Element ist wie `"Clicked"` im folgenden Beispiel lediglich eine `Event`-Eigenschaft erforderlich.

```xaml
<EventTrigger Event="Clicked">
    <local:NumericValidationTriggerAction />
</EventTrigger>
```

Beachten Sie, dass keine `Setter`-Elemente vorhanden sind. Stattdessen ist ein Verweis auf eine mit `local:NumericValidationTriggerAction` definierte Klasse enthalten, für die `xmlns:local` im XAML-Code der Seite deklariert werden muss:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:WorkingWithTriggers;assembly=WorkingWithTriggers"
```

Die Klasse selbst implementiert `TriggerAction`, d.h. sie sollte die `Invoke`-Methode überschreiben, die aufgerufen wird, wenn das Triggerereignis auftritt.

Eine Implementierung einer Triggeraktion sollte folgende Aktionen durchführen:

* Die generische `TriggerAction<T>`-Klasse mit dem generischen Parameter implementieren, der dem Typ des Steuerelements entspricht, auf das der Trigger angewendet wird. Sie können übergeordnete Klassen wie `VisualElement` verwenden, um Triggeraktionen zu schreiben, die für eine Vielzahl von Steuerelementen funktionieren, oder ein Steuerelementtyp wie `Entry` anzugeben.

* Die `Invoke`-Methode überschreiben. Diese wird immer dann aufgerufen, wenn die Triggerbedingungen erfüllt werden.

* Eigenschaften optional zur Verfügung stellen, die im XAML-Code festgelegt werden können, wenn der Trigger deklariert wird (z.B. `Anchor`, `Scale` und `Length` im folgenden Beispiel).

```csharp
public class NumericValidationTriggerAction : TriggerAction<Entry>
{
    protected override void Invoke (Entry entry)
    {
        double result;
        bool isValid = Double.TryParse (entry.Text, out result);
        entry.TextColor = isValid ? Color.Default : Color.Red;
    }
}
```

Die von der Triggeraktion zur Verfügung gestellten Eigenschaften können wie folgt in der XAML-Deklaration festgelegt werden:

```xaml
<EventTrigger Event="TextChanged">
    <local:NumericValidationTriggerAction />
</EventTrigger>
```

Passen Sie beim Freigeben von Triggern in einem `ResourceDictionary` auf. Eine Instanz wird unter mehreren Steuerelementen geteilt, also wird jeder Zustand, der einmal konfiguriert wird, auf alle Steuerelemente angewendet.

Beachten Sie, dass Ereignistrigger `EnterActions` und `ExitActions` nicht unterstützen ([siehe unten](#enterexit)).

<a name="multi" />

## <a name="multi-triggers"></a>Multitrigger

Ein `MultiTrigger` sieht `Trigger` oder `DataTrigger` ähnlich, mit der Ausnahme, dass sie mehrere Bedingungen aufweisen können. Alle Bedingungen müssen erfüllt werden, bevor `Setter`-Elemente ausgelöst werden.

Hier finden Sie ein Beispiel für einen Trigger für eine Schaltfläche, die zwei verschiedene Eingaben (`email` und `phone`) bindet:

```xaml
<MultiTrigger TargetType="Button">
    <MultiTrigger.Conditions>
        <BindingCondition Binding="{Binding Source={x:Reference email},
                                   Path=Text.Length}"
                               Value="0" />
        <BindingCondition Binding="{Binding Source={x:Reference phone},
                                   Path=Text.Length}"
                               Value="0" />
    </MultiTrigger.Conditions>

  <Setter Property="IsEnabled" Value="False" />
    <!-- multiple Setter elements are allowed -->
</MultiTrigger>
```

Die `Conditions`-Sammlung kann wie folgt auch `PropertyCondition`-Elemente enthalten:

```xaml
<PropertyCondition Property="Text" Value="OK" />
```

### <a name="building-a-require-all-multi-trigger"></a>Erstellen eines Multitriggers, der alle Bedingungen erfordert

Der Multitrigger aktualisiert sein Steuerelement nur, wenn alle Bedingungen erfüllt werden. Das Testen, ob „alle Feldlängen 0 (null) sind“ (z.B. eine Anmeldeseite, bei der alle Eingaben vollständig sein müssen), ist schwierig, da Sie die Bedingung „where Text.Length > 0“ (Textlänge größer als 0 (null)) benötigen, die in XAML nicht ausgedrückt werden kann.

Hierzu können Sie die Schnittstelle `IValueConverter` verwenden. Mit dem folgenden Konvertercode wird die `Text.Length`-Bindung in einen `bool`-Wert transformiert, der angibt, ob ein Feld leer ist oder nicht:

```csharp
public class MultiTriggerConverter : IValueConverter
{
    public object Convert(object value, Type targetType,
        object parameter, CultureInfo culture)
    {
        if ((int)value > 0) // length > 0 ?
            return true;            // some data has been entered
        else
            return false;            // input is empty
    }

    public object ConvertBack(object value, Type targetType,
        object parameter, CultureInfo culture)
    {
        throw new NotSupportedException ();
    }
}
```

Um diesen Konverter in einem Multitrigger zu verwenden, fügen Sie ihn zunächst zum Ressourcenverzeichnis der Seite hinzu (zusammen mit einer benutzerdefinierten `xmlns:local`-Namespacedefinition):

```xaml
<ResourceDictionary>
   <local:MultiTriggerConverter x:Key="dataHasBeenEntered" />
</ResourceDictionary>
```

Der XAML-Code wird unten veranschaulicht. Beachten Sie die folgenden Unterschiede gegenüber dem ersten Beispiel für einen Multitrigger:

* Für die Schaltfläche ist standardmäßig `IsEnabled="false"` festgelegt.
* Die Bedingungen des Multitriggers verwenden den Konverter, um den Wert `Text.Length` in einen `boolean`-Wert zu konvertieren.
* Wenn alle Bedingungen den Wert `true` aufweisen, das Setter-Element legt `true` für die `IsEnabled`-Eigenschaft der Schaltfläche fest.

```xaml
<Entry x:Name="user" Text="" Placeholder="user name" />

<Entry x:Name="pwd" Text="" Placeholder="password" />

<Button x:Name="loginButton" Text="Login"
        FontSize="Large"
        HorizontalOptions="Center"
        IsEnabled="false">
  <Button.Triggers>
    <MultiTrigger TargetType="Button">
      <MultiTrigger.Conditions>
        <BindingCondition Binding="{Binding Source={x:Reference user},
                              Path=Text.Length,
                              Converter={StaticResource dataHasBeenEntered}}"
                          Value="true" />
        <BindingCondition Binding="{Binding Source={x:Reference pwd},
                              Path=Text.Length,
                              Converter={StaticResource dataHasBeenEntered}}"
                          Value="true" />
      </MultiTrigger.Conditions>
      <Setter Property="IsEnabled" Value="True" />
    </MultiTrigger>
  </Button.Triggers>
</Button>
```

Im folgenden Screenshot wird der Unterschied zwischen den zwei oben gezeigten Beispielen für Multitrigger dargestellt. Im oberen Teil der Bildschirme genügt die Texteingabe in einem `Entry`-Element, um die Schaltfläche **Save** (Speichern) zu aktivieren.
Im unteren Teil der Bildschirme bleibt die Schaltfläche **Login** (Anmelden) inaktiv, bis beide Felder Daten enthalten.

![](triggers-images/multi-requireall.png "Beispiele für Multitrigger")

<a name="enterexit" />

## <a name="enteractions-and-exitactions"></a>EnterActions und ExitActions

Das Hinzufügen der `EnterActions`- und `ExitActions`-Sammlungen und das Festlegen von `TriggerAction<T>`-Implementierungen stellt eine weitere Möglichkeit zum Implementieren von Änderungen, wenn ein Trigger ausgelöst wird.

Sie können die *beiden* Eigenschaften `EnterActions` und `ExitActions` sowie `Setter` in einem Trigger angeben. Achten Sie jedoch darauf, dass `Setter` sofort aufgerufen werden (sie warten nicht auf den Abschluss von `EnterAction` oder `ExitAction`). Alternativ können Sie alles im Code ausführen, anstatt `Setter` überhaupt zu verwenden.

```xaml
<Entry Placeholder="enter job title">
    <Entry.Triggers>
        <Trigger TargetType="Entry"
                 Property="Entry.IsFocused" Value="True">
            <Trigger.EnterActions>
                <local:FadeTriggerAction StartsFrom="0"" />
            </Trigger.EnterActions>

            <Trigger.ExitActions>
                <local:FadeTriggerAction StartsFrom="1" />
            </Trigger.ExitActions>
                        <!-- You can use both Enter/Exit and Setter together if required -->
        </Trigger>
    </Entry.Triggers>
</Entry>
```

Sie sollten wie im folgenden Beispiel gezeigt immer einen Namespace wie `xmlns:local` deklarieren, wenn in XAML auf eine Klasse verwiesen wird:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:WorkingWithTriggers;assembly=WorkingWithTriggers"
```

Im Folgenden wird der `FadeTriggerAction`-Code dargestellt:

```csharp
public class FadeTriggerAction : TriggerAction<VisualElement>
{
    public FadeTriggerAction() {}

    public int StartsFrom { set; get; }

    protected override void Invoke (VisualElement visual)
    {
            visual.Animate("", new Animation( (d)=>{
                var val = StartsFrom==1 ? d : 1-d;
                visual.BackgroundColor = Color.FromRgb(1, val, 1);

            }),
            length:1000, // milliseconds
            easing: Easing.Linear);
    }
}
```

Hinweis: `EnterActions` und `ExitActions` werden bei **Ereignistriggern** ignoriert.



## <a name="related-links"></a>Verwandte Links

- [Triggers Sample (Triggerbeispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithtriggers)
- [Xamarin.Forms-API-Dokumentation](xref:Xamarin.Forms.TriggerAction`1)
