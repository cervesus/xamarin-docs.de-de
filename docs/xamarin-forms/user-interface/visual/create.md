---
title: Erstellen eines Xamarin.Forms visuellen Renderers
description: Erstellen Xamarin.Forms Sie Visualisierungen, die selektiv auf visualelement-Objekte angewendet werden, ohne dass untergeordnete Sichten vorhanden sein müssen Xamarin.Forms .
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 23edbb007e912d13858686d1c5ec574c9e3349c7
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2020
ms.locfileid: "84127139"
---
# <a name="create-a-xamarinforms-visual-renderer"></a>Erstellen eines Xamarin.Forms visuellen Renderers

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-visualdemos)

Xamarin.FormsVisual ermöglicht, dass Renderer erstellt und selektiv auf Objekte angewendet werden können [`VisualElement`](xref:Xamarin.Forms.VisualElement) , ohne dass untergeordnete Sichten vorhanden sein müssen Xamarin.Forms . Ein Renderer, der einen- `IVisual` Typ als Teil seines angibt `ExportRendererAttribute` , wird verwendet, um die ausgewählten Ansichten anstelle des standardrenderers zu renderzieren. Bei der Auswahl Zeit des Renderers `Visual` wird die-Eigenschaft der Sicht überprüft und in den rendererauswahlvorgang eingeschlossen.

> [!IMPORTANT]
> Die- [`Visual`](xref:Xamarin.Forms.VisualElement.Visual) Eigenschaft kann derzeit nicht geändert werden, nachdem die Sicht gerendert wurde. Dies wird jedoch in einer zukünftigen Version geändert.

Der Prozess zum Erstellen und Verwenden eines Xamarin.Forms visuellen Renderers lautet wie folgt:

1. Erstellen Sie Platt Form Renderer für die erforderliche Ansicht. Weitere Informationen finden Sie unter [Erstellen von Renderer](#create-platform-renderers).
1. Erstellen Sie einen Typ, der von abgeleitet wird `IVisual` . Weitere Informationen finden Sie unter [Erstellen eines ivisual-Typs](#create-an-ivisual-type).
1. Registrieren Sie den `IVisual` Typ als Teil von `ExportRendererAttribute` , der die Renderer ergänzt. Weitere Informationen finden Sie unter [Registrieren des ivisual-Typs](#register-the-ivisual-type).
1. Verwenden Sie den visuellen Renderer, indem [`Visual`](xref:Xamarin.Forms.VisualElement.Visual) Sie die-Eigenschaft für die Ansicht auf den `IVisual` Namen festlegen. Weitere Informationen finden Sie unter verwenden [des visuellen Renderers](#consume-the-visual-renderer).
1. optionale Registrieren Sie einen Namen für den `IVisual` Typ. Weitere Informationen finden Sie unter [Registrieren eines Namens für den ivisual-Typ](#register-a-name-for-the-ivisual-type).

## <a name="create-platform-renderers"></a>Erstellen von Plattformrenderern

Weitere Informationen zum Erstellen einer Rendererklasse finden Sie unter [benutzerdefinierte Renderer](~/xamarin-forms/app-fundamentals/custom-renderer/index.md). Beachten Sie jedoch, dass ein Xamarin.Forms visueller Renderer auf eine Ansicht angewendet wird, ohne dass die Ansicht unterteilt werden muss.

In den hier beschriebenen rendererklassen wird ein benutzerdefiniertes implementiert [`Button`](xref:Xamarin.Forms.Button) , das seinen Text mit einem Schatten anzeigt.

### <a name="ios"></a>iOS

Das folgende Codebeispiel zeigt den Schaltflächen-Renderer für ios:

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

Das folgende Codebeispiel zeigt den Schaltflächen-Renderer für Android:

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

## <a name="create-an-ivisual-type"></a>Erstellen eines ivisual-Typs

Erstellen Sie in ihrer plattformübergreifenden Bibliothek einen Typ, der von abgeleitet ist `IVisual` :

```csharp
public class CustomVisual : IVisual
{
}
```

Der `CustomVisual` Typ kann dann für die rendererklassen registriert werden, sodass [`Button`](xref:Xamarin.Forms.Button) Objekte die Verwendung der Renderer abonnieren können.

## <a name="register-the-ivisual-type"></a>Registrieren des ivisual-Typs

Fügen Sie in den Platt Form Projekten das `ExportRendererAttribute` auf Assemblyebene hinzu:

```csharp
[assembly: ExportRenderer(typeof(Xamarin.Forms.Button), typeof(CustomButtonRenderer), new[] { typeof(CustomVisual) })]
namespace VisualDemos.iOS
{
    public class CustomButtonRenderer : ButtonRenderer
    {
        protected override void OnElementChanged(ElementChangedEventArgs<Button> e)
        {
            // ...
        }
    }
}
```

In diesem Beispiel für das IOS-Platt Form Projekt `ExportRendererAttribute` gibt das an, dass die- `CustomButtonRenderer` Klasse zum Rendering von [`Button`](xref:Xamarin.Forms.Button) nutzenden Objekten verwendet wird, wobei der `IVisual` Typ als drittes Argument registriert ist. Ein Renderer, der einen- `IVisual` Typ als Teil seines angibt `ExportRendererAttribute` , wird verwendet, um die ausgewählten Ansichten anstelle des standardrenderers zu renderzieren.

## <a name="consume-the-visual-renderer"></a>Verwenden des visuellen Renderers

Ein- [`Button`](xref:Xamarin.Forms.Button) Objekt kann die Verwendung der rendererklassen verwenden, indem seine-Eigenschaft auf festgelegt wird [`Visual`](xref:Xamarin.Forms.VisualElement.Visual) `Custom` :

```xaml
<Button Visual="Custom"
        Text="CUSTOM BUTTON"
        BackgroundColor="{StaticResource PrimaryColor}"
        TextColor="{StaticResource SecondaryTextColor}"
        HorizontalOptions="FillAndExpand" />
```

> [!NOTE]
> In XAML entfällt ein Typkonverter darauf, dass das Suffix "Visual" in den [`Visual`](xref:Xamarin.Forms.VisualElement.Visual) Eigenschafts Wert eingeschlossen werden muss. Allerdings kann auch der vollständige Typname angegeben werden.

Der entsprechende C#-Code lautet:

```csharp
Button button = new Button { Text = "CUSTOM BUTTON", ... };
button.Visual = new CustomVisual();
```

Bei der Auswahl Zeit des Renderers [`Visual`](xref:Xamarin.Forms.VisualElement.Visual) wird die [`Button`](xref:Xamarin.Forms.Button) -Eigenschaft des überprüft und in den rendererauswahlvorgang eingeschlossen. Wenn ein Renderer nicht gefunden wird, Xamarin.Forms wird der Standardrenderer verwendet.

Die folgenden Screenshots zeigen das gerenderte [`Button`](xref:Xamarin.Forms.Button) , das seinen Text mit einem Schatten anzeigt:

[![Screenshot der benutzerdefinierten Schaltfläche mit Schatten Text unter IOS und Android](material-visual-images/custom-button.png "Schaltfläche mit Schatten Text")](material-visual-images/custom-button-large.png#lightbox)

## <a name="register-a-name-for-the-ivisual-type"></a>Registrieren eines Namens für den ivisual-Typ

[`VisualAttribute`](xref:Xamarin.Forms.VisualAttribute)Kann verwendet werden, um optional einen anderen Namen für den Typ zu registrieren `IVisual` . Dieser Ansatz kann verwendet werden, um Namenskonflikte zwischen verschiedenen visuellen Bibliotheken aufzulösen, oder in Situationen, in denen Sie nur auf eine Visualisierung mit einem anderen Namen als dem Typnamen verweisen möchten.

[`VisualAttribute`](xref:Xamarin.Forms.VisualAttribute)Muss auf Assemblyebene entweder in der plattformübergreifenden Bibliothek oder im Platt Form Projekt definiert werden:

```csharp
[assembly: Visual("MyVisual", typeof(CustomVisual))]
```

Der `IVisual` Typ kann dann über den registrierten Namen genutzt werden:

```xaml
<Button Visual="MyVisual"
        ... />
```

> [!NOTE]
> Wenn eine Visualisierung über den registrierten Namen genutzt wird, muss jedes beliebige "Visual"-Suffix eingeschlossen werden.

## <a name="related-links"></a>Verwandte Links

- [Visuelles Material (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-visualdemos)
- [Xamarin.FormsMaterial Visualisierung](material-visual.md)
- [Benutzerdefinierte Renderer](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
