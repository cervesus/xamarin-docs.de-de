---
title: Wie erfasse ich die aktuellen Aufruflisten des Visual Studio-Prozesses?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 64c24b09-2c4a-43ad-b94d-6cd05a1aee44
author: asb3993
ms.author: amburns
ms.date: 03/30/2017
ms.openlocfilehash: e81c28f0610a0df2e4fe06349685ef5e0744071a
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61159121"
---
# <a name="how-do-i-collect-the-current-call-stacks-of-the-visual-studio-process"></a>Wie erfasse ich die aktuellen Aufruflisten des Visual Studio-Prozesses?

Bei die grafischen Benutzeroberfläche nach oben (Abstürze, fixiert) in Visual Studio gesperrt wurde, ist ein wichtiger Bestandteil von Diagnoseinformationen sammeln der Satz von Aufruflisten aller Threads der Visual Studio-Prozesses. Um diese Informationen für einen nicht reagierenden Instanz von Visual Studio zu speichern, können Sie eine zweite Instanz von Visual Studio verwenden:

1. Starten Sie eine zweite Instanz (ein neues Fenster) von Visual Studio.

2. Schließen Sie alle geöffneten Projektmappen in der neuen Instanz von Visual Studio.

3. Klicken Sie auf **Debuggen > An den Prozess anhängen**.

  ![](vs-callstack-images/image1.png "Die Option Debuggen > an den Prozess anhängen")

4. Wählen Sie die nicht reagierende ursprüngliche Instanz der `devenv.exe` aus der Liste der **verfügbare Prozesse**.

5. Wählen Sie **Debuggen > Alle unterbrechen**.

  ![](vs-callstack-images/image2.png "Die Option Debuggen > Alle unterbrechen")

6. Wählen Sie **Debuggen > Dump speichern unter**.

  ![](vs-callstack-images/image3.png "Die Option Debuggen > Dump speichern unter")

7. Änderung **Dateityp** zu **Minidump (\*.dmp)**. Dies erzeugt eine kleinere Datei als **Minidump mit Heap**, und der Heap ist in der Regel nicht relevant, für die Diagnose von gesperrt.

  ![](vs-callstack-images/image4.png "Dies erzeugt eine kleinere Datei als Minidump mit Heap und der Heap ist in der Regel nicht relevant, für die Diagnose von gesperrt")

8. Speichern Sie die Abbilddatei ein. Wenn die Datei online übermitteln zu können, können Sie darauf, um die Größe zu verringern zip.
