---
title: Arbeiten mit TvOS Auflistungsansichten in Xamarin
description: Dieses Dokument beschreibt das Arbeiten mit Auflistungsansichten in einer TvOS-app mit Xamarin erstellt wurde. Hierin sind Auflistung Ansichtslayouts, Erstellen von Zellen und zusätzliche Ansichten, reagieren auf Ereignisse und vieles mehr.
ms.prod: xamarin
ms.assetid: 5125C4C7-2DDF-4C19-A362-17BB2B079178
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: f815afa6b1abb15348019b0c53333b4acb054008
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "60933721"
---
# <a name="working-with-tvos-collection-views-in-xamarin"></a>Arbeiten mit TvOS Auflistungsansichten in Xamarin

Auflistungsansichten können für eine Gruppe von Inhalten mithilfe von beliebigen Layouts angezeigt werden sollen. Verwenden die integrierten Unterstützung, ermöglichen sie die einfache Erstellung Raster-ähnliches oder linearen Layouts gleichzeitig unterstützt auch benutzerdefinierte Layouts.

[![](collection-views-images/collection01.png "Beispielansicht für die Sammlung")](collection-views-images/collection01.png#lightbox)

Die Auflistungsansicht verwaltet eine Auflistung von Elementen, die mithilfe eines Delegaten und einer Datenquelle Eingreifen des Benutzers und den Inhalt der Auflistung bereitstellen. Da die Auflistungsansicht in einem Subsystem Layout basiert, der die Sicht selbst unabhängig ist, kann die Darstellung der Auflistungsansicht Daten auf dynamische Bereitstellung von einem anderen Layout leicht ändern.

<a name="About-Collection-Views" />

## <a name="about-collection-views"></a>Über die Auflistungsansichten

Wie bereits erwähnt, eine Auflistungsansicht (`UICollectionView`) verwaltet eine geordnete Auflistung von Elementen und zeigt diese Elemente mit anpassbaren Layouts. Auflistungsansichten funktionieren auf ähnliche Weise zu Tabellensichten (`UITableView`), es sei denn sie Layouts vorhanden Elemente in mehr als nur eine einzelne Spalte verwenden können.

Ihre app ist verantwortlich für das Bereitstellen der Daten im Zusammenhang mit der Auflistung unter Verwendung einer Datenquelle, bei Verwendung einer Auflistungsansicht in TvOS (`UICollectionViewDataSource`). Optional können Auflistung anzeigen von Daten organisiert und in unterschiedliche Gruppen (Abschnitte) angezeigt werden.

Die Auflistungsansicht stellt die einzelnen Elemente, die auf dem Bildschirm, indem eine Zelle (`UICollectionViewCell`), die die Darstellung der eine bestimmte Information aus der Auflistung (z. B. ein Bild und Titel) bereitstellt.

Zusätzliche Ansichten können optional die Auflistungsansicht Präsentation fungieren als Kopf- und Fußzeilen für die Abschnitte und Zellen hinzugefügt werden. Die Auflistungsansicht Layout ist verantwortlich für die Platzierung von diesen Ansichten zusammen mit der die einzelnen Zellen definieren.

Die Auflistungsansicht kann auf eine Benutzerinteraktion mit einem Delegaten reagieren (`UICollectionViewDelegate`). Dieser Delegat ist auch zuständig für das ermitteln, wenn eine bestimmte Zelle den Fokus besitzt, abrufen kann, wenn eine Zelle hervorgehoben wurden wurde oder eine ausgewählt wurde. In einigen Fällen bestimmt der Delegat, der Größe der einzelnen Zellen.

<a name="Collection-View-Layouts" />

## <a name="collection-view-layouts"></a>Sammlung von Ansichtslayouts

Ein wichtiges Feature von eine Auflistungsansicht ist die Trennung zwischen den Daten, die sie darstellen, ist und das zugehörige Layout. Eine Auflistung der Standardankündigung (`UICollectionViewLayout`) Dient zum Bereitstellen der Organisation und den Speicherort der Zellen (und keine zusätzlichen Ansichten) in der Auflistungsansicht auf dem Bildschirm Präsentation.

Die einzelnen Zellen werden werden durch die Auflistungsansicht angefügten Datenquelle erstellt und dann angeordnet und durch die angegebene Auflistung Ansichtslayout angezeigt.

Das Ansichtslayout Auflistung erfolgt normalerweise, wenn die Auflistungsansicht erstellt wird. Allerdings können Sie das Ansichtslayout Sammlung zu einem beliebigen Zeitpunkt ändern, und die auf dem Bildschirm Darstellung von Daten für die Auflistungsansicht wird automatisch aktualisiert werden über das neue Layout bereitgestellt.

Das Ansichtslayout Auflistung bietet mehrere Methoden, die verwendet werden können, um den Übergang zwischen zwei unterschiedliche Layouts (standardmäßig keine Animation ausgeführt wird) zu animieren. Darüber hinaus können die Ansichtslayouts für die Sammlung mit Geste Erkennungen weiter Eingreifen des Benutzers zu animieren, die eine Änderung im Layout führt arbeiten.

<a name="Creating-Cells-and-Supplementary-Views" />

## <a name="creating-cells-and-supplementary-views"></a>Erstellen von Zellen und zusätzliche Ansichten

Eine Auflistungsansicht-Datenquelle ist nicht nur verantwortlich für das Bereitstellen der Daten, Sichern der Auflistung der Elemente, sondern auch der Zellen, die verwendet werden, um den Inhalt anzuzeigen.

Da Auflistungsansichten Behandeln von großen Auflistungen von Elementen konzipiert wurden, können die einzelnen Zellen aus der Warteschlange entfernt und erneut verwendet, um zu verhindern, dass das Speicherlimit überschritten werden. Es gibt zwei unterschiedliche Methoden zum Entfernen von Ansichten:

- `DequeueReusableCell` : Erstellt, oder gibt eine Zelle des angegebenen Typs (wie in der app-Storyboard angegeben).
- `DequeueReusableSupplementaryView` : Erstellt ein, oder gibt eine zusätzliche Ansicht des angegebenen Typs (wie in der app-Storyboard angegeben).

Sie müssen vor dem Aufrufen dieser Methoden, die Klasse, registrieren Storyboard oder `.xib` Datei verwendet, um mit dem die Auflistungsansicht der Zelle Ansicht erstellen. Zum Beispiel:

```csharp
public CityCollectionView (IntPtr handle) : base (handle)
{
    // Initialize
    RegisterClassForCell (typeof(CityCollectionViewCell), CityViewDatasource.CardCellId);
    ...
}
```

Wo `typeof(CityCollectionViewCell)` stellt die Klasse, die die Ansicht unterstützt und `CityViewDatasource.CardCellId` stellt die ID verwendet, wenn die Zelle (oder Ansicht), aus der Warteschlange entfernt wird.

Nach die Zelle aus der Warteschlange entfernt wird, konfigurieren es mit den Daten für das Element, den, das es darstellt, ist, und zur Auflistungsansicht für die Anzeige zurückgegeben.

<a name="About-Collection-View-Controllers" />

## <a name="about-collection-view-controllers"></a>Zur Auflistung View-Controller

Eine Auflistung-View-Controller (`UICollectionViewController`) ist eine spezielle View-Controller (`UIViewController`), die Folgendes bereitstellt:

- Er ist verantwortlich für das Laden der Auflistungsansicht aus das Storyboard oder `.xib` Datei- und Instanziierung der Ansicht. Wenn im Code erstellt wird, erstellt es automatisch eine neue, nicht konfigurierte Auflistungsansicht.
- Nach dem Laden der Auflistungsansicht versucht der Controller, laden die Datenquelle und den Delegaten aus dem Storyboard oder `.xib` Datei. Wenn keiner verfügbar ist, wird sich selbst als Quelle für beide.
- Stellt sicher, dass die Daten geladen werden, bevor die Auflistungsansicht, auf der ersten angezeigt gefüllt wird, und lädt erneut und löschen die SELECT-Anweisung für jede nachfolgende Anzeige.

Darüber hinaus bietet der Sammlungsansichtscontroller überschreibbare Methoden, die verwendet werden können, um die Verwaltung des Lebenszyklus der Auflistungsansicht wie z. B. `AwakeFromNib` und `ViewWillDisplay`.

<a name="Collection-Views-and-Storyboards" />

## <a name="collection-views-and-storyboards"></a>Auflistungsansichten und Storyboards

Die einfachste Möglichkeit zum Arbeiten mit einer Auflistungsansicht in Ihrer Xamarin.tvOS-app ist auf das Storyboard hinzu. Als kurzes Beispiel werden wir eine Beispiel-app zu erstellen, die ein Bild, Titel und eine auswählen-Schaltfläche darstellt. Wenn der Benutzer die auswählen-Schaltfläche klicken, wird eine Auflistungsansicht angezeigt werden, mit denen den Benutzer ein neues Image auswählen zu können. Wenn ein Bild ausgewählt ist, wird die Auflistungsansicht wird geschlossen, und das neue Bild und den Titel angezeigt.

Lassen Sie uns wie folgt vor:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

    
1. Starten Sie eine neue **Einzelansicht TvOS-App** in Visual Studio für Mac.
1. In der **Projektmappen-Explorer**, doppelklicken Sie auf die `Main.storyboard` und öffnen Sie sie in der iOS-Designer.
1. Der vorhandenen Ansicht fügen Sie eine Image-Sicht, eine Bezeichnung und eine Schaltfläche hinzu und konfigurieren Sie sie wie folgt aussehen: 

    [![](collection-views-images/collection02.png "Beispiel-layout")](collection-views-images/collection02.png#lightbox)
1. Zuweisen einer **Namen** der Bildansicht und die Bezeichnung in der **Widget-Registerkarte** von der **Eigenschaften-Explorer**. Zum Beispiel: 

    [![](collection-views-images/collection03.png "Festlegen des Namens")](collection-views-images/collection03.png#lightbox)
1. Als Nächstes ziehen Sie einen Ansichtscontroller für die Sammlung, in das Storyboard zu erhalten: 

    [![](collection-views-images/collection04.png "Eine Auflistung-Ansichtscontroller")](collection-views-images/collection04.png#lightbox)
1. Steuerelement ziehen über die Schaltfläche auf den View-Controller-Auflistung, und wählen Sie **Push** im Popupmenü: 

    [![](collection-views-images/collection05.png "Wählen Sie im Popupmenü Push")](collection-views-images/collection05.png#lightbox)
1. Wenn die app ausgeführt wird, wird diese stellen die Auflistungsansicht angezeigt werden, wenn der Benutzer die Schaltfläche klickt.
1. Wählen Sie die Ansicht der Auflistung, und geben Sie die folgenden Werte in der **Registerkarte "Layout"** von der **Eigenschaften-Explorer**: 

    [![](collection-views-images/collection06.png "Die Eigenschaften-Explorer")](collection-views-images/collection06.png#lightbox)
1. Steuert die Größe der einzelnen Zellen sowie die Rahmen zwischen den Zellen und die äußeren Rand der Auflistungsansicht.
1. Wählen Sie den Ansichtscontroller für die Sammlung aus, und legen Sie die Klasse auf `CityCollectionViewController` in die **Widget Registerkarte**: 

    [![](collection-views-images/collection07.png "Legen Sie die Klasse auf CityCollectionViewController")](collection-views-images/collection07.png#lightbox)
1. Wählen Sie die Auflistungsansicht und legen Sie die Klasse auf `CityCollectionView` in die **Widget Registerkarte**: 

    [![](collection-views-images/collection08.png "Legen Sie die Klasse auf CityCollectionView")](collection-views-images/collection08.png#lightbox)
1. Wählen Sie die Sammlungsansichtszelle und legen Sie die Klasse auf `CityCollectionViewCell` in die **Widget Registerkarte**: 

    [![](collection-views-images/collection09.png "Legen Sie die Klasse auf CityCollectionViewCell")](collection-views-images/collection09.png#lightbox)
1. In der **Widget-Registerkarte** sicher, dass die **Layout** ist `Flow` und **Richtung der Bildlauf** ist `Vertical` für die Sammlung an: 

    [![](collection-views-images/collection10.png "Die Registerkarte \"Widget\"")](collection-views-images/collection10.png#lightbox)
1. Wählen Sie die Sammlungsansichtszelle und legen Sie seine **Identität** zu `CityCell` in die **Widget Registerkarte**: 

    [![](collection-views-images/collection11.png "Legen Sie die Identität auf CityCell")](collection-views-images/collection11.png#lightbox)
1. Speichern Sie die Änderungen.
    

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

    
1. Starten Sie eine neue **Einzelansicht TvOS-App** in Visual Studio.
1. In der **Projektmappen-Explorer**, doppelklicken Sie auf die `Main.storyboard` und öffnen Sie sie in der iOS-Designer.
1. Der vorhandenen Ansicht fügen Sie eine Image-Sicht, eine Bezeichnung und eine Schaltfläche hinzu und konfigurieren Sie sie wie folgt aussehen: 

    [![](collection-views-images/collection02vs.png "Konfigurieren Sie das layout")](collection-views-images/collection02vs.png#lightbox)
1. Zuweisen einer **Namen** der Bildansicht und die Bezeichnung in der **Widget-Registerkarte** von der **Eigenschaften-Explorer**. Zum Beispiel: 

    [![](collection-views-images/collection03vs.png "Die Eigenschaften-Explorer")](collection-views-images/collection03vs.png#lightbox)
1. Als Nächstes ziehen Sie einen Ansichtscontroller für die Sammlung, in das Storyboard zu erhalten: 

    [![](collection-views-images/collection04vs.png "Eine Auflistung-Ansichtscontroller")](collection-views-images/collection04vs.png#lightbox)
1. Steuerelement ziehen über die Schaltfläche auf den View-Controller-Auflistung, und wählen Sie **Push** im Popupmenü: 

    [![](collection-views-images/collection05vs.png "Wählen Sie im Popupmenü Push")](collection-views-images/collection05vs.png#lightbox)
1. Wenn die app ausgeführt wird, wird diese stellen die Auflistungsansicht angezeigt werden, wenn der Benutzer die Schaltfläche klickt.
1. Wählen Sie die Auflistungsansicht und klicken Sie in der **Registerkarte "Layout"** von der **Eigenschaften-Explorer** Geben Sie die **Breite** als _361_ und  **Höhe** als _256_ 
1. Steuert die Größe der einzelnen Zellen sowie die Rahmen zwischen den Zellen und die äußeren Rand der Auflistungsansicht.
1. Wählen Sie den Ansichtscontroller für die Sammlung aus, und legen Sie die Klasse auf `CityCollectionViewController` in die **Widget Registerkarte**: 

    [![](collection-views-images/collection07vs.png "Legen Sie die Klasse auf CityCollectionViewController")](collection-views-images/collection07vs.png#lightbox)
1. Wählen Sie die Auflistungsansicht und legen Sie die Klasse auf `CityCollectionView` in die **Widget Registerkarte**: 

    [![](collection-views-images/collection08vs.png "Legen Sie die Klasse auf CityCollectionView")](collection-views-images/collection08vs.png#lightbox)
1. Wählen Sie die Sammlungsansichtszelle und legen Sie die Klasse auf `CityCollectionViewCell` in die **Widget Registerkarte**: 

    [![](collection-views-images/collection09vs.png "Legen Sie die Klasse auf CityCollectionViewCell")](collection-views-images/collection09vs.png#lightbox)
1. In der **Widget-Registerkarte** sicher, dass die **Layout** ist `Flow` und **Richtung der Bildlauf** ist `Vertical` für die Sammlung an: 

    [![](collection-views-images/collection10vs.png "Registerkarte \"%tdas-Widget\"")](collection-views-images/collection10vs.png#lightbox)
1. Wählen Sie die Sammlungsansichtszelle und legen Sie seine **Identität** zu `CityCell` in die **Widget Registerkarte**: 

    [![](collection-views-images/collection11vs.png "Legen Sie die Identität auf CityCell")](collection-views-images/collection11vs.png#lightbox)
1. Speichern Sie die Änderungen.
    

-----

Wenn wir uns entschieden hatte `Custom` für die Auflistungsansicht **Layout**, es wäre möglich gewesen, ein benutzerdefiniertes Layout. Apple bietet eine integrierte `UICollectionViewFlowLayout` und `UICollectionViewDelegateFlowLayout` , die können problemlos präsentieren von Daten in einem Layout mit Raster-basierte (diese werden verwendet, durch die `flow` Layoutstil). 

Weitere Informationen zum Arbeiten mit Storyboards, informieren Sie sich unsere [Hello, TvOS – Kurzanleitung](~/ios/tvos/get-started/hello-tvos.md).

<a name="Providing-Data-for-the-Collection-View" />

## <a name="providing-data-for-the-collection-view"></a>Bereitstellen von Daten für die Auflistung an

Nun, da wir unsere Auflistungsansicht (und Sammlungsansichtscontroller) unserer Storyboard hinzugefügt haben, müssen wir die Daten für die Sammlung angeben. 

<a name="The-Data-Model" />

### <a name="the-data-model"></a>Das Datenmodell

Zunächst werden wir ein Modell für die Daten zu erstellen, enthält den Dateinamen für die anzuzeigende Bild, Titel und einem Flag für das Zulassen der Stadt ausgewählt werden.

Erstellen Sie eine `CityInfo` Klasse, und stellen sie wie folgt aussehen:

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

### <a name="the-collection-view-cell"></a>Die Sammlungsansichtszelle

Jetzt müssen wir definieren, wie die Daten für jede Zelle dargestellt werden. Bearbeiten der `CityCollectionViewCell.cs` (erstellte Datei für Sie automatisch aus der Storyboard-Datei), und stellen sie wie folgt aussehen:

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

Für unsere TvOS-app werden wir ein Abbild und einen optionalen Titel angezeigt werden. Wenn die angegebene Stadt nicht ausgewählt werden kann, werden wir die Image-Sicht, die mithilfe des folgenden Codes abblendet:

```csharp
CityView.Alpha = (City.CanSelect) ? 1.0f : 0.5f;
```

Wenn die Zelle mit dem Image im Fokus vom Benutzer aktiviert wird, verwendet werden soll die integrierte Parallaxeneffekt auf die folgende Eigenschaft festlegen:

```csharp
CityView.AdjustsImageWhenAncestorFocused = true;
```

Weitere Informationen zu Navigation und Fokus, finden Sie unserem [arbeiten mit Navigation und Fokus](~/ios/tvos/app-fundamentals/navigation-focus.md) und [Siri Remote- und Bluetooth-Controller](~/ios/tvos/platform/remote-bluetooth.md) Dokumentation.


<a name="The-Collection-View-Data-Provider" />

### <a name="the-collection-view-data-provider"></a>Der Datenanbieter der Sammlung anzeigen

Erstellen Sie mit unserem Datenmodell erstellt und unsere Zellenlayout definiert eine Datenquelle wir für unsere Auflistungsansicht. Die Datenquelle werden dafür verantwortlich, nicht nur die dahinterliegenden Daten, sondern auch dabei die Zellen für die einzelnen Zellen, die auf dem Bildschirm angezeigt.

Erstellen Sie eine `CityViewDatasource` Klasse, und stellen sie wie folgt aussehen:

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

Lassen Sie diese Klasse im Detail ansehen. Zunächst wir erben `UICollectionViewDataSource` , und geben Sie eine Verknüpfung auf die Zellen-ID (die wir im iOS-Designer zugewiesen):

```csharp
public static NSString CardCellId = new NSString ("CityCell");
```

Als Nächstes Bereitstellen von Speicher für unsere Daten und geben Sie eine Klasse, um die Daten zu füllen:

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

Wir überschreiben die `NumberOfSections` Methode, und geben die Anzahl von Abschnitten (Gruppen von Elementen), die die Sammlung anzeigen. In diesem Fall besteht nur ein:

```csharp
public override nint NumberOfSections (UICollectionView collectionView)
{
    return 1;
}
```

Als Nächstes geben wir die Anzahl der Elemente in der Sammlung, die mithilfe des folgenden Codes zurück:

```csharp
public override nint GetItemsCount (UICollectionView collectionView, nint section)
{
    return Cities.Count;
}
```

Abschließend entfernen wir eine wieder verwendbare Zelle, wenn die Auflistungsansicht anfordern, mit dem folgenden Code:

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

Nachdem wir einer Sammlungsansichtszelle von erhalten unsere `CityCollectionViewCell` Typ wir füllen Sie es mit dem angegebenen Element.

<a name="Responding-to-User-Events" />

## <a name="responding-to-user-events"></a>Reaktion auf Benutzerereignisse

Da soll den Benutzer ein Element aus die Sammlung auswählen können, müssen wir eine Sammlung anzeigen-Delegat, der diese Interaktion bereitstellen. Und wir müssen angeben, dass eine Möglichkeit, können unsere aufrufende Ansicht, die die Benutzer wissen, welches Element ausgewählt wurde.

<a name="The-App-Delegate" />

### <a name="the-app-delegate"></a>Der App-Delegat

Wir benötigen eine Möglichkeit, das aktuell ausgewählte Element aus der Auflistungsansicht zurück an die aufrufende Sicht zu verbinden. Wir verwenden eine benutzerdefinierte Eigenschaft auf unsere `AppDelegate`. Bearbeiten der `AppDelegate.cs` Datei, und fügen Sie den folgenden Code hinzu:

```csharp
public CityInfo SelectedCity { get; set;} = new CityInfo("City02.jpg", "Turning Circle", true);
```

Dies definiert die Eigenschaft, und legt die Standard-Stadt, die anfänglich angezeigt wird. Später werden wir nutzen, diese Eigenschaft zum Anzeigen von der Auswahl des Benutzers ein, und wählen Sie aus, die geändert werden können.

<a name="The-Collection-View-Delegate" />

### <a name="the-collection-view-delegate"></a>Der Delegat der Auflistung anzeigen

Als Nächstes fügen Sie einen neuen `CityViewDelegate` Klasse dem Projekt, und stellen sie wie folgt aussehen:


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

Werfen Sie einen genaueren Blick auf diese Klasse an. Zunächst wir erben `UICollectionViewDelegateFlowLayout`. Der Grund, die wir von dieser Klasse erben und nicht die `UICollectionViewDelegate` ist die Verwendung der integriertes `UICollectionViewFlowLayout` , unsere Elemente und nicht der Typ ein benutzerdefiniertes Layout zu präsentieren.

Kehren wir die Größe für die einzelnen Elemente, die mit dem folgenden Code:

```csharp
public override CGSize GetSizeForItem (UICollectionView collectionView, UICollectionViewLayout layout, NSIndexPath indexPath)
{
    return new CGSize (361, 256);
}
```

Klicken Sie dann entscheiden wir, wenn eine bestimmte Zelle den Fokus mithilfe des folgenden Codes abrufen kann: 

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

Wir überprüfen, um festzustellen, ob eine bestimmte Sicherung Daten hat seine `CanSelect` Flag festgelegt, um `true` und gibt diesen Wert zurück. Weitere Informationen zu Navigation und Fokus, finden Sie unserem [arbeiten mit Navigation und Fokus](~/ios/tvos/app-fundamentals/navigation-focus.md) und [Siri Remote- und Bluetooth-Controller](~/ios/tvos/platform/remote-bluetooth.md) Dokumentation.

Zum Schluss reagieren wir auf den Benutzer, die Auswahl eines Elements mit den folgenden Code:

```csharp
public override void ItemSelected (UICollectionView collectionView, NSIndexPath indexPath)
{
    var controller = collectionView as CityCollectionView;
    App.SelectedCity = controller.Source.Cities [indexPath.Row];

    // Close Collection
    controller.ParentController.DismissViewController(true,null);
}
```

Hier legen wir die `SelectedCity` Eigenschaft unserer `AppDelegate` auf das Element, dass vom Benutzer ausgewählten wir schließen und den Ansichtscontroller-Auflistung, an die Ansicht, die uns aufgerufen. Wir noch nicht definiert die `ParentController` Eigenschaft noch unsere Auflistungsansicht, machen wir also weiter.

<a name="Configuring-the-Collection-View" />

## <a name="configuring-the-collection-view"></a>Konfigurieren die Auflistungsansicht

Nun müssen wir unsere Auflistungsansicht bearbeiten, und weisen Sie unsere Datenquelle und den Delegaten. Bearbeiten der `CityCollectionView.cs` Datei (für uns erstellt automatisch aus unserer Storyboard), und stellen sie wie folgt aussehen:

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

Zunächst bieten wir eine Verknüpfung zum Zugriff auf unsere `AppDelegate`: 

```csharp
public static AppDelegate App {
    get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
}
```

Anschließend geben wir eine Verknüpfung, die Auflistungsansicht Datenquelle sowie eine Eigenschaft, auf der Sammlung-View-Controller (durch unsere Delegaten, der oben genannten um der Auflistung zu schließen, wenn der Benutzer eine Auswahl trifft verwendet):

```csharp
public CityViewDatasource Source {
    get { return DataSource as CityViewDatasource;}
}

public CityCollectionViewController ParentController { get; set;}
```

Anschließend verwenden wir den folgenden Code die Auflistungsansicht zu initialisieren und weisen Sie den Cell-Klasse, die Datenquelle und den Delegaten:

```csharp
public CityCollectionView (IntPtr handle) : base (handle)
{
    // Initialize
    RegisterClassForCell (typeof(CityCollectionViewCell), CityViewDatasource.CardCellId);
    DataSource = new CityViewDatasource (this);
    Delegate = new CityViewDelegate ();
}
```

Schließlich möchten wir den Titel aus, unter dem Bild nur angezeigt werden, wenn der Benutzer die Markierung hat (bildschärfenmodus). Hierzu verwenden Sie den folgenden Code:

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

Legen wir die Transparence des vorherigen Elements, das den Fokus, wenn Sie NULL (0), und die Transparence des nächsten Elements erhalten, den Fokus auf 100 %. Dieser Übergang zu erhalten sowie animiert.


## <a name="configuring-the-collection-view-controller"></a>Konfigurieren des Sammlung-View-Controllers

Nun müssen wir die endgültige Konfiguration auf unserem Auflistungsansicht und dass der Controller zum Festlegen der Eigenschaft, die wir definiert werden, damit die Auflistungsansicht geschlossen werden kann, nachdem der Benutzer eine Auswahl trifft.

Bearbeiten der `CityCollectionViewController.cs` (automatisch aus unserer Storyboard erstellte) Datei, und stellen sie wie folgt aussehen:

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

Nun, da wir alle haben Teile zusammengestellt, die zum Auffüllen und unsere Auflistungsansicht steuern muss, die letzten Änderungen zu unserer wichtigsten, um alles vereinen zu können.

Bearbeiten der `ViewController.cs` (automatisch aus unserer Storyboard erstellte) Datei, und stellen sie wie folgt aussehen:

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

Der folgende Code zeigt anfänglich das ausgewählte Element aus der `SelectedCity` Eigenschaft der `AppDelegate` und zeigt es, wenn der Benutzer eine Auswahl aus der Auflistung an vorgenommen hat:

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

## <a name="testing-the-app"></a>Testen der App

Wenn Sie beim Erstellen und führen Sie die app wird die Hauptansicht nach Abschluss der Vorbereitung mit dem standardmäßigen Ort angezeigt:

[![](collection-views-images/run01.png "Hauptbildschirm")](collection-views-images/run01.png#lightbox)

Wenn der Benutzer auf die **wählen Sie eine Ansicht** Schaltfläche, die Auflistungsansicht wird angezeigt:

[![](collection-views-images/run02.png "Die Auflistungsansicht")](collection-views-images/run02.png#lightbox)

Jede Stadt, die die `CanSelect` -Eigenschaft auf festgelegt `false` erscheint abgeblendet ist und der Benutzer wird nicht in der Lage, den Fokus festgelegt. Wenn der Benutzer ein Element hervorgehoben (Stellen sie im Fokus) der Titel angezeigt wird, und sie können den Parallaxeneffekt, Besonderheit Neigung das Bild in 3D.

Klickt der Benutzer eine wählen Sie das Abbild, die Auflistungsansicht wird geschlossen, und die Hauptansicht wird mit dem neuen Image erneut angezeigt:

[![](collection-views-images/run03.png "Ein neues Image auf dem Startbildschirm")](collection-views-images/run03.png#lightbox)

<a name="Creating-Custom-Layout-and-Reordering-Items" />

## <a name="creating-custom-layout-and-reordering-items"></a>Erstellen von benutzerdefinierten Layouts und neuanordnung von Elementen

Einer der wichtigsten Features der Verwendung einer Auflistungsansicht ist die Möglichkeit, benutzerdefinierte Layouts zu erstellen. Da TvOS iOS erbt, ist der Prozess zum Erstellen eines benutzerdefinierten Layouts identisch. Informieren Sie sich unsere [Einführung in die Auflistungsansichten](~/ios/user-interface/controls/uicollectionview.md) finden Sie Dokumentation.

Vor kurzem für iOS zu Auflistungsansichten hinzugefügt wurde 9 die Möglichkeit, ermöglichen die neuanordnung von Elementen in der Auflistung. In diesem Fall da TvOS 9 eine Teilmenge von iOS 9 ist, dies erfolgt diese genauso. Informieren Sie sich unsere [Auflistung anzeigen von Änderungen](~/ios/user-interface/controls/uicollectionview.md) Dokument Weitere Informationen.


<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden behandelt, Entwerfen und Arbeiten mit Auflistungsansichten in ein Xamarin.tvOS-app. Zuerst erläutert alle Elemente, aus denen die Auflistungsansicht besteht. Als Nächstes wird aufgezeigt, wie Sie entwerfen und implementieren eine Auflistungsansicht mit einem Storyboard. Schließlich wird Links zu Informationen bereitgestellt, auf das Erstellen von benutzerdefinierten Layouts und neuanordnung von Elementen.



## <a name="related-links"></a>Verwandte Links

- [tvOS-Beispiele](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [TvOS Human Interface-Handbücher](https://developer.apple.com/tvos/human-interface-guidelines/)
- [App-Programmierhandbuch für tvos verwendet.](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
