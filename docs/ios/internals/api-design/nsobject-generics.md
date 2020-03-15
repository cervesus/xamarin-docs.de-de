---
title: Generische Unterklassen von NSObject in Xamarin.iOS
description: In diesem Dokument wird das Erstellen generischer Unterklassen von NSObject beschrieben. Es wird untersucht, was möglich ist und was nicht, und es werden der statische Registrar erläutert sowie die Leistung untersucht.
ms.prod: xamarin
ms.assetid: BB99EBD7-308A-C865-1829-4DFFDB1BBCA4
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/21/2017
ms.openlocfilehash: 279fcac1611038613bf442e1b766fda45dd5a429
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2020
ms.locfileid: "73022363"
---
# <a name="generic-subclasses-of-nsobject-in-xamarinios"></a>Generische Unterklassen von NSObject in Xamarin.iOS

## <a name="using-generics-with-nsobjects"></a>Verwenden von Generics mit NSObjects

Es ist möglich, Generics in Unterklassen von `NSObject` zu verwenden, z. B. [UIView-](xref:UIKit.UIView):

```csharp
class Foo<T> : UIView {
    public Foo (CGRect x) : base (x) {}
    public override void Draw (CoreGraphics.CGRect rect)
    {
        Console.WriteLine ("T: {0}. Type: {1}", typeof (T), GetType ().Name);
    }
}
```

Da Objekte dieser Unterklasse `NSObject` mit der Objective-C-Runtime registriert werden, gibt es einige Einschränkungen hinsichtlich der Möglichkeiten, die für generische Unterklassen von `NSObject`-Typen gegeben sind.

## <a name="considerations-for-generic-subclasses-of-nsobject"></a>Überlegungen zu generischen Unterklassen von NSObject

In diesem Dokument werden die Einschränkungen der eingeschränkten Unterstützung für generische Unterklassen von `NSObjects` ausführlich erläutert.

### <a name="generic-type-arguments-in-member-signatures"></a>Generische Typargumente in Membersignaturen

Alle generischen Typargumente in einer Membersignatur, die für Objective-C verfügbar gemacht ist, müssen über eine `NSObject`-Einschränkung verfügen.

**Gut**:

```csharp
class Generic<T> : NSObject where T: NSObject
{
    [Export ("myMethod:")]
    public void MyMethod (T value)
    {
    }
}
```

**Grund:** Der generische Typparameter ist ein `NSObject`, sodass die Selektorsignatur für `myMethod:` sicher für Objective-C verfügbar gemacht werden kann (sie ist immer `NSObject` oder eine Unterklasse davon).

**Schlecht**:

```csharp
class Generic<T> : NSObject
{
    [Export ("myMethod:")]
    public void MyMethod (T value)
    {
    }
}
```

**Grund**: Es ist nicht möglich, eine Objective-C-Signatur für die exportierten Member zu erstellen, die von Objective-C-Code aufgerufen werden können, da die Signatur sich abhängig vom exakten Typ des generischen Typs `T` unterscheiden würde.

**Gut**:

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

**Grund**: Es ist möglich, uneingeschränkte generische Typargumente zu verwenden, solange sie nicht Teil der exportierten Membersignatur sind.

**Gut**:

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

**Grund**: Der `T`-Parameter in der von Objective-C exportierten `MyMethod` ist dahingehend eingeschränkt, dass er nur ein `NSObject` sein kann, während der uneingeschränkte Typ `U` nicht Teil der Signatur ist.

### <a name="instantiations-of-generic-types-from-objective-c"></a>Instanziierungen generischer Typen aus Objective-C

Die Instanziierung generischer Typen aus Objective-C ist nicht zulässig. Dies tritt normalerweise auf, wenn ein verwalteter Typ in einer XIB oder einem Storyboard verwendet wird.

Beachten Sie diese Klassendefinition, die einen Konstruktor verfügbar macht, der einen `IntPtr` akzeptiert (die Art von Xamarin.iOS zur Konstruktion eines C#-Objekts aus einer nativen Objective-C-Instanz):

```csharp
class Generic<T> : NSObject where T : NSObject
{
    public Generic () {}
    public Generic (IntPtr ptr) : base (ptr) {}
}
```

Das obige Konstrukt ist zwar in Ordnung, löst zur Laufzeit aber eine Ausnahme aus, wenn Objective-C versucht, eine Instanz davon zu erstellen.

Dies liegt daran, dass Objective-C kein Konzept generischer Typen besitzt und nicht den genauen generischen Typ angeben kann, der erstellt werden soll.

Sie können dieses Problem umgehen, indem Sie eine spezialisierte Unterklasse des generischen Typs erstellen. Zum Beispiel:

```csharp
class Generic<T> : NSObject where T : NSObject
{
    public Generic () {}
    public Generic (IntPtr ptr) : base (ptr) {}
}

class GenericUIView : Generic<UIView>
{
}
```

Nun liegt keine Mehrdeutigkeit mehr vor, und die Klasse `GenericUIView` kann in XIBs oder Storyboards verwendet werden kann.

## <a name="no-support-for-generic-methods"></a>Keine Unterstützung für generische Methoden

### <a name="generic-methods-are-not-allowed"></a>Generische Methoden sind nicht zulässig.

Der folgende Code lässt sich nicht kompilieren:

```csharp
class MyClass : NSObject
{
    [Export ("myMethod")]
    public void MyMethod<T> (T argument)
    {
    }
}
```

**Grund:** Dies ist nicht zulässig, weil Xamarin.iOS nicht weiß, welcher Typ für das Typargument `T` verwendet werden soll, wenn die Methode aus Objective-C aufgerufen wird.

Eine Alternative besteht darin, eine spezialisierte Methode zu erstellen und Folgendes stattdessen zu exportieren:

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

### <a name="no-exported-static-members-allowed"></a>Keine exportierten statischen Member zulässig

Sie können keine statischen Member für Objective-C verfügbar machen, wenn sie in einer generischen Unterklasse von `NSObject` gehostet werden.

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

**Grund:** Wie bei generischen Methoden muss die Xamarin.iOS-Runtime bestimmen können, welcher Typ für das generische Typargument `T` verwendet werden soll.

Bei Instanzmembern wird die Instanz selbst verwendet (da nie eine Instanz `Generic<T>` vorhanden sein wird, es wird immer eine `Generic<SomeSpecificClass>` sein), aber für statische Member sind diese Informationen nicht vorhanden.

Beachten Sie, dass dies auch dann gilt, wenn der betreffende Member das Typargument `T` in keiner Weise verwendet.

Die Alternative besteht in diesem Fall darin, eine spezialisierte Unterklasse zu erstellen:

```csharp
class GenericUIView : Generic<UIView>
{
    [Export ("myUIViewMethod")]
    public static void MyUIViewMethod ()
    {
        MyMethod ();
    }

    [Export ("myProperty")]
    public static UIView MyUIViewProperty {
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

## <a name="performance"></a>Leistung

Der statische Registrar kann einen exportierten Member zur Buildzeit nicht wie gewöhnlich in einen generischen Typ auflösen, da er zur Laufzeit nachgeschlagen werden muss. Dies bedeutet, dass das Aufrufen einer solchen Methode aus Objective-C ein wenig langsamer ist als das Aufrufen von Membern aus nicht generischen Klassen.
