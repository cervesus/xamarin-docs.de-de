---
title: Wann und wie sollte ich einen Fehlerbericht speichern?
description: In diesem Dokument wird beschrieben, wann, wo und wie auf einen Fehlerbericht einzureichen. Darüber hinaus Bericht "Fehlerstatus", dass die bewährte Methoden, mit die Entwickler am besten können das Problem zu diagnostizieren.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 8AD9CFBF-282A-4C1F-95E9-25F21141B052
author: conceptdev
ms.author: crdun
ms.date: 08/01/2018
ms.openlocfilehash: 1224d38a2230fa2f5c7ca08f6e33c5468886c206
ms.sourcegitcommit: 3489c281c9eb5ada2cddf32d73370943342a1082
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/18/2019
ms.locfileid: "58855210"
---
# <a name="when-and-how-should-i-file-a-bug-report"></a>Wann und wie sollte ich einen Fehlerbericht speichern?

> [!TIP]
> Verwenden der **Melden eines Problems** Menüelement in Visual Studio &ndash; sendet diese Diagnoseinformationen zusammen mit den Problembericht, um das Problem zu beheben.
>
> Es gibt ausführliche Informationen zur [2019 für Visual Studio oder Visual Studio 2017](https://docs.microsoft.com/visualstudio/ide/how-to-report-a-problem-with-visual-studio) und [Visual Studio für Mac](https://docs.microsoft.com/visualstudio/mac/report-a-problem).
>
> Sie können für vorhandene Berichte suchen, auf die [Visual Studio Developer Community](https://developercommunity.visualstudio.com/) Website.

## <a name="file-a-bug-if"></a>Datei einen Fehler aus, wenn...

Sie haben eine Reihe von Schritten Sie halten die Techniker verwenden, um ein Problem reproduzieren können.

ODER

Sie können die sichtbare Symptome des Problems sorgfältig beschreiben, insbesondere dann, wenn Sie auch die genauen Umständen im Zusammenhang mit der das Problem beschreiben können. <sup> [[1]](#note-1)</sup>

## <a name="best-practices-to-help-address-bugs-quickly-and-efficiently"></a>Bewährte Methoden zum Beheben von Fehlern schnell und effizient zu unterstützen

1. <a name="ref-1" />Suche [Visual Studio Developer Community](https://developercommunity.visualstudio.com/) und das Web für vorhandene Fehler Berichten oder Vorschläge Nutzung, die das Problem direkt behandeln können.<sup> [[2]](#note-2)</sup><sup>[[3]](#note-3)</sup>

1. <a name="ref-2" />Beschreiben Sie Problem klar und präzise wie möglich, einschließlich einer Beschreibung, was geschehen ist, und wurde als erwartet auftreten.

1. <a name="ref-3" />Umfassen alle relevanten stapelüberwachungen, die Fehlermeldungstext oder absturzprotokolle (bei Verwendung der **Melden eines Problems** Feature folgende Werte sind möglich enthalten automatisch). <sup>[[4]](#note-4)</sup>

1. <a name="ref-4" />Notieren Sie sich wichtige Fehlermeldungen, die im Screenshot Anlagen als nur-Text zu angezeigt werden.

1. <a name="ref-5" />Sind Sie einen kleineren, eigenständiges-Testfall aus, der von der Fehler mit so wenig Code wie möglich reproduziert.  Wenn das Problem mit einem neuen Projekt (mit einer der integrierten Vorlagen erstellt) nicht reproduziert werden können, komprimieren Sie dann die Sie ein Projekt, das Problem demonstriert, und fügen Sie ihn an den Problembericht.  Stellen Sie das Beispielprojekt so einfach wie möglich, bevor er angefügt wird. <sup> [[5]](#note-5)</sup><sup>[[6]](#note-6)</sup>

1. <a name="ref-6" />Beschreiben Sie die Umgebung, in dem der Fehler, einschließlich des Betriebssystems gefunden wurde, und [Versionen von Xamarin](~/cross-platform/troubleshooting/questions/version-logs.md) und Abhängigkeiten.

## <a name="additional-details"></a>Weitere Informationen

1. <a name="note-1" />[*^*](#ref-1) Im Idealfall sollte die Beschreibung der Symptome"sichtbar" genügend Details dazu enthalten, so dass andere Kunden bestätigen können, ob sie das gleiche Problem auftritt (gleichen Fehlermeldungen, gleiche leistungsbeeinträchtigung, gleiche stapelüberwachung aus einem Absturz _usw.._ ). "Genau unter Bedingungen" wäre ein gutes Beispiel, wenn Sie etwa sagen können: "Klicke ich auf das Problem normalerweise 75 % der Zeit, aber wenn ich ändern, dass diese eine Sache ich können verhindern, das Problem vollständig." Ähnlich wie ein weiteres Beispiel für eine Situation"präzise" ist, wenn das Problem ein Downgrade auf eine frühere Version von Xamarin beendet werden.

1. <a name="note-2" />[*^*](#ref-2) Wie zu erwarten ist, werden Ausschnitte Fehler (oder jeder anderen eindeutig beschreibenden Text) in der Regel die beste Suchbegriffe. Wenn der vorhandene Fehlerbericht nicht vollständig ist, sind Sie zum Hinzufügen von Details oder eine neue Datei besser Fehlerbericht Willkommen.

1. <a name="note-3" />[*^*](#ref-3) Eine weitere gute Frage ist, gibt an, ob das gleiche Problem für alle Java gemeldet wurde Objective-C oder Swift-apps. Wenn dies der Fall ist, ist das Problem wahrscheinlich Teil von Android oder iOS selbst und nicht Teil von Xamarin.

1. <a name="note-4" />[*^*](#ref-4) Einige Beispiele für Informationen anfordern:

    1. Für Fehler beim Erstellen eines Projekts, fügen Sie die vollständige [diagnostische Buildausgabe](~/android/troubleshooting/troubleshooting.md#Diagnostic_MSBuild_Output) auf den Problembericht.

    1. Für Fehler beim Erstellen oder Debuggen einer iOS-Projekt in Visual Studio, und führen Sie **Hilfe > Xamarin > Zip-Protokolle** nach Erreichen des Fehlers und die resultierende ZIP-Datei auf den Problembericht enthalten.

    1. Für Ausnahmen oder abstürzen, Android oder iOS-Apps, schließen Sie die relevanten [Debugprotokolle für Xamarin.Android und Xamarin.iOS-apps](~/cross-platform/troubleshooting/questions/version-logs.md#debug-logs-for-xamarin-apps).

1. <a name="note-5" />[*^*](#ref-5) Wenn möglich ist eine Option für Ihr bestimmtes Problem, um das Problem zu reproduzieren, indem Sie eine völlig neue Lösung eine kleine Anzahl von Dateien aus der ursprünglichen Projektmappe hinzufügen. Das Xamarin-Team kann häufig zum Untersuchen von Problemen, die auch auf größeren Testfälle (vorausgesetzt, dass die Schritte zum Reproduzieren deutlich erläutert werden), gibt jedoch einfacher von Testfällen, die die beste chance, dass der Fehler schnell behoben wird.

1. <a name="note-6" />[*^*](#ref-6) Ist dies _nicht_ möglich, wenn das Problem zu reproduzieren, indem Sie eine völlig neue Lösung eine kleine Anzahl von Dateien hinzugefügt werden, dann zippen und fügen Sie den Ordner für die gesamte Projektmappe für Ihre vollständige app. Löschen Sie die `bin`, `obj`, `Components`, und `packages` Ordner für die kleineren Datei ZIP-Datei vornehmen. (Die IDE und des Buildprozesses werden in der Regel wiederherstellen oder den Inhalt dieser Ordner bei Bedarf neu erstellen.) Sie können auch beliebig viele Code- und Dateien aus dem Projekt wie gewünscht, löschen, solange diese Lösung immer noch das ursprüngliche Problem veranschaulicht.
