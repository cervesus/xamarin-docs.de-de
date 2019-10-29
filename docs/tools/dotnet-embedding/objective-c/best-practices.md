---
title: Bewährte Methoden für die .net-Einbettung für Ziel-C
description: In diesem Dokument werden verschiedene bewährte Methoden für die Verwendung der .net-Einbettung mit Ziel-C beschrieben. Es wird erläutert, wie eine Teilmenge des verwalteten Codes verfügbar gemacht wird, und die Bereitstellung einer Segl-API, Benennung und vieles mehr.
ms.prod: xamarin
ms.assetid: 63C7F5D2-8933-4D4A-8348-E9CBDA45C472
author: davidortinau
ms.author: daortin
ms.date: 11/14/2017
ms.openlocfilehash: 2f632e3218d817aa0162a63ea81c61ca18c52b93
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73006771"
---
# <a name="net-embedding-best-practices-for-objective-c"></a>Bewährte Methoden für die .net-Einbettung für Ziel-C

Dies ist ein Entwurf, der möglicherweise nicht mit den Funktionen synchronisiert ist, die derzeit vom Tool unterstützt werden. Wir hoffen, dass sich dieses Dokument separat entwickeln und schließlich mit dem fertigen Tool übereinstimmt, d. h. Wir empfehlen Ihnen, langfristige, nicht unmittelbare Problem Umgehungen zu erzielen.

Ein großer Teil dieses Dokuments gilt auch für andere unterstützte Sprachen. Alle bereitgestellten Beispiele sind jedoch C# in und Ziel-C.

## <a name="exposing-a-subset-of-the-managed-code"></a>Verfügbar machen einer Teilmenge des verwalteten Codes

Die generierte native Bibliothek/das generierte Framework enthält den Ziel-C-Code zum Abrufen der einzelnen verwalteten APIs, die verfügbar gemacht werden. Je mehr API Sie nutzen (öffentlich machen), desto größer wird _die native Verbindungs_ Bibliothek.

Es könnte eine gute Idee sein, eine andere, kleinere Assembly zu erstellen, um nur die erforderlichen APIs für den systemeigenen Entwickler verfügbar zu machen. Diese Fassade ermöglicht Ihnen außerdem mehr Kontrolle über die Sichtbarkeit, Benennung, Fehlerüberprüfung... des generierten Codes.

## <a name="exposing-a-chunkier-api"></a>Verfügbar machen einer Segl-API

Es gibt einen Preis, der für den Übergang vom systemeigenen zum verwalteten (und zurück) zu zahlen ist. Daher ist es besser, segmentierte APIs _anstelle von ' geschwätzige '_ -APIs für Native Entwickler verfügbar zu machen, z. b.

**' Geschwätzige '**

```csharp
public class Person {
  public string FirstName { get; set; }
  public string LastName { get; set; }
}
```

```objc
// this requires 3 calls / transitions to initialize the instance
Person *p = [[Person alloc] init];
p.firstName = @"Sebastien";
p.lastName = @"Pouliot";
```

**Chunky**

```csharp
public class Person {
  public Person (string firstName, string lastName) {}
}
```

```objc
// a single call / transition will perform better
Person *p = [[Person alloc] initWithFirstName:@"Sebastien" lastName:@"Pouliot"];
```

Da die Anzahl der Übergänge kleiner ist, ist die Leistung besser. Außerdem ist es erforderlich, dass weniger Code generiert wird, sodass auch eine kleinere native Bibliothek erstellt wird.

## <a name="naming"></a>Ernannte

Das Benennen von Dingen ist eines von zwei schwierigsten Problemen in der Computerwissenschaft, bei anderen handelt es sich um eine Cache Validierung und um nicht-1-Fehler. Hoffen Sie, dass .net-Einbettung Sie von allen außer den Namen abschirmen kann

### <a name="types"></a>Typen

"Ziel-C" unterstützt keine Namespaces. Im Allgemeinen werden den zugehörigen Typen das Zeichen Präfix 2 (für Apple) oder 3 (für Dritte) vorangestellt, z. b. `UIView` für die UIKit-Ansicht, die das Framework bezeichnet.

Bei .NET-Typen ist das Überspringen des Namespaces nicht möglich, da er doppelte oder verwirrende Namen verursachen kann. Dies macht vorhandene .NET-Typen sehr lang, z. b.

```csharp
namespace Xamarin.Xml.Configuration {
  public class Reader {}
}
```

wird wie folgt verwendet:

```objc
id reader = [[Xamarin_Xml_Configuration_Reader alloc] init];
```

Sie können den Typ jedoch wie folgt wieder verfügbar machen:

```csharp
public class XAMXmlConfigReader : Xamarin.Xml.Configuration.Reader {}
```

die Verwendung der Ziel-C-Benutzerfreundlichkeit, z. b.:

```objc
id reader = [[XAMXmlConfigReader alloc] init];
```

### <a name="methods"></a>Methoden

Selbst gute .net-Namen sind möglicherweise nicht ideal für eine Ziel-C-API.

Benennungs Konventionen in Ziel-C unterscheiden sich von .net (Camel Case anstelle von Pascal Case, more verbose).
Lesen Sie die [Codierungs Richtlinien für Cocoa](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/CodingGuidelines/Articles/NamingMethods.html#//apple_ref/doc/uid/20001282-BCIGIJJF).

Aus Sicht eines Ziel-C-Entwicklers impliziert eine Methode mit einem `Get`-Präfix, dass Sie die Instanz nicht besitzen, d. h. die [Get-Regel](https://developer.apple.com/library/content/documentation/CoreFoundation/Conceptual/CFMemoryMgmt/Concepts/Ownership.html#//apple_ref/doc/uid/20001148-SW1).

Diese Benennungs Regel entspricht nicht der .NET GC-Welt. eine .NET-Methode mit einem `Create`-Präfix verhält sich in .net identisch. Für Ziel-C-Entwickler bedeutet dies jedoch normalerweise, dass Sie die zurückgegebene Instanz besitzen, d. h. die [Create-Regel](https://developer.apple.com/library/content/documentation/CoreFoundation/Conceptual/CFMemoryMgmt/Concepts/Ownership.html#//apple_ref/doc/uid/20001148-103029).

## <a name="exceptions"></a>Ausnahmen

In .net ist es häufig üblich, dass Ausnahmen häufig zum Melden von Fehlern verwendet werden. Sie sind jedoch langsam und in Ziel-C nicht ganz identisch. Wenn möglich, sollten Sie Sie beim Ziel-C-Entwickler ausblenden.

Beispielsweise kann das .net `Try`-Muster viel leichter aus dem Ziel-C-Code verwendet werden:

```csharp
public int Parse (string number)
{
  return Int32.Parse (number);
}
```

Gegensatz

```csharp
public bool TryParse (string number, out int value)
{
  return Int32.TryParse (number, out value);
}
```

### <a name="exceptions-inside-init"></a>Ausnahmen in `init*`

In .NET muss ein Konstruktor entweder erfolgreich sein und eine (_hoffentlich_) gültige Instanz zurückgeben oder eine Ausnahme auslösen.

Im Gegensatz dazu ermöglicht Ziel-C `init*`, `nil` zurückzugeben, wenn keine Instanz erstellt werden kann. Dies ist ein allgemeines, aber nicht allgemeines Muster, das in vielen Frameworks von Apple verwendet wird. In einigen anderen Fällen kann eine `assert` auftreten (und den aktuellen Prozess beenden).

Der Generator folgt dem gleichen `return nil` Muster für generierte `init*` Methoden. Wenn eine verwaltete Ausnahme ausgelöst wird, wird sie gedruckt (mit `NSLog`), und `nil` wird an den Aufrufer zurückgegeben.

## <a name="operators"></a>Operatoren

Ziel-C lässt nicht zu, dass Operatoren wie C# verwendet werden, sodass diese in Klassenselektoren konvertiert werden.

["Friendly"](https://docs.microsoft.com/dotnet/standard/design-guidelines/operator-overloads) benannte Methoden werden im Gegensatz zu den Operator Überladungen generiert, wenn Sie gefunden werden, und können eine leichter zu nutzende API erzeugen.

Klassen, die die Operatoren `==` and\oder `!=` überschreiben, sollten auch die standardmäßige Gleichheits Methode (Object) überschreiben.
