---
title: Zusammenfassung zu Kapitel 1. Wie passt sich xamarin. Forms an?
description: 'Erstellen von Mobile Apps mit xamarin. Forms: Zusammenfassung von Kapitel 1. Wie passt sich xamarin. Forms an?'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: F3F864FF-EE70-49D0-90D1-388889037625
author: davidbritch
ms.author: dabritch
ms.date: 07/19/2018
ms.openlocfilehash: 6dfa473bdfb4c1dd88ca833dbf5011a0bbdec42a
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73032882"
---
# <a name="summary-of-chapter-1-how-does-xamarinforms-fit-in"></a>Zusammenfassung zu Kapitel 1. Wie passt sich xamarin. Forms an?

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter01)

> [!NOTE]
> Hinweise auf dieser Seite geben Bereiche an, in denen xamarin. Forms von dem im Buch dargestellten Material abweickte.

Einer der unangenehmsten Aufträge bei der Programmierung besteht darin, eine Codebasis von einer Plattform auf eine andere zu portieren, insbesondere, wenn diese Plattform eine andere Programmiersprache umfasst. Bei der Portierung des Codes besteht eine Versuchung, wenn beide Plattformen parallel verwaltet werden müssen, und die Unterschiede zwischen den beiden Codebasen erschweren die zukünftige Wartung.

## <a name="cross-platform-mobile-development"></a>Plattformübergreifende mobile Entwicklung

Dieses Problem kommt häufig vor, wenn Sie auf Mobile Plattformen abzielen. Derzeit gibt es zwei wichtige Mobile Plattformen: die Apple-Produktfamilie von iPhones und iPads, auf denen das IOS-Betriebssystem ausgeführt wird, und das Android-Betriebssystem, das auf einer Vielzahl von Telefonen und Tablets ausgeführt wird. Eine weitere bedeutende Plattform ist die universelle Windows-Plattform von Microsoft (UWP), die ein einzelnes Programm für Windows 10 als Ziel ermöglicht.

Ein Softwarehersteller, der diese Plattformen als Zielplattform anfordert, muss verschiedene Benutzeroberflächen Paradigmen, drei verschiedene Entwicklungsumgebungen, drei verschiedene Programmierschnittstellen und&mdash;, die sich möglicherweise besonders umständlich&mdash;drei unterschiedlichen Programmiersprachen: Ziel-C für iPhone und iPad, Java für Android und C# für Windows.

## <a name="the-c-and-net-solution"></a>Die C# -und .net-Lösung

Obwohl "Ziel-C", " C# Java" und alle von der Programmiersprache C abgeleitet sind, haben Sie sich durch sehr unterschiedliche Pfade entwickelt. C#ist die aktuellste dieser Sprachen und wurde auf sehr nützliche Weise gereift. Darüber hinaus C# ist eng mit einer gesamten Programmier Infrastruktur namens .NET verknüpft und bietet Unterstützung für Mathematik, Debugging, Reflektion, Auflistungen, Globalisierung, Datei-e/a, Netzwerke, Sicherheit, Threading, Webdienste, Datenverarbeitung, und lesen und Schreiben von XML und JSON.

Xamarin stellt derzeit Tools bereit, mit denen die systemeigenen Mac-, IOS- C# und Android-APIs mithilfe von und .net ausgerichtet werden. Diese Tools werden als xamarin. Mac, xamarin. IOS und xamarin. Android bezeichnet und auch als xamarin Platform bezeichnet. Dabei handelt es sich um Bibliotheken und Bindungen, die die nativen APIs dieser Plattformen mit .net-Idioms Ausdrücken.

Entwickler können die xamarin-Plattform zum Schreiben von Anwendungen C# in die Ziel-Mac-, IOS-oder Android-Anwendungen verwenden. Wenn Sie jedoch mehr als eine Plattform als Ziel verwenden, ist es sehr sinnvoll, einen Teil des Codes für die Zielplattformen freizugeben. Dies beinhaltet das Aufteilen des Programms in Platt Form abhängigen Code (im Allgemeinen die Benutzeroberfläche) und plattformunabhängigen Code, der in der Regel nur das Basis-.NET Framework erfordert. Dieser plattformunabhängige Code kann sich entweder in einer portablen Klassenbibliothek (Portable Class Library, PCL) oder in einem freigegebenen Projekt befinden, das häufig als frei gegebenes Ressourcen Projekt oder SAP bezeichnet wird.

> [!NOTE]
> Portable Klassenbibliotheken wurden durch .NET Standard-Bibliotheken ersetzt. Der gesamte Beispielcode aus dem Buch wurde in die Verwendung von .NET Standard-Bibliotheken konvertiert.

## <a name="introducing-xamarinforms"></a>Einführung in xamarin. Forms

Wenn Sie mehrere mobile Plattformen ansprechen, bietet xamarin. Forms noch mehr Code Freigabe. Ein einzelnes Programm, das für xamarin. Forms geschrieben wurde, kann auf diese Plattformen ausgerichtet werden:

- IOS für Programme, die auf iPhone, iPad und iPod Touchscreen ausgeführt werden
- Android for Programme, die auf Android-Smartphones und-Tablets ausgeführt werden
- der universelle Windows-Plattform für Windows 10.

> [!NOTE]
> Xamarin. Forms unterstützt Windows 8.1, Windows Phone 8,1 oder Windows 10 Mobile nicht mehr, aber xamarin. Forms-Anwendungen werden auf dem Windows 10-Desktop ausgeführt. Es gibt auch eine Vorschau Unterstützung für die Plattformen [Mac](~/xamarin-forms/platform/other/mac.md), [WPF](~/xamarin-forms/platform/other/wpf.md), [gtk #](~/xamarin-forms/platform/other/gtk.md)und [tizen](~/xamarin-forms/platform/other/tizen.md) .

Der Großteil eines xamarin. Forms-Programms ist in einer Bibliothek oder einem SAP vorhanden. Jede der Plattformen besteht aus einem kleinen anwendungstub, der den freigegebenen Code aufruft.

Die xamarin. Forms-APIs sind systemeigenen Steuerelementen auf jeder Plattform zugeordnet, sodass jede Plattform das Aussehen und Gefühl der Merkmale beibehält:

[![Dreifacher Screenshot der Freigabe von Platt Form Visualisierungen](images/ch01fg03-small.png "Xamarin. Forms-Steuerelemente auf jeder Plattform")](images/ch01fg03-large.png#lightbox "Xamarin. Forms-Steuerelemente auf jeder Plattform")

Die Screenshots von links nach rechts zeigen ein iPhone und ein Android-Telefon:

Auf jedem Bildschirm enthält die Seite eine xamarin. Forms- [`Label`](xref:Xamarin.Forms.Label) zum Anzeigen von Text, eine [`Button`](xref:Xamarin.Forms.Button) zum Initiieren von Aktionen, eine [`Switch`](xref:Xamarin.Forms.Switch) zum Auswählen eines on/off-Werts und eine [`Slider`](xref:Xamarin.Forms.Slider) zum Angeben eines Werts in einem kontinuierlichen Bereich. . Alle vier Ansichten sind untergeordnete Elemente einer [`StackLayout`](xref:Xamarin.Forms.StackLayout) auf einem [`ContentPage`](xref:Xamarin.Forms.ContentPage).

Auch an die Seite angefügt ist eine xamarin. Forms-Symbolleiste, die aus mehreren [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem) Objekten besteht. Diese sind als Symbole am oberen Rand der IOS-und Android-Bildschirme und unten auf dem Bildschirm Windows 10 Mobile sichtbar.

Xamarin. Forms unterstützt auch XAML, die Extensible Application Markup Language für mehrere Anwendungsplattformen bei Microsoft entwickelt. Alle visuellen Elemente des oben gezeigten Programms werden in XAML definiert, wie im Beispiel [**platformvisuals**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter01/PlatformVisuals) veranschaulicht.

Ein xamarin. Forms-Programm kann ermitteln, auf welcher Plattform es ausgeführt wird, und unterschiedliche Codes entsprechend ausführen. Entwickler können benutzerdefinierten Code für die verschiedenen Plattformen schreiben und diesen Code aus einem xamarin. Forms-Programm plattformunabhängig ausführen. Entwickler können auch zusätzliche Steuerelemente erstellen, indem Sie Renderer für jede Plattform schreiben.

Xamarin. Forms ist eine gute Lösung für Branchen Anwendungen oder für das Erstellen von Prototypen, oder es ist weniger ideal für Anwendungen, die Vektorgrafiken oder eine komplexe Berührungs Interaktion erfordern.

## <a name="your-development-environment"></a>Ihre Entwicklungsumgebung

Ihre Entwicklungsumgebung hängt davon ab, welche Plattformen Sie als Ziel verwenden möchten und welche Computer Sie verwenden möchten.

Wenn Sie auf IOS abzielen möchten, benötigen Sie einen Mac mit Xcode und der installierten xamarin-Plattform. Die Unterstützung von Android erfordert auch die Installation von Java und der erforderlichen sdche. Anschließend können Sie IOS und Android mithilfe von Visual Studio für Mac als Ziel verwenden.

Durch die Installation von Visual Studio können Sie auf dem PC IOS-, Android-und alle Windows-Plattformen als Zielplattform installieren. Allerdings erfordert die Zielplattform für IOS aus Visual Studio weiterhin einen Mac mit Xcode und die xamarin-Plattform.

Sie können Programme entweder auf einem Gerät, das über USB verbunden ist, mit dem Computer oder in einem Simulator testen.

## <a name="installation"></a>Installation

Bevor Sie eine xamarin. Forms-Anwendung erstellen und erstellen, sollten Sie versuchen, eine IOS-Anwendung, eine Android-Anwendung und eine UWP-Anwendung abhängig von den Plattformen, die Sie als Ziel verwenden möchten, und ihrer Entwicklungsumgebung zu erstellen und zu erstellen.

Die xamarin-und Microsoft-Websites enthalten Informationen zu diesem Zweck:

- [Einstieg in ios](~/ios/get-started/index.md)
- [Einstieg in Android](~/android/get-started/index.md)
- [Windows Developer Center](https://dev.windows.com)

Wenn Sie Projekte für diese einzelnen Plattformen erstellen und ausführen können, sollten Sie keine Probleme beim Erstellen und Ausführen einer xamarin. Forms-Anwendung haben.

## <a name="related-links"></a>Verwandte Links

- [Kapitel 1 Volltext (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch01-Apr2016.pdf)
- [Kapitel 1 (Beispiel)](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter01)
