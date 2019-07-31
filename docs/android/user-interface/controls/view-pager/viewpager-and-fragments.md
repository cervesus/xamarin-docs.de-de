---
title: ViewPager mit Fragmenten
description: Viewpager ist ein Layoutmanager, mit dem Sie die Gestural-Navigation implementieren können. Die Gestural-Navigation ermöglicht dem Benutzer das Schwenken von Links und rechts zum Durchlaufen von Datenseiten. In diesem Handbuch wird erläutert, wie Sie mithilfe von Fragmenten als Datenseiten eine Benutzeroberfläche mit "viewpager" implementieren können.
ms.prod: xamarin
ms.assetid: 62B6286F-3680-48F3-B91B-453692E457E5
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
ms.openlocfilehash: 90bffc2360654f571728f76810f144e702a81e57
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68646102"
---
# <a name="viewpager-with-fragments"></a>ViewPager mit Fragmenten

_Viewpager ist ein Layoutmanager, mit dem Sie die Gestural-Navigation implementieren können. Die Gestural-Navigation ermöglicht dem Benutzer das Schwenken von Links und rechts zum Durchlaufen von Datenseiten. In diesem Handbuch wird erläutert, wie Sie mithilfe von Fragmenten als Datenseiten eine Benutzeroberfläche mit "viewpager" implementieren können._

 
## <a name="overview"></a>Übersicht

`ViewPager`wird häufig in Verbindung mit Fragmenten verwendet, sodass es einfacher ist, den Lebenszyklus jeder Seite in der `ViewPager`zu verwalten. In dieser `ViewPager` exemplarischen Vorgehensweise wird zum Erstellen einer App namens " **Flash cardpager** " verwendet, die eine Reihe mathematischer Probleme auf Flash Karten darstellt. Jede Flash Karte wird als Fragment implementiert. Der Benutzer blinkt nach links und rechts durch die Flash Karten und tippt auf ein mathematisches Problem, um seine Antwort anzuzeigen. Diese App erstellt eine `Fragment` -Instanz für jede Flash Karte und implementiert einen von `FragmentPagerAdapter`abgeleiteten Adapter. In [viewpager und Ansichten](~/android/user-interface/controls/view-pager/viewpager-and-views.md)wurden die meisten Aufgaben in `MainActivity` Lebenszyklus Methoden durchgeführt. In " **flashcardpager**" wird der größte Teil der Arbeit von einem `Fragment` in einer seiner Lebenszyklus Methoden durchgeführt. 

In dieser Anleitung werden die Grundlagen von Fragmenten &ndash; nicht behandelt, wenn Sie mit Fragmenten in xamarin. Android noch nicht vertraut sind. Weitere Informationen finden Sie unter [Fragmente](~/android/platform/fragments/index.md) , die Ihnen beim Einstieg in Fragmente helfen. 



## <a name="start-an-app-project"></a>Starten eines App-Projekts

Erstellen Sie ein neues Android-Projekt namens " **Flash cardpager**". Starten Sie als nächstes den nuget-Paket-Manager (Weitere Informationen zum Installieren von nuget [-Paketen finden Sie unter Exemplarische Vorgehensweise: Einschließen eines nuget-Projekts in](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)Ihr Projekt). Suchen und installieren Sie das **xamarin. Android. Support. v4** -Paket, wie unter [viewpager und Views](~/android/user-interface/controls/view-pager/viewpager-and-views.md)erläutert. 



## <a name="add-an-example-data-source"></a>Hinzufügen einer Beispiel Datenquelle

In " **flashcardpager**" ist die Datenquelle ein Stapel von Flash Karten, die `FlashCardDeck` von der-Klasse dargestellt werden. `ViewPager` diese Datenquelle liefert den Element Inhalt. `FlashCardDeck`enthält eine vorgefertigte Auflistung von mathematischen Problemen und Antworten. Der `FlashCardDeck` Konstruktor erfordert keine Argumente: 

```csharp
FlashCardDeck flashCards = new FlashCardDeck();
```

Die Sammlung von Flash Karten in `FlashCardDeck` ist so organisiert, dass auf jede Flash Karte durch einen Indexer zugegriffen werden kann. Mit der folgenden Codezeile wird beispielsweise das vierte Flash Karten Problem im Stapel abgerufen: 

```csharp
string problem = flashCardDeck[3].Problem;
```

Diese Codezeile Ruft die entsprechende Antwort auf das vorherige Problem ab:

```csharp
string answer = flashCardDeck[3].Answer;
```

Da die Implementierungsdetails von `FlashCardDeck` nicht für das Verständnis `ViewPager`relevant sind, `FlashCardDeck` wird der Code hier nicht aufgeführt.
Der Quellcode für `FlashCardDeck` ist unter [FlashCardDeck.cs](https://github.com/xamarin/monodroid-samples/blob/master/UserInterface/FlashCardPager/FlashCardPager/FlashCardDeck.cs)verfügbar.
Laden Sie diese Quelldatei herunter (oder kopieren Sie den Code, und fügen Sie ihn in eine neue **FlashCardDeck.cs** -Datei ein), und fügen Sie ihn dem Projekt hinzu



## <a name="create-a-viewpager-layout"></a>Erstellen eines viewpager-Layouts

Öffnen Sie **Resources/Layout/Main. axml** , und ersetzen Sie den Inhalt durch den folgenden XML-Code:

```xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.v4.view.ViewPager
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/viewpager"
    android:layout_width="match_parent"
    android:layout_height="match_parent" >

    </android.support.v4.view.ViewPager>
```

Dieser XML-Code `ViewPager` definiert einen, der den gesamten Bildschirm einnimmt. Beachten Sie, dass Sie den voll qualifizierten Namen **Android. Support. v4. View. viewpager** verwenden müssen `ViewPager` , da in einer Unterstützungs Bibliothek verpackt ist. `ViewPager`ist nur in der [Android-Unterstützungs Bibliothek V4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)verfügbar. Er ist im Android SDK nicht verfügbar.


## <a name="set-up-viewpager"></a>Einrichten von "viewpager"

Bearbeiten Sie **MainActivity.cs** , und fügen `using` Sie die folgenden Anweisungen hinzu:

```csharp
using Android.Support.V4.View;
using Android.Support.V4.App;
```

Ändern Sie `MainActivity` die Klassen Deklaration, sodass Sie von `FragmentActivity`abgeleitet ist:

```csharp
public class MainActivity : FragmentActivity
```

`MainActivity`wird von`FragmentActivity` ( `Activity`statt) abgeleitet, da `FragmentActivity` die Verwaltung der Unterstützung von Fragmenten kennt. Ersetzen Sie die `OnCreate`-Methode durch folgenden Code: 

```csharp
protected override void OnCreate(Bundle bundle)
{
    base.OnCreate(bundle);
    SetContentView(Resource.Layout.Main);
    ViewPager viewPager = FindViewById<ViewPager>(Resource.Id.viewpager);
    FlashCardDeck flashCards = new FlashCardDeck();
}
```

Dieser Code führt folgende Schritte aus:

1.  Legt die Ansicht aus der **Main. axml** -layoutressource fest.

2.  Ruft einen Verweis auf den `ViewPager` aus dem Layout ab.

3.  Instanziiert eine neue `FlashCardDeck` als Datenquelle.

Wenn Sie diesen Code erstellen und ausführen, sollte eine Anzeige angezeigt werden, die dem folgenden Screenshot ähnelt: 

[![Screenshot der App "Flash cardpager" mit leerem viewpager](viewpager-and-fragments-images/01-initial-screen-sml.png)](viewpager-and-fragments-images/01-initial-screen.png#lightbox)

An diesem Punkt ist der `ViewPager` leer, da er nicht über die verwendeten `ViewPager`Fragmente verfügt und einen Adapter zum Erstellen dieser Fragmente aus den Daten in **Flash cardkarten**fehlt. 

In den folgenden Abschnitten wird eine `FlashCardFragment` erstellt, um die Funktionalität der einzelnen Flash Karten zu implementieren, und `FragmentPagerAdapter` eine wird erstellt, um `ViewPager` die mit den Fragmenten zu verbinden, die `FlashCardDeck`aus den Daten im erstellt wurden. 



## <a name="create-the-fragment"></a>Erstellen des Fragments

Jede Flash Karte wird von einem UI-Fragment mit dem `FlashCardFragment`Namen verwaltet. `FlashCardFragment`in der Ansicht werden die Informationen angezeigt, die in einer einzelnen Flash Karte enthalten sind. Jede Instanz von `FlashCardFragment` wird von der `ViewPager`gehostet. 
`FlashCardFragment`die Ansicht besteht aus einer `TextView` , in der der Text des Flash Karten Problems angezeigt wird. In dieser Ansicht wird ein Ereignishandler implementiert, der `Toast` einen verwendet, um die Antwort anzuzeigen, wenn der Benutzer auf die Flash Karten Frage tippt. 



### <a name="create-the-flashcardfragment-layout"></a>Erstellen des "Flash Fragment"-Layouts

Bevor `FlashCardFragment` implementiert werden kann, muss das Layout definiert werden. Dieses Layout ist ein fragmentcontainerlayout für ein einzelnes Fragment. Fügen Sie ein neues Android-Layout zu **Ressourcen/Layout** namens **flashcard_layout. axml**hinzu. Öffnen Sie **Resources/Layout/flashcard_layout. axml** , und ersetzen Sie den Inhalt durch den folgenden Code: 

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    <TextView
        android:id="@+id/flash_card_question"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:gravity="center"
            android:textAppearance="@android:style/TextAppearance.Large"
            android:textSize="100sp"
            android:layout_centerHorizontal="true"
            android:layout_centerVertical="true"
            android:text="Question goes here" />
    </RelativeLayout>
```

Dieses Layout definiert ein einzelnes Flash Karten Fragment. jedes Fragment besteht aus einem `TextView` , das ein mathematisches Problem mithilfe einer großen (100 SP) Schriftart anzeigt. Dieser Text wird vertikal und horizontal auf der Flash Karte zentriert. 



### <a name="create-the-initial-flashcardfragment-class"></a>Erstellen der anfänglichen "Flash cardfragment"-Klasse

Fügen Sie eine neue Datei mit dem Namen **FlashCardFragment.cs** hinzu, und ersetzen Sie Ihren Inhalt durch den folgenden Code:

```csharp
using System;
using Android.OS;
using Android.Views;
using Android.Widget;
using Android.Support.V4.App;

namespace FlashCardPager
{
    public class FlashCardFragment : Android.Support.V4.App.Fragment
    {
        public FlashCardFragment() { }

        public static FlashCardFragment newInstance(String question, String answer)
        {
            FlashCardFragment fragment = new FlashCardFragment();
            return fragment;
        }
        public override View OnCreateView (
            LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState)
        {
            View view = inflater.Inflate (Resource.Layout.flashcard_layout, container, false);
            TextView questionBox = (TextView)view.FindViewById (Resource.Id.flash_card_question);
            return view;
        }
    }
}
```

Mit diesem Code wird die erforderliche `Fragment` Definition für die Anzeige einer Flash Karte erstellt. Beachten Sie `FlashCardFragment` , dass von der Version der Unterstützungs Bibliothek `Fragment` von abgeleitet `Android.Support.V4.App.Fragment`ist, die in definiert ist. Der Konstruktor ist leer, sodass die `newInstance` Factorymethode zum Erstellen eines neuen `FlashCardFragment` anstelle eines Konstruktors verwendet wird. 

Die `OnCreateView` Lifecycle-Methode erstellt und konfiguriert `TextView`. Dadurch wird das Layout für das Fragment vergrößert `TextView` `TextView` , und das-Objekt wird an den Aufrufer zurückgegeben. `LayoutInflater`und `ViewGroup` werden an den `OnCreateView` -Wert an übermittelt, sodass er das Layout erhöhen kann. Das `savedInstanceState` Paket enthält Daten, `OnCreateView` die `TextView` von verwendet werden, um aus einem gespeicherten Zustand neu zu erstellen. 

Die fragmentansicht wird durch den Aufrufen `inflater.Inflate`von explizit aufgeblasen. Das `container` -Argument ist das übergeordnete Element der Sicht `false` , und das-Flag weist den Inflater hat an, das übergeordnete Element nicht der übergeordneten Ansicht hinzuzufügen (es `ViewPager` wird hinzugefügt, wenn `GetItem` die-Methode des Adapters später in diesem Exemplarische Vorgehensweise). 



### <a name="add-state-code-to-flashcardfragment"></a>Hinzufügen von Zustands Code zu "Flash Fragment"

Wie eine-Aktivität verfügt ein Fragment über `Bundle` einen, der zum Speichern und Abrufen seines Zustands verwendet wird. In **flashcardpager**wird dies `Bundle` verwendet, um die Frage und den Antworttext für die zugehörige Flash Karte zu speichern. Fügen Sie in **FlashCardFragment.cs**am Anfang `Bundle` `FlashCardFragment` der Klassendefinition die folgenden Schlüssel hinzu: 

```csharp
private static string FLASH_CARD_QUESTION = "card_question";
private static string FLASH_CARD_ANSWER = "card_answer";
```

Ändern Sie `newInstance` die Factorymethode so, `Bundle` dass Sie ein-Objekt erstellt und die oben genannten Schlüssel verwendet, um die übergebenen Frage und den Antworttext im Fragment zu speichern, nachdem Sie instanziiert wurde: 

```csharp
public static FlashCardFragment newInstance(String question, String answer)
{
    FlashCardFragment fragment = new FlashCardFragment();

    Bundle args = new Bundle();
    args.PutString(FLASH_CARD_QUESTION, question);
    args.PutString(FLASH_CARD_ANSWER, answer);
    fragment.Arguments = args;

    return fragment;
}
```

Ändern Sie die Methode `OnCreateView` des fragmentlebens Zyklus, um diese Informationen aus dem bestandenen Bündel abzurufen und den Frage Text in die `TextBox`zu laden: 

```csharp
public override View OnCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState)
{
    string question = Arguments.GetString(FLASH_CARD_QUESTION, "");
    string answer = Arguments.GetString(FLASH_CARD_ANSWER, "");

    View view = inflater.Inflate(Resource.Layout.flashcard_layout, container, false);
    TextView questionBox = (TextView)view.FindViewById(Resource.Id.flash_card_question);
    questionBox.Text = question;

    return view;
}
```

Die `answer` Variable wird hier nicht verwendet, Sie wird jedoch später verwendet, wenn dieser Datei Ereignishandlercode hinzugefügt wird. 


## <a name="create-the-adapter"></a>Erstellen des Adapters

`ViewPager`verwendet ein Adapter Controller Objekt, das sich zwischen `ViewPager` der-und der-Datenquelle befindet (siehe Abbildung im Artikel viewpager- [Adapter](~/android/user-interface/controls/view-pager/index.md#adapter) ). Für den Zugriff auf diese `ViewPager` Daten erfordert, dass Sie einen benutzerdefinierten Adapter `PagerAdapter`bereitstellen, der von abgeleitet ist. Da in diesem Beispiel Fragmente verwendet werden, wird `FragmentPagerAdapter` eine &ndash; `FragmentPagerAdapter` von `PagerAdapter`abgeleitet. 
`FragmentPagerAdapter`stellt jede Seite als `Fragment` dar, die permanent im fragmentmanager beibehalten wird, solange der Benutzer zur Seite zurückkehren kann. Wenn der Benutzer `ViewPager`die Seiten des durchläuft, extrahiert der `FragmentPagerAdapter` Informationen aus der Datenquelle und verwendet diese zum Erstellen `Fragment`von s für die `ViewPager` anzuzeigende. 

Wenn Sie einen `FragmentPagerAdapter`implementieren, müssen Sie Folgendes überschreiben:

-   **Anzahl** &ndash; Schreibgeschützte Eigenschaft, die die Anzahl der verfügbaren Sichten (Seiten) zurückgibt.

-   **GetItem** &ndash; Gibt das Fragment zurück, das für die angegebene Seite angezeigt werden soll.

Fügen Sie eine neue Datei mit dem Namen **FlashCardDeckAdapter.cs** hinzu, und ersetzen Sie Ihren Inhalt durch den folgenden Code:

```csharp
using System;
using Android.Views;
using Android.Widget;
using Android.Support.V4.App;

namespace FlashCardPager
{
    class FlashCardDeckAdapter : FragmentPagerAdapter
    {
        public FlashCardDeckAdapter (Android.Support.V4.App.FragmentManager fm, FlashCardDeck flashCards)
            : base(fm)
        {
        }

        public override int Count
        {
            get { throw new NotImplementedException(); }
        }

        public override Android.Support.V4.App.Fragment GetItem(int position)
        {
            throw new NotImplementedException();
        }
    }
}
```

Mit diesem Code wird die wesentliche `FragmentPagerAdapter` -Implementierung Stubel. In den folgenden Abschnitten wird jede dieser Methoden durch funktionierenden Code ersetzt. Der Zweck des Konstruktors besteht darin, den fragmentmanager an den `FlashCardDeckAdapter`Basisklassenkonstruktor von zu übergeben. 



### <a name="implement-the-adapter-constructor"></a>Implementieren des adapterkonstruktors

Wenn die APP instanziiert `FlashCardDeckAdapter`, liefert Sie einen Verweis auf den fragmentmanager und eine instanziierte. `FlashCardDeck` Fügen Sie die folgende Member-Variable am Anfang der `FlashCardDeckAdapter` -Klasse in **FlashCardDeckAdapter.cs**hinzu: 

```csharp
public FlashCardDeck flashCardDeck;
```

Fügen Sie dem `FlashCardDeckAdapter` Konstruktor die folgende Codezeile hinzu: 

```csharp
this.flashCardDeck = flashCards;
```

Diese Codezeile speichert die `FlashCardDeck` -Instanz, die `FlashCardDeckAdapter` von verwendet wird. 



### <a name="implement-count"></a>Anzahl implementieren

Die `Count` Implementierung ist relativ einfach: Sie gibt die Anzahl von Flash Karten im Flash Kartenstapel zurück. Ersetzen Sie den Code `Count` durch folgenden Code: 

```csharp
public override int Count
{
    get { return flashCardDeck.NumCards; }
}
```


Die `NumCards` -Eigenschaft `FlashCardDeck` von gibt die Anzahl der Flash Karten (Anzahl der Fragmente) im DataSet zurück. 



### <a name="implement-getitem"></a>Implementieren von GetItem

Die `GetItem` -Methode gibt das Fragment zurück, das der angegebenen Position zugeordnet ist. Wenn `GetItem` für eine Position im Flash Kartenstapel aufgerufen wird, wird ein `FlashCardFragment` -Wert zurückgegeben, der für die Anzeige des Flash Karten Problems an dieser Stelle konfiguriert ist. Ersetzen Sie die `GetItem`-Methode durch folgenden Code: 

```csharp
public override Android.Support.V4.App.Fragment GetItem(int position)
{
    return (Android.Support.V4.App.Fragment)
        FlashCardFragment.newInstance (
            flashCardDeck[position].Problem, flashCardDeck[position].Answer);
}
```

Dieser Code führt folgende Schritte aus:

1.  Sucht die mathematische Problem Zeichenfolge im `FlashCardDeck` Stapel für die angegebene Position. 

2.  Sucht die Antwort Zeichenfolge im `FlashCardDeck` Stapel für die angegebene Position. 

3.  Ruft die `FlashCardFragment` Factory- `newInstance`Methode auf und übergibt das Flash Karten Problem und die Antwort Zeichenfolgen. 

4.  Erstellt eine neue Flash Karte `Fragment` , die den Frage-und Antworttext für diese Position enthält, und gibt diese zurück. 

`Fragment` `TextBox` Wenn das bei `ViewPager` `position` rendert, wird das mit der mathematischen Problem Zeichenfolge angezeigt, die sich auf dem Flash Kartenstapel befindet. `position` 



## <a name="add-the-adapter-to-the-viewpager"></a>Hinzufügen des Adapters zum viewpager

Nachdem der `FlashCardDeckAdapter` implementiert wurde, ist es an der Zeit, ihn dem `ViewPager`hinzuzufügen. Fügen Sie in **MainActivity.cs**die folgende Codezeile am Ende der `OnCreate` -Methode hinzu:

```csharp
FlashCardDeckAdapter adapter =
    new FlashCardDeckAdapter(SupportFragmentManager, flashCards);
viewPager.Adapter = adapter;
```

Dieser Code instanziiert den `FlashCardDeckAdapter`und übergibt `SupportFragmentManager` in das erste Argument. (Die `SupportFragmentManager` -Eigenschaft von fragmentactivity wird verwendet, um einen Verweis auf `FragmentManager` den &ndash; zu erhalten. weitere `FragmentManager`Informationen zu finden Sie unter [Verwalten von Fragmenten](~/android/platform/fragments/managing-fragments.md).) 

Die Kern Implementierung ist nun Complete &ndash; Build und führt die APP aus.
Das erste Bild des Flash Karten Stapels sollte auf dem Bildschirm angezeigt werden, wie auf der linken Seite im nächsten Screenshot dargestellt. Wischen Sie nach links, um weitere Flash Karten anzuzeigen, und wischen Sie nach rechts, um durch den Flash Kartenstapel zu wechseln:

[![Beispiel-Screenshots der App "Flash cardpager" ohne Pager-Indikatoren](viewpager-and-fragments-images/02-example-views-sml.png)](viewpager-and-fragments-images/02-example-views.png#lightbox)



## <a name="add-a-pager-indicator"></a>Hinzufügen eines Pager-Indikators

Diese minimale `ViewPager` Implementierung zeigt die einzelnen Flash Karten im Kartenstapel an, gibt jedoch keinen Hinweis darauf, wo sich der Benutzer im Stapel befindet. Der nächste Schritt besteht darin, eine `PagerTabStrip`hinzuzufügen. Der `PagerTabStrip` informiert den Benutzer darüber, welche Problem Nummer angezeigt wird, und stellt Navigations Kontext durch Anzeigen eines Hinweises der vorherigen und der nächsten Flash Karten bereit. 

Öffnen Sie **Resources/Layout/Main. axml** , und `PagerTabStrip` fügen Sie dem Layout ein hinzu:

```xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.v4.view.ViewPager xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/pager"
    android:layout_width="match_parent"
    android:layout_height="match_parent" >

  <android.support.v4.view.PagerTabStrip
      android:layout_width="match_parent"
      android:layout_height="wrap_content"
      android:layout_gravity="top"
      android:paddingBottom="10dp"
      android:paddingTop="10dp"
      android:textColor="#fff" />

</android.support.v4.view.ViewPager>
```

Wenn Sie die APP erstellen und ausführen, sollte die leere `PagerTabStrip` am oberen Rand jeder Flash Karte angezeigt werden: 

[![Closeup von PagerTabStrip ohne Text](viewpager-and-fragments-images/03-empty-pagetabstrip-sml.png)](viewpager-and-fragments-images/03-empty-pagetabstrip.png#lightbox)



### <a name="display-a-title"></a>Anzeigen eines Titels

Um jeder Seiten Registerkarte einen Titel hinzuzufügen, implementieren `GetPageTitleFormatted` Sie die-Methode im Adapter. `ViewPager`Ruft `GetPageTitleFormatted` (falls implementiert) zum Abrufen der Titel Zeichenfolge auf, die die Seite an der angegebenen Position beschreibt. Fügen Sie der- `FlashCardDeckAdapter` Klasse in **FlashCardDeckAdapter.cs**die folgende Methode hinzu: 

```csharp
public override Java.Lang.ICharSequence GetPageTitleFormatted(int position)
{
    return new Java.Lang.String("Problem " + (position + 1));
}
```

Mit diesem Code wird die Position im Flash Kartenstapel in eine Problem Nummer konvertiert. Die resultierende Zeichenfolge wird in Java `String` konvertiert, das `ViewPager`an zurückgegeben wird. Wenn Sie die APP mit dieser neuen Methode ausführen, wird auf jeder Seite die Problem Nummer in `PagerTabStrip`der angezeigt: 

[![Screenshots von Flash Manager, bei denen die Problem Nummer oberhalb jeder Seite angezeigt wird](viewpager-and-fragments-images/04-pagetabstrip-sml.png)](viewpager-and-fragments-images/04-pagetabstrip.png#lightbox)

Sie können hin und her schwenken, um die Problem Nummer im Flash Kartenstapel anzuzeigen, der am oberen Rand jeder Flash Karte angezeigt wird. 



## <a name="handle-user-input"></a>Behandeln von Benutzereingaben

" **Flash cardpager** " stellt eine Reihe von fragmentbasierten Flash Karten `ViewPager`in einem dar, verfügt aber noch nicht über eine Möglichkeit, die Antwort für jedes Problem zu verdeutlichen. In diesem Abschnitt wird ein Ereignishandler zum hinzugefügt `FlashCardFragment` , um die Antwort anzuzeigen, wenn der Benutzer auf den Text des Flash Karten Problems tippt. 

Öffnen Sie **FlashCardFragment.cs** , und fügen Sie den folgenden Code am Ende `OnCreateView` der Methode hinzu, kurz bevor die Ansicht an den Aufrufer zurückgegeben wird: 

```csharp
questionBox.Click += delegate
{
    Toast.MakeText(Activity.ApplicationContext,
            "Answer: " + answer, ToastLength.Short).Show();
};
```

Dieser `Click` Ereignishandler zeigt die Antwort in einem Toast an, der angezeigt wird, wenn `TextBox`der Benutzer auf das tippt. Die `answer` Variable wurde zuvor initialisiert, als Zustandsinformationen aus dem Paket gelesen wurden, das an `OnCreateView`weitergegeben wurde. Erstellen und führen Sie die APP aus, und tippen Sie dann auf den Problem Text auf den einzelnen Flash Karten, um die Antwort anzuzeigen: 

[![Screenshots von appcardpager-App-Umfassungen, wenn das mathematische Problem getippt wird](viewpager-and-fragments-images/05-answer-sml.png)](viewpager-and-fragments-images/05-answer.png#lightbox)

Der in dieser exemplarischen Vorgehensweise vorgestellte " **Flash cardpager** " verwendet einen `MainActivity` , der von `MainActivity` `FragmentActivity`abgeleitet `AppCompatActivity` ist, aber Sie können auch von ableiten (was auch die Verwaltung von Fragmenten unterstützt). Ein `AppCompatActivity` Beispiel finden Sie unter [Flash cardpager](https://docs.microsoft.com/samples/xamarin/monodroid-samples/userinterface-flashcardpager) im Beispiel Katalog.



## <a name="summary"></a>Zusammenfassung

In dieser exemplarischen Vorgehensweise wurde ein schrittweises Beispiel für das Erstellen einer einfachen `ViewPager`-basierten App mit `Fragment`s bereitgestellt. Es wurde eine Beispiel Datenquelle mit Flash Karten Fragen und-Antworten angezeigt `ViewPager` , ein Layout zum Anzeigen der Flash Karten und `FragmentPagerAdapter` eine Unterklasse, die `ViewPager` die mit der Datenquelle verbindet. Um dem Benutzer die Navigation durch die Flash Karten zu erleichtern, sind Anweisungen enthalten, die erläutern, `PagerTabStrip` wie ein hinzugefügt wird, um die Problem Nummer am oberen Rand jeder Seite anzuzeigen. Schließlich wurde der Ereignis Behandlungs Code hinzugefügt, um die Antwort anzuzeigen, wenn der Benutzer auf ein Flash Karten Problem tippt. 



## <a name="related-links"></a>Verwandte Links

- [Flashcardpager (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/userinterface-flashcardpager)
