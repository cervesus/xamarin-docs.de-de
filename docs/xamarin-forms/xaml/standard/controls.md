---
title: Verwendung von XAML-Standard (Vorschau)-Steuerelemente
description: Untersuchen die Verwendung von XAML-Standard-Vorschau in Xamarin.Forms erste Schritte
ms.topic: article
ms.prod: xamarin
ms.assetid: 287E6631-D1C5-46C5-8905-AB53D34E365D
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/15/2017
ms.openlocfilehash: 3f30a77975a9f42380ecf7efd73426763ec83ef0
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="xaml-standard-preview-controls"></a>Verwendung von XAML-Standard (Vorschau)-Steuerelemente

![Vorschau](~/media/shared/preview.png)

Auf dieser Seite sind die Verwendung von XAML-Standardsteuerelemente sind in der Vorschau, zusammen mit ihren entsprechenden Xamarin.Forms-Steuerelement.

Es gibt auch eine Liste der Steuerelemente, die neue Eigenschaft und Enumeration Namen in XAML-Standard.

## <a name="controls"></a>Steuerelemente

<table style="width:300px">
  <tr><th>Xamarin.Forms</th><th>XAML Standard</th></tr>
  <tr><td>Frame</td><td>Rahmen</td></tr>
  <tr><td>Datumsauswahl</td><td>ComboBox</td></tr>
  <tr><td>ActivityIndicator</td><td>ProgressRing</td></tr>
  <tr><td>StackLayout</td><td>StackPanel</td></tr>
  <tr><td>Bezeichnung</td><td>TextBlock</td></tr>
  <tr><td>Eingabe</td><td>TextBox</td></tr>
  <tr><td>Schalter</td><td>ToggleSwitch</td></tr>
  <tr><td>ContentView</td><td>UserControl</td></tr>
</table>

## <a name="properties-and-enumerations"></a>Eigenschaften und Enumerationen

<table>
  <tr><th>Xamarin.Forms<br/>Steuerelemente mit aktualisierten Eigenschaften</th><th>Xamarin.Forms<br/>Eigenschaft oder eine Enumerationsklasse</th><th>XAML Standard<br/>Entsprechung</th></tr>
  <tr><td>Schaltfläche, Eintrag, Bezeichnung, DatePicker, Editor, SearchBar, TimePicker</td><td>TextColor</td><td>Vordergrund</td></tr>
  <tr><td>VisualElement</td><td>BackgroundColor</td><td><i>Hintergrund *</i></td></tr>
  <tr><td>Auswahl, einer Schaltfläche</td><td>BorderColor, OutlineColor</td><td>BorderBrush</td></tr>
  <tr><td>Schaltfläche</td><td>BorderWidth</td><td>BorderThickness</td></tr>
  <tr><td>ProgressBar</td><td>Status</td><td>Wert</td></tr>
  <tr><td>Schaltfläche, Eintrag, Bezeichnung, Editor, SearchBar, Spanne, Schriftart</td><td>FontAttributes<br/>Fett, kursiv, keine</td><td>FontStyle<br/>Kursiv – Normal</td></tr>
  <tr><td>Schaltfläche, Eintrag, Bezeichnung, Editor, SearchBar, Spanne, Schriftart</td><td>FontAttributes</td><td><i>FontWeights *</i><br/>Fett, Normal</td></tr>
  <tr><td>InputView</td><td>Tastatur<br/>Standard-Url, Anzahl, Telefon, Text, Chat, E-Mail</td><td><i>InputScopeNameValue *</i><br/>Standard-Url, Anzahl, "telephoneNumber", Text, Chat, EmailNameOrAddress</td></tr>
  <tr><td>StackPanel</td><td>StackOrientation</td><td><i>Ausrichtung *</i></td></tr>
</table>

> [!IMPORTANT]
> Gekennzeichnete Elemente mit * sind in der aktuellen Vorschau unvollständig


## <a name="related-links"></a>Verwandte Links

- [Vorschau NuGet](https://aka.ms/xf-xamlstandard-nuget)
