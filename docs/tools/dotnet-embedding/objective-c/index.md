---
title: "Objective-C-Unterstützung"
ms.topic: article
ms.prod: xamarin
ms.assetid: 3367A4A4-EC88-4B75-96D0-51B1FCBCE614
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: 047f7d7497a114bf4b7c94e50bdf09862b882794
ms.sourcegitcommit: 5fc1c4d17cd9c755604092cf7ff038a6358f8646
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/17/2018
---
# <a name="objective-c-support"></a>Objective-C-Unterstützung

## <a name="specific-features"></a>Spezifische Funktionen

Die Generierung von ObjC hat eine einige spezielle Funktionen, die Folgendes zu beachten sind.

### <a name="automatic-reference-counting"></a>Automatische Verweiszählung

Ist die Verwendung der automatischen Verweis zählen (ARC) **erforderlichen** generierten Bindungen aufrufen. Projekt mithilfe einer Embeddinator-basierten Bibliothek muss kompiliert werden, mit `-fobjc-arc`.

### <a name="nsstring-support"></a>NSString-Unterstützung

APIs, die verfügbar machen `System.String` -Typen werden in konvertiert `NSString`. Dies erleichtert die Verwaltung des Arbeitsspeichers als Umgang mit `char*`.

### <a name="protocols-support"></a>Unterstützung von Protokollen

Verwaltete Schnittstellen werden in ObjC-Protokolle, in dem alle Elemente werden, konvertiert `@required`.

### <a name="nsobject-protocol-support"></a>NSObject protokollunterstützung

Standardmäßig wird angenommen, das Standard-hashing und Gleichheit .net und die Laufzeit ObjC sind gut und austauschbar, wie sie sehr ähnliche Semantik aufweisen.

Wenn ein verwalteter Typ außer Kraft `Equals(Object)` oder `GetHashCode` bedeutet dies im Allgemeinen, dass das Verhalten einer (.NET) wurde nicht das beste aus. Wir können davon ausgehen, dass entweder das Standardverhalten für Objective-C nicht sein würde.

In diesem Fall der Generator überschreibt die [ `isEqual:` ](https://developer.apple.com/reference/objectivec/1418956-nsobject/1418795-isequal?language=objc) Methode und [ `hash` ](https://developer.apple.com/reference/objectivec/1418956-nsobject/1418859-hash?language=objc) -Eigenschaft definiert, der [ `NSObject` Protokoll](https://developer.apple.com/reference/objectivec/1418956-nsobject?language=objc). Dadurch wird die benutzerdefinierte verwaltete Implementierung transparent ObjC Code verwendet werden.

### <a name="comparison"></a>Vergleich

Von verwalteten Typen, die implementieren `IComparable` oder es ist eine generische Version `IComparable<T>` erzeugt ObjC benutzerfreundliche Methoden, die zurückgibt eine `NSComparisonResult` und akzeptieren Sie einen `nil` Argument. Dadurch wird die generierte API benutzerfreundlichere ObjC-Entwicklern, z. B.

```csharp
- (NSComparisonResult)compare:(XAMComparableType * _Nullable)other;
```

### <a name="categories"></a>Kategorien

Verwaltete Erweiterungen aus, die Methoden in Kategorien konvertiert werden. Zum Beispiel die folgenden Erweiterungsmethoden auf `Collection`:

```csharp
    public static class SomeExtensions {

        public static int CountNonNull (this Collection collection) { ... }

        public static int CountNull (this Collection collection) { ... }
    }
```

würde eine Objective-C-Kategorie wie diesen zu erstellen:

```csharp
@interface Collection (SomeExtensions)

- (int)countNonNull;
- (int)countNull;
@end
```

Bei eine einzelnen verwalteten erweitert Typ mehrere Typen an, und dann mehrere Objective-C-Kategorien generiert werden.

### <a name="subscripting"></a>Indizierung 

Verwaltete indizierte Eigenschaften werden in die Indizierung Objekt konvertiert. Zum Beispiel:

```csharp
    public bool this[int index] {
        get { return c[index]; }
        set { c[index] = value; }
    }
```

würden Objective-C ähnelt erstellen:

```csharp
- (id)objectAtIndexedSubscript:(int)idx;
- (void)setObject:(id)obj atIndexedSubscript:(int)idx;
```

die über die subscripting Objective-C-Syntax verwendet werden können:

```csharp
    if ([intCollection [0] isEqual:@42])
        intCollection[0] = @13;
```

Je nach dem Typ der Indexer wird indiziert oder schlüsselgebundenen Indizierung gegebenenfalls generiert.

Dies [Artikel](http://nshipster.com/object-subscripting/) ist eine gute Einführung in die Indizierung.

## <a name="main-differences-with-net"></a>Hauptunterschiede mit .NET

### <a name="constructors-vs-initializers"></a>Konstruktoren und Initialisierer

In Objective-C können Sie jede der Initialisierung Prototypen aller übergeordneten Klassen in der Vererbungskette aufrufen, wenn sie als nicht verfügbar (NS_UNAVAILABLE) markiert ist.

In c# müssen Sie explizit einen Konstruktor Member innerhalb einer Klasse deklarieren, dies bedeutet, dass Konstruktoren nicht geerbt werden.

Um die richtige Darstellung der C#-API für Objective-C-verfügbar zu machen, fügen wir `NS_UNAVAILABLE` auf alle Initialisierer, die nicht in der untergeordneten Klasse von der übergeordneten Klasse vorhanden ist.

C# API:

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

```objectivec
@interface SuperUnique : Unique

- (instancetype)initWithId:(int)id NS_UNAVAILABLE;
- (instancetype)init;

@end
```

Hier sehen wir, dass `initWithId:` als nicht verfügbar markiert wurde.

### <a name="operator"></a>Operator

ObjC unterstützt keine Operator überladen wie c#, sodass Operatoren Klassenselektoren konvertiert werden:

```csharp
    public static AllOperators operator + (AllOperators c1, AllOperators c2)
    {
        return new AllOperators (c1.Value + c2.Value);
    }
```

auf

```csharp
+ (instancetype)add:(Overloads_AllOperators *)anObjectC1 c2:(Overloads_AllOperators *)anObjectC2;
```

Allerdings für einige Sprachen .NET unterstützen keine Operatoren überladen, daher ist es häufig auch ein ["Anzeigename"](https://msdn.microsoft.com/en-us/library/ms229032(v=vs.110).aspx) mit dem Namen-Methode zusätzlich zur operatorüberladung.

Wenn der Operator-Version und die Version "Anzeigename" gefunden werden, nur die angezeigte Version generiert werden, wie sie in den Namen, Objective-c generiert werden.

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

```csharp
+ (instancetype)add:(Overloads_AllOperatorsWithFriendly *)anObjectC1 c2:(Overloads_AllOperatorsWithFriendly *)anObjectC2;
```

### <a name="equality-operator"></a>Gleichheitsoperator

Im Allgemeinen Operator == in c# werden als ein allgemeiner Operator Inhaltslinks oben verarbeitet.

Jedoch wenn "Anzeigename" Gleichheitsoperator gefunden wird, werden beide Operator == und dem Operator! = wird im Bereich der Generation übersprungen werden.

### <a name="datetime-vs-nsdate"></a>"DateTime" Vs NSDate

Von [NSDates](https://developer.apple.com/reference/foundation/nsdate?language=objc) Dokumentation:

> NSDate Objekte kapseln einen einzelnen Fehlerpunkt zeitlich, unabhängig von bestimmten calendrical System oder die Zeitzone an. Date-Objekte sind unveränderlich, eine invariante Zeitintervall relativ zu einem absoluten Verweis Datum darstellt (00: 00:00 UTC am 1. Januar 2001).

Aufgrund von `NSDate` verweisen Datum, alle Konvertierungen zwischen ihm und `DateTime` muss erfolgen in UTC.

#### <a name="datetime-to-nsdate"></a>"DateTime", um NSDate

Beim Konvertieren von `DateTime` auf `NSDate` der "DateTime" `Kind` Eigenschaft berücksichtigt wird.

|Art|Ergebnisse                                                                                            |
|---|---|
|(UTC)|Konvertierung erfolgt unter Verwendung der angegebenen `DateTime` Objekt ist.|
|Lokal|Das Ergebnis des Aufrufs `ToUniversalTime()` im bereitgestellten `DateTime` Objekt für die Konvertierung verwendet wird.|
|Nicht angegeben.|Der bereitgestellte `DateTime` Objekt gilt als (UTC), also dasselbe Verhalten wie eine Art == (UTC).|

Die Konvertierung erfolgt mithilfe der folgenden Formel:

> [!NOTE]
> **TimeInterval** = DateTimeObjectTicks - NSDateReferenceDateTicks [dt] / [TicksPerSecond](https://msdn.microsoft.com/en-us/library/system.timespan.tickspersecond(v=vs.110).aspx)

Nachdem wir die TimeInterval haben wir verwenden des NSDate [DateWithTimeIntervalSinceReferenceDate:](https://developer.apple.com/reference/foundation/nsdate/1591577-datewithtimeintervalsincereferen?language=objc) Selektor, ihn zu erstellen.

#### <a name="nsdate-to-datetime"></a>NSDate in "DateTime"

"DateTime" wird angenommen, wir werden eine NSDate abrufen möchten aus NSDate Instanz Verweis Datum ist **00:00:00 UTC am 1. Januar 2001** und verwenden Sie die folgende Formel:

> [!NOTE]
> **DateTimeTicks** = NSDateTimeIntervalSinceReferenceDate * [TicksPerSecond](https://msdn.microsoft.com/en-us/library/system.timespan.tickspersecond(v=vs.110).aspx) + NSDateReferenceDateTicks [dt]

Sobald wir berechnet die **DateTimeTicks** verwenden wir die folgenden "DateTime" [Konstruktor](https://msdn.microsoft.com/en-us/library/w0d47c9c(v=vs.110).aspx) Einstellung seine `kind` auf `DateTimeKind.Utc`.

Sind einige Aspekte, die Sie kennen müssen, NSDate `nil` aber ein DateTime-Wert ist eine Struktur in .NET und per Definition kann nicht mehr `null`. Wenn Sie geben eine `nil` NSDate wir übersetzt sie in der Standard-DateTime-Wert, der den zuordnet `DateTime.MinValue`.

"MinValue" und "MaxValue" sind ebenfalls unterschiedlich, NSDate größer und untere Grenzen als des "DateTime" unterstützen kann, damit wir, wenn Sie einen Wert größer oder kleiner Geben sie in der "DateTime" festgelegt wird ["MaxValue"](https://msdn.microsoft.com/en-us/library/system.datetime.maxvalue(v=vs.110).aspx) oder ["MinValue"](https://msdn.microsoft.com/en-us/library/system.datetime.minvalue(v=vs.110).aspx) bzw.

**DT**: NSDate Verweis Datum **00:00:00 UTC am 1. Januar 2001** => `new DateTime (year:2001, month:1, day:1, hour:0, minute:0, second:0, kind:DateTimeKind.Utc).Ticks;`
