---
Title: " Xamarin.Forms Auswahl" Beschreibung: "die Xamarin.Forms Auswahl zeigt eine kurze Liste von Elementen an, aus der der Benutzer ein Element auswählen kann. In diesem Artikel wird erläutert, wie Sie die Auswahl Klasse verwenden, um ein Textelement aus einer Liste von Daten auszuwählen.
ms. Prod: xamarin ms. assetid: D4815A4B-104B-4294-951B-BD8F2EC33C86 ms. Technology: xamarin-Forms Author: davidbritch ms. Author: dabritch ms. Date: 02/26/2019 NO-LOC: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="xamarinforms-picker"></a>Xamarin.FormsBild

_Die Auswahl Ansicht ist ein Steuerelement zum Auswählen eines Text Elements aus einer Datenliste._

Das Xamarin.Forms [`Picker`](xref:Xamarin.Forms.Picker) zeigt eine kurze Liste von Elementen an, aus der der Benutzer ein Element auswählen kann. `Picker` definiert die folgenden Eigenschaften:

- [`Title`](xref:Xamarin.Forms.Picker.Title)vom Typ `string` , der standardmäßig auf festgelegt ist `null` .
- `TitleColor`vom Typ [`Color`](xref:Xamarin.Forms.Color) , die Farbe, die zum Anzeigen des Texts verwendet wird `Title` .
- [`ItemsSource`](xref:Xamarin.Forms.Picker.ItemsSource)vom Typ `IList` , die Quell Liste der anzuzeigenden Elemente, deren Standard ist `null` .
- [`SelectedIndex`](xref:Xamarin.Forms.Picker.SelectedIndex)vom Typ `int` , der Index des ausgewählten Elements, dessen Standardwert-1 ist.
- [`SelectedItem`](xref:Xamarin.Forms.Picker.SelectedItem)vom Typ `object` , das ausgewählte Element, dessen Standard ist `null` .
- [`TextColor`](xref:Xamarin.Forms.Picker.TextColor)vom Typ [`Color`](xref:Xamarin.Forms.Color) : die Farbe, die zum Anzeigen des Texts verwendet wird, dessen Standard ist [`Color.Default`](xref:Xamarin.Forms.Color.Default) .
- [`FontAttributes`](xref:Xamarin.Forms.Picker.FontAttributes)vom Typ [`FontAttributes`](xref:Xamarin.Forms.FontAttributes) , der standardmäßig auf festgelegt ist [`FontAtributes.None`](xref:Xamarin.Forms.FontAttributes.None) .
- [`FontFamily`](xref:Xamarin.Forms.Picker.FontFamily)vom Typ `string` , der standardmäßig auf festgelegt ist `null` .
- [`FontSize`](xref:Xamarin.Forms.Picker.FontSize)vom Typ `double` , der standardmäßig auf-1,0 festgelegt ist.
- `CharacterSpacing`ist vom Typ `double` , der Abstand zwischen den Zeichen des Elements, das von angezeigt wird `Picker` .

Alle Eigenschaften werden von Objekten unterstützt [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . Dies bedeutet, dass Sie formatiert werden können, und die Eigenschaften können Ziele von Daten Bindungen sein. Die [`SelectedIndex`](xref:Xamarin.Forms.Picker.SelectedIndex) -Eigenschaft und die-Eigenschaft [`SelectedItem`](xref:Xamarin.Forms.Picker.SelectedItem) verfügen über einen Standard Bindungs Modus von [`BindingMode.TwoWay`](xref:Xamarin.Forms.BindingMode.TwoWay) . Dies bedeutet, dass Sie Ziele von Daten Bindungen in einer Anwendung sein können, die die [Model-View-ViewModel (MVVM)-](~/xamarin-forms/enterprise-application-patterns/mvvm.md) Architektur verwendet. Weitere Informationen zum Festlegen von Schriftart Eigenschaften finden Sie unter [Schriftarten](~/xamarin-forms/user-interface/text/fonts.md).

Ein [`Picker`](xref:Xamarin.Forms.Picker) zeigt beim ersten anzeigen keine Daten an. Stattdessen wird der Wert seiner- [`Title`](xref:Xamarin.Forms.Picker.Title) Eigenschaft als Platzhalter auf den IOS-und Android-Plattformen angezeigt:

[![](images/picker-initial.png "Initial Picker Display")](images/picker-initial-large.png#lightbox "Initial Picker Display")

Wenn der [`Picker`](xref:Xamarin.Forms.Picker) Fokus erreicht wird, werden seine Daten angezeigt, und der Benutzer kann ein Element auswählen:

[![](images/picker-selection.png "Picker Selecting an Item")](images/picker-selection-large.png#lightbox "Picker Selecting an Item")

Das löst ein-Ereignis aus, [`Picker`](xref:Xamarin.Forms.Picker) [`SelectedIndexChanged`](xref:Xamarin.Forms.Picker.SelectedIndexChanged) Wenn der Benutzer ein Element auswählt. Nach der Auswahl wird das ausgewählte Element von angezeigt `Picker` :

![](images/picker-after-selection.png "Picker after Selection")

Es gibt zwei Verfahren zum Auffüllen eines [`Picker`](xref:Xamarin.Forms.Picker) mit Daten:

- Festlegen der- [`ItemsSource`](xref:Xamarin.Forms.Picker.ItemsSource) Eigenschaft auf die anzuzeigenden Daten. Dies ist das empfohlene Verfahren. Weitere Informationen finden Sie unter [Festlegen der ItemsSource-Eigenschaft einer](populating-itemssource.md)Auswahl.
- Hinzufügen der Daten, die der Auflistung angezeigt werden sollen [`Items`](xref:Xamarin.Forms.Picker.Items) . Diese Technik war der ursprüngliche Vorgang zum Auffüllen eines [`Picker`](xref:Xamarin.Forms.Picker) mit Daten. Weitere Informationen finden Sie unter [Hinzufügen von Daten zur Element Auflistung einer Auswahl](populating-items.md).

## <a name="related-links"></a>Verwandte Links

- [Auswahl](xref:Xamarin.Forms.Picker)
