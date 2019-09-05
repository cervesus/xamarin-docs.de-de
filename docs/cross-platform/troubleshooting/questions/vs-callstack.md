---
title: Wie erfasse ich die aktuellen Aufruflisten des Visual Studio-Prozesses?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 64c24b09-2c4a-43ad-b94d-6cd05a1aee44
author: conceptdev
ms.author: crdun
ms.date: 03/30/2017
ms.openlocfilehash: f07bc4437293bdc49e4811c0104fd7245ccf9738
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70282945"
---
# <a name="how-do-i-collect-the-current-call-stacks-of-the-visual-studio-process"></a>Wie erfasse ich die aktuellen Aufruflisten des Visual Studio-Prozesses?

Wenn die GUI in Visual Studio gesperrt ist (hängt von der Sperre ab), ist ein wichtiger Teil der zu sammelnden Diagnoseinformationen der Satz von Aufruf Listen aller Threads des Visual Studio-Prozesses. Um diese Informationen für eine nicht reagierende Instanz von Visual Studio zu speichern, können Sie eine zweite Instanz von Visual Studio verwenden:

1. Starten Sie eine zweite Instanz (ein neues Fenster) von Visual Studio.

2. Schließen Sie alle geöffneten Projektmappen in der neuen Instanz von Visual Studio.

3. Klicken Sie auf **Debuggen > An den Prozess anhängen**.

   ![](vs-callstack-images/image1.png "Wählen Sie Debuggen > an Prozess anhängen.")

4. Wählen Sie in der Liste der `devenv.exe` **verfügbaren Prozesse**die ursprüngliche nicht reagierende Instanz von aus.

5. Wählen Sie **Debuggen > Alle abbrechen**aus.

   ![](vs-callstack-images/image2.png "Wählen Sie Debuggen > Alle abbrechen")

6. Wählen Sie **Debuggen aus, > dump speichern**unter.

   ![](vs-callstack-images/image3.png "Wählen Sie Debuggen > dump speichern unter.")

7. Ändern Sie den **Dateityp** in **\*Minidump (. dmp)** . Dadurch wird eine viel kleinere Datei als **Minidump mit Heap**erzeugt, und der Heap ist in der Regel nicht für die Diagnose von friert relevant.

   ![](vs-callstack-images/image4.png "Dadurch wird eine viel kleinere Datei als Minidump mit Heap erzeugt, und der Heap ist normalerweise nicht für die Diagnose von friert relevant.")

8. Speichern Sie die Dumpdatei. Wenn Sie die Datei online übermitteln, können Sie Sie zippen, um die Größe zu verringern.
