---
title: Xamarin.Formsallgemeine Steuerelement Eigenschaften, Methoden und Ereignisse
description: In diesem Artikel werden allgemeine Eigenschaften, Methoden und Ereignisse beschrieben, die für die visualelement-Klasse definiert sind und häufig in abgeleiteten Klassen verwendet werden.
ms.prod: xamarin
ms.assetId: 85A0CCF5-C1D8-40BB-927F-A4D944E5534D
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 06/19/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: f3ab70dc20dda78e3acf400cf51d0ee9df84ff93
ms.sourcegitcommit: 16847681df17ed59b3b3528761c02e8fb48ffc4f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/19/2020
ms.locfileid: "85104324"
---
# <a name="xamarinforms-common-control-properties-methods-and-events"></a>Xamarin.Formsallgemeine Steuerelement Eigenschaften, Methoden und Ereignisse

Die- Xamarin.Forms `VisualElement` Klasse ist die Basisklasse für die meisten Steuerelemente, die in einer-Anwendung verwendet werden Xamarin.Forms . Die- `VisualElement` Klasse definiert viele [Eigenschaften](#properties), [Methoden](#methods)und [Ereignisse](#events) , die in abgeleiteten Klassen verwendet werden.

## <a name="properties"></a>Eigenschaften

Die folgenden Eigenschaften sind für- [`VisualElement`](xref:Xamarin.Forms.VisualElement) Objekte verfügbar.

### [`AnchorX`](xref:Xamarin.Forms.VisualElement.AnchorX)

Die- `AnchorX` Eigenschaft ist ein `double` Wert, der den Mittelpunkt auf der X-Achse für Transformationen wie Skalierung und Drehung definiert. Der Standardwert ist 0,5.

### [`AnchorY`](xref:Xamarin.Forms.VisualElement.AnchorY)

Die- `AnchorY` Eigenschaft ist ein `double` Wert, der den Mittelpunkt auf der X-Achse für Transformationen wie Skalierung und Drehung definiert. Der Standardwert ist 0,5.

### [`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor)

Die- `BackgroundColor` Eigenschaft ist eine `Color` , die die Hintergrundfarbe des Steuer Elements bestimmt. Wenn der Wert nicht festgelegt ist, ist der Hintergrund das Standard `Color` Objekt, das als transparent gerendert wird.

### [`Behaviors`](xref:Xamarin.Forms.VisualElement.Behaviors)

Die- `Behaviors` Eigenschaft ist eine `List` von- `Behavior` Objekten. Mithilfe von Verhalten können Sie wiederverwendbare Funktionen an Elemente anfügen, indem Sie Sie der Liste hinzufügen `Behaviors` . Weitere Informationen zur- `Behavior` Klasse finden Sie unter [ Xamarin.Forms Verhalten](~/xamarin-forms/app-fundamentals/behaviors/index.md).

### [`Bounds`](xref:Xamarin.Forms.VisualElement.Bounds)

Die- `Bounds` Eigenschaft ist ein Schreib geschütztes- `Rectangle` Objekt, das den vom-Steuerelement belegten Bereich darstellt. Der `Bounds` Eigenschafts Wert wird während des layoutcycle zugewiesen. Die `Rectangle` `struct` enthält nützliche Eigenschaften und Methoden zum Testen der Schnittmenge und Kapselung von Rechtecke. Weitere Informationen finden Sie unter The [ Xamarin.Forms Rechteck API](xref:Xamarin.Forms.Rectangle).

### `Clip`

Die- `Clip` Eigenschaft ist ein- `Geometry` Objekt, das die Gliederung des Inhalts eines Elements definiert. Zum Definieren eines Clips verwenden Sie ein `Geometry` -Objekt, z `EllipseGeometry` . b., um die-Eigenschaft des Elements festzulegen `Clip` . Nur der Bereich, der sich innerhalb des Bereichs der Geometrie befindet, wird angezeigt. Weitere Informationen finden Sie unter [Clip Geometrien](~/xamarin-forms/user-interface/shapes/geometries.md#clip-geometries).

### [`Effects`](xref:Xamarin.Forms.Element.Effects)

Die- `Effects` Eigenschaft ist eine `List` von- `Effect` Objekten, die von der `Element` (Xref:) geerbt werden Xamarin.Forms . Element)-Klasse. Mit Effekten können native Steuerelemente angepasst werden, und Sie werden in der Regel für kleine Formatierungs Änderungen verwendet. Weitere Informationen zur- `Effect` Klasse finden Sie unter [ Xamarin.Forms Effekte](~/xamarin-forms/app-fundamentals/effects/index.md).

### [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection)

Die `FlowDirection` Eigenschaft ist ein `FlowDirection` Enumerationswert. Die Fluss Richtung kann auf, oder festgelegt werden `MatchParent` `LeftToRight` `RightToLeft` und bestimmt die layoutreihenfolge und die Richtung. Die- `FlowDirection` Eigenschaft wird in der Regel zur Unterstützung von Sprachen verwendet, die von rechts nach links gelesen werden.

### [`Height`](xref:Xamarin.Forms.VisualElement.Height)

Die- `Height` Eigenschaft ist ein Schreib geschützter- `double` Wert, der die gerenderte Höhe des-Steuer Elements beschreibt. Die `Height` -Eigenschaft wird während des layoutcycle berechnet und kann nicht direkt festgelegt werden. Die Höhe eines Steuer Elements kann mithilfe der " [accesstrequest](#heightrequest)"-Eigenschaft angefordert werden.

### [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest)

Die- `HeightRequest` Eigenschaft ist ein `double` Wert, der die gewünschte Höhe des Steuer Elements bestimmt. Die absolute Höhe des Steuer Elements stimmt möglicherweise nicht mit dem angeforderten Wert identisch. Weitere Informationen finden Sie unter [Anforderungs Eigenschaften](#request-properties).

### [`InputTransparent`](xref:Xamarin.Forms.VisualElement.InputTransparent)

Die- `InputTransparent` Eigenschaft ist eine `bool` , die bestimmt, ob das Steuerelement Benutzereingaben empfängt. Der Standardwert ist `false` . Dadurch wird sichergestellt, dass das Element Eingaben empfängt. Diese Eigenschaft wird auf untergeordnete Elemente übertragen, wenn Sie festgelegt wird. Das Festlegen der- `InputTransparent` Eigenschaft auf `true` für eine Layoutklasse führt dazu, dass alle Elemente innerhalb des Layouts keine Eingaben erhalten.

### [`IsEnabled`](xref:Xamarin.Forms.VisualElement.IsEnabled)

Die- `IsEnabled` Eigenschaft ist ein `bool` Wert, der bestimmt, ob das Steuerelement auf Benutzereingaben reagiert. Der Standardwert ist `true`. Wenn diese Eigenschaft auf false festgelegt wird, wird verhindert, dass das Steuerelement Benutzereingaben akzeptiert.

### [`IsFocused`](xref:Xamarin.Forms.VisualElement.IsFocused)

Die- `IsFocused` Eigenschaft ist ein `bool` Wert, der beschreibt, ob das-Steuerelement derzeit das fokussierte Objekt ist. Das Aufrufen der- [`Focus`](#focus) Methode für das-Steuerelement führt dazu `IsFocused` , dass der Wert auf true festgelegt wird. Wenn Sie die- [`Unfocus`](#unfocus) Methode aufrufen, wird diese Eigenschaft auf false festgelegt.

### [`IsTabStop`](xref:Xamarin.Forms.VisualElement.IsTabStop)

Die- `IsTabStop` Eigenschaft ist ein `bool` Wert, der definiert, ob das Steuerelement den Fokus erhält, wenn der Benutzer durch die Steuerelemente mit der Tab-Taste bewegt wird. Wenn diese Eigenschaft false ist, `TabIndex` hat die Eigenschaft keine Auswirkung.

### [`IsVisible`](xref:Xamarin.Forms.VisualElement.IsVisible)

Die- `IsVisible` Eigenschaft ist ein `bool` Wert, der bestimmt, ob das Steuerelement gerendert wird. Steuerelemente, bei denen die- `IsVisible` Eigenschaft auf false festgelegt ist, werden nicht angezeigt, werden während des layoutlaufs nicht als Speicherplatz Berechnungen angesehen und können keine Benutzereingaben akzeptieren.

### [`MinimumHeightRequest`](xref:Xamarin.Forms.VisualElement.MinimumHeightRequest)

Die- `MinimumHeightRequest` Eigenschaft ist ein `double` Wert, der bestimmt, wie der Überlauf behandelt wird, wenn zwei Elemente um begrenzten Platz konkurrieren. Das Festlegen der- `MinimumHeightRequest` Eigenschaft ermöglicht es dem Layoutprozess, das Element auf die angeforderte minimale Dimension zu skalieren. Wenn kein `MinimumHeightRequest` angegeben wird, ist der Standardwert-1, und der Layoutprozess berücksichtigt als `HeightRequest` Minimalwert. Dies bedeutet, dass Elemente ohne `MinimumHeightRequest` Wert keine skalierbare Höhe haben.

Weitere Informationen finden Sie unter [minimale Anforderungs Eigenschaften](#minimum-request-properties).

### [`MinimumWidthRequest`](xref:Xamarin.Forms.VisualElement.MinimumWidthRequest)

Die- `MinimumWidthRequest` Eigenschaft ist ein `double` Wert, der bestimmt, wie der Überlauf behandelt wird, wenn zwei Elemente um begrenzten Platz konkurrieren. Das Festlegen der- `MinimumWidthRequest` Eigenschaft ermöglicht es dem Layoutprozess, das Element auf die angeforderte minimale Dimension zu skalieren. Wenn kein `MinimumWidthRequest` angegeben wird, ist der Standardwert-1, und der Layoutprozess berücksichtigt als `WidthRequest` Minimalwert. Dies bedeutet, dass Elemente ohne `MinimumWidthRequest` Wert keine skalierbare Breite haben.

Weitere Informationen finden Sie unter [minimale Anforderungs Eigenschaften](#minimum-request-properties).

### [`Opacity`](xref:Xamarin.Forms.VisualElement.Opacity)

Die- `Opacity` Eigenschaft ist ein `double` Wert zwischen 0 (null) und 1, der die Durchlässigkeit des Steuer Elements während des Renderings bestimmt. Der Standardwert für diese Eigenschaft ist 1,0. Werte außerhalb des Bereichs zwischen 0 und 1 werden geklammert. Die- `Opacity` Eigenschaft wird nur angewendet, wenn die- [`IsVisible`](#isvisible) Eigenschaft ist `true` . Die Deckkraft wird iterativ angewendet. Wenn ein übergeordnetes Steuerelement über 0,5 Deckkraft verfügt und dessen untergeordnetes Element über eine 0,5-Deckkraft verfügt, wird das untergeordnete Steuerelement daher mit einem effektiven Wert von 0,25 Deck Kraft Das Festlegen der- `Opacity` Eigenschaft eines Eingabe Steuer Elements auf 0 weist ein nicht definiertes Verhalten auf.

### [`Parent`](xref:Xamarin.Forms.Element.Parent)

Die `Parent`-Eigenschaft wird von der `Element`-Klasse geerbt. Diese Eigenschaft ist ein `Element` Objekt, das das übergeordnete Element des Steuer Elements ist. Die `Parent` -Eigenschaft wird in der Regel automatisch für ein Element festgelegt, wenn es als untergeordnetes Element eines anderen Elements hinzugefügt wird.

### [`Resources`](xref:Xamarin.Forms.VisualElement.Resources)

Die- `Resources` Eigenschaft ist eine- `ResourceDictionary` Instanz, die mit Schlüssel-Wert-Paaren aufgefüllt wird, die in der Regel zur Laufzeit aus XAML aufgefüllt werden. Mithilfe dieses Wörterbuchs können Anwendungsentwickler in XAML definierte Objekte sowohl zur Kompilierzeit als auch zur Laufzeit wieder verwenden. Die Schlüssel im Wörterbuch werden aus dem- `x:Key` Attribut des XAML-Tags aufgefüllt. Das-Objekt, das aus XAML erstellt wurde, wird `ResourceDictionary` für den angegebenen Schlüssel in die eingefügt. Nachdem Sie initialisiert wurde.

Weitere Informationen finden Sie unter [Ressourcen Wörterbücher](~/xamarin-forms/xaml/resource-dictionaries.md).

### [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation)

Die `Rotation` -Eigenschaft ist ein `double` Wert zwischen 0 (null) und 360, der die Drehung um die Z-Achse in Grad definiert. Der Standardwert dieser Eigenschaft ist 0. Die Drehung wird relativ zu den [`AnchorX`](#anchorx) Werten und angewendet [`AnchorY`](#anchory) .

### [`RotationX`](xref:Xamarin.Forms.VisualElement.RotationX)

Die `RotationX` -Eigenschaft ist ein `double` Wert zwischen 0 (null) und 360, der die Drehung der X-Achse in Grad definiert. Der Standardwert dieser Eigenschaft ist 0. Die Drehung wird relativ zu den [`AnchorX`](#anchorx) Werten und angewendet [`AnchorY`](#anchory) .

### [`RotationY`](xref:Xamarin.Forms.VisualElement.RotationY)

Die `RotationY` -Eigenschaft ist ein `double` Wert zwischen 0 (null) und 360, der die Drehung um die Y-Achse in Grad definiert. Der Standardwert dieser Eigenschaft ist 0. Die Drehung wird relativ zu den [`AnchorX`](#anchorx) Werten und angewendet [`AnchorY`](#anchory) .

### [`Scale`](xref:Xamarin.Forms.VisualElement.Scale)

Die- `Scale` Eigenschaft ist ein `double` Wert, der die Skalierung des Steuer Elements definiert. Der Standardwert dieser Eigenschaft ist 1,0. Die Skalierung wird relativ zu [`AnchorX`](#anchorx) den [`AnchorY`](#anchory) Werten und angewendet.

### [`ScaleX`](xref:Xamarin.Forms.VisualElement.ScaleX)

Die- `ScaleX` Eigenschaft ist ein `double` Wert, der die Skalierung des Steuer Elements auf der X-Achse definiert. Der Standardwert dieser Eigenschaft ist 1,0. Die- `ScaleX` Eigenschaft wird relativ zum- [`AnchorX`](#anchorx) Wert angewendet.

### [`ScaleY`](xref:Xamarin.Forms.VisualElement.ScaleY)

Die- `ScaleY` Eigenschaft ist ein `double` Wert, der die Skalierung des Steuer Elements auf der Y-Achse definiert. Der Standardwert dieser Eigenschaft ist 1,0. Die- `ScaleY` Eigenschaft wird relativ zum- [`AnchorY`](#anchory) Wert angewendet.

### [`Style`](xref:Xamarin.Forms.NavigableElement.Style)

Die `Style`-Eigenschaft wird von der `NavigableElement`-Klasse geerbt. Diese Eigenschaft ist eine Instanz der- `Style` Klasse. Die- `Style` Klasse enthält Trigger, Setter und Verhalten, die die Darstellung und das Verhalten von visuellen Elementen definieren. Weitere Informationen finden Sie unter [ Xamarin.Forms XAML-Stile](~/xamarin-forms/user-interface/styles/xaml/index.md).

### [`StyleClass`](xref:Xamarin.Forms.NavigableElement.StyleClass)

Die- `StyleClass` Eigenschaft ist eine Liste von- `string` Objekten, die die Namen von `Style` Klassen darstellen. Diese Eigenschaft wird von der `NavigableElement`-Klasse geerbt. Die- `StyleClass` Eigenschaft ermöglicht das Anwenden mehrerer Stil Attribute auf eine- `VisualElement` Instanz. Weitere Informationen finden Sie unter [ Xamarin.Forms Style Classes](~/xamarin-forms/user-interface/styles/xaml/style-class.md).

### [`TabIndex`](xref:Xamarin.Forms.VisualElement.TabIndex)

Die- `TabIndex` Eigenschaft ist ein- `int` Wert, der die Reihenfolge der Steuerelemente definiert, wenn die Steuerelemente mit der Tab-Taste durch Die- `TabIndex` Eigenschaft ist die Implementierung der-Eigenschaft, die für die- `ITabStopElement` Schnittstelle definiert ist, die die- `VisualElement` Klasse implementiert.

### [`TranslationX`](xref:Xamarin.Forms.VisualElement.TranslationX)

Die- `TranslationX` Eigenschaft ist ein `double` Wert, der die Delta Übersetzung definiert, die auf die X-Achse angewendet werden soll. Die Übersetzung wird nach dem Layout angewendet und wird in der Regel zum Anwenden von Animationen verwendet. Wenn ein Element außerhalb der Grenzen des übergeordneten Containers übersetzt wird, wird verhindert, dass Eingaben funktionieren.

Weitere Informationen finden Sie unter [Animation in Xamarin.Forms ](~/xamarin-forms/user-interface/animation/index.md).

### [`TranslationY`](xref:Xamarin.Forms.VisualElement.TranslationY)

Die- `TranslationY` Eigenschaft ist ein `double` Wert, der die Delta Übersetzung definiert, die auf die Y-Achse angewendet werden soll. Die Übersetzung wird nach dem Layout angewendet und wird in der Regel zum Anwenden von Animationen verwendet. Wenn ein Element außerhalb der Grenzen des übergeordneten Containers übersetzt wird, wird verhindert, dass Eingaben funktionieren.

Weitere Informationen finden Sie unter [Animation in Xamarin.Forms ](~/xamarin-forms/user-interface/animation/index.md).

### [`Triggers`](xref:Xamarin.Forms.VisualElement.Triggers)

Die- `Triggers` Eigenschaft ist eine schreibgeschützte `List` von- `TriggerBase` Objekten. Trigger ermöglichen Anwendungsentwicklern das Ausdrücken von Aktionen in XAML, die die visuelle Darstellung von Steuerelementen als Reaktion auf Ereignis-oder Eigenschafts Änderungen ändern. Weitere Informationen finden Sie unter [ Xamarin.Forms Trigger](~/xamarin-forms/app-fundamentals/triggers.md).

### [`Visual`](xref:Xamarin.Forms.VisualElement.Visual)

Die- `Visual` Eigenschaft ist eine `IVisual` -Instanz, mit der Renderer erstellt und selektiv auf-Instanzen angewendet werden können `VisualElement` . Die- `Visual` Eigenschaft wird so festgelegt, dass Sie Ihrem übergeordneten Element entspricht. das Definieren eines Renderers für eine Komponente gilt auch für alle untergeordneten Elemente dieser Komponente. Wenn kein benutzerdefinierter Renderer für ein Steuerelement oder seine Vorgänger festgelegt ist, Xamarin.Forms wird der Standardrenderer verwendet. Weitere Informationen finden Sie unter [ Xamarin.Forms Visual](~/xamarin-forms/user-interface/visual/index.md).

### [`Width`](xref:Xamarin.Forms.VisualElement.Width)

Die- `Width` Eigenschaft ist ein Schreib geschützter- `double` Wert, der die gerenderte Breite des-Steuer Elements beschreibt. Die `Width` -Eigenschaft wird während des layoutcycle berechnet und kann nicht direkt festgelegt werden. Die Breite eines Steuer Elements kann mithilfe der [widthrequest-Eigenschaft](#widthrequest)angefordert werden.

### [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest)

Die- `WidthRequest` Eigenschaft ist ein `double` Wert, der die gewünschte Breite des Steuer Elements bestimmt. Die absolute Breite des Steuer Elements stimmt möglicherweise nicht mit dem angeforderten Wert identisch. Weitere Informationen finden Sie unter [Anforderungs Eigenschaften](#request-properties).

### [`X`](xref:Xamarin.Forms.VisualElement.X)

Die- `X` Eigenschaft ist ein Schreib geschützter- `double` Wert, der die aktuelle X-Position des-Steuer Elements beschreibt.

### [`Y`](xref:Xamarin.Forms.VisualElement.Y)

Die- `Y` Eigenschaft ist ein Schreib geschützter- `double` Wert, der die aktuelle Y-Position des-Steuer Elements beschreibt.

## <a name="methods"></a>Methoden

Die folgenden Methoden sind für die- `VisualElement` Klasse verfügbar. Eine umfassende Liste finden Sie unter [visualelement-API-Methoden](xref:Xamarin.Forms.VisualElement#methods).

### [`FindByName`](xref:Xamarin.Forms.Element.FindByName*)

Die `FindByName` -Methode wird von der `Element` -Klasse geerbt und weist die folgende Signatur auf:

```csharp
public object FindByName (string name)
```

Diese Methode durchsucht alle untergeordneten Elemente nach dem angegebenen `name` Argument und gibt das Element mit dem angegebenen Namen zurück. Wenn keine Übereinstimmung gefunden wird, wird `null` zurückgegeben.

### [`Focus`](xref:Xamarin.Forms.VisualElement.Focus)

Die- `Focus` Methode versucht, den Fokus auf das Element festzulegen. Diese Methode hat die folgende Signatur:

```csharp
public bool Focus ()
```

Die `Focus` Methode gibt zurück, `true` Wenn der Tastaturfokus erfolgreich festgelegt wurde, und `false` Wenn der Methodenaufrufe nicht zu einer Fokus Änderung geführt hat. Das Element muss in der Lage sein, den Fokus zu erhalten, damit diese Methode funktioniert. Das Aufrufen der- `Focus` Methode für Elemente, die Offscreen oder nicht realisiert sind, hat ein nicht definiertes Verhalten.

### [`Unfocus`](xref:Xamarin.Forms.VisualElement.Unfocus)

Die `Unfocus` Methode versucht, den Fokus auf das Element zu entfernen. Diese Methode hat die folgende Signatur:

```csharp
public void Unfocus ()
```

Das-Element muss bereits den Fokus haben, damit diese Methode funktioniert.

## <a name="events"></a>Ereignisse

Die folgenden Ereignisse sind für die- `VisualElement` Klasse verfügbar. Eine umfassende Liste finden Sie unter [ Xamarin.Forms visualelement-Ereignisse](xref:Xamarin.Forms.VisualElement#events).

### [`Focused`](xref:Xamarin.Forms.VisualElement.Focused)

Das- `Focused` Ereignis wird immer dann ausgelöst, wenn die `VisualElement` Instanz den Fokus erhält. Dieses Ereignis wird nicht durch den Xamarin.Forms Stapel gestapelt, sondern direkt vom systemeigenen Steuerelement empfangen. Dieses Ereignis wird vom [`IsFocused`](#isfocused) Eigenschaften Setter ausgegeben.

### [`SizeChanged`](xref:Xamarin.Forms.VisualElement.SizeChanged)

Das- `SizeChanged` Ereignis wird ausgelöst, wenn sich die- `VisualElement` Instanz `Height` oder- `Width` Eigenschaften ändern. Wenn Entwickler direkt auf die Größenänderung antworten möchten, anstatt auf das nach Änderungs Ereignis zu reagieren, sollten Sie [`OnSizeAllocated`](xref:Xamarin.Forms.VisualElement.OnSizeAllocated*) stattdessen die virtuelle Methode implementieren.

### [`Unfocused`](xref:Xamarin.Forms.VisualElement.Unfocused)

Das- `Unfocused` Ereignis wird ausgelöst, wenn die- `VisualElement` Instanz den Fokus verliert. Dieses Ereignis wird nicht durch den Xamarin.Forms Stapel gestapelt, sondern direkt vom systemeigenen Steuerelement empfangen. Dieses Ereignis wird vom [`IsFocused`](#isfocused) Eigenschaften Setter ausgegeben.

## <a name="units-of-measurement"></a>Maßeinheiten

Android-, IOS-und UWP-Plattformen verfügen jeweils über unterschiedliche Maßeinheiten, die Geräte übergreifend variieren können. Xamarin.Formsverwendet eine plattformunabhängige Maßeinheit, mit der Einheiten über Geräte und Plattformen hinweg normalisiert werden. Es sind 160 Einheiten pro Zoll oder 64 Einheiten pro Zentimeter in vorhanden Xamarin.Forms .

## <a name="request-properties"></a>Anforderungseigenschaften

Eigenschaften, deren Namen "Request" enthalten, definieren einen gewünschten Wert, der möglicherweise nicht dem tatsächlich gerenderten Wert entspricht. Kann z. b. `HeightRequest` auf 150 festgelegt werden, aber wenn das Layout nur Platz für 100 Einheiten zulässt, ist der gerenderte `Height` des Steuer Elements nur 100. Die gerenderte Größe ist von verfügbarem Speicherplatz und enthaltenen Komponenten betroffen.

## <a name="minimum-request-properties"></a>Minimale Anforderungs Eigenschaften

Die Mindestanzahl von Anforderungs Eigenschaften umfasst `MinimumHeightRequest` und `MinimumWidthRequest` , und soll eine präzisere Kontrolle darüber ermöglichen, wie Elemente den Überlauf relativ zueinander verarbeiten. Das Layoutverhalten im Zusammenhang mit diesen Eigenschaften hat jedoch einige wichtige Überlegungen.

### <a name="unspecified-minimum-property-values"></a>Nicht angegebene minimale Eigenschaftswerte

Wenn kein Minimalwert festgelegt ist, ist die minimale Eigenschaft standardmäßig auf-1 festgelegt. Der Layoutprozess ignoriert diesen Wert und berücksichtigt den absoluten Wert als Minimalwert. Die praktische Folge dieses Verhaltens ist, dass ein Element ohne minimalen Wert nicht verkleinert **wird** . Ein Element, bei dem ein Minimalwert angegeben ist, **wird** verkleinert.

Der folgende XAML-Code zeigt zwei `BoxView` Elemente in einem horizontalen `StackLayout` :

```xaml
<StackLayout Orientation="Horizontal">
    <BoxView HeightRequest="100" BackgroundColor="Purple" WidthRequest="500"></BoxView>
    <BoxView HeightRequest="100" BackgroundColor="Green" WidthRequest="500" MinimumWidthRequest="250"></BoxView>
</StackLayout>
```

Die erste `BoxView` Instanz fordert eine Breite von 500 an und gibt keine Mindestbreite an. Die zweite `BoxView` Instanz fordert eine Breite von 500 und eine Mindestbreite von 250 an. Wenn das übergeordnete `StackLayout` Element nicht breit genug ist, um beide Komponenten mit der angeforderten Breite zu enthalten, wird die erste `BoxView` Instanz vom Layoutprozess als Mindestbreite von 500 betrachtet, da kein anderes gültiges Minimalwert angegeben wird. Die zweite `BoxView` Instanz kann auf 250 zentral herunterskaliert werden, und die Größe wird verkleinert, bis die Breite 250 Einheiten erreicht.

Wenn das gewünschte Verhalten dafür gilt, dass die erste `BoxView` Instanz ohne minimale Breite herunterskaliert werden soll, `MinimumWidthRequest` muss auf einen gültigen Wert festgelegt werden, z. b. 0.

### <a name="minimum-and-absolute-property-values"></a>Minimale und absolute Eigenschaftswerte

Das Verhalten ist nicht definiert, wenn der minimale Wert größer ist als der absolute Wert. Wenn beispielsweise `WidthRequest` auf 100 festgelegt ist, sollte die- `MinimumWidthRequest` Eigenschaft niemals den Wert 100 überschreiten. Beim Angeben eines minimalen Eigenschafts Werts sollten Sie immer einen absoluten Wert angeben, um sicherzustellen, dass der absolute Wert größer als der minimale Wert ist.

### <a name="minimum-properties-within-a-grid"></a>Minimale Eigenschaften innerhalb eines Rasters

`Grid`Layouts verfügen über ein eigenes System für die relative Größenänderung von Zeilen und Spalten. Die `MinimumWidthRequest` Verwendung `MinimumHeightRequest` von oder innerhalb eines `Grid` Layouts hat keinen Effekt. Weitere Informationen finden Sie unter [ Xamarin.Forms Grid](~/xamarin-forms/user-interface/layouts/grid.md).

## <a name="related-links"></a>Verwandte Links

- [Visualelement-API](xref:Xamarin.Forms.VisualElement)
