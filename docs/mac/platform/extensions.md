---
title: Unterstützung von Xamarin.Mac
description: Dieses Dokument beschreibt die Xamarin.Mac-Unterstützung für Finder, Freigabe und heute Erweiterungen. Es werden Einschränkungen und bekannte Probleme, Links zu einer exemplarischen Vorgehensweise und Beispiel-app, und enthält Tipps zum Arbeiten mit Erweiterungen.
ms.prod: xamarin
ms.assetid: 4148F1BE-DFA0-46B6-9FCD-425A6541F510
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/14/2017
ms.openlocfilehash: 60b981a764a2656210730ae0602ff32dc580cd0a
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50117563"
---
# <a name="xamarinmac-extension-support"></a>Unterstützung von Xamarin.Mac

In der Vorschau von Xamarin.Mac 2.10 wurde Unterstützung für mehrere MacOS-Erweiterungspunkte hinzugefügt:

- Finder
- Freigeben
- Heute

<a name="Limitations-and-Known-Issues" />

## <a name="limitations-and-known-issues"></a>Einschränkungen und bekannte Probleme

Im folgenden sind die Einschränkungen und bekannte Probleme, die auftreten können, bei der Entwicklung von Xamarin.Mac-Erweiterungen:

* Es gibt derzeit kein debugging-Unterstützung in Visual Studio für Mac. Alle debuggen müssen über erfolgen **NSLog** und **Konsole**. Siehe Tipps unten im Abschnitt Details.
* Erweiterungen müssen enthalten sein, in einer hostanwendung, die bei einmal mit registrieren Sie sich mit dem System ausführen. Sie müssen dann aktiviert werden die **Erweiterung** im Abschnitt **Systemeinstellungen**. 
* Einige Abstürze Erweiterung können die hostanwendung destabilisieren und unerwartetem Verhalten führen. Insbesondere **Finder** und **heute** Teil der **Mitteilungszentrale** "ein Papierstau aufgetreten" werden und reagiert möglicherweise. Dies wurde in Erweiterungsprojekten in Xcode als auch erfahrene und zurzeit wird angezeigt, unabhängig von Xamarin.Mac. Häufig kann dies im Systemprotokoll angezeigt werden (über **Konsole**, finden Sie unter Tipps für Details) wiederholte Druckfehlermeldungen. Neustarten von MacOS scheint dieses Problem zu beheben.

<a name="Tips" />

## <a name="tips"></a>Tipps

Die folgenden Tipps können hilfreich sein, bei der Arbeit mit Xamarin.Mac-Erweiterungen:

- Da Xamarin.Mac Debugerweiterungen derzeit nicht unterstützt, das Debuggen dabei hängt in erster Linie für Ausführung und `printf` wie Anweisungen. Allerdings Erweiterungen in einem sandkastenprozess ausgeführt, daher `Console.WriteLine` nicht angewendet wird, wie in anderen Xamarin.Mac-Anwendungen. Aufrufen von [ `NSLog` direkt](https://gist.github.com/chamons/e2e409013a449cfbe1f2fbe5547f6554) gibt debugging-Meldungen im Systemprotokoll.
- Nicht abgefangenen Ausnahmen stürzt der verlängerungsprozess, die Bereitstellung nur einer kleinen Menge nützlicher Informationen in den **Systemprotokoll**. Umschließen die problematischen Code in eine `try/catch` (Ausnahme) zu blockieren, `NSLog`des vor dem erneuten Auslösen nützlich sein kann.
- Die **Systemprotokoll** zugegriffen werden kann, aus der **Konsole** -app unter **Anwendungen** > **Dienstprogramme**:

    [![](extensions-images/extension02.png "Das Systemprotokoll")](extensions-images/extension02.png#lightbox)
- Wie bereits erwähnt, wird mit der hostanwendung für die Erweiterung mit dem System registriert. Löschen das Anwendungspaket mit heben Sie die Registrierung. 
- Wenn "vereinzelten" Versionen der app-Erweiterungen registriert sind, verwenden Sie den folgenden Befehl, um sie zu finden (sodass sie gelöscht werden können): `plugin kit -mv`


<a name="Walkthrough-and-Sample-App" />

## <a name="walkthrough-and-sample-app"></a>Exemplarische Vorgehensweise und Beispiel-App

Da der Entwickler erstellt und mit Xamarin.Mac-Erweiterungen in die gleiche Weise wie die Xamarin.iOS-Erweiterungen funktionieren, finden Sie in unserem [Einführung in die Erweiterungen](~/ios/platform/extensions.md) Dokumentation.

Ein Beispiel für Xamarin.Mac-Projekt mit kleinen, funktionstüchtige Beispiele für jeden Erweiterungstyp finden [hier](https://developer.xamarin.com/samples/mac/ExtensionSamples/).

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

In diesem Artikel verfügt über einen kurzen Blick auf die Arbeit mit einem Xamarin.Max Version 2.10 (und höher) der app-Erweiterungen ausgeführt werden.

## <a name="related-links"></a>Verwandte Links

- [Hello, Mac (Hallo, Mac)](~/mac/get-started/hello-mac.md)
- [ExtensionSamples](https://developer.xamarin.com/samples/mac/ExtensionSamples/)
- [Eingaberichtlinien für OS X](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
