---
title: Ziel-C-Unterstützung
description: Dieses Dokument enthält eine Beschreibung der Unterstützung für Ziel-C in .net-Einbettung. In diesem Thema werden die automatische Verweis Zählung, NSString, Protokolle, das NSObject-Protokoll, Ausnahmen und mehr erläutert.
ms.prod: xamarin
ms.assetid: 3367A4A4-EC88-4B75-96D0-51B1FCBCE614
author: davidortinau
ms.author: daortin
ms.date: 11/14/2017
ms.openlocfilehash: e72f950dc6fcf12e70714e0fbb996ad5ea432548
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029711"
---
# <a name="objective-c-support"></a>Ziel-C-Unterstützung

## <a name="specific-features"></a>Spezifische Funktionen

Die Generierung von Ziel-C bietet einige besondere Features, die beachtet werden sollten.

### <a name="automatic-reference-counting"></a>Automatische Verweis Zählung

Zum Abrufen der generierten Bindungen ist die Verwendung der automatischen Verweis Zählung (ARC) **erforderlich** . Das Projekt, das eine .net-Einbettungs basierte Bibliothek verwendet, muss mit `-fobjc-arc`kompiliert werden.

### <a name="nsstring-support"></a>NSString-Unterstützung

APIs, die `System.String` Typen verfügbar machen, werden in `NSString`konvertiert. Dies vereinfacht die Speicherverwaltung, als beim Umgang mit `char*`.

### <a name="protocols-support"></a>Unterstützung von Protokollen

Verwaltete Schnittstellen werden in Ziele-C-Protokolle konvertiert, in denen alle Elemente `@required`werden.

### <a name="nsobject-protocol-support"></a>Unterstützung des NSObject-Protokolls

Standardmäßig wird davon ausgegangen, dass die standardmäßige Hashwerte und die Gleichheit von .net und der Ziel-C-Laufzeit austauschbar sind, da Sie eine ähnliche Semantik gemeinsam nutzen.

Wenn ein verwalteter Typ `Equals(Object)` oder `GetHashCode`überschreibt, bedeutet dies im Allgemeinen, dass das Standardverhalten (.net) nicht ausreicht. Dies impliziert, dass das standardmäßige Ziel-C-Verhalten wahrscheinlich nicht ausreicht.

In solchen Fällen überschreibt der Generator die [`isEqual:`](https://developer.apple.com/reference/objectivec/1418956-nsobject/1418795-isequal?language=objc) Methode und [`hash`](https://developer.apple.com/reference/objectivec/1418956-nsobject/1418859-hash?language=objc) Eigenschaft, die im [`NSObject`-Protokoll](https://developer.apple.com/reference/objectivec/1418956-nsobject?language=objc)definiert ist. Dadurch kann die benutzerdefinierte verwaltete Implementierung aus dem Ziel-C-Code transparent verwendet werden.

### <a name="exceptions-support"></a>Unterstützung von Ausnahmen

Wenn Sie `--nativeexception` als Argument an `objcgen` übergeben, werden verwaltete Ausnahmen in Ziel-C-Ausnahmen konvertiert, die abgefangen und verarbeitet werden können. 

### <a name="comparison"></a>Vergleich

Verwaltete Typen, die `IComparable` (oder deren generische Version `IComparable<T>`) implementieren, führen zu benutzerfreundlichen Methoden, die eine `NSComparisonResult` zurückgeben und ein `nil` Argument akzeptieren. Dadurch wird die generierte API für Ziel-C-Entwickler freundlicher. Beispiel:

```objc
- (NSComparisonResult)compare:(XAMComparableType * _Nullable)other;
```

### <a name="categories"></a>Kategorien

Verwaltete Erweiterungs Methoden werden in Kategorien konvertiert. Zum Beispiel die folgenden Erweiterungs Methoden auf `Collection`:

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

Dieser [Artikel](https://nshipster.com/object-subscripting/) ist eine gute Einführung in das Abonnieren von.

## <a name="main-differences-with-net"></a>Wesentliche Unterschiede zu .net

### <a name="constructors-vs-initializers"></a>Konstruktoren im Vergleich zu Initialisierern

In Ziel-C können Sie beliebige initialisiererprototypen aller übergeordneten Klassen in der Vererbungs Kette abrufen, es sei denn, Sie ist als nicht verfügbar markiert (`NS_UNAVAILABLE`).

In C# müssen Sie explizit einen konstruktormember in einer Klasse deklarieren, was bedeutet, dass Konstruktoren nicht geerbt werden.

Um die richtige Darstellung der C# API für "Ziel-C" verfügbar zu machen, wird`NS_UNAVAILABLE`jedem Initialisierer hinzugefügt, der in der untergeordneten Klasse aus der übergeordneten Klasse nicht vorhanden ist.

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

Hier ist `initWithId:` als nicht verfügbar markiert.

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

Im allgemeinen Operator wird `==` C# in als allgemeiner Operator behandelt, wie oben erwähnt.

Wenn jedoch der "Friendly" Gleichheits Operator gefunden wird, werden sowohl der Operator `==` als auch der Operator `!=` bei der Generierung übersprungen.

### <a name="datetime-vs-nsdate"></a>DateTime im Vergleich zu nsdate

Aus der [`NSDate`](https://developer.apple.com/reference/foundation/nsdate?language=objc) -Dokumentation:

> `NSDate` Objekte Kapseln einen einzelnen Zeitpunkt, unabhängig von einem bestimmten Kalendersystem oder einer bestimmten Zeitzone. Date-Objekte sind unveränderlich und stellen ein unveränderliches Zeitintervall in Relation zu einem absoluten Verweis Datum dar (00:00:00 UTC am 1. Januar 2001).

Aufgrund `NSDate` Bezugs Datums müssen alle Konvertierungen zwischen der IT-und der-`DateTime` in UTC ausgeführt werden.

#### <a name="datetime-to-nsdate"></a>DateTime zu nsdate

Beim Umstellen von `DateTime` in `NSDate`wird die Eigenschaft `Kind` in `DateTime` berücksichtigt:

|Art|Ergebnisse|
|---|---|
|`Utc`|Die Konvertierung erfolgt mit dem bereitgestellten `DateTime` Objekt unverändert.|
|`Local`|Das Ergebnis der aufrufenden `ToUniversalTime()` im bereitgestellten `DateTime`-Objekt wird für die Konvertierung verwendet.|
|`Unspecified`|Das angegebene `DateTime` Objekt wird als UTC-Zeit angenommen, das gleiche Verhalten, wenn `Kind` `Utc`ist.|

Die Konvertierung verwendet die folgende Formel:

```
TimeInterval = DateTimeObjectTicks - NSDateReferenceDateTicks / TicksPerSecond
```

In dieser Formel: 

- `NSDateReferenceDateTicks` wird auf der Grundlage des `NSDate` Verweis Datums von 00:00:00 UTC am 1. Januar 2001 berechnet: 

    ```csharp
    new DateTime (year:2001, month:1, day:1, hour:0, minute:0, second:0, kind:DateTimeKind.Utc).Ticks;
    ```

- [`TicksPerSecond`](https://docs.microsoft.com/dotnet/api/system.timespan.tickspersecond) ist für [`TimeSpan`](https://docs.microsoft.com/dotnet/api/system.timespan) definiert.

Zum Erstellen des `NSDate` Objekts wird der `TimeInterval` mit dem `NSDate` [datewithtimeintervalsincereferencedate:](https://developer.apple.com/reference/foundation/nsdate/1591577-datewithtimeintervalsincereferen?language=objc) Selector verwendet.

#### <a name="nsdate-to-datetime"></a>Nsdate in DateTime

Bei der Konvertierung von `NSDate` in `DateTime` wird die folgende Formel verwendet:

```
DateTimeTicks = NSDateTimeIntervalSinceReferenceDate * TicksPerSecond + NSDateReferenceDateTicks
```

In dieser Formel: 

- `NSDateReferenceDateTicks` wird auf der Grundlage des `NSDate` Verweis Datums von 00:00:00 UTC am 1. Januar 2001 berechnet: 

    ```csharp
    new DateTime (year:2001, month:1, day:1, hour:0, minute:0, second:0, kind:DateTimeKind.Utc).Ticks;
    ```

- [`TicksPerSecond`](https://docs.microsoft.com/dotnet/api/system.timespan.tickspersecond) ist für [`TimeSpan`](https://docs.microsoft.com/dotnet/api/system.timespan) definiert.

Nachdem `DateTimeTicks`berechnet wurde, wird der `DateTime`- [Konstruktor](https://docs.microsoft.com/dotnet/api/system.datetime.-ctor?#System_DateTime__ctor_System_Int64_System_DateTimeKind_) aufgerufen, wobei dessen `kind` auf `DateTimeKind.Utc`festgelegt wird.

> [!NOTE]
> `NSDate` können `nil`werden, aber eine `DateTime` ist eine Struktur in .net, die definitionsgemäß nicht `null`werden kann. Wenn Sie ein `nil` `NSDate`geben, wird es in den Standard `DateTime` Wert übersetzt, der `DateTime.MinValue`zugeordnet wird.

`NSDate` unterstützt ein höheres Maximum und einen niedrigeren Minimalwert als `DateTime`. Bei der Konvertierung von `NSDate` in `DateTime` werden diese höheren und niedrigeren Werte in [MinValue](https://docs.microsoft.com/dotnet/api/system.datetime.maxvalue) bzw. [MaxValue](https://docs.microsoft.com/dotnet/api/system.datetime.minvalue) von `DateTime` geändert.
