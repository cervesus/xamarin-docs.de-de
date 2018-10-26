---
title: Datumsauswahl
description: Auswählen von Kalenderdaten, die mit dem DatePickerDialog und DialogFragment
ms.prod: xamarin
ms.assetid: F2BCD8D4-8957-EA53-C5A8-6BB603ADB47B
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 01/22/2018
ms.openlocfilehash: 9f82317f6041de3952d11b391afffafe6fbd8761
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50122971"
---
# <a name="date-picker"></a>Datumsauswahl

## <a name="overview"></a>Übersicht

Es gibt Situationen, wenn ein Benutzer die Daten in einer Android-Anwendung eingeben muss. Um dabei zu helfen, das Android-Framework bietet die [ `DatePicker` ](https://developer.xamarin.com/api/type/Android.Widget.DatePicker/) Widget und [ `DatePickerDialog` ](https://developer.xamarin.com/api/type/Android.App.DatePickerDialog/) . Die `DatePicker` ermöglicht es Benutzern, wählen Sie das Jahr, Monat und Tag in eine einheitliche Schnittstelle für Geräte und Anwendungen. Die `DatePickerDialog` ist eine Hilfsklasse, die kapselt die `DatePicker` in einem Dialogfeld.

Modernen Android-Anwendungen sollten angezeigt werden. die `DatePickerDialog` in einem [ `DialogFragment` ](https://developer.xamarin.com/api/type/Android.App.DialogFragment/). Dies kann eine Anwendung zum Anzeigen von "DatePicker" als ein Popupdialogfeld oder in einer Aktivität eingebettet. Darüber hinaus die `DialogFragment` verwaltet den Lebenszyklus und die Anzeige des Dialogfelds, verringert die Menge des Codes, die implementiert werden müssen.

Dieses Handbuch zeigen, wie verwenden Sie die `DatePickerDialog`, eingeschlossen in einem `DialogFragment`. Die beispielanwendung zeigt die `DatePickerDialog` als modales Dialogfeld klickt der Benutzer eine Schaltfläche für eine Aktivität. Wenn das Datum vom Benutzer festgelegt ist eine `TextView` wird aktualisiert, mit dem Datum, das ausgewählt wurde.

[![Screenshot des Datum auswählen-Schaltfläche und dann Datumsauswahl-Dialogfeld](date-picker-images/image-01-sml.png)](date-picker-images/image-01.png#lightbox)

## <a name="requirements"></a>Anforderungen

Die beispielanwendung für dieses Handbuch ist Android 4.1 (API-Ebene
16) oder höher, sondern gilt für Android 3.0 (API-Ebene 11 oder höher). Es ist möglich, ältere Versionen von Android durch das Hinzufügen der Bibliothek für Android unterstützt v4 auf das Projekt, und einige Änderungen am Code unterstützen.

## <a name="using-the-datepicker"></a>Mithilfe von "DatePicker"

Dieses Beispiel erweitern `DialogFragment`. Die Unterklasse gehostet wird, und zeigt eine `DatePickerDialog`:

![Nahaufnahme der Datumsauswahl-Dialogfeld](date-picker-images/image-02.png)

Wenn der Benutzer wählt ein Datum und die auf die **OK** Schaltfläche der `DatePickerDialog` rufen Sie die Methode [ `IOnDateSetListener.OnDateSet` ](https://developer.xamarin.com/api/member/Android.App.DatePickerDialog+IOnDateSetListener.OnDateSet/p/Android.Widget.DatePicker/System.Int32/System.Int32/System.Int32/).
Diese Schnittstelle wird implementiert, indem Sie das Hosten `DialogFragment`. Wenn der Benutzer klickt auf die **Abbrechen** Schaltfläche, und dann das Dialogfeld und Fragment selbst schließt.

Es gibt mehrere Möglichkeiten der `DialogFragment` kann das ausgewählte Datum zurückgeben, mit der hosting-Aktivität:

1. **Aufrufen einer Methode oder eine Eigenschaft festlegen,** &ndash; bieten die Aktivität, einer Eigenschaft oder Methode speziell für das Festlegen dieses Werts.

2. **Auslösen eines Ereignisses** &ndash; der `DialogFragment` ein definieren, die ausgelöst wird, wenn `OnDateSet` aufgerufen.

3. **Verwenden einer `Action`**  &ndash; der `DialogFragment` Aufrufen einer `Action<DateTime>` zum Anzeigen des Datums in der Aktivität. Die Aktivität stellt die `Action<DateTime` beim Instanziieren der `DialogFragment`. In diesem Beispiel wird die dritte Technik verwenden und verlangen, dass die Aktivität angeben einer `Action<DateTime>` auf die `DialogFragment`.



### <a name="extending-dialogfragment"></a>Erweitern von DialogFragment

Der erste Schritt bei der Anzeige einer `DatePickerDialog` besteht darin, Unterklasse `DialogFragment` und lassen Sie sie implementieren die `IOnDateSetListener` Schnittstelle:

```csharp
public class DatePickerFragment : DialogFragment, 
                                  DatePickerDialog.IOnDateSetListener
{
    // TAG can be any string of your choice.
    public static readonly string TAG = "X:" + typeof (DatePickerFragment).Name.ToUpper();
    
    // Initialize this value to prevent NullReferenceExceptions.
    Action<DateTime> _dateSelectedHandler = delegate { };
    
    public static DatePickerFragment NewInstance(Action<DateTime> onDateSelected)
    {
        DatePickerFragment frag = new DatePickerFragment();
        frag._dateSelectedHandler = onDateSelected;
        return frag;
    }
    
    public override Dialog OnCreateDialog(Bundle savedInstanceState)
    {
        DateTime currently = DateTime.Now;
        DatePickerDialog dialog = new DatePickerDialog(Activity, 
                                                       this, 
                                                       currently.Year, 
                                                       currently.Month - 1,
                                                       currently.Day);
        return dialog;
    }
    
    public void OnDateSet(DatePicker view, int year, int monthOfYear, int dayOfMonth)
    {
        // Note: monthOfYear is a value between 0 and 11, not 1 and 12!
        DateTime selectedDate = new DateTime(year, monthOfYear + 1, dayOfMonth);
        Log.Debug(TAG, selectedDate.ToLongDateString());
        _dateSelectedHandler(selectedDate);
    }
}
```

Die `NewInstance` Methode wird aufgerufen, um ein neues instanziieren `DatePickerFragment`. Diese Methode akzeptiert eine `Action<DateTime>` wird, die aufgerufen werden, klickt der Benutzer auf die **OK** Schaltfläche der `DatePickerDialog`.

Wenn das Fragment ist, die angezeigt werden, wird die Methode von Android aufgerufen `OnCreateDialog`. Diese Methode erstellt eine neue `DatePickerDialog` Objekt, und initialisieren Sie es mit dem aktuellen Datum und die Callback-Objekt (d.h., dass die aktuelle Instanz von der `DatePickerFragment`).


> [!NOTE]
> Beachten Sie, den Wert des Monats bei `IOnDateSetListener.OnDateSet` wird aufgerufen, wird im Bereich von 0 bis 11, und nicht von 1 bis 12. Der Tag des Monats werden im Bereich von 1 bis 31 (je nachdem, welche Monat ausgewählt wurde).



### <a name="showing-the-datepickerfragment"></a>Zeigt die DatePickerFragment

Nachdem die `DialogFragment` wurde implementiert, wird in diesem Abschnitt verwenden Sie das Fragment in einer Aktivität überprüfen. In der Beispiel-app, die diesem Handbuch beigefügt ist, wird die Aktivität instanziiert die `DialogFragment` mithilfe der `NewInstance` -Factorymethode, und klicken Sie dann anzeigen, die sie aufrufen `DialogFragment.Show`. Als Teil der Instanziierung der `DialogFragment`, die Aktivität übergibt eine `Action<DateTime>`, das zeigt des Datums im eine `TextView` , die von der Aktivität gehostet wird:

```csharp
[Activity(Label = "@string/app_name", MainLauncher = true, Icon = "@drawable/icon")]
public class MainActivity : Activity
{
    TextView _dateDisplay;
    Button _dateSelectButton;

    protected override void OnCreate(Bundle bundle)
    {
        base.OnCreate(bundle);
        SetContentView(Resource.Layout.Main);

        _dateDisplay = FindViewById<TextView>(Resource.Id.date_display);
        _dateSelectButton = FindViewById<Button>(Resource.Id.date_select_button);
        _dateSelectButton.Click += DateSelect_OnClick;
    }

    void DateSelect_OnClick(object sender, EventArgs eventArgs)
    {
        DatePickerFragment frag = DatePickerFragment.NewInstance(delegate(DateTime time)
                                                                 {
                                                                     _dateDisplay.Text = time.ToLongDateString();
                                                                 });
        frag.Show(FragmentManager, DatePickerFragment.TAG);
    }
}
```


## <a name="summary"></a>Zusammenfassung

In diesem Beispiel erläutert die Vorgehensweise beim Anzeigen einer `DatePicker` Widgets, wie ein modales Popup-Dialogfeld als Teil einer Android-Aktivität. Diese beispielimplementierung DialogFragment bereitgestellt und erläutert die `IOnDateSetListener` Schnittstelle. In diesem Beispiel wird auch gezeigt, wie die DialogFragment interagieren kann, mit dem Host-Aktivität, um das ausgewählte Datum angezeigt.


## <a name="related-links"></a>Verwandte Links

- [DialogFragment](https://developer.xamarin.com/api/type/Android.App.DialogFragment/)
- [DatePicker](https://developer.xamarin.com/api/type/Android.Widget.DatePicker/)
- [DatePickerDialog](https://developer.xamarin.com/api/type/Android.App.DatePickerDialog/)
- [DatePickerDialog.IOnDateSetListener](https://developer.xamarin.com/api/type/Android.App.DatePickerDialog+IOnDateSetListener/)
- [Wählen Sie ein Datum](https://github.com/xamarin/recipes/tree/master/Recipes/android/controls/datepicker/select_a_date)
