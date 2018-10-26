---
title: Wie Anbieter arbeiten
ms.prod: xamarin
ms.assetid: B9E2EF89-7EBE-45F5-1ED9-7D2C70BE792C
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: df4c2e10e34c308e4fadb44fba9c6a14714ae1b9
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50108937"
---
# <a name="how-content-providers-work"></a>Wie Anbieter arbeiten

Es gibt zwei Klassen beteiligt eine `ContentProvider` Interaktion:

- **ContentProvider** &ndash; implementiert eine API, die einen Satz von Daten in eine standardisierte Möglichkeit verfügbar macht. Die wichtigsten Methoden sind Abfragen, INSERT-, Update- und Delete.

- **ContentResolver** &ndash; ein statischer Proxy, mit denen kommuniziert eine `ContentProvider` auf ihre Daten, entweder innerhalb derselben Anwendung oder aus einer anderen Anwendung zugreifen.

Inhaltsanbieter wird normalerweise durch eine SQLite-Datenbank gesichert, aber die API bedeutet, dass die Nutzung von Code, nicht unbedingt die zugrunde liegende SQL vertraut sind. Abfragen werden, erfolgt über einen Uri mit Konstanten auf Spaltennamen (um die Abhängigkeiten für die zugrunde liegende Datenstruktur zu reduzieren), und ein `ICursor` wird zurückgegeben, für den konsumierenden Code durchlaufen.


## <a name="consuming-a-contentprovider"></a>Verwenden von ContentProvider

`ContentProviders` Machen Sie ihre Funktionen über ein Uri, der in registriert wird die **"androidmanifest.xml"** der Anwendung, die die Daten veröffentlicht. Es ist eine Konvention, in dem der Uri und Datenspalten, die verfügbar gemacht werden als Konstanten zu erleichtern, binden an die Daten verfügbar werden sollen. Android die integrierten `ContentProviders` bieten Klassen von der Einfachheit halber mit Konstanten, die die Datenstruktur in verweisen auf die [ `Android.Providers` ](https://developer.xamarin.com/api/namespace/Android.Provider/) Namespace.



### <a name="built-in-providers"></a>Integrierte Anbieter

Android bietet Zugriff auf eine Vielzahl von System und Benutzer mit `ContentProviders`:

- *Browser* &ndash; Lesezeichen und Browserverlauf (erfordert die Berechtigung `READ_HISTORY_BOOKMARKS` und/oder `WRITE_HISTORY_BOOKMARKS`).

- *CallLog* &ndash; aktuelle Aufrufe vorgenommen oder mit dem Gerät empfangen.

- *Kontakte* &ndash; detaillierte Informationen aus der Liste des Benutzers wenden Sie sich an, z. B. Personen, Smartphones, Fotos und Gruppen.

- *MediaStore* &ndash; Inhalt, der das Gerät des Benutzers: Audio (Alben, Künstler, Genres, Wiedergabelisten), (einschließlich Miniaturansichten) Bilder und Videos.

- *Einstellungen* &ndash; systemweiten Einstellungen und Voreinstellungen.

- *UserDictionary* &ndash; Inhalt des benutzerdefinierten Wörterbuchs für die Textvorhersage Eingabe verwendet.

- *Voicemail* &ndash; Verlauf der Mailbox-Nachrichten.



## <a name="classes-overview"></a>Übersicht über Klassen

Primären Klassen verwendet, bei der Arbeit mit einem `ContentProvider` werden hier angezeigt:

[![Klassendiagramm des Inhaltsanbieter-Anwendung und Consuming Anwendungsinteraktionen](how-it-works-images/classdiagram1.png)](how-it-works-images/classdiagram1.png#lightbox)

In diesem Diagramm die `ContentProvider` Abfragen implementiert und registriert die URIS, die andere Anwendungen verwenden, um Daten zu suchen. Die `ContentResolver` fungiert als 'Proxy', um die `ContentProvider` (Abfragen, INSERT-, Update- und Delete-Methoden). Die `SQLiteOpenHelper` enthält Daten, die von verwendet die `ContentProvider`, aber es ist nicht direkt mit der Nutzung von apps verfügbar.
Die `CursorAdapter` zurückgegebenen Cursors übergibt die `ContentResolver` anzuzeigenden eine `ListView`. Die `UriMatcher` ist eine Hilfsklasse, die URIs analysiert, bei der Verarbeitung von Abfragen.

Der Zweck jeder Klasse lautet wie folgt:

- **ContentProvider** &ndash; Implementierung dieser abstrakten Klasse-Methoden, um die Daten verfügbar zu machen. Die API wird verfügbar gemacht zu anderen Klassen und die Anwendungen über das Uri-Attribut, das die Definition der Klasse hinzugefügt wird.

- **SQLiteOpenHelper** &ndash; hilft implementieren den SQLite-Datenspeicher, die verfügbar gemacht werden die `ContentProvider`.

- **UriMatcher** &ndash; verwenden `UriMatcher` in Ihre `ContentProvider` Implementierung zur Verwaltung von Uris, die zum Abfragen des Inhalts verwendet werden.

- **ContentResolver** &ndash; Nutzen von Code verwendet ein `ContentResolver` für den Zugriff auf eine `ContentProvider` Instanz. Die beiden Klassen sorgt zusammen die prozessübergreifende Kommunikationsprobleme auf, sodass Daten einfach zwischen Anwendungen gemeinsam genutzt werden. Nutzen von Code nie erstellt eine `ContentProvider` Klasse explizit; stattdessen erfolgt die Daten durch Erstellen eines Cursors auf Grundlage eines Uri verfügbar gemacht werden, indem die `ContentProvider` Anwendung.

- **CursorAdapter** &ndash; verwenden `CursorAdapter` oder `SimpleCursorAdapter` zum Anzeigen von Daten, die auf das über eine `ContentProvider`.

Die `ContentProvider` -API ermöglicht es Consumern, die eine Vielzahl von Vorgängen an den Daten, wie z. B. ausführen:

-  Abfragen von Daten, um Listen oder die einzelnen Datensätze zurückzugeben.
-  Ändern Sie die einzelnen Datensätze.
-  Neue Datensätze hinzufügen.
-  Löschen von Datensätzen.

Dieses Dokument enthält ein Beispiel, eine vom System bereitgestellte verwendet `ContentProvider`, sowie ein einfaches Beispiel für nur-Lese, die eine benutzerdefinierte implementiert `ContentProvider`.

