---
title: 'Exemplarische Vorgehensweise zu Fragmenten: Teil 2'
ms.prod: xamarin
ms.topic: tutorial
ms.assetid: 444A894D-5197-4726-934F-79BA80A71CB0
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/26/2018
ms.openlocfilehash: 7ec8ad6ce428107d2255dd07c7e69c9e77780c09
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61026273"
---
# <a name="fragments-walkthrough-ndash-landscape"></a>Exemplarische Vorgehensweise zu Fragmenten &ndash; Querformat

Die [Fragmente Exemplarische Vorgehensweise &ndash; Teil 1](./walkthrough.md) veranschaulicht das Erstellen und verwenden Sie Fragmente in einem Android-app, die auf einem Smartphone die kleineren Bildschirmen abzielt. Der nächste Schritt in dieser exemplarischen Vorgehensweise wird zum Ändern der Anwendung zu den zusätzlichen horizontalen Platz auf Tablet nutzen &ndash; fallen in eine Aktivität, die immer die Liste der Wiedergabe werden (die `TitlesFragment`) und `PlayQuoteFragment` dynamisch hinzugefügt werden mit der Aktivität als Reaktion auf eine vom Benutzer vorgenommene Auswahl:

[![App auf einem tablet](./walkthrough-landscape-images/01-tablet-screenshot-sml.png)](./walkthrough-landscape-images/01-tablet-screenshot.png#lightbox)

Telefone, die im Querformatmodus ausgeführt werden, werden auch von dieser Erweiterung profitieren:

[![App auf einem Android-Telefon im Querformat](./images/intro-screenshot-phone-land-sml.png)](./images/intro-screenshot-phone-land.png#lightbox)

## <a name="updating-the-app-to-handle-landscape-orientation"></a>Aktualisieren die app zum Behandeln von Querformat

Die folgenden Änderungen baut auf der Arbeit, die durchgeführt wurde, in der [Fragmente Exemplarische Vorgehensweise – Telefon](./walkthrough.md)

1. Erstellen Sie ein alternatives Layout zum Anzeigen der `TitlesFragment` und `PlayQuoteFragment`.
1. Update `TitlesFragment` zu erkennen, ob das Gerät gleichzeitig beide Fragmente angezeigt wird, und ändern das Verhalten entsprechend.
1. Update `PlayQuoteActivity` zu schließen, wenn das Gerät im Querformatmodus ausgeführt wird.

## <a name="1-create-an-alternate-layout"></a>1. Erstellen Sie ein alternatives layout

Bei der Main-Aktivität auf einem Android-Gerät erstellt wird, wird Android entscheiden, welches Layout Laden auf die Ausrichtung des Geräts basieren. Android bietet standardmäßig die **Resources/layout/activity_main.axml** Layoutdatei. Für Geräte, die im Querformat laden Android bieten die **Resources/layout-land/activity_main.axml** Layoutdatei. Das Handbuch auf [Android-Ressourcen](/xamarin/android/app-fundamentals/resources-in-android) enthält weitere Einzelheiten, wie Android entscheidet, welche Ressourcendateien, um für eine Anwendung zu laden.

Erstellen Sie ein alternatives Layout, das als Ziel **Querformat** Ausrichtung anhand der Schritte in der [alternative Layouts](/xamarin/android/user-interface/android-designer/alternative-layout-views) Guide. Dadurch sollte eine neue Ressource Layoutdatei hinzugefügt, in das Projekt **Resources/layout/activity_main.axml**:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![Alternative Layouts im Projektmappen-Explorer](./walkthrough-landscape-images/02-alternate-layout.w157-sml.png)](./walkthrough-landscape-images/02-alternate-layout.w157.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

[![Alternative Layouts im Projektmappenpad](./walkthrough-landscape-images/02-alternate-layout.m743-sml.png)](./walkthrough-landscape-images/02-alternate-layout.m743.png#lightbox)

-----

Bearbeiten Sie die Quelle der Datei nach dem Erstellen des alternative Layouts können **Resources/layout-land/activity_main.axml** , damit sie diesen XML-Code entspricht:

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

Die Stammansicht der Aktivität erhält die Ressourcen-ID `two_fragments_layout` und verfügt über zwei untergeordnete Ansichten, eine `fragment` und `FrameLayout`. Während der `fragment` statisch geladen ist, wird die `FrameLayout` fungiert als eine "Platzhalter", die zur Laufzeit durch ersetzt werden die `PlayQuoteFragment`. Jedes Mal einen neuen Spiel, in ausgewählt ist der `TitlesFragment`, `playquote_container` aktualisiert werden, durch eine neue Instanz der der `PlayQuoteFragment`.

Jede der untergeordneten Ansichten, nimmt die Gesamthöhe ihres übergeordneten Elements. Die Breite der einzelnen Unteransicht wird gesteuert, indem die `android:layout_weight` und `android:layout_width` Attribute. In diesem Beispiel wird jede Unteransicht, 50 % der Breite nimmt bereitstellen, indem Sie das übergeordnete Element. Finden Sie unter [Googles-Dokument, auf die LinearLayout](https://developer.android.com/guide/topics/ui/layout/linear.html) Einzelheiten _Layout Gewichtung_.

## <a name="2-changes-to-titlesfragment"></a>2. Änderungen an TitlesFragment

Nach das alternative Layout erstellt wurde, ist es notwendig, aktualisieren Sie `TitlesFragment`. Wenn die app zeigt die zwei Fragmente auf eine Aktivität, klicken Sie dann `TitlesFragment` laden sollte die `PlayQuoteFragment` in der übergeordneten Aktivität. Andernfalls `TitlesFragment` sollte gestartet werden die `PlayQuoteActivity` welchem Host die `PlayQuoteFragment`. Ein boolesches Flag hilft `TitlesFragment` bestimmen die Verhalten sollte. Dieses Flag wird initialisiert werden, der `OnActivityCreated` Methode.

Fügen Sie zuerst eine Instanzvariable hinzu, am oberen Rand der `TitlesFragment` Klasse:

```csharp
bool showingTwoFragments;
```

Fügen Sie dann den folgenden Codeausschnitt zu `OnActivityCreated` zur Initialisierung der Variablen: 

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

Wenn das Gerät im Querformat ausgeführt wird und dann die `FrameLayout` mit den Ressourcen-ID `playquote_container` auf dem Bildschirm sichtbar damit `showingTwoFragments` initialisiert, `true`. Wenn das Gerät im Hochformat, klicken Sie dann ausgeführt wird `playquote_container` werden nicht auf dem Bildschirm, also `showingTwoFragments` werden `false`.

Die `ShowPlayQuote` Methode zu ändern, wie sie ein Angebot angezeigt müssen &ndash; in einem Fragment oder Starten einer neuen Aktivität.  Update der `ShowPlayQuote` Methode, um ein Fragment zu laden, wenn zwei Fragmente angezeigt. Andernfalls sollten sie eine Aktivität zu starten:

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

Wenn der Benutzer ein Spiel ausgewählt hat, die sich von dem unterscheidet, die gerade in angezeigt wird `PlayQuoteFragment`, klicken Sie dann ein neues `PlayQuoteFragment` wird erstellt, und ersetzen den Inhalt des der `playquote_container` innerhalb des Kontexts einer `FragmentTransaction`.

### <a name="complete-code-for-titlesfragment"></a>Vollständige Code für TitlesFragment

Nach Abschluss der vorherigen Änderungen an `TitlesFragment`, die vollständige Klasse sollte dieser Code entsprechen:

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

Es ist eine endgültige Details kümmern: `PlayQuoteActivity` ist nicht erforderlich, wenn das Gerät im Querformatmodus ausgeführt wird. Wenn das Gerät im Querformat ist die `PlayQuoteActivity` sollten nicht sichtbar sein. Update der `OnCreate` -Methode der `PlayQuoteActivity` so, dass es selbst geschlossen wird. Dieser Code ist die endgültige Version von `PlayQuoteActivity.OnCreate`:

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

Diese Änderung wird überprüft, ob der geräteausrichtung hinzugefügt. Wenn sie im Querformat ist `PlayQuoteActivity` wird geschlossen, selbst.

## <a name="4-run-the-application"></a>4. Ausführen der Anwendung

Nachdem diese Änderungen abgeschlossen sind, führen Sie die app drehen Sie das Gerät auf Querformat (falls erforderlich), und wählen Sie dann auf eine Play. Das Angebot sollte auf dem gleichen Bildschirm wie die Liste der Wiedergabe angezeigt werden:

[![App auf einem Android-Telefon im Querformat](./images/intro-screenshot-phone-land-sml.png)](./images/intro-screenshot-phone-land.png#lightbox)

[![App auf einem Android-tablet](./images/intro-screenshot-tablet-sml.png)](./images/intro-screenshot-tablet.png#lightbox)
