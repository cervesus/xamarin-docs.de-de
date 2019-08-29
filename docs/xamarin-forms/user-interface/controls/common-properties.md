---
title: Allgemeine xamarin. Forms-Steuerelement Eigenschaften,-Methoden und-Ereignisse
description: In diesem Artikel werden allgemeine Eigenschaften, Methoden und Ereignisse beschrieben, die für die visualelement-Klasse definiert sind und häufig in abgeleiteten Klassen verwendet werden.
ms.prod: xamarin
ms.assetId: 85A0CCF5-C1D8-40BB-927F-A4D944E5534D
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 08/21/2019
ms.openlocfilehash: c487442af7df4e4b8dc8860dcea4cd6065087a7f
ms.sourcegitcommit: 3d21bb1a6d9b78b65aa49917b545c39d44aa3e3c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2019
ms.locfileid: "70075656"
---
# <a name="xamarinforms-common-control-properties-methods-and-events"></a>Allgemeine xamarin. Forms-Steuerelement Eigenschaften,-Methoden und-Ereignisse

Die xamarin. Forms `VisualElement` -Klasse ist die Basisklasse für die meisten Steuerelemente, die in einer xamarin. Forms-Anwendung verwendet werden. Die `VisualElement` -Klasse definiert viele [Eigenschaften](#properties), [Methoden](#methods)und [Ereignisse](#events) , die in abgeleiteten Klassen verwendet werden.

## <a name="properties"></a>Eigenschaften

Die folgenden Eigenschaften sind in `VisualElement` -Instanzen verfügbar. Eine umfassende Liste finden Sie in den [visualelement-API-Eigenschaften](xref:Xamarin.Forms.VisualElement#properties).

### [`AnchorX`](xref:Xamarin.Forms.VisualElement.AnchorX)

Die `AnchorX` -Eigenschaft ist `double` ein Wert, der den Mittelpunkt auf der X-Achse für Transformationen wie Skalierung und Drehung definiert. Der Standardwert ist 0,5.

### [`AnchorY`](xref:Xamarin.Forms.VisualElement.AnchorY)

Die `AnchorY` -Eigenschaft ist `double` ein Wert, der den Mittelpunkt auf der X-Achse für Transformationen wie Skalierung und Drehung definiert. Der Standardwert ist 0,5.

### [`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor)

Die `BackgroundColor` -Eigenschaft ist `Color` eine, die die Hintergrundfarbe des Steuer Elements bestimmt. Wenn der Wert nicht festgelegt ist, ist der `Color` Hintergrund das Standard Objekt, das als transparent gerendert wird.

### [`Behaviors`](xref:Xamarin.Forms.VisualElement.Behaviors)

Die `Behaviors` -Eigenschaft ist `List` eine `Behavior` von-Objekten. Mithilfe von Verhalten können Sie wiederverwendbare Funktionen an Elemente anfügen, `Behaviors` indem Sie Sie der Liste hinzufügen. Weitere Informationen `Behavior` zur-Klasse finden Sie unter [xamarin. Forms-Verhalten](~/xamarin-forms/app-fundamentals/behaviors/index.md).

### [`Bounds`](xref:Xamarin.Forms.VisualElement.Bounds)

Die `Bounds` -Eigenschaft ist ein Schreib geschütztes `Rectangle` -Objekt, das den vom-Steuerelement belegten Bereich darstellt. Der `Bounds` Eigenschafts Wert wird während des layoutcycle zugewiesen. `Rectangle` Die`struct` enthält nützliche Eigenschaften und Methoden zum Testen der Schnittmenge und Kapselung von Rechtecke. Weitere Informationen finden Sie in der [xamarin. Forms-Rechteck-API](xref:Xamarin.Forms.Rectangle).

### [`Effects`](xref:Xamarin.Forms.Element.Effects)

Die `Effects` -Eigenschaft ist `List` eine `Effect` von-Objekten, die `Element`von der-Klasse (Xref: xamarin. Forms. Element) geerbt wird. Mit Effekten können native Steuerelemente angepasst werden, und Sie werden in der Regel für kleine Formatierungs Änderungen verwendet. Weitere Informationen `Effect` zur-Klasse finden Sie unter [xamarin. Forms-Effekte](~/xamarin-forms/app-fundamentals/effects/index.md).

### [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection)

Die `FlowDirection` Eigenschaft ist ein `FlowDirection` Enumerationswert. Die Fluss Richtung kann auf `MatchParent`, `LeftToRight`oder `RightToLeft` festgelegt werden und bestimmt die layoutreihenfolge und die Richtung. Die `FlowDirection` -Eigenschaft wird in der Regel zur Unterstützung von Sprachen verwendet, die von rechts nach links gelesen werden.

### [`Height`](xref:Xamarin.Forms.VisualElement.Height)

Die `Height` -Eigenschaft ist ein Schreib geschützter `double` -Wert, der die gerenderte Höhe des-Steuer Elements beschreibt. Die `Height` -Eigenschaft wird während des layoutcycle berechnet und kann nicht direkt festgelegt werden. Die Höhe eines Steuer Elements kann mithilfe der " [accesstrequest](#heightrequest)"-Eigenschaft angefordert werden.

### [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest)

Die `HeightRequest` -Eigenschaft ist `double` ein Wert, der die gewünschte Höhe des Steuer Elements bestimmt. Die absolute Höhe des Steuer Elements stimmt möglicherweise nicht mit dem angeforderten Wert identisch. Weitere Informationen finden Sie unter [Anforderungs Eigenschaften](#request-properties).

### [`InputTransparent`](xref:Xamarin.Forms.VisualElement.InputTransparent)

Die `InputTransparent` -Eigenschaft ist `bool` eine, die bestimmt, ob das Steuerelement Benutzereingaben empfängt. Der Standardwert ist `false`. Dadurch wird sichergestellt, dass das Element Eingaben empfängt. Diese Eigenschaft wird auf untergeordnete Elemente übertragen, wenn Sie festgelegt wird. Das Festlegen `InputTransparent` der- `true` Eigenschaft auf für eine Layoutklasse führt dazu, dass alle Elemente innerhalb des Layouts keine Eingaben erhalten.

### [`IsEnabled`](xref:Xamarin.Forms.VisualElement.IsEnabled)

Die `IsEnabled` -Eigenschaft ist `bool` ein Wert, der bestimmt, ob das Steuerelement auf Benutzereingaben reagiert. Der Standardwert ist `true`. Wenn diese Eigenschaft auf false festgelegt wird, wird verhindert, dass das Steuerelement Benutzereingaben akzeptiert.

### [`IsFocused`](xref:Xamarin.Forms.VisualElement.IsFocused)

Die `IsFocused` -Eigenschaft ist `bool` ein Wert, der beschreibt, ob das-Steuerelement derzeit das fokussierte Objekt ist. Das Aufrufen [`Focus`](#focus) der-Methode für das-Steuerelement `IsFocused` führt dazu, dass der Wert auf true festgelegt wird. Wenn Sie [`Unfocus`](#unfocus) die-Methode aufrufen, wird diese Eigenschaft auf false festgelegt.

### [`IsTabStop`](xref:Xamarin.Forms.VisualElement.IsTabStop)

Die `IsTabStop` -Eigenschaft ist `bool` ein Wert, der definiert, ob das Steuerelement den Fokus erhält, wenn der Benutzer durch die Steuerelemente mit der Tab-Taste bewegt wird. Wenn diese Eigenschaft false ist, hat `TabIndex` die Eigenschaft keine Auswirkung.

### [`IsVisible`](xref:Xamarin.Forms.VisualElement.IsVisible)

Die `IsVisible` -Eigenschaft ist `bool` ein Wert, der bestimmt, ob das Steuerelement gerendert wird. Steuerelemente, `IsVisible` bei denen die-Eigenschaft auf false festgelegt ist, werden nicht angezeigt, werden während des layoutlaufs nicht als Speicherplatz Berechnungen angesehen und können keine Benutzereingaben akzeptieren.

### [`MinimumHeightRequest`](xref:Xamarin.Forms.VisualElement.MinimumHeightRequest)

Die `MinimumHeightRequest` -Eigenschaft ist `double` ein Wert, der die kleinste gewünschte Höhe des Steuer Elements bestimmt. Weitere Informationen finden Sie unter [Anforderungs Eigenschaften](#request-properties).

### [`MinimumWidthRequest`](xref:Xamarin.Forms.VisualElement.MinimumWidthRequest)

Die `MinimumWidthRequest` -Eigenschaft ist `double` ein Wert, der die kleinste gewünschte Breite des Steuer Elements bestimmt. Weitere Informationen finden Sie unter [Anforderungs Eigenschaften](#request-properties).

### [`Opacity`](xref:Xamarin.Forms.VisualElement.Opacity)

Die `Opacity` -Eigenschaft ist `double` ein Wert zwischen 0 (null) und 1, der die Durchlässigkeit des Steuer Elements während des Renderings bestimmt. Der Standardwert für diese Eigenschaft ist 1,0. Werte außerhalb des Bereichs zwischen 0 und 1 werden geklammert. Die `Opacity` -Eigenschaft wird nur angewendet, [`IsVisible`](#isvisible) wenn die `true`-Eigenschaft ist. Die Deckkraft wird iterativ angewendet. Wenn ein übergeordnetes Steuerelement über 0,5 Deckkraft verfügt und dessen untergeordnetes Element über eine 0,5-Deckkraft verfügt, wird das untergeordnete Steuerelement daher mit einem effektiven Wert von 0,25 Deck Kraft Das Festlegen `Opacity` der-Eigenschaft eines Eingabe Steuer Elements auf 0 weist ein nicht definiertes Verhalten auf.

### [`Parent`](xref:Xamarin.Forms.Element.Parent)

Die `Parent`-Eigenschaft wird von der `Element`-Klasse geerbt. Diese Eigenschaft ist ein `Element` Objekt, das das übergeordnete Element des Steuer Elements ist. Die `Parent` -Eigenschaft wird in der Regel automatisch für ein Element festgelegt, wenn es als untergeordnetes Element eines anderen Elements hinzugefügt wird.

### [`Resources`](xref:Xamarin.Forms.VisualElement.Resources)

Die `Resources` -Eigenschaft ist `ResourceDictionary` eine-Instanz, die mit Schlüssel-Wert-Paaren aufgefüllt wird, die in der Regel zur Laufzeit aus XAML aufgefüllt werden. Mithilfe dieses Wörterbuchs können Anwendungsentwickler in XAML definierte Objekte sowohl zur Kompilierzeit als auch zur Laufzeit wieder verwenden. Die Schlüssel im Wörterbuch werden aus dem `x:Key` -Attribut des XAML-Tags aufgefüllt. Das-Objekt, das aus XAML erstellt wurde `ResourceDictionary` , wird für den angegebenen Schlüssel in die eingefügt. Nachdem Sie initialisiert wurde.

Weitere Informationen finden Sie unter [Ressourcen Wörterbücher](~/xamarin-forms/xaml/resource-dictionaries.md).

### [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation)

Die `Rotation` -Eigenschaft ist `double` ein Wert zwischen 0 (null) und 360, der die Drehung um die Z-Achse in Grad definiert. Der Standardwert dieser Eigenschaft ist 0. Die Drehung wird relativ zu den [`AnchorX`](#anchorx) Werten [`AnchorY`](#anchory) und angewendet.

### [`RotationX`](xref:Xamarin.Forms.VisualElement.RotationX)

Die `RotationX` -Eigenschaft ist `double` ein Wert zwischen 0 (null) und 360, der die Drehung der X-Achse in Grad definiert. Der Standardwert dieser Eigenschaft ist 0. Die Drehung wird relativ zu den [`AnchorX`](#anchorx) Werten [`AnchorY`](#anchory) und angewendet.

### [`RotationY`](xref:Xamarin.Forms.VisualElement.RotationY)

Die `RotationY` -Eigenschaft ist `double` ein Wert zwischen 0 (null) und 360, der die Drehung um die Y-Achse in Grad definiert. Der Standardwert dieser Eigenschaft ist 0. Die Drehung wird relativ zu den [`AnchorX`](#anchorx) Werten [`AnchorY`](#anchory) und angewendet.

### [`Scale`](xref:Xamarin.Forms.VisualElement.Scale)

Die `Scale` -Eigenschaft ist `double` ein Wert, der die Skalierung des Steuer Elements definiert. Der Standardwert dieser Eigenschaft ist 1,0. Die Skalierung wird relativ zu [`AnchorX`](#anchorx) den [`AnchorY`](#anchory) Werten und angewendet.

### [`ScaleX`](xref:Xamarin.Forms.VisualElement.ScaleX)

Die `ScaleX` -Eigenschaft ist `double` ein Wert, der die Skalierung des Steuer Elements auf der X-Achse definiert. Der Standardwert dieser Eigenschaft ist 1,0. Die `ScaleX` -Eigenschaft wird relativ [`AnchorX`](#anchorx) zum-Wert angewendet.

### [`ScaleY`](xref:Xamarin.Forms.VisualElement.ScaleY)

Die `ScaleY` -Eigenschaft ist `double` ein Wert, der die Skalierung des Steuer Elements auf der Y-Achse definiert. Der Standardwert dieser Eigenschaft ist 1,0. Die `ScaleY` -Eigenschaft wird relativ [`AnchorY`](#anchory) zum-Wert angewendet.

### [`Style`](xref:Xamarin.Forms.NavigableElement.Style)

Die `Style`-Eigenschaft wird von der `NavigableElement`-Klasse geerbt. Diese Eigenschaft ist eine Instanz der `Style` -Klasse. Die `Style` -Klasse enthält Trigger, Setter und Verhalten, die die Darstellung und das Verhalten von visuellen Elementen definieren. Weitere Informationen finden Sie unter [xamarin. Forms-XAML-Stile](~/xamarin-forms/user-interface/styles/xaml/index.md).

### [`StyleClass`](xref:Xamarin.Forms.NavigableElement.StyleClass)

Die `StyleClass` -Eigenschaft ist eine Liste `string` von-Objekten, die die `Style` Namen von Klassen darstellen. Diese Eigenschaft wird von der `NavigableElement`-Klasse geerbt. Die `StyleClass` -Eigenschaft ermöglicht das Anwenden mehrerer Stil Attribute auf eine `VisualElement` -Instanz. Weitere Informationen finden Sie unter [xamarin. Forms-Klassen Stil](~/xamarin-forms/user-interface/styles/xaml/style-class.md).

### [`TabIndex`](xref:Xamarin.Forms.VisualElement.TabIndex)

Die `TabIndex` -Eigenschaft ist `int` ein-Wert, der die Reihenfolge der Steuerelemente definiert, wenn die Steuerelemente mit der Tab-Taste durch Die `TabIndex` -Eigenschaft ist die Implementierung der-Eigenschaft, die `ITabStopElement` für die-Schnitt `VisualElement` Stelle definiert ist, die die-Klasse implementiert.

### [`TranslationX`](xref:Xamarin.Forms.VisualElement.TranslationX)

Die `TranslationX` -Eigenschaft ist `double` ein Wert, der die Delta Übersetzung definiert, die auf die X-Achse angewendet werden soll. Die Übersetzung wird nach dem Layout angewendet und wird in der Regel zum Anwenden von Animationen verwendet. Wenn ein Element außerhalb der Grenzen des übergeordneten Containers übersetzt wird, wird verhindert, dass Eingaben funktionieren.

Weitere Informationen finden Sie unter [Animation in xamarin. Forms](~/xamarin-forms/user-interface/animation/index.md).

### [`TranslationY`](xref:Xamarin.Forms.VisualElement.TranslationY)

Die `TranslationY` -Eigenschaft ist `double` ein Wert, der die Delta Übersetzung definiert, die auf die Y-Achse angewendet werden soll. Die Übersetzung wird nach dem Layout angewendet und wird in der Regel zum Anwenden von Animationen verwendet. Wenn ein Element außerhalb der Grenzen des übergeordneten Containers übersetzt wird, wird verhindert, dass Eingaben funktionieren.

Weitere Informationen finden Sie unter [Animation in xamarin. Forms](~/xamarin-forms/user-interface/animation/index.md).

### [`Triggers`](xref:Xamarin.Forms.VisualElement.Triggers)

Die `Triggers` -Eigenschaft ist eine schreibgeschützte `List` von `TriggerBase` -Objekten. Trigger ermöglichen Anwendungsentwicklern das Ausdrücken von Aktionen in XAML, die die visuelle Darstellung von Steuerelementen als Reaktion auf Ereignis-oder Eigenschafts Änderungen ändern. Weitere Informationen finden Sie unter [xamarin. Forms-Trigger](~/xamarin-forms/app-fundamentals/triggers.md).

### [`Visual`](xref:Xamarin.Forms.VisualElement.Visual)

Die `Visual` -Eigenschaft ist `IVisual` eine-Instanz, mit der Renderer erstellt und selektiv auf `VisualElement` -Instanzen angewendet werden können. Die `Visual` -Eigenschaft wird so festgelegt, dass Sie Ihrem übergeordneten Element entspricht. das Definieren eines Renderers für eine Komponente gilt auch für alle untergeordneten Elemente dieser Komponente. Wenn kein benutzerdefinierter Renderer für ein Steuerelement oder seine Vorgänger festgelegt ist, wird der xamarin. Forms-Standard-Renderer verwendet. Weitere Informationen finden Sie unter [xamarin. Forms Visual](~/xamarin-forms/user-interface/visual/index.md).

### [`Width`](xref:Xamarin.Forms.VisualElement.Width)

Die `Width` -Eigenschaft ist ein Schreib geschützter `double` -Wert, der die gerenderte Breite des-Steuer Elements beschreibt. Die `Width` -Eigenschaft wird während des layoutcycle berechnet und kann nicht direkt festgelegt werden. Die Breite eines Steuer Elements kann mithilfe der [widthrequest-Eigenschaft](#widthrequest)angefordert werden.

### [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest)

Die `WidthRequest` -Eigenschaft ist `double` ein Wert, der die gewünschte Breite des Steuer Elements bestimmt. Die absolute Breite des Steuer Elements stimmt möglicherweise nicht mit dem angeforderten Wert identisch. Weitere Informationen finden Sie unter [Anforderungs Eigenschaften](#request-properties).

### [`X`](xref:Xamarin.Forms.VisualElement.X)

Die `X` -Eigenschaft ist ein Schreib geschützter `double` -Wert, der die aktuelle X-Position des-Steuer Elements beschreibt.

### [`Y`](xref:Xamarin.Forms.VisualElement.Y)

Die `Y` -Eigenschaft ist ein Schreib geschützter `double` -Wert, der die aktuelle Y-Position des-Steuer Elements beschreibt.

## <a name="methods"></a>Methoden

Die folgenden Methoden sind für die `VisualElement` -Klasse verfügbar. Eine umfassende Liste finden Sie unter [visualelement-API-Methoden](xref:Xamarin.Forms.VisualElement#methods).

### [`FindByName`](xref:Xamarin.Forms.Element.FindByName*)

Die `FindByName` -Methode wird von der `Element` -Klasse geerbt und weist die folgende Signatur auf:

```csharp
public object FindByName (string name)
```

Diese Methode durchsucht alle untergeordneten Elemente nach `name` dem angegebenen Argument und gibt das Element mit dem angegebenen Namen zurück. Wenn keine Entsprechung gefunden wird `null` , wird zurückgegeben.

### [`Focus`](xref:Xamarin.Forms.VisualElement.Focus)

Die `Focus` -Methode versucht, den Fokus auf das Element festzulegen. Diese Methode hat die folgende Signatur:

```csharp
public bool Focus ()
```

Die `Focus` Methode gibt `true` zurück, wenn der Tastaturfokus erfolgreich `false` festgelegt wurde, und wenn der Methodenaufrufe nicht zu einer Fokus Änderung geführt hat. Das Element muss in der Lage sein, den Fokus zu erhalten, damit diese Methode funktioniert. Das Aufrufen `Focus` der-Methode für Elemente, die Offscreen oder nicht realisiert sind, hat ein nicht definiertes Verhalten.

### [`Unfocus`](xref:Xamarin.Forms.VisualElement.Unfocus)

Die `Unfocus` Methode versucht, den Fokus auf das Element zu entfernen. Diese Methode hat die folgende Signatur:

```csharp
public void Unfocus ()
```

Das-Element muss bereits den Fokus haben, damit diese Methode funktioniert.

## <a name="events"></a>Ereignisse

Die folgenden Ereignisse sind für die `VisualElement` -Klasse verfügbar. Eine vollständige Liste finden Sie unter [xamarin. Forms visualelement-Ereignisse](xref:Xamarin.Forms.VisualElement#events).

### [`Focused`](xref:Xamarin.Forms.VisualElement.Focused)

Das `Focused` -Ereignis wird immer dann `VisualElement` ausgelöst, wenn die Instanz den Fokus erhält. Dieses Ereignis wird nicht durch den xamarin. Forms-Stapel, sondern direkt vom systemeigenen Steuerelement empfangen. Dieses Ereignis wird vom [`IsFocused`](#isfocused) Eigenschaften Setter ausgegeben.

### [`SizeChanged`](xref:Xamarin.Forms.VisualElement.SizeChanged)

Das `SizeChanged` -Ereignis wird ausgelöst, `VisualElement` wenn sich `Width` die-Instanz `Height` oder-Eigenschaften ändern. Wenn Entwickler direkt auf die Größenänderung antworten möchten, anstatt auf das nach Änderungs Ereignis zu reagieren, sollten Sie stattdessen die [`OnSizeAllocated`](xref:Xamarin.Forms.VisualElement.OnSizeAllocated*) virtuelle Methode implementieren.

### [`Unfocused`](xref:Xamarin.Forms.VisualElement.Unfocused)

Das `Unfocused` -Ereignis wird ausgelöst, `VisualElement` wenn die-Instanz den Fokus verliert. Dieses Ereignis wird nicht durch den xamarin. Forms-Stapel, sondern direkt vom systemeigenen Steuerelement empfangen. Dieses Ereignis wird vom [`IsFocused`](#isfocused) Eigenschaften Setter ausgegeben.

## <a name="units-of-measurement"></a>Maßeinheiten

Android-, IOS-und UWP-Plattformen verfügen jeweils über unterschiedliche Maßeinheiten, die Geräte übergreifend variieren können. Xamarin. Forms verwendet eine plattformunabhängige Maßeinheit, mit der Einheiten über Geräte und Plattformen hinweg normalisiert werden. In xamarin. Forms sind 160 Einheiten pro Zoll oder 64 Einheiten pro Zentimeter vorhanden.

## <a name="request-properties"></a>Anforderungseigenschaften

Eigenschaften, deren Namen "Request" enthalten, definieren einen gewünschten Wert, der möglicherweise nicht dem tatsächlich gerenderten Wert entspricht. `HeightRequest` Kann z. b. auf 150 festgelegt werden, aber wenn das Layout nur Platz für 100 Einheiten zulässt `Height` , ist der gerenderte des Steuer Elements nur 100. Die gerenderte Größe ist von verfügbarem Speicherplatz und enthaltenen Komponenten betroffen.

## <a name="related-links"></a>Verwandte Links

* [Visualelement-API-Dokumentation](xref:Xamarin.Forms.VisualElement)
