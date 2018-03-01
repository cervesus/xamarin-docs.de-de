---
title: "Problembehandlung bei Xamarin-Arbeitsmappen auf Android-Geräten"
ms.topic: article
ms.prod: xamarin
ms.assetid: F1BD293B-4EB7-4C18-A699-718AB2844DFB
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.openlocfilehash: eb188abb3e757f6f66af7758ced311ae1236d3ce
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="troubleshooting-xamarin-workbooks-on-android"></a>Problembehandlung bei Xamarin-Arbeitsmappen auf Android-Geräten

## <a name="emulator-support"></a>Emulator-Unterstützung

Zum Ausführen einer Android-Arbeitsmappe muss ein Android-Emulator für die Verwendung verfügbar sein. Physische Android-Geräte werden nicht unterstützt.

Google-Emulator zusammen mit HAXM wird empfohlen, wenn Ihr Computer unterstützt.
Wenn Sie Hyper-V auf Ihrem System aktiviert haben müssen, gehen Sie mit dem Visual Studio-Android-Emulator auf.

Sie benötigen einen Emulator, der Android 5.0 oder höher ausgeführt wird. ARM-Emulatoren werden nicht unterstützt. Verwendung `x86` oder `x86_64` nur für Geräte.

Lesen Sie [unserer Dokumentation zum Einrichten der Android-Emulatoren] [ android-emu] , wenn Sie nicht mit dem Prozess vertraut sind.

**Hinweis:** Arbeitsmappen 1.1 und früher Worddokument versuchen (und nicht!) für ARM-Emulatoren verwenden, sofern diese verfügbar sind. So umgehen Sie dieses, starten Sie den X86 Emulator Ihrer Wahl vor dem Öffnen oder erstellen eine Android-Arbeitsmappe. Arbeitsmappen werden immer für die Verbindung mit einem laufenden Emulator bevorzugen, solange er kompatibel ist.

## <a name="workbooks-wont-load"></a>Arbeitsmappen nicht geladen.

### <a name="workbook-window-spins-forever-never-loads-windows"></a>Arbeitsmappenfenster dreht sich immer, nie lädt (Windows)

Überprüfen Sie zunächst, dass der Emulator vollständig funktionierende Netzwerk zugreifen, durch eine beliebige Website im Webbrowser des Emulators testen kann.

### <a name="visual-studio-android-emulator-cannot-connect-to-internet"></a>Visual Studio-Android-Emulator kann keine Verbindung zum Internet herstellen.

Wenn der Emulator nicht Netzwerkzugriff verfügt, müssen Sie diese Schritte ausführen, um die Hyper-V-Netzwerkswitch zu beheben. Wenn beim Wechsel zwischen WLAN-Netzwerke häufig müssen Sie dies in regelmäßigen Abständen zu wiederholen:

0. **Stellen Sie sicher, dass kritische Netzwerkvorgänge abgeschlossen sind, wie dies Windows vorübergehend aus dem Internet die Verbindung trennen kann.**
1. Emulatoren zu schließen.
2. Öffnen Sie `Hyper-V Manager`.
3. Klicken Sie unter `Actions`öffnen `Virtual Switch Manager...`.
4. Löschen Sie alle virtuellen Switches an.
5. Klicken Sie auf `OK`.
6. VS-Android-Emulator zu starten. Wahrscheinlich werden Sie aufgefordert, virtueller Netzwerkswitch neu zu erstellen.
7. Testen Sie, ob VS Android Emulator Browser auf das Internet zugreifen kann.

[android-emu]: https://developer.xamarin.com/guides/android/deployment,_testing,_and_metrics/debug-on-emulator/


## <a name="related-links"></a>Verwandte Links

- [Melden von Fehlern](~/tools/workbooks/install.md#reporting-bugs)
