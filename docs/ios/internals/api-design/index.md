---
title: Xamarin.iOS-API-Design
description: Dieses Dokument beschreibt einige grundlegende Prinzipien, mit dem Entwerfen der Xamarin.iOS-APIs und diese Beziehung zum Objective-c
ms.prod: xamarin
ms.assetid: 322D2724-AF27-6FFE-BD21-AA1CFE8C0545
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/21/2017
ms.openlocfilehash: cd25e5c78885f53902c577a900958b842a70219c
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50116952"
---
# <a name="xamarinios-api-design"></a>Xamarin.iOS-API-Design

Zusätzlich zu den Basisklassenbibliotheken, die Teil von Mono, [Xamarin.iOS](http://www.xamarin.com/iOS) im Lieferumfang von Bindungen für verschiedene iOS-APIs, um Entwicklern das Erstellen von native iOS-Anwendungen mit Mono zu ermöglichen.

Den Kern von Xamarin.iOS, besteht eine interop-Engine, die die c#-Welt mit den Objective-C-Welt als auch Bindungen für die iOS-C-basierte APIs wie CoreGraphics verbindet und [OpenGL ES](#OpenGLES).

Die Low-Level-Laufzeit für die Kommunikation mit Objective-C-Code ist in [MonoTouch.ObjCRuntime](#MonoTouch.ObjCRuntime). Zusätzlich zu diesem, Bindungen für [Foundation](#MonoTouch.Foundation), CoreFoundation, und [UIKit](#MonoTouch.UIKit) bereitgestellt werden.

## <a name="design-principles"></a>Entwurfsprinzipien

Dies sind einige unserer Entwurfsprinzipien für die Xamarin.iOS-Bindungen (gelten auch für Xamarin.Mac, die Mono-Bindungen für Objective-C unter MacOS):

- Führen Sie die [Framework-Entwurfsrichtlinien](https://docs.microsoft.com/dotnet/standard/design-guidelines)
- Ermöglichen Sie es Entwicklern, Unterklasse Objective-C-Klassen:

  - Von einer vorhandenen Klasse abgeleitet werden
  - Eine Verkettung der Basiskonstruktor aufrufen.
  - Überschreiben von Methoden sollte mit # außer Kraft setzen-System ausgeführt werden
  - Unterklassen sollten mit C#-standard-Konstrukte verwendet werden.

- Machen Sie nicht die Entwickler Objective-C-Selektoren
- Geben Sie einen Mechanismus zum Aufrufen von beliebiger Objective-C-Bibliotheken
- Ermöglichen Sie die Objective-C-Aufgaben schwierig und einfach Objective-C-Aufgaben
- Machen Sie Objective-C-Eigenschaften verfügbar, als C#-Eigenschaften
- Machen Sie eine stark typisierte API verfügbar:

  - Höhere typsicherheit
  - Minimieren von Fehlern der Common Language runtime
  - Abrufen von IDE IntelliSense für Rückgabetypen
  - Ermöglicht die IDE-Popup-Dokumentation

- Empfehlen Sie in der IDE Durchsuchen von APIs:

  - Beispiel: anstelle von verfügbar machen eine schwach typisierte Array wie folgt aus:
    
    ```objc
    NSArray *getViews
    ```
    Machen Sie einen starken Typ, wie folgt:
    
    ```csharp
    NSView [] Views { get; set; }
    ```
    
    Dies bietet Visual Studio für Mac die Möglichkeit zur automatischen Vervollständigung beim Durchsuchen der API, die alle die `System.Array` Vorgänge, die für den zurückgegebenen Wert und verfügbar sind und den Rückgabewert zur Teilnahme an LINQ ermöglicht.

- Systemeigene C#-Typen:

  - [`NSString` wird `string`](~/ios/internals/api-design/nsstring.md)
  - Aktivieren Sie `int` und `uint` Parameter, die Enumerationen in C#-Enumerationen und C#-Enumerationen mit wurden sollten `[Flags]` Attribute
  - Anstelle von Typ neutrale `NSArray` Objekte verfügbar machen, Arrays als stark typisierte Arrays.
  - Bieten Sie für Ereignisse und Benachrichtigungen Benutzern die Möglichkeit, zwischen:

    - Eine stark typisierte Version standardmäßig
    - Eine schwach typisierte Version für erweiterte Anwendungsfälle.

- Unterstützen Sie die Objective-C-Delegat-Muster:

    - C#-Ereignissystem
    - Bereitstellen der c#-Delegaten (Lambdas, anonyme Methoden und `System.Delegate`) Objective-C-APIs als Blöcke

### <a name="assemblies"></a>Assemblys

Xamarin.iOS enthält eine Reihe von Assemblys, die bilden die *Xamarin.iOS Profil*. Die [Assemblys](~/cross-platform/internals/available-assemblies.md) Seite enthält weitere Informationen.

### <a name="major-namespaces"></a>Major-Namespaces 

<a name="MonoTouch.ObjCRuntime" />

#### <a name="objcruntime"></a>ObjCRuntime

Die [ObjCRuntime](https://developer.xamarin.com/api/namespace/ObjCRuntime/) Namespace ermöglicht den Entwicklern, die Welt zwischen c# und Objective-c zu überbrücken.
Dies ist eine neue Bindung, die speziell für iOS, basierend auf der Erfahrung von Cocoa- und GTK#-entworfen wurde.

<a name="MonoTouch.Foundation" />

#### <a name="foundation"></a>Foundation

Die [Foundation](https://developer.xamarin.com/api/namespace/Foundation/) -Namespace stellt die grundlegenden Datentypen entworfen, mit dem Objective-C-Foundation-Framework zusammenarbeiten, die Teil des iOS ist und die Basis für objektorientierte Programmierung in Objective-c ist bereit.

Xamarin.iOS spiegelt in c# die Hierarchie von Klassen, die in Objective-c Z. B. die Objective-C-Basisklasse [NSObject](http://developer.apple.com/iphone/library/documentation/Cocoa/Reference/Foundation/Classes/NSObject_Class/Reference/Reference.html) ist sowohl in c# über [Foundation.NSObject](https://developer.xamarin.com/api/type/Foundation.NSObject/).

Obwohl dieser Namespace Bindungen für die zugrunde liegenden Objective-C-Foundation-Typen enthält, in einigen Fällen haben wir die zugrunde liegende Typen zu .NET-Typen zugeordnet. Zum Beispiel:

- Anstelle von [NSString](http://developer.apple.com/iphone/library/documentation/Cocoa/Reference/Foundation/Classes/NSString_Class/Reference/NSString.html) und [nsarray im](https://developer.apple.com/library/ios/#documentation/Cocoa/Reference/Foundation/Classes/NSArray_Class/NSArray.html), die Common Language Runtime stellt diese als C#- [Zeichenfolge](xref:System.String)s und stark typisierte [Array](xref:System.Array)s in der gesamten die API.

- Verschiedene Hilfs-APIs sind hier, damit Entwickler zum Binden von Drittanbietern Objective-C-APIs, andere iOS-APIs oder APIs, die derzeit nicht von Xamarin.iOS gebunden sind verfügbar.

Weitere Informationen zu APIs binden, finden Sie unter den [Xamarin.iOS-Bindung Generator](~/cross-platform/macios/binding/binding-types-reference.md) Abschnitt.


##### <a name="nsobject"></a>NSObject

Die [NSObject](https://developer.xamarin.com/api/type/Foundation.NSObject/) Typ bildet die Grundlage für alle Objective-C-Bindungen. Xamarin.iOS-Typen zu spiegeln zwei Klassen von Typen aus dem iOS-CocoaTouch-APIs: die C-Typen (in der Regel bezeichneten als CoreFoundation Typen) und die Objective-C-Typen (diese alle von der NSObject-Klasse abgeleitet sind).

Für jeden Typ, der einen nicht verwalteten Typ entspricht, ist es möglich, das systemeigene Objekt durch Abrufen der [behandeln](https://developer.xamarin.com/api/property/Foundation.NSObject.Handle/) Eigenschaft.

Während Mono Garbagecollection für alle Objekte bereitstellen, wird die `Foundation.NSObject` implementiert die [System.IDisposable](xref:System.IDisposable) Schnittstelle. Dies bedeutet, dass Sie alle angegebenen NSObject die Ressourcen explizit freigeben können, ohne zu warten, bis der Garbage Collector Kick-in. Dies ist wichtig, wenn Sie hohe NSObjects, z. B. UIImages verwenden, die Zeiger auf die große Datenblöcke enthalten kann.

Wenn Ihr Typ deterministische Beendigung ausführen muss, überschreiben die [NSObject.Dispose(bool)-Methode](https://developer.xamarin.com/api/type/Foundation.NSObject/%2fM%2fDispose) Parameters Dispose-Methode ist "" bool "verwerfen", und legen Sie es "true" bedeutet, dass die Dispose-Methode aufgerufen wird da der Benutzer explizit aufgerufene Dispose () für das Objekt. Wenn der Wert auf "false" festgelegt ist, bedeutet dies, dass der Finalizer-Thread die Methode Dispose (Bool disposing) vom Finalizer aufgerufen wird. []()


##### <a name="categories"></a>Kategorien

Ab Xamarin.iOS 8.10 es ist möglich, Objective-C-Kategorien aus c# zu erstellen.

Dies erfolgt mithilfe der `Category` Attribut, und geben den Typ als Argument für das Attribut erweitern. Im folgende Beispiel wird z. B. NSString erweitern.

    [Category (typeof (NSString))]

Jede Kategorie-Methode verwendet den normalen Mechanismus zum Exportieren von Methoden in Objective-C mit der `Export` Attribut:

    [Export ("today")]
    public static string Today ()
    {
        return "Today";
    }

Alle verwalteten Erweiterungsmethoden müssen statisch sein, aber es ist möglich, Objective-C-Instanzmethoden, die unter Verwendung der Standardsyntax für die Erweiterungsmethoden im C# -Code zu erstellen:

    [Export ("toUpper")]
    public static string ToUpper (this NSString self)
    {
        return self.ToString ().ToUpper ();
    }

und das erste Argument für die Erweiterungsmethode ist die Instanz, auf der die Methode aufgerufen wurde.

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

In diesem Beispiel wird eine systemeigene "ToUpper" Instanz-Methode der NSString-Klasse hinzufügen die in Objective-c aufgerufen werden kann

```csharp
[Category (typeof (UIViewController))]
public static class MyViewControllerCategory
{
    [Export ("shouldAudoRotate")]
    static bool GlobalRotate ()
    {
        return true;
    }
}
```

Ein Szenario, in denen dies hilfreich ist, einen ganzen Satz von Klassen in Ihrer Codebasis eine Methode hinzugefügt wird, z. B. würde dies alle stellen `UIViewController` Instanzen melden, dass sie drehen können:

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

"Preserveattribute" ist ein benutzerdefiniertes Attribut, mit dem Teilen Mtouch – des Xamarin.iOS-Bereitstellungstools – einen Typ oder ein Member eines Typs beibehalten, während der Phase, wenn die Anwendung verarbeitet wird, um es zu verkleinern.

Jeder Member, der nicht statisch durch die Anwendung verknüpft ist, muss entfernt werden. Daher wird dieses Attribut verwendet, um Member zu kennzeichnen, sind nicht statisch verwiesen wird, aber immer noch von der Anwendung erforderlich sind.

Wenn Sie beispielsweise Typen dynamisch instanziieren, sollten Sie den Standardkonstruktor Ihrer Typen beibehalten. Wenn Sie die XML-Serialisierung verwenden, sollten Sie die Eigenschaften Ihrer Typen beibehalten.

Sie können dieses Attribut auf alle Member eines Typs oder auf den Typ selbst anwenden. Wenn Sie den gesamten Typ beibehalten möchten, können Sie die Syntax [beibehalten (AllMembers = True)] für den Typ.

<a name="MonoTouch.UIKit" />

#### <a name="uikit"></a>UIKit

Die [UIKit](https://developer.xamarin.com/api/namespace/UIKit/) -Namespace enthält eine 1: 1-Zuordnung für alle von der UI-Komponenten, aus denen CocoaTouch in Form von Klassen in c# besteht. Die API wurde geändert, um die Konventionen, die in der C#-Sprache verwendet.

C#-Delegaten werden für häufige Vorgänge bereitgestellt. Finden Sie unter den [Delegaten](#Delegates) Abschnitt, um weitere Informationen.

<a name="OpenGLES" />

#### <a name="opengles"></a>OpenGLES

Für OpenGLES, wir Verteilen einer [Version geändert](https://developer.xamarin.com/api/namespace/OpenTK/) von der [OpenTK](http://www.opentk.com/) -API, eine objektorientierte-Bindung mit OpenGL, die geändert wurde, um CoreGraphics-Datentypen und-Strukturen verwendet, als auch nur verfügbar zu machen die die Funktionalität, die unter iOS verfügbar ist.

OpenGLES-1.1-Funktion wird über den Typ ES11.GL dokumentiert [hier](https://developer.xamarin.com/api/type/OpenTK.Graphics.ES11.GL/) Typ.

OpenGLES-2.0-Funktion wird über den Typ ES20.GL dokumentiert [hier](https://developer.xamarin.com/api/type/OpenTK.Graphics.ES20.GL/) Typ.

OpenGLES-3.0-Funktion wird über den Typ ES30.GL dokumentiert [hier](https://developer.xamarin.com/api/type/OpenTK.Graphics.ES30.GL/) Typ.


### <a name="binding-design"></a>Design binden

Xamarin.iOS ist nicht lediglich eine Bindung an die zugrunde liegende Objective-C-Plattform. Es erweitert das Typensystem von .NET und Dispatchsystem besser Blend C#- und Objective-c

Genau wie P/Invoke ein nützliches Tool zum Aufrufen von systemeigener Bibliotheken unter Windows und Linux ist oder als IJW-Unterstützung für COM-Interop für Windows verwendet werden kann, erweitert Xamarin.iOS die Laufzeit zur Unterstützung von Bindungsobjekten c# für Objective-C-Objekte.

Die Diskussion in den nächsten Abschnitten ist nicht erforderlich für Benutzer, die Xamarin.iOS-Anwendungen erstellen, jedoch können Entwickler verstehen, wie Dinge fertig sind, und unterstützt sie beim Erstellen von komplexen Anwendungen.



#### <a name="types"></a>Typen

In denen es sinnvoll, vorgenommen werden c#-Typen anstelle von Low-Level Foundation-Typen, für das C#-Universum bereitgestellt.  Dies bedeutet, dass [die-API verwendet den C#-Typ "String" anstelle von NSString](~/ios/internals/api-design/nsstring.md) und verwendet stark typisierte c#-Arrays anstelle von nsarray im verfügbar zu machen.

Im Allgemeinen im Xamarin.iOS- und Xamarin.Mac-Entwurf die zugrunde liegende `NSArray` Objekt nicht verfügbar gemacht wird. Dagegen konvertiert die Laufzeit automatisch `NSArray`s, um stark typisierte Arrays einiger `NSObject` Klasse. Daher macht eine schwach typisierte Methode wie GetViews ein nsarray im zurückzugebenden von Xamarin.iOS nicht verfügbar:

```csharp
NSArray GetViews ();
```

Stattdessen stellt einen stark typisierten Wert zurück, wie dies von die Bindung zur Verfügung:

```csharp
UIView [] GetViews ();
```

Es sind einige der Methoden in `NSArray`, für die Sonderfälle, in denen Sie möglicherweise verwenden möchten, eine `NSArray` direkt, aber ihre Verwendung wird abgeraten, in der API-Bindung.

Darüber hinaus wird in der **Classic API** statt verfügbar zu machen `CGRect`, `CGPoint` und `CGSize` aus der API CoreGraphics ersetzten wir mit der `System.Drawing` Implementierungen `RectangleF`, `PointF`und `SizeF` , wie sie helfen würde erhalten Entwickler vorhandenen OpenGL-Code, OpenTK verwendet. Bei Verwendung der neuen 64-Bit- **Unified API**, sollte die CoreGraphics-API verwendet werden.

<a name="Inheritance" />

#### <a name="inheritance"></a>Vererbung

Der Xamarin.iOS-API-Entwurf kann Entwickler native Objective-C-Typen auf die gleiche Weise zu erweitern, dass sie einen C#-Typ mithilfe des Schlüsselworts "Override" in einer abgeleiteten Klasse als auch verketten Sie die grundlegende Implementierung, die mit dem "base" C#-Schlüsselwort auftreten würde.

Dieser Entwurf ermöglicht Entwicklern Umgang mit Objective-C-Selektoren als Teil von ihren Entwicklungsprozess zu vermeiden, da das gesamte Objective-C-System in die Xamarin.iOS-Bibliotheken bereits umschlossen wird.


#### <a name="types-and-interface-builder"></a>Typen und Interface Builder

Wenn Sie Klassen von .NET, die Instanzen von Typen, die von Interface Builder erstellt sind erstellen, müssen Sie einen Konstruktor bereit, die akzeptiert ein einzelnes `IntPtr` Parameter.
Dies ist erforderlich, um die Instanz verwaltetes Objekt mit dem nicht verwalteten Objekt zu binden.
Der Code besteht aus einer einzigen Zeile, wie folgt aus:

```csharp
public partial class void MyView : UIView {
   // This is the constructor that you need to add.
   public MyView (IntPtr handle) : base (handle) {}
}
```

<a name="Delegates" />


#### <a name="delegates"></a>Delegaten

Objective-C und c# haben verschiedene Bedeutungen für den Word-Delegaten, in der jeweiligen Sprache.

In der Objective-C-Welt und in der Dokumentation, die Sie online zu CocoaTouch gefunden werden, ist ein Delegat in der Regel eine Instanz einer Klasse, die auf eine Reihe von Methoden reagieren. Dies ähnelt sehr einer C#-Schnittstelle, mit dem Unterschied, dass die Methoden nicht immer erforderlich sind.

Diese Delegaten spielen eine wichtige Rolle bei UIKit und andere CocoaTouch-APIs. Sie werden verwendet, um verschiedene Aufgaben auszuführen:

-  Um Benachrichtigungen zu Ihrem Code (ähnlich wie Ereignisübermittlung in c# oder Gtk +) bereitzustellen.
-  So implementieren Sie Modelle für die Visualisierung Datensteuerelemente
-  Um das Verhalten eines Steuerelements zu steuern.


Das programmierschema wurde entwickelt, um die Erstellung von abgeleiteten Klassen zum Ändern des Verhaltens für ein Steuerelement zu minimieren. Diese Lösung ist ähnlich wie im Geiste, was andere GUI-Toolkits im Laufe der Jahre getan haben: GTK#s signalisiert, Qt-Slots "," Windows Forms-Ereignisse "," WPF/Silverlight-Ereignisse und so weiter. Um zu vermeiden, dass Hunderte von Schnittstellen (eine für jede Aktion) oder verlangt von Entwicklern, zu viele Methoden zu implementieren, die sie nicht benötigen, unterstützt Objective-C-Definitionen optional. Dies ist anders als C#-Schnittstellen, die alle Methoden implementiert werden müssen.

In Objective-C-Klassen, sehen Sie, dass Klassen, mit denen dieses Muster Programmierung eine Eigenschaft in der Regel mit dem Namen verfügbar machen `delegate`, dies ist erforderlich, um die erforderliche Teile der Schnittstelle und NULL oder mehr der optionalen Komponenten implementieren.

In Xamarin.iOS sind drei sich gegenseitig ausschließende Mechanismen zum Binden an diese Delegaten angeboten:

1.  [Über Ereignisse](#Via_Events).
2.  [Stark typisierte über eine `Delegate` Eigenschaft](#StrongDelegate)
3.  [Lose typisierte über eine `WeakDelegate` Eigenschaft](#WeakDelegate)

Betrachten Sie beispielsweise die [UIWebView](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebView_Class/Reference/Reference.html) Klasse. Dies sendet, um eine [UIWebViewDelegate](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebViewDelegate_Protocol/Reference/Reference.html) -Instanz, die zugewiesen ist die [Delegieren](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebView_Class/Reference/Reference.html#//apple_ref/occ/instp/UIWebView/delegate) Eigenschaft.

<a name="Via_Events" />

##### <a name="via-events"></a>Über Ereignisse

Für viele Typen Xamarin.iOS erstellt automatisch einen entsprechenden Delegaten die Weiterleiten der `UIWebViewDelegate` Aufrufe auf C#-Ereignissen. Für `UIWebView`:

-  Die [WebViewDidStartLoad](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebViewDelegate_Protocol/Reference/Reference.html#//apple_ref/occ/intfm/UIWebViewDelegate/webViewDidStartLoad:) -Methode zugeordnet ist, um die [UIWebView.LoadStarted](https://developer.xamarin.com/api/event/UIKit.UIWebView.LoadStarted/) Ereignis.
-  Die [WebViewDidFinishLoad](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebViewDelegate_Protocol/Reference/Reference.html#//apple_ref/occ/intfm/UIWebViewDelegate/webViewDidFinishLoad:) -Methode zugeordnet ist, um die [UIWebView.LoadFinished](https://developer.xamarin.com/api/event/UIKit.UIWebView.LoadFinished/) Ereignis.
-  Die [WebView:didFailLoadWithError](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebViewDelegate_Protocol/Reference/Reference.html#//apple_ref/occ/intfm/UIWebViewDelegate/webView:didFailLoadWithError:) -Methode zugeordnet ist, um die [UIWebView.LoadError](https://developer.xamarin.com/api/event/UIKit.UIWebView.LoadError/) Ereignis.

Dieses einfache Programm zeichnet z. B. die Start- und Endzeiten, die beim Laden von einer Web anzuzeigen:

```csharp
DateTime startTime, endTime;
var web = new UIWebView (new CGRect (0, 0, 200, 200));
web.LoadStarted += (o, e) => startTime = DateTime.Now;
web.LoadFinished += (o, e) => endTime = DateTime.Now;
```


##### <a name="via-properties"></a>VIA-Eigenschaften

Ereignisse sind nützlich, wenn dort mehr als einen Abonnenten für das Ereignis womöglich. Darüber hinaus sind Ereignisse auf die Fälle beschränkt, es kein Rückgabewert aus dem Code ist.

In Fällen, wo der Code erwartet wird, um einen Wert zurückzugeben, entschieden wir stattdessen für Eigenschaften. Dies bedeutet, dass nur eine Methode zu einem bestimmten Zeitpunkt in einem Objekt festgelegt werden kann.

Beispielsweise können Sie diesen Mechanismus verwenden, um die Tastatur auf dem Bildschirm auf den Handler für eine `UITextField`:

```csharp
void SetupTextField (UITextField tf)
{
    tf.ShouldReturn = delegate (textfield) {
        textfield.ResignFirstResponder ();
        return true;
    }
}
```

Die `UITextField`des `ShouldReturn` Eigenschaft in diesem Fall wird als Argument akzeptiert ein Delegat, der einen booleschen Wert zurückgibt, und bestimmt, ob das Textfeld mit der zurück-Schaltfläche gedrückt wird etwas tun sollte. In unserem-Methode, die wir zurückgeben *"true"* an den Aufrufer, aber wir entfernen auch die Tastatur auf dem Bildschirm (Dies geschieht, wenn die Textfield ruft `ResignFirstResponder`).

<a name="StrongDelegate"/>

##### <a name="strongly-typed-via-a-delegate-property"></a>Über eine Eigenschaft des Delegaten typisiert stark.

Wenn Sie keine Ereignisse verwenden möchten, Sie Ihren eigenen bereitstellen [UIWebViewDelegate](https://developer.xamarin.com/api/type/UIKit.UIWebViewDelegate/) Unterklasse und weisen sie Sie der [UIWebView.Delegate](https://developer.xamarin.com/api/property/UIKit.UIWebView.Delegate/) Eigenschaft. Sobald UIWebView.Delegate zugewiesen wurde, der Ereignismechanismus Dispatch UIWebView funktioniert nicht mehr, und die UIWebViewDelegate-Methoden werden aufgerufen werden, wenn die entsprechenden Ereignisse auftreten.

Dieser einfache Typ zeichnet z. B. das lange es, das dauert eine Webansicht zu laden:

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

Die oben genannten wird wie folgt in Code verwendet:

```csharp
var web = new UIWebView (new CGRect (0, 0, 200, 200));
web.Delegate = new Notifier ();
```

Die oben genannten erstellt eine UIWebViewer und wird angewiesen, sie zum Senden von Nachrichten mit einer Instanz von Änderungsbenachrichtigung, eine Klasse, die wir erstellt haben, um auf Nachrichten reagieren.

Dieses Muster wird auch verwendet, um das Verhalten bestimmter Steuerelemente, z. B. im Fall UIWebView steuern die [UIWebView.ShouldStartLoad](https://developer.xamarin.com/api/property/UIKit.UIWebView.ShouldStartLoad/) Eigenschaft ermöglicht die `UIWebView` Instanz, um zu steuern, ob die `UIWebView` lädt ein Seite oder nicht.

Das Muster wird auch verwendet, um die Daten nach Bedarf für einige Steuerelemente bereitzustellen. Z. B. die [UITableView](https://developer.xamarin.com/api/type/UIKit.UITableView/) Steuerelement ist ein leistungsfähiges Tabelle-Rendering-Steuerelement – und sowohl das Aussehen und den Inhalt von einer Instanz gesteuert werden eine [UITableViewDataSource](https://developer.xamarin.com/api/type/UIKit.UITableView/DataSource)

<a name="WeakDelegate"/>

### <a name="loosely-typed-via-the-weakdelegate-property"></a>Über die Eigenschaft WeakDelegate Typbindung flexibler

Zusätzlich zu den stark typisierten Eigenschaft gibt es auch ein schwacher typisierter Delegaten, der dem Entwickler erlaubt, Dinge anders zu binden, falls gewünscht.
Überall ein stark typisiertes `Delegate` -Eigenschaft verfügbar gemacht wird, in der Xamarin.iOS-Bindung, ein entsprechendes `WeakDelegate` -Eigenschaft ist ebenfalls verfügbar.

Bei Verwendung der `WeakDelegate`, Sie sind verantwortlich für ordnungsgemäß durch ergänzen der Klasse mit dem [exportieren](https://developer.xamarin.com/api/type/Foundation.ExportAttribute/) Attribut, um die Auswahl anzugeben. Zum Beispiel:

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

Beachten Sie, einmal die `WeakDelegate` Eigenschaft zugewiesen wurde, die `Delegate` Eigenschaft wird nicht verwendet werden. Darüber hinaus, wenn Sie die Methode in einer geerbten Klasse, die Sie [Exportieren] möchten implementieren, müssen Sie eine öffentliche Methode sein.


## <a name="mapping-of-the-objective-c-delegate-pattern-to-c35"></a>Zuordnen des Delegaten Objective-C-Musters in C&#35;

Wenn Sie finden Sie in Objective-C-Beispielen, die wie folgt aussehen:

```csharp
foo.delegate = [[SomethingDelegate] alloc] init]
```

Dadurch wird die Sprache zum Erstellen, und erstellen Sie eine Instanz der Klasse "SomethingDelegate", und weisen Sie den Wert der Eigenschaft Delegaten für die Variable "Foo" angewiesen. Dieser Mechanismus wird von Xamarin.iOS unterstützt, und die Syntax von c# ist:

```csharp
foo.Delegate = new SomethingDelegate ();
```

Xamarin.iOS enthält bereitgestellte-, stark typisierte Klassen, die die Objective-C zugeordnet Delegatklasse. Um diese zu verwenden, werden Sie Unterklassen und Überschreiben der Methoden, die durch die Xamarin.iOS-Implementierung definiert werden. Weitere Informationen dazu, wie sie funktionieren finden Sie unter dem Abschnitt "Models" unten.


##### <a name="mapping-delegates-to-c35"></a>Zuordnen von Delegaten in C&#35;

UIKit verwendet Objective-C-Delegaten im Allgemeinen in zwei Formen.

Die erste Form stellt eine Schnittstelle mit einer Komponente Modell bereit. Z. B. als ein Mechanismus, um Daten nach Bedarf für eine Sicht, z. B. für die Aufbewahrung der Daten für eine Listenansicht bereitzustellen.  In diesen Fällen sollten Sie erstellen eine Instanz der Klasse und weisen Sie die Variable.

Im folgenden Beispiel geben wir die `UIPickerView` über eine Implementierung für ein Modell, das Zeichenfolgen verwendet:

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

Die zweite Form stellt eine Benachrichtigung für Ereignisse bereitzustellen. In diesen Fällen Obwohl wir weiterhin die API in der oben beschriebenen Form zur Verfügung stellen bieten wir auch c#-Ereignisse, die einfacher zu verwenden für schnelle Vorgänge und Integration mit anonymen Delegaten und Lambda-Ausdrücke in C# geschrieben werden soll.

Sie können z. B. abonnieren, `UIAccelerometer` Ereignisse:

```csharp
UIAccelerometer.SharedAccelerometer.Acceleration += (sender, args) => {
   UIAcceleration acc = args.Acceleration;
   Console.WriteLine ("Time={0} at {1},{2},{3}", acc.Time, acc.X, acc.Y, acc.Z);
}
```

Die beiden Optionen sind verfügbar, wo sie sinnvoll sein, aber Sie müssen als Programmierer eine auswählen. Wenn Sie eine eigene Instanz eines stark typisierten Beantworter/Delegaten erstellen und zuweisen, werden die C#-Ereignisse nicht funktionsfähig ist. Wenn Sie die C#-Ereignisse verwenden, werden die Methoden in der Beantworter/Delegat-Klasse nie aufgerufen werden.

Im vorherigen Beispiel, mit denen `UIWebView` mit Lambda-Ausdrücken von c# 3.0 folgendermaßen geschrieben werden:

```csharp
var web = new UIWebView (new CGRect (0, 0, 200, 200));
web.LoadStarted += () => { startTime = DateTime.Now; }
web.LoadFinished += () => { endTime = DateTime.Now; }
```


#### <a name="responding-to-events"></a>Reagieren auf Ereignisse

In Objective-C-Code manchmal-Ereignishandlern für mehrere Steuerelemente und Anbietern von Informationen für mehrere Steuerelemente, gehostet in derselben Klasse. Dies ist möglich, da die Klassen auf Meldungen reagieren und solange Klassen auf Meldungen reagieren, es ist möglich, Objekte miteinander zu verknüpfen.

Wie zuvor beschrieben Xamarin.iOS unterstützt sowohl die C#-Ereignis basierendes Programmiermodell und die Objective-C-Delegat Muster, in dem Sie eine neue Klasse erstellen können der Delegat implementiert und überschreibt die gewünschten Methoden.

Es ist auch möglich, Objective-C-Muster zu unterstützen, in denen Responder für mehrere verschiedene Vorgänge verwenden alle in der gleichen Instanz einer Klasse gehostet werden. Aber dazu müssen Sie die Low-Level-Funktionen der Xamarin.iOS-Bindung.

Angenommen, Sie möchten Ihre Klasse auf beide reagieren die `UITextFieldDelegate.textFieldShouldClear`: Nachricht und die `UIWebViewDelegate.webViewDidStartLoad`: in der gleichen Instanz einer Klasse, müssen Sie die Attributdeklaration [Exportieren] zu verwenden:

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

Die C# Namen für die Methoden nicht wichtig sind. die absolute Hauptrolle sind die Zeichenfolgen, die an das [Export]-Attribut.

Wenn Sie diese Art der Programmierung verwenden zu können, stellen Sie sicher, dass die C#-Parameter die tatsächlichen Typen entsprechen, die die Common Language Runtime-Engine übergeben wird.

<a name="Models" />

#### <a name="models"></a>Modelle

In UIKit speichereinrichtungen oder Responder, die mithilfe von Hilfsklassen implementiert werden, diese werden in der Regel in der Objective-C-Code bezeichnet, als Delegaten, und sie werden als Protokolle implementiert.

Objective-C-Protokolle sind, wie Schnittstellen, aber sie optionale Methoden unterstützen – d. h. nicht alle Methoden für das Protokoll funktioniert implementiert werden müssen.

Es gibt zwei Möglichkeiten, ein Modell implementiert. Sie können es manuell zu implementieren oder verwenden Sie den vorhandenen Definitionen von stark typisierten.


Die manuelle Methode ist erforderlich, wenn Sie versuchen, eine Klasse zu implementieren, die von Xamarin.iOS nicht gebunden wurde. Es ist sehr einfach:

-  Kennzeichnen Sie Ihre Klasse für die Registrierung mit der runtime
-  Wenden Sie das [Export]-Attribut mit dem Namen der tatsächlichen Selektor für jede Methode, die Sie außer Kraft setzen möchten
-  Instanziieren Sie die Klasse, und übergeben Sie es.

Beispielsweise implementieren Folgendes nur eine der optionalen Methoden in der Definition des UIApplicationDelegate-Protokoll:

```csharp
public class MyAppController : NSObject {
        [Export ("applicationDidFinishLaunching:")]
        public void FinishedLaunching (UIApplication app)
        {
                SetupWindow ();
        }
}
```

Die Objective-C-Selektor-Name ("ApplicationDidFinishLaunching:") wird mit dem Export-Attribut deklariert, und die Klasse wird registriert, mit der `[Register]` Attribut.

Xamarin.iOS enthält stark typisierte Deklarationen, einsatzbereit, die keine manuelle Bindung erforderlich ist. Um dieses Programmiermodell zu unterstützen, unterstützt die Xamarin.iOS-Runtime das [Model]-Attribut in einer Klassendeklaration. Dadurch wird darüber informiert, dass die Laufzeit, die sie nicht alle Methoden in der Klasse verknüpfen sollte, wenn die Methoden sind explizit implementiert wird.

In UIKit bedeutet dass, werden die Klassen, die ein Protokoll mit optionalen Methoden darstellen, wie folgt geschrieben:

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

Wenn ein Modell zu implementieren, die nur einige der Methoden implementiert werden sollen, ist alles, was man dazu Unternehmen muss die Methoden, die Sie interessiert sind, und ignorieren die anderen Methoden überschreiben. Die Laufzeit wird nur von der überschriebenen Methoden, nicht die ursprünglichen Methoden in der Objective-C-Welt verknüpfen.

Entspricht dem vorherigen Beispiel für die manuelle:

```csharp
public class AppController : UIApplicationDelegate {
    public override void FinishedLaunching (UIApplication uia)
    {
     ...
    }
}
```

Die Vorteile sind, besteht keine Notwendigkeit, in die Objective-C-Header-Dateien finden Sie die Auswahl, die Typen der Argumente oder die Zuordnung zu C# -Code beschäftigen, und, dass Sie Intellisense in Visual Studio für Mac, zusammen mit starken Typen abrufen


#### <a name="xib-outlets-and-c35"></a>XIB-Outlets und C&#35;

> [!IMPORTANT]
> Dieser Abschnitt erläutert die IDE-Integration mit Outlets Verwendung XIB-Dateien. Bei Verwendung des Xamarin-Designers für iOS, dies wird alle ersetzt durch Eingabe eines Namens unter **Identität > Namen** im Abschnitt mit Eigenschaften Ihrer IDE, wie unten dargestellt:
>
> [![](images/designeroutlet.png "Ein Name-Element in der iOS Designer eingeben")](images/designeroutlet.png#lightbox)
>
>Weitere Informationen zu den iOS-Designer, finden Sie in der [Einführung in iOS Designer](~/ios/user-interface/designer/introduction.md#how-it-works) Dokument.

Dies wird wird auf niedriger Ebene beschrieben, wie Outlets in C# -Code integriert werden und für erfahrene Benutzer, die von Xamarin.iOS. Code auf dem Flug mithilfe von Visual Studio für Mac die Zuordnung automatisch im Hintergrund mithilfe Abschluss ist für Sie generiert werden.

Wenn Sie Ihre Benutzeroberfläche mit Interface Builder entwerfen, wird nur das Aussehen der Anwendung entwerfen, und Sie werden einige standardverbindungen herzustellen. Wenn Sie programmgesteuert Informationen abrufen, ändern das Verhalten eines Steuerelements zur Laufzeit oder das Steuerelement zur Laufzeit ändern möchten, ist es erforderlich, um einige der Steuerelemente an verwalteten Code zu binden.

Dies erfolgt in wenigen Schritten:

1.  Hinzufügen der **Outlet Deklaration** auf Ihre **Besitzer der Datei**.
1.  Verbinden Sie das Steuerelement die **Besitzer der Datei**.
1.  Store der Benutzeroberfläche sowie die Verbindungen in Ihrer XIB/NIB-Datei.
1.  Laden Sie die NIB-Datei zur Laufzeit.
1.  Zugreifen auf die Outlet-Variable.


Die Schritte (1) und (3) werden in der Apple Dokumentation zum Erstellen von Schnittstellen mit Interface Builder behandelt.

Wenn Sie Xamarin.iOS verwenden zu können, müssen Ihrer Anwendung eine Klasse erstellen, die von UIViewController abgeleitet. Die Implementierung erfolgt sie wie folgt:

```csharp
public class MyViewController : UIViewController {
    public MyViewController (string nibName, NSBundle bundle) : base (nibName, bundle)
    {
        // You can have as many arguments as you want, but you need to call
        // the base constructor with the provided nibName and bundle.
    }
}
```

Klicken Sie dann um Ihre ViewController aus einer NIB-Datei zu laden, verwenden Sie dazu:

```csharp
var controller = new MyViewController ("HelloWorld", NSBundle.MainBundle, this);
```

Dadurch wird die Benutzeroberfläche aus der NIB geladen. Für den Zugriff auf die Outlets, ist es nun erforderlich, um die Laufzeit darüber zu informieren, die wir darauf zugreifen möchten. Zu diesem Zweck die `UIViewController` Unterklasse benötigt, deklarieren die Eigenschaften und mit Anmerkungen versehen mit dem Attribut [verbinden]. Und zwar so:

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

Die Implementierung der Eigenschaft ist diejenige, die tatsächlich abruft und speichert den Wert für den eigentlichen nativen Typ.

Sie müssen sich keine Gedanken bei Verwendung von Visual Studio für Mac und InterfaceBuilder. Visual Studio für Mac spiegelt automatisch alle deklarierten Outlets mit Code in einer partiellen Klasse, die als Teil des Projekts kompiliert wird.

#### <a name="selectors"></a>Selektoren

Ein Kernkonzept von Objective-C-Programmierung ist die Auswahl. Häufig werden stoßen Sie auf APIs, die Sie eine Auswahl zu übergeben, oder erwartet, dass Ihr Code auf eine Auswahl zu reagieren.

Erstellen neue Selektoren in c# ist sehr einfach – erstellen Sie einfach eine neue Instanz der dem `ObjCRuntime.Selector` Klasse, und verwenden Sie das Ergebnis in einer beliebigen Stelle in der API, die es benötigt. Zum Beispiel:

```csharp
var selector_add = new Selector ("add:plus:");
```

Für eine C#-Methode reagieren auf einen Aufruf für die Auswahl, müssen sie erbt von der `NSObject` Typs und der C#-Methode mit dem Selektor Namen versehen werden die `[Export]` Attribut. Zum Beispiel:

```csharp
public class MyMath : NSObject {
    [Export ("add:plus:")]
    int Add (int first, int second)
    {
         return first + second;
    }
}
```

Beachten Sie diese Auswahl Namen **müssen** genau übereinstimmen, einschließlich alle zwischen- und nachfolgende Doppelpunkte (":"), falls vorhanden.

#### <a name="nsobject-constructors"></a>Konstruktoren von NSObject

Die meisten Klassen in Xamarin.iOS, die abgeleitet `NSObject` Konstruktoren, die spezifisch für die Funktionalität des-Objekts verfügbar macht, aber sie werden auch stellen die verschiedenen Konstruktoren, die nicht sofort offensichtlich sind.

Die Konstruktoren werden wie folgt verwendet:

```csharp
public Foo (IntPtr handle)
```

Dieser Konstruktor wird verwendet, um Ihre Klasse zu instanziieren, wenn die Laufzeit Ihre Klasse eine nicht verwaltete Klasse zuordnen muss. Dies geschieht, wenn Sie eine XIB/NIB-Datei zu laden.  An diesem Punkt die Objective-C-Laufzeit wird ein Objekt in die nicht verwaltete Umgebung erstellt haben, und dieser Konstruktor wird aufgerufen, um der verwalteten Seite zu initialisieren.

In der Regel müssen Sie lediglich mit dem Handle-Parameter, und klicken Sie im Text der Basiskonstruktor aufrufen, ist jede Initialisierung, die erforderlich ist.

```csharp
public Foo ()
```

Dies ist der Standardkonstruktor für eine Klasse, und in Xamarin.iOS-Klassen bereitgestellt, initialisiert die Foundation.NSObject-Klasse und alle Klassen in der Zwischenzeit, und am Ende verkettet Sie dies für den Objective-C- `init` Methode für die Klasse.

```csharp
public Foo (NSObjectFlag x)
```

Dieser Konstruktor dient zum Initialisieren der Instanz, aber verhindern, dass des Codes die Objective-C "Init"-Methode aufrufen, am Ende. In der Regel verwendet, wenn Sie für die Initialisierung bereits registriert haben (bei Verwendung von `[Export]` für den Konstruktor), oder Sie haben bereits nach Abschluss der Initialisierung über eine andere Mittel.

```csharp
public Foo (NSCoder coder)
```

Dieser Konstruktor wird für die Fälle bereitgestellt, in dem das Objekt aus einer NSCoding Instanz initialisiert wird. Weitere Informationen finden Sie auf der Apple [Archive und Serialisierung Programming Guide.](http://developer.apple.com/mac/library/documentation/Cocoa/Conceptual/Archiving/index.html#//apple_ref/doc/uid/10000047i)

#### <a name="exceptions"></a>Ausnahmen

Der Xamarin.iOS-API-Entwurf wird Objective-C-Ausnahmen nicht als C#-Ausnahmen ausgelöst werden. Der Entwurf erzwingt, dass keine Garbage in erster Linie in der Objective-C-Welt gesendet werden und alle Ausnahmen, die erzeugt werden müssen durch die Bindung selbst erstellt werden, bis die ungültige Daten je übergeben in der Objective-C-Welt.

#### <a name="notifications"></a>Benachrichtigungen

Unter iOS und OS X können Entwickler Benachrichtigungen abonnieren, die von der zugrunde liegenden Plattform übertragen werden. Dies erfolgt mithilfe der `NSNotificationCenter.DefaultCenter.AddObserver` Methode. Die `AddObserver` Methode akzeptiert zwei Parameter, die eine ist die Benachrichtigung, die Sie abonnieren möchten; die andere ist die Methode aufgerufen werden, wenn die Benachrichtigung ausgelöst wird.

In Xamarin.iOS und Xamarin.Mac werden die Schlüssel für die verschiedenen Benachrichtigungen für die Klasse gehostet, die die Benachrichtigungen ausgelöst. Z. B. die Benachrichtigungen ausgelöst, durch die `UIMenuController` gehostet werden, als `static NSString` Eigenschaften in der `UIMenuController` Klassen, die mit dem Namen "Benachrichtigung" enden.

### <a name="memory-management"></a>Speicherverwaltung

Xamarin.iOS verfügt über einen Garbage Collector, der kümmern wird der Freigabe von Ressourcen für Sie, wenn sie nicht mehr verwendet werden. Zusätzlich zu den Garbage Collector alle Objekte, die abgeleitet `NSObject` implementieren die `System.IDisposable` Schnittstelle.

#### <a name="nsobject-and-idisposable"></a>NSObject und "IDisposable"

Verfügbarmachen von der `IDisposable` Schnittstelle ist eine bequeme Möglichkeit zur Unterstützung von Entwicklern bei der Freigabe von Objekten, die große Speicherblöcke kapseln können (z. B. eine `UIImage` nur einen harmlosen Zeiger aussehen könnte, aber konnte zu einem Bild 2 MB verwiesen ) und andere wichtigen und begrenzten Ressourcen (z. B. ein video Decodierung Puffer).

NSObject implementiert, die IDisposable-Schnittstelle sowie die [.NET Dispose-Muster](http://msdn.microsoft.com/library/fs2xkftw.aspx). Dadurch können Entwickler diese Unterklasse NSObject außer Kraft setzen das Dispose-Verhalten und ihre eigenen Ressourcen bei Bedarf freizugeben. Betrachten Sie z. B. View Controller, mit dem für eine Reihe von Bildern aus:

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

Wenn ein verwaltetes Objekt verworfen wird, ist es nicht mehr nützlich. Sie möglicherweise immer noch einen Verweis auf die Objekte, aber das Objekt ist an diesem Punkt zum Ladeprozess ungültig. Einige .NET APIs Vergewissern Sie sich der durch eine ObjectDisposedException auslösen, wenn Sie versuchen, Sie Methoden für ein verworfenes Objekt, z. B. zugreifen:

```csharp
var image = UIImage.FromFile ("demo.png");
image.Dispose ();
image.XXX = false;  // this at this point is an invalid operation
```

Auch wenn Sie die Variable "Image" weiterhin zugreifen können, ist es wirklich einen ungültigen Verweis und nicht mehr verweist auf die Objective-C-Objekt, das das Bild gespeichert.

Verwerfen eines Objekts in c# bedeutet jedoch nicht, dass sich unbedingt das Objekt zerstört wird. Alles, was Sie tun das Freigeben des Verweises, die C# -Code auf das Objekt ist. Es ist möglich, dass die Cocoa-Umgebung einen Verweis auf für die eigene Verwendung beibehalten haben kann. Beispielsweise wenn Sie eine UIImageViews Image-Eigenschaft zu einem Bild festlegen und dann Sie das Bild verwerfen, das zugrunde liegende UIImageView gedauert hatte, seinen eigenen Verweis und hält einen Verweis auf dieses Objekt aus, bis er abgeschlossen ist verwenden.

#### <a name="when-to-call-dispose"></a>Beim Aufruf von Dispose

Sie müssen die Dispose Nutzungsart Mono im Entfernen des Objekts aufrufen. Ein möglicher Anwendungsfall ist, wenn Mono nicht kennt, dass Ihre NSObject tatsächlich einen Verweis auf eine wichtige Ressource, z. B. Arbeitsspeicher oder einem Pool Informationen enthält. In diesen Fällen sollten Sie Dispose, um den Verweis auf den Arbeitsspeicher, anstatt abzuwarten, bis die Mono ausführen einen Garbage Collection-Zyklus sofort freizugeben aufrufen.

Wenn Mono erstellt intern [NSString Verweise aus C#-Zeichenfolgen](~/ios/internals/api-design/nsstring.md), es sofort, um den Arbeitsaufwand zu reduzieren, die der Garbage Collector hat Sie entfernt. Je weniger Objekte etwa für den Umgang mit, desto schneller-Garbage Collector werden ausgeführt.

#### <a name="when-to-keep-references-to-objects"></a>Wenn Verweise auf Objekte beibehalten

Ein Nebeneffekt, die automatische Speicherverwaltung ist, dass der Garbage Collector nicht verwendete Objekte loszuwerden wird so lange keine Verweise auf diese vorhanden sind. Dies kann überraschend Nebeneffekte, manchmal z. B. haben, wenn Sie eine lokale Variable für Ihren Controller Ansicht der obersten Ebene erstellen oder Ihr Fenster auf oberster Ebene, und klicken Sie dann mit den verschwinden hinter Ihnen zur Seite.

Wenn Sie keinen Verweis in Ihrem statischen oder Instanzvariablen an Ihren Objekten beibehalten werden, zum Glück wird Mono die Dispose()-Methode aufgerufen, darauf, und sie den Verweis auf das Objekt frei. Da dies die nur ausstehende-Verweis sein kann, werden die Objective-C-Laufzeit das Objekt für Sie zerstört.

## <a name="related-links"></a>Verwandte Links

- [Binden von Feldern](~/cross-platform/macios/binding/objective-c-libraries.md#Binding_Fields)
