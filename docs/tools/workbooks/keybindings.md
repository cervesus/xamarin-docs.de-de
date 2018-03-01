---
title: "Tastenkombinationen für Xamarin Arbeitsmappen-Editor"
ms.topic: article
ms.prod: xamarin
ms.assetid: 6375A371-3215-4A7C-B97B-A19E58BE96D6
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.openlocfilehash: cbbf070a9c94221d98dedcd1df884a897ff7df3e
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="xamarin-workbooks-editor-keyboard-shortcuts"></a>Tastenkombinationen für Xamarin Arbeitsmappen-Editor

## <a name="the-return-key-and-its-nuances"></a>Die zurück-Taste, und seine feinen Unterschieden bei vertraut

Die folgende Tabelle beschreibt die verschiedenen tastenbindungen zum Ausführen des Codes und zum Erstellen von Markdown. Wir haben mit großen Sorgfalt aussagefähige und konsistente tastenbindungen auswählen, die vertraut sind und flüssigen werden ausgeführt.

<table>
  <tr>
    <th>Tastenkombination</th>
    <th>Code-Zelle</th>
    <th>Markdown-Zelle</th>
  </tr>
  <tr>
    <td><kbd>Return</kbd></td>
    <td>
      <p>Wenn die Einfügemarke am Ende des Puffers Zelle wird, und die Zelle erfolgreich analysiert werden kann, ausgeführt wird unterhalb des Puffers werden Ergebnisse angezeigt werden und eine neue Code Zelle eingefügt werden und sich der Fokus Zelle nach der ausgeführten Zelle.</p>
      
      <p>Wenn die Analyse nicht erfolgreich ist, wird eine neue Zeile in den Puffer eingefügt werden. Compilerdiagnose wird nicht erstellt werden, wenn die Analyse nicht erfolgreich ist.</p>
    </td>
    <td>
      <p>
        <kbd>Zurückgeben</kbd> ist das unterschiedliches Verhalten je nach Kontext Markdown an der Einfügemarke.
      </p>
      <ul>
        <li>
Wenn die Einfügemarke in einem Codeblock Markdown befindet, ist eine literal neue Zeile eingefügt.
        </li>
        <li>
Wenn die Einfügemarke in einem Block der Markdown-Liste ist, Erstellen eines neuen Listenelements, oder Teilen Sie das aktuellen Listenelement.
        </li>
        <li>
Wenn die Einfügemarke in andere Typen von Markdown-Block ist, erstellen Sie einen neuen Absatz Block, oder Teilen Sie des aktuellen Blocks.
        </li>
    </td>
  </tr>
  <tr>
    <td>
      <dl>
        <dt>Mac</dt>
        <dd><kbd>Command‑Return</kbd></dd>
        <dt>Win</dt>
        <dd><kbd>Control‑Return</kbd></dd>
      </dl>
    </td>
    <td>
      <p>Versucht immer, zu analysieren, und führen Sie den Zelleninhalt. Wenn die Kompilierung erfolgreich ist, werden Ergebnisse (einschließlich Ausführungsausnahmen) unterhalb des Puffers angezeigt wird, und wenn keine nachfolgenden Zellen vorhanden sind, ein neues Konto erstellt und mit Fokus.</p>
      
      <p>Kompilierungsfehler vorliegen, Diagnose wird angezeigt, und der Puffer bleibt mit der Position der Einfügemarke unverändert mit Fokus.</p>
    </td>
    <td>
Fügt ein und konzentriert sich eine neue Code Zelle nach der aktuellen Markdown-Zelle.
    </td>
  </tr>
  <tr>
    <td>
      <dl>
        <dt>Mac</dt>
        <dd><kbd>Command‑Shift‑Return</kbd></dd>
        <dt>Win</dt>
        <dd><kbd>Control‑Shift‑Return</kbd></dd>
      </dl>
    </td>
    <td>
Fügt ein und konzentriert sich eine neue Markdown-Zelle nach der aktuellen Zelle.
    </td>
    <td>
Dasselbe Verhalten wie <kbd>zurückgeben</kbd>
    </td>
  </tr>
  <tr>
    <td><kbd>Shift‑Return</kbd></td>
    <td>
Immer Fügt eine neue Zeile, unabhängig von der Position des Textcursors oder Inhalt.
    </td>
    <td>
Fügt einen Festplatte Zeilenumbruch innerhalb des aktuellen Markdown-Blocks.
    </td>
  </tr>
</table>
