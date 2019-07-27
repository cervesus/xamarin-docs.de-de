---
title: Funktionsweise von Inhaltsanbietern
ms.prod: xamarin
ms.assetid: B9E2EF89-7EBE-45F5-1ED9-7D2C70BE792C
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: 64a12f4f797630ad37e5821cd04a14a9d561c53e
ms.sourcegitcommit: b07e0259d7b30413673a793ebf4aec2b75bb9285
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/26/2019
ms.locfileid: "68510680"
---
# <a name="how-content-providers-work"></a>Funktionsweise von Inhaltsanbietern

Es gibt zwei Klassen, die an `ContentProvider` einer Interaktion beteiligt sind:

- **Contentprovider** &ndash; Implementiert eine API, die eine Gruppe von Daten auf eine standardmäßige Weise verfügbar macht. Die Hauptmethoden sind Query, INSERT, Update und DELETE.

- **Contentresolver** Ein statischer Proxy, der mit `ContentProvider` einem kommuniziert, um auf seine Daten zuzugreifen, entweder aus derselben Anwendung oder aus einer anderen Anwendung. &ndash;

Ein Inhaltsanbieter wird normalerweise von einer SQLite-Datenbank unterstützt, aber die API bedeutet, dass der verarbeitende Code keine Informationen über den zugrunde liegenden SQL-Code benötigt. Abfragen werden über einen URI durchgeführt, indem Konstanten verwendet werden, um auf Spaltennamen zu verweisen (um Abhängigkeiten von der zugrunde `ICursor` liegenden Datenstruktur zu verringern), und eine wird zurückgegeben, damit der verarbeitende Code durchlaufen werden kann.


## <a name="consuming-a-contentprovider"></a>Verwenden eines Contentprovider

`ContentProviders`machen Sie Ihre Funktionen über einen URI verfügbar, der in der Datei " **androidmanifest. XML** " der Anwendung registriert ist, die die Daten veröffentlicht. Es gibt eine Konvention, bei der der URI und die Datenspalten, die verfügbar gemacht werden, als Konstanten verfügbar sein müssen, um die Bindung an die Daten zu vereinfachen. `ContentProviders` Alle integrierten Android-Klassen bieten Hilfsklassen mit Konstanten, die [`Android.Providers`](xref:Android.Provider) auf die Datenstruktur im-Namespace verweisen.



### <a name="built-in-providers"></a>Integrierte Anbieter

Android bietet Zugriff auf eine Vielzahl von System-und Benutzerdaten mithilfe `ContentProviders`von:

- *Browser* Lesezeichen und Browserverlauf (erfordert `READ_HISTORY_BOOKMARKS` Berechtigungen und/ `WRITE_HISTORY_BOOKMARKS`oder). &ndash;

- *CallLog* &ndash; aktuelle Aufrufe, die mit dem Gerät durchgeführt oder empfangen wurden.

- *Kontakte* &ndash; ausführliche Informationen aus der Kontaktliste des Benutzers, einschließlich Personen, Smartphones, Fotos & Gruppen.

- *MediaStore* &ndash; Inhalt des Geräts des Benutzers: Audiodaten (Alben, Artists, Genres, Wiedergabelisten), Bilder (einschließlich Miniaturansichten) & Video.

- *Einstellungen* &ndash; systemweite Geräteeinstellungen und-Einstellungen.

- *Benutzerwörterbuch* &ndash; Inhalt des benutzerdefinierten Wörterbuchs, das für die Eingabe von Vorhersage Text verwendet wird.

- *Voicemail* &ndash; Verlauf von Voicemailnachrichten.



## <a name="classes-overview"></a>Übersicht über Klassen

Die primären Klassen, die verwendet werden, `ContentProvider` Wenn Sie mit einem arbeiten, werden hier angezeigt:

[![Klassendiagramm der Inhaltsanbieter Anwendung und Verwendung von Anwendungs Interaktionen](how-it-works-images/classdiagram1.png)](how-it-works-images/classdiagram1.png#lightbox)

In diesem Diagramm implementiert die `ContentProvider` Abfragen und registriert URIs, die andere Anwendungen zum Suchen von Daten verwenden. Der `ContentResolver` fungiert als Proxy für die `ContentProvider` (Abfrage-, INSERT-, Update-und Delete-Methoden). Die `SQLiteOpenHelper` enthält Daten `ContentProvider`, die von verwendet werden, aber Sie ist nicht direkt für die Verwendung von apps verfügbar.
Das `CursorAdapter` übergibt den Cursor, der von `ContentResolver` der zurückgegeben wird `ListView`, um in einer anzuzeigen. Ist `UriMatcher` eine Hilfsklasse, die bei der Verarbeitung von Abfragen URIs analysiert.

Der Zweck der einzelnen Klassen wird im folgenden beschrieben:

- **Contentprovider** &ndash; Implementieren Sie die Methoden dieser abstrakten Klasse, um Daten verfügbar zu machen. Die API wird anderen Klassen und Anwendungen über das URI-Attribut zur Verfügung gestellt, das der Klassendefinition hinzugefügt wird.

- **Sqliteopenhelper** Hilft bei der Implementierung des SQLite-Daten Stores, der `ContentProvider`von verfügbar gemacht wird. &ndash;

- **Urimatcher** Verwenden Sie in Ihrer`ContentProvider` -Implementierung, um URIs zu verwalten, die zum Abfragen des Inhalts verwendet werden. `UriMatcher` &ndash;

- **Contentresolver** Beim Verarbeiten von Code `ContentResolver` wird ein verwendet `ContentProvider` , um auf eine-Instanz zuzugreifen &ndash; Die beiden Klassen sorgen gemeinsam für prozessübergreifende Kommunikationsprobleme, sodass Daten problemlos zwischen Anwendungen freigegeben werden können. Durch die Verwendung von Code `ContentProvider` wird nie eine Klasse explizit erstellt; stattdessen wird auf die Daten zugegriffen, indem ein Cursor auf Grundlage eines von `ContentProvider` der Anwendung verfügbar gemachten URIs erstellt wird.

- **CursorAdapter** &ndash; `ContentProvider`Verwenden Sie`CursorAdapter` oder ,umDatenanzuzeigen,auf`SimpleCursorAdapter` die über zugegriffen wird.

Die `ContentProvider` API ermöglicht es Consumern, eine Vielzahl von Vorgängen für die Daten auszuführen, z. b.:

-  Abfragen von Daten, um Listen oder einzelne Datensätze zurückzugeben.
-  Ändern einzelner Datensätze.
-  Fügen Sie neue Datensätze hinzu.
-  Löschen von Datensätzen.

Dieses Dokument enthält ein Beispiel, in dem ein vom System `ContentProvider`bereitgestelltes verwendet wird, sowie ein einfaches Schreib geschütztes Beispiel, in `ContentProvider`dem ein benutzerdefiniertes implementiert wird.

