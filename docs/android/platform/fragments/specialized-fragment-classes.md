---
title: Spezielle Fragmentklassen
ms.prod: xamarin
ms.assetid: 7A0AEB2C-EE77-63BF-652A-DA049B691C64
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/08/2018
ms.openlocfilehash: b004fbf121374a2bb3bf5d85f45d8cae293573bf
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2020
ms.locfileid: "73027315"
---
# <a name="specialized-fragment-classes"></a>Spezielle Fragmentklassen

Die Fragment-API stellt weitere Unterklassen zur Verfügung, die einige der häufigeren Funktionen in Anwendungen kapseln. Diese Unterklassen sind:

- **ListFragment** &ndash; Dieses Fragment wird verwendet, um eine Liste von Elementen anzuzeigen, die an eine Datenquelle gebunden sind, z. B. an ein Array oder einen Cursor.

- **DialogFragment** &ndash; Dieses Fragment wird als Wrapper für ein Dialogfeld verwendet. Das Fragment zeigt das Dialogfeld über seiner Aktivität an.

- **PreferenceFragment** &ndash; Dieses Fragment wird verwendet, um Einstellungsobjekte als Listen anzuzeigen.

## <a name="the-listfragment"></a>ListFragment

`ListFragment` ähnelt hinsichtlich Konzept und Funktionalität `ListActivity`. Dabei handelt es sich um einen Wrapper, der eine `ListView` in einem Fragment hostet. Die folgende Abbildung zeigt ein `ListFragment`, das auf einem Tablet und einem Smartphone ausgeführt wird:

[![Screenshots von ListFragment auf einem Tablet und einem Smartphone](specialized-fragment-classes-images/intro-screenshot-sml.png)](specialized-fragment-classes-images/intro-screenshot.png#lightbox)

### <a name="binding-data-with-the-listadapter"></a>Binden von Daten mit ListAdapter

Die `ListFragment`-Klasse stellt bereits ein Standardlayout bereit. Daher ist es nicht erforderlich, `OnCreateView` zu überschreiben, um den Inhalt von `ListFragment` anzuzeigen. `ListView` wird mithilfe einer `ListAdapter`-Implementierung an Daten gebunden. Das folgende Beispiel zeigt, wie dies mithilfe eines einfachen Arrays von Zeichenfolgen durchgeführt werden kann:

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

Wenn Sie `ListAdapter` festlegen, ist es wichtig, die `ListFragment.ListAdapter`-Eigenschaft und nicht die `ListView.ListAdapter`-Eigenschaft zu verwenden. Die Verwendung von `ListView.ListAdapter` bewirkt, dass wichtiger Initialisierungscode übersprungen wird.

### <a name="responding-to-user-selection"></a>Reagieren auf die Benutzerauswahl

Um auf die Benutzerauswahl zu reagieren, muss eine Anwendung die `OnListItemClick`-Methode überschreiben. Das folgende Beispiel zeigt eine solche Möglichkeit:

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

Wenn der Benutzer im Code oben ein Element in `ListFragment` auswählt, wird ein neues Fragment in der Hostingaktivität angezeigt, das weitere Details zum ausgewählten Element anzeigt.

## <a name="dialogfragment"></a>DialogFragment

*DialogFragment* ist ein Fragment, das verwendet wird, um ein Dialogobjekt innerhalb eines Fragments anzuzeigen, das über dem Fenster der Aktivität schwebt. Es soll die verwalteten Dialogfeld-APIs (ab Android 3.0) ersetzen. Der folgende Screenshot zeigt ein Beispiel für ein `DialogFragment`:

[![Screenshot von DialogFragment, das das EditBox-Element „Neues Fahrzeug hinzufügen“ anzeigt](specialized-fragment-classes-images/dialog-fragment-example.png)](specialized-fragment-classes-images/dialog-fragment-example.png#lightbox)

Mit einem `DialogFragment` wird sichergestellt, dass der Zustand zwischen dem Fragment und dem Dialogfeld konsistent bleibt. Alle Interaktionen und die Steuerung des Dialogfeldobjekts sollten über die `DialogFragment`-API erfolgen und nicht mit direkten Aufrufen des Dialogfeldobjekts vorgenommen werden. Die `DialogFragment`-API stellt für jede Instanz eine `Show()`-Methode bereit, die zum Anzeigen eines Fragments verwendet wird. Es gibt zwei Möglichkeiten, ein Fragment zu entfernen:

- Aufrufen von `DialogFragment.Dismiss()` für die `DialogFragment`-Instanz. 

- Anzeigen eines anderen `DialogFragment`.

Zum Erstellen eines `DialogFragment` erbt eine Klasse von `Android.App.DialogFragment,` und überschreibt dann eine der beiden folgenden Methoden:

- **OnCreateView** &ndash; Eine Sicht wird erstellt und zurückgegeben.

- **OnCreateDialog** &ndash; Erstellt ein benutzerdefiniertes Dialogfeld. Wird in der Regel verwendet, um einen *AlertDialog* anzuzeigen. Beim Überschreiben dieser Methode ist es nicht erforderlich, `OnCreateView` zu überschreiben.

### <a name="a-simple-dialogfragment"></a>Ein einfaches DialogFragment

Der folgende Screenshot zeigt ein einfaches `DialogFragment`, das über eine `TextView` und zwei `Button`s verfügt:

[![Beispiel-DialogFragment mit einer TextView und zwei Schaltflächen](specialized-fragment-classes-images/dialog-fragment-example-2.png)](specialized-fragment-classes-images/dialog-fragment-example-2.png#lightbox)

`TextView` zeigt an, wie oft der Benutzer auf eine Schaltfläche im `DialogFragment` geklickt hat, während durch Klicken auf die andere Schaltfläche das Fragment geschlossen wird. Der Code für `DialogFragment` lautet:

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

Wie alle Fragmente wird ein `DialogFragment` im Kontext einer `FragmentTransaction` angezeigt.

Die `Show()`-Methode für ein `DialogFragment` nimmt ein `FragmentTransaction`-Element und ein `string`-Element als Eingabe an. Das Dialogfeld wird der-Aktivität hinzugefügt, und die `FragmentTransaction` wird committet.

Der folgende Code veranschaulicht eine mögliche Methode, mit der eine Aktivität die `Show()`-Methode zum Anzeigen eines `DialogFragment` verwenden kann:

```csharp
public void ShowDialog()
{
    var transaction = FragmentManager.BeginTransaction();
    var dialogFragment = new MyDialogFragment();
    dialogFragment.Show(transaction, "dialog_fragment");
}
```

### <a name="dismissing-a-fragment"></a>Verwerfen eines Fragments

Das Aufrufen von `Dismiss()` für eine Instanz eines `DialogFragment` bewirkt, dass ein Fragment aus der Aktivität entfernt und ein Commit für diese Transaktion ausgeführt wird.
Die Fragment-Lebenszyklus-Standardmethoden, die am Verwerfen eines Fragments beteiligt sind, werden aufgerufen.

### <a name="alert-dialog"></a>Warnungsdialogfeld

Anstatt `OnCreateView` zu überschreiben, kann ein `DialogFragment` stattdessen `OnCreateDialog` überschreiben. Dies ermöglicht es der Anwendung, ein [AlertDialog](xref:Android.App.AlertDialog)-Element zu erstellen, das von einem Fragment verwaltet wird. Der folgende Code ist ein Beispiel, das `AlertDialog.Builder` zum Erstellen eines `Dialog`-Elements verwendet:

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

Zur Unterstützung der Verwaltung von Einstellungen stellt die Fragment-API die `PreferenceFragment`-Unterklasse bereit. `PreferenceFragment` ähnelt [PreferenceActivity](xref:Android.Preferences.PreferenceActivity) &ndash; Es wird eine Hierarchie von Einstellungen für den Benutzer in einem Fragment anzeigt. Wenn der Benutzer mit den Einstellungen interagiert, werden diese automatisch in [SharedPreferences](https://developer.android.com/reference/android/content/SharedPreferences.html) gespeichert.
Verwenden Sie in Android 3.0-Anwendungen oder höher `PreferenceFragment`, um Einstellungen in Anwendungen zu verarbeiten. Die folgende Abbildung zeigt ein Beispiel für ein `PreferenceFragment`:

[![Beispiel-PreferenceFragment mit Inline-, Dialogfeld- und Starteinstellungen](specialized-fragment-classes-images/preferences-dialog.png)](specialized-fragment-classes-images/preferences-dialog.png#lightbox)

### <a name="create-a-preference-fragment-from-a-resource"></a>Erstellen eines PreferenceFragment aus einer Ressource

Das PreferenceFragment kann mithilfe der [PreferenceFragment.AddPreferencesFromResource](xref:Android.Preferences.PreferenceFragment.AddPreferencesFromResource*)-Methode aus einer XML-Ressourcendatei aufgefüllt werden. Ein logischer Ort zum Aufrufen dieser Methode im Lebenszyklus des Fragments wäre in der `OnCreate`-Methode.

Das oben gezeigte `PreferenceFragment` wurde durch Laden einer Ressource aus XML erstellt. Die Ressourcendatei ist:

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

Der Code für das PreferenceFragment lautet wie folgt:

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

### <a name="querying-activities-to-create-a-preference-fragment"></a>Abfragen von Aktivitäten zum Erstellen eines PreferenceFragment

Ein weiteres Verfahren zum Erstellen eines `PreferenceFragment` umfasst das Abfragen von Aktivitäten. Jede Aktivität kann das [METADATA\_KEY\_PREFERENCE](xref:Android.Preferences.PreferenceManager.MetadataKeyPreferences)-Attribut verwenden, das auf eine XML-Ressourcendatei verweist. In Xamarin.Android wird dies durch das Versehen einer Aktivität mit `MetaDataAttribute` und das anschließende Angeben der zu verwendenden Ressourcendatei erreicht. Die `PreferenceFragment`-Klasse stellt die Methode [AddPreferenceFromIntent](xref:Android.Preferences.PreferenceFragment.AddPreferencesFromIntent*) bereit, die verwendet werden kann, um eine Aktivität zum Auffinden dieser XML-Ressource abzufragen und eine Einstellungshierarchie für diese aufzufüllen.

Ein Beispiel für diesen Vorgang finden Sie im folgenden Codeausschnitt, der `AddPreferencesFromIntent` verwendet, um ein `PreferenceFragment` zu erstellen:

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

Android verwendet die Klasse `MyActivityWithPreference`. Die Klasse muss mit dem `MetaDataAttribute,` versehen werden, wie im folgenden Codeausschnitt gezeigt:

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

`MetaDataAttribute` deklariert eine XML-Ressourcendatei, die von `PreferenceFragment` verwendet wird, um die Einstellungshierarchie aufzufüllen. Wenn `MetatDataAttribute` nicht angegeben wird, wird zur Laufzeit eine Ausnahme ausgelöst. Bei Ausführung dieses Codes wird `PreferenceFragment` wie im folgenden Screenshot angezeigt:

[![Screenshot einer Beispiel-App mit angezeigtem PreferenceFragment](specialized-fragment-classes-images/preference-fragment-getpreferencesfromintent.png)](specialized-fragment-classes-images/preference-fragment-getpreferencesfromintent.png#lightbox)
