---
title: Xamarin.Forms Layouts
description: Xamarin.Forms-Layouts werden verwendet, um die Benutzeroberflächen-Steuerelemente in visual Strukturen zu verfassen. In diesem Artikel werden die Layouts enthalten in Xamarin.Forms aufgelistet.
ms.prod: xamarin
ms.assetid: F4180997-BA21-453A-9958-D1E2940DF050
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/21/2018
ms.openlocfilehash: 6e9889bf8ec748ed2034d63acfec9784d074ca44
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/08/2018
ms.locfileid: "35243089"
---
# <a name="xamarinforms-layouts"></a>Xamarin.Forms Layouts

_Xamarin.Forms-Layouts werden verwendet, um die Benutzeroberflächen-Steuerelemente in visual Strukturen zu verfassen._

Die [ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout) und [ `Layout<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/) Klassen in Xamarin.Forms sind spezielle Untertypen von Sichten, die als Container für Ansichten und andere Tastaturlayouts stimmen dienen. Die `Layout` Klasse leitet sich von [ `View` ](views.md). Ein `Layout` Ableitung in der Regel enthält Logik, um die Position und Größe von untergeordneten Elementen in Xamarin.Forms Anwendungen festzulegen.

[![Xamarin.Forms Layout Typen](layouts-images/layouts-sml.png "Xamarin.Forms Layout Typen")](layouts-images/layouts.png#lightbox "Xamarin.Forms-Layout-Typen")

Die abgeleitete Klassen `Layout` können in zwei Kategorien unterteilt werden:

## <a name="layouts-with-single-content"></a>Layouts mit einzelnen Inhalt

Diese Klassen werden aus [ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/), die definiert [ `Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/) und [ `IsClippedToBounds` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.IsClippedToBounds/) Eigenschaften.

<a name="contentView" />

### <a name="contentview"></a>ContentView

|     |     |
| --- | --- |
| [`ContentView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/) enthält ein einzelnes untergeordnetes Element, das mit festgelegt ist die [ `Content` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ContentView.Content/) Eigenschaft. Die `Content` Eigenschaft kann festgelegt werden, um alle `View` Ableitung, einschließlich anderer `Layout` ableitungen. `ContentView` Dient hauptsächlich als ein strukturelles Element und dient als Basisklasse für [ `Frame` ](#frame).<br /><br />[API-Dokumentation](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/) | [![Beispiel für ContentView](layouts-images/ContentView.png "ContentView Beispiel")](layouts-images/ContentView-Large.png#lightbox "ContentView-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ContentViewDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ContentViewDemoPage.xaml) |
|     |     |

<a named="frame" />

### <a name="frame"></a>Frame

|     |     |
| --- | --- |
| Die [ `Frame` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Frame/) Klasse abgeleitet [ `ContentView` ](#contentView) und zeigt einen rechteckigen Rahmen um den untergeordneten. `Frame` verfügt über einen standardmäßigen [ `Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/) Wert von 20 und definiert auch [ `OutlineColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Frame.OutlineColor/), [ `CornerRadius` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Frame.CornerRadius/), und [ `HasShadow` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Frame.HasShadow/)Eigenschaften.<br /><br />[API-Dokumentation](https://developer.xamarin.com/api/type/Xamarin.Forms.Frame/) | [![Frame-Beispiel](layouts-images/Frame.png "Frame-Beispiel")](layouts-images/Frame-Large.png#lightbox "Frame-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/FrameDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/FrameDemoPage.xaml) |
|     |     |

<a name="scrollView" />

### <a name="scrollview"></a>ScrollView

|     |     |
| --- | --- |
| [`ScrollView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/) Durchführen eines Bildlaufs sein Inhalt kann. Legen Sie die [ `Content` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ScrollView.Content/) Eigenschaft auf eine Sicht oder Layout zu groß, um auf den Bildschirm passen. (Der Inhalt einer `ScrollView` ist sehr häufig eine [ `StackLayout` ](#stackLayout).) Legen Sie die [ `Orientation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ScrollView.Orientation/) Eigenschaft, um anzugeben, ob vertikale,, Durchführen eines Bildlaufs werden sollte Horizontal oder beides.<br /><br />[API-Dokumentation](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/) / [Handbuch](~/xamarin-forms/user-interface/layouts/scroll-view.md) / [Beispiel](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/) | [![Beispiel für ScrollView](layouts-images/ScrollView.png "ScrollView Beispiel")](layouts-images/ScrollView-Large.png#lightbox "ScrollView-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ScrollViewDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ScrollViewDemoPage.xaml) |
|     |     |

### <a name="templatedview"></a>TemplatedView

|     |     |
| --- | --- |
| [`TemplatedView`](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplatedView/) Zeigt den Inhalt mit einer Steuerelementvorlage und ist die Basisklasse für [ `ContentView` ](#contentView).<br /><br />[API-Dokumentation](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplatedView/) / [Handbuch](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md) | [![Beispiel für TemplatedView](layouts-images/TemplatedView.png "TemplatedView Beispiel")](layouts-images/TemplatedView.png#lightbox "TemplatedView-Beispiel") |
|     |     |

### <a name="contentpresenter"></a>ContentPresenter

|     |     |
| --- | --- |
| [`ContentPresenter`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPresenter/) ist eine Layout-Manager für vorlagenbasierte Ansichten, innerhalb einer [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) zu markieren, die Inhalte, die dargestellt werden soll, wobei angezeigt wird.<br /><br />[API-Dokumentation](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPresenter/) / [Handbuch](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md) | [![Beispiel für ContentPresenter](layouts-images/ContentPresenter.png "ContentPresenter Beispiel")](layouts-images/ContentPresenter.png#lightbox "ContentPresenter-Beispiel") |
|     |     |

## <a name="layouts-with-multiple-children"></a>Layouts mit mehreren untergeordneten Elementen

Diese Klassen werden aus [ `Layout<View>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/).

<a name="stackLayout" />

### <a name="stacklayout"></a>StackLayout

|     |     |
| --- | --- |
| [`StackLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) Positioniert untergeordnete Elemente in einem Stapel entweder horizontal oder vertikal basierend auf den [ `Orientation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.StackLayout.Orientation/) Eigenschaft. Die [ `Spacing` ](https://developer.xamarin.com/api/property/Xamarin.Forms.StackLayout.Spacing/) Eigenschaft bestimmt den Abstand zwischen den untergeordneten Elementen, und hat den Standardwert 6.<br /><br />[API-Dokumentation](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) / [Handbuch](~/xamarin-forms/user-interface/layouts/stack-layout.md) / [Beispiel](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)| [![Beispiel für StackLayout](layouts-images/StackLayout.png "StackLayout Beispiel")](layouts-images/StackLayout-Large.png#lightbox "StackLayout-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/StackLayoutDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/StackLayoutDemoPage.xaml) |
|     |     |

<a name="grid" />

### <a name="grid"></a>Raster

|     |     |
| --- | --- |
| [`Grid`](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/) positioniert die untergeordneten Elemente in einem Raster mit Zeilen und Spalten an. Eine untergeordnete Position wird angegeben, mit der [angefügte Eigenschaften](~/xamarin-forms/xaml/attached-properties.md) [ `Row` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Grid.RowProperty/), [ `Column` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Grid.ColumnProperty/), [ `RowSpan` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Grid.RowSpanProperty/), und [ `ColumnSpan` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Grid.ColumnSpanProperty/).<br /><br />[API-Dokumentation](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/) / [Handbuch](~/xamarin-forms/user-interface/layouts/grid.md) / [Beispiel](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/) | [![Rasterbeispiel](layouts-images/Grid.png "Rasterbeispiel")](layouts-images/Grid-Large.png#lightbox "Rasterbeispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/GridDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/GridDemoPage.xaml) |
|     |     |

### <a name="absolutelayout"></a>AbsoluteLayout

|     |     |
| --- | --- |
| [`AbsoluteLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.AbsoluteLayout/) Positioniert untergeordnete Elemente an bestimmten Positionen relativ zu seinem übergeordneten Element. Eine untergeordnete Position wird angegeben, mit der [angefügte Eigenschaften](~/xamarin-forms/xaml/attached-properties.md) [ `LayoutBounds` ](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayout.LayoutBoundsProperty/) und [ `LayoutFlags` ](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayout.LayoutFlagsProperty/). Ein `AbsoluteLayout` eignet sich für die Animation der Positionen der Ansichten.<br /><br />[API-Dokumentation](https://developer.xamarin.com/api/type/Xamarin.Forms.AbsoluteLayout/) / [Handbuch](~/xamarin-forms/user-interface/layouts/absolute-layout.md) / [Beispiel](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/) | [![Beispiel für AbsoluteLayout](layouts-images/AbsoluteLayout.png "AbsoluteLayout Beispiel")](layouts-images/AbsoluteLayout-Large.png#lightbox "AbsoluteLayout-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/AbsoluteLayoutdDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/AbsoluteLayoutDemoPage.xaml) mit [CodeBehind](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/AbsoluteLayoutDemoPage.xaml.cs) |
|     |     |

### <a name="relativelayout"></a>RelativeLayout

|     |     |
| --- | --- |
| [`RelativeLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.RelativeLayout/) Positioniert untergeordnete Elemente relativ zu den `RelativeLayout` selbst oder ihre gleichgeordneten Elemente. Eine untergeordnete Position wird angegeben, mit der [angefügte Eigenschaften](~/xamarin-forms/xaml/attached-properties.md) , festgelegt werden, um Objekte des Typs [ `Constraint` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Constraint/) und [ `BoundsConstraint` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Constraint/).<br /><br />[API-Dokumentation](https://developer.xamarin.com/api/type/Xamarin.Forms.RelativeLayout/) / [Handbuch](~/xamarin-forms/user-interface/layouts/relative-layout.md) / [Beispiel](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/) | [![Beispiel für RelativeLayout](layouts-images/RelativeLayout.png "RelativeLayout Beispiel")](layouts-images/RelativeLayout-Large.png#lightbox "RelativeLayout-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/RelativeLayoutDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/RelativeLayoutDemoPage.xaml) |
|     |     |

### <a name="flexlayout"></a>FlexLayout

|     |     |
| --- | --- |
| [`FlexLayout`](xref:Xamarin.Forms.FlexLayout) basiert auf den CSS-Code [Flexible Box Layout Module](http://www.w3.org/TR/css-flexbox-1/), das häufig als bezeichnet _flex Layout_ oder _-Box-Flex_. `FlexLayout` definiert sechs bindbare Eigenschaften und fünf angefügte bindbare Eigenschaften, die ermöglichen, untergeordnete Elemente gestapelt oder mit vielen Ausrichtung und Ausrichtung Optionen umschlossen werden soll.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.FlexLayout) / [Handbuch](~/xamarin-forms/user-interface/layouts/flex-layout.md) / [Beispiel](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/) | [![Beispiel für FlexLayout](layouts-images/FlexLayout.png "FlexLayout Beispiel")](layouts-images/FlexLayout-Large.png#lightbox "FlexLayout-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/FlexLayoutDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/FlexLayoutDemoPage.xaml) |
|     |     |

## <a name="related-links"></a>Verwandte Links

- [Introduction to Xamarin.Forms (Einführung in Xamarin.Forms)](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [Xamarin.Forms FormsGallery-Beispiel](https://developer.xamarin.com/samples/FormsGallery/)
- [Xamarin.Forms Samples (Beispiele für Xamarin.Forms)](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [Xamarin.Forms-API-Dokumentation](https://developer.xamarin.com/api/root/Xamarin.Forms/)
