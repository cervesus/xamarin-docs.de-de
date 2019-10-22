---
title: XAML-Steuerelemente
description: Auf alle Sichten, die in xamarin. Forms definiert sind, kann in XAML-Dateien verwiesen werden.
ms.topic: article
ms.prod: xamarin
ms.assetid: 639BD392-1496-41BB-BB09-7652273AC9D8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/11/2019
ms.openlocfilehash: f73de44cbae0281d56cc6fd7ab21f8aa60264ed4
ms.sourcegitcommit: 21d8be9571a2fa89fb7d8ff0787ff4f957de0985
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2019
ms.locfileid: "72696117"
---
# <a name="xaml-controls"></a>XAML-Steuerelemente

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)

Sichten sind Benutzeroberflächen Objekte, wie z. b. Bezeichnungen, Schaltflächen und Schieberegler, die in anderen grafischen Programmierumgebungen häufig als Steuer *Elemente* oder *Widgets* bezeichnet werden. Die von xamarin. Forms unterstützten Ansichten werden von der [`View`](xref:Xamarin.Forms.View) -Klasse abgeleitet.

Auf alle Sichten, die in xamarin. Forms definiert sind, kann in XAML-Dateien verwiesen werden.

## <a name="views-for-presentation"></a>Sichten für die Präsentation

|     |     |
| --- | --- |
| <h3>BoxView</h3>Zeigt ein Rechteck einer bestimmten Farbe an.<p align="center">![Screenshot einer boxview](xaml-controls-images/BoxView.png "BoxView")</p>[API](xref:Xamarin.Forms.BoxView) - / [Handbuch](~/xamarin-forms/user-interface/boxview.md) | <pre valign="center">&lt;BoxView Color="Accent"<br />         WidthRequest="150"<br />         HeightRequest="150"<br />         HorizontalOptions="Center"&gt;</pre></p> |
| <h3>Bild</h3>Zeigt eine Bitmap an.<p align="center">![Screenshot eines Bilds](xaml-controls-images/Image.png "Bild")</p>[API](xref:Xamarin.Forms.Image) - / [Handbuch](~/xamarin-forms/user-interface/images.md) | <pre>&lt;Image Source="https://aka.ms/campus.jpg"<br />       Aspect="AspectFit"<br />       HorizontalOptions="Center" /&gt;</pre></p> |
| <h3>Bezeichnung</h3>Zeigt eine oder mehrere Textzeilen an.<p align="center">![Screenshot einer Bezeichnung](xaml-controls-images/Label.png "Bezeichnung")</p>[API](xref:Xamarin.Forms.Label) - / [Handbuch](~/xamarin-forms/user-interface/text/label.md) | <p valign="center"><pre>&lt;Label Text="Hello, Xamarin.Forms!"<br />       FontSize="Large"<br />       FontAttributes="Italic"<br />       HorizontalTextAlignment="Center" /&gt;</pre></p> |
| <h3>Zuordnung</h3>Zeigt eine Karte an.<p align="center">![Screenshot einer Karte](xaml-controls-images/Map.png "Zuordnung")</p>[API](xref:Xamarin.Forms.Maps.Map) - / [Handbuch](~/xamarin-forms/user-interface/map/index.md) | <p valign="center"><pre>&lt;maps:Map ItemsSource="{Binding Locations}" /&gt;</pre></p> |
| <h3>WebView</h3>Zeigt Webseiten oder HTML-Inhalte an.<p align="center">![Screenshot einer WebView](xaml-controls-images/WebView.png "WebView")</p>[API](xref:Xamarin.Forms.WebView) - / [Handbuch](~/xamarin-forms/user-interface/webview.md) | <p valign="center"><pre>&lt;WebView Source="https://docs.microsoft.com/xamarin/"<br/>         VerticalOptions="FillAndExpand" /&gt;</pre></p> |
|     |     |

## <a name="views-that-initiate-commands"></a>Sichten, die Befehle initiieren

|     |     |
| --- | --- |
| <h3>Schaltfläche</h3>Zeigt Text in einem rechteckigen Objekt an.<p align="center">![Screenshot einer Schaltfläche](xaml-controls-images/Button.png "Schaltfläche")</p>[API](xref:Xamarin.Forms.Button) - / [Handbuch](~/xamarin-forms/user-interface/button.md) | <p valign="center"><pre>&lt;Button Text="Click Me!"<br />        Font="Large"<br />        BorderWidth="1"<br />        HorizontalOptions="Center"<br />        VerticalOptions="CenterAndExpand"<br />        Clicked="OnButtonClicked" /&gt;</pre></p> |
| <h3>ImageButton</h3>Zeigt ein Bild in einem rechteckigen Objekt an.<p align="center">![Screenshot von ImageButton](xaml-controls-images/ImageButton.png "ImageButton")</p>[API](xref:Xamarin.Forms.ImageButton) - / [Handbuch](~/xamarin-forms/user-interface/imagebutton.md) | <p valign="center"><pre>&lt;ImageButton Source="XamarinLogo.png"<br />             HorizontalOptions="Center"<br />             VerticalOptions="CenterAndExpand"<br />             Clicked="OnImageButtonClicked" /&gt;</pre></p> |
| <h3>Aktuerfrischendes Ansicht</h3>Stellt Pull-to-Refresh-Funktionen für scrollbaren Inhalt bereit.<p align="center">![Screenshot einer aktuerfrischenden Ansicht](xaml-controls-images/RefreshView.png "Aktuerfrischendes Ansicht")</p>[Lenken](~/xamarin-forms/user-interface/refreshview.md) | <p valign="center"><pre>&lt;RefreshView IsRefreshing="{Binding IsRefreshing}"<br />             Command="{Binding RefreshCommand}" &gt;<br />    &lt;!-- Scrollable control goes here --&gt;<br />&lt;/RefreshView&gt;</pre></p> |

## <a name="views-for-setting-values"></a>Sichten zum Festlegen von Werten

|     |     |
| --- | --- |
| <h3>CheckBox</h3>Ermöglicht die Auswahl eines `boolean` Werts.<p align="center">![Screenshot eines Kontrollkästchens](xaml-controls-images/CheckBox.png "CheckBox")</p> [Lenken](~/xamarin-forms/user-interface/checkbox.md) | <p valign="center"><pre>&lt;CheckBox IsChecked="true"<br />          HorizontalOptions="Center"<br />          VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
| <h3>Slider</h3>Ermöglicht die Auswahl eines `double` Werts aus einem kontinuierlichen Bereich.<p align="center">![Screenshot eines Schiebereglers](xaml-controls-images/Slider.png "Slider")</p>[API](xref:Xamarin.Forms.Slider) - / [Handbuch](~/xamarin-forms/user-interface/slider.md) | <p valign="center"><pre>&lt;Slider Minimum="0"<br />        Maximum="100"<br />        VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
| <h3>Stepper</h3>Ermöglicht die Auswahl eines `double` Werts aus einem inkrementellen Bereich.<p align="center">![Screenshot eines Stepper](xaml-controls-images/Stepper.png "Stepper")</p>[API](xref:Xamarin.Forms.Stepper) - / [Handbuch](~/xamarin-forms/user-interface/stepper.md) | <p valign="center"><pre>&lt;Stepper Minimum="0"<br />         Maximum="10"<br />         Increment="0.1"<br />         HorizontalOptions="Center"<br />         VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
| <h3>Schalter</h3>Ermöglicht die Auswahl eines `boolean` Werts.<p align="center">![Screenshot eines Schalters](xaml-controls-images/Switch.png "Schalter")</p>[API](xref:Xamarin.Forms.Switch) - / [Handbuch](~/xamarin-forms/user-interface/switch.md)| <p valign="center"><pre>&lt;Switch IsToggled="false"<br />        HorizontalOptions="Center"<br />        VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
| <h3>DatePicker</h3>Ermöglicht die Auswahl eines Datums.<p align="center">![Screenshot eines DatePicker](xaml-controls-images/DatePicker.png "DatePicker")</p>[API](xref:Xamarin.Forms.DatePicker) - / [Handbuch](~/xamarin-forms/user-interface/datepicker.md) | <p valign="center"><pre>&lt;DatePicker Format="D"<br/>            VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
| <h3>TimePicker</h3>Ermöglicht die Auswahl einer Uhrzeit.<p align="center">![Screenshot einer timePicker](xaml-controls-images/TimePicker.png "TimePicker")</p>[API](xref:Xamarin.Forms.TimePicker) - / [Handbuch](~/xamarin-forms/user-interface/timepicker.md) | <p valign="center"><pre>&lt;TimePicker Format="T"<br />            VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
|     |     |

## <a name="views-for-editing-text"></a>Ansichten zum Bearbeiten von Text

|     |     |
| --- | --- |
| <h3>Eingabe</h3>Ermöglicht das eingeben und Bearbeiten einer einzelnen Textzeile.<p align="center">![Screenshot eines Eintrags](xaml-controls-images/Entry.png "Eingabe")</p>[API](xref:Xamarin.Forms.Entry) - / [Handbuch](~/xamarin-forms/user-interface/text/entry.md) | <p valign="center"><pre>&lt;Entry Keyboard="Email"<br />       Placeholder="Enter email address"<br />       VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
| <h3>Editor</h3>Ermöglicht das eingeben und Bearbeiten mehrerer Textzeilen.<p align="center">![Screenshot eines Editors](xaml-controls-images/Editor.png "Bezeichnung")</p>[API](xref:Xamarin.Forms.Editor) - / [Handbuch](~/xamarin-forms/user-interface/text/editor.md) | <p valign="center"><pre>&lt;Editor VerticalOptions="FillAndExpand" /&gt;</pre></p> |
|     |     |

## <a name="views-to-indicate-activity"></a>Sichten zur Angabe der Aktivität

|     |     |
| --- | --- |
| <h3>ActivityIndicator</h3>Zeigt eine Animation an, die anzeigt, dass die Anwendung mit einer langwierigen Aktivität beschäftigt ist, ohne dass der Fortschritt angegeben wird.<p align="center">![Screenshot eines activityindicator](xaml-controls-images/ActivityIndicator.png "ActivityIndicator")</p>[API](xref:Xamarin.Forms.ActivityIndicator) - / [Handbuch](~/xamarin-forms/user-interface/activityindicator.md) | <p valign="center"><pre>&lt;ActivityIndicator IsRunning="True"<br />                   VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
| <h3>ProgressBar</h3>Zeigt eine Animation an, die anzeigt, dass die Anwendung durch eine lange Aktivität verläuft.<p align="center">![Screenshot einer ProgressBar](xaml-controls-images/ProgressBar.png "ProgressBar")</p>[API](xref:Xamarin.Forms.ProgressBar) - / [Handbuch](~/xamarin-forms/user-interface/progressbar.md) | <p valign="center"><pre>&lt;ProgressBar Progress=".5"<br />             VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
|     |     |

## <a name="views-that-display-collections"></a>Sichten, die Sammlungen anzeigen

|     |     |
| --- | --- |
| <h3>Carouselview</h3>Zeigt eine scrollbare Liste von Datenelementen an.<p align="center">![Screenshot einer "carouselview"](xaml-controls-images/CarouselView.png "Carouselview")</p>[Lenken](~/xamarin-forms/user-interface/carouselview/index.md) | <p valign="center"><pre>&lt;CarouselView ItemsSource="{Binding Monkeys}"&gt;<br/>              ItemTemplate="{StaticResource MonkeyTemplate}" /&gt;</pre></p>|
| <h3>CollectionView</h3>Zeigt eine Bild lauffähigen Liste auswählbarer Datenelemente mit unterschiedlichen layoutspezifikationen an.<p align="center">![Screenshot einer CollectionView](xaml-controls-images/CollectionView.png "CollectionView")</p>[Lenken](~/xamarin-forms/user-interface/collectionview/index.md) | <p valign="center"><pre>&lt;CollectionView ItemsSource="{Binding Monkeys}"&gt;<br/>                ItemTemplate="{StaticResource MonkeyTemplate}"<br />    &lt;CollectionView.ItemsLayout&gt;<br />       &lt;GridItemsLayout Orientation="Vertical"<br />                        Span="2" /&gt;<br />    &lt;/CollectionView.ItemsLayout&gt;<br />&lt;/CollectionView/&gt;</pre></p> |
| <h3>ListView</h3>Zeigt eine Liste auswählbarer Datenelemente an.<p align="center">![Screenshot einer ListView](xaml-controls-images/ListView.png "ListView")</p>[API](xref:Xamarin.Forms.ListView) - / [Handbuch](~/xamarin-forms/user-interface/listview/index.md) | <p valign="center"><pre>&lt;ListView ItemsSource="{Binding Monkeys}"&gt;<br />          ItemTemplate="{StaticResource MonkeyTemplate}" /&gt;</pre></p> |
| <h3>Auswahl</h3>Zeigt ein SELECT-Element aus einer Liste von Text Zeichenfolgen an.<p align="center">![Screenshot einer Auswahl](xaml-controls-images/Picker.png "Auswahl")</p>[API](xref:Xamarin.Forms.Picker) - / [Handbuch](~/xamarin-forms/user-interface/picker/index.md) | <p valign="center"><pre>&lt;Picker Title="Select a monkey"<br />        TitleColor="Red"&gt;<br />  &lt;Picker.ItemsSource&lt;<br />    &lt;x:Array Type="{x:Type x:String}"&gt;<br />      &lt;x:String&gt;Baboon&lt;/x:String&gt;<br />      &lt;x:String&gt;Capuchin Monkey&lt;/x:String&gt;<br />      &lt;x:String&gt;Blue Monkey&lt;/x:String&gt;<br />      &lt;x:String&gt;Squirrel Monkey&lt;/x:String&gt;<br />      &lt;x:String&gt;Golden Lion Tamarin&lt;/x:String&gt;<br />      &lt;x:String&gt;Howler Monkey&lt;/x:String&gt;<br />      &lt;x:String&gt;Japanese Macaque&lt;/x:String&gt;<br />    &lt;/x:Array&gt;<br />  &lt;/Picker.ItemsSource&gt;<br />&lt;/Picker&gt;</pre></p> |
| <h3>TableView</h3>Zeigt eine Liste interaktiver Zeilen an.<p align="center">![Screenshot einer TableView](xaml-controls-images/TableView.png "TableView")</p>[API](xref:Xamarin.Forms.TableView) - / [Handbuch](~/xamarin-forms/user-interface/tableview.md) | <p valign="center"><pre>&lt;TableView Intent="Settings"&gt;<br />    &lt;TableRoot&gt;<br />        &lt;TableSection Title="Ring"&gt;<br />            &lt;SwitchCell Text="New Voice Mail" /&gt;<br />            &lt;SwitchCell Text="New Mail" On="true" /&gt;<br />        &lt;/TableSection&gt;<br />    &lt;/TableRoot&gt;<br />&lt;/TableView&gt;</pre></p> |
|     |     |

## <a name="related-links"></a>Verwandte Links

- [Xamarin. Forms formsgallery-Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)
- [Xamarin.Forms Samples (Beispiele für Xamarin.Forms)](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.Forms)
- [Xamarin.Forms-API-Dokumentation](https://docs.microsoft.com/dotnet/api/xamarin.forms?view=xamarin-forms)
