---
title: Ränder und Abstände
description: Der Rand und Abstand Eigenschaften steuern Layout-Verhalten, wenn ein Element in der Benutzeroberfläche gerendert wird. Dieser Artikel veranschaulicht den Unterschied zwischen den zwei Eigenschaften, und wie diese festgelegt.
ms.prod: xamarin
ms.assetid: BEB096BB-51DF-410F-B0F1-D235287B0F4A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/27/2016
ms.openlocfilehash: 595e673c59d23a45cbaf923a0d58faff2000c296
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996615"
---
# <a name="margin-and-padding"></a>Ränder und Abstände

_Der Rand und Abstand Eigenschaften steuern Layout-Verhalten, wenn ein Element in der Benutzeroberfläche gerendert wird. Dieser Artikel veranschaulicht den Unterschied zwischen den zwei Eigenschaften, und wie diese festgelegt._

## <a name="overview"></a>Übersicht

Ränder und Abstände werden die dazugehörigen Konzepte sind:

- Die [ `Margin` ](xref:Xamarin.Forms.View.Margin) Eigenschaft repräsentiert die Entfernung zwischen einem Element und die angrenzenden Elemente, und wird verwendet, um die Position des Elements Rendering und die Renderingposition seiner Nachbarn zu steuern. `Margin` Werte können angegeben werden, auf [Layout](~/xamarin-forms/user-interface/controls/layouts.md) und [Ansicht](~/xamarin-forms/user-interface/controls/views.md) Klassen.
- Die [ `Padding` ](xref:Xamarin.Forms.Layout.Padding) Eigenschaft, der den Abstand zwischen einem Element und seine untergeordneten Elemente darstellt, und wird verwendet, um das Steuerelement von der eigene Inhalt zu trennen. `Padding` Werte können angegeben werden, auf [Layout](~/xamarin-forms/user-interface/controls/layouts.md) Klassen.

Das folgende Diagramm veranschaulicht die beiden Konzepte:

[![](margin-and-padding-images/margins-and-padding-sml.png "Seitenränder und Textabstand Konzepte")](margin-and-padding-images/margins-and-padding.png#lightbox "Seitenränder und Textabstand-Konzepte")

Beachten Sie, dass [ `Margin` ](xref:Xamarin.Forms.View.Margin) Werte sind additiv. Wenn zwei benachbarte Elemente einen Rand von 20 Pixel angeben, wird der Abstand zwischen den Elementen aus diesem Grund 40 Pixel sein. Ränder und Abstände darüber hinaus sind additiv, wenn beide angewendet werden, da die Entfernung zwischen einem Element und alle Inhalte stehen den Rand und Abstand,.

## <a name="specifying-a-thickness"></a>Eine Breite angeben

Die [ `Margin` ](xref:Xamarin.Forms.View.Margin) und [ `Padding` ](xref:Xamarin.Forms.Layout.Padding) Eigenschaften sind beide vom Typ [ `Thickness` ](xref:Xamarin.Forms.Thickness). Es gibt drei Möglichkeiten beim Erstellen einer `Thickness` Struktur:

- Erstellen Sie eine [ `Thickness` ](xref:Xamarin.Forms.Thickness) Struktur, die von einem einzelnen einheitlichen Wert definiert. Der single-Wert wird auf die Links, oben, rechten und Unterseite des Elements angewendet.
- Erstellen Sie eine [ `Thickness` ](xref:Xamarin.Forms.Thickness) Struktur, die durch die horizontalen und vertikalen Werte definiert. Der horizontale Wert gilt symmetrisch auf der linken und rechten Seite des Elements mit den vertikalen Wert symmetrisch auf die untere und obere Seite des Elements angewendet wird.
- Erstellen Sie eine [ `Thickness` ](xref:Xamarin.Forms.Thickness) Struktur definiert anhand von vier unterschiedlichen Werte, die auf die Links, oben, rechten und Unterseite des Elements angewendet werden.

Im folgende XAML-Codebeispiel zeigt alle drei Möglichkeiten:

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
> `Thickness` Werte können negativ sein, die in der Regel schneidet oder overdraws des Inhalts.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel veranschaulicht den Unterschied zwischen der [ `Margin` ](xref:Xamarin.Forms.View.Margin) und [ `Padding` ](xref:Xamarin.Forms.Layout.Padding) Eigenschaften und deren Festlegung. Die Eigenschaften steuern die Layout-Verhalten, wenn ein Element in der Benutzeroberfläche gerendert wird.


## <a name="related-links"></a>Verwandte Links

- [Rand](xref:Xamarin.Forms.View.Margin)
- [Auffüllung](xref:Xamarin.Forms.Layout.Padding)
- [Stärke](xref:Xamarin.Forms.Thickness)
