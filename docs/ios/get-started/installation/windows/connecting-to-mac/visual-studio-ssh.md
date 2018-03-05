---
title: Wo ist mein Mac?
description: "Anweisungen für das Konfigurieren Ihres Macs für Visual Studio zum Erstellen von Xamarin.iOS-Projekten"
ms.topic: article
ms.prod: xamarin
ms.assetid: 4f792690-b3b1-4d56-a5e2-363b4f246059
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 491d7cea4b88fa44bb15e76dd92ecd5216ce9984
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="wheres-my-mac"></a>Wo ist mein Mac?

_Anweisungen für das Konfigurieren Ihres Macs für Visual Studio zum Erstellen von Xamarin.iOS-Projekten_

Die aktuelle **stabile** Version von Xamarin für Visual Studio interagiert anders mit Ihrem Mac als frühere Versionen[^](#earlier-versions).

Um einen Mac als Xamarin-Mac-Agent zu verwenden, müssen Sie die **Entfernte Anmeldung** auf Ihrem Mac aktivieren.

1. Öffnen Sie *Spotlight* (`Cmd-Space`), und suchen Sie nach **Entfernte Anmeldung**. Wählen Sie den Ergebniseintrag **Freigabe** aus. Dadurch werden im **Freigabe**-Panel die **Systemeinstellungen** geöffnet.

  ![](visual-studio-ssh-images/spotlight.png "Spotlight-Suche für Remoteanmeldung")

2. Aktivieren Sie in der **Dienst**-Liste auf der linken Seite das Kontrollkästchen **Entfernte Anmeldung**, um die Verbindung von Xamarin für Visual Studio zu Ihrem Mac zuzulassen.

  ![](visual-studio-ssh-images/sharing.png "Aktivieren Sie in der Liste „Dienst“ die Option „Remoteanmeldung“")

3. Stellen Sie sicher, dass die **Remoteanmeldung** so festgelegt ist, dass der Zugriff entweder für **Alle Benutzer** zugelassen wird oder dass Ihr Mac-Benutzername oder Ihre Mac-Benutzergruppe in der Liste der zugelassenen Benutzer auf der rechten Seite enthalten ist.

Ihr Mac sollte nun von Visual Studio erkannt werden, wenn dieser sich im selben Netzwerk befindet.
Wenn Visual Studio versucht, eine Verbindung zum Mac herzustellen, werden Sie von Visual Studio zum Anmelden mit Ihren Mac-Benutzeranmeldeinformationen aufgefordert.

## <a name="where-can-i-find-more-information"></a>Wo finde ich weitere Informationen?

Es ist eine Reihe von Handbüchern verfügbar, die Sie beim Einrichten und Verstehen des neuen Agents unterstützen. Diese sind im Folgenden aufgelistet:

- [Herstellen einer Verbindung mit Ihrem Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md)
- [Problembehandlung](~/ios/get-started/installation/windows/connecting-to-mac/troubleshooting.md)
- [Xamarin University – Xamarin-Mac-Agent](https://university.xamarin.com/lightninglectures/xamarin-mac-agent)

<a name="earlier-versions" />

## <a name="migrating-from-previous-versions"></a>Migrieren von früheren Versionen

Wenn Sie eine frühere Version von Xamarin für Visual Studio verwendet haben, sollten Sie mit der Anwendung **Xamarin.iOS-Buildhost** vertraut sein, die auf Ihrem Mac gestartet werden und dann über eine PIN-Nummer mit Ihrer Instanz von Visual Studio *gekoppelt* werden musste.

Die Buildhostanwendung ist für diese Version von Xamarin für Visual Studio nicht mehr erforderlich.
