---
title: Xamarin.Forms-Zellen
description: Xamarin.Forms-Zellen können durch ListViews und TableViews hinzugefügt werden. Dieser Artikel beschreibt die Zellen in Xamarin.Forms enthalten.
ms.prod: xamarin
ms.assetid: 77DA0C89-35D6-4C09-A072-3ADE53FD56CF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/12/2016
ms.openlocfilehash: e8f3d8af7aab2a7a73787021f114470726d74b72
ms.sourcegitcommit: d0e6436edbf7c52d760027d5e0ccaba2531d9fef
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75488087"
---
# <a name="xamarinforms-cells"></a>Xamarin.Forms-Zellen

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)

_Xamarin.Forms-Zellen können durch ListViews und TableViews hinzugefügt werden._

Ein *Zelle* ist eine spezielle Element für Elemente in einer Tabelle verwendet, und beschreibt, wie jedes Element in einer Liste gerendert werden soll. Die [ `Cell` ](xref:Xamarin.Forms.Cell) Klasse leitet sich von [ `Element` ](xref:Xamarin.Forms.Element), von dem [ `VisualElement` ](xref:Xamarin.Forms.Element) auch abgeleitet wird. Eine Zelle ist nicht selbst ein visuelles Element; Es wird stattdessen eine Vorlage für ein visuelles Element erstellen.

`Cell` wird verwendet, ausschließlich mit [ `ListView` ](views.md#listview) und [ `TableView` ](views.md#tableview) Steuerelemente. Weitere Informationen zum verwenden und Anpassen von Zellen, finden Sie in der [ `ListView` ](~/xamarin-forms/user-interface/listview/index.md) und [ `TableView` ](~/xamarin-forms/user-interface/tableview.md) Dokumentation.

## <a name="cells"></a>Zellen

Xamarin.Forms unterstützt die folgenden Zellentypen:

<a name="textCell" />

### <a name="textcell"></a>TextCell

|     |     |
| --- | --- |
| Ein [ `TextCell` ](xref:Xamarin.Forms.TextCell) zeigt ein oder zwei Textzeichenfolgen. Legen Sie die [ `Text` ](xref:Xamarin.Forms.TextCell.Text) Eigenschaft und optional die [ `Detail` ](xref:Xamarin.Forms.TextCell.Detail) Eigenschaft, um diese Textzeichenfolgen.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.TextCell) / [Handbuch](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#textcell) | [![Textcell-Beispiel](cells-images/TextCell.png "Textcell-Beispiel")](cells-images/TextCell-Large.png#lightbox "Textcell-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TextCellDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TextCellDemoPage.xaml) |
|     |     |

### <a name="imagecell"></a>ImageCell

|     |     |
| --- | --- |
| Die [ `ImageCell` ](xref:Xamarin.Forms.ImageCell) zeigt dieselbe Informationen wie [ `TextCell` ](#textCell) enthält eine Bitmap, die Sie eingerichtet, aber die [ `Source` ](xref:Xamarin.Forms.Image.Source) Eigenschaft.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.ImageCell) / [Handbuch](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#imagecell) | [![ImageCell-Beispiel](cells-images/ImageCell.png "ImageCell-Beispiel")](cells-images/ImageCell-Large.png#lightbox "ImageCell-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ImageCellDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ImageCellDemoPage.xaml) |
|     |     |

### <a name="switchcell"></a>SwitchCell

|     |     |
| --- | --- |
| Der [`SwitchCell`](xref:Xamarin.Forms.SwitchCell) enthält einen Textsatz mit der [`Text`](xref:Xamarin.Forms.SwitchCell.Text) -Eigenschaft und einen on/off-Switch, der anfänglich mit der booleschen [`On`](xref:Xamarin.Forms.SwitchCell.On) -Eigenschaft festgelegt wurde. Behandeln der [ `OnChanged` ](xref:Xamarin.Forms.SwitchCell.OnChanged) Ereignis, wann die `On` eigenschaftenänderungen.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.SwitchCell) / [Handbuch](~/xamarin-forms/user-interface/tableview.md#switchcell) | [![Switchcell-Beispiel](cells-images/SwitchCell.png "Switchcell-Beispiel")](cells-images/SwitchCell-Large.png#lightbox "Switchcell-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SwitchCellDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SwitchCellDemoPage.xaml) |
|     |     |

### <a name="entrycell"></a>EntryCell

|     |     |
| --- | --- |
| Die [ `EntryCell` ](xref:Xamarin.Forms.EntryCell) definiert eine [ `Label` ](xref:Xamarin.Forms.EntryCell.Label) , die die Zelle und bearbeitbaren Text in eine einzige Zeile identifiziert die [ `Text` ](xref:Xamarin.Forms.EntryCell.Text) Eigenschaft. Behandeln der [ `Completed` ](xref:Xamarin.Forms.EntryCell.Completed) Ereignis benachrichtigt werden, wenn der Benutzer die Texteingabe abgeschlossen hat.<br /><br />[API-Dokumentation](xref:Xamarin.Forms.EntryCell) / [Handbuch](~/xamarin-forms/user-interface/tableview.md#entrycell) | [![Entrycell-Beispiel](cells-images/EntryCell.png "Entrycell-Beispiel")](cells-images/EntryCell-Large.png#lightbox "Entrycell-Beispiel")<br />[C#-Code für diese Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/EntryCellDemoPage.cs) / [XAML-Seite](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/EntryCellDemoPage.xaml) |
|     |     |

## <a name="related-links"></a>Verwandte Themen

- [Beispiel für Xamarin.Forms FormsGallery](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)
- [Xamarin.Forms-Beispiele](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.Forms)
- [Xamarin.Forms-API-Dokumentation](https://docs.microsoft.com/dotnet/api/xamarin.forms?view=xamarin-forms)
