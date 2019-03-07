---
title: Xamarin.Forms-Material Visual
description: Xamarin.Forms-Material-Visual kann zum Erstellen von Xamarin.Forms-Anwendungen, die Aussehen identische oder weitgehend identisch, unter iOS und Android verwendet werden.
ms.prod: xamarin
ms.assetid: B774F68C-EF9E-49E1-B738-CDC64879ADA2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/05/2019
ms.openlocfilehash: 46ffe29f898e2f7bd8fcbe759f5a2f54e3dd60e1
ms.sourcegitcommit: 00744f754527e5b55154365f89691caaf1c9d929
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/07/2019
ms.locfileid: "57557502"
---
# <a name="xamarinforms-material-visual"></a>Xamarin.Forms-Material Visual

Xamarin.Forms-Material-Visual kann zum Erstellen von Xamarin.Forms-Anwendungen, die Aussehen identische oder weitgehend identisch, unter iOS und Android verwendet werden. Dies erfolgt mithilfe von zusätzlichen Renderern, die eine grundlegenden Darstellung mit Anwendungen, die Entscheidung für die Darstellung durch Implementieren der `Visual` Eigenschaft:

```xaml
<ContentPage ...
             Visual="Material">
    ...
</ContentPage>
```

Der entsprechende C#-Code ist:

```csharp
ContentPage contentPage = new ContentPage();
contentPage.Visual = VisualMarker.Material;
```

> [!IMPORTANT]
> Die `Visual` Eigenschaft wird definiert, der `VisualElement` Klasse mit Vererbung Ansichten der `Visual` Eigenschaftswert von ihren übergeordneten Elementen. Daher ist das Festlegen der `Visual` Eigenschaft für eine `ContentPage` wird sichergestellt, dass alle unterstützten Ansichten auf der Seite, visuelle Darstellung verwenden. Darüber hinaus die `Visual` Eigenschaft kann für eine Sicht überschrieben werden.

Material Renderer sind derzeit enthalten, für die folgenden Ansichten unter iOS und Android:

- [`Button`](xref:Xamarin.Forms.Button)
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

Funktionell unterscheiden sich die Material-Renderer nicht auf die Standard-Renderer.

Unter iOS und Android, das plattformprojekt müssen die [Xamarin.Forms.Visual.Material](https://www.nuget.org/packages/ https://www.nuget.org/packages/Xamarin.Forms.Visual.Material/) NuGet-Paket installiert. Nach der Installation des NuGet-Pakets ist erforderlich, Code zum Initialisieren der in jedem plattformprojekt *nach* der `Xamarin.Forms.Forms.Init` Methodenaufruf. Verwenden Sie für iOS den folgenden Code ein:

```csharp
global::Xamarin.Forms.Forms.Init();
FormsMaterial.Init();
```

Unter Android, müssen Sie die gleichen Parameter, die übergeben werden, übergeben `Forms.Init`:

```csharp
global::Xamarin.Forms.Forms.Init(this, bundle);
FormsMaterial.Init(this, bundle);
```

> [!IMPORTANT]
> Unter Android funktioniert Material nur TargetFramework 9.0 (28-API). Darüber hinaus das plattformprojekt muss v28 Unterstützung Bibliotheken verwenden, und das Design von Komponenten von Material Design zu erben, oder ein Design AppCompat erben weiterhin benötigt. Weitere Informationen finden Sie unter [erste Schritte mit der Material-Komponenten für Android](https://github.com/material-components/material-components-android/blob/master/docs/getting-started.md).

Die folgenden Screenshots zeigen eine Benutzeroberfläche, die die vier Ansichten enthält, für welches, die, die Material Renderer vorhanden, aber mit dem Standard-Renderer gerendert:

[![Screenshot der Standard-Renderer, unter iOS und Android](material-visual-images/default-renderers.png "Ansichten mit Standard-Renderer")](material-visual-images/default-renderers-large.png#lightbox)

Die folgenden Screenshots zeigen die gleiche Benutzeroberfläche, die mit der Material-Renderer gerendert:

[![Screenshot der Material-Renderer, unter iOS und Android](material-visual-images/material-renderers.png "Ansichten mit der Material-Renderer")](material-visual-images/material-renderers-large.png#lightbox)

> [!NOTE]
> Klicken Sie mit Material Renderer die gerenderten Steuerelemente bleiben nativen Steuerelementen, und aus diesem Grund wird immer noch vorhanden sein Unterschiede bei der Benutzeroberfläche zwischen den Plattformen für Bereiche wie Schriftarten, Schatten, Farben und Erhöhung der Rechte.

## <a name="material-renderers"></a>Material-Renderer

Material-Renderer können, wie der Standard-Renderer, angepasst werden mit den folgenden Basisklassen:

- `MaterialButtonRenderer`
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

Der folgende Code zeigt ein Beispiel für das Anpassen der `MaterialProgressBarRenderer` Klasse:

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

## <a name="related-links"></a>Verwandte Links

- [Benutzerdefinierte Renderer](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
