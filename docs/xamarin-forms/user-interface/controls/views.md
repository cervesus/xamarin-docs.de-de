---
title: Xamarin.Forms-Ansichten
description: Xamarin.Forms-Ansichten sind die Bausteine von plattformübergreifenden mobilen Benutzeroberflächen. Dieser Artikel beschreibt die Ansichten, die in Xamarin.Forms enthalten sind.
ms.prod: xamarin
ms.assetid: AC070686-A423-4A98-8BB6-0B9F94C062CC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/13/2019
ms.openlocfilehash: 7d53623ef1fb1eeb917cbf4cd6d65d461e525982
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2020
ms.locfileid: "76724237"
---
# <a name="xamarinforms-views"></a>Xamarin.Forms-Ansichten

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery/)

_Xamarin.Forms-Ansichten sind die Bausteine von plattformübergreifenden mobilen Benutzeroberflächen._

Ansichten sind Benutzeroberflächenobjekte wie Bezeichnungen, Schaltflächen und Schieberegler an, die häufig als bekanntermaßen *Steuerelemente* oder *Widgets* in andere grafische programmierumgebungen. Die Ansichten von Xamarin.Forms alle abgeleitet unterstützt die [ `View` ](xref:Xamarin.Forms.View) Klasse. Sie können in verschiedene Kategorien unterteilt werden:

## <a name="views-for-presentation"></a>Ansichten für die Präsentation

### <a name="label"></a>Bezeichnung

|     |     |
| --- | --- |
| [`Label`](xref:Xamarin.Forms.Label) Zeigt Textzeichenfolgen Einzel- oder mehrzeilige Blöcke von Text, der entweder Konstanten- oder Variablenwerte Formatierung. Legen Sie die [ `Text` ](xref:Xamarin.Forms.Label.Text) Eigenschaft in eine Zeichenfolge für die Konstante Formatierung, oder legen die [ `FormattedText` ](xref:Xamarin.Forms.Label.FormattedText) Eigenschaft, um eine [ `FormattedString` ](xref:Xamarin.Forms.FormattedString) -Objekt für die Variable die Formatierung an.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.Label) / [Handbuch](~/xamarin-forms/user-interface/text/label.md) / [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text) | [![Beispiel für eine Bezeichnung](views-images/Label.png "Beispiel für eine Bezeichnung")](views-images/Label-Large.png#lightbox "Beispiel für eine Bezeichnung")<br /> [C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/LabelDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/LabelDemoPage.xaml) |
|     |     |

### <a name="image"></a>Bild

|     |     |
| --- | --- |
| [`Image`](xref:Xamarin.Forms.Image) wird eine Bitmap. Bitmaps heruntergeladen werden kann, über das Web, in das allgemeine oder Plattformprojekte als Ressourcen eingebettet oder mit einer .NET erstellt `Stream` Objekt.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.Image) / [Handbuch](~/xamarin-forms/user-interface/images.md) / [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithimages) | [![Bildbeispiel](views-images/Image.png "Bildbeispiel")](views-images/Image-Large.png#lightbox "Bildbeispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ImageDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ImageDemoPage.xaml) |
|     |     |

### <a name="boxview"></a>BoxView

|     |    |
| --- | ---|
| [`BoxView`](xref:Xamarin.Forms.BoxView) Zeigt ein solid Rechteck durch farbige an die [ `Color` ](xref:Xamarin.Forms.BoxView.Color) Eigenschaft. `BoxView` verfügt über eine standardmäßige Größe Anforderung von 40 x 40. Informationen zu den anderen Größen weisen die [ `WidthRequest` ](xref:Xamarin.Forms.VisualElement.WidthRequest) und [ `HeightRequest` ](xref:Xamarin.Forms.VisualElement.HeightRequest) Eigenschaften.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.BoxView) / [Handbuch](~/xamarin-forms/user-interface/boxview.md) / [Beispiel 1](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-basicboxview), [2](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-textdecoration), [3](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-listviewcolors/), [4 ](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-gameoflife), [5](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-dotmatrixclock), und [6](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-boxviewclock) | [![Boxview-Beispiel](views-images/BoxView.png "Boxview-Beispiel")](views-images/BoxView-Large.png#lightbox "Boxview-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/BoxViewDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/BoxViewDemoPage.xaml) |
|     |     |

### <a name="webview"></a>WebView

|     |     |
| --- | --- |
| [`WebView`](xref:Xamarin.Forms.WebView) Zeigt an, Webseiten oder HTML-Inhalt, abhängig davon, ob die [ `Source` ](xref:Xamarin.Forms.WebView.Source) -Eigenschaftensatz auf eine [ `UriWebViewSource` ](xref:Xamarin.Forms.UrlWebViewSource) oder [ `HtmlWebViewSource` ](xref:Xamarin.Forms.HtmlWebViewSource) Objekt.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.WebView) / [Handbuch](~/xamarin-forms/user-interface/webview.md) / [Beispiel 1](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithwebview) und [2](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-webview) | [![WebView-Beispiel](views-images/WebView.png "WebView-Beispiel")](views-images/WebView-Large.png#lightbox "WebView-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/WebViewDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/WebViewDemoPage.xaml) |
|     |     |

### <a name="openglview"></a>OpenGLView

|     |     |
| --- | --- |
| [`OpenGLView`](xref:Xamarin.Forms.OpenGLView) Zeigt OpenGL-Grafiken in iOS und Android-Projekte. Es gibt keine Unterstützung für die universelle Windows-Plattform. Die IOS- und Android-Projekte erfordern einen Verweis auf die **OpenTK-1.0-** Assembly oder der **OpenTK** Assembly Version 1.0.0.0. `OpenGLView` ist einfacher, die in ein freigegebenes Projekt verwenden. Wenn in einer .NET Standard-Bibliothek verwendet wird, klicken Sie dann werden eine Abhängigkeitsdienst auch (wie im Beispielcode gezeigt) benötigt.<br /><br />Dies ist die einzige Grafik Funktion, die in xamarin. Forms integriert ist, aber eine xamarin. Forms-Anwendung kann auch Grafiken mithilfe von [`SkiaSharp`](~/xamarin-forms/user-interface/graphics/skiasharp/index.md)oder [`UrhoSharp`](~/xamarin-forms/user-interface/graphics/urhosharp.md)Renderen.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.OpenGLView)<br /><br /> | [![Openglview-Beispiel](views-images/OpenGLView.png "Openglview-Beispiel")](views-images/OpenGLView-Large.png#lightbox "Openglview-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/OpenGLViewDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/OpenGLViewDemoPage.xaml) mit [CodeBehind](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/OpenGLViewDemoPage.xaml.cs) |
|     |     |

### <a name="map"></a>Zuordnung

|     |     |
| --- | --- |
| [`Map`](xref:Xamarin.Forms.Maps.Map) eine Karte angezeigt. Das **xamarin. Forms. Maps** -nuget-Paket muss installiert sein. Android und die universelle Windows-Plattform erforderlich, einen Map-Autorisierungsschlüssel.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.Maps.Map) / [Handbuch](~/xamarin-forms/user-interface/map/index.md) / [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps/) | [![Map-Beispiel](views-images/Map.png "Map-Beispiel")](views-images/Map-Large.png#lightbox "Map-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/MapDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/MapDemoPage.xaml) |
|     |     |

## <a name="views-that-initiate-commands"></a>Ansichten, die Befehle initiieren

### <a name="button"></a>Schaltfläche

|     |     |
| --- | --- |
| [`Button`](xref:Xamarin.Forms.Button) ist ein rechteckiges-Objekt, das Text anzeigt und die löst eine [ `Clicked` ](xref:Xamarin.Forms.Button.Clicked) Ereignis aus, wenn es gedrückt wurde, wird.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.Button) / [Handbuch](~/xamarin-forms/user-interface/button.md) / [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-buttondemos/) | [![Schaltflächen Beispiel](views-images/Button.png "Schaltflächen Beispiel")](views-images/Button-Large.png#lightbox "Schaltflächen Beispiel")<br /> [C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ButtonDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ButtonDemoPage.xaml) mit [CodeBehind](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ButtonDemoPage.xaml.cs) |
|     |     |

### <a name="imagebutton"></a>ImageButton

|     |     |
| --- | --- |
| `ImageButton` ist ein rechteckiges Objekt ein Bild anzeigt und die löst eine `Clicked` Ereignis aus, wenn es gedrückt wurde, wird.<br /><br /> [Handbuch](~/xamarin-forms/user-interface/imagebutton.md) / [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery) | [![ImageButton-Beispiel](views-images/ImageButton.png "ImageButton-Beispiel")](views-images/ImageButton-Large.png#lightbox "ImageButton-Beispiel")<br /> [C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ImageButtonDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ImageButtonDemoPage.xaml) mit [CodeBehind](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ImageButtonDemoPage.xaml.cs) |
|     |     |

### <a name="refreshview"></a>RefreshView

|     |     |
| --- | --- |
| `RefreshView` ist ein Container Steuerelement, das Pull-to-Refresh-Funktionen für scrollbaren Inhalt bereitstellt. Die von der `Command`-Eigenschaft definierte `ICommand` wird ausgeführt, wenn eine Aktualisierung ausgelöst wird, und die `IsRefreshing`-Eigenschaft gibt den aktuellen Zustand des Steuer Elements an.<br /><br /> [Handbuch](~/xamarin-forms/user-interface/refreshview.md) / [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery) | [![Beispiel für eine aktuaktu](views-images/RefreshView.png "Beispiel für eine aktuaktu")](views-images/RefreshView-Large.png#lightbox "Beispiel für eine aktuaktu")<br /> [C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/RefreshViewDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/RefreshViewDemoPage.xaml) mit [CodeBehind](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/RefreshViewDemoPage.xaml.cs) |
|     |     |

### <a name="searchbar"></a>SearchBar

|     |     |
| --- | --- |
| [`SearchBar`](xref:Xamarin.Forms.SearchBar) Zeigt einen Bereich für den Benutzer geben eine Zeichenfolge, und eine Schaltfläche (oder eine Taste der Tastatur), der signalisiert die Anwendung gesucht werden soll. Die [ `Text` ](xref:Xamarin.Forms.SearchBar.Text) Eigenschaft bietet Zugriff auf den Text ein, und die [ `SearchButtonPressed` ](xref:Xamarin.Forms.SearchBar.SearchButtonPressed) Ereignis weist darauf hin, dass die Schaltfläche gedrückt wurde.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.SearchBar) / [Handbuch](~/xamarin-forms/user-interface/searchbar.md) / [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-searchbardemos/) | [![Beispiel für eine Searchbar](views-images/SearchBar.png "Beispiel für eine Searchbar")](views-images/SearchBar-Large.png#lightbox "Beispiel für eine Searchbar")<br /> [C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SearchBarDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SearchBarDemoPage.xaml) mit [CodeBehind](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SearchBarDemoPage.xaml.cs) |
|     |     |

### <a name="swipeview"></a>SwipeView

|     |     |
| --- | --- |
| `SwipeView` ist ein Container Steuerelement, das ein Inhalts Element umschließt und Kontextmenü Elemente bereitstellt, die durch eine Schwenkbewegung angezeigt werden. Jedes Menü Element wird durch einen `SwipeItem`dargestellt, der über eine `Command` Eigenschaft verfügt, die eine `ICommand` ausführt, wenn das Element getippt wird.<br /><br /> [Handbuch](~/xamarin-forms/user-interface/swipeview.md) / [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery) | [![Swipeer View-Beispiel](views-images/SwipeView.png "Swipeer View-Beispiel")](views-images/SwipeView-Large.png#lightbox "Swipeer View-Beispiel")<br /> [C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SwipeViewDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SwipeViewDemoPage.xaml) mit [CodeBehind](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SwipeViewDemoPage.xaml.cs) |
|     |     |

## <a name="views-for-setting-values"></a>Ansichten zum Festlegen von Werten

### <a name="checkbox"></a>CheckBox

|     |     |
| --- | --- |
| `CheckBox` ermöglicht dem Benutzer die Auswahl eines booleschen Werts mithilfe eines Schaltflächen Typs, der entweder aktiviert oder leer ist. Die `IsChecked`-Eigenschaft ist der Status des `CheckBox`, und das `CheckedChanged`-Ereignis wird ausgelöst, wenn sich der Status ändert.<br /><br />API-Dokumentation/ [Handbuch](~/xamarin-forms/user-interface/checkbox.md) / [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-checkboxdemos) | [![CheckBox-Beispiel](views-images/CheckBox.png "CheckBox-Beispiel")](views-images/CheckBox-Large.png#lightbox "CheckBox-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/CheckBoxPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/CheckBoxPage.xaml) |
|     |     |

### <a name="slider"></a>Slider

|     |     |
| --- | --- |
| [`Slider`](xref:Xamarin.Forms.Slider) ermöglicht dem Benutzer, wählen Sie eine `double` Wert aus einen durchgehenden Bereich angegeben, mit der [ `Minimum` ](xref:Xamarin.Forms.Slider.Minimum) und [ `Maximum` ](xref:Xamarin.Forms.Slider.Maximum) Eigenschaften.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.Slider) / [Handbuch](~/xamarin-forms/user-interface/slider.md) / [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-sliderdemos) | [![Beispiel für Schieberegler](views-images/Slider.png "Beispiel für Schieberegler")](views-images/Slider-Large.png#lightbox "Beispiel für Schieberegler")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SliderDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SliderDemoPage.xaml) |
|     |     |

### <a name="stepper"></a>Stepper

|     |     |
| --- | --- |
| [`Stepper`](xref:Xamarin.Forms.Stepper) ermöglicht dem Benutzer, wählen Sie eine `double` Wert aus einem inkrementellen Werte, die mit dem angegebenen Bereich der [ `Minimum` ](xref:Xamarin.Forms.Stepper.Minimum), [ `Maximum` ](xref:Xamarin.Forms.Stepper.Maximum), und [ `Increment` ](xref:Xamarin.Forms.Stepper.Increment) Eigenschaften.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.Stepper)  / [Handbuch](~/xamarin-forms/user-interface/stepper.md) / [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-stepperdemos) | [![Beispiel für Stepper](views-images/Stepper.png "Beispiel für Stepper")](views-images/Stepper-Large.png#lightbox "Beispiel für Stepper")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/StepperDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/StepperDemoPage.xaml) |
|     |     |

### <a name="switch"></a>Schalter

|     |     |
| --- | --- |
| [`Switch`](xref:Xamarin.Forms.Switch) nimmt die Form einer Ein-/Ausschalter, die der Benutzer einen booleschen Wert auswählen kann. Die [ `IsToggled` ](xref:Xamarin.Forms.Switch.IsToggled) Eigenschaft ist der Status des Switches, und die [ `Toggled` ](xref:Xamarin.Forms.Switch.Toggled) Ereignis wird ausgelöst, wenn sich der Status ändert.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.Switch) / [Handbuch](~/xamarin-forms/user-interface/switch.md) / [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-switchdemos/) | [![Switch-Beispiel](views-images/Switch.png "Switch-Beispiel")](views-images/Switch-Large.png#lightbox "Switch-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SwitchDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SwitchDemoPage.xaml) |
|     |     |

### <a name="datepicker"></a>DatePicker

|     |     |
| --- | --- |
| [`DatePicker`](xref:Xamarin.Forms.DatePicker) ermöglicht dem Benutzer mit der Plattform Datumsauswahl ein Datum auswählen. Legen Sie einen Datumsbereich zulässige mit der [ `MinimumDate` ](xref:Xamarin.Forms.DatePicker.MinimumDate) und [ `MaximumDate` ](xref:Xamarin.Forms.DatePicker.MaximumDate) Eigenschaften. Die [ `Date` ](xref:Xamarin.Forms.DatePicker.Date) -Eigenschaft ist dem ausgewählten Datum ein, und die [ `DateSelected` ](xref:Xamarin.Forms.DatePicker.DateSelected) Ereignis wird ausgelöst, wenn diese Eigenschaft geändert wird.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.DatePicker) / [Handbuch](~/xamarin-forms/user-interface/datepicker.md) / [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-datepicker) | [![DatePicker-Beispiel](views-images/DatePicker.png "DatePicker-Beispiel")](views-images/DatePicker-Large.png#lightbox "DatePicker-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/DatePickerDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/DatePickerDemoPage.xaml) |
|     |     |

### <a name="timepicker"></a>TimePicker

|     |     |
| --- | --- |
| [`TimePicker`](xref:Xamarin.Forms.TimePicker) ermöglicht dem Benutzer, eine Zeit mit der Plattform / Zeitauswahl auszuwählen. Die [ `Time` ](xref:Xamarin.Forms.TimePicker.Time) -Eigenschaft ist der ausgewählten Zeit. Überwachung einer Anwendung kann Änderungen an der `Time` Eigenschaft, indem Sie installieren einen Handler für die [ `PropertyChanged` ](xref:Xamarin.Forms.BindableObject.PropertyChanged) Ereignis.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.TimePicker) / [Handbuch](~/xamarin-forms/user-interface/timepicker.md) / [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-timepicker) | [![TimePicker-Beispiel](views-images/TimePicker.png "TimePicker-Beispiel")](views-images/TimePicker-Large.png#lightbox "TimePicker-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TimePickerDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TimePickerDemoPage.xaml) |
|     |     |

## <a name="views-for-editing-text"></a>Ansichten zum Bearbeiten von Text

Diese beiden Klassen werden von der [ `InputView` ](xref:Xamarin.Forms.InputView) -Klasse, die definiert, die [ `Keyboard` ](xref:Xamarin.Forms.InputView.Keyboard) Eigenschaft.

### <a name="entry"></a>Eingabe

|     |     |
| --- | --- |
| [`Entry`](xref:Xamarin.Forms.Entry) ermöglicht dem Benutzer zum eingeben und bearbeiten eine einzelne Textzeile. Der Text ist verfügbar, als die [ `Text` ](xref:Xamarin.Forms.Entry.Text) -Eigenschaft, und die [ `TextChanged` ](xref:Xamarin.Forms.Entry.TextChanged) und [ `Completed` ](xref:Xamarin.Forms.Entry.Completed) Ereignisse werden ausgelöst, wenn der Text ändert oder der Benutzer signalisiert Abschluss durch Tippen auf die EINGABETASTE.<br /><br />Verwenden einer [ `Editor` ](#editor) zum eingeben und bearbeiten mehrere Textzeilen.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.Entry) / [Handbuch](~/xamarin-forms/user-interface/text/entry.md) / [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text) | [![Beispiel für den Eintrag](views-images/Entry.png "Beispiel für den Eintrag")](views-images/Entry-Large.png#lightbox "Beispiel für den Eintrag")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/EntryDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/EntryDemoPage.xaml) |
|     |     |

### <a name="editor"></a>-Editor

|     |     |
| --- | --- |
| [`Editor`](xref:Xamarin.Forms.Editor) ermöglicht dem Benutzer zum eingeben und bearbeiten mehrere Textzeilen. Der Text ist verfügbar, als die [ `Text` ](xref:Xamarin.Forms.Editor.Text) -Eigenschaft, und die [ `TextChanged` ](xref:Xamarin.Forms.Editor.TextChanged) und [ `Completed` ](xref:Xamarin.Forms.Editor.Completed) Ereignisse werden ausgelöst, wenn der Text ändert oder der Benutzer signalisiert Abschluss.<br /><br />Verwenden einer [ `Entry` ](#entry) Ansicht für die Eingabe und Bearbeitung von einer einzelnen Textzeile an.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.Editor) / [Handbuch](~/xamarin-forms/user-interface/text/editor.md) / [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text) | [![Beispiel für den Eintrag](views-images/Editor.png "Editor-Beispiel")](views-images/Editor-Large.png#lightbox "Editor-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/EditorDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/EditorDemoPage.xaml) |
|     |     |

## <a name="views-to-indicate-activity"></a>Ansichten zum Anzeigen einer Aktivität

### <a name="activityindicator"></a>ActivityIndicator

|     |     |
| --- | --- |
| [`ActivityIndicator`](xref:Xamarin.Forms.ActivityIndicator) mit einer Animation an, dass die Anwendung in einer langen Aktivität beteiligt ist, ohne dass darauf ausgeführt wird. Die [ `IsRunning` ](xref:Xamarin.Forms.ActivityIndicator.IsRunning) -Eigenschaft steuert die Animation.<br /><br />Wenn die Aktivität den Status bekannt ist, verwenden Sie eine [ `ProgressBar` ](#progressbar) stattdessen.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.ActivityIndicator) / [Handbuch](~/xamarin-forms/user-interface/activityindicator.md) / [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-activityindicatordemos/) | [![Beispiel für activityindicator](views-images/ActivityIndicator.png "Beispiel für activityindicator")](views-images/ActivityIndicator-Large.png#lightbox "Beispiel für activityindicator")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ActivityIndicatorDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ActivityIndicatorDemoPage.xaml) |
|     |     |

### <a name="progressbar"></a>Statusanzeige

|     |     |
| --- | --- |
| [`ProgressBar`](xref:Xamarin.Forms.ProgressBar) verwendet eine Animation an, dass die Anwendung über eine lange Aktivität fortgesetzt wird. Legen Sie die [ `Progress` ](xref:Xamarin.Forms.ProgressBar.Progress) Eigenschaft, um Werte zwischen 0 und 1 fest, um den Status angeben.<br /><br />Wenn der Aktivität Status unbekannt ist, verwenden Sie eine [ `ActivityIndicator` ](#activityindicator) stattdessen.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.ProgressBar) / [Handbuch](~/xamarin-forms/user-interface/progressbar.md) / [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-progressbardemos/) | [![ProgressBar-Beispiel](views-images/ProgressBar.png "ProgressBar-Beispiel")](views-images/ProgressBar-Large.png#lightbox "ProgressBar-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ProgressBarDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ProgressBarDemoPage.xaml) mit [CodeBehind](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ProgressBarDemoPage.xaml.cs) |
|     |     |

## <a name="views-that-display-collections"></a>Ansichten, die Sammlungen anzeigen

### <a name="carouselview"></a>CarouselView

|     |     |
| --- | --- |
| [`CarouselView`](xref:Xamarin.Forms.CarouselView) zeigt eine scrollbare Liste von Datenelementen an. Legen Sie die `ItemsSource`-Eigenschaft auf eine Auflistung von-Objekten fest, und legen Sie die `ItemTemplate`-Eigenschaft auf ein [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) Objekt fest, das beschreibt, wie die Elemente formatiert werden sollen. Das `CurrentItemChanged`-Ereignis signalisiert, dass sich das aktuell angezeigte Element geändert hat, das als `CurrentItem`-Eigenschaft verfügbar ist.<br /><br />[Handbuch](~/xamarin-forms/user-interface/carouselview/index.md) / [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-carouselviewdemos/) | [![Carouselview-Beispiel](views-images/CarouselView.png "Carouselview-Beispiel")](views-images/CarouselView-Large.png#lightbox "Carouselview-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/CarouselViewDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/CarouselViewDemoPage.xaml) |
|     |     |

### <a name="collectionview"></a>CollectionView

|     |     |
| --- | --- |
| [`CollectionView`](xref:Xamarin.Forms.CollectionView) zeigt eine Bild lauffähigen Liste auswählbarer Datenelemente mit unterschiedlichen layoutspezifikationen an. Es zielt darauf ab, eine flexiblere und leistungsfähigere Alternative zu [`ListView`](xref:Xamarin.Forms.ListView)bereitzustellen. Legen Sie die `ItemsSource`-Eigenschaft auf eine Auflistung von-Objekten fest, und legen Sie die `ItemTemplate`-Eigenschaft auf ein [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) Objekt fest, das beschreibt, wie die Elemente formatiert werden sollen. Das `SelectionChanged`-Ereignis signalisiert, dass eine Auswahl getroffen wurde, die als `SelectedItem`-Eigenschaft verfügbar ist.<br /><br />[Handbuch](~/xamarin-forms/user-interface/collectionview/index.md) / [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/) | [![CollectionView-Beispiel](views-images/CollectionView.png "CollectionView-Beispiel")](views-images/CollectionView-Large.png#lightbox "CollectionView-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/CollectionViewDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/CollectionViewDemoPage.xaml) |
|     |     |

### <a name="indicatorview"></a>IndicatorView

|     |     |
| --- | --- |
| `IndicatorView` zeigt Indikatoren an, die die Anzahl der Elemente in einer `CarouselView`darstellen. Legen Sie die `ItemsSourceBy`-Eigenschaft auf das `CarouselView` Objekt fest, für das Indikatoren angezeigt werden sollen. <br /><br />[Handbuch](~/xamarin-forms/user-interface/indicatorview.md) / [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-indicatorviewdemos/) | [![Beispiel für die Anzeige Ansicht](views-images/IndicatorView.png "Beispiel für die Anzeige Ansicht")](views-images/IndicatorView-Large.png#lightbox "Beispiel für die Anzeige Ansicht")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/IndicatorViewDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/IndicatorViewDemoPage.xaml) |
|     |     |

### <a name="listview"></a>ListView

|     |     |
| --- | --- |
| [`ListView`](xref:Xamarin.Forms.ListView) leitet sich von [ `ItemsView` ](xref:Xamarin.Forms.ItemsView`1) und eine bildlauffähige Liste der auswählbaren Datenelemente angezeigt. Legen Sie die [ `ItemsSource` ](xref:Xamarin.Forms.ItemsView`1.ItemsSource) Eigenschaft, um eine Auflistung von Objekten, und legen Sie die [ `ItemTemplate` ](xref:Xamarin.Forms.ItemsView`1.ItemTemplate) Eigenschaft, um eine [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) Objekt beschreibt, wie die Elemente werden formatiert werden. Die [ `ItemSelected` ](xref:Xamarin.Forms.ListView.ItemSelected) Ereignis signalisiert, dass eine Auswahl getroffen wurde, die als verfügbar ist, die [ `SelectedItem` ](xref:Xamarin.Forms.ListView.SelectedItem) Eigenschaft.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.ListView) / [Handbuch](~/xamarin-forms/user-interface/listview/index.md) / [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithlistview/) | [![ListView-Beispiel](views-images/ListView.png "ListView-Beispiel")](views-images/ListView-Large.png#lightbox "ListView-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ListViewDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ListViewDemoPage.xaml) |
|     |     |

### <a name="picker"></a>Auswahl

|     |     |
| --- | --- |
| [`Picker`](xref:Xamarin.Forms.Picker) Zeigt ein ausgewähltes Element aus einer Liste von Textzeichenfolgen und ermöglicht die Auswahl dieses Elements, wenn die Ansicht angetippt wird. Legen Sie die [ `Items` ](xref:Xamarin.Forms.Picker.Items) Eigenschaft, um eine Liste von Zeichenfolgen, oder die [ `ItemsSource` ](xref:Xamarin.Forms.Picker.ItemsSource) Eigenschaft, um eine Auflistung von Objekten. Die [ `SelectedIndexChanged` ](xref:Xamarin.Forms.Picker.SelectedIndexChanged) Ereignis wird ausgelöst, wenn ein Element ausgewählt ist.<br /><br />Die `Picker` zeigt die Liste der Elemente nur, wenn diese Option ausgewählt ist. Verwenden einer [ `ListView` ](#listview) oder [ `TableView` ](#tableview) eine bildlauffähige Liste, die auf der Seite verbleibt.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.Picker) / [Handbuch](~/xamarin-forms/user-interface/picker/index.md) / [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-pickerdemo) | [![Auswahl Beispiel](views-images/Picker.png "Auswahl Beispiel")](views-images/Picker-Large.png#lightbox "Auswahl Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/PickerDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/PickerDemoPage.xaml) mit [CodeBehind](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/PickerDemoPage.xaml.cs) |
|     |     |

### <a name="tableview"></a>TableView

|     |     |
| --- | --- |
| [`TableView`](xref:Xamarin.Forms.TableView) Zeigt eine Liste von Zeilen vom Typ [ `Cell` ](xref:Xamarin.Forms.Cell) mit optionalen Header und Subheader hinzuzufügen. Legen Sie die [ `Root` ](xref:Xamarin.Forms.TableView.Root) Eigenschaft, um ein Objekt vom Typ [ `TableRoot` ](xref:Xamarin.Forms.TableRoot), und fügen [ `TableSection` ](xref:Xamarin.Forms.TableSection) -Objekte mit `TableRoot`. Jede `TableSection` ist eine Sammlung von `Cell` Objekte.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.TableView) / [Handbuch](~/xamarin-forms/user-interface/tableview.md) / [Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-tableview) | [![TableView-Beispiel](views-images/TableView.png "TableView-Beispiel")](views-images/TableView-Large.png#lightbox "TableView-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TableViewFormDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TableViewFormDemoPage.xaml) |
|     |     |

## <a name="related-links"></a>Verwandte Themen

- [Beispiel für Xamarin.Forms FormsGallery](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)
- [Xamarin.Forms-Beispiele](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.Forms)
- [Xamarin.Forms-API-Dokumentation](https://docs.microsoft.com/dotnet/api/xamarin.forms?view=xamarin-forms)
