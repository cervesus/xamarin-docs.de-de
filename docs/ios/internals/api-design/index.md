---
title: Xamarin.iOS-API-Design
description: Leitlinien bei der Ausarbeitung der Xamarin.iOS-APIs und ihre Beziehung zu Objective-C.
ms.prod: xamarin
ms.assetid: 322D2724-AF27-6FFE-BD21-AA1CFE8C0545
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/21/2017
ms.openlocfilehash: a2435b30b7d5b468fca6c55d295c87b9a0d20652
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/13/2020
ms.locfileid: "79303726"
---
# <a name="xamarinios-api-design"></a>Xamarin.iOS-API-Design

Zusätzlich zu den wichtigsten Basisklassenbibliotheken, die Teil von Mono sind, bietet [Xamarin.iOS](~/ios/index.yml) Bindungen für verschiedene iOS-APIs, die Entwicklern ermöglichen, mithilfe von Mono native iOS-Anwendungen zu erstellen.

Im Mittelpunkt von Xamarin.iOS stehen eine Interop-Engine, die eine Brücke zwischen C# und Objective-C schlägt, sowie Bindungen für die iOS C-basierten APIs wie CoreGraphics und [OpenGL ES](#opengles).

Die Runtime auf niedriger Ebene zur Kommunikation mit Objective-C-Code ist in [MonoTouch.ObjCRuntime](#objcruntime). Darauf aufbauend werden Bindungen für [Foundation](#foundation), CoreFoundation und [UIKit](#uikit) bereitgestellt.

## <a name="design-principles"></a>Entwurfsprinzipien

Es folgen einige unserer Designprinzipien für die Xamarin.iOS-Bindungen (sie gelten auch für Xamarin.Mac, die Mono-Bindungen für Objective-C unter macOS):

- Befolgen Sie die [Framework-Entwurfsrichtlinien](https://docs.microsoft.com/dotnet/standard/design-guidelines).
- Erlauben Sie Entwicklern, Unterklassen von Objective-C-Klassen zu erstellen:

  - Arbeiten Sie mit Ableitungen von einer vorhandenen Klasse.
  - Rufen Sie den verkettenden Basiskonstruktor auf.
  - Das Überschreiben von Methoden sollte mit dem Überschreibungssystem von C# erfolgen.
  - Das Erstellen von Unterklassen sollten mit C#-Standardkonstrukten erfolgen.

- Machen Sie Objective-C-Selektoren nicht für Entwickler verfügbar.
- Stellen Sie einen Mechanismus zum Abrufen beliebiger Objective-C-Bibliotheken bereit.
- Erleichtern Sie gängige Objective-C-Aufgaben, und ermöglichen Sie komplexe Objective-C-Aufgaben.
- Machen Sie Objective-C-Eigenschaften als C#-Eigenschaften verfügbar.
- Machen Sie eine stark typisierte API verfügbar:

  - Erhöhen Sie die Typsicherheit.
  - Minimieren Sie Laufzeitfehler.
  - Nutzen Sie IntelliSense für Rückgabetypen.
  - Ermöglichen Sie die Dokumentation von IDE-Popups.

- Fördern Sie in der IDE die Erkundung der APIs:

  - Anstatt beispielsweise ein schwach typisiertes Array wie das folgende verfügbar zu machen:

    ```objc
    NSArray *getViews
    ```

    Machen Sie einen starken Typ wie folgt verfügbar:

    ```csharp
    NSView [] Views { get; set; }
    ```

    Dies ermöglicht Visual Studio für Mac die automatische Vervollständigung während des Durchsuchens der API, macht alle `System.Array`-Vorgänge für den Rückgabewert verfügbar und ermöglicht dem Rückgabewert die Teilnahme an LINQ.

- Native C#-Typen:

  - [`NSString` wird zu `string`](~/ios/internals/api-design/nsstring.md)
  - Wandeln Sie die Parameter `int` und `uint`, die Enumerationen hätten sein sollen, in C#-Enumerationen und C#-Enumerationen mit `[Flags]`-Attributen um.
  - Machen Sie Arrays statt als typneutrale `NSArray`-Objekte als stark typisierte Arrays verfügbar.
  - Räumen Sie Benutzern für Ereignisse und Benachrichtigungen eine Wahl zwischen Folgendem ein:

    - Standardmäßig einer stark typisierten Version
    - Einer schwach typisierten Version für erweiterte Anwendungsfälle

- Unterstützen Sie das Delegatmuster von Objective-C:

  - C#-Ereignissystem
  - Machen Sie C#-Delegaten (Lambdaausdrücke, anonyme Methoden und `System.Delegate`) für Objective-C-APIs als Blöcke verfügbar.

### <a name="assemblies"></a>Assemblys

Xamarin.iOS bietet eine Reihe von Assemblys, die das *Xamarin.iOS-Profil* bilden. Weitere Informationen finden Sie auf der Seite [Assemblys](~/cross-platform/internals/available-assemblies.md).

### <a name="major-namespaces"></a>Wichtige Namespaces

#### <a name="objcruntime"></a>ObjCRuntime

Der Namespace [ObjCRuntime](xref:ObjCRuntime) gibt Entwicklern die Möglichkeit, eine Brücke zwischen C# und Objective-C zu schlagen.
Dies ist eine neue, speziell für iOS entwickelte Bindung, die auf den Erfahrungen mit Cocoa# und Gtk# basiert.

#### <a name="foundation"></a>Foundation

Der Namespace [Foundation](xref:Foundation) stellt die grundlegenden Datentypen für die Zusammenarbeit mit dem Framework der Objective-C Foundation zur Verfügung, das Teil von iOS ist und die Grundlage für die objektorientierte Programmierung in Objective-C bildet.

Xamarin.iOS spiegelt in C# die Hierarchie der Klassen in Objective-C wider. Beispielsweise ist die Objective-C-Basisklasse NSObject in C# über [Foundation.NSObject](xref:Foundation.NSObject) einsetzbar.

Obwohl dieser Namespace Bindungen für die zugrunde liegenden Objective-C Foundation-Typen bereitstellt, haben wir in einigen Fällen die zugrunde liegenden Typen .NET-Typen zugeordnet. Zum Beispiel:

- Anstatt sich mit NSString und [NSArray](https://developer.apple.com/library/ios/#documentation/Cocoa/Reference/Foundation/Classes/NSArray_Class/NSArray.html) zu befassen, macht die Laufzeit diese als C#-[String](xref:System.String)-Elemente und stark typisierte [Arrays](xref:System.Array) in der gesamten API verfügbar.

- Nachstehend werden verschiedene Hilfs-APIs vorgestellt, die es Entwicklern ermöglichen, Objective-C-APIs von Drittanbietern, andere iOS-APIs oder APIs, die derzeit nicht an Xamarin.iOS gebunden sind, zu binden.

Weitere Informationen zu Bindungs-APIs finden Sie im Abschnitt [Xamarin.iOS-Bindungsgenerator](~/cross-platform/macios/binding/binding-types-reference.md).

##### <a name="nsobject"></a>NSObject

Der Typ [NSObject](xref:Foundation.NSObject) ist die Grundlage sämtlicher Objective-C-Bindungen. Xamarin.iOS-Typen spiegeln zwei Klassen von Typen der iOS-CocoaTouch-APIs wider: die C-Typen (typischerweise als CoreFoundation-Typen bezeichnet) und die Objective-C-Typen (die alle von der NSObject-Klasse abgeleitet sind).

Für jeden Typ, der einen nicht verwalteten Typ widerspiegelt, ist es möglich, das native Objekt über die [Handle](xref:Foundation.NSObject.Handle)-Eigenschaft zu beziehen.

Während Mono die Garbage Collection für alle Ihre Objekte übernimmt, implementiert `Foundation.NSObject` die Schnittstelle [System.IDisposable](xref:System.IDisposable). Das bedeutet, dass Sie die Ressourcen eines beliebigen NSObject explizit freigeben können, ohne darauf warten zu müssen, dass der Garbage Collector aktiviert wird. Dies ist wichtig, wenn Sie umfangreiche NSObjects verwenden, z. B. UIImages, die Zeiger auf große Datenblöcke enthalten können.

Wenn Ihr Typ eine deterministische Finalisierung durchführen muss, überschreiben Sie die [NSObject.Dispose(bool)-Methode](xref:Foundation.NSObject.Dispose(System.Boolean)). Der Parameter für „Dispose“ ist „bool disposing“. Falls auf TRUE festgelegt, bedeutet dies, dass Ihre Dispose-Methode aufgerufen wird, weil der Benutzer Dispose () explizit für das Objekt aufgerufen hat. Wenn der Wert FALSE ist, bedeutet dies, dass Ihre Dispose(bool disposing)-Methode vom Finalizer für den Finalizerthread aufgerufen wird.

##### <a name="categories"></a>Kategorien

Ab Xamarin.iOS 8.10 ist es möglich, Objective-C-Kategorien in C# zu erstellen.

Dies erfolgt mithilfe des Attributs `Category`, wobei der zu erweiternde Typ als Argument für das Attribut angegeben wird. Im folgenden Beispiel wird z. B. NSString erweitert.

```csharp
[Category (typeof (NSString))]
```

Jede Kategoriemethode verwendet den normalen Mechanismus zum Exportieren von Methoden in Objective-C mit dem Attribut `Export`:

```csharp
[Export ("today")]
public static string Today ()
{
    return "Today";
}
```

Alle verwalteten Erweiterungsmethoden müssen statisch sein. Es ist jedoch möglich, Objective-C-Instanzmethoden unter Verwendung der Standardsyntax für Erweiterungsmethoden in C# zu erstellen:

```csharp
[Export ("toUpper")]
public static string ToUpper (this NSString self)
{
    return self.ToString ().ToUpper ();
}
```

Das erste Argument für die Erweiterungsmethode ist die Instanz, für die die Methode aufgerufen wurde.

Vollständiges Beispiel:

```csharp
[Category (typeof (NSString))]
public static class MyStringCategory
{
    [Export ("toUpper")]
    static string ToUpper (this NSString self)
    {
        return self.ToString ().ToUpper ();
    }
}
```

In diesem Beispiel wird der NSString-Klasse eine native toUpper-Instanzmethode hinzugefügt, die in Objective-C aufgerufen werden kann.

```csharp
[Category (typeof (UIViewController))]
public static class MyViewControllerCategory
{
    [Export ("shouldAutoRotate")]
    static bool GlobalRotate ()
    {
        return true;
    }
}
```

Ein Szenario, in dem dies nützlich ist, ist das Hinzufügen einer Methode zu einer ganzen Gruppe von Klassen in Ihrer Codebasis. So würden beispielsweise alle `UIViewController`-Instanzen melden, dass sie rotieren können:

```csharp
[Category (typeof (UINavigationController))]
class Rotation_IOS6 {
      [Export ("shouldAutorotate:")]
      static bool ShouldAutoRotate (this UINavigationController self)
      {
          return true;
      }
}
```

##### <a name="preserveattribute"></a>PreserveAttribute

PreserveAttribute ist ein benutzerdefiniertes Attribut, mit dem mtouch, das Bereitstellungstool von Xamarin.iOS, angewiesen wird, einen Typ, oder ein Member eines Typs, in der Phase beizubehalten, in der die Anwendung verarbeitet wird, um sie zu verkleinern.

Jeder Member, der nicht statisch durch die Anwendung verknüpft ist, muss entfernt werden. Daher wird dieses Attribut verwendet, um Member zu kennzeichnen, auf die nicht statisch verwiesen wird, die aber dennoch von Ihrer Anwendung benötigt werden.

Wenn Sie beispielsweise Typen dynamisch instanziieren, sollten Sie den Standardkonstruktor Ihrer Typen beibehalten. Wenn Sie die XML-Serialisierung verwenden, sollten Sie die Eigenschaften Ihrer Typen beibehalten.

Sie können dieses Attribut auf alle Member eines Typs oder auf den Typ selbst anwenden. Wenn Sie den gesamten Typ beibehalten möchten, können Sie für den Typ die Syntax [Preserve (AllMembers = true)] verwenden.

#### <a name="uikit"></a>UIKit

Der Namespace [UIKit](xref:UIKit) enthält eine 1:1-Zuordnung zu allen Benutzeroberflächenkomponenten, aus denen CocoaTouch besteht, und zwar in Form von C#-Klassen. Die API wurde so geändert, dass sie den in der Sprache C# üblichen Konventionen folgt.

Für allgemeine Vorgänge werden C#-Delegaten zur Verfügung gestellt. Weitere Informationen finden Sie im Abschnitt [Delegate](#delegates).

#### <a name="opengles"></a>OpenGLES

Für OpenGLES verteilen wir eine [modifizierte Version](xref:OpenTK) der [OpenTK](https://opentk.net/)-API, eine objektorientierte Bindung an OpenGL, die so geändert wurde, dass sie CoreGraphics-Datentypen und -Strukturen verwendet und nur die unter iOS verfügbare Funktionalität verfügbar macht.

OpenGLES 1.1-Funktionalität ist über den [Typ „ES11.GL“](xref:OpenTK.Graphics.ES11.GL) verfügbar.

OpenGLES 2.0-Funktionalität ist über den [Typ „ES20.GL“](xref:OpenTK.Graphics.ES20.GL) verfügbar.

OpenGLES 3.0-Funktionalität ist über den [Typ „ES30.GL“](xref:OpenTK.Graphics.ES30.GL) verfügbar.

### <a name="binding-design"></a>Bindungsdesign

Xamarin.iOS ist nicht nur eine Bindung an die zugrunde liegende Objective-C-Plattform. Es erweitert das .NET-Typ- und -Verteilungssystem, um C# und Objective-C besser zu integrieren.

Ebenso wie P/Invoke ein nützliches Tool zum Aufrufen nativer Bibliotheken unter Windows und Linux ist oder IJW-Unterstützung für COM-Interoperabilität unter Windows genutzt werden kann, erweitert Xamarin.iOS die Runtime durch die Unterstützung der Bindung von C#-Objekten an Objective-C-Objekte.

Die Erörterung in den nächsten Abschnitten ist nicht für Benutzer erforderlich, die Xamarin.iOS-Anwendungen entwickeln. Sie soll Entwicklern jedoch helfen, die Abläufe zu verstehen und sie bei der Realisierung komplexerer Anwendungen unterstützen.

#### <a name="types"></a>Typen

Sofern sinnvoll, werden C#-Typen anstelle von Foundation-Typen auf niedriger Ebene in C# verfügbar gemacht.  Dies bedeutet, dass [die API den C#-Typ „String“ anstelle von NSString](~/ios/internals/api-design/nsstring.md) und stark typisierte C#-Arrays verwendet, anstatt NSArray verfügbar zu machen.

Im Allgemeinen wird im Xamarin.iOS- und Xamarin. Mac-Design das zugrunde liegende `NSArray`-Objekt nicht verfügbar gemacht. Stattdessen konvertiert die Runtime `NSArray`-Elemente automatisch in stark typisierte Arrays einer beliebigen `NSObject`-Klasse. Daher macht Xamarin.iOS eine schwach typisierte Methode wie GetViews zur Rückgabe eines NSArray nicht verfügbar:

```csharp
NSArray GetViews ();
```

Stattdessen stellt die Bindung einen stark typisierten Rückgabewert wie diesen zur Verfügung:

```csharp
UIView [] GetViews ();
```

Es gibt einige wenige Methoden, die in `NSArray` verfügbar gemacht werden, für die Eckfälle, in denen Sie vielleicht direkt ein `NSArray` verwenden möchten. Von ihrer Verwendung wird jedoch in der API-Bindung abgeraten.

Darüber hinaus haben wir in der **Classic API**, statt `CGRect`, `CGPoint` und `CGSize` über die CoreGraphics-API verfügbar zu machen, diese durch die `System.Drawing`-Implementierungen `RectangleF`, `PointF` und `SizeF` ersetzt. Denn diese helfen Entwicklern, vorhandenen OpenGL-Code, der OpenTK verwendet, beizubehalten. Wenn Sie die neue 64-Bit-**Unified API** einsetzen, muss die CoreGraphics-API verwendet werden.

#### <a name="inheritance"></a>Vererbung

Das Xamarin.iOS-API-Design ermöglicht Entwicklern, native Objective-C-Typen auf dieselbe Weise zu erweitern, wie sie einen C#-Typ erweitern würden. Dazu wird das Schlüsselwort „Override“ für eine abgeleitete Klasse sowie die Verkettung bis zur Basisimplementierung mithilfe des C#-Schlüsselworts „base“ verwendet.

Dieses Design ermöglicht Entwicklern, den Umgang mit Objective-C-Selektoren als Teil ihres Entwicklungsprozesses zu vermeiden, da das gesamte Objective-C-System bereits von den Xamarin.iOS-Bibliotheken umschlossen ist.

#### <a name="types-and-interface-builder"></a>Typen und Interface Builder

Wenn Sie .NET-Klassen erstellen, die Instanzen von mit Interface Builder erstellten Typen sind, müssen Sie einen Konstruktor bereitstellen, der einen einzelnen `IntPtr`-Parameter verwendet.
Dies ist erforderlich, um die verwaltete Objektinstanz an das nicht verwaltete Objekt zu binden.
Der Code besteht aus einer einzelnen Zeile wie dieser:

```csharp
public partial class void MyView : UIView {
   // This is the constructor that you need to add.
   public MyView (IntPtr handle) : base (handle) {}
}
```

#### <a name="delegates"></a>Delegaten

In den Programmiersprachen Objective-C und C# hat das Wort „Delegat“ eine unterschiedliche Bedeutung.

In Objective-C und der Dokumentation, die Sie online zu CocoaTouch finden, ist ein Delegat normalerweise eine Instanz einer Klasse, die auf eine Reihe von Methoden reagiert. Dies ist einer C#-Schnittstelle sehr ähnlich, allerdings mit dem Unterschied, dass die Methoden nicht immer obligatorisch sind.

Diese Delegaten spielen eine wichtige Rolle in UIKit und anderen CocoaTouch-APIs. Sie dienen zum Ausführen verschiedener Aufgaben:

- Bereitstellen von Benachrichtigungen zu Ihrem Code (ähnlich der Ereignisübermittlung in C# oder Gtk+)
- Implementieren von Modellen für Steuerelemente zur Datenvisualisierung
- Bestimmen des Verhaltens eines Steuerelements

Das Programmiermuster wurde so angelegt, dass das Erstellen abgeleiteter Klassen zur Änderung des Verhaltens für ein Steuerelement minimiert wird. Diese Lösung ist im Prinzip vergleichbar mit dem Ansatz anderer GUI-Toolkits in den letzten Jahren: Signale in Gtk, Slots in Qt, Ereignisse in WinForms und WPF/Silverlight usw. Um Hunderte von Schnittstellen (eine für jede Aktion) zu vermeiden oder von Entwicklern zu verlangen, zu viele nicht benötigte Methoden zu implementieren, unterstützt Objective-C optionale Methodendefinitionen. Dies ist anders als bei C#-Schnittstellen, bei denen alle Methoden implementiert werden müssen.

In Objective-C-Klassen stellen Sie fest, dass Klassen, die dieses Programmiermuster befolgen, eine normalerweise als `delegate` bezeichnete Eigenschaft verfügbar machen. Sie ist erforderlich, um die obligatorischen Teile der Schnittstelle und null oder mehr der optionalen Teile zu implementieren.

In Xamarin.iOS werden drei sich gegenseitig ausschließende Verfahren zur Bindung an diese Delegaten geboten:

1. [Über Ereignisse](#via-events)
2. [Stark typisiert über eine `Delegate`-Eigenschaft](#strongly-typed-via-a-delegate-property)
3. [Schwach typisiert über eine `WeakDelegate`-Eigenschaft](#loosely-typed-via-the-weakdelegate-property)

Betrachten Sie beispielsweise die UIWebView-Klasse. Sie wird an eine UIWebViewDelegate-Instanz verteilt, die der Delegateigenschaft zugewiesen wird.

##### <a name="via-events"></a>Über Ereignisse

Für viele Typen erstellt Xamarin.iOS automatisch einen entsprechenden Delegaten, der die Aufrufe von `UIWebViewDelegate` an C#-Ereignisse weiterleitet. Für `UIWebView`:

- Die webViewDidStartLoad-Methode wird dem Ereignis [UIWebView.LoadStarted](xref:UIKit.UIWebView.LoadStarted) zugeordnet.
- Die webViewDidFinishLoad-Methode wird dem Ereignis [UIWebView.LoadFinished](xref:UIKit.UIWebView.LoadFinished) zugeordnet.
- Die webView:didFailLoadWithError-Methode wird dem Ereignis [UIWebView.LoadError](xref:UIKit.UIWebView.LoadError) zugeordnet.

Dieses einfache Programm zeichnet beispielsweise die Start- und Endzeit beim Laden einer Webansicht auf:

```csharp
DateTime startTime, endTime;
var web = new UIWebView (new CGRect (0, 0, 200, 200));
web.LoadStarted += (o, e) => startTime = DateTime.Now;
web.LoadFinished += (o, e) => endTime = DateTime.Now;
```

##### <a name="via-properties"></a>Über Eigenschaften

Ereignisse sind nützlich, wenn es mehr als einen Abonnenten des Ereignisses geben kann. Außerdem sind Ereignisse auf Fälle beschränkt, in denen der Code keinen Rückgabewert liefert.

Für Fälle, in denen der Code einen Wert zurückgeben soll, haben wir uns stattdessen für Eigenschaften entschieden. Das bedeutet, dass zu einem bestimmten Zeitpunkt in einem Objekt nur eine Methode festgelegt werden kann.

Sie können diesen Mechanismus anwenden, um z. B. die Tastatur auf dem Bildschirm in einem Handler für ein `UITextField` zu schließen:

```csharp
void SetupTextField (UITextField tf)
{
    tf.ShouldReturn = delegate (textfield) {
        textfield.ResignFirstResponder ();
        return true;
    }
}
```

Die `ShouldReturn`-Eigenschaft von `UITextField` verwendet in diesem Fall als Argument einen Delegaten, der einen booleschen Wert zurückgibt und bestimmt, ob das Textfeld bei gedrückter EINGABETASTE etwas tun soll. In unserer Methode geben wir an den Aufrufer *TRUE* zurück. Wir entfernen aber auch die Tastatur vom Bildschirm (dies geschieht, wenn das Textfeld `ResignFirstResponder` aufruft).

##### <a name="strongly-typed-via-a-delegate-property"></a>Stark typisiert über eine Delegateigenschaft

Wenn Sie keine Ereignisse verwenden möchten, können Sie Ihre eigene Unterklasse [UIWebViewDelegate](xref:UIKit.UIWebViewDelegate) bereitstellen und sie der [UIWebView.Delegate](xref:UIKit.UIWebView.Delegate)-Eigenschaft zuweisen. Nachdem UIWebView.Delegate zugewiesen wurde, funktioniert der Ereignisverteilungsmechanismus von UIWebView nicht mehr. Die UIWebViewDelegate-Methoden werden aufgerufen, sobald die entsprechenden Ereignisse eintreten.

Dieser einfache Typ erfasst z. B. die Zeit, die zum Laden einer Webansicht benötigt wird:

```csharp
class Notifier : UIWebViewDelegate  {
    DateTime startTime, endTime;

    public override LoadStarted (UIWebView webview)
    {
        startTime = DateTime.Now;
    }

    public override LoadingFinished (UIWebView webView)
    {
        endTime= DateTime.Now;
    }
}
```

Obiges wird in Code wie dem folgenden verwendet:

```csharp
var web = new UIWebView (new CGRect (0, 0, 200, 200));
web.Delegate = new Notifier ();
```

Der obige Code erstellt einen UIWebViewer und weist ihn an, Nachrichten an eine Instanz der Notifier-Klasse zu senden, die wir zum Antworten auf Nachrichten erstellt haben.

Dieses Muster wird auch befolgt, um das Verhalten für bestimmte Steuerelemente zu steuern. Im Fall von UIWebView ermöglicht beispielsweise die Eigenschaft [UIWebView.ShouldStartLoad](xref:UIKit.UIWebView.ShouldStartLoad) der `UIWebView`-Instanz zu steuern, ob `UIWebView` eine Seite laden soll oder nicht.

Das Muster wird auch verwendet, um die Daten bedarfsgesteuert für einige wenige Steuerelemente bereitzustellen. Beispielsweise ist das Steuerelement [UITableView](xref:UIKit.UITableView) ein leistungsstarkes Steuerelement zum Rendern von Tabellen. Aussehen und Inhalt werden von einer Instanz von [UITableViewDataSource](xref:UIKit.UITableViewDataSource) gesteuert.

### <a name="loosely-typed-via-the-weakdelegate-property"></a>Schwach typisiert über die WeakDelegate-Eigenschaft

Zusätzlich zur stark typisierten Eigenschaft gibt es auch einen schwach typisierten Delegaten, der dem Entwickler auf Wunsch eine andere Bindung ermöglicht.
Überall dort, wo in der Bindung von Xamarin.iOS eine stark typisierte `Delegate`-Eigenschaft verfügbar gemacht wird, ist auch eine entsprechende `WeakDelegate`-Eigenschaft verfügbar.

Wenn Sie `WeakDelegate` verwenden, sind Sie für das ordnungsgemäße Ergänzen Ihrer Klasse mit dem [Export](xref:Foundation.ExportAttribute)-Attribut zur Angabe des Selektors zuständig. Zum Beispiel:

```csharp
class Notifier : NSObject  {
    DateTime startTime, endTime;

    [Export ("webViewDidStartLoad:")]
    public void LoadStarted (UIWebView webview)
    {
        startTime = DateTime.Now;
    }

    [Export ("webViewDidFinishLoad:")]
    public void LoadingFinished (UIWebView webView)
    {
        endTime= DateTime.Now;
    }
}

[...]

var web = new UIWebView (new CGRect (0, 0, 200, 200));
web.WeakDelegate = new Notifier ();
```

Beachten Sie, dass nach Zuweisung der `WeakDelegate`-Eigenschaft die `Delegate`-Eigenschaft nicht mehr verwendet wird. Wenn Sie darüber hinaus die Methode in einer geerbten Basisklasse implementieren, die Sie [exportieren] möchten, müssen Sie sie zu einer öffentlichen Methode machen.

## <a name="mapping-of-the-objective-c-delegate-pattern-to-c"></a>Zuordnen des Delegatmusters von Objective-C zu C\#:

Wenn Sie Objective-C-Beispiele wie die folgenden sehen:

```objc
foo.delegate = [[SomethingDelegate] alloc] init]
```

Dadurch wird die Programmiersprache angewiesen, eine Instanz der SomethingDelegate-Klasse zu erstellen und zu konstruieren und den Wert der Delegateigenschaft der Variablen „foo“ zuzuweisen. Dieser Mechanismus wird von Xamarin.iOS unterstützt, und in C# ist die Syntax:

```csharp
foo.Delegate = new SomethingDelegate ();
```

In Xamarin.iOS haben wir stark typisierte Klassen bereitgestellt, die den Delegatklassen von Objective-C zugeordnet sind. Um sie zu verwenden, müssen Sie für die von der Xamarin.iOS-Implementierung definierten Methoden Unterklassen erstellen und die Methoden überschreiben. Weitere Informationen zur Funktionsweise finden Sie im Abschnitt „Modelle“ weiter unten.

### <a name="mapping-delegates-to-c"></a>Zuordnen von Delegaten zu C\#

UIKit verwendet im Allgemeinen Objective-C-Delegaten in zwei Formen.

Die erste Form bietet eine Schnittstelle zum Modell einer Komponente. Zum Beispiel als Mechanismus zur bedarfsgerechten Bereitstellung von Daten für eine Ansicht, wie z. B. die Datenspeichervorrichtung für eine Listenansicht.  In diesen Fällen müssen Sie stets eine Instanz der passenden Klasse erzeugen und die Variable zuweisen.

Im folgenden Beispiel stellen wir `UIPickerView` eine Implementierung für ein Modell zur Verfügung, das Zeichenfolgen verwendet:

```csharp
public class SampleTitleModel : UIPickerViewTitleModel {

    public override string TitleForRow (UIPickerView picker, nint row, nint component)
    {
        return String.Format ("At {0} {1}", row, component);
    }
}

[...]

pickerView.Model = new MyPickerModel ();
```

Die zweite Form ist das Bereitstellen einer Benachrichtigung zu Ereignissen. In diesen Fällen stellen wir zwar immer noch die API in der zuvor beschriebenen Form zur Verfügung, aber auch C#-Ereignisse bereit, die für schnelle Vorgänge einfacher zu verwenden und mit anonymen Delegaten und Lambdaausdrücken in C# integriert sein sollten.

Beispielsweise können Sie `UIAccelerometer`-Ereignisse abonnieren:

```csharp
UIAccelerometer.SharedAccelerometer.Acceleration += (sender, args) => {
   UIAcceleration acc = args.Acceleration;
   Console.WriteLine ("Time={0} at {1},{2},{3}", acc.Time, acc.X, acc.Y, acc.Z);
}
```

Die beiden Optionen sind dort verfügbar, wo es sinnvoll ist, aber als Programmierer müssen Sie sich für eine der beiden entscheiden. Wenn Sie Ihre eigene Instanz eines stark typisierten Responders/Delegaten erstellen und zuweisen, funktionieren die C#-Ereignisse nicht. Bei Verwendung der C#-Ereignisse werden die Methoden in Ihrer Klasse des Typs „Responder/Delegat“ nicht aufgerufen.

Das vorherige Beispiel, in dem `UIWebView` verwendet wurde, kann mit C# 3.0-Lambdaausdrücken wie folgt geschrieben werden:

```csharp
var web = new UIWebView (new CGRect (0, 0, 200, 200));
web.LoadStarted += () => { startTime = DateTime.Now; }
web.LoadFinished += () => { endTime = DateTime.Now; }
```

#### <a name="responding-to-events"></a>Reagieren auf Ereignisse

Im Objective-C-Code werden mitunter Ereignishandler für mehrere Steuerelemente und Anbieter von Informationen für mehrere Steuerelemente in derselben Klasse gehostet. Dies ist möglich, weil Klassen auf Nachrichten reagieren, und solange Klassen auf Nachrichten reagieren, ist es möglich, Objekte miteinander zu verknüpfen.

Wie bereits erwähnt, unterstützt Xamarin.iOS sowohl das ereignisbasierte C#-Programmiermodell als auch das Objective-C-Delegatmuster. Gemäß diesem können Sie eine neue Klasse erstellen, die den Delegaten implementiert und die gewünschten Methoden überschreibt.

Es ist auch möglich, das Muster von Objective-C zu unterstützen, bei dem die Responder für mehrere verschiedene Vorgänge alle in derselben Instanz einer Klasse gehostet werden. Dazu müssen Sie jedoch die Features auf niedriger Ebene der Xamarin.iOS-Bindung verwenden.

Wenn Sie beispielsweise möchten, dass Ihre Klasse sowohl auf die Nachricht `UITextFieldDelegate.textFieldShouldClear` als auch auf die Nachricht `UIWebViewDelegate.webViewDidStartLoad` in derselben Instanz einer Klasse reagiert, müssen Sie die Deklaration des Attributs [Export] verwenden:

```csharp
public class MyCallbacks : NSObject {
    [Export ("textFieldShouldClear:"]
    public bool should_we_clear (UITextField tf)
    {
        return true;
    }

    [Export ("webViewDidStartLoad:")]
    public void OnWebViewStart (UIWebView view)
    {
        Console.WriteLine ("Loading started");
    }
}
```

Die C#-Namen für die Methoden sind nicht wichtig. Ausschlaggebend sind lediglich die an das Attribut [Export] übergebenen Zeichenfolgen.

Bei dieser Art der Programmierung muss sichergestellt werden, dass die C#-Parameter mit den tatsächlichen Typen übereinstimmen, die die Runtime-Engine übergibt.

#### <a name="models"></a>Modelle

In UIKit-Speichervorrichtungen oder -Respondern, die mit Hilfsklassen implementiert sind, werden diese normalerweise im Objective-C-Code als Delegaten bezeichnet und als Protokolle implementiert.

Objective-C-Protokolle sind wie Schnittstellen, unterstützen aber optionale Methoden. Das heißt, dass nicht alle Methoden implementiert sein müssen, damit das Protokoll funktioniert.

Es gibt zwei Möglichkeiten, ein Modell zu implementieren. Sie können es entweder manuell implementieren oder die vorhandenen stark typisierten Definitionen verwenden.

Der manuelle Mechanismus ist notwendig, wenn Sie versuchen, eine Klasse zu implementieren, die nicht an Xamarin.iOS gebunden ist. Das ist ganz einfach:

- Markieren Sie Ihre Klasse für die Registrierung bei der Runtime.
- Wenden Sie das Attribut [Export] mit dem tatsächlichen Selektornamen auf jede Methode an, die Sie überschreiben möchten.
- Instanziieren Sie die Klasse, und übergeben Sie sie.

Beispielsweise implementiert der folgende Code nur eine der optionalen Methoden in der Definition des UIApplicationDelegate-Protokolls:

```csharp
public class MyAppController : NSObject {
        [Export ("applicationDidFinishLaunching:")]
        public void FinishedLaunching (UIApplication app)
        {
                SetupWindow ();
        }
}
```

Der Name des Objective-C-Selektors (applicationDidFinishLaunching:) wird mit dem Export-Attribut deklariert und die Klasse mit dem `[Register]`-Attribut registriert.

Xamarin.iOS bietet einsatzbereite, stark typisierte Deklarationen, die keine manuelle Bindung erfordern. Um dieses Programmiermodell zu unterstützen, unterstützt die Xamarin. iOS-Runtime in einer Klassendeklaration das Attribut [Model]. Dies informiert die Runtime, dass sie nicht alle Methoden der Klasse verknüpfen soll, es sei denn, die Methoden sind explizit implementiert.

Das bedeutet, dass in UIKit die Klassen, die ein Protokoll mit optionalen Methoden darstellen, wie folgt geschrieben werden:

```csharp
[Model]
public class SomeViewModel : NSObject {
    [Export ("someMethod:")]
    public virtual int SomeMethod (TheView view) {
       throw new ModelNotImplementedException ();
    }
    ...
}
```

Wenn Sie ein Modell implementieren möchten, das nur einige der Methoden implementiert, müssen Sie nur die Methoden, die Sie interessieren, überschreiben und die anderen Methoden ignorieren. Die Runtime verbindet nur die überschriebenen Methoden, nicht die ursprünglichen Methoden mit Objective-C.

Die Entsprechung des vorherigen manuellen Beispiels ist:

```csharp
public class AppController : UIApplicationDelegate {
    public override void FinishedLaunching (UIApplication uia)
    {
     ...
    }
}
```

Die Vorteile sind, dass es nicht notwendig ist, die Objective-C-Headerdateien zu durchsuchen, um den Selektor, die Typen der Argumente oder die Zuordnung zu C# zu finden, und dass Sie in Visual Studio für Mac Intellisense zusammen mit starken Typen erhalten.

#### <a name="xib-outlets-and-c"></a>XIB-Outlets und C\#

> [!IMPORTANT]
> In diesem Abschnitt wird die IDE-Integration mit Outlets bei Verwendung von XIB-Dateien erklärt. Wenn Sie den Xamarin Designer für iOS verwenden, wird dies alles durch die Eingabe eines Namens unter **Identität > Name** im Abschnitt „Eigenschaften“ Ihrer IDE ersetzt, wie unten gezeigt:
>
> [![](images/designeroutlet.png "Entering an item Name in the iOS Designer")](images/designeroutlet.png#lightbox)
>
>Weitere Informationen zum iOS Designer finden Sie unter [IOS Designer-Grundlagen](~/ios/user-interface/designer/introduction.md#how-it-works).

Dies ist eine detaillierte Beschreibung der Integration von Outlets in C#, die für fortgeschrittene Benutzer von Xamarin.iOS bereitgestellt wird. Bei Verwendung von Visual Studio für Mac erfolgt die Zuordnung automatisch hinter den Kulissen mit dynamisch für Sie generiertem Code.

Wenn Sie mit Interface Builder Ihre Benutzeroberfläche entwerfen, gestalten Sie lediglich das Aussehen der Anwendung und stellen einige Standardverbindungen her. Wenn Sie Informationen programmgesteuert abrufen, das Verhalten eines Steuerelements zur Laufzeit ändern oder das Steuerelement zur Laufzeit modifizieren möchten, müssen einige der Steuerelemente an Ihren verwalteten Code gebunden werden.

Dies erfolgt in wenigen Schritten:

1. Fügen Sie die **Outlet-Deklaration** dem **Besitzer Ihrer Datei** hinzu.
1. Verbinden Sie Ihr Steuerelement mit dem **Besitzer der Datei**.
1. Speichern Sie die Benutzeroberfläche sowie die Verbindungen in Ihrer XIB-/NIB-Datei.
1. Laden Sie die NIB-Datei zur Laufzeit.
1. Greifen Sie auf die Outlet-Variable zu.

Die Schritte 1 bis 3 werden in der Apple-Dokumentation zur Erstellung von Benutzeroberflächen mit Interface Builder behandelt.

Wenn Sie Xamarin.iOS verwenden, muss Ihre Anwendung eine von UIViewController abgeleitete Klasse erstellen. Sie wird wie folgt implementiert:

```csharp
public class MyViewController : UIViewController {
    public MyViewController (string nibName, NSBundle bundle) : base (nibName, bundle)
    {
        // You can have as many arguments as you want, but you need to call
        // the base constructor with the provided nibName and bundle.
    }
}
```

Um dann Ihren ViewController aus einer NIB-Datei zu laden, gehen Sie so vor:

```csharp
var controller = new MyViewController ("HelloWorld", NSBundle.MainBundle, this);
```

Dadurch wird die Benutzeroberfläche aus dem NIB geladen. Um nun auf die Outlets zuzugreifen, ist es notwendig, die Runtime darüber zu informieren, dass wir auf sie zugreifen möchten. Dazu muss die Unterklasse `UIViewController` die Eigenschaften deklarieren und sie mit dem Attribut [Connect] versehen. Und zwar so:

```csharp
[Connect]
UITextField UserName {
    get {
        return (UITextField) GetNativeField ("UserName");
    }
    set {
        SetNativeField ("UserName", value);
    }
}
```

Die Eigenschaftsimplementierung ist diejenige, die den Wert für den eigentlichen nativen Typ tatsächlich abruft und speichert.

Bei Verwendung von Visual Studio für Mac und Interface Builder müssen Sie sich darüber keine Gedanken machen. Visual Studio für Mac spiegelt automatisch alle deklarierten Outlets mit Code in einer Teilklasse, die als Bestandteil Ihres Projekts kompiliert wird.

#### <a name="selectors"></a>Selektoren

Ein Kernkonzept bei der Objective-C-Programmierung sind Selektoren. Häufig werden Sie auf APIs stoßen, bei denen Sie einen Selektor übergeben müssen oder die erwarten, dass Ihr Code auf einen Selektor antwortet.

Das Erstellen neuer Selektoren in C# ist ganz einfach. Sie erstellen dazu eine neue Instanz der Klasse `ObjCRuntime.Selector` und verwenden das Ergebnis an einer beliebigen Stelle in der API, die dies erfordert. Zum Beispiel:

```csharp
var selector_add = new Selector ("add:plus:");
```

Damit eine C#-Methode auf einen Selektoraufruf antwortet, muss sie vom Typ `NSObject` erben. Die C#-Methode muss mit dem Namen des Selektors über das Attribut `[Export]` ergänzt werden. Zum Beispiel:

```csharp
public class MyMath : NSObject {
    [Export ("add:plus:")]
    int Add (int first, int second)
    {
         return first + second;
    }
}
```

Beachten Sie, dass die Namen der Selektoren genau übereinstimmen **müssen**, einschließlich aller zwischengeschalteten und nachgestellten Doppelpunkte (":"), sofern vorhanden.

#### <a name="nsobject-constructors"></a>NSObject-Konstruktoren

Die meisten Klassen in Xamarin.iOS, die von `NSObject` abgeleitet sind, machen Konstruktoren verfügbar, die spezifisch für die Funktionalität des Objekts sind. Sie machen aber auch diverse Konstruktoren verfügbar, die nicht sofort offensichtlich sind.

Die Konstruktoren werden wie folgt verwendet:

```csharp
public Foo (IntPtr handle)
```

Dieser Konstruktor wird verwendet, um Ihre Klasse zu instanziieren, wenn die Runtime Ihre Klasse einer nicht verwalteten Klasse zuordnen muss. Dies geschieht, wenn Sie eine XIB-/NIB-Datei laden.  Zu diesem Zeitpunkt hat die Objective-C-Runtime ein Objekt auf der nicht verwalteten Seite erstellt. Dieser Konstruktor wird aufgerufen, um die verwaltete Seite zu initialisieren.

Normalerweise müssen Sie lediglich den Basiskonstruktor mit dem Handleparameter aufrufen und im Text alle notwendigen Initialisierungen vornehmen.

```csharp
public Foo ()
```

Dies ist der Standardkonstruktor einer Klasse. In den von Xamarin.iOS bereitgestellten Klassen initialisiert dieser die Foundation.NSObject-Klasse und alle dazwischen liegenden Klassen. Zum Schluss erfolgt die Verkettung mit der Objective-C-Methode `init` für die Klasse.

```csharp
public Foo (NSObjectFlag x)
```

Dieser Konstruktor dient zum Initialisieren der Instanz, verhindert aber, dass der Code am Schluss die Objective-C-Methode „init“ aufruft. Diese verwenden Sie normalerweise, wenn Sie sich bereits für die Initialisierung registriert haben (bei Verwendung von `[Export]` in Ihrem Konstruktor) oder die Initialisierung bereits auf einem anderen Weg erfolgt ist.

```csharp
public Foo (NSCoder coder)
```

Dieser Konstruktor ist für die Fälle vorgesehen, in denen das Objekt von einer NSCoding-Instanz initialisiert wird.

#### <a name="exceptions"></a>Ausnahmen

Das Xamarin.iOS-API-Design löst Objective-C-Ausnahmen nicht als C#-Ausnahmen aus. Das Design erzwingt, dass überhaupt kein Garbage an die Objective-C gesendet wird und dass alle Ausnahmen, die produziert werden müssen, von der Bindung selbst erzeugt werden, bevor ungültige Daten überhaupt an die Objective-C übergeben werden.

#### <a name="notifications"></a>Benachrichtigungen

Sowohl in iOS als auch in OS X können Entwickler Benachrichtigungen abonnieren, die von der zugrunde liegenden Plattform gesendet werden. Dies erfolgt mithilfe der `NSNotificationCenter.DefaultCenter.AddObserver`-Methode. Die `AddObserver`-Methode verwendet zwei Parameter. Der eine ist die Benachrichtigung, die Sie abonnieren möchten, der andere die Methode, die aufgerufen wird, sobald die Benachrichtigung ausgelöst wird.

Sowohl in Xamarin.iOS als auch in Xamarin.Mac werden die Schlüssel für die verschiedenen Benachrichtigungen in der Klasse gehostet, die die Benachrichtigungen auslöst. Beispielsweise werden die durch `UIMenuController` ausgelösten Benachrichtigungen als `static NSString`-Eigenschaften in den `UIMenuController`-Klassen gehostet, die mit dem Namen „Notification“ enden.

### <a name="memory-management"></a>Speicherverwaltung

Xamarin.iOS verfügt über einen Garbage Collector, der die Freigabe von Ressourcen für Sie übernimmt, falls diese nicht mehr genutzt werden. Zusätzlich zum Garbage Collector implementieren alle von `NSObject` abgeleiteten Objekte die Schnittstelle `System.IDisposable`.

#### <a name="nsobject-and-idisposable"></a>NSObject und IDisposable

Das Verfügbarmachen der Schnittstelle `IDisposable` ist eine praktische Möglichkeit, Entwicklern bei der Freigabe von Objekten zu helfen, die möglicherweise große Speicherblöcke (z. B. kann `UIImage` wie ein harmloser Zeiger aussehen, aber auf ein 2 Megabyte großes Bild zeigen) und andere wichtige und endliche Ressourcen (wie einen Videodecodierungspuffer) kapseln.

NSObject implementiert die IDisposable-Schnittstelle und auch das [Dispose-Muster von .NET](https://msdn.microsoft.com/library/fs2xkftw.aspx). Dadurch können Entwickler, die für NSObject eine Unterklasse erstellen, das Dispose-Verhalten außer Kraft setzen und bei Bedarf ihre eigenen Ressourcen freigeben. Betrachten Sie zum Beispiel diesen Ansichtscontroller, der um eine Reihe von Bildern beibehalten wird:

```csharp
class MenuViewController : UIViewController {
    UIImage breakfast, lunch, dinner;
    [...]
    public override void Dispose (bool disposing)
    {
        if (disposing){
             if (breakfast != null) breakfast.Dispose (); breakfast = null;
             if (lunch != null) lunch.Dispose (); lunch = null;
             if (dinner != null) dinner.Dispose (); dinner = null;
        }
        base.Dispose (disposing)
    }
}
```

Wenn ein verwaltetes Objekt verworfen wird, ist es nicht mehr nützlich. Möglicherweise haben Sie immer noch einen Verweis auf die Objekte, aber das Objekt ist zu diesem Zeitpunkt in jeder Hinsicht ungültig. Einige .NET-APIs stellen dies sicher, indem sie eine ObjectDisposedException auslösen, wenn Sie z. B. versuchen, auf Methoden für ein verworfenes Objekt zuzugreifen:

```csharp
var image = UIImage.FromFile ("demo.png");
image.Dispose ();
image.XXX = false;  // this at this point is an invalid operation
```

Selbst wenn Sie immer noch auf die Variable „image“ zugreifen können, handelt es sich in Wirklichkeit um einen ungültigen Verweis, der nicht mehr auf das Objective-C-Objekt zeigt, das das Bild enthielt.

Das Löschen eines Objekts in C# bedeutet jedoch nicht, dass es notwendigerweise zerstört wird. Alles, was Sie tun müssen, ist den Verweis von C# auf das Objekt freigeben. Es ist möglich, dass die Cocoa-Umgebung für eigene Zwecke einen Verweis beibehalten hat. Wenn Sie z. B. die Image-Eigenschaft von UIImageView auf ein Bild festlegen und dann das Bild entfernen, hat die zugrunde liegende UIImageView einen eigenen Verweis vorgenommen und behält einen Verweis auf dieses Objekt, bis sie es nicht mehr verwendet.

#### <a name="when-to-call-dispose"></a>Wann sollte „Dispose“ aufgerufen werden?

Sie sollten „Dispose“ anrufen, wenn Sie Mono zum Entfernen des Objekts benötigen. Ein möglicher Anwendungsfall ist, wenn Mono nicht weiß, dass Ihr NSObject tatsächlich einen Verweis auf eine wichtige Ressource wie Arbeitsspeicher oder einen Informationspool hält. In diesen Fällen müssen Sie „Dispose“ anrufen, um den Verweis auf den Arbeitsspeicher sofort freizugeben, anstatt darauf zu warten, dass Mono einen Garbage-Collection-Zyklus durchläuft.

Wenn Mono intern [NSString-Verweise anhand von C#-Zeichenfolgen](~/ios/internals/api-design/nsstring.md) erstellt, werden diese sofort gelöscht, um den Arbeitsaufwand für den Garbage Collector zu reduzieren. Je weniger Objekte zu bewältigen sind, desto schneller läuft die Garbage Collection.

#### <a name="when-to-keep-references-to-objects"></a>Wann sollen Verweise auf Objekte beibehalten werden?

Eine Nebenwirkung der automatischen Arbeitsspeicherverwaltung besteht darin, dass die Garbage Collection unbenutzte Objekte entsorgt, solange es keine Verweise auf sie gibt. Dies kann mitunter überraschende Nebenfolgen haben, z. B. wenn Sie eine lokale Variable erstellen, die Ihren obersten Ansichtscontroller oder Ihr oberstes Fenster enthält, und diese dann einfach im Hintergrund verschwinden.

Wenn Sie in Ihren statischen oder Instanzvariablen keinen Verweis auf Ihre Objekte beibehalten, ruft Mono gerne die Dispose()-Methode für diese Objekte auf, woraufhin der Verweis auf das Objekt freigegeben wird. Da dies möglicherweise der einzige ausstehende Verweis ist, wird die Objective-C-Runtime das Objekt für Sie zerstören.

## <a name="related-links"></a>Verwandte Links

- [Binden von Feldern](~/cross-platform/macios/binding/objective-c-libraries.md#Binding_Fields)
