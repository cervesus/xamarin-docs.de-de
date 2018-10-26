---
title: Verwenden von Daten in einer iOS-app
description: Dieses Dokument beschreibt die DataAccess_Adv Beispiel veranschaulicht das Erfassen von Benutzereingaben und führen Sie zu erstellen, lesen, aktualisieren und löschen (CRUD) von Datenbankvorgängen in einer Xamarin.iOS-app.
ms.prod: xamarin
ms.assetid: 2CB8150E-CD2C-4E97-8605-1EE8CBACFEEC
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 10/11/2016
ms.openlocfilehash: e3127f85841c13422d9674bcf12373af9222afba
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50115666"
---
# <a name="using-data-in-an-ios-app"></a>Verwenden von Daten in einer iOS-app

Die **DataAccess_Adv** Beispiel wird gezeigt, eine Anwendung, die Benutzereingaben zulässt und *CRUD* Datenbankfunktionen (Create, Read, Update und Delete). Die Anwendung besteht aus zwei Bildschirme: eine Liste und ein Dateneingabeformular. Alle der Datenzugriffscode ist wieder verwendbare in iOS und Android ohne Änderung.

Nach dem Hinzufügen von Daten wird unter iOS die Bildschirme für die Anwendung wie folgt aussehen:

 ![](using-data-in-an-app-images/image9.png "Liste der iOS-beispielanwendung")

 ![](using-data-in-an-app-images/image10.png "Details der iOS-beispielanwendung")

IOS Project ist unten dargestellt – in der Code in diesem Abschnitt enthalten ist das **Orm** Verzeichnis:

 ![](using-data-in-an-app-images/image13.png "iOS-Projektstruktur")

Der native UI-Code für die ViewControllers in iOS ist nicht Gegenstand dieses Dokuments.
Finden Sie in der [iOS arbeiten mit Tabellen und Zellen](~/ios/user-interface/controls/tables/index.md) Anleitung finden Sie weitere Informationen auf der UI-Steuerelemente.

## <a name="read"></a>Lesen

Es gibt eine Reihe von Lesevorgängen, in dem Beispiel aus:

-  Lesen Sie die Liste
-  Lesen Sie die einzelnen Datensätze


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

iOS rendert die Daten anders als eine `UITableView`.

## <a name="create-and-update"></a>Erstellen und aktualisieren

Um den Anwendungscode zu vereinfachen, wird ein einzelnes save-Methode bereitgestellt, die einen Einfügevorgang ausführt oder aktualisieren, je nachdem, ob die PrimaryKey festgelegt wurde. Da die `Id` Eigenschaft markiert ist, mit einem `[PrimaryKey]` Attribut sollten Sie es nicht im Code festlegen.
Diese Methode erkennt, ob der Wert vorherigen wurde (durch Überprüfen der Primärschlüsseleigenschaft) gespeichert und entweder das Einfügen oder aktualisieren das Objekt entsprechend:

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



Echte Anwendungen müssen in der Regel eine Validierung (z.B. erforderliche Felder, Mindestlänge oder anderer Geschäftsregeln).
Gute plattformübergreifende Anwendungen so großen Teil der Überprüfung logische wie möglich im freigegebenen Code auf und übergibt Validierungsfehler an die Benutzeroberfläche für die Anzeige gemäß der Funktionen der Plattform implementieren.

## <a name="delete"></a>Löschen

Im Gegensatz zu den `Insert` und `Update` Methoden, die `Delete<T>` Methode lässt nur den Primärschlüsselwert anstatt ein vollständiges `Stock` Objekt.
In diesem Beispiel eine `Stock` Objekt wird an die Methode übergeben, aber nur die Id-Eigenschaft übergeben wird, die `Delete<T>` Methode.

```csharp
public int DeleteStock(Stock stock)
{
    lock (locker) {
        return Delete<Stock> (stock.Id);
    }
}
```

## <a name="using-a-pre-populated-sqlite-database-file"></a>Verwenden einer voraufgefüllten SQLite-Datenbankdatei

Einige Anwendungen werden mit einer Datenbank, die bereits mit Daten gefüllt ausgeliefert.
Sie können ganz einfach in Ihrer mobilen Anwendung dazu durch den Versand von einer vorhandenen Datenbankdatei von SQLite mit Ihrer app, und es in ein beschreibbares Verzeichnis kopiert, bevor Sie darauf zugreifen. Da SQLite ein standard-Dateiformat, die auf vielen Plattformen verwendet wird handelt, gibt es eine Reihe von Tools zur Verfügung, um eine SQLite-Datenbank-Datei zu erstellen:

-  **Firefox-Erweiterung für SQLite Manager** – kann unter Mac und Windows und erstellt Dateien, die mit iOS und Android kompatibel sind.
-  **Über die Befehlszeile** – finden Sie unter [www.sqlite.org/sqlite.html](http://www.sqlite.org/sqlite.html) .


Wenn Sie eine Datei für die Verteilung mit Ihrer app zu erstellen, achten Sie mit der Benennung von Tabellen und Spalten, stellen Sie sicher, was Ihr Code erwartet, sie übereinstimmen, insbesondere dann, wenn Sie von SQLite.NET verwenden, der erwartet die Namen Ihrer c#-Klassen und Eigenschaften übereinstimmen (oder die benutzerdefinierte Attribute) zugeordnet.

Für iOS, schließen Sie die Sqlite-Datei in Ihrer Anwendung aus, und stellen Sie sicher, ist es mit markiert **Buildvorgang: Inhalt**. Fügen Sie den Code in die `FinishedLaunching` auf die Datei in ein beschreibbares Verzeichnis kopieren *vor* Sie keine Datenmethoden aufgerufen werden. Der folgende Code kopiert eine vorhandene Datenbank mit dem Namen **data.sqlite**, nur dann, wenn er nicht bereits vorhanden.

```csharp
// Copy the database across (if it doesn't exist)
var appdir = NSBundle.MainBundle.ResourcePath;
var seedFile = Path.Combine (appdir, "data.sqlite");
if (!File.Exists (Database.DatabaseFilePath))
{
  File.Copy (seedFile, Database.DatabaseFilePath);
}
```

Datenzugriffscode (ob ADO.NET oder Verwenden von SQLite.NET), die ausgeführt wird, nachdem dies abgeschlossen ist, haben Zugriff auf die Daten vorab aufgefüllten hat.


## <a name="related-links"></a>Verwandte Links

- [DataAccess Basic (Beispiel)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [DataAccess-erweitert (Beispiel)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [iOS-Daten-Rezepte](https://github.com/xamarin/recipes/tree/master/Recipes/ios/data/sqlite)
- [Xamarin.Forms-Datenzugriff](~/xamarin-forms/app-fundamentals/databases.md)
