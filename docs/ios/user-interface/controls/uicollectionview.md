---
title: Auflistungsansichten
description: "Auflistungsansichten können Inhalte mithilfe von beliebigen Layouts angezeigt werden sollen. Dateien können problemlos Raster-ähnliches Layouts ausgegeben, und unterstützt auch benutzerdefinierte Layouts erstellen."
ms.topic: article
ms.prod: xamarin
ms.assetid: F4B85F25-0CB5-4FEA-A3B5-D22FCDC81AE4
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: 716555c2456663cb2be24498348240c571849c24
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="collection-views"></a>Auflistungsansichten

_Auflistungsansichten können Inhalte mithilfe von beliebigen Layouts angezeigt werden sollen. Dateien können problemlos Raster-ähnliches Layouts ausgegeben, und unterstützt auch benutzerdefinierte Layouts erstellen._

Auflistungsansichten, zur Verfügung, in der `UICollectionView` Klasse, ein neues Konzept in iOS 6, in denen mehrere Elemente darstellen, auf dem Bildschirm mithilfe von Layouts eingeführt werden. Die Muster zum Bereitstellen von Daten für eine `UICollectionView` Elemente erstellen und diese Elemente führen Sie die gleichen Delegierung und die Daten Datenquelle in der iOS-Entwicklung häufig verwendeter Muster interagieren.

Allerdings funktionieren die Auflistungsansichten mit einem Layoutsubsystem, das unabhängig von der `UICollectionView` selbst. Aus diesem Grund kann einfach ein anderes Layout bereitstellen, einfach die Darstellung von einer Auflistungsansicht ändern.

iOS bietet eine Layoutklasse namens `UICollectionViewFlowLayout` , mit denen Aufruflistenansicht als zeilenbasierte Layouts, z. B. einer Tabelle ohne zusätzliche Arbeit erstellt werden können. Darüber hinaus können benutzerdefinierte Layouts auch erstellt werden, die eine Präsentation zu ermöglichen, können sich sicher vorstellen.

## <a name="uicollectionview-basics"></a>UICollectionView-Grundlagen

Die `UICollectionView` Klasse besteht aus drei Elementen:

-  **Zellen** – datengesteuerte Ansichten für jedes Element
-  **Zusätzliche Ansichten** – datengesteuerte Ansichten, die einen Abschnitt zugeordnet.
-  **Die Ergänzung Ansichten** – nicht-datengesteuerte Sichten erstellt, indem ein Layout

## <a name="cells"></a>Zellen

Zellen sind Objekte, die ein einzelnes Element in den Daten darstellen, die durch die Auflistungsansicht präsentiert wird. Jede Zelle ist eine Instanz der `UICollectionViewCell` -Klasse, die aus drei verschiedene Ansichten, besteht, wie in der folgenden Abbildung gezeigt:

 [ ![](uicollectionview-images/01-uicollectionviewcell.png "Jede Zelle drei verschiedene Ansichten, besteht wie hier gezeigt.")](uicollectionview-images/01-uicollectionviewcell.png)

Die `UICollectionViewCell` -Klasse verfügt über die folgenden Eigenschaften für jede dieser Ansichten:

-   `ContentView` – In dieser Ansicht enthält den Inhalt, in dem die Zelle dargestellt. Es wird in der obersten Z-Reihenfolge auf dem Bildschirm gerendert.
-   `SelectedBackgroundView` – Zellen haben integrierte Unterstützung zur Auswahl. In dieser Ansicht wird verwendet, um visuell zu kennzeichnen, dass eine Zelle ausgewählt ist. Diese gerendert direkt unterhalb der `ContentView` Wenn eine Zelle ausgewählt ist.
-   `BackgroundView` – Zellen können auch einen Hintergrund von gegeben ist Anzeigen der `BackgroundView` . In dieser Ansicht wird gerendert, unterhalb der `SelectedBackgroundView` .


Durch Festlegen der `ContentView` , dass es kleiner als ist die `BackgroundView` und `SelectedBackgroundView`, die `BackgroundView` können verwendet werden, um visuell frame-Inhalte, While der `SelectedBackgroundView` wird angezeigt, wenn eine Zelle ausgewählt wird, wie unten dargestellt:

 [ ![](uicollectionview-images/02-cells.png "Die andere Zellenelemente")](uicollectionview-images/02-cells.png)

Die Zellen in der oben dargestellten Screenshot entstehen durch Erben von `UICollectionViewCell` verwendet wird und die `ContentView`, `SelectedBackgroundView` und `BackgroundView` Eigenschaften bzw., wie im folgenden Code gezeigt:

```csharp
public class AnimalCell : UICollectionViewCell
{
        UIImageView imageView;

        [Export ("initWithFrame:")]
        public AnimalCell (CGRect frame) : base (frame)
        {
            BackgroundView = new UIView{BackgroundColor = UIColor.Orange};

            SelectedBackgroundView = new UIView{BackgroundColor = UIColor.Green};

            ContentView.Layer.BorderColor = UIColor.LightGray.CGColor;
            ContentView.Layer.BorderWidth = 2.0f;
            ContentView.BackgroundColor = UIColor.White;
            ContentView.Transform = CGAffineTransform.MakeScale (0.8f, 0.8f);

            imageView = new UIImageView (UIImage.FromBundle ("placeholder.png"));
            imageView.Center = ContentView.Center;
            imageView.Transform = CGAffineTransform.MakeScale (0.7f, 0.7f);

            ContentView.AddSubview (imageView);
        }

        public UIImage Image {
            set {
                imageView.Image = value;
            }
        }
}
```

 <a name="Supplementary_Views" />


## <a name="supplementary-views"></a>Zusätzliche Ansichten

Zusätzliche Ansichten sind Ansichten, die jedes Abschnitts des zugeordneten Informationen präsentieren einer `UICollectionView`. Zusätzliche Ansichten sind z. B. Zellen datengesteuerte. Wo Zellen die Elementdaten aus einer Datenquelle vorhanden, präsentieren zusätzliche Ansichten der Abschnitt-Daten, z. B. die Kategorien des Buchs in einer Bibliothek oder der "Genre" Musik in einer Bibliothek "Musik".

Beispielsweise konnte eine zusätzliche Ansicht verwendende an einen Header für einen bestimmten Abschnitt wie in der folgenden Abbildung gezeigt:

 [ ![](uicollectionview-images/02a-supplementary-view.png "Eine zusätzliche Ansicht verwendet, um einen Header für einen bestimmten Abschnitt vorhanden, wie hier gezeigt")](uicollectionview-images/02a-supplementary-view.png)

Um eine zusätzliche Ansicht verwenden, muss zuerst in registriert werden die `ViewDidLoad` Methode:

```csharp
CollectionView.RegisterClassForSupplementaryView (typeof(Header), UICollectionElementKindSection.Header, headerId);
```

Klicken Sie dann die Ansicht mit zurückgegeben werden muss `GetViewForSupplementaryElement`erstelltes mit `DequeueReusableSupplementaryView`, und erbt von `UICollectionReusableView`. Der folgende Codeausschnitt liefert die SupplementaryView im Screenshot oben gezeigt:

```csharp
public override UICollectionReusableView GetViewForSupplementaryElement (UICollectionView collectionView, NSString elementKind, NSIndexPath indexPath)
        {
            var headerView = (Header)collectionView.DequeueReusableSupplementaryView (elementKind, headerId, indexPath);
            headerView.Text = "Supplementary View";
            return headerView;
        }

```

Zusätzliche Ansichten sind mehr als nur Kopf- und Fußzeilen generische.
Sie können eine beliebige Stelle in der Auflistungsansicht positioniert werden und können Sichten, machen ihre Darstellung vollständig anpassbare umfassen.

 <a name="Decoration_Views" />


## <a name="decoration-views"></a>Die Ergänzung Ansichten

Ergänzung Ansichten sind rein visuelle Ansichten, die in angezeigt werden, eine `UICollectionView`. Im Gegensatz zu den Zellen und zusätzlichen Ansichten werden sie nicht von datengesteuerten. Sie werden immer in einem Layout-Unterklasse erstellt und anschließend als Layout für den Inhalt ändern können. Beispielsweise konnte eine Ergänzung-Sicht verwendet werden, zum Präsentieren von einer Hintergrundansicht, die führt einen Bildlauf durch den Inhalt in die `UICollectionView`, wie unten dargestellt:

 [ ![](uicollectionview-images/02c-decoration-view.png "Die Ergänzung Ansicht mit einem roten Hintergrund")](uicollectionview-images/02c-decoration-view.png)

 Der Codeausschnitt unten wird der Hintergrund Rot dargestellt, in den Beispielen `CircleLayout` Klasse:

 ```csharp
 public class MyDecorationView : UICollectionReusableView
    {
        [Export ("initWithFrame:")]
        public MyDecorationView (CGRect frame) : base (frame)
        {
            BackgroundColor = UIColor.Red;
        }
    }
 ```


## <a name="data-source"></a>Datenquelle

Wie bei anderen Teilen von iOS, z. B. `UITableView` und `MKMapView`, `UICollectionView` ruft seine Daten aus einer *Datenquelle*, wird die in Xamarin.iOS über verfügbar gemacht der  **`UICollectionViewDataSource`**  Klasse. Diese Klasse ist verantwortlich für das Bereitstellen von Inhalt an die `UICollectionView` wie z. B.:

-  **Zellen** – Merry `GetCell` Methode.
-  **Zusätzliche Ansichten** – Merry `GetViewForSupplementaryElement` Methode.
-  **Anzahl von Abschnitten** – Merry `NumberOfSections` Methode. Der Standardwert ist 1, wenn nicht implementiert.
-  **Anzahl der Elemente im Abschnitt** – Merry `GetItemsCount` Methode.

### <a name="uicollectionviewcontroller"></a>UICollectionViewController
Der Einfachheit halber der `UICollectionViewController` Klasse ist verfügbar. Dies wird automatisch konfiguriert, um sowohl der Delegat, der im nächsten Abschnitt erläutert wird, und die Datenquelle für die `UICollectionView` anzeigen.

Wie bei `UITableView`die `UICollectionView` Klasse wird die Datenquelle, um die Zellen für Elemente abzurufen, die auf dem Bildschirm sind nur aufrufen.
Zellen, die außerhalb des Bildschirms einen Bildlauf an eine Warteschlange für die Wiederverwendung, befinden sich wie die folgende Abbildung veranschaulicht:

 [ ![](uicollectionview-images/03-cell-reuse.png "Zellen, die außerhalb des Bildschirms einen Bildlauf an eine Warteschlange für die Wiederverwendung befinden wie hier gezeigt.")](uicollectionview-images/03-cell-reuse.png)

Wiederverwendung von Zelle wurde mit vereinfacht `UICollectionView` und `UITableView`. Sie müssen nicht mehr zum Erstellen einer Zelle direkt in der Datenquelle, wenn eine nicht verfügbar in der Warteschlange Wiederverwendung ist als der Zellen im System registriert sind. Wenn eine Zelle beim Aufruf auf die Zelle aus der Warteschlange Wiederverwendung Warteschlange nicht verfügbar ist, erstellt iOS automatisch basierend auf den Typ oder die Nib, die registriert wurde.
Das gleiche Verfahren ist auch für zusätzliche Ansichten verfügbar.

Betrachten Sie beispielsweise den folgenden Code, der registriert die `AnimalCell` Klasse:

```csharp
static NSString animalCellId = new NSString ("AnimalCell");
CollectionView.RegisterClassForCell (typeof(AnimalCell), animalCellId);
```

Wenn eine `UICollectionView` eine Zelle benötigt, da das Element auf dem Bildschirm ist die `UICollectionView` ruft dessen Datenquelle `GetCell` Methode. Ähnlich wie dies mit UITableView funktioniert, diese Methode ist verantwortlich für das Konfigurieren einer Zelle aus den Daten sichern, die eine `AnimalCell` in diesem Fall Klasse.

Der folgende Code zeigt eine Implementierung von `GetCell` zurückgibt ein `AnimalCell` Instanz:

```csharp
public override UICollectionViewCell GetCell (UICollectionView collectionView, Foundation.NSIndexPath indexPath)
{
        var animalCell = (AnimalCell)collectionView.DequeueReusableCell (animalCellId, indexPath);

        var animal = animals [indexPath.Row];

        animalCell.Image = animal.Image;

        return animalCell;
}
```

Der Aufruf von `DequeReusableCell` ist, an der die Zelle entweder aus der Warteschlange Wiederverwendung Deduplizierung in die Warteschlange eingereiht wird oder wenn eine Zelle nicht verfügbar in der Warteschlange, und erstellt basierend auf den Typ, der im Aufruf registriert ist `CollectionView.RegisterClassForCell`.

In diesem Fall durch die Registrierung der `AnimalCell` Klasse iOS erstellt eine neue `AnimalCell` intern erstellt und zurückgegeben, wenn eine Zelle Warteschlange aufgerufen wird, nach dem er mit dem Image in die Eindringens Klasse enthalten und für die Anzeige an die zurückgegebenkonfiguriertist`UICollectionView`.

 <a name="Delegate" />


### <a name="delegate"></a>delegate

Die `UICollectionView` Klasse verwendet einen Delegaten vom Typ `UICollectionViewDelegate` zur Unterstützung der Interaktion mit Inhalt in die `UICollectionView`. Dies ermöglicht eine Steuerung der:

-  **Auswahl der Zelle** – bestimmt, ob eine Zelle ausgewählt ist.
-  **Hervorhebung der Zelle** – bestimmt, ob eine Zelle derzeit betroffen sind.
-  **Menüs Zelle** – Menü, das für eine Zelle in der Antwort auf eine lange drücken Geste angezeigt.


Wie bei der Datenquelle, die `UICollectionViewController` ist standardmäßig so konfiguriert, als der Delegat für die `UICollectionView`.

 <a name="Cell_HighLighting" />


#### <a name="cell-highlighting"></a>Hervorhebung der Zelle

Wenn eine Zelle, wird die Zelle Übergänge in den markierten Status gedrückt wird und es nicht, bis aktiviert ist hebt die Benutzer ihre Finger aus der Zelle. Dies ermöglicht eine temporäre Änderung in die Darstellung der Zelle, bevor er tatsächlich ausgewählt wird. Bei Auswahl der Zelle des `SelectedBackgroundView` wird angezeigt. Die folgende Abbildung zeigt den markierten Status unmittelbar vor die Auswahl tritt auf:

 [ ![](uicollectionview-images/04-cell-highlight.png "Diese Abbildung zeigt den markierten Zustand, unmittelbar vor die Auswahl tritt auf")](uicollectionview-images/04-cell-highlight.png)

Hervorhebung, implementieren die `ItemHighlighted` und `ItemUnhighlighted` Methoden die `UICollectionViewDelegate` kann verwendet werden. Beispielsweise gilt der folgende Code einen gelben Hintergrund von der `ContentView` Wenn die Zelle markiert ist, und einen weißen Hintergrund, wenn nicht hervorgehoben, wie in der Abbildung oben gezeigt:

```csharp
public override void ItemHighlighted (UICollectionView collectionView, NSIndexPath indexPath)
{
        var cell = collectionView.CellForItem(indexPath);
        cell.ContentView.BackgroundColor = UIColor.Yellow;
}

public override void ItemUnhighlighted (UICollectionView collectionView, NSIndexPath indexPath)
{
        var cell = collectionView.CellForItem(indexPath);
        cell.ContentView.BackgroundColor = UIColor.White;
}
```

 <a name="Disabling_Selection" />


#### <a name="disabling-selection"></a>Deaktivieren der Auswahl

Auswahl ist standardmäßig aktiviert, in `UICollectionView`. Um die Auswahl zu deaktivieren, überschreiben `ShouldHighlightItem` und "false", wie unten gezeigt zurückgeben:

```csharp
public override bool ShouldHighlightItem (UICollectionView collectionView, NSIndexPath indexPath)
{
        return false;
}
```

Wenn Hervorhebung deaktiviert ist, wird der Prozess der Auswahl einer Zelle ebenfalls deaktiviert. Darüber hinaus steht auch ein `ShouldSelectItem` -Methode, die Auswahl direkt, obwohl steuert Wenn `ShouldHighlightItem` implementiert ist, und gibt "false" `ShouldSelectItem` wird nicht aufgerufen.

 `ShouldSelectItem` ermöglicht die Auswahl auf aktiviert oder deaktiviert werden, auf Basis von Element, wenn `ShouldHighlightItem` ist nicht implementiert. Sie können außerdem Hervorhebung ohne Auswahl, wenn `ShouldHighlightItem` implementiert ist, und gibt true zurück, während `ShouldSelectItem` "false" zurückgegeben.

 <a name="Cell_Menus" />


#### <a name="cell-menus"></a>Zelle Menüs

Jede Zelle in einer `UICollectionView` kann zeigt ein Menü, das Ausschneiden, kopieren und einfügen, um optional unterstützt werden kann. So erstellen Sie ein Bearbeitungsmenü auf einer Zelle

1.  Überschreiben Sie `ShouldShowMenu` und Zurückgeben von true, wenn das Element ein Menü angezeigt werden sollen.
1.  Überschreiben Sie `CanPerformAction` und gibt true zurück, für jede Aktion, die das Element ausführen können, werden die Ausschneiden, kopieren oder einfügen.
1.  Überschreiben Sie `PerformAction` kopieren Sie zum Ausführen der Bearbeitung des Einfügevorgangs.


Der folgende Screenshot Menü anzeigen, wenn eine Zelle lange gedrückt wird:

 [ ![](uicollectionview-images/04a-menu.png "Diesem Screenshot Menü anzeigen, wenn eine Zelle lange gedrückt wird")](uicollectionview-images/04a-menu.png)

 <a name="Layout" />


## <a name="layout"></a>Layout

`UICollectionView` unterstützt ein Layoutsystem, die ermöglicht, die Positionierung der alle Elemente, Zellen, zusätzliche Ansichten und Decoration-Ansichten zu verwaltende unabhängig von der `UICollectionView` selbst.
Verwenden das Layoutsystem, kann eine Anwendung Layouts wie Raster-ähnliches unterstützen, wir in diesem Artikel kennen gelernt haben, als auch benutzerdefinierte Layouts bereitzustellen.

 <a name="Layout_Basics" />


### <a name="layout-basics"></a>Layout-Grundlagen

Layouts in einem `UICollectionView` werden in einer Klasse, die von erben definiert `UICollectionViewLayout`. Die Implementierung Layout ist verantwortlich für das Erstellen der Layoutattribute für jedes Element in der `UICollectionView`. Es gibt zwei Möglichkeiten, ein Layout zu erstellen:

-  Verwenden Sie die integrierte `UICollectionViewFlowLayout` .
-  Geben Sie ein benutzerdefiniertes Layout durch Vererben von `UICollectionViewLayout` .


 <a name="Flow_Layout" />


### <a name="flow-layout"></a>Flusslayout

Die `UICollectionViewFlowLayout` Klasse stellt ein linienbasiertes Layout für das Anordnen von Inhalt in einem Raster von Zellen, wie wir gesehen haben, geeignet.

Ein Flusslayout verwenden zu können:

-  Erstellen Sie eine Instanz des `UICollectionViewFlowLayout` :


```csharp
var layout = new UICollectionViewFlowLayout ();
```

-  Übergeben Sie die Instanz an den Konstruktor der `UICollectionView` :


```csharp
simpleCollectionViewController = new SimpleCollectionViewController (layout);
```

Das ist alles, die Layout von Inhalt in einem Raster benötigt wird. Auch wenn die Ausrichtung ändert, ändert der `UICollectionViewFlowLayout` behandelt, wie unten dargestellt, entsprechend neu anordnen des Inhalts:

 [ ![](uicollectionview-images/05-layout-orientation.png "Beispiel für die Ausrichtung-Änderungen")](uicollectionview-images/05-layout-orientation.png)

 <a name="Section_Inset" />


#### <a name="section-inset"></a>Abschnitt Inset

Einige Platz um Bereitstellen der `UIContentView`, Layouts haben eine `SectionInset` Eigenschaft vom Typ `UIEdgeInsets`. Der folgende Code enthält z. B. einen 50 Pixel Puffer auf, um jedes Abschnitts des der `UIContentView` bei von angelegt eine `UICollectionViewFlowLayout`:

```csharp
var layout = new UICollectionViewFlowLayout ();
layout.SectionInset = new UIEdgeInsets (50,50,50,50);
```

Daraus ergibt sich der Abstand, um den Abschnitt, wie unten dargestellt:

 [ ![](uicollectionview-images/06-sectioninset.png "Um den Abschnitt Abstände, wie hier gezeigt.")](uicollectionview-images/06-sectioninset.png)

 <a name="Subclassing_UICollectionViewFlowLayout" />


#### <a name="subclassing-uicollectionviewflowlayout"></a>Erstellen von Unterklassen für UICollectionViewFlowLayout

In der Edition mit `UICollectionViewFlowLayout` direkt, sie können auch werden als Unterklasse um das Layout des Inhalts entlang einer Linie noch weiter anzupassen. Dies kann beispielsweise verwendet werden, zum Erstellen eines Layouts, das die Zellen in einem Raster wird nicht umbrochen, sondern erstellt eine einzelne Zeile mit einem horizontalen Bildlauf Effekt, wie unten dargestellt:

 [ ![](uicollectionview-images/07-line-layout.png "Eine einzelne Zeile mit einem horizontalen Bildlauf Effekt")](uicollectionview-images/07-line-layout.png)

Zur Implementierung dieser Funktion durch die Erstellung von Unterklassen von `UICollectionViewFlowLayout` erfordert:

-  Initialisieren alle Layouteigenschaften, die für das Layout selbst gelten oder alle Elemente im Layout im Konstruktor.
-  Überschreiben `ShouldInvalidateLayoutForBoundsChange` , Rückgabe "true", bei Grenzen des der `UICollectionView` ändert das Layout der Zellen werden neu berechnet. Hiermit wird den Code in diesem Fall sicherstellen, für der Mittelwert Zelle angewendeten Transformation während des Bildlaufs angewendet werden.
-  Überschreiben von `TargetContentOffset` den Mittelwert zu Ausrichten an der Mitte der Zelle die `UICollectionView` als Durchführen eines Bildlaufs beendet.
-  Überschreiben von `LayoutAttributesForElementsInRect` zurückzugebenden ein Array von `UICollectionViewLayoutAttributes` . Jede `UICollectionViewLayoutAttribute` enthält Informationen zum Layout das entsprechende Element, einschließlich Eigenschaften wie z. B. seine `Center` , `Size` , `ZIndex` und `Transform3D` .


Der folgende Code zeigt eine solche Implementierung:

```csharp
using System;
using CoreGraphics;
using Foundation;
using UIKit;
using CoreGraphics;
using CoreAnimation;

namespace SimpleCollectionView
{
    public class LineLayout : UICollectionViewFlowLayout
    {
        public const float ITEM_SIZE = 200.0f;
        public const int ACTIVE_DISTANCE = 200;
        public const float ZOOM_FACTOR = 0.3f;

        public LineLayout ()
        {
            ItemSize = new CGSize (ITEM_SIZE, ITEM_SIZE);
            ScrollDirection = UICollectionViewScrollDirection.Horizontal;
            SectionInset = new UIEdgeInsets (400,0,400,0);
            MinimumLineSpacing = 50.0f;
        }

        public override bool ShouldInvalidateLayoutForBoundsChange (CGRect newBounds)
        {
            return true;
        }

        public override UICollectionViewLayoutAttributes[] LayoutAttributesForElementsInRect (CGRect rect)
        {
            var array = base.LayoutAttributesForElementsInRect (rect);
            var visibleRect = new CGRect (CollectionView.ContentOffset, CollectionView.Bounds.Size);

            foreach (var attributes in array) {
                if (attributes.Frame.IntersectsWith (rect)) {
                    float distance = (float)(visibleRect.GetMidX () - attributes.Center.X);
                    float normalizedDistance = distance / ACTIVE_DISTANCE;
                    if (Math.Abs (distance) < ACTIVE_DISTANCE) {
                        float zoom = 1 + ZOOM_FACTOR * (1 - Math.Abs (normalizedDistance));
                        attributes.Transform3D = CATransform3D.MakeScale (zoom, zoom, 1.0f);
                        attributes.ZIndex = 1;
                    }
                }
            }
            return array;
        }

        public override CGPoint TargetContentOffset (CGPoint proposedContentOffset, CGPoint scrollingVelocity)
        {
            float offSetAdjustment = float.MaxValue;
            float horizontalCenter = (float)(proposedContentOffset.X + (this.CollectionView.Bounds.Size.Width / 2.0));
            CGRect targetRect = new CGRect (proposedContentOffset.X, 0.0f, this.CollectionView.Bounds.Size.Width, this.CollectionView.Bounds.Size.Height);
            var array = base.LayoutAttributesForElementsInRect (targetRect);
            foreach (var layoutAttributes in array) {
                float itemHorizontalCenter = (float)layoutAttributes.Center.X;
                if (Math.Abs (itemHorizontalCenter - horizontalCenter) < Math.Abs (offSetAdjustment)) {
                    offSetAdjustment = itemHorizontalCenter - horizontalCenter;
                }
            }
            return new CGPoint (proposedContentOffset.X + offSetAdjustment, proposedContentOffset.Y);
        }

    }
}
```

 <a name="Custom_Layout" />


### <a name="custom-layout"></a>Benutzerdefinierte Layouts

Zusätzlich zur Verwendung von `UICollectionViewFlowLayout`, Layouts auch vollständig angepasst werden durch erben direkt von `UICollectionViewLayout`.

Die wichtigsten Methoden überschrieben werden:

-   `PrepareLayout` – Verwendet zum Ausführen des anfänglichen geometrischer Berechnungen, die während des Prozesses Layout verwendet werden.
-   `CollectionViewContentSize` – Gibt die Größe des Bereichs verwendet, um Inhalt anzuzeigen.
-   `LayoutAttributesForElementsInRect` – Wie im weiter oben dargestellten Beispiel UICollectionViewFlowLayout diese Methode verwendet wird, Informationen zum Bereitstellen der `UICollectionView` zum Layout jedes Element. Jedoch im Gegensatz zu den `UICollectionViewFlowLayout` , wenn Sie ein benutzerdefiniertes Layout zu erstellen, können Sie Elemente positionieren, aber Sie auswählen.


Beispielsweise konnte der gleiche Inhalt in einem zirkuläre Layout präsentiert werden, wie unten dargestellt:

 [ ![](uicollectionview-images/08-circle-layout.png "Eine zirkuläre benutzerdefinierte Layouts wie hier gezeigt.")](uicollectionview-images/08-circle-layout.png)

Das leistungsstarke, was zu Layouts ist, dass aus dem Raster-ähnliches Layout in einem horizontalen Bildlauf Layout zu ändern und anschließend zu dieser zirkuläre Layout erfordert nur die Layoutklasse, die bereitgestellt, um die `UICollectionView` geändert werden. "Nothing" in der `UICollectionView`, dessen Delegaten oder Datenquellen codeänderungen überhaupt.


## <a name="changes-in-ios-9"></a>Änderungen in iOS 9


In iOS 9, die Auflistungsansicht (`UICollectionView`) unterstützt das Ziehen Sie jetzt neuanordnung von Elementen ausgegeben, durch Hinzufügen einer neuen Standard Geste Erkennung und mehrere neue unterstützender Methoden.

Verwenden diese neuen Methoden, können Sie einfach implementieren ziehen, um in der Auflistungsansicht neu anordnen und haben die Möglichkeit zum Anpassen der Darstellung der Elemente jeder Phase des Prozesses neu anordnen.

[ ![](uicollectionview-images/intro01.png "Ein Beispiel für den Prozess neu anordnen")](uicollectionview-images/intro01.png)

In diesem Artikel führen wir einen Blick auf das Implementieren von Drag-zu-anordnen in einer Anwendung Xamarin.iOS sowie einige andere Änderungen, die dem Steuerelement Auflistung iOS 9 vereinbart hat:

- [Einfache Neuanordnung von Elementen](#Easy-Reordering-of-Items)
    - [Einfaches Beispiel für die neu anordnen](#Simple-Reordering-Example)
    - [Mithilfe einer benutzerdefinierten Gestenhandler-Erkennung](#Using-a-Custom-Gesture-Recognizer)
    - [Benutzerdefinierte Layouts und Neuanordnen von](#Custom-Layouts-and-Reording)
- [Sammlungsänderungen-Sicht](#Collection-View-Changes)

<a name="Easy-Reordering-of-Items" />

## <a name="reordering-of-items"></a>Neuanordnung von Elementen

Wie bereits erwähnt, war eines der wichtigsten Änderungen an der Auflistungsansicht iOS 9 das Hinzufügen von einfache Drag-anordnen-Funktionalität ausgegeben.

In iOS 9, die schnellste Möglichkeit zum Hinzufügen zu einer Auflistungsansicht neuanordnung ist die Verwendung einer `UICollectionViewController`.
Der Auflistung View-Controller verfügt jetzt über eine `InstallsStandardGestureForInteractiveMovement` -Eigenschaft, die einen Standard fügt *Gestenhandler Erkennungsmodul* , unterstützt, ziehen Elemente in der Auflistung neu anordnen.
Da der Standardwert ist `true`, haben nur implementieren die `MoveItem` Methode der `UICollectionViewDataSource` zur Unterstützung von Drag &-Anordnen-Klasse. Zum Beispiel:

```csharp
public override void MoveItem (UICollectionView collectionView, NSIndexPath sourceIndexPath, NSIndexPath destinationIndexPath)
{
    // Reorder our list of items
    ...
}
```
<a name="Simple-Reordering-Example" />

### <a name="simple-reordering-example"></a>Einfaches Beispiel für die neu anordnen

Starten Sie ein neues Xamarin.iOS-Projekt als ein kurzes Beispiel, und Bearbeiten der **Main.storyboard** Datei. Ziehen Sie eine `UICollectionViewController` auf die Entwurfsoberfläche:

[ ![](uicollectionview-images/quick01.png "Hinzufügen einer UICollectionViewController")](uicollectionview-images/quick01.png)

Wählen Sie die Auflistungsansicht (es kann am einfachsten, dies aus der dokumentgliederung tun werden). Legen Sie die folgenden Größen in der Registerkarte "Layout" Pad Eigenschaften wie im folgenden Screenshot gezeigt:

- **Größe der Zelle**: Breite – 60 | Höhe – 60
- **Headergröße**: Breite: 0 | Höhe – 0
- **Fußbereichsgröße**: Breite: 0 | Höhe – 0
- **Min Abstand**: für Zellen – 8 | Zeilen – 8
- **Im Abschnitt Abstände**: Top – 16 | Unten – 16 | Links – 16 | Rechts – 16

[ ![](uicollectionview-images/quick04.png "Legen Sie die Auflistungsansicht Größen")](uicollectionview-images/quick04.png)

Als Nächstes bearbeiten Sie die Standard-Zelle ein:
    - Ändern Sie die Hintergrundfarbe Blau
    - Fügen Sie eine Bezeichnung, die als Titel für die Zelle hinzu.
    - Legen Sie den Bezeichner für die Wiederverwendung auf **Zelle**

[ ![](uicollectionview-images/quick02.png "Die Standardeinstellung Zelle bearbeiten")](uicollectionview-images/quick02.png)

Fügen Sie behalten die Bezeichnung, die in der Zelle zentriert wird, während sich dieser Größe ändert-Einschränkungen hinzu:

In der **Eigenschaft Pad** für die _CollectionViewCell_ und legen Sie die **Klasse** auf `TextCollectionViewCell`:

[ ![](uicollectionview-images/quick05.png "Legen Sie die Klasse den TextCollectionViewCell")](uicollectionview-images/quick05.png)

Legen Sie die **Wiederverwendbare Auflistungsansicht** auf `Cell`:

[ ![](uicollectionview-images/quick06.png "Legen Sie die Wiederverwendbare Auflistungsansicht Zelle")](uicollectionview-images/quick06.png)

Schließlich wählen Sie die Bezeichnung, und nennen Sie sie `TextLabel`:

[ ![](uicollectionview-images/quick07.png "name label TextLabel")](uicollectionview-images/quick07.png)

Bearbeiten der `TextCollectionViewCell` -Klasse und fügen Sie die folgenden Eigenschaften hinzu.:

```csharp
using System;
using Foundation;
using UIKit;

namespace CollectionView
{
    public partial class TextCollectionViewCell : UICollectionViewCell
    {
        #region Computed Properties
        public string Title {
            get { return TextLabel.Text; }
            set { TextLabel.Text = value; }
        }
        #endregion

        #region Constructors
        public TextCollectionViewCell (IntPtr handle) : base (handle)
        {
        }
        #endregion
    }
}
```

Hier die `Text` -Eigenschaft der Bezeichnung wird als Titel der Zelle verfügbar, sodass sie vom Code festgelegt werden kann.

Das Projekt eine neue C#-Klasse hinzu, und rufen sie `WaterfallCollectionSource`. Bearbeiten Sie die Datei, und stellen sie wie folgt aussehen:

```csharp
using System;
using Foundation;
using UIKit;
using System.Collections.Generic;

namespace CollectionView
{
    public class WaterfallCollectionSource : UICollectionViewDataSource
    {
        #region Computed Properties
        public WaterfallCollectionView CollectionView { get; set;}
        public List<int> Numbers { get; set; } = new List<int> ();
        #endregion

        #region Constructors
        public WaterfallCollectionSource (WaterfallCollectionView collectionView)
        {
            // Initialize
            CollectionView = collectionView;

            // Init numbers collection
            for (int n = 0; n < 100; ++n) {
                Numbers.Add (n);
            }
        }
        #endregion

        #region Override Methods
        public override nint NumberOfSections (UICollectionView collectionView) {
            // We only have one section
            return 1;
        }

        public override nint GetItemsCount (UICollectionView collectionView, nint section) {
            // Return the number of items
            return Numbers.Count;
        }

        public override UICollectionViewCell GetCell (UICollectionView collectionView, NSIndexPath indexPath)
        {
            // Get a reusable cell and set {~~it's~>its~~} title from the item
            var cell = collectionView.DequeueReusableCell ("Cell", indexPath) as TextCollectionViewCell;
            cell.Title = Numbers [(int)indexPath.Item].ToString();

            return cell;
        }

        public override bool CanMoveItem (UICollectionView collectionView, NSIndexPath indexPath) {
            // We can always move items
            return true;
        }

        public override void MoveItem (UICollectionView collectionView, NSIndexPath sourceIndexPath, NSIndexPath destinationIndexPath)
        {
            // Reorder our list of items
            var item = Numbers [(int)sourceIndexPath.Item];
            Numbers.RemoveAt ((int)sourceIndexPath.Item);
            Numbers.Insert ((int)destinationIndexPath.Item, item);
        }
        #endregion
    }
}
```

Diese Klasse wird die Datenquelle für unsere Auflistungsansicht werden und geben Sie die Informationen für jede Zelle in der Auflistung.
Beachten Sie, dass die `MoveItem` Methode wird implementiert, um das Ziehen von Elementen in der Auflistung neu angeordneten zulassen.

Das Projekt eine andere neue C#-Klasse hinzu, und rufen sie `WaterfallCollectionDelegate`. Bearbeiten Sie diese Datei, und stellen sie wie folgt aussehen:

```csharp
using System;
using Foundation;
using UIKit;
using System.Collections.Generic;

namespace CollectionView
{
    public class WaterfallCollectionDelegate : UICollectionViewDelegate
    {
        #region Computed Properties
        public WaterfallCollectionView CollectionView { get; set;}
        #endregion

        #region Constructors
        public WaterfallCollectionDelegate (WaterfallCollectionView collectionView)
        {

            // Initialize
            CollectionView = collectionView;

        }
        #endregion

        #region Overrides Methods
        public override bool ShouldHighlightItem (UICollectionView collectionView, NSIndexPath indexPath) {
            // Always allow for highlighting
            return true;
        }

        public override void ItemHighlighted (UICollectionView collectionView, NSIndexPath indexPath)
        {
            // Get cell and change to green background
            var cell = collectionView.CellForItem(indexPath);
            cell.ContentView.BackgroundColor = UIColor.FromRGB(183,208,57);
        }

        public override void ItemUnhighlighted (UICollectionView collectionView, NSIndexPath indexPath)
        {
            // Get cell and return to blue background
            var cell = collectionView.CellForItem(indexPath);
            cell.ContentView.BackgroundColor = UIColor.FromRGB(164,205,255);
        }
        #endregion
    }
}
```

Dies fungiert als der Delegat für unsere Auflistungsansicht. Methoden wurden überschrieben, um eine Zelle zu markieren, die der Benutzer in der Auflistungsansicht mit ihm interagiert.

Das Projekt eine letzte C#-Klasse hinzu, und rufen sie `WaterfallCollectionView`. Bearbeiten Sie diese Datei, und stellen sie wie folgt aussehen:

```csharp
using System;
using UIKit;
using System.Collections.Generic;
using Foundation;

namespace CollectionView
{
    [Register("WaterfallCollectionView")]
    public class WaterfallCollectionView : UICollectionView
    {

        #region Constructors
        public WaterfallCollectionView (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void AwakeFromNib ()
        {
            base.AwakeFromNib ();

            // Initialize
            DataSource = new WaterfallCollectionSource(this);
            Delegate = new WaterfallCollectionDelegate(this);

        }
        #endregion
    }
}
```

Beachten Sie, dass `DataSource` und `Delegate` , der oben erstellten werden festgelegt, wenn die Auflistungsansicht durch das Storyboard erstellt wird (oder **.xib** Datei).

Bearbeiten der **Main.storyboard** Datei erneut, und wählen Sie die Auflistungsansicht, und wechseln Sie zur der **Eigenschaften**. Legen Sie die **Klasse** der benutzerdefinierten `WaterfallCollectionView` -Klasse, die wir oben definiert:

Speichern Sie die Änderungen, die Sie auf der Benutzeroberfläche vorgenommen, und führen Sie die app.
Wenn der Benutzer ein Element aus der Liste wählt und es in einem neuen Speicherort zieht, werden die anderen Elemente automatisch animieren, wie sie aus dem Weg des Elements verschoben.
Wenn der Benutzer das Element an einer neuen Position ablegt, wird es zu diesem Speicherort vorhanden sein. Zum Beispiel:

[ ![](uicollectionview-images/intro01.png "Ein Beispiel für ein Element an eine neue Position ziehen.")](uicollectionview-images/intro01.png)

<a name="Using-a-Custom-Gesture-Recognizer" />

### <a name="using-a-custom-gesture-recognizer"></a>Mithilfe einer benutzerdefinierten Gestenhandler-Erkennung

In Fällen, in denen Sie können keine `UICollectionViewController` und muss eine reguläre `UIViewController`, oder wenn Sie mehr Kontrolle über die Drag-and-Drop-Aktion durchführen möchten, erstellen Sie eine eigene benutzerdefinierte Geste Erkennung und die Auflistungsansicht hinzufügen, wenn die Sicht laden. Zum Beispiel:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // Create a custom gesture recognizer
    var longPressGesture = new UILongPressGestureRecognizer ((gesture) => {

        // Take action based on state
        switch(gesture.State) {
        case UIGestureRecognizerState.Began:
            var selectedIndexPath = CollectionView.IndexPathForItemAtPoint(gesture.LocationInView(View));
            if (selectedIndexPath !=null) {
                CollectionView.BeginInteractiveMovementForItem(selectedIndexPath);
            }
            break;
        case UIGestureRecognizerState.Changed:
            CollectionView.UpdateInteractiveMovementTargetPosition(gesture.LocationInView(View));
            break;
        case UIGestureRecognizerState.Ended:
            CollectionView.EndInteractiveMovement();
            break;
        default:
            CollectionView.CancelInteractiveMovement();
            break;
        }

    });

    // Add the custom recognizer to the collection view
    CollectionView.AddGestureRecognizer(longPressGesture);
}
```

Hier verwenden wir mehrere neue Methoden, die die Auflistungsansicht hinzugefügt zu implementieren und Steuern des Ziehvorgangs:

 - `BeginInteractiveMovementForItem` -Kennzeichnet den Beginn eines Verschiebevorgangs.
 - `UpdateInteractiveMovementTargetPosition` -Wird gesendet, als Speicherort für das Element aktualisiert wird.
 - `EndInteractiveMovement` -Kennzeichnet das Ende verschieben eines Elements.
 - `CancelInteractiveMovement` – Den Benutzer den Vorgang abbrechen markiert.

Wenn die Anwendung ausgeführt wird, funktionieren des Ziehvorgangs wird, genau wie die Standardeinstellung ziehen Gestenhandler-Erkennung, die in der Auflistungsansicht enthalten ist.

<a name="Custom-Layouts-and-Reording" />

### <a name="custom-layouts-and-reordering"></a>Benutzerdefinierte Layouts und Neuanordnen von

In iOS 9 wurden mehrere neue Methoden zum Arbeiten mit Drag &-zu-neu anordnen und benutzerdefinierten Layouts in einer Auflistungsansicht hinzugefügt. Um dieses Feature zu untersuchen, fügen Sie ein benutzerdefiniertes Layout auf die Auflistung.

Fügen Sie zunächst eine neue C#-Klasse mit dem Namen `WaterfallCollectionLayout` zum Projekt. Bearbeiten Sie es, und stellen Sie es wie folgt aussehen:

```csharp
using System;
using Foundation;
using UIKit;
using System.Collections.Generic;
using CoreGraphics;

namespace CollectionView
{
    [Register("WaterfallCollectionLayout")]
    public class WaterfallCollectionLayout : UICollectionViewLayout
    {
        #region Private Variables
        private int columnCount = 2;
        private nfloat minimumColumnSpacing = 10;
        private nfloat minimumInterItemSpacing = 10;
        private nfloat headerHeight = 0.0f;
        private nfloat footerHeight = 0.0f;
        private UIEdgeInsets sectionInset = new UIEdgeInsets(0, 0, 0, 0);
        private WaterfallCollectionRenderDirection itemRenderDirection = WaterfallCollectionRenderDirection.ShortestFirst;
        private Dictionary<nint,UICollectionViewLayoutAttributes> headersAttributes = new Dictionary<nint, UICollectionViewLayoutAttributes>();
        private Dictionary<nint,UICollectionViewLayoutAttributes> footersAttributes = new Dictionary<nint, UICollectionViewLayoutAttributes>();
        private List<CGRect> unionRects = new List<CGRect>();
        private List<nfloat> columnHeights = new List<nfloat>();
        private List<UICollectionViewLayoutAttributes> allItemAttributes = new List<UICollectionViewLayoutAttributes>();
        private List<List<UICollectionViewLayoutAttributes>> sectionItemAttributes = new List<List<UICollectionViewLayoutAttributes>>();
        private nfloat unionSize = 20;
        #endregion

        #region Computed Properties
        [Export("ColumnCount")]
        public int ColumnCount {
            get { return columnCount; }
            set {
                WillChangeValue ("ColumnCount");
                columnCount = value;
                DidChangeValue ("ColumnCount");

                InvalidateLayout ();
            }
        }

        [Export("MinimumColumnSpacing")]
        public nfloat MinimumColumnSpacing {
            get { return minimumColumnSpacing; }
            set {
                WillChangeValue ("MinimumColumnSpacing");
                minimumColumnSpacing = value;
                DidChangeValue ("MinimumColumnSpacing");

                InvalidateLayout ();
            }
        }

        [Export("MinimumInterItemSpacing")]
        public nfloat MinimumInterItemSpacing {
            get { return minimumInterItemSpacing; }
            set {
                WillChangeValue ("MinimumInterItemSpacing");
                minimumInterItemSpacing = value;
                DidChangeValue ("MinimumInterItemSpacing");

                InvalidateLayout ();
            }
        }

        [Export("HeaderHeight")]
        public nfloat HeaderHeight {
            get { return headerHeight; }
            set {
                WillChangeValue ("HeaderHeight");
                headerHeight = value;
                DidChangeValue ("HeaderHeight");

                InvalidateLayout ();
            }
        }

        [Export("FooterHeight")]
        public nfloat FooterHeight {
            get { return footerHeight; }
            set {
                WillChangeValue ("FooterHeight");
                footerHeight = value;
                DidChangeValue ("FooterHeight");

                InvalidateLayout ();
            }
        }

        [Export("SectionInset")]
        public UIEdgeInsets SectionInset {
            get { return sectionInset; }
            set {
                WillChangeValue ("SectionInset");
                sectionInset = value;
                DidChangeValue ("SectionInset");

                InvalidateLayout ();
            }
        }

        [Export("ItemRenderDirection")]
        public WaterfallCollectionRenderDirection ItemRenderDirection {
            get { return itemRenderDirection; }
            set {
                WillChangeValue ("ItemRenderDirection");
                itemRenderDirection = value;
                DidChangeValue ("ItemRenderDirection");

                InvalidateLayout ();
            }
        }
        #endregion

        #region Constructors
        public WaterfallCollectionLayout ()
        {
        }

        public WaterfallCollectionLayout(NSCoder coder) : base(coder) {

        }
        #endregion

        #region Public Methods
        public nfloat ItemWidthInSectionAtIndex(int section) {

            var width = CollectionView.Bounds.Width - SectionInset.Left - SectionInset.Right;
            return (nfloat)Math.Floor ((width - ((ColumnCount - 1) * MinimumColumnSpacing)) / ColumnCount);
        }
        #endregion

        #region Override Methods
        public override void PrepareLayout ()
        {
            base.PrepareLayout ();

            // Get the number of sections
            var numberofSections = CollectionView.NumberOfSections();
            if (numberofSections == 0)
                return;

            // Reset collections
            headersAttributes.Clear ();
            footersAttributes.Clear ();
            unionRects.Clear ();
            columnHeights.Clear ();
            allItemAttributes.Clear ();
            sectionItemAttributes.Clear ();

            // Initialize column heights
            for (int n = 0; n < ColumnCount; n++) {
                columnHeights.Add ((nfloat)0);
            }

            // Process all sections
            nfloat top = 0.0f;
            var attributes = new UICollectionViewLayoutAttributes ();
            var columnIndex = 0;
            for (nint section = 0; section < numberofSections; ++section) {
                // Calculate section specific metrics
                var minimumInterItemSpacing = (MinimumInterItemSpacingForSection == null) ? MinimumColumnSpacing :
                    MinimumInterItemSpacingForSection (CollectionView, this, section);

                // Calculate widths
                var width = CollectionView.Bounds.Width - SectionInset.Left - SectionInset.Right;
                var itemWidth = (nfloat)Math.Floor ((width - ((ColumnCount - 1) * MinimumColumnSpacing)) / ColumnCount);

                // Calculate section header
                var heightHeader = (HeightForHeader == null) ? HeaderHeight :
                    HeightForHeader (CollectionView, this, section);

                if (heightHeader > 0) {
                    attributes = UICollectionViewLayoutAttributes.CreateForSupplementaryView (UICollectionElementKindSection.Header, NSIndexPath.FromRowSection (0, section));
                    attributes.Frame = new CGRect (0, top, CollectionView.Bounds.Width, heightHeader);
                    headersAttributes.Add (section, attributes);
                    allItemAttributes.Add (attributes);

                    top = attributes.Frame.GetMaxY ();
                }

                top += SectionInset.Top;
                for (int n = 0; n < ColumnCount; n++) {
                    columnHeights [n] = top;
                }

                // Calculate Section Items
                var itemCount = CollectionView.NumberOfItemsInSection(section);
                List<UICollectionViewLayoutAttributes> itemAttributes = new List<UICollectionViewLayoutAttributes> ();

                for (nint n = 0; n < itemCount; n++) {
                    var indexPath = NSIndexPath.FromRowSection (n, section);
                    columnIndex = NextColumnIndexForItem (n);
                    var xOffset = SectionInset.Left + (itemWidth + MinimumColumnSpacing) * (nfloat)columnIndex;
                    var yOffset = columnHeights [columnIndex];
                    var itemSize = (SizeForItem == null) ? new CGSize (0, 0) : SizeForItem (CollectionView, this, indexPath);
                    nfloat itemHeight = 0.0f;

                    if (itemSize.Height > 0.0f && itemSize.Width > 0.0f) {
                        itemHeight = (nfloat)Math.Floor (itemSize.Height * itemWidth / itemSize.Width);
                    }

                    attributes = UICollectionViewLayoutAttributes.CreateForCell (indexPath);
                    attributes.Frame = new CGRect (xOffset, yOffset, itemWidth, itemHeight);
                    itemAttributes.Add (attributes);
                    allItemAttributes.Add (attributes);
                    columnHeights [columnIndex] = attributes.Frame.GetMaxY () + MinimumInterItemSpacing;
                }
                sectionItemAttributes.Add (itemAttributes);

                // Calculate Section Footer
                nfloat footerHeight = 0.0f;
                columnIndex = LongestColumnIndex();
                top = columnHeights [columnIndex] - MinimumInterItemSpacing + SectionInset.Bottom;
                footerHeight = (HeightForFooter == null) ? FooterHeight : HeightForFooter(CollectionView, this, section);

                if (footerHeight > 0) {
                    attributes = UICollectionViewLayoutAttributes.CreateForSupplementaryView (UICollectionElementKindSection.Footer, NSIndexPath.FromRowSection (0, section));
                    attributes.Frame = new CGRect (0, top, CollectionView.Bounds.Width, footerHeight);
                    footersAttributes.Add (section, attributes);
                    allItemAttributes.Add (attributes);
                    top = attributes.Frame.GetMaxY ();
                }

                for (int n = 0; n < ColumnCount; n++) {
                    columnHeights [n] = top;
                }
            }

            var i =0;
            var attrs = allItemAttributes.Count;
            while(i < attrs) {
                var rect1 = allItemAttributes [i].Frame;
                i = (int)Math.Min (i + unionSize, attrs) - 1;
                var rect2 = allItemAttributes [i].Frame;
                unionRects.Add (CGRect.Union (rect1, rect2));
                i++;
            }

        }

        public override CGSize CollectionViewContentSize {
            get {
                if (CollectionView.NumberOfSections () == 0) {
                    return new CGSize (0, 0);
                }

                var contentSize = CollectionView.Bounds.Size;
                contentSize.Height = columnHeights [0];
                return contentSize;
            }
        }

        public override UICollectionViewLayoutAttributes LayoutAttributesForItem (NSIndexPath indexPath)
        {
            if (indexPath.Section >= sectionItemAttributes.Count) {
                return null;
            }

            if (indexPath.Item >= sectionItemAttributes [indexPath.Section].Count) {
                return null;
            }

            var list = sectionItemAttributes [indexPath.Section];
            return list [(int)indexPath.Item];
        }

        public override UICollectionViewLayoutAttributes LayoutAttributesForSupplementaryView (NSString kind, NSIndexPath indexPath)
        {
            var attributes = new UICollectionViewLayoutAttributes ();

            switch (kind) {
            case "header":
                attributes = headersAttributes [indexPath.Section];
                break;
            case "footer":
                attributes = footersAttributes [indexPath.Section];
                break;
            }

            return attributes;
        }

        public override UICollectionViewLayoutAttributes[] LayoutAttributesForElementsInRect (CGRect rect)
        {
            var begin = 0;
            var end = unionRects.Count;
            List<UICollectionViewLayoutAttributes> attrs = new List<UICollectionViewLayoutAttributes> ();


            for (int i = 0; i < end; i++) {
                if (rect.IntersectsWith(unionRects[i])) {
                    begin = i * (int)unionSize;
                }
            }

            for (int i = end - 1; i >= 0; i--) {
                if (rect.IntersectsWith (unionRects [i])) {
                    end = (int)Math.Min ((i + 1) * (int)unionSize, allItemAttributes.Count);
                    break;
                }
            }

            for (int i = begin; i < end; i++) {
                var attr = allItemAttributes [i];
                if (rect.IntersectsWith (attr.Frame)) {
                    attrs.Add (attr);
                }
            }

            return attrs.ToArray();
        }

        public override bool ShouldInvalidateLayoutForBoundsChange (CGRect newBounds)
        {
            var oldBounds = CollectionView.Bounds;
            return (newBounds.Width != oldBounds.Width);
        }
        #endregion

        #region Private Methods
        private int ShortestColumnIndex() {
            var index = 0;
            var shortestHeight = nfloat.MaxValue;
            var n = 0;

            // Scan each column for the shortest height
            foreach (nfloat height in columnHeights) {
                if (height < shortestHeight) {
                    shortestHeight = height;
                    index = n;
                }
                ++n;
            }

            return index;
        }

        private int LongestColumnIndex() {
            var index = 0;
            var longestHeight = nfloat.MinValue;
            var n = 0;

            // Scan each column for the shortest height
            foreach (nfloat height in columnHeights) {
                if (height > longestHeight) {
                    longestHeight = height;
                    index = n;
                }
                ++n;
            }

            return index;
        }

        private int NextColumnIndexForItem(nint item) {
            var index = 0;

            switch (ItemRenderDirection) {
            case WaterfallCollectionRenderDirection.ShortestFirst:
                index = ShortestColumnIndex ();
                break;
            case WaterfallCollectionRenderDirection.LeftToRight:
                index = ColumnCount;
                break;
            case WaterfallCollectionRenderDirection.RightToLeft:
                index = (ColumnCount - 1) - ((int)item / ColumnCount);
                break;
            }

            return index;
        }
        #endregion

        #region Events
        public delegate CGSize WaterfallCollectionSizeDelegate(UICollectionView collectionView, WaterfallCollectionLayout layout, NSIndexPath indexPath);
        public delegate nfloat WaterfallCollectionFloatDelegate(UICollectionView collectionView, WaterfallCollectionLayout layout, nint section);
        public delegate UIEdgeInsets WaterfallCollectionEdgeInsetsDelegate(UICollectionView collectionView, WaterfallCollectionLayout layout, nint section);

        public event WaterfallCollectionSizeDelegate SizeForItem;
        public event WaterfallCollectionFloatDelegate HeightForHeader;
        public event WaterfallCollectionFloatDelegate HeightForFooter;
        public event WaterfallCollectionEdgeInsetsDelegate InsetForSection;
        public event WaterfallCollectionFloatDelegate MinimumInterItemSpacingForSection;
        #endregion
    }
}
```

Dies ist möglich Klasse verwendet, um eine benutzerdefinierte zweispaltigen, Wasserfall Typlayout für die Auflistungsansicht bereitzustellen.
Der Code verwendet die Codierung von Schlüssel-Wert (über die `WillChangeValue` und `DidChangeValue` Methoden), die Datenbindung für unsere berechnete Eigenschaften in dieser Klasse zu ermöglichen.

Als Nächstes Bearbeiten der `WaterfallCollectionSource` , und stellen Sie die folgenden Änderungen und Ergänzungen:

```csharp
private Random rnd = new Random();
...

public List<nfloat> Heights { get; set; } = new List<nfloat> ();
...

public WaterfallCollectionSource (WaterfallCollectionView collectionView)
{
    // Initialize
    CollectionView = collectionView;

    // Init numbers collection
    for (int n = 0; n < 100; ++n) {
        Numbers.Add (n);
        Heights.Add (rnd.Next (0, 100) + 40.0f);
    }
}
```

Dadurch wird eine zufällige Höhe aller Elemente erstellt, die in der Liste angezeigt werden.

Als Nächstes Bearbeiten der `WaterfallCollectionView` -Klasse und fügen Sie die folgende Hilfsfunktion-Eigenschaft hinzu:

```csharp
public WaterfallCollectionSource Source {
    get { return (WaterfallCollectionSource)DataSource; }
}
```

Dadurch wird es auf Ihre Datenquelle (und die Höhe des Elements) aus dem benutzerdefinierten Layout abrufen vereinfacht.

Schließlich bearbeiten Sie die modellansichtcontroller, und fügen Sie den folgenden Code hinzu:

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    var waterfallLayout = new WaterfallCollectionLayout ();

    // Wireup events
    waterfallLayout.SizeForItem += (collectionView, layout, indexPath) => {
        var collection = collectionView as WaterfallCollectionView;
        return new CGSize((View.Bounds.Width-40)/3,collection.Source.Heights[(int)indexPath.Item]);
    };

    // Attach the custom layout to the collection
    CollectionView.SetCollectionViewLayout(waterfallLayout, false);
}
```

Dies erstellt eine Instanz des unsere benutzerdefinierte Layouts, legt das Ereignis an die Größe jedes Elements und fügt das neue Layout zu unserem Auflistungsansicht.

Wenn wir die app Xamarin.iOS erneut ausführen, wird die Auflistungsansicht jetzt wie folgt aussehen:

[ ![](uicollectionview-images/custom01.png "Die Auflistungsansicht wird nun wie folgt aussehen.")](uicollectionview-images/custom01.png)

Wir können noch Drag-zu-Elemente neu anordnen als vor, aber die Elemente ändert sich jetzt auf die Größe am neuen Speicherort anpassen, wenn sie gelöscht werden.

## <a name="collection-view-changes"></a>Sammlungsänderungen-Sicht

In den folgenden Abschnitten führen wir eine ausführliche Übersicht über die von iOS 9 für jede Klasse in der Auflistungsansicht vorgenommenen Änderungen.

### <a name="uicollectionview"></a>UICollectionView

Die folgenden Änderungen oder Erweiterungen wurden vorgenommen, um die `UICollectionView` Klasse für iOS 9:

 - `BeginInteractiveMovementForItem` – Kennzeichnet den Anfang eines Ziehvorgangs ein.
 - `CancelInteractiveMovement` – Informiert Sie die Auflistung anzeigen, dass der Benutzer einen Ziehvorgang abgebrochen wurde.
 - `EndInteractiveMovement` – Informiert Sie die Auflistung anzeigen, dass der Benutzer einen Ziehvorgang beendet wurde.
 - `GetIndexPathsForVisibleSupplementaryElements` – Gibt die `indexPath` einer Kopf-oder Fußzeile in einem Abschnitt der Auflistung anzeigen.
 - `GetSupplementaryView` – Gibt die angegebene Kopf- oder Fußzeile.
 - `GetVisibleSupplementaryViews` – Gibt eine Liste aller sichtbaren Kopf-und Fußzeilen.
 - `UpdateInteractiveMovementTargetPosition` – Informiert Sie die Auflistung anzeigen, dass der Benutzer verschoben wurde oder verschoben werden, wird ein Element während eines Ziehvorgangs.

### <a name="uicollectionviewcontroller"></a>UICollectionViewController

Die folgenden Änderungen oder Erweiterungen wurden vorgenommen, um die `UICollectionViewController` Klasse in iOS 9:

 - `InstallsStandardGestureForInteractiveMovement` – If `true` das neue Geste Erkennungsmodul, das automatisch Drag-zu-anordnen unterstützt verwendet werden.
 - `CanMoveItem` – Die Auflistungsansicht informiert, wenn ein bestimmtes Element ziehen, die neu angeordnet werden kann.
 - `GetTargetContentOffset` – Wird verwendet, um den Offset eines bestimmten sammelelements Ansicht abrufen.
 - `GetTargetIndexPathForMove` – Ruft die `indexPath` eines angegebenen Elements für einen Ziehvorgang.
 - `MoveItem` – Verschiebt die Reihenfolge der ein bestimmtes Element in der Liste.


### <a name="uicollectionviewdatasource"></a>UICollectionViewDataSource

Die folgenden Änderungen oder Erweiterungen wurden vorgenommen, um die `UICollectionViewDataSource` Klasse in iOS 9:

 - `CanMoveItem` – Die Auflistungsansicht informiert, wenn ein bestimmtes Element ziehen, die neu angeordnet werden kann.
 - `MoveItem` – Verschiebt die Reihenfolge der ein bestimmtes Element in der Liste.

### <a name="uicollectionviewdelegate"></a>UICollectionViewDelegate

Die folgenden Änderungen oder Erweiterungen wurden vorgenommen, um die `UICollectionViewDelegate` Klasse in iOS 9:

 - `GetTargetContentOffset` – Wird verwendet, um den Offset eines bestimmten sammelelements Ansicht abrufen.
 - `GetTargetIndexPathForMove` – Ruft die `indexPath` eines angegebenen Elements für einen Ziehvorgang.

### <a name="uicollectionviewflowlayout"></a>UICollectionViewFlowLayout

Die folgenden Änderungen oder Erweiterungen wurden vorgenommen, um die `UICollectionViewFlowLayout` Klasse in iOS 9:

 - `SectionFootersPinToVisibleBounds` – Die Fußzeilen im Abschnitt zu den Grenzen des sichtbaren Auflistung Ansicht sticks.
 - `SectionHeadersPinToVisibleBounds` – Die Abschnittsheader zu den Grenzen des sichtbaren Auflistung Ansicht sticks.

### <a name="uicollectionviewlayout"></a>UICollectionViewLayout

Die folgenden Änderungen oder Erweiterungen wurden vorgenommen, um die `UICollectionViewLayout` Klasse in iOS 9:

 - `GetInvalidationContextForEndingInteractiveMovementOfItems` – Gibt invalidierung Kontext am Ende eines Ziehvorgangs zurück, wenn der Benutzer den Ziehvorgang beendet oder wird abgebrochen.
 - `GetInvalidationContextForInteractivelyMovingItems` – Gibt die Ungültigkeit Kontext am Anfang eines Ziehvorgangs ein.
 - `GetLayoutAttributesForInteractivelyMovingItem` – Ruft die Attribute Layout für ein bestimmtes Element beim Ziehen eines Elements.
 - `GetTargetIndexPathForInteractivelyMovingItem` – Gibt die `indexPath` des Elements, das am angegebenen Punkt befindet, beim Ziehen eines Elements.

### <a name="uicollectionviewlayoutattributes"></a>UICollectionViewLayoutAttributes

Die folgenden Änderungen oder Erweiterungen wurden vorgenommen, um die `UICollectionViewLayoutAttributes` Klasse in iOS 9:

 - `CollisionBoundingPath` – Gibt den Pfad der Konflikt von zwei Elementen während eines Ziehvorgangs.
 - `CollisionBoundsType` – Gibt den Typ der Kollisionen (als eine `UIDynamicItemCollisionBoundsType`), die während eines Ziehvorgangs aufgetreten.

### <a name="uicollectionviewlayoutinvalidationcontext"></a>UICollectionViewLayoutInvalidationContext

Die folgenden Änderungen oder Erweiterungen wurden vorgenommen, um die `UICollectionViewLayoutInvalidationContext` Klasse in iOS 9:

 - `InteractiveMovementTarget` – Gibt des Zielelements des eines Ziehvorgangs ein.
 - `PreviousIndexPathsForInteractivelyMovingItems` – Gibt die `indexPaths` anderer Elemente beteiligt eines Drag & Vorgang neu anordnen.
 - `TargetIndexPathsForInteractivelyMovingItems` – Gibt die `indexPaths` von Elementen, die als Ergebnis einer Operation Drag-zu-anordnen neu angeordnet werden können.

### <a name="uicollectionviewsource"></a>UICollectionViewSource

Die folgenden Änderungen oder Erweiterungen wurden vorgenommen, um die `UICollectionViewSource` Klasse in iOS 9:

 - `CanMoveItem` – Die Auflistungsansicht informiert, wenn ein bestimmtes Element ziehen, die neu angeordnet werden kann.
 - `GetTargetContentOffset` – Gibt die Offsets der Elemente, die über einen Drag-anordnen-Vorgang verschoben werden.
 - `GetTargetIndexPathForMove` – Gibt die `indexPath` eines Elements, die während eines Drag-anordnen-Vorgangs verschoben werden.
 - `MoveItem` – Verschiebt die Reihenfolge der ein bestimmtes Element in der Liste.

## <a name="summary"></a>Zusammenfassung

Dieser Artikel behandelt die Änderungen zu Auflistungsansichten in iOS 9 und beschrieben, wie sie in der Xamarin.iOS implementieren.
Es fallen, implementieren eine einfache Drag-anordnen-Aktion in einer Auflistungsansicht; verwenden eine benutzerdefinierte Aktion Erkennung mit Drag &-zu-anordnen; und wie Drag-zu-Anordnen einer benutzerdefinierten Sammlung Ansichtslayout auswirkt.

## <a name="related-links"></a>Verwandte Links

- [iOS 9-Beispiele](https://developer.xamarin.com/samples/ios/iOS9/)
- [Collection-Viewer-Beispiel](https://developer.xamarin.com/samples/monotouch/ios9/CollectionView/)
- [SimpleCollectionView (Beispiel)](https://developer.xamarin.com/samples/SimpleCollectionView/)
- [Ereignisse, Protokolle und Delegaten](~/ios/app-fundamentals/delegates-protocols-and-events.md)
- [Arbeiten mit Tabellen und Zellen](~/ios/user-interface/controls/tables/index.md)
