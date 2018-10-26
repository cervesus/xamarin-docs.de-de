---
title: EventKit in Xamarin.iOS
description: Dieses Dokument beschreibt EventKit und für die Verwendung in Xamarin.iOS. Es wird erläutert, Kalender, Termine im Kalender und Erinnerungen, sucht Klassen, die häufig verwendet, wenn es sich bei der Programmierung mit EventKit und vieles mehr.
ms.prod: xamarin
ms.assetid: 00E88629-357D-1FCD-4FCE-1330D5D9D32C
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/19/2017
ms.openlocfilehash: ea03c8b382e2de29bd20ab1d696d7abb7733e182
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50120228"
---
# <a name="eventkit-in-xamarinios"></a>EventKit in Xamarin.iOS

iOS enthält zwei kalenderbezogenen Anwendungen integrierten: die Kalender-Anwendung, und die Erinnerungen-Anwendung. Es ist einfach genug, um zu verstehen, wie der Kalenderanwendung Kalenderdaten verwaltet, aber die Erinnerungen-Anwendung ist weniger offensichtlich. Erinnerungen Datumsangaben fällig, wenn sie abgeschlossen sind, werden diese in Form von zugeordnet haben usw. Daher iOS alle Kalenderdaten, speichert, ob sie Termine im Kalender oder Erinnerungen an einem Ort, dem Namen der *Kalenderdatenbank*.

Das EventKit-Framework bietet eine Möglichkeit zum Zugriff auf die *Kalender*, *Termine im Kalender*, und *Erinnerungen* Daten, die Kalender-Datenbank speichert. Zugriff auf den Kalender und Kalender Ereignisse ist seit verfügbaren iOS 4, jedoch Zugriff auf Erinnerungen ist neu in iOS 6.

In diesem Handbuch werden wir behandeln:

-   **Grundlagen der EventKit** – Dadurch werden die grundlegenden Teile der EventKit über die wichtigsten Klassen vorgestellt, und bietet einen Überblick über ihre Verwendung. Dieser Abschnitt ist erforderlich, Lesen vor dem Umgang mit dem nächsten Teil dieses Dokuments. 
-   **Allgemeine Aufgaben** – richtet sich an den Bereich für allgemeine Aufgaben eine Kurzübersicht über das allgemeine Aktionen wie z. B. sein; Kalender auflisten, erstellen, speichern und Abrufen von "Kalender" Ereignisse und Erinnerungen, als auch mithilfe der integrierten Controller für Erstellen und Termine im Kalender zu ändern. In diesem Abschnitt muss nicht von vorne, hinten, gelesen werden, da es als Referenz für bestimmte Aufgaben vorgesehen ist. 


Alle Aufgaben in dieser Anleitung sind in der beispielanwendung Companion verfügbar:

 [![](eventkit-images/01.png "Die begleitende Beispiel Anwendung Bildschirme")](eventkit-images/01.png#lightbox)

## <a name="requirements"></a>Anforderungen

EventKit in iOS 4.0 eingeführt wurde, aber Zugriff auf Daten von Erinnerungen in iOS 6.0 eingeführt wurde. Um allgemeine EventKit Entwicklung zu tun, Sie müssen mindestens als Ziel Version 4.0 und 6.0 Erinnerungen einrichten.

Darüber hinaus ist die Erinnerungen-Anwendung nicht verfügbar, auf dem Simulator, was bedeutet, dass Erinnerungen Daten auch nicht verfügbar sein soll, wenn Sie diese zuerst hinzufügen. Darüber hinaus werden Anforderungen nur an den Benutzer auf dem Gerät selbst angezeigt. Daher wird EventKit Entwicklung am besten auf dem Gerät getestet.

## <a name="event-kit-basics"></a>Ereignis-Kit-Grundlagen

Bei der Arbeit mit EventKit ist es wichtig, dass die allgemeine Klassen und ihre Verwendung verstanden haben. Alle diese Klassen befinden sich die `EventKit` und `EventKitUI` (für die `EKEventEditController`).

### <a name="eventstore"></a>Eventstore-Komponente

Die *EventStore* Klasse ist die wichtigste Klasse in EventKit, da dies für alle Operationen in EventKit erforderlich ist. Sie kann als beständigen Speicher benötigen oder für alle EventKit-Datenbank-Engine betrachtet werden. Von `EventStore` haben Zugriff auf die Kalender und die Ereignisse im Kalender in der Kalenderanwendung sowie Erinnerungen in der Anwendung Erinnerungen.

Da `EventStore` ist wie eine Datenbank-Engine wird langlebig, was bedeutet, dass es erstellt und zerstört während der Lebensdauer einer Anwendungsinstanz so wenig wie möglich wird. In der Tat, es wird empfohlen, nach dem Erstellen einer Instanz von einem `EventStore` in einer Anwendung, Sie behalten Sie diesen Verweis auf für die gesamte Lebensdauer der Anwendung, es sei denn, Sie sind Sie sicher, dass es nicht erneut erforderlich. Darüber hinaus funktionieren alle Aufrufe an eine einzelne `EventStore` Instanz. Aus diesem Grund empfiehlt das Singleton-Muster für die Beibehaltung von einer einzelnen Instanz zur Verfügung.

#### <a name="creating-an-event-store"></a>Erstellen eine Store-Ereignis

Der folgende Code veranschaulicht eine effiziente Möglichkeit zum Erstellen einer einzelnen Instanz von der `EventStore` Klasse und statisch in einer Anwendung zur Verfügung stellen:

```csharp
public class App
{
    public static App Current {
            get { return current; }
    }
    private static App current;

    public EKEventStore EventStore {
            get { return eventStore; }
    }
    protected EKEventStore eventStore;

    static App ()
    {
            current = new App();
    }
    protected App () 
    {
            eventStore = new EKEventStore ( );
    }
}
```

Der obige Code verwendet das Singleton-Muster zum Instanziieren einer Instanz von der `EventStore` Wenn die Anwendung geladen wurde. Die `EventStore` möglich und erfolgt global von innerhalb der Anwendung wie folgt:

```csharp
App.Current.EventStore;
```

Beachten Sie, dass alle Beispiele hier dieses Muster verwenden, damit die Ereignisse verweisen auf die `EventStore` über `App.Current.EventStore`.

#### <a name="requesting-access-to-calendar-and-reminder-data"></a>Anfordern des Zugriffs auf den Kalender und Erinnerungsdaten

Bevor er den Zugriff auf alle Daten über die eventstore-Komponente, muss eine Anwendung zunächst anfordern Zugriff auf die Ereignisse Kalenderdaten oder Erinnerungen, je nachdem, welche benötigten Daten. Zu diesem Zweck die `EventStore` macht eine Methode namens `RequestAccess` die – bei Aufruf – zeigt eine Warnungsansicht für dem Benutzer, der angibt, dass die Anwendung Zugriff auf die Kalenderdaten oder Erinnerungsdaten, je nachdem welche anfordert`EKEntityType`übergeben wird. Da es sich um eine Warnungsansicht auslöst, der Aufruf asynchron aufrufen, und einen Abschlusshandler übergeben als ein `NSAction` (oder Lambda), der zwei empfängt Parameter; ein boolescher Wert, der davon, ob der Zugriff gewährt wurde, und ein `NSError`, das Wenn ungleich Null wird enthalten Sie die Fehlerinformationen in der Anforderung. Beispielsweise wird die folgende codiert anfordern des Zugriffs auf kalenderereignisdaten und anzeigen, die eine Warnung angezeigt, wenn die Anforderung nicht gewährt wurde.

```csharp
App.Current.EventStore.RequestAccess (EKEntityType.Event, 
    (bool granted, NSError e) => {
            if (granted)
                    //do something here
            else
                    new UIAlertView ( "Access Denied", 
"User Denied Access to Calendar Data", null,
"ok", null).Show ();
            } );
```

Sobald die Anforderung gewährt wurde, wird es so lange die Anwendung auf dem Gerät installiert ist und eine Warnung für den Benutzer nicht eingeblendet wird gespeichert.
Allerdings erhält Zugriff nur auf den Typ der Ressource, die entweder Termine im Kalender oder Erinnerungen gewährt. Wenn eine Anwendung Zugriff auf beide benötigt, sollten sie beide anfordern.

Da sich Berechtigung gespeichert wird, ist es relativ günstig zu stellen die Anforderung jedes Mal, daher ist es eine gute Idee, immer den Zugriff anfordern, vor dem Ausführen eines Vorgangs.

Darüber hinaus der Abschlusshandler in einem separaten (nicht-UI) Thread aufgerufen wird, alle Aktualisierungen der Benutzeroberfläche in den Abschlusshandler aufgerufen werden soll, über `InvokeOnMainThread`, andernfalls wird eine Ausnahme ausgelöst werden und wenn nicht abgefangen wird, stürzt die Anwendung ab.

### <a name="ekentitytype"></a>EKEntityType

`EKEntityType` ist eine Enumeration, die den Typ des beschreibt `EventKit` Element oder Daten. Es verfügt über zwei Werte: `Event` und Erinnerung. Hiermit wird in einer Reihe von Methoden, einschließlich `EventStore.RequestAccess` mitteilen `EventKit` welche Art von Daten erhalten Sie Zugriff auf oder abzurufen.

### <a name="ekcalendar"></a>EKCalendar

 *EKCalendar* stellt einen Kalender dar, die eine Gruppe von Termine im Kalender enthält. Kalender können z. B. in vielen verschiedenen Speicherorten gespeichert werden lokal in *iCloud*in a 3rd party Anbieterstandort wie z. B. eine *Exchange Server* oder *Google*usw. Oft `EKCalendar` mitteilt `EventKit` , wo Sie nach Ereignissen suchen oder deren Speicherort.

### <a name="ekeventeditcontroller"></a>EKEventEditController

 *EKEventEditController* finden Sie in der `EventKitUI` Namespace und ist ein integrierte Controller, der zum Bearbeiten oder erstellen Termine im Kalender verwendet werden kann. Ähnlich wie bei der Kamera-Controller, den integrierten `EKEventEditController` leistet die Schwerarbeit für Sie in der Benutzeroberfläche angezeigt, und Behandeln von speichern.

### <a name="ekevent"></a>EKEvent

 *EKEvent* stellt ein Kalenderereignis dar. Beide `EKEvent` und `EKReminder` erben `EKCalendarItem` und verfügen über Felder wie z. B. `Title`, `Notes`und so weiter.

### <a name="ekreminder"></a>EKReminder

 *EKReminder* Erinnerung Element darstellt.

### <a name="ekspan"></a>EKSpan

*EKSpan* ist eine Enumeration, der die Spanne der Ereignisse aus, wenn Ereignisse zu ändern, die wiederholt werden kann, und verfügt über zwei Werte beschreibt: *ThisEvent* und *FutureEvents*. `ThisEvent` bedeutet, dass Änderungen nur auf das bestimmte Ereignis in der Reihe, die verwiesen wird, Vorkommen werden, während `FutureEvents` wirkt sich dieses Ereignisses und alle zukünftigen Wiederholungen.

## <a name="tasks"></a>Aufgaben

Für die der Einfachheit halber wurde EventKit Nutzung in allgemeine Aufgaben, die in den folgenden Abschnitten beschriebenen unterteilt wurde.

### <a name="enumerate-calendars"></a>Kalender auflisten

Rufen Sie zum Aufzählen der Kalender, die der Benutzer auf dem Gerät konfiguriert hat `GetCalendars` auf die `EventStore` und übergeben Sie den Typ der Kalender (Erinnerungen oder Ereignisse), die Sie erhalten möchten:

```csharp
EKCalendar[] calendars = 
App.Current.EventStore.GetCalendars ( EKEntityType.Event );
```

### <a name="add-or-modify-an-event-using-the-built-in-controller"></a>Fügen Sie hinzu oder ändern Sie ein Ereignis mithilfe des integrierten Controller

Die *EKEventEditViewController* übernimmt einen Großteil der Schwerstarbeit für Sie, wenn zum Erstellen oder bearbeiten ein Ereignis mit der die gleiche Benutzeroberfläche, die bei Verwendung der Kalender-Anwendung, die dem Benutzer angezeigt werden sollen:

 [![](eventkit-images/02.png "Die Benutzeroberfläche, die für den Benutzer angezeigt wird, bei Verwendung der Kalender-Anwendung")](eventkit-images/02.png#lightbox)

Um es zu verwenden, sollten Sie als eine Variable auf Klassenebene deklarieren, damit es Garbage collection erhalten nicht, wenn er innerhalb einer Methode deklariert wird:

```csharp
public class HomeController : DialogViewController
{
        protected CreateEventEditViewDelegate eventControllerDelegate;
        ...
}
```

Klicken Sie dann, ihn zu starten: instanziieren, weisen Sie ihm einen Verweis auf die `EventStore`, Verknüpfen einer *EKEventEditViewDelegate* , delegieren, und klicken Sie dann mithilfe `PresentViewController`:

```csharp
EventKitUI.EKEventEditViewController eventController = 
        new EventKitUI.EKEventEditViewController ();

// set the controller's event store - it needs to know where/how to save the event
eventController.EventStore = App.Current.EventStore;

// wire up a delegate to handle events from the controller
eventControllerDelegate = new CreateEventEditViewDelegate ( eventController );
eventController.EditViewDelegate = eventControllerDelegate;

// show the event controller
PresentViewController ( eventController, true, null );
```

Optional, wenn das Ereignis vorab aufgefüllt werden soll, können Sie entweder ein völlig neues Ereignis (wie unten dargestellt) erstellen oder Sie können eine gespeicherte Ereignis abrufen:

```csharp
EKEvent newEvent = EKEvent.FromStore ( App.Current.EventStore );
// set the alarm for 10 minutes from now
newEvent.AddAlarm ( EKAlarm.FromDate ( DateTime.Now.AddMinutes ( 10 ) ) );
// make the event start 20 minutes from now and last 30 minutes
newEvent.StartDate = DateTime.Now.AddMinutes ( 20 );
newEvent.EndDate = DateTime.Now.AddMinutes ( 50 );
newEvent.Title = "Get outside and exercise!";
newEvent.Notes = "This is your reminder to go and exercise for 30 minutes.”;
```

Wenn Sie die Benutzeroberfläche vorab auffüllen möchten, stellen Sie sicher, dass die Ereignis-Eigenschaft auf dem Controller festgelegt:

```csharp
eventController.Event = newEvent;
```

Um ein vorhandenes Ereignis zu verwenden, finden Sie unter den *Abrufen eines Ereignisses nach ID* im Abschnitt weiter unten auf.

Der Delegat sollte überschreiben die `Completed` -Methode, die vom Controller aufgerufen wird, wenn der Benutzer das Dialogfeld abgeschlossen ist:

```csharp
protected class CreateEventEditViewDelegate : EventKitUI.EKEventEditViewDelegate
{
        // we need to keep a reference to the controller so we can dismiss it
        protected EventKitUI.EKEventEditViewController eventController;

        public CreateEventEditViewDelegate (EventKitUI.EKEventEditViewController eventController)
        {
                // save our controller reference
                this.eventController = eventController;
        }

        // completed is called when a user eith
        public override void Completed (EventKitUI.EKEventEditViewController controller, EKEventEditViewAction action)
        {
                eventController.DismissViewController (true, null);
                }
        }
}
```

Optional in den Delegaten, sehen Sie sich die *Aktion* in die `Completed` Methode, um das Ereignis und erneut speichern zu ändern oder andere Dinge tun, wenn der Abbruch, usw.:

```csharp
public override void Completed (EventKitUI.EKEventEditViewController controller, EKEventEditViewAction action)
{
        eventController.DismissViewController (true, null);

        switch ( action ) {

        case EKEventEditViewAction.Canceled:
                break;
        case EKEventEditViewAction.Deleted:
                break;
        case EKEventEditViewAction.Saved:
                // if you wanted to modify the event you could do so here,
// and then save:
                //App.Current.EventStore.SaveEvent ( controller.Event, )
                break;
        }
}
```

### <a name="creating-an-event-programmatically"></a>Programmgesteuertes Erstellen eines Ereignisses

Verwenden Sie zum Erstellen eines Ereignisses im Code die *FromStore* Factorymethode für die `EKEvent` Klasse, und legen Sie alle Daten darauf:

```csharp
EKEvent newEvent = EKEvent.FromStore ( App.Current.EventStore );
// set the alarm for 10 minutes from now
newEvent.AddAlarm ( EKAlarm.FromDate ( DateTime.Now.AddMinutes ( 10 ) ) );
// make the event start 20 minutes from now and last 30 minutes
newEvent.StartDate = DateTime.Now.AddMinutes ( 20 );
newEvent.EndDate = DateTime.Now.AddMinutes ( 50 );
newEvent.Title = "Get outside and do some exercise!";
newEvent.Notes = "This is your motivational event to go and do 30 minutes of exercise. Super important. Do this.";
```

Sie müssen festlegen, dass des Kalenders, den das Ereignis in gespeichert werden sollen, aber wenn Sie keine Einstellung haben, können Sie die Standardeinstellung verwenden:

```csharp
newEvent.Calendar = App.Current.EventStore.DefaultCalendarForNewEvents;
```

Um das Ereignis zu speichern, rufen die *SaveEvent* Methode für die `EventStore`:

```csharp
NSError e;
App.Current.EventStore.SaveEvent ( newEvent, EKSpan.ThisEvent, out e );
```

Nach dem Speichern der *EventIdentifier* Eigenschaft mit einem eindeutigen Bezeichner, die später verwendet werden kann, um das Ereignis abzurufen, aktualisiert werden:

```csharp
Console.WriteLine ("Event Saved, ID: " + newEvent.CalendarItemIdentifier);
```

 `EventIdentifier` ist eine GUID formatierte Zeichenfolge.

### <a name="create-a-reminder-programmatically"></a>Erstellen Sie programmgesteuert eine Erinnerung.

Erstellen eine Erinnerung in Code ist ähnlich wie ein Kalenderereignis erstellen:

```csharp
EKReminder reminder = EKReminder.Create ( App.Current.EventStore );
reminder.Title = "Do something awesome!";
reminder.Calendar = App.Current.EventStore.DefaultCalendarForNewReminders;
```

Rufen Sie zum Speichern der *SaveReminder* Methode für die `EventStore`:

```csharp
NSError e;
App.Current.EventStore.SaveReminder ( reminder, true, out e );
```

### <a name="retrieving-an-event-by-id"></a>Ein Ereignis nach ID abrufen

Zum Abrufen eines Ereignisses durch diese ID ist, verwenden die *EventFromIdentifier* Methode für die `EventStore` , und übergeben sie die `EventIdentifier` , stammt aus dem Ereignis:

```csharp
EKEvent mySavedEvent = App.Current.EventStore.EventFromIdentifier ( newEvent.EventIdentifier );
```

Für Ereignisse ist sind zwei weitere Eigenschaften der objektkennung, aber `EventIdentifier` ist der einzige, das für dieses funktioniert.

### <a name="retrieving-a-reminder-by-id"></a>Eine Erinnerung nach ID abrufen

Verwenden Sie zum Abrufen einer erinnerungs der *GetCalendarItem* Methode für die `EventStore` , und übergeben sie die *CalendarItemIdentifier*:

```csharp
EKCalendarItem myReminder = App.Current.EventStore.GetCalendarItem ( reminder.CalendarItemIdentifier );
```

Da `GetCalendarItem` gibt ein `EKCalendarItem`, müssen Sie in umgewandelt werden `EKReminder` bei Bedarf auf Erinnerungsdaten zuzugreifen, oder verwenden Sie die Instanz als ein `EKReminder` später noch mal.

Verwenden Sie keine `GetCalendarItem` für Kalenderereignisse, wie zum Zeitpunkt der Verfassung, es funktioniert nicht.

### <a name="deleting-an-event"></a>Löschen eines Ereignisses

Um ein Kalenderereignis löschen, rufen *RemoveEvent* auf Ihre `EventStore` und übergeben einen Verweis auf das Ereignis und die entsprechenden `EKSpan`:

```csharp
NSError e;
App.Current.EventStore.RemoveEvent ( mySavedEvent, EKSpan.ThisEvent, true, out e);
```

Beachten Sie jedoch, nachdem ein Ereignis gelöscht wurde, der Ereignisverweis werden `null`.

### <a name="deleting-a-reminder"></a>Erinnerung löschen

Um eine Erinnerung löschen, rufen *RemoveReminder* auf die `EventStore` und übergeben einen Verweis auf die Erinnerung:

```csharp
NSError e;
App.Current.EventStore.RemoveReminder ( myReminder as EKReminder, true, out e);
```

Beachten Sie, dass im Code oben gibt es eine Umwandlung in `EKReminder`, da `GetCalendarItem` wurde verwendet, um ihn abrufen

### <a name="searching-for-events"></a>Suche nach Ereignissen

Um Termine im Kalender suchen, müssen Sie erstellen eine *NSPredicate* -Objekt über die *PredicateForEvents* Methode für die `EventStore`. Ein `NSPredicate` ist eine Abfrage Datenobjekt, iOS verwendet, um Übereinstimmungen zu suchen:

```csharp
DateTime startDate = DateTime.Now.AddDays ( -7 );
DateTime endDate = DateTime.Now;
// the third parameter is calendars we want to look in, to use all calendars, we pass null
NSPredicate query = App.Current.EventStore.PredicateForEvents ( startDate, endDate, null );
```

Nach der Erstellung der `NSPredicate`, verwenden Sie die *EventsMatching* Methode für die `EventStore`:

```csharp
// execute the query
EKCalendarItem[] events = App.Current.EventStore.EventsMatching ( query );
```

Beachten Sie, dass Abfragen werden synchrone (blockierende) und sehr lange, je nach Abfrage, dauern können daher Sie einen neuen Thread oder eine Aufgabe sollten, dafür zu erstellen.

### <a name="searching-for-reminders"></a>Erinnerungen werden gesucht

Suchen nach Erinnerungen ist ähnlich wie Ereignisse. Es muss ein Prädikat, aber der Aufruf ist bereits asynchron, sodass Sie die Blockierung des Threads kümmern müssen nicht:

```csharp
// create our NSPredicate which we'll use for the query
NSPredicate query = App.Current.EventStore.PredicateForReminders ( null );

// execute the query
App.Current.EventStore.FetchReminders (
        query, ( EKReminder[] items ) => {
                // do someting with the items
        } );
```

## <a name="summary"></a>Zusammenfassung

In diesem Dokument haben einen Überblick über die beiden wichtigen Teile von Framework EventKit und eine Anzahl der am häufigsten auszuführenden Aufgaben. Das Framework EventKit ist jedoch sehr umfangreiche und leistungsstarke, und umfasst Features, die hier, wie z. B. eingeführt wurden noch nicht: Batchaktualisierungen, Alarme, Konfigurieren der Wiederholung für Ereignis-, registrieren und das Lauschen auf Änderungen auf der Kalenderdatenbank konfigurieren Festlegen von GeoFences und vieles mehr.  Weitere Informationen finden Sie in der Apple [Kalender und Erinnerungen Programming Guide](https://developer.apple.com/library/prerelease/ios/#documentation/DataManagement/Conceptual/EventKitProgGuide/Introduction/Introduction.html).


## <a name="related-links"></a>Verwandte Links

- [Kalender (Beispiel)](https://developer.xamarin.com/samples/monotouch/Calendars/)
- [Einführung in iOS 6](~/ios/platform/introduction-to-ios6/index.md)
- [Einführung in die Kalender und Erinnerungen](https://developer.apple.com/library/prerelease/ios/#documentation/DataManagement/Conceptual/EventKitProgGuide/Introduction/Introduction.html)
