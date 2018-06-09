---
title: Xamarin.Forms-Trigger
description: In diesem Artikel erläutert die Xamarin.Forms-Trigger verwenden, um auf Benutzer Schnittstelle ändert sich mit XAML zu reagieren. Triggern ermöglichen es Ihnen, Aktionen deklarativ in XAML auszudrücken, die die Darstellung von Steuerelementen, die basierend auf Ereignisse oder eigenschaftenänderungen zu ändern.
ms.prod: xamarin
ms.assetid: 60460F57-63C6-4916-BBB5-A870F1DF53D7
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/01/2016
ms.openlocfilehash: b28ebb8845b7eae0d818e1279b4d6eaef4ad5b8b
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/08/2018
ms.locfileid: "35241434"
---
# <a name="xamarinforms-triggers"></a>Xamarin.Forms-Trigger

Triggern ermöglichen es Ihnen, Aktionen deklarativ in XAML auszudrücken, die die Darstellung von Steuerelementen, die basierend auf Ereignisse oder eigenschaftenänderungen zu ändern.

Sie können zuweisen ein Triggers direkt an ein Steuerelement oder Hinzufügen einer Seite oder app-Ebene Ressourcenverzeichnis auf mehrere Steuerelemente angewendet werden soll.

Es gibt vier Arten von Trigger:

* [Eigenschaftsauslöser](#property) -tritt auf, wenn eine Eigenschaft für ein Steuerelement auf einen bestimmten Wert festgelegt ist.

* [DDL-Trigger Data](#data) - Daten verwendet, die Trigger, die auf Grundlage der Eigenschaften eines anderen Steuerelements binden.

* [Ereignisauslöser](#event) -tritt auf, wenn auf das Steuerelement ein Ereignis auftritt.

* [Mehrere Trigger](#multi) -können mehrere Trigger-Bedingungen festgelegt werden, bevor eine Aktion ausgeführt wird.

<a name="property" />

## <a name="property-triggers"></a>Eigenschaftstrigger

Ein einfache Trigger kann ausschließlich in XAML ausgedrückt werden Hinzufügen einer `Trigger` Element an ein Steuerelement Collection auslöst.
Dieses Beispiel zeigt einen Trigger, die ändert ein `Entry` Hintergrundfarbe, wenn es den Fokus erhält:

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

Die wichtigen Teile der Trigger Deklaration sind:

* **TargetType** -den Steuerelementtyp ", die der Trigger gilt.

* **Eigenschaft** -die Eigenschaft des Steuerelements, das überwacht wird.

* **Wert** -der Wert auftretenden für die überwachten Eigenschaft, wodurch der Aktivierung des Triggers.

* **Setter** -eine Auflistung von `Setter` -Elemente hinzugefügt werden können, und wenn die auslöserbedingung erfüllt ist. Sie müssen angeben, die `Property` und `Value` festlegen.

* **Eigenschaftsauslösern und "ExitActions"** (nicht dargestellt) – in Code geschrieben sind, und kann verwendet werden, zusätzlich zu (oder instead of) `Setter` Elemente. Sie sind [unten beschriebenen](#enterexit).

### <a name="applying-a-trigger-using-a-style"></a>Anwenden eines Triggers mithilfe eines Stils

Trigger können auch hinzugefügt werden, um eine `Style` Deklaration auf ein Steuerelement in einer Seite oder einer Anwendung `ResourceDictionary`. Dieses Beispiel deklariert einen impliziten Stil (d. h. keine `Key` festgelegt ist) Dies bedeutet, er gilt für alle `Entry` Steuerelemente auf der Seite.

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

Datentrigger verwenden die Datenbindung dazu führen, dass ein anderes Steuerelement überwachen die `Setter`s aufgerufen. Statt die `Property` -Attribut in einem Eigenschaftentrigger, legen die `Binding` Attribut für den angegebenen Wert zu überwachen.

Im folgenden Beispiel wird die Syntax zum Binden von Daten `{Binding Source={x:Reference entry}, Path=Text.Length}` also wie verweisen wir auf Eigenschaften des Steuerelements. Wenn die Länge des der `entry` 0 (null), wird der Trigger aktiviert ist. In diesem Beispiel wird der Trigger die Schaltfläche deaktiviert, wenn die Eingabe leer ist.

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

Tipp: bei der Auswertung `Path=Text.Length` geben immer einen Standardwert für die Zieleigenschaft (z. b. `Text=""`), da er andernfalls kann `null` und des Triggers funktioniert nicht wie erwartet.

Zusätzlich zur Angabe `Setter`s können Sie auch bereitstellen [ `EnterActions` und `ExitActions` ](#enterexit).

<a name="event" />

## <a name="event-triggers"></a>Ereignistrigger

Die `EventTrigger` -Element ist nur ein `Event` -Eigenschaft, z. B. `"Clicked"` im folgenden Beispiel.

```xaml
<EventTrigger Event="Clicked">
    <local:NumericValidationTriggerAction />
</EventTrigger>
```

Beachten Sie, dass es keine `Setter` Elemente jedoch eher einen Verweis auf eine Klasse, definiert durch `local:NumericValidationTriggerAction` erfordert die `xmlns:local` deklariert werden, auf der Seite des XAML:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:WorkingWithTriggers;assembly=WorkingWithTriggers"
```

Implementiert die Klasse selbst `TriggerAction` Dies bedeutet, dass sollten Geben Sie eine Außerkraftsetzung für die `Invoke` Methode, die Eintreten des Ereignisses Trigger aufgerufen wird.

Eine Aktion triggerimplementierung sollten:

* Implementieren Sie die generische `TriggerAction<T>` -Klasse, mit dem generischen Parameter entspricht, mit dem Typ des Steuerelements auf der Trigger angewendet werden. Sie können z. B. Klassen verwenden `VisualElement` Triggeraktionen schreiben, die funktionieren mit einer Vielzahl von Steuerelementen, oder geben Sie ein Steuerelementtyp wie `Entry`.

* Überschreiben Sie die `Invoke` - Dies wird aufgerufen, wenn die Auslöserkriterien erfüllt sind.

* Optional verfügbar machen Eigenschaften, die in der XAML-Code festgelegt werden können, wenn der Trigger deklariert wird (z. B. `Anchor`, `Scale`, und `Length` in diesem Beispiel).

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

Die Eigenschaften, die von der Triggeraktion verfügbar gemacht werden können wie folgt in der XAML-Deklaration festgelegt werden:

```xaml
<EventTrigger Event="TextChanged">
    <local:NumericValidationTriggerAction />
</EventTrigger>
```

Seien Sie vorsichtig beim Freigeben von Triggern in einer `ResourceDictionary`, eine Instanz wird zwischen Steuerelementen genutzt werden, damit einem beliebigen Zustand, der einmal konfiguriert ist für alle gelten.

Beachten Sie, dass Ereignistriggern nicht unterstützen `EnterActions` und `ExitActions` [unten beschriebenen](#enterexit).    

<a name="multi" />

## <a name="multi-triggers"></a>Multi-Trigger

Ein `MultiTrigger` sieht wie ein `Trigger` oder `DataTrigger` außer kann mehr als eine Bedingung vorhanden sein. Alle Bedingungen muss "true", bevor die `Setter`s ausgelöst werden.

Hier ist ein Beispiel eines Triggers für eine Schaltfläche, die an zwei verschiedene Eingaben binden (`email` und `phone`):

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

Die `Conditions` Auflistung kann auch enthalten `PropertyCondition` Elemente wie folgt:

```xaml
<PropertyCondition Property="Text" Value="OK" />
```

### <a name="building-a-require-all-multi-trigger"></a>Erstellen eines Multi-Triggers "müssen Sie für alle"

Der Multi-Trigger aktualisiert das Steuerelement nur, wenn alle Voraussetzungen erfüllt sind. Tests für "alle Feldlängen 0 (null sind)" (z. B. eine Anmeldeseite, in dem alle Eingaben müssen abgeschlossen sein) ist schwierig, da eine Bedingung verwendet werden soll ", in denen Text.Length > 0", aber dies kann nicht in XAML ausgedrückt werden.

Dies erreichen Sie mit einer `IValueConverter`. Den Konvertercode unten Transformationen der `Text.Length` binden in eine `bool` , der angibt, ob ein Feld leer ist:


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

Für die Verwendung dieser Konverter in einem Multi-Trigger zuerst hinzufügen auf der Seite Ressourcenverzeichnis (zusammen mit einer benutzerdefinierten `xmlns:local` Namespacedefinition):

```xaml
<ResourceDictionary>
   <local:MultiTriggerConverter x:Key="dataHasBeenEntered" />
</ResourceDictionary>
```

Der XAML-Code wird unten gezeigt. Beachten Sie die folgenden Unterschiede gegenüber im ersten Beispiel der Multi-Trigger aus:

* Die Schaltfläche "" `IsEnabled="false"` standardmäßig festgelegt.
* Die Multi-Auslösebedingungen verwenden Sie den Konverter zum Aktivieren der `Text.Length` Wert in einen booleschen Wert.
* Wenn alle Bedingungen sind `true`, Setter-Methode wird der Schaltfläche `IsEnabled` Eigenschaft `true`.

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

Diese Screenshots zeigen den Unterschied zwischen den zwei mehrfach Trigger Beispielen oben. Im oberen Teil der Bildschirme Eingabetext in nur einem `Entry` reicht zum Aktivieren der **speichern** Schaltfläche.
Im unteren Teil der Bildschirme die **Anmeldung** Schaltfläche bleibt inaktiv, bis beide Felder Daten enthalten.


![](triggers-images/multi-requireall.png "MultiTrigger Beispiele")

<a name="enterexit" />

## <a name="enteractions-and-exitactions"></a>Eigenschaftsauslösern und "ExitActions"

Eine andere Möglichkeit, Änderungen zu implementieren, tritt ein Trigger wird durch Hinzufügen von `EnterActions` und `ExitActions` Sammlungen und Angeben von `TriggerAction<T>` Implementierungen.

Sie können angeben, *beide* `EnterActions` und `ExitActions` sowie `Setter`n in einem Trigger, sich aber bewusst sein, die `Setter`s werden sofort aufgerufen, (sie warten nicht die `EnterAction` oder `ExitAction` auf Führen Sie). Alternativ können Sie alles, was im Code ausführen und keine `Setter`s überhaupt.

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

Wie immer, wenn eine Klasse in XAML verwiesen wird sollten Sie z. B. einen Namespace deklarieren `xmlns:local` wie hier gezeigt:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:WorkingWithTriggers;assembly=WorkingWithTriggers"
```

Die `FadeTriggerAction` Code wird unten gezeigt:

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

Hinweis: `EnterActions` und `ExitActions` werden ignoriert, auf **Ereignistriggern**.



## <a name="related-links"></a>Verwandte Links

- [Trigger-Beispiel](https://developer.xamarin.com/samples/WorkingWithTriggers)
- [Xamarin.Forms-API-Dokumentation](https://developer.xamarin.com/api/type/Xamarin.Forms.TriggerAction%3CT%3E/)
