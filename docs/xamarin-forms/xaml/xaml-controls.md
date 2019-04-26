---
title: XAML-Steuerelemente
description: Alle Ansichten, die in Xamarin.Forms definiert sind, kann von XAML-Dateien verwiesen werden.
ms.topic: article
ms.prod: xamarin
ms.assetid: 639BD392-1496-41BB-BB09-7652273AC9D8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/03/2019
ms.openlocfilehash: a8a61ac505eab8c458c49bde9184d6e96583d37f
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61222529"
---
# <a name="xaml-controls"></a>XAML-Steuerelemente

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://developer.xamarin.com/samples/FormsGallery/)

Ansichten sind Benutzeroberflächenobjekte wie Bezeichnungen, Schaltflächen und Schieberegler an, die häufig als bekanntermaßen *Steuerelemente* oder *Widgets* in andere grafische programmierumgebungen. Die Ansichten von Xamarin.Forms alle abgeleitet unterstützt die [ `View` ](xref:Xamarin.Forms.View) Klasse.

Alle Ansichten, die in Xamarin.Forms definiert sind, kann von XAML-Dateien verwiesen werden.

## <a name="views-for-presentation"></a>Ansichten für die Präsentation

|     |     |
| --- | --- |
| <h3>BoxView</h3>Zeigt ein Rechteck einer bestimmten Farbe an.<p align="center">![Screenshot des eine BoxView](xaml-controls-images/BoxView.png "BoxView")</p>[API](xref:Xamarin.Forms.BoxView) / [Handbuch](https://developer.xamarin.com/guides/xamarin-forms/user-interface/boxview/) | <pre valign="center">&lt;BoxView Color="Accent"<br />         WidthRequest="150"<br />         HeightRequest="150"<br />         HorizontalOptions="Center"&gt;</pre></p> |
| <h3>Bild</h3>wird eine Bitmap.<p align="center">![Screenshot eines Bilds](xaml-controls-images/Image.png "Image")</p>[API](xref:Xamarin.Forms.Image) / [Handbuch](~/xamarin-forms/user-interface/images.md) | <pre>&lt;Image Source="https://aka.ms/campus.jpg"<br />       Aspect="AspectFit"<br />       HorizontalOptions="Center" /&gt;</pre></p> |
| <h3>Bezeichnung</h3>Zeigt eine oder mehrere Textzeilen.<p align="center">![Screenshot einer Bezeichnung](xaml-controls-images/Label.png "Bezeichnung")</p>[API](xref:Xamarin.Forms.Label) / [Handbuch](~/xamarin-forms/user-interface/text/label.md) | <p valign="center"><pre>&lt;Label Text="Hello, Xamarin.Forms!"<br />       FontSize="Large"<br />       FontAttributes="Italic"<br />       HorizontalTextAlignment="Center" /&gt;</pre></p> |
| <h3>Zuordnung</h3>eine Karte angezeigt.<p align="center">![Bildschirmabbildung von einer Zuordnung](xaml-controls-images/Map.png "Zuordnung")</p>[API](xref:Xamarin.Forms.Maps.Map) / [Handbuch](~/xamarin-forms/user-interface/map.md) | <p valign="center"><pre>&lt;maps:Map ItemsSource="{Binding Locations}" /&gt;</pre></p> |
| <h3>WebView</h3>Zeigt an, Webseiten oder HTML-Inhalt.<p align="center">![Screenshot der WebView](xaml-controls-images/WebView.png "WebView")</p>[API](xref:Xamarin.Forms.WebView) / [Handbuch](~/xamarin-forms/user-interface/webview.md) | <p valign="center"><pre>&lt;WebView Source="https://docs.microsoft.com/xamarin/"<br/>         VerticalOptions="FillAndExpand" /&gt;</pre></p> |
|     |     |

## <a name="views-that-initiate-commands"></a>Sichten, die Befehle zu initiieren.

|     |     |
| --- | --- |
| <h3>Schaltfläche</h3>Zeigt den Text in einem rechteckigen-Objekt.<p align="center">![Screenshot der Schaltfläche](xaml-controls-images/Button.png "Schaltfläche")</p>[API](xref:Xamarin.Forms.Button) / [Handbuch](~/xamarin-forms/user-interface/button.md) | <p valign="center"><pre>&lt;Button Text="Click Me!"<br />        Font="Large"<br />        BorderWidth="1"<br />        HorizontalOptions="Center"<br />        VerticalOptions="CenterAndExpand"<br />        Clicked="OnButtonClicked" /&gt;</pre></p> |
| <h3>ImageButton</h3>Zeigt ein Bild in einem rechteckigen-Objekt.<p align="center">![Screenshot der ImageButton](xaml-controls-images/ImageButton.png "ImageButton")</p>[API](xref:Xamarin.Forms.ImageButton) / [Handbuch](~/xamarin-forms/user-interface/imagebutton.md) | <p valign="center"><pre>&lt;ImageButton Source="XamarinLogo.png"<br />             HorizontalOptions="Center"<br />             VerticalOptions="CenterAndExpand"<br />             Clicked="OnImageButtonClicked" /&gt;</pre></p> |
| <h3>SearchBar</h3>Zeigt eine Suchleiste, für das Ausführen einer Suche an.<p align="center">![Screenshot des eine SearchBar](xaml-controls-images/SearchBar.png "SearchBar")</p>[API](xref:Xamarin.Forms.SearchBar) | <p valign="center"><pre>&lt;SearchBar Placeholder="Xamarin.Forms Property"<br />           SearchButtonPressed="OnSearchBarButtonPressed" /&gt;</pre></p> |
|     |     |

## <a name="views-for-setting-values"></a>Ansichten für das Festlegen von Werten

|     |     |
| --- | --- |
| <h3>Slider</h3>Ermöglicht die Auswahl des eine `double` Wert aus einen durchgehenden Bereich.<p align="center">![Screenshot eines Schiebereglers](xaml-controls-images/Slider.png "Schieberegler")</p>[API](xref:Xamarin.Forms.Slider) / [Handbuch](~/xamarin-forms/user-interface/slider.md) | <p valign="center"><pre>&lt;Slider Minimum="0"<br />        Maximum="100"<br />        VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
| <h3>Stepper</h3>Ermöglicht die Auswahl des eine `double` Wert aus einem inkrementellen Bereich.<p align="center">![Screenshot des eine zugeordnetem](xaml-controls-images/Stepper.png "zugeordnetem")</p>[API](xref:Xamarin.Forms.Stepper) / [Handbuch](~/xamarin-forms/user-interface/stepper.md) | <p valign="center"><pre>&lt;Stepper Minimum="0"<br />         Maximum="10"<br />         Increment="0.1"<br />         HorizontalOptions="Center"<br />         VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
| <h3>Schalter</h3>Ermöglicht die Auswahl des eine `boolean` Wert.<p align="center">![Screenshot eines Schalters](xaml-controls-images/Switch.png "Switch")</p>[API](xref:Xamarin.Forms.Switch) | <p valign="center"><pre>&lt;Switch IsToggled="false"<br />        HorizontalOptions="Center"<br />        VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
| <h3>DatePicker</h3>Ermöglicht die Auswahl eines Datums.<p align="center">![Screenshot, der eine "DatePicker"](xaml-controls-images/DatePicker.png "\"DatePicker\"")</p>[API](xref:Xamarin.Forms.DatePicker) / [Handbuch](~/xamarin-forms/user-interface/datepicker.md) | <p valign="center"><pre>&lt;DatePicker Format="D"<br/>            VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
| <h3>TimePicker</h3>Ermöglicht die Auswahl ein.<p align="center">![Screenshot des ein TimePicker](xaml-controls-images/TimePicker.png "TimePicker")</p>[API](xref:Xamarin.Forms.TimePicker) / [Handbuch](~/xamarin-forms/user-interface/timepicker.md) | <p valign="center"><pre>&lt;TimePicker Format="T"<br />            VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
|     |     |

## <a name="views-for-editing-text"></a>Ansichten zum Bearbeiten von text

|     |     |
| --- | --- |
| <h3>Eingabe</h3>Ermöglicht eine Textzeile eingegeben und bearbeitet werden.<p align="center">![Screenshot eines Eintrags](xaml-controls-images/Entry.png "Eintrag")</p>[API](xref:Xamarin.Forms.Entry) / [Handbuch](~/xamarin-forms/user-interface/text/entry.md) | <p valign="center"><pre>&lt;Entry Keyboard="Email"<br />       Placeholder="Enter email address"<br />       VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
| <h3>Editor</h3>Können mehrere Zeilen Text eingegeben und bearbeitet werden.<p align="center">![Screenshot eines Editors](xaml-controls-images/Editor.png "Bezeichnung")</p>[API](xref:Xamarin.Forms.Editor) / [Handbuch](~/xamarin-forms/user-interface/text/editor.md) | <p valign="center"><pre>&lt;Editor VerticalOptions="FillAndExpand" /&gt;</pre></p> |
|     |     |

## <a name="views-to-indicate-activity"></a>Ansichten für die Aktivität angeben.

|     |     |
| --- | --- |
| <h3>ActivityIndicator</h3>Zeigt eine Animation, um anzugeben, dass die Anwendung in einer langen Aktivität beteiligt ist, ohne dass darauf ausgeführt.<p align="center">![Screenshot des ein ActivityIndicator](xaml-controls-images/ActivityIndicator.png "ActivityIndicator")</p>[API](xref:Xamarin.Forms.ActivityIndicator) | <p valign="center"><pre>&lt;ActivityIndicator IsRunning="True"<br />                   VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
| <h3>ProgressBar</h3>Zeigt eine Animation, um anzugeben, dass die Anwendung über eine lange Aktivität fortgesetzt wird.<p align="center">![Screenshot der ProgressBar](xaml-controls-images/ProgressBar.png "\"ProgressBar\"")</p>[API](xref:Xamarin.Forms.ProgressBar) | <p valign="center"><pre>&lt;ProgressBar Progress=".5"<br />             VerticalOptions="CenterAndExpand" /&gt;</pre></p> |
|     |     |

## <a name="views-that-display-collections"></a>Ansichten, die Auflistungen anzeigen.

|     |     |
| --- | --- |
| <h3>CollectionView</h3>Zeigt eine bildlauffähige Liste der auswählbaren Datenelemente, die mit anderen Layout-Spezifikationen.<p align="center">![Screenshot des eine CollectionView](xaml-controls-images/CollectionView.png "CollectionView")</p>[Anleitung](~/xamarin-forms/user-interface/collectionview/index.md) | <p valign="center"><pre>&lt;CollectionView ItemsSource="{Binding Monkeys}"&gt;<br/>                ItemTemplate="{StaticResource MonkeyTemplate}"<br />    &lt;CollectionView.ItemsLayout&gt;<br />       &lt;GridItemsLayout Orientation="Vertical"<br />                        Span="2" /&gt;<br />    &lt;/CollectionView.ItemsLayout&gt;<br />&lt;/CollectionView/&gt;</pre></p> |
| <h3>ListView</h3>Zeigt eine bildlauffähige Liste der auswählbaren Datenelemente.<p align="center">![Screenshot von einer ListView](xaml-controls-images/ListView.png "ListView")</p>[API](xref:Xamarin.Forms.ListView) / [Handbuch](~/xamarin-forms/user-interface/listview/index.md) | <p valign="center"><pre>&lt;ListView ItemsSource="{Binding Monkeys}"&gt;<br />          ItemTemplate="{StaticResource MonkeyTemplate}" /&gt;</pre></p> |
| <h3>Auswahl</h3>Zeigt eine select-Element aus einer Liste von Textzeichenfolgen.<p align="center">![Screenshot der einer Auswahl](xaml-controls-images/Picker.png "Auswahl")</p>[API](xref:Xamarin.Forms.Picker) / [Handbuch](~/xamarin-forms/user-interface/picker/index.md) | <p valign="center"><pre>&lt;Picker Title="Select a monkey"<br />        TitleColor="Red"&gt;<br />  &lt;Picker.ItemsSource&lt;<br />    &lt;x:Array Type="{x:Type x:String}"&gt;<br />      &lt;x:String&gt;Baboon&lt;/x:String&gt;<br />      &lt;x:String&gt;Capuchin Monkey&lt;/x:String&gt;<br />      &lt;x:String&gt;Blue Monkey&lt;/x:String&gt;<br />      &lt;x:String&gt;Squirrel Monkey&lt;/x:String&gt;<br />      &lt;x:String&gt;Golden Lion Tamarin&lt;/x:String&gt;<br />      &lt;x:String&gt;Howler Monkey&lt;/x:String&gt;<br />      &lt;x:String&gt;Japanese Macaque&lt;/x:String&gt;<br />    &lt;/x:Array&gt;<br />  &lt;/Picker.ItemsSource&gt;<br />&lt;/Picker&gt;</pre></p> |
| <h3>TableView</h3>Zeigt eine Liste der interaktiven Zeilen.<p align="center">![Screenshot des eine TableView](xaml-controls-images/TableView.png "TableView")</p>[API](xref:Xamarin.Forms.TableView) / [Handbuch](~/xamarin-forms/user-interface/tableview.md) | <p valign="center"><pre>&lt;TableView Intent="Settings"&gt;<br />    &lt;TableRoot&gt;<br />        &lt;TableSection Title="Ring"&gt;<br />            &lt;SwitchCell Text="New Voice Mail" /&gt;<br />            &lt;SwitchCell Text="New Mail" On="true" /&gt;<br />        &lt;/TableSection&gt;<br />    &lt;/TableRoot&gt;<br />&lt;/TableView&gt;</pre></p> |
|     |     |

## <a name="related-links"></a>Verwandte Links

- [Beispiel für Xamarin.Forms FormsGallery](https://developer.xamarin.com/samples/FormsGallery/)
- [Xamarin.Forms Samples (Beispiele für Xamarin.Forms)](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [Xamarin.Forms-API-Dokumentation](https://docs.microsoft.com/dotnet/api/xamarin.forms?view=xamarin-forms)
