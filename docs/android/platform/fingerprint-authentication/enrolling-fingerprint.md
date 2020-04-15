---
title: Registrieren eines Fingerabdrucks
description: Einrichten einer Bildschirmsperre und Registrieren eines Fingerabdrucks auf einem Android-Gerät oder -Emulator.
ms.prod: xamarin
ms.assetid: 52092F63-00EE-4F8B-A49F-65C9CCBA7EF2
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/16/2018
ms.openlocfilehash: c0290dfa3b4aa301a07a589f78577899e8282158
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2020
ms.locfileid: "73027593"
---
# <a name="enrolling-a-fingerprint"></a>Registrieren eines Fingerabdrucks

## <a name="enrolling-a-fingerprint-overview"></a>Registrieren eines Fingerabdrucks: Übersicht

Eine Android-Anwendung kann Fingerabdruckauthentifizierung nur nutzen, wenn das Gerät bereits mit Fingerabdruckauthentifizierung konfiguriert wurde. In diesem Leitfaden wird das Einrichten einer Bildschirmsperre und Registrieren eines Fingerabdrucks auf einem Android-Gerät oder -Emulator beschrieben. Emulatoren verfügen nicht über die tatsächliche Hardware zum Durchführen einer Fingerabdrucküberprüfung, aber es ist möglich, eine Fingerabdrucküberprüfung mithilfe der Android Debug Bridge (unten beschrieben) zu simulieren.  In diesem Leitfaden wird erläutert, wie Sie die Bildschirmsperre auf einem Android-Gerät aktivieren und einen Fingerabdruck für die Authentifizierung registrieren.

## <a name="requirements"></a>Anforderungen

Zum Registrieren eines Fingerabdrucks müssen Sie über ein Android-Gerät oder einen Emulator verfügen, auf dem API-Ebene 23 (Android 6.0) ausgeführt wird.

Die Verwendung der Android Debug Bridge (ADB) erfordert Vertrautheit mit der Eingabeaufforderung, und die ausführbare **ADB**-Datei muss sich im Pfad (PATH) Ihrer Bash-, PowerShell- oder Eingabeaufforderungsumgebung befinden.

## <a name="configuring-a-screen-lock-and-enrolling-a-fingerprint"></a>Konfigurieren einer Bildschirmsperre und Registrieren eines Fingerabdrucks 

Führen Sie die folgenden Schritte aus, um eine Bildschirmsperre einzurichten:

1. Navigieren Sie zu **Einstellungen > Sicherheit**, und wählen Sie **Bildschirmsperre** aus:

    ![Position der Bildschirmsperrenauswahl auf dem Bildschirm „Sicherheit“](enrolling-fingerprint-images/testing-01.png)

2. Der nächste Bildschirm, der angezeigt wird, ermöglicht Ihnen, eine der Sicherheitsmethoden für die Bildschirmsperre auszuwählen und zu konfigurieren: 

    ![Auswählen von Wischen, Muster, PIN oder Kennwort](enrolling-fingerprint-images/testing-02.png)

   Wählen Sie eine der verfügbaren Bildschirmsperrmethoden aus, und konfigurieren Sie sie.

3. Nachdem die Bildschirmsperre konfiguriert wurde, kehren Sie zur Seite **Einstellungen > Sicherheit** zurück, und wählen Sie **Fingerabdruck** aus:

    ![Position der Fingerabdruckauswahl auf dem Bildschirm „Sicherheit“](enrolling-fingerprint-images/testing-03.png)

4. Befolgen Sie dort die Sequenz, zum Hinzufügen eines Fingerabdrucks zum Gerät:

    [![Sequenz von Screenshots zum Hinzufügen eines Fingerabdrucks zum Gerät](enrolling-fingerprint-images/testing-04-sml.png)](enrolling-fingerprint-images/testing-04.png#lightbox)

5. Auf dem letzten Bildschirm werden Sie aufgefordert, Ihren Finger auf dem Fingerabdruckscanner zu platzieren: 

    ![Bildschirm, der Sie auffordert, Ihren Finger auf den Sensor zu legen](enrolling-fingerprint-images/testing-05.png)

    Wenn Sie ein Android-Gerät verwenden, schließen Sie den Prozess ab, indem mit einem Finger den Scanner berühren. 

### <a name="simulating-a-fingerprint-scan-on-the-emulator"></a>Simulieren einer Fingerabdrucküberprüfung im Emulator

In einem Android-Emulator ist es möglich, eine Fingerabdrucküberprüfung mithilfe der Android Debug Bridge zu simulieren. Starten Sie unter OS X eine Terminalsitzung. Starten Sie unter Windows eine Eingabeaufforderung oder eine PowerShell-Sitzung, und führen Sie `adb` aus:

```shell
$ adb -e emu finger touch 1
```

Der Wert von **1** ist die _finger\_id_ für den Finger, der „überprüft“ wurde. Dabei handelt es sich um einen eindeutigen Integerwert, den Sie für jeden virtuellen Fingerabdruck zuweisen. Wenn die App ausgeführt wird, können Sie den gleichen ADB-Befehl jedes Mal ausführen, wenn der Emulator Sie zur Eingabe eines Fingerabdrucks auffordert. Sie können den Befehl `adb` ausführen und die _finger\_id_ übergeben, um die Fingerabdrucküberprüfung zu simulieren.

Nachdem die Überprüfung des Fingerabdrucks abgeschlossen ist, werden Sie von Android benachrichtigt, dass der Fingerabdruck hinzugefügt wurde:  

![Bildschirm, der anzeigt, dass der Fingerabdruck hinzugefügt wurde.](enrolling-fingerprint-images/testing-06.png)

## <a name="summary"></a>Zusammenfassung 

In diesem Leitfaden wurde das Einrichten einer Bildschirmsperre und Registrieren eines Fingerabdrucks auf einem Android-Gerät oder einem Android-Emulator gezeigt. 
