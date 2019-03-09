---
title: Zusammenfassung der Kapitel 10. XAML-Markuperweiterungen
description: 'Erstellen von mobilen Apps mit Xamarin.Forms: Zusammenfassung der Kapitel 10. XAML-Markuperweiterungen'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 575EAE55-BD4D-470F-A583-3D065FA102E2
author: davidbritch
ms.author: dabritch
ms.date: 07/19/2018
ms.openlocfilehash: 076e9f5155492e5a69d906c587b24495fe39d3f1
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2019
ms.locfileid: "57672637"
---
# <a name="summary-of-chapter-10-xaml-markup-extensions"></a>Zusammenfassung der Kapitel 10. XAML-Markuperweiterungen

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10)

In der Regel der XAML-Parser eine beliebige Zeichenfolge als Attributwert festgelegt, um den Typ der Eigenschaft, die basierend auf standardkonvertierungen für die grundlegende .NET Datentypen konvertiert oder [ `TypeConverter` ](xref:Xamarin.Forms.TypeConverter) Ableitung angefügt, um die Eigenschaft oder den Typ mit einer [`TypeConverterAttribute`](xref:Xamarin.Forms.TypeConverterAttribute).

Aber manchmal ist es sinnvoll, ein Attribut aus einer anderen Quelle, z. B. ein Element in einem Wörterbuch oder der Wert von einer statischen Eigenschaft oder Feld oder eine Berechnung irgendeiner Art festgelegt.

Dies ist die Aufgabe von einer *XAML-Markuperweiterung*. Trotz des Namens XAML-Markuperweiterungen sind *nicht* eine Erweiterung in XML. XAML ist immer gültige XML-Daten.

## <a name="the-code-infrastructure"></a>Der Code-Infrastruktur

Eine XAML-Markuperweiterung ist eine Klasse, implementiert die [ `IMarkupExtension` ](xref:Xamarin.Forms.Xaml.IMarkupExtension) Schnittstelle. Eine solche Klasse verfügt oft über das Wort `Extension` am Ende des Namens aber normalerweise wird in XAML ohne dieses Suffix.

Der folgenden XAML-Markuperweiterungen werden von allen Implementierungen von XAML unterstützt:

- `x:Static` unterstützt von [`StaticExtension`](xref:Xamarin.Forms.Xaml.StaticExtension)
- `x:Reference` unterstützt von [`ReferenceExtension`](xref:Xamarin.Forms.Xaml.ReferenceExtension)
- `x:Type` unterstützt von [`TypeExtension`](xref:Xamarin.Forms.Xaml.TypeExtension)
- `x:Null` unterstützt von [`NullExtension`](xref:Xamarin.Forms.Xaml.NullExtension)
- `x:Array` unterstützt von [`ArrayExtension`](xref:Xamarin.Forms.Xaml.ArrayExtension)

Diese vier XAML-Markuperweiterungen werden von vielen Implementierungen von XAML, einschließlich Xamarin.Forms unterstützt:

- `StaticResource` unterstützt von [`StaticResourceExtension`](xref:Xamarin.Forms.Xaml.StaticResourceExtension)
- `DynamicResource` unterstützt von [`DynamicResourceExtension`](xref:Xamarin.Forms.Xaml.DynamicResourceExtension)
- `Binding` unterstützt von [ `BindingExtension` ](xref:Xamarin.Forms.Xaml.BindingExtension) &mdash;ausführlicher [Kapitel 16. Datenbindung](chapter16.md)
- `TemplateBinding` unterstützt von [ `TemplateBindingExtension` ](xref:Xamarin.Forms.Xaml.TemplateBindingExtension) &mdash;nicht im Buch behandelt.

Eine zusätzliche Erweiterung des XAML-Markup befindet sich in Xamarin.Forms in Verbindung mit [ `RelativeLayout` ](xref:Xamarin.Forms.RelativeLayout):

- [`ConstraintExpression`](xref:Xamarin.Forms.ConstraintExpression)&mdash;nicht behandelt im Buch.

## <a name="accessing-static-members"></a>Zugriff auf statische Member

Verwenden der [ `x:Static` ](xref:Xamarin.Forms.Xaml.StaticExtension) -Elements ein Attribut auf den Wert eines öffentlichen, statischen Eigenschaft, Feld oder eines Enumerationswerts Members. Legen Sie die [ `Member` ](xref:Xamarin.Forms.Xaml.StaticExtension.Member) Eigenschaft, um den statischen Member. Es ist in der Regel einfacher an `x:Static` und den Namen des Members in geschweiften Klammern. Der Name des der `Member` Eigenschaft muss nicht eingeschlossen ist, werden nur das Element selbst. Diese allgemeine Syntax wird angezeigt, der [ **SharedStatics** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/SharedStatics) Beispiel. Die statische Felder selbst sind definiert, der [ `AppConstants` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter10/SharedStatics/SharedStatics/SharedStatics/AppConstants.cs) Klasse. Dieses Verfahren können Sie über ein Programm verwendete Konstanten herzustellen.

Mit zusätzlichen XML-Namespacedeklaration, Sie können auf verweisen öffentliche statische Eigenschaften, Felder oder Enumerationsmember, die in .NET Framework definierten wie in der [ **SystemStatics** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/SystemStatics) Beispiel .

## <a name="resource-dictionaries"></a>Ressourcenverzeichnis

Die `VisualElement` -Klasse definiert eine Eigenschaft namens [ `Resources` ](xref:Xamarin.Forms.VisualElement.Resources) , die Sie festlegen können, um ein Objekt des Typs [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary). In XAML, können Sie Elemente in diesem Wörterbuch speichern und identifizieren sie mit der `x:Key` Attribut. Die Elemente, die im Ressourcenverzeichnis gespeichert werden für alle Verweise auf das Element freigegeben.

### <a name="staticresource-for-most-purposes"></a>StaticResource für die meisten Zwecke

In den meisten Fällen verwenden Sie die [ `StaticResource` ](xref:Xamarin.Forms.Xaml.StaticResourceExtension) Markuperweiterung für ein Element aus dem Ressourcenverzeichnis zu verweisen, wie die [ **ResourceSharing** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/ResourceSharing) Beispiel . Sie können eine `StaticResourceExtension` Element oder `StaticResource` in geschweiften Klammern:

[![Dreifacher Screenshot der gemeinsamen ressourcennutzung](images/ch10fg03-small.png "Ressourcenfreigabe")](images/ch10fg03-large.png#lightbox "gemeinsame Nutzung von Ressourcen")

Verwechseln Sie nicht die `x:Static` Markuperweiterung und `StaticResource` Markuperweiterung.

### <a name="a-tree-of-dictionaries"></a>Eine Struktur von Wörterbüchern

Wenn der XAML-Parser erkennt eine `StaticResource`, es beginnt die Suche, die die visuelle Struktur nach einem übereinstimmenden Schlüssel, und sucht dann in der `ResourceDictionary` in der Anwendung `App` Klasse. Dadurch können Elemente in einem Ressourcenverzeichnis tiefer in der visuellen Struktur ein Ressourcenverzeichnisses, die weiter oben in der visuellen Struktur zu überschreiben. Dies wird veranschaulicht, der [ **ResourceTrees** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/ResourceTrees) Beispiel.

### <a name="dynamicresource-for-special-purposes"></a>DynamicResource für besondere Zwecke

Die `StaticResource` Markuperweiterung bewirkt, dass ein Element aus dem Wörterbuch abgerufen werden soll, wenn eine visuelle Struktur, während erstellt wird die `InitializeComponent` aufrufen. Eine Alternative zum `StaticResource` ist [ `DynamicResource` ](xref:Xamarin.Forms.Xaml.DynamicResourceExtension), die einen Link zu der Wörterbuchschlüssel verwaltet und aktualisiert das Ziel aus, wenn das Element durch die wichtigsten Änderungen auf die verwiesen wird.

Der Unterschied zwischen `StaticResource` und `DynamicResource` wird veranschaulicht, der [ **DynamicVsStatic** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/DynamicVsStatic) Beispiel.

Eine Eigenschaft festlegen, indem `DynamicResource` muss durch eine bindbare Eigenschaft unterstützt werden, wie unter [Kapitel 11, die bindbare Infrastruktur](chapter11.md).

## <a name="lesser-used-markup-extensions"></a>Wenig genutzte Markuperweiterungen

Verwenden der [ `x:Null` ](xref:Xamarin.Forms.Xaml.NullExtension) Markuperweiterung, eine Eigenschaft festzulegen, um `null`.

Verwenden der [ `x:Type` ](xref:Xamarin.Forms.Xaml.TypeExtension) Markuperweiterung zum Festlegen einer Eigenschaft in ein .NET `Type` Objekt.

Verwendung [ `x:Array` ](xref:Xamarin.Forms.Xaml.ArrayExtension) um ein Array zu definieren. Geben Sie den Typ der Arraymember durch Festlegen der [`Type`]-Eigenschaft auf eine `x:Type` Markuperweiterung.

## <a name="a-custom-markup-extension"></a>Eine benutzerdefinierte Markuperweiterung

Sie können Ihre eigenen XAML-Markuperweiterungen erstellen, indem Sie eine Klasse, die implementiert schreiben die [ `IMarkupExtension` ](xref:Xamarin.Forms.Xaml.IMarkupExtension) eine Verbindung mit einem [ `ProvideValue` ](xref:Xamarin.Forms.Xaml.IMarkupExtension.ProvideValue(System.IServiceProvider)) Methode.

Die [ `HslColorExtension` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/HslColorExtension.cs) Klasse erfüllt diese Anforderung. Erstellt einen Wert vom Typ `Color` basierend auf den Werten der Eigenschaften, die mit dem Namen `H`, `S`, `L`, und `A`. Diese Klasse ist das erste Element in einer Xamarin.Forms-Bibliothek, die mit dem Namen [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) , die Sie erstellt und im Laufe dieses Buchs verwendet.

Die [ **CustomExtensionDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/CustomExtensionDemo) Beispiel veranschaulicht, wie diese Bibliothek verweisen, und verwenden die benutzerdefinierte Markuperweiterung.

## <a name="related-links"></a>Verwandte Links

- [Kapitel 10 Volltext (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch10-Apr2016.pdf)
- [Kapitel 10-Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10)
- [XAML-Markuperweiterungen](~/xamarin-forms/xaml/markup-extensions/index.md)
