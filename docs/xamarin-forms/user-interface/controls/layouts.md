---
Title: " Xamarin.Forms Layouts" Beschreibung: " Xamarin.Forms Layouts werden verwendet, um Benutzeroberflächen-Steuerelemente in visuellen Strukturen zu verfassen. In diesem Artikel werden die in enthaltenen Layouts aufgeführt Xamarin.Forms .
ms. Prod: xamarin ms. assetid: F4180997-BA21-453a-9958-D1E2940DF050 ms. Technology: xamarin-Forms Author: davidbritch ms. Author: dabritch ms. Date: 05/21/2018 NO-LOC: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="xamarinforms-layouts"></a>Xamarin.FormsLayouts

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)

_Xamarin. Forms-Layouts werden verwendet, um Benutzeroberflächen-Steuerelemente in visuellen Strukturen zu verfassen._

Die [`Layout`](xref:Xamarin.Forms.Layout) [`Layout<T>`](xref:Xamarin.Forms.Layout`1) Klassen und in Xamarin.Forms sind spezialisierte Untertypen von Sichten, die als Container für Ansichten und andere Layouts fungieren. Die `Layout` Klasse selbst wird von abgeleitet [`View`](views.md) . Eine `Layout` Ableitung enthält in der Regellogik zum Festlegen der Position und Größe von untergeordneten Elementen in Xamarin.Forms Anwendungen.

[![Xamarin.FormsLayouttypen](layouts-images/layouts-sml.png "[! Schel. No-Loc (xamarin. Forms)]-Layouttypen")](layouts-images/layouts.png#lightbox "[! Schel. No-Loc (xamarin. Forms)]-Layouttypen")

Die Klassen, die von abgeleitet `Layout` werden, können in zwei Kategorien unterteilt werden:

## <a name="layouts-with-single-content"></a>Layouts mit Einzelinhalt

Diese Klassen werden von abgeleitet [`Layout`](xref:Xamarin.Forms.Layout) , wodurch [`Padding`](xref:Xamarin.Forms.Layout.Padding) -und-Eigenschaften definiert werden [`IsClippedToBounds`](xref:Xamarin.Forms.Layout.IsClippedToBounds) .

### <a name="contentview"></a>ContentView

|     |     |
| --- | --- |
| [`ContentView`](xref:Xamarin.Forms.ContentView)enthält ein einzelnes untergeordnetes Element, das mit der-Eigenschaft festgelegt wird [`Content`](xref:Xamarin.Forms.ContentView.Content) . Die- `Content` Eigenschaft kann auf eine beliebige Ableitung festgelegt werden `View` , einschließlich anderer `Layout` Ableitungen. `ContentView`wird größtenteils als Strukturelement verwendet und fungiert als Basisklasse für [`Frame`](#frame) .<br /><br />[API-Dokumentation](xref:Xamarin.Forms.ContentView)  /  [Leitfaden](~/xamarin-forms/user-interface/layouts/contentview.md)  /  [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-contentviewdemos/) | [![Contentview-Beispiel](layouts-images/ContentView.png "Contentview-Beispiel")](layouts-images/ContentView-Large.png#lightbox "Contentview-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ContentViewDemoPage.cs)  /  [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ContentViewDemoPage.xaml) |
|     |     |

### <a name="frame"></a>Frame

|     |     |
| --- | --- |
| Die [`Frame`](xref:Xamarin.Forms.Frame) -Klasse wird von abgeleitet [`ContentView`](#contentview) und zeigt einen Rahmen oder einen Rahmen um das untergeordnete Element an. Die `Frame` -Klasse hat den Standard [`Padding`](xref:Xamarin.Forms.Layout.Padding) Wert 20 und definiert auch die [`BorderColor`](xref:Xamarin.Forms.Frame.BorderColor) [`CornerRadius`](xref:Xamarin.Forms.Frame.CornerRadius) Eigenschaften, und [`HasShadow`](xref:Xamarin.Forms.Frame.HasShadow) .<br /><br />[API-Dokumentation](xref:Xamarin.Forms.Frame)  /  [Leitfaden](~/xamarin-forms/user-interface/layouts/frame.md)  /  [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-frame/) | [![Frame-Beispiel](layouts-images/Frame.png "Frame-Beispiel")](layouts-images/Frame-Large.png#lightbox "Frame-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/FrameDemoPage.cs)  /  [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/FrameDemoPage.xaml) |
|     |     |

### <a name="scrollview"></a>ScrollView

|     |     |
| --- | --- |
| [`ScrollView`](xref:Xamarin.Forms.ScrollView)kann seinen Inhalt scrollen. Legen Sie die- [`Content`](xref:Xamarin.Forms.ScrollView.Content) Eigenschaft auf eine Ansicht oder ein Layout fest, die für den Bildschirm zu groß ist. (Der Inhalt eines `ScrollView` ist häufig ein [`StackLayout`](#stacklayout) .) Legen Sie die- [`Orientation`](xref:Xamarin.Forms.ScrollView.Orientation) Eigenschaft fest, um anzugeben, ob das Scrollen vertikal, horizontal oder beides sein soll.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.ScrollView)  /  [Leitfaden](~/xamarin-forms/user-interface/layouts/scrollview.md)  /  [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout) | [![ScrollView-Beispiel](layouts-images/ScrollView.png "ScrollView-Beispiel")](layouts-images/ScrollView-Large.png#lightbox "ScrollView-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ScrollViewDemoPage.cs)  /  [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ScrollViewDemoPage.xaml) |
|     |     |

### <a name="templatedview"></a>Templatedview

|     |     |
| --- | --- |
| [`TemplatedView`](xref:Xamarin.Forms.TemplatedView)zeigt Inhalt mit einer Steuerelement Vorlage an, und ist die Basisklasse für [`ContentView`](#contentview) .<br /><br />[API-Dokumentation](xref:Xamarin.Forms.TemplatedView)  /  [Leitfaden](~/xamarin-forms/app-fundamentals/templates/control-template.md) | [![Templatedview-Beispiel](layouts-images/TemplatedView.png "Templatedview-Beispiel")](layouts-images/TemplatedView.png#lightbox "Templatedview-Beispiel") |
|     |     |

### <a name="contentpresenter"></a>ContentPresenter

|     |     |
| --- | --- |
| [`ContentPresenter`](xref:Xamarin.Forms.ContentPresenter)ist ein LayoutManager für Vorlagen Sichten, der in einem verwendet wird, [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) um zu markieren, wo der Inhalt angezeigt wird, der angezeigt werden soll.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.ContentPresenter)  /  [Leitfaden](~/xamarin-forms/app-fundamentals/templates/control-template.md) | [![ContentPresenter-Beispiel](layouts-images/ContentPresenter.png "ContentPresenter-Beispiel")](layouts-images/ContentPresenter.png#lightbox "ContentPresenter-Beispiel") |
|     |     |

## <a name="layouts-with-multiple-children"></a>Layouts mit mehreren untergeordneten Elementen

Diese Klassen werden von abgeleitet [`Layout<View>`](xref:Xamarin.Forms.Layout`1) .

### <a name="stacklayout"></a>StackLayout

|     |     |
| --- | --- |
| [`StackLayout`](xref:Xamarin.Forms.StackLayout)positioniert untergeordnete Elemente in einem Stapel entweder horizontal oder vertikal basierend auf der- [`Orientation`](xref:Xamarin.Forms.StackLayout.Orientation) Eigenschaft. Die [`Spacing`](xref:Xamarin.Forms.StackLayout.Spacing) -Eigenschaft bestimmt den Abstand zwischen den untergeordneten Elementen und hat den Standardwert 6.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.StackLayout)  /  [Leitfaden](~/xamarin-forms/user-interface/layouts/stacklayout.md)  /  [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout)| [![Stacklayout-Beispiel](layouts-images/StackLayout.png "Stacklayout-Beispiel")](layouts-images/StackLayout-Large.png#lightbox "Stacklayout-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/StackLayoutDemoPage.cs)  /  [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/StackLayoutDemoPage.xaml) |
|     |     |

### <a name="grid"></a>Raster

|     |     |
| --- | --- |
| [`Grid`](xref:Xamarin.Forms.Grid)positioniert die untergeordneten Elemente in einem Raster von Zeilen und Spalten. Die Position eines untergeordneten Elements wird mithilfe der [angefügten Eigenschaften](~/xamarin-forms/xaml/attached-properties.md) [`Row`](xref:Xamarin.Forms.Grid.RowProperty) , [`Column`](xref:Xamarin.Forms.Grid.ColumnProperty) , [`RowSpan`](xref:Xamarin.Forms.Grid.RowSpanProperty) und angegeben [`ColumnSpan`](xref:Xamarin.Forms.Grid.ColumnSpanProperty) .<br /><br />[API-Dokumentation](xref:Xamarin.Forms.Grid)  /  [Leitfaden](~/xamarin-forms/user-interface/layouts/grid.md)  /  [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout) | [![Raster Beispiel](layouts-images/Grid.png "Raster Beispiel")](layouts-images/Grid-Large.png#lightbox "Raster Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/GridDemoPage.cs)  /  [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/GridDemoPage.xaml) |
|     |     |

### <a name="absolutelayout"></a>AbsoluteLayout

|     |     |
| --- | --- |
| [`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout)positioniert untergeordnete Elemente an bestimmten Speicherorten relativ zum übergeordneten Element. Die Position eines untergeordneten Elements wird mithilfe der [angefügten Eigenschaften](~/xamarin-forms/xaml/attached-properties.md) [`LayoutBounds`](xref:Xamarin.Forms.AbsoluteLayout.LayoutBoundsProperty) und angegeben [`LayoutFlags`](xref:Xamarin.Forms.AbsoluteLayout.LayoutFlagsProperty) . Ein `AbsoluteLayout` eignet sich zum Animieren der Positionen von Sichten.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.AbsoluteLayout)  /  [Leitfaden](~/xamarin-forms/user-interface/layouts/absolute-layout.md)  /  [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout) | [![Beispiel für "AbsoluteLayout"](layouts-images/AbsoluteLayout.png "Beispiel für "AbsoluteLayout"")](layouts-images/AbsoluteLayout-Large.png#lightbox "Beispiel für "AbsoluteLayout"")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/AbsoluteLayoutDemoPage.cs)  /  [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/AbsoluteLayoutDemoPage.xaml) mit [Code Behind](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/AbsoluteLayoutDemoPage.xaml.cs) |
|     |     |

### <a name="relativelayout"></a>RelativeLayout

|     |     |
| --- | --- |
| [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout)positioniert untergeordnete Elemente in Bezug auf die `RelativeLayout` selbst oder Ihre gleich geordneten Elemente. Die Position eines untergeordneten Elements wird mithilfe der [angefügten Eigenschaften](~/xamarin-forms/xaml/attached-properties.md) angegeben, die auf Objekte des Typs und festgelegt sind [`Constraint`](xref:Xamarin.Forms.Constraint) [`BoundsConstraint`](xref:Xamarin.Forms.Constraint) .<br /><br />[API-Dokumentation](xref:Xamarin.Forms.RelativeLayout)  /  [Leitfaden](~/xamarin-forms/user-interface/layouts/relative-layout.md)  /  [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout) | [![Relativelayout-Beispiel](layouts-images/RelativeLayout.png "Relativelayout-Beispiel")](layouts-images/RelativeLayout-Large.png#lightbox "Relativelayout-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/RelativeLayoutDemoPage.cs)  /  [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/RelativeLayoutDemoPage.xaml) |
|     |     |

### <a name="flexlayout"></a>FlexLayout

|     |     |
| --- | --- |
| [`FlexLayout`](xref:Xamarin.Forms.FlexLayout)basiert auf dem flexiblen CSS- [Box-Layoutmodul](https://www.w3.org/TR/css-flexbox-1/), das im Allgemeinen als " _flexlayout_ " oder " _Flexbox_" bezeichnet wird. `FlexLayout`definiert sechs bindbare Eigenschaften und fünf angefügte bindbare Eigenschaften, mit denen untergeordnete Elemente gestapelt oder mit vielen Optionen für Ausrichtung und Ausrichtung umschließt werden können.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.FlexLayout)  /  [Leitfaden](~/xamarin-forms/user-interface/layouts/flex-layout.md)  /  [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-flexlayoutdemos) | [![Flexlayout-Beispiel](layouts-images/FlexLayout.png "Flexlayout-Beispiel")](layouts-images/FlexLayout-Large.png#lightbox "Flexlayout-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/FlexLayoutDemoPage.cs)  /  [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/FlexLayoutDemoPage.xaml) |
|     |     |

## <a name="related-links"></a>Verwandte Links

- [Xamarin.FormsFormsgallery-Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)
- [Xamarin.Forms-Beispiele](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.Forms)
- [Xamarin.FormsAPI-Dokumentation](https://docs.microsoft.com/dotnet/api/xamarin.forms?view=xamarin-forms)
