---
title: Visuelles xamarin. Forms-Material
description: Xamarin. Forms Material Visual kann verwendet werden, um xamarin. Forms-Anwendungen zu erstellen, die unter IOS und Android identisch oder größtenteils identisch aussehen.
ms.prod: xamarin
ms.assetid: B774F68C-EF9E-49E1-B738-CDC64879ADA2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/12/2019
ms.openlocfilehash: b735541d51321231775b025745e68c54552697d3
ms.sourcegitcommit: dad4dfcd194b63ec9e903363351b6d9e543d4888
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/18/2019
ms.locfileid: "71198497"
---
# <a name="xamarinforms-material-visual"></a>Visuelles xamarin. Forms-Material

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-visualdemos)

[Material Design](https://material.io) ist ein von Google erstelltes Entwurfssystem, das die Größe, die Farbe, den Abstand und andere Aspekte der Anzeige und des Verhaltens von Ansichten und Layouts festlegt.

Das visuelle xamarin. Forms-Material kann verwendet werden, um Material Entwurfs Regeln auf xamarin. Forms-Anwendungen anzuwenden, um Anwendungen zu erstellen, die in IOS und Android identisch oder sehr identisch aussehen. Wenn die visuelle Visualisierung von Material aktiviert ist, übernehmen unterstützte Ansichten denselben Entwurf plattformübergreifend und sorgen für ein einheitliches Aussehen und Gefühl. Dies wird mit Material Renderer erreicht, die die Material Entwurfs Regeln anwenden.

Der Prozess zum Aktivieren der visuellen xamarin. Forms-Materialisierungen in der Anwendung lautet wie folgt:

1. Fügen Sie das nuget-Paket [xamarin. Forms. Visual. Material](https://www.nuget.org/packages/Xamarin.Forms.Visual.Material/) zu ihren IOS-und Android-Platt Form Projekten hinzu. Dieses nuget-Paket bietet optimierte Material Design-Renderer unter IOS und Android. Unter IOS stellt das Paket die transitiv Abhängigkeit zu [xamarin. IOS. materialcomponents](https://www.nuget.org/packages/Xamarin.iOS.MaterialComponents)bereit. Hierbei handelt es C# sich um eine Bindung an die [Material Komponenten von Google für IOS](https://material.io/develop/ios/). Unter Android stellt das Paket Buildziele bereit, um sicherzustellen, dass das TargetFramework ordnungsgemäß eingerichtet ist.
1. Initialisieren Sie die Material-Renderer in jedem Platt Form Projekt. Weitere Informationen finden Sie unter [Initialisieren von materialrenderern](#initialize-material-renderers).
1. Verwenden Sie die Material-Renderer `Material`, indem Sie die [`Visual`](xref:Xamarin.Forms.VisualElement.Visual) -Eigenschaft auf alle Seiten festlegen, die die Material Entwurfs Regeln übernehmen sollen. Weitere Informationen finden Sie unter [nutzungsmaterialrenderer](#consume-material-renderers).
1. optionale Anpassen der Material-Renderer. Weitere Informationen finden Sie unter [Anpassen von Material Renderer](#customize-material-renderers).

> [!IMPORTANT]
> Unter Android ist für die Material-Renderer mindestens eine Version von 5,0 (API 21) oder höher und ein TargetFramework der Version 9,0 (API 28) erforderlich. Außerdem erfordert Ihr Platt Form Projekt Android-Unterstützungs Bibliotheken 28.0.0 oder höher, und das Design muss von einem Material Components-Design erben oder weiterhin von einem AppCompat-Design erben. Weitere Informationen finden Sie unter [Getting Started with Material Components for Android](https://github.com/material-components/material-components-android/blob/master/docs/getting-started.md).

Material-Renderer sind zurzeit im [xamarin. Forms. Visual. Material](https://www.nuget.org/packages/Xamarin.Forms.Visual.Material/) -nuget-Paket für die folgenden Sichten enthalten:

- [`Button`](xref:Xamarin.Forms.Button)
- `CheckBox`
- [`Entry`](xref:Xamarin.Forms.Entry)
- [`Frame`](xref:Xamarin.Forms.Frame)
- [`ProgressBar`](xref:Xamarin.Forms.ProgressBar)
- [`DatePicker`](xref:Xamarin.Forms.DatePicker)
- [`TimePicker`](xref:Xamarin.Forms.TimePicker)
- [`Picker`](xref:Xamarin.Forms.Picker)
- [`ActivityIndicator`](xref:Xamarin.Forms.ActivityIndicator)
- [`Editor`](xref:Xamarin.Forms.Editor)
- [`Slider`](xref:Xamarin.Forms.Slider)
- [`Stepper`](xref:Xamarin.Forms.Stepper)

Funktionell unterscheiden sich die materialrenderer nicht von den Standard Renderer.

## <a name="initialize-material-renderers"></a>Material-Renderer initialisieren

Nachdem Sie das [xamarin. Forms. Visual. Material](https://www.nuget.org/packages/Xamarin.Forms.Visual.Material/) -nuget-Paket installiert haben, müssen die Material-Renderer in jedem Platt Form Projekt initialisiert werden.

Unter IOS sollte dies in **AppDelegate.cs** erfolgen, indem die `Xamarin.Forms.FormsMaterial.Init`-Methode *nach* der `Xamarin.Forms.Forms.Init`-Methode aufgerufen wird:

```csharp
global::Xamarin.Forms.Forms.Init();
global::Xamarin.Forms.FormsMaterial.Init();
```

Unter Android sollte dies in **MainActivity.cs** erfolgen, indem die `Xamarin.Forms.FormsMaterial.Init`-Methode *nach* der `Xamarin.Forms.Forms.Init`-Methode aufgerufen wird:

```csharp
global::Xamarin.Forms.Forms.Init(this, savedInstanceState);
global::Xamarin.Forms.FormsMaterial.Init(this, savedInstanceState);
```

## <a name="consume-material-renderers"></a>Nutzbare Renderer verbrauchen

Anwendungen können die Material-Renderer verwenden, indem Sie die [`VisualElement.Visual`](xref:Xamarin.Forms.VisualElement.Visual) -Eigenschaft für eine Seite, ein Layout oder eine Ansicht auf `Material` festlegen:

```xaml
<ContentPage Visual="Material"
             ...>
    ...
</ContentPage>
```

Der entsprechende C#-Code lautet:

```csharp
ContentPage contentPage = new ContentPage();
contentPage.Visual = VisualMarker.Material;
```

Die [`Visual`](xref:Xamarin.Forms.VisualElement.Visual) -Eigenschaft kann auf jeden Typ festgelegt werden, der `IVisual` implementiert, wobei die [`VisualMarker`](xref:Xamarin.Forms.VisualMarker) -Klasse die folgenden `IVisual` Eigenschaften bereitstellt:

- `Default` – gibt an, dass die Ansicht mit dem Standardrenderer renderrender.
- `MatchParent` – gibt an, dass die Ansicht denselben Renderer wie das direkt übergeordnete Element verwenden soll.
- `Material` – gibt an, dass die Sicht mithilfe eines materialrenderers rendererrenderer.

> [!IMPORTANT]
> Die [`Visual`](xref:Xamarin.Forms.VisualElement.Visual) -Eigenschaft wird in der [`VisualElement`](xref:Xamarin.Forms.VisualElement) -Klasse definiert, wobei Sichten den `Visual`-Eigenschafts Wert von ihren übergeordneten Elementen erben. Wenn Sie die `Visual`-Eigenschaft auf einem [`ContentPage`](xref:Xamarin.Forms.ContentPage) festlegen, wird dadurch sichergestellt, dass diese Visualisierung von allen unterstützten Ansichten auf der Seite verwendet wird. Außerdem kann die `Visual`-Eigenschaft für eine Sicht überschrieben werden.

Die folgenden Screenshots zeigen eine Benutzeroberfläche, die mithilfe der Standard-Renderer gerendert wird:

[![Screenshot der Standard-Renderer unter IOS und Android](material-visual-images/default-renderers.png "Sichten, die standardrerenderer verwenden")](material-visual-images/default-renderers-large.png#lightbox)

Die folgenden Screenshots zeigen die gleiche Benutzeroberfläche, die mithilfe der Material-Renderer gerendert wird:

[![Screenshot der Material-Renderer unter IOS und Android](material-visual-images/material-renderers.png "Sichten mit Material Renderer")](material-visual-images/material-renderers-large.png#lightbox)

Die wichtigsten sichtbaren Unterschiede zwischen den standardmäßigen renderatoren und den rendererer für Materialien, die hier gezeigt werden, sind, dass die Material-Renderer [`Button`](xref:Xamarin.Forms.Button) Text in Großbuchstaben schreiben und die Ecken [`Frame`](xref:Xamarin.Forms.Frame) Rahmen abrunden. Allerdings verwenden Material Renderer systemeigene Steuerelemente, sodass es möglicherweise weiterhin Unterschiede zwischen den Plattformen für Bereiche wie Schriftarten, Schatten, Farben und Höhe gibt.

> [!NOTE]
> Material Entwurfs Komponenten halten sich an die Richtlinien von Google. Demzufolge sind die Renderer für Material Entwürfe auf diese Größenanpassung und dieses Verhalten ausgerichtet. Wenn Sie eine bessere Kontrolle über Stile oder das Verhalten benötigen, können Sie dennoch eine eigene [Auswirkung](~/xamarin-forms/app-fundamentals/effects/index.md), ein [Verhalten](~/xamarin-forms/app-fundamentals/behaviors/index.md)oder einen [benutzerdefinierten Renderer](~/xamarin-forms/app-fundamentals/custom-renderer/index.md) erstellen, um die benötigten Details zu erzielen.

## <a name="customize-material-renderers"></a>Anpassen von Material Renderer

Material-Renderer können mithilfe der folgenden Basisklassen optional angepasst werden, genau wie die standardrerenderer:

- `MaterialButtonRenderer`
- `MaterialCheckBoxRenderer`
- `MaterialEntryRenderer`
- `MaterialFrameRenderer`
- `MaterialProgressBarRenderer`
- `MaterialDatePickerRenderer`
- `MaterialTimePickerRenderer`
- `MaterialPickerRenderer`
- `MaterialActivityIndicatorRenderer`
- `MaterialEditorRenderer`
- `MaterialSliderRenderer`
- `MaterialStepperRenderer`

Der folgende Code zeigt ein Beispiel für die Anpassung der `MaterialProgressBarRenderer`-Klasse:

```csharp
using Xamarin.Forms.Material.Android;

[assembly: ExportRenderer(typeof(ProgressBar), typeof(CustomMaterialProgressBarRenderer), new[] { typeof(VisualMarker.MaterialVisual) })]
namespace MyApp.Android
{
    public class CustomMaterialProgressBarRenderer : MaterialProgressBarRenderer
    {
        ...
    }
}
```

In diesem Beispiel gibt der `ExportRendererAttribute` an, dass die `CustomMaterialProgressBarRenderer` Klasse zum Rendering der [`ProgressBar`](xref:Xamarin.Forms.ProgressBar) Ansicht verwendet wird, wobei der `IVisual` Typ als drittes Argument registriert wird.

> [!NOTE]
> Ein Renderer, der einen `IVisual` Typ angibt, wird als Teil seiner `ExportRendererAttribute` verwendet, um die ausgelieferten Ansichten anstelle des standardrenderers zu renderzieren. Beim rendererauswahlzeitpunkt wird die `Visual`-Eigenschaft der Sicht überprüft und in den rendererauswahlvorgang eingeschlossen.

Weitere Informationen zu benutzerdefinierten renderatoren finden Sie unter [benutzerdefinierte Renderer](~/xamarin-forms/app-fundamentals/custom-renderer/index.md).

## <a name="related-links"></a>Verwandte Links

- [Visuelles Material (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-visualdemos)
- [Erstellen eines visuellen xamarin. Forms-Renderers](create.md)
- [Benutzerdefinierte Renderer](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
