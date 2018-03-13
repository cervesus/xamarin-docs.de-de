---
title: Exemplarische Vorgehensweise
ms.topic: article
ms.prod: xamarin
ms.assetid: ED368FA9-A34E-DC39-D535-5C34C32B9761
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/15/2018
ms.openlocfilehash: e5c058f173f64efe4a5c777872e9ea67120115f0
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="walkthrough"></a>Exemplarische Vorgehensweise

In den folgenden Schritten wird eine einfache app mit Fragmenten erstellt. Der erste Schritt besteht darin eine neue Xamarin.Android für Android-Projekt zu erstellen.

## <a name="1-create-a-project"></a>1. Erstellen eines Projekts

Erstellen Sie ein neue Xamarin.Android Projekt mit der Bezeichnung **FragmentSample**. Die **mindestens Android** Version sollte festgelegt werden auf Android 3.1 oder höher, wie in der folgenden Abbildung gezeigt:

[![Festlegen der mindestens Android-version](walkthrough-images/00.png)](walkthrough-images/00.png#lightbox)


## <a name="2-create-the-mainactivity"></a>2. Erstellen Sie die Verwendung des Layoutnamens

Wir als Nächstes erstellen Sie die `MainActivity` Klasse. Hierbei handelt es sich um den Start-Aktivität für die Anwendung. Diese Aktivität wird eine oder beide Fragmente, je nach Größe des Bildschirms gehostet. `MainActivity` wird dazu Laden der Layoutdatei, die für das Gerät geeignet ist.

```csharp
[Activity(Label = "Fragments Walkthrough", MainLauncher = true, Icon = "@drawable/launcher")]
public class MainActivity : Activity
{
   protected override void OnCreate(Bundle bundle)
   {
       base.OnCreate(bundle);
       SetContentView(Resource.Layout.activity_main);
   }
}
```

> [!NOTE]
> `Note:` Unterklassen Fragment müssen einen öffentlichen Standard kein Argumentkonstruktor verfügen.

## <a name="3-create-the-layout-files"></a>3. Erstellen Sie das Layout-Dateien

Die zwei unterschiedlichen Bildschirmgrößen erfordern zwei anderes Layout-Dateien. Wir also erstellen Sie einen neuen Ordner **Ressourcen/Layout-große**, und erstellen Sie ein neues Layout aufgerufen **activity_main.axml**. Wir müssen auch umbenennen, heißt die Standard-Layout-Datei als **Resources/Layout/activity_main.axml**. Nach der Änderung sollte der Layout-Ordner im folgenden Screenshot entsprechen:

[![Screenshot des Layout-Ordner in der IDE](walkthrough-images/01.png)](walkthrough-images/01.png#lightbox)


Alle Geräte geladen und verwenden Sie die Layoutdatei in **Ressourcen/Layout**.
Es ist ein sehr einfaches Layout, das zeigt nur einen `TitlesFragment`:

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
       android:orientation="horizontal"
       android:layout_width="fill_parent"
       android:layout_height="fill_parent">
 <fragment class="com.xamarin.sample.fragments.supportlib.TitlesFragment"
           android:id="@+id/titles_fragment"
           android:layout_width="fill_parent"
           android:layout_height="fill_parent" />
</LinearLayout>
```

Für Geräte mit einem großen Bildschirm, lädt Android die Layoutdatei in **Ressourcen/Layout-große**. Der Inhalt des Layouts für Tablets lautet wie folgt:

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
       android:orientation="horizontal"
       android:layout_width="fill_parent"
       android:layout_height="fill_parent">
 <fragment class="com.xamarin.sample.fragments.supportlib.TitlesFragment"
           android:id="@+id/titles_fragment"
           android:layout_weight="1"
           android:layout_width="0px"
           android:layout_height="match_parent" />
 <FrameLayout android:id="@+id/details"
              android:layout_weight="1"
              android:layout_width="0px"
              android:layout_height="match_parent" />
</LinearLayout>
```

Die Layoutdatei für größere Bildschirme ist etwas anders. Ist nicht nur die `TitlesFragment` in dieser Layoutdatei angezeigt, aber eine `FrameLayout` wird rechts neben das Fragment hinzugefügt. Auf die größere Bildschirme die `DetailsFragment` programmgesteuert hinzugefügt `MainActivity` Wenn der Benutzer wählt eine Wiedergabe. Später erläutern wir die im Detail wie dies funktioniert.

Eine neue Methode zum Angeben des Bildschirmlayouts, Android 3.2 eingeführt. Diese neuen Qualifizierer geben an, die Menge des Speicherplatzes, der das Layout benötigt wird, anstatt die Größe des Bildschirms. Wenn diese Anwendung voneinander nur bei Android 3.2 oder höher ausgeführt wurde, erstellen wir eine **Ressource/Layout-sw600dp** Ordner (anstatt den Ordner **Ressource/Layout-große**) für die Layoutdatei  **activity_main.axml**. Von allen Geräten, die eine minimale Bildschirmbreite 600 Dichte typunabhängig Pixel haben, würden diese Ressourcendatei geladen werden. Allerdings wie diese Anwendung auf Ziel Android 3.1 festgelegt oder höher, verwendet der älteren ressourcenqualifizierer.

## <a name="4-create-the-titlesfragment"></a>4. Erstellen Sie die TitlesFragment

`TitlesFragment` Zeigen Sie die Titel von verschiedenen spielt, wir also fügen dem Projekt ein neues Fragment aufgerufen wird `TitlesFragment`:

[![Das TitlesFragment-Projekt hinzugefügt ein neues fragment](walkthrough-images/02.png)](walkthrough-images/02.png#lightbox)

Nach dem `TitlesFragment` wurde hinzugefügt, es muss die Klasse ändern, sodass es erbt `Android.App.ListFragment`. `ListFragment` ist ein spezielle Fragmenttyp, der Funktion enthält.
`TitlesFragment` überschreibt auch `OnActivityCreated` (ein anderes Fragment Lifecycle-Methode), und geben Sie eine `Adapter` , `ListFragment` zum Auffüllen der Liste verwendet wird:

```csharp
public override void OnActivityCreated(Bundle savedInstanceState)
{
   base.OnActivityCreated(savedInstanceState);
   var adapter = new ArrayAdapter<String>(Activity, Android.Resource.Layout.SimpleListItemChecked, Shakespeare.Titles);
   ListAdapter = adapter;
   if (savedInstanceState != null)
   {
       _currentPlayId = savedInstanceState.GetInt("current_play_id", 0);
   }
   var detailsFrame = Activity.FindViewById<View>(Resource.Id.details);
   _isDualPane = detailsFrame != null && detailsFrame.Visibility == ViewStates.Visible;
   if (_isDualPane)
   {
       ListView.ChoiceMode = (int) ChoiceMode.Single;
       ShowDetails(_currentPlayId);
   }
}
```

Wie bereits erwähnt wurde, hat die Anwendung zwei Layouts für `MainActivity`. Der Code in `OnActivityCreated` erkennt das Vorhandensein der `FrameLayout` und bestimmt, welche Layoutdatei geladen wurde. Wenn die `FrameLayout` vorhanden ist, in das Layout der `_isDualPane` Flag wird festgelegt, um `true`. Die `_isDualPane` Flag wird verwendet, an anderer Stelle in der Aktivität, insbesondere durch die `ShowDetails` Methode. Die `ShowDetails` Methode wird im folgenden ausführlicher erläutert werden.

`TitlesFragment` ist eine Liste, und reagieren muss, auf der Auswahl des Benutzers in der Liste. Hierzu `TitlesFragment` überschreiben Sie die Methode `OnListItemClick`. In `OnListItemClick`, ein neues `DetailsFragment` wird erstellt und angezeigt, der `FrameLayout`und programmgesteuert. Der relevante Code im `TitlesFragment` ist:

```csharp
public override void OnListItemClick(ListView l, View v, int position, long id)
{
   ShowDetails(position);
}
private void ShowDetails(int playId)
{
   _currentPlayId = playId;
   if (_isDualPane)
   {
       // We can display everything in place with fragments.
       // Have the list highlight this item and show the data.
       ListView.SetItemChecked(playId, true);
       // Check what fragment is shown, replace if needed.
       var details = FragmentManager.FindFragmentById(Resource.Id.details) as DetailsFragment;
       if (details == null || details.ShownPlayId != playId)
       {
           // Make new fragment to show this selection.
           details = DetailsFragment.NewInstance(playId);
           // Execute a transaction, replacing any existing
           // fragment with this one inside the frame.
           var ft = FragmentManager.BeginTransaction();
           ft.Replace(Resource.Id.details, details);
           ft.SetTransition(FragmentTransaction.TransitFragmentFade);
           ft.Commit();
       }
   }
   else
   {
       // Otherwise we need to launch a new Activity to display
       // the dialog fragment with selected text.
       var intent = new Intent();
       intent.SetClass(Activity, typeof (DetailsActivity));
       intent.PutExtra("current_play_id", playId);
       StartActivity(intent);
   }
}
```

Der Code ermittelt vom Gerät zum Formatieren und Anzeigen von die anführung wie in den ausgewählten wiedergeben. Im Fall von Tablet-PCs die `_isDualPane` Flag wird festgelegt `true`, und somit neben das Angebot angezeigt wird der `TitlesFragment`. Wenn die ausgewählten Play `id` nicht bereits angezeigt wird, klicken Sie dann ein neues `DetailsFragment` erstellt werden, und klicken Sie dann in geladen der `FrameLayout` für die Aktivität. Bei anderen Geräten, auf denen eine große Anzeige kein &ndash; Telefone, z. B. &ndash; `isDualPane` festgelegt, um `false` daher ein neues `DetailsActivity` gestartet wird.


## <a name="5-create-the-detailsactivity"></a>5. Erstellen Sie die DetailsActivity

Die `DetailsActivity` zeigt die `DetailsFragment` für kleinere Geräte. Um dies zu sehen, zunächst wir fügen eine neue Aktivität auf das Projekt mit dem Namen `DetailsActivity`. `DetailsActivity` ist eine sehr einfache Aktivität an. Sie erstellen und dann einen neuen host `DetailsFragment` für die Wiedergabe `id` , wurde gesendet:

```csharp
[Activity(Label = "Details Activity")]
public class DetailsActivity : Activity
{
   protected override void OnCreate(Bundle bundle)
   {
       base.OnCreate(bundle);
       var index = Intent.Extras.GetInt("current_play_id", 0);

       var details = DetailsFragment.NewInstance(index); // DetailsFragment.NewInstance is a factory method to create a Details Fragment
       var fragmentTransaction = FragmentManager.BeginTransaction();
       fragmentTransaction.Add(Android.Resource.Id.Content, details);
       fragmentTransaction.Commit();
   }
}
```

Beachten Sie, die keine Layoutdatei für geladen wird `DetailsActivity`. Stattdessen `DetailsFragment` wird in der Stammansicht der Aktivität geladen. In dieser Stammansicht hat die spezielle ID `Android.Resource.Id.Content`. Ein neues `DetailFragment` wird erstellt, und klicken Sie dann auf diese Sicht Root innerhalb eines hinzugefügt eine `FragmentTransaction` werden von der Aktivitäts erstellt `FragmentManager`.


## <a name="6-create-the-detailsfragment"></a>6. Erstellen Sie die DetailsFragment

Nun fügen Sie ein anderes Fragment an die Anwendung mit dem Namen `DetailsFragment`. Dieses Fragment wird ein Angebot aus dem ausgewählten Play angezeigt. Der folgende Code zeigt die vollständige `DetailsFragment`:

```csharp
internal class DetailsFragment : Fragment
{
   public static DetailsFragment NewInstance(int playId)
   {
       var detailsFrag = new DetailsFragment {Arguments = new Bundle()};
       detailsFrag.Arguments.PutInt("current_play_id", playId);
       return detailsFrag;
   }
   public int ShownPlayId
   {
       get { return Arguments.GetInt("current_play_id", 0); }
   }
   public override View OnCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState)
   {
       if (container == null)
       {
           // Currently in a layout without a container, so no reason to create our view.
           return null;
       }
       var scroller = new ScrollView(Activity);
       var text = new TextView(Activity);
       var padding = Convert.ToInt32(TypedValue.ApplyDimension(ComplexUnitType.Dip, 4, Activity.Resources.DisplayMetrics));
       text.SetPadding(padding, padding, padding, padding);
       text.TextSize = 24;
       text.Text = Shakespeare.Dialogue[ShownPlayId];
       scroller.AddView(text);
       return scroller;
   }
}
```

In der Reihenfolge für `DetailsFragment` um ordnungsgemäß zu funktionieren, muss den Index des wiedergeben, die ausgewählt wurde die `TitlesFragment`. Es gibt viele Möglichkeiten, diesen Wert auf bereitzustellen `DetailsFragment`; in diesem Beispiel wird die Wiedergabe `Id` befindet sich in einem Paket Paket gespeicherten, Arguments-Eigenschaft einer Instanz von der `DetailsFragment`. Die Eigenschaft `ShownPlayId` wird der Einfachheit halber bereitgestellt &ndash; werden durch Instanzen der kann `DetailsFragment` zum Abrufen dieses Werts aus dem Paket.

`OnCreateView` wird aufgerufen, wenn das Fragment die Benutzeroberfläche zeichnen muss und zurückgeben sollte ein `Android.Views.View` Objekt. In den meisten Fällen ist dies ein `View` aus einer vorhandenen Layoutdatei vergrößert. Im obigen Beispiel Fall erstellen das Fragment programmgesteuert der Sicht, die für die Anzeige verwendet werden.

Herzlichen Glückwunsch! Sie haben jetzt eine Anwendung erstellt, die Fragmente zur Vereinfachung der Entwicklung über Formfaktoren verwendet.

In der [nächsten Abschnitt](supporting-pre-honeycomb.md), wir werden diese Anwendung zu erweitern, sodass auf Geräten vor Android 3.0 geeignet sind.

