---
ms.assetid: 7C132A7C-4973-4B2D-98DC-3661C08EA33F
title: WPF-und xamarin. Forms-App-Lebenszyklus
description: In diesem Dokument werden die Ähnlichkeiten und Unterschiede zwischen dem Anwendungslebenszyklus für xamarin. Forms-und WPF-Anwendungen verglichen. Außerdem werden die visuellen Strukturen, Grafiken, Ressourcen und Stile beleuchtet.
author: davidortinau
ms.author: daortin
ms.date: 04/26/2017
ms.openlocfilehash: 725af8aebb111ac620a7d1ca5eebf73f31ee4a5e
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73016470"
---
# <a name="wpf-vs-xamarinforms-app-lifecycle"></a>WPF-und xamarin. Forms-App-Lebenszyklus

Xamarin. Forms hat eine Reihe von Entwurfs Anleitungen aus den XAML-basierten Frameworks, die vor ihm standen, insbesondere WPF. Auf andere Weise weicht Sie jedoch erheblich ab, was für Benutzer, die versuchen, zu migrieren, eine Kurznotiz sein kann. In diesem Dokument wird versucht, einige dieser Probleme zu identifizieren und Anleitungen bereitzustellen, um das WPF-wissen in xamarin. Forms zu überbrücken.

## <a name="app-lifecycle"></a>App-Lebenszyklus

Der Anwendungslebenszyklus zwischen WPF und xamarin. Forms ist ähnlich. Beide beginnen in externem (Platt Form-) Code und starten die Benutzeroberfläche mithilfe eines Methoden Aufrufes. Der Unterschied besteht darin, dass xamarin. Forms immer in einer plattformspezifischen Assembly startet, die dann die Benutzeroberfläche für die APP initialisiert und erstellt.

**WPF**

- `Main method > App > MainWindow`

> [!NOTE]
> Die `Main`-Methode ist standardmäßig automatisch generiert und im Code nicht sichtbar.

**Xamarin.Forms**

- **IOS** -&ndash; `Main method > AppDelegate > App > ContentPage`
- **Android** -&ndash; `MainActivity > App > ContentPage`
- **UWP** -&ndash; `Main method > App(UWP) > MainPage(UWP) > App > ContentPage`

### <a name="application-class"></a>Anwendungsklasse

Sowohl WPF als auch xamarin. Forms verfügen über eine `Application`-Klasse, die als Singleton erstellt wird. In den meisten Fällen werden apps von dieser Klasse abgeleitet, um eine benutzerdefinierte Anwendung bereitzustellen, obwohl dies in WPF nicht unbedingt erforderlich ist. Beide stellen eine `Application.Current`-Eigenschaft zur Verfügung, um das erstellte Singleton zu suchen.

### <a name="global-properties--persistence"></a>Globale Eigenschaften und Persistenz

Sowohl WPF als auch xamarin. Forms verfügen über ein `Application.Properties` Wörterbuch, in dem Sie globale Objekte auf App-Ebene speichern können, die überall in der Anwendung verfügbar sind. Der Hauptunterschied besteht darin, dass xamarin. _Forms alle primitiven_ Typen persistent speichert, die in der Auflistung gespeichert werden, wenn die APP angehalten wird, und Sie beim erneuten Laden erneut laden. WPF unterstützt dieses Verhalten nicht automatisch, da die meisten Entwickler stattdessen auf isoliertem Speicher angewiesen sind oder die integrierte `Settings` Unterstützung genutzt haben.

## <a name="defining-pages-and-the-visual-tree"></a>Definieren von Seiten und der visuellen Struktur

WPF verwendet die `Window` als Stamm Element für ein beliebiges visuelles Element der obersten Ebene. Dadurch wird ein HWND in der Windows-Welt zum Anzeigen von Informationen definiert. In WPF können beliebig viele Fenster gleichzeitig erstellt und angezeigt werden.

In xamarin. Forms wird das visuelle Element der obersten Ebene immer von der Plattform definiert, z. b. unter IOS, es ist eine `UIWindow`. Xamarin. Forms rendert seinen Inhalt mithilfe einer `Page`-Klasse in diese nativen Platt Form Darstellungen. Jede `Page` in xamarin. Forms stellt eine eindeutige "Seite" in der Anwendung dar, bei der jeweils nur eine Seite sichtbar ist.

Sowohl wpfs `Window` als auch xamarin. Forms `Page` enthalten eine `Title`-Eigenschaft, um den angezeigten Titel zu beeinflussen, und beide verfügen über eine `Icon`-Eigenschaft, um ein bestimmtes Symbol für die Seite anzuzeigen (**beachten Sie** , dass der Titel und das Symbol in xamarin. Forms nicht immer sichtbar sind. ). Außerdem können Sie allgemeine visuelle Eigenschaften sowohl für die Hintergrundfarbe als auch für das Bild ändern.

Es ist technisch möglich, eine Darstellung in zwei separate Platt Form Ansichten durchzuführen (z. b. zwei `UIWindow` Objekte zu definieren und das zweite in eine externe Anzeige oder in eine Airplay-Datei zu stellen). hierfür ist ein plattformspezifischer Code erforderlich, und es handelt sich nicht um ein direkt unterstütztes Feature von Xamarin. Forms selbst.

### <a name="views"></a>Ansichten

Die visuelle Hierarchie für beide Frameworks ist ähnlich. WPF ist aufgrund der Unterstützung von WYSIWYG-Dokumenten etwas tiefer.

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

### <a name="view-lifecycle"></a>Ansichtslebenszyklus

Xamarin. Forms ist hauptsächlich auf Mobile Szenarios ausgerichtet. Daher werden Anwendungen _aktiviert_, angehalten und _erneut aktiviert_ _, wenn_der Benutzer mit Ihnen interagiert. Dies ist vergleichbar mit dem Klicken auf die `Window` in einer WPF-Anwendung, und es gibt eine Reihe von Methoden und entsprechenden Ereignissen, die Sie überschreiben oder in das Sie eingebunden werden können, um dieses Verhalten zu überwachen.

| Zweck | WPF-Methode | Xamarin. Forms-Methode |
|--- |--- |--- |
|Anfängliche Aktivierung|ctor + Window. OnLoaded|ctor + Page. OnStart|
|Zeigten|Window. isvisiblechanged|Seite. wird angezeigt|
|Hidden|Window. isvisiblechanged|Seite. verschwindet|
|Suspend/verlorener Fokus|Window. ondeaktiviert|Page. onsleep|
|Aktivierter/Fokus|Window. onaktivierte|Page. onresume|
|Closed|Window. OnClosing + Window. OnClosed|n/v|

Beide unterstützen auch das Ausblenden/Anzeigen von untergeordneten Steuerelementen. in WPF handelt es sich um eine `IsVisible` mit drei Zuständen (sichtbar, ausgeblendet und reduziert). In xamarin. Forms ist Sie nur durch die `IsVisible`-Eigenschaft sichtbar oder ausgeblendet.

### <a name="layout"></a>Layout

Das Seitenlayout tritt im gleichen 2-Durchlauf (Measure/anordnen) auf, der in WPF auftritt. Sie können sich mit dem Seitenlayout verbinden, indem Sie die folgenden Methoden in der `Page`-Klasse von xamarin. Forms überschreiben:

| Methode | Zweck |
|--- |--- |
|Onchildmessreinvaliveraltet|Die bevorzugte Größe eines untergeordneten Elements hat sich geändert.|
|Onsizezugeordnet|Der Seite wurde eine Breite/Höhe zugewiesen.|
|Layoutchanged-Ereignis|Layout/Größe der Seite wurde geändert.|

Es gibt kein globales layoutereignis, das heute aufgerufen wird, und es ist auch kein globales `CompositionTarget.Rendering` Ereignis wie in WPF zu finden.

#### <a name="common-layout-properties"></a>Allgemeine Layouteigenschaften

WPF und xamarin. Forms unterstützen `Margin`, um den Abstand um ein Element zu steuern, und `Padding`, um den Abstand _innerhalb_ eines Elements zu steuern. Außerdem verfügen die meisten xamarin. Forms-Layoutansichten über Eigenschaften zum Steuern des Abstands (z. b. Zeilen oder Spalten).

Außerdem verfügen die meisten Elemente über Eigenschaften, die beeinflussen, wie Sie im übergeordneten Container positioniert werden:

| WPF | Xamarin.Forms | Zweck |
|--- |--- |--- |
|HorizontalAlignment|Horizontaloptions|Links/zentriert/rechts/streckungs Optionen|
|VerticalAlignment|Verticaloptions|Top/Center/Bottom/Stretch-Optionen|

> [!NOTE]
> Die tatsächliche Interpretation dieser Eigenschaften hängt vom übergeordneten Container ab.

#### <a name="layout-views"></a>Layoutansichten

Sowohl WPF als auch xamarin. Forms verwenden Layoutsteuerelemente, um untergeordnete Elemente zu positionieren. In den meisten Fällen sind diese in Bezug auf die Funktionalität sehr nah beieinander.

| WPF | Xamarin.Forms | Layoutstil |
|--- |--- |--- |
|StackPanel|StackLayout|Von links nach rechts oder von oben nach unten (unendliche Stapel)|
|Raster|Raster|Tabellenformat (Zeilen und Spalten)|
|DockPanel|n/v|An Kanten des Fensters andocken|
|Zeichenbereich|AbsoluteLayout|Pixel/Koordinaten Positionierung|
|WrapPanel|n/v|Stapel wird umwickelt|
|n/v|RelativeLayout|Relative regelbasierte Positionierung|

> [!NOTE]
> Xamarin. Forms unterstützt keine `GridSplitter`.

Beide Plattformen verwenden _angefügte Eigenschaften_ , um untergeordnete Elemente zu verfeinern.

### <a name="rendering"></a>Rendern

Die renderingmechanismen für WPF und xamarin. Forms unterscheiden sich radikal. In WPF werden die von Ihnen erstellten Steuerelemente direkt in Pixel auf dem Bildschirm dargestellt. WPF verwaltet zwei Objekt_Diagramme (Strukturen_), die dies darstellen. die _logische_ Struktur stellt die Steuerelemente dar, die in Code oder XAML definiert sind, und die _visuelle_ Struktur stellt das tatsächliche Rendering dar, das auf dem Bildschirm auftritt, der ausgeführt wird. direkt durch das visuelle Element (durch eine virtuelle Draw-Methode) oder durch ein XAML-definiertes `ControlTemplate` das ersetzt oder angepasst werden kann. In der Regel ist die visuelle Struktur komplexer, da Sie Elemente wie z. b. Rahmen um Steuerelemente, Bezeichnungen für impliziten Inhalt usw. enthält. WPF enthält eine Reihe von APIs (`LogicalTreeHelper` und `VisualTreeHelper`), mit denen diese beiden Objekt Diagramme untersucht werden können.

In xamarin. Forms sind die Steuerelemente, die Sie in einem `Page` definieren, tatsächlich nur einfache Datenobjekte. Sie ähneln der logischen Struktur Darstellung, aber Sie werden niemals eigenständig Inhalte Rendering. Stattdessen handelt es sich um das _Datenmodell_ , das sich auf das Rendering von Elementen auswirkt. Das tatsächliche Rendering erfolgt durch einen [separaten Satz von _visuellen renderatoren_ , die den einzelnen Steuerelement Typen zugeordnet sind](~/xamarin-forms/app-fundamentals/custom-renderer/index.md). Diese Renderer werden in jedem der plattformspezifischen Projekte von plattformspezifischen xamarin. Forms-Assemblys registriert. [Hier](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md)finden Sie eine Liste. Zusätzlich zum Ersetzen oder Erweitern des Renderers bietet xamarin. Forms auch Unterstützung für [Effekte](~/xamarin-forms/app-fundamentals/effects/index.md) , die verwendet werden können, um das Native Rendering Platt Form bezogen zu beeinflussen.

#### <a name="the-logicalvisual-tree"></a>Die logische/visuelle Struktur

Es gibt keine verfügbar gemachte API, um die logische Struktur in xamarin. Forms zu durchlaufen, aber Sie können die Reflektion verwenden, um dieselben Informationen zu erhalten. [Hier ist beispielsweise eine Methode, die logische](https://github.com/xamarinhq/xamu-infrastructure/blob/master/src/XamU.Infrastructure/Extensions/ElementExtensions.cs#L108) untergeordnete Elemente mit Reflektion aufzählen kann.

## <a name="graphics"></a>Grafik

Xamarin. Forms enthält kein Grafiksystem für primitive außerhalb eines einfachen Rechtecks (`BoxView`). Sie können Bibliotheken von Drittanbietern wie [skiasharp](~/graphics-games/skiasharp/index.md) einschließen, um eine plattformübergreifende 2D-Zeichnung zu erhalten, oder [urhusharp](~/graphics-games/urhosharp/index.md) für 3D.

## <a name="resources"></a>Ressourcen

WPF und xamarin. Forms haben beide das Konzept von Ressourcen und Ressourcen Wörterbüchern. Sie können einen beliebigen Objekttyp in einem `ResourceDictionary` mit einem Schlüssel platzieren und dann nach `{StaticResource}` suchen, die sich nicht ändern, oder `{DynamicResource}`, die im Wörterbuch zur Laufzeit geändert werden können. Die Verwendung und die Mechanismen sind identisch mit einem Unterschied: xamarin. Forms erfordert, dass Sie die `ResourceDictionary` definieren, die der `Resources`-Eigenschaft zugewiesen werden sollen, während WPF eine vorab-erstellt und Ihnen zuweist.

Informationen hierzu finden Sie beispielsweise in der folgenden Definition:

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

Wenn Sie die `ResourceDictionary`nicht definieren, wird ein Laufzeitfehler generiert.

## <a name="styles"></a>Stile

Stile werden in xamarin. Forms ebenfalls vollständig unterstützt und können verwendet werden, um die xamarin. Forms-Elemente zu formatieren, die die Benutzeroberfläche bilden. Sie unterstützen Trigger (Eigenschaft, Ereignis und Daten), Vererbung durch `BasedOn`und Ressourcen Suchvorgänge für Werte. Stile werden entweder explizit über die `Style`-Eigenschaft oder implizit durch die Angabe eines Ressourcen Schlüssels, wie WPF, angewendet.

### <a name="device-styles"></a>Gerätestile

WPF verfügt über eine Reihe vordefinierter Eigenschaften (als statische Werte für einen Satz statischer Klassen, z. b. `SystemColors`), die Systemfarben, Schriftarten und Metriken in Form von Werten und Ressourcen Schlüsseln vorschreiben. Xamarin. Forms ist ähnlich, aber definiert einen Satz von [Geräte Stilen](~/xamarin-forms/user-interface/styles/device.md) , die dieselben Elemente darstellen. Diese Stile werden vom Framework bereitgestellt und auf Werte basierend auf der Laufzeitumgebung (z. b. Barrierefreiheit) festgelegt.

**WPF**

```xaml
<Label Text="Title" Foreground="{DynamicResource {x:Static SystemColors.DesktopBrushKey}}" />
```

**Xamarin.Forms**

```xaml
<Label Text="Title" Style="{DynamicResource TitleStyle}" />
```
