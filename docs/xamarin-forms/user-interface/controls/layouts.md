---
title: Xamarin.Forms-Layouts
description: Xamarin.Forms-Layouts werden verwendet, um Steuerelemente der Benutzeroberfläche in visual Strukturen zu erstellen. Dieser Artikel beschreibt die Layouts, die in Xamarin.Forms enthalten.
ms.prod: xamarin
ms.assetid: F4180997-BA21-453A-9958-D1E2940DF050
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/21/2018
ms.openlocfilehash: 82ca106f29eb28672abcbd282b60841bfdb4da8c
ms.sourcegitcommit: 41a029c69925e3a9d2de883751ebfd649e8747cd
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2019
ms.locfileid: "68980832"
---
# <a name="xamarinforms-layouts"></a>Xamarin.Forms-Layouts

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)

_Xamarin.Forms-Layouts werden verwendet, um Benutzeroberflächen-Steuerelemente in visual Strukturen zu erstellen._

Die [ `Layout` ](xref:Xamarin.Forms.Layout) und [ `Layout<T>` ](xref:Xamarin.Forms.Layout`1) Klassen in Xamarin.Forms sind spezielle Untertypen von Ansichten, die als Container für Ansichten und anderer Layouts zu fungieren. Die `Layout` Klasse leitet sich von [ `View` ](views.md). Ein `Layout` Ableitung enthält in der Regel die Logik, um die Position und Größe der untergeordneten Elemente in Xamarin.Forms-Anwendungen festlegen.

[![Typen von Xamarin.Forms-Layouts](layouts-images/layouts-sml.png "Typen von Xamarin.Forms-Layouts")](layouts-images/layouts.png#lightbox "Typen von Xamarin.Forms-Layouts")

Die von abgeleiteten Klassen `Layout` kann in zwei Kategorien unterteilt werden:

## <a name="layouts-with-single-content"></a>Layouts mit einzelnen Inhalt

Diese Klassen werden aus [ `Layout` ](xref:Xamarin.Forms.Layout), die definiert, [ `Padding` ](xref:Xamarin.Forms.Layout.Padding) und [ `IsClippedToBounds` ](xref:Xamarin.Forms.Layout.IsClippedToBounds) Eigenschaften.

<a name="contentView" />

### <a name="contentview"></a>ContentView

|     |     |
| --- | --- |
| [`ContentView`](xref:Xamarin.Forms.ContentView) enthält ein einzelnes untergeordnetes Element, das mit festgelegt ist die [ `Content` ](xref:Xamarin.Forms.ContentView.Content) Eigenschaft. Die `Content` Eigenschaft kann festgelegt werden, um alle `View` Ableitung, einschließlich anderer `Layout` ableitungen. `ContentView` wird hauptsächlich als ein strukturelles Element und dient als Basisklasse hinzu [ `Frame` ](#frame).<br /><br />[API-Dokumentation](xref:Xamarin.Forms.ContentView) | [![Beispiel für ContentView](layouts-images/ContentView.png "ContentView Beispiel")](layouts-images/ContentView-Large.png#lightbox "ContentView-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ContentViewDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ContentViewDemoPage.xaml) |
|     |     |

<a named="frame" />

### <a name="frame"></a>Frame

|     |     |
| --- | --- |
| Die [`Frame`](xref:Xamarin.Forms.Frame) -Klasse wird [`ContentView`](#contentView) von abgeleitet und zeigt einen Rahmen oder einen Rahmen um das untergeordnete Element an. Die `Frame` -Klasse hat den [`Padding`](xref:Xamarin.Forms.Layout.Padding) Standardwert 20 und definiert [`BorderColor`](xref:Xamarin.Forms.Frame.BorderColor)auch die Eigenschaften [`CornerRadius`](xref:Xamarin.Forms.Frame.CornerRadius), und [`HasShadow`](xref:Xamarin.Forms.Frame.HasShadow) .<br /><br />[API-Dokumentation](xref:Xamarin.Forms.Frame) / [Handbuch](~/xamarin-forms/user-interface/layouts/frame.md) / [Beispiel](https://docs.microsoft.com/en-us/samples/xamarin/xamarin-forms-samples/userinterface-frame/) | [![Frame-Beispiel](layouts-images/Frame.png "Frame-Beispiel")](layouts-images/Frame-Large.png#lightbox "Frame-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/FrameDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/FrameDemoPage.xaml) |
|     |     |

<a name="scrollView" />

### <a name="scrollview"></a>ScrollView

|     |     |
| --- | --- |
| [`ScrollView`](xref:Xamarin.Forms.ScrollView) ist in der Lage des Bildlaufs an seinen Inhalt. Legen Sie die [ `Content` ](xref:Xamarin.Forms.ScrollView.Content) Eigenschaft auf eine Sicht oder Layout zu groß, um auf dem Bildschirm zu passen. (Der Inhalt des eine `ScrollView` sehr häufig wird eine [ `StackLayout` ](#stackLayout).) Legen Sie die [ `Orientation` ](xref:Xamarin.Forms.ScrollView.Orientation) Eigenschaft, um anzugeben, wenn vertikalen, Durchführen eines Bildlaufs werden soll Horizontal oder beides.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.ScrollView) / [Handbuch](~/xamarin-forms/user-interface/layouts/scroll-view.md) / [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout) | [![Beispiel für ScrollView](layouts-images/ScrollView.png "ScrollView-Beispiel")](layouts-images/ScrollView-Large.png#lightbox "ScrollView-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ScrollViewDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ScrollViewDemoPage.xaml) |
|     |     |

### <a name="templatedview"></a>TemplatedView

|     |     |
| --- | --- |
| [`TemplatedView`](xref:Xamarin.Forms.TemplatedView) Zeigt den Inhalt mit einer Steuerelementvorlage und ist die Basisklasse für [ `ContentView` ](#contentView).<br /><br />[API-Dokumentation](xref:Xamarin.Forms.TemplatedView) / [Handbuch](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md) | [![Beispiel für TemplatedView](layouts-images/TemplatedView.png "TemplatedView Beispiel")](layouts-images/TemplatedView.png#lightbox "TemplatedView-Beispiel") |
|     |     |

### <a name="contentpresenter"></a>ContentPresenter-Element

|     |     |
| --- | --- |
| [`ContentPresenter`](xref:Xamarin.Forms.ContentPresenter) ist ein Layout-Manager für auf Vorlagen basierenden Ansichten, die innerhalb einer [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) , markieren Sie der Inhalt, der angezeigt werden, wobei angezeigt wird.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.ContentPresenter) / [Handbuch](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md) | [![ContentPresenter-Beispiel](layouts-images/ContentPresenter.png "ContentPresenter Beispiel")](layouts-images/ContentPresenter.png#lightbox "ContentPresenter-Beispiel") |
|     |     |

## <a name="layouts-with-multiple-children"></a>Layouts mit mehreren untergeordneten Elementen

Diese Klassen werden aus [ `Layout<View>` ](xref:Xamarin.Forms.Layout`1).

<a name="stackLayout" />

### <a name="stacklayout"></a>StackLayout

|     |     |
| --- | --- |
| [`StackLayout`](xref:Xamarin.Forms.StackLayout) Positioniert untergeordnete Elemente in einem Stapel entweder horizontal oder vertikal basierend auf den [ `Orientation` ](xref:Xamarin.Forms.StackLayout.Orientation) Eigenschaft. Die [ `Spacing` ](xref:Xamarin.Forms.StackLayout.Spacing) Eigenschaft bestimmt den Abstand zwischen den untergeordneten Elementen und hat den Standardwert von 6.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.StackLayout) / [Handbuch](~/xamarin-forms/user-interface/layouts/stack-layout.md) / [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout)| [![Beispiel für StackLayout](layouts-images/StackLayout.png "StackLayout Beispiel")](layouts-images/StackLayout-Large.png#lightbox "StackLayout-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/StackLayoutDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/StackLayoutDemoPage.xaml) |
|     |     |

<a name="grid" />

### <a name="grid"></a>Raster

|     |     |
| --- | --- |
| [`Grid`](xref:Xamarin.Forms.Grid) positioniert die untergeordneten Elemente in einem Raster mit Zeilen und Spalten an. Ein untergeordnetes Element des Position wird angegeben, mit der [angefügte Eigenschaften](~/xamarin-forms/xaml/attached-properties.md) [ `Row` ](xref:Xamarin.Forms.Grid.RowProperty), [ `Column` ](xref:Xamarin.Forms.Grid.ColumnProperty), [ `RowSpan` ](xref:Xamarin.Forms.Grid.RowSpanProperty), und [ `ColumnSpan` ](xref:Xamarin.Forms.Grid.ColumnSpanProperty).<br /><br />[API-Dokumentation](xref:Xamarin.Forms.Grid) / [Handbuch](~/xamarin-forms/user-interface/layouts/grid.md) / [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout) | [![Rasterbeispiel](layouts-images/Grid.png "Rasterbeispiel")](layouts-images/Grid-Large.png#lightbox "Rasterbeispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/GridDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/GridDemoPage.xaml) |
|     |     |

### <a name="absolutelayout"></a>Von "AbsoluteLayout"

|     |     |
| --- | --- |
| [`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout) Positioniert untergeordnete Elemente an bestimmten Positionen relativ zum übergeordneten Element. Ein untergeordnetes Element des Position wird angegeben, mit der [angefügte Eigenschaften](~/xamarin-forms/xaml/attached-properties.md) [ `LayoutBounds` ](xref:Xamarin.Forms.AbsoluteLayout.LayoutBoundsProperty) und [ `LayoutFlags` ](xref:Xamarin.Forms.AbsoluteLayout.LayoutFlagsProperty). Ein `AbsoluteLayout` eignet sich für die Animation der Positionen der Ansichten.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.AbsoluteLayout) / [Handbuch](~/xamarin-forms/user-interface/layouts/absolute-layout.md) / [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout) | [![Beispiel für die von "AbsoluteLayout"](layouts-images/AbsoluteLayout.png "von \"AbsoluteLayout\" Beispiel")](layouts-images/AbsoluteLayout-Large.png#lightbox "von \"AbsoluteLayout\"-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/AbsoluteLayoutdDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/AbsoluteLayoutDemoPage.xaml) mit [CodeBehind](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/AbsoluteLayoutDemoPage.xaml.cs) |
|     |     |

### <a name="relativelayout"></a>RelativeLayout

|     |     |
| --- | --- |
| [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) Positioniert untergeordnete Elemente relativ zu den `RelativeLayout` selbst oder den nebengeordneten Elementen. Ein untergeordnetes Element des Position wird angegeben, mit der [angefügte Eigenschaften](~/xamarin-forms/xaml/attached-properties.md) , die auf Objekte des Typs festgelegt werden [ `Constraint` ](xref:Xamarin.Forms.Constraint) und [ `BoundsConstraint` ](xref:Xamarin.Forms.Constraint).<br /><br />[API-Dokumentation](xref:Xamarin.Forms.RelativeLayout) / [Handbuch](~/xamarin-forms/user-interface/layouts/relative-layout.md) / [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout) | [![Beispiel für RelativeLayout](layouts-images/RelativeLayout.png "RelativeLayout Beispiel")](layouts-images/RelativeLayout-Large.png#lightbox "RelativeLayout-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/RelativeLayoutDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/RelativeLayoutDemoPage.xaml) |
|     |     |

### <a name="flexlayout"></a>FlexLayout

|     |     |
| --- | --- |
| [`FlexLayout`](xref:Xamarin.Forms.FlexLayout) basiert darauf, dass der CSS [Flexible Box-Layout-Module](http://www.w3.org/TR/css-flexbox-1/), häufig als bezeichnet _flex Layout_ oder _-Box-Flex_. `FlexLayout` definiert sechs bindbare Eigenschaften und fünf angefügte bindbare Eigenschaften, mit denen untergeordnete Elemente gestapelt oder mit vielen Ausrichtung und die Ausrichtung Optionen umschlossen werden soll.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.FlexLayout) / [Handbuch](~/xamarin-forms/user-interface/layouts/flex-layout.md) / [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-flexlayoutdemos) | [![Beispiel für FlexLayout](layouts-images/FlexLayout.png "FlexLayout Beispiel")](layouts-images/FlexLayout-Large.png#lightbox "FlexLayout-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/FlexLayoutDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/FlexLayoutDemoPage.xaml) |
|     |     |

## <a name="related-links"></a>Verwandte Links

- [Beispiel für Xamarin.Forms FormsGallery](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)
- [Xamarin.Forms Samples (Beispiele für Xamarin.Forms)](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.Forms)
- [Xamarin.Forms-API-Dokumentation](https://docs.microsoft.com/dotnet/api/xamarin.forms?view=xamarin-forms)
