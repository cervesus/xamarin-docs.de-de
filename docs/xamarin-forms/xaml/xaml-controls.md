---
title: XAML-Steuerelemente
description: Auf alle in definierten Sichten Xamarin.Forms kann in XAML-Dateien verwiesen werden.
ms.topic: article
ms.prod: xamarin
ms.assetid: 639BD392-1496-41BB-BB09-7652273AC9D8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/09/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: fd871b883d843fae039f6bdabac5812e90c451ca
ms.sourcegitcommit: cd0c0999b53e825b60471bfbfd4144cfcd783587
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2020
ms.locfileid: "86225539"
---
# <a name="xaml-controls"></a>XAML-Steuerelemente

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)

Sichten sind Benutzeroberflächen Objekte, wie z. b. Bezeichnungen, Schaltflächen und Schieberegler, die in anderen grafischen Programmierumgebungen häufig als Steuer *Elemente* oder *Widgets* bezeichnet werden. Die von allen unterstützten Sichten werden Xamarin.Forms von der- [`View`](xref:Xamarin.Forms.View) Klasse abgeleitet.

Auf alle in definierten Sichten Xamarin.Forms kann in XAML-Dateien verwiesen werden.

## <a name="views-for-presentation"></a>Ansichten für die Präsentation

|     |     |
| --- | --- |
| <h3>BoxView</h3>Zeigt ein Rechteck einer bestimmten Farbe an.<p align="center">![Screenshot einer boxview](xaml-controls-images/BoxView.png "BoxView")</p>[API](xref:Xamarin.Forms.BoxView)  /  [Leitfaden](~/xamarin-forms/user-interface/boxview.md) | <p valign="center"><pre>&lt;BoxView Color="Accent"<br />         WidthRequest="150"<br />         HeightRequest="150"<br />         HorizontalOptions="Center"&gt;</pre></p> |
| <h3>Ellipse</h3>Zeigt eine Ellipse oder einen Kreis an.<p align="center">![Screenshot einer Ellipse](xaml-controls-images/Ellipse.png "Ellipse")</p>[API](xref:Xamarin.Forms.Shapes.Ellipse)  /  [Leitfaden](~/xamarin-forms/user-interface/shapes/ellipse.md) | <p valign="center"><pre>&lt;Ellipse Fill="Red"<br />         WidthRequest="150"<br />         HeightRequest="50"<br />         HorizontalOptions="Center" /&gt;</pre></p> |
| <h3>Expander</h3>Stellt einen erweiterbaren Container bereit, um beliebige Inhalte zu hosten.<p align="center">![Screenshot eines Expander](xaml-controls-images/Expander.png "Expander")</p>[Lenken](~/xamarin-forms/user-interface/expander.md) | <pre>&lt;Expander&gt;<br />    &lt;Expander.Header&gt;<br />        &lt;Label Text="Baboon" /&gt;<br />    &lt;/Expander.Header&gt;<br />    &lt;Image Source="Baboon.png"<br />           Aspect="AspectFill" /&gt;<br />&lt;/Expander&gt;</pre></p> |
| <h3>Image</h3>Zeigt eine Bitmap an.<p align="center">![Screenshot eines Bilds](xaml-controls-images/Image.png "Bild")</p>[API](xref:Xamarin.Forms.Image)  /  [Leitfaden](~/xamarin-forms/user-interface/images.md) | <pre>&lt;Image Source="https://aka.ms/campus.jpg"<br />       Aspect="AspectFit"<br />       HorizontalOptions="Center" /&gt;</pre></p> |
| <h3>Bezeichnung</h3>Zeigt eine oder mehrere Textzeilen an.<p align="center">![Screenshot einer Bezeichnung](xaml-controls-images/Label.png "Bezeichnung")</p>[API](xref:Xamarin.Forms.Label)  /  [Leitfaden](~/xamarin-forms/user-interface/text/label.md) | <p valign="center"><pre>&lt;Label Text="Hello, Xamarin.Forms!"<br />       FontSize="Large"<br />       FontAttributes="Italic"<br />       HorizontalTextAlignment="Center" /&gt;</pre></p> |
| <h3>Linie</h3>Zeigt eine Zeile an.<p align="center">![Screenshot einer Zeile](xaml-controls-images/Line.png "Linie")</p>[API](xref:Xamarin.Forms.Shapes.Line)  /  [Leitfaden](~/xamarin-forms/user-interface/shapes/line.md) | <p valign="center"><pre>&lt;Line X1="40"<br />      Y1="0"<br />      X2="0"<br />      Y2="120"<br />      Stroke="Red"<br />      HorizontalOptions="Center" /&gt;</pre></p> |
| <h3>Zuordnung</h3>Zeigt eine Karte an.<p align="center">![Screenshot einer Karte](xaml-controls-images/Map.png "Zuordnung")</p>[API](xref:Xamarin.Forms.Maps.Map)  /  [Leitfaden](~/xamarin-forms/user-interface/map/index.md) | <p valign="center"><pre>&lt;maps:Map ItemsSource="{Binding Locations}" /&gt;</pre></p> |
| <h3>MediaElement</h3>Abspielen von Videos oder Audiodaten.<p align="center">![Screenshot eines Media Element-Elements](xaml-controls-images/MediaElement.png "Media Element")</p>[API](xref:Xamarin.Forms.MediaElement)  /  [Leitfaden](~/xamarin-forms/user-interface/mediaelement.md) | <p valign="center"><pre>&lt;MediaElement Source="https://sec.ch9.ms/ch9/XamarinShow_mid.mp4"<br />              AutoPlay="True"<br />              ShowsPlaybackControls="True" /&gt;</pre></p> |
| <h3>Pfad</h3>Anzeigen von Kurven und komplexen Formen.<p align="center">![Screenshot eines Pfads](xaml-controls-images/Path.png "Pfad")</p>[API](xref:Xamarin.Forms.Shapes.Path)  /  [Leitfaden](~/xamarin-forms/user-interface/shapes/path.md) | <p valign="center"><pre>&lt;Path Stroke="Black"<br />      Aspect="Uniform"<br />      HorizontalOptions="Center"<br />      HeightRequest="100"<br />      WidthRequest="100"<br />      Data="M13.9,16.2<br />            L32,16.2 32,31.9 13.9,30.1Z<br />            M0,16.2<br />            L11.9,16.2 11.9,29.9 0,28.6Z<br />            M11.9,2<br />            L11.9,14.2 0,14.2 0,3.3Z<br />            M32,0<br />            L32,14.2 13.9,14.2 13.9,1.8Z" /&gt;</pre></p> |
| <h3>Polygon</h3>Anzeigen eines Polygons.<p align="center">![Screenshot eines Polygons](xaml-controls-images/Polygon.png "Polygon")</p>[API](xref:Xamarin.Forms.Shapes.Polygon)  /  [Leitfaden](~/xamarin-forms/user-interface/shapes/polygon.md) | <p valign="center"><pre>&lt;Polygon Points="0 48, 0 144, 96 150, 100 0, 192 0, 192 96,<br/>                 50 96, 48 192, 150 200 144 48"<br />         Fill="Blue"<br />         Stroke="Red"<br />         StrokeThickness="3"<br />         HorizontalOptions="Center" /&gt;</pre></p> |
| <h3>Polylinie</h3>Zeigt eine Reihe verbundener gerader Linien an.<p align="center">![Screenshot einer Polylinie](xaml-controls-images/Polyline.png "Polylinie")</p>[API](xref:Xamarin.Forms.Shapes.Polyline)  /  [Leitfaden](~/xamarin-forms/user-interface/shapes/Polyline.md) | <p valign="center"><pre>&lt;Polyline Points="0,0 10,30, 15,0 18,60 23,30 35,30 40,0<br />                  43,60 48,30 100,30"<br />          Stroke="Red"<br />          HorizontalOptions="Center" /&gt;</pre></p> |
| <h3>Rechteck</h3>Anzeigen eines Rechtecks oder Quadrats.<p align="center">![Screenshot eines Rechtecks](xaml-controls-images/Rectangle.png "Rechteck")</p>[API](xref:Xamarin.Forms.Shapes.Rectangle)  /  [Leitfaden](~/xamarin-forms/user-interface/shapes/rectangle.md) | <p valign="center"><pre>&lt;Rectangle Fill="Red"<br />           WidthRequest="150"<br />           HeightRequest="50"<br />           HorizontalOptions="Center" /&gt;</pre></p> |  
| <h3>WebView</h3>Zeigt Webseiten oder HTML-Inhalte an.<p align="center">![Screenshot einer WebView](xaml-controls-images/WebView.png "WebView")</p>[API](xref:Xamarin.Forms.WebView)  /  [Leitfaden](~/xamarin-forms/user-interface/webview.md) | <p valign="center"><pre>&lt;WebView Source="https://docs.microsoft.com/xamarin/"<br/>         VerticalOptions="FillAndExpand" /&gt;</pre></p> |
|     |     |

## <a name="views-that-initiate-commands"></a>Ansichten, die Befehle initiieren

|     |     |
| --- | --- |
| <h3>Schaltfläche</h3>Zeigt Text in einem rechteckigen Objekt an.<p align="center">![Screenshot einer Schaltfläche](xaml-controls-images/Button.png "Schaltfläche")</p>[API](xref:Xamarin.Forms.Button)  /  [Leitfaden](~/xamarin-forms/user-interface/button.md) | <p valign="center"><pre>&lt;Button Text="Click Me!"<br />        Font="Large"<br />        BorderWidth="1"<br />        HorizontalOptions="Center"<br />        VerticalOptions="CenterAndExpand"<br />        Clicked="OnButtonClicked" /&gt;</pre></p> |
| <h3>ImageButton</h3>Zeigt ein Bild in einem rechteckigen Objekt an.<p align="center">![Screenshot von ImageButton](xaml-controls-images/ImageButton.png "ImageButton")</p>[API](xref:Xamarin.Forms.ImageButton)  /  [Leitfaden](~/xamarin-forms/user-interface/imagebutton.md) | <p valign="center"><pre>&lt;ImageButton Source="XamarinLogo.png"<br />             HorizontalOptions="Center"<br />             VerticalOptions="CenterAndExpand"<br />             Clicked="OnImageButtonClicked" /&gt;</pre></p> |
| <h3>RadioButton</h3>Ermöglicht die Auswahl einer Option aus einer Menge.<p align="center">![Screenshot eines RadioButton](xaml-controls-images/RadioButton.png "RadioButton")</p>[Lenken](~/xamarin-forms/user-interface/radiobutton.md) | <p valign="center"><pre>&lt;RadioButton Text="Pineapple"<br/>             CheckedChanged="OnRadioButtonCheckedChanged" /&gt;</pre></p> |
| <h3>RefreshView</h3>Stellt Pull-to-Refresh-Funktionen für scrollbaren Inhalt bereit.<p align="center">![Screenshot einer aktuerfrischenden Ansicht](xaml-controls-images/RefreshView.png "RefreshView")</p>[Lenken](~/xamarin-forms/user-interface/refreshview.md) | <p valign="center"><pre>&lt;RefreshView IsRefreshing="{Binding IsRefreshing}"<br />             Command="{Binding RefreshCommand}" &gt;<br />    &lt;!-- Scrollable control goes here --&gt;<br />&lt;/RefreshView&gt;</pre></p> |
| <h3>SearchBar</h3> Akzeptiert Benutzereingaben, die zum Durchführen einer Suche verwendet werden.<p align="center">![Screenshot einer Suchleiste](xaml-controls-images/SearchBar.png "SearchBar")</p>[Lenken](~/xamarin-forms/user-interface/searchbar.md) | <p valign="center"><pre>&lt;SearchBar Placeholder="Enter search term"<br />           SearchButtonPressed="OnSearchBarButtonPressed" /&gt;</pre></p> |
| <h3>SwipeView</h3> Stellt Kontextmenü Elemente bereit, die durch eine Schwenkbewegung angezeigt werden.<p align="center">![Screenshot einer swidanview](xaml-controls-images/SwipeView.png "SwipeView")</p>[Lenken](~/xamarin-forms/user-interface/swipeview.md) | <p valign="center"><pre>&lt;SwipeView&gt;<br />    &lt;SwipeView.LeftItems&gt;<br />        &lt;SwipeItems&gt;<br />            &lt;SwipeItem Text="Delete"<br />                       IconImageSource="delete.png"<br />                       BackgroundColor="LightPink"<br />                       Invoked="OnDeleteInvoked" /&gt;<br />        &lt;/SwipeItems&gt;<br />    &lt;/SwipeView.LeftItems&gt;<br />    &lt;!-- Content --&gt;<br />&lt;/SwipeView&gt;</pre></p> |
|     |     |

## <a name="views-for-setting-values"></a>Ansichten zum Festlegen von Werten

|     |     |
| --- | --- |
| <h3>CheckBox</h3>Ermöglicht die Auswahl eines `boolean` Werts.<p align="center">![Screenshot eines Kontrollkästchens](xaml-controls-images/CheckBox.png "CheckBox")</p> [Lenken](~/xamarin-forms/user-interface/checkbox.md) | <p valign="center"><pre>&lt;CheckBox IsChecked="true"<br />          HorizontalOptions="Center"<br />          VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
| <h3>Schieberegler</h3>Ermöglicht die Auswahl eines `double` Werts aus einem kontinuierlichen Bereich.<p align="center">![Screenshot eines Schiebereglers](xaml-controls-images/Slider.png "Slider")</p>[API](xref:Xamarin.Forms.Slider)  /  [Leitfaden](~/xamarin-forms/user-interface/slider.md) | <p valign="center"><pre>&lt;Slider Minimum="0"<br />        Maximum="100"<br />        VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
| <h3>Stepper</h3>Ermöglicht die Auswahl eines `double` Werts aus einem inkrementellen Bereich.<p align="center">![Screenshot eines Stepper](xaml-controls-images/Stepper.png "Stepper")</p>[API](xref:Xamarin.Forms.Stepper)  /  [Leitfaden](~/xamarin-forms/user-interface/stepper.md) | <p valign="center"><pre>&lt;Stepper Minimum="0"<br />         Maximum="10"<br />         Increment="0.1"<br />         HorizontalOptions="Center"<br />         VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
| <h3>Schalter</h3>Ermöglicht die Auswahl eines `boolean` Werts.<p align="center">![Screenshot eines Schalters](xaml-controls-images/Switch.png "Schalter")</p>[API](xref:Xamarin.Forms.Switch)  /  [Leitfaden](~/xamarin-forms/user-interface/switch.md)| <p valign="center"><pre>&lt;Switch IsToggled="false"<br />        HorizontalOptions="Center"<br />        VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
| <h3>DatePicker</h3>Ermöglicht die Auswahl eines Datums.<p align="center">![Screenshot eines DatePicker](xaml-controls-images/DatePicker.png "DatePicker")</p>[API](xref:Xamarin.Forms.DatePicker)  /  [Leitfaden](~/xamarin-forms/user-interface/datepicker.md) | <p valign="center"><pre>&lt;DatePicker Format="D"<br/>            VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
| <h3>TimePicker</h3>Ermöglicht die Auswahl einer Uhrzeit.<p align="center">![Screenshot einer timePicker](xaml-controls-images/TimePicker.png "TimePicker")</p>[API](xref:Xamarin.Forms.TimePicker)  /  [Leitfaden](~/xamarin-forms/user-interface/timepicker.md) | <p valign="center"><pre>&lt;TimePicker Format="T"<br />            VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
|     |     |

## <a name="views-for-editing-text"></a>Ansichten zum Bearbeiten von Text

|     |     |
| --- | --- |
| <h3>Eingabe</h3>Ermöglicht das eingeben und Bearbeiten einer einzelnen Textzeile.<p align="center">![Screenshot eines Eintrags](xaml-controls-images/Entry.png "Eingabe")</p>[API](xref:Xamarin.Forms.Entry)  /  [Leitfaden](~/xamarin-forms/user-interface/text/entry.md) | <p valign="center"><pre>&lt;Entry Keyboard="Email"<br />       Placeholder="Enter email address"<br />       VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
| <h3>Editor</h3>Ermöglicht das eingeben und Bearbeiten mehrerer Textzeilen.<p align="center">![Screenshot eines Editors](xaml-controls-images/Editor.png "Bezeichnung")</p>[API](xref:Xamarin.Forms.Editor)  /  [Leitfaden](~/xamarin-forms/user-interface/text/editor.md) | <p valign="center"><pre>&lt;Editor VerticalOptions="FillAndExpand" /&gt;</pre></p> |
|     |     |

## <a name="views-to-indicate-activity"></a>Ansichten zum Anzeigen einer Aktivität

|     |     |
| --- | --- |
| <h3>ActivityIndicator</h3>Zeigt eine Animation an, die anzeigt, dass die Anwendung mit einer langwierigen Aktivität beschäftigt ist, ohne dass der Fortschritt angegeben wird.<p align="center">![Screenshot eines activityindicator](xaml-controls-images/ActivityIndicator.png "ActivityIndicator")</p>[API](xref:Xamarin.Forms.ActivityIndicator)  /  [Leitfaden](~/xamarin-forms/user-interface/activityindicator.md) | <p valign="center"><pre>&lt;ActivityIndicator IsRunning="True"<br />                   VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
| <h3>ProgressBar</h3>Zeigt eine Animation an, die anzeigt, dass die Anwendung durch eine lange Aktivität verläuft.<p align="center">![Screenshot einer ProgressBar](xaml-controls-images/ProgressBar.png "ProgressBar")</p>[API](xref:Xamarin.Forms.ProgressBar)  /  [Leitfaden](~/xamarin-forms/user-interface/progressbar.md) | <p valign="center"><pre>&lt;ProgressBar Progress=".5"<br />             VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
|     |     |

## <a name="views-that-display-collections"></a>Ansichten, die Sammlungen anzeigen

|     |     |
| --- | --- |
| <h3>CarouselView</h3>Zeigt eine scrollbare Liste von Datenelementen an.<p align="center">![Screenshot einer "carouselview"](xaml-controls-images/CarouselView.png "CarouselView")</p>[Lenken](~/xamarin-forms/user-interface/carouselview/index.md) | <p valign="center"><pre>&lt;CarouselView ItemsSource="{Binding Monkeys}"&gt;<br/>              ItemTemplate="{StaticResource MonkeyTemplate}" /&gt;</pre></p>|
| <h3>CollectionView</h3>Zeigt eine Bild lauffähigen Liste auswählbarer Datenelemente mit unterschiedlichen layoutspezifikationen an.<p align="center">![Screenshot einer CollectionView](xaml-controls-images/CollectionView.png "CollectionView")</p>[Lenken](~/xamarin-forms/user-interface/collectionview/index.md) | <p valign="center"><pre>&lt;CollectionView ItemsSource="{Binding Monkeys}"&gt;<br/>                ItemTemplate="{StaticResource MonkeyTemplate}"<br />    &lt;CollectionView.ItemsLayout&gt;<br />       &lt;GridItemsLayout Orientation="Vertical"<br />                        Span="2" /&gt;<br />    &lt;/CollectionView.ItemsLayout&gt;<br />&lt;/CollectionView/&gt;</pre></p> |
| <h3>IndicatorView</h3>Zeigt Indikatoren an, die die Anzahl der Elemente in einer darstellen `CarouselView` .<p align="center">![Screenshot einer "indikatorview"](xaml-controls-images/IndicatorView.png "IndicatorView")</p>[Lenken](~/xamarin-forms/user-interface/indicatorview.md) | <p valign="center"><pre>&lt;IndicatorView x:Name="indicatorView"<br />               IndicatorColor="LightGray"<br />               SelectedIndicatorColor="DarkGray" /&gt;</pre></p> |
| <h3>ListView</h3>Zeigt eine Liste auswählbarer Datenelemente an.<p align="center">![Screenshot einer ListView](xaml-controls-images/ListView.png "ListView")</p>[API](xref:Xamarin.Forms.ListView)  /  [Leitfaden](~/xamarin-forms/user-interface/listview/index.md) | <p valign="center"><pre>&lt;ListView ItemsSource="{Binding Monkeys}"&gt;<br />          ItemTemplate="{StaticResource MonkeyTemplate}" /&gt;</pre></p> |
| <h3>Auswahl</h3>Zeigt ein SELECT-Element aus einer Liste von Text Zeichenfolgen an.<p align="center">![Screenshot einer Auswahl](xaml-controls-images/Picker.png "Auswahl")</p>[API](xref:Xamarin.Forms.Picker)  /  [Leitfaden](~/xamarin-forms/user-interface/picker/index.md) | <p valign="center"><pre>&lt;Picker Title="Select a monkey"<br />        TitleColor="Red"&gt;<br />  &lt;Picker.ItemsSource&gt;<br />    &lt;x:Array Type="{x:Type x:String}"&gt;<br />      &lt;x:String&gt;Baboon&lt;/x:String&gt;<br />      &lt;x:String&gt;Capuchin Monkey&lt;/x:String&gt;<br />      &lt;x:String&gt;Blue Monkey&lt;/x:String&gt;<br />      &lt;x:String&gt;Squirrel Monkey&lt;/x:String&gt;<br />      &lt;x:String&gt;Golden Lion Tamarin&lt;/x:String&gt;<br />      &lt;x:String&gt;Howler Monkey&lt;/x:String&gt;<br />      &lt;x:String&gt;Japanese Macaque&lt;/x:String&gt;<br />    &lt;/x:Array&gt;<br />  &lt;/Picker.ItemsSource&gt;<br />&lt;/Picker&gt;</pre></p> |
| <h3>TableView</h3>Zeigt eine Liste interaktiver Zeilen an.<p align="center">![Screenshot einer TableView](xaml-controls-images/TableView.png "TableView")</p>[API](xref:Xamarin.Forms.TableView)  /  [Leitfaden](~/xamarin-forms/user-interface/tableview.md) | <p valign="center"><pre>&lt;TableView Intent="Settings"&gt;<br />    &lt;TableRoot&gt;<br />        &lt;TableSection Title="Ring"&gt;<br />            &lt;SwitchCell Text="New Voice Mail" /&gt;<br />            &lt;SwitchCell Text="New Mail" On="true" /&gt;<br />        &lt;/TableSection&gt;<br />    &lt;/TableRoot&gt;<br />&lt;/TableView&gt;</pre></p> |
|     |     |

## <a name="related-links"></a>Verwandte Links

- [Xamarin.FormsFormsgallery-Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)
- [Xamarin.Forms-Beispiele](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.Forms)
- [Xamarin.FormsAPI-Dokumentation](https://docs.microsoft.com/dotnet/api/xamarin.forms?view=xamarin-forms)
