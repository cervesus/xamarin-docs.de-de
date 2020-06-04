---
title: ''
description: ''
Creating Mobile Apps with Xamarin.Forms: Summary of Chapter 5. Dealing with sizes''
ms.prod: ''
ms.technology: ''
ms.assetid: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 37403cfe9f37972c20fb074db5e30cc54b60fea9
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2020
ms.locfileid: "84136876"
---
# <a name="summary-of-chapter-5-dealing-with-sizes"></a>Zusammenfassung von Kapitel 5: Umgang mit Größen

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05)

> [!NOTE]
> In den Anmerkungen auf dieser Seite wird beschrieben, inwiefern die Angaben innerhalb des Buchs heute nicht mehr für Xamarin.Forms gelten.

In Xamarin.Forms wurden bisher mehrere Größen verwendet:

- Die Höhe der iOS-Statusleiste ist 20.
- `BoxView` hat eine Standardbreite und -höhe von 40.
- Der Standardwert für `Padding` in einem `Frame` ist 20.
- Der Standardwert für `Spacing` bei `StackLayout` ist 6.
- Die Methode `Device.GetNamedSize` gibt einen numerischen Schriftgrad zurück.

Bei diesen Größen handelt es sich nicht um Pixel. Es handelt sich dabei vielmehr um geräteunabhängige Einheiten, die von jeder Plattform unabhängig erkannt werden.

## <a name="pixels-points-dps-dips-and-dius"></a>Pixel, Punkte, dp, DIP und geräteunabhängige Einheiten

In den Anfängen von Apple Mac und Microsoft Windows arbeiteten Programmierer mit Pixeln als Einheit. Als Displays mit höherer Auflösung auf den Markt kamen, wurde allerdings eine virtualisiertere und abstraktere Herangehensweise für Bildschirmkoordinaten erforderlich. In der Mac-Welt arbeiteten Programmierer mit *Punkten* als Einheit, die traditionell 1/72 Zoll entsprach, während Windows-Entwickler *geräteunabhängige Einheiten* verwendeten, die auf 1/96 Zoll basierten.

Mobile Geräte werden allerdings allgemein deutlich näher an das Gesicht gehalten und haben eine höhere Auflösung als Desktopbildschirme, womit einhergeht, dass eine höhere Pixeldichte möglich ist.

Programmierer, die für die Geräte Apple iPhone und iPad programmieren, arbeiten weiterhin mit *Punkten* als Einheit. Allerdings entsprechen nun 160 dieser Punkte einem Zoll. Je nach Gerät kann ein Punkt 1, 2 oder 3 Pixeln entsprechen.

Bei Android verhält es sich ähnlich. Programmierer arbeiten mit *dichteunabhängigen Pixeln* (dp, density-independent pixels) als Einheit, wobei das Verhältnis von dp zu Pixeln auf einem Verhältnis von 160 dp pro Zoll basiert.

Für Windows Phones und mobile Windows-Geräte haben sich ebenfalls Skalierungsfaktoren etabliert, die ca. 160 geräteunabhängigen Einheiten pro Zoll entsprechen.

> [!NOTE]
> Xamarin.Forms unterstützt keine Windows-basierten Smartphones und mobilen Geräte mehr.

Kurz gesagt: Xamarin.Forms-Programmierer, die für Smartphones und Tablets programmieren, können davon ausgehen, dass alle Maßeinheiten auf dem folgenden Kriterium basieren:

- 160 Einheiten pro Zoll, entspricht
- 64 Einheiten pro Zentimeter

Die von `VisualElement` definierten schreibgeschützten Eigenschaften [`Width`](xref:Xamarin.Forms.VisualElement.Width) und [`Height`](xref:Xamarin.Forms.VisualElement.Height) haben standardmäßige „Pseudowerte“ von &ndash; 1. Nur wenn ein Element im Layout berücksichtigt und dessen Größe dort festgelegt ist, spiegeln diese Eigenschaften die tatsächliche Größe des Elements in geräteunabhängigen Einheiten wider. Diese Größe beinhaltet Werte für `Padding`, die für das Element festgelegt wurden, aber nicht den Wert für `Margin`.

Ein visuelles Objekt löst das [`SizeChanged`](xref:Xamarin.Forms.VisualElement.SizeChanged)-Ereignis aus, wenn sein Wert für `Width` oder `Height` sich geändert hat. Im Beispiel [**WhatSize**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/WhatSize) wird mit diesem Ereignis die Größe der Anzeige des Programms angezeigt.

## <a name="metrical-sizes"></a>Metrische Größen

[**MetricalBoxView**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/MetricalBoxView) verwendet [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) und [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest), um eine `BoxView` mit einer Höhe von einem Zoll und einer Breite von einem Zentimeter anzuzeigen.

## <a name="estimated-font-sizes"></a>Geschätzte Schriftgrade

Im Beispiel [**FontSizes**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/FontSizes) wird gezeigt, wie Sie die 160-Einheiten-pro-Zoll-Regel verwenden, um Schriftgrade in Punkten anzugeben. Bei Plattformen, die diese Technik verwenden, ist die visuelle Konsistenz besser als mit `Device.GetNamedSize`.

## <a name="fitting-text-to-available-size"></a>Textgröße an den verfügbaren Platz anpassen

Ein Textblock kann innerhalb eines bestimmten Rechtecks angepasst werden, indem der Wert für `FontSize` für das `Label` anhand der folgenden Kriterien berechnet wird:

- Der Zeilenabstand entspricht 120 % des Schriftgrads (130 % auf Windows-Plattformen).
- Die durchschnittliche Zeichenbreite ist 50 % des Schriftgrads.

Im Beispiel [**EstimatedFontSize**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/EstimatedFontSize) wird diese Technik gezeigt. Dieses Programm wurde geschrieben, bevor die Eigenschaft [`Margin`](xref:Xamarin.Forms.View.Margin) verfügbar war. Daher wird darin [`ContentView`](xref:Xamarin.Forms.ContentView) mit der Einstellung [`Padding`](xref:Xamarin.Forms.Layout.Padding) verwendet, um einen Rand zu simulieren.

[![Dreifacher Screenshot mit geschätztem Schriftgrad](images/ch05fg07-small.png "An den verfügbaren Platz angepasster Text")](images/ch05fg07-large.png#lightbox "An den verfügbaren Platz angepasster Text")

## <a name="a-fit-to-size-clock"></a>Uhr mit angepasster Größe

Im Beispiel [**FitToSizeClock**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/FitToSizeClock) wird gezeigt, wie mit [`Device.StartTimer`](xref:Xamarin.Forms.Device.StartTimer(System.TimeSpan,System.Func{System.Boolean})) ein Timer gestartet wird, der die Anwendung in regelmäßigen Abständen darüber informiert, dass die Uhr aktualisiert werden muss. Als Schriftgrad ist ein Sechstel der Seitenbreite festgelegt, um die Anzeige so groß wie möglich zu machen.

## <a name="accessibility-issues"></a>Barrierefreiheitsprobleme

Die Programme **EstimatedFontSize** und **FitToSizeClock** enthalten beide einen kleinen Mangel: Wenn der Benutzer auf seinem Smartphone unter Android oder Windows 10 Mobile die Einstellungen für die Barrierefreiheit ändert, kann das Programm nicht mehr auf Grundlage des Schriftgrads schätzen, wie groß der Text gerendert wird. Im Beispiel [**AccessibilityTest**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/AccessibilityTest) wird dieses Problem demonstriert.

## <a name="empirically-fitting-text"></a>Empirisches Anpassen der Textgröße

Eine andere Möglichkeit, Text an ein Rechteck anzupassen, ist die empirische Berechnung der gerenderten Textgröße und entsprechende Anpassung nach oben oder unten. Das Programm im Buch ruft [`GetSizeRequest`](xref:Xamarin.Forms.VisualElement.GetSizeRequest(System.Double,System.Double)) für ein visuelles Objekt auf, um die gewünschte Größe für das Element zu erhalten. Diese Methode ist veraltet, und in Programmen sollte stattdessen [`Measure`](xref:Xamarin.Forms.VisualElement.Measure(System.Double,System.Double,Xamarin.Forms.MeasureFlags)) aufgerufen werden.

Bei einem `Label` sollte das erste Argument die Breite des Containers sein (um ein Umbrechen zu ermöglichen), während für das zweite Argument `Double.PositiveInfinity` festgelegt werden sollte, um die Höhe nicht zu beschränken. Diese Technik wird im Beispiel [**EmpiricalFontSize**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/EmpiricalFontSize) gezeigt.

## <a name="related-links"></a>Verwandte Links

- [Kapitel 5: vollständiger Text (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch05-Apr2016.pdf)
- [Kapitel 5: Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05)
- [Kapitel 5: F#-Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/FS)
