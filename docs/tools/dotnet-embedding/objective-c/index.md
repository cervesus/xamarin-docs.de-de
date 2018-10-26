---
title: Objective-C-Unterstützung
description: Dieses Dokument enthält eine Beschreibung der Unterstützung für Objective-C, in das Einbetten von .NET. Es wird erläutert, automatische Verweiszählung, NSString, Protokolle, NSObject-Protokoll, Ausnahmen und vieles mehr.
ms.prod: xamarin
ms.assetid: 3367A4A4-EC88-4B75-96D0-51B1FCBCE614
author: lobrien
ms.author: laobri
ms.date: 11/14/2017
ms.openlocfilehash: 48caa70cf2bd408f8afc673b400f7d5a4369e108
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50110718"
---
# <a name="objective-c-support"></a>Objective-C-Unterstützung

## <a name="specific-features"></a>Bestimmte Funktionen

Die Generierung von Objective-C verfügt über einige spezielle Funktionen, die beachtet sollten werden.

### <a name="automatic-reference-counting"></a>Automatische Verweiszählung

Die Verwendung der automatische Verweis gezählt (Bogen) ist **erforderlichen** aufrufen, die generierten Bindungen. Projekt mit einer Bibliothek Einbetten von .NET basierende muss kompiliert werden, mit `-fobjc-arc`.

### <a name="nsstring-support"></a>NSString-Unterstützung

APIs, verfügbar machen `System.String` Typen werden in konvertiert `NSString`. Dies erleichtert die Verwaltung des Arbeitsspeichers als beim Umgang mit `char*`.

### <a name="protocols-support"></a>Protokolle zu unterstützen.

Verwaltete Schnittstellen werden in Objective-C-Protokolle, in dem alle Elemente werden, konvertiert `@required`.

### <a name="nsobject-protocol-support"></a>Unterstützung von NSObject-Protokolls

In der Standardeinstellung die standardmäßige hashing und Gleichheit sowohl .NET-als auch die Objective-C-Laufzeit wird angenommen, dass austauschbar, ähnliche Semantik wie Teilen.

Wenn ein verwalteter Typ außer Kraft `Equals(Object)` oder `GetHashCode`, es wird in der Regel bedeutet, dass das Standardverhalten (.NET) nicht ausreichend war; dies bedeutet, dass das Standardverhalten für Objective-C wahrscheinlich ist nicht genügend entweder.

In solchen Fällen der Generator überschreibt die [ `isEqual:` ](https://developer.apple.com/reference/objectivec/1418956-nsobject/1418795-isequal?language=objc) Methode und [ `hash` ](https://developer.apple.com/reference/objectivec/1418956-nsobject/1418859-hash?language=objc) -Eigenschaft definiert, der [ `NSObject` Protokoll](https://developer.apple.com/reference/objectivec/1418956-nsobject?language=objc). Dadurch wird die benutzerdefinierte verwaltete Implementierung transparent aus Objective-C-Code verwendet werden.

### <a name="exceptions-support"></a>Unterstützung von Ausnahmen

Übergeben von `--nativeexception` als Argument an `objcgen` konvertiert verwaltete Ausnahmen in Objective-C-Ausnahmen, die abgefangen und verarbeitet werden können. 

### <a name="comparison"></a>Vergleich

Verwaltete Typen, die implementieren `IComparable` (oder die generische Version `IComparable<T>`) erzeugt benutzerfreundliche Objective-C-Methoden, die zurückgeben eine `NSComparisonResult` und akzeptieren Sie ein `nil` Argument. Dadurch wird die generierte API benutzerfreundlichere für Objective-C-Entwickler. Zum Beispiel:

```objc
- (NSComparisonResult)compare:(XAMComparableType * _Nullable)other;
```

### <a name="categories"></a>Kategorien

Verwaltete Erweiterungen aus, die Methoden in Kategorien konvertiert werden. Z. B. die folgenden Erweiterungsmethoden auf `Collection`:

```csharp
public static class SomeExtensions {
    public static int CountNonNull (this Collection collection) { ... }
    public static int CountNull (this Collection collection) { ... }
}
```

Erstellen Sie eine Objective-C-Kategorie wie dieser würde:

```objc
@interface Collection (SomeExtensions)

- (int)countNonNull;
- (int)countNull;

@end
```

Wenn mehrere Typen von ein einzelnen verwalteter Typ erweitert wird, werden mehrere Objective-C-Kategorien generiert.

### <a name="subscripting"></a>Indizierung 

Verwaltete indizierte Eigenschaften werden in Objekt-Indizierung konvertiert. Zum Beispiel:

```csharp
public bool this[int index] {
    get { return c[index]; }
    set { c[index] = value; }
}
```

Objective-C würde wie erstellen:

```objc
- (id)objectAtIndexedSubscript:(int)idx;
- (void)setObject:(id)obj atIndexedSubscript:(int)idx;
```

die über die subscripting Objective-C-Syntax verwendet werden kann:

```objc
if ([intCollection [0] isEqual:@42])
    intCollection[0] = @13;
```

Je nach Art der der Indexer werden indiziert oder schlüsselgebundene Indizierung gegebenenfalls generiert.

Dies [Artikel](http://nshipster.com/object-subscripting/) ist eine hervorragende Einführung in die Indizierung.

## <a name="main-differences-with-net"></a>Hauptunterschiede mit .NET

### <a name="constructors-vs-initializers"></a>Konstruktoren und Initialisierer

In Objective-C können Sie Aufrufen eines der Initialisierer Prototypen von der übergeordneten Klassen in der Vererbungskette, es sei denn, sie als nicht verfügbar markiert ist (`NS_UNAVAILABLE`).

In C# müssen Sie explizit deklarieren, ein Konstruktor-Element innerhalb einer Klasse, die bedeutet, dass Konstruktoren nicht geerbt.

Um die richtige Darstellung des verfügbar zu machen die C# Objective-C-API `NS_UNAVAILABLE` wurde Initialisierer, der nicht in der untergeordneten Klasse von der übergeordneten Klasse vorhanden ist.

C#-API:

```csharp
public class Unique {
    public Unique () : this (1)
    {
    }

    public Unique (int id)
    {
    }
}

public class SuperUnique : Unique {
    public SuperUnique () : base (911)
    {
    }
}
```

Objective-C-eingeblendet API:

```objc
@interface SuperUnique : Unique

- (instancetype)initWithId:(int)id NS_UNAVAILABLE;
- (instancetype)init;

@end
```

Hier `initWithId:` wurde als nicht verfügbar markiert.

### <a name="operator"></a>Operator

Objective-C unterstützt keine operatorüberladung als C# ausgeführt, sodass Operatoren Klassenselektoren konvertiert werden:

```csharp
public static AllOperators operator + (AllOperators c1, AllOperators c2)
{
    return new AllOperators (c1.Value + c2.Value);
}
```

bis

```objc
+ (instancetype)add:(Overloads_AllOperators *)anObjectC1 c2:(Overloads_AllOperators *)anObjectC2;
```

Allerdings einige Sprachen für .NET unterstützen keine operatorüberladung, daher ist es üblich, die auch eine ["einfachen"](https://docs.microsoft.com/dotnet/standard/design-guidelines/operator-overloads) Methode zusätzlich zu den operatorüberladung.

Wenn sowohl die Operator-Version als auch die "einfachen" Version gefunden werden, nur die angezeigte Version generiert werden, wie sie in den Namen, Objective-C generiert werden.

```csharp
public static AllOperatorsWithFriendly operator + (AllOperatorsWithFriendly c1, AllOperatorsWithFriendly c2)
{
    return new AllOperatorsWithFriendly (c1.Value + c2.Value);
}

public static AllOperatorsWithFriendly Add (AllOperatorsWithFriendly c1, AllOperatorsWithFriendly c2)
{
    return new AllOperatorsWithFriendly (c1.Value + c2.Value);
}
```

wird:

```objc
+ (instancetype)add:(Overloads_AllOperatorsWithFriendly *)anObjectC1 c2:(Overloads_AllOperatorsWithFriendly *)anObjectC2;
```

### <a name="equality-operator"></a>Equality-operator

Im Allgemeinen Operator `==` in C# als allgemeine Operator wie bereits erwähnt oben erfolgt.

Jedoch wenn Sie der "einfachen" Equals-Operator gefunden wird, werden beide Operator `==` and -Operator `!=` Generation übersprungen werden.

### <a name="datetime-vs-nsdate"></a>"DateTime" Vs NSDate

Von der [ `NSDate` ](https://developer.apple.com/reference/foundation/nsdate?language=objc) Dokumentation:

> `NSDate` Objekte werden nur einen rechtzeitig, unabhängig von bestimmten calendrical System oder Zeitzone kapseln. Datumsobjekte sind unveränderlich, eine invariante Zeitintervall relativ zu einem absoluten Verweis Datum darstellt (00: 00:00 UTC am 1. Januar 2001).

Aufgrund von `NSDate` verweisen Datum, alle Konvertierungen zwischen ihm und `DateTime` muss in UTC ausgeführt werden.

#### <a name="datetime-to-nsdate"></a>DateTime-Wert zum NSDate

Beim Konvertieren von `DateTime` zu `NSDate`, `Kind` Eigenschaft `DateTime` berücksichtigt wird:

|Art|Ergebnisse|
|---|---|
|`Utc`|Konvertierung mit dem bereitgestellten `DateTime` Objekts ist.|
|`Local`|Das Ergebnis des Aufrufs `ToUniversalTime()` im bereitgestellten `DateTime` Objekt für die Konvertierung verwendet wird.|
|`Unspecified`|Der bereitgestellte `DateTime` Objekt wird als UTC, sodass dasselbe Verhalten beim `Kind` ist `Utc`.|

Die Konvertierung wird die folgende Formel verwendet:

```
TimeInterval = DateTimeObjectTicks - NSDateReferenceDateTicks / TicksPerSecond
```

In dieser Formel: 

- `NSDateReferenceDateTicks` wird basierend auf berechnet die `NSDate` Datum von 00:00:00 UTC am 1. Januar 2001 verweisen: 
    ```csharp
    new DateTime (year:2001, month:1, day:1, hour:0, minute:0, second:0, kind:DateTimeKind.Utc).Ticks;
    ```
- [`TicksPerSecond`](https://docs.microsoft.com/dotnet/api/system.timespan.tickspersecond) wird auf definiert. [`TimeSpan`](https://docs.microsoft.com/dotnet/api/system.timespan)

Zum Erstellen der `NSDate` -Objekt, das `TimeInterval` wird zusammen mit der `NSDate` [DateWithTimeIntervalSinceReferenceDate:](https://developer.apple.com/reference/foundation/nsdate/1591577-datewithtimeintervalsincereferen?language=objc) Selektor.

#### <a name="nsdate-to-datetime"></a>NSDate "DateTime"

Die Konvertierung von `NSDate` zu `DateTime` verwendet die folgende Formel:

```
DateTimeTicks = NSDateTimeIntervalSinceReferenceDate * TicksPerSecond + NSDateReferenceDateTicks
```

In dieser Formel: 

- `NSDateReferenceDateTicks` wird basierend auf berechnet die `NSDate` Datum von 00:00:00 UTC am 1. Januar 2001 verweisen: 
    ```csharp
    new DateTime (year:2001, month:1, day:1, hour:0, minute:0, second:0, kind:DateTimeKind.Utc).Ticks;
    ```
- [`TicksPerSecond`](https://docs.microsoft.com/dotnet/api/system.timespan.tickspersecond) wird auf definiert. [`TimeSpan`](https://docs.microsoft.com/dotnet/api/system.timespan)

Nach der Berechnung `DateTimeTicks`, `DateTime` [Konstruktor](https://docs.microsoft.com/dotnet/api/system.datetime.-ctor?#System_DateTime__ctor_System_Int64_System_DateTimeKind_) aufgerufen wird, wird die Einstellung der `kind` zu `DateTimeKind.Utc`.

> [!NOTE]
> `NSDate` kann `nil`, aber ein `DateTime` ist eine Struktur in .NET werden gemäß der Definition darf nicht sein `null`. Wenn Sie geben eine `nil` `NSDate`, es übersetzt werden auf den Standardwert `DateTime` -Wert, der zuordnet `DateTime.MinValue`.

`NSDate` unterstützt einen höheren Maximalwert als auch einen minimalen niedrigeren Wert als `DateTime`. Beim Konvertieren von `NSDate` zu `DateTime`, diese höhere und niedrigere Werte werden geändert, um die `DateTime` [MaxValue](https://docs.microsoft.com/dotnet/api/system.datetime.maxvalue) oder [MinValue](https://docs.microsoft.com/dotnet/api/system.datetime.minvalue)bzw.
