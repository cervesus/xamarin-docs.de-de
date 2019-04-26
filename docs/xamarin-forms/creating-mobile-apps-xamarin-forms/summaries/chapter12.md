---
title: Zusammenfassung der Kapitel 12. Stile
description: 'Erstellen von mobilen Apps mit Xamarin.Forms: Zusammenfassung der Kapitel 12. Stile'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 3EAE6BDC-8EFB-464B-A87B-1C35B8387BB3
author: davidbritch
ms.author: dabritch
ms.date: 07/19/2018
ms.openlocfilehash: bb2cd1c97cc588923e0da1a8793f16445c111f0e
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61334343"
---
# <a name="summary-of-chapter-12-styles"></a>Zusammenfassung der Kapitel 12. Stile

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12)

In Xamarin.Forms können Stile mehrere Ansichten für eine Sammlung von Einstellungen der Eigenschaften gemeinsam nutzen. Dies reduziert die Markup und konsistente visuelle Designs zu verwalten.

Stile werden fast immer definiert und im Markup verarbeitet. Ein Objekt des Typs [ `Style` ](xref:Xamarin.Forms.Style) in einem Ressourcenverzeichnis instanziiert ist, und legen Sie dann auf die [ `Style` ](xref:Xamarin.Forms.VisualElement.Style) Eigenschaft eines visuellen Elements mit einem `StaticResource` oder `DynamicResource` Markup die Erweiterung.

## <a name="the-basic-style"></a>Das grundlegende Format

Ein `Style` erfordert, dass die [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType) in den Typ des visuellen Objekts für gilt festgelegt werden. Wenn eine `Style` instanziiert wird in einem Ressourcenverzeichnis (wie üblich ist) müssen sie auch eine `x:Key` Attribut.

Die `Style` verfügt über eine Content-Eigenschaft des Typs [ `Setters` ](xref:Xamarin.Forms.Style.Setters), dies ist eine Auflistung von [ `Setter` ](xref:Xamarin.Forms.Setter) Objekte. Jede `Setter` ordnet eine [ `Property` ](xref:Xamarin.Forms.Setter.Property) mit einem [ `Value` ](xref:Xamarin.Forms.Setter.Value).

In XAML die `Property` Einstellung ist der Name einer CLR-Eigenschaft (z. B. die `Text` Eigenschaft `Button`), aber die formatierte-Eigenschaft muss durch eine bindbare Eigenschaft unterstützt werden. Darüber hinaus muss die Eigenschaft definiert werden, in der Klasse, angegeben durch die `TargetType` Einstellung, oder der von dieser Klasse geerbt.

Sie können angeben, die `Value` Einstellungen mithilfe der Property-Element `<Setter.Value>`. Dieser Option können Sie `Value` auf ein Objekt, das eine Zeichenfolge, oder auf ausgedrückt werden kann ein `OnPlatform` Objekt oder mit einem Objekt instanziiert `x:Arguments` oder `x:FactoryMethod`. Die `Value` Eigenschaft kann auch festgelegt werden, mit einem `StaticResource` Ausdruck in ein anderes Element im Wörterbuch.

Die [ **BasicStyle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/BasicStyle) Programm veranschaulicht die grundlegende Syntax und zeigt, wie auf die `Style` mit einem `StaticResource` Markuperweiterung:

[![Dreifacher Screenshot der grundlegenden Stil](images/ch12fg01-small.png "grundlegende Formate bei")](images/ch12fg01-large.png#lightbox "grundlegende Formate")

Die `Style` -Objekt und ein Objekt erstellt, der `Style` als Objekt ein `Value` Einstellung werden alle Sichten verweisen, freigegeben `Style`. Die `Style` dürfen nicht alle Elemente, z. B. gemeinsam genutzt werden kann, die eine `View` Ableitung.

-Ereignishandler können nicht festgelegt werden, einem `Style`. Die `GestureRecognizers` Eigenschaft nicht festgelegt werden, einem `Style` , da es nicht durch eine bindbare Eigenschaft gesichert wird.

## <a name="styles-in-code"></a>Formatvorlagen in code

Obwohl es keine allgemeine ist, können Sie instanziiert und initialisiert `Style` -Objekte im Code. Dies wird veranschaulicht, durch die [ **BasicStyleCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/BasicStyleCode) Beispiel.

## <a name="style-inheritance"></a>Stilvererbung

`Style` verfügt über eine [ `BasedOn` ](xref:Xamarin.Forms.Style.BasedOn) -Eigenschaft, die Sie, um festlegen können eine `StaticResource` Markuperweiterung verweisen auf ein anderes Format. Dadurch können Stile zum erben von der vorherigen Formatvorlagen, hinzufügen und ersetzen die eigenschafteneinstellungen. Die [ **StyleInheritance** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/StyleInheritance) veranschaulicht dies.

Wenn `Style2` basiert auf `Style1`, `TargetType` von `Style2` müssen identisch sein `Style1` oder davon abgeleitete `Style1`. Das Ressourcenverzeichnis in der `Style1` gespeichert muss das gleiche Ressourcenverzeichnis als `Style2` oder eines Ressourcenverzeichnisses, die weiter oben in der visuellen Struktur.

## <a name="implicit-styles"></a>Implizite Stile

Wenn eine `Style` in einer Ressource Wörterbuch verfügt nicht über eine `x:Key` attributeinstellung, einen Dictionary-Schlüssel wird automatisch zugewiesen und die `Style` Objekt wird ein *impliziter Stil*. Eine Ansicht ohne eine `Style` Einstellung und entspricht, dessen Typ die `TargetType` genau sehen, Stil, wie die [ **ImplicitStyle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/ImplicitStyle) Beispiel wird veranschaulicht.

Ein impliziter Stil ableiten kann eine `Style` mit einer `x:Key` Einstellung jedoch nicht umgekehrt. Sie können nicht explizit einen impliziten Stil verweisen.

Sie können drei Arten von Hierarchie mit Stilen implementieren und `BasedOn`:

- Von Stilen definiert die `Application` und `Page` auf Stile, Layouts, die weiter unten in der visuellen Struktur definiert.
- Von Stilen für Basisklassen definiert, wie z. B. `VisualElement` und `View` , Stile, die für bestimmte Klassen definiert.
- Von Stilen mit expliziten Wörterbuchschlüssel zum impliziten Stilen.

Diese Hierarchien werden veranschaulicht die [ **StyleHierarchy** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/StyleHierarchy) Beispiel.

## <a name="dynamic-styles"></a>Dynamische Stile

Ein Stil in einem Ressourcenwörterbuch verwiesen werden kann, durch `DynamicResource` statt `StaticResource`. Dadurch wird das Format einer *dynamic Style*. Wenn dieser Art im Ressourcenverzeichnis durch ein anderes Format mit dem gleichen Schlüssel, die Ansichten, die dieser Stil mit ersetzt wird `DynamicResource` automatisch geändert wird. Darüber hinaus bewirkt das Fehlen von einen Wörterbucheintrag mit dem angegebenen Schlüssel `StaticResource` zum Auslösen einer Ausnahme aber nicht `DynamicResource`.

Sie können dieses Verfahren verwenden, Stile oder Designs als dynamisch ändern der [ **DynamicStyles** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/DynamicStyles) veranschaulicht.

Allerdings können Sie nicht Festlegen der `BasedOn` Eigenschaft, um eine `DynamicResource` Zusammensetzung Erweiterung da `BasedOn` wird nicht unterstützt, durch eine bindbare Eigenschaft. Legen Sie einen Stil dynamisch ableiten, nicht `BasedOn`. Legen Sie stattdessen die [ `BaseResourceKey` ](xref:Xamarin.Forms.Style.BaseResourceKey) Eigenschaft, um die Wörterbuchschlüssel des Stils abgeleitet werden soll. Die [ **DynamicStylesInheritance** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/DynaStylesInh) Beispiel wird diese Technik veranschaulicht.

## <a name="device-styles"></a>Gerätestile

Die [ `Device.Styles` ](xref:Xamarin.Forms.Device.Styles) geschachtelte Klasse definiert zwölf statische schreibgeschützte Felder für sechs Formatvorlagen mit einem `TargetType` von `Label` , die Sie für allgemeine Typen von Text Verwendungen verwenden können.

Sechs dieser Felder sind vom Typ `Style` , dass Sie festlegen können, direkt an eine `Style` Eigenschaft im Code:

- [`BodyStyle`](xref:Xamarin.Forms.Device.Styles.BodyStyle)
- [`TitleStyle`](xref:Xamarin.Forms.Device.Styles.TitleStyle)
- [`SubtitleStyle`](xref:Xamarin.Forms.Device.Styles.SubtitleStyle)
- [`CaptionStyle`](xref:Xamarin.Forms.Device.Styles.CaptionStyle)
- [`ListItemTextStyle`](xref:Xamarin.Forms.Device.Styles.ListItemTextStyle)
- [`ListItemDetailTextStyle`](xref:Xamarin.Forms.Device.Styles.ListItemDetailTextStyle)

Die anderen sechs Felder sind vom Typ `string` und kann als Wörterbuchschlüssel für dynamische Stile verwendet werden:

- [`BodyStyleKey`](xref:Xamarin.Forms.Device.Styles.BodyStyleKey) gleich "BodyStyle"
- [`TitleStyleKey`](xref:Xamarin.Forms.Device.Styles.TitleStyleKey) gleich "TitleStyle"
- [`SubtitleStyleKey`](xref:Xamarin.Forms.Device.Styles.SubtitleStyleKey) gleich "SubtitleStyle"
- [`CaptionStyleKey`](xref:Xamarin.Forms.Device.Styles.CaptionStyleKey) gleich "CaptionStyle"
- [`ListItemTextStyleKey`](xref:Xamarin.Forms.Device.Styles.ListItemTextStyleKey) gleich "ListItemTextStyle"
- [`ListItemDetailTextStyleKey`](xref:Xamarin.Forms.Device.Styles.ListItemDetailTextStyleKey) gleich "ListItemDetailTextStyle"

Diese Formate sind dargestellt, durch die [ **DeviceStylesList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/DeviceStylesList) Beispiel.

## <a name="related-links"></a>Verwandte Links

- [Kapitel 12 Volltext (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch12-Apr2016.pdf)
- [Kapitel 12-Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12)
- [Stile](~/xamarin-forms/user-interface/styles/index.md)
