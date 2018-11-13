---
title: Wiederverwendbare EventToCommandBehavior
description: Verhaltensweisen können Befehle mit Steuerelementen zuordnen, die nicht entwickelt wurden, für die Interaktion mit Befehlen verwendet werden. Dieser Artikel veranschaulicht das Erstellen und nutzen ein Xamarin.Forms-Verhalten, um einen Befehl aufzurufen, wenn ein Ereignis ausgelöst wird.
ms.prod: xamarin
ms.assetid: EC7F6556-9776-40B8-9424-A8094482A2F3
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/09/2018
ms.openlocfilehash: 8bf8f86cf708806d1c17b3fe4eda0755f98fd646
ms.sourcegitcommit: 03dfb4a2c20ad68515875b415e7d84ee9b0a8cb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/12/2018
ms.locfileid: "51563185"
---
# <a name="reusable-eventtocommandbehavior"></a>Wiederverwendbare EventToCommandBehavior

_Verhaltensweisen können Befehle mit Steuerelementen zuordnen, die nicht entwickelt wurden, für die Interaktion mit Befehlen verwendet werden. Dieser Artikel veranschaulicht das Erstellen und nutzen ein Xamarin.Forms-Verhalten, um einen Befehl aufzurufen, wenn ein Ereignis ausgelöst wird._

## <a name="overview"></a>Übersicht

Die `EventToCommandBehavior` Klasse ist ein wiederverwendbarer benutzerdefinierten Xamarin.Forms-Verhalten, das als Reaktion auf einen Befehl ausführt *alle* Auslösen von Ereignissen. In der Standardeinstellung die Ereignisargumente für das Ereignis an den Befehl übergeben werden wird, und kann optional sein konvertiert, indem ein [ `IValueConverter` ](xref:Xamarin.Forms.IValueConverter) Implementierung.

Das Verhalten verwendet, müssen die folgenden Verhaltenseigenschaften festgelegt werden:

- **EventName** – der Name des Ereignisses auf das Verhalten lauscht.
- **Befehl** – die `ICommand` ausgeführt werden. Das Verhalten erwartet die `ICommand` Instanz in der [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) von das angefügte Steuerelement, das von einem übergeordneten Element geerbt werden kann.

Die folgenden Eigenschaften für die optionales Verhalten können auch festgelegt werden:

- **CommandParameter** – ein `object` , wird an den Befehl übergeben werden.
- **Konverter** – ein [ `IValueConverter` ](xref:Xamarin.Forms.IValueConverter) Implementierung, die das Format der Ereignisdaten Argument geändert wird, wie die Übergabe zwischen *Quelle* und *Ziel*durch die Bindungs-Engine.

> [!NOTE]
> Die `EventToCommandBehavior` ist eine benutzerdefinierte Klasse, die gefunden werden kann die [EventToCommand Verhalten Beispiel](https://developer.xamarin.com/samples/xamarin-forms/behaviors/eventtocommandbehavior/), und sind nicht Teil von Xamarin.Forms.

## <a name="creating-the-behavior"></a>Erstellen das Verhalten

Die `EventToCommandBehavior` Klasse leitet sich von der `BehaviorBase<T>` -Klasse, die wiederum von abgeleitet ist die [ `Behavior<T>` ](xref:Xamarin.Forms.Behavior`1) Klasse. Der Zweck der `BehaviorBase<T>` Klasse ist eine Basisklasse für alle Xamarin.Forms-Verhaltensweisen, die erfordern die [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) des Verhaltens, das auf das angefügte Steuerelement festgelegt werden. Dadurch wird sichergestellt, dass das Verhalten kann zu binden, und führen die `ICommand` gemäß der `Command` Eigenschaft, wenn das Verhalten genutzt wird.

Die `BehaviorBase<T>` -Klasse stellt eine überschreibbare [ `OnAttachedTo` ](xref:Xamarin.Forms.Behavior`1.OnAttachedTo(Xamarin.Forms.BindableObject)) Methode, die festlegt der [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) das Verhalten und eine überschreibbare [ `OnDetachingFrom` ](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject))-Methode, die bereinigt die `BindingContext`. Darüber hinaus die Klasse speichert einen Verweis auf das angefügte Steuerelement in der `AssociatedObject` Eigenschaft.

### <a name="implementing-bindable-properties"></a>Implementieren die bindbare Eigenschaften

Die `EventToCommandBehavior` Klasse definiert vier [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) Instanzen, die ein Benutzer ausführen Befehl definiert, wenn ein Ereignis ausgelöst wird. Diese Eigenschaften werden im folgenden Codebeispiel gezeigt:

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

Wenn die `EventToCommandBehavior` Klasse wird verwendet, die `Command` -Eigenschaft an Daten gebunden werden soll ein `ICommand` als Reaktion auf den Auslösen von Ereignissen ausgeführt werden, die in definiert ist die `EventName` Eigenschaft. Erwarten, dass Sie das Verhalten finden Sie die `ICommand` auf die [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) des zugeordneten Steuerelements.

Standardmäßig werden die Ereignisargumente für das Ereignis an den Befehl übergeben werden. Diese Daten konvertiert werden können optional, wie sie zwischen übergeben wird *Quelle* und *Ziel* durch die Bindungs-Engine, durch Angeben einer [ `IValueConverter` ](xref:Xamarin.Forms.IValueConverter) Implementierung als die `Converter` -Eigenschaftswert. Alternativ kann ein Parameter für den Befehl übergeben werden, durch Angabe der `CommandParameter` -Eigenschaftswert.

### <a name="implementing-the-overrides"></a>Implementieren die Außerkraftsetzungen

Die `EventToCommandBehavior` -Klasse überschreibt die [ `OnAttachedTo` ](xref:Xamarin.Forms.Behavior`1.OnAttachedTo(Xamarin.Forms.BindableObject)) und [ `OnDetachingFrom` ](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject)) Methoden der `BehaviorBase<T>` Klasse, wie im folgenden Codebeispiel gezeigt:

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

Die [ `OnAttachedTo` ](xref:Xamarin.Forms.Behavior`1.OnAttachedTo(Xamarin.Forms.BindableObject)) Methode führt Setup durch Aufrufen der `RegisterEvent` Methode und übergeben den Wert des der `EventName` -Eigenschaft als Parameter. Die [ `OnDetachingFrom` ](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject)) -Methode führt die Bereinigung durch Aufrufen der `DeregisterEvent` Methode und übergeben den Wert des der `EventName` -Eigenschaft als Parameter.

### <a name="implementing-the-behavior-functionality"></a>Implementieren der Verhaltensfunktionalität.

Das Verhalten beim Ausführen des Befehls durch definierten dient der `Command` Eigenschaft als Reaktion auf die das Auslösen von Ereignissen, die von definiert ist die `EventName` Eigenschaft. Die Kernfunktionen von Verhalten wird im folgenden Codebeispiel dargestellt:

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

Die `RegisterEvent` Methode wird ausgeführt, als Reaktion auf die `EventToCommandBehavior` angefügt wird, ein Steuerelement, und empfängt den Wert von der `EventName` -Eigenschaft als Parameter. Die Methode dann versucht, suchen Sie das Ereignis definiert, der `EventName` Eigenschaft; für das angefügte Steuerelement. Vorausgesetzt, dass das Ereignis gefunden wurde, kann die `OnEvent` Methode werden die Ereignishandlermethode für das Ereignis registriert ist.

Die `OnEvent` Methode wird ausgeführt, als Reaktion auf die das Auslösen von Ereignissen, die in definiert ist die `EventName` Eigenschaft. Vorausgesetzt, dass die `Command` -Eigenschaft verweist auf eine gültige `ICommand`, die Methode versucht, einen Parameter an übergeben Abrufen der `ICommand` wie folgt:

- Wenn die `CommandParameter` Eigenschaft definiert einen Parameter, es wird abgerufen.
- Andernfalls gilt: Wenn die `Converter` -Eigenschaft definiert eine [ `IValueConverter` ](xref:Xamarin.Forms.IValueConverter) der Konverter-Implementierung ausgeführt wird und die Ereignisdaten für das Argument konvertiert, wie sie zwischen übergeben wird *Quelle* und *Ziel* durch die Bindungs-Engine.
- Andernfalls die Ereignisargumente wird angenommen, dass der Parameter sein.

Der datengebundene `ICommand` dann ausgeführt wird, wird im Parameter übergeben wird, an den Befehl, der bereitgestellt, die die [ `CanExecute` ](xref:Xamarin.Forms.Command.CanExecute(System.Object)) Methodenrückgabe `true`.

Obwohl hier nicht angezeigt, die `EventToCommandBehavior` enthält auch eine `DeregisterEvent` -Methode, die ausgeführt wird, indem die [ `OnDetachingFrom` ](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject)) Methode. Die `DeregisterEvent` Methode dient zum Suchen und Aufheben der Registrierung des Ereignis definiert, der `EventName` Eigenschaft verwenden, um die Bereinigung, die alle möglichen Speicherverlusten.

## <a name="consuming-the-behavior"></a>Nutzen das Verhalten

Die `EventToCommandBehavior` Klasse angefügt werden kann, um die [ `Behaviors` ](xref:Xamarin.Forms.VisualElement.Behaviors) Auflistung eines Steuerelements, wie in den folgenden XAML-Codebeispiel veranschaulicht:

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

Die `Command` Eigenschaft des Verhaltens an datengebunden ist die `OutputAgeCommand` Eigenschaft der zugehörigen ViewModel zuständig ist, während die `Converter` -Eigenschaftensatz auf die `SelectedItemConverter` -Instanz, gibt die [ `SelectedItem` ](xref:Xamarin.Forms.ListView.SelectedItem)von der [ `ListView` ](xref:Xamarin.Forms.ListView) aus der [ `SelectedItemChangedEventArgs` ](xref:Xamarin.Forms.SelectedItemChangedEventArgs).

Zur Laufzeit antwortet auf Benutzerinteraktion mit dem Steuerelement das Verhalten. Wenn ein Element ausgewählt ist, der [ `ListView` ](xref:Xamarin.Forms.ListView), [ `ItemSelected` ](xref:Xamarin.Forms.ListView.ItemSelected) Ereignis wird ausgelöst, die ausgeführt werden die `OutputAgeCommand` in "ViewModel". Dies aktualisiert wiederum das "ViewModel" `SelectedItemText` Eigenschaft, die die [ `Label` ](xref:Xamarin.Forms.Label) bindet zu, wie in den folgenden Screenshots gezeigt:

[![](event-to-command-behavior-images/screenshots-sml.png "Beispielanwendung mit EventToCommandBehavior")](event-to-command-behavior-images/screenshots.png#lightbox "Beispielanwendung mit EventToCommandBehavior")

Der Vorteil der Verwendung dieses Verhalten, um einen Befehl auszuführen, wenn ein Ereignis ausgelöst wird, ist, dass Befehle mit Steuerelementen verknüpft werden können, die entworfen wurden, für die Interaktion mit den Befehlen. Darüber hinaus entfällt dadurch Ereignisbehandlung partitionierungsaufgaben vom Code-Behind-Dateien.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wird gezeigt, mithilfe eines Xamarin.Forms-Verhaltens zum Aufrufen eines Befehls, wenn ein Ereignis ausgelöst wird. Verhaltensweisen können Befehle mit Steuerelementen zuordnen, die nicht entwickelt wurden, für die Interaktion mit Befehlen verwendet werden.

## <a name="related-links"></a>Verwandte Links

- [EventToCommand-Verhalten (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/behaviors/eventtocommandbehavior/)
- [Verhalten](xref:Xamarin.Forms.Behavior)
- [Verhalten&lt;T&gt;](xref:Xamarin.Forms.Behavior`1)
