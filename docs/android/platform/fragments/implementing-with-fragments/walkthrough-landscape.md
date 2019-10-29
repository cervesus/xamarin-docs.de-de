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
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73020266"
---
# <a name="fragments-walkthrough-ndash-landscape"></a>Fragmente Exemplarische Vorgehensweise &ndash; Landscape

In der exemplarischen Vorgehensweise " [Fragmente" &ndash; Teil 1](./walkthrough.md) wurde veranschaulicht, wie Fragmente in einer Android-App erstellt und verwendet werden, die auf die kleineren Bildschirme auf einem Telefon Der nächste Schritt in dieser exemplarischen Vorgehensweise besteht darin, die Anwendung so zu ändern, dass Sie den zusätzlichen horizontalen Leerraum auf Tablet nutzt &ndash; es gibt eine Aktivität, bei der es sich immer um die Liste der Wiedergaben (die `TitlesFragment`) handelt und `PlayQuoteFragment` der Aktivität in r dynamisch hinzugefügt wird. Treffen Sie eine Auswahl durch den Benutzer:

[![APP, die auf Tablet ausgeführt wird](./walkthrough-landscape-images/01-tablet-screenshot-sml.png)](./walkthrough-landscape-images/01-tablet-screenshot.png#lightbox)

Telefone, die im Querformat ausgeführt werden, profitieren ebenfalls von dieser Erweiterung:

[![-APP, die auf einem Android-Telefon im Querformat ausgeführt wird](./images/intro-screenshot-phone-land-sml.png)](./images/intro-screenshot-phone-land.png#lightbox)

## <a name="updating-the-app-to-handle-landscape-orientation"></a>Aktualisieren der App zur Handhabung der Querformat Ausrichtung

Die folgenden Änderungen bauen auf der Arbeit auf, die in der exemplarischen Vorgehensweise " [Fragmente" durch](./walkthrough.md) geführt wurde.

1. Erstellen Sie ein alternatives Layout, um sowohl die `TitlesFragment` als auch die `PlayQuoteFragment`anzuzeigen.
1. Aktualisieren Sie `TitlesFragment`, um zu ermitteln, ob das Gerät beide Fragmente gleichzeitig anzeigt, und ändern Sie das Verhalten entsprechend.
1. Aktualisieren Sie `PlayQuoteActivity`, um zu schließen, wenn sich das Gerät im Querformat befindet.

## <a name="1-create-an-alternate-layout"></a>1. Erstellen eines alternativen Layouts

Wenn die Hauptaktivität auf einem Android-Gerät erstellt wird, entscheidet Android basierend auf der Ausrichtung des Geräts, welches Layout geladen werden soll. Standardmäßig wird die Layoutdatei **Resources/Layout/activity_main. axml** von Android bereitgestellt. Bei Geräten, die im Querformat geladen werden, stellt Android die Layoutdatei **Resources/Layout-Land/activity_main. axml** bereit. Das Handbuch zu [Android-Ressourcen](/xamarin/android/app-fundamentals/resources-in-android) enthält weitere Details dazu, wie Android entscheidet, welche Ressourcen Dateien für eine Anwendung geladen werden.

Erstellen Sie ein alternatives Layout, das sich auf die **quer** Ausrichtung bezieht, indem Sie die im Abschnitt " [Alternative Layouts](/xamarin/android/user-interface/android-designer/alternative-layout-views) " beschriebenen Schritte befolgen. Dadurch sollte dem Projekt, **Resources/Layout/activity_main. axml**, eine neue layoutressourcendatei hinzugefügt werden:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![alternatives Layout in Projektmappen-Explorer](./walkthrough-landscape-images/02-alternate-layout.w157-sml.png)](./walkthrough-landscape-images/02-alternate-layout.w157.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

[![alternatives Layout in Lösungspad](./walkthrough-landscape-images/02-alternate-layout.m743-sml.png)](./walkthrough-landscape-images/02-alternate-layout.m743.png#lightbox)

-----

Nachdem Sie das alternative Layout erstellt haben, bearbeiten Sie die Quelle der Datei **Resources/Layout-Land/activity_main. axml** so, dass Sie mit dieser XML-Datei übereinstimmt:

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

Die Stamm Ansicht der Aktivität erhält die Ressourcen-ID `two_fragments_layout` und verfügt über zwei untergeordnete Sichten, eine `fragment` und eine `FrameLayout`. Während die `fragment` statisch geladen ist, fungiert die `FrameLayout` als "Platzhalter", der zur Laufzeit durch den `PlayQuoteFragment`ersetzt wird. Jedes Mal, wenn ein neues Play im `TitlesFragment`ausgewählt wird, wird die `playquote_container` mit einer neuen Instanz der `PlayQuoteFragment`aktualisiert.

Jede der untergeordneten Sichten belegt die volle Höhe ihres übergeordneten Elements. Die Breite jeder unter Ansicht wird durch die Attribute `android:layout_weight` und `android:layout_width` gesteuert. In diesem Beispiel belegt jede unter Ansicht 50% der Breite, die vom übergeordneten Element bereitgestellt wird. Ausführliche Informationen zur _layoutgewichtung_finden Sie [im Google-Dokument über das LinearLayout](https://developer.android.com/guide/topics/ui/layout/linear.html) .

## <a name="2-changes-to-titlesfragment"></a>2. Änderungen an titlesfragment

Nachdem das alternative Layout erstellt wurde, müssen `TitlesFragment`aktualisiert werden. Wenn die APP die beiden Fragmente in einer Aktivität anzeigt, sollte `TitlesFragment` die `PlayQuoteFragment` in der übergeordneten Aktivität laden. Andernfalls sollten `TitlesFragment` den `PlayQuoteActivity` starten, der die `PlayQuoteFragment`hostet. Mit einem booleschen Flag können `TitlesFragment` bestimmen, welches Verhalten verwendet werden soll. Dieses Flag wird in der `OnActivityCreated`-Methode initialisiert.

Fügen Sie zunächst eine Instanzvariable am Anfang der `TitlesFragment` Klasse hinzu:

```csharp
bool showingTwoFragments;
```

Fügen Sie anschließend den folgenden Code Ausschnitt hinzu, um zu `OnActivityCreated`, um die Variable zu initialisieren: 

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

Wenn das Gerät im Querformat Modus ausgeführt wird, wird der `FrameLayout` mit der Ressourcen-ID `playquote_container` auf dem Bildschirm angezeigt, sodass `showingTwoFragments` für `true`initialisiert wird. Wenn das Gerät im Hochformat Modus ausgeführt wird, wird `playquote_container` nicht auf dem Bildschirm angezeigt, sodass `showingTwoFragments` `false`wird.

Die `ShowPlayQuote`-Methode muss die Anzeige eines Anführungs Zeichens &ndash; entweder in einem Fragment oder das Starten einer neuen Aktivität ändern.  Aktualisieren Sie die `ShowPlayQuote`-Methode, um ein Fragment zu laden, wenn zwei Fragmente angezeigt werden. andernfalls sollte eine Aktivität gestartet werden:

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

Wenn der Benutzer eine Wiedergabe ausgewählt hat, die sich von der unterscheidet, die derzeit in `PlayQuoteFragment`angezeigt wird, wird ein neuer `PlayQuoteFragment` erstellt und ersetzt den Inhalt der `playquote_container` im Kontext eines `FragmentTransaction`.

### <a name="complete-code-for-titlesfragment"></a>Vervollständigen von Code für titlesfragment

Nachdem alle vorherigen Änderungen an `TitlesFragment`abgeschlossen wurden, sollte die vollständige Klasse mit diesem Code identisch sein:

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

## <a name="3-changes-to-playquoteactivity"></a>3. Änderungen an playquoteactivity

Es gibt ein letztes Detail, das Sie berücksichtigen sollten: `PlayQuoteActivity` ist nicht erforderlich, wenn sich das Gerät im Querformat befindet. Wenn sich das Gerät im Querformat befindet, sollte `PlayQuoteActivity` nicht sichtbar sein. Aktualisieren Sie die `OnCreate`-Methode von `PlayQuoteActivity`, damit Sie sich selbst schließt. Bei diesem Code handelt es sich um die endgültige Version von `PlayQuoteActivity.OnCreate`:

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

Durch diese Änderung wird eine Überprüfung der Geräte Ausrichtung hinzugefügt. Wenn Sie sich im Querformat befindet, wird `PlayQuoteActivity` geschlossen.

## <a name="4-run-the-application"></a>4. führen Sie die Anwendung aus.

Nachdem diese Änderungen vorgenommen wurden, führen Sie die APP aus, drehen Sie das Gerät (falls erforderlich) in den Querformat, und wählen Sie dann eine Wiedergabe aus. Das Anführungszeichen sollte auf dem gleichen Bildschirm wie die Liste der Spiele angezeigt werden:

[![-APP, die auf einem Android-Telefon im Querformat ausgeführt wird](./images/intro-screenshot-phone-land-sml.png)](./images/intro-screenshot-phone-land.png#lightbox)

[![-APP, die auf einem Android-Tablet ausgeführt wird](./images/intro-screenshot-tablet-sml.png)](./images/intro-screenshot-tablet.png#lightbox)
