---
title: Xamarin Hot Restart
description: In diesem Dokument wird die Einrichtung und Verwendung von Xamarin Hot Restart zum Debuggen einer iOS-App beschrieben.
ms.prod: xamarin
ms.assetid: 6BC62A88-9368-41BB-8494-760F2A4805DB
ms.technology: xamarin-forms
author: maddyleger1
ms.author: maleger
ms.date: 03/16/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 45b7d0d20c43aa22ebde3a17552f10ceea77a48b
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84139138"
---
# <a name="xamarin-hot-restart-preview"></a>Xamarin Hot Restart (Preview)

![Feature der Vorschauversion](~/media/shared/preview.png)

Mit Xamarin Hot Restart können Sie Änderungen an Ihrer App während der Entwicklung, der Bearbeitung von Code in mehreren Dateien, Ressourcen und Verweisen schnell testen. Die neuen Änderungen am bestehenden App-Bundle werden an das Debugziel gepusht, wodurch ein wesentlich schnellerer Entwicklungs- und Bereitstellungszyklus ermöglicht wird.

> [!IMPORTANT]
> Xamarin Hot Restart ist derzeit in Visual Studio 2019 Version 16.5 (stabil) verfügbar und unterstützt iOS-Apps mit Xamarin.Forms. Die Unterstützung für Visual Studio für Mac und Nicht-Xamarin.Forms-Apps befindet sich in Planung.

## <a name="requirements"></a>Anforderungen

- Visual Studio 2019 Version 16.5
- iTunes (64 Bit)
- Apple Developer-Konto und kostenpflichtige Registrierung beim [Apple-Entwicklerprogramm](https://developer.apple.com/programs)


## <a name="initial-setup"></a>Erste Einrichtung

> [!NOTE]
> Xamarin Hot Restart ist in der Vorschauversion standardmäßig deaktiviert. Sie können die Funktion unter **Tools > Optionen... > Umgebung > Vorschaufeatures > Xamarin Hot Restart aktivieren** aktivieren.

1. Stellen Sie sicher, dass das iOS-Projekt als Startupprojekt festgelegt und die Buildkonfiguration auf **Debug|iPhone** (Debuggen |iPhone) festgelegt ist.

   1. Wenn es sich um ein bestehendes Projekt handelt, navigieren Sie zu **Erstellen > Konfigurations-Manager...** , und stellen Sie sicher, dass **Bereitstellen** für das iOS-Projekt aktiviert ist.

2. Wählen Sie **Lokales Gerät** in der Symbolleiste aus, und klicken Sie darauf, um den Setup-Assistenten zu starten:

    [![](hot-restart-images/toolbar.png "Screenshot of the Visual Studio toolbar with local device set as the debug target.")](hot-restart-images/toolbar.png)

3. Wenn iTunes nicht installiert ist, klicken Sie auf **Download iTunes** (iTunes herunterladen), um den Installer herunterzuladen. Klicken Sie auf **Weiter**, wenn die iTunes-Installation fertiggestellt ist.

4. Verbinden Sie ein iOS-Gerät mit Ihrem Computer. Wurde das Gerät bereits eingesteckt, stecken Sie es aus, und schließen Sie es erneut an. Der Gerätename wird im Assistenten angezeigt, sobald er erkannt wurde. Klicken Sie auf **Weiter**.

5. Geben Sie Ihre Anmeldeinformationen für Ihr Apple Developer-Konto ein, und klicken Sie auf **Weiter**.

6. Wählen Sie im Dropdownmenü ein Entwicklungsteam aus, um die [automatische Bereitstellung](~/ios/get-started/installation/device-provisioning/automatic-provisioning.md) im Projekt zu aktivieren. Klicken Sie auf **Fertig stellen**.

> [!NOTE]
> Die Verwendung der automatischen Bereitstellung wird empfohlen, damit zusätzliche iOS-Geräte einfach für die Bereitstellung konfiguriert werden können. Sie können diese jedoch deaktivieren und die manuelle Bereitstellung weiterhin verwenden, wenn die richtigen Bereitstellungsprofile vorhanden sind.

## <a name="use-xamarin-hot-restart"></a>Verwenden von Xamarin Hot Restart
Nach der anfänglichen Einrichtung erscheint Ihr verbundenes Gerät im Dropdownmenü für das Debugziel. Wählen Sie Ihr Gerät im Dropdownmenü aus, und klicken Sie auf die Schaltfläche **Ausführen**, um Ihre App zu debuggen. Möglicherweise wird Ihnen in Visual Studio eine Benachrichtigung mit der Bitte angezeigt, die App auf dem Gerät manuell zu starten, um die Debugsitzung zu beginnen.

Sie können während des Debuggens Änderungen an Ihren Codedateien vornehmen und dann die Schaltfläche **Neu starten** in der Debugsymbolleiste drücken oder **STRG+Umschalt+F5** verwenden, um die Debugsitzung mit Ihren neuen Änderungen neu zu starten:

[![](hot-restart-images/restart.png "Screenshot of the debug toolbar with the restart button highlighted.")](hot-restart-images/toolbar.png)

Sie können auch das Präprozessorsymbol `HOTRESTART` verwenden, um die Ausführung von bestimmtem Code beim Debuggen mit Xamarin Hot Restart zu verhindern.

## <a name="limitations"></a>Einschränkungen

- Nur iOS-Apps, die mit Xamarin.Forms und iOS-Geräten erstellt wurden, werden derzeit unterstützt.
- Nur 64-Bit-iOS-Geräte werden unterstützt. Ab iOS 11 ist es nicht mehr möglich, iOS-Apps in der 32-Bit-Architektur (Geräte vor iPhone 5S) auszuführen.
- Storyboard- und XIB-Dateien werden nicht unterstützt, und die Anwendung kann abstürzen, wenn sie versucht, diese zur Laufzeit zu laden. Verwenden Sie das Präprozessorsymbol `HOTRESTART`, um die Ausführung dieses Codes zu verhindern.
- Statische iOS-Bibliotheken und -Frameworks werden nicht unterstützt. Es kann zu Laufzeitfehlern oder Abstürzen kommen, wenn Ihre App versucht, diese zu laden. Verwenden Sie das Präprozessorsymbol `HOTRESTART`, um die Ausführung dieses Codes zu verhindern. Dynamische iOS-Bibliotheken werden unterstützt.
- Sie können Xamarin Hot Restart nicht verwenden, um App-Bundles für die Veröffentlichung zu erstellen. Sie benötigen weiterhin einen Mac, um eine vollständige Kompilierung, Signierung und Bereitstellung Ihrer Anwendung für die Produktion durchzuführen.

## <a name="troubleshoot"></a>Problembehandlung

- Der Setup-Assistent erkennt iTunes nicht, wenn es über den Microsoft Store installiert wurde. Sie müssen diese Version zuerst deinstallieren und dann den [Installer von Apple](https://go.microsoft.com/fwlink/?linkid=2101014) herunterladen.
- Es gibt ein bekanntes Problem, bei dem durch aktivierte gerätespezifische Builds verhindert wird, dass die App in den Debugmodus wechselt. Problemumgehung: Deaktivieren Sie aktivierte gerätespezifische Builds unter **Properties > iOS Build** (Eigenschaften > iOS-Build), und versuchen Sie noch einmal, den Debugmodus zu starten. Dieses Problem wird in einem kommenden Release behoben.
- Wenn die App bereits auf dem Gerät vorhanden ist, kann der Versuch der Bereitstellung mit Hot Restart mit einem `AMDeviceStartHouseArrestService`-Fehler fehlschlagen. Das Problem kann umgangen werden, indem die App auf dem Gerät deinstalliert wird und dann noch mal bereitgestellt wird.
- Die Eingabe einer Apple-ID, die nicht Teil des Apple-Entwicklerprogramms ist, kann zu folgendem Fehler führen: `Authentication Error. Xcode 7.3 or later is required to continue developing with your Apple ID`. Sie benötigen ein gültiges Apple Developer-Konto, um Xamarin Hot Restart auf iOS-Geräten verwenden zu können. 

Verwenden Sie bitte das Feedbacktool unter [Hilfe > Feedback senden > Ein Problem melden](/visualstudio/ide/feedback-options?view=vs-2019#report-a-problem), um zusätzliche Probleme zu melden.
