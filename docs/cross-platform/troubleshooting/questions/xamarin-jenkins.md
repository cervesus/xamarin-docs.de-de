---
title: Warum wird Jenkins von Microsoft nicht unterstützt?
description: In diesem Dokument wird die xamarin-Interaktion mit dem Jenkins CI-System auf hoher Ebene beschrieben. Außerdem werden einige häufige Probleme erläutert, die bei der Arbeit mit Jenkins auftreten.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 9951F980-2C6C-47C0-8A35-A78F06C20BEB
author: conceptdev
ms.author: crdun
ms.date: 06/05/2018
ms.openlocfilehash: 4f09f4ca97dcf50891aa0a0415e47d474297c411
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70282768"
---
# <a name="why-isnt-jenkins-supported-by-microsoft"></a>Warum wird Jenkins von Microsoft nicht unterstützt?

## <a name="jenkins-support-explanation"></a>Jenkins-Unterstützung Erklärung

Jenkins ist eine Open-Source-CI-Suite. Aufgrund dieser zahlreichen Probleme, die direkt von der Jenkins-Datei verursacht werden, müssen Sie als Probleme bei der Position des Codes *protokolliert werden.* beispielsweise das [Jenkins-Hauptrepository](https://github.com/jenkinsci/jenkins)oder das Repository für [Jenkins. app](https://github.com/stisti/jenkins-app).

Eine Ausnahme hiervon sind Probleme, die auf bestimmte Fehler in den xamarin-Tools isoliert werden können. Wenn Sie vermuten, dass dies der Fall ist, können Sie die [Supportoptionen](~/cross-platform/troubleshooting/support-options.md)überprüfen, aber das Problem kann etwas außerhalb der Dinge liegen, die das xamarin-Support Team *direkt* dabei unterstützen kann.

## <a name="setup-jenkins-with-xamarin"></a>Einrichten von Jenkins mit xamarin

Wie bereits erwähnt, werden Jenkins-Probleme von unserem Team nicht direkt unterstützt. Das Handbuch [using Jenkins with xamarin](~/tools/ci/jenkins-walkthrough.md) kann verwendet werden, um einen Jenkins-CI-Server einzurichten, der in xamarin integriert ist. 

## <a name="fixes-for-common-issues"></a>Korrekturen für häufige Probleme

### <a name="jenkins-is-unable-to-find-the-android-sdk"></a>Jenkins kann die Android SDK nicht finden.

Die Fehlermeldung für dieses Problem sieht etwa folgendermaßen aus:

> Fehler XA5205: Das Android SDK Verzeichnis konnte nicht gefunden werden. Legen Sie über/p: androidsdkdirectory fest.

Die Optionen zum Festlegen des SDK-Speicher Orts können je nach ausgewähltem Jenkins-Android-Plug-in variieren, das Sie verwenden. ein guter Ausgangspunkt für die Festlegung finden Sie im Plug-in-Handbuch. Beispiel: Das [Android-Emulator-Plug](https://wiki.jenkins-ci.org/display/JENKINS/Android+Emulator+Plugin#AndroidEmulatorPlugin-Systemconfiguration) -in sucht automatisch nach dem SDK, wenn es nicht gefunden werden kann. der Speicherort kann auch über die Jenkins-Systemkonfigurations Seite für dieses Plug-in festgelegt werden. 


## <a name="deprecated-errors"></a>Veraltete Fehler

> [!IMPORTANT]
> Dieses Problem wurde in den letzten Versionen von xamarin gelöst. Wenn das Problem jedoch in der aktuellen Version der Software auftritt, melden Sie einen [neuen Fehler](~/cross-platform/troubleshooting/questions/howto-file-bug.md) mit den vollständigen Versionsinformationen und der vollständigen buildprotokolleausgabe.



### <a name="jenkins-reports-an-invalid-xamarin-license"></a>Jenkins meldet eine ungültige xamarin-Lizenz.
Die Fehlermeldungen für dieses Problem lauten in der Regel wie folgt.

> XA9008-Fehler: Zum Entwickeln von der Befehlszeile wird eine Geschäftslizenz benötigt.

oder

> Fehler: Die Starter-Edition von xamarin. IOS unterstützt das aufbauen außerhalb von Xamarin Studio 

Die häufigste Ursache für dieses Szenario ist die Verwendung von Jenkins, indem Sie sich mit einem Benutzerkonto anmelden, das nicht mit ihrer xamarin-Lizenz verknüpft ist. Die einfachste Möglichkeit, dieses Problem zu beheben, besteht darin, Jenkins als APP direkt über das Benutzerkonto zu installieren. Dieser Prozess und einige weitere Überlegungen werden hier beschrieben:[https://forums.xamarin.com/discussion/comment/99397/#Comment_99397](https://forums.xamarin.com/discussion/comment/99397/#Comment_99397)
