---
title: ''
description: Eine Beschreibung aller Benutzeroberflächen Elemente, die zum Erstellen einer- Xamarin.Forms Anwendung verwendet werden. In diesem Artikel werden die Steuerelement Gruppen aufgelistet, aus denen sich die Benutzeroberfläche einer- Xamarin.Forms Anwendung besteht.
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: e843f0e42f4f66a6ce4e60c2f5d8a233d19f1df6
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2020
ms.locfileid: "84136395"
---
# <a name="controls-reference"></a>Steuerelementreferenz

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery/)

Die Benutzeroberfläche einer- Xamarin.Forms Anwendung wird von Objekten erstellt, die den nativen Steuerelementen der einzelnen Zielplattformen zugeordnet werden. Dadurch können plattformspezifische Anwendungen für IOS, Android und das universelle Windows-Plattform Xamarin.Forms in einer [.NET Standard Bibliothek](~/cross-platform/app-fundamentals/net-standard.md)enthaltene Code verwenden.

Die vier Haupt Steuerungsgruppen, die zum Erstellen der Benutzeroberfläche einer- Xamarin.Forms Anwendung verwendet werden, lauten wie folgt:

- [**Seiten**](pages.md)
- [**Layouts**](layouts.md)
- [**Sichten**](views.md)
- [**Zellen**](cells.md)

Eine Xamarin.Forms Seite nimmt in der Regel den gesamten Bildschirm ein. Die Seite enthält normalerweise ein Layout, das Sichten und möglicherweise andere Layouts enthält. Zellen sind spezialisierte Komponenten, die in Verbindung mit und verwendet werden [`TableView`](views.md#tableview) [`ListView`](views.md#listview) . Ein Klassendiagramm, das die Hierarchie von Typen anzeigt, die in der Regel zum Erstellen einer Benutzeroberfläche in verwendet werden, finden Sie Xamarin.Forms unter Steuer [ Xamarin.Forms Elemente-Klassenhierarchie](~/xamarin-forms/internals/class-hierarchy.md).

In den vier Artikeln zu [**Seiten**](pages.md), [**Layouts**](layouts.md), [**Ansichten**](views.md)und [**Zellen**](cells.md)wird jede Art von Steuerelement mit Links zu der zugehörigen API-Dokumentation, einem Artikel zum Beschreiben der Verwendung (falls vorhanden) und einem oder mehreren Beispielprogrammen (sofern vorhanden) beschrieben. Jede Art von Steuerelement wird auch von einem Screenshot begleitet, der eine Seite des [**formsgallery**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery) -Beispiels anzeigt, die auf IOS-und Android-Geräten ausgeführt wird. Unter den einzelnen Screenshots finden Sie Links zum Quellcode der c#-Seite, der entsprechenden XAML-Seite und (falls zutreffend) der c#-Code Behind-Datei für die XAML-Seite.

> [!NOTE]
> Seiten, Layouts und Sichten werden von der- `VisualElement` Klasse abgeleitet. Die- `VisualElement` Klasse bietet eine Vielzahl von Eigenschaften, Methoden und Ereignissen, die beim Ableiten von Klassen nützlich sind. Weitere Informationen finden Sie unter [visualelement-Eigenschaften,-Methoden und-Ereignisse](common-properties.md).

Zusätzlich zu den Steuerelementen, die mit bereitgestellt Xamarin.Forms werden, sind Steuerelemente von Drittanbietern verfügbar. Weitere Informationen finden Sie unter Steuer [Elemente von Drittanbietern](thirdparty.md).

## <a name="related-links"></a>Verwandte Links

- [Xamarin.FormsFormsgallery-Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)
- [Xamarin.FormsControls-Klassenhierarchie](~/xamarin-forms/internals/class-hierarchy.md)
- [API-Dokumentation](https://docs.microsoft.com/dotnet/api/xamarin.forms?view=xamarin-forms)
