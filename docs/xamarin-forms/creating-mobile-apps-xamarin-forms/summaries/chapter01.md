---
title: Zusammenfassung der Kapitel 1. Wie Funktion hat xamarin.Forms?
description: 'Erstellen von mobilen Apps mit Xamarin.Forms: Zusammenfassung der Kapitel 1. Wie Funktion hat xamarin.Forms?'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: F3F864FF-EE70-49D0-90D1-388889037625
author: davidbritch
ms.author: dabritch
ms.date: 07/19/2018
ms.openlocfilehash: 58d3b3ae067913a85c3ada5f5b35e64511523ff8
ms.sourcegitcommit: 395774577f7524b57035c5cca3c9034a4b636489
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/10/2019
ms.locfileid: "54207907"
---
# <a name="summary-of-chapter-1-how-does-xamarinforms-fit-in"></a>Zusammenfassung der Kapitel 1. Wie Funktion hat xamarin.Forms?

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter01)

> [!NOTE]
> Anmerkungen zu dieser Version auf dieser Seite Geben Sie Bereiche, in denen Xamarin.Forms aus den Informationen im Buch abweichend hat, an.

Einer der am häufigsten unangenehme Aufträge in der Programmierung ist eine Codebasis zwischen zwei Plattformen zu einem anderen portieren, insbesondere dann, wenn die Plattform auf eine andere Programmiersprache umfasst. Beim Portieren von Code zum Gestalten Sie ihn auch Versuchung besteht, aber wenn beide Plattformen gleichzeitig verwaltet werden müssen, klicken Sie dann die Unterschiede zwischen der zwei Codebasen werden erschweren zukünftigen Wartung.

## <a name="cross-platform-mobile-development"></a>Plattformübergreifende mobile Entwicklung

Dieses Problem ist üblich, bei der mobile Plattformen als Ziel. Derzeit vorhanden sind zwei wichtigen mobilen Plattformen, die Apple-Produktfamilie iPhones und iPads unter iOS-Betriebssystem und Android-Betriebssysteme, die auf einer Vielzahl von Smartphones und Tablets ausgeführt wird. Eine andere wichtige Plattform ist Microsofts universelle Windows Plattform (UWP), wodurch ein einzelnes Programm beide Windows 10 als Ziel.

Muss ein Softwarehersteller, die diese Plattformen möchte mit verschiedenen Benutzeroberflächen-Paradigmen, die drei verschiedenen entwicklungsumgebungen, drei verschiedene Programmierschnittstellen, verarbeiten und&mdash;vielleicht am häufigsten falsch&mdash;drei verschiedene Programmiersprachen: Objective-C für das iPhone und iPad, Java für Android und C# für Windows.

## <a name="the-c-and-net-solution"></a>Die Lösung für C# und .NET

Obwohl die Objective-C, Java und C# -Code alle von der Programmiersprache C abgeleitet werden, haben sie durch sehr unterschiedliche Pfade weiterentwickelt. C# ist die aktuellste Version dieser Sprachen und verfügt über sehr praktischerweise Reifen wurde. Darüber hinaus C# ist eng verknüpft mit einer gesamten Programmierung Infrastruktur wird aufgerufen, .NET, die mathematische, Debuggen, Reflektion, Sammlungen, Globalisierung, e/a-, Netzwerk, Sicherheit, threading, Webdienste, Behandlung von Daten und XML unterstützt und JSON-Code lesen und schreiben.

Xamarin bietet derzeit die Tools, um die native Mac, iOS und Android-APIs mit C# und .NET als Ziel festzulegen. Diese Tools heißen Xamarin.Mac, Xamarin.iOS und Xamarin.Android, die zusammen als der Xamarin-Plattform bezeichnet. Dies sind die Bibliotheken und Bindungen, die Ausdrücken, die nativen APIs dieser Plattformen mit .NET Ausdrücke.

Die Xamarin-Plattform können Entwickler um Anwendungen in C# zu schreiben, für Mac, iOS oder Android. Aber wenn mehr als eine Plattform, damit sehr sinnvoll, einen Teil des Codes für die Zielplattformen freigeben. Dies umfasst das Aufteilen des Programms in plattformabhängigen Code (in der Regel mit der Benutzeroberfläche), und die plattformunabhängigen Code auf, die in der Regel nur grundlegende .NET Framework erforderlich ist. Diese plattformunabhängigen Code kann entweder in eine Portable Klassenbibliothek (PCL) oder ein freigegebenes Projekt, das häufig bezeichnet ein freigegebenes Projekt für Asset oder SAP befinden.

> [!NOTE]
> Portable Klassenbibliotheken wurden von .NET Standard-Bibliotheken ersetzt. Der Beispielcode aus dem Buch wurde zur Verwendung von .NET standard-Bibliotheken konvertiert.

## <a name="introducing-xamarinforms"></a>Einführung in Xamarin.Forms

Wenn mehrere mobile Plattformen abzielen, können Xamarin.Forms sogar noch mehr Freigeben von Code. Ein einzelnes Programm geschrieben, die für Xamarin.Forms kann diese Zielplattformen:

- iOS für Programme, die auf dem iPhone, iPad und iPod Touch ausgeführt werden soll.
- Android für Programme, die auf Android-Telefone und Tablets ausgeführt werden soll.
- die universelle Windows-Plattform für Windows 10

> [!NOTE]
> Xamarin.Forms nicht mehr unterstützt wird, Windows 8.1, Windows Phone 8.1 oder Windows 10 Mobile Xamarin.Forms-Anwendungen werden auf dem Windows 10-Desktop ausgeführt. Es gibt auch Unterstützung für die Vorschauversion der [Mac](~/xamarin-forms/platform/other/mac.md), [WPF](~/xamarin-forms/platform/other/wpf.md), [GTK#-](~/xamarin-forms/platform/other/gtk.md), und [Tizen](~/xamarin-forms/platform/other/tizen.md) Plattformen.

Der größte Teil einer Xamarin.Forms-Anwendung ist in einer Bibliothek oder eines von SAP vorhanden. Alle Plattformen besteht eine kleine Anwendung-Stub, der dieser freigegebene Code aufruft.

Die Xamarin.Forms-APIs zuordnen in native Steuerelemente für jede Plattform, sodass jede Plattform das Merkmal Erscheinungsbild verwaltet:

[![Dreifacher Screenshot der Plattform visuelle Elemente freigeben](images/ch01fg03-small.png "Xamarin.Forms Controls on Each Platform")](images/ch01fg03-large.png#lightbox "Xamarin.Forms Controls on Each Platform")

Die Screenshots von links nach rechts um ein iPhone und Android-Telefon anzeigen:

Auf den einzelnen Bildschirmen, die Seite enthält eine Xamarin.Forms [ `Label` ](xref:Xamarin.Forms.Label) zum Anzeigen von Text, ein [ `Button` ](xref:Xamarin.Forms.Button) für das Initiieren von Aktionen, eine [ `Switch` ](xref:Xamarin.Forms.Switch) für Wählen einen/aus-Wert, und ein [ `Slider` ](xref:Xamarin.Forms.Slider) für die Angabe eines Werts in einen durchgehenden Bereich. Alle vier dieser Ansichten sind untergeordnete Elemente von einem [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) auf eine [ `ContentPage` ](xref:Xamarin.Forms.ContentPage).

Auch auf der Seite angefügt ist eine Xamarin.Forms-Symbolleiste, bestehend aus mehreren [ `ToolbarItem` ](xref:Xamarin.Forms.ToolbarItem) Objekte. Diese werden als Symbole oben auf den IOS- und Android-Bildschirm, und klicken Sie am unteren Rand des Bildschirms für Windows 10 Mobile.

Xamarin.Forms unterstützt auch das XAML, der Extensible Application Markup Language, die bei Microsoft für mehrere Plattformen entwickelt. Alle visuellen Elemente des oben gezeigten-Programms sind in XAML definiert, wie in der [ **PlatformVisuals** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter01/PlatformVisuals) Beispiel.

Eine Xamarin.Forms-Anwendung kann ermitteln welcher Plattform er ausgeführt wird und anderen Code entsprechend ausführen. Entwickler können weitere leistungsstarke, Schreiben benutzerdefinierten Code für die verschiedenen Plattformen und führen ihn aus einer Xamarin.Forms-Anwendung, auf plattformunabhängige Weise. Entwickler können auch zusätzliche Steuerelemente erstellen, durch das Schreiben von Renderern für jede Plattform.

Xamarin.Forms ist eine gute Lösung für Line-of-Business-Anwendungen, oder für die Erstellung von Prototypen, und stellen eine kurze Demonstration des Proof of Concept-, es ist nur bedingt geeignet für Anwendungen, die müssen Vektorgrafiken oder komplexe Touch-Interaktionen.

## <a name="your-development-environment"></a>Entwicklungsumgebung

Entwicklungsumgebung verwenden, hängt davon ab, welche Plattformen, die Sie abzielen möchten und was Sie Computer verwenden möchten.

Wenn Sie auf iOS abzielen möchten, benötigen Sie einen Mac mit Xcode und Xamarin-Plattform installiert. Unterstützung von Android sowie erfordert die Installation von Java und der erforderlichen SDKs. Sie können dann iOS und Android mithilfe von Visual Studio für Mac als Ziel

Installieren von Visual Studio Ihnen auf dem PC zu iOS, Android und alle Windows-Plattformen abzielen. Für iOS in Visual Studio erfordert jedoch immer noch einen Mac mit Xcode und Xamarin-Plattform installiert.

Sie können Programme auf entweder einem echten Gerät über USB an den Computer angeschlossen ist, oder auf einem Simulator testen.

## <a name="installation"></a>Installation

Vor dem Erstellen und eine Xamarin.Forms-Anwendung erstellen, sollten Sie zum Erstellen und separat erstellen Sie eine iOS-Anwendung, eine Android-Anwendung und eine UWP-Anwendung, abhängig von den Plattformen, die Sie Ziel und die Entwicklungsumgebung vorbereiten möchten.

Die Xamarin und Microsoft-Websites enthalten Informationen zur Vorgehensweise hierfür:

- [Erste Schritte mit iOS](~/ios/get-started/index.md)
- [Erste Schritte mit Android](~/android/get-started/index.md)
- [Windows Developer Center](http://dev.windows.com)

Sie können einmal erstellen und Ausführen von Projekten für diese einzelne Plattformen, müssen Sie kein Problem erstellen und Ausführen einer Xamarin.Forms-Anwendung.

## <a name="related-links"></a>Verwandte Links

- [Kapitel 1 Volltext (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch01-Apr2016.pdf)
- [Kapitel 1-Beispiel](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter01)
