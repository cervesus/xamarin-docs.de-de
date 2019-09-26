---
title: Verwenden von Daten in einer IOS-App
description: In diesem Dokument wird das DataAccess_Adv-Beispiel beschrieben, das veranschaulicht, wie Benutzereingaben erfasst und Daten Bank Vorgänge zum Erstellen, lesen, aktualisieren und löschen (CRUD) in einer xamarin. IOS-App durchgeführt werden.
ms.prod: xamarin
ms.assetid: 2CB8150E-CD2C-4E97-8605-1EE8CBACFEEC
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 10/11/2016
ms.openlocfilehash: 9441596cd457c3cc3a881e5db319ec3bbfc5a312
ms.sourcegitcommit: 699de58432b7da300ddc2c85842e5d9e129b0dc5
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2019
ms.locfileid: "70766855"
---
# <a name="using-data-in-an-ios-app"></a>Verwenden von Daten in einer IOS-App

Das **DataAccess_Adv** -Beispiel zeigt eine funktionierende Anwendung, die die Datenbankfunktionalität Benutzereingabe und *CRUD* (Create, Read, Update und DELETE) ermöglicht. Die Anwendung besteht aus zwei Bildschirmen: einer Liste und einem Dateneingabe Formular. Der gesamte Datenzugriffs Code kann ohne Änderung in IOS und Android wieder verwendet werden.

Nachdem Sie einige Daten hinzugefügt haben, sehen die Anwendungs Bildschirme in ios wie folgt aus:

 ![](using-data-in-an-app-images/image9.png "IOS-Beispielliste")

 ![](using-data-in-an-app-images/image10.png "IOS-Beispiel Details")

Das IOS-Projekt wird unten angezeigt – der in diesem Abschnitt gezeigte Code ist im **ORM** -Verzeichnis enthalten:

 ![](using-data-in-an-app-images/image13.png "IOS-Projektstruktur")

Der Native UI-Code für die viewcontrollers in ios ist für dieses Dokument nicht verfügbar.
Weitere Informationen zu den UI-Steuerelementen finden Sie im Handbuch [IOS Working with Tables and Cells](~/ios/user-interface/controls/tables/index.md) .

## <a name="read"></a>Lesen

Das Beispiel enthält eine Reihe von Lesevorgängen:

- Lesen der Liste
- Lesen einzelner Datensätze

Die beiden Methoden in der `StockDatabase` -Klasse sind:

```csharp
public IEnumerable<Stock> GetStocks ()
{
    lock (locker) {
        return (from i in Table<Stock> () select i).ToList ();
    }
}
public Stock GetStock (int id)
{
    lock (locker) {
        return Table<Stock>().FirstOrDefault(x => x.Id == id);
    }
}
```

IOS rendert die Daten anders als `UITableView`.

## <a name="create-and-update"></a>Erstellen und aktualisieren

Um den Anwendungscode zu vereinfachen, wird eine einzelne Save-Methode bereitgestellt, die einen INSERT-oder Update-Vorgang durchführt, je nachdem, ob PrimaryKey festgelegt wurde. Da die `Id` -Eigenschaft mit einem `[PrimaryKey]` -Attribut gekennzeichnet ist, sollten Sie Sie nicht im Code festlegen.
Diese Methode erkennt, ob der Wert zuvor gespeichert wurde (durch Überprüfen der Primärschlüssel Eigenschaft), und fügt das Objekt entsprechend ein:

```csharp
public int SaveStock (Stock item)
{
    lock (locker) {
        if (item.Id != 0) {
            Update (item);
            return item.Id;
    } else {
            return Insert (item);
        }
    }
}
```

Echte Anwendungen erfordern in der Regel eine gewisse Überprüfung (z. b. erforderliche Felder, minimale Längen oder andere Geschäftsregeln).
Gute plattformübergreifende Anwendungen implementieren so viel von der Überprüfung logisch wie möglich in frei gegebenem Code, wobei Validierungs Fehler an die Benutzeroberfläche weitergeleitet werden, sodass Sie entsprechend den Funktionen der Plattform angezeigt werden.

## <a name="delete"></a>Löschen

Im Gegensatz `Insert` zu `Update` den-und `Delete<T>` -Methoden kann die-Methode nur den Primärschlüssel Wert anstelle `Stock` eines Complete-Objekts akzeptieren.
In diesem Beispiel wird `Stock` ein-Objekt an die-Methode weitergegeben, aber nur die ID-Eigenschaft `Delete<T>` wird an die-Methode weitergegeben.

```csharp
public int DeleteStock(Stock stock)
{
    lock (locker) {
        return Delete<Stock> (stock.Id);
    }
}
```

## <a name="using-a-pre-populated-sqlite-database-file"></a>Verwenden einer vorab aufgefüllten SQLite-Datenbankdatei

Einige Anwendungen werden mit einer Datenbank ausgeliefert, die bereits mit Daten aufgefüllt wurde.
Sie können dies problemlos in Ihrer mobilen Anwendung erreichen, indem Sie eine vorhandene SQLite-Datenbankdatei mit Ihrer APP versenden und in ein beschreibbares Verzeichnis kopieren, bevor Sie darauf zugreifen. Da SQLite ein Standarddatei Format ist, das auf vielen Plattformen verwendet wird, stehen eine Reihe von Tools zum Erstellen einer SQLite-Datenbankdatei zur Verfügung:

- **SQLite Manager Firefox Extension** – funktioniert unter Mac und Windows und erzeugt Dateien, die mit IOS und Android kompatibel sind.
- **Befehlszeile** – Weitere Informationen finden Sie unter [www.sqlite.org/sqlite.html](http://www.sqlite.org/sqlite.html) .

Wenn Sie eine Datenbankdatei für die Verteilung mit Ihrer APP erstellen, achten Sie darauf, dass Tabellen und Spalten benannt werden, um sicherzustellen, dass Sie mit den Anforderungen Ihres Codes identisch sind. Dies gilt insbesondere, C# Wenn Sie sqlite.NET verwenden, der davon ausgeht, dass die Namen der Klassen und Eigenschaften entsprechen oder die zugeordneten benutzerdefinierten Attribute).

Fügen Sie für IOS die SQLite-Datei in Ihre Anwendung ein, und stellen Sie **sicher, dass Sie mit Buildaktion gekennzeichnet ist: Inhalt**. Platzieren Sie den Code in `FinishedLaunching` , um die Datei in ein beschreibbares Verzeichnis zu kopieren, *bevor* Sie Daten Methoden abrufen. Mit dem folgenden Code wird eine vorhandene Datenbank mit dem Namen **Data. sqlite**kopiert, sofern diese nicht bereits vorhanden ist.

```csharp
// Copy the database across (if it doesn't exist)
var appdir = NSBundle.MainBundle.ResourcePath;
var seedFile = Path.Combine (appdir, "data.sqlite");
if (!File.Exists (Database.DatabaseFilePath))
{
  File.Copy (seedFile, Database.DatabaseFilePath);
}
```

Jeglicher Datenzugriffs Code (ob ADO.net oder using sqlite.net), der nach Abschluss dieses Vorgangs ausgeführt wird, hat Zugriff auf die vorab aufgefüllten Daten.

## <a name="related-links"></a>Verwandte Links

- [DataAccess Basic (Beispiel)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [DataAccess (erweitert) (Beispiel)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [IOS-Daten Rezepte](https://github.com/xamarin/recipes/tree/master/Recipes/ios/data/sqlite)
- [Xamarin. Forms-Datenzugriff](~/xamarin-forms/data-cloud/data/databases.md)
