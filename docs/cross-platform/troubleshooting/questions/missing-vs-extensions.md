---
title: Fehlende Visual Studio-Erweiterungen nach der Installation
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 066d36a3-e553-48d6-8769-c972274d7641
author: asb3993
ms.author: amburns
ms.date: 03/20/2017
ms.openlocfilehash: 3e3d426e7b00725eafeba139de5bc46d416c368a
ms.sourcegitcommit: 650458de1d362cd7de174cacef7838f0e74426f3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/15/2019
ms.locfileid: "58070890"
---
# <a name="missing-visual-studio-extensions-after-installation"></a>Fehlende Visual Studio-Erweiterungen nach der Installation

## <a name="error-message-this-project-is-incompatible-with-the-current-edition-of-visual-studio"></a>Fehlermeldung: Dieses Projekt ist nicht kompatibel mit der aktuellen Edition von Visual Studio

Sicherstellen von Visual Studio 2017 (Community, Professional oder Enterprise) oder höher installiert ist.

Siehe auch die [Windows Anforderungen](~/cross-platform/get-started/requirements.md#windows-requirements).

## <a name="possible-fix-1-change-the-installation-to-make-sure-the-visual-studio-extensions-are-installed"></a>Mögliche Lösung 1: Ändern Sie die Installation aus, um sicherzustellen, dass Visual Studio-Erweiterungen installiert sind

In bestimmten Situationen möglicherweise die Xamarin-Installers deaktivieren Sie automatisch die Installationsoptionen für die Visual Studio-Erweiterungen. Wenn dies die Ursache des Problems ist, installieren Sie die fehlenden Visual Studio-Erweiterungen, die unter Verwendung des Installationsprogramms **Änderung** Befehl. Geben Sie beispielsweise Folgendes ein, um die Erweiterungen für Visual Studio 2013 installieren:

1. Öffnen Sie die Windows **Programme und Funktionen** Systemsteuerung.

2. Klicken Sie mit der rechten Maustaste auf die **Xamarin** Eintrag, und wählen Sie **Änderung**.

3. Klicken Sie auf **Weiter**, klicken Sie dann **Änderung**.

4. Stellen Sie sicher, dass die **Xamarin für Visual Studio 2013** Option wird festgelegt, um zu installieren:

    ![](missing-vs-extensions-images/installer.png "Aktivieren Sie Xamarin für Visual Studio 2013-Installation")

5. Fahren Sie mit der Rest des Installations-Assistenten fort.

## <a name="possible-fix-2-ask-visual-studio-to-set-up-the-extensions-again"></a>Mögliche Lösung 2: Bitten Sie Visual Studio, um das Erweiterungen erneut einrichten

1. Überprüfen Sie, ob die Xamarin-Erweiterungen in den Visual Studio-Extensions-Ordner kopiert wurden:

    `C:\Program Files (x86)\Microsoft Visual Studio 12.0\Common7\IDE\Extensions\Xamarin\Xamarin\3.1.228.0`

    Wenn die Erweiterungen (für Version 3.1.228) ordnungsgemäß installiert sind, werden im Ordner 60 Elemente sein:


    ![](missing-vs-extensions-images/folder.png "Liste der \"Xamarin\3.1.228.0\" Ordnerinhalt im Explorer")

2. Nachdem Sie bestätigt haben, dass dieser Ordner korrekt aussieht, teilen Sie Visual Studio zum Einrichten der Erweiterungen erneut versuchen:

    `"C:\Program Files (x86)\Microsoft Visual Studio 12.0\Common7\IDE\devenv.exe" /setup`

## <a name="possible-fix-3-try-a-fresh-reinstall-of-xamarin"></a>Das Update 3 möglich: Versuchen Sie eine neue Neuinstallation von Xamarin

1.  Deinstallieren Sie keines der folgenden, die vorhanden sind, in der Windows-Systemsteuerung:

    *   Xamarin

    *   Xamarin für Windows

    *   Xamarin.Android

    *   Xamarin.iOS

    *   Xamarin für Visual Studio

2.  Löschen Sie im Explorer alle verbleibenden Dateien aus den Xamarin Visual Studio-Erweiterungsordnern (alle Versionen einschließlich **Programmdateien** und **Programmdateien (x86)**):

    `C:\Program Files*\Microsoft Visual Studio 1*.0\Common7\IDE\Extensions\Xamarin`

3.  Überprüfen Sie auch in das Verzeichnis "VirtualStore", um festzustellen, ob möglicherweise gibt es keine "Overlay" Kopien einer Erweiterung Verzeichnisse:

    `%LOCALAPPDATA%\VirtualStore`

4.  Öffnen Sie den Registrierungs-Editor (Regedit).

5.  Suchen Sie nach diesem Schlüssel:

    _HKEY\_LOCAL\_MACHINE\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\SharedDlls_

6.  Suchen und löschen Sie alle Einträge, die diesem Muster entsprechen:

    _C:\Program Files\*\Microsoft Visual Studio 1\*.0\Common7\IDE\Extensions\Xamarin_

7.  Suchen Sie nach diesem Schlüssel:

    `HKEY\_CURRENT\_USER\Software\Microsoft\VisualStudio\1\*.0\ExtensionManager\PendingDeletions`

8.  Löschen Sie alle Einträge, die zu Xamarin gehören könnten. Beispielsweise verwendet hier ein, um Probleme in früheren Versionen von Xamarin zu verursachen:

    _Mono.VisualStudio.Shell,1.0_

9.  Starten Sie den Computer neu.

10.  Installieren Sie die aktuelle stabile Version von Xamarin von [visualstudio.com](https://visualstudio.com/xamarin).

## <a name="possible-fix-4-repair-visual-studio-installation"></a>Das Update 4 möglich: Reparieren Sie Visual Studio-installation

1.  Öffnen Sie die Windows **Programme und Funktionen** Systemsteuerung.

2.  Klicken Sie mit der rechten Maustaste auf den relevanten Microsoft Visual Studio-Eintrag, und wählen **ändern**

3.  Klicken Sie auf die **Reparatur** -Schaltfläche in der Visual Studio-Dialogfeld, das geöffnet wird.
