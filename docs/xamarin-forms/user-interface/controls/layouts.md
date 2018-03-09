---
title: Xamarin.Forms Layouts
description: "Xamarin.Forms-Layouts werden verwendet, um die Steuerelemente der Benutzeroberfläche zu logischen Strukturen zu kombinieren."
ms.topic: article
ms.prod: xamarin
ms.assetid: F4180997-BA21-453A-9958-D1E2940DF050
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/12/2016
ms.openlocfilehash: ecea0f55360fcde7a50c52bb33c45a2c5fff5eeb
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2018
---
# <a name="xamarinforms-layouts"></a>Xamarin.Forms Layouts

_Xamarin.Forms-Layouts werden verwendet, um die Steuerelemente der Benutzeroberfläche zu logischen Strukturen zu kombinieren._

<style>.tableimg { max-width: none !important;}</style>

## <a name="layouts"></a>Layouts

Die [`Layout`](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout) Klasse in Xamarin.Forms ist ein spezialisierter Untertyp des anzeigen, die als Container für andere Layouts oder Sichten fungiert. Es enthält in der Regel Logik, um die Position und Größe von untergeordneten Elementen in Xamarin.Forms Anwendungen festzulegen.

 [ ![](layouts-images/layouts-sml.png "Xamarin.Forms Layout Typen")](layouts-images/layouts.png "Xamarin.Forms-Layout-Typen")

<br clear="all" />

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
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPresenter/">ContentPresenter</a>
    </td>
    <td valign="top">
Ein Layout-Manager für vorlagenbasierte Ansichten. Innerhalb einer <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/">ControlTemplate</a> , markieren Sie der Inhalt so angezeigt werden, wo angezeigt wird.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/Templates/ControlTemplates/SimpleTheme/SimpleTheme/App.xaml"><img src="layouts-images/ContentPresenter.png" title="ContentPresenter-Beispiel" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/">ContentView</a>
    </td>
    <td valign="top">
Ein Element mit einem einzelnen Inhalt. ContentView hat kaum Verwendung. Dieses dient als Basisklasse für benutzerdefinierte zusammengesetzten Ansichten dienen.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/ContentViewDemoPage.cs"><img src="layouts-images/ContentView.png" title="ContentView-Beispiel" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Frame/">Frame</a>
    </td>
    <td valign="top">
Ein Element, das ein einzelnes untergeordnetes Element, mit einigen Framing-Optionen enthält. Frame haben einen Standardwert <a href="https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/">Xamarin.Forms.Layout.Padding</a> von 20.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/FrameDemoPage.cs"><img src="layouts-images/Frame.png" title="Frame-Beispiel" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/">ScrollView</a>
    </td>
    <td valign="top">
Ein Element kann Durchführen eines Bildlaufs wird jedoch Inhalt ist erforderlich.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/ScrollViewDemoPage.cs"><img src="layouts-images/ScrollView.png" title="ScrollView-Beispiel" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TemplatedView/">TemplatedView</a>
    </td>
    <td valign="top">
Ein Element, das Inhalt mit einer Steuerelementvorlage und die Basisklasse für zeigt <a href=""/api/type/Xamarin.Forms.ContentView/">ContentView</a>.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/tree/master/Templates/ControlTemplates/"><img src="layouts-images/TemplatedView.png" title="TemplatedView-Beispiel" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.AbsoluteLayout/">AbsoluteLayout</a>
    </td>
    <td valign="top">
Positioniert untergeordnete Elemente an den absoluten angeforderte Positionen an. Benutzer mit Anker und Grenzen definiert die Position und Größe des Steuerelements.
    </td>
    <td valign="top">
      <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/AbsoluteLayoutDemoPage.cs"><img src="layouts-images/AbsoluteLayout.png" title="AbsoluteLayout-Beispiel" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/">Grid</a>
    </td>
    <td valign="top">
Ein Layout mit Sichten, die in Zeilen und Spalten angeordnet sind.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/GridDemoPage.cs"><img src="layouts-images/Grid.png" title="Rasterbeispiel" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td>
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.RelativeLayout/">RelativeLayout</a>
    </td>
    <td valign="top">
Ein <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/%601">Layout</a> , verwendet <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Constraint/">Einschränkung</a>s Layout seiner untergeordneten Elemente.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/RelativeLayoutDemoPage.cs"><img src="layouts-images/RelativeLayout.png" title="RelativeLayout-Beispiel" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/">StackLayout</a>
    </td>
    <td valign="top">
Ein <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/">Layout</a> positioniert, die untergeordnete Elemente in einer einzelnen Zeile, die vertikal oder horizontal ausgerichtet werden kann. Dieses Layout wird das untergeordnete Element Grenzen automatisch während einer Layoutzyklus festgelegt. Benutzer, die zugewiesenen Grenzen überschrieben werden und daher sollte nicht festgelegt werden auf dem untergeordneten Element vom Benutzer.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/StackLayoutDemoPage.cs"><img src="layouts-images/StackLayout.png" title="StackLayout-Beispiel" class="tableimg">
    </a></td>
  </tr>
  </tbody>
</table>



## <a name="related-links"></a>Verwandte Links

- [Introduction to Xamarin.Forms (Einführung in Xamarin.Forms)](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [Xamarin.Forms-Katalog (Beispiel)](https://developer.xamarin.com/samples/FormsGallery/)
- [Xamarin.Forms-Beispiele](https://developer.xamarin.com/samples/tag/Xamarin.Forms/)
- [Xamarin.Forms-API-Dokumentation](https://developer.xamarin.com/api/namespace/Xamarin.Forms)
