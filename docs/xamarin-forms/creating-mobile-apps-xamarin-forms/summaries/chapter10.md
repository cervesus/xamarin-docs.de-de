---
title: Zusammenfassung von Kapitel 10. XAML-Markuperweiterungen
description: 'Erstellen von mobilen Apps mit Xamarin.Forms: Zusammenfassung von Kapitel 10. XAML-Markuperweiterungen'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 575EAE55-BD4D-470F-A583-3D065FA102E2
author: davidbritch
ms.author: dabritch
ms.date: 07/19/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 8f23034df684e778677e4f2e480e1c41807536fb
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84136811"
---
# <a name="summary-of-chapter-10-xaml-markup-extensions"></a>Zusammenfassung von Kapitel 10. XAML-Markuperweiterungen

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10)

Normalerweise konvertiert der XAML-Parser alle als Attributwert festgelegten Zeichenfolgen in den Typ der Eigenschaft, basierend auf Standardkonversionen für die grundlegenden .NET-Datentypen, oder eine [`TypeConverter`](xref:Xamarin.Forms.TypeConverter)-Ableitung, die an die Eigenschaft angefügt ist, oder deren Typ mit einem [`TypeConverterAttribute`](xref:Xamarin.Forms.TypeConverterAttribute).

Manchmal ist es jedoch praktisch, ein Attribut aus einer anderen Quelle festzulegen, z. B. einem Element in einem Wörterbuch, dem Wert einer statischen Eigenschaft oder eines statischen Felds oder aus einer Berechnung.

Dies ist die Aufgabe einer *XAML-Markuperweiterung*. Trotz des Namens sind XAML-Markuperweiterungen *keine* Erweiterung von XML. XAML ist immer gültiger XML-Code.

## <a name="the-code-infrastructure"></a>Die Codeinfrastruktur

Ein XAML-Markuperweiterung ist eine Klasse, die die [`IMarkupExtension`](xref:Xamarin.Forms.Xaml.IMarkupExtension)-Schnittstelle implementiert. Bei einer solchen Klasse tritt häufig das Wort `Extension` am Ende ihres Namens auf, doch normalerweise kommt es in XAML ohne dieses Suffix vor.

Die folgenden XAML-Markuperweiterungen werden von allen Implementierungen von XAML unterstützt:

- `x:Static` unterstützt von [`StaticExtension`](xref:Xamarin.Forms.Xaml.StaticExtension).
- `x:Reference` unterstützt von [`ReferenceExtension`](xref:Xamarin.Forms.Xaml.ReferenceExtension).
- `x:Type` unterstützt von [`TypeExtension`](xref:Xamarin.Forms.Xaml.TypeExtension).
- `x:Null` unterstützt von [`NullExtension`](xref:Xamarin.Forms.Xaml.NullExtension).
- `x:Array` unterstützt von [`ArrayExtension`](xref:Xamarin.Forms.Xaml.ArrayExtension).

Diese vier XAML-Markuperweiterungen werden von vielen Implementierungen von XAML unterstützt, z. B. Xamarin.Forms:

- `StaticResource` unterstützt von [`StaticResourceExtension`](xref:Xamarin.Forms.Xaml.StaticResourceExtension).
- `DynamicResource` unterstützt von [`DynamicResourceExtension`](xref:Xamarin.Forms.Xaml.DynamicResourceExtension).
- `Binding` unterstützt von [`BindingExtension`](xref:Xamarin.Forms.Xaml.BindingExtension) – besprochen in [Kapitel 16, Datenbindung](chapter16.md)
- `TemplateBinding` unterstützt von [`TemplateBindingExtension`](xref:Xamarin.Forms.Xaml.TemplateBindingExtension) – nicht behandelt in diesem Buch.

Eine zusätzliche XAML-Markuperweiterung ist in Xamarin.Forms in Verbindung mit [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) enthalten:

- [`ConstraintExpression`](xref:Xamarin.Forms.ConstraintExpression) – nicht behandelt in diesem Buch.

## <a name="accessing-static-members"></a>Zugreifen auf statische Member

Verwenden Sie das [`x:Static`](xref:Xamarin.Forms.Xaml.StaticExtension)-Element, um ein Attribut auf den Wert einer/s öffentlichen, statischen Eigenschaft, Felds oder Enumerationsmembers festzulegen. Legen Sie die Eigenschaft [`Member`](xref:Xamarin.Forms.Xaml.StaticExtension.Member) auf den statischen Member fest. Normalerweise ist es einfacher, `x:Static` und den Membernamen in geschweiften Klammern anzugeben. Der Name der `Member`-Eigenschaft muss nicht eingeschlossen werden, nur der Member selbst. Diese gängige Syntax wird im [**SharedStatics**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/SharedStatics)-Beispiel gezeigt. Die statischen Felder selbst werden in der [`AppConstants`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter10/SharedStatics/SharedStatics/SharedStatics/AppConstants.cs)-Klasse definiert. Diese Methode gestattet es Ihnen, von einem Programm verwendete Konstanten einzurichten.

Mit einer zusätzlichen XML-Namespacedeklaration können Sie auf öffentliche, statische Eigenschaften, Felder oder Enumerationsmember im .NET Framework verweisen, wie im [**SystemStatics**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/SystemStatics)-Beispiel veranschaulicht.

## <a name="resource-dictionaries"></a>Ressourcenverzeichnis

Die `VisualElement`-Klasse definiert eine Eigenschaft namens [`Resources`](xref:Xamarin.Forms.VisualElement.Resources), die Sie auf ein Objekt vom Typ [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) festlegen können. Innerhalb von XAML können Sie Elemente in diesem Verzeichnis speichern und sie mit dem `x:Key`-Attribut identifizieren. Die im Ressourcenverzeichnis gespeicherten Elemente werden von allen Verweisen auf das Element gemeinsam genutzt.

### <a name="staticresource-for-most-purposes"></a>StaticResource für die meisten Zwecke

In den meisten Fällen verwenden Sie die [`StaticResource`](xref:Xamarin.Forms.Xaml.StaticResourceExtension)-Markuperweiterung, um auf ein Element aus dem Ressourcenverzeichnis zu verweisen, wie im [**ResourceSharing**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/ResourceSharing)-Beispiel gezeigt. Sie können ein `StaticResourceExtension`-Element oder `StaticResource` in geschweiften Klammern verwenden:

[![Dreifacher Screenshot der gemeinsamen Nutzung von Ressourcen](images/ch10fg03-small.png "Gemeinsame Nutzung von Ressourcen")](images/ch10fg03-large.png#lightbox "Gemeinsame Nutzung von Ressourcen")

Verwechseln Sie die `x:Static`-Markuperweiterung nicht mit der `StaticResource`-Markuperweiterung.

### <a name="a-tree-of-dictionaries"></a>Struktur von Datenwörterbüchern

Wenn der XAML-Parser eine `StaticResource` findet, beginnt er damit, die visuelle Struktur aufwärts nach einem übereinstimmenden Schlüssel zu durchsuchen, und schlägt dann im `ResourceDictionary` in der `App`-Klasse der Anwendung nach. Hierdurch können Elemente, die sich in einem Ressourcenverzeichnis tiefer in der visuellen Struktur befinden, eine Ressource überschreiben, die sich weiter oben in der visuellen Struktur befindet. Dies wird im [**ResourceTrees**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/ResourceTrees)-Beispiel demonstriert.

### <a name="dynamicresource-for-special-purposes"></a>DynamicResource für spezielle Zwecke

Die `StaticResource`-Markuperweiterung veranlasst das Abrufen eines Elements aus dem Verzeichnis, wenn eine visuelle Struktur während des `InitializeComponent`-Aufrufs erstellt wird. Eine Alternative zu `StaticResource` ist [`DynamicResource`](xref:Xamarin.Forms.Xaml.DynamicResourceExtension), das eine Verknüpfung mit dem Verzeichnisschlüssel bewahrt und das Ziel aktualisiert, wenn das von dem Schlüssel referenzierte Element sich ändert.

Der Unterschied zwischen `StaticResource` und `DynamicResource` wird im [**DynamicVsStatic**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/DynamicVsStatic)-Beispiel veranschaulicht.

Eine von `DynamicResource` festgelegte Eigenschaft muss, wie in [Kapitel 11, „Die bindbare Infrastruktur“](chapter11.md), besprochen, von einer bindbaren Eigenschaft unterstützt werden.

## <a name="lesser-used-markup-extensions"></a>Seltener verwendete Markuperweiterungen

Verwenden Sie die [`x:Null`](xref:Xamarin.Forms.Xaml.NullExtension)-Markuperweiterung, um eine Eigenschaft auf `null` festzulegen.

Verwenden Sie die [`x:Type`](xref:Xamarin.Forms.Xaml.TypeExtension)-Markuperweiterung, um eine Eigenschaft auf ein `Type`-Objekt von .NET festzulegen.

Verwenden Sie [`x:Array`](xref:Xamarin.Forms.Xaml.ArrayExtension), um ein Array zu definieren. Geben Sie den Typ der Arraymember an, indem Sie die Eigenschaft [`Type`] auf eine `x:Type`-Markuperweiterung festlegen.

## <a name="a-custom-markup-extension"></a>Eine benutzerdefinierte Markuperweiterung

Sie können Ihre eigene XAML-Markuperweiterung erstellen, indem Sie eine Klasse schreiben, die die [`IMarkupExtension`](xref:Xamarin.Forms.Xaml.IMarkupExtension)-Schnittstelle mit einer [`ProvideValue`](xref:Xamarin.Forms.Xaml.IMarkupExtension.ProvideValue(System.IServiceProvider))-Methode implementiert.

Die [`HslColorExtension`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/HslColorExtension.cs)-Klasse erfüllt diese Anforderung. Sie erstellt einen Wert vom Typ `Color`, basierend auf Werten der Eigenschaften namens `H`, `S`, `L` und `A`. Diese Klasse ist das erste Element in einer Xamarin.Forms-Bibliothek mit dem Namen [**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit), die im Verlauf dieses Buchs aufgebaut und verwendet wird.

Das [**CustomExtensionDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/CustomExtensionDemo)-Beispiel veranschaulicht, wie Sie auf diese Bibliothek verweisen und die benutzerdefinierte Markuperweiterung verwenden.

## <a name="related-links"></a>Verwandte Links

- [Kapitel 10 – vollständiger Text (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch10-Apr2016.pdf)
- [Kapitel 10 – Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10)
- [XAML-Markuperweiterungen](~/xamarin-forms/xaml/markup-extensions/index.md)
