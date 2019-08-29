---
title: Xamarin.Forms-XAML-Grundlagen
description: Dieses Handbuch erklärt, wie Sie erste Schritte mit plattformübergreifenden XAML für mobile Geräte. XAML ermöglicht Entwicklern, Benutzeroberflächen in Xamarin.Forms-Anwendungen mit Markup statt Code zu definieren.
ms.prod: xamarin
ms.assetid: 67CC2CD6-D10A-4B14-9696-1D3A410EFFBF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/25/2017
ms.openlocfilehash: a7e98f64672788f13b247aa6cbba8adca84bb319
ms.sourcegitcommit: 1dd7d09b60fcb1bf15ba54831ed3dd46aa5240cb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2019
ms.locfileid: "70121547"
---
# <a name="xamarinforms-xaml-basics"></a>Xamarin.Forms-XAML-Grundlagen

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xamlsamples)

XAML (Extensible Application Markup Language) ermöglicht es Entwicklern, Benutzeroberflächen in Xamarin.Forms-Anwendungen mit Markup statt mit Code zu definieren. XAML ist nicht in einer Xamarin.Forms-Anwendung erforderlich, aber häufig ist kompakt und visuell kohärent ist als der entsprechende Code und potenziell lässt. XAML eignet sich besonders gut für die Verwendung mit der gängigen MVVM (Model-View-ViewModel)-Anwendungsarchitektur: XAML definiert die Sicht, die über XAML-basierte Daten Bindungen mit dem ViewModel-Code verknüpft ist.

## <a name="xaml-basics-contents"></a>Inhalt der XAML-Grundlagen

- [Übersicht](#Overview)
- [Teil 1. Erste Schritte mit XAML](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md)
- [Teil 2. Grundlegende XAML-Syntax](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md)
- [Teil 3. XAML-Markuperweiterungen](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Teil 4. Grundlagen der Datenbindung](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)
- [Teil 5. Aus einer Datenbindung zu MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)

Sie können zusätzlich zu diesen Artikeln XAML-Grundlagen Kapiteln des Buchs herunterladen [Erstellen mobiler Apps mit Xamarin.Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md):

[![](images/cover-sml.png "Buch-Abdeckung")](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md)

XAML-Themen werden behandelt, ausführlicher in viele Kapiteln des Buchs, einschließlich:


| Kapitel | Herunterladen | Zusammenfassung |
|---------|---------|---------|
| Kapitel 7. Visual Studio mit XAML. Code | [PDF herunterladen](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch07-Apr2016.pdf) | [Zusammenfassung](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter07.md) |
| Kapitel 8. Code und XAML in Harmonie | [PDF herunterladen](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch08-Apr2016.pdf) | [Zusammenfassung](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter08.md) |
| Kapitel 10. XAML-Markuperweiterungen | [PDF herunterladen](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch10-Apr2016.pdf) | [Zusammenfassung](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter10.md) |
| Kapitel 18. MVVM | [PDF herunterladen](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch18-Apr2016.pdf) | [Zusammenfassung](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter18.md) |

Diese Kapitel möglich [kostenlos heruntergeladen](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md).

<a name="Overview" />

## <a name="overview"></a>Übersicht

XAML ist eine XML-basierte Sprache, die von Microsoft erstellt, als Alternative zur Programmcode für das Instanziieren und Initialisieren von Objekten und die Objekte in der über-/ unterordnungshierarchien zu organisieren. XAML wurde an mehrere Technologien in .NET Framework angepasst wurde, jedoch die größte Hilfsprogramm hat bei der Definition des Layouts von Benutzeroberflächen in Windows Presentation Foundation (WPF), Silverlight, die Windows-Runtime und die universelle Windows gefunden Plattform (UWP).

XAML ist auch Teil von Xamarin.Forms, die plattformübergreifende nativ-basierte Programmierschnittstelle für iOS-, Android- und UWP mobile Geräte. In der XAML-Datei kann der Xamarin.Forms-Entwickler von Benutzeroberflächen, die mit allen der Xamarin.Forms-Ansichten, Layouts und Seiten als auch als benutzerdefinierte Klassen definieren. Die XAML-Datei kann entweder kompiliert oder in die ausführbare Datei eingebettet werden. In beiden Fällen wird die XAML-Informationen analysiert, zur Buildzeit können Sie benannte Objekte ermitteln, und wieder zur Laufzeit zum Instanziieren und Initialisieren von Objekten und Links zwischen diesen Objekten und Programmiercode einzurichten.

XAML hat mehrere Vorteile gegenüber der entsprechende Code:

- XAML ist häufig kompakt und besser lesbar als die entsprechenden Code.
- Die XML-eigenen über-/ unterordnungshierarchie kann XAML, um für mehr visual Klarheit der über-/ unterordnungshierarchie von Benutzeroberflächenobjekten zu imitieren.
- XAML kann leicht von Programmierern handschriftlichen, aber eignet sich Tools bearbeitbaren und von visuellen Entwurfstools generiert werden.

Es gibt natürlich auch Nachteile, Einschränkungen, die von Markupsprachen für das systeminterne indexspeicherkosten beziehen:

- XAML kann nicht mit Code enthalten. Alle Ereignishandler müssen in einer Codedatei definiert werden.
- XAML kann keine Schleifen zur wiederholten Verarbeitung enthalten. (Allerdings mehrere Xamarin.Forms visuelle Objekte – insbesondere [ `ListView` ](xref:Xamarin.Forms.ListView) – kann mehrere untergeordnete Elemente erstellt auf der Grundlage der Objekte in der `ItemsSource` Sammlung.)
- XAML kann nicht als bedingte Verarbeitung enthalten (jedoch kann eine Datenbindung einen codebasierten Binding-Konverter, der eine bedingte Verarbeitung ermöglicht verweisen.)
- XAML kann nicht in der Regel Klassen instanziiert, die einen parameterlosen Konstruktor nicht definieren. (Allerdings es ist manchmal eine Methode zur Umgehung dieser Einschränkung.)
- XAML kann nicht in der Regel Methoden aufrufen. (In diesem Fall kann diese Einschränkung manchmal überwunden werden.)

Es ist noch kein visuellen Designer für die Generierung von XAML in Xamarin.Forms-Anwendungen. Alle XAML handschriftlich, es ist jedoch ein [XAML-Vorschau](~/xamarin-forms/xaml/xaml-previewer/index.md). Neue XAML Programmierer sollten so häufig erstellen, und führen ihre Anwendungen, insbesondere nach der alle Elemente, die möglicherweise nicht offensichtlich richtig. Sogar Entwickler, die mit viel Erfahrung in XAML wissen, dass es sich bei experimentieren belohnen ist.

XAML ist im Grunde XML, aber XAML verfügt über einige Features für die spezielle Syntax. Die wichtigsten sind:

- Property-Elemente
- Angefügte Eigenschaften
- Markuperweiterungen

Diese Funktionen sind *nicht* XML-Erweiterungen. XAML ist vollständig gültige XML-Daten. Aber diese Funktionen der XAML-Syntax Verwenden von XML in eindeutigen Möglichkeiten. Sie werden in den folgenden Artikeln ausführlich beschrieben, die mit der eine Einführung zur Verwendung von XAML für die Implementierung von MVVM schließen.

## <a name="requirements"></a>Anforderungen

In diesem Artikel geht davon aus, mit Xamarin.Forms auszukennen. Dieser Artikel setzt auch voraus eine gewisse Vertrautheit mit XML, einschließlich Grundlegendes zur Verwendung von XML-Namespace-Deklarationen und die Bedingungen *Element*, *Tag*, und *Attribut*.

Wenn Sie mit Xamarin.Forms und XML vertraut sind, lesen Sie zum Einstieg [Teil 1. Erste Schritte mit XAML](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md).

## <a name="related-links"></a>Verwandte Links

- [XamlSamples](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xamlsamples)
- [Erstellen Mobile Apps-Buch](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md)
- [Xamarin.Forms Samples (Beispiele für Xamarin.Forms)](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.Forms)
