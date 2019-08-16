---
title: Zeitauswahl
description: Auswählen einer Uhrzeit mithilfe von timepickerdialog und dialogfragment
ms.prod: xamarin
ms.assetid: EB4E8206-E8AD-9F04-AC1C-82AC9364A9DD
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: dfee003ba327b199974ae277a93cb1ca55a81b0d
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2019
ms.locfileid: "69522881"
---
# <a name="android-time-picker"></a>Android-Zeitauswahl

Um dem Benutzer die Möglichkeit zu geben, eine Uhrzeit auszuwählen, können Sie [timePicker](xref:Android.Widget.TimePicker)verwenden. Android-Apps verwenden `TimePicker` in der Regel mit [timepickerdialog](xref:Android.App.TimePickerDialog) zum auswählen &ndash; eines Zeitwerts, mit dem eine konsistente Schnittstelle zwischen Geräten und Anwendungen gewährleistet werden kann. `TimePicker`ermöglicht Benutzern das Auswählen der Tageszeit im 24-Stunden-oder 12-Stunden-Modus.
`TimePickerDialog`ist eine Hilfsklasse, die `TimePicker` in einem Dialogfeld kapselt.

[![Beispiel Bildschirm Abbildung des Dialog Felds "Zeitauswahl" in Aktion](time-picker-images/01-example-screen-sml.png)](time-picker-images/01-example-screen.png#lightbox)

## <a name="overview"></a>Übersicht

Moderne Android-Anwendungen zeigen `TimePickerDialog` den in einem [dialogfragment](xref:Android.App.DialogFragment)an. Dies ermöglicht es einer Anwendung, die `TimePicker` als Popup Dialogfeld anzuzeigen oder in eine Aktivität einzubetten. Außerdem `DialogFragment` verwaltet den Lebenszyklus und die Anzeige des Dialog Felds und reduziert so die Menge an Code, die implementiert werden muss.

In dieser Anleitung wird veranschaulicht, wie `TimePickerDialog`die in einem `DialogFragment`umschließenen verwendet wird. In der Beispielanwendung wird `TimePickerDialog` als modales Dialogfeld angezeigt, wenn der Benutzer auf eine Schaltfläche in einer Aktivität klickt. Wenn die Uhrzeit vom Benutzer festgelegt wird, wird das Dialogfeld beendet, und ein `TextView` Handler aktualisiert eine auf dem Aktivitäts Bildschirm mit der ausgewählten Zeit.

## <a name="requirements"></a>Anforderungen

Die Beispielanwendung für dieses Handbuch zielt auf Android 4,1 (API-Ebene) ab.
16) oder höher, aber kann mit Android 3,0 (API-Ebene 11 oder höher) verwendet werden. Es ist möglich, ältere Android-Versionen zu unterstützen, wobei die Android-Unterstützungs Bibliothek V4 zum Projekt hinzugefügt wird und einige Codeänderungen vorgenommen werden.

## <a name="using-the-timepicker"></a>Verwenden der timePicker

Dieses Beispiel erweitert `DialogFragment`; die Unterklassen Implementierung von `DialogFragment` (unten `TimePickerFragment` genannt) und zeigt einen `TimePickerDialog`an. Wenn die Beispiel-App zum ersten Mal gestartet wird, wird die Schaltfläche " `TextView` **Pick Time** " oberhalb eines angezeigt, das zum Anzeigen der ausgewählten Zeit verwendet wird:

[![Bildschirm für erste Beispiel-App](time-picker-images/02-initial-app-screen-sml.png)](time-picker-images/02-initial-app-screen.png#lightbox)

Wenn Sie auf die Schaltfläche " **Pick Time** " klicken, wird `TimePickerDialog` die Beispiel-APP wie in diesem Screenshot gezeigt gestartet:

[![Screenshot des von der APP angezeigten standardmäßigen Zeitauswahl Dialogfelds](time-picker-images/03-am-pm-time-dialog-sml.png)](time-picker-images/03-am-pm-time-dialog.png#lightbox)

Wenn Sie in eine Uhrzeit auswählen und auf die Schaltfläche `TimePickerDialog` `TimePickerDialog` OK klicken, wird die Methode " [iontimesetlistener. ontimeset](xref:Android.App.TimePickerDialog.IOnTimeSetListener.OnTimeSet*)" aufgerufen.
Diese Schnittstelle wird durch das Hosting `DialogFragment` implementiert`TimePickerFragment`(, wie unten beschrieben). Wenn Sie auf die Schaltfläche **Abbrechen** klicken, werden das Fragment und Dialogfeld verworfen.

`DialogFragment`gibt auf eine von drei Arten die ausgewählte Zeit für die hostingaktivität zurück:

1. **Aufrufen einer Methode oder Festlegen einer Eigenschaft** &ndash; Die-Aktivität kann eine Eigenschaft oder Methode bereitstellen, die speziell zum Festlegen dieses Werts vorgesehen ist.

2. **Ereignisse werden angehoben** Der kann ein Ereignis definieren, das ausgelöst wird, `OnTimeSet` wenn aufgerufen wird. `DialogFragment` &ndash;

3. **Mithilfe eines `Action`**  &ndash; kanneine`Action<DateTime>` aufrufen, um die Zeit in der Aktivität anzuzeigen. `DialogFragment` Die-Aktivität stellt beim `Action<DateTime` `DialogFragment`Instanziieren von bereit.

In diesem Beispiel wird das dritte Verfahren verwendet, das erfordert, dass die- `Action<DateTime>` Aktivität einen Handler `DialogFragment`für die bereitstellt.

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

Dabei handelt es sich um ein einfaches [LinearLayout](xref:Android.Widget.LinearLayout) mit einer [TextView](xref:Android.Widget.TextView) , die die Uhrzeit und eine [Schaltfläche](xref:Android.Widget.Button) anzeigt, die das `TimePickerDialog`öffnet. Beachten Sie, dass in diesem Layout hart codierte Zeichen folgen und Dimensionen verwendet werden, damit die APP &ndash; einfacher und leichter zu verstehen ist, wenn eine Produktions-App normalerweise Ressourcen für diese Werte verwendet (wie im Beispiel für das [DatePicker](https://github.com/xamarin/recipes/blob/master/Recipes/android/controls/datepicker/select_a_date/Resources/layout/Main.axml) -Code zu sehen).

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

[![Bildschirm für erste APP](time-picker-images/02-initial-app-screen-sml.png)](time-picker-images/02-initial-app-screen.png#lightbox)

Das Klicken auf die Schaltfläche " **Pick Time** " bewirkt nichts, weil `DialogFragment` noch nicht implementiert wurde, um die `TimePicker`anzuzeigen.
Der nächste Schritt besteht darin, diese `DialogFragment`zu erstellen.

## <a name="extending-dialogfragment"></a>Erweitern von dialogfragment

Um für `DialogFragment` die Verwendung mit `TimePicker`erweitert werden zu können, muss eine Unterklasse erstellt werden, die `DialogFragment` von abgeleitet `TimePickerDialog.IOnTimeSetListener`wird und implementiert. Fügen Sie die folgende Klasse zu **MainActivity.cs**hinzu:

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

Diese `TimePickerFragment` Klasse ist in kleinere Teile aufgeteilt und wird im nächsten Abschnitt erläutert.

### <a name="dialogfragment-implementation"></a>Dialogfragment-Implementierung

`TimePickerFragment`implementiert mehrere Methoden: eine Factorymethode, eine Dialog Feld-Instanziierung `OnTimeSet` -Methode und die `TimePickerDialog.IOnTimeSetListener`Handlermethode, die von benötigt wird.

- `TimePickerFragment`ist eine Unterklasse von `DialogFragment`. Außerdem wird die `TimePickerDialog.IOnTimeSetListener` -Schnittstelle implementiert (d. h., `OnTimeSet` Sie stellt die erforderliche-Methode bereit):

    ```csharp
    public class TimePickerFragment : DialogFragment, TimePickerDialog.IOnTimeSetListener
    ```

- `TAG`wird zu Protokollierungs Zwecken initialisiert (*mytimepickerfragment* kann in beliebige Zeichen folgen geändert werden, die Sie verwenden möchten). Die `timeSelectedHandler` Aktion wird mit einem leeren Delegaten initialisiert, um NULL-Verweis Ausnahmen zu verhindern:

    ```csharp
    public static readonly string TAG = "MyTimePickerFragment";
    Action<DateTime> timeSelectedHandler = delegate { };
    ```

- Die `NewInstance` Factory-Methode wird aufgerufen, um eine neue `TimePickerFragment`zu instanziieren. Diese Methode nimmt einen `Action<DateTime>` Handler an, der aufgerufen wird, wenn der Benutzer auf die Schalt `TimePickerDialog`Fläche **OK** in der klickt:

    ```csharp
    public static TimePickerFragment NewInstance(Action<DateTime> onTimeSelected)
    {
        TimePickerFragment frag = new TimePickerFragment();
        frag.timeSelectedHandler = onTimeSelected;
        return frag;
    }
    ```

- Wenn das Fragment angezeigt werden soll, ruft Android die `DialogFragment` [onkreatedialog](xref:Android.App.DialogFragment.OnCreateDialog*)-Methode auf.
    Diese Methode erstellt ein neues `TimePickerDialog` -Objekt und initialisiert es mit der-Aktivität, dem Rückruf Objekt (bei dem es sich um `TimePickerFragment`die aktuelle Instanz von handelt) und der aktuellen Uhrzeit:

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

- Wenn der Benutzer die Zeiteinstellung im `TimePicker` Dialogfeld ändert, wird die `OnTimeSet` -Methode aufgerufen. `OnTimeSet`erstellt ein `DateTime` -Objekt mit dem aktuellen Datum und den Zusammenführungen in der Zeit (Stunde und Minute), die vom Benutzer ausgewählt wurde:

    ```csharp
    public void OnTimeSet(TimePicker view, int hourOfDay, int minute)
    {
        DateTime currentTime = DateTime.Now;
        DateTime selectedTime = new DateTime(currentTime.Year, currentTime.Month, currentTime.Day, hourOfDay, minute, 0);
    ```


- Dieses `DateTime` Objekt wird an das- `timeSelectedHandler` Objekt, das beim Erstellungs Zeitpunkt beim- `TimePickerFragment` Objekt registriert ist, an das-Objekt `OnTimeSet`Ruft diesen Handler auf, um die Zeitanzeige der Aktivität auf den ausgewählten Zeitpunkt zu aktualisieren (dieser Handler wird im nächsten Abschnitt implementiert):

    ```csharp
    timeSelectedHandler (selectedTime);
    ```

## <a name="displaying-the-timepickerfragment"></a>Anzeigen von timepickerfragment

Nachdem der `DialogFragment` implementiert wurde, ist es an der Zeit, `DialogFragment` mithilfe der `NewInstance` Factory-Methode zu instanziieren und ihn durch Aufrufen von [dialogfragment. Show](xref:Android.App.DialogFragment.Show*)anzuzeigen:

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

Nachdem `TimeSelectOnClick` ein`TimePickerFragment`-Objekt instanziiert wurde, erstellt es einen Delegaten und übergibt ihn an eine anonyme Methode, die die Uhrzeit Anzeige der Aktivität mit dem übergebenen Uhrzeitwert aktualisiert. Schließlich wird das `TimePicker` Dialog Fragment (via `DialogFragment.Show`) gestartet, um das `TimePicker` für den Benutzer anzuzeigen.

Fügen Sie am Ende der `OnCreate` Methode die folgende Zeile hinzu, um den Ereignishandler an die Schaltfläche " **Pick Time** " anzufügen, mit der das Dialogfeld gestartet wird:

```csharp
timeSelectButton.Click += TimeSelectOnClick;
```

Beim Klicken auf die Schaltfläche **Pick Time** wird aufgerufen, `TimeSelectOnClick` um dem Benutzer `TimePicker` das Dialog Fragment anzuzeigen.

## <a name="try-it"></a>Probieren Sie es aus!

Erstellen Sie die App, und führen Sie sie aus. Wenn Sie auf die Schaltfläche " **Pick Time** " klicken, wird das `TimePickerDialog` im Standardzeit Format für die Aktivität angezeigt (in diesem Fall im 12-Stunden-Modus):

[![Das Zeit Dialogfeld wird im am/pm-Modus angezeigt.](time-picker-images/03-am-pm-time-dialog-sml.png)](time-picker-images/03-am-pm-time-dialog.png#lightbox)
   
Wenn Sie im `TimePicker` Dialogfeld auf OK klicken, aktualisiert der Handler die- `TextView` Aktivität mit der ausgewählten Uhrzeit und wird dann beendet:

[![Eine/M-Zeit wird in der TextView-Aktivität angezeigt.](time-picker-images/04-after-time-dialog-sml.png)](time-picker-images/04-after-time-dialog.png#lightbox)

Fügen Sie als nächstes die folgende Codezeile `OnCreateDialog` direkt nach `is24HourFormat` dem deklarierten und Initialisieren von hinzu:

```csharp
is24HourFormat = true;
```

Diese Änderung erzwingt, dass das Flag `TimePickerDialog` , das an den `true` Konstruktor übergeben wird, so lautet, dass anstelle des Zeit Formats der hostingaktivität der 24-Stunden-Modus verwendet wird. Wenn Sie die APP erneut erstellen und ausführen, klicken Sie auf die Schaltfläche **Zeit auswählen** . das `TimePicker` Dialogfeld wird nun im 24-Stunden-Format angezeigt:

[![TimePicker-Dialogfeld im 24-Stunden-Format](time-picker-images/05-24hr-time-dialog-sml.png)](time-picker-images/05-24hr-time-dialog.png#lightbox)

Da der Handler [DateTime. ToShortTimeString](xref:System.DateTime.ToShortDateString*) aufruft, um die Uhrzeit in der der- `TextView`Aktivität auszugeben, wird die Uhrzeit weiterhin im standardmäßigen 12-Stunden-e/a-Format gedruckt.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde erläutert, wie `TimePicker` ein Widget aus einer Android-Aktivität als modales Popup Dialogfeld angezeigt wird. Es wurde eine Beispiel `DialogFragment` Implementierung bereitgestellt und `IOnTimeSetListener` die-Schnittstelle erläutert. In diesem Beispiel wurde auch veranschaulicht `DialogFragment` , wie mit der Host Aktivität interagiert werden kann, um den ausgewählten Zeitpunkt anzuzeigen.

## <a name="related-links"></a>Verwandte Links

- [DialogFragment](xref:Android.App.DialogFragment)
- [TimePicker](xref:Android.Widget.TimePicker)
- [TimePickerDialog](xref:Android.App.TimePickerDialog)
- [TimePickerDialog.IOnTimeSetListener](xref:Android.App.TimePickerDialog.IOnTimeSetListener)
- [Timepickerdemo (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/userinterface-timepickerdemo)
