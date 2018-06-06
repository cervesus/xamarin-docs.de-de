---
title: Generische Unterklassen des NSObject in Xamarin.iOS
description: Dieses Dokument enthält Informationen zum Erstellen generische Unterklassen des NSObject erstellen. Was können und kann nicht durchgeführt werden, die statische Registrierungsstelle erläutert und untersucht, mit der Leistung untersucht.
ms.prod: xamarin
ms.assetid: BB99EBD7-308A-C865-1829-4DFFDB1BBCA4
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 9caad9d4990225a0468be8ee4987eaa9fea0c118
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34786482"
---
# <a name="generic-subclasses-of-nsobject-in-xamarinios"></a>Generische Unterklassen des NSObject in Xamarin.iOS

## <a name="using-generics-with-nsobjects"></a>Verwenden von Generika mit NSObjects

Beginnend mit Xamarin.iOS 7.2.1 Sie Generika verwenden kann, in Unterklassen der `NSObject` (z. B. [UIView](https://developer.xamarin.com/api/type/UIKit.UIView/)).

Sie können jetzt die generische Klassen, wie diese erstellen:

```csharp
class Foo<T> : UIView {
    public Foo (CGRect x) : base (x) {}
    public override void Draw (CoreGraphics.CGRect rect)
    {
        Console.WriteLine ("T: {0}. Type: {1}", typeof (T), GetType ().Name);
    }
}
```

Da Objekte, Unterklasse `NSObject` registriert sind mit der Objective-C-Laufzeit gelten einige Einschränkungen, was mit generischen Unterklassen des möglich ist `NSObject` Typen.
    
## <a name="considerations-for-generic-subclasses-of-nsobject"></a>Überlegungen für die generische Unterklassen der NSObject

In diesem Dokument werden die Einschränkungen in der eingeschränkten Unterstützung für generische Unterklassen des `NSObjects` mit Xamarin.iOS 7.2.1 eingeführt wurde.
    
### <a name="generic-type-arguments-in-member-signatures"></a>Generische Typargumente in Membersignaturen

Alle generische Typargumente in einer Signatur des Members für Objective-C verfügbar gemacht, benötigen eine `NSObject` Einschränkung.

**Gute**:

```csharp
class Generic<T> : NSObject where T: NSObject
{
    [Export ("myMethod:")]
    public void MyMethod (T value)
    {
    }
}
```

**Grund**: der generische Typparameter ist ein `NSObject`, sodass die Signatur Selektor für `myMethod:` sicher verfügbar gemacht werden können, Objective-C (es werden immer `NSObject` oder eine Unterklasse davon).

**Ungültige**:

```csharp
class Generic<T> : NSObject
{
    [Export ("myMethod:")]
    public void MyMethod (T value)
    {
    }
}
```

**Grund**: Es ist nicht möglich, eine Objective-C-Signatur für die exportierte Elemente zu erstellen, die Objective-C-Code aufgerufen werden kann, da die Signatur der genaue Typ des generischen Typs hängen würde, `T`.

**Gute**:

```csharp
class Generic<T> : NSObject
{
    T storage;

    [Export ("myMethod:")]
    public void MyMethod (NSObject value)
    {
    }
}
```

**Grund**: Es ist möglich, generischen Typargumenten uneingeschränkten haben, solange sie nicht Teil der exportierten Membersignatur ausführen.

**Gute**:

```csharp
class Generic<T, U> : NSObject where T: NSObject
{
    [Export ("myMethod:")]
    public void MyMethod (T value)
    {
        Console.WriteLine (typeof (U));
    }
}
```

**Grund**: die `T` Parameter in der Objective-C exportiert `MyMethod` ist beschränkt auf eine `NSObject`, beschränkten Typ `U` ist nicht Teil der Signatur.
    
### <a name="instantiations-of-generic-types-from-objective-c"></a>Instanziierungen von generischen Typen von Objective-C

Instanziierung von generischen Typen von Objective-C ist nicht zulässig. Dies tritt normalerweise auf, wenn ein verwalteter Typ in einen Xib verwendet wird.

Betrachten Sie diese Klassendefinition, die einen Konstruktor verfügbar macht, die akzeptiert ein `IntPtr` (die Xamarin.iOS Methode zum Erstellen eines C#-Objekts aus einer systemeigenen Objective-C-Instanz):
    
```
class Generic<T> : NSObject where T : NSObject
{
    public Generic () {}
    public Generic (IntPtr ptr) : base (ptr) {}
}
```

Während der oben genannten Konstrukt Ordnung, zur Laufzeit ist, löst diese Ausnahme an, wenn Objective-C versucht, eine Instanz zu erstellen.

Dies ist darauf zurückzuführen, dass Objective-C verfügt über kein Konzept für generische Typen und kann nicht den genauen generischen Typ erstellen angeben.

Dieses Problem kann umgangen werden, indem Sie eine spezielle Unterklasse des generischen Typs erstellen.   Zum Beispiel:
    
```
class Generic<T> : NSObject where T : NSObject
{
    public Generic () {}
    public Generic (IntPtr ptr) : base (ptr) {}
}

class GenericUIView : Generic<UIView>
{
}
```

Jetzt gibt es keine Mehrdeutigkeit mehr, die Klasse `GenericUIView` in Xibs verwendet werden kann.

## <a name="no-support-for-generic-methods"></a>Keine Unterstützung für generische Methoden

### <a name="generic-methods-are-not-allowed"></a>Generische Methoden sind nicht zulässig.

Der folgende Code wird nicht kompiliert:

```csharp
class MyClass : NSObject
{
    [Export ("myMethod")]
    public void MyMethod<T> (T argument)
    {
    }
}
```

**Grund**: Dies ist nicht zulässig, da Xamarin.iOS nicht kennt den Typ an, die für das Typargument verwendet `T` beim Aufrufen der Methodennamens aus Objective-C.

Eine Alternative ist eine spezielle Methode erstellen und exportieren, die stattdessen:

```csharp
class MyClass : NSObject
{
    [Export ("myMethod")]
    public void MyUIViewMethod (UIView argument)
    {
        MyMethod<UIView> (argument);
    }
    public void MyMethod<T> (T argument)
    {
    }
}
```

### <a name="no-exported-static-members-allowed"></a>Keine exportierten statische Member zulässig

Sie können nicht verfügbar machen eine statische Member für Objective-C-innerhalb einer generischen Unterklasse von gehostet wird `NSObject`.

Beispiel für ein nicht unterstütztes Szenario:

```csharp
class Generic<T> : NSObject where T : NSObject
{
    [Export ("myMethod:")]
    public static void MyMethod ()
    {
    }

    [Export ("myProperty")]
    public static T MyProperty { get; set; }
}
```

**Ursache:** ebenso wie generische Methoden, die Xamarin.iOS Common Language Runtime-Anforderungen in der Lage zu wissen, welcher Typ für das generische Typargument t verwenden

Z. B. Elemente, die die Instanz selbst verwendet wird (da wird nie eine Instanz generische werden<T>, werden immer generische<SomeSpecificClass>), doch für statische Member dieser Informationen ist nicht vorhanden.

Beachten Sie, dass dies gilt auch, wenn das betreffende Element nicht das Argument vom Typ T in keiner Weise verwendet.

Die Alternative ist in diesem Fall erstellen Sie eine spezielle Unterklasse:

```csharp
class GenericUIView : Generic<UIView>
{
    [Export ("myUIViewMethod")]
    public static void MyUIViewMethod ()
    {
        MyMethod ();
    }

    [Export ("myProperty")]
    public static UIView MyUIVIewProperty {
        get { return MyProperty; }
        set { MyProperty = value; }
    }
}

class Generic<T> : NSObject where T : NSObject
{
    public static void MyMethod () {}
    public static T MyProperty { get; set; }
}
```

### <a name="requires-new-static-registrar"></a>Neue statische Registrierungsstelle erfordert

Die Generikunterstützung erfordert das neue [Registrierungssystem](~/ios/internals/registrar.md).

Wenn Sie versuchen, verwenden Sie die alte legacy-Erfassung System werden Warnungen angezeigt, wenn generische Typen (in Ergänzung zum korrekten Code, was zu nicht definiertem Verhalten führt nicht zu generieren) gefunden wird.
    
## <a name="performance"></a>Leistung

Statische Registrierungsstelle kann keinen exportierten Member in einem generischen Typ zur Buildzeit aufgelöst, wie normalerweise der Fall ist, ist ihr zur Laufzeit gesucht werden soll. Dies bedeutet, dass solche Methode Objective-C aufrufen etwas langsamer als das Aufrufen von Membern von nicht generischen Klassen.

