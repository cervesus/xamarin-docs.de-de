---
title: Zusammenfassung der Chapter 1. Wie ist Xamarin.Forms angebracht?
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: F3F864FF-EE70-49D0-90D1-388889037625
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 534c36a16acdc10ffb6f6b6703296a672875286e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="summary-of-chapter-1-how-does-xamarinforms-fit-in"></a>Zusammenfassung der Chapter 1. Wie ist Xamarin.Forms angebracht?

Einer der am häufigsten unliebsame Aufträge in der Programmierung ist eine Codebasis zwischen zwei Plattformen in eine andere portieren, insbesondere, wenn diese Plattform eine andere Programmiersprache umfasst. Eine Versuchung vorhanden ist, wenn Sie den Code, um es auch Umgestalten portieren, aber wenn beide Plattformen parallel verwaltet werden müssen, klicken Sie dann die Unterschiede zwischen den zwei Codebasen werden künftige Wartung schwieriger machen.

## <a name="cross-platform-mobile-development"></a>Plattformübergreifende mobile Entwicklung

Dieses Problem ist üblich, wenn mobile Plattformen abzielen. Aktuell existieren zwei wichtigen mobilen Plattformen, die Apple-Familie von iPhones und iPads ausgeführt, das Betriebssystem iOS und Android-Betriebssysteme, die auf einer Vielzahl von Mobiltelefonen und Tablets ausgeführt wird. Eine andere wichtige Plattform ist Microsofts universelle Windows-Plattform (UWP), wodurch ein einzelnes Programm für Windows 10 und Windows 10 Mobile zu entwickeln.

Muss eine Softwareanbieter, die für diese drei Plattformen entwickeln möchte mit verschiedenen Benutzeroberflächen-Paradigmen, drei anderen entwicklungsumgebungen, drei verschiedene Programmierschnittstellen, verarbeiten und&mdash;vielleicht am häufigsten falsch&mdash; drei verschiedenen Programmiersprachen: Objective-C für iPhone und iPad für Android Java und c# für Windows.

## <a name="the-c-and-net-solution"></a>Die Lösung für c# und .NET

Obwohl Objective-C, Java und c# alle von der Programmiersprache C abgeleitet sind, haben sie durch sehr unterschiedliche Pfade entwickelt. C# ist die aktuellste Version der beiden Sprachen und wurde auf nützliche Weise verzichten wurde. Darüber hinaus c# ist eng in eine gesamte Programmierung Infrastruktur aufgerufen .NET mit Unterstützung für mathematische, Debuggen, Reflektion, Sammlungen, Globalisierung, Datei-e/a, Netzwerk, Sicherheit, threading, Webdienste, die Behandlung von Daten und XML- und JSON, lesen und schreiben.

Xamarin bietet derzeit Tools, um die systemeigenen Mac, iOS und Android-APIs mit c# und .NET als Ziel. Diese Tools heißen Xamarin.Mac, Xamarin.iOS und Xamarin.Android, die zusammen als die Xamarin-Plattform bezeichnet. Dies sind die Bibliotheken und Bindungen, die den systemeigenen APIs dieser Plattformen mit .NET Idiome express.

Die Xamarin-Plattform können Entwickler Anwendungen für Mac, iOS oder Android in c# schreiben. Jedoch wenn mehr als eine Plattform verwenden möchten, können sehr sinnvoll, einigen Teilen des Codes zwischen Zielplattformen freizugeben. Dies umfasst das Aufteilen des Programms in plattformabhängigen Code (in der Regel mit der Benutzeroberfläche), und plattformunabhängigen Code, die in der Regel nur die grundlegende .NET Framework erforderlich ist. Diese plattformunabhängigen Code kann entweder in eine Portable Klassenbibliothek (PCL) oder einem freigegebenen Projekt häufig aufgerufen wird, einen freigegebenen Asset-Projekts oder SAP befinden.

## <a name="introducing-xamarinforms"></a>Einführung in Xamarin.Forms

Wenn Sie mehrere mobile Plattformen zu verwenden, können Xamarin.Forms noch mehr Codefreigabe. Ein einzelnes Programm für Xamarin.Forms geschrieben kann fünf unterschiedliche Zielplattformen:

- iOS für Programme, die auf dem iPhone, iPad und iPod Touch ausgeführt werden soll.
- Android für Programme, die auf Android-Telefone und Tablets ausgeführt werden soll.
- die universelle Windows-Plattform Ziel Windows 10 und Windows 10 Mobile
- der Windows-Laufzeit-API von Windows 8.1
- der Windows-Laufzeit-API des Windows Phone 8.1

Die aktuelle Xamarin.Forms-Projektmappenvorlagen enthalten keine Projekte von Vorlagen für Windows 8.1 und Windows Phone 8.1-Plattformen.

Der Großteil einer Xamarin.Forms-Programm ist in einer PCL oder ein SAP vorhanden. Alle Plattformen besteht eine kleine Anwendung-Stub, der in der PCL aufruft. Die Xamarin.Forms-APIs zuordnen in native Steuerelemente für jede Plattform, sodass jede Plattform des Merkmal Erscheinungsbilds verwaltet:

[![Dreifacher Screenshot der Plattform visuelle Elemente, die Freigabe](images/ch01fg03-small.png "Xamarin.Forms Controls on Each Platform")](images/ch01fg03-large.png#lightbox "Xamarin.Forms Controls on Each Platform")

Die Screenshots von links nach rechts um ein iPhone, einem Android-Telefon und einem Windows 10 Mobile-Telefon anzeigen. Auf jedem Bildschirm enthält die Seite eine Xamarin.Forms [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) für die Anzeige von Text, einen [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) zum Initiieren von Aktionen, eine [ `Switch` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Switch/) für Wählen einen Wert ein bzw. aus und ein [ `Slider` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/) zum Angeben eines Werts in eine Breite Palette. Alle vier dieser Ansichten sind untergeordnete Elemente des eine [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) auf eine [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/).

Auch auf der Seite "angefügt ist eine Xamarin.Forms-Symbolleiste, bestehend aus mehreren [ `ToolbarItem` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ToolbarItem/) Objekte. Diese werden als Symbole oben auf IOS- und Android Bildschirme und am unteren Rand des Bildschirms für Windows 10 Mobile.

Xamarin.Forms unterstützt auch das XAML, die die Extensible Application Markup Language, die bei Microsoft für mehrere Anwendungsplattformen entwickelt. Alle visuellen Elemente des oben gezeigten Programms werden in XAML definiert, wie in der [ **PlatformVisuals** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter01/PlatformVisuals) Beispiel.

Ein Programm Xamarin.Forms bestimmen Plattform ausgeführt wird und anderen Code entsprechend ausführen. Entwickler können Kundenlisten erfolgreicher, Schreiben benutzerdefinierten Code für die verschiedenen Plattformen und führen Sie diesen Code aus einem Programm Xamarin.Forms auf plattformunabhängige Weise. Entwickler können auch zusätzliche Steuerelemente erstellen, durch das Schreiben von Renderern für jede Plattform.

Während Xamarin.Forms eine geeignete Lösung für Line-of-Business-Anwendungen oder für Prototypen oder machen eine schnelle Proof of Concept Demonstration ist, ist es weniger ideal für Anwendungen, die Vektorgrafiken oder komplexe Touch-Interaktion erforderlich ist.

## <a name="your-development-environment"></a>Entwicklungsumgebung vorbereiten

Die Entwicklungsumgebung, hängt davon ab, welche Plattformen, die Sie abzielen möchten und welche Maschinen verwenden möchten.

Wenn Sie auf iOS möchten, benötigen Sie einen Mac mit Xcode und Xamarin-Plattform installiert. Android sowie unterstützende erfordert das Installieren von Java und der erforderlichen SDKs. Sie können dann IOS- und Android mithilfe von Visual Studio für Mac abzielen

Installieren von Visual Studio können Sie auf dem PC iOS, Android und alle Windows-Plattformen als Ziel. Allerdings erfordert die Zielplattform iOS von Visual Studio noch einen Mac mit Xcode und Xamarin-Plattform installiert.

Sie können die Programme, die entweder einem echten Gerät über USB an den Computer angeschlossen, oder auf einem Simulator testen.

## <a name="installation"></a>Installation

Vor dem Erstellen, und erstellen eine Xamarin.Forms-Anwendung, sollten Sie versuchen, erstellen und separat erstellen Sie eine iOS-Anwendung, einer Android-Anwendung und einer UWP-Anwendung, abhängig von den Plattformen, Ziel und die Entwicklungsumgebung werden soll.

Xamarin und Microsoft-Websites enthalten Informationen über die Vorgehensweise:

- [Erste Schritte mit iOS](~/ios/get-started/index.md)
- [Erste Schritte mit Android](~/android/get-started/index.md)
- [Windows Dev Center](http://dev.windows.com)

Sie können einmal erstellen und Projekte für diesen einzelnen Plattformen ausgeführt werden, sollten Sie kein Problem erstellen und Ausführen einer Xamarin.Forms-Anwendung haben.



## <a name="related-links"></a>Verwandte Links

- [Chapter 1 Volltext (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch01-Apr2016.pdf)
- [Chapter 1-Beispiel](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter01)
