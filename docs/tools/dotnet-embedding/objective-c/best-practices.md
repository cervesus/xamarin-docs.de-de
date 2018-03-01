---
title: "Embeddinator 4000 bewährte Methoden für ObjC"
ms.topic: article
ms.prod: xamarin
ms.assetid: 932C3F0C-D968-42D1-BB14-D97C73361983
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: 0000cbfdd690b8478b52cbcb84230ed878746daa
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="embeddinator-4000-best-practices-for-objc"></a>Embeddinator 4000 bewährte Methoden für ObjC

Dies ist ein Entwurf und wird möglicherweise nicht synchron mit den Funktionen derzeit unterstützt das Tool. Wir hoffen, dass dieses Dokument wird separat entwickeln und schließlich das endgültige Tool entsprechen, d. h. empfehlen wir langfristig bewährte Vorgehensweisen - keine unmittelbare problemumgehungen.

Ein großer Teil dieses Dokuments gilt auch für andere unterstützten Sprachen. Sind jedoch alle bereitgestellte Beispielen in c# und Objective-C.


# <a name="exposing-a-subset-of-the-managed-code"></a>Verfügbarmachen von eine Teilmenge des verwalteten Codes

Das generierte systemeigene Bibliothek/Framework enthält Objective-C-Code zum Aufrufen der verwalteten APIs, die verfügbar gemacht wird. Die weitere-API, die Sie der Entwurfsoberfläche (öffentlich) dann größer dem systemeigenen _Kleben_ Bibliothek werden.

Es ist möglicherweise eine gute Idee, erstellen Sie eine andere, kleinere Assembly, um nur die erforderlichen APIs für den systemeigenen-Entwickler verfügbar zu machen. Diese Fassade ermöglichen Sie außerdem mehr Kontrolle über die Sichtbarkeit, Benennung, Fehler beim Überprüfen der... des generierten Codes.


# <a name="exposing-a-chunkier-api"></a>Verfügbarmachen von eine übersichtlichere-API

Es gibt ein Preis für den Übergang von systemeigenem zu verwalteten (und zurück). Daher ist es besser, verfügbar machen _anstelle von ' geschwätzige ' segmentierten_ APIs den Zugriff auf die systemeigene Entwickler z. B.

**Chatty**
```
public class Person {
    public string FirstName { get; set; }
    public string LastName { get; set; }
}
```

```csharp
// this requires 3 calls / transitions to initialize the instance
Person *p = [[Person alloc] init];
p.firstName = @"Sebastien";
p.lastName = @"Pouliot";
```

**Chunky**
```
public class Person {
    public Person (string firstName, string lastName) {}
}
```

```csharp
// a single call / transition will perform better
Person *p = [[Person alloc] initWithFirstName:@"Sebastien" lastName:@"Pouliot"];
```

Die Leistung wird besser sein, da die Anzahl der Übergänge kleiner ist. Außerdem erfordert weniger Code generiert werden, damit dieser ebenfalls eine kleinere systemeigene Bibliothek erstellt wird.


# <a name="naming"></a>Benennen von

Benennung ist eines der beiden schwierigsten Probleme in der Informatik, die anderen Caches Ungültigkeit und off-von-1-Fehler. Hoffentlich können .NET einbetten Sie geschützt werden, aus außer benennen.

## <a name="types"></a>Typen

Objective-C unterstützt keine Namespaces. Im Allgemeinen die Typen werden mit dem Präfix 2 (für Apple) oder 3 (für 3. Parteien) Zeichen-Präfix, z. B. `UIView` für die Ansicht des UIKit kennzeichnet die das Framework.

Für Typen von .NET ist überspringen den Namespace nicht möglich, wie es verwirrende, oder doppelte Namen führen kann. Dadurch wird z. B. vorhandene .NET-oder Schematypen sehr lange

```csharp
namespace Xamarin.Xml.Configuration {
    public class Reader {}
}
```

würde z. B. verwendet werden:

```csharp
id reader = [[Xamarin_Xml_Configuration_Reader alloc] init];
```

Jedoch können Sie den Typ als erneut verfügbar machen:

```csharp
public class XAMXmlConfigReader : Xamarin.Xml.Configuration.Reader {}
```

somit weitere Objective-C-freundliche verwenden, z. B.:

```csharp
id reader = [[XAMXmlConfigReader alloc] init];
```

## <a name="methods"></a>Methoden

Auch gute .NET Namen möglicherweise nicht ideal für eine Objective-C-API.

Benennungskonventionen in Objective-C unterscheiden sich von .NET (Camel-Case statt Pascal-Schreibweise, ausführlicher) zur Verfügung.
Lesen Sie die [Codierungsrichtlinien für Kakao](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/CodingGuidelines/Articles/NamingMethods.html#//apple_ref/doc/uid/20001282-BCIGIJJF).

Aus einem Objective-C-Entwickler der Sicht, eine Methode mit einem `Get` Präfix impliziert Sie nicht besitzen die Instanz, d. h. die [Regel abrufen](https://developer.apple.com/library/content/documentation/CoreFoundation/Conceptual/CFMemoryMgmt/Concepts/Ownership.html#//apple_ref/doc/uid/20001148-SW1).

Diese Benennungsregel wurde keine Übereinstimmung in der .NET GC-Welt; eine .NET-Methode übergeben wird mit einem `Create` Präfix wird in .NET Verhalten identisch. Allerdings Objective-C es normalerweise bedeutet, Sie besitzen also die zurückgegebene Instanz der [Regel erstellen](https://developer.apple.com/library/content/documentation/CoreFoundation/Conceptual/CFMemoryMgmt/Concepts/Ownership.html#//apple_ref/doc/uid/20001148-103029).

# <a name="exceptions"></a>Ausnahmen

Es ist ziemlich Commont in .NET Ausnahmen intensiv zum Melden von Fehlern verwendet. Allerdings sind sie langsam und ObjC Recht nicht identisch sind. Nach Möglichkeit sollten Sie diese aus der Objective-C-Entwickler ausblenden.

Beispielsweise .NET `Try` Muster wird viel einfacher aus Objective-C-Code genutzt werden:

```csharp
public int Parse (string number)
{
    return Int32.Parse (number);
}
```

im Vergleich zu

```csharp
public bool TryParse (string number, out int value)
{
    return Int32.TryParse (number, out value);
}
```

## <a name="exceptions-inside-init"></a>Ausnahmen in `init*`

In .NET ein Konstruktor muss entweder erfolgreich ausgeführt werden, und Zurückgeben einer (_hoffentlich_) gültige Instanz oder eine Ausnahme auslösen.

Im Gegensatz dazu ermöglicht das Objective-C `init*` zurückzugebenden `nil` bei eine Instanz kann nicht erstellt werden kann. Dies ist eine häufige, jedoch nicht allgemeine Muster, die in vielen der Apple Frameworks verwendet. In anderen Fällen ein `assert` kann der Fall sein (und brechen Sie den aktuellen Prozess).

Der Generator führen Sie die gleiche `return nil` Muster für generiert `init*` Methoden. Wenn eine verwaltete Ausnahme wird ausgelöst, werden ausgegeben (mit `NSLog`) und `nil` an den Aufrufer zurückgegeben wird.

## <a name="operators"></a>Operatoren

ObjC lässt keine Operatoren überladen werden, wie c#, damit diese Klassenselektoren konvertiert werden.

["Anzeigename"](https://msdn.microsoft.com/en-us/library/ms229032(v=vs.110).aspx) benannte Methode werden anstelle der operatorüberladungen generiert Wenn gefunden, und eine einfachere API nutzen, liefern.

Klassen, die die Operatoren außer Kraft setzen und/oder ==! = sollten der standard ist gleich (Objekt)-Methode ebenfalls überschreiben.
