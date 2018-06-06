---
title: Suchen Sie Balken in Xamarin.iOS
description: Dieses Dokument beschreibt, wie Suche Balken in Xamarin.iOS verwendet wird. Es wird erläutert, wie Suche Balken programmgesteuert und in Storyboards erstellen.
ms.prod: xamarin
ms.assetid: 22A8249A-19C6-4734-8331-E49FE3170771
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 07/11/2017
ms.openlocfilehash: cd78c58ecb119c437296a0befe1d319d8837edae
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34789924"
---
# <a name="search-bars-in-xamarinios"></a>Suchen Sie Balken in Xamarin.iOS

Die UISearchBar wird verwendet, um eine Liste mit Werten durchsuchen. 

Es enthält drei Hauptkomponenten: 

- Ein Feld verwendet, um Text einzugeben. Benutzer können diese Option, um ihre Suchbegriff eingeben nutzen.
- Eine Schaltfläche löschen, so entfernen Sie einen beschreibenden Text in das Suchfeld ein.
- Eine Schaltfläche "Abbrechen", um die Suchfunktion zu beenden.

![Suchleiste](searchbar-images/image1.png)

## <a name="implementing-the-search-bar"></a>Implementieren die Suchleiste

So implementieren Sie Beginn Leiste suchen instanziieren ein neues Konto:

```csharp
searchBar = new UISearchBar();
```

Ein, und legen Sie es. Im folgenden Beispiel wird gezeigt, wie in der Navigationsleiste oder in der HeaderView einer Tabelle platziert wird:

```csharp
NavigationItem.TitleView = searchBar;

\\or

TableView.TableHeaderView = searchBar;
```

Festlegen von Eigenschaften für die Suchleiste:

```csharp
 searchBar = new UISearchBar(){
                Placeholder = "Enter your search Item",
                Prompt = "Search Entered here",
                ShowsScopeBar = true,
                ScopeButtonTitles = new string[]{ "Boston", "London", "SF" },
            };
```

![Suchleiste-Eigenschaften](searchbar-images/image6.png)

Auslösen der `SearchButtonClicked` Ereignis beim Klicken auf die Schaltfläche "Suchen" aufgerufen wird. Dadurch wird die Suchlogik aufgerufen:

```csharp
searchBar.SearchButtonClicked += (sender, e) => {
                Search ();
            };
```

Informationen zum Verwalten der Präsentation der Suchleiste und Suchergebnisse finden Sie unter der [Suche Controller ](https://developer.xamarin.com/recipes/ios/content_controls/search-controller/) Rezept.

## <a name="using-the-search-bar-in-the-designer"></a>Verwenden die Suchleiste im Designer

Der Designer bietet zwei Optionen für das implementieren eine Suchleiste im designer

- Suchleiste
- Suchleiste durch Suche Anzeige Controller (veraltet)

![Statusleiste-Steuerelement im Designer suchen](searchbar-images/image2.png)

Verwenden Sie den Eigenschaftenbereich zum Festlegen von Eigenschaften auf der Suchleiste

![Suche Leiste Eigenschaften-designer](searchbar-images/image3.png)

Diese Eigenschaften werden im folgenden erläutert:

- **Text, Platzhalter Eingabeaufforderung** – diese Eigenschaften dienen zum vorschlagen und anweisen, wie Benutzer die Suchleiste verwenden sollten. Angenommen, wenn Ihre app Stores aufgelistet können die Prompt-Eigenschaft Sie informieren, Benutzern "eine Stadt, die Story Namen oder die Postleitzahl eingeben können"
- **Suchen Sie die Formatvorlage** – Sie können festlegen, dass die Suchleiste müssen **Prominent** oder **minimale**. Mithilfe der deutliche Farbton wird alles auf dem Bildschirm, mit Ausnahme der Suchleiste, verursacht den Fokus, um die Suchleiste gezeichnet werden soll. Die minimale Stil Suchleiste wird sich mit dessen Umgebung blend.
- **Funktionen** – aktivieren diese Eigenschaften nur das Element der Benutzeroberfläche angezeigt. Die Funktionalität muss implementiert werden, für diese Ereignisse auslösen des Ereignisses richtig, wie ausführlich in die [Leiste-Suchdienst-API-Dokumentation](https://developer.xamarin.com/api/type/UIKit.UISearchBar/)
    - Zeigt Suchergebnisse / Lesezeichen Schaltfläche – zeigt ein Symbol "Suchergebnisse" oder "Lesezeichen" auf der Suchleiste
    - Zeigt die Schaltfläche "Abbrechen" – ermöglicht Benutzern bei der Suchfunktion zu beenden. Es wird empfohlen, dass diese Option ausgewählt ist.
    - Zeigt Bereich – dadurch Benutzer um den Bereich für ihre Suche einzuschränken. Beispielsweise kann bei der Suche in der app Musik der Benutzer auswählen, ob sie Apple Musik- oder die Bibliothek für einen bestimmten Titel oder Interpreten gesucht werden soll. Um verschiedene Optionen anzuzeigen, fügen Sie ein Array von Titeln zu den **ScopeBarTitles** Eigenschaft.
    ![Suche Leiste Bereich Titel](searchbar-images/image4.png)

- **Textverhalten** – diese Optionen werden verwendet, um die Adresse wie die Benutzereingabe beim Eingeben von formatiert werden. Großschreibung wird den Anfang jedes Wort oder ein Satz, festgelegt oder jedes Zeichen als Großbuchstaben. Korrektur und Rechtschreibprüfung mit fordert den Benutzer mit Schreibweisen von Wörtern, wie sie eingeben.
- **Tastatur** – Steuerelemente die Tastatur-Format angezeigt, für die Eingabe, und daher welche Schlüssel auf der Tastatur verfügbar sind. Dies schließt Ziffernblock, Phone Pad, e-Mail-URL zusammen mit anderen Optionen.
- **Darstellung** – steuert die Darstellungsart der Tastatur und werden entweder dunkle oder helle Design.
- **Schlüssel zurückgegeben,** – ändern Sie die Bezeichnung auf die EINGABETASTE, um besser Rechnung, welche Aktion ausgeführt werden soll. Unterstützte Werte sind Go, Join, weiter, Route, Fertig, und suchen.
- **Sichere** – gibt an, ob die Eingabe maskiert wird (z. B. eine Kennworteingabe).

## <a name="related-links"></a>Verwandte Links

- [Search-Controller](https://developer.xamarin.com/recipes/ios/content_controls/search-controller/)
