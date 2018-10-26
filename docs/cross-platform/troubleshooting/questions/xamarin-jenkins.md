---
title: Warum wird Jenkins nicht von Microsoft unterstützt?
description: Dieses Dokument beschreibt auf hoher Ebene, die Xamarin Interaktion mit dem Jenkins-CI-System. Darüber hinaus werden einige allgemeine Probleme, die bei der Arbeit mit Jenkins erläutert.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 9951F980-2C6C-47C0-8A35-A78F06C20BEB
author: asb3993
ms.author: amburns
ms.date: 06/05/2018
ms.openlocfilehash: c2e409b796d5ef2525079e02aafdd0c6e8db5d81
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50113442"
---
# <a name="why-isnt-jenkins-supported-by-microsoft"></a>Warum wird Jenkins nicht von Microsoft unterstützt?

## <a name="jenkins-support-explanation"></a>Erläuterung der Jenkins-Unterstützung

Jenkins ist ein Open-Source-CI-Suite. aufgrund von diesem vieler Probleme, die direkt von der Jenkins verursacht werden *selbst* müssen gesendet werden, wie Probleme vor, in dem Sie den Code abgerufen haben, z.B. die [main Jenkins-Repository](https://github.com/jenkinsci/jenkins), oder das Repository für [ Jenkins.app](https://github.com/stisti/jenkins-app).

Die Ausnahme ist für Probleme, die zu bestimmten Fehlern im Xamarin Tools isoliert werden können. Wenn Sie diese Option, um die Groß-/Kleinschreibung werden vermuten sehen Sie sich Ihre [Supportoptionen](~/cross-platform/troubleshooting/support-options.md), obwohl in diesem Fall das Problem möglicherweise etwas außerhalb der Xamarin-Unterstützung kann Team *direkt* unterstützen.

## <a name="setup-jenkins-with-xamarin"></a>Einrichten von Jenkins mit Xamarin

Während der oben genannten Jenkins Probleme direkt von unserem Team nicht unterstützt werden; die [mithilfe von Jenkins mit Xamarin](~/tools/ci/jenkins-walkthrough.md) Leitfaden kann verwendet werden, um ein Jenkins-CI-Server einrichten, die mit Xamarin integriert ist. 

## <a name="fixes-for-common-issues"></a>Lösungen für übliche Probleme

### <a name="jenkins-is-unable-to-find-the-android-sdk"></a>Jenkins ist nicht das Android SDK gefunden.

Die Fehlermeldung für dieses Problem lautet etwa wie folgt:

> Fehler XA5205: der Android SDK-Verzeichnis wurde nicht gefunden. Legen Sie über /p:AndroidSdkDirectory

Die Optionen zum Festlegen der SDK-Speicherorts variieren je nach den genauen Android für Jenkins-Plug-in, die Sie verwenden; eine gute Möglichkeit zum Konfigurieren dieses gesucht ist im Handbuch-Plug-in. Beispiel: die [-Plug-In für Android-Emulator](https://wiki.jenkins-ci.org/display/JENKINS/Android+Emulator+Plugin#AndroidEmulatorPlugin-Systemconfiguration) sucht automatisch nach dem SDK, aber wenn es nicht gefunden werden; der Speicherort kann auch über die Systemkonfiguration der Jenkins-Seite,-Plug-in festgelegt werden. 


## <a name="deprecated-errors"></a>Als veraltet markierte Fehler

> [!IMPORTANT]
> In früheren Versionen von Xamarin hat dieses Problem behoben wurde. Jedoch, wenn das Problem auf die neueste Version der Software auftritt, melden Sie bitte eine [neuer Fehler](~/cross-platform/troubleshooting/questions/howto-file-bug.md) erstellen Sie mit der Ihre vollständige versionsverwaltung Informationen "und" vollständig "Protokollausgabe.



### <a name="jenkins-reports-an-invalid-xamarin-license"></a>Jenkins meldet eine ungültige Xamarin-Lizenz
Die Fehlermeldungen für dieses Problem sind in der Regel etwa

> XA9008-Fehler: Erstellen von Befehlszeile ist eine Business-Lizenz erforderlich.

oder

> Fehler: Die Starter Edition von Xamarin.iOS unterstützt kein erstellen, die außerhalb von Xamarin Studio 

Die häufigste Ursache für dieses Szenario ist die Verwendung von Jenkins, indem Sie sich mit einem Benutzerkonto nicht Ihre Xamarin-Lizenz zugeordnet. Die einfachste Möglichkeit zum Auflösen von diesem ist Jenkins direkt über das Benutzerkonto, das als app installiert. Dieser Prozess und einige zusätzliche Aspekte berücksichtigt werden hier beschrieben: [https://forums.xamarin.com/discussion/comment/99397/#Comment_99397](https://forums.xamarin.com/discussion/comment/99397/#Comment_99397)
