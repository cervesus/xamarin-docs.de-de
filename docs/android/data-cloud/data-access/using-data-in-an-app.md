---
title: Verwenden von Daten in einer Android-App
ms.prod: xamarin
ms.assetid: D5932AEB-0B6E-4F37-8B32-9BE4775AEE85
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/08/2018
ms.openlocfilehash: c0ff15c516fa2eb85ac9748004df5a14425b510c
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73023739"
---
# <a name="using-data-in-an-app"></a>Verwenden von Daten in einer App

Das **DataAccess_Adv** -Beispiel zeigt eine funktionierende Anwendung, die die Datenbankfunktionalität Benutzereingaben und CRUD (Create, Read, Update und DELETE) ermöglicht. Die Anwendung besteht aus zwei Bildschirmen: einer Liste und einem Dateneingabe Formular. Der gesamte Datenzugriffs Code kann ohne Änderung in IOS und Android wieder verwendet werden.

Nachdem Sie einige Daten hinzugefügt haben, sehen die Anwendungs Bildschirme auf Android wie folgt aus:

![Android-Beispielliste](using-data-in-an-app-images/image11.png "Android-Beispielliste")

![Android-Beispiel Details](using-data-in-an-app-images/image12.png "Android-Beispiel Details")

Das Android-Projekt wird unten dargestellt, &ndash; der in diesem Abschnitt gezeigte Code im **ORM** -Verzeichnis enthalten ist:

![Android-Projektstruktur](using-data-in-an-app-images/image14.png "Android-Projektstruktur")

Der Native UI-Code für die Aktivitäten in Android ist für dieses Dokument nicht verfügbar. Weitere Informationen zu den UI-Steuerelementen finden Sie im Handbuch zu den [Android-Listenansichten und-Adaptern](~/android/user-interface/layouts/list-view/index.md) .

## <a name="read"></a>Lesen

Das Beispiel enthält eine Reihe von Lesevorgängen:

- Lesen der Liste
- Lesen einzelner Datensätze

Die beiden Methoden in der `StockDatabase`-Klasse lauten wie folgt:

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

Android rendert die Daten als `ListView`.

## <a name="create-and-update"></a>Erstellen und aktualisieren

Um den Anwendungscode zu vereinfachen, wird eine einzelne Save-Methode bereitgestellt, die einen INSERT-oder Update-Vorgang durchführt, je nachdem, ob PrimaryKey festgelegt wurde. Da die `Id`-Eigenschaft mit einem `[PrimaryKey]`-Attribut gekennzeichnet ist, sollten Sie Sie nicht im Code festlegen. Diese Methode erkennt, ob der Wert zuvor gespeichert wurde (durch Überprüfen der Primärschlüssel Eigenschaft), und fügt das Objekt entsprechend ein:

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

Echte Anwendungen erfordern in der Regel eine gewisse Überprüfung (z. b. erforderliche Felder, minimale Längen oder andere Geschäftsregeln). Gute plattformübergreifende Anwendungen implementieren so viel von der Überprüfung logisch wie möglich in frei gegebenem Code, wobei Validierungs Fehler an die Benutzeroberfläche weitergeleitet werden, sodass Sie entsprechend den Funktionen der Plattform angezeigt werden.

## <a name="delete"></a>Löschen

Im Gegensatz zu den Methoden `Insert` und `Update` kann die `Delete<T>`-Methode nur den Primärschlüssel Wert anstelle eines kompletten `Stock` Objekts akzeptieren. In diesem Beispiel wird ein `Stock` Objekt an die-Methode weitergegeben, aber nur die ID-Eigenschaft wird an die `Delete<T>`-Methode weitergegeben.

```csharp
public int DeleteStock(Stock stock)
{
    lock (locker) {
        return Delete<Stock> (stock.Id);
    }
}
```

## <a name="using-a-pre-populated-sqlite-database-file"></a>Verwenden einer vorab aufgefüllten SQLite-Datenbankdatei

Einige Anwendungen werden mit einer Datenbank ausgeliefert, die bereits mit Daten aufgefüllt wurde. Sie können dies problemlos in Ihrer mobilen Anwendung erreichen, indem Sie eine vorhandene SQLite-Datenbankdatei mit Ihrer APP versenden und in ein beschreibbares Verzeichnis kopieren, bevor Sie darauf zugreifen. Da SQLite ein Standarddatei Format ist, das auf vielen Plattformen verwendet wird, stehen eine Reihe von Tools zum Erstellen einer SQLite-Datenbankdatei zur Verfügung:

- Der **SQLite-Manager Firefox-Erweiterungs** &ndash; funktioniert unter Mac und Windows und erzeugt Dateien, die mit IOS und Android kompatibel sind.

- &ndash; **Befehlszeile** finden Sie unter [www.sqlite.org/sqlite.html](https://www.sqlite.org/sqlite.html) .

Wenn Sie eine Datenbankdatei für die Verteilung mit Ihrer APP erstellen, achten Sie darauf, dass Tabellen und Spalten benannt werden, um sicherzustellen, dass Sie mit den Anforderungen Ihres Codes identisch sind. Dies gilt insbesondere, C# Wenn Sie sqlite.NET verwenden, der davon ausgeht, dass die Namen der Klassen und Eigenschaften entsprechen oder die zugeordneten benutzerdefinierten Attribute).

Um sicherzustellen, dass Code vor anderen in Ihrer Android-App ausgeführt wird, können Sie ihn in der ersten zu ladenden Aktivität platzieren, oder Sie können eine `Application` Unterklasse erstellen, die vor Aktivitäten geladen wird. Der folgende Code zeigt eine `Application` Unterklasse, die eine vorhandene Datenbankdatei **Data. sqlite** aus dem Verzeichnis **/Resources/RAW/** kopiert.

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
- [DataAccess (erweitert) (Beispiel)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [Android-Daten Rezepte](https://github.com/xamarin/recipes/tree/master/Recipes/android/data)
- [Xamarin. Forms-Datenzugriff](~/xamarin-forms/data-cloud/data/databases.md)
