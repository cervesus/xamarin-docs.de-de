---
title: Arbeiten mit Text und Suchfelder
description: Dieser Artikel umfasst das Entwerfen von und Arbeiten mit Text und Suchfelder innerhalb einer Xamarin.tvOS-app.
ms.prod: xamarin
ms.assetid: 9EE63CA6-2F31-4EE0-AAE5-82E18CFAC06C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 220c6e3d1c6f358c67a2f596c977f4d2132298a8
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="working-with-text-and-search-fields"></a>Arbeiten mit Text und Suchfelder

_Dieser Artikel umfasst das Entwerfen von und Arbeiten mit Text und Suchfelder innerhalb einer Xamarin.tvOS-app._



Wenn erforderlich, kann Ihre app Xamarin.tvOS kleine Teile des Texts fordern Sie vom Benutzer (z. B. Benutzer-IDs und Kennwörter) mit einem Text-Feld und die Bildschirmtastatur:

[![](text-fields-and-search-images/intro01.png "Beispiel-suchen (Feld)")](text-fields-and-search-images/intro01.png#lightbox)

Sie können optional Schlüsselwort Suche Fähigkeit der app-Inhalte, die mit einem Feld Suche bereitstellen:

[![](text-fields-and-search-images/intro02.png "Beispiel-Suchergebnisse")](text-fields-and-search-images/intro02.png#lightbox)

Dieses Dokument werden die Informationen zum Arbeiten mit Text und Suchfelder in einer app Xamarin.tvOS behandelt.

<a name="About-Text-and-Search-Fields" />

## <a name="about-text-and-search-fields"></a>Informationen zu Text und Suchfelder

Wie oben angegeben, falls erforderlich, kann Ihre Xamarin.tvOS stellen ein oder mehrere Textfelder zum Sammeln von kleiner Mengen von Text aus dem Benutzer mithilfe einer auf dem Bildschirm angezeigt (oder optional Bluetooth-Tastatur, je nach Version tvos. außerdem hat der Benutzer installiert wurden). 

Wenn Ihre app große Mengen von Inhalten für den Benutzer (z. B. eine Musik, Videos und eine Bild-Auflistung) enthält, sollten Sie darüber hinaus eine Search-Feld enthalten, die dem Benutzer ermöglicht, geben Sie eine kleine Menge von Text in die Liste der verfügbaren Elemente zu filtern.

<a name="Text-Fields" />

## <a name="text-fields"></a>Textfelder

Tvos. außerdem wurden, ein Textfeld wird ein fester Höhe, abgerundete Ecke Eintrag bringen wird unbearbeitet eine Bildschirmtastatur, wenn der Benutzer darauf klickt:

[![](text-fields-and-search-images/text01.png "Text Felder In tvos. außerdem wurden")](text-fields-and-search-images/text01.png#lightbox)

Wenn der Benutzer wechselt [Fokus](~/ios/tvos/app-fundamentals/navigation-focus.md) zu einem bestimmten Text-Feld wird anwachsen und einen tiefen Schatten angezeigt. Sie müssen dies Bedenken beim Entwerfen der Benutzeroberfläche, wie Textfelder, andere Elemente der Benutzeroberfläche, wenn im Fokus überlappen können.

Apple hat die folgenden Vorschläge zum Arbeiten mit Textfeldern an:

- **Text Eintrag sparsam verwenden** – aufgrund der Natur des der Bildschirmtastatur, längere Abschnitte des Texts eingeben oder mehrere Textfelder ausfüllen ist mühsam für den Benutzer. Eine bessere Lösung besteht darin, die Texteingabe begrenzen mithilfe von Auswahllisten oder [Schaltflächen](~/ios/tvos/user-interface/buttons.md).
- **Verwenden der Hinweise zum Zweck kommunizieren** -Textfeld kann Platzhalter "Hints" angezeigt, wenn Sie leer. Falls zutreffend, verwenden Sie Hinweise, um den Zweck des Textfelds anstelle einer separaten Bezeichnung beschreiben.
- **Wählen Sie den entsprechenden Standard Tastatur** -tvos. außerdem wurden mehrere, speziell Tastatur stellt verschiedene Typen bereit, die Sie für das Feld Text angeben können. Beispielsweise kann der e-Mail-Adresse Tastatur Eintrag vereinfachen, indem der Benutzer aus einer Liste der zuletzt erfassten Adressen auswählen.
- **Wenn dies angebracht ist, verwenden Sie Textfelder Secure** -ein Secure Textfeld zeigt die Zeichen eingegeben als Punkte (statt der tatsächlichen Buchstaben). Verwenden Sie immer ein Textfeld Secure auf, wenn vertraulichen Informationen wie Kennwörter zu sammeln.

<a name="Keyboards" />

## <a name="keyboards"></a>Tastaturen

Tastatur wird angezeigt, sobald der Benutzer auf dem Bildschirm auf ein Textfeld in der Benutzeroberfläche, ein lineares klickt. Der Benutzer verwendet die Oberfläche berühren der [Siri Remote](~/ios/tvos/platform/remote-bluetooth.md#The-Siri-Remote) Briefe über die Tastatur auswählen, und geben die angeforderte Informationen:

[![](text-fields-and-search-images/keyboard01.png "Der Siri Remote-Tastatur")](text-fields-and-search-images/keyboard01.png#lightbox)

Wenn mehr als ein Textfeld auf die aktuelle Ansicht eine **Weiter** Schaltfläche angezeigt wird, automatisch um die Benutzer in das nächste Feld für den Text zu übernehmen. Ein **Fertig** Schaltfläche wird angezeigt, für das letzte Text-Feld, die Texteingabe zu beenden und den Benutzer zum vorherigen Bildschirm zurück. 

Der Benutzer kann jederzeit, auch Drücken der **Menü** Schaltfläche auf der Remoteinstanz Siri Texteingabe zu beenden und wieder zum vorherigen Bildschirm zurück.

Apple hat die folgenden Vorschläge für die Arbeit mit auf dem Bildschirm Tastaturen:

- **Wählen Sie den entsprechenden Standard Tastatur** -tvos. außerdem wurden mehrere, speziell Tastatur stellt verschiedene Typen bereit, die Sie für das Feld Text angeben können. Beispielsweise kann der e-Mail-Adresse Tastatur Eintrag vereinfachen, indem der Benutzer aus einer Liste der zuletzt erfassten Adressen auswählen.
- **Wenn dies angebracht ist, verwenden Sie Tastatur Zubehör Ansichten** : Zusätzlich zu den Standard Informationen, die immer angezeigt werden, optional Zubehör-Ansichten (z. B. Bilder oder Bezeichnungen) hinzugefügt werden kann die Bildschirmtastatur Erläuterung des Zwecks des Texteingabe bzw. unterstützen des Benutzers die erforderlichen Informationen eingegeben.

Weitere Informationen zum Arbeiten mit der Bildschirmtastatur, finden Sie in der Apple- [UIKeyboardType](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITextInputTraits_Protocol/index.html#//apple_ref/c/tdef/UIKeyboardType), [Verwalten der Tastatur](https://developer.apple.com/library/tvos/documentation/StringsTextFonts/Conceptual/TextAndWebiPhoneOS/KeyboardManagement/KeyboardManagement.html#//apple_ref/doc/uid/TP40009542-CH5-SW1), [benutzerdefinierte Ansichten für die Dateneingabe](https://developer.apple.com/library/tvos/documentation/StringsTextFonts/Conceptual/TextAndWebiPhoneOS/InputViews/InputViews.html#//apple_ref/doc/uid/TP40009542-CH12-SW1) und [ Text-Programmierhandbuch für iOS-](https://developer.apple.com/library/tvos/documentation/StringsTextFonts/Conceptual/TextAndWebiPhoneOS/Introduction/Introduction.html) Dokumentation.

<a name="Search" />

## <a name="search"></a>Suchen

Ein Suchfeld einen speziellen Bildschirm ein Textfeld bereitstellen präsentieren und Bildschirmtastatur, die ermöglicht dem Benutzer, die eine Auflistung von Elementen zu filtern, die unterhalb der Tastatur angezeigt werden:

[![](text-fields-and-search-images/search01.png "Beispiel-Suchergebnisse")](text-fields-and-search-images/search01.png#lightbox)

Wie der Benutzer in das Suchfeld Buchstaben eingibt, werden die folgenden Ergebnisse automatisch die Ergebnisse der Suche wiedergeben. Der Benutzer kann jederzeit verschieben den Fokus auf die Ergebnisse und wählen Sie eines der Elemente angezeigt.

Apple hat die folgenden Vorschläge für die Arbeit mit Suchfeldern:

- **Geben Sie die letzte Suchvorgänge** : Da eingeben von Text mit der Remoteinstanz Siri kann zeitraubend sein, und Benutzer sind tendenziell wiederholen die Search-Anforderungen, erwägen einen Abschnitt der aktuelle Suchergebnisse vor der aktuellen Ergebnisse unterhalb des Bereichs der Tastatur.
- **Wenn möglich, begrenzen Sie die Anzahl der Ergebnisse** : Da eine langen Liste von Elementen für den Benutzer zu analysieren und zu navigieren, können die Anzahl der zurückgegebenen Ergebnisse begrenzen schwierig sein kann.
- **Falls zutreffend, geben Sie Suche Ergebnis filtert** : Falls von Ihrer Anwendung bereitgestellte Inhalte, geradezu erwägen Bereich Balken, damit der Benutzer aus, um die Suchergebnisse weiter zu filtern kann.

Weitere Informationen finden Sie in der Apple- [UISearchController Class Reference](https://developer.apple.com/library/tvos/documentation/UIKit/Reference/UISearchController/index.html).

<a name="Working-with-Text-Fields" />

## <a name="working-with-text-fields"></a>Arbeiten mit Textfeldern

Die einfachste Möglichkeit zum Arbeiten mit Textfeldern in einer app Xamarin.tvOS werden diese mit der iOS-Designer auf die Benutzeroberfläche hinzufügen.

Führen Sie folgende Schritte aus:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

1. In der **Lösung Pad**, doppelklicken Sie auf die `Main.storyboard` Datei zur Bearbeitung zu öffnen.
1. Ziehen Sie eine oder mehrere **Textfelder** Int der Entwurfsoberfläche auf eine Sicht: 

    [![](text-fields-and-search-images/text02.png "Ein Textfeld")](text-fields-and-search-images/text02.png#lightbox)
1. Wählen Sie die **Textfelder** , und geben Sie jeweils eine eindeutige **Namen** in der **Widget** auf der Registerkarte die **Eigenschaften Pad**: 

    [![](text-fields-and-search-images/text03.png "Der Registerkarte "Widget" die Eigenschaften mit Leerstellen auffüllen")](text-fields-and-search-images/text03.png#lightbox)
1. In der **Textfeld** Abschnitt definieren Sie Elemente wie z. B. die **Platzhalter** -Hinweis und Standard **Wert**: 

    [![](text-fields-and-search-images/text04.png "Textfeld "im Abschnitt")](text-fields-and-search-images/text04.png#lightbox)
1. Einen Bildlauf nach unten, um Eigenschaften zu definieren, wie z. B. **Rechtschreibprüfung**, **Großschreibung** und der standardmäßige **Tastatur**: 

    [![](text-fields-and-search-images/text05.png "Rechtschreibprüfung, Groß-/Kleinschreibung und den Standardtyp der Tastatur")](text-fields-and-search-images/text05.png#lightbox) 
1. Speichern Sie die Änderungen auf das Storyboard.
    
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)
    
1. Doppelklicken Sie im **Projektmappen-Explorer** auf die Datei `Main.storyboard`, um sie zur Bearbeitung zu öffnen.
1. Ziehen Sie eine oder mehrere **Textfelder** Int der Entwurfsoberfläche auf eine Sicht: 

    [![](text-fields-and-search-images/text02-vs.png "Ein Textfeld")](text-fields-and-search-images/text02-vs.png#lightbox)
1. Wählen Sie die **Textfelder** , und geben Sie jeweils eine eindeutige **Namen** in der **Widget** auf der Registerkarte die **Eigenschaften-Explorer**: 

    [![](text-fields-and-search-images/text03-vs.png "Die Registerkarte "Widget"")](text-fields-and-search-images/text03-vs.png#lightbox)
1. In der **Textfeld** Abschnitt definieren Sie Elemente wie z. B. die **Platzhalter** -Hinweis und Standard **Wert**: 

    [![](text-fields-and-search-images/text04-vs.png "Textfeld "im Abschnitt")](text-fields-and-search-images/text04-vs.png#lightbox)
1. Einen Bildlauf nach unten, um Eigenschaften zu definieren, wie z. B. **Rechtschreibprüfung**, **Großschreibung** und der standardmäßige **Tastatur**: 

    [![](text-fields-and-search-images/text05-vs.png "Rechtschreibprüfung, Groß-/Kleinschreibung und den Standardtyp der Tastatur")](text-fields-and-search-images/text05-vs.png#lightbox) 
1. Speichern Sie die Änderungen auf das Storyboard.
    
-----

Im Code können Sie abrufen oder festlegen den Wert eines Textfelds mithilfe seiner `Text` Eigenschaft:

```csharp
Console.WriteLine ("User ID {0} and Password {1}", UserId.Text, Password.Text);
```

Optional können Sie die `Started` und `Ended` Textfeld Ereignisse so reagieren Sie auf die Texteingabe beginnt und endet.

<a name="Working-with-Search-Fields" />

## <a name="working-with-search-fields"></a>Arbeiten mit Suchfeldern

Die einfachste Möglichkeit zur Bearbeitung der Suchfelder in einer app Xamarin.tvOS werden diese mit dem Benutzeroberflächen-Designer auf die Benutzeroberfläche hinzufügen.

Führen Sie folgende Schritte aus:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)
    
1. In der **Lösung Pad**, doppelklicken Sie auf die `Main.storyboard` Datei zur Bearbeitung zu öffnen.
1. Ziehen Sie eine neue Auflistung-View-Controller aus, auf das Storyboard, um die Ergebnisse der Suche des Benutzers vorhanden: 

    [![](text-fields-and-search-images/search02.png "Eine Auflistung-View-Controller")](text-fields-and-search-images/search02.png#lightbox)
1. In der **Widget** auf der Registerkarte die **Eigenschaften Pad**, verwenden `SearchResultsViewController` für die **Klasse** und `SearchResults` für die **Storyboard-ID**: 

    [![](text-fields-and-search-images/search03.png "Die Registerkarte "Widget"")](text-fields-and-search-images/search03.png#lightbox)
1. Wählen Sie die **Zelle Prototyp** auf der Entwurfsoberfläche angezeigt.
1. In der **Widget** auf der Registerkarte die **Eigenschaften-Explorer**, verwenden Sie `SearchResultCell` für die **Klasse** und `ImageCell` für die **Bezeichner**: 

    [![](text-fields-and-search-images/search04.png "Die Registerkarte "Widget"")](text-fields-and-search-images/search04.png#lightbox)
1. Layout des Entwurfs von der **Zelle Prototyp** und Verfügbarmachen von jedem Element mit einer eindeutigen **Namen** in der **Widget** auf der Registerkarte die **Eigenschaften-Explorer**: 

    [![](text-fields-and-search-images/search05.png "Layout der Entwurf des Prototyps Zelle")](text-fields-and-search-images/search05.png#lightbox)
1. Speichern Sie die Änderungen auf das Storyboard.
    
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)
    
1. Doppelklicken Sie im **Projektmappen-Explorer** auf die Datei `Main.storyboard`, um sie zur Bearbeitung zu öffnen.
1. Ziehen Sie eine neue Auflistung-View-Controller aus, auf das Storyboard, um die Ergebnisse der Suche des Benutzers vorhanden: 

    [![](text-fields-and-search-images/seach02-vs.png "Eine Auflistung-View-Controller")](text-fields-and-search-images/seach02-vs.png#lightbox)
1. In der **Widget** auf der Registerkarte die **Eigenschaften-Explorer**, verwenden Sie `SearchResultsViewController` für die **Klasse** und `SearchResults` für die **Storyboard-ID**: 

    [![](text-fields-and-search-images/search03-vs.png "Die Registerkarte "Widget"")](text-fields-and-search-images/search03-vs.png#lightbox)
1. Wählen Sie die **Zelle Prototyp** auf der Entwurfsoberfläche angezeigt.
1. In der **Widget** auf der Registerkarte die **Eigenschaften-Explorer**, verwenden Sie `SearchResultCell` für die **Klasse** und `ImageCell` für die **Bezeichner**: 

    [![](text-fields-and-search-images/search04-vs.png "Die Registerkarte "Widget"")](text-fields-and-search-images/search04-vs.png#lightbox)
1. Layout des Entwurfs von der **Zelle Prototyp** und Verfügbarmachen von jedem Element mit einer eindeutigen **Namen** in der **Widget** auf der Registerkarte die **Eigenschaften-Explorer**: 

    [![](text-fields-and-search-images/search05-vs.png "Layout der Entwurf des Prototyps Zelle")](text-fields-and-search-images/search05-vs.png#lightbox)
1. Speichern Sie die Änderungen auf das Storyboard.
    
-----

<a name="Provide-a-Data-Model" />

### <a name="provide-a-data-model"></a>Geben Sie ein Datenmodell

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Als Nächstes müssen Sie eine Klasse angegeben, als das Datenmodell für die Ergebnisse zu fungieren, die der Benutzer gesucht wird. In der **Projektmappen-Explorer**mit der rechten Maustaste auf den Projektnamen, und wählen Sie **hinzufügen** > **neue Datei...**   >  **Allgemeine** > **leere Klasse** , und geben Sie einen **Namen**: 

[![](text-fields-and-search-images/search06.png "Wählen Sie die leere Klasse ist, und geben Sie einen Namen")](text-fields-and-search-images/search06.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Als Nächstes müssen Sie eine Klasse angegeben, als das Datenmodell für die Ergebnisse zu fungieren, die der Benutzer gesucht wird. In der **Projektmappen-Explorer**mit der rechten Maustaste auf den Projektnamen, und wählen Sie **hinzufügen** > **neues Element...**   >  **Apple** > **Verschiedenes** > **Klasse** , und geben Sie einen **Namen**: 

[![](text-fields-and-search-images/search06-vs.png "Wählen Sie Klasse aus, und geben Sie einen Namen")](text-fields-and-search-images/search06-vs.png#lightbox)

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

### <a name="the-collection-view-cell"></a>Die Auflistung Ansicht Zelle

Bearbeiten Sie mit dem Datenmodell vorhanden, die **Prototyp Zelle** (`SearchResultViewCell.cs`) und stellen es aussehen liegen die folgenden:

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

Die `UpdateUI` Methode wird verwendet, um die Anzeige der einzelnen Felder von der **PictureInformation** Elemente (die `PictureInfo` Eigenschaft) in die benannte Benutzeroberflächenelemente jedes Mal die Eigenschaft wird aktualisiert. Beispielsweise das Bild und Titel, die dem Bild zugeordnet.

<a name="The-Collection-View-Controller" />

### <a name="the-collection-view-controller"></a>Die Auflistung-View-Controller

Als Nächstes bearbeiten Sie die Suche Ergebnisse Auflistung-View-Controller (`SearchResultsViewController.cs`), und stellen sie wie folgt aussehen:

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

Zunächst wird die `IUISearchResultsUpdating` -Schnittstelle wird hinzugefügt, um die Klasse zum Behandeln von des Suche Controller-Filters, die vom Benutzer aktualisiert wird:

```csharp
public partial class SearchResultsViewController : UICollectionViewController , IUISearchResultsUpdating
```

Eine Konstante wird auch definiert, um anzugeben, die ID des der **Prototyp Zelle** (, die Übereinstimmung mit der ID in der oben genannten Benutzeroberflächen-Designer definiert), werden später verwendet, wenn der Controller Auflistung eine neue Zelle anfordert:

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

Wenn die `SearchFilter` wird geändert, wird die Liste der übereinstimmenden Elemente aktualisiert und die Auflistungsansicht Inhalt wird erneut geladen. Die `FindPictures` Routine ist verantwortlich für die Suche nach Elementen, die dem neuen Suchbegriff entsprechen:

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

Der Wert des der `SearchFilter` werden aktualisiert (die die Ergebnisse Auflistungsansicht aktualisiert) Wenn der Benutzer den Filter im Controller Suche ändert:

```csharp
public void UpdateSearchResultsForSearchController (UISearchController searchController)
{
    // Save the search filter and update the Collection View
    SearchFilter = searchController.SearchBar.Text ?? string.Empty;
}
```

Die `PopulatePictures` Methode zunächst füllt die Auflistung der verfügbaren Elemente:

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

Für dieses Beispiel alle der Stichprobendaten ist erstellende im Arbeitsspeicher beim Laden der Auflistung-View-Controller. Klicken Sie in eine wirkliche app diese Daten werden aus einer Datenbank oder Web-Dienst wahrscheinlich liest, und nur bei Bedarf beibehalten der Apple-Fernseher dieses Arbeitsspeicher beschränkt.

Die `NumberOfSections` und `GetItemsCount` Methoden bereit, die Anzahl der übereinstimmenden Elemente:

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

Die `GetCell` Methode gibt ein neues **Prototyp Zelle** (basierend auf den `CellID` oben im Storyboard definierten) für jedes Element in der Auflistungsansicht:

```csharp
public override UICollectionViewCell GetCell (UICollectionView collectionView, NSIndexPath indexPath)
{
    // Get a new cell and return it
    var cell = collectionView.DequeueReusableCell (CellID, indexPath);
    return (UICollectionViewCell)cell;
}
```

Die `WillDisplayCell` Methode wird aufgerufen, bevor Sie die Zelle, die angezeigt werden, damit diese konfiguriert werden kann:

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

Die `DidUpdateFocus` -Methode stellt visuelles Feedback für den Benutzer bereit, wie sie Elemente in der Auflistungsansicht Ergebnisse hervorzuheben:

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

Schließlich die `ItemSelected` Methode verarbeitet den Benutzer ein Element (Klicken auf der Oberfläche berühren, mit der Siri Remote) in der Auflistungsansicht Ergebnisse auswählen:

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

Wenn die Search-Feld als ein modales Dialogfeld anzeigen (über den oberen Rand der Ansicht, das Aufrufen dieser), verwenden präsentiert wurde die `DismissViewController` Methode, um der Suchansicht zu schließen, wenn der Benutzer ein Element auswählt. In diesem Beispiel wird der Inhalt einer Registerkarte die Registerkarte Suchen (Feld) als dargestellt, damit es nicht hier verworfen wird.

Weitere Informationen zu Auflistungsansichten, finden Sie unter unsere [arbeiten mit Auflistungsansichten](~/ios/tvos/user-interface/collection-views.md) Dokumentation.

<a name="Presenting the Search Field" />

### <a name="presenting-the-search-field"></a>Das Suchfeld darstellen

Es sind zwei Hauptmethoden, die eine Search-Feld (und die zugehörigen Bildschirmtastatur und die Suchergebnisse) in tvos. außerdem wurden an den Benutzer angezeigt werden können: 

- **Anzeigen der modalen Dialogfeld** -suchen (Feld) das über die aktuelle Ansicht und View-Controller als ein modales Dialogfeld Vollbildansicht angezeigt. Dies erfolgt in der Regel in der Antwort des Benutzers auf eine Schaltfläche oder andere UI-Element. Das Dialogfeld wird geschlossen, wenn der Benutzer ein Element in den Suchergebnissen auswählt.
- **Anzeigen von Inhalt** -die Search-Feld ist eine direkte Teil einer angegebenen Ansicht. Z. B. der Inhalt einer Registerkarte "Suchen" in einer Registerkarte-View-Controller.

Klicken Sie beispielsweise eine durchsuchbare Liste mit Bildern, die oben genannte Search-Feld als ' Inhalt anzeigen ', in der Registerkarte "Suchen" dargestellt wird und Suche Registerkarte-View-Controller sieht folgendermaßen aus:

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

Zunächst eine Konstante ist definiert, die entspricht der **Storyboard-Bezeichner** , die mit dem Controller Auflistungsansicht der Suchergebnisse in der Benutzeroberflächen-Designer zugewiesen wurde:

```csharp
public const string SearchResultsID = "SearchResults";
```

Als Nächstes wird die `ShowSearchController` -Methode erstellt eine neue Suche Auflistung Modellansichtcontroller und zeigt es war erforderlich:

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

In der oben genannten Methode einmal eine `SearchResultsViewController` aus dem Storyboard instanziiert wurde ein neues `UISearchController` wird erstellt, um das Suchfeld darstellen und Bildschirmtastatur für den Benutzer. Die Suchergebnisse-Auflistung (gemäß der `SearchResultsViewController`) wird unter diesem Tastatur angezeigt werden.

Als Nächstes wird die `SearchBar` wird mit Informationen wie z. B. konfiguriert die **Platzhalter** Hinweis. Dies stellt Informationen bereit, um den Benutzer über den Typ der Suche durchgeführt wird.

Klicken Sie dann erhält die Search-Feld für den Benutzer auf zwei Arten:

- **Modales Dialogfeld anzeigen** : die `PresentViewController` Methode wird aufgerufen, um die Suche über die vorhandene Sicht zu präsentieren Vollbildmodus.
- **Anzeigen von Inhalt** – ein `UISearchContainerViewController` wird erstellt, um die Suche Controller enthalten. Ein `UINavigationController` wird erstellt, um den Container Suche enthalten den Controller für die Navigation, wird die View-Controller hinzugefügt `AddChildViewController (navController)`, und die Ansicht präsentiert `View.Add (navController.View)`.

Schließlich und erneut basierend auf den Typ Präsentation entweder die `ViewDidLoad` oder `ViewDidAppear` -Methode aufrufen, wird die `ShowSearchController` Methode, um die Suche für den Benutzer anzuzeigen:

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

Wenn die app ausgeführt wird und der Registerkarte "Suchen" vom Benutzer ausgewählt ist, wird die vollständige ungefilterte Liste von Elementen, die dem Benutzer angezeigt:

[![](text-fields-and-search-images/intro02.png "Standard-Suchergebnisse")](text-fields-and-search-images/intro02.png#lightbox)

Wie der Benutzer beginnt, einen Suchbegriff eingeben, wird die Liste der Ergebnisse nach dieser Begriff gefiltert und automatisch aktualisiert werden:

[![](text-fields-and-search-images/intro03.png "Gefilterter Suchergebnissen")](text-fields-and-search-images/intro03.png#lightbox)

Der Benutzer kann jederzeit wechseln den Fokus auf ein Element in den Suchergebnissen und klicken Sie auf der Oberfläche berühren der Remoteinstanz Siri, um es auszuwählen.

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden behandelt, Entwerfen und Arbeiten mit Text und Suchfelder innerhalb einer Xamarin.tvOS-app. Es wurde gezeigt, wie Text und Sammlung Durchsuchen von Inhalt in der Benutzeroberflächen-Designer erstellen und es angezeigt werden. um zwei verschiedene Möglichkeiten, die dem Benutzer im tvos. außerdem wurden ein Suchfeld angezeigt werden konnte.



## <a name="related-links"></a>Verwandte Links

- [tvOS-Beispiele](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvos. außerdem wurden Handbücher für interaktive Workflowdienste-Schnittstelle](https://developer.apple.com/tvos/human-interface-guidelines/)
- [App-Programmierhandbuch für tvos. außerdem wurden](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
