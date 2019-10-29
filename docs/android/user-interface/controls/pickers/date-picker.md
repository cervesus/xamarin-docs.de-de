---
title: Datumsauswahl
description: Auswählen von Kalender Datumsangaben mithilfe von datepickerdialog und dialogfragment
ms.prod: xamarin
ms.assetid: F2BCD8D4-8957-EA53-C5A8-6BB603ADB47B
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 01/22/2018
ms.openlocfilehash: b54a0795dee0bd7a02b419da497f9a1b3f3ee7ff
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029217"
---
# <a name="android-date-picker"></a>Android-Datumsauswahl

## <a name="overview"></a>Übersicht

Es gibt Situationen, in denen ein Benutzerdaten in eine Android-Anwendung eingeben muss. Um dies zu unterstützen, stellt das Android-Framework das [`DatePicker`](xref:Android.Widget.DatePicker) -Widget und die [`DatePickerDialog`](xref:Android.App.DatePickerDialog) bereit. Der `DatePicker` ermöglicht es Benutzern, das Jahr, den Monat und den Tag in einer konsistenten Schnittstelle über Geräte und Anwendungen hinweg auszuwählen. Der `DatePickerDialog` ist eine Hilfsklasse, die die `DatePicker` in einem Dialogfeld kapselt.

Moderne Android-Anwendungen sollten die `DatePickerDialog` in einem [`DialogFragment`](xref:Android.App.DialogFragment)anzeigen. Dadurch kann eine Anwendung DatePicker als Popup Dialogfeld anzeigen oder in eine Aktivität eingebettet werden. Außerdem verwaltet der `DialogFragment` den Lebenszyklus und die Anzeige des Dialog Felds, wodurch die zu implementierende Code Menge reduziert wird.

In dieser Anleitung wird veranschaulicht, wie die `DatePickerDialog`in einem `DialogFragment`verwendet wird. In der Beispielanwendung wird die `DatePickerDialog` als modales Dialogfeld angezeigt, wenn der Benutzer auf eine Schaltfläche in einer Aktivität klickt. Wenn das Datum vom Benutzer festgelegt wird, wird ein `TextView` mit dem ausgewählten Datum aktualisiert.

[![Screenshot der Schaltfläche "Datum auswählen" gefolgt von Datumsauswahl Dialogfeld](date-picker-images/image-01-sml.png)](date-picker-images/image-01.png#lightbox)

## <a name="requirements"></a>Anforderungen

Die Beispielanwendung für dieses Handbuch zielt auf Android 4,1 (API-Ebene) ab.
16) oder höher, aber auf Android 3,0 (API-Ebene 11 oder höher) anwendbar ist. Es ist möglich, ältere Android-Versionen zu unterstützen, wobei die Android-Unterstützungs Bibliothek V4 zum Projekt hinzugefügt wird und einige Codeänderungen vorgenommen werden.

## <a name="using-the-datepicker"></a>Verwenden von DatePicker

In diesem Beispiel wird `DialogFragment`erweitert. Die Unterklasse hostet und zeigt eine `DatePickerDialog`an:

![Dialogfeld "Schließung der Datumsauswahl"](date-picker-images/image-02.png)

Wenn der Benutzer ein Datum auswählt und auf die Schaltfläche **OK** klickt, wird die-Methode vom `DatePickerDialog` aufgerufen [`IOnDateSetListener.OnDateSet`](xref:Android.App.DatePickerDialog.IOnDateSetListener.OnDateSet*).
Diese Schnittstelle wird vom Hosting`DialogFragment`implementiert. Wenn der Benutzer auf die Schaltfläche **Abbrechen** klickt, werden Fragment und Dialogfeld selbst geschlossen.

Es gibt mehrere Möglichkeiten, wie die `DialogFragment` das ausgewählte Datum an die hostingaktivität zurückgeben kann:

1. **Aufrufen einer Methode oder Festlegen einer Eigenschaft** &ndash; die Aktivität kann eine Eigenschaft oder eine Methode bereitstellen, die speziell zum Festlegen dieses Werts vorgesehen ist.

2. **Ein Ereignis** wird ausgelöst, &ndash; der `DialogFragment` ein Ereignis definieren kann, das beim Aufrufen `OnDateSet` ausgelöst wird.

3. **Verwenden Sie eine `Action`** &ndash; das `DialogFragment` eine `Action<DateTime>` aufrufen kann, um das Datum in der Aktivität anzuzeigen. Die-Aktivität stellt beim Instanziieren der `DialogFragment`die `Action<DateTime` bereit. In diesem Beispiel wird das dritte Verfahren verwendet, und es ist erforderlich, dass die Aktivität eine `Action<DateTime>` für die `DialogFragment`bereitstellt.

### <a name="extending-dialogfragment"></a>Erweitern von dialogfragment

Der erste Schritt beim Anzeigen einer `DatePickerDialog` ist die Unterklasse `DialogFragment` und die Implementierung der `IOnDateSetListener`-Schnittstelle:

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

Die `NewInstance`-Methode wird aufgerufen, um eine neue `DatePickerFragment`zu instanziieren. Diese Methode nimmt eine `Action<DateTime>` an, die aufgerufen wird, wenn der Benutzer auf die Schaltfläche **OK** im `DatePickerDialog`klickt.

Wenn das Fragment angezeigt werden soll, ruft Android die-Methode `OnCreateDialog`auf. Diese Methode erstellt ein neues `DatePickerDialog`-Objekt und initialisiert es mit dem aktuellen Datum und dem Rückruf Objekt (bei dem es sich um die aktuelle Instanz des `DatePickerFragment`handelt).

> [!NOTE]
> Beachten Sie, dass der Wert des Monats, wenn `IOnDateSetListener.OnDateSet` aufgerufen wird, im Bereich von 0 bis 11 und nicht zwischen 1 und 12 liegt. Der Tag des Monats liegt im Bereich von 1 bis 31 (abhängig von dem ausgewählten Monat).

### <a name="showing-the-datepickerfragment"></a>Das datepickerfragment wird angezeigt.

Nachdem der `DialogFragment` implementiert wurde, wird in diesem Abschnitt erläutert, wie das Fragment in einer-Aktivität verwendet wird. In der Beispiel-APP, die dieser Anleitung folgt, instanziiert die Aktivität die `DialogFragment` mithilfe der `NewInstance` Factory-Methode und zeigt dann `DialogFragment.Show`Aufrufen an. Als Teil der Instanziierung des `DialogFragment`übergibt die-Aktivität eine `Action<DateTime>`, die das Datum in einem `TextView` anzeigt, das von der-Aktivität gehostet wird:

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

In diesem Beispiel wurde erläutert, wie ein `DatePicker`-Widget als modales Popup Dialogfeld als Teil einer Android-Aktivität angezeigt wird. Es wurde eine Beispiel dialogfragment-Implementierung bereitgestellt und die `IOnDateSetListener`-Schnittstelle erläutert. Dieses Beispiel hat auch veranschaulicht, wie das dialogfragment mit der Host Aktivität interagieren kann, um das ausgewählte Datum anzuzeigen.

## <a name="related-links"></a>Verwandte Links

- [Dialogfragment](xref:Android.App.DialogFragment)
- [DatePicker](xref:Android.Widget.DatePicker)
- [Datepickerdialog](xref:Android.App.DatePickerDialog)
- [Datepickerdialog. ionidatesetlistener](xref:Android.App.DatePickerDialog.IOnDateSetListener)
- [Datum auswählen](https://github.com/xamarin/recipes/tree/master/Recipes/android/controls/datepicker/select_a_date)
