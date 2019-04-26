---
title: Problembehandlung bei Xamarin Workbooks unter Android
description: Dieses Dokument enthält Tipps zur Problembehandlung für die Arbeit mit Xamarin Workbooks unter Android. Es wird erläutert, Unterstützung von Emulatoren, Arbeitsmappen, die nicht geladen werden und anderen Themen.
ms.prod: xamarin
ms.assetid: F1BD293B-4EB7-4C18-A699-718AB2844DFB
author: lobrien
ms.author: laobri
ms.date: 03/30/2017
ms.openlocfilehash: a93288829ff99027a4b33e7720a7f849df37e9b1
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61423797"
---
# <a name="troubleshooting-xamarin-workbooks-on-android"></a>Problembehandlung bei Xamarin Workbooks unter Android

## <a name="emulator-support"></a>Unterstützung von Emulatoren

Zum Ausführen einer Android-Arbeitsmappe muss ein Android-Emulator für die Verwendung verfügbar sein. Physische Android-Geräte werden nicht unterstützt.

Google Emulator mit HAXM wird empfohlen, wenn der Computer diesen unterstützt.
Wenn Sie Hyper-V auf Ihrem System aktiviert haben müssen, verwenden Sie die Visual Studio-Android-Emulator stattdessen.

Sie benötigen einen Emulator, der Android 5.0 oder höher ausgeführt wird. ARM-Emulatoren werden nicht unterstützt. Verwendung `x86` oder `x86_64` nur für Geräte.

Lesen Sie [unserer Dokumentation zum Einrichten der Android-Emulatoren] [ android-emu] , wenn Sie nicht mit dem Prozess vertraut sind.

> [!NOTE]
> Arbeitsmappen, 1.1 und früher werden versuchen Sie es (fehl und!) für ARM-Emulatoren verwenden, sofern diese verfügbar sind. Eine problemumgehung dieser, starten die X86-Emulator Ihrer Wahl vor dem Öffnen oder erstellen eine Android-Arbeitsmappe. Arbeitsmappen werden immer für die Verbindung zu einem ausgeführten Emulator zu bevorzugen, solange er kompatibel ist.

## <a name="workbooks-wont-load"></a>Arbeitsmappen Laden nicht.

### <a name="workbook-window-spins-forever-never-loads-windows"></a>Arbeitsmappenfenster dreht sich immer, nie geladen (Windows)

Überprüfen Sie zunächst, dass der Emulator vollständig funktionierende-Netzwerkzugriff verfügt, testen Sie jede Website im Webbrowser des Emulators.

### <a name="visual-studio-android-emulator-cannot-connect-to-the-internet"></a>Visual Studio Emulator für Android kann nicht mit dem Internet herstellen.

Wenn es sich bei Ihrem Emulator nicht über Zugriff auf das Netzwerk verfügt, müssen Sie diese Schritte ausführen, um Ihre Hyper-V-Netzwerkswitch zu beheben. Wenn der Wechsel zwischen häufig Wi-Fi-Netzwerke müssen Sie dies in regelmäßigen Abständen wiederholt werden soll:

0. **Stellen Sie sicher, dass kritische Netzwerkvorgänge abgeschlossen sind, wie dies Windows vorübergehend aus dem Internet die Verbindung trennen kann.**
1. Schließen Sie die Emulatoren.
2. Öffnen Sie `Hyper-V Manager`.
3. Klicken Sie unter `Actions`öffnen `Virtual Switch Manager...`.
4. Löschen Sie alle virtuellen Switches an.
5. Klicken Sie auf `OK`.
6. Starten Sie Visual Studio-Android-Emulator. Wahrscheinlich werden Sie aufgefordert, virtuelles Netzwerk-Switch neu zu erstellen.
7. Prüfen Sie, ob es sich bei Visual Studio Android Emulator-Browser auf das Internet zugreifen kann.

[android-emu]: https://developer.xamarin.com/guides/android/deployment,_testing,_and_metrics/debug-on-emulator/


## <a name="related-links"></a>Verwandte Links

- [Melden von Fehlern](~/tools/workbooks/install.md#reporting-bugs)
