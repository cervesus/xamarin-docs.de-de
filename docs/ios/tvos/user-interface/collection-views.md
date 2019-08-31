---
title: Arbeiten mit tvos-Auflistungs Ansichten in xamarin
description: In diesem Dokument wird beschrieben, wie Sie in einer mit xamarin erstellten tvos-App mit Sammlungs Ansichten arbeiten. Es behandelt Auflistungs Ansichts Layouts, das Erstellen von Zellen und ergänzenden Sichten, das reagieren auf Benutzer Ereignisse und vieles mehr.
ms.prod: xamarin
ms.assetid: 5125C4C7-2DDF-4C19-A362-17BB2B079178
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: 616b20872a01b4df6c3f27c636ce7b8ee912414e
ms.sourcegitcommit: 1e3a0d853669dcc57d5dee0894d325d40c7d8009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2019
ms.locfileid: "70200342"
---
# <a name="working-with-tvos-collection-views-in-xamarin"></a>Arbeiten mit tvos-Auflistungs Ansichten in xamarin

Mithilfe von Sammlungs Ansichten kann eine Gruppe von Inhalten mit beliebigen Layouts angezeigt werden. Mithilfe integrierter Unterstützung ermöglichen Sie die einfache Erstellung von Raster ähnlichen oder linearen Layouts, während gleichzeitig auch benutzerdefinierte Layouts unterstützt werden.

[![](collection-views-images/collection01.png "Beispiel Sammlungsansicht")](collection-views-images/collection01.png#lightbox)

Die Auflistungs Ansicht verwaltet eine Auflistung von Elementen, die sowohl einen Delegaten als auch eine Datenquelle verwenden, um die Benutzerinteraktion und den Inhalt der Auflistung bereitzustellen. Da die Auflistungs Ansicht auf einem layoutsubsystem basiert, das unabhängig von der Sicht ist, kann die Bereitstellung eines anderen Layouts die Darstellung der Daten der Sammlungsansicht im Handumdrehen ändern.

<a name="About-Collection-Views" />

## <a name="about-collection-views"></a>Informationen zu Sammlungs Ansichten

Wie bereits erwähnt, verwaltet eine Auflistungs Ansicht (`UICollectionView`) eine geordnete Auflistung von Elementen und zeigt diese Elemente mit anpassbaren Layouts an. Auflistungs Ansichten funktionieren ähnlich wie Tabellen Sichten (`UITableView`), mit dem Unterschied, dass Sie Layouts verwenden können, um Elemente in mehr als nur einer einzelnen Spalte darzustellen.

Wenn Sie in tvos eine Auflistungs Ansicht verwenden, ist Ihre APP für die Bereitstellung der Daten, die der Sammlung zugeordnet`UICollectionViewDataSource`sind, mit einer Datenquelle () verantwortlich. Sammlungs Ansichts Daten können optional organisiert und in verschiedenen Gruppen (Abschnitten) dargestellt werden.

Die Auflistungs Ansicht zeigt die einzelnen Elemente auf dem Bildschirm`UICollectionViewCell`mithilfe einer Zelle () an, die die Darstellung eines bestimmten Informations Elements aus der Auflistung bereitstellt (z. b. ein Bild und seinen Titel).

Optional können zusätzliche Sichten zur Darstellung der Sammlungsansicht hinzugefügt werden, um Sie als Kopf-und Fußzeilen für die Abschnitte und Zellen zu fungieren. Das Layout der Sammlungsansicht ist dafür verantwortlich, die Platzierung dieser Ansichten zusammen mit den einzelnen Zellen zu definieren.

Die Auflistungs Ansicht kann mithilfe eines Delegaten (`UICollectionViewDelegate`) auf die Benutzerinteraktion reagieren. Dieser Delegat ist auch dafür verantwortlich, zu bestimmen, ob eine bestimmte Zelle den Fokus erhalten kann, wenn eine Zelle hervorgehoben oder ausgewählt wurde. In einigen Fällen bestimmt der Delegat die Größe der einzelnen Zellen.

<a name="Collection-View-Layouts" />

## <a name="collection-view-layouts"></a>Layouts der Sammlungsansicht

Eine zentrale Funktion einer Auflistungs Ansicht ist die Trennung zwischen den Daten, die Sie darstellen, und dem Layout. Ein Auflistungs Ansichts Layout (`UICollectionViewLayout`) ist dafür verantwortlich, die Organisation und den Speicherort der Zellen (und aller ergänzenden Sichten) in der Bildschirmpräsentation der Sammlungsansicht bereitzustellen.

Die einzelnen Zellen werden von der Auflistungs Ansicht aus der angefügten Datenquelle erstellt und dann durch das angegebene Layout der Sammlungsansicht angeordnet und angezeigt.

Das Layout der Sammlungsansicht wird normalerweise bereitgestellt, wenn die Auflistungs Ansicht erstellt wird. Allerdings können Sie das Layout der Sammlungsansicht jederzeit ändern, und die Bildschirmpräsentation der Daten der Sammlungsansicht wird automatisch mithilfe des neuen Layouts aktualisiert.

Das Auflistungs Ansichts Layout stellt mehrere Methoden bereit, die verwendet werden können, um den Übergang zwischen zwei verschiedenen Layouts zu animieren (Standardmäßig ist keine Animation abgeschlossen). Außerdem können Auflistungs Ansichts Layouts mit Gesten Erkennungs Programmen verwendet werden, um die Benutzerinteraktion weiter zu animieren, die zu einer Änderung des Layouts führt.

<a name="Creating-Cells-and-Supplementary-Views" />

## <a name="creating-cells-and-supplementary-views"></a>Erstellen von Zellen und ergänzenden Sichten

Die Datenquelle einer Auflistungs Ansicht ist nicht nur für die Bereitstellung der Daten zuständig, die das Element der Sammlung unterstützen, sondern auch für die Zellen, die zum Anzeigen des Inhalts verwendet werden.

Da Auflistungs Sichten für die Verarbeitung großer Auflistungen von Elementen entworfen wurden, können die einzelnen Zellen aus der Warteschlange entfernt und wieder verwendet werden, um die übermäßige Arbeitsspeicher Beschränkung beizubehalten. Es gibt zwei verschiedene Methoden zum Entfernen von Sichten aus der Warteschlange:

- `DequeueReusableCell`-Erstellt oder gibt eine Zelle des angegebenen Typs zurück (wie im Storyboard der APP angegeben).
- `DequeueReusableSupplementaryView`-Erstellt oder gibt eine ergänzende Ansicht des angegebenen Typs zurück (wie im Storyboard der APP angegeben).

Bevor Sie eine dieser Methoden aufrufen, müssen Sie die Klasse, das Storyboard oder `.xib` die Datei registrieren, die zum Erstellen der Zellen Ansicht in der Auflistungs Ansicht verwendet wird. Beispiel:

```csharp
public CityCollectionView (IntPtr handle) : base (handle)
{
    // Initialize
    RegisterClassForCell (typeof(CityCollectionViewCell), CityViewDatasource.CardCellId);
    ...
}
```

Dabei stellt die Klasse bereit, die die Ansicht `CityViewDatasource.CardCellId` unterstützt und die ID bereitstellt, die verwendet wird, wenn die Zelle (oder Ansicht) aus der Warteschlange entfernt wird `typeof(CityCollectionViewCell)`

Nachdem die Zelle aus der Warteschlange entfernt wurde, konfigurieren Sie Sie mit den Daten für das Element, das Sie darstellt, und kehren zur Anzeige zur Auflistungs Ansicht zurück.

<a name="About-Collection-View-Controllers" />

## <a name="about-collection-view-controllers"></a>Informationen zu Sammlungs Ansichts Controllern

Ein Sammlungs Ansichts Controller`UICollectionViewController`() ist ein spezieller Ansichts`UIViewController`Controller (), der folgendes Verhalten bereitstellt:

- Er ist dafür verantwortlich, die Sammlungsansicht aus dem Storyboard oder `.xib` der Datei zu laden und die Ansicht zu instanziieren. Wenn Sie im Code erstellt wird, wird automatisch eine neue, nicht konfigurierte Auflistungs Ansicht erstellt.
- Nachdem die Sammlungsansicht geladen wurde, versucht der Controller, seine Datenquelle und den Delegaten aus dem Storyboard `.xib` oder der Datei zu laden. Wenn keine verfügbar sind, legt Sie sich selbst als Quelle für beide fest.
- Stellt sicher, dass die Daten geladen werden, bevor die Auflistungs Ansicht zuerst angezeigt wird, und lädt die Auswahl für jede nachfolgende Anzeige erneut.

Darüber hinaus stellt der Sammlungs Ansichts Controller über schreibbare Methoden bereit, die zum Verwalten des Lebenszyklus der Auflistungs `AwakeFromNib` Ansicht `ViewWillDisplay`verwendet werden können, z. b. und.

<a name="Collection-Views-and-Storyboards" />

## <a name="collection-views-and-storyboards"></a>Sammlungs Ansichten und Storyboards

Die einfachste Möglichkeit, mit einer Sammlungsansicht in ihrer xamarin. tvos-APP zu arbeiten, besteht darin, eine dem Storyboard hinzuzufügen. Als kurzes Beispiel erstellen wir eine Beispiel-APP, die ein Bild, einen Titel und eine Auswahl Schaltfläche darstellt. Wenn der Benutzer auf die Schaltfläche auswählen klickt, wird eine Sammlungsansicht angezeigt, in der der Benutzer ein neues Bild auswählen kann. Wenn ein Bild ausgewählt wird, wird die Sammlungsansicht geschlossen, und das neue Bild und der Titel werden angezeigt.

Führen Sie die folgenden Schritte aus:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

    
1. Starten Sie eine neue **tvos-App für die Einzelansicht** in Visual Studio für Mac.
1. Doppelklicken Sie im **Projektmappen-Explorer**auf die `Main.storyboard` Datei, und öffnen Sie Sie im IOS-Designer.
1. Fügen Sie der vorhandenen Ansicht eine Bildansicht, eine Bezeichnung und eine Schaltfläche hinzu, und konfigurieren Sie Sie so, dass Sie wie folgt aussieht: 

    [![](collection-views-images/collection02.png "Beispiel Layout")](collection-views-images/collection02.png#lightbox)
1. Weisen Sie der Bildansicht und der Bezeichnung im **Eigenschaften-Explorer**auf der **Registerkarte widget** einen **Namen** zu. Beispiel: 

    [![](collection-views-images/collection03.png "Festlegen des Namens")](collection-views-images/collection03.png#lightbox)
1. Ziehen Sie als nächstes einen Sammlungs Ansichts Controller auf das Storyboard: 

    [![](collection-views-images/collection04.png "Einen Sammlungs Ansichts Controller")](collection-views-images/collection04.png#lightbox)
1. Steuerelement: ziehen Sie von der Schaltfläche auf den Sammlungs Ansichts Controller, und wählen Sie im Popup Fenster **Push** aus: 

    [![](collection-views-images/collection05.png "Auswählen von Push aus dem Popup")](collection-views-images/collection05.png#lightbox)
1. Wenn die app ausgeführt wird, wird die Sammlungsansicht immer dann angezeigt, wenn der Benutzer auf die Schaltfläche klickt.
1. Wählen Sie die Sammlungsansicht aus, und geben Sie im **Eigenschaften-Explorer**auf der **Registerkarte Layout** die folgenden Werte ein: 

    [![](collection-views-images/collection06.png "Der Eigenschaften-Explorer")](collection-views-images/collection06.png#lightbox)
1. Dadurch wird die Größe der einzelnen Zellen und der Rahmen zwischen den Zellen und dem äußeren Rand der Auflistungs Ansicht gesteuert.
1. Wählen Sie den Sammlungs Ansichts Controller aus, und `CityCollectionViewController` legen Sie seine Klasse auf der **Registerkarte widget**auf fest: 

    [![](collection-views-images/collection07.png "Legen Sie die Klasse auf citycollectionviewcontroller fest.")](collection-views-images/collection07.png#lightbox)
1. Wählen Sie die Sammlungsansicht aus, und legen `CityCollectionView` Sie Ihre Klasse auf der **Registerkarte widget**auf fest: 

    [![](collection-views-images/collection08.png "Legen Sie die Klasse auf citycollectionview fest.")](collection-views-images/collection08.png#lightbox)
1. Wählen Sie die Zelle der Sammlungsansicht aus, und `CityCollectionViewCell` legen Sie Ihre Klasse auf der **Registerkarte widget**auf fest: 

    [![](collection-views-images/collection09.png "Legen Sie die Klasse auf citycollectionviewcell fest.")](collection-views-images/collection09.png#lightbox)
1. Stellen Sie auf der **Registerkarte widget** sicher, `Flow` dass das **Layout** lautet und `Vertical` die **Scrollrichtung** für die Sammlungsansicht gilt: 

    [![](collection-views-images/collection10.png "Die Widget-Registerkarte")](collection-views-images/collection10.png#lightbox)
1. Wählen Sie die Zelle der Sammlungsansicht aus , und `CityCell` legen Sie Ihre Identität auf der **Registerkarte widget**auf fest: 

    [![](collection-views-images/collection11.png "Legen Sie die Identität auf citycell fest.")](collection-views-images/collection11.png#lightbox)
1. Speichern Sie die Änderungen.
    

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

    
1. Starten Sie eine neue **tvos-App für die Einzelansicht** in Visual Studio.
1. Doppelklicken Sie im **Projektmappen-Explorer**auf die `Main.storyboard` Datei, und öffnen Sie Sie im IOS-Designer.
1. Fügen Sie der vorhandenen Ansicht eine Bildansicht, eine Bezeichnung und eine Schaltfläche hinzu, und konfigurieren Sie Sie so, dass Sie wie folgt aussieht: 

    [![](collection-views-images/collection02vs.png "Layout konfigurieren")](collection-views-images/collection02vs.png#lightbox)
1. Weisen Sie der Bildansicht und der Bezeichnung im **Eigenschaften-Explorer**auf der **Registerkarte widget** einen **Namen** zu. Beispiel: 

    [![](collection-views-images/collection03vs.png "Der Eigenschaften-Explorer")](collection-views-images/collection03vs.png#lightbox)
1. Ziehen Sie als nächstes einen Sammlungs Ansichts Controller auf das Storyboard: 

    [![](collection-views-images/collection04vs.png "Einen Sammlungs Ansichts Controller")](collection-views-images/collection04vs.png#lightbox)
1. Steuerelement: ziehen Sie von der Schaltfläche auf den Sammlungs Ansichts Controller, und wählen Sie im Popup Fenster **Push** aus: 

    [![](collection-views-images/collection05vs.png "Auswählen von Push aus dem Popup")](collection-views-images/collection05vs.png#lightbox)
1. Wenn die app ausgeführt wird, wird die Sammlungsansicht immer dann angezeigt, wenn der Benutzer auf die Schaltfläche klickt.
1. Wählen Sie die Sammlungsansicht aus, und geben Sie im **Eigenschaften-Explorer** auf der **Registerkarte Layout** die **Breite** als _361_ und **height** als _256_ ein. 
1. Dadurch wird die Größe der einzelnen Zellen und der Rahmen zwischen den Zellen und dem äußeren Rand der Auflistungs Ansicht gesteuert.
1. Wählen Sie den Sammlungs Ansichts Controller aus, und `CityCollectionViewController` legen Sie seine Klasse auf der **Registerkarte widget**auf fest: 

    [![](collection-views-images/collection07vs.png "Legen Sie die Klasse auf citycollectionviewcontroller fest.")](collection-views-images/collection07vs.png#lightbox)
1. Wählen Sie die Sammlungsansicht aus, und legen `CityCollectionView` Sie Ihre Klasse auf der **Registerkarte widget**auf fest: 

    [![](collection-views-images/collection08vs.png "Legen Sie die Klasse auf citycollectionview fest.")](collection-views-images/collection08vs.png#lightbox)
1. Wählen Sie die Zelle der Sammlungsansicht aus, und `CityCollectionViewCell` legen Sie Ihre Klasse auf der **Registerkarte widget**auf fest: 

    [![](collection-views-images/collection09vs.png "Legen Sie die Klasse auf citycollectionviewcell fest.")](collection-views-images/collection09vs.png#lightbox)
1. Stellen Sie auf der **Registerkarte widget** sicher, `Flow` dass das **Layout** lautet und `Vertical` die **Scrollrichtung** für die Sammlungsansicht gilt: 

    [![](collection-views-images/collection10vs.png "Registerkarte \"Widget\"")](collection-views-images/collection10vs.png#lightbox)
1. Wählen Sie die Zelle der Sammlungsansicht aus , und `CityCell` legen Sie Ihre Identität auf der **Registerkarte widget**auf fest: 

    [![](collection-views-images/collection11vs.png "Legen Sie die Identität auf citycell fest.")](collection-views-images/collection11vs.png#lightbox)
1. Speichern Sie die Änderungen.
    

-----

Wenn wir das `Custom` **Layout**der Sammlungsansicht ausgewählt hätten, hätten wir möglicherweise ein benutzerdefiniertes Layout angegeben. Apple bietet eine integrierte `UICollectionViewFlowLayout` und `UICollectionViewDelegateFlowLayout` , die Daten problemlos in einem Raster basierten Layout darstellen können ( `flow` diese werden vom Layoutstil verwendet). 

Weitere Informationen zum Arbeiten mit Storyboards finden Sie in unserer [Hello-, tvos-Schnellstarthandbuch](~/ios/tvos/get-started/hello-tvos.md).

<a name="Providing-Data-for-the-Collection-View" />

## <a name="providing-data-for-the-collection-view"></a>Bereitstellen von Daten für die Sammlungsansicht

Nachdem wir unsere Sammlungsansicht (und den Sammlungs Ansichts Controller) dem Storyboard hinzugefügt haben, müssen wir die Daten für die Sammlung bereitstellen. 

<a name="The-Data-Model" />

### <a name="the-data-model"></a>Das Datenmodell

Zuerst erstellen wir ein Modell für unsere Daten, das den Dateinamen für das anzuzeigende Bild enthält, den Titel und ein Flag, mit dem die Stadt ausgewählt werden kann.

Erstellen Sie `CityInfo` eine Klasse, und machen Sie Sie wie folgt:

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

### <a name="the-collection-view-cell"></a>Die Sammlungs Ansichts Zelle

Nun müssen wir definieren, wie die Daten für die einzelnen Zellen angezeigt werden. Bearbeiten Sie `CityCollectionViewCell.cs` die Datei (die für Sie automatisch erstellt wurde, aus der storyboarddatei), und führen Sie Sie wie folgt aus:

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

Für unsere tvos-App werden wir ein Bild und einen optionalen Titel anzeigen. Wenn die angegebene Stadt nicht ausgewählt werden kann, wird die Bildansicht mit dem folgenden Code ausgeblendet:

```csharp
CityView.Alpha = (City.CanSelect) ? 1.0f : 0.5f;
```

Wenn die Zelle, in der das Bild enthalten ist, vom Benutzer in den Fokus versetzt wird, möchten wir den integrierten, auf Sie basierende Effekt anwenden, indem wir die folgende Eigenschaft festlegen:

```csharp
CityView.AdjustsImageWhenAncestorFocused = true;
```

Weitere Informationen zur Navigation und zum Fokus finden Sie in der Dokumentation [Arbeiten mit Navigation und Fokus](~/ios/tvos/app-fundamentals/navigation-focus.md) und [Siri-Remote-und Bluetooth-Controller](~/ios/tvos/platform/remote-bluetooth.md) .


<a name="The-Collection-View-Data-Provider" />

### <a name="the-collection-view-data-provider"></a>Die Auflistungs Ansicht Datenanbieter

Nachdem das Datenmodell erstellt und das Zell Layout definiert ist, erstellen wir eine Datenquelle für die Sammlungsansicht. Die Datenquelle ist dafür verantwortlich, nicht nur die Unterstützungs Daten bereitzustellen, sondern auch die Zellen aus der Warteschlange zu entfernen, um die einzelnen Zellen auf dem Bildschirm anzuzeigen.

Erstellen Sie `CityViewDatasource` eine Klasse, und machen Sie Sie wie folgt:

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

Sehen Sie sich diese Klasse ausführlich an. Zuerst erben wir von `UICollectionViewDataSource` und stellen eine Verknüpfung zur Zellen-ID (die wir im IOS-Designer zugewiesen haben) bereit:

```csharp
public static NSString CardCellId = new NSString ("CityCell");
```

Als Nächstes stellen wir Speicher für die Sammlungs Daten bereit und stellen eine Klasse zum Auffüllen der Daten bereit:

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

Anschließend wird die `NumberOfSections` -Methode überschrieben und die Anzahl der Abschnitte (Gruppen von Elementen) zurückgegeben, die in der Sammlungsansicht angezeigt werden. In diesem Fall gibt es nur einen:

```csharp
public override nint NumberOfSections (UICollectionView collectionView)
{
    return 1;
}
```

Im nächsten Schritt wird die Anzahl der Elemente in der Sammlung zurückgegeben, indem der folgende Code verwendet wird:

```csharp
public override nint GetItemsCount (UICollectionView collectionView, nint section)
{
    return Cities.Count;
}
```

Zum Schluss entfernen wir eine wiederverwendbare Zelle aus der Warteschlange, wenn die Auflistungs Ansichts Anforderung mit folgendem Code

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

Nachdem wir eine Sammlungs Ansichts Zelle des `CityCollectionViewCell` Typs erhalten haben, füllen wir Sie mit dem angegebenen Element.

<a name="Responding-to-User-Events" />

## <a name="responding-to-user-events"></a>Reagieren auf Benutzer Ereignisse

Da der Benutzer in der Lage sein soll, ein Element aus der Sammlung auszuwählen, muss ein Auflistungs Ansichts Delegat bereitgestellt werden, um diese Interaktion zu verarbeiten. Und wir müssen eine Methode bereitstellen, mit der die Aufruf Ansicht weiß, welches Element der Benutzer ausgewählt hat.

<a name="The-App-Delegate" />

### <a name="the-app-delegate"></a>Der APP-Delegat

Wir benötigen eine Methode, um das aktuell ausgewählte Element aus der Auflistungs Ansicht zurück zur aufrufenden Ansicht zu verknüpfen. Wir verwenden eine benutzerdefinierte Eigenschaft für unsere `AppDelegate`. Bearbeiten Sie `AppDelegate.cs` die Datei, und fügen Sie den folgenden Code hinzu:

```csharp
public CityInfo SelectedCity { get; set;} = new CityInfo("City02.jpg", "Turning Circle", true);
```

Dadurch wird die-Eigenschaft definiert und die Standard Stadt festgelegt, die anfänglich angezeigt wird. Später wird diese Eigenschaft verwendet, um die Auswahl des Benutzers anzuzeigen, und die Auswahl kann geändert werden.

<a name="The-Collection-View-Delegate" />

### <a name="the-collection-view-delegate"></a>Der Sammlungs Ansichts Delegat

Fügen Sie dem Projekt als `CityViewDelegate` nächstes eine neue Klasse hinzu, und machen Sie Sie wie folgt:


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

Sehen wir uns diese Klasse genauer an. Zuerst erben wir von `UICollectionViewDelegateFlowLayout`. Der Grund, warum wir von dieser Klasse erben und nicht `UICollectionViewDelegate` , besteht darin, dass wir das integrierte `UICollectionViewFlowLayout` zum präsentieren unserer Elemente und keinen benutzerdefinierten Layouttyp verwenden.

Im nächsten Schritt wird die Größe der einzelnen Elemente mit diesem Code zurückgegeben:

```csharp
public override CGSize GetSizeForItem (UICollectionView collectionView, UICollectionViewLayout layout, NSIndexPath indexPath)
{
    return new CGSize (361, 256);
}
```

Anschließend entscheiden wir, ob eine bestimmte Zelle den Fokus mit dem folgenden Code erhalten kann: 

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

Wir überprüfen, ob für ein bestimmtes Unterstützungs Daten- `CanSelect` Flag das Flag auf `true` festgelegt ist, und geben diesen Wert zurück. Weitere Informationen zur Navigation und zum Fokus finden Sie in der Dokumentation [Arbeiten mit Navigation und Fokus](~/ios/tvos/app-fundamentals/navigation-focus.md) und [Siri-Remote-und Bluetooth-Controller](~/ios/tvos/platform/remote-bluetooth.md) .

Zum Schluss Antworten wir darauf, dass der Benutzer ein Element mit folgendem Code ausgewählt hat:

```csharp
public override void ItemSelected (UICollectionView collectionView, NSIndexPath indexPath)
{
    var controller = collectionView as CityCollectionView;
    App.SelectedCity = controller.Source.Cities [indexPath.Row];

    // Close Collection
    controller.ParentController.DismissViewController(true,null);
}
```

Hier legen wir die `SelectedCity` -Eigenschaft `AppDelegate` des-Elements auf das Element fest, das der Benutzer ausgewählt hat, und schließen den Sammlungs Ansichts Controller und kehren zur Ansicht zurück, die uns aufgerufen hat. Wir haben die `ParentController` Eigenschaft unserer Sammlungsansicht noch nicht definiert. Wir werden dies als nächstes tun.

<a name="Configuring-the-Collection-View" />

## <a name="configuring-the-collection-view"></a>Konfigurieren der Sammlungsansicht

Nun müssen wir unsere Sammlungsansicht bearbeiten und die Datenquelle und den Delegaten zuweisen. Bearbeiten Sie `CityCollectionView.cs` die Datei (die für uns automatisch aus dem Storyboard erstellt wurde), und führen Sie Sie wie folgt aus:

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

Zuerst stellen wir eine Verknüpfung für den Zugriff auf `AppDelegate`unsere bereit: 

```csharp
public static AppDelegate App {
    get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
}
```

Als Nächstes stellen wir eine Verknüpfung zur Datenquelle der Sammlungsansicht und eine Eigenschaft für den Zugriff auf den Sammlungs Ansichts Controller bereit (wird von unserem obigen Delegaten verwendet, um die Auflistung zu schließen, wenn der Benutzer eine Auswahl trifft):

```csharp
public CityViewDatasource Source {
    get { return DataSource as CityViewDatasource;}
}

public CityCollectionViewController ParentController { get; set;}
```

Anschließend verwenden wir den folgenden Code, um die Auflistungs Ansicht zu initialisieren und die Zellen Klasse, die Datenquelle und den Delegaten zuzuweisen:

```csharp
public CityCollectionView (IntPtr handle) : base (handle)
{
    // Initialize
    RegisterClassForCell (typeof(CityCollectionViewCell), CityViewDatasource.CardCellId);
    DataSource = new CityViewDatasource (this);
    Delegate = new CityViewDelegate ();
}
```

Schließlich soll der Titel unter dem Bild nur sichtbar sein, wenn der Benutzer ihn hervorgehoben hat (im Fokus). Dies geschieht mit folgendem Code:

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

Wir legen die Transparenz des vorherigen Elements fest, das den Fokus auf NULL (0) verliert, und die Transparenz des nächsten Elements erhält den Fokus auf 100%. Diese Umstellung wird ebenfalls animiert.


## <a name="configuring-the-collection-view-controller"></a>Konfigurieren des Sammlungs Ansichts Controllers

Nun müssen wir die endgültige Konfiguration in der Sammlungsansicht durchführen und dem Controller die Festlegung der von uns definierten Eigenschaft gestatten, damit die Sammlungsansicht geschlossen werden kann, nachdem der Benutzer eine Auswahl getroffen hat.

Bearbeiten Sie `CityCollectionViewController.cs` die Datei (die automatisch über das Storyboard erstellt wurde), und führen Sie Sie wie folgt aus:

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

## <a name="putting-it-all-together"></a>Alles vereint 

Nun, da wir alle Teile zusammengestellt haben, um unsere Sammlungsansicht aufzufüllen und zu steuern, müssen wir die letzten Änderungen an unserer Hauptansicht vornehmen, um alles zusammenzubringen.

Bearbeiten Sie `ViewController.cs` die Datei (die automatisch über das Storyboard erstellt wurde), und führen Sie Sie wie folgt aus:

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

Der folgende Code zeigt zunächst das ausgewählte Element aus der `SelectedCity` `AppDelegate` -Eigenschaft von an und zeigt es erneut an, wenn der Benutzer eine Auswahl aus der Sammlungsansicht getroffen hat:

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

Wenn Sie die APP erstellen und ausführen, wird die Hauptansicht mit der Standard Stadt angezeigt:

[![](collection-views-images/run01.png "Der Hauptbildschirm")](collection-views-images/run01.png#lightbox)

Wenn der Benutzer auf die Schaltfläche " **Ansicht auswählen** " klickt, wird die Sammlungsansicht angezeigt:

[![](collection-views-images/run02.png "Die Auflistungs Ansicht")](collection-views-images/run02.png#lightbox)

Jede Stadt, deren `CanSelect` -Eigenschaft auf `false` festgelegt ist, wird abgeblendet angezeigt, und der Benutzer kann den Fokus nicht darauf festlegen. Wenn der Benutzer ein Element hervorhebt (in den Fokus setzen), wird der Titel angezeigt, und Sie können den Wert des Parametern verwenden, um das Bild in 3D zu kippen.

Wenn der Benutzer auf ein ausgewähltes Bild klickt, wird die Sammlungsansicht geschlossen und die Hauptansicht mit dem neuen Bild erneut angezeigt:

[![](collection-views-images/run03.png "Ein neues Bild auf dem Startbildschirm")](collection-views-images/run03.png#lightbox)

<a name="Creating-Custom-Layout-and-Reordering-Items" />

## <a name="creating-custom-layout-and-reordering-items"></a>Erstellen eines benutzerdefinierten Layouts und Neuanordnen von Elementen

Eines der wichtigsten Features der Verwendung einer Auflistungs Ansicht ist die Möglichkeit, benutzerdefinierte Layouts zu erstellen. Da tvos von IOS erbt, ist der Prozess zum Erstellen eines benutzerdefinierten Layouts identisch. Weitere Informationen finden Sie in der Dokumentation [zur Einführung in die Sammlungs Ansichten](~/ios/user-interface/controls/uicollectionview.md) .

Kürzlich zu den Sammlungs Ansichten für IOS 9 hinzugefügt wurde die Möglichkeit, die Neuanordnung von Elementen in der Auflistung zu ermöglichen. Da tvos 9 eine Teilmenge von IOS 9 ist, erfolgt dies auf dieselbe Weise. Weitere Informationen finden Sie im Dokument " [Änderungen der Sammlungsansicht](~/ios/user-interface/controls/uicollectionview.md) ".


<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde das Entwerfen und arbeiten mit Sammlungs Ansichten in einer xamarin. tvos-App behandelt. Zuerst wurden alle Elemente erläutert, die die Auflistungs Ansicht bilden. Im nächsten Schritt wurde gezeigt, wie eine Auflistungs Ansicht mithilfe eines Storyboards entworfen und implementiert wird. Schließlich finden Sie Links zu Informationen über das Erstellen von benutzerdefinierten Layouts und Neuanordnen von Elementen.



## <a name="related-links"></a>Verwandte Links

- [tvOS-Beispiele](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [tvOS](https://developer.apple.com/tvos/)
- [tvos Human Interface Guides](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Leitfaden zur APP-Programmierung für tvos](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
