---
title: Darstellungen im Xamarin-Arbeitsmappen
ms.prod: xamarin
ms.assetid: 5C7A60E3-1427-47C9-A022-720F25ECB031
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 03/30/2017
ms.openlocfilehash: a5593ac902bfd2478cd8587aeef7e4a3926627dd
ms.sourcegitcommit: 775a7d1cbf04090eb75d0f822df57b8d8cff0c63
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/18/2018
---
# <a name="representations-in-xamarin-workbooks"></a>Darstellungen im Xamarin-Arbeitsmappen

## <a name="representations"></a>Darstellungen

Innerhalb einer Arbeitsmappe oder Inspektor-Sitzung wird Code, der ausgeführt wird und ergibt ein Ergebnis (z. B. eine Methode einen Wert oder das Ergebnis eines Ausdrucks zurückgeben) über die Pipeline Darstellung im-Agent verarbeitet. Alle Objekte, mit Ausnahme von primitive Typen wie Zahlen, werden wiedergegeben werden, um interaktive Member Diagramme zu erzeugen und durchlaufen Sie einen Prozess alternative Darstellungen bereitstellen, die der Client umfassender gerendert werden kann. Objekte eines beliebigen Größe und der Tiefe werden sicher (einschließlich Zyklen und unbegrenzte aufzählbare Elemente bereit) aufgrund der verzögerten und interaktive Reflektion und Remoting unterstützt.

Xamarin-Arbeitsmappen enthält einige allgemeine Typen für alle Agents und Clients, die umfangreiche Rendering der Ergebnisse zu ermöglichen. [`Color`][xir-color] ist ein Beispiel eines solchen Typs, in dem Beispiel auf iOS der Agent verantwortlich für das Konvertieren ist `CGColor` oder `UIColor` Objekte in einem `Xamarin.Interactive.Representations.Color` Objekt.

Zusätzlich zu den allgemeinen Darstellungen bietet die SDK-Integration APIs zum Serialisieren von benutzerdefinierter Darstellungen im Agent und Darstellungen im Client rendern.

## <a name="external-representations"></a>Externe Darstellungen

[`Xamarin.Interactive.IAgent.RepresentationManager`][repman] bietet die Möglichkeit zum Registrieren einer [ `RepresentationProvider` ] [ repp], die eine Integration implementieren muss, zum Konvertieren von ein beliebiges Objekt in einem agnostisch Formular zum Rendern. Diese agnostischen Formen müssen implementieren die [ `ISerializableObject` ] [ serobj] Schnittstelle.

Implementieren der `ISerializableObject` Schnittstelle fügt eine Serialisierungsmethode, die genau steuert, wie die Objekte serialisiert werden. Die `Serialize` Methode erwartet, dass ein Entwickler genau angeben wird, welche Eigenschaften serialisiert werden und wie der endgültige Name werden. Ansehen der `Person` -Objekt in unserem [`KitchenSink` Beispiel] [Sample], können wir sehen, wie dies funktioniert:

```csharp
public sealed class Person : ISerializableObject
{
  public string Name { get; }

  // Rest of the code is omitted…

  void ISerializableObject.Serialize (ObjectSerializer serializer)
    => serializer.Property (nameof (Name), Name);
}
```

Wenn wir eine Obermenge oder eine Teilmenge der Eigenschaften von einem Originalobjekt bereitstellen möchten, können erledigen, die mit `Serialize`. Beispielsweise können wir tun etwa wie folgt einen vorberechneten bereitstellen `Age` Eigenschaft `Person`:

```csharp
public sealed class Person : ISerializableObject
{
  public string Name { get; set; }
  public DateTime DateOfBirth { get; set; }

  // <snip>

  void ISerializableObject.Serialize (ObjectSerializer serializer)
  {
    serializer.Property (nameof (Name), Name);
    serializer.Property (nameof (DateOfBirth), DateOfBirth);

    // Let's pre-compute an Age property that's the person's age in years,
    // so we don't have to compute it in the renderer.
    var age = (DateTime.MinValue + (DateTime.Now - DateOfBirth)).Year - 1;
    serializer.Property ("Age", age)
  }
}
```

> [!NOTE]
> APIs, die erzeugen `ISerializableObject` Objekte direkt müssen nicht vom behandelt eine `RepresentationProvider`. Wenn das Objekt angezeigt werden soll **nicht** ein `ISerializableObject`, möchten behandeln wrapping in Ihre `RepresentationProvider`.

### <a name="rendering-a-representation"></a>Eine Darstellung Rendern

Renderer werden in JavaScript implementiert und haben Zugriff auf einen JavaScript-Version des Objekts dargestellt, die über `ISerializableObject`. Die JavaScript-Kopie müssen zudem einen `$type` string-Eigenschaft, die den Typnamen .NET angibt.

Es empfiehlt sich TypeScript für Client-Integration Code natürlich in Vanille JavaScript kompiliert wird. In beiden Fällen das SDK bietet [Typings] [ typings] diese verweist direkt TypeScript oder einfach bezeichnet manuell Wenn Vanille Schreiben von JavaScript wird bevorzugt werden.

Die wichtigsten Integrationspunkt für das Rendering ist `xamarin.interactive.RendererRegistry`:

```js
xamarin.interactive.RendererRegistry.registerRenderer(
  function (source) {
    if (source.$type === "SampleExternalIntegration.Person")
      return new PersonRenderer;
    return undefined;
  }
);
```

Hier `PersonRenderer` implementiert die `Renderer` Schnittstelle. Finden Sie unter der [Typings] [ typings] Weitere Details.

[typings]: https://github.com/xamarin/Workbooks/blob/master/SDK/typings/xamarin-interactive.d.ts
[xir-color]: https://developer.xamarin.com/api/type/Xamarin.Interactive.Representations.Color/
[repman]: https://developer.xamarin.com/api/type/Xamarin.Interactive.Representations.IRepresentationManager/
[repp]: https://developer.xamarin.com/api/type/Xamarin.Interactive.Representations.RepresentationProvider/
[serobj]: https://developer.xamarin.com/api/type/Xamarin.Interactive.Serialization.ISerializableObject/
