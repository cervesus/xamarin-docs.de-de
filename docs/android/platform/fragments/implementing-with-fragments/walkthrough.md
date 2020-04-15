---
title: 'Exemplarische Vorgehensweise zu Xamarin.Android-Fragmenten: Teil 1'
ms.prod: xamarin
ms.topic: tutorial
ms.assetid: ED368FA9-A34E-DC39-D535-5C34C32B9761
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 08/21/2018
ms.openlocfilehash: 043ad02f9ca9148910364ac82917551ee58d72ba
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2020
ms.locfileid: "73027405"
---
# <a name="fragments-walkthrough-ndash-phone"></a>Exemplarische Vorgehensweise zu Fragmenten &ndash; Smartphone

Dies ist der erste Teil einer exemplarischen Vorgehensweise, in der eine Xamarin.Android-App erstellt wird, die ein Android-Gerät in Hochformat als Ziel verwendet. In dieser exemplarischen Vorgehensweise wird erläutert, wie Fragmente in Xamarin.Android erstellt werden und wie diese einem Beispiel hinzugefügt werden.

[![](./images/intro-screenshot-phone-sml.png)](./images/intro-screenshot-phone.png#lightbox)

Die folgenden Klassen werden für diese App erstellt:

1. `PlayQuoteFragment` &nbsp; Dieses Fragment zeigt ein Zitat aus einem Stück von William Shakespeare an. Es wird von `PlayQuoteActivity` gehostet.
1. `Shakespeare` &nbsp; Diese Klasse enthält zwei hartcodierte Arrays als Eigenschaften.
1. `TitlesFragment` &nbsp; Dieses Fragment zeigt eine Liste mit Titeln von Stücken an, die von William Shakespeare geschrieben wurden. Es wird von `MainActivity` gehostet.
1. `PlayQuoteActivity` &nbsp; `TitlesFragment` startet die `PlayQuoteActivity` als Reaktion auf den Benutzer, der ein Werk in `TitlesFragment` auswählt.

## <a name="1-create-the-android-project"></a>1. Erstellen des Android-Projekts

Erstellen Sie ein neues Xamarin.Android-Projekt mit dem Namen **FragmentSample**.
# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[![Erstellen eines neuen Xamarin.Android-Projekts](./walkthrough-images/01-newproject.w157-sml.png)](./walkthrough-images/01-newproject.w157.png#lightbox)

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

[![Erstellen eines neuen Xamarin.Android-Projekts](./walkthrough-images/01-newproject.m742-sml.png)](./walkthrough-images/01-newproject.m742.png#lightbox)

Es wird empfohlen, für diese exemplarische Vorgehensweise **Moderne Entwicklung** auszuwählen.

Nachdem Sie das Projekt erstellt haben, benennen Sie die Datei **layout/Main.axml** in **layout/activity_main.axml** um.

-----

## <a name="2-add-the-data"></a>2. Hinzufügen der Daten

Die Daten für diese Anwendung werden in zwei hartcodierten Zeichenfolgenarrays gespeichert, die Eigenschaften einer Klasse namens `Shakespeare` sind:

* `Shakespeare.Titles` &nbsp; Dieses Array enthält eine Liste der Stücke von William Shakespeare. Dies ist die Datenquelle für das `TitlesFragment`.
* `Shakespeare.Dialogue` &nbsp; Dieses Array enthält eine Liste mit Zitaten aus einem der in `Shakespeare.Titles` enthaltenen Stücke. Dies ist die Datenquelle für das `PlayQuoteFragment`.

Fügen Sie dem **FragmentSample**-Projekt eine neue C#-Klasse hinzu, und nennen Sie sie **Shakespeare.cs**. Erstellen Sie in dieser Datei eine neue C#-Klasse mit dem Namen `Shakespeare` und dem folgenden Inhalt.

```csharp
class Shakespeare
{
    public static string[] Titles = {
                                      "Henry IV (1)",
                                      "Henry V",
                                      "Henry VIII",
                                      "Richard II",
                                      "Richard III",
                                      "Merchant of Venice",
                                      "Othello",
                                      "King Lear"
                                    };

    public static string[] Dialogue = {
                                        "So shaken as we are, so wan with care, Find we a time for frighted peace to pant, And breathe short-winded accents of new broils To be commenced in strands afar remote. No more the thirsty entrance of this soil Shall daub her lips with her own children's blood; Nor more shall trenching war channel her fields, Nor bruise her flowerets with the armed hoofs Of hostile paces: those opposed eyes, Which, like the meteors of a troubled heaven, All of one nature, of one substance bred, Did lately meet in the intestine shock And furious close of civil butchery Shall now, in mutual well-beseeming ranks, March all one way and be no more opposed Against acquaintance, kindred and allies: The edge of war, like an ill-sheathed knife, No more shall cut his master. Therefore, friends, As far as to the sepulchre of Christ, Whose soldier now, under whose blessed cross We are impressed and engaged to fight, Forthwith a power of English shall we levy; Whose arms were moulded in their mothers' womb To chase these pagans in those holy fields Over whose acres walk'd those blessed feet Which fourteen hundred years ago were nail'd For our advantage on the bitter cross. But this our purpose now is twelve month old, And bootless 'tis to tell you we will go: Therefore we meet not now. Then let me hear Of you, my gentle cousin Westmoreland, What yesternight our council did decree In forwarding this dear expedience.",
                                        "Hear him but reason in divinity, And all-admiring with an inward wish You would desire the king were made a prelate: Hear him debate of commonwealth affairs, You would say it hath been all in all his study: List his discourse of war, and you shall hear A fearful battle render'd you in music: Turn him to any cause of policy, The Gordian knot of it he will unloose, Familiar as his garter: that, when he speaks, The air, a charter'd libertine, is still, And the mute wonder lurketh in men's ears, To steal his sweet and honey'd sentences; So that the art and practic part of life Must be the mistress to this theoric: Which is a wonder how his grace should glean it, Since his addiction was to courses vain, His companies unletter'd, rude and shallow, His hours fill'd up with riots, banquets, sports, And never noted in him any study, Any retirement, any sequestration From open haunts and popularity.",
                                        "I come no more to make you laugh: things now, That bear a weighty and a serious brow, Sad, high, and working, full of state and woe, Such noble scenes as draw the eye to flow, We now present. Those that can pity, here May, if they think it well, let fall a tear; The subject will deserve it. Such as give Their money out of hope they may believe, May here find truth too. Those that come to see Only a show or two, and so agree The play may pass, if they be still and willing, I'll undertake may see away their shilling Richly in two short hours. Only they That come to hear a merry bawdy play, A noise of targets, or to see a fellow In a long motley coat guarded with yellow, Will be deceived; for, gentle hearers, know, To rank our chosen truth with such a show As fool and fight is, beside forfeiting Our own brains, and the opinion that we bring, To make that only true we now intend, Will leave us never an understanding friend. Therefore, for goodness' sake, and as you are known The first and happiest hearers of the town, Be sad, as we would make ye: think ye see The very persons of our noble story As they were living; think you see them great, And follow'd with the general throng and sweat Of thousand friends; then in a moment, see How soon this mightiness meets misery: And, if you can be merry then, I'll say A man may weep upon his wedding-day.",
                                        "First, heaven be the record to my speech! In the devotion of a subject's love, Tendering the precious safety of my prince, And free from other misbegotten hate, Come I appellant to this princely presence. Now, Thomas Mowbray, do I turn to thee, And mark my greeting well; for what I speak My body shall make good upon this earth, Or my divine soul answer it in heaven. Thou art a traitor and a miscreant, Too good to be so and too bad to live, Since the more fair and crystal is the sky, The uglier seem the clouds that in it fly. Once more, the more to aggravate the note, With a foul traitor's name stuff I thy throat; And wish, so please my sovereign, ere I move, What my tongue speaks my right drawn sword may prove.",
                                        "Now is the winter of our discontent Made glorious summer by this sun of York; And all the clouds that lour'd upon our house In the deep bosom of the ocean buried. Now are our brows bound with victorious wreaths; Our bruised arms hung up for monuments; Our stern alarums changed to merry meetings, Our dreadful marches to delightful measures. Grim-visaged war hath smooth'd his wrinkled front; And now, instead of mounting barded steeds To fright the souls of fearful adversaries, He capers nimbly in a lady's chamber To the lascivious pleasing of a lute. But I, that am not shaped for sportive tricks, Nor made to court an amorous looking-glass; I, that am rudely stamp'd, and want love's majesty To strut before a wanton ambling nymph; I, that am curtail'd of this fair proportion, Cheated of feature by dissembling nature, Deformed, unfinish'd, sent before my time Into this breathing world, scarce half made up, And that so lamely and unfashionable That dogs bark at me as I halt by them; Why, I, in this weak piping time of peace, Have no delight to pass away the time, Unless to spy my shadow in the sun And descant on mine own deformity: And therefore, since I cannot prove a lover, To entertain these fair well-spoken days, I am determined to prove a villain And hate the idle pleasures of these days. Plots have I laid, inductions dangerous, By drunken prophecies, libels and dreams, To set my brother Clarence and the king In deadly hate the one against the other: And if King Edward be as true and just As I am subtle, false and treacherous, This day should Clarence closely be mew'd up, About a prophecy, which says that 'G' Of Edward's heirs the murderer shall be. Dive, thoughts, down to my soul: here Clarence comes.",
                                        "To bait fish withal: if it will feed nothing else, it will feed my revenge. He hath disgraced me, and hindered me half a million; laughed at my losses, mocked at my gains, scorned my nation, thwarted my bargains, cooled my friends, heated mine enemies; and what's his reason? I am a Jew. Hath not a Jew eyes? hath not a Jew hands, organs, dimensions, senses, affections, passions? fed with the same food, hurt with the same weapons, subject to the same diseases, healed by the same means, warmed and cooled by the same winter and summer, as a Christian is? If you prick us, do we not bleed? if you tickle us, do we not laugh? if you poison us, do we not die? and if you wrong us, shall we not revenge? If we are like you in the rest, we will resemble you in that. If a Jew wrong a Christian, what is his humility? Revenge. If a Christian wrong a Jew, what should his sufferance be by Christian example? Why, revenge. The villany you teach me, I will execute, and it shall go hard but I will better the instruction.",
                                        "Virtue! a fig! 'tis in ourselves that we are thus or thus. Our bodies are our gardens, to the which our wills are gardeners: so that if we will plant nettles, or sow lettuce, set hyssop and weed up thyme, supply it with one gender of herbs, or distract it with many, either to have it sterile with idleness, or manured with industry, why, the power and corrigible authority of this lies in our wills. If the balance of our lives had not one scale of reason to poise another of sensuality, the blood and baseness of our natures would conduct us to most preposterous conclusions: but we have reason to cool our raging motions, our carnal stings, our unbitted lusts, whereof I take this that you call love to be a sect or scion.",
                                        "Blow, winds, and crack your cheeks! rage! blow! You cataracts and hurricanoes, spout Till you have drench'd our steeples, drown'd the cocks! You sulphurous and thought-executing fires, Vaunt-couriers to oak-cleaving thunderbolts, Singe my white head! And thou, all-shaking thunder, Smite flat the thick rotundity o' the world! Crack nature's moulds, an germens spill at once, That make ingrateful man!"
                                    };
}
```

## <a name="3-create-the-playquotefragment"></a>3. Erstellen von PlayQuoteFragment

Bei `PlayQuoteFragment` handelt es sich um ein Android-Fragment, das ein Zitat aus einem Shakespeare-Stück anzeigt, das zuvor in der Anwendung vom Benutzer ausgewählt wurde. Dieses Fragment verwendet keine Android-Layoutdatei. Stattdessen wird die Benutzeroberfläche dynamisch erstellt. Fügen Sie dem Projekt eine neue `Fragment`-Klasse namens `PlayQuoteFragment` hinzu:

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[![Hinzufügen einer neuen C#-Klasse](./walkthrough-images/04-addfragment.w157-sml.png)](./walkthrough-images/02-addclass.w157.png#lightbox)

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

[![Hinzufügen einer neuen C#-Klasse](./walkthrough-images/04-addfragment.m742-sml.png)](./walkthrough-images/02-addclass.m742.png#lightbox)

-----

Ändern Sie dann den Code für das Fragment so, dass er diesem Codeausschnitt ähnelt:

```csharp
public class PlayQuoteFragment : Fragment
{
    public int PlayId => Arguments.GetInt("current_play_id", 0);

    public static PlayQuoteFragment NewInstance(int playId)
    {
        var bundle = new Bundle();
        bundle.PutInt("current_play_id", playId);
        return new PlayQuoteFragment {Arguments = bundle};
    }

    public override View OnCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState)
    {
        if (container == null)
        {
            return null;
        }

        var textView = new TextView(Activity);
        var padding = Convert.ToInt32(TypedValue.ApplyDimension(ComplexUnitType.Dip, 4, Activity.Resources.DisplayMetrics));
        textView.SetPadding(padding, padding, padding, padding);
        textView.TextSize = 24;
        textView.Text = Shakespeare.Dialogue[PlayId];

        var scroller = new ScrollView(Activity);
        scroller.AddView(textView);

        return scroller;
    }
}
```

Es ist ein gängiges Muster in Android-Apps, eine Factorymethode bereitzustellen, die ein Fragment instanziiert. Dadurch wird sichergestellt, dass das Fragment mit den erforderlichen Parametern für ein einwandfreies Funktionieren erstellt wird. In dieser exemplarischen Vorgehensweise wird erwartet, dass die App die `PlayQuoteFragment.NewInstance`-Methode verwendet, um bei jeder Auswahl eines Zitats ein neues Fragment zu erstellen. Die Methode `NewInstance` nimmt einen einzelnen Parameter &ndash; an, den Index des anzuzeigenden Zitats.

Die `OnCreateView`-Methode wird von Android aufgerufen, wenn es an der Zeit ist, das Fragment auf dem Bildschirm zu rendern. Sie gibt ein Android-Objekt `View` zurück, das das Fragment ist. Dieses Fragment verwendet keine Layoutdatei, um eine Ansicht zu erstellen. Stattdessen erstellt es die Ansicht programmgesteuert, indem ein **TextView**-Element instanziiert wird, das das Zitat enthält, und es zeigt dieses Widget in einem **ScrollView**-Element an.

> [!NOTE]
> Fragmentunterklassen müssen über einen öffentlichen Standardkonstruktor verfügen, der keine Parameter besitzt.

## <a name="4-create-the-playquoteactivity"></a>4. Erstellen von PlayQuoteActivity

Fragmente müssen innerhalb einer Aktivität gehostet werden. Daher erfordert diese App eine Aktivität, die `PlayQuoteFragment` hostet. Die Aktivität fügt dem Layout das Fragment dynamisch zur Laufzeit hinzu. Fügen Sie der Anwendung eine neue Aktivität hinzu, und nennen Sie sie `PlayQuoteActivity`:

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[![Hinzufügen einer Android-Aktivität zum Projekt](./walkthrough-images/03-addactivity.w157-sml.png)](./walkthrough-images/03-addactivity.w157.png#lightbox)

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

[![Hinzufügen einer Android-Aktivität zum Projekt](./walkthrough-images/03-addactivity.m742-sml.png)](./walkthrough-images/03-addactivity.m742.png#lightbox)

-----

Bearbeiten Sie den Code in `PlayQuoteActivity`:

```csharp
[Activity(Label = "PlayQuoteActivity")]
public class PlayQuoteActivity : Activity
{
    protected override void OnCreate(Bundle savedInstanceState)
    {
        base.OnCreate(savedInstanceState);

        var playId = Intent.Extras.GetInt("current_play_id", 0);

        var detailsFrag = PlayQuoteFragment.NewInstance(playId);
        FragmentManager.BeginTransaction()
                        .Add(Android.Resource.Id.Content, detailsFrag)
                        .Commit();
    }
}
```

Wenn `PlayQuoteActivity` erstellt wird, wird ein neues `PlayQuoteFragment` instanziiert und dieses Fragment in der Stammansicht im Kontext einer `FragmentTransaction` geladen. Beachten Sie, dass diese Aktivität keine Android-Layoutdatei für ihre Benutzeroberfläche lädt. Stattdessen wird ein neues `PlayQuoteFragment` zur Stammansicht der Anwendung hinzugefügt. Der Ressourcenbezeichner `Android.Resource.Id.Content` wird verwendet, um auf die Stammansicht einer Aktivität zu verweisen, ohne deren spezifischen Bezeichner zu kennen.

## <a name="5-create-titlesfragment"></a>5. Erstellen von TitlesFragment

`TitlesFragment` erstellt eine Unterklasse für ein spezialisiertes Fragment (als `ListFragment` bezeichnet), das die Logik für die Anzeige einer `ListView` in einem Fragment kapselt. `ListFragment` stellt eine `ListAdapter`-Eigenschaft (die von `ListView` zum Anzeigen des Inhalts verwendet wird) und einen Ereignishandler mit dem Namen `OnListItemClick` bereit, wodurch das Fragment auf Mausklicks auf eine Zeile reagieren kann, die durch `ListView` angezeigt wird.

Um zu beginnen, fügen Sie dem Projekt ein neues Fragment hinzu und nennen es **TitlesFragment**:

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[![Hinzufügen eines Android-Fragments zum Projekt](./walkthrough-images/04-addfragment.w157-sml.png)](./walkthrough-images/04-addfragment.w157.png#lightbox)

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

[![Hinzufügen eines Android-Fragments zum Projekt](./walkthrough-images/04-addfragment.m742-sml.png)](./walkthrough-images/04-addfragment.m742.png#lightbox)

-----

Bearbeiten Sie den Code innerhalb des Fragments:

```csharp
public class TitlesFragment : ListFragment
{
    int selectedPlayId;

    public TitlesFragment()
    {
        // Being explicit about the requirement for a default constructor.
    }

    public override void OnActivityCreated(Bundle savedInstanceState)
    {
        base.OnActivityCreated(savedInstanceState);
        ListAdapter = new ArrayAdapter<String>(Activity, Android.Resource.Layout.SimpleListItemActivated1, Shakespeare.Titles);

        if (savedInstanceState != null)
        {
            selectedPlayId = savedInstanceState.GetInt("current_play_id", 0);
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
        var intent = new Intent(Activity, typeof(PlayQuoteActivity));
        intent.PutExtra("current_play_id", playId);
        StartActivity(intent);
    }
}
```

Wenn die Aktivität erstellt wird, ruft Android die `OnActivityCreated`-Methode des Fragments auf. An dieser Stelle wird der Listenadapter für `ListView` erstellt.  Die `ShowQuoteFromPlay`-Methode startet eine Instanz von `PlayQuoteActivity`, um das Zitat für das ausgewählte Stück anzuzeigen.

## <a name="display-titlesfragment-in-mainactivity"></a>Anzeigen von TitlesFragment in MainActivity

Der letzte Schritt besteht darin, `TitlesFragment` in `MainActivity` anzuzeigen. Die Aktivität lädt das Fragment nicht dynamisch. Stattdessen wird das Fragment statisch geladen, indem es in der Layoutdatei der Aktivität mithilfe eines `fragment`-Elements deklariert wird. Das zu ladende Fragment wird durch Festlegen des `android:name`-Attributs auf die Fragmentklasse (einschließlich des Namespace des Typs) identifiziert. Wenn Sie z. B. `TitlesFragment` verwendet werden soll, wird `android:name` auf `FragmentSample.TitlesFragment`festgelegt.

Bearbeiten Sie die Layoutdatei **activity_main.axml**, und ersetzen Sie dabei den vorhandenen XML-Code durch Folgendes:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:orientation="horizontal"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    <fragment
        android:name="FragmentSample.TitlesFragment"
        android:id="@+id/titles"
        android:layout_width="match_parent"
        android:layout_height="match_parent" />
</LinearLayout>
```

> [!NOTE]
> Das `class`-Attribut ist ein gültiger Ersatz für `android:name`. Es gibt keine formale Empfehlung, welche Form bevorzugt wird. Es gibt viele Beispiele für Codebasen, die `class` austauschbar mit `android:name` verwenden.

Für MainActivity sind keine Codeänderungen erforderlich. Der Code in dieser Klasse sollte dem folgenden Codeausschnitt sehr ähnlich sein:

```csharp
[Activity(Label = "@string/app_name", Theme = "@style/AppTheme", MainLauncher = true)]
public class MainActivity : Activity
{
    protected override void OnCreate(Bundle savedInstanceState)
    {
        base.OnCreate(savedInstanceState);
        SetContentView(Resource.Layout.activity_main);
    }
}
```

## <a name="run-the-app"></a>Ausführen der App

Nachdem der Code nun vollständig ist, führen Sie die App auf einem Gerät aus, um sie in Aktion zu sehen.

[![Screenshots der Anwendung, die auf einem Smartphone ausgeführt wird.](./walkthrough-images/05-app-screenshots-sml.png)](./walkthrough-images/05-app-screenshots.png#lightbox)

[Teil 2 dieser exemplarischen Vorgehensweise](./walkthrough-landscape.md) optimiert diese Anwendung für Geräte, die im Querformat ausgeführt werden.
