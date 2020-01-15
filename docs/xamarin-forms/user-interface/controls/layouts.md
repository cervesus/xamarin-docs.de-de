---
title: Xamarin.Forms-Layouts
description: Xamarin.Forms-Layouts werden verwendet, um Steuerelemente der Benutzeroberfläche in visual Strukturen zu erstellen. Dieser Artikel beschreibt die Layouts, die in Xamarin.Forms enthalten.
ms.prod: xamarin
ms.assetid: F4180997-BA21-453A-9958-D1E2940DF050
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/21/2018
ms.openlocfilehash: e247be8387ce984d6695431ec432119d01344b42
ms.sourcegitcommit: 211fed94fb96127a3e158ae1ff5d7eb831a203d8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/15/2020
ms.locfileid: "75955768"
---
# <a name="xamarinforms-layouts"></a>Xamarin.Forms-Layouts

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)

_Xamarin.Forms-Layouts werden verwendet, um Benutzeroberflächen-Steuerelemente in visual Strukturen zu erstellen._

Die [ `Layout` ](xref:Xamarin.Forms.Layout) und [ `Layout<T>` ](xref:Xamarin.Forms.Layout`1) Klassen in Xamarin.Forms sind spezielle Untertypen von Ansichten, die als Container für Ansichten und anderer Layouts zu fungieren. Die `Layout` Klasse leitet sich von [ `View` ](views.md). Ein `Layout` Ableitung enthält in der Regel die Logik, um die Position und Größe der untergeordneten Elemente in Xamarin.Forms-Anwendungen festlegen.

[![Xamarin. Forms-Layouttypen](layouts-images/layouts-sml.png "Xamarin. Forms-Layouttypen")](layouts-images/layouts.png#lightbox "Xamarin. Forms-Layouttypen")

Die von abgeleiteten Klassen `Layout` kann in zwei Kategorien unterteilt werden:

## <a name="layouts-with-single-content"></a>Layouts mit einzelnen Inhalt

Diese Klassen werden aus [ `Layout` ](xref:Xamarin.Forms.Layout), die definiert, [ `Padding` ](xref:Xamarin.Forms.Layout.Padding) und [ `IsClippedToBounds` ](xref:Xamarin.Forms.Layout.IsClippedToBounds) Eigenschaften.

<a name="contentView" />

### <a name="contentview"></a>ContentView

|     |     |
| --- | --- |
| [`ContentView`](xref:Xamarin.Forms.ContentView) enthält ein einzelnes untergeordnetes Element, das mit festgelegt ist die [ `Content` ](xref:Xamarin.Forms.ContentView.Content) Eigenschaft. Die `Content` Eigenschaft kann festgelegt werden, um alle `View` Ableitung, einschließlich anderer `Layout` ableitungen. `ContentView` wird hauptsächlich als ein strukturelles Element und dient als Basisklasse hinzu [ `Frame` ](#frame).<br /><br />[API-Dokumentation](xref:Xamarin.Forms.ContentView) / [Handbuch](~/xamarin-forms/user-interface/layouts/contentview.md) / [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-cardview/) | [![Contentview-Beispiel](layouts-images/ContentView.png "Contentview-Beispiel")](layouts-images/ContentView-Large.png#lightbox "Contentview-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ContentViewDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ContentViewDemoPage.xaml) |
|     |     |

<a named="frame" />

### <a name="frame"></a>Frame

|     |     |
| --- | --- |
| Die [`Frame`](xref:Xamarin.Forms.Frame) -Klasse wird von [`ContentView`](#contentView) abgeleitet und zeigt einen Rahmen (oder Rahmen) um das untergeordnete Element an. Die `Frame`-Klasse verfügt über einen Standard [`Padding`](xref:Xamarin.Forms.Layout.Padding) Wert von 20 und definiert außerdem die Eigenschaften [`BorderColor`](xref:Xamarin.Forms.Frame.BorderColor), [`CornerRadius`](xref:Xamarin.Forms.Frame.CornerRadius)und [`HasShadow`](xref:Xamarin.Forms.Frame.HasShadow) .<br /><br />[API-Dokumentation](xref:Xamarin.Forms.Frame) / [Handbuch](~/xamarin-forms/user-interface/layouts/frame.md) / [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-frame/) | [![Frame-Beispiel](layouts-images/Frame.png "Frame-Beispiel")](layouts-images/Frame-Large.png#lightbox "Frame-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/FrameDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/FrameDemoPage.xaml) |
|     |     |

<a name="scrollView" />

### <a name="scrollview"></a>ScrollView

|     |     |
| --- | --- |
| [`ScrollView`](xref:Xamarin.Forms.ScrollView) ist in der Lage des Bildlaufs an seinen Inhalt. Legen Sie die [ `Content` ](xref:Xamarin.Forms.ScrollView.Content) Eigenschaft auf eine Sicht oder Layout zu groß, um auf dem Bildschirm zu passen. (Der Inhalt einer `ScrollView` ist häufig [`StackLayout`](#stackLayout).) Legen Sie die [`Orientation`](xref:Xamarin.Forms.ScrollView.Orientation) -Eigenschaft fest, um anzugeben, ob das Scrollen vertikal, horizontal oder beides sein soll.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.ScrollView) / [Handbuch](~/xamarin-forms/user-interface/layouts/scroll-view.md) / [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout) | [![ScrollView-Beispiel](layouts-images/ScrollView.png "ScrollView-Beispiel")](layouts-images/ScrollView-Large.png#lightbox "ScrollView-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ScrollViewDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ScrollViewDemoPage.xaml) |
|     |     |

### <a name="templatedview"></a>TemplatedView

|     |     |
| --- | --- |
| [`TemplatedView`](xref:Xamarin.Forms.TemplatedView) Zeigt den Inhalt mit einer Steuerelementvorlage und ist die Basisklasse für [ `ContentView` ](#contentView).<br /><br />[API-Dokumentation](xref:Xamarin.Forms.TemplatedView) / [Handbuch](~/xamarin-forms/app-fundamentals/templates/control-template.md) | [![Templatedview-Beispiel](layouts-images/TemplatedView.png "Templatedview-Beispiel")](layouts-images/TemplatedView.png#lightbox "Templatedview-Beispiel") |
|     |     |

### <a name="contentpresenter"></a>ContentPresenter-Element

|     |     |
| --- | --- |
| [`ContentPresenter`](xref:Xamarin.Forms.ContentPresenter) ist ein Layout-Manager für auf Vorlagen basierenden Ansichten, die innerhalb einer [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) , markieren Sie der Inhalt, der angezeigt werden, wobei angezeigt wird.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.ContentPresenter) / [Handbuch](~/xamarin-forms/app-fundamentals/templates/control-template.md) | [![ContentPresenter-Beispiel](layouts-images/ContentPresenter.png "ContentPresenter-Beispiel")](layouts-images/ContentPresenter.png#lightbox "ContentPresenter-Beispiel") |
|     |     |

## <a name="layouts-with-multiple-children"></a>Layouts mit mehreren untergeordneten Elementen

Diese Klassen werden aus [ `Layout<View>` ](xref:Xamarin.Forms.Layout`1).

<a name="stackLayout" />

### <a name="stacklayout"></a>StackLayout

|     |     |
| --- | --- |
| [`StackLayout`](xref:Xamarin.Forms.StackLayout) Positioniert untergeordnete Elemente in einem Stapel entweder horizontal oder vertikal basierend auf den [ `Orientation` ](xref:Xamarin.Forms.StackLayout.Orientation) Eigenschaft. Die [ `Spacing` ](xref:Xamarin.Forms.StackLayout.Spacing) Eigenschaft bestimmt den Abstand zwischen den untergeordneten Elementen und hat den Standardwert von 6.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.StackLayout) / [Handbuch](~/xamarin-forms/user-interface/layouts/stack-layout.md) / [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout)| [![Stacklayout-Beispiel](layouts-images/StackLayout.png "Stacklayout-Beispiel")](layouts-images/StackLayout-Large.png#lightbox "Stacklayout-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/StackLayoutDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/StackLayoutDemoPage.xaml) |
|     |     |

<a name="grid" />

### <a name="grid"></a>Raster

|     |     |
| --- | --- |
| [`Grid`](xref:Xamarin.Forms.Grid) positioniert die untergeordneten Elemente in einem Raster mit Zeilen und Spalten an. Ein untergeordnetes Element des Position wird angegeben, mit der [angefügte Eigenschaften](~/xamarin-forms/xaml/attached-properties.md) [ `Row` ](xref:Xamarin.Forms.Grid.RowProperty), [ `Column` ](xref:Xamarin.Forms.Grid.ColumnProperty), [ `RowSpan` ](xref:Xamarin.Forms.Grid.RowSpanProperty), und [ `ColumnSpan` ](xref:Xamarin.Forms.Grid.ColumnSpanProperty).<br /><br />[API-Dokumentation](xref:Xamarin.Forms.Grid) / [Handbuch](~/xamarin-forms/user-interface/layouts/grid.md) / [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout) | [![Raster Beispiel](layouts-images/Grid.png "Raster Beispiel")](layouts-images/Grid-Large.png#lightbox "Raster Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/GridDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/GridDemoPage.xaml) |
|     |     |

### <a name="absolutelayout"></a>AbsoluteLayout

|     |     |
| --- | --- |
| [`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout) Positioniert untergeordnete Elemente an bestimmten Positionen relativ zum übergeordneten Element. Ein untergeordnetes Element des Position wird angegeben, mit der [angefügte Eigenschaften](~/xamarin-forms/xaml/attached-properties.md) [ `LayoutBounds` ](xref:Xamarin.Forms.AbsoluteLayout.LayoutBoundsProperty) und [ `LayoutFlags` ](xref:Xamarin.Forms.AbsoluteLayout.LayoutFlagsProperty). Ein `AbsoluteLayout` eignet sich für die Animation der Positionen der Ansichten.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.AbsoluteLayout) / [Handbuch](~/xamarin-forms/user-interface/layouts/absolute-layout.md) / [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout) | [![Beispiel für "AbsoluteLayout"](layouts-images/AbsoluteLayout.png "Beispiel für "AbsoluteLayout"")](layouts-images/AbsoluteLayout-Large.png#lightbox "Beispiel für "AbsoluteLayout"")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/AbsoluteLayoutdDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/AbsoluteLayoutDemoPage.xaml) mit [CodeBehind](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/AbsoluteLayoutDemoPage.xaml.cs) |
|     |     |

### <a name="relativelayout"></a>RelativeLayout

|     |     |
| --- | --- |
| [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) Positioniert untergeordnete Elemente relativ zu den `RelativeLayout` selbst oder den nebengeordneten Elementen. Ein untergeordnetes Element des Position wird angegeben, mit der [angefügte Eigenschaften](~/xamarin-forms/xaml/attached-properties.md) , die auf Objekte des Typs festgelegt werden [ `Constraint` ](xref:Xamarin.Forms.Constraint) und [ `BoundsConstraint` ](xref:Xamarin.Forms.Constraint).<br /><br />[API-Dokumentation](xref:Xamarin.Forms.RelativeLayout) / [Handbuch](~/xamarin-forms/user-interface/layouts/relative-layout.md) / [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout) | [![Relativelayout-Beispiel](layouts-images/RelativeLayout.png "Relativelayout-Beispiel")](layouts-images/RelativeLayout-Large.png#lightbox "Relativelayout-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/RelativeLayoutDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/RelativeLayoutDemoPage.xaml) |
|     |     |

### <a name="flexlayout"></a>FlexLayout

|     |     |
| --- | --- |
| [`FlexLayout`](xref:Xamarin.Forms.FlexLayout) basiert darauf, dass der CSS [Flexible Box-Layout-Module](https://www.w3.org/TR/css-flexbox-1/), häufig als bezeichnet _flex Layout_ oder _-Box-Flex_. `FlexLayout` definiert sechs bindbare Eigenschaften und fünf angefügte bindbare Eigenschaften, mit denen untergeordnete Elemente gestapelt oder mit vielen Ausrichtung und die Ausrichtung Optionen umschlossen werden soll.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.FlexLayout) / [Handbuch](~/xamarin-forms/user-interface/layouts/flex-layout.md) / [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-flexlayoutdemos) | [![Flexlayout-Beispiel](layouts-images/FlexLayout.png "Flexlayout-Beispiel")](layouts-images/FlexLayout-Large.png#lightbox "Flexlayout-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/FlexLayoutDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/FlexLayoutDemoPage.xaml) |
|     |     |

## <a name="related-links"></a>Verwandte Themen

- [Beispiel für Xamarin.Forms FormsGallery](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)
- [Xamarin.Forms-Beispiele](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.Forms)
- [Xamarin.Forms-API-Dokumentation](https://docs.microsoft.com/dotnet/api/xamarin.forms?view=xamarin-forms)
