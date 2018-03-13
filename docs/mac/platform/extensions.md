---
title: Xamarin.Mac Extension Support
description: "Dieser Artikel behandelt die Unterstützung der Erweiterung in Xamarin.Mac Version 2.10 (und höher)."
ms.topic: article
ms.prod: xamarin
ms.assetid: 4148F1BE-DFA0-46B6-9FCD-425A6541F510
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 530e53230e9f0dea165b083fa6795558025a293f
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="xamarinmac-extension-support"></a>Xamarin.Mac Extension Support

In der Vorschau Xamarin.Mac 2.10 wurde Unterstützung für mehrere MacOS Erweiterungspunkte hinzugefügt:

- Finder
- Freigeben
- Heute

<a name="Limitations-and-Known-Issues" />

## <a name="limitations-and-known-issues"></a>Einschränkungen und bekannte Probleme

Im folgenden sind die Einschränkungen und bekannten Problemen, die beim Entwickeln von Erweiterungen in Xamarin.Mac auftreten können:

* Es gibt derzeit keine debugging-Unterstützung in Visual Studio für Mac Alle debuggen müssen über erfolgen **NSLog** und **Konsole**. Finden Sie unter Tipps im Abschnitt unten.
* Erweiterungen müssen enthalten sein, in eine hostanwendung, das bei einmal mit registrieren mit dem System ausführen. Muss dann in aktiviert werden die **Erweiterung** Abschnitt **Systemeinstellungen**. 
* Einige Abstürze Erweiterung möglicherweise destabilisiert werden die hostanwendung und ungewöhnliches Verhalten verursachen. Insbesondere **Finder** und die **heute** Teil der **Mitteilungszentrale** werden "blockiert" und reagiert möglicherweise. Dies wurde in Erweiterungsprojekten in Xcode als auch erfahrene und wird derzeit keinen Bezug zu Xamarin.Mac angezeigt. Häufig kann dies im Systemprotokoll angezeigt werden (über **Konsole**, finden Sie weitere Informationen unter Tipps) wiederholte Druckfehlermeldungen. Neustarten MacOS wird angezeigt, um dieses Problem zu beheben.

<a name="Tips" />

## <a name="tips"></a>Tipps

Die folgenden Tipps können hilfreich sein, bei der Arbeit mit Erweiterungen in Xamarin.Mac:

- Da Xamarin.Mac Debugerweiterungen derzeit nicht unterstützt wird, der Debugvorgang ist in erster Linie davon abhängig, Ausführung und `printf` wie z. B. Anweisungen. Jedoch Erweiterungen in einem sandkastenprozess ausgeführt, daher `Console.WriteLine` fungiert nicht wie in anderen Anwendungen Xamarin.Mac funktioniert. Aufrufen von [ `NSLog` direkt](https://gist.github.com/chamons/e2e409013a449cfbe1f2fbe5547f6554) Debugmeldungen an das Systemprotokoll ausgegeben wird.
- Nicht abgefangenen Ausnahmen stürzt der Erweiterung werden bietet nur eine kleine Menge von nützlichen Informationen in den **Systemprotokoll**. Umschließen problematische Code in eine `try/catch` (Ausnahme) blockieren, die `NSLog`des vor dem erneuten Auslösen hilfreich sein kann.
- Die **Systemprotokoll** zugegriffen werden kann, aus der **Konsole** app unter der **Anwendungen** > **Hilfsprogramme**:

    [![](extensions-images/extension02.png "Systemprotokoll")](extensions-images/extension02.png#lightbox)
- Wie oben bereits erwähnt, werden die hostanwendung Erweiterung ausführen mit dem System registriert. Löschen das Anwendungspaket mit Ihre Registrierung aufheben. 
- Wenn "verirrte"-Versionen ein app-Erweiterungen registriert sind, verwenden Sie den folgenden Befehl, um diese zu suchen (sodass sie gelöscht werden können): `plugin kit -mv`


<a name="Walkthrough-and-Sample-App" />

## <a name="walkthrough-and-sample-app"></a>Exemplarische Vorgehensweise und Beispiel-App

Da der Entwickler erstellt und mit Xamarin.Mac Erweiterungen in die gleiche Weise wie Xamarin.iOS Erweiterungen arbeiten, finden Sie in unserem [Einführung in die Erweiterungen](~/ios/platform/extensions.md) Dokumentation.

Ein Xamarin.Mac Beispielprojekt mit kleinen funktionstüchtige Beispiele für jeden Erweiterungstyp verwendbaren [hier](https://developer.xamarin.com/samples/mac/ExtensionSamples/).

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

Dieser Artikel hat einen kurzen Blick auf das Arbeiten mit Erweiterungen in einer Xamarin.Max Version 2.10 (und höher)-app übernommen.

## <a name="related-links"></a>Verwandte Links

- [Hello, Mac (Hallo, Mac)](~/mac/get-started/hello-mac.md)
- [ExtensionSamples](https://developer.xamarin.com/samples/mac/ExtensionSamples/)
- [Eingaberichtlinien für OS X](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
