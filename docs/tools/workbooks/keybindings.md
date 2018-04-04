---
title: Tastenkombinationen für Xamarin Arbeitsmappen-Editor
ms.prod: xamarin
ms.assetid: 6375A371-3215-4A7C-B97B-A19E58BE96D6
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.openlocfilehash: 62cd8fd546667643ca90098f63866ab1ca6c4e99
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="xamarin-workbooks-editor-keyboard-shortcuts"></a>Tastenkombinationen für Xamarin Arbeitsmappen-Editor

## <a name="the-return-key-and-its-nuances"></a>Die zurück-Taste, und seine feinen Unterschieden bei vertraut

Die folgende Tabelle beschreibt die verschiedenen tastenbindungen zum Ausführen des Codes und zum Erstellen von Markdown. Wir haben mit großen Sorgfalt aussagefähige und konsistente tastenbindungen auswählen, die vertraut sind und flüssigen werden ausgeführt.

|Tastenkombination|Code-Zelle|Markdown-Zelle|
|--- |--- |--- |
|<kbd>Return</kbd>|<p>Wenn die Einfügemarke am Ende des Puffers Zelle wird, und die Zelle erfolgreich analysiert werden kann, ausgeführt wird unterhalb des Puffers werden Ergebnisse angezeigt werden und eine neue Code Zelle eingefügt werden und sich der Fokus Zelle nach der ausgeführten Zelle.</p><p>Wenn die Analyse nicht erfolgreich ist, wird eine neue Zeile in den Puffer eingefügt werden. Compilerdiagnose wird nicht erstellt werden, wenn die Analyse nicht erfolgreich ist.</p>|<p><kbd>Zurückgeben</kbd> ist das unterschiedliches Verhalten je nach Kontext Markdown an der Einfügemarke.</p><ul><li>Wenn die Einfügemarke in einem Codeblock Markdown befindet, ist eine literal neue Zeile eingefügt.</li><li>Wenn die Einfügemarke in einem Block der Markdown-Liste ist, Erstellen eines neuen Listenelements, oder Teilen Sie das aktuellen Listenelement.</li><li>Wenn die Einfügemarke in andere Typen von Markdown-Block ist, erstellen Sie einen neuen Absatz Block, oder Teilen Sie des aktuellen Blocks.</li></ul>|
|<dl><dt>Mac</dt><dd><kbd>Command‑Return</kbd></dd><dt>Win</dt><dd><kbd>Control‑Return</kbd></dd></dl>|<p>Versucht immer, zu analysieren, und führen Sie den Zelleninhalt. Wenn die Kompilierung erfolgreich ist, werden Ergebnisse (einschließlich Ausführungsausnahmen) unterhalb des Puffers angezeigt wird, und wenn keine nachfolgenden Zellen vorhanden sind, ein neues Konto erstellt und mit Fokus.</p><p>Kompilierungsfehler vorliegen, Diagnose wird angezeigt, und der Puffer bleibt mit der Position der Einfügemarke unverändert mit Fokus.</p>|Fügt ein und konzentriert sich eine neue Code Zelle nach der aktuellen Markdown-Zelle.|
|<dl><dt>Mac</dt><dd><kbd>Command‑Shift‑Return</kbd><dd><dt>Win</dt><dd><kbd>Control‑Shift‑Return</kbd></dd></dl>|Fügt ein und konzentriert sich eine neue Markdown-Zelle nach der aktuellen Zelle.|Dasselbe Verhalten wie <kbd>zurückgeben</kbd>|
|<kbd>Shift‑Return</kbd>|Immer Fügt eine neue Zeile, unabhängig von der Position des Textcursors oder Inhalt.|Fügt einen Festplatte Zeilenumbruch innerhalb des aktuellen Markdown-Blocks.|
