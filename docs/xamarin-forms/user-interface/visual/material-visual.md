---
title: Xamarin.FormsMaterial Visualisierung
description: Xamarin.FormsDas visuelle Material kann verwendet werden, um Anwendungen zu erstellen Xamarin.Forms , die unter IOS und Android größtenteils identisch aussehen.
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: bba7d77d8cf565b1b2db2c1324e171389c5d0280
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2020
ms.locfileid: "84127178"
---
# <a name="xamarinforms-material-visual"></a>Xamarin.FormsMaterial Visualisierung

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-visualdemos)

[Material Design](https://material.io) ist ein von Google erstelltes Entwurfssystem, das die Größe, die Farbe, den Abstand und andere Aspekte der Anzeige und des Verhaltens von Ansichten und Layouts festlegt.

Xamarin.FormsMaterial Visual kann verwendet werden, um Material Design Regeln auf Xamarin.Forms Anwendungen anzuwenden und Anwendungen zu erstellen, die unter IOS und Android größtenteils identisch aussehen. Wenn die visuelle Visualisierung von Material aktiviert ist, übernehmen unterstützte Ansichten denselben Entwurf plattformübergreifend und sorgen für ein einheitliches Aussehen und Gefühl.

[![Visuelle Material-Screenshots](material-visual-images/material-visual-cropped.png)](material-visual-images/material-visual.png#lightbox)

Der Prozess zum Aktivieren der Xamarin.Forms visuellen Material Visualisierung in der Anwendung lautet:

1. Fügen Sie hinzu [ Xamarin.Forms . Visual. Material](https://www.nuget.org/packages/Xamarin.Forms.Visual.Material/) nuget-Paket zu ihren IOS-und Android-Platt Form Projekten. Dieses nuget-Paket bietet optimierte Material Design-Renderer unter IOS und Android. Unter IOS bietet das Paket die transitiv Abhängigkeit zu [xamarin. IOS. materialcomponents](https://www.nuget.org/packages/Xamarin.iOS.MaterialComponents), bei der es sich um eine c#-Bindung an die [Material Komponenten von Google für IOS](https://material.io/develop/ios/)handelt. Unter Android stellt das Paket Buildziele bereit, um sicherzustellen, dass das TargetFramework ordnungsgemäß eingerichtet ist.
1. Initialisieren der visuellen Material Visualisierung in den einzelnen Platt Form Projekten. Weitere Informationen finden Sie unter [Initialisieren der visuellen Material Visualisierung](#initialize-material-visual).
1. Erstellen Sie visuelle Material Steuerelemente, indem Sie die- [`Visual`](xref:Xamarin.Forms.VisualElement.Visual) Eigenschaft `Material` auf allen Seiten festlegen, die die Material Entwurfs Regeln übernehmen sollen. Weitere Informationen finden Sie unter [nutzungsmaterialrenderer](#apply-material-visual).
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

Nach der Installation von [ Xamarin.Forms . Visual. Material](https://www.nuget.org/packages/Xamarin.Forms.Visual.Material/) nuget-Paket, die Material-Renderer müssen in jedem Platt Form Projekt initialisiert werden.

Unter IOS sollte dies in **AppDelegate.cs** erfolgen, indem die- `Xamarin.Forms.FormsMaterial.Init` Methode *nach* der-Methode aufgerufen wird `Xamarin.Forms.Forms.Init` :

```csharp
global::Xamarin.Forms.Forms.Init();
global::Xamarin.Forms.FormsMaterial.Init();
```

Unter Android sollte dies in **MainActivity.cs** erfolgen, indem die- `Xamarin.Forms.FormsMaterial.Init` Methode *nach* der-Methode aufgerufen wird `Xamarin.Forms.Forms.Init` :

```csharp
global::Xamarin.Forms.Forms.Init(this, savedInstanceState);
global::Xamarin.Forms.FormsMaterial.Init(this, savedInstanceState);
```

## <a name="apply-material-visual"></a>Material visuelles Element anwenden

Anwendungen können die visuelle Materialisierungen aktivieren, indem Sie die- [`VisualElement.Visual`](xref:Xamarin.Forms.VisualElement.Visual) Eigenschaft auf einer Seite, einem Layout oder einer Ansicht auf Folgendes festlegen `Material` :

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

Wenn Sie die- `VisualElement.Visual` Eigenschaft auf festlegen, wird `Material` Ihre Anwendung angewiesen, anstelle der Standard-Renderer die visuellen Renderer für Material zu verwenden. Die- [`Visual`](xref:Xamarin.Forms.VisualElement.Visual) Eigenschaft kann auf einen beliebigen Typ festgelegt werden, der implementiert `IVisual` , wobei die- [`VisualMarker`](xref:Xamarin.Forms.VisualMarker) Klasse die folgenden Eigenschaften bereitstellt `IVisual` :

- `Default`– Gibt an, dass die Ansicht mithilfe des standardrenderers renderrender.
- `MatchParent`– Gibt an, dass die Ansicht denselben Renderer wie das direkt übergeordnete Element verwenden soll.
- `Material`– Gibt an, dass die Sicht mithilfe eines materialrenderers rendererrenderer.

> [!IMPORTANT]
> Die- [`Visual`](xref:Xamarin.Forms.VisualElement.Visual) Eigenschaft wird in der- [`VisualElement`](xref:Xamarin.Forms.VisualElement) Klasse definiert, wobei Sichten den `Visual` Eigenschafts Wert von ihren übergeordneten Elementen erben. Daher wird durch das Festlegen der- `Visual` Eigenschaft auf eine [`ContentPage`](xref:Xamarin.Forms.ContentPage) sichergestellt, dass alle unterstützten Sichten auf der Seite diese Visualisierung verwenden. Außerdem kann die- `Visual` Eigenschaft für eine Sicht überschrieben werden.

Die folgenden Screenshots zeigen eine Benutzeroberfläche, die mithilfe der Standard-Renderer gerendert wird:

[![Screenshot der Standard-Renderer unter IOS und Android](material-visual-images/default-renderers.png "Sichten, die standardrerenderer verwenden")](material-visual-images/default-renderers-large.png#lightbox)

Die folgenden Screenshots zeigen die gleiche Benutzeroberfläche, die mithilfe der Material-Renderer gerendert wird:

[![Screenshot der Material-Renderer unter IOS und Android](material-visual-images/material-renderers.png "Sichten mit Material Renderer")](material-visual-images/material-renderers-large.png#lightbox)

Die wichtigsten sichtbaren Unterschiede zwischen den standardmäßigen renderatoren und den Material-renderatoren, wie hier gezeigt, sind, dass der Material-Renderer groß-und Kleinbuchstaben nutzt [`Button`](xref:Xamarin.Forms.Button) und die Ecken der Rahmen abrundet [`Frame`](xref:Xamarin.Forms.Frame) . Allerdings verwenden Material Renderer systemeigene Steuerelemente, sodass es möglicherweise weiterhin Unterschiede zwischen den Plattformen für Bereiche wie Schriftarten, Schatten, Farben und Höhe gibt.

> [!NOTE]
> Material Entwurfs Komponenten halten sich an die Richtlinien von Google. Demzufolge sind die Renderer für Material Entwürfe auf diese Größenanpassung und dieses Verhalten ausgerichtet. Wenn Sie eine bessere Kontrolle über Stile oder das Verhalten benötigen, können Sie dennoch eine eigene [Auswirkung](~/xamarin-forms/app-fundamentals/effects/index.md), ein [Verhalten](~/xamarin-forms/app-fundamentals/behaviors/index.md)oder einen [benutzerdefinierten Renderer](~/xamarin-forms/app-fundamentals/custom-renderer/index.md) erstellen, um die benötigten Details zu erzielen.

## <a name="customize-material-visual"></a>Material Visualisierung anpassen

Das Material Visual nuget-Paket ist eine Sammlung von Renderer, die die Steuer Xamarin.Forms Elemente erkennen. Das Anpassen von visuellen Material Steuerelementen ist identisch mit der Anpassung von Standard Steuerelementen.

Effekte sind das empfohlene Verfahren, wenn ein vorhandenes Steuerelement angepasst werden soll. Wenn ein visueller Renderer für Material vorhanden ist, ist es weniger Aufwand, das Steuerelement mit einem Effekt anzupassen, als es für die Unterklasse des Renderers gilt. Weitere Informationen zu den Auswirkungen finden Sie unter [ Xamarin.Forms Effekte](~/xamarin-forms/app-fundamentals/effects/index.md).

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

Die Unterklassen für einen materialrenderer sind fast identisch mit nicht-Material-Renderern. Wenn Sie jedoch einen Renderer exportieren, der einen Material-Renderer Unterklassen ausführt, müssen Sie ein drittes Argument für das Attribut bereitstellen, `ExportRenderer` das den `VisualMarker.MaterialVisual` Typ angibt:

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

In diesem Beispiel gibt das `ExportRendererAttribute` an, dass die- `CustomMaterialProgressBarRenderer` Klasse zum Rendering der Ansicht verwendet wird [`ProgressBar`](xref:Xamarin.Forms.ProgressBar) , wobei der `IVisual` Typ als drittes Argument registriert ist.

> [!NOTE]
> Ein Renderer, der einen- `IVisual` Typ als Teil seines angibt `ExportRendererAttribute` , wird verwendet, um die ausgewählten Ansichten anstelle des standardrenderers zu renderzieren. Bei der Auswahl Zeit des Renderers `Visual` wird die-Eigenschaft der Sicht überprüft und in den rendererauswahlvorgang eingeschlossen.

Weitere Informationen zu benutzerdefinierten renderatoren finden Sie unter [benutzerdefinierte Renderer](~/xamarin-forms/app-fundamentals/custom-renderer/index.md).

## <a name="related-links"></a>Verwandte Links

- [Visuelles Material (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-visualdemos)
- [Erstellen eines Xamarin.Forms visuellen Renderers](create.md)
- [Xamarin.FormsEinflüssen](~/xamarin-forms/app-fundamentals/effects/index.md)
- [Benutzerdefinierte Renderer](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
