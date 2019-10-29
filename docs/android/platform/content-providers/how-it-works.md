---
title: Funktionsweise von Inhaltsanbietern
ms.prod: xamarin
ms.assetid: B9E2EF89-7EBE-45F5-1ED9-7D2C70BE792C
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/16/2018
ms.openlocfilehash: e61be6f0189eb825c15fd75764a16706e588ebc9
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73020514"
---
# <a name="how-content-providers-work"></a>Funktionsweise von Inhaltsanbietern

Es gibt zwei Klassen, die an einer `ContentProvider` Interaktion beteiligt sind:

- **Contentprovider** &ndash; implementiert eine API, die eine Gruppe von Daten auf eine standardmäßige Weise verfügbar macht. Die Hauptmethoden sind Query, INSERT, Update und DELETE.

- **Contentresolver** &ndash; ein statischer Proxy, der mit einer `ContentProvider` kommuniziert, um auf seine Daten zuzugreifen, entweder aus derselben Anwendung oder aus einer anderen Anwendung.

Ein Inhaltsanbieter wird normalerweise von einer SQLite-Datenbank unterstützt, aber die API bedeutet, dass der verarbeitende Code keine Informationen über den zugrunde liegenden SQL-Code benötigt. Abfragen werden über einen URI durchgeführt, indem Konstanten verwendet werden, um auf Spaltennamen zu verweisen (um Abhängigkeiten von der zugrunde liegenden Datenstruktur zu verringern), und ein `ICursor` wird zurückgegeben, damit der verarbeitende Code durchlaufen werden kann.

## <a name="consuming-a-contentprovider"></a>Verwenden eines Contentprovider

`ContentProviders` ihre Funktionalität über einen URI verfügbar machen, der in der Anwendung " **androidmanifest. XML** " der Anwendung registriert ist, die die Daten veröffentlicht. Es gibt eine Konvention, bei der der URI und die Datenspalten, die verfügbar gemacht werden, als Konstanten verfügbar sein müssen, um die Bindung an die Daten zu vereinfachen. Die integrierten `ContentProviders` von Android bieten alle Hilfsklassen mit Konstanten, die auf die Datenstruktur im [`Android.Providers`](xref:Android.Provider) -Namespace verweisen.

### <a name="built-in-providers"></a>Integrierte Anbieter

Android bietet mithilfe `ContentProviders`Zugriff auf eine Vielzahl von System-und Benutzerdaten:

- *Browser* &ndash; Lesezeichen und Browserverlauf (erfordert Berechtigungen `READ_HISTORY_BOOKMARKS` und/oder `WRITE_HISTORY_BOOKMARKS`).

- *CallLog* &ndash; zuletzt ausgeführte oder mit dem Gerät empfangene Anrufe.

- *Kontakte* &ndash; ausführliche Informationen aus der Kontaktliste des Benutzers, einschließlich Personen, Smartphones, Fotos & Gruppen.

- *MediaStore* &ndash; Inhalt des Geräts des Benutzers: Audiodaten (Alben, Artists, Genres, Wiedergabelisten), Bilder (einschließlich Miniaturansichten) & Video.

- *Einstellungen* &ndash; systemweiten Geräteeinstellungen und-Einstellungen.

- *Userdictionary* &ndash; Inhalt des benutzerdefinierten Wörterbuchs, das für die Eingabe von Vorhersage Text verwendet wird.

- *Voicemail* &ndash; Verlauf von Voicemailnachrichten.

## <a name="classes-overview"></a>Übersicht über Klassen

Die primären Klassen, die beim Arbeiten mit einem `ContentProvider` verwendet werden, werden hier angezeigt:

[![Klassendiagramm der Inhaltsanbieter Anwendung und der Verwendung von Anwendungs Interaktionen](how-it-works-images/classdiagram1.png)](how-it-works-images/classdiagram1.png#lightbox)

In diesem Diagramm implementiert die `ContentProvider` Abfragen und registriert URIs, die andere Anwendungen zum Suchen von Daten verwenden. Der `ContentResolver` fungiert als ' Proxy ' für den `ContentProvider` (Abfrage-, INSERT-, Update-und Delete-Methoden). Der `SQLiteOpenHelper` enthält Daten, die vom `ContentProvider`verwendet werden, aber er ist nicht direkt für die Verwendung von apps verfügbar.
Der `CursorAdapter` übergibt den von der `ContentResolver` zurückgegebenen Cursor, um ihn in einem `ListView`anzuzeigen. Der `UriMatcher` ist eine Hilfsklasse, die bei der Verarbeitung von Abfragen URIs analysiert.

Der Zweck der einzelnen Klassen wird im folgenden beschrieben:

- **Contentprovider** &ndash; diese Methoden der abstrakten Klasse implementieren, um Daten verfügbar zu machen. Die API wird anderen Klassen und Anwendungen über das URI-Attribut zur Verfügung gestellt, das der Klassendefinition hinzugefügt wird.

- **Sqliteopenhelper** &ndash; hilft, den SQLite-Datenspeicher zu implementieren, der vom `ContentProvider`verfügbar gemacht wird.

- **Urimatcher** &ndash; `UriMatcher` in ihrer `ContentProvider`-Implementierung verwenden, um die Verwaltung von URIs zu unterstützen, die zum Abfragen des Inhalts verwendet werden.

- Von **contentresolver** &ndash; Code wird ein `ContentResolver` verwendet, um auf eine `ContentProvider` Instanz zuzugreifen. Die beiden Klassen sorgen gemeinsam für prozessübergreifende Kommunikationsprobleme, sodass Daten problemlos zwischen Anwendungen freigegeben werden können. Durch die Verwendung von Code wird nie eine `ContentProvider` Klasse expliziten erstellt; Stattdessen wird auf die Daten zugegriffen, indem ein Cursor auf der Grundlage eines URI erstellt wird, der von der `ContentProvider` Anwendung bereitgestellt wird.

- Der **CursorAdapter** &ndash; `CursorAdapter` oder `SimpleCursorAdapter` zum Anzeigen von Daten verwenden, auf die über ein `ContentProvider`zugegriffen wird.

Die `ContentProvider`-API ermöglicht es Consumern, eine Vielzahl von Vorgängen für die Daten auszuführen, z. b.:

- Abfragen von Daten, um Listen oder einzelne Datensätze zurückzugeben.
- Ändern einzelner Datensätze.
- Fügen Sie neue Datensätze hinzu.
- Löschen von Datensätzen.

Dieses Dokument enthält ein Beispiel für die Verwendung eines vom System bereitgestellten `ContentProvider`sowie ein einfaches Schreib geschütztes Beispiel, das eine benutzerdefinierte `ContentProvider`implementiert.
