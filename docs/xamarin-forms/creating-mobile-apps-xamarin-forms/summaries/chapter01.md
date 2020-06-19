---
title: 'title: "Zusammenfassung von Kapitel 1: Die Nische von Xamarin.Forms"'
description: 'description: "Erstellen von mobilen Apps mit Xamarin.Forms: Zusammenfassung von Kapitel 1: Die Nische von Xamarin.Forms"'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: F3F864FF-EE70-49D0-90D1-388889037625
author: davidbritch
ms.author: dabritch
ms.date: 07/19/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 48b2fb429d206f6582886c94d4d99839d790dc8d
ms.sourcegitcommit: ea9269b5d9e3d68b61bb428560a10034117ee457
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2020
ms.locfileid: "84136926"
---
# <a name="summary-of-chapter-1-how-does-xamarinforms-fit-in"></a>ms.prod: xamarin ms.technology: xamarin-forms ms.assetid: F3F864FF-EE70-49D0-90D1-388889037625 author: davidbritch ms.author: dabritch ms.date: 07/19/2018 no-loc: [Xamarin.Forms, Xamarin.Essentials] Zusammenfassung von Kapitel 1:

Die Nische von Xamarin.Forms

> [!NOTE]
> [![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter01)

Die Hinweise auf dieser Seite weisen auf Stellen hin, an denen die aktuelle Version von Xamarin.Forms von den Darstellungen im Buch abweicht. Eine der unangenehmsten Aufgaben beim Programmieren ist die Portierung einer Codebasis von einer Plattform auf eine andere. Dies gilt insbesondere dann, wenn diese Plattform eine andere Programmiersprache verwendet.

## <a name="cross-platform-mobile-development"></a>Es besteht die Versuchung, beim Portieren des Codes auch ein Refactoring durchzuführen. Wenn jedoch beide Plattformen parallel gewartet werden müssen, erschweren die Unterschiede zwischen den Codebasen eine zukünftige Wartung.

Plattformübergreifende mobile Entwicklung Dieses Problem tritt häufig bei der Entwicklung für mobile Plattformen auf. Aktuell gibt es zwei große mobile Plattformen: die Apple-Familie der iPhones und iPads mit dem iOS-Betriebssystem, und das Android-Betriebssystem, das auf einer Vielzahl von Smartphones und Tablets ausgeführt wird.

Eine weitere wichtige Plattform ist die Universelle Windows-Plattform von Microsoft (Universal Windows Platform, UWP), mit der ein einzelnes Programm auf beide Windows 10-Varianten ausgerichtet werden kann.

## <a name="the-c-and-net-solution"></a>Ein Softwareanbieter, der auf beide Plattformen abzielen möchte, muss sich mit unterschiedlichen Paradigmen für die Benutzerschnittstelle, drei verschiedenen Entwicklungsumgebungen, drei verschiedenen Programmierschnittstellen und &mdash; im ungünstigsten Fall &mdash; mit drei verschiedenen Programmiersprachen auseinandersetzen: Objective-C für iPhone und iPad, Java für Android und C# für Windows.

Die Lösung mit C# und .NET Wenngleich Objective-C, Java und C# alle von der Programmiersprache C abgeleitet sind, haben sie sich sehr unterschiedlich entwickelt. C# ist die jüngste dieser Sprachen und bietet viele sehr nützliche Funktionen.

Darüber hinaus ist C# eng mit einer Programmierinfrastruktur namens .NET verbunden, die Unterstützung für mathematische Operationen, Debugging, Reflexion, Sammlungen, Globalisierung, Datei-E/A, Netzwerkbetrieb, Sicherheit, Threading, Webdienste, Datenverarbeitung sowie XML- und JSON-Lese- und Schreibvorgänge bietet. Xamarin stellt aktuell Tools bereit, die mithilfe von C# und .NET eine Ausrichtung auf die nativen Mac-, iOS- und Android-APIs ermöglichen. Diese Tools sind Xamarin.Mac, Xamarin.iOS und Xamarin.Android, zusammengefasst bezeichnet als die Xamarin-Plattform.

Es handelt sich hierbei um Bibliotheken und Bindungen, die die nativen APIs dieser Plattformen mit .NET-Begriffen ausdrücken. Entwickler können mithilfe der Xamarin-Plattform Anwendungen in C#, die auf Mac, iOS oder Android abzielen. Aber wenn es um mehr als eine Plattform geht, ist es sinnvoll, einen Teil des Codes für die Zielplattformen gemeinsam zu verwenden. Dabei wird das Programm in plattformabhängigen Code (in der Regel unter Einbeziehung der Benutzerschnittstelle) und plattformunabhängigen Code unterteilt, für den in der Regel nur das Basis-.NET-Framework erforderlich ist.

> [!NOTE]
> Dieser plattformunabhängige Code kann sich entweder in einer portablen Klassenbibliothek (Portable Class Library, PCL) oder in einem freigegebenen Projekt befinden, diese werden häufig auch als Projekte mit freigegebenen Ressourcen bezeichnet. Portable Klassenbibliotheken wurden durch .NET Standard-Bibliotheken ersetzt.

## <a name="introducing-xamarinforms"></a>Der gesamte Beispielcode innerhalb des Buchs wurde aktualisiert und verwendet jetzt die .NET Standard-Bibliotheken.

Einführung von Xamarin.Forms Bei der Entwicklung für mehrere mobile Zielplattformen ermöglicht Xamarin.Forms sogar eine noch umfassendere gemeinsame Codeverwendung.

- Ein einzelnes, mit Xamarin.Forms geschriebenes Programm kann auf den folgenden Plattformen verwendet werden:
- iOS für Programme, die auf iPhone, iPad und iPod Touch ausgeführt werden
- Android für Programme, die auf Android-Smartphones und -Tablets ausgeführt werden

> [!NOTE]
> Universelle Windows-Plattform für Windows 10 Xamarin.Forms bietet keine Unterstützung mehr für Windows 8.1, Windows Phone 8.1 oder Windows 10 Mobile, Xamarin.Forms-Anwendungen können jedoch unter Windows 10 Desktop ausgeführt werden.

Außerdem wird Vorschauunterstützung für [Mac](~/xamarin-forms/platform/other/mac.md), [WPF](~/xamarin-forms/platform/other/wpf.md), [GTK#](~/xamarin-forms/platform/other/gtk.md) und [Tizen](~/xamarin-forms/platform/other/tizen.md) bereitgestellt. Der Großteil eines Xamarin.Forms-Programms liegt in einer Bibliothek oder einem Projekt mit freigegebenen Ressourcen.

Jede der Plattformen umfasst einen kleinen Anwendungsstub, der diesen gemeinsam verwendeten Code aufruft.

Die Xamarin.Forms-APIs werden den nativen Steuerelementen auf jeder Plattform zugeordnet, sodass jede Plattform ihr charakteristisches Erscheinungsbild beibehält:

[![Screenshots zur Freigabe visueller Plattformelemente](images/ch01fg03-small.png "Xamarin.Forms-Steuerelemente auf jeder Plattform")](images/ch01fg03-large.png#lightbox "Xamarin.Forms-Steuerelemente auf jeder Plattform")

Der rechte Screenshot zeigt ein iPhone, der linke ein Android-Smartphone: Die Seite enthält in beiden Bildschirmen ein Xamarin.Forms-[`Label`](xref:Xamarin.Forms.Label)-Objekt zur Anzeige von Text, ein [`Button`](xref:Xamarin.Forms.Button)-Objekt zum Einleiten von Aktionen, ein [`Switch`](xref:Xamarin.Forms.Switch)-Objekt zur Auswahl eines EIN/AUS-Werts und ein [`Slider`](xref:Xamarin.Forms.Slider)-Objekt zur Festlegung eines Werts innerhalb eines fortlaufenden Bereichs.

Alle vier Ansichten sind untergeordnete Elemente von [`StackLayout`](xref:Xamarin.Forms.StackLayout) für eine [`ContentPage`](xref:Xamarin.Forms.ContentPage). Ebenfalls angefügt an die Seite ist eine Xamarin.Forms-Symbolleiste, die verschiedene [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem)-Objekte umfasst.

Diese werden als Symbole werden den iOS- und Android-Bildschirmen oben und auf dem Windows 10 Mobile-Bildschirm unten angezeigt. Xamarin.Forms unterstützt außerdem XAML (Extensible Application Markup Language), die von Microsoft für verschiedene Anwendungsplattformen entwickelt wurde.

Alle visuellen Elemente des oben gezeigten Programms werden in XAML definiert, wie im Beispiel [**PlatformVisuals**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter01/PlatformVisuals) veranschaulicht. Ein Xamarin.Forms-Programm kann erkennen, auf welcher Plattform es ausgeführt wird, und entsprechend unterschiedlichen Code ausführen. Noch leistungsfähiger ist die Möglichkeit, benutzerdefinierten Code für die verschiedenen Plattformen zu schreiben und diesen Code über ein Xamarin.Forms-Programm plattformunabhängig auszuführen.

Entwickler können auch zusätzliche Steuerelemente erstellen, indem Sie Renderer für jede Plattform schreiben.

## <a name="your-development-environment"></a>Xamarin.Forms ist eine gute Lösung für Branchenanwendungen, für die Erstellung von Prototypen oder für eine schnelle Proof-of-Concept-Demonstration. Weniger geeignet ist es hingegen für Anwendungen, die Vektorgrafiken oder komplexe Touchinteraktionen erfordern.

Ihre Entwicklungsumgebung

Ihre Entwicklungsumgebung richtet sich danach, auf welche Plattformen Sie abzielen und welche Computer Sie verwenden möchten. Wenn Sie für iOS entwickeln, benötigen Sie einen Mac mit Xcode und installierter Xamarin-Plattform. Wenn Sie außerdem Android unterstützen möchten, müssen Sie Java und die erforderlichen SDKs installieren.

Anschließend können Sie mit Visual Studio für Mac sowohl für iOS als auch für Android entwickeln. Durch die Installation von Visual Studio auf dem PC können Sie iOS, Android und alle Windows-Plattformen als Zielplattform nutzen.

Die Entwicklung für iOS mit Visual Studio setzt jedoch nach wie vor einen Mac mit Xcode und installierter Xamarin-Plattform voraus.

## <a name="installation"></a>Sie können Programme entweder auf einem physischen Gerät testen, das über USB an den Computer angeschlossen ist, oder auf einem Simulator.

Installation

Bevor Sie eine Xamarin.Forms-Anwendung erstellen und entwickeln, sollten Sie – je nach gewünschter Zielplattform und Ihrer Entwicklungsumgebung – versuchen, jeweils separat eine iOS-, eine Android- und eine UWP-Anwendung zu entwickeln.

- Die Xamarin- und Microsoft-Websites stellen hierzu Informationen bereit:
- [Erste Schritte mit iOS](~/ios/get-started/index.md)
- [Erste Schritte mit Android](~/android/get-started/index.md)

[Windows Developer Center](https://dev.windows.com)

## <a name="related-links"></a>Wenn Sie Projekte für die einzelnen Plattformen erstellen und ausführen können, sollten Sie keine Probleme haben, eine Xamarin.Forms-Anwendung zu erstellen und auszuführen.

- Verwandte Links
- [Kapitel 1 – vollständiger Text (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch01-Apr2016.pdf)
