---
title: Xamarin.FormsAnsichten
description: Xamarin.FormsAnsichten sind die Bausteine von plattformübergreifenden mobilen Benutzeroberflächen. Dieser Artikel listet die Ansichten auf, die in enthalten sind Xamarin.Forms .
ms.prod: xamarin
ms.assetid: AC070686-A423-4A98-8BB6-0B9F94C062CC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/16/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 8c15626e405645d28a785c32d276860f9751ea25
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84132378"
---
# <a name="xamarinforms-views"></a>Xamarin.FormsAnsichten

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery/)

_Xamarin. Forms-Ansichten sind die Bausteine von plattformübergreifenden mobilen Benutzeroberflächen._

Sichten sind Benutzeroberflächen Objekte, wie z. b. Bezeichnungen, Schaltflächen und Schieberegler, die in anderen grafischen Programmierumgebungen häufig als Steuer *Elemente* oder *Widgets* bezeichnet werden. Die von allen unterstützten Sichten werden Xamarin.Forms von der- [`View`](xref:Xamarin.Forms.View) Klasse abgeleitet. Sie können in verschiedene Kategorien unterteilt werden:

## <a name="views-for-presentation"></a>Ansichten für die Präsentation

### <a name="boxview"></a>BoxView

|     |    |
| --- | ---|
| [`BoxView`](xref:Xamarin.Forms.BoxView)zeigt ein durch die-Eigenschaft farbiges solides Rechteck an [`Color`](xref:Xamarin.Forms.BoxView.Color) . `BoxView`hat eine Standardgröße von 40 x 40 Anforderungen. Weisen Sie für andere Größen die [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) Eigenschaften und zu [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) .<br /><br />[API-Dokumentation](xref:Xamarin.Forms.BoxView)  /  [Leitfaden](~/xamarin-forms/user-interface/boxview.md)  /  [Beispiel 1](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-basicboxview), [2](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-textdecoration), [3](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-listviewcolors/), [4](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-gameoflife), [5](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-dotmatrixclock)und [6](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-boxviewclock) | [![Boxview-Beispiel](views-images/BoxView.png "Boxview-Beispiel")](views-images/BoxView-Large.png#lightbox "Boxview-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/BoxViewDemoPage.cs)  /  [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/BoxViewDemoPage.xaml) |
|     |     |

### <a name="expander"></a>Expander

|     |     |
| --- | --- |
| `Expander`stellt einen erweiterbaren Container zum Hosten beliebiger Inhalte bereit und besteht aus einem Header und Inhalt. Legen `Header` Sie die-Eigenschaft auf einen fest, der [`View`](xref:Xamarin.Forms.View) als-Header angezeigt wird, und die- `Content` Eigenschaft auf einen [`View`](xref:Xamarin.Forms.View) , der angezeigt wird, wenn der Header durch Tippen erweitert wird.<br /><br />[Leitfaden](~/xamarin-forms/user-interface/expander.md)  /  [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-expanderdemos) | [![Expander-Beispiel](views-images/Expander.png "Expander-Beispiel")](views-images/Expander-Large.png#lightbox "Expander-Beispiel")<br /> [C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ExpanderDemoPage.cs)  /  [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ExpanderDemoPage.xaml) |
|     |     |

### <a name="label"></a>Bezeichnung

|     |     |
| --- | --- |
| [`Label`](xref:Xamarin.Forms.Label)zeigt einzeilige Text Zeichenfolgen oder mehrzeilige Textblöcke an, entweder mit konstanter oder variabler Formatierung. Legen Sie die- [`Text`](xref:Xamarin.Forms.Label.Text) Eigenschaft auf eine Zeichenfolge für die Konstante Formatierung fest, oder legen Sie die- [`FormattedText`](xref:Xamarin.Forms.Label.FormattedText) Eigenschaft auf ein- [`FormattedString`](xref:Xamarin.Forms.FormattedString) Objekt zur Variablen Formatierung fest<br /><br />[API-Dokumentation](xref:Xamarin.Forms.Label)  /  [Leitfaden](~/xamarin-forms/user-interface/text/label.md)  /  [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text) | [![Beispiel für eine Bezeichnung](views-images/Label.png "Beispiel für eine Bezeichnung")](views-images/Label-Large.png#lightbox "Beispiel für eine Bezeichnung")<br /> [C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/LabelDemoPage.cs)  /  [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/LabelDemoPage.xaml) |
|     |     |

### <a name="image"></a>Image

|     |     |
| --- | --- |
| [`Image`](xref:Xamarin.Forms.Image)zeigt eine Bitmap an. Bitmaps können über das Internet heruntergeladen, als Ressourcen in die gängigen Projekt-oder Platt Form Projekte eingebettet oder mithilfe eines .NET-Objekts erstellt werden `Stream` .<br /><br />[API-Dokumentation](xref:Xamarin.Forms.Image)  /  [Leitfaden](~/xamarin-forms/user-interface/images.md)  /  [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithimages) | [![Bildbeispiel](views-images/Image.png "Bildbeispiel")](views-images/Image-Large.png#lightbox "Bildbeispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ImageDemoPage.cs)  /  [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ImageDemoPage.xaml) |
|     |     |

### <a name="map"></a>Karte

|     |     |
| --- | --- |
| [`Map`](xref:Xamarin.Forms.Maps.Map)zeigt eine Karte an. Die ** Xamarin.Forms . Das Karten** -nuget-Paket muss installiert sein. Android und universelle Windows-Plattform erfordern einen Karten Autorisierungs Schlüssel.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.Maps.Map)  /  [Leitfaden](~/xamarin-forms/user-interface/map/index.md)  /  [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps/) | [![Map-Beispiel](views-images/Map.png "Map-Beispiel")](views-images/Map-Large.png#lightbox "Map-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/MapDemoPage.cs)  /  [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/MapDemoPage.xaml) |
|     |     |

### <a name="mediaelement"></a>MediaElement

|     |     |
| --- | --- |
| [`MediaElement`](xref:Xamarin.Forms.MediaElement)gibt Video oder Audiodatei wieder. Medien können über eine URL oder eine lokale Datei abgespielt werden, je nachdem, ob die- [`Source`](xref:Xamarin.Forms.MediaElement.Source) Eigenschaft auf oder festgelegt ist [`UriMediaSource`](xref:Xamarin.Forms.UriMediaSource) [`FileMediaSource`](xref:Xamarin.Forms.FileMediaSource) .<br /><br />[API-Dokumentation](xref:Xamarin.Forms.MediaElement)  /  [Leitfaden](~/xamarin-forms/user-interface/mediaelement.md)  /  [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-mediaelementdemos) | [![Media Element-Beispiel](views-images/MediaElement.png "Media Element-Beispiel")](views-images/MediaElement-Large.png#lightbox "Media Element-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/MediaElementDemoPage.cs)  /  [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/MediaElementDemoPage.xaml) |
|     |     |

### <a name="openglview"></a>Openglview

|     |     |
| --- | --- |
| [`OpenGLView`](xref:Xamarin.Forms.OpenGLView)zeigt OpenGL-Grafiken in IOS-und Android-Projekten an. Die universelle Windows-Plattform wird nicht unterstützt. Die IOS-und Android-Projekte erfordern einen Verweis auf die **opentk-1,0-** Assembly oder die **opentk** -Version 1.0.0.0 Assembly. `OpenGLView`ist in einem freigegebenen Projekt einfacher zu verwenden. Wenn Sie in einer .NET Standard Bibliothek verwendet wird, ist auch ein Abhängigkeits Dienst erforderlich (wie im Beispielcode gezeigt).<br /><br />Dies ist die einzige Grafik Funktion, die in integriert ist Xamarin.Forms . eine- Xamarin.Forms Anwendung kann jedoch auch Grafiken mithilfe von [`SkiaSharp`](~/xamarin-forms/user-interface/graphics/skiasharp/index.md) , oder renderrender. [`UrhoSharp`](~/xamarin-forms/user-interface/graphics/urhosharp.md)<br /><br />[API-Dokumentation](xref:Xamarin.Forms.OpenGLView)<br /><br /> | [![Openglview-Beispiel](views-images/OpenGLView.png "Openglview-Beispiel")](views-images/OpenGLView-Large.png#lightbox "Openglview-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/OpenGLViewDemoPage.cs)  /  [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/OpenGLViewDemoPage.xaml) mit [Code Behind](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/OpenGLViewDemoPage.xaml.cs) |
|     |     |

### <a name="webview"></a>WebView

|     |     |
| --- | --- |
| [`WebView`](xref:Xamarin.Forms.WebView)zeigt Webseiten oder HTML-Inhalt an, je nachdem, ob die- [`Source`](xref:Xamarin.Forms.WebView.Source) Eigenschaft auf ein- [`UriWebViewSource`](xref:Xamarin.Forms.UrlWebViewSource) Objekt oder ein-Objekt festgelegt ist [`HtmlWebViewSource`](xref:Xamarin.Forms.HtmlWebViewSource) .<br /><br />[API-Dokumentation](xref:Xamarin.Forms.WebView)  /  [Leitfaden](~/xamarin-forms/user-interface/webview.md)  /  [Beispiel 1](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithwebview) und [2](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-webview) | [![WebView-Beispiel](views-images/WebView.png "WebView-Beispiel")](views-images/WebView-Large.png#lightbox "WebView-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/WebViewDemoPage.cs)  /  [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/WebViewDemoPage.xaml) |
|     |     |

## <a name="views-that-initiate-commands"></a>Ansichten, die Befehle initiieren

### <a name="button"></a>Schaltfläche

|     |     |
| --- | --- |
| [`Button`](xref:Xamarin.Forms.Button)ist ein rechteckiges Objekt, das Text anzeigt und das ein [`Clicked`](xref:Xamarin.Forms.Button.Clicked) -Ereignis auslöst, wenn es gedrückt wird.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.Button)  /  [Leitfaden](~/xamarin-forms/user-interface/button.md)  /  [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-buttondemos/) | [![Schaltflächen Beispiel](views-images/Button.png "Schaltflächen Beispiel")](views-images/Button-Large.png#lightbox "Schaltflächen Beispiel")<br /> [C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ButtonDemoPage.cs)  /  [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ButtonDemoPage.xaml) mit [Code Behind](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ButtonDemoPage.xaml.cs) |
|     |     |

### <a name="imagebutton"></a>ImageButton

|     |     |
| --- | --- |
| `ImageButton`ist ein rechteckiges Objekt, das ein Bild anzeigt und ein `Clicked` -Ereignis auslöst, wenn es gedrückt wird.<br /><br /> [Leitfaden](~/xamarin-forms/user-interface/imagebutton.md)  /  [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery) | [![ImageButton-Beispiel](views-images/ImageButton.png "ImageButton-Beispiel")](views-images/ImageButton-Large.png#lightbox "ImageButton-Beispiel")<br /> [C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ImageButtonDemoPage.cs)  /  [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ImageButtonDemoPage.xaml) mit [Code Behind](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ImageButtonDemoPage.xaml.cs) |
|     |     |

### <a name="radiobutton"></a>RadioButton

|     |     |
| --- | --- |
| `RadioButton`ermöglicht die Auswahl einer Option aus einer Menge und löst ein-Ereignis aus, `CheckedChanged` Wenn die Auswahl erfolgt.<br /><br />[Leitfaden](~/xamarin-forms/user-interface/radiobutton.md)  /  [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-radiobuttondemos/) | [![RadioButton-Beispiel](views-images/RadioButton.png "RadioButton-Beispiel")](views-images/RadioButton-Large.png#lightbox "RadioButton-Beispiel")<br /> [C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/RadioButtonDemoPage.cs)  /  [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/RadioButtonDemoPage.xaml) mit [Code Behind](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/RadioButtonDemoPage.xaml.cs) |
|     |     |

### <a name="refreshview"></a>RefreshView

|     |     |
| --- | --- |
| `RefreshView`ist ein Container Steuerelement, das Pull-to-Refresh-Funktionen für scrollbaren Inhalt bereitstellt. Der `ICommand` , der durch die- `Command` Eigenschaft definiert wird, wird ausgeführt, wenn eine Aktualisierung ausgelöst wird, und die- `IsRefreshing` Eigenschaft gibt den aktuellen Zustand des Steuer Elements an.<br /><br /> [Leitfaden](~/xamarin-forms/user-interface/refreshview.md)  /  [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery) | [![Beispiel für eine aktuaktu](views-images/RefreshView.png "Beispiel für eine aktuaktu")](views-images/RefreshView-Large.png#lightbox "Beispiel für eine aktuaktu")<br /> [C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/RefreshViewDemoPage.cs)  /  [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/RefreshViewDemoPage.xaml) mit [Code Behind](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/RefreshViewDemoPage.xaml.cs) |
|     |     |

### <a name="searchbar"></a>SearchBar

|     |     |
| --- | --- |
| [`SearchBar`](xref:Xamarin.Forms.SearchBar)zeigt einen Bereich an, in dem der Benutzer eine Text Zeichenfolge eingeben kann, sowie eine Schaltfläche (oder eine Tastatur Taste), die der Anwendung signalisiert, eine Suche auszuführen. Die [`Text`](xref:Xamarin.Forms.InputView.Text) -Eigenschaft ermöglicht den Zugriff auf den Text, und das- [`SearchButtonPressed`](xref:Xamarin.Forms.SearchBar.SearchButtonPressed) Ereignis gibt an, dass die Schaltfläche gedrückt wurde.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.SearchBar)  /  [Leitfaden](~/xamarin-forms/user-interface/searchbar.md)  /  [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-searchbardemos/) | [![Beispiel für eine Searchbar](views-images/SearchBar.png "Beispiel für eine Searchbar")](views-images/SearchBar-Large.png#lightbox "Beispiel für eine Searchbar")<br /> [C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SearchBarDemoPage.cs)  /  [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SearchBarDemoPage.xaml) mit [Code Behind](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SearchBarDemoPage.xaml.cs) |
|     |     |

### <a name="swipeview"></a>SwipeView

|     |     |
| --- | --- |
| `SwipeView`ist ein Container Steuerelement, das ein Element des Inhalts umschließt und Kontextmenü Elemente bereitstellt, die durch eine Schwenkbewegung angezeigt werden. Jedes Menü Element wird durch ein dargestellt `SwipeItem` , das über eine-Eigenschaft verfügt, die `Command` eine ausführt, `ICommand` Wenn das Element getippt wird.<br /><br /> [Leitfaden](~/xamarin-forms/user-interface/swipeview.md)  /  [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery) | [![Swipeer View-Beispiel](views-images/SwipeView.png "Swipeer View-Beispiel")](views-images/SwipeView-Large.png#lightbox "Swipeer View-Beispiel")<br /> [C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SwipeViewDemoPage.cs)  /  [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SwipeViewDemoPage.xaml) mit [Code Behind](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SwipeViewDemoPage.xaml.cs) |
|     |     |

## <a name="views-for-setting-values"></a>Ansichten zum Festlegen von Werten

### <a name="checkbox"></a>CheckBox

|     |     |
| --- | --- |
| `CheckBox`ermöglicht dem Benutzer die Auswahl eines booleschen Werts mithilfe einer Schaltfläche, die entweder aktiviert oder leer ist. Die `IsChecked` -Eigenschaft ist der Zustand von `CheckBox` , und das- `CheckedChanged` Ereignis wird ausgelöst, wenn sich der Status ändert.<br /><br />API-Dokumentation/ [Führungs Faden](~/xamarin-forms/user-interface/checkbox.md)  /  [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-checkboxdemos) | [![CheckBox-Beispiel](views-images/CheckBox.png "CheckBox-Beispiel")](views-images/CheckBox-Large.png#lightbox "CheckBox-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/CheckBoxPage.cs)  /  [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/CheckBoxPage.xaml) |
|     |     |

### <a name="slider"></a>Slider

|     |     |
| --- | --- |
| [`Slider`](xref:Xamarin.Forms.Slider)ermöglicht dem Benutzer die Auswahl eines `double` Werts aus einem kontinuierlichen Bereich, der mit der-Eigenschaft und der-Eigenschaft angegeben wird [`Minimum`](xref:Xamarin.Forms.Slider.Minimum) [`Maximum`](xref:Xamarin.Forms.Slider.Maximum) .<br /><br />[API-Dokumentation](xref:Xamarin.Forms.Slider)  /  [Leitfaden](~/xamarin-forms/user-interface/slider.md)  /  [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-sliderdemos) | [![Beispiel für Schieberegler](views-images/Slider.png "Beispiel für Schieberegler")](views-images/Slider-Large.png#lightbox "Beispiel für Schieberegler")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SliderDemoPage.cs)  /  [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SliderDemoPage.xaml) |
|     |     |

### <a name="stepper"></a>Stepper

|     |     |
| --- | --- |
| [`Stepper`](xref:Xamarin.Forms.Stepper)ermöglicht dem Benutzer die Auswahl eines `double` Werts aus einem Bereich von inkrementellen Werten, die mit den [`Minimum`](xref:Xamarin.Forms.Stepper.Minimum) Eigenschaften, und angegeben werden [`Maximum`](xref:Xamarin.Forms.Stepper.Maximum) [`Increment`](xref:Xamarin.Forms.Stepper.Increment) .<br /><br />[API-Dokumentation](xref:Xamarin.Forms.Stepper)   /  [Leitfaden](~/xamarin-forms/user-interface/stepper.md)  /  [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-stepperdemos) | [![Beispiel für Stepper](views-images/Stepper.png "Beispiel für Stepper")](views-images/Stepper-Large.png#lightbox "Beispiel für Stepper")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/StepperDemoPage.cs)  /  [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/StepperDemoPage.xaml) |
|     |     |

### <a name="switch"></a>Schalter

|     |     |
| --- | --- |
| [`Switch`](xref:Xamarin.Forms.Switch)hat die Form eines ein-/ausschalten-Schalters, um dem Benutzer die Auswahl eines booleschen Werts zu ermöglichen. Die [`IsToggled`](xref:Xamarin.Forms.Switch.IsToggled) -Eigenschaft stellt den Zustand des Schalters dar, und das- [`Toggled`](xref:Xamarin.Forms.Switch.Toggled) Ereignis wird ausgelöst, wenn sich der Status ändert.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.Switch)  /  [Leitfaden](~/xamarin-forms/user-interface/switch.md)  /  [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-switchdemos/) | [![Switch-Beispiel](views-images/Switch.png "Switch-Beispiel")](views-images/Switch-Large.png#lightbox "Switch-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SwitchDemoPage.cs)  /  [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SwitchDemoPage.xaml) |
|     |     |

### <a name="datepicker"></a>DatePicker

|     |     |
| --- | --- |
| [`DatePicker`](xref:Xamarin.Forms.DatePicker)ermöglicht es dem Benutzer, ein Datum mit der Platt Form Datumsauswahl auszuwählen. Legen Sie einen Bereich zulässiger Datumsangaben mit den [`MinimumDate`](xref:Xamarin.Forms.DatePicker.MinimumDate) Eigenschaften und fest [`MaximumDate`](xref:Xamarin.Forms.DatePicker.MaximumDate) . Die [`Date`](xref:Xamarin.Forms.DatePicker.Date) -Eigenschaft ist das ausgewählte Datum, und das- [`DateSelected`](xref:Xamarin.Forms.DatePicker.DateSelected) Ereignis wird ausgelöst, wenn sich die Eigenschaft ändert.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.DatePicker)  /  [Leitfaden](~/xamarin-forms/user-interface/datepicker.md)  /  [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-datepicker) | [![DatePicker-Beispiel](views-images/DatePicker.png "DatePicker-Beispiel")](views-images/DatePicker-Large.png#lightbox "DatePicker-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/DatePickerDemoPage.cs)  /  [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/DatePickerDemoPage.xaml) |
|     |     |

### <a name="timepicker"></a>TimePicker

|     |     |
| --- | --- |
| [`TimePicker`](xref:Xamarin.Forms.TimePicker)ermöglicht es dem Benutzer, eine Uhrzeit mit der Platt Form Zeitauswahl auszuwählen. Die- [`Time`](xref:Xamarin.Forms.TimePicker.Time) Eigenschaft ist die ausgewählte Uhrzeit. Eine Anwendung kann Änderungen in der-Eigenschaft überwachen, `Time` indem Sie einen Handler für das- [`PropertyChanged`](xref:Xamarin.Forms.BindableObject.PropertyChanged) Ereignis installiert.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.TimePicker)  /  [Leitfaden](~/xamarin-forms/user-interface/timepicker.md)  /  [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-timepicker) | [![TimePicker-Beispiel](views-images/TimePicker.png "TimePicker-Beispiel")](views-images/TimePicker-Large.png#lightbox "TimePicker-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TimePickerDemoPage.cs)  /  [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TimePickerDemoPage.xaml) |
|     |     |

## <a name="views-for-editing-text"></a>Ansichten zum Bearbeiten von Text

Diese beiden Klassen werden von der- [`InputView`](xref:Xamarin.Forms.InputView) Klasse abgeleitet, die die- [`Keyboard`](xref:Xamarin.Forms.InputView.Keyboard) Eigenschaft definiert.

### <a name="entry"></a>Eingabe

|     |     |
| --- | --- |
| [`Entry`](xref:Xamarin.Forms.Entry)ermöglicht es dem Benutzer, eine einzelne Textzeile einzugeben und zu bearbeiten. Der Text ist als [`Text`](xref:Xamarin.Forms.InputView.Text) -Eigenschaft verfügbar, und das [`TextChanged`](xref:Xamarin.Forms.InputView.TextChanged) -Ereignis und das- [`Completed`](xref:Xamarin.Forms.Entry.Completed) Ereignis werden ausgelöst, wenn der Text geändert wird, oder der Benutzer signalisiert den Abschluss durch Tippen auf die EINGABETASTE.<br /><br />Verwenden Sie, [`Editor`](#editor) um mehrere Textzeilen einzugeben und zu bearbeiten.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.Entry)  /  [Leitfaden](~/xamarin-forms/user-interface/text/entry.md)  /  [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text) | [![Beispiel für den Eintrag](views-images/Entry.png "Beispiel für den Eintrag")](views-images/Entry-Large.png#lightbox "Beispiel für den Eintrag")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/EntryDemoPage.cs)  /  [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/EntryDemoPage.xaml) |
|     |     |

### <a name="editor"></a>Editor

|     |     |
| --- | --- |
| [`Editor`](xref:Xamarin.Forms.Editor)ermöglicht es dem Benutzer, mehrere Textzeilen einzugeben und zu bearbeiten. Der Text ist als [`Text`](xref:Xamarin.Forms.InputView.Text) -Eigenschaft verfügbar, und das [`TextChanged`](xref:Xamarin.Forms.InputView.TextChanged) -Ereignis und das- [`Completed`](xref:Xamarin.Forms.Editor.Completed) Ereignis werden ausgelöst, wenn der Text geändert wird oder wenn der Benutzer den Abschluss signalisiert.<br /><br />Verwenden Sie eine [`Entry`](#entry) Ansicht, um eine einzelne Textzeile einzugeben und zu bearbeiten.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.Editor)  /  [Leitfaden](~/xamarin-forms/user-interface/text/editor.md)  /  [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text) | [![Beispiel für den Eintrag](views-images/Editor.png "Editor-Beispiel")](views-images/Editor-Large.png#lightbox "Editor-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/EditorDemoPage.cs)  /  [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/EditorDemoPage.xaml) |
|     |     |

## <a name="views-to-indicate-activity"></a>Ansichten zum Anzeigen einer Aktivität

### <a name="activityindicator"></a>ActivityIndicator

|     |     |
| --- | --- |
| [`ActivityIndicator`](xref:Xamarin.Forms.ActivityIndicator)verwendet eine Animation, um anzuzeigen, dass die Anwendung mit einer langwierigen Aktivität beschäftigt ist, ohne dass der Fortschritt angegeben wird. Die- [`IsRunning`](xref:Xamarin.Forms.ActivityIndicator.IsRunning) Eigenschaft steuert die Animation.<br /><br />Wenn der Fortschritt der Aktivität bekannt ist, verwenden Sie [`ProgressBar`](#progressbar) stattdessen eine.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.ActivityIndicator)  /  [Leitfaden](~/xamarin-forms/user-interface/activityindicator.md)  /  [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-activityindicatordemos/) | [![Beispiel für activityindicator](views-images/ActivityIndicator.png "Beispiel für activityindicator")](views-images/ActivityIndicator-Large.png#lightbox "Beispiel für activityindicator")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ActivityIndicatorDemoPage.cs)  /  [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ActivityIndicatorDemoPage.xaml) |
|     |     |

### <a name="progressbar"></a>ProgressBar

|     |     |
| --- | --- |
| [`ProgressBar`](xref:Xamarin.Forms.ProgressBar)verwendet eine Animation, um anzuzeigen, dass die Anwendung durch eine lange Aktivität verläuft. Legen Sie die- [`Progress`](xref:Xamarin.Forms.ProgressBar.Progress) Eigenschaft auf Werte zwischen 0 und 1 fest, um den Fortschritt anzuzeigen.<br /><br />Wenn der Fortschritt der Aktivität nicht bekannt ist, verwenden Sie [`ActivityIndicator`](#activityindicator) stattdessen eine.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.ProgressBar)  /  [Leitfaden](~/xamarin-forms/user-interface/progressbar.md)  /  [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-progressbardemos/) | [![ProgressBar-Beispiel](views-images/ProgressBar.png "ProgressBar-Beispiel")](views-images/ProgressBar-Large.png#lightbox "ProgressBar-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ProgressBarDemoPage.cs)  /  [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ProgressBarDemoPage.xaml) mit [Code Behind](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ProgressBarDemoPage.xaml.cs) |
|     |     |

## <a name="views-that-display-collections"></a>Ansichten, die Sammlungen anzeigen

### <a name="carouselview"></a>CarouselView

|     |     |
| --- | --- |
| [`CarouselView`](xref:Xamarin.Forms.CarouselView)zeigt eine scrollbare Liste von Datenelementen an. Legen `ItemsSource` Sie die-Eigenschaft auf eine Auflistung von-Objekten fest, und legen Sie die- `ItemTemplate` Eigenschaft auf ein-Objekt fest, in dem die [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) Formatierung der Elemente beschrieben wird. Das- `CurrentItemChanged` Ereignis signalisiert, dass sich das aktuell angezeigte Element geändert hat, das als-Eigenschaft verfügbar ist `CurrentItem` .<br /><br />[Leitfaden](~/xamarin-forms/user-interface/carouselview/index.md)  /  [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-carouselviewdemos/) | [![Carouselview-Beispiel](views-images/CarouselView.png "Carouselview-Beispiel")](views-images/CarouselView-Large.png#lightbox "Carouselview-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/CarouselViewDemoPage.cs)  /  [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/CarouselViewDemoPage.xaml) |
|     |     |

### <a name="collectionview"></a>CollectionView

|     |     |
| --- | --- |
| [`CollectionView`](xref:Xamarin.Forms.CollectionView)zeigt eine Bild lauffähigen Liste auswählbarer Datenelemente mit unterschiedlichen layoutspezifikationen an. Es zielt darauf ab, eine flexiblere und leistungsfähigere Alternative zu bereitzustellen [`ListView`](xref:Xamarin.Forms.ListView) . Legen `ItemsSource` Sie die-Eigenschaft auf eine Auflistung von-Objekten fest, und legen Sie die- `ItemTemplate` Eigenschaft auf ein-Objekt fest, in dem die [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) Formatierung der Elemente beschrieben wird. Das- `SelectionChanged` Ereignis signalisiert, dass eine Auswahl getroffen wurde, die als-Eigenschaft verfügbar ist `SelectedItem` .<br /><br />[Leitfaden](~/xamarin-forms/user-interface/collectionview/index.md)  /  [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/) | [![CollectionView-Beispiel](views-images/CollectionView.png "CollectionView-Beispiel")](views-images/CollectionView-Large.png#lightbox "CollectionView-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/CollectionViewDemoPage.cs)  /  [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/CollectionViewDemoPage.xaml) |
|     |     |

### <a name="indicatorview"></a>IndicatorView

|     |     |
| --- | --- |
| `IndicatorView`zeigt Indikatoren an, die die Anzahl der Elemente in einer darstellen `CarouselView` . Legen Sie die- `CarouselView.IndicatorView` Eigenschaft auf das- `IndicatorView` Objekt fest, um Indikatoren für den anzuzeigen `CarouselView` . <br /><br />[Leitfaden](~/xamarin-forms/user-interface/indicatorview.md)  /  [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-indicatorviewdemos/) | [![Beispiel für die Anzeige Ansicht](views-images/IndicatorView.png "Beispiel für die Anzeige Ansicht")](views-images/IndicatorView-Large.png#lightbox "Beispiel für die Anzeige Ansicht")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/IndicatorViewDemoPage.cs)  /  [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/IndicatorViewDemoPage.xaml) |
|     |     |

### <a name="listview"></a>ListView

|     |     |
| --- | --- |
| [`ListView`](xref:Xamarin.Forms.ListView)wird von abgeleitet [`ItemsView`](xref:Xamarin.Forms.ItemsView`1) und zeigt eine scrollbare Liste auswählbarer Datenelemente an. Legen [`ItemsSource`](xref:Xamarin.Forms.ItemsView`1.ItemsSource) Sie die-Eigenschaft auf eine Auflistung von-Objekten fest, und legen Sie die- [`ItemTemplate`](xref:Xamarin.Forms.ItemsView`1.ItemTemplate) Eigenschaft auf ein-Objekt fest, in dem die [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) Formatierung der Elemente beschrieben wird. Das- [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) Ereignis signalisiert, dass eine Auswahl getroffen wurde, die als-Eigenschaft verfügbar ist [`SelectedItem`](xref:Xamarin.Forms.ListView.SelectedItem) .<br /><br />[API-Dokumentation](xref:Xamarin.Forms.ListView)  /  [Leitfaden](~/xamarin-forms/user-interface/listview/index.md)  /  [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithlistview/) | [![ListView-Beispiel](views-images/ListView.png "ListView-Beispiel")](views-images/ListView-Large.png#lightbox "ListView-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ListViewDemoPage.cs)  /  [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ListViewDemoPage.xaml) |
|     |     |

### <a name="picker"></a>Auswahl

|     |     |
| --- | --- |
| [`Picker`](xref:Xamarin.Forms.Picker)zeigt ein ausgewähltes Element aus einer Liste von Text Zeichenfolgen an und ermöglicht das Auswählen dieses Elements, wenn die Ansicht getippt wird. Legen Sie die- [`Items`](xref:Xamarin.Forms.Picker.Items) Eigenschaft auf eine Liste von Zeichen folgen oder die- [`ItemsSource`](xref:Xamarin.Forms.Picker.ItemsSource) Eigenschaft auf eine Auflistung von-Objekten fest. Das- [`SelectedIndexChanged`](xref:Xamarin.Forms.Picker.SelectedIndexChanged) Ereignis wird ausgelöst, wenn ein Element ausgewählt wird.<br /><br />Die `Picker` zeigt die Liste der Elemente nur an, wenn Sie ausgewählt ist. Verwenden Sie für eine Bild lauffähigen [`ListView`](#listview) [`TableView`](#tableview) Liste, die auf der Seite verbleibt<br /><br />[API-Dokumentation](xref:Xamarin.Forms.Picker)  /  [Leitfaden](~/xamarin-forms/user-interface/picker/index.md)  /  [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-pickerdemo) | [![Auswahl Beispiel](views-images/Picker.png "Auswahl Beispiel")](views-images/Picker-Large.png#lightbox "Auswahl Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/PickerDemoPage.cs)  /  [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/PickerDemoPage.xaml) mit [Code Behind](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/PickerDemoPage.xaml.cs) |
|     |     |

### <a name="tableview"></a>TableView

|     |     |
| --- | --- |
| [`TableView`](xref:Xamarin.Forms.TableView)zeigt eine Liste von Zeilen vom Typ [`Cell`](xref:Xamarin.Forms.Cell) mit optionalen Headern und unter Headern an. Legen Sie die [`Root`](xref:Xamarin.Forms.TableView.Root) -Eigenschaft auf ein Objekt vom Typ fest [`TableRoot`](xref:Xamarin.Forms.TableRoot) , und fügen Sie [`TableSection`](xref:Xamarin.Forms.TableSection) diesem Objekte hinzu `TableRoot` . Jede `TableSection` ist eine Auflistung von- `Cell` Objekten.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.TableView)  /  [Leitfaden](~/xamarin-forms/user-interface/tableview.md)  /  [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-tableview) | [![TableView-Beispiel](views-images/TableView.png "TableView-Beispiel")](views-images/TableView-Large.png#lightbox "TableView-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TableViewFormDemoPage.cs)  /  [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TableViewFormDemoPage.xaml) |
|     |     |

## <a name="related-links"></a>Verwandte Links

- [Xamarin.FormsFormsgallery-Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)
- [Xamarin.Forms-Beispiele](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.Forms)
- [Xamarin.FormsAPI-Dokumentation](https://docs.microsoft.com/dotnet/api/xamarin.forms?view=xamarin-forms)
