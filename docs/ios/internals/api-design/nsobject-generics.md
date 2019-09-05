---
title: Generische Unterklassen von NSObject in xamarin. IOS
description: In diesem Dokument wird das Erstellen generischer Unterklassen von NSObject beschrieben. Es wird untersucht, was möglich ist und was nicht, und erläutert die statische Registrierungsstelle und berücksichtigt die Leistung.
ms.prod: xamarin
ms.assetid: BB99EBD7-308A-C865-1829-4DFFDB1BBCA4
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/21/2017
ms.openlocfilehash: 531d0b2b6141cc0e1f4014f1d3422af3c6f8643a
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70291980"
---
# <a name="generic-subclasses-of-nsobject-in-xamarinios"></a>Generische Unterklassen von NSObject in xamarin. IOS

## <a name="using-generics-with-nsobjects"></a>Verwenden von Generika mit NSObjects

Es ist möglich, Generika in Unterklassen von `NSObject`, z. b. [UIView](xref:UIKit.UIView), zu verwenden:

```csharp
class Foo<T> : UIView {
    public Foo (CGRect x) : base (x) {}
    public override void Draw (CoreGraphics.CGRect rect)
    {
        Console.WriteLine ("T: {0}. Type: {1}", typeof (T), GetType ().Name);
    }
}
```

Da Objekte, die unter `NSObject` Klassen bei der Ziel-C-Laufzeit registriert sind, einige Einschränkungen hinsichtlich der möglichen Möglichkeiten mit generischen Unterklassen `NSObject` von Typen aufweisen.

## <a name="considerations-for-generic-subclasses-of-nsobject"></a>Überlegungen zu generischen Unterklassen von NSObject

In diesem Dokument werden die Einschränkungen der eingeschränkten Unterstützung für generische Unterklassen `NSObjects`von ausführlich erläutert.

### <a name="generic-type-arguments-in-member-signatures"></a>Generische Typargumente in Element Signaturen

Alle generischen Typargumente in einer für "Ziel-C" verfügbar `NSObject` gemachten Element Signatur müssen über eine Einschränkung verfügen.

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

**Grund**: Der generische Typparameter ist ein `NSObject`, sodass die Auswahl Signatur für `myMethod:` sicher für "Ziel-C" verfügbar gemacht werden kann (es `NSObject` wird immer oder eine Unterklasse davon sein).

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

**Grund**: Es ist nicht möglich, eine Ziel-c-Signatur für die exportierten Elemente zu erstellen, die von Ziel-c-Code aufgerufen werden können, da die Signatur sich abhängig vom exakten `T`Typ des generischen Typs unterscheiden würde.

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

**Grund**: Es ist möglich, dass nicht eingeschränkte generische Typargumente vorhanden sind, solange Sie nicht Teil der Signatur des exportierten Elements sind.

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

**Grund**: der `T` -Parameter im Ziel-C- `MyMethod` Export ist auf einen `NSObject`beschränkt, der nicht eingeschränkte Typ `U` ist nicht Teil der Signatur.

### <a name="instantiations-of-generic-types-from-objective-c"></a>Instanziierungen generischer Typen aus Ziel-C

Die Instanziierung von generischen Typen aus Ziel-C ist nicht zulässig. Dies tritt normalerweise auf, wenn ein verwalteter Typ in einem XIb oder einem Storyboard verwendet wird.

Beachten Sie diese Klassendefinition, die einen Konstruktor verfügbar macht, `IntPtr` der einen-Wert annimmt (die xamarin. C# IOS-Methode, um ein-Objekt aus einer nativen Ziel-C-Instanz zu erstellen):

```csharp
class Generic<T> : NSObject where T : NSObject
{
    public Generic () {}
    public Generic (IntPtr ptr) : base (ptr) {}
}
```

Obwohl das obige Konstrukt in Ordnung ist, löst dies zur Laufzeit eine Ausnahme aus, wenn Ziel-C versucht, eine Instanz davon zu erstellen.

Dies liegt daran, dass Ziel-C kein Konzept generischer Typen hat und nicht den genauen generischen Typ angeben kann, der erstellt werden soll.

Sie können dieses Problem umgehen, indem Sie eine spezialisierte Unterklasse des generischen Typs erstellen. Beispiel:

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

Jetzt gibt es keine Mehrdeutigkeit mehr, da `GenericUIView` die Klasse in XIb-oder Storyboards verwendet werden kann.

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

**Grund**: Dies ist nicht zulässig, da xamarin. IOS nicht weiß, welcher Typ für das Typargument `T` verwendet werden soll, wenn die Methode von Ziel-C aufgerufen wird.

Eine Alternative besteht darin, eine spezialisierte Methode zu erstellen und stattdessen zu exportieren:

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

### <a name="no-exported-static-members-allowed"></a>Keine exportierten statischen Member zulässig.

Sie können keine statischen Member für "Ziel-C" verfügbar machen, wenn Sie in einer generischen unter `NSObject`Klasse von gehostet wird.

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

**Weshalb** Ebenso wie generische Methoden muss die xamarin. IOS-Laufzeit wissen, welcher Typ für das generische Typargument `T`verwendet werden muss.

Für Instanzmember wird die Instanz selbst verwendet (da nie eine Instanz `Generic<T>`vorhanden ist, Sie ist `Generic<SomeSpecificClass>`immer), aber für statische Member sind diese Informationen nicht vorhanden.

Beachten Sie, dass dies auch dann gilt, wenn der betreffende Member das Typargument `T` nicht verwendet.

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

Die statische Registrierungsstelle kann einen exportierten Member bei der Buildzeit nicht in einem generischen Typ auflösen, da er in der Regel zur Laufzeit gesucht werden muss. Dies bedeutet, dass das Aufrufen einer solchen Methode von Ziel-C etwas langsamer ist als das Aufrufen von Membern aus nicht generischen Klassen.

