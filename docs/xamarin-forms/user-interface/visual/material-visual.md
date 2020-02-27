---
title: Visuelles xamarin. Forms-Material
description: Xamarin. Forms Material Visual kann verwendet werden, um xamarin. Forms-Anwendungen zu erstellen, die unter IOS und Android größtenteils identisch aussehen.
ms.prod: xamarin
ms.assetid: B774F68C-EF9E-49E1-B738-CDC64879ADA2
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 11/25/2019
ms.openlocfilehash: 81ef3c44d6eb8aaf4dd0ec467e11ef04adb02a76
ms.sourcegitcommit: 5a6124271a679b8961fa9430bd738fcb18544e92
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/26/2020
ms.locfileid: "77618402"
---
# <a name="xamarinforms-material-visual"></a>Visuelles xamarin. Forms-Material

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-visualdemos)

[Material Design](https://material.io) ist ein von Google erstelltes Entwurfssystem, das die Größe, die Farbe, den Abstand und andere Aspekte der Anzeige und des Verhaltens von Ansichten und Layouts festlegt.

Das visuelle xamarin. Forms-Material kann verwendet werden, um Material Entwurfs Regeln auf xamarin. Forms-Anwendungen anzuwenden und Anwendungen zu erstellen, die unter IOS und Android größtenteils identisch aussehen. Wenn die visuelle Visualisierung von Material aktiviert ist, übernehmen unterstützte Ansichten denselben Entwurf plattformübergreifend und sorgen für ein einheitliches Aussehen und Gefühl.

[visuelle Screenshots der ![Materialien](material-visual-images/material-visual-cropped.png)](material-visual-images/material-visual.png#lightbox)

Der Prozess zum Aktivieren der visuellen xamarin. Forms-Materialisierungen in der Anwendung lautet wie folgt:

1. Fügen Sie das nuget-Paket [xamarin. Forms. Visual. Material](https://www.nuget.org/packages/Xamarin.Forms.Visual.Material/) zu ihren IOS-und Android-Platt Form Projekten hinzu. Dieses nuget-Paket bietet optimierte Material Design-Renderer unter IOS und Android. Unter IOS stellt das Paket die transitiv Abhängigkeit zu [xamarin. IOS. materialcomponents](https://www.nuget.org/packages/Xamarin.iOS.MaterialComponents)bereit. Hierbei handelt es C# sich um eine Bindung an die [Material Komponenten von Google für IOS](https://material.io/develop/ios/). Unter Android stellt das Paket Buildziele bereit, um sicherzustellen, dass das TargetFramework ordnungsgemäß eingerichtet ist.
1. Initialisieren der visuellen Material Visualisierung in den einzelnen Platt Form Projekten. Weitere Informationen finden Sie unter [Initialisieren der visuellen Material Visualisierung](#initialize-material-visual).
1. Erstellen Sie visuelle Steuerelemente, indem Sie die [`Visual`](xref:Xamarin.Forms.VisualElement.Visual) -Eigenschaft auf alle Seiten `Material` festlegen, die die Material Entwurfs Regeln übernehmen sollen. Weitere Informationen finden Sie unter [nutzungsmaterialrenderer](#apply-material-visual).
1. optionale Anpassen von Material Steuerelementen. Weitere Informationen finden Sie unter [Anpassen von Material Steuerelementen](#customize-material-visual).

> [!IMPORTANT]
> Unter Android erfordert die Material Visualisierung mindestens eine Version von 5,0 (API 21) oder höher und ein TargetFramework der Version 9,0 (API 28). Außerdem erfordert Ihr Platt Form Projekt Android-Unterstützungs Bibliotheken 28.0.0 oder höher, und das Design muss von einem Material Components-Design erben oder weiterhin von einem AppCompat-Design erben. Weitere Informationen finden Sie unter [Getting Started with Material Components for Android](https://github.com/material-components/material-components-android/blob/master/docs/getting-started.md).

Material Visual unterstützt derzeit die folgenden Steuerelemente:

- [`ActivityIndicator`](xref:Xamarin.Forms.ActivityIndicator)
- [`Button`](xref:Xamarin.Forms.Button)
- [`CheckBox`](xref:Xamarin.Forms.CheckBox)
- [`DatePicker`](xref:Xamarin.Forms.DatePicker)
- [`Editor`](xref:Xamarin.Forms.Editor)
- [`Entry`](xref:Xamarin.Forms.Entry)
- [`Frame`](xref:Xamarin.Forms.Frame)
- [`Picker`](xref:Xamarin.Forms.Picker)
- [`ProgressBar`](xref:Xamarin.Forms.ProgressBar)
- [`Slider`](xref:Xamarin.Forms.Slider)
- [`Stepper`](xref:Xamarin.Forms.Stepper)
- [`TimePicker`](xref:Xamarin.Forms.TimePicker)

Material-Steuerelemente werden durch materialrenderer realisiert, die die Material Entwurfs Regeln anwenden. Funktionale, materialrenderer unterscheiden sich nicht von den Standard renderatoren. Weitere Informationen finden Sie unter [Anpassen der visuellen Material Visualisierung](#customize-material-visual).

## <a name="initialize-material-visual"></a>Visuelle Material Visualisierung initialisieren

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

## <a name="apply-material-visual"></a>Material visuelles Element anwenden

Anwendungen können das visuelle Material visuell aktivieren, indem Sie die [`VisualElement.Visual`](xref:Xamarin.Forms.VisualElement.Visual) -Eigenschaft auf einer Seite, einem Layout oder einer Ansicht auf `Material`festlegen:

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

Wenn Sie die `VisualElement.Visual`-Eigenschaft auf `Material` festlegen, wird Ihre Anwendung angewiesen, die visuellen Renderer für Material anstelle der Standard-Renderer zu verwenden. Die [`Visual`](xref:Xamarin.Forms.VisualElement.Visual) -Eigenschaft kann auf jeden Typ festgelegt werden, der `IVisual`implementiert, wobei die [`VisualMarker`](xref:Xamarin.Forms.VisualMarker) -Klasse die folgenden `IVisual` Eigenschaften bereitstellt:

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

## <a name="customize-material-visual"></a>Material Visualisierung anpassen

Das Material Visual nuget-Paket ist eine Sammlung von Renderer, die die xamarin. Forms-Steuerelemente erkennen. Das Anpassen von visuellen Material Steuerelementen ist identisch mit der Anpassung von Standard Steuerelementen.

Effekte sind das empfohlene Verfahren, wenn ein vorhandenes Steuerelement angepasst werden soll. Wenn ein visueller Renderer für Material vorhanden ist, ist es weniger Aufwand, das Steuerelement mit einem Effekt anzupassen, als es für die Unterklasse des Renderers gilt. Weitere Informationen zu den Auswirkungen finden Sie unter [xamarin. Forms-Effekte](~/xamarin-forms/app-fundamentals/effects/index.md).

Benutzerdefinierte Renderer sind das empfohlene Verfahren, wenn kein materialrenderer vorhanden ist. Die folgenden rendererklassen sind in der visuellen Material Visualisierung enthalten:

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

Die Unterklassen für einen materialrenderer sind fast identisch mit nicht-Material-Renderern. Wenn Sie jedoch einen Renderer exportieren, der einen Material-Renderer Unterklassen ausführt, müssen Sie ein drittes Argument für das `ExportRenderer`-Attribut bereitstellen, das den `VisualMarker.MaterialVisual` Typ angibt:

```csharp
using Xamarin.Forms.Material.Android;

[assembly: ExportRenderer(typeof(ProgressBar), typeof(CustomMaterialProgressBarRenderer), new[] { typeof(VisualMarker.MaterialVisual) })]
namespace MyApp.Android
{
    public class CustomMaterialProgressBarRenderer : MaterialProgressBarRenderer
    {
        //...
    }
}
```

In diesem Beispiel gibt der `ExportRendererAttribute` an, dass die `CustomMaterialProgressBarRenderer` Klasse zum Rendering der [`ProgressBar`](xref:Xamarin.Forms.ProgressBar) Ansicht verwendet wird, wobei der `IVisual` Typ als drittes Argument registriert wird.

> [!NOTE]
> Ein Renderer, der einen `IVisual` Typ angibt, wird als Teil seiner `ExportRendererAttribute`verwendet, um die ausgelieferten Ansichten anstelle des standardrenderers zu renderzieren. Beim rendererauswahlzeitpunkt wird die `Visual`-Eigenschaft der Sicht überprüft und in den rendererauswahlvorgang eingeschlossen.

Weitere Informationen zu benutzerdefinierten renderatoren finden Sie unter [benutzerdefinierte Renderer](~/xamarin-forms/app-fundamentals/custom-renderer/index.md).

## <a name="related-links"></a>Verwandte Links

- [Visuelles Material (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-visualdemos)
- [Erstellen eines visuellen xamarin. Forms-Renderers](create.md)
- [Xamarin. Forms-Effekte](~/xamarin-forms/app-fundamentals/effects/index.md)
- [Benutzerdefinierte Renderer](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
