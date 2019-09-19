---
title: Auflistungs Ansichten in xamarin. IOS
description: Mithilfe von Auflistungs Ansichten können Inhalte mit beliebigen Layouts angezeigt werden. Sie ermöglichen die einfache Erstellung von Raster ähnlichen Layouts, während gleichzeitig auch benutzerdefinierte Layouts unterstützt werden.
ms.prod: xamarin
ms.assetid: F4B85F25-0CB5-4FEA-A3B5-D22FCDC81AE4
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/20/2017
ms.openlocfilehash: 078d5a2d5c05f39a9c6d8d081b08faa7b4b8ec67
ms.sourcegitcommit: 6b833f44d5fd8dc7ab7f8546e8b7d383e5a989db
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/18/2019
ms.locfileid: "71106108"
---
# <a name="collection-views-in-xamarinios"></a>Auflistungs Ansichten in xamarin. IOS

_Mithilfe von Auflistungs Ansichten können Inhalte mit beliebigen Layouts angezeigt werden. Sie ermöglichen die einfache Erstellung von Raster ähnlichen Layouts, während gleichzeitig auch benutzerdefinierte Layouts unterstützt werden._

Sammlungs Sichten, die in der `UICollectionView` -Klasse verfügbar sind, sind ein neues Konzept in ios 6, das die Darstellung mehrerer Elemente auf dem Bildschirm mithilfe von Layouts einführt. Die Muster für die Bereitstellung von `UICollectionView` Daten für einen, um Elemente zu erstellen und mit diesen Elementen zu interagieren, folgen derselben Delegierung und denselben Datenquellen Mustern, die häufig bei der IOS-Entwicklung

Auflistungs Ansichten funktionieren jedoch mit einem layoutsubsystem, das unabhängig `UICollectionView` von der selbst ist. Daher kann die Darstellung einer Sammlungsansicht durch einfaches Bereitstellen eines anderen Layouts problemlos geändert werden.

IOS bietet eine Layoutklasse `UICollectionViewFlowLayout` mit dem Namen, die das Erstellen von Zeilen basierten Layouts, wie z. b. einem Raster, ohne zusätzliche Arbeit ermöglicht. Außerdem können benutzerdefinierte Layouts erstellt werden, die jede Präsentation ermöglichen, die Sie sich vorstellen können.

## <a name="uicollectionview-basics"></a>Uicollectionview-Grundlagen

Die `UICollectionView` Klasse besteht aus drei unterschiedlichen Elementen:

- **Zellen** – datengesteuerte Sichten für jedes Element
- **Ergänzende Sichten** – datengesteuerte Sichten, die einem Abschnitt zugeordnet sind.
- **Dekorations Ansichten** – nicht datengesteuerte Sichten, die von einem Layout erstellt werden

## <a name="cells"></a>Zellen

Zellen sind Objekte, die ein einzelnes Element in dem DataSet darstellen, das von der Auflistungs Ansicht angezeigt wird. Jede Zelle ist eine Instanz der `UICollectionViewCell` -Klasse, die aus drei verschiedenen Ansichten besteht, wie in der folgenden Abbildung dargestellt:

 [![](uicollectionview-images/01-uicollectionviewcell.png "Jede Zelle besteht aus drei verschiedenen Ansichten, wie hier gezeigt.")](uicollectionview-images/01-uicollectionviewcell.png#lightbox)

Die `UICollectionViewCell` -Klasse verfügt für jede dieser Sichten über die folgenden Eigenschaften:

- `ContentView`– Diese Ansicht enthält den Inhalt, der in der Zelle dargestellt wird. Sie wird in der obersten z-Reihenfolge auf dem Bildschirm gerendert.
- `SelectedBackgroundView`–-Zellen verfügen über integrierte Unterstützung für die Auswahl. Diese Ansicht wird verwendet, um zu kennzeichnen, dass eine Zelle ausgewählt ist. Sie wird direkt unterhalb `ContentView` von gerendert, wenn eine Zelle ausgewählt wird.
- `BackgroundView`– Zellen können auch einen Hintergrund anzeigen, der von `BackgroundView` dargestellt wird. Diese Ansicht wird unterhalb `SelectedBackgroundView` von gerendert.

Wenn Sie die `ContentView` Einstellung so festlegen, dass Sie kleiner `BackgroundView` als `SelectedBackgroundView`und ist `BackgroundView` , kann verwendet werden, um den Inhalt visuell zu gestalten `SelectedBackgroundView` , während der angezeigt wird, wenn eine Zelle ausgewählt wird, wie unten dargestellt:

 [![](uicollectionview-images/02-cells.png "Die unterschiedlichen Zellen Elemente")](uicollectionview-images/02-cells.png#lightbox)

Die Zellen im obigen Screenshot werden erstellt `UICollectionViewCell` , indem Sie von erben und die `ContentView`-, `SelectedBackgroundView` - `BackgroundView` und-Eigenschaften festlegen, wie im folgenden Code gezeigt:

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

## <a name="supplementary-views"></a>Ergänzende Ansichten

Ergänzende Sichten sind Sichten, die Informationen darstellen, die den einzelnen Abschnitten `UICollectionView`eines zugeordnet sind. Wie Zellen sind ergänzende Sichten Daten gesteuert. Wenn Zellen die Elementdaten aus einer Datenquelle darstellen, stellen ergänzende Sichten die Abschnitts Daten dar, z. b. die Buch Kategorien in einem Buch Regal oder das Genre der Musik in einer Musikbibliothek.

Beispielsweise kann eine ergänzende Ansicht verwendet werden, um einen Header für einen bestimmten Abschnitt darzustellen, wie in der folgenden Abbildung dargestellt:

 [![](uicollectionview-images/02a-supplementary-view.png "Eine ergänzende Ansicht, die verwendet wird, um einen Header für einen bestimmten Abschnitt darzustellen, wie hier gezeigt.")](uicollectionview-images/02a-supplementary-view.png#lightbox)

Um eine ergänzende Ansicht zu verwenden, muss Sie zuerst in der `ViewDidLoad` -Methode registriert werden:

```csharp
CollectionView.RegisterClassForSupplementaryView (typeof(Header), UICollectionElementKindSection.Header, headerId);
```

Anschließend muss die Sicht mithilfe `GetViewForSupplementaryElement`von zurückgegeben werden, erstellt mithilfe `DequeueReusableSupplementaryView`von, und erbt von `UICollectionReusableView`. Der folgende Code Ausschnitt erzeugt die ergänzende Ansicht, die im obigen Screenshot dargestellt wird:

```csharp
public override UICollectionReusableView GetViewForSupplementaryElement (UICollectionView collectionView, NSString elementKind, NSIndexPath indexPath)
        {
            var headerView = (Header)collectionView.DequeueReusableSupplementaryView (elementKind, headerId, indexPath);
            headerView.Text = "Supplementary View";
            return headerView;
        }

```

Ergänzende Sichten sind generischer als nur Kopf-und Fußzeilen.
Sie können an einer beliebigen Stelle in der Sammlungsansicht positioniert werden und können aus beliebigen Ansichten bestehen, sodass ihre Darstellung vollständig anpassbar ist.

 <a name="Decoration_Views" />

## <a name="decoration-views"></a>Dekorations Ansichten

Dekorations Ansichten sind reine visuelle Ansichten, die in einem `UICollectionView`angezeigt werden können. Im Gegensatz zu Zellen und ergänzenden Ansichten sind Sie nicht Daten gesteuert. Sie werden immer in der Unterklasse eines Layouts erstellt und können anschließend als Layout des Inhalts geändert werden. So kann z. b. eine-anfügeansicht verwendet werden, um eine Hintergrund Ansicht anzuzeigen, `UICollectionView`die mit dem Inhalt im scrollt, wie unten dargestellt:

 [![](uicollectionview-images/02c-decoration-view.png "Ansicht \"Dekoration\" mit rotem Hintergrund")](uicollectionview-images/02c-decoration-view.png#lightbox)

 Der folgende Code Ausschnitt ändert in der Samples `CircleLayout` -Klasse den Hintergrund in rot:

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

Wie bei anderen Teilen von IOS, wie z `UITableView` . `MKMapView`b `UICollectionView` . und, ruft seine Daten aus einer *Datenquelle*ab, die über die **`UICollectionViewDataSource`** -Klasse in xamarin. IOS verfügbar gemacht wird. Diese Klasse ist verantwortlich für die `UICollectionView` Bereitstellung von Inhalten für, wie z. b.:

- **Zellen** – von der `GetCell` -Methode zurückgegeben.
- **Ergänzende Sichten** – von der `GetViewForSupplementaryElement` -Methode zurückgegeben.
- **Anzahl von Abschnitten** – wird von `NumberOfSections` der-Methode zurückgegeben. Der Standardwert ist 1, wenn er nicht implementiert ist.
- **Anzahl von Elementen pro Abschnitt** – wird von `GetItemsCount` der-Methode zurückgegeben.

### <a name="uicollectionviewcontroller"></a>UICollectionViewController
Der Zweck der- `UICollectionViewController` Klasse ist die-Klasse verfügbar. Diese wird automatisch so konfiguriert, dass Sie sowohl der Delegat ist, der im nächsten Abschnitt erläutert wird, als auch `UICollectionView` die Datenquelle für die Ansicht.

Wie bei `UITableView`Ruft die `UICollectionView` -Klasse nur Ihre Datenquelle auf, um Zellen für Elemente zu erhalten, die sich auf dem Bildschirm befinden.
Zellen, die auf dem Bildschirm scrollen, werden zur Wiederverwendung in eine Warteschlange eingefügt, wie in der folgenden Abbildung veranschaulicht:

 [![](uicollectionview-images/03-cell-reuse.png "Zellen, die auf dem Bildschirm scrollen, werden zur Wiederverwendung in eine Warteschlange eingefügt, wie hier gezeigt.")](uicollectionview-images/03-cell-reuse.png#lightbox)

Die Wiederverwendung von Zellen wurde `UICollectionView` mit `UITableView`und vereinfacht. Es ist nicht mehr erforderlich, eine Zelle direkt in der Datenquelle zu erstellen, wenn Sie in der Warteschlange für die Wiederverwendung nicht verfügbar ist, da Zellen beim System registriert sind. Wenn eine Zelle nicht verfügbar ist, wenn der Befehl zum Aufheben der Warteschlange der Zelle aus der Warteschlange für die Wiederverwendung verfügbar ist, erstellt IOS diese automatisch basierend auf dem registrierten Typ oder NIB.
Die gleiche Technik ist auch für ergänzende Ansichten verfügbar.

Sehen Sie sich beispielsweise den folgenden Code an, `AnimalCell` der die-Klasse registriert:

```csharp
static NSString animalCellId = new NSString ("AnimalCell");
CollectionView.RegisterClassForCell (typeof(AnimalCell), animalCellId);
```

Wenn ein `UICollectionView` eine Zelle benötigt, da sich das Element auf dem Bildschirm `UICollectionView` befindet, ruft die die `GetCell` -Methode der zugehörigen Datenquelle auf. Ähnlich wie dies mit uitableview funktioniert, ist diese Methode für das Konfigurieren einer Zelle aus den Unterstützungs Daten zuständig, die in diesem Fall `AnimalCell` eine Klasse wäre.

Der folgende Code zeigt eine Implementierung von `GetCell` , die eine `AnimalCell` -Instanz zurückgibt:

```csharp
public override UICollectionViewCell GetCell (UICollectionView collectionView, Foundation.NSIndexPath indexPath)
{
        var animalCell = (AnimalCell)collectionView.DequeueReusableCell (animalCellId, indexPath);

        var animal = animals [indexPath.Row];

        animalCell.Image = animal.Image;

        return animalCell;
}
```

Der- `CollectionView.RegisterClassForCell`Vorgang istderOrt,andemdieZelleentwederausderWarteschlangefürdieWiederverwendungausderWarteschlangeentferntwirdoderwenneineZellenichtinderWarteschlangeverfügbarist,basierendaufdemTyp,derim-Aufrufe`DequeReusableCell` von registriert ist.

In diesem Fall erstellt IOS durch das `AnimalCell` Registrieren der-Klasse intern eine neue `AnimalCell` und gibt diese zurück, wenn ein aufzurufende Befehl zum Aufheben der Warteschlange für eine Zelle erstellt wird. Anschließend wird Sie mit dem in der Tier-Klasse enthaltenen Bild konfiguriert und für die Anzeige an die `UICollectionView`.

 <a name="Delegate" />

### <a name="delegate"></a>delegate

Die `UICollectionView` -Klasse verwendet einen Delegaten `UICollectionViewDelegate` vom Typ, um die `UICollectionView`Interaktion mit Inhalt in zu unterstützen. Dies ermöglicht Folgendes:

- **Zellen Auswahl** – bestimmt, ob eine Zelle ausgewählt ist.
- **Zellen Hervorhebung** – bestimmt, ob eine Zelle gerade berührt wird.
- **Zellen Menüs** – Menü, das als Reaktion auf eine lange Press Bewegung für eine Zelle angezeigt wird.

Wie bei der Datenquelle ist die `UICollectionViewController` standardmäßig so konfiguriert, `UICollectionView`dass Sie der-Delegat ist.

 <a name="Cell_HighLighting" />

#### <a name="cell-highlighting"></a>Zellen Hervorhebung

Wenn eine Zelle gedrückt wird, wechselt die Zelle in einen markierten Zustand und wird erst ausgewählt, wenn der Benutzer den Finger von der Zelle anhebt. Dies ermöglicht eine temporäre Änderung der Darstellung der Zelle, bevor Sie tatsächlich ausgewählt wird. Bei Auswahl werden die Zellen `SelectedBackgroundView` angezeigt. Die folgende Abbildung zeigt den hervorgehobenen Zustand, kurz bevor die Auswahl erfolgt:

 [![](uicollectionview-images/04-cell-highlight.png "Diese Abbildung zeigt den hervorgehobenen Zustand, kurz bevor die Auswahl erfolgt.")](uicollectionview-images/04-cell-highlight.png#lightbox)

Zum Implementieren der Hervorhebung `ItemHighlighted` `UICollectionViewDelegate` können `ItemUnhighlighted` die-Methode und die-Methode von verwendet werden. Der folgende Code wendet z. b. einen gelben Hintergrund des an `ContentView` , wenn die Zelle hervorgehoben ist, und einen weißen Hintergrund, wenn er nicht hervorgehoben ist, wie in der Abbildung oben dargestellt:

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

Die Auswahl ist in `UICollectionView`standardmäßig aktiviert. Um die Auswahl zu deaktivieren `ShouldHighlightItem` , überschreiben Sie, und geben Sie false zurück

```csharp
public override bool ShouldHighlightItem (UICollectionView collectionView, NSIndexPath indexPath)
{
        return false;
}
```

Wenn die Hervorhebung deaktiviert ist, wird der Vorgang der Auswahl einer Zelle ebenfalls deaktiviert. Außerdem gibt es auch eine `ShouldSelectItem` Methode, die die Auswahl direkt steuert. Wenn `ShouldHighlightItem` jedoch implementiert ist und false zurückgibt, `ShouldSelectItem` wird nicht aufgerufen.

 `ShouldSelectItem`ermöglicht das Aktivieren oder Deaktivieren der Auswahl auf Element Basis, wenn `ShouldHighlightItem` nicht implementiert ist. Außerdem ermöglicht Sie die Hervorhebung ohne Auswahl `ShouldHighlightItem` , wenn implementiert ist und true zurück `ShouldSelectItem` gibt, während false zurückgibt.

 <a name="Cell_Menus" />

#### <a name="cell-menus"></a>Zellen Menüs

Jede Zelle in einem `UICollectionView` kann ein Menü zeigen, das Ausschneiden, kopieren und einfügen ermöglicht, optional unterstützt zu werden. So erstellen Sie ein Bearbeitungs Menü in einer Zelle:

1. Über `ShouldShowMenu` schreiben Sie, und geben Sie true zurück, wenn das Element ein Menü anzeigen soll.
1. Über `CanPerformAction` schreiben Sie, und geben Sie für jede Aktion, die vom Element ausgeführt werden kann, true zurück, wobei es sich um Ausschneiden, kopieren oder Einfügen handelt.
1. Über `PerformAction` schreiben, um den Vorgang zum Bearbeiten, Kopieren von Einfügevorgang auszuführen

Der folgende Screenshot zeigt das Menü, wenn eine Zelle lange gedrückt ist:

 [![](uicollectionview-images/04a-menu.png "Dieser Screenshot zeigt das Menü an, wenn eine Zelle lang gedrückt ist.")](uicollectionview-images/04a-menu.png#lightbox)

 <a name="Layout" />

## <a name="layout"></a>Layout

`UICollectionView`unterstützt ein Layoutsystem, das es ermöglicht, alle zugehörigen Elemente, Zellen, ergänzenden Sichten und Dekorations Ansichten unabhängig von der `UICollectionView` selbst zu verwalten.
Mithilfe des Layoutsystems kann eine Anwendung Layouts unterstützen, wie z. b. das Raster, das wir in diesem Artikel gesehen haben, sowie benutzerdefinierte Layouts bereitstellen.

 <a name="Layout_Basics" />

### <a name="layout-basics"></a>Grundlagen des Layouts

Layouts in einem `UICollectionView` werden in einer Klasse definiert, die von `UICollectionViewLayout`erbt. Die layoutimplementierung ist für das Erstellen der Layoutattribute für jedes Element `UICollectionView`in der verantwortlich. Es gibt zwei Möglichkeiten, ein Layout zu erstellen:

- Verwenden Sie die integrierte `UICollectionViewFlowLayout` .
- Stellen Sie ein benutzerdefiniertes Layout bereit `UICollectionViewLayout` , indem Sie von erben.

 <a name="Flow_Layout" />

### <a name="flow-layout"></a>Flusslayout

Die `UICollectionViewFlowLayout` -Klasse bietet ein Linien basiertes Layout, das zum Anordnen von Inhalt in einem Raster von Zellen geeignet ist, wie wir gesehen haben.

So verwenden Sie ein Fluss Layout:

- Erstellen Sie eine Instanz `UICollectionViewFlowLayout` von:

```csharp
var layout = new UICollectionViewFlowLayout ();
```

- Übergeben Sie die-Instanz an den Konstruktor `UICollectionView` von:

```csharp
simpleCollectionViewController = new SimpleCollectionViewController (layout);
```

Das ist alles, was zum Layoutinhalt in einem Raster benötigt wird. Wenn sich die Ausrichtung ändert, `UICollectionViewFlowLayout` ordnet der den Inhalt ebenfalls entsprechend an, wie unten dargestellt:

 [![](uicollectionview-images/05-layout-orientation.png "Beispiel für die Ausrichtung von Änderungen")](uicollectionview-images/05-layout-orientation.png#lightbox)

 <a name="Section_Inset" />

#### <a name="section-inset"></a>Abschnitts INSET

Zum Bereitstellen von Speicherplatz `UIContentView`um den verfügen Layouts `SectionInset` über eine Eigenschaft `UIEdgeInsets`vom Typ. Der folgende Code stellt z. b. einen 50-Pixel-Puffer um jeden Abschnitt `UIContentView` von bereit, wenn er `UICollectionViewFlowLayout`von einem angelegt wird:

```csharp
var layout = new UICollectionViewFlowLayout ();
layout.SectionInset = new UIEdgeInsets (50,50,50,50);
```

Dies führt zu einem Abstand um den Abschnitt, wie unten dargestellt:

 [![](uicollectionview-images/06-sectioninset.png "Abstand um den Abschnitt, wie hier gezeigt")](uicollectionview-images/06-sectioninset.png#lightbox)

 <a name="Subclassing_UICollectionViewFlowLayout" />

#### <a name="subclassing-uicollectionviewflowlayout"></a>Subclassinguicollectionviewflowlayout

In der Edition zur `UICollectionViewFlowLayout` direkten Verwendung kann Sie auch untergeordnet werden, um das Layout von Inhalten an einer Zeile weiter anzupassen. Dies kann z. b. verwendet werden, um ein Layout zu erstellen, das die Zellen nicht in ein Raster packt, sondern stattdessen eine einzelne Zeile mit einem horizontalen scrolleffekt erstellt, wie unten dargestellt:

 [![](uicollectionview-images/07-line-layout.png "Eine einzelne Zeile mit einem horizontalen scrolleffekt")](uicollectionview-images/07-line-layout.png#lightbox)

Um dies durch Unterklassen `UICollectionViewFlowLayout` zu implementieren, ist Folgendes erforderlich:

- Initialisieren von Layouteigenschaften, die für das Layout selbst oder für alle Elemente im Layout im Konstruktor gelten.
- Überschreiben von true `UICollectionView` ,sodassdasLayoutderZellen,wennsichdieGrenzenderÄnderungenändern,neuberechnetwird.`ShouldInvalidateLayoutForBoundsChange` Dies wird in diesem Fall verwendet, um sicherzustellen, dass der Code für die Transformation, die auf die zentriste Zelle angewendet wird, beim Scrollen angewendet wird.
- Überschreiben `UICollectionView` , um die mittelste Zelle in den Mittelpunkt der zu setzen, während der Bildlauf angehalten wird. `TargetContentOffset`
- Überschreiben, um ein- `UICollectionViewLayoutAttributes` Array zurückzugeben. `LayoutAttributesForElementsInRect` Jede `UICollectionViewLayoutAttribute` enthält Informationen zum Layout des jeweiligen Elements, `Size` einschließlich der `Center` `ZIndex` Eigenschaften, und `Transform3D` .

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

### <a name="custom-layout"></a>Benutzerdefiniertes Layout

Zusätzlich zur Verwendung `UICollectionViewFlowLayout`von können Layouts auch vollständig angepasst werden, indem direkt von `UICollectionViewLayout`geerbt wird.

Folgende Schlüsselmethoden müssen überschrieben werden:

- `PrepareLayout`– Wird zum Durchführen ursprünglicher geometrischer Berechnungen verwendet, die während des gesamten Layoutprozesses verwendet werden.
- `CollectionViewContentSize`– Gibt die Größe des Bereichs zurück, der zum Anzeigen von Inhalten verwendet wird.
- `LayoutAttributesForElementsInRect`– Wie das oben gezeigte uicollectionviewflowlayout-Beispiel, wird diese Methode zum Bereitstellen von Informationen für `UICollectionView` das Layout der einzelnen Elemente verwendet. Anders als bei der `UICollectionViewFlowLayout` Erstellung eines benutzerdefinierten Layouts können Sie jedoch die Elemente positionieren, die Sie auswählen.

Beispielsweise kann derselbe Inhalt in einem zirkulären Layout wie unten gezeigt dargestellt werden:

 [![](uicollectionview-images/08-circle-layout.png "Ein kreisförmiges benutzerdefiniertes Layout, wie hier gezeigt")](uicollectionview-images/08-circle-layout.png#lightbox)

Der leistungsstarke Vorteil von Layouts besteht darin, dass Sie zum Ändern des Raster ähnlichen Layouts in ein horizontales scrolllayout und anschließende anschließende zu diesem zirkulären Layout nur die Layoutklasse ändern müssen, die für die `UICollectionView` geändert wird. Nichts in der `UICollectionView`, der Delegat oder der Datenquellen Code ändert sich überhaupt.

## <a name="changes-in-ios-9"></a>Änderungen in ios 9

In ios 9 unterstützt die Auflistungs Ansicht (`UICollectionView`) jetzt die Neuanordnung von Elementen im Standardfeld, indem eine neue Standard Gestenerkennung und mehrere neue unterstützende Methoden hinzugefügt werden.

Mithilfe dieser neuen Methoden können Sie Drag to Order in der Auflistungs Ansicht problemlos implementieren und die Darstellung der Elemente in jeder Phase des Neuanordnungs Vorgangs anpassen.

[![](uicollectionview-images/intro01.png "Ein Beispiel für den Neuanordnungs Vorgang")](uicollectionview-images/intro01.png#lightbox)

In diesem Artikel wird die Implementierung von Drag-to-Order in einer xamarin. IOS-Anwendung sowie einige der anderen Änderungen erläutert, die IOS 9 an der Sammlungsansicht vorgenommen hat:

- [Einfache Neuanordnung von Elementen](#Easy-Reordering-of-Items)
  - [Beispiel für einfache Neuanordnung](#Simple-Reordering-Example)
  - [Verwenden einer benutzerdefinierten Gestenerkennung](#Using-a-Custom-Gesture-Recognizer)
  - [Benutzerdefinierte Layouts und Neuanordnen](#Custom-Layouts-and-Reording)
- [Änderungen der Sammlungsansicht](#collection-view-changes)

<a name="Easy-Reordering-of-Items" />

## <a name="reordering-of-items"></a>Neuanordnen von Elementen

Wie bereits erwähnt, war eine der signifikantesten Änderungen an der Sammlungsansicht in ios 9 das Hinzufügen von einfachen Drag-to-Order-Funktionen.

In ios 9 ist die schnellste Möglichkeit zum Hinzufügen einer Neuanordnung zu einer Auflistungs Ansicht `UICollectionViewController`die Verwendung eines.
Der Auflistungs Ansichts Controller `InstallsStandardGestureForInteractiveMovement` verfügt nun über eine-Eigenschaft, die eine Standard *Gestenerkennung* hinzufügt, die das ziehen zum Neuanordnen von Elementen in der Auflistung unterstützt.
Da der Standardwert ist `true`, müssen Sie nur die `MoveItem` -Methode der `UICollectionViewDataSource` -Klasse implementieren, um die Drag-to-Order-Unterstützung zu unterstützen. Beispiel:

```csharp
public override void MoveItem (UICollectionView collectionView, NSIndexPath sourceIndexPath, NSIndexPath destinationIndexPath)
{
  // Reorder our list of items
  ...
}
```

<a name="Simple-Reordering-Example" />

### <a name="simple-reordering-example"></a>Beispiel für einfache Neuanordnung

Starten Sie als schnelles Beispiel ein neues xamarin. IOS-Projekt, und bearbeiten Sie die Datei " **Main. Storyboard** ". Ziehen Sie `UICollectionViewController` einen auf die Entwurfs Oberfläche:

[![](uicollectionview-images/quick01.png "Hinzufügen von uicollectionviewcontroller")](uicollectionview-images/quick01.png#lightbox)

Wählen Sie die Sammlungsansicht aus (Dies ist möglicherweise am einfachsten in der Dokument Gliederung). Legen Sie auf der Registerkarte Layout des Eigenschaftenpad die folgenden Größen fest, wie im folgenden Screenshot veranschaulicht:

- **Zellgröße**: Breite – 60 | Höhe – 60
- **Header Größe**: Breite – 0 | Höhe – 0
- **Footergröße**: Breite – 0 | Höhe – 0
- Minimaler **Abstand**: Für Zellen – 8 | Für Zeilen – 8
- **Abschnitts insets**: Top – 16 | Bottom – 16 | Left – 16 | Right – 16

[![](uicollectionview-images/quick04.png "Festlegen der Größen der Sammlungsansicht")](uicollectionview-images/quick04.png#lightbox)

Bearbeiten Sie als nächstes die Standard Zelle:

- Ändern der Hintergrundfarbe in blau
- Fügen Sie eine Bezeichnung hinzu, die als Titel für die Zelle fungiert.
- Festlegen des Verwendungs Bezeichners auf die **Zelle**

[![](uicollectionview-images/quick02.png "Standard Zelle bearbeiten")](uicollectionview-images/quick02.png#lightbox)

Fügen Sie Einschränkungen hinzu, damit die Bezeichnung in der Zelle zentriert bleibt, wenn Sie die Größe ändert:

Legen Sie im **eigenschaftenpad** für _collectionviewcell_ fest, und `TextCollectionViewCell`legen Sie die- **Klasse** auf fest:

[![](uicollectionview-images/quick05.png "Legen Sie die Klasse auf textcollectionviewcell fest.")](uicollectionview-images/quick05.png#lightbox)

Legen Sie die wieder **verwendbare Ansicht** der Auflistung auf `Cell`fest:

[![](uicollectionview-images/quick06.png "Wiederverwendbare Ansicht der Sammlung auf Zelle festlegen")](uicollectionview-images/quick06.png#lightbox)

Wählen Sie abschließend die Bezeichnung aus, und `TextLabel`benennen Sie Sie:

[![](uicollectionview-images/quick07.png "namens Bezeichnung Text Label")](uicollectionview-images/quick07.png#lightbox)

Bearbeiten Sie `TextCollectionViewCell` die-Klasse, und fügen Sie die folgenden Eigenschaften hinzu:

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

Hier wird `Text` die-Eigenschaft der Bezeichnung als Titel der Zelle verfügbar gemacht, sodass Sie aus dem Code festgelegt werden kann.

Fügen Sie dem C# Projekt eine neue Klasse hinzu, und `WaterfallCollectionSource`nennen Sie Sie. Bearbeiten Sie die Datei, und führen Sie Sie wie folgt aus:

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

Diese Klasse ist die Datenquelle für die Sammlungsansicht und stellt die Informationen für jede Zelle in der Auflistung bereit.
Beachten Sie, `MoveItem` dass die-Methode implementiert wird, damit Elemente in der Auflistung neu angeordnet werden können.

Fügen Sie dem C# Projekt eine weitere neue Klasse hinzu, `WaterfallCollectionDelegate`und nennen Sie Sie. Bearbeiten Sie diese Datei, und führen Sie Sie wie folgt aus:

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

Dies fungiert als Delegat für die Auflistungs Ansicht. Methoden wurden überschrieben, um eine Zelle hervorzuheben, während der Benutzer in der Auflistungs Ansicht mit ihr interagiert.

Fügen Sie dem C# Projekt eine letzte Klasse hinzu, und `WaterfallCollectionView`nennen Sie Sie. Bearbeiten Sie diese Datei, und führen Sie Sie wie folgt aus:

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

Beachten Sie `DataSource` , `Delegate` dass und, die wir oben erstellt haben, festgelegt werden, wenn die Auflistungs Ansicht aus dem Storyboard (oder **XIb** -Datei) erstellt wird.

Bearbeiten Sie die Datei " **Main. Storyboard** " erneut, und wählen Sie die Sammlungsansicht aus, und wechseln Sie zu den **Eigenschaften**. Legen Sie die- **Klasse** auf `WaterfallCollectionView` die benutzerdefinierte Klasse fest, die wir oben definiert haben:

Speichern Sie die an der Benutzeroberfläche vorgenommenen Änderungen, und führen Sie die APP aus.
Wenn der Benutzer ein Element aus der Liste auswählt und an einen neuen Speicherort zieht, werden die anderen Elemente automatisch animiert, wenn Sie sich nicht mehr auf das Element bewegen.
Wenn der Benutzer das Element an einem neuen Speicherort löscht, wird er an diesem Speicherort abgelegt. Beispiel:

[![](uicollectionview-images/intro01.png "Ein Beispiel für das Ziehen eines Elements an einen neuen Speicherort")](uicollectionview-images/intro01.png#lightbox)

<a name="Using-a-Custom-Gesture-Recognizer" />

### <a name="using-a-custom-gesture-recognizer"></a>Verwenden einer benutzerdefinierten Gestenerkennung

In Fällen, in denen Sie keine `UICollectionViewController` verwenden können und eine reguläre `UIViewController`verwenden müssen, oder wenn Sie mehr Kontrolle über die Drag & Drop-Bewegung benötigen, können Sie eine eigene benutzerdefinierte Gestenerkennung erstellen und diese der Auflistungs Ansicht hinzufügen, wenn die Ansicht geladen wird. Beispiel:

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

Hier werden mehrere neue Methoden verwendet, die der Auflistungs Ansicht hinzugefügt werden, um den Zieh Vorgang zu implementieren und zu steuern:

- `BeginInteractiveMovementForItem`-Markiert den Anfang eines Verschiebungs Vorgangs.
- `UpdateInteractiveMovementTargetPosition`-Wird gesendet, wenn der Speicherort des Elements aktualisiert wird.
- `EndInteractiveMovement`-Markiert das Ende eines Element Verschiebens.
- `CancelInteractiveMovement`-Markiert den Benutzer, der den Verschiebungs Vorgang abbricht.

Wenn die Anwendung ausgeführt wird, funktioniert der Zieh Vorgang genau wie die standardmäßige Erkennung der Zieh Gesten in der Auflistungs Ansicht.

<a name="Custom-Layouts-and-Reording" />

### <a name="custom-layouts-and-reordering"></a>Benutzerdefinierte Layouts und Neuanordnen

In ios 9 wurden mehrere neue Methoden hinzugefügt, um mit Drag-to-Order-und benutzerdefinierten Layouts in einer Auflistungs Ansicht zu arbeiten. Um dieses Feature zu durchsuchen, fügen wir der Auflistung ein benutzerdefiniertes Layout hinzu.

Fügen Sie zunächst dem Projekt C# eine neue `WaterfallCollectionLayout` Klasse mit dem Namen hinzu. Bearbeiten Sie die Datei, und führen Sie Sie wie folgt aus:

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

Dies kann verwendet werden, um ein benutzerdefiniertes Wasserfall Layout für zwei Spalten für die Auflistungs Ansicht bereitzustellen.
Der Code verwendet Schlüssel-Wert-Codierung (über `WillChangeValue` die `DidChangeValue` -Methode und die-Methode), um Daten Bindungen für unsere berechneten Eigenschaften in dieser Klasse bereitzustellen.

Bearbeiten Sie als nächstes `WaterfallCollectionSource` das, und nehmen Sie die folgenden Änderungen und Ergänzungen vor:

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

Dadurch wird eine zufällige Höhe für jedes Element erstellt, das in der Liste angezeigt wird.

Bearbeiten Sie als nächstes `WaterfallCollectionView` die-Klasse, und fügen Sie die folgende Hilfseigenschaft hinzu:

```csharp
public WaterfallCollectionSource Source {
  get { return (WaterfallCollectionSource)DataSource; }
}
```

Dadurch wird es einfacher, die Datenquelle (und die Elementhöhe) aus dem benutzerdefinierten Layout zu erhalten.

Bearbeiten Sie schließlich den Ansichts Controller, und fügen Sie den folgenden Code hinzu:

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

Dadurch wird eine Instanz des benutzerdefinierten Layouts erstellt, das-Ereignis festgelegt, um die Größe der einzelnen Elemente anzugeben, und das neue Layout wird an unsere Auflistungs Ansicht angefügt.

Wenn die xamarin. IOS-App erneut ausgeführt wird, sieht die Sammlungsansicht nun wie folgt aus:

[![](uicollectionview-images/custom01.png "Die Sammlungsansicht sieht nun wie folgt aus.")](uicollectionview-images/custom01.png#lightbox)

Wir können Elemente weiterhin wie zuvor per Drag & amp; Drop neu anordnen, aber die Größe der Elemente ändert sich nun, damit Sie an den neuen Speicherort angepasst werden, wenn Sie gelöscht werden.

## <a name="collection-view-changes"></a>Änderungen der Sammlungsansicht

In den folgenden Abschnitten werden die Änderungen, die an den einzelnen Klassen in der Sammlungsansicht von IOS 9 vorgenommen werden, ausführlich erläutert.

### <a name="uicollectionview"></a>UICollectionView

Die folgenden Änderungen oder Ergänzungen wurden an der `UICollectionView` -Klasse für IOS 9 vorgenommen:

- `BeginInteractiveMovementForItem`– Markiert den Anfang eines Zieh Vorgangs.
- `CancelInteractiveMovement`– Informiert die Sammlungsansicht darüber, dass der Benutzer einen Zieh Vorgang abgebrochen hat.
- `EndInteractiveMovement`– Informiert die Sammlungsansicht darüber, dass der Benutzer einen Zieh Vorgang abgeschlossen hat.
- `GetIndexPathsForVisibleSupplementaryElements`– Gibt den `indexPath` einer Kopfzeile oder Fußzeile in einem Auflistungs Ansichts Abschnitt zurück.
- `GetSupplementaryView`– Gibt die angegebene Kopfzeile oder Fußzeile zurück.
- `GetVisibleSupplementaryViews`– Gibt eine Liste aller sichtbaren Kopf-und Fußzeilen zurück.
- `UpdateInteractiveMovementTargetPosition`– Informiert die Sammlungsansicht, dass der Benutzer während eines Zieh Vorgangs ein Element verschoben oder verschoben hat.

### <a name="uicollectionviewcontroller"></a>UICollectionViewController

Die folgenden Änderungen oder Ergänzungen wurden an der `UICollectionViewController` -Klasse in ios 9 vorgenommen:

- `InstallsStandardGestureForInteractiveMovement`–, `true` Wenn das neue Gesten Erkennungs Modul verwendet wird, das die Drag-to-Order-Funktion automatisch unterstützt.
- `CanMoveItem`– Informiert die Auflistungs Ansicht, wenn ein angegebenes Element per Drag-up gezeichnet werden kann.
- `GetTargetContentOffset`– Wird verwendet, um den Offset eines angegebenen Auflistungs Ansichts Elements zu erhalten.
- `GetTargetIndexPathForMove`– Ruft den `indexPath` eines bestimmten Elements für einen Zieh Vorgang ab.
- `MoveItem`– Verschiebt die Reihenfolge eines bestimmten Elements in der Liste.

### <a name="uicollectionviewdatasource"></a>UICollectionViewDataSource

Die folgenden Änderungen oder Ergänzungen wurden an der `UICollectionViewDataSource` -Klasse in ios 9 vorgenommen:

- `CanMoveItem`– Informiert die Auflistungs Ansicht, wenn ein angegebenes Element per Drag-up gezeichnet werden kann.
- `MoveItem`– Verschiebt die Reihenfolge eines bestimmten Elements in der Liste.

### <a name="uicollectionviewdelegate"></a>UICollectionViewDelegate

Die folgenden Änderungen oder Ergänzungen wurden an der `UICollectionViewDelegate` -Klasse in ios 9 vorgenommen:

- `GetTargetContentOffset`– Wird verwendet, um den Offset eines angegebenen Auflistungs Ansichts Elements zu erhalten.
- `GetTargetIndexPathForMove`– Ruft den `indexPath` eines bestimmten Elements für einen Zieh Vorgang ab.

### <a name="uicollectionviewflowlayout"></a>UICollectionViewFlowLayout

Die folgenden Änderungen oder Ergänzungen wurden an der `UICollectionViewFlowLayout` -Klasse in ios 9 vorgenommen:

- `SectionFootersPinToVisibleBounds`– Bindet die Abschnitts Fußzeilen an die sichtbaren Begrenzungen der Auflistungs Ansicht.
- `SectionHeadersPinToVisibleBounds`– Bindet die Abschnitts Header an die sichtbaren Begrenzungen der Auflistungs Ansicht.

### <a name="uicollectionviewlayout"></a>UICollectionViewLayout

Die folgenden Änderungen oder Ergänzungen wurden an der `UICollectionViewLayout` -Klasse in ios 9 vorgenommen:

- `GetInvalidationContextForEndingInteractiveMovementOfItems`– Gibt den invalidierungskontext am Ende eines Zieh Vorgangs zurück, wenn der Benutzer das ziehen beendet oder abbricht.
- `GetInvalidationContextForInteractivelyMovingItems`– Gibt den invalidierungskontext zu Beginn eines Zieh Vorgangs zurück.
- `GetLayoutAttributesForInteractivelyMovingItem`– Ruft die Layoutattribute für ein angegebenes Element beim Ziehen eines Elements ab.
- `GetTargetIndexPathForInteractivelyMovingItem`– Gibt den `indexPath` des Elements zurück, das sich am angegebenen Punkt befindet, wenn ein Element gezogen wird.

### <a name="uicollectionviewlayoutattributes"></a>UICollectionViewLayoutAttributes

Die folgenden Änderungen oder Ergänzungen wurden an der `UICollectionViewLayoutAttributes` -Klasse in ios 9 vorgenommen:

- `CollisionBoundingPath`– Gibt während eines Zieh Vorgangs den Kollisions Pfad zweier Elemente zurück.
- `CollisionBoundsType`– Gibt den Typ der Kollision (als `UIDynamicItemCollisionBoundsType`) zurück, die während eines Zieh Vorgangs aufgetreten ist.

### <a name="uicollectionviewlayoutinvalidationcontext"></a>UICollectionViewLayoutInvalidationContext

Die folgenden Änderungen oder Ergänzungen wurden an der `UICollectionViewLayoutInvalidationContext` -Klasse in ios 9 vorgenommen:

- `InteractiveMovementTarget`– Gibt das Ziel Element eines Zieh Vorgangs zurück.
- `PreviousIndexPathsForInteractivelyMovingItems`– Gibt das `indexPaths` -Element anderer Elemente zurück, die an einem Drag to neu anordnen-Vorgang beteiligt sind.
- `TargetIndexPathsForInteractivelyMovingItems`– Gibt den `indexPaths` der Elemente zurück, die als Ergebnis eines Drag-to-Order-Vorgangs neu angeordnet werden.

### <a name="uicollectionviewsource"></a>UICollectionViewSource

Die folgenden Änderungen oder Ergänzungen wurden an der `UICollectionViewSource` -Klasse in ios 9 vorgenommen:

- `CanMoveItem`– Informiert die Auflistungs Ansicht, wenn ein angegebenes Element per Drag-up gezeichnet werden kann.
- `GetTargetContentOffset`– Gibt die Offsets von Elementen zurück, die über einen Drag-to-Order-Vorgang verschoben werden.
- `GetTargetIndexPathForMove`– Gibt den `indexPath` eines Elements zurück, das während eines Drag-to-Order-Vorgangs verschoben wird.
- `MoveItem`– Verschiebt die Reihenfolge eines bestimmten Elements in der Liste.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden die Änderungen an den Sammlungs Ansichten in ios 9 erläutert und beschrieben, wie Sie in xamarin. IOS implementiert werden.
Es wurde beschrieben, wie eine einfache Drag-to-Order-Aktion in einer Auflistungs Ansicht implementiert wird. Verwenden einer benutzerdefinierten Gestenerkennung mit Drag-to-Order und wie sich Drag-to-Order auf ein benutzerdefiniertes Auflistungs Ansichts Layout auswirkt.

## <a name="related-links"></a>Verwandte Links

- [IOS 9-Beispiele](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS9)
- [Beispiel für Sammlungsansicht](https://docs.microsoft.com/samples/xamarin/ios-samples/ios9-collectionview)
- [Simplecollectionview (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/simplecollectionview)
- [Ereignisse, Protokolle und Delegaten](~/ios/app-fundamentals/delegates-protocols-and-events.md)
- [Arbeiten mit Tabellen und Zellen](~/ios/user-interface/controls/tables/index.md)
