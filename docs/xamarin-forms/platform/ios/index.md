---
Title: "IOS-Platt Form Features in Xamarin.Forms " Beschreibung: "Hinzufügen von IOS-spezifischer Funktionalität zu Xamarin.Forms Anwendungen".
ms. Prod: xamarin ms. assetid: 634ab62e-68c8-454c-838b-f 1cc4e4e21bc ms. Technology: xamarin-Forms Author: davidbritch ms. Author: dabritch ms. Date: 03/05/2020 NO-LOC: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="ios-platform-features-in-xamarinforms"></a>IOS-Plattformfunktionen inXamarin.Forms

Xamarin.FormsZum Entwickeln von Anwendungen für IOS ist Visual Studio erforderlich. Die [Seite "Unterstützte Plattformen](~/get-started/supported-platforms.md) " enthält weitere Informationen zu den Voraussetzungen.

## <a name="platform-specifics"></a>Plattformeigenschaften

Platt Form Besonderheiten ermöglichen es Ihnen, Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar sind, ohne dass benutzerdefinierte Renderer oder Effekte implementiert werden.

Die folgenden plattformspezifischen Funktionen werden für Xamarin.Forms Sichten, Seiten und Layouts unter IOS bereitgestellt:

- Weichzeichnerunterstützung für beliebige [`VisualElement`](xref:Xamarin.Forms.VisualElement) . Weitere Informationen finden Sie unter [visualelement-weich auf IOS](visualelement-blur.md).
- Deaktivieren des Legacy Farb Modus auf einem unterstützten [`VisualElement`](xref:Xamarin.Forms.VisualElement) . Weitere Informationen finden Sie unter [visualelement Legacy Color Mode on IOS](legacy-color-mode.md).
- Aktivieren eines Schlag Schattens für ein [`VisualElement`](xref:Xamarin.Forms.VisualElement) . Weitere Informationen finden Sie unter [visualelement Drop Shadows on IOS](visualelement-drop-shadow.md).
- Aktivieren eines- [`VisualElement`](xref:Xamarin.Forms.VisualElement) Objekts, um zum ersten Antwort Ereignis zu werden. Weitere Informationen finden Sie unter [visualelement First Responder](visualelement-first-responder.md).

Die folgenden plattformspezifischen Funktionen werden für Xamarin.Forms Ansichten in ios bereitgestellt:

- Festlegen der [`Cell`](xref:Xamarin.Forms.Cell) Hintergrundfarbe. Weitere Informationen finden Sie unter [Zellen Hintergrundfarbe unter IOS](cell-background-color.md).
- Steuern, wann die Elementauswahl in einem Auftritt [`DatePicker`](xref:Xamarin.Forms.DatePicker) . Weitere Informationen finden Sie unter [Auswahl von "DatePicker Item" unter IOS](datepicker-selection.md).
- Stellen Sie sicher, dass der eingeputzte Text in einen passt, [`Entry`](xref:Xamarin.Forms.Entry) indem Sie den Schrift Grad anpassen. Weitere Informationen finden Sie unter [Entry Font Size on IOS](entry-font-size.md).
- Festlegen der Cursor Farbe in einem [`Entry`](xref:Xamarin.Forms.Entry) . Weitere Informationen finden Sie unter [Entry Cursor Color on IOS](entry-cursor-color.md).
- Steuern, ob die [`ListView`](xref:Xamarin.Forms.ListView) Header Zellen beim Scrollen schweben. Weitere Informationen finden Sie unter [ListView Group Header Style on IOS](listview-group-header-style.md).
- Steuern, ob Zeilen Animationen beim Aktualisieren der Element Sammlung deaktiviert werden [`ListView`](xref:Xamarin.Forms.ListView) . Weitere Informationen finden Sie unter [ListView-Zeilen Animationen unter IOS](listview-row-animations.md).
- Festlegen des Trennzeichen Stils für ein [`ListView`](xref:Xamarin.Forms.ListView) . Weitere Informationen finden Sie unter dem [ListView-Trennzeichen Stil unter IOS](listview-separator-style.md).
- Steuern, wann die Elementauswahl in einem Auftritt [`Picker`](xref:Xamarin.Forms.Picker) . Weitere Informationen finden Sie unter [Auswahl Elementauswahl unter IOS](picker-selection.md).
- Steuern, ob ein [`SearchBar`](xref:Xamarin.Forms.SearchBar) über einen Hintergrund verfügt. Weitere Informationen finden Sie unter [der Suchleiste unter IOS](searchbar-style.md).
- Aktivieren der [`Slider.Value`](xref:Xamarin.Forms.Slider.Value) Festlegung der Eigenschaft, indem Sie auf eine Position auf der [`Slider`](xref:Xamarin.Forms.Slider) Leiste tippen, anstatt den Ziehpunkt ziehen zu müssen `Slider` . Weitere Informationen finden Sie unter " [Slider Thumb Tap on IOS](slider-thumb.md)".
- Steuern des Übergangs, der beim Öffnen eines verwendet wird `SwipeView` . Weitere Informationen finden Sie unter [swipeer View-Swipe-Übergangsmodus](swipeview-swipetransitionmode.md).
- Steuern, wann die Elementauswahl in einem Auftritt [`TimePicker`](xref:Xamarin.Forms.TimePicker) . Weitere Informationen finden Sie unter [timePicker Item Selection on IOS](timepicker-selection.md).

Die folgenden plattformspezifischen Funktionen werden für Xamarin.Forms Seiten unter IOS bereitgestellt:

- Steuern, ob die Detailseite eines [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) auf den Schatten angewendet wird, wenn die Master Seite offengelegt wird. Weitere Informationen finden Sie unter [masterdetailpage Shadow](masterdetailpage-shadow.md).
- Ausblenden des Navigationsleisten Trennzeichens auf einem [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) . Weitere Informationen finden Sie unter " [navigationpage-Balken Trennzeichen" unter IOS](navigation-bar-separator.md).
- Steuern, ob die Navigationsleiste durchsichtig ist. Weitere Informationen finden Sie unter [Navigation in der Navigationsleiste unter IOS](navigation-bar-translucent.md).
- Steuern, ob die Textfarbe der Statusleiste für einen entsprechend [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) der Helligkeit der Navigationsleiste angepasst wird. Weitere Informationen finden Sie unter [navigationpage Bar-textfarbmodus unter IOS](status-bar-text-color.md).
- Steuern, ob der Seitentitel als großer Titel in der Seiten Navigationsleiste angezeigt wird. Weitere Informationen finden Sie [im Thema zu großen Seitentiteln unter IOS](page-large-title.md).
- Festlegen der Sichtbarkeit des Start Indikators für einen [`Page`](xref:Xamarin.Forms.Page) . Weitere Informationen finden Sie unter [Start Indikator Sichtbarkeit unter IOS](page-home-indicator.md).
- Festlegen der Sichtbarkeit der Statusleiste für einen [`Page`](xref:Xamarin.Forms.Page) . Weitere Informationen finden Sie unter [Übersicht über die Seiten Status Leiste unter IOS](page-status-bar-visibility.md).
- Sicherstellen, dass Seiten Inhalt in einem Bereich des Bildschirms positioniert ist, der für alle IOS-Geräte sicher ist. Weitere Informationen finden Sie [im Handbuch zum Safe Area Layout unter IOS](page-safe-area-layout.md).
- Festlegen des Präsentations Stils von modalen Seiten. Weitere Informationen finden Sie unter [modale Seiten Präsentationsstil](page-presentation-style.md).
- Festlegen des Transaktionsmodus der Registerkarten Leiste eines [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) . Weitere Informationen finden Sie unter [der tabbedpage-Bild Lauf Leiste unter IOS](tabbedpage-translucent-tabbar.md).

Die folgenden plattformspezifischen Funktionen werden für Xamarin.Forms Layouts unter IOS bereitgestellt:

- Steuern, ob eine [`ScrollView`](xref:Xamarin.Forms.ScrollView) touchbewegung behandelt oder an ihren Inhalt übergibt. Weitere Informationen finden Sie unter [Übersicht über ScrollView-Inhalte unter IOS](scrollview-content-touches.md).

Die folgende plattformspezifische Funktionalität wird für die- Xamarin.Forms [`Application`](xref:Xamarin.Forms.Application) Klasse unter IOS bereitgestellt:

- Deaktivieren der Barrierefreiheits Skalierung für benannte Schriftgrößen. Weitere Informationen finden Sie unter [Barrierefreiheits Skalierung für benannte Schriftgrößen unter IOS](named-font-size-scaling.md).
- Aktivieren von Steuerelement Layout und Renderingupdates, die im Haupt Thread ausgeführt werden. Weitere Informationen finden Sie unter [Haupt-Thread Steuerungs Updates unter IOS](main-thread-updates-ui.md).
- Aktivieren [`PanGestureRecognizer`](xref:Xamarin.Forms.PanGestureRecognizer) von in einer scrollansicht zum Erfassen und Freigeben der Schwenkbewegung mit der scrollansicht. Weitere Informationen finden Sie unter [gleichzeitige Erkennung von Pan-Gesten unter IOS](application-pan-gesture.md).

## <a name="ios-specific-formatting"></a>IOS-spezifische Formatierung

Xamarin.Formsermöglicht die Festlegung von plattformübergreifenden Benutzeroberflächen Formaten und Farben, aber es gibt noch weitere Optionen zum Festlegen des Designs von IOS mithilfe von Plattform-APIs im IOS-Projekt.

[Erfahren Sie mehr über das Formatieren](formatting.md) der Benutzeroberfläche mithilfe von IOS-spezifischen APIs, wie z **. b. info. plist** -Konfiguration und `UIAppearance` API.

![](images/status-white-sml.png "iOS Theming")

## <a name="other-ios-features"></a>Andere IOS-Features

Mithilfe von [benutzerdefinierten Renderer](~/xamarin-forms/app-fundamentals/custom-renderer/index.md), [dependencyservice](~/xamarin-forms/app-fundamentals/dependency-service/index.md)und [messagingcenter](~/xamarin-forms/app-fundamentals/messaging-center.md)ist es möglich, eine Vielzahl von systemeigenen Funktionen in Xamarin.Forms Anwendungen für IOS einzubinden.
