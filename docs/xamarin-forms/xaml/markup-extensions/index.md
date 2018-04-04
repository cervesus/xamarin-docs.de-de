---
title: Verwendung von XAML-Markuperweiterungen
description: Erweitern Sie den Bereich von Quellen, die über das XAML-Attributen festgelegt ist
ms.prod: xamarin
ms.assetid: EB06C8B7-3FD5-47B7-A09C-A13063BD110F
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 01/05/2018
ms.openlocfilehash: b81bc4b31edd1d8b8f5f43f97885c38e889dd32c
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="xaml-markup-extensions"></a>Verwendung von XAML-Markuperweiterungen

Verwendung von XAML-Markuperweiterungen können die Leistungsfähigkeit und Flexibilität von XAML erweitern kann, da Attribute des Elements aus anderen Quellen als literaltextzeichenfolgen festgelegt werden.

Z. B. normalerweise legen Sie die `Color` Eigenschaft `BoxView` wie folgt:

```xaml
<BoxView Color="Blue" />
```

Oder Sie können einen hexadezimalen RGB-Farbwert festlegen:

```xaml
<BoxView Color="#FF0080" />
```

In beiden Fällen die Textzeichenfolge festlegen, um die `Color` Attribut konvertiert eine `Color` vom Wert der [ `ColorTypeConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ColorTypeConverter/) Klasse.

Möglicherweise bevorzugen Sie stattdessen zum Festlegen der `Color` Attribut aus einem Wert in einem Ressourcenwörterbuch, den Wert einer statischen Eigenschaft einer Klasse, die Sie erstellt haben, oder über eine Eigenschaft vom Typ `Color` eines anderen Elements auf der Seite oder konstruierten aus Trennen Sie Werte für Farbton, Sättigung und Helligkeit.

Alle diese Optionen sind mit der Verwendung von XAML-Markuperweiterungen möglich. Aber lassen Sie nicht den Ausdruck "Markuperweiterungen" erschrecken Sie: Verwendung von XAML-Markuperweiterungen sind *nicht* Erweiterungen in XML. Selbst bei Verwendung von XAML-Markuperweiterungen ist XAML immer gültige XML-Daten. 

Eine Markuperweiterung ist genau genommen nur eine andere Möglichkeit, ein Attribut eines Elements express. Verwendung von XAML-Markuperweiterungen werden in der Regel durch eine attributeinstellung, die in der geschweiften Klammern eingeschlossen ist zu identifizieren:

```xaml
<BoxView Color="{StaticResource themeColor}" />
```

Ist keiner attributeinstellung in der geschweiften Klammern *immer* eine XAML-Markuperweiterung. Wie Sie sehen, können jedoch XAML-Markuperweiterungen auch ohne die Verwendung von geschweiften Klammern verwiesen.

Dieser Artikel ist in zwei Teile unterteilt:

## <a name="consuming-xaml-markup-extensionsconsumingmd"></a>[Verwenden von XAML-Markuperweiterungen](consuming.md)  

Verwenden Sie die Verwendung von XAML-Markuperweiterungen in Xamarin.Forms definiert.

## <a name="creating-xaml-markup-extensionscreatingmd"></a>[Erstellen von XAML-Markuperweiterungen](creating.md) 

Schreiben Sie Ihre eigenen benutzerdefinierten XAML-Markuperweiterungen.



## <a name="related-links"></a>Verwandte Links

- [Markuperweiterungen (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/XAML/MarkupExtensions/)
- [Verwendung von XAML-Markup Extensions Kapitel Xamarin.Forms Buchs](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter10.md)
- [Ressourcenverzeichnisse](~/xamarin-forms/xaml/resource-dictionaries.md)
- [Dynamische Stile](~/xamarin-forms/user-interface/styles/dynamic.md)
- [Datenbindung](~/xamarin-forms/app-fundamentals/data-binding/index.md)
