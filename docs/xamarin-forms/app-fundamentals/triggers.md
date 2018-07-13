---
title: Xamarin.Forms-Trigger
description: In diesem Artikel wird erläutert, wie Xamarin.Forms-Trigger verwenden, um auf Änderungen an der Benutzeroberfläche mit XAML zu reagieren. Trigger können Sie Aktionen deklarativ in XAML auszudrücken, die die Darstellung von Steuerelementen basierend auf Ereignisse oder eigenschaftenänderungen zu ändern.
ms.prod: xamarin
ms.assetid: 60460F57-63C6-4916-BBB5-A870F1DF53D7
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/01/2016
ms.openlocfilehash: 954a0967e034e0321964e12ca0725ae2a85e3bc6
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995536"
---
# <a name="xamarinforms-triggers"></a>Xamarin.Forms-Trigger

Trigger können Sie Aktionen deklarativ in XAML auszudrücken, die die Darstellung von Steuerelementen basierend auf Ereignisse oder eigenschaftenänderungen zu ändern.

Sie können ein Triggers direkt an ein Steuerelement zuweisen oder Hinzufügen einer Ressource auf Seitenebene oder app-Ebene-Wörterbuch, das auf mehrere Steuerelemente angewendet werden.

Es gibt vier Arten von Triggern:

* [Eigenschaftstrigger](#property) -tritt auf, wenn eine Eigenschaft eines Steuerelements zu einem bestimmten Wert festgelegt ist.

* [Trigger Data](#data) – verwendet Datenbindung, Trigger, die auf Grundlage der Eigenschaften eines anderen Steuerelements.

* [Ereignistrigger](#event) -tritt auf, wenn ein Ereignis für das Steuerelement.

* [Mehrere Trigger](#multi) -können mehrere triggerbedingungen festgelegt werden, bevor eine Aktion stattfindet.

<a name="property" />

## <a name="property-triggers"></a>Eigenschaftstrigger

Ein einfacher Trigger ausgedrückt werden kann, ausschließlich in XAML, Hinzufügen einer `Trigger` Element eines Steuerelements Collection auslöst.
Dieses Beispiel zeigt einen Trigger, der sich ändert ein `Entry` Hintergrundfarbe, wenn es den Fokus erhält:

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

Die wichtigen Teile des Triggers Deklaration sind:

* **TargetType** : der Steuerelementtyp, der der Trigger angewendet wird.

* **Eigenschaft** -die Eigenschaft für das Steuerelement, das überwacht wird.

* **Wert** -der Wert tritt für die überwachten Eigenschaft, die Aktivierung des Triggers verursacht.

* **Setter** -eine Auflistung von `Setter` -Elemente hinzugefügt werden können, und wenn die auslöserbedingung erfüllt ist. Sie müssen angeben, die `Property` und `Value` festlegen.

* **Eigenschaftsauslösern und "ExitActions"** (nicht dargestellt): in Code geschrieben sind und kann verwendet werden, zusätzlich zu (oder instead of) `Setter` Elemente. Sie sind [unten beschriebenen](#enterexit).

### <a name="applying-a-trigger-using-a-style"></a>Anwenden eines Triggers, der mit einem Stil

Trigger können auch hinzugefügt werden, um eine `Style` Deklaration für ein Steuerelement in einer Seite oder einer Anwendung `ResourceDictionary`. Das folgende Beispiel deklariert einen impliziten Stil (d. h. keine `Key` festgelegt ist) was bedeutet, dass gelten für alle `Entry` Steuerelemente auf der Seite.

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

Datentrigger verwenden die Datenbindung zum Überwachen von einem anderen Steuerelement, das dazu führen, dass die `Setter`s aufgerufen. Statt die `Property` -Attribut in einem Eigenschaftentrigger, legen die `Binding` Attribut für den angegebenen Wert zu überwachen.

Im folgenden Beispiel wird die Datenbindungssyntax `{Binding Source={x:Reference entry}, Path=Text.Length}` wie verweisen wir auf Eigenschaften des Steuerelements ist. Wenn die Länge des der `entry` 0 (null), wird der Trigger wird aktiviert. In diesem Beispiel wird der Trigger die Schaltfläche deaktiviert, wenn die Eingabe leer ist.

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

Tipp: bei der Auswertung von `Path=Text.Length` geben immer einen Standardwert für die Zieleigenschaft (z. b. `Text=""`), da er andernfalls kann `null` und der Trigger funktioniert nicht wie erwartet.

Zusätzlich zur Angabe `Setter`s, die Sie auch bieten [ `EnterActions` und `ExitActions` ](#enterexit).

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

Implementiert die Klasse selbst `TriggerAction` was bedeutet, dass sollten Geben Sie eine Außerkraftsetzung für die `Invoke` Methode, die aufgerufen wird, wenn dieses Ereignis eintritt.

Eine Aktion triggerimplementierung sollten Folgendes beachten:

* Implementiert die generische `TriggerAction<T>` -Klasse mit den generischen Parameter entspricht, mit dem Typ des Steuerelements auf der Trigger angewendet werden. Sie können eine übergeordnete Klasse wie z. B. `VisualElement` Triggeraktionen zu schreiben, die funktionieren mit einer Vielzahl von Steuerelementen, oder geben Sie ein Steuerelementtyp wie `Entry`.

* Überschreiben der `Invoke` -Methode: wird dieses aufgerufen, wenn die auslösekriterien erfüllt sind.

* Optional verfügbar machen Eigenschaften, die in der XAML festgelegt werden können, wenn der Trigger deklariert wird (z. B. `Anchor`, `Scale`, und `Length` in diesem Beispiel).

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

Seien Sie vorsichtig bei der Freigabe von Triggern in einer `ResourceDictionary`, eine Instanz wird zwischen Steuerelementen genutzt werden, wendet auf alle an einem beliebigen Zustand, der einmal konfiguriert ist.

Beachten Sie, dass nicht Ereignistrigger unterstützen `EnterActions` und `ExitActions` [unten beschriebenen](#enterexit).    

<a name="multi" />

## <a name="multi-triggers"></a>Mehrere Trigger

Ein `MultiTrigger` sieht in etwa wie eine `Trigger` oder `DataTrigger` außer können mehr als eine Bedingung vorhanden sein. Alle Bedingungen erfüllt, bevor Sie sein, die `Setter`s ausgelöst werden.

Es folgt ein Beispiel eines Triggers für eine Schaltfläche, die an zwei verschiedene Eingaben binden (`email` und `phone`):

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

Die `Conditions` Auflistung kann auch enthalten `PropertyCondition` Elemente wie folgt aus:

```xaml
<PropertyCondition Property="Text" Value="OK" />
```

### <a name="building-a-require-all-multi-trigger"></a>Erstellen einen Multi-Trigger "alle erforderlich"

Der Multi-Trigger aktualisiert das Steuerelement nur, wenn alle Bedingungen erfüllt sind. Tests für "alle Feldlängen 0 (null sind)" (z. B. eine Seite für die Anmeldung, in dem alle Eingaben müssen abgeschlossen sein) ist schwierig, da Sie möchten, dass eine Bedingung ", in denen Text.Length > 0", aber dies kann nicht in XAML ausgedrückt werden.

Dies erreichen Sie mit einem `IValueConverter`. Die folgenden Transformationen Konvertercode der `Text.Length` Bindung in eine `bool` , der angibt, ob ein Feld leer oder nicht ist:


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

Für die Verwendung dieser Konverter in einem Trigger mit mehreren zuerst hinzufügen Ressourcenverzeichnis der Seite (zusammen mit einem benutzerdefinierten `xmlns:local` Namespacedefinition):

```xaml
<ResourceDictionary>
   <local:MultiTriggerConverter x:Key="dataHasBeenEntered" />
</ResourceDictionary>
```

Die XAML ist unten dargestellt. Beachten Sie die folgenden Unterschiede gegenüber den im ersten Beispiel mit mehreren-Trigger wird:

* Die Schaltfläche `IsEnabled="false"` standardmäßig festgelegt.
* Die Multi-Auslösebedingungen verwenden Sie den Konverter zum Aktivieren der `Text.Length` Wert in einen booleschen Wert.
* Wenn alle Bedingungen sind `true`, der Setter ist der Schaltfläche " `IsEnabled` Eigenschaft `true`.

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

Diese Screenshots ist den Unterschied zwischen den beiden mehrere Trigger Beispielen oben dargestellt. Im oberen Teil der Bildschirme, geben Sie Text in nur einem `Entry` reicht aus, aktivieren die **speichern** Schaltfläche.
Im unteren Bereich der Bildschirme die **Anmeldung** Schaltfläche inaktiv bleibt, bis beide Felder Daten enthalten.


![](triggers-images/multi-requireall.png "MultiTrigger-Beispiele")

<a name="enterexit" />

## <a name="enteractions-and-exitactions"></a>Eigenschaftsauslösern und "ExitActions"

Eine andere Möglichkeit, Änderungen zu implementieren, wenn ein Trigger wird aktiviert, wird durch Hinzufügen von `EnterActions` und `ExitActions` Sammlungen und Angeben von `TriggerAction<T>` Implementierungen.

Sie können angeben, *sowohl* `EnterActions` und `ExitActions` sowie `Setter`s in einem Trigger, aber denken Sie daran, die die `Setter`s werden sofort aufgerufen (sie nicht warten die `EnterAction` oder `ExitAction` auf Führen Sie). Alternativ können Sie alles, was in den Code ausführen und verwenden Sie nicht `Setter`s überhaupt.

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

Wie immer, wenn eine Klasse in XAML verwiesen wird deklarieren Sie einen Namespace wie z. B. `xmlns:local` wie hier gezeigt:

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

Hinweis: `EnterActions` und `ExitActions` werden auf ignoriert **Ereignistrigger**.



## <a name="related-links"></a>Verwandte Links

- [Trigger-Beispiel](https://developer.xamarin.com/samples/WorkingWithTriggers)
- [Xamarin.Forms-API-Dokumentation](xref:Xamarin.Forms.TriggerAction`1)
