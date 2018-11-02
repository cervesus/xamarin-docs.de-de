---
title: Xamarin.iOS-Leistung
description: Dieses Dokument beschreibt die Techniken, die zum Verbessern der Leistung und Speicherauslastung in Xamarin.iOS-Anwendungen verwendet werden können.
ms.prod: xamarin
ms.assetid: 02b1f628-52d9-49de-8479-f2696546ca3f
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 01/29/2016
ms.openlocfilehash: caf35ab601d20e1cb235ab9ebb131e6dffc614fc
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50108876"
---
# <a name="xamarinios-performance"></a>Xamarin.iOS-Leistung

Eine schlechte Anwendungsleistung kann sich auf unterschiedliche Weise bemerkbar machen. Die Anwendung reagiert scheinbar nicht mehr, der Bildlauf ist möglicherweise verlangsamt, und auch die Akkulaufzeit kann abnehmen. Leistungsoptimierung umfasst jedoch mehr als das bloße Implementieren eines effizienten Codes. Es muss ebenfalls berücksichtigt werden, wie der Benutzer die Leistung der Anwendung wahrnimmt. Wenn beispielsweise Vorgänge ausgeführt werden können, ohne dass der Benutzer daran gehindert wird, gleichzeitig andere Aktivitäten auszuführen, kann dies dazu beitragen die Benutzerfreundlichkeit zu verbessern. 

Dieses Dokument beschreibt die Techniken, die zum Verbessern der Leistung und Speicherauslastung in Xamarin.iOS-Anwendungen verwendet werden können.

> [!NOTE]
> Bevor Sie diesen Artikel lesen, sollten Sie zuerst den Artikel [Cross-Platform Performance (Plattformübergreifende Leistung)](~/cross-platform/deploy-test/memory-perf-best-practices.md) lesen, der nicht-plattformspezifische Methoden zur Verbesserung der Arbeitsspeicherauslastung und Leistung von Anwendungen beschreibt, die mit der Xamarin-Plattform erstellt wurden.

## <a name="avoid-strong-circular-references"></a>Vermeiden starker Zirkelverweise

In einigen Situationen ist es möglich, starke Verweiszyklen zu erstellen, die Objekte davor schützen, dass ihr Speicher vom Garbage Collector wieder zurückgefordert wird. Betrachten Sie beispielsweise den Fall, in dem eine von [`NSObject`](https://developer.xamarin.com/api/type/Foundation.NSObject/) abgeleitete Unterklasse, wie beispielsweise eine Klasse, die von [`UIView`](https://developer.xamarin.com/api/type/UIKit.UIView/) erbt, zu einem von `NSObject` abgeleiteten Container hinzugefügt und von Objective-C, wie im folgenden Codebeispiel gezeigt, stark referenziert wird:

```csharp
class Container : UIView
{
    public void Poke ()
    {
    // Call this method to poke this object
    }
}

class MyView : UIView
{
    Container parent;
    public MyView (Container parent)
    {
        this.parent = parent;
    }

    void PokeParent ()
    {
        parent.Poke ();
    }
}

var container = new Container ();
container.AddSubview (new MyView (container));
```

Wenn dieser Code die `Container`-Instanz erstellt, enthält das C#-Objekt einen starken Verweis auf ein Objective-C-Objekt. Die `MyView`-Instanz enthält auch einen ähnlichen, starken Verweis auf ein Objective-C-Objekt.

Zudem erhöht der Aufruf von `container.AddSubview` den Verweiszähler für die nicht verwaltete `MyView`-Instanz. In diesem Fall erstellt die Xamarin.iOS-Runtime eine `GCHandle`-Instanz, damit das `MyView`-Objekt in verwaltetem Code aktiv bleibt, da es keine Garantie dafür gibt, dass irgendwelche verwalteten Objekte einen Verweis darauf behalten. Im Hinblick auf den verwalteten Code würde das `MyView`-Objekt nach dem [`AddSubview`](https://developer.xamarin.com/api/member/UIKit.UIView.AddSubview/p/UIKit.UIView/)-Aufruf zurückgefordert werden, wäre da nicht `GCHandle`.

Das nicht verwaltete `MyView`-Objekt verfügt über einen `GCHandle`-Zeiger auf das verwaltete Objekt, bekannt als *starker Link*. Das verwaltete Objekt enthält einen Verweis auf die `Container`-Instanz. Die `Container`-Instanz enthält wiederum einen verwalteten Verweis auf das `MyView`-Objekt.

In Fällen, in denen ein enthaltenes Objekt einen Link zu seinem Container speichert, stehen mehrere Optionen für den Umgang mit den Zirkelverweisen zur Verfügung:

-  Unterbrechen Sie den Zyklus manuell, indem Sie den Link zum Container auf `null`setzen.
-  Entfernen Sie die enthaltenen Objekte manuell aus dem Container.
-  Rufen Sie `Dispose` für die Objekte auf.
-  Vermeiden Sie, dass der Zirkelverweis einen Weak-Verweis auf den Container beibehält. Weitere Informationen über Weak-Verweise finden Sie unter:

### <a name="using-weakreferences"></a>Verwenden von Weak-Verweisen

Eine Möglichkeit, einen Zyklus zu verhindern, besteht in der Verwendung eines Weak-Verweises vom untergeordneten zum übergeordneten Element. Der obige Code kann z. B. wie folgt formuliert werden:

```csharp
class Container : UIView
{
    public void Poke ()
    {
        // Call this method to poke this object
    }
}

class MyView : UIView
{
    WeakReference<Container> weakParent;
    public MyView (Container parent)
    {
        this.weakParent = new WeakReference<Container> (parent);
    }

    void PokeParent ()
    {
        if (weakParent.TryGetTarget (out var parent))
            parent.Poke ();
    }
}

var container = new Container ();
container.AddSubview (new MyView (container));
```

Hier halt das darin enthaltene Objekt das übergeordnete Element nicht aktiv. Allerdings hält das übergeordnete Element das untergeordnete Element während des erfolgten `container.AddSubView` Aufrufs aktiv.

Dies kommt auch in den iOS-APIs vor, die den Delegaten oder das Datenquellenmuster verwenden. Dabei enthält eine Peerklasse die Implementierung, z.B. beim Festlegen der Eigenschaften [`Delegate`](https://developer.xamarin.com/api/property/MonoTouch.UIKit.UITableView.Delegate/)
oder [`DataSource`](https://developer.xamarin.com/api/property/MonoTouch.UIKit.UITableView.DataSource/)
in der [`UITableView`](https://developer.xamarin.com/api/type/UIKit.UITableView/)-Klasse.

Bei Klassen, die ausschließlich zum Zweck der Implementierung eines Protokolls erstellt werden, z.B. die [`IUITableViewDataSource`](https://developer.xamarin.com/api/type/MonoTouch.UIKit.IUITableViewDataSource/)-Klasse, können Sie, anstatt eine Unterklasse zu erstellen, nur die Schnittstelle in der Klasse implementieren, die Methode überschreiben und `this` die `DataSource`-Eigenschaft zuweisen.

#### <a name="weak-attribute"></a>Weak-Attribut

In [Xamarin.iOS 11.10](https://developer.xamarin.com/releases/ios/xamarin.ios_11/xamarin.ios_11.10/#WeakAttribute) wurde das `[Weak]`-Attribut eingeführt. Genau wie `WeakReference <T>` kann auch `[Weak]` verwendet werden, um [starke Zirkelverweise](https://docs.microsoft.com/en-us/xamarin/ios/deploy-test/performance#avoid-strong-circular-references), zu unterbrechen, jedoch mit noch weniger Code.

Betrachten Sie den folgenden Code, der `WeakReference <T>` verwendet:

```csharp
public class MyFooDelegate : FooDelegate {
    WeakReference<MyViewController> controller;
    public MyFooDelegate (MyViewController ctrl) => controller = new WeakReference<MyViewController> (ctrl);
    public void CallDoSomething ()
    {
        MyViewController ctrl;
        if (controller.TryGetTarget (out ctrl)) {
            ctrl.DoSomething ();
        }
    }
}
```

Entsprechender Code mit `[Weak]` ist weitaus präziser:

```csharp
public class MyFooDelegate : FooDelegate {
    [Weak] MyViewController controller;
    public MyFooDelegate (MyViewController ctrl) => controller = ctrl;
    public void CallDoSomething () => controller.DoSomething ();
}
```

Folgender Ausdruck ist ein weiteres Beispiel der Verwendung von `[Weak]` im Kontext des [Delegierungsmusters](https://developer.apple.com/library/content/documentation/General/Conceptual/DevPedia-CocoaCore/Delegation.html):

```csharp
public class MyViewController : UIViewController 
{
    WKWebView webView;

    protected MyViewController (IntPtr handle) : base (handle) { }

    public override void ViewDidLoad ()
    {
        base.ViewDidLoad ();
        webView = new WKWebView (View.Bounds, new WKWebViewConfiguration ());
        webView.UIDelegate = new UIDelegate (this);
        View.AddSubview (webView);
    }
}

public class UIDelegate : WKUIDelegate 
{
    [Weak] MyViewController controller;

    public UIDelegate (MyViewController ctrl) => controller = ctrl;

    public override void RunJavaScriptAlertPanel (WKWebView webView, string message, WKFrameInfo frame, Action completionHandler)
    {
        var msg = $"Hello from: {controller.Title}";
        var alertController = UIAlertController.Create (null, msg, UIAlertControllerStyle.Alert);
        alertController.AddAction (UIAlertAction.Create ("Ok", UIAlertActionStyle.Default, null));
        controller.PresentViewController (alertController, true, null);
        completionHandler ();
    }
}
```

### <a name="disposing-of-objects-with-strong-references"></a>Verwerfen von Objekten mit starken Verweisen

Wenn ein starker Verweis vorhanden ist, und es schwierig ist, die Abhängigkeit zu entfernen, führen Sie eine `Dispose`-Methode zum Deaktivieren des übergeordneten Zeigers aus.

Für Container überschreiben Sie, wie im folgenden Codebeispiel gezeigt, die `Dispose`-Methode, um die enthaltenen Objekte zu entfernen:

```csharp
class MyContainer : UIView
{
    public override void Dispose ()
    {
        // Brute force, remove everything
        foreach (var view in Subviews)
        {
              view.RemoveFromSuperview ();
        }
        base.Dispose ();
    }
}
```

Für ein untergeordnetes Objekt, das einen starken Verweis zum übergeordneten Objekt beibehält, deaktivieren Sie den Verweis auf das übergeordnete Element in der `Dispose`-Implementierung:

```csharp
class MyChild : UIView 
{
    MyContainer container;
    public MyChild (MyContainer container)
    {
        this.container = container;
    }
    public override void Dispose ()
    {
        container = null;
    }
}
```

Weitere Informationen zum Freigeben von starken Verweisen finden Sie unter [Release IDisposable Resources (Freigeben von IDisposable Ressourcen)](~/cross-platform/deploy-test/memory-perf-best-practices.md#idisposable).
Dieser Blogbeitrag enthält eine gute Erläuterung: [Xamarin.iOS, the garbage collector and me (Xamarin.iOS, der Garbage Collector und ich)](http://krumelur.me/2015/04/27/xamarin-ios-the-garbage-collector-and-me/).

### <a name="more-information"></a>Weitere Informationen

Weitere Informationen finden Sie unter [Rules to Avoid Retain Cycles (Regeln zur Vermeidung von Beibehaltungszyklen)](http://www.cocoawithlove.com/2009/07/rules-to-avoid-retain-cycles.html) auf Cocoa With Love und unter [Is this a bug in MonoTouch GC (Ist dies ein Fehler im MonoTouch GC)](http://stackoverflow.com/questions/13058521/is-this-a-bug-in-monotouch-gc) und [Why can‘t Mono Touch GC kill managed objects with refcount > 1? (Warum kann MonoTouch GC verwaltete Objekte nicht mit Refcount > 1 löschen?)](http://stackoverflow.com/questions/13064669/why-cant-monotouch-gc-kill-managed-objects-with-refcount-1) auf StackOverflow.

## <a name="optimize-table-views"></a>Optimieren von Tabellenansichten

Benutzer erwarten einen sanften Bildlauf und schnelle Ladezeiten für [`UITableView`](https://developer.xamarin.com/api/type/UIKit.UITableView/)-Instanzen. Die Bildlaufleistung kann jedoch beeinträchtigt werden, wenn Zellen tief geschachtelte Ansichtshierarchien oder komplexe Layouts enthalten. Es gibt jedoch Techniken, die verwendet werden können, um eine schlechte `UITableView`-Leistung zu vermeiden:

- Wiederverwenden von Zellen Weitere Informationen finden Sie unter [Reuse Cells (Wiederverwenden von Zellen)](#reusecells).
- Verringern Sie die Anzahl von Unteransichten.
- Zwischenspeichern des Zellinhalts, der von einem Webdienst abgerufen wird
- Zwischenspeichern der Höhe beliebiger Zeilen, die sie nicht identisch sind
- Eine Zelle und alle anderen Ansichten undurchsichtig machen
- Vermeiden von Bildskalierung und Farbverläufen

Gemeinsam können diese Techniken in [`UITableView`](https://developer.xamarin.com/api/type/UIKit.UITableView/)-Instanzen zu einem sanften Bildlauf beitragen.

### <a name="reuse-cells"></a>Wiederverwenden von Zellen

Beim Anzeigen von Hunderten von Zeilen in einer [`UITableView`](https://developer.xamarin.com/api/type/UIKit.UITableView/)-Instanz wäre es eine Arbeitsspeicherverschwendung, Hunderte von [`UITableViewCell`](https://developer.xamarin.com/api/type/UIKit.UITableViewCell/)-Objekten zu erstellen, wenn nur eine kleine Anzahl von ihnen gleichzeitig auf dem Bildschirm angezeigt werden kann. Stattdessen können auch nur die Zellen, die auf dem Bildschirm sichtbar sind in den Arbeitsspeicher geladen werden, und der **Inhalt** wird in diese wiederverwendeten Zellen geladen. Dies verhindert die Instanziierung Hunderter zusätzlicher Objekte, wodurch sowohl Zeit als auch Arbeitsspeicher gespart werden.

Aus diesem Grund kann eine Zelle, wenn sie vom Bildschirm verschwindet in einer Warteschlange für die Wiederverwendung, wie im folgenden Codebeispiel gezeigt, platziert werden:

```csharp
class MyTableSource : UITableViewSource
{
    public override UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath)
    {
        // iOS will create a cell automatically if one isn't available in the reuse pool
        var cell = (MyCell) tableView.DequeueReusableCell (MyCellId, indexPath);

        // Perform required cell actions
        return cell;
    }
}
```

Wenn der Benutzer scrollt, ruft die [`UITableView`](https://developer.xamarin.com/api/type/UIKit.UITableView/)-Instanz die `GetCell`-Außerkraftsetzung auf, um die neuen anzuzeigenden Ansichten anzufordern. Diese Außerkraftsetzung ruft dann die [`DequeueReusableCell`](https://developer.xamarin.com/api/member/UIKit.UITableView.DequeueReusableCell/p/Foundation.NSString/)-Methode auf. Wenn eine Zelle zur Wiederverwendung verfügbar ist, wird sie zurückgegeben.

Weitere Informationen finden Sie unter [Cell Reuse (Wiederverwenden von Zellen)](~/ios/user-interface/controls/tables/populating-a-table-with-data.md) in [Populating a Table with Data (Befüllen einer Tabelle mit Daten)](~/ios/user-interface/controls/tables/populating-a-table-with-data.md).

## <a name="use-opaque-views"></a>Verwenden nicht transparenter Ansichten

Stellen Sie sicher, dass bei allen Ansichten, für die keine Transparenz definiert wurde die [`Opaque`](https://developer.xamarin.com/api/property/UIKit.UIView.Opaque/)-Eigenschaft gesetzt wird. Dadurch wird sichergestellt, dass die Ansichten vom Zeichensystem optimal dargestellt werden. Dies ist besonders wichtig, wenn eine Ansicht in ein [`UIScrollView`](https://developer.xamarin.com/api/type/UIKit.UIScrollView/)-Element eingebettet ist oder Teil einer komplexen Animation ist. Andernfalls setzt das Zeichensystem die Ansichten mit anderem Inhalt zusammen, was deutliche Auswirkungen auf die Leistung haben kann.

## <a name="avoid-fat-xibs"></a>Vermeiden von FAT XIBs

Zwar sind XIBs größtenteils durch Storyboards ersetzt worden, es gibt jedoch einige Situationen, in denen XIBs gegebenenfalls weiterhin verwendet werden. Wenn eine XIB in den Arbeitsspeicher geladen wird, werden ihre gesamten Inhalte, einschließlich aller Bilder, in den Arbeitsspeicher geladen. Enthält die XIB eine Ansicht, die nicht direkt verwendet wird, wird Arbeitsspeicher verschwendet. Stellen Sie daher bei der Verwendung von XIBs sicher, dass nur eine XIB pro Ansichtscontroller vorhanden ist, und trennen Sie, wenn möglich, die Ansichtshierarchie des Ansichtscontrollers in separate XIBs.

## <a name="optimize-image-resources"></a>Optimieren von Bildressourcen

Bilder gehören zu den speicherintensivsten Ressourcen, die Anwendungen verwenden, und werden häufig in hoher Auflösung aufgenommen. Stellen Sie aus diesem Grund sicher, dass beim Anzeigen eines Bilds aus dem App-Bundle in einer [`UIImageView`](https://developer.xamarin.com/api/type/UIKit.UIImageView/)-Klasse, das Bild und die `UIImageView` dieselbe Größe haben. Das Skalieren von Bildern zur Laufzeit kann ein aufwendiger Vorgang sein, insbesondere, wenn die `UIImageView`-Klasse in eine [`UIScrollView`](https://developer.xamarin.com/api/type/UIKit.UIScrollView/)-Klasse eingebettet ist.

Weitere Informationen finden Sie unter [Optimize Image Resources (Optimieren von Bildressourcen)](~/cross-platform/deploy-test/memory-perf-best-practices.md#optimizeimages) im Leitfaden [Cross Plattform Performance (Plattformübergreifende Leistung)](~/cross-platform/deploy-test/memory-perf-best-practices.md).

## <a name="test-on-devices"></a>Testen auf Geräten

Beginnen Sie so früh wie möglich mit der Bereitstellung und dem Testen einer Anwendung auf einem physischen Gerät. Simulatoren sind nicht genau auf das Verhalten und die Einschränkungen von Geräten abgestimmt. Deshalb sollten Sie so früh wie möglich in einem echten Szenario für ein Gerät testen.

Der Simulator kann z.B. nicht die CPU- oder Arbeitsspeichereinschränkungen eines physischen Geräts simulieren.

## <a name="synchronize-animations-with-the-display-refresh"></a>Synchronisieren von Animationen mit der Aktualisierung der Anzeige

Spiele verwenden oft enge Schleifen, um die Spiellogik auszuführen und den Bildschirm zu aktualisieren. Typische Bildfrequenzen reichen von 30 bis 60 Bildern pro Sekunde. Einige Entwickler denken, dass sie den Bildschirm so oft wie möglich pro Sekunde aktualisieren sollten. Sie kombinieren ihre Spielsimulation mit Aktualisierungen des Bildschirms und sind so versucht, die Anzeige mehr als sechzig Mal pro Sekunde zu aktualisieren.

Allerdings erlaubt der Bildschirmserver nicht mehr als 60 Bilder pro Sekunde. Daher können Versuche, den Bildschirm häufiger zu aktualisieren, zu Screen Tearing und Micro-Stuttering führen. Sie sollten den Code so strukturieren, dass Änderungen auf dem Bildschirm mit der Aktualisierung der Anzeige synchronisiert werden. Dies kann durch die [`CoreAnimation.CADisplayLink`](https://developer.xamarin.com/api/type/CoreAnimation.CADisplayLink/)-Klasse erreicht werden, die mit 60 Bildern pro Sekunde ausgeführt wird und einen Timer darstellt, der für die Visualisierung und für Spiele geeignet ist.

## <a name="avoid-core-animation-transparency"></a>Vermeiden von Transparenz bei der Kernanimation

Das Vermeiden von Transparenz bei der Kernanimation verbessert die Bitmap-Mischleistung. Vermeiden Sie allgemein transparente Ebenen und weichgezeichnete Rahmen, falls möglich.

## <a name="avoid-code-generation"></a>Vermeiden von Code-Generierung

Das dynamische Generieren von Code mit `System.Reflection.Emit` oder der *Dynamic Language Runtime* müssen vermieden werden, da der iOS-Kernel verhindert, dass Code dynamisch ausgeführt wird.

## <a name="summary"></a>Zusammenfassung

Dieser Artikel hat viele Techniken zum Verbessern der Leistung von Anwendungen, die mit Xamarin.iOS erstellt wurden beschrieben und erläutert. Wenn Sie diese Kniffe kombinieren, können Sie die CPU-Auslastung und die Speichermenge, die von einer Anwendung verwendet wird, erheblich reduzieren.

## <a name="related-links"></a>Verwandte Links

- [Plattformübergreifende Leistung](~/cross-platform/deploy-test/memory-perf-best-practices.md)
