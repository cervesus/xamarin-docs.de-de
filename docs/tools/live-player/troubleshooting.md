---
title: Problembehandlung
description: Bekannte Probleme mit der Xamarin-Live-Player und wie sie diesen Fehler zu beheben.
ms.topic: article
ms.prod: xamarin
ms.assetid: 29A97ADA-80E0-40A1-8B26-C68FFABE7D26
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 05/17/2017
ms.openlocfilehash: d7c5bedb03d7c869be65e3c704bac58a9cdfcbbd
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="troubleshooting"></a>Problembehandlung

![Vorschaufunktion](~/media/shared/preview.png)

Diesem Artikel erläutert einige der häufigsten Probleme sowie Schritte, um diese zu beheben.


## <a name="mobile-device-does-not-connect-after-scanning-barcode-or-entering-code"></a>Mobiler Geräte wird nicht nach Scan Barcode (oder Code eingeben) eine Verbindung herstellen.

Tritt auf, wenn das mobile Gerät mit Xamarin-Live-Player im selben Netzwerk wie der Computer mit der IDE nicht ist. Sehen Sie sich Folgendes:

- Vergewissern Sie sich, dass das Gerät und der Computer im selben WiFi-Netzwerk sind.
  - Wenn der Computer auch mit einem verkabelten Netzwerk verbunden ist, versuchen Sie die drahtgebundene Verbindung trennen.
- Das Netzwerk möglicherweise eng gesichert werden (z. B. Unternehmensnetzwerken), den von Xamarin Live Player erforderlichen Ports blockieren.
- Schließen Sie die Xamarin-Live-Player-Anwendung, und starten Sie ihn neu.


## <a name="error-while-trying-to-deploy-message-in-ide"></a>"Fehler beim Versuch, das Bereitstellen von" Nachricht in der IDE

**"IOException: kann nicht zum Lesen von Daten von der übertragungsverbindung: Vorgang auf Socket nicht blockierend würde blockieren."**

Dieser Fehler wird häufig aufgetreten ist, wenn das mobile Gerät mit Xamarin Live Player nicht im selben Netzwerk wie der Computer mit der IDE ist; Dies erfolgt häufig auf, wenn auf einem Gerät eine Verbindung herstellen, die zuvor erfolgreich zugewiesen wurde.

* Überprüfen Sie, dass das Gerät und der Computer im selben WiFi-Netzwerk sind.
* Das Netzwerk möglicherweise eng gesichert werden (z. B. Unternehmensnetzwerken), den von Xamarin Live Player erforderlichen Ports blockieren. Die folgenden Ports sind für die Xamarin-Live-Player erforderlich:
  * 37847 – internen Netzwerkzugriff 
  * 8090 – externe Netzwerkzugriff

## <a name="type-or-namespace-cannot-be-found-message-in-ide"></a>"Typ oder Namespace wurde nicht gefunden" Nachricht in der IDE

Überprüfung, die Sie ausgewählt haben eine **Startprojekt** , die den Typ Ihres Geräts (iOS oder Android) entspricht und dass die Konfiguration dieser Gerätetyp (z. b. übereinstimmt **Debuggen | iPhone-Simulator** für iOS).

## <a name="constructor-on-type-interpretedxamarinformsbutton-not-found-message-in-player"></a>"Constructor on type 'InterpretedXamarin.Forms.Button' not found" message in Player

Einige Systemklassen können, z. B. nicht überschrieben werden:

```csharp
public class SomeCustomButton : Xamarin.Forms.Button { ... }
```

## <a name="mainactivitycs-resourcelayout-does-not-contain-a-definition-for-main"></a>"MainActivity.cs:"Resource.Layout"enthält keine Definition für"Main""

Dieser Fehler tritt für Android-Projekte mit Benutzeroberflächen in AXML-Dateien definiert.
AXML Dateien sind derzeit nicht im Xamarin-Live-Player unterstützt.

### <a name="android-toolbar-and-tabs-render-incorrectly-using-xamarinforms"></a>Android Symbolleiste und Registerkarten falsch mit Xamarin.Forms gerendert werden

Xamarin.Forms Android-Projekte müssen "Toolbar.axml" und "Tabbar.axml" für den Namen der relevanten Layoutdateien verwenden. Die Standardvorlage verwendet diese Namen. Probleme beim Rendern wird durch das Umbenennen sie verursacht werden.


Melden Sie alle weiteren Probleme auf [Bugzilla](https://aka.ms/live-player-report-issue).


## <a name="related-links"></a>Verwandte Links

- [Einschränkungen](~/tools/live-player/limitations.md)
- [Setup (Einrichtung)](~/tools/live-player/install.md)
