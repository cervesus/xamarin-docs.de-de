---
title: Steuerelementreferenz
description: 'Eine Beschreibung aller Benutzeroberflächen Elemente, die zum Erstellen einer- :::no-loc(Xamarin.Forms)::: Anwendung verwendet werden. In diesem Artikel werden die Steuerelement Gruppen aufgelistet, aus denen sich die Benutzeroberfläche einer- :::no-loc(Xamarin.Forms)::: Anwendung besteht.'
ms.prod: xamarin
ms.assetid: F2A02DEE-7137-42F4-9C0A-4E1CF75EA08F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/08/2019
no-loc:
- ':::no-loc(Xamarin.Forms):::'
- ':::no-loc(Xamarin.Essentials):::'
ms.openlocfilehash: b9e3f8b61ebfc73a26a967b83f60e005a652563f
ms.sourcegitcommit: 952db1983c0bc373844c5fbe9d185e04a87d8fb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "86997372"
---
# <a name="controls-reference"></a>Steuerelementreferenz

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery/)

Die Benutzeroberfläche einer- :::no-loc(Xamarin.Forms)::: Anwendung wird von Objekten erstellt, die den nativen Steuerelementen der einzelnen Zielplattformen zugeordnet werden. Dadurch können plattformspezifische Anwendungen für IOS, Android und das universelle Windows-Plattform :::no-loc(Xamarin.Forms)::: in einer [.NET Standard Bibliothek](~/cross-platform/app-fundamentals/net-standard.md)enthaltene Code verwenden.

Die vier Haupt Steuerungsgruppen, die zum Erstellen der Benutzeroberfläche einer- :::no-loc(Xamarin.Forms)::: Anwendung verwendet werden, lauten wie folgt:

- [**Seiten**](pages.md)
- [**Layouts**](layouts.md)
- [**Ansichten**](views.md)
- [**Zellen**](cells.md)

Eine :::no-loc(Xamarin.Forms)::: Seite nimmt in der Regel den gesamten Bildschirm ein. Die Seite enthält normalerweise ein Layout, das Sichten und möglicherweise andere Layouts enthält. Zellen sind spezialisierte Komponenten, die in Verbindung mit und verwendet werden [`TableView`](xref::::no-loc(Xamarin.Forms):::.TableView) [`ListView`](xref::::no-loc(Xamarin.Forms):::.ListView) . Ein Klassendiagramm, das die Hierarchie von Typen anzeigt, die in der Regel zum Erstellen einer Benutzeroberfläche in verwendet werden, finden Sie :::no-loc(Xamarin.Forms)::: unter Steuer [ :::no-loc(Xamarin.Forms)::: Elemente-Klassenhierarchie](~/xamarin-forms/internals/class-hierarchy.md).

In den vier Artikeln zu [**Seiten**](pages.md), [**Layouts**](layouts.md), [**Ansichten**](views.md)und [**Zellen**](cells.md)wird jede Art von Steuerelement mit Links zu der zugehörigen API-Dokumentation, einem Artikel zum Beschreiben der Verwendung (falls vorhanden) und einem oder mehreren Beispielprogrammen (sofern vorhanden) beschrieben. Jede Art von Steuerelement wird auch von einem Screenshot begleitet, der eine Seite des [**formsgallery**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery) -Beispiels anzeigt, die auf IOS-und Android-Geräten ausgeführt wird. Unter den einzelnen Screenshots finden Sie Links zum Quellcode der c#-Seite, der entsprechenden XAML-Seite und (falls zutreffend) der c#-Code Behind-Datei für die XAML-Seite.

> [!NOTE]
> Seiten, Layouts und Sichten werden von der- `VisualElement` Klasse abgeleitet. Die- `VisualElement` Klasse bietet eine Vielzahl von Eigenschaften, Methoden und Ereignissen, die beim Ableiten von Klassen nützlich sind. Weitere Informationen finden Sie unter [visualelement-Eigenschaften,-Methoden und-Ereignisse](common-properties.md).

Zusätzlich zu den Steuerelementen, die mit bereitgestellt :::no-loc(Xamarin.Forms)::: werden, sind Steuerelemente von Drittanbietern verfügbar. Weitere Informationen finden Sie unter Steuer [Elemente von Drittanbietern](thirdparty.md).

## <a name="related-links"></a>Verwandte Links

- [:::no-loc(Xamarin.Forms):::Formsgallery-Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)
- [:::no-loc(Xamarin.Forms):::Controls-Klassenhierarchie](~/xamarin-forms/internals/class-hierarchy.md)
- [API-Dokumentation](https://docs.microsoft.com/dotnet/api/xamarin.forms?view=xamarin-forms)
