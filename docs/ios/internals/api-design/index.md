---
title: API-Entwurf
description: Die Perspektive der Xamarin.iOS-API-Entwurf
ms.topic: article
ms.prod: xamarin
ms.assetid: 322D2724-AF27-6FFE-BD21-AA1CFE8C0545
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 5fab7579be256e478c69b76b5e41b8c1b0568ba6
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2018
---
# <a name="api-design"></a>API-Entwurf

Zusätzlich zu den Kern Base Class Libraries, die Teil der Mono, [Xamarin.iOS](http://www.xamarin.com/iOS) im Lieferumfang von Bindungen für verschiedene iOS-APIs ermöglichen Entwicklern das Erstellen von systemeigenen iOS-Anwendungen mit Mono.

Im Kern der Xamarin.iOS, besteht eine Interop-Modul, das mit der Außenwelt Objective-C als c#-Welt verbindet, sowie Bindungen für das iOS wie C basierende APIs CoreGraphics und [OpenGLES](#OpenGLES).

Die Low-Level-Laufzeit für die Kommunikation mit Objective-C-Code ist in der [MonoTouch.ObjCRuntime](#MonoTouch.ObjCRuntime). Zusätzlich können Bindungen für [Foundation](#MonoTouch.Foundation), CoreFoundation und [UIKit](#MonoTouch.UIKit) bereitgestellt werden.

## <a name="design-principles"></a>Entwurfsprinzipien

Es gibt einige unserer Entwurfsprinzipien für die Bindung Xamarin.iOS (diese gelten auch für Xamarin.Mac Mono-Bindungen für Objective-C unter OS X):


- Führen Sie die Framework-Entwurfsrichtlinien
- Damit Entwickler Unterklasse Objective-C-Klassen:

  - Leiten Sie von einer vorhandenen Klasse
  - Zu verketten Basiskonstruktor aufrufen.
  - Überschreiben von Methoden sollten mit # Außerkraftsetzung System ausgeführt werden

- Unterklassen sollten mit standardmäßige Konstrukte zusammenarbeiten.
- Machen Sie Entwickler Objective-C-Selektoren nicht verfügbar
- Geben Sie einen Mechanismus zum Aufrufen von beliebiger Objective-C-Bibliotheken
- Stellen Sie allgemeine Aufgaben für Objective-C schwierig und einfach Objective-C-Aufgaben möglich
- Machen Sie Objective-C-Eigenschaften als C#-Eigenschaften verfügbar.
- Stellen Sie eine stark typisierte-API:
- Erhöhen der typsicherheit
- Minimieren-Runtime-Fehler
- Abrufen von IDE Intellisense für Rückgabetypen
- Ermöglicht die Dokumentation für IDE-popup
- Ermutigen Sie in der IDE zum Durchsuchen von APIs:
- Systemeigene C#-Typen:

    - Beispiel: anstelle von Verfügbarmachen von schwach typisierten Arrays wie folgt aus:
        ```
        NSArray *getViews
        ```
        Wir richten sie starke Typen können wie folgt:
    
        ```
        NSView [] Views { get; set; }
        ```
    
    Dies ermöglicht es Visual Studio für Mac, führen Sie die automatische Vervollständigung beim Durchsuchen der API und können auch alle von der `System.Array` Vorgänge auf den zurückgegebenen Wert verfügbar sein und den Rückgabewert zur Teilnahme an LINQ ermöglicht

- [NSString wird die Zeichenfolge](~/ios/internals/api-design/nsstring.md)
- Aktivieren Sie Int-Typen und "uint"-Parameter, die Enumerationen als C#-Enumerationen und C#-Enumerationen mit [Flags]-Attribute wurden sollten
- Anstelle von Typ Neutral NSArray Verfügbarmachen von Objekten als Array von stark typisierten Arrays.
- Ereignisse und Benachrichtigungen, erhalten Benutzer eine Auswahl zwischen:

    - Stark typisierte Version ist die Standardeinstellung
    - Schwach typisierte Version für erweiterte Anwendungsfälle

- Unterstützen Sie die Objective-C-Delegat-Muster:

    - C#-Ereignissystem
    - Machen Sie C#-Delegaten (Lambdas, anonyme Methoden und System.Delegate) wird für Objective-C-APIs verfügbar, als "Blöcke"

### <a name="assemblies"></a>Assemblys

Xamarin.iOS umfasst eine Reihe von Assemblys, die bilden die *Xamarin.iOS Profil*. Die [Assemblys](~/cross-platform/internals/available-assemblies.md) Seite verfügt über mehr Informationen.

### <a name="major-namespaces"></a>Major-Namespaces 

 <a name="MonoTouch.ObjCRuntime" />

#### <a name="objcruntime"></a>ObjCRuntime

Die [ObjCRuntime](https://developer.xamarin.com/api/namespace/ObjCRuntime/) Namespace ermöglicht Entwicklern, die Bereichen zwischen C#- und Objective-c zu überbrücken.
Dies ist eine neue Bindung, die speziell für iOS, basierend auf der Erfahrung von Kakao- und Gtk # entworfen.

 <a name="MonoTouch.Foundation" />


#### <a name="foundation"></a>Foundation

Die [Foundation](https://developer.xamarin.com/api/namespace/Foundation/) -Namespace stellt die grundlegenden Datentypen entwickelt, mit dem Objective-C-Foundation-Framework zusammenarbeiten, die Teil der e/as ist und sie stellt die Basis für die objektorientierte Programmierung in Objective-C.

Xamarin.iOS spiegelt in c# die Hierarchie der Klassen von Objective-c. Z. B. die Objective-C-Basisklasse [NSObject](http://developer.apple.com/iphone/library/documentation/Cocoa/Reference/Foundation/Classes/NSObject_Class/Reference/Reference.html) ist in c# über [Foundation.NSObject](https://developer.xamarin.com/api/type/Foundation.NSObject/).

Obwohl dieser Namespace Bindungen für die zugrunde liegenden Foundation Objective-C-Typen enthält, in einigen Fällen haben wir die zugrunde liegende Typen, .NET-oder Schematypen zugeordnet. Zum Beispiel:

- Anstelle von [NSString](http://developer.apple.com/iphone/library/documentation/Cocoa/Reference/Foundation/Classes/NSString_Class/Reference/NSString.html) und [NSArray](https://developer.apple.com/library/ios/#documentation/Cocoa/Reference/Foundation/Classes/NSArray_Class/NSArray.html), die Common Language Runtime stellt diese als C#- [Zeichenfolge](https://developer.xamarin.com/api/type/System.String/)s und stark typisierte [Array](https://developer.xamarin.com/api/type/System.Array/)s in der gesamten die API.

- Verschiedene Hilfs-APIs sind hier, damit Entwickler zum Binden von Drittanbietern Objective-C-APIs, andere iOS-APIs oder APIs, die derzeit keine Xamarin.iOS gebunden sind verfügbar.


Weitere Informationen zum Binden von APIs finden Sie unter der [Xamarin.iOS Bindung Generator](~/cross-platform/macios/binding/binding-types-reference.md) Abschnitt.

 <a name="NSObject" />


##### <a name="nsobject"></a>NSObject

Die [NSObject](https://developer.xamarin.com/api/type/Foundation.NSObject/) Typ bildet die Grundlage für alle Objective-C-Bindungen. Typen von Xamarin.iOS spiegeln zwei Klassen von Typen aus den iOS CocoaTouch-APIs: der C-Typen (in der Regel haben, als CoreFoundation Typen) und die Objective-C-Typen (diese alle von der NSObject-Klasse abgeleitet sind).

Für jeden Typ, die einen nicht verwalteten Typ sind, ist es möglich, das systemeigene Objekt durch Abrufen der [behandeln](https://developer.xamarin.com/api/property/Foundation.NSObject.Handle/) Eigenschaft.

Während Mono Garbagecollection für alle Objekte bereitstellen, wird die `Foundation.NSObject` implementiert die [System.IDisposable](https://developer.xamarin.com/api/type/System.IDisposable/) Schnittstelle. Dies bedeutet, dass Sie die Ressourcen für alle angegebenen NSObject explizit freigeben können, ohne zu warten, bis die Garbage Collection Kick in. Dies ist wichtig, bei Verwendung starker NSObjects, z. B. UIImages, die Zeiger auf große Datenblöcke enthalten kann.

Wenn Ihr Typ deterministischen Abschluss ausführen muss, überschreiben die [NSObject.Dispose(bool)-Methode](https://developer.xamarin.com/api/type/Foundation.NSObject/%2fM%2fDispose) dem Dispose-Parameter ist "Bool disposing", und legen Sie es "true" bedeutet, dass die Methode Dispose da aufgerufen wird der Benutzer explizit aufgerufene Dispose () für das Objekt. Wenn der Wert "false" ist, bedeutet dies, dass die Methode Dispose (Bool disposing) aus den Finalizer im Finalizer-Thread aufgerufen wird. []()

<a name="Categories" />

##### <a name="categories"></a>Kategorien

Beginnend mit Xamarin.iOS 8.10 es ist möglich, Objective-C-Kategorien von c# zu erstellen.

Dies erfolgt mithilfe der `Category` Attribut, und geben den Typ als Argument für das Attribut zu erweitern. Im folgende Beispiel wird z. B. die NSString erweitert.

    [Category (typeof (NSString))]

Jede Kategorie-Methode wird mithilfe des normalen Mechanismus zum Exportieren von Methoden in Objective-C mit dem `Export` Attribut:

    [Export ("today")]
    public static string Today ()
    {
        return "Today";
    }

Alle verwalteten Erweiterungsmethoden müssen statisch sein, aber es ist möglich, verwenden die Standardsyntax für Erweiterungsmethoden in c# Objective-C-Instanzmethoden zu erstellen:

    [Export ("toUpper")]
    public static string ToUpper (this NSString self)
    {
        return self.ToString ().ToUpper ();
    }

und das erste Argument der Erweiterungsmethode werden auf der Instanz auf der die Methode aufgerufen wurde.

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

In diesem Beispiel wird eine systemeigene ToUpper-Instanzmethode zu der Klasse NSString hinzuzufügen, die von Objective-c aufgerufen werden können

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

Ein Szenario, in denen dies hilfreich ist, Hinzufügen einer Methode um einen ganzen Satz von Klassen in der Codebase, z. B. würde dies alle machen `UIViewController` Instanzen meldet, dass gedreht werden kann:

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

<a name="PreserveAttribute" />

##### <a name="preserveattribute"></a>PreserveAttribute

PreserveAttribute ist ein benutzerdefiniertes Attribut ab, das Teilen Mtouch – Bereitstellungstool Xamarin.iOS – einen Typ oder ein Member eines Typs beibehalten, während der Phase, wann die Anwendung verarbeitet wird, um die Größe zu verringern.

Jeder Member, die von der Anwendung nicht statisch ist kann entfernt werden. Daher wird dieses Attribut verwendet, um Elemente zu markieren, sind nicht statisch verwiesen, aber, die von der Anwendung weiterhin benötigt werden.

Z. B. Wenn Sie Typen dynamisch instanziieren, können Sie den Standardkonstruktor Ihrer Typen beibehalten möchten. Wenn Sie XML-Serialisierung verwenden, empfiehlt es sich um die Eigenschaften der Typen beizubehalten.

Sie können dieses Attribut auf alle Member eines Typs oder auf den Typ selbst anwenden. Wenn Sie den gesamten Datentyp beibehalten möchten, können Sie die Syntax [beibehalten (AllMembers = "true")] für den Typ.

 <a name="MonoTouch.UIKit" />


#### <a name="uikit"></a>UIKit

Die [UIKit](https://developer.xamarin.com/api/namespace/UIKit/) -Namespace enthält eine Zuordnung aller der UI-Komponenten, die CocoaTouch in Form von C#-Klassen bilden. Die API wurde geändert, um die in der C#-Sprache verwendeten Konventionen.

C#-Delegaten dienen häufig ausgeführte Vorgänge. Finden Sie unter der [Delegaten](#Delegates) Abschnitt, um weitere Informationen.

 <a name="OpenGLES" />


#### <a name="opengles"></a>OpenGLES

OpenGLES, wir Verteilen einer [Version geändert](https://developer.xamarin.com/api/namespace/OpenTK/) von der [OpenTK](http://www.opentk.com/) -API, eine Bindung mit dem objektorientierten, um OpenGL, die Verwendung von CoreGraphics-Datentypen und-Strukturen geändert wurde, als auch nur die Funktionen, die auf iOS verfügbar ist.

OpenGLES-1.1-Funktionalität steht über den ES11.GL-Typ, dokumentiert [hier](https://developer.xamarin.com/api/type/OpenTK.Graphics.ES11.GL/) Typ.

OpenGLES-2.0-Funktionalität steht über den ES20.GL-Typ, dokumentiert [hier](https://developer.xamarin.com/api/type/OpenTK.Graphics.ES20.GL/) Typ.

OpenGLES-3.0-Funktionalität steht über den ES30.GL-Typ, dokumentiert [hier](https://developer.xamarin.com/api/type/OpenTK.Graphics.ES30.GL/) Typ.

 <a name="Binding_Design" />


### <a name="binding-design"></a>Binden von Entwurf

Xamarin.iOS ist nicht lediglich eine Bindung an die zugrunde liegende Objective-C-Plattform. Es erweitert die Typsystem und Dispatchsystem eine bessere Blend C#-und Objective-c

Ebenso wie P/Invoke ein hilfreiches Tool zum Aufrufen von systemeigenen Bibliotheken unter Windows und Linux ist oder als IJW Unterstützung für COM-Interop unter Windows verwendet werden kann, erweitert Xamarin.iOS der Laufzeit zur Unterstützung von C#-Bindungsobjekten Objective-C-Objekten.

Die Diskussion in den nächsten Abschnitten ist nicht erforderlich für Benutzer, die Xamarin.iOS Anwendungen erstellen, aber ermöglicht es Entwicklern, verstehen, wie Aufgaben fertig sind, und hilft ihnen beim Erstellen komplexer Anwendungen.


 <a name="Types" />


#### <a name="types"></a>Typen

Bei denen es sinnvoll, getroffen werden C#-Typen statt auf niedriger Ebene Foundation-Typen, für die C#-Universe bereitgestellt.  Dies bedeutet, dass [die-API verwendet den C#-Typ "String" anstelle von NSString](~/ios/internals/api-design/nsstring.md) und stark typisierte Arrays c# nicht, sondern Verfügbarmachen von NSArray verwendet.

Im Allgemeinen in den Entwurf Xamarin.iOS und Xamarin.Mac des zugrunde liegenden `NSArray` Objekt nicht verfügbar gemacht wird. Dagegen konvertiert die Common Language Runtime automatisch `NSArray`s, um stark typisierte Arrays einiger `NSObject` Klasse. Daher macht Xamarin.iOS keine schwach typisierte Methode wie GetViews ein NSArray zurückgegeben:

```csharp
NSArray GetViews ();
```

Die Bindung macht stattdessen einen stark typisierten Rückgabewert wie folgt:

```csharp
UIView [] GetViews ();
```

Es werden einige der Methoden in `NSArray`, für die Sonderfälle, in denen Sie möglicherweise verwenden möchten, ein `NSArray` direkt, aber ihre Verwendung wird abgeraten, in der API-Bindung.

Darüber hinaus wird in der **klassische API** statt Verfügbarmachen von `CGRect`, `CGPoint` und `CGSize` aus der CoreGraphics-API, ersetzt die mit der `System.Drawing` Implementierungen `RectangleF`, `PointF`und `SizeF` wie sie hilfreich sein würde erhalten Entwickler vorhandenen OpenGL-Code, der OpenTK verwendet. Bei Verwendung des neue 64-Bit- **einheitliche API**, sollte die CoreGraphics-API verwendet werden.

 <a name="Inheritance" />


#### <a name="inheritance"></a>Vererbung

Der Entwurf Xamarin.iOS-API ermöglicht Entwicklern systemeigene Objective-C-Typen auf die gleiche Weise erweitert, dass sie einen C#-Typ, verwenden das Schlüsselwort "Override" in einer abgeleiteten Klasse sowie verketten Sie die grundlegende Implementierung, die mit dem "Basis" c#-Schlüsselwort auftreten würde.

Dieser Entwurf ermöglicht Entwicklern von Umgang mit Objective-C-Selektoren als Teil ihrer Entwicklungsprozess zu vermeiden, da das gesamte Objective-C-System bereits in den Bibliotheken Xamarin.iOS eingebunden ist.

 <a name="Types_and_Interface_Builder" />


#### <a name="types-and-interface-builder"></a>Typen und Benutzeroberflächen-Generator

Wenn Sie Klassen, die Instanzen von Typen von Benutzeroberflächen-Generator erstellt werden erstellen, müssen Sie einen Konstruktor bereitstellen, die eine einzelne akzeptiert `IntPtr` Parameter.
Dies ist erforderlich, um die verwalteten Objektinstanz mit dem nicht verwalteten Objekt zu binden.
Der Code besteht aus einer einzelnen Zeile ein, wie folgt:

```csharp
public partial class void MyView : UIView {
   // This is the constructor that you need to add.
   public MyView (IntPtr handle) : base (handle) {}
}
```


 <a name="Delegates" />


#### <a name="delegates"></a>Delegaten

Objective-C und c# haben verschiedene Bedeutungen für den Delegaten Wort in der jeweiligen Sprache.

In der Welt Objective-C und in der Dokumentation, die Sie online zu CocoaTouch sehen, ist ein Delegat in der Regel eine Instanz einer Klasse, die einen Satz von Methoden reagiert. Dies ähnelt sehr einer C#-Schnittstelle, mit dem Unterschied, dass die Methoden nicht immer erforderlich sind.

Diese Delegaten spielen eine wichtige Rolle beim UIKit und andere CocoaTouch-APIs. Sie werden verwendet, um verschiedene Aufgaben auszuführen:

-  Um Benachrichtigungen in den Code (ähnlich wie Ereignisübermittlung in c# oder Gtk +) bereitzustellen.
-  So implementieren Sie Modelle für die Visualisierung Datensteuerelemente
-  Um das Verhalten eines Steuerelements zu erzielen.


Die programmierschema wurde entwickelt, minimieren Sie die Erstellung von abgeleiteten Klassen, um Verhalten für ein Steuerelement zu ändern. Diese Lösung ist ähnlich Willen im Laufe der Jahre andere GUI-Toolkits haben: Gtks signalisiert, unbedingt Slots, Winforms, Ereignisse WPF/Silverlight und so weiter. Um zu vermeiden, haben Hunderte von Schnittstellen (eine für jede Aktion) oder dass Entwickler für die Implementierung zu viele Methoden, die sie nicht benötigen, unterstützt Objective-C Methodendefinitionen optional. Dies ist anders als c#-Schnittstellen, die alle Methoden implementiert werden müssen.

In Objective-C-Klassen, sehen Sie, dass Klassen, mit denen diese programmierschema eine Eigenschaft in der Regel mit verfügbar machen `delegate`, dies ist erforderlich, um die obligatorische Teile der Schnittstelle und 0 (null) oder mehrere der optionalen Teile zu implementieren.

In Xamarin.iOS sind drei sich gegenseitig ausschließende Mechanismen zum Binden an diese Delegaten angeboten:

1.  [Über Ereignisse](#Events) .
2.  [Stark typisierte über eine `Delegate`Eigenschaft](#StrongDelegate) .
3.  [Lose typisierte über eine `WeakDelegate`Eigenschaft](#WeakDelegate) .


Betrachten Sie beispielsweise die [UIWebView](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebView_Class/Reference/Reference.html) Klasse. Diese Verteilung an einen [UIWebViewDelegate](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebViewDelegate_Protocol/Reference/Reference.html) -Instanz, die zugewiesen ist die [Delegieren](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebView_Class/Reference/Reference.html#//apple_ref/occ/instp/UIWebView/delegate) Eigenschaft.

 <a name="Via_Events" />


##### <a name="via-events"></a>Über Ereignisse

Für viele Arten Xamarin.iOS erstellt automatisch einen entsprechenden Delegaten weiterleiten, wird die `UIWebViewDelegate` Aufrufe auf C#-Ereignisse. Für `UIWebView`:

-  Die [WebViewDidStartLoad](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebViewDelegate_Protocol/Reference/Reference.html#//apple_ref/occ/intfm/UIWebViewDelegate/webViewDidStartLoad:) -Methode zugeordnet ist die [UIWebView.LoadStarted](https://developer.xamarin.com/api/event/UIKit.UIWebView.LoadStarted/) Ereignis.
-  Die [WebViewDidFinishLoad](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebViewDelegate_Protocol/Reference/Reference.html#//apple_ref/occ/intfm/UIWebViewDelegate/webViewDidFinishLoad:) -Methode zugeordnet ist die [UIWebView.LoadFinished](https://developer.xamarin.com/api/event/UIKit.UIWebView.LoadFinished/) Ereignis.
-  Die [WebView:didFailLoadWithError](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebViewDelegate_Protocol/Reference/Reference.html#//apple_ref/occ/intfm/UIWebViewDelegate/webView:didFailLoadWithError:) -Methode zugeordnet ist die [UIWebView.LoadError](https://developer.xamarin.com/api/event/UIKit.UIWebView.LoadError/) Ereignis.


Dieses einfache Programm zeichnet z. B. die Start- und Endzeiten beim Laden einer Webs anzeigen:

```csharp
DateTime startTime, endTime;
var web = new UIWebView (new CGRect (0, 0, 200, 200));
web.LoadStarted += (o, e) => startTime = DateTime.Now;
web.LoadFinished += (o, e) => endTime = DateTime.Now;
```

 <a name="Via_Properties" />


##### <a name="via-properties"></a>VIA-Eigenschaften

Ereignisse sind nützlich, wenn es gibt möglicherweise mehr als einen Abonnenten für das Ereignis. Darüber hinaus werden Ereignisse auf die Fälle beschränkt, es keinen Wert zurückgibt, aus dem Code gibt.

Für Fälle, in denen der Code erwartet wird, um einen Wert zurückzugeben, entschieden wir stattdessen für Eigenschaften. Dies bedeutet, dass nur eine Methode zu einem bestimmten Zeitpunkt in ein Objekt festgelegt werden kann.

Sie können z. B. dieses Mechanismus verwenden, beim Schließen der Tastatur auf dem Bildschirm auf den Handler für eine `UITextField`:

```csharp
void SetupTextField (UITextField tf)
{
    tf.ShouldReturn = delegate (textfield) {
        textfield.ResignFirstResponder ();
        return true;
    }
}
```

Die `UITextField`des `ShouldReturn` Eigenschaft in diesem Fall wird als Argument akzeptiert ein Delegat, der einen booleschen Wert zurückgibt, und bestimmt, ob das Textfeld mit der zurück-Schaltfläche gedrückt wird etwas tun sollten. In unserem-Methode zurückgegeben werden wir *"true"* an den Aufrufer, aber wir die Tastatur auch aus dem Bildschirm entfernen (Dies geschieht, wenn das Textfeld ruft `ResignFirstResponder`).


##### <a name="strongly-typed-via-a-delegate-property"></a>Stark typisierte über einen Delegaten-Eigenschaft

Wenn Sie nicht Ereignisse verwenden möchten, Sie bieten eine eigene [UIWebViewDelegate](https://developer.xamarin.com/api/type/UIKit.UIWebViewDelegate/) Unterklasse und weisen Sie ihn der [UIWebView.Delegate](https://developer.xamarin.com/api/property/UIKit.UIWebView.Delegate/) Eigenschaft. Sobald UIWebView.Delegate zugewiesen wurde, der Ereignismechanismus Dispatch UIWebView funktioniert nicht mehr, und die Methoden UIWebViewDelegate werden aufgerufen, wenn die entsprechenden Ereignisse auftreten.

Mit diesem einfache Typ zeichnet z. B. den Zeitaufwand für das Laden einer Webansicht:

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

Oben im Code wie folgt verwendet:

```csharp
var web = new UIWebView (new CGRect (0, 0, 200, 200));
web.Delegate = new Notifier ();
```

Die oben genannten erstellt eine UIWebViewer und wird es anweisen, es zum Senden von Nachrichten mit einer Instanz von Änderungsbenachrichtigung, eine Klasse, die wir zum Antworten auf Nachrichten erstellt.

Dieses Muster wird auch verwendet, um Verhalten für bestimmte Steuerelemente, z. B. im Fall UIWebView steuern die [UIWebView.ShouldStartLoad](https://developer.xamarin.com/api/property/UIKit.UIWebView.ShouldStartLoad/) -Eigenschaft kann die `UIWebView` Instanz, um zu steuern, ob die `UIWebView` lädt ein Seite oder nicht.

Das Muster wird auch verwendet, um die Daten bei Bedarf für einige Steuerelemente bereitzustellen. Z. B. die [UITableView](https://developer.xamarin.com/api/type/UIKit.UITableView/) Steuerelement ist ein leistungsstarkes Tabelle-Rendering-Steuerelement – und das Aussehen und die Inhalte werden von einer Instanz von driven eine [UITableViewDataSource](https://developer.xamarin.com/api/type/UIKit.UITableView/DataSource)


Lose typisierte über die Eigenschaft WeakDelegate @###

Zusätzlich zu den stark typisierten Eigenschaft ist auch eine schwach typisierte Delegat, der ermöglicht den Entwickler, die Dinge unterschiedlich binden, falls gewünscht.
Ein stark typisiertes Everywhere `Delegate` -Eigenschaft verfügbar gemacht wird, bei der Bindung des Xamarin.iOS, ein entsprechendes `WeakDelegate` Eigenschaft ist ebenfalls verfügbar gemacht.

Bei Verwendung der `WeakDelegate`, Sie sind verantwortlich für ordnungsgemäß ergänzen die Klasse mithilfe der [exportieren](https://developer.xamarin.com/api/type/Foundation.ExportAttribute/) Attribut, um die Auswahl anzugeben. Zum Beispiel:

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

Beachten Sie, einmal die `WeakDelegate` Eigenschaft zugewiesen wurde, die `Delegate` Eigenschaft wird nicht verwendet werden. Darüber hinaus, wenn Sie die Methode in einer geerbten Klasse, die Sie [Exportieren] möchten implementieren, müssen Sie eine öffentliche Methode erleichtern.


## <a name="mapping-of-the-objective-c-delegate-pattern-to-c35"></a>Zuordnen des Musters für den Delegaten Objective-C, K &#35;

Wenn Sie finden Sie unter Objective-C-Beispiele, in denen sieht wie folgt aus:

```csharp
foo.delegate = [[SomethingDelegate] alloc] init]
```

Dadurch wird die Sprache erstellen, erstellen eine Instanz der Klasse "SomethingDelegate", und weisen Sie den Wert der Eigenschaft Delegaten für die Variable Foo angewiesen. Dieser Mechanismus wird von Xamarin.iOS unterstützt, und C#-Syntax ist:

```csharp
foo.Delegate = new SomethingDelegate ();
```

Xamarin.iOS enthält bereitgestellten Delegatklasse, stark typisierte Klassen, die die Objective-C zugeordnet. Informationen zu ihrer Verwendung werden Sie Unterklasse erstellen und überschreiben die Methoden, die durch die Implementierung des Xamarin.iOS definiert werden. Weitere Informationen zu deren Funktionsweise finden Sie unter dem Abschnitt "Modelle" weiter unten.


##### <a name="mapping-delegates-to-c35"></a>Zuordnen von Delegaten zu C &#35;

UIKit verwendet Objective-C-Delegaten im Allgemeinen in zwei Formen.

Die erste Form stellt eine Schnittstelle mit einer Komponente im Modell. Z. B. einen Mechanismus, um Daten nach Bedarf für eine Sicht, z. B. für die Aufbewahrung der Daten für eine Listenansicht bereitzustellen.  In diesen Fällen sollten Sie eine Instanz der Klasse erstellen und Zuweisen von Variablen.

Im folgenden Beispiel geben wir die `UIPickerView` mit einer Implementierung für ein Modell, das Zeichenfolgen verwendet:

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

Die zweite Form stellt eine Benachrichtigung für Ereignisse bereitzustellen. In diesen Fällen zwar immer noch die API in der oben beschriebenen Form zur Verfügung bieten wir auch c#-Ereignisse, die einfacher zu verwenden für schnelle Vorgänge und integriert in anonymen Delegaten und Lambda-Ausdrücke in C# geschrieben werden soll.

Sie können z. B. abonnieren, `UIAccelerometer` Ereignisse:

```csharp
UIAccelerometer.SharedAccelerometer.Acceleration += (sender, args) => {
   UIAcceleration acc = args.Acceleration;
   Console.WriteLine ("Time={0} at {1},{2},{3}", acc.Time, acc.X, acc.Y, acc.Z);
}
```

Die beiden Optionen sind verfügbar, sie sinnvoll, aber als Programmierer müssen Sie eines dieser Zuordnungsverfahren auswählen. Wenn Sie eine eigene Instanz eines stark typisierten Beantworter/Delegaten zu erstellen und zuweisen, werden der C#-Ereignisse nicht funktionsfähig ist. Bei Verwendung von c#-Ereignisse werden die Methoden in Ihrer Beantworter/Delegatklasse nie aufgerufen werden.

Im vorherigen Beispiel, mit denen `UIWebView` können mithilfe von c# 3.0 Lambdas wie folgt geschrieben werden:

```csharp
var web = new UIWebView (new CGRect (0, 0, 200, 200));
web.LoadStarted += () => { startTime = DateTime.Now; }
web.LoadFinished += () => { endTime = DateTime.Now; }
```


#### <a name="responding-to-events"></a>Reagieren auf Ereignisse

In Objective-C-Code wird wird manchmal Ereignishandler für mehrere Steuerelemente und Anbieter von Informationen für mehrere Steuerelemente gehostet werden in der gleichen Klasse. Dies ist möglich, da die Klassen auf Nachrichten reagieren und als Klassen auf Nachrichten reagieren, es ist möglich, Objekte miteinander zu verbinden.

Wie zuvor ausführlich Xamarin.iOS unterstützt sowohl die C#-ereignisbasierte Programmiermodell und Objective-C-Delegat-Muster, in dem Sie eine neue Klasse erstellen können der Delegat implementiert und überschreibt die gewünschten Methoden.

Es ist auch möglich, Objective-C-Muster unterstützen, in denen Responder für mehrere verschiedene Vorgänge verwenden alle in der gleichen Instanz einer Klasse gehostet werden. Obwohl hierfür müssen Sie die Low-Level-Funktionen der Xamarin.iOS Bindung verwenden.

Angenommen, Sie möchten Ihre Klasse sowohl reagieren die `UITextFieldDelegate.textFieldShouldClear`: Meldung und die `UIWebViewDelegate.webViewDidStartLoad`: in der gleichen Instanz einer Klasse, müssten Sie die Attributdeklaration [Exportieren] verwenden:

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

Die C#-Namen für die Methoden sind nicht wichtig. wichtig sind die Zeichenfolgen, die an das Attribut [Exportieren] übergeben.

Wenn Sie diese Art der Programmierung zu verwenden, stellen Sie sicher, dass die C#-Parameter die tatsächlichen Typen entsprechen, die das Laufzeitmodul übergeben werden.


#### <a name="models"></a>Modelle

UIKit Speicher Computersystemen oder Responder, die mithilfe von Hilfsklassen implementiert werden, diese werden in der Regel im Objective-C-Code bezeichnet, als Stellvertreter, und Protokolle implementiert.

Objective-C-Protokolle sind, wie Schnittstellen, aber sie die optionale Methoden unterstützen – also nicht alle Methoden für das Protokoll funktioniert implementiert werden müssen.

Es gibt zwei Möglichkeiten, ein Modell aus. Sie können ihn manuell implementieren oder verwenden die vorhandenen stark typisierten Definitionen.


Die manuelle Mechanismus ist erforderlich, wenn Sie versuchen, eine Klasse zu implementieren, die nicht von Xamarin.iOS gebunden wurde. Es ist sehr einfach:

-  Kennzeichnen Sie die Klasse für die Registrierung mit der Common Language runtime
-  Wenden Sie das [Exportieren]-Attribut mit dem Namen des tatsächlichen Selektor für jede Methode, die Sie überschreiben möchten.
-  Instanziieren der Klasse, und übergeben Sie ihn.

Z. B. Implementieren der folgenden nur eine optionale Methoden in die Protokolldefinition UIApplicationDelegate:

```csharp
public class MyAppController : NSObject {
        [Export ("applicationDidFinishLaunching:")]
        public void FinishedLaunching (UIApplication app)
        {
                SetupWindow ();
        }
}
```

Objective-C-Auswahlnamen ("ApplicationDidFinishLaunching:") mit dem Export-Attribut deklariert ist, und die Klasse wird registriert, mit der `[Register]` Attribut.

Xamarin.iOS stellt stark typisierte Deklarationen, einsatzbereit, die keine manuelle Bindung erforderlich ist. Um dieses Programmiermodell zu unterstützen, unterstützt die Xamarin.iOS-Laufzeit [Modell]-Attribut in einer Klassendeklaration. Damit wird mitgeteilt, dass die Common Language Runtime, die sie nicht alle Methoden in der Klasse Netzwerkdaten sollten, es sei denn, die Methoden sind explizit implementiert wird.

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

Bei der Implementierung eines Modells, das nur einige der Methoden implementiert werden sollen, ist alles, was Sie tun müssen die Methoden, die Sie interessiert sind, und ignorieren die anderen Methoden überschreiben. Die Common Language Runtime wird nur der überschriebenen Methoden nicht die ursprünglichen Methoden zur Objective-C-Welt verknüpfen.

Entspricht dem vorherigen Beispiel für die manuelle:

```csharp
public class AppController : UIApplicationDelegate {
    public override void FinishedLaunching (UIApplication uia)
    {
     ...
    }
}
```

Die Vorteile sind, besteht keine Notwendigkeit, die in Objective-C-Header-Dateien gefunden, die Auswahl, die Typen der Argumente oder die Zuordnung zu C#-vorzudringen und, dass Sie Intellisense in Visual Studio für Mac, zusammen mit starken Typen erhalten


#### <a name="xib-outlets-and-c35"></a>XIB Steckdosen und k &#35;

> [!IMPORTANT]
> Dieser Abschnitt erklärt die IDE-Integration mit Steckdosen bei Verwendung von XIB Datendateien. Wenn Sie die Xamarin-Designer für iOS verwenden, dies wird alle ersetzt durch Eingabe eines Namens unter **Identität > Name** im Abschnitt "Eigenschaften" von der IDE, wie unten dargestellt:
>
> [![](images/designeroutlet.png "Ein Name-Element in der iOS-Designer eingeben")](images/designeroutlet.png)
>
>Weitere Informationen zu den iOS-Designer, überprüfen Sie die [Einführung in die iOS-Designer](~/ios/user-interface/designer/introduction.md#how-it-works) Dokument.

Dies wird auf niedriger Ebene beschrieben, wie Steckdosen in c# integriert werden und für fortgeschrittene Benutzer von Xamarin.iOS bereitgestellt. Wenn mithilfe von Visual Studio für Mac die Zuordnung automatisch im Hintergrund mit erfolgt, wird Code auf der Flug für Sie generiert.

Beim Entwerfen der Benutzeroberfläche mit dem Benutzeroberflächen-Generator werden nur die Darstellung der Anwendung entwerfen, und Sie werden einige standardverbindungen herstellen. Wenn Sie programmgesteuert Informationen abrufen, ändern das Verhalten eines Steuerelements zur Laufzeit oder das Steuerelement zur Laufzeit ändern möchten, ist es erforderlich, um einige Steuerelemente an verwalteten Code zu binden.

Dies erfolgt in wenigen Schritten:

1.  Hinzufügen der **Steckdose Deklaration** auf Ihre **des Dateibesitzer**.
1.  Verbinden Sie das Steuerelement die **des Dateibesitzer**.
1.  Die Benutzeroberfläche sowie die Verbindungen in Ihre XIB/NIB-Datei zu speichern.
1.  NIB-Datei zur Laufzeit zu laden.
1.  Zugriff auf die Variable an.


In der Apple-Dokumentation zum Erstellen von Schnittstellen mit Schnittstelle-Generator werden die Schritte (1) bis (3) behandelt.

Bei Verwendung von Xamarin.iOS müssen Ihre Anwendung eine Klasse erstellen, die von UIViewController abgeleitet wird. Die Implementierung erfolgt er wie folgt:

```csharp
public class MyViewController : UIViewController {
    public MyViewController (string nibName, NSBundle bundle) : base (nibName, bundle)
    {
        // You can have as many arguments as you want, but you need to call
        // the base constructor with the provided nibName and bundle.
    }
}
```

Führen Sie dann um Ihre ViewController aus einer NIB-Datei zu laden, Sie dies:

```csharp
var controller = new MyViewController ("HelloWorld", NSBundle.MainBundle, this);
```

Dadurch wird die Benutzeroberfläche von der NIB geladen. Für den Zugriff auf die Steckdosen, ist es nun erforderlich, die Laufzeit mitteilen, die wir darauf zugreifen möchten. Hierzu die `UIViewController` Unterklasse benötigt, deklarieren die Eigenschaften und diese mit dem [verbinden]-Attribut versehen. Und zwar so:

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

Die Implementierung der Eigenschaft ist dasjenige, das tatsächlich abruft und speichert den Wert für den eigentlichen systemeigenen Typ.

Sie müssen nicht zu diesem Gedanken machen, wenn Visual Studio für Mac und InterfaceBuilder verwenden. Visual Studio für Mac spiegelt automatisch alle deklarierten Steckdosen durch Code in einer partiellen Klasse, die als Teil Ihres Projekts kompiliert wird.

#### <a name="selectors"></a>Selektoren

Eine Kernkomponente von Objective-C-Programmierung ist Selektoren. Sie werden häufig über APIs stammen, die erfordern, dass Sie einen Selektor übergeben oder den Code So reagieren Sie auf einen Selektor erwartet.

Erstellen neue Selektoren in c# ist sehr einfach: Sie erstellen Sie einfach eine neue Instanz der dem `ObjCRuntime.Selector` Klasse, und verwenden Sie das Ergebnis in einer beliebigen Stelle in der API, die dies erfordert. Zum Beispiel:

```csharp
var selector_add = new Selector ("add:plus:");
```

Für eine C#-Methode reagieren auf einen Anruf Selektor, müssen sie erbt von der `NSObject` Typ und die C#-Methode müssen mit dem Selektor Namen ergänzt werden die `[Export]` Attribut. Zum Beispiel:

```csharp
public class MyMath : NSObject {
    [Export ("add:plus:")]
    int Add (int first, int second)
    {
         return first + second;
    }
}
```

Beachten Sie diese Auswahl Namen **müssen** genau übereinstimmen, einschließlich aller intermediate und nachfolgende Doppelpunkte (":"), falls vorhanden.

#### <a name="nsobject-constructors"></a>NSObject Konstruktoren

Die meisten Klassen in Xamarin.iOS, die davon Herleiten `NSObject` Konstruktoren, die spezifisch für die Funktionalität des Objekts verfügbar, werden auch verschiedene Konstruktoren, die nicht sofort bemerkbar machen jedoch.

Die Konstruktoren werden wie folgt verwendet:

```csharp
public Foo (IntPtr handle)
```

Dieser Konstruktor wird verwendet, zum Instanziieren Ihrer Klasse, wenn die Laufzeit Ihre Klasse eine nicht verwaltete Klasse zuordnen muss. Dies geschieht, wenn Sie eine XIB/NIB-Datei zu laden.  An diesem Punkt Objective-C-Laufzeit wird ein Objekt in die nicht verwaltete Umgebung erstellt haben, und dieser Konstruktor wird zum Initialisieren der verwalteten Seite aufgerufen werden.

In der Regel alles, was Sie tun müssen ist mit dem Handle-Parameter, und klicken Sie im Text der Basiskonstruktor aufrufen, müssen Sie keine Initialisierung, die erforderlich sind.

```csharp
public Foo ()
```

Dies ist der Standardkonstruktor für eine Klasse, und in Xamarin.iOS Klassen bereitgestellt, die Foundation.NSObject-Klasse und alle Klassen in der Zwischenzeit initialisiert, und am Ende, verkettet ist dies für den Objective-C- `init` Methode für die Klasse.

```csharp
public Foo (NSObjectFlag x)
```

Dieser Konstruktor wird zum Initialisieren der Instanz, jedoch verhindern, dass des Codes die Objective-C "Init"-Methode aufrufen, am Ende. In der Regel verwendet, wenn Sie bereits für die Initialisierung registriert ist (bei Verwendung von `[Export]` für Ihr Konstruktor) oder wenn Sie Ihre Initialisierung durch eine andere Mittel bereits getan haben.

```csharp
public Foo (NSCoder coder)
```

Dieser Konstruktor wird für die Fälle bereitgestellt, in dem das Objekt aus einer NSCoding Instanz initialisiert wird. Weitere Informationen finden Sie auf der Apple [Archive und Programmierhandbuch Serialisierung.](http://developer.apple.com/mac/library/documentation/Cocoa/Conceptual/Archiving/index.html#//apple_ref/doc/uid/10000047i)

#### <a name="exceptions"></a>Ausnahmen

Die Xamarin.iOS-API-Entwurf wird Objective-C-Ausnahmen nicht als C#-Ausnahmen ausgelöst werden. Der Entwurf erzwingt, dass keine Garbage in erster Linie auf der Welt Objective-C gesendet werden, sodass alle Ausnahmen, die erzeugt werden müssen von der Bindung selbst erstellt werden, damit ungültige Daten je übergeben der Welt Objective-C.

#### <a name="notifications"></a>Benachrichtigungen

In iOS und OS X können Entwickler Benachrichtigungen abonnieren, die von der zugrunde liegenden Plattform übertragen werden. Dies erfolgt mithilfe der `NSNotificationCenter.DefaultCenter.AddObserver` Methode. Die `AddObserver` Methode akzeptiert zwei Parameter, die eine ist die Benachrichtigung, die Sie abonnieren möchten; das andere ist die Methode, die aufgerufen werden, wenn die Benachrichtigung ausgelöst wird.

Bei Xamarin.iOS und Xamarin.Mac werden die Schlüssel für die verschiedenen Benachrichtigungen für die Klasse gehostet, die die Benachrichtigungen auslöst. Z. B. die Benachrichtigungen ausgelöst durch den `UIMenuController` gehostet werden, als `static NSString` Eigenschaften in der `UIMenuController` Klassen, die mit dem Namen "Benachrichtigung" enden.

### <a name="memory-management"></a>Speicherverwaltung

Xamarin.iOS hat eine Garbage Collection, die kümmern wird für Sie Ressourcen freigeben, wenn sie nicht mehr verwendet werden. Zusätzlich zu den Garbage Collector alle Objekte, die abgeleitet `NSObject` implementieren die `System.IDisposable` Schnittstelle.

#### <a name="nsobject-and-idisposable"></a>NSObject und IDisposable

Verfügbarmachen der `IDisposable` Schnittstelle ist eine praktische Möglichkeit zur Unterstützung von Entwicklern in der Freigabe von Objekten, die große Speicherblöcke kapseln können (z. B. eine `UIImage` kann nur ein unverfänglichen Zeiger aussehen, aber zu einem Bild 2 MB zeigen werden konnte ) und andere wichtigen und begrenzten Ressourcen (z. B. ein video decodieren Puffer).

NSObject implementiert die IDisposable-Schnittstelle und auch die [.NET Dispose-Muster](http://msdn.microsoft.com/en-us/library/fs2xkftw.aspx). Dadurch können Entwickler diese Unterklasse NSObject außer Kraft setzen das Dispose-Verhalten und ihre eigenen Ressourcen bei Bedarf freigeben. Betrachten Sie beispielsweise diese View-Controller, der um eine Reihe von Bildern bleibt:

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

Wenn ein verwaltetes Objekt verworfen wird, ist es nicht mehr nützlich. Sie möglicherweise immer noch einen Verweis auf die Objekte, aber das Objekt ist an diesem Punkt zum Verteilungsvorgang ungültig. Einige .NET APIs sicherstellen dies, indem Sie eine "ObjectDisposedException" ausgelöst wird, wenn Sie versuchen, z. B. den Zugriff auf alle Methoden für ein verworfenes Objekt:

```csharp
var image = UIImage.FromFile ("demo.png");
image.Dispose ();
image.XXX = false;  // this at this point is an invalid operation
```

Auch wenn Sie die Variable "Image" weiterhin zugreifen können, ist es tatsächlich einen ungültigen Verweis und nicht mehr verweist auf das Objective-C-Objekt, das das Bild gespeichert.

Aber verwerfen eines Objekts in c# bedeutet nicht, dass das Objekt unbedingt zerstört wird. Alles, was Sie tun wird den Verweis freizugeben, den C#-auf das Objekt hat. Es ist möglich, dass Kakao-Umgebung beibehalten haben, für die eigene Verwendung verweisen kann. Z. B. Festlegen einer UIImageView Image-Eigenschaft zu einem Bild, und dann verwerfen Sie das Bild, das zugrunde liegende UIImageView hatte gebracht seinen eigenen Verweis und hält einen Verweis auf dieses Objekt aus, bis er abgeschlossen ist verwenden.

#### <a name="when-to-call-dispose"></a>Wenn Dispose-Funktion aufrufen

Sie müssen die Dispose aufrufen, wenn Sie Mono in erste Ihres Objekts rid benötigen. Eine Verwendungsmöglichkeit Groß-/Kleinschreibung wird bei Mono nicht bekannt ist, dass Ihre NSObject tatsächlich einen Verweis auf eine wichtige Ressource, z. B. Arbeitsspeicher oder einen Pool Informationen enthalten kann. In diesen Fällen sollten Sie Dispose zum sofort freigeben des Verweises auf den Speicher, nicht zum Ausführen eines Garbage Collection-Zyklus Mono gewartet aufrufen.

Wenn Mono erstellt intern [NSString verweist auf aus C#-Zeichenfolgen](~/ios/internals/api-design/nsstring.md), wird er sofort, um den Arbeitsaufwand zu reduzieren, die der Garbage Collector nur muss dispose. Je weniger Objekte um, je schneller der globale Katalogserver beinhalten werden ausgeführt.

#### <a name="when-to-keep-references-to-objects"></a>Wenn Verweise auf Objekte zu halten.

Ein Nebeneffekt, die automatische Speicherverwaltung ist, dass der globale Katalogserver nicht verwendete Objekte beseitigen wird so lange keine Verweise auf diese vorhanden sind. Dies kann überraschend Nebeneffekte, manchmal z. B. verwendet, wenn Sie eine lokale Variable, um Ihren Ansicht der obersten Ebene in der Regel erstellen oder die Fenster auf oberster Ebene, und müssen diese verschwinden hinter Ihrem Back.

Wenn Sie keinen Verweis in Ihrer statischen oder Nachrichteninstanzvariablen an Ihren Objekten beibehalten werden, Mono angerufen stört die Dispose()-Methode darauf, und gibt sie den Verweis auf das Objekt frei. Da der Verweis nur ausstehende möglicherweise, zerstört Objective-C-Laufzeit das Objekt für Sie.

## <a name="related-links"></a>Verwandte Links

- [Binden von Feldern](~/cross-platform/macios/binding/objective-c-libraries.md#Binding_Fields)
