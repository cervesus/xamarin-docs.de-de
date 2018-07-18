---
title: ViewPager mit Fragmenten
description: ViewPager handelt es sich um ein Layout-Manager, der gestural Navigation implementiert werden kann. Gestural Navigation kann der Benutzer streichen Sie nach links und rechts zum schrittweisen Durchlaufen von Datenseiten. Dieses Handbuch erläutert, wie eine swipeable Benutzeroberfläche mit ViewPager, die Verwendung von Fragmenten als die Datenseiten implementiert.
ms.prod: xamarin
ms.assetid: 62B6286F-3680-48F3-B91B-453692E457E5
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: 83b9123b24b39e2d7bda392d05c6595f5fc3af0d
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
ms.locfileid: "30772415"
---
# <a name="viewpager-with-fragments"></a>ViewPager mit Fragmenten

_ViewPager handelt es sich um ein Layout-Manager, der gestural Navigation implementiert werden kann. Gestural Navigation kann der Benutzer streichen Sie nach links und rechts zum schrittweisen Durchlaufen von Datenseiten. Dieses Handbuch erläutert, wie eine swipeable Benutzeroberfläche mit ViewPager, die Verwendung von Fragmenten als die Datenseiten implementiert._

 
## <a name="overview"></a>Übersicht

`ViewPager` wird häufig in Verbindung mit Fragmenten verwendet, sodass sie einfacher zu verwalten, den Lebenszyklus der einzelnen Seiten im ist die `ViewPager`. In dieser exemplarischen Vorgehensweise `ViewPager` dient zum Erstellen einer eine app namens **FlashCardPager** , die zeigt eine Reihe von Mathematikaufgaben für Flash-Karten. Jede Karte wird als ein Fragment implementiert. Der Benutzer Kundenkarte linken und rechten durch die Flash-Karten und tippt auf ein Problem mathematische Funktionen, um ihre Antwort anzuzeigen. Diese app erstellt eine `Fragment` Instanz für jede Karte und implementiert, ein Adapter abgeleitet `FragmentPagerAdapter`. In [Viewpager und Ansichten](~/android/user-interface/controls/view-pager/viewpager-and-views.md), erfolgte die meiste Arbeit `MainActivity` Lebenszyklusmethoden. In **FlashCardPager**, die meiste Arbeit erfolgt durch eine `Fragment` in einer der Lebenszyklusmethoden. 

Dieses Handbuch nicht behandelt die Grundlagen der Fragmente &ndash; , wenn Sie nicht mit Fragmenten in Xamarin.Android vertraut sind, finden Sie unter [Fragmente](~/android/platform/fragments/index.md) können Sie die ersten Schritte mit Fragmenten. 



## <a name="start-an-app-project"></a>Starten Sie ein App-Projekt

Erstellen Sie ein neues Android-Projekt namens **FlashCardPager**. Als Nächstes starten Sie den NuGet-Paket-Manager (Weitere Informationen zum Installieren von NuGet-Pakete finden Sie unter [Exemplarische Vorgehensweise: einschließlich eines NuGet in Ihrem Projekt](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)). Suchen und Installieren der **Xamarin.Android.Support.v4** Paket wie im erläutert [Viewpager und Ansichten](~/android/user-interface/controls/view-pager/viewpager-and-views.md). 



## <a name="add-an-example-data-source"></a>Fügen Sie eine Beispiel-Datenquelle hinzu

In **FlashCardPager**, die Datenquelle ist ein Deckblatt Flash-Karten, dargestellt durch die `FlashCardDeck` Klasse; diese Daten, die Datenquelle stellt die `ViewPager` mit Elementinhalt. `FlashCardDeck` enthält eine vorgefertigte Sammlung von Mathematikaufgaben und Antworten. Die `FlashCardDeck` Konstruktor erfordert keine Argumente: 

```csharp
FlashCardDeck flashCards = new FlashCardDeck();
```

Die Auflistung der Flash-Karten in `FlashCardDeck` wird so organisiert, dass jede Karte durch einen Indexer zugegriffen werden kann. Die folgende Codezeile ruft beispielsweise das vierte Flash-Karte Problem im Stapel ab: 

```csharp
string problem = flashCardDeck[3].Problem;
```

Diese Codezeile Ruft die entsprechende Antwort auf das zuvor beschriebene Problem ab:

```csharp
string answer = flashCardDeck[3].Answer;
```

Da die Implementierungsdetails der `FlashCardDeck` sind nicht mit der Funktionsweise relevanten `ViewPager`, die `FlashCardDeck` Code ist hier nicht aufgeführt.
Quellcode und `FlashCardDeck` finden Sie unter [FlashCardDeck.cs](https://github.com/xamarin/monodroid-samples/blob/master/UserInterface/FlashCardPager/FlashCardPager/FlashCardDeck.cs).
Laden Sie diese Quelldatei (oder kopieren Sie den Code in ein neues **FlashCardDeck.cs** Datei) und dem Projekt hinzugefügt.



## <a name="create-a-viewpager-layout"></a>Erstellen Sie ein Layout ViewPager

Open **Resources/layout/Main.axml** und Ersetzen Sie den Inhalt durch folgendes XML:

```xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.v4.view.ViewPager
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/viewpager"
    android:layout_width="match_parent"
    android:layout_height="match_parent" >

    </android.support.v4.view.ViewPager>
```

Diese XML-Datei definiert eine `ViewPager` , die den gesamten Bildschirm einnimmt. Beachten Sie, dass Sie den vollqualifizierten Namen verwenden, müssen **android.support.v4.view.ViewPager** da `ViewPager` wird in einer Unterstützungsbibliothek verpackt. `ViewPager` steht nur über die [Android Unterstützungsbibliothek v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/); es ist nicht im Android SDK verfügbar.


## <a name="set-up-viewpager"></a>ViewPager einrichten

Bearbeiten Sie **MainActivity.cs** und fügen Sie die folgenden `using` Anweisungen:

```csharp
using Android.Support.V4.View;
using Android.Support.V4.App;
```

Ändern der `MainActivity` -Klassendeklaration, damit sie von abgeleitet ist `AppCompatActivity`:

```csharp
public class MainActivity : FragmentActivity
```

`MainActivity` stammt aus`FragmentActivity` (statt `Activity`) da `FragmentActivity` weiß, wie die Unterstützung der Fragmente zu verwalten. Ersetzen Sie die `OnCreate`-Methode durch folgenden Code: 

```csharp
protected override void OnCreate(Bundle bundle)
{
    base.OnCreate(bundle);
    SetContentView(Resource.Layout.Main);
    ViewPager viewPager = FindViewById<ViewPager>(Resource.Id.viewpager);
    FlashCardDeck flashCards = new FlashCardDeck();
}
```

Dieser Code führt Folgendes aus:

1.  Legt die Ansicht aus der **Main.axml** Layout-Ressource.

2.  Ruft einen Verweis auf die `ViewPager` aus dem Layout.

3.  Instanziiert eine neue `FlashCardDeck` als Datenquelle.

Wenn Sie erstellen und diesen Code ausführen, sehen Sie eine Anzeige, die den folgenden Screenshot ähnelt: 

[![Screenshot der FlashCardPager-app mit leeren ViewPager](viewpager-and-fragments-images/01-initial-screen-sml.png)](viewpager-and-fragments-images/01-initial-screen.png#lightbox)

An diesem Punkt der `ViewPager` ist leer, da es die Fragmente fehlt, die verwendet werden zu füllen der `ViewPager`, und es fehlt einen Adapter zum Erstellen dieser Fragmente aus den Daten im **FlashCardDeck**. 

In den folgenden Abschnitten eine `FlashCardFragment` erstellen, um die Funktionalität der einzelnen Flash-Karte implementiert wird und eine `FragmentPagerAdapter` wird erstellt, um eine Verbindung herstellen der `ViewPager` um Fragmente erstellt aus den Daten in der `FlashCardDeck`. 



## <a name="create-the-fragment"></a>Erstellen Sie das Fragment

Jede Karte wird über ein UI-Fragment aufgerufen verwaltet `FlashCardFragment`. `FlashCardFragment`die Ansicht zeigt die Informationen, die mit einer einzelnen Flash-Karte enthalten. Jede Instanz des `FlashCardFragment` gehostet werden, indem Sie die `ViewPager`. 
`FlashCardFragment`die Ansicht besteht aus einer `TextView` , die der Karte Problem Text anzeigt. In dieser Ansicht wird einen Ereignishandler mithilfe implementieren eine `Toast` anzuzeigenden Antwort beim Tippen auf der Frage Flash-Karte. 



### <a name="create-the-flashcardfragment-layout"></a>Erstellen Sie das Layout FlashCardFragment

Vor dem `FlashCardFragment` möglich implementiert, muss das zugehörige Layout definiert werden. Dieses Layout ist kein Layout im Fragment Container für ein einzelnes Fragment. Fügen Sie ein neues Android Layout für **Ressourcen/Layout** aufgerufen **flashcard_layout.axml**. Open **Resources/layout/flashcard_layout.axml** und Ersetzen Sie den Inhalt durch folgenden Code: 

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

Dieses Layout definiert ein Fragment Flash Karte; jedes Fragment besteht aus einem `TextView` , die mathematische Problem beim Verwenden einer Schriftart groß (100sp) anzeigt. Dieser Text wird vertikal und horizontal auf die Karte zentriert. 



### <a name="create-the-initial-flashcardfragment-class"></a>Erstellen Sie die anfängliche FlashCardFragment-Klasse

Fügen Sie eine neue Datei namens **FlashCardFragment.cs** und Ersetzen Sie den Inhalt durch folgenden Code:

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

Dieser Code versieht die grundlegende `Fragment` Definition, die zum Anzeigen einer Karte verwendet werden. Beachten Sie, dass `FlashCardFragment` stammt aus dem Support Library-Version von `Fragment` in definierten `Android.Support.V4.App.Fragment`. Der Konstruktor leer ist, damit die `newInstance` Factorymethode dient zum Erstellen eines neuen `FlashCardFragment` anstelle eines Konstruktors. 

Die `OnCreateView` Lifecycle-Methode erstellt und konfiguriert die `TextView`. Es erweitert das Layout für des Fragments `TextView` und gibt die vergrößerte `TextView` an den Aufrufer. `LayoutInflater` und `ViewGroup` übergeben werden, um `OnCreateView` , damit sie das Layout vergrößern kann. Die `savedInstanceState` Paket enthält Daten, die `OnCreateView` erstellt die `TextView` aus einem gespeicherten Zustand. 

Das Fragment Ansicht wird durch den Aufruf von explizit vergrößert `inflater.Inflate`. Die `container` Argument ist der Ansicht übergeordneter, und die `false` Flag weist den Inflater, stellen sicher, dass die Sicht übergeordneten vergrößerte Ansicht hinzugefügt (es wird werden hinzugefügt, wenn `ViewPager` Aufruf des Adapters `GetItem` Methode weiter unten in diesem Exemplarische Vorgehensweise). 



### <a name="add-state-code-to-flashcardfragment"></a>FlashCardFragment State Code hinzufügen

Wie eine Aktivität ein Fragment enthält eine `Bundle` , speichern und Abrufen des Datenbankzustands verwendet. In **FlashCardPager**, gibt diese `Bundle` wird verwendet, um die Frage zu speichern, und beantworten Text für die zugeordnete Flash-Karte. In **FlashCardFragment.cs**, fügen Sie die folgenden `Bundle` Schlüssel an den Anfang der `FlashCardFragment` Klassendefinition: 

```csharp
private static string FLASH_CARD_QUESTION = "card_question";
private static string FLASH_CARD_ANSWER = "card_answer";
```

Ändern der `newInstance` Factory-Methode so, dass die It erstellt ein `Bundle` -Objekt und verwendet die oben aufgeführten Schlüssel zum Speichern von übergebenen Frage und beantworten Text im Fragment, nachdem er instanziiert wird: 

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

Ändern Sie die Fragment Lifecycle Methode `OnCreateView` zum Abrufen dieser Informationen aus dem Paket übergebene und laden den Fragetext auf die `TextBox`: 

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

Die `answer` Variable hier nicht verwendet wird, aber sie werden später verwendet, wenn diese Datei Ereignishandlercode hinzugefügt wird. 


## <a name="create-the-adapter"></a>Erstellen des Adapters

`ViewPager` verwendet ein Adapter-Controller-Objekt, das zwischen den `ViewPager` und der Datenquelle (finden Sie unter der Abbildung in der ViewPager [Adapter](~/android/user-interface/controls/view-pager/index.md#adapter) Artikel). Auf diese Daten zugreifen `ViewPager` erfordert, dass Sie einen benutzerdefinierten Adapter abgeleitet bereitstellen `PagerAdapter`. Da in diesem Beispiel Fragmenten verwendet, wird es einem `FragmentPagerAdapter` &ndash; `FragmentPagerAdapter` stammt aus `PagerAdapter`. 
`FragmentPagerAdapter` stellt jede Seite als ein `Fragment` , wird nach mehreren versuchen weiterhin im Fragment-Manager für beibehalten, solange der Benutzer auf der Seite zurückkehren kann. Als der Benutzer Kundenkarte durch Datenseiten der `ViewPager`, die `FragmentPagerAdapter` extrahiert Informationen aus der Datenquelle und zum Erstellen verwendet `Fragment`s für die `ViewPager` angezeigt. 

Beim Implementieren einer `FragmentPagerAdapter`, müssen Sie die folgenden überschreiben:

-   **Anzahl** &ndash; schreibgeschützte Eigenschaft, die die Anzahl der verfügbaren Ansichten (Seiten) zurückgibt.

-   **GetItem** &ndash; gibt das Fragment, das für die angegebene Seite angezeigt.

Fügen Sie eine neue Datei namens **FlashCardDeckAdapter.cs** und Ersetzen Sie den Inhalt durch folgenden Code:

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

Dieser Code versieht die grundlegende `FragmentPagerAdapter` Implementierung. In den folgenden Abschnitten wird jede dieser Methoden mit Arbeitscode ersetzt. Der Zweck des Konstruktors ist den Fragment-Manager übergeben der `FlashCardDeckAdapter`des Basisklassenkonstruktors. 



### <a name="implement-the-adapter-constructor"></a>Implementieren Sie den Adapterkonstruktor

Wenn die app instanziiert die `FlashCardDeckAdapter`, stellt einen Verweis auf das Fragment-Manager und eine instanziierte `FlashCardDeck`. Fügen Sie die folgenden Membervariable an den Anfang der `FlashCardDeckAdapter` -Klasse im **FlashCardDeckAdapter.cs**: 

```csharp
public FlashCardDeck flashCardDeck;
```

Fügen Sie die folgende Codezeile, die `FlashCardDeckAdapter` Konstruktor: 

```csharp
this.flashCardDeck = flashCards;
```

Diese Zeile der Code speichert die `FlashCardDeck` Instanz, die `FlashCardDeckAdapter` verwenden. 



### <a name="implement-count"></a>Anzahl der implementieren

Die `Count` Implementierung ist relativ einfach: Gibt die Anzahl der Flash-Karten zurück, in der Flash-Kartenstapel. Ersetzen Sie den Code `Count` durch folgenden Code: 

```csharp
public override int Count
{
    get { return flashCardDeck.NumCards; }
}
```


Die `NumCards` Eigenschaft `FlashCardDeck` gibt die Anzahl der Flash-Karten (Anzahl der Fragmente) in das DataSet zurück. 



### <a name="implement-getitem"></a>GetItem implementieren

Die `GetItem` Methodenrückgabe das Fragment, das der angegebenen Position zugeordnet. Wenn `GetItem` wird aufgerufen, für eine Position in der Flash-Kartenstapel gibt eine `FlashCardFragment` so konfiguriert, dass das Problem für die Karte an dieser Position angezeigt. Ersetzen Sie die `GetItem`-Methode durch folgenden Code: 

```csharp
public override Android.Support.V4.App.Fragment GetItem(int position)
{
    return (Android.Support.V4.App.Fragment)
        FlashCardFragment.newInstance (
            flashCardDeck[position].Problem, flashCardDeck[position].Answer);
}
```

Dieser Code führt Folgendes aus:

1.  Sucht nach der Mathematik-Problem-Zeichenfolge in der `FlashCardDeck` Deckblatt für die angegebene Position. 

2.  Sucht nach der Zeichenfolge "Antwort" in der `FlashCardDeck` Deckblatt für die angegebene Position. 

3.  Ruft die `FlashCardFragment` Factorymethode `newInstance`, und übergeben Sie die Verbindungszeichenfolgen für die Karte Problem und Antwort. 

4.  Erstellt und gibt eine neue Flash-Karte `Fragment` , die Frage und Antwort Text für diese Position enthält. 

Wenn die `ViewPager` rendert die `Fragment` am `position`, angezeigt die `TextBox` mit der Mathematik Problem-Zeichenfolge, die sich befinden, auf `position` in der Flash-Kartenstapel. 



## <a name="add-the-adapter-to-the-viewpager"></a>Hinzufügen des Adapters zu den ViewPager

Nun, dass die `FlashCardDeckAdapter` wird implementiert, es ist Zeit, die sie zum Hinzufügen der `ViewPager`. In **MainActivity.cs**, fügen Sie die folgende Codezeile am Ende der `OnCreate` Methode:

```csharp
FlashCardDeckAdapter adapter =
    new FlashCardDeckAdapter(SupportFragmentManager, flashCards);
viewPager.Adapter = adapter;
```

Dieser Code instanziiert die `FlashCardDeckAdapter`, und übergeben Sie die `SupportFragmentManager` im ersten Argument. (Die `SupportFragmentManager` Eigenschaft FragmentActivity dient zum Abrufen eines Verweises auf die `FragmentManager` &ndash; für Weitere Informationen zu den `FragmentManager`, finden Sie unter [Verwalten von Fragmenten](~/android/platform/fragments/managing-fragments.md).) 

Die basisimplementierung ist jetzt vollständig &ndash; erstellen und Ausführen der app.
Daraufhin sollte das erste Bild von der Flash Kartenstapel auf dem Bildschirm angezeigt werden, wie auf der linken Seite im nächsten Screenshot dargestellt. Streichen Sie nach links, um weitere Flash-Karten, finden Sie unter dann streichen Sie nach rechts, um wieder zum Bewegen in der Flash-Kartenstapel:

[![Beispiel Screenshots FlashCardPager app ohne Pager-Indikatoren](viewpager-and-fragments-images/02-example-views-sml.png)](viewpager-and-fragments-images/02-example-views.png#lightbox)



## <a name="add-a-pager-indicator"></a>Hinzufügen eines Indikators Pager

Dieser minimale `ViewPager` Implementierung zeigt jede Karte in der Deckblatt, verfügt aber keine Hinweis aufgeführt, wo der Benutzer innerhalb der Deckblatt ist. Der nächste Schritt besteht, zum Hinzufügen einer `PagerTabStrip`. Die `PagerTabStrip` informiert den Benutzer hinsichtlich des welches Problem Anzahl angezeigt wird, und bietet navigationskontexts durch einen Hinweis auf die vorherige und kommende Flash-Karten anzeigen. 

Open **Resources/layout/Main.axml** und Hinzufügen einer `PagerTabStrip` des Layouts:

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

Wenn Sie erstellen und der app ausführen, sehen Sie die leere `PagerTabStrip` am oberen Rand jeder Karte angezeigt: 

[![Nahaufnahme der PagerTabStrip ohne text](viewpager-and-fragments-images/03-empty-pagetabstrip-sml.png)](viewpager-and-fragments-images/03-empty-pagetabstrip.png#lightbox)



### <a name="display-a-title"></a>Anzeigen eines Titels

Zum Hinzufügen eines Titels zu jeder Seitenregisterkarte implementieren die `GetPageTitleFormatted` Methode im Adapter. `ViewPager` Aufrufe `GetPageTitleFormatted` (falls implementiert zurzeit) Titelzeichenfolge abrufen, die der Seite an der angegebenen Position beschrieben. Fügen Sie die folgende Methode, die `FlashCardDeckAdapter` -Klasse im **FlashCardDeckAdapter.cs**: 

```csharp
public override Java.Lang.ICharSequence GetPageTitleFormatted(int position)
{
    return new Java.Lang.String("Problem " + (position + 1));
}
```

Dieser Code konvertiert die Position in der Flash-Kartenstapel in ein Problem. Die resultierende Zeichenfolge wird in einer Java konvertiert `String` zurückgegeben wird, um die `ViewPager`. Beim Ausführen der app mit dieser neuen Methode zeigt jede Seite die Problem Zahl in die `PagerTabStrip`: 

[![Screenshots der FlashCardPager mit dem Problem über jede Seite angezeigt](viewpager-and-fragments-images/04-pagetabstrip-sml.png)](viewpager-and-fragments-images/04-pagetabstrip.png#lightbox)

Sie können hin und her navigieren, um die Anzahl der Problem in der Flash-Kartenstapel anzuzeigen, die am oberen Rand jeder Karte angezeigt wird. 



## <a name="handle-user-input"></a>Behandeln von Benutzereingaben

**FlashCardPager** enthält eine Reihe von Fragment-basierten Flash Karten in einem `ViewPager`, aber noch keine Möglichkeit, die Antwort für das jeweilige Problem anzuzeigen. In diesem Abschnitt wird ein Ereignishandler hinzugefügt, um die `FlashCardFragment` an die Antwort angezeigt wird, wenn der Benutzer auf der Karte Problem Text tippt. 

Open **FlashCardFragment.cs** und fügen Sie den folgenden Code am Ende der `OnCreateView` Methode unmittelbar vor die Sicht an den Aufrufer zurückgegeben wird: 

```csharp
questionBox.Click += delegate
{
    Toast.MakeText(Activity.ApplicationContext,
            "Answer: " + answer, ToastLength.Short).Show();
};
```

Dies `Click` Ereignishandler zeigt die Antwort in einer Toast, die angezeigt wird, wenn der Benutzer tippt der `TextBox`. Die `answer` Variable wurde zuvor initialisiert, wenn die Zustandsinformationen aus dem Paket gelesen wurde, die übergeben wurde `OnCreateView`. Erstellen Sie, führen Sie die app, und tippen Sie auf den Text Problem auf jeden Flash-Karte, um die Antwort finden Sie unter: 

[![Screenshots der FlashCardPager app Popups beim mathematische Problem abgerufen werden](viewpager-and-fragments-images/05-answer-sml.png)](viewpager-and-fragments-images/05-answer.png#lightbox)

Der **FlashCardPager** dargestellt, die in dieser exemplarischen Vorgehensweise verwendet einen `MainActivity` abgeleitet `FragmentActivity`, aber Sie können auch ableiten `MainActivity` aus `AppCompatActivity` (die bietet auch Unterstützung für die Verwaltung von Fragmenten). Anzeigen einer `AppCompatActivity` Beispiel finden Sie unter [FlashCardPager](https://developer.xamarin.com/samples/monodroid/UserInterface%5CFlashCardPager/) in der Beispiel-Katalog. 



## <a name="summary"></a>Zusammenfassung

In dieser exemplarischen Vorgehensweise bereitgestellt ein ausführliches Beispiel zum Erstellen einer grundlegenden `ViewPager`-basierte Anwendung mit `Fragment`s. Sie eine Beispiel-Datenquelle mit Flash-Karte Fragen und Antworten angezeigt ein `ViewPager` Layout der Flash-Karten anzeigen und eine `FragmentPagerAdapter` Unterklasse, die eine Verbindung herstellt der `ViewPager` mit der Datenquelle. Um Benutzer über die Flash-Karten navigieren zu erleichtern, wurden Anweisungen enthalten, die erläutern das Hinzufügen einer `PagerTabStrip` Problem am oberen Rand jeder Seite anzeigen. Schließlich wurde Ereignishandlercode hinzugefügt, um die Antwort angezeigt wird, wenn der Benutzer auf ein Problem Flash-Karte tippt. 



## <a name="related-links"></a>Verwandte Links

- [FlashCardPager (sample)](https://developer.xamarin.com/samples/monodroid/UserInterface/FlashCardPager)
