---
title: Generische Unterklassen von NSObject in Xamarin.iOS
description: Dieses Dokument enthält Informationen zum Erstellen generische Unterklassen von NSObject zu erstellen. Er untersucht, was können und kann nicht durchgeführt werden, wird erläutert, die statische Registrierungsstelle und wirft einen Blick auf die Leistung.
ms.prod: xamarin
ms.assetid: BB99EBD7-308A-C865-1829-4DFFDB1BBCA4
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/21/2017
ms.openlocfilehash: 512280e9c298cfbcea6f693b0691236fd1cf5a5f
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61036473"
---
# <a name="generic-subclasses-of-nsobject-in-xamarinios"></a>Generische Unterklassen von NSObject in Xamarin.iOS

## <a name="using-generics-with-nsobjects"></a>Verwenden von Generika mit NSObjects

Ab Xamarin.iOS 7.2.1 Sie Generika verwenden kann, in Unterklassen der `NSObject` (z. B. [UIView](xref:UIKit.UIView).

Sie können jetzt generische Klassen, wie die folgende erstellen:

```csharp
class Foo<T> : UIView {
    public Foo (CGRect x) : base (x) {}
    public override void Draw (CoreGraphics.CGRect rect)
    {
        Console.WriteLine ("T: {0}. Type: {1}", typeof (T), GetType ().Name);
    }
}
```

Da Objekte diese Unterklasse `NSObject` registriert sind mit den Objective-C-Laufzeit gelten einige Einschränkungen hinsichtlich der Möglichkeiten generische Unterklassen von `NSObject` Typen.
    
## <a name="considerations-for-generic-subclasses-of-nsobject"></a>Überlegungen für generische Unterklassen von NSObject

In diesem Dokument werden die Einschränkungen bei der eingeschränkten Unterstützung für generische Unterklassen von `NSObjects` mit Xamarin.iOS 7.2.1 eingeführt wurde.
    
### <a name="generic-type-arguments-in-member-signatures"></a>Generische Typargumente in Membersignaturen

Alle generischen Typargumente in eine Membersignatur verfügbar gemacht werden, mit Objective-C benötigen eine `NSObject` Einschränkung.

**Good**:

```csharp
class Generic<T> : NSObject where T: NSObject
{
    [Export ("myMethod:")]
    public void MyMethod (T value)
    {
    }
}
```

**Reason**: Der generische Typparameter ist eine `NSObject`, sodass die Auswahl-Signatur für `myMethod:` sicher verfügbar gemacht werden können mit Objective-C (sie werden immer `NSObject` oder eine Unterklasse dieser).

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

**Grund**: Es ist nicht möglich, die eine Objective-C-Signatur für die exportierten Elemente zu erstellen, die Objective-C-Code aufrufen kann, da die Signatur, hängt von der genaue Typ des generischen Typs würden, `T`.

**Good**:

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

**Grund**: Es ist möglich, die generischen Typargumente uneingeschränkt haben, solange sie keinen Teil der Signatur der exportierten Member belegen.

**Good**:

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

**Grund**: die `T` Parameter in die Objective-C exportiert `MyMethod` beschränkt sein ein `NSObject`, die beschränkten Typ `U` ist nicht Teil der Signatur.
    
### <a name="instantiations-of-generic-types-from-objective-c"></a>Instanziierungen von generischen Typen von Objective-C

Instanziierung von generischen Typen von Objective-C ist nicht zulässig. Dies tritt normalerweise auf, wenn ein verwalteter Typ in eine Xib verwendet wird.

Betrachten Sie diese Klassendefinition, die einen Konstruktor verfügbar macht, die akzeptiert eine `IntPtr` (die Methode zum Erstellen von Xamarin.iOS ein C# Objekt von einer nativen Objective-C-Instanz):
    
```
class Generic<T> : NSObject where T : NSObject
{
    public Generic () {}
    public Generic (IntPtr ptr) : base (ptr) {}
}
```

Obwohl das obige Konstrukt in Ordnung, zur Laufzeit ist, wird dies zur eine Ausnahme auslöst, wenn Objective-C-versucht wird, um eine Instanz davon erstellen.

Ist dies liegt daran, dass Objective-C kein Konzept für generische Typen hat und können nicht den genauen generischen Typ erstellen angeben.

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

Jetzt gibt es keine Mehrdeutigkeit, nicht mehr die Klasse `GenericUIView` in Xibs verwendet werden kann.

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

**Reason**: Dies ist nicht zulässig, da Xamarin.iOS nicht weiß, welchen Typ, der für das Typargument verwendet `T` Wenn die Methode von Objective-C aufgerufen wird.

Eine Alternative ist stattdessen exportieren und erstellen Sie eine spezielle Methode:

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

Sie können nicht verfügbar machen eine statische Member mit Objective-C innerhalb einer generischen Unterklasse von gehostet ist `NSObject`.

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

**Reason:** Genau wie generische Methoden muss die Xamarin.iOS-Runtime können zu wissen, welcher Typ für die Verwendung für das generische Typargument "t".

Z. B. Elemente, die die Instanz selbst wird verwendet (da es nie eine Instanz generische werden<T>, sie werden immer die generische<SomeSpecificClass>), für statische Member dieser Informationen ist jedoch nicht vorhanden.

Beachten Sie, dass dies gilt auch, wenn das betreffende Element nicht den Argument vom Typ T in keiner Weise verwendet.

Die Alternative ist in diesem Fall eine spezialisierte Unterklasse erstellen:

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

Die Generikunterstützung benötigt die neue [Registrierungssystem](~/ios/internals/registrar.md).

Wenn Sie versuchen, verwenden Sie die alten legacy Registrierungssystem werden Warnungen anzeigen, wenn es feststellt, dass generische Typen (in Ergänzung Code nicht korrekt, was zu nicht definiertem Verhalten erstellen).
    
## <a name="performance"></a>Leistung

Die statische Registrierungsstelle kann keinen exportierten Member in einem generischen Typ zum Zeitpunkt der Erstellung aufgelöst, wie dies in der Regel der Fall ist, müssen sie zur Laufzeit gesucht werden soll. Dies bedeutet, dass eine solche Methode von Objective-C aufrufen etwas langsamer als das Aufrufen der Member von nicht generischen Klassen.

