---
title: NSString in Xamarin.iOS und Xamarin.Mac
description: Im vorliegenden Dokument wird beschrieben, wie Xamarin.iOS NSString-Objekte transparent in C#-Zeichenfolgenobjekte konvertiert, wenn dies nicht von allein geschieht.
ms.prod: xamarin
ms.assetid: 785744B3-42E2-4590-8F41-435325E609B9
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/21/2017
ms.openlocfilehash: 314c94fc9208a63e2f9305511df262327df921a5
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2020
ms.locfileid: "84565069"
---
# <a name="nsstring-in-xamarinios-and-xamarinmac"></a>NSString in Xamarin.iOS und Xamarin.Mac

Sowohl das Design von Xamarin.iOS als auch das von Xamarin.Mac erfordert die Verwendung einer API, um den nativen .NET-Zeichenfolgentyp `string` für die Zeichenfolgenmanipulation in C# und anderen .NET-Programmiersprachen verfügbar zu machen und Zeichenfolgen nicht als Datentyp  `NSString`, sondern als den von der API offengelegten Datentyp verfügbar zu machen.

Dies bedeutet, dass Entwickler Zeichenfolgen für den Aufruf in Xamarin.iOS & Xamarin.Mac API (Unified) nicht in einem speziellen Typ (`Foundation.NSString`) verwalten müssen, sondern weiterhin den `System.String` von Mono für alle Operationen verwenden können. Und falls eine API in Xamarin.iOS oder Xamarin.Mac eine Zeichenfolge benötigt, kümmert sich unsere API-Bindung um das Marshalling der Informationen.

Beispielsweise wird die Objective-C-Eigenschaft „text“ für ein `UILabel` vom Typ `NSString` folgendermaßen deklariert:

```objc
@property(nonatomic, copy) NSString *text
```

In Xamarin.iOS erfolgt die Offenlegung wie folgt:

```csharp
class UILabel {
    public string Text { get; set; }
}
```

Hinter den Kulissen führt die Implementierung dieser Eigenschaft zum Marshalling der C#-Zeichenfolge in einen `NSString`, und die `objc_msgSend`-Methode wird auf die gleiche Weise aufgerufen, wie dies bei Objective-C der Fall wäre.

Es gibt eine Handvoll Objective-C-APIs von Drittanbietern, die anstelle von `NSString` eine C-Zeichenfolge (ein *char*) nutzen. In diesen Fällen können Sie weiterhin den C#-Zeichenfolgendatentyp verwenden, aber Sie müssen den Bindungsgenerator mit dem [[PlainString]](~/cross-platform/macios/binding/objective-c-libraries.md)-Attribut darüber informieren, dass für diese Zeichenfolge kein Marshalling in einen `NSString`, sondern stattdessen in eine C-Zeichenfolge durchgeführt werden muss.

 <a name="Exceptions_to_the_Rule"></a>

## <a name="exceptions-to-the-rule"></a>Ausnahmen von der Regel

Sowohl in Xamarin.iOS als auch in Xamarin.Mac gibt es Ausnahmen von dieser Regel. Die Entscheidung, wann wir einen  `string` und wann ausnahmsweise einen  `NSString` verfügbar machen, wird getroffen, wenn die  `NSString`-Methode einen Zeigervergleich anstelle eines Inhaltsvergleichs durchführen könnte.

Dies kann vorkommen, wenn eine Objective-C-API eine öffentliche  `NSString`-Konstante als Token verwendet, die eine Aktion repräsentiert, statt die tatsächlichen Inhalte der Zeichenfolge zu vergleichen.

In diesen Fällen werden `NSString`-APIs verfügbar gemacht, dies gilt für einige wenige APIs. Darüber hinaus werden Sie feststellen, dass NSString-Eigenschaften in einigen Klassen verfügbar gemacht werden. Diese `NSString`-Eigenschaften werden für Elemente wie beispielsweise Nachrichten offengelegt. Diese Eigenschaften sehen üblicherweise wie folgt aus:

```csharp
class Foo {
     public NSString FooNotification { get; }
}
```

Benachrichtigungen sind Schlüssel, die für die `NSNotification`-Klasse verwendet werden, wenn Sie sich für ein bestimmtes Ereignis registrieren möchten, das von der Runtime übertragen wird.

Schlüssel sehen in der Regel ungefähr so aus:

```csharp
class Foo {
     public NSString FooBarKey { get; }
}
```

Eine weitere Stelle, an der `NSString`-Elemente in der API verfügbar gemacht werden, sind Token, die als Parameter für bestimmte APIs in iOS oder OS X verwendet werden, die `NSDictionary`-Objekte als Parameter verwenden. Das Wörterbuch enthält typischerweise `NSString`-Schlüssel. Xamarin.iOS benennt diese statischen `NSString`-Eigenschaften per Konvention durch Hinzufügen des Namens „Key“.
