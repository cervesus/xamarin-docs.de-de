---
title: Xamarin Hot Restart
description: In diesem Dokument wird die Einrichtung und Verwendung von Xamarin Hot Restart zum Debuggen einer iOS-App beschrieben.
ms.prod: xamarin
ms.assetid: 6BC62A88-9368-41BB-8494-760F2A4805DB
ms.technology: xamarin-forms
author: jimmgarrido
ms.author: jigarrid
ms.date: 01/14/2020
ms.openlocfilehash: 2cf925a96e952e6b760da9ca5416e124a3e3716b
ms.sourcegitcommit: ccbf914615c0ce6b3f308d930f7a77418aeb4dbc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/07/2020
ms.locfileid: "77071152"
---
# <a name="xamarin-hot-restart-preview"></a>Xamarin Hot Restart (Preview)

![Feature der Vorschauversion](~/media/shared/preview.png)

Mit Xamarin Hot Restart können Sie Änderungen an Ihrer App während der Entwicklung, der Bearbeitung von Code in mehreren Dateien, Ressourcen und Verweisen schnell testen. Die neuen Änderungen am bestehenden App-Bundle werden an das Debugziel gepusht, wodurch ein wesentlich schnellerer Entwicklungs- und Bereitstellungszyklus ermöglicht wird.

> [!NOTE]
> Xamarin Hot Restart ist derzeit in Visual Studio 2019 Version 16.5 Preview verfügbar und unterstützt iOS-Apps mit Xamarin.Forms. Die Unterstützung für Visual Studio für Mac und anderen Apps als Xamarin.Forms befindet sich in Planung.

## <a name="requirements"></a>Anforderungen

- Visual Studio 2019 Version 16.5 Preview 2 oder neuer
- iTunes (64 Bit)
- Apple Developer-Konto


## <a name="initial-setup"></a>Erste Einrichtung

1. Stellen Sie sicher, dass das iOS-Projekt als Startupprojekt festgelegt und die Buildkonfiguration auf **Debug|iPhone** (Debuggen |iPhone) festgelegt wird.

   1. Wenn es sich um ein bestehendes Projekt handelt, navigieren Sie zu **Erstellen > Konfigurations-Manager...** , und stellen Sie sicher, dass **Bereitstellen** für das iOS-Projekt aktiviert ist.

2. Wählen Sie **Lokales Gerät** in der Symbolleiste aus, und klicken Sie darauf, um den Setup-Assistenten zu starten:

    [![](hot-restart-images/toolbar.png "Screenshot of the Visual Studio toolbar with local device set as the debug target.")](hot-restart-images/toolbar.png)

3. Wenn iTunes nicht installiert ist, klicken Sie auf **Download iTunes** (iTunes herunterladen), um den Installer herunterzuladen. Klicken Sie auf **Weiter**, wenn die iTunes-Installation fertiggestellt ist.

4. Verbinden Sie ein iOS-Gerät mit Ihrem Computer. Der Gerätename wird im Assistenten angezeigt, sobald er erkannt wurde. Klicken Sie auf **Weiter**.

5. Geben Sie Ihre Anmeldeinformationen für Ihr Apple Developer-Konto ein, und klicken Sie auf **Weiter**.

6. Wählen Sie im Dropdownmenü ein Entwicklungsteam aus, um die [automatische Bereitstellung](~/ios/get-started/installation/device-provisioning/automatic-provisioning.md) im Projekt zu aktivieren. Klicken Sie auf **Fertig stellen**.

> [!NOTE]
> Die Verwendung der automatischen Bereitstellung wird empfohlen, damit zusätzliche iOS-Geräte einfach für die Bereitstellung konfiguriert werden können. Sie können diese jedoch deaktivieren und die manuelle Bereitstellung weiterhin verwenden, wenn die richtigen Bereitstellungsprofile vorhanden sind.

## <a name="use-xamarin-hot-restart"></a>Verwenden von Xamarin Hot Restart
Nach der anfänglichen Einrichtung erscheint Ihr verbundenes Gerät im Dropdownmenü für das Debugziel. Wählen Sie Ihr Gerät im Dropdownmenü aus, und klicken Sie auf die Schaltfläche **Ausführen**, um Ihre App zu debuggen. Möglicherweise wird Ihnen in Visual Studio eine Benachrichtigung mit der Bitte angezeigt, die App auf dem Gerät manuell zu starten, um die Debugsitzung zu beginnen.

Sie können während des Debuggens Änderungen an Ihren Codedateien vornehmen und dann die Schaltfläche **Neu starten** in der Debugsymbolleiste drücken oder **STRG+Umschalt+F5** verwenden, um die Debugsitzung mit Ihren neuen Änderungen neu zu starten:

[![](hot-restart-images/restart.png "Screenshot of the debug toolbar with the restart button highlighted.")](hot-restart-images/toolbar.png)

## <a name="limitations"></a>Einschränkungen
- Nur iOS-Apps, die mit Xamarin.Forms und iOS-Geräten erstellt wurden, werden derzeit unterstützt.
- Storyboard- und XIB-Dateien werden nicht unterstützt, und die Anwendung kann abstürzen, wenn sie versucht, diese zur Laufzeit zu laden. Wenn Sie diese in Ihrer App verwenden, teilen Sie uns dies bitte mit, da wir daran interessiert sind, dieses Szenario in Zukunft zu unterstützen.
- Statische iOS-Bibliotheken und -Frameworks werden nicht unterstützt. Es kann zu Laufzeitfehlern oder Abstürzen kommen, wenn Ihre App versucht, diese zu laden. Dynamische iOS-Bibliotheken werden jedoch unterstützt.
- Sie können Xamarin Hot Restart nicht verwenden, um App-Bundles für die Veröffentlichung zu erstellen. Sie benötigen weiterhin einen Mac, um eine vollständige Kompilierung, Signierung und Bereitstellung Ihrer Anwendung für die Produktion durchzuführen.

## <a name="troubleshoot"></a>Problembehandlung
- Der Setup-Assistent erkennt iTunes nicht, wenn es über den Microsoft Store installiert wurde. Sie müssen diese Version zuerst deinstallieren und dann den [Installer von Apple](https://go.microsoft.com/fwlink/?linkid=2101014) herunterladen.
- Es gibt ein bekanntes Problem, bei dem durch aktivierte gerätespezifische Builds verhindert wird, dass die App in den Debugmodus wechselt. Problemumgehung: Deaktivieren Sie aktivierte gerätespezifische Builds unter **Properties > iOS Build** (Eigenschaften > iOS-Build), und versuchen Sie noch einmal, den Debugmodus zu starten. Dieses Problem wird in einem kommenden Release behoben.
- Wenn die App bereits auf dem Gerät vorhanden ist, kann der Versuch der Bereitstellung mit Hot Restart mit einem `AMDeviceStartHouseArrestService`-Fehler fehlschlagen. Das Problem kann umgangen werden, indem die App auf dem Gerät deinstalliert wird und dann noch mal bereitgestellt wird.

Verwenden Sie bitte das Feedbacktool unter [Hilfe > Feedback senden > Ein Problem melden](/visualstudio/ide/feedback-options?view=vs-2019#report-a-problem), um zusätzliche Probleme zu melden.