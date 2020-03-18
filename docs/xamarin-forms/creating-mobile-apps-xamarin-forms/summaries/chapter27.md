---
title: Zusammenfassung von Kapitel 27. Benutzerdefinierte Renderer
description: 'Erstellen von mobilen Apps mit Xamarin.Forms: Zusammenfassung von Kapitel 27. Benutzerdefinierte Renderer'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 49961953-9336-4FD4-A42F-6D9B05FF52E7
author: davidbritch
ms.author: dabritch
ms.date: 07/18/2018
ms.openlocfilehash: fd4014fa4db4e90596c100d454cf0467512240a4
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2020
ms.locfileid: "70760506"
---
# <a name="summary-of-chapter-27-custom-renderers"></a>Zusammenfassung von Kapitel 27. Benutzerdefinierte Renderer

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27)

> [!NOTE] 
> In den Anmerkungen auf dieser Seite wird erläutert, inwiefern die Angaben innerhalb des Buchs heute nicht mehr für Xamarin.Forms gelten.

Ein Xamarin.Forms-Element wie ein `Button` wird mit einer plattformspezifischen Schaltfläche gerendert, die in eine Klasse namens `ButtonRenderer` gekapselt ist.  Hier finden Sie die [iOS-Version von `ButtonRenderer`](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.iOS/Renderers/ButtonRenderer.cs), die [Android-Version von `ButtonRenderer`](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.Android/Renderers/ButtonRenderer.cs) und die [UWP-Version von `ButtonRenderer`](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.UAP/ButtonRenderer.cs).

In diesem Kapitel wird erläutert, wie Sie eigene Renderer schreiben können, um benutzerdefinierte Ansichten zu erstellen, die plattformspezifischen Objekten zugeordnet sind.

## <a name="the-complete-class-hierarchy"></a>Die vollständige Klassenhierarchie

Es gibt vier Assemblys, die den plattformspezifischen Code für Xamarin.Forms enthalten.
Sie können sich die Quelle auf GitHub mithilfe dieser Links ansehen:

- [**Xamarin.Forms.Platform**](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform) (sehr klein)
- [**Xamarin.Forms.Platform.iOS**](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform.iOS)
- [**Xamarin.Forms.Platform.Android**](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform.Android)
- [**Xamarin.Forms.Platform.UAP**](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform.UAP)

> [!NOTE]
> Die in diesem Buch erwähnten `WinRT`-Assemblys sind nicht mehr Teil dieser Projektmappe. 

Das [**PlatformClassHierarchy**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/PlatformClassHierarchy)-Beispiel zeigt eine Klassenhierarchie für die Assemblys an, die für die ausführende Plattform gültig sind.

Ihnen wird eine wichtige Klasse namens `ViewRenderer` auffallen. Dies ist die Klasse, von der Sie ableiten, wenn Sie einen plattformspezifischen Renderer erstellen. Sie ist in drei verschiedenen Versionen vorhanden, weil sie an das Ansichtssystem der Zielplattform gebunden ist:

Der iOS-[`ViewRenderer<TView, TNativeView>`](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.iOS/ViewRenderer.cs#L25) besitzt generische Argumente:

- `TView`, beschränkt auf [`Xamarin.Forms.View`](xref:Xamarin.Forms.View).
- `TNativeView`, beschränkt auf [`UIKit.UIView`](xref:UIKit.UIView).

Der Android-[`ViewRenderer<TView, TNativeView>`](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.Android/ViewRenderer.cs#L17) besitzt generische Argumente:

- `TView`, beschränkt auf [`Xamarin.Forms.View`](xref:Xamarin.Forms.View).
- `TNativeView`, beschränkt auf [`Android.Views.View`](xref:Android.Views.View).

Der UWP-[`ViewRenderer<TElement, TNativeElement>`](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.UAP/ViewRenderer.cs#L6) besitzt unterschiedlich benannte generische Argumente:

- `TElement`, eingeschränkt auf [`Xamarin.Forms.View`](xref:Xamarin.Forms.View).
- `TNativeElement`, eingeschränkt auf [`Windows.UI.Xaml.FrameworkElement`](/uwp/api/Windows.UI.Xaml.FrameworkElement).

Beim Schreiben eines Renderers leiten Sie eine Klasse von `View` ab und schreiben dann mehrere `ViewRenderer`-Klassen, eine für jede unterstützte Plattform. Jede plattformspezifische Implementierung verweist auf eine native Klasse, die von dem Typ abgeleitet wird, den Sie als Parameter `TNativeView` oder `TNativeElement` angeben.

## <a name="hello-custom-renderers"></a>Hallo benutzerdefinierte Renderer!

Das [**HelloRenderers**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/HelloRenderers)-Programm verweist auf eine benutzerdefinierte Ansicht namens `HelloView` in seiner [`App`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers/App.cs)-Klasse.

Die [`HelloView`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers/HelloView.cs)-Klasse ist im **HelloRenderers**-Projekt enthalten und wird einfach von `View` abgeleitet.

Die [`HelloViewRenderer`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers.iOS/HelloViewRenderer.cs)-Klasse im **HelloRenderers.iOS**-Projekt wird von `ViewRenderer<HelloView, UILabel>` abgeleitet. In der `OnElementChanged`-Außerkraftsetzung erstellt sie ein natives iOS-`UILabel` und ruft `SetNativeControl` auf.

Die [`HelloViewRenderer`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers.Droid/HelloViewRenderer.cs)-Klasse im **HelloRenderers.Droid**-Projekt wird von `ViewRenderer<HelloView, TextView>` abgeleitet. In der `OnElementChanged`-Außerkraftsetzung erstellt sie eine Android-`TextView` und ruft `SetNativeControl` auf.

Die [`HelloViewRenderer`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers.UWP/HelloViewRenderer.cs)-Klasse im **HelloRenderers.UWP**- und anderen Projekten wird von `ViewRenderer<HelloView, TextBlock>` abgeleitet. In der `OnElementChanged`-Außerkraftsetzung erstellt sie einen Windows-`TextBlock` und ruft `SetNativeControl` auf.

Alle `ViewRenderer`-Ableitungen enthalten ein `ExportRenderer`-Attribut auf Assemblyebene, das die `HelloView`-Klasse der `HelloViewRenderer`-Klasse zuordnet. Auf diese Weise lokalisiert Xamarin.Forms Renderer in den einzelnen Plattformprojekten:

[![Dreifacher Screenshot von „Hallo“-Ansicht](images/ch27fg02-small.png "Benutzerdefinierte Renderer")](images/ch27fg02-large.png#lightbox "Benutzerdefinierte Renderer")

## <a name="renderers-and-properties"></a>Renderer und Eigenschaften

Der nächste Satz von Renderern implementiert das Zeichnen von Ellipsen und befindet sich in den verschiedenen Projekten der [**Xamarin.FormsBook.Platform**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform)-Projektmappe.

Die [`EllipseView`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/EllipseView.cs)-Klasse ist in der **Xamarin.FormsBook.Platform**-Plattform. Die Klasse ähnelt der `BoxView` und definiert nur eine einzelne Eigenschaft: `Color` vom Typ `Color`.

Die Renderer können Eigenschaftswerte übertragen, die für eine `View` auf das native Objekt festgelegt sind, indem die `OnElementPropertyChanged`-Methode im Renderer außer Kraft gesetzt wird. Innerhalb dieser Methode (und innerhalb der meisten Renderer) sind zwei Eigenschaften verfügbar:

- `Element`, das Xamarin.Forms-Element.
- `Control`, das native Ansichts-, Widget- oder Steuerelementobjekt.

Die Typen dieser Eigenschaften werden von den generischen Parametern von `ViewRenderer` bestimmt. In diesem Beispiel ist `Element` vom Typ `EllipseView`.

Die `OnElementPropertyChanged`-Außerkraftsetzung kann deshalb den `Color`-Wert des `Element` an das native `Control`-Objekt übertragen, wahrscheinlich mit einer Form von Konvertierung. Die drei Renderer sind:

- iOS: [`EllipseViewRenderer`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/EllipseViewRenderer.cs), der eine [`EllipseUIView`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/EllipseUIView.cs)-Klasse für die Ellipse verwendet.
- Android: [`EllipseViewRenderer`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/EllipseViewRenderer.cs), der eine [`EllipseDrawableView`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/EllipseDrawableView.cs)-Klasse für die Ellipse verwendet.
- UWP: [`EllipseViewRenderer`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/EllipseViewRenderer.cs), der die native [`Ellipse`](/uwp/api/Windows.UI.Xaml.Shapes.Ellipse)-Klasse von Windows verwenden kann.

Die [**EllipseDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/EllipseDemo)-Klasse zeigt mehrere dieser `EllipseView`-Objekte an:

[![Dreifacher Screenshot der Ellipsendemo](images/ch27fg03-small.png "Benutzerdefinierte Renderer der „EllipseView“")](images/ch27fg03-large.png#lightbox "Benutzerdefinierte Renderer der „EllipseView“")

Beim [**BouncingBall**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/BouncingBall)-Beispiel prallt eine `EllipseView` von den Rändern des Bildschirms ab.

## <a name="renderers-and-events"></a>Renderer und Ereignisse

Renderer können außerdem Ereignisse indirekt generieren. Die [`StepSlider`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/StepSlider.cs)-Klasse ähnelt dem normalen `Slider` in Xamarin.Forms, gestattet aber das Angeben einer Zahl diskreter Schritte zwischen den Werten `Minimum` und `Maximum`.

Die drei Renderer sind:

- iOS: [`StepSliderRenderer`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/StepSliderRenderer.cs)
- Android: [`StepSliderRenderer`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/StepSliderRenderer.cs)
- UWP: [`StepSliderRenderer`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/StepSliderRenderer.cs)

Die Renderer erkennen Änderungen am nativen Steuerelement und rufen dann `SetValueFromRenderer` auf, das auf eine bindbare Eigenschaft verweist, die im `StepSlider` definiert ist, wobei eine Änderung an diesem dafür sorgt, dass `StepSlider` ein `ValueChanged`-Ereignis auslöst.

Im [**StepSliderDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/StepSliderDemo)-Beispiel wird dieser neue Schieberegler veranschaulicht.

## <a name="related-links"></a>Verwandte Links

- [Kapitel 27 – vollständiger Text (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch27-Apr2016.pdf)
- [Kapitel 27 – Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27)
- [Benutzerdefinierte Renderer](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
