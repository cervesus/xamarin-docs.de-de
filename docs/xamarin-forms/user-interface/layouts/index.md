---
title: Layouts
description: Anordnen von Ansichten, auf dem Bildschirm.
ms.topic: article
ms.prod: xamarin
ms.assetid: 65030DA3-C7C1-4A02-B478-811073C39139
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/26/2017
ms.openlocfilehash: 1fe290983bf7b130dee6f1a1878a32dce3efc4c4
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="layouts"></a>Layouts

Xamarin.Forms verfügt über mehrere Layouts und Funktionen zum Strukturieren von Inhalt auf dem Bildschirm. Jedes Layoutsteuerelement wird sowie weitere Details zum Behandeln von Bildschirm Ausrichtung wird im folgenden beschrieben:

* **[StackLayout](stack-layout.md)**  &ndash; verwendet, um linear, Anordnen von Ansichten horizontal oder vertikal. Ansichten in einer StackLayout können an das Rechenzentrum, nach links oder rechts des Layouts ausgerichtet werden.
* **[AbsoluteLayout](absolute-layout.md)**  &ndash; zum Anordnen der Sichten durch Festlegen von Koordinaten & Größe als Absolute Werte oder Verhältnisse verwendet. AbsoluteLayout kann verwendet werden, überlagern Sie die Ansichten als auch auf der linken Seite, rechtsbündig oder mittig zu verankern.
* **[RelativeLayout](relative-layout.md)**  &ndash; verwendet, um Sichten zu ordnen, indem Sie Einschränkungen relativ zu ihrer übergeordneten Dimensionen & Position festlegen.
* **[Raster](grid.md)**  &ndash; verwendet, um Ansichten in einem Raster anordnen. Im Hinblick auf die absoluten Werte oder Verhältnisse können Zeilen und Spalten angegeben werden.
* **[ScrollView](scroll-view.md)**  &ndash; scrollen, wenn eine Sicht vollständig innerhalb der Grenzen des Bildschirms anpassen kann nicht bereitgestellt.
* **[LayoutOptions](layout-options.md)**  &ndash; Ausrichtung und die Erweiterung für eine Ansicht, relativ zu seinem übergeordneten definieren.
* **[Geben Sie Transparenz](#input_transparency)**  &ndash; gibt an, ob ein Element die Eingaben empfängt.
* **[Rand und Abstand](margin-and-padding.md)**  &ndash; veranschaulicht, wie Layoutverhalten steuern, wenn ein Element in der Benutzeroberfläche gerendert wird.
* **[Geräteausrichtung](device-orientation.md)**  &ndash; wird erläutert, wie Geräte Orientierung zu behandeln.
* **[Layout auf Tablets und Desktopgeräte](tablet.md)**  &ndash; wird gezeigt, wie größere Bildschirme auf jeder Plattform zu optimieren.
* **[Erstellen ein benutzerdefiniertes Layout](custom.md)**  &ndash; wird erläutert, wie eine benutzerdefiniertes Layout-Klasse zu erstellen.
* **[Layout-Komprimierung](layout-compression.md)**  &ndash; entfernt den angegebenen Layout aus der visuellen Struktur Versuch zur Verbesserung der Leistung beim Rendern der Seite.

Plattformsteuerelemente können auch verwendet werden, direkt in Xamarin.Forms Layouts mit [ **Native einbetten** ](~/xamarin-forms/platform/native-views/index.md) (neu in Xamarin.Forms 2.2), und Sie können [ **benutzerdefinierte LayoutsErstellen** ](custom.md) bestimmte Anforderungen erfüllen.

Die folgende Grafik visualisiert Layout-Steuerelemente:

[ ![](images/layouts-sml.png "Xamarin.Forms Layouts")](images/layouts.png "Xamarin.Forms Layouts")

## <a name="choosing-the-right-layout"></a>Auswählen des richtigen Layouts

Layouts in Ihrer app gewählten können helfen oder Sie beeinträchtigen, wie Sie eine ansprechend und verwendbare Xamarin.Forms-app erstellen. Dauert einige Zeit zu überlegen, wie jedes Layout Works übersichtlichere und besser skalierbare Benutzeroberflächencode zu schreiben helfen kann. Ein Bildschirm kann eine Kombination von verschiedenen Layouts zum Erreichen eines bestimmten Entwurfs haben.

### <a name="stacklayoutstack-layoutmd"></a>[StackLayout](stack-layout.md)

Die `StackLayout` dient zum Anzeigen von Sichten entlang einer Linie, die entweder horizontal oder vertikal ist. Position und Größe innerhalb des Layouts wird basierend auf einer Sicht bestimmt `HeightRequest`, `WidthRequest`, `HorizontalOptions` und `VerticalOptions`. `StackLayout` das grundlegende Layout, Anordnen von anderen Layouts auf dem Bildschirm wird häufig verwendet werden.

Wenn ein Beispiel für `StackLayout` gute Wahl, sollten Sie eine app, die zum Anzeigen einer Schaltfläche und einer Bezeichnung, mit der Bezeichnung linksbündig und die Schaltfläche "rechtsbündig" benötigt würde.

```xaml
<StackLayout Orientation="Horizontal">
  <Label HorizontalOptions="StartAndExpand" Text="Label" />
  <Button HorizontalOptions="End" Text="Button" />
</StackLayout>
```

### <a name="absolutelayoutabsolute-layoutmd"></a>[AbsoluteLayout](absolute-layout.md)

Die `AbsoluteLayout` für die Anzeige von Sichten mit Größe und Position angegeben werden, entweder als explizite Werte oder Werte relativ zur Größe des Layouts verwendet wird. Im Gegensatz zu `StackLayout` und `Grid`, `AbsoluteLayout` ermöglicht untergeordneten Ansichten überlappen. Im Gegensatz zu `RelativeLayout`, `AbsoluteLayout` können nicht außerhalb des Bildschirms Elemente anzuordnen.

Wenn ein Beispiel für `AbsoluteLayout` gute Wahl, sollten Sie eine app, die Auflistungen von Objekten als Stapel vorhanden wäre. Dies ist häufig bei der Darstellung von Alben und Fotos oder Titel. Der folgende Code bietet die Darstellung von einem Stapel mit Elementen gedreht, um den Inhalt der Stapel-Hinweis:

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

Beachten Sie die folgenden Aspekte des vorangehenden Codes ein:

- Jede `Image` wird angezeigt, in der gleichen Position (in der Mitte der horizontale Leerraum)
- Die `Padding` von berücksichtigt `AbsoluteLayout`sind im Gegensatz zum `RelativeLayout`, die ignoriert.
- `AbsoluteLayout.LayoutFlags` Gibt an, wie das Layout Grenzen interpretiert werden. In diesem Fall `PositionProportional`, bedeutet, dass die Koordinaten ein Verhältnis von der Größe des Layouts, während die Größe als eine explizite Größe interpretiert wird.
- `AbsoluteLayout.Layoutbounds` Gibt die horizontale Position, die vertikale Position, die Breite und die Höhe in dieser Reihenfolge.

### <a name="relativelayoutrelative-layoutmd"></a>[RelativeLayout](relative-layout.md)

Die `RelativeLayout` dient zum Anzeigen von Sichten mit Größe und Position relativ zu den Werten des Layouts oder einer anderen Ansicht als angegeben. Relative Werte müssen nicht übereinstimmen, er entspricht der Wert für die verknüpfte Sicht. Beispielsweise ist es möglich, einer Sicht festzulegen `Width` Eigenschaft zu einer anderen Ansicht proportional zu `X` Eigenschaft.

RelativeLayout kann verwendet werden, Benutzeroberflächen zu erstellen, das Gerät Größen proportional zu skalieren. Der folgende XAML-Code implementiert ein Design mit Feldern im obersten Ecken, mit einem Flagpole mit Flag in der Mitte an:

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

Beachten Sie die folgenden Aspekte des vorangehenden Codes ein:

- Positionen und Größen werden als Einschränkungen angegeben.
- Die Flagpole lautet, damit das Flag (grüne Box) Position relativ zu den Flagpole festgelegt werden kann.
- Die Einschränkungsausdrücke besitzen `Factor` und `Constant` Eigenschaften, die zum Definieren von Positionen und Größen als Vielfaches verwendet werden können (oder Sekundenbruchteile) der Eigenschaften anderer Objekte sowie eine Konstante. Konstanten können negativ sein.

### <a name="gridgridmd"></a>[Raster](grid.md)

Die `Grid` dient zum Anzeigen von Elementen in Zeilen und Spalten. Beachten Sie, dass das Raster keine Tabelle ist, damit sie nicht das Konzept der Zellen, Zeilen von Kopf- und Fußzeile oder Grenzen zwischen Zeilen und Spalten verfügt. Im Allgemeinen eignet sich Raster nicht zum Anzeigen von Tabellendaten. Für diese verwenden, sollten Sie eine [ListView](~/xamarin-forms/user-interface/listview/index.md) oder [TableView](~/xamarin-forms/user-interface/tableview.md).

Wenn ein Beispiel für eine `Grid` ist das richtige Layout verwenden, sollten eine numerische Eingabe für einen Rechner. Eine numerische Eingabe für eine Rechner kann aus vier Zeilen und drei Spalten mit einer Schaltfläche bestehen. Der folgende Code implementiert diesen Entwurf an:

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

Beachten Sie die folgenden Aspekte des vorangehenden Codes ein:

- Raster und Spalten werden explizit angegeben nicht anhand des Inhalts abgeleitet.
- `Height` und `Width` Werte festgelegt werden können, Stern, was bedeutet, dass das Raster diese Werte in den verfügbaren Platz ausfüllen festgelegt wird.
- Jede Schaltfläche Position wird angegeben, indem `Grid.Row`  &  `Grid.Column` Eigenschaften.

### <a name="layoutoptionslayout-optionsmd"></a>[LayoutOptions](layout-options.md)

Die [ `LayoutOptions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutOptions/) Struktur kann verwendet werden, um Ausrichtung und die Erweiterung für eine Ansicht, relativ zu seinem übergeordneten definieren.

### <a name="margin-and-paddingmargin-and-paddingmd"></a>[Rand und Abstand](margin-and-padding.md)

Die [ `Margin` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.Margin/) und [ `Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/) Eigenschaften Layoutverhalten steuern, wenn ein Element in der Benutzeroberfläche gerendert wird.

<a name="input_transparency" />

### <a name="input-transparency"></a>Transparenz der Eingabe

Jedes Element verfügt über eine [ `InputTransparent` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.InputTransparent/) -Eigenschaft, die verwendet wird, um zu definieren, ob das Element Eingaben empfängt. Der Standardwert ist `false`, um sicherzustellen, dass das Element Eingaben empfängt.

Wenn festgelegt ist diese Eigenschaft auf einen Containerklasse, z. B. eine Layoutklasse, dessen Wert überträgt die untergeordneten Elemente. Daher ist das Festlegen der [ `InputTransparent` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.InputTransparent/) Eigenschaft `true` in einem Layout führt die Klasse in den Elementen innerhalb des Layouts empfängt keine Eingabe.

### <a name="device-orientationdevice-orientationmd"></a>[Geräteausrichtung](device-orientation.md)

Xamarin.Forms und seine integrierten Layouts sind Änderungen in geräteausrichtung verarbeiten kann. Betrachten Sie die Ausrichtungen, die Ihre app werden sowie unterstützt wie Sie machen das dafür vorgesehene im Quer- und Hochformat Modi verwenden.

### <a name="layout-for-tablet-and-desktop-appstabletmd"></a>[Layout für Tablet und Desktop-apps](tablet.md)

iOS, Android und Windows Plattformen unterstützen alle größere Bildschirmgrößen auf tabletgeräten (sowie Laptops und Desktops für Windows). Xamarin.Forms können Sie Ihre app für größere Bildschirme zu optimieren, indem Sie erkennen, Gerätetyp und Anpassen des Seitenlayouts oder eine vollkommen andere Seite vollständig für größere Bildschirme.

### <a name="creating-a-custom-layoutcustommd"></a>[Erstellen eines benutzerdefinierten Layouts](custom.md)

Xamarin.Forms definiert vier Klassen für Layout - [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/), [ `AbsoluteLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AbsoluteLayout/), [ `RelativeLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.RelativeLayout/), und [ `Grid` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/), und jede ordnet die untergeordneten Elemente auf andere Weise. Allerdings manchmal erforderlichen zum Organisieren von Seiteninhalt, die nicht mit einem Layout gebotenen Xamarin.Forms. In diesem Artikel wird erläutert, wie eine benutzerdefiniertes Layout-Klasse schreiben, und zeigt eine Ausrichtung Akzent `WrapLayout` -Klasse, die ordnet seine untergeordneten Elemente horizontal über die Seite, und klicken Sie dann dient als Wrapper für die Anzeige von nachfolgenden untergeordneten Elemente, um zusätzliche Zeilen.

### <a name="layout-compressionlayout-compressionmd"></a>[Layout-Komprimierung](layout-compression.md)

Layout-Komprimierung entfernt angegebenen Layouts aus der visuellen Struktur, in dem Versuch zur Verbesserung der Leistung beim Rendern der Seite. Der daraus resultierende Leistungsvorteil variiert je nach Komplexität einer Seite, der Version des verwendeten Betriebssystems und des Geräts, auf dem die Anwendung ausgeführt wird. Die größten Leistungssteigerungen werden jedoch bei älteren Geräten zu verzeichnen sein.

## <a name="making-your-choice"></a>Machen Ihre Auswahl

Denken Sie daran, dass in den meisten Fällen mehr als eine Layout-Auswahl verwendet werden kann, um den gewünschten Entwurf zu implementieren. Wenn mehrere gültige Optionen sind, berücksichtigen Sie, welcher Ansatz am einfachsten für Ihre Situation.
Die meisten Entwürfe können nicht mit nur einem Layout realisiert werden, damit verschachteln Layouts als zum Erstellen von komplexeren Entwürfe benötigten.


## <a name="related-links"></a>Verwandte Links

- [Apple Human Interface-Richtlinien](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/MobileHIG)
- [Android-Design-Website](https://developer.android.com/design/index.html)
- [Layout (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)
- [BusinessTumble-Beispiel (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BusinessTumble/)
