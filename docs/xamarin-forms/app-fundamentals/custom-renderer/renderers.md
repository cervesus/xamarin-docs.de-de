---
title: 'title: "Rendererbasisklassen und native Steuerelemente" description: "Jedes Xamarin.Forms-Steuerelement verfügt über einen entsprechenden Renderer für jede Plattform, der eine Instanz eines nativen Steuerelements erstellt.'
description: 'In diesem Artikel werden die Klassen für Renderer und native Steuerelemente aufgelistet, die Xamarin.Forms-Seiten, -Layouts, -Ansichten und -Zellen implementieren." ms.prod: xamarin ms.assetid: A8909AE3-ED0E-4D24-BF96-B49E732E3B93 ms.technology: xamarin-forms author: davidbritch ms.author: dabritch ms.date: 04/17/2020 no-loc: [Xamarin.Forms, Xamarin.Essentials]'
ms.prod: xamarin
ms.assetid: A8909AE3-ED0E-4D24-BF96-B49E732E3B93
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/17/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 9f6eff831f3eb653e281dd9ee750bae7f7dfda06
ms.sourcegitcommit: 2105091f2eeb7844b19ae94708a6ab07e3e79bce
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2020
ms.locfileid: "84198390"
---
# <a name="renderer-base-classes-and-native-controls"></a>Rendererbasisklassen und native Steuerelemente

_Jedes Xamarin.Forms-Steuerelement verfügt über einen entsprechenden Renderer für jede Plattform, die eine Instanz eines nativen Steuerelements erstellt. In diesem Artikel werden die Klassen für Renderer und native Steuerelemente aufgelistet, die eine Xamarin.Forms-Seite, ein Xamarin.Forms-Layout, eine Xamarin.Forms-Ansicht und eine Xamarin.Forms-Zelle implementieren._

Mit Ausnahme der `MapRenderer`-Klasse finden Sie die plattformspezifischen Renderer in den folgenden Namespaces:

- **iOS**: Xamarin.Forms.Platform.iOS
- **Android**: Xamarin.Forms.Platform.Android
- **Android (AppCompat)** : Xamarin.Forms.Platform.Android.AppCompat
- **Android (FastRenderers)**  - Xamarin.Forms.Platform.Android.FastRenderers
- **Universelle Windows-Plattform (UWP)** : Xamarin.Forms.Platform.UWP

Weitere Informationen zu schnellen Renderern finden Sie unter [Xamarin.FormsSchnelle Renderer](~/xamarin-forms/internals/fast-renderers.md).

Die `MapRenderer`-Klasse finden Sie in den folgenden Namespaces:

- **iOS**: Xamarin.Forms.Maps.iOS
- **Android**: Xamarin.Forms.Maps.Android
- **Universelle Windows-Plattform (UWP)** : Xamarin.Forms.Maps.UWP

> [!NOTE]
> Weitere Informationen zum Erstellen von benutzerdefinierten Renderern für Shell-Anwendungen finden Sie unter [Benutzerdefinierte Xamarin.Forms-Shell-Renderer](~/xamarin-forms/app-fundamentals/shell/customrenderers.md).

## <a name="pages"></a>Seiten

In der folgenden Tabelle werden die Klassen für Renderer und native Steuerelemente aufgelistet, die jeden [Page](~/xamarin-forms/user-interface/controls/pages.md)-Typ von Xamarin.Forms implementieren.

|Seite|Renderer|iOS|Android|Android (AppCompat)|UWP|
|--- |--- |--- |--- |--- |--- |
|[`ContentPage`](xref:Xamarin.Forms.ContentPage)|[PageRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/contentpage.md)|UIViewController|ViewGroup||FrameworkElement|
|[`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage)|PhoneMasterDetailRenderer (iOS – Phone), TabletMasterDetailPageRenderer (iOS – Tablet), MasterDetailRenderer (Android), MasterDetailPageRenderer (Android AppCompat), MasterDetailPageRenderer (UWP)|UIViewController (Phone), UISplitViewController (Tablet)|DrawerLayout (v4)|DrawerLayout (v4)|FrameworkElement (benutzerdefiniertes Steuerelement)|
|[`NavigationPage`](xref:Xamarin.Forms.NavigationPage)|NavigationRenderer (iOS und Android), NavigationPageRenderer (Android AppCompat), NavigationPageRenderer (UWP)|UIToolbar|ViewGroup|ViewGroup|FrameworkElement (benutzerdefiniertes Steuerelement)|
|[`TabbedPage`](xref:Xamarin.Forms.TabbedPage)|TabbedRenderer (iOS und Android), TabbedPageRenderer (Android AppCompat), TabbedPageRenderer (UWP)|UIView|ViewPager|ViewPager|FrameworkElement (Pivot)|
|[`TemplatedPage`](xref:Xamarin.Forms.TemplatedPage)|PageRenderer|UIViewController|ViewGroup||FrameworkElement|
|[`CarouselPage`](xref:Xamarin.Forms.CarouselPage)|CarouselPageRenderer|UIScrollView|ViewPager|ViewPager|FrameworkElement (FlipView)|

## <a name="layouts"></a>Layouts

In der folgenden Tabelle werden die Klassen für Renderer und native Steuerelemente aufgelistet, die jeden [Layout](~/xamarin-forms/user-interface/controls/layouts.md)-Typ von Xamarin.Forms implementieren.

|Layout|Renderer|iOS|Android|Android (AppCompat)|UWP|
|--- |--- |--- |--- |--- |
|[`ContentPresenter`](xref:Xamarin.Forms.ContentPresenter)|ViewRenderer|UIView|Ansicht|Ansicht|FrameworkElement|
|[`ContentView`](xref:Xamarin.Forms.ContentView)|ViewRenderer|UIView|Ansicht|Ansicht|FrameworkElement|
|[`FlexLayout`](xref:Xamarin.Forms.FlexLayout)|ViewRenderer|UIView|Ansicht|Ansicht|FrameworkElement|
|[`Frame`](xref:Xamarin.Forms.Frame)|FrameRenderer|UIView|ViewGroup|CardView|Rahmen|
|[`ScrollView`](xref:Xamarin.Forms.ScrollView)|ScrollViewRenderer|UIScrollView|ScrollView|ScrollView|ScrollViewer|
|[`TemplatedView`](xref:Xamarin.Forms.TemplatedView)|ViewRenderer|UIView|Ansicht|Ansicht|FrameworkElement|
|[`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout)|ViewRenderer|UIView|Ansicht|Ansicht|FrameworkElement|
|[`Grid`](xref:Xamarin.Forms.Grid)|ViewRenderer|UIView|Ansicht|Ansicht|FrameworkElement|
|[`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout)|ViewRenderer|UIView|Ansicht|Ansicht|FrameworkElement|
|[`StackLayout`](xref:Xamarin.Forms.StackLayout)|ViewRenderer|UIView|Ansicht|Ansicht|FrameworkElement|

## <a name="views"></a>Ansichten

In der folgenden Tabelle werden die Klassen für Renderer und native Steuerelemente aufgelistet, die jeden [View](~/xamarin-forms/user-interface/controls/views.md)-Typ von Xamarin.Forms implementieren.

|Ansichten|Renderer|iOS|Android|Android (AppCompat)|UWP|
|--- |--- |--- |--- |--- |--- |
|[`ActivityIndicator`](xref:Xamarin.Forms.ActivityIndicator)|ActivityIndicatorRenderer|UIActivityIndicator|ProgressBar||ProgressBar|
|[`BoxView`](xref:Xamarin.Forms.BoxView)|BoxRenderer (iOS und Android), BoxViewRenderer (UWP)|UIView|ViewGroup||Rechteck|
|[`Button`](xref:Xamarin.Forms.Button)|ButtonRenderer|UIButton|Schaltfläche|AppCompatButton|Schaltfläche|
|[`CarouselView`](xref:Xamarin.Forms.CarouselView)|CarouselViewRenderer|UICollectionView||RecyclerView|ListViewBase|
|`CheckBox`|CheckBoxRenderer|UIButton||AppCompatCheckBox|CheckBox|
|[`CollectionView`](xref:Xamarin.Forms.CollectionView)|CollectionViewRenderer|UICollectionView||RecyclerView|ListViewBase|
|[`DatePicker`](xref:Xamarin.Forms.DatePicker)|DatePickerRenderer|UITextField|EditText||DatePicker|
|[`Editor`](xref:Xamarin.Forms.Editor)|EditorRenderer|UITextView|EditText||TextBox|
|[`Entry`](xref:Xamarin.Forms.Entry)|[EntryRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/entry.md)|UITextField|EditText||TextBox|
|[`Image`](xref:Xamarin.Forms.Image)|ImageRenderer|UIImageView|ImageView||Bild|
|[`ImageButton`](xref:Xamarin.Forms.ImageButton)|ImageButtonRenderer|UIButton||AppCompatImageButton|Schaltfläche|
|`IndicatorView`|IndicatorViewRenderer|UIPageControl||LinearLayout||
|[`Label`](xref:Xamarin.Forms.Label)|LabelRenderer|UILabel|TextView||TextBlock|
|[`ListView`](xref:Xamarin.Forms.ListView)|[ListViewRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/listview.md)|UITableView|ListView||ListView|
|[`Map`](xref:Xamarin.Forms.Maps.Map)|[MapRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/map-pin.md)|MKMapView|MapView||MapControl|
|[`MediaElement`](xref:Xamarin.Forms.MediaElement)|MediaElementRenderer|UIView||VideoView|MediaElement|
|[`Picker`](xref:Xamarin.Forms.Picker)|PickerRenderer|UITextField|EditText|EditText|ComboBox|
|`RadioButton`|RadioButtonRenderer|UIButton||AppCompatRadioButton|RadioButton|
|[`ProgressBar`](xref:Xamarin.Forms.ProgressBar)|ProgressBarRenderer|UIProgressView|ProgressBar||ProgressBar|
|[`RefreshView`](xref:Xamarin.Forms.RefreshView)|RefreshViewRenderer|UIView||SwipeRefreshLayout|RefreshContainer|
|[`SearchBar`](xref:Xamarin.Forms.SearchBar)|SearchBarRenderer|UISearchBar|SearchView||AutoSuggestBox|
|[`Slider`](xref:Xamarin.Forms.Slider)|SliderRenderer|UISlider|SeekBar||Slider|
|[`Stepper`](xref:Xamarin.Forms.Stepper)|StepperRenderer|UIStepper|LinearLayout||Steuerelement|
|`SwipeView`|SwipeViewRenderer|UIView||Ansicht|SwipeControl|
|[`Switch`](xref:Xamarin.Forms.Switch)|SwitchRenderer|UISwitch|Schalter|SwitchCompat|ToggleSwitch|
|[`TableView`](xref:Xamarin.Forms.TableView)|TableViewRenderer|UITableView|ListView||ListView|
|[`TimePicker`](xref:Xamarin.Forms.TimePicker)|TimePickerRenderer|UITextField|EditText||TimePicker|
|[`WebView`](xref:Xamarin.Forms.WebView)|WkWebViewRenderer (iOS), WebViewRenderer (Android und UWP)|WkWebView|WebView||WebView|

> [!NOTE]
> Das `Expander`-Steuerelement wird mithilfe einer [`StackLayout`](xref:Xamarin.Forms.StackLayout)-Eigenschaft mit Animation implementiert. Daher sind keine Plattformrenderer vorhanden.

## <a name="cells"></a>Zellen

In der folgenden Tabelle werden die Klassen für Renderer und native Steuerelemente aufgelistet, die jeden [Cell](~/xamarin-forms/user-interface/controls/cells.md)-Typ von Xamarin.Forms implementieren.

|Zellen|Renderer|iOS|Android|UWP|
|--- |--- |--- |--- |--- |
|[`EntryCell`](xref:Xamarin.Forms.EntryCell)|EntryCellRenderer|UITableViewCell mit UITextField|LinearLayout mit TextView und EditText|DataTemplate mit TextBox|
|[`SwitchCell`](xref:Xamarin.Forms.SwitchCell)|SwitchCellRenderer|UITableViewCell mit UISwitch|Schalter|DataTemplate mit einem Raster, das TextBlock und ToggleSwitch enthält|
|[`TextCell`](xref:Xamarin.Forms.TextCell)|TextCellRenderer|UITableViewCell|LinearLayout mit zwei TextViews|DataTemplate mit StackPanel, das zwei TextBlocks enthält|
|[`ImageCell`](xref:Xamarin.Forms.ImageCell)|ImageCellRenderer|UITableViewCell mit UIImage|LinearLayout mit zwei TextViews und einem ImageView|DataTemplate mit einem Raster, das ein Image und zwei TextBlocks enthält|
|[`ViewCell`](xref:Xamarin.Forms.ViewCell)|[ViewCellRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md)|UITableViewCell|Ansicht|DataTemplate mit ContentPresenter|

## <a name="related-links"></a>Verwandte Links

- [Xamarin.FormsSchnelle Renderer](~/xamarin-forms/internals/fast-renderers.md)
- [Xamarin.FormsBenutzerdefinierte Shell-Renderer](~/xamarin-forms/app-fundamentals/shell/customrenderers.md)
