---
title: Unterstützung für Xamarin.Mac-Erweiterungen
description: In diesem Dokument wird die Unterstützung von xamarin. Mac für Finder-, Freigabe-und heutige Erweiterungen beschrieben. Sie untersucht Einschränkungen und bekannte Probleme, Links zu einer exemplarischen Vorgehensweise und einer Beispiel-APP und bietet Tipps zum Arbeiten mit Erweiterungen.
ms.prod: xamarin
ms.assetid: 4148F1BE-DFA0-46B6-9FCD-425A6541F510
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 03/14/2017
ms.openlocfilehash: 9a9dbb63b78b00a9bcac9d7833530da02890afc6
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73017298"
---
# <a name="xamarinmac-extension-support"></a>Unterstützung für Xamarin.Mac-Erweiterungen

In xamarin. Mac 2,10 wurde für mehrere macOS-Erweiterungs Punkte eine Unterstützung hinzugefügt:

- Finder
- Freigeben
- Heute

<a name="Limitations-and-Known-Issues" />

## <a name="limitations-and-known-issues"></a>Einschränkungen und bekannte Probleme

Im folgenden finden Sie die Einschränkungen und bekannten Probleme, die beim Entwickeln von Erweiterungen in xamarin. Mac auftreten können:

- Derzeit gibt es keine Debuggingunterstützung in Visual Studio für Mac. Das Debuggen muss über **NSLog** und die- **Konsole**durchgeführt werden. Weitere Informationen finden Sie im Abschnitt Tipps.
- Erweiterungen müssen in einer Host Anwendung enthalten sein, die bei einem einmaligen ausführen mit Register beim System ausgeführt wird. Sie müssen dann im **Erweiterungs** Abschnitt der **System Einstellungen**aktiviert werden. 
- Einige Erweiterungs Abstürze können die Host Anwendung destabilisieren und ein seltsames Verhalten verursachen. Insbesondere der **Finder** und der **heutige** Abschnitt des **Benachrichtigungs Centers** werden möglicherweise "Jammed" und werden nicht mehr reagiert. Dies ist auch in Erweiterungs Projekten in Xcode zu verzeichnen und wird zurzeit nicht in Zusammenhang mit xamarin. Mac angezeigt. Dies kann häufig im System Protokoll (über die **Konsole**, siehe Tipps für Details) zum Drucken von wiederholten Fehlermeldungen angezeigt werden. Dieser Fehler wird durch das Neustarten von macOS behoben.

<a name="Tips" />

## <a name="tips"></a>Tipps

Die folgenden Tipps können bei der Arbeit mit Erweiterungen in xamarin. Mac hilfreich sein:

- Da xamarin. Mac derzeit keine debuggingerweiterungen unterstützt, hängt die debuggingweise hauptsächlich von der Ausführung und `printf` like-Anweisungen ab. Erweiterungen werden jedoch in einem Sandkasten Prozess ausgeführt, sodass `Console.WriteLine` nicht wie in anderen xamarin. Mac-Anwendungen agieren. Wenn Sie [`NSLog` direkt](https://gist.github.com/chamons/e2e409013a449cfbe1f2fbe5547f6554) aufrufen, werden Debugmeldungen in das System Protokoll ausgegeben.
- Alle nicht abgefangenen Ausnahmen abstürzen den Erweiterungsprozess, sodass nur eine kleine Menge nützlicher Informationen im **System Protokoll**bereitgestellt wird. Das Packen von problematischen Code in einem `try/catch`-Block (Ausnahme), der vor dem erneuten Auslösen `NSLog` ist, kann nützlich sein.
- Der Zugriff auf das **System Protokoll** kann über die **Konsolen** -app unter **Anwendungen**  > -**Hilfsprogramme**erfolgen:

    [![](extensions-images/extension02.png "The system log")](extensions-images/extension02.png#lightbox)
- Wie bereits erwähnt, wird das Ausführen der Erweiterungs Host Anwendung beim System registriert. Löschen des Anwendungspakets mit der Aufhebung der Registrierung. 
- Wenn die "Stray"-Versionen der Erweiterungen einer APP registriert sind, verwenden Sie den folgenden Befehl, um Sie zu suchen (sodass Sie gelöscht werden können): `plugin kit -mv`

<a name="Walkthrough-and-Sample-App" />

## <a name="walkthrough-and-sample-app"></a>Exemplarische Vorgehensweise und Beispiel-App

Da der Entwickler xamarin. Mac-Erweiterungen auf die gleiche Weise wie xamarin. IOS-Erweiterungen erstellt und bearbeitet, finden Sie weitere Informationen in unserer [Einführung in die Extensions](~/ios/platform/extensions.md) -Dokumentation.

Ein xamarin. Mac-Beispiel Projekt, das kleine, funktionierende Beispiele für jeden Erweiterungstyp enthält, finden Sie [hier](https://docs.microsoft.com/samples/xamarin/mac-samples/extensionsamples).

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde die Arbeit mit Erweiterungen in einer xamarin. Mac-app der Version 2,10 (und höher) kurz erläutert.

## <a name="related-links"></a>Verwandte Links

- [Hello, Mac (Hallo, Mac)](~/mac/get-started/hello-mac.md)
- [Extensionsamples](https://docs.microsoft.com/samples/xamarin/mac-samples/extensionsamples)
- [macOS-Eingaberichtlinien](https://developer.apple.com/design/human-interface-guidelines/macos/overview/themes/)
