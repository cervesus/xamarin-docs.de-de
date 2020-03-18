---
title: Funktionsweise von Inhaltsanbietern
ms.prod: xamarin
ms.assetid: B9E2EF89-7EBE-45F5-1ED9-7D2C70BE792C
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/16/2018
ms.openlocfilehash: e61be6f0189eb825c15fd75764a16706e588ebc9
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2020
ms.locfileid: "73020514"
---
# <a name="how-content-providers-work"></a>Funktionsweise von Inhaltsanbietern

An einer `ContentProvider`-Interaktion sind zwei Klassen beteiligt:

- **ContentProvider** &ndash; implementiert eine API, die eine Reihe von Daten auf übliche Weise verfügbar macht. Die wichtigsten Methoden sind Query, Insert, Update und Delete.

- **ContentResolver** &ndash; ein statischer Proxy, der mit einem `ContentProvider` kommuniziert, um auf dessen Daten zuzugreifen, entweder innerhalb derselben Anwendung oder aus einer anderen Anwendung.

Ein Inhaltsanbieter wird in der Regel durch eine SQLite-Datenbank unterstützt, durch die API muss der nutzende Code den zugrunde liegenden SQL-Code jedoch nicht kennen. Abfragen über einen URI verwenden Konstanten zum Verweisen auf Spaltennamen (um die Abhängigkeiten in der zugrunde liegenden Datenstruktur zu reduzieren), und es wird ein `ICursor` zurückgegeben, den der nutzende Code durchlaufen kann.

## <a name="consuming-a-contentprovider"></a>Nutzen eines ContentProvider

`ContentProviders` machen ihre Funktionalität über einen URI verfügbar, der in der **AndroidManifest.xml**-Datei der Anwendung registriert ist, die die Daten veröffentlicht. Es gibt eine Konvention, der zufolge der URI und die Datenspalten als Konstanten verfügbar gemacht werden sollten, um die Bindung an die Daten zu vereinfachen. Alle integrierten `ContentProviders` von Android stellen Hilfsklassen mit Konstanten bereit, die auf die Datenstruktur im Namespace [`Android.Providers`](xref:Android.Provider) verweisen.

### <a name="built-in-providers"></a>Integrierte Anbieter

Android bietet über `ContentProviders` Zugriff auf eine umfassende Palette an System- und Benutzerdaten:

- *Browser* &ndash; Lesezeichen und Browserverlauf (erfordert die Berechtigung `READ_HISTORY_BOOKMARKS` und/oder `WRITE_HISTORY_BOOKMARKS`).

- *CallLog* &ndash; kürzlich mit dem Gerät getätigte oder empfangene Anrufe.

- *Contacts* &ndash; detaillierte Informationen aus der Kontaktliste des Benutzers, einschließlich Personen, Smartphones, Fotos und Gruppen.

- *MediaStore* &ndash; Inhalte des Geräts eines Benutzers: Audioinhalte (Alben, Künstler, Genres, Playlists), Bilder (einschließlich Miniaturbildern) und Videoinhalte.

- *Settings* &ndash; systemweite Geräteeinstellungen.

- *UserDictionary* &ndash; Inhalte des benutzerdefinierten Wörterbuchs, das zur Texteingabe mit Wortvorschlägen verwendet wird.

- *Voicemail* &ndash; Verlauf von Voicemailnachrichten.

## <a name="classes-overview"></a>Übersicht über Klassen

Im Folgenden finden Sie die wichtigsten Klassen für die Arbeit mit einem `ContentProvider`:

[![Klassendiagramm der Inhaltsanbieteranwendung und der Interaktionen der nutzenden Anwendung](how-it-works-images/classdiagram1.png)](how-it-works-images/classdiagram1.png#lightbox)

In diesem Diagramm implementiert der `ContentProvider` Abfragen und registriert URIs, die andere Anwendungen zum Auffinden von Daten verwenden. Der `ContentResolver` fungiert als „Proxy“ zum `ContentProvider` (Methoden: „Query“, „Insert“, „Update“ und „Delete“). Der `SQLiteOpenHelper` enthält Daten, die vom `ContentProvider` verwendet werden, ist für nutzende Apps aber nicht direkt verfügbar.
Der `CursorAdapter` übergibt den vom `ContentResolver` zurückgegebenen Cursor, um Inhalte in einer `ListView` anzuzeigen. Der `UriMatcher` ist eine Hilfsklasse, die bei der Verarbeitung von Abfragen URIs analysiert.

Im Folgenden wird der Zweck der einzelnen Klassen beschrieben:

- **ContentProvider** &ndash; implementiert die Methoden dieser abstrakten Klasse, um Daten verfügbar zu machen. Die API wird über das URI-Attribut, das der Klassendefinition hinzugefügt wird, für andere Klassen und Anwendungen zur Verfügung gestellt.

- **SQLiteOpenHelper** &ndash; hilft bei der Implementierung des SQLite-Datenspeichers, der vom `ContentProvider` verfügbar gemacht wird.

- **UriMatcher** &ndash; verwenden Sie `UriMatcher` in Ihrer `ContentProvider`-Implementierung, um URIs zu verwalten, die zum Abfragen des Inhalts verwendet werden.

- **ContentResolver** &ndash; der nutzende Code verwendet einen `ContentResolver` für den Zugriff auf eine `ContentProvider`-Instanz. Diese beiden Klassen verarbeiten zusammen die Kommunikation zwischen den Prozessen, sodass Daten einfacher von mehreren Anwendungen gemeinsam genutzt werden können. Der nutzende Code erstellt niemals explizit eine `ContentProvider`-Klasse; stattdessen erfolgt der Datenzugriff durch die Erstellung eines Cursors basierend auf einem URI, der von der `ContentProvider`-Anwendung verfügbar gemacht wird.

- **CursorAdapter** &ndash; verwenden Sie `CursorAdapter` oder `SimpleCursorAdapter`, um Daten anzuzeigen, auf die über einen `ContentProvider` zugegriffen wird.

Über die `ContentProvider`-API können Consumer eine Vielzahl von Vorgängen mit den Daten ausführen, wie z. B. die folgenden:

- Abfragen von Daten für die Rückgabe von Listen oder einzelnen Datensätzen
- Ändern von einzelnen Datensätzen
- Hinzufügen von neuen Datensätzen
- Löschen von Datensätzen

Dieses Dokument enthält ein Beispiel, das einen systemseitig bereitgestellten `ContentProvider` verwendet, sowie ein einfaches, schreibgeschütztes Beispiel, das einen benutzerdefinierten `ContentProvider` implementiert.
