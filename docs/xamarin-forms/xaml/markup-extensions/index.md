---
title: XAML-Markuperweiterungen
description: Der Artikel erläutert das Xamarin.Forms-XAML-Markuperweiterungen verwenden, um die Leistungsfähigkeit und Flexibilität von XAML erweitern kann, da Attribute des Elements aus anderen Quellen als literale Zeichenfolgen festgelegt werden.
ms.prod: xamarin
ms.assetid: EB06C8B7-3FD5-47B7-A09C-A13063BD110F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
ms.openlocfilehash: cfdd639672f7fa624c7c8e30f17fbfc9dad403af
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/07/2018
ms.locfileid: "53060160"
---
# <a name="xaml-markup-extensions"></a>XAML-Markuperweiterungen

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://developer.xamarin.com/samples/xamarin-forms/XAML/MarkupExtensions/)

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
