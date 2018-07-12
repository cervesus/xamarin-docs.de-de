---
title: Steuerelemente für XAML-Standard (Vorschau)
description: In diesem Artikel untersucht die XAML Standardsteuerelemente in Xamarin.Forms verfügbar ist.
ms.prod: xamarin
ms.assetid: 287E6631-D1C5-46C5-8905-AB53D34E365D
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/15/2017
ms.openlocfilehash: 1b01d0773f0c2150db575875b770957eb6452f41
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2018
ms.locfileid: "38843942"
---
# <a name="xaml-standard-preview-controls"></a>Steuerelemente für XAML-Standard (Vorschau)

![Vorschau](~/media/shared/preview.png)

Diese Seite listet die XAML Standardsteuerelemente sind in der Vorschau, zusammen mit ihren entsprechenden Xamarin.Forms-Steuerelements.

Es gibt auch eine Liste der Steuerelemente, die neue Namen von Eigenschaften und -Enumeration in XAML-Standard aufweisen.

## <a name="controls"></a>Steuerelemente

|Xamarin.Forms|XAML-Standard|
|--- |--- |
|Frame|Rahmen|
|Zeitauswahl|ComboBox|
|ActivityIndicator|ProgressRing|
|StackLayout|StackPanel|
|Bezeichnung|TextBlock|
|Eingabe|TextBox|
|Schalter|ToggleSwitch|
|ContentView|UserControl|


## <a name="properties-and-enumerations"></a>Eigenschaften und Enumerationen

|Xamarin.Forms-Steuerelemente mit aktualisierten Eigenschaften|Xamarin.Forms-Eigenschaft oder eine Enumerationsklasse|XAML-Standard entspricht.|
|--- |--- |--- |
|Schaltfläche ", Eintrag, Label," DatePicker ", Editor, SearchBar, TimePicker|TextColor|Vordergrund|
|VisualElement|BackgroundColor|Hintergrund *|
|Auswahl, Schaltfläche|BorderColor, OutlineColor|BorderBrush|
|Schaltfläche|BorderWidth|BorderThickness|
|ProgressBar|Status|Wert|
|Schaltfläche ", Eintrag, Label, -Editor, SearchBar, Spanne, Schriftart|FontAttributesBold, kursiv, keine|FontStyleItalic, Normal|
|Schaltfläche ", Eintrag, Label, -Editor, SearchBar, Spanne, Schriftart|FontAttributes|FontWeights * fett, Normal|
|InputView|KeyboardDefault "," Url "," Number "," Telefon "," Text-Chat, per E-Mail|InputScopeNameValue * Standard-Url, Anzahl, "telephoneNumber", Text, Chat, EmailNameOrAddress|
|StackPanel|StackOrientation|Ausrichtung *|

> [!IMPORTANT]
> Gekennzeichnete Elemente mit * sind nicht vollständig in der aktuellen Vorschau

## <a name="related-links"></a>Verwandte Links

- [Vorschau NuGet](https://aka.ms/xf-xamlstandard-nuget)
