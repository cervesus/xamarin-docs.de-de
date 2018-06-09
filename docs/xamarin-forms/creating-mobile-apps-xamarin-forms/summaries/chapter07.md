---
title: Zusammenfassung der Kapitel 7. XAML und code
description: 'Beim Erstellen mobiler Apps mit Xamarin.Forms: Zusammenfassung der Kapitel 7. XAML und code'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: E91F387B-CE90-481C-8D90-CB25519BFD2B
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: efa3a22c12983ef742bb46f91ab6096294cdc533
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/08/2018
ms.locfileid: "35241447"
---
# <a name="summary-of-chapter-7-xaml-vs-code"></a>Zusammenfassung der Kapitel 7. XAML und code

Xamarin.Forms unterstützt eine XML-basierte Markupsprache, die die Extensible Application Markup Language aufgerufen oder die Verwendung von XAML-("xsämmel"). XAML stellt eine Alternative zum C#-in definiert das Layout der Benutzeroberfläche einer Xamarin.Forms-Anwendung, und definieren Bindungen zwischen Elementen der Benutzeroberfläche und des zugrunde liegenden Daten.

## <a name="properties-and-attributes"></a>Eigenschaften und Attribute

Xamarin.Forms-Klassen und Strukturen werden XML-Elemente in XAML, und Eigenschaften dieser Klassen und Strukturen werden XML-Attribute. Um in XAML instanziiert zu werden, muss eine Klasse in der Regel einen öffentlichen parameterlosen Konstruktor haben. Alle Eigenschaften in XAML festgelegt benötigen öffentliche `set` Accessoren.

Für die Eigenschaften der grundlegenden Datentypen (`string`, `double`, `bool`usw.), XAML-Parser verwendet die Standard `TryParse` Methoden attributeinstellungen in diese Datentypen konvertiert. XAML-Parser kann auch problemlos bewältigt werden Enumerationstypen und es kann Enumerationsmember kombiniert werden, wenn der Enumerationstyp gekennzeichnet ist, mit der `Flags` Attribut.

Zur Unterstützung von XAML-Parser komplexer Typen (oder die Eigenschaften dieser Typen) können enthalten eine [ `TypeConverterAttribute` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TypeConverterAttribute/) , identifiziert eine Klasse, die abgeleitet [ `TypeConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TypeConverter/) diese unterstützt die Konvertierung von Zeichenfolgenwerte für diese Typen. Z. B. die [ `ColorTypeConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ColorTypeConverter/) konvertiert Farbe Namen und Zeichenfolgen, z. B. "#rrggbb", in `Color` Werte.

## <a name="property-element-syntax"></a>Eigenschaftselement syntax

In XAML werden Klassen und die von ihnen erstellten Objekte als XML-Elemente ausgedrückt werden. Diese werden als bezeichnet *Objektelemente*. Die meisten Eigenschaften dieser Objekte werden als XML-Attribute angegeben. Diese heißen *Eigenschaftenattribute*.

In einigen Fällen muss eine Eigenschaft für ein Objekt festgelegt werden, die als eine einfache Zeichenfolge ausgedrückt werden kann. In einem solchen Fall XAML unterstützt ein Tag Namens eine *Eigenschaftselement* , den Klassennamen und den Eigenschaftennamen, die durch einen Punkt getrennt besteht. Ein Object-Element kann dann in ein Paar von Property-Element Tags angezeigt werden.

## <a name="adding-a-xaml-page-to-your-project"></a>Dem Projekt hinzufügen eine XAML-Seite

Einer portablen Klassenbibliothek mit Xamarin.Forms können eine XAML-Seite enthalten, wenn der Dienst erstmalig erstellt wird, oder Sie können einem vorhandenen Projekt eine XAML-Seite hinzufügen. Wählen Sie im Dialogfeld zum Hinzufügen eines neuen Elements, das Element, das auf eine XAML-Seite verweist oder `ContentPage` und XAML. (Keine `ContentView`.)

Es werden zwei Dateien erstellt: einer XAML-Datei mit der Filename-Erweiterung .xaml und einer C#-Datei mit der Erweiterung. xaml.cs. Die C#-Datei wird häufig als bezeichnet den *CodeBehind* der Verwendung von XAML-Datei. Die Code-Behind-Datei ist eine partielle Klassendefinition, die abgeleitet `ContentPage`. Beim Erstellen der XAML-Code analysiert, und eine andere Definition für die partielle Klasse für die gleiche Klasse generiert wird. Diese generierten Klasse enthält eine Methode namens `InitializeComponent` , die vom Konstruktor der CodeBehind-Datei aufgerufen wird.

Während der Laufzeit, am Ende der `InitializeComponent` aufrufen, die alle Elemente der Verwendung von XAML-Datei instanziiert und initialisiert werden, als ob sie im C#-Code erstellt wurden, hatten wurden.

Das Stammelement in der XAML-Datei ist `ContentPage`. Das Stamm-Tag enthält mindestens zwei XML-Namespace deklarieren eine für die Xamarin.Forms-Elemente und das andere definieren eine `x` -Präfix für die Elemente und Attribute für alle XAML-Implementierungen. Das Stamm-Tag enthält auch eine `x:Class` Attribut, das angibt, dem Namespace und Namen der Klasse, die abgeleitet `ContentPage`. Dies entspricht den Namen Namespace- und Klassennamen in der Code-Behind-Datei.

Die Kombination von XAML und generierter Code wird veranschaulicht, durch die [ **CodePlusXaml** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07) Beispiel.

## <a name="the-xaml-compiler"></a>Der XAML-compiler

Xamarin.Forms verfügt über einen XAML-Compiler, dessen Verwendung ist jedoch optional basierend auf die Verwendung von einem [ `XamlCompilationAttribute` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.XamlCompilationAttribute/). Wenn der XAML-Code nicht kompiliert wird, der XAML-Code wird während des Buildvorgangs analysiert, und die XAML-Datei in der PCL, in dem sie auch zur Laufzeit analysiert wird eingebettet ist. Wenn der XAML-Code kompiliert wird, während des Erstellungsprozesses konvertiert des XAML-Codes in ein Binärformat, und die Common Language Runtime-Verarbeitung ist effizienter.

## <a name="platform-specificity-in-the-xaml-file"></a>Besonderheit der Plattform in der XAML-Datei

In XAML wird die [ `OnPlatform` ](https://developer.xamarin.com/api/type/Xamarin.Forms.OnPlatform%3CT%3E/) Klasse kann verwendet werden, um plattformabhängige Markup auswählen. Dies ist eine generische Klasse und instanziiert werden müssen, mit einer `x:TypeArguments` Attribut, das den Zieltyp entspricht. Die [ `OnIdiom` ](https://developer.xamarin.com/api/type/Xamarin.Forms.OnIdiom%3CT%3E/) Klasse ist ähnlich, aber verwendet immer seltener.

Die Verwendung von `OnPlatform` wurde geändert, seit das Buch veröffentlicht wurde. Wurde ursprünglich verwendet in Verbindung mit Eigenschaften, die mit dem Namen `iOS`, `Android`, und `WinPhone`. Wird jetzt mit der untergeordneten verwendet [ `On` ](https://developer.xamarin.com/api/type/Xamarin.Forms.On/) Objekte. Festlegen der [ `Platform` ](https://developer.xamarin.com/api/property/Xamarin.Forms.On.Platform/) in eine Zeichenfolge, die konsistent mit der öffentlichen Eigenschaft `const` Felder von der [ `Device` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Device/) Klasse. Legen Sie die [ `Value` ](https://developer.xamarin.com/api/property/Xamarin.Forms.On.Value/) Eigenschaft mit einem Wert, der konsistent mit der `x:TypeArguments` Attribut des der `OnPlatform` Tag.

`OnPlatform` wird veranschaulicht, der [ **ScaryColorList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07/ScaryColorList) Beispiel so aufgerufen werden, da sie fast identisch XAML-Blöcken enthält. Das Vorhandensein dieses sich wiederholende Markup wird vorgeschlagen, Techniken, diesen zu verringern verfügbar sein sollen.

## <a name="the-content-property-attributes"></a>Die Inhaltseigenschaft-Attribute

Einige Eigenschaftenelemente relativ häufig auftreten, wie z. B. der `<ContentPage.Content>` tag, wenn darauf das Stammelement einer `ContentPage`, oder die `<StackLayout.Children>` Tag, das die untergeordneten Elemente enthält `StackLayout`.

Jede Klasse ist zulässig, bezeichnen eine Eigenschaft mit einem [ `ContentPropertyAttribute` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPropertyAttribute/) für die Klasse. Die Property-Element Tags sind für diese Eigenschaft nicht erforderlich. `ContentPage` der Content-Eigenschaft als definiert `Content`, und `Layout<T>` (Klasse, aus der `StackLayout` abgeleitet) definiert, der Content-Eigenschaft als `Children`. Diese Eigenschaft Element-Tags sind nicht erforderlich.

Das Eigenschaftselement von `Label` ist `Text`.

## <a name="formatted-text"></a>Formatierter Text

Die [ **TextVariations** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07/TextVariations) Beispiel enthält mehrere Beispiele für die Einstellung der `Text` und `FormattedText` Eigenschaften des `Label`. In XAML `Span` Objekte werden als untergeordnete Elemente von der `FormattedString` Objekt.

 Wenn eine mehrzeilige Zeichenfolge festgelegt wird, um die `Text` -Eigenschaft End-of-Line-Zeichen werden in Leerzeichen konvertiert, aber die End-of-Line-Zeichen werden beibehalten, wenn eine mehrzeilige Zeichenfolge als Inhalt angezeigt wird der `Label` oder `Label.Text` Tags:

 [![Dreifacher Screenshot Text Variationen Freigabe](images/ch07fg03-small.png "formatierten Text Variationen")](images/ch07fg03-large.png#lightbox "formatierten Text Varianten")



## <a name="related-links"></a>Verwandte Links

- [Kapitel 7 Volltext (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch07-Apr2016.pdf)
- [Kapitel 7-Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07)
- [Kapitel 7 f#-Beispiel](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07/FS/CodePlusXaml)
