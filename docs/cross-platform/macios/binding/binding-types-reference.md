---
title: Bindungstypen Referenzhandbuch
ms.topic: article
ms.prod: xamarin
ms.assetid: C6618E9D-07FA-4C84-D014-10DAC989E48D
ms.technology: xamarin-cross-platform
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/06/2018
ms.openlocfilehash: 6042ab9aa861a08da421140857459b02a78f7c70
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="binding-types-reference-guide"></a>Bindungstypen Referenzhandbuch

Dieses Dokument beschreibt die Liste der Attribute, die können Sie Ihre API-Vertrag-Dateien, um die Bindung Laufwerk Kommentieren und Generieren des Codes

Xamarin.iOS und Xamarin.Mac-API-Verträge sind hauptsächlich in Form Schnittstellendefinitionen, die definieren, wie in c# geschrieben, Objective-C-Code in c# eingeblendet wird. Der Prozess umfasst eine Mischung aus Schnittstellendeklarationen sowie einige grundlegende Typdefinitionen, die möglicherweise API-Vertrags. Eine Einführung in die Bindung, finden Sie in unserem Begleithandbuch [Bindung Objective-C-Bibliotheken](~/cross-platform/macios/binding/objective-c-libraries.md).

## <a name="type-definitions"></a>Typdefinitionen

Syntax:

```csharp
[BaseType (typeof (BTYPE))
interface MyType [: Protocol1, Protocol2] {
     IntPtr Constructor (string foo);
}
```

Jede Schnittstelle in Ihrem Vertragsdefinition, die verfügt die `[BaseType]` -Attribut, das den Basistyp für das generierte Objekt deklariert. In der oben genannten Deklaration eine `MyType` Klasse C#-Typ generiert werden, bindet ein Objective-C-Typ aufgerufen **MyType**.

Wenn Sie keine Typen nach den angegebenen Typnamen angeben (im obigen Beispiel `Protocol1` und `Protocol2`) mit der Schnittstelle Vererbung Syntax der Inhalte mit diesen Oberflächen werden inline als wären sie Teil des Vertrags für vorhanden `MyType`.
Die Möglichkeit, der Xamarin.iOS Oberflächen, dass ein Typ ein Protokoll nimmt dient inlining alle Methoden und Eigenschaften, die in das Protokoll in den Typ selbst deklariert wurden.

Im folgenden gezeigt wie Objective-C-Deklaration für `UITextField` würde in einem Xamarin.iOS-Vertrag definiert werden:

```objc
@interface UITextField : UIControl <UITextInput> {

}
```

Wird wie folgt als C#-API-Vertrag geschrieben:

```csharp
[BaseType (typeof (UIControl))]
interface UITextField : UITextInput {
}
```

Sie können viele andere Aspekte der codegenerierung steuern, indem Sie andere Attribute auf die Schnittstelle anwenden sowie zum Konfigurieren der BaseType-Attribut.


### <a name="generating-events"></a>Ereignisse generieren

Eine Funktion von Xamarin.iOS und Xamarin.Mac-API-Entwurf ist, dass es als C#-Ereignisse und Rückrufe Objective-C Delegatklassen zuordnen. Benutzer können in einem instanzweise an, ob sie die Objective-C-Programmierungsmustern dar, indem Sie Eigenschaften wie zuweisen übernehmen möchten **Delegaten** eine Instanz einer Klasse, die die verschiedenen Methoden implementiert, die die Objective-C Common Language Runtime aufrufen würde, oder durch Auswählen der c#-Stil, Ereignisse und Eigenschaften.

Lassen Sie uns finden Sie ein Beispiel zum Verwenden des Modells Objective-C:

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

Im obigen Beispiel können Sie sehen, dass wir haben sich entschieden, zwei Methoden überschreiben, ist eine eine Benachrichtigung, dass ein Bildlauf Ereignis Stelle und das zweite auf, die stattgefunden hat einen Rückruf, der zurückgeben sollte einen booleschen Wert, der die ScrollView angewiesen wird, ob nach oben durchgeführt werden sollen oder nicht.

Das C#-Modell ermöglicht es, die Benutzer der Bibliothek zum Überwachen von Benachrichtigungen, die mithilfe der C#-Ereignissyntax oder die Syntax der Eigenschaft um Rückrufe zu verknüpfen, die zum Zurückgeben von Werten erwartet werden.

Wie sieht der C#-Code für die gleiche Funktion Verwendung von Lambdas sieht folgendermaßen aus:

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

Da Ereignisse keine Werte zurückgeben (einen void-Rückgabetyp aufweisen) können Sie mehrere Kopien verbinden. Die `ShouldScrollToTop` ist kein Ereignis, es wird stattdessen eine Eigenschaft mit dem Typ `UIScrollViewCondition` die über diese Signatur verfügt:

```csharp
public delegate bool UIScrollViewCondition (UIScrollView scrollView);
```

Es gibt einen booleschen Wert zurück, in diesem Fall die Lambda-Syntax erlaubt es uns, den Wert von Zurückgeben der `MakeDecision` Funktion.

Der Generator für die Bindung unterstützt wird, Generieren von Ereignissen und Eigenschaften, die eine Klasse wie verknüpft `UIScrollView` mit seiner `UIScrollViewDelegate` (auch rufen diese Skriptobjektmodell-Klasse), dies erfolgt durch Hinzufügen einer Anmerkung zu Ihrer `BaseType` Definition mit der `Events` und `Delegates`Parameter (siehe unten). Zusätzlich zum Hinzufügen einer Anmerkung zu den `BaseType` mit dieser Parameter ist es erforderlich, um den Generator einige weitere Komponenten zu informieren.

Für Ereignisse, die mehr als einen Parameter annehmen (in Objective-C die Konvention ist, dass der erste Parameter in einer Delegatklasse die Instanz des Objekts Sender) müssen Sie den Namen, die Sie für die generierte EventArgs-Klasse werden würde wie folgt angeben. Dies erfolgt mit der `EventArgs` Attribut in der Deklaration der Methode in Ihrer Modellklasse. Zum Beispiel:

```csharp
[BaseType (typeof (UINavigationControllerDelegate))]
[Model][Protocol]
public interface UIImagePickerControllerDelegate {
    [Export ("imagePickerController:didFinishPickingImage:editingInfo:"), EventArgs ("UIImagePickerImagePicked")]
    void FinishedPickingImage (UIImagePickerController picker, UIImage image, NSDictionary editingInfo);
}
```

Die obige Deklaration generiert eine `UIImagePickerImagePickedEventArgs` von abgeleitete Klasse `EventArgs` Packs werden beide Parameter die `UIImage` und `NSDictionary`. Der Generator erzeugt dies:

```csharp
public partial class UIImagePickerImagePickedEventArgs : EventArgs {
    public UIImagePickerImagePickedEventArgs (UIImage image, NSDictionary editingInfo);
    public UIImage Image { get; set; }
    public NSDictionary EditingInfo { get; set; }
}
```

Macht klicken Sie dann Folgendes in die UIImagePickerController-Klasse:

```csharp
public event EventHandler<UIImagePickerImagePickedEventArgs> FinishedPickingImage { add; remove; }
```

Modellmethoden, die einen Wert zurückgeben gebunden sind unterschiedlich. Diese erfordern sowohl einen Namen für den generierten C#-Delegaten (die Signatur für die Methode) und auch einen Standardwert zurückgeben, wenn der Benutzer keine Implementierung selbst bietet. Z. B. die `ShouldScrollToTop` Definition ist dies:

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
public interface UIScrollViewDelegate {
    [Export ("scrollViewShouldScrollToTop:"), DelegateName ("UIScrollViewCondition"), DefaultValue ("true")]
    bool ShouldScrollToTop (UIScrollView scrollView);
}
```

Die oben genannten erstellt eine `UIScrollViewCondition` mit der Signatur des Delegaten, der oben gezeigten wurde, und wenn der Benutzer nicht um eine Implementierung bereitstellt, wird der Rückgabewert "true" sein.

Zusätzlich zu den `DefaultValue` -Attribut, können Sie auch die `DefaultValueFromArgument` , die den Generator, um den Wert des angegebenen Parameters im Aufruf zurückzugeben leitet oder die `NoDefaultValue` Parameter, die dem Generator anweist, dass kein Standardwert vorhanden ist.


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

Verwenden Sie die `Name` Eigenschaft, um den Namen zu steuern, die diesen Typ an der Objective-C-Welt binden. Dies wird normalerweise verwendet, um die C#-Typ geben Sie einen Namen mit den .NET Framework-Entwurfsrichtlinien kompatibel ist, aber die ordnet eines Namens in Objective-C, die nicht diese Konvention folgt.

Beispiel im folgenden Fall ordnen wir die Objective-C `NSURLConnection` zu Typ `NSUrlConnection`, wie die .NET Framework-Entwurfsrichtlinien "Url" statt "URL" verwenden:

```csharp
[BaseType (typeof (NSObject), Name="NSURLConnection")]
interface NSUrlConnection {
}
```

Der angegebene Name angegeben ist, als der Wert für die generierte verwendet `[Register]` Attribut in der Bindung. Wenn `Name` nicht angegeben ist, wird der kurze Typname verwendet als der Wert für die `Register` -Attribut in der generierten Ausgabe.

#### <a name="basetypeevents-and-basetypedelegates"></a>BaseType.Events und BaseType.Delegates

Diese Eigenschaften werden verwendet, um die Generierung von C#-Laufwerk-Ereignisse in den generierten Klassen formatieren. Sie werden zum Verknüpfen einer angegebenen Klasse mit ihrer Objective-C-Delegat-Klasse. Häufig treten, in dem eine Klasse eine Delegatklasse zum Senden von Benachrichtigungen und Ereignisse verwendet, werden. Zum Beispiel eine `BarcodeScanner` müsste eine begleitende `BardodeScannerDelegate` Klasse. Die `BarcodeScanner` Klasse müsste in der Regel eine "Delegat"-Eigenschaft, die Sie eine Instanz zuordnen würden `BarcodeScannerDelegate` , um diese funktioniert zwar, Sie möglicherweise für Ihre Benutzer eine c# verfügbar machen möchten-wie Ereignisschnittstelle Stil, und klicken Sie in diesen Fällen verwenden Sie die `Events` und `Delegates` Eigenschaften der `BaseType` Attribut.

Diese Eigenschaften werden immer zusammen festgelegt und müssen die gleiche Anzahl von Elementen aufweisen, und synchron gehalten werden. Die `Delegates` Array eine Zeichenfolge für jede schwach typisierte Delegaten, die Sie einschließen möchten, und die Ereignisse Array enthält einen Typ für jeden Typ, die Sie zuordnen möchten.

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

Wenn Sie dieses Attribut anwenden, wenn neue Instanzen dieser Klasse erstellt werden, wird die Instanz dieses Objekts um beibehalten werden, bis die Methode verweist die `KeepRefUntil` aufgerufen wurde. Dies ist nützlich, um die Verwendbarkeit von APIs, verbessert werden, wenn Sie nicht, dass Ihre Benutzer zu einem Verweis auf ein Objekt, um den Code verwenden möchten. Der Wert dieser Eigenschaft ist der Name einer Methode in der `Delegate` Klasse, damit Sie diese in Kombination mit den Ereignissen verwenden müssen und `Delegates` sowie Eigenschaften.

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

Wenn dieses Attribut, um die Schnittstellendefinition angewendet wird wird es verhindert, dass den Generator, erzeugt des Standardkonstruktors.

Verwenden Sie dieses Attribut, wenn Sie das Objekt, das mit den anderen Konstruktoren in der Klasse initialisiert werden, benötigen.


### <a name="privatedefaultctorattribute"></a>PrivateDefaultCtorAttribute

Wenn dieses Attribut, um die Schnittstellendefinition angewendet wird wird es den Standardkonstruktor als privat kennzeichnen. Dies bedeutet, dass weiterhin werden Objekt dieser Klasse intern von der Erweiterungsdatei instanziiert können, aber nur nicht zugegriffen werden kann, um Benutzer der Klasse darauf.


### <a name="categoryattribute"></a>CategoryAttribute

Verwenden Sie dieses Attribut auf eine Typdefinition Objective-C-Kategorien zu binden und verfügbar zu machen, die als C#-Erweiterungsmethoden, die Möglichkeit zu spiegeln, die Objective-C-Funktionen verfügbar macht.

Kategorien sind ein Objective-C-Mechanismus verwendet, um den Satz von Methoden und Eigenschaften verfügbar, in einer Klasse zu erweitern.   In der Praxis, sie werden entweder Erweitern der Funktionalität von einer Basisklasse verwendet (z. B. `NSObject`) bei ein bestimmtes Frameworks in verknüpft ist (z. B. `UIKit`), machen ihre Methoden verfügbar, jedoch nur, wenn das neue Framework verknüpft ist.   In einigen Fällen werden sie zum Organisieren von Funktionen in einer Klasse nach Funktionalität.   Sie ähneln Willen C#-Erweiterungsmethoden.

Dies ist eine Kategorie wie im Ziel-"c:" Suchen

```objc
@interface UIView (MyUIViewExtension)
-(void) makeBackgroundRed;
@end
```

Im obigen Beispiel wenn finden Sie auf eine Bibliothek erweitern Instanzen `UIView` mit der Methode `makeBackgroundRed`.

Um diese zu binden, können Sie die `[Category]` Attribut in einer Schnittstellendefinition.   Bei Verwendung der `Category` Attribut, die Bedeutung von der `[BaseType]` Attribut ändert sich von verwendet wird, an die Basisklasse erweitern, um den Typ erweitert werden.

Im folgenden gezeigt wie die `UIView` Erweiterungen gebunden und in C#-Erweiterungsmethoden umgewandelt werden:

```csharp
[BaseType (typeof (UIView))]
[Category]
interface MyUIViewExtension {
    [Export ("makeBackgroundRed")]
    void MakeBackgroundRed ();
}
```

Die oben genannten erstellt eine `MyUIViewExtension` einer Klasse enthält die `MakeBackgroundRed` Erweiterungsmethode.   Dies bedeutet, dass Sie nun "MakeBackgroundRed" aufrufen können, auf einem beliebigen `UIView` -Unterklasse, und Sie haben die gleiche Funktionalität erhalten Sie in Objective-C.

In einigen Fällen finden Sie **statische** Member in Kategorien wie im folgenden Beispiel:

```objc
@interface FooObject (MyFooObjectExtension)
+ (BOOL)boolMethod:(NSRange *)range;
@end
```

Dies führt zu einer **falsch** Kategorie c# Schnittstellendefinition:

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

Dies ist falsch da verwendet die `BoolMethod` Erweiterung benötigen Sie eine Instanz des `FooObject` , aber Sie sind ein ObjC binden **statische** Erweiterung, dies ist ein Nebeneffekt aufgrund der Tatsache der Umsetzung von C#-Erweiterungsmethoden.

Die einzige Möglichkeit, die oben beschriebenen Definitionen zu verwenden, wird durch den folgenden problematischen Code:

```csharp
(null as FooObject).BoolMethod (range);
```

Wird empfohlen, dieses Problem zu vermeiden, Inline der `BoolMethod` Definition innerhalb der `FooObject` Schnittstellendefinition selbst, dadurch können Sie diese Erweiterung aufrufen, wie dies soll `FooObject.BoolMethod (range)`.

```csharp
[BaseType (typeof (NSObject))]
interface FooObject {

    [Static]
    [Export ("boolMethod:")]
    bool BoolMethod (NSRange range);
}
```

Es wird eine Warnung (BI1117) ausgeben, wenn es gefunden eine `[Static]` Member innerhalb einer `[Category]` Definition. Wenn Sie tatsächlich haben möchten `[Static]` Elemente innerhalb Ihrer `[Category]` Definitionen können Sie die Warnung unterdrücken, mit `[Category (allowStaticMembers: true)]` oder werden, indem entweder das Element oder `[Category]` -Schnittstellendefinition mit `[Internal]`.


### <a name="staticattribute"></a>StaticAttribute

Wenn dieses Attribut auf eine Klasse angewendet wird, wird es nur eine statische Klasse, die nicht von abgeleitet ist generiert `NSObject` also die `[BaseType]` Attribut wird ignoriert. Statische Klassen dienen zum Hosten von C öffentlicher Variablen, die Sie verfügbar machen möchten.

Zum Beispiel:

```csharp
[Static]
interface CBAdvertisement {
    [Field ("CBAdvertisementDataServiceUUIDsKey")]
    NSString DataServiceUUIDsKey { get; }
```

Generiert eine C#-Klasse mit der folgenden-API:

```csharp
public partial class CBAdvertisement  {
    public static NSString DataServiceUUIDsKey { get; }
}
```


## <a name="protocol-definitionsmodel"></a>Definitionen/-Protokollmodell

Modelle werden in der Regel von protokollimplementierung verwendet.
Sie unterscheiden sich, dass die Common Language Runtime nur bei Objective-C-Methoden registriert werden, die tatsächlich überschrieben wurde.
Andernfalls wird die Methode nicht registriert werden.

Wenn daher im Allgemeinen Unterklasse eine Klasse, die mit gekennzeichnet wurde, hat die `ModelAttribute`, sollten Sie die Basismethode nicht aufrufen.   Beim Aufrufen dieser Methode löst eine Ausnahme, die Sie die gesamte Implementierung des Verhaltens auf die Unterklasse für alle Methoden, die Sie überschreiben soll.


### <a name="abstractattribute"></a>AbstractAttribute

Standardmäßig sind Elemente, die Teil eines Protokolls sind nicht obligatorisch. Dies ermöglicht es Benutzern, erstellen Sie eine Unterklasse von der `Model` Objekt, indem Sie lediglich Ableiten von der Klasse in C# geschrieben und überschreiben nur die Methoden, die ihnen wichtig. In einigen Fällen der Objective-C-Vertrag erfordert, dass der Benutzer eine Implementierung für diese Methode bietet (die gekennzeichnet werden, mit der @required -Direktive in Objective-C). In diesen Fällen sollten Sie kennzeichnen Methoden mit den `Abstract` Attribut.

Die `Abstract` Attribut kann auf Methoden oder Eigenschaften angewendet werden und bewirkt, dass den Generator so kennzeichnen Sie die generierte Element als "abstract" und die Klasse, um eine abstrakte Klasse sein.

Folgendes wird von Xamarin.iOS entnommen:

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

Gibt den Standardwert von eine Modell-Methode zurückgegeben werden sollen, wenn der Benutzer nicht über eine Methode für diese bestimmte Methode in das Modellobjekt erbringt

Syntax:

```csharp
public class DefaultValueAttribute : Attribute {
        public DefaultValueAttribute (object o);
        public object Default { get; set; }
}
```

Z. B. in der folgenden imaginären Delegate-Klasse für eine `Camera` Klasse, die wir bieten eine `ShouldUploadToServer` die würde verfügbar gemacht werden als Eigenschaft auf die `Camera` Klasse. Wenn der Benutzer die `Camera` Klasse nicht explizit festlegt eine der Wert einen Lambda-Ausdruck, der "true" oder "false" reagieren kann, der Standardwert return in diesem Fall wäre "false" der Wert, den wir in der `DefaultValue` Attribut:

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
interface CameraDelegate {
    [Export ("camera:shouldPromptForAction:"), DefaultValue (false)]
    bool ShouldUploadToServer (Camera camera, CameraAction action);
}
```

Der Benutzer einen Handler in der imaginäre Klasse festgelegt, wird dieser Wert ignoriert:

```csharp
var camera = new Camera ();
camera.ShouldUploadToServer = (camera, action) => return SomeDecision ();
```

Siehe auch: [NoDefaultValueAttribute](#NoDefaultValueAttribute), [DefaultValueFromArgumentAttribute](#DefaultValueFromArgumentAttribute).

<a name="DefaultValueFromArgumentAttribute" />

### <a name="defaultvaluefromargumentattribute"></a>DefaultValueFromArgumentAttribute

Syntax:

```csharp
public class DefaultValueFromArgumentAttribute : Attribute {
    public DefaultValueFromArgumentAttribute (string argument);
    public string Argument { get; }
}
```

Dieses Attribut, wenn auf eine Methode angegeben, die einen Wert, auf eine Modellklasse zurückgibt weisen Sie den Generator, um den Wert des angegebenen Parameters zurück, wenn der Benutzer sein eigenes Methode oder einem Lambda nicht bereitgestellt wurden.

Beispiel:

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
public interface NSAnimationDelegate {
    [Export ("animation:valueForProgress:"), DelegateName ("NSAnimationProgress"), DefaultValueFromArgumentAttribute ("progress")]
    float ComputeAnimationCurve (NSAnimation animation, nfloat progress);
}
```

In der oben genannten Fall, wenn der Benutzer die `NSAnimation` Klasse C#-Ereignisse/Eigenschaften verwenden möchten, und wurde nicht festgelegt `NSAnimation.ComputeAnimationCurve` an eine Methode oder ein Lambda, wäre der Rückgabewert in den Status-Parameter übergebene Wert.

Siehe auch: [NoDefaultValueAttribute](#NoDefaultValueAttribute), [DefaultValueAttribute](#DefaultValueAttribute)

### <a name="ignoredindelegateattribute"></a>IgnoredInDelegateAttribute

In einigen Fällen ist es sinnvoll nicht verfügbar zu machen ein Ereignis oder die Eigenschaft eine Modellklasse in die Hostklasse delegieren, damit durch Hinzufügen dieses Attributs den Generator, vermeiden Sie die Generierung einer beliebigen Methode, die mit ihm ergänzt anweisen.

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

Dieses Attribut wird im Modell-Methoden verwendet, die zum Festlegen des Namens der Signatur des Delegaten verwendet Werte zurückgeben.

Beispiel:

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
public interface NSAnimationDelegate {
    [Export ("animation:valueForProgress:"), DelegateName ("NSAnimationProgress"), DefaultValueFromArgumentAttribute ("progress")]
    float ComputeAnimationCurve (NSAnimation animation, float progress);
}
```

Mit der oben genannten Definition liefert der Generator die folgende öffentliche Deklaration:

```csharp
public delegate float NSAnimationProgress (MonoMac.AppKit.NSAnimation animation, float progress);
```

### <a name="delegateapinameattribute"></a>DelegateApiNameAttribute

Dieses Attribut wird verwendet, um den Generator zum Ändern des Namens der Eigenschaft in die Hostklasse generiert zu ermöglichen. Manchmal ist es hilfreich, wenn der Name der Methode der FooDelegate-Klasse sinnvoll für die Delegate-Klasse ist, aber in die Hostklasse als Eigenschaft ungerade sieht.

Dies ist auch wirklich nützlich (und erforderlichen) Wenn Sie zwei oder weitere Überladung-Methoden, die sinnvoll, bleiben in der Klasse FooDelegate ist mit dem Namen sich Sie sie in der Hostklasse mit einem besseren angegebenen Namen verfügbar machen möchten.

Beispiel:

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
public interface NSAnimationDelegate {
    [Export ("animation:valueForProgress:"), DelegateApiName ("ComputeAnimationCurve"), DelegateName ("Func<NSAnimation, float, float>"), DefaultValueFromArgument ("progress")]
    float GetValueForProgress (NSAnimation animation, float progress);
}
```

Mit der oben genannten Definition liefert der Generator die folgende öffentliche Deklaration in der Hostklasse:

```csharp
public Func<NSAnimation, float, float> ComputeAnimationCurve { get; set; }
```

### <a name="eventargsattribute"></a>EventArgsAttribute

Für Ereignisse, die mehr als einen Parameter annehmen (in Objective-C die Konvention ist, dass der erste Parameter in einer Delegatklasse die Instanz des Objekts Sender) müssen Sie den Namen, die Sie für die generierte EventArgs-Klasse werden würde wie folgt angeben. Dies erfolgt mit der `EventArgs` Attribut in der Deklaration der Methode in Ihrer `Model` Klasse.

Zum Beispiel:

```csharp
[BaseType (typeof (UINavigationControllerDelegate))]
[Model][Protocol]
public interface UIImagePickerControllerDelegate {
    [Export ("imagePickerController:didFinishPickingImage:editingInfo:"), EventArgs ("UIImagePickerImagePicked")]
    void FinishedPickingImage (UIImagePickerController picker, UIImage image, NSDictionary editingInfo);
}
```

Die obige Deklaration generiert eine `UIImagePickerImagePickedEventArgs` -Klasse, die von EventArgs abgeleitet ist und beide Parameter packs der `UIImage` und `NSDictionary`. Der Generator erzeugt dies:

```csharp
public partial class UIImagePickerImagePickedEventArgs : EventArgs {
    public UIImagePickerImagePickedEventArgs (UIImage image, NSDictionary editingInfo);
    public UIImage Image { get; set; }
    public NSDictionary EditingInfo { get; set; }
}
```

Macht klicken Sie dann Folgendes in die UIImagePickerController-Klasse:

```csharp
public event EventHandler<UIImagePickerImagePickedEventArgs> FinishedPickingImage { add; remove; }
```


### <a name="eventnameattribute"></a>EventNameAttribute

Dieses Attribut wird verwendet, um den Generator zum Ändern des Namens eines Ereignisses oder einer Eigenschaft, die in der Klasse generiert zu ermöglichen. Manchmal ist es hilfreich, wenn der Name des der `Model` Klassenmethode ist sinnvoll für die Modellklasse, aber sieht wie ein Ereignis oder eine Eigenschaft in der ursprünglichen Klasse ungerade.

Z. B. die `UIWebView` verwendet die folgenden Bit aus der `UIWebViewDelegate`:

```csharp
[Export ("webViewDidFinishLoad:"), EventArgs ("UIWebView"), EventName ("LoadFinished")]
void LoadingFinished (UIWebView webView);
```

Die oben genannten macht `LoadingFinished` wie die Methode in der `UIWebViewDelegate`, aber `LoadFinished` des Ereignisses zum Einbinden in eine `UIWebView`:

```csharp
var webView = new UIWebView (...);
webView.LoadFinished += delegate { Console.WriteLine ("done!"); }
```


### <a name="modelattribute"></a>ModelAttribute

Beim Anwenden der `Model` Attribut auf eine Typdefinition in Ihrem Vertrag-API, die Common Language Runtime generiert speziellen Code, der nur Aufrufe an Methoden in der Klasse Oberfläche wird, wenn der Benutzer eine Methode in der Klasse überschrieben wurde. Dieses Attribut ist in der Regel auf alle APIs angewendet, die eine Objective-C-Delegatklasse umschließen.

<a name="NoDefaultValueAttribute" />

### <a name="nodefaultvalueattribute"></a>NoDefaultValueAttribute

Gibt an, dass die Methode für das Modell keinen Standardrückgabewert bereitstellt.

Dies funktioniert bei Objective-C-Laufzeit von "falsch" reagiert auf die Objective-C-Laufzeit-Anforderung zu bestimmen, ob es sich bei der angegebene Selektor in dieser Klasse implementiert wird.

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
interface CameraDelegate {
    [Export ("shouldDisplayPopup"), NoDefaultValue]
    bool ShouldUploadToServer ();
}
```

Siehe auch: [DefaultValueAttribute](#DefaultValueAttribute) und [DefaultValueAttribute](#DefaultValueAttribute).

## <a name="protocols"></a>Protokolle

Das Konzept der Objective-C-Protokoll ist in c# nicht wirklich vorhanden. Protokolle sind mit C#-Schnittstellen vergleichbar, allerdings unterscheiden sich darin, dass nicht alle Methoden und Eigenschaften deklariert ein Protokoll von der Klasse implementiert werden muss, die übernommen. Stattdessen sind einige der Methoden und Eigenschaften optional.

Einige Protokolle werden im Allgemeinen als Modellklassen verwendet, die gebunden werden sollte mit dem Modell-Attribut.

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

Beginnend mit MonoTouch 7.0 einen neuen und verbesserten protokollbindung-Funktionalität wurde integriert.  Jede Definition, enthält die `[Protocol]` Attribut generiert tatsächlich drei unterstützende Klassen, die die Möglichkeit, die Sie Protokolle verwenden erheblich zu verbessern:

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

Die **klassenimplementierung** bietet eine vollständige abstrakte Klasse, dass Sie einzelne Methoden überschreiben und vollständige typsicherheit abrufen können. Jedoch aufgrund von c# mehrfache Vererbung nicht unterstützt, es gibt Szenarien, in denen Sie möglicherweise erfordern eine andere Basisklasse, aber immer noch eine Schnittstelle implementieren möchten.

Dies ist, wenn die generierte **-Schnittstellendefinition** eingeht.  Es ist eine Schnittstelle, die erforderlichen Methoden aus dem Protokoll aufweist.  Dadurch können Entwickler, die Ihr Protokoll zum lediglich Implementieren der Schnittstelle implementieren möchten.  Die Laufzeit wird den Typ als Einführung des Protokolls automatisch registriert.

Beachten Sie, dass die Schnittstelle nur die erforderlichen Methoden aufgelistet und die optionalen Methoden macht.   Dies bedeutet, dass Klassen, die das Protokoll übernimmt erhalten vollständige typüberprüfung für die erforderlichen Methoden, jedoch Sie hierfür auf schwache Typisierung (manuell ExportAttribute und entspricht der Signatur müssen) für die optionale Protokollmethoden.

Zum erleichtern die eine API nutzen, die Protokolle verwendet, wird das Tool für die Bindung auch eine Erweiterungen Methode Klasse erstellen, die alle die optionalen Methoden verfügbar macht.   Dies bedeutet, dass so lange Sie eine API nutzen, um Protokolle zu behandeln, als dass alle Methoden werden kann.

Wenn Sie die Definitionen des Protokolls in Ihrer API verwenden möchten, müssen Sie das Gerüst eines leere Schnittstellen definieren Sie in der API schreiben.   Wenn Sie die MyProtocol in einer API verwenden möchten, müssen Sie dazu:

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

Oben ist erforderlich, da zur Bindungszeit der `IMyProtocol` ist nicht vorhanden, d. h. Warum müssen Sie eine leere Schnittstelle bereitstellen.

### <a name="adopting-protocol-generated-interfaces"></a>Protokoll generierten Schnittstellen eingeführt

Wenn Sie eine der Schnittstellen, die für die Protokolle, wie folgt generiert implementieren:

```csharp
class MyDelegate : NSObject, IUITableViewDelegate {
    nint IUITableViewDelegate.GetRowHeight (nint row) {
        return 1;
    }
}
```

Die Implementierung für die Schnittstellenmethoden ruft automatisch mit dem richtigen Namen exportiert, damit sie dies entspricht:

```csharp
class MyDelegate : NSObject, IUITableViewDelegate {
    [Export ("getRowHeight:")]
    nint IUITableViewDelegate.GetRowHeight (nint row) {
        return 1;
    }
}
```

Es spielt keine Rolle, wenn die Schnittstelle, implizit oder explizit implementiert wird.

### <a name="protocol-inlining"></a>Protokoll inlining

Während Sie vorhandene Objective-C-Typen, die binden als ein Protokoll einführen deklariert worden sein, empfiehlt um Inline des Protokolls direkt. Zu diesem Zweck als Schnittstelle ohne Ihr Protokoll lediglich deklarieren `[BaseType]` -Attribut und das Protokoll in der Liste der für die Schnittstelle die Basisschnittstellen auflisten.

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

Die Attribute in diesem Abschnitt gelten für einzelne Member eines Typs: Eigenschaften und Methodendeklarationen.


### <a name="alignattribute"></a>AlignAttribute

Dient zum Angeben der Ausrichtungswert für Eigenschaftentypen zurück. Bestimmte Eigenschaften nehmen Zeiger auf die Adressen, die an bestimmte-Grenzen ausgerichtet sein müssen (im Xamarin.iOS hierfür ist, z. B. mit einigen `GLKBaseEffect` Eigenschaften, die 16-Byte zu Transportservers ausgerichtet). Sie können mit dieser Eigenschaft können Sie ergänzen die Getter und verwenden Sie den Ausrichtungswert. Dies wird normalerweise verwendet, mit der `OpenTK.Vector4` und `OpenTK.Matrix4` -Typen in Objective-C-APIs integriert.

Beispiel:

```csharp
public interface GLKBaseEffect {
    [Export ("constantColor")]
    Vector4 ConstantColor { [Align (16)] get; set;  }
}
```


### <a name="appearanceattribute"></a>AppearanceAttribute

Die `Appearance` -Attribut ist beschränkt auf iOS5, in dem der Darstellung Manager eingeführt wurde.

Die `Appearance` Attribut kann angewendet werden, um eine beliebige Methode oder Eigenschaft, die Bestandteil der `UIAppearance` Framework. Wenn dieses Attribut auf eine Methode oder Eigenschaft in einer Klasse angewendet wird, weist er den Generator Bindung eine Darstellung stark typisierte Klasse zu erstellen, die zum Formatieren von allen Instanzen dieser Klasse oder Instanzen, die bestimmte Kriterien erfüllen, verwendet wird.

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

Die oben aufgeführten würde durch folgenden Code in UIToolbar generiert:

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

Verwenden der `AutoReleaseAttribute` auf Methoden und Eigenschaften zum Umschließen des Aufrufs an die Methode in einer `NSAutoReleasePool`.

In Objective-C stehen einige Methoden, die Werte zurückgeben, die auf den Standardwert hinzugefügt werden `NSAutoReleasePool`. Standardmäßig würde diese an Ihr Thread wechseln `NSAutoReleasePool`, da Xamarin.iOS auch über einen Verweis auf die Objekte bleiben, solange das verwaltete Objekt aktiv ist, Sie möchten jedoch möglicherweise keinen zusätzlichen Verweis im beibehalten der `NSAutoReleasePool` der werden nur bis zu dem Thread entladen abrufen Gibt die Steuerung an die nächste Thread, oder Sie zurück auf die Hauptschleife.

Dieses Attribut gilt z. B. auf hoher Eigenschaften (z. B. `UIImage.FromFile`), die Objekte, die auf den Standardwert hinzugefügt wurden zurückgibt `NSAutoReleasePool`. Ohne dieses Attribut würde die Bilder beibehalten werden, solange der Thread kein Steuerelement an die Hauptschleife zurück. Uf dem Thread wurde irgendeine Hintergrund-Downloadprogramm immer aktiv ist und von Arbeit wartet, die Bilder würden nie freigegeben werden.

### <a name="forcedtypeattribute"></a>ForcedTypeAttribute

Die `ForcedTypeAttribute` wird verwendet, um die Erstellung eines verwalteten Typs zu erzwingen, auch wenn das zurückgegebene nicht verwaltete Objekt nicht in die Bindungsdefinition beschriebenen Typs übereinstimmt.

Dies ist nützlich, wenn der Typ beschrieben, die in einem Header nicht mit den zurückgegebenen Typ, der die systemeigene Methode übereinstimmt, akzeptieren Sie z. B. die folgende Objective-C-Definition aus `NSURLSession`:

`- (NSURLSessionDownloadTask *)downloadTaskWithRequest:(NSURLRequest *)request`

Es deutlich gibt an, dass es zurückgegeben wird ein `NSURLSessionDownloadTask` Instanz, jedoch noch es **gibt** eine `NSURLSessionTask`, also eine übergeordnete Klasse und daher nicht konvertiert werden kann, `NSURLSessionDownloadTask`. Da wir in einem Kontext als typsicherer sind ein `InvalidCastException` erfolgt.

Die Beschreibung der Header entsprechen, und vermeiden der `InvalidCastException`, die `ForcedTypeAttribute` verwendet wird.

```csharp
[BaseType (typeof (NSObject), Name="NSURLSession")]
interface NSUrlSession {

    [Export ("downloadTaskWithRequest:")]
    [return: ForcedType]
    NSUrlSessionDownloadTask CreateDownloadTask (NSUrlRequest request);
}
```

Die `ForcedTypeAttribute` auch akzeptiert einen booleschen Wert, der mit dem Namen `Owns` also `false` standardmäßig `[ForcedType (owns: true)]`. Der Parameter wird verwendet, um die Folgen der Besitzer ist die [Besitz Richtlinie](https://developer.apple.com/library/content/documentation/CoreFoundation/Conceptual/CFMemoryMgmt/Concepts/Ownership.html) für **Core Foundation** Objekte.

Die `ForcedTypeAttribute` gilt nur für `parameters`, `properties` und `return value`.

### <a name="bindasattribute"></a>BindAsAttribute

Die `BindAsAttribute` ermöglicht die Bindung `NSNumber`, `NSValue` und `NSString`(Enumerationen) in genauere C#-Typen. Das Attribut kann verwendet werden, erstellen Sie eine bessere und genauere .NET API gegenüber einer systemeigenen API.

Sie können Methoden (Rückgabewert), Parameter und Eigenschaften mit ergänzen `BindAs`. Die einzige Einschränkung besteht Ihr Mitglied ist, **muss nicht** werden innerhalb einer `[Protocol]` oder `[Model]` Schnittstelle.

Zum Beispiel:

```csharp
[return: BindAs (typeof (bool?))]
[Export ("shouldDrawAt:")]
NSNumber ShouldDraw ([BindAs (typeof (CGRect))] NSValue rect);
```

Ausgabe:

```csharp
[Export ("shouldDrawAt:")]
bool? ShouldDraw (CGRect rect) { ... }
```

Intern werden wir führen die `bool?`  <->  `NSNumber` und `CGRect`  <->  `NSValue` Konvertierungen.

Die aktuelle Kapselung unterstützten Typen sind:

* `NSValue`
* `NSNumber`
* `NSString`

#### <a name="nsvalue"></a>NSValue

Die folgenden C#-Datentypen werden unterstützt, um gekapselt werden, von/in `NSValue`:

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

Die folgenden C#-Datentypen werden unterstützt, um gekapselt werden, von/in `NSNumber`:

* bool
* byte
* double
* float
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

`[BindAs]` funktioniert in Conjuntion mit [Enumerationen durch eine Konstante NSString gesichert](#enum-attributes) , damit Sie besser .NET API, z. B. erstellen können:

```csharp
[BindAs (typeof (CAScroll))]
[Export ("supportedScrollMode")]
NSString SupportedScrollMode { get; set; }
```

Ausgabe:

```csharp
[Export ("supportedScrollMode")]
CAScroll SupportedScrollMode { get; set; }
```

Behandelt die `enum`  <->  `NSString` Konvertierung nur dann, wenn der bereitgestellten Enumerationstyp, `[BindAs]` ist [durch eine Konstante NSString gesichert](#enum-attributes).

#### <a name="arrays"></a>Arrays

`[BindAs]` unterstützt auch Arrays eines der unterstützten Typen können Sie als Beispiel die folgende API-Definition haben:

```csharp
[return: BindAs (typeof (CAScroll []))]
[Export ("getScrollModesAt:")]
NSString [] GetScrollModes ([BindAs (typeof (CGRect []))] NSValue [] rects);
```

Ausgabe:

```csharp
[Export ("getScrollModesAt:")]
CAScroll? [] GetScrollModes (CGRect [] rects) { ... }
```

Die `rects` Parameter gekapselt in einer `NSArray` , enthält eine `NSValue` für jede `CGRect` und im Gegenzug erhalten Sie ein Array von `CAScroll?` die wurde erstellt unter Verwendung der Werte der zurückgegebenen `NSArray` mit `NSStrings`.

### <a name="bindattribute"></a>BindAttribute

Die `Bind` Attribut verfügt über zwei verwendet bei Anwendung auf eine Methode oder Eigenschaftendeklaration, und eine andere bei Anwendung auf die einzelne Getter oder Setter-Methode in einer Eigenschaft.

Wenn für eine Methode oder Eigenschaft verwendet wird, ist die Auswirkung der Bind-Attribut generieren eine Methode, die den angegebenen Selektor aufruft. Aber die resultierende generierte Methode ist nicht mit ergänzt die `[Export]` -Attribut, das bedeutet, dass es keine Methode überschreiben beteiligt sein kann. Dies wird normalerweise verwendet, in Kombination mit der `Target` Attribut für die Implementierung Objective-C-Erweiterungsmethoden.

Zum Beispiel:

```csharp
public interface UIView {
    [Bind ("drawAtPoint:withFont:")]
    SizeF DrawString ([Target] string str, CGPoint point, UIFont font);
}
```

Wenn Sie in einen Getter oder Setter, verwendet die `Bind` Attribut wird verwendet, um die Standardwerte, die vom Code-Generator abgeleitet werden, während der Generierung der Getter und Setter Objective-C-Auswahlnamen für eine Eigenschaft zu ändern. Wenn Sie eine Eigenschaft mit dem Namen "FooBar" kennzeichnen der Generator würde generiert standardmäßig einen Export "FooBar" für die Getter und "SetFooBar:" für den Setter. In einigen Fällen Objective-C ist nicht entsprechen dieser Konvention, in der Regel ändern sie den Getter-Namen "IsFooBar" sein.
Sie würden dieses Attribut verwenden, um den Generator dieses zu informieren.

Zum Beispiel:

```csharp
// Default behavior
[Export ("active")]
bool Active { get; set; }

// Custom naming with the Bind attribute
[Export ("visible")]
bool Visible { [Bind ("isVisible")] get; set; }
```


### <a name="asyncattribute"></a>AsyncAttribute

Nur auf Xamarin.iOS 6.3 und höher verfügbar.

Dieses Attribut kann auf Methoden angewendet werden, die eine Abschlusshandler als ihre letzte Argument annehmen.

Sie können die `[Async]` Attribut auf Methoden, deren letzte Argument ein Rückruf ist.  Wenn Sie diese an eine Methode angewendet wird, generiert der Bindung-Generator eine Version dieser Methode mit dem Suffix `Async`.  Wenn der Rückruf keine Parameter akzeptiert, wird der Rückgabewert eine `Task`, wenn der Rückruf auf einen Parameter verwendet, wird das Ergebnis einer Aufgabe werden&lt;T&gt;.

```csharp
[Export ("upload:complete:")]
[Async]
void LoadFile (string file, NSAction complete)
```

Im folgenden wird diese Async-Methode generiert:

```csharp
Task LoadFileAsync (string file);
```

Wenn der Rückruf mehrere Parameter akzeptiert, legen Sie die `ResultType` oder `ResultTypeName` die gewünschten Namen des generierten Typs an, der alle Eigenschaften enthalten soll.

```csharp
delegate void OnComplete (string [] files, nint byteCount);

[Export ("upload:complete:")]
[Async (ResultTypeName="FileLoading")]
void LoadFiles (string file, OnComplete complete)
```

Im folgenden werden diese Async-Methode generiert, in denen `FileLoading` enthält Eigenschaften, um den Zugriff auf "Dateien" und "ByteCount":

```csharp
Task<FileLoading> LoadFile (string file);
```

Wenn der letzte Parameter des Rückrufs ist ein `NSError`, klicken Sie dann auf die generierten `Async` Methode überprüft, ob der Wert nicht null, und wenn dies der Fall ist, wird die generierten asynchronen Methode die Ausnahme einer Aufgabe festgelegt.

```csharp
[Export ("upload:onComplete:")]
[Async]
void Upload (string file, Action<string,NSError> onComplete);
```

Die oben genannten generiert die folgende Async-Methode:

```csharp
Task<string> UploadAsync (string file);
```

Und die resultierende Aufgabe hat bei einem Fehler, die Ausnahme, die zum Festlegen einer `NSErrorException` , umschließt der resultierende `NSError`.

#### <a name="asyncattributeresulttype"></a>AsyncAttribute.ResultType

Verwenden Sie diese Eigenschaft zum Angeben des Werts für die Rückgabe `Task` Objekt.   Dieser Parameter weist einen bereits vorhandener Typ, daher muss es auf eine der Definitionen api Core definiert werden.

#### <a name="asyncattributeresulttypename"></a>AsyncAttribute.ResultTypeName

Verwenden Sie diese Eigenschaft zum Angeben des Werts für die Rückgabe `Task` Objekt.   Dieser Parameter weist den Namen der gewünschten Typ, der Generator eine Reihe von Eigenschaften dar, eine für jeden Parameter, die der Rückruf akzeptiert.

#### <a name="asyncattributemethodname"></a>AsyncAttribute.MethodName

Verwenden Sie diese Eigenschaft zum Anpassen des Namens der generierten Async-Methoden.   Die Standardeinstellung ist, verwenden Sie den Namen der Methode, und fügen Sie den Text "Async", Sie können Hiermit können Sie diese Standardeinstellung ändern.

### <a name="disablezerocopyattribute"></a>DisableZeroCopyAttribute

Dieses Attribut String-Parameter oder Eigenschaften angewendet wird und weist den Codegenerator, die NULL-kopieren-Zeichenfolge, die Marshalling für diesen Parameter nicht verwenden und stattdessen eine neue Instanz der NSString aus der C#-Zeichenfolge erstellen.
Dieses Attribut ist nur für Zeichenfolgen erforderlich, wenn Sie den Generator verwenden, Kopieren von 0 (null) Marshalling entweder anweisen, die `--zero-copy` Befehlszeilenoption oder das Attribut auf Assemblyebene festlegen `ZeroCopyStringsAttribute`.

Dies ist erforderlich, in Fällen, in dem die Eigenschaft wird, in Objective-C, um deklariert, eine Eigenschaft "beibehalten" oder "zuweisen" statt einer Objekteigenschaft "Kopieren" werden. Diese wird in der Regel in Bibliotheken der Drittanbieter, die fälschlicherweise "von Entwicklern dahingehend optimiert wurden," auftreten. Im Allgemeinen "beibehalten" oder "zuweisen" `NSString` / sind falsch, da `NSMutableString` oder Benutzer abgeleitete Klassen von `NSString` möglicherweise den Inhalt der Zeichenfolgen ohne Kenntnis des Bibliothekscodes, unterbrechen die Anwendung leicht ändern. Dies geschieht in der Regel aufgrund von vorzeitige Optimierung.

Das folgende Beispiel zeigt zwei solche Eigenschaften in Objective-c:

```csharp
@property(nonatomic,retain) NSString *name;
@property(nonatomic,assign) NSString *name2;
```


### <a name="disposeattribute"></a>DisposeAttribute

Beim Anwenden der `DisposeAttribute` auf eine Klasse, die Sie Bereitstellen eines Codeausschnitts, die hinzugefügt werden, die `Dispose()` methodenimplementierung der-Klasse.

Da die `Dispose` Methode wird automatisch generiert, indem die `bmac-native` und `btouch-native` Tools Sie verwenden müssen die `Dispose` Attribut zum Einfügen von Code in der generierten `Dispose` methodenimplementierung.

Zum Beispiel:

```csharp
[BaseType (typeof (NSObject))]
[Dispose ("if (OpenConnections > 0) CloseAllConnections ();")]
interface DatabaseConnection {
}
```


### <a name="exportattribute"></a>ExportAttribute

Die `Export` Attribut wird verwendet, um das flag einer Methode oder Eigenschaft für Objective-C-Laufzeit verfügbar gemacht werden. Dieses Attribut ist das Tool für die Bindung und die tatsächliche Xamarin.iOS und Xamarin.Mac Laufzeiten freigegeben. Für Methoden, die Parameter wird übergeben wörtliche an den generierten Code für Eigenschaften, einen Getter und Setter Exporte auf Grundlage der Basisdeklaration generiert werden (finden Sie im Abschnitt für die `BindAttribute` Informationen zum Ändern des Verhaltens von der Bindung-Tool).

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

Die [Selektor](http://developer.apple.com/library/ios/#documentation/cocoa/conceptual/objectivec/Chapters/ocSelectors.html) und der zugrunde liegenden Objective-C-Name der Methode oder Eigenschaft, die gebunden wird, darstellt.


#### <a name="exportattributeargumentsemantic"></a>ExportAttribute.ArgumentSemantic


### <a name="fieldattribute"></a>FieldAttribute

Dieses Attribut wird verwendet, eine globale Variable für C# als ein Feld verfügbar machen, die bei Bedarf geladen wird und für C#-Code verfügbar gemacht. Dies ist normalerweise erforderlich, um die Werte der Konstanten erhalten, in C oder Objective-C definiert sind und entweder einige APIs verwendeten Token sein kann, oder, deren Werte nicht transparent und müssen als verwendet werden – durch Benutzercode ist.

Syntax:

```csharp
public class FieldAttribute : Attribute {
    public FieldAttribute (string symbolName);
    public FieldAttribute (string symbolName, string libraryName);
    public string SymbolName { get; set; }
    public string LibraryName { get; set; }
}
```

Die `symbolName` ist der C#-Symbol zu verknüpfen. Standardmäßig wird dies über eine Bibliothek, deren Name aus dem Namespace abgeleitet wird, geladen, in dem der Typ definiert ist. Wenn dies nicht der Bibliothek ist, in dem das Symbol nachgeschlagen wird, übergeben Sie die `libraryName` Parameter. Wenn Sie eine statische Bibliothek verknüpfen möchten, verwenden Sie "__Internal" als die `libraryName` Parameter.

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

Settern für Eigenschaften können nicht für [Enumerationen durch Konstanten NSString gesichert](#enum-attributes), aber sie können manuell gebunden werden bei Bedarf.

Beispiel:

```csharp
[Static]
interface CameraEffects {
     [Field ("kCameraEffectsZoomFactorKey", "CameraLibrary")]
     NSString ZoomFactorKey { get; }
}
```

### <a name="internalattribute"></a>InternalAttribute

Die `Internal` Attribut kann auf Methoden oder Eigenschaften angewendet werden, und sie wirkt sich das kennzeichnen Sie den generierten Code mit dem "intern" c#-Schlüsselwort, den Code in der generierten Assembly nur für Code zugänglich machen. Dies dient normalerweise zum Ausblenden von APIs, die zu auf niedriger Ebene sind, oder geben Sie eine nicht optimalen öffentliche API, die zur Verbesserung der nach oder für APIs, die von den Generator nicht unterstützt werden sollen, und erfordern einige handcodierung von.

Wenn Sie die Bindung entwerfen, würden Sie in der Regel auszublenden, die Methode oder Eigenschaft, die Verwendung dieses Attributs und geben Sie einen anderen Namen für die Methode oder Eigenschaft und fügen Sie dann auf Ihrem C#-ergänzende Unterstützungsdatei, einen stark typisierten Wrapper, der verfügbar macht die Funktionen zugrunde liegen.

Zum Beispiel:

```csharp
[Internal]
[Export ("setValue:forKey:");
void _SetValueForKey (NSObject value, NSObject key);

[Internal]
[Export ("getValueForKey:")]
NSObject _GetValueForKey (NSObject key);
```

Anschließend wird in der Datei unterstützenden Code wie haben kann:

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

### <a name="isthreadstaticattribute"></a>IsThreadStaticAttribute

Dieses Attribut kennzeichnet das dahinter liegende Feld für eine Eigenschaft an, mit der Anmerkung versehen sein `[ThreadStatic]` Attribut. Dies ist hilfreich, wenn das Feld eine statische Thread-Variable ist.

### <a name="marshalnativeexceptions-xamarinios-606"></a>MarshalNativeExceptions (Xamarin.iOS 6.0.6)

Dieses Attribut wird eine Methode Unterstützung native (ObjectiveC) Ausnahmen stellen.
Statt `objc_msgSend` direkt, geht der Aufruf über eine benutzerdefinierte Trampoline ObjectiveC Ausnahmen abfängt und marshallt diese in verwaltete Ausnahmen.

Zurzeit nur wenige `objc_msgSend` Signaturen werden unterstützt (Sie werden feststellen, ob eine Signatur nicht unterstützt wird, wenn ein Fehler und eine fehlende Monotouch_ native Verknüpfen einer App, die die Bindung verwendet auftritt*_objc_msgSend* Symbol), jedoch kann mehr auf Anforderung hinzugefügt.


### <a name="newattribute"></a>NewAttribute

Dieses Attribut gilt für Methoden und Eigenschaften für den Generator, das "new"-Schlüsselwort vor der Deklaration generiert haben.

Es wird verwendet, um Compiler-Warnungen zu vermeiden, wenn die dieselbe Methode oder den Eigenschaftennamen in einer Unterklasse eingeleitet wird, die bereits in einer Basisklasse vorhanden waren.


### <a name="notificationattribute"></a>NotificationAttribute

Sie können dieses Attribut auf Felder, die der Generator erzeugen eine stark typisierte Hilfsprogramm Benachrichtigungen Klasse anwenden.

Dieses Attribut kann verwendet werden, ohne Argumente für Benachrichtigungen, die keine Nutzlast enthalten, oder Sie können angeben, eine `System.Type` , die auf einer anderen Schnittstelle in der API-Definition, in der Regel verweist, mit dem Namen mit "EventArgs" endet. Der Generator wandeln der Schnittstelle in einer Klasse, Unterklassen `EventArgs` und schließt alle aufgeführten Eigenschaften. Die `[Export]` Attribut sollte verwendet werden, der `EventArgs` Klasse den Namen des Schlüssels gesucht, das Objective-C-Wörterbuch, das den Wert abzurufen.

Zum Beispiel:

```csharp
interface MyClass {
    [Notification]
    [Field ("MyClassDidStartNotification")]
    NSString DidStartNotification { get; }
}
```

Der obige Code generiert eine geschachtelte Klasse `MyClass.Notifications` mit den folgenden Methoden:

```csharp
public class MyClass {
   [..]
   public Notifications {
      public static NSObject ObserveDidStart (EventHandler<NSNotificationEventArgs> handler)
      public static NSObject ObserveDidStart (NSObject objectToObserve, EventHandler<NSNotificationEventArgs> handler)
   }
}
```

Benutzer Ihres Codes können dann problemlos Abonnieren von Benachrichtigungen, die veröffentlicht werden, um die [NSDefaultCenter](https://developer.xamarin.com/api/property/Foundation.NSNotificationCenter.DefaultCenter/) mithilfe von Code wie folgt:

```csharp
var token = MyClass.Notifications.ObserverDidStart ((notification) => {
    Console.WriteLine ("Observed the 'DidStart' event!");
});
```

Oder festlegen ein bestimmtes Objekts beobachten. Wenn Sie übergeben `null` auf `objectToObserve` diese Methode verhält sich genau wie die anderen Peer.

```csharp
var token = MyClass.Notifications.ObserverDidStart (objectToObserve, (notification) => {
    Console.WriteLine ("Observed the 'DidStart' event on objectToObserve!");
});
```

Der zurückgegebene Wert von `ObserveDidStart` können verwendet werden, um problemlos den Empfang von Benachrichtigungen, wie folgt:

```csharp
token.Dispose ();
```

Oder Sie rufen [NSNotification.DefaultCenter.RemoveObserver](https://developer.xamarin.com/api/member/Foundation.NSNotificationCenter.RemoveObserver/p/Foundation.NSObject//) und übergeben Sie das Token. Wenn Ihre Benachrichtigung Parameter enthält, müssen Sie angeben, dass eine Hilfsprogramm `EventArgs` -Schnittstelle, wie folgt:

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

Oben generiert eine `MyScreenChangedEventArgs` -Klasse mit der `ScreenX` und `ScreenY` Eigenschaften, die die Daten aus abzurufen, werden die [NSNotification.UserInfo](https://developer.xamarin.com/api/property/Foundation.NSNotification.UserInfo/) Wörterbuch mit Schlüsseln **ScreenXKey** und **ScreenYKey** bzw. und die richtigen Konvertierungen gelten. Die `[ProbePresence]` -Attributs für den Generator Prüfpunkt, wenn der Schlüssel, in festgelegt ist der `UserInfo`, anstatt zu versuchen, den Wert zu extrahieren. Dies ist für Fälle verwendet, in denen das Vorhandensein des Schlüssels der Wert (in der Regel für boolesche Werte) ist.

Dadurch können Sie Code wie folgt schreiben:

```csharp
var token = MyClass.NotificationsObserveScreenChanged ((notification) => {
    Console.WriteLine ("The new screen dimensions are {0},{1}", notification.ScreenX, notification.ScreenY);
});
```

In einigen Fällen müssen Sie keine Konstante zugeordneten auf das Wörterbuch übergebene Wert vorhanden ist.  Apple manchmal verwendet öffentlichen Symboldateien Konstanten und Zeichenfolgenkonstanten in einigen Fällen verwendet.  Standardmäßig die `[Export]` Attribut in der vorgesehenen `EventArgs` Klasse wird den angegebenen Namen als öffentliche Symbol verwenden, zur Laufzeit gesucht werden soll.  Wenn dies nicht der Fall ist, und stattdessen sollte als Zeichenfolgenkonstante nachgeschlagen werden, und übergeben der `ArgumentSemantic.Assign` Wert zum Export-Attribut.

**Neues in Xamarin.iOS 8.4**

In manchen Fällen Benachrichtigungen beginnt Lebensdauer ohne Argumente, daher die Verwendung von `[Notification]` ohne Argumente zulässig ist.  Aber in manchen Fällen werden Parameter für die Benachrichtigung eingeleitet werden.  Um dieses Szenario zu unterstützen, kann das Attribut mehrmals angewendet werden.

Wenn Sie eine Bindung entwickeln, und zu vermeiden, dass vorhandene Benutzercode werden sollen, aktivieren Sie eine vorhandene Benachrichtigung aus:

```csharp
interface MyClass {
    [Notification]
    [Field ("MyClassScreenChangedNotification")]
    NSString ScreenChangedNotification { get; }
}
```

In eine Version, die die Benachrichtigung Attribut zweimal wie folgt aufgeführt:

```csharp
interface MyClass {
    [Notification]
    [Notification (typeof (MyScreenChangedEventArgs)]
    [Field ("MyClassScreenChangedNotification")]
    NSString ScreenChangedNotification { get; }
}
```

### <a name="nullallowedattribute"></a>NullAllowedAttribute

Wenn dies auf eine Eigenschaft angewendet wird kennzeichnet es die Eigenschaft als den Wert null zugewiesen werden können. Dies gilt nur für Verweistypen.

Wenn dies auf einen Parameter in einer Methodensignatur angewendet wird gibt es an, dass der angegebene Parameter null sein kann und keine Überprüfung durchgeführt werden soll, für die Übergabe von null-Werte.

Der Verweistyp nicht über dieses Attribut verfügt, die Bindung Tool generiert eine Überprüfung auf den Wert vor der Übergabe an Objective-C zugewiesen wird, und generiert eine Überprüfung, die ausgelöst wird ein `ArgumentNullException` , wenn der zugewiesene Wert null ist.

Zum Beispiel:

```csharp
// In properties

[NullAllowed]
UIImage IconFile { get; set; }

// In methods
void SetImage ([NullAllowed] UIImage image, State forState);
```

<a name="OverrideAttribute"/>

### <a name="overrideattribute"></a>OverrideAttribute

Verwenden Sie dieses Attribut, um die Bindung Generator anzuweisen, dass die Bindung für diese bestimmte Methode mit einem Schlüsselwort "Override" gekennzeichnet werden soll.


### <a name="presnippetattribute"></a>PreSnippetAttribute

Sie können dieses Attribut verwenden, zum Einfügen von Code eingefügt werden, nachdem die Eingabeparameter überprüft wurden, aber noch der Code ruft in Objective-C

Beispiel:

```csharp
[Export ("demo")]
[PreSnippet ("var old = ViewController;")]
void Demo ();
```


### <a name="prologuesnippetattribute"></a>PrologueSnippetAttribute

Zum Einfügen von Code eingefügt werden, bevor Sie einen der Parameter in die generierte Methode überprüft werden, können Sie dieses Attribut verwenden.

Beispiel:

```csharp
[Export ("demo")]
[Prologue ("Trace.Entry ();")]
void Demo ();
```


### <a name="postgetattribute"></a>PostGetAttribute

Weist den Generator Bindung zum Aufrufen der angegebenen Eigenschaft aus dieser Klasse, die einen Wert daraus abzurufen.

Diese Eigenschaft wird normalerweise verwendet, um den Cache zu aktualisieren, der auf Verweisobjekte, das Beibehalten des Objektdiagramms verweist, auf die verwiesen wird. In der Regel wird es im Code mit, wie z. B. hinzufügen/entfernen. Diese Methode wird verwendet, sodass nach Elemente hinzugefügt oder entfernt werden, dass die interne Cache aktualisiert werden, um sicherzustellen, dass wir verwaltete Verweise auf Objekte ordnungsgemäß ausgeführt werden, die tatsächlich in Verwendung sind. Dies ist möglich, da die Bindung-Tool eine dahinter liegende Feld für die Verweisobjekte, die alle in einer angegebenen Bindung erstellt.

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

In diesem Fall die `Dependencies` Eigenschaft wird aufgerufen, die nach dem Hinzufügen oder Entfernen von Abhängigkeiten von der `NSOperation` -Objekt, um sicherzustellen, dass wir verfügen über ein Diagramm, das den tatsächlichen darstellt geladen, verhindert sowohl von Arbeitsspeicherverlusten als auch Speicherschäden.


### <a name="postsnippetattribute"></a>PostSnippetAttribute

Sie können dieses Attribut verwenden, zum Einfügen von einigen C#-Quellcode eingefügt werden, nachdem der Code die zugrunde liegende Objective-C-Methode aufgerufen hat

Beispiel:

```csharp
[Export ("demo")]
[PostSnippet ("if (old != null) old.DemoComplete ();")]
void Demo ();
```


### <a name="proxyattribute"></a>ProxyAttribute

Dieses Attribut wird zur Rückgabe von Werten, um sie als Proxyobjekte flag angewendet. Einige Objective-C-APIs zurückgegeben Proxy-Objekte, die nicht vom Benutzer Bindungen unterschieden werden können. Die Auswirkungen dieses Attributs ist so kennzeichnen Sie das Objekt als eine `DirectBinding` Objekt. Für ein Szenario in Xamarin.Mac, sehen Sie die [detaillierte Informationen zu diesem Fehler](https://bugzilla.novell.com/show_bug.cgi?id=670844).


### <a name="retainlistattribute"></a>RetainListAttribute

Weist den Generators zum Beibehalten eines verwalteten Verweis auf den Parameter oder entfernen einen internen Verweis auf den Parameter an. Dies wird verwendet, auf die verwiesen wird Objekte zu halten.

Syntax:

```csharp
public class RetainListAttribute: Attribute {
     public RetainListAttribute (bool doAdd, string listName);
}
```

Wenn der Wert von "DoAdd" ist "true", der Parameter hinzugefügt, um die `__mt_{0}_var List<NSObject>;`. Auf dem `{0}` wird ersetzt, mit der angegebenen `listName`. Diese dahinter liegende Feld müssen in Ihre ergänzende Teilklasse an die API deklariert werden.

Ein Beispiel finden Sie unter [foundation.cs](https://github.com/mono/maccore/blob/master/src/foundation.cs) und [NSNotificationCenter.cs](https://github.com/mono/maccore/blob/master/src/Foundation/NSNotificationCenter.cs)

### <a name="releaseattribute-xamarinios-60"></a>ReleaseAttribute (Xamarin.iOS 6.0)

Dies kann angewendet werden, um die Rückgabetypen, um anzugeben, dass der Generator aufrufen sollte `Release` für das Objekt vor der Rückgabe. Dies ist nur erforderlich, wenn eine Methode stellt Ihnen ein Objekt zur beibehalten (im Gegensatz zu einer Autoreleased Objekt, das das häufigste Szenario)

Beispiel:

```csharp
[Export ("getAndRetainObject")]
[return: Release ()]
NSObject GetAndRetainObject ();
```

Darüber hinaus wird dieses Attribut auf den generierten Code weitergegeben, damit die Laufzeit Xamarin.iOS weiß, dass das Objekt bei der Rückkehr zum Objective-C aus einer solchen Funktion beibehalten werden muss.


### <a name="sealedattribute"></a>SealedAttribute

Weist den Generator, um die generierte Methode als versiegelt zu kennzeichnen. Wenn dieses Attribut nicht angegeben wird, ist die Standardeinstellung zum Generieren einer virtuellen Methode (entweder eine virtuelle Methode, eine abstrakte Methode oder eine Überschreibung, je nachdem wie andere Attribute verwendet werden).


### <a name="staticattribute"></a>StaticAttribute

Wenn die `Static` Attribut angewendet wird an eine Methode oder Eigenschaft einer statischen Methode oder Eigenschaft generiert. Wenn dieses Attribut nicht angegeben ist, erzeugt der Generator eine Instanzmethode oder Eigenschaft.


### <a name="transientattribute"></a>TransientAttribute

Verwenden Sie dieses Attribut auf Flageigenschaften, deren Werte vorübergehend ist, d. h. Objekte, die von iOS vorübergehend erstellt werden, jedoch sind nicht besonders langlebig. Wenn dieses Attribut auf eine Eigenschaft angewendet wird, erstellt der Generator keine dahinter liegende Feld für diese Eigenschaft, was bedeutet, dass die verwaltete Klasse einen Verweis auf das Objekt nicht Schritt halten kann.


### <a name="wrapattribute"></a>WrapAttribute

In den Entwurf der Xamarin.iOS/Xamarin.Mac Bindungen die `Wrap` Attribut wird verwendet, um eine schwach typisierte Objekt mit einem stark typisierten Objekt zu umschließen. Dies kommt hauptsächlich mit Objective-C "delegieren"-Objekte, die in der Regel als Typ deklariert sind `id` oder `NSObject`. Die Konvention von Xamarin.iOS und Xamarin.Mac verwendet wird, diese Delegaten oder Datenquellen als Typ verfügbar zu machen `NSObject` und mit der Konvention "Weak" + den verfügbar gemachten Namen benannt. Ein "Id" delegateigenschaften von Objective-C würde verfügbar gemacht werden, als ein `NSObject WeakDelegate { get; set; }` Eigenschaft in der API-Vertrags-Datei.

In der Regel der Wert, der für diesen Delegaten zugewiesen wird weist jedoch einen starken Typ, damit wir den starken Typ Oberfläche, und wenden die `Wrap` -Attribut, das bedeutet, dass Benutzer auswählen können, die unsichere Typen verwenden, ggf. einige Fine-Steuerelement, oder wenn sie auf niedriger Ebene Tric verwenden müssen KS, oder sie können die stark typisierte Eigenschaft für die meisten ihrer Arbeit verwenden.

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

Und dies wird wie der Benutzer wird die stark typisierte Version verwenden, beachten Sie, dass der Benutzer von # Typsystem profitiert und verwendet das Override-Schlüsselwort, um seine Absicht deklarieren und, dass er nicht auf manuell ergänzen die Methode mit `Export`, da wir geschehen Arbeiten Sie in der Bindung für den Benutzer:

```csharp
// This is the strong case,
class MyDelegate : DemoDelegate {
   override void Demo DoDemo () {}
}

var strongDemo = new Demo ();
demo.Delegate = new MyDelegate ();
```


Eine weitere Verwendungsmöglichkeit von der `Wrap` Attribut ist, um stark typisierte Version Methoden zu unterstützen.   Zum Beispiel:

```csharp
[BaseType (typeof (NSObject))]
interface XyzPanel {
    [Export ("playback:withOptions:")]
    void Playback (string fileName, [NullAllowed] NSDictionary options);

    [Wrap ("Playback (fileName, options == null ? null : options.Dictionary")]
    void Playback (string fileName, XyzOptions options);
}
```

Vom generierten Elemente `[Wrap]` sind nicht `virtual` standardmäßig, müssen Sie eine `virtual` Member, die Sie zum festlegen können `true` das optionale `isVirtual` Parameter.

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

Dieser Abschnitt beschreibt die Attribute, die auf die Parameter in einer Methodendefinition angewendet werden können, sowie die `NullAttribute` , die für eine Eigenschaft als Ganzes gilt.

<a name="BlockCallback" />

### <a name="blockcallback"></a>BlockCallback

Dieses Attribut wird angewendet, um Parametertypen in c# Delegatdeklarationen Binder zu benachrichtigen, dass der betreffende Parameter entspricht der Objective-C-Block, der Aufrufkonvention und auf diese Weise marshallen soll.

Dies wird normalerweise für Rückrufe verwendet, die wie folgt definiert sind, in Objective-c:

```csharp
typedef returnType (^SomeTypeDefinition) (int parameter1, NSString *parameter2);
```

Siehe auch: [CCallback](#CCallback).

<a name="CCallback" />

### <a name="ccallback"></a>CCallback

Dieses Attribut wird angewendet, um Parametertypen in c# Delegatdeklarationen Binder zu benachrichtigen, dass der betreffende Parameter der C#-ABI-Funktion Zeiger-Aufrufkonvention entspricht und auf diese Weise marshallen soll.

Dies wird normalerweise für Rückrufe verwendet, die wie folgt definiert sind, in Objective-c:

    typedef returnType (*SomeTypeDefinition) (int parameter1, NSString *parameter2);

Siehe auch: [BlockCallback](#BlockCallback).

### <a name="params"></a>params

Sie können die `[Params]` Attribut in der letzten Arrayparameter einer Methodendefinition den Generator einfügen "Params" in der Definition aufweisen.   Dies ermöglicht die Bindung für optionale Parameter problemlos zu ermöglichen.

Beispielsweise wie folgt definiert:

    [Export ("loadFiles:")]
    void LoadFiles ([Params]NSUrl [] files);

Können den folgenden Code geschrieben werden:

    foo.LoadFiles (new NSUrl (url));
    foo.LoadFiles (new NSUrl (url1), new NSUrl (url2), new NSUrl (url3));

Dies hat den zusätzlichen Vorteil, dass keine Benutzer zum Erstellen eines Arrays ausschließlich für die Übergabe von Elemente erforderlich ist.

<a name="plainstring" />

### <a name="plainstring"></a>PlainString

Können Sie die `[PlainString]` Attribut vor Zeichenfolgenparametern weisen Sie den Generator Bindung als eine C-Zeichenfolge anstelle der Übergabe des Parameters als Zeichenfolge übergeben an eine `NSString`.

Die meisten Objective-C-APIs nutzen `NSString` Parameter, aber eine Handvoll APIs verfügbar machen eine `char *` API für die Übergabe von Zeichenfolgen, anstatt die `NSString` Variante.
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

Weist den Generator, um einen Verweis auf den angegebenen Parameter zu halten. Der Generator gebe den Sicherungsspeicher für dieses Feld ein, oder geben Sie einen Namen (die `WrapName`) zum Speichern des Werts an. Dies ist hilfreich, um einen Verweis auf ein verwaltetes Objekt zu speichern, die an Objective-C als Parameter übergeben wird, und wenn Sie wissen, dass Objective-C nur diese Kopie des Objekts beibehalten werden sollen. Z. B. eine API wie `SetDisplay (SomeObject)` würde dieses Attribut verwenden, wie es wahrscheinlich ist, dass die SetDisplay jeweils nur ein Objekt angezeigt. Wenn Sie zum Nachverfolgen mehr als ein Objekt (z. B. für einen Stapel-ähnliche-API) würden Sie verwenden die `RetainList` Attribut.

Syntax:

```csharp
public class RetainAttribute {
    public RetainAttribute ();
    public RetainAttribute (string wrapName);
    public string WrapName { get; }
}
```


### <a name="retainlistattribute"></a>RetainListAttribute

Weist den Generators zum Beibehalten eines verwalteten Verweis auf den Parameter oder entfernen einen internen Verweis auf den Parameter an. Dies wird verwendet, auf die verwiesen wird Objekte zu halten.

Syntax:

```csharp
public class RetainListAttribute: Attribute {
     public RetainListAttribute (bool doAdd, string listName);
}
```

Wenn der Wert von "DoAdd" ist "true", der Parameter hinzugefügt, um die `__mt_{0}_var List<NSObject>`. Auf dem `{0}` wird ersetzt, mit der angegebenen `listName`. Diese dahinter liegende Feld müssen in Ihre ergänzende Teilklasse an die API deklariert werden.

Ein Beispiel finden Sie unter [foundation.cs](https://github.com/mono/maccore/blob/master/src/foundation.cs) und [NSNotificationCenter.cs](https://github.com/mono/maccore/blob/master/src/Foundation/NSNotificationCenter.cs)


### <a name="transientattribute"></a>TransientAttribute

Dieses Attribut angewendet wird, um Parameter und wird nur verwendet, bei der Umstellung von Objective-C in c#.  Während dieser Übergänge verschiedene Objective-C-NSObjects sind Parameter in einer verwalteten Darstellung des Objekts eingeschlossen.

Die Common Language Runtime einen Verweis auf das systemeigene Objekt übernimmt und behalten Sie den Verweis aus, bis der letzte verwaltete Verweis auf das Objekt nicht mehr vorhanden ist, und der globale Katalogserver wurde ausgeführt.

In einigen Fällen ist es wichtig, für die C#-Common Language Runtime einen Verweis auf das systemeigene Objekt nicht beibehalten.  Dies geschieht in einigen Fällen auf, wenn die zugrunde liegenden systemeigene Code ein spezielles Verhalten für den Lebenszyklus des Parameters angefügt hat.  Zum Beispiel: der Destruktor für den Parameter führen Sie eine Bereinigung-Aktion oder eine wertvolle Ressource dispose wird.

Dieses Attribut informiert die Laufzeit, die Sie wünschen, dass das Objekt bei der Rückgabe an Objective-C aus der überschriebenen Methode möglichst verworfen werden sollen.

Die Regel ist sehr einfach: Wenn die Laufzeit musste erstellen eine neue verwaltete Darstellung aus das systemeigene Objekt, und klicken Sie dann am Ende der Funktion, die Anzahl der Retain für das systemeigene Objekt gelöscht werden, und die Handle-Eigenschaft des verwalteten Objekts wird gelöscht.   Dies bedeutet, dass wenn Sie einen Verweis auf das verwaltete Objekt gehalten, diesen Verweis unbrauchbar werden soll (Aufrufen von Methoden auf, es wird eine Ausnahme ausgelöst).

Wenn das übergebene Objekt nicht erstellt wurde, oder es wurde bereits eine ausstehende verwaltete Darstellung des Objekts, ist die erzwungene Dispose nicht stattfinden. 


## <a name="property-attributes"></a>Eigenschaftenattribute

### <a name="notimplementedattribute"></a>NotImplementedAttribute

Dieses Attribut wird verwendet, um Objective-C-Idiom, in denen eine Eigenschaft mit einer Get-Methode wird in einer Basisklasse eingeführt wird, und eine änderbare Unterklasse führt einen Setter, zu unterstützen.

Da c# dieses Modell nicht unterstützt die Basisklasse benötigt Setter-Methode und der Getter und Sie eine Unterklasse können der [OverrideAttribute](#OverrideAttribute).

Dieses Attribut wird nur in Eigenschaften-Settern verwendet und dient zur Unterstützung der änderbaren Idioms in Objective-c

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

<a name="enum-attributes"/>

## <a name="enum-attributes"></a>Enum-Attribut

Zuordnen von `NSString` Konstanten, mit denen Enumerationswerte ist eine einfache Möglichkeit, eine bessere .NET API zu erstellen. Es:

* ermöglicht das codevervollständigung nützlicher, da anzeigt werden **nur** die korrekten Werte für die API;
* Fügt der typsicherheit, können keine anderen `NSString` Konstanten in einem falschen Kontext; und
* ermöglicht es, einige Konstanten, ausführenden codevervollständigung kürzeren API-Liste angezeigt wird, ohne die Funktionen ausblenden.

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

Aus den oben genannten Bindungsdefinition erstellt der Generator die `enum` selbst und erstellt auch eine `*Extensions` statischen Typ, der zwei-Wege-Konvertierungsmethoden zwischen die Enumerationswerte enthält und die `NSString` Konstanten. Das bedeutet, dass die Konstanten bleibt für Entwickler verfügbar, auch wenn sie nicht Teil der API sind.

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

Sie können ergänzen **eine** Enum-Wert mit diesem Attribut. Dadurch werden die Konstante, die zurückgegeben werden, wenn der Enum-Wert nicht bekannt ist.

Von den oben genannten Beispiel:

```csharp
var x = (NSRunLoopMode) 99;
Call (x.GetConstant ()); // NSDefaultRunLoopMode will be used
```

Wenn kein Enumerationswert ergänzt wird ein `NotSupportedException` ausgelöst.

### <a name="errordomainattribute"></a>ErrorDomainAttribute

Fehlercodes, die als Enumerationswerten gebunden sind. Im Allgemeinen eine Fehler-Domäne für sie vorhanden ist, und es ist nicht immer einfach zu suchen, welche gilt (oder ggf. eine selbst).

Sie können dieses Attribut verwenden, mit der Fehler-Domäne mit der Enumeration selbst verknüpft.

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

Sie können dann aufrufen die Erweiterungsmethode `GetDomain` um die Domäne eines eventuellen Fehlers Konstanten abzurufen.

### <a name="fieldattribute"></a>FieldAttribute

Dies ist der gleiche `[Field]` für Konstanten in Typ verwendete Attribut. Auch kann innerhalb von Enumerationen verwendet werden, um einen Wert mit einer bestimmten Konstanten zuzuordnen.

Ein `null` Wert dienen zum Angeben der Enum-Wert zurückgegeben werden soll, wenn eine `null` `NSString` Konstante angegeben wird.

Von den oben genannten Beispiel:

```csharp
var constant = NSRunLoopMode.NewInWatchOS3; // will be null in watchOS 2.x
Call (NSRunLoopModeExtensions.GetValue (constant)); // will return 1000
```

Wenn kein `null` Wert existiert ein `ArgumentNullException` ausgelöst.

## <a name="global-attributes"></a>Globale Attribute

Globale Attribute gelten entweder mithilfe der `[assembly:]` Attributmodifizierer wie die `LinkWithAttribute` oder kann überall, z. B. verwendet die `Lion` und `Since` Attribute.


### <a name="linkwithattribute"></a>LinkWithAttribute

Dies ist ein Attribut auf Assemblyebene dem Entwickler die verknüpfen Flags erforderlich, um eine gebundene Bibliothek wiederverwenden, ohne dass der Consumer der Bibliothek manuell konfigurieren, die Gcc_flags und zusätzliche Mtouch-Argumente, die an eine Bibliothek angeben kann.

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

Dieses Attribut auf Assemblyebene angewendet wird, sieht z. B., was die [CorePlot Bindungen](https://github.com/mono/monotouch-bindings/tree/master/CorePlot) verwenden:

```csharp
[assembly: LinkWith ("libCorePlot-CocoaTouch.a", LinkTarget.ArmV7 | LinkTarget.ArmV7s | LinkTarget.Simulator, Frameworks = "CoreGraphics QuartzCore", ForceLoad = true)]
```

Bei Verwendung der `LinkWith` -Attribut mit dem angegebenen `libraryName` eingebettet ist, in die resultierende Assembly, sodass Benutzer eine einzige DLL ausgeliefert, die sowohl die nicht verwalteten Abhängigkeiten als auch die Befehlszeilenflags erforderlich, um ordnungsgemäß zu nutzen, enthält die Xamarin.iOS-Bibliothek.

Es ist auch möglich, geben Sie keine `libraryName`, in diesem Fall die `LinkWith` Attribut kann verwendet werden, um nur zusätzliche Linker-Flags angeben:

 ``` csharp
[assembly: LinkWith (LinkerFlags = "-lsqlite3")]
 ```


#### <a name="linkwithattribute-constructors"></a>LinkWithAttribute-Konstruktoren

Diese Konstruktoren ermöglichen Ihnen das Festlegen die Bibliothek zu verknüpfen und Einbetten in die resultierende Assembly, die unterstützten Ziele, die die Bibliothek unterstützt und optionale Flags, die erforderlich sind, um mit der Bibliothek zu verknüpfen.

Beachten Sie, dass das Argument LinkTarget von Xamarin.iOS abgeleitet ist und muss nicht festgelegt werden.

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

Die `ForceLoad` Eigenschaft wird verwendet, um zu entscheiden, und zwar unabhängig davon, ob die `-force_load` Link-Flag wird verwendet, für die systemeigene Bibliothek zu verknüpfen. Jetzt sollte dies immer "true" sein.


#### <a name="linkwithattributeframeworks"></a>LinkWithAttribute.Frameworks

Wenn die Bibliothek gebunden wird keines der Frameworks zwingend schwer abhängt (außer `Foundation` und `UIKit`), sollten Sie festlegen der `Frameworks` -Eigenschaft in eine Zeichenfolge, die eine durch Leerzeichen getrennte Liste der Frameworks erforderliche Plattform enthält. Beispielsweise, wenn Sie eine Bibliothek binden, erfordert `CoreGraphics` und `CoreText`, legen Sie die `Frameworks` Eigenschaft `"CoreGraphics CoreText"`.


#### <a name="linkwithattributeiscxx"></a>LinkWithAttribute.IsCxx

Legen Sie diese Eigenschaft auf "true", wenn die resultierende ausführbare Datei mit einem C++-Compiler anstelle des Standardwerts, also eine C-Compiler kompiliert werden muss. Verwenden Sie diese Option, wenn die Bibliothek, die Sie binden in C++ geschrieben wurde.


#### <a name="linkwithattributelibraryname"></a>LinkWithAttribute.LibraryName

Der Name der nicht verwaltete Bibliothek bündeln. Dies ist eine Datei mit der Erweiterung "a", und es kann Objektcode für mehrere Plattformen (z. B. "," ARM "und" X86 für den Simulator ") enthalten.

Frühere Versionen von Xamarin.iOS überprüft die `LinkTarget` -Eigenschaft zum Bestimmen von der Plattformaufrufs Ihrer Bibliothek unterstützt, aber dies ist jetzt automatisch erkannt, und die `LinkTarget` Eigenschaft wird ignoriert.


#### <a name="linkwithattributelinkerflags"></a>LinkWithAttribute.LinkerFlags

Die `LinkerFlags` Zeichenfolge bietet eine Möglichkeit für Autoren von Bindungen an alle zusätzlichen Linker-Flags erforderlich, wenn die systemeigene Bibliothek in der Anwendung zu verknüpfen.

Beispielsweise, wenn die systemeigene Bibliothek libxml2 und Zlib erforderlich ist, legen Sie die `LinkerFlags` Zeichenfolge `"-lxml2 -lz"`.


#### <a name="linkwithattributelinktarget"></a>LinkWithAttribute.LinkTarget

Frühere Versionen von Xamarin.iOS überprüft die `LinkTarget` -Eigenschaft zum Bestimmen von der Plattformaufrufs Ihrer Bibliothek unterstützt, aber dies ist jetzt automatisch erkannt, und die `LinkTarget` Eigenschaft wird ignoriert.

#### <a name="linkwithattributeneedsgccexceptionhandling"></a>LinkWithAttribute.NeedsGccExceptionHandling

Legen Sie diese Eigenschaft auf "true", wenn die Bibliothek, die Sie verknüpfen die Ausnahmebehandlung GCC-Bibliothek (Gcc_eh) erforderlich ist.


#### <a name="linkwithattributesmartlink"></a>LinkWithAttribute.SmartLink

Die `SmartLink` Eigenschaft sollte festgelegt werden, auf "true", um Xamarin.iOS bestimmen können, ob `ForceLoad` erforderlich ist, oder nicht.


#### <a name="linkwithattributeweakframeworks"></a>LinkWithAttribute.WeakFrameworks

Die `WeakFrameworks` Eigenschaft funktioniert genauso wie die `Frameworks` -Eigenschaft, außer dass zum Zeitpunkt der Verknüpfung, die `-weak_framework` Spezifizierer an Gcc für jede der aufgeführten Frameworks übergeben wird.

`WeakFrameworks` ermöglicht es Bibliotheken und Anwendungen zu schwach Link für den Webplattform-Frameworks, damit sie optional sie verwenden können, wenn sie verfügbar sind, jedoch keine harte Abhängigkeiten auf ihnen dies nützlich akzeptieren ist, wenn Ihre Bibliothek dafür, zum Hinzufügen von zusätzlicher Funktionen vorgesehen ist auf neuere iOS-Versionen. Weitere Informationen zum Verknüpfen von schwachen finden Sie in Apple Dokumentation auf [schwache verknüpfen](http://developer.apple.com/library/mac/#documentation/MacOSX/Conceptual/BPFrameworks/Concepts/WeakLinking.html).

Gute Kandidaten für das Verknüpfen von schwachen wäre `Frameworks` wie z. B. Konten, `CoreBluetooth`, `CoreImage`, `GLKit`, `NewsstandKit` und `Twitter` , da sie nur in Ios5 verfügbar sind.

### <a name="sinceattribute-ios-and-lionattribute-macos"></a>SinceAttribute (iOS) und LionAttribute (MacOS)

Sie verwenden die `Since` Attribut Flag APIs müssen an einem bestimmten Zeitpunkt eingeführt wird. Das Attribut sollte nur verwendet werden, um den kennzeichnen, Typen und Methoden, die eine Laufzeit-Problem verursachen könnte, wenn die zugrunde liegenden Klasse, Methode oder Eigenschaft nicht verfügbar ist.

Syntax:

```csharp
public SinceAttribute : Attribute {
     public SinceAttribute (byte major, byte minor);
     public byte Major, Minor;
}
```

Sie sollten im Allgemeinen nicht auf Enumerationen, Einschränkungen oder neue Strukturen angewendet werden als solche keinen Laufzeitfehler verursachen würde, wenn sie auf einem Gerät mit einer älteren Version des Betriebssystems ausgeführt werden.

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

Beispiel, wenn es an einen neuen Member angewendet:

```csharp
[BaseType (typeof (UIViewController))]
public interface UITableViewController {
    [Export ("tableView", ArgumentSemantic.Retain)]
    UITableView TableView { get; set; }

    [Since (3,2)]
    [Export ("clearsSelectionOnViewWillAppear")]
    bool ClearsSelectionOnViewWillAppear { get; set; }
```

Die `Lion` Attribut angewendet wird, auf die gleiche Weise aber für Typen, die mit Lion eingeführt wurde. Der Grund für das verwenden `Lion` im Vergleich zu die spezifische Versionsnummer, die im iOS verwendet wird, ist, iOS wird sehr häufig überarbeitet, während die OS X-Hauptversionen geschieht nur selten, und es ist einfacher, die durch ihre Codename als durch die Versionsnummer des Betriebssystems Beachten


### <a name="adviceattribute"></a>AdviceAttribute

Verwenden Sie dieses Attribut, um einen Hinweis zu anderen APIs Entwicklern, die möglicherweise bequemer verwenden.   Z. B. Wenn Sie eine stark typisierte Version einer API bereitstellen, können dieses Attribut für das Attribut schwach typisierte Sie um den Entwickler, die eine bessere-API zu leiten.

Die Informationen von diesem Attribut wird angezeigt, in der Dokumentation und Tools können entwickelt werden, um Benutzer Vorschläge zum Verbessern bieten

### <a name="zerocopystringsattribute"></a>ZeroCopyStringsAttribute

Nur in Xamarin.iOS 5.4 und höher verfügbar.

Dieses Attribut weist den Generator, der die Bindung für diesen spezifischen Bibliothek (wenn angewendet `[assembly:]`) oder Typ das schnelle Kopieren von NULL-Zeichenfolge Marshalling verwenden sollten. Dieses Attribut entspricht die Befehlszeilenoption übergeben `--zero-copy` an den Generator.

Beim Kopieren von 0 (null) für Zeichenfolgen zu verwenden, der Generator effektiv verwendet die gleiche C#-Zeichenfolge als die Zeichenfolge, die in Objective-C benötigt werden, ohne dass die Erstellung eines neuen `NSString` -Objekt und die Daten von den C#-Zeichenfolgen kopiert, auf die Objective-C-Zeichenfolge zu vermeiden. Der einzige Nachteil der Verwendung von Zeichenfolgen Kopie von 0 (null) ist, Sie sicherstellen müssen, dass alle Zeichenfolgeneigenschaft, die Sie zu umschließen, die zufällig zur gekennzeichnet werden, wie "beibehalten" oder "Kopieren" hat die `DisableZeroCopy` -Attribut festgelegt ist. Dies ist erforderlich, da das Handle für das Kopieren von 0 (null) Zeichenfolgen wird auf dem Stapel zugeordnet und ist nach der Funktionsrückgabe ungültig.

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

Sie können auch das Attribut auf Assemblyebene anwenden, und es gilt für alle Typen der Assembly:

    [assembly:ZeroCopyStrings]

## <a name="strongly-typed-dictionaries"></a>Stark typisierte Wörterbücher

Mit Xamarin.iOS 8.0 eingeführt wir Unterstützung für das einfache Erstellen stark typisierter Klassen, Wrap `NSDictionaries`.

Während es immer möglich, wurde die [DictionaryContainer](https://developer.xamarin.com/api/type/Foundation.DictionaryContainer/) Datentyp zusammen mit einer manuellen-API, es ist jetzt viel einfacher zu diesem Zweck.  Weitere Informationen finden Sie unter [einbringen starke Typen](~/cross-platform/macios/binding/objective-c-libraries.md#Surfacing_Strong_Types).


### <a name="strongdictionary"></a>StrongDictionary

Wenn dieses Attribut auf eine Schnittstelle angewendet wird, wird der Generator erzeugen eine Klasse mit dem gleichen Namen wie die Schnittstelle, die abgeleitet [DictionaryContainer](https://developer.xamarin.com/api/type/Foundation.DictionaryContainer/) und jede Eigenschaft in der Schnittstelle definiert, in einen stark typisierten aktiviert Getter und Setter für das Wörterbuch.

Dies generiert automatisch eine Klasse, die instanziiert werden kann von einem vorhandenen `NSDictionary` oder neu erstellt wurde.

Dieses Attribut nimmt einen Parameter, den Namen der Klasse, die mit den Schlüsseln, die Zugriff auf die Elemente auf das Wörterbuch verwendet werden.   Standardmäßig wird jede Eigenschaft in der Schnittstelle mit dem Attribut ein Element in den angegebenen Typ für einen Namen mit dem Suffix "Schlüssel" suchen.

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

Im obigen Fall die `MyOption` Klasse erzeugt eine Zeichenfolgeneigenschaft für `Name` verwenden, die die `MyOptionKeys.NameKey` als Schlüssel in das Wörterbuch, das eine Zeichenfolge abzurufen.   Verwendet die `MyOptionKeys.AgeKey` als Schlüssel in das Wörterbuch zum Abrufen einer `NSNumber` enthält ein "int".

Wenn Sie einen anderen Product Key verwenden möchten, können Sie z. B. das Exportieren-Attribut für die Eigenschaft verwenden:

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

<table border="1" cellpadding="1" cellspacing="1" width="80%">
<tbody>
  <tr>
    <td>C#-Schnittstellentyp</td>
    <td>NSDictionary Speichertyp</td>
  </tr>
  <tr>
    <td>bool</td>
    <td>Boolescher Wert in einer NSNumber gespeichert</td>
  </tr>
  <tr>
    <td>Enumerationswerte</td>
    <td>ganze Zahl, die in einer NSNumber gespeichert</td>
  </tr>
  <tr>
    <td>int</td>
    <td>32-Bit-Ganzzahl, die in einer NSNumber gespeichert</td>
  </tr>
  <tr>
    <td>uint</td>
    <td>32-Bit-Ganzzahl ohne Vorzeichen in eine NSNumber gespeichert</td>
  </tr>
  <tr>
    <td>nint</td>
    <td>Gespeichert in einer NSNumber NSInteger</td>
  </tr>
  <tr>
    <td>nuint</td>
    <td>Gespeichert in einer NSNumber NSUInteger</td>
  </tr>
  <tr>
    <td>long</td>
    <td>64-Bit-Ganzzahl, die in einer NSNumber gespeichert</td>
  </tr>
  <tr>
    <td>float</td>
    <td>32-Bit-Ganzzahl, die als eine NSNumber gespeichert</td>
  </tr>
  <tr>
    <td>double</td>
    <td>64-Bit-Ganzzahl, die als eine NSNumber gespeichert</td>
  </tr>
  <tr>
    <td>NSObject und Unterklassen einzugeben</td>
    <td>NSObject</td>
  </tr>
  <tr>
    <td>NSDictionary</td>
    <td>NSDictionary</td>
  </tr>
  <tr>
    <td>Zeichenfolge</td>
    <td>NSString</td>
  </tr>
  <tr>
    <td>NSString</td>
    <td>NSString</td>
  </tr>
  <tr>
    <td>C#-Array von NSObject</td>
    <td>NSArray</td>
  </tr>
  <tr>
    <td>C#-Array von Enumerationen</td>
    <td>NSArray mit NSNumbers mit dem Wert</td>
  </tr>
</tbody>
