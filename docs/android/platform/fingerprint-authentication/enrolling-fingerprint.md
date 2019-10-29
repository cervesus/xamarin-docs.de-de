---
title: Registrieren eines Fingerabdrucks
description: Einrichten einer Bildschirmsperre und Registrieren eines Fingerabdrucks auf einem Android-Gerät oder-Emulator.
ms.prod: xamarin
ms.assetid: 52092F63-00EE-4F8B-A49F-65C9CCBA7EF2
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/16/2018
ms.openlocfilehash: c0290dfa3b4aa301a07a589f78577899e8282158
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73027593"
---
# <a name="enrolling-a-fingerprint"></a>Registrieren eines Fingerabdrucks

## <a name="enrolling-a-fingerprint-overview"></a>Umschließen eines Fingerabdruck (Übersicht)

Eine Android-Anwendung kann die Fingerabdruckauthentifizierung nur nutzen, wenn das Gerät bereits mit Fingerabdruckauthentifizierung konfiguriert wurde. In dieser Anleitung wird erläutert, wie Sie einen Fingerabdruck auf einem Android-Gerät oder-Emulator registrieren. Emulatoren verfügen nicht über die tatsächliche Hardware zum Durchführen eines Fingerabdruck Scans, aber es ist möglich, eine Fingerabdruck Überprüfung mithilfe der Android Debug Bridge (unten beschrieben) zu simulieren.  In dieser Anleitung wird erläutert, wie Sie die Bildschirmsperre auf einem Android-Gerät aktivieren und einen Fingerabdruck für die Authentifizierung registrieren.

## <a name="requirements"></a>Anforderungen

Zum Registrieren eines Fingerabdrucks müssen Sie über ein Android-Gerät oder einen Emulator verfügen, auf dem API-Ebene 23 (Android 6,0) ausgeführt wird.

Die Verwendung des Android Debug Bridge (ADB) erfordert Vertrautheit mit der Eingabeaufforderung, und die ausführbare **ADB** -Datei muss sich im Pfad Ihrer bash-, PowerShell-oder Eingabe Aufforderungs Umgebung befinden.

## <a name="configuring-a-screen-lock-and-enrolling-a-fingerprint"></a>Konfigurieren einer Bildschirmsperre und Anmelden eines Fingerabdrucks 

Führen Sie die folgenden Schritte aus, um eine Bildschirmsperre einzurichten:

1. Wechseln Sie zu **Einstellungen > Sicherheit**, und wählen Sie **Bildschirmsperre**:

    ![Speicherort der Bildschirm Sperr Auswahl auf dem Bildschirm "Sicherheit"](enrolling-fingerprint-images/testing-01.png)

2. Der nächste Bildschirm, der angezeigt wird, ermöglicht Ihnen, eine der Sicherheitsmethoden für die Bildschirmsperre auszuwählen und zu konfigurieren: 

    ![Wählen Sie swipe, Muster, PIN oder Kennwort aus.](enrolling-fingerprint-images/testing-02.png)

   Wählen Sie eine der verfügbaren Bildschirm Sperr Methoden aus, und vervollständigen Sie Sie.

3. Nachdem die screenlock konfiguriert wurde, kehren Sie zur Seite **Einstellungen > Sicherheit** zurück, und wählen Sie **Fingerabdruck**:

    ![Speicherort der Fingerabdruck Auswahl auf dem Bildschirm "Sicherheit"](enrolling-fingerprint-images/testing-03.png)

4. Befolgen Sie dort die Sequenz, um dem Gerät einen Fingerabdruck hinzuzufügen:

    [![Sequenz von Screenshots zum Hinzufügen eines Fingerabdrucks zum Gerät](enrolling-fingerprint-images/testing-04-sml.png)](enrolling-fingerprint-images/testing-04.png#lightbox)

5. Auf dem letzten Bildschirm werden Sie aufgefordert, den Fingerabdruckscanner zu platzieren: 

    ![Bildschirm, der Sie auffordert, den Finger auf den Sensor zu legen](enrolling-fingerprint-images/testing-05.png)

    Wenn Sie ein Android-Gerät verwenden, schließen Sie den Prozess ab, indem Sie einen Finger an den Scanner berühren. 

### <a name="simulating-a-fingerprint-scan-on-the-emulator"></a>Simulieren eines Fingerabdruck Scans im Emulator

Auf einem Android-Emulator ist es möglich, eine Fingerabdruck Überprüfung mithilfe des Android Debug Bridge zu simulieren. Starten Sie unter OS X eine Terminal Sitzung, und starten Sie unter Windows eine Eingabeaufforderung oder eine PowerShell-Sitzung, und führen Sie `adb`aus:

```shell
$ adb -e emu finger touch 1
```

Der Wert **1** ist der _Finger\_ID_ für den Finger, der "gescannt" war. Dabei handelt es sich um eine eindeutige Ganzzahl, die Sie für jeden virtuellen Fingerabdruck zuweisen. Wenn die app ausgeführt wird, können Sie den gleichen ADB-Befehl jedes Mal ausführen, wenn der Emulator Sie zur Eingabe eines Fingerabdrucks auffordert. Sie können den `adb` Befehl ausführen und den _Finger\_ID_ übergeben, um den Fingerabdruck Scan zu simulieren.

Nachdem die Überprüfung des Fingerabdrucks abgeschlossen ist, werden Sie von Android benachrichtigt, dass der Fingerabdruck hinzugefügt wurde:  

![Bildschirm mit hinzugefügter Fingerabdruck](enrolling-fingerprint-images/testing-06.png)

## <a name="summary"></a>Zusammenfassung 

In dieser Anleitung wurde beschrieben, wie Sie eine Bildschirmsperre einrichten und einen Fingerabdruck auf einem Android-Gerät oder in einem Android-Emulator registrieren. 
