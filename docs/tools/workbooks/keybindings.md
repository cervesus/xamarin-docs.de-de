---
title: Xamarin Workbooks-Editor-Tastenkombinationen
description: Dieses Dokument beschreibt die Tastenkombinationen für die Verwendung in der Xamarin Workbooks-Editor. Insbesondere überprüft er verschiedene Möglichkeiten, die die Return-Taste verwendet wird.
ms.prod: xamarin
ms.assetid: 6375A371-3215-4A7C-B97B-A19E58BE96D6
author: topgenorth
ms.author: toopge
ms.date: 03/30/2017
ms.openlocfilehash: c2b4a8c1bcb8f7b88ab2ae1e2906b1c9c702b76a
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2018
ms.locfileid: "39351677"
---
# <a name="xamarin-workbooks-editor-keyboard-shortcuts"></a>Xamarin Workbooks-Editor-Tastenkombinationen

## <a name="the-return-key-and-its-nuances"></a>Die zurück-Taste, und die Details in Bezug auf

Die folgende Tabelle beschreibt die verschiedenen tastenzuordnungen für die Ausführung von Code, und erstellen Markdown. Wir haben mit großer Sorgfalt sinnvolle und konsistente tastenzuordnungen auswählen, die bekannte und flüssig ausgeführt.

|Tastenzuordnung|Codezelle|Markdown-Zelle|
|--- |--- |--- |
|<kbd>Return</kbd>|<p>Wenn sich die Einfügemarke befindet sich am Ende des Puffers Zelle, und die Zelle erfolgreich analysiert werden kann, ausgeführt wird und Ergebnisse werden unterhalb des Puffers angezeigt werden und eine neue codezelle eingefügt werden soll, und konzentriert sich Zelle nach der ausgeführten Zelle.</p><p>Wenn die Analyse nicht erfolgreicher, wird eine neue Zeile in den Puffer eingefügt werden. Compilerdiagnose wird nicht erstellt werden, wenn die Analyse nicht erfolgreich ist.</p>|<p><kbd>Zurückgeben</kbd> ist das unterschiedliches Verhalten je nach Kontext Markdown, an der Einfügemarke.</p><ul><li>Wenn die Einfügemarke in einem Markdown-Codeblock, wird eine literale neue Zeile eingefügt.</li><li>Wenn die Einfügemarke in einem Markdown-Liste-Block, erstellen Sie ein neues Listenelement, oder Teilen Sie das aktuelle Listenelement aus.</li><li>Wenn die Einfügemarke in eine andere Art von Markdown-Block, erstellen Sie einen neuen absatzblock, oder Teilen Sie des aktuellen Codeblocks.</li></ul>|
|<dl><dt>Mac</dt><dd><kbd>Command‑Return</kbd></dd><dt>Windows-Taste</dt><dd><kbd>Control‑Return</kbd></dd></dl>|<p>Versucht immer, zu analysieren, führen Sie den Zelleninhalt. Wenn die Kompilierung erfolgreich ist, Ergebnisse (einschließlich Ausführungsausnahmen) unter den Puffer angezeigt, und wenn keine weiteren Zellen vorhanden sind, eine neue Ressourcengruppe wird erstellt und mit Fokus.</p><p>Treten Fehler bei der Kompilierung, Diagnose wird angezeigt, und der Puffer bleibt mit der Position der Einfügemarke unverändert mit Fokus.</p>|Fügt ein, und der Schwerpunkt liegt eine neue codezelle nach der aktuellen Zelle mit Markdown.|
|<dl><dt>Mac</dt><dd><kbd>Command‑Shift‑Return</kbd><dd><dt>Windows-Taste</dt><dd><kbd>Control‑Shift‑Return</kbd></dd></dl>|Fügt ein, und konzentriert sich eine neuen Markdown-Zelle nach der aktuellen Zelle.|Dasselbe Verhalten wie <kbd>zurückgeben</kbd>|
|<kbd>Shift‑Return</kbd>|Fügt immer eine neue Zeile, unabhängig von der Einfügemarke oder Inhalt ein.|Fügt einen Festplatte Zeilenumbruch innerhalb des aktuellen Codeblocks mit Markdown.|
