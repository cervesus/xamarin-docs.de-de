---
title: Fehlende Visual Studio-Erweiterungen nach der installation
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 066d36a3-e553-48d6-8769-c972274d7641
author: asb3993
ms.author: amburns
ms.date: 03/20/2017
ms.openlocfilehash: e47cfc4de77a6310a81867eefb07c3c1e5cc7060
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/09/2018
---
# <a name="missing-visual-studio-extensions-after-installation"></a>Fehlende Visual Studio-Erweiterungen nach der installation

## <a name="error-message-this-project-is-incompatible-with-the-current-edition-of-visual-studio"></a>Fehlermeldung: Dieses Projekt ist nicht kompatibel mit der aktuellen Edition von Visual Studio

Stellen Sie sicher, dass eine kompatible Version von Visual Studio installiert ist:

-   Visual Studio-2017 (Community, Professional oder Enterprise)
-   Visual Studio 2015 (Community, Professional oder Enterprise)

Siehe auch die [Windows Anforderungen](~/cross-platform/get-started/requirements.md#windows).

## <a name="possible-fix-1-change-the-installation-to-make-sure-the-visual-studio-extensions-are-installed"></a>Mögliche Lösung 1: Ändern Sie die Installation, um sicherzustellen, dass die Visual Studio-Erweiterungen installiert sind

In bestimmten Situationen tritt möglicherweise ein Xamarin-Installationsprogramm automatisch deaktivieren Sie die Installationsoptionen für die Visual Studio-Erweiterungen. Wenn dies die Ursache des Problems ist, installieren Sie die fehlenden Visual Studio-Erweiterungen, die mit dem Installer **Änderung** Befehl. Geben Sie beispielsweise Folgendes ein, um die Erweiterungen für Visual Studio 2013 zu installieren:

1. Öffnen Sie das Windows **Programme und Funktionen** in der Systemsteuerung.

2. Klicken Sie mit der rechten Maustaste auf die **Xamarin** Eintrag, und wählen Sie **Änderung**.

3. Klicken Sie auf **Weiter**, klicken Sie dann **Änderung**.

4. Stellen Sie sicher, dass die **Xamarin für Visual Studio 2013** Option wird festgelegt, zu installieren:

    ![](missing-vs-extensions-images/installer.png "Aktivieren Sie Xamarin für Visual Studio 2013-Installationsoption")

5. Fahren Sie mit dem Rest des Installations-Assistenten fort.

## <a name="possible-fix-2-ask-visual-studio-to-set-up-the-extensions-again"></a>Mögliche Lösung 2: Bitten Sie Visual Studio erneut die Erweiterungen einrichten

1. Überprüfen Sie, ob die Xamarin-Erweiterungen in Visual Studio-Erweiterungsordner kopiert wurden:

    `C:\Program Files (x86)\Microsoft Visual Studio 12.0\Common7\IDE\Extensions\Xamarin\Xamarin\3.1.228.0`

    Wenn die Erweiterungen (für Version 3.1.228) ordnungsgemäß installiert sind, wird im Ordner "" 60 Elemente sein:


    ![](missing-vs-extensions-images/folder.png "Liste der \"Xamarin\3.1.228.0\" Ordnerinhalt auflisten im Explorer")

2. Nachdem Sie sichergestellt haben, dass dieser Ordner richtig aussieht, teilen Sie Visual Studio zum Einrichten der Erweiterung erneut versuchen:

    `"C:\Program Files (x86)\Microsoft Visual Studio 12.0\Common7\IDE\devenv.exe" /setup`

## <a name="possible-fix-3-try-a-fresh-reinstall-of-xamarin"></a>Mögliche Lösung 3: Versuchen, eine neue Neuinstallation von Xamarin

1.  Deinstallieren Sie keines der folgenden, die vorhanden sind, in der Windows-Systemsteuerung:

    *   Xamarin

    *   Xamarin für Windows

    *   Xamarin.Android

    *   Xamarin.iOS

    *   Xamarin für Visual Studio

2.  Löschen Sie alle übrigen Dateien im Explorer von Xamarin für Visual Studio-Erweiterungsordner (alle Versionen einschließlich beide **Programmdateien** und **Programmdateien (x86)**):

    `C:\Program Files*\Microsoft Visual Studio 1*.0\Common7\IDE\Extensions\Xamarin`

3.  Überprüfen Sie auch in das Verzeichnis "VirtualStore", um festzustellen, ob möglicherweise keine "Overlay" Kopien eines beliebigen Erweiterung Verzeichnisse:

    `%LOCALAPPDATA%\VirtualStore`

4.  Öffnen Sie den Registrierungs-Editor (Regedit).

5.  Suchen Sie nach diesem Schlüssel:

    _HKEY\_LOCAL\_MACHINE\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\SharedDlls_

6.  Suchen und löschen Sie alle Einträge, die diesem Muster entsprechen:

    _C:\Program Files\*\Microsoft Visual Studio 1\*.0\Common7\IDE\Extensions\Xamarin_

7.  Suchen Sie nach diesem Schlüssel:

    `HKEY\_CURRENT\_USER\Software\Microsoft\VisualStudio\1\*.0\ExtensionManager\PendingDeletions`

8.  Löschen Sie alle Einträge, die zu Xamarin gehören könnten. Beispielsweise verwendet es folgt eine, die in früheren Versionen von Xamarin Probleme verursachen:

    _Mono.VisualStudio.Shell,1.0_

9.  Starten Sie den Computer neu.

10.  Installieren Sie die aktuelle stabile Version des Xamarin aus [visualstudio.com](https://visualstudio.com/xamarin).

## <a name="possible-fix-4-repair-visual-studio-installation"></a>Mögliche Update 4: Repair Visual Studio-installation

1.  Öffnen Sie das Windows **Programme und Funktionen** in der Systemsteuerung.

2.  Klicken Sie mit der mit der rechten Maustaste auf den relevanten Microsoft Visual Studio-Eintrag, und wählen Sie **ändern**

3.  Klicken Sie auf die **Reparatur** Schaltfläche in der Visual Studio-Dialogfeld, das geöffnet wird.
