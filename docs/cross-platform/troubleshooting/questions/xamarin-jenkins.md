---
title: Warum wird von Xamarin nicht Jenkins unterstützt?
description: Dieses Dokument beschreibt auf hoher Ebene, die Xamarin Interaktion mit der Jenkins CI System. Darüber hinaus werden einige allgemeine Probleme, die aufgerufen werden, bei der Arbeit mit Jenkins erläutert.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 9951F980-2C6C-47C0-8A35-A78F06C20BEB
author: asb3993
ms.author: amburns
ms.openlocfilehash: cf1a59d3084f178187209fdf3999af10efe6203a
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34782449"
---
# <a name="why-isnt-jenkins-supported-by-xamarin"></a>Warum wird von Xamarin nicht Jenkins unterstützt?

## <a name="jenkins-support-explanation"></a>Erklärung Jenkins-Unterstützung

Jenkins ist ein Open Source-CI; Viele Probleme, die direkt durch die Jenkins entstehen aufgrund von *selbst* müssen als Probleme vor, in dem Sie den Code erhalten haben, z. B. abgelegt werden die [main Jenkins-Repository](https://github.com/jenkinsci/jenkins), oder das Repository für [ Jenkins.app](https://github.com/stisti/jenkins-app).

Eine Ausnahme ist für Probleme, die auf bestimmte Fehler in die Xamarin Tools isoliert werden können. Wenn Sie diese Option, um die Groß-/Kleinschreibung vermuten sehen Sie sich Ihre [Supportoptionen](~/cross-platform/troubleshooting/support-options.md), obwohl Sie erneut, das Problem möglicherweise etwas außerhalb der Xamarin-Unterstützung Team kann *direkt* mit Hilfe.

## <a name="setup-jenkins-with-xamarin"></a>Setup Jenkins mit Xamarin

Während wie oben bereits erwähnt Jenkins Probleme direkt durch Team nicht unterstützt werden; die [Jenkins mit Xamarin verwenden](~/tools/ci/jenkins-walkthrough.md) Handbuch kann verwendet werden, um einen Jenkins CI-Server einzurichten, die mit Xamarin integriert ist. 

## <a name="fixes-for-common-issues"></a>Fehlerbehebungen für häufige Probleme

### <a name="jenkins-is-unable-to-find-the-android-sdk"></a>Jenkins wurde der Android-SDK gefunden

Die Fehlermeldung für dieses Problem ist etwa so aussehen:

> Fehler XA5205: das Android SDK-Verzeichnis wurde nicht gefunden. Legen Sie über /p:AndroidSdkDirectory

Die Optionen zum Festlegen der SDK-Speicherorts variiert in Abhängigkeit der genaue Jenkins Android-Plug-in, die Sie verwenden; eine gute nach dazu gesucht werden soll, ist in der Plug-in-Handbuch. Beispiel: die [-Plug-In für Android-Emulator](https://wiki.jenkins-ci.org/display/JENKINS/Android+Emulator+Plugin#AndroidEmulatorPlugin-Systemconfiguration) sucht automatisch nach dem SDK, aber wenn es nicht gefunden; der Speicherort kann auch über die Systemkonfiguration Jenkins-Seite für diese-Plug-in festgelegt werden. 


## <a name="deprecated-errors"></a>Als veraltet markierte Fehler

> [!IMPORTANT]
> Dieses Problem wurde in den neuesten Versionen von Xamarin behoben. Allerdings tritt das Problem auf die neueste Version der Software, bitte der Datei eine [neuen Fehler](~/cross-platform/troubleshooting/questions/howto-file-bug.md) mit Ihrer vollständigen versionsverwaltung-Informationen "und" vollständig "Ausgabeprotokoll erstellen.



### <a name="jenkins-reports-an-invalid-xamarin-license"></a>Jenkins meldet eine ungültige Xamarin-Lizenz
Die Fehlermeldungen für dieses Problem sind in der Regel etwa

> XA9008-Fehler: Erstellen von der Befehlszeile ist eine Business-Lizenz erforderlich.

oder

> Fehler: Die Starter Edition von Xamarin.iOS Gebäude außerhalb von Xamarin Studio nicht unterstützt 

Die häufigste Ursache für dieses Szenario ist die Verwendung von Jenkins über die Anmeldung mit einem Benutzerkonto, das nicht mit Xamarin-Lizenz zugeordnet. Die einfachste Möglichkeit zum Auflösen von diesem ist Jenkins als eine app direkt über das Benutzerkonto installiert. Dieser Prozess und einige zusätzliche Aspekte berücksichtigt werden hier beschrieben: [https://forums.xamarin.com/discussion/comment/99397/#Comment_99397](https://forums.xamarin.com/discussion/comment/99397/#Comment_99397)
