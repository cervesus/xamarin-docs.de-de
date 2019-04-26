---
title: Kalender
ms.prod: xamarin
ms.assetid: 78666541-CA14-4CD8-A94A-A9621C57813E
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: d8a6044c47c568c7f1e17d01915e2e5a6e888f95
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61278025"
---
# <a name="calendar"></a>Kalender


## <a name="calendar-api"></a>Kalender-API

Ein neuer Satz von Kalender-APIs verwendet, die in Android 4 unterstützt Anwendungen, die zum Lesen oder Schreiben von Daten in der Calendar-Anbieter entwickelt wurden. Diese APIs unterstützen eine Fülle von Optionen für die Interaktion mit Kalenderdaten, einschließlich der Möglichkeit zum Lesen und Schreiben von Ereignissen, Teilnehmer und Erinnerungen an. Mithilfe des Kalender-Anbieters in Ihrer Anwendung zu verwenden, werden die Daten, die Sie über die API hinzufügen in die integrierte Kalender-app angezeigt, die mit Android 4.


## <a name="adding-permissions"></a>Hinzufügen von Berechtigungen

Bei der Arbeit mit den neuen Kalender-APIs in Ihrer Anwendung ist, müssen Sie, zunächst die entsprechenden Berechtigungen das Android-Manifest hinzufügen. Die Berechtigungen, die Sie hinzufügen müssen sind `android.permisson.READ_CALENDAR` und `android.permission.WRITE_CALENDAR`, je nachdem, ob Sie lesen und/oder Schreiben von Kalenderdaten sind.


## <a name="using-the-calendar-contract"></a>Verwendung des Kalender-Vertrags

Nachdem Sie die Berechtigungen festgelegt, Sie können mit interagieren Kalenderdaten mithilfe der `CalendarContract` Klasse. Diese Klasse stellt ein Datenmodell, die Anwendungen verwenden können, wenn eine Interaktion mit dem Kalenderanbieter. Die `CalendarContract` ermöglicht es Anwendungen, die Uris auf der Kalender-Entitäten, z. B. Kalender und Ereignisse zu beheben. Darüber hinaus eine Möglichkeit zur Interaktion mit verschiedenen Felder in den einzelnen Entitäten, z. B. im Kalender Name und ID oder eines Ereignisses die Anfangs- und Enddatum.

Sehen wir uns ein Beispiel, das die Kalender-API verwendet. In diesem Beispiel untersuchen wir Kalender und die zugehörigen Ereignisse aufgelistet werden, wie und wie einem Kalender ein neues Ereignis hinzugefügt.


## <a name="listing-calendars"></a>Kalender auflisten

Zunächst sehen wir uns die Kalender auflisten, die in der Kalender-app registriert wurden. Zu diesem Zweck können wir Instanziieren einer `CursorLoader`. Eingeführt in Android 3.0 (API-11), `CursorLoader` ist die bevorzugte Methode zum Verarbeiten einer `ContentProvider`. Zumindest müssen wir angeben des Inhalts-Uri für Kalender und die Spalten, die wir zurückgeben möchten; die Spezifikation für diese Spalte wird als bezeichnet ein _Projektion_.

Aufrufen der `CursorLoader.LoadInBackground` -Methode ermöglicht es uns Inhaltsanbieter für Daten, z. B. des Kalender-Anbieters abgefragt.
`LoadInBackground` Führt den eigentlichen Ladevorgang ab und gibt eine `Cursor` mit den Ergebnissen der Abfrage.

Die `CalendarContract` hilft uns bei der Angabe von der Content `Uri` und der Projektion. Zum Abrufen des Inhalts `Uri` zum Abfragen von Kalendern, können wir einfach die `CalendarContract.Calendars.ContentUri` Eigenschaft wie folgt:

```csharp
var calendarsUri = CalendarContract.Calendars.ContentUri;
```

Mithilfe der `CalendarContract` sollten Spalten ist genauso einfach auf der Kalender angeben. Wir fügen einfach Felder in der `CalendarContract.Calendars.InterfaceConsts` Klasse, um ein Array. Der folgende Code enthält z. B. der im Kalender-ID, Anzeigename und Kontoname:

```csharp
string[] calendarsProjection = {
    CalendarContract.Calendars.InterfaceConsts.Id,
    CalendarContract.Calendars.InterfaceConsts.CalendarDisplayName,
    CalendarContract.Calendars.InterfaceConsts.AccountName
};
```

Die `Id` ist wichtig, einzuschließen, bei Verwendung einer `SimpleCursorAdapter` um die Daten an die Benutzeroberfläche zu binden, wie wir gleich sehen werden. Der Inhalts-Uri und Projektion vorhanden, wir Instanziieren der `CursorLoader` , und rufen Sie die `CursorLoader.LoadInBackground` Methode, um ein Cursor mit Kalenderdaten zurückzugeben, wie unten dargestellt:

```csharp
var loader = new CursorLoader(this, calendarsUri, calendarsProjection, null, null, null);
var cursor = (ICursor)loader.LoadInBackground();

```

Die Benutzeroberfläche für dieses Beispiel enthält eine `ListView`, wobei die einzelnen Elemente in der Liste, die einen einzelnen Kalender darstellt. Das folgende XML zeigt das Markup mit dem `ListView`:

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

Darüber hinaus müssen wir die Benutzeroberfläche für jedes Listenelement angeben, die wir in einer separaten XML-Datei wie folgt an:

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

Ab diesem Zeitpunkt ist es lediglich um normale Android-Code, um die Daten aus dem Cursor auf die Benutzeroberfläche zu binden. Wir verwenden eine `SimpleCursorAdapter` wie folgt:

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

Im obigen Code wird der Adapter die in angegebenen Spalten der `sourceColumns` array und schreibt sie in der Elemente der Benutzeroberfläche in der `targetResources` Array für jeden Kalendereintrag im Cursor. Die Aktivität, die hier verwendete ist eine Unterklasse von `ListActivity`; er enthält die `ListAdapter` Eigenschaftensatz, der wir den Adapter.

Hier ist dieser Screenshot zeigt das Ergebnis mit den Kalender-Informationen angezeigt, der `ListView`:

[![Ausführen im Emulator, Anzeigen von zwei Kalendereinträgen CalendarDemo](calendar-images/11-calendar.png)](calendar-images/11-calendar.png#lightbox)



## <a name="listing-calendar-events"></a>Termine im Kalender auflisten

Nächste sehen wir uns die Ereignisse für einen angegebenen Kalender auflisten.
Aufbauend auf das obige Beispiel, werden wir eine Liste der Ereignisse, die bei der Auswahl eines anderen vorhanden. Aus diesem Grund müssen wir die Elementauswahl im vorherigen Code behandeln:

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

In diesem Code werden wir eine Absicht, öffnen Sie eine Aktivität vom Typ erstellen `EventListActivity`, der im Kalender-ID in seiner Absicht übergeben. Wir benötigen die ID, die wissen, welcher Kalender zur Abfrage nach Ereignissen. In der `EventListActivity`des `OnCreate` -Methode, können wir die ID von Abrufen der `Intent` wie unten dargestellt:

```csharp
_calId = Intent.GetIntExtra ("calId", -1);
```

Nachdem wir Abfrageereignisse für diese ID Kalender Der Prozess zur Abfrage nach Ereignissen ist ähnlich wie die wir für eine Liste der zuvor Kalender abgefragt, nur diese Zeit, die wir in Zusammenarbeit mit der `CalendarContract.Events` Klasse. Der folgende Code erstellt eine Abfrage zum Abrufen von Ereignissen:

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

In diesem Code wird es zunächst den Inhalt `Uri` für Ereignisse aus dem `CalendarContract.Events.ContentUri` Eigenschaft. Klicken Sie dann geben wir die Datenspalten, die im Array EventsProjection abgerufen werden soll.
Abschließend instanziieren wir eine `CursorLoader` mit diesen Informationen und das Ladeprogramm des Aufrufs `LoadInBackground` -Methode zur Rückgabe einer `Cursor` mit den Ereignisdaten.

Um die Daten in der Benutzeroberfläche anzuzeigen, können wir das Markup und Code nur wie zuvor zum Anzeigen der Liste der Kalender verwenden. In diesem Fall verwenden wir `SimpleCursorAdapter` , die Sie Daten binden ein `ListView` wie im folgenden Code gezeigt:

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

Der Hauptunterschied zwischen diesem Code und den Code, die wir zuvor verwendet, um die Kalenderliste anzuzeigen, ist die Verwendung von einem `ViewBinder`, die in der Zeile festgelegt wird:

```csharp
adapter.ViewBinder = new ViewBinder ();
```

Die `ViewBinder` Klasse kann wir noch weiter zu steuern, wie wir Werte mit Ansichten zu binden. In diesem Fall verwenden wir diese Startzeit des Ereignisses in Millisekunden, eine Datumszeichenfolge zu konvertieren. Siehe die folgende Implementierung:

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

Dies zeigt eine Liste der Ereignisse an, wie unten dargestellt:

[![Screenshot der Beispiel-app, die drei Termine im Kalender anzeigen](calendar-images/12-events.png)](calendar-images/12-events.png#lightbox)



## <a name="adding-a-calendar-event"></a>Hinzufügen eines Kalender-Ereignisses

Wir haben gesehen, wie Kalenderdaten. Jetzt sehen wir uns wie einen Kalender ein Ereignis hinzugefügt. Damit dies funktioniert, müssen Sie die `android.permission.WRITE_CALENDAR` Berechtigung wie bereits erwähnt. Um einen Kalender ein Ereignis hinzugefügt haben, wird Folgendes beschrieben:

1.  Erstellen Sie eine `ContentValues` Instanz.
1.  Verwenden Sie die Schlüssel aus der `CalendarContract.Events.InterfaceConsts` Klasse zum Auffüllen der `ContentValues` Instanz.
1.  Legen Sie die Zeitzonen für die Start- und Endzeiten.
1.  Verwenden einer `ContentResolver` zum Einfügen von Daten für das Ereignis in den Kalender.


Der folgende Code veranschaulicht die folgenden Schritte aus:

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

Beachten Sie, wenn wir stellen Sie die Zeitzone, die eine Ausnahme vom Typ keine `Java.Lang.IllegalArgumentException` ausgelöst. Da Event Zeitwerte in Millisekunden seit Beginn der Epoche ausgedrückt werden müssen, erstellen wir eine `GetDateTimeMS` Methode (in `EventListActivity`) unserer Datum-Spezifikationen in Millisekunden-Format zu konvertieren:

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

Wenn wir die Ereignisliste Benutzeroberfläche eine Schaltfläche hinzu, und führen Sie den obigen Code wird in der Schaltfläche click-Ereignishandler, das Ereignis hinzugefügt dem Kalender und in unserer Liste aktualisiert wird, wie unten dargestellt:

[![Screenshot der Beispiel-app mit Kalenderterminen, gefolgt von der Schaltfläche "Beispiel-Ereignis hinzufügen"](calendar-images/13.png)](calendar-images/13.png#lightbox)

Wenn wir die Kalender-app öffnen, klicken Sie dann sehen wir, dass das Ereignis gibt es auch geschrieben wird:

[![Screenshot des Kalender-app, die das Ereignis anzeigen](calendar-images/14.png)](calendar-images/14.png#lightbox)

Wie Sie sehen können, ermöglicht es Android leistungsstarke und einfachen Zugriff auf das Abrufen und Speichern von Kalenderdaten, ermöglicht es Anwendungen, die Kalender-Funktionen integrieren.


## <a name="related-links"></a>Verwandte Links

- [Kalender-Demo (Beispiel)](https://developer.xamarin.com/samples/CalendarDemo/)
- [Einführung in die Ice Cream Sandwich](http://www.android.com/about/ice-cream-sandwich/)
- [Android 4.0 Platform](https://developer.android.com/sdk/android-4.0.html)
