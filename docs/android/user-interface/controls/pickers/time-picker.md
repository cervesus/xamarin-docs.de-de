---
title: / Zeitauswahl
description: Eine Uhrzeit TimePickerDialog und DialogFragment auswählen
ms.prod: xamarin
ms.assetid: EB4E8206-E8AD-9F04-AC1C-82AC9364A9DD
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: c4261e3dccaccc4c88afe9c1033fb16b730fea6e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="time-picker"></a>/ Zeitauswahl

Um die Möglichkeit für den Benutzer auf eine Uhrzeit auswählen, können Sie [TimePicker](https://developer.xamarin.com/api/type/Android.Widget.TimePicker/). Android-apps in der Regel verwenden `TimePicker` mit [TimePickerDialog](https://developer.xamarin.com/api/type/Android.App.TimePickerDialog/) für die Auswahl eines Zeitwerts &ndash; Dadurch wird eine konsistente Oberfläche verschiedene Geräte und Anwendungen sichergestellt. `TimePicker` ermöglicht Benutzern, wählen Sie die Uhrzeit im 24-Stunden-Format oder im 12-Stunden AM/PM-Modus.
`TimePickerDialog` ist eine Hilfsklasse, die kapselt die `TimePicker` in einem Dialogfeld.

[![Beispiel-Screenshot, der die Zeit-Auswahldialogfeld in Aktion](time-picker-images/01-example-screen-sml.png)](time-picker-images/01-example-screen.png#lightbox)

## <a name="overview"></a>Übersicht

Moderne Android-Anwendungen Anzeigen der `TimePickerDialog` in einem [DialogFragment](https://developer.xamarin.com/api/type/Android.App.DialogFragment/). Dies ermöglicht es einer Anwendung zum Anzeigen der `TimePicker` als ein Popupdialogfeld oder in einer Aktivität einbetten. Darüber hinaus die `DialogFragment` verwaltet den Lebenszyklus und die Anzeige des Dialogfelds reduziert die Menge an Code, der implementiert werden muss.

Dieses Handbuch veranschaulicht, wie die `TimePickerDialog`, die eingebunden in eine `DialogFragment`. Zeigt die Beispiel-Anwendung die `TimePickerDialog` als modales Dialogfeld klickt der Benutzer eine Schaltfläche in einer Aktivität. Wenn die Zeit, die vom Benutzer festgelegt ist, der Dialog beendet und ein Ereignishandler aktualisiert eine `TextView` auf dem Bildschirm der Aktivität mit der Zeit, die ausgewählt wurden.

## <a name="requirements"></a>Anforderungen

Die beispielanwendung für diese Anleitung richtet Android 4.1 (API-Ebene
16) oder höher, ist jedoch mit Android 3.0 (API-Ebene 11 oder höher) verwendet werden kann. Es ist möglich, durch das Hinzufügen der Bibliothek für Android unterstützt v4 auf das Projekt und einige Änderungen am Code ältere Versionen von Android unterstützt.

## <a name="using-the-timepicker"></a>Verwenden die TimePicker

In diesem Beispiel erweitert `DialogFragment`; die Unterklasse Implementierung `DialogFragment` (aufgerufen `TimePickerFragment` unten) hostet, und zeigt eine `TimePickerDialog`. Wenn zuerst die Beispiel-app gestartet wird, zeigt er eine **PICK Zeit** Komponentengruppen einer `TextView` , die zum Anzeigen der ausgewählten Uhrzeit verwendet werden:

[![Bildschirm der ersten Beispiel-app](time-picker-images/02-initial-app-screen-sml.png)](time-picker-images/02-initial-app-screen.png#lightbox)

Beim Klicken auf die **PICK Zeit** Schaltfläche, die Beispiel-app gestartet wird die `TimePickerDialog` wie in diesem Screenshot dargestellt:

[![Screenshot des Dialogfelds-Zeitauswahl "Standard" von der app angezeigt wird](time-picker-images/03-am-pm-time-dialog-sml.png)](time-picker-images/03-am-pm-time-dialog.png#lightbox)

In der `TimePickerDialog`, eine Uhrzeit auswählen, und klicken Sie auf die **OK** Schaltfläche Ursachen der `TimePickerDialog` zum Aufrufen der Methode [IOnTimeSetListener.OnTimeSet](https://developer.xamarin.com/api/member/Android.App.TimePickerDialog+IOnTimeSetListener.OnTimeSet/p/Android.Widget.TimePicker/System.Int32/System.Int32/System.Int32/).
Diese Schnittstelle wird implementiert, indem Sie das Hosten `DialogFragment` (`TimePickerFragment`, siehe nachfolgende Ausführungen). Klicken auf die **"Abbrechen"** Schaltfläche bewirkt, dass das Fragment und das Dialogfeld geschlossen werden.

`DialogFragment` Gibt die ausgewählte Zeit auf das hosting Actvity auf drei Arten:

1. **Aufrufen einer Methode oder Festlegen einer Eigenschaft** &ndash; der Aktivität können bereit, einer Eigenschaft oder Methode, die speziell für das Festlegen dieses Werts.

2. **Durch das Auslösen eines Ereignisses** &ndash; der `DialogFragment` können definieren, ein Ereignis, die ausgelöst wird, wenn `OnTimeSet` aufgerufen wird.

3. **Mithilfe einer `Action`**  &ndash; der `DialogFragment` aufrufen kann ein `Action<DateTime>` zeigt den Zeitpunkt in der Aktivität an. Geben Sie die Aktivität wird der `Action<DateTime` bei der Instanziierung der `DialogFragment`. 

In diesem Beispiel wird der dritten Technik, was erfordert, dass die Aktivität erneut verwenden ein `Action<DateTime>` -Handler die `DialogFragment`.



## <a name="start-an-app-project"></a>Starten Sie ein App-Projekt

Starten Sie ein neues Android-Projekt namens **TimePickerDemo** (Wenn Sie nicht mit dem Erstellen von Projekten Xamarin.Android vertraut sind, finden Sie unter [Hello, Android](~/android/get-started/hello-android/hello-android-quickstart.md) Informationen zum Erstellen eines neuen Projekts).

Bearbeiten Sie **Resources/layout/Main.axml** und Ersetzen Sie den Inhalt durch folgendes XML:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:layout_gravity="center_horizontal"
    android:padding="16dp">
    <Button
        android:id="@+id/select_button"
        android:paddingLeft="24dp"
        android:paddingRight="24dp"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="PICK TIME"
        android:textSize="20dp" />
    <TextView
        android:id="@+id/time_display"
        android:layout_height="wrap_content"
        android:layout_width="match_parent"
        android:paddingTop="22dp"
        android:text="Picked time will be displayed here"
        android:textSize="24dp" />
</LinearLayout>
```

Dies ist ein grundlegender [LinearLayout](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/) mit einer [TextView](https://developer.xamarin.com/api/type/Android.Widget.TextView/) , das zeigt an, und ein [Schaltfläche](https://developer.xamarin.com/api/type/Android.Widget.Button/) , öffnet der `TimePickerDialog`. Beachten Sie, dass dieses Layout hartcodierte Zeichenfolgen und Dimensionen verwendet, um die app einfacher und leichter zu verstehen, stellen &ndash; Produktions-app verwendet normalerweise Ressourcen für diese Werte (wie in der [DatePicker](https://github.com/xamarin/recipes/blob/master/android/controls/datepicker/select_a_date/Resources/layout/Main.axml) Codebeispiel).

Bearbeiten Sie **MainActivity.cs** und Ersetzen Sie den Inhalt durch folgenden Code:

```csharp
using Android.App;
using Android.Widget;
using Android.OS;
using System;
using Android.Util;
using Android.Text.Format;

namespace TimePickerDemo
{
    [Activity(Label = "TimePickerDemo", MainLauncher = true, Icon = "@drawable/icon")]
    public class MainActivity : Activity
    {
        TextView timeDisplay;
        Button timeSelectButton;

        protected override void OnCreate(Bundle bundle)
        {
            base.OnCreate(bundle);
            SetContentView(Resource.Layout.Main);
            timeDisplay = FindViewById<TextView>(Resource.Id.time_display);
            timeSelectButton = FindViewById<Button>(Resource.Id.select_button);
        }
    }
}
```

Wenn Sie erstellen und dieses Beispiel ausführen, sehen Sie einen Startbildschirm im folgenden Screenshot ähnelt:

[![App-Startbildschirm](time-picker-images/02-initial-app-screen-sml.png)](time-picker-images/02-initial-app-screen.png#lightbox)

Auf der **PICK Zeit** Schaltfläche führt keine Aktion da die `DialogFragment` wurde noch nicht implementiert zum Anzeigen der `TimePicker`.
Der nächste Schritt ist zum Erstellen dieser `DialogFragment`.



## <a name="extending-dialogfragment"></a>Erweitern von DialogFragment

Erweitern `DialogFragment` für die Verwendung mit `TimePicker`, es ist notwendig, eine Unterklasse erstellen, die abgeleitet ist `DialogFragment` und implementiert `TimePickerDialog.IOnTimeSetListener`. Fügen Sie die folgende Klasse zu **MainActivity.cs**:

```csharp
public class TimePickerFragment : DialogFragment, TimePickerDialog.IOnTimeSetListener
{
    public static readonly string TAG = "MyTimePickerFragment";
    Action<DateTime> timeSelectedHandler = delegate { };

    public static TimePickerFragment NewInstance(Action<DateTime> onTimeSelected)
    {
        TimePickerFragment frag = new TimePickerFragment();
        frag.timeSelectedHandler = onTimeSelected;
        return frag;
    }

    public override Dialog OnCreateDialog (Bundle savedInstanceState)
    {
        DateTime currentTime = DateTime.Now;
        bool is24HourFormat = DateFormat.Is24HourFormat(Activity);
        TimePickerDialog dialog = new TimePickerDialog
            (Activity, this, currentTime.Hour, currentTime.Minute, is24HourFormat);
        return dialog;
    }

    public void OnTimeSet(TimePicker view, int hourOfDay, int minute)
    {
        DateTime currentTime = DateTime.Now;
        DateTime selectedTime = new DateTime(currentTime.Year, currentTime.Month, currentTime.Day, hourOfDay, minute, 0);
        Log.Debug(TAG, selectedTime.ToLongTimeString());
        timeSelectedHandler (selectedTime);
    }
}
```

Dies `TimePickerFragment` Klasse ist unterteilt in kleinere Teile und im nächsten Abschnitt erläutert.


### <a name="dialogfragment-implementation"></a>DialogFragment-Implementierung

`TimePickerFragment` mehrere Methoden implementiert: eine Factorymethode, ein Dialogfeld Instanziierung-Methode, und die `OnTimeSet` Handlermethode erforderlichen `TimePickerDialog.IOnTimeSetListener`.

-   `TimePickerFragment` ist eine Unterklasse von `DialogFragment`. Es implementiert auch die `TimePickerDialog.IOnTimeSetListener` Schnittstelle (bereitstellt, also die erforderlichen `OnTimeSet` Methode):

    ```csharp
    public class TimePickerFragment : DialogFragment, TimePickerDialog.IOnTimeSetListener
    ```

-   `TAG` wird für die Protokollierung initialisiert (*MyTimePickerFragment* kann geändert werden, auf die Zeichenfolge ein, die Sie verwenden möchten). Die `timeSelectedHandler` Aktion mit einer leeren Delegat, der null-verweisausnahmen zu verhindern, dass initialisiert wird:

    ```csharp
    public static readonly string TAG = "MyTimePickerFragment";
    Action<DateTime> timeSelectedHandler = delegate { };
    ```

-   Die `NewInstance` Factory-Methode wird aufgerufen, um ein neues instanziieren `TimePickerFragment`. Diese Methode nimmt ein `Action<DateTime>` Handler, der aufgerufen wird, wenn der Benutzer klickt auf die **OK** Schaltfläche der `TimePickerDialog`:

    ```csharp
    public static TimePickerFragment NewInstance(Action<DateTime> onTimeSelected)
    {
        TimePickerFragment frag = new TimePickerFragment();
        frag.timeSelectedHandler = onTimeSelected;
        return frag;
    }
    ```

-   Wenn das Fragment ist, die angezeigt werden, Android Ruft die `DialogFragment` Methode [OnCreateDialog](https://developer.xamarin.com/api/member/Android.App.DialogFragment.OnCreateDialog/p/Android.OS.Bundle/). 
    Diese Methode erstellt ein neues `TimePickerDialog` -Objekt und initialisiert sie mit der Aktivität, die das Rückrufobjekt (also in die aktuelle Instanz der dem `TimePickerFragment`), und die aktuelle Zeit:

    ```csharp
    public override Dialog OnCreateDialog (Bundle savedInstanceState)
    {
        DateTime currentTime = DateTime.Now;
        bool is24HourFormat = DateFormat.Is24HourFormat(Activity);
        TimePickerDialog dialog = new TimePickerDialog
            (Activity, this, currentTime.Hour, currentTime.Minute, is24HourFormat);
        return dialog;
    }
    ```

-   Wenn der Benutzer ändert die Einstellung in der `TimePicker` im Dialogfeld die `OnTimeSet` Methode aufgerufen wird. `OnTimeSet` erstellt eine `DateTime` -Objekt mit dem aktuellen Datum und Zusammenführungen in der Uhrzeit (Stunde und Minute), die vom Benutzer ausgewählten:

    ```csharp
    public void OnTimeSet(TimePicker view, int hourOfDay, int minute)
    {
        DateTime currentTime = DateTime.Now;
        DateTime selectedTime = new DateTime(currentTime.Year, currentTime.Month, currentTime.Day, hourOfDay, minute, 0);
    ```


-   Dies `DateTime` -Objekt übergeben wird die `timeSelectedHandler` , registriert ist, mit der `TimePickerFragment` Objekt zum Zeitpunkt der Erstellung. `OnTimeSet` Ruft dieser Handler, um die Aktivität Uhrzeitanzeige auf den ausgewählten Zeitpunkt aktualisieren (dieser Handler wird im nächsten Abschnitt implementiert):

    ```csharp
    timeSelectedHandler (selectedTime);
    ```



## <a name="displaying-the-timepickerfragment"></a>Anzeigen der TimePickerFragment

Nun, dass die `DialogFragment` wurde implementiert, ist es Zeit, die zum Instanziieren der `DialogFragment` mithilfe der `NewInstance` Factorymethode und zeigen Sie es durch den Aufruf [DialogFragment.Show](https://developer.xamarin.com/api/member/Android.App.DialogFragment.Show/p/Android.App.FragmentManager/System.String/):

Fügen Sie die folgende Methode hinzu `MainActivity`:

```csharp
void TimeSelectOnClick (object sender, EventArgs eventArgs)
{
    TimePickerFragment frag = TimePickerFragment.NewInstance (
        delegate (DateTime time)
        {
            timeDisplay.Text = time.ToShortTimeString();
        });

    frag.Show(FragmentManager, TimePickerFragment.TAG);
}
```

Nach dem `TimeSelectOnClick` instanziiert einen `TimePickerFragment`, erstellt und übergibt einen Delegaten für eine anonyme Methode, die die Aktivität Uhrzeitanzeige mit dem übergebenen-in-Time-Wert aktualisiert. Schließlich gestartet der `TimePicker` Dialogfeld Fragment (über `DialogFragment.Show`) zum Anzeigen der `TimePicker` für den Benutzer.

Am Ende der `OnCreate` -Methode, fügen die folgende Zeile ein, um den Ereignishandler zum Anfügen der **PICK Zeit** Schaltfläche, der den Dialog startet:

```csharp
timeSelectButton.Click += TimeSelectOnClick;
```

Wenn die **PICK Zeit** Schaltfläche geklickt wird, `TimeSelectOnClick` wird aufgerufen, die zum Anzeigen der `TimePicker` Dialogfeld Fragment an den Benutzer.



## <a name="try-it"></a>Probieren Sie es aus!

Erstellen Sie die App, und führen Sie sie aus. Beim Klicken auf die **PICK Zeit** Schaltfläche, die `TimePickerDialog` wird in das Standardzeitformat angezeigt, für die Aktivität (in diesem Groß-/Kleinschreibung, 12 Stunden AM/PM-Modus):

[![In AM/PM-Modus wird die Zeit-Dialogfeld angezeigt.](time-picker-images/03-am-pm-time-dialog-sml.png)](time-picker-images/03-am-pm-time-dialog.png#lightbox)
   
Beim Klicken auf **OK** in der `TimePicker` im Dialogfeld der Handler aktualisiert der Aktivitätssymbols `TextView` mit den ausgewählten Zeitpunkt und wird dann beendet:

[![A/M Zeit wird in der Aktivität TextView angezeigt.](time-picker-images/04-after-time-dialog-sml.png)](time-picker-images/04-after-time-dialog.png#lightbox)

Fügen Sie die folgende Zeile des Codes für `OnCreateDialog` sofort nach dem `is24HourFormat` deklariert und initialisiert wird:

```csharp
is24HourFormat = true;
```

Diese Änderung erzwingt, dass das Flag, das an die `TimePickerDialog` Konstruktor werden `true` damit, 24-Stunden-Modus nicht das Uhrzeitformat der hosting-Aktivität verwendet wird. Wenn Sie erstellen und führen die app erneut aus, klicken Sie auf die **PICK Zeit** Schaltfläche, die `TimePicker` wird jetzt im 24-Stunden-Format angezeigt:

[![TimePicker Dialogfeld im 24-Stunden-format](time-picker-images/05-24hr-time-dialog-sml.png)](time-picker-images/05-24hr-time-dialog.png#lightbox)

Da der Handler aufruft [DateTime.ToShortTimeString](https://msdn.microsoft.com/en-us/library/system.datetime.toshortdatestring%28v=vs.110%29.aspx) So drucken Sie die Zeit für der Aktivitätssymbols `TextView`, die Zeit wird im 12-Stunden-AM/PM-Standardformat noch gedruckt.



## <a name="summary"></a>Zusammenfassung

In diesem Artikel wird erläutert, wie zum Anzeigen einer `TimePicker` Widget als Popup modales Dialogfeld aus einer Android-Aktivität. Sie ein Beispiel bereitgestellt `DialogFragment` Implementierung und erläutert die `IOnTimeSetListener` Schnittstelle. In diesem Beispiel wurde auch veranschaulicht wie die `DialogFragment` können interagieren mit dem Host-Aktivität, um den ausgewählten Zeitpunkt anzuzeigen.


## <a name="related-links"></a>Verwandte Links

- [DialogFragment](https://developer.xamarin.com/api/type/Android.App.DialogFragment/)
- [TimePicker](https://developer.xamarin.com/api/type/Android.Widget.TimePicker/)
- [TimePickerDialog](https://developer.xamarin.com/api/type/Android.App.TimePickerDialog/)
- [TimePickerDialog.IOnTimeSetListener](https://developer.xamarin.com/api/type/Android.App.TimePickerDialog+IOnTimeSetListener/)
- [TimePickerDemo (Beispiel)](https://developer.xamarin.com/samples/monodroid/UserInterface/TimePickerDemo)
