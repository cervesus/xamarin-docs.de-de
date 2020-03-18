---
title: Zusammenfassung von Kapitel 12. Stile
description: 'Erstellen von mobilen Apps mit Xamarin.Forms: Zusammenfassung von Kapitel 12. Stile'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 3EAE6BDC-8EFB-464B-A87B-1C35B8387BB3
author: davidbritch
ms.author: dabritch
ms.date: 07/19/2018
ms.openlocfilehash: 408f171a3c7c690b700f7be21a3dcaff503467d9
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2020
ms.locfileid: "65926909"
---
# <a name="summary-of-chapter-12-styles"></a>Zusammenfassung von Kapitel 12. Stile

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12)

In Xamarin.Forms ermöglichen Stile die gemeinsame Nutzung einer Sammlung von Eigenschaftseinstellungen durch mehrere Ansichten. Dadurch wird Markup verringert und die Aufrechterhaltung konsistenter visueller Designs ermöglicht.

Stile werden fast immer im Markup definiert und verbraucht. Ein Objekt vom Typ [`Style`](xref:Xamarin.Forms.Style) wird in einem Ressourcenverzeichnis instanziiert und dann mithilfe einer `StaticResource`- oder `DynamicResource`-Markuperweiterung auf die [`Style`](xref:Xamarin.Forms.NavigableElement.Style)-Eigenschaft eines visuellen Elements festgelegt.

## <a name="the-basic-style"></a>Der grundlegende Stil

Ein `Style` erfordert, dass sein [`TargetType`](xref:Xamarin.Forms.Style.TargetType) auf den Typ des visuellen Objekts festgelegt wird, für den er gilt. Wenn ein `Style` in einem Ressourcenverzeichnis instanziiert wird (wie es üblich ist), erfordert er auch ein `x:Key`-Attribut.

Der `Style` besitzt eine Inhaltseigenschaft vom Typ [`Setters`](xref:Xamarin.Forms.Style.Setters), bei der es sich um eine Sammlung von [`Setter`](xref:Xamarin.Forms.Setter)-Objekten handelt. Jeder `Setter` ordnet einer [`Property`](xref:Xamarin.Forms.Setter.Property) einen [`Value`](xref:Xamarin.Forms.Setter.Value) zu.

In XAML ist die `Property`-Einstellung der Name einer CLR-Eigenschaft (z. B. die `Text`-Eigenschaft von `Button`), aber die über eine Vorlage formatierte Eigenschaft muss von einer bindbaren Eigenschaft unterstützt werden. Außerdem muss die Eigenschaft in der Klasse definiert werden, die durch die `TargetType`-Einstellung angegeben oder von dieser Klasse geerbt wird.

Sie können die `Value`-Eigenschaft mithilfe des Eigenschaftselements `<Setter.Value>` angeben. Dieses gestattet Ihnen das Festlegen von `Value` auf ein Objekt, das sich nicht in einer Textzeichenfolge ausdrücken lässt, oder auf ein `OnPlatform`-Objekt oder auf ein Objekt, das mithilfe von `x:Arguments` oder `x:FactoryMethod` instanziiert wurde. Die `Value`-Eigenschaft kann auch mit einem `StaticResource`-Ausdruck auf ein anderes Element im Verzeichnis festgelegt werden.

Das [**BasicStyle**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/BasicStyle)-Programm veranschaulicht die grundlegende Syntax und zeigt, wie Sie mit einer `StaticResource`-Markuperweiterung auf den `Style` verweisen:

[![Dreifacher Screenshot des grundlegenden Stils](images/ch12fg01-small.png "Grundlegende Stile")](images/ch12fg01-large.png#lightbox "Grundlegende Stile")

Das `Style`-Objekt und jedes in dem `Style`-Objekt als `Value`-Einstellung erstellte Objekt werden von allen Ansichten gemeinsam genutzt, die auf den `Style` verweisen. Der `Style` kann nichts enthalten, das nicht freigegeben werden kann, wie z. B. eine `View`-Ableitung.

Ereignishandler können in einem `Style` nicht festgelegt werden. Die `GestureRecognizers`-Eigenschaft kann in einem `Style` nicht festgelegt werden, weil sie nicht von einer bindbaren Eigenschaft unterstützt wird.

## <a name="styles-in-code"></a>Stile in Code

Es ist zwar nicht gängig, doch Sie können `Style`-Objekte in Code instanziieren und initialisieren. Dieser Vorgang wird im [**BasicStyleCode**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/BasicStyleCode)-Beispiel veranschaulicht.

## <a name="style-inheritance"></a>Stilvererbung

`Style` besitzt eine [`BasedOn`](xref:Xamarin.Forms.Style.BasedOn)-Eigenschaft, die Sie auf eine `StaticResource`-Markuperweiterung festlegen können, die auf einen weiteren Stil verweist. Dies ermöglicht Stilen, von vorherigen Stilen zu erben und Eigenschaftseinstellungen hinzuzufügen oder zu ersetzen. Dies wird im [**StudentCardFile**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/StyleInheritance)-Beispiel veranschaulicht.

Falls `Style2` auf `Style1` basiert, muss der `TargetType` von `Style2` mit `Style1` identisch oder von `Style1` abgeleitet sein. Das Ressourcenverzeichnis, in dem `Style1` gespeichert ist, muss mit dem Ressourcenverzeichnis von `Style2` identisch sein oder ein Ressourcenverzeichnis, das sich höher in der visuellen Struktur befindet.

## <a name="implicit-styles"></a>Implizite Stile

Wenn ein `Style` in einem Ressourcenverzeichnis keine `x:Key`-Attributeinstellung besitzt, wird ihm automatisch ein Verzeichnisschlüssel zugewiesen, und das `Style`-Objekt wird zu einem *impliziten Stil*. Eine Ansicht ohne eine `Style`-Einstellung deren Typ exakt mit dem `TargetType` übereinstimmt, findet diesen Stil, wie im [**ImplicitStyle**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/ImplicitStyle)-Beispiel gezeigt.

Ein impliziter Stil kann von einem `Style` mit einer `x:Key`-Einstellung abgeleitet werden, aber nicht umgekehrt. Sie können nicht explizit auf einen impliziten Stil verweisen.

Sie können drei Typen von Hierarchien mit Stilen und `BasedOn` implementieren:

- Aus in `Application` und `Page` definierten Stilen bis hinunter zu Stilen, die in Layouts weiter unten in der visuellen Struktur definiert sind.
- Aus für Basisklassen definierten Stilen wie `VisualElement` und `View` bis hin zu für bestimmte Klassen definierten Stilen.
- Aus Stilen mit expliziten Verzeichnisschlüsseln bis hin zu impliziten Stilen.

Diese Hierarchien werden im [**StyleHierarchy**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/StyleHierarchy)-Beispiel veranschaulicht.

## <a name="dynamic-styles"></a>Dynamische Stile

Auf einen Stil in einem Ressourcenverzeichnis lässt sich eher mit `DynamicResource` als mit `StaticResource` verweisen. Hierdurch wird der Stil zu einem *dynamischen Stil*. Wenn dieser Stil im Ressourcenverzeichnis durch einen anderen Stil mit demselben Schlüssel ersetzt wird, ändern sich die Ansichten automatisch, die mit `DynamicResource` auf diesen Stil verweisen. Außerdem führt das Fehlen eines Verzeichniseintrags mit dem angegebenen Schlüssel dazu, dass `StaticResource` eine Ausnahme auslöst, `DynamicResource` aber nicht.

Sie können diese Methode verwenden, um Stile oder Designs dynamisch zu ändern, wie im [**DynamicStyles**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/DynamicStyles)-Beispiel veranschaulicht wird.

Sie können jedoch nicht die `BasedOn`-Eigenschaft auf eine `DynamicResource`-Markuperweiterung festlegen, weil `BasedOn` nicht von einer bindbaren Eigenschaft unterstützt wird. Um einen Stil dynamisch abzuleiten, legen Sie `BasedOn` nicht fest. Legen Sie stattdessen die [`BaseResourceKey`](xref:Xamarin.Forms.Style.BaseResourceKey)-Eigenschaft auf den Verzeichnisschlüssel des Stils fest, von dem Sie ableiten möchten. Diese Methode wird im [**DynamicStylesInheritance**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/DynaStylesInh)-Beispiel veranschaulicht.

## <a name="device-styles"></a>Gerätestile

Die geschachtelte [`Device.Styles`](xref:Xamarin.Forms.Device.Styles)-Klasse definiert zwölf statische, schreibgeschützte Felder für sechs Stile mit einem `TargetType` von `Label`, das Sie für gängige Typen der Textverwendung verwenden können.

Sechs dieser Felder sind vom Typ `Style`, den Sie in Code direkt auf eine `Style`-Eigenschaft festlegen können:

- [`BodyStyle`](xref:Xamarin.Forms.Device.Styles.BodyStyle)
- [`TitleStyle`](xref:Xamarin.Forms.Device.Styles.TitleStyle)
- [`SubtitleStyle`](xref:Xamarin.Forms.Device.Styles.SubtitleStyle)
- [`CaptionStyle`](xref:Xamarin.Forms.Device.Styles.CaptionStyle)
- [`ListItemTextStyle`](xref:Xamarin.Forms.Device.Styles.ListItemTextStyle)
- [`ListItemDetailTextStyle`](xref:Xamarin.Forms.Device.Styles.ListItemDetailTextStyle)

Die anderen sechs Felder sind vom Typ `string` und können als Verzeichnisschlüssel für dynamische Stile verwendet werden:

- [`BodyStyleKey`](xref:Xamarin.Forms.Device.Styles.BodyStyleKey) entspricht „BodyStyle“.
- [`TitleStyleKey`](xref:Xamarin.Forms.Device.Styles.TitleStyleKey) entspricht „TitleStyle“.
- [`SubtitleStyleKey`](xref:Xamarin.Forms.Device.Styles.SubtitleStyleKey) entspricht „SubtitleStyle“.
- [`CaptionStyleKey`](xref:Xamarin.Forms.Device.Styles.CaptionStyleKey) entspricht „CaptionStyle“.
- [`ListItemTextStyleKey`](xref:Xamarin.Forms.Device.Styles.ListItemTextStyleKey) entspricht „ListItemTextStyle“.
- [`ListItemDetailTextStyleKey`](xref:Xamarin.Forms.Device.Styles.ListItemDetailTextStyleKey) entspricht „ListItemDetailTextStyle“.

Diese Stile werden im [**DeviceStylesList**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/DeviceStylesList)-Beispiel illustriert.

## <a name="related-links"></a>Verwandte Links

- [Kapitel 12 – vollständiger Text (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch12-Apr2016.pdf)
- [Kapitel 12 – Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12)
- [Stile](~/xamarin-forms/user-interface/styles/index.md)
