---
title: Zusammenfassung der Kapitel 27. Benutzerdefinierte Renderer
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 0c1dc9ba5cf382551a1142110c68d16421db07e4
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2018
---
# <a name="summary-of-chapter-27-custom-renderers"></a>Zusammenfassung der Kapitel 27. Benutzerdefinierte Renderer

Eine Xamarin.Forms-Element, z. B. `Button` wird mit einer plattformspezifischen-Schaltfläche in einer Klasse mit dem Namen gekapselt gerendert `ButtonRenderer`.  So sieht die [iOS-Version `ButtonRenderer` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.iOS/Renderers/ButtonRenderer.cs), die [Android-Version des `ButtonRenderer` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.Android/Renderers/ButtonRenderer.cs), und die [Windows-Runtime-Version von `ButtonRenderer` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.WinRT/ButtonRenderer.cs).

In diesem Kapitel wird erörtert, wie Sie eigene Renderer zum Erstellen benutzerdefinierter Ansichten, die Clientplattform-spezifische Objekte zuordnen schreiben können.

## <a name="the-complete-class-hierarchy"></a>Die vollständige Klassenhierarchie

Es gibt sieben Assemblys, die die Xamarin.Forms plattformspezifischen Code enthalten.
Sie können die Quelle auf GitHub, die über diese Links anzeigen:

- [**Xamarin.Forms.Platform** ](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform) (sehr klein)
- [**Xamarin.Forms.Platform.iOS**](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform.iOS)
- [**Xamarin.Forms.Platform.Android**](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform.Android)
- [**Xamarin.Forms.Platform.WinRT**](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform.WinRT) (larger than the next three)
- [**Xamarin.Forms.Platform.UAP**](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform.UAP)
- [**Xamarin.Forms.Platform.WinRT.Tablet**](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform.WinRT.Tablet)
- [**Xamarin.Forms.Platform.WinRT.Phone**](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform.WinRT.Phone)

Die [ **PlatformClassHierarchy** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/PlatformClassHierarchy) Beispiel zeigt eine Hierarchie von Klassen für die Assemblys, die für die ausgeführte Plattform gültig sind.

Sie sehen eine wichtige Klasse mit dem Namen `ViewRenderer`. Dies ist die Klasse, die einen plattformspezifischen-Renderer für die Erstellung von abgeleitet. Da es an das System die Ansicht für die Zielplattform gebunden ist vorhanden in drei verschiedenen Versionen:

IOS [ `ViewRenderer<TView, TNativeView>` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.iOS/ViewRenderer.cs#L26) enthält generische Argumente:

- `TView` beschränkt auf [`Xamarin.Forms.View`](https://developer.xamarin.com/api/type/Xamarin.Forms.View/)
- `TNativeView` beschränkt auf [`UIKit.UIView`](https://developer.xamarin.com/api/type/UIKit.UIView/)

Die Android [ `ViewRenderer<TView, TNativeView>` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.Android/ViewRenderer.cs#L14) enthält generische Argumente:

- `TView` beschränkt auf [`Xamarin.Forms.View`](https://developer.xamarin.com/api/type/Xamarin.Forms.View/)
- `TNativeView` beschränkt auf [`Android.Views.View`](https://developer.xamarin.com/api/type/Android.Views.View/)

Windows-Runtime [ `ViewRenderer<TElement, TNativeElement>` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.WinRT/ViewRenderer.cs#L12) generische Argumente wurde verschieden benannt:

- `TElement` beschränkt auf [`Xamarin.Forms.View`](https://developer.xamarin.com/api/type/Xamarin.Forms.View/)
- `TNativeElement` beschränkt auf [`Windows.UI.Xaml.FrameworkElement`](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.aspx)

Wenn Sie einen Renderer zu schreiben, Sie werden werden Ableiten einer Klasse von `View`, und klicken Sie dann Schreiben mehrerer `ViewRenderer` Klasse, eine für jede unterstützte Plattform. Jede Clientplattform-spezifische Implementierung eine systemeigene Klasse, die vom Typ abgeleitet ist, Sie als geben, verweist die `TNativeView` oder `TNativeElement` Parameter.

## <a name="hello-custom-renderers"></a>Hello, benutzerdefinierten Renderer!

Die [ **HelloRenderers** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/HelloRenderers) Programm verweist auf eine benutzerdefinierte Ansicht mit dem Namen `HelloView` in seiner [ `App` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers/App.cs) Klasse.

Die [ `HelloView` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers/HelloView.cs) Klasse dient in der **HelloRenderers** Projekt, und einfach leitet sich von `View`.

Die [ `HelloViewRenderer` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers.iOS/HelloViewRenderer.cs) -Klasse in der **HelloRenderers.iOS** Projekt leitet sich von `ViewRenderer<HelloView, UILabel>`. In der `OnElementChanged` "Override", erstellt er eine systemeigene iOS `UILabel` und ruft `SetNativeControl`.

Die [ `HelloViewRenderer` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers.Droid/HelloViewRenderer.cs) -Klasse in der **HelloRenderers.Droid** Projekt leitet sich von `ViewRenderer<HelloView, TextView>`. In der `OnElementChanged` "Override", erstellt es ein Android `TextView` und ruft `SetNativeControl`.

Die [ `HelloViewRenderer` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers.UWP/HelloViewRenderer.cs) -Klasse in der **HelloRenderers.UWP** und anderen Windows-Projekt abgeleitet `ViewRenderer<HelloView, TextBlock>`. In der `OnElementChanged` "Override", erstellt er eine Windows `TextBlock` und ruft `SetNativeControl`.

Alle der `ViewRenderer` ableitungen enthalten eine `ExportRenderer` Attribut auf Assemblyebene, die ordnet die `HelloView` -Klasse mit den jeweiligen `HelloViewRenderer` Klasse. Wie Xamarin.Forms Renderern in den einzelnen plattformprojekten sucht sieht folgendermaßen aus:

[![Dreifacher Screenshot Hello Sicht](images/ch27fg02-small.png "benutzerdefinierten Renderern")](images/ch27fg02-large.png "benutzerdefinierten Renderern")

## <a name="renderers-and-properties"></a>Renderern und Eigenschaften

Der nächste Satz von Renderern Ellipse Zeichnung implementiert und befindet sich in den verschiedenen Projekten von den [ **Xamarin.FormsBook.Platform** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform) Lösung.

Die [ `EllipseView` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/EllipseView.cs) Klasse befindet sich in der **Xamarin.FormsBook.Platform** Plattform. Die Klasse ähnelt `BoxView` und nur eine einzelne Eigenschaft definiert: `Color` vom Typ `Color`.

Der Renderer können Eigenschaftswerte auf Übertragen einer `View` an das systemeigene Objekt durch Überschreiben der `OnElementPropertyChanged` -Methode in der Renderer. Innerhalb dieser Methode (und in den meisten des Renderers) sind zwei Eigenschaften verfügbar:

- `Element`, das Xamarin.Forms-Element
- `Control`, die Ansicht mit systemeigenen oder Gadget oder Control-Objekt

Diese Eigenschaften werden durch die generische Parameter bestimmt `ViewRenderer`. In diesem Beispiel `Element` ist vom Typ `EllipseView`.

Die `OnElementPropertyChanged` Außerkraftsetzung kann daher übertragen der `Color` Wert, der die `Element` dem systemeigenen `Control` Objekt wahrscheinlich mit einigen Art der Konvertierung. Die drei Renderer sind:

- iOS: [ `EllipseViewRenderer` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/EllipseViewRenderer.cs), verwendet eine [ `EllipseUIView` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/EllipseUIView.cs) Klasse für die Ellipse.
- Android: [ `EllipseViewRenderer` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/EllipseViewRenderer.cs), verwendet eine [ `EllipseDrawableView` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/EllipseDrawableView.cs) Klasse für die Ellipse.
- Windows-Runtime: [ `EllipseViewRenderer` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/EllipseViewRenderer.cs), können die systemeigenen Windows [ `Ellipse` ](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.ellipse.aspx) Klasse.

Die [ **EllipseDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/EllipseDemo) Klasse zeigt mehrere dieser `EllipseView` Objekte:

[![Dreifacher Screenshot der Ellipse Demo](images/ch27fg03-small.png "EllipseView benutzerdefinierten Renderern")](images/ch27fg03-large.png "EllipseView benutzerdefinierten Renderern")

Die [ **BouncingBall** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/BouncingBall) abprallt ein `EllipseView` deaktiviert die Seiten des Bildschirms.

## <a name="renderers-and-events"></a>Renderern und Ereignisse

Es ist auch möglich, bei Renderern mit indirekt Ereignisse generiert. Die [ `StepSlider` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/StepSlider.cs) Klasse ähnelt dem normalen Xamarin.Forms `Slider` ermöglicht die Angabe einer Anzahl von diskreten von Testschritten zwischen aber die `Minimum` und `Maximum` Werte.

Die drei Renderer sind:

- iOS: [`StepSliderRenderer`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/StepSliderRenderer.cs)
- Android: [`StepSliderRenderer`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/StepSliderRenderer.cs)
- Windows-Runtime: [`StepSliderRenderer`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/StepSliderRenderer.cs)

Der Renderer Erkennen von Änderungen an der systemeigenen-Steuerelement, und rufen Sie dann `SetValueFromRenderer`, die verweist auf eine bindbare Eigenschaft definiert, der `StepSlider`, eine Änderung an, wodurch die `StepSlider` ausgelöst werden eine `ValueChanged` Ereignis.

Die [ **StepSliderDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/StepSliderDemo) demonstriert neue Schieberegler.



## <a name="related-links"></a>Verwandte Links

- [Kapitel 27 Volltext (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch27-Apr2016.pdf)
- [Kapitel 27-Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27)
- [Benutzerdefinierte Renderer](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
