---
title: 'title: "Deinstallieren von Xamarin" description: "In diesem Dokument wird beschrieben, wie Sie unter Windows Xamarin in Visual Studio deinstallieren."'
description: 'ms.prod: xamarin ms.assetid: b83a85ec-842a-444c-8f82-c2464eda099b author: conceptdev ms.author: crdun ms.date: 01/22/2020 no-loc: [Xamarin.Forms, Xamarin.Essentials]'
ms.prod: xamarin
ms.assetid: b83a85ec-842a-444c-8f82-c2464eda099b
author: conceptdev
ms.author: crdun
ms.date: 01/22/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: b4d644591ab85f185709e7bd53353580a6578d77
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2020
ms.locfileid: "84570109"
---
# <a name="uninstall-xamarin-from-visual-studio"></a>Deinstallieren von Xamarin in Visual Studio

In diesem Leitfaden wird erläutert, wie Sie Xamarin unter Windows aus Visual Studio entfernen können.

<a name="uninstallvs2017"></a>

## <a name="visual-studio-2019-and-visual-studio-2017"></a>Visual Studio 2019 und Visual Studio 2017

Xamarin kann mithilfe der Installer-App in Visual Studio 2017 und 2019 deinstalliert werden:

1. Verwenden Sie das **Startmenü**, um den **Visual Studio-Installer** zu öffnen.

2. Klicken Sie auf die Schaltfläche **Ändern**, um die gewünschte Instanz zu ändern.

    [![](uninstalling-xamarin-images/vs2017-02-sml.png "Press the modify button")](uninstalling-xamarin-images/vs2017-02.png#lightbox)

3. Heben Sie auf der Registerkarte **Workloads** im Abschnitt **Mobil und Gaming** die Auswahl für **Mobile-Entwicklung mit .NET** auf.

    [![](uninstalling-xamarin-images/vs2017-03-sml.png "Uncheck the Mobile Development workload")](uninstalling-xamarin-images/vs2017-03.png#lightbox)

4. Klicken Sie unten rechts im Fenster auf die Schaltfläche **Ändern**.

5. Der Installer entfernt nun die Komponenten, für die die Auswahl aufgehoben wurde. Beachten Sie,dass Visual Studio 2017 geschlossen werden muss, bevor der Installer Änderungen vornehmen kann.

    [![](uninstalling-xamarin-images/vs2017-04-sml.png "Press the Modify button")](uninstalling-xamarin-images/vs2017-04.png#lightbox)

Sie können einzelne Xamarin-Komponenten wie den Profiler oder Workbooks deinstallieren, indem Sie zur Registerkarte **Einzelne Komponenten** aus Schritt 3 wechseln und die Auswahl für bestimmte Komponenten aufheben:

[![](uninstalling-xamarin-images/vs2017-components-sml.png "Uninstall individual components")](uninstalling-xamarin-images/vs2017-components.png#lightbox)

Wählen Sie zur vollständigen Deinstallation von Visual Studio 2017 die Option **Deinstallieren** aus dem Hamburgermenü neben der Schaltfläche **Starten**.

[![](uninstalling-xamarin-images/vs2017-uninstall-sml.png "Uninstall Visual Studio completely")](uninstalling-xamarin-images/vs2017-uninstall.png#lightbox)

> [!IMPORTANT]
> Wenn mindestens zwei Instanzen von Visual Studio – z.B. eine Release- und eine Vorschauversion – parallel installiert sind (SxS), kann das Deinstallieren einer Instanz dazu führen, dass einige Xamarin-Funktionen aus anderen Visual Studio-Instanzen entfernt werden. Zu diesen Funktionen zählen:
>
> - Xamarin Profiler
> - Xamarin Workbooks/Inspector
> - Xamarin Remote iOS Simulator
> - Apple Bonjour SDK
>
> Unter bestimmten Umständen kann das Deinstallieren einer SxS-Instanz dazu führen, dass diese Funktionen nicht korrekt entfernt werden. Dadurch kann die Leistung der Xamarin-Plattform in den Visual Studio-Instanzen beeinträchtigt werden, die nach der Deinstallation der SxS-Instanz im System verbleiben.
>
>Sie können dieses Problem durch Ausführen der Option **Reparatur** im Visual Studio-Installer beheben. Hierbei werden die fehlenden Komponenten neu installiert.

<a name="uninstallvs2015"></a>

## <a name="visual-studio-2015-and-earlier"></a>Visual Studio 2015 und frühere Versionen

Um Visual Studio 2015 vollständig zu deinstallieren, verwenden Sie [die Supportantwort auf visualstudio.com](https://visualstudio.microsoft.com/vs/support/vs2015/uninstall-visual-studio-2015/).

Sie können Xamarin auf einem Windows-Computer über die **Systemsteuerung** deinstallieren. Navigieren Sie zu **Programme und Funktionen** oder **Programme > Programm deinstallieren** wie unten gezeigt:

 [![](uninstalling-xamarin-images/image3.png "Navigate to Programs and Features or Programs  Uninstall a Program as illustrated here")](uninstalling-xamarin-images/image3.png#lightbox)

Deinstallieren Sie in der Systemsteuerung jedes der folgenden Elemente, sofern es vorhanden ist:

- Xamarin
- Xamarin für Windows
- Xamarin.Android
- Xamarin.iOS
- Xamarin für Visual Studio

Löschen Sie im Explorer alle verbleibenden Dateien aus den Xamarin Visual Studio-Erweiterungsordnern. Löschen Sie alle Versionen sowohl aus „Programme“ als auch aus „Programme (x86)“:

```
C:\Program Files*\Microsoft Visual Studio 1*.0\Common7\IDE\Extensions\Xamarin
```

Löschen Sie das Visual Studio-Verzeichnis für den MEF-Komponentencache, das sich in folgendem Speicherort befinden sollte:

```
%LOCALAPPDATA%\Microsoft\VisualStudio\1*.0\ComponentModelCache
```

Überprüfen Sie im **VirtualStore**-Verzeichnis, ob Windows dort Überlagerungsdateien für die Verzeichnisse **Erweiterungen\Xamarin** oder **ComponentModelCache** gespeichert hat:

```
%LOCALAPPDATA%\VirtualStore
```

Öffnen Sie den Registrierungs-Editor (regedit), und suchen Sie nach folgendem Schlüssel:

```
HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\SharedDlls
```

Suchen und löschen Sie alle Einträge, die diesem Muster entsprechen:

```
C:\Program Files*\Microsoft Visual Studio 1*.0\Common7\IDE\Extensions\Xamarin
```

Suchen Sie nach diesem Schlüssel:

```
HKEY_CURRENT_USER\Software\Microsoft\VisualStudio\1*.0\ExtensionManager\PendingDeletions
```

Löschen Sie alle Einträge, die zu Xamarin gehören könnten. Hierzu gehören z.B. alle Einträge mit den Begriffen `mono` oder `xamarin`.

Öffnen Sie eine Eingabeaufforderung mit Administratorrechten (cmd.exe), und führen Sie die Befehle `devenv /setup` und `devenv /updateconfiguration` für jede installierte Version von Visual Studio aus. Beispiel für Visual Studio 2015:

```cmd
"%ProgramFiles(x86)%\Microsoft Visual Studio 14.0\Common7\IDE\devenv.exe" /setup
"%ProgramFiles(x86)%\Microsoft Visual Studio 14.0\Common7\IDE\devenv.exe" /updateconfiguration
```
