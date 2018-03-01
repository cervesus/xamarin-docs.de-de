---
title: Kalender
ms.topic: article
ms.prod: xamarin
ms.assetid: 78666541-CA14-4CD8-A94A-A9621C57813E
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 8075473464472c5a830f62ebfc91c00ad54d1b98
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="calendar"></a>Kalender

<a name="Calendar_API" />

## <a name="calendar-api"></a>Kalender-API

Ein neuer Satz von Kalender APIs Android 4 eingeführten unterstützt Anwendungen, die darauf ausgelegt sind, lesen und Schreiben von Daten an den Kalenderanbieter. Diese APIs unterstützen eine Vielzahl von Optionen der Interaktion mit Kalenderdaten, einschließlich der Möglichkeit zum Lesen und Schreiben von Ereignissen, Teilnehmer und Erinnerungen. Mithilfe des Calendar-Anbieters in Ihrer Anwendung werden Daten, die Sie über die API hinzufügen in der integrierten Kalender-app angezeigt, die mit Android 4 ausgeliefert wird.

<a name="Adding_Permissions" />

## <a name="adding-permissions"></a>Hinzufügen von Berechtigungen

Bei der Arbeit mit dem neuen Kalender APIs in Ihrer Anwendung wird im ersten Schritt müssen Sie die entsprechenden Berechtigungen auf der Android-Manifest hinzufügen. Die Berechtigungen, die Sie hinzufügen müssen sind `android.permisson.READ_CALENDAR` und `android.permission.WRITE_CALENDAR`, je nachdem, ob Sie lesen oder Schreiben von Kalenderdaten.

<a name="Using_the_Calendar_Contract" />

## <a name="using-the-calendar-contract"></a>Verwendung des Kalender-Vertrags

Nachdem Sie die Berechtigungen festgelegt haben, können Sie mit interagieren Kalenderdaten mithilfe der `CalendarContract` Klasse. Diese Klasse stellt ein Datenmodell, die Anwendungen verwenden können, bei der Interaktion mit der Calendar-Anbieter. Die `CalendarContract` ermöglicht es Anwendungen, die Uris zu Kalender Entitäten, z. B. Kalender und Ereignisse zu beheben. Darüber hinaus eine Möglichkeit für die Interaktion mit verschiedenen Felder in jeder Entität, z. B. eines Kalenders Name und ID, oder ein Ereignis die Anfangs- und Enddatum.

Sehen wir uns ein Beispiel, das die Kalender-API verwendet. In diesem Beispiel untersuchen wir, Kalender und die zugehörigen Ereignisse auflisten und wie einen Kalender ein neues Ereignis hinzu.

<a name="Listing_Calendars" />

## <a name="listing-calendars"></a>Auflisten von Kalendern

Zunächst sehen wir uns auf die Kalender auflisten, die in der Kalender-app registriert wurden. Zu diesem Zweck können Instanziieren einer `CursorLoader`. Eingeführt in Android 3.0 (11-API), `CursorLoader` ist die bevorzugte Methode zum Verarbeiten einer `ContentProvider`. Zumindest müssen wir den Inhalt angeben, Uri für Kalender und die Spalten aus, die zurückgegeben werden soll; die Spezifikation für diese Spalte wird als bezeichnet eine _Projektion_.

Aufrufen der `CursorLoader.LoadInBackground` -Methode erlaubt es uns, Content-Anbieter für Daten, z. B. des Kalender-Anbieters abgefragt.
`LoadInBackground` die tatsächliche Last durchführt und gibt eine `Cursor` mit den Ergebnissen der Abfrage.

Die `CalendarContract` hilft uns beim Angeben von sowohl des Inhalts `Uri` und der Projektion. Um den Inhalt abzurufen `Uri` zum Abfragen von Kalendern, können wir einfach die `CalendarContract.Calendars.ContentUri` Eigenschaft wie folgt:

```csharp
var calendarsUri = CalendarContract.Calendars.ContentUri;
```

Mithilfe der `CalendarContract` an, welche Kalender Being Spalten ist gleichermaßen einfach. Wir fügen nur Felder in der `CalendarContract.Calendars.InterfaceConsts` Klasse, um ein Array. Der folgende Code enthält beispielsweise der Kalender-ID, einen Anzeigenamen und Kontoname:

```csharp
string[] calendarsProjection = {
    CalendarContract.Calendars.InterfaceConsts.Id,
    CalendarContract.Calendars.InterfaceConsts.CalendarDisplayName,
    CalendarContract.Calendars.InterfaceConsts.AccountName
};
```

Die `Id` muss berücksichtigt werden, wenn Sie verwenden eine `SimpleCursorAdapter` um die Daten an die Benutzeroberfläche binden, da es in Kürze angezeigt wird. Mit dem Content-Uri und Projektion direktes instanziieren wir die `CursorLoader` , und rufen Sie die `CursorLoader.LoadInBackground` Methode, um einen Cursor mit dem Kalenderdaten zurückzugeben, wie unten dargestellt:

```csharp
var loader = new CursorLoader(this, calendarsUri, calendarsProjection, null, null, null);
var cursor = (ICursor)loader.LoadInBackground();

```

Die Benutzeroberfläche für dieses Beispiel enthält eine `ListView`, wobei jedes Element in der Liste, die einen einzelnen Kalender darstellt. Das folgende XML zeigt das Markup mit dem `ListView`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
android:orientation="vertical"
android:layout_width="fill_parent"
android:layout_height="fill_parent">
  <ListView
    android:id="@android:id/android:list"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content" />
</LinearLayout>
```

Darüber hinaus müssen wir die Benutzeroberfläche für jedes Listenelement angeben, die wir in einer separaten XML-Datei, wie folgt platzieren:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
android:orientation="vertical"
android:layout_width="fill_parent"
android:layout_height="wrap_content">
  <TextView android:id="@+id/calDisplayName"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:textSize="16dip" />
  <TextView android:id="@+id/calAccountName"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:textSize="12dip" />
</LinearLayout>
```

Ab diesem Punkt ist es nur normale Android Code zum Binden von Daten aus dem Cursor an der Benutzeroberfläche. Wir verwenden eine `SimpleCursorAdapter` wie folgt:

```csharp
string[] sourceColumns = {
    CalendarContract.Calendars.InterfaceConsts.CalendarDisplayName,
    CalendarContract.Calendars.InterfaceConsts.AccountName };

int[] targetResources = {
    Resource.Id.calDisplayName, Resource.Id.calAccountName };      

SimpleCursorAdapter adapter = new SimpleCursorAdapter (this,
    Resource.Layout.CalListItem, cursor, sourceColumns, targetResources);

ListAdapter = adapter;
```

Im obigen Code, verwendet der Adapter im angegebenen Spalten der `sourceColumns` array erstellt und schreibt sie in die Elemente der Benutzeroberfläche in der `targetResources` Array für jeden Kalendereintrag im Cursor. Die Aktivität, die hier verwendete ist eine Unterklasse von `ListActivity`; er enthält die `ListAdapter` -Eigenschaft, die wir den Adapter festgelegt.

Hier ist ein Screenshot, der das Endergebnis anzeigt, mit der Calendar-Informationen angezeigt, der `ListView`:

[![Anzeigen von zwei Kalendereinträge-Emulator ausführen CalendarDemo](calendar-images/11-calendar.png)](calendar-images/11-calendar.png)


<a name="Listing_Calendar_Events" />

## <a name="listing-calendar-events"></a>Angebot Kalenderereignissen

Nächste sehen wir uns das Aufzählen der Ereignisse für einen angegebenen Kalender.
Aufbauend auf das obige Beispiel, fügen wir eine Liste der Ereignisse aus, wenn der Benutzer einen Kalender wählt vorhanden. Aus diesem Grund müssen wir die Elementauswahl im vorherigen Code zu behandeln:

```csharp
ListView.ItemClick += (sender, e) => {
    int i = (e as ItemEventArgs).Position;

    cursor.MoveToPosition(i);
    int calId =
        cursor.GetInt (cursor.GetColumnIndex (calendarsProjection [0]));

    var showEvents = new Intent(this, typeof(EventListActivity));
    showEvents.PutExtra("calId", calId);
    StartActivity(showEvents);
};
```

In diesem Code wird eine Absicht, öffnen Sie eine Aktivität des Typs erstellt `EventListActivity`, den Zweck der Kalender-ID übergeben. Wir benötigen die ID der Kalender zur Abfrage nach Ereignissen zu kennen. In der `EventListActivity`des `OnCreate` -Methode, können wir die-ID Abrufen der `Intent` wie unten dargestellt:

```csharp
_calId = Intent.GetIntExtra ("calId", -1);
```

Jetzt sehen wir Abfrageereignisse dafür "" ID Kalender Der Prozess zur Abfrage nach Ereignissen ist ähnlich wie die wir für eine Liste der Kalender früher abgefragt nur diesmal arbeiten wir mit der `CalendarContract.Events` Klasse. Der folgende Code erstellt eine Abfrage zum Abrufen von Ereignissen:

```csharp
var eventsUri = CalendarContract.Events.ContentUri;

string[] eventsProjection = {
    CalendarContract.Events.InterfaceConsts.Id,
    CalendarContract.Events.InterfaceConsts.Title,
    CalendarContract.Events.InterfaceConsts.Dtstart
};

var loader = new CursorLoader(this, eventsUri, eventsProjection,
                   String.Format ("calendar_id={0}", _calId), null, "dtstart ASC");
var cursor = (ICursor)loader.LoadInBackground();
```

In diesem Code wird zuerst Inhalt abrufen `Uri` für Ereignisse aus der `CalendarContract.Events.ContentUri` Eigenschaft. Wir geben Sie dann die Datenspalten, die wir im EventsProjection Array abrufen möchten.
Schließlich wir instanziieren eine `CursorLoader` mit diesen Informationen, und rufen das Ladeprogramm `LoadInBackground` -Methode zur Rückgabe einer `Cursor` mit den Ereignisdaten.

Um Daten für das Ereignis in der Benutzeroberfläche anzuzeigen, können wir Markup und Code, wie wir vor war, um die Liste der Kalender anzuzeigen. Wir verwenden erneut `SimpleCursorAdapter` zum Binden der Daten eine `ListView` wie im folgenden Code gezeigt:

```csharp
string[] sourceColumns = {
    CalendarContract.Events.InterfaceConsts.Title,
    CalendarContract.Events.InterfaceConsts.Dtstart };

int[] targetResources = {
    Resource.Id.eventTitle,
    Resource.Id.eventStartDate };

var adapter = new SimpleCursorAdapter (this, Resource.Layout.EventListItem,
    cursor, sourceColumns, targetResources);

adapter.ViewBinder = new ViewBinder ();       
ListAdapter = adapter;
```

Der Hauptunterschied zwischen diesem Code und den Code, die wir zuvor verwendet, um die Liste der Kalender anzuzeigen, ist die Verwendung von einem `ViewBinder`, die in der Zeile festgelegt wird:

```csharp
adapter.ViewBinder = new ViewBinder ();
```

Die `ViewBinder` -Klasse ermöglicht es uns weiter zu steuern, wie wir die Werte mit Ansichten binden. In diesem Fall nutzen wir sie konvertieren Startzeit des Ereignisses aus Millisekunden in eine Datumszeichenfolge, wie in der folgenden Implementierung dargestellt:

```csharp
class ViewBinder : Java.Lang.Object, SimpleCursorAdapter.IViewBinder
{    
    public bool SetViewValue (View view, Android.Database.ICursor cursor,
        int columnIndex)
    {
        if (columnIndex == 2) {
            long ms = cursor.GetLong (columnIndex);

            DateTime date = new DateTime (1970, 1, 1, 0, 0, 0,
                DateTimeKind.Utc).AddMilliseconds (ms).ToLocalTime ();

            TextView textView = (TextView)view;
            textView.Text = date.ToLongDateString ();

            return true;
        }
        return false;
    }    
}
```

Dadurch wird eine Liste der Ereignisse, wie unten dargestellt:

[![Screenshot der Beispiel-app, die drei Kalenderereignisse anzeigen](calendar-images/12-events.png)](calendar-images/12-events.png)


<a name="Adding_a_Calendar_Event" />

## <a name="adding-a-calendar-event"></a>Hinzufügen eines Kalenderereignisses

Gewusst wie: Lesen von Kalenderdaten kennen gelernt haben. Jetzt sehen wir, wie ein Ereignis auf einen Kalender hinzugefügt. Damit dies funktioniert, werden Sie sicherstellen, dass die `android.permission.WRITE_CALENDAR` Berechtigung, die zuvor erwähnten. Um einen Kalender ein Ereignis hinzugefügt haben, wird Folgendes beschrieben:

1.  Erstellen einer `ContentValues` Instanz.
1.  Verwenden Sie die Schlüssel aus der `CalendarContract.Events.InterfaceConsts` Klasse zum Auffüllen der `ContentValues` Instanz.
1.  Legen Sie die Zeitzonen für die Start- und Endzeiten.
1.  Verwenden einer `ContentResolver` zum Einfügen von Daten für das Ereignis in den Kalender dar.


Der folgende Code veranschaulicht diese Schritte:

```csharp
ContentValues eventValues = new ContentValues ();

eventValues.Put (CalendarContract.Events.InterfaceConsts.CalendarId,
    _calId);
eventValues.Put (CalendarContract.Events.InterfaceConsts.Title,
    "Test Event from M4A");
eventValues.Put (CalendarContract.Events.InterfaceConsts.Description,
    "This is an event created from Xamarin.Android");
eventValues.Put (CalendarContract.Events.InterfaceConsts.Dtstart,
    GetDateTimeMS (2011, 12, 15, 10, 0));
eventValues.Put (CalendarContract.Events.InterfaceConsts.Dtend,
    GetDateTimeMS (2011, 12, 15, 11, 0));

eventValues.Put(CalendarContract.Events.InterfaceConsts.EventTimezone,
    "UTC");
eventValues.Put(CalendarContract.Events.InterfaceConsts.EventEndTimezone,
    "UTC");

var uri = ContentResolver.Insert (CalendarContract.Events.ContentUri,
    eventValues);
```

Beachten Sie, auch wenn wir stellen Sie die Zeitzone, die eine Ausnahme vom Typ keine `Java.Lang.IllegalArgumentException` ausgelöst. Da Ereignis Time-Werten in Millisekunden seit Beginn der Epoche ausgedrückt werden müssen, erstellen wir eine `GetDateTimeMS` -Methode (in `EventListActivity`) unsere Datumsangaben in Millisekunden Format zu konvertieren:

```csharp
long GetDateTimeMS (int yr, int month, int day, int hr, int min)
{
    Calendar c = Calendar.GetInstance (Java.Util.TimeZone.Default);

    c.Set (Java.Util.CalendarField.DayOfMonth, 15);
    c.Set (Java.Util.CalendarField.HourOfDay, hr);
    c.Set (Java.Util.CalendarField.Minute, min);
    c.Set (Java.Util.CalendarField.Month, Calendar.December);
    c.Set (Java.Util.CalendarField.Year, 2011);

    return c.TimeInMillis;
}
```

Wenn wir die Ereignisliste UI eine Schaltfläche hinzu, und führen Sie den oben aufgeführten Code click-Ereignishandler der Schaltfläche wird, wird das Ereignis auf den Kalender hinzugefügt und in unserer Liste aktualisiert wird, wie unten dargestellt:

[![Screenshot der Beispiel-app mit Kalenderereignisse gefolgt von Schaltfläche hinzufügen (Beispielereignis)](calendar-images/13.png)](calendar-images/13.png)

Wenn wir die Kalender-app öffnen, wird es angezeigt, dass das Ereignis gibt es auch geschrieben werden:

[![Screenshot des Kalender-app, die das ausgewählte Ereignis anzeigen](calendar-images/14.png)](calendar-images/14.png)

Wie Sie sehen können, ermöglicht Android leistungsstarke und einfach Zugriff auf abgerufen und Beibehalten von Kalenderdaten, Kalender Funktionen nahtlos integrieren von Anwendungen gestatten.


## <a name="related-links"></a>Verwandte Links

- [Calendar-Demo (Beispiel)](https://developer.xamarin.com/samples/CalendarDemo/)
- [Einführung in Eis Rustikal Sandwich](http://www.android.com/about/ice-cream-sandwich/)
- [Android 4.0 Platform](http://developer.android.com/sdk/android-4.0.html)
