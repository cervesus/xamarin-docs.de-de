---
title: Verwendung von XAML-Standard (Vorschau)-Steuerelemente
description: Untersuchen die Verwendung von XAML-Standard-Vorschau in Xamarin.Forms erste Schritte
ms.prod: xamarin
ms.assetid: 287E6631-D1C5-46C5-8905-AB53D34E365D
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/15/2017
ms.openlocfilehash: 2fc7fb9581f344e0d54bd9f690d334eda78cc97a
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="xaml-standard-preview-controls"></a>Verwendung von XAML-Standard (Vorschau)-Steuerelemente

![Vorschau](~/media/shared/preview.png)

Auf dieser Seite sind die Verwendung von XAML-Standardsteuerelemente sind in der Vorschau, zusammen mit ihren entsprechenden Xamarin.Forms-Steuerelement.

Es gibt auch eine Liste der Steuerelemente, die neue Eigenschaft und Enumeration Namen in XAML-Standard.

## <a name="controls"></a>Steuerelemente

|Xamarin.Forms|XAML Standard|
|--- |--- |
|Frame|Rahmen|
|Datumsauswahl|ComboBox|
|ActivityIndicator|ProgressRing|
|StackLayout|StackPanel|
|Bezeichnung|TextBlock|
|Eingabe|TextBox|
|Schalter|ToggleSwitch|
|ContentView|UserControl|


## <a name="properties-and-enumerations"></a>Eigenschaften und Enumerationen

|Xamarin.Forms-Steuerelemente mit aktualisierten Eigenschaften|Xamarin.Forms-Eigenschaft oder eine Enumerationsklasse|Verwendung von XAML-Standard entspricht.|
|--- |--- |--- |
|Schaltfläche, Eintrag, Bezeichnung, DatePicker, Editor, SearchBar, TimePicker|TextColor|Vordergrund|
|VisualElement|BackgroundColor|Hintergrund *|
|Auswahl, einer Schaltfläche|BorderColor, OutlineColor|BorderBrush|
|Schaltfläche|BorderWidth|BorderThickness|
|ProgressBar|Status|Wert|
|Schaltfläche, Eintrag, Bezeichnung, Editor, SearchBar, Spanne, Schriftart|FontAttributesBold, kursiv, keine|FontStyleItalic, Normal|
|Schaltfläche, Eintrag, Bezeichnung, Editor, SearchBar, Spanne, Schriftart|FontAttributes|FontWeights * fett, Normal|
|InputView|KeyboardDefault, Url, Anzahl, Telefon, Text, Chat, per e-Mail senden|InputScopeNameValue * Standard, Url, Anzahl, "telephoneNumber", Text, Chat, EmailNameOrAddress|
|StackPanel|StackOrientation|Ausrichtung *|

> [!IMPORTANT]
> Gekennzeichnete Elemente mit * sind in der aktuellen Vorschau unvollständig

## <a name="related-links"></a>Verwandte Links

- [Vorschau NuGet](https://aka.ms/xf-xamlstandard-nuget)
