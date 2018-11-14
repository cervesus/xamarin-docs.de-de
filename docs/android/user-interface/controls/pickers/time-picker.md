---
title: Zeitauswahl
description: Wählen eine Uhrzeit TimePickerDialog und DialogFragment
ms.prod: xamarin
ms.assetid: EB4E8206-E8AD-9F04-AC1C-82AC9364A9DD
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: 08fa9000a1fd9c97f7881a4a13c15fabfa6dda47
ms.sourcegitcommit: 6be6374664cd96a7d924c2e0c37aeec4adf8be13
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/13/2018
ms.locfileid: "51617669"
---
# <a name="time-picker"></a>Zeitauswahl

Um die Möglichkeit für den Benutzer auf eine Uhrzeit auswählen, können Sie [TimePicker](https://developer.xamarin.com/api/type/Android.Widget.TimePicker/). Verwenden Sie die Android-apps in der Regel `TimePicker` mit [TimePickerDialog](https://developer.xamarin.com/api/type/Android.App.TimePickerDialog/) zum Auswählen eines Werts Zeit &ndash; Hiermit können Sie um eine einheitliche Schnittstelle zwischen Geräten und Anwendungen zu gewährleisten. `TimePicker` Wählen Sie die Uhrzeit im 24-Stunden- oder 12-Stunden AM/PM-Modus ermöglicht.
`TimePickerDialog` ist eine Hilfsklasse, die kapselt die `TimePicker` in einem Dialogfeld.

[![Beispielscreenshot, der das Dialogfeld zur Dateiauswahl Zeit in Aktion](time-picker-images/01-example-screen-sml.png)](time-picker-images/01-example-screen.png#lightbox)

## <a name="overview"></a>Übersicht

Anzeigen von modernen Android-Anwendungen die `TimePickerDialog` in einer [DialogFragment](https://developer.xamarin.com/api/type/Android.App.DialogFragment/). Dadurch kann für eine Anwendung zum Anzeigen der `TimePicker` als ein Dialogfenster wird oder Sie betten Sie sie in einer Aktivität. Darüber hinaus die `DialogFragment` verwaltet den Lebenszyklus und die Anzeige des Dialogfelds, verringert die Menge des Codes, die implementiert werden müssen.

Diese Anleitung veranschaulicht, wie Sie mit der `TimePickerDialog`, eingeschlossen in einem `DialogFragment`. Zeigt die Beispiel-Anwendung die `TimePickerDialog` als modales Dialogfeld klickt der Benutzer eine Schaltfläche für eine Aktivität. Wenn die Zeit vom Benutzer festgelegt ist, beendet das Dialogfeld, und ein Ereignishandler aktualisiert eine `TextView` auf dem Bildschirm "Aktivität" mit der Zeit, die ausgewählt wurde.

## <a name="requirements"></a>Anforderungen

Die beispielanwendung für dieses Handbuch ist Android 4.1 (API-Ebene
16) oder höher, ist jedoch mit Android 3.0 (API-Ebene 11 oder höher) verwendet werden kann. Es ist möglich, ältere Versionen von Android durch das Hinzufügen der Bibliothek für Android unterstützt v4 auf das Projekt, und einige Änderungen am Code unterstützen.

## <a name="using-the-timepicker"></a>Verwenden von TimePicker

Dieses Beispiel erweitert `DialogFragment`; die Unterklasse-Implementierung von `DialogFragment` (namens `TimePickerFragment` unten) hostet, und zeigt eine `TimePickerDialog`. Wenn die Beispiel-app zum ersten Mal gestartet wird, zeigt er eine **PICK Zeit** Schaltfläche oben eine `TextView` wird, wird für den ausgewählten Zeitraum anzuzeigen:

[![Anfängliche Beispiel-app-Bildschirm](time-picker-images/02-initial-app-screen-sml.png)](time-picker-images/02-initial-app-screen.png#lightbox)

Beim Klicken auf die **PICK Zeit** Schaltfläche, die Beispiel-app-Startvorgänge der `TimePickerDialog` wie im folgenden Screenshot zu sehen:

[![Screenshot des Dialogfelds für standardmäßige Zeitauswahl angezeigt, die von der app](time-picker-images/03-am-pm-time-dialog-sml.png)](time-picker-images/03-am-pm-time-dialog.png#lightbox)

In der `TimePickerDialog`, eine Uhrzeit auswählen und auf die **OK** Schaltfläche bewirkt, dass die `TimePickerDialog` zum Aufrufen der Methode [IOnTimeSetListener.OnTimeSet](https://developer.xamarin.com/api/member/Android.App.TimePickerDialog+IOnTimeSetListener.OnTimeSet/p/Android.Widget.TimePicker/System.Int32/System.Int32/System.Int32/).
Diese Schnittstelle wird implementiert, indem Sie das Hosten `DialogFragment` (`TimePickerFragment`im folgenden beschriebenen). Klicken auf die **Abbrechen** -Schaltfläche bewirkt, dass die Fragment und das Dialogfeld geschlossen werden.

`DialogFragment` Gibt den ausgewählten Zeitraum an das hosting Actvity in einer von drei Methoden zurück:

1. **Aufrufen einer Methode oder Festlegen einer Eigenschaft** &ndash; bieten die Aktivität, einer Eigenschaft oder Methode speziell für das Festlegen dieses Werts.

2. **Auslösen eines Ereignisses** &ndash; der `DialogFragment` ein definieren, die ausgelöst wird, wenn `OnTimeSet` aufgerufen wird.

3. **Mit einer `Action`**  &ndash; der `DialogFragment` aufrufen können eine `Action<DateTime>` , die Zeit in der Aktivität an. Die Aktivität stellt die `Action<DateTime` beim Instanziieren der `DialogFragment`. 

Dieses Beispiel verwendet der dritten Technik, die erfordert, dass die Aktivität erneut eine `Action<DateTime>` Handler, der die `DialogFragment`.



## <a name="start-an-app-project"></a>Starten Sie ein App-Projekt

Starten Sie ein neues Android-Projekt namens **TimePickerDemo** (Wenn Sie nicht mit dem Erstellen von Xamarin.Android-Projekte vertraut sind, finden Sie unter [Hallo, Android](~/android/get-started/hello-android/hello-android-quickstart.md) zu erfahren, wie Sie ein neues Projekt erstellen).

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

Dies ist eine einfache [LinearLayout](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/) mit einem [TextView](https://developer.xamarin.com/api/type/Android.Widget.TextView/) , das zeigt an, und ein [Schaltfläche](https://developer.xamarin.com/api/type/Android.Widget.Button/) , öffnet der `TimePickerDialog`. Beachten Sie, dass dieses Layout hartcodierte Zeichenfolgen und Dimensionen verwendet, um die app einfacher und leichter zu verstehen, stellen &ndash; eine Produktions-app üblicherweise Ressourcen verwendet, um diese Werte (wie in der angezeigt werden die ["DatePicker"](https://github.com/xamarin/recipes/blob/master/Recipes/android/controls/datepicker/select_a_date/Resources/layout/Main.axml) Codebeispiel).

Bearbeiten Sie **"mainactivity.cs"** und Ersetzen Sie den Inhalt durch den folgenden Code:

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

Wenn Sie erstellen und dieses Beispiel ausführen, sehen Sie einen Startbildschirm, die im folgenden Screenshot ähnelt:

[![Startbildschirm der App](time-picker-images/02-initial-app-screen-sml.png)](time-picker-images/02-initial-app-screen.png#lightbox)

Klicken auf die **PICK Zeit** Schaltfläche führt keine Aktion da der `DialogFragment` wurde noch nicht implementiert zum Anzeigen der `TimePicker`.
Der nächste Schritt ist die Erstellung dieser `DialogFragment`.



## <a name="extending-dialogfragment"></a>Erweitern von DialogFragment

Erweitern `DialogFragment` für die Verwendung mit `TimePicker`, es ist notwendig, eine Unterklasse zu erstellen, das von abgeleitet ist `DialogFragment` und implementiert `TimePickerDialog.IOnTimeSetListener`. Fügen Sie die folgende Klasse **"mainactivity.cs"**:

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

Dies `TimePickerFragment` -Klasse ist in kleinere Teile unterteilt, und im nächsten Abschnitt erläutert.


### <a name="dialogfragment-implementation"></a>DialogFragment-Implementierung

`TimePickerFragment` mehrere Methoden implementiert: eine Factorymethode, die eine Methode zum Dialogfeld Instanziierung, und die `OnTimeSet` Handlermethode, die erforderlichen `TimePickerDialog.IOnTimeSetListener`.

-   `TimePickerFragment` ist eine Unterklasse von `DialogFragment`. Es implementiert auch die `TimePickerDialog.IOnTimeSetListener` Schnittstelle (d. h., gibt er die erforderlichen `OnTimeSet` Methode):

    ```csharp
    public class TimePickerFragment : DialogFragment, TimePickerDialog.IOnTimeSetListener
    ```

-   `TAG` für die Protokollierung initialisiert wird (*MyTimePickerFragment* kann geändert werden, um die Zeichenfolge ein, die Sie verwenden möchten). Die `timeSelectedHandler` Aktion mit einer leeren Delegat, der null-verweisausnahmen zu verhindern, dass initialisiert wird:

    ```csharp
    public static readonly string TAG = "MyTimePickerFragment";
    Action<DateTime> timeSelectedHandler = delegate { };
    ```

-   Die `NewInstance` Factory-Methode wird aufgerufen, um ein neues instanziieren `TimePickerFragment`. Diese Methode akzeptiert eine `Action<DateTime>` Handler, der aufgerufen wird, wenn der Benutzer klickt auf die **OK** Schaltfläche der `TimePickerDialog`:

    ```csharp
    public static TimePickerFragment NewInstance(Action<DateTime> onTimeSelected)
    {
        TimePickerFragment frag = new TimePickerFragment();
        frag.timeSelectedHandler = onTimeSelected;
        return frag;
    }
    ```

-   Wenn das Fragment ist, die angezeigt werden, um Android Ruft die `DialogFragment` Methode [OnCreateDialog](https://developer.xamarin.com/api/member/Android.App.DialogFragment.OnCreateDialog/p/Android.OS.Bundle/). 
    Diese Methode erstellt ein neues `TimePickerDialog` -Objekt und initialisiert sie mit der Aktivität, die das Rückrufobjekt an (Dies ist die aktuelle Instanz von der `TimePickerFragment`), und die aktuelle Zeit:

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

-   Wenn der Benutzer ändert die Einstellung in der `TimePicker` im Dialogfeld die `OnTimeSet` -Methode wird aufgerufen. `OnTimeSet` erstellt eine `DateTime` -Objekt unter Verwendung des aktuellen Systemdatums und Zusammenführungen in der die Uhrzeit (Stunde und Minute), die vom Benutzer ausgewählt wurde:

    ```csharp
    public void OnTimeSet(TimePicker view, int hourOfDay, int minute)
    {
        DateTime currentTime = DateTime.Now;
        DateTime selectedTime = new DateTime(currentTime.Year, currentTime.Month, currentTime.Day, hourOfDay, minute, 0);
    ```


-   Dies `DateTime` Objekt wird zum Übergeben der `timeSelectedHandler` , die registriert wird die `TimePickerFragment` Objekt zum Zeitpunkt der Erstellung. `OnTimeSet` Ruft dieser Handler der Aktivität angezeigt, die auf der ausgewählten Zeit aktualisieren (dieser Handler wird im nächsten Abschnitt implementiert):

    ```csharp
    timeSelectedHandler (selectedTime);
    ```



## <a name="displaying-the-timepickerfragment"></a>Anzeigen der TimePickerFragment

Nun, dass die `DialogFragment` wurde implementiert, ist es Zeit, zu instanziieren der `DialogFragment` mithilfe der `NewInstance` Factorymethode und zeigen Sie es durch Aufrufen von [DialogFragment.Show](https://developer.xamarin.com/api/member/Android.App.DialogFragment.Show/p/Android.App.FragmentManager/System.String/):

Fügen Sie die folgende Methode `MainActivity`:

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

Nach dem `TimeSelectOnClick` instanziiert ein `TimePickerFragment`, erstellt und übergibt einen Delegaten für eine anonyme Methode, die der Aktivität angezeigt mit dem übergebenen-in-Time-Wert aktualisiert. Abschließend startet es die `TimePicker` Dialogfeld Fragment (über `DialogFragment.Show`) zum Anzeigen der `TimePicker` für den Benutzer.

Am Ende der `OnCreate` -Methode, fügen die folgende Zeile zum Anfügen des Ereignishandler, der **PICK Zeit** Schaltfläche, das Dialogfeld gestartet wird:

```csharp
timeSelectButton.Click += TimeSelectOnClick;
```

Bei der **PICK Zeit** Schaltfläche geklickt wird, `TimeSelectOnClick` aufgerufen, um die Anzeige der `TimePicker` Dialogfeld-Fragment, das der Benutzer.



## <a name="try-it"></a>Probieren Sie es aus!

Erstellen Sie die App, und führen Sie sie aus. Beim Klicken auf die **PICK Zeit** Schaltfläche der `TimePickerDialog` wird das Standardformat für die Zeit für die Aktivität (in diesem Fall, 12-Stunden AM/PM-Modus) angezeigt:

[![Time-Dialogfeld wird angezeigt, in der AM/PM-Modus](time-picker-images/03-am-pm-time-dialog-sml.png)](time-picker-images/03-am-pm-time-dialog.png#lightbox)
   
Beim Klicken auf **OK** in die `TimePicker` Dialogfeld den Handler für updates der Aktivitäts `TextView` mit der ausgewählten Zeit und wird dann beendet:

[![Ein/Min. Zeit wird in der TextView-Aktivität angezeigt.](time-picker-images/04-after-time-dialog-sml.png)](time-picker-images/04-after-time-dialog.png#lightbox)

Fügen Sie die folgende Codezeile, `OnCreateDialog` unmittelbar `is24HourFormat` deklariert und initialisiert wird:

```csharp
is24HourFormat = true;
```

Diese Änderung erzwingt, dass das Flag, das die an die `TimePickerDialog` Konstruktor sein `true` damit, 24-Stunden-Modus nicht das Uhrzeitformat der hosting-Aktivität verwendet wird. Wenn Sie erstellen und führen die app erneut aus, klicken Sie auf die **PICK Zeit** Schaltfläche der `TimePicker` Dialogfeld wird jetzt im 24-Stunden-Format angezeigt:

[![TimePicker-Dialogfeld im 24-Stunden-format](time-picker-images/05-24hr-time-dialog-sml.png)](time-picker-images/05-24hr-time-dialog.png#lightbox)

Da der Handler ruft [DateTime.ToShortTimeString](xref:System.DateTime.ToShortDateString*) So drucken Sie die Zeit der Aktivität `TextView`, die Zeit wird immer noch im standardmäßigen 12-Stunden AM/PM-Format ausgegeben.



## <a name="summary"></a>Zusammenfassung

In diesem Artikel erläutert die Vorgehensweise beim Anzeigen einer `TimePicker` Widgets, wie ein modales Popup-Dialogfeld aus einer Android-Aktivität. Sie ein Beispiel bereitgestellt `DialogFragment` Implementierung und erläutert die `IOnTimeSetListener` Schnittstelle. Darüber hinaus wird in diesem Beispiel veranschaulicht die `DialogFragment` Interaktion mit dem Host-Aktivität, um den ausgewählten Zeitpunkt anzuzeigen.


## <a name="related-links"></a>Verwandte Links

- [DialogFragment](https://developer.xamarin.com/api/type/Android.App.DialogFragment/)
- [TimePicker](https://developer.xamarin.com/api/type/Android.Widget.TimePicker/)
- [TimePickerDialog](https://developer.xamarin.com/api/type/Android.App.TimePickerDialog/)
- [TimePickerDialog.IOnTimeSetListener](https://developer.xamarin.com/api/type/Android.App.TimePickerDialog+IOnTimeSetListener/)
- [TimePickerDemo (Beispiel)](https://developer.xamarin.com/samples/monodroid/UserInterface/TimePickerDemo)
