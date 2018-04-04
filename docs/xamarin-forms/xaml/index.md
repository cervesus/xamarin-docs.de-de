---
title: eXtensible Application Markup Language (XAML)
description: XAML ist eine deklarative Markupsprache, die zum Definieren von Benutzeroberflächen verwendet werden kann. Die Benutzeroberfläche wird definiert, in eine XML-Datei, die die Verwendung von XAML-Syntax verwenden, während das Laufzeitverhalten in eine separate Code-Behind-Datei definiert ist.
ms.prod: xamarin
ms.assetid: CD30EECC-8AC1-4CF5-A4FE-348420A6231E
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 10/24/2016
ms.openlocfilehash: bb3b4c4f80171f676e8b5f9a7464f4da890a4643
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="extensible-application-markup-language-xaml"></a>eXtensible Application Markup Language (XAML)

_XAML ist eine deklarative Markupsprache, die zum Definieren von Benutzeroberflächen verwendet werden kann. Die Benutzeroberfläche wird definiert, in eine XML-Datei, die die Verwendung von XAML-Syntax verwenden, während das Laufzeitverhalten in eine separate Code-Behind-Datei definiert ist._

> [!VIDEO https://youtube.com/embed/H6UOrSyhTEE]

**Weiterentwickelt 2016: Einem Masterauftrag für den XAML-zunehmend**

> [!NOTE]
> Probieren Sie die [XAML-Standard-Vorschau](standard/index.md)

<a name="xaml" />

## <a name="xaml-basicsxaml-basicsindexmd"></a>[XAML-Grundlagen](xaml-basics/index.md)

XAML ermöglicht Entwicklern das Definieren von Benutzeroberflächen in Xamarin.Forms-Anwendungen, die mithilfe von Code, sondern Markup. XAML ist nie in einem Programm Xamarin.Forms erforderlich, doch sie wird toolfähig, und ist oft visuell kohärente und kompaktere als der entsprechende Code. XAML eignet sich besonders gut für die Verwendung mit der beliebten Model-View-ViewModel (MVVM) Anwendungsarchitektur: XAML-Code definiert die Sicht, die durch XAML-basierte datenbindungen ViewModel Code verknüpft ist.

## <a name="xaml-compilationxamlcmd"></a>[XAML-Kompilierung](xamlc.md)

XAML kann optional auch direkt mit dem XAML-Compiler (XAMLC) in der Zwischensprache (Intermediate Language, IL) kompiliert werden. Dieser Artikel beschreibt, wie XAMLC und seine Vorteile.

## <a name="xaml-previewerxaml-previewermd"></a>[XAML-Vorschau](xaml-previewer.md)

Die [XAML-Vorschau](~/xamarin-forms/xaml/xaml-previewer.md) vorgestellt Xamarin entwickeln 2016 steht für Tests in den Alpha-Kanal.

## <a name="xaml-namespacesnamespacesmd"></a>[XAML-Namespaces](namespaces.md)

XAML verwendet die `xmlns` XML-Attribut für Namespacedeklarationen. Dieser Artikel führt die Verwendung von XAML-Namespace-Syntax und zeigt, wie einen XAML-Namespace auf einen Typ zu deklarieren.

## <a name="xaml-markup-extensionsmarkup-extensionsindexmd"></a>[XAML-Markuperweiterungen](markup-extensions/index.md)

XAML umfasst Markuperweiterungen zum Festlegen von Attributen auf Werten oder Objekten hinter was mit einfachen Zeichenfolgen ausgedrückt werden kann. Dazu gehören Konstanten, statische Eigenschaften und Felder, Ressourcenverzeichnis und datenbindungen verweisen.

## <a name="passing-argumentspassing-argumentsmd"></a>[Übergeben von Argumenten](passing-arguments.md)

XAML kann verwendet werden, um Argumente zu übergeben, um nicht standardmäßige Konstruktoren oder Factorymethoden. Dieser Artikel beschreibt die Verwendung von XAML-Attributen, die verwendet werden können, um Argumente auf Konstruktoren, Factorymethoden aufrufen, und geben Sie ein generisches Argument übergeben.

## <a name="bindable-propertiesbindable-propertiesmd"></a>[Bindbare Eigenschaften](bindable-properties.md)

In Xamarin.Forms wird die Funktionalität der common Language Runtime (CLR)-Eigenschaften von bindbare Eigenschaften verlängert. Eine bindbare Eigenschaft ist eine besondere Art von Eigenschaft, auf dem Wert der Eigenschaft wird von dem Eigenschaftensystem Xamarin.Forms nachverfolgt. Dieser Artikel bietet eine Einführung in die bindungsfähigen Eigenschaften und veranschaulicht das Erstellen und nutzen müssen.

## <a name="attached-propertiesattached-propertiesmd"></a>[Angefügte Eigenschaften](attached-properties.md)

Eine angefügte Eigenschaft ist eine besondere Art von bindbare Eigenschaft, die in einer Klasse definiert, jedoch auf andere Objekte angefügt und erkennbaren in XAML als ein Attribut enthält, die eine Klasse, und ein Eigenschaftsnamen, die durch einen Punkt getrennt. Dieser Artikel bietet eine Einführung in angefügte Eigenschaften und veranschaulicht das Erstellen und nutzen müssen.

## <a name="resource-dictionariesresource-dictionariesmd"></a>[Ressourcenverzeichnisse](resource-dictionaries.md)

Verwendung von XAML-Ressourcen sind Definitionen der Objekte, die mehr als einmal verwendet werden können. Ein [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) können Ressourcen in einem zentralen Ort definiert und wieder in der gesamten einer Xamarin.Forms-Anwendung verwendet werden. In diesem Artikel wird veranschaulicht, wie erstellen und nutzen einen `ResourceDictionary`, und zum Zusammenführen `ResourceDictionary` in eine andere.
