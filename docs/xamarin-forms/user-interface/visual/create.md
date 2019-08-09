---
title: Erstellen eines visuellen xamarin. Forms-Renderers
description: Erstellen Sie xamarin. Forms-Visualisierungen, die selektiv auf visualelement-Objekte angewendet werden, ohne dass die Unterklasse xamarin. Forms-Ansichten unterteilt werden muss.
ms.prod: xamarin
ms.assetid: 80BF9C72-AC28-4AAF-9DDD-B60CBDD1CD59
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/12/2019
ms.openlocfilehash: 8173ebcc174df6e34a53f226a43083bd28941031
ms.sourcegitcommit: 2e5a6b8bcd1a073b54604f51538fd108e1c2a8e5
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/09/2019
ms.locfileid: "68869380"
---
# <a name="create-a-xamarinforms-visual-renderer"></a>Erstellen eines visuellen xamarin. Forms-Renderers

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-visualdemos)

Xamarin. Forms Visual ermöglicht, dass Renderer erstellt und selektiv auf [`VisualElement`](xref:Xamarin.Forms.VisualElement) -Objekte angewendet werden können, ohne dass eine Unterklasse von xamarin. Forms-Ansichten erstellt werden muss. Ein Renderer, der einen `IVisual` -Typ als Teil seines `ExportRendererAttribute`angibt, wird verwendet, um die ausgewählten Ansichten anstelle des standardrenderers zu renderzieren. Zum Zeitpunkt der Renderer Auswahl der `Visual` -Eigenschaft der Ansicht werden untersucht und in den Renderer Prozess enthalten.

> [!IMPORTANT]
> Die [`Visual`](xref:Xamarin.Forms.VisualElement.Visual) -Eigenschaft kann derzeit nicht geändert werden, nachdem die Sicht gerendert wurde. Dies wird jedoch in einer zukünftigen Version geändert.

Der Prozess zum Erstellen und Verwenden eines visuellen xamarin. Forms-Renderers ist:

1. Erstellen Sie Platt Form Renderer für die erforderliche Ansicht. Weitere Informationen finden Sie unter [Erstellen von Renderer](#create-platform-renderers).
1. Erstellen Sie einen Typ, der `IVisual`von abgeleitet wird. Weitere Informationen finden Sie unter [Erstellen eines ivisual-Typs](#create-an-ivisual-type).
1. Registrieren Sie `IVisual` den Typ als Teil `ExportRendererAttribute` von, der die Renderer ergänzt. Weitere Informationen finden Sie unter [Registrieren des ivisual-Typs](#register-the-ivisual-type).
1. Verwenden Sie den visuellen Renderer, indem [`Visual`](xref:Xamarin.Forms.VisualElement.Visual) Sie die-Eigenschaft für die `IVisual` Ansicht auf den Namen festlegen. Weitere Informationen finden Sie unter verwenden [des visuellen](#consume-the-visual-renderer)Renderers.
1. optionale Registrieren Sie einen Namen für `IVisual` den Typ. Weitere Informationen finden Sie unter [Registrieren eines Namens für den ivisual-Typ](#register-a-name-for-the-ivisual-type).

## <a name="create-platform-renderers"></a>Erstellen von Platt Form Renderer

Weitere Informationen zum Erstellen einer Rendererklasse finden Sie unter [benutzerdefinierte Renderer](~/xamarin-forms/app-fundamentals/custom-renderer/index.md). Beachten Sie jedoch, dass ein xamarin. Forms-visueller Renderer auf eine Ansicht angewendet wird, ohne dass die Ansicht unterteilt werden muss.

In den hier beschriebenen rendererklassen wird [`Button`](xref:Xamarin.Forms.Button) ein benutzerdefiniertes implementiert, das seinen Text mit einem Schatten anzeigt.

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

Erstellen Sie in ihrer plattformübergreifenden Bibliothek einen Typ, der von `IVisual`abgeleitet ist:

```csharp
public class CustomVisual : IVisual
{
}
```

Der `CustomVisual` Typ kann dann für die rendererklassen registriert werden [`Button`](xref:Xamarin.Forms.Button) , sodass Objekte die Verwendung der Renderer abonnieren können.

## <a name="register-the-ivisual-type"></a>Registrieren des ivisual-Typs

Ergänzen Sie in den Platt Form Projekten die Renderer-Namespaces `ExportRendererAttribute`mit dem:

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

In diesem Beispiel für das IOS-Platt Form Projekt `ExportRendererAttribute` gibt das an `CustomButtonRenderer` , dass die-Klasse zum Rendering [`Button`](xref:Xamarin.Forms.Button) von nutzenden Objekten `IVisual` verwendet wird, wobei der Typ als drittes Argument registriert ist. Ein Renderer, der einen `IVisual` -Typ als Teil seines `ExportRendererAttribute`angibt, wird verwendet, um die ausgewählten Ansichten anstelle des standardrenderers zu renderzieren.

## <a name="consume-the-visual-renderer"></a>Verwenden des visuellen Renderers

Ein [`Button`](xref:Xamarin.Forms.Button) -Objekt kann die Verwendung der rendererklassen verwenden, [`Visual`](xref:Xamarin.Forms.VisualElement.Visual) indem seine `Custom`-Eigenschaft auf festgelegt wird:

```xaml
<Button Visual="Custom"
        Text="CUSTOM BUTTON"
        BackgroundColor="{StaticResource PrimaryColor}"
        TextColor="{StaticResource SecondaryTextColor}"
        HorizontalOptions="FillAndExpand" />
```

> [!NOTE]
> In XAML entfällt ein Typkonverter darauf, dass das Suffix "Visual" in den [`Visual`](xref:Xamarin.Forms.VisualElement.Visual) Eigenschafts Wert eingeschlossen werden muss. Allerdings kann auch der vollständige Typname angegeben werden.

Der entsprechende C#-Code ist:

```csharp
Button button = new Button { Text = "CUSTOM BUTTON", ... };
button.Visual = new CustomVisual();
```

Bei der Auswahl Zeit des Renderers [`Visual`](xref:Xamarin.Forms.VisualElement.Visual) [`Button`](xref:Xamarin.Forms.Button) wird die-Eigenschaft des überprüft und in den rendererauswahlvorgang eingeschlossen. Wenn ein Renderer nicht gefunden wird, wird der xamarin. Forms-Standardrenderer verwendet.

Die folgenden Screenshots zeigen das gerenderte [`Button`](xref:Xamarin.Forms.Button), das seinen Text mit einem Schatten anzeigt:

[![Screenshot der benutzerdefinierten Schaltfläche mit Schatten Text unter IOS und Android](material-visual-images/custom-button.png "Schaltfläche mit Schatten Text")](material-visual-images/custom-button-large.png#lightbox)

## <a name="register-a-name-for-the-ivisual-type"></a>Registrieren eines Namens für den ivisual-Typ

Kann verwendet werden, um optional einen anderen Namen für den `IVisual` Typ zu registrieren. [`VisualAttribute`](xref:Xamarin.Forms.VisualAttribute) Dieser Ansatz kann verwendet werden, um Namenskonflikte zwischen verschiedenen visuellen Bibliotheken aufzulösen, oder in Situationen, in denen Sie nur auf eine Visualisierung mit einem anderen Namen als dem Typnamen verweisen möchten.

[`VisualAttribute`](xref:Xamarin.Forms.VisualAttribute) Muss auf Assemblyebene entweder in der plattformübergreifenden Bibliothek oder im Platt Form Projekt definiert werden:

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
- [Visuelles xamarin. Forms-Material](material-visual.md)
- [Benutzerdefinierte Renderer](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
