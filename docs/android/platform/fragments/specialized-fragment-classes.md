---
title: Spezielle Fragmentklassen
ms.prod: xamarin
ms.assetid: 7A0AEB2C-EE77-63BF-652A-DA049B691C64
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/08/2018
ms.openlocfilehash: 8d4dcedae6298d9a56ba52d4da3d081d4d69afe1
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70757559"
---
# <a name="specialized-fragment-classes"></a>Spezielle Fragmentklassen

Die Fragments-API bietet andere Unterklassen, die einige der gängigeren Funktionen in-Anwendungen Kapseln. Diese Unterklassen sind:

- **Listfragment** &ndash; Dieses Fragment wird verwendet, um eine Liste von Elementen anzuzeigen, die an eine Datenquelle gebunden sind, z. b. ein Array oder einen Cursor.

- **Dialogfragment** &ndash; Dieses Fragment wird als Wrapper für ein Dialogfeld verwendet. Das Fragment zeigt das Dialogfeld oberhalb der Aktivität an.

- **Preferencefragment** &ndash; Dieses Fragment wird verwendet, um Preference-Objekte als Listen anzuzeigen.

## <a name="the-listfragment"></a>Das listfragment

Der `ListFragment` ähnelt dem Konzept und der `ListActivity`Funktionalität von. es handelt sich um einen Wrapper, der einen `ListView` in einem Fragment hostet. Das folgende Bild zeigt eine `ListFragment` , die auf einem Tablet und einem Telefon ausgeführt wird:

[![Screenshots von listfragment auf einem Tablet und einem Telefon](specialized-fragment-classes-images/intro-screenshot-sml.png)](specialized-fragment-classes-images/intro-screenshot.png#lightbox)

### <a name="binding-data-with-the-listadapter"></a>Binden von Daten mit dem listadapter

Die `ListFragment` -Klasse stellt bereits ein Standardlayout bereit. Daher ist es nicht notwendig `OnCreateView` , außer Kraft zu setzen, `ListFragment`um den Inhalt von anzuzeigen. Die `ListView` wird mithilfe einer `ListAdapter` -Implementierung an Daten gebunden. Das folgende Beispiel zeigt, wie dies mithilfe eines einfachen Arrays von Zeichen folgen durchgeführt werden kann:

```csharp
public override void OnActivityCreated(Bundle savedInstanceState)
{
    base.OnActivityCreated(savedInstanceState);
    string[] values = new[] { "Android", "iPhone", "WindowsMobile",
                "Blackberry", "WebOS", "Ubuntu", "Windows7", "Max OS X",
                "Linux", "OS/2" };
    this.ListAdapter = new ArrayAdapter<string>(Activity, Android.Resource.Layout.SimpleExpandableListItem1, values);
}
```

Beim Festlegen `ListAdapter`von ist es wichtig, die `ListFragment.ListAdapter` -Eigenschaft und nicht die `ListView.ListAdapter` -Eigenschaft zu verwenden. Die `ListView.ListAdapter` Verwendung von bewirkt, dass ein wichtiger Initialisierungs Code übersprungen wird.

### <a name="responding-to-user-selection"></a>Reaktion auf Benutzer Auswahl

Um auf die Benutzer Auswahl zu reagieren, muss eine Anwendung `OnListItemClick` die-Methode überschreiben. Das folgende Beispiel zeigt eine mögliche Möglichkeit:

```csharp
public override void OnListItemClick(ListView l, View v, int index, long id)
{
    // We can display everything in place with fragments.
    // Have the list highlight this item and show the data.
    ListView.SetItemChecked(index, true);

    // Check what fragment is shown, replace if needed.
    var details = FragmentManager.FindFragmentById<DetailsFragment>(Resource.Id.details);
    if (details == null || details.ShownIndex != index)
    {
        // Make new fragment to show this selection.
        details = DetailsFragment.NewInstance(index);

        // Execute a transaction, replacing any existing
        // fragment with this one inside the frame.
        var ft = FragmentManager.BeginTransaction();
        ft.Replace(Resource.Id.details, details);
        ft.SetTransition(FragmentTransit.FragmentFade);
        ft.Commit();
    }
}
```

Wenn der Benutzer im obigen Code ein Element in der `ListFragment`auswählt, wird ein neues Fragment in der hostingaktivität angezeigt, das weitere Details über das Element anzeigt, das ausgewählt wurde.

## <a name="dialogfragment"></a>DialogFragment

Das *dialogfragment* ist ein Fragment, das verwendet wird, um ein Dialog Objekt innerhalb eines Fragments anzuzeigen, das über dem Aktivitäts Fenster schwebt. Er soll die verwalteten Dialog-APIs (beginnend mit Android 3,0) ersetzen. Der folgende Screenshot zeigt ein Beispiel für einen `DialogFragment`:

[![Screenshot von dialogfragment mit dem Kontrollkästchen "neues Fahrzeug hinzufügen"](specialized-fragment-classes-images/dialog-fragment-example.png)](specialized-fragment-classes-images/dialog-fragment-example.png#lightbox)

Ein `DialogFragment` stellt sicher, dass der Zustand zwischen dem Fragment und dem Dialogfeld konsistent bleibt. Alle Interaktionen und die Steuerung des Dialog Objekts sollten über die `DialogFragment` API erfolgen und nicht mit direkten Aufrufen des Dialog Objekts. Die `DialogFragment` API stellt jeder Instanz eine `Show()` Methode zur Verfügung, mit der ein Fragment angezeigt wird. Es gibt zwei Möglichkeiten, ein Fragment zu entfernen:

- Ruft `DialogFragment.Dismiss()` für die `DialogFragment` -Instanz auf. 

- Einen anderen `DialogFragment`anzeigen.

Zum Erstellen eines `DialogFragment`erbt eine Klasse von `Android.App.DialogFragment,` und überschreibt dann eine der folgenden beiden Methoden:

- **Onkreateview** &ndash; Dadurch wird eine Sicht erstellt und zurückgegeben.

- **Onkreatedialog** &ndash; Dadurch wird ein benutzerdefiniertes Dialogfeld erstellt. Sie wird in der Regel zum Anzeigen eines *alertdialog-Dialog*Felds verwendet. Beim Überschreiben dieser Methode ist es nicht erforderlich, zu `OnCreateView` überschreiben.

### <a name="a-simple-dialogfragment"></a>Ein einfaches dialogfragment

Der folgende Screenshot zeigt eine einfache `DialogFragment` , die über `TextView` einen und `Button`zwei s verfügt:

[![Beispiel dialogfragment mit einer TextView und zwei Schaltflächen](specialized-fragment-classes-images/dialog-fragment-example-2.png)](specialized-fragment-classes-images/dialog-fragment-example-2.png#lightbox)

Das `TextView` zeigt`DialogFragment`an, wie oft der Benutzer auf eine Schaltfläche in geklickt hat, während durch Klicken auf die andere Schaltfläche das Fragment geschlossen wird. Der Code für `DialogFragment` ist:

```csharp
public class MyDialogFragment : DialogFragment
{
    private int _clickCount;
    public override void OnCreate(Bundle savedInstanceState)
    {
        _clickCount = 0;
    }

    public override View OnCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState)
    {
        base.OnCreate(savedInstanceState)
        
        var view = inflater.Inflate(Resource.Layout.dialog_fragment_layout, container, false);
        var textView = view.FindViewById<TextView>(Resource.Id.dialog_text_view);
            
        view.FindViewById<Button>(Resource.Id.dialog_button).Click += delegate
            {
                textView.Text = "You clicked the button " + _clickCount++ + " times.";
            };

        // Set up a handler to dismiss this DialogFragment when this button is clicked.
        view.FindViewById<Button>(Resource.Id.dismiss_dialog_button).Click += (sender, args) => Dismiss();
        return view;
        }
    }
}
```

### <a name="displaying-a-fragment"></a>Anzeigen eines Fragments

Wie alle Fragmente wird ein `DialogFragment` im Kontext `FragmentTransaction`von angezeigt.

Die `Show()` -Methode für `DialogFragment` einen nimmt `FragmentTransaction` ein und `string` ein als Eingabe an. Das Dialogfeld wird der-Aktivität hinzugefügt, und `FragmentTransaction` der Commit wird ausgeführt.

Der folgende Code veranschaulicht eine mögliche Art und Weise, wie eine `Show()` Aktivität die-Methode `DialogFragment`verwendet, um einen anzuzeigen:

```csharp
public void ShowDialog()
{
    var transaction = FragmentManager.BeginTransaction();
    var dialogFragment = new MyDialogFragment();
    dialogFragment.Show(transaction, "dialog_fragment");
}
```

### <a name="dismissing-a-fragment"></a>Verwerfen eines Fragments

Das `Dismiss()` aufrufen`DialogFragment` von für eine Instanz von bewirkt, dass ein Fragment aus der Aktivität entfernt wird und ein Commit für diese Transaktion ausgeführt wird.
Die standardmäßigen fragmentlebens Zyklus-Methoden, die an der Zerstörung eines Fragments beteiligt sind, werden aufgerufen.

### <a name="alert-dialog"></a>Warn Dialogfeld

Anstatt zu über `OnCreateView`schreiben, `DialogFragment` kann ein stattdessen `OnCreateDialog`überschreiben. Dadurch kann eine Anwendung ein [alertdialog-Dialog](xref:Android.App.AlertDialog) Feld erstellen, das von einem Fragment verwaltet wird. Der folgende Code ist ein Beispiel für die `AlertDialog.Builder` Verwendung von zum Erstellen eines: `Dialog`

```csharp
public class AlertDialogFragment : DialogFragment
{
    public override Dialog OnCreateDialog(Bundle savedInstanceState)
    {
        EventHandler<DialogClickEventArgs> okhandler;
        var builder = new AlertDialog.Builder(Activity)
            .SetMessage("This is my dialog.")
            .SetPositiveButton("Ok", (sender, args) =>
                                         {
                                             // Do something when this button is clicked.
                                         })
            .SetTitle("Custom Dialog");
        return builder.Create();
    }
}
```

## <a name="preferencefragment"></a>PreferenceFragment

Zur Unterstützung der Verwaltung von Einstellungen stellt die Fragmente `PreferenceFragment` -API die-Unterklasse bereit. Das `PreferenceFragment` ähnelt der [preferenceactivity-Aktivität](xref:Android.Preferences.PreferenceActivity) &ndash; und zeigt eine Hierarchie von Voreinstellungen für den Benutzer in einem Fragment an. Wenn der Benutzer mit den Einstellungen interagiert, werden diese automatisch in [sharedpreferences](https://developer.android.com/reference/android/content/SharedPreferences.html)gespeichert.
Verwenden Sie in Anwendungen mit Android 3,0 oder höher `PreferenceFragment` , um die Einstellungen in Anwendungen zu behandeln. Die folgende Abbildung zeigt ein Beispiel für einen `PreferenceFragment`:

[![Beispiel für preferencesfragment mit Inline-, Dialog-und Start Einstellungen](specialized-fragment-classes-images/preferences-dialog.png)](specialized-fragment-classes-images/preferences-dialog.png#lightbox)

### <a name="create-a-preference-fragment-from-a-resource"></a>Erstellen eines Einstellungs Fragments aus einer Ressource

Das Preference-Fragment kann mithilfe der [preferencefragment. addpreferencesfromresource](xref:Android.Preferences.PreferenceFragment.AddPreferencesFromResource*) -Methode aus einer XML-Ressourcen Datei aufgeblasen werden. Eine logische Stelle zum Aufrufen dieser Methode im Lebenszyklus des Fragments wäre die `OnCreate` -Methode.

Der `PreferenceFragment` oben gezeigte wurde durch Laden einer Ressource aus XML erstellt. Die Ressourcen Datei lautet wie folgt:

```xml
<?xml version="1.0" encoding="utf-8"?>

<PreferenceScreen xmlns:android="http://schemas.android.com/apk/res/android">

  <PreferenceCategory android:title="Inline Preferences">
    <CheckBoxPreference android:key="checkbox_preference"
                        android:title="Checkbox Preference Title"
                        android:summary="Checkbox Preference Summary" />

  </PreferenceCategory>

  <PreferenceCategory android:title="Dialog Based Preferences">

    <EditTextPreference android:key="edittext_preference"
                        android:title="EditText Preference Title"
                        android:summary="EditText Preference Summary"
                        android:dialogTitle="Edit Text Preferrence Dialog Title" />

  </PreferenceCategory>

  <PreferenceCategory android:title="Launch Preferences">

    <!-- This PreferenceScreen tag serves as a screen break (similar to page break
             in word processing). Like for other preference types, we assign a key
             here so it is able to save and restore its instance state. -->
    <PreferenceScreen android:key="screen_preference"
                      android:title="Title Screen Preferences"
                      android:summary="Summary Screen Preferences">

      <!-- You can place more preferences here that will be shown on the next screen. -->

      <CheckBoxPreference android:key="next_screen_checkbox_preference"
                          android:title="Next Screen Toggle Preference Title"
                          android:summary="Next Screen Toggle Preference Summary" />

    </PreferenceScreen>

    <PreferenceScreen android:title="Intent Preference Title"
                      android:summary="Intent Preference Summary">

      <intent android:action="android.intent.action.VIEW"
              android:data="http://www.android.com" />

    </PreferenceScreen>

  </PreferenceCategory>

</PreferenceScreen>
```

Der Code für das Einstellungs Fragment lautet wie folgt:

```csharp
public class PrefFragment : PreferenceFragment
{
    public override void OnCreate(Bundle savedInstanceState)
    {
        base.OnCreate(savedInstanceState);
        AddPreferencesFromResource(Resource.Xml.preferences);
    }
}
```

### <a name="querying-activities-to-create-a-preference-fragment"></a>Abfragen von Aktivitäten zum Erstellen eines Einstellungs Fragments

Ein weiteres Verfahren zum Erstellen `PreferenceFragment` einer umfasst das Abfragen von Aktivitäten. Jede Aktivität kann das [\_metadatenschlüsseleinstellungs\_](xref:Android.Preferences.PreferenceManager.MetadataKeyPreferences) -Attribut verwenden, das auf eine XML-Ressourcen Datei verweist. In xamarin. Android erfolgt dies durch das hinzulegen einer Aktivität mit dem `MetaDataAttribute`und das anschließende angeben der zu verwendenden Ressourcen Datei. Die `PreferenceFragment` -Klasse stellt die [addpreferencefromintent](xref:Android.Preferences.PreferenceFragment.AddPreferencesFromIntent*) -Methode bereit, die verwendet werden kann, um eine Aktivität zum Auffinden dieser XML-Ressource abzufragen und eine bevorzugte Hierarchie für diese zu erhöhen.

Ein Beispiel für diesen Prozess finden Sie im folgenden Code Ausschnitt, der verwendet `AddPreferencesFromIntent` , um eine `PreferenceFragment`zu erstellen:

```csharp
public class MyPreferenceFragment : PreferenceFragment
{
    public override void OnCreate(Bundle savedInstanceState)
    {
        base.OnCreate(savedInstanceState);
        var intent = new Intent(this.Activity, typeof (MyActivityWithPreferences));
        AddPreferencesFromIntent(intent);
    }
}
```

Android sieht sich die-Klasse `MyActivityWithPreference`an. Die Klasse muss mit dem `MetaDataAttribute,` versehen werden, wie im folgenden Code Ausschnitt gezeigt:

```csharp
[Activity(Label = "My Activity with Preferences")]
[MetaData(PreferenceManager.MetadataKeyPreferences, Resource = "@xml/preference_from_intent")]
public class MyActivityWithPreferences : Activity
{
    protected override void OnCreate(Bundle bundle)
    {
        base.OnCreate(bundle);
        // This is deliberately blank
    }
}
```

Der `MetaDataAttribute` deklariert eine XML-Ressourcen Datei, `PreferenceFragment` die von verwendet wird, um die Voreinstellungs Hierarchie aufzufüllen. Wenn nicht angegeben wird, wird zur Laufzeit eine Ausnahme ausgelöst. `MetatDataAttribute` Wenn dieser Code ausgeführt wird, `PreferenceFragment` wird der wie im folgenden Screenshot angezeigt:

[![Screenshot der Beispiel-App mit angezeigter preferencefragment](specialized-fragment-classes-images/preference-fragment-getpreferencesfromintent.png)](specialized-fragment-classes-images/preference-fragment-getpreferencesfromintent.png#lightbox)
