---
title: Debuggen von Integrationen
ms.topic: article
ms.prod: xamarin
ms.assetid: 90143544-084D-49BF-B44D-7AF943668F6C
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.openlocfilehash: a0873c6b902e29174da5e27a09e8f580d6d69eb7
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="debugging-integrations"></a>Debuggen von Integrationen

## <a name="debugging-agent-side-integrations"></a>Debuggen von agentseitig Integrationen

Debuggen agentseitig Integrationen erfolgt mithilfe von den Protokollierungsmethoden gebotenen am besten die `Log` in Klasse `Xamarin.Interactive.Logging`. Finden Sie unter der [ `API docs` ](https://developer.xamarin.com/api/type/Xamarin.Interactive.Logging.Log/) für die Methoden aufrufen.

Auf MacOS, protokollmeldungen angezeigt, in dem sowohl die Protokoll-Viewer-Menü (**Fenster > Protokollanzeige**) und in das Clientprotokoll. Unter Windows Nachrichten werden nur angezeigt, im Clientprotokoll, da es keine Protokollanzeige vorhanden ist.

Das Clientprotokoll ist an den folgenden Speicherorten auf MacOS und Windows:

- Mac: `~/Library/Logs/Xamarin/Inspector/Xamarin Inspector {date}.log`
- Windows: `%LOCALAPPDATA%\Xamarin\Inspector\logs\Xamarin Inspector {date}.log`

Eins zu berücksichtigen ist, die beim Laden von Integrationen nutzen, über die üblichen `#r` Mechanismus während der Entwicklung, die Assembly für die Integration wird upgradetest werden als eine _Abhängigkeit_ der Arbeitsmappe und verpackt, wobei es ist ein absoluter Pfad nicht verwendet. Dies kann dazu führen, dass die Änderungen nicht verteilt wurde, angezeigt werden, als ob nichts das Neuerstellen der Integration hat.

## <a name="debugging-client-side-integrations"></a>Debuggen von clientseitigen Integrationen

B. clientseitige Integrationen in JavaScript geschrieben wurden und in unserem Web-Browser-Oberfläche geladen (finden Sie unter der [Architektur](~/tools/workbooks/sdk/architecture.md) Dokumentation), die beste Methode zum Debuggen verwenden die WebKit-Entwicklertools auf Mac ist, oder verwenden Sie F12-Auswahl unter Windows .

Beide Sätze von Tools ermöglichen es Ihnen, JavaScript/TypeScript Quelltext anzeigen, Haltepunkte festlegen, Konsolenausgabe, anzeigen und zu überprüfen und ändern das DOM validiert werden.

### <a name="mac"></a>Mac

Um die Developer-Tools für Xamarin-Arbeitsmappen auf einem Mac zu aktivieren, führen Sie den folgenden Befehl in Ihrem Terminal aus:

```shell
defaults write com.xamarin.Inspector WebKitDeveloperExtras -bool true
```

und starten Sie die Xamarin-Arbeitsmappen. Sobald dies geschehen ist, sehen Sie **Element überprüfen** angezeigt werden, in Ihrem Rechtsklick-Kontextmenü, und eine neue **Developer** Bereich wird in Arbeitsmappen Voreinstellungen verfügbar sein. Diese Option können Sie wählen, ob Sie die Developer-Tools, die beim Start geöffnet werden soll:

[![Developer-Bereich](debugging-images/developer-pane-small.png)](debugging-images/developer-pane.png#lightbox)

Diese Einstellung kann nur neu starten sowie – Sie müssen die Arbeitsmappen-Client neu starten, damit dafür auf neuer Arbeitsmappen wirksam wird. Aktivieren die Developer-Tools über das Kontextmenü oder die Einstellungen wird der vertrauten Safari-Benutzeroberfläche angezeigt werden:

[![Safari-Entwicklungstools](debugging-images/mac-dev-tools.png)](debugging-images/mac-dev-tools.png#lightbox)

Informationen zur Verwendung von Safari-Entwicklertools finden Sie unter der [WebKit-Inspektor Dokumentation][webkit-docs].

### <a name="windows"></a>Windows

Unter Windows, bietet das IE-Team ein Tool, das als "F12-Auswahl" bezeichnet, eine Remotedebugger für eingebettete Instanzen von Internet Explorer. Sie finden das Tool an folgendem Speicherort:

```shell
C:\Windows\System32\F12\F12Chooser.exe
```

Ausführen F12-Auswahl, und Sie sollte eingebettete Instanz angezeigt, die die Oberfläche des Arbeitsmappen-Client in der Liste verwendet werden. Wählen Sie es, und der vertrauten F12-Tools zum Debuggen von Internet Explorer angezeigt werden, an den Client angehängt:

[![F12-tools](debugging-images/windows-dev-tools.png)](debugging-images/windows-dev-tools.png#lightbox)

[webkit-docs]: https://trac.webkit.org/wiki/WebInspector