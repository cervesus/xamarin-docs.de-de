---
title: Xamarin.Forms Pages
description: Xamarin.FormsSeiten stellen plattformübergreifende Mobile Anwendungs Bildschirme dar. In diesem Artikel werden die Seiten aufgelistet, die in enthalten sind Xamarin.Forms .
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: c576186dcfd598cb4fcfecd6d36edf04f73eee64
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2020
ms.locfileid: "84132820"
---
# <a name="xamarinforms-pages"></a>Xamarin.Forms Pages

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery/)

_Xamarin. Forms-Seiten stellen plattformübergreifende Mobile Anwendungs Bildschirme dar._

Alle unten beschriebenen Seiten Typen werden von der-Klasse abgeleitet Xamarin.Forms [`Page`](xref:Xamarin.Forms.Page) . Diese visuellen Elemente belegen den gesamten Bildschirm. Ein `Page` -Objekt stellt ein `ViewController` in IOS und eine `Page` in der universelle Windows-Plattform dar. Unter Android nimmt jede Seite den Bildschirm wie ein `Activity` , aber die Xamarin.Forms Seiten sind *keine* `Activity` Objekte.

[![](pages-images/pages-sml.png "Xamarin.Forms Page Types")](pages-images/pages.png#lightbox "Xamarin.Forms Page Types")

## <a name="pages"></a>Seiten

Xamarin.Formsunterstützt die folgenden Seiten Typen:

<a name="contentPage" />

### <a name="contentpage"></a>Inhaltsseite

|     |     |
| --- | --- |
| [`ContentPage`](xref:Xamarin.Forms.ContentPage)ist die einfachste und häufigste Art von Seite. Legen Sie die- [`Content`](xref:Xamarin.Forms.ContentPage.Content) Eigenschaft auf ein einzelnes- [`View`](views.md) Objekt fest. Dies ist meistens ein, [`Layout`](layouts.md) z [`StackLayout`](layouts.md#stackLayout) . b [`Grid`](layouts.md#grid) ., oder [`ScrollView`](layouts.md#scrollView) .<br /><br />[API-Dokumentation](xref:Xamarin.Forms.ContentPage) | [![ContentPage-Beispiel](pages-images/ContentPage.png "ContentPage-Beispiel")](pages-images/ContentPage-Large.png#lightbox "ContentPage-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ContentPageDemoPage.cs)  /  [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ContentPageDemoPage.xaml) |
|     |     |

### <a name="masterdetailpage"></a>MasterDetailPage

|     |     |
| --- | --- |
| Mit einem werden [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) zwei Informationsbereiche verwaltet. Legen Sie die- [`Master`](xref:Xamarin.Forms.MasterDetailPage.Master) Eigenschaft auf eine Seite fest, die im Allgemeinen eine Liste oder ein Menü anzeigt Legen Sie die- [`Detail`](xref:Xamarin.Forms.MasterDetailPage.Detail) Eigenschaft auf eine Seite fest, die ein ausgewähltes Element auf der Master Seite anzeigt. Die- [`IsPresented`](xref:Xamarin.Forms.MasterDetailPage.IsPresented) Eigenschaft bestimmt, ob die Master-oder Detailseite sichtbar ist.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.MasterDetailPage)  /  [Leitfaden](~/xamarin-forms/app-fundamentals/navigation/master-detail-page.md)  /  [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-masterdetailpage) | [![Masterdetailpage-Beispiel](pages-images/MasterDetailPage.png "Masterdetailpage-Beispiel")](pages-images/MasterDetailPage-Large.png#lightbox "Masterdetailpage-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/MasterDetailPageDemoPage.cs)  /  [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/MasterDetailPageDemoPage.xaml) mit [Code Behind](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/MasterDetailPageDemoPage.xaml.cs) |
|     |     |

### <a name="navigationpage"></a>NavigationPage

|     |     |
| --- | --- |
| Der [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) verwaltet die Navigation zwischen anderen Seiten mithilfe einer Stapel basierten Architektur. Wenn Sie die Seitennavigation in der Anwendung verwenden, sollte eine Instanz der Startseite an den Konstruktor eines-Objekts übergeben werden `NavigationPage` .<br /><br />[API-Dokumentation](xref:Xamarin.Forms.NavigationPage)  /  [Leitfaden](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md)  /  [Beispiel 1](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-hierarchical), [2](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-passingdata)und [3](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-loginflow)  | [![Navigationpage-Beispiel](pages-images/NavigationPage.png "Navigationpage-Beispiel")](pages-images/NavigationPage-Large.png#lightbox "Navigationpage-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/NavigationPageDemoPage.cs)  /  [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/NavigationPageDemoPage.xaml) mit [Code = Behind](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/NavigationPageDemoPage.xaml.cs) |
|     |     |

### <a name="tabbedpage"></a>TabbedPage

|     |     |
| --- | --- |
| [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)wird von der abstrakten [`MultiPage`](xref:Xamarin.Forms.MultiPage`1) -Klasse abgeleitet und ermöglicht die Navigation zwischen untergeordneten Seiten mithilfe von Registerkarten. Legen [`Children`](xref:Xamarin.Forms.MultiPage`1.Children) Sie die-Eigenschaft auf eine Auflistung von Seiten fest, oder legen Sie die [`ItemsSource`](xref:Xamarin.Forms.MultiPage`1.ItemsSource) -Eigenschaft auf eine Auflistung von Datenobjekten und die- [`ItemTemplate`](xref:Xamarin.Forms.MultiPage`1.ItemTemplate) Eigenschaft auf fest, um zu beschreiben, [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) wie jedes Objekt visuell dargestellt werden soll.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.TabbedPage)  /  [Leitfaden](~/xamarin-forms/app-fundamentals/navigation/tabbed-page.md)  /  [Beispiel 1](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-tabbedpage) und [2](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-tabbedpagewithnavigationpage) | [![Tabbedpage-Beispiel](pages-images/TabbedPage.png "Beispiel für TabbedPage")](pages-images/TabbedPage-Large.png#lightbox "Beispiel für TabbedPage")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TabbedPageDemoPage.cs)  /  [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TabbedPageDemoPage.xaml) |
|     |     |

### <a name="carouselpage"></a>CarouselPage

|     |     |
| --- | --- |
| [`CarouselPage`](xref:Xamarin.Forms.CarouselPage)wird von der abstrakten [`MultiPage`](xref:Xamarin.Forms.MultiPage`1) -Klasse abgeleitet und ermöglicht die Navigation zwischen untergeordneten Seiten durch fingerwischen. Legen [`Children`](xref:Xamarin.Forms.MultiPage`1.Children) Sie die-Eigenschaft auf eine Auflistung von [`ContentPage`](#contentPage) -Objekten fest, oder legen Sie die [`ItemsSource`](xref:Xamarin.Forms.MultiPage`1.ItemsSource) -Eigenschaft auf eine Auflistung von Datenobjekten und die- [`ItemTemplate`](xref:Xamarin.Forms.MultiPage`1.ItemTemplate) Eigenschaft auf fest, um zu beschreiben, [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) wie jedes Objekt visuell dargestellt werden soll.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.CarouselPage)  /  [Leitfaden](~/xamarin-forms/app-fundamentals/navigation/carousel-page.md)  /  [Beispiel 1](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-carouselpage) und [2](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/navigation-carouselpagetemplate) | [![Carouselpage-Beispiel](pages-images/CarouselPage.png "Carouselpage-Beispiel")](pages-images/CarouselPage-Large.png#lightbox "Carouselpage-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/CarouselPageDemoPage.cs)  /  [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/CarouselPageDemoPage.xaml) |
|     |     |

### <a name="templatedpage"></a>Templatedpage

|     |     |
| --- | --- |
| [`TemplatedPage`](xref:Xamarin.Forms.TemplatedPage)zeigt voll Bildinhalt mit einer Steuerelement Vorlage an, und ist die Basisklasse für [`ContentPage`](#contentPage) .<br /><br />[API-Dokumentation](xref:Xamarin.Forms.TemplatedPage)  /  [Leitfaden](~/xamarin-forms/app-fundamentals/templates/control-template.md) | [![Templatedpage-Beispiel](pages-images/TemplatedPage.png "Templatedpage-Beispiel")](pages-images/TemplatedPage.png "Templatedpage-Beispiel") |
|     |     |

## <a name="related-links"></a>Verwandte Links

- [Xamarin.FormsFormsgallery-Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)
- [Xamarin.FormsStich](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.Forms)
- [Xamarin.FormsAPI-Dokumentation](https://docs.microsoft.com/dotnet/api/xamarin.forms?view=xamarin-forms)
