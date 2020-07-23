---
title: Xamarin.Forms-Trigger
description: In diesem Artikel wird beschrieben, wie Xamarin.Forms-Trigger verwendet werden, um auf Änderungen der Benutzeroberfläche mit XAML zu reagieren. Mit Triggern können Sie Aktionen deklarativ in XAML ausdrücken, die die Darstellung von Steuerelementen basierend auf Ereignissen oder Eigenschaftenänderungen ändern.
ms.prod: xamarin
ms.assetid: 60460F57-63C6-4916-BBB5-A870F1DF53D7
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/17/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 2a71f48fb9911267188e7aa4b4124cd9b7488d31
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86936473"
---
# <a name="xamarinforms-triggers"></a>Xamarin.Forms-Trigger

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithtriggers)

Mit Triggern können Sie Aktionen deklarativ in XAML ausdrücken, die die Darstellung von Steuerelementen basierend auf Ereignissen oder Eigenschaftenänderungen ändern. Darüber hinaus definieren Zustandstrigger, eine spezielle Gruppe von Triggern, wann ein [`VisualState`](xref:Xamarin.Forms.VisualState) angewendet werden muss.

Sie können einen Trigger direkt zu einem Steuerelement zuweisen oder zu einem Ressourcenverzeichnis auf Seitenebene oder Anwendungsebene hinzufügen, das auf mehrere Steuerelemente angewendet wird.

## <a name="property-triggers"></a>Eigenschaftstrigger

Ein einfacher Trigger kann ausschließlich in XAML ausgedrückt werden, indem ein `Trigger`-Element in die Triggersammlung eines Steuerelements hinzugefügt wird.
In diesem Beispiel wird ein Trigger veranschaulicht, der eine `Entry`-Hintergrundfarbe ändert, wenn sie den Fokus erhält:

```xaml
<Entry Placeholder="enter name">
    <Entry.Triggers>
        <Trigger TargetType="Entry"
                 Property="IsFocused" Value="True">
            <Setter Property="BackgroundColor" Value="Yellow" />
            <!-- multiple Setters elements are allowed -->
        </Trigger>
    </Entry.Triggers>
</Entry>
```

Die folgenden Bestandteile der Deklaration des Triggers sind wichtig:

- **TargetType:** der Steuerelementtyp, für den der Trigger gilt.

- **Property:** die Eigenschaft des Steuerelements, das überwacht wird.

- **Value:** der Wert, der die Aktivierung des Triggers auslöst, wenn er bei der überwachten Eigenschaft auftritt.

- **Setter:** eine Sammlung von `Setter`-Elementen kann hinzugefügt werden, wenn die Bedingung des Triggers erfüllt wird. Sie müssen `Property` und `Value` angeben, um diese Eigenschaft festzulegen.

- **EnterActions und ExitActions:** werden im Code geschrieben und können zusätzlich zu (oder anstelle von) `Setter`-Elementen verwendet werden (nicht gezeigt). Sie werden [unten beschrieben](#enteractions-and-exitactions).

### <a name="applying-a-trigger-using-a-style"></a>Anwenden eines Triggers mithilfe eines Stils

Trigger können auch zu einer `Style`-Deklaration in einem Steuerelement, auf einer Seite oder in ein `ResourceDictionary` (Ressourcenverzeichnis) einer Anwendung hinzugefügt werden. In diesem Beispiel wird ein impliziter Stil deklariert (d. h. kein `Key` ist festgelegt), weshalb er für alle `Entry`-Steuerelemente auf der Seite gilt.

```xaml
<ContentPage.Resources>
    <ResourceDictionary>
        <Style TargetType="Entry">
                        <Style.Triggers>
                <Trigger TargetType="Entry"
                         Property="IsFocused" Value="True">
                    <Setter Property="BackgroundColor" Value="Yellow" />
                    <!-- multiple Setters elements are allowed -->
                </Trigger>
            </Style.Triggers>
        </Style>
    </ResourceDictionary>
</ContentPage.Resources>
```

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
            <!-- multiple Setters elements are allowed -->
        </DataTrigger>
    </Button.Triggers>
</Button>
```

> [!TIP]
> Geben Sie beim Auswerten von `Path=Text.Length` immer einen Standardwert für die Zieleigenschaft an (z.B. `Text=""`), da der Wert sonst `null` wäre und der Trigger nicht erwartungsgemäß funktionieren würde.

Zusätzlich zur Angabe von `Setter`-Eigenschaften können Sie auch [`EnterActions` und `ExitActions`](#enteractions-and-exitactions) bereitstellen.

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

- Die generische `TriggerAction<T>`-Klasse mit dem generischen Parameter implementieren, der dem Typ des Steuerelements entspricht, auf das der Trigger angewendet wird. Sie können übergeordnete Klassen wie `VisualElement` verwenden, um Triggeraktionen zu schreiben, die für eine Vielzahl von Steuerelementen funktionieren, oder ein Steuerelementtyp wie `Entry` anzugeben.

- Die `Invoke`-Methode überschreiben. Diese wird immer dann aufgerufen, wenn die Triggerbedingungen erfüllt werden.

- Eigenschaften optional zur Verfügung stellen, die im XAML-Code festgelegt werden können, wenn der Trigger deklariert wird. Ein Beispiel hierfür finden Sie in der `VisualElementPopTriggerAction`-Klasse in der begleitenden Beispielanwendung.

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

Der Ereignistrigger kann dann von XAML verwendet werden:

```xaml
<EventTrigger Event="TextChanged">
    <local:NumericValidationTriggerAction />
</EventTrigger>
```

Passen Sie beim Freigeben von Triggern in einem `ResourceDictionary` auf. Eine Instanz wird unter mehreren Steuerelementen geteilt, also wird jeder Zustand, der einmal konfiguriert wird, auf alle Steuerelemente angewendet.

Beachten Sie, dass Ereignistrigger `EnterActions` und `ExitActions` nicht unterstützen ([siehe unten](#enteractions-and-exitactions)).

## <a name="multi-triggers"></a>Mehrere Trigger

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

- Für die Schaltfläche ist standardmäßig `IsEnabled="false"` festgelegt.
- Die Bedingungen des Multitriggers verwenden den Konverter, um den Wert `Text.Length` in einen `boolean`-Wert zu konvertieren.
- Wenn alle Bedingungen den Wert `true` aufweisen, das Setter-Element legt `true` für die `IsEnabled`-Eigenschaft der Schaltfläche fest.

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

![Beispiele für MultiTrigger](triggers-images/multi-requireall.png)

## <a name="enteractions-and-exitactions"></a>EnterActions und ExitActions

Das Hinzufügen der `EnterActions`- und `ExitActions`-Sammlungen und das Festlegen von `TriggerAction<T>`-Implementierungen stellt eine weitere Möglichkeit zum Implementieren von Änderungen, wenn ein Trigger ausgelöst wird.

Die [`EnterActions`](xref:Xamarin.Forms.TriggerBase.EnterActions)-Sammlung wird verwendet, um ein `IList` von [`TriggerAction`](xref:Xamarin.Forms.TriggerAction)-Objekten zu definieren, die aufgerufen werden, wenn die Triggerbedingung erfüllt ist. Die [`ExitActions`](xref:Xamarin.Forms.TriggerBase.ExitActions)-Sammlung wird verwendet, um ein `IList` von `TriggerAction`-Objekten zu definieren, die aufgerufen werden, wenn die Triggerbedingung nicht mehr erfüllt ist.

> [!NOTE]
> Die in den Sammlungen `EnterActions` und `ExitActions` definierten Objekte [`TriggerAction`](xref:Xamarin.Forms.TriggerAction) werden von der Klasse [`EventTrigger`](xref:Xamarin.Forms.EventTrigger) ignoriert.    

Sie können die *beiden* Eigenschaften `EnterActions` und `ExitActions` sowie `Setter`-Elemente in einem Trigger angeben. Achten Sie jedoch darauf, dass die `Setter`-Elemente sofort aufgerufen werden (sie warten nicht auf den Abschluss von `EnterAction` oder `ExitAction`). Alternativ können Sie alles im Code ausführen, anstatt `Setter` überhaupt zu verwenden.

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
    public int StartsFrom { set; get; }

    protected override void Invoke(VisualElement sender)
    {
        sender.Animate("FadeTriggerAction", new Animation((d) =>
        {
            var val = StartsFrom == 1 ? d : 1 - d;
            // so i was aiming for a different color, but then i liked the pink :)
            sender.BackgroundColor = Color.FromRgb(1, val, 1);
        }),
        length: 1000, // milliseconds
        easing: Easing.Linear);
    }
}
```

## <a name="state-triggers"></a>Zustandstrigger

Zustandstrigger sind spezialisierte Trigger, die die Bedingungen definieren, bei denen [`VisualState`](xref:Xamarin.Forms.VisualState) angewendet werden muss. 

Zustandstrigger werden der Sammlung [`StateTriggers`](xref:Xamarin.Forms.VisualState.StateTriggers) eines [`VisualState`](xref:Xamarin.Forms.VisualState) hinzugefügt. Diese Sammlung kann Trigger mit einem oder mehreren Zustandstriggern enthalten. Ein [`VisualState`](xref:Xamarin.Forms.VisualState) wird angewendet, wenn alle Zustandstrigger in der Sammlung aktiv sind.

Bei Verwendung von Zustandstriggern zur Steuerung visueller Zustände befolgt Xamarin.Forms die folgenden Prioritätsregeln, um zu bestimmen, welcher Trigger (und welches entsprechende [`VisualState`](xref:Xamarin.Forms.VisualState)-Element) aktiv ist:

1. Alle von [`StateTriggerBase`](xref:Xamarin.Forms.StateTriggerBase) abgeleiteten Trigger.
1. Ein [`AdaptiveTrigger`](xref:Xamarin.Forms.AdaptiveTrigger), der aktiviert wird, da die Bedingung [`MinWindowWidth`](xref:Xamarin.Forms.AdaptiveTrigger.MinWindowWidth) erfüllt ist.
1. Ein [`AdaptiveTrigger`](xref:Xamarin.Forms.AdaptiveTrigger), der aktiviert wird, da die Bedingung [`MinWindowHeight`](xref:Xamarin.Forms.AdaptiveTrigger.MinWindowHeight) erfüllt ist.

Wenn mehrere Trigger gleichzeitig aktiv sind (z. B. zwei benutzerdefinierte Trigger), hat der erste im Markup deklarierte Trigger Vorrang.

> [!NOTE]
> Zustandstrigger können in einem [`Style`](xref:Xamarin.Forms.Style) oder direkt für Elemente festgelegt werden.

Weitere Informationen zu visuellen Zuständen finden Sie unter [Xamarin.Forms: Visual State-Manager](~/xamarin-forms/user-interface/visual-state-manager.md).

### <a name="state-trigger"></a>Zustandstrigger

Die Klasse [`StateTrigger`](xref:Xamarin.Forms.StateTrigger), die von der Klasse [`StateTriggerBase`](xref:Xamarin.Forms.StateTriggerBase) abgeleitet ist, hat die bindbare Eigenschaft [`IsActive`](xref:Xamarin.Forms.StateTrigger.IsActive). Ein `StateTrigger` löst eine [`VisualState`](xref:Xamarin.Forms.VisualState)-Änderung aus, wenn die `IsActive`-Eigenschaft ihren Wert ändert.

Die Klasse [`StateTriggerBase`](xref:Xamarin.Forms.StateTriggerBase), die die Basisklasse für alle Zustandstrigger ist, hat die Eigenschaft [`IsActive`](xref:Xamarin.Forms.StateTriggerBase.IsActive) und das Ereignis [`IsActiveChanged`](xref:Xamarin.Forms.StateTriggerBase.IsActiveChanged). Dieses Ereignis wird immer dann ausgelöst, wenn eine [`VisualState`](xref:Xamarin.Forms.VisualState)-Änderung eintritt. Außerdem enthält die `StateTriggerBase`-Klasse überschreibbare `OnAttached`- und `OnDetached`-Methoden.

> [!IMPORTANT]
> Die bindbare Eigenschaft [`StateTrigger.IsActive`](xref:Xamarin.Forms.StateTrigger.IsActive) blendet die geerbte Eigenschaft [`StateTriggerBase.IsActive`](xref:Xamarin.Forms.StateTriggerBase.IsActive) aus.

Im folgenden XAML-Beispiel wird ein [`Style`](xref:Xamarin.Forms.Style) mit [`StateTrigger`](xref:Xamarin.Forms.StateTrigger)-Objekten veranschaulicht:

```xaml
<Style TargetType="Grid">
    <Setter Property="VisualStateManager.VisualStateGroups">
        <VisualStateGroupList>
            <VisualStateGroup>
                <VisualState x:Name="Checked">
                    <VisualState.StateTriggers>
                        <StateTrigger IsActive="{Binding IsToggled}"
                                      IsActiveChanged="OnCheckedStateIsActiveChanged" />
                    </VisualState.StateTriggers>
                    <VisualState.Setters>
                        <Setter Property="BackgroundColor"
                                Value="Black" />
                    </VisualState.Setters>
                </VisualState>
                <VisualState x:Name="Unchecked">
                    <VisualState.StateTriggers>
                        <StateTrigger IsActive="{Binding IsToggled, Converter={StaticResource inverseBooleanConverter}}"
                                      IsActiveChanged="OnUncheckedStateIsActiveChanged" />
                    </VisualState.StateTriggers>
                    <VisualState.Setters>
                        <Setter Property="BackgroundColor"
                                Value="White" />
                    </VisualState.Setters>
                </VisualState>
            </VisualStateGroup>
        </VisualStateGroupList>
    </Setter>
</Style>
```

Bei diesem Beispiel gilt der implizite [`Style`](xref:Xamarin.Forms.Style) für [`Grid`](xref:Xamarin.Forms.Grid)-Objekte. Wenn die `IsToggled`-Eigenschaft des gebundenen Objekts `true` ist, wird die Hintergrundfarbe von `Grid` auf schwarz festgelegt. Wenn die `IsToggled`-Eigenschaft des gebundenen Objekts `false` wird, wird eine [`VisualState`](xref:Xamarin.Forms.VisualState)-Änderung ausgelöst, und die Hintergrundfarbe von `Grid` wird weiß.

Außerdem wird jedes Mal, wenn eine [`VisualState`](xref:Xamarin.Forms.VisualState)-Änderung auftritt, das Ereignis [`IsActiveChanged`](xref:Xamarin.Forms.StateTriggerBase.IsActiveChanged) für `VisualState` ausgelöst. Jeder `VisualState` registriert einen Ereignishandler für dieses Ereignis:

```csharp
void OnCheckedStateIsActiveChanged(object sender, EventArgs e)
{
    StateTriggerBase stateTrigger = sender as StateTriggerBase;
    Console.WriteLine($"Checked state active: {stateTrigger.IsActive}");
}

void OnUncheckedStateIsActiveChanged(object sender, EventArgs e)
{
    StateTriggerBase stateTrigger = sender as StateTriggerBase;
    Console.WriteLine($"Unchecked state active: {stateTrigger.IsActive}");
}
```

Wenn in diesem Beispiel ein Handler für das [`IsActiveChanged`](xref:Xamarin.Forms.StateTriggerBase.IsActiveChanged)-Ereignis ausgelöst wird, gibt der Handler aus, ob das Ereignis [`VisualState`](xref:Xamarin.Forms.VisualState) aktiv ist oder nicht. Beispielsweise werden die folgenden Meldungen im Konsolenfenster ausgegeben, wenn vom visuellen Zustand `Checked` zum visuellen Zustand `Unchecked` gewechselt wird:

```
Checked state active: False
Unchecked state active: True
```

> [!NOTE]
> Benutzerdefinierte Zustandstrigger können erstellt werden, indem von der [`StateTriggerBase`](xref:Xamarin.Forms.StateTriggerBase)-Klasse abgeleitet wird und die `OnAttached`- und `OnDetached`-Methoden überschrieben werden, damit alle erforderlichen Registrierungen und Bereinigungen angewendet werden.

### <a name="adaptive-trigger"></a>Adaptive Trigger

Ein [`AdaptiveTrigger`](xref:Xamarin.Forms.AdaptiveTrigger) löst eine [`VisualState`](xref:Xamarin.Forms.VisualState)-Änderung aus, wenn das Fenster eine bestimmte Höhe oder Breite hat. Dieser Trigger hat zwei bindbare Eigenschaften:

- [`MinWindowHeight`](xref:Xamarin.Forms.AdaptiveTrigger.MinWindowHeight) mit Typ `double`, der die Mindestfensterhöhe angibt, bei der [`VisualState`](xref:Xamarin.Forms.VisualState) angewendet werden soll.
- [`MinWindowWidth`](xref:Xamarin.Forms.AdaptiveTrigger.MinWindowHeight) mit Typ `double`, der die Mindestfensterbreite angibt, bei der [`VisualState`](xref:Xamarin.Forms.VisualState) angewendet werden soll.

> [!NOTE]
> [`AdaptiveTrigger`](xref:Xamarin.Forms.AdaptiveTrigger) ist von der Klasse [`StateTriggerBase`](xref:Xamarin.Forms.StateTriggerBase) abgeleitet und kann daher einen Ereignishandler an das Ereignis [`IsActiveChanged`](xref:Xamarin.Forms.StateTriggerBase.IsActiveChanged) anfügen.

Im folgenden XAML-Beispiel wird ein [`Style`](xref:Xamarin.Forms.Style) mit [`AdaptiveTrigger`](xref:Xamarin.Forms.AdaptiveTrigger)-Objekten veranschaulicht:

```xaml
<Style TargetType="StackLayout">
    <Setter Property="VisualStateManager.VisualStateGroups">
        <VisualStateGroupList>
            <VisualStateGroup>
                <VisualState x:Name="Vertical">
                    <VisualState.StateTriggers>
                        <AdaptiveTrigger MinWindowWidth="0" />
                    </VisualState.StateTriggers>
                    <VisualState.Setters>
                        <Setter Property="Orientation"
                                Value="Vertical" />
                    </VisualState.Setters>
                </VisualState>
                <VisualState x:Name="Horizontal">
                    <VisualState.StateTriggers>
                        <AdaptiveTrigger MinWindowWidth="800" />
                    </VisualState.StateTriggers>
                    <VisualState.Setters>
                        <Setter Property="Orientation"
                                Value="Horizontal" />
                    </VisualState.Setters>
                </VisualState>
            </VisualStateGroup>
        </VisualStateGroupList>
    </Setter>
</Style>
```

Bei diesem Beispiel gilt der implizite [`Style`](xref:Xamarin.Forms.Style) für [`StackLayout`](xref:Xamarin.Forms.StackLayout)-Objekte. Wenn die Fensterbreite im Bereich von 0 bis 800 geräteunabhängigen Einheiten liegt, haben `StackLayout`-Objekte, für die der `Style` gilt, eine vertikale Ausrichtung. Wenn die Fensterbreite größer oder gleich 800 geräteunabhängige Einheiten ist, wird die [`VisualState`](xref:Xamarin.Forms.VisualState)-Änderung ausgelöst, und die Ausrichtung von `StackLayout` wechselt in horizontal:

![Vertikales StackLayout: VisualState](triggers-images/adaptivetrigger-vertical.png "AdaptiveTrigger-Beispiel")
![Horizontales StackLayout: VisualState](triggers-images/adaptivetrigger-horizontal.png "AdaptiveTrigger-Beispiel")

Die Eigenschaften [`MinWindowHeight`](xref:Xamarin.Forms.AdaptiveTrigger.MinWindowHeight) und [`MinWindowWidth`](xref:Xamarin.Forms.AdaptiveTrigger.MinWindowHeight) können unabhängig oder in Verbindung miteinander verwendet werden. Die folgende XAML zeigt ein Beispiel der Festlegung beider Eigenschaften:

```xaml
<AdaptiveTrigger MinWindowWidth="800"
                 MinWindowHeight="1200"/>
```

In diesem Beispiel gibt der [`AdaptiveTrigger`](xref:Xamarin.Forms.AdaptiveTrigger) an, dass der entsprechende [`VisualState`](xref:Xamarin.Forms.VisualState) angewendet wird, wenn die aktuelle Fensterbreite größer oder gleich 800 geräteunabhängige Einheiten und die aktuelle Fensterhöhe größer oder gleich 1200 geräteunabhängige Einheiten ist.

### <a name="compare-state-trigger"></a>Zustandsvergleichstrigger

Der [`CompareStateTrigger`](xref:Xamarin.Forms.CompareStateTrigger) löst eine [`VisualState`](xref:Xamarin.Forms.VisualState)-Änderung aus, wenn eine Eigenschaft gleich einem bestimmten Wert ist. Dieser Trigger hat zwei bindbare Eigenschaften:

- [`Property`](xref:Xamarin.Forms.CompareStateTrigger.Property) mit Typ `object`, der die Eigenschaft angibt, die vom Trigger verglichen wird.
- [`Value`](xref:Xamarin.Forms.CompareStateTrigger.Value) mit Typ `object`, der den Wert angibt, bei dem [`VisualState`](xref:Xamarin.Forms.VisualState) angewendet werden soll.

> [!NOTE]
> [`CompareStateTrigger`](xref:Xamarin.Forms.CompareStateTrigger) ist von der Klasse [`StateTriggerBase`](xref:Xamarin.Forms.StateTriggerBase) abgeleitet und kann daher einen Ereignishandler an das Ereignis [`IsActiveChanged`](xref:Xamarin.Forms.StateTriggerBase.IsActiveChanged) anfügen.

Im folgenden XAML-Beispiel wird ein [`Style`](xref:Xamarin.Forms.Style) mit [`CompareStateTrigger`](xref:Xamarin.Forms.CompareStateTrigger)-Objekten veranschaulicht:

```xaml
<Style TargetType="Grid">
    <Setter Property="VisualStateManager.VisualStateGroups">
        <VisualStateGroupList>
            <VisualStateGroup>
                <VisualState x:Name="Checked">
                    <VisualState.StateTriggers>
                        <CompareStateTrigger Property="{Binding Source={x:Reference checkBox}, Path=IsChecked}"
                                             Value="True" />
                    </VisualState.StateTriggers>
                    <VisualState.Setters>
                        <Setter Property="BackgroundColor"
                                Value="Black" />
                    </VisualState.Setters>
                </VisualState>
                <VisualState x:Name="Unchecked">
                    <VisualState.StateTriggers>
                        <CompareStateTrigger Property="{Binding Source={x:Reference checkBox}, Path=IsChecked}"
                                             Value="False" />
                    </VisualState.StateTriggers>
                    <VisualState.Setters>
                        <Setter Property="BackgroundColor"
                                Value="White" />
                    </VisualState.Setters>
                </VisualState>
            </VisualStateGroup>
        </VisualStateGroupList>
    </Setter>
</Style>
...
<Grid>
    <Frame BackgroundColor="White"
           CornerRadius="12"
           Margin="24"
           HorizontalOptions="Center"
           VerticalOptions="Center">
        <StackLayout Orientation="Horizontal">
            <CheckBox x:Name="checkBox"
                      VerticalOptions="Center" />
            <Label Text="Check the CheckBox to modify the Grid background color."
                   VerticalOptions="Center" />
        </StackLayout>
    </Frame>
</Grid>
```

Bei diesem Beispiel gilt der implizite [`Style`](xref:Xamarin.Forms.Style) für [`Grid`](xref:Xamarin.Forms.Grid)-Objekte. Wenn die [`IsChecked`](xref:Xamarin.Forms.CheckBox.IsChecked)-Eigenschaft von [`CheckBox`](xref:Xamarin.Forms.CheckBox) auf `false` festgelegt ist, wird die Hintergrundfarbe von `Grid` auf weiß festgelegt. Wenn die `CheckBox.IsChecked`-Eigenschaft `true` wird, wird eine [`VisualState`](xref:Xamarin.Forms.VisualState)-Änderung ausgelöst, und die Hintergrundfarbe von `Grid` wird schwarz:

[![Screenshot einer ausgelösten visuellen Zustandsänderung unter iOS und Android](triggers-images/comparestatetrigger-unchecked.png "CompareStateTrigger-Beispiel")](triggers-images/comparestatetrigger-unchecked-large.png#lightbox "CompareStateTrigger-Beispiel")
[![Screenshot einer ausgelösten visuellen Zustandsänderung unter iOS und Android](triggers-images/comparestatetrigger-checked.png "CompareStateTrigger-Beispiel")](triggers-images/comparestatetrigger-unchecked-large.png#lightbox "CompareStateTrigger-Beispiel")

### <a name="device-state-trigger"></a>Gerätezustandstrigger

Der [`DeviceStateTrigger`](xref:Xamarin.Forms.DeviceStateTrigger) löst eine [`VisualState`](xref:Xamarin.Forms.VisualState)-Änderung aus, die auf der Geräteplattform basiert, auf der die Anwendung ausgeführt wird. Dieser Trigger hat eine einzelne bindbare Eigenschaft:

- [`Device`](xref:Xamarin.Forms.DeviceStateTrigger.Device) mit Typ `string`, der die Geräteplattform angibt, auf der [`VisualState`](xref:Xamarin.Forms.VisualState) angewendet werden soll.

> [!NOTE]
> [`DeviceStateTrigger`](xref:Xamarin.Forms.DeviceStateTrigger) ist von der Klasse [`StateTriggerBase`](xref:Xamarin.Forms.StateTriggerBase) abgeleitet und kann daher einen Ereignishandler an das Ereignis [`IsActiveChanged`](xref:Xamarin.Forms.StateTriggerBase.IsActiveChanged) anfügen.

Im folgenden XAML-Beispiel wird ein [`Style`](xref:Xamarin.Forms.Style) mit `DeviceStateTrigger`-Objekten veranschaulicht:

```xaml
<Style x:Key="DeviceStateTriggerPageStyle"
       TargetType="ContentPage">
    <Setter Property="VisualStateManager.VisualStateGroups">
        <VisualStateGroupList>
            <VisualStateGroup>
                <VisualState x:Name="iOS">
                    <VisualState.StateTriggers>
                        <DeviceStateTrigger Device="iOS" />
                    </VisualState.StateTriggers>
                    <VisualState.Setters>
                        <Setter Property="BackgroundColor"
                                Value="Silver" />
                    </VisualState.Setters>
                </VisualState>
                <VisualState x:Name="Android">
                    <VisualState.StateTriggers>
                        <DeviceStateTrigger Device="Android" />
                    </VisualState.StateTriggers>
                    <VisualState.Setters>
                        <Setter Property="BackgroundColor"
                                Value="#2196F3" />
                    </VisualState.Setters>
                </VisualState>
                <VisualState x:Name="UWP">
                    <VisualState.StateTriggers>
                        <DeviceStateTrigger Device="UWP" />
                    </VisualState.StateTriggers>
                    <VisualState.Setters>
                        <Setter Property="BackgroundColor"
                                Value="Aquamarine" />
                    </VisualState.Setters>
                </VisualState>
            </VisualStateGroup>
        </VisualStateGroupList>
    </Setter>
</Style>
```

Bei diesem Beispiel gilt der explizite [`Style`](xref:Xamarin.Forms.Style) für [`ContentPage`](xref:Xamarin.Forms.ContentPage)-Objekte. `ContentPage`-Objekte mit diesem Stil legen ihre Hintergrundfarbe unter iOS auf silber, unter Android auf hellblau und auf der UWP auf aquamarin fest. Die folgenden Screenshots zeigen die resultierenden Seiten unter iOS und Android:

[![Screenshot einer ausgelösten visuellen Zustandsänderung unter iOS und Android](triggers-images/devicestatetrigger.png "DeviceStateTrigger-Beispiel")](triggers-images/devicestatetrigger-large.png#lightbox "DeviceStateTrigger-Beispiel")

### <a name="orientation-state-trigger"></a>Ausrichtungszustandstrigger

Der [`OrientationStateTrigger`](xref:Xamarin.Forms.OrientationStateTrigger) löst eine [`VisualState`](xref:Xamarin.Forms.VisualState)-Änderung aus, wenn sich die Ausrichtung des Geräts ändert. Dieser Trigger hat eine einzelne bindbare Eigenschaft:

- [`Orientation`](xref:Xamarin.Forms.OrientationStateTrigger.Orientation) mit Typ [`DeviceOrientation`](xref:Xamarin.Forms.Internals.DeviceOrientation), der die Ausrichtung angibt, auf die [`VisualState`](xref:Xamarin.Forms.VisualState) angewendet werden soll.

> [!NOTE]
> [`OrientationStateTrigger`](xref:Xamarin.Forms.OrientationStateTrigger) ist von der Klasse [`StateTriggerBase`](xref:Xamarin.Forms.StateTriggerBase) abgeleitet und kann daher einen Ereignishandler an das Ereignis [`IsActiveChanged`](xref:Xamarin.Forms.StateTriggerBase.IsActiveChanged) anfügen.

Im folgenden XAML-Beispiel wird ein [`Style`](xref:Xamarin.Forms.Style) mit `OrientationStateTrigger`-Objekten veranschaulicht:

```xaml
<Style x:Key="OrientationStateTriggerPageStyle"
       TargetType="ContentPage">
    <Setter Property="VisualStateManager.VisualStateGroups">
        <VisualStateGroupList>
            <VisualStateGroup>
                <VisualState x:Name="Portrait">
                    <VisualState.StateTriggers>
                        <OrientationStateTrigger Orientation="Portrait" />
                    </VisualState.StateTriggers>
                    <VisualState.Setters>
                        <Setter Property="BackgroundColor"
                                Value="Silver" />
                    </VisualState.Setters>
                </VisualState>
                <VisualState x:Name="Landscape">
                    <VisualState.StateTriggers>
                        <OrientationStateTrigger Orientation="Landscape" />
                    </VisualState.StateTriggers>
                    <VisualState.Setters>
                        <Setter Property="BackgroundColor"
                                Value="White" />
                    </VisualState.Setters>
                </VisualState>
            </VisualStateGroup>
        </VisualStateGroupList>
    </Setter>
</Style>
```

Bei diesem Beispiel gilt der explizite [`Style`](xref:Xamarin.Forms.Style) für [`ContentPage`](xref:Xamarin.Forms.ContentPage)-Objekte. `ContentPage` Objekte mit dem Stil legen ihre Hintergrundfarbe auf silber fest, wenn die Ausrichtung Hochformat ist. Sie legen ihre Hintergrundfarbe auf weiß fest, wenn die Ausrichtung Querformat ist.

## <a name="related-links"></a>Verwandte Links

- [Triggers Sample (Triggerbeispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithtriggers)
- [Xamarin.Forms: Visual State-Manager](~/xamarin-forms/user-interface/visual-state-manager.md)
- [Xamarin.Forms: Trigger-API](xref:Xamarin.Forms.TriggerAction`1)
