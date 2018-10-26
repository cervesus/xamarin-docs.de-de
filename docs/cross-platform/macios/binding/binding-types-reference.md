---
title: Handbuch für Verweistypen Bindung
description: Dieser Verweis beschreibt verschiedene Attribute und Konzepte, die erforderlich sind, um Informationen zu Problemen beim Erstellen von C# Bindungen für Objective-C-Bibliotheken.
ms.prod: xamarin
ms.assetid: C6618E9D-07FA-4C84-D014-10DAC989E48D
author: conceptdev
ms.author: crdun
ms.date: 03/06/2018
ms.openlocfilehash: 369e1a37cc75bb4d10cc71d8f79ed1dd473378ba
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50119435"
---
# <a name="binding-types-reference-guide"></a>Handbuch für Verweistypen Bindung

In diesem Dokument wird beschrieben, die Liste der Attribute, die können Sie Ihre API-Vertrag-Dateien, um die Bindung steuern mit Anmerkungen versehen und den Code generiert hat

Xamarin.iOS- und Xamarin.Mac-API-Verträge sind in geschrieben C# hauptsächlich in Form, die die Methode, der Objective-C-Code wird definieren, um angezeigt Definitionen für Schnittstellen C#. Der Prozess umfasst eine Mischung aus Schnittstellendeklarationen sowie einige einfache Typdefinitionen, die möglicherweise der API-Vertrag erforderlich. Eine Einführung in Bindungstypen, finden Sie unter unserem Begleithandbuch [Bindung Objective-C-Bibliotheken](~/cross-platform/macios/binding/objective-c-libraries.md).

## <a name="type-definitions"></a>Typdefinitionen

Syntax:

```csharp
[BaseType (typeof (BTYPE))
interface MyType [: Protocol1, Protocol2] {
     IntPtr Constructor (string foo);
}
```

Jede Schnittstelle in Ihrer Vertragsdefinition, die die [ `[BaseType]` ](#BaseTypeAttribute) Attribut, das den Basistyp für das generierte Objekt deklariert. In der oben genannten Deklaration einer `MyType` Klasse C# Typ generiert werden, an ein Objective-C-Typ aufgerufen `MyType`.

Wenn Sie keine Typen nach Typename angeben (im obigen Beispiel `Protocol1` und `Protocol2`) mit der Schnittstelle Vererbung Syntax den Inhalt dieser Schnittstellen werden intern als wären sie Teil des Vertrags für vorhanden `MyType`.
Die Möglichkeit, die Xamarin.iOS-Oberflächen, dass ein Typ, ein Protokoll übernimmt durch inlineverwendung aller Methoden und Eigenschaften, die in das Protokoll in den Typ selbst deklariert wurden.

Im folgenden dargestellt wie die Objective-C-Deklaration für `UITextField` in einem Xamarin.iOS-Vertrag definiert:

```objc
@interface UITextField : UIControl <UITextInput> {

}
```

Würde folgendermaßen geschrieben werden als eine C# API-Vertrag:

```csharp
[BaseType (typeof (UIControl))]
interface UITextField : UITextInput {
}
```

Sie können viele andere Aspekte der codegenerierung steuern, indem Sie andere Attribute auf die Schnittstelle anwenden sowie zum Konfigurieren der [ `[BaseType]` ](#BaseTypeAttribute) Attribut.


### <a name="generating-events"></a>Generieren von Ereignissen

Eine Funktion von der Xamarin.iOS- und Xamarin.Mac-API-Entwurf ist, dass wir die Objective-C--Delegatklasse als zuordnen C# Ereignisse und Rückrufe. Benutzer können in einer Basis pro Instanz auswählen, ob sie, verwenden die Objective-C-Programmierungsmuster, der durch Zuweisen von Eigenschaften wie möchten `Delegate` eine Instanz einer Klasse, die die verschiedenen Methoden implementiert, die die Objective-C-Laufzeit aufgerufen werden musste oder durch Auswählen der C#-Stil, Ereignisse und Eigenschaften.

Lassen Sie uns finden Sie ein Beispiel, das Objective-C-Modell verwenden:

```csharp
bool MakeDecision ()
{
    return true;
}

void Setup ()
{
     var scrollView = new UIScrollView (myRect);
     scrollView.Delegate = new MyScrollViewDelegate ();
     ...
}

class MyScrollViewDelegate : UIScrollViewDelegate {
    public override void Scrolled (UIScrollView scrollView)
    {
        Console.WriteLine ("Scrolled");
    }

    public override bool ShouldScrollToTop (UIScrollView scrollView)
    {
        return MakeDecision ();
    }
}
```

Im obigen Beispiel sehen Sie, dass wir zwei Methoden überschreiben möchten, eine Benachrichtigung, dass ein fortlaufendes Ereignis vorhanden, und die zweite auf, die stattgefunden hat er einen Rückruf, der einen booleschen Wert angewiesen zurückgeben soll die `scrollView` , ob es auf durchgeführt werden sollen die Top oder nicht.

Die C# Modell kann der Benutzer Ihrer Bibliothek mit Benachrichtigungen überwacht die C# Ereignissyntax oder die Eigenschaftensyntax können, um Rückrufe zu verknüpfen, die zum Zurückgeben von Werten erwartet werden.

Dies ist die C# code für die gleiche Funktion sieht aus wie die Verwendung von Lambda-Ausdrücke:

```csharp
void Setup ()
{
    var scrollview = new UIScrollView (myRect);
    // Event connection, use += and multiple events can be connected
    scrollView.Scrolled += (sender, eventArgs) { Console.WriteLine ("Scrolled"); }

    // Property connection, use = only a single callback can be used
    scrollView.ShouldScrollToTop = (sv) => MakeDecision ();
}
```

Da Ereignisse keine Werte zurückgeben (einen void-Rückgabetyp aufweisen), können Sie mehrere Kopien verbinden. Die `ShouldScrollToTop` ist kein Ereignis, es wird stattdessen eine Eigenschaft mit dem Typ `UIScrollViewCondition` die verfügt über folgende Signatur:

```csharp
public delegate bool UIScrollViewCondition (UIScrollView scrollView);
```

Gibt eine `bool` Wert in diesem Fall die Lambda-Syntax kann wir nur den Wert von Zurückgeben der `MakeDecision` Funktion.

Der Bindung-Generator unterstützt wird, Generieren von Ereignissen und Eigenschaften, die eine Klasse wie verknüpfen `UIScrollView` mit seiner `UIScrollViewDelegate` (auch bezeichnet dies die Model-Klasse), dies erfolgt durch Hinzufügen von Kommentaren Ihre [ `[BaseType]` ](#BaseTypeAttribute) Definition mit der `Events` und `Delegates` Parameter (siehe unten). Zusätzlich zum Hinzufügen von Anmerkungen zu den [ `[BaseType]` ](#BaseTypeAttribute) mit diesen Parametern ist es erforderlich, die den Generator, der ein paar weitere Komponenten zu informieren.

Für Ereignisse, die mehr als einen Parameter (in Objective-C-die Konvention ist, dass der erste Parameter in einer "Delegate"-Klasse die Instanz des Sender-Objekts) Geben Sie den Namen, die Sie möchten für die generierte `EventArgs` Klasse sein. Dies erfolgt mit der [ `[EventArgs]` ](#EventArgsAttribute) Attribut für die Deklaration der Methode in Ihrer Modellklasse. Zum Beispiel:

```csharp
[BaseType (typeof (UINavigationControllerDelegate))]
[Model][Protocol]
public interface UIImagePickerControllerDelegate {
    [Export ("imagePickerController:didFinishPickingImage:editingInfo:"), EventArgs ("UIImagePickerImagePicked")]
    void FinishedPickingImage (UIImagePickerController picker, UIImage image, NSDictionary editingInfo);
}
```

Die obige Deklaration generiert eine `UIImagePickerImagePickedEventArgs` abgeleitete Klasse `EventArgs` Packs beide Parameter, die `UIImage` und `NSDictionary`. Der Generator erstellt dies:

```csharp
public partial class UIImagePickerImagePickedEventArgs : EventArgs {
    public UIImagePickerImagePickedEventArgs (UIImage image, NSDictionary editingInfo);
    public UIImage Image { get; set; }
    public NSDictionary EditingInfo { get; set; }
}
```

Es stellt dann die folgenden in das `UIImagePickerController` Klasse:

```csharp
public event EventHandler<UIImagePickerImagePickedEventArgs> FinishedPickingImage { add; remove; }
```

SMO-Methoden, die einen Wert zurückgeben, sind unterschiedlich gebunden. Diese erfordern sowohl einen Namen für die generierte C# Delegaten (die Signatur für die Methode) und einen Standardwert zurück, für den Fall, dass der Benutzer keine Implementierung selbst bietet. Z. B. die `ShouldScrollToTop` Definition ist dies:

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
public interface UIScrollViewDelegate {
    [Export ("scrollViewShouldScrollToTop:"), DelegateName ("UIScrollViewCondition"), DefaultValue ("true")]
    bool ShouldScrollToTop (UIScrollView scrollView);
}
```

Die oben genannten erstellt eine `UIScrollViewCondition` delegieren, die mit der Signatur, die weiter oben gezeigt wurde, und wenn der Benutzer keine Implementierung bereitstellt, wird der zurückgegebene Wert "true" werden.

Zusätzlich zu den [ `[DefaultValue]` ](#DefaultValueAttribute) -Attribut können Sie auch die [ `[DefaultValueFromArgument]` ](#DefaultValueFromArgumentAttribute) Attribut, die den Generator, um den Wert des angegebenen Parameters im Aufruf der oder die zurückzugebenweiterleitet[ `[NoDefaultValue]` ](#NoDefaultValueAttribute) Parameter, der dem Generator angewiesen, dass kein Standardwert vorhanden ist.

<a name="BaseTypeAttribute" />

### <a name="basetypeattribute"></a>BaseTypeAttribute

Syntax:

```csharp
public class BaseTypeAttribute : Attribute {
        public BaseTypeAttribute (Type t);

        // Properties
        public Type BaseType { get; set; }
        public string Name { get; set; }
        public Type [] Events { get; set; }
        public string [] Delegates { get; set; }
        public string KeepRefUntil { get; set; }
}
```

#### <a name="basetypename"></a>BaseType.Name

Sie verwenden die `Name` Eigenschaft, um den Namen zu steuern, die dieser Typ in der Objective-C-Welt zu binden. Dies wird in der Regel verwendet, geben die C# Geben Sie einen Namen mit den .NET Framework-Entwurfsrichtlinien kompatibel ist, aber die ordnet eines Namens in Objective-C, die diese Konvention nicht folgt.

Beispiel im folgenden Fall ordnen wir die Objective-C `NSURLConnection` Typ `NSUrlConnection`, wie Sie den .NET Framework-Entwurfsrichtlinien "Url" anstelle von "URL" verwenden:

```csharp
[BaseType (typeof (NSObject), Name="NSURLConnection")]
interface NSUrlConnection {
}
```

Der angegebene Name wird als Wert verwendet, für die generierte `[Register]` Attribut in der Bindung. Wenn `Name` nicht angegeben ist, den Typ der Kurzname wird verwendet, als Wert für die `[Register]` -Attribut in der generierten Ausgabe.

#### <a name="basetypeevents-and-basetypedelegates"></a>BaseType.Events und BaseType.Delegates

Diese Eigenschaften werden verwendet, um die Generierung von Laufwerk C#-Ereignisse in den generierten Klassen formatieren. Sie werden verwendet, um eine bestimmte Klasse mit der Objective-C "Delegate"-Klasse zu verknüpfen. Sie werden häufig auftreten, in dem eine Klasse eine "Delegate"-Klasse zum Senden von Benachrichtigungen und Ereignissen verwendet. Zum Beispiel eine `BarcodeScanner` müsste eine begleitende `BardodeScannerDelegate` Klasse. Die `BarcodeScanner` Klasse müssten in der Regel eine `Delegate` -Eigenschaft, die Sie eine Instanz von zuweisen würde `BarcodeScannerDelegate` , damit dies funktioniert zwar, Sie möchten für Ihre Benutzer verfügbar machen eine C#-wie Style Senkenereignis-Schnittstelle, und klicken Sie in diesen Fällen verwenden Sie die `Events` und `Delegates` Eigenschaften der [ `[BaseType]` ](#BaseTypeAttribute) Attribut.

Diese Eigenschaften werden immer zusammen festgelegt werden, müssen die gleiche Anzahl von Elementen und synchron gehalten. Die `Delegates` Array enthält eine Zeichenfolge für jeden schwach typisierte Delegaten, die Sie umbrochen werden möchten, und die `Events` Array enthält einen Typ für jeden Typ, der Sie es zuordnen möchten.

```csharp
[BaseType (typeof (NSObject),
           Delegates=new string [] { "WeakDelegate" },
           Events=new Type [] {typeof(UIAccelerometerDelegate)})]
public interface UIAccelerometer {
}

[BaseType (typeof (NSObject))]
[Model][Protocol]
public interface UIAccelerometerDelegate {
}
```


#### <a name="basetypekeeprefuntil"></a>BaseType.KeepRefUntil

Wenn Sie dieses Attribut anwenden, wenn neue Instanzen dieser Klasse erstellt werden, die Instanz dieses Objekts bleiben für bis die Methode, die auf die verwiesen wird durch die `KeepRefUntil` aufgerufen wurde. Dies ist nützlich, um die benutzerfreundlichkeit Ihrer APIs, verbessern, wenn Sie nicht, dass Ihre Benutzer, der einen Verweis auf ein Objekt um beibehalten wird, um Ihren Code verwenden möchten. Der Wert dieser Eigenschaft ist der Name einer Methode in der `Delegate` Klasse, daher müssen Sie dies in Kombination mit verwenden die `Events` und `Delegates` auch Eigenschaften.

Das folgende Beispiel zeigt, wie dies von verwendet wird `UIActionSheet` in Xamarin.iOS:

```csharp
[BaseType (typeof (NSObject), KeepRefUntil="Dismissed")]
[BaseType (typeof (UIView),
           KeepRefUntil="Dismissed",
           Delegates=new string [] { "WeakDelegate" },
           Events=new Type [] {typeof(UIActionSheetDelegate)})]
public interface UIActionSheet {
}

[BaseType (typeof (NSObject))]
[Model][Protocol]
public interface UIActionSheetDelegate {
    [Export ("actionSheet:didDismissWithButtonIndex:"), EventArgs ("UIButton")]
    void Dismissed (UIActionSheet actionSheet, nint buttonIndex);
}
```


### <a name="disabledefaultctorattribute"></a>DisableDefaultCtorAttribute

Wenn dieses Attribut zur Schnittstellendefinition angewendet wird wird es verhindert, dass den Generator erzeugen des Standardkonstruktors.

Verwenden Sie dieses Attribut, wenn Sie das Objekt, das mit einem der anderen Konstruktoren in der Klasse initialisiert werden, benötigen.


### <a name="privatedefaultctorattribute"></a>PrivateDefaultCtorAttribute

Wenn dieses Attribut zur Schnittstellendefinition angewendet wird wird es der Standard-Konstruktor als privat gekennzeichnet. Dies bedeutet, dass Sie weiterhin Objekt dieser Klasse intern von Ihrer Erweiterungsdatei instanziieren können, aber es einfach nicht für Benutzer Ihrer Klasse zugegriffen werden kann.

<a name="CategoryAttribute" />

### <a name="categoryattribute"></a>CategoryAttribute

Binden von Objective-C-Kategorien und diese als verfügbar zu machen, verwenden Sie dieses Attribut auf eine Typdefinition C# Erweiterungsmethoden wie Objective-C-gespiegelt macht die Funktionalität.

Kategorien sind ein Objective-C-Mechanismus verwendet, um den Satz von Methoden und Eigenschaften zur Verfügung stehen, in einer Klasse zu erweitern.   In der Praxis werden sie auf die Funktionalität einer Basisklasse entweder das deduplizierungsverarbeitungsfenster verwendet (z. B. `NSObject`) Wenn ein bestimmtes Framework in verknüpft ist (z. B. `UIKit`), sodass ihre Methoden verfügbar, aber nur, wenn das neue Framework verknüpft ist.   In einigen Fällen werden sie zum Organisieren von Funktionen in einer Klasse von verwendet Funktionen.   Sie ähneln im Geiste, C# Erweiterungsmethoden.

Dies ist eine Kategorie in Objective-c: aussehen sollen

```objc
@interface UIView (MyUIViewExtension)
-(void) makeBackgroundRed;
@end
```

Im obige Beispiel befindet sich auf eine Bibliothek, die Instanzen von auftreten würde `UIView` mit der Methode `makeBackgroundRed`.

Um diese zu binden, können Sie die [ `[Category]` ](#CategoryAttribute) Attribut in einer Schnittstellendefinition.   Bei Verwendung der [ `[Category]` ](#CategoryAttribute) Attribut, die Bedeutung des das [ `[BaseType]` ](#BaseTypeAttribute) Attribut ändert, die an der Basisklasse zu erweitern, dass der zu erweiternden Typs verwendet werden.

Im folgenden dargestellt wie die `UIView` Erweiterungen gebunden sind und die Umwandlung in C# Erweiterungsmethoden:

```csharp
[BaseType (typeof (UIView))]
[Category]
interface MyUIViewExtension {
    [Export ("makeBackgroundRed")]
    void MakeBackgroundRed ();
}
```

Die oben genannten erstellt eine `MyUIViewExtension` einer Klasse enthält die `MakeBackgroundRed` Erweiterungsmethode.   Dies bedeutet, dass Sie jetzt aufrufen können `MakeBackgroundRed` für jedes beliebige `UIView` -Unterklasse, sodass Sie die gleiche Funktionalität Sie in Objective-c erhalten

In einigen Fällen finden Sie **statische** Member in Kategorien wie im folgenden Beispiel:

```objc
@interface FooObject (MyFooObjectExtension)
+ (BOOL)boolMethod:(NSRange *)range;
@end
```

Dies führt zu einer **falsche** Kategorie C# Schnittstellendefinition:

```csharp
[Category]
[BaseType (typeof (FooObject))]
interface FooObject_Extensions {

    // Incorrect Interface definition
    [Static]
    [Export ("boolMethod:")]
    bool BoolMethod (NSRange range);
}
```

Dies ist falsch da verwenden die `BoolMethod` Erweiterung benötigen Sie eine Instanz der `FooObject` , aber Sie binden ein ObjC **statische** -Erweiterung, dies ist ein Nebeneffekt, dass aufgrund der Tatsache wie C# Erweiterungsmethoden implementiert werden .

Die einzige Möglichkeit, verwenden Sie die oben beschriebenen Definitionen ist mit dem folgenden hässliche Code:

```csharp
(null as FooObject).BoolMethod (range);
```

Es wird empfohlen, dieses Problem zu vermeiden, Inline der `BoolMethod` Definition in der `FooObject` Schnittstellendefinition selbst, dadurch können Sie diese Erweiterung aufgerufen, wie dies soll `FooObject.BoolMethod (range)`.

```csharp
[BaseType (typeof (NSObject))]
interface FooObject {

    [Static]
    [Export ("boolMethod:")]
    bool BoolMethod (NSRange range);
}
```

Es gibt eine Warnung (BI1117) auf, wenn es gefunden eine [ `[Static]` ](#StaticAttribute) Member innerhalb einer [ `[Category]` ](#CategoryAttribute) Definition. Wenn Sie wirklich, damit möchten [ `[Static]` ](#StaticAttribute) Member innerhalb Ihrer [ `[Category]` ](#CategoryAttribute) Definitionen können Sie die Warnung unterdrücken, mithilfe von `[Category (allowStaticMembers: true)]` oder über ergänzende entweder Member oder [ `[Category]` ](#CategoryAttribute) Schnittstellendefinition mit [ `[Internal]` ](#InternalAttribute).

<a name="StaticAttribute_Class" />

### <a name="staticattribute"></a>StaticAttribute

Wenn dieses Attribut auf eine Klasse angewendet wird, wird es nur eine statische Klasse, eine, die nicht von abgeleitet ist generiert `NSObject`, sodass die [ `[BaseType]` ](#BaseTypeAttribute) -Attribut wird ignoriert. Statische Klassen werden verwendet, um öffentliche C-Variablen zu hosten, die Sie verfügbar machen möchten.

Zum Beispiel:

```csharp
[Static]
interface CBAdvertisement {
    [Field ("CBAdvertisementDataServiceUUIDsKey")]
    NSString DataServiceUUIDsKey { get; }
```

Generiert eine C# Klasse mit der folgenden-API:

```csharp
public partial class CBAdvertisement  {
    public static NSString DataServiceUUIDsKey { get; }
}
```

## <a name="protocolmodel-definitions"></a>Protokoll/Model-Definitionen

Modelle werden in der Regel von protokollimplementierung verwendet.
Sie unterscheiden sich darin, dass die Laufzeit nur mit Objective-C-Methoden registriert wird, die tatsächlich überschrieben wurden.
Andernfalls wird die Methode nicht registriert werden.

Das bedeutet im Allgemeinen, wenn Sie eine Unterklasse einer Klasse, die mit gekennzeichnet wurden, wurden die `ModelAttribute`, Sie sollten die Basismethode nicht aufrufen.   Aufruf dieser Methode löst eine Ausnahme aus, der Sie werden aufgefordert, um das gesamte Verhalten in Ihrer Unterklasse für alle Methoden zu implementieren, die Sie außer Kraft setzen.

<a name="AbstractAttribute" />

### <a name="abstractattribute"></a>AbstractAttribute

Standardmäßig sind die Elemente, die Teil eines Protokolls nicht obligatorisch. Dies ermöglicht Benutzern die Erstellung eine Unterklasse von der `Model` Objekt, indem lediglich Ableiten von der Klasse in C# und überschreiben nur die Methoden, die sie interessieren. Manchmal wird in den Objective-C-Vertrag erforderlich ist, dass der Benutzer eine Implementierung für diese Methode gibt (die gekennzeichnet werden, mit der `@required` -Direktive in Objective-C). In diesen Fällen sollten Sie diese Methoden kennzeichnen die `[Abstract]` Attribut.

Die `[Abstract]` Attribut kann auf Methoden oder Eigenschaften angewendet werden, und führt dazu, dass den Generator so kennzeichnen Sie das generierte Element als abstrakt, und die Klasse aus, um eine abstrakte Klasse sein.

Folgendes stammt von Xamarin.iOS:

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
public interface UITableViewDataSource {
    [Export ("tableView:numberOfRowsInSection:")]
    [Abstract]
    nint RowsInSection (UITableView tableView, nint section);
}
```

<a name="DefaultValueAttribute" />

### <a name="defaultvalueattribute"></a>DefaultValueAttribute

Gibt an, den Standardwert zurück, die von einer Methode zurückgegeben werden, wenn der Benutzer nicht über eine Methode für diese bestimmte Methode in der Model-Objekts bietet

Syntax:

```csharp
public class DefaultValueAttribute : Attribute {
        public DefaultValueAttribute (object o);
        public object Default { get; set; }
}
```

Z. B. in der folgenden imaginären Delegatklasse für eine `Camera` Klasse, bieten wir eine `ShouldUploadToServer` die würde verfügbar gemacht werden als Eigenschaft auf die `Camera` Klasse. Wenn der Benutzer die `Camera` Klasse nicht explizit festlegt eine wäre der Wert auf einen Lambda-Ausdruck, der True oder False reagieren kann, der Standardwert zurück in diesem Fall "false" der Wert, der im angegebenen die `DefaultValue` Attribut:

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
interface CameraDelegate {
    [Export ("camera:shouldPromptForAction:"), DefaultValue (false)]
    bool ShouldUploadToServer (Camera camera, CameraAction action);
}
```

Wenn der Benutzer einen Handler in der Klasse imaginären festlegt, wird dieser Wert ignoriert werden:

```csharp
var camera = new Camera ();
camera.ShouldUploadToServer = (camera, action) => return SomeDecision ();
```

Siehe auch: [ `[NoDefaultValue]` ](#NoDefaultValueAttribute), [ `[DefaultValueFromArgument]` ](#DefaultValueFromArgumentAttribute).

<a name="DefaultValueFromArgumentAttribute" />

### <a name="defaultvaluefromargumentattribute"></a>DefaultValueFromArgumentAttribute

Syntax:

```csharp
public class DefaultValueFromArgumentAttribute : Attribute {
    public DefaultValueFromArgumentAttribute (string argument);
    public string Argument { get; }
}
```

Dieses Attribut, wenn für eine Methode bereitgestellt wird, die einen Wert, auf eine Modellklasse zurückgibt wird angewiesen, den Generator, geben Sie den Wert des angegebenen Parameters zurück, wenn der Benutzer seine eigene Methode oder einem Lambdaausdruck nicht bereitgestellt hat.

Beispiel:

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
public interface NSAnimationDelegate {
    [Export ("animation:valueForProgress:"), DelegateName ("NSAnimationProgress"), DefaultValueFromArgumentAttribute ("progress")]
    float ComputeAnimationCurve (NSAnimation animation, nfloat progress);
}
```

In der oben genannten Fall, wenn der Benutzer die `NSAnimation` Klasse ausgewählt haben, verwenden die C# /Eigenschaften, Ereignisse und wurde nicht festgelegt `NSAnimation.ComputeAnimationCurve` an eine Methode oder der Lambda, wäre der zurückgegebene Wert den Wert, der den Status-Parameter übergeben.

Siehe auch: [ `[NoDefaultValue]` ](#NoDefaultValueAttribute), [`[DefaultValue]`](#DefaultValueAttribute)

### <a name="ignoredindelegateattribute"></a>IgnoredInDelegateAttribute

Manchmal empfiehlt nicht verfügbar machen ein Ereignis aus, oder delegieren Eigenschaft aus einer Modellklasse in die Hostklasse ein, sodass das Hinzufügen dieses Attributs wird den Generator, um die Generierung von jeder Methode, die mit ihm zu vermeiden anweisen.

```csharp
[BaseType (typeof (UINavigationControllerDelegate))]
[Model][Protocol]
public interface UIImagePickerControllerDelegate {
    [Export ("imagePickerController:didFinishPickingImage:editingInfo:"), EventArgs ("UIImagePickerImagePicked")]
    void FinishedPickingImage (UIImagePickerController picker, UIImage image, NSDictionary editingInfo);

    [Export ("imagePickerController:didFinishPickingImage:"), IgnoredInDelegate)] // No event generated for this method
    void FinishedPickingImage (UIImagePickerController picker, UIImage image);
}
```

### <a name="delegatenameattribute"></a>DelegateNameAttribute

Dieses Attribut wird in der SMO-Methoden verwendet, die Werte entsprechend den Namen der Signatur des Delegaten mit zurückgeben.

Beispiel:

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
public interface NSAnimationDelegate {
    [Export ("animation:valueForProgress:"), DelegateName ("NSAnimationProgress"), DefaultValueFromArgumentAttribute ("progress")]
    float ComputeAnimationCurve (NSAnimation animation, float progress);
}
```

Mit der Definition des oben genannten erzeugt der Generator die folgende öffentliche Deklaration:

```csharp
public delegate float NSAnimationProgress (MonoMac.AppKit.NSAnimation animation, float progress);
```

### <a name="delegateapinameattribute"></a>DelegateApiNameAttribute

Dieses Attribut wird verwendet, um den Generator so ändern Sie den Namen der Eigenschaft in der Hostklasse generiert zu ermöglichen. Manchmal ist es hilfreich, wenn der Name der Klassenmethode FooDelegate, die Delegate-Klasse sinnvoll, sondern in der Hostklasse als Eigenschaft ungerade sieht.

Dies ist auch sehr nützlich (und erforderlichen) Wenn verfügen Sie über zwei oder weitere Überladung-Methoden, die sinnvoll ist, mit dem Namen in der Klasse FooDelegate unverändert bleiben jedoch Sie sie in der Hostklasse mit einem besseren angegebenen Namen verfügbar machen möchten.

Beispiel:

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
public interface NSAnimationDelegate {
    [Export ("animation:valueForProgress:"), DelegateApiName ("ComputeAnimationCurve"), DelegateName ("Func<NSAnimation, float, float>"), DefaultValueFromArgument ("progress")]
    float GetValueForProgress (NSAnimation animation, float progress);
}
```

Mit der Definition des oben genannten erzeugt der Generator die folgende öffentliche Deklaration in die Hostklasse:

```csharp
public Func<NSAnimation, float, float> ComputeAnimationCurve { get; set; }
```

<a name="EventArgsAttribute" />

### <a name="eventargsattribute"></a>EventArgsAttribute

Für Ereignisse, die mehr als einen Parameter (in Objective-C-die Konvention ist, dass der erste Parameter in einer "Delegate"-Klasse die Instanz des Sender-Objekts) Geben Sie den Namen, die für die generierte EventArgs-Klasse sein soll. Dies erfolgt mit der `[EventArgs]` Attribut in der Deklaration der Methode in Ihrer `Model` Klasse.

Zum Beispiel:

```csharp
[BaseType (typeof (UINavigationControllerDelegate))]
[Model][Protocol]
public interface UIImagePickerControllerDelegate {
    [Export ("imagePickerController:didFinishPickingImage:editingInfo:"), EventArgs ("UIImagePickerImagePicked")]
    void FinishedPickingImage (UIImagePickerController picker, UIImage image, NSDictionary editingInfo);
}
```

Die obige Deklaration generiert eine `UIImagePickerImagePickedEventArgs` -Klasse, die von EventArgs abgeleitet wird, und packs beide Parameter, die `UIImage` und `NSDictionary`. Der Generator erstellt dies:

```csharp
public partial class UIImagePickerImagePickedEventArgs : EventArgs {
    public UIImagePickerImagePickedEventArgs (UIImage image, NSDictionary editingInfo);
    public UIImage Image { get; set; }
    public NSDictionary EditingInfo { get; set; }
}
```

Es stellt dann die folgenden in das `UIImagePickerController` Klasse:

```csharp
public event EventHandler<UIImagePickerImagePickedEventArgs> FinishedPickingImage { add; remove; }
```


### <a name="eventnameattribute"></a>EventNameAttribute

Dieses Attribut wird verwendet, um den Generator so ändern Sie den Namen eines Ereignisses oder der Eigenschaft, die in der Klasse generiert zu ermöglichen. Manchmal ist es hilfreich, wenn der Name der Methode der Model-Klasse Sinn, dass die Model-Klasse macht, aber es wie ein Ereignis oder eine Eigenschaft in der ursprünglichen Klasse ungerade sieht.

Z. B. die `UIWebView` verwendet das folgende Bit aus dem `UIWebViewDelegate`:

```csharp
[Export ("webViewDidFinishLoad:"), EventArgs ("UIWebView"), EventName ("LoadFinished")]
void LoadingFinished (UIWebView webView);
```

Die oben genannten macht `LoadingFinished` wie die Methode in der `UIWebViewDelegate`, aber `LoadFinished` wie das Ereignis zum Einbinden in eine `UIWebView`:

```csharp
var webView = new UIWebView (...);
webView.LoadFinished += delegate { Console.WriteLine ("done!"); }
```

<a name="ModelAttribute" />

### <a name="modelattribute"></a>ModelAttribute

Beim Anwenden der `[Model]` Attribut, um die Definition eines Typs in Ihrem Vertrag-API, die Common Language Runtime generiert speziellen Code, der nur Aufrufe für Methoden in der Klasse sichtbar wird, wenn der Benutzer eine Methode in der Klasse überschrieben wurde. Dieses Attribut wird in der Regel auf alle APIs angewendet, die eine Objective-C-Delegat-Klasse zu umschließen.

<a name="NoDefaultValueAttribute" />

### <a name="nodefaultvalueattribute"></a>NoDefaultValueAttribute

Gibt an, dass die Methode für das Modell keinen Standardrückgabewert bietet.

Dies funktioniert mit Objective-C-Laufzeit, durch die Antwort `false` an die Objective-C-Laufzeit-Anforderung zu bestimmen, ob es sich bei der angegebene Selektor in dieser Klasse implementiert wird.

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
interface CameraDelegate {
    [Export ("shouldDisplayPopup"), NoDefaultValue]
    bool ShouldUploadToServer ();
}
```

Siehe auch: [ `[DefaultValue]` ](#DefaultValueAttribute), [`[DefaultValueFromArgument]`](#DefaultValueFromArgumentAttribute)  

<a name="ProtocolAttribute" />

## <a name="protocols"></a>Protokolle

Das Konzept der Objective-C-Protokoll ist eigentlich nicht in C#. Protokolle sind C# Schnittstellen, aber sie unterscheiden sich, die nicht alle Methoden und Eigenschaften, die in ein Protokoll deklariert müssen implementiert werden, von der Klasse, der übernommen. Stattdessen sind einige der Methoden und Eigenschaften optional.

Einige Protokolle werden in der Regel als ViewModel-Klassen verwendet, die gebunden werden mithilfe der [ `[Model]` ](#ModelAttribute) Attribut.

```csharp
[BaseType (typeof (NSObject))]
[Model, Protocol]
interface MyProtocol {
    // Use [Abstract] when the method is defined in the @required section
    // of the protocol definition in Objective-C
    [Abstract]
    [Export ("say:")]
    void Say (string msg);

    [Export ("listen")]
    void Listen ();
}
```

Ab Xamarin.iOS 7.0 einen neuen und verbesserten protokollbindung-Funktionalität wurde integriert.  Definition, enthält die `[Protocol]` Attribut generiert tatsächlich drei unterstützende Klassen, die die Möglichkeit zu steigern, um Sie Protokolle verwenden:

```csharp
// Full method implementation, contains all methods
class MyProtocol : IMyProtocol {
    public void Say (string msg);
    public void Listen (string msg);
}

// Interface that contains only the required methods
interface IMyProtocol: INativeObject, IDisposable {
    [Export ("say:")]
    void Say (string msg);
}

// Extension methods
static class IMyProtocol_Extensions {
    public static void Optional (this IMyProtocol this, string msg);
    }
}
```

Die **-klassenimplementierung** bietet eine vollständige abstrakte Klasse, dass Sie einzelne Methoden von außer Kraft setzen und vollständige typsicherheit. Aber aufgrund von C# unterstützen keine mehrfache Vererbung, es gibt Szenarien, in denen Sie möglicherweise benötigen eine andere Basisklasse, aber immer noch eine Schnittstelle implementieren möchten.

Hier werden die generierten **Schnittstellendefinition** ins Spiel.  Es ist eine Schnittstelle, die über die erforderlichen Methoden aus dem Protokoll verfügt.  Dadurch können Entwickler, die Ihr Protokoll, um lediglich die Schnittstelle implementieren implementieren möchten.  Die Laufzeit wird den Typ automatisch registriert, als die Einführung des Protokolls.

Beachten Sie, dass die Schnittstelle nur sind die erforderlichen Methoden aufgeführt, und macht die optionalen Methoden verfügbar.   Dies bedeutet, dass Klassen, die das Protokoll zu übernehmen vollständiger typüberprüfung für die erforderlichen Methoden erhalten, müssen aber zurückgreifen, um schwache Typisierung (manuell mithilfe von Attributen für Export und die Signatur), für die optionales Protokoll-Methoden.

Damit wird es praktisch sein, eine API nutzen, die Protokolle, erzeugt das Tool für die Bindung auch eine Extensions-Methode-Klasse, die alle die optionalen Methoden verfügbar macht.   Dies bedeutet, dass, solange Sie eine API verwenden, um Protokolle zu behandeln, als dass alle Methoden werden kann.

Wenn Sie die Definitionen des Protokolls in Ihrer API verwenden möchten, müssen Sie Skelette leere Schnittstellen in Ihrer API-Definition zu schreiben.   Wenn Sie die MyProtocol in eine API verwenden möchten, müssen Sie würde dazu:

```csharp
[BaseType (typeof (NSObject))]
[Model, Protocol]
interface MyProtocol {
    // Use [Abstract] when the method is defined in the @required section
    // of the protocol definition in Objective-C
    [Abstract]
    [Export ("say:")]
    void Say (string msg);

    [Export ("listen")]
    void Listen ();
}

interface IMyProtocol {}

[BaseType (typeof(NSObject))]
interface MyTool {
    [Export ("getProtocol")]
    IMyProtocol GetProtocol ();
}
```

Die oben genannten ist erforderlich, da an der Zeit zum Binden der `IMyProtocol` ist nicht vorhanden, also Warum müssen Sie eine leere Schnittstelle angeben.

### <a name="adopting-protocol-generated-interfaces"></a>Einführung von Protokoll generierten Schnittstellen

Jedes Mal, wenn Sie eine der für die Protokolle, wie folgt generiert Schnittstellen implementieren:

```csharp
class MyDelegate : NSObject, IUITableViewDelegate {
    nint IUITableViewDelegate.GetRowHeight (nint row) {
        return 1;
    }
}
```

Die Implementierung für die Schnittstellenmethoden ruft automatisch mit dem richtigen Namen, exportiert, damit er dies entspricht:

```csharp
class MyDelegate : NSObject, IUITableViewDelegate {
    [Export ("getRowHeight:")]
    nint IUITableViewDelegate.GetRowHeight (nint row) {
        return 1;
    }
}
```

Es spielt keine Rolle, wenn die Schnittstelle implizit oder explizit implementiert wird.

### <a name="protocol-inlining"></a>Inlining Protokoll

Obwohl Sie vorhandene Objective-C-Typen zu, die binden als ein Protokoll Einführung deklariert wurden, werden Inline des Protokolls direkt möchten. Dazu deklarieren Sie lediglich Ihr Protokoll als eine Schnittstelle ohne [ `[BaseType]` ](#BaseTypeAttribute) Attribut, und das Protokoll in der Liste der für Ihre Schnittstelle die Basisschnittstellen auflisten.

Beispiel:

```csharp
interface SpeakProtocol {
    [Export ("say:")]
    void Say (string msg);
}

[BaseType (typeof (NSObject))]
interface Robot : SpeakProtocol {
    [Export ("awake")]
    bool Awake { get; set; }
}
```


## <a name="member-definitions"></a>Elementdefinitionen

Die Attribute in diesem Abschnitt an einzelne Mitglieder eines Typs angewendet werden: Eigenschaften und Methodendeklarationen.


### <a name="alignattribute"></a>AlignAttribute

Dient zum Angeben der Ausrichtungswert für die Rückgabetypen der Eigenschaft. Bestimmte Eigenschaften werden Verweise auf die Adressen, die an bestimmte Grenzen ausgerichtet sein müssen (in Xamarin.iOS, die in diesem z. B. mit einigen Fall `GLKBaseEffect` Eigenschaften, die 16-Byte-müssen ausgerichtet). Sie können mit dieser Eigenschaft können um den Getter zu ergänzen, und verwenden Sie den Ausrichtungswert. Dies wird normalerweise verwendet, mit der `OpenTK.Vector4` und `OpenTK.Matrix4` Typen bei der Integration mit Objective-C-APIs.

Beispiel:

```csharp
public interface GLKBaseEffect {
    [Export ("constantColor")]
    Vector4 ConstantColor { [Align (16)] get; set;  }
}
```


### <a name="appearanceattribute"></a>AppearanceAttribute

Die `[Appearance]` -Attribut ist beschränkt auf iOS 5, bei dem der Erscheinungsbild-Manager eingeführt wurde.

Die `[Appearance]` Attribut kann angewendet werden, um eine beliebige Methode oder Eigenschaft, die Teilnahme an der `UIAppearance` Framework. Wenn dieses Attribut an eine Methode oder Eigenschaft in einer Klasse angewendet wird, weist er den Bindung-Generator zum Erstellen einer stark typisierten Darstellung-Klasse, die verwendet wird, so formatieren Sie alle Instanzen dieser Klasse oder Instanzen, die bestimmte Kriterien erfüllen.

Beispiel:

```csharp
public interface UIToolbar {
    [Since (5,0)]
    [Export ("setBackgroundImage:forToolbarPosition:barMetrics:")]
    [Appearance]
    void SetBackgroundImage (UIImage backgroundImage, UIToolbarPosition position, UIBarMetrics barMetrics);

    [Since (5,0)]
    [Export ("backgroundImageForToolbarPosition:barMetrics:")]
    [Appearance]
    UIImage GetBackgroundImage (UIToolbarPosition position, UIBarMetrics barMetrics);
}
```

Die oben genannten würde den folgenden Code in UIToolbar generieren:

```csharp
public partial class UIToolbar {
    public partial class UIToolbarAppearance : UIView.UIViewAppearance {
        public virtual void SetBackgroundImage (UIImage backgroundImage, UIToolbarPosition position, UIBarMetrics barMetrics);
        public virtual UIImage GetBackgroundImage (UIToolbarPosition position, UIBarMetrics barMetrics)
    }
    public static new UIToolbarAppearance Appearance { get; }
    public static new UIToolbarAppearance AppearanceWhenContainedIn (params Type [] containers);
}
```

### <a name="autoreleaseattribute-xamarinios-54"></a>AutoReleaseAttribute (Xamarin.iOS 5.4)

Verwenden der `[AutoReleaseAttribute]` auf Methoden und Eigenschaften für den Wrapper für den Methodenaufruf an die Methode in einer `NSAutoReleasePool`.

In Objective-C stehen einige Methoden, die Werte zurückgeben, die auf den Standardwert hinzugefügt werden `NSAutoReleasePool`. Standardmäßig würde diese auf Ihren Thread wechseln `NSAutoReleasePool`, aber da Xamarin.iOS, solange das verwaltete Objekt befindet sich auch einen Verweis auf die Objekte verfolgt, sollten nicht um einen zusätzlichen Verweis zu halten, der `NSAutoReleasePool` die wird nur bis der Thread ausgeglichen abrufen Gibt die Steuerung an den nächsten Thread aus, oder Sie wechseln Sie zurück zu der main-Schleife.

Dieses Attribut gilt z. B. hohe Eigenschaften (z. B. `UIImage.FromFile`), die Objekte, die auf den Standardwert hinzugefügt wurden zurückgibt `NSAutoReleasePool`. Ohne dieses Attribut würde die Bilder beibehalten werden, solange der Thread Steuerelement nicht in der Hauptschleife beendet wurde. Uf der Thread wurde eine Art von Hintergrund-Downloadprogramm, das immer aktiv ist, und warten, Arbeit, die Bilder werden nie freigegeben werden.

### <a name="forcedtypeattribute"></a>ForcedTypeAttribute

Die `[ForcedTypeAttribute]` wird verwendet, um die Erstellung eines verwalteten Typs zu erzwingen, auch wenn das zurückgegebene, nicht verwaltete Objekt nicht in der Bindungsdefinition beschriebenen Typs übereinstimmt.

Dies ist nützlich, wenn in einem Header beschriebenen Typs nicht mit den zurückgegebenen Typ der nativen Methode übereinstimmt, nehmen Sie z. B. die folgende Objective-C-Definition von `NSURLSession`:

`- (NSURLSessionDownloadTask *)downloadTaskWithRequest:(NSURLRequest *)request`

Es wird deutlich, dass er zurückgegeben wird ein `NSURLSessionDownloadTask` Instanz, jedoch noch es **gibt** eine `NSURLSessionTask`, die ist eine übergeordnete Klasse und daher nicht in `NSURLSessionDownloadTask`. Da wir in einem Kontext typsicher sind ein `InvalidCastException` erfolgt.

Die Header-Beschreibung entsprechen und vermeiden der `InvalidCastException`, `[ForcedTypeAttribute]` wird verwendet.

```csharp
[BaseType (typeof (NSObject), Name="NSURLSession")]
interface NSUrlSession {

    [Export ("downloadTaskWithRequest:")]
    [return: ForcedType]
    NSUrlSessionDownloadTask CreateDownloadTask (NSUrlRequest request);
}
```

Die `[ForcedTypeAttribute]` akzeptiert auch einen booleschen Wert, der mit dem Namen `Owns` , `false` standardmäßig `[ForcedType (owns: true)]`. Der Parameter wird verwendet, um die Folgen der Besitzer ist der [Besitz Richtlinie](https://developer.apple.com/library/content/documentation/CoreFoundation/Conceptual/CFMemoryMgmt/Concepts/Ownership.html) für **Core Foundation** Objekte.

Die `[ForcedTypeAttribute]` ist nur gültig für Parameter, Eigenschaften und Rückgabewert.

<a name="BindAsAttribute" />

### <a name="bindasattribute"></a>BindAsAttribute

Die `[BindAsAttribute]` Bindung ermöglicht `NSNumber`, `NSValue` und `NSString`(Enumerationen) in mehrere genaue C# Typen. Das Attribut kann verwendet werden, erstellen Sie eine bessere und genauere .NET API gegenüber einer systemeigenen API.

Sie können die Methoden (Rückgabewert), Parameter und Eigenschaften mit ergänzen `BindAs`. Die einzige Einschränkung gehört, die Ihre **darf nicht** innerhalb einer `[Protocol]` oder [ `[Model]` ](#ModelAttribute) Schnittstelle.

Zum Beispiel:

```csharp
[return: BindAs (typeof (bool?))]
[Export ("shouldDrawAt:")]
NSNumber ShouldDraw ([BindAs (typeof (CGRect))] NSValue rect);
```

Ausgabe wäre:

```csharp
[Export ("shouldDrawAt:")]
bool? ShouldDraw (CGRect rect) { ... }
```

Intern werden wir die `bool?`  <->  `NSNumber` und `CGRect`  <->  `NSValue` Konvertierungen.

Die aktuelle unterstützte Kapselung Typen sind:

* `NSValue`
* `NSNumber`
* `NSString`

#### <a name="nsvalue"></a>NSValue

Die folgenden C# -Datentypen werden unterstützt, um die gekapselt werden, aus bzw. in `NSValue`:

* CGAffineTransform
* NSRange
* CGVector
* SCNMatrix4
* CLLocationCoordinate2D
* SCNVector3
* SCNVector4
* CGPoint / PointF
* CGRect / RectangleF
* CGSize / SizeF
* UIEdgeInsets
* UIOffset
* MKCoordinateSpan
* CMTimeRange
* CMTime
* CMTimeMapping
* CATransform3D

#### <a name="nsnumber"></a>NSNumber

Die folgenden C# -Datentypen werden unterstützt, um die gekapselt werden, aus bzw. in `NSNumber`:

* bool
* byte
* double
* frei verschieben
* short
* int
* long
* sbyte
* ushort
* uint
* ulong
* nfloat
* nint
* nuint
* Enumerationen

#### <a name="nsstring"></a>NSString

[`[BindAs]`](#BindAsAttribute) funktioniert in Conjuntion mit [Enumerationen gesichert durch ein NSString](#enum-attributes) damit Sie besser .NET API, z. B. erstellen können:

```csharp
[BindAs (typeof (CAScroll))]
[Export ("supportedScrollMode")]
NSString SupportedScrollMode { get; set; }
```

Ausgabe wäre:

```csharp
[Export ("supportedScrollMode")]
CAScroll SupportedScrollMode { get; set; }
```

Wir kümmern uns anschließend wird der `enum`  <->  `NSString` Konvertierung nur dann, wenn die angegebene Enumeration geben Sie [ `[BindAs]` ](#BindAsAttribute) ist [gesichert durch ein NSString](#enum-attributes).

#### <a name="arrays"></a>Arrays

[`[BindAs]`](#BindAsAttribute) unterstützt auch Arrays eines beliebigen der unterstützten Typen sein, dass die folgende API-Definition als Beispiel:

```csharp
[return: BindAs (typeof (CAScroll []))]
[Export ("getScrollModesAt:")]
NSString [] GetScrollModes ([BindAs (typeof (CGRect []))] NSValue [] rects);
```

Ausgabe wäre:

```csharp
[Export ("getScrollModesAt:")]
CAScroll? [] GetScrollModes (CGRect [] rects) { ... }
```

Die `rects` Parameter vohersageabfragetasks in einer `NSArray` , enthält ein `NSValue` für jede `CGRect` und im Gegenzug erhalten Sie ein Array von `CAScroll?` die wurde unter Verwendung der Werte des zurückgegebenen `NSArray` mit `NSStrings`.

<a name="BindAttribute" />

### <a name="bindattribute"></a>BindAttribute

Die `[Bind]` Attribut verfügt über zwei Verwendungen ein, wenn auf eine Methode oder Eigenschaft-Deklaration und eine andere bei Anwendung auf die einzelnen Getter oder Setter in einer Eigenschaft angewendet.

Wenn für eine Methode oder Eigenschaft, die Auswirkungen von verwendet die `[Bind]` -Attribut ist, generieren eine Methode, die den angegebenen Selektor aufruft. Aber die resultierende generierte Methode ist nicht mit versehen der [ `[Export]` ](#ExportAttribute) -Attribut, das bedeutet, dass es keine Methode überschreiben teilnehmen kann. Dies wird normalerweise verwendet, in Kombination mit der `[Target]` Attribut für die Implementierung von Objective-C-Erweiterungsmethoden.

Zum Beispiel:

```csharp
public interface UIView {
    [Bind ("drawAtPoint:withFont:")]
    SizeF DrawString ([Target] string str, CGPoint point, UIFont font);
}
```

Bei der Verwendung in einen Getter oder Setter der `[Bind]` Attribut wird verwendet, um die Standardeinstellungen, abgeleitet vom Code-Generator beim Generieren der Getter und Setter Objective-C-Auswahlnamen für eine Eigenschaft ändern. Standard, wenn Sie eine Eigenschaft mit dem Namen kennzeichnen `fooBar`, der Generator generiert eine `fooBar` exportieren, die für den Getter und `setFooBar:` für den Setter. In einigen Fällen werden die Objective-C nicht diese Konvention befolgt, die in der Regel ändern sie den Namen "Getter" sein `isFooBar`.
Sie würden dieses Attribut verwenden, um den Generator dieses darüber zu informieren.

Zum Beispiel:

```csharp
// Default behavior
[Export ("active")]
bool Active { get; set; }

// Custom naming with the Bind attribute
[Export ("visible")]
bool Visible { [Bind ("isVisible")] get; set; }
```

<a name="AsyncAttribute" />

### <a name="asyncattribute"></a>AsyncAttribute

Nur auf dem 6.3 für Xamarin.iOS und höher verfügbar.

Dieses Attribut kann auf Methoden angewendet werden, die einen Abschlusshandler als letzten Argument zu akzeptieren.

Sie können die `[Async]` Attribut auf Methoden, deren letzte Argument ein Rückruf ist.  Wenn Sie diese an eine Methode angewendet wird, generiert der Bindung-Generator eine Version dieser Methode mit dem Suffix `Async`.  Wenn der Rückruf keine Parameter akzeptiert, wird der Rückgabewert eine `Task`, wenn der Rückruf einen Parameter akzeptiert, wird das Ergebnis einer `Task<T>`.

```csharp
[Export ("upload:complete:")]
[Async]
void LoadFile (string file, NSAction complete)
```

Im folgenden wird die asynchrone Methode generiert:

```csharp
Task LoadFileAsync (string file);
```

Wenn der Rückruf mehrere Parameter akzeptiert, legen Sie die `ResultType` oder `ResultTypeName` den gewünschten Namen des generierten Typs angeben, die alle Eigenschaften enthalten wird.

```csharp
delegate void OnComplete (string [] files, nint byteCount);

[Export ("upload:complete:")]
[Async (ResultTypeName="FileLoading")]
void LoadFiles (string file, OnComplete complete)
```

Im folgenden werden dieser Async-Methode generiert, in denen `FileLoading` enthält Eigenschaften, die Zugriff auf `files` und `byteCount`:

```csharp
Task<FileLoading> LoadFile (string file);
```

Wenn der letzte Parameter des Rückrufs ist ein `NSError`, klicken Sie dann die generierte `Async` Methode überprüft, ob der Wert nicht null ist, und wenn dies der Fall ist, wird die generierte Async-Methode die aufgabenausnahme festgelegt.

```csharp
[Export ("upload:onComplete:")]
[Async]
void Upload (string file, Action<string,NSError> onComplete);
```

Die oben genannten wird die folgende asynchrone Methode generiert:

```csharp
Task<string> UploadAsync (string file);
```

Und bei einem Fehler die resultierende Aufgabe wird die Ausnahme, die zum Festlegen einer `NSErrorException` , umschließt die resultierende `NSError`.

#### <a name="asyncattributeresulttype"></a>AsyncAttribute.ResultType

Verwenden Sie diese Eigenschaft zum Angeben des Werts für die Rückgabe `Task` Objekt.   Dieser Parameter weist einen vorhandenen Typ, daher muss Sie in einer der Core-API-Definitionen definiert sein.

#### <a name="asyncattributeresulttypename"></a>AsyncAttribute.ResultTypeName

Verwenden Sie diese Eigenschaft zum Angeben des Werts für die Rückgabe `Task` Objekt.   Dieser Parameter weist den Namen der gewünschte Typ, der Generator erstellt eine Reihe von Eigenschaften für jeden Parameter, der der Rückruf verwendet.

#### <a name="asyncattributemethodname"></a>AsyncAttribute.MethodName

Verwenden Sie diese Eigenschaft zum Anpassen des Namens der generierten Async-Methoden.   Der Standardwert ist, verwenden Sie den Namen der Methode, und fügen Sie den Text "Async", Hiermit können Sie diese Standardeinstellung ändern.

### <a name="disablezerocopyattribute"></a>DisableZeroCopyAttribute

Dieses Attribut-Parameter oder Eigenschaften angewendet wird, und weist den Codegenerator nicht das Kopieren von 0 (null) Marshallen von Zeichenfolgen für diesen Parameter, und erstellen Sie stattdessen eine neue Instanz der NSString aus der C# Zeichenfolge.
Dieses Attribut ist nur für Zeichenfolgen erforderlich, wenn Sie anweisen, die vom Generator zu verwendenden Marshallen von 0 (null)-Copy-Zeichenfolgen mithilfe der `--zero-copy` Befehlszeilenoption oder Festlegen des Attributs auf Assemblyebene `ZeroCopyStringsAttribute`.

Dies ist in Fällen, in dem die Eigenschaft in Objective-C nach werden deklariert ist, erforderlich, eine `retain` oder `assign` Eigenschaft anstatt einer `copy` Eigenschaft. Diese wird in der Regel in Drittanbieter-Bibliotheken, die fälschlicherweise "von Entwicklern dahingehend optimiert wurden," auftreten. Im allgemeinen `retain` oder `assign` `NSString` sind falsch, da `NSMutableString` oder Benutzer abgeleitete Klassen von `NSString` ändert sich möglicherweise den Inhalt der Zeichenfolgen ohne Kenntnis des Bibliothekscodes, unterbrechen leicht die die Anwendung. Dies geschieht in der Regel aufgrund von vorzeitige Optimierung.

Das folgende Beispiel zeigt zwei solche Eigenschaften in Objective-c:

```csharp
@property(nonatomic,retain) NSString *name;
@property(nonatomic,assign) NSString *name2;
```


### <a name="disposeattribute"></a>DisposeAttribute

Beim Anwenden der `[DisposeAttribute]` auf eine Klasse, die Sie Bereitstellen eines Codeausschnitts, die hinzugefügt werden, wird, die `Dispose()` methodenimplementierung der-Klasse.

Da die `Dispose` Methode wird automatisch generiert, indem die `bmac-native` und `btouch-native` Tools, die Sie verwenden möchten die `[Dispose]` Attribut zum Einfügen von Code in der generierten `Dispose` methodenimplementierung.

Zum Beispiel:

```csharp
[BaseType (typeof (NSObject))]
[Dispose ("if (OpenConnections > 0) CloseAllConnections ();")]
interface DatabaseConnection {
}
```

<a name="ExportAttribute" />

### <a name="exportattribute"></a>ExportAttribute

Die `[Export]` Attribut wird zum Kennzeichnen einer Methode oder Eigenschaft an die Objective-C-Laufzeit verfügbar gemacht werden. Dieses Attribut ist das Tool für die Bindung und die tatsächliche Xamarin.iOS- und Xamarin.Mac-Laufzeiten gemeinsam verwendet. Für Methoden, der Parameter übergeben wird wörtlich an dem generierten Code für die Eigenschaften für einen Getter und Setter-Exporte basierend auf der Basis-Deklaration generiert werden (finden Sie im Abschnitt der [ `[BindAttribute]` ](#BindAttribute) Informationen zum Ändern der Verhalten des Tools Bindung).

Syntax:

```csharp
public enum ArgumentSemantic {
    None, Assign, Copy, Retain.
}

[AttributeUsage (AttributeTargets.Method | AttributeTargets.Constructor | AttributeTargets.Property)]
public class ExportAttribute : Attribute {
    public ExportAttribute();
    public ExportAttribute (string selector);
    public ExportAttribute (string selector, ArgumentSemantic semantic);
    public string Selector { get; set; }
    public ArgumentSemantic ArgumentSemantic { get; set; }
}
```

Die [Selektor](https://developer.apple.com/library/content/documentation/General/Conceptual/DevPedia-CocoaCore/Selector.html) stellt den Namen der zugrunde liegenden Objective-C-Methode oder Eigenschaft, die gebunden wird.

#### <a name="exportattributeargumentsemantic"></a>ExportAttribute.ArgumentSemantic

<a name="FieldAttribute" />

### <a name="fieldattribute"></a>FieldAttribute

Dieses Attribut wird verwendet, um eine globale C-Variable als Feld verfügbar zu machen, die bei Bedarf geladen und verfügbar gemacht werden, um C# Code. Dies ist normalerweise erforderlich, um die Werte der Konstanten in C oder Objective-C definiert sind, entweder Token in einige APIs verwendet werden konnte und, deren Werte nicht transparent und müssen als verwendet werden – durch Benutzercode ist.

Syntax:

```csharp
public class FieldAttribute : Attribute {
    public FieldAttribute (string symbolName);
    public FieldAttribute (string symbolName, string libraryName);
    public string SymbolName { get; set; }
    public string LibraryName { get; set; }
}
```

Die `symbolName` handelt es sich zur Verknüpfung mit der C-Symbol. Standardmäßig wird diese aus einer Bibliothek, deren Name aus dem Namespace abgeleitet wird, geladen, in denen der Typ definiert ist. Wenn dies nicht der Bibliothek ist, in dem das Symbol gesucht wird, sollten Sie übergeben die `libraryName` Parameter. Wenn Sie eine statische Bibliothek eine Verknüpfung herstellen, verwenden Sie `__Internal` als die `libraryName` Parameter.

Die generierten Eigenschaften sind immer statisch.

Eigenschaften, die mit dem Feld-Attribut gekennzeichnet sind, können der folgenden Typen sein:

* `NSString`
* `NSArray`
* `nint` / `int` / `long`
* `nuint` / `uint` / `ulong`
* `nfloat` / `float`
* `double`
* `CGSize`
* `System.IntPtr`
* Enumerationen

Setter können nicht für [Enumerationen gesichert durch NSString Konstanten](#enum-attributes), aber sie können manuell gebunden werden bei Bedarf.

Beispiel:

```csharp
[Static]
interface CameraEffects {
     [Field ("kCameraEffectsZoomFactorKey", "CameraLibrary")]
     NSString ZoomFactorKey { get; }
}
```

<a name="InternalAttribute" />

### <a name="internalattribute"></a>InternalAttribute

Die `[Internal]` Attribut kann auf Methoden oder Eigenschaften angewendet werden und hat den Effekt des generierten Codes mit kennzeichnen die `internal` C# Schlüsselwort, die den Code in die generierte Assembly nur für Code verfügbar machen. Dies dient normalerweise zum Ausblenden von APIs, die auch auf niedriger Ebene sind, oder geben eine suboptimale öffentliche API, die zur Verbesserung der bei der oder für APIs, die vom Generator nicht unterstützt werden sollen, und erfordert einige manuelle codieren.

Beim Entwerfen der Bindungs, Sie würden in der Regel auszublenden, die Methode oder Eigenschaft, die mit diesem Attribut und geben Sie einen anderen Namen aus, für die Methode oder Eigenschaft, und klicken Sie dann auf Ihre C# ergänzende Unterstützungsdatei hinzufügen einen stark typisierten Wrapper, die verfügbar macht die der zugrunde liegende Funktionalität.

Zum Beispiel:

```csharp
[Internal]
[Export ("setValue:forKey:")]
void _SetValueForKey (NSObject value, NSObject key);

[Internal]
[Export ("getValueForKey:")]
NSObject _GetValueForKey (NSObject key);
```

Anschließend können Sie in der unterstützenden-Datei Code wie aufweisen:

```csharp
public NSObject this [NSObject idx] {
    get {
        return _GetValueForKey (idx);
    }
    set {
        _SetValueForKey (value, idx);
    }
}
```

<a name="IsThreadStaticAttribute" />

### <a name="isthreadstaticattribute"></a>IsThreadStaticAttribute

Dieses Attribut kennzeichnet das dahinter liegende Feld für eine Eigenschaft, die mit Anmerkungen versehen werden, mit dem .NET `[ThreadStatic]` Attribut. Dies ist nützlich, wenn das Feld eine statische Thread-Variable ist.

### <a name="marshalnativeexceptions-xamarinios-606"></a>MarshalNativeExceptions (Xamarin.iOS 6.0.6 für)

Dieses Attribut wird eine Methode unterstützt native (Objective-C) Ausnahmen zu machen.
Statt `objc_msgSend` direkt, geht der Aufruf über eine benutzerdefinierte Trampoline ObjectiveC Ausnahmen abfängt und marshallt diese in verwalteten Ausnahmen.

Derzeit nur wenige `objc_msgSend` Signaturen werden unterstützt (Sie werden feststellen, ob eine Signatur nicht unterstützt wird, wenn native Verknüpfung von einer app, die die Bindung verwendet, die mit einer fehlenden Monotouch_ fehlschlägt *_objc_msgSend* Symbol), jedoch größer sein kann auf Anforderung hinzugefügt.


### <a name="newattribute"></a>NewAttribute

Dieses Attribut gilt für Methoden und Eigenschaften, damit den Generator generieren die `new` Schlüsselwort vor der Deklaration.

Es wird verwendet, um Compiler-Warnungen zu vermeiden, wenn die gleiche Methode oder Eigenschaftenname in einer Unterklasse eingeführt wird, die bereits in einer Basisklasse vorhanden waren.

<a name="NotificationAttribute" />

### <a name="notificationattribute"></a>NotificationAttribute

Sie können dieses Attribut auf Felder, damit der Generator erzeugen eine stark typisierte Hilfsprogramm Notifications-Klasse anwenden.

Dieses Attribut kann verwendet werden, ohne Argumente für Benachrichtigungen, die keine Nutzlast enthalten, oder Sie können angeben, ein `System.Type` , die auf einer anderen Schnittstelle in der API-Definition in der Regel verweist, mit dem Namen "EventArgs" enden. Der Generator wandeln der Schnittstelle in einer Klasse, Unterklassen `EventArgs` und enthält alle Eigenschaften aufgeführt. Die [ `[Export]` ](#ExportAttribute) Attribut sollte verwendet werden, der `EventArgs` Klasse, um die Liste der Namen des Schlüssels verwendet, um die Objective-C-Wörterbuch, das Abrufen des Werts suchen.

Zum Beispiel:

```csharp
interface MyClass {
    [Notification]
    [Field ("MyClassDidStartNotification")]
    NSString DidStartNotification { get; }
}
```

Im obigen Code wird eine geschachtelte Klasse generiert `MyClass.Notifications` mit den folgenden Methoden:

```csharp
public class MyClass {
   [..]
   public Notifications {
      public static NSObject ObserveDidStart (EventHandler<NSNotificationEventArgs> handler)
      public static NSObject ObserveDidStart (NSObject objectToObserve, EventHandler<NSNotificationEventArgs> handler)
   }
}
```

Benutzer Ihres Codes können dann problemlos Abonnieren von Benachrichtigungen, die bereitgestellt werden, um die [NSDefaultCenter](https://developer.xamarin.com/api/property/Foundation.NSNotificationCenter.DefaultCenter/) mithilfe von Code wie folgt:

```csharp
var token = MyClass.Notifications.ObserverDidStart ((notification) => {
    Console.WriteLine ("Observed the 'DidStart' event!");
});
```

Oder legen Sie ein bestimmtes Objekt, das überwacht werden. Wenn Sie übergeben `null` zu `objectToObserve` diese Methode verhält sich genauso wie die anderen Peer.

```csharp
var token = MyClass.Notifications.ObserverDidStart (objectToObserve, (notification) => {
    Console.WriteLine ("Observed the 'DidStart' event on objectToObserve!");
});
```

Der zurückgegebene Wert von `ObserveDidStart` können verwendet werden, um ganz einfach keine Benachrichtigungen mehr empfangen, wie folgt:

```csharp
token.Dispose ();
```

Oder Sie rufen [NSNotification.DefaultCenter.RemoveObserver](https://developer.xamarin.com/api/member/Foundation.NSNotificationCenter.RemoveObserver/p/Foundation.NSObject//) und übergeben Sie das Token. Wenn die Benachrichtigung an den Parameter enthält, sollten Sie angeben, dass eine Hilfsprogramm `EventArgs` Schnittstelle, die wie folgt:

```csharp
interface MyClass {
    [Notification (typeof (MyScreenChangedEventArgs)]
    [Field ("MyClassScreenChangedNotification")]
    NSString ScreenChangedNotification { get; }
}

// The helper EventArgs declaration
interface MyScreenChangedEventArgs {
    [Export ("ScreenXKey")]
    nint ScreenX { get; set; }

    [Export ("ScreenYKey")]
    nint ScreenY { get; set; }

    [Export ("DidGoOffKey")]
    [ProbePresence]
    bool DidGoOff { get; }
}
```

Der oben generiert eine `MyScreenChangedEventArgs` -Klasse mit der `ScreenX` und `ScreenY` Eigenschaften, mit denen die Daten abgerufen werden die [NSNotification.UserInfo](https://developer.xamarin.com/api/property/Foundation.NSNotification.UserInfo/) Wörterbuch mit den Schlüsseln `ScreenXKey` und `ScreenYKey` bzw. und wenden Sie die ordnungsgemäße Konvertierungen. Die `[ProbePresence]` -Attribut für den Generator verwendet, um den Prüfpunkt, wenn der Schlüssel, in festgelegt ist der `UserInfo`, anstatt zu versuchen, den Wert zu extrahieren. Dies ist für Fälle verwendet, in denen das Vorhandensein des Schlüssels den Wert (in der Regel für boolesche Werte) ist.

Dadurch können Sie Code wie diesen schreiben:

```csharp
var token = MyClass.NotificationsObserveScreenChanged ((notification) => {
    Console.WriteLine ("The new screen dimensions are {0},{1}", notification.ScreenX, notification.ScreenY);
});
```

In einigen Fällen müssen Sie keine Konstante, die Verbindung mit dem Wert im Wörterbuch übergeben vorhanden ist.  Apple verwendet gelegentlich die öffentlichen Symboldateien Konstanten und manchmal verwendet Zeichenfolgenkonstanten.  In der Standardeinstellung die [ `[Export]` ](#ExportAttribute) -Attribut in der angegebenen `EventArgs` Klasse verwendet den angegebenen Namen als Symbol für die öffentliche Laufzeit gesucht werden soll.  Wenn dies nicht der Fall ist, und sie wird stattdessen als Zeichenfolgenkonstante nachgeschlagen werden, und übergeben Sie normalerweise die `ArgumentSemantic.Assign` Wert, der das Export-Attribut.

**Neues in Xamarin.iOS 8.4**

In einigen Fällen Benachrichtigungen beginnt die Lebensdauer ohne Argumente, also die Verwendung von [ `[Notification]` ](#NotificationAttribute) ohne Argumente zulässig ist.  Aber manchmal, Parameter für die Benachrichtigung werden vorgestellt.  Um dieses Szenario zu unterstützen, kann das Attribut mehrmals angewendet werden.

Wenn Sie eine Bindung entwickeln, und Sie zu vermeiden, dass vorhandene Benutzercode möchten, würden Sie eine vorhandene Benachrichtigung von deaktivieren:

```csharp
interface MyClass {
    [Notification]
    [Field ("MyClassScreenChangedNotification")]
    NSString ScreenChangedNotification { get; }
}
```

In eine Version, die Benachrichtigungen werden die Attribute aufgeführt, wie folgt:

```csharp
interface MyClass {
    [Notification]
    [Notification (typeof (MyScreenChangedEventArgs)]
    [Field ("MyClassScreenChangedNotification")]
    NSString ScreenChangedNotification { get; }
}
```

<a name="NullAllowedAttribute" />

### <a name="nullallowedattribute"></a>NullAllowedAttribute

Wenn dies auf eine Eigenschaft angewendet wird kennzeichnet es die Eigenschaft als zulassen der Beibehaltung der `null` zugewiesen werden. Dies ist nur für Verweistypen zulässig.

Wenn dies auf einen Parameter in einer Methodensignatur gibt an, dass der angegebene Parameter null sein kann und keine Überprüfung soll, für die Übergabe ausgeführt werden angewendet wird `null` Werte.

Der Verweistyp nicht über dieses Attribut verfügt, die Bindung Tool wird überprüft, ob der Wert zugewiesen wird, vor der Übergabe an die Objective-C generiert, und generiert eine Überprüfung, die eine Ausnahme auslöst, ein `ArgumentNullException` , wenn der zugewiesene Wert ist `null`.

Zum Beispiel:

```csharp
// In properties

[NullAllowed]
UIImage IconFile { get; set; }

// In methods
void SetImage ([NullAllowed] UIImage image, State forState);
```

<a name="OverrideAttribute" />

### <a name="overrideattribute"></a>OverrideAttribute

Mit diesem Attribut können Sie dem Bindung Generator anzuweisen, dass die Bindung für diese bestimmte Methode gekennzeichnet werden soll, mit einem `override` Schlüsselwort.

### <a name="presnippetattribute"></a>PreSnippetAttribute

Sie können dieses Attribut zum Einfügen von Code eingefügt werden soll, nachdem die Eingabeparameter validiert wurden, aber vor der Code ruft in Objective-c verwenden

Beispiel:

```csharp
[Export ("demo")]
[PreSnippet ("var old = ViewController;")]
void Demo ();
```

### <a name="prologuesnippetattribute"></a>PrologueSnippetAttribute

Sie können dieses Attribut verwenden, um das injizieren von Code eingefügt werden soll, bevor einer der Parameter in der generierten Methode überprüft werden.

Beispiel:

```csharp
[Export ("demo")]
[Prologue ("Trace.Entry ();")]
void Demo ();
```

### <a name="postgetattribute"></a>PostGetAttribute

Weist den Generator Bindung zum Aufrufen der angegebenen Eigenschaft aus dieser Klasse, die von ihm ein Wert abgerufen.

Diese Eigenschaft wird normalerweise verwendet, um den Cache zu aktualisieren, der auf Verweisobjekte verweist, die das Objektdiagramm, die auf die verwiesen wird beibehalten. In der Regel wird es in Code verwenden, Vorgänge wie das hinzufügen/entfernen. Diese Methode wird verwendet, sodass nach dem Elemente hinzugefügt oder entfernt werden, dass die interne Cache aktualisiert werden, um sicherzustellen, dass wir verwalteten Verweise auf Objekte gehalten werden, die auch tatsächlich genutzt werden. Dies ist möglich, da das Tool für die Bindung ein dahinter liegendes Feld für alle Verweisobjekte in einer bestimmten Bindung generiert.

Beispiel:

```csharp
[BaseType (typeof (NSObject))]
[Since (4,0)]
public interface NSOperation {
    [Export ("addDependency:")][PostGet ("Dependencies")]
    void AddDependency (NSOperation op);

    [Export ("removeDependency:")][PostGet ("Dependencies")]
    void RemoveDependency (NSOperation op);

    [Export ("dependencies")]
    NSOperation [] Dependencies { get; }
}
```

In diesem Fall die `Dependencies` Eigenschaft wird aufgerufen, nachdem das Hinzufügen oder entfernen Abhängigkeiten von der `NSOperation` , sicherzustellen, dass haben wir ein Diagramm, das die tatsächliche darstellt geladen Objekte, die verhindern, die Speicherverluste sowie Speicherbeschädigung.

### <a name="postsnippetattribute"></a>PostSnippetAttribute

Sie können dieses Attribut verwenden, um eine langsamere C# Quellcode eingefügt werden soll, nachdem der Code die zugrunde liegende Objective-C-Methode aufgerufen hat

Beispiel:

```csharp
[Export ("demo")]
[PostSnippet ("if (old != null) old.DemoComplete ();")]
void Demo ();
```

### <a name="proxyattribute"></a>ProxyAttribute

Dieses Attribut wird angewendet, um Werte an, um diese flag als Proxy-Objekte zurückzugeben. Objective-C-APIs zurückgegeben Proxyobjekte, die vom Benutzer Bindungen nicht unterschieden werden können. Die Auswirkungen dieses Attributs ist so kennzeichnen Sie das Objekt als eine `DirectBinding` Objekt. Für ein Szenario in Xamarin.Mac, sehen Sie die [Informationen zu diesem Fehler](https://bugzilla.novell.com/show_bug.cgi?id=670844).

### <a name="retainlistattribute"></a>RetainListAttribute

Weist den Generator an, um einen verwalteten Verweis auf den Parameter beibehalten oder entfernen Sie einen internen Verweis auf den Parameter an. Hiermit wird die zu Objekten, die auf die verwiesen wird.

Syntax:

```csharp
public class RetainListAttribute: Attribute {
     public RetainListAttribute (bool doAdd, string listName);
}
```

Wenn der Wert des `doAdd` ist "true", und klicken Sie dann die Parameter hinzugefügt wird die `__mt_{0}_var List<NSObject>;`. In denen `{0}` ersetzt wird, mit der angegebenen `listName`. Sie müssen dieses dahinter liegende Feld in die komplementäre partielle Klasse an die API deklarieren.

Ein Beispiel finden Sie unter [foundation.cs](https://github.com/mono/maccore/blob/master/src/foundation.cs) und [NSNotificationCenter.cs](https://github.com/mono/maccore/blob/master/src/Foundation/NSNotificationCenter.cs)

### <a name="releaseattribute-xamarinios-60"></a>ReleaseAttribute (Xamarin.iOS 6.0)

Diese angewendet werden, um die Rückgabetypen, um anzugeben, dass der Generator aufrufen soll `Release` für das Objekt vor der Rückgabe. Dies ist nur erforderlich, wenn eine Methode erhalten Sie ein Objekt beibehalten (im Gegensatz zu einer Autoreleased Objekt, das häufigste Szenario)

Beispiel:

```csharp
[Export ("getAndRetainObject")]
[return: Release ()]
NSObject GetAndRetainObject ();
```

Außerdem wird dieses Attribut auf dem generierten Code weitergegeben, damit die Xamarin.iOS-Runtime weiß, dass sie das Objekt nach dem Beenden einer solchen Funktion mit Objective-C beibehalten muss.


### <a name="sealedattribute"></a>SealedAttribute

Weist den Generator an, um die generierte Methode als versiegelt zu kennzeichnen. Wenn dieses Attribut nicht angegeben wird, ist die Standardeinstellung, um eine virtuelle Methode (eine virtuelle Methode, eine abstrakte Methode oder eine Außerkraftsetzung, je nachdem wie andere Attribute verwendet werden) zu generieren.

<a name="StaticAttribute" />

### <a name="staticattribute"></a>StaticAttribute

Wenn die `[Static]` -Attribut auf eine Methode oder Eigenschaft angewendet wird, dadurch wird eine statische Methode oder Eigenschaft. Wenn dieses Attribut nicht angegeben ist, erzeugt der Generator eine Instanzmethode oder Eigenschaft.


### <a name="transientattribute"></a>TransientAttribute

Verwenden Sie dieses Attribut auf Eigenschaften von Installationsflag, deren Werte, also vorübergehend sind, Objekte, die unter iOS werden vorübergehend erstellt, aber sind nicht dauerhaften. Wenn dieses Attribut auf eine Eigenschaft angewendet wird, erstellt der Generator keinem dahinter liegenden Feld für diese Eigenschaft, was bedeutet, dass die verwaltete Klasse einen Verweis auf das Objekt nicht Schritt halten kann.

<a name="WrapAttribute" />

### <a name="wrapattribute"></a>WrapAttribute

Beim Entwurf der Xamarin.iOS/Xamarin.Mac Bindungen die `[Wrap]` Attribut wird verwendet, um eine schwach typisierte Objekt mit einem stark typisierten Objekt zu umschließen. Dies kommt vor allem mit Objective-C-Delegatobjekte, die in der Regel als Typ deklariert werden `id` oder `NSObject`. Die Konvention, die von Xamarin.iOS- und Xamarin.Mac verwendet wird, diese Delegaten oder Datenquellen als Typ verfügbar zu machen `NSObject` und mit der Konvention "Weak" und den verfügbar gemachten Namen benannt. Ein `id delegate` von Objective-C-Eigenschaft verfügbar sein würde eine `NSObject WeakDelegate { get; set; }` Eigenschaft in der API-Vertrag-Datei.

In der Regel der Wert, der für diesen Delegaten zugewiesen wird ist jedoch einen starken Typ, damit wir den starken Typ Oberfläche und Anwenden der `[Wrap]` -Attribut, das bedeutet, dass die Benutzer auswählen können, die unsichere Typen verwenden, ggf. einige Fine-Steuerelement, oder wenn sie auf Low-Level Tric zurückgreifen müssen KS, oder sie können die stark typisierte Eigenschaft für die meisten ihrer Arbeit verwenden.

Beispiel:

```csharp
[BaseType (typeof (NSObject))]
interface Demo {
     [Export ("delegate"), NullAllowed]
     NSObject WeakDelegate { get; set; }

     [Wrap ("WeakDelegate")]
     DemoDelegate Delegate { get; set; }
}

[BaseType (typeof (NSObject))]
[Model][Protocol]
interface DemoDelegate {
    [Export ("doDemo")]
    void DoDemo ();
}
```

Dies ist wie der Benutzer die schwach typisierte Version des Delegaten verwenden würden:

```csharp
// The weak case, user has to roll his own
class SomeObject : NSObject {
    [Export ("doDemo")]
    void CallbackForDoDemo () {}

}

var demo = new Demo ();
demo.WeakDelegate = new SomeObject ();
```

Und das ist wie der Benutzer die stark typisierte Version verwenden würden Beachten Sie, dass der Benutzer nutzt C#des Typsystems und seiner Absicht deklarieren das Override-Schlüsselwort verwendet und dass er keine manuell ergänzen Sie die Methode mit `[Export]`, da wir haben Sie die Arbeit in der Bindung für den Benutzer an:

```csharp
// This is the strong case,
class MyDelegate : DemoDelegate {
   override void Demo DoDemo () {}
}

var strongDemo = new Demo ();
demo.Delegate = new MyDelegate ();
```

Eine weitere Verwendungsmöglichkeit der der `[Wrap]` -Attribut ist, um stark typisierte Version der Methoden zu unterstützen.  Zum Beispiel:

```csharp
[BaseType (typeof (NSObject))]
interface XyzPanel {
    [Export ("playback:withOptions:")]
    void Playback (string fileName, [NullAllowed] NSDictionary options);

    [Wrap ("Playback (fileName, options == null ? null : options.Dictionary")]
    void Playback (string fileName, XyzOptions options);
}
```

Die Elemente, die vom `[Wrap]` sind nicht `virtual` standardmäßig, wenn Sie benötigen eine `virtual` Member, die Sie zum festlegen können `true` den optionalen `isVirtual` Parameter.

```csharp
[BaseType (typeof (NSObject))]
interface FooExplorer {
    [Export ("fooWithContentsOfURL:")]
    void FromUrl (NSUrl url);

    [Wrap ("FromUrl (NSUrl.FromString (url))", isVirtual: true)]
    void FromUrl (string url);
}
```

## <a name="parameter-attributes"></a>Parameterattribute

In diesem Abschnitt wird beschrieben, die Attribute, die auf die Parameter in eine Methodendefinition angewendet werden können, sowie die `[NullAttribute]` , die für eine Eigenschaft als Ganzes gilt.

<a name="BlockCallback" />

### <a name="blockcallback"></a>BlockCallback

Dieses Attribut wird angewendet, um Parametertypen in C# Delegatdeklarationen den Binder benachrichtigt der fraglichen Parameter, die die Objective-C entspricht Aufrufkonvention blockiert und auf diese Weise marshallen soll.

Dies wird in der Regel für Rückrufe verwendet, die wie folgt definiert sind, in der Objective-c:

```objc
typedef returnType (^SomeTypeDefinition) (int parameter1, NSString *parameter2);
```

Siehe auch: [CCallback](#CCallback).

<a name="CCallback" />

### <a name="ccallback"></a>CCallback

Dieses Attribut wird angewendet, um Parametertypen in C# Delegieren von Deklarationen, die der Bindung zu benachrichtigen, dass der betreffende Parameter entspricht der C-ABI-Funktionszeiger, der Aufrufkonvention und auf diese Weise marshallen soll.

Dies wird in der Regel für Rückrufe verwendet, die wie folgt definiert sind, in der Objective-c:

```objc
typedef returnType (*SomeTypeDefinition) (int parameter1, NSString *parameter2);
```

Siehe auch: [BlockCallback](#BlockCallback).

### <a name="params"></a>params

Sie können die `[Params]` Attribut in der letzten Arrayparameter einer Methodendefinition, um den Generator "Params" in der Definition einfügen müssen.   Dies ermöglicht die Bindung für optionale Parameter ermöglichen.

Z. B. die folgende Definition:

```csharp
[Export ("loadFiles:")]
void LoadFiles ([Params]NSUrl [] files);
```

Können den folgenden Code geschrieben werden:

```csharp
foo.LoadFiles (new NSUrl (url));
foo.LoadFiles (new NSUrl (url1), new NSUrl (url2), new NSUrl (url3));
```

Dies hat den zusätzlichen Vorteil, dass keine Benutzer zum Erstellen eines Arrays ausschließlich für die Übergabe von Elementen erforderlich ist.

<a name="plainstring" />

### <a name="plainstring"></a>PlainString

Können Sie die `[PlainString]` Attribut vor der Zeichenfolgenparameter an anweisen, den Generator Bindung übergeben Sie die Zeichenfolge als eine C-Zeichenfolge anstelle der Übergabe des Parameters als ein `NSString`.

Die meisten Objective-C-APIs nutzen `NSString` Parameter, aber eine Reihe von APIs verfügbar zu machen eine `char *` -API für das Übergeben von Zeichenfolgen, anstatt die `NSString` Variante.
Verwendung `[PlainString]` in diesen Fällen.

Um beispielsweise die folgenden Objective-C-Deklarationen:

```csharp
- (void) setText: (NSString *) theText;
- (void) logMessage: (char *) message;
```

Sollte wie folgt gebunden werden:

```csharp
[Export ("setText:")]
void SetText (string theText);

[Export ("logMessage:")]
void LogMessage ([PlainString] string theText);
```

### <a name="retainattribute"></a>RetainAttribute

Weist den Generator an, um einen Verweis auf den angegebenen Parameter zu halten. Der Generator bietet des Sicherungsspeichers für dieses Feld, oder geben Sie einen Namen (der `WrapName`) zum Speichern des Werts an. Dies ist nützlich für einen Verweis auf ein verwaltetes Objekt, das mit Objective-C als Parameter übergeben wird und wenn Sie wissen, dass die Objective-C nur diese Kopie des Objekts beibehält. Z. B. eine API wie `SetDisplay (SomeObject)` würden dieses Attribut verwenden, da es wahrscheinlich ist, dass die SetDisplay nur ein einzelnes Objekt gleichzeitig anzeigen kann. Wenn Sie zum Nachverfolgen mehr als ein Objekt (z. B. für eine API-Stapel-ähnliche müssen) möchten, verwenden die `[RetainList]` Attribut.

Syntax:

```csharp
public class RetainAttribute {
    public RetainAttribute ();
    public RetainAttribute (string wrapName);
    public string WrapName { get; }
}
```


### <a name="retainlistattribute"></a>RetainListAttribute

Weist den Generator an, um einen verwalteten Verweis auf den Parameter beibehalten oder entfernen Sie einen internen Verweis auf den Parameter an. Hiermit wird die zu Objekten, die auf die verwiesen wird.

Syntax:

```csharp
public class RetainListAttribute: Attribute {
     public RetainListAttribute (bool doAdd, string listName);
}
```

Wenn der Wert des `doAdd` ist "true", und klicken Sie dann die Parameter hinzugefügt wird die `__mt_{0}_var List<NSObject>`. In denen `{0}` ersetzt wird, mit der angegebenen `listName`. Sie müssen dieses dahinter liegende Feld in die komplementäre partielle Klasse an die API deklarieren.

Ein Beispiel finden Sie unter [foundation.cs](https://github.com/mono/maccore/blob/master/src/foundation.cs) und [NSNotificationCenter.cs](https://github.com/mono/maccore/blob/master/src/Foundation/NSNotificationCenter.cs)


### <a name="transientattribute"></a>TransientAttribute

Dieses Attribut auf Parameter angewendet wird, und wird nur verwendet werden, wenn der Übergang von Objective-C nach C#.  Während dieser Übergänge die verschiedenen Objective-C `NSObject` Parameter werden in eine verwaltete Darstellung des Objekts eingeschlossen.

Die Runtime einen Verweis auf das systemeigene Objekt übernimmt und der Verweis wird beibehalten, bis der letzte verwaltete Verweis auf das Objekt nicht mehr vorhanden ist, und der Garbage Collector hat die Möglichkeit, ausführen.

In einigen Fällen ist es wichtig, für die C# Laufzeit, um einen Verweis auf das systemeigene Objekt nicht beibehalten.  Dies geschieht gelegentlich auf, wenn es sich bei der zugrunde liegenden systemeigene Code ein besonderes Verhalten an den Lebenszyklus des Parameters angefügt hat.  Zum Beispiel: der Destruktor für den Parameter führen Sie eine Bereinigungsaktion wird, oder löschen Sie eine wertvolle Ressource.

Dieses Attribut informiert die Laufzeit, die Sie wünschen, dass das Objekt, das bei der Rückgabe an Objective-C-aus der überschriebenen Methode Wenn möglich freigegeben werden.

Die Regel ist sehr einfach: Wenn musste, dass die Laufzeit eine neue verwaltete Darstellung aus dem systemeigenen Objekt zu erstellen, und klicken Sie dann am Ende der Funktion, die Retain-Anzahl für das systemeigene Objekt gelöscht werden, und die Handle-Eigenschaft des verwalteten Objekts werden gelöscht.   Dies bedeutet, dass wenn Sie einen Verweis auf das verwaltete Objekt beibehalten, diesen Verweis nutzlos werden soll (Aufrufen von Methoden auf, es wird eine Ausnahme ausgelöst).

Wenn das übergebene Objekt nicht erstellt wurde, oder es wurde bereits eine ausstehende verwaltete Darstellung des Objekts, findet der erzwungenen Dispose nicht statt. 


## <a name="property-attributes"></a>Eigenschaftenattribute

<a name="NotImplementedAttribute" />

### <a name="notimplementedattribute"></a>NotImplementedAttribute

Dieses Attribut wird verwendet, um ein Objective-C-Idiom, in denen eine Eigenschaft mit einem Get-Methode wird in einer Basisklasse eingeführt und eine Unterklasse des änderbare führt einen Setter, zu unterstützen.

Da C# keine Unterstützung von diesem Modell wird die Basisklasse die Setter- und Getter muss und können Sie eine Unterklasse der [OverrideAttribute](#OverrideAttribute).

Dieses Attribut wird nur in Eigenschaften-Settern verwendet und dient zur Unterstützung des änderbar Idioms in Objective-c

Beispiel:

```csharp
[BaseType (typeof (NSObject))]
interface MyString {
    [Export ("initWithValue:")]
    IntPtr Constructor (string value);

    [Export ("value")]
    string Value {
        get;

    [NotImplemented ("Not available on MyString, use MyMutableString to set")]
        set;
    }
}

[BaseType (typeof (MyString))]
interface MyMutableString {
    [Export ("value")]
    [Override]
    string Value { get; set; }
}
```

<a name="enum-attributes" />

## <a name="enum-attributes"></a>Enum-Attribut

Zuordnen von `NSString` Konstanten Enum-Werte ist eine einfache Möglichkeit zum Erstellen besserer .NET API. Es:

* ermöglicht die codevervollständigung nützlicher, durch die Anzeige sein **nur** die richtigen Werte für die API;
* Fügt der typsicherheit, können keine anderen `NSString` Konstante in einem falschen Kontext und
* zum Ausblenden einiger Konstanten, codevervollständigung, kürzere API-Liste angezeigt wird, ohne Verlust der Funktionalität vornehmen können.

Beispiel:

```csharp
enum NSRunLoopMode {

    [DefaultEnumValue]
    [Field ("NSDefaultRunLoopMode")]
    Default,

    [Field ("NSRunLoopCommonModes")]
    Common,

    [Field (null)]
    Other = 1000
}
```

Aus der obigen Bindungsdefinition der Generator erstellt die `enum` selbst und erstellt auch eine `*Extensions` statischen Typ, der zwei-Wege-Konvertierungsmethoden zwischen Enum-Werte enthält und die `NSString` Konstanten. Dies bedeutet die Konstanten bleibt Entwicklern zur Verfügung, auch wenn sie nicht Teil der API sind.

Beispiele:

```csharp
// using the NSString constant in a different API / framework / 3rd party code
CallApiRequiringAnNSString (NSRunLoopMode.Default.GetConstant ());
```

```csharp
// converting the constants from a different API / framework / 3rd party code
var constant = CallApiReturningAnNSString ();
// back into an enum value
CallApiWithEnum (NSRunLoopModeExtensions.GetValue (constant));
```

### <a name="defaultenumvalueattribute"></a>DefaultEnumValueAttribute

Sie können ergänzen **eine** Enum-Wert mit diesem Attribut. Dadurch werden die Konstante zurückgegeben wird, wenn der Enum-Wert nicht bekannt ist.

Im obigen Beispiel:

```csharp
var x = (NSRunLoopMode) 99;
Call (x.GetConstant ()); // NSDefaultRunLoopMode will be used
```

Wenn kein Enumerationswert ergänzt wird ein `NotSupportedException` ausgelöst.

### <a name="errordomainattribute"></a>ErrorDomainAttribute

Fehlercodes sind als ein Enum-Werte gebunden. In der Regel eine Fehler-Domäne für sie vorhanden ist und es ist nicht immer leicht zu finden, die angewendet wird (oder ggf. vorhandenen eigenschaftennamenparameter selbst).

Sie können dieses Attribut verwenden, der Enumeration selbst Fehler Domäne zugeordnet werden soll.

Beispiel:

```csharp
    [Native]
    [ErrorDomain ("AVKitErrorDomain")]
    public enum AVKitError : nint {
        None = 0,
        Unknown = -1000,
        PictureInPictureStartFailed = -1001
    }
```

Sie können dann aufrufen die Erweiterungsmethode `GetDomain` um die Domäne eines eventuellen Fehlers Konstanten zu erhalten.

### <a name="fieldattribute"></a>FieldAttribute

Dies ist das gleiche `[Field]` für Konstanten im Typ verwendete Attribut. Sie können auch in Enumerationen verwendet werden, um einen Wert mit einer bestimmten Konstanten zuzuordnen.

Ein `null` Wert verwendet werden kann, geben Sie die Enum-Wert zurückgegeben werden sollen, wenn eine `null` `NSString` -Konstante angegeben wird.

Im obigen Beispiel:

```csharp
var constant = NSRunLoopMode.NewInWatchOS3; // will be null in watchOS 2.x
Call (NSRunLoopModeExtensions.GetValue (constant)); // will return 1000
```

Wenn kein `null` Wert vorhanden ist wird eine `ArgumentNullException` ausgelöst.

## <a name="global-attributes"></a>Globale Attribute

Globale Attribute gelten entweder mithilfe der `[assembly:]` Attributmodifizierer wie die [ `[LinkWithAttribute]` ](#LinkWithAttribute) oder kann verwendet und wie die [ `[Lion]` ](#SinceAndLionAttributes) und [ `[Since]` ](#SinceAndLionAttributes) Attribute.

<a name="LinkWithAttribute" />

### <a name="linkwithattribute"></a>LinkWithAttribute

Dies ist ein Attribut auf Assemblyebene die Entwickler zum Angeben der verknüpfungsframework-Flags erforderlich, um eine gebundene Bibliothek erneut verwenden, ohne dass der Verbraucher der Bibliothek die Gcc_flags manuell konfigurieren und den zusätzlichen Mtouch-Argumente, die in einer Bibliothek übergeben kann.

Syntax:

```csharp
// In properties
[Flags]
public enum LinkTarget {
    Simulator    = 1,
    ArmV6    = 2,
    ArmV7    = 4,
    Thumb    = 8,
}

[AttributeUsage(AttributeTargets.Assembly, AllowMultiple=true)]
public class LinkWithAttribute : Attribute {
    public LinkWithAttribute ();
    public LinkWithAttribute (string libraryName);
    public LinkWithAttribute (string libraryName, LinkTarget target);
    public LinkWithAttribute (string libraryName, LinkTarget target, string linkerFlags);
    public bool ForceLoad { get; set; }
    public string Frameworks { get; set; }
    public bool IsCxx { get; set;  }
    public string LibraryName { get; }
    public string LinkerFlags { get; set; }
    public LinkTarget LinkTarget { get; set; }
    public bool NeedsGccExceptionHandling { get; set; }
    public bool SmartLink { get; set; }
    public string WeakFrameworks { get; set; }
}
```

Dieses Attribut auf Assemblyebene angewendet wird, z. B. genau hierfür gibt es die [CorePlot Bindungen](https://github.com/mono/monotouch-bindings/tree/master/CorePlot) verwenden:

```csharp
[assembly: LinkWith ("libCorePlot-CocoaTouch.a", LinkTarget.ArmV7 | LinkTarget.ArmV7s | LinkTarget.Simulator, Frameworks = "CoreGraphics QuartzCore", ForceLoad = true)]
```

Bei Verwendung der `[LinkWith]` Attribut, das angegebene `libraryName` eingebettet ist, mit der resultierenden Assembly, die Benutzer können mit eine einzigen DLL zu liefern, die sowohl nicht verwalteten Abhängigkeiten als auch die Befehlszeilenflags erforderlich, um ordnungsgemäß zu nutzen, enthält die Xamarin.iOS-Bibliothek.

Es ist auch möglich, die nicht bieten eine `libraryName`, in diesem Fall die `LinkWith` Attribut an, dass nur zusätzliche Linker-Flags verwendet werden kann:

``` csharp
[assembly: LinkWith (LinkerFlags = "-lsqlite3")]
```

#### <a name="linkwithattribute-constructors"></a>LinkWithAttribute-Konstruktoren

Diese Konstruktoren ermöglichen Ihnen die Angabe der Bibliothek aus, um eine Verknüpfung mit und Einbetten in die resultierende Assembly, die unterstützten Ziele, die die Bibliothek unterstützt und alle optionalen Bibliotheksflags, die zur Verknüpfung mit der Bibliothek erforderlich sind.

Beachten Sie, dass die `LinkTarget` Argument wird abgeleitet von Xamarin.iOS und muss nicht festgelegt werden.

Beispiele:

```csharp
// Specify additional linker:
[assembly: LinkWith (LinkerFlags = "-sqlite3")]

// Specify library name for the constructor:
[assembly: LinkWith ("libDemo.a");

// Specify library name, and link target for the constructor:
[assembly: LinkWith ("libDemo.a", LinkTarget.Thumb | LinkTarget.Simulator);

// Specify only the library name, link target and linker flags for the constructor:
[assembly: LinkWith ("libDemo.a", LinkTarget.Thumb | LinkTarget.Simulator, SmartLink = true, ForceLoad = true, IsCxx = true);
```

#### <a name="linkwithattributeforceload"></a>LinkWithAttribute.ForceLoad

Die `ForceLoad` Eigenschaft wird verwendet, um zu entscheiden, ob die `-force_load` Link-Flag wird für das Verknüpfen von der nativen Bibliothek verwendet. Jetzt sollte dies immer "true" sein.

#### <a name="linkwithattributeframeworks"></a>LinkWithAttribute.Frameworks

Verfügt die Bibliothek gebunden wird keine spezielle Anforderung für alle Frameworks (außer `Foundation` und `UIKit`), legen Sie die `Frameworks` Eigenschaft, um eine Zeichenfolge, die eine durch Leerzeichen getrennte Liste der Frameworks erforderliche Plattform enthält. Z. B. Wenn Sie eine Bibliothek binden, erfordert `CoreGraphics` und `CoreText`, legen Sie die `Frameworks` Eigenschaft `"CoreGraphics CoreText"`.

#### <a name="linkwithattributeiscxx"></a>LinkWithAttribute.IsCxx

Legen Sie diese Eigenschaft auf "true", wenn die resultierende ausführbare Datei verwenden anstelle des Standardwerts, die einen C-Compiler ist einen C++-Compiler kompiliert werden muss. Verwenden Sie diese Option, wenn die Bibliothek, die Sie binden in C++ geschrieben wurde.

#### <a name="linkwithattributelibraryname"></a>LinkWithAttribute.LibraryName

Der Name der nicht verwaltete Bibliothek gebündelt. Dies ist eine Datei mit der Erweiterung "a" aus, und es kann Objektcode für mehrere Plattformen (z. B. "," ARM "und" X86 für den Simulator ") enthalten.

Frühere Versionen von Xamarin.iOS überprüft die `LinkTarget` Eigenschaft, um zu bestimmen, der Plattform Ihrer Bibliothek unterstützt, aber dies wird jetzt automatisch erkannt, und die `LinkTarget` Eigenschaft wird ignoriert.

#### <a name="linkwithattributelinkerflags"></a>LinkWithAttribute.LinkerFlags

Die `LinkerFlags` Zeichenfolge bietet eine Möglichkeit für Autoren von Bindungen an alle zusätzlichen Linker-Flags erforderlich, wenn es sich bei die native Bibliothek bei der Anwendung zu verknüpfen.

Z. B. wenn die native Bibliothek libxml2 und Zlib erfordert, legen Sie die `LinkerFlags` von string in `"-lxml2 -lz"`.

#### <a name="linkwithattributelinktarget"></a>LinkWithAttribute.LinkTarget

Frühere Versionen von Xamarin.iOS überprüft die `LinkTarget` Eigenschaft, um zu bestimmen, der Plattform Ihrer Bibliothek unterstützt, aber dies wird jetzt automatisch erkannt, und die `LinkTarget` Eigenschaft wird ignoriert.

#### <a name="linkwithattributeneedsgccexceptionhandling"></a>LinkWithAttribute.NeedsGccExceptionHandling

Legen Sie diese Eigenschaft auf "true", wenn die Bibliothek, die Sie verknüpfen die GCC-Ausnahmebehandlung-Bibliothek (Gcc_eh) erforderlich ist.

#### <a name="linkwithattributesmartlink"></a>LinkWithAttribute.SmartLink

Die `SmartLink` Eigenschaft sollte festgelegt werden, auf "true", können Sie Xamarin.iOS zu bestimmen, ob `ForceLoad` erforderlich ist, oder nicht.

#### <a name="linkwithattributeweakframeworks"></a>LinkWithAttribute.WeakFrameworks

Die `WeakFrameworks` Eigenschaft funktioniert genauso wie die `Frameworks` -Eigenschaft, außer dass zum Zeitpunkt der Verknüpfung, die `-weak_framework` Spezifizierer an Gcc übergeben wird, für jeden der aufgelisteten Frameworks.

`WeakFrameworks` ermöglicht es Bibliotheken und Anwendungen schwache Verknüpfung mit dem Webplattform-Frameworks, damit sie optional diese verwenden können, wenn sie verfügbar sind, aber nicht für harte Abhängigkeiten zwischen ihnen dies nützlich nehmen ist, wenn Ihre Bibliothek vorgesehen ist, um zusätzliche Features hinzuzufügen neuere iOS-Versionen. Weitere Informationen zur schwache Verknüpfung finden Sie in Apple Dokumentation auf [schwache Verknüpfung](http://developer.apple.com/library/mac/#documentation/MacOSX/Conceptual/BPFrameworks/Concepts/WeakLinking.html).

Es wäre gut für eine schwache Verknüpfung `Frameworks` Konten wie `CoreBluetooth`, `CoreImage`, `GLKit`, `NewsstandKit` und `Twitter` , da sie nur in iOS 5 verfügbar sind.

<a name="SinceAndLionAttributes" />

### <a name="sinceattribute-ios-and-lionattribute-macos"></a>SinceAttribute (iOS) und LionAttribute (MacOS)

Sie verwenden die `[Since]` Attribut auf Flag APIs mit rechtzeitig zu einem bestimmten Zeitpunkt eingeführt wird. Das Attribut sollte nur verwendet werden, um zu kennzeichnen Typen und Methoden, die ein Laufzeit-Problem verursachen könnten, wenn die zugrunde liegenden Klasse, Methode oder Eigenschaft nicht verfügbar ist.

Syntax:

```csharp
public SinceAttribute : Attribute {
     public SinceAttribute (byte major, byte minor);
     public byte Major, Minor;
}
```

Sie sollten im Allgemeinen nicht auf Enumerationen, Einschränkungen oder neue Strukturen angewendet werden da diese keinen Laufzeitfehler verursachen würde, wenn sie auf einem Gerät mit einer älteren Version des Betriebssystems ausgeführt werden.

Beispiel, wenn auf einen Typ angewendet:

```csharp
// Type introduced with iOS 4.2
[Since (4,2)]
[BaseType (typeof (UIPrintFormatter))]
interface UIViewPrintFormatter {
    [Export ("view")]
    UIView View { get; }
}
```

Beispiel, wenn für einen neuen Member angewendet:

```csharp
[BaseType (typeof (UIViewController))]
public interface UITableViewController {
    [Export ("tableView", ArgumentSemantic.Retain)]
    UITableView TableView { get; set; }

    [Since (3,2)]
    [Export ("clearsSelectionOnViewWillAppear")]
    bool ClearsSelectionOnViewWillAppear { get; set; }
```

Die `[Lion]` Attribut angewendet wird, auf die gleiche Weise, aber für Typen, die mit Lion eingeführt wurde. Der Grund für die Verwendung `[Lion]` und die spezifische Versionsnummer, die in iOS verwendet wird ist, iOS wird sehr häufig überarbeitet, OS X-Hauptversionen geschieht nur selten und es ist einfacher, das Betriebssystem durch ihre Codename als durch die Versionsnummer merken

### <a name="adviceattribute"></a>AdviceAttribute

Verwenden Sie dieses Attribut, um Entwicklern einen Hinweis zu anderen APIs, die möglicherweise einfacher zu verwenden.   Z. B. Wenn Sie eine stark typisierte Version einer API bereitstellen, können dieses Attribut für das Attribut schwach typisierte Sie den Entwickler, die eine bessere-API zu leiten.

Die Informationen von diesem Attribut wird angezeigt, in der Dokumentation und Tools können entwickelt werden, sodass benutzervorschläge zur Verbesserung von

### <a name="zerocopystringsattribute"></a>ZeroCopyStringsAttribute

Nur in Xamarin.iOS 5.4 und höher verfügbar.

Dieses Attribut weist den Generator, die die Bindung für diese bestimmte Bibliothek (wenn mit angewendet `[assembly:]`) oder Typ sollte das schnelle Kopieren von 0 (null) Zeichenfolge Marshalling verwenden. Dieses Attribut entspricht die Befehlszeilenoption übergeben `--zero-copy` des Generators.

Beim Kopieren von 0 (null) für Zeichenfolgen verwendet wird, verwendet der Generator effektiv dasselbe C# Zeichenfolge als die Zeichenfolge, die in Objective-C benötigt werden, ohne dass die Erstellung eines neuen `NSString` -Objekt, und vermeiden, kopieren die Daten aus der C# Formatzeichenfolgen auf der Objective-C-Zeichenfolge. Der einzige Nachteil der Verwendung von 0 (null) kopieren Zeichenfolgen ist, sicherzustellen, dass jede Zeichenfolgeneigenschaft, die Sie einschließen muss, die als gekennzeichnet werden zufällig `retain` oder `copy` hat die `[DisableZeroCopy]` -Attribut festgelegt. Dies ist erforderlich, da das Handle für das Kopieren von 0 (null) Zeichenfolgen wird auf dem Stapel zugeordnet ist, und nach der Funktionsrückgabe ungültig ist.

Beispiel:

```csharp
[ZeroCopyStrings]
[BaseType (typeof (NSObject))]
interface MyBinding {
    [Export ("name")]
    string Name { get; set; }

    [Export ("domain"), NullAllowed]
    string Domain { get; set; }

    [DisablZeroCopy]
    [Export ("someRetainedNSString")]
    string RetainedProperty { get; set; }
}

```

Sie können auch das Attribut auf Assemblyebene anwenden, und es gelten für alle Typen der Assembly:

```csharp
[assembly:ZeroCopyStrings]
```

## <a name="strongly-typed-dictionaries"></a>Stark typisierte Wörterbücher

Mit Xamarin.iOS 8.0, die wir haben Unterstützung für das leichte erstellen stark typisierter Klassen, Wrap `NSDictionaries`.

Während es immer möglich, verwenden, wurde die [DictionaryContainer](https://developer.xamarin.com/api/type/Foundation.DictionaryContainer/) Datentyp zusammen mit einer manuellen-API, es ist jetzt wesentlich einfacher, dazu.  Weitere Informationen finden Sie unter [starke Typen verfügbar macht.](~/cross-platform/macios/binding/objective-c-libraries.md#Surfacing_Strong_Types).

<a name="StrongDictionary" />

### <a name="strongdictionary"></a>StrongDictionary

Wenn dieses Attribut auf eine Schnittstelle angewendet wird, erzeugt der Generator eine Klasse mit dem gleichen Namen wie die Schnittstelle, die abgeleitet [DictionaryContainer](https://developer.xamarin.com/api/type/Foundation.DictionaryContainer/) und jede Eigenschaft, die in der Schnittstelle definiert, in ein stark typisiertes aktiviert Getter und Setter für das Wörterbuch.

Dies generiert automatisch eine Klasse, die instanziiert werden kann aus einer vorhandenen `NSDictionary` oder neu erstellt wurde.

Dieses Attribut nimmt einen Parameter, den Namen der Klasse, die die Schlüssel, die verwendet werden, Zugriff auf die Elemente im Wörterbuch enthält.   Standardmäßig wird jede Eigenschaft in der Schnittstelle mit dem Attribut ein Element in den angegebenen Typ für einen Namen mit dem Suffix "Key" suchen.

Zum Beispiel:

```csharp
[StrongDictionary ("MyOptionKeys")]
interface MyOption {
    string Name { get; set; }
    nint    Age  { get; set; }
}

[Static]
interface MyOptionKeys {
    // In Objective-C this is "NSString *MYOptionNameKey;"
    [Field ("MYOptionNameKey")]
    NSString NameKey { get; }

    // In Objective-C this is "NSString *MYOptionAgeKey;"
    [Field ("MYOptionAgeKey")]
    NSString AgeKey { get; }
}

```

Im obigen Fall der `MyOption` Klasse erzeugt eine Zeichenfolgeneigenschaft für `Name` verwenden, die die `MyOptionKeys.NameKey` als Schlüssel in das Wörterbuch, das eine Zeichenfolge abzurufen.   Und verwendet die `MyOptionKeys.AgeKey` als Schlüssel in das Wörterbuch, das Abrufen einer `NSNumber` enthält eine ganze Zahl.

Wenn Sie einen anderen Schlüssel verwenden möchten, können das Export-Attribut für die Eigenschaft, z. B. Sie:

```csharp
[StrongDictionary ("MyColoringKeys")]
interface MyColoringOptions {
    [Export ("TheName")]  // Override the default which would be NameKey
    string Name { get; set; }

    [Export ("TheAge")] // Override the default which would be AgeKey
    nint    Age  { get; set; }
}

[Static]
interface MyColoringKeys {
    // In Objective-C this is "NSString *MYColoringNameKey"
    [Field ("MYColoringNameKey")]
    NSString TheName { get; }

    // In Objective-C this is "NSString *MYColoringAgeKey"
    [Field ("MYColoringAgeKey")]
    NSString TheAge { get; }
}
```

#### <a name="strong-dictionary-types"></a>Starke Wörterbuchtypen

Die folgenden Datentypen werden in unterstützt die `StrongDictionary` Definition:

|C#Schnittstellentyp|`NSDictionary` Speichertyp|
|---|---|
|`bool`|`Boolean` in gespeicherten ein `NSNumber`|
|Enumerationswerte|ganze Zahl gespeichert, ein `NSNumber`|
|`int`|32-Bit-Ganzzahl, die in gespeicherten ein `NSNumber`|
|`uint`|32-Bit-Ganzzahl ohne Vorzeichen gespeichert, die einer `NSNumber`|
|`nint`|`NSInteger` in gespeicherten ein `NSNumber`|
|`nuint`|`NSUInteger` in gespeicherten ein `NSNumber`|
|`long`|64-Bit-Ganzzahl, die in gespeicherten ein `NSNumber`|
|`float`|32-Bit-Ganzzahl gespeichert als eine `NSNumber`|
|`double`|64-Bit-Ganzzahl gespeichert als eine `NSNumber`|
|`NSObject` und Unterklassen|`NSObject`|
|`NSDictionary`|`NSDictionary`|
|`string`|`NSString`|
|`NSString`|`NSString`|
|C#`Array` von `NSObject`|`NSArray`|
|C#`Array` von Enumerationen|`NSArray` mit `NSNumber` Werte|
