---
title: iOS-Plattformfeatures in Xamarin.Forms
description: Hinzufügen einer iOS-spezifische Funktion für Xamarin.Forms-Anwendungen.
ms.prod: xamarin
ms.assetid: 634AB62E-68C8-454C-838B-F1CC4E4E21BC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/22/2019
ms.openlocfilehash: 7aa9d298c2219dff60ca8ca1f917c72194ad21ca
ms.sourcegitcommit: 0fd04ea3af7d6a6d6086525306523a5296eec0df
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2019
ms.locfileid: "67512675"
---
# <a name="ios-platform-features-in-xamarinforms"></a>iOS-Plattformfeatures in Xamarin.Forms

Entwickeln von Xamarin.Forms-Anwendungen für iOS erfordert Visual Studio. Die [Seite "speicherplatzanforderungen"](~/get-started/requirements.md) enthält weitere Informationen zu den Voraussetzungen.

## <a name="platform-specifics"></a>Plattformeigenschaften

Plattformeigenschaften können Sie Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar ist ohne die Implementierung der benutzerdefinierten Renderern und Effekte.

Die folgende plattformspezifischen Funktionen werden für Xamarin.Forms-Ansichten, Seiten und Layouts unter iOS bereitgestellt:

- Blur-Unterstützung für eine [ `VisualElement` ](xref:Xamarin.Forms.VisualElement). Weitere Informationen finden Sie unter [VisualElement Blur unter iOS](visualelement-blur.md).
- Deaktivieren auf einer unterstützten legacy Farbmodus [ `VisualElement` ](xref:Xamarin.Forms.VisualElement). Weitere Informationen finden Sie unter [VisualElement Farbe Legacymodus unter iOS](legacy-color-mode.md).
- Aktivieren einen Schlagschatten für eine [ `VisualElement` ](xref:Xamarin.Forms.VisualElement). Weitere Informationen finden Sie unter [VisualElement löschen Shadows unter iOS](visualelement-drop-shadow.md).

Die folgende plattformspezifischen Funktionen werden für Xamarin.Forms-Ansichten unter iOS bereitgestellt:

- Festlegen der [ `Cell` ](xref:Xamarin.Forms.Cell) Hintergrundfarbe. Weitere Informationen finden Sie unter [Hintergrundfarbe der Zelle unter iOS](cell-background-color.md).
- Um sicherzustellen, die eingegebenen Text passt in einen [ `Entry` ](xref:Xamarin.Forms.Entry) durch den Schriftgrad anpassen. Weitere Informationen finden Sie unter [Eintrag Schriftgrad unter iOS](entry-font-size.md).
- Festlegen der cursorfarbe in einem [ `Entry` ](xref:Xamarin.Forms.Entry). Weitere Informationen finden Sie unter [Eintrag Cursorfarbe unter iOS](entry-cursor-color.md).
- Steuern, ob [ `ListView` ](xref:Xamarin.Forms.ListView) Headerzellen "float" während des Bildlaufs. Weitere Informationen finden Sie unter [ListView-Group-Headerformat unter iOS](listview-group-header-style.md).
- Steuern, ob die Zeile Animationen deaktiviert sind, wenn die [ `ListView` ](xref:Xamarin.Forms.ListView) Items-Auflistung aktualisiert wird. Weitere Informationen finden Sie unter [ListView-Zeile-Animationen in iOS](listview-row-animations.md).
- Festlegen von den Trennzeichenstil für eine [ `ListView` ](xref:Xamarin.Forms.ListView). Weitere Informationen finden Sie unter [ListView Trennzeichenstil unter iOS](listview-separator-style.md).
- Steuern der bei Auswahl von Listenelementen in tritt auf, eine [ `Picker` ](xref:Xamarin.Forms.Picker). Weitere Informationen finden Sie unter [Auswahl Elementauswahl unter iOS](picker-selection.md).
- Aktivieren der [ `Slider.Value` ](xref:Xamarin.Forms.Slider.Value) festzulegende Eigenschaft durch Tippen auf eine Position auf der [ `Slider` ](xref:Xamarin.Forms.Slider) Balken-und nicht durch Ziehen mit der `Slider` Thumb-Steuerelement. Weitere Informationen finden Sie unter [Schieberegler Thumb-Steuerelement, tippen Sie auf die iOS](slider-thumb.md).

Die folgende plattformspezifischen Funktionen werden für Xamarin.Forms-Seiten unter iOS bereitgestellt:

- Ausblenden von-Trennzeichens. die Navigation auf einer [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage). Weitere Informationen finden Sie unter ["NavigationPage"-Trennzeichens unter iOS](navigation-bar-separator.md).
- Steuern, ob die Navigationsleiste transparent ist. Weitere Informationen finden Sie unter [Navigation Leiste Durchsichtigkeit unter iOS](navigation-bar-translucent.md).
- Steuern, ob die Farbe des Statusleiste angezeigte Texts auf einer [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) wird angepasst, um die Helligkeit der Navigationsleiste zu entsprechen. Weitere Informationen finden Sie unter ["NavigationPage" Text Color Leistenmodus für iOS](status-bar-text-color.md).
- Steuern, ob es sich bei der Seitentitel als große Titel auf der Seite Navigationsleiste angezeigt wird. Weitere Informationen finden Sie unter [große Seitentitel unter iOS](page-large-title.md).
- Festlegen der Sichtbarkeit des Indikators, home auf eine [ `Page` ](xref:Xamarin.Forms.Page). Weitere Informationen finden Sie unter [Startseite Indikator Sichtbarkeit unter iOS](page-home-indicator.md).
- Festlegen der Sichtbarkeit der Status für eine [ `Page` ](xref:Xamarin.Forms.Page). Weitere Informationen finden Sie unter [Seite Status Sichtbarkeit unter iOS](page-status-bar-visibility.md).
- Sicherstellen der Seite Inhalt auf einen Bereich des Bildschirms befindet sich, die für alle iOS-Geräte sicher ist. Weitere Informationen finden Sie unter [abgesicherten Bereich Layoutführungslinie unter iOS](page-safe-area-layout.md).
- Festlegen der Präsentationsstil der modale Seiten auf einem iPad an. Weitere Informationen finden Sie unter [iPad modale Seite Präsentation Style](ipad-page-presentation-style.md).

Die folgende plattformspezifischen Funktionen werden für Xamarin.Forms-Layouts unter iOS bereitgestellt:

- Steuern, ob eine [ `ScrollView` ](xref:Xamarin.Forms.ScrollView) behandelt eine Touch-Geste oder übergibt es an seinen Inhalt. Weitere Informationen finden Sie unter [ScrollView Content tangiert iOS](scrollview-content-touches.md).

Die folgende plattformspezifische Funktionalität wird bereitgestellt, für die Xamarin.Forms [ `Application` ](xref:Xamarin.Forms.Application) Klasse, die unter iOS:

- Die Deaktivierung der Barrierefreiheit, die für die benannte Schriftgrade Ihren Bedürfnissen entsprechend skalieren. Weitere Informationen finden Sie unter [Zugriff Schriftgraden unter iOS mit dem Namen Skalierung](named-font-size-scaling.md).
- Aktivieren der Steuerung des Layouts und Rendern von Updates auf dem Hauptthread ausgeführt werden. Weitere Informationen finden Sie unter [Main Thread Steuern von Updates auf iOS](main-thread-updates-ui.md).
- Aktivieren einer [ `PanGestureRecognizer` ](xref:Xamarin.Forms.PanGestureRecognizer) in einer fortlaufenden zu erfassen und freigeben die Bewegung Schwenken mit der Bildlauf angezeigt. Weitere Informationen finden Sie unter [gleichzeitige Pan Gestenerkennung unter iOS](application-pan-gesture.md).

## <a name="ios-specific-formatting"></a>iOS-spezifische Formatierung

Xamarin.Forms ermöglicht plattformübergreifende Stile der Benutzeroberfläche und Farben festlegen, werden –, aber es gibt weitere Optionen zum Festlegen des Designs Ihrer iOS-Plattform-APIs im iOS-Projekt verwenden.

[Erfahren Sie mehr](formatting.md) zur Formatierung der Benutzeroberfläche mithilfe von iOS-spezifische APIs, wie z. B. **"Info.plist"** Konfiguration und die `UIAppearance` API.

![](images/status-white-sml.png "iOS-Design")

## <a name="other-ios-features"></a>Andere iOS-features

Mithilfe von [benutzerdefinierte Renderer](~/xamarin-forms/app-fundamentals/custom-renderer/index.md), [DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md), und die [MessagingCenter](~/xamarin-forms/app-fundamentals/messaging-center.md), es ist möglich, eine Vielzahl von systemeigenen Funktionen in integrieren Xamarin.Forms-Anwendungen für iOS.
