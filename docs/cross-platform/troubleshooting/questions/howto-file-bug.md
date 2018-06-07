---
title: Wann und wie sollte ich einen Fehlerbericht Datei?
description: Dieses Dokument wird beschrieben, wann, wo und wie einen Fehlerbericht Datei. Darüber hinaus Bericht "Fehlerstatus", dass die bewährte Methoden, die am besten Engineers ermöglichen das Problem zu diagnostizieren.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 8AD9CFBF-282A-4C1F-95E9-25F21141B052
author: asb3993
ms.author: amburns
ms.openlocfilehash: 08a782e9637442a43e9c63305ddf161403519169
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34781942"
---
# <a name="when-and-how-should-i-file-a-bug-report"></a>Wann und wie sollte ich einen Fehlerbericht Datei?


Fehler in der hier für die Xamarin Bugzilla Fehler Tracker-Datei: [ https://bugzilla.xamarin.com/enter_bug.cgi?classification=__all ](https://bugzilla.xamarin.com/enter_bug.cgi?classification=__all).

## <a name="file-a-bug-if"></a>Melden Sie Fehler, wenn...


Sie haben eine Reihe von Schritten, die Sie glauben, dass der Xamarin-Ingenieure werden können, verwenden Sie zum Reproduzieren eines Problems, das vom Xamarin verursacht wird.

OR

Insbesondere, wenn Sie auch präzise Umständen im Zusammenhang mit der das Problem beschreiben können, können Sie sorgfältig die sichtbaren Symptome das Problem beschreiben. <sup> [[1]](#note-1)</sup>


## <a name="best-practices-to-help-xamarin-address-bugs-quickly-and-efficiently"></a>Bewährte Methoden zum Beheben von Fehlern Xamarin schnell und effizient zu unterstützen


1. <a name="ref-1" />Suche [Bugzilla](https://bugzilla.xamarin.com/query.cgi?format=specific&amp;bug_status=__all__) "und" für vorhandene Berichte oder Nutzung Vorschläge, die direkt Behandlung des Problems möglicherweise Fehler.<sup> [[2]](#note-2)</sup><sup>[[3]](#note-3)</sup>

1. <a name="ref-2" />Führen Sie die [Fehler, die Schreiben von Richtlinien](https://bugzilla.xamarin.com/page.cgi?id=bug-writing.html) um das Problem zu beschreiben, wie deutlich und deutlicher wie möglich, einschließlich einer Beschreibung, was geschehen ist, und wurde erwartet, durchgeführt werden soll.

1. <a name="ref-3" />Alle relevanten auftretenden Fehlern stapelverfolgungen, die Fehlermeldungstext enthalten oder absturzprotokolle. <sup>[[4]](#note-4)</sup>

1. <a name="ref-4" />Notieren Sie alle wichtigen Fehlermeldungen, die im Screenshot Anlagen als nur-Text zu angezeigt werden.

1. <a name="ref-5" />Schließen Sie einen kleinen, eigenständigen Testfall, der den Fehler mit so wenig Code wie möglich reproduziert.  Wenn Sie das Problem mit einem neuen Projekt (erstellt mit einer der integrierten Vorlagen) können nicht reproduzieren, dann zippen Sie ein Projekt, das das entsprechende Problem demonstriert, und fügen Sie es an den Problembericht.  Stellen Sie das Beispielprojekt so einfach wie möglich vor dem Anfügen. <sup> [[5]](#note-5)</sup><sup>[[6]](#note-6)</sup>

1. <a name="ref-6" />Beschreiben Sie die Umgebung, in dem der Fehler ist, einschließlich des Betriebssystems aufgetreten, und [Versionen von Xamarin](~/cross-platform/troubleshooting/questions/version-logs.md) und alle Abhängigkeiten.

---

## <a name="additional-details"></a>Weitere Informationen

1. <a name="note-1" />[*^*](#ref-1) Im Idealfall sollte die Beschreibung der Symptome"sichtbar" genügend Details enthalten, sodass andere Kunden überprüfen können, ob sie das gleiche Problem vorliegen (gleichen Fehlermeldungen, dieselbe Leistungseinbußen, dieselbe stapelüberwachung aus einem Absturz _usw.._ ). "Präzise unter Umständen", ein gutes Beispiel wäre, wenn Sie etwa sagen können: "ich erreicht das Problem normalerweise 75 % der Zeit, aber wenn ich dieses aufpassen ändern klicken Sie dann ich können Problem vermeiden, die vollständig." Ähnlich wie ein weiteres Beispiel für eine "präzise Situation" ist das Problem Downgrade auf eine frühere Version von Xamarin beendet werden.

1. <a name="note-2" />[*^*](#ref-2) Wie zu erwarten, werden Ausschnitte Fehler (oder eine beliebige andere eindeutig beschreibenden Text) in der Regel die beste Suchbegriffe. Wenn der vorhandene Fehlerbericht nicht vollständig ist, können Sie Details hinzufügen oder eine neue Datei, eine bessere Leistung bug Bericht Willkommen.

1. <a name="note-3" />[*^*](#ref-3) Eine weitere gute Frage ist, ob das gleiche Problem für alle Java gemeldet wurde Objective-C oder Swift-apps. Wenn dies der Fall ist, ist das Problem sehr wahrscheinlich Teil Android oder iOS selbst und nicht als Teil der Xamarin.

1. <a name="note-4" />[*^*](#ref-4) Einige Beispiele von Informationen enthalten:

    1. Für Fehler beim Erstellen eines Projekts geben Sie auch die vollständige [diagnostische Buildausgabe](~/android/troubleshooting/troubleshooting.md#Diagnostic_MSBuild_Output) auf den Problembericht.
    
    1. Führen Sie nach Fehlern, die auftreten, bei der Erstellung oder ein iOS-Projekt in Visual Studio debuggen, _Hilfe > Xamarin > Zip Protokolle_ nach Erreichens des Fehlers, und schließen Sie die resultierende ZIP-Datei auf den Problembericht.
    
    1. Für Ausnahmen oder Abstürzen in Android oder iOS-apps, enthalten Sie bitte den entsprechenden "[Debugprotokolle für Xamarin.Android und Xamarin.iOS apps](~/cross-platform/troubleshooting/questions/version-logs.md#debug-logs-for-xamarin-apps)."

1. <a name="note-5" />[*^*](#ref-5) Nach Möglichkeit ist eine hervorragende Möglichkeit für Ihre bestimmte Problem das Problem neu erstellen, indem Sie eine kleine Anzahl von Dateien aus der ursprünglichen Projektmappe in eine vollkommen neue Projektmappe hinzufügen. Das Team Xamarin werden häufig zum Untersuchen von Problemen gilt auch für größere Testfälle (vorausgesetzt, die Schritte zur Reproduktion deutlich ausführlich erläutert werden), gibt jedoch einfacher von Testfällen, die am besten chance, dass der Fehler wird schneller gelöst werden, können.


1. <a name="note-6" />[*^*](#ref-6) Ist er _nicht_ möglich, wenn das Problem zu reproduzieren, indem Sie eine kleine Anzahl von Dateien zu einer neuen Projektmappe hinzufügen, dann komprimieren und die gesamte Projektmappenordner für die vollständige app anfügen. Löschen Sie die `bin`, `obj`, `Components`, und `packages` Ordner die ZIP-Datei kleinere Datei vornehmen. (Die IDE und während des Erstellungsprozesses werden in der Regel wiederherstellen oder den Inhalt dieser Ordner nach Bedarf neu erstellen.) Sie können auch so viele Code und Resource-Dateien aus dem Projekt wie gewünscht, löschen, solange sich so ergebende Lösung weiterhin die ursprüngliche entsprechende Problem demonstriert.

