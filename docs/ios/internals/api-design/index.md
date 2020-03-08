---
title: Xamarin. IOS-API-Design
description: Leitfäden, die zum Entwerfen der xamarin. IOS-APIs und deren Beziehung zu "Ziel-C" verwendet wurden.
ms.prod: xamarin
ms.assetid: 322D2724-AF27-6FFE-BD21-AA1CFE8C0545
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/21/2017
ms.openlocfilehash: a2435b30b7d5b468fca6c55d295c87b9a0d20652
ms.sourcegitcommit: eedc6032eb5328115cb0d99ca9c8de48be40b6fa
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/07/2020
ms.locfileid: "78915179"
---
# <a name="xamarinios-api-design"></a>Xamarin. IOS-API-Design

Zusätzlich zu den Basisklassen Bibliotheken, die Teil von Mono sind, werden in [xamarin. IOS](~/ios/index.yml) Bindungen für verschiedene IOS-APIs geliefert, damit Entwickler Native IOS-Anwendungen mit Mono erstellen können.

Im Kern von xamarin. IOS gibt es eine Interop-Engine, die die C# Welt mit der Ziel-C-Welt verbindet, sowie Bindungen für die auf IOS C basierenden APIs wie CoreGraphics und [OpenGL es](#opengles).

Die Laufzeit auf niedriger Ebene für die Kommunikation mit dem Ziel-C-Code befindet sich in [MonoTouch. objcruntime](#objcruntime). Darüber hinaus werden Bindungen für [Foundation](#foundation), CoreFoundation und [UIKit](#uikit) bereitgestellt.

## <a name="design-principles"></a>Entwurfsprinzipien

Dies sind einige unserer Entwurfs Prinzipien für die xamarin. IOS-Bindungen (Sie gelten auch für xamarin. Mac, Mono-Bindungen für Ziel-C unter macOS):

- Befolgen Sie die [Framework-Entwurfs Richtlinien](https://docs.microsoft.com/dotnet/standard/design-guidelines)
- Ermöglicht Entwicklern die Unterklasse von Ziel-C-Klassen:

  - Von einer vorhandenen Klasse ableiten
  - Abrufen des basiskonstruktors für die Verkettung
  - Überschreiben von Methoden sollte mit C#dem Außerkraftsetzungs System durchgeführt werden
  - Die Unterklassen sollten mit C# standardkonstrukten funktionieren.

- Machen Sie keine Entwickler für Ziel-C-Selektoren verfügbar.
- Bereitstellen eines Mechanismus zum Abrufen beliebiger Ziel-C-Bibliotheken
- Einfache und harte Ziele für allgemeine Ziel-c-Aufgaben
- Ziel-C-Eigenschaften als C# Eigenschaften verfügbar machen
- Stellen Sie eine stark typisierte API bereit:

  - Erhöhen der Typsicherheit
  - Minimierung von Laufzeitfehlern
  - IntelliSense der IDE bei Rückgabe Typen
  - Ermöglicht die IDE-Popup Dokumentation

- In-IDE-Untersuchung der APIs fördern:

  - Anstatt beispielsweise ein schwach typisiertes Array wie das folgende verfügbar zu machen:

    ```objc
    NSArray *getViews
    ```

    Machen Sie einen starken Typ wie folgt verfügbar:

    ```csharp
    NSView [] Views { get; set; }
    ```

    Dadurch haben Visual Studio für Mac die Möglichkeit, die automatische Vervollständigung beim Durchsuchen der API durchzuführen, alle `System.Array` Vorgänge für den zurückgegebenen Wert verfügbar zu machen, und der Rückgabewert kann an LINQ teilnehmen.

- Native C# Typen:

  - [`NSString` wird `string`](~/ios/internals/api-design/nsstring.md)
  - Umwandeln von `int` und `uint` Parametern, die C# Enumerationen und C# Enumerationen mit `[Flags]` Attributen sein sollten
  - Stellen Sie Arrays anstelle von typneutralen `NSArray` Objekten als stark typisierte Arrays bereit.
  - Für Ereignisse und Benachrichtigungen können Sie den Benutzern folgende Möglichkeiten einräumen:

    - Standardmäßig eine stark typisierte Version
    - Eine schwach typisierte Version für erweiterte Anwendungsfälle

- Unterstützen Sie das Ziel-C-delegatmuster:

  - C#Ereignis System
  - C# Verfügbar machen von Delegaten (Lambdas, anonyme Methoden und `System.Delegate`) an Ziel-C-APIs als Blöcke

### <a name="assemblies"></a>Assemblys

Xamarin. IOS enthält eine Reihe von Assemblys, die das *xamarin. IOS-Profil*bilden. Die [Seite](~/cross-platform/internals/available-assemblies.md) "Assemblys" enthält weitere Informationen.

### <a name="major-namespaces"></a>Haupt Namespaces

#### <a name="objcruntime"></a>ObjCRuntime

Der [objcruntime](xref:ObjCRuntime) -Namespace ermöglicht es Entwicklern, die Welten C# zwischen und Ziel-C zu überbrücken.
Dabei handelt es sich um eine neue Bindung, die speziell für IOS entwickelt wurde, basierend auf der Verwendung von Cocoa # und GTK #.

#### <a name="foundation"></a>Foundation

Der [Foundation](xref:Foundation) -Namespace stellt die grundlegenden Datentypen bereit, die für die Zusammenarbeit mit dem Ziel-c Foundation-Framework entwickelt wurden, das Teil von IOS ist, und es ist die Basis für die objektorientierte Programmierung in Ziel-c.

Xamarin. IOS-Spiegelungen in C# der Klassenhierarchie von "Ziel-C". Beispielsweise kann das NSObject der Ziel-C-Basisklasse von C# über [Foundation. NSObject](xref:Foundation.NSObject)verwendet werden.

Obwohl dieser Namespace Bindungen für die zugrunde liegenden Ziel-C Foundation-Typen bereitstellt, haben wir in einigen Fällen die zugrunde liegenden Typen .NET-Typen zugeordnet. Beispiel:

- Anstatt NSString und [NSArray zu verwenden](https://developer.apple.com/library/ios/#documentation/Cocoa/Reference/Foundation/Classes/NSArray_Class/NSArray.html), macht die Common Language Runtime diese C#in der gesamten API als [Zeichen](xref:System.String)folgen-und stark typisierte [Array](xref:System.Array)-e verfügbar.

- Hier werden verschiedene Hilfsobjekte bereitgestellt, die es Entwicklern ermöglichen, Ziel-C-APIs von Drittanbietern, andere IOS-APIs oder APIs zu binden, die derzeit nicht von xamarin. IOS gebunden sind.

Weitere Informationen zu Bindungs-APIs finden Sie im Abschnitt [xamarin. IOS Binding Generator](~/cross-platform/macios/binding/binding-types-reference.md) .

##### <a name="nsobject"></a>Nsobjekt

Der [NSObject](xref:Foundation.NSObject) -Typ ist die Grundlage für alle Ziel-C-Bindungen. Xamarin. IOS-Typen spiegeln zwei Klassentypen aus den IOS-cocoatouch-APIs: die C-Typen (in der Regel als CoreFoundation-Typen bezeichnet) und die Ziel-C-Typen (diese werden alle von der NSObject-Klasse abgeleitet).

Für jeden Typ, der einen nicht verwalteten Typ widerspiegelt, ist es möglich, das Native Objekt über die [handle](xref:Foundation.NSObject.Handle) -Eigenschaft abzurufen.

Während Mono Garbage Collection für alle Objekte bereitstellt, implementiert die `Foundation.NSObject` die [System. iverwerfbare](xref:System.IDisposable) Schnittstelle. Dies bedeutet, dass Sie die Ressourcen eines beliebigen beliebigen NSObject explizit freigeben können, ohne darauf warten zu müssen, dass der Garbage Collector gestartet wird. Dies ist wichtig, wenn Sie große NSObjects verwenden, z. b. UIimages, die möglicherweise Zeiger auf große Datenblöcke enthalten.

Wenn Ihr Typ deterministische Finalisierung ausführen muss, überschreiben Sie die [Methode NSObject. verwerfen (bool)](xref:Foundation.NSObject.Dispose(System.Boolean)) , da der zu löschende Parameter "bool disposing" lautet. Wenn dieser Wert auf "true" festgelegt ist, bedeutet dies, dass die verwerfen-Methode aufgerufen wird, da der Benutzer explizit "verwerfen ()" für das Objekt aufgerufen hat. Wenn der Wert false ist, bedeutet dies, dass die verwerfen-Methode (bool disposing) vom Finalizer im Finalizerthread aufgerufen wird.

##### <a name="categories"></a>Kategorien

Ab xamarin. IOS 8,10 ist es möglich, Ziel-C-Kategorien aus C#zu erstellen.

Dies erfolgt mithilfe des `Category`-Attributs, wobei der Typ angegeben wird, der als Argument für das Attribut erweitert werden soll. Im folgenden Beispiel wird z. b. NSString erweitert.

```csharp
[Category (typeof (NSString))]
```

Jede kategoriemethode verwendet den normalen Mechanismus zum Exportieren von Methoden zu "Ziel-C" mithilfe des `Export` Attributs:

```csharp
[Export ("today")]
public static string Today ()
{
    return "Today";
}
```

Alle verwalteten Erweiterungs Methoden müssen statisch sein, aber es ist möglich, Ziel-C-Instanzmethoden mithilfe der Standard Syntax für Erweiterungs Methoden C#in zu erstellen:

```csharp
[Export ("toUpper")]
public static string ToUpper (this NSString self)
{
    return self.ToString ().ToUpper ();
}
```

und das erste Argument der Erweiterungsmethode ist die Instanz, für die die Methode aufgerufen wurde.

Beispiel:

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

In diesem Beispiel wird eine native toupperinstanzmethode zur NSString-Klasse hinzugefügt, die von "Ziel-C" aufgerufen werden kann.

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

Ein Szenario, in dem dies hilfreich ist, besteht darin, eine Methode zu einem vollständigen Satz von Klassen in Ihrer Codebasis hinzuzufügen. Dadurch werden z. b. alle `UIViewController` Instanzen melden, dass Sie sich drehen können:

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

Preserveattribute ist ein benutzerdefiniertes Attribut, das verwendet wird, um mberühren – das Bereitstellungs Tool xamarin. IOS – zum Beibehalten eines Typs oder eines Members eines Typs während der Phase, in der die Anwendung verarbeitet wird, um die Größe zu verringern.

Jeder Member, der nicht statisch durch die Anwendung verknüpft ist, muss entfernt werden. Daher wird dieses Attribut verwendet, um Member zu kennzeichnen, auf die nicht statisch verwiesen wird, die aber noch von Ihrer Anwendung benötigt werden.

Wenn Sie beispielsweise Typen dynamisch instanziieren, sollten Sie den Standardkonstruktor Ihrer Typen beibehalten. Wenn Sei die XML-Serialisierung verwenden, sollten Sie die Eigenschaften Ihrer Typen beibehalten.

Sie können dieses Attribut auf alle Member eines Typs oder auf den Typ selbst anwenden. Wenn Sie den gesamten Typ beibehalten möchten, können Sie die Syntax [Preserve (AllMembers = true)] für den Typ verwenden.

#### <a name="uikit"></a>UIKit

Der [UIKit](xref:UIKit) -Namespace enthält eine eins-zu-Eins-Zuordnung zu allen UI-Komponenten, die cocoatouch in Form von C# Klassen bilden. Die API wurde so geändert, dass Sie den in der C# Sprache verwendeten Konventionen entspricht.

C#Delegaten werden für allgemeine Vorgänge bereitgestellt. Weitere Informationen finden [Sie im Abschnitt](#delegates) Delegaten.

#### <a name="opengles"></a>OpenGLES

Für opengles verteilen wir eine [geänderte Version](xref:OpenTK) der [opentk](https://opentk.net/) -API, eine objektorientierte Bindung an OpenGL, die so geändert wurde, dass CoreGraphics-Datentypen und-Strukturen verwendet werden, und stellen nur die Funktionalität bereit, die unter IOS verfügbar ist.

Die opengles 1,1-Funktionalität ist über den [ES11.gl-Typ](xref:OpenTK.Graphics.ES11.GL)verfügbar.

Die opengles 2,0-Funktionalität ist über den [ES20.gl-Typ](xref:OpenTK.Graphics.ES20.GL)verfügbar.

Die opengles 3,0-Funktionalität ist über den [ES30.gl-Typ](xref:OpenTK.Graphics.ES30.GL)verfügbar.

### <a name="binding-design"></a>Bindungs Entwurf

Xamarin. IOS ist nicht lediglich eine Bindung an die zugrunde liegende Ziel-C-Plattform. Dadurch wird das .net-Typsystem und das Dispatch-System C# auf eine bessere Mischung und ein besseres Ziel-C erweitert.

Ebenso wie P/aufrufen ein nützliches Tool zum Aufrufen nativer Bibliotheken unter Windows und Linux, oder da die Unterstützung von IJW für COM-Interop unter Windows verwendet werden kann, erweitert xamarin. IOS die C# Laufzeit, um das Binden von Objekten an Ziel-C-Objekte zu unterstützen.

Die Diskussion in den nächsten Abschnitten ist für Benutzer, die xamarin. IOS-Anwendungen erstellen, nicht erforderlich, hilft Entwicklern jedoch dabei, zu verstehen, wie alles funktioniert, und unterstützt Sie beim Erstellen komplizierterer Anwendungen.

#### <a name="types"></a>Typen

Wenn es sinnvoll ist, C# werden Typen anstelle von Basis Typen auf niedriger Ebene im C# Universum verfügbar gemacht.  Dies bedeutet, dass [die API den C# Typ "String" anstelle von NSString verwendet](~/ios/internals/api-design/nsstring.md) und stark typisierte C# Arrays verwendet, anstatt NSArray verfügbar zu machen.

Im Allgemeinen wird das zugrunde liegende `NSArray` Objekt im xamarin. IOS-und xamarin. Mac-Design nicht verfügbar gemacht. Stattdessen konvertiert die Common Language Runtime `NSArray`s automatisch in stark typisierte Arrays einer `NSObject` Klasse. Daher macht xamarin. IOS keine schwach typisierte Methode wie GetViews verfügbar, um einen NSArray zurückzugeben:

```csharp
NSArray GetViews ();
```

Stattdessen stellt die Bindung einen stark typisierten Rückgabewert wie folgt bereit:

```csharp
UIView [] GetViews ();
```

In `NSArray`stehen einige Methoden zur Verfügung, für die Fälle, in denen Sie möglicherweise eine `NSArray` direkt verwenden möchten, aber ihre Verwendung wird in der API-Bindung nicht empfohlen.

Außerdem haben wir im **Classic API** statt `CGRect`, `CGPoint` und `CGSize` von der CoreGraphics-API zur Verfügung zu stellen, die Sie durch die `System.Drawing`-Implementierungen `RectangleF`, `PointF` und `SizeF` ersetzt haben, da Sie Entwicklern den vorhandenen OpenGL-Code beibehält, der opentk verwendet. Wenn Sie die neue 64-Bit- **Unified API**verwenden, sollte die CoreGraphics-API verwendet werden.

#### <a name="inheritance"></a>Vererbung

Der xamarin. IOS-API-Entwurf ermöglicht es Entwicklern, Native Ziel-C-Typen auf die gleiche Weise zu erweitern C# , wie Sie einen Typ erweitern, indem das Schlüsselwort "override" für eine abgeleitete Klasse verwendet wird, und mit dem Schlüsselwort " C# Base" eine Verkettung für die Basis Implementierung durchführt.

Dieser Entwurf ermöglicht Entwicklern, den Umgang mit Ziel-c-Selektoren im Rahmen des Entwicklungsprozesses zu vermeiden, da das gesamte Ziel-c-System bereits in die xamarin. IOS-Bibliotheken integriert ist.

#### <a name="types-and-interface-builder"></a>Typen und Interface Builder

Wenn Sie .NET-Klassen erstellen, bei denen es sich um Instanzen von Typen handelt, die von Interface Builder erstellt wurden, müssen Sie einen Konstruktor bereitstellen, der einen einzelnen `IntPtr` Parameter annimmt.
Dies ist erforderlich, um die verwaltete Objektinstanz an das nicht verwaltete Objekt zu binden.
Der Code besteht aus einer einzelnen Zeile, wie z. b.:

```csharp
public partial class void MyView : UIView {
   // This is the constructor that you need to add.
   public MyView (IntPtr handle) : base (handle) {}
}
```

#### <a name="delegates"></a>Delegaten

Ziel-C und C# haben unterschiedliche Bedeutungen für den Word-Delegaten in jeder Sprache.

In der Ziel-C-Welt und in der Dokumentation, die Sie online über cocoatouch finden werden, ist ein Delegat in der Regel eine Instanz einer Klasse, die auf einen Satz von Methoden antwortet. Dies ähnelt einer C# -Schnittstelle, wobei der Unterschied darin besteht, dass die Methoden nicht immer obligatorisch sind.

Diese Delegaten spielen eine wichtige Rolle bei UIKit und anderen cocoatouch-APIs. Sie werden zum Ausführen verschiedener Aufgaben verwendet:

- Zum Bereitstellen von Benachrichtigungen für Ihren Code (ähnlich wie bei C# der Ereignis Übermittlung in oder GTK +).
- Zum Implementieren von Modellen für Steuerelemente zur Datenvisualisierung.
- , Um das Verhalten eines Steuer Elements zu steuern.

Das-Programmier Muster wurde entworfen, um die Erstellung abgeleiteter Klassen zu minimieren, um das Verhalten eines Steuer Elements zu ändern. Diese Lösung ist in der Praxis ähnlich, wie andere GUI-Toolkits im Laufe der Jahre ausgeführt wurden: GTK-Signale, Qt-Slots, WinForms-Ereignisse, WPF/Silverlight-Ereignisse usw. Um zu vermeiden, dass Hunderte von Schnittstellen vorhanden sind (eine für jede Aktion) oder dass Entwickler zu viele Methoden implementieren müssen, die Sie nicht benötigen, unterstützt Ziel-C optionale Methoden Definitionen. Dies unterscheidet sich C# von Schnittstellen, für die alle Methoden implementiert werden müssen.

In Ziel-C-Klassen werden Sie feststellen, dass Klassen, die dieses Programmier Muster verwenden, eine Eigenschaft verfügbar machen, die in der Regel als `delegate`bezeichnet wird, die erforderlich ist, um die obligatorischen Teile der Schnittstelle zu implementieren, und NULL (oder mehr) der optionalen Teile.

In xamarin. IOS werden drei gegenseitig ausschließende Mechanismen zur Bindung an diese Delegaten angeboten:

1. [Über Ereignisse](#via-events).
2. [Stark typisiert über eine `Delegate`-Eigenschaft](#strongly-typed-via-a-delegate-property)
3. [Lose Typisierung über eine `WeakDelegate`-Eigenschaft](#loosely-typed-via-the-weakdelegate-property)

Betrachten Sie z. b. die UIWebView-Klasse. Dies wird an eine uiwebviewdelegatinstanz gesendet, die der delegateigenschaft zugewiesen wird.

##### <a name="via-events"></a>Via-Ereignisse

Für viele Typen erstellt xamarin. IOS automatisch einen geeigneten Delegaten, der die `UIWebViewDelegate` Aufrufe an C# Ereignisse weitergibt. Für `UIWebView`:

- Die webviewdidstartload-Methode wird dem Ereignis [UIWebView. LoadStarted](xref:UIKit.UIWebView.LoadStarted) zugeordnet.
- Die webviewdidfinishload-Methode wird dem Ereignis [UIWebView. loadbeendeten](xref:UIKit.UIWebView.LoadFinished) zugeordnet.
- Die Methode WebView: didfailloadwitherror ist dem Ereignis [UIWebView. LoadError](xref:UIKit.UIWebView.LoadError) zugeordnet.

Dieses einfache Programm zeichnet z. b. die Start-und Endzeit beim Laden einer Webansicht auf:

```csharp
DateTime startTime, endTime;
var web = new UIWebView (new CGRect (0, 0, 200, 200));
web.LoadStarted += (o, e) => startTime = DateTime.Now;
web.LoadFinished += (o, e) => endTime = DateTime.Now;
```

##### <a name="via-properties"></a>Via-Eigenschaften

Ereignisse sind nützlich, wenn es möglicherweise mehr als einen Abonnenten für das Ereignis gibt. Ereignisse sind auch auf Fälle beschränkt, in denen kein Rückgabewert aus dem Code vorhanden ist.

Für Fälle, in denen erwartet wird, dass der Code einen Wert zurückgibt, haben wir uns stattdessen für die Eigenschaften entschieden. Dies bedeutet, dass zu einem bestimmten Zeitpunkt in einem Objekt nur eine Methode festgelegt werden kann.

Beispielsweise können Sie diesen Mechanismus verwenden, um die Tastatur auf dem Bildschirm des Handlers für einen `UITextField`zu schließen:

```csharp
void SetupTextField (UITextField tf)
{
    tf.ShouldReturn = delegate (textfield) {
        textfield.ResignFirstResponder ();
        return true;
    }
}
```

In diesem Fall nimmt die `ShouldReturn`-Eigenschaft des `UITextField`als Argument einen Delegaten an, der einen booleschen Wert zurückgibt, und bestimmt, ob das Textfeld eine Aktion ausführen soll, wenn die Rückgabe Schaltfläche gedrückt wird. In unserer Methode wird für den Aufrufer " *true* " zurückgegeben, aber wir entfernen die Tastatur auch aus dem Bildschirm (Dies geschieht, wenn das Textfeld `ResignFirstResponder`aufruft).

##### <a name="strongly-typed-via-a-delegate-property"></a>Stark typisiert über eine delegateigenschaft

Wenn Sie keine Ereignisse verwenden möchten, können Sie eine eigene [uiwebviewdelegat-unter](xref:UIKit.UIWebViewDelegate) Klasse angeben und Sie der [UIWebView.](xref:UIKit.UIWebView.Delegate) delegateigenschaft zuweisen. Nachdem UIWebView. Delegat zugewiesen wurde, funktioniert der UIWebView-Ereignis Dispatchmechanismus nicht mehr, und die uiwebviewdelegatmethoden werden aufgerufen, wenn die entsprechenden Ereignisse eintreten.

Mit diesem einfachen Typ wird z. b. die Zeit aufgezeichnet, die zum Laden einer Webansicht benötigt wird:

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

Der obige Code wird in Code wie dem folgenden verwendet:

```csharp
var web = new UIWebView (new CGRect (0, 0, 200, 200));
web.Delegate = new Notifier ();
```

Der obige Vorgang erstellt einen uiwebviewer und weist ihn an, Nachrichten an eine Instanz des Notifizierers zu senden, eine Klasse, die wir erstellt haben, um auf Nachrichten zu antworten.

Dieses Muster wird auch zum Steuern des Verhaltens bestimmter Steuerelemente verwendet, z. b. im UIWebView-Fall, mit der [UIWebView. Schulter dstartload](xref:UIKit.UIWebView.ShouldStartLoad) -Eigenschaft kann die `UIWebView` Instanz steuern, ob die `UIWebView` eine Seite lädt.

Das Muster wird auch verwendet, um die Daten für einige Steuerelemente Bedarfs gesteuert bereitzustellen. Das [uitableview](xref:UIKit.UITableView) -Steuerelement ist beispielsweise ein leistungsfähiges Tabellen Rendering-Steuerelement – und sowohl das Aussehen als auch der Inhalt werden von einer Instanz von " [uitableviewdatasource](xref:UIKit.UITableViewDataSource) " gesteuert.

### <a name="loosely-typed-via-the-weakdelegate-property"></a>Lose Typisierung über die weakdelegateigenschaft

Zusätzlich zur Eigenschaft mit starker Typisierung gibt es auch einen schwachen typisierten Delegaten, der es dem Entwickler ermöglicht, die Dinge bei Bedarf anders zu binden.
Überall dort, wo eine stark typisierte `Delegate` Eigenschaft in der xamarin. IOS-Bindung verfügbar gemacht wird, wird auch eine entsprechende `WeakDelegate` Eigenschaft verfügbar gemacht.

Wenn Sie die `WeakDelegate`verwenden, sind Sie dafür verantwortlich, die Klasse ordnungsgemäß mit dem [Export](xref:Foundation.ExportAttribute) -Attribut zu versehen, um die Auswahl anzugeben. Beispiel:

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

Beachten Sie, dass die Eigenschaft `Delegate` nicht verwendet wird, sobald die `WeakDelegate`-Eigenschaft zugewiesen wurde. Außerdem müssen Sie, wenn Sie die Methode in einer geerbten Basisklasse implementieren, die Sie [exportieren] möchten, eine öffentliche Methode erstellen.

## <a name="mapping-of-the-objective-c-delegate-pattern-to-c"></a>Zuordnung des Ziels-C-delegatmusters zu C\#

Wenn Sie Ziel-C-Beispiele sehen, die wie folgt aussehen:

```objc
foo.delegate = [[SomethingDelegate] alloc] init]
```

Dadurch wird die Sprache angewiesen, eine Instanz der Klasse "somethingdelegat" zu erstellen und zu erstellen und den Wert der delegateigenschaft der foo-Variablen zuzuweisen. Dieser Mechanismus wird von xamarin. IOS unterstützt C# , und die Syntax lautet wie folgt:

```csharp
foo.Delegate = new SomethingDelegate ();
```

In xamarin. IOS haben wir stark typisierte Klassen bereitgestellt, die den Ziel-C-Delegatklassen zugeordnet werden. Um diese zu verwenden, werden Sie die von der xamarin. IOS-Implementierung definierten Methoden Unterklassen und überschreiben. Weitere Informationen zur Funktionsweise finden Sie im Abschnitt "Models" (Modelle) weiter unten.

### <a name="mapping-delegates-to-c"></a>Zuordnung von Delegaten zu C-\#

UIKit verwendet in der Regel Ziele-C-Delegaten in zwei Formen.

Das erste Formular stellt eine Schnittstelle zum Modell einer Komponente bereit. Beispielsweise als Mechanismus zur Bereitstellung von Daten nach Bedarf für eine Sicht, wie z. b. die Datenspeicher Einrichtung für eine Listenansicht.  In diesen Fällen sollten Sie immer eine Instanz der richtigen Klasse erstellen und die Variable zuweisen.

Im folgenden Beispiel stellen wir dem `UIPickerView` eine Implementierung für ein Modell bereit, das Zeichen folgen verwendet:

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

Die zweite Form besteht darin, Benachrichtigungen für Ereignisse bereitzustellen. In diesen Fällen, obwohl wir die API weiterhin in dem oben beschriebenen Formular verfügbar machen, stellen wir C# auch Ereignisse bereit, die für schnelle Vorgänge einfacher zu verwenden sind und in C#Anonyme Delegaten und Lambda-Ausdrücke integriert werden sollten.

Beispielsweise können Sie `UIAccelerometer` Ereignisse abonnieren:

```csharp
UIAccelerometer.SharedAccelerometer.Acceleration += (sender, args) => {
   UIAcceleration acc = args.Acceleration;
   Console.WriteLine ("Time={0} at {1},{2},{3}", acc.Time, acc.X, acc.Y, acc.Z);
}
```

Die beiden Optionen sind verfügbar, wenn Sie sinnvoll sind, aber als Programmierer müssen Sie einen oder den anderen auswählen. Wenn Sie eine eigene Instanz eines stark typisierten Responder/Delegaten erstellen und diese zuweisen, C# sind die Ereignisse nicht funktionsfähig. Wenn Sie die C# Ereignisse verwenden, werden die Methoden in der Responder-/Delegatklasse nie aufgerufen.

Das vorherige Beispiel, das `UIWebView` verwendet, kann mit C# 3,0 Lambdas wie folgt geschrieben werden:

```csharp
var web = new UIWebView (new CGRect (0, 0, 200, 200));
web.LoadStarted += () => { startTime = DateTime.Now; }
web.LoadFinished += () => { endTime = DateTime.Now; }
```

#### <a name="responding-to-events"></a>Reagieren auf Ereignisse

In Ziel-C-Code werden manchmal Ereignishandler für mehrere Steuerelemente und Anbieter von Informationen für mehrere Steuerelemente in derselben Klasse gehostet. Dies ist möglich, da Klassen auf Nachrichten reagieren, und solange Klassen auf Nachrichten reagieren, können Objekte miteinander verknüpft werden.

Wie bereits erwähnt, unterstützt xamarin. IOS sowohl C# das ereignisbasierte Programmiermodell als auch das Ziel-C-delegatmuster, in dem Sie eine neue Klasse erstellen können, die den Delegaten implementiert und die gewünschten Methoden überschreibt.

Es ist auch möglich, das Muster von Ziel C zu unterstützen, bei dem Beantworter für mehrere verschiedene Vorgänge alle in derselben Instanz einer Klasse gehostet werden. Zu diesem Zweck müssen Sie jedoch Features auf niedriger Ebene der xamarin. IOS-Bindung verwenden.

Wenn Sie z. b. möchten, dass Ihre Klasse sowohl auf die `UITextFieldDelegate.textFieldShouldClear`:-Nachricht als auch auf die `UIWebViewDelegate.webViewDidStartLoad`antwortet: in derselben Instanz einer Klasse müssen Sie die [Export]-Attribut Deklaration verwenden:

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

Die C# Namen für die Methoden sind nicht wichtig. alles, was wichtig ist, sind die an das [Export]-Attribut über gebenden Zeichen folgen.

Wenn Sie diese Art der Programmierung verwenden, stellen Sie C# sicher, dass die Parameter den tatsächlichen Typen entsprechen, die vom Lauf Zeit Modul übergeben werden.

#### <a name="models"></a>Modelle

In UIKit-Speichereinrichtungen oder in Respondern, die mithilfe von Hilfsklassen implementiert werden, werden diese in der Regel im Ziel-C-Code als Delegaten bezeichnet und als Protokolle implementiert.

Ziel-C-Protokolle sind wie Schnittstellen, Sie unterstützen jedoch optionale Methoden – das heißt, dass nicht alle Methoden implementiert werden müssen, damit das Protokoll funktioniert.

Es gibt zwei Möglichkeiten, ein Modell zu implementieren. Sie können Sie entweder manuell implementieren oder vorhandene stark typisierte Definitionen verwenden.

Der manuelle Mechanismus ist erforderlich, wenn Sie versuchen, eine Klasse zu implementieren, die nicht von xamarin. IOS gebunden wurde. Das ist ganz einfach:

- Markieren der Klasse für die Registrierung bei der Laufzeit
- Wenden Sie das [Export]-Attribut mit dem tatsächlichen Auswahl Namen für jede Methode an, die Sie überschreiben möchten.
- Instanziieren Sie die-Klasse, und übergeben Sie Sie.

Der folgende Code implementiert beispielsweise nur eine der optionalen Methoden in der uiapplicationdelegatdefinition:

```csharp
public class MyAppController : NSObject {
        [Export ("applicationDidFinishLaunching:")]
        public void FinishedLaunching (UIApplication app)
        {
                SetupWindow ();
        }
}
```

Der Name der Ziel-C-Auswahl ("applicationdidfinishstarts:") wird mit dem Export-Attribut deklariert, und die-Klasse wird mit dem `[Register]`-Attribut registriert.

Xamarin. IOS stellt stark typisierte Deklarationen bereit, die verwendet werden können und für die keine manuelle Bindung erforderlich ist. Um dieses Programmiermodell zu unterstützen, unterstützt die xamarin. IOS-Laufzeit das [Model]-Attribut für eine Klassen Deklaration. Dadurch wird der Laufzeit mitgeteilt, dass Sie nicht alle Methoden in der Klasse verknüpfen sollte, es sei denn, die Methoden werden explizit implementiert.

Dies bedeutet, dass in UIKit die Klassen, die ein Protokoll mit optionalen Methoden darstellen, wie folgt geschrieben werden:

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

Wenn Sie ein Modell implementieren möchten, das nur einige der Methoden implementiert, müssen Sie lediglich die Methoden überschreiben, an denen Sie interessiert sind, und die anderen Methoden ignorieren. Die Laufzeit fügt nur die überschriebenen Methoden ein, nicht die ursprünglichen Methoden der Ziel-C-Welt.

Das Äquivalent zum vorherigen manuellen Beispiel ist:

```csharp
public class AppController : UIApplicationDelegate {
    public override void FinishedLaunching (UIApplication uia)
    {
     ...
    }
}
```

Der Vorteil besteht darin, dass es nicht erforderlich ist, die Ziel-C-Header Dateien zu finden, um den Selektor, die Typen der Argumente oder die C#Zuordnung zu finden und IntelliSense aus Visual Studio für Mac zusammen mit starken Typen zu erhalten.

#### <a name="xib-outlets-and-c"></a>XIb-Outlets und C-\#

> [!IMPORTANT]
> In diesem Abschnitt wird die IDE-Integration in Outlets bei der Verwendung von XIb-Dateien erläutert. Wenn Sie die Xamarin Designer für IOS verwenden, wird dies durch Eingeben eines Namens unter **Identity > Name** im Abschnitt Properties der IDE ersetzt, wie unten dargestellt:
>
> [![](images/designeroutlet.png "Entering an item Name in the iOS Designer")](images/designeroutlet.png#lightbox)
>
>Weitere Informationen zum IOS-Designer finden Sie in der [Einführung in das IOS-Designer](~/ios/user-interface/designer/introduction.md#how-it-works) -Dokument.

Dies ist eine Beschreibung auf niedriger Ebene, wie Outlets in C# integriert werden und für erweiterte Benutzer von xamarin. IOS bereitgestellt werden. Bei der Verwendung Visual Studio für Mac wird die Zuordnung automatisch im Hintergrund durchgeführt, indem der generierte Code auf dem Flug für Sie verwendet wird.

Wenn Sie Ihre Benutzeroberfläche mit Interface Builder entwerfen, entwerfen Sie nur das Aussehen der Anwendung, und es werden einige Standardverbindungen hergestellt. Wenn Sie Informationen Programm gesteuert abrufen, das Verhalten eines Steuer Elements zur Laufzeit ändern oder das Steuerelement zur Laufzeit ändern möchten, müssen einige der Steuerelemente an den verwalteten Code gebunden werden.

Dies erfolgt in wenigen Schritten:

1. Fügen Sie dem **Besitzer der Datei**die **Outlet-Deklaration** hinzu.
1. Verbinden Sie das Steuerelement mit dem **Besitzer der Datei**.
1. Speichern Sie die Benutzeroberfläche und die Verbindungen in ihrer XIb/NIB-Datei.
1. Laden Sie die NIB-Datei zur Laufzeit.
1. Zugreifen auf die-Outlet-Variable.

Die Schritte (1) bis (3) werden in der Apple-Dokumentation zum Aufbauen von Schnittstellen mit Interface Builder behandelt.

Wenn Sie xamarin. IOS verwenden, muss die Anwendung eine Klasse erstellen, die von UIViewController abgeleitet wird. Es wird wie folgt implementiert:

```csharp
public class MyViewController : UIViewController {
    public MyViewController (string nibName, NSBundle bundle) : base (nibName, bundle)
    {
        // You can have as many arguments as you want, but you need to call
        // the base constructor with the provided nibName and bundle.
    }
}
```

Gehen Sie dann wie folgt vor, um den ViewController aus einer NIB-Datei zu laden:

```csharp
var controller = new MyViewController ("HelloWorld", NSBundle.MainBundle, this);
```

Dadurch wird die Benutzeroberfläche aus dem NIB geladen. Um jetzt auf die Outlets zuzugreifen, ist es erforderlich, die Laufzeit darüber zu informieren, dass wir auf Sie zugreifen möchten. Zu diesem Zweck muss die `UIViewController` Unterklasse die Eigenschaften deklarieren und Sie mit dem [Connect]-Attribut versehen. Dies sieht folgendermaßen aus:

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

Die Implementierung der-Eigenschaft ruft den Wert für den eigentlichen systemeigenen Typ ab und speichert diesen.

Sie müssen sich keine Gedanken darüber machen, wenn Sie Visual Studio für Mac und InterfaceBuilder verwenden. Visual Studio für Mac werden alle deklarierten Outlets automatisch mit Code in einer partiellen Klasse, die als Teil des Projekts kompiliert wird, widerspiegeln.

#### <a name="selectors"></a>Selektoren

Ein zentrales Konzept der Ziel-C-Programmierung sind Selectors. Sie werden häufig über APIs verfügen, bei denen Sie einen Selektor übergeben müssen, oder erwarten, dass Ihr Code auf einen Selektor antwortet.

Das Erstellen neuer Selektoren C# in ist sehr einfach – Sie erstellen einfach eine neue Instanz der `ObjCRuntime.Selector`-Klasse und verwenden das Ergebnis an einer beliebigen Stelle in der API, die dies erfordert. Beispiel:

```csharp
var selector_add = new Selector ("add:plus:");
```

Bei einer C# Methode, die auf einen Auswahl Befehl antwortet, muss Sie vom `NSObject`-Typ erben C# , und die-Methode muss mit dem Auswahl Namen versehen werden, wobei das `[Export]`-Attribut verwendet wird. Beispiel:

```csharp
public class MyMath : NSObject {
    [Export ("add:plus:")]
    int Add (int first, int second)
    {
         return first + second;
    }
}
```

Beachten Sie, dass die Auswahl Amen genau übereinstimmen **müssen** , einschließlich aller zwischen-und nachfolgenden Doppelpunkte (":"), falls vorhanden.

#### <a name="nsobject-constructors"></a>NSObject-Konstruktoren

Die meisten Klassen in xamarin. IOS, die von `NSObject` abgeleitet werden, machen für die Funktionalität des Objekts spezifische Konstruktoren verfügbar. Sie machen jedoch auch verschiedene Konstruktoren verfügbar, die nicht sofort offensichtlich sind.

Die Konstruktoren werden wie folgt verwendet:

```csharp
public Foo (IntPtr handle)
```

Dieser Konstruktor wird verwendet, um die Klasse zu instanziieren, wenn die Laufzeit die Klasse einer nicht verwalteten Klasse zuordnen muss. Dies geschieht, wenn Sie eine XIb/NIB-Datei laden.  An diesem Punkt hat die Ziel-C-Laufzeit ein Objekt in der nicht verwalteten Welt erstellt, und dieser Konstruktor wird aufgerufen, um die verwaltete Seite zu initialisieren.

In der Regel müssen Sie lediglich den Basiskonstruktor mit dem handle-Parameter aufrufen und im Text jede erforderliche Initialisierung ausführen.

```csharp
public Foo ()
```

Dies ist der Standardkonstruktor für eine-Klasse, und in von xamarin. IOS bereitgestellten Klassen initialisiert dies die Foundation. NSObject-Klasse und alle Klassen in between und verkettet diese mit der Ziel-C-`init`-Methode für die-Klasse.

```csharp
public Foo (NSObjectFlag x)
```

Dieser Konstruktor wird verwendet, um die Instanz zu initialisieren, verhindert jedoch, dass der Code am Ende die "init"-Methode von "Ziel-C" aufruft. Dies wird in der Regel verwendet, wenn Sie sich bereits für die Initialisierung registriert haben (wenn Sie `[Export]` auf Ihrem Konstruktor verwenden) oder wenn Sie die Initialisierung bereits über einen anderen Mittelwert durchgeführt haben.

```csharp
public Foo (NSCoder coder)
```

Dieser Konstruktor wird für die Fälle bereitgestellt, in denen das Objekt von einer nscoding-Instanz initialisiert wird.

#### <a name="exceptions"></a>Ausnahmen

Das xamarin. IOS-API-Design gibt keine Ziel-C- C# Ausnahmen als Ausnahmen aus. Der Entwurf erzwingt, dass an erster Stelle kein Garbage Collector an die Ziel-c-Welt gesendet wird und alle Ausnahmen, die erstellt werden müssen, von der Bindung selbst erzeugt werden, bevor ungültige Daten an die Ziel-c-Welt übermittelt werden.

#### <a name="notifications"></a>Benachrichtigungen

Sowohl unter IOS als auch OS X können Entwickler Benachrichtigungen abonnieren, die von der zugrunde liegenden Plattform gesendet werden. Dies erfolgt mithilfe der `NSNotificationCenter.DefaultCenter.AddObserver`-Methode. Die `AddObserver`-Methode nimmt zwei Parameter an: eine ist die Benachrichtigung, die Sie abonnieren möchten. die andere ist die Methode, die aufgerufen werden soll, wenn die Benachrichtigung ausgelöst wird.

Sowohl in xamarin. IOS als auch in xamarin. Mac werden die Schlüssel für die verschiedenen Benachrichtigungen in der Klasse gehostet, die die Benachrichtigungen auslöst. Beispielsweise werden die Benachrichtigungen, die vom `UIMenuController` ausgelöst werden, als `static NSString` Eigenschaften in den `UIMenuController`-Klassen gehostet, die mit dem Namen "Notification" enden.

### <a name="memory-management"></a>Arbeitsspeicherverwaltung

Xamarin. IOS verfügt über eine Garbage Collector, die die Freigabe von Ressourcen für Sie übernimmt, wenn Sie nicht mehr verwendet werden. Zusätzlich zum Garbage Collector werden alle Objekte, die von abgeleitet sind, `NSObject` die `System.IDisposable`-Schnittstelle implementieren.

#### <a name="nsobject-and-idisposable"></a>NSObject und iverwerf

Das verfügbar machen der `IDisposable`-Schnittstelle ist eine bequeme Möglichkeit, Entwickler bei der Freigabe von Objekten zu unterstützen, die möglicherweise große Speicherblöcke Kapseln (z. b. ein `UIImage` könnte z. b. wie ein unbedeutender Zeiger aussehen, aber auf ein Image mit 2 Megabyte zeigen) und andere wichtige und begrenzte Ressourcen (z. b. ein Video-Decodierung)

NSObject implementiert die iverwerfbare Schnittstelle und auch das .net-Lösch [Muster](https://msdn.microsoft.com/library/fs2xkftw.aspx). Dadurch können Entwickler, die eine Unterklasse NSObject haben, das Lösch Verhalten außer Kraft setzen und ihre eigenen Ressourcen bei Bedarf freigeben. Betrachten Sie beispielsweise diesen Ansichts Controller, der eine Reihe von Bildern beibehält:

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

Wenn ein verwaltetes Objekt verworfen wird, ist es nicht mehr sinnvoll. Sie haben möglicherweise noch einen Verweis auf die Objekte, aber das Objekt ist für alle Intents und Zwecke, die an dieser Stelle ungültig sind. Einige .NET-APIs stellen dies sicher, indem Sie eine ObjectDisposedException auslösen, wenn Sie versuchen, auf Methoden für ein frei gegebenes Objekt zuzugreifen, z. b.:

```csharp
var image = UIImage.FromFile ("demo.png");
image.Dispose ();
image.XXX = false;  // this at this point is an invalid operation
```

Auch wenn Sie weiterhin auf die Variable "Image" zugreifen können, handelt es sich tatsächlich um einen ungültigen Verweis, der nicht mehr auf das Ziel-C-Objekt verweist, das das Bild enthielt.

Das Löschen eines Objekts in C# bedeutet jedoch nicht, dass das Objekt notwendigerweise zerstört wird. Sie müssen lediglich den Verweis freigeben, C# der auf das Objekt verweist. Möglicherweise hat die Cocoa-Umgebung einen Verweis auf Ihre eigene Verwendung beibehalten. Wenn Sie z. b. die Image-Eigenschaft eines UIImageView-Objekts auf ein Bild festlegen und dann das Abbild verwerfen, hat der zugrunde liegende UIImageView seinen eigenen Verweis übernommen und behält einen Verweis auf dieses Objekt bei, bis die Verwendung dieses Objekts abgeschlossen ist.

#### <a name="when-to-call-dispose"></a>Wann sollte die Freigabe aufgerufen werden?

Sie sollten verwerfen abrufen, wenn Sie Mono benötigen, um Ihr Objekt zu entfernen. Ein möglicher Anwendungsfall ist, wenn Mono nicht weiß, dass Ihr NSObject tatsächlich einen Verweis auf eine wichtige Ressource wie den Arbeitsspeicher oder einen Informationspool enthält. In diesen Fällen sollten Sie verwerfen ausführen, um den Verweis auf den Speicher sofort freizugeben, anstatt auf Mono zu warten, um einen Garbage Collection Cycle auszuführen.

Intern, wenn Mono [NSString-Verweise aus C# ](~/ios/internals/api-design/nsstring.md)Zeichen folgen erstellt, werden diese sofort verworfen, um den Arbeitsaufwand zu reduzieren, den der Garbage Collector erledigen muss. Je weniger Objekte zu behandeln sind, desto schneller wird die GC ausgeführt.

#### <a name="when-to-keep-references-to-objects"></a>Zeitpunkt, zu dem Verweise auf Objekte beibehalten werden sollen

Ein Nebeneffekt der automatischen Speicherverwaltung besteht darin, dass die GC nicht verwendete Objekte löscht, solange keine Verweise darauf vorhanden sind. Dies kann manchmal überraschende Nebeneffekte haben, z. b. Wenn Sie eine lokale Variable erstellen, die den Ansichts Controller auf oberster Ebene oder das Fenster der obersten Ebene enthält, und diese dann hinter dem Hintergrund verschwinden.

Wenn Sie für Ihre Objekte keinen Verweis in den statischen Variablen oder Instanzvariablen beibehalten, ruft Mono die verwerfen ()-Methode auf, und Sie geben den Verweis auf das Objekt frei. Da dies möglicherweise der einzige ausstehende Verweis ist, wird das-Objekt von der Ziel-C-Laufzeit zerstört.

## <a name="related-links"></a>Verwandte Links

- [Bindungs Felder](~/cross-platform/macios/binding/objective-c-libraries.md#Binding_Fields)
