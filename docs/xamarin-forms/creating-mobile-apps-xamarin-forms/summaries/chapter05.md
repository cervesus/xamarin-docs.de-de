---
title: Zusammenfassung der Kapitel 5. Umgang mit Größen
description: 'Erstellen von mobilen Apps mit Xamarin.Forms: Zusammenfassung der Kapitel 5. Umgang mit Größen'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 486800E9-C09F-4B95-9AC2-C0F8FE563BCF
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 36d208f2326c7584bc03c351b4a5b05a3f3928c9
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995452"
---
# <a name="summary-of-chapter-5-dealing-with-sizes"></a>Zusammenfassung der Kapitel 5. Umgang mit Größen

Mehrere Größen in Xamarin.Forms haben bisher gefunden wurde:

- Die Höhe der Statusleiste iOS ist 20
- Die `BoxView` einer standardmäßigen Breite und Höhe von 40
- Der Standardwert `Padding` in einem `Frame` ist 20.
- Der Standardwert `Spacing` auf die `StackLayout` ist 6
- Die `Device.GetNamedSize` Methode gibt einen numerischen Schriftgrad zurück.

Diese Größen sind nicht Pixel. Stattdessen sind sie geräteunabhängige Einheiten, die unabhängig von jeder Plattform erkannt werden.

## <a name="pixels-points-dps-dips-and-dius"></a>Pixel, Punkte, Dps, DIPs und DIUs

Arbeitete frühzeitig in den Verlauf von Apple Mac- und Microsoft Windows Programmierer in Pixel. Allerdings erforderlich, die Einführung von höherer Auflösung zeigt einen virtualisierten und abstrakten Ansatz in Bildschirmkoordinaten. Der Mac-Welt gearbeitet Programmierer in Einheiten von *Punkte*, normalerweise 1/72 Zoll, bei der Windows-Entwickler, die *geräteunabhängige Einheiten* (DIUs) auf der Grundlage von 1/96 Zoll.

Mobile Geräte, allerdings werden in der Regel wesentlich näher an das Gesicht aufrechterhalten und haben eine höhere Auflösung als Desktop-Bildschirm an, das bedeutet, dass eine größere Pixeldichte toleriert werden kann.

Programmierer, die für Apple iPhone und iPad-Geräte funktioniert weiterhin in Einheiten von *Punkte*, aber es 160 dieser Punkte pro Zoll gibt. Je nach dem Gerät kann 1, 2 oder 3 Pixel auf den Zeitpunkt vorhanden sein.

Android ist ähnlich. Programmierer, die in der Einheit arbeiten *Dichte unabhängigen Pixeln* (Dps), und die Beziehung zwischen den Dps und Pixel basieren auf 160 DP pro Zoll.

Die Windows-Runtime hat auch Skalierungsfaktoren eingerichtet, die sehr nahe kommen 160 geräteunabhängige Einheiten pro Zoll implizieren.

Zusammengefasst kann ein Xamarin.Forms-Programmierer, die für Smartphones und Tablets davon ausgehen, dass alle Maßeinheiten in der folgenden Kriterien basieren:

- 160 Einheiten pro Zoll, entspricht
- 64-Einheiten, um die Zentimeter

Die schreibgeschützte [ `Width` ](xref:Xamarin.Forms.VisualElement.Width) und [ `Height` ](xref:Xamarin.Forms.VisualElement.Height) definierten Eigenschaften `VisualElement` standardmäßige "Werte modellieren" &ndash;1. Nur, wenn ein Element wurde die Größe und Layout untergebracht werden diese Eigenschaften die tatsächliche Größe des Elements in geräteunabhängigen Einheiten entsprechen. Diese Größe enthält alle `Padding` legen Sie für das Element aber nicht die `Margin`.

Wird ausgelöst, ein visuelles Element die [ `SizeChanged` ](xref:Xamarin.Forms.VisualElement.SizeChanged) Ereignis bei der `Width` oder `Height` hat sich geändert. Die [ **WhatSize** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/WhatSize) Beispiel wird dieses Ereignis verwendet, um die Größe des Programms Bildschirm anzuzeigen.

## <a name="metrical-sizes"></a>Metrische Größen

Die [ **MetricalBoxView** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/MetricalBoxView) verwendet [ `WidthRequest` ](xref:Xamarin.Forms.VisualElement.WidthRequest) und [ `HeightRequest` ](xref:Xamarin.Forms.VisualElement.HeightRequest) zum Anzeigen einer `BoxView` Zolls hoch und eine Zentimeter breit.

## <a name="estimated-font-sizes"></a>Geschätzte Schriftgrade Ihren Bedürfnissen entsprechend

Die [ **Schriftgrade** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/FontSizes) Beispiel veranschaulicht, wie die 160-Einheiten-um-die-Zoll-Regel Schriftgrade Ihren Bedürfnissen entsprechend Punkte als Maßeinheit an. Der visuelle Konsistenz zwischen den Plattformen, die mithilfe dieser Technik ist besser als `Device.GetNamedSize`.

## <a name="fitting-text-to-available-size"></a>Text, verfügbare Größe anpassen

Es ist möglich, passen einen Textblock in einem bestimmten Rechteck durch das Berechnen einer `FontSize` von der `Label` anhand der folgenden Kriterien:

- Zeilenabstand ist 120 Prozent des Schriftgrads (130 % auf den Windows-Plattformen).
- Durchschnittliche Zeichenbreite ist 50 % des Schriftgrads.

Die [ **EstimatedFontSize** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/EstimatedFontSize) Beispiel wird diese Technik veranschaulicht. Dieses Programm wurde geschrieben, bevor Sie die [ `Margin` ](xref:Xamarin.Forms.View.Margin) Eigenschaft war verfügbar ist, verwendet eine [ `ContentView` ](xref:Xamarin.Forms.ContentView) mit einer [ `Padding` ](xref:Xamarin.Forms.Layout.Padding) Einstellung aus, um zu simulieren einer Rand.

[![Dreifacher Screenshot der geschätzten Schriftgrad](images/ch05fg07-small.png "Text angepasst wird, um die verfügbare Größe")](images/ch05fg07-large.png#lightbox "Text, die an die verfügbare Größe anpassen")

## <a name="a-fit-to-size-clock"></a>Eine Uhr für Anpassung-zu-size

Die [ **FitToSizeClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/FitToSizeClock) Beispiel zeigt die Verwendung [ `Device.StartTimer` ](xref:Xamarin.Forms.Device.StartTimer(System.TimeSpan,System.Func{System.Boolean})) einen Zeitgeber gestartet, die in regelmäßigen Abständen die Anwendung benachrichtigt, wenn es Zeit, um die Uhr aktualisiert ist. Der Schriftgrad ist ein Sechstel Seitenbreite fest, um die Anzeige so groß wie möglich zu machen.

## <a name="accessibility-issues"></a>Probleme mit der Barrierefreiheit

Die **EstimatedFontSize** Programm und die **FitToSizeClock** Programm beide enthalten einen geringfügigen Fehler: Wenn der Benutzer Einstellungen für die Barrierefreiheit des Telefons auf Android oder Windows 10 Mobile, das Programm nicht mehr ändert. können basierend auf den Schriftgrad schätzen wie groß der Text gerendert wird. Die [ **AccessibilityTest** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/AccessibilityTest) Beispiel veranschaulicht das Problem.

## <a name="empirically-fitting-text"></a>Empirisch Einpassen von text

Eine weitere Möglichkeit zum Anpassen von Text, der ein Rechteck ist empirisch berechnet die Größe der gerenderten Text und nach oben oder unten anpassen. Das Programm in den Aufrufen Buch [ `GetSizeRequest` ](xref:Xamarin.Forms.VisualElement.GetSizeRequest(System.Double,System.Double)) auf ein visuelles Element auf die gewünschte Größe des Elements zu erhalten. Methode ist veraltet und Programme sollten stattdessen Aufrufen [ `Measure` ](xref:Xamarin.Forms.VisualElement.Measure(System.Double,System.Double,Xamarin.Forms.MeasureFlags)).

Für eine `Label`, das erste Argument muss die Breite des Containers (damit können Wrapping), während das zweite Argument sollte festgelegt werden, `Double.PositiveInfinity` , wenn die Höhe uneingeschränkt festgelegt. Die [ **EmpiricalFontSize** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/EmpiricalFontSize) Beispiel wird diese Technik veranschaulicht.



## <a name="related-links"></a>Verwandte Links

- [Kapitel 5 Volltext (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch05-Apr2016.pdf)
- [Kapitel 5-Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05)
- [Kapitel 5 F#-Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/FS)
