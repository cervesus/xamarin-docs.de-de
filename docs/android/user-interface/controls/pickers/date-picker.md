---
title: Datumsauswahl
description: "Datumsangaben, die mit dem DatePickerDialog und DialogFragment auswählen"
ms.topic: article
ms.prod: xamarin
ms.assetid: F2BCD8D4-8957-EA53-C5A8-6BB603ADB47B
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 01/22/2018
ms.openlocfilehash: b62af404ce0d3f5dacc479682a3002af49e968d1
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/12/2018
---
# <a name="date-picker"></a>Datumsauswahl

## <a name="overview"></a>Übersicht

Es gibt Situationen, wenn ein Benutzer Daten in einer Android-Anwendung eingeben muss. Um dabei zu helfen, das Android-Framework bietet die [ `DatePicker` ](https://developer.xamarin.com/api/type/Android.Widget.DatePicker/) Widget und [ `DatePickerDialog` ](https://developer.xamarin.com/api/type/Android.App.DatePickerDialog/) . Die `DatePicker` ermöglicht es Benutzern, wählen Sie das Jahr, Monat und Tag in einer konsistenten Schnittstelle verschiedene Geräte und Anwendungen. Die `DatePickerDialog` ist eine Hilfsklasse, die kapselt die `DatePicker` in einem Dialogfeld.

Moderne Android-Anwendungen sollten angezeigt werden die `DatePickerDialog` in einem [ `DialogFragment` ](https://developer.xamarin.com/api/type/Android.App.DialogFragment/). Dies kann eine Anwendung die DatePicker als ein Popupdialogfeld angezeigt oder in einer Aktivität eingebettet. Darüber hinaus die `DialogFragment` Verwalten des Lebenszyklus und die Anzeige des Dialogfelds reduziert die Menge an Code, der implementiert werden muss.

Dieses Handbuch zeigen, wie mithilfe der `DatePickerDialog`, die eingebunden in eine `DialogFragment`. Die beispielanwendung zeigt die `DatePickerDialog` als modales Dialogfeld klickt der Benutzer eine Schaltfläche in einer Aktivität. Wenn das Datum, durch den Benutzer festgelegt ist ein `TextView` wird mit dem Datum, die ausgewählt wurden aktualisiert.

[![Screenshot der Pick Datum Schaltfläche gefolgt von Datumsauswahl-Dialogfeld](date-picker-images/image-01-sml.png)](date-picker-images/image-01.png#lightbox)

## <a name="requirements"></a>Anforderungen

Die beispielanwendung für diese Anleitung richtet Android 4.1 (API-Ebene
16) oder höher, sondern gilt für Android 3.0 (API-Ebene 11 oder höher). Es ist möglich, durch das Hinzufügen der Bibliothek für Android unterstützt v4 auf das Projekt und einige Änderungen am Code ältere Versionen von Android unterstützt.

## <a name="using-the-datepicker"></a>Mithilfe der Datumsauswahl

Erweitern Sie in diesem Beispiel `DialogFragment`. Die Unterklasse gehostet wird, und zeigt eine `DatePickerDialog`:

![Nahaufnahme der Datumsauswahl-Dialogfeld](date-picker-images/image-02.png)

Wenn der Benutzer wählt ein Datum, und klickt auf die **OK** Schaltfläche, die `DatePickerDialog` rufen Sie die Methode [ `IOnDateSetListener.OnDateSet` ](https://developer.xamarin.com/api/member/Android.App.DatePickerDialog+IOnDateSetListener.OnDateSet/p/Android.Widget.DatePicker/System.Int32/System.Int32/System.Int32/).
Diese Schnittstelle wird implementiert, indem Sie das Hosten `DialogFragment`. Wenn der Benutzer klickt auf die **"Abbrechen"** Schaltfläche, Fragment und Dialogfeld selbst verworfen werden.

Es gibt mehrere Möglichkeiten der `DialogFragment` kann das ausgewählte Datum zurückgeben, mit der hosting-Aktivität:

1. **Aufrufen einer Methode oder eine Eigenschaft festlegen,** &ndash; der Aktivität können bereit, einer Eigenschaft oder Methode, die speziell für das Festlegen dieses Werts.

2. **Ein Ereignis auszulösen,** &ndash; der `DialogFragment` können ein Ereignis, die definieren, wird ausgelöst, wenn `OnDateSet` aufgerufen wird.

3. **Verwenden einer `Action`**  &ndash; der `DialogFragment` können Aufrufen einer `Action<DateTime>` das Datum in der Aktivität anzeigen. Geben Sie die Aktivität wird der `Action<DateTime` bei der Instanziierung der `DialogFragment`. In diesem Beispiel wird das dritte Verfahren verwenden erforderlich, und geben Sie die Aktivität ein `Action<DateTime>` auf die `DialogFragment`.



### <a name="extending-dialogfragment"></a>Erweitern von DialogFragment

Der erste Schritt beim Anzeigen einer `DatePickerDialog` Unterklasse ist `DialogFragment` und Implementieren der `IOnDateSetListener` Schnittstelle:

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

Die `NewInstance` Methode wird aufgerufen, um ein neues instanziieren `DatePickerFragment`. Diese Methode nimmt ein `Action<DateTime>` , wird aufgerufen, die der Benutzer klickt auf die **OK** Schaltfläche der `DatePickerDialog`.

Wenn das Fragment ist, die angezeigt werden, Android wird rufen Sie die Methode `OnCreateDialog`. Diese Methode erstellt eine neue `DatePickerDialog` Objekt, und initialisieren Sie es mit dem aktuellen Datum und die Callback-Objekt (also in die aktuelle Instanz der dem `DatePickerFragment`).


> [!NOTE]
> Beachten Sie, den Wert des Monats, wenn `IOnDateSetListener.OnDateSet` wird aufgerufen, liegt im Bereich von 0 bis 11 und nicht von 1 bis 12. Der Tag des Monats werden im Bereich von 1 bis 31 (je nachdem, welche Monat ausgewählt wurde).



### <a name="showing-the-datepickerfragment"></a>Zeigt die DatePickerFragment

Nachdem die `DialogFragment` wurde implementiert, wird in diesem Abschnitt verwenden Sie das Fragment in einer Aktivität untersuchen. In der Beispielapp, die mit diesem Handbuch wird die Aktivität instanziiert die `DialogFragment` mithilfe der `NewInstance` Factory-Methode, und klicken Sie dann anzeigen, die sie aufrufen `DialogFragment.Show`. Im Rahmen der Instanziierung der `DialogFragment`, die Aktivität übergibt eine `Action<DateTime>`, dem zeigt des Datums in einer `TextView` , die von der Aktivität gehostet wird:

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

In diesem Beispiel erläutert die Vorgehensweise beim Anzeigen einer `DatePicker` Widget als ein modales Popupdialogfeld als Teil einer Android-Aktivität. Beispielimplementierung DialogFragment bereitgestellt werden können, und erläutert die `IOnDateSetListener` Schnittstelle. Dieses Beispiel veranschaulicht auch, wie die DialogFragment interagieren kann, mit dem Host-Aktivität, um das ausgewählte Datum anzuzeigen.


## <a name="related-links"></a>Verwandte Links

- [DialogFragment](https://developer.xamarin.com/api/type/Android.App.DialogFragment/)
- [DatePicker](https://developer.xamarin.com/api/type/Android.Widget.DatePicker/)
- [DatePickerDialog](https://developer.xamarin.com/api/type/Android.App.DatePickerDialog/)
- [DatePickerDialog.IOnDateSetListener](https://developer.xamarin.com/api/type/Android.App.DatePickerDialog+IOnDateSetListener/)
- [Wählen Sie ein Datum](https://github.com/xamarinhttps://developer.xamarin.com/recipes/tree/master/android/controls/datepicker/select_a_date)
