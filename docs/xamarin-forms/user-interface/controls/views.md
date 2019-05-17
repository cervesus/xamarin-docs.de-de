---
title: Xamarin.Forms-Ansichten
description: Xamarin.Forms-Ansichten sind die Bausteine von plattformübergreifenden mobilen Benutzeroberflächen. Dieser Artikel beschreibt die Ansichten, die in Xamarin.Forms enthalten sind.
ms.prod: xamarin
ms.assetid: AC070686-A423-4A98-8BB6-0B9F94C062CC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/21/2019
ms.openlocfilehash: 5b2e58901d4a850863f68b26ce41e1aa4e8daee4
ms.sourcegitcommit: a9c60f50b40203dd784e3e790b0d83e2bfc86129
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/16/2019
ms.locfileid: "61358949"
---
# <a name="xamarinforms-views"></a>Xamarin.Forms-Ansichten

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://developer.xamarin.com/samples/FormsGallery/)

_Xamarin.Forms-Ansichten sind die Bausteine von plattformübergreifenden mobilen Benutzeroberflächen._

Ansichten sind Benutzeroberflächenobjekte wie Bezeichnungen, Schaltflächen und Schieberegler an, die häufig als bekanntermaßen *Steuerelemente* oder *Widgets* in andere grafische programmierumgebungen. Die Ansichten von Xamarin.Forms alle abgeleitet unterstützt die [ `View` ](xref:Xamarin.Forms.View) Klasse. Sie können in verschiedene Kategorien unterteilt werden:

## <a name="views-for-presentation"></a>Ansichten für die Präsentation

### <a name="label"></a>Bezeichnung

|     |     |
| --- | --- |
| [`Label`](xref:Xamarin.Forms.Label) Zeigt Textzeichenfolgen Einzel- oder mehrzeilige Blöcke von Text, der entweder Konstanten- oder Variablenwerte Formatierung. Legen Sie die [ `Text` ](xref:Xamarin.Forms.Label.Text) Eigenschaft in eine Zeichenfolge für die Konstante Formatierung, oder legen die [ `FormattedText` ](xref:Xamarin.Forms.Label.FormattedText) Eigenschaft, um eine [ `FormattedString` ](xref:Xamarin.Forms.FormattedString) -Objekt für die Variable die Formatierung an.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.Label) / [Handbuch](~/xamarin-forms/user-interface/text/label.md) / [Beispiel](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text/) | [![Beispiel für Bezeichnung](views-images/Label.png "Bezeichnung Beispiel")](views-images/Label-Large.png#lightbox "Bezeichnung Beispiel")<br /> [C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/LabelDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/LabelDemoPage.xaml) |
|     |     |

### <a name="image"></a>Bild

|     |     |
| --- | --- |
| [`Image`](xref:Xamarin.Forms.Image) wird eine Bitmap. Bitmaps heruntergeladen werden kann, über das Web, in das allgemeine oder Plattformprojekte als Ressourcen eingebettet oder mit einer .NET erstellt `Stream` Objekt.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.Image) / [Handbuch](~/xamarin-forms/user-interface/images.md) / [Beispiel](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithImages/) | [![Abbildung Beispiel](views-images/Image.png "Image Beispiel")](views-images/Image-Large.png#lightbox "Image-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ImageDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ImageDemoPage.xaml) |
|     |     |

### <a name="boxview"></a>BoxView

|     |    |
| --- | ---|
| [`BoxView`](xref:Xamarin.Forms.BoxView) Zeigt ein solid Rechteck durch farbige an die [ `Color` ](xref:Xamarin.Forms.BoxView.Color) Eigenschaft. `BoxView` verfügt über eine standardmäßige Größe Anforderung von 40 x 40. Informationen zu den anderen Größen weisen die [ `WidthRequest` ](xref:Xamarin.Forms.VisualElement.WidthRequest) und [ `HeightRequest` ](xref:Xamarin.Forms.VisualElement.HeightRequest) Eigenschaften.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.BoxView) / [Handbuch](~/xamarin-forms/user-interface/boxview.md) / [Beispiel 1](https://developer.xamarin.com/samples/xamarin-forms/BoxView/BasicBoxView), [2](https://developer.xamarin.com/samples/xamarin-forms/BoxView/TextDecoration), [3](https://developer.xamarin.com/samples/xamarin-forms/BoxView/ColorListBox), [4 ](https://developer.xamarin.com/samples/xamarin-forms/BoxView/GameOfLife), [5](https://developer.xamarin.com/samples/xamarin-forms/BoxView/DotMatrixClock), und [6](https://developer.xamarin.com/samples/xamarin-forms/BoxView/BoxViewClock) | [![Beispiel für BoxView](views-images/BoxView.png "BoxView Beispiel")](views-images/BoxView-Large.png#lightbox "BoxView-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/BoxViewDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/BoxViewDemoPage.xaml) |
|     |     |

### <a name="webview"></a>WebView

|     |     |
| --- | --- |
| [`WebView`](xref:Xamarin.Forms.WebView) Zeigt an, Webseiten oder HTML-Inhalt, abhängig davon, ob die [ `Source` ](xref:Xamarin.Forms.WebView.Source) -Eigenschaftensatz auf eine [ `UriWebViewSource` ](xref:Xamarin.Forms.UrlWebViewSource) oder [ `HtmlWebViewSource` ](xref:Xamarin.Forms.HtmlWebViewSource) Objekt.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.WebView) / [Handbuch](~/xamarin-forms/user-interface/webview.md) / [Beispiel 1](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithWebview/) und [2](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/WebView) | [![WebView-Beispiel](views-images/WebView.png "WebView Beispiel")](views-images/WebView-Large.png#lightbox "WebView-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/WebViewDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/WebViewDemoPage.xaml) |
|     |     |

### <a name="openglview"></a>OpenGLView

|     |     |
| --- | --- |
| [`OpenGLView`](xref:Xamarin.Forms.OpenGLView) Zeigt OpenGL-Grafiken in iOS und Android-Projekte. Es gibt keine Unterstützung für die universelle Windows-Plattform. Die IOS- und Android-Projekte erfordern einen Verweis auf die **OpenTK-1.0-** Assembly oder der **OpenTK** Assembly Version 1.0.0.0. `OpenGLView` ist einfacher, die in ein freigegebenes Projekt verwenden. Wenn in einer .NET Standard-Bibliothek verwendet wird, klicken Sie dann werden eine Abhängigkeitsdienst auch (wie im Beispielcode gezeigt) benötigt.<br /><br />Dies ist der einzige Graphics-Funktion, die in Xamarin.Forms integriert ist, aber eine Xamarin.Forms-Anwendung kann auch Rendern von Grafiken mit [ `CocosSharp` ](~/xamarin-forms/user-interface/graphics/cocossharp.md), [ `SkiaSharp` ](~/xamarin-forms/user-interface/graphics/skiasharp/index.md), oder [ `UrhoSharp` ](~/xamarin-forms/user-interface/graphics/urhosharp.md).<br /><br />[API-Dokumentation](xref:Xamarin.Forms.OpenGLView)<br /><br /> | [![Beispiel für OpenGLView](views-images/OpenGLView.png "OpenGLView Beispiel")](views-images/OpenGLView-Large.png#lightbox "OpenGLView-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/OpenGLViewDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/OpenGLViewDemoPage.xaml) mit [CodeBehind](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/OpenGLViewDemoPage.xaml.cs) |
|     |     |

### <a name="map"></a>Zuordnung

|     |     |
| --- | --- |
| [`Map`](xref:Xamarin.Forms.Maps.Map) eine Karte angezeigt. Die **Xamarin.Forms.Maps** Nuget-Paket muss installiert sein. Android und die universelle Windows-Plattform erforderlich, einen Map-Autorisierungsschlüssel.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.Maps.Map) / [Handbuch](~/xamarin-forms/user-interface/map.md) / [Beispiel](https://developer.xamarin.com/samples/WorkingWithMaps/) | [![Zuordnen von Beispiel](views-images/Map.png "zuordnen Beispiel")](views-images/Map-Large.png#lightbox "Beispiel zuordnen")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/MapDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/MapDemoPage.xaml) |
|     |     |

## <a name="views-that-initiate-commands"></a>Sichten, die Befehle zu initiieren.

### <a name="button"></a>Schaltfläche

|     |     |
| --- | --- |
| [`Button`](xref:Xamarin.Forms.Button) ist ein rechteckiges-Objekt, das Text anzeigt und die löst eine [ `Clicked` ](xref:Xamarin.Forms.Button.Clicked) Ereignis aus, wenn es gedrückt wurde, wird.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.Button) / [Handbuch](~/xamarin-forms/user-interface/button.md) / [Beispiel](https://developer.xamarin.com/samples/UserInterface/ButtonDemos/) | [![Schaltfläche Beispiel](views-images/Button.png "Schaltfläche Beispiel")](views-images/Button-Large.png#lightbox "Schaltfläche-Beispiel")<br /> [C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ButtonDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ButtonDemoPage.xaml) mit [CodeBehind](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ButtonDemoPage.xaml.cs) |
|     |     |

### <a name="imagebutton"></a>ImageButton

|     |     |
| --- | --- |
| `ImageButton` ist ein rechteckiges Objekt ein Bild anzeigt und die löst eine `Clicked` Ereignis aus, wenn es gedrückt wurde, wird.<br /><br /> [Handbuch](~/xamarin-forms/user-interface/imagebutton.md) / [Beispiel](https://developer.xamarin.com/samples/xamarin-forms/FormsGallery/) | [![Beispiel für ImageButton](views-images/ImageButton.png "ImageButton Beispiel")](views-images/ImageButton-Large.png#lightbox "ImageButton-Beispiel")<br /> [C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ImageButtonDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ImageButtonDemoPage.xaml) mit [CodeBehind](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ImageButtonDemoPage.xaml.cs) |
|     |     |

### <a name="searchbar"></a>SearchBar

|     |     |
| --- | --- |
| [`SearchBar`](xref:Xamarin.Forms.SearchBar) Zeigt einen Bereich für den Benutzer geben eine Zeichenfolge, und eine Schaltfläche (oder eine Taste der Tastatur), der signalisiert die Anwendung gesucht werden soll. Die [ `Text` ](xref:Xamarin.Forms.SearchBar.Text) Eigenschaft bietet Zugriff auf den Text ein, und die [ `SearchButtonPressed` ](xref:Xamarin.Forms.SearchBar.SearchButtonPressed) Ereignis weist darauf hin, dass die Schaltfläche gedrückt wurde.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.SearchBar) | [![Beispiel für SearchBar](views-images/SearchBar.png "SearchBar Beispiel")](views-images/SearchBar-Large.png#lightbox "SearchBar-Beispiel")<br /> [C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SearchBarDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SearchBarDemoPage.xaml) mit [CodeBehind](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SearchBarDemoPage.xaml.cs) |
|     |     |

## <a name="views-for-setting-values"></a>Ansichten für das Festlegen von Werten

### <a name="slider"></a>Slider

|     |     |
| --- | --- |
| [`Slider`](xref:Xamarin.Forms.Slider) ermöglicht dem Benutzer, wählen Sie eine `double` Wert aus einen durchgehenden Bereich angegeben, mit der [ `Minimum` ](xref:Xamarin.Forms.Slider.Minimum) und [ `Maximum` ](xref:Xamarin.Forms.Slider.Maximum) Eigenschaften.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.Slider) / [Handbuch](~/xamarin-forms/user-interface/slider.md) / [Beispiel](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/SliderDemos) | [![Beispiel für Schieberegler](views-images/Slider.png "Schieberegler Beispiel")](views-images/Slider-Large.png#lightbox "Schieberegler-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SliderDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SliderDemoPage.xaml) |
|     |     |

### <a name="stepper"></a>Zugeordnetem

|     |     |
| --- | --- |
| [`Stepper`](xref:Xamarin.Forms.Stepper) ermöglicht dem Benutzer, wählen Sie eine `double` Wert aus einem inkrementellen Werte, die mit dem angegebenen Bereich der [ `Minimum` ](xref:Xamarin.Forms.Stepper.Minimum), [ `Maximum` ](xref:Xamarin.Forms.Stepper.Maximum), und [ `Increment` ](xref:Xamarin.Forms.Stepper.Increment) Eigenschaften.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.Stepper)  / [Handbuch](~/xamarin-forms/user-interface/stepper.md) / [Beispiel](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/StepperDemos) | [![Beispiel mit zugeordnetem](views-images/Stepper.png "zugeordnetem Beispiel")](views-images/Stepper-Large.png#lightbox "zugeordnetem-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/StepperDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/StepperDemoPage.xaml) |
|     |     |

### <a name="switch"></a>Schalter

|     |     |
| --- | --- |
| [`Switch`](xref:Xamarin.Forms.Switch) nimmt die Form einer Ein-/Ausschalter, die der Benutzer einen booleschen Wert auswählen kann. Die [ `IsToggled` ](xref:Xamarin.Forms.Switch.IsToggled) Eigenschaft ist der Status des Switches, und die [ `Toggled` ](xref:Xamarin.Forms.Switch.Toggled) Ereignis wird ausgelöst, wenn sich der Status ändert.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.Switch) | [![Beispiel für einen Switch](views-images/Switch.png "Beispiel für einen Switch")](views-images/Switch-Large.png#lightbox "Beispiel wechseln")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SwitchDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SwitchDemoPage.xaml) |
|     |     |

### <a name="datepicker"></a>DatePicker

|     |     |
| --- | --- |
| [`DatePicker`](xref:Xamarin.Forms.DatePicker) ermöglicht dem Benutzer mit der Plattform Datumsauswahl ein Datum auswählen. Legen Sie einen Datumsbereich zulässige mit der [ `MinimumDate` ](xref:Xamarin.Forms.DatePicker.MinimumDate) und [ `MaximumDate` ](xref:Xamarin.Forms.DatePicker.MaximumDate) Eigenschaften. Die [ `Date` ](xref:Xamarin.Forms.DatePicker.Date) -Eigenschaft ist dem ausgewählten Datum ein, und die [ `DateSelected` ](xref:Xamarin.Forms.DatePicker.DateSelected) Ereignis wird ausgelöst, wenn diese Eigenschaft geändert wird.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.DatePicker) / [Handbuch](~/xamarin-forms/user-interface/datepicker.md) / [Beispiel](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/DatePicker) | [![Beispiel für "DatePicker"](views-images/DatePicker.png "\"DatePicker\" Beispiel")](views-images/DatePicker-Large.png#lightbox "\"DatePicker\"-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/DatePickerDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/DatePickerDemoPage.xaml) |
|     |     |

### <a name="timepicker"></a>TimePicker

|     |     |
| --- | --- |
| [`TimePicker`](xref:Xamarin.Forms.TimePicker) ermöglicht dem Benutzer, eine Zeit mit der Plattform / Zeitauswahl auszuwählen. Die [ `Time` ](xref:Xamarin.Forms.TimePicker.Time) -Eigenschaft ist der ausgewählten Zeit. Überwachung einer Anwendung kann Änderungen an der `Time` Eigenschaft, indem Sie installieren einen Handler für die [ `PropertyChanged` ](xref:Xamarin.Forms.BindableObject.PropertyChanged) Ereignis.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.TimePicker) / [Handbuch](~/xamarin-forms/user-interface/timepicker.md) / [Beispiel](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/TimePicker) | [![Beispiel für TimePicker](views-images/TimePicker.png "TimePicker-Beispiel")](views-images/TimePicker-Large.png#lightbox "TimePicker-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TimePickerDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TimePickerDemoPage.xaml) |
|     |     |

## <a name="views-for-editing-text"></a>Ansichten zum Bearbeiten von text

Diese beiden Klassen werden von der [ `InputView` ](xref:Xamarin.Forms.InputView) -Klasse, die definiert, die [ `Keyboard` ](xref:Xamarin.Forms.InputView.Keyboard) Eigenschaft.

<a name="entry" />

### <a name="entry"></a>Eingabe

|     |     |
| --- | --- |
| [`Entry`](xref:Xamarin.Forms.Entry) ermöglicht dem Benutzer zum eingeben und bearbeiten eine einzelne Textzeile. Der Text ist verfügbar, als die [ `Text` ](xref:Xamarin.Forms.Entry.Text) -Eigenschaft, und die [ `TextChanged` ](xref:Xamarin.Forms.Entry.TextChanged) und [ `Completed` ](xref:Xamarin.Forms.Entry.Completed) Ereignisse werden ausgelöst, wenn der Text ändert oder der Benutzer signalisiert Abschluss durch Tippen auf die EINGABETASTE.<br /><br />Verwenden einer [ `Editor` ](#editor) zum eingeben und bearbeiten mehrere Textzeilen.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.Entry) / [Handbuch](~/xamarin-forms/user-interface/text/entry.md) / [Beispiel](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text) | [![Beispiel für einen Indexeintrag](views-images/Entry.png "Beispiel für einen Indexeintrag")](views-images/Entry-Large.png#lightbox "Beispiel für einen Indexeintrag")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/EntryDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/EntryDemoPage.xaml) |
|     |     |

<a name="editor" />

### <a name="editor"></a>Editor

|     |     |
| --- | --- |
| [`Editor`](xref:Xamarin.Forms.Editor) ermöglicht dem Benutzer zum eingeben und bearbeiten mehrere Textzeilen. Der Text ist verfügbar, als die [ `Text` ](xref:Xamarin.Forms.Editor.Text) -Eigenschaft, und die [ `TextChanged` ](xref:Xamarin.Forms.Editor.TextChanged) und [ `Completed` ](xref:Xamarin.Forms.Editor.Completed) Ereignisse werden ausgelöst, wenn der Text ändert oder der Benutzer signalisiert Abschluss.<br /><br />Verwenden einer [ `Entry` ](#entry) Ansicht für die Eingabe und Bearbeitung von einer einzelnen Textzeile an.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.Editor) / [Handbuch](~/xamarin-forms/user-interface/text/editor.md) / [Beispiel](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text) | [![Beispiel für einen Indexeintrag](views-images/Editor.png "Beispiel für den Editor")](views-images/Editor-Large.png#lightbox "-Editor-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/EditorDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/EditorDemoPage.xaml) |
|     |     |

## <a name="views-to-indicate-activity"></a>Ansichten für die Aktivität angeben.

<a name="activityindicator" />

### <a name="activityindicator"></a>ActivityIndicator

|     |     |
| --- | --- |
| [`ActivityIndicator`](xref:Xamarin.Forms.ActivityIndicator) mit einer Animation an, dass die Anwendung in einer langen Aktivität beteiligt ist, ohne dass darauf ausgeführt wird. Die [ `IsRunning` ](xref:Xamarin.Forms.ActivityIndicator.IsRunning) -Eigenschaft steuert die Animation.<br /><br />Wenn die Aktivität den Status bekannt ist, verwenden Sie eine [ `ProgressBar` ](#progressbar) stattdessen.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.ActivityIndicator) | [![ActivityIndicator-Beispiel](views-images/ActivityIndicator.png "ActivityIndicator-Beispiel")](views-images/ActivityIndicator-Large.png#lightbox "ActivityIndicator-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ActivityIndicatorDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ActivityIndicatorDemoPage.xaml) |
|     |     |

<a name="progressbar" />

### <a name="progressbar"></a>ProgressBar

|     |     |
| --- | --- |
| [`ProgressBar`](xref:Xamarin.Forms.ProgressBar) verwendet eine Animation an, dass die Anwendung über eine lange Aktivität fortgesetzt wird. Legen Sie die [ `Progress` ](xref:Xamarin.Forms.ProgressBar.Progress) Eigenschaft, um Werte zwischen 0 und 1 fest, um den Status angeben.<br /><br />Wenn der Aktivität Status unbekannt ist, verwenden Sie eine [ `ActivityIndicator` ](#activityindicator) stattdessen.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.ProgressBar) | [![Beispiel für "ProgressBar"](views-images/ProgressBar.png "\"ProgressBar\" Beispiel")](views-images/ProgressBar-Large.png#lightbox "ProgressBar-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ProgressBarDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ProgressBarDemoPage.xaml) mit [CodeBehind](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ProgressBarDemoPage.xaml.cs) |
|     |     |

## <a name="views-that-display-collections"></a>Ansichten, die Auflistungen anzeigen.

### <a name="collectionview"></a>CollectionView

|     |     |
| --- | --- |
| `CollectionView` Zeigt eine bildlauffähige Liste der auswählbaren Datenelemente, die mit anderen Layout-Spezifikationen. Es zielt darauf ab, die eine flexiblere, bereitstellen und leistungsfähige Alternative zu [ `ListView` ](xref:Xamarin.Forms.ListView). Legen Sie die `ItemsSource` Eigenschaft, um eine Auflistung von Objekten, und legen Sie die `ItemTemplate` Eigenschaft, um eine [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) beschreiben, wie die Elemente zu formatierende Objekt. Die `SelectionChanged` Ereignis signalisiert, dass eine Auswahl getroffen wurde, die als verfügbar ist, die `SelectedItem` Eigenschaft.<br /><br />[Handbuch](~/xamarin-forms/user-interface/collectionview/index.md) / [Beispiel](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/CollectionViewDemos/) | [![Beispiel für CollectionView](views-images/CollectionView.png "CollectionView Beispiel")](views-images/CollectionView-Large.png#lightbox "CollectionView-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/forms40/FormsGallery/FormsGallery/FormsGallery/CodeExamples/CollectionViewDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/forms40/FormsGallery/FormsGallery/FormsGallery/XamlExamples/CollectionViewDemoPage.xaml) |
|     |     |

<a name="listView" />

### <a name="listview"></a>ListView

|     |     |
| --- | --- |
| [`ListView`](xref:Xamarin.Forms.ListView) leitet sich von [ `ItemsView` ](xref:Xamarin.Forms.ItemsView`1) und eine bildlauffähige Liste der auswählbaren Datenelemente angezeigt. Legen Sie die [ `ItemsSource` ](xref:Xamarin.Forms.ItemsView`1.ItemsSource) Eigenschaft, um eine Auflistung von Objekten, und legen Sie die [ `ItemTemplate` ](xref:Xamarin.Forms.ItemsView`1.ItemTemplate) Eigenschaft, um eine [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) Objekt beschreibt, wie die Elemente werden formatiert werden. Die [ `ItemSelected` ](xref:Xamarin.Forms.ListView.ItemSelected) Ereignis signalisiert, dass eine Auswahl getroffen wurde, die als verfügbar ist, die [ `SelectedItem` ](xref:Xamarin.Forms.ListView.SelectedItem) Eigenschaft.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.ListView) / [Handbuch](~/xamarin-forms/user-interface/listview/index.md) / [Beispiel](https://developer.xamarin.com/samples/WorkingWithListview) | [![ListView-Beispiel](views-images/ListView.png "ListView-Beispiel")](views-images/ListView-Large.png#lightbox "ListView-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ListViewDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ListViewDemoPage.xaml) |
|     |     |

### <a name="picker"></a>Auswahl

|     |     |
| --- | --- |
| [`Picker`](xref:Xamarin.Forms.Picker) Zeigt ein ausgewähltes Element aus einer Liste von Textzeichenfolgen und ermöglicht die Auswahl dieses Elements, wenn die Ansicht angetippt wird. Legen Sie die [ `Items` ](xref:Xamarin.Forms.Picker.Items) Eigenschaft, um eine Liste von Zeichenfolgen, oder die [ `ItemsSource` ](xref:Xamarin.Forms.Picker.ItemsSource) Eigenschaft, um eine Auflistung von Objekten. Die [ `SelectedIndexChanged` ](xref:Xamarin.Forms.Picker.SelectedIndexChanged) Ereignis wird ausgelöst, wenn ein Element ausgewählt ist.<br /><br />Die `Picker` zeigt die Liste der Elemente nur, wenn diese Option ausgewählt ist. Verwenden einer [ `ListView` ](#listView) oder [ `TableView` ](#tableView) eine bildlauffähige Liste, die auf der Seite verbleibt.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.Picker) / [Handbuch](~/xamarin-forms/user-interface/picker/index.md) / [Beispiel](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PickerDemo/) | [![Beispiel für die Auswahl](views-images/Picker.png "Auswahl Beispiel")](views-images/Picker-Large.png#lightbox "Picker-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/PickerDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/PickerDemoPage.xaml) mit [CodeBehind](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/PickerDemoPage.xaml.cs) |
|     |     |

<a name="tableView" />

### <a name="tableview"></a>TableView

|     |     |
| --- | --- |
| [`TableView`](xref:Xamarin.Forms.TableView) Zeigt eine Liste von Zeilen vom Typ [ `Cell` ](xref:Xamarin.Forms.Cell) mit optionalen Header und Subheader hinzuzufügen. Legen Sie die [ `Root` ](xref:Xamarin.Forms.TableView.Root) Eigenschaft, um ein Objekt vom Typ [ `TableRoot` ](xref:Xamarin.Forms.TableRoot), und fügen [ `TableSection` ](xref:Xamarin.Forms.TableSection) -Objekte mit `TableRoot`. Jede `TableSection` ist eine Sammlung von `Cell` Objekte.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.TableView) / [Handbuch](~/xamarin-forms/user-interface/tableview.md) / [Beispiel](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/TableView) | [![Beispiel für TableView](views-images/TableView.png "TableView Beispiel")](views-images/TableView-Large.png#lightbox "TableView-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TableViewDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TableViewDemoPage.xaml) |
|     |     |

## <a name="related-links"></a>Verwandte Links

- [Beispiel für Xamarin.Forms FormsGallery](https://developer.xamarin.com/samples/FormsGallery/)
- [Xamarin.Forms Samples (Beispiele für Xamarin.Forms)](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [Xamarin.Forms-API-Dokumentation](https://docs.microsoft.com/dotnet/api/xamarin.forms?view=xamarin-forms)
