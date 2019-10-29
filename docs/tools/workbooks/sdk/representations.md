---
title: Darstellungen in Xamarin Workbooks
description: In diesem Dokument wird die Pipeline für die Xamarin Workbooks Darstellung beschrieben, die das Rendering umfassender Ergebnisse für jeden Code ermöglicht, der einen Wert zurückgibt.
ms.prod: xamarin
ms.assetid: 5C7A60E3-1427-47C9-A022-720F25ECB031
author: davidortinau
ms.author: daortin
ms.date: 03/30/2017
ms.openlocfilehash: c1afba6943faa03ee07a3ec70624f668748041cb
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73018584"
---
# <a name="representations-in-xamarin-workbooks"></a>Darstellungen in Xamarin Workbooks

## <a name="representations"></a>Erklärungen

Innerhalb einer Arbeitsmappe oder Inspektor-Sitzung wird Code, der ausgeführt wird und ein Ergebnis ergibt (z. b. eine Methode, die einen Wert oder das Ergebnis eines Ausdrucks zurückgibt), über die Darstellungs Pipeline im Agent verarbeitet. Alle Objekte, mit Ausnahme von primitiven, wie z. b. ganze Zahlen, werden reflektiert, um interaktive Element Diagramme zu erzeugen, und durchlaufen einen Prozess, um alternative Darstellungen bereitzustellen, die vom Client stärker gerenbt werden können. Objekte beliebiger Größe und Tiefe werden aufgrund von verzögerter und interaktiver Reflektion und Remoting sicher unterstützt (einschließlich Zyklen und unendlichen Enumerables).

Xamarin Workbooks bietet einige Typen, die allen Agents und Clients gemeinsam sind, die ein umfassendes Rendering von Ergebnissen ermöglichen. `Color` ist ein Beispiel für einen solchen Typ, bei dem z. b. für IOS der Agent für die Umstellung von `CGColor` oder `UIColor` Objekten in ein `Xamarin.Interactive.Representations.Color` Objekt zuständig ist.

Zusätzlich zu allgemeinen Darstellungen stellt das Integrations-SDK APIs zum Serialisieren von benutzerdefinierten Darstellungen im Agent und zum Rendern von Darstellungen im Client bereit.

## <a name="external-representations"></a>Externe Darstellungen

`Xamarin.Interactive.IAgent.RepresentationManager` bietet die Möglichkeit, ein `RepresentationProvider`zu registrieren, das von einer Integration implementiert werden muss, um von einem beliebigen Objekt in ein agnostisches Formular zum Rendering zu konvertieren. Diese agnostischen Formen müssen die `ISerializableObject`-Schnittstelle implementieren.

Beim Implementieren der `ISerializableObject`-Schnittstelle wird eine Serialize-Methode hinzugefügt, die genau steuert, wie Objekte serialisiert werden Die `Serialize`-Methode erwartet, dass ein Entwickler genau angibt, welche Eigenschaften serialisiert werden sollen und wie der endgültige Name lauten soll. Wenn wir uns das `Person` Objekt in unserem [`KitchenSink` Sample] [Sample] ansehen, können wir sehen, wie dies funktioniert:

```csharp
public sealed class Person : ISerializableObject
{
  public string Name { get; }

  // Rest of the code is omitted…

  void ISerializableObject.Serialize (ObjectSerializer serializer)
    => serializer.Property (nameof (Name), Name);
}
```

Wenn eine übergeordnete Gruppe oder eine Teilmenge der Eigenschaften des ursprünglichen Objekts bereitgestellt werden soll, können wir dies mit `Serialize`tun. Beispielsweise könnten wir etwa so vorgehen, um eine Voraus berechnete `Age` Eigenschaft für `Person`bereitzustellen:

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
> APIs, die `ISerializableObject` Objekte direkt liefern, müssen nicht von einem `RepresentationProvider`behandelt werden. Wenn das Objekt, das Sie anzeigen möchten, **kein** `ISerializableObject`ist, sollten Sie es in Ihrem `RepresentationProvider`behandeln.

### <a name="rendering-a-representation"></a>Rendering einer Darstellung

Renderer werden in JavaScript implementiert und haben Zugriff auf eine JavaScript-Version des Objekts, das über `ISerializableObject`dargestellt wird. Die JavaScript-Kopie verfügt auch über eine `$type` String-Eigenschaft, die den Namen des .net-Typs angibt.

Es wird empfohlen, typescript für den Client Integrations Code zu verwenden, der natürlich in Vanille-JavaScript kompiliert wird. In jedem Fall stellt das SDK Eingaben [bereit, auf][typings] die direkt über typescript verwiesen werden kann, oder auf die Sie einfach manuell verweisen, wenn Sie das Schreiben von Vanille-JavaScript bevorzugen.

Der Haupt Integrationspunkt für das Rendering ist `xamarin.interactive.RendererRegistry`:

```js
xamarin.interactive.RendererRegistry.registerRenderer(
  function (source) {
    if (source.$type === "SampleExternalIntegration.Person")
      return new PersonRenderer;
    return undefined;
  }
);
```

Hier implementiert `PersonRenderer` die `Renderer`-Schnittstelle. Weitere Informationen finden Sie in den [typeingaben][typings] .

[typings]: https://github.com/xamarin/Workbooks/blob/master/SDK/typings/xamarin-interactive.d.ts
