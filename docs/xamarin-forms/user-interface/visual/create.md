---
title: Erstellen eines Xamarin.Forms-Visual-Renderers
description: Erstellen von Xamarin.Forms-Visualisierungen, selektiv auf VisualElement Objekte angewendet werden, ohne dass Unterklasse Xamarin.Forms-Ansichten.
ms.prod: xamarin
ms.assetid: 80BF9C72-AC28-4AAF-9DDD-B60CBDD1CD59
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/12/2019
ms.openlocfilehash: a11c2045fa6119d0689834c35794bc8913c80bd6
ms.sourcegitcommit: a7170494e1975f0f1be547a45444752fd8e57819
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2019
ms.locfileid: "58506966"
---
# <a name="create-a-xamarinforms-visual-renderer"></a>Erstellen eines Xamarin.Forms-Visual-Renderers

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/VisualDemos/)

Xamarin.Forms Visual ermöglicht Renderer erstellt und selektiv auf angewendet werden [ `VisualElement` ](xref:Xamarin.Forms.VisualElement) Objekte, ohne dass Unterklasse Xamarin.Forms-Ansichten. Ein Renderer, der angibt, ein `IVisual` Typ als Teil der `ExportRendererAttribute`, wird verwendet, um Rendern von Ansichten, statt der Standard-Renderer aktiviert. Zum Zeitpunkt der Renderer Auswahl der `Visual` -Eigenschaft der Ansicht werden untersucht und in den Renderer Prozess enthalten.

> [!IMPORTANT]
> Derzeit den [ `Visual` ](xref:Xamarin.Forms.VisualElement.Visual) Eigenschaft kann nicht geändert werden, nachdem die Ansicht gerendert wurde, aber dadurch wird in einer zukünftigen Version geändert.

Der Prozess zum Erstellen und nutzen einen Xamarin.Forms-Visual-Renderer ist:

1. Plattform-Renderer für die gewünschte Ansicht zu erstellen. Weitere Informationen finden Sie unter [Renderer erstellen](#create-platform-renderers).
1. Erstellen Sie einen Typ, die von abgeleitet `IVisual`. Weitere Informationen finden Sie unter [erstellen Sie einen Typ IVisual](#create-an-ivisual-type).
1. Registrieren der `IVisual` Typ als Teil der `ExportRendererAttribute` , die die Renderer ergänzt. Weitere Informationen finden Sie unter [den Typ zu registrieren IVisual](#register-the-ivisual-type).
1. Nutzen Sie die Visual-Renderer durch Festlegen der [ `Visual` ](xref:Xamarin.Forms.VisualElement.Visual) Eigenschaft für die Sicht auf die `IVisual` Name. Weitere Informationen finden Sie unter [nutzen den Visual-Renderer](#consume-the-visual-renderer).
1. [optional] Registrieren Sie einen Namen für die `IVisual` Typ. Weitere Informationen finden Sie unter [registrieren Sie einen Namen für die IVisual-Typ](#register-a-name-for-the-ivisual-type).

## <a name="create-platform-renderers"></a>Erstellen von Plattform-Renderer

Weitere Informationen zum Erstellen einer Rendererklasse, finden Sie unter [benutzerdefinierten Renderern](~/xamarin-forms/app-fundamentals/custom-renderer/index.md). Beachten Sie jedoch, dass eine Xamarin.Forms-Visual-Renderers einer Ansicht angewendet wird, ohne Unterklasse der Ansicht.

Die Renderingklassen, die hier beschriebenen implementieren ein benutzerdefinierten [ `Button` ](xref:Xamarin.Forms.Button) , dessen Text mit einem Schatten anzeigt.

### <a name="ios"></a>iOS

Das folgende Codebeispiel zeigt den Schaltfläche-Renderer für iOS:

```csharp
public class CustomButtonRenderer : ButtonRenderer
{
    protected override void OnElementChanged(ElementChangedEventArgs<Button> e)
    {
        base.OnElementChanged(e);

        if (e.OldElement != null)
        {
            // Cleanup
        }

        if (e.NewElement != null)
        {
            Control.TitleShadowOffset = new CoreGraphics.CGSize(1, 1);
            Control.SetTitleShadowColor(Color.Black.ToUIColor(), UIKit.UIControlState.Normal);
        }
    }
}
```

### <a name="android"></a>Android

Das folgende Codebeispiel zeigt die Schaltfläche "-Renderer für Android:

```csharp
public class CustomButtonRenderer : Xamarin.Forms.Platform.Android.AppCompat.ButtonRenderer
{
    public CustomButtonRenderer(Context context) : base(context)
    {
    }

    protected override void OnElementChanged(ElementChangedEventArgs<Button> e)
    {
        base.OnElementChanged(e);

        if (e.OldElement != null)
        {
            // Cleanup
        }

        if (e.NewElement != null)
        {
            Control.SetShadowLayer(5, 3, 3, Color.Black.ToAndroid());
        }
    }
}
```

## <a name="create-an-ivisual-type"></a>Erstellen Sie einen IVisual-Typ

Erstellen Sie in Ihrer Bibliothek plattformübergreifende einen Typ, der abgeleitet `IVisual`:

```csharp
public class CustomVisual : IVisual
{
}
```

Die `CustomVisual` Typ kann dann erfasst werden für den Renderingklassen zulassen [ `Button` ](xref:Xamarin.Forms.Button) Objekte festlegen, mit dem Renderer.

## <a name="register-the-ivisual-type"></a>Registrieren Sie sich die IVisual-Typ

In den plattformprojekten, ergänzen die Renderingklassen, mit der `ExportRendererAttribute`:

```csharp
[assembly: ExportRenderer(typeof(Xamarin.Forms.Button), typeof(CustomButtonRenderer), new[] { typeof(CustomVisual) })]
public class CustomButtonRenderer : ButtonRenderer
{
    protected override void OnElementChanged(ElementChangedEventArgs<Button> e)
    {
        ...
    }
}
```

In diesem Beispiel die `ExportRendererAttribute` gibt an, dass die `CustomButtonRenderer` Klasse wird verwendet, um die Nutzung zu rendern [ `Button` ](xref:Xamarin.Forms.Button) Objekte, mit der `IVisual` Typ registriert als drittes Argument. Ein Renderer, der angibt, ein `IVisual` Typ als Teil der `ExportRendererAttribute`, wird verwendet, um Rendern von Ansichten, statt der Standard-Renderer aktiviert.

## <a name="consume-the-visual-renderer"></a>Nutzen Sie die Visual-renderer

Ein [ `Button` ](xref:Xamarin.Forms.Button) Objekt kann festlegen, mit der Renderingklassen durch Festlegen seiner [ `Visual` ](xref:Xamarin.Forms.VisualElement.Visual) Eigenschaft `Custom`:

```xaml
<Button Visual="Custom"
        Text="CUSTOM BUTTON"
        BackgroundColor="{StaticResource PrimaryColor}"
        TextColor="{StaticResource SecondaryTextColor}"
        HorizontalOptions="FillAndExpand" />
```

> [!NOTE]
> In XAML, ein Typkonverter besteht keine Notwendigkeit zum Einschließen in des 'Visual'-Suffixes der [ `Visual` ](xref:Xamarin.Forms.VisualElement.Visual) -Eigenschaftswert. Allerdings kann auch der vollständigen Typnamen angegeben werden.

Der entsprechende C#-Code ist:

```csharp
Button button = new Button { Text = "CUSTOM BUTTON", ... };
button.Visual = new CustomVisual();
```

Zum Zeitpunkt der Renderer Auswahl der [ `Visual` ](xref:Xamarin.Forms.VisualElement.Visual) Eigenschaft der [ `Button` ](xref:Xamarin.Forms.Button) überprüft und in den Renderer Prozess enthalten ist. Wenn ein Renderer nicht gefunden wird, wird der Xamarin.Forms-Standard-Renderer verwendet werden.

Die folgenden Screenshots zeigen das gerenderte [ `Button` ](xref:Xamarin.Forms.Button), dessen Text mit einem Schatten wird angezeigt:

[![Screenshot der benutzerdefinierte Schaltfläche mit Volumeschattenkopie unter iOS und Android](material-visual-images/custom-button.png "Schaltfläche mit einem Schatten")](material-visual-images/custom-button-large.png#lightbox)

## <a name="register-a-name-for-the-ivisual-type"></a>Registrieren Sie einen Namen für die IVisual-Typ

Die [ `VisualAttribute` ](xref:Xamarin.Forms.VisualAttribute) können verwendet werden, um optional einen anderen Namen für die Registrierung der `IVisual` Typ. Dieser Ansatz kann zum Auflösen von Namenskonflikte zwischen verschiedenen Visual Bibliotheken oder in Situationen, in dem Sie nur auf eine Visualisierung zu verweisen möchten, von einem anderen Namen als den Typnamen verwendet werden.

Die [ `VisualAttribute` ](xref:Xamarin.Forms.VisualAttribute) sollte auf der Assemblyebene in entweder dem plattformübergreifenden Bibliothek oder in das plattformprojekt definiert werden:

```csharp
[assembly: Visual("MyVisual", typeof(CustomVisual))]
```

Die `IVisual` Typ kann dann über dessen registriertem Namen verwendet werden:

```xaml
<Button Visual="MyVisual"
        ... />
```

> [!NOTE]
> Bei der Nutzung einer Visualisierung über dessen registriertem Namen muss das Suffix any 'Visual' enthalten sein.

## <a name="related-links"></a>Verwandte Links

- [Material-Visual (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/VisualDemos/)
- [Xamarin.Forms-Material Visual](material-visual.md)
- [Benutzerdefinierte Renderer](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
