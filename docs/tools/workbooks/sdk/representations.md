---
title: Darstellungen in Xamarin Workbooks
description: In diesem Dokument wird die Pipeline für die Xamarin Workbooks Darstellung beschrieben, die das Rendering umfassender Ergebnisse für jeden Code ermöglicht, der einen Wert zurückgibt.
ms.prod: xamarin
ms.assetid: 5C7A60E3-1427-47C9-A022-720F25ECB031
author: lobrien
ms.author: laobri
ms.date: 03/30/2017
ms.openlocfilehash: b61452fc21d81f427249825decee4f119c50abf0
ms.sourcegitcommit: b07e0259d7b30413673a793ebf4aec2b75bb9285
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/26/2019
ms.locfileid: "68511499"
---
# <a name="representations-in-xamarin-workbooks"></a>Darstellungen in Xamarin Workbooks

## <a name="representations"></a>Erklärungen

Innerhalb einer Arbeitsmappe oder Inspektor-Sitzung wird Code, der ausgeführt wird und ein Ergebnis ergibt (z. b. eine Methode, die einen Wert oder das Ergebnis eines Ausdrucks zurückgibt), über die Darstellungs Pipeline im Agent verarbeitet. Alle Objekte, mit Ausnahme von primitiven, wie z. b. ganze Zahlen, werden reflektiert, um interaktive Element Diagramme zu erzeugen, und durchlaufen einen Prozess, um alternative Darstellungen bereitzustellen, die vom Client stärker gerenbt werden können. Objekte beliebiger Größe und Tiefe werden aufgrund von verzögerter und interaktiver Reflektion und Remoting sicher unterstützt (einschließlich Zyklen und unendlichen Enumerables).

Xamarin Workbooks bietet einige Typen, die allen Agents und Clients gemeinsam sind, die ein umfassendes Rendering von Ergebnissen ermöglichen. `Color`ein Beispiel für einen solchen Typ, bei dem z. b. für IOS der Agent für die Umstellung `CGColor` von `UIColor` -oder- `Xamarin.Interactive.Representations.Color` Objekten in ein-Objekt zuständig ist.

Zusätzlich zu allgemeinen Darstellungen stellt das Integrations-SDK APIs zum Serialisieren von benutzerdefinierten Darstellungen im Agent und zum Rendern von Darstellungen im Client bereit.

## <a name="external-representations"></a>Externe Darstellungen

`Xamarin.Interactive.IAgent.RepresentationManager`bietet die Möglichkeit, ein `RepresentationProvider`-Objekt zu registrieren, das von einer Integration implementiert werden muss, um von einem beliebigen Objekt in ein agnostisches Formular zum Rendering zu konvertieren. Diese agnostischen Formen müssen die `ISerializableObject` -Schnittstelle implementieren.

Durch Implementieren `ISerializableObject` der-Schnittstelle wird eine Serialize-Methode hinzugefügt, die genau steuert, wie Objekte serialisiert werden Die `Serialize` -Methode erwartet, dass ein Entwickler genau angibt, welche Eigenschaften serialisiert werden sollen und wie der endgültige Name lauten soll. Wenn wir uns `Person` das Objekt in unserem`KitchenSink` [Sample] [Sample] ansehen, können wir sehen, wie dies funktioniert:

```csharp
public sealed class Person : ISerializableObject
{
  public string Name { get; }

  // Rest of the code is omitted…

  void ISerializableObject.Serialize (ObjectSerializer serializer)
    => serializer.Property (nameof (Name), Name);
}
```

Wenn eine übergeordnete Gruppe oder eine Teilmenge der Eigenschaften des ursprünglichen Objekts bereitgestellt werden soll, können wir dies mit `Serialize`tun. Beispielsweise könnten wir etwa so vorgehen, um eine vorberechnete `Age` `Person`Eigenschaft für bereitzustellen:

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
> APIs, die `ISerializableObject` -Objekte direkt liefern, müssen nicht von einem `RepresentationProvider`behandelt werden. Wenn es sich bei dem Objekt `ISerializableObject`, das Sie anzeigen möchten, **nicht** um handelt, sollten Sie es in `RepresentationProvider`der behandeln.

### <a name="rendering-a-representation"></a>Rendering einer Darstellung

Renderer werden in JavaScript implementiert und haben Zugriff auf eine JavaScript-Version des Objekts, das durch `ISerializableObject`dargestellt wird. Die JavaScript-Kopie verfügt auch über `$type` eine Zeichen folgen Eigenschaft, die den Namen des .net-Typs angibt.

Es wird empfohlen, typescript für den Client Integrations Code zu verwenden, der natürlich in Vanille-JavaScript kompiliert wird. In jedem [Fall stellt das][typings] SDK Eingaben bereit, auf die direkt über typescript verwiesen werden kann, oder auf die Sie einfach manuell verweisen, wenn Sie das Schreiben von Vanille-JavaScript bevorzugen.

Der Haupt Integrationspunkt für das Rendering `xamarin.interactive.RendererRegistry`ist:

```js
xamarin.interactive.RendererRegistry.registerRenderer(
  function (source) {
    if (source.$type === "SampleExternalIntegration.Person")
      return new PersonRenderer;
    return undefined;
  }
);
```

Hier implementiert die `Renderer` -Schnittstelle. `PersonRenderer` Weitere Informationen finden Sie in den [Eingaben][typings].

[typings]: https://github.com/xamarin/Workbooks/blob/master/SDK/typings/xamarin-interactive.d.ts
