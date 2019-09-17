---
title: Ziel-C-Unterstützung
description: Dieses Dokument enthält eine Beschreibung der Unterstützung für Ziel-C in .net-Einbettung. In diesem Thema werden die automatische Verweis Zählung, NSString, Protokolle, das NSObject-Protokoll, Ausnahmen und mehr erläutert.
ms.prod: xamarin
ms.assetid: 3367A4A4-EC88-4B75-96D0-51B1FCBCE614
author: conceptdev
ms.author: crdun
ms.date: 11/14/2017
ms.openlocfilehash: 8798e0acf7b1184c64c7012b2f724e2fa7d2c816
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70292764"
---
# <a name="objective-c-support"></a>Ziel-C-Unterstützung

## <a name="specific-features"></a>Bestimmte Features

Die Generierung von Ziel-C bietet einige besondere Features, die beachtet werden sollten.

### <a name="automatic-reference-counting"></a>Automatische Verweis Zählung

Zum Abrufen der generierten Bindungen ist die Verwendung der automatischen Verweis Zählung (ARC) **erforderlich** . Das Projekt, das eine .net-Einbettungs basierte Bibliothek verwendet `-fobjc-arc`, muss mit kompiliert werden.

### <a name="nsstring-support"></a>NSString-Unterstützung

APIs, die `System.String` -Typen verfügbar machen `NSString`, werden in konvertiert. Dies vereinfacht die Speicherverwaltung, als beim Umgang `char*`mit.

### <a name="protocols-support"></a>Unterstützung von Protokollen

Verwaltete Schnittstellen werden in Ziele-C-Protokolle konvertiert, in `@required`denen alle Elemente sind.

### <a name="nsobject-protocol-support"></a>Unterstützung des NSObject-Protokolls

Standardmäßig wird davon ausgegangen, dass die standardmäßige Hashwerte und die Gleichheit von .net und der Ziel-C-Laufzeit austauschbar sind, da Sie eine ähnliche Semantik gemeinsam nutzen.

Wenn ein verwalteter Typ `Equals(Object)` oder `GetHashCode`überschreibt, bedeutet dies im Allgemeinen, dass das Standardverhalten (.net) nicht ausreicht. Dies impliziert, dass das Standardverhalten von "Ziel-C" wahrscheinlich nicht ausreicht.

In solchen Fällen überschreibt der Generator die [`isEqual:`](https://developer.apple.com/reference/objectivec/1418956-nsobject/1418795-isequal?language=objc) im [ `NSObject` Protokoll](https://developer.apple.com/reference/objectivec/1418956-nsobject?language=objc)definierte Methode und [`hash`](https://developer.apple.com/reference/objectivec/1418956-nsobject/1418859-hash?language=objc) Eigenschaft. Dadurch kann die benutzerdefinierte verwaltete Implementierung aus dem Ziel-C-Code transparent verwendet werden.

### <a name="exceptions-support"></a>Unterstützung von Ausnahmen

Wenn `--nativeexception` Sie als Argument an `objcgen` übergeben, werden verwaltete Ausnahmen in Ziel-C-Ausnahmen konvertiert, die abgefangen und verarbeitet werden können. 

### <a name="comparison"></a>Vergleich

Verwaltete Typen, die `IComparable` (oder die generische Version `IComparable<T>`) implementieren, führen zu benutzerfreundlichen Ziel-C- `NSComparisonResult` Methoden, die `nil` einen zurückgeben und ein-Argument akzeptieren. Dadurch wird die generierte API für Ziel-C-Entwickler freundlicher. Beispiel:

```objc
- (NSComparisonResult)compare:(XAMComparableType * _Nullable)other;
```

### <a name="categories"></a>Kategorien

Verwaltete Erweiterungs Methoden werden in Kategorien konvertiert. Beispielsweise die folgenden Erweiterungs Methoden `Collection`für:

```csharp
public static class SomeExtensions {
    public static int CountNonNull (this Collection collection) { ... }
    public static int CountNull (this Collection collection) { ... }
}
```

Erstellen Sie eine Ziel-C-Kategorie wie die folgende:

```objc
@interface Collection (SomeExtensions)

- (int)countNonNull;
- (int)countNull;

@end
```

Wenn ein einzelner verwalteter Typ mehrere Typen erweitert, werden mehrere Ziel-C-Kategorien generiert.

### <a name="subscripting"></a>Indizierung

Verwaltete indizierte Eigenschaften werden in Objekt Abonnements konvertiert. Beispiel:

```csharp
public bool this[int index] {
    get { return c[index]; }
    set { c[index] = value; }
}
```

erstellt "Ziel-C" ähnlich wie:

```objc
- (id)objectAtIndexedSubscript:(int)idx;
- (void)setObject:(id)obj atIndexedSubscript:(int)idx;
```

Diese können mithilfe der Ziel-C-Abonnement Syntax verwendet werden:

```objc
if ([intCollection [0] isEqual:@42])
    intCollection[0] = @13;
```

Abhängig vom Typ Ihres Indexers werden nach Bedarf indizierte oder Schlüssel gebundene Abonnements generiert.

Dieser [Artikel](http://nshipster.com/object-subscripting/) ist eine gute Einführung in das Abonnieren von.

## <a name="main-differences-with-net"></a>Wesentliche Unterschiede zu .net

### <a name="constructors-vs-initializers"></a>Konstruktoren im Vergleich zu Initialisierern

In Ziel-C können Sie beliebige initialisiererprototypen aller übergeordneten Klassen in der Vererbungs Kette abrufen, es sei denn, Sie ist als nicht verfügbar markiert`NS_UNAVAILABLE`().

In C# müssen Sie explizit einen konstruktormember in einer Klasse deklarieren, was bedeutet, dass Konstruktoren nicht geerbt werden.

Um die richtige Darstellung der C# API für "Ziel-C" verfügbar `NS_UNAVAILABLE` zu machen, wird jedem Initialisierer hinzugefügt, der in der untergeordneten Klasse aus der übergeordneten Klasse nicht vorhanden ist.

C#FOUND

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

Ziel-C-API:

```objc
@interface SuperUnique : Unique

- (instancetype)initWithId:(int)id NS_UNAVAILABLE;
- (instancetype)init;

@end
```

`initWithId:` Hier ist als nicht verfügbar markiert.

### <a name="operator"></a>Operator

Ziel-C unterstützt das Überladen von Operatoren nicht wie C# , sodass Operatoren in Klassenselektoren konvertiert werden:

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

Einige .NET-Sprachen unterstützen jedoch nicht das Überladen von Operatoren. Daher ist es üblich, zusätzlich zur Operator Überladung auch eine ["freundliche"](https://docs.microsoft.com/dotnet/standard/design-guidelines/operator-overloads) benannte Methode einzufügen.

Wenn sowohl die Operator Version als auch die "benutzerfreundliche" Version gefunden werden, wird nur die benutzerfreundliche Version generiert, da Sie denselben Ziel-C-Namen generieren.

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

verfügbar

```objc
+ (instancetype)add:(Overloads_AllOperatorsWithFriendly *)anObjectC1 c2:(Overloads_AllOperatorsWithFriendly *)anObjectC2;
```

### <a name="equality-operator"></a>Gleichheits Operator

Im Allgemeinen wird `==` der C# Operator in als allgemeiner Operator behandelt, wie oben erwähnt.

Wenn der "Friendly" Gleichheits Operator gefunden wird, werden der Operator `==` und der `!=` Operator bei der Generierung übersprungen.

### <a name="datetime-vs-nsdate"></a>DateTime im Vergleich zu nsdate

Aus der [`NSDate`](https://developer.apple.com/reference/foundation/nsdate?language=objc) Dokumentation:

> `NSDate`-Objekte Kapseln einen einzelnen Zeitpunkt, unabhängig von einem bestimmten Kalendersystem oder einer bestimmten Zeitzone. Date-Objekte sind unveränderlich und stellen ein unveränderliches Zeitintervall in Relation zu einem absoluten Verweis Datum dar (00:00:00 UTC am 1. Januar 2001).

Aufgrund des `DateTime` Bezugs Datums müssen alle Konvertierungen zwischen diesem und in UTC ausgeführt werden. `NSDate`

#### <a name="datetime-to-nsdate"></a>DateTime zu nsdate

Bei der Umstellung `DateTime` von `NSDate`in wird `Kind` die- `DateTime` Eigenschaft für berücksichtigt:

|Art|Ergebnisse|
|---|---|
|`Utc`|Die Konvertierung erfolgt mit dem bereit `DateTime` gestellten-Objekt unverändert.|
|`Local`|Das Ergebnis des Aufruf `ToUniversalTime()` von im bereit `DateTime` gestellten-Objekt wird für die Konvertierung verwendet.|
|`Unspecified`|Das angegebene `DateTime` Objekt wird als UTC-Zeit angenommen, das gleiche Verhalten `Kind` , `Utc`wenn gleich ist.|

Die Konvertierung verwendet die folgende Formel:

```
TimeInterval = DateTimeObjectTicks - NSDateReferenceDateTicks / TicksPerSecond
```

In dieser Formel: 

- `NSDateReferenceDateTicks`wird basierend auf dem `NSDate` Verweis Datum 00:00:00 UTC am 1. Januar 2001 berechnet: 

    ```csharp
    new DateTime (year:2001, month:1, day:1, hour:0, minute:0, second:0, kind:DateTimeKind.Utc).Ticks;
    ```

- [`TicksPerSecond`](https://docs.microsoft.com/dotnet/api/system.timespan.tickspersecond)ist definiert für[`TimeSpan`](https://docs.microsoft.com/dotnet/api/system.timespan)

Zum Erstellen des `NSDate` -Objekts `TimeInterval` wird mit `NSDate` [datewithtimeintervalsincereferencedate:](https://developer.apple.com/reference/foundation/nsdate/1591577-datewithtimeintervalsincereferen?language=objc) Selector verwendet.

#### <a name="nsdate-to-datetime"></a>Nsdate in DateTime

Die Konvertierung von `NSDate` in `DateTime` verwendet die folgende Formel:

```
DateTimeTicks = NSDateTimeIntervalSinceReferenceDate * TicksPerSecond + NSDateReferenceDateTicks
```

In dieser Formel: 

- `NSDateReferenceDateTicks`wird basierend auf dem `NSDate` Verweis Datum 00:00:00 UTC am 1. Januar 2001 berechnet: 

    ```csharp
    new DateTime (year:2001, month:1, day:1, hour:0, minute:0, second:0, kind:DateTimeKind.Utc).Ticks;
    ```

- [`TicksPerSecond`](https://docs.microsoft.com/dotnet/api/system.timespan.tickspersecond)ist definiert für[`TimeSpan`](https://docs.microsoft.com/dotnet/api/system.timespan)

Nachdem die `DateTimeTicks`Berechnung erfolgt `DateTime` ist, wird `kind` der [Konstruktor](https://docs.microsoft.com/dotnet/api/system.datetime.-ctor?#System_DateTime__ctor_System_Int64_System_DateTimeKind_) aufgerufen, `DateTimeKind.Utc`der auf festgelegt wird.

> [!NOTE]
> `NSDate`kann sein `nil`, aber eine `DateTime` ist eine Struktur in .net, die definitionsgemäß nicht sein `null`kann. Wenn `nil` Sie ein `NSDate`-Wert übergeben, wird dieser in den Standard `DateTime` Wert übersetzt, der zugeordnet `DateTime.MinValue`ist.

`NSDate`unterstützt einen höheren maximalen und einen niedrigeren Minimalwert `DateTime`als. Bei der Konvertierung von `NSDate` in `DateTime` werden diese höheren und niedrigeren Werte in [MinValue](https://docs.microsoft.com/dotnet/api/system.datetime.maxvalue) bzw. [MaxValue](https://docs.microsoft.com/dotnet/api/system.datetime.minvalue) von `DateTime` geändert.
