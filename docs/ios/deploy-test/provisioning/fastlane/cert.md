---
title: Fastlane für iOS – Cert
description: 'In diesem Artikel wird Fastlane beschrieben. Ein Tool, das viele Bestandteile des Bereitstellungsprozesses der iOS-Anwendung automatisiert: Das Anfordern von Zertifikaten, das Hinzufügen eines Geräts in das Entwicklerportal von Apple, das Erstellen einer App-ID und mehr.'
ms.prod: xamarin
ms.assetid: 900FA6FF-F3C9-4D35-993E-B0D88E6B1883
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/19/2017
ms.openlocfilehash: ba0348ff0cf6dc394f67b3c5779fd49eb852673f
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73028520"
---
# <a name="fastlane-for-ios--cert"></a>Fastlane für iOS – Cert

> [!IMPORTANT]
> Fastlane empfiehlt das [`match`](~/ios/deploy-test/provisioning/fastlane/match.md)-Tool zum Generieren und Verwalten von Zertifikaten. Verwenden Sie `cert` nur direkt, wenn Sie vollständige Kontrolle möchten und sich mit dem Codesignieren auskennen. Mit dieser Aktion laden Sie die neueste Codesignierungsidentität herunter.

## <a name="overview"></a>Übersicht

Die Gerätebereitstellung wird normalerweise von jedem Mitglied eines Entwicklungsteams über Xcode oder im Apple Developer Portal ausgeführt. Sie umfasst mehrere Schritte:

- Anfordern eines Entwicklungszertifikats
- Hinzufügen eines Geräts zum Portal
- Erstellen einer App-ID
- Erstellen eines Bereitstellungsprofils
- Herunterladen von Profilen und Zertifikaten

Jeder dieser Schritte enthält Variablen, die behoben werden müssen, und die von der von Ihnen entwickelten Anwendungsart abhängig sind. Weitere Informationen über die erforderlichen Schritte, mit denen ein Geräts für die Entwicklung eingerichtet und entweder manuell oder über Xcode bereitgestellt werden kann, finden Sie in der Anleitung zur [Gerätebereitstellung](~/ios/get-started/installation/device-provisioning/index.md).

In dieser Anleitung werden Fastlane-Tools eingeführt, die alternativ zu Xcode verwendet werden können, und es wird Folgendes erläutert:

- [Was ist Cert?](#whatiscert)
- [Verwenden von Cert](#using)
- [Zusätzliche Optionen](#options)

## <a name="installation"></a>Installation

Informationen zum Installieren und Aktualisieren von Fastlane finden Sie in der Einführung des [Fastlane](~/ios/deploy-test/provisioning/fastlane/index.md#Installation)-Leitfadens.

<a name="whatiscert" />

## <a name="what-is-cert"></a>Was ist Cert?

Cert bietet eine Terminalschnittstelle, die neue Codesignierungsidentitäten (oft als _Entwicklerzertifikat_ bezeichnet) für Entwicklungs- und Verteilungsumgebungen erstellt.

<a name="using" />

## <a name="using-cert"></a>Verwenden von Cert

Um das Cert-Hilfsprogramm zu verwenden, geben Sie den folgenden Befehl in die Terminal-CLI ein:

```
fastlane cert
```

Standardmäßig wird dadurch ein Verteilungszertifikat erstellt. Um ein Entwicklungszertifikat zu erstellen, übergeben Sie das `--development`-Flag:

```
fastlane cert --development
```

Geben Sie Ihre Apple-ID und Ihr Kennwort ein, wenn Cert Sie dazu auffordert:

[![](cert-images/fastlane-image1.png "cert will prompt for your Apple ID and password")](cert-images/fastlane-image1.png#lightbox)

> [!IMPORTANT]
> Wenn Sie Ihr Kennwort zum ersten Mal eingeben, wird es in der lokalen macOS-Keychain gespeichert. Alternativ können Sie Umgebungsvariablen zum Speichern des Benutzernamens und Kennworts verwenden, oder `export fastlane_DONT_STORE_PASSWORD=1` verwenden, wenn Sie nicht möchten, dass Ihr Kennwort in der Keychain gespeichert wird. Weitere Informationen zum Verwalten von Anmeldeinformationen mit Fastlane finden Sie im Leitfaden zu [CredentialsManager](https://github.com/fastlane/fastlane/blob/master/credentials_manager/README.md).

Die Apple-ID kann auch mithilfe des folgenden Befehls als Argument übergeben werden:

```
fastlane cert -u myemailadress@domain.com
```

Wenn Ihre Apple-ID mit mehreren Teams verbunden ist, werden sie hier angezeigt. Wählen Sie die Zahl aus, die dem Team entspricht, das Sie verwenden möchten:

[![](cert-images/fastlane-image2.png "Select the team that you wish to use")](cert-images/fastlane-image2.png#lightbox)

Die Team-ID kann auch mithilfe des folgenden Flags übergeben werden:

```
fastlane cert -l 2TU993NY9J
```

Fastlane überprüft, ob eines der verfügbaren Signaturzertifikate auf dem lokalen Computer installiert ist. Wenn dies der Fall ist, wird es verwendet.

Wenn kein Signaturzertifikat vorhanden ist, wird Cert

- einen neuen privaten Schlüssel erstellen und die Anforderung signieren,
- das Zertifikat generieren, herunterladen und installieren sowie
- Das Zertifikat und den privaten Schlüssel in die Keychain importieren

Wenn die maximale Anzahl von für Ihr Konto zulässigen Signierungsidentitäten erreicht wurde, wird eine Ausnahme ausgelöst. Wenn Sie eine neue Signierungsidentität erstellen möchten, müssen Sie eines der vorhandenen Zertifikate manuell über das Developer Center widerrufen und es erneut versuchen.

> [!NOTE]
> Fastlane kann keine vorhandenen Signierungsidentitäten aus dem Developer Center herunterladen, wenn sie nicht bereits in Ihrer Keychain vorhanden sind. Dies liegt daran, dass der private Schlüssel immer nur auf Ihrem Computer oder in der Version exported(*.p12) des Zertifikats und nie im Developer Center vorhanden ist.

<a name="options" />

## <a name="additional-options"></a>Zusätzliche Optionen

Die folgenden Optionen können verwendet werden, um zusätzliche Unterstützung bei der Verwendung von Cert zu bieten:

- Verwenden des `-–help`-Flags für eine Liste aller verfügbaren Befehle:

    ```
    fastlane cert --help
    ```

- Verwenden des `-–verbose`-Flags zur Erhöhung des Ausführlichkeitsgrads der Ausgabe

    ```
    fastlane cert --development --verbose
    ```

## <a name="related-links"></a>Verwandte Links

- [Fastlane – Cert](https://github.com/fastlane/fastlane/blob/master/cert/README.md)
