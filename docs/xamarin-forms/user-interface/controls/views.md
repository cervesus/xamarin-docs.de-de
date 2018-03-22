---
title: Xamarin.Forms Views
description: "Xamarin.Forms-Ansichten sind die Bausteine von plattformübergreifenden mobilen Benutzeroberflächen."
ms.topic: article
ms.prod: xamarin
ms.assetid: AC070686-A423-4A98-8BB6-0B9F94C062CC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/12/2016
ms.openlocfilehash: f7d27c5226741ec2b105633206ebaa0ac73d9a7b
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/22/2018
---
# <a name="xamarinforms-views"></a>Xamarin.Forms Views

_Xamarin.Forms-Ansichten sind die Bausteine von plattformübergreifenden mobilen Benutzeroberflächen._

Ansichten sind Benutzeroberflächenobjekte wie z. B. Schaltflächen, Bezeichnungen und Schieberegler, die häufig als bekanntermaßen *Steuerelemente* oder *Widgets* in andere grafische programmierumgebungen. Die Ansichten von Xamarin.Forms alle abgeleitet unterstützt die [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) Klasse. Sie können in verschiedene Kategorien unterteilt werden:

## <a name="views-for-presentation"></a>Ansichten für die Präsentation

### <a name="label"></a>Bezeichnung

|     |     |
| --- | --- |
| [`Label`](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) Zeigt Textzeichenfolgen Einzel- oder mehrzeilige Textblöcke, entweder Konstanten- oder Variablenwerte Formatierung. Festlegen der [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/) -Eigenschaft in eine Zeichenfolge für Konstante Formatierung oder die [ `FormattedText` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.FormattedText/) Eigenschaft, um eine [ `FormattedString` ](https://developer.xamarin.com/api/type/Xamarin.Forms.FormattedString/) Objekt für die Variable die Formatierung an.<br /><br />[API-Dokumentation](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) / [Handbuch](~/xamarin-forms/user-interface/text/label.md) / [Beispiel](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text/) | [![Bezeichnen von Beispiel](views-images/Label.png "bezeichnen Beispiel")](views-images/Label-Large.png#lightbox "bezeichnen-Beispiel")<br /> [C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/LabelDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/LabelDemoPage.xaml) |
|     |     |

### <a name="image"></a>Bild

|     |     |
| --- | --- |
| [`Image`](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) Zeigt eine Bitmap an. Bitmaps heruntergeladen werden kann, über das Internet, eingebettet als Ressourcen in der common-Projekts oder Plattformprojekte oder mit einer .NET erstellt `Stream` Objekt.<br /><br />[API-Dokumentation](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) / [Handbuch](~/xamarin-forms/user-interface/images.md) / [Beispiel](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithImages/) | [![Abbildung Beispiel](views-images/Image.png "Image Beispiel")](views-images/Image-Large.png#lightbox "Bild-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ImageDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ImageDemoPage.xaml) |
|     |     |

### <a name="boxview"></a>BoxView

|     |    |
| --- | ---|
| [`BoxView`](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/) Zeigt eine durchgehende Rechteck gefärbt, indem Sie an der [ `Color` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BoxView.Color/) Eigenschaft. `BoxView` verfügt über eine Standard-Größe-Anforderung von 40 x 40. Weisen Sie für andere Größen der [ `WidthRequest` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.WidthRequest/) und [ `HeightRequest` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.HeightRequest/) Eigenschaften.<br /><br />[API-Dokumentation](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/) / [Handbuch](~/xamarin-forms/user-interface/boxview.md) / [Beispiel 1](https://developer.xamarin.com/samples/xamarin-forms/BoxView/BasicBoxView), [2](https://developer.xamarin.com/samples/xamarin-forms/BoxView/TextDecoration), [3](https://developer.xamarin.com/samples/xamarin-forms/BoxView/ColorListBox), [4 ](https://developer.xamarin.com/samples/xamarin-forms/BoxView/GameOfLife), [5](https://developer.xamarin.com/samples/xamarin-forms/BoxView/DotMatrixClock), und [6](https://developer.xamarin.com/samples/xamarin-forms/BoxView/BoxViewClock) | [![Beispiel für BoxView](views-images/BoxView.png "BoxView Beispiel")](views-images/BoxView-Large.png#lightbox "BoxView-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/BoxViewDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/BoxViewDemoPage.xaml) |
|     |     |

### <a name="webview"></a>WebView

|     |     |
| --- | --- |
| [`WebView`](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) Zeigt an, Webseiten oder HTML-Inhalt, abhängig davon, ob die [ `Source` ](https://developer.xamarin.com/api/property/Xamarin.Forms.WebView.Source/) -Eigenschaftensatz auf eine [ `UriWebViewSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.UrlWebViewSource/) oder ein [ `HtmlWebViewSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.HtmlWebViewSource/) Objekt.<br /><br />[API-Dokumentation](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) / [Handbuch](~/xamarin-forms/user-interface/webview.md) / [Beispiel 1](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithWebview/) und [2](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/WebView) | [![WebView-Beispiel](views-images/WebView.png "WebView-Beispiel")](views-images/WebView-Large.png#lightbox "WebView-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/WebViewDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/WebViewDemoPage.xaml) |
|     |     |

### <a name="openglview"></a>OpenGLView

|     |     |
| --- | --- |
| [`OpenGLView`](https://developer.xamarin.com/api/type/Xamarin.Forms.OpenGLView/) Zeigt OpenGL-Grafiken in iOS und Android-Projekte. Es gibt keine Unterstützung für die universelle Windows-Plattform. IOS- und Android-Projekte erfordern einen Verweis auf die **OpenTK 1.0** Assembly oder der **OpenTK** Version 1.0.0.0-Assembly. `OpenGLView` ist einfacher, die in einem freigegebenen Projekt verwenden. Wenn in einer PCL "oder" Standard ".NET Bibliothek verwendet wird, wird eine Abhängigkeitsdienst auch (wie im Beispielcode gezeigt) erforderlich sein.<br /><br />Dies ist der einzige Grafiken-Funktion, die in Xamarin.Forms integriert ist, aber eine Xamarin.Forms-Anwendung kann auch Rendern Grafiken mit [ `CocosSharp` ](~/xamarin-forms/user-interface/graphics/cocossharp.md), [ `SkiaSharp` ](~/xamarin-forms/user-interface/graphics/skiasharp/index.md), oder [ `UrhoSharp` ](~/xamarin-forms/user-interface/graphics/urhosharp.md).<br /><br />[API-Dokumentation](https://developer.xamarin.com/api/type/Xamarin.Forms.OpenGLView/)<br /><br /> | [![Beispiel für OpenGLView](views-images/OpenGLView.png "OpenGLView Beispiel")](views-images/OpenGLView-Large.png#lightbox "OpenGLView-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/OpenGLViewDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/OpenGLViewDemoPage.xaml) mit [CodeBehind](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/OpenGLViewDemoPage.xaml.cs) |
|     |     |

### <a name="map"></a>Zuordnung

|     |     |
| --- | --- |
| [`Map`](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) eine Karte angezeigt. Die **Xamarin.Forms.Maps** NuGet-Paket muss installiert sein. Android und universelle Windows-Plattform erfordern eine Zuordnung Autorisierungsschlüssel.<br /><br />[API-Dokumentation](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) / [Handbuch](~/xamarin-forms/user-interface/map.md) / [Beispiel](https://developer.xamarin.com/samples/WorkingWithMaps/) | [![Ordnen Sie Beispiel](views-images/Map.png "zuordnen Beispiel")](views-images/Map-Large.png#lightbox "Beispiel zuordnen")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/MapDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/MapDemoPage.xaml) |
|     |     |

## <a name="views-that-initiate-commands"></a>Sichten, die Befehle zu initiieren.

### <a name="button"></a>Schaltfläche

|     |     |
| --- | --- |
| [`Button`](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) ist ein rechteckiges-Objekt, in dem Text angezeigt und welche löst eine [ `Clicked` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Button.Clicked/) Ereignis aus, wenn sie wird gedrückt wurde.<br /><br />[API-Dokumentation](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) | [![Schaltfläche Beispiel](views-images/Button.png "Schaltfläche Beispiel")](views-images/Button-Large.png#lightbox "Schaltfläche Beispiel")<br /> [C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ButtonDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ButtonDemoPage.xaml) mit [CodeBehind](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ButtonDemoPage.xaml.cs) |
|     |     |

### <a name="searchbar"></a>SearchBar

|     |     |
| --- | --- |
| [`SearchBar`](https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/) Zeigt einen Bereich für den Benutzer auf den Typ einer Textzeichenfolge und einer Schaltfläche (oder eine Taste auf der Tastatur), der signalisiert die Anwendung gesucht werden soll. Die [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.SearchBar.Text/) Eigenschaft ermöglicht den Zugriff auf den Text ein, und die [ `SearchButtonPressed` ](https://developer.xamarin.com/api/event/Xamarin.Forms.SearchBar.SearchButtonPressed/) Ereignis zeigt an, die gedrückt wurde.<br /><br />[API-Dokumentation](https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/) | [![Beispiel für SearchBar](views-images/SearchBar.png "SearchBar Beispiel")](views-images/SearchBar-Large.png#lightbox "SearchBar-Beispiel")<br /> [C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SearchBarDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SearchBarDemoPage.xaml) mit [CodeBehind](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SearchBarDemoPage.xaml.cs) |
|     |     |

## <a name="views-for-setting-values"></a>Ansichten für das Festlegen von Werten 

### <a name="slider"></a>Slider

|     |     |
| --- | --- |
| [`Slider`](https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/) ermöglicht dem Benutzer, wählen Sie eine `double` Wert über eine Breite Palette angegeben, mit der [ `Minimum` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Slider.Minimum/) und [ `Maximum` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Slider.Maximum/) Eigenschaften.<br /><br />[API-Dokumentation](https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/) / [Handbuch](~/xamarin-forms/user-interface/slider.md) / [Beispiel](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/SliderDemos) | [![Schieberegler-Beispiel](views-images/Slider.png "Schieberegler Beispiel")](views-images/Slider-Large.png#lightbox "Schieberegler-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SliderDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SliderDemoPage.xaml) |
|     |     |

### <a name="stepper"></a>Zugeordnetem

|     |     |
| --- | --- |
| [`Stepper`](https://developer.xamarin.com/api/type/Xamarin.Forms.Stepper/) ermöglicht dem Benutzer, wählen Sie eine `double` aus einem Bereich inkrementelle Werte, die mit dem angegebenen Wert der [ `Minimum` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Stepper.Minimum/), [ `Maximum` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Stepper.Maximum/), und [ `Increment` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Stepper.Increment/) Eigenschaften.<br /><br />[API-Dokumentation](https://developer.xamarin.com/api/type/Xamarin.Forms.Stepper/) | [![Beispiel mit zugeordnetem](views-images/Stepper.png "zugeordnetem Beispiel")](views-images/Stepper-Large.png#lightbox "zugeordnetem-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/StepperDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/StepperDemoPage.xaml) |
|     |     |

### <a name="switch"></a>Schalter 

|     |     |
| --- | --- |
| [`Switch`](https://developer.xamarin.com/api/type/Xamarin.Forms.Switch/) nimmt die Form ein/aus-Schalter, damit der Benutzer einen booleschen Wert auswählen kann. Die [ `IsToggled` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Switch.IsToggled/) Eigenschaft ist der Status des Switches, und die [ `Toggled` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Switch.Toggled/) Ereignis wird ausgelöst, wenn sich der Status ändert.<br /><br />[API-Dokumentation](https://developer.xamarin.com/api/type/Xamarin.Forms.Switch/) | [![Beispiel für einen Switch](views-images/Switch.png "Beispiel für einen Switch")](views-images/Stepper-Large.png#lightbox "Beispiel wechseln")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SwitchDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SwitchDemoPage.xaml) |
|     |     |

### <a name="datepicker"></a>DatePicker

|     |     |
| --- | --- |
| [`DatePicker`](https://developer.xamarin.com/api/type/Xamarin.Forms.DatePicker/) ermöglicht es dem Benutzer mit der Plattform Datumsauswahl ein Datum auswählen. Legen Sie einen Datumsbereich zulässige mit der [ `MinimumDate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.MinimumDate/) und [ `MaximumDate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.MaximumDate/) Eigenschaften. Die [ `Date` ](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.Date/) Eigenschaft ist für das ausgewählte Datum und die [ `DateSelected` ](https://developer.xamarin.com/api/event/Xamarin.Forms.DatePicker.DateSelected/) Ereignis wird ausgelöst, wenn diese Eigenschaft geändert wird.<br /><br />[API-Dokumentation](https://developer.xamarin.com/api/type/Xamarin.Forms.DatePicker/) / [Handbuch](~/xamarin-forms/user-interface/datepicker.md) / [Beispiel](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/DatePicker) | [![DatePicker-Beispiel](views-images/DatePicker.png "DatePicker-Beispiel")](views-images/DatePicker-Large.png#lightbox "DatePicker-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/DatePickerDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/DatePickerDemoPage.xaml) |
|     |     |

### <a name="timepicker"></a>TimePicker

|     |     |
| --- | --- |
| [`TimePicker`](https://developer.xamarin.com/api/type/Xamarin.Forms.TimePicker/) ermöglicht es dem Benutzer, die eine Uhrzeit auswählen, mit der Plattform/Zeitauswahl. Die [ `Time` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TimePicker.Time/) Eigenschaft ist für die ausgewählte Zeit. Überwachen einer Anwendung Änderungen in der `Time` Eigenschaft durch die Installation von einem Handler für das [ `PropertyChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.BindableObject.PropertyChanged/) Ereignis.<br /><br />[API-Dokumentation](https://developer.xamarin.com/api/type/Xamarin.Forms.TimePicker/) | [![Beispiel für TimePicker](views-images/TimePicker.png "TimePicker Beispiel")](views-images/TimePicker-Large.png#lightbox "TimePicker-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TimePickerDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TimePickerDemoPage.xaml) |
|     |     |

## <a name="views-for-editing-text"></a>Ansichten zum Bearbeiten von text

Diese beiden Klassen werden von der [ `InputView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.InputView/) Klasse, die definiert die [ `Keyboard` ](https://developer.xamarin.com/api/property/Xamarin.Forms.InputView.Keyboard/) Eigenschaft.

<a name="entry" />

### <a name="entry"></a>Eingabe

|     |     |
| --- | --- |
| [`Entry`](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) ermöglicht es dem Benutzer eingeben und bearbeiten eine einzelne Textzeile. Der Text ist verfügbar, als die [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.Text/) -Eigenschaft, und die [ `TextChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Entry.TextChanged/) und [ `Completed` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Entry.Completed/) Ereignisse werden ausgelöst, wenn der Text ändert sich oder der Benutzer signalisiert Abschluss durch Tippen auf die EINGABETASTE.<br /><br />Verwenden einer [ `Editor` ](#editor) zum eingeben und bearbeiten mehrere Textzeilen.<br /><br />[API-Dokumentation](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) / [Handbuch](~/xamarin-forms/user-interface/text/entry.md) / [Beispiel](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text) | [![Beispiel für einen Indexeintrag](views-images/Entry.png "Eintrag Beispiel")](views-images/Entry-Large.png#lightbox "Eintrag Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/EntryDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/EntryDemoPage.xaml) |
|     |     |

<a name="editor" />

### <a name="editor"></a>Editor

|     |     |
| --- | --- |
| [`Editor`](https://developer.xamarin.com/api/type/Xamarin.Forms.Editor/) ermöglicht es dem Benutzer zur Eingabe und mehrere Textzeilen bearbeiten. Der Text ist verfügbar, als die [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Editor.Text/) -Eigenschaft, und die [ `TextChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Editor.TextChanged/) und [ `Completed` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Editor.Completed/) Ereignisse werden ausgelöst, wenn der Text ändert sich oder der Benutzer signalisiert Abschluss.<br /><br />Verwenden einer [ `Entry` ](#entry) Ansicht zum eingeben und bearbeiten eine einzelne Textzeile.<br /><br />[API-Dokumentation](https://developer.xamarin.com/api/type/Xamarin.Forms.Editor/) / [Handbuch](~/xamarin-forms/user-interface/text/editor.md) / [Beispiel](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text) | [![Beispiel für einen Indexeintrag](views-images/Editor.png "-Editor-Beispiel")](views-images/Editor-Large.png#lightbox "-Editor-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/EditorDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/EditorDemoPage.xaml) |
|     |     |

## <a name="views-to-indicate-activity"></a>Ansichten an, dass Aktivität

<a name="activityindicator" />

### <a name="activityindicator"></a>ActivityIndicator

|     |     |
| --- | --- |
| [`ActivityIndicator`](https://developer.xamarin.com/api/type/Xamarin.Forms.ActivityIndicator/) verwendet eine Animation an, dass die Anwendung in einer längeren Aktivität beteiligt ist, ohne Angabe des Status an. Die [ `IsRunning` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ActivityIndicator.IsRunning/) Eigenschaft steuert die Animation.<br /><br />Wenn die Aktivität Status bekannt ist, verwenden Sie eine [ `ProgressBar` ](#progressbar) stattdessen.<br /><br />[API-Dokumentation](https://developer.xamarin.com/api/type/Xamarin.Forms.ActivityIndicator/) | [![Beispiel für ActivityIndicator](views-images/ActivityIndicator.png "ActivityIndicator Beispiel")](views-images/ActivityIndicator-Large.png#lightbox "ActivityIndicator-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ActivityIndicatorDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ActivityIndicatorDemoPage.xaml) |
|     |     |

<a name="progressbar" />

### <a name="progressbar"></a>ProgressBar

|     |     |
| --- | --- |
| [`ProgressBar`](https://developer.xamarin.com/api/type/Xamarin.Forms.ProgressBar/) verwendet eine Animation an, dass die Anwendung über einen längeren Aktivität voranschreitet. Legen Sie die [ `Progress` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ProgressBar.Progress/) Eigenschaft, um Werte zwischen 0 und 1, um den Fortschritt anzuzeigen.<br /><br />Wenn die Aktivität Bearbeitung unbekannt ist, verwenden eine [ `ActivityIndicator` ](#activityindicator) stattdessen.<br /><br />[API-Dokumentation](https://developer.xamarin.com/api/type/Xamarin.Forms.ProgressBar/) | [![Beispiel für "ProgressBar"](views-images/ProgressBar.png ""ProgressBar" Beispiel")](views-images/ProgressBar-Large.png#lightbox ""ProgressBar"-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ProgressBarDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ProgressBarDemoPage.xaml) mit [CodeBehind](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ProgressBarDemoPage.xaml.cs) |
|     |     |

## <a name="views-that-display-collections"></a>Sichten, die Auflistungen anzeigen.

### <a name="picker"></a>Datumsauswahl

|     |     |
| --- | --- |
| [`Picker`](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) Zeigt ein ausgewähltes Element aus einer Liste von Textzeichenfolgen und ermöglicht die Auswahl dieses Elements aus, wenn die Sicht abgerufen werden. Legen Sie die [ `Items` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.Items/) Eigenschaft, um eine Liste von Zeichenfolgen, oder die [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.ItemsSource/) Eigenschaft, um eine Auflistung von Objekten. Die [ `SelectedIndexChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Picker.SelectedIndexChanged/) Ereignis wird ausgelöst, wenn ein Element ausgewählt ist.<br /><br />Die `Picker` zeigt die Liste der Elemente nur, wenn diese Option ausgewählt ist. Verwenden einer [ `ListView` ](#listView) oder [ `TableView` ](#tableView) einer bildlauffähigen Liste, die auf der Seite bleibt.<br /><br />[API-Dokumentation](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) / [Handbuch](~/xamarin-forms/user-interface/picker/index.md) / [Beispiel](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PickerDemo/) | [![Beispiel für die Auswahl einer](views-images/Picker.png "Picker-Beispiel")](views-images/Picker-Large.png#lightbox "Picker-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/PickerDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/PickerDemoPage.xaml) mit [CodeBehind](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/PickerDemoPage.xaml.cs) |
|     |     |

<a name="listView" />

### <a name="listview"></a>ListView

|     |     |
| --- | --- |
| [`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) leitet sich von [ `ItemsView[Cell]` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/) und eine bildlauffähige Liste der auswählbaren Datenelemente angezeigt. Festlegen der [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemsSource/) Eigenschaft, um eine Auflistung von Objekten, und legen die [ `ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemTemplate/) Eigenschaft, um eine [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) Objekt beschreiben, wie die Elemente formatiert werden. Die [ `ItemSelected` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemSelected/) -Ereignis signalisiert, dass eine Auswahl getroffen wurde, die als verfügbar ist, die [ `SelectedItem` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.SelectedItem/) Eigenschaft.<br /><br />[API-Dokumentation](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) / [Handbuch](~/xamarin-forms/user-interface/listview/index.md) / [Beispiel](https://developer.xamarin.com/samples/WorkingWithListview) | [![ListView-Beispiel](views-images/ListView.png "ListView-Beispiel")](views-images/ListView-Large.png#lightbox "ListView-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ListViewDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ListViewDemoPage.xaml) |
|     |     |

<a name="tableView" />

### <a name="tableview"></a>TableView

|     |     |
| --- | --- |
| [`TableView`](https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/) Zeigt eine Liste mit Zeilen vom Typ [ `Cell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/) mit optionalen Header und Subheaders. Festlegen der [ `Root` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TableView.Root/) Eigenschaft, um ein Objekt des Typs [ `TableRoot` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TableRoot/), und fügen [ `TableSection` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TableSection/) -Objekte mit `TableRoot`. Jede `TableSection` ist eine Sammlung von `Cell` Objekte.<br /><br />[API-Dokumentation](https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/) / [Handbuch](~/xamarin-forms/user-interface/tableview.md) / [Beispiel](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/TableView) | [![Beispiel für TableView](views-images/TableView.png "TableView Beispiel")](views-images/TableView-Large.png#lightbox "TableView-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TableViewDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TableViewDemoPage.xaml) |
|     |     |

## <a name="related-links"></a>Verwandte Links

- [Introduction to Xamarin.Forms (Einführung in Xamarin.Forms)](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [Xamarin.Forms FormsGallery-Beispiel](https://developer.xamarin.com/samples/FormsGallery/)
- [Xamarin.Forms Samples (Beispiele für Xamarin.Forms)](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [Xamarin.Forms-API-Dokumentation](https://developer.xamarin.com/api/root/Xamarin.Forms/)
