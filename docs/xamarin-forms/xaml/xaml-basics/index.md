---
Title: " Xamarin.Forms XAML-Grundlagen" Beschreibung: "in diesem Handbuch wird erläutert, wie Sie plattformübergreifende XAML für mobile Geräte starten. Mit XAML können Entwickler Benutzeroberflächen in Xamarin.Forms Anwendungen mit Markup anstelle von Code definieren. "
ms. Prod: xamarin ms. Custom: Video ms. assetid: 67cc2cd6-d10a-4b14-9696-1d3a410effbf ms. Technology: xamarin-Forms Author: davidbritch ms. Author: dabritch ms. Date: 10/25/2017 NO-LOC: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="xamarinforms-xaml-basics"></a>Xamarin.FormsGrundlagen zu XAML

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xamlsamples)

Die Extensible Application Markup Language (XAML) ist eine XML-basierte Sprache, die von Microsoft als Alternative zum Programmieren von Code zum Instanziieren und Initialisieren von Objekten und organisieren dieser Objekte in über-/unterordnungshierarchien erstellt wird. XAML wurde an verschiedene Technologien in .NET Framework angepasst, aber es wurde das größte Hilfsprogramm zum Definieren des Layouts von Benutzeroberflächen in den Windows Presentation Foundation (WPF), Silverlight, Windows-Runtime und universelle Windows-Plattform (UWP) gefunden.

Mit XAML können Entwickler Benutzeroberflächen in Xamarin.Forms Anwendungen mit Markup anstelle von Code definieren. XAML ist in einem Programm nie erforderlich Xamarin.Forms , aber es ist oft eher präglicher und visuell kohärenter als äquivalenter Code und potenziell Toolbar. XAML eignet sich gut für die Verwendung mit der gängigen MVVM (Model-View-ViewModel)-Anwendungsarchitektur: XAML definiert die Ansicht, die mit dem ViewModel-Code durch XAML-basierte Daten Bindungen verknüpft ist.

In einer XAML-Datei Xamarin.Forms kann der Entwickler Benutzeroberflächen definieren, indem er alle Xamarin.Forms Sichten, Layouts und Seiten sowie benutzerdefinierte Klassen verwendet. Die XAML-Datei kann entweder kompiliert oder in die ausführbare Datei eingebettet werden. In beiden Fällen werden die XAML-Informationen zur Buildzeit analysiert, um benannte Objekte zu suchen, und wieder zur Laufzeit, um Objekte zu instanziieren und zu initialisieren und um Links zwischen diesen Objekten und dem Programmiercode herzustellen.

XAML hat mehrere Vorteile gegenüber äquivalenten Code:

- XAML ist oft prägnanter und besser lesbar als äquivalenter Code.
- Die über-/unterordnungshierarchie, die in XML enthalten ist, ermöglicht es XAML, die über-/unterordnungshierarchie von Benutzeroberflächen Objekten zu imitieren.
- XAML kann problemlos von Programmierern Hand geschrieben werden, kann aber auch von den visuellen Entwurfs Tools erstellt und generiert werden.

Es gibt auch Nachteile, die sich hauptsächlich auf Einschränkungen beziehen, die in Markup Sprachen intrinsisch sind:

- XAML darf keinen Code enthalten. Alle Ereignishandler müssen in einer Codedatei definiert werden.
- XAML kann keine Schleifen für die wiederholte Verarbeitung enthalten. ( Xamarin.Forms Es können jedoch mehrere visuelle Objekte – insbesondere [`ListView`](xref:Xamarin.Forms.ListView) –, basierend auf den Objekten in der Auflistung mehrere untergeordnete Objekte generieren `ItemsSource` .)
- XAML kann keine bedingte Verarbeitung enthalten (eine Datenbindung kann jedoch auf einen Code basierten Bindungs Konverter verweisen, der die bedingte Verarbeitung effektiv zulässt).
- XAML kann im Allgemeinen keine Klassen instanziieren, die keinen Parameter losen Konstruktor definieren. (Manchmal gibt es jedoch eine Möglichkeit, diese Einschränkung zu umgehen.)
- XAML kann im Allgemeinen keine Methoden aufzurufen. (Diese Einschränkung kann manchmal auch überwunden werden.)

Es gibt noch keinen visuellen Designer zum Erstellen von XAML-Code in Xamarin.Forms Anwendungen. Alle XAML-Steuerpunkte müssen Hand geschrieben sein, aber es ist ein [XAML-Previewer](~/xamarin-forms/xaml/xaml-previewer/index.md)vorhanden. Programmierer, die noch nicht mit XAML vertraut sind, können Ihre Anwendungen häufig erstellen und ausführen, vor allem, wenn alles möglicherweise nicht korrekt ist. Auch Entwickler mit vielen Funktionen in XAML wissen, dass das Experimentieren lohnenswert ist.

XAML ist im Grunde XML, aber XAML verfügt über einige eindeutige Syntax Funktionen. Die wichtigsten sind:

- Eigenschaften Elemente
- Angefügte Eigenschaften
- Markuperweiterungen

Diese Funktionen sind *keine* XML-Erweiterungen. XAML ist vollständig gültiger XML-Code. Diese XAML-Syntax Funktionen verwenden jedoch XML auf besondere Weise. Sie werden in den folgenden Artikeln ausführlich erläutert, die eine Einführung in die Verwendung von XAML zum Implementieren von MVVM haben.

## <a name="requirements"></a>Requirements (Anforderungen)

In diesem Artikel wird davon ausgegangen, dass Sie mit Arbeiten Xamarin.Forms . Außerdem wird in diesem Artikel eine gewisse Vertrautheit mit XML vorausgesetzt, einschließlich der Verwendung von XML-Namespace Deklarationen und den Begriffen *Element*, *Tag*und *Attribut*.

Wenn Sie mit und XML vertraut sind, beginnen Sie mit dem Xamarin.Forms Lesen von [Teil 1. Der Einstieg in XAML](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md).

## <a name="related-links"></a>Verwandte Links

- [Xamlsamples](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xamlsamples)
- [Erstellen Mobile Apps Buchs](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md)
- [Xamarin.Forms-Beispiele](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.Forms)

## <a name="related-video"></a>Zugehörige Videos

> [!Video https://channel9.msdn.com/Series/Xamarin-101/XamarinForms-UI-with-XAML-5-of-11/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
