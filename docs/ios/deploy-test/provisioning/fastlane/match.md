---
title: "fastlane für iOS – match"
ms.topic: article
ms.prod: xamarin
ms.assetid: C4A2A67E-0643-4CED-B1A9-79D65054F3CA
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: d92f820e22277148b4de3ff87e3fdaca0f573f52
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/12/2018
---
# <a name="fastlane-for-ios---match"></a>fastlane für iOS – match

Die Gerätebereitstellung wird normalerweise von jedem Mitglied eines Entwicklungsteams über Xcode oder im Apple Developer-Portal ausgeführt. Sie umfasst mehrere Schritte:

- Anfordern eines Entwicklungszertifikats
- Hinzufügen eines Geräts zum Portal
- Erstellen einer App-ID
- Erstellen eines Bereitstellungsprofils
- Herunterladen von Profilen und Zertifikaten

Weitere Informationen über die erforderlichen Schritte, mit denen ein Gerät für die Entwicklung eingerichtet und entweder manuell oder über Xcode bereitgestellt werden kann, finden Sie in der Anleitung zur [Gerätebereitstellung](~/ios/get-started/installation/device-provisioning/index.md).

In dieser Anleitung wird gezeigt, wie fastlane-Tools alternativ zu Xcode verwendet werden können.

## <a name="installation"></a>Installation

Weitere Informationen zum Installieren von fastlane finden Sie in der Einführung des [fastlane](~/ios/deploy-test/provisioning/fastlane/index.md#Installation)-Leitfadens.

<a name="whatismatch" />

## <a name="what-is-match"></a>Was ist match?

match wird für das Erstellen und Verwalten von Codesignierungszertifikaten und Bereitstellungsprofilen verwendet und ermöglicht es einem iOS-Entwicklungsteam, eine Codesignierungsidentität für alle Entwickler freizugeben.

Beim Bereitstellen einer App für den App Store, beim Durchführen von Betatests oder dem Installieren der App auf einem Gerät besitzt jedes Mitglied eines Entwicklungsteams eine eigene Signierungsidentität. Dies kann zu in Konflikt stehenden Identitäten und Profilen führen, sodass Profile und App-IDs manuell erstellt, rotiert und verwaltet werden müssen.

Stattdessen erstellt und verwaltet match alle Zertifikate und Profile für Sie und speichert diese in einem privaten Git-Repository. Dadurch können alle Entwickler in einem Team auf die Anmeldeinformationen zugreifen und diese verwenden. Dies bedeutet zusätzliche Sicherheit für Ihre Zertifikate, denn sie befinden sich nicht nur in einem privaten Git-Repository, sondern werden ebenfalls mit einer [Passphrase](#passphrase) verschlüsselt. Das Speichern von Codesignierungsartefakten in einem Repository ermöglicht es Team-Agents und Administratoren, Zertifikate bei Bedarf zu aktualisieren und zu rotieren. Das bedeutet, dass weniger Zeit in das Verteilen neuer Zertifikate an jeden Entwickler investiert wird.

> [!IMPORTANT]
> match unterstützt derzeit keine internen Unternehmensprofile.

<a name="initializing" />

## <a name="initializing-your-project-with-match"></a>Initialisieren Ihres Projekts mit match

Wenn Sie der Teamadministrator sind, erstellen Sie entweder über github.com oder bitbucket.com ein privates Git-Repository, und stellen Sie sicher, dass alle Teammitglieder als Mitwirkende zum Repository hinzugefügt werden.

Verwenden Sie Ihr Terminal, um zum Projektverzeichnis zu wechseln und das Projekt auszuführen:

    fastlane match init

Geben Sie die URL des Git-Repositorys ein, wenn Sie dazu aufgefordert werden:

 [![](match-images/fastlane-image7.png "Eingabe der URL des Git-Repositorys")](match-images/fastlane-image7.png#lightbox)

Die URL kann wie im Folgenden dargestellt gefunden und kopiert werden, indem Sie auf die Schaltfläche **Clone or Download** (Klonen oder Herunterladen) auf github.com klicken:

[![](match-images/fastlane-image6.png "Die URL unter der Schaltfläche „Klonen“ oder „Herunterladen“ auf github.com")](match-images/fastlane-image6.png#lightbox)

Das Initialisieren des Projekts erstellt eine match-Datei. Dabei handelt es sich um eine Textdatei, die bearbeitet werden kann, um Umgebungsvariablen an das match-Tool zu übergeben. Im Folgenden finden Sie ein Beispiel für eine match-Datei:

[![](match-images/fastlane-image8.png "Ein Beispiel für matchfile")](match-images/fastlane-image8.png#lightbox)

<a name="running" />

## <a name="running-match"></a>Ausführen von match

> [!NOTE]
> fastlane empfiehlt, dass Sie Ihre vorhandenen Profile und Zertifikate mithilfe des [match-Befehls „nuke“](#using) löschen, bevor Sie match zum ersten Mal ausführen.

Abhängig davon, welche Umgebung Sie benötigen, können Sie jeden der folgenden Befehle verwenden, um ein neues Zertifikat und ein neues Bereitstellungsprofil zu erstellen und dieses in Ihrem neuen Git-Repository zu speichern:

    fastlane match appstore

    fastlane match adhoc

    fastlane match development

Zusätzlich zum Erstellen neuer Zertifikate und Profile fügt das Verwenden dieser Befehle jedes der folgenden Elemente zu Ihrem Git-Repository hinzu oder aktualisiert diese, falls sie bereits vorhanden sind:

- Ordner für Zertifikate
- Ordner für Profile
- Eine Infodatei mit grundlegenden Anweisungen
- Eine Version von match

[![](match-images/fastlane-image9.png "Die Projektstruktur im Git-Repository")](match-images/fastlane-image9.png#lightbox)

Bereitstellungsprofile werden in `~/Library/MobileDevice/Provisioning Profiles` installiert. Zertifikate und private Schüssel werden direkt in Ihrer Keychain installiert.

<a name="using" />

### <a name="using-the-nuke-command"></a>Verwenden des `nuke`-Befehls

Wenn Sie unsystematische Zertifikate besitzen, können Sie `nuke` verwenden, um Zertifikate und Profile für jede Umgebung mithilfe der folgenden Befehle zu widerrufen:

    fastlane match nuke

So widerrufen Sie alle Zertifikate und Bereitstellungsprofile für eine bestimmte Umgebung:

    fastlane match nuke development

 oder

    fastlane match nuke distribution

fastlane bestätigt die Dateien, die gelöscht werden sollen, bevor irgendetwas gelöscht wird.

<a name="passphrase" />

### <a name="passphrase"></a>Passphrase

Wenn Sie `match` zum ersten Mal ausführen, werden Sie dazu aufgefordert, eine Passphrase für das Git-Repository festzulegen. Merken Sie sich das Kennwort, da Sie dieses benötigen, um match auf einem anderen Computer auszuführen. Dies ist eine zusätzliche Sicherheitsebene, denn jede der Dateien wird mit OpenSSL verschlüsselt. Bei jeder weiteren Ausführung von `match` auf einem neuen Computer werden Sie zur Eingabe dieser Passphrase aufgefordert. Nachdem Sie die Passphrase zum ersten Mal eingegeben haben, wird diese zum lokalen Schlüsselbund hinzugefügt.

Verwenden Sie `MATCH_PASSWORD`, um die Passphrase für das Verschlüsseln Ihrer Profile mithilfe einer Umgebungsvariable festzulegen.

<a name="options" />

## <a name="additional-options"></a>Zusätzliche Optionen

Die folgenden Optionen können für zusätzliche Unterstützung bei der Verwendung von match verwendet werden:

- Verwenden Sie das `-–help`-Flag für eine Liste aller verfügbaren Befehle:

        fastlane match cert --help

- Verwenden Sie das `-–verbose`-Flag zur Erhöhung des Ausführlichkeitsgrads der Ausgabe:

        fastlane match --development --verbose

- Verwenden Sie das `--force_for_new_devices`-Flag, um die Bereitstellungsprofile zum Erneuern zu zwingen, wenn die Anzahl der Geräte im Entwicklerportal sich geändert hat.

        fastlane match development --force_for_new_devices

## <a name="related-links"></a>Verwandte Links

- [fastlane – match](https://github.com/fastlane/fastlane/blob/master/match/README.md)
