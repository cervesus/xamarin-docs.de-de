---
title: Wann und wie sollte ich einen Fehlerbericht speichern?
description: In diesem Dokument wird beschrieben, wann und wie ein Fehlerbericht gespeichert wird. Außerdem werden bewährte Methoden für den Fehlerbericht bereitstellt, mit denen Entwickler das Problem am besten diagnostizieren können.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 8AD9CFBF-282A-4C1F-95E9-25F21141B052
author: davidortinau
ms.author: daortin
ms.date: 08/01/2018
ms.openlocfilehash: df00eebe682d2d06b99721a2d3c3b90d13454c75
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73014006"
---
# <a name="when-and-how-should-i-file-a-bug-report"></a>Wann und wie sollte ich einen Fehlerbericht speichern?

> [!TIP]
> Verwenden Sie das Menü Element " **Problem melden** " in Visual Studio &ndash; dadurch werden Diagnoseinformationen zusammen mit dem Fehlerbericht gesendet, um das Problem zu beheben.
>
> Es gibt ausführliche Anweisungen für [Visual Studio 2019 oder Visual Studio 2017](https://docs.microsoft.com/visualstudio/ide/how-to-report-a-problem-with-visual-studio) und [Visual Studio für Mac](https://docs.microsoft.com/visualstudio/mac/report-a-problem).
>
> Sie können auf der [Visual Studio Developer Community](https://developercommunity.visualstudio.com/) -Website nach vorhandenen Berichten suchen.

## <a name="file-a-bug-if"></a>Fehler melden, wenn...

Sie haben eine Reihe von Schritten, die von den Technikern verwendet werden können, um ein Problem zu reproduzieren.

ODER

Sie können die sichtbaren Symptome des Problems sorgfältig beschreiben, insbesondere dann, wenn Sie auch einige genaue Umstände im Zusammenhang mit dem Problem beschreiben können. <sup> [[1]](#note-1)</sup>

## <a name="best-practices-to-help-address-bugs-quickly-and-efficiently"></a>Bewährte Methoden für die schnelle und effiziente Behandlung von Fehlern

1. <a name="ref-1" />[Visual Studio Developer Community](https://developercommunity.visualstudio.com/) und das Web nach vorhandenen Fehlerberichten oder Verwendungs Vorschlägen durchsuchen, die das Problem möglicherweise direkt beheben. <sup>[[2]](#note-2)</sup><sup>[[3]](#note-3)</sup>

1. <a name="ref-2" />beschreiben Sie das Problem so eindeutig und so kurz wie möglich, einschließlich einer Beschreibung, was passiert ist und erwartet wurde.

1. <a name="ref-3" />beliebige relevante Stapel Überwachungen, Fehlermeldungs Text oder Absturz Protokolle einschließen (wenn Sie die Funktion " **Problem melden** " verwenden, können diese automatisch eingeschlossen werden). <sup>[0:](#note-4)</sup>

1. <a name="ref-4" />notieren Sie alle wichtigen Fehlermeldungen, die in Screenshots als nur-Text angezeigt werden.

1. <a name="ref-5" />enthalten einen kleinen, eigenständigen Testfall, der den Fehler mit so geringem Code wie möglich wiederherstellt.  Wenn Sie das Problem nicht mit einem brandneuen Projekt reproduzieren können (erstellt mit einer der integrierten Vorlagen), zippen Sie ein Projekt, das das Problem veranschaulicht, und fügen Sie es an den Fehlerbericht an.  Machen Sie das Beispiel Projekt so einfach wie möglich, bevor Sie es anfügen. <sup>[[5]](#note-5)</sup><sup>[[6]](#note-6)</sup>

1. <a name="ref-6" />beschreiben die Umgebung, in der der Fehler aufgetreten ist, einschließlich des Betriebssystems und der [Versionen von xamarin](~/cross-platform/troubleshooting/questions/version-logs.md) und jeglicher Abhängigkeiten.

## <a name="additional-details"></a>Weitere Details

1. <a name="note-1" />[ *^* ](#ref-1) idealerweise die Beschreibung der "sichtbaren Symptome" ausreichend Details enthalten, damit andere Kunden prüfen können, ob das gleiche Problem angezeigt wird (dieselben Fehlermeldungen, dieselbe Leistungsminderung, dieselbe Stapel Überwachung von einem Absturz _usw._ ). Für "exakte Umstände" wäre ein gutes Beispiel, wenn Sie Folgendes sagen können: "Ich habe normalerweise das Problem 75% der Zeit, aber wenn ich diese Änderung ändere, kann ich das Problem vollständig vermeiden." Ein weiteres ähnliches Beispiel für einen "präzisen Umstand" ist, wenn das Problem durch Herabstufen auf eine frühere Version von xamarin behoben wird.

1. <a name="note-2" />[ *^* ](#ref-2) wie erwartet, sind Ausschnitte aus Fehlertext (oder einem anderen eindeutig beschreibenden Text) in der Regel die besten Suchbegriffe. Wenn der vorhandene Fehlerbericht unvollständig ist, sind Sie willkommen, wenn Sie Details hinzufügen oder einen neuen, besseren Fehlerbericht hinzufügen möchten.

1. <a name="note-3" />[ *^* ](#ref-3) eine andere gute Frage ist, ob für Java-, Ziel-C-oder SWIFT-apps dasselbe Problem gemeldet wurde. Wenn dies der Fall ist, ist das Problem sehr wahrscheinlich Teil von Android oder IOS selbst und nicht Teil von xamarin.

1. <a name="note-4" />[ *^* ](#ref-4) einige Beispiele für Informationen, die enthalten sein sollen:

    1. Für Fehler, die beim Erstellen eines Projekts auftreten, fügen Sie die gesamte [Ausgabe des diagnosebuilds](~/android/troubleshooting/troubleshooting.md#Diagnostic_MSBuild_Output) in den Fehlerbericht ein.

    1. Bei Fehlern, die beim entwickeln oder Debuggen eines IOS-Projekts in Visual Studio auftreten, führen Sie die **Hilfe > xamarin > zip-Protokolle** aus, nachdem der Fehler aufgetreten ist, und schließen Sie die resultierende ZIP-Datei in den Fehlerbericht ein.

    1. Für Ausnahmen oder Abstürze in Android-oder IOS-apps müssen Sie die relevanten [Debugprotokolle für xamarin. Android-und xamarin. IOS-apps](~/cross-platform/troubleshooting/questions/version-logs.md#debug-logs-for-xamarin-apps)einschließen.

1. <a name="note-5" />[ *^* ](#ref-5) wenn möglich für ihr bestimmtes Problem, besteht eine Möglichkeit darin, das Problem erneut zu erstellen, indem Sie eine kleine Anzahl von Dateien aus der ursprünglichen Lösung zu einer neuen Projekt Mappe hinzufügen. Das xamarin-Team ist häufig in der Lage, Probleme zu untersuchen, auch bei größeren Testfällen (unter der Voraussetzung, dass die Reproduktions Schritte eindeutig erläutert werden), aber einfachere Testfälle stellen die beste Wahrscheinlichkeit dar, dass der Fehler schnell behoben wird.

1. <a name="note-6" />[ *^* ](#ref-6) wenn es _nicht_ möglich ist, das Problem zu reproduzieren, indem Sie eine kleine Anzahl von Dateien zu einer brandneuen Projekt Mappe hinzufügen, können Sie den gesamten Projektmappenordner für Ihre vollständige App komprimieren und anfügen. Löschen Sie die Ordner `bin`, `obj`, `Components`und `packages`, um die ZIP-Datei zu verkleinern. (In der IDE und im Buildprozess werden die Inhalte dieser Ordner in der Regel nach Bedarf wieder hergestellt oder neu erstellt.) Sie können auch beliebig viele Code-und Ressourcen Dateien aus dem Projekt löschen, solange die resultierende Lösung immer noch das ursprüngliche Problem veranschaulicht.
