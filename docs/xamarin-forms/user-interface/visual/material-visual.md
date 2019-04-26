---
title: Xamarin.Forms-Material Visual
description: Xamarin.Forms-Material-Visual kann zum Erstellen von Xamarin.Forms-Anwendungen, die Aussehen identische oder weitgehend identisch, unter iOS und Android verwendet werden.
ms.prod: xamarin
ms.assetid: B774F68C-EF9E-49E1-B738-CDC64879ADA2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/12/2019
ms.openlocfilehash: cf6ab8266b0798ccbf29078313bbc7454125a1af
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61026790"
---
# <a name="xamarinforms-material-visual"></a>Xamarin.Forms-Material Visual

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/VisualDemos/)

[Materialdesign](https://material.io) ist ein wertendes Design-System, das von Google, das vorschreibt, die Größe, Farbe, Abstand und andere Aspekte der wie Ansichten und Layouts Aussehen und Verhalten soll, erstellt.

Xamarin.Forms-Material-Visual kann verwendet werden, Anwenden von Material Design-Regeln auf Xamarin.Forms-Anwendungen, Erstellen von Anwendungen, die Aussehen identische oder weitgehend die gleichen, unter iOS und Android. Wenn Visual Material aktiviert ist, übernehmen unterstützte Ansichten die gleichen Entwurf plattformübergreifenden, erstellen ein einheitliches Aussehen und Verhalten. Dies erfolgt mit Material Renderern, die die Material Design-Regeln gelten.

Der Prozess zum Aktivieren von Xamarin.Forms Material Visual in Ihrer Anwendung ist:

1. Hinzufügen der [Xamarin.Forms.Visual.Material](https://www.nuget.org/packages/Xamarin.Forms.Visual.Material/) NuGet-Paket zu iOS und Android-Plattform-Projekten. Dieses NuGet-Paket bietet optimierte Material Design-Renderer für iOS und Android. Unter iOS, bietet das Paket die transitive Abhängigkeit hinzu [Xamarin.iOS.MaterialComponents](https://www.nuget.org/packages/Xamarin.iOS.MaterialComponents), d.h. eine C# Binden an Google [Material-Komponenten für iOS](https://material.io/develop/ios/). Unter Android enthält das Paket Build-Ziele, um sicherzustellen, dass Ihre TargetFramework ordnungsgemäß eingerichtet ist.
1. Initialisieren der Material-Renderer in jedem plattformprojekt. Weitere Informationen finden Sie unter [initialisieren Material Renderer](#initialize-material-renderers).
1. Nutzen Sie durch Festlegen der Material-Renderer die [ `Visual` ](xref:Xamarin.Forms.VisualElement.Visual) Eigenschaft `Material` auf den Seiten, die die Regeln Material Design übernehmen sollten. Weitere Informationen finden Sie unter [nutzen Sie Material Renderer](#consume-material-renderers).
1. [optional] Anpassen der Material-Renderer. Weitere Informationen finden Sie unter [Material Renderer anpassen](#customize-material-renderers).

> [!IMPORTANT]
> Unter Android erfordern die Material-Renderer mindestens Version 5.0 (API 21) oder höher, und einen TargetFramework-Version 9.0 (28-API). Darüber hinaus erfordert das plattformprojekt für Android-Unterstützungsbibliotheken 28.0.0 oder höher, und das Design von Komponenten von Material Design zu erben, oder ein Design AppCompat erben weiterhin benötigt. Weitere Informationen finden Sie unter [erste Schritte mit der Material-Komponenten für Android](https://github.com/material-components/material-components-android/blob/master/docs/getting-started.md).

Material Renderer befinden sich derzeit der [Xamarin.Forms.Visual.Material](https://www.nuget.org/packages/Xamarin.Forms.Visual.Material/) NuGet-Paket für die folgenden Ansichten:

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

## <a name="initialize-material-renderers"></a>Initialisieren von Material-Renderer

Nach der Installation der [Xamarin.Forms.Visual.Material](https://www.nuget.org/packages/Xamarin.Forms.Visual.Material/) NuGet-Paket, das Material Renderer müssen in jedes plattformprojekt initialisiert werden.

Unter iOS, sollte dies in auftreten **Datei "appdelegate.cs"** durch Aufrufen der `FormsMaterial.Init` Methode *nach* der `Xamarin.Forms.Forms.Init` Methode:

```csharp
global::Xamarin.Forms.Forms.Init();
FormsMaterial.Init();
```

Unter Android, sollte dies in auftreten **"mainactivity.cs"** durch Aufrufen der `FormsMaterial.Init` Methode *nach* der `Xamarin.Forms.Forms.Init` Methode:

```csharp
global::Xamarin.Forms.Forms.Init(this, savedInstanceState);
FormsMaterial.Init(this, savedInstanceState);
```

## <a name="consume-material-renderers"></a>Nutzen Sie Material Renderer

Anwendungen können festlegen, mit dem Material Renderer durch Festlegen der [ `VisualElement.Visual` ](xref:Xamarin.Forms.VisualElement.Visual) Eigenschaft für eine Seite, die Layout oder die Sicht, um `Material`:

```xaml
<ContentPage Visual="Material"
             ...>
    ...
</ContentPage>
```

Der entsprechende C#-Code ist:

```csharp
ContentPage contentPage = new ContentPage();
contentPage.Visual = VisualMarker.Material;
```

Die [ `Visual` ](xref:Xamarin.Forms.VisualElement.Visual) Eigenschaft kann festgelegt werden, auf einen beliebigen Typ, der implementiert `IVisual`, mit der [ `VisualMarker` ](xref:Xamarin.Forms.VisualMarker) -Klasse, die Folgendes bereitstellt `IVisual` Eigenschaften:

- `Default` – Gibt an, dass die Ansicht mit dem Standardrenderer gerendert werden soll.
- `MatchParent` – Gibt an, dass die Ansicht den gleichen-Renderer der direkt übergeordnete Element verwenden soll.
- `Material` – Gibt an, dass die Ansicht Renderer mit einem Material gerendert werden soll.

> [!IMPORTANT]
> Die [ `Visual` ](xref:Xamarin.Forms.VisualElement.Visual) Eigenschaft wird definiert, der [ `VisualElement` ](xref:Xamarin.Forms.VisualElement) Klasse mit Vererbung Ansichten der `Visual` Eigenschaftswert von ihren übergeordneten Elementen. Daher ist das Festlegen der `Visual` Eigenschaft für eine [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) wird sichergestellt, dass alle unterstützten Ansichten auf der Seite auf diese Visualisierung verwendet werden. Darüber hinaus die `Visual` Eigenschaft kann für eine Sicht überschrieben werden.

Die folgenden Screenshots zeigen eine Benutzeroberfläche, die mit dem Standard-Renderer gerendert:

[![Screenshot der Standard-Renderer, unter iOS und Android](material-visual-images/default-renderers.png "Ansichten mit Standard-Renderer")](material-visual-images/default-renderers-large.png#lightbox)

Die folgenden Screenshots zeigen die gleiche Benutzeroberfläche, die mit der Material-Renderer gerendert:

[![Screenshot der Material-Renderer, unter iOS und Android](material-visual-images/material-renderers.png "Ansichten mit der Material-Renderer")](material-visual-images/material-renderers-large.png#lightbox)

Die Hauptunterschiede zwischen dem Standard-Renderer und dem Material Renderern, die hier gezeigten sichtbar sind, profitieren von der Material-Renderer [ `Button` ](xref:Xamarin.Forms.Button) Text und Abrunden der Ecken [ `Frame` ](xref:Xamarin.Forms.Frame)Rahmen. Allerdings Material Renderern verwendet native Steuerelemente, und aus diesem Grund werden ggf. noch Unterschiede bei der Benutzeroberfläche zwischen den Plattformen für Bereiche wie Schriftarten, Schatten, Farben und Erhöhung der Rechte.

> [!NOTE]
> Material Design-Komponenten eng an die Google Richtlinien. Daher beziehen sich auf Material Design-Renderer, Größe und Verhalten. Wenn Sie eine bessere Steuerung der Stile oder Verhalten benötigen, können Sie immer noch erstellen Ihre eigenen [Auswirkung](~/xamarin-forms/app-fundamentals/effects/index.md), [Verhalten](~/xamarin-forms/app-fundamentals/behaviors/index.md), oder [Custom Renderer](~/xamarin-forms/app-fundamentals/custom-renderer/index.md) um die Details zu erreichen, Sie benötigen.

## <a name="customize-material-renderers"></a>Anpassen von Material-Renderer

Material-Renderer können optional, wie die Standard-Renderer, durch den folgenden Basisklassen angepasst werden:

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

In diesem Beispiel die `ExportRendererAttribute` gibt an, dass die `CustomMaterialProgressBarRenderer` Klasse wird verwendet, um das Rendern der [ `ProgressBar` ](xref:Xamarin.Forms.ProgressBar) anzeigen, mit der `IVisual` Typ registriert als drittes Argument.

> [!NOTE]
> Ein Renderer, der angibt, ein `IVisual` Typ als Teil der `ExportRendererAttribute`, wird verwendet, um Rendern von Ansichten, statt der Standard-Renderer aktiviert. Zum Zeitpunkt der Renderer Auswahl der `Visual` -Eigenschaft der Ansicht werden untersucht und in den Renderer Prozess enthalten.

Weitere Informationen zu benutzerdefinierten Renderern finden Sie unter [benutzerdefinierte Renderer](~/xamarin-forms/app-fundamentals/custom-renderer/index.md).

## <a name="related-links"></a>Verwandte Links

- [Material-Visual (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/VisualDemos/)
- [Erstellen eines Xamarin.Forms-Visual-Renderers](create.md)
- [Benutzerdefinierte Renderer](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
