---
title: Xamarin.FormsSchlä
description: Xamarin.FormsZellen können ListViews und tableviews hinzugefügt werden. In diesem Artikel werden die in enthaltenen Zellen aufgeführt Xamarin.Forms .
ms.prod: xamarin
ms.assetid: 77DA0C89-35D6-4C09-A072-3ADE53FD56CF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/12/2016
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: fff62aea5a20a8a14271123c4664c2c0b4e26d1e
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84573325"
---
# <a name="xamarinforms-cells"></a>Xamarin.FormsSchlä

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)

_Xamarin. Forms-Zellen können ListViews und tableviews hinzugefügt werden._

Eine *Zelle* ist ein spezielles Element, das für Elemente in einer Tabelle verwendet wird, und beschreibt, wie jedes Element in einer Liste gerendert werden soll. Die- [`Cell`](xref:Xamarin.Forms.Cell) Klasse wird von abgeleitet [`Element`](xref:Xamarin.Forms.Element) , von der [`VisualElement`](xref:Xamarin.Forms.Element) ebenfalls abgeleitet ist. Eine Zelle ist nicht selbst ein visuelles Element. Es handelt sich vielmehr um eine Vorlage zum Erstellen eines visuellen Elements.

`Cell`wird ausschließlich mit [`ListView`](views.md#listview) -und-Steuer [`TableView`](views.md#tableview) Elementen verwendet. Informationen zum verwenden und Anpassen von Zellen finden Sie in der [`ListView`](~/xamarin-forms/user-interface/listview/index.md) Dokumentation zu und [`TableView`](~/xamarin-forms/user-interface/tableview.md) .

## <a name="cells"></a>Zellen

Xamarin.Formsunterstützt die folgenden Zelltypen:

### <a name="textcell"></a>Textcell

|     |     |
| --- | --- |
| Eine [`TextCell`](xref:Xamarin.Forms.TextCell) zeigt eine oder zwei Text Zeichenfolgen an. Legen [`Text`](xref:Xamarin.Forms.TextCell.Text) Sie die-Eigenschaft und optional die- [`Detail`](xref:Xamarin.Forms.TextCell.Detail) Eigenschaft auf diese Text Zeichenfolgen fest.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.TextCell)  /  [Leitfaden](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#textcell) | [![Textcell-Beispiel](cells-images/TextCell.png "Textcell-Beispiel")](cells-images/TextCell-Large.png#lightbox "Textcell-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TextCellDemoPage.cs)  /  [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TextCellDemoPage.xaml) |
|     |     |

### <a name="imagecell"></a>ImageCell

|     |     |
| --- | --- |
| [`ImageCell`](xref:Xamarin.Forms.ImageCell)Zeigt die gleichen Informationen wie [`TextCell`](#textcell) an, enthält jedoch eine Bitmap, die Sie mit der- [`Source`](xref:Xamarin.Forms.Image.Source) Eigenschaft festlegen.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.ImageCell)  /  [Leitfaden](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#imagecell) | [![ImageCell-Beispiel](cells-images/ImageCell.png "ImageCell-Beispiel")](cells-images/ImageCell-Large.png#lightbox "ImageCell-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ImageCellDemoPage.cs)  /  [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ImageCellDemoPage.xaml) |
|     |     |

### <a name="switchcell"></a>Switchcell

|     |     |
| --- | --- |
| Der [`SwitchCell`](xref:Xamarin.Forms.SwitchCell) enthält Textsatz mit der [`Text`](xref:Xamarin.Forms.SwitchCell.Text) -Eigenschaft und einem on/off-Switch, der anfänglich mit der booleschen Eigenschaft festgelegt wurde [`On`](xref:Xamarin.Forms.SwitchCell.On) . Behandeln [`OnChanged`](xref:Xamarin.Forms.SwitchCell.OnChanged) Sie das Ereignis, das benachrichtigt werden soll, wenn sich die `On` Eigenschaft ändert.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.SwitchCell)  /  [Leitfaden](~/xamarin-forms/user-interface/tableview.md#switchcell) | [![Switchcell-Beispiel](cells-images/SwitchCell.png "Switchcell-Beispiel")](cells-images/SwitchCell-Large.png#lightbox "Switchcell-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SwitchCellDemoPage.cs)  /  [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SwitchCellDemoPage.xaml) |
|     |     |

### <a name="entrycell"></a>Entrycell

|     |     |
| --- | --- |
| [`EntryCell`](xref:Xamarin.Forms.EntryCell)Definiert eine [`Label`](xref:Xamarin.Forms.EntryCell.Label) Eigenschaft, die die Zelle und eine einzelne Zeile mit bearbeitbaren Text in der- [`Text`](xref:Xamarin.Forms.EntryCell.Text) Eigenschaft bezeichnet. Behandeln [`Completed`](xref:Xamarin.Forms.EntryCell.Completed) Sie das Ereignis, das benachrichtigt werden soll, wenn der Benutzer den Text Eintrag abgeschlossen hat.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.EntryCell)  /  [Leitfaden](~/xamarin-forms/user-interface/tableview.md#entrycell) | [![Entrycell-Beispiel](cells-images/EntryCell.png "Entrycell-Beispiel")](cells-images/EntryCell-Large.png#lightbox "Entrycell-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/EntryCellDemoPage.cs)  /  [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/EntryCellDemoPage.xaml) |
|     |     |

## <a name="related-links"></a>Verwandte Links

- [Xamarin.FormsFormsgallery-Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)
- [Xamarin.Forms-Beispiele](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.Forms)
- [Xamarin.FormsAPI-Dokumentation](https://docs.microsoft.com/dotnet/api/xamarin.forms?view=xamarin-forms)
