---
title: Xamarin.Forms-Ansichten
description: Xamarin.Forms-Ansichten sind die Bausteine von plattformübergreifenden mobilen Benutzeroberflächen. Dieser Artikel beschreibt die Ansichten, die in Xamarin.Forms enthalten sind.
ms.prod: xamarin
ms.assetid: AC070686-A423-4A98-8BB6-0B9F94C062CC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/14/2020
ms.openlocfilehash: 09bcb49db7f257a415518b259672ca8e776cdbc4
ms.sourcegitcommit: 5d22f37dfc358678df52a4d17c57261056a72cb7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2020
ms.locfileid: "77674564"
---
# <a name="xamarinforms-views"></a>Xamarin.Forms-Ansichten

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery/)

_Xamarin. Forms-Ansichten sind die Bausteine von plattformübergreifenden mobilen Benutzeroberflächen._

Sichten sind Benutzeroberflächen Objekte, wie z. b. Bezeichnungen, Schaltflächen und Schieberegler, die in anderen grafischen Programmierumgebungen häufig als Steuer *Elemente* oder *Widgets* bezeichnet werden. Die von xamarin. Forms unterstützten Ansichten werden von der [`View`](xref:Xamarin.Forms.View) -Klasse abgeleitet. Sie können in verschiedene Kategorien unterteilt werden:

## <a name="views-for-presentation"></a>Ansichten für die Präsentation

### <a name="label"></a>Bezeichnung

|     |     |
| --- | --- |
| [`Label`](xref:Xamarin.Forms.Label) zeigt einzeilige Text Zeichenfolgen oder mehrzeilige Textblöcke an, entweder mit konstanter oder variabler Formatierung. Legen Sie die [`Text`](xref:Xamarin.Forms.Label.Text) -Eigenschaft auf eine Zeichenfolge für die Konstante Formatierung fest, oder legen Sie die [`FormattedText`](xref:Xamarin.Forms.Label.FormattedText) -Eigenschaft auf ein [`FormattedString`](xref:Xamarin.Forms.FormattedString) Objekt für die Variablen Formatierung fest.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.Label) / [Handbuch](~/xamarin-forms/user-interface/text/label.md) / [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text) | [![Beispiel für eine Bezeichnung](views-images/Label.png "Beispiel für eine Bezeichnung")](views-images/Label-Large.png#lightbox "Beispiel für eine Bezeichnung")<br /> Code für diese Seite / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/LabelDemoPage.xaml) [ C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/LabelDemoPage.cs) |
|     |     |

### <a name="image"></a>Image

|     |     |
| --- | --- |
| [`Image`](xref:Xamarin.Forms.Image) zeigt eine Bitmap an. Bitmaps können über das Internet heruntergeladen, als Ressourcen in die gängigen Projekt-oder Platt Form Projekte eingebettet oder mithilfe eines .net `Stream`-Objekts erstellt werden.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.Image) / [Handbuch](~/xamarin-forms/user-interface/images.md) / [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithimages) | [![Bildbeispiel](views-images/Image.png "Bildbeispiel")](views-images/Image-Large.png#lightbox "Bildbeispiel")<br />Code für diese Seite / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ImageDemoPage.xaml) [ C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ImageDemoPage.cs) |
|     |     |

### <a name="boxview"></a>BoxView

|     |    |
| --- | ---|
| [`BoxView`](xref:Xamarin.Forms.BoxView) zeigt ein durch die [`Color`](xref:Xamarin.Forms.BoxView.Color) -Eigenschaft farbiges voll Rechtecks an. `BoxView` hat eine Standardgröße von 40 x 40 Anforderungen. Weisen Sie für andere Größen die Eigenschaften [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) und [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) zu.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.BoxView) / [Handbuch](~/xamarin-forms/user-interface/boxview.md) / [Beispiel 1](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-basicboxview), [2](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-textdecoration), [3](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-listviewcolors/), [4](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-gameoflife), [5](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-dotmatrixclock)und [6](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-boxviewclock) | [![Boxview-Beispiel](views-images/BoxView.png "Boxview-Beispiel")](views-images/BoxView-Large.png#lightbox "Boxview-Beispiel")<br />Code für diese Seite / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/BoxViewDemoPage.xaml) [ C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/BoxViewDemoPage.cs) |
|     |     |

### <a name="webview"></a>WebView

|     |     |
| --- | --- |
| [`WebView`](xref:Xamarin.Forms.WebView) zeigt Webseiten oder HTML-Inhalt an, je nachdem, ob die [`Source`](xref:Xamarin.Forms.WebView.Source) -Eigenschaft auf eine [`UriWebViewSource`](xref:Xamarin.Forms.UrlWebViewSource) oder ein [`HtmlWebViewSource`](xref:Xamarin.Forms.HtmlWebViewSource) Objekt festgelegt ist.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.WebView) / [Handbuch](~/xamarin-forms/user-interface/webview.md) / [Beispiel 1](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithwebview) und [2](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-webview) | [![WebView-Beispiel](views-images/WebView.png "WebView-Beispiel")](views-images/WebView-Large.png#lightbox "WebView-Beispiel")<br />Code für diese Seite / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/WebViewDemoPage.xaml) [ C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/WebViewDemoPage.cs) |
|     |     |

### <a name="openglview"></a>OpenGLView

|     |     |
| --- | --- |
| [`OpenGLView`](xref:Xamarin.Forms.OpenGLView) zeigt OpenGL-Grafiken in IOS-und Android-Projekten an. Es gibt keine Unterstützung für die universelle Windows-Plattform. Die IOS-und Android-Projekte erfordern einen Verweis auf die **opentk-1,0-** Assembly oder die **opentk** -Version 1.0.0.0 Assembly. `OpenGLView` ist in einem freigegebenen Projekt einfacher zu verwenden. Wenn Sie in einer .NET Standard Bibliothek verwendet wird, ist auch ein Abhängigkeits Dienst erforderlich (wie im Beispielcode gezeigt).<br /><br />Dies ist die einzige Grafik Funktion, die in xamarin. Forms integriert ist, aber eine xamarin. Forms-Anwendung kann auch Grafiken mithilfe von [`SkiaSharp`](~/xamarin-forms/user-interface/graphics/skiasharp/index.md)oder [`UrhoSharp`](~/xamarin-forms/user-interface/graphics/urhosharp.md)Renderen.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.OpenGLView)<br /><br /> | [![Openglview-Beispiel](views-images/OpenGLView.png "Openglview-Beispiel")](views-images/OpenGLView-Large.png#lightbox "Openglview-Beispiel")<br />Code für diese Seite / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/OpenGLViewDemoPage.xaml) mit [Code Behind](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/OpenGLViewDemoPage.xaml.cs) [ C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/OpenGLViewDemoPage.cs) |
|     |     |

### <a name="map"></a>Karte

|     |     |
| --- | --- |
| [`Map`](xref:Xamarin.Forms.Maps.Map) eine Karte anzeigt. Das **xamarin. Forms. Maps** -nuget-Paket muss installiert sein. Android und die universelle Windows-Plattform erforderlich, einen Map-Autorisierungsschlüssel.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.Maps.Map) / [Handbuch](~/xamarin-forms/user-interface/map/index.md) / [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps/) | [![Map-Beispiel](views-images/Map.png "Map-Beispiel")](views-images/Map-Large.png#lightbox "Map-Beispiel")<br />Code für diese Seite / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/MapDemoPage.xaml) [ C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/MapDemoPage.cs) |
|     |     |

### <a name="mediaelement"></a>MediaElement

|     |     |
| --- | --- |
| [`MediaElement`](xref:Xamarin.Forms.MediaElement) Wiedergabe von Video-oder Audiodaten. Medien können über eine URL oder eine lokale Datei abgespielt werden, je nachdem, ob die [`Source`](xref:Xamarin.Forms.MediaElement.Source) -Eigenschaft auf einen [`UriMediaSource`](xref:Xamarin.Forms.UriMediaSource) oder eine [`FileMediaSource`](xref:Xamarin.Forms.FileMediaSource)festgelegt ist.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.MediaElement) / [Handbuch](~/xamarin-forms/user-interface/mediaelement.md) / [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-mediaelementdemos) | [![Media Element-Beispiel](views-images/MediaElement.png "Media Element-Beispiel")](views-images/MediaElement-Large.png#lightbox "Media Element-Beispiel")<br />Code für diese Seite / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/MediaElementDemoPage.xaml) [ C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/MediaElementDemoPage.cs) |
|     |     |

## <a name="views-that-initiate-commands"></a>Ansichten, die Befehle initiieren

### <a name="button"></a>Taste

|     |     |
| --- | --- |
| [`Button`](xref:Xamarin.Forms.Button) ist ein rechteckiges Objekt, das Text anzeigt und ein [`Clicked`](xref:Xamarin.Forms.Button.Clicked) Ereignis auslöst, wenn es gedrückt wird.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.Button) / [Handbuch](~/xamarin-forms/user-interface/button.md) / [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-buttondemos/) | [![Schaltflächen Beispiel](views-images/Button.png "Schaltflächen Beispiel")](views-images/Button-Large.png#lightbox "Schaltflächen Beispiel")<br /> Code für diese Seite / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ButtonDemoPage.xaml) mit [Code Behind](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ButtonDemoPage.xaml.cs) [ C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ButtonDemoPage.cs) |
|     |     |

### <a name="imagebutton"></a>ImageButton

|     |     |
| --- | --- |
| `ImageButton` ist ein rechteckiges Objekt, das ein Bild anzeigt und ein `Clicked` Ereignis auslöst, wenn es gedrückt wird.<br /><br /> [Leitfaden](~/xamarin-forms/user-interface/imagebutton.md) / [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery) | [![ImageButton-Beispiel](views-images/ImageButton.png "ImageButton-Beispiel")](views-images/ImageButton-Large.png#lightbox "ImageButton-Beispiel")<br /> Code für diese Seite / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ImageButtonDemoPage.xaml) mit [Code Behind](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ImageButtonDemoPage.xaml.cs) [ C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ImageButtonDemoPage.cs) |
|     |     |

### <a name="refreshview"></a>RefreshView

|     |     |
| --- | --- |
| `RefreshView` ist ein Container Steuerelement, das Pull-to-Refresh-Funktionen für scrollbaren Inhalt bereitstellt. Die von der `Command`-Eigenschaft definierte `ICommand` wird ausgeführt, wenn eine Aktualisierung ausgelöst wird, und die `IsRefreshing`-Eigenschaft gibt den aktuellen Zustand des Steuer Elements an.<br /><br /> [Leitfaden](~/xamarin-forms/user-interface/refreshview.md) / [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery) | [![Beispiel für eine aktuaktu](views-images/RefreshView.png "Beispiel für eine aktuaktu")](views-images/RefreshView-Large.png#lightbox "Beispiel für eine aktuaktu")<br /> Code für diese Seite / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/RefreshViewDemoPage.xaml) mit [Code Behind](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/RefreshViewDemoPage.xaml.cs) [ C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/RefreshViewDemoPage.cs) |
|     |     |

### <a name="searchbar"></a>SearchBar

|     |     |
| --- | --- |
| in [`SearchBar`](xref:Xamarin.Forms.SearchBar) wird ein Bereich angezeigt, in dem der Benutzer eine Text Zeichenfolge eingeben kann, sowie eine Schaltfläche (oder eine Tastatur Taste), die der Anwendung signalisiert, eine Suche auszuführen. Die [`Text`](xref:Xamarin.Forms.InputView.Text) -Eigenschaft ermöglicht den Zugriff auf den Text, und das [`SearchButtonPressed`](xref:Xamarin.Forms.SearchBar.SearchButtonPressed) -Ereignis gibt an, dass die Schaltfläche gedrückt wurde.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.SearchBar) / [Handbuch](~/xamarin-forms/user-interface/searchbar.md) / [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-searchbardemos/) | [![Beispiel für eine Searchbar](views-images/SearchBar.png "Beispiel für eine Searchbar")](views-images/SearchBar-Large.png#lightbox "Beispiel für eine Searchbar")<br /> Code für diese Seite / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SearchBarDemoPage.xaml) mit [Code Behind](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SearchBarDemoPage.xaml.cs) [ C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SearchBarDemoPage.cs) |
|     |     |

### <a name="swipeview"></a>SwipeView

|     |     |
| --- | --- |
| `SwipeView` ist ein Container Steuerelement, das ein Inhalts Element umschließt und Kontextmenü Elemente bereitstellt, die durch eine Schwenkbewegung angezeigt werden. Jedes Menü Element wird durch einen `SwipeItem`dargestellt, der über eine `Command` Eigenschaft verfügt, die eine `ICommand` ausführt, wenn das Element getippt wird.<br /><br /> [Leitfaden](~/xamarin-forms/user-interface/swipeview.md) / [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery) | [![Swipeer View-Beispiel](views-images/SwipeView.png "Swipeer View-Beispiel")](views-images/SwipeView-Large.png#lightbox "Swipeer View-Beispiel")<br /> Code für diese Seite / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SwipeViewDemoPage.xaml) mit [Code Behind](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SwipeViewDemoPage.xaml.cs) [ C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SwipeViewDemoPage.cs) |
|     |     |

## <a name="views-for-setting-values"></a>Ansichten zum Festlegen von Werten

### <a name="checkbox"></a>CheckBox

|     |     |
| --- | --- |
| `CheckBox` ermöglicht dem Benutzer die Auswahl eines booleschen Werts mithilfe eines Schaltflächen Typs, der entweder aktiviert oder leer ist. Die `IsChecked`-Eigenschaft ist der Status des `CheckBox`, und das `CheckedChanged`-Ereignis wird ausgelöst, wenn sich der Status ändert.<br /><br />API-Dokumentation/ [Handbuch](~/xamarin-forms/user-interface/checkbox.md) / [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-checkboxdemos) | [![CheckBox-Beispiel](views-images/CheckBox.png "CheckBox-Beispiel")](views-images/CheckBox-Large.png#lightbox "CheckBox-Beispiel")<br />Code für diese Seite / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/CheckBoxPage.xaml) [ C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/CheckBoxPage.cs) |
|     |     |

### <a name="slider"></a>Schieberegler

|     |     |
| --- | --- |
| [`Slider`](xref:Xamarin.Forms.Slider) ermöglicht dem Benutzer die Auswahl eines `double` Werts aus einem kontinuierlichen Bereich, der mit den Eigenschaften [`Minimum`](xref:Xamarin.Forms.Slider.Minimum) und [`Maximum`](xref:Xamarin.Forms.Slider.Maximum) angegeben wird.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.Slider) / [Handbuch](~/xamarin-forms/user-interface/slider.md) / [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-sliderdemos) | [![Beispiel für Schieberegler](views-images/Slider.png "Beispiel für Schieberegler")](views-images/Slider-Large.png#lightbox "Beispiel für Schieberegler")<br />Code für diese Seite / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SliderDemoPage.xaml) [ C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SliderDemoPage.cs) |
|     |     |

### <a name="stepper"></a>Stepper

|     |     |
| --- | --- |
| [`Stepper`](xref:Xamarin.Forms.Stepper) ermöglicht dem Benutzer das Auswählen eines `double` Werts aus einem Bereich von inkrementellen Werten, die mit den Eigenschaften [`Minimum`](xref:Xamarin.Forms.Stepper.Minimum), [`Maximum`](xref:Xamarin.Forms.Stepper.Maximum)und [`Increment`](xref:Xamarin.Forms.Stepper.Increment) angegeben werden.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.Stepper)  / [Handbuch](~/xamarin-forms/user-interface/stepper.md) / [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-stepperdemos) | [![Beispiel für Stepper](views-images/Stepper.png "Beispiel für Stepper")](views-images/Stepper-Large.png#lightbox "Beispiel für Stepper")<br />Code für diese Seite / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/StepperDemoPage.xaml) [ C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/StepperDemoPage.cs) |
|     |     |

### <a name="switch"></a>Schalter

|     |     |
| --- | --- |
| [`Switch`](xref:Xamarin.Forms.Switch) hat die Form eines ein-/ausschalten-Schalters, um dem Benutzer die Auswahl eines booleschen Werts zu ermöglichen. Die [`IsToggled`](xref:Xamarin.Forms.Switch.IsToggled) -Eigenschaft ist der Status des Schalters, und das [`Toggled`](xref:Xamarin.Forms.Switch.Toggled) -Ereignis wird ausgelöst, wenn sich der Status ändert.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.Switch) / [Handbuch](~/xamarin-forms/user-interface/switch.md) / [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-switchdemos/) | [![Switch-Beispiel](views-images/Switch.png "Switch-Beispiel")](views-images/Switch-Large.png#lightbox "Switch-Beispiel")<br />Code für diese Seite / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SwitchDemoPage.xaml) [ C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SwitchDemoPage.cs) |
|     |     |

### <a name="datepicker"></a>DatePicker

|     |     |
| --- | --- |
| [`DatePicker`](xref:Xamarin.Forms.DatePicker) ermöglicht dem Benutzer, ein Datum mit der Platt Form Datumsauswahl auszuwählen. Legen Sie einen Bereich zulässiger Datumsangaben mit den Eigenschaften [`MinimumDate`](xref:Xamarin.Forms.DatePicker.MinimumDate) und [`MaximumDate`](xref:Xamarin.Forms.DatePicker.MaximumDate) fest. Die [`Date`](xref:Xamarin.Forms.DatePicker.Date) -Eigenschaft ist das ausgewählte Datum, und das [`DateSelected`](xref:Xamarin.Forms.DatePicker.DateSelected) -Ereignis wird ausgelöst, wenn sich die Eigenschaft ändert.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.DatePicker) / [Handbuch](~/xamarin-forms/user-interface/datepicker.md) / [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-datepicker) | [![DatePicker-Beispiel](views-images/DatePicker.png "DatePicker-Beispiel")](views-images/DatePicker-Large.png#lightbox "DatePicker-Beispiel")<br />Code für diese Seite / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/DatePickerDemoPage.xaml) [ C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/DatePickerDemoPage.cs) |
|     |     |

### <a name="timepicker"></a>TimePicker

|     |     |
| --- | --- |
| [`TimePicker`](xref:Xamarin.Forms.TimePicker) ermöglicht es dem Benutzer, eine Uhrzeit mit der Platt Form Zeitauswahl auszuwählen. Die [`Time`](xref:Xamarin.Forms.TimePicker.Time) -Eigenschaft ist die ausgewählte Uhrzeit. Eine Anwendung kann Änderungen in der `Time`-Eigenschaft überwachen, indem Sie einen Handler für das [`PropertyChanged`](xref:Xamarin.Forms.BindableObject.PropertyChanged) -Ereignis installiert.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.TimePicker) / [Handbuch](~/xamarin-forms/user-interface/timepicker.md) / [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-timepicker) | [![TimePicker-Beispiel](views-images/TimePicker.png "TimePicker-Beispiel")](views-images/TimePicker-Large.png#lightbox "TimePicker-Beispiel")<br />Code für diese Seite / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TimePickerDemoPage.xaml) [ C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TimePickerDemoPage.cs) |
|     |     |

## <a name="views-for-editing-text"></a>Ansichten zum Bearbeiten von Text

Diese beiden Klassen werden von der [`InputView`](xref:Xamarin.Forms.InputView) -Klasse abgeleitet, die die [`Keyboard`](xref:Xamarin.Forms.InputView.Keyboard) -Eigenschaft definiert.

### <a name="entry"></a>Eintrag

|     |     |
| --- | --- |
| [`Entry`](xref:Xamarin.Forms.Entry) ermöglicht es dem Benutzer, eine einzelne Textzeile einzugeben und zu bearbeiten. Der Text ist als [`Text`](xref:Xamarin.Forms.InputView.Text) -Eigenschaft verfügbar, und die Ereignisse [`TextChanged`](xref:Xamarin.Forms.InputView.TextChanged) und [`Completed`](xref:Xamarin.Forms.Entry.Completed) werden ausgelöst, wenn der Text geändert wird, oder der Benutzer signalisiert den Abschluss, indem er auf die EINGABETASTE klickt.<br /><br />Verwenden Sie eine [`Editor`](#editor) , um mehrere Textzeilen einzugeben und zu bearbeiten.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.Entry) / [Handbuch](~/xamarin-forms/user-interface/text/entry.md) / [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text) | [![Beispiel für den Eintrag](views-images/Entry.png "Beispiel für den Eintrag")](views-images/Entry-Large.png#lightbox "Beispiel für den Eintrag")<br />Code für diese Seite / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/EntryDemoPage.xaml) [ C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/EntryDemoPage.cs) |
|     |     |

### <a name="editor"></a>Editor

|     |     |
| --- | --- |
| [`Editor`](xref:Xamarin.Forms.Editor) ermöglicht es dem Benutzer, mehrere Textzeilen einzugeben und zu bearbeiten. Der Text ist als [`Text`](xref:Xamarin.Forms.InputView.Text) -Eigenschaft verfügbar, und die Ereignisse [`TextChanged`](xref:Xamarin.Forms.InputView.TextChanged) und [`Completed`](xref:Xamarin.Forms.Editor.Completed) werden ausgelöst, wenn sich der Text ändert oder der Benutzer den Abschluss signalisiert.<br /><br />Verwenden Sie eine [`Entry`](#entry) Ansicht, um eine einzelne Textzeile einzugeben und zu bearbeiten.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.Editor) / [Handbuch](~/xamarin-forms/user-interface/text/editor.md) / [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text) | [![Beispiel für den Eintrag](views-images/Editor.png "Editor-Beispiel")](views-images/Editor-Large.png#lightbox "Editor-Beispiel")<br />Code für diese Seite / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/EditorDemoPage.xaml) [ C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/EditorDemoPage.cs) |
|     |     |

## <a name="views-to-indicate-activity"></a>Ansichten zum Anzeigen einer Aktivität

### <a name="activityindicator"></a>ActivityIndicator

|     |     |
| --- | --- |
| in [`ActivityIndicator`](xref:Xamarin.Forms.ActivityIndicator) wird eine Animation verwendet, um anzuzeigen, dass die Anwendung mit einer langwierigen Aktivität beschäftigt ist, ohne dass der Fortschritt angegeben wird. Die [`IsRunning`](xref:Xamarin.Forms.ActivityIndicator.IsRunning) -Eigenschaft steuert die Animation.<br /><br />Wenn der Status der Aktivität bekannt ist, verwenden Sie stattdessen einen [`ProgressBar`](#progressbar) .<br /><br />[API-Dokumentation](xref:Xamarin.Forms.ActivityIndicator) / [Handbuch](~/xamarin-forms/user-interface/activityindicator.md) / [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-activityindicatordemos/) | [![Beispiel für activityindicator](views-images/ActivityIndicator.png "Beispiel für activityindicator")](views-images/ActivityIndicator-Large.png#lightbox "Beispiel für activityindicator")<br />Code für diese Seite / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ActivityIndicatorDemoPage.xaml) [ C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ActivityIndicatorDemoPage.cs) |
|     |     |

### <a name="progressbar"></a>Statusanzeige

|     |     |
| --- | --- |
| [`ProgressBar`](xref:Xamarin.Forms.ProgressBar) verwendet eine Animation, um anzuzeigen, dass die Anwendung durch eine lange Aktivität verläuft. Legen Sie die [`Progress`](xref:Xamarin.Forms.ProgressBar.Progress) -Eigenschaft auf Werte zwischen 0 und 1 fest, um den Fortschritt anzuzeigen.<br /><br />Wenn der Fortschritt der Aktivität nicht bekannt ist, verwenden Sie stattdessen eine- [`ActivityIndicator`](#activityindicator) .<br /><br />[API-Dokumentation](xref:Xamarin.Forms.ProgressBar) / [Handbuch](~/xamarin-forms/user-interface/progressbar.md) / [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-progressbardemos/) | [![ProgressBar-Beispiel](views-images/ProgressBar.png "ProgressBar-Beispiel")](views-images/ProgressBar-Large.png#lightbox "ProgressBar-Beispiel")<br />Code für diese Seite / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ProgressBarDemoPage.xaml) mit [Code Behind](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ProgressBarDemoPage.xaml.cs) [ C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ProgressBarDemoPage.cs) |
|     |     |

## <a name="views-that-display-collections"></a>Ansichten, die Sammlungen anzeigen

### <a name="carouselview"></a>CarouselView

|     |     |
| --- | --- |
| [`CarouselView`](xref:Xamarin.Forms.CarouselView) zeigt eine scrollbare Liste von Datenelementen an. Legen Sie die `ItemsSource`-Eigenschaft auf eine Auflistung von-Objekten fest, und legen Sie die `ItemTemplate`-Eigenschaft auf ein [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) Objekt fest, das beschreibt, wie die Elemente formatiert werden sollen. Das `CurrentItemChanged`-Ereignis signalisiert, dass sich das aktuell angezeigte Element geändert hat, das als `CurrentItem`-Eigenschaft verfügbar ist.<br /><br />[Leitfaden](~/xamarin-forms/user-interface/carouselview/index.md) / [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-carouselviewdemos/) | [![Carouselview-Beispiel](views-images/CarouselView.png "Carouselview-Beispiel")](views-images/CarouselView-Large.png#lightbox "Carouselview-Beispiel")<br />Code für diese Seite / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/CarouselViewDemoPage.xaml) [ C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/CarouselViewDemoPage.cs) |
|     |     |

### <a name="collectionview"></a>CollectionView

|     |     |
| --- | --- |
| [`CollectionView`](xref:Xamarin.Forms.CollectionView) zeigt eine Bild lauffähigen Liste auswählbarer Datenelemente mit unterschiedlichen layoutspezifikationen an. Es zielt darauf ab, eine flexiblere und leistungsfähigere Alternative zu [`ListView`](xref:Xamarin.Forms.ListView)bereitzustellen. Legen Sie die `ItemsSource`-Eigenschaft auf eine Auflistung von-Objekten fest, und legen Sie die `ItemTemplate`-Eigenschaft auf ein [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) Objekt fest, das beschreibt, wie die Elemente formatiert werden sollen. Das `SelectionChanged`-Ereignis signalisiert, dass eine Auswahl getroffen wurde, die als `SelectedItem`-Eigenschaft verfügbar ist.<br /><br />[Leitfaden](~/xamarin-forms/user-interface/collectionview/index.md) / [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/) | [![CollectionView-Beispiel](views-images/CollectionView.png "CollectionView-Beispiel")](views-images/CollectionView-Large.png#lightbox "CollectionView-Beispiel")<br />Code für diese Seite / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/CollectionViewDemoPage.xaml) [ C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/CollectionViewDemoPage.cs) |
|     |     |

### <a name="indicatorview"></a>IndicatorView

|     |     |
| --- | --- |
| `IndicatorView` zeigt Indikatoren an, die die Anzahl der Elemente in einer `CarouselView`darstellen. Legen Sie die `CarouselView.IndicatorView`-Eigenschaft auf das `IndicatorView`-Objekt fest, um Indikatoren für die `CarouselView`anzuzeigen. <br /><br />[Leitfaden](~/xamarin-forms/user-interface/indicatorview.md) / [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-indicatorviewdemos/) | [![Beispiel für die Anzeige Ansicht](views-images/IndicatorView.png "Beispiel für die Anzeige Ansicht")](views-images/IndicatorView-Large.png#lightbox "Beispiel für die Anzeige Ansicht")<br />Code für diese Seite / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/IndicatorViewDemoPage.xaml) [ C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/IndicatorViewDemoPage.cs) |
|     |     |

### <a name="listview"></a>ListView

|     |     |
| --- | --- |
| [`ListView`](xref:Xamarin.Forms.ListView) wird von [`ItemsView`](xref:Xamarin.Forms.ItemsView`1) abgeleitet und zeigt eine Bild lauffähigen Liste auswählbarer Datenelemente an. Legen Sie die [`ItemsSource`](xref:Xamarin.Forms.ItemsView`1.ItemsSource) -Eigenschaft auf eine Auflistung von-Objekten fest, und legen Sie die [`ItemTemplate`](xref:Xamarin.Forms.ItemsView`1.ItemTemplate) -Eigenschaft auf ein [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) Objekt fest, das beschreibt, wie die Elemente formatiert werden sollen. Das [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) -Ereignis signalisiert, dass eine Auswahl getroffen wurde, die als [`SelectedItem`](xref:Xamarin.Forms.ListView.SelectedItem) -Eigenschaft verfügbar ist.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.ListView) / [Handbuch](~/xamarin-forms/user-interface/listview/index.md) / [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithlistview/) | [![ListView-Beispiel](views-images/ListView.png "ListView-Beispiel")](views-images/ListView-Large.png#lightbox "ListView-Beispiel")<br />Code für diese Seite / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ListViewDemoPage.xaml) [ C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ListViewDemoPage.cs) |
|     |     |

### <a name="picker"></a>Auswahl

|     |     |
| --- | --- |
| [`Picker`](xref:Xamarin.Forms.Picker) zeigt ein ausgewähltes Element aus einer Liste von Text Zeichenfolgen an und ermöglicht das Auswählen dieses Elements, wenn die Ansicht getippt wird. Legen Sie die [`Items`](xref:Xamarin.Forms.Picker.Items) -Eigenschaft auf eine Liste von Zeichen folgen oder die [`ItemsSource`](xref:Xamarin.Forms.Picker.ItemsSource) -Eigenschaft auf eine Auflistung von-Objekten fest. Das [`SelectedIndexChanged`](xref:Xamarin.Forms.Picker.SelectedIndexChanged) Ereignis wird ausgelöst, wenn ein Element ausgewählt wird.<br /><br />Die `Picker` zeigt die Liste der Elemente nur an, wenn Sie ausgewählt wird. Verwenden Sie eine [`ListView`](#listview) oder [`TableView`](#tableview) für eine Bild lauffähigen Liste, die auf der Seite verbleibt.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.Picker) / [Handbuch](~/xamarin-forms/user-interface/picker/index.md) / [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-pickerdemo) | [![Auswahl Beispiel](views-images/Picker.png "Auswahl Beispiel")](views-images/Picker-Large.png#lightbox "Auswahl Beispiel")<br />Code für diese Seite / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/PickerDemoPage.xaml) mit [Code Behind](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/PickerDemoPage.xaml.cs) [ C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/PickerDemoPage.cs) |
|     |     |

### <a name="tableview"></a>TableView

|     |     |
| --- | --- |
| in [`TableView`](xref:Xamarin.Forms.TableView) wird eine Liste der Zeilen vom Typ [`Cell`](xref:Xamarin.Forms.Cell) mit optionalen Headern und unter Headern angezeigt. Legen Sie die [`Root`](xref:Xamarin.Forms.TableView.Root) -Eigenschaft auf ein Objekt vom Typ [`TableRoot`](xref:Xamarin.Forms.TableRoot)fest, und fügen Sie diesem `TableRoot`[`TableSection`](xref:Xamarin.Forms.TableSection) Objekte hinzu. Jede `TableSection` ist eine Auflistung von `Cell` Objekten.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.TableView) / [Handbuch](~/xamarin-forms/user-interface/tableview.md) / [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-tableview) | [![TableView-Beispiel](views-images/TableView.png "TableView-Beispiel")](views-images/TableView-Large.png#lightbox "TableView-Beispiel")<br />Code für diese Seite / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TableViewFormDemoPage.xaml) [ C# ](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TableViewFormDemoPage.cs) |
|     |     |

## <a name="related-links"></a>Verwandte Links

- [Xamarin. Forms formsgallery-Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)
- [Xamarin.Forms Samples (Beispiele für Xamarin.Forms)](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.Forms)
- [Xamarin.Forms-API-Dokumentation](https://docs.microsoft.com/dotnet/api/xamarin.forms?view=xamarin-forms)
