---
title: Zusammenfassung der Kapitel 27. Benutzerdefinierte Renderer
description: 'Erstellen von mobilen Apps mit Xamarin.Forms: Zusammenfassung der Kapitel 27. Benutzerdefinierte Renderer'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 49961953-9336-4FD4-A42F-6D9B05FF52E7
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 0497770909b33108eaac0fa5044e98febeb61763
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996307"
---
# <a name="summary-of-chapter-27-custom-renderers"></a>Zusammenfassung der Kapitel 27. Benutzerdefinierte Renderer

Eine Xamarin.Forms-Element, z. B. `Button` gerendert wird, mit einer plattformspezifischen-Schaltfläche in einer Klasse, die mit dem Namen gekapselt `ButtonRenderer`.  Hier ist die [iOS-Version von `ButtonRenderer` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.iOS/Renderers/ButtonRenderer.cs), [Android-Version des `ButtonRenderer` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.Android/Renderers/ButtonRenderer.cs), und die [Windows-Runtime-Version der `ButtonRenderer` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.WinRT/ButtonRenderer.cs).

In diesem Kapitel wird erläutert, wie Sie Ihre eigenen Renderer zum Erstellen benutzerdefinierter Ansichten, die plattformspezifischen Objekte zuordnen schreiben können.

## <a name="the-complete-class-hierarchy"></a>Die vollständige Hierarchie

Es gibt sieben Assemblys, die Xamarin.Forms plattformspezifischen Code enthalten.
Sie können die Quelle auf GitHub über diese Links anzeigen:

- [**Xamarin.Forms.Platform** ](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform) (sehr klein)
- [**Xamarin.Forms.Platform.iOS**](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform.iOS)
- [**Xamarin.Forms.Platform.Android**](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform.Android)
- [**Xamarin.Forms.Platform.WinRT**](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform.WinRT) (larger than the next three)
- [**Xamarin.Forms.Platform.UAP**](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform.UAP)
- [**Xamarin.Forms.Platform.WinRT.Tablet**](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform.WinRT.Tablet)
- [**Xamarin.Forms.Platform.WinRT.Phone**](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform.WinRT.Phone)

Die [ **PlatformClassHierarchy** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/PlatformClassHierarchy) Beispiel zeigt eine Klassenhierarchie für die Assemblys, die für das ausgeführte gültig sind.

Sie sehen eine wichtige Klasse, die mit dem Namen `ViewRenderer`. Dies ist die Klasse, der Sie von abgeleitet werden, wenn Sie einen plattformspezifische Renderer erstellen. Da es mit dem System Anzeigen der Zielplattform verknüpft ist vorhanden in drei verschiedenen Versionen:

IOS [ `ViewRenderer<TView, TNativeView>` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.iOS/ViewRenderer.cs#L26) verfügt über generische Argumente:

- `TView` beschränkt auf [`Xamarin.Forms.View`](xref:Xamarin.Forms.View)
- `TNativeView` beschränkt auf [`UIKit.UIView`](https://developer.xamarin.com/api/type/UIKit.UIView/)

Die Android [ `ViewRenderer<TView, TNativeView>` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.Android/ViewRenderer.cs#L14) verfügt über generische Argumente:

- `TView` beschränkt auf [`Xamarin.Forms.View`](xref:Xamarin.Forms.View)
- `TNativeView` beschränkt auf [`Android.Views.View`](https://developer.xamarin.com/api/type/Android.Views.View/)

Die Windows-Runtime [ `ViewRenderer<TElement, TNativeElement>` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.WinRT/ViewRenderer.cs#L12) verfügt über die generischen Argumente unterschiedlich benannt:

- `TElement` beschränkt auf [`Xamarin.Forms.View`](xref:Xamarin.Forms.View)
- `TNativeElement` beschränkt auf [`Windows.UI.Xaml.FrameworkElement`](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.aspx)

Wenn Sie einen Renderer zu schreiben, Sie werden werden Ableiten einer Klasse von `View`, und klicken Sie dann das Schreiben mehrerer `ViewRenderer` Klasse, eine für jede unterstützte Plattform. Jedes plattformspezifische Implementierung verweist auf eine systemeigene Klasse, die vom Typ abgeleitet ist, Sie als geben, die `TNativeView` oder `TNativeElement` Parameter.

## <a name="hello-custom-renderers"></a>Hallo, benutzerdefinierte Renderer!

Die [ **HelloRenderers** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/HelloRenderers) Programm verweist auf eine benutzerdefinierte Ansicht, die mit dem Namen `HelloView` in seine [ `App` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers/App.cs) Klasse.

Die [ `HelloView` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers/HelloView.cs) Klasse befindet sich auf die **HelloRenderers** Projekt, und einfach abgeleitet `View`.

Die [ `HelloViewRenderer` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers.iOS/HelloViewRenderer.cs) -Klasse in der **HelloRenderers.iOS** Projekt abgeleitet `ViewRenderer<HelloView, UILabel>`. In der `OnElementChanged` Override "", Erstellen einer systemeigenen iOS `UILabel` und ruft `SetNativeControl`.

Die [ `HelloViewRenderer` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers.Droid/HelloViewRenderer.cs) -Klasse in der **HelloRenderers.Droid** Projekt abgeleitet `ViewRenderer<HelloView, TextView>`. In der `OnElementChanged` "Override", erstellt es eine Android `TextView` und ruft `SetNativeControl`.

Die [ `HelloViewRenderer` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers.UWP/HelloViewRenderer.cs) -Klasse in der **HelloRenderers.UWP** und anderen Windows-Projekten leitet sich von `ViewRenderer<HelloView, TextBlock>`. In der `OnElementChanged` Override "", erstellt eine Windows `TextBlock` und ruft `SetNativeControl`.

Alle der `ViewRenderer` ableitungen enthalten eine `ExportRenderer` Attribut auf Assemblyebene, die verknüpft die `HelloView` -Klasse mit den jeweiligen `HelloViewRenderer` Klasse. Dies ist wie Renderer von Xamarin.Forms in die einzelnen Plattformprojekte sucht:

[![Dreifacher Screenshot der Ansicht "Hello"](images/ch27fg02-small.png "benutzerdefinierten Renderern")](images/ch27fg02-large.png#lightbox "benutzerdefinierten Renderern")

## <a name="renderers-and-properties"></a>Renderer und Eigenschaften

Der nächste Satz von Renderern Ellipse zeichnen implementiert und befindet sich in den verschiedenen Projekten von der [ **Xamarin.FormsBook.Platform** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform) Lösung.

Die [ `EllipseView` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/EllipseView.cs) Klasse ist in der **Xamarin.FormsBook.Platform** Plattform. Die Klasse ist vergleichbar mit `BoxView` und definiert nur eine einzelne Eigenschaft: `Color` des Typs `Color`.

Die Renderer können übertragen die Werte der Eigenschaften legen Sie für eine `View` an das systemeigene Objekt durch Überschreiben der `OnElementPropertyChanged` -Methode in der Renderer. Innerhalb dieser Methode (und in den meisten des Renderers) sind zwei Eigenschaften verfügbar:

- `Element`, das Xamarin.Forms-Element
- `Control`, die native anzeigen oder das Widget oder Steuerelement-Objekt

Die Arten von diese Eigenschaften werden festgelegt, durch die generischen Parameter `ViewRenderer`. In diesem Beispiel `Element` ist vom Typ `EllipseView`.

Die `OnElementPropertyChanged` außer Kraft setzen kann daher übertragen die `Color` Wert der `Element` zur systemeigenen `Control` Objekt wahrscheinlich mit einer Art der Konvertierung. Im folgenden werden die drei Renderer sind:

- iOS: [ `EllipseViewRenderer` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/EllipseViewRenderer.cs), verwendet eine [ `EllipseUIView` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/EllipseUIView.cs) -Klasse für die Ellipse.
- Android: [ `EllipseViewRenderer` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/EllipseViewRenderer.cs), verwendet eine [ `EllipseDrawableView` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/EllipseDrawableView.cs) -Klasse für die Ellipse.
- Windows-Runtime: [ `EllipseViewRenderer` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/EllipseViewRenderer.cs), die können die systemeigene Windows [ `Ellipse` ](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.ellipse.aspx) Klasse.

Die [ **EllipseDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/EllipseDemo) Klasse zeigt mehrere dieser `EllipseView` Objekte:

[![Dreifacher Screenshot der Ellipse-Demo](images/ch27fg03-small.png "EllipseView benutzerdefinierten Renderern")](images/ch27fg03-large.png#lightbox "EllipseView benutzerdefinierten Renderern")

Die [ **BouncingBall** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/BouncingBall) springt ein `EllipseView` deaktiviert die Seiten des Bildschirms.

## <a name="renderers-and-events"></a>Renderer und Ereignisse

Es ist auch möglich, für den Renderer zum Generieren von Ereignissen indirekt. Die [ `StepSlider` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/StepSlider.cs) Klasse ist vergleichbar mit der normalen Xamarin.Forms `Slider` jedoch ermöglicht es Ihnen, eine Anzahl von diskreten Schritte zwischen der `Minimum` und `Maximum` Werte.

Im folgenden werden die drei Renderer sind:

- iOS: [`StepSliderRenderer`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/StepSliderRenderer.cs)
- Android: [`StepSliderRenderer`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/StepSliderRenderer.cs)
- Windows-Runtime: [`StepSliderRenderer`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/StepSliderRenderer.cs)

Der Renderer erkennt Änderungen an das native Steuerelement und rufen Sie anschließend `SetValueFromRenderer`, das verweist einer bindbaren Eigenschaft definiert, der `StepSlider`, bewirkt, dass eine Änderung der `StepSlider` ausgelöst werden eine `ValueChanged` Ereignis.

Die [ **StepSliderDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/StepSliderDemo) veranschaulicht diese neuen Slider.



## <a name="related-links"></a>Verwandte Links

- [Kapitel 27 Volltext (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch27-Apr2016.pdf)
- [Kapitel 27-Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27)
- [Benutzerdefinierte Renderer](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
