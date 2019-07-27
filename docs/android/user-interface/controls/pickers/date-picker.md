---
title: Datumsauswahl
description: Auswählen von Kalender Datumsangaben mithilfe von datepickerdialog und dialogfragment
ms.prod: xamarin
ms.assetid: F2BCD8D4-8957-EA53-C5A8-6BB603ADB47B
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 01/22/2018
ms.openlocfilehash: ef9abbd60fc83622631b916c50f4993c1c848b00
ms.sourcegitcommit: b07e0259d7b30413673a793ebf4aec2b75bb9285
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/26/2019
ms.locfileid: "68510257"
---
# <a name="android-date-picker"></a>Android-Datumsauswahl

## <a name="overview"></a>Übersicht

Es gibt Situationen, in denen ein Benutzerdaten in eine Android-Anwendung eingeben muss. Um dies zu unterstützen, stellt das Android [`DatePicker`](xref:Android.Widget.DatePicker) [`DatePickerDialog`](xref:Android.App.DatePickerDialog) -Framework das-Widget und bereit. `DatePicker` Ermöglicht Benutzern die Auswahl von Jahr, Monat und Tag in einer konsistenten Schnittstelle über Geräte und Anwendungen hinweg. Ist eine Hilfsklasse, die die `DatePicker` in einem Dialogfeld kapselt. `DatePickerDialog`

Moderne Android-Anwendungen sollten den `DatePickerDialog` in einem [`DialogFragment`](xref:Android.App.DialogFragment)anzeigen. Dadurch kann eine Anwendung DatePicker als Popup Dialogfeld anzeigen oder in eine Aktivität eingebettet werden. Außerdem `DialogFragment` verwaltet das den Lebenszyklus und die Anzeige des Dialog Felds und reduziert so die Menge an Code, die implementiert werden muss.

In dieser Anleitung wird veranschaulicht, wie die `DatePickerDialog`in einem `DialogFragment`umschließenen verwendet wird. In der Beispielanwendung wird `DatePickerDialog` als modales Dialogfeld angezeigt, wenn der Benutzer auf eine Schaltfläche in einer Aktivität klickt. Wenn das Datum vom Benutzer festgelegt wird, wird `TextView` ein mit dem ausgewählten Datum aktualisiert.

[![Screenshot der Schaltfläche "Datum auswählen" gefolgt von Datumsauswahl Dialogfeld](date-picker-images/image-01-sml.png)](date-picker-images/image-01.png#lightbox)

## <a name="requirements"></a>Anforderungen

Die Beispielanwendung für dieses Handbuch zielt auf Android 4,1 (API-Ebene) ab.
16) oder höher, aber auf Android 3,0 (API-Ebene 11 oder höher) anwendbar ist. Es ist möglich, ältere Android-Versionen zu unterstützen, wobei die Android-Unterstützungs Bibliothek V4 zum Projekt hinzugefügt wird und einige Codeänderungen vorgenommen werden.

## <a name="using-the-datepicker"></a>Verwenden von DatePicker

Dieses Beispiel wird erweitert `DialogFragment`. Die Unterklasse hostet und zeigt `DatePickerDialog`Folgendes an:

![Dialogfeld "Schließung der Datumsauswahl"](date-picker-images/image-02.png)

Wenn der Benutzer ein Datum auswählt und auf  die Schaltfläche OK `DatePickerDialog` klickt, ruft die [`IOnDateSetListener.OnDateSet`](xref:Android.App.DatePickerDialog.IOnDateSetListener.OnDateSet*)die-Methode auf.
Diese Schnittstelle wird durch das Hosting `DialogFragment`implementiert. Wenn der Benutzer auf die Schaltfläche **Abbrechen** klickt, werden Fragment und Dialogfeld selbst geschlossen.

Es gibt mehrere Möglichkeiten, `DialogFragment` wie das ausgewählte Datum an die hostingaktivität zurückgeben kann:

1. **Aufrufen einer Methode oder Festlegen einer Eigenschaft** &ndash; Die-Aktivität kann eine Eigenschaft oder Methode bereitstellen, die speziell zum Festlegen dieses Werts vorgesehen ist.

2. **Ein Ereignis hervorrufen** Der kann ein Ereignis definieren, das ausgelöst wird, `OnDateSet` wenn aufgerufen wird. `DialogFragment` &ndash;

3. **Verwenden Sie `Action` einen** &ndash; , der `DialogFragment` einen`Action<DateTime>` aufrufen kann, um das Datum in der Aktivität anzuzeigen. Die-Aktivität stellt beim `Action<DateTime` `DialogFragment`Instanziieren von bereit. `Action<DateTime>` In diesem Beispiel wird das dritte Verfahren verwendet, und es ist erforderlich, dass die- `DialogFragment`Aktivität für die bereitstellt.

### <a name="extending-dialogfragment"></a>Erweitern von dialogfragment

Der erste Schritt beim Anzeigen eines `DatePickerDialog` ist die Unterklasse `DialogFragment` und die Implementierung der `IOnDateSetListener` -Schnittstelle:

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

Die `NewInstance` -Methode wird aufgerufen, um eine neue `DatePickerFragment`zu instanziieren. Diese Methode nimmt einen `Action<DateTime>` -Wert an, der aufgerufen wird, wenn der Benutzer auf die Schalt `DatePickerDialog`Fläche **OK** in klickt.

Wenn das Fragment angezeigt werden soll, ruft Android die-Methode `OnCreateDialog`auf. Diese Methode erstellt ein neues `DatePickerDialog` -Objekt und initialisiert es mit dem aktuellen Datum und dem Rückruf Objekt (bei dem es sich um die aktuelle Instanz `DatePickerFragment`von handelt).

> [!NOTE]
> Beachten Sie, dass der Wert des Monats, `IOnDateSetListener.OnDateSet` wenn aufgerufen wird, im Bereich von 0 bis 11 und nicht zwischen 1 und 12 liegt. Der Tag des Monats liegt im Bereich von 1 bis 31 (abhängig von dem ausgewählten Monat).

### <a name="showing-the-datepickerfragment"></a>Das datepickerfragment wird angezeigt.

Nachdem der `DialogFragment` implementiert wurde, wird in diesem Abschnitt erläutert, wie das-Fragment in einer-Aktivität verwendet wird. In der Beispiel-APP, die diese Anleitung begleitet, instanziiert die- `DialogFragment` Aktivität mithilfe `NewInstance` der Factorymethode und zeigt `DialogFragment.Show`dann den Aufruf an. Als Teil der Instanziierung `DialogFragment`von übergibt die-Aktivität einen `Action<DateTime>`, der das Datum in einer `TextView` anzeigt, die von der-Aktivität gehostet wird:

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

In diesem Beispiel wurde erläutert, wie `DatePicker` ein Widget als modales Popup Dialogfeld als Teil einer Android-Aktivität angezeigt wird. Es wurde eine Beispiel dialogfragment-Implementierung bereitgestellt `IOnDateSetListener` und die-Schnittstelle erläutert. Dieses Beispiel hat auch veranschaulicht, wie das dialogfragment mit der Host Aktivität interagieren kann, um das ausgewählte Datum anzuzeigen.

## <a name="related-links"></a>Verwandte Links

- [DialogFragment](xref:Android.App.DialogFragment)
- [DatePicker](xref:Android.Widget.DatePicker)
- [DatePickerDialog](xref:Android.App.DatePickerDialog)
- [DatePickerDialog.IOnDateSetListener](xref:Android.App.DatePickerDialog.IOnDateSetListener)
- [Datum auswählen](https://github.com/xamarin/recipes/tree/master/Recipes/android/controls/datepicker/select_a_date)
