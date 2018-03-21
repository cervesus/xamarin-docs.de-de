---
title: Xamarin.Forms-Seiten
description: "Xamarin.Forms Seiten stellen Bildschirme für plattformübergreifende mobile-Anwendung dar."
ms.topic: article
ms.prod: xamarin
ms.assetid: 9C8C710F-E312-420B-9324-A7A20CEDB7EC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/12/2016
ms.openlocfilehash: 5f979d2dbb894107d8d606ec1f41de44c294cdd3
ms.sourcegitcommit: d450ae06065d8f8c80f3588bc5a614cfd97b5a67
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/21/2018
---
# <a name="xamarinforms-pages"></a>Xamarin.Forms-Seiten

_Xamarin.Forms Seiten stellen Bildschirme für plattformübergreifende mobile-Anwendung dar._

Alle Seitentypen, die unten beschriebenen Ableiten der Xamarin.Forms [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) Klasse. Diese visuelle Elemente belegen alle oder die meisten des Bildschirms. Ein `Page` -Objekt stellt eine `ViewController` in iOS und ein `Page` in universellen Windows-Plattform. Unter Android, füllt jeder Seite der Bildschirm wie ein `Activity`, jedoch werden Seiten Xamarin.Forms *nicht* `Activity` Objekte.

[ ![](pages-images/pages-sml.png "Xamarin.Forms Seitentypen")](pages-images/pages.png#lightbox "Xamarin.Forms Seitentypen")

## <a name="pages"></a>Seiten

Xamarin.Forms unterstützt die folgenden Seite:

<a name="contentPage" />

### <a name="contentpage"></a>ContentPage

|     |     | 
| --- | --- | 
| [`ContentPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) ist die einfachste und häufigste Seite. Festlegen der [ `Content` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ContentPage.Content/) Eigenschaft auf einen einzelnen [ `View` ](views.md) -Objekt, das am häufigsten wird eine [ `Layout` ](layouts.md) wie z. B. [ `StackLayout` ](layouts.md#stackLayout), [ `Grid` ](layouts.md#grid), oder [ `ScrollView` ](layouts.md#scrollView).<br /><br />[API-Dokumentation](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) | [![ContentPage verwendet Beispiel](pages-images/ContentPage.png "ContentPage verwendet Beispiel")](pages-images/ContentPage-Large.png#lightbox "ContentPage verwendet wird")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ContentPageDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ContentPageDemoPage.xaml) |
|     |     |

### <a name="masterdetailpage"></a>MasterDetailPage

|     |     | 
| --- | --- | 
| Ein [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) zwei Informationsbereiche verwaltet. Legen Sie die [ `Master` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Master/) Eigenschaft, um eine Seite in der Regel mit einer Liste oder einem Menü. Legen Sie die [ `Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Detail/) Eigenschaft, um eine Seite mit der ein ausgewähltes Element aus der Masterseite. Die [ `IsPresented` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.IsPresented/) -Eigenschaft steuert, ob die Seite Master oder Detail angezeigt wird.<br /><br />[API-Dokumentation](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) / [Handbuch](~/xamarin-forms/app-fundamentals/navigation/master-detail-page.md) / [Beispiel](https://developer.xamarin.com/samples/xamarin-forms/Navigation/MasterDetailPage/) | [![Beispiel für MasterDetailPage](pages-images/MasterDetailPage.png "MasterDetailPage Beispiel")](pages-images/MasterDetailPage-Large.png#lightbox "MasterDetailPage-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/MasterDetailPageDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/MasterDetailPageDemoPage.xaml) mit [CodeBehind](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/MasterDetailPageDemoPage.xaml.cs) |
|     |     |

### <a name="navigationpage"></a>NavigationPage

|     |     | 
| --- | --- | 
| Die [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) Navigation zwischen den anderen Seiten, die unter Verwendung einer Architektur stapelbasierten verwaltet. Bei der Seitennavigation in Ihrer Anwendung verwenden zu können, sollte eine Instanz von der Startseite "an den Konstruktor des übergeben ein `NavigationPage` Objekt.<br /><br />[API-Dokumentation](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) / [Handbuch](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md) / [Beispiel 1](https://developer.xamarin.com/samples/xamarin-forms/Navigation/Hierarchical/), [2](https://developer.xamarin.com/samples/xamarin-forms/Navigation/PassingData/), und [3](https://developer.xamarin.com/samples/xamarin-forms/Navigation/LoginFlow/)  | [![Beispiel für NavigationPage](pages-images/NavigationPage.png "NavigationPage Beispiel")](pages-images/NavigationPage-Large.png#lightbox "NavigationPage-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/NavigationPageDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/NavigationPageDemoPage.xaml) mit [Code hinter =](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/NavigationPageDemoPage.xaml.cs) |
|     |     |

### <a name="tabbedpage"></a>TabbedPage

|     |     | 
| --- | --- | 
| [`TabbedPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) leitet sich von der abstrakten [ `MultiPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MultiPage%3CT%3E/) -Klasse und ermöglicht das Navigieren zwischen untergeordneten Seiten mit Registerkarten. Legen Sie die [ `Children` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%3CT%3E.Children/) Eigenschaft, um eine Auflistung von Seiten, oder legen Sie die [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%3CT%3E.ItemsSource/) Eigenschaft, um eine Auflistung von Datenobjekten und die [ `ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%3CT%3E.ItemTemplate/) Eigenschaft, um eine [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) beschreiben, wie jedes Objekt ist, visuell dargestellt werden soll.<br /><br />[API-Dokumentation](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) / [Handbuch](~/xamarin-forms/app-fundamentals/navigation/tabbed-page.md) / [Beispiel 1](https://developer.xamarin.com/samples/xamarin-forms/Navigation/TabbedPage/) und [2](https://developer.xamarin.com/samples/xamarin-forms/Navigation/TabbedPageWithNavigationPage) | [![Beispiel für TabbedPage](pages-images/TabbedPage.png "TabbedPage Beispiel")](pages-images/TabbedPage-Large.png#lightbox "TabbedPage-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TabbedPageDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TabbedPageDemoPage.xaml) |
|     |     |

### <a name="carouselpage"></a>CarouselPage

|     |     | 
| --- | --- | 
| [`CarouselPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) leitet sich von der abstrakten [ `MultiPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MultiPage%3CT%3E/) -Klasse und ermöglicht das Navigieren zwischen untergeordneten Seiten über Finger Streifen. Festlegen der [ `Children` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%3CT%3E.Children/) Eigenschaft, um eine Auflistung von [ `ContentPage` ](#contentPage) Objekte oder Satz der [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%3CT%3E.ItemsSource/) Eigenschaft, um eine Auflistung von Datenobjekten und die [ `ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%3CT%3E.ItemTemplate/) Eigenschaft, um eine [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) beschreiben, wie jedes Objekt ist, visuell dargestellt werden soll.<br /><br />[API-Dokumentation](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) / [Handbuch](~/xamarin-forms/app-fundamentals/navigation/carousel-page.md) / [Beispiel 1](https://developer.xamarin.com/samples/xamarin-forms/Navigation/CarouselPage/) und [2](https://developer.xamarin.com/samples/xamarin-forms/Navigation/CarouselPageTemplate/) | [![Beispiel für CarouselPage](pages-images/CarouselPage.png "CarouselPage Beispiel")](pages-images/CarouselPage-Large.png#lightbox "CarouselPage-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/CarouselPageDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/CarouselPageDemoPage.xaml) |
|     |     |

### <a name="templatedpage"></a>TemplatedPage

|     |     | 
| --- | --- | 
| [`TemplatedPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplatedPage/) Zeigt den Vollbild-Inhalt mit einer Steuerelementvorlage und ist die Basisklasse für [ `ContentPage` ](#contentPage).<br /><br />[API-Dokumentation](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplatedPage/) / [Handbuch](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md) | [![Beispiel für TemplatedPage](pages-images/TemplatedPage.png "TemplatedPage Beispiel")](pages-images/TemplatedPage.png "TemplatedPage-Beispiel") |
|     |     |

## <a name="related-links"></a>Verwandte Links

- [Introduction to Xamarin.Forms (Einführung in Xamarin.Forms)](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [Xamarin.Forms FormsGallery-Beispiel](https://developer.xamarin.com/samples/FormsGallery/)
- [Xamarin.Forms Samples (Beispiele für Xamarin.Forms)](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [Xamarin.Forms-API-Dokumentation](https://developer.xamarin.com/api/root/Xamarin.Forms/)
