---
title: NSString in Xamarin.iOS- und Xamarin.Mac
description: Dieses Dokument beschreibt, wie Xamarin.iOS transparent NSString Objekte konvertiert C# Zeichenfolgeobjekte, wenn dies nicht der Fall.
ms.prod: xamarin
ms.assetid: 785744B3-42E2-4590-8F41-435325E609B9
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/21/2017
ms.openlocfilehash: cc9e3f992642f3cdc3d16fe6f829b6a6c06b50fc
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61277813"
---
# <a name="nsstring-in-xamarinios-and-xamarinmac"></a>NSString in Xamarin.iOS- und Xamarin.Mac

Das Design der Xamarin.iOS- und Xamarin.Mac aufruft, für die API verwenden, um von den systemeigenen .NET Zeichenfolgen-Datentyp, verfügbar zu machen `string`, für die Bearbeitung in C# und anderen .NET-Programmiersprachen und Zeichenfolge als Datentyp verfügbar gemacht, von der API nicht verfügbar zu machen die `NSString` -Datentyp.

Dies bedeutet, dass Entwickler nicht haben soll, um Zeichenfolgen zu halten, die für den Aufruf von Xamarin.iOS- und Xamarin.Mac-API (einheitliche) verwendet werden sollen in eine besondere Art (`Foundation.NSString`), sie können der Mono verwenden weiterhin `System.String` für alle Vorgänge, und jedes Mal, wenn eine API in Xamarin.iOS oder Xamarin.Mac erfordert eine Zeichenfolge, die API-Bindung übernimmt die Marshallinginformationen.

Z. B. die Objective-C "Text"-Eigenschaft auf einen `UILabel` des Typs `NSString`, wird wie folgt deklariert:

```objc
@property(nonatomic, copy) NSString *text
```

Dies ist in Xamarin.iOS als zur Verfügung gestellt:

```csharp
class UILabel {
    public string Text { get; set; }
}
```

Hinter den Kulissen wird die Implementierung dieser Eigenschaft marshallt die C# -Zeichenfolge in eine `NSString` und ruft die `objc_msgSend` -Methode in der gleichen Weise wie Objective-C.

Es gibt eine Reihe von Drittanbieter-Objective-C-APIs, die nicht zwingend eine `NSString`, aber stattdessen eine C-Zeichenfolge (eine "*Char*"). In diesen Fällen können Sie trotzdem verwenden die C# string-Datentyp, aber Sie müssen die [[PlainString]](~/cross-platform/macios/binding/objective-c-libraries.md) Attribut, um den Generator für die Bindung zu informieren, die diese Zeichenfolge nicht sollen, können Sie als gemarshallt werden ein `NSString`, aber stattdessen als eine C-Zeichenfolge.

 <a name="Exceptions_to_the_Rule" />

## <a name="exceptions-to-the-rule"></a>Ausnahmen von der Regel

Wir haben in sowohl Xamarin.iOS- und Xamarin.Mac Ausnahme dieser Regel vorgenommen. Die Entscheidung zwischen, wenn es verfügbar zu machen `string`s, und wenn wir ein except und verfügbar zu machen `NSString`s wird durchgeführt, wenn die `NSString` Methode einen zeigervergleich, statt einen Content-Vergleich ausführen.

Dies kann passieren, wenn ein Objective-C-APIs, eine öffentliche verwendet `NSString` -Konstanten als ein Token, das eine Aktion ausführen, anstatt zu vergleichen den eigentlichen Inhalt der Zeichenfolge darstellt.

In diesen Fällen `NSString`  APIs verfügbar gemacht werden, und es gibt eine geringe Anzahl von APIs, die dies. Sie sehen außerdem, dass NSString-Eigenschaften in Klassen verfügbar gemacht werden. Die `NSString` Eigenschaften werden für Elemente wie Benachrichtigungen verfügbar gemacht. Dies sind, dass Eigenschaften in der Regel wie folgt aussehen:

```csharp
class Foo {
     public NSString FooNotification { get; }
}
```
Benachrichtigungen sind Schlüssel, die verwendet werden, für die `NSNotification` Klasse bei der Registrierung für ein bestimmtes Ereignis wird von der Laufzeit übertragen werden sollen.

Schlüssel Aussehen in der Regel etwa folgendermaßen:

```csharp
class Foo {
     public NSString FooBarKey { get; }
}
```

Einen anderen Speicherort, in denen `NSString`s werden verfügbar gemacht, die API ist, als Token, die als Parameter für bestimmte APIs unter iOS oder OS X verwendet werden, die annehmen `NSDictionary` Objekte als Parameter. Das Wörterbuch in der Regel enthält `NSString` Schlüssel. Xamarin.iOS-, gemäß der Konvention benannt, die statische `NSString` Eigenschaften durch den Namen "Key" hinzufügen.
