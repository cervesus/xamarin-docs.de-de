---
title: Auflistungsansichten in Xamarin.iOS
description: Auflistungsansichten können Inhalte mithilfe von beliebigen Layouts angezeigt werden. Sie können problemlos Raster-ähnliches Layouts standardmäßig unterstützt benutzerdefinierte Layouts erstellen.
ms.prod: xamarin
ms.assetid: F4B85F25-0CB5-4FEA-A3B5-D22FCDC81AE4
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/20/2017
ms.openlocfilehash: f57f2a2bc17690b7a1e0a72c583b0e94519ca4db
ms.sourcegitcommit: 946ce514fd6575aa6b93ff24181e02a60b24b106
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2019
ms.locfileid: "58677988"
---
# <a name="collection-views-in-xamarinios"></a>Auflistungsansichten in Xamarin.iOS

_Auflistungsansichten können Inhalte mithilfe von beliebigen Layouts angezeigt werden. Sie können problemlos Raster-ähnliches Layouts standardmäßig unterstützt benutzerdefinierte Layouts erstellen._

Auflistungsansichten, verfügbar in der `UICollectionView` -Klasse sind ein neues Konzept in iOS 6, die Sie einführen, mehrere Elemente auf dem Bildschirm mithilfe von Layouts darstellen. Die Muster zum Bereitstellen von Daten für eine `UICollectionView` Elemente erstellen und diese Elemente führen Sie die gleichen Delegierung und Datenquellen-Muster, die häufig in iOS-Entwicklung verwendet interagieren.

Auflistungsansichten funktioniert jedoch mit einem Layoutsubsystem, das unabhängig von der `UICollectionView` selbst. Aus diesem Grund kann ermöglichen lediglich ein anderes Layout einfach die Darstellung von einer Auflistungsansicht ändern.

iOS bietet eine Layoutklasse namens `UICollectionViewFlowLayout` , mit der Aufruflistenansicht als zeilenbasierte Layouts z. B. einer Tabelle ohne zusätzliche Arbeit erstellt werden. Darüber hinaus können benutzerdefinierte Layouts auch erstellt werden, die jede Präsentation zu ermöglichen, die Sie sich vorstellen können.

## <a name="uicollectionview-basics"></a>UICollectionView-Grundlagen

Die `UICollectionView` Klasse besteht aus drei verschiedenen Elementen:

-  **Zellen** – datengesteuerte Views für jedes Element
-  **Zusätzliche Ansichten** – datengesteuerte Ansichten mit einem Abschnitt.
-  **Ansichten der Dekoration** – nicht-Data-driven ein Layout erstellt hat

## <a name="cells"></a>Zellen

Zellen sind Objekte, die ein einzelnes Element in der Menge der Daten darstellen, die durch die Auflistungsansicht präsentiert wird. Jede Zelle ist eine Instanz der `UICollectionViewCell` -Klasse, die aus drei verschiedene Informationsansichten besteht, wie in der folgenden Abbildung gezeigt:

 [![](uicollectionview-images/01-uicollectionviewcell.png "Jede Zelle drei verschiedene Informationsansichten besteht wie hier gezeigt.")](uicollectionview-images/01-uicollectionviewcell.png#lightbox)

Die `UICollectionViewCell` -Klasse verfügt über die folgenden Eigenschaften für jede dieser Ansichten:

-   `ContentView` – In dieser Ansicht enthält den Inhalt, den die Zelle enthält. Es wird in der obersten Z-Reihenfolge auf dem Bildschirm gerendert.
-   `SelectedBackgroundView` – Zellen haben integrierte Unterstützung für die Auswahl. In dieser Ansicht wird verwendet, um visuell zu kennzeichnen, dass eine Zelle ausgewählt ist. Es wird gerendert, direkt unterhalb der `ContentView` Wenn eine Zelle ausgewählt ist.
-   `BackgroundView` – Zellen können auch anzeigen, einen Hintergrund, das von vorgelegt wird die `BackgroundView` . Diese Ansicht gerendert wird, unter dem `SelectedBackgroundView` .


Durch Festlegen der `ContentView` so, dass er kleiner als ist die `BackgroundView` und `SelectedBackgroundView`, wird die `BackgroundView` können verwendet werden, um die frame-Inhalten, While visuell die `SelectedBackgroundView` wird angezeigt, wenn eine Zelle ausgewählt ist, wie unten dargestellt:

 [![](uicollectionview-images/02-cells.png "Die andere Zellenelemente")](uicollectionview-images/02-cells.png#lightbox)

Die Zellen im obigen Screenshot werden erstellt, durch Erben von `UICollectionViewCell` und Einstellung der `ContentView`, `SelectedBackgroundView` und `BackgroundView` Eigenschaften, bzw., wie im folgenden Code gezeigt:

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

Zusätzliche Ansichten sind Ansichten, die den gesamten zugeordneten Informationen präsentieren einer `UICollectionView`. Wie Zellen sind zusätzliche Ansichten, datengesteuerte. Wenn Zellen die Elementdaten aus einer Datenquelle vorhanden, zu präsentieren zusätzliche Ansichten Abschnitt, z. B. die Kategorien von Buch im Bücherregal oder das Genre Musik in einer Bibliothek "Musik".

Beispielsweise könnte eine zusätzliche Ansicht dienen stellen einen Header für einen bestimmten Abschnitt, wie in der folgenden Abbildung gezeigt:

 [![](uicollectionview-images/02a-supplementary-view.png "Eine zusätzliche Ansicht verwendet, um einen Header für einen bestimmten Abschnitt zu präsentieren, wie hier gezeigt")](uicollectionview-images/02a-supplementary-view.png#lightbox)

Um eine zusätzliche Ansicht verwenden zu können, muss zuerst in registriert werden die `ViewDidLoad` Methode:

```csharp
CollectionView.RegisterClassForSupplementaryView (typeof(Header), UICollectionElementKindSection.Header, headerId);
```

Klicken Sie dann die Ansicht mit zurückgegeben werden muss, `GetViewForSupplementaryElement`, der mithilfe von `DequeueReusableSupplementaryView`, und erbt von `UICollectionReusableView`. Der folgende Codeausschnitt erzeugt die SupplementaryView im Screenshot oben gezeigt:

```csharp
public override UICollectionReusableView GetViewForSupplementaryElement (UICollectionView collectionView, NSString elementKind, NSIndexPath indexPath)
        {
            var headerView = (Header)collectionView.DequeueReusableSupplementaryView (elementKind, headerId, indexPath);
            headerView.Text = "Supplementary View";
            return headerView;
        }

```

Zusätzliche Ansichten sind allgemeiner als nur die Kopf- und Fußzeilen.
Sie können eine beliebige Stelle in der Auflistung positioniert werden und können alle Ansichten, sodass ihre Darstellung zu vollständig anpassbaren umfassen.

 <a name="Decoration_Views" />


## <a name="decoration-views"></a>Decoration-Ansichten

Decoration-Ansichten sind rein visuelle Ansichten, die in angezeigt werden, können ein `UICollectionView`. Im Gegensatz zu den Zellen und zusätzliche Ansichten sind sie nicht datengesteuert. Sie werden immer innerhalb des Layouts-Unterklasse erstellt und später als des Inhalts Layout ändern können. Beispielsweise kann eine Decoration-Ansicht verwendet werden, präsentieren Sie eine Hintergrundansicht, die einen Bildlauf durchführt, mit dem Inhalt der `UICollectionView`, wie unten dargestellt:

 [![](uicollectionview-images/02c-decoration-view.png "Decoration-Ansicht mit einem roten Hintergrund")](uicollectionview-images/02c-decoration-view.png#lightbox)

 Der folgende Codeausschnitt wird den Hintergrund Rot dargestellt, in den Beispielen `CircleLayout` Klasse:

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

Wie bei anderen Teilen von iOS-, wie z. B. `UITableView` und `MKMapView`, `UICollectionView` Ruft die Daten aus der ein *Datenquelle*, verfügbar gemacht in Xamarin.iOS über die **`UICollectionViewDataSource`** Klasse. Diese Klasse ist zuständig für die Bereitstellung von Inhalt an die `UICollectionView` wie z.B.:

-  **Zellen** – Merry `GetCell` Methode.
-  **Zusätzliche Ansichten** – Merry `GetViewForSupplementaryElement` Methode.
-  **Anzahl von Abschnitten** – Merry `NumberOfSections` Methode. Der Standardwert ist 1, wenn nicht implementiert.
-  **Anzahl der Elemente im Abschnitt** – Merry `GetItemsCount` Methode.

### <a name="uicollectionviewcontroller"></a>UICollectionViewController
Der Einfachheit halber die `UICollectionViewController` -Klasse ist verfügbar. Dies wird automatisch konfiguriert, um sowohl der Delegat, der im nächsten Abschnitt erläutert wird, und die Datenquelle für die `UICollectionView` anzeigen.

Wie bei `UITableView`, `UICollectionView` Klasse ruft nur die Datenquelle aus, um die Zellen für Elemente abzurufen, die auf dem Bildschirm sind.
Zellen, die außerhalb des Bildschirms einen Bildlauf in an eine Warteschlange für die Wiederverwendung befinden wie die folgende Abbildung veranschaulicht:

 [![](uicollectionview-images/03-cell-reuse.png "Zellen, die außerhalb des Bildschirms einen Bildlauf an eine Warteschlange für die Wiederverwendung befinden sich wie hier gezeigt.")](uicollectionview-images/03-cell-reuse.png#lightbox)

Wiederverwendung von Zelle wurde mit vereinfacht `UICollectionView` und `UITableView`. Sie müssen nicht mehr zum Erstellen einer Zelle direkt in der Datenquelle, wenn eine nicht verfügbar in der Warteschlange Wiederverwendung ist wie die Zellen mit dem System registriert sind. Wenn eine Zelle beim Aufruf zum Entfernen aus die Zelle aus der Warteschlange für die Wiederverwendung der Warteschlange nicht verfügbar ist, wird iOS erstellt sie automatisch basierend auf den Typ oder die Nib, der registriert wurde.
Das gleiche Verfahren ist auch für zusätzliche Ansichten verfügbar.

Betrachten Sie beispielsweise den folgenden Code, der registriert die `AnimalCell` Klasse:

```csharp
static NSString animalCellId = new NSString ("AnimalCell");
CollectionView.RegisterClassForCell (typeof(AnimalCell), animalCellId);
```

Wenn eine `UICollectionView` eine Zelle benötigt, da dessen Element auf dem Bildschirm, um die `UICollectionView` ruft seine Datenquelle `GetCell` Methode. Ähnlich wie bei der Funktionsweise mit UITableView werden diese Methode ist verantwortlich für die Konfiguration einer Zelle aus den Daten sichern, die eine `AnimalCell` Klasse in diesem Fall.

Der folgende Code zeigt eine Implementierung von `GetCell` zurückgibt, die eine `AnimalCell` Instanz:

```csharp
public override UICollectionViewCell GetCell (UICollectionView collectionView, Foundation.NSIndexPath indexPath)
{
        var animalCell = (AnimalCell)collectionView.DequeueReusableCell (animalCellId, indexPath);

        var animal = animals [indexPath.Row];

        animalCell.Image = animal.Image;

        return animalCell;
}
```

Der Aufruf von `DequeReusableCell` ist, an die Zelle entweder aus der Warteschlange für die Wiederverwendung aufheben in Warteschlangen gestellt werden oder, wenn eine Zelle nicht in der Warteschlange, erstellt auf Grundlage der im Aufruf von registrierten Typ verfügbar ist `CollectionView.RegisterClassForCell`.

In diesem Fall durch die Registrierung der `AnimalCell` Klasse iOS erstellt eine neue `AnimalCell` intern und gibt ihn zurück, wenn eine Zelle entfernen aus der Warteschlange aufgerufen wird, nach dem er mit dem Image in das Animal-Klasse enthalten und für die Anzeige zurückgegeben, um die konfiguriertist`UICollectionView`.

 <a name="Delegate" />


### <a name="delegate"></a>delegate

Die `UICollectionView` Klasse verwendet einen Delegaten vom Typ `UICollectionViewDelegate` zur Unterstützung der Interaktion mit Inhalt in die `UICollectionView`. Dies ermöglicht das Steuern von:

-  **Auswahl der Zelle** – bestimmt, ob eine Zelle ausgewählt wird.
-  **Zellen hervorheben** – bestimmt, ob eine Zelle wird derzeit verwendet wird.
-  **Zelle Menüs** – Menü, das für eine Zelle in der Antwort auf eine Geste für ein langes Drücken angezeigt.


Wie bei der Datenquelle, die `UICollectionViewController` ist standardmäßig so konfiguriert, werden der Delegat für die `UICollectionView`.

 <a name="Cell_HighLighting" />


#### <a name="cell-highlighting"></a>Zelle zu markieren

Wenn eine Zelle, wird die Zelle Übergänge in den markierten Status gedrückt wird, und es nicht, bis aktiviert ist wird der Benutzer den Finger aus der Zelle. Dies ermöglicht einen temporären ändern in die Darstellung der Zelle aus, bevor es tatsächlich ausgewählt wird. Bei Auswahl der Zelle des `SelectedBackgroundView` wird angezeigt. Die folgende Abbildung zeigt den markierten Status auf, kurz bevor die Auswahl vorgenommen wird:

 [![](uicollectionview-images/04-cell-highlight.png "Diese Abbildung zeigt den markierten Status auf, kurz bevor die Auswahl vorgenommen wird")](uicollectionview-images/04-cell-highlight.png#lightbox)

Zum Implementieren von Hervorhebungen, die `ItemHighlighted` und `ItemUnhighlighted` Methoden der `UICollectionViewDelegate` kann verwendet werden. Beispielsweise gilt der folgende Code einen gelben Hintergrund auf den `ContentView` Wenn die Zelle markiert ist, und einen weißen Hintergrund, wenn nicht hervorgehoben, wie in der Abbildung oben gezeigt:

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


#### <a name="disabling-selection"></a>Auswahl deaktivieren

Auswahl ist standardmäßig aktiviert, in `UICollectionView`. Überschreiben, um die Auswahl zu deaktivieren, `ShouldHighlightItem` und Zurückgeben von "false", wie unten dargestellt:

```csharp
public override bool ShouldHighlightItem (UICollectionView collectionView, NSIndexPath indexPath)
{
        return false;
}
```

Wenn Hervorhebung deaktiviert ist, ist der Prozess der Auswahl einer Zelle ebenfalls deaktiviert. Darüber hinaus ist auch eine `ShouldSelectItem` -Methode, die Auswahl direkt, obwohl steuert, wenn `ShouldHighlightItem` implementiert ist und "falsch" und `ShouldSelectItem` wird nicht aufgerufen.

 `ShouldSelectItem` ermöglicht die Auswahl auf aktiviert oder deaktiviert werden, auf Element-von-Element ab, wenn `ShouldHighlightItem` ist nicht implementiert. Darüber hinaus können ohne Auswahl hervorgehoben, wenn `ShouldHighlightItem` wird implementiert, und gibt true zurück, während `ShouldSelectItem` "false" zurückgibt.

 <a name="Cell_Menus" />


#### <a name="cell-menus"></a>Zelle Menüs

Jede Zelle in einer `UICollectionView` kann mit der ein Menü, das Ausschneiden, kopieren und einfügen, um optional unterstützt werden kann. So erstellen ein Menü "Bearbeiten" auf eine Zelle:

1.  Außer Kraft setzen `ShouldShowMenu` und true zurückgeben, wenn das Element ein Menü angezeigt werden sollen.
1.  Außer Kraft setzen `CanPerformAction` und WAHR zurückgeben, für jede Aktion, die das Element ausführen können, die Ausschneiden, kopieren oder einfügen sein.
1.  Außer Kraft setzen `PerformAction` um die Bearbeitung auszuführen, kopieren Sie für den Einfügevorgang.


Der folgende Screenshot Menü anzeigen, wenn eine Zelle lang gedrückt wird:

 [![](uicollectionview-images/04a-menu.png "Dieser Screenshot zeigt das Menü anzeigen, wenn eine Zelle lang gedrückt wird")](uicollectionview-images/04a-menu.png#lightbox)

 <a name="Layout" />


## <a name="layout"></a>Layout

`UICollectionView` unterstützt ein Layout, die ermöglicht, die Positionierung der alle Elemente, Zellen, zusätzliche Ansichten und Decoration-Ansichten, unabhängig von der Verwaltung der `UICollectionView` selbst.
Verwenden das Layoutsystem, kann eine Anwendung Layouts wie Raster-ähnliches unterstützen, wir haben gesehen, in diesem Artikel als auch benutzerdefinierte Layouts zu bereitstellen.

 <a name="Layout_Basics" />


### <a name="layout-basics"></a>Layout-Grundlagen

Layouts in einer `UICollectionView` werden definiert, in einer Klasse, die von erbt `UICollectionViewLayout`. Die Layout-Implementierung ist verantwortlich für das Erstellen der Layoutattribute für jedes Element in der `UICollectionView`. Es gibt zwei Möglichkeiten, ein Layout zu erstellen:

-  Die integrierte `UICollectionViewFlowLayout` .
-  Geben Sie ein benutzerdefiniertes Layout durch Erben von `UICollectionViewLayout` .


 <a name="Flow_Layout" />


### <a name="flow-layout"></a>Flusslayout

Die `UICollectionViewFlowLayout` -Klasse stellt ein linienbasiertes Layout, geeignet für die Anordnung von Inhalt in einem Raster von Zellen, wie wir gesehen haben.

So verwenden Sie eine Flow-layout

-  Erstellen Sie eine Instanz des `UICollectionViewFlowLayout` :


```csharp
var layout = new UICollectionViewFlowLayout ();
```

-  Übergeben Sie die Instanz an den Konstruktor der `UICollectionView` :


```csharp
simpleCollectionViewController = new SimpleCollectionViewController (layout);
```

Dies ist alles, die was Layout Inhalt in einem Raster benötigt wird. Auch wenn die Ausrichtung ändert, ändert der `UICollectionViewFlowLayout` behandelt, wie unten dargestellt, entsprechend neu anordnen des Inhalts:

 [![](uicollectionview-images/05-layout-orientation.png "Beispiel für die Änderungen der bildschirmausrichtung")](uicollectionview-images/05-layout-orientation.png#lightbox)

 <a name="Section_Inset" />


#### <a name="section-inset"></a>Abschnitt Inset

Zu einigen Raum der `UIContentView`, Layouts sind eine `SectionInset` Eigenschaft vom Typ `UIEdgeInsets`. Beispielsweise der folgende Code enthält einen 50 Pixel-Puffer, um den gesamten der `UIContentView` beim Layout durch eine `UICollectionViewFlowLayout`:

```csharp
var layout = new UICollectionViewFlowLayout ();
layout.SectionInset = new UIEdgeInsets (50,50,50,50);
```

Dies führt zu Abstand um den Abschnitt, wie unten dargestellt:

 [![](uicollectionview-images/06-sectioninset.png "Abstand um den Abschnitt, wie hier gezeigt.")](uicollectionview-images/06-sectioninset.png#lightbox)

 <a name="Subclassing_UICollectionViewFlowLayout" />


#### <a name="subclassing-uicollectionviewflowlayout"></a>Erstellen von Unterklassen für UICollectionViewFlowLayout

In der Edition mit `UICollectionViewFlowLayout` direkt, sie können auch als Unterklasse definiert werden um das Layout des Inhalt auf einer Linie weiter anzupassen. Dies kann beispielsweise verwendet werden, erstellen Sie ein Layout, die die Zellen in einem Raster wird nicht umbrochen, sondern erstellt stattdessen eine einzelne Zeile mit einem horizontalen Bildlauf Effekt, wie unten dargestellt:

 [![](uicollectionview-images/07-line-layout.png "Eine einzelne Zeile mit einem horizontalen Bildlauf Effekt")](uicollectionview-images/07-line-layout.png#lightbox)

Dies implementieren, indem Unterklassen `UICollectionViewFlowLayout` erfordert:

-  Initialisieren alle Layouteigenschaften, die auf das Layout selbst anwenden oder alle Elemente im Layout im Konstruktor.
-  Überschreiben `ShouldInvalidateLayoutForBoundsChange` , zurückgeben "true", also bei Grenzen des der `UICollectionView` Änderungen das Layout der Zellen werden neu berechnet. Hiermit wird den Code in diesem Fall sicherstellen, für die während des Bildlaufs auf der Mittelwert Zelle angewendete Transformation angewendet wird.
-  Überschreiben von `TargetContentOffset` den Mittelwert zu Ausrichtung an der Mitte der Zelle die `UICollectionView` als Durchführen eines Bildlaufs beendet wird.
-  Überschreiben von `LayoutAttributesForElementsInRect` zurückzugebenden ein Array von `UICollectionViewLayoutAttributes` . Jede `UICollectionViewLayoutAttribute` enthält Informationen zum Layout, das bestimmte Element, einschließlich Eigenschaften, wie z. B. die `Center` , `Size` , `ZIndex` und `Transform3D` .


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

Zusätzlich zur Verwendung von `UICollectionViewFlowLayout`, Layouts können auch vollständig angepasst werden durch erben direkt von `UICollectionViewLayout`.

Die wichtigsten Methoden überschrieben werden:

-   `PrepareLayout` : Wird zum Ausführen der anfänglichen geometrischer Berechnungen, die während des Prozesses Layout verwendet werden.
-   `CollectionViewContentSize` – Gibt die Größe des Bereichs verwendet, um Inhalt anzuzeigen.
-   `LayoutAttributesForElementsInRect` – Wie im zuvor gezeigten Beispiel UICollectionViewFlowLayout diese Methode verwendet wird, um Informationen zum Bereitstellen der `UICollectionView` zum Layout jedes Element. Anders als bei der `UICollectionViewFlowLayout` , wenn Sie ein benutzerdefiniertes Layout erstellen, können Sie Elemente positionieren, aber Sie auswählen.


Beispielsweise kann derselbe Inhalt in einem kreisförmigen Layout angezeigt wie unten dargestellt:

 [![](uicollectionview-images/08-circle-layout.png "Eine benutzerdefinierte kreisförmigen Layout wie hier gezeigt.")](uicollectionview-images/08-circle-layout.png#lightbox)

Das Layouts leistungsfähigere an, die aus dem Raster-ähnliches-Layout, in einem horizontalen Bildlauf Layout zu ändern und anschließend dieses zirkuläre Layout erfordert nur die Layoutklasse, die bereitgestellt, um die `UICollectionView` geändert werden. "Nothing" in der `UICollectionView`, Delegaten oder seine Datenquelle Änderungen am Code überhaupt.


## <a name="changes-in-ios-9"></a>Änderungen in iOS 9


In iOS 9, die Auflistungsansicht (`UICollectionView`) unterstützt das Ziehen Sie jetzt neuanordnung von Elementen, die standardmäßig durch Hinzufügen einer neuen Standard stiftbewegungs-Erkennung und mehrere neue unterstützender Methoden.

Verwenden diese neuen Methoden, können Sie ganz einfach implementieren ziehen Sie in der Auflistungsansicht Neuanordnen und haben die Möglichkeit zum Anpassen der Darstellung der Elemente jeder Phase des Prozesses Neuordnung von Elementen.

[![](uicollectionview-images/intro01.png "Ein Beispiel für die Neuordnung")](uicollectionview-images/intro01.png#lightbox)

In diesem Artikel werden wir sehen uns die Implementierung von Drag-zu-neuanordnung in einer Xamarin.iOS-Anwendung als auch einige der anderen vorgenommenen Änderungen, die dem Steuerelement Auflistung iOS 9 vorgenommen hat:

- [Einfache Neuanordnung von Elementen](#Easy-Reordering-of-Items)
    - [Einfaches Beispiel für die Neuordnung von Elementen](#Simple-Reordering-Example)
    - [Verwenden eine benutzerdefinierte Stiftbewegungs-Erkennung](#Using-a-Custom-Gesture-Recognizer)
    - [Benutzerdefinierte Layouts und neuanordnung von Spalten](#Custom-Layouts-and-Reording)
- [Änderungen der Auflistung anzeigen](#collection-view-changes)

<a name="Easy-Reordering-of-Items" />

## <a name="reordering-of-items"></a>Neuanordnung von Elementen

Wie bereits erwähnt, war einer der wichtigsten Änderungen an der Auflistungsansicht in iOS 9 das Hinzufügen von einfach Drag-neuanordnung-Funktionalität standardmäßig.

In iOS 9 die schnellste Möglichkeit zum Hinzufügen einer Ansicht der Auflistung Neuanordnen von ist die Verwendung einer `UICollectionViewController`.
Der Auflistung-View-Controller verfügt jetzt über eine `InstallsStandardGestureForInteractiveMovement` -Eigenschaft, die einen Standard fügt *Geste Erkennung* , die Sie ziehen, um Elemente in der Auflistung Neuanordnen unterstützt.
Da der Standardwert ist `true`, haben nur implementieren die `MoveItem` -Methode der der `UICollectionViewDataSource` Klasse zur Unterstützung von Drag-zu-neuanordnung. Zum Beispiel:

```csharp
public override void MoveItem (UICollectionView collectionView, NSIndexPath sourceIndexPath, NSIndexPath destinationIndexPath)
{
    // Reorder our list of items
    ...
}
```
<a name="Simple-Reordering-Example" />

### <a name="simple-reordering-example"></a>Einfaches Beispiel für die Neuordnung von Elementen

Als kurzes Beispiel, starten Sie ein neues Xamarin.iOS-Projekt, und Bearbeiten der **"Main.Storyboard"** Datei. Ziehen Sie eine `UICollectionViewController` auf die Entwurfsoberfläche:

[![](uicollectionview-images/quick01.png "Hinzufügen einer UICollectionViewController")](uicollectionview-images/quick01.png#lightbox)

Wählen Sie die Auflistungsansicht (es kann am einfachsten, diesen Vorgang durchführen, die dokumentgliederung sein). Legen Sie in der Registerkarte "Layout" des Eigenschaftenpads die folgenden Größen, wie im folgenden Screenshot dargestellt:

- **Größe der Zelle**: Width – 60 | Height – 60
- **Größe des Headers**: Width – 0 | Height – 0
- **Fußzeilengröße**: Width – 0 | Height – 0
- **Min-Abstand**: Für Zellen – 8 | Für Zeilen – 8
- **Im Abschnitt Abstände**: Top – 16 | Unten – 16 | Left – 16 | Rechts: 16

[![](uicollectionview-images/quick04.png "Legen Sie die Auflistungsansicht-Größen")](uicollectionview-images/quick04.png#lightbox)

Als Nächstes bearbeiten Sie die standardmäßige Zelle ein:
    - Ändern der Hintergrundfarbe in Blau
    - Fügen Sie eine Bezeichnung, die als Titel für die Zelle fungiert.
    - Legen Sie den Bezeichner für die Wiederverwendung auf **Zelle**

[![](uicollectionview-images/quick02.png "Bearbeiten Sie die standardmäßige Zelle")](uicollectionview-images/quick02.png#lightbox)

Fügen Sie die Einschränkungen, um die Bezeichnung, die in der Zelle zentriert werden, wie sie die Größe ändert beibehalten:

In der **Pad "Eigenschaft"** für die _CollectionViewCell_ und legen Sie die **Klasse** zu `TextCollectionViewCell`:

[![](uicollectionview-images/quick05.png "Legen Sie die Klasse auf TextCollectionViewCell")](uicollectionview-images/quick05.png#lightbox)

Legen Sie die **Wiederverwendbare Auflistungsansicht** zu `Cell`:

[![](uicollectionview-images/quick06.png "Legen Sie die Wiederverwendbare Auflistungsansicht auf Zelle")](uicollectionview-images/quick06.png#lightbox)

Abschließend wählen Sie die Bezeichnung, und nennen Sie sie `TextLabel`:

[![](uicollectionview-images/quick07.png "Bezeichnung für TextLabel")](uicollectionview-images/quick07.png#lightbox)

Bearbeiten der `TextCollectionViewCell` Klasse und fügen Sie die folgenden Eigenschaften.:

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

Hier die `Text` -Eigenschaft der Bezeichnung wird verfügbar gemacht werden als Titel der Zelle, damit sie vom Code festgelegt werden kann.

Fügen Sie einen neuen C# Klasse dem Projekt, und rufen Sie es `WaterfallCollectionSource`. Bearbeiten Sie die Datei, und stellen sie wie folgt aussehen:

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

Diese Klasse wird die Datenquelle für unsere Auflistungsansicht werden, und geben Sie die Informationen für jede Zelle in der Auflistung.
Beachten Sie, dass die `MoveItem` Methode wird implementiert, um das Ziehen von Elementen in der Auflistung können neu angeordnet.

Fügen Sie ein weiteres neues C# Klasse dem Projekt, und rufen Sie es `WaterfallCollectionDelegate`. Bearbeiten Sie diese Datei, und stellen sie wie folgt aussehen:

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

Dies wird als der Delegat für unsere Auflistungsansicht fungieren. Um eine Zelle zu markieren, während der Benutzer in der Auflistung mit ihm interagiert wurde Methoden überschrieben.

Fügen Sie einen letzten C# Klasse dem Projekt, und rufen Sie es `WaterfallCollectionView`. Bearbeiten Sie diese Datei, und stellen sie wie folgt aussehen:

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

Beachten Sie, dass `DataSource` und `Delegate` , die wir oben erstellt werden festgelegt, wenn die Auflistungsansicht aus das Storyboard erstellt wird (oder **XIB** Datei).

Bearbeiten der **"Main.Storyboard"** -Datei erneut, und wählen Sie die Auflistungsansicht aus, und wechseln Sie zu der **Eigenschaften**. Legen Sie die **Klasse** mit den benutzerdefinierten `WaterfallCollectionView` -Klasse, die wir oben definiert:

Speichern Sie die Änderungen, die Sie an der Benutzeroberfläche vorgenommen, und führen Sie die app.
Wenn der Benutzer ein Element aus der Liste wählt und es in einem neuen Speicherort zieht, werden automatisch die anderen Elemente animieren, wie sie aus dem Weg des Elements verschoben.
Wenn der Benutzer das Element an einer neuen Position ablegt, wird es zu diesem Speicherort vorhanden sein. Zum Beispiel:

[![](uicollectionview-images/intro01.png "Ein Beispiel für ein Element an eine neue Position ziehen")](uicollectionview-images/intro01.png#lightbox)

<a name="Using-a-Custom-Gesture-Recognizer" />

### <a name="using-a-custom-gesture-recognizer"></a>Verwenden eine benutzerdefinierte Stiftbewegungs-Erkennung

In Fällen, in dem können keine `UICollectionViewController` und müssen eine regulären `UIViewController`, oder wenn Sie mehr Kontrolle über die Drag & Drop-Geste nutzen möchten, erstellen Sie eigene benutzerdefinierte Stiftbewegungs-Erkennung und die Auflistungsansicht hinzufügen, wenn das Laden der Ansicht. Zum Beispiel:

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

Hier verwenden wir mehrere neue Methoden hinzugefügt, um die Auflistungsansicht zu implementieren und steuern den Ziehvorgang entsteht:

 - `BeginInteractiveMovementForItem` – Kennzeichnet den Anfang eines Verschiebevorgangs.
 - `UpdateInteractiveMovementTargetPosition` -Wird gesendet, wie die Position des Elements aktualisiert wird.
 - `EndInteractiveMovement` : Markiert das Ende der Verschiebung eines Elements.
 - `CancelInteractiveMovement` : Den Benutzer den Vorgang abgebrochen markiert.

Wenn die Anwendung ausgeführt wird, funktioniert der Ziehvorgang genau so, wie der Standardwert ziehen stiftbewegungs-Erkennung, die die Auflistungsansicht gehört.

<a name="Custom-Layouts-and-Reording" />

### <a name="custom-layouts-and-reordering"></a>Benutzerdefinierte Layouts und neuanordnung von Spalten

In iOS 9 wurden mehrere neue Methoden zum Arbeiten mit Drag-zu-neu anordnen und benutzerdefinierte Layouts in einer Auflistung hinzugefügt. Um dieses Feature zu untersuchen, fügen Sie ein benutzerdefiniertes Layout der Auflistung.

Fügen Sie zunächst ein neues C# Klasse mit dem Namen `WaterfallCollectionLayout` zum Projekt. Bearbeiten Sie sie aus, und stellen Sie sie wie folgt aussehen:

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

Dies liegt möglicherweise Klasse verwendet, um ein benutzerdefiniertes zwei Spalten, die Wasserfall-Typlayout zur Auflistungsansicht bereitzustellen.
Der Code verwendet die Schlüssel / Wert-Codierung (über die `WillChangeValue` und `DidChangeValue` Methoden), die Datenbindung für unsere berechnete Eigenschaften in dieser Klasse zu ermöglichen.

Als Nächstes bearbeiten Sie die `WaterfallCollectionSource` , und stellen Sie die folgenden Änderungen und Ergänzungen:

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

Dadurch wird eine zufällige Höhe für jedes der Elemente erstellt, die in der Liste angezeigt werden.

Als Nächstes bearbeiten Sie die `WaterfallCollectionView` Klasse und fügen Sie die folgende Hilfsprogramm-Eigenschaft hinzu:

```csharp
public WaterfallCollectionSource Source {
    get { return (WaterfallCollectionSource)DataSource; }
}
```

Dies wird es auf Ihre Datenquelle (und die Höhe des Elements) aus dem benutzerdefinierten Layout zu vereinfachen.

Schließlich bearbeiten Sie den ansichtscontroller ein, und fügen Sie den folgenden Code hinzu:

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

Dies erstellt eine Instanz des unserer benutzerdefinierten Layouts das Ereignis, das die Größe jedes Elements festlegt und fügt das neue Layout zu unserer Auflistungsansicht.

Wenn wir die Xamarin.iOS-app erneut ausführen, wird die Auflistungsansicht jetzt wie folgt aussehen:

[![](uicollectionview-images/custom01.png "Die Auflistungsansicht sieht nun wie folgt aus.")](uicollectionview-images/custom01.png#lightbox)

Wir können noch Drag-zu-Elemente neu anordnen als vor, aber Elemente ändert sich jetzt an ihrem neuen Speicherort anpassen, wenn sie gelöscht werden.

## <a name="collection-view-changes"></a>Änderungen der Auflistung anzeigen

In den folgenden Abschnitten werden wir einen detaillierten Einblick in die Änderungen, die an jede Klasse in der Auflistung von iOS 9 dauern.

### <a name="uicollectionview"></a>UICollectionView

Die folgenden Änderungen oder Erweiterungen wurden vorgenommen, um die `UICollectionView` -Klasse für iOS 9:

 - `BeginInteractiveMovementForItem` – Kennzeichnet den Anfang eines Ziehvorgangs.
 - `CancelInteractiveMovement` – Informiert Sie die Auflistung anzeigen, dass der Benutzer einen Ziehvorgang abgebrochen wurde.
 - `EndInteractiveMovement` – Informiert Sie die Auflistung anzeigen, dass der Benutzer einen Ziehvorgang beendet wurde.
 - `GetIndexPathsForVisibleSupplementaryElements` – Gibt die `indexPath` einer Kopf-oder Fußzeile in einem Abschnitt der Auflistung anzeigen.
 - `GetSupplementaryView` – Gibt die angegebene Kopf- oder Fußzeile.
 - `GetVisibleSupplementaryViews` – Gibt eine Liste aller sichtbaren Kopf- und Fußzeilen zurück.
 - `UpdateInteractiveMovementTargetPosition` – Informiert Sie die Auflistung anzeigen, dass der Benutzer verschoben wurde oder verschoben wurde, ein Element bei einem Ziehvorgang.

### <a name="uicollectionviewcontroller"></a>UICollectionViewController

Die folgenden Änderungen oder Erweiterungen wurden vorgenommen, um die `UICollectionViewController` Klasse in iOS 9:

 - `InstallsStandardGestureForInteractiveMovement` – If `true` der neuen Stiftbewegungs-Erkennung, die Drag-zu-neuanordnung automatisch unterstützt verwendet werden.
 - `CanMoveItem` – Die Auflistungsansicht informiert, wenn ein bestimmtes Element ziehen neu geordnet werden kann.
 - `GetTargetContentOffset` – Wird verwendet, um den Offset eines Elements der angegebenen Auflistung Ansicht zu erhalten.
 - `GetTargetIndexPathForMove` – Ruft die `indexPath` eines angegebenen Elements für einen Ziehvorgang.
 - `MoveItem` : Verschiebt die Reihenfolge der ein bestimmtes Element in der Liste.


### <a name="uicollectionviewdatasource"></a>UICollectionViewDataSource

Die folgenden Änderungen oder Erweiterungen wurden vorgenommen, um die `UICollectionViewDataSource` Klasse in iOS 9:

 - `CanMoveItem` – Die Auflistungsansicht informiert, wenn ein bestimmtes Element ziehen neu geordnet werden kann.
 - `MoveItem` : Verschiebt die Reihenfolge der ein bestimmtes Element in der Liste.

### <a name="uicollectionviewdelegate"></a>UICollectionViewDelegate

Die folgenden Änderungen oder Erweiterungen wurden vorgenommen, um die `UICollectionViewDelegate` Klasse in iOS 9:

 - `GetTargetContentOffset` – Wird verwendet, um den Offset eines Elements der angegebenen Auflistung Ansicht zu erhalten.
 - `GetTargetIndexPathForMove` – Ruft die `indexPath` eines angegebenen Elements für einen Ziehvorgang.

### <a name="uicollectionviewflowlayout"></a>UICollectionViewFlowLayout

Die folgenden Änderungen oder Erweiterungen wurden vorgenommen, um die `UICollectionViewFlowLayout` Klasse in iOS 9:

 - `SectionFootersPinToVisibleBounds` : Der angemeldete der Fußzeilen im Abschnitt zu den Grenzen des sichtbaren Auflistung anzeigen.
 - `SectionHeadersPinToVisibleBounds` : Der angemeldete die Abschnittsheader zu den Grenzen des sichtbaren Auflistung anzeigen.

### <a name="uicollectionviewlayout"></a>UICollectionViewLayout

Die folgenden Änderungen oder Erweiterungen wurden vorgenommen, um die `UICollectionViewLayout` Klasse in iOS 9:

 - `GetInvalidationContextForEndingInteractiveMovementOfItems` – Gibt die invalidierung Kontext am Ende des Ziehvorgangs zurück, wenn der Benutzer das Ziehen abgeschlossen oder wird abgebrochen.
 - `GetInvalidationContextForInteractivelyMovingItems` – Gibt den Kontext invalidierung am Anfang eines Ziehvorgangs.
 - `GetLayoutAttributesForInteractivelyMovingItem` – Ruft die Layout-Attribute für ein bestimmtes Element beim Ziehen eines Elements.
 - `GetTargetIndexPathForInteractivelyMovingItem` – Gibt die `indexPath` des Elements, das am angegebenen Punkt befindet, beim Ziehen eines Elements.

### <a name="uicollectionviewlayoutattributes"></a>UICollectionViewLayoutAttributes

Die folgenden Änderungen oder Erweiterungen wurden vorgenommen, um die `UICollectionViewLayoutAttributes` Klasse in iOS 9:

 - `CollisionBoundingPath` – Gibt den Pfad der Kollision von zwei Elementen während eines Ziehvorgangs.
 - `CollisionBoundsType` – Gibt den Typ eines Konflikts zurück (wie eine `UIDynamicItemCollisionBoundsType`), die während eines Ziehvorgangs aufgetreten.

### <a name="uicollectionviewlayoutinvalidationcontext"></a>UICollectionViewLayoutInvalidationContext

Die folgenden Änderungen oder Erweiterungen wurden vorgenommen, um die `UICollectionViewLayoutInvalidationContext` Klasse in iOS 9:

 - `InteractiveMovementTarget` – Gibt das Zielelement eines Ziehvorgangs.
 - `PreviousIndexPathsForInteractivelyMovingItems` – Gibt die `indexPaths` anderer Elemente beteiligt ein Ziehvorgang, Vorgang neu anzuordnen.
 - `TargetIndexPathsForInteractivelyMovingItems` – Gibt die `indexPaths` von Elementen, die als Ergebnis einer Operation Drag-zu-neuanordnung neu angeordnet werden werden.

### <a name="uicollectionviewsource"></a>UICollectionViewSource

Die folgenden Änderungen oder Erweiterungen wurden vorgenommen, um die `UICollectionViewSource` Klasse in iOS 9:

 - `CanMoveItem` – Die Auflistungsansicht informiert, wenn ein bestimmtes Element ziehen neu geordnet werden kann.
 - `GetTargetContentOffset` – Gibt die Offsets der Elemente, die über eine Drag-zu-neuordnungsvorgang verschoben werden.
 - `GetTargetIndexPathForMove` – Gibt die `indexPath` eines Elements, die während eines Drag-zu-neuanordnung verschoben werden.
 - `MoveItem` : Verschiebt die Reihenfolge der ein bestimmtes Element in der Liste.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde erläutert die Änderungen zu Auflistungsansichten in iOS 9 und beschrieben, wie sie in Xamarin.iOS zu implementieren.
Es wird beschrieben, implementieren eine einfache Drag-neuanordnung-Aktion in einer Auflistungsansicht; verwenden eine benutzerdefinierte Stiftbewegungs-Erkennung mit Drag-zu-anordnen; und wie Drag-zu-neuanordnung ein Ansichtslayouts für benutzerdefinierte Sammlung auswirkt.

## <a name="related-links"></a>Verwandte Links

- [iOS 9-Beispiele](https://developer.xamarin.com/samples/ios/iOS9/)
- [Collection-Viewer-Beispiel](https://developer.xamarin.com/samples/monotouch/ios9/CollectionView/)
- [SimpleCollectionView (Beispiel)](https://developer.xamarin.com/samples/SimpleCollectionView/)
- [Ereignisse, Protokolle und Delegaten](~/ios/app-fundamentals/delegates-protocols-and-events.md)
- [Arbeiten mit Tabellen und Zellen](~/ios/user-interface/controls/tables/index.md)
