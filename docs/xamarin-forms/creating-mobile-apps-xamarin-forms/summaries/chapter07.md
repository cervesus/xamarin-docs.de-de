---
title: Zusammenfassung der Kapitel 7. XAML und code
description: 'Erstellen von mobilen Apps mit Xamarin.Forms: Zusammenfassung der Kapitel 7. XAML und code'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: E91F387B-CE90-481C-8D90-CB25519BFD2B
author: davidbritch
ms.author: dabritch
ms.date: 07/19/2018
ms.openlocfilehash: ce4dde3716176daf826678809339afb84c25d84a
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61334741"
---
# <a name="summary-of-chapter-7-xaml-vs-code"></a>Zusammenfassung der Kapitel 7. XAML und code

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07)

> [!NOTE]
> Anmerkungen zu dieser Version auf dieser Seite Geben Sie Bereiche, in denen Xamarin.Forms aus den Informationen im Buch abweichend hat, an.

Xamarin.Forms unterstützt eine XML-basierte Markupsprache Namens der Extensible Application Markup Language oder XAML ("xsämmel"). XAML bietet eine Alternative zur C#-definieren Sie das Layout der Benutzeroberfläche einer Xamarin.Forms-Anwendung, und klicken Sie im Definieren der Bindungen zwischen Elementen der Benutzeroberfläche und dem zugrunde liegenden Daten.

## <a name="properties-and-attributes"></a>Eigenschaften und Attribute

Xamarin.Forms-Klassen und Strukturen werden XML-Elemente in XAML aus, und die Eigenschaften dieser Klassen und Strukturen werden XML-Attribute. Um in XAML instanziiert werden, muss eine Klasse in der Regel einen öffentlichen parameterlosen Konstruktor verfügen. Alle Eigenschaften in XAML festlegen müssen öffentliche `set` Accessoren.

Für die Eigenschaften der basic-Datentypen (`string`, `double`, `bool`usw.), der XAML-Parser verwendet den Standard `TryParse` Methoden, um die attributeinstellungen für diese Typen zu konvertieren. Der XAML-Parser kann auch auf einfache Weise behandeln, Enumerationstypen, und es kann Enumerationsmember kombinieren, wenn der Enumerationstyp gekennzeichnet ist, mit der `Flags` Attribut.

Um den XAML-Parser zu unterstützen, komplexe Typen (oder Eigenschaften dieser Typen) zählen eine [ `TypeConverterAttribute` ](xref:Xamarin.Forms.TypeConverterAttribute) , die eine abgeleitete Klasse identifiziert [ `TypeConverter` ](xref:Xamarin.Forms.TypeConverter) unterstützt die Konvertierung von String-Werte, die diese Typen. Z. B. die [ `ColorTypeConverter` ](xref:Xamarin.Forms.ColorTypeConverter) konvertiert color-Namen und Zeichenfolgen, z. B. "#rrggbb", in `Color` Werte.

## <a name="property-element-syntax"></a>Eigenschaftenelement syntax

In XAML werden die Klassen und den von ihnen erstellten Objekten als XML-Elemente ausgedrückt. Diese werden als bezeichnet *Objektelemente*. Die meisten Eigenschaften dieser Objekte werden als XML-Attributen ausgedrückt. Diese heißen *Eigenschaftenattribute*.

Manchmal muss eine Eigenschaft auf ein Objekt festgelegt werden, die als einfache Zeichenfolge ausgedrückt werden kann. In diesem Fall unterstützt XAML ein Tag Namens eine *Property-Element* , der aus dem Klassennamen und den Eigenschaftennamen, die durch einen Punkt getrennt. Ein Objektelement kann dann innerhalb eines Paares von Eigenschaftenelement Tags angezeigt werden.

## <a name="adding-a-xaml-page-to-your-project"></a>Hinzufügen einer XAML-Seite zu Ihrem Projekt

Eine Xamarin.Forms Portable Class Library kann eine XAML-Seite enthalten, bei seiner ersten Erstellung, oder Sie können eine XAML-Seite zu einem vorhandenen Projekt hinzufügen. Wählen Sie im Dialogfeld zum Hinzufügen eines neuen Elements das Element, das auf einer XAML-Seite verweist oder `ContentPage` und XAML. (Keinen `ContentView`.)

> [!NOTE]
> Visual Studio-Optionen wurden geändert, da in diesem Kapitel geschrieben wurde.

Es werden zwei Dateien erstellt: eine XAML-Datei mit dem Dateinamen Erweiterung .xaml und einer C#-Datei mit der Erweiterung. "XAML.cs" ausgedrückt. Die C#-Datei wird häufig als bezeichnet die *CodeBehind* der XAML-Datei. Die Code-Behind-Datei ist eine partielle Klassendefinition, die von abgeleitet `ContentPage`. Zur Buildzeit das XAML analysiert wird, und einer anderen partiellen Klassendefinition für die gleiche Klasse generiert. Die generierte Klasse enthält eine Methode namens `InitializeComponent` , die aus dem Konstruktor der CodeBehind-Datei aufgerufen wird.

Während der Laufzeit am Ende der `InitializeComponent` aufrufen, die alle Elemente der XAML-Datei instanziiert und initialisiert werden, als ob sie in C#-Code erstellt wurde wurden.

Das Stammelement in der XAML-Datei ist `ContentPage`. Das Stamm-Tag enthält mindestens zwei XML-Namespacedeklarationen, eine für die Xamarin.Forms-Elemente und die andere zum Definieren einer `x` Präfix für Elemente und Attribute, die für alle XAML-Implementierungen systemintern. Das Stamm-Tag enthält auch eine `x:Class` Attribut, das angibt, dem Namespace und Namen der Klasse, die abgeleitet `ContentPage`. Dies entspricht der Namespace und den Namen in der CodeBehind-Datei.

Die Kombination von XAML und Code wird veranschaulicht, durch die [ **CodePlusXaml** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07) Beispiel.

## <a name="the-xaml-compiler"></a>Der XAML-compiler

Xamarin.Forms verfügt über einen XAML-Compiler, ihre Verwendung ist jedoch optional basierend auf der Verwendung von einem [ `XamlCompilationAttribute` ](xref:Xamarin.Forms.Xaml.XamlCompilationAttribute). Wenn die XAML nicht kompiliert wird, zum Zeitpunkt der Erstellung der XAML analysiert wird und die XAML-Datei eingebettet ist, in der PCL, wo sie auch zur Laufzeit analysiert wird. Wenn die XAML kompiliert wird, der Buildprozess konvertiert die XAML in ein Binärformat und die Common Language Runtime-Verarbeitung ist effizienter.

## <a name="platform-specificity-in-the-xaml-file"></a>Plattform detailgenauigkeit in der XAML-Datei

In XAML die [ `OnPlatform` ](xref:Xamarin.Forms.OnPlatform`1) Klasse kann verwendet werden, um plattformabhängige Markup auswählen. Dies ist eine generische Klasse, und muss instanziiert werden, mit einem `x:TypeArguments` -Attribut, das mit dem Zieltyp übereinstimmt. Die [ `OnIdiom` ](xref:Xamarin.Forms.OnIdiom`1) Klasse ist ähnlich, aber verwendet immer seltener.

Die Verwendung von `OnPlatform` wurde geändert, seit das Buch veröffentlicht wurde. Wurde ursprünglich verwendet in Verbindung mit den Eigenschaften `iOS`, `Android`, und `WinPhone`. Es wird jetzt mit untergeordneten verwendet [ `On` ](xref:Xamarin.Forms.On) Objekte. Legen Sie die [ `Platform` ](xref:Xamarin.Forms.On.Platform) Eigenschaft in eine Zeichenfolge, die konsistent mit den öffentlichen `const` Felder der [ `Device` ](xref:Xamarin.Forms.Device) Klasse. Legen Sie die [ `Value` ](xref:Xamarin.Forms.On.Value) Eigenschaft mit einem Wert, der konsistent mit der `x:TypeArguments` Attribut der `OnPlatform` Tag.

`OnPlatform` wird veranschaulicht, der [ **ScaryColorList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07/ScaryColorList) Beispiel so genannt, da es sich um Blöcke von nahezu identische XAML enthält. Das Vorhandensein dieses wiederkehrende Markup legt nahe, dass es sich bei Techniken zur Verfügung, um es zu reduzieren werden soll.

## <a name="the-content-property-attributes"></a>Die Attribute der Content-Eigenschaft

Einige Eigenschaftenelemente relativ häufig auftreten, z. B. die `<ContentPage.Content>` tag für das Stammelement des eine `ContentPage`, oder die `<StackLayout.Children>` Tag, das die untergeordneten Elemente des einschließt `StackLayout`.

Jede Klasse kann zum Identifizieren einer Eigenschaft mit einem [ `ContentPropertyAttribute` ](xref:Xamarin.Forms.ContentPropertyAttribute) für die Klasse. Die Eigenschaft-Element-Tags sind für diese Eigenschaft nicht erforderlich. `ContentPage` definiert die Inhaltseigenschaft als `Content`, und `Layout<T>` (Klasse, aus der `StackLayout` abgeleitet ist) definiert die Inhaltseigenschaft als `Children`. Diese Eigenschaftenelement-Tags sind nicht erforderlich.

Der Property-Element der `Label` ist `Text`.

## <a name="formatted-text"></a>Formatierter Text

Die [ **TextVariations** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07/TextVariations) Beispiel enthält einige Beispiele für die Einstellung der `Text` und `FormattedText` Eigenschaften `Label`. In XAML `Span` Objekte werden als untergeordnete Elemente von der `FormattedString` Objekt.

 Wenn eine mehrzeilige Zeichenfolge festgelegt ist, um die `Text` -Eigenschaft, End-of-Line-Zeichen werden in Leerzeichen konvertiert, aber die End-of-Line-Zeichen werden beibehalten, wenn eine mehrzeilige Zeichenfolge als Inhalt angezeigt wird der `Label` oder `Label.Text` Tags:

 [![Dreifacher Screenshot des Text-Varianten, die gemeinsame Nutzung](images/ch07fg03-small.png "formatierten Text Variationen")](images/ch07fg03-large.png#lightbox "Variationen für formatierten Text")

## <a name="related-links"></a>Verwandte Links

- [Kapitel 7 Volltext (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch07-Apr2016.pdf)
- [Kapitel 7-Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07)
- [Kapitel 7 F# Beispiel](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07/FS/CodePlusXaml)
- [XAML-Grundlagen](~/xamarin-forms/xaml/xaml-basics/index.md)
