---
title: Xamarin.Forms-Layouts
description: Xamarin.Forms-Layouts werden verwendet, um Steuerelemente der Benutzeroberfläche in visual Strukturen zu erstellen. Dieser Artikel beschreibt die Layouts, die in Xamarin.Forms enthalten.
ms.prod: xamarin
ms.assetid: F4180997-BA21-453A-9958-D1E2940DF050
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/21/2018
ms.openlocfilehash: 4747ce6555a6440c687dc3d239d75307f68683ca
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2020
ms.locfileid: "76724482"
---
# <a name="xamarinforms-layouts"></a>Xamarin.Forms-Layouts

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)

_Xamarin. Forms-Layouts werden verwendet, um Benutzeroberflächen-Steuerelemente in visuellen Strukturen zu verfassen._

Die Klassen [`Layout`](xref:Xamarin.Forms.Layout) und [`Layout<T>`](xref:Xamarin.Forms.Layout`1) in xamarin. Forms sind spezialisierte Untertypen von Sichten, die als Container für Ansichten und andere Layouts fungieren. Die `Layout` Klasse selbst wird von [`View`](views.md)abgeleitet. Eine `Layout` Ableitung enthält in der Regellogik zum Festlegen der Position und Größe von untergeordneten Elementen in xamarin. Forms-Anwendungen.

[![Xamarin. Forms-Layouttypen](layouts-images/layouts-sml.png "Xamarin. Forms-Layouttypen")](layouts-images/layouts.png#lightbox "Xamarin. Forms-Layouttypen")

Die Klassen, die von `Layout` abgeleitet werden, können in zwei Kategorien unterteilt werden:

## <a name="layouts-with-single-content"></a>Layouts mit einzelnen Inhalt

Diese Klassen werden von [`Layout`](xref:Xamarin.Forms.Layout)abgeleitet, die [`Padding`](xref:Xamarin.Forms.Layout.Padding) -und [`IsClippedToBounds`](xref:Xamarin.Forms.Layout.IsClippedToBounds) -Eigenschaften definiert.

<a name="contentView" />

### <a name="contentview"></a>ContentView

|     |     |
| --- | --- |
| [`ContentView`](xref:Xamarin.Forms.ContentView) enthält ein einzelnes untergeordnetes Element, das mit der [`Content`](xref:Xamarin.Forms.ContentView.Content) -Eigenschaft festgelegt wird. Die `Content`-Eigenschaft kann auf beliebige `View` Ableitung festgelegt werden, einschließlich anderer `Layout`-Ableitungen. `ContentView` wird größtenteils als Strukturelement verwendet und fungiert als Basisklasse, um [`Frame`](#frame).<br /><br />[API-Dokumentation](xref:Xamarin.Forms.ContentView) / [Handbuch](~/xamarin-forms/user-interface/layouts/contentview.md) / [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-contentviewdemos/) | [![Contentview-Beispiel](layouts-images/ContentView.png "Contentview-Beispiel")](layouts-images/ContentView-Large.png#lightbox "Contentview-Beispiel")<br />Code für diese Seite / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ContentViewDemoPage.xaml) [ C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ContentViewDemoPage.cs) |
|     |     |

<a named="frame" />

### <a name="frame"></a>Frame

|     |     |
| --- | --- |
| Die [`Frame`](xref:Xamarin.Forms.Frame) -Klasse wird von [`ContentView`](#contentView) abgeleitet und zeigt einen Rahmen (oder Rahmen) um das untergeordnete Element an. Die `Frame`-Klasse verfügt über einen Standard [`Padding`](xref:Xamarin.Forms.Layout.Padding) Wert von 20 und definiert außerdem die Eigenschaften [`BorderColor`](xref:Xamarin.Forms.Frame.BorderColor), [`CornerRadius`](xref:Xamarin.Forms.Frame.CornerRadius)und [`HasShadow`](xref:Xamarin.Forms.Frame.HasShadow) .<br /><br />[API-Dokumentation](xref:Xamarin.Forms.Frame) / [Handbuch](~/xamarin-forms/user-interface/layouts/frame.md) / [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-frame/) | [![Frame-Beispiel](layouts-images/Frame.png "Frame-Beispiel")](layouts-images/Frame-Large.png#lightbox "Frame-Beispiel")<br />Code für diese Seite / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/FrameDemoPage.xaml) [ C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/FrameDemoPage.cs) |
|     |     |

<a name="scrollView" />

### <a name="scrollview"></a>ScrollView

|     |     |
| --- | --- |
| [`ScrollView`](xref:Xamarin.Forms.ScrollView) kann seinen Inhalt scrollen. Legen Sie die [`Content`](xref:Xamarin.Forms.ScrollView.Content) -Eigenschaft auf eine Ansicht oder ein Layout fest, die für den Bildschirm zu groß ist. (Der Inhalt einer `ScrollView` ist häufig [`StackLayout`](#stackLayout).) Legen Sie die [`Orientation`](xref:Xamarin.Forms.ScrollView.Orientation) -Eigenschaft fest, um anzugeben, ob das Scrollen vertikal, horizontal oder beides sein soll.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.ScrollView) / [Handbuch](~/xamarin-forms/user-interface/layouts/scroll-view.md) / [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout) | [![ScrollView-Beispiel](layouts-images/ScrollView.png "ScrollView-Beispiel")](layouts-images/ScrollView-Large.png#lightbox "ScrollView-Beispiel")<br />Code für diese Seite / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ScrollViewDemoPage.xaml) [ C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ScrollViewDemoPage.cs) |
|     |     |

### <a name="templatedview"></a>TemplatedView

|     |     |
| --- | --- |
| [`TemplatedView`](xref:Xamarin.Forms.TemplatedView) zeigt Inhalt mit einer Steuerelement Vorlage an, und ist die Basisklasse für [`ContentView`](#contentView).<br /><br />[API-Dokumentation](xref:Xamarin.Forms.TemplatedView) / [Handbuch](~/xamarin-forms/app-fundamentals/templates/control-template.md) | [![Templatedview-Beispiel](layouts-images/TemplatedView.png "Templatedview-Beispiel")](layouts-images/TemplatedView.png#lightbox "Templatedview-Beispiel") |
|     |     |

### <a name="contentpresenter"></a>ContentPresenter-Element

|     |     |
| --- | --- |
| [`ContentPresenter`](xref:Xamarin.Forms.ContentPresenter) ist ein LayoutManager für Vorlagen Sichten, der in einem [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) verwendet wird, um zu markieren, wo der Inhalt angezeigt wird, der dargestellt werden soll.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.ContentPresenter) / [Handbuch](~/xamarin-forms/app-fundamentals/templates/control-template.md) | [![ContentPresenter-Beispiel](layouts-images/ContentPresenter.png "ContentPresenter-Beispiel")](layouts-images/ContentPresenter.png#lightbox "ContentPresenter-Beispiel") |
|     |     |

## <a name="layouts-with-multiple-children"></a>Layouts mit mehreren untergeordneten Elementen

Diese Klassen werden von [`Layout<View>`](xref:Xamarin.Forms.Layout`1)abgeleitet.

<a name="stackLayout" />

### <a name="stacklayout"></a>StackLayout

|     |     |
| --- | --- |
| [`StackLayout`](xref:Xamarin.Forms.StackLayout) positioniert untergeordnete Elemente in einem Stapel entweder horizontal oder vertikal basierend auf der [`Orientation`](xref:Xamarin.Forms.StackLayout.Orientation) -Eigenschaft. Die [`Spacing`](xref:Xamarin.Forms.StackLayout.Spacing) -Eigenschaft steuert den Abstand zwischen den untergeordneten Elementen und hat den Standardwert 6.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.StackLayout) / [Handbuch](~/xamarin-forms/user-interface/layouts/stack-layout.md) / [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout)| [![Stacklayout-Beispiel](layouts-images/StackLayout.png "Stacklayout-Beispiel")](layouts-images/StackLayout-Large.png#lightbox "Stacklayout-Beispiel")<br />Code für diese Seite / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/StackLayoutDemoPage.xaml) [ C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/StackLayoutDemoPage.cs) |
|     |     |

<a name="grid" />

### <a name="grid"></a>Raster

|     |     |
| --- | --- |
| [`Grid`](xref:Xamarin.Forms.Grid) positioniert seine untergeordneten Elemente in einem Raster von Zeilen und Spalten. Die Position eines untergeordneten Elements wird mithilfe der [angefügten Eigenschaften](~/xamarin-forms/xaml/attached-properties.md) [`Row`](xref:Xamarin.Forms.Grid.RowProperty), [`Column`](xref:Xamarin.Forms.Grid.ColumnProperty), [`RowSpan`](xref:Xamarin.Forms.Grid.RowSpanProperty)und [`ColumnSpan`](xref:Xamarin.Forms.Grid.ColumnSpanProperty)angegeben.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.Grid) / [Handbuch](~/xamarin-forms/user-interface/layouts/grid.md) / [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout) | [![Raster Beispiel](layouts-images/Grid.png "Raster Beispiel")](layouts-images/Grid-Large.png#lightbox "Raster Beispiel")<br />Code für diese Seite / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/GridDemoPage.xaml) [ C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/GridDemoPage.cs) |
|     |     |

### <a name="absolutelayout"></a>Von "AbsoluteLayout"

|     |     |
| --- | --- |
| [`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout) positioniert untergeordnete Elemente an bestimmten Speicherorten relativ zum übergeordneten Element. Die Position eines untergeordneten Elements wird mithilfe der [angefügten Eigenschaften](~/xamarin-forms/xaml/attached-properties.md) [`LayoutBounds`](xref:Xamarin.Forms.AbsoluteLayout.LayoutBoundsProperty) und [`LayoutFlags`](xref:Xamarin.Forms.AbsoluteLayout.LayoutFlagsProperty)angegeben. Eine `AbsoluteLayout` ist für die Animation der Positionen von Ansichten nützlich.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.AbsoluteLayout) / [Handbuch](~/xamarin-forms/user-interface/layouts/absolute-layout.md) / [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout) | [![Beispiel für "AbsoluteLayout"](layouts-images/AbsoluteLayout.png "Beispiel für AbsoluteLayout")](layouts-images/AbsoluteLayout-Large.png#lightbox "Beispiel für AbsoluteLayout")<br />Code für diese Seite / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/AbsoluteLayoutDemoPage.xaml) mit [Code Behind](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/AbsoluteLayoutDemoPage.xaml.cs) [ C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/AbsoluteLayoutDemoPage.cs) |
|     |     |

### <a name="relativelayout"></a>RelativeLayout

|     |     |
| --- | --- |
| [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) positioniert untergeordnete Elemente in Bezug auf die `RelativeLayout` selbst oder Ihre gleich geordneten Elemente. Die Position eines untergeordneten Elements wird mithilfe der [angefügten Eigenschaften](~/xamarin-forms/xaml/attached-properties.md) angegeben, die auf Objekte vom Typ [`Constraint`](xref:Xamarin.Forms.Constraint) und [`BoundsConstraint`](xref:Xamarin.Forms.Constraint)festgelegt sind.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.RelativeLayout) / [Handbuch](~/xamarin-forms/user-interface/layouts/relative-layout.md) / [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout) | [![Relativelayout-Beispiel](layouts-images/RelativeLayout.png "Relativelayout-Beispiel")](layouts-images/RelativeLayout-Large.png#lightbox "Relativelayout-Beispiel")<br />Code für diese Seite / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/RelativeLayoutDemoPage.xaml) [ C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/RelativeLayoutDemoPage.cs) |
|     |     |

### <a name="flexlayout"></a>FlexLayout

|     |     |
| --- | --- |
| [`FlexLayout`](xref:Xamarin.Forms.FlexLayout) basiert auf dem flexiblen CSS- [Box-Layoutmodul](https://www.w3.org/TR/css-flexbox-1/), das im Allgemeinen als " _flexlayout_ " oder " _Flexbox_" bezeichnet wird. `FlexLayout` definiert sechs bindbare Eigenschaften und fünf angefügte bindbare Eigenschaften, mit denen untergeordnete Elemente gestapelt oder mit vielen Optionen für Ausrichtung und Ausrichtung umschließt werden können.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.FlexLayout) / [Handbuch](~/xamarin-forms/user-interface/layouts/flex-layout.md) / [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-flexlayoutdemos) | [![Flexlayout-Beispiel](layouts-images/FlexLayout.png "Flexlayout-Beispiel")](layouts-images/FlexLayout-Large.png#lightbox "Flexlayout-Beispiel")<br />Code für diese Seite / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/FlexLayoutDemoPage.xaml) [ C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/FlexLayoutDemoPage.cs) |
|     |     |

## <a name="related-links"></a>Verwandte Links

- [Xamarin. Forms formsgallery-Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)
- [Xamarin.Forms Samples (Beispiele für Xamarin.Forms)](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.Forms)
- [Xamarin.Forms-API-Dokumentation](https://docs.microsoft.com/dotnet/api/xamarin.forms?view=xamarin-forms)
