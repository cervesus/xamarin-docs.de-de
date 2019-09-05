---
title: Tastenkombinationen für den Xamarin Workbooks-Editor
description: In diesem Dokument werden die für die Verwendung im Xamarin Workbooks-Editor verfügbaren Tastenkombinationen beschrieben. Insbesondere werden verschiedene Möglichkeiten zum Verwenden der Rückgabetaste untersucht.
ms.prod: xamarin
ms.assetid: 6375A371-3215-4A7C-B97B-A19E58BE96D6
author: conceptdev
ms.author: crdun
ms.date: 03/30/2017
ms.openlocfilehash: 9904d0f9fb1acfc3c3c197b9881c2add00aba534
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70285324"
---
# <a name="xamarin-workbooks-editor-keyboard-shortcuts"></a>Tastenkombinationen für den Xamarin Workbooks-Editor

## <a name="the-return-key-and-its-nuances"></a>Der Rückgabe Schlüssel und seine Nuancen

In der folgenden Tabelle werden die verschiedenen Tastenkombinationen zum Ausführen von Code und zum Erstellen von markdown beschrieben. Wir haben sehr sorgfältig darauf geachtet, sinnvolle und konsistente Tastenkombinationen auszuwählen, die sowohl vertraut als auch flüssig sind.

|Schlüsselbindung|Codezelle|Markdown-Zelle|
|--- |--- |--- |
|<kbd>Return</kbd>|<p>Wenn sich die Einfügemarke am Ende des Zellen Puffers befindet und die Zelle erfolgreich analysiert werden kann, wird Sie ausgeführt, und die Ergebnisse werden unterhalb des Puffers angezeigt, und nach der ausgeführten Zelle wird eine neue codezelle eingefügt und eine fokussierte Zelle eingefügt.</p><p>Wenn die Verarbeitung nicht erfolgreich ist, wird eine neue Zeile in den Puffer eingefügt. Die Compilerdiagnose wird nicht erstellt, wenn die Verarbeitung nicht erfolgreich ist.</p>|<p><kbd>Gibt</kbd> abhängig vom markdown-Kontext am Caretzeichen ein anderes Verhalten aus.</p><ul><li>Wenn sich die Einfügemarke in einem markdown-Codeblock befindet, wird eine Literale neue Zeile eingefügt.</li><li>Wenn sich die Einfügemarke in einem markdown-Listen Block befindet, erstellen Sie ein neues Listenelement, oder Teilen Sie das aktuelle Listenelement.</li><li>Wenn die Einfügemarke in einem anderen Typ von markdown-Block vorhanden ist, erstellen Sie einen neuen Absatz Block, oder Teilen Sie den aktuellen Block auf.</li></ul>|
|<dl><dt>Mac</dt><dd><kbd>Command‑Return</kbd></dd><dt>Erringen</dt><dd><kbd>Control‑Return</kbd></dd></dl>|<p>Versucht immer, den Zellen Inhalt zu analysieren und auszuführen. Wenn die Kompilierung erfolgreich ist, werden die Ergebnisse (einschließlich der Ausführungs Ausnahmen) unterhalb des Puffers angezeigt, und wenn keine nachfolgenden Zellen vorhanden sind, wird eine neue erstellt und konzentriert.</p><p>Wenn Kompilierungsfehler vorliegen, wird die Diagnose angezeigt, und der Puffer bleibt mit dem Fokus der Position der Einfügemarke unverändert.</p>|Fügt eine neue codezelle nach der aktuellen markdown-Zelle ein und legt diese fest.|
|<dl><dt>Mac</dt><dd><kbd>Command‑Shift‑Return</kbd><dd><dt>Erringen</dt><dd><kbd>Control‑Shift‑Return</kbd></dd></dl>|Fügt eine neue markdown-Zelle nach der aktuellen Zelle ein und legt diese fest.|Gleiches Verhalten wie bei <kbd>Rückgabe</kbd>|
|<kbd>Shift‑Return</kbd>|Fügt immer eine neue Zeile ein, unabhängig von der Position oder dem Inhalt der Einfügemarke.|Fügt einen harten Zeilenumbruch innerhalb des aktuellen markdown-Blocks ein.|
