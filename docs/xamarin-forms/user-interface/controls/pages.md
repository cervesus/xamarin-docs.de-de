---
title: Xamarin.Forms-Seiten
description: Xamarin.Forms-Seiten stellen die Bildschirme für plattformübergreifende mobile Anwendung dar. Dieser Artikel listet die Seiten, die in Xamarin.Forms enthalten sind.
ms.prod: xamarin
ms.assetid: 9C8C710F-E312-420B-9324-A7A20CEDB7EC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/12/2016
ms.openlocfilehash: fea1db6c65e533692601439f4712d15371740644
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995897"
---
# <a name="xamarinforms-pages"></a>Xamarin.Forms-Seiten

_Xamarin.Forms-Seiten stellen die Bildschirme für plattformübergreifende mobile Anwendung dar._

Alle Seitentypen, die nachfolgend beschriebenen abgeleitet von der Xamarin.Forms [ `Page` ](xref:Xamarin.Forms.Page) Klasse. Diese visuellen Elemente umfassen alle oder die meisten des Bildschirms. Ein `Page` -Objekt stellt eine `ViewController` in iOS und ein `Page` in die universelle Windows-Plattform. Unter Android, verbraucht jede Seite dem Bildschirm, z.B. eine `Activity`, Xamarin.Forms-Seiten sind jedoch *nicht* `Activity` Objekte.

[ ![](pages-images/pages-sml.png "Typen von Xamarin.Forms-Startseite")](pages-images/pages.png#lightbox "Typen von Xamarin.Forms-Startseite")

## <a name="pages"></a>Seiten

Xamarin.Forms unterstützt die folgenden Seite:

<a name="contentPage" />

### <a name="contentpage"></a>ContentPage

|     |     |
| --- | --- |
| [`ContentPage`](xref:Xamarin.Forms.ContentPage) ist der einfachste und am häufigsten verwendete Typ der Seite. Legen Sie die [ `Content` ](xref:Xamarin.Forms.ContentPage.Content) Eigenschaft auf einen einzelnen [ `View` ](views.md) Objekt, das meisten Fällen ist eine [ `Layout` ](layouts.md) wie z. B. [ `StackLayout` ](layouts.md#stackLayout), [ `Grid` ](layouts.md#grid), oder [ `ScrollView` ](layouts.md#scrollView).<br /><br />[API-Dokumentation](xref:Xamarin.Forms.ContentPage) | [![Beispiel für ContentPage](pages-images/ContentPage.png "ContentPage Beispiel")](pages-images/ContentPage-Large.png#lightbox "ContentPage-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ContentPageDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ContentPageDemoPage.xaml) |
|     |     |

### <a name="masterdetailpage"></a>MasterDetailPage

|     |     |
| --- | --- |
| Ein [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) verwaltet zwei Bereiche von Informationen. Legen Sie die [ `Master` ](xref:Xamarin.Forms.MasterDetailPage.Master) Eigenschaft, um eine Seite in der Regel mit einer Liste oder einem Menü. Legen Sie die [ `Detail` ](xref:Xamarin.Forms.MasterDetailPage.Detail) Eigenschaft, um eine Seite mit der Masterseite ein ausgewähltes Element. Die [ `IsPresented` ](xref:Xamarin.Forms.MasterDetailPage.IsPresented) -Eigenschaft steuert, ob die Master- oder Detail-Seite sichtbar ist.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.MasterDetailPage) / [Handbuch](~/xamarin-forms/app-fundamentals/navigation/master-detail-page.md) / [Beispiel](https://developer.xamarin.com/samples/xamarin-forms/Navigation/MasterDetailPage/) | [![Beispiel für MasterDetailPage](pages-images/MasterDetailPage.png "MasterDetailPage Beispiel")](pages-images/MasterDetailPage-Large.png#lightbox "MasterDetailPage-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/MasterDetailPageDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/MasterDetailPageDemoPage.xaml) mit [CodeBehind](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/MasterDetailPageDemoPage.xaml.cs) |
|     |     |

### <a name="navigationpage"></a>"NavigationPage"

|     |     |
| --- | --- |
| Die [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) verwaltet die Navigation zwischen den anderen Seiten, die mit einer stapelbasierten-Architektur. Bei der Seitennavigation in Ihrer Anwendung verwenden zu können, sollte eine Instanz der Startseite an den Konstruktor übergeben werden eine `NavigationPage` Objekt.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.NavigationPage) / [Handbuch](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md) / [Beispiel 1](https://developer.xamarin.com/samples/xamarin-forms/Navigation/Hierarchical/), [2](https://developer.xamarin.com/samples/xamarin-forms/Navigation/PassingData/), und [3](https://developer.xamarin.com/samples/xamarin-forms/Navigation/LoginFlow/)  | [![Beispiel für die "NavigationPage"](pages-images/NavigationPage.png "\"NavigationPage\" Beispiel")](pages-images/NavigationPage-Large.png#lightbox "\"NavigationPage\"-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/NavigationPageDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/NavigationPageDemoPage.xaml) mit [Code hinter =](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/NavigationPageDemoPage.xaml.cs) |
|     |     |

### <a name="tabbedpage"></a>"Tabbedpage"

|     |     |
| --- | --- |
| [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) leitet sich von der abstrakten [ `MultiPage` ](xref:Xamarin.Forms.MultiPage`1) -Klasse und ermöglicht die Navigation zwischen den untergeordneten Seiten, die mithilfe von Registerkarten. Legen Sie die [ `Children` ](xref:Xamarin.Forms.MultiPage`1.Children) Eigenschaft, um eine Auflistung von Seiten, oder legen die [ `ItemsSource` ](xref:Xamarin.Forms.MultiPage`1.ItemsSource) Eigenschaft, um eine Auflistung von Datenobjekten und die [ `ItemTemplate` ](xref:Xamarin.Forms.MultiPage`1.ItemTemplate) Eigenschaft, um eine [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) beschreibt, wie jedes Objekt ist, visuell dargestellt werden soll.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.TabbedPage) / [Handbuch](~/xamarin-forms/app-fundamentals/navigation/tabbed-page.md) / [Beispiel 1](https://developer.xamarin.com/samples/xamarin-forms/Navigation/TabbedPage/) und [2](https://developer.xamarin.com/samples/xamarin-forms/Navigation/TabbedPageWithNavigationPage) | [![Beispiel für die "tabbedpage"](pages-images/TabbedPage.png "\"tabbedpage\" Beispiel")](pages-images/TabbedPage-Large.png#lightbox "\"tabbedpage\"-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TabbedPageDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TabbedPageDemoPage.xaml) |
|     |     |

### <a name="carouselpage"></a>CarouselPage

|     |     |
| --- | --- |
| [`CarouselPage`](xref:Xamarin.Forms.CarouselPage) leitet sich von der abstrakten [ `MultiPage` ](xref:Xamarin.Forms.MultiPage`1) -Klasse und ermöglicht die Navigation zwischen den untergeordneten Seiten über die Finger Wischen. Legen Sie die [ `Children` ](xref:Xamarin.Forms.MultiPage`1.Children) Eigenschaft, um eine Auflistung von [ `ContentPage` ](#contentPage) Objekte, oder legen die [ `ItemsSource` ](xref:Xamarin.Forms.MultiPage`1.ItemsSource) Eigenschaft, um eine Auflistung von Datenobjekten und die [ `ItemTemplate` ](xref:Xamarin.Forms.MultiPage`1.ItemTemplate) Eigenschaft, um eine [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) beschreibt, wie jedes Objekt ist, visuell dargestellt werden soll.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.CarouselPage) / [Handbuch](~/xamarin-forms/app-fundamentals/navigation/carousel-page.md) / [Beispiel 1](https://developer.xamarin.com/samples/xamarin-forms/Navigation/CarouselPage/) und [2](https://developer.xamarin.com/samples/xamarin-forms/Navigation/CarouselPageTemplate/) | [![Beispiel für CarouselPage](pages-images/CarouselPage.png "CarouselPage Beispiel")](pages-images/CarouselPage-Large.png#lightbox "CarouselPage-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/CarouselPageDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/CarouselPageDemoPage.xaml) |
|     |     |

### <a name="templatedpage"></a>TemplatedPage

|     |     |
| --- | --- |
| [`TemplatedPage`](xref:Xamarin.Forms.TemplatedPage) Vollbild-Inhalt mit einer Steuerelementvorlage angezeigt und ist die Basisklasse für [ `ContentPage` ](#contentPage).<br /><br />[API-Dokumentation](xref:Xamarin.Forms.TemplatedPage) / [Handbuch](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md) | [![Beispiel für TemplatedPage](pages-images/TemplatedPage.png "TemplatedPage Beispiel")](pages-images/TemplatedPage.png "TemplatedPage-Beispiel") |
|     |     |

## <a name="related-links"></a>Verwandte Links

- [Introduction to Xamarin.Forms (Einführung in Xamarin.Forms)](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [Beispiel für Xamarin.Forms FormsGallery](https://developer.xamarin.com/samples/FormsGallery/)
- [Xamarin.Forms Samples (Beispiele für Xamarin.Forms)](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [Xamarin.Forms-API-Dokumentation](https://docs.microsoft.com/dotnet/api/xamarin.forms?view=xamarin-forms)
