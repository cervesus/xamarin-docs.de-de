---
title: Verwenden von Daten in eine App
ms.topic: article
ms.prod: xamarin
ms.assetid: D5932AEB-0B6E-4F37-8B32-9BE4775AEE85
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/08/2018
ms.openlocfilehash: 5350317c82d1073d7679843272e3215634f91217
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="using-data-in-an-app"></a>Verwenden von Daten in eine App

Die **DataAccess_Adv** Beispiel zeigt eine funktionierende Anwendung, die Benutzereingaben und CRUD (Create, Read, Update und Delete)-Datenbankfunktionen ermöglicht. Die Anwendung besteht aus zwei Bildschirme: eine Liste und ein Dateneingabeformular. Alle Datenzugriffscode ist wieder verwendbare in iOS und Android unverändert.

Nach dem Hinzufügen von Daten wird unter Android die Anwendung Bildschirme wie folgt aussehen:

![Beispiel zu Android-Liste](using-data-in-an-app-images/image11.png "Beispiel zu Android-Liste")

![Beispiel zu Android-Detail](using-data-in-an-app-images/image12.png "Beispiel zu Android-Detail")

Das Android-Projekt wird unten gezeigt &ndash; der in diesem Abschnitt gezeigten Code befindet sich innerhalb der **Orm** Verzeichnis:

![Android Projektstruktur](using-data-in-an-app-images/image14.png "Android-Projekt-Struktur")

Der systemeigene UI-Code für Aktivitäten in Android ist nicht Gegenstand dieses Dokuments. Finden Sie in der [Android Listenansichten und Adapter](~/android/user-interface/layouts/list-view/index.md) Handbuch für Weitere Informationen zu den UI-Steuerelementen.

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

Android rendert die Daten als ein `ListView`.

## <a name="create-and-update"></a>Erstellen und aktualisieren

Um den Code der Anwendung zu vereinfachen, wird eine einzelne save-Methode bereitgestellt, die einen Einfügevorgang ausführt oder Update abhängig, ob die PrimaryKey festgelegt wurde. Da die `Id` Eigenschaft gekennzeichnet ist, mit einem `[PrimaryKey]` Attribut sollten Sie es nicht im Code festlegen. Diese Methode erkennt, ob der Wert vorherigen wurde (durch Überprüfen der Eigenschaft für Primärschlüssel) gespeichert und einfügen oder aktualisieren Sie das Objekt entsprechend:

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

Praxis werden in der Regel einige Überprüfung (z. B. Pflichtfelder, Mindestlänge oder andere Geschäftsregeln) erforderlich ist. Gute plattformübergreifende Anwendungen so viel wie möglich im freigegebenen Code, übergeben Validierungsfehler auf der Benutzeroberfläche für die Anzeige entsprechend den Funktionen der Plattform sichern logische Validierung implementieren.

## <a name="delete"></a>Löschen

Im Gegensatz zu den `Insert` und `Update` Methoden, die `Delete<T>` -Methode kann nur dem Primärschlüsselwert anstelle einer vollständigen akzeptieren `Stock` Objekt. In diesem Beispiel wird eine `Stock` Objekt wird an die Methode übergeben, aber nur die Id-Eigenschaft übergeben wird, die `Delete<T>` Methode.

```csharp
public int DeleteStock(Stock stock)
{
    lock (locker) {
        return Delete<Stock> (stock.Id);
    }
}
```

## <a name="using-a-pre-populated-sqlite-database-file"></a>Verwenden einer vorgegebenen SQLite-Datenbankdatei

Einige Anwendungen sind im Lieferumfang einer Datenbank, die bereits mit Daten aufgefüllt. Sie können problemlos in der mobilen Anwendung dies durch eine vorhandene SQLite-Datenbankdatei mit der app für den Vertrieb, und es in ein beschreibbaren Verzeichnis kopieren, bevor Sie darauf zugreifen. Da SQLite eine standard-Dateiformat handelt, ist auf vielen Plattformen verwendet wird, sind gibt es eine Reihe von Tools zum Erstellen einer Datenbankdatei SQLite verfügbar:

-   **SQLite Manager Firefox-Erweiterung** &ndash; funktioniert für Macintosh und Windows und erstellt Dateien, die mit iOS und Android kompatibel sind.

-   **Über die Befehlszeile** &ndash; finden Sie unter [www.sqlite.org/sqlite.html](http://www.sqlite.org/sqlite.html) .

Wenn Sie eine Datenbankdatei für die Verteilung mit Ihrer app zu erstellen, achten Sie darauf mit der Benennung von Tabellen und Spalten, um sicherzustellen, dass sie übereinstimmen was Codes erwartet, insbesondere dann, wenn Sie SQLite.NET verwenden die Namen die auszuwählenden der C#-Klassen und Eigenschaften entsprechen erwarten wird (oder die benutzerdefinierte Attribute) zugeordnet.

Um sicherzustellen, dass ein Teil des Codes vor dem Sonstiges in Ihrer Android-app ausgeführt wird, können Sie diesen in der ersten Aktivität geladen platzieren, oder erstellen eine `Application` Unterklasse, die geladen wird, bevor alle Aktivitäten. Der folgende Code zeigt eine `Application` Unterklasse, die kopiert eine vorhandene Datenbankdatei **data.sqlite** aus der **Systemablagen/Raw/** Verzeichnis.

```csharp
[Application]
public class YourAndroidApp : Application {
    public override void OnCreate ()
    {
        base.OnCreate ();
        var docFolder = Environment.GetFolderPath(Environment.SpecialFolder.Personal);
        Console.WriteLine ("Data path:" + Database.DatabaseFilePath);
        var dbFile = Path.Combine(docFolder, "data.sqlite"); // FILE NAME TO USE WHEN COPIED
        if (!System.IO.File.Exists(dbFile)) {
            var s = Resources.OpenRawResource(Resource.Raw.data);  // DATA FILE RESOURCE ID
            FileStream writeStream = new FileStream(dbFile, FileMode.OpenOrCreate, FileAccess.Write);
            ReadWriteStream(s, writeStream);
        }
    }
    // readStream is the stream you need to read
    // writeStream is the stream you want to write to
    private void ReadWriteStream(Stream readStream, Stream writeStream)
    {
        int Length = 256;
        Byte[] buffer = new Byte[Length];
        int bytesRead = readStream.Read(buffer, 0, Length);
        // write the required bytes
        while (bytesRead > 0)
        {
            writeStream.Write(buffer, 0, bytesRead);
            bytesRead = readStream.Read(buffer, 0, Length);
        }
        readStream.Close();
        writeStream.Close();
    }
}
```


## <a name="related-links"></a>Verwandte Links

- [DataAccess Basic (Beispiel)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [DataAccess erweiterte (Beispiel)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [Android Daten Rezepte](https://developer.xamarin.com/recipes/android/data/)
- [Xamarin.Forms-Datenzugriff](~/xamarin-forms/app-fundamentals/databases.md)
