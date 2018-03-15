---
title: Renderer-Basisklassen und systemeigenen Steuerelementen
description: "Jedes Xamarin.Forms-Steuerelement verfügt über eine begleitende Renderer für jede Plattform, die eine Instanz eines systemeigenen Steuerelements erstellt. Dieser Artikel führt des Renderers und systemeigene Steuerelementklassen, die jede Xamarin.Forms-Seite, Layout, Ansicht und Zelle zu implementieren."
ms.topic: article
ms.prod: xamarin
ms.assetid: A8909AE3-ED0E-4D24-BF96-B49E732E3B93
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/15/2016
ms.openlocfilehash: 06887e6c1a39dd695fdaddb2fade8a463d9d4580
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/15/2018
---
# <a name="renderer-base-classes-and-native-controls"></a>Renderer-Basisklassen und systemeigenen Steuerelementen

_Jedes Xamarin.Forms-Steuerelement verfügt über eine begleitende Renderer für jede Plattform, die eine Instanz eines systemeigenen Steuerelements erstellt. Dieser Artikel führt des Renderers und systemeigene Steuerelementklassen, die jede Xamarin.Forms-Seite, Layout, Ansicht und Zelle zu implementieren._

Mit Ausnahme von der `MapRenderer` -Klasse, die plattformspezifischen Renderern finden Sie in den folgenden Namespaces:

- **iOS** – Xamarin.Forms.Platform.iOS
- **Android** – Xamarin.Forms.Platform.Android
- **Android (AppCompat)** – Xamarin.Forms.Platform.Android.AppCompat
- **Universelle Windows-Plattform (UWP)** – Xamarin.Forms.Platform.UWP

Die `MapRenderer` Klasse finden Sie in den folgenden Namespaces:

- **iOS** – Xamarin.Forms.Maps.iOS
- **Android** – Xamarin.Forms.Maps.Android
- **Universelle Windows-Plattform (UWP)** – Xamarin.Forms.Maps.UWP

## <a name="pages"></a>Seiten

Die folgende Tabelle enthält die Renderer und der native Steuerelementklassen, die jede Xamarin.Forms implementieren [Seite](~/xamarin-forms/user-interface/controls/pages.md) Typ:

|Seite|Renderer|iOS|Android|Android (AppCompat)|UWP|
|--- |--- |--- |--- |--- |--- |
|[`ContentPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)|[PageRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/contentpage.md)|UIViewController|ViewGroup||FrameworkElement|
|[`MasterDetailPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/)|PhoneMasterDetailRenderer (iOS – Phone), TabletMasterDetailPageRenderer (iOS – Tablet), MasterDetailRenderer (Android), MasterDetailPageRenderer (Android AppCompat), MasterDetailPageRenderer (UWP)|UIViewController (Phone), UISplitViewController (Tablet)|DrawerLayout (v4)|DrawerLayout (v4)|"FrameworkElement" (benutzerdefiniertes Steuerelement)|
|[`NavigationPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/)|NavigationRenderer (iOS und Android), NavigationPageRenderer (Android AppCompat), NavigationPageRenderer (UWP)|UIToolbar|ViewGroup|ViewGroup|"FrameworkElement" (benutzerdefiniertes Steuerelement)|
|[`TabbedPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/)|TabbedRenderer (iOS und Android), TabbedPageRenderer (Android AppCompat), TabbedPageRenderer (UWP)|UIView|ViewPager|ViewPager|"FrameworkElement" (PowerPivot)|
|[`TemplatedPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplatedPage/)|PageRenderer|UIViewController|ViewGroup||FrameworkElement|
|[`CarouselPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/)|CarouselPageRenderer|UIScrollView|ViewPager|ViewPager|"FrameworkElement" (FlipView)|

## <a name="layouts"></a>Layouts

Die folgende Tabelle enthält die Renderer und der native Steuerelementklassen, die jede Xamarin.Forms implementieren [Layout](~/xamarin-forms/user-interface/controls/layouts.md) Typ:

|Layout|Renderer|iOS|Android|UWP|
|--- |--- |--- |--- |--- |
|[`ContentPresenter`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPresenter/)|ViewRenderer|UIView|Ansicht|FrameworkElement|
|[`ContentView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/)|ViewRenderer|UIView|Ansicht|FrameworkElement|
|[`Frame`](https://developer.xamarin.com/api/type/Xamarin.Forms.Frame/)|FrameRenderer|UIView|ViewGroup|Rahmen|
|[`ScrollView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/)|ScrollViewRenderer|UIScrollView|ScrollView|ScrollViewer|
|[`TemplatedView`](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplatedView/)|ViewRenderer|UIView|Ansicht|FrameworkElement|
|[`AbsoluteLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.AbsoluteLayout/)|ViewRenderer|UIView|Ansicht|FrameworkElement|
|[`Grid`](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/)|ViewRenderer|UIView|Ansicht|FrameworkElement|
|[`RelativeLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.RelativeLayout/)|ViewRenderer|UIView|Ansicht|FrameworkElement|
|[`StackLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/)|ViewRenderer|UIView|Ansicht|FrameworkElement|

## <a name="views"></a>Ansichten

Die folgende Tabelle enthält die Renderer und der native Steuerelementklassen, die jede Xamarin.Forms implementieren [Ansicht](~/xamarin-forms/user-interface/controls/views.md) Typ:

|Ansichten|Renderer|iOS|Android|Android (AppCompat)|UWP|
|--- |--- |--- |--- |--- |--- |
|[`ActivityIndicator`](https://developer.xamarin.com/api/type/Xamarin.Forms.ActivityIndicator/)|ActivityIndicatorRenderer|UIActivityIndicator|ProgressBar||ProgressBar|
|[`BoxView`](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/)|BoxRenderer (iOS und Android), BoxViewRenderer (Windows Phone und WinRT)|UIView|ViewGroup||Rechteck|
|[`Button`](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/)|ButtonRenderer|UIButton|Schaltfläche|AppCompatButton|Schaltfläche|
|[`CarouselView`](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselView/)|CarouselViewRenderer|UIScrollView|RecyclerView||FlipView|
|[`DatePicker`](https://developer.xamarin.com/api/type/Xamarin.Forms.DatePicker/)|DatePickerRenderer|UITextField|EditText||DatePicker|
|[`Editor`](https://developer.xamarin.com/api/type/Xamarin.Forms.Editor/)|EditorRenderer|UITextView|EditText||TextBox|
|[`Entry`](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/)|[EntryRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/entry.md)|UITextField|EditText||TextBox|
|[`Image`](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/)|ImageRenderer|UIImageView|ImageView||Bild|
|[`Label`](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/)|LabelRenderer|UILabel|TextView||TextBlock|
|[`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)|[ListViewRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/listview.md)|UITableView|ListView||ListView|
|[`Map`](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/)|[MapRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md)|MKMapView|MapView||MapControl|
|[`Picker`](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/)|PickerRenderer|UITextField|EditText|EditText|ComboBox|
|[`ProgressBar`](https://developer.xamarin.com/api/type/Xamarin.Forms.ProgressBar/)|ProgressBarRenderer|UIProgressView|ProgressBar||ProgressBar|
|[`SearchBar`](https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/)|SearchBarRenderer|UISearchBar|SearchView||AutoSuggestBox|
|[`Slider`](https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/)|SliderRenderer|UISlider|SeekBar||Slider|
|[`Stepper`](https://developer.xamarin.com/api/type/Xamarin.Forms.Stepper/)|StepperRenderer|UIStepper|LinearLayout||Steuerelement|
|[`Switch`](https://developer.xamarin.com/api/type/Xamarin.Forms.Switch/)|SwitchRenderer|UISwitch|Schalter|SwitchCompat|ToggleSwitch|
|[`TableView`](https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/)|TableViewRenderer|UITableView|ListView||ListView|
|[`TimePicker`](https://developer.xamarin.com/api/type/Xamarin.Forms.TimePicker/)|TimePickerRenderer|UITextField|EditText||TimePicker|
|[`WebView`](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/)|WebViewRenderer|UIWebView|WebView||WebView|

## <a name="cells"></a>Zellen

Die folgende Tabelle enthält die Renderer und der native Steuerelementklassen, die jede Xamarin.Forms implementieren [Zelle](~/xamarin-forms/user-interface/controls/cells.md) Typ:

|Zellen|Renderer|iOS|Android|UWP|
|--- |--- |--- |--- |--- |
|[`EntryCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.EntryCell/)|EntryCellRenderer|Mit einem UITextField UITableViewCell|Mit einem TextView und EditText-LinearLayout|Mit einem Textfeld DataTemplate|
|[`SwitchCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.SwitchCell/)|SwitchCellRenderer|Mit einem UISwitch UITableViewCell|Schalter|Mit einem Raster mit einem TextBlock und ToggleSwitch DataTemplate|
|[`TextCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.TextCell/)|TextCellRenderer|UITableViewCell|LinearLayout mit zwei TextViews|Mit einem StackPanel mit zwei TextBlocks DataTemplate|
|[`ImageCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageCell/)|ImageCellRenderer|Mit einem UIImage UITableViewCell|LinearLayout mit zwei TextViews und eine ImageView|Mit einem Raster enthält ein Bild und zwei TextBlocks DataTemplate|
|[`ViewCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/)|[ViewCellRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md)|UITableViewCell|Ansicht|Mit einem ContentPresenter DataTemplate|

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde eingestellt, des Renderers und systemeigene Steuerelementklassen, die jede Xamarin.Forms-Seite, Layout, Ansicht und Zelle zu implementieren. Jedes Xamarin.Forms-Steuerelement verfügt über eine begleitende Renderer für jede Plattform, die eine Instanz eines systemeigenen Steuerelements erstellt.

## <a name="related-links"></a>Verwandte Links

- [Benutzerdefinierte Renderer (Xamarin-University-Video)](https://developer.xamarin.com/videos/cross-platform/xamarinforms-custom-renderers/)
