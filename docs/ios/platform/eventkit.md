---
title: EventKit
description: Dieses Handbuch enthält eine Übersicht über das Zugreifen auf und Arbeiten mit Kalendern, CalendarEvents und Hinweise zu Daten, die in der Calendar-Datenbank gespeichert, über die EventKit verfügbar gemacht. Es umfasst die wichtigsten Berechtigungsklassen und ihrer Rolle in EventKit-Programmierung sowie eine Anzahl von häufig anfallender Aufgaben bezüglich der EventKit-Framework.
ms.prod: xamarin
ms.assetid: 00E88629-357D-1FCD-4FCE-1330D5D9D32C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: a8439586ac92f8139cf9341611125352c85706e5
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="eventkit"></a>EventKit

_Dieses Handbuch enthält eine Übersicht über das Zugreifen auf und Arbeiten mit Kalendern, CalendarEvents und Hinweise zu Daten, die in der Calendar-Datenbank gespeichert, über die EventKit verfügbar gemacht. Es umfasst die wichtigsten Berechtigungsklassen und ihrer Rolle in EventKit-Programmierung sowie eine Anzahl von häufig anfallender Aufgaben bezüglich der EventKit-Framework._

iOS verfügt über zwei kalenderbezogener Anwendungen integrierte: der Kalender-Anwendung, und die Erinnerungen-Anwendung. Es ist einfach genug, um zu verstehen, wie die Anwendung Kalender Kalenderdaten verwaltet, aber die Anwendung Erinnerungen ist weniger offensichtlich. Erinnerungen können tatsächlich haben die Daten, wenn sie fällig, wenn sie abgeschlossen sind, sind sie in Form eines zugeordnet usw. Daher speichert iOS alle Kalenderdaten, ob es Kalenderereignisse oder Erinnerungen, in einer einzigen Stelle aufgerufen werden der *Kalenderdatenbank*.

Das EventKit-Framework bietet eine Möglichkeit zum Zugriff auf die *Kalender*, *Kalenderereignisse*, und *Erinnerungen* Daten, die Kalender-Datenbank speichert. Zugriff auf die Kalender und Kalender Ereignisse wurde verfügbar seit iOS 4, jedoch Zugriff auf Erinnerungen ist neu in iOS 6.

In diesem Handbuch werden wir abdecken:

-   **Grundlagen der EventKit** – Dies bedeutet, dass die fundamentale Teile der EventKit über die wichtigsten Berechtigungsklassen und bietet einen Überblick über ihre Verwendung. Dieser Abschnitt ist erforderlich, Lesen vor dem bewältigt Fortschritts im nächsten Teil des Dokuments. 
-   **Allgemeine Aufgaben** – die allgemeine Aufgaben Abschnitt dient eine Kurzübersicht zu allgemeinen Aufgaben wie z. B. Vorgehensweise werden; beim Aufzählen der Kalender, erstellen, speichern und Abrufen von "Kalender" Ereignisse und Erinnerungen, als auch die integrierte Controller für Erstellen und Bearbeiten von Kalenderereignissen. In diesem Abschnitt muss nicht vorne nach hinten gelesen werden, da es als Referenz für bestimmte Aufgaben vorgesehen ist. 


Alle Aufgaben in diesem Handbuch sind in der beispielanwendung Companion verfügbar:

 [![](eventkit-images/01.png "Die Begleithandbuch zum Beispiel Anwendung Bildschirme")](eventkit-images/01.png#lightbox)

## <a name="requirements"></a>Anforderungen

EventKit iOS 4.0 eingeführt wurde, jedoch Zugriff auf Daten von Erinnerungen iOS 6.0 eingeführt wurde. Daher müssen Sie hierfür allgemeine EventKit Entwicklung Ziel mindestens Version 4.0 und 6.0 für Erinnerungen.

Darüber hinaus ist die Erinnerungen Anwendung nicht verfügbar ist, im Simulator, was bedeutet, dass Erinnerungen Daten auch nicht verfügbar sein soll, wenn Sie diese zuerst hinzufügen. Darüber hinaus werden die zugriffsanforderungen nur für den Benutzer auf das eigentliche Gerät angezeigt. EventKit Entwicklung wird daher am besten auf dem Gerät getestet.

## <a name="event-kit-basics"></a>Ereignis-Kit-Grundlagen

Bei der Arbeit mit EventKit ist es wichtig, den ein verstehen der allgemeine Klassen und deren Verwendung. Alle diese Klassen finden Sie in der `EventKit` und `EventKitUI` (für die `EKEventEditController`).

### <a name="eventstore"></a>EventStore

Die *EventStore* Klasse ist die wichtigste Klasse in EventKit, da er erforderlich ist, keine Vorgänge im EventKit möglich. Sie können als die permanente Speicherung oder das Datenbankmodul für alle Daten, EventKit betrachtet werden. Aus `EventStore` haben Sie Zugriff auf die sowohl die Kalender und Kalenderereignisse in der Calendar-Anwendung sowie in der Anwendung Erinnerungen Erinnerungen.

Da `EventStore` ist wie ein Datenbankmodul werden er langlebige, was bedeutet, dass es erstellt und während der Lebensdauer einer Instanz einer Anwendung so wenig wie möglich zerstört. In der Tat es wird empfohlen, die nach dem Erstellen einer Instanz von einem `EventStore` in einer Anwendung, Sie behalten, um die auf während der gesamten Lebensdauer der Anwendung, es sei denn, Sie sind Sie sicher, dass es nicht erneut erforderlich. Darüber hinaus sollten alle Aufrufe an einen einzelnen go `EventStore` Instanz. Aus diesem Grund empfiehlt sich die Singleton-Muster für die Beibehaltung von einer einzelnen Instanz verfügbar.

#### <a name="creating-an-event-store"></a>Erstellen eine Ereignisspeicher

Das folgende Codebeispiel veranschaulicht eine effiziente Möglichkeit, erstellen Sie eine einzelne Instanz der `EventStore` -Klasse und die statisch innerhalb einer Anwendung zur Verfügung stellen:

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

Der obige Code verwendet die Singleton-Muster instantiieren eine Instanz von der `EventStore` Wenn die Anwendung geladen wird. Die `EventStore` kann dann von zugegriffen werden global innerhalb der Anwendung wie folgt:

```csharp
App.Current.EventStore;
```

Beachten Sie, dass hier alle Beispiele dieses Muster verwenden, sodass die Ereignisse verweisen auf die `EventStore` über `App.Current.EventStore`.

#### <a name="requesting-access-to-calendar-and-reminder-data"></a>Anfordern des Zugriffs auf die Kalender und Erinnerung Daten

Bevor Sie Berechtigung Zugriff auf alle Daten über die EventStore, muss eine Anwendung zunächst anfordern Zugriff auf die Ereignisse von Kalenderdaten oder die Erinnerungen Daten, je nachdem, die was Sie benötigen. Zu diesem Zweck die `EventStore` macht eine Methode namens `RequestAccess` die – bei Aufruf – zeigt eine Warnungsansicht für dem Benutzer informiert wird, dass die Anwendung Zugriff auf die Kalenderdaten oder einem Erinnerung Daten, je nachdem welche anfordert`EKEntityType`übergeben wird. Da es sich um eine Warnungsansicht ausgelöst, der Aufruf asynchron aufrufen, und eine Abschlusshandler als übergeben ein `NSAction` (oder Lambda), der zwei empfängt Parameter ist ein boolescher Wert, der davon, ob der Zugriff erteilt wurde, und ein `NSError`, das ungleich Null in diesem Fall enthalten Sie die Fehlerinformationen in der Anforderung. Die folgenden codiert werden z. B. Zugriff auf Kalender Ereignisdaten und zeigt eine Warnung angezeigt, wenn die Anforderung nicht gewährt wurde angefordert.

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

Nachdem die Anforderung erteilt wurde, wird es gespeichert haben, solange die Anwendung auf dem Gerät installiert ist und eine Warnung für den Benutzer wird nicht angezeigt.
Zugriff ist jedoch nur auf den Typ der Ressource, Kalenderereignisse oder Erinnerungen gewährt gewährt. Wenn eine Anwendung Zugriff auf beide benötigt, sollten sie beide anfordern.

Da Berechtigung gespeichert ist, ist es relativ billig, führen Sie die Anforderung in jedem Fall, daher ist es ratsam, stets Zugriff fordern Sie vor dem Ausführen eines Vorgangs.

Darüber hinaus der Abschlusshandler in einem separaten (nicht-UI)-Thread aufgerufen wird, alle Updates für die Benutzeroberfläche in den Abschlusshandler sollte aufgerufen werden, über `InvokeOnMainThread`, andernfalls wird eine Ausnahme ausgelöst werden und wenn nicht abgefangen wird, stürzt die Anwendung.

### <a name="ekentitytype"></a>EKEntityType

`EKEntityType` ist eine Enumeration, die den Typ des beschreibt `EventKit` Element oder die. Er verfügt über zwei Werte: `Event` und Erinnerung. Es wird verwendet, in einer Reihe von Methoden, einschließlich `EventStore.RequestAccess` informieren `EventKit` welche Art von Daten erhalten Zugriff auf oder abrufen.

### <a name="ekcalendar"></a>EKCalendar

 *EKCalendar* stellt einen Kalender dar, die eine Gruppe von Kalenderereignisse enthält. Kalender können z. B. in vielen verschiedenen Orten gespeichert werden lokal in *iCloud*in a 3rd party Anbieterstandort wie z. B. ein *Exchange Server* oder *Google*usw. Oft `EKCalendar` wird verwendet, um mitzuteilen `EventKit` , wo Sie nach Ereignissen zu suchen, oder wo zu speichern.

### <a name="ekeventeditcontroller"></a>EKEventEditController

 *EKEventEditController* finden Sie in der `EventKitUI` Namespace und ein integrierten Controller, der zum Bearbeiten oder erstellen Kalenderereignisse verwendet werden kann. Ähnlich wie bei der Kamera-Controller, der integrierten `EKEventEditController` Sitzungsstatusanbieter im Anzeigen der Benutzeroberfläche und Behandlung von Speichern für Sie übernimmt.

### <a name="ekevent"></a>EKEvent

 *EKEvent* Kalenderereignisse darstellt. Beide `EKEvent` und `EKReminder` Vererben `EKCalendarItem` und Felder wie z. B. `Title`, `Notes`und so weiter.

### <a name="ekreminder"></a>EKReminder

 *EKReminder* Erinnerung Element darstellt.

### <a name="ekspan"></a>EKSpan

*EKSpan* ist eine Enumeration, beschreibt die Spanne der Ereignisse aus, wenn Ereignisse, die wiederholt werden können, und verfügt über zwei Werte zu ändern: *ThisEvent* und *FutureEvents*. `ThisEvent` bedeutet, die Änderungen nur auf das bestimmte Ereignis in der Reihe, die auf Sie verwiesen wird, auftreten, wohingegen `FutureEvents` wirkt sich dieses Ereignisses und alle zukünftigen Wiederholungen.

## <a name="tasks"></a>Aufgaben

Zur Vereinfachung der verwenden hat EventKit Nutzung in allgemeine Aufgaben, die in den folgenden Abschnitten beschriebenen aufgeteilt wurde.

### <a name="enumerate-calendars"></a>Auflisten von Kalendern

Um die Kalender aufzulisten, die der Benutzer auf dem Gerät konfiguriert wurde, rufen `GetCalendars` auf die `EventStore` und übergeben Sie den Typ der Kalender (Erinnerungen oder Ereignisse), die Sie empfangen möchten:

```csharp
EKCalendar[] calendars = 
App.Current.EventStore.GetCalendars ( EKEntityType.Event );
```

### <a name="add-or-modify-an-event-using-the-built-in-controller"></a>Hinzufügen oder Ändern eines Ereignisses mit der integrierten-Controller

Die *EKEventEditViewController* übernimmt einen Großteil der Arbeit für Sie, wenn Sie erstellen oder bearbeiten ein Ereignis mit der gleichen Benutzeroberfläche, die dem Benutzer angezeigt wird, wenn die Kalender-Anwendung verwenden möchten:

 [![](eventkit-images/02.png "Die Benutzeroberfläche, die dem Benutzer angezeigt wird, wenn die Kalender-Anwendung verwenden")](eventkit-images/02.png#lightbox)

Um es verwenden zu können, benötigen Sie als einer Variablen auf Klassenebene deklarieren, damit es Garbage collection erhalten nicht, wenn sie innerhalb einer Methode deklariert wird:

```csharp
public class HomeController : DialogViewController
{
        protected CreateEventEditViewDelegate eventControllerDelegate;
        ...
}
```

Klicken Sie dann, ihn zu starten: instanziiert, weisen Sie ihm einen Verweis auf die `EventStore`, Verknüpfen einer *EKEventEditViewDelegate* zu delegieren, und zeigen Sie ihn mit `PresentViewController`:

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

Optional, wenn Sie das Ereignis vorab auffüllen möchten, können Sie entweder ein völlig neues Ereignis (wie unten dargestellt) erstellen oder Sie können ein gespeichertes Ereignis abrufen:

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

Wenn Sie möchten, dass die Benutzeroberfläche vorab aufzufüllen, stellen Sie sicher, dass die Ereigniseigenschaft auf dem Controller:

```csharp
eventController.Event = newEvent;
```

Um ein vorhandenes Ereignis zu verwenden, finden Sie unter der *Abrufen eines Ereignisses nach ID* im Abschnitt weiter unten auf.

Der Delegat sollte außer Kraft setzen die `Completed` -Methode, die vom Controller aufgerufen wird, wenn der Benutzer das Dialogfeld abgeschlossen ist:

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

Optional im Delegaten, sehen Sie sich die *Aktion* in die `Completed` Methode ändern, die Ereignis- und erneut speichern oder andere Aktionen durchführen können, wenn sie abgebrochen wird, welche:

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

Verwenden Sie zum Erstellen eines Ereignisses im Code die *FromStore* Factorymethode auf die `EKEvent` Klasse, und legen Sie alle Daten darauf:

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

Sie müssen festlegen, dass des Kalenders, den das Ereignis in gespeichert werden soll, aber wenn Sie keine Einstellung haben, können Sie die Standardeinstellung:

```csharp
newEvent.Calendar = App.Current.EventStore.DefaultCalendarForNewEvents;
```

Um das Ereignis zu speichern, rufen die *SaveEvent* Methode für die `EventStore`:

```csharp
NSError e;
App.Current.EventStore.SaveEvent ( newEvent, EKSpan.ThisEvent, out e );
```

Nachdem es gespeichert ist, die *EventIdentifier* Eigenschaft wird mit einem eindeutigen Bezeichner, die später verwendet werden kann, um das Ereignis abzurufen aktualisiert werden:

```csharp
Console.WriteLine ("Event Saved, ID: " + newEvent.CalendarItemIdentifier);
```

 `EventIdentifier` eine Zeichenfolge GUID formatiert ist.

### <a name="create-a-reminder-programmatically"></a>Erstellen Sie eine Erinnerung programmgesteuert

Erstellen eine Erinnerung im Code ist ähnlich wie ein Ereignis erstellen:

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

Zum Abrufen eines Ereignisses durch diese ID ist, verwenden Sie die *EventFromIdentifier* Methode für die `EventStore` und übergibt ihn dann der `EventIdentifier` aus dem Ereignis per Pull abgerufen wurde:

```csharp
EKEvent mySavedEvent = App.Current.EventStore.EventFromIdentifier ( newEvent.EventIdentifier );
```

Bei Ereignissen, es ist zwei Bezeichnereigenschaften sind jedoch `EventIdentifier` ist die einzige Person, die für diesen funktioniert.

### <a name="retrieving-a-reminder-by-id"></a>Eine Erinnerung nach ID abrufen

Um eine Erinnerung abzurufen, verwenden Sie die *GetCalendarItem* Methode für die `EventStore` und übergibt ihn dann der *CalendarItemIdentifier*:

```csharp
EKCalendarItem myReminder = App.Current.EventStore.GetCalendarItem ( reminder.CalendarItemIdentifier );
```

Da `GetCalendarItem` gibt eine `EKCalendarItem`, müssen umgewandelt werden, um `EKReminder` ggf. Erinnerung Datenzugriff oder verwenden Sie die Instanz als ein `EKReminder` später.

Verwenden Sie keine `GetCalendarItem` für Kalenderereignisse, wie zum Zeitpunkt der Verfassung, funktioniert dies nicht.

### <a name="deleting-an-event"></a>Löschen eines Ereignisses

Um ein Kalenderereignis löschen, rufen *RemoveEvent* auf Ihre `EventStore` und übergeben Sie einen Verweis auf das Ereignis und die entsprechenden `EKSpan`:

```csharp
NSError e;
App.Current.EventStore.RemoveEvent ( mySavedEvent, EKSpan.ThisEvent, true, out e);
```

Beachten Sie jedoch, nachdem ein Ereignis gelöscht wurde, der Ereignisverweis werden `null`.

### <a name="deleting-a-reminder"></a>Löschen eine Erinnerung

Um eine Erinnerung zu löschen, rufen *RemoveReminder* auf die `EventStore` und übergeben Sie einen Verweis auf die Erinnerung:

```csharp
NSError e;
App.Current.EventStore.RemoveReminder ( myReminder as EKReminder, true, out e);
```

Beachten Sie, dass im obigen Code eine Umwandlung in den besteht `EKReminder`, da `GetCalendarItem` wurde verwendet, um sie abzurufen

### <a name="searching-for-events"></a>Suchen nach Ereignissen

Um Kalenderereignisse zu suchen, müssen Sie erstellen eine *NSPredicate* Objekt über die *PredicateForEvents* Methode für die `EventStore`. Ein `NSPredicate` ist eine Abfrage Datenobjekt, iOS verwendet wird, um Übereinstimmungen zu suchen:

```csharp
DateTime startDate = DateTime.Now.AddDays ( -7 );
DateTime endDate = DateTime.Now;
// the third parameter is calendars we want to look in, to use all calendars, we pass null
NSPredicate query = App.Current.EventStore.PredicateForEvents ( startDate, endDate, null );
```

Nachdem Sie erstellt haben die `NSPredicate`, verwenden die *EventsMatching* Methode für die `EventStore`:

```csharp
// execute the query
EKCalendarItem[] events = App.Current.EventStore.EventsMatching ( query );
```

Beachten Sie, dass Abfragen sind synchrone (blockierende) und je nach der Abfrage sehr lange dauern können, daher Sie einen neuen Thread oder eine Aufgabe sollten zu diesem Zweck zu starten.

### <a name="searching-for-reminders"></a>Erinnerungen werden gesucht

Suchen nach Erinnerungen entspricht Ereignisse ist. erfordert ein Prädikat, aber der Aufruf ist bereits asynchron, sodass Sie needn't Blockieren des Threads Gedanken:

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

Dieses Dokument hat einen Überblick über die beiden wichtigen Teile von EventKit Framework und eine Anzahl der am häufigsten auszuführenden Aufgaben. Allerdings EventKit Framework ist sehr groß und leistungsstarke und verfügt über Funktionen, die hier, wie z. B. eingeführt wurden noch nicht: Batchaktualisierungen, Alarme, die Wiederholung auf Ereignisse, registrieren und überwacht Änderungen an der Kalenderdatenbank konfigurieren Konfigurieren Festlegen von GeoFences und vieles mehr.  Weitere Informationen finden Sie in der Apple [Kalender und Erinnerungen Programmierhandbuch](https://developer.apple.com/library/prerelease/ios/#documentation/DataManagement/Conceptual/EventKitProgGuide/Introduction/Introduction.html).


## <a name="related-links"></a>Verwandte Links

- [Kalender (Beispiel)](https://developer.xamarin.com/samples/monotouch/Calendars/)
- [Einführung in iOS 6](~/ios/platform/introduction-to-ios6/index.md)
- [Einführung in die Kalender die Erinnerung und Bewertung](https://developer.apple.com/library/prerelease/ios/#documentation/DataManagement/Conceptual/EventKitProgGuide/Introduction/Introduction.html)
