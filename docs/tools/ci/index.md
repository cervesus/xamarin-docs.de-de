---
title: Einführung in Continuous Integration mit Xamarin
description: Dieses Dokument enthält Links zu Leitfäden, die fortlaufende Integration in Xamarin zu beschreiben. Verknüpfter Inhalt bietet einen Überblick über die fortlaufende Integration und erläutert die App Center Build, TeamCity und Jenkins.
ms.prod: xamarin
ms.assetid: 99484E96-DC69-4697-8BBB-1B44C5CBB5ED
author: lobrien
ms.author: laobri
ms.date: 10/23/2018
ms.openlocfilehash: 9171eeb461f2e4d363de6e01ea92fa448f04d41a
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50121632"
---
# <a name="introduction-to-continuous-integration-with-xamarin"></a>Einführung in Continuous Integration mit Xamarin

> [!Video https://youtube.com/embed/wXgnh2Q7Uv8]

## <a name="introduction-to-continuous-integrationtoolsciintro-to-cimd"></a>[Einführung in Continuous Integration](~/tools/ci/intro-to-ci.md)

Dieser Abschnitt behandelt die verschiedenen Komponenten, die bei der continuous Integration und ihre Beziehungen. Es wird beschrieben, die continuous Integration-Umgebungen, die in den entsprechenden Abschnitten weiter unten erläutert werden.

## <a name="devops-with-xamarintoolscidevopsmd"></a>[DevOps mit Xamarin](~/tools/ci/devops.md)

In diesem Abschnitt werden die DevOps-Funktionen in Azure und Visual Studio, die Sie erwarten können, auch bei einem Xamarin-Projekt ordnungsgemäß funktionieren.

## <a name="working-with-continuous-integration-environments"></a>Arbeiten mit Umgebungen für fortlaufende Integration

### <a name="build-xamarin-apps-with-azure-pipelineshttpsdocsmicrosoftcomazuredevopspipelineslanguagesxamarin"></a>[Erstellen Sie Xamarin-apps mit Azure-Pipelines](https://docs.microsoft.com/azure/devops/pipelines/languages/xamarin/)

Verwenden Sie Azure-Pipelines, um Xamarin-apps für Android und iOS automatisch zu erstellen.

### <a name="build-xamarin-apps-using-app-centerhttpsdocsmicrosoftcomappcenterbuildxamarin"></a>[Erstellen Sie Xamarin-apps mit App Center](https://docs.microsoft.com/appcenter/build/xamarin/)

Erstellen von Xamarin.iOS und Xamarin.Android-Lösungen mit App Center direkt aus GitHub, Bitbucket oder ein Azure DevOps.

### <a name="build-xamarin-apps-with-teamcitytoolsciteamcitymd"></a>[Erstellen von Xamarin-apps mit TeamCity](~/tools/ci/teamcity.md)

Dieser Leitfaden erläutert die Schritte zum Verwenden von TeamCity zum Kompilieren von mobilen apps, und diese dann an App Center-Test zu senden.

### <a name="build-xamarin-apps-with-jenkinstoolscijenkins-walkthroughmd"></a>[Erstellen von Xamarin-apps mit Jenkins](~/tools/ci/jenkins-walkthrough.md)

Dieser Leitfaden veranschaulicht, wie Sie Jenkins als continuous Integrationsserver einrichten und automatisieren, Kompilieren die mobilen apps mit Xamarin erstellt wurden. Es wird beschrieben, wie zum Installieren von Jenkins auf OS X, konfigurieren und Einrichten der Aufträge, Xamarin.iOS und Xamarin.Android-apps zu kompilieren, wenn Änderungen an das System zur Versionskontrolle übergeben werden.
