---
title: Problembehandlung bei Xamarin Workbooks unter Android
description: Dieses Dokument enthält Tipps zur Problembehandlung beim Arbeiten mit Xamarin Workbooks unter Android. Es erläutert die Emulator-Unterstützung, Arbeitsmappen, die nicht geladen werden, und andere Themen.
ms.prod: xamarin
ms.assetid: F1BD293B-4EB7-4C18-A699-718AB2844DFB
author: davidortinau
ms.author: daortin
ms.date: 03/30/2017
ms.openlocfilehash: be19005ab1125c060ab0111e9f37568d5f4abe45
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029593"
---
# <a name="troubleshooting-xamarin-workbooks-on-android"></a>Problembehandlung bei Xamarin Workbooks unter Android

## <a name="emulator-support"></a>Emulator-Unterstützung

Zum Ausführen einer Android-Arbeitsmappe muss ein Android-Emulator zur Verwendung verfügbar sein. Physische Android-Geräte werden nicht unterstützt.

Es wird empfohlen, den Google-Emulator zusammen mit haxm zu empfehlen, wenn der Computer ihn unterstützt.
Wenn Hyper-V auf Ihrem System aktiviert sein muss, wechseln Sie stattdessen zum Visual Studio-Android-Emulator.

Sie müssen über einen Emulator verfügen, auf dem Android 5,0 oder höher ausgeführt wird. Arm-Emulatoren werden nicht unterstützt. Verwenden Sie nur `x86` oder `x86_64` Geräte.

Wenn Sie mit dem Prozess nicht vertraut sind, lesen Sie [unsere Dokumentation zum Einrichten von Android-Emulatoren][android-emu] .

> [!NOTE]
> Die Arbeitsmappen 1,1 und früher versuchen (und schlagen!), wenn Sie verfügbar sind, um Arm-Emulatoren zu verwenden. Um dieses Problem zu umgehen, starten Sie den x86-Emulator Ihrer Wahl, bevor Sie eine Android-Arbeitsmappe öffnen oder erstellen. Arbeitsmappen bevorzugen immer, eine Verbindung mit einem laufenden Emulator herzustellen, sofern er kompatibel ist.

## <a name="workbooks-wont-load"></a>Arbeitsmappen werden nicht geladen

### <a name="workbook-window-spins-forever-never-loads-windows"></a>Arbeitsmappenfenster wird dauerhaft und nie geladen (Windows)

Überprüfen Sie zunächst, ob Ihr Emulator über vollständigen Netzwerk Zugriff verfügt, indem Sie eine beliebige Website im Webbrowser des Emulators testen.

### <a name="visual-studio-android-emulator-cannot-connect-to-the-internet"></a>Visual Studio-Android-Emulator kann keine Verbindung mit dem Internet herstellen.

Wenn Ihr Emulator keinen Netzwerk Zugriff hat, müssen Sie möglicherweise die folgenden Schritte ausführen, um den Hyper-V-Netzwerk Switch zu korrigieren. Wenn Sie häufig zwischen Wi-Fi-Netzwerken wechseln, müssen Sie dies möglicherweise regelmäßig wiederholen:

1. **Stellen Sie sicher, dass alle wichtigen Netzwerk Vorgänge ausgeführt werden, da dadurch Windows vorübergehend vom Internet getrennt werden kann.**
1. Schließen Sie Emulatoren.
1. Öffnen Sie `Hyper-V Manager`.
1. Öffnen Sie unter `Actions``Virtual Switch Manager...`.
1. Löschen Sie alle virtuellen Switches.
1. Klicken Sie auf `OK`.
1. Starten Sie vs Android-Emulator. Sie werden wahrscheinlich aufgefordert, den Switch für ein virtuelles Netzwerk neu zu erstellen.
1. Testen Sie, ob der Browser von vs Android-Emulator auf das Internet zugreifen kann.

[android-emu]: ~/android/deploy-test/debugging/debug-on-emulator.md

## <a name="related-links"></a>Verwandte Links

- [Melden von Fehlern](~/tools/workbooks/install.md#reporting-bugs)
