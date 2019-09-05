---
title: Debugintegrationen
description: In diesem Dokument wird beschrieben, wie Sie Xamarin Workbooks-Integrationen sowohl auf Agent-Seite als auch auf Clientseite unter Windows und Mac Debuggen.
ms.prod: xamarin
ms.assetid: 90143544-084D-49BF-B44D-7AF943668F6C
author: conceptdev
ms.author: crdun
ms.date: 06/19/2018
ms.openlocfilehash: fbb5673a70328ad6edde78af1b35d2801fe65ca8
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70283927"
---
# <a name="debugging-integrations"></a>Debugintegrationen

## <a name="debugging-agent-side-integrations"></a>Debuggen von agentseitigen Integrationen

Das Debuggen von agentseitigen Integrationen erfolgt am besten mithilfe der Protokollierungs `Log` Methoden der `Xamarin.Interactive.Logging`-Klasse in.

Unter macOS werden Protokollmeldungen sowohl im Menü Log Viewer (**Fenster > Log Viewer**) als auch im Client Protokoll angezeigt. Unter Windows werden Meldungen nur im Client Protokoll angezeigt, da dort kein Protokoll-Viewer vorhanden ist.

Das Client Protokoll befindet sich unter macOS und Windows an den folgenden Speicherorten:

- Mac: `~/Library/Logs/Xamarin/Workbooks/Xamarin Workbooks {date}.log`
- Windows: `%LOCALAPPDATA%\Xamarin\Workbooks\logs\Xamarin Workbooks {date}.log`

Beachten Sie, dass beim Laden von Integrationen über den üblichen `#r` Mechanismus während der Entwicklung die integrationsassembly als _Abhängigkeit_ von der Arbeitsmappe übernommen und mit ihr verpackt wird, wenn kein absoluter Pfad verwendet wird. Dies kann dazu führen, dass Änderungen nicht weitergegeben werden, so als ob das Neuerstellen der Integration nichts bewirkt hat.

## <a name="debugging-client-side-integrations"></a>Debuggen von Client seitigen Integrationen

Da Client seitige Integrationen in JavaScript geschrieben und in unsere Webbrowser Oberfläche geladen werden (siehe [Architektur](~/tools/workbooks/sdk/architecture.md) Dokumentation), ist die beste Möglichkeit, Sie zu debuggen, die Verwendung der WebKit-Entwicklertools unter Mac oder die Verwendung von F12-Auswahl unter Windows.

Beide Toolsets ermöglichen es Ihnen, die JavaScript/typescript-Quelle anzuzeigen, Haltepunkte festzulegen, Konsolen Ausgaben anzuzeigen und das DOM zu überprüfen und zu ändern.

### <a name="mac"></a>Mac

Führen Sie den folgenden Befehl in Ihrem Terminal aus, um die Entwicklertools für Xamarin Workbooks auf dem Mac zu aktivieren:

```shell
defaults write com.xamarin.Workbooks WebKitDeveloperExtras -bool true
```

und dann Xamarin Workbooks neu starten. Wenn Sie dies tun, sollte das Element "über **prüfen** " in Ihrem Kontextmenü angezeigt werden, und ein neuer **Entwickler** Bereich ist in den Arbeitsmappen-Einstellungen verfügbar. Mit dieser Option können Sie auswählen, ob die Entwicklertools beim Start geöffnet werden sollen:

[![Entwicklerbereich](debugging-images/developer-pane-small.png)](debugging-images/developer-pane.png#lightbox)

Diese Einstellung ist nur neu starten – Sie müssen den Arbeitsmappen-Client neu starten, damit er in neuen Arbeitsmappen wirksam wird. Wenn Sie die Entwicklertools über das Kontextmenü oder die Einstellungen aktivieren, wird die vertraute Safari-Benutzeroberfläche angezeigt:

[![Safari-Entwicklertools](debugging-images/mac-dev-tools.png)](debugging-images/mac-dev-tools.png#lightbox)

Informationen zur Verwendung der Safari-Entwicklertools finden Sie in der [Dokumentation zum WebKit-Inspektor][webkit-docs].

### <a name="windows"></a>Windows

Unter Windows stellt das Internet Explorer-Team ein Tool mit dem Namen "F12 Chooser" bereit, bei dem es sich um einen Remote Debugger für eingebettete Internet Explorer-Instanzen handelt. Das Tool befindet sich an folgendem Speicherort:

```shell
C:\Windows\System32\F12\F12Chooser.exe
```

Führen Sie F12 Chooser aus, und es sollte die eingebettete Instanz angezeigt werden, die die Client Oberfläche der Arbeitsmappen in der Liste unterstützt. Wählen Sie es aus, und die vertrauten F12-Debugtools von Internet Explorer werden angezeigt, die an den Client angefügt sind:

[![F12-Tools](debugging-images/windows-dev-tools.png)](debugging-images/windows-dev-tools.png#lightbox)

[webkit-docs]: https://trac.webkit.org/wiki/WebInspector
