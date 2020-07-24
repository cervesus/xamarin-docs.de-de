---
title: Wie erfasse ich die aktuellen Aufruflisten des Visual Studio-Prozesses?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 64c24b09-2c4a-43ad-b94d-6cd05a1aee44
author: davidortinau
ms.author: daortin
ms.date: 03/30/2017
ms.openlocfilehash: aa8ce8791c598fa4891257b3d832478ecc5ee136
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86938800"
---
# <a name="how-do-i-collect-the-current-call-stacks-of-the-visual-studio-process"></a>Wie erfasse ich die aktuellen Aufruflisten des Visual Studio-Prozesses?

Wenn die GUI in Visual Studio gesperrt ist (hängt von der Sperre ab), ist ein wichtiger Teil der zu sammelnden Diagnoseinformationen der Satz von Aufruf Listen aller Threads des Visual Studio-Prozesses. Um diese Informationen für eine nicht reagierende Instanz von Visual Studio zu speichern, können Sie eine zweite Instanz von Visual Studio verwenden:

1. Starten Sie eine zweite Instanz (ein neues Fenster) von Visual Studio.

2. Schließen Sie alle geöffneten Projektmappen in der neuen Instanz von Visual Studio.

3. Wählen Sie **Debuggen > an den Prozess anhängen**.

   ![Wählen Sie Debuggen > an Prozess anhängen.](vs-callstack-images/image1.png)

4. Wählen Sie in `devenv.exe` der Liste der **verfügbaren Prozesse**die ursprüngliche nicht reagierende Instanz von aus.

5. Wählen Sie **Debuggen > alle Abbrechen**aus.

   ![Wählen Sie Debuggen > alle Abbrechen](vs-callstack-images/image2.png)

6. Wählen Sie **Debuggen aus, > Dump speichern**unter.

   ![Wählen Sie Debuggen > Dump speichern unter.](vs-callstack-images/image3.png)

7. Ändern Sie den **Dateityp** in **Minidump ( \* . dmp)**. Dadurch wird eine viel kleinere Datei als **Minidump mit Heap**erzeugt, und der Heap ist in der Regel nicht für die Diagnose von friert relevant.

   ![Dadurch wird eine viel kleinere Datei als Minidump mit Heap erzeugt, und der Heap ist normalerweise nicht für die Diagnose von friert relevant.](vs-callstack-images/image4.png)

8. Speichern Sie die Dumpdatei. Wenn Sie die Datei online übermitteln, können Sie Sie zippen, um die Größe zu verringern.
