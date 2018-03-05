---
title: Xamarin.Forms-Seiten
description: "Xamarin.Forms Seiten darstellen, plattformübergreifende mobile app-Bildschirme."
ms.topic: article
ms.prod: xamarin
ms.assetid: F2A02DEE-7137-42F4-9C0A-4E1CF75EA08F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/12/2016
ms.openlocfilehash: 35822dbbb7d5694e7f1f0a3f35f10df404206af9
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2018
---
# <a name="xamarinforms-pages"></a>Xamarin.Forms-Seiten

_Xamarin.Forms Seiten darstellen, plattformübergreifende mobile app-Bildschirme._

<style>.tableimg { max-width: none !important;}</style>

## <a name="pages"></a>Seiten

Die [ `Page` ](http://iosapi.xamarin.com/?link=T%3aXamarin.Forms.Page) Klasse ist ein visuelles Element, die belegt die meisten oder alle Rand des Bildschirms und ein einzelnes untergeordnetes Element enthält. Ein `Xamarin.Forms.Page` stellt eine View-Controller in iOS oder eine Seite in Windows Phone. Unter Android füllt jede Seite der Bildschirm wie eine Aktivität, jedoch Xamarin.Forms Seiten *nicht* Aktivitäten.

 [ ![](pages-images/pages-sml.png "Xamarin.Forms Seitentypen")](pages-images/pages.png "Xamarin.Forms Seitentypen")

<br clear="all" />

Xamarin.Forms unterstützt:

<table align="center" border="1" cellpadding="1" cellspacing="1">
  <tr>
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
  </thead></tr>
  <tbody>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/">ContentPage</a>
    </td>
    <td align="center" valign="top">
Ein <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/">ContentPage verwendet</a> zeigt eine einzelne <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">Ansicht</a>, häufig ein Container wie z. B. eine <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/">StackLayout</a> oder ein <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/">ScrollView</a>.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/ContentPageDemoPage.cs"><img src="pages-images/ContentPage.png" title="ContentPage verwendet wird" class="tableimg">
    </a></td>
  </tr><tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/">MasterDetailPage</a>
    </td>
    <td valign="top">
Ein <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Page/">Seite</a> , die zwei Informationsbereiche verwaltet.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/MasterDetailPageDemoPage.cs"><img src="pages-images/MasterDetailPage.png" title="MasterDetailPage-Beispiel" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/">NavigationPage</a>
    </td>
    <td valign="top">
Ein <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Page/">Seite</a> , die die Navigation und benutzerfreundlichkeit einem Stapel von anderen Seiten verwaltet.  
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/NavigationPageDemoPage.cs"><img src="pages-images/NavigationPage.png" title="NavigationPage-Beispiel" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/">TabbedPage</a>
    </td>
    <td valign="top">
Ein <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Page/">Seite</a> , ermöglicht die Navigation zwischen den untergeordneten Seiten mit Registerkarten.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/TabbedPageDemoPage.cs"><img src="pages-images/TabbedPage.png" title="TabbedPage-Beispiel" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TemplatedPage/">TemplatedPage</a>
    </td>
    <td valign="top">
Ein <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Page/">Seite</a> , anzeigt Vollbild-Inhalt mit einer Steuerelementvorlage und die Basisklasse für <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/">ContentPage verwendet</a>.
    </td>
    <td valign="top">
    <a href="https://github.com/xamarin/xamarin-forms-samples/tree/master/Templates/ControlTemplates/"><img src="pages-images/TemplatedPage.png" title="TemplatedPage-Beispiel" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/">CarouselPage</a>
    </td>
    <td valign="top">
Ein <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Page/">Seite</a> Wischen Gesten zwischen Unterseiten, z. B. ein Katalog ermöglicht.
    </td>
    <td valign="top">
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CarouselPageDemoPage.cs"><img src="pages-images/CarouselPage.png" title="CarouselPage-Beispiel" class="tableimg">
    </a></td>
  </tr>
  </tbody>
</table>



## <a name="related-links"></a>Verwandte Links

- [Introduction to Xamarin.Forms (Einführung in Xamarin.Forms)](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [Xamarin.Forms-Katalog (Beispiel)](https://developer.xamarin.com/samples/FormsGallery/)
- [Xamarin.Forms-Beispiele](https://developer.xamarin.com/samples/tag/Xamarin.Forms/)
- [Xamarin.Forms-API-Dokumentation](http://iosapi.xamarin.com/?link=N%3aXamarin.Forms)
