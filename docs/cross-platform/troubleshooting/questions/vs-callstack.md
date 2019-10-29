---
title: Wie erfasse ich die aktuellen Aufruflisten des Visual Studio-Prozesses?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 64c24b09-2c4a-43ad-b94d-6cd05a1aee44
author: davidortinau
ms.author: daortin
ms.date: 03/30/2017
ms.openlocfilehash: 9ed79b2273758b8051a96169d4c9b53870de1fb1
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73013025"
---
# <a name="how-do-i-collect-the-current-call-stacks-of-the-visual-studio-process"></a>Wie erfasse ich die aktuellen Aufruflisten des Visual Studio-Prozesses?

Wenn die GUI in Visual Studio gesperrt ist (hängt von der Sperre ab), ist ein wichtiger Teil der zu sammelnden Diagnoseinformationen der Satz von Aufruf Listen aller Threads des Visual Studio-Prozesses. Um diese Informationen für eine nicht reagierende Instanz von Visual Studio zu speichern, können Sie eine zweite Instanz von Visual Studio verwenden:

1. Starten Sie eine zweite Instanz (ein neues Fenster) von Visual Studio.

2. Schließen Sie alle geöffneten Projektmappen in der neuen Instanz von Visual Studio.

3. Klicken Sie auf **Debuggen > An den Prozess anhängen**.

   ![](vs-callstack-images/image1.png "Select Debug > Attach to Process")

4. Wählen Sie in der Liste der **verfügbaren Prozesse**die ursprüngliche nicht reagierende Instanz von aus `devenv.exe` aus.

5. Wählen Sie **Debuggen > Alle abbrechen**aus.

   ![](vs-callstack-images/image2.png "Select Debug > Break All")

6. Wählen Sie **Debuggen aus, > dump speichern**unter.

   ![](vs-callstack-images/image3.png "Select Debug > Save Dump As")

7. Ändern Sie den **Dateityp** in **Minidump (\*. dmp)** . Dadurch wird eine viel kleinere Datei als **Minidump mit Heap**erzeugt, und der Heap ist in der Regel nicht für die Diagnose von friert relevant.

   ![](vs-callstack-images/image4.png "This will produce a much smaller file than Minidump with Heap, and the heap is usually not relevant for diagnosing freezes")

8. Speichern Sie die Dumpdatei. Wenn Sie die Datei online übermitteln, können Sie Sie zippen, um die Größe zu verringern.
