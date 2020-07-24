---
title: Fortlaufende Integration in Xamarin
description: Dieses Dokument ist mit Anleitungen verknüpft, die Continuous Integration mit xamarin beschreiben. Verknüpfter Inhalt bietet einen Überblick über Continuous Integration und erläutert App Center Build, TeamCity und Jenkins.
ms.prod: xamarin
ms.assetid: 99484E96-DC69-4697-8BBB-1B44C5CBB5ED
author: davidortinau
ms.author: daortin
ms.date: 10/23/2018
ms.openlocfilehash: 9c87a65481ca58b2861c40a420459d629852f6b6
ms.sourcegitcommit: 952db1983c0bc373844c5fbe9d185e04a87d8fb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "86997422"
---
# <a name="continuous-integration-with-xamarin"></a>Fortlaufende Integration in Xamarin

> [!Video https://youtube.com/embed/wXgnh2Q7Uv8]

## <a name="introduction-to-continuous-integration"></a>[Einführung in Continuous Integration](~/tools/ci/intro-to-ci.md)

In diesem Abschnitt werden die verschiedenen Komponenten behandelt, die mit Continuous Integration und ihren Beziehungen verbunden sind. Es beschreibt die Continuous Integration Umgebungen, die in den folgenden Abschnitten erläutert werden.

## <a name="devops-with-xamarin"></a>[DevOps mit Xamarin](~/tools/ci/devops.md)

In diesem Abschnitt wird beschrieben, welche devops-Features in Azure und Visual Studio mit einem xamarin-Projekt gut funktionieren.

## <a name="working-with-continuous-integration-environments"></a>Arbeiten mit Continuous Integration-Umgebungen

### <a name="build-xamarin-apps-with-azure-pipelines"></a>[Erstellen von xamarin-apps mit Azure Pipelines](https://docs.microsoft.com/azure/devops/pipelines/languages/xamarin/)

Verwenden Sie Azure Pipelines, um automatisch xamarin-Apps für Android und IOS zu erstellen.

### <a name="build-xamarin-apps-using-app-center"></a>[Erstellen von xamarin-apps mit App Center](https://docs.microsoft.com/appcenter/build/xamarin/)

Erstellen Sie xamarin. IOS-und xamarin. Android-Lösungen mit App Center direkt aus GitHub, Azure devops oder Bitbucket.

### <a name="build-xamarin-apps-with-teamcity"></a>[Erstellen von xamarin-apps mit TeamCity](~/tools/ci/teamcity.md)

In diesem Handbuch werden die Schritte beschrieben, die für die Verwendung von TeamCity zum Kompilieren mobiler apps und deren Übermittlung an App Center Test ausgeführt werden müssen.

### <a name="build-xamarin-apps-with-jenkins"></a>[Erstellen von xamarin-apps mit Jenkins](~/tools/ci/jenkins-walkthrough.md)

In dieser Anleitung wird veranschaulicht, wie Sie Jenkins als Continuous Integration Server einrichten und die Kompilierung mobiler apps automatisieren, die mit xamarin erstellt wurden. Es wird beschrieben, wie Sie Jenkins unter OS X installieren, konfigurieren und Aufträge zum Kompilieren von xamarin. IOS-und xamarin. Android-Apps einrichten, wenn Änderungen an das Versionskontrollsystem übertragen werden.
