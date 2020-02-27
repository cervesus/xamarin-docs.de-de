---
title: IOS-Plattformfunktionen in xamarin. Forms
description: Hinzufügen von IOS-spezifischer Funktionalität zu xamarin. Forms-Anwendungen.
ms.prod: xamarin
ms.assetid: 634AB62E-68C8-454C-838B-F1CC4E4E21BC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/15/2020
ms.openlocfilehash: 015db40ce983d979109d4cce32c011f8c61a729c
ms.sourcegitcommit: 10b4d7952d78f20f753372c53af6feb16918555c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/26/2020
ms.locfileid: "77635661"
---
# <a name="ios-platform-features-in-xamarinforms"></a>IOS-Plattformfunktionen in xamarin. Forms

Zum Entwickeln von xamarin. Forms-Anwendungen für IOS ist Visual Studio erforderlich. Die [Seite "Unterstützte Plattformen](~/get-started/supported-platforms.md) " enthält weitere Informationen zu den Voraussetzungen.

## <a name="platform-specifics"></a>Plattformeigenschaften

Plattformeigenschaften können Sie Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar ist ohne die Implementierung der benutzerdefinierten Renderern und Effekte.

Die folgenden plattformspezifischen Funktionen werden für xamarin. Forms-Ansichten,-Seiten und-Layouts unter IOS bereitgestellt:

- Weichzeichnerunterstützung für beliebige [`VisualElement`](xref:Xamarin.Forms.VisualElement). Weitere Informationen finden Sie unter [visualelement-weich auf IOS](visualelement-blur.md).
- Deaktivieren des Legacy Farb Modus auf einem unterstützten [`VisualElement`](xref:Xamarin.Forms.VisualElement). Weitere Informationen finden Sie unter [visualelement Legacy Color Mode on IOS](legacy-color-mode.md).
- Aktivieren eines Schlag Schattens für einen [`VisualElement`](xref:Xamarin.Forms.VisualElement). Weitere Informationen finden Sie unter [visualelement Drop Shadows on IOS](visualelement-drop-shadow.md).
- Aktivieren eines [`VisualElement`](xref:Xamarin.Forms.VisualElement) Objekts, das zum ersten Antwort Ereignis wird. Weitere Informationen finden Sie unter [visualelement First Responder](visualelement-first-responder.md).

Die folgenden plattformspezifischen Funktionen werden für xamarin. Forms-Ansichten unter IOS bereitgestellt:

- Festlegen der [`Cell`](xref:Xamarin.Forms.Cell) Hintergrundfarbe. Weitere Informationen finden Sie unter [Zellen Hintergrundfarbe unter IOS](cell-background-color.md).
- Steuern, wann die Elementauswahl in einer [`DatePicker`](xref:Xamarin.Forms.DatePicker)erfolgt. Weitere Informationen finden Sie unter [Auswahl von "DatePicker Item" unter IOS](datepicker-selection.md).
- Stellen Sie sicher, dass der eingeputzte Text in eine [`Entry`](xref:Xamarin.Forms.Entry) passt, indem Sie den Schrift Grad anpassen. Weitere Informationen finden Sie unter [Entry Font Size on IOS](entry-font-size.md).
- Festlegen der Cursor Farbe in einem [`Entry`](xref:Xamarin.Forms.Entry). Weitere Informationen finden Sie unter [Entry Cursor Color on IOS](entry-cursor-color.md).
- Steuern, ob beim Scrollen [`ListView`](xref:Xamarin.Forms.ListView) Header Zellen schweben. Weitere Informationen finden Sie unter [ListView Group Header Style on IOS](listview-group-header-style.md).
- Steuern, ob Zeilen Animationen deaktiviert werden, wenn die Auflistung der [`ListView`](xref:Xamarin.Forms.ListView) Elemente aktualisiert wird. Weitere Informationen finden Sie unter [ListView-Zeilen Animationen unter IOS](listview-row-animations.md).
- Festlegen des Trennzeichen Stils für einen [`ListView`](xref:Xamarin.Forms.ListView). Weitere Informationen finden Sie unter dem [ListView-Trennzeichen Stil unter IOS](listview-separator-style.md).
- Steuern, wann die Elementauswahl in einer [`Picker`](xref:Xamarin.Forms.Picker)erfolgt. Weitere Informationen finden Sie unter [Auswahl Elementauswahl unter IOS](picker-selection.md).
- Aktivieren der [`Slider.Value`](xref:Xamarin.Forms.Slider.Value) Eigenschaft, indem Sie auf eine Position in der [`Slider`](xref:Xamarin.Forms.Slider) Leiste tippen, anstatt den `Slider` Thumb zu ziehen. Weitere Informationen finden Sie unter " [Slider Thumb Tap on IOS](slider-thumb.md)".
- Steuern des Übergangs, der beim Öffnen einer `SwipeView`verwendet wird. Weitere Informationen finden Sie unter [swipeer View-Swipe-Übergangsmodus](swipeview-swipetransitionmode.md).
- Steuern, wann die Elementauswahl in einer [`TimePicker`](xref:Xamarin.Forms.TimePicker)erfolgt. Weitere Informationen finden Sie unter [timePicker Item Selection on IOS](timepicker-selection.md).

Die folgenden plattformspezifischen Funktionen werden für xamarin. Forms-Seiten unter IOS bereitgestellt:

- Ausblenden des Navigationsleisten Trennzeichens in einem [`NavigationPage`](xref:Xamarin.Forms.NavigationPage). Weitere Informationen finden Sie unter " [navigationpage-Balken Trennzeichen" unter IOS](navigation-bar-separator.md).
- Steuern, ob die Navigationsleiste transparent ist. Weitere Informationen finden Sie unter [Navigation in der Navigationsleiste unter IOS](navigation-bar-translucent.md).
- Steuern, ob die Textfarbe der Statusleiste in einem [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) entsprechend der Helligkeit der Navigationsleiste angepasst wird. Weitere Informationen finden Sie unter [navigationpage Bar-textfarbmodus unter IOS](status-bar-text-color.md).
- Steuern, ob es sich bei der Seitentitel als große Titel auf der Seite Navigationsleiste angezeigt wird. Weitere Informationen finden Sie [im Thema zu großen Seitentiteln unter IOS](page-large-title.md).
- Festlegen der Sichtbarkeit des Start Indikators für einen [`Page`](xref:Xamarin.Forms.Page). Weitere Informationen finden Sie unter [Start Indikator Sichtbarkeit unter IOS](page-home-indicator.md).
- Festlegen der Sichtbarkeit der Statusleiste für einen [`Page`](xref:Xamarin.Forms.Page). Weitere Informationen finden Sie unter [Übersicht über die Seiten Status Leiste unter IOS](page-status-bar-visibility.md).
- Sicherstellen der Seite Inhalt auf einen Bereich des Bildschirms befindet sich, die für alle iOS-Geräte sicher ist. Weitere Informationen finden Sie [im Handbuch zum Safe Area Layout unter IOS](page-safe-area-layout.md).
- Festlegen des Präsentations Stils von modalen Seiten. Weitere Informationen finden Sie unter [modale Seiten Präsentationsstil](page-presentation-style.md).
- Festlegen des Transaktionsmodus der Registerkarten Leiste eines [`TabbedPage`](xref:Xamarin.Forms.TabbedPage). Weitere Informationen finden Sie unter [der tabbedpage-Bild Lauf Leiste unter IOS](tabbedpage-translucent-tabbar.md).

Die folgenden plattformspezifischen Funktionen werden für xamarin. Forms-Layouts unter IOS bereitgestellt:

- Steuern, ob eine [`ScrollView`](xref:Xamarin.Forms.ScrollView) eine touchbewegung verarbeitet oder an ihren Inhalt übergibt. Weitere Informationen finden Sie unter [Übersicht über ScrollView-Inhalte unter IOS](scrollview-content-touches.md).

Die folgende plattformspezifische Funktionalität wird für die xamarin. Forms- [`Application`](xref:Xamarin.Forms.Application) -Klasse unter IOS bereitgestellt:

- Deaktivieren der Barrierefreiheits Skalierung für benannte Schriftgrößen. Weitere Informationen finden Sie unter [Barrierefreiheits Skalierung für benannte Schriftgrößen unter IOS](named-font-size-scaling.md).
- Aktivieren der Steuerung des Layouts und Rendern von Updates auf dem Hauptthread ausgeführt werden. Weitere Informationen finden Sie unter [Haupt-Thread Steuerungs Updates unter IOS](main-thread-updates-ui.md).
- Aktivieren einer [`PanGestureRecognizer`](xref:Xamarin.Forms.PanGestureRecognizer) in einer scrollansicht zum Erfassen und Freigeben der Schwenkbewegung mit der scrollansicht. Weitere Informationen finden Sie unter [gleichzeitige Erkennung von Pan-Gesten unter IOS](application-pan-gesture.md).

## <a name="ios-specific-formatting"></a>IOS-spezifische Formatierung

Xamarin. Forms ermöglicht die Festlegung von plattformübergreifenden Benutzeroberflächen Formaten und Farben, aber es gibt noch weitere Optionen zum Festlegen des Designs von IOS mithilfe von Plattform-APIs im IOS-Projekt.

[Erfahren Sie mehr über das Formatieren](formatting.md) der Benutzeroberfläche mithilfe von IOS-spezifischen APIs, wie z **. b. info. plist** -Konfiguration und der `UIAppearance`-API.

![](images/status-white-sml.png "iOS Theming")

## <a name="other-ios-features"></a>Andere IOS-Features

Mithilfe von [benutzerdefinierten Renderer](~/xamarin-forms/app-fundamentals/custom-renderer/index.md), [dependencyservice](~/xamarin-forms/app-fundamentals/dependency-service/index.md)und [messagingcenter](~/xamarin-forms/app-fundamentals/messaging-center.md)ist es möglich, eine Vielzahl von systemeigenen Funktionen in xamarin. Forms-Anwendungen für IOS einzubinden.
