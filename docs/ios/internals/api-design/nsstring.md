---
title: NSString in xamarin. IOS und xamarin. Mac
description: In diesem Dokument wird beschrieben, wie xamarin. IOS NSString- C# Objekte transparent in Zeichen folgen Objekte konvertiert, wenn dies nicht der Fall ist.
ms.prod: xamarin
ms.assetid: 785744B3-42E2-4590-8F41-435325E609B9
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/21/2017
ms.openlocfilehash: f744f4ed5619e4e7f4a9d85897c4451bf7e5b9bc
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73022346"
---
# <a name="nsstring-in-xamarinios-and-xamarinmac"></a>NSString in xamarin. IOS und xamarin. Mac

Der Entwurf von xamarin. IOS und xamarin. Mac erfordert die Verwendung der API, um den systemeigenen .net-Zeichen Folgentyp, `string`, für die C# Zeichen folgen Bearbeitung in und anderen .NET-Programmiersprachen verfügbar zu machen und Zeichenfolge als den Datentyp verfügbar zu machen, der von der API verfügbar gemacht wird, anstelle von `NSString` -Datentyp.

Dies bedeutet, dass Entwickler keine Zeichen folgen beibehalten müssen, die für den Aufruf von xamarin. IOS & xamarin. Mac-API (vereinheitlicht) in einem besonderen Typ (`Foundation.NSString`) verwendet werden sollen. Sie können weiterhin Mono-`System.String` für alle Vorgänge und immer dann verwenden, wenn eine API in Xamarin. IOS oder xamarin. Mac erfordert eine Zeichenfolge, und die API-Bindung übernimmt das Marshalling der Informationen.

Beispielsweise wird die "Text"-Eigenschaft von "Ziel-C" für eine `UILabel` vom Typ "`NSString`" wie folgt deklariert:

```objc
@property(nonatomic, copy) NSString *text
```

Dies wird in xamarin. IOS als verfügbar gemacht:

```csharp
class UILabel {
    public string Text { get; set; }
}
```

Im Hintergrund Marshalls die-Implementierung dieser Eigenschaft die C# Zeichenfolge in eine`NSString`und ruft die`objc_msgSend`-Methode auf die gleiche Weise auf wie bei "Ziel-C".

Es gibt eine Handvoll von Ziel-C-APIs von Drittanbietern, die keine `NSString`verwenden, sondern stattdessen eine C-Zeichenfolge ("*char*") verwenden. In diesen Fällen können Sie weiterhin den C# String-Datentyp verwenden, aber Sie müssen das Attribut [[plainstring]](~/cross-platform/macios/binding/objective-c-libraries.md) verwenden, um den Bindungs Generator darüber zu informieren, dass diese Zeichenfolge nicht als`NSString`gemarshallt werden soll, sondern als C-Zeichenfolge.

 <a name="Exceptions_to_the_Rule" />

## <a name="exceptions-to-the-rule"></a>Ausnahmen für die Regel

In xamarin. IOS und xamarin. Mac haben wir eine Ausnahme von dieser Regel vorgenommen. Die Entscheidung zwischen dem verfügbar machen von `string`s und dem Verwenden von mit Ausnahme von `NSString`s erfolgt, wenn die `NSString` -Methode einen Zeiger Vergleich anstelle eines Inhalts Vergleichs ausführen könnte.

Dies kann vorkommen, wenn eine Ziel-C-API eine öffentliche `NSString` Konstante als Token verwendet, das eine Aktion darstellt, anstatt den eigentlichen Inhalt der Zeichenfolge zu vergleichen.

In diesen Fällen werden `NSString` -APIs verfügbar gemacht, und es gibt eine Minderheit von APIs, die über diese verfügen. Sie werden auch bemerken, dass NSString-Eigenschaften in einigen Klassen verfügbar gemacht werden. Diese `NSString` Eigenschaften werden für Elemente wie Benachrichtigungen verfügbar gemacht. Dies sind Eigenschaften, die in der Regel wie folgt aussehen:

```csharp
class Foo {
     public NSString FooNotification { get; }
}
```

Benachrichtigungen sind Schlüssel, die für die `NSNotification` Klasse verwendet werden, wenn Sie für ein bestimmtes Ereignis, das von der Laufzeit gesendet wird, registriert werden sollen.

Schlüssel sehen in der Regel etwa wie folgt aus:

```csharp
class Foo {
     public NSString FooBarKey { get; }
}
```

Ein anderer Ort, an dem `NSString`s in der API verfügbar gemacht werden, ist die Verwendung von Token, die als Parameter für bestimmte APIs in ios oder OS X verwendet werden, die `NSDictionary` Objekte als Parameter annehmen. Das Wörterbuch enthält in der Regel `NSString` Schlüssel. Xamarin. IOS benennt diese statischen `NSString` Eigenschaften gemäß der Konvention durch Hinzufügen des Namens "Key".
