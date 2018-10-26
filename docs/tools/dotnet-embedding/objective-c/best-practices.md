---
title: Einbetten von .NET bewährte Methoden für Objective-C
description: Dieses Dokument beschreibt die verschiedenen best Practices für die Verwendung von Einbetten von .NET mit Objective-c Es wird erläutert, eine Teilmenge von verwaltetem Code verfügbar zu machen, eine übersichtlichere API verfügbar zu machen, benennen und mehr.
ms.prod: xamarin
ms.assetid: 63C7F5D2-8933-4D4A-8348-E9CBDA45C472
author: lobrien
ms.author: laobri
ms.date: 11/14/2017
ms.openlocfilehash: 33138b7858b8bc04a5be30f9fad1709e916f5575
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50105394"
---
# <a name="net-embedding-best-practices-for-objective-c"></a>Einbetten von .NET bewährte Methoden für Objective-C

Dies ist ein Entwurf, und möglicherweise nicht synchron mit den Funktionen derzeit unterstützt durch das Tool. Wir hoffen, dass in diesem Dokument wird separat weiterentwickelt und schließlich auswählen der endgültigen Tools, d. h. werden empfohlen langfristig bewährte Ansätze – nicht problemumgehungen.

Ein großer Teil dieses Dokuments gilt auch für andere unterstützte Sprachen. Jedoch alle bereitgestellten Beispiele sind C# und Objective-c

## <a name="exposing-a-subset-of-the-managed-code"></a>Eine Teilmenge von verwaltetem Code verfügbar zu machen

Die generierte systemeigene Bibliothek bzw. ein Framework enthält die Objective-C-Code, um jede der verwalteten APIs aufrufen, die verfügbar gemacht wird. Die weitere-API, die Sie der Entwurfsoberfläche (Öffentliche Stellen) klicken Sie dann größer der systemeigenen _"Klebstoff"_ Bibliothek werden.

Es kann eine gute Idee, erstellen Sie eine andere, kleinere Assembly aus, um nur die erforderlichen APIs für den systemeigenen Entwickler verfügbar zu machen sein. Dieser Fassade ermöglichen Ihnen auch mehr Kontrolle über die Sichtbarkeit, Benennung, Fehler beim Überprüfen der... des generierten Codes.

## <a name="exposing-a-chunkier-api"></a>Eine übersichtlichere API verfügbar zu machen

Es gibt ein Preis für den Übergang von nativem zu verwalteten (und zurück). Daher ist es besser, verfügbar machen _anstelle von ' geschwätzige ' grobe_ APIs, um die Entwickler systemeigenen Anwendungen, z. B.

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

**Grobe**

```csharp
public class Person {
  public Person (string firstName, string lastName) {}
}
```

```objc
// a single call / transition will perform better
Person *p = [[Person alloc] initWithFirstName:@"Sebastien" lastName:@"Pouliot"];
```

Die Leistung wird besser sein, da die Anzahl der Übergänge kleiner ist. Außerdem müssen weniger Code generiert werden, damit dies auch eine kleinere native Bibliothek erstellt wird.

## <a name="naming"></a>Benennen

Benennung ist eines der beiden schwierigsten Probleme in Informatik, die anderen Cache-invalidierung und off-von-1-Fehler angezeigt wird. Hoffentlich können Einbetten von .NET Sie geschützt werden, über alle außer benennen.

### <a name="types"></a>Typen

Objective-C-unterstützt Namespaces nicht. Im Allgemeinen die Typen werden mit dem Präfix mit einer 2 (für Apple) oder 3 (für die 3. Parteien) Zeichen-Präfix, z. B. `UIView` für UIKit Ansicht, das das Framework bezeichnet.

Für Typen in .NET ist wird übersprungen, den Namespace nicht möglich, da sie verwirrende, oder doppelte Namen führen kann. Dadurch vorhandene .NET Typen, die sehr lang werden, z. B.

```csharp
namespace Xamarin.Xml.Configuration {
  public class Reader {}
}
```

würde z. B. verwendet werden:

```objc
id reader = [[Xamarin_Xml_Configuration_Reader alloc] init];
```

Jedoch können Sie den Typ als wieder verfügbar machen:

```csharp
public class XAMXmlConfigReader : Xamarin.Xml.Configuration.Reader {}
```

somit benutzerfreundlicheren Objective-C verwenden, z.B.:

```objc
id reader = [[XAMXmlConfigReader alloc] init];
```

### <a name="methods"></a>Methoden

Sogar .NET Namen möglicherweise nicht ideal für eine Objective-C-API.

Benennungskonventionen in Objective-C-unterscheiden sich .NET (Camel-Case anstelle der Pascal-Schreibweise, ausführlicher) zur Verfügung.
Lesen Sie die [Codierungsrichtlinien für Cocoa](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/CodingGuidelines/Articles/NamingMethods.html#//apple_ref/doc/uid/20001282-BCIGIJJF).

Gesichtspunkt ein Objective-C-Entwickler, eine Methode mit einem `Get` Präfix bedeutet, Sie besitzen die Instanz, d. h. keine der [Regel abrufen](https://developer.apple.com/library/content/documentation/CoreFoundation/Conceptual/CFMemoryMgmt/Concepts/Ownership.html#//apple_ref/doc/uid/20001148-SW1).

Diese Benennungsregel wurde keine Übereinstimmung in der Welt .NET GC; eine .NET-Methode übergeben wird mit einem `Create` Präfix verhält sich genauso wie in .NET. Allerdings für Objective-C-Entwickler normalerweise bedeutet dies Sie besitzen die zurückgegebene Instanz ist, d. h. die [erstellen Regel](https://developer.apple.com/library/content/documentation/CoreFoundation/Conceptual/CFMemoryMgmt/Concepts/Ownership.html#//apple_ref/doc/uid/20001148-103029).

## <a name="exceptions"></a>Ausnahmen

Es ist durchaus üblich, in .NET mit Ausnahmen, die häufig zum Melden von Fehlern verwendet. Allerdings sind sie langsam und nicht mit ihr identisch in Objective-c Nach Möglichkeit sollten Sie sie aus der Objective-C-Entwickler verbergen.

Z. B. .NET `Try` Muster werden wesentlich einfacher, Objective-C-Code verwenden:

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

### <a name="exceptions-inside-init"></a>Ausnahmen in `init*`

In .NET ein Konstruktor muss entweder erfolgreich ausgeführt werden, und Zurückgeben einer (_hoffentlich_) gültige Instanz oder eine Ausnahme ausgelöst.

Im Gegensatz dazu Objective-C erlaubt `init*` zurückzugebenden `nil` Wenn eine Instanz kann nicht erstellt werden kann. Dies ist ein häufig, jedoch keine allgemeine Muster, die in vielen der Apple Frameworks verwendet. In anderen Fällen einer `assert` können auftreten, (und den aktuellen Prozess).

Der Generator führen Sie die gleichen `return nil` Muster für generierte `init*` Methoden. Wenn eine verwaltete Ausnahme wird ausgelöst, wird es gedruckt werden (mit `NSLog`) und `nil` wird an den Aufrufer zurückgegeben werden.

## <a name="operators"></a>Operatoren

Objective-C erlaubt keine Operatoren überladen werden, als C# ausgeführt, sodass diese in Klassenselektoren konvertiert werden.

["Anzeigename"](https://docs.microsoft.com/dotnet/standard/design-guidelines/operator-overloads) benannte Methoden generiert werden, statt die überladenen Wenn gefunden, und eine einfachere API nutzen liefern.

Klassen, die die Operatoren überschreiben `==` und/oder `!=` sollten auch die standardmäßige Equals (Objekt)-Methode überschreiben.
