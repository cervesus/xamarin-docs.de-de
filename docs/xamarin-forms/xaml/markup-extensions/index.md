---
title: ''
description: Der Artikel erläutert, wie Xamarin.Forms XAML-Markup Erweiterungen verwendet werden, um die Leistungsfähigkeit und Flexibilität von XAML zu erweitern, indem Element Attribute aus anderen Quellen als Literalzeichenfolgen festgelegt werden können.
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 568cffc335f28b1a47f3278ad061d851ebef84b6
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2020
ms.locfileid: "84130389"
---
# <a name="xaml-markup-extensions"></a>XAML-Markuperweiterungen

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-markupextensions)

XAML-Markup Erweiterungen helfen, die Leistungsfähigkeit und Flexibilität von XAML zu erweitern, indem es ermöglicht wird, Element Attribute aus anderen Quellen als Literalzeichenfolgen festzulegen.

Normalerweise legen Sie z. b. die- `Color` Eigenschaft von wie folgt fest `BoxView` :

```xaml
<BoxView Color="Blue" />
```

Oder Sie können ihn auf einen hexadezimalen RGB-Farbwert festlegen:

```xaml
<BoxView Color="#FF0080" />
```

In beiden Fällen wird die Text Zeichenfolge, die auf das-Attribut festgelegt `Color` ist, `Color` von der-Klasse in einen Wert konvertiert [`ColorTypeConverter`](xref:Xamarin.Forms.ColorTypeConverter) .

Möglicherweise bevorzugen Sie stattdessen das Festlegen des- `Color` Attributs aus einem in einem Ressourcen Wörterbuch gespeicherten Wert oder aus dem Wert einer statischen Eigenschaft einer Klasse, die Sie erstellt haben, oder aus einer Eigenschaft des Typs `Color` eines anderen Elements auf der Seite oder aus separaten Farbton-, Sättigungs-und Helligkeits Werten.

Alle diese Optionen können mithilfe von XAML-Markup Erweiterungen verwendet werden. Aber lassen Sie den Ausdruck "Markup Erweiterungen" nicht erschrecken: XAML-Markup Erweiterungen sind *keine* XML-Erweiterungen. Auch mit XAML-Markup Erweiterungen ist XAML immer gültiger XML-Code.

Eine Markup Erweiterung ist eigentlich nur eine andere Möglichkeit, ein Attribut eines Elements auszudrücken. XAML-Markup Erweiterungen können normalerweise durch eine Attribut Einstellung identifiziert werden, die in geschweiften Klammern eingeschlossen ist:

```xaml
<BoxView Color="{StaticResource themeColor}" />
```

Jede Attribut Einstellung in geschweiften Klammern ist *immer* eine XAML-Markup Erweiterung. Wie Sie sehen werden, kann jedoch auch ohne die Verwendung von geschweiften Klammern auf XAML-Markup Erweiterungen verwiesen werden.

Dieser Artikel ist in zwei Teile unterteilt:

## <a name="consuming-xaml-markup-extensions"></a>[Verwenden von XAML-Markuperweiterungen](consuming.md)  

Verwenden Sie die XAML-Markup Erweiterungen, die in definiert sind Xamarin.Forms .

## <a name="creating-xaml-markup-extensions"></a>[Erstellen von XAML-Markuperweiterungen](creating.md)

Schreiben Sie Ihre eigenen benutzerdefinierten XAML-Markup Erweiterungen.

## <a name="related-links"></a>Verwandte Links

- [Markup Erweiterungen (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-markupextensions)
- [Kapitel zu XAML-Markup Erweiterungen aus Xamarin.Forms Buch](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter10.md)
- [Ressourcenverzeichnisse](~/xamarin-forms/xaml/resource-dictionaries.md)
- [Dynamische Stile](~/xamarin-forms/user-interface/styles/dynamic.md)
- [Datenbindung](~/xamarin-forms/app-fundamentals/data-binding/index.md)
