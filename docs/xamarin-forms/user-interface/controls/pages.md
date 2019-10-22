---
title: Xamarin. Forms-Seiten
description: Xamarin. Forms-Seiten stellen plattformübergreifende Mobile Anwendungs Bildschirme dar. In diesem Artikel werden die Seiten aufgelistet, die in xamarin. Forms enthalten sind.
ms.prod: xamarin
ms.assetid: 9C8C710F-E312-420B-9324-A7A20CEDB7EC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/12/2016
ms.openlocfilehash: 278256f75d94fe47510ae4d15f12a3ff3a6a2b19
ms.sourcegitcommit: 9bfedf07940dad7270db86767eb2cc4007f2a59f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2019
ms.locfileid: "69976449"
---
# <a name="xamarinforms-pages"></a>Xamarin. Forms-Seiten

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery/)

_Xamarin. Forms-Seiten stellen plattformübergreifende Mobile Anwendungs Bildschirme dar._

Alle unten beschriebenen Seiten Typen werden von der xamarin. Forms- [`Page`](xref:Xamarin.Forms.Page) -Klasse abgeleitet. Diese visuellen Elemente belegen den gesamten Bildschirm. Ein `Page`-Objekt stellt eine `ViewController` in IOS und eine `Page` in der universelle Windows-Plattform dar. Unter Android nimmt jede Seite den Bildschirm wie eine `Activity` auf, aber xamarin. Forms-Seiten sind *nicht* `Activity` Objekte.

[![](pages-images/pages-sml.png "Xamarin.Forms Page Types")](pages-images/pages.png#lightbox "Xamarin.Forms Page Types")

## <a name="pages"></a>Seiten

Xamarin. Forms unterstützt die folgenden Seiten Typen:

<a name="contentPage" />

### <a name="contentpage"></a>ContentPage

|     |     |
| --- | --- |
| [`ContentPage`](xref:Xamarin.Forms.ContentPage) ist die einfachste und gängigste Art von Seite. Legen Sie die [`Content`](xref:Xamarin.Forms.ContentPage.Content) -Eigenschaft auf ein einzelnes [`View`](views.md) Objekt fest, das meistens eine [`Layout`](layouts.md) wie [`StackLayout`](layouts.md#stackLayout), [`Grid`](layouts.md#grid)oder [1](layouts.md#scrollView)ist.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.ContentPage) | [![ContentPage-Beispiel](pages-images/ContentPage.png "ContentPage-Beispiel")](pages-images/ContentPage-Large.png#lightbox "ContentPage-Beispiel")<br />Code für diese Seite  / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ContentPageDemoPage.xaml) [ C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ContentPageDemoPage.cs) |
|     |     |

### <a name="masterdetailpage"></a>MasterDetailPage

|     |     |
| --- | --- |
| Eine [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) die zwei Informationsbereiche verwaltet. Legen Sie die [`Master`](xref:Xamarin.Forms.MasterDetailPage.Master) -Eigenschaft auf eine Seite fest, die eine Liste oder ein Menü anzeigt. Legen Sie die [`Detail`](xref:Xamarin.Forms.MasterDetailPage.Detail) -Eigenschaft auf eine Seite fest, die ein ausgewähltes Element auf der Master Seite anzeigt. Die [`IsPresented`](xref:Xamarin.Forms.MasterDetailPage.IsPresented) -Eigenschaft bestimmt, ob die Master-oder Detailseite sichtbar ist.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.MasterDetailPage)  / [Handbuch](~/xamarin-forms/app-fundamentals/navigation/master-detail-page.md)  / [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-masterdetailpage) | [![Masterdetailpage-Beispiel](pages-images/MasterDetailPage.png "Masterdetailpage-Beispiel")](pages-images/MasterDetailPage-Large.png#lightbox "Masterdetailpage-Beispiel")<br />Code für diese Seite  / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/MasterDetailPageDemoPage.xaml) mit [Code Behind](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/MasterDetailPageDemoPage.xaml.cs) [ C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/MasterDetailPageDemoPage.cs) |
|     |     |

### <a name="navigationpage"></a>NavigationPage

|     |     |
| --- | --- |
| Der [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) verwaltet die Navigation zwischen anderen Seiten mithilfe einer Stapel basierten Architektur. Wenn Sie die Seitennavigation in der Anwendung verwenden, sollte eine Instanz der Startseite an den Konstruktor eines `NavigationPage` Objekts übergeben werden.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.NavigationPage)  / [Handbuch](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md)  / [Beispiel 1](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-hierarchical), [2](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-passingdata)und [3](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-loginflow)  | [![Navigationpage-Beispiel](pages-images/NavigationPage.png "Navigationpage-Beispiel")](pages-images/NavigationPage-Large.png#lightbox "Navigationpage-Beispiel")<br />Code für diese Seite  / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/NavigationPageDemoPage.xaml) mit [Code = Behind](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/NavigationPageDemoPage.xaml.cs) [ C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/NavigationPageDemoPage.cs) |
|     |     |

### <a name="tabbedpage"></a>TabbedPage

|     |     |
| --- | --- |
| [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) von der abstrakten [`MultiPage`](xref:Xamarin.Forms.MultiPage`1) -Klasse abgeleitet und ermöglicht die Navigation zwischen untergeordneten Seiten mithilfe von Registerkarten. Legen Sie die [`Children`](xref:Xamarin.Forms.MultiPage`1.Children) -Eigenschaft auf eine Auflistung von Seiten fest, oder legen Sie die [`ItemsSource`](xref:Xamarin.Forms.MultiPage`1.ItemsSource) -Eigenschaft auf eine Auflistung von Datenobjekten und die [`ItemTemplate`](xref:Xamarin.Forms.MultiPage`1.ItemTemplate) -Eigenschaft auf einen [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) fest, der beschreibt, wie jedes Objekt visuell dargestellt werden soll.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.TabbedPage)  / [Handbuch](~/xamarin-forms/app-fundamentals/navigation/tabbed-page.md)  / [Beispiel 1](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-tabbedpage) und [2](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-tabbedpagewithnavigationpage) | [![Tabbedpage-Beispiel](pages-images/TabbedPage.png "Tabbedpage-Beispiel")](pages-images/TabbedPage-Large.png#lightbox "Tabbedpage-Beispiel")<br />Code für diese Seite  / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TabbedPageDemoPage.xaml) [ C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TabbedPageDemoPage.cs) |
|     |     |

### <a name="carouselpage"></a>CarouselPage

|     |     |
| --- | --- |
| [`CarouselPage`](xref:Xamarin.Forms.CarouselPage) von der abstrakten [`MultiPage`](xref:Xamarin.Forms.MultiPage`1) -Klasse abgeleitet und ermöglicht die Navigation zwischen untergeordneten Seiten durch fingerwischen. Legen Sie die [`Children`](xref:Xamarin.Forms.MultiPage`1.Children) -Eigenschaft auf eine Auflistung von [`ContentPage`](#contentPage) Objekten fest, oder legen Sie die Eigenschaft [`ItemsSource`](xref:Xamarin.Forms.MultiPage`1.ItemsSource) auf eine Auflistung von Datenobjekten und die [`ItemTemplate`](xref:Xamarin.Forms.MultiPage`1.ItemTemplate) -Eigenschaft auf einen [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) fest, der beschreibt, wie die einzelnen Objekte visuell sein sollen. Re.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.CarouselPage)  / [Handbuch](~/xamarin-forms/app-fundamentals/navigation/carousel-page.md)  / [Beispiel 1](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-carouselpage) und [2](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-carouselpagetemplate) | [![Carouselpage-Beispiel](pages-images/CarouselPage.png "Carouselpage-Beispiel")](pages-images/CarouselPage-Large.png#lightbox "Carouselpage-Beispiel")<br />Code für diese Seite  / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/CarouselPageDemoPage.xaml) [ C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/CarouselPageDemoPage.cs) |
|     |     |

### <a name="templatedpage"></a>Templatedpage

|     |     |
| --- | --- |
| [`TemplatedPage`](xref:Xamarin.Forms.TemplatedPage) zeigt voll Bildinhalte mit einer Steuerelement Vorlage an, und ist die Basisklasse für [`ContentPage`](#contentPage).<br /><br />[API-Dokumentation](xref:Xamarin.Forms.TemplatedPage)  / [Handbuch](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md) | [![Templatedpage-Beispiel](pages-images/TemplatedPage.png "Templatedpage-Beispiel")](pages-images/TemplatedPage.png "Templatedpage-Beispiel") |
|     |     |

## <a name="related-links"></a>Verwandte Links

- [Xamarin. Forms formsgallery-Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)
- [Xamarin.Forms Samples (Beispiele für Xamarin.Forms)](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.Forms)
- [Xamarin.Forms-API-Dokumentation](https://docs.microsoft.com/dotnet/api/xamarin.forms?view=xamarin-forms)
