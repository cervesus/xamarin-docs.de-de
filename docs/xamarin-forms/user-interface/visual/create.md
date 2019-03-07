---
title: Erstellen eines Xamarin.Forms-Visual-Renderers
description: Xamarin.Forms Visual ermöglicht Renderer selektiv auf VisualElement Objekte angewendet werden, ohne Unterklasse Xamarin.Forms-Steuerelemente.
ms.prod: xamarin
ms.assetid: 80BF9C72-AC28-4AAF-9DDD-B60CBDD1CD59
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/05/2019
ms.openlocfilehash: 3c614bcd0044a0aa4a698c830d2f2af5aba6c65a
ms.sourcegitcommit: 00744f754527e5b55154365f89691caaf1c9d929
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/07/2019
ms.locfileid: "57557512"
---
# <a name="xamarinforms-visual"></a>Xamarin.Forms-Visual

Xamarin.Forms Visual ermöglicht Renderer selektiv auf anzuwendenden `VisualElement` Objekte ohne Unterklasse Xamarin.Forms-Steuerelemente. Jeder Renderer, die angeben, ein `VisualType` als Teil der `ExportRendererAttribute` Renderer Ansichten statt der Standard-Renderer verwendet werden. Zum Zeitpunkt der Renderer Auswahl der `Visual` -Eigenschaft der Ansicht werden untersucht und in den Renderer Prozess enthalten.

> [!IMPORTANT]
> Derzeit den `Visual` kann nicht geändert werden, nachdem das Steuerelement gerendert wurde, aber dadurch wird in einer zukünftigen Version geändert.

Xamarin.Forms enthält eine visuelle Darstellung, die basierend auf Material Design. Weitere Informationen finden Sie unter [Xamarin.Forms Material Visual](material-visual.md).

## <a name="create-a-visual"></a>Erstellen einer Visualisierung

Erstellen Sie in Ihrer Bibliothek plattformübergreifende einen Typ, der abgeleitet `IVisual`:

```csharp
public class CustomVisual : IVisual
{
}
```

## <a name="register-the-visual-type"></a>Registrieren Sie den Typ des Visuals

Erstellen Sie einen Renderer für die gewünschte Ansicht aus, und registrieren Sie den Typ des Visuals im Rahmen der Plattformen `ExportRenderAttribute`:

```csharp
using Xamarin.Forms.Material.Android;

[assembly: ExportRenderer(typeof(ContentPage), typeof(CustomPageRenderer), new[] { typeof(CustomVisual) })]
namespace MyApp.Android
{
    public class CustomPageRenderer : PageRenderer
    {
        ...
    }
}
```

## <a name="consume-the-visual"></a>Nutzen Sie das visuelle Element

Anwendungen können mithilfe der benutzerdefinierten Visualisierung durch Festlegen der `Visual` Eigenschaft:

```xaml
<ContentPage ...
             Visual="Custom">
    ...
</ContentPage>
```

Der entsprechende C#-Code ist:

```csharp
ContentPage contentPage = new ContentPage();
contentPage.Visual = new CustomVisual();
```

## <a name="register-a-name-for-a-visual"></a>Registrieren Sie einen Namen für ein Visual

Möglicherweise bestehen Konflikte zwischen verschiedenen Visual Bibliotheken oder vielleicht möchten auf eine Visualisierung durch einen anderen Namen zu verweisen. In diesem Szenario können Sie ein Assemblyattribut verwenden, um einen anderen Namen für eine bestimmte Visualisierung zu registrieren:

```csharp
[assembly: Visual("MyMaterialName", typeof(VisualMarker.MaterialVisual))]
```

Die benutzerdefinierte Visualisierung kann dann über dessen registriertem Namen verwendet werden:

```xaml
<ContentPage ...
             Visual="MyMaterialName">
    ...
</ContentPage>
````

## <a name="related-links"></a>Verwandte Links

- [Xamarin.Forms-Material Visual](material-visual.md)
- [Benutzerdefinierte Renderer](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
