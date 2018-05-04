---
ms.assetid: 7C132A7C-4973-4B2D-98DC-3661C08EA33F
title: WPF im Vergleich zu. Xamarin.Forms-App-Lebenszyklus
description: Grundlegendes zu den app-Start-Prozess sowie für den Umgang mit Hintergrund Zuständen
ms.technology: xamarin-crossplatform
author: asb3993
ms.author: amburns
ms.date: 04/26/2017
ms.openlocfilehash: dfcc00fa34d3bd5026653a4550e16634bd2c6238
ms.sourcegitcommit: 4b0582a0f06598f3ff8ad5b817946459fed3c42a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/03/2018
---
# <a name="wpf-vs-xamarinforms-app-lifecycle"></a>WPF im Vergleich zu. Xamarin.Forms-App-Lebenszyklus

Xamarin.Forms belegt viele Leitfaden zum Design aus der XAML-basierte Frameworks, die davor besonders WPF stammen. Allerdings abweicht auf andere Weise es deutlich u. einen Zettel Punkt für Personen, die bei dem Versuch, die über migrieren können. Dieses Dokument versucht, diese Probleme zu identifizieren und Sie erhalten Hilfestellung möglichst WPF Knowledge-Brücke in Xamarin.Forms.

## <a name="app-lifecycle"></a>App-Lebenszyklus

Der Lebenszyklus der Anwendung zwischen WPF und Xamarin.Forms ähnelt. Sowohl in externen (Plattform) Code starten Sie, und starten Sie die Benutzeroberfläche über einen Methodenaufruf. Der Unterschied besteht darin, dass Xamarin.Forms immer in einer Assembly plattformspezifischen gestartet wird, die dann initialisiert und die Benutzeroberfläche für die app erstellt.

**WPF**

- `Main method > App > MainWindow`

> [!NOTE]
> Die `Main` Methode ist, wird standardmäßig automatisch generiert und in den Code nicht sichtbar.

**Xamarin.Forms**

- **iOS** &ndash; `Main method > AppDelegate > App > ContentPage`
- **Android** &ndash; `MainActivity > App > ContentPage`
- **UNIVERSELLE WINDOWS-PLATTFORM** &ndash; `Main method > App(UWP) > MainPage(UWP) > App > ContentPage`

### <a name="application-class"></a>Application-Klasse

WPF und Xamarin.Forms haben eine `Application` Klasse als Singleton erstellt wurde. In den meisten Fällen werden dieser Klasse, die eine benutzerdefinierte Anwendung apps ableiten, obwohl dies in WPF nicht zwingend erforderlich ist. Beide verfügbar machen, ein `Application.Current` Eigenschaft erstellten Singleton gefunden.

### <a name="global-properties--persistence"></a>Globale Eigenschaften + Persistenz

WPF und Xamarin.Forms haben eine `Application.Properties` Wörterbuch verfügbar, in dem Sie globale Objekte auf app-Ebene, die an einer beliebigen Stelle in der Anwendung zugegriffen werden kann speichern können. Der wesentliche Unterschied ist, wird Xamarin.Forms _beibehalten_ alle primitiven Typen, die in der Auflistung gespeichert werden, wenn die Anwendung wird angehalten, und Laden Sie sie erneut, wenn er neu gestartet wird. WPF unterstützt dieses Verhalten nicht automatisch – Sie stattdessen die meisten Entwickler basieren auf den isolierten Speicher oder verwendet die integrierte `Settings` unterstützen.

## <a name="defining-pages-and-the-visual-tree"></a>Definieren von Seiten und die visuelle Struktur

WPF verwendet die `Window` als Stammelement für alle visual-Element der obersten Ebene. Dadurch wird einen HWND definiert, in der Windows-Welt, um Informationen anzuzeigen. Sie können erstellt und wie viele Fenster gleichzeitig beliebig in WPF anzeigen.

In Xamarin.Forms mit, der obersten Ebene das visuelle Element immer von der Plattform - z. B. auf iOS definiert ist, es ist ein `UIWindow`. Xamarin.Forms rendert in diese systemeigene Plattform Darstellungen mit Inhalt ist eine `Page` Klasse. Jede `Page` in Xamarin.Forms stellt eine eindeutige "Seite" in der Anwendung, in denen nur einer zu einem Zeitpunkt sichtbar ist.

Beide WPFs `Window` und Xamarin.Forms `Page` enthalten eine `Title` -Eigenschaft, die den angezeigten Titel, und die beiden beeinflussen ein `Icon` Eigenschaft, um ein bestimmtes Symbol für die Seite anzuzeigen (**Hinweis** , die Titel und das Symbol sind nicht immer in Xamarin.Forms sichtbar). Darüber hinaus können Sie allgemeine visuelle Eigenschaften auf beide z. B. die Hintergrundfarbe oder ein Bild ändern.

Es ist technisch möglich, mit zwei separaten Plattform Ansichten rendern (definieren z. B. zwei `UIWindow` Objekte und die zweite Rendern einer externen anzeigen oder AirPlay haben), es erfordert plattformspezifischen Code zu diesem Zweck und ist keine direkte unterstützte Funktion der Xamarin.Forms selbst.

### <a name="views"></a>Ansichten

Die visuelle Hierarchie für beide Frameworks gleicht. WPF ist ein bisschen tiefer aufgrund der Unterstützung für WYSIWYG-Dokumente.

**WPF**

```
DependencyObject - base class for all bindable things
   Visual - rendering mechanics
      UIElement - common events + interactions
         FrameworkElement - adds layout
            Shape - 2D graphics
            Control - interactive controls
```

**Xamarin.Forms**

```
BindableObject - base class for all bindable things
   Element - basic parent/child support + resources + effects
      VisualElement - adds visual rendering properties (color, fonts, transforms, etc.)
         View - layout + gesture support
```

### <a name="view-lifcycle"></a>Ansicht Lifcycle

Xamarin.Forms ist in erster Linie um mobile-Szenarios ausgerichtet. Daher sind _aktiviert_, _angehalten_, und _reaktiviert_ wie der Benutzer mit ihnen interagiert. Dies ist vergleichbar mit dem verlassen Sie durch Klicken auf die `Window` in einer WPF-Anwendung, und es gibt ein Satz von Methoden und entsprechenden Ereignisse können Sie außer Kraft gesetzt oder in verknüpfen, um dieses Verhalten zu überwachen.

| Zweck | WPF-Methode | Xamarin.Forms-Methode |
|--- |--- |--- |
|Erste Aktivierung|Ctor + Window.OnLoaded|Ctor + Page.OnStart|
|Angezeigt|Window.IsVisibleChanged|Page.Appearing|
|Hidden|Window.IsVisibleChanged|Page.Disapearing|
|Anhalten/Lost Fokus|Window.OnDeactivated|Page.OnSleep|
|Aktiviert/taiwanischen Fokus|Window.OnActivated|Page.OnResume|
|Geschlossen|Window.OnClosing + Window.OnClosed|n/v|


Sowohl als auch untergeordnete Steuerelemente anzeigen/ausblenden-Unterstützung in WPF ist es eine drei-Status-Eigenschaft `IsVisible` (sichtbar, ausgeblendet und reduzierten). In Xamarin.Forms, es ist nur sichtbar oder ausgeblendet, bis die `IsVisible` Eigenschaft.

### <a name="layout"></a>Layout

Seitenlayout tritt auf, in der gleichen 2-Durchlauf (Measures/anordnen), der in WPF erfolgt. Sie können in das Seitenlayout verknüpfen, durch Überschreiben die folgenden Methoden in der Xamarin.Forms `Page` Klasse:

| Methode | Zweck |
|--- |--- |
|OnChildMeasureInvalidated|Bevorzugte Imagegröße ein untergeordnetes Element wurde geändert.|
|OnSizeAllocated|Seite "wurde eine Breite/Höhe zugewiesen.|
|LayoutChanged-Ereignis|Layout und Größe der Seite "wurde geändert.|

Es ist kein globaler layoutereignis der heute aufgerufen wird, noch ist es eine globale `CompositionTarget.Rendering` Ereignis wie in WPF gefunden.

#### <a name="common-layout-properties"></a>Allgemeine Layouteigenschaften

WPF und Xamarin.Forms beide unterstützen das `Margin` Steuerelement Abstand um ein Element und `Padding` Steuerelement Abstand _in_ ein Element. Darüber hinaus verfügen die meisten Ansichten Layout Xamarin.Forms Eigenschaften zum Steuern der Abstand (z. B. Zeile oder Spalte).

Darüber hinaus verfügen die meisten Elemente Eigenschaften, die beeinflussen, wie sie in den übergeordneten Container positioniert werden:

| WPF | Xamarin.Forms | Zweck |
|--- |--- |--- |
|HorizontalAlignment|HorizontalOptions|Links/Mitte/rechts/Stretch-Optionen|
|Vertikale Ausrichtung|VerticalOptions|Nach oben/Mitte/Ende/Stretch-Optionen|

> [!NOTE]
> Die tatsächliche Interpretation dieser Eigenschaften hängt von den übergeordneten Container ab.

#### <a name="layout-views"></a>Layoutansichten

WPF und Xamarin.Forms verwenden Layout-Steuerelemente, um untergeordnete Elemente zu positionieren. In den meisten Fällen sind diese sehr nah beieinander positioniert im Hinblick auf Funktionen.

| WPF | Xamarin.Forms | Layoutstil |
|--- |--- |--- |
|StackPanel|StackLayout|Stapeln von links nach rechts und oben nach unten unendlich|
|Raster|Raster|Tabellenformat (Zeilen und Spalten)|
|DockPanel|n/v|An den Rändern des Fensters Andocken|
|Canvas|AbsoluteLayout|Positionieren von Pixel-Koordinate|
|WrapPanel|n/v|Wrapping Stapel|
|n/v|RelativeLayout|Relative Positionierung regelbasierte|

> [!NOTE]
> Xamarin.Forms unterstützt keine `GridSplitter`.

Beide Plattformen _angefügte Eigenschaften_ Kinder zu optimieren.

### <a name="rendering"></a>Rendern

Die Rendering-Mechanismen für WPF und Xamarin.Forms unterscheiden sich grundlegend. In WPF Rendern die Steuerelemente, die Sie nicht direkt erstellen Inhalt in Pixel auf dem Bildschirm. WPF verwaltet zwei Objektdiagramme (_Strukturen_) zur dies - Darstellung der _logischen Struktur_ dar, die Steuerelemente im Code oder in XAML definiert und die _visuellen Struktur_ darstellt der tatsächliche Rendern, die auf dem Bildschirm auftritt, also entweder direkt über das visuelle Element (über eine virtuelle Draw-Methode), ausgeführt oder durch einen XAML-definierte `ControlTemplate` diese ausgetauscht oder angepasst werden können. Die visuelle Struktur ist in der Regel komplexer, wenn sie dies z. B. Rahmenlinien um Steuerelemente, die Bezeichnungen für implizite Inhalte usw. enthält. WPF umfasst eine Reihe von APIs (`LogicalTreeHelper` und `VisualTreeHelper`) untersuchen diese zwei Objektdiagramme.

In Xamarin.Forms, die Steuerelemente definieren Sie einer `Page` sind tatsächlich nur einfache Datentypen Objekte. Sie sind der logischen Darstellung ähnelt, jedoch nie Rendern von Inhalt auf ihren eigenen. Stattdessen sind sie der _Datenmodell_ der Einfluss auf das Rendern von Elementen. Das tatsächliche Rendering erfolgt durch eine [trennen Satz von _visual Renderer_ , der jeden Steuerelementtyp zugeordnet sind](~/xamarin-forms/app-fundamentals/custom-renderer/index.md). Diese Renderer werden in jedem der plattformspezifischen Projekte plattformspezifischen Xamarin.Forms Assemblys registriert. Sie können eine Liste finden Sie unter [hier](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md). Zusätzlich zum Ersetzen oder erweitern den Renderer, Xamarin.Forms bietet auch Unterstützung für [Effekte](~/xamarin-forms/app-fundamentals/effects/index.md) die, die das systemeigene Rendering auf Basis eines je Plaform beeinflussen verwendet werden können.

#### <a name="the-logicalvisual-tree"></a>Der logische/Visual-Struktur

Es ist keine API verfügbar gemacht, die logische Struktur in Xamarin.Forms - durchlaufen, jedoch können Sie mithilfe von Reflektion die gleiche Informationen abrufen. Beispielsweise [hier ist eine Methode, die logischen untergeordneten Element auflisten kann](https://github.com/xamarinhq/xamu-infrastructure/blob/master/src/XamU.Infrastructure/Extensions/ElementExtensions.cs#L108) mit Reflektion.

## <a name="graphics"></a>Grafik

Xamarin.Forms keine Grafiksystem für primitive Typen über ein einfaches Rechteck einschließt (`BoxView`). Sie können z. B. 3rd Party Bibliotheken einschließen [SkiaSharp](~/graphics-games/skiasharp/index.md) abzurufenden plattformübergreifende 2D Zeichnung oder [UrhoSharp](~/graphics-games/urhosharp/index.md) für 3D.

## <a name="resources"></a>Ressourcen

WPF und Xamarin.Forms haben sowohl das Konzept von Ressourcen und Ressourcenwörterbücher. Sie können einen beliebigen Objekttyp in Platzieren einer `ResourceDictionary` mit einem Schlüssel und suchen sie mit `{StaticResource}` für Elemente, die nicht geändert werden, oder `{DynamicResource}` für Elemente, die im Wörterbuch zur Laufzeit ändern können. Die Verwendung und die Mechanismen sind identisch mit folgendem Unterschied: Xamarin.Forms erfordert, dass Sie definieren die `ResourceDictionary` Zuweisen der `Resources` Eigenschaft während WPF vorab erstellt und sie für Sie weist.

Beispielsweise finden Sie die folgende Definition ein:

**WPF**

```xaml
<Window.Resources>
   <Color x:Key="redColor">#ff0000</Color>
   ...
</Window.Resources>
```

**Xamarin.Forms**

```xaml
<ContentPage.Resources>
   <ResourceDictionary>
      <Color x:Key="redColor">#ff0000</Color>
      ...
   </ResourceDictionary>
</ContentPage.Resources>
```

Wenn Sie keine definieren die `ResourceDictionary`, wird ein Laufzeitfehler generiert.

## <a name="styles"></a>Stile

Stile werden in Xamarin.Forms auch vollständig unterstützt und kann zum Design Xamarin.Forms-Elemente, aus denen die Benutzeroberfläche besteht. Sie unterstützen die Vererbung (Eigenschaft, Ereignis und Daten), Trigger, durch `BasedOn`, und Suchvorgänge für Werte. Stile gelten für Elemente entweder explizit durch die `Style` Eigenschaft oder implizit durch ein Ressourcenschlüssel – genau wie WPF nicht bereitstellen.

### <a name="device-styles"></a>Geräte-Stile

WPF verfügt über eine Reihe von vordefinierten Eigenschaften (gespeichert als statische Werte auf einen Satz von statischen Klassen, z. B. `SystemColors`) die diktieren Systembus Farben, Schriftarten und Metriken in Form von Werten und Ressourcenschlüssel. Xamarin.Forms ist ähnlich, aber definiert einen Satz von [Gerät Stile](~/xamarin-forms/user-interface/styles/device.md) dasselbe darstellen. Diese Stile werden von dem Framework angegeben und auf Grundlage der Common Language Runtime-Umgebung (z. B. Eingabehilfen) Werte festgelegt sind.

**WPF**

```xaml
<Label Text="Title" Foreground="{DynamicResource {x:Static SystemColors.DesktopBrushKey}}" />
```

**Xamarin.Forms**

```xaml
<Label Text="Title" Style="{DynamicResource TitleStyle}" />
```
