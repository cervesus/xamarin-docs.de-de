---
title: Zusammenfassung der Kapitel 10. Verwendung von XAML-Markuperweiterungen
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 8ded1dba0e1d4d1a9062d0f75935b3d748a83370
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="summary-of-chapter-10-xaml-markup-extensions"></a>Zusammenfassung der Kapitel 10. Verwendung von XAML-Markuperweiterungen

In der Regel wird die Verwendung von XAML-Parser konvertiert eine beliebige Zeichenfolge, die auf den Typ der Eigenschaft basierend auf standardkonvertierungen für die grundlegenden Datentypen .NET als Attributwert festgelegt oder ein [ `TypeConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TypeConverter/) Ableitung angefügt, um die Eigenschaft oder dessen Typ mit einer [`TypeConverterAttribute`](https://developer.xamarin.com/api/type/Xamarin.Forms.TypeConverterAttribute/).

Jedoch ist es manchmal sinnvoll, ein Attribut aus einer anderen Quelle, z. B. ein Element in einem Wörterbuch oder den Wert für eine statische Eigenschaft oder ein Feld, oder eine Berechnung der irgendeine festgelegt.

Dies ist die Aufgabe von einer *XAML-Markuperweiterung*. Ungeachtet des Namens XAML-Markuperweiterungen sind *nicht* eine Erweiterung in XML. XAML ist immer gültige XML-Daten.

## <a name="the-code-infrastructure"></a>Der Code-Infrastruktur

Eine XAML-Markuperweiterung ist eine Klasse, implementiert die [ `IMarkupExtension` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.IMarkupExtension/) Schnittstelle. Eine solche Klasse weist häufig das Wort `Extension` am Ende des Namens jedoch normalerweise in XAML wird ohne angezeigt, Suffix.

Der folgenden XAML-Markuperweiterungen werden von der alle Implementierungen von XAML unterstützt:

- `x:Static` Unterstützt von [`StaticExtension`](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.StaticExtension/)
- `x:Reference` Unterstützt von [`ReferenceExtension`](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.ReferenceExtension/)
- `x:Type` Unterstützt von [`TypeExtension`](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.TypeExtension/)
- `x:Null` Unterstützt von [`NullExtension`](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.NullExtension/)
- `x:Array` Unterstützt von [`ArrayExtension`](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.ArrayExtension/)

Diese vier XAML-Markuperweiterungen werden durch die verschiedensten Implementierungen dieses XAML, einschließlich Xamarin.Forms unterstützt:

- `StaticResource` Unterstützt von [`StaticResourceExtension`](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.StaticResourceExtension/)
- `DynamicResource` Unterstützt von [`DynamicResourceExtension`](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.DynamicResourceExtension/)
- `Binding` unterstützt von [ `BindingExtension` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.BindingExtension/) &mdash;in behandelten [Kapitel 16. Datenbindung](#chapter16)
- `TemplateBinding` unterstützt von [ `TemplateBindingExtension` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.TemplateBindingExtension/) &mdash;im Handbuch nicht behandelt.

Ist eine zusätzliche Verwendung von XAML-Markuperweiterung in Verbindung mit Xamarin.Forms enthalten [ `RelativeLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.RelativeLayout/):

- [`ConstraintExpression`](https://developer.xamarin.com/api/type/Xamarin.Forms.ConstraintExpression/)&mdash;im Handbuch behandelt nicht.

## <a name="accessing-static-members"></a>Zugriff auf statische Member

Verwenden der [ `x:Static` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.StaticExtension/) ein Attribut auf den Wert eines öffentlichen, statischen-Eigenschaft, ein Feld oder Enumeration Members festzulegenden Elements. Legen Sie die [ `Member` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Xaml.StaticExtension.Member/) Eigenschaft, um den statischen Member. Es ist in der Regel einfacher, geben Sie `x:Static` und den Elementnamen in geschweiften Klammern. Der Name des der `Member` Eigenschaft muss nicht eingeschlossen werden, werden nur das Element selbst. Diese allgemeine Syntax wird angezeigt, der [ **SharedStatics** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/SharedStatics) Beispiel. Die statischen Felder selbst werden definiert, der [ `AppConstants` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter10/SharedStatics/SharedStatics/SharedStatics/AppConstants.cs) Klasse. Bei dieser Technik können Sie über ein Programm verwendete Konstanten herstellen.

Mit zusätzlichen XML-Namespacedeklaration, Sie können verweisen öffentliche statische Eigenschaften, Felder oder Enumerationsmember, die in .NET Framework definierten wie in der [ **SystemStatics** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/SystemStatics) Beispiel .

## <a name="resource-dictionaries"></a>Ressourcenverzeichnis

Die `VisualElement` Klasse definiert eine Eigenschaft namens [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Resources/) , die Sie können festlegen, um ein Objekt des Typs [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/). In XAML, Sie können Elemente in diesem Wörterbuch speichern identifizieren und beheben sie mit der `x:Key` Attribut. Die im Ressourcenverzeichnis gespeicherten Elemente werden für alle Verweise auf das Element freigegeben.

### <a name="staticresource-for-most-purposes"></a>In den meisten Fällen StaticResource

In den meisten Fällen verwenden Sie die [ `StaticResource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.StaticResourceExtension/) Markuperweiterung auf ein Element aus dem Ressourcenwörterbuch verweisen, wie durch die [ **ResourceSharing** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/ResourceSharing) Beispiel . Sie können eine `StaticResourceExtension` Element oder `StaticResource` in geschweiften Klammern:

[![Dreifacher Screenshot der Ressourcenfreigabe](images/ch10fg03-small.png "Ressourcenfreigabe")](images/ch10fg03-large.png "Ressourcenfreigabe")

Verwechseln Sie nicht die `x:Static` Markuperweiterung und `StaticResource` Markuperweiterung.

### <a name="a-tree-of-dictionaries"></a>Eine Struktur von Wörterbüchern

Bei der Verwendung von XAML-Parser erkennt eine `StaticResource`, es beginnt die Suche, um die visuelle Struktur für ein übereinstimmender Schlüssel, und sucht dann der `ResourceDictionary` in der Anwendungsverzeichnis `App` Klasse. Dadurch werden Elemente in einem Ressourcenwörterbuch tiefer in der visuellen Struktur ein Ressourcenverzeichnisses weiter oben in der visuellen Struktur zu überschreiben. Dies wird dargestellt, der [ **ResourceTrees** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/ResourceTrees) Beispiel.

### <a name="dynamicresource-for-special-purposes"></a>DynamicResource für spezielle Zwecke

Die `StaticResource` Markuperweiterung bewirkt, dass ein Element aus dem Wörterbuch abgerufen werden soll, wenn eine visuelle Struktur, während erstellt wird die `InitializeComponent` aufrufen. Eine Alternative zum `StaticResource` ist [ `DynamicResource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.DynamicResourceExtension/), die einen Link zu der Wörterbuchschlüssel verwaltet und das Ziel aktualisiert, wenn das Element durch die wichtigsten Änderungen auf die verwiesen wird.

Der Unterschied zwischen `StaticResource` und `DynamicResource` wird veranschaulicht, der [ **DynamicVsStatic** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/DynamicVsStatic) Beispiel.

Eine Eigenschaft festlegen, indem `DynamicResource` müssen über eine bindbare Eigenschaft gesichert werden, wie beschrieben in [Kapitel 11 bindbare Infrastruktur](chapter11.md).

## <a name="lesser-used-markup-extensions"></a>Kleinere verwendet Markuperweiterungen

Verwenden der [ `x:Null` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.NullExtension/) Markuperweiterung, eine Eigenschaft festzulegen, um `null`.

Verwenden der [ `x:Type` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.TypeExtension/) Markuperweiterung zum Festlegen einer Eigenschaft in ein .NET `Type` Objekt.

Verwendung [ `x:Array` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.ArrayExtension/) , ein Array zu definieren. Geben Sie den Typ der Array-Elemente durch Festlegen der [`Type`]-Eigenschaft auf eine `x:Type` Markuperweiterung.

## <a name="a-custom-markup-extension"></a>Eine benutzerdefinierte Markuperweiterung

Sie können Ihre eigenen Markuperweiterungen für XAML erstellen, indem Sie beim Schreiben einer Klasse, implementiert die [ `IMarkupExtension` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.IMarkupExtension/) eine Verbindung mit einer [ `ProvideValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Xaml.IMarkupExtension.ProvideValue/p/System.IServiceProvider/) Methode.

Die [ `HslColorExtension` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/HslColorExtension.cs) Klasse diese Anforderung erfüllt. Erstellt einen Wert vom Typ `Color` basierend auf Werten von Eigenschaften, die mit dem Namen `H`, `S`, `L`, und `A`. Diese Klasse ist das erste Element in einer Xamarin.Forms-Bibliothek, die mit dem Namen [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) , erstellt und im Verlauf dieses Buchs verwendet wird.

Die [ **CustomExtensionDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/CustomExtensionDemo) Beispiel zeigt, wie zum Verweisen auf diese Bibliothek und die benutzerdefinierte Markuperweiterung verwenden.



## <a name="related-links"></a>Verwandte Links

- [Kapitel 10 Volltext (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch10-Apr2016.pdf)
- [Kapitel 10-Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10)
