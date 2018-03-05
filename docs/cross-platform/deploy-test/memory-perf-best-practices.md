---
title: "Plattformübergreifende Leistung"
description: "Sie haben verschiedene Möglichkeiten, die Leistung von Anwendungen zu verbessern, die mit Xamarin.Android erstellt wurden. Wenn Sie diese Kniffe kombinieren, können Sie die CPU-Auslastung und die Speichermenge, die von einer Anwendung verwendet wird, erheblich reduzieren. In diesem Artikel werden die Techniken beschrieben und erläutert."
ms.topic: article
ms.prod: xamarin
ms.assetid: 9ce61f18-22ac-4b93-91be-5b499677d661
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/24/2017
ms.openlocfilehash: 56d868f64de009d01930ec34ee2cb436276006ef
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="cross-platform-performance"></a>Plattformübergreifende Leistung

_Sie haben verschiedene Möglichkeiten, die Leistung von Anwendungen zu verbessern, die mit der Xamarin-Plattform erstellt wurden. Wenn Sie diese Kniffe kombinieren, können Sie die CPU-Auslastung und die Speichermenge, die von einer Anwendung verwendet wird, erheblich reduzieren. In diesem Artikel wird Folgendes erläutert._

Eine schlechte Anwendungsleistung kann sich auf unterschiedliche Weise bemerkbar machen. Die Anwendung reagiert scheinbar nicht mehr, der Bildlauf ist möglicherweise verlangsamt, und auch die Akkulaufzeit kann abnehmen. Leistungsoptimierung umfasst jedoch mehr als das bloße Implementieren eines effizienten Codes. Es muss ebenfalls berücksichtigt werden, wie der Benutzer die Leistung der Anwendung wahrnimmt. Wenn laufende Vorgänge Benutzer beispielsweise nicht an anderen Aktivitäten hindern, kann dies die Benutzerfreundlichkeit verbessern.

Die (wahrgenommene) Leistung von Anwendungen, die mit Xamarin.Android erstellt wurden, lässt sich auf unterschiedliche Arten und Weisen steigern. Dazu zählen:

- [Verwenden des Profilers](#profiler)
- [Freigeben von IDisposable-Ressourcen](#idisposable)
- [Abbestellen von Ereignisabonnements](#events)
- [Verwenden schwacher Verweise, um speicherresidente Objekte zu vermeiden](#weakreferences)
- [Verzögern der Kosten der Objekterstellung](#lazy)
- [Implementieren von asynchronen Vorgängen](#async)
- [Verwenden des SGen-Garbage Collectors](#sgen)
- [Reduzieren der Anwendungsgröße](#linker)
- [Optimieren von Bildressourcen](#optimizeimages)
- [Reduzieren des Aktivierungszeitraums der Anwendung](#activationperiod)
- [Reduzieren der Kommunikation des Webdiensts](#webservicecommunication)

In diesem [Video der Xamarin University](https://university.xamarin.com/guestlectures/avoiding-common-pitfalls-in-xamarin-apps) finden Sie weitere hilfreiche Tipps für das Entwerfen von Xamarin-Apps.

[ ![](memory-perf-best-practices-images/clancey-sml.png "Kostenloses Xamarin University-Video zum Vermeiden gängiger Fehlerquellen")](https://university.xamarin.com/guestlectures/avoiding-common-pitfalls-in-xamarin-apps)

<a name="profiler" />

## <a name="use-the-profiler"></a>Verwenden des Profilers

Beim Entwickeln einer Anwendung sollte eine Optimierung des Codes erst nach dem Profiling versucht werden. Beim Profiling wird bestimmt, in welchen Bereichen Codeoptimierungen am wirkungsvollsten Leistungsprobleme beheben. Der Profiler analysiert die Speichernutzung der Anwendung und zeichnet die Ausführungszeit von Methoden in der Anwendung auf. Die Daten helfen bei der Navigation durch die Ausführungspfade der Anwendung und die Ausführungskosten des Codes. So kann ermittelt werden, wie die Leistung am besten optimiert werden kann.

Der Xamarin Profiler misst, evaluiert und ermittelt leistungsbedingte Probleme in der Anwendung. Außerdem kann damit in Visual Studio für Mac oder Visual Studio ein Profil für Xamarin.iOS- und Xamarin.Android-Anwendungen erstellt werden. Weitere Informationen zum Xamarin Profiler finden Sie in der [Einführung in den Xamarin Profiler](~/tools/profiler/index.md).

Die folgenden Best Practices werden für das Profiling einer App empfohlen:

- Vermeiden Sie es, eine Anwendung in einem Simulator zu profilen, da der Simulator die Leistung der Anwendung beeinträchtigen kann.
- Das Profiling sollte im Idealfall auf einer Vielzahl von Geräten ausgeführt werden, da Leistungsmessungen auf einem Gerät nicht unbedingt die Leistungsmerkmale auf anderen Geräten widerspiegeln. Ist dies nicht möglich, sollte das Profiling zumindest auf einem Gerät ausgeführt werden, das die niedrigste, erwartete Spezifikation aufweist.
- Schließen Sie alle anderen Anwendungen. So stellen Sie sicher, dass nur die Auswirkungen der Anwendung, für die ein Profil erstellt wurde, und keiner anderen Anwendung gemessen werden.

<a name="idisposable" />

## <a name="release-idisposable-resources"></a>Freigeben von IDisposable-Ressourcen

Die `IDisposable`-Schnittstelle stellt einen Mechanismus zum Freigeben von Ressourcen und eine `Dispose`-Methode bereit, die zum expliziten Freigeben von Ressourcen implementiert werden sollte. `IDisposable` ist kein Destruktor und sollte nur unter den folgenden Umständen implementiert werden:

- Wenn die Klasse nicht verwaltete Ressourcen besitzt. Typische, nicht verwaltete Ressourcen, die eine Freigabe erfordern, sind Dateien, Datenströme und Netzwerkverbindungen.
- Wenn die Klasse nicht verwaltete `IDisposable`-Ressourcen besitzt.

Typconsumer können anschließend die `IDisposable.Dispose`-Implementierung aufrufen, um Ressourcen freizugeben, wenn die Instanz nicht mehr benötigt wird. Dies erreichen Sie auf zweierlei Art und Weise:

- Durch Umschließen des `IDisposable`-Objekt in einer `using`-Anweisung
- Durch Umschließen des Aufrufs von `IDisposable.Dispose` in einem `try`/`finally`-Block

### <a name="wrapping-the-idisposable-object-in-a-using-statement"></a>Umschließen des IDisposable-Objekts in einer using-Anweisung

Im folgenden Codebeispiel wird veranschaulicht, wie ein `IDisposable`-Objekt in einer `using`-Anweisung umschlossen wird:

```csharp
public void ReadText (string filename)
{
  ...
  string text;
  using (StreamReader reader = new StreamReader (filename)) {
    text = reader.ReadToEnd ();
  }
  ...
}
```

Die `StreamReader`-Klasse implementiert `IDisposable`, und die `using`-Anweisung stellt eine praktische Syntax bereit, die die Methode `StreamReader.Dispose` für das `StreamReader`-Objekt aufruft, bevor es den Bereich verlässt. Innerhalb des `using`-Blocks ist das `StreamReader`-Objekt schreibgeschützt und kann nicht neu zugewiesen werden. Die `using`-Anweisung stellt auch sicher, dass die `Dispose`-Methode selbst dann aufgerufen wird, wenn der Compiler die Zwischensprache (Intermediate Language, IL) für einen `try`/`finally`-Block implementiert und dabei eine Ausnahme auftritt.

### <a name="wrapping-the-call-to-idisposabledispose-in-a-tryfinally-block"></a>Umschließen des Aufrufs von IDisposable.Dispose in einem Try/Finally-Block

Im folgenden Codebeispiel wird veranschaulicht, wie der Aufruf von `IDisposable.Dispose` in einem `try`/`finally`-Block umschlossen wird:

```csharp
public void ReadText (string filename)
{
  ...
  string text;
  StreamReader reader = null;
  try {
    reader = new StreamReader (filename);
    text = reader.ReadToEnd ();
  } finally {
    if (reader != null) {
      reader.Dispose ();
    }
  }
  ...
}
```

Die `StreamReader`-Klasse implementiert `IDisposable`, und der `finally`-Block ruft die Methode `StreamReader.Dispose` auf, um die Ressource freizugeben.

Weitere Informationen finden Sie unter [System.IDisposable](https://developer.xamarin.com/api/type/System.IDisposable/).

<a name="events" />

## <a name="unsubscribe-from-events"></a>Abbestellen von Ereignisabonnements

Um Speicherverluste zu verhindern, sollten Ereignisabonnements abbestellt werden, bevor das abonnierende Objekt verworfen wird. Bis zur Abbestellung verweist der Delegat des Ereignisses im Veröffentlichungsobjekt auf einen Delegaten, der den Ereignishandler des Abonnenten einkapselt. Solange das Veröffentlichungsobjekt diesen Verweis enthält, wird die Garbage Collection den vom abonnierenden Objekt genutzten Speicher nicht freigeben.

Im folgenden Codebeispiel wird dargestellt, wie ein Ereignisabonnement abbestellt wird:

```csharp
public class Publisher
{
  public event EventHandler MyEvent;

  public void OnMyEventFires ()
  {
    if (MyEvent != null) {
      MyEvent (this, EventArgs.Empty);
    }
  }
}

public class Subscriber : IDisposable
{
  readonly Publisher publisher;

  public Subscriber (Publisher publish)
  {
    publisher = publish;
    publisher.MyEvent += OnMyEventFires;
  }

  void OnMyEventFires (object sender, EventArgs e)
  {
    Debug.WriteLine ("The publisher notified the subscriber of an event");
  }

  public void Dispose ()
  {
    publisher.MyEvent -= OnMyEventFires;
  }
}
```

Die `Subscriber`-Klasse bestellt das Abonnement des Ereignisses in der `Dispose`-Methode ab.

Verweiszyklen können auch im Zusammenhang mit Ereignishandlern und Lambdasyntax auftreten, so wie Lambdaausdrücke auf Objekte verweisen und sie im Speicher resident halten können. Daher kann ein Verweis auf die anonyme Methode in einem Feld gespeichert und wie im folgenden Codebeispiel gezeigt dazu verwendet werden, ein Ereignisabonnement abzubestellen:

```csharp
public class Subscriber : IDisposable
{
  readonly Publisher publisher;
  EventHandler handler;

  public Subscriber (Publisher publish)
  {
    publisher = publish;
    handler = (sender, e) => {
      Debug.WriteLine ("The publisher notified the subscriber of an event");
    };
    publisher.MyEvent += handler;
  }

  public void Dispose ()
  {
    publisher.MyEvent -= handler;
  }
}
```

Das `handler`-Feld enthält den Verweis auf die anonyme Methode und dient zum Abonnieren und Abbestellen von Ereignissen.

<a name="weakreferences" />

## <a name="use-weak-references-to-prevent-immortal-objects"></a>Verwenden schwacher Verweise, um speicherresidente Objekte zu vermeiden

> [!NOTE]
> iOS-Entwickler sollten sich mit der Dokumentation zum [Vermeiden von Zirkelbezügen in iOS](~/ios/deploy-test/performance.md#avoidcircularreferences) vertraut machen, um sicherzustellen, dass ihre Apps Speicher effizient verwenden.

<a name="lazy" />

## <a name="delay-the-cost-of-creating-objects"></a>Verzögern der Kosten der Objekterstellung

Mit der verzögerten Initialisierung kann die Erstellung eines Objekts bis zu seiner ersten Verwendung hinausgezögert werden. Diese Methode wird vorwiegend zur Steigerung der Leistung, der Vermeidung von Berechnungen und der Verringerung von Speicheranforderungen verwendet.


Für Objekte mit hohen Erstellungskosten kann die verzögerte Initialisierung in diesen beiden Szenarios verwendet werden:

- Wenn die Anwendung das Objekt möglicherweise nicht verwendet
- Wenn andere kostenintensive Vorgänge abgeschlossen werden müssen, bevor das Objekt erstellt wird

Mit der `Lazy<T>`-Klasse kann wie im folgenden Codebeispiel gezeigt ein verzögert initialisierter Typ definiert werden:

```csharp
void ProcessData(bool dataRequired = false)
{
  Lazy<double> data = new Lazy<double>(() =>
  {
    return ParallelEnumerable.Range(0, 1000)
                 .Select(d => Compute(d))
                 .Aggregate((x, y) => x + y);
  });

  if (dataRequired)
  {
    if (data.Value > 90)
    {
      ...
    }
  }
}

double Compute(double x)
{
  ...
}
```

Die verzögerte Initialisierung tritt beim ersten Zugriff auf die `Lazy<T>.Value`-Eigenschaft auf. Beim ersten Zugriff wird der umschlossene Typ erstellt, zurückgegeben und für einen späteren Zugriff gespeichert.

Weitere Informationen zur verzögerten Initialisierung finden Sie unter [Verzögerte Initialisierung](https://msdn.microsoft.com/en-us/library/dd997286(v=vs.110).aspx).

<a name="async" />

## <a name="implement-asynchronous-operations"></a>Implementieren von asynchronen Vorgängen

.NET stellt asynchrone Versionen von vielen der dazugehörigen APIs bereit. Im Gegensatz zu synchronen APIs gewährleisten asynchrone APIs, dass der aktive Ausführungsthread den aufrufenden Thread nie für längere Zeit blockiert. Verwenden Sie daher die asynchrone API, sofern verfügbar, wenn eine API vom UI-Thread aufgerufen wird. So wird der UI-Thread nicht blockiert und die Benutzerfreundlichkeit der Anwendung optimiert.

Darüber hinaus sollten lang andauernde Vorgänge in einem Hintergrundthread ausgeführt werden, um zu vermeiden, dass der UI-Thread blockiert wird. .NET stellt die Schlüsselwörter `async` und `await` für das Schreiben von asynchronem Code bereit, der lang andauernde Vorgänge in einem Hintergrundthread ausführt und nach Abschluss auf die Ergebnisse zugreift. Obwohl lang andauernde Vorgänge asynchron mit dem `await`-Schlüsselwort ausgeführt können, bedeutet dies nicht automatisch, dass der Vorgang auch in einem Hintergrundthread ausgeführt wird. Dies können Sie gewährleisten, indem Sie den lang andauernden Vorgang wie im folgenden Codebeispiel gezeigt an `Task.Run` übergeben:

```csharp
public class FaceDetection
{
  ...
  async void RecognizeFaceButtonClick(object sender, EventArgs e)
  {
    await Task.Run(() => RecognizeFace ());
    ...
  }

  async Task RecognizeFace()
  {
    ...
  }
}
```

Die `RecognizeFace`-Methode führt in einem Hintergrundthread aus, während die Methode `RecognizeFaceButtonClick` bis zum Abschluss der `RecognizeFace`-Methode wartet, um fortzufahren.

Lang andauernde Vorgänge sollten auch einen Abbruch unterstützen. Es ist z.B. nicht notwendig, einen lang andauernden Vorgang fortzusetzen, wenn der Benutzer innerhalb der Anwendung navigiert. Das Muster für die Implementierung eines Abbruch lautet wie folgt:

- Erstellen Sie eine `CancellationTokenSource`-Instanz. Diese Instanz verwaltet und sendet Benachrichtigungen über den Abbruch.
- Übergeben Sie den `CancellationTokenSource.Token`-Eigenschaftswert an jede Aufgabe, deren Abbruch möglich sein soll.
- Stellen Sie für jede Aufgabe einen Mechanismus bereit, um auf den Abbruch zu reagieren.
- Rufen Sie die `CancellationTokenSource.Cancel`-Methode auf, um eine Benachrichtigung über den Abbruch bereitzustellen.

> [!IMPORTANT]
> Die `CancellationTokenSource`-Klasse implementiert die `IDisposable`-Schnittstelle. Die `CancellationTokenSource.Dispose`-Methode wird daher aufgerufen, sobald die `CancellationTokenSource`-Instanz abgeschlossen wurde.



Weitere Informationen finden Sie unter [Async Support Overview (Übersicht über die asynchrone Unterstützung)](~/cross-platform/platform/async.md).

<a name="sgen" />

## <a name="use-the-sgen-garbage-collector"></a>Verwenden des SGen-Garbage Collectors

Verwaltete Sprachen wie C# verwenden die Garbage Collection, um Speicher freizugeben, der nicht mehr genutzten Objekten zugewiesen ist. Die beiden von der Xamarin-Plattform genutzten Garbage Collectors sind:

- [**SGen**](http://www.mono-project.com/docs/advanced/garbage-collector/sgen/): Dies ist ein generationeller Garbage Collector und der Standard für die Xamarin-Plattform.
- [**Boehm**](http://www.hboehm.info/gc/): Dies ist ein konservativer, nicht generationeller Garbage Collector und der Standard für Xamarin.iOS-Anwendungen, die die Classic API verwenden.

SGen nutzt einen dieser drei Heaps, um Objekten Speicher zuzuweisen:

-  **Die Nursery**: Diesem Heap werden neue, kleine Objekte zugewiesen. Wenn in der Nursery kein Speicherplatz mehr verfügbar ist, erfolgt eine kleinere Garbage Collection. Alle Liveobjekte werden dabei zum Major Heap verschoben.
-  **Major Heap**: Hier werden lang ausgeführte Objekte aufbewahrt. Wenn im Major Heap nicht genügend Speicher verfügbar ist, erfolgt eine große Garbage Collection. Wenn dabei nicht genügend Speicher freigegeben wird, fordert SGen beim System mehr Speicher an.
-  **Large Object Space**: Hier werden Objekte aufbewahrt, die mehr als 8.000 Byte erfordern. Große Objekte werden von Anfang an diesem Heap zugewiesen, nicht der Nursery.

Einer der Vorteile von SGen ist, dass sich der Zeitaufwand für eine kleinere Garbage Collection proportional zur Anzahl der neuen Liveobjekte verhält, die seit der letzten kleineren Garbage Collection erstellt wurden. Dies verringert die Auswirkungen der Garbage Collection auf die Anwendungsleistung, da kleinere Garbage Collections weniger Zeit in Anspruch nehmen als längere. Größere Garbage Collections finden weiterhin statt, jedoch seltener.

### <a name="reducing-pressure-on-the-garbage-collector"></a>Verringern der Auslastung des Garbage Collectors

Wenn SGen eine Garbage Collection beginnt, werden die Threads der Anwendung während der Freigabe von Speicher angehalten. Während der Freigabe kann die Benutzeroberfläche der Anwendung kurz anhalten oder stocken. Ob dieses Stocken wahrnehmbar ist, hängt von zwei Faktoren ab:

1. **Häufigkeit**: Die Häufigkeit, in der Garbage Collections erfolgen. Die Häufigkeit von Garbage Collections erhöht sich, je mehr Speicher zwischen Bereinigungen zugewiesen wird.
1. **Dauer**: Die Dauer der einzelnen Garbage Collections. Sie ist in etwa proportional zur Anzahl der Liveobjekte, die erfasst werden.

Alles in allem bedeutet dies, dass wenn viele, nicht speicherresidente Objekte zugewiesen werden, dann viele kurze Garbage Collections erfolgen. Im Umkehrschluss bedeutet dies, dass wenn neue, speicherresidente Objekte langsam zugewiesen werden, erfolgen weniger, aber umfassendere Garbage Collections.

Um die Auslastung des Garbage Collectors zu reduzieren, beachten Sie Folgendes:

- Vermeiden Sie Garbage Collection in engen Schleifen mithilfe von Objektpools. Dies ist besonders für Spiele wichtig, in denen der Großteil der Objekte im Voraus erstellt werden muss.
- Geben Sie Ressourcen wie Datenströme, Netzwerkverbindungen, große Speicherblöcke und Dateien explizit frei, sobald sie nicht mehr benötigt werden. Weitere Informationen finden Sie im Abschnitt [Freigeben von IDisposable-Ressourcen](#idisposable).
- Heben Sie die Registrierung von Ereignishandlern auf, sobald sie nicht länger benötigt werden, damit die Objekte bereinigt werden können. Weitere Informationen finden Sie unter [Abbestellen von Ereignisabonnements](#events).

<a name="linker" />

## <a name="reduce-the-size-of-the-application"></a>Reduzieren der Anwendungsgröße

Um nachvollziehen zu können, woher die Größe der ausführbaren Datei einer Anwendung kommt, müssen Sie den Kompilierungsprozess auf jeder Plattform verstehen:

- iOS-Anwendungen werden Ahead-of-Time (AOT) in der ARM-Assemblysprache kompiliert. .NET Framework ist enthalten, wobei nicht verwendete Klassen nur dann entfernt werden, wenn die entsprechende Linkeroption aktiviert ist.
- Android-Anwendungen werden in der IL kompiliert und mit MonoVM und der Just-in-Time-Kompilierung verpackt. Nicht verwendete Framework-Klassen werden nur dann entfernt, wenn die entsprechende Linkeroption aktiviert ist.
- Windows Phone-Anwendungen werden in der IL kompiliert und von der integrierten Runtime ausgeführt.

Wenn eine Anwendung außerdem Generika umfassend verwendet, wird die endgültige Größe der ausführbaren Datei weiter steigen, da sie nativ kompilierte Versionen der generischen Möglichkeiten enthält.

Um die Größe von Anwendungen zu verringern, ist in den Build-Tools der Xamarin-Plattform ein Linker enthalten. Dieser ist standardmäßig deaktiviert und muss in den Projektoptionen für die Anwendung aktiviert werden. Zur Buildzeit führt er statische Analysen der Anwendung aus, um zu bestimmen, welche Typen und Member tatsächlich von der Anwendung verwendet werden. Anschließend entfernt er alle nicht verwendeten Typen und Methoden aus der Anwendung.

Der folgende Screenshot zeigt die Linkeroptionen in Visual Studio für Mac für ein Xamarin.iOS-Projekt:

![](memory-perf-best-practices-images/linker-options-ios.png)

Der folgende Screenshot zeigt die Linkeroptionen in Visual Studio für Mac für ein Xamarin.Android-Projekt:

![](memory-perf-best-practices-images/linker-options-droid.png)

Der Linker stellt drei unterschiedliche Einstellungen zur Steuerung des Verhaltens bereit:

-  **Nicht verknüpfen**: Der Linker entfernt keine nicht verwendeten Typen und Methoden. Aus Leistungsgründen ist dies die Standardeinstellung für Debugbuilds.
-  **Nur Framework SDKs verknüpfen/Nur SDK-Assemblys**: Diese Einstellung reduziert lediglich die Größe der Assemblys, die von Xamarin versendet werden. Der Benutzercode wird nicht beeinflusst.
-  **Alle Assemblys verknüpfen**: Dies ist eine aggressivere Optimierung, die auf die SDK-Assemblys und den Benutzercode abzielt. Diese Einstellung entfernt bei Bindungen nicht verwendete Unterstützungsfelder und verschlankt alle Instanzen (oder gebundenen Objekte), sodass sie weniger Speicher beanspruchen.

Die Einstellung *Alle Assemblys verknüpfen* sollte mit Vorsicht verwendet werden, da sie die Anwendung auf unerwartete Weise unterbrechen kann. Die vom Linker ausgeführte statische Analyse kann möglicherweise den gesamten erforderlichen Code nicht richtig ermitteln, woraufhin zu viel Code aus der kompilierten Anwendung entfernt wird. Diese Situation ergibt sich nur zur Laufzeit, wenn die Anwendung abstürzt. Aus diesem Grund ist es wichtig, eine Anwendung nach einer Änderung des Linkerverhaltens gründlich zu testen.

Wenn Tests anzeigen, dass der Linker fälschlicherweise eine Klasse oder Methode entfernt hat, können Sie die Typen oder Methoden, auf die nicht statisch verwiesen wird, die aber von der Anwendung benötigt werden, mit einem der folgenden Attribute markieren:

-  `Xamarin.iOS.Foundation.PreserveAttribute`: Dieses Attribut gilt für Xamarin.iOS-Projekte.
-  `Android.Runtime.PreserveAttribute`: Dieses Attribut gilt für Xamarin.Android-Projekte.

Es kann z.B. erforderlich sein, die Standardkonstruktoren von Typen beizubehalten, die dynamisch instanziiert werden. Auch für die XML-Serialisierung kann eine Beibehaltung der Eigenschaften von Typen erforderlich sein.

Weitere Informationen finden Sie in den Artikeln zum [Linker für iOS](~/ios/deploy-test/linker.md) und zum [Linker für Android](~/android/deploy-test/linker.md).

### <a name="additional-size-reduction-techniques"></a>Zusätzliche Methoden zur Größenreduktion

Es gibt unzählige CPU-Architekturen für mobile Geräte. Xamarin.iOS und Xamarin.Android erstellen daher *Fat Binarys*, die eine kompilierte Version der Anwendung für jede CPU-Architektur enthalten. Dadurch wird sichergestellt, dass eine mobile Anwendung auf jedem Gerät unabhängig von der CPU-Architektur ausgeführt werden kann.

Anhand der folgenden Schritte können Sie die Größe ausführbarer Anwendungsdateien weiter reduzieren:

- Stellen Sie sicher, dass ein Releasebuild erstellt wird.
- Reduzieren Sie die Anzahl von Architekturen, für die eine Anwendung erstellt wurde, damit kein FAT-Binär erstellt wird.
- Stellen Sie sicher, dass der LLVM-Compiler verwendet wird, um eine optimierte ausführbare Datei zu generieren.
- Reduzieren Sie die Größe des verwalteten Anwendungscodes. Sie erreichen dies, indem Sie den Linker für jede Assembly aktivieren (*Alle verknüpfen* für iOS-Projekte und *Alle Assemblys verknüpfen* für Android-Projekte).

Android-Apps können auch für jede ABI („Architektur“) in ein separates APK aufgeteilt werden.
Weitere Informationen finden Sie im Blogbeitrag [How To Keep Your Android App Size Down (So verhindern Sie, dass sich Ihre Android-App vergrößert)](http://motzcod.es/post/112072508362/how-to-keep-your-android-app-size-down).

<a name="optimizeimages" />

## <a name="optimize-image-resources"></a>Optimieren von Bildressourcen

Bilder gehören zu den speicherintensivsten Ressourcen, die Anwendungen verwenden können, und werden häufig mit hoher Auflösung erfasst. So werden zwar gestochen scharfe, lebendige Bilder erstellt, jedoch benötigen Anwendungen, die solche Bilder anzeigen, in der Regel mehr CPU-Auslastung, um das Bild zu decodieren, und mehr Speicher für das decodierte Bild. Es ist unwirtschaftlich, ein hochauflösendes Bild im Speicher zu decodieren, wenn es anschließend für die Anzeige auf eine kleinere Größe herunterskaliert wird. Reduzieren Sie stattdessen die CPU-Auslastung und die Speicherbeanspruchung, indem Sie mehrere Auflösungsversionen der gespeicherten Bilder erstellen, die nah bei den vorhergesagten Anzeigegrößen liegen. Beispiel: Ein Bild, das in einer Listenansicht angezeigt wird, benötigt wahrscheinlich eine niedrigere Auflösung als ein Bild für den Vollbildmodus. Darüber hinaus können die zentral herunterskalierten Versionen hochauflösender Bilder geladen werden, um sie effizient mit minimaler Speichernutzung anzuzeigen. Weitere Informationen finden Sie unter [Load Large Bitmaps Efficiently (Effizientes Laden großer Bitmaps)](https://developer.xamarin.com/recipes/android/resources/general/load_large_bitmaps_efficiently/).

Unabhängig von der Bildauflösung kann das Anzeigen von Bildressourcen den Speicherbedarf der App erheblich erhöhen. Sie sollten daher nur erstellt werden, wenn dies erforderlich ist. Und sie sollten freigegeben werden, sobald die Anwendung sie nicht mehr benötigt.

<a name="activationperiod" />

## <a name="reduce-the-application-activation-period"></a>Reduzieren des Aktivierungszeitraums der Anwendung

Alle Anwendungen haben eine *Aktivierungszeitraum*. Damit ist die Zeit zwischen dem Anwendungsstart und dem Zeitpunkt gemeint, an dem die Anwendung zur Verwendung bereit ist. Während dieses Aktivierungszeitraums erhalten Benutzer einen ersten Eindruck der Anwendung. Daher ist es wichtig, die Aktivierungsdauer für die Benutzer so kurz wie möglich zu halten, damit sie einen positiven ersten Eindruck von der Anwendung gewinnen.

Vor der anfänglichen Benutzeroberfläche zeigt eine Anwendung einen Begrüßungsbildschirm an, um zu verdeutlichen, dass die Anwendung gestartet wird. Wenn die Anwendung die Startbenutzeroberfläche nicht schnell anzeigen kann, sollten Benutzer während des Aktivierungszeitraums auf dem Begrüßungsbildschirm über den Ladefortschritt informiert werden. So wird angezeigt, dass die Anwendung nicht abgestürzt ist. Dies könnte über eine Statusanzeige oder ein ähnliches Steuerelement erfolgen.

Während des Aktivierungszeitraums führen Anwendungen Aktivierungslogik aus, die häufig das Laden und Verarbeiten von Ressourcen umfasst. Der Aktivierungszeitraum kann reduziert werden, indem sichergestellt wird, dass die erforderliche Ressourcen innerhalb der App verpackt und nicht remote abgerufen werden. Unter bestimmten Umständen müssen z.B. lokal gespeicherte Platzhalterdaten während des Aktivierungszeitraums geladen werden. Sobald die anfängliche UI angezeigt wird und der Benutzer mit der App interagieren kann, können die Platzhalterdaten nach und nach aus einer Remotequelle ersetzt werden. Außerdem sollte die Aktivierungslogik der Anwendung nur Tasks ausführen, die für den Anwendungsstart erforderlich sind. Dies kann hilfreich sein, wenn dadurch das Laden zusätzlicher Assemblys verzögert wird, da Assemblys bei der ersten Verwendung geladen werden.

<a name="webservicecommunication" />

## <a name="reduce-web-service-communication"></a>Reduzieren der Kommunikation des Webdiensts

Das Herstellen einer Verbindung mit einem Webdienst aus einer Anwendung kann Auswirkungen auf die Anwendungsleistung haben. Eine höhere Nutzung der Netzwerkbandbreite führt beispielsweise zu einer gesteigerten Nutzung des Akkus. Darüber hinaus verwenden Benutzer die Anwendung möglicherweise in einer Umgebung mit eingeschränkter Bandbreite. Daher ist es sinnvoll, die Nutzung der Bandbreite zwischen einer Anwendung und einem Webdienst zu beschränken.

Dies kann erreicht werden, indem Daten vor der Übertragung über ein Netzwerk komprimiert werden. Die zusätzliche CPU-Auslastung aus dem Komprimierungsvorgang kann jedoch auch zu einer erhöhten Akkunutzung führen. Aus diesem Grund sollte dieser Kompromiss sorgfältig abgewägt werden, bevor Sie entscheiden, komprimierte Daten über ein Netzwerk zu verschieben.

Ein weiteres Problem ist das Format der Daten, die zwischen einer Anwendung und einen Webdienst verschoben werden. Die zwei primären Formate sind Extensible Markup Language (XML) und JavaScript Object Notation (JSON). XML ist ein textbasiertes Datenaustauschformat, das relativ große Datennutzlasten erzeugt, da es eine große Anzahl von Formatierungszeichen enthält. JSON ist ein textbasiertes Datenaustauschformat, das kompakte Datennutzlasten erzeugt, was zu geringeren Anforderungen an die Netzwerkbandbreite beim Senden und Empfangen von Daten führt. Aus diesem Grund ist JSON häufig das bevorzugte Format für mobile Anwendungen.

Es wird empfohlen, für die Übertragung von Daten zwischen Anwendungen an einen Webdienst Datenübertragungsobjekte (Data Transfer Objects, DTOs) zu verwenden. Ein DTO enthält einen Satz von Daten für die netzwerkübergreifende Übertragung. Mithilfe von DTOs können mehr Daten in einem einzelnen Remoteaufruf übertragen werden. So kann die Anzahl der von der Anwendung ausgeführten Remoteaufrufe reduziert werden. Ein Remoteaufruf mit einer größeren Datennutzlast dauert im Allgemeinen ähnlich lange wie ein Aufruf, der nur eine kleine Datennutzlast übergibt.

Vom Webdienst abgerufene Daten sollten lokal zwischengespeichert werden, wobei sie verwendet und nicht wiederholt vom Webdienst abgerufenen werden sollten. Für diesen Ansatz sollte jedoch eine geeignete Cachestrategie implementiert werden, damit Daten im lokalen Cache aktualisiert werden, wenn sie sich im Webdienst ändern.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden Möglichkeiten beschrieben und erläutert, wie Sie die Leistung von Anwendungen verbessern können, die mit der Xamarin-Plattform erstellt wurden. Wenn Sie diese Kniffe kombinieren, können Sie die CPU-Auslastung und die Speichermenge, die von einer Anwendung verwendet wird, erheblich reduzieren.

## <a name="related-links"></a>Verwandte Links

- [Xamarin.iOS-Leistung](~/ios/deploy-test/performance.md)
- [Xamarin.Android-Leistung](~/android/deploy-test/performance.md)
- [Introduction to the Xamarin Profiler (Einführung in den Xamarin Profiler)](~/tools/profiler/index.md)
- [Xamarin.Forms-Leistung](~/xamarin-forms/deploy-test/performance.md)
- [Async Support Overview (Übersicht über die asynchrone Unterstützung)](~/cross-platform/platform/async.md)
- [IDisposable](https://developer.xamarin.com/api/type/System.IDisposable/)
- [Avoiding Common Pitfalls in Xamarin Apps (Vermeiden häufiger Fehlerquellen in Xamarin-Apps) (Video)](https://university.xamarin.com/guestlectures/avoiding-common-pitfalls-in-xamarin-apps)
