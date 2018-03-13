---
title: "Zusammenfassung der Kapitel 5. Umgang mit der Größe"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 486800E9-C09F-4B95-9AC2-C0F8FE563BCF
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 1df1751c55c6a031bf9f26d774b739f4ca83fa91
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/12/2018
---
# <a name="summary-of-chapter-5-dealing-with-sizes"></a>Zusammenfassung der Kapitel 5. Umgang mit der Größe

Einige Größen in Xamarin.Forms hat bisher erkannt wurde:

- Die Höhe der iOS-Statusleiste ist 20
- Die `BoxView` standardmäßige Breite und Höhe von 40 hat
- Die Standardeinstellung `Padding` in einem `Frame` ist 20.
- Die Standardeinstellung `Spacing` auf die `StackLayout` ist 6.
- Die `Device.GetNamedSize` Methode gibt einen numerischen Schriftgrad zurück.

Diese Größen sind nicht Pixel. Stattdessen sind sie geräteunabhängigen Einheiten, die unabhängig von jeder Plattform erkannt.

## <a name="pixels-points-dps-dips-and-dius"></a>Pixel, Punkte, Dps, DIPs und DIUs

Früh in der Versionsgeschichte der Apple Mac- und Microsoft Windows bearbeitet Programmierer in Pixel. Allerdings muss die Einführung von höherer Auflösung angezeigt einen mehr virtuellen und abstrakten Ansatz in Bildschirmkoordinaten. In der Mac-Welt gearbeitet Programmierer in Einheiten von *Punkt*, traditionell 1/72 Zoll beim Windows-Entwickler verwendet *geräteunabhängigen Einheiten* (DIUs) auf der Grundlage von 1/96 Zoll.

Mobile Geräte, allerdings werden im Allgemeinen ähnelt also eher dem Zifferblatt der aufrechterhalten und haben eine höhere Auflösung als Desktop Bildschirme, das bedeutet, dass eine größere Pixeldichte toleriert werden kann.

Programmierer, die für Apple iPhone und iPad-Geräte auch weiterhin in Einheiten von *Punkte*, aber es 160 dieser Punkte pro Zoll gibt. Je nach Gerät kann 1, 2 oder 3 Pixel zu dem Zeitpunkt vorhanden sein.

Android gleicht. Programmierer, die in der Einheit arbeiten *Dichte typunabhängig Pixel* (DP), und die Beziehung zwischen Dps und Pixel basiert auf 160 Dps pro Zoll.

Windows-Runtime wurde auch Skalierungsfaktoren hergestellt, die ein Element in der Nähe 160 geräteunabhängigen Einheiten pro Zoll implizieren.

Zusammengefasst kann Zielgruppenadressierung Telefone und Tablets Xamarin.Forms Programmierer davon ausgehen, dass alle Maßeinheiten auf die folgenden Kriterien basieren:

- 160 Einheiten pro Zoll entspricht
- zu den Zentimeter 64 Einheiten

Die schreibgeschützte [ `Width` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Width/) und [ `Height` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Height/) von definierte Eigenschaften `VisualElement` Standardwerte "Werte & #x 2013; modellieren" 1. Nur, wenn ein Element wurde angepasst und im Layout untergebracht werden diese Eigenschaften die tatsächliche Größe des Elements in dem geräteunabhängigen Einheiten entsprechen. Diese Größe enthält alle `Padding` für das Element festgelegt, aber nicht die `Margin`.

Wird ausgelöst, ein visuelles Element der [ `SizeChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.VisualElement.SizeChanged/) Ereignis bei seiner `Width` oder `Height` hat sich geändert. Die [ **WhatSize** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/WhatSize) Beispiel verwendet dieses Ereignis, um das Programm Bildschirmgröße anzuzeigen.

## <a name="metrical-sizes"></a>Metrische Größen

Die [ **MetricalBoxView** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/MetricalBoxView) verwendet [ `WidthRequest` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.WidthRequest/) und [ `HeightRequest` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.HeightRequest/) zum Anzeigen einer `BoxView` ein Zoll hoch und eine Zentimeter breit.

## <a name="estimated-font-sizes"></a>Geschätzte Schriftgrade Ihren Bedürfnissen

Die [ **Schriftgrade** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/FontSizes) Beispiel zeigt, wie die 160 Einheiten-bis-des-Zoll-Regel Schriftgrade Ihren Bedürfnissen in Einheiten von Punkten an. Die visual Konsistenz zwischen den Plattformen, die auf diese Weise ist besser als `Device.GetNamedSize`.

## <a name="fitting-text-to-available-size"></a>Text, verfügbare Größe anpassen

Es ist möglich, passen einen Textblock innerhalb eines bestimmten Rechtecks durch Berechnen einer `FontSize` von der `Label` anhand der folgenden Kriterien:

- Zeilenabstand entspricht 120 % des Schriftgrads (130 % auf den Windows-Plattformen).
- Durchschnittliche Zeichenbreite ist 50 % des Schriftgrads.

Die [ **EstimatedFontSize** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/EstimatedFontSize) Beispiel wird diese Technik veranschaulicht. Dieses Programm wurde geschrieben, bevor Sie die [ `Margin` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.Margin/) Eigenschaft war verfügbar ist, damit es verwendet ein [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/) mit eine [ `Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/) Einstellung zum Simulieren einer Rand.

[![Dreifacher Screenshot des geschätzten Schriftgrad](images/ch05fg07-small.png ", verfügbare Größe des Texts")](images/ch05fg07-large.png#lightbox "Text an verfügbare Größe anpassen")

## <a name="a-fit-to-size-clock"></a>Eine Anpassung auf Größe Uhr

Die [ **FitToSizeClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/FitToSizeClock) Beispiel veranschaulicht die Verwendung [ `Device.StartTimer` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.StartTimer/p/System.TimeSpan/System.Func%7BSystem.Boolean%7D/) einen Timer gestartet, die die Anwendung in regelmäßigen Abständen werden benachrichtigt, wenn die Uhr aktualisiert wird. Der Schriftgrad wird auf einem-sechste von der Seitenbreite festgelegt, um die Anzeige so groß wie möglich zu machen.

## <a name="accessibility-issues"></a>Probleme mit dem Zugriff

Die **EstimatedFontSize** Programm und die **FitToSizeClock** Programm beide enthalten einen geringfügigen Fehler: Wenn der Benutzer das Telefon Eingabehilfen-Einstellungen für Android oder Windows 10 Mobile, das Programm nicht mehr geändert können basierend auf den Schriftgrad schätzen wie groß der Text gerendert wird. Die [ **AccessibilityTest** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/AccessibilityTest) Beispiel veranschaulicht das Problem.

## <a name="empirically-fitting-text"></a>Empirisch Einpassen von text

Eine andere Möglichkeit, einem Rechteck des Texts ist, empirisch Berechnen der Textgröße des gerenderten, und passen es nach oben oder unten. Das Programm in den Aufrufen Buch [ `GetSizeRequest` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.GetSizeRequest/p/System.Double/System.Double/) auf ein visuelles Element, um die gewünschte Größe des Elements zu erhalten. -Methode ist veraltet, und Programme sollten stattdessen aufrufen [`Measure`] (/ api/member/Xamarin.Forms.VisualElement.Measure/p/System.Double/System.Double/Xamarin.Forms.MeasureFlags/).

Für eine `Label`, das erste Argument muss die Breite des Containers (zur Einbindung zulassen), während das zweite Argument sollte festgelegt werden, `Double.PositiveInfinity` die Höhe uneingeschränkten vornehmen. Die [ **EmpiricalFontSize** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/EmpiricalFontSize) Beispiel wird diese Technik veranschaulicht.



## <a name="related-links"></a>Verwandte Links

- [Kapitel 5 Volltext (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch05-Apr2016.pdf)
- [Kapitel 5: Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05)
- [Kapitel 5 f#-Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/FS)
