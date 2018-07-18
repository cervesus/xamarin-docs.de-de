---
title: XAML-Markuperweiterungen
description: Der Artikel erläutert das Xamarin.Forms-XAML-Markuperweiterungen verwenden, um die Leistungsfähigkeit und Flexibilität von XAML erweitern kann, da Attribute des Elements aus anderen Quellen als literale Zeichenfolgen festgelegt werden.
ms.prod: xamarin
ms.assetid: EB06C8B7-3FD5-47B7-A09C-A13063BD110F
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 01/05/2018
ms.openlocfilehash: d507ff3c74de6bb4ea36c1a7b7dc2cd5dd60823b
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996739"
---
# <a name="xaml-markup-extensions"></a>XAML-Markuperweiterungen

XAML-Markuperweiterungen können die Leistungsfähigkeit und Flexibilität von XAML erweitern kann, da Attribute des Elements aus anderen Quellen als literale Zeichenfolgen festgelegt werden.

Zum Beispiel normalerweise legen Sie die `Color` Eigenschaft `BoxView` wie folgt aus:

```xaml
<BoxView Color="Blue" />
```

Alternativ können Sie sie auf eine hexadezimale RGB-Farbwert festlegen:

```xaml
<BoxView Color="#FF0080" />
```

In beiden Fällen die Textzeichenfolge festlegen, um die `Color` Attribut konvertiert wird eine `Color` Wert durch die [ `ColorTypeConverter` ](xref:Xamarin.Forms.ColorTypeConverter) Klasse.

Das Verlagen stattdessen legen Sie die `Color` Attribut über einen Wert in einem Ressourcenverzeichnis gespeichert oder aus dem Wert einer statischen Eigenschaft einer Klasse, die Sie erstellt haben oder aus einer Eigenschaft vom Typ `Color` eines anderen Elements auf der Seite oder erstellte aus Trennen Sie Werte für Farbton, Sättigung und Helligkeit.

Alle diese Optionen sind möglich, die Verwendung von Markuperweiterungen für XAML. Jedoch nicht den Ausdruck "Markuperweiterungen" erschrecken Sie: XAML-Markuperweiterungen sind *nicht* Erweiterungen in XML. Selbst bei XAML-Markuperweiterungen ist XAML immer gültige XML-Daten.

Eine Markuperweiterung ist eigentlich nur eine andere Möglichkeit, ein Attribut eines Elements auszudrücken. XAML-Markuperweiterungen sind in der Regel durch eine attributeinstellung, die in geschweifte Klammern eingeschlossen ist erkennbar:

```xaml
<BoxView Color="{StaticResource themeColor}" />
```

Alle attributeinstellung in geschweiften Klammern ist *immer* eine XAML-Markuperweiterung. Aber wie Sie sehen werden, können XAML-Markuperweiterungen auch ohne die Verwendung von geschweiften Klammern verwiesen werden.

In diesem Artikel ist in zwei Teile unterteilt:

## <a name="consuming-xaml-markup-extensionsconsumingmd"></a>[Verwenden von XAML-Markuperweiterungen](consuming.md)  

Verwenden Sie die XAML-Markuperweiterungen in Xamarin.Forms definiert.

## <a name="creating-xaml-markup-extensionscreatingmd"></a>[Erstellen von XAML-Markuperweiterungen](creating.md)

Schreiben Sie Ihre eigenen benutzerdefinierten XAML-Markuperweiterungen.



## <a name="related-links"></a>Verwandte Links

- [Markuperweiterungen (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/XAML/MarkupExtensions/)
- [XAML-Markup Extensions Kapitel von Xamarin.Forms-Buch](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter10.md)
- [Ressourcenverzeichnisse](~/xamarin-forms/xaml/resource-dictionaries.md)
- [Dynamische Stile](~/xamarin-forms/user-interface/styles/dynamic.md)
- [Datenbindung](~/xamarin-forms/app-fundamentals/data-binding/index.md)
