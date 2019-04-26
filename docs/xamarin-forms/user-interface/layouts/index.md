---
title: Layouts in Xamarin.Forms
description: Xamarin.Forms verfügt über mehrere Layouts und Funktionen zum Organisieren von Inhalt auf dem Bildschirm, und dieser Artikel beschreibt diese.
ms.prod: xamarin
ms.assetid: 65030DA3-C7C1-4A02-B478-811073C39139
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 12/18/2018
ms.openlocfilehash: 5bd232293c979566faed2856de7287903da94054
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61300400"
---
# <a name="layouts-in-xamarinforms"></a>Layouts in Xamarin.Forms

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)

Xamarin.Forms verfügt über mehrere Layouts und Funktionen zum Organisieren von Inhalt auf dem Bildschirm.

> [!VIDEO https://youtube.com/embed/4HlLjTZQzjM]

**Xamarin.Forms-Layouts, [Xamarin University](https://university.xamarin.com/)**

Jedes Layoutsteuerelement ist, sowie weitere Details zur Verarbeitung von Änderungen der bildschirmausrichtung im folgenden beschrieben:

* **[StackLayout](stack-layout.md)**  : werden verwendet, linear, Anordnen von Ansichten horizontal oder vertikal. Ansichten in einem StackLayout können an das Rechenzentrum, nach links oder rechts des Layouts ausgerichtet werden.
* **[Von "AbsoluteLayout"](absolute-layout.md)**  – wird zum Anordnen von Ansichten durch Festlegen von Koordinaten und Größe in Bezug auf Absolute Werte oder Verhältnisse verwendet. Layer-Ansichten als auch auf der linken Seite, rechtsbündig oder mittig zu verankern, kann von "AbsoluteLayout" verwendet werden.
* **[RelativeLayout](relative-layout.md)**  – verwendet, um Ansichten zu anordnen, indem Sie Einschränkungen, die relativ zu ihrer übergeordneten Dimensionen & Position festlegen.
* **[Raster](grid.md)**  : werden verwendet, um Ansichten in einem Raster anzuordnen. Zeilen und Spalten können in Bezug auf Absolute Werte oder Verhältnisse angegeben werden.
* **[FlexLayout](flex-layout.md)**  : werden verwendet, um die Ansichten horizontal oder vertikal mit umbrechen anordnen.
* **[ScrollView](scroll-view.md)**  : werden verwendet, zu scrollen, wenn eine Sicht nicht vollständig innerhalb der Grenzen des Bildschirms geeignet.
* **[LayoutOptions](layout-options.md)**  – definieren, Ausrichtung und die Erweiterung für eine Ansicht, relativ zum übergeordneten Element.
* **[Geben Sie Transparenz](#input_transparency)**  – gibt an, ob ein Element Eingaben empfängt.
* **[Ränder und Abstände](margin-and-padding.md)**  – veranschaulicht die Layout-Verhalten zu steuern, wenn ein Element in der Benutzeroberfläche gerendert wird.
* **[Geräteausrichtung](device-orientation.md)**  – wird erläutert, wie Änderungen an Device-Ausrichtung zu behandeln.
* **[Layout auf Tablets und Desktopgeräte](tablet.md)**  – zeigt, wie größere Bildschirme auf jeder Plattform zu optimieren.
* **[Bindbare Layouts](bindable-layouts.md)**  – Aktivieren der Layout-Klassen, um ihre Inhalte durch Bindung an eine Auflistung von Elementen zu generieren.
* **[Erstellen eines benutzerdefinierten Layouts](custom.md)**  – wird erläutert, wie eine benutzerdefiniertes Layout-Klasse zu erstellen.
* **[Layoutkomprimierung](layout-compression.md)**  – entfernt den angegebenen Layout aus der visuellen Struktur in um die Leistung des Seitenrenderings zu verbessern.

Plattformsteuerelemente können auch verwendet werden, direkt in Xamarin.Forms-Layouts mit [ **Native Einbettung** ](~/xamarin-forms/platform/native-views/index.md) (neu in Xamarin.Forms 2.2), und Sie können [ **erstellen Sie benutzerdefinierte Layouts** ](custom.md) um bestimmte Anforderungen zu erfüllen.

Die folgende Abbildung stellt dar, die Layoutsteuerelemente:

[![](images/layouts-sml.png "Xamarin.Forms-Layouts")](images/layouts.png#lightbox "Xamarin.Forms-Layouts")

## <a name="choosing-the-right-layout"></a>Auswählen des richtigen Layouts

Die Layouts, die Sie in Ihrer app auswählen können helfen oder Sie beeinträchtigen, wie Sie eine attraktive und verwendbare Xamarin.Forms-app erstellen. Dauert einige Zeit zu überlegen, wie jedes Layout funktioniert Sie übersichtlicher und besser skalierbaren UI-Code schreiben kann. Ein Bildschirm haben eine Kombination aus verschiedenen Layouts zu einen bestimmten Entwurf zu erzielen.

### <a name="stacklayoutstack-layoutmd"></a>[StackLayout](stack-layout.md)

Die `StackLayout` wird verwendet, für die Anzeige von Ansichten auf einer Linie, die entweder horizontal oder vertikal verläuft. Position und Größe innerhalb des Layouts wird basierend auf einer Sicht bestimmt `HeightRequest`, `WidthRequest`, `HorizontalOptions` und `VerticalOptions`. `StackLayout` das grundlegende Layout, anderer Layouts auf dem Bildschirm angeordnet wird häufig verwendet werden.

Ein Beispiel dafür, wann `StackLayout` würde eine gute Wahl, sollten Sie eine app, die eine Schaltfläche und eine Bezeichnung, mit der Bezeichnung linksbündig und die Schaltfläche "rechtsbündig" angezeigt werden soll.

```xaml
<StackLayout Orientation="Horizontal">
  <Label HorizontalOptions="StartAndExpand" Text="Label" />
  <Button HorizontalOptions="End" Text="Button" />
</StackLayout>
```

### <a name="flexlayoutflex-layoutmd"></a>[FlexLayout](flex-layout.md)

Die `FlexLayout` ähnelt `StackLayout` , da es entweder horizontal oder vertikal untergeordnete Ansichten anzeigt:

```xaml
<FlexLayout Direction="Column"
            AlignItems="Center"
            JustifyContent="SpaceEvenly">

    <Label Text="FlexLayout in Action" />
    <Button Text="Button" />
    <Label Text="Another Label" />
</FlexLayout>
```

Jedoch, wenn zu viele untergeordnete Elemente in einer einzelnen Zeile oder Columm, passen `FlexLayout` kann auch diese Ansichten umschließen. `FlexLayout` basiert darauf, dass der CSS-Flexible Box-Layout-Module und verfügt über viele der gleichen integrierten Optionen zum Positionieren und Ausrichten von ihren untergeordneten Elementen.

### <a name="absolutelayoutabsolute-layoutmd"></a>[AbsoluteLayout](absolute-layout.md)

Die `AbsoluteLayout` für die Anzeige von Ansichten, mit der Größe und Position angegeben wird, entweder als explizite Werte oder Werte, die relativ zur Größe des Layouts verwendet wird. Im Gegensatz zu `StackLayout` und `Grid`, `AbsoluteLayout` ermöglicht untergeordneten Ansichten überlappen. Im Gegensatz zu `RelativeLayout`, `AbsoluteLayout` nicht, können Sie Elemente aus dem platzieren.

Ein Beispiel dafür, wann `AbsoluteLayout` würde eine gute Wahl, betrachten Sie eine app, die Auflistungen von Objekten als Stapel vorhanden. Dies wird häufig angezeigt, bei Alben und Fotos oder Titel der Präsentation. Der folgende Code gibt die Darstellung eines Stapels, mit Elementen, die als Hinweise für den Inhalt der eine Ansammlung gedreht:

In XAML:

```xaml
<AbsoluteLayout Padding="15">
  <Image AbsoluteLayout.LayoutFlags="PositionProportional" AbsoluteLayout.LayoutBounds="0.5, 0, 100, 100" Rotation="30"
    Source="bottom.png" />
  <Image AbsoluteLayout.LayoutFlags="PositionProportional" AbsoluteLayout.LayoutBounds="0.5, 0, 100, 100" Rotation="60"
    Source="middle.png" />
  <Image AbsoluteLayout.LayoutFlags="PositionProportional" AbsoluteLayout.LayoutBounds="0.5, 0, 100, 100"
    Source="cover.png" />
</AbsoluteLayout>
```

Beachten Sie die folgenden Aspekte des obigen Codes ein:

- Jede `Image` wird angezeigt, in der gleichen Position (in der Mitte der horizontale Leerraum)
- Die `Padding` von berücksichtigt `AbsoluteLayout`anders als beim `RelativeLayout`, wird er ignoriert.
- `AbsoluteLayout.LayoutFlags` Gibt an, wie die Grenzen Layout interpretiert werden. In diesem Fall `PositionProportional`, bedeutet, dass die Koordinaten ein Verhältnis von der Größe des Layouts, während die Größe als eine explizite Größe interpretiert wird.
- `AbsoluteLayout.Layoutbounds` Gibt die horizontale Position, vertikale Position, Breite und Höhe in dieser Reihenfolge an.

### <a name="relativelayoutrelative-layoutmd"></a>[RelativeLayout](relative-layout.md)

Die `RelativeLayout` wird für die Anzeige von Ansichten, mit der Größe und Position relativ zu die Werte für das Layout oder einer anderen Ansicht als angegeben verwendet. Relative Werte müssen nicht übereinstimmen, er entspricht der Wert in der Ansicht "verwandter". Als Beispiel, es ist möglich, eine Ansicht des `Width` Eigenschaft zu einer anderen Ansicht proportional `X` Eigenschaft.

RelativeLayout kann verwendet werden, um Benutzeroberflächen zu erstellen, die proportional Gerät Größe skaliert werden. Der folgende XAML implementiert ein Design mit Feldern im obersten Ecken, mit einem Flagpole mit-Flag in der Mitte:

```xaml
<RelativeLayout HorizontalOptions="FillAndExpand" VerticalOptions="FillAndExpand">
  <BoxView Color="Blue" HeightRequest="50" WidthRequest="50"
    RelativeLayout.XConstraint= "{ConstraintExpression Type=RelativeToParent, Property=Width, Factor = 0}"
    RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor = 0}" />
  <BoxView Color="Red" HeightRequest="50" WidthRequest="50"
    RelativeLayout.XConstraint= "{ConstraintExpression Type=RelativeToParent, Property=Width, Factor = .9}"
    RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor = 0}" />
  <BoxView Color="Gray" WidthRequest="15" x:Name="pole"
    RelativeLayout.HeightConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=.75}"
    RelativeLayout.XConstraint= "{ConstraintExpression Type=RelativeToParent, Property=Width, Factor = .45}"
    RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor = .25}" />
  <BoxView Color="Green"
    RelativeLayout.HeightConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=.10, Constant=10}"
    RelativeLayout.WidthConstraint="{ConstraintExpression Type=RelativeToParent,Property=Width, Factor=.2, Constant=20}"
    RelativeLayout.XConstraint= "{ConstraintExpression Type=RelativeToView, ElementName=pole, Property=X, Constant=15}"
    RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToView, ElementName=pole, Property=Y, Constant=0}" />
</RelativeLayout>
```

Beachten Sie die folgenden Aspekte des obigen Codes ein:

- Sowohl Positionen und Größen werden als Einschränkungen angegeben.
- Die Flagpole heißt so, dass das Flag (grünen Kästchen der) Position relativ zu den Flagpole festgelegt werden kann.
- Die Einschränkungsausdrücke besitzen `Factor` und `Constant` Eigenschaften, die zum Definieren von Positionen und Größen als Vielfaches verwendet werden können (oder Sekundenbruchteile) der Eigenschaften, die von anderen Objekten sowie eine Konstante. Konstanten können negativ sein.

### <a name="gridgridmd"></a>[Raster](grid.md)

Die `Grid` dient zum Anzeigen von Elementen in Zeilen und Spalten. Beachten Sie, dass im Raster keine Tabelle, damit sie nicht das Konzept der Zellen, Zeilen von Kopf- und Fußzeile oder Grenzen zwischen Zeilen und Spalten verfügt. Raster ist im Allgemeinen nicht für die Anzeige von Tabellendaten geeignet. Für verwenden, sollten Sie eine [ListView](~/xamarin-forms/user-interface/listview/index.md) oder [TableView](~/xamarin-forms/user-interface/tableview.md).

Ein Beispiel dafür, wann eine `Grid` ist das richtige Layout verwenden, sollten eine numerische Eingabe für einen Rechner. Eine numerische Eingabe für einen Rechner kann mit vier Zeilen und drei Spalten auf, beide mit einer Schaltfläche bestehen. Der folgende Code implementiert dieses Entwurfs:

```xaml
<Grid>
  <Grid.RowDefinitions>
    <RowDefinition Height="*" />
    <RowDefinition Height="*" />
    <RowDefinition Height="*" />
    <RowDefinition Height="*" />
  </Grid.RowDefinitions>

  <Grid.ColumnDefinitions>
    <ColumnDefinition Width="*" />
    <ColumnDefinition Width="*" />
    <ColumnDefinition Width="*" />
  </Grid.ColumnDefinitions>
  <Button Text="1" Grid.Row="0" Grid.Column="0" />
  <Button Text="2" Grid.Row="0" Grid.Column="1" />
  <Button Text="3" Grid.Row="0" Grid.Column="2" />
  <Button Text="4" Grid.Row="1" Grid.Column="0" />
  <Button Text="5" Grid.Row="1" Grid.Column="1" />
  <Button Text="6" Grid.Row="1" Grid.Column="2" />
  <Button Text="7" Grid.Row="2" Grid.Column="0" />
  <Button Text="8" Grid.Row="2" Grid.Column="1" />
  <Button Text="9" Grid.Row="2" Grid.Column="2" />
  <Button Text="0" Grid.Row="3" Grid.Column="1" />
  <Button Text="&lt;-" Grid.Row="3" Grid.Column="2" />
</Grid>
```

Beachten Sie die folgenden Aspekte des obigen Codes ein:

- Rastern und Spalten werden explizit angegeben nicht aus dem Inhalt abgeleitet.
- `Height` und `Width` können festgelegt werden auf den Stern, was bedeutet, dass das Raster auf diese Werte mit den verfügbaren Platz ausfüllen festgelegt wird.
- Jede Schaltfläche auf die Position wird angegeben, indem `Grid.Row`  &  `Grid.Column` Eigenschaften.

### <a name="layoutoptionslayout-optionsmd"></a>[LayoutOptions](layout-options.md)

Die [ `LayoutOptions` ](xref:Xamarin.Forms.LayoutOptions) Struktur kann verwendet werden, um die Ausrichtung und die Erweiterung für eine Ansicht, relativ zum übergeordneten Element definieren.

### <a name="margin-and-paddingmargin-and-paddingmd"></a>[Ränder und Abstände](margin-and-padding.md)

Die [ `Margin` ](xref:Xamarin.Forms.View.Margin) und [ `Padding` ](xref:Xamarin.Forms.Layout.Padding) Eigenschaften steuern die Layout-Verhalten, wenn ein Element in der Benutzeroberfläche gerendert wird.

<a name="input_transparency" />

### <a name="input-transparency"></a>Eingabe Transparenz

Jedes Element verfügt über eine [ `InputTransparent` ](xref:Xamarin.Forms.VisualElement.InputTransparent) -Eigenschaft, die verwendet wird, definieren, ob das Element Eingaben empfängt. Der Standardwert ist `false`, um sicherzustellen, dass das Element Eingaben empfängt.

Wenn festgelegt ist diese Eigenschaft auf einen Containerklasse, z. B. eine Layoutklasse, dessen Wert Übertragungen von untergeordneten Elementen. Daher ist das Festlegen der [ `InputTransparent` ](xref:Xamarin.Forms.VisualElement.InputTransparent) Eigenschaft `true` auf ein Layout Klasse führt dazu, dass die Elemente innerhalb des Layouts, die keine Eingaben empfängt.

### <a name="device-orientationdevice-orientationmd"></a>[Geräteausrichtung](device-orientation.md)

Xamarin.Forms und dessen integrierte Layouts sind Änderungen an der geräteausrichtung verarbeiten kann. Betrachten Sie die Ausrichtungen, die Ihre app ebenfalls unterstützt wird, wie Sie machen das angegebene Feld im Quer-und Hochformat verwenden.

### <a name="layout-for-tablet-and-desktop-appstabletmd"></a>[Layout für Tablet und Desktop-apps](tablet.md)

IOS-, Android- und Universal Windows Platform unterstützen alle größeren Bildschirmgrößen auf tablet-Geräte (sowie Laptops und Desktops für Windows). Xamarin.Forms können Sie Ihre app für größere Bildschirme zu optimieren, indem Sie erkennen, Gerätetyp und Anpassen des Seitenlayouts oder eine völlig andere Seite vollständig für größere Bildschirme.

### <a name="bindable-layoutsbindable-layoutsmd"></a>[Bindbare Layouts](bindable-layouts.md)

Die `BindableLayout` -Klasse ermöglicht es, jede abgeleitete Layoutklasse der [ `Layout<T>` ](xref:Xamarin.Forms.Layout`1) Klasse, um den Inhalt durch Bindung an eine Auflistung von Elementen, mit der Option, die die Darstellung der einzelnen Elemente in festlegen generieren eine [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate).

### <a name="creating-a-custom-layoutcustommd"></a>[Erstellen eines benutzerdefinierten Layouts](custom.md)

Xamarin.Forms definiert vier Klassen für Layout - [ `StackLayout` ](xref:Xamarin.Forms.StackLayout), [ `AbsoluteLayout` ](xref:Xamarin.Forms.AbsoluteLayout), [ `RelativeLayout` ](xref:Xamarin.Forms.RelativeLayout), und [ `Grid` ](xref:Xamarin.Forms.Grid), und jedes seiner untergeordneten Elemente auf andere Weise angeordnet. Allerdings manchmal erforderlichen zum Organisieren von Inhalt der Seite, die nicht mit einem Layout von Xamarin.Forms bereitgestelltes. In diesem Artikel wird erläutert, wie eine benutzerdefinierte Layout-Klasse schreiben, und zeigt eine Ausrichtung keine Unterscheidung nach Kanatyp `WrapLayout` -Klasse, ordnet die untergeordneten Elemente horizontal über die Seite und klicken Sie dann dient als Wrapper für die Anzeige von nachfolgenden untergeordneten Elemente auf zusätzliche Zeilen.

### <a name="layout-compressionlayout-compressionmd"></a>[Layoutkomprimierung](layout-compression.md)

Layoutkomprimierung werden bestimmte Layouts aus der visuellen Struktur Versuch zur Verbesserung der Leistung beim Rendern der Seite entfernt. Der daraus resultierende Leistungsvorteil variiert je nach Komplexität einer Seite, der Version des verwendeten Betriebssystems und des Geräts, auf dem die Anwendung ausgeführt wird. Die größten Leistungssteigerungen werden jedoch bei älteren Geräten zu verzeichnen sein.

## <a name="making-your-choice"></a>Und so Ihre Auswahl

Denken Sie daran, dass in den meisten Fällen mehr als eine Layout-Auswahl verwendet werden kann, Sie Ihren gewünschten Entwurf implementieren. Wenn mehrere gültige Optionen vorhanden sind, erwägen Sie, welcher Ansatz der einfachste Weg für Ihre Situation werden.
Die meisten Entwürfe gefunden mit nur einem Layout, wurde nicht damit Nest Layouts wie erforderlich, um komplexere Entwürfe zu erstellen.

## <a name="related-links"></a>Verwandte Links

- [Apple Human Interface Guidelines](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/MobileHIG)
- [Android-Design-Website](https://developer.android.com/design/index.html)
- [Layout (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)
- [BusinessTumble-Beispiel (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BusinessTumble/)
