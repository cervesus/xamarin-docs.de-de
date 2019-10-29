---
title: Zeitauswahl
description: Auswählen einer Uhrzeit mithilfe von timepickerdialog und dialogfragment
ms.prod: xamarin
ms.assetid: EB4E8206-E8AD-9F04-AC1C-82AC9364A9DD
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/06/2018
ms.openlocfilehash: 37dd57b0f3264ed25d0a53632f312d30761f747b
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029202"
---
# <a name="android-time-picker"></a>Android-Zeitauswahl

Um dem Benutzer die Möglichkeit zu geben, eine Uhrzeit auszuwählen, können Sie [timePicker](xref:Android.Widget.TimePicker)verwenden. Android-Apps verwenden in der Regel `TimePicker` mit [timepickerdialog](xref:Android.App.TimePickerDialog) zum Auswählen eines Zeit Werts &ndash; dadurch wird sichergestellt, dass eine konsistente Schnittstelle zwischen Geräten und Anwendungen gewährleistet ist. mit `TimePicker` können Benutzer die Uhrzeit im 24-Stunden-oder 12-Stunden-Modus auswählen.
`TimePickerDialog` ist eine Hilfsklasse, die die `TimePicker` in einem Dialogfeld kapselt.

[![Beispiel-Screenshot des Dialog Felds "Zeitauswahl" in Aktion](time-picker-images/01-example-screen-sml.png)](time-picker-images/01-example-screen.png#lightbox)

## <a name="overview"></a>Übersicht

Moderne Android-Anwendungen zeigen die `TimePickerDialog` in einem [dialogfragment](xref:Android.App.DialogFragment)an. Dies ermöglicht es einer Anwendung, die `TimePicker` als Popup Dialogfeld anzuzeigen oder in eine Aktivität einzubetten. Außerdem verwaltet der `DialogFragment` den Lebenszyklus und die Anzeige des Dialog Felds, wodurch die zu implementierende Code Menge reduziert wird.

In dieser Anleitung wird veranschaulicht, wie die `TimePickerDialog`in einem `DialogFragment`verwendet wird. Die Beispielanwendung zeigt die `TimePickerDialog` als modales Dialogfeld an, wenn der Benutzer auf eine Schaltfläche in einer Aktivität klickt. Wenn die Uhrzeit vom Benutzer festgelegt wird, wird das Dialogfeld beendet, und ein Handler aktualisiert eine `TextView` auf dem Aktivitäts Bildschirm mit der ausgewählten Zeit.

## <a name="requirements"></a>Anforderungen

Die Beispielanwendung für dieses Handbuch zielt auf Android 4,1 (API-Ebene) ab.
16) oder höher, aber kann mit Android 3,0 (API-Ebene 11 oder höher) verwendet werden. Es ist möglich, ältere Android-Versionen zu unterstützen, wobei die Android-Unterstützungs Bibliothek V4 zum Projekt hinzugefügt wird und einige Codeänderungen vorgenommen werden.

## <a name="using-the-timepicker"></a>Verwenden der timePicker

In diesem Beispiel wird `DialogFragment`erweitert. die Unterklassen Implementierung von `DialogFragment` (`TimePickerFragment` unten genannt) hostet eine `TimePickerDialog`und zeigt diese an. Wenn die Beispiel-App zum ersten Mal gestartet wird, wird eine Schaltfläche zum **Auswählen der Zeit** oberhalb einer `TextView` angezeigt, die zum Anzeigen der ausgewählten Zeit verwendet wird:

[Bildschirm "![-Beispiel-App](time-picker-images/02-initial-app-screen-sml.png)](time-picker-images/02-initial-app-screen.png#lightbox)

Wenn Sie auf die Schaltfläche " **Pick Time** " klicken, wird die `TimePickerDialog` in der Beispiel-APP wie in diesem Screenshot gezeigt gestartet:

[![Screenshot des Standardzeit Auswahl-Dialog Felds, das von der App angezeigt wird](time-picker-images/03-am-pm-time-dialog-sml.png)](time-picker-images/03-am-pm-time-dialog.png#lightbox)

Wenn Sie in der `TimePickerDialog`eine Uhrzeit auswählen und auf die Schaltfläche **OK** klicken, wird der `TimePickerDialog` die Methode " [iontimesetlistener. ontimeset](xref:Android.App.TimePickerDialog.IOnTimeSetListener.OnTimeSet*)" aufruft.
Diese Schnittstelle wird vom Hosting`DialogFragment` implementiert (`TimePickerFragment`, wie unten beschrieben). Wenn Sie auf die Schaltfläche **Abbrechen** klicken, werden das Fragment und Dialogfeld verworfen.

`DialogFragment` gibt den ausgewählten Zeitpunkt auf eine von drei Arten an die hostingaktivität zurück:

1. Das **Aufrufen einer Methode oder das Festlegen einer Eigenschaft** &ndash; die Aktivität kann eine Eigenschaft oder eine Methode bereitstellen, die speziell zum Festlegen dieses Werts vorgesehen ist.

2. Das Aufrufen **eines Ereignisses** &ndash; der `DialogFragment` kann ein Ereignis definieren, das beim Aufrufen `OnTimeSet` ausgelöst wird.

3. **Mithilfe eines `Action`** &ndash; die `DialogFragment` einen `Action<DateTime>` aufrufen können, um die Zeit in der Aktivität anzuzeigen. Die-Aktivität stellt beim Instanziieren der `DialogFragment`die `Action<DateTime` bereit.

In diesem Beispiel wird das dritte Verfahren verwendet, das erfordert, dass die-Aktivität einen `Action<DateTime>` Handler für die `DialogFragment`bereitstellt.

## <a name="start-an-app-project"></a>Starten eines App-Projekts

Starten Sie ein neues Android-Projekt mit dem Namen **timepickerdemo** (wenn Sie mit dem Erstellen von xamarin. Android-Projekten nicht vertraut sind, lesen Sie [Hello, Android](~/android/get-started/hello-android/hello-android-quickstart.md) , um zu erfahren, wie Sie ein neues Projekt erstellen).

Bearbeiten Sie **Resources/Layout/Main. axml** , und ersetzen Sie den Inhalt durch den folgenden XML-Code:

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

Dabei handelt es sich um ein einfaches [LinearLayout](xref:Android.Widget.LinearLayout) mit einer [TextView](xref:Android.Widget.TextView) , die die Uhrzeit und eine [Schaltfläche](xref:Android.Widget.Button) anzeigt, die die `TimePickerDialog`öffnet. Beachten Sie, dass in diesem Layout hart codierte Zeichen folgen und Dimensionen verwendet werden, damit die APP einfacher und leichter zu verstehen ist, &ndash; eine Produktions-App normalerweise Ressourcen für diese Werte verwendet (wie im Beispiel für das [DatePicker](https://github.com/xamarin/recipes/blob/master/Recipes/android/controls/datepicker/select_a_date/Resources/layout/Main.axml) -Code zu sehen).

Bearbeiten Sie **MainActivity.cs** , und ersetzen Sie den Inhalt durch den folgenden Code:

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

Wenn Sie dieses Beispiel erstellen und ausführen, sollte ein ursprünglicher Bildschirm angezeigt werden, der dem folgenden Screenshot ähnelt:

[Bildschirm zum![der ersten App](time-picker-images/02-initial-app-screen-sml.png)](time-picker-images/02-initial-app-screen.png#lightbox)

Das Klicken auf die Schaltfläche " **Pick Time** " bewirkt nichts, weil der `DialogFragment` noch nicht implementiert wurde, um die `TimePicker`anzuzeigen.
Der nächste Schritt besteht darin, diesen `DialogFragment`zu erstellen.

## <a name="extending-dialogfragment"></a>Erweitern von dialogfragment

Um `DialogFragment` für die Verwendung mit `TimePicker`zu erweitern, muss eine Unterklasse erstellt werden, die von `DialogFragment` abgeleitet ist und `TimePickerDialog.IOnTimeSetListener`implementiert. Fügen Sie die folgende Klasse zu **MainActivity.cs**hinzu:

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

Diese `TimePickerFragment` Klasse wird in kleinere Teile aufgeteilt und im nächsten Abschnitt erläutert.

### <a name="dialogfragment-implementation"></a>Dialogfragment-Implementierung

`TimePickerFragment` implementiert mehrere Methoden: eine Factorymethode, eine Dialog Feld-Instanziierung-Methode und die für `TimePickerDialog.IOnTimeSetListener`erforderliche `OnTimeSet` Handlermethode.

- `TimePickerFragment` ist eine Unterklasse von `DialogFragment`. Außerdem wird die `TimePickerDialog.IOnTimeSetListener`-Schnittstelle implementiert (d. h., Sie stellt die erforderliche `OnTimeSet`-Methode bereit):

    ```csharp
    public class TimePickerFragment : DialogFragment, TimePickerDialog.IOnTimeSetListener
    ```

- `TAG` wird zu Protokollierungs Zwecken initialisiert (*mytimepickerfragment* kann in beliebige Zeichen folgen geändert werden, die Sie verwenden möchten). Die `timeSelectedHandler` Aktion wird mit einem leeren Delegaten initialisiert, um NULL-Verweis Ausnahmen zu verhindern:

    ```csharp
    public static readonly string TAG = "MyTimePickerFragment";
    Action<DateTime> timeSelectedHandler = delegate { };
    ```

- Die `NewInstance` Factory-Methode wird aufgerufen, um eine neue `TimePickerFragment`zu instanziieren. Diese Methode nimmt einen `Action<DateTime>` Handler an, der aufgerufen wird, wenn der Benutzer auf die Schaltfläche **OK** im `TimePickerDialog`klickt:

    ```csharp
    public static TimePickerFragment NewInstance(Action<DateTime> onTimeSelected)
    {
        TimePickerFragment frag = new TimePickerFragment();
        frag.timeSelectedHandler = onTimeSelected;
        return frag;
    }
    ```

- Wenn das Fragment angezeigt werden soll, ruft Android die `DialogFragment` [onkreatedialog](xref:Android.App.DialogFragment.OnCreateDialog*)-Methode auf.
    Diese Methode erstellt ein neues `TimePickerDialog`-Objekt und initialisiert es mit der-Aktivität, dem Rückruf Objekt (bei dem es sich um die aktuelle Instanz des `TimePickerFragment`handelt) und der aktuellen Uhrzeit:

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

- Wenn der Benutzer die Zeiteinstellung im Dialogfeld "`TimePicker`" ändert, wird die `OnTimeSet`-Methode aufgerufen. `OnTimeSet` erstellt mithilfe des aktuellen Datums und der Zusammenführungen in der vom Benutzer ausgewählten Zeit (Stunde und Minute) ein `DateTime`-Objekt:

    ```csharp
    public void OnTimeSet(TimePicker view, int hourOfDay, int minute)
    {
        DateTime currentTime = DateTime.Now;
        DateTime selectedTime = new DateTime(currentTime.Year, currentTime.Month, currentTime.Day, hourOfDay, minute, 0);
    ```

- Dieses `DateTime` Objekt wird an die `timeSelectedHandler`, die beim Erstellungs Zeitpunkt beim `TimePickerFragment`-Objekt registriert ist, an die übermittelt. `OnTimeSet` ruft diesen Handler auf, um die Zeitanzeige der Aktivität auf den ausgewählten Zeitpunkt zu aktualisieren (dieser Handler wird im nächsten Abschnitt implementiert):

    ```csharp
    timeSelectedHandler (selectedTime);
    ```

## <a name="displaying-the-timepickerfragment"></a>Anzeigen von timepickerfragment

Nachdem der `DialogFragment` implementiert wurde, ist es Zeit, die `DialogFragment` mithilfe der `NewInstance` Factory-Methode zu instanziieren und durch Aufrufen von [dialogfragment. Show](xref:Android.App.DialogFragment.Show*)anzuzeigen:

Fügen Sie `MainActivity` zur folgenden Methode hinzu:

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

Nachdem `TimeSelectOnClick` eine `TimePickerFragment`instanziiert hat, erstellt Sie einen Delegaten und übergibt ihn an eine anonyme Methode, die die Uhrzeit Anzeige der Aktivität mit dem übergebenen Uhrzeitwert aktualisiert. Schließlich wird das `TimePicker` Dialog Fragment (über `DialogFragment.Show`) gestartet, um dem Benutzer die `TimePicker` anzuzeigen.

Fügen Sie am Ende der `OnCreate`-Methode die folgende Zeile hinzu, um den Ereignishandler an die Schaltfläche " **Pick Time** " anzufügen, die das Dialogfeld öffnet:

```csharp
timeSelectButton.Click += TimeSelectOnClick;
```

Wenn Sie auf die Schaltfläche " **Pick Time** " klicken, wird `TimeSelectOnClick` aufgerufen, um dem Benutzer das `TimePicker` Dialog Fragment anzuzeigen.

## <a name="try-it"></a>Probieren Sie es aus!

Erstellen Sie die App, und führen Sie sie aus. Wenn Sie auf die Schaltfläche " **Pick Time** " klicken, wird der `TimePickerDialog` im Standardzeit Format für die Aktivität angezeigt (in diesem Fall im 12-Stunden-Modus):

[![Zeit Dialogfeld wird im am/pm-Modus angezeigt.](time-picker-images/03-am-pm-time-dialog-sml.png)](time-picker-images/03-am-pm-time-dialog.png#lightbox)
   
Wenn Sie im Dialogfeld "`TimePicker`" auf " **OK** " klicken, aktualisiert der Handler die `TextView` der Aktivität mit der ausgewählten Uhrzeit und wird dann beendet:

[![A/M-Zeit wird in der Aktivitäts textsicht angezeigt.](time-picker-images/04-after-time-dialog-sml.png)](time-picker-images/04-after-time-dialog.png#lightbox)

Fügen Sie als nächstes die folgende Codezeile hinzu, um sofort zu `OnCreateDialog`, nachdem `is24HourFormat` deklariert und initialisiert wurde:

```csharp
is24HourFormat = true;
```

Diese Änderung zwingt, dass das Flag, das an den `TimePickerDialog`-Konstruktor übergeben wird, `true` wird, damit anstelle des Zeit Formats der hostingaktivität der 24-Stunden-Modus verwendet wird. Wenn Sie die APP erneut erstellen und ausführen, klicken Sie auf die Schaltfläche **Zeit auswählen** . das Dialogfeld `TimePicker` wird nun im 24-Stunden-Format angezeigt:

[![timePicker-Dialogfeld im 24-Stunden-Format](time-picker-images/05-24hr-time-dialog-sml.png)](time-picker-images/05-24hr-time-dialog.png#lightbox)

Da der Handler [DateTime. ToShortTimeString](xref:System.DateTime.ToShortDateString*) aufruft, um die Uhrzeit auf das `TextView`der Aktivität auszugeben, wird die Uhrzeit weiterhin im standardmäßigen 12-Stunden-Format (UTC) gedruckt.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde erläutert, wie ein `TimePicker`-Widget als modales Popup Dialogfeld aus einer Android-Aktivität angezeigt wird. Es wurde eine Beispiel `DialogFragment` Implementierung bereitgestellt, und es wurde die `IOnTimeSetListener` Schnittstelle erläutert. In diesem Beispiel wurde auch veranschaulicht, wie die `DialogFragment` mit der Host Aktivität interagieren kann, um den ausgewählten Zeitpunkt anzuzeigen.

## <a name="related-links"></a>Verwandte Links

- [Dialogfragment](xref:Android.App.DialogFragment)
- [TimePicker](xref:Android.Widget.TimePicker)
- [Timepickerdialog](xref:Android.App.TimePickerDialog)
- [Timepickerdialog. iontimesetlistener](xref:Android.App.TimePickerDialog.IOnTimeSetListener)
- [Timepickerdemo (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/userinterface-timepickerdemo)
