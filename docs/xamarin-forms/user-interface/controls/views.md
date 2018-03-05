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
ms.openlocfilehash: df8c8463b2556035c5369c70cb10dbc3dc6b6743
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2018
---
# <a name="xamarinforms-views"></a>Xamarin.Forms Views

_Xamarin.Forms-Ansichten sind die Bausteine von plattformübergreifenden mobilen Benutzeroberflächen._

<style>.tableimg { max-width: none !important;}</style>

## <a name="views"></a>Ansichten

Xamarin.Forms nutzt das Wort *Ansicht* zum Verweisen auf visuelle Objekte, z. B. Schaltflächen, Bezeichnungen oder Texteingabefelder - häufiger als Steuerelemente von Widgets bekannt sein können.

Diese Elemente der Benutzeroberfläche sind in der Regel ist eine Unterklasse der [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/).

<br clear="right" />

Xamarin.Forms unterstützt:

<table align="center" border="1" cellpadding="1" cellspacing="1">
<thead>
    <th>
      <strong>Datentyp</strong>
    </th>
    <th>
      <strong>Beschreibung</strong>
    </th>
    <th style="min-width:400px">
      <strong>bildschirmabbildung von</strong>
    </th>

  </thead>
  <tbody>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ActivityIndicator/">ActivityIndicator</a>
    </td>
    <td valign="top">
Ein visuelles Steuerelement verwendet, um anzugeben, dass etwas durchgeführt wird. Dieses Steuerelement ermöglicht einen visuellen Hinweis auf dem Benutzer, den etwas, ohne Angaben über den Fortschritt passiert an.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/ActivityIndicatorDemoPage.cs"><img src="views-images/ActivityIndicator.png" title="ActivityIndicator-Beispiel" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/">BoxView</a>
    </td>
    <td valign="top">
Ein <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">Ansicht</a> verwendet, um eine Volltonfarbe farbiges Rechteck gezeichnet werden soll. BoxView ist nützlich Ersatz für Bilder oder benutzerdefinierte Elemente, bei der ersten Erstellung des Prototyps. BoxView verfügt über eine Standard-Größe-Anforderung von 40 x 40. Wenn Sie eine andere Größe benötigen, weisen Sie die <a href="https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.WidthRequest/">VisualElement.WidthRequest</a> und <a href="https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.HeightRequest/">VisualElement.HeightRequest</a>.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/BoxViewDemoPage.cs"><img src="views-images/BoxView.png" title="BoxView-Beispiel" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Button/">Schaltfläche</a>
    </td>
    <td align="center" valign="top">
Eine Schaltfläche <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">Ansicht</a> , der antwortet auf um Ereignisse zu berühren.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/ButtonDemoPage.cs"><img src="views-images/Button.png" title="Schaltflächenbeispiel" class="tableimg">
    </a></td>
  </tr>
  <tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.DatePicker/">DatePicker</a>
    </td>
    <td valign="top">
Ein <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">Ansicht</a> Datum Kommissionierung ermöglicht. Die visuelle Darstellung einer DatePicker ist sehr ähnlich, mit einem der <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/">Eintrag</a>, außer dass ein spezielles Steuerelement zum Auswählen eines Datums anstelle einer Tastatur angezeigt wird. </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/DatePickerDemoPage.cs"><img src="views-images/DatePicker.png" title="DatePicker-Beispiel" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Editor/">Editor</a>
    </td>
    <td valign="top">
Ein Steuerelement, das mehrere Textzeilen bearbeiten kann. Einzeilige Einträge angezeigt werden, finden Sie unter <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/">Eintrag</a>.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/EditorDemoPage.cs"><img src="views-images/Editor.png" title="Editor-Beispiel" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/">Eintrag</a>
    </td>
    <td valign="top">
Ein Steuerelement, das eine einzelne Textzeile bearbeiten kann. Der Eintrag ist eine einzelne Zeile Text-Eintrag. Es ist am besten zum Sammeln von kleinen einzelne Fragmente von Informationen wie Benutzernamen und Kennwörter verwendet werden.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/EntryDemoPage.cs"><img src="views-images/Entry.png" title="Beispiel für einen Indexeintrag" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Image/">Bild</a>
    </td>
    <td valign="top">
Ein <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">Ansicht</a> , ein Bild enthält.
    <br />
    <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Image/">Image-API</a>
    <br />
    <a href="~/xamarin-forms/user-interface/images.md">Arbeiten mit Bildern</a>
    <br />
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/ImageDemoPage.cs">Demo-Quelle</a>
    </td>
    <td>
    <img src="views-images/Image.png" title="Image-Beispiel" class="tableimg">
    </td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Label/">Label</a>
    </td>
    <td valign="top">
Ein <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">Ansicht</a> , die in einem lesbaren Format nur Text anzeigt. Eine Bezeichnung wird verwendet, um Textelemente Einzel-als auch für Textblöcke mit mehreren Zeilen anzuzeigen.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/LabelDemoPage.cs"><img src="views-images/Label.png" title="Beispiel für Bezeichnung" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/">ListView</a>
    </td>
    <td valign="top">
Ein <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/">aufrufenArtikel aufrufen</a> , die eine Auflistung von Daten als einer vertikalen Liste anzeigt.
    <br />
    <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/">ListView-API</a>
    <br />
    <a href="~/xamarin-forms/user-interface/listview/index.md">ListView-Dokumentation</a>
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/ListViewDemoPage.cs"><img src="views-images/ListView.png" title="ListView-Beispiel" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.OpenGLView/">OpenGLView</a>
    </td>
    <td valign="top">
Ein <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">Ansicht</a> , die OpenGL-Inhalte anzeigt.
    <ul>
      <li>Funktioniert nur für IOS- und Android-Projekte (keine Windows Phone-Unterstützung).
      <li>Erfordert einen Verweis auf die <b>OpenTK 1.0</b> Assembly in die IOS- und Android-Projekte.</li>
      <li>Es ist bestens geeignet für die Verwendung in Projekten gemeinsam genutzt; Wenn in einer PCL verwendet dann eine DependencyService auch erforderlich sein.</li>
    </ul>
    </td>
    <td>
    <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.OpenGLView/"><img src="views-images/OpenGL.png" title="OpenGlView-Beispiel" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/">Datumsauswahl</a>
    </td>
    <td valign="top">
Ein <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">Ansicht</a> Steuerelement ein Element in einer Liste auswählen. Die visuelle Darstellung einer Auswahl ähnelt einer <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/">Eintrag</a>, aber ein Auswahlsteuerelement anstelle einer Tastatur angezeigt.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/PickerDemoPage.cs"><img src="views-images/Picker.png" title="Auswahl-Beispiel" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ProgressBar/">ProgressBar</a>
    </td>
    <td valign="top">
Ein <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">Ansicht</a> Steuerelement einen Status angibt.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/ProgressBarDemoPage.cs"><img src="views-images/ProgressBar.png" title="ProgressBar-Beispielklasse ="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/">SearchBar</a>
    </td>
    <td valign="top">
Ein <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">Ansicht</a> -Steuerelement, ein Suchfeld zu ermöglichen.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/SearchBarDemoPage.cs"><img src="views-images/SearchBar.png" title="SearchBar-Beispiel" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/">Schieberegler</a>
    </td>
    <td valign="top">
Ein <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">Ansicht</a> Steuerelement, das einen linearen Wert eingibt.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/SliderDemoPage.cs"><img src="views-images/Slider.png" title="Schieberegler-Beispiel" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Stepper/">Stepper</a>
    </td>
    <td valign="top">
Ein <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">Ansicht</a> Steuerelement, das einen diskreten Wert Eingaben auf einen Bereich beschränkt.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/StepperDemoPage.cs"><img src="views-images/Stepper.png" title="Zugeordnetem-Beispiel" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Switch/">Switch</a>
    </td>
    <td valign="top">
Ein <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">Ansicht</a> Steuerelement, das einen umgeschalteten Wert bereitstellt.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/SwitchDemoPage.cs"><img src="views-images/Switch.png" title="Switch-Beispiel" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/">TableView</a>
    </td>
    <td valign="top">
Ein <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">Ansicht</a> enthält Datenzeilen <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/">Zelle</a>s.
    <br />
    <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/">TableView API</a>
    <br />
    <a href="~/xamarin-forms/user-interface/tableview.md">TableView-Dokumentation</a>
    <br />
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/TableViewFormDemoPage.cs">Demo-Quelle</a>
    </td>
    <td>
    <img src="views-images/TableViewNewest.png" title="TableView-Beispiel" class="tableimg">
    </td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TimePicker/">TimePicker</a>
    </td>
    <td valign="top">
Ein <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">Ansicht</a> -Steuerelement, Zeit Kommissionierung zu ermöglichen. Die visuelle Darstellung einer TimePicker ist sehr ähnlich, mit einem der <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/">Eintrag</a>, außer dass ein spezielles Steuerelement für die Entnahme nacheinander anstelle einer Tastatur angezeigt wird.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/TimePickerDemoPage.cs"><img src="views-images/TimePicker.png" title="TimePicker-Beispiel" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/">WebView</a>
    </td>
    <td valign="top">
Ein <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">Ansicht</a> präsentiert HTML-Inhalt.
    <br />
    <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/">WebView-API</a>
    <br />
    <a href="~/xamarin-forms/user-interface/webview.md">WebView-Dokumentation</a>
    <br />
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/WebViewDemoPage.cs">Demo-Quelle</a>
    </td>
    <td>
    <img src="views-images/WebView.png" title="WebView-Beispiel" class="tableimg">
    </td>
  </tr>
  </tbody>
</table>



## <a name="related-links"></a>Verwandte Links

- [Introduction to Xamarin.Forms (Einführung in Xamarin.Forms)](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [Xamarin.Forms-Katalog (Beispiel)](https://developer.xamarin.com/samples/FormsGallery/)
- [Xamarin.Forms-Beispiele](https://developer.xamarin.com/samples/tag/Xamarin.Forms/)
- [Xamarin.Forms-API-Dokumentation](https://developer.xamarin.com/api/root/Xamarin.Forms/)
