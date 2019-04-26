---
title: Erstellen eines Fragments
ms.prod: xamarin
ms.assetid: F2997242-BC29-1440-7F1A-CFC447CD73FA
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/07/2018
ms.openlocfilehash: 339de4930242e35c40b034af2ce6ba47fe1543af
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61023672"
---
# <a name="creating-a-fragment"></a>Erstellen eines Fragments

Um ein Fragment zu erstellen, muss von eine Klasse erben `Android.App.Fragment` und überschreiben Sie dann die `OnCreateView` Methode. `OnCreateView` wird von der hosting-Aktivität aufgerufen, wenn dabei handelt es sich Zeit, mit dem Fragment auf dem Bildschirm, ergibt eine `View`. Eine typische `OnCreateView` erstellt diese `View` jedoch eine Layoutdatei, und klicken Sie dann an einem übergeordneten Container anzufügen. Des Containers Merkmale sind wichtig, da Android die layoutparameter, der das übergeordnete Element der Benutzeroberfläche des Fragments angewendet wird. Dies wird anhand des folgenden Beispiels veranschaulicht:

```csharp
public override View OnCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState)
{
    return inflater.Inflate(Resource.Layout.Example_Fragment, container, false);
}
```

Der obige Code wird die Ansicht Vergrößerung `Resource.Layout.Example_Fragment`, und fügen Sie es als eine untergeordnete Ansicht, um die `ViewGroup` Container.


> [!NOTE]
> Fragment-Unterklassen müssen eine öffentliche Standard keine Argumentkonstruktor verfügen.

## <a name="adding-a-fragment-to-an-activity"></a>Hinzufügen eines Fragments zu einer Aktivität

Es gibt zwei Möglichkeiten, dass ein Fragment gehostet werden darf innerhalb einer Aktivität:

-   **Deklarativ** &ndash; Fragmente können verwendet werden, deklarativ in `.axml` Layout-Dateien mithilfe der `<Fragment>` Tag.

-   **Programmgesteuertes** &ndash; Fragmenten können auch dynamisch mithilfe von instanziiert werden die `FragmentManager` -Klasse-API.

Programmgesteuerte Verwendung über die `FragmentManager` Klasse wird später in diesem Handbuch erläutert werden.

### <a name="using-a-fragment-declaratively"></a>Verwenden ein Fragment deklarativ

Hinzufügen eines Fragments in das Layout erfordert die Verwendung der `<fragment>` Tag, und klicken Sie dann das Fragment identifizieren, durch die Bereitstellung entweder die `class` Attribut oder das `android:name` Attribut. Der folgende Codeausschnitt zeigt, wie Sie mit der `class` Attribut deklariert eine `fragment`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<fragment class="com.xamarin.sample.fragments.TitlesFragment"
            android:id="@+id/titles_fragment"
            android:layout_width="fill_parent"
            android:layout_height="fill_parent" />
```

Dieser nächste Codeausschnitt veranschaulicht, wie Sie deklarieren eine `fragment` mithilfe der `android:name` Attributs auf die Fragment-Klasse zu identifizieren:

```xml
<?xml version="1.0" encoding="utf-8"?>
<fragment android:name="com.xamarin.sample.fragments.TitlesFragment"
            android:id="@+id/titles_fragment"
            android:layout_width="fill_parent"
            android:layout_height="fill_parent" />
```

Wenn die Aktivität erstellt wird, Android jedes Fragment in der Layoutdatei angegeben instanziieren und fügen Sie die Ansicht, die aus erstellt wird wird `OnCreateView` anstelle von der `Fragment` Element.
Fragmente, die deklarativ auf eine Aktivität hinzugefügt werden, sind statisch und bleibt für die Aktivität, bis es zerstört wird; Es ist nicht möglich, dynamisch ersetzen oder entfernen Sie solche ein Fragment, während der Lebensdauer der Aktivität, dem er zugeordnet ist.

Jedes Fragment muss einen eindeutigen Bezeichner zugewiesen werden:

-  **Android: Id** &ndash; wie bei anderen UI-Elemente in einer Layoutdatei dieser eine eindeutige ID.

-  **Android: Tag** &ndash; dieses Attribut ist eine eindeutige Zeichenfolge.

Wenn keine der beiden oben genannten Methoden verwendet wird, wird das Fragment die ID des der Containeransicht ausgegangen. Im folgenden Beispiel, weder `android:id` noch `android:tag` angegeben wird, weisen die ID Android `fragment_container` dem Fragment:

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

### <a name="package-name-case"></a>Groß-/Kleinschreibung von Paket

Android ermöglicht keine Großbuchstaben in Paketnamen; Es wird eine Ausnahme ausgelöst, wenn Sie versuchen, um die Ansicht vergrößert werden soll, wenn Sie ein Paketnamen mit einen Großbuchstaben enthält. Xamarin.Android ist jedoch mehr Fehler toleriert, und Großbuchstaben in den Namespace tolerieren.

Beispielsweise funktionieren beide die folgenden Codeausschnitte, die mit Xamarin.Android. Der zweite Codeausschnitt bewirkt jedoch eine `android.view.InflateException` von einer reinen Java-basierte Android-Anwendung ausgelöst wird.

```xml
<fragment class="com.example.DetailsFragment" android:id="@+id/fragment_content" android:layout_width="match_parent" android:layout_height="match_parent" />
```

ODER

```xml
<fragment class="Com.Example.DetailsFragment" android:id="@+id/fragment_content" android:layout_width="match_parent" android:layout_height="match_parent" />
```


## <a name="fragment-lifecycle"></a>Fragment-Lebenszyklus

Fragmente haben ihre eigenen Lebenszyklus, der ziemlich unabhängig von, aber immer noch von, betroffen ist die [Lebenszyklus der hostenden Aktivität](~/android/app-fundamentals/activity-lifecycle/index.md).
Z. B. wenn eine Aktivität angehalten wird, werden alle seine zugeordneten Fragmente angehalten. Das folgende Diagramm zeigt den Lebenszyklus des Fragments.

[![Flussdiagramm zur Veranschaulichung des Fragment-Lebenszyklus](creating-a-fragment-images/fragment-lifecycle.png)](creating-a-fragment-images/fragment-lifecycle.png#lightbox)


### <a name="fragment-creation-lifecycle-methods"></a>Fragment-Erstellung Lebenszyklusmethoden

Die nachstehende Liste zeigt den Fluss der verschiedenen Rückrufe im Lebenszyklus eines Fragments, während es erstellt wird:

-   **`OnInflate()`** &ndash; Wird aufgerufen, wenn das Fragment im Rahmen eines Ansichtslayouts erstellt wird. Dies kann aufgerufen werden, sobald das Fragment deklarativ aus einer XML-Layout-Datei erstellt wird. Das Fragment ist nicht mit ihrer Aktivität verbunden noch, aber die **Aktivität**, **Bundle**, und **AttributeSet** aus der Sicht Hierarchie werden als Parameter übergeben. Diese Methode wird am besten zum Analysieren der **AttributeSet** und geben Sie für das Speichern der Attribute, die das Fragment später verwendet werden kann.

-   **`OnAttach()`** &ndash; Wird aufgerufen, nachdem das Fragment der Aktivität zugeordnet ist. Dies ist die erste Methode ausgeführt werden, wenn das Fragment bereit ist, verwendet werden. Fragmente sollte im Allgemeinen keinen Konstruktor implementieren oder überschreiben Sie den Standardkonstruktor. Alle Komponenten, die für das Fragment erforderlich sind, sollte diese Methode initialisiert werden.

-   **`OnCreate()`** &ndash; Wird aufgerufen, von der Aktivität, um das Fragment zu erstellen. Wenn diese Methode aufgerufen wird, kann die Hierarchie von Inhaltsansichten der hosting-Aktivität nicht vollständig instanziiert werden, damit das Fragment für alle Teile der Aktivitäts Hierarchie von Inhaltsansichten erst später in das Fragment des Lebenszyklus nicht verlassen sollten. Verwenden Sie diese Methode beispielsweise nicht, um alle Anpassungen oder Anpassungen an der Benutzeroberfläche der Anwendung auszuführen. Dies ist die früheste Uhrzeit, an dem das Fragment beginnen kann, Erfassen der Daten, die er benötigt. Das Fragment an diesem Punkt im UI-Thread ausgeführt wird, also eine langwierige Verarbeitung zu vermeiden oder die Verarbeitung in einem Hintergrundthread ausführen. Diese Methode wird möglicherweise übersprungen, wenn **SetRetainInstance(true)** aufgerufen wird.
    Diese Alternative werden im folgenden ausführlicher beschrieben.

-   **`OnCreateView()`** &ndash; Erstellt die Ansicht für das Fragment.
    Diese Methode wird aufgerufen, nach der Aktivitäts **OnCreate()** Methode abgeschlossen ist. An diesem Punkt ist es sicher ist, für die Interaktion mit der Hierarchie von Inhaltsansichten der Aktivität. Diese Methode sollte die Ansicht zurück, die das Fragment verwendet werden.

-   **`OnActivityCreated()`** &ndash; Wird aufgerufen, nachdem **"Activity.OnCreate"** von der hosting-Aktivität abgeschlossen wurde.
    Endgültige Optimierungen an der Benutzeroberfläche sollten zu diesem Zeitpunkt ausgeführt werden.

-   **`OnStart()`** &ndash; Wird aufgerufen, nachdem die enthaltende Aktivität fortgesetzt wurde. Dadurch wird das Fragment für den Benutzer sichtbar. In vielen Fällen wird das Fragment enthalten Code, der andernfalls wäre die **OnStart()** Methode einer Aktivität.

-   **`OnResume()`** &ndash; Dies ist die letzte Methode wird aufgerufen, bevor der Benutzer das Fragment interagieren kann. Ein Beispiel für die Art von Code, der in dieser Methode ausgeführt werden soll würde aktivieren, wenn Sie Funktionen von Geräten, bei denen der Benutzer, z. B. die Kamera interagieren kann, die den Standortdiensten. Dienste, wie diese können dazu führen, dass eine übermäßige akkuentleerung, jedoch und eine Anwendung sollte Verwendungsbeispielen Akkulaufzeit beibehalten minimieren.


### <a name="fragment-destruction-lifecycle-methods"></a>Fragment Zerstörung Lebenszyklusmethoden

Die folgenden Liste wird erläutert, die Methoden, die aufgerufen werden, wie ein Fragment zerstört wird:

-   **`OnPause()`** &ndash; Der Benutzer ist nicht mehr mit dem Fragment interagieren. Diese Situation ist vorhanden, da dieses Fragment ein anderen Fragmentvorgang ändert oder die hosting-Aktivität wurde angehalten. Es ist möglich, dass die Aktivität, die dieses Fragment hosten möglicherweise immer noch sichtbar sein, d. h. die Aktivität im Fokus ist teilweise transparent oder nicht den gesamten Bildschirm einnehmen. Wenn diese Methode aktiv wird, ist es das erste Anzeichen, dass der Benutzer das Fragment verlässt. Das Fragment sollen die Änderungen zu speichern.

-   **`OnStop()`** &ndash; Das Fragment wird nicht mehr angezeigt. Der Host-Aktivität beendet werden kann, oder ein Fragmentvorgang ändert sie in der Aktivität. Dieser Rückruf dient demselben Zweck wie **Activity.OnStop**.

-   **`OnDestroyView()`** &ndash; Diese Methode wird aufgerufen, um mit der Ansicht verknüpfte Ressourcen zu bereinigen. Wird aufgerufen, wenn die Sicht zugeordneten zerstört wurde.

-   **`OnDestroy()`** &ndash; Diese Methode wird aufgerufen, wenn das Fragment nicht mehr verwendet wird. Es ist immer noch von der Aktivität zugeordnet, aber das Fragment ist nicht mehr funktionsfähig. Diese Methode sollte alle Ressourcen, die von dem Fragment, z. B. werden Freigeben einer [ **SurfaceView** ](https://developer.xamarin.com/api/type/Android.Views.SurfaceView/) , die für eine Kamera verwendet werden kann. Diese Methode wird möglicherweise übersprungen, wenn **SetRetainInstance(true)** aufgerufen wird. Diese Alternative werden im folgenden ausführlicher beschrieben.

-   **`OnDetach()`** &ndash; Diese Methode wird aufgerufen, kurz bevor das Fragment nicht mehr der Aktivität zugeordnet ist. Die Hierarchie von Inhaltsansichten des Fragments wird nicht mehr vorhanden ist, und alle Ressourcen, die das Fragment verwendet werden zu diesem Zeitpunkt freigegeben werden soll.


### <a name="using-setretaininstance"></a>Verwenden von SetRetainInstance

Es ist möglich, dass ein Fragment, um anzugeben, dass es nicht vollständig zerstört werden soll, wenn die Aktivität neu erstellt wird. Die `Fragment` Klasse stellt die Methode `SetRetainInstance` für diesen Zweck. Wenn `true` an diese Methode übergeben wird, wenn die Aktivität neu gestartet wird, wird die gleiche Instanz des Fragments verwendet. Wenn dies geschieht, und klicken Sie dann alle Rückrufmethoden, mit Ausnahme aufgerufen werden der `OnCreate` und `OnDestroy` Lebenszyklus-Rückrufe. Dieser Prozess wird in das Diagramm zum sicherungslebenszyklus (durch die grüne gepunkteten Linien) oben dargestellt.


## <a name="fragment-state-management"></a>Fragment-Zustandsverwaltung

Fragmente Mai speichern und Wiederherstellen von ihren Status während des Lebenszyklus Fragment mit einer Instanz von einem `Bundle`. Das Paket kann ein Fragment, das Speichern von Daten als Schlüssel/Wert-Paare und eignet sich für einfache Daten, die viel Arbeitsspeicher erfordern, nicht. Ein Fragment kann seinen Zustand speichern, mit einem Aufruf von `OnSaveInstanceState`:

```csharp
public override void OnSaveInstanceState(Bundle outState)
{
    base.OnSaveInstanceState(outState);
    outState.PutInt("current_choice", _currentCheckPosition);
}
```

Wenn eine neue Instanz eines Fragments erstellt wird, die im gespeicherten Zustand der `Bundle` verfügbar, mit der neuen Instanz über die `OnCreate`, `OnCreateView`, und `OnActivityCreated` Methoden der neuen Instanz.
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

Überschreiben von `OnSaveInstanceState` geeigneten Mechanismus zum Speichern vorübergehender Daten in einem Fragment auf Änderungen der bildschirmausrichtung, wie z. B. ist der `current_choice` Wert im obigen Beispiel. Allerdings die standardmäßige Implementierung des `OnSaveInstanceState` kümmert sich um kurzlebige Daten zu speichern, in der Benutzeroberfläche für jede Ansicht, die eine ID zugewiesen wurde. Betrachten Sie beispielsweise eine Anwendung mit einer `EditText` Element im XML-Code wie folgt definiert:

```xml
<EditText android:id="@+id/myText"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"/>
```

Da die `EditText` Steuerelement verfügt über eine `id` zugewiesen, das Fragment speichert die Daten automatisch im Widget beim `OnSaveInstanceState` aufgerufen wird.


### <a name="bundle-limitations"></a>Bundle-Einschränkungen

Obwohl durch die Verwendung `OnSaveInstanceState` macht es leicht zu bedienenden speichern vorübergehender Daten, der diese Methode weist einige Einschränkungen:

-  Wenn das Fragment wird in den Backstack nicht hinzugefügt, und klicken Sie dann der Zustand nicht wiederhergestellt werden, wird Wenn der Benutzer drückt die **wieder** Schaltfläche.

-  Wenn das Paket verwendet wird, um Daten zu speichern, werden diese Daten serialisiert. Dies kann zu Verzögerungen bei der Verarbeitung führen.


## <a name="contributing-to-the-menu"></a>Mitwirkung an das Menü "

Fragmente können es sich um Elemente im Menü der hostenden Aktivität beitragen.
Menüelemente wird von eine Aktivität zuerst verarbeitet. Wenn die Aktivität nicht über einen Handler verfügt, wird Klicken Sie dann das Ereignis in der Fragment, übergeben die dann übernimmt.

Um Elemente zum Menü "der Aktivität" hinzuzufügen, muss ein Fragment zwei Dinge tun.
Das Fragment muss zunächst die Methode implementieren `OnCreateOptionsMenu` und platzieren Sie die Elemente in im Menü an, wie im folgenden Code gezeigt:

```csharp
public override void OnCreateOptionsMenu(IMenu menu, MenuInflater menuInflater)
{
    menuInflater.Inflate(Resource.Menu.menu_fragment_vehicle_list, menu);
    base.OnCreateOptionsMenu(menu, menuInflater);
}
```

Klicken Sie im Menü im vorherigen Codeausschnitt wird aus den folgenden XML-Code befindet sich in der Datei vergrößert `menu_fragment_vehicle_list.xml`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
  <item android:id="@+id/add_vehicle"
        android:icon="@drawable/ic_menu_add_data"
        android:title="@string/add_vehicle" />
</menu>
```

Als Nächstes muss das Fragment Aufrufen `SetHasOptionsMenu(true)`. Der Aufruf dieser Methode kündigt für Android, dass das Fragment Menüelemente zum mitwirken an den im Listenfeld die Option verfügt. Es sei denn, die an diese Methode aufgerufen wird, werden die Menüelemente für das Fragment nicht im Listenfeld die Option der Aktivität hinzugefügt werden. Dies erfolgt normalerweise in der Lebenszyklusmethode `OnCreate()`, wie im nächsten Codeausschnitt gezeigt:

```csharp
public override void OnCreate(Bundle savedState)
{
    base.OnCreate(savedState);
    SetHasOptionsMenu(true);
}
```

Der folgende Bildschirm zeigt an, wie in diesem Menü aussehen würde:

[![Beispielhafter Screenshot der Meine Reisen app Anzeigen von Menüelementen](creating-a-fragment-images/fragment-menu-example.png)](creating-a-fragment-images/fragment-menu-example.png#lightbox)
