---
title: Erstellen eines Fragments
ms.prod: xamarin
ms.assetid: F2997242-BC29-1440-7F1A-CFC447CD73FA
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/07/2018
ms.openlocfilehash: 0e8d3748c7ddd337cf2f27f5b272b208e79d503a
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2020
ms.locfileid: "73027503"
---
# <a name="creating-a-fragment"></a>Erstellen eines Fragments

Zum Erstellen eines Fragments muss eine Klasse von `Android.App.Fragment` erben und dann die `OnCreateView`-Methode überschreiben. `OnCreateView` wird von der Hostingaktivität aufgerufen, wenn es an der Zeit ist, das Fragment auf dem Bildschirm anzuzeigen, und gibt ein `View`-Element zurück. Eine typische `OnCreateView` erstellt diese `View`, indem eine Layoutdatei aufgefüllt und dann an einen übergeordneten Container angefügt wird. Die Merkmale des Containers sind wichtig, da Android die Layoutparameter des übergeordneten Elements auf die Benutzeroberfläche des Fragments anwendet. Dies wird anhand des folgenden Beispiels veranschaulicht:

```csharp
public override View OnCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState)
{
    return inflater.Inflate(Resource.Layout.Example_Fragment, container, false);
}
```

Der obige Code füllt die Ansicht `Resource.Layout.Example_Fragment` auf und fügt sie dem Container `ViewGroup` als untergeordnete Ansicht hinzu.

> [!NOTE]
> Fragmentunterklassen müssen über einen öffentlichen Standardkonstruktor ohne Argument verfügen.

## <a name="adding-a-fragment-to-an-activity"></a>Hinzufügen eines Fragments zu einer Aktivität

Es gibt zwei Möglichkeiten, wie ein Fragment innerhalb einer Aktivität gehostet werden kann:

- **Deklarativ** &ndash; Fragmente können innerhalb von `.axml`-Layoutdateien mithilfe des `<Fragment>`-Tags deklarativ verwendet werden.

- **Programmgesteuert** &ndash; Fragmente können auch dynamisch mithilfe der API der `FragmentManager`-Klasse instanziiert werden.

Die programmgesteuerte Verwendung über die `FragmentManager`-Klasse wird später in diesem Leitfaden erläutert.

### <a name="using-a-fragment-declaratively"></a>Deklarative Verwendung eines Fragments

Das Hinzufügen eines Fragments innerhalb des Layouts erfordert die Verwendung des `<fragment>`-Tags und die anschließende Identifizierung des Fragments durch Bereitstellen des `class`-Attributs oder des `android:name`-Attributs. Der folgende Codeausschnitt zeigt, wie das `class`-Attribut verwendet wird, um ein `fragment` zu deklarieren:

```xml
<?xml version="1.0" encoding="utf-8"?>
<fragment class="com.xamarin.sample.fragments.TitlesFragment"
            android:id="@+id/titles_fragment"
            android:layout_width="fill_parent"
            android:layout_height="fill_parent" />
```

Der nächste Codeausschnitt veranschaulicht, wie ein `fragment` mithilfe des `android:name`-Attributs deklariert wird, um die Fragment-Klasse zu identifizieren:

```xml
<?xml version="1.0" encoding="utf-8"?>
<fragment android:name="com.xamarin.sample.fragments.TitlesFragment"
            android:id="@+id/titles_fragment"
            android:layout_width="fill_parent"
            android:layout_height="fill_parent" />
```

Wenn die Aktivität erstellt wird, instanziiert Android jedes in der Layoutdatei angegebene Fragment und fügt die Ansicht anstelle des `Fragment`-Elements ein, die aus `OnCreateView` erstellt wird.
Fragmente, die einer Aktivität deklarativ hinzugefügt werden, sind statisch und verbleiben bei der Aktivität, bis sie zerstört wird. Es ist nicht möglich, ein solches Fragment während der Lebensdauer der Aktivität, an die es angefügt ist, dynamisch zu ersetzen oder zu entfernen.

Jedem Fragment muss ein eindeutiger Bezeichner zugewiesen werden:

- **android:id** &ndash; Wie bei anderen Benutzeroberflächenelementen in einer Layoutdatei ist dies eine eindeutige ID.

- **android:tag** &ndash; Dieses Attribut ist eine eindeutige Zeichenfolge.

Wenn keine der beiden genannten Methoden verwendet wird, geht das Fragment von der ID der Containeransicht aus. Im folgenden Beispiel, in dem weder `android:id` noch `android:tag` bereitgestellt werden, weist Android dem Fragment die ID `fragment_container` zu:

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
                android:id="+@id/fragment_container"
                android:orientation="horizontal"
                android:layout_width="match_parent"
                android:layout_height="match_parent">

        <fragment class="com.example.android.apis.app.TitlesFragment"
                android:layout_width="match_parent"
                android:layout_height="match_parent" />
</LinearLayout>
```

### <a name="package-name-case"></a>Groß-/Kleinschreibung von Paketnamen

Android lässt in Paketnamen keine Großbuchstaben zu. Es wird eine Ausnahme ausgelöst, wenn versucht wird, die Ansicht aufzufüllen, wenn ein Paketname einen Großbuchstaben enthält. Xamarin.Android ist jedoch toleranter und duldet Großbuchstaben im Namespace.

Beispielsweise funktionieren die beiden folgenden Codeausschnitte mit Xamarin.Android. Der zweite Codeausschnitt bewirkt jedoch, dass eine `android.view.InflateException` von einer rein Java-basierten Android-Anwendung ausgelöst wird.

```xml
<fragment class="com.example.DetailsFragment" android:id="@+id/fragment_content" android:layout_width="match_parent" android:layout_height="match_parent" />
```

ODER

```xml
<fragment class="Com.Example.DetailsFragment" android:id="@+id/fragment_content" android:layout_width="match_parent" android:layout_height="match_parent" />
```

## <a name="fragment-lifecycle"></a>Fragmentlebenszyklus

Fragmente haben ihren eigenen Lebenszyklus, der in gewisser Weise unabhängig vom [Lebenszyklus der Hostingaktivität](~/android/app-fundamentals/activity-lifecycle/index.md) ist, aber dennoch von diesem beeinflusst wird.
Wenn z. B. eine Aktivität angehalten wird, werden alle zugehörigen Fragmente angehalten. Die folgende Abbildung zeigt den Lebenszyklus des Fragments.

[![Flussdiagramm zur Veranschaulichung des Fragmentlebenszyklus](creating-a-fragment-images/fragment-lifecycle.png)](creating-a-fragment-images/fragment-lifecycle.png#lightbox)

### <a name="fragment-creation-lifecycle-methods"></a>Lebenszyklusmethoden der Fragmenterstellung

Die nachstehende Liste zeigt den Fluss der verschiedenen Rückrufe während des Lebenszyklus eines Fragments während seiner Erstellung:

- **`OnInflate()`** &ndash; Wird aufgerufen, wenn das Fragment als Teil eines Ansichtslayouts erstellt wird. Dieser Aufruf kann unmittelbar nach dem deklarativen Erstellen des Fragments aus einer XML-Layoutdatei erfolgen. Das Fragment ist noch nicht mit seiner Aktivität verknüpft, aber **Activity**, **Bundle** und **AttributeSet** aus der Ansichtshierarchie werden als Parameter übergeben. Diese Methode wird am besten zum Analysieren von **AttributeSet** und zum Speichern der Attribute verwendet, die möglicherweise später vom Fragment verwendet werden.

- **`OnAttach()`** &ndash; Wird aufgerufen, nachdem das Fragment der Aktivität zugeordnet wurde. Dies ist die erste Methode, die ausgeführt werden soll, wenn das Fragment zur Verwendung bereit ist. Im Allgemeinen sollten Fragmente keinen Konstruktor implementieren und auch nicht den Standardkonstruktor überschreiben. Alle Komponenten, die für das Fragment erforderlich sind, sollten in dieser Methode initialisiert werden.

- **`OnCreate()`** &ndash; Wird von der-Aktivität aufgerufen, um das Fragment zu erstellen. Wenn diese Methode aufgerufen wird, wird die Ansichtshierarchie der Hostingaktivität möglicherweise nicht vollständig instanziiert, sodass sich das Fragment bis zu einem späteren Zeitpunkt im Lebenszyklus des Fragments nicht auf Teile der Ansichtshierarchie der Aktivität verlassen sollte. Verwenden Sie diese Methode beispielsweise nicht, um Optimierungen oder Anpassungen an der Benutzeroberfläche der Anwendung vorzunehmen. Dies ist der früheste Zeitpunkt, zu dem das Fragment mit dem Erfassen der benötigten Daten beginnen kann. Das Fragment wird an diesem Punkt im Benutzeroberflächenthread ausgeführt. Vermeiden Sie daher eine lange Verarbeitung, oder führen Sie diese Verarbeitung in einem Hintergrundthread aus. Diese Methode kann übersprungen werden, wenn **SetRetainInstance(true)** aufgerufen wird.
    Diese Alternative wird weiter unten ausführlicher beschrieben.

- **`OnCreateView()`** &ndash; Erstellt die Ansicht für das Fragment.
    Diese Methode wird aufgerufen, sobald die **OnCreate()** -Methode der Aktivität abgeschlossen wurde. An diesem Punkt ist es sicher, mit der Ansichtshierarchie der Aktivität zu interagieren. Diese Methode sollte die Ansicht zurückgeben, die vom Fragment verwendet wird.

- **`OnActivityCreated()`** &ndash; Wird aufgerufen, nachdem **Activity.OnCreate** von der Hostingaktivität abgeschlossen wurde.
    Zu diesem Zeitpunkt sollten abschließende Optimierungen der Benutzeroberfläche durchgeführt werden.

- **`OnStart()`** &ndash; Wird aufgerufen, nachdem die enthaltende Aktivität fortgesetzt wurde. Dadurch wird das Fragment für den Benutzer sichtbar. In vielen Fällen enthält das Fragment Code, der andernfalls in der **OnStart()** -Methode einer Aktivität enthalten wäre.

- **`OnResume()`** &ndash; Dies ist die letzte Methode, die aufgerufen wird, bevor der Benutzer mit dem Fragment interagieren kann. Ein Beispiel für die Art von Code, der in dieser Methode ausgeführt werden sollte, ist das Aktivieren der Features eines Geräts, mit denen der Benutzer interagieren kann, z. B. die Kamera, die der Standort bereitstellt. Dienste wie diese können jedoch zu einem übermäßigen Akkuverbrauch führen, und eine Anwendung sollte deren Verwendung minimieren, um zur Akkulebensdauer beizutragen.

### <a name="fragment-destruction-lifecycle-methods"></a>Lebenszyklusmethoden der Fragmentzerstörung

In der nächsten Liste werden die Lebenszyklusmethoden erläutert, die aufgerufen werden, wenn ein Fragment zerstört wird:

- **`OnPause()`** &ndash; Der Benutzer ist nicht mehr in der Lage, mit dem Fragment zu interagieren. Diese Situation entsteht, weil ein anderer Fragmentvorgang dieses Fragment ändert oder die Hostingaktivität angehalten wurde. Es ist möglich, dass die Aktivität, die dieses Fragment hostet, weiterhin sichtbar ist, d. h., die Aktivität im Fokus ist teilweise transparent oder belegt nicht den vollständigen Bildschirm. Wenn diese Methode aktiv wird, ist dies der erste Hinweis darauf, dass der Benutzer das Fragment verlässt. Das Fragment sollte alle Änderungen speichern.

- **`OnStop()`** &ndash; Das Fragment ist nicht mehr sichtbar. Die Hostaktivität wurde möglicherweise angehalten, oder ein Fragmentvorgang ändert sie in der Aktivität. Dieser Rückruf dient demselben Zweck wie **Activity.OnStop**.

- **`OnDestroyView()`** &ndash; Diese Methode wird aufgerufen, um der Ansicht zugeordnete Ressourcen zu bereinigen. Diese Methode wird aufgerufen, wenn die dem Fragment zugeordnete Ansicht zerstört wurde.

- **`OnDestroy()`** &ndash; Diese Methode wird aufgerufen, wenn das Fragment nicht mehr verwendet wird. Es ist noch mit der-Aktivität verknüpft, aber das Fragment ist nicht mehr funktionsfähig. Diese Methode sollte alle Ressourcen freigeben, die vom Fragment verwendet werden, z. B. eine [**SurfaceView**](xref:Android.Views.SurfaceView), die möglicherweise für eine Kamera verwendet wird. Diese Methode kann übersprungen werden, wenn **SetRetainInstance(true)** aufgerufen wird. Diese Alternative wird weiter unten ausführlicher beschrieben.

- **`OnDetach()`** &ndash; Diese Methode wird aufgerufen, kurz bevor das Fragment nicht mehr der Aktivität zugeordnet ist. Die Ansichtshierarchie des Fragments ist nicht mehr vorhanden, und alle vom Fragment verwendeten Ressourcen sollten zu diesem Zeitpunkt freigegeben werden.

### <a name="using-setretaininstance"></a>Verwenden von SetRetainInstance

Es ist möglich, dass ein Fragment angibt, dass es nicht vollständig zerstört werden soll, wenn die Aktivität erneut erstellt wird. Die `Fragment`-Klasse stellt für diesen Zweck die `SetRetainInstance`-Methode bereit. Wenn `true` an diese Methode weitergegeben wird, wird die gleiche Instanz des Fragments beim erneuten Starten der Aktivität verwendet. Wenn dies geschieht, werden alle Rückrufmethoden mit Ausnahme der `OnCreate`- und `OnDestroy`-Lebenszyklusrückrufe aufgerufen. Dieser Vorgang wird im oben gezeigten Lebenszyklusdiagramm (durch die grünen gepunkteten Linien) veranschaulicht.

## <a name="fragment-state-management"></a>Verwaltung des Fragmentstatus

Fragmente können ihren Zustand während des Fragmentlebenszyklus mithilfe einer Instanz eines `Bundle` speichern und wiederherstellen. Das Bundle ermöglicht einem Fragment, Daten als Schlüssel-Wert-Paare zu speichern, und eignet sich für einfache Daten, die nicht viel Arbeitsspeicher benötigen. Ein Fragment kann seinen Zustand mit einem Aufruf von `OnSaveInstanceState` speichern:

```csharp
public override void OnSaveInstanceState(Bundle outState)
{
    base.OnSaveInstanceState(outState);
    outState.PutInt("current_choice", _currentCheckPosition);
}
```

Wenn eine neue Instanz eines Fragments erstellt wird, wird der Zustand, der im `Bundle` gespeichert ist, über die Methoden `OnCreate`, `OnCreateView` und `OnActivityCreated` der neuen Instanz für die neue Instanz verfügbar.
Im folgenden Beispiel wird veranschaulicht, wie Sie den Wert `current_choice` aus dem `Bundle` abrufen:

```csharp
public override void OnActivityCreated(Bundle savedInstanceState)
{
    base.OnActivityCreated(savedInstanceState);
    if (savedInstanceState != null)
    {
        _currentCheckPosition = savedInstanceState.GetInt("current_choice", 0);
    }
}
```

Das Überschreiben von `OnSaveInstanceState` ist ein geeigneter Mechanismus zum Speichern vorübergehender Daten in einem Fragment über Ausrichtungsänderungen hinweg (z. B. des `current_choice`-Werts im Beispiel oben). Die Standardimplementierung von `OnSaveInstanceState` übernimmt jedoch das Speichern vorübergehender Daten in der Benutzeroberfläche für jede Ansicht, der eine ID zugewiesen ist. Betrachten Sie z. B. eine Anwendung, die wie folgt über ein in XML definiertes `EditText`-Element verfügt:

```xml
<EditText android:id="@+id/myText"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"/>
```

Da dem `EditText`-Steuerelement eine `id` zugewiesen ist, speichert das Fragment die Daten automatisch im Widget, wenn `OnSaveInstanceState` aufgerufen wird.

### <a name="bundle-limitations"></a>Bundleeinschränkungen

Obwohl die Verwendung von `OnSaveInstanceState` das Speichern vorübergehender Daten vereinfacht, gelten für die Verwendung dieser Methode einige Einschränkungen:

- Wenn das Fragment nicht zum Back-Stapel hinzugefügt wird, wird sein Zustand nicht wiederhergestellt, wenn der Benutzer die Schaltfläche **Zurück** drückt.

- Wenn das Bundle verwendet wird, um Daten zu speichern, werden diese Daten serialisiert. Dies kann zu Verzögerungen bei der Verarbeitung führen.

## <a name="contributing-to-the-menu"></a>Beitragen zum Menü

Fragmente können Elemente zum Menü ihrer Hostingaktivität beitragen.
Eine Aktivität verarbeitet Menüelemente zuerst. Wenn die Aktivität über keinen Handler verfügt, wird das Ereignis an das Fragment weitergegeben, das es dann verarbeitet.

Um dem Menü der Aktivität Elemente hinzuzufügen, muss ein Fragment zwei Aktionen ausführen.
Zuerst muss das Fragment die Methode `OnCreateOptionsMenu` implementieren und seine Elemente im Menü platzieren, wie im folgenden Code gezeigt:

```csharp
public override void OnCreateOptionsMenu(IMenu menu, MenuInflater menuInflater)
{
    menuInflater.Inflate(Resource.Menu.menu_fragment_vehicle_list, menu);
    base.OnCreateOptionsMenu(menu, menuInflater);
}
```

Das Menü im vorherigen Codeausschnitt wird über den folgenden XML-Code aufgefüllt, der sich in der Datei `menu_fragment_vehicle_list.xml` befindet:

```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
  <item android:id="@+id/add_vehicle"
        android:icon="@drawable/ic_menu_add_data"
        android:title="@string/add_vehicle" />
</menu>
```

Nun muss das Fragment `SetHasOptionsMenu(true)` aufrufen. Der Aufruf dieser Methode kündigt Android an, dass das Fragment über Menüelemente verfügt, die zum Optionsmenü beitragen sollen. Wenn diese Methode nicht aufgerufen wird, werden die Menüelemente für das Fragment nicht zum Optionsmenü der Aktivität hinzugefügt. Dies erfolgt in der Regel in der Lebenszyklusmethode `OnCreate()`, wie im folgenden Codeausschnitt gezeigt:

```csharp
public override void OnCreate(Bundle savedState)
{
    base.OnCreate(savedState);
    SetHasOptionsMenu(true);
}
```

Der folgende Bildschirm zeigt, wie dieses Menü aussehen würde:

[![Beispielscreenshot der App „My Trips“, die Menüelemente anzeigt](creating-a-fragment-images/fragment-menu-example.png)](creating-a-fragment-images/fragment-menu-example.png#lightbox)
