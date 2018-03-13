---
title: Verwenden von Daten in eine App
ms.topic: article
ms.prod: xamarin
ms.assetid: 2CB8150E-CD2C-4E97-8605-1EE8CBACFEEC
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 10/11/2016
ms.openlocfilehash: aefec1992a5f061f9d50d499b3594601a2651b17
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/12/2018
---
# <a name="using-data-in-an-app"></a>Verwenden von Daten in eine App

Die **DataAccess_Adv** Beispiel zeigt eine funktionierende Anwendung, die Benutzereingaben zulässt und *CRUD* (Create, Read, Update und Delete)-Datenbankfunktionen. Die Anwendung besteht aus zwei Bildschirme: eine Liste und ein Dateneingabeformular. Alle Datenzugriffscode ist wieder verwendbare in iOS und Android unverändert.

Nach dem Hinzufügen von Daten wird auf iOS der Anwendung Bildschirme wie folgt aussehen:

 ![](using-data-in-an-app-images/image9.png "Liste der iOS-Beispiel")

 ![](using-data-in-an-app-images/image10.png "iOS-Beispiel-detail")

IOS Projekt wird unten gezeigt – die in diesem Abschnitt gezeigten Code befindet sich innerhalb der **Orm** Verzeichnis:

 ![](using-data-in-an-app-images/image13.png "iOS-Projektstruktur")

Der systemeigene UI-Code für die ViewControllers in iOS ist nicht Gegenstand dieses Dokuments.
Finden Sie in der [iOS arbeiten mit Tabellen und Zellen](~/ios/user-interface/controls/tables/index.md) Handbuch für Weitere Informationen zu den UI-Steuerelementen.

## <a name="read"></a>Lesen

Es gibt eine Reihe von Lesevorgängen in der Stichprobe ein:

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

Um den Code der Anwendung zu vereinfachen, wird eine einzelne save-Methode bereitgestellt, die einen Einfügevorgang ausführt oder Update abhängig, ob die PrimaryKey festgelegt wurde. Da die `Id` Eigenschaft gekennzeichnet ist, mit einem `[PrimaryKey]` Attribut sollten Sie es nicht im Code festlegen.
Diese Methode erkennt, ob der Wert vorherigen wurde (durch Überprüfen der Eigenschaft für Primärschlüssel) gespeichert und einfügen oder aktualisieren Sie das Objekt entsprechend:

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



Praxis werden in der Regel einige Überprüfung (z. B. Pflichtfelder, Mindestlänge oder andere Geschäftsregeln) erforderlich ist.
Gute plattformübergreifende Anwendungen so viel wie möglich im freigegebenen Code, übergeben Validierungsfehler auf der Benutzeroberfläche für die Anzeige entsprechend den Funktionen der Plattform sichern logische Validierung implementieren.

## <a name="delete"></a>Löschen

Im Gegensatz zu den `Insert` und `Update` Methoden, die `Delete<T>` -Methode kann nur dem Primärschlüsselwert anstelle einer vollständigen akzeptieren `Stock` Objekt.
In diesem Beispiel wird eine `Stock` Objekt wird an die Methode übergeben, aber nur die Id-Eigenschaft übergeben wird, die `Delete<T>` Methode.

```csharp
public int DeleteStock(Stock stock)
{
    lock (locker) {
        return Delete<Stock> (stock.Id);
    }
}
```

## <a name="using-a-pre-populated-sqlite-database-file"></a>Verwenden einer vorgegebenen SQLite-Datenbankdatei

Einige Anwendungen sind im Lieferumfang einer Datenbank, die bereits mit Daten aufgefüllt.
Sie können problemlos in der mobilen Anwendung dies durch eine vorhandene SQLite-Datenbankdatei mit der app für den Vertrieb, und es in ein beschreibbaren Verzeichnis kopieren, bevor Sie darauf zugreifen. Da SQLite eine standard-Dateiformat handelt, ist auf vielen Plattformen verwendet wird, sind gibt es eine Reihe von Tools zum Erstellen einer Datenbankdatei SQLite verfügbar:

-  **SQLite Manager Firefox-Erweiterung** – funktioniert für Macintosh und Windows und erstellt Dateien, die mit iOS und Android kompatibel sind.
-  **Über die Befehlszeile** – finden Sie unter [www.sqlite.org/sqlite.html](http://www.sqlite.org/sqlite.html) .


Wenn Sie eine Datenbankdatei für die Verteilung mit Ihrer app zu erstellen, achten Sie darauf mit der Benennung von Tabellen und Spalten, um sicherzustellen, dass sie übereinstimmen was Codes erwartet, insbesondere dann, wenn Sie SQLite.NET verwenden die Namen die auszuwählenden der C#-Klassen und Eigenschaften entsprechen erwarten wird (oder die benutzerdefinierte Attribute) zugeordnet.

Für iOS, schließen Sie die Sqlite-Datei in der Anwendung, und stellen Sie sicher, es mit RuntimeCompatibility **Buildvorgang: Inhalt**. Fügen Sie den Code in der `FinishedLaunching` So kopieren Sie die Datei zu einem beschreibbaren Verzeichnis *vor* Sie keine Datenmethoden aufrufen. Im folgende Code wird eine vorhandene Datenbank mit dem Namen kopieren **data.sqlite**, nur dann, wenn er nicht bereits vorhanden.

```csharp
// Copy the database across (if it doesn't exist)
var appdir = NSBundle.MainBundle.ResourcePath;
var seedFile = Path.Combine (appdir, "data.sqlite");
if (!File.Exists (Database.DatabaseFilePath))
{
  File.Copy (seedFile, Database.DatabaseFilePath);
}
```

Alle Datenzugriffscode (ob ADO.NET oder SQLite.NET verwenden), die ausgeführt wird, nachdem dies abgeschlossen wird, haben Zugriff auf die Daten bereits aufgefüllten hat.


## <a name="related-links"></a>Verwandte Links

- [DataAccess Basic (Beispiel)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [DataAccess erweiterte (Beispiel)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [iOS Daten Rezepte](https://developer.xamarin.com/recipes/ios/data/sqlite/)
- [Xamarin.Forms-Datenzugriff](~/xamarin-forms/app-fundamentals/databases.md)
