---
title: Grundlagen der Verwendung von XAML-Xamarin.Forms
description: "Erste Schritte mit plattformübergreifenden Markup für mobile Geräte"
ms.topic: article
ms.prod: xamarin
ms.assetid: 67CC2CD6-D10A-4B14-9696-1D3A410EFFBF
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 10/25/2017
ms.openlocfilehash: a3f3dbbe0f12cfa7cc1fc6606ec8bd48a96e407c
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2018
---
# <a name="xamarinforms-xaml-basics"></a>Grundlagen der Verwendung von XAML-Xamarin.Forms

XAML – die eXtensible Application Markup Language – ermöglicht es Entwicklern, Definieren von Benutzeroberflächen in Xamarin.Forms-Anwendungen, die mithilfe von Code, sondern Markup. XAML ist nie in einem Programm Xamarin.Forms erforderlich, aber es ist häufig kompakt und visuell kohärente als der entsprechende Code und potenziell toolfähig. XAML eignet sich besonders gut für die Verwendung mit der beliebten MVVM (Model-View-ViewModel) Anwendungsarchitektur: XAML-Code definiert die Sicht, die durch XAML-basierte datenbindungen ViewModel Code verknüpft ist.

## <a name="xaml-basics-contents"></a>Verwendung von XAML-Grundlagen Inhalt

* [Übersicht](#Overview)
* [Teil 1. Erste Schritte mit XAML](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md)
* [Teil 2. Grundlegende XAML-Syntax](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md)
* [Teil 3. Verwendung von XAML-Markuperweiterungen](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
* [Teil 4. Data Binding-Grundlagen](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)
* [Teil 5. Aus dem Datenbindung an MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)

Sie können zusätzlich zu den folgenden Artikeln Grundlagen der Verwendung von XAML-Kapiteln des Buchs herunterladen [Erstellen mobiler Apps mit Xamarin.Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md):

[![](images/cover-sml.png "Buch Abdeckung")](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md)

Verwendung von XAML-Themen werden behandelt, ausführlicher in vielen Kapiteln des Buchs, einschließlich:

<table style="border:0px; box-shadow:0 0px 0px" cellpadding="0" cellspacing="2" border="0" width="85%">
<tr style="background:#ecf0f1">
  <td style="border:0px;">
    <h4>Kapitel 7. XAML vs. Code</h4>
  </td>
  <td style="border:0px;" align="right"><a href="https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch07-Apr2016.pdf">PDF herunterladen</a> </td>
  <td style="border:0px;" align="right"><a href="~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter07.md">Zusammenfassung</a></td>
</tr>
<tr style="background:#f8f9fa">
  <td style="border:0px;">
    <h4>Kapitel 8. Code und XAML-Code in harmonische</h4>
  </td>
  <td style="border:0px;" align="right"><a href="https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch08-Apr2016.pdf">PDF herunterladen</a> </td>
  <td style="border:0px;" align="right"><a href="~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter08.md">Zusammenfassung</a></td>
</tr>
<tr style="background:#f8f9fa">
  <td style="border:0px;">
    <h4>Kapitel 10. Verwendung von XAML-Markuperweiterungen</h4>
  </td>
  <td style="border:0px;" align="right"><a href="https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch10-Apr2016.pdf">PDF herunterladen</a> </td>
  <td style="border:0px;" align="right"><a href="~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter10.md">Zusammenfassung</a></td>
</tr>
<tr style="background:#f8f9fa">
  <td style="border:0px;">
    <h4>Kapitel 18. MVVM</h4>
  </td>
  <td style="border:0px;" align="right"><a href="https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch18-Apr2016.pdf">PDF herunterladen</a> </td>
  <td style="border:0px;" align="right"><a href="~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter18.md">Zusammenfassung</a></td></tr>
</table>

Diese Kapitel kann [kostenlos heruntergeladen](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md).

<a name="Overview" />

## <a name="overview"></a>Übersicht

XAML ist eine XML-basierte Sprache, die von Microsoft als Alternative zur Programmcode zum Instanziieren und Initialisieren von Objekten und Organisieren von Objekten in über-/ unterordnungshierarchien erstellt. XAML wurde an verschiedene Technologien innerhalb von .NET Framework angepasst wurde, aber er hat größte Nützlichkeit gefunden, bei der Definition des Layouts von Benutzeroberflächen in Windows Presentation Foundation (WPF), Silverlight, Windows-Runtime und die universelle Windows Plattform (UWP).

XAML ist ebenfalls Teil der Xamarin.Forms, die plattformübergreifende systemintern-basierte Programmierschnittstelle für iOS, Android und uwp-Mobilgeräten. In der XAML-Datei kann Entwickler Xamarin.Forms Benutzeroberflächen, die mit allen Xamarin.Forms Ansichten, Layouts, und Seiten, als auch als benutzerdefinierte Klassen definieren. Die XAML-Datei kann entweder kompiliert oder in die ausführbare Datei eingebettet werden. In beiden Fällen wird die Verwendung von XAML-Informationen analysiert, zur Buildzeit können Sie benannte Objekte ermitteln und zur Laufzeit zu instanziieren und Initialisieren von Objekten und Links zwischen diesen Objekten und Programmcode einzurichten.

XAML-hat mehrere Vorteile gegenüber den entsprechenden Code:

-  XAML ist häufig kompakt und besser lesbar als die entsprechenden Code.
-  Die über-/ unterordnungshierarchie inhärenten im XML-Format ermöglicht XAML mit größer Übersichtlichkeit der über-/ unterordnungshierarchie von Benutzeroberflächenobjekten zu imitieren.
-  XAML kann einfach als Hand von Programmierern geschrieben werden, aber auch eignet sich zum toolfähig und vom visuellen Entwurfstools generiert werden.

Es gibt natürlich auch Nachteile, Einschränkungen, die systeminterne Markupsprachen sind vor allem aufgrund:

-  XAML kann nicht mit Code enthalten. Alle Ereignishandler müssen in einer Codedatei definiert werden.
-  XAML kann nicht für die wiederholte Verarbeitung Schleifen enthalten. (Allerdings mehrere Xamarin.Forms visueller Objekte – insbesondere [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) – können mehrere untergeordnete Elemente auf der Grundlage der Objekte in generieren seine `ItemsSource` Auflistung.)
-  XAML kann nicht als bedingte Verarbeitung enthalten (jedoch kann eine Datenbindung einen Code-basierte Bindung-Konverter, der eine bedingte Verarbeitung effektiv ermöglicht verweisen.)
-  XAML kann nicht in der Regel Klassen instanziiert, die einen parameterlosen Konstruktor nicht definieren. (Besteht jedoch manchmal eine Methode zur Umgehung dieser Einschränkung.)
-  Methoden kann nicht in der XAML im Allgemeinen aufgerufen. (In diesem Fall kann diese Einschränkung manchmal behoben werden.)

Es ist noch kein visuellen Designer für das Generieren von XAML in Xamarin.Forms-Anwendungen. Alle XAML muss manuell geschrieben, aber es gibt eine [XAML-Vorschau](~/xamarin-forms/xaml/xaml-previewer.md). Neue Programmierer in XAML sollten so häufig erstellen, und führen ihre Anwendungen, insbesondere nach Elementen, die möglicherweise nicht offensichtlich korrekte. Sogar Entwickler mit viel Erfahrung in XAML wissen, dass es sich bei Experimente belohnen ist.

XAML ist im Grunde XML, aber XAML-hat einige Funktionen spezielle Syntax. Am wichtigsten sind:

- Eigenschaftenelemente
- Angefügte Eigenschaften
- Markuperweiterungen

Diese Funktionen sind *nicht* XML-Erweiterungen. XAML ist vollständig gültige XML-Daten. Aber diese Features der XAML-Syntax verwenden XML auf eindeutige Weise. Sie werden detailliert in den folgenden Artikeln vorgestellt, die eine Einführung zur Verwendung von XAML für das Implementieren von MVVM enden.

## <a name="requirements"></a>Anforderungen

In diesem Artikel geht davon aus einem Kenntnisse Xamarin.Forms. Lesen von [eine Einführung in Xamarin.Forms](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md) wird dringend empfohlen.

In diesem Artikel geht davon aus mit XML, einschließlich der Grundlegendes zur Verwendung von XML-Namespace deklarieren und den Begriffen vertraut *Element*, *Tag*, und *Attribut*.

Wenn Sie mit Xamarin.Forms und XML vertraut sind, beginnen Sie lesen [Teil 1. Erste Schritte mit XAML](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md).



## <a name="related-links"></a>Verwandte Links

- [XamlSamples](https://developer.xamarin.com/samples/xamarin-forms/XamlSamples/)
- [Eine Einführung in Xamarin.Forms](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [Erstellen Mobile Apps-Buch](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md)
- [Xamarin.Forms-Beispiele](https://developer.xamarin.com/samples/xamarin-forms/all/)
