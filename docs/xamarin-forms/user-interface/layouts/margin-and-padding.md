---
title: Ränder und Abstände
description: Die Eigenschaften Margin und Padding steuern das Layoutverhalten, wenn ein Element in der Benutzeroberfläche gerendert wird. In diesem Artikel wird der Unterschied zwischen den beiden Eigenschaften und deren Festlegung veranschaulicht.
ms.prod: xamarin
ms.assetid: BEB096BB-51DF-410F-B0F1-D235287B0F4A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/27/2016
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 6ae4116be99f076a7afd5ed9c2823bc12f445e18
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84137987"
---
# <a name="margin-and-padding"></a>Ränder und Abstände

_Die Eigenschaften Margin und Padding steuern das Layoutverhalten, wenn ein Element in der Benutzeroberfläche gerendert wird. In diesem Artikel wird der Unterschied zwischen den beiden Eigenschaften und deren Festlegung veranschaulicht._

## <a name="overview"></a>Übersicht

Margin und Auffüllung sind Verwandte Layoutkonzepte:

- Die- [`Margin`](xref:Xamarin.Forms.View.Margin) Eigenschaft stellt den Abstand zwischen einem Element und seinen angrenzenden Elementen dar und wird verwendet, um die Renderingposition des Elements und die Renderingposition seiner Nachbarn zu steuern. `Margin`Werte können für [Layout](~/xamarin-forms/user-interface/controls/layouts.md) -und [Ansichts](~/xamarin-forms/user-interface/controls/views.md) Klassen angegeben werden.
- Die- [`Padding`](xref:Xamarin.Forms.Layout.Padding) Eigenschaft stellt den Abstand zwischen einem Element und seinen untergeordneten Elementen dar und wird verwendet, um das Steuerelement von seinem eigenen Inhalt zu trennen. `Padding`Werte können für [layoutklassen](~/xamarin-forms/user-interface/controls/layouts.md) angegeben werden.

Das folgende Diagramm veranschaulicht die beiden Konzepte:

[![](margin-and-padding-images/margins-and-padding-sml.png "Margins and Padding Concepts")](margin-and-padding-images/margins-and-padding.png#lightbox "Margins and Padding Concepts")

Beachten Sie, dass die [`Margin`](xref:Xamarin.Forms.View.Margin) Werte Additiv sind. Wenn also zwei angrenzende Elemente einen Rand von 20 Pixeln angeben, beträgt der Abstand zwischen den Elementen 40 Pixel. Außerdem sind Margin und Auffüllung Additiv, wenn beide angewendet werden, da der Abstand zwischen einem Element und einem beliebigen Inhalt der Rand plus Auffüllung ist.

## <a name="specifying-a-thickness"></a>Angeben einer Stärke

Die [`Margin`](xref:Xamarin.Forms.View.Margin) -Eigenschaft und die-Eigenschaft [`Padding`](xref:Xamarin.Forms.Layout.Padding) sind beide vom Typ [`Thickness`](xref:Xamarin.Forms.Thickness) . Es gibt drei Möglichkeiten, eine Struktur zu erstellen `Thickness` :

- Erstellen Sie eine Struktur, die [`Thickness`](xref:Xamarin.Forms.Thickness) durch einen einzelnen einheitlichen Wert definiert wird. Der einzelne Wert wird auf die linke, obere, Rechte und untere Seite des Elements angewendet.
- Erstellen Sie eine Struktur, die [`Thickness`](xref:Xamarin.Forms.Thickness) durch horizontale und vertikale Werte definiert wird. Der horizontale Wert wird symmetrisch auf die linke und die Rechte Seite des Elements angewendet, wobei der vertikale Wert symmetrisch auf die obere und untere Seite des Elements angewendet wird.
- Erstellen Sie eine Struktur, die [`Thickness`](xref:Xamarin.Forms.Thickness) durch vier unterschiedliche Werte definiert wird, die auf die linke, obere, Rechte und untere Seite des Elements angewendet werden.

Das folgende XAML-Codebeispiel zeigt alle drei Möglichkeiten:

```xaml
<StackLayout Padding="0,20,0,0">
  <Label Text="Xamarin.Forms" Margin="20" />
  <Label Text="Xamarin.iOS" Margin="10, 15" />
  <Label Text="Xamarin.Android" Margin="0, 20, 15, 5" />
</StackLayout>
```

Der äquivalente C#-Code ist im folgenden Codebeispiel zu sehen:

```csharp
var stackLayout = new StackLayout {
  Padding = new Thickness(0,20,0,0),
  Children = {
    new Label { Text = "Xamarin.Forms", Margin = new Thickness (20) },
    new Label { Text = "Xamarin.iOS", Margin = new Thickness (10, 25) },
    new Label { Text = "Xamarin.Android", Margin = new Thickness (0, 20, 15, 5) }
  }
};
```

> [!NOTE]
> `Thickness`Werte können negativ sein, was in der Regel den Inhalt schneidet oder überschreibt.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde der Unterschied zwischen den [`Margin`](xref:Xamarin.Forms.View.Margin) Eigenschaften und erläutert [`Padding`](xref:Xamarin.Forms.Layout.Padding) und erläutert, wie Sie festgelegt werden. Die Eigenschaften steuern das Layoutverhalten, wenn ein Element in der Benutzeroberfläche gerendert wird.

## <a name="related-links"></a>Verwandte Links

- [V](xref:Xamarin.Forms.View.Margin)
- [Abstand](xref:Xamarin.Forms.Layout.Padding)
- [Stärke](xref:Xamarin.Forms.Thickness)
