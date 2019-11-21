---
title: Einführung in fastlane für iOS
description: Dieser Leitfaden beschreibt die verschiedenen fastlane-Tools zum Hinzufügen einer Codesignatur bei iOS-Anwendungen. Es wird beschrieben, wie Sie fastlane-Tools aktualisieren, installieren und verwenden.
ms.prod: xamarin
ms.assetid: 8202C57D-22FF-4224-A5B1-AAEF12B7C106
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/19/2017
ms.openlocfilehash: 0af85c1c27d2b329d81cc680a0fc4c075d4a86dd
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73028604"
---
# <a name="introduction-to-fastlane-for-ios"></a>Einführung in fastlane für iOS

Fastlane ist ein Open Source-Projekt, das der Vereinfachung des verwirrenden und häufig mühsamen Freigabeprozesses von iOS- und Android-Apps dienen soll. Es enthält verschiedene Dienstprogramme, von denen jedes einen bestimmten Aspekt der App-Freigabe behandelt:

- [deliver](https://github.com/fastlane/fastlane/tree/master/deliver#readme): Verwalten und Hochladen von Screenshots, Metadaten und Anwendungsbündeln zu iTunes Connect.
- [produce](https://github.com/fastlane/fastlane/tree/master/produce#readme): Erstellen einer App in iTunes Connect und im Entwicklerportal (häufig als AppID bezeichnet). Darüber hinaus Unterstützung für App-Gruppen und Anwendungsdienste.
- [pem](https://github.com/fastlane/fastlane/tree/master/pem#readme): Erstellen und Verwalten von Pushbenachrichtigungs-Bereitstellungsprofilen.
- [gym](https://github.com/fastlane/fastlane/tree/master/gym#readme): Erstellen und Signieren einer iOS-Anwendung (Xamarin-Apps verwenden bereits MSBuild zum Erstellen, Signieren und Archivieren von Apps).
- [cert](https://github.com/fastlane/fastlane/tree/master/cert#readme): Erstellen und Verwalten von Codesignaturzertifikaten. 
- [sigh](https://github.com/fastlane/fastlane/tree/master/sigh#readme): Erstellen und Verwalten von Bereitstellungsprofilen.
- [match](https://github.com/fastlane/fastlane/tree/master/match#readme): Erstellen und Verwalten von Zertifikaten und Profilen und anschließendes Speichern in einem Git-Repository, um sie über ein Entwicklungsteam zu synchronisieren.

Fastlane kann auf unterschiedliche Weise verwendet werden: über Terminalbefehle, über dateibasierte Mittel oder mithilfe von Umgebungsvariablen für fortlaufende Integrationsbuilds. 

In diesem Leitfaden wird insbesondere das Einrichten eines Geräts für die Entwicklung mit iOS-Apps behandelt. Dabei liegt der Schwerpunkt auf den drei Dienstprogrammen **cert**, **sigh** und **match**. 

Der Inhalt dieser Anleitung kann als Ausgangspunkt für die Hilfe bei der App-Verteilung verwendet werden, einschließlich der vollständigen Automatisierung des Prozesses auf einem Server für fortlaufende Integration. Sie sollten allerdings beachten, dass fastlane von einem Drittanbieter stammt, der Tools zur Unterstützung von Xcode-Projekten bereitstellt. Es ist daher möglich, dass einige Tools oder Befehle, wie z.B. `fastlane init`, mit CSPROJ-Dateien nicht wie erwartet funktionieren. Weitere Informationen zur Verwendung von fastlane, zusätzlichen Tools oder Freigaben für Android mithilfe von fastlane finden Sie unter [https://fastlane.tools/](https://fastlane.tools/).

<a name="Installation" />

## <a name="installation"></a>Installation

1. Stellen Sie sicher, dass Xcode-Befehlszeilentools auf Ihrem macOS-Computer installiert sind. Verwenden Sie zum Installieren der Tools den Befehl `xcode-select --install` in Terminal. Wenn die Tools bereits installiert sind, wird der folgende Fehler angezeigt:

    ```bash
    error: command line tools are already installed, use "Software Update" to install updates
    ```

2. Laden Sie fastlane-Tools hier herunter: [https://download.fastlane.tools](https://download.fastlane.tools). 

    > [!NOTE]
    > Sie können fastlane-Tools auch über Homebrew mit `brew cask install fastlane` installieren oder über Rubygems (2.0 oder höher) mit `sudo gem install fastlane –NV`. Bei der Verwendung des Installers wird jedoch sichergestellt, dass die richtigen Abhängigkeiten verfügbar sind. 

3. Entzippen Sie die Datei, und doppelklicken Sie auf die ausführbare Datei `install`, um fastlane zu installieren. Wird ein Fehler angezeigt mit der Meldung, dass die Datei „nicht geöffnet werden kann, da sie von einem unbekannten Entwickler stammt“, klicken Sie auf OK, und führen Sie folgende Schritte aus:
    - Halten Sie STRG gedrückt, und klicken Sie auf die ausführbare Datei `install`. Dadurch wird das folgende Dialogfeld angezeigt:

     ![](images/fastlane-image12.png "The install dialog")

    - Klicken Sie auf OK, um die Installation der fastlane-Tools zu starten.

4. Terminal zeigt Ihnen das folgende Dialogfeld an. Drücken Sie `y`:

   ![](images/fastlane-image13.png "The Terminal prompt")

5. Führen Sie `which fastlane` aus, bevor Sie fastlane zum ersten Mal verwenden. Der Pfad sollte wie folgt aussehen: 

    ```bash
    /Users/[user]/.fastlane/bin
    ```

6. Stimmt der Pfad überein, dann können Sie beginnen.

     Andernfalls führen Sie die folgenden Schritte aus:  Öffnen Sie mit dem folgenden Befehl in macOS die Datei `.bash_profile` (eine versteckte Klartextdatei im Basisverzeichnis):

    ```bash
    open ~/.bash_profile
    ```

7. Fügen Sie die folgende PATH-Umgebungsvariable hinzu, und speichern Sie sie: 

    ```bash
    export PATH="$HOME/.fastlane/bin:$PATH"
    ```

8. Führen Sie erneut `which fastlane` aus, um sicherzustellen, dass der Pfad wie folgt aussieht: `/Users/[user]/.fastlane/bin`.

## <a name="updating-fastlane"></a>Aktualisieren von fastlane

Fastlane ist ein sehr aktives Open Source-Projekt, das regelmäßig neue Versionen veröffentlicht. Sobald eine neue Version von fastlane verfügbar ist, werden Sie beim Ausführen eines beliebigen fastlane-Befehls zu Folgendem aufgefordert:

[![](images/fastlane-image0.png "The fast lane update prompt")](images/fastlane-image0.png#lightbox)

Laden Sie [hier](https://download.fastlane.tools) das neueste Paket herunter, um auf eine neue Version von fastlane zu aktualisieren. Doppelklicken Sie zum Ausführen auf das Installationspaket:

[![](images/fastlane-image0a.png "Running the install package")](images/fastlane-image0a.png#lightbox)

## <a name="contents"></a>Inhalt

In dieser Leitfadensammlung werden einige der Tools vorgestellt, die fastlane zum Hinzufügen einer Codesignatur bei Ihrer iOS-App als Vorbereitung zur Entwicklung oder Verteilung verwendet. Die derzeit behandelten Tools sind:

- [cert](~/ios/deploy-test/provisioning/fastlane/cert.md)
- [sigh](~/ios/deploy-test/provisioning/fastlane/sigh.md)
- [match](~/ios/deploy-test/provisioning/fastlane/match.md)

„cert“ und „sigh“ befassen sich mit dem Erstellen und Verwalten von Signaturzertifikaten und Bereitstellungsprofilen auf einem lokalen Computer. „match“ geht in diesem Prozess noch einen Schritt weiter. Es erstellt und verwaltet Zertifikate und Bereitstellungsprofile und speichert sie in einem Git-Repository. Dadurch können alle Mitglieder eines Entwicklungsteams darauf zugreifen. Lesen Sie alle Abschnitte durch, um die Funktionsweise der einzelnen Tools kennenzulernen, und finden Sie heraus, wie Sie diese nutzen können.

## <a name="using-fastlane-tools-with-xamarin"></a>Verwenden von fastlane-Tools mit Xamarin

Nachdem Sie mit fastlane eine Signierungsidentität und Bereitstellungsprofile erstellt haben, sollte das Festlegen der Bündelsignierungsoptionen in Visual Studio für Mac kein Problem sein. Voraussetzung ist, dass sich die Zertifikate und privaten Schlüssel in der macOS-Keychain befinden und die Bereitstellungsprofile im Ordner `~/Library/MobileDevice/Provisioning Profiles` vorliegen.

Zum Festlegen der Codesignierungsoptionen für eine Xamarin.iOS-Anwendung klicken Sie mit der rechten Maustaste auf den Projektnamen, und wählen Sie **Projektoptionen > Erstellen > iOS-Bundle-Signierung** aus. Legen Sie die Signierungsidentität und die Bereitstellungsprofile wie folgt eindeutig fest:

[![](images/fastlane-image11.png "Set the Signing Identity and Provisioning Profile explicitly")](images/fastlane-image11.png#lightbox)

## <a name="related-links"></a>Verwandte Links

- [fastlane Docs (fastlane-Dokumente)](https://fastlane.tools/)
- [fastlane Code Signing Docs (fastlane-Codesignaturdokumente)](https://docs.fastlane.tools/codesigning/getting-started/)
- [Code Signing guide (Leitfaden zum Hinzufügen einer Codesignatur)](https://codesigning.guide/)
