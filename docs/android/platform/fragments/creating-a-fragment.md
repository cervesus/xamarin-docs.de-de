---
title: Erstellen ein Fragment
ms.topic: article
ms.prod: xamarin
ms.assetid: F2997242-BC29-1440-7F1A-CFC447CD73FA
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/07/2018
ms.openlocfilehash: c1dd3495b0d7f76197126094cfd10e50d0ca760d
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="creating-a-fragment"></a>Erstellen ein Fragment

Um ein Fragment zu erstellen, muss von eine Klasse erben `Android.App.Fragment` und überschreiben Sie dann die `OnCreateView` Methode. `OnCreateView` wird von der hosting-Aktivität aufgerufen werden, wenn es Zeit ist, das Fragment auf dem Bildschirm eingefügt werden soll, und zurück gibt, eine `View`. Eine typische `OnCreateView` wird, erstellen Sie diese `View` von überhöhte eine Layoutdatei, und klicken Sie dann an einem übergeordneten Container anzufügen. Der Container Merkmale sind wichtig, wie Android, dem die layoutparameter des übergeordneten Elements auf die Benutzeroberfläche des Fragments angewendet wird. Dies wird anhand des folgenden Beispiels veranschaulicht:

```csharp
public override View OnCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState)
{
    return inflater.Inflate(Resource.Layout.Example_Fragment, container, false);
}
```

Der obige Code wird die Ansicht vergrößern `Resource.Layout.Example_Fragment`, und fügen Sie es als eine untergeordnete Ansicht zu den `ViewGroup` Container.


> [!NOTE]
> **Hinweis:** Fragment Unterklassen müssen einen öffentlichen Standardwert kein Argumentkonstruktor aufweisen.

## <a name="adding-a-fragment-to-an-activity"></a>Hinzufügen eines Fragments zu einer Aktivität

Es gibt zwei Möglichkeiten, dass ein Fragment gehostet werden dürfen innerhalb einer Aktivität:

-   **Deklarativ** &ndash; Fragmente können verwendet werden, deklarativ in `.axml` Layout-Dateien mithilfe der `<Fragment>` Tag.

-   **Programmgesteuert** &ndash; Fragmenten können auch dynamisch mithilfe instanziiert werden die `FragmentManager` API-Klasse.

Programmgesteuerte Nutzung über die `FragmentManager` Klasse wird weiter unten in diesem Handbuch besprochen werden.

### <a name="using-a-fragment-declaratively"></a>Verwenden deklarative ein Fragment

Hinzufügen eines Fragments in das Layout ist erforderlich, die mit der `<fragment>` Tag und legt anschließend das Fragment durch Bereitstellen von entweder der `class` Attribut oder die `android:name` Attribut. Der folgende Codeausschnitt zeigt, wie die `class` Attribut deklariert eine `fragment`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<fragment class="com.xamarin.sample.fragments.TitlesFragment"
            android:id="@+id/titles_fragment"
            android:layout_width="fill_parent"
            android:layout_height="fill_parent" />
```

Diese weiter Codeausschnitt zeigt, wie zum Deklarieren einer `fragment` mithilfe der `android:name` Attribut zum Identifizieren der Fragment-Klasse:

```xml
<?xml version="1.0" encoding="utf-8"?>
<fragment android:name="com.xamarin.sample.fragments.TitlesFragment"
            android:id="@+id/titles_fragment"
            android:layout_width="fill_parent"
            android:layout_height="fill_parent" />
```

Beim Erstellen der Aktivität Android instanziieren jedes Fragment in der Layoutdatei angegeben und fügen Sie die Ansicht, die aus erstellt wird wird `OnCreateView` anstelle von der `Fragment` Element.
Fragmente, die eine Aktivität deklarativ hinzugefügt werden, sind statisch und für die Aktivität verbleiben, bis es zerstört wird; Es ist nicht möglich, dynamisch ersetzen oder entfernen Sie solche ein Fragment während der Lebensdauer der Aktivität, die an das es angefügt ist.

Jedes Fragment muss einen eindeutigen Bezeichner zugewiesen werden:

-  **Android: Id** &ndash; wie mit anderen Benutzeroberflächenelementen in einer Layoutdatei, eine eindeutige ID. sieht

-  **Android: Tag** &ndash; dieses Attribut ist eine eindeutige Zeichenfolge.

Wenn keines der beiden vorherigen Methoden verwendet wird, wird das Fragment die ID der Containeransicht ausgegangen. Im folgenden Beispiel, wobei weder `android:id` noch `android:tag` angegeben wird, weisen die ID Android `fragment_container` dem Fragment:

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

### <a name="package-name-case"></a>Paket Groß-/Kleinschreibung

Android ist Großbuchstaben in Paketnamen nicht zulässig; Es wird eine Ausnahme ausgelöst, bei dem Versuch, die Sicht vergrößert werden soll, wenn ein Paketname einen Großbuchstaben enthält. Allerdings Xamarin.Android wird eine allgemeinere und wird im Namespace Großbuchstaben tolerieren.

Beispielsweise werden sowohl von den folgenden Codeausschnitten mit Xamarin.Android funktionieren. Der zweite Ausschnitt führt allerdings eine `android.view.InflateException` von einer reinen Java-basierte Android-Anwendung ausgelöst werden.

```xml
<fragment class="com.example.DetailsFragment" android:id="@+id/fragment_content" android:layout_width="match_parent" android:layout_height="match_parent" />
```

ODER

```xml
<fragment class="Com.Example.DetailsFragment" android:id="@+id/fragment_content" android:layout_width="match_parent" android:layout_height="match_parent" />
```


## <a name="fragment-lifecycle"></a>Fragment-Lebenszyklus

Fragmente haben ihre eigenen Lebenszyklus, der etwas unabhängig von, aber dennoch von der, betroffen ist die [Lebenszyklus der hosting-Aktivität](~/android/app-fundamentals/activity-lifecycle/index.md).
Z. B. wenn eine Aktivität angehalten wird, sind alle seine zugeordneten Fragmente angehalten. Das folgende Diagramm beschreibt den Lebenszyklus des Fragments.

[![Flussdiagramm zur Veranschaulichung des Fragment-Lebenszyklus](creating-a-fragment-images/fragment-lifecycle.png)](creating-a-fragment-images/fragment-lifecycle.png)


### <a name="fragment-creation-lifecycle-methods"></a>Fragment Erstellung Lebenszyklusmethoden

Die Liste unten veranschaulicht den Ablauf von verschiedenen Rückrufe im Lebenszyklus eines Fragments, wie es erstellt wird:

-   **`OnInflate()`** &ndash; Wird aufgerufen, wenn das Fragment als Teil einer Ansichtslayout erstellt wird. Dies kann aufgerufen werden, sofort, nachdem das Fragment aus einer XML-Datendatei Layout deklarativ erstellt wird. Das Fragment ist noch nicht ihre Aktivität zugeordnet, aber die **Aktivität**, **Bundle**, und **AttributeSet** aus der Sicht Hierarchie als Parameter übergeben werden. Diese Methode wird am besten zum Analysieren der **AttributeSet** und speichern die Attribute, die durch das Fragment später verwendet werden kann.

-   **`OnAttach()`** &ndash; Wird aufgerufen, nachdem das Fragment der Aktivität zugeordnet ist. Dies ist die erste Methode, die ausgeführt werden, sobald das Fragment verwendet werden kann. Fragmente sollte im Allgemeinen keinen Konstruktor implementieren oder Überschreiben des Standardkonstruktors. Alle Komponenten, die für das Fragment erforderlich sind, die in dieser Methode initialisiert werden sollte.

-   **`OnCreate()`** &ndash; Wird aufgerufen, von der Aktivität, um das Fragment zu erstellen. Wenn diese Methode aufgerufen wird, kann die Hierarchie Anzeigen der hosting-Aktivität nicht vollständig instanziiert werden, weshalb das Fragment nicht für alle Teile der Aktivität anzeigen Hierarchie erst später im Lebenszyklus des Fragments sollten. Verwenden Sie diese Methode z. B. keine kleiner Anpassungen oder Anpassungen an der Benutzeroberfläche der Anwendung ausführen. Dies ist die früheste Uhrzeit, an dem das Fragment beginnen möglicherweise Erfassen der Daten, die benötigt, werden. Das Fragment an diesem Punkt im UI-Thread ausgeführt wird, so vermeiden Sie langwierige Verarbeitung oder die Verarbeitung in einem Hintergrundthread ausführen. Diese Methode kann übersprungen werden, wenn **SetRetainInstance(true)** aufgerufen wird.
    Diese Alternative werden im folgenden ausführlicher beschrieben.

-   **`OnCreateView()`** &ndash; Erstellt die Ansicht für das Fragment.
    Diese Methode wird einmal der Aktivitätssymbols aufgerufen **OnCreate()** Methode abgeschlossen ist. An diesem Punkt ist es sicher für die Interaktion mit der Hierarchie Anzeigen der Aktivität. Diese Methode sollte die Sicht zurück, die durch das Fragment verwendet werden.

-   **`OnActivityCreated()`** &ndash; Wird aufgerufen, nachdem **"Activity.OnCreate"** hosting Aktivität abgeschlossen wurde.
    Zu diesem Zeitpunkt sollten endgültige kleiner Anpassungen an der Benutzeroberfläche ausgeführt werden.

-   **`OnStart()`** &ndash; Wird aufgerufen, nachdem die enthaltende Aktivität fortgesetzt wurde. Dadurch wird das Fragment für den Benutzer sichtbar. In vielen Fällen wird das Fragment enthält Code, die sonst in der **OnStart()** Methode einer Aktivität.

-   **`OnResume()`** &ndash; Dies ist die letzte Methode wird aufgerufen, bevor der Benutzer das Fragment interagieren kann. Ein Beispiel für die Art des Codes, die in dieser Methode ausgeführt werden sollten würde aktivieren, wenn Sie Funktionen von Geräten, bei denen der Benutzer, z. B. die Kamera interagieren kann, die den Standortdiensten. Dienste wie diese können dazu führen, dass eine übermäßige Akku ausgleichen, und eine Anwendung sollten minimieren, verwenden Sie diese Akkulaufzeit beibehalten.


### <a name="fragment-destruction-lifecycle-methods"></a>Fragment Zerstörung Lebenszyklusmethoden

Die folgenden Liste wird Lebenszyklusmethoden, die aufgerufen werden, wie ein Fragment zerstört wird Folgendes erläutert:

-   **`OnPause()`** &ndash; Der Benutzer ist nicht mehr mit dem Fragment kommunizieren können. Dieses Problem besteht, weil dieses Fragment ein anderen Fragment-Vorgangs ändert oder hosting Aktivität angehalten wurde. Es ist möglich, dass die Aktivität, die dieses Fragment hosting möglicherweise immer noch sichtbar ist, bedeutet, dass die Aktivität im Fokus ist teilweise transparent oder beansprucht keinen den gesamten Bildschirm. Wenn diese Methode aktiviert wird, ist es das erste Anzeichen, dass der Benutzer das Fragment verlässt. Das Fragment sollten Änderungen zu speichern.

-   **`OnStop()`** &ndash; Das Fragment wird nicht mehr angezeigt. Der Host Aktivität möglicherweise im beendeten Zustand oder ein Fragmentvorgang ist in der Aktivität ändern. Dieser Rückruf dient, den gleichen Zweck wie **Activity.OnStop**.

-   **`OnDestroyView()`** &ndash; Diese Methode wird aufgerufen, um mit der Ansicht verknüpfte Ressourcen zu bereinigen. Wird aufgerufen, wenn die Sicht zugeordneten zerstört wurde.

-   **`OnDestroy()`** &ndash; Diese Methode wird aufgerufen, wenn das Fragment nicht mehr verwendet wird. Mit der Aktivität weiterhin verknüpft ist, aber das Fragment ist nicht mehr funktionsfähig. Diese Methode sollten alle Ressourcen frei, die vom Fragment, z. B. werden eine [ **SurfaceView** ](https://developer.xamarin.com/api/type/Android.Views.SurfaceView/) für eine Kamera verwendet werden kann. Diese Methode kann übersprungen werden, wenn **SetRetainInstance(true)** aufgerufen wird. Diese Alternative werden im folgenden ausführlicher beschrieben.

-   **`OnDetach()`** &ndash; Diese Methode wird aufgerufen, kurz bevor das Fragment nicht mehr mit der Aktivität verknüpft ist. Die Hierarchie anzeigen des Fragments nicht mehr vorhanden ist, und alle Ressourcen, die vom Fragment verwendet werden zu diesem Zeitpunkt freigegeben werden soll.


### <a name="using-setretaininstance"></a>Verwenden von SetRetainInstance

Es ist möglich, dass ein Fragment, um anzugeben, dass es nicht vollständig zerstört werden soll, wenn die Aktivität neu erstellt wird. Die `Fragment` -Klasse stellt die Methode `SetRetainInstance` für diesen Zweck. Wenn `true` an diese Methode übergeben wird, wenn die Aktivität neu gestartet wird, wird die gleiche Instanz des Fragments verwendet. Wenn dies geschieht, alle Rückrufmethoden werden aufgerufen, mit Ausnahme der `OnCreate` und `OnDestroy` Lebenszyklus Rückrufe. Dieser Prozess wird im Lebenszyklusdiagramm oben (durch die gepunkteten Linien mit Grün) veranschaulicht.


## <a name="fragment-state-management"></a>Fragment Zustandsverwaltung

Fragmente Mai speichern und Wiederherstellen von ihren Status während des Lebenszyklus Fragment mit einer Instanz von einem `Bundle`. Das Paket kann ein Fragment zum Speichern von Daten als Schlüssel/Wert-Paare und eignet sich für einfache Datentypen, die viel Arbeitsspeicher erfordern, nicht. Ein Fragment kann seinen Zustand speichern, mit einem Aufruf von `OnSaveInstanceState`:

```csharp
public override void OnSaveInstanceState(Bundle outState)
{
    base.OnSaveInstanceState(outState);
    outState.PutInt("current_choice", _currentCheckPosition);
}
```

Wenn eine neue Instanz eines Fragments erstellt wird, wird den Status gespeichert wird, der `Bundle` zur Verfügung gestellt, mit der neuen Instanz über die `OnCreate`, `OnCreateView`, und `OnActivityCreated` Methoden der neuen Instanz.
Im folgende Beispiel wird veranschaulicht, wie zum Abrufen des Werts `current_choice` aus der `Bundle`:

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

Überschreiben von `OnSaveInstanceState` ist ein geeigneter Mechanismus zum Speichern vorübergehender Daten in einem Fragment über Ausrichtung ändert, z. B. die `current_choice` Wert im obigen Beispiel. Allerdings die standardmäßige Implementierung des `OnSaveInstanceState` übernimmt speichern vorübergehende Daten in der Benutzeroberfläche für jede Ansicht mit der ID zugewiesen. Betrachten Sie beispielsweise eine Anwendung mit einem `EditText` Element im XML-Code wie folgt definiert:

```xml
<EditText android:id="@+id/myText"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"/>
```

Seit der `EditText` Steuerelement verfügt über eine `id` zugewiesen, das Fragment automatisch speichert die Daten im Widget "" Wenn `OnSaveInstanceState` aufgerufen wird.


### <a name="bundle-limitations"></a>Bundle-Einschränkungen

Obwohl durch die Verwendung `OnSaveInstanceState` macht es einfach speichern vorübergehender Daten dieser Methode verwenden, gelten einige Einschränkungen:

-  Wenn das Fragment nicht das Back-Stapel hinzugefügt wird, dann seinen Status nicht wiederhergestellt werden, wird Wenn der Benutzer drückt die **wieder** Schaltfläche.

-  Wenn das Paket zum Speichern von Daten verwendet wird, werden diese Daten serialisiert. Dies kann zu Verzögerungen bei der Verarbeitung führen.


## <a name="contributing-to-the-menu"></a>Klicken Sie im Menü verwendet werden sollen

Fragmente können Elemente für das Menü, deren hosting Aktivität beitragen.
Eine Aktivität wird Menüelemente zuerst verarbeitet. Wenn die Aktivität nicht über einen Handler verfügt, wird dann das Ereignis dem Fragment weitergegeben werden, die dann behandelt.

Um die Aktivität im Menü Elemente hinzugefügt haben, muss ein Fragment zwei Dinge tun.
Das Fragment muss zunächst die Methode implementieren `OnCreateOptionsMenu` und enthaltenen Elemente in der Menüleiste platziert werden, wie im folgenden Code gezeigt:

```csharp
public override void OnCreateOptionsMenu(IMenu menu, MenuInflater menuInflater)
{
    menuInflater.Inflate(Resource.Menu.menu_fragment_vehicle_list, menu);
    base.OnCreateOptionsMenu(menu, menuInflater);
}
```

Klicken Sie im Menü im vorherigen Codeausschnitt ist aus den folgenden XML-Code befindet sich in der Datei steigt `menu_fragment_vehicle_list.xml`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
  <item android:id="@+id/add_vehicle"
        android:icon="@drawable/ic_menu_add_data"
        android:title="@string/add_vehicle" />
</menu>
```

Das Fragment muss rufen Sie als Nächstes `SetHasOptionsMenu(true)`. Der Aufruf dieser Methode kündigt, Android, dass das Fragment Menüelemente im Listenfeld mitwirken verfügt. Es sei denn, die an diese Methode aufgerufen wird, werden die Menüelemente für das Fragment im für die Aktivität die Option nicht hinzugefügt. Dies erfolgt in der Regel in der Anwendungslebenszyklus-Methode `OnCreate()`, wie im nächsten Codeausschnitt gezeigt:

```csharp
public override void OnCreate(Bundle savedState)
{
    base.OnCreate(savedState);
    SetHasOptionsMenu(true);
}
```

Der folgende Bildschirm zeigt, wie in diesem Menü aussehen würde:

[![Beispiel-Screenshot Meine Reisen App anzeigen von Menüelementen](creating-a-fragment-images/fragment-menu-example.png)](creating-a-fragment-images/fragment-menu-example.png)
