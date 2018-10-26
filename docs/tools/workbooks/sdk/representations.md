---
title: Darstellungen in Xamarin Workbooks
description: Dieses Dokument beschreibt die Pipeline an Xamarin Workbooks Darstellung, die das Rendering von umfangreichen Ergebnissen für jeden Code ermöglicht, die einen Wert zurückgibt.
ms.prod: xamarin
ms.assetid: 5C7A60E3-1427-47C9-A022-720F25ECB031
author: lobrien
ms.author: laobri
ms.date: 03/30/2017
ms.openlocfilehash: d9aafbe13e06875b6577a4d2308e419932fd1589
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50103711"
---
# <a name="representations-in-xamarin-workbooks"></a>Darstellungen in Xamarin Workbooks

## <a name="representations"></a>Darstellungen

In einer Arbeitsmappe oder Inspektor-Sitzung wird Code, der ausgeführt wird, und ergibt ein Ergebnis (z. B. eine Methode einen Wert oder das Ergebnis eines Ausdrucks zurückgegeben wird) über die Pipeline Darstellung im Agent verarbeitet. Alle Objekte, mit Ausnahme von primitive Typen wie Ganzzahlen, spiegelt sich um interaktive Member Graphen zu erstellen, und wechseln Sie mit der werden durch einen Prozess aus, um alternative Darstellungen bereitzustellen, die der Client umfassender gerendert werden kann. Objekte, die jeder Größe und Tiefe werden sicher (einschließlich Zyklen und unbegrenzte aufzählbare Elemente bereit) aufgrund der verzögerten und interaktive Reflektion und Remoting unterstützt.

Xamarin Workbooks bietet einige Datentypen, die auf allen Agents und Clients, die umfangreiche Rendering der Ergebnisse ermöglichen. [`Color`][xir-color] ist ein Beispiel für einen solchen Typ, in dem Beispiel unter iOS, der Agent verantwortlich für die Konvertierung ist `CGColor` oder `UIColor` Objekte in einem `Xamarin.Interactive.Representations.Color` Objekt.

Zusätzlich zu den gängigen Darstellungen bietet der Integrations-SDK APIs für Serialisierung von benutzerdefinierten Darstellungen im Agent und das Rendern von Darstellungen im Client.

## <a name="external-representations"></a>Externe Darstellungen

[`Xamarin.Interactive.IAgent.RepresentationManager`][repman] bietet die Möglichkeit zum Registrieren einer [`RepresentationProvider`][repp], die eine Integration implementieren muss, zum Konvertieren von ein beliebiges Objekt in einem agnostisch Formular zum Rendern. Diese agnostischen Formen müssen implementieren die [`ISerializableObject`][serobj] Schnittstelle.

Implementieren der `ISerializableObject` Schnittstelle fügt eine Serialisierungsmethode, die steuert, genau wie die Objekte serialisiert werden. Die `Serialize` -Methode erwartet, dass ein Entwickler wird genau angeben, welche Eigenschaften serialisiert werden soll und wie der endgültige Name sein wird. Ansehen der `Person` -Objekt in unserem [`KitchenSink` Beispiel] [Beispiel], wir können sehen, wie dies funktioniert:

```csharp
public sealed class Person : ISerializableObject
{
  public string Name { get; }

  // Rest of the code is omitted…

  void ISerializableObject.Serialize (ObjectSerializer serializer)
    => serializer.Property (nameof (Name), Name);
}
```

Wenn wir eine Obermenge oder eine Teilmenge der Eigenschaften aus dem ursprünglichen Objekt geben möchten, können wir, dass bei `Serialize`. Beispielsweise können wir etwa wie folgt, geben Sie eine vorab berechnete `Age` Eigenschaft `Person`:

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
> APIs, die erzeugen `ISerializableObject` Objekte direkt müssen nicht vom behandelt werden eine `RepresentationProvider`. Ist das Objekt, das Sie anzeigen möchten **nicht** ein `ISerializableObject`, möchten Sie behandeln, umschließen ihn in Ihre `RepresentationProvider`.

### <a name="rendering-a-representation"></a>Eine Darstellung Rendern

Renderer werden in JavaScript implementiert und haben Zugriff auf einen JavaScript-Version des Objekts dargestellt über `ISerializableObject`. Außerdem haben Sie die JavaScript-Kopie einer `$type` string-Eigenschaft, die den Namen angibt.

Es wird empfohlen, mithilfe von TypeScript-Integration Clientcode, der natürlich in Vanilla JavaScript kompiliert wird. In beiden Fällen das SDK bietet [Typings][typings] diese verweist direkt TypeScript oder einfach bezeichnet manuell Wenn Vanille Schreiben von JavaScript wird bevorzugt werden.

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

Hier `PersonRenderer` implementiert die `Renderer` Schnittstelle. Finden Sie unter der [Typings][typings] Weitere Details.

[typings]: https://github.com/xamarin/Workbooks/blob/master/SDK/typings/xamarin-interactive.d.ts
[xir-color]: https://developer.xamarin.com/api/type/Xamarin.Interactive.Representations.Color/
[repman]: https://developer.xamarin.com/api/type/Xamarin.Interactive.Representations.IRepresentationManager/
[repp]: https://developer.xamarin.com/api/type/Xamarin.Interactive.Representations.RepresentationProvider/
[serobj]: https://developer.xamarin.com/api/type/Xamarin.Interactive.Serialization.ISerializableObject/
