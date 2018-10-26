---
title: Search-Balken in Xamarin.iOS
description: In diesem Dokument wird beschrieben, wie Suchleisten in Xamarin.iOS verwendet wird. Es wird erläutert, wie Suchleisten programmgesteuert und in einem Storyboard zu erstellen.
ms.prod: xamarin
ms.assetid: 22A8249A-19C6-4734-8331-E49FE3170771
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 07/11/2017
ms.openlocfilehash: 923eebc2674abdbbd66d9db11bebf9f503928569
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50107322"
---
# <a name="search-bars-in-xamarinios"></a>Search-Balken in Xamarin.iOS

Die UISearchBar wird verwendet, um über eine Liste von Werten zu suchen. 

Es enthält drei Hauptkomponenten: 

- Ein Feld zum Eingeben von Text. Benutzer können diese Option, um ihren Suchbegriff eingeben nutzen.
- Eine Schaltfläche "löschen", um einen beliebigen Text in das Suchfeld zu entfernen.
- Eine Schaltfläche "Abbrechen", bei der Suchfunktion zu beenden.

![Suchleiste](searchbar-images/image1.png)

## <a name="implementing-the-search-bar"></a>Implementieren der Suchleiste

Zu Beginn Leiste suchen implementieren, indem Sie eine neue zu instanziieren:

```csharp
searchBar = new UISearchBar();
```

Ein, und legen Sie es. Das folgende Beispiel zeigt, wie sie in der Navigationsleiste oder in der HeaderView einer Tabelle platziert wird:

```csharp
NavigationItem.TitleView = searchBar;

\\or

TableView.TableHeaderView = searchBar;
```

Festlegen von Eigenschaften auf der Suchleiste:

```csharp
 searchBar = new UISearchBar(){
                Placeholder = "Enter your search Item",
                Prompt = "Search Entered here",
                ShowsScopeBar = true,
                ScopeButtonTitles = new string[]{ "Boston", "London", "SF" },
            };
```

![Suchleiste-Eigenschaften](searchbar-images/image6.png)

Auslösen der `SearchButtonClicked` Ereignis aus, wenn die Durchsuchen-Schaltfläche gedrückt wird. Dadurch wird Ihre Logik für die Suche aufgerufen:

```csharp
searchBar.SearchButtonClicked += (sender, e) => {
                Search ();
            };
```

Informationen zum Verwalten der Präsentation der Suchleiste und Suchergebnisse finden Sie in der [Suche Controller ](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/search-controller) Anleitung.

## <a name="using-the-search-bar-in-the-designer"></a>Verwenden die Suchleiste im Designer

Der Designer bietet zwei Optionen für die Implementierung eine Suchleiste im designer

- Suchleiste
- Suchleiste mit Search-Anzeige-Controller (veraltet)

![Search-Steuerelemente im designer](searchbar-images/image2.png)

Verwenden Sie den Bereich für die Eigenschaft zum Festlegen von Eigenschaften in der Suchleiste

![Suche Leiste Eigenschaften-designer](searchbar-images/image3.png)

Diese Eigenschaften werden im folgenden erläutert:

- **Text und Platzhalter Eingabeaufforderung** – diese Eigenschaften werden zum vorschlagen und anweisen, wie Benutzer auf die Suchleiste verwenden sollten. Beispielsweise wenn Ihre app eine Liste der Speicher angezeigt. können die Prompt-Eigenschaft Sie darauf hinzuweisen, dass der Benutzer "City, Story-Name oder Postleitzahl eingeben können"
- **Suchen Sie Stil** – Sie können festlegen, dass die Suchleiste, um entweder **Prominent** oder **minimale**. Verwenden die deutliche wird alles auf dem Bildschirm mit Ausnahme der Suchleiste, sodass des Fokus auf die Suchleiste gezeichnet werden getönt werden soll. Die minimale Style-Suchleiste einfügen mit dessen Umgebung.
- **Funktionen** – aktivieren diese Eigenschaften nur das Element der Benutzeroberfläche angezeigt. Die Funktionalität muss implementiert werden, diese durch das richtige Ereignis wie im Auslösen der [Leiste-Websuche-API-Dokumentation](https://developer.xamarin.com/api/type/UIKit.UISearchBar/)
    - Suchergebnisse zeigt / Schaltfläche "Lesezeichen" – zeigt ein Symbol "Suchergebnisse" oder "Lesezeichen" auf der Suchleiste
    - Zeigt die Schaltfläche "Abbrechen" – ermöglicht Benutzern bei der Suchfunktion zu beenden. Es wird empfohlen, dass diese Option ausgewählt ist.
    - Zeigt Bereichsleiste – Dies ermöglicht, dass Benutzer, um den Umfang ihrer Suche einzuschränken. Beispielsweise kann bei der Suche in der Musik-app der Benutzer auswählen, ob sie Apple Music oder ihre Bibliothek für einen bestimmten Titel oder Künstler suchen möchten. Um verschiedene Optionen anzuzeigen, fügen Sie ein Array von Titeln zu den **ScopeBarTitles** Eigenschaft.
    ![Leiste Bereich-Titel suchen](searchbar-images/image4.png)

- **Textverhalten** – diese Optionen werden verwendet, um die Adresse, wie die Benutzereingabe formatiert werden, wenn sie eine Eingabe vornehmen. Großschreibung wird festgelegt, den Anfang jedes Wort oder einen Satz ein, oder alle Zeichen als Großbuchstaben. Korrektur und Rechtschreibprüfung mit auffordern den Benutzer mit der vorgeschlagenen Schreibweisen Wörter homophone.
- **Tastatur** – Steuerelemente der Tastatur-Stil angezeigt, für die Eingabe, und welche Schlüssel sind daher verfügbar, auf der Tastatur. Dies schließt Pad "Anzahl" "," Phone Pad ","-e-Mail "," URL zusammen mit anderen Optionen.
- **Darstellung** – steuert die Darstellungsart des der Tastatur und werden entweder dunkel oder helles Design.
- **Schlüssel zurückgegeben,** – ändern Sie die Bezeichnung für die Return-Taste, besser widerzuspiegeln, welche Aktion durchgeführt wird. Unterstützte Werte sind Go, Join, weiter, Route, Fertig, und suchen.
- **Sichere** – gibt an, ob die Eingabe maskiert wird (z. B. eine Kennworteingabe).

## <a name="related-links"></a>Verwandte Links

- [Search-Controller](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/search-controller)
