---
title: Renderer-Basisklassen und Native Steuerelemente
description: Alle Xamarin.Forms-Steuerelements verfügt über eine zugehörige Renderer für jede Plattform, die eine Instanz eines systemeigenen Steuerelements erstellt. Dieser Artikel listet die Renderer und dem nativen Steuerelements-Klassen, die jede Xamarin.Forms-Seite, Layout, Ansicht und Zelle zu implementieren.
ms.prod: xamarin
ms.assetid: A8909AE3-ED0E-4D24-BF96-B49E732E3B93
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/19/2018
ms.openlocfilehash: 56df2f7e6b83ddd4a5780506471cbd32a3aced40
ms.sourcegitcommit: 5fc171a45697f7c610d65f74d1f3cebbac445de6
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2018
ms.locfileid: "52171949"
---
# <a name="renderer-base-classes-and-native-controls"></a>Renderer-Basisklassen und Native Steuerelemente

_Alle Xamarin.Forms-Steuerelements verfügt über eine zugehörige Renderer für jede Plattform, die eine Instanz eines systemeigenen Steuerelements erstellt. Dieser Artikel listet die Renderer und dem nativen Steuerelements-Klassen, die jede Xamarin.Forms-Seite, Layout, Ansicht und Zelle zu implementieren._

Mit Ausnahme von der `MapRenderer` -Klasse, die plattformspezifische Renderer finden Sie in den folgenden Namespaces:

- **iOS** – Xamarin.Forms.Platform.iOS
- **Android** – Xamarin.Forms.Platform.Android
- **Android (AppCompat)** – Xamarin.Forms.Platform.Android.AppCompat
- **Universelle Windows-Plattform (UWP)** : Xamarin.Forms.Platform.UWP

Die `MapRenderer` Klasse finden Sie in den folgenden Namespaces:

- **iOS** – Xamarin.Forms.Maps.iOS
- **Android** – Xamarin.Forms.Maps.Android
- **Universelle Windows-Plattform (UWP)** – Xamarin.Forms.Maps.UWP

## <a name="pages"></a>Seiten

Die folgende Tabelle enthält die Renderer und dem nativen Steuerelements-Klassen, die jede Xamarin.Forms implementieren [Seite](~/xamarin-forms/user-interface/controls/pages.md) Typ:

|Seite|Renderer|iOS|Android|Android (AppCompat)|UWP|
|--- |--- |--- |--- |--- |--- |
|[`ContentPage`](xref:Xamarin.Forms.ContentPage)|[PageRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/contentpage.md)|UIViewController|ViewGroup||FrameworkElement|
|[`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage)|PhoneMasterDetailRenderer (iOS-Telefon) TabletMasterDetailPageRenderer (iOS – Tablet), MasterDetailRenderer (Android), MasterDetailPageRenderer (Android AppCompat), MasterDetailPageRenderer (UWP)|UIViewController (Phone), UISplitViewController (Tablet)|DrawerLayout (v4)|DrawerLayout (v4)|"FrameworkElement" (benutzerdefiniertes Steuerelement)|
|[`NavigationPage`](xref:Xamarin.Forms.NavigationPage)|NavigationRenderer (iOS und Android), NavigationPageRenderer (Android AppCompat), NavigationPageRenderer (UWP)|UIToolbar|ViewGroup|ViewGroup|"FrameworkElement" (benutzerdefiniertes Steuerelement)|
|[`TabbedPage`](xref:Xamarin.Forms.TabbedPage)|TabbedRenderer (iOS und Android), TabbedPageRenderer (Android AppCompat), TabbedPageRenderer (UWP)|UIView|ViewPager|ViewPager|"FrameworkElement" (PowerPivot)|
|[`TemplatedPage`](xref:Xamarin.Forms.TemplatedPage)|PageRenderer|UIViewController|ViewGroup||FrameworkElement|
|[`CarouselPage`](xref:Xamarin.Forms.CarouselPage)|CarouselPageRenderer|UIScrollView-Element|ViewPager|ViewPager|"FrameworkElement" (FlipView)|

## <a name="layouts"></a>Layouts

Die folgende Tabelle enthält die Renderer und dem nativen Steuerelements-Klassen, die jede Xamarin.Forms implementieren [Layout](~/xamarin-forms/user-interface/controls/layouts.md) Typ:

|Layout|Renderer|iOS|Android|UWP|
|--- |--- |--- |--- |--- |
|[`ContentPresenter`](xref:Xamarin.Forms.ContentPresenter)|ViewRenderer|UIView|Ansicht|FrameworkElement|
|[`ContentView`](xref:Xamarin.Forms.ContentView)|ViewRenderer|UIView|Ansicht|FrameworkElement|
|[`FlexLayout`](xref:Xamarin.Forms.FlexLayout)|ViewRenderer|UIView|Ansicht|FrameworkElement|
|[`Frame`](xref:Xamarin.Forms.Frame)|FrameRenderer|UIView|ViewGroup|Rahmen|
|[`ScrollView`](xref:Xamarin.Forms.ScrollView)|ScrollViewRenderer|UIScrollView-Element|ScrollView|ScrollViewer|
|[`TemplatedView`](xref:Xamarin.Forms.TemplatedView)|ViewRenderer|UIView|Ansicht|FrameworkElement|
|[`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout)|ViewRenderer|UIView|Ansicht|FrameworkElement|
|[`Grid`](xref:Xamarin.Forms.Grid)|ViewRenderer|UIView|Ansicht|FrameworkElement|
|[`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout)|ViewRenderer|UIView|Ansicht|FrameworkElement|
|[`StackLayout`](xref:Xamarin.Forms.StackLayout)|ViewRenderer|UIView|Ansicht|FrameworkElement|

## <a name="views"></a>Ansichten

Die folgende Tabelle enthält die Renderer und dem nativen Steuerelements-Klassen, die jede Xamarin.Forms implementieren [Ansicht](~/xamarin-forms/user-interface/controls/views.md) Typ:

|Ansichten|Renderer|iOS|Android|Android (AppCompat)|UWP|
|--- |--- |--- |--- |--- |--- |
|[`ActivityIndicator`](xref:Xamarin.Forms.ActivityIndicator)|ActivityIndicatorRenderer|UIActivityIndicator|ProgressBar||ProgressBar|
|[`BoxView`](xref:Xamarin.Forms.BoxView)|BoxRenderer (iOS und Android), BoxViewRenderer (UWP)|UIView|ViewGroup||Rechteck|
|[`Button`](xref:Xamarin.Forms.Button)|ButtonRenderer|UIButton|Schaltfläche|AppCompatButton|Schaltfläche|
|[`DatePicker`](xref:Xamarin.Forms.DatePicker)|DatePickerRenderer|UITextField|EditText||DatePicker|
|[`Editor`](xref:Xamarin.Forms.Editor)|EditorRenderer|UITextView|EditText||TextBox|
|[`Entry`](xref:Xamarin.Forms.Entry)|[EntryRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/entry.md)|UITextField|EditText||TextBox|
|[`Image`](xref:Xamarin.Forms.Image)|ImageRenderer|UIImageView|ImageView||Bild|
|`ImageButton`|ImageButtonRenderer|UIButton||AppCompatImageButton|Schaltfläche|
|[`Label`](xref:Xamarin.Forms.Label)|LabelRenderer|UILabel|TextView||TextBlock|
|[`ListView`](xref:Xamarin.Forms.ListView)|[ListViewRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/listview.md)|UITableView|ListView||ListView|
|[`Map`](xref:Xamarin.Forms.Maps.Map)|[MapRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md)|MKMapView|MapView||MapControl|
|[`Picker`](xref:Xamarin.Forms.Picker)|PickerRenderer|UITextField|EditText|EditText|ComboBox|
|[`ProgressBar`](xref:Xamarin.Forms.ProgressBar)|ProgressBarRenderer|UIProgressView|ProgressBar||ProgressBar|
|[`SearchBar`](xref:Xamarin.Forms.SearchBar)|SearchBarRenderer|UISearchBar|SearchView||AutoSuggestBox|
|[`Slider`](xref:Xamarin.Forms.Slider)|SliderRenderer|UISlider|SeekBar||Slider|
|[`Stepper`](xref:Xamarin.Forms.Stepper)|StepperRenderer|UIStepper|LinearLayout||Steuerelement|
|[`Switch`](xref:Xamarin.Forms.Switch)|SwitchRenderer|UISwitch|Schalter|SwitchCompat|ToggleSwitch|
|[`TableView`](xref:Xamarin.Forms.TableView)|TableViewRenderer|UITableView|ListView||ListView|
|[`TimePicker`](xref:Xamarin.Forms.TimePicker)|TimePickerRenderer|UITextField|EditText||TimePicker|
|[`WebView`](xref:Xamarin.Forms.WebView)|WebViewRenderer|UIWebView|WebView||WebView|

## <a name="cells"></a>Zellen

Die folgende Tabelle enthält die Renderer und dem nativen Steuerelements-Klassen, die jede Xamarin.Forms implementieren [Zelle](~/xamarin-forms/user-interface/controls/cells.md) Typ:

|Zellen|Renderer|iOS|Android|UWP|
|--- |--- |--- |--- |--- |
|[`EntryCell`](xref:Xamarin.Forms.EntryCell)|EntryCellRenderer|UITableViewCell mit ein UITextField|LinearLayout mit einem TextView und EditText|Mit "TextBox" DataTemplate-Element|
|[`SwitchCell`](xref:Xamarin.Forms.SwitchCell)|SwitchCellRenderer|UITableViewCell mit einem UISwitch|Schalter|Mit einem Raster mit einem TextBlock und ToggleSwitch DataTemplate-Element|
|[`TextCell`](xref:Xamarin.Forms.TextCell)|TextCellRenderer|UITableViewCell|LinearLayout mit zwei TextViews|DataTemplate-Element mit einem StackPanel mit zwei TextBlocks|
|[`ImageCell`](xref:Xamarin.Forms.ImageCell)|ImageCellRenderer|UITableViewCell mit einem UIImage|LinearLayout mit zwei TextViews und eine ImageView|DataTemplate-Element mit einem Raster mit einem Bild und zwei TextBlocks|
|[`ViewCell`](xref:Xamarin.Forms.ViewCell)|[ViewCellRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md)|UITableViewCell|Ansicht|DataTemplate-Element mit dem ein ContentPresenter-Element|

## <a name="summary"></a>Zusammenfassung

In diesem Artikel aufgeführten hat die Renderer und dem nativen Steuerelements-Klassen, die jede Xamarin.Forms-Seite, Layout, Ansicht und Zelle zu implementieren. Alle Xamarin.Forms-Steuerelements verfügt über eine zugehörige Renderer für jede Plattform, die eine Instanz eines systemeigenen Steuerelements erstellt.

## <a name="related-links"></a>Verwandte Links

- [Benutzerdefinierte Renderer (Xamarin University-Video)](https://developer.xamarin.com/videos/cross-platform/xamarinforms-custom-renderers/)
