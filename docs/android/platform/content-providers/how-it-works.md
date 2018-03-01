---
title: Wie Anbieter Arbeit
ms.topic: article
ms.prod: xamarin
ms.assetid: B9E2EF89-7EBE-45F5-1ED9-7D2C70BE792C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 142ef16606bbf47de073122791fa2509ed6b6353
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="how-content-providers-work"></a>Wie Anbieter Arbeit

Es gibt zwei Klassen Beteiligten eine `ContentProvider` Interaktion:

- **ContentProvider** &ndash; implementiert eine API, die einen Satz von Daten in ein gängiges Verfahren verfügbar macht. Die wichtigsten Methoden sind Abfragen, Insert, Update und Delete.

- **ContentResolver** &ndash; ein statischer Proxy, die Kommunikation mit einem `ContentProvider` Zugriff auf ihre Daten innerhalb derselben Anwendung oder aus einer anderen Anwendung.

Ein Inhaltsanbieter wird normalerweise durch eine SQLite-Datenbank gesichert, aber die API bedeutet, dass Code nutzen keine Informationen über die zugrunde liegende SQL. Über einen Uri Abfragen fertig sind, verwenden von Konstanten auf Spaltennamen (um Abhängigkeiten auf die zugrunde liegende Datenstruktur zu reduzieren), verweisen und eine `ICursor` wird zurückgegeben, für den verwendeten Code zu durchlaufen.

<a name="Consuming_a_ContentProvider" />

## <a name="consuming-a-contentprovider"></a>Consuming a ContentProvider

`ContentProviders` verfügbar machen ihre Funktionen über ein Uri, der in der **AndroidManifest.xml** der Anwendung, die die Daten veröffentlicht. Es liegt eine Konvention, in dem der Uri und Datenspalten, die verfügbar gemacht werden als Konstanten zu erleichtern, binden an die Daten verfügbar werden sollte. Android des integrierten `ContentProviders` alle halber Klassen bereitstellen, mit Konstanten, die sich auf die Struktur der Daten in der [ `Android.Providers` ](https://developer.xamarin.com/api/namespace/Android.Provider/) Namespace.


<a name="Built-In_Providers" />

### <a name="built-in-providers"></a>Integrierte Anbieter

Android bietet Zugriff auf eine Vielzahl von System- und Benutzer Daten mit `ContentProviders`:

- *Browser* &ndash; Lesezeichen und Browserverlauf (erfordert die Berechtigung `READ_HISTORY_BOOKMARKS` und/oder `WRITE_HISTORY_BOOKMARKS`).

- *CallLog* &ndash; letzte Anrufe vorgenommen oder mit dem Gerät empfangen.

- *Kontakte* &ndash; detaillierte Informationen aus der Liste des Benutzers wenden Sie sich an, z. B. Personen, Smartphones, Fotos und Gruppen.

- *MediaStore* &ndash; Inhalt, der dem Gerät des Benutzers: Audio (Alben, Künstler, Genres, Wiedergabelisten), (einschließlich Miniaturansichten) Bilder und Video.

- *Einstellungen* &ndash; systemweiten Einstellungen und Voreinstellungen.

- *UserDictionary* &ndash; Inhalte des Wörterbuchs benutzerdefinierte für predictive Texteingabe verwendet.

- *Voicemail* &ndash; Verlauf der Voicemail-Nachrichten.


<a name="Classes_Overview" />

## <a name="classes-overview"></a>Übersicht über Klassen

Die primären Klassen verwendet, bei der Arbeit mit einem `ContentProvider` werden hier angezeigt:

[![Klassendiagramm für Inhaltsanbieter-Anwendung und den Verbrauch Anwendungsinteraktionen](how-it-works-images/classdiagram1.png)](how-it-works-images/classdiagram1.png)

In diesem Diagramm die `ContentProvider` Abfragen implementiert und registriert die URI, der andere Anwendungen verwenden, um Daten zu suchen. Die `ContentResolver` fungiert als "Proxy", um die `ContentProvider` (Abfragen, INSERT-, Update- und Löschmethoden). Die `SQLiteOpenHelper` vom verwendete Daten enthält die `ContentProvider`, aber es wird nicht direkt verfügbar gemacht, welche apps nutzen.
Die `CursorAdapter` zurückgegebenen Cursors übergibt die `ContentResolver` anzuzeigenden eine `ListView`. Die `UriMatcher` ist eine Hilfsklasse, die URIs analysiert, bei der Verarbeitung von Abfragen.

Der Zweck der einzelnen Klassen wird im folgenden beschrieben:

- **ContentProvider** &ndash; implementiert diese abstrakte Klasse-Methoden, um Daten darzustellen. Die API wird zur Verfügung gestellt für andere Klassen und die Anwendungen über das Uri-Attribut, das die Definition der Klasse hinzugefügt wird.

- **SQLiteOpenHelper** &ndash; hilft implementieren den SQLite-Datenspeicher, der vom verfügbar gemacht wird die `ContentProvider`.

- **UriMatcher** &ndash; verwenden `UriMatcher` in Ihrer `ContentProvider` Implementierung zur Verwaltung von Uris, die zum Abfragen des Inhalts verwendet werden.

- **ContentResolver** &ndash; verwendenden Code verwendet eine `ContentResolver` für den Zugriff auf eine `ContentProvider` Instanz. Die beiden Klassen kümmern zusammen die prozessübergreifende Kommunikationsprobleme, da Daten problemlos zwischen Anwendungen freigegeben werden. Nutzen der Code nie erstellt eine `ContentProvider` -Klasse explizit; stattdessen erfolgt die Daten durch Erstellen eines Cursors basierend auf einen Uri verfügbar gemacht werden, indem die `ContentProvider` Anwendung.

- **CursorAdapter** &ndash; verwenden `CursorAdapter` oder `SimpleCursorAdapter` zum Anzeigen von Daten erfolgt über eine `ContentProvider`.

Die `ContentProvider` -API ermöglicht es Consumern, wie z. B. eine Vielzahl von Vorgängen an den Daten ausführen:

-  Abfragen von Daten, um Listen oder einzelne Datensätze zurückzugeben.
-  Ändern Sie die einzelnen Datensätze.
-  Neue Datensätze hinzufügen.
-  Löschen von Datensätzen.

Dieses Dokument enthält ein Beispiel, eine vom System vorgegebene verwendet `ContentProvider`, sowie ein einfaches Beispiel ohne Schreibzugriff, die eine benutzerdefinierte implementiert `ContentProvider`.

