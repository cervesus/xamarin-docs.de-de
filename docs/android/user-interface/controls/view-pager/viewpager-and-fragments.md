---
title: ViewPager mit Fragmenten
description: Viewpager ist ein Layoutmanager, mit dem Sie die Gestural-Navigation implementieren können. Die Gestural-Navigation ermöglicht dem Benutzer das Schwenken von Links und rechts zum Durchlaufen von Datenseiten. In diesem Handbuch wird erläutert, wie Sie mithilfe von Fragmenten als Datenseiten eine Benutzeroberfläche mit "viewpager" implementieren können.
ms.prod: xamarin
ms.assetid: 62B6286F-3680-48F3-B91B-453692E457E5
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/01/2018
ms.openlocfilehash: d0fdfa44ce6caa0c5f0e0aa2eed6406e606eacc4
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029038"
---
# <a name="viewpager-with-fragments"></a>ViewPager mit Fragmenten

_viewpager ist ein Layoutmanager, mit dem Sie die Gestural-Navigation implementieren können. Die Gestural-Navigation ermöglicht dem Benutzer das Schwenken von Links und rechts zum Durchlaufen von Datenseiten. In diesem Handbuch wird erläutert, wie Sie mithilfe von Fragmenten als Datenseiten eine Benutzeroberfläche mit "viewpager" implementieren können._

## <a name="overview"></a>Übersicht

`ViewPager` wird häufig in Verbindung mit Fragmenten verwendet, sodass es einfacher ist, den Lebenszyklus jeder Seite im `ViewPager`zu verwalten. In dieser exemplarischen Vorgehensweise wird `ViewPager` zum Erstellen einer App namens " **Flash cardpager** " verwendet, die eine Reihe mathematischer Probleme auf Flash Karten darstellt. Jede Flash Karte wird als Fragment implementiert. Der Benutzer blinkt nach links und rechts durch die Flash Karten und tippt auf ein mathematisches Problem, um seine Antwort anzuzeigen. Diese App erstellt eine `Fragment` Instanz für jede Flash Karte und implementiert einen Adapter, der von `FragmentPagerAdapter`abgeleitet ist. In [viewpager und Ansichten](~/android/user-interface/controls/view-pager/viewpager-and-views.md)wurden die meisten Aufgaben in `MainActivity` Lifecycle-Methoden durchgeführt. In " **flashcardpager**" wird der größte Teil der Arbeit von einem `Fragment` in einer seiner Lebenszyklus Methoden durchgeführt. 

Dieses Handbuch behandelt nicht die Grundlagen von Fragmenten &ndash; Wenn Sie noch nicht mit Fragmenten in xamarin. Android vertraut sind, finden Sie weitere Informationen unter [Fragmente](~/android/platform/fragments/index.md) , die Ihnen bei den ersten Schritten mit Fragmenten helfen. 

## <a name="start-an-app-project"></a>Starten eines App-Projekts

Erstellen Sie ein neues Android-Projekt namens " **Flash cardpager**". Starten Sie als nächstes den nuget-Paket-Manager (Weitere Informationen zum Installieren von nuget-Paketen finden Sie unter [Exemplarische Vorgehensweise: Einschließen eines nuget-Projekts in das Projekt](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)). Suchen und installieren Sie das **xamarin. Android. Support. v4** -Paket, wie unter [viewpager und Views](~/android/user-interface/controls/view-pager/viewpager-and-views.md)erläutert. 

## <a name="add-an-example-data-source"></a>Hinzufügen einer Beispiel Datenquelle

In " **Flash cardpager**" ist die Datenquelle ein Stapel von Flash Karten, die durch die `FlashCardDeck`-Klasse dargestellt werden. Diese Datenquelle stellt die `ViewPager` mit Element Inhalt bereit. `FlashCardDeck` enthält eine vorgefertigte Auflistung von mathematischen Problemen und Antworten. Der `FlashCardDeck`-Konstruktor erfordert keine Argumente: 

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

Da die Implementierungsdetails `FlashCardDeck` für das Verständnis von `ViewPager`nicht relevant sind, wird der `FlashCardDeck` Code hier nicht aufgeführt.
Der zu `FlashCardDeck` Quellcode ist unter [FlashCardDeck.cs](https://github.com/xamarin/monodroid-samples/blob/master/UserInterface/FlashCardPager/FlashCardPager/FlashCardDeck.cs)verfügbar.
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

Dieser XML-Code definiert eine `ViewPager`, die den gesamten Bildschirm einnimmt. Beachten Sie, dass Sie den voll qualifizierten Namen **Android. Support. v4. View. viewpager** verwenden müssen, da `ViewPager` in einer Unterstützungs Bibliothek verpackt ist. `ViewPager` ist nur in der [Android-Unterstützungs Bibliothek V4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)verfügbar. Er ist im Android SDK nicht verfügbar.

## <a name="set-up-viewpager"></a>Einrichten von "viewpager"

Bearbeiten Sie **MainActivity.cs** , und fügen Sie die folgenden `using`-Anweisungen hinzu:

```csharp
using Android.Support.V4.View;
using Android.Support.V4.App;
```

Ändern Sie die Deklaration der `MainActivity` Klasse so, dass Sie von `FragmentActivity`abgeleitet ist:

```csharp
public class MainActivity : FragmentActivity
```

`MainActivity` wird von`FragmentActivity` abgeleitet (anstatt `Activity`), da `FragmentActivity` weiß, wie die Unterstützung von Fragmenten verwaltet wird. Ersetzen Sie die `OnCreate`-Methode durch folgenden Code: 

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

1. Legt die Ansicht aus der **Main. axml** -layoutressource fest.

2. Ruft einen Verweis auf die `ViewPager` aus dem Layout ab.

3. Instanziiert eine neue `FlashCardDeck` als Datenquelle.

Wenn Sie diesen Code erstellen und ausführen, sollte eine Anzeige angezeigt werden, die dem folgenden Screenshot ähnelt: 

[Screenshot der App "Flash cardpager" mit leerem viewpager![](viewpager-and-fragments-images/01-initial-screen-sml.png)](viewpager-and-fragments-images/01-initial-screen.png#lightbox)

An diesem Punkt ist der `ViewPager` leer, da er nicht über die Fragmente verfügt, die zum Auffüllen des `ViewPager`verwendet werden, und es fehlt ein Adapter zum Erstellen dieser Fragmente aus den Daten in **Flash cardkarten**. 

In den folgenden Abschnitten wird eine `FlashCardFragment` erstellt, um die Funktionalität der einzelnen Flash Karten zu implementieren, und ein `FragmentPagerAdapter` wird erstellt, um die `ViewPager` mit den aus den Daten im `FlashCardDeck`erstellten Fragmenten zu verbinden. 

## <a name="create-the-fragment"></a>Erstellen des Fragments

Jede Flash Karte wird von einem UI-Fragment mit dem Namen `FlashCardFragment`verwaltet. in `FlashCardFragment`Ansicht werden die Informationen angezeigt, die in einer einzelnen Flash Karte enthalten sind. Jede Instanz von `FlashCardFragment` wird vom `ViewPager`gehostet. 
`FlashCardFragment`Ansicht besteht aus einer `TextView`, in der der Text des Flash Karten Problems angezeigt wird. In dieser Ansicht wird ein Ereignishandler implementiert, der eine `Toast` verwendet, um die Antwort anzuzeigen, wenn der Benutzer auf die Flash Karten Frage tippt. 

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

Dieses Layout definiert ein einzelnes Flash Karten Fragment. jedes Fragment besteht aus einer `TextView`, die ein mathematisches Problem mithilfe einer großen (100 SP) Schriftart anzeigt. Dieser Text wird vertikal und horizontal auf der Flash Karte zentriert. 

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

Mit diesem Code wird die wesentliche `Fragment` Definition, die zum Anzeigen einer Flash Karte verwendet wird, stuftet. Beachten Sie, dass `FlashCardFragment` von der in `Android.Support.V4.App.Fragment`definierten Version der Unterstützungs Bibliothek `Fragment` abgeleitet ist. Der Konstruktor ist leer, sodass die `newInstance` Factorymethode zum Erstellen eines neuen `FlashCardFragment` anstelle eines Konstruktors verwendet wird. 

Mit der `OnCreateView` Lifecycle-Methode wird die `TextView`erstellt und konfiguriert. Dadurch wird das Layout für die `TextView` des Fragments vergrößert und die aufgeblähte `TextView` an den Aufrufer zurückgegeben. `LayoutInflater` und `ViewGroup` werden an `OnCreateView`, damit Sie das Layout erhöhen können. Das `savedInstanceState` Bündel enthält Daten, die `OnCreateView` verwendet, um die `TextView` aus einem gespeicherten Zustand neu zu erstellen. 

Die fragmentansicht wird durch den Aufrufen von `inflater.Inflate`explizit aufgeblasen. Das `container`-Argument ist das übergeordnete Element der Ansicht, und das `false`-Flag weist den Inflater hat an, das übergeordnete Element nicht der übergeordneten Ansicht hinzuzufügen (es wird hinzugefügt, wenn `ViewPager` später in dieser exemplarischen Vorgehensweise die `GetItem` Methode des Adapters aufruft). 

### <a name="add-state-code-to-flashcardfragment"></a>Hinzufügen von Zustands Code zu "Flash Fragment"

Wie eine-Aktivität verfügt ein Fragment über eine `Bundle`, die zum Speichern und Abrufen des Zustands verwendet wird. In " **flashcardpager**" wird dieses `Bundle` verwendet, um den Frage-und Antworttext für die zugeordnete Flash Karte zu speichern. Fügen Sie in **FlashCardFragment.cs**die folgenden `Bundle` Schlüssel oben in der `FlashCardFragment`-Klassendefinition hinzu: 

```csharp
private static string FLASH_CARD_QUESTION = "card_question";
private static string FLASH_CARD_ANSWER = "card_answer";
```

Ändern Sie die `newInstance` Factory-Methode so, dass Sie ein `Bundle` Objekt erstellt und die obigen Schlüssel verwendet, um die übergebenen Frage und den Antworttext im Fragment zu speichern, nachdem es instanziiert wurde: 

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

Ändern Sie die Methode des fragmentlebens Zyklus `OnCreateView`, um diese Informationen aus dem bestandenen Bündel abzurufen und den Frage Text in den `TextBox`zu laden: 

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

`ViewPager` verwendet ein Adapter Controller Objekt, das sich zwischen dem `ViewPager` und der Datenquelle befindet (siehe Abbildung im Artikel viewpager- [Adapter](~/android/user-interface/controls/view-pager/index.md#adapter) ). Für den Zugriff auf diese Daten `ViewPager` erfordert, dass Sie einen benutzerdefinierten Adapter bereitstellen, der von `PagerAdapter`abgeleitet ist. Da in diesem Beispiel Fragmente verwendet werden, wird eine `FragmentPagerAdapter` verwendet, &ndash; `FragmentPagerAdapter` von `PagerAdapter`abgeleitet ist. 
`FragmentPagerAdapter` stellt jede Seite als `Fragment` dar, die permanent im fragmentmanager beibehalten wird, solange der Benutzer zur Seite zurückkehren kann. Wenn der Benutzer die Seiten des `ViewPager`durchläuft, extrahiert der `FragmentPagerAdapter` Informationen aus der Datenquelle und verwendet diese, um `Fragment`s zu erstellen, damit die `ViewPager` angezeigt werden. 

Wenn Sie einen `FragmentPagerAdapter`implementieren, müssen Sie Folgendes überschreiben:

- **Count** &ndash; schreibgeschützte Eigenschaft, die die Anzahl der verfügbaren Sichten (Seiten) zurückgibt.

- **GetItem** &ndash; gibt das Fragment zurück, das für die angegebene Seite angezeigt werden soll.

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

Mit diesem Code wird die wesentliche `FragmentPagerAdapter` Implementierung Stubel. In den folgenden Abschnitten wird jede dieser Methoden durch funktionierenden Code ersetzt. Der Zweck des Konstruktors besteht darin, den fragmentmanager an den Basisklassenkonstruktor des `FlashCardDeckAdapter`zu übergeben. 

### <a name="implement-the-adapter-constructor"></a>Implementieren des adapterkonstruktors

Wenn die APP den `FlashCardDeckAdapter`instanziiert, liefert Sie einen Verweis auf den fragmentmanager und eine instanziierte `FlashCardDeck`. Fügen Sie die folgende Member-Variable am Anfang der `FlashCardDeckAdapter`-Klasse in **FlashCardDeckAdapter.cs**hinzu: 

```csharp
public FlashCardDeck flashCardDeck;
```

Fügen Sie dem `FlashCardDeckAdapter`-Konstruktor die folgende Codezeile hinzu: 

```csharp
this.flashCardDeck = flashCards;
```

Diese Codezeile speichert die `FlashCardDeck` Instanz, die von der `FlashCardDeckAdapter` verwendet wird. 

### <a name="implement-count"></a>Anzahl implementieren

Die `Count` Implementierung ist relativ einfach: Sie gibt die Anzahl von Flash Karten im Flash Kartenstapel zurück. Ersetzen Sie den Code `Count` durch folgenden Code: 

```csharp
public override int Count
{
    get { return flashCardDeck.NumCards; }
}
```

Die `NumCards`-Eigenschaft von `FlashCardDeck` gibt die Anzahl der Flash Karten (Anzahl der Fragmente) im DataSet zurück. 

### <a name="implement-getitem"></a>Implementieren von GetItem

Die `GetItem`-Methode gibt das Fragment zurück, das der angegebenen Position zugeordnet ist. Wenn `GetItem` für eine Position im Flash Kartenstapel aufgerufen wird, wird ein `FlashCardFragment` zurückgegeben, das für die Anzeige des Flash Karten Problems an dieser Stelle konfiguriert ist. Ersetzen Sie die `GetItem`-Methode durch folgenden Code: 

```csharp
public override Android.Support.V4.App.Fragment GetItem(int position)
{
    return (Android.Support.V4.App.Fragment)
        FlashCardFragment.newInstance (
            flashCardDeck[position].Problem, flashCardDeck[position].Answer);
}
```

Dieser Code führt folgende Schritte aus:

1. Sucht die mathematische Problem Zeichenfolge im `FlashCardDeck` Karten für die angegebene Position. 

2. Sucht die Antwort Zeichenfolge im `FlashCardDeck` Karten für die angegebene Position. 

3. Ruft die `FlashCardFragment` Factory-Methode `newInstance`auf und übergibt das Flash Karten Problem und die Antwort Zeichenfolgen. 

4. Erstellt eine neue Flash Karte `Fragment`, die den Frage-und Antworttext für diese Position enthält, und gibt diese zurück. 

Wenn das `ViewPager` die `Fragment` bei `position`rendert, wird die `TextBox` mit der mathematischen Problem Zeichenfolge angezeigt, die sich auf `position` im Flash Kartenstapel befindet. 

## <a name="add-the-adapter-to-the-viewpager"></a>Hinzufügen des Adapters zum viewpager

Nachdem der `FlashCardDeckAdapter` implementiert wurde, ist es an der Zeit, ihn dem `ViewPager`hinzuzufügen. Fügen Sie in **MainActivity.cs**am Ende der `OnCreate`-Methode die folgende Codezeile hinzu:

```csharp
FlashCardDeckAdapter adapter =
    new FlashCardDeckAdapter(SupportFragmentManager, flashCards);
viewPager.Adapter = adapter;
```

Dieser Code instanziiert die `FlashCardDeckAdapter`und übergibt die `SupportFragmentManager` im ersten Argument. (Die `SupportFragmentManager`-Eigenschaft von fragmentactivity wird verwendet, um einen Verweis auf die `FragmentManager` &ndash; zu erhalten. Weitere Informationen zum `FragmentManager`finden Sie unter [Verwalten von Fragmenten](~/android/platform/fragments/managing-fragments.md).) 

Die Kern Implementierung ist nun fertig, &ndash; die APP zu erstellen und auszuführen.
Das erste Bild des Flash Karten Stapels sollte auf dem Bildschirm angezeigt werden, wie auf der linken Seite im nächsten Screenshot dargestellt. Wischen Sie nach links, um weitere Flash Karten anzuzeigen, und wischen Sie nach rechts, um durch den Flash Kartenstapel zu wechseln:

[![Beispiel-Screenshots der App "Flash cardpager" ohne Pager-Indikatoren](viewpager-and-fragments-images/02-example-views-sml.png)](viewpager-and-fragments-images/02-example-views.png#lightbox)

## <a name="add-a-pager-indicator"></a>Hinzufügen eines Pager-Indikators

Diese minimale `ViewPager` Implementierung zeigt die einzelnen Flash Karten im Stapel an, gibt jedoch keinen Hinweis darauf, wo sich der Benutzer im Stapel befindet. Der nächste Schritt besteht darin, eine `PagerTabStrip`hinzuzufügen. Der `PagerTabStrip` informiert den Benutzer darüber, welche Problem Nummer angezeigt wird, und stellt Navigations Kontext bereit, indem ein Hinweis der vorherigen und der nächsten Flash Karten angezeigt wird. 

Öffnen Sie **Resources/Layout/Main. axml** , und fügen Sie dem Layout eine `PagerTabStrip` hinzu:

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

[![closeup von PagerTabStrip ohne Text](viewpager-and-fragments-images/03-empty-pagetabstrip-sml.png)](viewpager-and-fragments-images/03-empty-pagetabstrip.png#lightbox)

### <a name="display-a-title"></a>Anzeigen eines Titels

Um jeder Seiten Registerkarte einen Titel hinzuzufügen, implementieren Sie die `GetPageTitleFormatted`-Methode im Adapter. `ViewPager` ruft `GetPageTitleFormatted` (sofern implementiert) zum Abrufen der Titel Zeichenfolge auf, die die Seite an der angegebenen Position beschreibt. Fügen Sie der `FlashCardDeckAdapter`-Klasse in **FlashCardDeckAdapter.cs**die folgende Methode hinzu: 

```csharp
public override Java.Lang.ICharSequence GetPageTitleFormatted(int position)
{
    return new Java.Lang.String("Problem " + (position + 1));
}
```

Mit diesem Code wird die Position im Flash Kartenstapel in eine Problem Nummer konvertiert. Die resultierende Zeichenfolge wird in ein Java-`String` konvertiert, das an die `ViewPager`zurückgegeben wird. Wenn Sie die APP mit dieser neuen Methode ausführen, zeigt jede Seite die Problem Nummer in der `PagerTabStrip`an: 

[![Screenshots von "Flash cardpager" mit der auf jeder Seite angezeigten Problem Nummer](viewpager-and-fragments-images/04-pagetabstrip-sml.png)](viewpager-and-fragments-images/04-pagetabstrip.png#lightbox)

Sie können hin und her schwenken, um die Problem Nummer im Flash Kartenstapel anzuzeigen, der am oberen Rand jeder Flash Karte angezeigt wird. 

## <a name="handle-user-input"></a>Behandeln von Benutzereingaben

" **Flash cardpager** " stellt eine Reihe von fragmentbasierten Flash Karten in einem `ViewPager`dar, verfügt aber noch nicht über eine Möglichkeit, die Antwort für jedes Problem zu verdeutlichen. In diesem Abschnitt wird dem `FlashCardFragment` ein Ereignishandler hinzugefügt, um die Antwort anzuzeigen, wenn der Benutzer auf den Text des Flash Karten Problems tippt. 

Öffnen Sie **FlashCardFragment.cs** , und fügen Sie den folgenden Code am Ende der `OnCreateView`-Methode hinzu, kurz bevor die Ansicht an den Aufrufer zurückgegeben wird: 

```csharp
questionBox.Click += delegate
{
    Toast.MakeText(Activity.ApplicationContext,
            "Answer: " + answer, ToastLength.Short).Show();
};
```

Dieser `Click` Ereignishandler zeigt die Antwort in einem Toast an, der angezeigt wird, wenn der Benutzer auf die `TextBox`tippt. Die `answer` Variable wurde zuvor initialisiert, als Zustandsinformationen aus dem Paket gelesen wurden, das an `OnCreateView`weitergegeben wurde. Erstellen und führen Sie die APP aus, und tippen Sie dann auf den Problem Text auf den einzelnen Flash Karten, um die Antwort anzuzeigen: 

[![Screenshots der APP-Abtaster von Flash cardpager, wenn ein mathematisches Problem auftritt](viewpager-and-fragments-images/05-answer-sml.png)](viewpager-and-fragments-images/05-answer.png#lightbox)

Der in dieser exemplarischen Vorgehensweise vorgestellte " **Flash cardpager** " verwendet eine `MainActivity`, die von `FragmentActivity`abgeleitet ist, aber Sie können auch `MainActivity` von `AppCompatActivity` ableiten (das auch Unterstützung für die Verwaltung von Fragmenten bietet). Ein `AppCompatActivity` Beispiel finden Sie unter [Flash cardpager](https://docs.microsoft.com/samples/xamarin/monodroid-samples/userinterface-flashcardpager) im Beispiel Katalog.

## <a name="summary"></a>Zusammenfassung

In dieser exemplarischen Vorgehensweise wurde ein schrittweises Beispiel für das Erstellen einer einfachen `ViewPager`basierten App mithilfe `Fragment`s bereitgestellt. Es wurde eine Beispiel Datenquelle mit Flash Karten Fragen und-Antworten vorgestellt, ein `ViewPager` Layout zum Anzeigen der Flash Karten und eine `FragmentPagerAdapter`-Unterklasse, die die `ViewPager` mit der Datenquelle verbindet. Um dem Benutzer die Navigation durch die Flash Karten zu erleichtern, sind Anweisungen enthalten, die erläutern, wie ein `PagerTabStrip` hinzugefügt wird, um die Problem Nummer oben auf jeder Seite anzuzeigen. Schließlich wurde der Ereignis Behandlungs Code hinzugefügt, um die Antwort anzuzeigen, wenn der Benutzer auf ein Flash Karten Problem tippt. 

## <a name="related-links"></a>Verwandte Links

- [Flashcardpager (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/userinterface-flashcardpager)
