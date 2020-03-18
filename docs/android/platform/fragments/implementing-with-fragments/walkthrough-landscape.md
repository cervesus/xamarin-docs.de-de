---
title: 'Exemplarische Vorgehensweise zu Fragmenten: Teil 2'
ms.prod: xamarin
ms.topic: tutorial
ms.assetid: 444A894D-5197-4726-934F-79BA80A71CB0
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 04/26/2018
ms.openlocfilehash: 4d9ef88f39914f8fa5e578577ee9f6977c2bc88e
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2020
ms.locfileid: "73020266"
---
# <a name="fragments-walkthrough-ndash-landscape"></a>Exemplarische Vorgehensweise zu Fragmenten: Querformat

Im [ersten Teil zur exemplarischen Vorgehensweise zu Fragmenten](./walkthrough.md) wurde erläutert, wie Fragmente in Android-Apps erstellt und verwendet werden, die auf den kleineren Bildschirmen eines Smartphones ausgeführt werden. Der nächste Schritt dieser exemplarischen Vorgehensweise besteht darin, die Anwendung so anzupassen, dass sie den horizontalen Raum, der auf einem Tablet zusätzlich zur Verfügung steht, ausnutzen kann. Es wird dazu eine Aktivität verwendet, eine dauerhaft angezeigte Liste von Theaterstücken (`TitlesFragment`), und `PlayQuoteFragment` wird der Aktivität dynamisch als Reaktion auf eine vom Benutzer vorgenommene Auswahl hinzugefügt:

[![Auf einem Tablet ausgeführte App](./walkthrough-landscape-images/01-tablet-screenshot-sml.png)](./walkthrough-landscape-images/01-tablet-screenshot.png#lightbox)

Smartphones, die im Querformat ausgeführt werden, profitieren ebenfalls von dieser Optimierung:

[![App, die auf einem Android-Smartphone im Querformatmodus ausgeführt wird](./images/intro-screenshot-phone-land-sml.png)](./images/intro-screenshot-phone-land.png#lightbox)

## <a name="updating-the-app-to-handle-landscape-orientation"></a>Aktualisieren der App zur Verarbeitung der Ausrichtung im Querformatmodus

Die folgenden Anpassungen bauen auf den in der [exemplarischen Vorgehensweise zu Fragmenten: Smartphone](./walkthrough.md) vorgenommenen Änderungen auf.

1. Erstellen Sie ein alternatives Layout, damit sowohl `TitlesFragment` als auch `PlayQuoteFragment` angezeigt werden können.
1. Aktualisieren Sie `TitlesFragment`, um festzustellen, ob das Gerät beide Fragmente gleichzeitig anzeigt, und ändern Sie das Verhalten entsprechend.
1. Aktualisieren Sie `PlayQuoteActivity`, sodass PlayQuoteActivity geschlossen wird, wenn sich das Gerät im Querformatmodus befindet.

## <a name="1-create-an-alternate-layout"></a>1. Erstellen eines alternativen Layouts

Wenn die Hauptaktivität auf einem Android-Gerät erstellt wird, entscheidet Android basierend auf der Ausrichtung des Geräts, welches Layout geladen wird. Standardmäßig stellt Android die Layoutdatei **Resources/layout/activity_main.axml** bereit. Für Geräte, die im Querformatmodus geladen werden, stellt Android die Layoutdatei **Resources/layout-land/activity_main.axml** bereit. Die Anleitung zu [Android-Ressourcen](/xamarin/android/app-fundamentals/resources-in-android) enthält weitere Details dazu, wie Android entscheidet, welche Ressourcendateien für eine Anwendung geladen werden.

Erstellen Sie ein alternatives Layout für die Ausrichtung im **Querformat**, indem Sie die in der Anleitung zu [Alternativen Layouts](/xamarin/android/user-interface/android-designer/alternative-layout-views) beschriebenen Schritte befolgen. Dadurch sollte dem Projekt eine neue Layoutressourcendatei (**Resources/layout/activity_main.axml**) hinzugefügt werden:

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[![Alternatives Layout im Projektmappen-Explorer](./walkthrough-landscape-images/02-alternate-layout.w157-sml.png)](./walkthrough-landscape-images/02-alternate-layout.w157.png#lightbox)

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

[![Alternatives Layout im Lösungspad](./walkthrough-landscape-images/02-alternate-layout.m743-sml.png)](./walkthrough-landscape-images/02-alternate-layout.m743.png#lightbox)

-----

Bearbeiten Sie nach Erstellung des alternativen Layouts die Quelle der Datei (**Resources/layout-land/activity_main.axml**), sodass sie dieser XML-Datei entspricht:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/two_fragments_layout"
    android:orientation="horizontal"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <fragment android:name="FragmentSample.TitlesFragment"
        android:id="@+id/titles"
        android:layout_weight="1"
        android:layout_width="0px"
        android:layout_height="match_parent" />

    <FrameLayout android:id="@+id/playquote_container"
            android:layout_weight="1"
            android:layout_width="0px"
            android:layout_height="match_parent"
             />
</LinearLayout>
```

Die Stammansicht der Aktivität wird von der Ressourcen-ID `two_fragments_layout` zur Verfügung gestellt und verfügt über zwei untergeordnete Ansichten, `fragment` und `FrameLayout`. Während `fragment` statisch geladen wird, dient `FrameLayout` als „Platzhalter“, der zur Laufzeit durch `PlayQuoteFragment` ersetzt wird. Sobald in `TitlesFragment` ein neues Theaterstück ausgewählt wird, wird `playquote_container` mit einer neuen Instanz von `PlayQuoteFragment` aktualisiert.

Alle untergeordneten Ansichten nutzen die komplette Höhe der übergeordneten Ansicht. Die Breite der einzelnen untergeordneten Ansichten wird durch die `android:layout_weight`- und `android:layout_width`-Attribute gesteuert. In diesem Beispiel nutzen die einzelnen untergeordneten Ansichten 50 Prozent der Breite der übergeordnete Ansicht. In diesem [Artikel von Google zu LinearLayout](https://developer.android.com/guide/topics/ui/layout/linear.html) finden Sie weitere Informationen zur _Layoutgewichtung_.

## <a name="2-changes-to-titlesfragment"></a>2. Änderungen an TitlesFragment

Sobald das alternative Layout erstellt wurde, muss `TitlesFragment` aktualisiert werden. Wenn die App die beiden Fragmente für eine Aktivität anzeigt, sollte `TitlesFragment` die `PlayQuoteFragment`-Klasse in der übergeordneten Aktivität laden. Andernfalls sollte die `TitlesFragment`-Klasse `PlayQuoteActivity` starten. In PlayQuoteActivity wird `PlayQuoteFragment` gehostet. Mithilfe eines booleschen Flags kann `TitlesFragment` bestimmen, welches Verhalten verwendet werden soll. Dieses Flag wird in der `OnActivityCreated`-Methode initialisiert.

Fügen Sie zunächst oben in der `TitlesFragment`-Klasse eine Instanzvariable hinzu:

```csharp
bool showingTwoFragments;
```

Fügen Sie dann in `OnActivityCreated` den folgenden Codeausschnitt hinzu, um die Variable zu initialisieren: 

```csharp
var quoteContainer = Activity.FindViewById(Resource.Id.playquote_container);
showingTwoFragments = quoteContainer != null &&
                      quoteContainer.Visibility == ViewStates.Visible;
if (showingTwoFragments)
{
    ListView.ChoiceMode = ChoiceMode.Single;
    ShowPlayQuote(selectedPlayId);
}
```

Wenn das Gerät im Querformatmodus ausgeführt wird, wird `FrameLayout` mit der Ressourcen-ID `playquote_container` auf dem Bildschirm angezeigt, `showingTwoFragments` wird also als `true` initialisiert. Wenn das Gerät im Hochformatmodus ausgeführt wird, wird `playquote_container` nicht auf dem Bildschirm angezeigt, für `showingTwoFragments` gilt also `false`.

Die `ShowPlayQuote`-Methode muss anpassen, wie ein Zitat angezeigt wird &ndash; entweder in einem Fragment, oder indem eine neue Aktivität gestartet wird.  Aktualisieren Sie die `ShowPlayQuote`-Methode, sodass ein Fragment geladen wird, wenn zwei Fragmente angezeigt werden. Andernfalls soll eine Aktivität gestartet werden:

```csharp
void ShowPlayQuote(int playId)
{
    selectedPlayId = playId;
    if (showingTwoFragments)
    {
        ListView.SetItemChecked(selectedPlayId, true);

        var playQuoteFragment = FragmentManager.FindFragmentById(Resource.Id.playquote_container) as PlayQuoteFragment;

        if (playQuoteFragment == null || playQuoteFragment.PlayId != playId)
        {
            var container = Activity.FindViewById(Resource.Id.playquote_container);
            var quoteFrag = PlayQuoteFragment.NewInstance(selectedPlayId);

            FragmentTransaction ft = FragmentManager.BeginTransaction();
            ft.Replace(Resource.Id.playquote_container, quoteFrag);
            ft.Commit();
        }
    }
    else
    {
        var intent = new Intent(Activity, typeof(PlayQuoteActivity));
        intent.PutExtra("current_play_id", playId);
        StartActivity(intent);
    }
}
```

Wenn der Benutzer ein anderes als das aktuell in `PlayQuoteFragment` angezeigte Theaterstück auswählt, wird ein neues `PlayQuoteFragment` erstellt, und es ersetzt die Inhalte von `playquote_container` im Kontext von `FragmentTransaction`.

### <a name="complete-code-for-titlesfragment"></a>Vollständiger Code für TitlesFragment

Nachdem alle vorherigen Änderungen an `TitlesFragment` abgeschlossen sind, sollte die vollständige Klasse dem folgenden Code entsprechen:

```csharp
public class TitlesFragment : ListFragment
{
    int selectedPlayId;
    bool showingTwoFragments;

    public override void OnActivityCreated(Bundle savedInstanceState)
    {
        base.OnActivityCreated(savedInstanceState);
        ListAdapter = new ArrayAdapter<string>(Activity, Android.Resource.Layout.SimpleListItemActivated1, Shakespeare.Titles);

        if (savedInstanceState != null)
        {
            selectedPlayId = savedInstanceState.GetInt("current_play_id", 0);
        }

        var quoteContainer = Activity.FindViewById(Resource.Id.playquote_container);
        showingTwoFragments = quoteContainer != null &&
                                quoteContainer.Visibility == ViewStates.Visible;
        if (showingTwoFragments)
        {
            ListView.ChoiceMode = ChoiceMode.Single;
            ShowPlayQuote(selectedPlayId);
        }
    }

    public override void OnSaveInstanceState(Bundle outState)
    {
        base.OnSaveInstanceState(outState);
        outState.PutInt("current_play_id", selectedPlayId);
    }

    public override void OnListItemClick(ListView l, View v, int position, long id)
    {
        ShowPlayQuote(position);
    }

    void ShowPlayQuote(int playId)
    {
        selectedPlayId = playId;
        if (showingTwoFragments)
        {
            ListView.SetItemChecked(selectedPlayId, true);

            var playQuoteFragment = FragmentManager.FindFragmentById(Resource.Id.playquote_container) as PlayQuoteFragment;

            if (playQuoteFragment == null || playQuoteFragment.PlayId != playId)
            {
                var container = Activity.FindViewById(Resource.Id.playquote_container);
                var quoteFrag = PlayQuoteFragment.NewInstance(selectedPlayId);

                FragmentTransaction ft = FragmentManager.BeginTransaction();
                ft.Replace(Resource.Id.playquote_container, quoteFrag);
                ft.AddToBackStack(null);
                ft.SetTransition(FragmentTransit.FragmentFade);
                ft.Commit();
            }
        }
        else
        {
            var intent = new Intent(Activity, typeof(PlayQuoteActivity));
            intent.PutExtra("current_play_id", playId);
            StartActivity(intent);
        }
    }
}
```

## <a name="3-changes-to-playquoteactivity"></a>3. Änderungen an PlayQuoteActivity

Ein letztes Detail muss noch beachtet werden: `PlayQuoteActivity` ist nicht erforderlich, wenn sich das Gerät im Querformatmodus befindet. Wenn das Gerät im Querformatmodus ausgeführt wird, sollte `PlayQuoteActivity` nicht angezeigt werden. Aktualisieren Sie die `OnCreate`-Methode von `PlayQuoteActivity`, sodass sie sich selbst schließt. Dieser Code ist die endgültige Version von `PlayQuoteActivity.OnCreate`:

```csharp
protected override void OnCreate(Bundle savedInstanceState)
{
    base.OnCreate(savedInstanceState);

    if (Resources.Configuration.Orientation == Android.Content.Res.Orientation.Landscape)
    {
        Finish();
    }

    var playId = Intent.Extras.GetInt("current_play_id", 0);
    var playQuoteFrag = PlayQuoteFragment.NewInstance(playId);
    FragmentManager.BeginTransaction()
                    .Add(Android.Resource.Id.Content, playQuoteFrag)
                    .Commit();
}
```

Durch diese Änderung wird eine Überprüfung der Geräteausrichtung hinzugefügt. Wenn das Gerät im Querformatmodus ausgeführt wird, schließt `PlayQuoteActivity` sich selbst.

## <a name="4-run-the-application"></a>4. Ausführen der Anwendung

Sobald diese Änderungen abgeschlossen sind, führen Sie die App aus, drehen das Gerät in das Querformat (falls erforderlich), und wählen dann ein Theaterstück aus. Das Zitat sollte dann auf demselben Bildschirm wie die Liste der Theaterstücke angezeigt werden:

[![App, die auf einem Android-Smartphone im Querformatmodus ausgeführt wird](./images/intro-screenshot-phone-land-sml.png)](./images/intro-screenshot-phone-land.png#lightbox)

[![App, die auf einem Android-Tablet ausgeführt wird](./images/intro-screenshot-tablet-sml.png)](./images/intro-screenshot-tablet.png#lightbox)
