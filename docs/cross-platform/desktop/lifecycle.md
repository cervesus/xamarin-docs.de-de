---
ms.assetid: 7C132A7C-4973-4B2D-98DC-3661C08EA33F
title: WPF im Vergleich zu. Xamarin.Forms-App-Lebenszyklus
description: In diesem Dokument vergleicht die die ähnlichkeiten und Unterschiede zwischen den Lebenszyklus der Anwendung für Xamarin.Forms und WPF-Anwendungen. Er untersucht außerdem die visuelle Struktur, Grafiken, Ressourcen und Stile.
author: asb3993
ms.author: amburns
ms.date: 04/26/2017
ms.openlocfilehash: a59f257d1e6285fa2d899271a1aae9778b04d985
ms.sourcegitcommit: 47709db4d115d221e97f18bc8111c95723f6cb9b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2018
ms.locfileid: "40251018"
---
# <a name="wpf-vs-xamarinforms-app-lifecycle"></a>WPF im Vergleich zu. Xamarin.Forms-App-Lebenszyklus

Xamarin.Forms hat viele Leitfaden zum Design aus der XAML-basierte Frameworks, die davor, besonders in WPF bereitgestellt wurde. Allerdings abweicht auf andere Weise es erheblich die möglicherweise eine Kurznotiz Punkt für Personen, die beim Migrieren. Dieses Dokument versucht, einige dieser Probleme zu identifizieren und Orientierungshilfen möglichst an die Bridge WPF-Kenntnisse in Xamarin.Forms.

## <a name="app-lifecycle"></a>App-Lebenszyklus

Der Lebenszyklus der Anwendung zwischen WPF und Xamarin.Forms entspricht. Extern (Plattform) Code Start- und starten Sie die Benutzeroberfläche durch einen Methodenaufruf. Der Unterschied besteht darin, dass Xamarin.Forms immer in einer Assembly plattformspezifische gestartet wird, die dann initialisiert und die Benutzeroberfläche für die app erstellt.

**WPF**

- `Main method > App > MainWindow`

> [!NOTE]
> Die `Main` Methode ist, wird standardmäßig automatisch generiert und in den Code nicht sichtbar.

**Xamarin.Forms**

- **iOS** &ndash; `Main method > AppDelegate > App > ContentPage`
- **Android** &ndash; `MainActivity > App > ContentPage`
- **UWP** &ndash; `Main method > App(UWP) > MainPage(UWP) > App > ContentPage`

### <a name="application-class"></a>Application-Klasse

Sowohl WPF als auch Xamarin.Forms ein `Application` Klasse, die als Singleton erstellt wird. In den meisten Fällen, apps von dieser Klasse zu einer benutzerdefinierten Anwendung ableitet, obwohl dies in WPF nicht unbedingt erforderlich ist. Beide verfügbar machen, eine `Application.Current` Eigenschaft, um die erstellten Singleton zu suchen.

### <a name="global-properties--persistence"></a>Globale Eigenschaften und Persistenz

Sowohl WPF als auch Xamarin.Forms eine `Application.Properties` Wörterbuch verfügbar zum Speichern globalen auf app-Ebene-Objekte, die an einer beliebigen Stelle in der Anwendung zugegriffen werden kann. Der Hauptunterschied ist Xamarin.Forms wird _beibehalten_ alle primitiven Typen, die in der Auflistung gespeichert werden, wenn die Anwendung wird angehalten, und sie erneut zu laden, wenn er erneut gestartet wird. WPF unterstützt dieses Verhalten nicht automatisch – Sie stattdessen die meisten Entwickler auf isolierten Speicher verlassen oder verwendet die integrierte `Settings` unterstützen.

## <a name="defining-pages-and-the-visual-tree"></a>Definieren von Seiten und die visuelle Struktur

WPF verwendet die `Window` als Stammelement für alle visuelle Element oberster Ebene. Definiert ein HWND in der Windows-Welt, um Informationen anzuzeigen. Sie können erstellt und so viele Fenster gleichzeitig beliebig in WPF anzeigen.

In Xamarin.Forms der obersten Ebene das visuelle Element immer von der Plattform – z. B. auf iOS definiert ist, es ist ein `UIWindow`. Xamarin.Forms rendert Inhalt in diesen systemeigene Plattform-Darstellungen, die mit einem `Page` Klasse. Jede `Page` in Xamarin.Forms stellt eine eindeutige "Seite" in der Anwendung, in denen nur einer gleichzeitig sichtbar ist.

Beide WPFs `Window` und Xamarin.Forms `Page` enthalten eine `Title` -Eigenschaft, die angezeigten Titel, und beide beeinflussen ein `Icon` Eigenschaft, um ein bestimmtes Symbol für die Seite anzuzeigen (**Hinweis** , die die Titel und das Symbol sind nicht immer in Xamarin.Forms sichtbar). Darüber hinaus können Sie allgemeine Eigenschaften visueller Elemente sowohl, wie z. B. die Hintergrundfarbe oder ein Bild ändern.

Es ist technisch möglich, die in zwei separaten Plattform Ansichten gerendert werden (z. B. definieren zwei `UIWindow` Objekten und dem zweiter rendern, um einen externen Bildschirm oder AirPlay haben), es erfordert Sie plattformspezifischen Code zu diesem Zweck und ist nicht direkt eine Funktion von Xamarin.Forms selbst.

### <a name="views"></a>Ansichten

Die visuelle Hierarchie für beide Frameworks ist ähnlich. WPF ist ein wenig vertieft, aufgrund dessen-Unterstützung für die WYSIWYG-Dokumente.

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

Xamarin.Forms ist in erster Linie für mobile Szenarien ausgerichtet. Anwendungen sind _aktiviert_, _angehalten_, und _reaktiviert_ während der Benutzer mit ihnen interagiert. Dies ist vergleichbar mit dem Benutzer klicken auf die `Window` in einer WPF-Anwendung, und es gibt eine Reihe von Methoden und die entsprechenden Ereignisse, die Sie außer Kraft setzen können, oder in verknüpfen, um dieses Verhalten zu überwachen.

| Zweck | WPF-Methode | Xamarin.Forms-Methode |
|--- |--- |--- |
|Erste Aktivierung|"ctor" + Window.OnLoaded|"ctor" + Page.OnStart|
|Angezeigt|Window.IsVisibleChanged|Page.Appearing|
|Hidden|Window.IsVisibleChanged|Page.Disapearing|
|Anhalten/Verlust-Fokus|Window.OnDeactivated|Page.OnSleep|
|Aktiviert/erhalten konzentrieren|Window.OnActivated|Page.OnResume|
|Geschlossen|Window.OnClosing + Window.OnClosed|n/v|


Beide auch untergeordnete Steuerelemente anzeigen/ausblenden-Unterstützung in WPF es ist eine drei-Status-Eigenschaft `IsVisible` (sichtbar, ausgeblendet, und reduziert). In Xamarin.Forms, es ist nur sichtbar oder ausgeblendet, über die `IsVisible` Eigenschaft.

### <a name="layout"></a>Layout

Seitenlayout tritt auf, in der gleichen 2-Durchlauf (Measure/anordnen), die in WPF wird. Sie können in das Seitenlayout verknüpfen, durch das Überschreiben der folgenden Methoden in der Xamarin.Forms `Page` Klasse:

| Methode | Zweck |
|--- |--- |
|OnChildMeasureInvalidated|Bevorzugte Größe eines untergeordneten Elements hat sich geändert.|
|OnSizeAllocated|Seite wurde eine Breite/Höhe zugewiesen.|
|LayoutChanged-Ereignis|Layout/Größe der Seite hat sich geändert.|

Es ist keine globale Layout-Ereignis der heute aufgerufen wird, es ist auch ein globales `CompositionTarget.Rendering` Ereignis wie finden Sie in WPF.

#### <a name="common-layout-properties"></a>Allgemeine Layouteigenschaften

WPF und Xamarin.Forms, die beide unterstützen `Margin` Steuerelement Abstand um ein Element, und `Padding` Steuerelement Abstand _in_ eines Elements. Darüber hinaus verfügen die meisten Ansichten Layout Xamarin.Forms Eigenschaften zum Steuern der Abstand (z. B. Zeile oder Spalte).

Darüber hinaus verfügen die meisten Elemente der Eigenschaften, die beeinflussen, wie sie in den übergeordneten Container positioniert werden:

| WPF | Xamarin.Forms | Zweck |
|--- |--- |--- |
|"HorizontalAlignment"|HorizontalOptions|Links/Center/rechts/Stretch-Optionen|
|Vertikale Ausrichtung|Eigenschaften "verticaloptions"|Top/Center/nach unten/Stretch-Optionen|

> [!NOTE]
> Die eigentliche Interpretation dieser Eigenschaften hängt von den übergeordneten Container ab.

#### <a name="layout-views"></a>Layoutansichten

WPF und Xamarin.Forms verwenden Layout-Steuerelemente, um untergeordnete Elemente zu positionieren. In den meisten Fällen sind dies sehr nah beieinander positioniert hinsichtlich der Funktionalität.

| WPF | Xamarin.Forms | Layoutstil |
|--- |--- |--- |
|StackPanel|StackLayout|Stapeln von links nach rechts oder oben-nach-unten-unendlich|
|Raster|Raster|Tabellenformat (Zeilen und Spalten)|
|DockPanel|n/v|An den Rändern des Fensters Andocken|
|Canvas|Von "AbsoluteLayout"|Positionieren von Pixelkoordinate /|
|WrapPanel|n/v|Wrapping-stack|
|n/v|RelativeLayout|Relative regelbasierte Positionierung|

> [!NOTE]
> Xamarin.Forms wird nicht unterstützt. eine `GridSplitter`.

Verwenden Sie beide Plattformen _angefügte Eigenschaften_ untergeordnete Elemente zu optimieren.

### <a name="rendering"></a>Rendern

Die Rendering-Mechanismen für WPF und Xamarin.Forms unterscheiden sich grundlegend. In WPF Rendern die Steuerelemente, die Sie direkt erstellen Inhalt Pixel auf dem Bildschirm. WPF verwaltet zwei Objektdiagramme (_Strukturen_) folgendermaßen aus: Darstellen der _logische Struktur_ gemäß Definition im Code oder XAML, die Steuerelemente darstellt und die _visuelle Struktur_ stellt der eigentliche Rendering, das auf dem Bildschirm auftritt, handelt es sich entweder direkt über das visuelle Element (über eine virtuelle Draw-Methode), ausgeführt oder über eine XAML-definierten `ControlTemplate` diese ersetzt oder angepasst werden. Die visuelle Struktur ist in der Regel komplexer, wenn sie dies z. B. einen Rahmen-Steuerelemente, die Bezeichnungen für implizite Inhalte usw. enthält. WPF umfasst eine Reihe von APIs (`LogicalTreeHelper` und `VisualTreeHelper`) überprüfen diese zwei Objektdiagramme.

In Xamarin.Forms, die Steuerelemente definieren Sie einem `Page` sind eigentlich nur einfache Datenobjekte. Sie sind vergleichbar mit Ihrer Darstellung in der logischen Struktur, jedoch nie Inhalt selbst zu rendern. Stattdessen sind sie der _Datenmodell_ die wirkt sich auf das Rendern von Elementen. Die eigentliche Rendering erfolgt durch eine [trennen Satz von _visual Renderer_ werden die für jeden Steuerelementtyp zugeordnet](~/xamarin-forms/app-fundamentals/custom-renderer/index.md). Diese Renderer werden in den einzelnen plattformspezifischen Projekten von plattformspezifischen Xamarin.Forms-Assemblys registriert. Sie sehen eine Liste [hier](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md). Zusätzlich zu ersetzen oder erweitern den Renderer, Xamarin.Forms bietet auch Unterstützung für [Effekte](~/xamarin-forms/app-fundamentals/effects/index.md) die verwendet werden können, um die native Rendern auf einer Basis pro Plattform zu beeinflussen.

#### <a name="the-logicalvisual-tree"></a>Der logische/Visual-Struktur

Es gibt keine verfügbar gemachte API, die logische Struktur in Xamarin.Forms - durchlaufen, aber Sie Reflektion verwenden, um die gleichen Informationen abzurufen. Z. B. [hier ist eine Methode, die logischen untergeordneten Elemente auflisten kann](https://github.com/xamarinhq/xamu-infrastructure/blob/master/src/XamU.Infrastructure/Extensions/ElementExtensions.cs#L108) mit Reflektion.

## <a name="graphics"></a>Grafik

Xamarin.Forms enthält kein Grafiksystem für primitive Typen über ein einfaches Rechteck (`BoxView`). Sie können z. B. 3. Bibliotheken von Drittanbietern einschließen [SkiaSharp](~/graphics-games/skiasharp/index.md) abzurufenden plattformübergreifende 2D-zeichnen oder [von UrhoSharp](~/graphics-games/urhosharp/index.md) für 3D.

## <a name="resources"></a>Ressourcen

WPF und Xamarin.Forms haben beide das Konzept der Ressourcen und Ressourcenwörterbücher. Sie können einen beliebigen Objekttyp in Platzieren einer `ResourceDictionary` mit einem Schlüssel, und suchen sie sich mit `{StaticResource}` für einige Elemente, die nicht geändert werden, oder `{DynamicResource}` für Dinge, die im Wörterbuch zur Laufzeit ändern können. Die Nutzung und die Mechanismen sind identisch mit einem Unterschied: Xamarin.Forms erfordert, dass Sie definieren die `ResourceDictionary` Zuweisen der `Resources` Eigenschaft während WPF vorab erstellt und diese für Sie weist.

Beispielsweise finden Sie in der folgenden Definition:

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

Stile werden in Xamarin.Forms auch vollständig unterstützt und kann zum Design der Xamarin.Forms-Elemente, aus denen die Benutzeroberfläche besteht. Sie unterstützen die Vererbung (Eigenschaft, Ereignis und Daten), Trigger, über `BasedOn`, und Suchvorgänge für Werte. Stile auf Elemente angewendet werden entweder explizit durch die `Style` Eigenschaft oder implizit durch ein Ressourcenschlüssel – genau wie WPF keine Angabe.

### <a name="device-styles"></a>Gerätestile

WPF enthält eine Reihe von vordefinierten Eigenschaften (gespeichert als statische Werte, die auf einem Satz von statischen Klassen wie z. B. `SystemColors`) das Diktieren ordnen Farben, Schriftarten und Metriken in Form von Werten und Ressourcenschlüssel. Xamarin.Forms ist ähnlich, aber definiert einen Satz von [Gerätestile](~/xamarin-forms/user-interface/styles/device.md) dieselben Dinge darstellen. Diese Formatvorlagen sind durch das Framework bereitgestellt und auf Werte, die basierend auf der Common Language Runtime-Umgebung (z. B. "Eingabehilfen") festgelegt sind.

**WPF**

```xaml
<Label Text="Title" Foreground="{DynamicResource {x:Static SystemColors.DesktopBrushKey}}" />
```

**Xamarin.Forms**

```xaml
<Label Text="Title" Style="{DynamicResource TitleStyle}" />
```
