---
title: NSString in xamarin. IOS und xamarin. Mac
description: In diesem Dokument wird beschrieben, wie xamarin. IOS NSString- C# Objekte transparent in Zeichen folgen Objekte konvertiert, wenn dies nicht der Fall ist.
ms.prod: xamarin
ms.assetid: 785744B3-42E2-4590-8F41-435325E609B9
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/21/2017
ms.openlocfilehash: 589b584e0526ffdc56dd5ec26aa25a0b61e2141a
ms.sourcegitcommit: 5f972a757030a1f17f99177127b4b853816a1173
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/21/2019
ms.locfileid: "69889903"
---
# <a name="nsstring-in-xamarinios-and-xamarinmac"></a>NSString in xamarin. IOS und xamarin. Mac

Der Entwurf von xamarin. IOS und xamarin. Mac erfordert die Verwendung der API zum verfügbar machen des systemeigenen .net-Zeichen folgen `string`Typs,, für die C# Zeichen folgen Bearbeitung in und anderen .NET-Programmiersprachen sowie für das verfügbar machen von Zeichen folgen als Datentyp, der von der API verfügbar gemacht wird, anstelle von  `NSString`der -Datentyp.

Dies bedeutet, dass Entwickler keine Zeichen folgen beibehalten müssen, die für den Aufruf von xamarin. IOS & xamarin. Mac-`Foundation.NSString` `System.String` API (vereinheitlicht) in einem besonderen Typ () verwendet werden sollen. Sie können weiterhin Mono für alle Vorgänge verwenden, und wann immer eine API in xamarin. IOS oder xamarin. Mac erfordert eine Zeichenfolge, und die API-Bindung übernimmt das Marshalling der Informationen.

Beispielsweise wird die "Text"-Eigenschaft von "Ziel- `UILabel` C" `NSString`für einen vom Typ wie folgt deklariert:

```objc
@property(nonatomic, copy) NSString *text
```

Dies wird in xamarin. IOS als verfügbar gemacht:

```csharp
class UILabel {
    public string Text { get; set; }
}
```

Im Hintergrund Marshalls die-Implementierung dieser Eigenschaft die C# Zeichenfolge in ein `NSString` -Objekt und ruft die `objc_msgSend` -Methode auf die gleiche Weise auf wie bei "Ziel-C".

Es gibt eine Handvoll von Ziel-C-APIs von Drittanbietern, die keinen `NSString`verwenden, sondern stattdessen eine C-Zeichenfolge ("*char*") verwenden. In diesen Fällen können Sie weiterhin den C# String-Datentyp verwenden, aber Sie müssen das Attribut [[plainstring]](~/cross-platform/macios/binding/objective-c-libraries.md) verwenden, um den Bindungs Generator darüber zu informieren, dass diese Zeichenfolge nicht als `NSString`, sondern als C-Zeichenfolge gemarshallt werden soll.

 <a name="Exceptions_to_the_Rule" />

## <a name="exceptions-to-the-rule"></a>Ausnahmen für die Regel

In xamarin. IOS und xamarin. Mac haben wir eine Ausnahme von dieser Regel vorgenommen. Die Entscheidung zwischen der Bereitstellung `string`von s und dem Zeitpunkt, an dem eine außer `NSString`und verfügbar gemacht wird, erfolgt `NSString`, wenn die Methode einen Zeiger Vergleich anstelle eines Inhalts Vergleichs ausführen könnte.

Dies kann vorkommen, wenn eine Ziel-C-API eine `NSString`öffentliche Konstante als Token verwendet, das eine Aktion darstellt, anstatt den eigentlichen Inhalt der Zeichenfolge zu vergleichen.

In diesen Fällen `NSString`  werden APIs verfügbar gemacht, und es gibt eine Minderheit von APIs, die über diese verfügen. Sie werden auch bemerken, dass NSString-Eigenschaften in einigen Klassen verfügbar gemacht werden. Diese `NSString` Eigenschaften werden für Elemente wie Benachrichtigungen verfügbar gemacht. Dies sind Eigenschaften, die in der Regel wie folgt aussehen:

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

Ein anderer Ort `NSString`, an dem s in der API verfügbar gemacht werden, ist die Verwendung von Token, die als Parameter für bestimmte APIs in `NSDictionary` IOS oder OS X verwendet werden und Objekte als Parameter annehmen. Das Wörterbuch enthält `NSString` in der Regel Schlüssel. Xamarin. IOS benennt diese statischen `NSString` Eigenschaften gemäß der Konvention durch Hinzufügen des Namens "Key".
