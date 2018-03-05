---
title: Xamarin.Forms Cells
description: "Xamarin.Forms Zellen können Listenansichten und TableViews hinzugefügt werden."
ms.topic: article
ms.prod: xamarin
ms.assetid: F2A02DEE-7137-42F4-9C0A-4E1CF75EA08F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/12/2016
ms.openlocfilehash: 509ecc509754bba544115c140e619f634bd64eae
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2018
---
# <a name="xamarinforms-cells"></a>Xamarin.Forms Cells

_Xamarin.Forms Zellen können Listenansichten und TableViews hinzugefügt werden._

<style>.tableimg { max-width: none !important;}</style>

## <a name="cells"></a>Zellen

Eine Zelle ist eine spezielle Element für Elemente in einer Tabelle verwendet, und es wird beschrieben, wie jedes Element in einer Liste gezeichnet werden soll. Zelle abgeleitet [ `Element` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Element/), aus der VisualElement auch abgeleitet. Obwohl Zelle nicht um ein visuelles Element ist, wird nur eine Vorlage zum Erstellen eines visuellen Elements beschrieben. [`Cell`](https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/) bietet Funktionen und die Basisklasse für alle Xamarin.Forms Zellen an. Zellen werden Elemente hinzugefügt werden sollen [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) oder [ `TableView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/) Steuerelemente.

Weitere Informationen zum verwenden und Anpassen von Zellen, finden Sie in der [ListView](~/xamarin-forms/user-interface/listview/index.md) und [TableView](~/xamarin-forms/user-interface/tableview.md) Dokumentation.

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
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.EntryCell/">EntryCell</a>
    </td>
    <td valign="top">
Ein <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/">Xamarin.Forms.Cell</a> mit einer Bezeichnung und eine einzelne Zeile Texteingabefeld angezeigt.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/EntryDemoPage.cs"><img src="cells-images/EntryCell.png" title="EntryCell-Beispiel" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.SwitchCell/">SwitchCell</a>
    </td>
    <td valign="top">
Ein <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/">Xamarin.Forms.Cell</a> mit einer Bezeichnung und ein-/Ausschalter.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/SwitchCellDemoPage.cs"><img src="cells-images/SwitchCell.png" title="SwitchCell-Beispiel" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TextCell/">TextCell</a>
    </td>
    <td valign="top">
Ein <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/">Xamarin.Forms.Cell</a> mit primären und sekundären Text.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/TextCellDemoPage.cs"><img src="cells-images/TextCell.png" title="TextCell-Beispiel" class="tableimg">
    </a></td>
  </tr>
      <tr>
    <td>
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ImageCell/">ImageCell</a>
    </td>
    <td valign="top">
Ein <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TextCell/">Textzelle</a> , auch ein Bild enthält.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/ImageCellDemoPage.cs"><img src="cells-images/ImageCell.png" title="ImageCell-Beispiel" class="tableimg">
    </a></td>
  </tr>
  </tbody>
</table>



## <a name="related-links"></a>Verwandte Links

- [Introduction to Xamarin.Forms (Einführung in Xamarin.Forms)](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [Xamarin.Forms-Katalog (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/FormsGallery/)
- [Xamarin.Forms-Beispiele](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [Xamarin.Forms-API-Dokumentation](https://developer.xamarin.com/api/namespace/Xamarin.Forms/)
