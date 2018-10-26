---
title: Debuggen von Integrationen
description: In diesem Dokument wird beschrieben, wie zum Debuggen von Xamarin Workbooks-Integrationen, sowohl agentseitig als auch die clientseitige unter Windows und Mac.
ms.prod: xamarin
ms.assetid: 90143544-084D-49BF-B44D-7AF943668F6C
author: lobrien
ms.author: laobri
ms.date: 06/19/2018
ms.openlocfilehash: 86d9c6af93e7f59eb0e819730e46324688df7566
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50105999"
---
# <a name="debugging-integrations"></a>Debuggen von Integrationen

## <a name="debugging-agent-side-integrations"></a>Debuggen von agentseitig-Integrationen

Debuggen von agentseitig Integrationen erfolgt am besten mithilfe von bereitgestellten Protokollierungsmethoden der `Log` -Klasse im `Xamarin.Interactive.Logging`. Finden Sie unter den [ `API docs` ](https://developer.xamarin.com/api/type/Xamarin.Interactive.Logging.Log/) für die Methoden aufrufen.

Unter MacOS, protokollmeldungen in sowohl im Protokolldatei-Viewer-Menü angezeigt werden (**Fenster > Protokollanzeige**) und im Clientprotokoll. Auf Windows Nachrichten werden nur angezeigt, im Clientprotokoll, da es keine Protokollanzeige vorhanden ist.

Das Clientprotokoll ist an den folgenden Speicherorten unter MacOS und Windows:

- Mac: `~/Library/Logs/Xamarin/Workbooks/Xamarin Workbooks {date}.log`
- Windows: `%LOCALAPPDATA%\Xamarin\Workbooks\logs\Xamarin Workbooks {date}.log`

Dabei ist zu beachten ist, die beim Laden von Integrationen über den üblichen `#r` Mechanismus während der Entwicklung, Integration Assembly übernommen als eine _Abhängigkeit_ der Arbeitsmappe und mit ihm verpackt werden, wenn ein absoluter Pfad ist nicht verwendet. Dadurch können Änderungen nicht weitergegeben werden, angezeigt werden, als ob die Neuerstellung der Integration hat nichts.

## <a name="debugging-client-side-integrations"></a>Debuggen von clientseitigen-Integrationen

Die clientseitige Integrationen in JavaScript geschrieben und in unsere Web-Browser-Oberfläche geladen (finden Sie unter der [Architektur](~/tools/workbooks/sdk/architecture.md) Dokumentation), die beste Möglichkeit, die sie Debuggen den WebKit-Developer-Tools auf einem Mac verwenden ist, oder verwenden Sie F12-Auswahl für Windows .

Beide Sätze von Tools können Sie JavaScript/TypeScript-Quelle anzeigen, legen Sie Haltepunkte, Anzeigen der Konsolenausgabe, und überprüfen und ändern das DOM.

### <a name="mac"></a>Mac

Um die Developer Tools für Xamarin Workbooks auf einem Mac zu aktivieren, führen Sie den folgenden Befehl in Ihrem Terminal aus:

```shell
defaults write com.xamarin.Workbooks WebKitDeveloperExtras -bool true
```

und starten Sie Xamarin Workbooks. Sobald Sie dies tun, sollten Sie sehen **Element überprüfen** in Ihre Rechtsklick-Kontextmenü, und ein neues angezeigt werden **Developer** Bereich stehen in Arbeitsmappen-Einstellungen. Diese Option können Sie wählen, wenn Sie die Developer-Tools, die beim Start geöffnet werden soll:

[![Entwickler-Bereich](debugging-images/developer-pane-small.png)](debugging-images/developer-pane.png#lightbox)

Diese Voreinstellung ist ebenfalls nur neu starten, müssen Sie die Arbeitsmappen-Client neu starten, damit es auf neue Arbeitsmappen wirksam. Aktivieren die Developer-Tools über das Kontextmenü oder die Einstellungen werden die vertraute Safari-Benutzeroberfläche angezeigt werden:

[![Safari-Entwicklungstools](debugging-images/mac-dev-tools.png)](debugging-images/mac-dev-tools.png#lightbox)

Weitere Informationen zur Verwendung der Safari-Entwickler-Tools finden Sie unter den [WebKit-Inspektor Dokumentation][webkit-docs].

### <a name="windows"></a>Windows

Auf Windows, bietet das IE-Team ein Tool, genannt "F12-Auswahl", um einen Remotedebugger für eingebettete Internet Explorer-Instanzen. Sie finden das Tool an folgendem Speicherort:

```shell
C:\Windows\System32\F12\F12Chooser.exe
```

Zur Auswahl von F12, und Sie sollte die eingebettete-Instanz, die die Arbeitsmappen-Client-Oberfläche in der Liste zugrunde liegen. Wählen Sie es, und die vertrauten F12 Tools zum Debuggen von Internet Explorer angezeigt werden, an den Client angehängt:

[![F12-tools](debugging-images/windows-dev-tools.png)](debugging-images/windows-dev-tools.png#lightbox)

[webkit-docs]: https://trac.webkit.org/wiki/WebInspector
