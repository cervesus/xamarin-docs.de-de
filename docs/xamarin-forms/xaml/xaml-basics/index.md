---
title: Xamarin. Forms-XAML-Grundlagen
description: In dieser Anleitung wird erläutert, wie Sie plattformübergreifende XAML für mobile Geräte starten. Mit XAML können Entwickler Benutzeroberflächen in xamarin. Forms-Anwendungen mithilfe von Markup anstelle von Code definieren.
ms.prod: xamarin
ms.custom: video
ms.assetid: 67CC2CD6-D10A-4B14-9696-1D3A410EFFBF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/25/2017
ms.openlocfilehash: e8f5a083f49565a00a7ffe4c068d8dbd7815cc8b
ms.sourcegitcommit: efbc69acf4ea484d8815311b058114379c9db8a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73842860"
---
# <a name="xamarinforms-xaml-basics"></a>Xamarin. Forms-XAML-Grundlagen

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xamlsamples)

Die Extensible Application Markup Language (XAML) ist eine XML-basierte Sprache, die von Microsoft als Alternative zum Programmieren von Code zum Instanziieren und Initialisieren von Objekten und organisieren dieser Objekte in über-/unterordnungshierarchien erstellt wird. XAML wurde an verschiedene Technologien innerhalb von .NET Framework angepasst, aber es wurde das größte Hilfsprogramm zum Definieren des Layouts von Benutzeroberflächen in den Windows Presentation Foundation (WPF), Silverlight, Windows-Runtime und den universellen Fenstern gefunden. Plattform (UWP).

Mit XAML können Entwickler Benutzeroberflächen in xamarin. Forms-Anwendungen mithilfe von Markup anstelle von Code definieren. XAML ist in einem xamarin. Forms-Programm nie erforderlich, aber es ist oft eher präglicher und visuell kohärenter als äquivalenter Code und potenziell Toolbar. XAML eignet sich gut für die Verwendung mit der gängigen MVVM (Model-View-ViewModel)-Anwendungsarchitektur: XAML definiert die Ansicht, die mit dem ViewModel-Code durch XAML-basierte Daten Bindungen verknüpft ist.

In einer XAML-Datei kann der xamarin. Forms-Entwickler Benutzeroberflächen definieren, indem er alle xamarin. Forms-Ansichten,-Layouts und-Seiten sowie benutzerdefinierte Klassen verwendet. Die XAML-Datei kann entweder kompiliert oder in die ausführbare Datei eingebettet werden. In beiden Fällen werden die XAML-Informationen zur Buildzeit analysiert, um benannte Objekte zu suchen, und wieder zur Laufzeit, um Objekte zu instanziieren und zu initialisieren und um Links zwischen diesen Objekten und dem Programmiercode herzustellen.

XAML hat mehrere Vorteile gegenüber äquivalenten Code:

- XAML ist oft prägnanter und besser lesbar als äquivalenter Code.
- Die über-/unterordnungshierarchie, die in XML enthalten ist, ermöglicht es XAML, die über-/unterordnungshierarchie von Benutzeroberflächen Objekten zu imitieren.
- XAML kann problemlos von Programmierern Hand geschrieben werden, kann aber auch von den visuellen Entwurfs Tools erstellt und generiert werden.

Es gibt auch Nachteile, die sich hauptsächlich auf Einschränkungen beziehen, die in Markup Sprachen intrinsisch sind:

- XAML darf keinen Code enthalten. Alle Ereignishandler müssen in einer Codedatei definiert werden.
- XAML kann keine Schleifen für die wiederholte Verarbeitung enthalten. (Einige visuelle xamarin. Forms-Objekte – insbesondere [`ListView`](xref:Xamarin.Forms.ListView) – können jedoch auf der Grundlage der Objekte in der `ItemsSource` Auflistung mehrere untergeordnete Elemente generieren.)
- XAML kann keine bedingte Verarbeitung enthalten (eine Datenbindung kann jedoch auf einen Code basierten Bindungs Konverter verweisen, der die bedingte Verarbeitung effektiv zulässt).
- XAML kann im Allgemeinen keine Klassen instanziieren, die keinen Parameter losen Konstruktor definieren. (Manchmal gibt es jedoch eine Möglichkeit, diese Einschränkung zu umgehen.)
- XAML kann im Allgemeinen keine Methoden aufzurufen. (Diese Einschränkung kann manchmal auch überwunden werden.)

Es ist noch kein visueller Designer zum Erstellen von XAML-Code in xamarin. Forms-Anwendungen vorhanden. Alle XAML-Steuerpunkte müssen Hand geschrieben sein, aber es ist ein [XAML-Previewer](~/xamarin-forms/xaml/xaml-previewer/index.md)vorhanden. Programmierer, die noch nicht mit XAML vertraut sind, können Ihre Anwendungen häufig erstellen und ausführen, vor allem, wenn alles möglicherweise nicht korrekt ist. Auch Entwickler mit vielen Funktionen in XAML wissen, dass das Experimentieren lohnenswert ist.

XAML ist im Grunde XML, aber XAML verfügt über einige eindeutige Syntax Funktionen. Die wichtigsten sind:

- Eigenschaften Elemente
- Angefügte Eigenschaften
- Markup Erweiterungen

Diese Funktionen sind *keine* XML-Erweiterungen. XAML ist vollständig gültiger XML-Code. Diese XAML-Syntax Funktionen verwenden jedoch XML auf besondere Weise. Sie werden in den folgenden Artikeln ausführlich erläutert, die eine Einführung in die Verwendung von XAML zum Implementieren von MVVM haben.

## <a name="requirements"></a>Anforderungen

In diesem Artikel wird davon ausgegangen, dass Sie mit xamarin. Forms vertraut sind. Außerdem wird in diesem Artikel eine gewisse Vertrautheit mit XML vorausgesetzt, einschließlich der Verwendung von XML-Namespace Deklarationen und den Begriffen *Element*, *Tag*und *Attribut*.

Wenn Sie mit xamarin. Forms und XML vertraut sind, beginnen Sie mit dem Lesen von [Teil 1. Der Einstieg in XAML](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md).

## <a name="related-links"></a>Verwandte Links

- [Xamlsamples](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xamlsamples)
- [Erstellen Mobile Apps Buchs](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md)
- [Xamarin.Forms Samples (Beispiele für Xamarin.Forms)](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.Forms)

## <a name="related-video"></a>Verwandte Videos

> [!Video https://channel9.msdn.com/Series/Xamarin-101/XamarinForms-UI-with-XAML-5-of-11/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
