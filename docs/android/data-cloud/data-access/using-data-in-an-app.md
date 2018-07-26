---
title: Verwenden von Daten in einer Android-App
ms.prod: xamarin
ms.assetid: D5932AEB-0B6E-4F37-8B32-9BE4775AEE85
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/08/2018
ms.openlocfilehash: 563c04ef1c8eec00108844894c5f9bdc0e9950e3
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/25/2018
ms.locfileid: "39241889"
---
# <a name="using-data-in-an-app"></a>Verwenden von Daten in einer App

Die **DataAccess_Adv** Beispiel wird gezeigt, eine Anwendung, die Benutzereingaben und Datenbankfunktionen CRUD (Create, Read, Update und Delete) ermöglicht. Die Anwendung besteht aus zwei Bildschirme: eine Liste und ein Dateneingabeformular. Alle der Datenzugriffscode ist wieder verwendbare in iOS und Android ohne Änderung.

Nach dem Hinzufügen von Daten wird unter Android die Bildschirme für die Anwendung wie folgt aussehen:

![Beispiel zu Android-Liste](using-data-in-an-app-images/image11.png "Beispiel zu Android-Liste")

![Beispiel zu Android-Detail](using-data-in-an-app-images/image12.png "Beispiel zu Android-Details")

Das Android-Projekt wird im folgenden dargestellt &ndash; der in diesem Abschnitt gezeigten Code enthaltenen der **Orm** Verzeichnis:

![Android-Projektstruktur](using-data-in-an-app-images/image14.png "Android-Projekt-Struktur")

Der native UI-Code für die Aktivitäten in Android ist nicht Gegenstand dieses Dokuments. Finden Sie in der [Android ListViews und Adapter](~/android/user-interface/layouts/list-view/index.md) Anleitung finden Sie weitere Informationen auf der UI-Steuerelemente.

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

Android rendert die Daten als eine `ListView`.

## <a name="create-and-update"></a>Erstellen und aktualisieren

Um den Anwendungscode zu vereinfachen, wird ein einzelnes save-Methode bereitgestellt, die einen Einfügevorgang ausführt oder aktualisieren, je nachdem, ob die PrimaryKey festgelegt wurde. Da die `Id` Eigenschaft markiert ist, mit einem `[PrimaryKey]` Attribut sollten Sie es nicht im Code festlegen. Diese Methode erkennt, ob der Wert vorherigen wurde (durch Überprüfen der Primärschlüsseleigenschaft) gespeichert und entweder das Einfügen oder aktualisieren das Objekt entsprechend:

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

Echte Anwendungen müssen in der Regel eine Validierung (z.B. erforderliche Felder, Mindestlänge oder anderer Geschäftsregeln). Gute plattformübergreifende Anwendungen so großen Teil der Überprüfung logische wie möglich im freigegebenen Code auf und übergibt Validierungsfehler an die Benutzeroberfläche für die Anzeige gemäß der Funktionen der Plattform implementieren.

## <a name="delete"></a>Löschen

Im Gegensatz zu den `Insert` und `Update` Methoden, die `Delete<T>` Methode lässt nur den Primärschlüsselwert anstatt ein vollständiges `Stock` Objekt. In diesem Beispiel eine `Stock` Objekt wird an die Methode übergeben, aber nur die Id-Eigenschaft übergeben wird, die `Delete<T>` Methode.

```csharp
public int DeleteStock(Stock stock)
{
    lock (locker) {
        return Delete<Stock> (stock.Id);
    }
}
```

## <a name="using-a-pre-populated-sqlite-database-file"></a>Verwenden einer voraufgefüllten SQLite-Datenbankdatei

Einige Anwendungen werden mit einer Datenbank, die bereits mit Daten gefüllt ausgeliefert. Sie können ganz einfach in Ihrer mobilen Anwendung dazu durch den Versand von einer vorhandenen Datenbankdatei von SQLite mit Ihrer app, und es in ein beschreibbares Verzeichnis kopiert, bevor Sie darauf zugreifen. Da SQLite ein standard-Dateiformat, die auf vielen Plattformen verwendet wird handelt, gibt es eine Reihe von Tools zur Verfügung, um eine SQLite-Datenbank-Datei zu erstellen:

-   **Firefox-Erweiterung für SQLite Manager** &ndash; funktioniert auf Mac und Windows und erstellt Dateien, die mit iOS und Android kompatibel sind.

-   **Über die Befehlszeile** &ndash; finden Sie unter [www.sqlite.org/sqlite.html](http://www.sqlite.org/sqlite.html) .

Wenn Sie eine Datei für die Verteilung mit Ihrer app zu erstellen, achten Sie mit der Benennung von Tabellen und Spalten, stellen Sie sicher, was Ihr Code erwartet, sie übereinstimmen, insbesondere dann, wenn Sie von SQLite.NET verwenden, der erwartet die Namen Ihrer c#-Klassen und Eigenschaften übereinstimmen (oder die benutzerdefinierte Attribute) zugeordnet.

Um sicherzustellen, dass Code vor allem anderen in Ihrer Android-app ausgeführt wird, kann man diese in der ersten Aktivität geladen, oder erstellen eine `Application` Unterklasse, die vor der Aktivitäten geladen wird. Der folgende Code zeigt eine `Application` Unterklasse, die eine vorhandenen Datenbankdatei kopiert **data.sqlite** aus der **Systemablagen/Raw/** Verzeichnis.

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
- [DataAccess-erweitert (Beispiel)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [Rezepte für Android-Daten](https://github.com/xamarin/recipes/tree/master/Recipes/android/data)
- [Xamarin.Forms-Datenzugriff](~/xamarin-forms/app-fundamentals/databases.md)
