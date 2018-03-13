---
title: Registrieren einen Fingerabdruck
description: "Zum Festlegen einer Bildschirmsperre einrichten und registrieren einen Fingerabdruck auf einem Android-Gerät oder -Emulator."
ms.topic: article
ms.prod: xamarin
ms.assetid: 52092F63-00EE-4F8B-A49F-65C9CCBA7EF2
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 20e6d693f2a3eba54afaf1d3c7054ad75d7a7610
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="enrolling-a-fingerprint"></a>Registrieren einen Fingerabdruck

## <a name="enrolling-a-fingerprint-overview"></a>Registrieren eine Fingerabdruck-Übersicht

Es kann nur für ein Android-Anwendung Fingerabdruckauthentifizierung nutzen, wenn das Gerät bereits mit Fingerabdruckauthentifizierung konfiguriert wurde. Dieses Handbuch besprechen Fingerabdruck auf einem Android-Gerät oder -Emulator zu registrieren. Emulatoren keinen der tatsächlichen Hardware eine Fingerabdruck Überprüfung durchführen, aber es ist möglich, einen Fingerabdruck-Scan mithilfe der Android Debug Bridge (siehe unten) zu simulieren.  Diese Anleitung wird erläutert, wie Bildschirmsperre auf einem Android-Gerät zu aktivieren, und registrieren einen Fingerabdruck für die Authentifizierung.

## <a name="requirements"></a>Anforderungen

Um einen Fingerabdruck zu registrieren, benötigen Sie ein Android-Gerät oder einen Emulator mit API-Ebene 23 (Android 6.0).

Die Verwendung von der Android Debug Bridge (ADB) erfordert Vertrautheit mit der Befehlszeile aus, und die **Adb** ausführbare Datei muss im Pfad der Ihre Bash, PowerShell oder Command Prompt-Umgebung.

## <a name="configuring-a-screen-lock-and-enrolling-a-fingerprint"></a>Eine Bildschirmsperren konfigurieren und registrieren einen Fingerabdruck 

Um eine bildschirmsperren einzurichten, führen Sie die folgenden Schritte aus:

1. Wechseln Sie zu **Einstellungen > Sicherheit**, und wählen Sie **Sperrbildschirm**:

    ![Speicherort der Auswahl der Bildschirm Sperren auf dem Bildschirm Sicherheit](enrolling-fingerprint-images/testing-01.png)

2. Im nächste Bildschirm, der angezeigt wird können Sie auswählen und konfigurieren Sie einen Bildschirm sperren Sicherheitsmethoden: 

    ![Wählen Sie Wischen, Muster, PIN oder Kennwort](enrolling-fingerprint-images/testing-02.png)

   Wählen Sie aus, und führen Sie eines der verfügbaren Bildschirm Lock-Methoden.

3. Sobald die Screenlock konfiguriert ist, zurück zu den **Einstellungen > Sicherheit** Seite und wählen Sie **Fingerabdruck**:

    ![Speicherort der Auswahl Fingerabdruck auf dem Bildschirm Sicherheit](enrolling-fingerprint-images/testing-03.png)

4. Führen Sie die Sequenz um einen Fingerabdruck des Geräts hinzufügen, dort:

    [![Sequenz von Screenshots des Geräts mittels Fingerabdruck hinzugefügt](enrolling-fingerprint-images/testing-04-sml.png)](enrolling-fingerprint-images/testing-04.png#lightbox)

5. Im abschließenden Bildschirm werden Sie aufgefordert, platzieren den Finger am Scanner Fingerabdruck: 

    ![Bildschirm, der Sie aufgefordert werden, der Sensor zusätzlich belasten, den finger](enrolling-fingerprint-images/testing-05.png)

    Wenn Sie ein Android-Gerät verwenden, können schließen Sie den Vorgang von einem Finger zum Scanner berühren ab. 
    
    
### <a name="simulating-a-fingerprint-scan-on-the-emulator"></a>Simulieren einer Überprüfung Fingerabdruck auf dem Emulator

Auf Android-Emulator ist es möglich, einen Fingerabdruck-Scan mithilfe der Android Debug Bridge zu simulieren. Klicken Sie auf OS X-Start eine Terminalsitzung und unter Windows gestartet werden, eine Eingabeaufforderung oder einer Powershell-Sitzung, und führen `adb`:

```shell
$ adb -e emu finger touch 1
```

Der Wert der **1** ist die _Finger\_Id_ für den Finger wieder an, die "gescannt wurde". Es ist eine eindeutige ganze Zahl, die Sie für jeden virtuellen Fingerabdruck zuweisen. In der Zukunft, wenn die app läuft können führen Sie dieses gleiche ADB-Befehl jedes Mal, wenn den Emulator gefragt, ob für einen Fingerabdruck, führen Sie die `adb` Befehl, und übergeben sie die _Finger\_Id_ simulieren Fingerabdruck-Scans .

Nach Abschluss die Überprüfung Fingerabdruck benachrichtigt Android Sie, dass der Fingerabdruck hinzugefügt wurde:  

![Bildschirm anzeigen Fingerabdruck hinzugefügt!](enrolling-fingerprint-images/testing-06.png)

## <a name="summary"></a>Zusammenfassung 

Dieses Handbuch behandelt, wie eine bildschirmsperren einrichten und registrieren einen Fingerabdruck auf einem Android-Gerät oder in einer Android-Emulator. 

