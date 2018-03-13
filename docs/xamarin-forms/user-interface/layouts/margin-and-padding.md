---
title: Rand und Abstand
description: "Der Rand und Abstand Eigenschaften Layoutverhalten steuern, wenn ein Element in der Benutzeroberfläche gerendert wird. Dieser Artikel zeigt den Unterschied zwischen den zwei Eigenschaften und wie sie festgelegt."
ms.topic: article
ms.prod: xamarin
ms.assetid: BEB096BB-51DF-410F-B0F1-D235287B0F4A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/27/2016
ms.openlocfilehash: 461430ba27b5d6008338019e5feaebed7b09d4cb
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="margin-and-padding"></a>Rand und Abstand

_Der Rand und Abstand Eigenschaften Layoutverhalten steuern, wenn ein Element in der Benutzeroberfläche gerendert wird. Dieser Artikel zeigt den Unterschied zwischen den zwei Eigenschaften und wie sie festgelegt._

## <a name="overview"></a>Übersicht

Rand und Abstand sind verwandte Layout-Konzepte:

- Die [ `Margin` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.Margin/) Eigenschaft repräsentiert die Entfernung zwischen einem Element und dessen benachbarte Elemente, und wird verwendet, um die Position des Elements Rendering und die Renderposition seiner Nachbarn zu steuern. `Margin` Werte können angegeben werden, auf [Layout](~/xamarin-forms/user-interface/controls/layouts.md) und [Ansicht](~/xamarin-forms/user-interface/controls/views.md) Klassen.
- Die [ `Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/) Eigenschaft repräsentiert die Entfernung zwischen einem Element und seinen untergeordneten Elementen und wird verwendet, um das Steuerelement über einen eigenen Inhalt zu trennen. `Padding` Werte können angegeben werden, auf [Layout](~/xamarin-forms/user-interface/controls/layouts.md) Klassen.

Das folgende Diagramm veranschaulicht die beiden Konzepte:

[![](margin-and-padding-images/margins-and-padding-sml.png "Seitenränder und Textabstand Konzepte")](margin-and-padding-images/margins-and-padding.png#lightbox "Seitenränder und Textabstand-Konzepte")

Beachten Sie, dass [ `Margin` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.Margin/) Werte sind additiv. Wenn zwei benachbarte Elemente einen Rand von 20 Pixel angeben, wird der Abstand zwischen den Elementen daher 40 Pixel sein. Darüber hinaus Rand und Abstand sind additiv, wenn beide angewendet werden, dass der Abstand zwischen einem Element und alle Inhalte, die der Rand und Abstand sein wird,.

## <a name="specifying-a-thickness"></a>Angeben einer Stärke

Die [ `Margin` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.Margin/) und [ `Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/) Eigenschaften befinden sich beide vom Typ [ `Thickness` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Thickness/). Es gibt drei Möglichkeiten für die Erstellung einer `Thickness` Struktur:

- Erstellen einer [ `Thickness` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Thickness/) Struktur, die von einem einzelnen, einheitlichen Wert definiert. Der Wert wird auf die Links, oben, rechten und Unterseite des Elements angewendet.
- Erstellen einer [ `Thickness` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Thickness/) Struktur von horizontalen und vertikalen Werte definiert. Der horizontale Wert ist symmetrisch auf der linken und rechten Seite des Elements an, mit dem vertikalen Wert angewendeten symmetrisch an den oberen und unteren Seiten des Elements angewendet.
- Erstellen einer [ `Thickness` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Thickness/) definiert, die durch vier unterschiedliche Werte, die auf die Links, oben, rechten und Unterseite des Elements angewendet werden.

Im folgende Codebeispiel für die Verwendung von XAML-zeigt alle drei Möglichkeiten:

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

Dieser Artikel veranschaulicht den Unterschied zwischen der [ `Margin` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.Margin/) und [ `Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/) Eigenschaften und wie sie festgelegt. Die Eigenschaften steuern Layoutverhalten auf, wenn ein Element in der Benutzeroberfläche gerendert wird.


## <a name="related-links"></a>Verwandte Links

- [Rand](https://developer.xamarin.com/api/property/Xamarin.Forms.View.Margin/)
- [Padding](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/)
- [Stärke](https://developer.xamarin.com/api/type/Xamarin.Forms.Thickness/)
