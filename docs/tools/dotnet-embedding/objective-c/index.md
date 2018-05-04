---
title: Objective-C-Unterstützung
ms.prod: xamarin
ms.assetid: 3367A4A4-EC88-4B75-96D0-51B1FCBCE614
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: 2a4a235dcb885364fdc5add5970e61f46b6e5d08
ms.sourcegitcommit: 4b0582a0f06598f3ff8ad5b817946459fed3c42a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/03/2018
---
# <a name="objective-c-support"></a>Objective-C-Unterstützung

## <a name="specific-features"></a>Spezifische Funktionen

Die Generierung von Objective-C verfügt über einige spezielle Features, die Folgendes zu beachten sind.

### <a name="automatic-reference-counting"></a>Automatische Verweiszählung

Ist die Verwendung der automatischen Verweis zählen (ARC) **erforderlichen** generierten Bindungen aufrufen. Projekt mithilfe einer Bibliothek einbetten .NET basierende muss kompiliert werden, mit `-fobjc-arc`.

### <a name="nsstring-support"></a>NSString-Unterstützung

APIs, die verfügbar machen `System.String` -Typen werden in konvertiert `NSString`. Dies erleichtert die Verwaltung des Arbeitsspeichers als beim Umgang mit `char*`.

### <a name="protocols-support"></a>Unterstützung von Protokollen

Verwaltete Schnittstellen werden in Objective-C-Protokolle, in dem alle Elemente werden, konvertiert `@required`.

### <a name="nsobject-protocol-support"></a>NSObject protokollunterstützung

Standardmäßig werden standardmäßig hashing und Gleichheit .NET und Objective-C-Laufzeit davon ausgegangen, dass untereinander austauschbar, sein, wie sie ähnliche Semantik aufweisen.

Wenn ein verwalteter Typ außer Kraft `Equals(Object)` oder `GetHashCode`, in der Regel bedeutet das, dass das Standardverhalten (.NET) nicht ausreichend war; dies bedeutet, dass das Standardverhalten für Objective-C wahrscheinlich ist nicht ausreichend entweder.

In solchen Fällen der Generator überschreibt die [ `isEqual:` ](https://developer.apple.com/reference/objectivec/1418956-nsobject/1418795-isequal?language=objc) Methode und [ `hash` ](https://developer.apple.com/reference/objectivec/1418956-nsobject/1418859-hash?language=objc) -Eigenschaft definiert, der [ `NSObject` Protokoll](https://developer.apple.com/reference/objectivec/1418956-nsobject?language=objc). Dadurch wird die benutzerdefinierte verwaltete Implementierung transparent Objective-C-Code verwendet werden.

### <a name="exceptions-support"></a>Unterstützung von Ausnahmen

Übergeben von `--nativeexception` als Argument an `objcgen` konvertiert verwaltete Ausnahmen in Objective-C-Ausnahmen, die abgefangen und verarbeitet werden können. 

### <a name="comparison"></a>Vergleich

Von verwalteten Typen, die implementieren `IComparable` (oder die generische Version `IComparable<T>`) erzeugt Objective-C benutzerfreundliche Methoden, mit denen eine `NSComparisonResult` und akzeptieren eine `nil` Argument. Dadurch wird die generierte API benutzerfreundlichere für Objective-C-Entwickler. Zum Beispiel:

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

würde eine Objective-C-Kategorie wie diesen zu erstellen:

```objc
@interface Collection (SomeExtensions)

- (int)countNonNull;
- (int)countNull;

@end
```

Wenn ein einzelnes verwaltetes Typs mehrere Typen erweitert, werden mehrere Objective-C-Kategorien generiert.

### <a name="subscripting"></a>Indizierung 

Verwaltete indizierte Eigenschaften werden in die Indizierung Objekt konvertiert. Zum Beispiel:

```csharp
public bool this[int index] {
    get { return c[index]; }
    set { c[index] = value; }
}
```

würden Objective-C ähnelt erstellen:

```objc
- (id)objectAtIndexedSubscript:(int)idx;
- (void)setObject:(id)obj atIndexedSubscript:(int)idx;
```

die über die subscripting Objective-C-Syntax verwendet werden können:

```objc
if ([intCollection [0] isEqual:@42])
    intCollection[0] = @13;
```

Je nach dem Typ der Indexer wird indiziert oder schlüsselgebundenen Indizierung gegebenenfalls generiert.

Dies [Artikel](http://nshipster.com/object-subscripting/) ist eine gute Einführung in die Indizierung.

## <a name="main-differences-with-net"></a>Hauptunterschiede mit .NET

### <a name="constructors-vs-initializers"></a>Konstruktoren und Initialisierer

In Objective-C, können Sie aufrufen keines der Initialisierer Prototypen aller übergeordneten Klassen in der Vererbungskette, es sei denn, sie als nicht verfügbar markiert ist (`NS_UNAVAILABLE`).

In c# müssen Sie explizit einen Konstruktor Member innerhalb einer Klasse deklarieren, dies bedeutet, dass Konstruktoren nicht geerbt werden.

Um die richtige Darstellung der C#-API für Objective-C-verfügbar zu machen `NS_UNAVAILABLE` wurde Initialisierer, die nicht in der untergeordneten Klasse von der übergeordneten Klasse vorhanden ist.

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

Objective-C eingeblendet API:

```objc
@interface SuperUnique : Unique

- (instancetype)initWithId:(int)id NS_UNAVAILABLE;
- (instancetype)init;

@end
```

Hier `initWithId:` als nicht verfügbar markiert wurde.

### <a name="operator"></a>Operator

Objective-C unterstützt keine Operator überladen wie c#, sodass Operatoren Klassenselektoren konvertiert werden:

```csharp
public static AllOperators operator + (AllOperators c1, AllOperators c2)
{
    return new AllOperators (c1.Value + c2.Value);
}
```

auf

```objc
+ (instancetype)add:(Overloads_AllOperators *)anObjectC1 c2:(Overloads_AllOperators *)anObjectC2;
```

Allerdings für einige Sprachen .NET unterstützen keine Operatoren überladen, daher ist es häufig auch ein ["Anzeigename"](https://docs.microsoft.com/dotnet/standard/design-guidelines/operator-overloads) mit dem Namen-Methode zusätzlich zur operatorüberladung.

Wenn der Operator-Version und die Version "Anzeigename" gefunden werden, nur die angezeigte Version generiert werden, wie sie in den Namen, Objective-C generiert werden.

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

### <a name="equality-operator"></a>Gleichheitsoperator

Im Allgemeinen Operator `==` in c# erfolgt als eine allgemeine Operator wie oben bereits erwähnt.

Jedoch wenn "Anzeigename" Gleichheitsoperator gefunden wird, werden beide Operator `==` and -Operator `!=` Generation übersprungen werden.

### <a name="datetime-vs-nsdate"></a>"DateTime" Vs NSDate

Aus der [ `NSDate` ](https://developer.apple.com/reference/foundation/nsdate?language=objc) Dokumentation:

> `NSDate` Objekte kapseln einen einzelnen Fehlerpunkt zeitlich, unabhängig von bestimmten calendrical System oder die Zeitzone an. Date-Objekte sind unveränderlich, eine invariante Zeitintervall relativ zu einem absoluten Verweis Datum darstellt (00: 00:00 UTC am 1. Januar 2001).

Aufgrund von `NSDate` verweisen Datum, alle Konvertierungen zwischen ihm und `DateTime` muss erfolgen in UTC.

#### <a name="datetime-to-nsdate"></a>"DateTime", um NSDate

Beim Konvertieren von `DateTime` auf `NSDate`, die `Kind` Eigenschaft `DateTime` wird berücksichtigt:

|Art|Ergebnisse|
|---|---|
|`Utc`|Konvertierung erfolgt unter Verwendung der angegebenen `DateTime` Objekt ist.|
|`Local`|Das Ergebnis des Aufrufs `ToUniversalTime()` im bereitgestellten `DateTime` Objekt für die Konvertierung verwendet wird.|
|`Unspecified`|Der bereitgestellte `DateTime` Objekt gilt als (UTC), also dasselbe Verhalten beim `Kind` ist `Utc`.|

Die Konvertierung wird die folgende Formel verwendet:

```
TimeInterval = DateTimeObjectTicks - NSDateReferenceDateTicks / TicksPerSecond
```

In der folgenden Formel: 

- `NSDateReferenceDateTicks` ist berechnet auf Grundlage der `NSDate` Datum von 00:00:00 UTC am 1. Januar 2001 verweisen: 
    ```csharp
    new DateTime (year:2001, month:1, day:1, hour:0, minute:0, second:0, kind:DateTimeKind.Utc).Ticks;
    ```
- [`TicksPerSecond`](https://docs.microsoft.com/dotnet/api/system.timespan.tickspersecond) wird auf definiert. [`TimeSpan`](https://docs.microsoft.com/dotnet/api/system.timespan)

Zum Erstellen der `NSDate` -Objekt, das `TimeInterval` wird verwendet, mit der `NSDate` [DateWithTimeIntervalSinceReferenceDate:](https://developer.apple.com/reference/foundation/nsdate/1591577-datewithtimeintervalsincereferen?language=objc) Selektor.

#### <a name="nsdate-to-datetime"></a>NSDate in "DateTime"

Die Konvertierung von `NSDate` auf `DateTime` verwendet die folgende Formel:

```
DateTimeTicks = NSDateTimeIntervalSinceReferenceDate * TicksPerSecond + NSDateReferenceDateTicks
```

In der folgenden Formel: 

- `NSDateReferenceDateTicks` ist berechnet auf Grundlage der `NSDate` Datum von 00:00:00 UTC am 1. Januar 2001 verweisen: 
    ```csharp
    new DateTime (year:2001, month:1, day:1, hour:0, minute:0, second:0, kind:DateTimeKind.Utc).Ticks;
    ```
- [`TicksPerSecond`](https://docs.microsoft.com/dotnet/api/system.timespan.tickspersecond) wird auf definiert. [`TimeSpan`](https://docs.microsoft.com/dotnet/api/system.timespan)

Nach der Berechnung `DateTimeTicks`, `DateTime` [Konstruktor](https://docs.microsoft.com/dotnet/api/system.datetime.-ctor?#System_DateTime__ctor_System_Int64_System_DateTimeKind_) aufgerufen wird, wird die Einstellung der `kind` auf `DateTimeKind.Utc`.

> [!NOTE]
> `NSDate` kann `nil`, aber ein `DateTime` eine Struktur in .NET, das definitionsgemäß nicht möglich ist `null`. Wenn Sie geben eine `nil` `NSDate`, wird übersetzt werden, auf den Standardwert `DateTime` -Wert, der zugeordnet `DateTime.MinValue`.

`NSDate` unterstützt eine höhere maximale und minimale Wert niedriger `DateTime`. Beim Konvertieren von `NSDate` auf `DateTime`, diese höheren und niedrigeren Werte werden geändert, um die `DateTime` ["MaxValue"](https://docs.microsoft.com/dotnet/api/system.datetime.maxvalue) oder ["MinValue"](https://docs.microsoft.com/dotnet/api/system.datetime.minvalue)bzw.
