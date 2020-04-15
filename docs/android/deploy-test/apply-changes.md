---
title: Änderungen übernehmen
description: Erste Schritte für die Verwendung des Features „Änderungen übernehmen“ in Xamarin.Android-Projekten
ms.assetid: 38950B0C-8880-448F-B12E-08D5BFE87D0D
author: JonDouglas
ms.author: jodou
ms.date: 03/09/2020
ms.openlocfilehash: 03bace97908a53ac6112cd2600b20d429fe2f521
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2020
ms.locfileid: "80215313"
---
# <a name="apply-changes"></a>Änderungen übernehmen

Mithilfe des Features „Änderungen übernehmen“ können Sie Änderungen, die an Ressourcen vorgenommen wurden, für Ihre ausgeführte App übernehmen, ohne sie neu starten zu müssen. Dadurch können Sie steuern, welche Bestandteile Ihrer App neu gestartet werden, wenn Sie kleine, inkrementelle Änderungen bereitstellen und testen, aber den aktuellen Zustand Ihres Geräts oder Emulators beibehalten möchten.

Das Feature „Änderungen übernehmen“ verwendet Funktionen in der [JVMTI-Implementierung für Android](https://docs.oracle.com/javase/8/docs/platform/jvmti/jvmti.html#bci), die auf Geräten oder Emulatoren mit Android 8.0 (API-Ebene 26) oder höher unterstützt wird.

## <a name="requirements"></a>Anforderungen

Im Folgenden sind die Anforderungen für die Verwendung des Features „Änderungen übernehmen“ aufgeführt:

- **Visual Studio:** Führen Sie unter Windows ein Update auf Visual Studio 2019, Version 16.5 oder höher, durch. Führen Sie unter macOS ein Update auf Visual Studio 2019 für Mac, Version 8.5 oder höher, durch.
- **Xamarin.Android:** Mit Visual Studio muss Xamarin.Android 10.2 oder höher installiert werden (Xamarin.Android wird unter Windows automatisch als Teil der Workload **Mobile Entwicklung mit .NET** und als Teil des **Visual Studio für Mac-Installers** installiert).
- **Android SDK:** Android API 28 oder höher muss über den Android-SDK-Manager installiert werden.
- **Zielgerät oder Emulator:** Auf Ihrem Gerät oder Emulator muss Android 8.0 (API-Ebene 26) oder höher ausgeführt werden.

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

## <a name="get-started"></a>Erste Schritte

Damit Sie das Feature „Änderungen übernehmen“ verwenden können, müssen Sie sicherstellen, dass Sie über ein Gerät oder einen Emulator verfügen, auf dem Android 8.0 (API-Ebene 26) oder höher ausgeführt wird. Führen Sie dann Ihre Android-Anwendung aus. Optional können Sie auch einen Debuggingvorgang ausführen.

Anschließend können Sie wie folgt das Feature „Änderungen übernehmen“ verwenden:

1. **Symbol auf der Symbolleiste:** Sie können auf der Symbolleiste auf das Symbol für das Feature „Änderungen übernehmen“ klicken, um die Änderungen für Ihr Zielgerät oder Ihren Emulator zu übernehmen.

    [![Symbol für „Änderungen übernehmen“ auf der Symbolleiste](apply-changes-images/Apply-Changes-Toolbar.png)](apply-changes-images/Apply-Changes-Toolbar.png#lightbox)

2. **Tastenkombination:** Über die Tastenkombination **UMSCHALT+ALT+F5** können Sie Änderungen für Ihr Zielgerät oder Ihren Emulator übernehmen.
3. **Menü „Debuggen“:** Sie können das Menüelement **Debuggen > Änderungen übernehmen** verwenden, um Änderungen für Ihr Zielgerät oder Ihren Emulator zu übernehmen.

    [![Feature „Änderungen übernehmen“ im Menü „Debuggen“](apply-changes-images/Apply-Changes-Debug-Menu.png)](apply-changes-images/Apply-Changes-Debug-Menu.png#lightbox)

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

## <a name="get-started"></a>Erste Schritte

Damit Sie das Feature „Änderungen übernehmen“ verwenden können, müssen Sie sicherstellen, dass Sie über ein Gerät oder einen Emulator verfügen, auf dem Android 8.0 (API-Ebene 26) oder höher ausgeführt wird. Führen Sie dann Ihre Android-Anwendung aus. Optional können Sie auch einen Debuggingvorgang ausführen.

Anschließend können Sie wie folgt das Feature „Änderungen übernehmen“ verwenden:

1. **Tastenkombination:** Über die Tastenkombination **⌥ + ⇧ + R** können Sie Änderungen für Ihr Zielgerät oder Ihren Emulator übernehmen.
2. **Menü „Ausführen“:** Sie können das Menüelement **Ausführen > Änderungen übernehmen** verwenden, um Änderungen für Ihr Zielgerät oder Ihren Emulator zu übernehmen.

    [![Feature „Änderungen übernehmen“ im Menü „Debuggen“](apply-changes-images/Apply-Changes-Debug-Menu-Mac.png)](apply-changes-images/Apply-Changes-Debug-Menu-Mac.png#lightbox)

-----

## <a name="limitations"></a>Einschränkungen

Die folgenden Änderungen erfordern einen Neustart der Anwendung:

- Ändern von C#-Code
- Hinzufügen oder Entfernen einer Ressource
- Ändern der Datei „AndroidManifest.xml“
- Ändern von nativen Bibliotheken (SO-Dateien).

## <a name="related-links"></a>Verwandte Links

- [Änderungen übernehmen](https://developer.android.com/studio/run#apply-changes)
