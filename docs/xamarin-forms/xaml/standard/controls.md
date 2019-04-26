---
title: Steuerelemente für XAML-Standard (Vorschau)
description: In diesem Artikel untersucht die XAML Standardsteuerelemente in Xamarin.Forms verfügbar ist.
ms.prod: xamarin
ms.assetid: 287E6631-D1C5-46C5-8905-AB53D34E365D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/15/2017
ms.openlocfilehash: b9bf0e1ba14f4e8584bfd8492776ac7c8668df87
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61175177"
---
# <a name="xaml-standard-preview-controls"></a>Steuerelemente für XAML-Standard (Vorschau)

![Vorschau](~/media/shared/preview.png)

Diese Seite listet die XAML Standardsteuerelemente sind in der Vorschau, zusammen mit ihren entsprechenden Xamarin.Forms-Steuerelements.

Es gibt auch eine Liste der Steuerelemente, die neue Namen von Eigenschaften und -Enumeration in XAML-Standard aufweisen.

## <a name="controls"></a>Steuerelemente

|Xamarin.Forms|XAML Standard|
|--- |--- |
|Frame|Rahmen|
|Auswahl|ComboBox|
|ActivityIndicator|ProgressRing|
|StackLayout|StackPanel|
|Bezeichnung|TextBlock|
|Eingabe|TextBox|
|Schalter|ToggleSwitch|
|ContentView|UserControl|


## <a name="properties-and-enumerations"></a>Eigenschaften und Enumerationen

|Xamarin.Forms-Steuerelemente mit aktualisierten Eigenschaften|Xamarin.Forms-Eigenschaft oder eine Enumerationsklasse|XAML-Standard entspricht.|
|--- |--- |--- |
|Button, Entry, Label, DatePicker, Editor, SearchBar, TimePicker|TextColor|Vordergrund|
|VisualElement|BackgroundColor|Hintergrund *|
|Auswahl, Schaltfläche|BorderColor, OutlineColor|BorderBrush|
|Schaltfläche|BorderWidth|BorderThickness|
|ProgressBar|Status|Wert|
|Schaltfläche ", Eintrag, Label, -Editor, SearchBar, Spanne, Schriftart|FontAttributesBold, Italic, None|FontStyleItalic, Normal|
|Schaltfläche ", Eintrag, Label, -Editor, SearchBar, Spanne, Schriftart|FontAttributes|FontWeights * fett, Normal|
|InputView|KeyboardDefault, Url, Number, Telephone, Text, Chat, Email|InputScopeNameValue *Default, Url, Number, TelephoneNumber, Text, Chat, EmailNameOrAddress|
|StackPanel|StackOrientation|Ausrichtung *|

> [!IMPORTANT]
> Gekennzeichnete Elemente mit * sind nicht vollständig in der aktuellen Vorschau

## <a name="related-links"></a>Verwandte Links

- [Vorschau NuGet](https://aka.ms/xf-xamlstandard-nuget)
