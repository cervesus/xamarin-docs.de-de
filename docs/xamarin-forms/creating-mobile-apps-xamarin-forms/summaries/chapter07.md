---
title: Zusammenfassung von Kapitel 7. XAML oder Code
description: 'Erstellen von mobilen Apps mit Xamarin.Forms: Zusammenfassung von Kapitel 7. XAML oder Code'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: E91F387B-CE90-481C-8D90-CB25519BFD2B
author: davidbritch
ms.author: dabritch
ms.date: 07/19/2018
ms.openlocfilehash: ce4dde3716176daf826678809339afb84c25d84a
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2020
ms.locfileid: "61334741"
---
# <a name="summary-of-chapter-7-xaml-vs-code"></a>Zusammenfassung von Kapitel 7. XAML oder Code

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07)

> [!NOTE]
> In den Anmerkungen auf dieser Seite wird erläutert, inwiefern die Angaben innerhalb des Buchs heute nicht mehr für Xamarin.Forms gelten.

Xamarin.Forms unterstützt eine XAML-basierte Markupsprache namens „Extensible Application Markup Language“, oder XAML („xsämmel“ ausgesprochen). XAML bietet eine Alternative zu C# beim Definieren des Layouts der Benutzeroberfläche einer Xamarin.Forms-Anwendung und beim Definieren von Bindungen zwischen Benutzeroberflächenelementen und zugrunde liegenden Daten.

## <a name="properties-and-attributes"></a>Eigenschaften und Attribute

Xamarin.Forms-Klassen und -Strukturen werden in XAML zu XML-Elementen, und Eigenschaften dieser Klassen und Strukturen werden zu XML-Attributen. Um in XAML instanziiert zu werden, muss eine Klasse generell einen öffentlichen, parameterlosen Konstruktor aufweisen. Alle in XAML festgelegten Eigenschaften müssen über öffentliche `set`-Zugriffsmethoden verfügen.

Für Eigenschaften der grundlegenden Datentypen (`string`, `double`, `bool` usw.) verwendet der XAML-Parser die `TryParse`-Standardmethoden, um Attributeinstellungen in diese Typen zu konvertieren. Der XAML-Parser kann auch Enumerationstypen ganz einfach verarbeiten, und er kann Enumerationsmember kombinieren, wenn der Enumerationstyp mit dem `Flags`-Attribut gekennzeichnet ist.

Zur Unterstützung des XAML-Parsers können komplexere Typen (oder Eigenschaften dieser Typen) ein [`TypeConverterAttribute`](xref:Xamarin.Forms.TypeConverterAttribute) enthalten, das eine Klasse identifiziert, die sich von [`TypeConverter`](xref:Xamarin.Forms.TypeConverter) ableitet, der die Konvertierung von Zeichenfolgenwerten in diese Typen unterstützt. Beispielsweise konvertiert [`ColorTypeConverter`](xref:Xamarin.Forms.ColorTypeConverter) Farbnamen und -zeichenfolgen wie „#rrggbb“ in `Color`-Werte.

## <a name="property-element-syntax"></a>Syntax von Eigenschaftselementen

In XAML werden Klassen und die aus diesen erstellten Objekte als XML-Elemente ausgedrückt. Diese werden als *Objektelemente* bezeichnet. Die meisten Eigenschaften dieser Objekte werden als XML-Attribute ausgedrückt. Diese werden als *Eigenschaftsattribute* bezeichnet.

Manchmal muss eine Eigenschaft auf ein Objekt festgelegt werden, das sich nicht als einfache Zeichenfolge ausdrücken lässt. In solch einem Fall unterstützt XAML ein Tag namens *Eigenschaftselement*, das aus dem Klassennamen und Eigenschaftsnamen, getrennt durch einen Punkt, besteht. Ein Objektelement kann dann in einem Paar aus Eigenschaftselement-Tags vorkommen.

## <a name="adding-a-xaml-page-to-your-project"></a>Hinzufügen einer XAML-Seite zu Ihrem Projekt

Eine portable Xamarin.Forms-Klassenbibliothek (Portable Class Library, PCL) kann bei ihrer ersten Erstellung eine XAML-Seite enthalten, oder Sie können einem vorhandenen Projekt eine XAML-Seite hinzufügen. In dem Dialogfeld zum Hinzufügen eines neuen Elements wählen Sie das Element aus, das auf eine XAML-Seite verweist, oder `ContentPage` und XAML. (Keine `ContentView`.)

> [!NOTE]
> Visual Studio-Optionen haben sich seit Verfassung dieses Kapitels geändert.

Zwei Dateien werden erstellt: eine XAML-Datei mit der Dateinamenerweiterung „.xaml“ und eine C#-Datei mit der Erweiterung „.xaml.cs“. Die C#-Datei wird häufig als *CodeBehind* der XAML-Datei bezeichnet. Die CodeBehind-Datei ist eine partielle Klassendefinition, die von `ContentPage` abgeleitet wird. Zur Buildzeit wird der XAML-Code analysiert, und es wird eine andere partielle Klassendefinition für dieselbe Klasse generiert. Diese generierte Klasse enthält eine Methode namens `InitializeComponent`, die vom Konstruktor der CodeBehind-Datei aufgerufen wird.

Während der Laufzeit, am Ende des `InitializeComponent`-Aufrufs, sind alle Elemente der XAML-Datei instanziiert und initialisiert, genau so, als ob Sie in C#-Code erstellt worden wären.

Das Stammelement in der XAML-Datei ist `ContentPage`. Das Stammtag enthält mindestens zwei XML-Namespacedeklarationen, eine für die Xamarin.Forms-Elemente und eine andere, die ein `x`-Präfix für Elemente und Attribute definiert, die für alle XAML-Implementierungen intrinsisch sind. Das Stammtag enthält außerdem ein `x:Class`-Attribut, das den Namespace und den Namen der Klasse angibt, die von `ContentPage` abgeleitet wird. Dies entspricht dem Namespace und Klassennamen in der CodeBehind-Datei.

Die Kombination aus XAML und Code wird im [**CodePlusXaml**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07)-Beispiel veranschaulicht.

## <a name="the-xaml-compiler"></a>Der XAML-Compiler

Xamarin.Forms besitzt einen XAML-Compiler, dessen Verwendung aber optional ist, abhängig von der Verwendung eines [`XamlCompilationAttribute`](xref:Xamarin.Forms.Xaml.XamlCompilationAttribute). Wenn der XAML-Code nicht kompiliert ist, wird er zur Buildzeit analysiert, und die XAML-Datei wird in die PCL eingebettet, wo sie ebenfalls zur Laufzeit analysiert wird. Wenn der XAML-Code kompiliert ist, konvertiert der Buildprozess den XAML-Code in ein Binärformat, wodurch die Laufzeitverarbeitung effizienter wird.

## <a name="platform-specificity-in-the-xaml-file"></a>Plattformabhängigkeit in der XAML-.Datei

In XAML kann die [`OnPlatform`](xref:Xamarin.Forms.OnPlatform`1)-Klasse verwendet werden, um plattformabhängiges Markup auszuwählen. Dies ist eine generische Klasse, und sie muss mit einem `x:TypeArguments`-Attribut instanziiert werden, das dem Zieltyp entspricht. Die [`OnIdiom`](xref:Xamarin.Forms.OnIdiom`1)-Klasse ist ähnlich, wird aber wesentlich seltener verwendet.

Die Verwendung von `OnPlatform` hat sich seit der Veröffentlichung dieses Buchs geändert. Ursprünglich wurde es zusammen mit Eigenschaften namens `iOS`, `Android` und `WinPhone` verwendet. Jetzt wird es mit untergeordneten [`On`](xref:Xamarin.Forms.On)-Objekten verwendet. Legen Sie die [`Platform`](xref:Xamarin.Forms.On.Platform)-Eigenschaft auf eine Zeichenfolge fest, die mit den öffentlichen `const`-Feldern der [`Device`](xref:Xamarin.Forms.Device)-Klasse konsistent ist. Legen Sie die [`Value`](xref:Xamarin.Forms.On.Value)-Eigenschaft auf einen Wert fest, die mit dem `x:TypeArguments`-Attribut des `OnPlatform`-Tags konsistent ist.

`OnPlatform` wird im [**ScaryColorList**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07/ScaryColorList)-Beispiel veranschaulicht, das deshalb so heißt, weil es Blöcke fast identischen XAML-Codes enthält. Das Vorhandensein dieses sich wiederholenden Markups legt nahe, dass Methoden vorhanden sein sollten, um dies zu verringern.

## <a name="the-content-property-attributes"></a>Die Inhaltseigenschaftsattribute

Einige Eigenschaftselemente treten ziemlich häufig auf, z. B. das `<ContentPage.Content>`-Tag beim Stammelement einer `ContentPage` oder das `<StackLayout.Children>`-Tag, das die untergeordneten Elemente von `StackLayout` umschließt.

Jede Klasse darf eine Eigenschaft mit einem [`ContentPropertyAttribute`](xref:Xamarin.Forms.ContentPropertyAttribute) der Klasse identifizieren. Für diese Eigenschaft sind die Eigenschaftselement-Tags nicht erforderlich. `ContentPage` definiert seine Inhaltseigenschaft als `Content`, und `Layout<T>` (die Klasse, von der `StackLayout` abgeleitet wird) definiert seine Inhaltseigenschaft als `Children`. Diese Eigenschaftselement-Tags sind nicht erforderlich.

Das Eigenschaftselement von `Label` ist `Text`.

## <a name="formatted-text"></a>Formatierter Text

Das [**TextVariations**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07/TextVariations)-Beispiel enthält mehrere Beispiele für das Festlegen der Eigenschaften `Text` und `FormattedText` von `Label`. In XAML werden `Span`-Objekte als untergeordnete Elemente des `FormattedString`-Objekts angezeigt.

 Wenn eine mehrzeilige Zeichenfolge auf die `Text`-Eigenschaft festgelegt wird, werden Zeilenendezeichen in Leerzeichen konvertiert, aber die Zeilenendezeichen bleiben erhalten, wenn eine mehrzeilige Zeichenfolge als Inhalt der Tags `Label` oder `Label.Text` angezeigt wird:

 [![Dreifacher Screenshot der gemeinsamen Nutzung von Textvariationen](images/ch07fg03-small.png "Variationen formatierten Texts")](images/ch07fg03-large.png#lightbox "Variationen formatierten Texts")

## <a name="related-links"></a>Verwandte Links

- [Kapitel 7 – vollständiger Text (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch07-Apr2016.pdf)
- [Kapitel 7 – Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07)
- [Kapitel 7 – F#-Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07/FS/CodePlusXaml)
- [XAML-Grundlagen](~/xamarin-forms/xaml/xaml-basics/index.md)
