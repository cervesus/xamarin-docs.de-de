---
title: eXtensible Application Markup Language (XAML)
description: XAML ist eine deklarative Markupsprache, die zum Definieren von Benutzeroberflächen verwendet werden kann. Die Benutzeroberfläche wird definiert, in einer XML-Datei die XAML-Syntax, während das Laufzeitverhalten in einer separaten CodeBehind-Datei definiert ist.
ms.prod: xamarin
ms.assetid: CD30EECC-8AC1-4CF5-A4FE-348420A6231E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/28/2019
ms.openlocfilehash: fa93cb86867cb8539fb7ce4db45ad4751bfe6e04
ms.sourcegitcommit: c4be32ef914465e808d89767c4d5ee72afe93cc6
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/02/2019
ms.locfileid: "58854223"
---
# <a name="extensible-application-markup-language-xaml"></a>eXtensible Application Markup Language (XAML)

_XAML ist eine deklarative Markupsprache, die zum Definieren von Benutzeroberflächen verwendet werden kann. Die Benutzeroberfläche wird definiert, in einer XML-Datei die XAML-Syntax, während das Laufzeitverhalten in einer separaten CodeBehind-Datei definiert ist._

> [!VIDEO https://youtube.com/embed/H6UOrSyhTEE]

**Weiterentwicklung 2016: Immer ein XAML-Master**

> [!NOTE]
> Testen Sie die [XAML-Standard (Vorschau)](standard/index.md)

<a name="xaml" />

## [<a name="xaml-basics"></a>XAML-Grundlagen](xaml-basics/index.md)

XAML ermöglicht Entwicklern, Benutzeroberflächen in Xamarin.Forms-Anwendungen mit Markup statt Code zu definieren. XAML ist nie in einer Xamarin.Forms-Anwendung erforderlich, aber dabei handelt es sich lässt, häufig ist es visuell kohärente und kompaktere als der entsprechende Code. XAML eignet sich besonders gut für die Verwendung mit der beliebten Anwendungsarchitektur von Model-View-ViewModel (MVVM): XAML definiert die Sicht, die über datenbindungen, die XAML-basierte ViewModel-Code verknüpft ist.

## [<a name="xaml-compilation"></a>XAML-Kompilierung](xamlc.md)

XAML kann optional auch direkt mit dem XAML-Compiler (XAMLC) in der Zwischensprache (Intermediate Language, IL) kompiliert werden. In diesem Artikel wird beschrieben, wie XAMLC und seinen Vorteilen verwendet wird.

## [<a name="xaml-previewer"></a>XAML-Vorschau](xaml-previewer/index.md)

Die [XAML-Vorschau](~/xamarin-forms/xaml/xaml-previewer/index.md) rendert eine Livevorschau der einen Seite Seite-an-Seite-mit dem XAML-Markup, sodass Sie Ihre Benutzeroberfläche gerendert wird, während der Eingabe angezeigt.

## [<a name="xaml-namespaces"></a>XAML-Namespaces](namespaces.md)

XAML verwendet das `xmlns` XML-Attribut für Namespacedeklarationen. In diesem Artikel stellt die Syntax des XAML-Namespace, und es wird veranschaulicht, wie einen XAML-Namespace auf einen Typ deklariert.

## [<a name="xaml-custom-namespace-schemas"></a>XAML-Namespace mit benutzerdefinierten Schemas](custom-namespace-schemas.md)

Ein XAML-Schema benutzerdefinierten Namespace definiert werden kann, mit der `XmlnsDefinitionAttribute` -Klasse, die eine Zuordnung zwischen einer benutzerdefinierten URL und einen oder mehrere CLR-Namespaces angibt. Das Schema des benutzerdefinierten Namespace können Sie dann in XAML-Namespace-Deklarationen verwendet werden.

## [<a name="xaml-namespace-recommended-prefixes"></a>XAML-Namespace, die Präfixe empfohlen](custom-prefix.md)

Die `XmlnsPrefixAttribute` Klasse kann von Autoren von Steuerelementen verwendet werden, an der ein empfohlenes Präfix, einen XAML-Namespace, für die XAML-Verwendung zugeordnet werden soll.

## [<a name="xaml-markup-extensions"></a>XAML-Markuperweiterungen](markup-extensions/index.md)

XAML umfasst Markuperweiterungen für das Festlegen von Attributen auf Werte oder Objekte außerhalb, einfache Zeichenfolgen ausgedrückt werden kann. Dazu gehören das Verweisen auf Konstanten, statische Eigenschaften und Felder, Ressourcenverzeichnisse und datenbindungen.

## [<a name="field-modifiers"></a>Feldmodifizierern](field-modifiers.md)

Die `x:FieldModifier` Namespace-Attribut gibt die Zugriffsebene für generierte Felder für benannte XAML-Elemente.

## [<a name="passing-arguments"></a>Übergeben von Argumenten](passing-arguments.md)

XAML kann verwendet werden, um Argumente zu übergeben, um nicht standardmäßige Konstruktoren oder Factorymethoden. In diesem Artikel wird veranschaulicht, wie die XAML-Attribute, die verwendet werden können, um Argumente an die Konstruktoren, die zum Aufrufen der Factorymethoden, und geben Sie ein generisches Argument übergeben.

## [<a name="bindable-properties"></a>Bindbare Eigenschaften](bindable-properties.md)

In Xamarin.Forms wird die Funktionalität der common Language Runtime (CLR)-Eigenschaften von bindbare Eigenschaften erweitert. Eine bindbare Eigenschaft ist eine besondere Art von Eigenschaft, in denen der Wert der Eigenschaft wird vom Eigenschaftensystem Xamarin.Forms nachverfolgt. Dieser Artikel bietet eine Einführung in bindbare Eigenschaften und veranschaulicht das Erstellen und deren Nutzung.

## [<a name="attached-properties"></a>Angefügte Eigenschaften](attached-properties.md)

Eine angefügte Eigenschaft ist eine besondere Art von bindbare Eigenschaft in einer Klasse definiert, aber mit anderen Objekten verbunden und in XAML als Attribut erkannt, die eine Klasse enthält, und ein Eigenschaftennamen an, die durch einen Punkt getrennt. Dieser Artikel bietet eine Einführung in angefügte Eigenschaften und veranschaulicht das Erstellen und deren Nutzung.

## [<a name="resource-dictionaries"></a>Ressourcenverzeichnisse](resource-dictionaries.md)

XAML-Ressourcen sind Definitionen der Objekte, die mehr als einmal verwendet werden können. Ein [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) können Ressourcen in einem zentralen Ort definiert und in einer Xamarin.Forms-Anwendung erneut verwendet werden. In diesem Artikel wird veranschaulicht, wie das Erstellen und nutzen einen `ResourceDictionary`, und das Zusammenführen `ResourceDictionary` in eine andere.

## [<a name="loading-xaml-at-runtime"></a>Laden von XAML zur Laufzeit](runtime-load.md)

XAML geladen und zur Laufzeit mit analysiert werden kann, die [ `LoadFromXaml` ](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*) Erweiterungsmethoden.
