---
title: Garbage Collection
ms.prod: xamarin
ms.assetid: 298139E2-194F-4A58-BC2D-1D22231066C4
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/15/2018
ms.openlocfilehash: 7563dede151d2f72e9496d71041a0f590f0cf68e
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2019
ms.locfileid: "69524793"
---
# <a name="garbage-collection"></a>Garbage Collection

Xamarin. Android verwendet das [einfache Generations Garbage Collector](https://www.mono-project.com/docs/advanced/garbage-collector/sgen/)von Mono. Dabei handelt es sich um eine Markierungs-und Sweep-Garbage Collector mit zwei Generationen und einem *großen Objekt Raum*mit zwei Arten von Auflistungen: 

- Kleinere Sammlungen (sammelt gen0 Heap) 
- Hauptsammlungen (sammelt Gen1 und große Objekt Raum Heaps). 

> [!NOTE]
> Wenn keine explizite Sammlung über GC vorhanden ist [. Collect ()](xref:System.GC.Collect) -Auflistungen sind *auf Abruf*basierend auf Heap Zuordnungen. *Dabei handelt es sich nicht um ein Verweis zählungs System*. Objekte werden *nicht erfasst, sobald keine ausstehenden Verweise vorhanden sind*oder wenn ein Bereich beendet wurde. Der GC wird ausgeführt, wenn der kleinere Heap für neue Zuordnungen nicht über genügend Arbeitsspeicher verfügt. Wenn keine Zuordnungen vorhanden sind, wird der Vorgang nicht ausgeführt.


Kleinere Sammlungen sind günstig und häufig und werden verwendet, um kürzlich zugeordnete und unzustellbare Objekte zu erfassen. Kleinere Sammlungen werden nach jedem Paar MB zugeordneter Objekte ausgeführt. Kleinere Sammlungen können durch den Aufruf von GC manuell ausgeführt werden [. Erfassen (0)](/dotnet/api/system.gc.collect#System_GC_Collect_System_Int32_) 

Größere Auflistungen sind aufwendig und seltener und werden verwendet, um alle nicht aktiven Objekte freizugeben. Wichtige Sammlungen werden ausgeführt, sobald der Arbeitsspeicher für die aktuelle Heapgröße (vor der Größenänderung des Heaps) erschöpft ist. Wichtige Sammlungen können durch den Aufruf von GC manuell ausgeführt werden [. Collect ()](xref:System.GC.Collect) oder durch Aufrufen von [GC. Collect (int)](/dotnet/api/system.gc.collect#System_GC_Collect_System_Int32_) mit dem Argument [GC. Maxgene Ration](xref:System.GC.MaxGeneration). 



## <a name="cross-vm-object-collections"></a>Übergreifende Objekt Auflistungen

Es gibt drei Kategorien von Objekttypen.

- **Verwaltete Objekte**: Typen, die *nicht* von [java. lang. Object](xref:Java.Lang.Object) erben, z. b. [System. String](xref:System.String). 
    Diese werden normalerweise vom GC gesammelt. 

- **Java-Objekte**: Java-Typen, die auf dem virtuellen Android-Lauf Zeit Computer vorhanden sind, aber nicht für den Mono-VM verfügbar sind Diese sind langweilig und werden nicht weiter erläutert. Diese werden normalerweise von der Android-Lauf Zeit-VM gesammelt. 

- **Peer Objekte**: Typen, die [ijavaobject](xref:Android.Runtime.IJavaObject) implementieren, z. b. alle [java. lang. Object](xref:Java.Lang.Object) -und [java. lang. Throwable](xref:Java.Lang.Throwable) -Unterklassen. Instanzen dieser Typen haben zwei "Halfs", einen *verwalteten Peer* und einen *nativen Peer*. Der verwaltete Peer ist eine Instanz der- C# Klasse. Der Native Peer ist eine Instanz einer Java-Klasse innerhalb der Android-Lauf Zeit-VM C# , und die [ijavaobject. handle](xref:Android.Runtime.IJavaObject.Handle) -Eigenschaft enthält einen globalen jni-Verweis auf den nativen Peer. 


Es gibt zwei Typen von nativen Peers:

- Frameworkpeers: "Normale" Java-Typen, die nichts von xamarin. Android wissen, z. b.   [Android. Content. Context](xref:Android.Content.Context).

- **Benutzer Peers** :   [Android Callable Wrapper](~/android/platform/java-integration/working-with-jni.md) , die zur Buildzeit für jede Unterklasse von Java. lang. Object generiert werden, die in der Anwendung vorhanden ist.


Da in einem xamarin. Android-Prozess zwei virtuelle Computer vorhanden sind, gibt es zwei Arten von Garbage Collections:

- Android-Lauf Zeit Sammlungen 
- Mono-Sammlungen 

Android-Lauf Zeit Sammlungen funktionieren normal, aber mit einem Nachteil: ein globaler jni-Verweis wird als GC-Stamm behandelt. Folglich kann das Objekt nicht gesammelt werden, wenn es einen globalen Verweis auf ein Android-Lauf Zeit-VM-Objekt enthält. Dies ist auch dann *nicht* möglich, wenn es für die Sammlung geeignet ist.

Mono-Auflistungen sind, wo der Spaß passiert Verwaltete Objekte werden normal gesammelt. Peer Objekte werden gesammelt, indem der folgende Vorgang durchgeführt wird:

1. Für alle Peer Objekte, die für die Mono-Sammlung qualifiziert sind, wird der globale jni-Verweis durch einen globalen jni-Verweis ersetzt. 

2. Eine Android-Runtime-VM-GC wird aufgerufen. Es kann eine beliebige Native Peer Instanz gesammelt werden. 

3. Die in (1) erstellten globalen jni-Verweise werden geprüft. Wenn der schwache Verweis gesammelt wurde, wird das Peer Objekt gesammelt. Wenn der schwache Verweis *nicht* erfasst wurde, wird der schwache Verweis durch einen globalen jni-Verweis ersetzt, und das Peer Objekt wird nicht erfasst. Hinweis: bei API 14 + bedeutet dies, dass sich der Wert, `IJavaObject.Handle` der von zurückgegeben wird, nach einem GC ändern kann. 

Das Endergebnis all dies besteht darin, dass eine Instanz eines Peer Objekts so lange aktiv ist, wie entweder von verwaltetem Code (z. b. in einer `static` Variablen gespeichert) oder durch Java-Code darauf verwiesen wird. Darüber hinaus wird die Lebensdauer von nativen Peers über das, was Sie andernfalls Leben, hinaus erweitert, da der Native Peer nicht entladbar ist, bis der Native Peer und der verwaltete Peer entladbar sind.


## <a name="object-cycles"></a>Objekt Zyklen

Peer Objekte sind logisch in der Android-Laufzeit und in Mono-VMS vorhanden. Beispielsweise verfügt eine verwaltete Peer-Instanz von [Android. app. Activity](xref:Android.App.Activity) über eine entsprechende Java-Instanz des [Android. app. Activity](https://developer.android.com/reference/android/app/Activity.html) -Framework-Peers. Alle Objekte, die von [java. lang. Object](xref:Java.Lang.Object) erben, können in beiden virtuellen Computern über Darstellungen verfügen. 

Alle Objekte, die auf beiden virtuellen Computern über eine Darstellung verfügen, verfügen Überlebens Dauer, die im Vergleich zu Objekten erweitert werden, die nur innerhalb einer [`System.Collections.Generic.List<int>`](xref:System.Collections.Generic.List%601)einzelnen VM (z. b.) vorhanden sind. GC wird aufgerufen [. Collect](xref:System.GC.Collect) sammelt diese Objekte nicht notwendigerweise, da die xamarin. Android-GC sicherstellen muss, dass auf das Objekt nicht von einem der beiden virtuellen Computer verwiesen wird, bevor Sie es sammeln. 

Zum Verkürzen der Objekt Lebensdauer sollte [java. lang. Object. verwerfen ()](xref:Java.Lang.Object.Dispose) aufgerufen werden. Dadurch wird die Verbindung des Objekts zwischen den beiden VMs manuell "abgerufen", indem der globale Verweis freigegeben wird, sodass die Objekte schneller gesammelt werden können. 


## <a name="automatic-collections"></a>Automatische Auflistungen

Ab [Release 4.1.0](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/android/mono_for_android_4/mono_for_android_4.1.0/index.md)führt xamarin. Android automatisch eine vollständige GC aus, wenn ein Gref-Schwellenwert überschritten wird. Dieser Schwellenwert ist 90% der bekannten maximalen Grefs für die Plattform: 1800 Grefs auf dem Emulator (2000 max) und 46800 Grefs auf der Hardware (maximal 52000). *Hinweis*: Xamarin. Android zählt nur die von [Android. Runtime. jnienv](xref:Android.Runtime.JNIEnv)erstellten Grefs und weiß nicht, welche anderen im Prozess erstellten Grefs verwendet werden. Dies ist *nur*eine Heuristik. 

Wenn eine automatische Sammlung durchgeführt wird, wird eine Meldung ähnlich der folgenden in das Debugprotokoll ausgegeben:

```shell
I/monodroid-gc(PID): 46800 outstanding GREFs. Performing a full GC!
```

Das Vorkommen von ist nicht deterministisch und kann zu nicht geeigneten Zeiten auftreten (z. b. in der Mitte des Grafik Rendering). Wenn diese Meldung angezeigt wird, können Sie an anderer Stelle eine explizite Auflistung ausführen, oder Sie möchten versuchen, [die Lebensdauer von Peer Objekten zu verringern](#Helping_the_GC). 

## <a name="gc-bridge-options"></a>Optionen für GC-Bridge

Xamarin. Android bietet transparente Speicherverwaltung mit Android und der Android-Laufzeit. Sie wird als Erweiterung des Mono-Garbage Collector implementiert, der als *GC-Bridge*bezeichnet wird. 

Die GC-Bridge funktioniert bei einem Mono-Garbage Collection und gibt an, welche Peer Objekte ihre "Elastizität" mit dem Android-Lauf Zeit Heap verifizieren müssen. Die GC-Bridge führt diese Bestimmung durch die folgenden Schritte aus (in der angegebenen Reihenfolge):

1. Auslösen des Mono-Referenz Diagramms von nicht erreichbaren Peer Objekten in die Java-Objekte, die Sie darstellen. 

2. Ausführen einer Java-GC

3. Überprüfen Sie, welche Objekte wirklich unaktiv sind. 

Dieser komplizierte Prozess ermöglicht es Unterklassen von `Java.Lang.Object` , frei auf beliebige Objekte zu verweisen C#. Dadurch werden alle Einschränkungen entfernt, an die Java-Objekte gebunden werden können. Aufgrund dieser Komplexität kann der Brücken Prozess sehr kostspielig sein und spürbare Pausen in einer Anwendung verursachen. Wenn die Anwendung bedeutende Pausen hat, ist es sinnvoll, eine der folgenden drei GC-Brücken Implementierungen zu untersuchen: 

- **Tarjan** : ein vollständig neuer Entwurf der GC-Bridge basierend auf [dem Algorithmus von Robert Tarjan und der rückwärts Verweis](https://en.wikipedia.org/wiki/Tarjan's_strongly_connected_components_algorithm)Weitergabe.
    Dies hat die beste Leistung bei unseren simulierten Workloads, verfügt aber auch über den größeren Anteil des experimentellen Codes. 

- **Neu** : eine wichtige Überprüfung des ursprünglichen Codes, die zwei Instanzen des quadratischen Verhaltens repariert, aber den Kern Algorithmus beibehalten (basierend auf [dem Algorithmus von kosaraju für die](https://en.wikipedia.org/wiki/Kosaraju's_algorithm) Suche nach stark verbundenen Komponenten). 

- **Old** : die ursprüngliche Implementierung (als die stabilste der drei). Dies ist die Bridge, die eine Anwendung verwenden sollte, `GC_BRIDGE` wenn die Pausen akzeptabel sind. 


Die einzige Möglichkeit, herauszufinden, welche GC-Brücke am besten funktioniert, besteht darin, eine Anwendung zu experimentieren und die Ausgabe zu analysieren. Es gibt zwei Möglichkeiten, die Daten für das Benchmarkverfahren zu erfassen: 

- **Aktivieren Sie die Protokollierung** : Aktivieren Sie die Protokollierung (wie im [Konfigurations](~/android/internals/garbage-collection.md) Abschnitt beschrieben) für jede GC-Bridge-Option, und erfassen und vergleichen Sie dann die Protokoll Ausgaben der einzelnen Einstellungen. Überprüfen `GC` Sie die Nachrichten für die einzelnen Optionen, `GC_BRIDGE` insbesondere für die Nachrichten. Hält bis zu 150 ms für nicht interaktive Anwendungen an, ist aber für sehr interaktive Anwendungen (z. b. Spiele) ein Problem. 

- **Bridge Accounting aktivieren** : bei Bridge Accounting werden die durchschnittlichen Kosten der Objekte angezeigt, die von den einzelnen auf den Bridge Prozess beteiligten Objekten verwiesen werden. Das Sortieren dieser Informationen nach Größe bietet Hinweise darauf, was die größte Menge an zusätzlichen Objekten enthält. 


So geben Sie `GC_BRIDGE` die Option an, die eine Anwendung `bridge-implementation=old`uns `bridge-implementation=new` , `bridge-implementation=tarjan` übergeben oder `MONO_GC_PARAMS` an die Umgebungsvariable übergeben soll, z. b.: 

```shell
MONO_GC_PARAMS=bridge-implementation=tarjan
```

Die Standardeinstellung ist **Tarjan**. Wenn eine Regression gefunden wird, ist es möglicherweise erforderlich, diese Option auf " **alt**" festzulegen. Sie können auch die stabilere **alte** Option verwenden, wenn **Tarjan** keine Verbesserung der Leistung erzielt. 

<a name="Helping_the_GC" />

## <a name="helping-the-gc"></a>Unterstützung der GC

Es gibt mehrere Möglichkeiten, die GC zu unterstützen, um Speicherauslastung und Sammlungs Zeiten zu reduzieren.



### <a name="disposing-of-peer-instances"></a>Verwerfen von Peer Instanzen

Der GC verfügt über eine unvollständige Ansicht des Prozesses und wird möglicherweise nicht ausgeführt, wenn der Arbeitsspeicher gering ist, da der GC nicht weiß, dass der Arbeitsspeicher gering ist. 

Eine Instanz eines [java. lang. Object](xref:Java.Lang.Object) -Typs oder eines abgeleiteten Typs hat z. b. eine Größe von mindestens 20 Bytes (vorbehaltlich der Änderung ohne vorherige Ankündigung usw.). 
[Verwaltete Callable Wrapper](~/android/internals/architecture.md) fügen keine zusätzlichen Instanzmember hinzu. Wenn Sie also über eine [Android. Graphics. Bitmap](xref:Android.Graphics.Bitmap) -Instanz verfügen, die auf ein 10 MB-BLOB des Arbeitsspeichers verweist, weiß die GC von xamarin. Android nicht, dass &ndash; der GC ein 20-Byte-Objekt sehen wird und kann nicht feststellen, ob es mit von Android-Runtime zugewiesenen Objekten verknüpft ist, die 10 MB Arbeitsspeicher aufrechterhalten. 

Häufig ist es notwendig, die GC zu unterstützen. Leider *GC. AddMemoryPressure ()* und *GC. Removememoryprint ()* wird nicht unterstützt. Wenn Sie also *wissen* , dass Sie gerade ein großes, von Java zugeordneter Objekt Graph freigegeben haben, müssen Sie möglicherweise GC manuell abrufen [. Erfassen ()](xref:System.GC.Collect) , um eine GC zum Freigeben des Java-basierten Speichers aufzufordern, oder Sie können explizit *java. lang. Object* -Unterklassen verwerfen und die Zuordnung zwischen dem verwalteten Aufruf baren Wrapper und der Java-Instanz unterbrechen. Informationen hierzu finden Sie beispielsweise in [Bug 1084](http://bugzilla.xamarin.com/show_bug.cgi?id=1084#c6). 


> [!NOTE]
> Beim Verwerfen von `Java.Lang.Object` Unterklassen Instanzen müssen Sie sehr vorsichtig sein.

Beachten Sie beim Aufrufen `Dispose()`von die folgenden Richtlinien, um die Möglichkeit einer Speicher Beschädigung zu minimieren.


#### <a name="sharing-between-multiple-threads"></a>Freigabe zwischen mehreren Threads

Wenn die *Java* -Instanz oder die verwaltete Instanz von mehreren Threads gemeinsam genutzt werden kann, sollte Sie **niemals** *d (d) `Dispose()`sein*. Zum Beispiel[`Typeface.Create()`](xref:Android.Graphics.Typeface.Create*) 
gibt möglicherweise eine *zwischengespeicherte-Instanz*zurück. Wenn mehrere Threads dieselben Argumente bereitstellen, erhalten Sie *dieselbe* Instanz. Folglich kann das `Typeface` `ArgumentException`Löschen der-Instanz von einem Thread andere Threads ungültig machen, was zu s von `JNIEnv.CallVoidMethod()` (u. a.) führen kann, da die Instanz von einem anderen Thread verworfen wurde. `Dispose()` 


#### <a name="disposing-bound-java-types"></a>Verwerfen von gebundenen Java-Typen

Wenn die Instanz einen gebundenen Java-Typ hat, kann die Instanz verworfen werden, solange die Instanz nicht aus verwaltetem Code wieder verwendet wird *und* die Java-Instanz nicht von Threads gemeinsam genutzt werden kann `Typeface.Create()` (siehe vorherige Erörterung). (Diese Bestimmung ist möglicherweise schwierig.) Wenn die Java-Instanz das nächste Mal in verwalteten Code gelangt, wird ein *neuer* Wrapper für Sie erstellt. 

Dies ist häufig nützlich, wenn es um drawables und andere ressourcenintensive Instanzen geht:

```csharp
using (var d = Drawable.CreateFromPath ("path/to/filename"))
    imageView.SetImageDrawable (d);
```

Der obige Code ist sicher, weil der von [drawable. kreatefrompath ()](xref:Android.Graphics.Drawables.Drawable.CreateFromPath*) zurückgegebene Peer auf einen frameworkpeer verweist, *nicht* auf einen benutzerpeer. Der `Dispose()`-Aufruf am Ende des `using`-Blocks unterbricht die Beziehung zwischen den verwalteten [Drawable](xref:Android.Graphics.Drawables.Drawable)-Instanzen und [Drawable](https://developer.android.com/reference/android/graphics/drawable/Drawable.html)-Frameworkinstanzen, sodass die Java-Instanz erfasst werden kann, sobald die Android-Laufzeit dies benötigt. Dies wäre *nicht* sicher, wenn Peer Instanz an einen benutzerpeer verwiesen wird. Hier verwenden wir "externe" Informationen, um zu *wissen* , `Drawable` dass nicht auf einen benutzerpeer verweisen kann, `Dispose()` und daher ist der-Rückruf sicher. 


#### <a name="disposing-other-types"></a>Verwerfen anderer Typen 

Wenn die Instanz auf einen Typ verweist, der keine Bindung eines Java-Typs ist (z. b `Activity`. ein benutzerdefiniertes `Dispose()` ), sollten Sie **nicht** aufzurufen, wenn Sie nicht *wissen* , dass der Java-Code überschriebene Methoden für diese Instanz aufruft. Wenn dies nicht der [ `NotSupportedException`](~/android/internals/architecture.md#Premature_Dispose_Calls)Fall ist, führt dies zu einer. 

Wenn Sie z. b. einen benutzerdefinierten klicklistener haben:

```csharp
partial class MyClickListener : Java.Lang.Object, View.IOnClickListener {
    // ...
}
```

Sie *sollten* diese Instanz nicht verwerfen, da Java in Zukunft versucht, Methoden dafür aufzurufen:

```csharp
// BAD CODE; DO NOT USE
Button b = FindViewById<Button> (Resource.Id.myButton);
using (var listener = new MyClickListener ())
    b.SetOnClickListener (listener);
```


#### <a name="using-explicit-checks-to-avoid-exceptions"></a>Verwenden expliziter Überprüfungen zur Vermeidung von Ausnahmen

Wenn Sie eine [java. lang. Object.](xref:Java.Lang.Object.Dispose*) verworfen-Überladungs Methode implementiert haben, vermeiden Sie das Berühren von Objekten, die jni betreffen. Dies kann zu einer *doppelten* Lösch Situation führen, die es Ihrem Code ermöglicht (Fatal), auf ein zugrunde liegendes Java-Objekt zuzugreifen, für das bereits eine Garbage Collection durchgeführt wurde. Dadurch wird eine Ausnahme ähnlich der folgenden erzeugt: 

```shell
System.ArgumentException: 'jobject' must not be IntPtr.Zero.
Parameter name: jobject
at Android.Runtime.JNIEnv.CallVoidMethod
```

Diese Situation tritt häufig auf, wenn der erste Löschvorgang eines-Objekts bewirkt, dass ein Member NULL wird, und ein nachfolgende Zugriffs Versuch für dieses NULL-Element bewirkt, dass eine Ausnahme ausgelöst wird. Insbesondere das- `Handle` Objekt (das eine verwaltete Instanz mit der zugrunde liegenden Java-Instanz verknüpft) wird beim ersten verwerfen ungültig, aber verwalteter Code versucht weiterhin, auf diese zugrunde liegende Java-Instanz zuzugreifen, auch wenn Sie nicht mehr verfügbar ist (siehe [). Verwaltete Aufruf Bare Wrapper](~/android/internals/architecture.md#Managed_Callable_Wrappers) für weitere Informationen zur Zuordnung zwischen Java-Instanzen und verwalteten Instanzen). 

Eine gute Möglichkeit, diese Ausnahme zu verhindern, besteht darin, in `Dispose` der-Methode explizit zu überprüfen, ob die Zuordnung zwischen der verwalteten Instanz und der zugrunde liegenden Java-Instanz noch gültig ist. das `Handle` heißt, ob`IntPtr.Zero`das Objekt den Wert NULL () hat. vor dem Zugriff auf die Member. Beispielsweise greift die folgende `Dispose` Methode auf ein `childViews` -Objekt zu: 

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

Wenn ein anfänglicher Lösch Durchlauf `childViews` bewirkt, dass einen `Handle`ungültigen `for` hat, löst der Schleifen `ArgumentException`Zugriff einen aus. Durch das Hinzufügen `Handle` einer expliziten NULL- `childViews` Überprüfung vor dem `Dispose` ersten Zugriff verhindert die folgende Methode, dass die Ausnahme auftritt: 

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


### <a name="reduce-referenced-instances"></a>Reduzieren der referenzierten Instanzen

Wenn eine Instanz eines Typs `Java.Lang.Object` oder einer Unterklasse während der GC gescannt wird, muss das gesamte *Objekt Diagramm* , auf das die Instanz verweist, ebenfalls gescannt werden. Das Objekt Diagramm ist der Satz von Objektinstanzen, auf die sich die Stamm Instanz bezieht, *sowie* alle Elemente, auf die die Stamm Instanz rekursiv verweist. 

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

Wenn `BadActivity` erstellt wird, enthält das Objekt Diagramm 10004-Instanzen (1 x `BadActivity`, 1X `strings`, 1X `string[]` , gehalten `strings`von, 10000 x Zeichen folgen Instanzen), die *alle* gescannt werden müssen, wenn die `BadActivity` die Instanz wird gescannt. 

Dies kann nachteilige Auswirkungen auf die Sammlungs Zeiten haben, was zu längeren GC-Pausenzeiten führt. 

Sie können den GC unterstützen, indem Sie die Größe von Objekt Diagrammen *verringern* , die von Benutzer Peer Instanzen stammen. Im obigen Beispiel kann dies durch einen Wechsel `BadActivity.strings` zu einer separaten Klasse erfolgen, die nicht von Java. lang. Object erbt: 

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

Kleinere Sammlungen können durch den Aufruf von GC manuell ausgeführt werden [. Erfassen (0)](xref:System.GC.Collect). Kleinere Sammlungen sind im Vergleich zu großen Auflistungen kostengünstig, haben aber einen erheblichen fixaufwand, sodass Sie Sie nicht zu oft auslöst und eine Pause von einigen Millisekunden aufweisen sollten. 

Wenn die Anwendung über einen "Duty-Cycle" verfügt, in dem dasselbe passiert ist, kann es ratsam sein, eine kleine Sammlung manuell auszuführen, sobald der Duty-Vorgang beendet wurde. Beispiel für Duty Cycles: 

- Der renderingcycle eines einzelnen Spiel Rahmens.
- Die gesamte Interaktion mit einem bestimmten app-Dialog (öffnen, ausfüllen, schließen) 
- Eine Gruppe von Netzwerk Anforderungen zum Aktualisieren/Synchronisieren von App-Daten.



## <a name="major-collections"></a>Hauptsammlungen

Wichtige Sammlungen können durch den Aufruf von GC manuell ausgeführt werden [. Collect ()](xref:System.GC.Collect) oder `GC.Collect(GC.MaxGeneration)`. 

Sie sollten nur selten ausgeführt werden und können eine Pause von einer Sekunde auf einem Android-Gerät sein, wenn Sie einen Heap mit 512 MB sammeln. 

Wichtige Sammlungen sollten nur manuell aufgerufen werden: 

- Am Ende von langwierigen Zeitzyklen und wenn eine lange Pause dem Benutzer kein Problem präsentiert. 

- Innerhalb einer überschriebenen [Android. app. Activity. onlowmemory ()](xref:Android.App.Activity.OnLowMemory) -Methode. 



## <a name="diagnostics"></a>Diagnose

Wenn Sie nachverfolgen möchten, wann globale Verweise erstellt und zerstört werden, können Sie die System Eigenschaft [Debug. Mono. log](~/android/troubleshooting/index.md) auf [*Gref*](~/android/troubleshooting/index.md) und/oder [*GC*](~/android/troubleshooting/index.md)festlegen. 



## <a name="configuration"></a>Konfiguration

Die xamarin. Android-Garbage Collector kann durch Festlegen der `MONO_GC_PARAMS` Umgebungsvariablen konfiguriert werden. Umgebungsvariablen können mit der Buildaktion " [androidenvironment](~/android/deploy-test/environment.md)" festgelegt werden.

Die `MONO_GC_PARAMS` Umgebungsvariable ist eine durch Trennzeichen getrennte Liste mit den folgenden Parametern: 

- `nursery-size` = *Größe* : Legt die Größe der Gärtnerei fest. Die Größe wird in Bytes angegeben und muss eine Potenz von zwei sein. Die Suffixe `k` `m` und`g` können verwendet werden, um Kilo-, Mega-bzw. Gigabyte anzugeben. Der Kindergarten ist die erste Generation (von zwei). Ein größerer Kindergarten beschleunigt das Programm in der Regel, aber es wird offensichtlich mehr Arbeitsspeicher verbrauchen. Die Standardgröße für die Groß-und 512 KB. 

- `soft-heap-limit` = *Größe* : Der für die APP maximal zulässige verwaltete Speicherverbrauch. Wenn die Speicherauslastung unter dem angegebenen Wert liegt, wird der GC für die Ausführungszeit (weniger Auflistungen) optimiert. 
    Oberhalb dieses Limits ist die GC für die Speicherauslastung optimiert (weitere Sammlungen). 

- `evacuation-threshold` = *Schwellenwert* : Legt den Schwellenwert für die Evakuierung in Prozent fest. Der Wert muss eine ganze Zahl im Bereich zwischen 0 und 100 sein. Der Standardwert ist 66. Wenn die Sweep-Phase der Auflistung feststellt, dass die Belegung eines bestimmten Heap Block Typs kleiner als dieser Prozentsatz ist, wird in der nächsten Haupt Auflistung eine Kopier Auflistung für diesen Blocktyp durchführt, sodass die Belegung auf 100 Prozent wieder hergestellt wird. Bei einem Wert von 0 wird die Evakuierung deaktiviert. 

- `bridge-implementation` = *Bridge Implementierung* : Dadurch wird die Option GC Bridge festgelegt, um GC-Leistungsprobleme zu beheben. Es gibt drei mögliche Werte: *alt* , *neu* , *Tarjan*.

- `bridge-require-precise-merge`: Die Tarjan-Bridge enthält eine Optimierung, die in seltenen Fällen dazu führen kann, dass ein Objekt nach dem ersten Garbage Collector eine Garbage Collection erfasst. Wenn Sie diese Option einschließen, wird diese Optimierung deaktiviert, sodass die GCS vorhersag barer, aber potenziell langsamer werden.

Wenn Sie z. b. für die GC eine Heapgröße von 128 MB konfigurieren möchten, fügen Sie dem Projekt eine neue Datei mit der Buildaktion `AndroidEnvironment` mit dem Inhalt hinzu: 

```shell
MONO_GC_PARAMS=soft-heap-limit=128m
```
