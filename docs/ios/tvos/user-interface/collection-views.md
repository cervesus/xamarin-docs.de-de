---
title: Arbeiten mit Auflistungsansichten
description: Dieser Artikel umfasst das Entwerfen von und Arbeiten mit Auflistungsansichten innerhalb einer Xamarin.tvOS-app.
ms.topic: article
ms.prod: xamarin
ms.assetid: 5125C4C7-2DDF-4C19-A362-17BB2B079178
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: f943d6b88d2fd7f38759fb32ecb612e102266657
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="working-with-collection-views"></a>Arbeiten mit Auflistungsansichten

_Dieser Artikel umfasst das Entwerfen von und Arbeiten mit Auflistungsansichten innerhalb einer Xamarin.tvOS-app._

Auflistungsansichten ermöglichen eine Gruppe von Inhalt mithilfe von beliebigen Layouts angezeigt werden sollen. Mithilfe der integrierten Unterstützung, ermöglichen sie die einfache Erstellung Raster-ähnliches oder linearen Layouts beim unterstützen auch benutzerdefinierte Layouts.

[![](collection-views-images/collection01.png "Beispiel-Auflistungsansicht")](collection-views-images/collection01.png#lightbox)

Die Auflistungsansicht verwaltet eine Auflistung von Elementen, die mit einem Delegaten und einer Datenquelle Eingreifen des Benutzers und den Inhalt der Auflistung bereitstellen. Da die Auflistungsansicht auf ein Subsystem Layout, die unabhängig von der die Sicht selbst ist basiert, kann die Darstellung von die Auflistungsansicht Daten auf dynamische Bereitstellung ein anderes Layout leicht ändern.

<a name="About-Collection-Views" />

## <a name="about-collection-views"></a>Informationen zu Auflistungsansichten

Wie bereits erwähnt, einer Auflistungsansicht (`UICollectionView`) verwaltet eine geordnete Auflistung von Elementen und stellt diese Elemente anpassbare Layouts zur Verfügung. Auflistungsansichten in ähnlicher Weise wie Tabellensichten arbeiten (`UITableView`), außer dass sie Layouts vorhanden Elemente in mehr als nur eine einzelne Spalte verwenden können.

Bei Verwendung einer Auflistungsansicht in tvos. außerdem wurden die app ist verantwortlich für das Bereitstellen von Daten, die der Auflistung mit einer Datenquelle zugeordnet sind (`UICollectionViewDataSource`). Ansicht Sammlungsdaten können optional strukturiert und dargestellt werden in unterschiedliche Gruppen (Abschnitte) sein.

Die Ansicht "Auflistung" zeigt die einzelnen Elemente, die auf dem Bildschirm, indem eine Zelle (`UICollectionViewCell`), die die Darstellung von Informationen aus der Auflistung (z. B. ein Bild und den Titel) einen bestimmten bereitstellt.

Zusätzliche Ansichten können optional die Auflistungsansicht Präsentation fungieren als Kopf- und Fußzeilen für die Abschnitte und Zellen hinzugefügt werden. Die Auflistungsansicht Layout ist verantwortlich für die Platzierung dieser Sichten zusammen mit den einzelnen Zellen definieren.

Die Auflistungsansicht kann Antworten auf eine Benutzerinteraktion mithilfe eines Delegaten (`UICollectionViewDelegate`). Dieser Delegat ist auch verantwortlich für die Bestimmung, ob eine bestimmte Zelle den Fokus, abrufen kann, wenn eine Zelle markiert wurde oder wenn eine ausgewählt wurde. In einigen Fällen bestimmt der Delegat die Größe der einzelnen Zellen.

<a name="Collection-View-Layouts" />

## <a name="collection-view-layouts"></a>Auflistung Ansichtslayouts

Ein wichtiges Feature von einer Auflistungsansicht ist die Trennung zwischen den Daten, die sie darstellen, ist und das zugehörige Layout. Eine Auflistung Ansichtslayout (`UICollectionViewLayout`) für die Bereitstellung der Organisation und den Speicherort der Zellen (und alle zusätzlichen Ansichten) mit in der Auflistungsansicht auf dem Bildschirm Präsentation zuständig ist.

Die einzelnen Zellen werden werden durch die Auflistungsansicht einer angefügten Datenquelle erstellt und dann angeordnet und durch die angegebene Auflistung Ansichtslayout angezeigt.

Das Ansichtslayout Auflistung wird normalerweise bereitgestellt werden, wenn die Auflistungsansicht erstellt wird. Allerdings können Sie das Ansichtslayout Auflistung jederzeit ändern und die auf dem Bildschirm Präsentation von Daten für die Auflistungsansicht werden automatisch aktualisiert werden mit dem neuen Layout bereitgestellt.

Das Ansichtslayout Auflistung bietet mehrere Methoden, die zu animierende den Übergang zwischen zwei unterschiedliche Layouts (standardmäßig keine Animation ausgeführt wird) verwendet werden können. Darüber hinaus können Auflistung Ansichtslayouts mit Geste weitere Benutzerinteraktion animieren, die eine Änderung im Layout führt arbeiten.

<a name="Creating-Cells-and-Supplementary-Views" />

## <a name="creating-cells-and-supplementary-views"></a>Erstellen von Zellen und zusätzliche Ansichten

Eine Auflistungsansicht-Datenquelle ist nicht nur verantwortlich für das Bereitstellen von Daten, Sichern der Auflistung Elemente, sondern auch der Zellen, die verwendet werden, um den Inhalt anzuzeigen.

Da Auflistungsansichten entwickelt wurden, um große Auflistungen von Elementen zu behandeln, können die einzelnen Zellen aus der Warteschlange entfernt und erneut verwendet, um zu verhindern, dass Einschränkungen beim Arbeitsspeicher überschritten werden. Es gibt zwei verschiedene Methoden zum Entfernen von Ansichten:

- `DequeueReusableCell` -Erstellt, oder gibt eine Zelle des angegebenen Typs (wie in der app Storyboard angegeben).
- `DequeueReusableSupplementaryView` -Erstellt, oder gibt eine zusätzliche Ansicht des angegebenen Typs (wie in der app Storyboard angegeben).

Sie müssen vor dem Aufrufen dieser Methoden, die Klasse registrieren Storyboard oder `.xib` Datei, die zum Erstellen der Ansicht für die Zelle mit der Auflistungsansicht verwendet. Zum Beispiel:

```csharp
public CityCollectionView (IntPtr handle) : base (handle)
{
    // Initialize
    RegisterClassForCell (typeof(CityCollectionViewCell), CityViewDatasource.CardCellId);
    ...
}
```

Wobei `typeof(CityCollectionViewCell)` enthält die Klasse, die die Ansicht unterstützt und `CityViewDatasource.CardCellId` stellt die ID verwendet, wenn die Zelle (oder Sicht) aus der Warteschlange entfernt wird.

Nach die Zelle aus der Warteschlange entfernt wird, konfigurieren Sie ihn mit den Daten für das Element, den, das es darstellt, ist, und Sie zurück an die Auflistungsansicht für die Anzeige.

<a name="About-Collection-View-Controllers" />

## <a name="about-collection-view-controllers"></a>Zur Auflistung View-Controller

Eine Auflistung-View-Controller (`UICollectionViewController`) ist eine spezielle View-Controller (`UIViewController`), die Folgendes bereitstellt:

- Es ist zuständig für das Laden der Auflistungsansicht aus seiner Storyboard oder `.xib` Datei- und Instanziierung der Ansicht. Wenn im Code erstellt, erstellt er automatisch eine neue, nicht konfigurierte Auflistungsansicht.
- Wenn die Auflistungsansicht geladen ist, versucht der Controller, dessen Datenquelle und der Delegat aus dem Storyboard zu laden oder `.xib` Datei. Wenn keine verfügbar sind, legt er selbst als Quelle für beide fest.
- Stellt sicher, dass die Daten geladen wird, bevor die Auflistungsansicht aufgefüllt wird, auf dem ersten angezeigt wird, und lädt und deaktivieren Sie die SELECT-Anweisung für jede nachfolgende Anzeige.

Darüber hinaus bietet das Collection-View-Controller überschreibbare Methoden, die verwendet werden können, wie z. B. Verwalten des Lebenszyklus der Auflistungsansicht `AwakeFromNib` und `ViewWillDisplay`.

<a name="Collection-Views-and-Storyboards" />

## <a name="collection-views-and-storyboards"></a>Auflistungsansichten und Storyboards

Die einfachste Möglichkeit zum Arbeiten mit einer Auflistungsansicht in Ihrer app Xamarin.tvOS ist das Storyboard hinzufügen. Als ein kurzes Beispiel werden wir eine Beispiel-app erstellen, in dem ein Bild, Titel und eine Schaltfläche auswählen dargestellt. Wenn der Benutzer klicken Sie auf die Schaltfläche auswählen, wird eine Auflistungsansicht angezeigt, mit denen den Benutzer ein neues Image auswählen können. Wenn ein Bild ausgewählt ist, wird die Auflistungsansicht wird geschlossen, und das neue Image und Titel angezeigt werden.

Führen Sie wir Folgendes:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

    
1. Starten Sie einen neuen **Ansicht tvos. außerdem wurden App** in Visual Studio für Mac.
1. In der **Projektmappen-Explorer**, doppelklicken Sie auf die `Main.storyboard` Datei und in der iOS-Designer zu öffnen.
1. Die vorhandene Sicht ein Bild anzeigen, eine Bezeichnung und eine Schaltfläche hinzu, und konfigurieren Sie sie wie folgt aussehen: 

    [![](collection-views-images/collection02.png "Beispiel-layout")](collection-views-images/collection02.png#lightbox)
1. Zuweisen einer **Namen** der Image-Ansicht und die Bezeichnung in der **Registerkarte "Widget"** von der **Eigenschaften-Explorer**. Zum Beispiel: 

    [![](collection-views-images/collection03.png "Der Name der Einstellung")](collection-views-images/collection03.png#lightbox)
1. Ziehen Sie anschließend eine Auflistung-View-Controller auf das Storyboard: 

    [![](collection-views-images/collection04.png "Eine Auflistung-View-Controller")](collection-views-images/collection04.png#lightbox)
1. Steuerelement ziehen über die Schaltfläche mit dem Auflistung View-Controller, und wählen Sie **Push** im Popupmenü: 

    [![](collection-views-images/collection05.png "Wählen Sie im Popupmenü Push")](collection-views-images/collection05.png#lightbox)
1. Wenn die app ausgeführt wird, wird dies stellen die Auflistungsansicht angezeigt werden, sobald der Benutzer die Schaltfläche klickt.
1. Wählen Sie die Auflistungsansicht, und geben Sie die folgenden Werte in der **Registerkarte "Layout"** von der **Eigenschaften-Explorer**: 

    [![](collection-views-images/collection06.png "Im Eigenschaften-Explorer")](collection-views-images/collection06.png#lightbox)
1. Steuert die Größe der einzelnen Zellen und die Rahmenlinien zwischen den Zellen und äußeren Rand der Auflistungsansicht.
1. Wählen Sie die Collection-View-Controller, und legen Sie die Klasse den `CityCollectionViewController` in der **Registerkarte "Widget"**: 

    [![](collection-views-images/collection07.png "Legen Sie die Klasse den CityCollectionViewController")](collection-views-images/collection07.png#lightbox)
1. Wählen Sie die Auflistungsansicht, und legen Sie die Klasse den `CityCollectionView` in der **Registerkarte "Widget"**: 

    [![](collection-views-images/collection08.png "Legen Sie die Klasse den CityCollectionView")](collection-views-images/collection08.png#lightbox)
1. Wählen Sie die Zelle der Auflistung anzeigen, und legen Sie die Klasse den `CityCollectionViewCell` in der **Registerkarte "Widget"**: 

    [![](collection-views-images/collection09.png "Legen Sie die Klasse den CityCollectionViewCell")](collection-views-images/collection09.png#lightbox)
1. In der **Registerkarte "Widget"** sicher, dass die **Layout** ist `Flow` und **Scroll Richtung** ist `Vertical` für die Auflistungsansicht: 

    [![](collection-views-images/collection10.png "Die Registerkarte "Widget"")](collection-views-images/collection10.png#lightbox)
1. Wählen Sie die Zelle der Auflistung anzeigen, und legen seine **Identität** auf `CityCell` in der **Registerkarte "Widget"**: 

    [![](collection-views-images/collection11.png "Legen Sie die Identität auf CityCell")](collection-views-images/collection11.png#lightbox)
1. Speichern Sie die Änderungen.
    

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

    
1. Starten Sie einen neuen **Ansicht tvos. außerdem wurden App** in Visual Studio.
1. In der **Projektmappen-Explorer**, doppelklicken Sie auf die `Main.storyboard` Datei und in der iOS-Designer zu öffnen.
1. Die vorhandene Sicht ein Bild anzeigen, eine Bezeichnung und eine Schaltfläche hinzu, und konfigurieren Sie sie wie folgt aussehen: 

    [![](collection-views-images/collection02vs.png "Konfigurieren Sie das layout")](collection-views-images/collection02vs.png#lightbox)
1. Zuweisen einer **Namen** der Image-Ansicht und die Bezeichnung in der **Registerkarte "Widget"** von der **Eigenschaften-Explorer**. Zum Beispiel: 

    [![](collection-views-images/collection03vs.png "Im Eigenschaften-Explorer")](collection-views-images/collection03vs.png#lightbox)
1. Ziehen Sie anschließend eine Auflistung-View-Controller auf das Storyboard: 

    [![](collection-views-images/collection04vs.png "Eine Auflistung-View-Controller")](collection-views-images/collection04vs.png#lightbox)
1. Steuerelement ziehen über die Schaltfläche mit dem Auflistung View-Controller, und wählen Sie **Push** im Popupmenü: 

    [![](collection-views-images/collection05vs.png "Wählen Sie im Popupmenü Push")](collection-views-images/collection05vs.png#lightbox)
1. Wenn die app ausgeführt wird, wird dies stellen die Auflistungsansicht angezeigt werden, sobald der Benutzer die Schaltfläche klickt.
1. Wählen Sie die Auflistungsansicht und klicken Sie in der **Registerkarte "Layout"** von der **Eigenschaften-Explorer** Geben Sie die **Breite** als _361_ und  **Höhe** als _256_ 
1. Steuert die Größe der einzelnen Zellen und die Rahmenlinien zwischen den Zellen und äußeren Rand der Auflistungsansicht.
1. Wählen Sie die Collection-View-Controller, und legen Sie die Klasse den `CityCollectionViewController` in der **Registerkarte "Widget"**: 

    [![](collection-views-images/collection07vs.png "Legen Sie die Klasse den CityCollectionViewController")](collection-views-images/collection07vs.png#lightbox)
1. Wählen Sie die Auflistungsansicht, und legen Sie die Klasse den `CityCollectionView` in der **Registerkarte "Widget"**: 

    [![](collection-views-images/collection08vs.png "Legen Sie die Klasse den CityCollectionView")](collection-views-images/collection08vs.png#lightbox)
1. Wählen Sie die Zelle der Auflistung anzeigen, und legen Sie die Klasse den `CityCollectionViewCell` in der **Registerkarte "Widget"**: 

    [![](collection-views-images/collection09vs.png "Legen Sie die Klasse den CityCollectionViewCell")](collection-views-images/collection09vs.png#lightbox)
1. In der **Registerkarte "Widget"** sicher, dass die **Layout** ist `Flow` und **Scroll Richtung** ist `Vertical` für die Auflistungsansicht: 

    [![](collection-views-images/collection10vs.png "Registerkarte "%tdas Widget"")](collection-views-images/collection10vs.png#lightbox)
1. Wählen Sie die Zelle der Auflistung anzeigen, und legen seine **Identität** auf `CityCell` in der **Registerkarte "Widget"**: 

    [![](collection-views-images/collection11vs.png "Legen Sie die Identität auf CityCell")](collection-views-images/collection11vs.png#lightbox)
1. Speichern Sie die Änderungen.
    

-----

Wenn wir ausgewählt hatten `Custom` für die Auflistungsansicht **Layout**, hätten wir ein benutzerdefiniertes Layout. Apple bietet eine integrierte `UICollectionViewFlowLayout` und `UICollectionViewDelegateFlowLayout` können problemlos Daten in einem Layout rasterbasierte vorweisen (diese werden verwendet, durch die `flow` Layoutstil). 

Weitere Informationen zum Arbeiten mit Storyboards finden Sie unter unsere [Hello tvos. außerdem wurden Quick Start Guide](~/ios/tvos/get-started/hello-tvos.md).

<a name="Providing-Data-for-the-Collection-View" />

## <a name="providing-data-for-the-collection-view"></a>Bereitstellen von Daten für die Auflistungsansicht

Nachdem wir unsere Auflistungsansicht (und Auflistung View Controller) an unsere Storyboard hinzugefügt haben, müssen wir die Daten für die Sammlung bereitstellen. 

<a name="The-Data-Model" />

### <a name="the-data-model"></a>Das Datenmodell

Zunächst werden wir ein Modell für unsere Daten zu erstellen, der den Dateinamen für das anzuzeigende Bild, Titel und ein Flag für das Zulassen der Stadt ausgewählt werden.

Erstellen einer `CityInfo` Klasse, und stellen sie wie folgt aussehen:

```csharp
using System;

namespace tvCollection
{
    public class CityInfo
    {
        #region Computed Properties
        public string ImageFilename { get; set; }
        public string Title { get; set; }
        public bool CanSelect{ get; set; }
        #endregion

        #region Constructors
        public CityInfo (string filename, string title, bool canSelect)
        {
            // Initialize
            this.ImageFilename = filename;
            this.Title = title;
            this.CanSelect = canSelect;
        }
        #endregion
    }
}
```

### <a name="the-collection-view-cell"></a>Die Auflistung Ansicht Zelle

Nun müssen wir definieren, wie die Daten für jede Zelle angezeigt werden. Bearbeiten der `CityCollectionViewCell.cs` Datei (erstellt für Sie automatisch aus der Storyboard-Datei), und stellen sie wie folgt aussehen:

```csharp
using System;
using Foundation;
using UIKit;
using CoreGraphics;

namespace tvCollection
{
    public partial class CityCollectionViewCell : UICollectionViewCell
    {
        #region Private Variables
        private CityInfo _city;
        #endregion

        #region Computed Properties
        public UIImageView CityView { get ; set; }
        public UILabel CityTitle { get; set; }

        public CityInfo City {
            get { return _city; }
            set {
                _city = value;
                CityView.Image = UIImage.FromFile (City.ImageFilename);
                CityView.Alpha = (City.CanSelect) ? 1.0f : 0.5f;
                CityTitle.Text = City.Title;
            }
        }
        #endregion

        #region Constructors
        public CityCollectionViewCell (IntPtr handle) : base (handle)
        {
            // Initialize
            CityView = new UIImageView(new CGRect(22, 19, 320, 171));
            CityView.AdjustsImageWhenAncestorFocused = true;
            AddSubview (CityView);

            CityTitle = new UILabel (new CGRect (22, 209, 320, 21)) {
                TextAlignment = UITextAlignment.Center,
                TextColor = UIColor.White,
                Alpha = 0.0f
            };
            AddSubview (CityTitle);
        }
        #endregion
    

    }
}
```

Für unsere tvos. außerdem wurden-app werden wir ein Abbild und einen optionalen Titel angezeigt. Wenn der angegebene Ort kann nicht ausgewählt werden, werden wir die Image-Sicht mithilfe des folgenden Codes Abblenden:

```csharp
CityView.Alpha = (City.CanSelect) ? 1.0f : 0.5f;
```

Wenn die Zelle mit dem Bild in den Fokus vom Benutzer geschaltet wird, möchten wir die integrierte Parallax Auswirkungen auf die folgende Eigenschaft festlegen:

```csharp
CityView.AdjustsImageWhenAncestorFocused = true;
```

Weitere Informationen zu navigieren und den Fokus, finden Sie unter unsere [arbeiten mit Navigationsbereich und den Fokus](~/ios/tvos/app-fundamentals/navigation-focus.md) und [Siri Remote und Bluetooth-Controller](~/ios/tvos/platform/remote-bluetooth.md) Dokumentation.


<a name="The-Collection-View-Data-Provider" />

### <a name="the-collection-view-data-provider"></a>Der Datenanbieter der Auflistung anzeigen

Erstellen Sie mit unseren Datenmodell erstellt und unsere Zelle-Layout definiert eine Datenquelle wir für unsere Auflistungsansicht. Die Datenquelle wird dafür verantwortlich, nicht nur die Daten sichern, sondern auch entfernen die Zellen aus, um die einzelnen Zellen, die auf dem Bildschirm angezeigt werden.

Erstellen einer `CityViewDatasource` Klasse, und stellen sie wie folgt aussehen:

```csharp
using System;
using System.Collections.Generic;
using UIKit;
using Foundation;
using CoreGraphics;
using ObjCRuntime;

namespace tvCollection
{
    public class CityViewDatasource : UICollectionViewDataSource
    {
        #region Application Access
        public static AppDelegate App {
            get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
        }
        #endregion

        #region Static Constants
        public static NSString CardCellId = new NSString ("CityCell");
        #endregion

        #region Computed Properties
        public List<CityInfo> Cities { get; set; } = new List<CityInfo>();
        public CityCollectionView ViewController { get; set; }
        #endregion

        #region Constructors
        public CityViewDatasource (CityCollectionView controller)
        {
            // Initialize
            this.ViewController = controller;
            PopulateCities ();
        }
        #endregion

        #region Public Methods
        public void PopulateCities() {

            // Clear existing cities
            Cities.Clear();

            // Add new cities
            Cities.Add(new CityInfo("City01.jpg", "Houses by Water", false));
            Cities.Add(new CityInfo("City02.jpg", "Turning Circle", true));
            Cities.Add(new CityInfo("City03.jpg", "Skyline at Night", true));
            Cities.Add(new CityInfo("City04.jpg", "Golden Gate Bridge", true));
            Cities.Add(new CityInfo("City05.jpg", "Roads by Night", true));
            Cities.Add(new CityInfo("City06.jpg", "Church Domes", true));
            Cities.Add(new CityInfo("City07.jpg", "Mountain Lights", true));
            Cities.Add(new CityInfo("City08.jpg", "City Scene", false));
            Cities.Add(new CityInfo("City09.jpg", "House in Winter", true));
            Cities.Add(new CityInfo("City10.jpg", "By the Lake", true));
            Cities.Add(new CityInfo("City11.jpg", "At the Dome", true));
            Cities.Add(new CityInfo("City12.jpg", "Cityscape", true));
            Cities.Add(new CityInfo("City13.jpg", "Model City", true));
            Cities.Add(new CityInfo("City14.jpg", "Taxi, Taxi!", true));
            Cities.Add(new CityInfo("City15.jpg", "On the Sidewalk", true));
            Cities.Add(new CityInfo("City16.jpg", "Midnight Walk", true));
            Cities.Add(new CityInfo("City17.jpg", "Lunchtime Cafe", true));
            Cities.Add(new CityInfo("City18.jpg", "Coffee Shop", true));
            Cities.Add(new CityInfo("City19.jpg", "Rustic Tavern", true));
        }
        #endregion

        #region Override Methods
        public override nint NumberOfSections (UICollectionView collectionView)
        {
            return 1;
        }

        public override nint GetItemsCount (UICollectionView collectionView, nint section)
        {
            return Cities.Count;
        }

        public override UICollectionViewCell GetCell (UICollectionView collectionView, NSIndexPath indexPath)
        {
            var cityCell = (CityCollectionViewCell)collectionView.DequeueReusableCell (CardCellId, indexPath);
            var city = Cities [indexPath.Row];

            // Initialize city
            cityCell.City = city;

            return cityCell;
        }
        #endregion
    }
}
```

Können Sie diese Klasse im Detail untersuchen. Erstens wir Vererben `UICollectionViewDataSource` , und geben Sie eine Verknüpfung auf die Zellen-ID (die wir in der iOS-Designer zugewiesen):

```csharp
public static NSString CardCellId = new NSString ("CityCell");
```

Als Nächstes geben Sie Speicher für unsere Sammlungsdaten und geben Sie eine Klasse zum Auffüllen der Daten:

```csharp
public List<CityInfo> Cities { get; set; } = new List<CityInfo>();
...

public void PopulateCities() {

    // Clear existing cities
    Cities.Clear();

    // Add new cities
    Cities.Add(new CityInfo("City01.jpg", "Houses by Water", false));
    Cities.Add(new CityInfo("City02.jpg", "Turning Circle", true));
    ...
}
```

Wir überschreiben die `NumberOfSections` -Methode und der Rückgabewert die Anzahl der Abschnitte (Gruppen von Elementen), die die Auflistung anzeigen ist. In diesem Fall besteht nur ein:

```csharp
public override nint NumberOfSections (UICollectionView collectionView)
{
    return 1;
}
```

Als Nächstes geben wir die Anzahl der Elemente in der Auflistung mithilfe des folgenden Codes zurück:

```csharp
public override nint GetItemsCount (UICollectionView collectionView, nint section)
{
    return Cities.Count;
}
```

Schließlich dequeue wir eine wieder verwendbare Zelle aus, wenn die Auflistungsansicht anfordern, durch den folgenden Code:

```csharp
public override UICollectionViewCell GetCell (UICollectionView collectionView, NSIndexPath indexPath)
{
    var cityCell = (CityCollectionViewCell)collectionView.DequeueReusableCell (CardCellId, indexPath);
    var city = Cities [indexPath.Row];

    // Initialize city
    cityCell.City = city;

    return cityCell;
}
```

Nach dem erhalten wir eine Auflistung Ansicht Zelle unserer `CityCollectionViewCell` Typ, wir weisen Sie ihm das angegebene Element.

<a name="Responding-to-User-Events" />

## <a name="responding-to-user-events"></a>Reagieren auf Ereignisse

Da wir die Benutzer können aus der Auflistung ein Element ausgewählt werden soll, müssen wir Bereitstellen einer Sammlung anzeigen-Delegat, der diese Interaktion zu behandeln. Und es müssen zur Verfügung zu stellen eine Möglichkeit, können unsere aufrufende Sicht wissen, welches Element der Benutzer ausgewählt hat.

<a name="The-App-Delegate" />

### <a name="the-app-delegate"></a>Der App-Delegat

Wir benötigen eine Möglichkeit, das aktuell ausgewählte Element aus der Auflistungsansicht zurück an die aufrufende Sicht beziehen. Wir werden eine benutzerdefinierte Eigenschaft wird auf mithilfe unserer `AppDelegate`. Bearbeiten der `AppDelegate.cs` Datei, und fügen Sie den folgenden Code hinzu:

```csharp
public CityInfo SelectedCity { get; set;} = new CityInfo("City02.jpg", "Turning Circle", true);
```

Definiert die Eigenschaft, und legt die Standard-Stadt, die anfänglich angezeigt wird. Später werden wir diese Eigenschaft, um die Auswahl des Benutzers anzeigen und auswählen zu ändernden ermöglichen nutzen.

<a name="The-Collection-View-Delegate" />

### <a name="the-collection-view-delegate"></a>Der Delegat der Auflistung anzeigen

Als Nächstes fügen Sie einen neuen `CityViewDelegate` -Klasse auf das Projekt, und stellen sie wie folgt aussehen:


```csharp
using System;
using System.Collections.Generic;
using UIKit;
using Foundation;
using CoreGraphics;

namespace tvCollection
{
    public class CityViewDelegate : UICollectionViewDelegateFlowLayout
    {
        #region Application Access
        public static AppDelegate App {
            get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
        }
        #endregion

        #region Constructors
        public CityViewDelegate ()
        {
        }
        #endregion

        #region Override Methods
        public override CGSize GetSizeForItem (UICollectionView collectionView, UICollectionViewLayout layout, NSIndexPath indexPath)
        {
            return new CGSize (361, 256);
        }

        public override bool CanFocusItem (UICollectionView collectionView, NSIndexPath indexPath)
        {
            if (indexPath == null) {
                return false;
            } else {
                var controller = collectionView as CityCollectionView;
                return controller.Source.Cities[indexPath.Row].CanSelect;
            }
        }

        public override void ItemSelected (UICollectionView collectionView, NSIndexPath indexPath)
        {
            var controller = collectionView as CityCollectionView;
            App.SelectedCity = controller.Source.Cities [indexPath.Row];

            // Close Collection
            controller.ParentController.DismissViewController(true,null);
        }
        #endregion
    }
}
```

Werfen Sie näher auf diese Klasse an. Erstens wir Vererben `UICollectionViewDelegateFlowLayout`. Der Grund für die wir von dieser Klasse erben und nicht die `UICollectionViewDelegate` ist, dass wir die integrierte verwenden `UICollectionViewFlowLayout` unsere Elemente und nicht auf ein benutzerdefiniertes Layout-Typ darstellen.

Als Nächstes geben wir die Größe der einzelnen Elemente, die Verwendung dieses Codes zurück:

```csharp
public override CGSize GetSizeForItem (UICollectionView collectionView, UICollectionViewLayout layout, NSIndexPath indexPath)
{
    return new CGSize (361, 256);
}
```

Wir entscheiden dann, wenn eine Zelle den Fokus mithilfe des folgenden Codes abrufen kann: 

```csharp
public override bool CanFocusItem (UICollectionView collectionView, NSIndexPath indexPath)
{
    if (indexPath == null) {
        return false;
    } else {
        var controller = collectionView as CityCollectionView;
        return controller.Source.Cities[indexPath.Row].CanSelect;
    }
}
```

Wir prüfen, um festzustellen, ob es sich bei ein bestimmter Teil der dahinter liegende Daten hat seine `CanSelect` Flag festgelegt, um `true` und gibt diesen Wert zurück. Weitere Informationen zu navigieren und den Fokus, finden Sie unter unsere [arbeiten mit Navigationsbereich und den Fokus](~/ios/tvos/app-fundamentals/navigation-focus.md) und [Siri Remote und Bluetooth-Controller](~/ios/tvos/platform/remote-bluetooth.md) Dokumentation.

Schließlich Antworten es dem Benutzer das Auswählen eines Elements durch den folgenden Code:

```csharp
public override void ItemSelected (UICollectionView collectionView, NSIndexPath indexPath)
{
    var controller = collectionView as CityCollectionView;
    App.SelectedCity = controller.Source.Cities [indexPath.Row];

    // Close Collection
    controller.ParentController.DismissViewController(true,null);
}
```

Legen wir den `SelectedCity` Eigenschaft unsere `AppDelegate` auf das Element, dass der Benutzer ausgewählt, und wir schließen die Collection-View-Controller Rückgabe an die Sicht, die uns aufgerufen. Wir noch keinen definiert die `ParentController` -Eigenschaft noch unser Auflistungsansicht können wir erledigen, weiter.

<a name="Configuring-the-Collection-View" />

## <a name="configuring-the-collection-view"></a>Konfigurieren die Auflistungsansicht

Nun müssen wir unsere Auflistungsansicht bearbeiten, und weisen Sie unsere Datenquelle und Delegaten. Bearbeiten der `CityCollectionView.cs` (automatisch für uns erstellt aus unserem Storyboard) Datei, und stellen sie wie folgt aussehen:

```csharp
using System;
using Foundation;
using UIKit;

namespace tvCollection
{
    public partial class CityCollectionView : UICollectionView
    {
        #region Application Access
        public static AppDelegate App {
            get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
        }
        #endregion

        #region Computed Properties
        public CityViewDatasource Source {
            get { return DataSource as CityViewDatasource;}
        }

        public CityCollectionViewController ParentController { get; set;}
        #endregion

        #region Constructors
        public CityCollectionView (IntPtr handle) : base (handle)
        {
            // Initialize
            RegisterClassForCell (typeof(CityCollectionViewCell), CityViewDatasource.CardCellId);
            DataSource = new CityViewDatasource (this);
            Delegate = new CityViewDelegate ();
        }
        #endregion

        #region Override Methods
        public override nint NumberOfSections ()
        {
            return 1;
        }

        public override void DidUpdateFocus (UIFocusUpdateContext context, UIFocusAnimationCoordinator coordinator)
        {
            var previousItem = context.PreviouslyFocusedView as CityCollectionViewCell;
            if (previousItem != null) {
                Animate (0.2, () => {
                    previousItem.CityTitle.Alpha = 0.0f;
                });
            }

            var nextItem = context.NextFocusedView as CityCollectionViewCell;
            if (nextItem != null) {
                Animate (0.2, () => {
                    nextItem.CityTitle.Alpha = 1.0f;
                });
            }
        }
        #endregion
    }
}
```

Wir geben Sie zuerst eine Verknüpfung zum Zugriff auf unsere `AppDelegate`: 

```csharp
public static AppDelegate App {
    get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
}
```

Als Nächstes stellen wir eine Verknüpfung, die Auflistungsansicht Datenquelle sowie eine Eigenschaft, um Zugriff auf die Auflistung-View-Controller (durch unsere oben Delegaten, um die Auflistung zu schließen, wenn der Benutzer eine Auswahl trifft verwendet):

```csharp
public CityViewDatasource Source {
    get { return DataSource as CityViewDatasource;}
}

public CityCollectionViewController ParentController { get; set;}
```

Verwenden Sie dann den folgenden Code zum Initialisieren der Auflistungsansicht und weisen Sie unsere Zellenklasse, Datenquelle und Delegaten:

```csharp
public CityCollectionView (IntPtr handle) : base (handle)
{
    // Initialize
    RegisterClassForCell (typeof(CityCollectionViewCell), CityViewDatasource.CardCellId);
    DataSource = new CityViewDatasource (this);
    Delegate = new CityViewDelegate ();
}
```

Zum Schluss möchten wir den Titel unter dem Bild nur angezeigt werden, sobald der Benutzer hervorgehoben ist (bildschärfenmodus). Wir führen, die durch den folgenden Code:

```csharp
public override void DidUpdateFocus (UIFocusUpdateContext context, UIFocusAnimationCoordinator coordinator)
{
    var previousItem = context.PreviouslyFocusedView as CityCollectionViewCell;
    if (previousItem != null) {
        Animate (0.2, () => {
            previousItem.CityTitle.Alpha = 0.0f;
        });
    }

    var nextItem = context.NextFocusedView as CityCollectionViewCell;
    if (nextItem != null) {
        Animate (0.2, () => {
            nextItem.CityTitle.Alpha = 1.0f;
        });
    }
}
```

Wir setzen die Transparence des vorherigen Elements Fokus um NULL (0), und die Transparence des nächsten Elements erhalten, den Fokus auf 100 %. Dieser Übergang abrufen sowie animiert.


## <a name="configuring-the-collection-view-controller"></a>Konfigurieren der Sammlung-View-Controller

Nun müssen wir die endgültige Konfiguration auf unserem Auflistungsansicht ausführen und ermöglichen den Controller aus, um die Eigenschaft festgelegt, die es definiert wurden, sodass die Auflistungsansicht geschlossen werden kann, nachdem der Benutzer eine Auswahl trifft.

Bearbeiten der `CityCollectionViewController.cs` (aus unserem Storyboard automatisch erstellte) Datei, und stellen sie wie folgt aussehen:

```csharp
// This file has been autogenerated from a class added in the UI designer.

using System;

using Foundation;
using UIKit;

namespace tvCollection
{
    public partial class CityCollectionViewController : UICollectionViewController
    {
        #region Computed Properties
        public CityCollectionView Collection {
            get { return CollectionView as CityCollectionView; }
        }
        #endregion

        #region Constructors
        public CityCollectionViewController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void AwakeFromNib ()
        {
            base.AwakeFromNib ();

            // Save link to controller
            Collection.ParentController = this;
        }
        #endregion
    }
}

```

## <a name="putting-it-all-together"></a>Alles zusammenführen 

Nun mit dem alle Teile zusammengestellt zum Auffüllen und unsere Auflistungsansicht steuern wir die letzten Änderungen an unseren Hauptansicht für alles, was das Zusammenführen vornehmen müssen.

Bearbeiten der `ViewController.cs` (aus unserem Storyboard automatisch erstellte) Datei, und stellen sie wie folgt aussehen:

```csharp
using System;
using Foundation;
using UIKit;
using tvCollection;

namespace MySingleView
{
    public partial class ViewController : UIViewController
    {
        #region Application Access
        public static AppDelegate App {
            get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
        }
        #endregion

        #region Constructors
        public ViewController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();
            // Perform any additional setup after loading the view, typically from a nib.
        }

        public override void ViewWillAppear (bool animated)
        {
            base.ViewWillAppear (animated);

            // Update image with the currently selected one
            CityView.Image = UIImage.FromFile(App.SelectedCity.ImageFilename);
            BackgroundView.Image = CityView.Image;
            CityTitle.Text = App.SelectedCity.Title;
        }

        public override void DidReceiveMemoryWarning ()
        {
            base.DidReceiveMemoryWarning ();
            // Release any cached data, images, etc that aren't in use.
        }
        #endregion
    }
}
```

Der folgende Code zeigt Anfangs an das ausgewählte Element aus der `SelectedCity` Eigenschaft von der `AppDelegate` und wird es erneut angezeigt, wenn der Benutzer eine Auswahl aus der Auflistungsansicht erzielt hat:

```csharp
public override void ViewWillAppear (bool animated)
{
    base.ViewWillAppear (animated);

    // Update image with the currently selected one
    CityView.Image = UIImage.FromFile(App.SelectedCity.ImageFilename);
    BackgroundView.Image = CityView.Image;
    CityTitle.Text = App.SelectedCity.Title;
}
```

<a name="Testing-the-app" />

## <a name="testing-the-app"></a>Testen der app

Wenn Sie beim Erstellen und die Anwendung ausführen, wird die Hauptansicht alles vorhanden mit dem Standard-Ort angezeigt:

[![](collection-views-images/run01.png "Der Hauptbildschirm")](collection-views-images/run01.png#lightbox)

Wenn der Benutzer auf die **wählen Sie eine Ansicht** Schaltfläche, die Auflistungsansicht wird angezeigt:

[![](collection-views-images/run02.png "Die Auflistungsansicht")](collection-views-images/run02.png#lightbox)

Jeder Ort, den hat seine `CanSelect` -Eigenschaftensatz auf `false` angezeigt abgeblendet ist und der Benutzer wird nicht in der Lage, den Fokus auf den sie festgelegt. Wenn der Benutzer ein Element hervorgehoben (Stellen sie in den Fokus hat) der Titel angezeigt wird und sie können den Effekt Parallax Besonderheit Neigung das Bild in 3D.

Klickt der Benutzer ein Bild auswählen, wird die Auflistungsansicht wird geschlossen, und die Hauptansicht wird erneut angezeigt, mit dem neuen Image:

[![](collection-views-images/run03.png "Ein neues Image auf dem Startbildschirm")](collection-views-images/run03.png#lightbox)

<a name="Creating-Custom-Layout-and-Reordering-Items" />

## <a name="creating-custom-layout-and-reordering-items"></a>Erstellen benutzerdefinierte Layouts und Neuanordnen von Elementen

Einer der wichtigsten Funktionen der Verwendung einer Auflistungsansicht ist die Fähigkeit zum Erstellen von benutzerdefinierten Layouts. Da tvos. außerdem wurden von iOS erbt, ist der Prozess zum Erstellen eines benutzerdefinierten Layouts identisch. Finden Sie in unserem [Einführung zu Auflistungsansichten](~/ios/user-interface/controls/uicollectionview.md) Dokumentation weitere Informationen.

Vor kurzem für iOS zu Auflistungsansichten hinzugefügt wurde 9 die Möglichkeit, leicht ermöglichen die neuanordnung von Elementen in der Auflistung. Erneut, da tvos. außerdem wurden 9 eine Teilmenge von iOS 9 ist, dies wird diese gleiche Weise. Finden Sie in unserer [Ansicht Sammlungsänderungen](~/ios/user-interface/controls/uicollectionview.md) Dokument Weitere Informationen.


<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden behandelt, Entwerfen und Arbeiten mit Auflistungsansichten innerhalb einer Xamarin.tvOS-app. Zuerst erläutert alle Elemente, aus denen die Auflistungsansicht besteht. Als Nächstes wurde es gezeigt, wie entwerfen und Implementieren einer Auflistungsansicht mithilfe eines Storyboards. Schließlich wird Links zu Informationen zum Erstellen von benutzerdefinierten Layouts und Neuanordnen von Elementen bereitgestellt.



## <a name="related-links"></a>Verwandte Links

- [tvOS-Beispiele](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvos. außerdem wurden Handbücher für interaktive Workflowdienste-Schnittstelle](https://developer.apple.com/tvos/human-interface-guidelines/)
- [App-Programmierhandbuch für tvos. außerdem wurden](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
