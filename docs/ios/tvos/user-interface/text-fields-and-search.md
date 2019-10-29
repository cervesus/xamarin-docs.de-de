---
title: Arbeiten mit tvos-Text-und-Suchfeldern in xamarin
description: In diesem Dokument wird beschrieben, wie Sie in einer mit xamarin erstellten tvos-App mit Text-und Suchfeldern arbeiten. Er bietet eine allgemeine Übersicht über Text-und Suchfelder und erläutert Tastaturen, Storyboard-Integration, Such Datenmodelle und vieles mehr.
ms.prod: xamarin
ms.assetid: 9EE63CA6-2F31-4EE0-AAE5-82E18CFAC06C
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/16/2017
ms.openlocfilehash: cfe747d89b3ccbff390326c915dc1311e40a60fc
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73022165"
---
# <a name="working-with-tvos-text-and-search-fields-in-xamarin"></a>Arbeiten mit tvos-Text-und-Suchfeldern in xamarin

Wenn dies erforderlich ist, kann Ihre xamarin. tvos-App mit einem Textfeld und der Bildschirmtastatur kleine Text Teile vom Benutzer (z. b. Benutzer-IDs und Kenn Wörter) anfordern:

[![](text-fields-and-search-images/intro01.png "Sample Search Field")](text-fields-and-search-images/intro01.png#lightbox)

Optional können Sie die Suche nach Schlüsselwörtern für den Inhalt der App mithilfe eines Suchfelds angeben:

[![](text-fields-and-search-images/intro02.png "Sample Search Results")](text-fields-and-search-images/intro02.png#lightbox)

In diesem Dokument werden die Details zum Arbeiten mit Text-und Suchfeldern in einer xamarin. tvos-App behandelt.

<a name="About-Text-and-Search-Fields" />

## <a name="about-text-and-search-fields"></a>Informationen zu Text-und Suchfeldern

Wie bereits erwähnt, kann Ihre xamarin. tvos bei Bedarf ein oder mehrere Textfelder darstellen, um kleine Mengen von Text vom Benutzer mithilfe einer Bildschirm Abbildung (oder einer optionalen Bluetooth-Tastatur abhängig von der vom Benutzer installierten tvos-Version) zu erfassen.

Wenn Ihre APP außerdem große Mengen an Inhalten für den Benutzer bereitstellt (z. b. eine Musik, Filme oder eine Bild Auflistung), möchten Sie möglicherweise ein Suchfeld einschließen, das es dem Benutzer ermöglicht, eine kleine Menge an Text einzugeben, um die Liste der verfügbaren Elemente zu filtern.

<a name="Text-Fields" />

## <a name="text-fields"></a>Textfelder

In tvos wird ein Textfeld als Eingabefeld mit fester Höhe (gerundet) angezeigt, das eine Bildschirmtastatur erhält, wenn der Benutzer darauf klickt:

[![](text-fields-and-search-images/text01.png "Text Fields In tvOS")](text-fields-and-search-images/text01.png#lightbox)

Wenn der Benutzer den [Fokus](~/ios/tvos/app-fundamentals/navigation-focus.md) auf ein bestimmtes Textfeld verschiebt, vergrößert sich dieser Wert und zeigt einen tiefen Schatten an. Beachten Sie beim Entwerfen der Benutzeroberfläche, dass Text Felder andere Elemente der Benutzeroberfläche überlappen können, wenn Sie sich im Fokus befinden.

Apple hat die folgenden Vorschläge zum Arbeiten mit Text Feldern:

- **Verwendung des Text Eintrags sparsam** : aufgrund der Art der Bildschirmtastatur ist die Eingabe von langen Textabschnitten oder das Ausfüllen mehrerer Textfelder für den Benutzer mühsam. Eine bessere Lösung besteht darin, den Umfang des Text Eintrags mithilfe von Auswahllisten oder [Schalt](~/ios/tvos/user-interface/buttons.md)Flächen einzuschränken.
- **Verwendung von Hinweisen für die Kommunikation** mit dem Textfeld "Zweck-Text" können Platzhalter "Hinweise" angezeigt werden Verwenden Sie ggf. Hinweise, um den Zweck des Textfelds anstelle einer separaten Bezeichnung zu beschreiben.
- **Wählen Sie den geeigneten Standardtyp der Tastatur** aus: tvos stellt verschiedene, zweckgebundene Tastaturtypen bereit, die Sie für das Textfeld angeben können. Beispielsweise kann die e-Mail-Adress Tastatur den Eintrag vereinfachen, indem der Benutzer die Auswahl aus einer Liste der zuletzt eingegebenen Adressen ermöglicht.
- **Verwenden Sie bei Bedarf sichere Textfelder** . in einem sicheren Textfeld werden die als Punkte eingegebenen Zeichen (anstelle der tatsächlichen Buchstaben) dargestellt. Verwenden Sie immer ein sicheres Textfeld, wenn Sie vertrauliche Informationen wie Kenn Wörter sammeln.

<a name="Keyboards" />

## <a name="keyboards"></a>Anzuzeigen

Wenn der Benutzer auf ein Textfeld in der Benutzeroberfläche klickt, wird eine lineare Tastatur auf dem Bildschirm angezeigt. Der Benutzer verwendet die Berührungs Oberfläche von [Siri Remote](~/ios/tvos/platform/remote-bluetooth.md#The-Siri-Remote) , um einzelne Buchstaben auf der Tastatur auszuwählen und die angeforderten Informationen einzugeben:

[![](text-fields-and-search-images/keyboard01.png "The Siri Remote keyboard")](text-fields-and-search-images/keyboard01.png#lightbox)

Wenn in der aktuellen Ansicht mehr als ein Textfeld vorhanden ist, wird automatisch eine Schaltfläche " **weiter** " angezeigt, um den Benutzer zum nächsten Textfeld zu gelangen. Eine **done** -Schaltfläche wird für das letzte Textfeld angezeigt, das den Text Eintrag beendet und den Benutzer an den vorherigen Bildschirm zurückgibt.

Der Benutzer kann jederzeit auch die **Menü** Schaltfläche auf der Seite "Siri-Remote-zu-Ende-Text" drücken und wieder zum vorherigen Bildschirm zurückkehren.

Apple hat die folgenden Vorschläge zum Arbeiten mit Bildschirmtastatur:

- **Wählen Sie den geeigneten Standardtyp der Tastatur** aus: tvos stellt verschiedene, zweckgebundene Tastaturtypen bereit, die Sie für das Textfeld angeben können. Beispielsweise kann die e-Mail-Adress Tastatur den Eintrag vereinfachen, indem der Benutzer die Auswahl aus einer Liste der zuletzt eingegebenen Adressen ermöglicht.
- **Verwenden Sie bei Bedarf Tastatur Zubehör Ansichten** . zusätzlich zu den Standardinformationen, die immer angezeigt werden, können optionale Zubehör Sichten (z. b. Bilder oder Bezeichnungen) der Bildschirmtastatur hinzugefügt werden, um den Zweck des Text Eintrags zu verdeutlichen oder zu unterstützen. der Benutzer, der die erforderlichen Informationen eingegeben hat.

Weitere Informationen zum Arbeiten mit der Bildschirmtastatur finden Sie unter " [uikeyboardtype](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITextInputTraits_Protocol/index.html#//apple_ref/c/tdef/UIKeyboardType)" von Apple, [Verwalten der Tastatur](https://developer.apple.com/library/tvos/documentation/StringsTextFonts/Conceptual/TextAndWebiPhoneOS/KeyboardManagement/KeyboardManagement.html#//apple_ref/doc/uid/TP40009542-CH5-SW1), [benutzerdefinierte Ansichten für Dateneingabe](https://developer.apple.com/library/tvos/documentation/StringsTextFonts/Conceptual/TextAndWebiPhoneOS/InputViews/InputViews.html#//apple_ref/doc/uid/TP40009542-CH12-SW1) und [Text Programmierung in der IOS](https://developer.apple.com/library/tvos/documentation/StringsTextFonts/Conceptual/TextAndWebiPhoneOS/Introduction/Introduction.html) -Dokumentation.

<a name="Search" />

## <a name="search"></a>Suche

Ein Suchfeld stellt einen speziellen Bildschirm mit einem Textfeld und einer Bildschirmtastatur dar, mit dem der Benutzer eine Auflistung von Elementen filtern kann, die unter der Tastatur angezeigt werden:

[![](text-fields-and-search-images/search01.png "Sample search results")](text-fields-and-search-images/search01.png#lightbox)

Wenn der Benutzer Buchstaben in das Suchfeld eingibt, werden die Ergebnisse der Suche automatisch angezeigt. Der Benutzer kann den Fokus jederzeit auf die Ergebnisse verschieben und eines der dargestellten Elemente auswählen.

Apple hat die folgenden Vorschläge zum Arbeiten mit Suchfeldern:

- **Aktuelle Suchvorgänge bereitstellen** : da das Eingeben von Text mit der Siri-Remote zeitaufwändig sein kann und Benutzer häufig Suchanforderungen wiederholen, sollten Sie ggf. einen Abschnitt der letzten Suchergebnisse im Tastatur Bereich vor den aktuellen Ergebnissen hinzufügen.
- **Begrenzen Sie nach Möglichkeit die Anzahl der Ergebnisse,** da eine große Liste von Elementen für den Benutzer schwierig sein kann, die Anzahl der zurückgegebenen Ergebnisse zu analysieren und zu navigieren.
- Stellen Sie ggf. **Suchergebnis Filter bereit** : Wenn sich der von Ihrer APP bereitgestellte Inhalt selbst eignet, können Sie Bereichs leisten hinzufügen, um dem Benutzer zu ermöglichen, die zurückgegebenen Suchergebnisse weiter zu filtern.

Weitere Informationen finden Sie in der [uisearchcontroller-Klassenreferenz](https://developer.apple.com/library/tvos/documentation/UIKit/Reference/UISearchController/index.html)von Apple.

<a name="Working-with-Text-Fields" />

## <a name="working-with-text-fields"></a>Arbeiten mit Text Feldern

Die einfachste Möglichkeit, mit Text Feldern in einer xamarin. tvos-APP zu arbeiten, besteht darin, Sie mithilfe des IOS-Designers dem Design der Benutzeroberfläche hinzuzufügen.

Führen Sie folgende Schritte aus:

<!-- markdownlint-disable MD001 -->

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

1. Doppelklicken Sie im **Lösungspad**auf die `Main.storyboard` Datei, um Sie zur Bearbeitung zu öffnen.
1. Ziehen Sie ein oder mehrere **Text Felder** auf der Entwurfs Oberfläche auf eine Ansicht:

    [![](text-fields-and-search-images/text02.png "A Text Field")](text-fields-and-search-images/text02.png#lightbox)
1. Wählen Sie die **Text Felder** aus, und versehen Sie jeden eindeutigen **Namen** auf der Registerkarte **Widget** der **Eigenschaftenpad**:

    [![](text-fields-and-search-images/text03.png "The Widget tab of the Properties Pad")](text-fields-and-search-images/text03.png#lightbox)
1. Im **Text Feld** Abschnitt können Sie Elemente wie den **Platzhalter** Hinweis und den Standard **Wert**definieren:

    [![](text-fields-and-search-images/text04.png "The Text Field section")](text-fields-and-search-images/text04.png#lightbox)
1. Scrollen Sie nach **unten, um** Eigenschaften wie **Rechtschreibprüfung**, Groß-/Kleinschreibung und den Standard **Tastatur Typ**zu definieren:

    [![](text-fields-and-search-images/text05.png "Spell Checking, Capitalization and the default Keyboard Type")](text-fields-and-search-images/text05.png#lightbox)
1. Speichern Sie die Änderungen in Ihrem Storyboard.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Doppelklicken Sie im **Projektmappen-Explorer** auf die Datei `Main.storyboard`, um sie zur Bearbeitung zu öffnen.
1. Ziehen Sie ein oder mehrere **Text Felder** auf der Entwurfs Oberfläche auf eine Ansicht:

    [![](text-fields-and-search-images/text02-vs.png "A Text Field")](text-fields-and-search-images/text02-vs.png#lightbox)
1. Wählen Sie die **Text Felder** aus, und legen Sie jedem im **Eigenschaften-Explorer**auf der Registerkarte **Widget** einen eindeutigen **Namen** zu:

    [![](text-fields-and-search-images/text03-vs.png "The Widget tab")](text-fields-and-search-images/text03-vs.png#lightbox)
1. Im **Text Feld** Abschnitt können Sie Elemente wie den **Platzhalter** Hinweis und den Standard **Wert**definieren:

    [![](text-fields-and-search-images/text04-vs.png "The Text Field section")](text-fields-and-search-images/text04-vs.png#lightbox)
1. Scrollen Sie nach **unten, um** Eigenschaften wie **Rechtschreibprüfung**, Groß-/Kleinschreibung und den Standard **Tastatur Typ**zu definieren:

    [![](text-fields-and-search-images/text05-vs.png "Spell Checking, Capitalization and the default Keyboard Type")](text-fields-and-search-images/text05-vs.png#lightbox)
1. Speichern Sie die Änderungen in Ihrem Storyboard.

-----

Im Code können Sie den Wert eines Textfelds mit der `Text`-Eigenschaft erhalten oder festlegen:

```csharp
Console.WriteLine ("User ID {0} and Password {1}", UserId.Text, Password.Text);
```

Sie können optional die `Started`-und `Ended` Text Feld Ereignisse verwenden, um auf den Beginn und das Beenden des Text Eintrags zu reagieren.

<a name="Working-with-Search-Fields" />

## <a name="working-with-search-fields"></a>Arbeiten mit Suchfeldern

Die einfachste Möglichkeit zum Arbeiten mit Suchfeldern in einer xamarin. tvos-App besteht darin, Sie mit dem Benutzeroberflächen-Designer dem Design der Benutzeroberfläche hinzuzufügen.

Führen Sie folgende Schritte aus:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

1. Doppelklicken Sie im **Lösungspad**auf die `Main.storyboard` Datei, um Sie zur Bearbeitung zu öffnen.
1. Ziehen Sie einen neuen Sammlungs Ansichts Controller in das Storyboard, um die Ergebnisse der Benutzersuche anzuzeigen:

    [![](text-fields-and-search-images/search02.png "A Collection View Controller")](text-fields-and-search-images/search02.png#lightbox)
1. Verwenden Sie auf der Registerkarte **Widget** des **Eigenschaftenpad**`SearchResultsViewController` für die- **Klasse** und `SearchResults` für die **Storyboard-ID**:

    [![](text-fields-and-search-images/search03.png "The Widget tab")](text-fields-and-search-images/search03.png#lightbox)
1. Wählen Sie auf der Entwurfs Oberfläche den **Zellen Prototyp** aus.
1. Verwenden Sie im **Eigenschaften-Explorer**auf der Registerkarte **Widget** `SearchResultCell` für die- **Klasse** und `ImageCell` für den **Bezeichner**:

    [![](text-fields-and-search-images/search04.png "The Widget tab")](text-fields-and-search-images/search04.png#lightbox)
1. Layout des Entwurfs des **Zellen Prototyps** und verfügbar machen jedes Elements mit einem **eindeutigen Namen** auf der Registerkarte " **Widget** " des **Eigenschaften-Explorers**:

    [![](text-fields-and-search-images/search05.png "Layout the design of the Cell Prototype")](text-fields-and-search-images/search05.png#lightbox)
1. Speichern Sie die Änderungen in Ihrem Storyboard.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Doppelklicken Sie im **Projektmappen-Explorer** auf die Datei `Main.storyboard`, um sie zur Bearbeitung zu öffnen.
1. Ziehen Sie einen neuen Sammlungs Ansichts Controller in das Storyboard, um die Ergebnisse der Benutzersuche anzuzeigen:

    [![](text-fields-and-search-images/seach02-vs.png "A Collection View Controller")](text-fields-and-search-images/seach02-vs.png#lightbox)
1. Verwenden Sie im **Eigenschaften-Explorer**auf der Registerkarte **Widget** `SearchResultsViewController` für die- **Klasse** und `SearchResults` für die **Storyboard-ID**:

    [![](text-fields-and-search-images/search03-vs.png "The Widget tab")](text-fields-and-search-images/search03-vs.png#lightbox)
1. Wählen Sie auf der Entwurfs Oberfläche den **Zellen Prototyp** aus.
1. Verwenden Sie im **Eigenschaften-Explorer**auf der Registerkarte **Widget** `SearchResultCell` für die- **Klasse** und `ImageCell` für den **Bezeichner**:

    [![](text-fields-and-search-images/search04-vs.png "The Widget tab")](text-fields-and-search-images/search04-vs.png#lightbox)
1. Layout des Entwurfs des **Zellen Prototyps** und verfügbar machen jedes Elements mit einem **eindeutigen Namen** auf der Registerkarte " **Widget** " des **Eigenschaften-Explorers**:

    [![](text-fields-and-search-images/search05-vs.png "Layout the design of the Cell Prototype")](text-fields-and-search-images/search05-vs.png#lightbox)
1. Speichern Sie die Änderungen in Ihrem Storyboard.

-----

<a name="Provide-a-Data-Model" />

### <a name="provide-a-data-model"></a>Bereitstellen eines Datenmodells

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

Als nächstes müssen Sie eine Klasse bereitstellen, die als Datenmodell für die Ergebnisse fungiert, nach denen der Benutzer suchen wird. Klicken Sie **im Projektmappen-Explorer**mit der rechten Maustaste auf den Projektnamen, und wählen Sie > **neue Datei** **Hinzufügen** ... > **Allgemein** > **leere Klasse** aus, und geben Sie einen **Namen**an:

[![](text-fields-and-search-images/search06.png "Select Empty Class and provide a Name")](text-fields-and-search-images/search06.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Als nächstes müssen Sie eine Klasse bereitstellen, die als Datenmodell für die Ergebnisse fungiert, nach denen der Benutzer suchen wird. Klicken Sie im **Projektmappen-Explorer**mit der rechten Maustaste auf den Projektnamen, und wählen Sie > **Neues Element** **Hinzufügen** ... > **Apple** > **misc** - > **Klasse** aus, und geben Sie einen **Namen**an:

[![](text-fields-and-search-images/search06-vs.png "Select Class and provide a Name")](text-fields-and-search-images/search06-vs.png#lightbox)

-----

Beispielsweise könnte eine APP, die es dem Benutzer ermöglicht, eine Auflistung von Bildern nach Titel und Schlüsselwort zu durchsuchen, wie folgt aussehen:

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

### <a name="the-collection-view-cell"></a>Die Sammlungs Ansichts Zelle

Wenn das Datenmodell vorhanden ist, bearbeiten Sie die **prototypenzelle** (`SearchResultViewCell.cs`), und machen Sie Folgendes aussehen:

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

Die `UpdateUI`-Methode wird verwendet, um jedes Mal, wenn die-Eigenschaft aktualisiert wird, einzelne Felder der **pictureinformation** -Elemente (die `PictureInfo`-Eigenschaft) in den benannten Benutzeroberflächen Elementen anzuzeigen. Beispielsweise das Bild und der Titel, die mit der Abbildung verknüpft sind.

<a name="The-Collection-View-Controller" />

### <a name="the-collection-view-controller"></a>Der Sammlungs Ansichts Controller

Bearbeiten Sie als nächstes den Ansichts Controller für die Suchergebnisse (`SearchResultsViewController.cs`), und legen Sie ihn wie folgt an:

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

Zuerst wird die `IUISearchResultsUpdating`-Schnittstelle zur-Klasse hinzugefügt, um den vom Benutzer aktualisierten Such Controller Filter zu verarbeiten:

```csharp
public partial class SearchResultsViewController : UICollectionViewController , IUISearchResultsUpdating
```

Außerdem wird eine Konstante definiert, um die ID der **prototypzelle** anzugeben (die mit der ID übereinstimmt, die im obigen Schnittstellen Designer definiert wurde), die später verwendet wird, wenn der Auflistungs Controller eine neue Zelle anfordert:

```csharp
public const string CellID = "ImageCell";
```

Der Speicher wird für die vollständige Liste der gesuchten Elemente, den Suchfilter Begriff und eine Liste von Elementen erstellt, die mit diesem Begriff übereinstimmen:

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

Wenn die `SearchFilter` geändert wird, wird die Liste der übereinstimmenden Elemente aktualisiert, und der Inhalt der Sammlungsansicht wird erneut geladen. Die `FindPictures` Routine ist für die Suche nach Elementen zuständig, die dem neuen Suchbegriff entsprechen:

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

Der Wert des `SearchFilter` wird aktualisiert (wodurch die Ergebnis Sammlungsansicht aktualisiert wird), wenn der Benutzer den Filter im Such Controller ändert:

```csharp
public void UpdateSearchResultsForSearchController (UISearchController searchController)
{
    // Save the search filter and update the Collection View
    SearchFilter = searchController.SearchBar.Text ?? string.Empty;
}
```

Die `PopulatePictures`-Methode füllt anfänglich die Auflistung verfügbarer Elemente auf:

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

Für dieses Beispiel werden alle Beispiel Daten im Arbeitsspeicher erstellt, wenn der Sammlungs Ansichts Controller geladen wird. In einer echten APP würden diese Daten wahrscheinlich aus einer Datenbank oder einem Webdienst gelesen werden, und zwar nur nach Bedarf, um den begrenzten Arbeitsspeicher von Apple TV zu überschreiten.

Die Methoden `NumberOfSections` und `GetItemsCount` geben die Anzahl der übereinstimmenden Elemente an:

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

Die `GetCell`-Methode gibt für jedes Element in der Auflistungs Ansicht eine neue **prototypzelle** (basierend auf der oben im Storyboard definierten `CellID`) zurück:

```csharp
public override UICollectionViewCell GetCell (UICollectionView collectionView, NSIndexPath indexPath)
{
    // Get a new cell and return it
    var cell = collectionView.DequeueReusableCell (CellID, indexPath);
    return (UICollectionViewCell)cell;
}
```

Die `WillDisplayCell`-Methode wird aufgerufen, bevor die Zelle angezeigt wird, sodass Sie konfiguriert werden kann:

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

Die `DidUpdateFocus`-Methode bietet dem Benutzer visuelles Feedback, wenn Sie Elemente in der Ergebnis Sammlungsansicht hervorheben:

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

Zum Schluss behandelt die `ItemSelected`-Methode den Benutzer, der in der Ergebnis Auflistungs Ansicht ein Element (auf die Berührungs Oberfläche mit der Siri-Remote Maustaste klickt) auswählt:

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

Wenn das Suchfeld als modale Dialogfeld Ansicht angezeigt wurde (über dem oberen Rand der aufrufenden Ansicht), verwenden Sie die `DismissViewController`-Methode, um die Such Ansicht zu schließen, wenn der Benutzer ein Element auswählt. In diesem Beispiel wird das Suchfeld als Inhalt einer Registerkarte mit Registerkarten Ansicht angezeigt, sodass es hier nicht verworfen wird.

Weitere Informationen zu Sammlungs Ansichten finden Sie in unserer Dokumentation zum [Arbeiten mit Sammlungs Ansichten](~/ios/tvos/user-interface/collection-views.md) .

<a name="Presenting the Search Field" />

### <a name="presenting-the-search-field"></a>Präsentieren des Suchfelds

Es gibt zwei Hauptmethoden, mit denen ein Suchfeld (und die zugehörigen Bildschirmtastatur und Suchergebnisse) dem Benutzer in tvos angezeigt werden können:

- **Modale Dialogfeld Ansicht** : das Suchfeld kann über den aktuellen Ansichts-und Ansichts Controller als Vollbild-modale Dialogfeld Ansicht dargestellt werden. Dies erfolgt in der Regel als Reaktion darauf, dass der Benutzer auf eine Schaltfläche oder ein anderes UI-Element klickt. Das Dialogfeld wird verworfen, wenn der Benutzer ein Element aus den Suchergebnissen auswählt.
- **Inhalt anzeigen** : das Suchfeld ist ein direkter Teil einer angegebenen Ansicht. Beispielsweise als Inhalt einer Registerkarte Suchen in einem Registerkarten Ansichts Controller.

Im Beispiel für eine durchsuchbare Liste mit Bildern, die oben angegeben ist, wird das Suchfeld als Inhalt anzeigen auf der Registerkarte Suchen angezeigt, und der Such Controller der Registerkarte Suche sieht wie folgt aus:

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

Zuerst wird eine Konstante definiert, die mit der **Storyboard** -ID übereinstimmt, die dem Sammlungs Ansichts Controller der Suchergebnisse im Schnittstellen-Designer zugewiesen wurde:

```csharp
public const string SearchResultsID = "SearchResults";
```

Als Nächstes erstellt die `ShowSearchController`-Methode einen neuen suchansichts-Sammlungs Controller und zeigt an, dass Sie benötigt wurde:

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

Wenn ein `SearchResultsViewController` in der obigen Methode aus dem Storyboard instanziiert wurde, wird eine neue `UISearchController` erstellt, um dem Benutzer das Suchfeld und die Bildschirmtastatur anzuzeigen. Die Suchergebnis Auflistung (gemäß der Definition durch die `SearchResultsViewController`) wird unter dieser Tastatur angezeigt.

Als nächstes wird der `SearchBar` mit Informationen wie dem **Platzhalter** Hinweis konfiguriert. Dadurch werden dem Benutzerinformationen über den Typ der vorformatierten Suche bereitgestellt.

Anschließend wird dem Benutzer das Suchfeld auf zwei Arten angezeigt:

- **Modale Dialog Feld Ansicht** : die `PresentViewController`-Methode wird aufgerufen, um die Suche über die vorhandene Ansicht (Vollbild) darzustellen.
- **Inhalt anzeigen** : Es wird eine `UISearchContainerViewController` erstellt, die den Such Controller enthält. Es wird eine `UINavigationController` erstellt, die den Such Container enthält. Anschließend wird der Navigations Controller dem Ansichts Controller `AddChildViewController (navController)`hinzugefügt, und die Ansicht wird `View.Add (navController.View)`dargestellt.

Zum Schluss und wieder, basierend auf dem Präsentationstyp, ruft die `ViewDidLoad` oder `ViewDidAppear` Methode die `ShowSearchController`-Methode auf, um dem Benutzer die Suche zu präsentieren:

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

Wenn die app ausgeführt wird und die Registerkarte Suchen vom Benutzer ausgewählt wird, wird dem Benutzer die vollständige ungefilterte Liste der Elemente angezeigt:

[![](text-fields-and-search-images/intro02.png "Default search results")](text-fields-and-search-images/intro02.png#lightbox)

Wenn der Benutzer mit der Eingabe eines Suchbegriffs beginnt, wird die Ergebnisliste nach diesem Begriff gefiltert und automatisch aktualisiert:

[![](text-fields-and-search-images/intro03.png "Filtered search results")](text-fields-and-search-images/intro03.png#lightbox)

Der Benutzer kann den Fokus jederzeit auf ein Element in den Suchergebnissen umschalten und auf die Berührungs Oberfläche von Siri Remote klicken, um ihn auszuwählen.

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde das Entwerfen und arbeiten mit Text-und Suchfeldern in einer xamarin. tvos-App behandelt. Es wurde gezeigt, wie Text erstellt und Sammlungsinhalte im Schnittstellen-Designer gesucht werden, und es wurden zwei unterschiedliche Möglichkeiten gezeigt, wie ein Suchfeld dem Benutzer in tvos angezeigt werden konnte.

## <a name="related-links"></a>Verwandte Links

- [tvOS-Beispiele](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [tvOS](https://developer.apple.com/tvos/)
- [tvos Human Interface Guides](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Leitfaden zur APP-Programmierung für tvos](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
