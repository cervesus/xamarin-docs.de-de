---
title: Arbeiten mit TvOS Text- und Suchfelder in Xamarin
description: Dieses Dokument beschreibt das Arbeiten mit Text, und suchen Sie Felder in einer TvOS-app mit Xamarin erstellt wurde. Er bietet einen allgemeinen Überblick über die Felder "Text" und "Suche und erläutert das Tastaturen, Storyboard-Integration, Datenmodelle für die Suche und mehr.
ms.prod: xamarin
ms.assetid: 9EE63CA6-2F31-4EE0-AAE5-82E18CFAC06C
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: f1085044f2147bec0910a87f8a6174c4648ef9b8
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61162430"
---
# <a name="working-with-tvos-text-and-search-fields-in-xamarin"></a>Arbeiten mit TvOS Text- und Suchfelder in Xamarin

Wenn erforderlich, kann Ihre app Xamarin.tvOS kleine Teile des Texts fordern Sie vom Benutzer (z. B. Benutzer-IDs und Kennwörter) mit einem Text-Feld und die Tastatur auf dem Bildschirm:

[![](text-fields-and-search-images/intro01.png "Suchfeld für Beispiel")](text-fields-and-search-images/intro01.png#lightbox)

Sie können optional Schlüsselwort Suche Möglichkeit der app-Inhalte, die über ein Suchfeld angeben:

[![](text-fields-and-search-images/intro02.png "Beispiel-Suchergebnisse")](text-fields-and-search-images/intro02.png#lightbox)

In diesem Dokument behandelt die Details für die Arbeit mit Text- und Suchfelder in einer Xamarin.tvOS-app.

<a name="About-Text-and-Search-Fields" />

## <a name="about-text-and-search-fields"></a>Informationen zu Text- und Suchfelder

Wie oben angegeben, falls erforderlich, kann Ihre Xamarin.tvOS darstellen eine oder mehrere Textfelder zum Sammeln von kleine Mengen an Text aus dem Benutzer mithilfe einer auf dem Bildschirm (oder optional Bluetooth-Tastatur, je nach Version TvOS, die der Benutzer hat installiert). 

Darüber hinaus, wenn Ihre app große Mengen von Inhalt für den Benutzer (z. B. eine Musik, Filme oder eine Bild-Auflistung) darstellt, möchten Sie ein Search-Feld enthalten, die dem Benutzer ermöglicht, eine kleine Menge von Text in die Liste der verfügbaren Elemente zu filtern, geben Sie ein.

<a name="Text-Fields" />

## <a name="text-fields"></a>Textfelder

In TvOS-ein Text-Feld wird angezeigt, als eine feste Höhe, gerundet angrenzt Eingabefeld, die Sie verfügbar macht eine Bildschirmtastatur, wenn der Benutzer darauf klickt:

[![](text-fields-and-search-images/text01.png "Text Felder In tvOS")](text-fields-and-search-images/text01.png#lightbox)

Wenn der Benutzer wechselt [Fokus](~/ios/tvos/app-fundamentals/navigation-focus.md) zu einem bestimmten Text-Feld, wird immer größer und einen tiefen Schatten angezeigt. Sie müssen dies Bedenken beim Entwerfen der Benutzeroberfläche, wie Textfelder auf andere UI-Elemente, wenn im Fokus überlappen können.

Apple hat die folgenden Vorschläge für die Arbeit mit Text-Felder:

- **Verwenden Sie Text Entry sparsam** – aufgrund der Natur von der Bildschirmtastatur, längere Abschnitte des Texts eingeben oder mehrere Text-Felder ausfüllen ist schwierig, den Benutzer. Eine bessere Lösung ist die Menge der Texteingabe zu begrenzen, indem Sie die Auswahllisten oder [Schaltflächen](~/ios/tvos/user-interface/buttons.md).
- **Verwenden Sie die Hinweise an, die Kommunikation Zweck** -Text-Feld kann Platzhalter "Hinweise" anzeigen, wenn keine Angabe erfolgt. Falls zutreffend, verwenden Sie Hinweise um zu beschreiben, das Text-Feld, anstatt eine separate Bezeichnung ein.
- **Wählen Sie den entsprechenden Standard Tastatur** -TvOS mehrere, speziell entwickelte Tastenkombinationen stellt verschiedene Typen bereit, die Sie für das Text-Feld angeben können. Beispielsweise kann der Tastatur des e-Mail-Adresse Eintrag erleichtern, indem der Benutzer eine Liste der zuletzt eingegebene Adressen aus.
- **Verwenden Sie bei Bedarf Textfelder Secure** -eine sichere Textfeld zeigt die Zeichen, Punkte (anstelle der echten Buchstaben) eingegeben. Verwenden Sie immer ein Textfeld Sichern auf, wenn vertraulichen Informationen wie z. B. Kennwörter gesammelt werden.

<a name="Keyboards" />

## <a name="keyboards"></a>Tastaturen

Tastatur wird angezeigt, wenn der Benutzer auf dem Bildschirm auf ein Textfeld in der Benutzeroberfläche, ein lineares klickt. Der Benutzer verwendet die Touch-Oberfläche die [Siri Remote](~/ios/tvos/platform/remote-bluetooth.md#The-Siri-Remote) wählen Sie die einzelne Buchstaben auf der Tastatur und geben die angeforderten Informationen:

[![](text-fields-and-search-images/keyboard01.png "Der Siri-Remote-Tastatur")](text-fields-and-search-images/keyboard01.png#lightbox)

Wenn mehr als ein Textfeld auf der aktuellen Sicht und eine **Weiter** Schaltfläche wird den Benutzer auf das nächste Text-Feld wird automatisch angezeigt werden. Ein **Fertig** Schaltfläche erscheint für das letzte Textfeld ein, die Texteingabe zu beenden und den Benutzer zum vorherigen Bildschirm zurückzukehren. 

Benutzer kann jederzeit auch Drücken der **Menü** Schaltfläche auf dem Remotecomputer Siri Texteingabe zu beenden und wieder zurück zum vorherigen Bildschirm.

Apple hat die folgenden Vorschläge für die Arbeit mit auf dem Bildschirm Tastaturen:

- **Wählen Sie den entsprechenden Standard Tastatur** -TvOS mehrere, speziell entwickelte Tastenkombinationen stellt verschiedene Typen bereit, die Sie für das Text-Feld angeben können. Beispielsweise kann der Tastatur des e-Mail-Adresse Eintrag erleichtern, indem der Benutzer eine Liste der zuletzt eingegebene Adressen aus.
- **Verwenden Sie bei Bedarf Tastatur Zubehör Ansichten** : Zusätzlich zu den Standard Informationen, die immer angezeigt, optional Zubehör-Ansichten (z. B. Bilder oder Bezeichnungen) hinzugefügt werden kann die Bildschirmtastatur, um eine Erläuterung des Zwecks der Eingabe von Text oder auf Bei der Benutzer an der die erforderlichen Informationen eingeben.

Weitere Informationen zum Arbeiten mit der Bildschirmtastatur, informieren Sie sich von Apple [UIKeyboardType](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITextInputTraits_Protocol/index.html#//apple_ref/c/tdef/UIKeyboardType), [Verwalten von der Tastatur](https://developer.apple.com/library/tvos/documentation/StringsTextFonts/Conceptual/TextAndWebiPhoneOS/KeyboardManagement/KeyboardManagement.html#//apple_ref/doc/uid/TP40009542-CH5-SW1), [benutzerdefinierte Ansichten für die Dateneingabe](https://developer.apple.com/library/tvos/documentation/StringsTextFonts/Conceptual/TextAndWebiPhoneOS/InputViews/InputViews.html#//apple_ref/doc/uid/TP40009542-CH12-SW1) und [ Text-Programmierhandbuch für iOS-](https://developer.apple.com/library/tvos/documentation/StringsTextFonts/Conceptual/TextAndWebiPhoneOS/Introduction/Introduction.html) Dokumentation.

<a name="Search" />

## <a name="search"></a>Suchen

Ein Suchfeld ein spezielle Bildschirm ein Textfeld bereitgestellt, und Tastatur, die auf dem Bildschirm ermöglicht dem Benutzer, die eine Auflistung von Elementen zu filtern, die unterhalb der Tastatur angezeigt werden:

[![](text-fields-and-search-images/search01.png "Beispiel-Suchergebnisse")](text-fields-and-search-images/search01.png#lightbox)

Wenn es sich bei der Benutzer die Buchstaben in das Suchfeld eingibt, werden die folgenden Ergebnisse automatisch die Ergebnisse der Suche übernommen. Zu jedem Zeitpunkt kann der Benutzer ändert den Fokus auf die Ergebnisse, und wählen Sie eines der Elemente angezeigt.

Apple hat die folgenden Vorschläge für die Arbeit mit Suchfelder:

- **Geben Sie die letzte Suchvorgänge** : Da die Eingabe von Text mit dem Remoterepository Siri kann mühsam sein, und Benutzer neigen dazu, wiederholen die Anforderungen zur Suche, erwägen einen Abschnitt der aktuellen Suchergebnisse, bevor Sie die aktuellen Ergebnisse im Bereich Tastatur.
- **Die Anzahl der Ergebnisse begrenzen, wenn möglich,** : Da es sich bei eine umfangreiche Liste von Elementen für den Benutzer zu analysieren und zu navigieren, sollten die Anzahl der zurückgegebenen Ergebnisse begrenzen schwierig sein kann.
- **Gegebenenfalls geben Suchergebnis filtert** : Wenn der Inhalt bereitgestellt, die Ihre app selbst ist erwägen Bereich Balken, um dem Benutzer ermöglichen, die zurückgegebenen Suchergebnissen weiter zu filtern.

Weitere Informationen finden Sie unter Apple [UISearchController Class Reference](https://developer.apple.com/library/tvos/documentation/UIKit/Reference/UISearchController/index.html).

<a name="Working-with-Text-Fields" />

## <a name="working-with-text-fields"></a>Arbeiten mit Text-Felder

Die einfachste Möglichkeit zum Arbeiten mit Textfeldern in einer Xamarin.tvOS-app werden sie auf die Verwendung des iOS Designers Benutzeroberfläche hinzufügen.

Führen Sie folgende Schritte aus:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

1. In der **Lösungspad**, doppelklicken Sie auf die `Main.storyboard` Datei, die sie für die Bearbeitung zu öffnen.
1. Ziehen Sie eine oder mehrere **Textfelder** Int der Entwurfsoberfläche auf eine Ansicht: 

    [![](text-fields-and-search-images/text02.png "Ein Text-Feld")](text-fields-and-search-images/text02.png#lightbox)
1. Wählen Sie die **Textfelder** und geben Sie jeweils eine eindeutige **Namen** in die **Widget** Registerkarte die **Pad "Eigenschaften"**: 

    [![](text-fields-and-search-images/text03.png "Das Pad \"Eigenschaften\" der Registerkarte \"Widget\"")](text-fields-and-search-images/text03.png#lightbox)
1. In der **Textfeld** Abschnitt definieren Sie Elemente wie z. B. die **Platzhalter** -Hinweis und standardmäßige **Wert**: 

    [![](text-fields-and-search-images/text04.png "Der Abschnitt Text-Feld")](text-fields-and-search-images/text04.png#lightbox)
1. Scrollen Sie nach unten, um Eigenschaften zu definieren, wie z. B. **Rechtschreibprüfung**, **Großschreibung** und der standardmäßige **Tastaturtyp**: 

    [![](text-fields-and-search-images/text05.png "Überprüfen der Rechtschreibung, Groß-/Kleinschreibung und dem Standard-Tastaturtyp")](text-fields-and-search-images/text05.png#lightbox) 
1. Speichern Sie die Änderungen, um das Storyboard.
    
# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)
    
1. Doppelklicken Sie im **Projektmappen-Explorer** auf die Datei `Main.storyboard`, um sie zur Bearbeitung zu öffnen.
1. Ziehen Sie eine oder mehrere **Textfelder** Int der Entwurfsoberfläche auf eine Ansicht: 

    [![](text-fields-and-search-images/text02-vs.png "Ein Text-Feld")](text-fields-and-search-images/text02-vs.png#lightbox)
1. Wählen Sie die **Textfelder** und geben Sie jeweils eine eindeutige **Namen** in die **Widget** Registerkarte die **Eigenschaften-Explorer**: 

    [![](text-fields-and-search-images/text03-vs.png "Die Registerkarte \"Widget\"")](text-fields-and-search-images/text03-vs.png#lightbox)
1. In der **Textfeld** Abschnitt definieren Sie Elemente wie z. B. die **Platzhalter** -Hinweis und standardmäßige **Wert**: 

    [![](text-fields-and-search-images/text04-vs.png "Der Abschnitt Text-Feld")](text-fields-and-search-images/text04-vs.png#lightbox)
1. Scrollen Sie nach unten, um Eigenschaften zu definieren, wie z. B. **Rechtschreibprüfung**, **Großschreibung** und der standardmäßige **Tastaturtyp**: 

    [![](text-fields-and-search-images/text05-vs.png "Überprüfen der Rechtschreibung, Groß-/Kleinschreibung und dem Standard-Tastaturtyp")](text-fields-and-search-images/text05-vs.png#lightbox) 
1. Speichern Sie die Änderungen, um das Storyboard.
    
-----

Im Code können Sie abrufen oder legen Sie den Wert eines Felds von Text mit der `Text` Eigenschaft:

```csharp
Console.WriteLine ("User ID {0} and Password {1}", UserId.Text, Password.Text);
```

Optional können Sie die `Started` und `Ended` Textfeld Ereignisse reagieren, Texteingabe beginnt und endet.

<a name="Working-with-Search-Fields" />

## <a name="working-with-search-fields"></a>Arbeiten mit Suchfelder

Die einfachste Möglichkeit, ein Xamarin.tvOS-app mit Suchfelder arbeiten werden sie mithilfe des Benutzeroberflächen-Designer auf die Benutzeroberfläche hinzufügen.

Führen Sie folgende Schritte aus:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)
    
1. In der **Lösungspad**, doppelklicken Sie auf die `Main.storyboard` Datei, die sie für die Bearbeitung zu öffnen.
1. Ziehen Sie eine neue Auflistung-View-Controller aus, in das Storyboard, um die Ergebnisse der Suche von des Benutzers darstellen: 

    [![](text-fields-and-search-images/search02.png "Eine Auflistung-Ansichtscontroller")](text-fields-and-search-images/search02.png#lightbox)
1. In der **Widget** auf der Registerkarte die **Pad "Eigenschaften"**, verwenden Sie `SearchResultsViewController` für die **Klasse** und `SearchResults` für die **Storyboard-ID**: 

    [![](text-fields-and-search-images/search03.png "Die Registerkarte \"Widget\"")](text-fields-and-search-images/search03.png#lightbox)
1. Wählen Sie die **Zelle Prototyp** auf der Entwurfsoberfläche angezeigt.
1. In der **Widget** auf der Registerkarte die **Eigenschaften-Explorer**, verwenden Sie `SearchResultCell` für die **Klasse** und `ImageCell` für die **Bezeichner**: 

    [![](text-fields-and-search-images/search04.png "Die Registerkarte \"Widget\"")](text-fields-and-search-images/search04.png#lightbox)
1. Layout des Entwurfs von der **Zelle Prototyp** und verfügbar zu machen jedes Element mit einem eindeutigen **Namen** in die **Widget** Registerkarte die **Eigenschaften-Explorer**: 

    [![](text-fields-and-search-images/search05.png "Layout der Entwurf des Prototyps Zelle")](text-fields-and-search-images/search05.png#lightbox)
1. Speichern Sie die Änderungen, um das Storyboard.
    
# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)
    
1. Doppelklicken Sie im **Projektmappen-Explorer** auf die Datei `Main.storyboard`, um sie zur Bearbeitung zu öffnen.
1. Ziehen Sie eine neue Auflistung-View-Controller aus, in das Storyboard, um die Ergebnisse der Suche von des Benutzers darstellen: 

    [![](text-fields-and-search-images/seach02-vs.png "Eine Auflistung-Ansichtscontroller")](text-fields-and-search-images/seach02-vs.png#lightbox)
1. In der **Widget** auf der Registerkarte die **Eigenschaften-Explorer**, verwenden Sie `SearchResultsViewController` für die **Klasse** und `SearchResults` für die **Storyboard-ID**: 

    [![](text-fields-and-search-images/search03-vs.png "Die Registerkarte \"Widget\"")](text-fields-and-search-images/search03-vs.png#lightbox)
1. Wählen Sie die **Zelle Prototyp** auf der Entwurfsoberfläche angezeigt.
1. In der **Widget** auf der Registerkarte die **Eigenschaften-Explorer**, verwenden Sie `SearchResultCell` für die **Klasse** und `ImageCell` für die **Bezeichner**: 

    [![](text-fields-and-search-images/search04-vs.png "Die Registerkarte \"Widget\"")](text-fields-and-search-images/search04-vs.png#lightbox)
1. Layout des Entwurfs von der **Zelle Prototyp** und verfügbar zu machen jedes Element mit einem eindeutigen **Namen** in die **Widget** Registerkarte die **Eigenschaften-Explorer**: 

    [![](text-fields-and-search-images/search05-vs.png "Layout der Entwurf des Prototyps Zelle")](text-fields-and-search-images/search05-vs.png#lightbox)
1. Speichern Sie die Änderungen, um das Storyboard.
    
-----

<a name="Provide-a-Data-Model" />

### <a name="provide-a-data-model"></a>Geben Sie ein Datenmodell

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

Als Nächstes müssen Sie eine Klasse, die als Datenmodell für die Ergebnisse fungiert, die der Benutzer suchte. In der **Projektmappen-Explorer**mit der rechten Maustaste auf den Projektnamen, und wählen Sie **hinzufügen** > **neue Datei...**   >  **Allgemeine** > **leere Klasse** und geben Sie einen **Namen**: 

[![](text-fields-and-search-images/search06.png "Wählen Sie die leere Klasse, und geben Sie einen Namen")](text-fields-and-search-images/search06.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Als Nächstes müssen Sie eine Klasse, die als Datenmodell für die Ergebnisse fungiert, die der Benutzer suchte. In der **Projektmappen-Explorer**mit der rechten Maustaste auf den Projektnamen, und wählen Sie **hinzufügen** > **neues Element...**   >  **Apple** > **Verschiedenes** > **Klasse** und geben Sie einen **Namen**: 

[![](text-fields-and-search-images/search06-vs.png "Wählen Sie die Klasse, und geben Sie einen Namen")](text-fields-and-search-images/search06-vs.png#lightbox)

-----

Beispielsweise könnte eine app, die dem Benutzer ermöglicht, eine Sammlung von Bildern von Titel und Schlüsselwort suchen wie folgt aussehen:

```csharp
using System;
using Foundation;

namespace tvText
{
    public class PictureInformation : NSObject
    {
        #region Computed Properties
        public string Title { get; set;}
        public string ImageName { get; set;}
        public string Keywords { get; set;}
        #endregion

        #region Constructors
        public PictureInformation (string title, string imageName, string keywords)
        {
            // Initialize
            this.Title = title;
            this.ImageName = imageName;
            this.Keywords = keywords;
        }
        #endregion
    }
}
```

<a name="The-Collection-View-Cell" />

### <a name="the-collection-view-cell"></a>Die Sammlungsansichtszelle

Mit dem Modell Daten direktes Bearbeiten der **Prototyp Zelle** (`SearchResultViewCell.cs`) und Aussehen liegen die folgenden:

```csharp
using Foundation;
using System;
using UIKit;

namespace tvText
{
    public partial class SearchResultViewCell : UICollectionViewCell
    {
        #region Private Variables
        private PictureInformation _pictureInfo = null;
        #endregion

        #region Computed Properties
        public PictureInformation PictureInfo {
            get { return _pictureInfo; }
            set {
                _pictureInfo = value;
                UpdateUI ();
            }
        }
        #endregion

        #region Constructors
        public SearchResultViewCell (IntPtr handle) : base (handle)
        {
            // Initialize
            UpdateUI ();
        }
        #endregion

        #region Private Methods
        private void UpdateUI ()
        {
            // Anything to process?
            if (PictureInfo == null) return;

            try {
                Picture.Image = UIImage.FromBundle (PictureInfo.ImageName);
                Picture.AdjustsImageWhenAncestorFocused = true;
                Title.Text = PictureInfo.Title;
                TextColor = UIColor.LightGray;
            } catch {
                // Ignore errors if view isn't fully loaded
            }
        }
        #endregion
    }

}
```

Die `UpdateUI` zum Anzeigen der einzelnen Felder der Methode verwendet werden die **PictureInformation** Elemente (die `PictureInfo` Eigenschaft) in der benannten Elemente der Benutzeroberfläche jedes Mal die Eigenschaft wird aktualisiert. Beispielsweise das Bild und Titel, die dem Bild zugeordnet.

<a name="The-Collection-View-Controller" />

### <a name="the-collection-view-controller"></a>Die Sammlungsansichtscontroller

Als Nächstes bearbeiten Sie die Suche Ergebnisse-Sammlungsansichtscontroller (`SearchResultsViewController.cs`), und legen Sie ihn wie folgt aussehen:

```csharp
using Foundation;
using System;
using UIKit;
using System.Collections.Generic;

namespace tvText
{
    public partial class SearchResultsViewController : UICollectionViewController , IUISearchResultsUpdating
    {
        #region Constants
        public const string CellID = "ImageCell";
        #endregion

        #region Private Variables
        private string _searchFilter = "";
        #endregion

        #region Computed Properties
        public List<PictureInformation> AllPictures { get; set;}
        public List<PictureInformation> FoundPictures { get; set; }
        public string SearchFilter {
            get { return _searchFilter; }
            set {
                _searchFilter = value.ToLower();
                FindPictures ();
                CollectionView?.ReloadData ();
            }
        }
        #endregion

        #region Constructors
        public SearchResultsViewController (IntPtr handle) : base (handle)
        {
            // Initialize
            this.AllPictures = new List<PictureInformation> ();
            this.FoundPictures = new List<PictureInformation> ();
            PopulatePictures ();
            FindPictures ();

        }
        #endregion

        #region Private Methods
        private void PopulatePictures ()
        {
            // Clear list
            AllPictures.Clear ();

            // Add images
            AllPictures.Add (new PictureInformation ("Antipasta Platter","Antipasta","cheese,grapes,tomato,coffee,meat,plate"));
            AllPictures.Add (new PictureInformation ("Cheese Plate", "CheesePlate", "cheese,plate,bread"));
            AllPictures.Add (new PictureInformation ("Coffee House", "CoffeeHouse", "coffee,people,menu,restaurant,cafe"));
            AllPictures.Add (new PictureInformation ("Computer and Expresso", "ComputerExpresso", "computer,coffee,expresso,phone,notebook"));
            AllPictures.Add (new PictureInformation ("Hamburger", "Hamburger", "meat,bread,cheese,tomato,pickle,lettus"));
            AllPictures.Add (new PictureInformation ("Lasagna Dinner", "Lasagna", "salad,bread,plate,lasagna,pasta"));
            AllPictures.Add (new PictureInformation ("Expresso Meeting", "PeopleExpresso", "people,bag,phone,expresso,coffee,table,tablet,notebook"));
            AllPictures.Add (new PictureInformation ("Soup and Sandwich", "SoupAndSandwich", "soup,sandwich,bread,meat,plate,tomato,lettus,egg"));
            AllPictures.Add (new PictureInformation ("Morning Coffee", "TabletCoffee", "tablet,person,man,coffee,magazine,table"));
            AllPictures.Add (new PictureInformation ("Evening Coffee", "TabletMagCoffee", "tablet,magazine,coffee,table"));
        }

        private void FindPictures ()
        {
            // Clear list
            FoundPictures.Clear ();

            // Scan each picture for a match
            foreach (PictureInformation picture in AllPictures) {
                if (SearchFilter == "") {
                    // If no search term, everything matches
                    FoundPictures.Add (picture);
                } else if (picture.Title.Contains (SearchFilter) || picture.Keywords.Contains (SearchFilter)) {
                    // If the search term is in the title or keywords, we've found a match
                    FoundPictures.Add (picture);
                }
            }
        }
        #endregion

        #region Override Methods
        public override nint NumberOfSections (UICollectionView collectionView)
        {
            // Only one section in this collection
            return 1;
        }

        public override nint GetItemsCount (UICollectionView collectionView, nint section)
        {
            // Return the number of matching pictures
            return FoundPictures.Count;
        }

        public override UICollectionViewCell GetCell (UICollectionView collectionView, NSIndexPath indexPath)
        {
            // Get a new cell and return it
            var cell = collectionView.DequeueReusableCell (CellID, indexPath);
            return (UICollectionViewCell)cell;
        }

        public override void WillDisplayCell (UICollectionView collectionView, UICollectionViewCell cell, NSIndexPath indexPath)
        {
            // Grab the cell
            var currentCell = cell as SearchResultViewCell;
            if (currentCell == null)
                throw new Exception ("Expected to display a `SearchResultViewCell`.");

            // Display the current picture info in the cell
            var item = FoundPictures [indexPath.Row];
            currentCell.PictureInfo = item;
        }

        public override void ItemSelected (UICollectionView collectionView, NSIndexPath indexPath)
        {
            // If this Search Controller was presented as a modal view, close
            // it before continuing
            // DismissViewController (true, null);

            // Grab the picture being selected and report it
            var picture = FoundPictures [indexPath.Row];
            Console.WriteLine ("Selected: {0}", picture.Title);
        }

        public void UpdateSearchResultsForSearchController (UISearchController searchController)
        {
            // Save the search filter and update the Collection View
            SearchFilter = searchController.SearchBar.Text ?? string.Empty;
        }

        public override void DidUpdateFocus (UIFocusUpdateContext context, UIFocusAnimationCoordinator coordinator)
        {
            var previousItem = context.PreviouslyFocusedView as SearchResultViewCell;
            if (previousItem != null) {
                UIView.Animate (0.2, () => {
                    previousItem.TextColor = UIColor.LightGray;
                });
            }

            var nextItem = context.NextFocusedView as SearchResultViewCell;
            if (nextItem != null) {
                UIView.Animate (0.2, () => {
                    nextItem.TextColor = UIColor.Black;
                });
            }
        }
        #endregion
    }
}
```

Zunächst wird die `IUISearchResultsUpdating` Schnittstelle wird von der-Klasse zur Verarbeitung von Search controllerfilter vom Benutzer aktualisiert wird hinzugefügt:

```csharp
public partial class SearchResultsViewController : UICollectionViewController , IUISearchResultsUpdating
```

Eine Konstante wird auch definiert, um die ID des angeben der **Prototyp Zelle** (entspricht die ID im Benutzeroberflächen-Designer oben definiert), wird später verwendet werden, wenn der Controller für die Auflistung eine neue Zelle angefordert:

```csharp
public const string CellID = "ImageCell";
```

Speicher für die vollständige Liste der Elemente, die durchsucht wird, wird des Filter Suchbegriffs erstellt und eine Liste der Elemente, die dieser Begriff:

```csharp
private string _searchFilter = "";
...

public List<PictureInformation> AllPictures { get; set;}
public List<PictureInformation> FoundPictures { get; set; }
public string SearchFilter {
    get { return _searchFilter; }
    set {
        _searchFilter = value.ToLower();
        FindPictures ();
        CollectionView?.ReloadData ();
    }
}
```

Wenn die `SearchFilter` wird geändert, wird die Liste der übereinstimmenden Elementen aktualisiert und die Auflistungsansicht Inhalt wird erneut geladen. Die `FindPictures` Routine ist verantwortlich für die Suche nach Elementen, die die neuen Suchbegriff entsprechen:

```csharp
private void FindPictures ()
{
    // Clear list
    FoundPictures.Clear ();

    // Scan each picture for a match
    foreach (PictureInformation picture in AllPictures) {
        if (SearchFilter == "") {
            // If no search term, everything matches
            FoundPictures.Add (picture);
        } else if (picture.Title.Contains (SearchFilter) || picture.Keywords.Contains (SearchFilter)) {
            // If the search term is in the title or keywords, we've found a match
            FoundPictures.Add (picture);
        }
    }
}
```

Der Wert des der `SearchFilter` aktualisiert werden (die die Ergebnisse Auflistungsansicht aktualisiert) Wenn der Benutzer den Filter in der Suche Controller ändert:

```csharp
public void UpdateSearchResultsForSearchController (UISearchController searchController)
{
    // Save the search filter and update the Collection View
    SearchFilter = searchController.SearchBar.Text ?? string.Empty;
}
```

Die `PopulatePictures` Methode füllt zuerst die Auflistung der verfügbaren Elemente:

```csharp
private void PopulatePictures ()
{
    // Clear list
    AllPictures.Clear ();

    // Add images
    AllPictures.Add (new PictureInformation ("Antipasta Platter","Antipasta","cheese,grapes,tomato,coffee,meat,plate"));
    ...
}
```

Dieses Beispiel alle Beispieldaten wird jetzt erstellt im Arbeitsspeicher beim Laden der Auflistung-View-Controller. In einer echten Anwendung diese Daten würden wahrscheinlich aus einer Datenbank oder Web-Dienst gelesen werden, und nur bei Bedarf beibehalten, dass überschritten das Apple TV Arbeitsspeicher beschränkt.

Die `NumberOfSections` und `GetItemsCount` Methoden geben die Anzahl der übereinstimmenden Elemente:

```csharp
public override nint NumberOfSections (UICollectionView collectionView)
{
    // Only one section in this collection
    return 1;
}

public override nint GetItemsCount (UICollectionView collectionView, nint section)
{
    // Return the number of matching pictures
    return FoundPictures.Count;
}
```

Die `GetCell` Methode gibt ein neues **Prototyp Zelle** (basierend auf den `CellID` im Storyboard oben definierten) für jedes Element in der Auflistung:

```csharp
public override UICollectionViewCell GetCell (UICollectionView collectionView, NSIndexPath indexPath)
{
    // Get a new cell and return it
    var cell = collectionView.DequeueReusableCell (CellID, indexPath);
    return (UICollectionViewCell)cell;
}
```

Die `WillDisplayCell` Methode wird aufgerufen, bevor Sie die Zelle, die angezeigt wird, damit diese konfiguriert werden kann:

```csharp
public override void WillDisplayCell (UICollectionView collectionView, UICollectionViewCell cell, NSIndexPath indexPath)
{
    // Grab the cell
    var currentCell = cell as SearchResultViewCell;
    if (currentCell == null)
        throw new Exception ("Expected to display a `SearchResultViewCell`.");

    // Display the current picture info in the cell
    var item = FoundPictures [indexPath.Row];
    currentCell.PictureInfo = item;
}
```

Die `DidUpdateFocus` Methode stellt visuelles Feedback an den Benutzer bereit, wie sie Elemente in der Auflistungsansicht Ergebnisse markieren:

```csharp
public override void DidUpdateFocus (UIFocusUpdateContext context, UIFocusAnimationCoordinator coordinator)
{
    var previousItem = context.PreviouslyFocusedView as SearchResultViewCell;
    if (previousItem != null) {
        UIView.Animate (0.2, () => {
            previousItem.TextColor = UIColor.LightGray;
        });
    }

    var nextItem = context.NextFocusedView as SearchResultViewCell;
    if (nextItem != null) {
        UIView.Animate (0.2, () => {
            nextItem.TextColor = UIColor.Black;
        });
    }
}
```

Zum Schluss die `ItemSelected` Methode verarbeitet den Benutzer, die in der Ergebnisansicht der Auflistung ein Element, das (Klicken auf der Touch-Oberfläche, mit dem Remoterepository Siri) auswählen:

```csharp
public override void ItemSelected (UICollectionView collectionView, NSIndexPath indexPath)
{
    // If this Search Controller was presented as a modal view, close
    // it before continuing
    // DismissViewController (true, null);

    // Grab the picture being selected and report it
    var picture = FoundPictures [indexPath.Row];
    Console.WriteLine ("Selected: {0}", picture.Title);
}
```

Wenn die Search-Feld als ein modales Dialogfeld anzeigen (über den oberen Rand der Ansicht, die sie aufgerufen wird), verwenden präsentiert wurde die `DismissViewController` Methode, um die Ansicht zu schließen, wenn der Benutzer ein Element auswählt. In diesem Beispiel wird das Suchfeld ein als Inhalt von einer Registerkarte Registerkartenansicht angezeigt, damit es hier nicht verworfen wird.

Weitere Informationen zu Auflistungsansichten, informieren Sie sich unsere [arbeiten mit Auflistungsansichten](~/ios/tvos/user-interface/collection-views.md) Dokumentation.

<a name="Presenting the Search Field" />

### <a name="presenting-the-search-field"></a>Das Suchfeld darstellen

Es gibt zwei Hauptmethoden, die ein Suchfeld (und die zugehörigen Bildschirmtastatur und Suchergebnisse) kann in TvOS an den Benutzer angezeigt werden: 

- **Anzeigen der modalen Dialogfeld** -der Search-Feld kann über die aktuelle angezeigt werden, anzeigen und die View-Controller als modales Dialogfeld Vollbild-Ansicht. Dies erfolgt in der Regel als Reaktion auf der Benutzer auf eine Schaltfläche oder andere Elemente der Benutzeroberfläche. Das Dialogfeld wird geschlossen, wenn der Benutzer ein Element in den Suchergebnissen auswählt.
- **Zeigen Sie Inhalt** -Feld für die Suche wird direkter Teil einer angegebenen Ansicht. Z. B. der Inhalt einer Registerkarte "Suchen" in einer Registerkarte-View-Controller.

Das Beispiel für eine durchsuchbare Liste mit den oben angegebenen Bilder das Suchfeld ein, die als Inhalt anzeigen, in der Registerkarte "Suche" dargestellt wird, und der Suche Registerkarte View-Controller sieht folgendermaßen aus:

```csharp
using System;
using UIKit;

namespace tvText
{
    public partial class SecondViewController : UIViewController
    {
        #region Constants
        public const string SearchResultsID = "SearchResults";
        #endregion

        #region Computed Properties
        public SearchResultsViewController ResultsController { get; set;}
        #endregion

        #region Constructors
        public SecondViewController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Private Methods
        public void ShowSearchController ()
        {
            // Build an instance of the Search Results View Controller from the Storyboard
            ResultsController = Storyboard.InstantiateViewController (SearchResultsID) as SearchResultsViewController;
            if (ResultsController == null)
                throw new Exception ("Unable to instantiate a SearchResultsViewController.");

            // Create an initialize a new search controller
            var searchController = new UISearchController (ResultsController) {
                SearchResultsUpdater = ResultsController,
                HidesNavigationBarDuringPresentation = false
            };

            // Set any required search parameters
            searchController.SearchBar.Placeholder = "Enter keyword (e.g. coffee)";

            // The Search Results View Controller can be presented as a modal view
            // PresentViewController (searchController, true, null);

            // Or in the case of this sample, the Search View Controller is being
            // presented as the contents of the Search Tab directly. Use either one
            // or the other method to display the Search Controller (not both).
            var container = new UISearchContainerViewController (searchController);
            var navController = new UINavigationController (container);
            AddChildViewController (navController);
            View.Add (navController.View);
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // If the Search Controller is being displayed as the content
            // of the search tab, include it here.
            ShowSearchController ();
        }

        public override void ViewDidAppear (bool animated)
        {
            base.ViewDidAppear (animated);

            // If the Search Controller is being presented as a modal view,
            // call it here to display it over the contents of the Search
            // tab.
            // ShowSearchController ();
        }
        #endregion
    }
}
```

Zunächst eine Konstante wird definiert, die entspricht der **Storyboard-ID** , die der Auflistung Anzeige-Controller im Benutzeroberflächen-Designer zugewiesen wurde:

```csharp
public const string SearchResultsID = "SearchResults";
```

Als Nächstes die `ShowSearchController` Methode erstellt eine neue Suche Auflistung Ansichtscontroller und zeigt, die es benötigt wurde:

```csharp
public void ShowSearchController ()
{
    // Build an instance of the Search Results View Controller from the Storyboard
    ResultsController = Storyboard.InstantiateViewController (SearchResultsID) as SearchResultsViewController;
    if (ResultsController == null)
        throw new Exception ("Unable to instantiate a SearchResultsViewController.");

    // Create an initialize a new search controller
    var searchController = new UISearchController (ResultsController) {
        SearchResultsUpdater = ResultsController,
        HidesNavigationBarDuringPresentation = false
    };

    // Set any required search parameters
    searchController.SearchBar.Placeholder = "Enter keyword (e.g. coffee)";

    // The Search Results View Controller can be presented as a modal view
    // PresentViewController (searchController, true, null);

    // Or in the case of this sample, the Search View Controller is being
    // presented as the contents of the Search Tab directly. Use either one
    // or the other method to display the Search Controller (not both).
    var container = new UISearchContainerViewController (searchController);
    var navController = new UINavigationController (container);
    AddChildViewController (navController);
    View.Add (navController.View);
}
```

In der oben genannten Methode einmal eine `SearchResultsViewController` aus dem Storyboard, instanziiert wurde ein neues `UISearchController` wird erstellt, um das Suchfeld darstellen und Bildschirmtastatur für den Benutzer. Die Suchergebnisse-Auflistung (gemäß der `SearchResultsViewController`) wird unter diesem Tastatur angezeigt werden.

Als Nächstes die `SearchBar` wird mit Informationen wie z. B. konfiguriert die **Platzhalter** Hinweis. Dies bietet Informationen um den Benutzer über den Typ der Suche durchgeführt wird.

Anschließend wird das Suchfeld für den Benutzer auf zwei Arten dargestellt:

- **Modales Dialogfeld anzeigen** : die `PresentViewController` Methode wird aufgerufen, um die Suche über die vorhandene Ansicht zu präsentieren Vollbildmodus.
- **Zeigen Sie Inhalt** – eine `UISearchContainerViewController` wird erstellt, um den Search-Controller enthalten. Ein `UINavigationController` wird erstellt, den Durchsuchen-Container, und klicken Sie dann die View-Controller der Navigationscontroller hinzugefügt wird `AddChildViewController (navController)`, und die Ansicht präsentiert `View.Add (navController.View)`.

Schließlich und erneut auf Grundlage des darstellungstyps, entweder die `ViewDidLoad` oder `ViewDidAppear` Methodenaufruf wird der `ShowSearchController` Methode, um die Suche, die der Benutzer angezeigt:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // If the Search Controller is being displayed as the content
    // of the search tab, include it here.
    ShowSearchController ();
}

public override void ViewDidAppear (bool animated)
{
    base.ViewDidAppear (animated);

    // If the Search Controller is being presented as a modal view,
    // call it here to display it over the contents of the Search
    // tab.
    // ShowSearchController ();
}
```

Wenn die app ausgeführt wird, und die Registerkarte "Suchen" vom Benutzer ausgewählt wurde, wird die vollständige ungefilterte Liste von Elementen für den Benutzer angezeigt:

[![](text-fields-and-search-images/intro02.png "Standard-Suchergebnisse")](text-fields-and-search-images/intro02.png#lightbox)

Wie der Benutzer beginnt, einen Suchbegriff einzugeben, wird die Liste der Ergebnisse nach dieser Begriff gefiltert und automatisch aktualisiert werden:

[![](text-fields-and-search-images/intro03.png "Gefilterte Ergebnisse")](text-fields-and-search-images/intro03.png#lightbox)

Zu jedem Zeitpunkt kann der Benutzer den Fokus auf ein Element in den Suchergebnissen wechseln und klicken Sie auf die Touch-Oberfläche für den Remotecomputer Siri, um es auszuwählen.

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden behandelt, Entwerfen und Arbeiten mit Text- und Suchfelder in ein Xamarin.tvOS-app. Wurde erläutert, wie Text "und" Sammlung Durchsuchen von Inhalt im Benutzeroberflächen-Designer zu erstellen, und darüber hinaus wurde gezeigt, um zwei verschiedene Möglichkeiten, die für dem Benutzer in TvOS ein Suchfeld angezeigt werden kann.



## <a name="related-links"></a>Verwandte Links

- [tvOS-Beispiele](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [TvOS Human Interface-Handbücher](https://developer.apple.com/tvos/human-interface-guidelines/)
- [App-Programmierhandbuch für tvos verwendet.](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
