---
title: Garbage Collection
ms.prod: xamarin
ms.assetid: 298139E2-194F-4A58-BC2D-1D22231066C4
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/15/2018
ms.openlocfilehash: 49bb340edcad0c5ce39a2d9db6da72d488a114b1
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
ms.locfileid: "30772967"
---
# <a name="garbage-collection"></a>Garbage Collection

Xamarin.Android verwendet Mono [einfache Generationen-Garbage Collectors](http://www.mono-project.com/docs/advanced/garbage-collector/sgen/). Dies ist eine Markierung Sweep Garbage collection mit zwei Generationen und ein *LOB Speicherplatz*, mit zwei Arten von Auflistungen: 

-   Kleinere Sammlungen (sammelt Gen0 Heap) 
-   Wichtigen Auflistungen (sammelt Gen1 und LOB space Heaps). 

> [!NOTE]
> Bei Abwesenheit einer expliziten Auflistung über [GC. Collect()](xref:System.GC.Collect) Auflistungen sind *bedarfsgesteuert*, basierend auf Heapzuordnungen. *Dies ist keiner verweiszählung System*; Objekte *nicht gesammelt werden, sobald es keine ausstehenden Verweise sind*, oder wenn ein Bereich verlassen hat. Der globale Katalogserver wird ausgeführt, wenn nicht genügend Arbeitsspeicher für neue Zuordnungen kleinere Heap ausgeführt wurde. Wenn keine Zuordnungen vorhanden sind, wird er nicht ausgeführt.


Kleinere Sammlungen sind billig und häufige und werden verwendet, um die zuletzt zugeordnete und inaktive Objekten zu sammeln. Kleinere Sammlungen werden nach jedem einige MB von zugeordneten Objekten ausgeführt. Kleinere Sammlungen möglicherweise manuell durchgeführt werden, durch den Aufruf [GC. Sammeln von (0)](/dotnet/api/system.gc.collect#System_GC_Collect_System_Int32_) 

Wichtige Sammlungen sind teuer und weniger häufig und werden verwendet, um alle inaktiven Objekten freizugeben. Wichtige Sammlungen werden ausgeführt, sobald Arbeitsspeicher für die aktuelle Heapgröße (vor dem Ändern der Größe der Heap) ausgeschöpft ist. Wichtige Sammlungen möglicherweise manuell durchgeführt werden, durch den Aufruf [GC. Sammeln von ()](xref:System.GC.Collect) oder durch Aufrufen von [GC. Sammeln von (Int)](/dotnet/api/system.gc.collect#System_GC_Collect_System_Int32_) mit dem Argument [GC. MaxGeneration](xref:System.GC.MaxGeneration). 



## <a name="cross-vm-object-collections"></a>Cross-VM-Auflistungen

Es gibt drei Kategorien von Objekttypen.

-   **Verwaltete Objekte**: Typen, die sind *nicht* Vererben [Java.Lang.Object](https://developer.xamarin.com/api/type/Java.Lang.Object/) , z. B. [System.String](xref:System.String). 
    Diese werden normalerweise durch das GC gesammelt. 

-   **Java-Objekte**: Java-Typen in der Android-Runtime-VM vorhanden ist, aber nicht für den Mono-VM verfügbar gemacht. Diese sind langweilig und wird nicht weiter erläutert werden. Diese werden normalerweise von der Android-Laufzeit VM gesammelt. 

-   **Peer-Objekte**: Typen welche implementieren [IJavaObject](https://developer.xamarin.com/api/type/Android.Runtime.IJavaObject/) , z. B. alle [Java.Lang.Object](https://developer.xamarin.com/api/type/Java.Lang.Object/) und ["java.lang.Throwable"](https://developer.xamarin.com/api/type/Java.Lang.Throwable/) Unterklassen. Instanzen dieser Typen haben zwei "Halfs" eine *verwalteter Peer* und ein *native Peer*. Der verwaltete Peer ist eine Instanz der C#-Klasse. Der systemeigene Peer ist eine Instanz einer Javaklasse innerhalb der Android-Laufzeit-VM und die C#- [IJavaObject.Handle](https://developer.xamarin.com/api/property/Android.Runtime.IJavaObject.Handle/) Eigenschaft enthält einen globalen JNI-Verweis auf den systemeigenen Peer. 


Es gibt zwei Arten von systemeigenen Peers:

-   **Framework Peers** : "Normal" Java-Datentypen, wenn nichts Xamarin.Android, z. B. bereits wissen [android.content.Context](https://developer.xamarin.com/api/type/Android.Content.Context/).

-   **Benutzer Peers** : [Android Aufrufwrappern](~/android/platform/java-integration/working-with-jni.md) das während des Buildvorgangs für jede Java.Lang.Object-Unterklasse, die in der Anwendung generiert werden.


Es sind zwei VMs innerhalb eines Prozesses Xamarin.Android, stehen zwei Arten der Garbage collection:

-   Android-Runtime-Auflistungen 
-   Mono-/ Sammlungen 

Sammlungen von Android-Runtime ausgeführt werden, normalerweise, aber mit einer Einschränkung: ein globale JNI-Verweis wird als Stamm-GC behandelt. Daher ist es eine JNI globale verweisen eine Android-Laufzeit VM-Objekts, das Objekt hält *kann nicht* gesammelt werden, auch wenn es andernfalls für die Auflistung geeignet ist.

Mono-/ Auflistungen sind, in dem das Fun geschieht. Verwaltete Objekte werden normalerweise gesammelt. Objekte auf derselben Ebene werden gesammelt, indem Sie die folgende Schritte ausführen:

1.  Alle für die Mono / Collection Objekte auf derselben Ebene haben ihre globalen JNI-Verweis durch eine globale JNI schwachen ersetzt. 

2.  Eine Android-Laufzeit VM GC wird aufgerufen. Eine beliebige systemeigene Peerinstanz kann gesammelt werden. 

3.  Die JNI schwache globalen Verweise erstellt (1) werden überprüft. Wenn der schwache Verweis gesammelt wurden, ist die Peer-Objekts erfasst. Wenn der schwache Verweis verfügt über *nicht* erfasst wurden, klicken Sie dann der schwache Verweis wird mit einem globalen JNI-Verweis ersetzt und das Peer-Objekt ist nicht erfasst. Hinweis: auf API-14+, dies bedeutet, dass der Rückgabewert `IJavaObject.Handle` möglicherweise nach einem globalen Katalog zu ändern. 

All dies ist erforderlich, eine Instanz eines Objekts Peer live wird als entweder verweist das Endergebnis zu verwaltetem Code (z. B. in gespeicherten eine `static` Variable) oder von Java-Code verwiesen wird. Darüber hinaus wird die Lebensdauer des systemeigenen Peers hinter andernfalls würde erweitert live, während der systemeigene Peer nicht entladbarer, bis Native Peer und den verwalteten Peer entladbare sind.


## <a name="object-cycles"></a>Objekt-Zyklen

Objekte auf derselben Ebene werden logisch innerhalb der Android-Laufzeit und die Mono-VM vorhanden. Angenommen, ein [Android.App.Activity](https://developer.xamarin.com/api/type/Android.App.Activity/) verwalteter Peer-Instanz weist einen entsprechenden [android.app.Activity](http://developer.android.com/reference/android/app/Activity.html) Framework Peer Java-Instanz. Alle Objekte, die von erben [Java.Lang.Object](https://developer.xamarin.com/api/type/Java.Lang.Object/) können beide virtuellen Computer heraus ausgetauschten erwartet werden. 

Alle Objekte, die Darstellung in beide virtuellen Computer werden verfügen über eine Lebensdauer, die erweitert werden im Vergleich zu Objekten, die nur innerhalb einer einzelnen virtuellen Computer vorhanden sind (z. B. eine [ `System.Collections.Generic.List<int>` ](xref:System.Collections.Generic.List%601)). Aufrufen von [GC. Sammeln von](xref:System.GC.Collect) wird nicht unbedingt diese Objekte sammeln, wie der Xamarin.Android globale Katalogserver muss sicherstellen, dass das Objekt vor dem durch die Sammlung von einer virtuellen Maschine verwiesen wird nicht. 

Lebensdauer eines Objekts, zu verkürzen [Java.Lang.Object.Dispose()](https://developer.xamarin.com/api/member/Java.Lang.Object.Dispose/) aufgerufen werden soll. Dies wird manuell "die Verbindung für das Objekt zwischen den zwei VMs freigegeben werden und des globale Verweis und somit ermöglicht die Objekte schneller gesammelt werden getrennt". 


## <a name="automatic-collections"></a>Automatische Sammlungen

Beginnend mit [Version 4.1.0](https://developer.xamarin.com/releases/android/mono_for_android_4/mono_for_android_4.1.0), Xamarin.Android wird automatisch eine vollständige GC ausführt, wenn ein Gref Schwellenwert überschritten wird. Dieser Schwellenwert ist 90 % der die bekannten maximale Grefs für die Plattform: 1800 Grefs auf dem Emulator (max. 2000) und 46800 Grefs auf Hardware (maximale 52000). *Hinweis:* Xamarin.Android zählt nur die Grefs erstellt, indem [Android.Runtime.JNIEnv](https://developer.xamarin.com/api/type/Android.Runtime.JNIEnv/), und alle anderen Grefs erstellt wurden, nicht kennen. Dies ist eine heuristische *nur*. 

Wenn eine automatische Auflistung ausgeführt wird, wird eine Meldung ähnlich der folgenden in das Debugprotokoll ausgegeben:

```shell
I/monodroid-gc(PID): 46800 outstanding GREFs. Performing a full GC!
```

Das Vorkommen dieser ist nicht deterministisch und kann zu ungünstigen Zeitpunkten (z. B. in der Mitte Grafikrendering) auftreten. Wenn Ihnen diese Meldung angezeigt, empfiehlt es sich um eine explizite Auflistung an anderer Stelle auszuführen oder Sie, wiederholen Sie den möchten [reduzieren Sie die Lebensdauer der Objekte auf derselben Ebene](#Helping_the_GC). 

## <a name="gc-bridge-options"></a>GC-Bridge-Optionen

Xamarin.Android bietet transparente Speicherverwaltung mit Android und Android-Runtime. Es wird als eine Erweiterung der Mono / Garbage Collector aufgerufen, implementiert die *GC-Bridge*. 

Der GC-Bridge funktioniert während einer Mono / Garbagecollection und Abbildungen, welche Peer Objekte benötigt ihre "Verfügbarkeit" mit dem Android Laufzeitheap überprüft werden. Der GC-Bridge trifft diese Entscheidung, indem Sie die folgenden Schritte aus (in entsprechender Reihenfolge):

1.  Auslösen der Mono / bezugsdiagramm unreachable Peer-Objekte in der Java-Objekte, die sie darstellen. 

2.  Führen Sie eine Java-GC.

3.  Überprüfen Sie, welche Objekte wirklich inaktiv sind. 

Diese kompliziert ist, was ermöglicht, dass Unterklassen des `Java.Lang.Object` problemlos auf die Objekte; es entfernt keine Einschränkungen auf der Java-Objekte in c# gebunden werden können. Aufgrund dieser Komplexität der Bridge-Prozess kann sehr teuer sein und kann bemerkbar Pausen in einer Anwendung verursachen. Wenn die Anwendung erhebliche Pausen auftreten, lohnt es sich eine der folgenden drei GC-Bridge Implementierungen untersuchen: 

-   **Tarjan** -anhand ein vollständig neuer Entwurf von der Bridge GC [Robert Tarjans-Algorithmus und Abwärtskompatibilität verweisen Weitergabe](http://en.wikipedia.org/wiki/Tarjan's_strongly_connected_components_algorithm).
    Kann die beste Leistung unter unsere simulierten Arbeitslasten, aber sie hat auch den größeren Anteil experimentellen Code. 

-   **Neue** -eine größere Überarbeitung des ursprünglichen Codes, korrigieren und zwei Instanzen von quadratische Verhalten werden jedoch beibehalten der Kernalgorithmus (basierend auf [Kosarajus Algorithmus](http://en.wikipedia.org/wiki/Kosaraju's_algorithm) suchen stark von den verbundenen Komponenten). 

-   **Alte** – die ursprüngliche Implementierung (stabilste drei betrachtet). Dies ist die Brücke, die eine Anwendung verwenden soll, wenn die `GC_BRIDGE` Pausen sind zulässig. 


Die einzige Möglichkeit, herauszufinden, welche GC-Bridge funktioniert am besten ist in einer Anwendung experimentieren und analysieren die Ausgabe. Es gibt zwei Möglichkeiten zum Erfassen der Daten für benchmarking: 

-   **Aktivieren der Protokollierung** -Protokollierung aktivieren (wie in beschrieben, die [Konfiguration](~/android/internals/garbage-collection.md) Abschnitt) dann für jede einzelne Option GC-Brücke zu erfassen und vergleichen Sie die Protokoll-Ausgaben von jeder Einstellung. Überprüfen Sie die `GC` Nachrichten für jede einzelne Option; insbesondere die `GC_BRIDGE` Nachrichten. Hält bis zu 150ms für nicht interaktive Anwendungen tolerierbar sind, aber die Pausen oben 60ms für sehr interaktive Anwendungen (z. B. Spiele) ein Problem aufgetreten sind. 

-   **Bridge Kontoführung aktivieren** -Bridge Kontoführung werden die durchschnittliche Kosten der Objekte, die durch jede der Bridge-Prozess Beteiligten Objekts angezeigt. Diese Informationen nach Größe sortieren, gebe Hints, was den maximalen Speicherplatz zusätzliche Objekte enthalten kann. 


Angeben, welche `GC_BRIDGE` Option eine Anwendung sollte, übergeben Sie `bridge-implementation=old`, `bridge-implementation=new` oder `bridge-implementation=tarjan` auf die `MONO_GC_PARAMS` Umgebungsvariable, beispielsweise: 

```shell
MONO_GC_PARAMS=bridge-implementation=tarjan
```

Die Standardeinstellung ist **Tarjan**. Wenn Sie eine Regression finden, Umständen ist es notwendig, diese Option festgelegt **alte**. Darüber hinaus können mithilfe der stabiler **alte** option verwenden, wenn **Tarjan** erzeugt keine Verbesserung der Leistung. 

<a name="Helping_the_GC" />

## <a name="helping-the-gc"></a>Helfen dem globalen Katalogserver

Es gibt mehrere Möglichkeiten, mit denen dem globalen Katalogserver zu Arbeitsspeicher-Nutzung und Sammlung von Zeiten zu minimieren.



### <a name="disposing-of-peer-instances"></a>Freigeben von Peer-Instanzen

Der globale Katalogserver wurde eine unvollständige Ansicht des Prozesses und möglicherweise nicht ausgeführt werden. Wenn Arbeitsspeicher zu gering ist, da nicht, dass der Arbeitsspeicher ist der globale Katalogserver bekannt ist niedrig. 

Z. B. einer Instanz von einer [Java.Lang.Object](https://developer.xamarin.com/api/type/Java.Lang.Object/) Typ oder abgeleiteten Typ ist mindestens einer Größe von 20 Bytes (ohne Hinweis usw. ist implementierungsspezifisch usw..). 
[Verwaltete Callable Wrapper](~/android/internals/architecture.md) fügen Sie keine zusätzlichen Instanzmember, also wenn unbedingt notwendig ein [Android.Graphics.Bitmap](https://developer.xamarin.com/api/type/Android.Graphics.Bitmap/) -Instanz, die in ein Blob 10 MB des Arbeitsspeichers, bezieht sich Xamarin.Android des GC erfahren Sie nicht, die &ndash; dem globalen Katalogserver ein 20-Byte-Objekt wird angezeigt, und wird in der Lage, feststellen, dass sie auf Android Runtime zugeordnete Objekte, die 10 MB des Arbeitsspeichers aktiv halten verknüpft ist. 

Es ist häufig erforderlich, die das GC-Hilfe. Leider *GC. AddMemoryPressure()* und *GC. RemoveMemoryPressure()* werden nicht unterstützt, wenn also Sie *wissen* , dass eine große Java zugewiesene Objektdiagramm gerade reserviert wird, Sie möglicherweise manuell aufrufen müssen [GC. Collect()](xref:System.GC.Collect) auf Aufforderung einen GC zum Freigeben von der Java-Seite Arbeitsspeicher, oder Sie können explizit freigeben, *Java.Lang.Object* Unterklassen, unterbrechen die Zuordnung zwischen den verwalteten callable Wrapper und Java-Instanz. Beispielsweise finden Sie unter [Fehler 1084](http://bugzilla.xamarin.com/show_bug.cgi?id=1084#c6). 


> [!NOTE]
> Sie muss *extrem* Sie vorsichtig, wenn der disposing `Java.Lang.Object` Unterklasse-Instanzen.

Um die Möglichkeit einer beschädigten Speicher zu minimieren, beachten Sie die folgenden Richtlinien beim Aufrufen von `Dispose()`.


#### <a name="sharing-between-multiple-threads"></a>Die gemeinsame Nutzung von mehreren Threads

Wenn die *Java oder verwaltete* Instanz kann von mehreren Threads gemeinsam genutzt werden *sollte nicht sein `Dispose()`d*, **jemals**. Beispielsweise [ `Typeface.Create()` ](https://developer.xamarin.com/api/member/Android.Graphics.Typeface.Create/(System.String%2cAndroid.Graphics.TypefaceStyle)) gelegten eine *zwischengespeicherte Instanz*. Wenn mehrere Threads die gleichen Argumente angeben, erhalten sie die *gleichen* Instanz. Folglich `Dispose()`lauscht mithilfe von der `Typeface` Instanz von einem Thread möglicherweise werden andere Threads führen können `ArgumentException`e aus `JNIEnv.CallVoidMethod()` (neben anderen), da die Instanz von einem anderen Thread freigegeben wurde. 


#### <a name="disposing-bound-java-types"></a>Freigeben von gebundenen Java-Typen

Wenn die Instanz eine Java-Typ gebunden ist, kann die Instanz freigegeben werden, der *solange* die Instanz wird nicht wiederverwendet werden, aus verwaltetem Code *und* Java-Instanz kann nicht freigegeben werden, unter anderem Threads (Siehe vorangegangene `Typeface.Create()` Diskussion). (Dieser Feststellung kann schwierig sein.) Das nächste Mal Java-Instanz geht in verwaltetem Code, ein *neue* Wrapper dafür erstellt werden. 

Dies ist häufig nützlich, bei der Drawables und andere Ressourcen beansprucht Instanzen:

```csharp
using (var d = Drawable.CreateFromPath ("path/to/filename"))
    imageView.SetImageDrawable (d);
```

Der oben genannten ruhig da Peers, [Drawable.CreateFromPath()](https://developer.xamarin.com/api/member/Android.Graphics.Drawables.Drawable.CreateFromPath/) gibt auf einem Peer Framework verweist, *nicht* Benutzer Peer. Die `Dispose()` rufen Sie am Ende der `using` Block wird die Beziehung zwischen verwalteten unterbrochen [Drawable](https://developer.xamarin.com/api/type/Android.Graphics.Drawables.Drawable/) und Framework [Drawable](http://developer.android.com/reference/android/graphics/drawable/Drawable.html) Instanzen, sodass die Java-Instanz sein gesammelt werden, sobald die Android-Laufzeit muss. Dies würde *nicht* sicher sind, wenn Peer-Instanz für einen Benutzer Peer bezeichnet werden; hier verwenden wir die "external" Informationen zu *wissen* , die die `Drawable` kann nicht auf einem Peer Benutzer verweisen und somit die `Dispose()` aufrufen ist threadsicher. 


#### <a name="disposing-other-types"></a>Andere Typen freigeben 

Wenn die Instanz auf einen Typ verweist, die eine Bindung eines Java-Typs nicht (z. B. eine benutzerdefinierte `Activity`), **nicht** Aufrufen `Dispose()` es sei denn, Sie *wissen* , dass keine Java-Code auf, die überschriebene Methoden aufruft -Instanz. Zu diesem Zweck führt dies zu [ `NotSupportedException`s](~/android/internals/architecture.md#Premature_Dispose_Calls). 

Z. B. Wenn Sie eine benutzerdefinierte klicken Sie auf Listener:

```csharp
partial class MyClickListener : Java.Lang.Object, View.IOnClickListener {
    // ...
}
```

Sie *sollte nicht* dispose dieser Instanz, wie Java versucht, in der Zukunft Aufrufen von Methoden auf:

```csharp
// BAD CODE; DO NOT USE
Button b = FindViewById<Button> (Resource.Id.myButton);
using (var listener = new MyClickListener ())
    b.SetOnClickListener (listener);
```


#### <a name="using-explicit-checks-to-avoid-exceptions"></a>Verwenden explizite Kontrolle Vermeidung von Ausnahmen

Wenn Sie implementiert haben eine [Java.Lang.Object.Dispose](https://developer.xamarin.com/api/member/Java.Lang.Object.Dispose(System.Boolean)/) überladen Methode sollten Sie vermeiden, berühren Objekte, die JNI einschließen. Erstellen Sie auf diese Weise kann eine *Double Dispose* Situation, mit dem Ihr Code auch (dass schwer) kann versuchen, auf ein zugrunde liegendes Java-Objekt zuzugreifen, die bereits der Garbage Collection stattgefunden hat. Auf diese Weise wird eine Ausnahme ähnlich der folgenden erzeugt: 

```shell
System.ArgumentException: 'jobject' must not be IntPtr.Zero.
Parameter name: jobject
at Android.Runtime.JNIEnv.CallVoidMethod
```

Diese Situation tritt häufig beim des ersten Dispose eines Objekts führt dazu, dass ein Element null sind und eine nachfolgende Zugriffsversuch auf null-Element löst dann eine Ausnahme ausgelöst wird. Insbesondere des Objekts `Handle` (die eine verwaltete Instanz mit der zugrunde liegenden Java-Instanz verknüpft ist) auf der ersten Dispose ungültig ist, sondern es verwalteter Code versucht weiterhin, diese zugrunde liegenden Java-Instanz zugreifen, obwohl er nicht mehr verfügbar (siehe ist[ Verwaltet Aufrufwrappern](~/android/internals/architecture.md#Managed_Callable_Wrappers) Weitere Informationen über die Zuordnung zwischen den Java-Instanzen und verwaltete Instanzen). 

Eine gute Möglichkeit, zu verhindern, dass diese Ausnahme wird explizit in Überprüfen Ihrer `Dispose` -Methode, die die Zuordnung zwischen der verwalteten Instanz und der zugrunde liegenden Java-Instanz ist noch gültig ist, überprüfen, wenn des Objekts `Handle` ist null (`IntPtr.Zero`) vor dem Zugriff auf ihre Member. Beispielsweise die folgenden `Dispose` Methode greift auf ein `childViews` Objekt: 

```csharp
class MyClass : Java.Lang.Object, ISomeInterface 
{
    protected override void Dispose (bool disposing)
    {
        base.Dispose (disposing);
        for (int i = 0; i < this.childViews.Count; ++i)
        {
            // ...
        }
    }
}
```

Wenn eine anfängliche Dispose Ursachen übergeben `childViews` haben einen ungültigen `Handle`, `for` Schleife Zugriff löst eine `ArgumentException`. Durch Hinzufügen einer expliziten `Handle` null, überprüfen Sie vor dem ersten `childViews` für den Zugriff auf die folgenden `Dispose` Methode verhindert, dass die Ausnahme tritt auf: 

```csharp
class MyClass : Java.Lang.Object, ISomeInterface 
{
    protected override void Dispose (bool disposing)
    {
        base.Dispose (disposing);

        // Check for a null handle:
        if (this.childViews.Handle == IntPtr.Zero)
            return;

        for (int i = 0; i < this.childViews.Count; ++i)
        {
            // ...
        }
    }
}
```


### <a name="reduce-referenced-instances"></a>Reduzieren Sie Instanzen auf die verwiesen wird

Wenn eine Instanz von eine `Java.Lang.Object` Typ oder Unterklassen wird überprüft, während die Garbage Collection, die gesamte *Objektdiagramm* , dass die Instanz bezieht muss ebenfalls überprüft werden. Objektdiagramm ist der Satz von Objektinstanzen, die auf die Instanz"Root" *plus* alles verwiesen wird, durch welche die Stamminstanz verweist, rekursiv. 

Betrachten Sie die folgende Klasse:

```csharp
class BadActivity : Activity {

    private List<string> strings;

    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);

        strings.Value = new List<string> (
                Enumerable.Range (0, 10000)
                .Select(v => new string ('x', v % 1000)));
    }
}
```

Wenn `BadActivity` wird erstellt, wird das Objektdiagramm 10004 Instanzen enthalten (1 X `BadActivity`, 1 X `strings`, 1 X `string[]` von reservierten `strings`, 10000 x Zeichenfolgeninstanzen), *alle* von dem werden müssen überprüft, wenn die `BadActivity` Instanz wird überprüft. 

Dies hat möglicherweise nachteilige Auswirkungen auf die Auflistung auf Zeiten, wodurch GC anhalten Zeiten erhöht. 

Helfen Sie dem globalen Katalogserver von *reduzieren* die Größe von Objektdiagrammen, die von Peer-Benutzerinstanzen über einen Stamm verfügen. Im obigen Beispiel hierzu umgezogen `BadActivity.strings` in eine separate Klasse, die von Java.Lang.Object erben nicht: 

```csharp
class HiddenReference<T> {

    static Dictionary<int, T> table = new Dictionary<int, T> ();
    static int idgen = 0;

    int id;

    public HiddenReference ()
    {
        lock (table) {
            id = idgen ++;
        }
    }

    ~HiddenReference ()
    {
        lock (table) {
            table.Remove (id);
        }
    }

    public T Value {
        get { lock (table) { return table [id]; } }
        set { lock (table) { table [id] = value; } }
    }
}

class BetterActivity : Activity {

    HiddenReference<List<string>> strings = new HiddenReference<List<string>>();

    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);

        strings.Value = new List<string> (
                Enumerable.Range (0, 10000)
                .Select(v => new string ('x', v % 1000)));
    }
}
```


## <a name="minor-collections"></a>Kleinere Sammlungen

Kleinere Sammlungen möglicherweise manuell durchgeführt werden, durch den Aufruf [GC. Collect(0)](xref:System.GC.Collect). Kleinere Sammlungen sind (im Vergleich zu wichtigen Auflistungen) billig, aber eine erhebliche festen Kosten, damit nicht zu häufig ausgelöst werden sollen, und müssen eine Verzögerung von wenigen Millisekunden. 

Wenn die Anwendung eine "Arbeitszyklus verfügt" in der dasselbe fortlaufend ausgeführt wird, kann es ratsam sein manuell Registrierungsvorgang eine kleinere Auflistung nach Druckvolumen beendet wurde. Beispiel-Zoll-Zyklen umfassen: 

-  Die von einem einzelnen Spiel Frame Renderingzyklus.
-  Die gesamte Interaktion mit einer bestimmten app-Dialogfeld (öffnen, füllen schließen) 
-  Eine Gruppe von Anforderungen über das Netzwerk aktualisieren/app-Daten synchronisiert werden sollen.



## <a name="major-collections"></a>Wichtige Sammlungen

Wichtige Sammlungen möglicherweise manuell durchgeführt werden, durch den Aufruf [GC. Collect()](xref:System.GC.Collect) oder `GC.Collect(GC.MaxGeneration)`. 

Sie sollten nur selten ausgeführt werden und möglicherweise eine Verzögerung von einer Sekunde auf einem Gerät Android-Format, wenn einen Heap 512 MB zu sammeln. 

Wichtige Sammlungen sollte nur manuell, falls überhaupt aufgerufen werden: 

-   Am Ende der langwierige Aufgabe wird nicht Zyklen und wenn ein langer anhalten für dem Benutzer ein Problem darstellen. 

-   Einer überschriebenen [Android.App.Activity.OnLowMemory()](https://developer.xamarin.com/api/member/Android.App.Activity.OnLowMemory/) Methode. 



## <a name="diagnostics"></a>Diagnose

Um nachzuverfolgen, wenn globale Verweise erstellt und zerstört werden, legen Sie die [debug.mono.log](~/android/troubleshooting/index.md) Systemeigenschaft enthalten [ *Gref* ](~/android/troubleshooting/index.md) und/oder [ *gc*](~/android/troubleshooting/index.md). 



## <a name="configuration"></a>Konfiguration

Der Xamarin.Android Garbagecollector kann konfiguriert werden, durch Festlegen der `MONO_GC_PARAMS` -Umgebungsvariablen angegeben. Umgebungsvariablen festgelegt werden können, mit dem Buildvorgang [AndroidEnvironment](~/android/deploy-test/environment.md).

Die `MONO_GC_PARAMS` Umgebungsvariable ist eine durch Trennzeichen getrennte Liste der folgenden Parameter: 

-   `nursery-size` = *Größe* : Legt die Größe der Baumschule fest. Die Größe in Bytes angegeben und muss eine Potenz von zwei sein. Die Suffixe `k` , `m` und `g` können verwendet werden, um kg, Megapixel und Gigabytes, anzugeben. Der Baumschule ist die erste Generation (von zwei). Eine größere Baumschule beschleunigt die Anwendung in der Regel jedoch offensichtlich beanspruchen mehr Arbeitsspeicher. Der Standard-Baumschule Größe 512 kb. 

-   `soft-heap-limit` = *Größe* : die maximale Ziel verwaltet die arbeitsspeichernutzung für die app. Wenn Speicher unter dem angegebenen Wert liegt, wird der globale Katalogserver für die Ausführungszeit (weniger Sammlungen) optimiert. 
    Über diesen Grenzwert ist der globale Katalogserver für die speicherauslastung (mehrere Sammlungen) optimiert. 

-   `evacuation-threshold` = *Schwellenwert* : Legt den Auslagern Schwellenwert in Prozent. Der Wert muss eine ganze Zahl im Bereich zwischen 0 und 100 sein. Der Standardwert ist 66. Wenn die Sweep-Phase der Auflistung gefunden wird, dass die Belegung der einen bestimmten Heap Blocktyp unterhalb dieses Prozentsatzes liegt, kommt es eine kopieren Sammlung für diese Blocktyp in der nächsten wichtigen Auflistung dadurch Wiederherstellung erforderlich sind, fast 100 % beträgt. Der Wert 0 deaktiviert auslagern. 

-   `bridge-implementation` = *Implementierung überbrückt* : Dadurch wird der GC-Bridge-Option aus, um die Adresse GC-Leistungsprobleme festgelegt. Es gibt drei mögliche Werte: *alte* , *neue* , *Tarjan*.

-   `bridge-require-precise-merge`: Die Tarjan Bridge eine Optimierung, die in seltenen Fällen ein Objekt führen kann enthält, gesammelt eine GC, nachdem er zuerst Garbage wird. Z. B. diese Option wird deaktiviert, Optimierung, wodurch globale Kataloge besser vorhersagbar, jedoch möglicherweise langsamer aus.

Z. B. zum Konfigurieren der globale Katalogserver, um einen Heap-Limit von 128 MB haben hinzufügen eine neue Datei zum Projekt mit einem **Buildvorgang** von `AndroidEnvironment` mit dem Inhalt: 

```shell
MONO_GC_PARAMS=soft-heap-limit=128m
```
