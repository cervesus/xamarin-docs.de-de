---
title: Zusammenfassung der Kapitel 12. Stile
description: 'Beim Erstellen mobiler Apps mit Xamarin.Forms: Zusammenfassung der Kapitel 12. Stile'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 3EAE6BDC-8EFB-464B-A87B-1C35B8387BB3
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: d69c6c18a28c89742b186474656ee666b1e6a0ee
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/08/2018
ms.locfileid: "35241675"
---
# <a name="summary-of-chapter-12-styles"></a>Zusammenfassung der Kapitel 12. Stile

Formatvorlagen zulassen in Xamarin.Forms mit, mehreren Ansichten auf eine Auflistung von Einstellungen der Eigenschaften gemeinsam nutzen. Dies reduziert die Markup und konsistente visuelle Designs verwalten kann.

Stile werden fast immer definiert und im Markup genutzt. Ein Objekt des Typs [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) in einem Ressourcenwörterbuch instanziiert ist, und legen Sie dann auf die [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) Eigenschaft einer visuelle Element mit einem `StaticResource` oder `DyanamicResource` Markup Erweiterung.

## <a name="the-basic-style"></a>Das grundlegende Format

Ein `Style` erfordert, dass seine [ `TargetType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.TargetType/) in den Typ des visuellen Objekts dies für gilt festgelegt werden. Wenn eine `Style` instanziiert wird in einem Ressourcenwörterbuch (wie üblich ist) darüber hinaus müssen ein `x:Key` Attribut.

Die `Style` verfügt über einen Content-Eigenschaft des Typs [ `Setters` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.Setters/), dies ist eine Auflistung von [ `Setter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Setter/) Objekte. Jede `Setter` ordnet eine [ `Property` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Setter.Property/) mit einem [ `Value` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Setter.Value/).

In XAML die `Property` Einstellung ist der Name einer CLR-Eigenschaft (z. B. die `Text` Eigenschaft `Button`), aber die formatierte-Eigenschaft muss eine bindbare Eigenschaft gesichert werden. Darüber hinaus muss die Eigenschaft definiert werden, in der Klasse, die von der `TargetType` Einstellung oder Klasse geerbt.

Sie können angeben, die `Value` festlegen, mit dem Eigenschaftselement `<Setter.Value>`. Auf diese Weise können Sie festlegen `Value` auf ein Objekt, das in einer Textzeichenfolge, oder auf ausgedrückt werden, kann ein `OnPlatform` Objekt oder mit einem Objekt instanziiert `x:Arguments` oder `x:FactoryMethod`. Die `Value` Eigenschaft kann auch festgelegt werden, mit einem `StaticResource` Ausdruck auf ein anderes Element im Wörterbuch.

Die [ **BasicStyle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/BasicStyle) Programm veranschaulicht die grundlegende Syntax und zeigt, wie verweisen die `Style` mit einem `StaticResource` Markuperweiterung:

[![Dreifacher Screenshot des grundlegenden Stil](images/ch12fg01-small.png "grundlegende Formate bei")](images/ch12fg01-large.png#lightbox "grundlegende Formate")

Die `Style` Objekt und alle Objekte erstellt, der `Style` -Objekt als eine `Value` Einstellung für alle Sichten verweisen, die freigegeben sind `Style`. Die `Style` dürfen nicht alle Funktionen, die, wie z. B. gemeinsam genutzt werden kann, eine `View` Ableitung.

-Ereignishandler können nicht festgelegt werden, einem `Style`. Die `GestureRecognizers` Eigenschaft kann nicht festgelegt werden, einem `Style` , da es nicht durch eine bindbare Eigenschaft gesichert wird.

## <a name="styles-in-code"></a>Stile in code

Obwohl es nicht üblich ist, können Sie instanziieren und initialisieren Sie `Style` Objekte im Code. Dies wird veranschaulicht, durch die [ **BasicStyleCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/BasicStyleCode) Beispiel.

## <a name="style-inheritance"></a>Stil-Vererbung

`Style` verfügt über eine [ `BasedOn` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.BasedOn/) -Eigenschaft, die Sie, um festlegen können eine `StaticResource` Markuperweiterung verweisen auf einen anderen Stil. Dadurch können Stile in früheren Formaten und hinzufügen oder davon erben eigenschafteneinstellungen ersetzen. Die [ **StyleInheritance** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/StyleInheritance) Beispiel veranschaulicht dies.

Wenn `Style2` basiert auf `Style1`, `TargetType` von `Style2` müssen identisch sein `Style1` oder abgeleitete `Style1`. Das Ressourcenwörterbuch, in dem `Style1` gespeichert muss das gleiche Ressourcenwörterbuch als `Style2` oder einem Ressourcenwörterbuch, die weiter oben in der visuellen Struktur.

## <a name="implicit-styles"></a>Impliziten Stilen

Wenn eine `Style` in einer Ressource Wörterbuch verfügt nicht über ein `x:Key` -Attribut festlegen, eine Wörterbuchschlüssel wird automatisch zugewiesen und die `Style` Objekt wird ein *impliziten Stil*. Eine Ansicht ohne eine `Style` Einstellung und entspricht, dessen Typ der `TargetType` genau findet, Stil als die [ **ImplicitStyle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/ImplicitStyle) demonstriert.

Eine implizite Formatvorlage ableiten kann eine `Style` mit einer `x:Key` Einstellung aber nicht umgekehrt. Sie können nicht explizit einen impliziten Stil verweisen.

Sie können drei Arten von Hierarchie mit Stilen implementieren und `BasedOn`:

- Formatvorlagen auf definiert die `Application` und `Page` zu Layouttabellen weiter unten in der visuellen Struktur definierte Formatvorlagen.
- Formatvorlagen für Basisklassen definiert sind, z. B. `VisualElement` und `View` Formatvorlagen, die für bestimmte Klassen definiert.
- Formatvorlagen mit expliziten Wörterbuchschlüssel zum impliziten Stilen.

Diese Hierarchien werden veranschaulicht, der [ **StyleHierarchy** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/StyleHierarchy) Beispiel.

## <a name="dynamic-styles"></a>Dynamische Formate

Ein Format in einem Ressourcenwörterbuch verwiesen werden kann, indem `DynamicResource` statt `StaticResource`. Dadurch wird das Format einer *dynamic Style*. Wenn diesem Format im Ressourcenverzeichnis durch einen anderen Stil mit dem gleichen Schlüssel, die Ansichten, verweisstil mit ersetzt wird `DynamicResource` automatisch geändert. Darüber hinaus verursacht das Fehlen einer Wörterbucheintrag mit dem angegebenen Schlüssel `StaticResource` zum Auslösen einer Ausnahme jedoch nicht `DynamicResource`.

Sie können dieses Verfahren verwenden, formatieren oder Designs als dynamisch ändern die [ **DynamicStyles** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/DynamicStyles) demonstriert.

Allerdings können Sie nicht Festlegen der `BasedOn` Eigenschaft, um eine `DynamicResource` Zusammensetzung Erweiterung da `BasedOn` wird nicht durch eine bindbare Eigenschaft gesichert. Um ein Format dynamisch abgeleitet werden, stellen Sie keine `BasedOn`. Legen Sie stattdessen die [ `BaseResourceKey` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.BaseResourceKey/) Eigenschaft, um die Wörterbuchschlüssel des Formats abgeleitet werden sollen. Die [ **DynamicStylesInheritance** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/DynaStylesInh) Beispiel wird diese Technik veranschaulicht.

## <a name="device-styles"></a>Geräte-Stile

Die [ `Device.Styles` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Device+Styles/) geschachtelte Klasse definiert zwölf statische schreibgeschützte Felder für sechs Formatvorlagen mit einem `TargetType` von `Label` , die Sie für allgemeine Typen von Text Verwendungen verwenden können.

Sechs dieser Felder sind vom Typ `Style` können direkt festlegen einer `Style` -Eigenschaft im Code:

- [`BodyStyle`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.BodyStyle/)
- [`TitleStyle`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.TitleStyle/)
- [`SubtitleStyle`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.SubtitleStyle/)
- [`CaptionStyle`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.CaptionStyle/)
- [`ListItemTextStyle`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.ListItemTextStyle/)
- [`ListItemDetailTextStyle`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.ListItemDetailTextStyle/)

Die anderen sechs Felder sind vom Typ `string` und als Wörterbuchschlüssel für dynamische Formate verwendet werden können:

- [`BodyStyleKey`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.BodyStyleKey/) "BodyStyle" gleich
- [`TitleStyleKey`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.TitleStyleKey/) "TitleStyle" gleich
- [`SubtitleStyleKey`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.SubtitleStyleKey/) "SubtitleStyle" gleich
- [`CaptionStyleKey`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.CaptionStyleKey/) "CaptionStyle" gleich
- [`ListItemTextStyleKey`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.ListItemTextStyleKey/) "ListItemTextStyle" gleich
- [`ListItemDetailTextStyleKey`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.ListItemDetailTextStyleKey/) "ListItemDetailTextStyle" gleich

Diese Stile werden anhand der [ **DeviceStylesList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/DeviceStylesList) Beispiel.



## <a name="related-links"></a>Verwandte Links

- [Kapitel 12 Volltext (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch12-Apr2016.pdf)
- [Kapitel 12-Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12)
- [Stile](~/xamarin-forms/user-interface/styles/index.md)
