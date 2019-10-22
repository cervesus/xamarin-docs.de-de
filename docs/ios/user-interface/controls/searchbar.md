---
title: Suchleisten in xamarin. IOS
description: In diesem Dokument wird beschrieben, wie Suchleisten in xamarin. IOS verwendet werden. Es wird erläutert, wie Suchleisten Programm gesteuert und in einem Storyboard erstellt werden.
ms.prod: xamarin
ms.assetid: 22A8249A-19C6-4734-8331-E49FE3170771
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 07/11/2017
ms.openlocfilehash: 36e339139a0a7f853a770fdb188b5f03ee93f7ee
ms.sourcegitcommit: 9bfedf07940dad7270db86767eb2cc4007f2a59f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2019
ms.locfileid: "70283350"
---
# <a name="search-bars-in-xamarinios"></a>Suchleisten in xamarin. IOS

Uisearchbar wird verwendet, um eine Liste von Werten zu durchsuchen.

Sie enthält drei Hauptkomponenten:

- Ein Feld, das zum Eingeben von Text verwendet wird. Benutzer können diese verwenden, um Ihren Suchbegriff einzugeben.
- Eine Schaltfläche zum Löschen, um Text aus dem Suchfeld zu entfernen.
- Eine Schaltfläche "Abbrechen", um die Suchfunktion zu beenden.

![Suchleiste](searchbar-images/image1.png)

## <a name="implementing-the-search-bar"></a>Implementieren der Suchleiste

Um die Suchleiste zu implementieren, beginnen Sie, indem Sie eine neue instanziieren:

```csharp
searchBar = new UISearchBar();
```

Und dann platzieren. Das folgende Beispiel zeigt, wie Sie es in der Navigationsleiste oder in der Header View einer Tabelle platzieren:

```csharp
NavigationItem.TitleView = searchBar;

// or

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

![Eigenschaften der Suchleiste](searchbar-images/image6.png)

Gibt das `SearchButtonClicked`-Ereignis aus, wenn die Such Schaltfläche gedrückt wird. Dadurch wird Ihre Suchlogik aufgerufen:

```csharp
searchBar.SearchButtonClicked += (sender, e) => {
                Search ();
            };
```

Informationen zum Verwalten der Darstellung der Suchleiste und der Suchergebnisse finden Sie in der Anleitung für den [Such Controller](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/search-controller) .

## <a name="using-the-search-bar-in-the-designer"></a>Verwenden der Suchleiste im Designer

Der Designer bietet zwei Optionen für die Implementierung einer Suchleiste im Designer.

- Suchleiste
- Suchleiste mit Suchanzeige Controller (veraltet)

![Suchleisten-Steuerelemente im Designer](searchbar-images/image2.png)

Verwenden Sie den Eigenschaften Bereich, um Eigenschaften auf der Suchleiste festzulegen.

![Suchleisten Eigenschaften-Designer](searchbar-images/image3.png)

Diese Eigenschaften werden im folgenden erläutert:

- **Text, Platzhalter, Aufforderung** – diese Eigenschaften werden verwendet, um vorzuschlagen und anzuweisen, wie Benutzer die Suchleiste verwenden sollten. Wenn Ihre APP z. b. eine Liste von Stores angezeigt hat, können Sie mithilfe der Prompt-Eigenschaft anzeigen, dass Benutzer "Ort", "textabame" oder "Postleitzahl" eingeben können.
- **Suchart** – Sie können festlegen, dass die Suchleiste entweder sehr **auffällig** oder **Minimal**ist. Wenn Sie den prominenten verwenden, wird alles auf dem Bildschirm angezeigt, mit Ausnahme der Suchleiste, wodurch der Fokus auf die Suchleiste gezogen wird. Die Suchleiste im minimalen Stil wird in die Umgebung integriert.
- **Funktionen** – durch Aktivieren dieser Eigenschaften wird nur das UI-Element angezeigt. Die Funktionalität muss für diese implementiert werden, indem das richtige Ereignis wie in den Suchleisten- [API](xref:UIKit.UISearchBar) -Dokumentation ausführlich aufgeführt wird.
  - Zeigt die Schaltfläche "Suchergebnisse/Lesezeichen" – zeigt ein Suchergebnis oder Lesezeichen Symbol auf der Suchleiste an
  - Zeigt die Schaltfläche "Abbrechen" – ermöglicht Benutzern das Beenden der Suchfunktion. Es wird empfohlen, diese Option auszuwählen.
  - Zeigt die Bereichs Leiste an – dadurch können Benutzer den Umfang Ihrer Suche einschränken. Wenn Sie z. b. in der Music-App suchen, kann der Benutzer auswählen, ob Apple Music oder seine Bibliothek nach einem bestimmten Song oder einer bestimmten Künstlerin durchsucht werden soll. Zum Anzeigen verschiedener Optionen fügen Sie der Eigenschaft **scopebartiteln** ein Array von Titeln hinzu.
  ![Search leisten Bereichs Titel ](searchbar-images/image4.png)

- **Text Verhalten** – diese Optionen werden verwendet, um zu adressieren, wie die Benutzereingabe bei der Eingabe formatiert wird. Durch die Groß-/Kleinschreibung wird der Anfang jedes Worts oder Satzes oder jedes Zeichen als Großbuchstabe festgelegt. Korrektur-und Rechtschreibprüfung, bei der der Benutzer zur Eingabe von empfohlenen rechtschreibweisen bei der Eingabe aufgefordert wird.
- **Tastatur** – steuert den Tastatur Stil, der für die Eingabe angezeigt wird, und damit die auf der Tastatur verfügbaren Tasten. Dies schließt das Zahlen Pad, den telefonpad, die e-Mail, die URL und andere Optionen ein.
- Darstellung **– steuert** den Stil der Tastatur und ist entweder dunkel oder hell.
- **Return Key** – ändern Sie die Bezeichnung in der Rückgabetaste, um besser widerzuspiegeln, welche Aktion durchgeführt wird. Zu den unterstützten Werten zählen go, Join, Next, Route, done und Search.
- **Secure** – gibt an, ob die Eingabe maskiert wird (z. b. für eine Kenn Wort Eingabe).

## <a name="related-links"></a>Verwandte Links

- [Controller suchen](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/search-controller)
