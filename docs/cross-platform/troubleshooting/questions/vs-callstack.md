---
title: 'Wie: Sammeln von der aktuellen Aufruflisten von Visual Studio-Prozess'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 64c24b09-2c4a-43ad-b94d-6cd05a1aee44
author: asb3993
ms.author: amburns
ms.date: 03/30/2017
ms.openlocfilehash: e81c28f0610a0df2e4fe06349685ef5e0744071a
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/09/2018
ms.locfileid: "33919760"
---
# <a name="how-do-i-collect-the-current-call-stacks-of-the-visual-studio-process"></a>Wie: Sammeln von der aktuellen Aufruflisten von Visual Studio-Prozess

Wenn die GUI einrichten (hängt Einfrieren) in Visual Studio gesperrt wurde, ist ein wichtiger Bestandteil von Diagnoseinformationen sammeln den Satz von Aufruflisten von allen Threads des Prozesses Visual Studio. Um diese Informationen für einen nicht reagierenden Instanz von Visual Studio zu speichern, können Sie eine zweite Instanz von Visual Studio verwenden:

1. Starten Sie eine zweite Instanz (einem neuen Fenster) von Visual Studio.

2. Schließen Sie alle geöffneten Projektmappen in der neuen Instanz von Visual Studio.

3. Klicken Sie auf **Debuggen > An den Prozess anhängen**.

  ![](vs-callstack-images/image1.png "Die Option Debuggen > an den Prozess anhängen")

4. Wählen Sie die ursprüngliche nicht reagierende Instanz des `devenv.exe` aus der Liste der **verfügbare Prozesse**.

5. Wählen Sie **Debuggen > Alle unterbrechen**.

  ![](vs-callstack-images/image2.png "Die Option Debuggen > Alle unterbrechen")

6. Wählen Sie **Debuggen > Dump speichern unter**.

  ![](vs-callstack-images/image3.png "Die Option Debuggen > Dump speichern unter")

7. Änderung **Dateityp** auf **Minidump (\*.dmp)**. Dadurch entsteht eine kleinere Datei als **Minidump mit Heap**, und der Heap ist in der Regel nicht relevant für die Diagnose von einfrieren.

  ![](vs-callstack-images/image4.png "Dadurch entsteht eine kleinere Datei als Minidump mit Heap und der Heap ist in der Regel nicht relevant für die Diagnose von Einfrieren")

8. Speichern Sie die Dumpdatei an. Wenn die Datei online übermitteln möchten, können Sie es zum Reduzieren der Größe zip.
