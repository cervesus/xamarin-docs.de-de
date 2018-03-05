---
title: Wiederverwendbare EventToCommandBehavior
description: "Verhaltensweisen können Befehle mit Steuerelementen zuordnen, die nicht entwickelt wurden, für die Interaktion mit Befehlen verwendet werden. Dieser Artikel veranschaulicht, wie mit einer Xamarin.Forms-Verhalten Aufrufen eines Befehls, wenn ein Ereignis ausgelöst wird."
ms.topic: article
ms.prod: xamarin
ms.assetid: EC7F6556-9776-40B8-9424-A8094482A2F3
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
ms.openlocfilehash: 2727d83e55e305af1372ece35bdf22abfc653fe7
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="reusable-eventtocommandbehavior"></a>Wiederverwendbare EventToCommandBehavior

_Verhaltensweisen können Befehle mit Steuerelementen zuordnen, die nicht entwickelt wurden, für die Interaktion mit Befehlen verwendet werden. Dieser Artikel veranschaulicht, wie mit einer Xamarin.Forms-Verhalten Aufrufen eines Befehls, wenn ein Ereignis ausgelöst wird._

## <a name="overview"></a>Übersicht

Die `EventToCommandBehavior` Klasse ist eine wiederverwendbare Xamarin.Forms benutzerdefiniertes Verhalten, die als Antwort auf einen Befehl ausführt *alle* Auslösen von Ereignissen. Wird standardmäßig die Ereignisargumente für das Ereignis an den Befehl übergeben werden wird, und kann optional konvertiert, indem ein [ `IValueConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.IValueConverter/) Implementierung.

Die folgenden Verhaltenseigenschaften müssen festgelegt werden, um das Verhalten verwendet:

- **Ereignisname** – der Name des Ereignisses auf das Verhalten lauscht.
- **Befehl** – die **ICommand** ausgeführt werden. Das Verhalten davon ausgeht, die `ICommand` Instanz, auf die [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) des angefügten Steuerelements, die von einem übergeordneten Element geerbt werden kann.

Die folgenden Eigenschaften für die optionales Verhalten können auch festgelegt werden:

- **CommandParameter** – ein `object` , wird der Befehl übergeben werden.
- **Konverter** – ein [ `IValueConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.IValueConverter/) Implementierung, die das Format der Ereignisdaten Argument geändert wird, bei der Übergabe zwischen *Quelle* und *Ziel*durch das Bindungsmodul.

## <a name="creating-the-behavior"></a>Erstellen das Verhalten

Die `EventToCommandBehavior` Klasse leitet sich von der `BehaviorBase<T>` -Klasse, die wiederum von abgeleitet ist die [ `Behavior<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/) Klasse. Der Zweck der `BehaviorBase<T>` Klasse ist eine Basisklasse für alle Xamarin.Forms Verhalten bereitstellen, die erforderlich ist der [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) des Verhaltens, das für das angefügte Steuerelement festgelegt werden. Dadurch wird sichergestellt, dass das Verhalten kann zum Binden und Ausführen der `ICommand` gemäß der `Command` Eigenschaft, wenn das Verhalten genutzt wird.

Die `BehaviorBase<T>` -Klasse stellt eine überschreibbare [ `OnAttachedTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnAttachedTo/p/Xamarin.Forms.BindableObject/) Methode, die festlegt der [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) das Verhalten und eine überschreibbare [ `OnDetachingFrom` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnDetachingFrom/p/Xamarin.Forms.BindableObject/)-Methode, die bereinigt die `BindingContext`. Darüber hinaus die Klasse speichert einen Verweis auf das angefügte Steuerelement in der `AssociatedObject` Eigenschaft.

### <a name="implementing-bindable-properties"></a>Implementieren von bindbare Eigenschaften

Die `EventToCommandBehavior` Klasse definiert vier [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) Instanzen, die ein Benutzer ausführen Befehl definiert, wenn ein Ereignis ausgelöst wird. Diese Eigenschaften werden im folgenden Codebeispiel gezeigt:

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

Wenn die `EventToCommandBehavior` Klasse verbraucht ist, die `Command` Eigenschaft an Daten gebunden werden soll ein `ICommand` als Antwort auf den Auslösen von Ereignissen ausgeführt werden, die in definiert ist die `EventName` Eigenschaft. Das Verhalten wird davon ausgegangen, dass die `ICommand` auf die [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) des zugeordneten Steuerelements.

Standardmäßig werden die Ereignisargumente für das Ereignis an den Befehl übergeben werden. Diese Daten können optional konvertiert werden, bei der Übergabe zwischen *Quelle* und *Ziel* durch das Bindungsmodul, durch Angeben einer [ `IValueConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.IValueConverter/) Implementierung als der `Converter` Eigenschaftswert. Alternativ kann ein Parameter an den Befehl übergeben werden, durch Angeben der `CommandParameter` Eigenschaftswert.

### <a name="implementing-the-overrides"></a>Implementieren die Außerkraftsetzungen

Der `EventToCommandBehavior` -Klasse überschreibt die [ `OnAttachedTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnAttachedTo/p/Xamarin.Forms.BindableObject/) und [ `OnDetachingFrom` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnDetachingFrom/p/Xamarin.Forms.BindableObject/) Methoden der `BehaviorBase<T>` Klasse, wie im folgenden Codebeispiel gezeigt:

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

Die [ `OnAttachedTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnAttachedTo/p/Xamarin.Forms.BindableObject/) Methode führt Setup durch Aufrufen der `RegisterEvent` -Methode auf und übergibt den Wert von der `EventName` Eigenschaft als Parameter. Die [ `OnDetachingFrom` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnDetachingFrom/p/Xamarin.Forms.BindableObject/) -Methode führt die Bereinigung durch Aufrufen der `DeregisterEvent` -Methode auf und übergibt den Wert von der `EventName` Eigenschaft als Parameter.

### <a name="implementing-the-behavior-functionality"></a>Implementieren die Verhaltensfunktionalität.

Das Verhalten beim Ausführen des Befehls definiert, indem Sie dient der `Command` Eigenschaft als Antwort auf die das Auslösen von Ereignissen, die von definiert ist die `EventName` Eigenschaft. Im folgenden Codebeispiel wird die Kernfunktionen von Verhalten angezeigt:

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

Die `RegisterEvent` Methode ausgeführt wird, als Antwort auf die `EventToCommandBehavior` dem Anfügen an ein Steuerelement, und er erhält den Wert von der `EventName` Eigenschaft als Parameter. Die Methode versucht anschließend, suchen Sie das Ereignis definiert, der `EventName` Eigenschaft, auf dem angefügten-Steuerelement. Vorausgesetzt, dass das Ereignis gefunden werden, kann die `OnEvent` Methode ist die Verwendung der Handlermethode für das Ereignis werden registriert.

Die `OnEvent` Methode ausgeführt wird, als Reaktion auf das Auslösen des Ereignisses, das in definiert die `EventName` Eigenschaft. Vorausgesetzt, dass die `Command` -Eigenschaft verweist auf eine gültige `ICommand`, die Methode versucht, einen Parameter an übergeben Abrufen der `ICommand` wie folgt:

- Wenn die `CommandParameter` Eigenschaft definiert einen Parameter, es wird abgerufen.
- Andernfalls gilt: Wenn die `Converter` -Eigenschaft definiert ein [ `IValueConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.IValueConverter/) Implementierung der Konverter wird ausgeführt, und konvertiert die Daten für das Ereignis-Argument, bei der Übergabe zwischen *Quelle* und *Ziel* durch das Bindungsmodul.
- Andernfalls die Ereignisargumente wird angenommen, dass der Parameter sein.

Die gebundenen Daten `ICommand` dann ausgeführt wird, wird im Parameter übergeben wird, den Befehl bereitgestellt, die die [ `CanExecute` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Command.CanExecute/p/System.Object/) -Methode zurückkehrt `true`.

Obwohl hier nicht angezeigt der `EventToCommandBehavior` enthält auch eine `DeregisterEvent` -Methode, die ausgeführt wird, indem Sie die [ `OnDetachingFrom` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnDetachingFrom/p/Xamarin.Forms.BindableObject/) Methode. Die `DeregisterEvent` Methode dient zum Suchen und Aufheben der Registrierung des Ereignis definiert, der `EventName` -Eigenschaft, um alle mögliche Speicherverluste Cleanup.

## <a name="consuming-the-behavior"></a>Nutzen das Verhalten

Die `EventToCommandBehavior` Klasse angefügt werden kann, um die [ `Behaviors` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Behaviors/) Auflistung eines Steuerelements, wie im folgenden Beispiel der Verwendung von XAML-Code veranschaulicht:

```xaml
<ListView ItemsSource="{Binding People}">
  <ListView.Behaviors>
    <local:EventToCommandBehavior EventName="ItemSelected" Command="{Binding OutputAgeCommand}"
                                  Converter="{StaticResource SelectedItemConverter}" />
  </ListView.Behaviors>
</ListView>
<Label Text="{Binding SelectedItemText}" />
```

Der äquivalente C#-Code ist im folgenden Codebeispiel zu sehen:

```csharp
var listView = new ListView ();
listView.SetBinding (ItemsView<Cell>.ItemsSourceProperty, "People");
listView.Behaviors.Add (new EventToCommandBehavior {
  EventName = "ItemSelected",
  Command = ((HomePageViewModel)BindingContext).OutputAgeCommand,
  Converter = new SelectedItemEventArgsToSelectedItemConverter ()
});

var selectedItemLabel = new Label ();
selectedItemLabel.SetBinding (Label.TextProperty, "SelectedItemText");
```

Die `Command` Eigenschaft des Verhaltens an datengebunden ist die `OutputAgeCommand` Eigenschaft des zugeordneten ViewModel während der `Converter` Eigenschaft auf festgelegt ist die `SelectedItemConverter` Instanz fest, welche gibt die [ `SelectedItem` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.SelectedItem/)von der [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) aus der [ `SelectedItemChangedEventArgs` ](https://developer.xamarin.com/api/type/Xamarin.Forms.SelectedItemChangedEventArgs/).

Zur Laufzeit wird das Verhalten auf die Interaktion mit dem Steuerelement reagieren. Wenn ein Element ausgewählt ist, der [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/), die [ `ItemSelected` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemSelected/) Ereignis wird ausgelöst, die ausgeführt werden die `OutputAgeCommand` im ViewModel. Dadurch wird wiederum aktualisiert das ViewModel `SelectedItemText` Eigenschaft, die die [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) bindet zu, wie in den folgenden Screenshots dargestellt:

[ ![](event-to-command-behavior-images/screenshots-sml.png "Beispielanwendung mit EventToCommandBehavior")](event-to-command-behavior-images/screenshots.png "Beispielanwendung mit EventToCommandBehavior")

Der Vorteil der Verwendung dieses Verhalten zur Ausführung eines Befehls, wenn ein Ereignis ausgelöst wird, ist, dass Befehle Steuerelemente zugeordnet werden können, die für die Interaktion mit Befehlen entworfen wurden. Darüber hinaus wird dies vom Code-Behind-Dateien mit Codebausteinen Ereignisbehandlung Code entfernt.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wird gezeigt, mithilfe eines Xamarin.Forms-Verhaltens zum Aufrufen eines Befehls, wenn ein Ereignis ausgelöst wird. Verhaltensweisen können Befehle mit Steuerelementen zuordnen, die nicht entwickelt wurden, für die Interaktion mit Befehlen verwendet werden.


## <a name="related-links"></a>Verwandte Links

- [EventToCommand Verhalten (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/behaviors/eventtocommandbehavior/)
- [Verhalten](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior/)
- [Verhalten<T>](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/)
