---
title: Spezielle Fragmentklassen
ms.prod: xamarin
ms.assetid: 7A0AEB2C-EE77-63BF-652A-DA049B691C64
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/08/2018
ms.openlocfilehash: 1011d74be971a3acba33c8f2f811e8f89e20cfc4
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50108443"
---
# <a name="specialized-fragment-classes"></a>Spezielle Fragmentklassen

Die Fragmente-API bietet andere Unterklassen, die einige der häufigeren Funktionalitäten in Anwendungen zu kapseln. Diese Unterklassen sind:

-   **ListFragment** &ndash; dieses Fragment wird zum Anzeigen einer Liste von Elementen, die an eine Datenquelle wie z. B. ein Array oder ein Cursor gebunden.

-   **DialogFragment** &ndash; dieses Fragment dient als Wrapper für ein Dialogfeld. Das Fragment zeigt das Dialogfeld auf die Aktivität.

-   **PreferenceFragment** &ndash; dieses Fragment wird verwendet, um die Einstellung Objekte als Liste angezeigt werden.



## <a name="the-listfragment"></a>Die ListFragment

Die `ListFragment` ähnelt Konzept und Funktionen für die `ListActivity`; es ist ein Wrapper, der hostet eine `ListView` in einem Fragment. Die folgende Abbildung zeigt eine `ListFragment` auf einem Tablet und einem Smartphone ausgeführt wird:

[![Screenshots der ListFragment auf einem Tablet und auf einem Smartphone](specialized-fragment-classes-images/intro-screenshot-sml.png)](specialized-fragment-classes-images/intro-screenshot.png#lightbox)


### <a name="binding-data-with-the-listadapter"></a>Binden von Daten mit der ListAdapter

Die `ListFragment` -Klasse ermöglicht es bereits ein Standardlayout, daher es nicht erforderlich ist, außer Kraft setzen `OnCreateView` zum Anzeigen der Inhalte der `ListFragment`. Die `ListView` an Daten gebunden ist, die mit einem `ListAdapter` Implementierung. Das folgende Beispiel zeigt, wie dies mit einem einfachen Array aus Zeichenfolgen ausgeführt werden konnte:

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

Beim Festlegen der `ListAdapter`, es ist wichtig, verwenden Sie die `ListFragment.ListAdapter` -Eigenschaft, und nicht die `ListView.ListAdapter` Eigenschaft. Mithilfe von `ListView.ListAdapter` bewirkt, dass wichtige Initialisierungscodes übersprungen werden soll.



### <a name="responding-to-user-selection"></a>Reagieren auf Benutzerauswahl

Um auf der Auswahl des Benutzers reagieren, eine Anwendung außer Kraft setzen muss die `OnListItemClick` Methode. Das folgende Beispiel zeigt eine solche Möglichkeit:

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

Im Code über, wenn der Benutzer ein Element auswählt der `ListFragment`, ein neues Fragment wird angezeigt, in der hostenden Aktivität, zeigen Weitere Details zu dem Element, das ausgewählt wurde.



## <a name="dialogfragment"></a>DialogFragment

Die *DialogFragment* ist ein Fragment, das verwendet wird, um ein-Objekt eines Dialogfelds in einem Fragment anzuzeigen, die über der Aktivität Fenster verschoben wird. Es ist vorgesehen, um das Dialogfeld "verwalteten" (beginnend in Android 3.0) APIs zu ersetzen. Der folgende Screenshot zeigt ein Beispiel für eine `DialogFragment`:

[![Screenshot der DialogFragment Hinzufügen eines neuen Fahrzeug EditBox anzeigen](specialized-fragment-classes-images/dialog-fragment-example.png)](specialized-fragment-classes-images/dialog-fragment-example.png#lightbox)

Ein `DialogFragment` wird sichergestellt, dass der Zustand zwischen dem Fragment und das Dialogfeld konsistent bleiben. Alle Interaktionen und Kontrolle über das Dialogfeldobjekt auftreten sollte, über die `DialogFragment` -API und nicht mit direkten aufrufen, die für das Dialogfeldobjekt hergestellt werden. Die `DialogFragment` -API bietet jede Instanz mit einem `Show()` -Methode, die verwendet wird, um ein Fragment anzuzeigen. Es gibt zwei Möglichkeiten, um ein Fragment zu beheben:

-  Rufen Sie `DialogFragment.Dismiss()` auf die `DialogFragment` Instanz. 

-  Zeigen Sie eine andere `DialogFragment`.

Zum Erstellen einer `DialogFragment`, eine Klasse erbt von `Android.App.DialogFragment,` und überschreibt dann eine der beiden folgenden Methoden:

- **OnCreateView** &ndash; erstellt und gibt eine Ansicht zurück.

- **OnCreateDialog** &ndash; Dadurch wird ein benutzerdefiniertes Dialogfeld erstellt. Es dient normalerweise zum Anzeigen einer *AlertDialog*. Wenn Sie diese Methode überschreiben, ist es nicht erforderlich, außer Kraft setzen `OnCreateView` .



### <a name="a-simple-dialogfragment"></a>Eine einfache DialogFragment

Der folgende Screenshot zeigt ein einfaches `DialogFragment` , bei dem ein `TextView` und zwei `Button`s:

[![Beispiel-DialogFragment mit einem TextView und zwei Schaltflächen](specialized-fragment-classes-images/dialog-fragment-example-2.png)](specialized-fragment-classes-images/dialog-fragment-example-2.png#lightbox)

Die `TextView` zeigt die Anzahl der Fälle, in denen der Benutzer eine Schaltfläche geklickt hat die `DialogFragment`, während Sie auf der anderen Schaltfläche das Fragment geschlossen wird. Der Code für `DialogFragment` ist:

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

Alle Fragmente, wie eine `DialogFragment` wird im Rahmen einer `FragmentTransaction`.

Die `Show()` Methode für eine `DialogFragment` nimmt eine `FragmentTransaction` und `string` als Eingabe. Das Dialogfeld für die Aktivität, hinzugefügt und die `FragmentTransaction` ein Commit ausgeführt.

Der folgende Code veranschaulicht eine Möglichkeit, die eine Aktivität können Sie die `Show()` Methode zum Anzeigen einer `DialogFragment`:

```csharp
public void ShowDialog()
{
    var transaction = FragmentManager.BeginTransaction();
    var dialogFragment = new MyDialogFragment();
    dialogFragment.Show(transaction, "dialog_fragment");
}
```


### <a name="dismissing-a-fragment"></a>Schließen ein Fragment

Aufrufen von `Dismiss()` auf einer Instanz von einem `DialogFragment` bewirkt, dass ein Fragment, das aus der Aktivität entfernt werden, und führt einen Commit dieser Transaktion.
Die standardmäßigen Fragment-Lifecycle-Methoden, die mit der Zerstörung eines Fragments beteiligt sind, werden aufgerufen werden.


### <a name="alert-dialog"></a>Dialogfeld "Warnung"

Anstelle von `OnCreateView`, `DialogFragment` kann stattdessen überschreiben `OnCreateDialog`. Dies ermöglicht einer Anwendung zum Erstellen einer [AlertDialog](https://developer.xamarin.com/api/type/Android.App.AlertDialog/) , die von einem Fragment verwaltet wird. Der folgende Code ist ein Beispiel, verwendet der `AlertDialog.Builder` zum Erstellen einer `Dialog`:

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

Damit die Einstellungen zu verwalten, die Fragmente-API bietet die `PreferenceFragment` Unterklasse. Die `PreferenceFragment` ist vergleichbar mit der [PreferenceActivity](https://developer.xamarin.com/api/type/Android.Preferences.PreferenceActivity/) &ndash; eine Hierarchie von Einstellungen für den Benutzer in einem Fragment wird angezeigt. Wie die Einstellungen der Benutzer interagiert, sie werden automatisch gespeichert [SharedPreferences](http://developer.android.com/reference/android/content/SharedPreferences.html).
Verwenden Sie in Android 3.0 oder höhere Anwendungen, die `PreferenceFragment` für den Umgang mit den Einstellungen in Anwendungen. Die folgende Abbildung zeigt ein Beispiel für eine `PreferenceFragment`:

[![Beispiel-PreferencesFragment mit Inline, Dialogfelder und Start-Einstellungen](specialized-fragment-classes-images/preferences-dialog.png)](specialized-fragment-classes-images/preferences-dialog.png#lightbox)


### <a name="create-a-preference-fragment-from-a-resource"></a>Erstellen Sie ein Preference-Fragment aus einer Ressource

Die Einstellung Fragment aus einer XML-Ressourcendatei kann, mithilfe vergrößert werden der [PreferenceFragment.AddPreferencesFromResource](https://developer.xamarin.com/api/member/Android.Preferences.PreferenceFragment.AddPreferencesFromResource/p/System.Int32/) Methode. Ein logischer Ansatzpunkt zum Aufrufen dieser Methode im Lebenszyklus des Fragments wird in der `OnCreate` Methode.

Die `PreferenceFragment` dargestellten oben erstellt wurde, laden Sie eine Ressource aus XML. Die Ressourcendatei ist:

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

Der Code für den Preference-Fragment lautet wie folgt aus:

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



### <a name="querying-activities-to-create-a-preference-fragment"></a>Abfragen von Aktivitäten, die ein Fragment Einstellung erstellen

Ein weiteres Verfahren zum Erstellen einer `PreferenceFragment` Abfragen Aktivitäten beinhaltet. Jede Aktivität können die [Metadaten\_Schlüssel\_Einstellung](https://developer.xamarin.com/api/field/Android.Preferences.PreferenceManager.MetadataKeyPreferences/) -Attribut, das auf einer XML-Ressourcendatei verweist. In Xamarin.Android Dies erfolgt durch eine Aktivität mit dem Verzieren der `MetaDataAttribute`, und anschließend die, die zu verwendende Ressourcendatei anzugeben. Die `PreferenceFragment` Klasse stellt die Methode [AddPreferenceFromIntent](https://developer.xamarin.com/api/member/Android.Preferences.PreferenceFragment.AddPreferencesFromIntent/p/Android.Content.Intent/)), die verwendet werden kann, um eine Aktivität zum Suchen von dieser XML-Ressource, und eine Hierarchie Einstellung dafür Vergrößerung abzufragen.

Ein Beispiel dieses Prozesses finden Sie im folgenden Codeausschnitt, der verwendet `AddPreferencesFromIntent` zum Erstellen einer `PreferenceFragment`:

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

Android sieht sich die Klasse `MyActivityWithPreference`. Die Klasse versehen werden muss, mit der `MetaDataAttribute,` wie im folgenden Codeausschnitt gezeigt:

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

Die `MetaDataAttribute` eine XML-Ressourcendatei wird deklariert, dass die `PreferenceFragment` verwenden, um die Vergrößerung der Preference-Hierarchie. Wenn die `MetatDataAttribute` nicht angegeben wird, und klicken Sie dann zur Laufzeit eine Ausnahme ausgelöst wird. Wenn dieser Code ausgeführt wird, die `PreferenceFragment` angezeigt wird, wie im folgenden Screenshot dargestellt:

[![Screenshot der Beispiel-app mit PreferenceFragment angezeigt](specialized-fragment-classes-images/preference-fragment-getpreferencesfromintent.png)](specialized-fragment-classes-images/preference-fragment-getpreferencesfromintent.png#lightbox)
