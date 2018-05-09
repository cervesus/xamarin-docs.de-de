---
title: Fragmente Exemplarische Vorgehensweise – Teil 2
ms.prod: xamarin
ms.topic: tutorial
ms.assetid: 444A894D-5197-4726-934F-79BA80A71CB0
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/26/2018
ms.openlocfilehash: 58291388d375a4fd9273c8e0cd46db3799966766
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
---
# <a name="fragments-walkthrough-ndash-landscape"></a>Exemplarische Vorgehensweise Fragmenten &ndash; Querformat

Die [Fragments Walkthrough &ndash; Teil 1](./walkthrough.md) veranschaulicht, wie das Erstellen und Verwenden von Fragmenten in einer Android-app, die auf einem Telefon zu der kleineren Bildschirmen ausgerichtet ist. Der nächste Schritt in dieser exemplarischen Vorgehensweise wird die Anwendung den zusätzlichen horizontalen Leerraum auf Tablet nutzen ändern &ndash; wird eine Aktivität, die immer die Liste der abgespielt werden (die `TitlesFragment`) und `PlayQuoteFragment` dynamisch hinzugefügt werden mit der Aktivität als Antwort auf eine vom Benutzer vorgenommene Auswahl:

[![App auf einem tablet](./walkthrough-landscape-images/01-tablet-screenshot-sml.png)](./walkthrough-landscape-images/01-tablet-screenshot.png#lightbox)

Telefone, die im Querformat ausgeführt werden, werden auch von dieser Erweiterung profitieren:

[![App auf einem Android-Telefon im Querformat](./images/intro-screenshot-phone-land-sml.png)](./images/intro-screenshot-phone-land.png#lightbox)

## <a name="updating-the-app-to-handle-landscape-orientation"></a>Die app zum Behandeln von Querformat aktualisiert.

Die folgenden Änderungen werden bauen auf der Arbeit, die erstellt wurde, in der [Fragments Walkthrough - Telefon](./walkthrough.md)

1. Erstellen Sie ein Layout für Alternative zum Anzeigen von sowohl die `TitlesFragment` und `PlayQuoteFragment`.
1. Update `TitlesFragment` zu ermitteln, ob das Gerät die beiden Fragmenten gleichzeitig angezeigt wird, und ändern das Verhalten entsprechend.
1. Update `PlayQuoteActivity` zu schließen, wenn das Gerät im Querformat.

## <a name="1-create-an-alternate-layout"></a>1. Erstellen Sie eine alternative Layouts

Wenn Main Aktivität auf einem Android-Gerät erstellt wird, entscheidet Android, welche Anordnung zu laden auf die Ausrichtung des Geräts basieren. Android bietet standardmäßig die **Resources/layout/activity_main.axml** Layoutdatei. Für Geräte, die im Querformat laden Android gebe den **Resources/layout-land/activity_main.axml** Layoutdatei. Im Administratorhandbuch zur [Android Ressourcen](/xamarin/android/app-fundamentals/resources-in-android) enthält weitere Details zum wie Android entscheidet, welche Ressourcendateien, um für eine Anwendung zu laden.

Erstellen Sie eine alternative Layouts, dessen Ziel **Querformat** Ausrichtung durch die Schritte der [alternative Layouts](/xamarin/android/user-interface/android-designer/alternative-layout-views) Handbuch. Dies sollte eine neue Layout-Ressourcendatei hinzufügen, auf das Projekt **Resources/layout/activity_main.axml**:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Alternative Layouts im Projektmappen-Explorer](./walkthrough-landscape-images/02-alternate-layout.w157-sml.png)](./walkthrough-landscape-images/02-alternate-layout.w157.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Alternative Layouts in Projektmappen mit Leerstellen auffüllen](./walkthrough-landscape-images/02-alternate-layout.m743-sml.png)](./walkthrough-landscape-images/02-alternate-layout.m743.png#lightbox)

-----

Nach dem Erstellen des alternative Layouts, bearbeiten Sie die Quelle der Datei **Resources/layout-land/activity_main.axml** , damit es diesen XML-Code entspricht:

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

Die Stammansicht der Aktivität erhält die Ressourcen-ID `two_fragments_layout` und verfügt über zwei untergeordnete Ansichten, eine `fragment` und ein `FrameLayout`. Während der `fragment` statisch geladen wird, wird die `FrameLayout` fungiert als eine "Platzhalter", die zur Laufzeit durch ersetzt werden die `PlayQuoteFragment`. Jedes Mal ein neues Spiel, in ausgewählt ist der `TitlesFragment`, die `playquote_container` wird durch eine neue Instanz der aktualisiert werden die `PlayQuoteFragment`.

Jeder untergeordnete Ansichten, nimmt die vollständige Höhe ihres übergeordneten Elements. Die Breite der einzelnen Unteransicht wird gesteuert, indem die `android:layout_weight` und `android:layout_width` Attribute. In diesem Beispiel wird jede Unteransicht 50 % der Breite belegen bereitstellen, indem Sie das übergeordnete Element. Finden Sie unter [Google-Dokument auf die LinearLayout](https://developer.android.com/guide/topics/ui/layout/linear.html) ausführliche Informationen zu _Layout Gewichtung_.

## <a name="2-changes-to-titlesfragment"></a>2. Änderungen an TitlesFragment

Sobald das alternative Layout erstellt wurde, wird für das update erforderlich `TitlesFragment`. Wenn die app anzeigt zwei Fragmente für eine Aktivität anzugeben, klicken Sie dann `TitlesFragment` sollten laden die `PlayQuoteFragment` in der übergeordneten Aktivität. Andernfalls `TitlesFragment` starten sollte die `PlayQuoteActivity` welchem Host die `PlayQuoteFragment`. Ein boolesches Flag hilft `TitlesFragment` bestimmen das Verhalten verwendet werden sollte. Dieses Flag wird so initialisiert, der `OnActivityCreated` Methode.

Fügen Sie zunächst eine Instanzvariable am oberen Rand der `TitlesFragment` Klasse:

```csharp
bool showingTwoFragments;
```

Fügen Sie dann den folgenden Codeausschnitt `OnActivityCreated` zur Initialisierung der Variablen: 

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

Wenn das Gerät im Querformat, läuft die `FrameLayout` mit dem Ressourcen-ID `playquote_container` werden auf dem Bildschirm angezeigt damit `showingTwoFragments` wird so initialisiert, um `true`. Wenn das Gerät im Hochformat,, klicken Sie dann ausgeführt wird `playquote_container` werden nicht auf dem Bildschirm daher `showingTwoFragments` werden `false`.

Die `ShowPlayQuote` Methode zu ändern, wie sie ein Angebot angezeigt müssen &ndash; in einem Fragment oder starten Sie eine neue Aktivität.  Update der `ShowPlayQuote` Methode, um ein Fragment zu laden, wenn zwei Fragmente mit andernfalls sollten sie eine Aktivität starten:

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

Wenn der Benutzer eine Play ausgewählt hat, die sich von der unterscheidet, die gegenwärtig in angezeigt wird `PlayQuoteFragment`, klicken Sie dann ein neues `PlayQuoteFragment` wird erstellt und ersetzt den Inhalt des der `playquote_container` innerhalb des Kontexts eine `FragmentTransaction`.

### <a name="complete-code-for-titlesfragment"></a>Vollständige Code für TitlesFragment

Nach Abschluss der vorherigen Änderungen an `TitlesFragment`, die vollständige Klasse übereinstimmen, diesen Code:

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

Ist eine endgültige Detail zu erledigen: `PlayQuoteActivity` ist nicht erforderlich, wenn das Gerät im Querformat. Wenn das Gerät im Querformat ist die `PlayQuoteActivity` nicht angezeigt werden. Update der `OnCreate` Methode `PlayQuoteActivity` so, dass es selbst geschlossen wird. Dieser Code ist die endgültige Version von `PlayQuoteActivity.OnCreate`:

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

Diese Änderung wird eine Überprüfung auf die geräteausrichtung hinzugefügt. Wenn es im Querformat ist `PlayQuoteActivity` wird geschlossen, selbst.

## <a name="4-run-the-application"></a>4. Ausführen der Anwendung

Nachdem diese Änderungen abgeschlossen sind, führen Sie die app drehen Sie das Gerät, um landscape Modus (falls erforderlich), und wählen Sie dann eine Wiedergabe. Das Angebot sollte auf der gleichen Seite wie die Liste der spielt angezeigt werden:

[![App auf einem Android-Telefon im Querformat](./images/intro-screenshot-phone-land-sml.png)](./images/intro-screenshot-phone-land.png#lightbox)

[![Auf einem Android-Tablet Ausführen einer App](./images/intro-screenshot-tablet-sml.png)](./images/intro-screenshot-tablet.png#lightbox)
