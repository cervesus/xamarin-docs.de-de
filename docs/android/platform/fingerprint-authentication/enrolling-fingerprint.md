---
title: Registrieren eines Fingerabdrucks
description: 'Gewusst wie: Festlegen eine bildschirmsperren einrichten und registrieren ein Fingerabdrucks auf einem Android-Gerät oder Emulator.'
ms.prod: xamarin
ms.assetid: 52092F63-00EE-4F8B-A49F-65C9CCBA7EF2
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: 18903a7d8f6c4033dc3ac7c4c0187a247d023bc1
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61027011"
---
# <a name="enrolling-a-fingerprint"></a>Registrieren eines Fingerabdrucks

## <a name="enrolling-a-fingerprint-overview"></a>Eine Übersicht über die per Fingerabdruck registrieren

Es ist nur möglich, dass eine Android-Anwendung Authentifizierung per Fingerabdruck zu nutzen, wenn das Gerät bereits mit der Authentifizierung per Fingerabdruck konfiguriert wurde. Diesem Leitfaden wird zum Registrieren eines Fingerabdrucks auf einem Android-Gerät oder Emulator erläutert werden. Emulatoren müssen sich nicht auf die tatsächliche Hardware einen Fingerabdruck-Scan ausführen, aber es ist möglich, einen Fingerabdruck-Scan mithilfe von Android Debug Bridge (siehe unten) zu simulieren.  Dieses Handbuch wird beschrieben, wie Bildschirmsperre auf einem Android-Gerät zu aktivieren und zum Registrieren eines Fingerabdrucks für die Authentifizierung.

## <a name="requirements"></a>Anforderungen

Um einen Fingerabdruck registrieren zu können, müssen Sie ein Android-Gerät oder einen Emulator mit API-Ebene 23 (Android 6.0) verfügen.

Die Verwendung von der Android Debug Bridge (ADB) ist es erforderlich, mit der Befehlszeile aus, und die **Adb** ausführbare Datei muss im Pfad des Ihrer Bash, PowerShell oder Command Prompt-Umgebung.

## <a name="configuring-a-screen-lock-and-enrolling-a-fingerprint"></a>Konfigurieren einen Bildschirmsperren, und Registrieren eines Fingerabdrucks 

Um eine bildschirmsperren einzurichten, führen Sie die folgenden Schritte aus:

1. Wechseln Sie zu **Einstellungen > Sicherheit**, und wählen Sie **Sperrbildschirm**:

    ![Speicherort der Auswahl der Bildschirm Sperren auf dem Bildschirm Sicherheit](enrolling-fingerprint-images/testing-01.png)

2. Im nächste Bildschirm, der angezeigt wird können Sie auswählen und konfigurieren Sie eine der Bildschirm sperren Sicherheitsmethoden: 

    ![Wählen Sie Wischen, Muster, PIN oder Kennwort](enrolling-fingerprint-images/testing-02.png)

   Wählen Sie aus, und führen Sie eine der Methoden zur Verfügung Bildschirm sperren.

3. Sobald die Screenlock konfiguriert ist, zurück zu den **Einstellungen > Sicherheit** Seite und wählen Sie **Fingerabdruck**:

    ![Speicherort der Fingerabdruck Auswahl auf dem Bildschirm Sicherheit](enrolling-fingerprint-images/testing-03.png)

4. Von dort aus die Schritte zum Hinzufügen eines Fingerabdrucks an das Gerät:

    [![Sequenz von Screenshots für das Hinzufügen eines Fingerabdrucks an das Gerät](enrolling-fingerprint-images/testing-04-sml.png)](enrolling-fingerprint-images/testing-04.png#lightbox)

5. Im abschließenden Bildschirm werden Sie aufgefordert, Ihren Finger auf des fingerabdruckscanners platzieren: 

    ![Bildschirm mit der Sie aufgefordert werden, fügen Ihren Finger auf dem sensor](enrolling-fingerprint-images/testing-05.png)

    Wenn Sie ein Android-Gerät verwenden, führen Sie den Prozess durch berühren einen Finger auf die Überprüfung. 
    
    
### <a name="simulating-a-fingerprint-scan-on-the-emulator"></a>Simulieren einen Fingerabdruck-Scan auf dem Emulator

Auf einem Android-Emulator ist es möglich, eine Überprüfung per Fingerabdruck zu simulieren, indem Sie mithilfe von Android Debug Bridge. Klicken Sie auf OS X-Start eine Terminalsitzung auf Windows starten Sie eine Eingabeaufforderung oder eine Powershell-Sitzung, und führen `adb`:

```shell
$ adb -e emu finger touch 1
```

Der Wert des **1** ist die _Finger\_Id_ für den Finger, die "gescannt wurde". Es ist eine eindeutige ganze Zahl, die Sie für jeden virtuellen Fingerabdruck zuweisen. In der Zukunft verwendet werden, wenn Sie die app ausgeführt wird können führen Sie dieses gleiche ADB Befehl jedes Mal, wenn den Emulator fordert Sie für einen Fingerabdruck, können Sie Ausführen der `adb` Befehl aus, und übergeben sie die _Finger\_Id_ zum Simulieren der Fingerprint-Überprüfung .

Nach Abschluss die Überprüfung per Fingerabdruck benachrichtigt Android Sie, dass der Fingerabdruck hinzugefügt wurde:  

![Bildschirm mit Fingerabdruck hinzugefügt!](enrolling-fingerprint-images/testing-06.png)

## <a name="summary"></a>Zusammenfassung 

Dieses Handbuch erläutert, wie Sie einen bildschirmsperren einrichten, und registrieren einen Fingerabdruck, die auf einem Android-Gerät oder im Android-Emulator. 

