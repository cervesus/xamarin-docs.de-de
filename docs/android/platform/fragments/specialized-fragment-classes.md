---
title: Spezielle Fragment-Klassen
ms.topic: article
ms.prod: xamarin
ms.assetid: 7A0AEB2C-EE77-63BF-652A-DA049B691C64
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/08/2018
ms.openlocfilehash: e7b4349ee2664a94ef6dff3c6a58d5f8f97682a1
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="specialized-fragment-classes"></a>Spezielle Fragment-Klassen

Die Fragmente-API ermöglicht anderen Unterklassen, die einige der häufigeren Funktionalität in Anwendungen zu kapseln. Diese Unterklassen sind:

-   **ListFragment** &ndash; dieses Fragment wird verwendet, um eine Liste von Elementen, die an eine Datenquelle, z. B. ein Array oder ein Cursor gebunden anzuzeigen.

-   **DialogFragment** &ndash; dieses Fragment dient als Wrapper um ein Dialogfeld. Das Fragment wird das Dialogfeld "auf der Aktivität angezeigt.

-   **PreferenceFragment** &ndash; dieses Fragment wird verwendet, um die Einstellung Objekte als Liste anzuzeigen.


<a name="The_ListFragment" />

## <a name="the-listfragment"></a>Die ListFragment

Die `ListFragment` ähnelt Konzept und Funktionen zum der `ListActivity`; es ist ein Wrapper, der hostet eine `ListView` in einem Fragment. Die folgende Abbildung zeigt eine `ListFragment` , die auf einem Tablet-PC und Telefonnummer ausgeführt:

[![Screenshots der ListFragment auf einem Tablet-PC und in einem Smartphone](specialized-fragment-classes-images/intro-screenshot-sml.png)](specialized-fragment-classes-images/intro-screenshot.png)

<a name="Binding_Data_With_The_ListAdapter" />

### <a name="binding-data-with-the-listadapter"></a>Binden von Daten mit der ListAdapter

Die `ListFragment` -Klasse ermöglicht es bereits ein Standardlayout, daher ist es nicht erforderlich, außer Kraft setzen `OnCreateView` zur Anzeige der Inhalte von den `ListFragment`. Die `ListView` an Daten gebunden ist, die mithilfe einer `ListAdapter` Implementierung. Das folgende Beispiel zeigt, wie dies mithilfe eines einfachen Arrays von Zeichenfolgen ausgeführt werden konnte:

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

Beim Festlegen der `ListAdapter`, es ist wichtig, verwenden Sie die `ListFragment.ListAdapter` -Eigenschaft, und nicht die `ListView.ListAdapter` Eigenschaft. Mithilfe von `ListView.ListAdapter` führt dazu, dass wichtige Initialisierungscode übersprungen werden soll.

<a name="Responding_to_User_Selection" />


### <a name="responding-to-user-selection"></a>Reagieren auf Benutzerauswahl

Um die Auswahl des Benutzers zu behandeln, eine Anwendung außer Kraft setzen muss die `OnListItemClick` Methode. Das folgende Beispiel zeigt eine Möglichkeit besteht:

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

Im Code über, wenn der Benutzer ein Element in wählt die `ListFragment`, ein neues Fragment wird angezeigt, in der hosting-Aktivität, weitere Details zu dem Element, das ausgewählt wurde.

<a name="DialogFragment" />


## <a name="dialogfragment"></a>DialogFragment

Die *DialogFragment* ein Fragment, das verwendet wird, um ein-Objekt eines Dialogfelds innerhalb eines Fragments anzuzeigen, die über die Aktivität Fenster verankert ist. Es ist vorgesehen, um das Dialogfeld "verwaltete" APIs (in Android 3.0 ab) zu ersetzen. Der folgende Screenshot zeigt ein Beispiel für eine `DialogFragment`:

[![Screenshot der DialogFragment Hinzufügen neuer Fahrzeug Bearbeitungsfeld anzeigen](specialized-fragment-classes-images/dialog-fragment-example.png)](specialized-fragment-classes-images/dialog-fragment-example.png)

Ein `DialogFragment` wird sichergestellt, dass der Zustand zwischen dem Fragment und das Dialogfeld "konsistent bleiben. Alle Interaktionen und Kontrolle über das Dialogfeldobjekt sollte erfolgen über die `DialogFragment` -API und nicht mit direkten Aufrufe auf dem Dialogfeldobjekt vorgenommen werden. Die `DialogFragment` -API bietet jede Instanz mit einem `Show()` -Methode, die verwendet wird, um ein Fragment anzuzeigen. Es gibt zwei Möglichkeiten, ein Fragment zu beheben:

-  Rufen Sie `DialogFragment.Dismiss()` auf die `DialogFragment` Instanz. 

-  Zeigen Sie eine andere `DialogFragment`.

Zum Erstellen einer `DialogFragment`, eine Klasse erbt von `Android.App.DialogFragment,` und überschreibt anschließend eine der beiden folgenden Methoden:

- **OnCreateView** &ndash; dadurch erstellt, und gibt eine Ansicht zurück.

- **OnCreateDialog** &ndash; ein benutzerdefiniertes Dialogfelds wird erstellt. Es dient normalerweise zum Anzeigen einer *AlertDialog*. Wenn Sie diese Methode überschreiben, ist es nicht erforderlich, außer Kraft setzen `OnCreateView` .


<a name="A_Simple_DialogFragment" />

### <a name="a-simple-dialogfragment"></a>Eine einfache DialogFragment

Der folgende Screenshot zeigt ein einfaches `DialogFragment` , besitzt eine `TextView` und zwei `Button`s:

[![Beispiel-DialogFragment mit zwei Schaltflächen und ein TextView](specialized-fragment-classes-images/dialog-fragment-example-2.png)](specialized-fragment-classes-images/dialog-fragment-example-2.png)

Die `TextView` zeigt die Anzahl der Fälle, in denen der Benutzer eine Schaltfläche geklickt hat die `DialogFragment`, während Sie auf die Schaltfläche "Sonstige" das Fragment schließen. Der Code für `DialogFragment` ist:

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

<a name="Displaying_a_Fragment" />

### <a name="displaying-a-fragment"></a>Anzeigen von einem Fragment

Wie alle Fragmente ein `DialogFragment` wird angezeigt, im Rahmen einer `FragmentTransaction`.

Die `Show()` Methode auf eine `DialogFragment` nimmt eine `FragmentTransaction` und ein `string` als Eingabe. Das Dialogfeld wird der Aktivität hinzugefügt werden und die `FragmentTransaction` ein Commit ausgeführt wurde.

Das folgende Codebeispiel veranschaulicht eine Möglichkeit, eine Aktivität möglicherweise verwenden Sie, die `Show()` Methode zum Anzeigen einer `DialogFragment`:

```csharp
public void ShowDialog()
{
    var transaction = FragmentManager.BeginTransaction();
    var dialogFragment = new MyDialogFragment();
    dialogFragment.Show(transaction, "dialog_fragment");
}
```

<a name="Dismissing_a_Fragment" />

### <a name="dismissing-a-fragment"></a>Schließen ein Fragment

Aufrufen von `Dismiss()` in einer Instanz von einem `DialogFragment` bewirkt, dass ein Fragment aus der Aktivität entfernt werden soll, und führt einen Commit für diese Transaktion.
Die standardmäßigen Fragment-Lifecycle-Methoden, die mit der Zerstörung eines Fragments beteiligt sind, werden aufgerufen werden.

<a name="Alert_Dialog" />

### <a name="alert-dialog"></a>Dialogfeld Warnung

Anstelle von `OnCreateView`, `DialogFragment` kann stattdessen überschreiben `OnCreateDialog`. Dies ermöglicht es einer Anwendung zum Erstellen einer [AlertDialog](https://developer.xamarin.com/api/type/Android.App.AlertDialog/) , die von einem Fragment verwaltet wird. Der folgende Code ist ein Beispiel, verwendet der `AlertDialog.Builder` zum Erstellen einer `Dialog`:

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

 <a name="PreferenceFragment" />


## <a name="preferencefragment"></a>PreferenceFragment

Zur einfachen Verwaltung der Einstellungen der Fragmente-API bietet die `PreferenceFragment` Unterklasse. Die `PreferenceFragment` ähnelt der [PreferenceActivity](https://developer.xamarin.com/api/type/Android.Preferences.PreferenceActivity/
) &ndash; eine Hierarchie von Einstellungen für den Benutzer in einem Fragment wird angezeigt. Wie der Benutzer mit den Einstellungen interagiert, sie werden automatisch gespeichert [SharedPreferences](http://developer.android.com/reference/android/content/SharedPreferences.html).
Verwenden Sie in Android 3.0 oder höher Anwendungen, die `PreferenceFragment` für den Umgang mit Voreinstellungen in Anwendungen. Das folgende Bild zeigt ein Beispiel für eine `PreferenceFragment`:

[![Beispiel-PreferencesFragment mit Inline, Dialogfelder und starten-Voreinstellungen](specialized-fragment-classes-images/preferences-dialog.png)](specialized-fragment-classes-images/preferences-dialog.png)

<a name="Create_A_Preference_Fragment_from_a_Resource" />

### <a name="create-a-preference-fragment-from-a-resource"></a>Erstellen Sie eine Einstellung Fragment aus einer Ressource

Die Voreinstellung Fragment aus einer XML-Ressourcendatei kann, mithilfe vergrößert werden der [PreferenceFragment.AddPreferencesFromResource](https://developer.xamarin.com/api/member/Android.Preferences.PreferenceFragment.AddPreferencesFromResource/p/System.Int32/) Methode. Eine logische direkten Aufruf dieser Methode in den Lebenszyklus des Fragments wird in der `OnCreate` Methode.

Die `PreferenceFragment` abgebildete höher von beim Laden einer Ressourcenpools aus XML erstellt wurde. Die Ressourcendatei wird:

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

Der Code für die Preference-Fragment lautet wie folgt:

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

 <a name="Querying_Activities_to_Create_a_Preference_Fragment" />


### <a name="querying-activities-to-create-a-preference-fragment"></a>Abfragen von Aktivitäten, die ein Fragment Einstellung erstellen

Eine andere Technik zum Erstellen einer `PreferenceFragment` Abfragen Aktivitäten beinhaltet. Jede Aktivität können Sie die [Metadaten\_Schlüssel\_Voreinstellung](https://developer.xamarin.com/api/field/Android.Preferences.PreferenceManager.MetadataKeyPreferences/) -Attribut, das auf einer XML-Ressourcendatei verweisen wird. In Xamarin.Android, dies erfolgt durch Erweitern einer Aktivität mit der `MetaDataAttribute`, und anschließend die zu verwendende Ressourcendatei anzugeben. Die `PreferenceFragment` -Klasse stellt die Methode [AddPreferenceFromIntent](https://developer.xamarin.com/api/member/Android.Preferences.PreferenceFragment.AddPreferencesFromIntent/p/Android.Content.Intent/)), die verwendet werden kann, um eine Aktivität, um diese XML-Ressource zu finden und Vergrößern einer Hierarchie Einstellung dafür abzufragen.

Ein Beispiel dieses Prozesses finden Sie unter den folgenden Codeausschnitt, der verwendet `AddPreferencesFromIntent` zum Erstellen einer `PreferenceFragment`:

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

Android wird untersucht, die Klasse `MyActivityWithPreference`. Die Klasse muss gestaltet werden, mit der `MetaDataAttribute,` wie im folgenden Codeausschnitt gezeigt:

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

Die `MetaDataAttribute` deklariert eine XML-Ressourcendatei, die die `PreferenceFragment` verwendet, um die Einstellung Hierarchie vergrößert werden soll. Wenn die `MetatDataAttribute` nicht angegeben wird, und klicken Sie dann zur Laufzeit eine Ausnahme ausgelöst wird. Wenn dieser Code ausgeführt wird, die `PreferenceFragment` wird der folgende Screenshot zeigt angezeigt:

[![Screenshot des Beispiel-app mit PreferenceFragment angezeigt](specialized-fragment-classes-images/preference-fragment-getpreferencesfromintent.png)](specialized-fragment-classes-images/preference-fragment-getpreferencesfromintent.png)
