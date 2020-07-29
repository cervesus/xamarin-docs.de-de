---
title: Wiederverwendbare EventToCommandBehavior-Klasse
description: Verhalten können zum Zuordnen von Befehlen zu Steuerelementen verwendet werden, die nicht dazu konzipiert sind, mit Befehlen zu interagieren. In diesem Artikel wird das Erstellen und Nutzen eines Xamarin.Forms-Verhaltens veranschaulicht, um einen Befehl aufzurufen, wenn ein Ereignis ausgelöst wird.
ms.prod: xamarin
ms.assetid: EC7F6556-9776-40B8-9424-A8094482A2F3
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/09/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 0967845ac61ddf5f8e1cc76664a50877d041f011
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86939671"
---
# <a name="reusable-eventtocommandbehavior"></a>Wiederverwendbare EventToCommandBehavior-Klasse

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/behaviors-eventtocommandbehavior)

_Verhalten können zum Zuordnen von Befehlen zu Steuerelementen verwendet werden, die nicht dazu konzipiert sind, mit Befehlen zu interagieren. In diesem Artikel wird das Erstellen und Nutzen eines Xamarin.Forms-Verhaltens veranschaulicht, um einen Befehl aufzurufen, wenn ein Ereignis ausgelöst wird._

## <a name="overview"></a>Übersicht

Die `EventToCommandBehavior`-Klasse ist ein wiederverwendbares benutzerdefiniertes Xamarin.Forms-Verhalten, mit dem ein Befehl als Reaktion auf das Auslösen *jeglicher* Ereignisse ausgeführt wird. Die Ereignisargumente für das Ereignis werden standardmäßig an den Befehl übergeben. Optional können sie auch mithilfe einer [`IValueConverter`](xref:Xamarin.Forms.IValueConverter)-Implementierung konvertiert werden.

Die folgenden Verhaltenseigenschaften müssen für die Verwendung des Verhaltens festgelegt werden:

- **EventName:** Der Name des Ereignis, dem das Verhalten lauscht.
- **Command:** Die `ICommand`-Schnittstelle, die ausgeführt werden soll. Das Verhalten erwartet die `ICommand`-Instanz in der [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)-Eigenschaft des angefügten Steuerelements, das von einem übergeordneten Element geerbt werden kann.

Die folgenden optionalen Verhaltenseigenschaften können ebenfalls festgelegt werden:

- **CommandParameter:** Ein `object`, das an den Befehl übergeben wird.
- **Converter:** Eine [`IValueConverter`](xref:Xamarin.Forms.IValueConverter)-Implementierung, die das Format der Ereignisargumentdaten ändert, während sie von der Bindungs-Engine zwischen *Quelle* und *Ziel* übermittelt werden.

> [!NOTE]
> `EventToCommandBehavior` ist eine benutzerdefinierte Klasse, die in [EventToCommand Behavior sample (EventToCommand-Verhaltensbeispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/behaviors-eventtocommandbehavior) enthalten ist. Allerdings ist sie nicht Teil von Xamarin.Forms.

## <a name="creating-the-behavior"></a>Erstellen des Verhaltens

Die `EventToCommandBehavior`-Klasse leitet von der `BehaviorBase<T>`-Klasse ab, die sich wiederum von der [`Behavior<T>`](xref:Xamarin.Forms.Behavior`1)-Klasse ableitet. Der Zweck der `BehaviorBase<T>`-Klasse ist es, eine Basisklasse für Xamarin.Forms-Verhalten bereitzustellen, für die die [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)-Eigenschaft des Verhaltens auf das angefügte Steuerelement festgelegt sein muss. Hiermit wird sichergestellt, dass das Verhalten an die von der `Command`-Eigenschaft angegebene `ICommand`-Schnittstelle angebunden und sie ausführen kann, wenn das Verhalten genutzt wird.

Die `BehaviorBase<T>`-Klasse stellt eine überschreibbare Methode bereit, [`OnAttachedTo`](xref:Xamarin.Forms.Behavior`1.OnAttachedTo(Xamarin.Forms.BindableObject)) method that sets the [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) of the behavior and an overridable [`OnDetachingFrom`](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject)), die `BindingContext` bereinigt. Darüber hinaus speichert die Klasse einen Verweis auf das angefügte Steuerelement in der `AssociatedObject`-Eigenschaft.

### <a name="implementing-bindable-properties"></a>Implementieren von bindbaren Eigenschaften

Die `EventToCommandBehavior`-Klasse definiert vier [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)-Instanzen, die einen benutzerdefinierten Befehl ausführen, wenn ein Ereignis ausgelöst wird. Diese Eigenschaften werden im folgenden Codebeispiel veranschaulicht:

```csharp
public class EventToCommandBehavior : BehaviorBase<View>
{
  public static readonly BindableProperty EventNameProperty =
    BindableProperty.Create ("EventName", typeof(string), typeof(EventToCommandBehavior), null, propertyChanged: OnEventNameChanged);
  public static readonly BindableProperty CommandProperty =
    BindableProperty.Create ("Command", typeof(ICommand), typeof(EventToCommandBehavior), null);
  public static readonly BindableProperty CommandParameterProperty =
    BindableProperty.Create ("CommandParameter", typeof(object), typeof(EventToCommandBehavior), null);
  public static readonly BindableProperty InputConverterProperty =
    BindableProperty.Create ("Converter", typeof(IValueConverter), typeof(EventToCommandBehavior), null);

  public string EventName { ... }
  public ICommand Command { ... }
  public object CommandParameter { ... }
  public IValueConverter Converter { ...  }
  ...
}
```

Wenn die `EventToCommandBehavior`-Klasse genutzt wird, sollte die `Command`-Eigenschaft an `ICommand` gebunden werden, damit sie als Reaktion auf die Auslösung des Ereignisses ausgeführt wird, das in der Eigenschaft `EventName` definiert ist. Das Verhalten erwartet die `ICommand`-Schnittstelle in der [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)-Eigenschaft des angefügten Steuerelements.

Die Ereignisargumente für das Ereignis werden standardmäßig an den Befehl übergeben. Diese Daten können Sie optional konvertieren, während sie von der Bindungs-Engine zwischen *Quelle* und *Ziel* übermittelt werden, indem Sie eine [`IValueConverter`](xref:Xamarin.Forms.IValueConverter)-Implementierung als `Converter`-Eigenschaftswert angeben. Alternativ können Sie einen Parameter an den Befehl übergeben, indem Sie den Eigenschaftswert `CommandParameter` festlegen.

### <a name="implementing-the-overrides"></a>Implementieren der Überschreibungen

Die `EventToCommandBehavior`-Klasse setzt die [`OnAttachedTo`](xref:Xamarin.Forms.Behavior`1.OnAttachedTo(Xamarin.Forms.BindableObject)) and [`OnDetachingFrom`](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject))-Methoden der `BehaviorBase<T>`-Klasse außer Kraft. Dies ist im folgenden Codebeispiel dargestellt:

```csharp
public class EventToCommandBehavior : BehaviorBase<View>
{
  ...
  protected override void OnAttachedTo (View bindable)
  {
    base.OnAttachedTo (bindable);
    RegisterEvent (EventName);
  }

  protected override void OnDetachingFrom (View bindable)
  {
    DeregisterEvent (EventName);
    base.OnDetachingFrom (bindable);
  }
  ...
}
```

Die [`OnAttachedTo`](xref:Xamarin.Forms.Behavior`1.OnAttachedTo(Xamarin.Forms.BindableObject)) method performs setup by calling the `RegisterEvent` method, passing in the value of the `EventName` property as a parameter. The [`OnDetachingFrom`](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject))-Methode führt die Bereinigung durch, indem die `DeregisterEvent`-Methode aufgerufen und den Wert der `EventName`-Eigenschaft als Parameter übergeben wird.

### <a name="implementing-the-behavior-functionality"></a>Implementieren der Verhaltensfunktionalität

Der Zweck des Verhaltens ist es, den von der `Command`-Eigenschaft definierten Befehl auszuführen, um auf die Auslösung des Ereignisses zu reagieren, das von der `EventName`-Eigenschaft definiert wird. Die Kernfunktionalität des Verhaltens wird im folgenden Codebeispiel veranschaulicht:

```csharp
public class EventToCommandBehavior : BehaviorBase<View>
{
  ...
  void RegisterEvent (string name)
  {
    if (string.IsNullOrWhiteSpace (name)) {
      return;
    }

    EventInfo eventInfo = AssociatedObject.GetType ().GetRuntimeEvent (name);
    if (eventInfo == null) {
      throw new ArgumentException (string.Format ("EventToCommandBehavior: Can't register the '{0}' event.", EventName));
    }
    MethodInfo methodInfo = typeof(EventToCommandBehavior).GetTypeInfo ().GetDeclaredMethod ("OnEvent");
    eventHandler = methodInfo.CreateDelegate (eventInfo.EventHandlerType, this);
    eventInfo.AddEventHandler (AssociatedObject, eventHandler);
  }

  void OnEvent (object sender, object eventArgs)
  {
    if (Command == null) {
      return;
    }

    object resolvedParameter;
    if (CommandParameter != null) {
      resolvedParameter = CommandParameter;
    } else if (Converter != null) {
      resolvedParameter = Converter.Convert (eventArgs, typeof(object), null, null);
    } else {
      resolvedParameter = eventArgs;
    }        

    if (Command.CanExecute (resolvedParameter)) {
      Command.Execute (resolvedParameter);
    }
  }
  ...
}
```

Die `RegisterEvent`-Methode wird als Reaktion auf das Anfügen von `EventToCommandBehavior` an ein Steuerelement ausgeführt und erhält den Wert der `EventName`-Eigenschaft als Parameter. Die Methode versucht dann, das in der `EventName`-Eigenschaft definierte Ereignis für das angefügte Steuerelement zu finden. Wenn das Ereignis gefunden wird, wird die `OnEvent`-Methode als Handlermethode für das Ereignis registriert.

Die `OnEvent`-Methode wird als Reaktion auf die Auslösung des Ereignisses ausgeführt, das in der `EventName`-Eigenschaft definiert wird. Wenn die `Command`-Eigenschaft auf eine gültige `ICommand`-Schnittstelle verweist, versucht die Methode einen Parameter abzurufen, der wie folgt an `ICommand` übergeben werden soll:

- Wenn die `CommandParameter`-Eigenschaft einen Parameter definiert, wird dieser abgerufen.
- Wenn die `Converter`-Eigenschaft andernfalls eine [`IValueConverter`](xref:Xamarin.Forms.IValueConverter)-Implementierung definiert, wird der Konverter ausgeführt und die Ereignisargumentdaten werden von der Bindungs-Engine zwischen *Quelle* und *Ziel* übermittelt.
- Andernfalls wird davon ausgegangen, dass die Ereignisargumente der Parameter sind.

Die datengebundene `ICommand`-Schnittstelle wird dann ausgeführt, wobei der Parameter an den Befehl übergeben wird, sofern die [`CanExecute`](xref:Xamarin.Forms.Command.CanExecute(System.Object))-Methode `true` zurückgibt.

Die `EventToCommandBehavior`-Klasse enthält auch eine `DeregisterEvent`-Methode (hier nicht dargestellt), die von der Eigenschaft [`OnDetachingFrom`](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject)) method. The `DeregisterEvent` method is used to locate and deregister the event defined in the `EventName` ausgeführt wird, um möglichen Arbeitsspeicherverlust zu bereinigen.

## <a name="consuming-the-behavior"></a>Nutzen des Verhaltens

Die `EventToCommandBehavior`-Klasse kann, wie im folgenden XAML-Codebeispiel veranschaulicht, der [`Behaviors`](xref:Xamarin.Forms.VisualElement.Behaviors)-Sammlung eines Steuerelements angefügt werden:

```xaml
<ListView ItemsSource="{Binding People}">
    <ListView.ItemTemplate>
        <DataTemplate>
            <TextCell Text="{Binding Name}" />
        </DataTemplate>
    </ListView.ItemTemplate>
    <ListView.Behaviors>
        <local:EventToCommandBehavior EventName="ItemSelected" Command="{Binding OutputAgeCommand}" Converter="{StaticResource SelectedItemConverter}" />
    </ListView.Behaviors>
</ListView>
<Label Text="{Binding SelectedItemText}" />
```

Der äquivalente C#-Code ist im folgenden Codebeispiel zu sehen:

```csharp
var listView = new ListView();
listView.SetBinding(ItemsView<Cell>.ItemsSourceProperty, "People");
listView.ItemTemplate = new DataTemplate(() =>
{
    var textCell = new TextCell();
    textCell.SetBinding(TextCell.TextProperty, "Name");
    return textCell;
});
listView.Behaviors.Add(new EventToCommandBehavior
{
    EventName = "ItemSelected",
    Command = ((HomePageViewModel)BindingContext).OutputAgeCommand,
    Converter = new SelectedItemEventArgsToSelectedItemConverter()
});

var selectedItemLabel = new Label();
selectedItemLabel.SetBinding(Label.TextProperty, "SelectedItemText");
```

Die `Command`-Eigenschaft des Verhaltens wird an die `OutputAgeCommand`-Eigenschaft der zugehörigen ViewModel-Klasse gebunden, und die `Converter`-Eigenschaft wird für die `SelectedItemConverter`-Instanz festgelegt, wodurch das [`SelectedItem`](xref:Xamarin.Forms.ListView.SelectedItem)-Element der [`ListView`](xref:Xamarin.Forms.ListView)-Klasse aus der [`SelectedItemChangedEventArgs`](xref:Xamarin.Forms.SelectedItemChangedEventArgs)-Klasse zurückgegeben wird.

Bei der Ausführung reagiert das Verhalten auf die Interaktion mit dem Steuerelement. Wenn ein Element in der [`ListView`](xref:Xamarin.Forms.ListView)-Klasse ausgewählt wird, wird das [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected)-Ereignis ausgelöst, wodurch der `OutputAgeCommand`-Befehl der ViewModel-Klasse ausgeführt wird. Dadurch wird wiederum die `SelectedItemText`-Eigenschaft der ViewModel-Klasse aktualisiert, sodass [`Label`](xref:Xamarin.Forms.Label) wie in den folgenden Screenshots gezeigt angebunden wird:

[![Beispielanwendung mit EventToCommandBehavior](event-to-command-behavior-images/screenshots-sml.png)](event-to-command-behavior-images/screenshots.png#lightbox "Beispielanwendung mit EventToCommandBehavior")

Dieses Verhalten zum Ausführen eines Befehls zu verwenden, wenn ein Ereignis ausgelöst wird, hat den Vorteil, dass Befehle mit Steuerelementen verknüpft werden können, die nicht für die Interaktion mit Befehlen konzipiert wurden. Darüber hinaus werden die Codebausteine für die Ereignisverarbeitung aus CodeBehind-Dateien entfernt.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde die Verwendung eines Xamarin.Forms-Verhaltens zum Aufrufen eines Befehls dargestellt, wenn ein Ereignis ausgelöst wird. Verhalten können zum Zuordnen von Befehlen zu Steuerelementen verwendet werden, die nicht dazu konzipiert sind, mit Befehlen zu interagieren.

## <a name="related-links"></a>Verwandte Links

- [EventToCommand Behavior (EventToCommand-Verhalten (Beispiel))](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/behaviors-eventtocommandbehavior)
- [Behavior class (Behavior-Klasse)](xref:Xamarin.Forms.Behavior)
- [Behavior&lt;T&gt; class (Behavior<T>-Klasse)](xref:Xamarin.Forms.Behavior`1)
