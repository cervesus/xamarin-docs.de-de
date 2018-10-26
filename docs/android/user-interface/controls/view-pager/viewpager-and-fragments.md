---
title: ViewPager mit Fragmenten
description: ViewPager handelt es sich um ein Layout-Manager, der gestural Navigation implementiert werden kann. Gestural Navigation kann der Benutzer Wischen nach links und rechts zum schrittweisen Durchlaufen von Datenseiten. Dieses Handbuch erklärt, wie Sie eine "wischfähige" Benutzeroberfläche mit ViewPager, Verwendung von Fragmenten als Datenseiten zu implementieren.
ms.prod: xamarin
ms.assetid: 62B6286F-3680-48F3-B91B-453692E457E5
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
ms.openlocfilehash: def46f69b139ef52bb6e65a1c415b9c899e63897
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50109418"
---
# <a name="viewpager-with-fragments"></a>ViewPager mit Fragmenten

_ViewPager handelt es sich um ein Layout-Manager, der gestural Navigation implementiert werden kann. Gestural Navigation kann der Benutzer Wischen nach links und rechts zum schrittweisen Durchlaufen von Datenseiten. Dieses Handbuch erklärt, wie Sie eine "wischfähige" Benutzeroberfläche mit ViewPager, Verwendung von Fragmenten als Datenseiten zu implementieren._

 
## <a name="overview"></a>Übersicht

`ViewPager` wird häufig in Verbindung mit Fragmenten verwendet, sodass er einfacher zu verwalten des Lebenszyklus jeder Seite im ist die `ViewPager`. In dieser exemplarischen Vorgehensweise `ViewPager` dient zum Erstellen einer eine app namens **FlashCardPager** , die eine Reihe von Mathematikaufgaben auf Speicherkarten darstellt. Jede Karte Flash ist als ein Fragment implementiert. Der Benutzer mit einer wischbewegung links und rechts durch die Flash-Karten und tippt auf ein Problem Mathematik, um seine Antwort anzuzeigen. Diese app erstellt ein `Fragment` Instanz für die einzelnen Flash-Karte und implementiert, abgeleitet von ein Adapter `FragmentPagerAdapter`. In [Viewpager und Ansichten](~/android/user-interface/controls/view-pager/viewpager-and-views.md), die meiste Arbeit im erfolgt `MainActivity` Lebenszyklusmethoden. In **FlashCardPager**, die meisten Aufgaben durchgeführt werden eine `Fragment` in eine ihrer Lebenszyklusmethoden. 

Dieses Handbuch deckt sich nicht auf die Grundlagen von Fragmenten &ndash; , wenn Sie noch nicht mit Fragmenten in Xamarin.Android vertraut sind, finden Sie unter [Fragmente](~/android/platform/fragments/index.md) können Sie den ersten Schritten mit Fragmenten. 



## <a name="start-an-app-project"></a>Starten Sie ein App-Projekt

Erstellen Sie ein neues Android-Projekt namens **FlashCardPager**. Als Nächstes starten Sie den NuGet-Paket-Manager (Weitere Informationen zum Installieren von NuGet-Pakete finden Sie unter [Exemplarische Vorgehensweise: Einschließen eines NuGet in Ihrem Projekt](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)). Suchen und Installieren der **Xamarin.Android.Support.v4** Paket wie unter [Viewpager und Ansichten](~/android/user-interface/controls/view-pager/viewpager-and-views.md). 



## <a name="add-an-example-data-source"></a>Hinzufügen einer Beispiel-Datenquelle

In **FlashCardPager**, die Datenquelle ist, einen Satz Flash-Karten, dargestellt durch die `FlashCardDeck` -Klasse, die diese Datenquelle stellt Daten der `ViewPager` mit Inhalt des Elements. `FlashCardDeck` enthält eine vorgefertigte Sammlung von Mathematikaufgaben und Antworten. Die `FlashCardDeck` -Konstruktor erfordert keine Argumente: 

```csharp
FlashCardDeck flashCards = new FlashCardDeck();
```

Die Auflistung von Speicherkarten in `FlashCardDeck` ist so organisiert, dass jede Flash-Karte von einem Indexer zugegriffen werden kann. Die folgende Codezeile ruft beispielsweise das vierte Flash-Karte Problem im Stapel ab: 

```csharp
string problem = flashCardDeck[3].Problem;
```

Diese Codezeile Ruft die entsprechende Antwort auf das vorherige Problem ab:

```csharp
string answer = flashCardDeck[3].Answer;
```

Da die Implementierungsdetails der `FlashCardDeck` sind nicht relevant für ein Verständnis `ViewPager`, `FlashCardDeck` Code hier nicht aufgeführt ist.
Der Quellcode `FlashCardDeck` finden Sie unter [FlashCardDeck.cs](https://github.com/xamarin/monodroid-samples/blob/master/UserInterface/FlashCardPager/FlashCardPager/FlashCardDeck.cs).
Laden Sie diese Quelldatei (oder kopieren und fügen Sie den Code in eine neue **FlashCardDeck.cs** Datei) und fügen sie Ihrem Projekt hinzu.



## <a name="create-a-viewpager-layout"></a>Erstellen eines Layouts ViewPager

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

Dieser XML-Code definiert eine `ViewPager` , die den gesamten Bildschirm einnimmt. Beachten Sie, dass Sie den vollqualifizierten Namen verwenden, müssen **android.support.v4.view.ViewPager** da `ViewPager` wird in einer Support-Bibliothek verpackt. `ViewPager` steht nur in der [Android Support-Bibliothek v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/); es ist nicht im Android SDK verfügbar.


## <a name="set-up-viewpager"></a>ViewPager einrichten

Bearbeiten Sie **"mainactivity.cs"** und fügen Sie die folgenden `using` Anweisungen:

```csharp
using Android.Support.V4.View;
using Android.Support.V4.App;
```

Ändern der `MainActivity` -Klassendeklaration so, dass es von abgeleitet ist `AppCompatActivity`:

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

1.  Legt die Ansicht aus der **Main.axml** layoutressource.

2.  Ruft einen Verweis auf die `ViewPager` aus dem Layout.

3.  Instanziiert ein neues `FlashCardDeck` als Datenquelle.

Wenn Sie erstellen und diesen Code ausführen, sehen Sie eine Anzeige, die im folgenden Screenshot ähnelt: 

[![Screenshot der FlashCardPager-app mit leeren ViewPager](viewpager-and-fragments-images/01-initial-screen-sml.png)](viewpager-and-fragments-images/01-initial-screen.png#lightbox)

An diesem Punkt die `ViewPager` ist leer, da es die Fragmente fehlt, die verwendet werden zu füllen die `ViewPager`, und es fehlt einen Adapter zum Erstellen dieser Fragmente aus den Daten im **FlashCardDeck**. 

In den folgenden Abschnitten ein `FlashCardFragment` erstellen, um die Funktionalität jeder Karte Flash zu implementieren und eine `FragmentPagerAdapter` wird erstellt, um eine Verbindung herstellen die `ViewPager` der Fragmente, die aus Daten erstellt die `FlashCardDeck`. 



## <a name="create-the-fragment"></a>Erstellen Sie das Fragment

Jede Karte Flash wird über ein UI-Fragment namens verwaltet `FlashCardFragment`. `FlashCardFragment`die Ansicht zeigt die Informationen, die eine einzelne-Karte enthalten sind. Jede Instanz des `FlashCardFragment` von gehosteten der `ViewPager`. 
`FlashCardFragment`die Ansicht besteht aus einem `TextView` , die die Flash-Karte Problem Text anzeigt. In dieser Ansicht wird einen Ereignishandler, der verwendet implementieren eine `Toast` um die Antwort zu anzuzeigen, wenn der Benutzer die Flash-Karte Frage tippt. 



### <a name="create-the-flashcardfragment-layout"></a>Erstellen Sie das Layout FlashCardFragment

Vor dem `FlashCardFragment` kann implementiert, muss das Layout definiert werden. Dieses Layout ist ein Fragment-Container-Layout für fragmentiert. Fügen Sie ein neues Android-Layout für **Ressourcen/Layout** namens **flashcard_layout.axml**. Open **Resources/layout/flashcard_layout.axml** und Ersetzen Sie den Inhalt durch den folgenden Code: 

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

Dieses Layout definiert ein Fragment der einzelnen Flash-Karte. jedes Fragment besteht aus einem `TextView` , die mathematische Problem bei der Verwendung einer Schriftart große (100sp) anzeigt. Dieser Text ist vertikal und horizontal zentriert auf die Flash-Karte. 



### <a name="create-the-initial-flashcardfragment-class"></a>Erstellen Sie die anfängliche FlashCardFragment-Klasse

Fügen Sie eine neue Datei namens **FlashCardFragment.cs** und Ersetzen Sie den Inhalt durch den folgenden Code:

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

Dieser Code versieht die grundlegende `Fragment` Definition, die verwendet werden wird, um eine-Karte anzuzeigen. Beachten Sie, dass `FlashCardFragment` stammt aus dem Support Library-Version von `Fragment` in definierten `Android.Support.V4.App.Fragment`. Der Konstruktor leer ist, damit die `newInstance` Factorymethode dient zum Erstellen eines neuen `FlashCardFragment` anstelle eines Konstruktors. 

Die `OnCreateView` Lebenszyklus-Methode erstellt und konfiguriert die `TextView`. Es vergrößert das Layout für des Fragments des `TextView` und gibt die vergrößerte `TextView` an den Aufrufer. `LayoutInflater` und `ViewGroup` übergeben werden, um `OnCreateView` , damit es das Layout nach vergrößern kann. Die `savedInstanceState` Paket Daten enthält, `OnCreateView` verwendet zum erneuten Erstellen der `TextView` aus einem gespeicherten Zustand. 

Das Fragment der Ansicht wird explizit durch den Aufruf von vergrößert `inflater.Inflate`. Die `container` Argument ist die Ansicht des übergeordneten und der `false` Flag weist der Inflater hat, nicht die vergrößerte Ansicht hinzufügen, das die Ansicht des übergeordneten (es wird werden hinzugefügt, wenn `ViewPager` Aufruf des des Adapters `GetItem` Methode weiter unten in diesem Exemplarische Vorgehensweise). 



### <a name="add-state-code-to-flashcardfragment"></a>FlashCardFragment State Code hinzufügen

Wie eine Aktivität, ein Fragment ist ein `Bundle` , speichern und Abrufen von seinen Zustand verwendet. In **FlashCardPager**wird in diesem `Bundle` wird verwendet, um die Frage zu speichern, und beantworten Text für die zugeordnete Flash-Karte. In **FlashCardFragment.cs**, fügen Sie die folgenden `Bundle` Schlüssel und dem oberen Rand der `FlashCardFragment` Definition der Klasse: 

```csharp
private static string FLASH_CARD_QUESTION = "card_question";
private static string FLASH_CARD_ANSWER = "card_answer";
```

Ändern der `newInstance` Factorymethode, dass die It erstellt eine `Bundle` -Objekt und verwendet die oben aufgeführten Schlüssel, speichern die übergebene Frage und beantworten Text im Fragment aus, nachdem er instanziiert wird: 

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

Ändern Sie die Fragment-Lebenszyklus-Methode `OnCreateView` zum Abrufen dieser Informationen aus dem übergebenen-Paket, und laden den Fragentext in den `TextBox`: 

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

Die `answer` Variable hier nicht verwendet wird, aber sie werden später verwendet, wenn Ereignishandlercode in der Datei hinzugefügt wird. 


## <a name="create-the-adapter"></a>Erstellen des Adapters

`ViewPager` verwendet ein Adapter-Controller-Objekt, das zwischen dem `ViewPager` und der Datenquelle (finden Sie unter der Abbildung in der ViewPager [Adapter](~/android/user-interface/controls/view-pager/index.md#adapter) Artikel). Auf diese Daten `ViewPager` erfordert, dass Sie einen benutzerdefinierten Adapter, die von abgeleiteten bereitstellen `PagerAdapter`. Da es sich bei diesem Beispiel Fragmenten verwendet, verwendet es einen `FragmentPagerAdapter` &ndash; `FragmentPagerAdapter` ergibt sich aus `PagerAdapter`. 
`FragmentPagerAdapter` stellt jede Seite als eine `Fragment` , wird dauerhaft im Fragment-Manager für beibehalten, solange der Benutzer auf der Seite zurückkehren kann. Als der Benutzer Kundenkarte durch Datenseiten der `ViewPager`, `FragmentPagerAdapter` extrahiert Informationen aus der Datenquelle, und wird verwendet, um das Erstellen `Fragment`s für die `ViewPager` angezeigt. 

Bei der Implementierung einer `FragmentPagerAdapter`, müssen Sie die folgenden überschreiben:

-   **Anzahl** &ndash; schreibgeschützte Eigenschaft, die die Anzahl der verfügbaren Ansichten (Seiten) zurückgibt.

-   **GetItem** &ndash; gibt zurück, das Fragment für die angegebene Seite angezeigt werden soll.

Fügen Sie eine neue Datei namens **FlashCardDeckAdapter.cs** und Ersetzen Sie den Inhalt durch den folgenden Code:

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

Dieser Code versieht die grundlegende `FragmentPagerAdapter` Implementierung. In den folgenden Abschnitten wird jede dieser Methoden mit den funktionierenden Code ersetzt. Der Zweck des Konstruktors übergeben Sie den Fragment-Manager ist die `FlashCardDeckAdapter`der Konstruktor der Basisklasse. 



### <a name="implement-the-adapter-constructor"></a>Implementieren Sie den Adapterkonstruktor

Wenn die app instanziiert die `FlashCardDeckAdapter`, gibt er einen Verweis auf die Fragment-Manager und instanziierten `FlashCardDeck`. Fügen Sie die folgende Membervariable an den Anfang der `FlashCardDeckAdapter` -Klasse im **FlashCardDeckAdapter.cs**: 

```csharp
public FlashCardDeck flashCardDeck;
```

Fügen Sie die folgende Codezeile, die `FlashCardDeckAdapter` Konstruktor: 

```csharp
this.flashCardDeck = flashCards;
```

Diese Zeile Code speichert die `FlashCardDeck` Instanz, die `FlashCardDeckAdapter` verwenden. 



### <a name="implement-count"></a>Anzahl der implementieren

Die `Count` Implementierung ist relativ einfach: Es gibt die Anzahl der Speicherkarten, Flash-Karte vorhanden. Ersetzen Sie den Code `Count` durch folgenden Code: 

```csharp
public override int Count
{
    get { return flashCardDeck.NumCards; }
}
```


Die `NumCards` Eigenschaft `FlashCardDeck` gibt die Anzahl der Flash-Karten (Anzahl der Fragmente) in den Daten zurück. 



### <a name="implement-getitem"></a>GetItem implementieren

Die `GetItem` Methode gibt das Fragment Zusammenhang mit der angegebenen Position zurück. Wenn `GetItem` wird aufgerufen, für eine Position in der Flash-Karten bestehenden Kartenstapel, wird eine `FlashCardFragment` so konfiguriert, dass das Karte von Flash-Problem an dieser Position anzuzeigen. Ersetzen Sie die `GetItem`-Methode durch folgenden Code: 

```csharp
public override Android.Support.V4.App.Fragment GetItem(int position)
{
    return (Android.Support.V4.App.Fragment)
        FlashCardFragment.newInstance (
            flashCardDeck[position].Problem, flashCardDeck[position].Answer);
}
```

Dieser Code führt Folgendes aus:

1.  Sucht die Math-Problem-Zeichenfolge in die `FlashCardDeck` Stapel für die angegebene Position. 

2.  Sucht nach der Antwort-Zeichenfolge in die `FlashCardDeck` Stapel für die angegebene Position. 

3.  Ruft die `FlashCardFragment` Factorymethode `newInstance`, und übergeben Sie die Flash-Karte Problem und die Antwort-Zeichenfolgen. 

4.  Erstellt und gibt eine neue-Karte `Fragment` , die Fragen und Antworten Text für diese Position enthält. 

Beim der `ViewPager` rendert die `Fragment` an `position`, zeigt der `TextBox` mit der Math-Problem-Zeichenfolge in `position` Flash-Karte vorhanden. 



## <a name="add-the-adapter-to-the-viewpager"></a>Hinzufügen des Adapters zu dem ViewPager

Nachdem der `FlashCardDeckAdapter` wird implementiert, ist es Zeit, die sie zum Hinzufügen der `ViewPager`. In **"mainactivity.cs"**, fügen Sie die folgende Codezeile am Ende der `OnCreate` Methode:

```csharp
FlashCardDeckAdapter adapter =
    new FlashCardDeckAdapter(SupportFragmentManager, flashCards);
viewPager.Adapter = adapter;
```

Dieser Code instanziiert die `FlashCardDeckAdapter`, und übergeben Sie die `SupportFragmentManager` im ersten Argument. (Die `SupportFragmentManager` FragmentActivity Eigenschaft wird verwendet, um einen Verweis auf die `FragmentManager` &ndash; für Weitere Informationen zu den `FragmentManager`, finden Sie unter [Verwalten von Fragmenten](~/android/platform/fragments/managing-fragments.md).) 

Die Core-Implementierung ist jetzt vollständig &ndash; erstellen und Ausführen der app.
Daraufhin sollte das erste Bild des Kartenstapels Flash-Karte, die auf dem Bildschirm angezeigt wird, wie auf der linken Seite im nächsten Screenshot gezeigt. Streichen Sie nach links, um weitere Speicherkarten, finden Sie unter dann Wischen nach rechts durch eine Flash-Karte vorhanden:

[![Beispielscreenshots FlashCardPager App ohne Pager-Indikatoren](viewpager-and-fragments-images/02-example-views-sml.png)](viewpager-and-fragments-images/02-example-views.png#lightbox)



## <a name="add-a-pager-indicator"></a>Einen Pager-Indikator hinzufügen

Diesem minimale `ViewPager` Implementierung zeigt jede Flash-Karte des Kartenstapels, bietet aber keine visuell dargestellt, in denen der Benutzer innerhalb des Kartenstapels wird. Der nächste Schritt besteht, zum Hinzufügen einer `PagerTabStrip`. Die `PagerTabStrip` informiert den Benutzer, welches Problem Zahl wird angezeigt, und bietet Navigationskontext durch einen Hinweis in der vorherigen und nächsten Flash-Karten anzeigen. 

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

Wenn Sie erstellen und der app ausführen, sollte die leere `PagerTabStrip` am oberen Rand jedes Flash-Karte angezeigt: 

[![Nahaufnahme der PagerTabStrip ohne text](viewpager-and-fragments-images/03-empty-pagetabstrip-sml.png)](viewpager-and-fragments-images/03-empty-pagetabstrip.png#lightbox)



### <a name="display-a-title"></a>Anzeigen eines Titels

Zum Hinzufügen eines Titels zu jeder Seitenregisterkarte implementieren die `GetPageTitleFormatted` -Methode in der der Adapter. `ViewPager` Aufrufe `GetPageTitleFormatted` (wenn implementiert) auf die Title-Zeichenfolge abzurufen, die beschreibt, die Seite an der angegebenen Position. Fügen Sie die folgende Methode der `FlashCardDeckAdapter` -Klasse im **FlashCardDeckAdapter.cs**: 

```csharp
public override Java.Lang.ICharSequence GetPageTitleFormatted(int position)
{
    return new Java.Lang.String("Problem " + (position + 1));
}
```

Dieser Code konvertiert die Position im Stapel Flash-Karte in einem Problem. Die resultierende Zeichenfolge wird in einer Java konvertiert `String` zurückgegeben wird, um die `ViewPager`. Wenn Sie die app mit dieser neuen Methode ausführen, zeigt jeder Seite die Problem-Anzahl in der `PagerTabStrip`: 

[![Screenshots der FlashCardPager mit dem Problem Anzahl über jede Seite angezeigt](viewpager-and-fragments-images/04-pagetabstrip-sml.png)](viewpager-and-fragments-images/04-pagetabstrip.png#lightbox)

Sie können hin und her navigieren, um die Anzahl der Problem im Stapel Flash-Karte anzuzeigen, die am oberen Rand jedes Flash-Karte angezeigt wird. 



## <a name="handle-user-input"></a>Behandeln von Benutzereingaben

**FlashCardPager** enthält eine Reihe von Fragment-basierte Speicherkarten in einer `ViewPager`, aber noch keine Antwort für das jeweilige Problem liefern können. In diesem Abschnitt ein Ereignishandler hinzugefügt wird, um die `FlashCardFragment` um die Antwort zu anzuzeigen, wenn der Benutzer auf den Text der Flash-Karte Problem tippt. 

Open **FlashCardFragment.cs** und fügen Sie den folgenden Code am Ende der `OnCreateView` -Methode unmittelbar vor die Ansicht, die an den Aufrufer zurückgegeben wird: 

```csharp
questionBox.Click += delegate
{
    Toast.MakeText(Activity.ApplicationContext,
            "Answer: " + answer, ToastLength.Short).Show();
};
```

Dies `Click` Ereignishandler zeigt die Antwort in ein Popup, das angezeigt wird, wenn der Benutzer tippt der `TextBox`. Die `answer` Variable wurde zuvor initialisiert, wenn die Zustandsinformationen aus dem Paket gelesen wurde, der an übergebene `OnCreateView`. Erstellen Sie, führen Sie die app, und tippen Sie auf den Text Problem auf jeden Flash-Karte, um die Antwort finden Sie unter: 

[![Screenshots der FlashCardPager app Popups, die bei der Math-Problem getippt wird](viewpager-and-fragments-images/05-answer-sml.png)](viewpager-and-fragments-images/05-answer.png#lightbox)

Der **FlashCardPager** angezeigt, die in dieser exemplarischen Vorgehensweise verwendet ein `MainActivity` abgeleitet `FragmentActivity`, aber Sie können auch ableiten `MainActivity` aus `AppCompatActivity` (die bietet auch Unterstützung zum Verwalten von Fragmenten). Anzeigen einer `AppCompatActivity` Beispiel finden Sie unter [FlashCardPager](https://developer.xamarin.com/samples/monodroid/UserInterface%5CFlashCardPager/) im Katalog-Beispiel. 



## <a name="summary"></a>Zusammenfassung

In dieser exemplarischen Vorgehensweise bereitgestellt, ein ausführliches Beispiel zum Erstellen eines Grundlegendes `ViewPager`-basierten app mit `Fragment`s. Sie eine Beispiel-Datenquelle, die mit Flash-Karte Fragen und Antworten angezeigt ein `ViewPager` Layout für die Flash-Karten, anzuzeigen und eine `FragmentPagerAdapter` Unterklasse, die eine Verbindung herstellt der `ViewPager` mit der Datenquelle. Damit können Benutzer über die Flash-Karten zu navigieren, sind Anweisungen enthalten, die erläutern, dass das Hinzufügen einer `PagerTabStrip` angewiesen, das Problem am oberen Rand jeder Seite anzuzeigen. Schließlich wurde die Ereignishandlercode hinzugefügt, um die Antwort zu anzuzeigen, wenn der Benutzer auf ein Problem Flash-Karte tippt. 



## <a name="related-links"></a>Verwandte Links

- [FlashCardPager (Beispiel)](https://developer.xamarin.com/samples/monodroid/UserInterface/FlashCardPager)
