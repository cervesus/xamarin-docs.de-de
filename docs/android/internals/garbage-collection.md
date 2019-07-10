---
title: Garbage Collection
ms.prod: xamarin
ms.assetid: 298139E2-194F-4A58-BC2D-1D22231066C4
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/15/2018
ms.openlocfilehash: 09466cc9eed4899ef0aa1198ff0aee5cd420e110
ms.sourcegitcommit: 58d8bbc19ead3eb535fb8248710d93ba0892e05d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2019
ms.locfileid: "67674633"
---
# <a name="garbage-collection"></a>Garbage Collection

Xamarin.Android verwendet die Mono [einfache Generationeller Garbagecollector](https://www.mono-project.com/docs/advanced/garbage-collector/sgen/). Dies ist ein Mark-and-Sweep-Garbage Collector mit zwei Generationen und *große Objektraum*, mit zwei Arten von Sammlungen: 

-   Kleinere Sammlungen (sammelt Gen0-Heap) 
-   Wichtige Sammlungen (gesammelt, Gen1 und LOB Leerzeichen Heaps). 

> [!NOTE]
> Ohne eine explizite Auflistung über [GC. Collect()](xref:System.GC.Collect) Sammlungen sind *bei Bedarf*, basierend auf dem Heap-Zuordnungen. *Dies ist dabei nicht um ein System für die verweiszählung*; Objekte *nicht gesammelt werden, sobald es keine ausstehenden Verweise gibt*, oder wenn ein Bereich wurde beendet. Der Garbage Collector wird ausgeführt, wenn der kleinere Heap nicht genügend Arbeitsspeicher für neue Zuweisungen ausgeführt wurde. Wenn keine Zuordnungen vorhanden sind, wird er nicht ausgeführt.


Kleinere Sammlungen sind günstig und häufige und werden verwendet, um die zuletzt zugeordnete und inaktive Objekte zu sammeln. Kleinere Sammlungen werden nach jedem Paar MB von zugeordneten Objekten ausgeführt. Kleinere Sammlungen können manuell ausgeführt werden, durch den Aufruf [GC. Sammeln von (0)](/dotnet/api/system.gc.collect#System_GC_Collect_System_Int32_) 

Wichtige Sammlungen teuer sind und weniger häufig und werden verwendet, um alle inaktiven Objekten freizugeben. Wichtige Sammlungen werden ausgeführt, sobald der Speicher für die aktuelle Heapgröße (vor dem Ändern der Größe des Heaps) ausgeschöpft ist. Wichtige Sammlungen können manuell ausgeführt werden, durch den Aufruf [GC. Sammeln von ()](xref:System.GC.Collect) oder durch Aufrufen von [GC. Sammeln von (Int)](/dotnet/api/system.gc.collect#System_GC_Collect_System_Int32_) mit dem Argument [GC. MaxGeneration](xref:System.GC.MaxGeneration). 



## <a name="cross-vm-object-collections"></a>VM-übergreifenden Auflistungen

Es gibt drei Kategorien von Objekttypen an.

-   **Verwaltete Objekte**: Typen, wozu *nicht* erben [Java.Lang.Object](https://developer.xamarin.com/api/type/Java.Lang.Object/) , z. B. [System.String](xref:System.String). 
    Diese werden normalerweise von der Garbage Collector gesammelt. 

-   **Java-Objekten**: Java-Typen in der Android-Runtime-VM vorhanden sind, aber nicht für die Mono-VM verfügbar gemacht. Diese sind langweilig und wird nicht weiter erläutert werden. Diese werden normalerweise von der Android-Runtime-VM gesammelt. 

-   **Peer-Objekte**: Typen der implementieren [IJavaObject](https://developer.xamarin.com/api/type/Android.Runtime.IJavaObject/) , z. B. alle [Java.Lang.Object](https://developer.xamarin.com/api/type/Java.Lang.Object/) und [Java.Lang.Throwable](https://developer.xamarin.com/api/type/Java.Lang.Throwable/) Unterklassen. Instanzen dieser Typen haben zwei "Halfs" eine *verwalteter Peer* und *native Peer*. Der verwaltete Peer ist eine Instanz von der C# Klasse. Der systemeigene Peer ist eine Instanz einer Java-Klasse in der Android-Runtime-VM, und die C# [IJavaObject.Handle](https://developer.xamarin.com/api/property/Android.Runtime.IJavaObject.Handle/) Eigenschaft enthält einen globalen JNI-verweisen, die dem nativen Peer. 


Es gibt zwei Arten von systemeigenen Peers:

-   **Framework-Peers** : "Normal" Java-Typen, die nichts von Xamarin.Android, z. B. wissen [android.content.Context](https://developer.xamarin.com/api/type/Android.Content.Context/).

-   **Benutzer-Peers** : [Android Callable Wrapper](~/android/platform/java-integration/working-with-jni.md) der Buildzeit für jede Java.Lang.Object-Unterklasse, die in der Anwendung vorhanden generiert werden.


Wie stehen zwei virtuelle Computer innerhalb eines Xamarin.Android-Prozesses an, gibt es zwei Arten der Garbage Collections:

-   Android-Runtime-Auflistungen 
-   Mono-Sammlungen 

Android-Runtime-Auflistungen ausgeführt werden, normalerweise, aber mit Vorsicht zu genießen: einen globalen JNI-verweisen als GC-Stamm behandelt. Daher ist es eine JNI globale verweisen auf ein Android-Runtime-VM-Objekt, das Objekt belegt *kann nicht* gesammelt werden, auch wenn es andernfalls Garbage Collection ist.

Mono-Sammlungen sind, in dem der Spaß geschieht. Verwaltete Objekte werden normalerweise nicht gesammelt. Objekte auf derselben Ebene werden gesammelt, indem Sie den folgenden Prozess ausführen:

1.  Alle Peer-Objekte, die für die Mono-Collection haben ihre globalen JNI-verweisen, die durch eine schwache globalen JNI-verweisen ersetzt. 

2.  Eine Android-Runtime-VM-GC wird aufgerufen. Eine beliebige systemeigene Peerinstanz kann erfasst werden. 

3.  Die schwachen JIN globale-verweisen in (1) erstellt werden überprüft. Wenn der schwache Verweis erfasst wurden, erfasst das Peerobjekt klicken Sie dann. Wenn der schwache Verweis ist *nicht* erfasst wurden, klicken Sie dann der schwache Verweis wird durch einen globalen JNI-verweisen ersetzt und das Peerobjekt werden nicht gesammelt. Hinweis: auf API 14 +, dies bedeutet, dass den Rückgabewert `IJavaObject.Handle` nach einer Garbage Collection kann sich ändern. 

Das Endergebnis aller dafür ist, dass eine Instanz eines Objekts Peer laufen, solange entweder darauf verweist, verwalteten Code (z. B. in gespeicherten eine `static` Variable) oder durch Java-Code auf die verwiesen wird. Darüber hinaus wird die Lebensdauer der systemeigenen Peers erweitert werden, nach was andernfalls würden Leben, da die systemeigene Peer entladbaren nicht bis sowohl die systemeigene Peer und dem Peer verwaltet entladbaren sind.


## <a name="object-cycles"></a>Objekt-Zyklen

Objekte auf derselben Ebene sind logisch in der Android-Laufzeit und des virtuellen Computers die Mono vorhanden. Z. B. eine [Android.App.Activity](https://developer.xamarin.com/api/type/Android.App.Activity/) verwalteter Peer-Instanz wird das Vorhandensein einer entsprechenden [android.app.Activity](https://developer.android.com/reference/android/app/Activity.html) Framework Peer-Java-Instanz. Alle Objekte, die von erben [Java.Lang.Object](https://developer.xamarin.com/api/type/Java.Lang.Object/) damit Darstellungen in beiden virtuellen Computern zu erwarten. 

Alle Objekte, die Darstellung auf beiden virtuellen Computern werden verfügen über eine Lebensdauer, die erweitert werden, im Vergleich zu den Objekten, die nur in einem einzelnen virtuellen Computer vorhanden sind (z. B. eine [ `System.Collections.Generic.List<int>` ](xref:System.Collections.Generic.List%601)). Aufrufen von [GC. Sammeln von](xref:System.GC.Collect) wird nicht unbedingt diese Objekte sammeln, wie die Xamarin.Android-GC muss sicherstellen, dass die von beiden virtuellen Computer auf das Objekt verwiesen wird nicht, bevor die Sammlung. 

Um die Lebensdauer des Objekts, zu verkürzen [Java.Lang.Object.Dispose()](https://developer.xamarin.com/api/member/Java.Lang.Object.Dispose/) aufgerufen werden soll. Dies wird manuell "die Verbindung für das Objekt zwischen den beiden VMs freigegeben werden und des globalen Verweis, sodass die Objekte schneller gesammelt werden getrennt". 


## <a name="automatic-collections"></a>Automatische Sammlungen

Beginnend mit [Version 4.1.0](https://developer.xamarin.com/releases/android/mono_for_android_4/mono_for_android_4.1.0), Xamarin.Android automatisch zu eine vollständige GC anfallenden ausführt, wenn ein Gref-Schwellenwert überschritten wird. Dieser Schwellenwert ist 90 Prozent der bekannten maximale Grefs für die Plattform: 1800 Grefs auf dem Emulator (max. 2000), und 46800 Grefs auf Hardware (maximale 52000). *Hinweis*: Xamarin.Android zählt nur die Grefs erstellt [Android.Runtime.JNIEnv](https://developer.xamarin.com/api/type/Android.Runtime.JNIEnv/), und alle anderen Grefs erstellt wurden, nicht kennen. Dies ist eine Heuristik *nur*. 

Wenn eine automatische Sammlung ausgeführt wird, wird eine Meldung ähnlich der folgenden in das Debugprotokoll ausgegeben:

```shell
I/monodroid-gc(PID): 46800 outstanding GREFs. Performing a full GC!
```

Das Vorhandensein dieses ist nicht deterministisch, und möglicherweise zu ungünstigen Zeitpunkten (z. B. in der Mitte Grafik-Rendering). Wenn diese Meldung wird angezeigt, Sie möglicherweise an anderer Stelle eine explizite Auflistung ausführen möchten oder Sie, versuchen Sie es möchten [reduzieren Sie die Lebensdauer der Objekte auf derselben Ebene](#Helping_the_GC). 

## <a name="gc-bridge-options"></a>GC-Bridge-Optionen

Xamarin.Android bietet transparente Speicherverwaltung mit Android und Android-Runtime. Es wird als eine Erweiterung der Mono Garbage Collector aufgerufen, implementiert die *GC-Bridge*. 

Der GC-Bridge arbeitet bei Mono Garbagecollection und Abbildungen, die Peer-Objekte benötigt ihre "Verfügbarkeitseigenschaften" mit dem Android-Runtime-Heap überprüft. Der GC-Bridge trifft diese Entscheidung auf, indem Sie die folgenden Schritte aus (in Reihenfolge):

1.  Auslösen von im mono bezugsdiagramm nicht erreichbar. Peer-Objekte in der Java-Objekte, die sie darstellen. 

2.  Führen Sie eine Java-GC.

3.  Überprüfen Sie, welche Objekte tatsächlich inaktiv sind. 

Dieser komplizierten Prozess ist, ermöglicht es, Unterklassen von `Java.Lang.Object` Objekte auf dem frei Verweis; keine Einschränkungen, die auf der Java Objekte können, um gebunden werden entfernt C#. Aufgrund dieser Komplexität der Bridge-Prozess kann sehr teuer sein, und kann dies zu einem spürbare Pausen in Anwendungen. Wenn die Anwendung erhebliche Pausen auftreten, ist es eine der folgenden drei GC-Bridge-Implementierungen zu untersuchen: 

-   **Tarjan** – ein vollständig neues Design der GC Bridge basierend auf [Robert Tarjans-Algorithmus und Abwärtskompatibilität verweisen Weitergabe](https://en.wikipedia.org/wiki/Tarjan's_strongly_connected_components_algorithm).
    Hat die beste Leistung unter unserem simulierten Workloads, aber auch über die größeren Anteil experimentelle Code verfügt. 

-   **Neue** -überarbeitet des ursprünglichen Codes, beheben zwei Instanzen des quadratischen Verhalten der Kernalgorithmus UserID-(basierend auf [Kosarajus Algorithmus](https://en.wikipedia.org/wiki/Kosaraju's_algorithm) suchen stark von den verbundenen Komponenten). 

-   **Alte** – die ursprüngliche Implementierung (als das stabilste der drei). Dies ist die Brücke, die eine Anwendung verwenden soll, wenn die `GC_BRIDGE` Pausen sind zulässig. 


Die einzige Möglichkeit, herauszufinden, welche GC-Bridge funktioniert am besten besteht in einer Anwendung experimentieren und Analysieren der Ausgabe. Es gibt zwei Möglichkeiten zum Erfassen der Daten für Vergleichstests: 

-   **Aktivieren der Protokollierung** -Protokollierung aktivieren (wie in beschrieben, die [Konfiguration](~/android/internals/garbage-collection.md) Abschnitt) dann für jede Option GC-Brücke zu erfassen, und vergleichen Sie die Log-Ausgaben von jeder Einstellung. Überprüfen Sie die `GC` Nachrichten für jede Option, vor allem die `GC_BRIDGE` Nachrichten. Hält bis zu 150ms für nichtinteraktive Anwendungen akzeptabel sind, aber die Pausen der oben 60ms für sehr interaktive Anwendungen (z. B. Spiele) ein Problem aufgetreten sind. 

-   **Bridge-Kontoführung aktivieren** -Bridge-Kontoführung zeigt die durchschnittliche Kosten der Objekte, die durch jedes Objekt, das die Brücke Prozess beteiligt. Diese Informationen nach Größe sortieren, bietet Hinweise, was die größte Menge zusätzlichen Objekte enthält. 


Um anzugeben, welche `GC_BRIDGE` Option eine Anwendung sollte, übergeben Sie `bridge-implementation=old`, `bridge-implementation=new` oder `bridge-implementation=tarjan` auf die `MONO_GC_PARAMS` Umgebungsvariable, z. B.: 

```shell
MONO_GC_PARAMS=bridge-implementation=tarjan
```

Die Standardeinstellung ist **Tarjan**. Wenn Sie eine Regression finden, Umständen ist es erforderlich, diese Option **alte**. Darüber hinaus können Sie mit der stabiler **alte** option, wenn **Tarjan** führt nicht zu einer Verbesserung der Leistung. 

<a name="Helping_the_GC" />

## <a name="helping-the-gc"></a>Unterstützung von der Garbage Collector

Es gibt mehrere Möglichkeiten zum die Freispeichersammlung alles für die Nutzung und Sammlung von Zeiten von Speicher zu reduzieren.



### <a name="disposing-of-peer-instances"></a>Freigeben von Peer-Instanzen

Der GC verfügt über eine unvollständige Sicht des Prozesses und kann möglicherweise nicht ausgeführt Arbeitsspeicher gering ist, da nicht, dass der Speicher ist die Freispeichersammlung bekannt ist niedrig. 

Z. B. eine Instanz von einer [Java.Lang.Object](https://developer.xamarin.com/api/type/Java.Lang.Object/) oder abgeleiteten Typ ist die Größe von mindestens 20 Bytes (vorbehalten ohne vorherige Ankündigung, usw., usw..). 
[Verwaltete Callable Wrapper](~/android/internals/architecture.md) fügen Sie keine zusätzlichen Instanzmember, also wenn eine [Android.Graphics.Bitmap](https://developer.xamarin.com/api/type/Android.Graphics.Bitmap/) -Instanz, die auf 10 MB Blob-Speicher verweist die Xamarin.Android-GC wird nicht kennen, die &ndash; der Garbage Collector sehen Sie ein 20-Byte-Objekt, und nicht feststellen, dass sie auf Android-Runtime zugeordnete Objekte, die 10 MB Speicher aktiv halten, ist verknüpft ist. 

Es ist häufig erforderlich, die der Garbage Collector zu unterstützen. Leider *GC. AddMemoryPressure()* und *GC. RemoveMemoryPressure()* werden nicht unterstützt, wenn also Sie *wissen* , dass ein großer Java zugeordnete Objektdiagramm gerade freigegeben wird, Sie möglicherweise manuell aufrufen müssen [GC. Collect()](xref:System.GC.Collect) an der Eingabeaufforderung eine GC freigeben die Java-Seite Arbeitsspeicher, oder Sie können explizit freigeben, *Java.Lang.Object* Unterklassen, unterbrechen die Zuordnung zwischen den verwalteten callable Wrapper und Java-Instanz. Beispielsweise finden Sie unter [Fehler 1084](http://bugzilla.xamarin.com/show_bug.cgi?id=1084#c6). 


> [!NOTE]
> Sie müssen sein *extrem* vorsichtig, wenn der disposing `Java.Lang.Object` Unterklasse-Instanzen.

Um die Möglichkeit, Speicherbeschädigung zu minimieren, beachten Sie die folgenden Richtlinien beim Aufrufen von `Dispose()`.


#### <a name="sharing-between-multiple-threads"></a>Datenfreigabe zwischen mehreren Threads

Wenn die *Java oder verwaltete* Instanz zwischen mehreren Threads gemeinsam genutzt werden kann *nicht `Dispose()`d*, **jemals**. Zum Beispiel [`Typeface.Create()`](https://developer.xamarin.com/api/member/Android.Graphics.Typeface.Create/(System.String%2cAndroid.Graphics.TypefaceStyle)) 
möglicherweise zurück, eine *zwischengespeicherte Instanz*. Wenn mehrere Threads die gleichen Argumente angeben, erhalten sie die *gleichen* Instanz. Folglich `Dispose()`-Verknüpfung von der `Typeface` Instanz von einem Thread möglicherweise andere Threads in kann dies ungültig `ArgumentException`aus `JNIEnv.CallVoidMethod()` (unter anderem), da die Instanz von einem anderen Thread freigegeben wurde. 


#### <a name="disposing-bound-java-types"></a>Freigeben von gebundenen Java-Typen

Wenn die Instanz von einer Java-Typ gebunden ist, kann die Instanz freigegeben werden, der *solange* die Instanz wird nicht von verwaltetem Code nicht wiederverwendet werden *und* Java-Instanz kann nicht für Threads (Siehe vorherige genutztwerden`Typeface.Create()` Diskussion). (Dieser Feststellung kann schwierig sein.) Das nächste Mal die Java-Instanz eingegeben verwalteter Code, eine *neue* Wrapper dafür erstellt werden. 

Dies ist häufig nützlich, bei zeichenbarer Ressourcen und andere Ressourcen beansprucht-Instanzen:

```csharp
using (var d = Drawable.CreateFromPath ("path/to/filename"))
    imageView.SetImageDrawable (d);
```

Das oben beschriebene sicher ist da dem Peer, [Drawable.CreateFromPath()](https://developer.xamarin.com/api/member/Android.Graphics.Drawables.Drawable.CreateFromPath/) zurückgegeben werden, die einen Peer Framework finden Sie unter *nicht* ein Peer für Benutzer. Die `Dispose()` rufen Sie am Ende der `using` Block unterbricht die Beziehung zwischen den verwalteten [Drawable](https://developer.xamarin.com/api/type/Android.Graphics.Drawables.Drawable/) und Framework [Drawable](https://developer.android.com/reference/android/graphics/drawable/Drawable.html) Instanzen, sodass die Java-Instanz sein gesammelt werden, sobald die Anforderungen der Android-Runtime an. Dies würde *nicht* sicher sind, wenn Sie Peer-Instanz, die ein Benutzer Peer bezeichnet werden; hier verwenden wir "external" Informationen zur *wissen* , die die `Drawable` kann nicht auf einem Peer Benutzer verweisen und somit die `Dispose()` aufrufen ist sicher. 


#### <a name="disposing-other-types"></a>Andere Typen freigeben 

Wenn die Instanz auf einen Typ verweist, die eine Bindung eines Java-Typs nicht (wie z. B. eine benutzerdefinierte `Activity`), **nicht** Aufrufen `Dispose()` es sei denn, Sie *wissen* , dass keine Java-Code auf, die überschriebene Methoden aufruft -Instanz. Führt bei unterlassen [ `NotSupportedException`s](~/android/internals/architecture.md#Premature_Dispose_Calls). 

Z. B. Wenn Sie eine benutzerdefinierte klicken Sie auf Listener:

```csharp
partial class MyClickListener : Java.Lang.Object, View.IOnClickListener {
    // ...
}
```

Sie *sollte nicht* verwerfen dieser Instanz, wie Java versucht, die in der Zukunft Aufrufen von Methoden auf:

```csharp
// BAD CODE; DO NOT USE
Button b = FindViewById<Button> (Resource.Id.myButton);
using (var listener = new MyClickListener ())
    b.SetOnClickListener (listener);
```


#### <a name="using-explicit-checks-to-avoid-exceptions"></a>Verwenden explizite Überprüfungen zum Vermeiden von Ausnahmen

Wenn Sie implementiert haben eine [Java.Lang.Object.Dispose](https://developer.xamarin.com/api/member/Java.Lang.Object.Dispose(System.Boolean)/) Überladungsmethode, vermeiden Sie die Objekte, bei denen JNI berühren. Erstellen Sie auf diese Weise kann eine *Double verwerfen* Situation, die Ihr Code (schwerwiegend) kann versuchen, auf ein zugrunde liegendes Java-Objekt zuzugreifen, die bereits der Garbage Collection stattgefunden hat. Auf diese Weise wird eine Ausnahme, die etwa wie folgt: 

```shell
System.ArgumentException: 'jobject' must not be IntPtr.Zero.
Parameter name: jobject
at Android.Runtime.JNIEnv.CallVoidMethod
```

Diese Situation tritt häufig bei der des ersten Dispose eines Objekts bewirkt, dass Mitglied zu null, und klicken Sie dann eine nachfolgende Zugriffsversuch auf diesem null-Element eine Ausnahme ausgelöst werden. Insbesondere des Objekts `Handle` auf des ersten Dispose ungültig ist (die eine verwaltete Instanz mit der zugrunde liegenden Java-Instanz verknüpft ist), aber verwalteter Code weiterhin versucht, auf diese zugrunde liegenden Java-Instanz zugreifen, obwohl es nicht mehr verfügbar (siehe ist[ Verwaltete Aufrufwrappern](~/android/internals/architecture.md#Managed_Callable_Wrappers) für Weitere Informationen zu die Zuordnung zwischen Java-Instanzen und verwaltete Instanzen). 

Überprüfen explizit in ist eine gute Möglichkeit, diese Ausnahme zu verhindern, dass Ihre `Dispose` -Methode, die die Zuordnung zwischen der verwalteten Instanz und der zugrunde liegenden Java-Instanz ist immer noch gültig ist, überprüfen, wenn des Objekts `Handle` ist null (`IntPtr.Zero`) vor dem Zugriff auf ihre Mitglieder. Beispielsweise die folgenden `Dispose` Methode greift auf eine `childViews` Objekt: 

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

Wenn eine anfängliche Dispose Ursachen übergeben `childViews` haben einen ungültigen `Handle`, `for` Schleife Zugriff löst eine `ArgumentException`. Durch das Hinzufügen einer expliziten `Handle` null-Überprüfung vor dem ersten `childViews` für den Zugriff auf die folgenden `Dispose` Methode verhindert, dass die Ausnahme, die auftreten: 

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


### <a name="reduce-referenced-instances"></a>Reduzieren Sie die Instanzen von auf die verwiesen wird

Wenn eine Instanz von eine `Java.Lang.Object` Typ oder Unterklasse wird überprüft, während die Garbage Collection, die gesamte *Objektdiagramm* , dass auf die Instanz bezieht auch überprüft werden muss. Das Objektdiagramm ist ein Satz von Objektinstanzen, die auf die Instanz"Root" *plus* alles verwiesen wird, durch welche die Stamminstanz verweist, rekursiv. 

Beachten Sie die folgende Klasse:

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

Wenn `BadActivity` wird erstellt, wird das Objektdiagramm 10004 Instanzen enthalten (1 X `BadActivity`, 1 X `strings`, 1 X `string[]` frei, die `strings`, 10000 x Zeichenfolgeninstanzen aus), *alle* von dem sein müssen überprüft jedes Mal, wenn die `BadActivity` Instanz wird überprüft. 

Dies haben nachteilige Auswirkungen auf die Auflistung auf Zeiten, wodurch speicherbereinigungspausenzeiten erhöht. 

Sie können die Freispeichersammlung von *reduzieren* die Größe von Objektdiagrammen, die von Peer-Benutzerinstanzen verankert sind. Im obigen Beispiel ist dies möglich durch das Verschieben `BadActivity.strings` in eine separate Klasse, die von Java.Lang.Object erben nicht: 

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

Kleinere Sammlungen können manuell ausgeführt werden, durch den Aufruf [GC. Collect(0)](xref:System.GC.Collect). Kleinere Sammlungen günstig (im Vergleich zu wichtigen Auflistungen) sind, aber verfügen über eine erhebliche Kosten "fixed", sodass Sie nicht diese oft auslösen möchten, und sollte eine Verzögerung von wenigen Millisekunden. 

Wenn Ihre Anwendung eine "Auslastung hat" in der immer wieder die gleiche Aufgabe abgeschlossen ist, ist es möglicherweise empfehlenswert, eine kleine Sammlung manuell ausführen, nachdem der Arbeitszyklus beendet wurde. Beispiel-Arbeitszyklen gehören: 

-  Des Renderingzyklus ein einzelnes Spiel Frame.
-  Die gesamte Interaktion mit einer bestimmten app-Dialogfeld ("Öffnen", "ausfüllen, schließen") 
-  Eine Gruppe von netzwerkanforderungen, um die Aktualisierung/app-Daten synchronisieren.



## <a name="major-collections"></a>Wichtige Sammlungen

Wichtige Sammlungen können manuell ausgeführt werden, durch den Aufruf [GC. Collect()](xref:System.GC.Collect) oder `GC.Collect(GC.MaxGeneration)`. 

Sie sollten nur selten ausgeführt werden und möglicherweise eine Verzögerung von einer Sekunde auf einem Android-Stil-Gerät, wenn einen Heap 512 MB zu sammeln. 

Wichtige Sammlungen sollte nur manuell, wenn überhaupt aufgerufen werden: 

-   Am Ende der langwierige Aufgabe wird nicht Zyklen und wenn eine lange anhalten, die dem Benutzer ein Problem darstellen. 

-   Einer überschriebenen [Android.App.Activity.OnLowMemory()](https://developer.xamarin.com/api/member/Android.App.Activity.OnLowMemory/) Methode. 



## <a name="diagnostics"></a>Diagnose

Um nachzuverfolgen, wenn globale Verweise erstellt und zerstört werden, Sie können festlegen, die [debug.mono.log](~/android/troubleshooting/index.md) Systemeigenschaft enthalten [ *Gref* ](~/android/troubleshooting/index.md) und/oder [ *gc*](~/android/troubleshooting/index.md). 



## <a name="configuration"></a>Konfiguration

Der Xamarin.Android-Garbagecollector kann konfiguriert werden, durch Festlegen der `MONO_GC_PARAMS` -Umgebungsvariablen angegeben. Umgebungsvariablen festgelegt werden können, mit einer Buildaktion von [AndroidEnvironment](~/android/deploy-test/environment.md).

Die `MONO_GC_PARAMS` Umgebungsvariable ist eine durch Trennzeichen getrennte Liste der folgenden Parameter: 

-   `nursery-size` = *Größe* : Legt die Größe der Nursery fest. Die Größe in Bytes angegeben ist, und es muss eine Potenz von zwei sein. Die Suffixe `k` , `m` und `g` können verwendet werden, um Kilogramm, Megapixel und Gigabyte, jeweils anzugeben. Der Nursery ist die erste Generation (zwei). Eine größere Nursery in der Regel beschleunigt das Programm aber natürlich beanspruchen mehr Arbeitsspeicher. Der Standard-Nursery Größe von 512 kb. 

-   `soft-heap-limit` = *Größe* : Die maximale Ziel verwaltet die arbeitsspeichernutzung für die app. Wenn der Speicher unter dem angegebenen Wert verwendet wird, ist der Garbage Collector für Ausführungszeit (weniger Sammlungen) optimiert. 
    Über diese Einschränkung ist der Garbage Collector für die arbeitsspeicherauslastung (Weitere Sammlungen) optimiert. 

-   `evacuation-threshold` = *Schwellenwert für* : Legt den Entfernungsvorgang Schwellenwert in Prozent fest. Der Wert muss eine ganze Zahl im Bereich von 0 bis 100 sein. Der Standardwert ist 66. Wenn die Sweeping-Phase der Auflistung gefunden wird, dass die Belegung der eines bestimmten Heap-Typs, die kleiner als dieser Prozentsatz ist, erfolgt er eine kopieren-Auflistung für diesen Blocktyp in der nächsten wichtigen-Auflistung, wodurch wiederherstellen Belegung, fast 100 % beträgt. Der Wert 0 deaktiviert die Auslagerung. 

-   `bridge-implementation` = *Überbrücken Sie die Implementierung* : Dadurch wird der GC-Bridge-Option aus, um die GC-Leistungsprobleme können festgelegt werden. Es gibt drei mögliche Werte: *alte* , *neue* , *Tarjan*.

-   `bridge-require-precise-merge`: Der Bridge eine Optimierung in seltenen Fällen ein Objekt enthält als verursachen, Tarjan erfasst eine GC nach wird es zunächst Garbage. Deaktiviert einschließlich diese Option, Optimierung, wodurch globale Kataloge jedoch möglicherweise langsamer, besser vorhersagbar.

Fügen Sie z. B. zum Konfigurieren der Garbage Collector, um eine Heap-größenbeschränkung von 128 MB haben eine neue Datei zu Ihrem Projekt mit einem **Buildvorgang** von `AndroidEnvironment` mit dem Inhalt: 

```shell
MONO_GC_PARAMS=soft-heap-limit=128m
```
