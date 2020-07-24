---
title: :::no-loc(Xamarin.Forms):::Schlä
description: ':::no-loc(Xamarin.Forms):::Zellen können ListViews und tableviews hinzugefügt werden. In diesem Artikel werden die in enthaltenen Zellen aufgeführt :::no-loc(Xamarin.Forms)::: .'
ms.prod: xamarin
ms.assetid: 77DA0C89-35D6-4C09-A072-3ADE53FD56CF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/12/2016
no-loc:
- ':::no-loc(Xamarin.Forms):::'
- ':::no-loc(Xamarin.Essentials):::'
ms.openlocfilehash: 21fca31ca9e1cf39843e04c3c381bb41ef77335f
ms.sourcegitcommit: 952db1983c0bc373844c5fbe9d185e04a87d8fb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "86997279"
---
# <a name="no-locxamarinforms-cells"></a>:::no-loc(Xamarin.Forms):::Schlä

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)

_:::no-loc(Xamarin.Forms):::Zellen können ListViews und tableviews hinzugefügt werden._

Eine *Zelle* ist ein spezielles Element, das für Elemente in einer Tabelle verwendet wird, und beschreibt, wie jedes Element in einer Liste gerendert werden soll. Die- [`Cell`](xref::::no-loc(Xamarin.Forms):::.Cell) Klasse wird von abgeleitet [`Element`](xref::::no-loc(Xamarin.Forms):::.Element) , von der [`VisualElement`](xref::::no-loc(Xamarin.Forms):::.Element) ebenfalls abgeleitet ist. Eine Zelle ist nicht selbst ein visuelles Element. Es handelt sich vielmehr um eine Vorlage zum Erstellen eines visuellen Elements.

`Cell`wird ausschließlich mit [`ListView`](xref::::no-loc(Xamarin.Forms):::.ListView) -und-Steuer [`TableView`](xref::::no-loc(Xamarin.Forms):::.TableView) Elementen verwendet. Informationen zum verwenden und Anpassen von Zellen finden Sie in der [`ListView`](~/xamarin-forms/user-interface/listview/index.md) Dokumentation zu und [`TableView`](~/xamarin-forms/user-interface/tableview.md) .

## <a name="cells"></a>Zellen

:::no-loc(Xamarin.Forms):::unterstützt die folgenden Zelltypen:

| Typ | Beschreibung | Darstellung |
| --- | --- | --- |
| `TextCell` | Eine [`TextCell`](xref::::no-loc(Xamarin.Forms):::.TextCell) zeigt eine oder zwei Text Zeichenfolgen an. Legen [`Text`](xref::::no-loc(Xamarin.Forms):::.TextCell.Text) Sie die-Eigenschaft und optional die- [`Detail`](xref::::no-loc(Xamarin.Forms):::.TextCell.Detail) Eigenschaft auf diese Text Zeichenfolgen fest.<br /><br />[API-Dokumentation](xref::::no-loc(Xamarin.Forms):::.TextCell)  /  [Leitfaden](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#textcell) | [![Textcell-Beispiel](cells-images/TextCell.png "Textcell-Beispiel")](cells-images/TextCell-Large.png#lightbox "Textcell-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TextCellDemoPage.cs)  /  [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TextCellDemoPage.xaml) |
| `ImageCell` | [`ImageCell`](xref::::no-loc(Xamarin.Forms):::.ImageCell)Zeigt die gleichen Informationen wie [`TextCell`](xref::::no-loc(Xamarin.Forms):::.TextCell) an, enthält jedoch eine Bitmap, die Sie mit der- [`Source`](xref::::no-loc(Xamarin.Forms):::.Image.Source) Eigenschaft festlegen.<br /><br />[API-Dokumentation](xref::::no-loc(Xamarin.Forms):::.ImageCell)  /  [Leitfaden](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#imagecell) | [![ImageCell-Beispiel](cells-images/ImageCell.png "ImageCell-Beispiel")](cells-images/ImageCell-Large.png#lightbox "ImageCell-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ImageCellDemoPage.cs)  /  [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ImageCellDemoPage.xaml) |
| `SwitchCell` | Der [`SwitchCell`](xref::::no-loc(Xamarin.Forms):::.SwitchCell) enthält Textsatz mit der [`Text`](xref::::no-loc(Xamarin.Forms):::.SwitchCell.Text) -Eigenschaft und einem on/off-Switch, der anfänglich mit der booleschen Eigenschaft festgelegt wurde [`On`](xref::::no-loc(Xamarin.Forms):::.SwitchCell.On) . Behandeln [`OnChanged`](xref::::no-loc(Xamarin.Forms):::.SwitchCell.OnChanged) Sie das Ereignis, das benachrichtigt werden soll, wenn sich die `On` Eigenschaft ändert.<br /><br />[API-Dokumentation](xref::::no-loc(Xamarin.Forms):::.SwitchCell)  /  [Leitfaden](~/xamarin-forms/user-interface/tableview.md#switchcell) | [![Switchcell-Beispiel](cells-images/SwitchCell.png "Switchcell-Beispiel")](cells-images/SwitchCell-Large.png#lightbox "Switchcell-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SwitchCellDemoPage.cs)  /  [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SwitchCellDemoPage.xaml) |
| `EntryCell` | [`EntryCell`](xref::::no-loc(Xamarin.Forms):::.EntryCell)Definiert eine [`Label`](xref::::no-loc(Xamarin.Forms):::.EntryCell.Label) Eigenschaft, die die Zelle und eine einzelne Zeile mit bearbeitbaren Text in der- [`Text`](xref::::no-loc(Xamarin.Forms):::.EntryCell.Text) Eigenschaft bezeichnet. Behandeln [`Completed`](xref::::no-loc(Xamarin.Forms):::.EntryCell.Completed) Sie das Ereignis, das benachrichtigt werden soll, wenn der Benutzer den Text Eintrag abgeschlossen hat.<br /><br />[API-Dokumentation](xref::::no-loc(Xamarin.Forms):::.EntryCell)  /  [Leitfaden](~/xamarin-forms/user-interface/tableview.md#entrycell) | [![Entrycell-Beispiel](cells-images/EntryCell.png "Entrycell-Beispiel")](cells-images/EntryCell-Large.png#lightbox "Entrycell-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/EntryCellDemoPage.cs)  /  [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/EntryCellDemoPage.xaml) |
| | | |

## <a name="related-links"></a>Verwandte Links

- [:::no-loc(Xamarin.Forms):::Formsgallery-Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)
- [:::no-loc(Xamarin.Forms):::-Beispiele](https://docs.microsoft.com/samples/browse/?products=xamarin&term=:::no-loc(Xamarin.Forms):::)
- [:::no-loc(Xamarin.Forms):::API-Dokumentation](https://docs.microsoft.com/dotnet/api/xamarin.forms?view=xamarin-forms)
