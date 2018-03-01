---
title: NSString
ms.topic: article
ms.prod: xamarin
ms.assetid: BB99EBD7-308A-C865-1829-4DFFDB1BBCA4
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: b52a9e926f5ec907746d2a2dd8ee7a6a6212742f
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="nsstring"></a>NSString

Das Design von Xamarin.iOS und Xamarin.Mac aufruft, für die API verwenden, die systemeigene .NET Zeichenfolgentyp verfügbar zu machen `string`, für die Bearbeitung in C# geschrieben und anderen Programmiersprachen .NET und Zeichenfolge als Datentyp API, der anstelle der verfügbarmachen`NSString` -Datentyp.


Dies bedeutet, dass Entwickler keine sollten, um Zeichenfolgen zu halten, die für den Aufruf von Xamarin.iOS & Xamarin.Mac-API (Unified) verwendet werden sollen, in eine Sonderform (`Foundation.NSString`), sie können den Mono verwenden weiterhin `System.String` für alle Vorgänge, und bei jedem eine API in Xamarin.iOS oder Xamarin.Mac muss eine Zeichenfolge, die unsere API-Bindung übernimmt die Marshallinginformationen.

Z. B. die Objective-C "Text"-Eigenschaft für ein `UILabel` des Typs `NSString`, wird wie folgt deklariert:

```csharp
@property(nonatomic, copy) NSString *text
```

Dies ist in Xamarin.iOS als zur Verfügung gestellt:

```csharp
class UILabel {
    public string Text { get; set; }
}
```

Hinter den Kulissen marshallt die Implementierung dieser Eigenschaft die C#-Zeichenfolge in eine `NSString` und ruft die `objc_msgSend` -Methode in der gleichen Weise wie Objective-C.

Es gibt eine Handvoll von Drittanbietern Objective-C-APIs, die müssen nicht zwingend ein `NSString`, stattdessen eine C-Zeichenfolge zu verarbeiten (ein "*Char*"). In diesen Fällen können Sie weiterhin den C#-String-Datentyp verwenden, aber Sie müssen die [[PlainString]](~/cross-platform/macios/binding/objective-c-libraries.md) Attribut, um den Generator für die Bindung zu informieren, die diese Zeichenfolge nicht sollen, können Sie als gemarshallt werden ein `NSString`, sondern stattdessen als eine C-Zeichenfolge.

 <a name="Exceptions_to_the_Rule" />


## <a name="exceptions-to-the-rule"></a>Ausnahmen von der Regel

In Xamarin.iOS und Xamarin.Mac haben wir eine Ausnahme von dieser Regel. Die Entscheidung zwischen Wenn zur Verfügung `string`s, und wir stellen, wenn ein except und verfügbar zu machen `NSString`s wird durchgeführt, wenn die `NSString` Methode konnte ausführen, wenn Sie einen zeigervergleich anstelle eines Vergleichs Inhalt.


Dies kann vorkommen, wenn ein Objective-C-APIs eine öffentliche verwendet `NSString` -Konstanten als ein Token, das eine Aktion aus, statt den tatsächlichen Inhalt der Zeichenfolge vergleichen darstellt.


In diesen Fällen `NSString` APIs bereitgestellt wurden, und es gibt eine Minderheit von APIs, die diese vorhanden ist. Beachten Sie auch, dass NSString-Eigenschaften in einigen Klassen verfügbar gemacht werden. Die `NSString` Eigenschaften für Elemente wie Benachrichtigungen verfügbar gemacht werden. Diese sind, dass Eigenschaften in der Regel sieht wie folgt aus:

```csharp
class Foo {
     public NSString FooNotification { get; }
}
```

Benachrichtigungen sind für verwendeten Schlüssel der `NSNotification` Klasse, wenn Sie für ein bestimmtes Ereignis von der Laufzeit broadcast registrieren möchten.

Schlüssel in der Regel wie folgt aussehen:

```csharp
class Foo {
     public NSString FooBarKey { get; }
}
```

Eine andere Stelle, wo `NSString`s werden verfügbar gemacht, die API wird als Token, die als Parameter an bestimmte APIs in iOS oder OS X verwendet werden, akzeptieren `NSDictionary` -Objekte als Parameter. Das Wörterbuch enthält i. d. r. `NSString` Schlüssel. Xamarin.iOS, gemäß der Konvention benennt die statische `NSString` Eigenschaften, indem Sie den Namen "Schlüssel" hinzufügen.
