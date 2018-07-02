---
title: Deinstallieren von Xamarin
description: In diesem Dokument wird beschrieben, wie Sie Xamarin unter Mac und Windows deinstallieren können. Es enthält spezifische Anweisungen zur Deinstallation von Mono, Xamarin.Android, Xamarin.iOS und anderen Tools.
ms.prod: xamarin
ms.assetid: b83a85ec-842a-444c-8f82-c2464eda099b
author: asb3993
ms.author: amburns
ms.date: 04/08/2017
ms.openlocfilehash: 444559672f25b13b7d3a769d6de4bd6384174965
ms.sourcegitcommit: d70fcc6380834127fdc58595aace55b7821f9098
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/19/2018
ms.locfileid: "36268887"
---
# <a name="uninstalling-xamarin"></a>Deinstallieren von Xamarin

Dieser Leitfaden erläutert, wie Sie Xamarin unter macOS oder aus Visual Studio unter Windows entfernen.

Wenn Xamarin mit dem Universal Installer erneut installiert werden muss, wird zuerst ein Neustart des Computers empfohlen.

## <a name="uninstalling-xamarin-on-macos"></a>Deinstallieren von Xamarin unter macOS

Mit diesem Handbuch können Sie jedes Produkt einzeln deinstallieren, indem Sie zum entsprechenden Abschnitt navigieren. Sie können das gesamte Xamarin-Toolset einschließlich der hier aufgeführten Produkte deinstallieren, indem Sie alle Schritte in diesem Leitfaden ausführen:

- [Mono](#uninstallmono)
- [Xamarin.Android](#uninstallandroid)
- [Xamarin.iOS](#uninstallios)
- [Xamarin.Mac](#uninstallmac)
- [Workbooks](#uninstallworkbooks)
- [Xamarin Profiler](#uninstallprofiler)
- [Installer](#uninstallinstaller)

> [!TIP]
> Wir haben ein [Deinstallationsskript](https://raw.githubusercontent.com/MicrosoftDocs/visualstudio-docs/master/mac/resources/uninstall-vsmac.sh) bereitgestellt, das Sie zum Entfernen von Xamarin von Ihrem macOS-Computer verwenden können. Weitere Informationen zur Verwendung des Skripts finden Sie im Abschnitt [Verwenden des Deinstallationsskripts](#uninstallscript) in diesem Leitfaden.

### <a name="uninstalling-visual-studio-for-mac"></a>Deinstallieren von Visual Studio für Mac

Der erste Schritt bei der Deinstallation von Visual Studio von einem Mac besteht darin, die Datei **Visual Studio.app** im Verzeichnis **/Anwendungen** zu suchen und in den **Papierkorb** zu legen. Alternativ können Sie mit der rechten Maustaste darauf klicken und wie hier dargestellt auf **In den Papierkorb legen** klicken:

![Visual Studio-Anwendung in den Papierkorb verschieben](uninstalling-xamarin-images/uninstall-image1.png)

Durch das Löschen dieses App Bundles wird Visual Studio für Mac entfernt, obwohl im Dateisystem möglicherweise noch andere Dateien im Zusammenhang mit Xamarin vorhanden sind.

Um alle Spuren von Visual Studio für Mac zu entfernen, führen Sie die folgenden Befehle im Terminal aus:

```bash
sudo rm -rf "/Applications/Visual Studio.app"
rm -rf ~/Library/Caches/VisualStudio
rm -rf ~/Library/Preferences/VisualStudio
rm -rf ~/Library/Preferences/Visual\ Studio
rm -rf ~/Library/Logs/VisualStudio
rm -rf ~/Library/VisualStudio
rm -rf ~/Library/Preferences/Xamarin/
rm -rf ~/Library/Developer/Xamarin
rm -rf ~/Library/Application\ Support/VisualStudio
rm -rf ~/Library/Application\ Support/VisualStudio/7.0/LocalInstall/Addins/
```

> [!NOTE]
> Informationen zum Deinstallieren von Visual Studio für Mac finden Sie in der Anleitung [Deinstallieren](https://docs.microsoft.com/visualstudio/mac/uninstall) auf docs.microsoft.com.

<a name="uninstallmono" />

### <a name="uninstall-mono-sdk-mdk"></a>Deinstallieren des Mono SDK (MDK)

Mono ist eine Open Source-Implementierung von .NET Framework und wird von allen Xamarin-Produkten (Xamarin.iOS, Xamarin.Android und Xamarin.Mac) verwendet, damit diese Plattformen in C# entwickelt werden können.

> [!WARNING]
> Außerhalb von Xamarin gibt es weitere Anwendungen wie Unity, die ebenfalls Mono verwenden. Achten Sie darauf, dass es keine weiteren Abhängigkeiten mit Mono bestehen, bevor Sie es deinstallieren.

Um das Mono-Framework zu entfernen, führen Sie die folgenden Befehle im Terminal aus:

```bash
sudo rm -rf /Library/Frameworks/Mono.framework
sudo pkgutil --forget com.xamarin.mono-MDK.pkg
sudo rm /etc/paths.d/mono-commands
```

<a name="uninstallandroid" />

### <a name="uninstall-xamarinandroid"></a>Deinstallieren von Xamarin.Android

Es gibt eine Reihe von Elementen, die für die Verwendung von Xamarin.Android erforderlich sind, wie z.B. das Android SDK und das Java SDK. Diese müssen beim Deinstallieren von Xamarin.Android entfernt werden. Dieser Abschnitt enthält alle Schritte zum Deinstallieren aller erforderlichen Bestandteile.

Um Xamarin.Android zu entfernen, führen Sie die folgenden Befehle im Terminal aus:

```bash
sudo rm -rf /Developer/MonoDroid
rm -rf ~/Library/MonoAndroid
sudo pkgutil --forget com.xamarin.android.pkg
sudo rm -rf /Library/Frameworks/Xamarin.Android.framework
```

#### <a name="uninstall-android-sdk-and-java-sdk"></a>Deinstallieren des Android SDK und des Java SDK

Das Android SDK ist für die Entwicklung von Android-Anwendungen erforderlich. Um alle Bestandteile des Android SDK vollständig zu entfernen, suchen Sie die Datei unter **~/Library/Developer/Xamarin/**, und verschieben Sie sie in den **Papierkorb**.

Das Java SDK (JDK) muss nicht deinstalliert werden, da es unter Mac OS X bereits vorab installiert ist.

#### <a name="uninstall-android-avd"></a>Deinstallieren von Android-AVDs

> [!WARNING]
> Es gibt weitere Anwendungen außerhalb von Visual Studio für Mac, die ebenfalls Android-AVDs und diese zusätzlichen Android-Komponenten verwenden, z.B. Android Studio.
> Das Entfernen dieses Verzeichnisses kann dazu führen, dass Projekte in Android Studio unterbrochen werden. 

Verwenden Sie zum Entfernen aller Android-AVDs und weiteren Android Komponenten den folgenden Befehl:

```bash
rm -rf ~/.android
```

Um nur die Android-AVDs zu entfernen, verwenden Sie den folgenden Befehl:

```bash
rm -rf ~/.android/avd
```

<a name="uninstallios" />

### <a name="uninstall-xamarinios"></a>Deinstallieren von Xamarin.iOS

Xamarin.iOS ermöglicht die Entwicklung von iOS-Anwendungen mit C# oder F#. Um Xamarin.iOS von einem Computer zu deinstallieren, führen Sie die folgenden Schritte aus:

Führen Sie die folgenden Befehle im Terminal aus, um alle Xamarin.iOS-Dateien zu entfernen:

```bash
rm -rf ~/Library/MonoTouch
sudo rm -rf /Library/Frameworks/Xamarin.iOS.framework
sudo rm -rf /Developer/MonoTouch
sudo pkgutil --forget com.xamarin.monotouch.pkg
sudo pkgutil --forget com.xamarin.xamarin-ios-build-host.pkg
sudo pkgutil --forget com.xamarin.xamarin.ios.pkg
```

<a name="uninstallmac" />

### <a name="uninstall-xamarinmac"></a>Deinstallieren von Xamarin.Mac

Sie können Xamarin.Mac und die zugehörige Lizenz mithilfe der folgenden beiden Befehle vom Computer entfernen:

```bash
sudo rm -rf /Library/Frameworks/Xamarin.Mac.framework
rm -rf ~/Library/Xamarin.Mac
```

<a name="uninstallworkbooks" />

### <a name="uninstall-workbooks"></a>Deinstallieren von Workbooks

Um Xamarin Workbooks Version 1.2.2 und höher zu entfernen, verwenden Sie die folgenden Befehle im Terminal:

```bash
sudo /Library/Frameworks/Xamarin.Interactive.framework/Versions/Current/uninstall
```

Informationen zu früheren Versionen finden Sie im Handbuch zur Deinstallation von [Workbooks](~/tools/workbooks/install.md#uninstall-macos).

<a name="uninstallprofiler" />

### <a name="uninstall-the-xamarin-profiler"></a>Deinstallieren von Xamarin Profiler

Um Xamarin Profiler zu entfernen, führen Sie die folgenden Befehle im Terminal aus:

```bash
sudo rm -rf "/Applications/Xamarin Profiler.app"
```

<a name="uninstallinstaller" />

### <a name="uninstall-the-xamarin-installer"></a>Deinstallieren des Xamarin-Installers

Um alle Spuren des Xamarin-Universal-Installers zu entfernen, verwenden Sie die folgenden Befehle:

```bash
rm -rf ~/Library/Caches/XamarinInstaller/
rm -rf ~/Library/Caches/VisualStudioInstaller/
rm -rf ~/Library/Logs/XamarinInstaller/
rm -rf ~/Library/Logs/VisualStudioInstaller/
rm -rf ~/Library/Preferences/Xamarin/
rm -rf "~/Library/Preferences/Visual Studio/"
```

<a name="uninstallscript" />

### <a name="using-the-uninstall-script"></a>Verwenden des Deinstallationsskripts

Mit diesem Deinstallationsskript können Sie Visual Studio für Mac und die zugehörigen Xamarin-Komponenten in einer Aktion deinstallieren.

Das Skript enthält die meisten Befehle, die Sie im Artikel finden. Es gibt zwei wichtigsten Auslassungen gegenüber dem Skript, die aufgrund von möglichen externen Abhängigkeiten nicht enthalten sind:

- Deinstallieren von Mono
- Deinstallieren von Android-AVDs

Führen Sie die folgenden Schritte durch, um das Skript auszuführen:

1. Klicken Sie mit der rechten Maustaste auf das Skript, und klicken Sie auf „Speichern unter...“, um die Datei auf Ihrem Mac zu speichern.

2.  Öffnen Sie das **Terminal**, und legen Sie als Arbeitsverzeichnis den Speicherort des heruntergeladenen Skripts fest:

        $ cd /location/of/file

3. Machen Sie das Skript ausführbar, und führen Sie es mit **sudo** aus:

        $ chmod +x ./xamarin_uninstall.sh
        $ sudo ./xamarin_uninstall.sh

4. Löschen Sie schließlich das Deinstallationsskript.

Xamarin sollte nun auf Ihrem Computer deinstalliert werden.

<a name="uninstallwindows" />

## <a name="uninstalling-xamarin-on-windows"></a>Deinstallieren von Xamarin unter Windows

Xamarin wurde in folgenden Produkten unterstützt:

- [Visual Studio 2017](#uninstallvs2017)
- [Visual Studio 2015](#uninstallvs2015)
- [Visual Studio 2013](#uninstallvs2015) [**nicht unterstützt**]
- [Xamarin Studio](#uninstallxamarinstudio) [**nicht unterstützt**]

<a name="uninstallvs2017" />

### <a name="visual-studio-2017"></a>Visual Studio 2017

Xamarin kann mithilfe der Installer-App von Visual Studio 2017 deinstalliert werden:

1. Verwenden Sie das **Startmenü**, um den **Visual Studio-Installer** zu öffnen.

2. Klicken Sie auf die Schaltfläche **Ändern**, um die gewünschte Instanz zu ändern.

    [![](uninstalling-xamarin-images/vs2017-02-sml.png "Klicken Sie auf die Schaltfläche „Ändern“")](uninstalling-xamarin-images/vs2017-02.png#lightbox)

3. Heben Sie auf der Registerkarte **Workloads** im Abschnitt **Mobil und Gaming** die Auswahl für **Mobile-Entwicklung mit .NET** auf.

    [![](uninstalling-xamarin-images/vs2017-03-sml.png "Aufheben der Auswahl der Workload „Mobile Entwicklung“")](uninstalling-xamarin-images/vs2017-03.png#lightbox)

4. Klicken Sie unten rechts im Fenster auf die Schaltfläche **Ändern**.

5. Der Installer entfernt nun die Komponenten, für die die Auswahl aufgehoben wurde. Beachten Sie,dass Visual Studio 2017 geschlossen werden muss, bevor der Installer Änderungen vornehmen kann.

    [![](uninstalling-xamarin-images/vs2017-04-sml.png "Klicken Sie auf die Schaltfläche „Ändern“")](uninstalling-xamarin-images/vs2017-04.png#lightbox)

Sie können einzelne Xamarin-Komponenten wie den Profiler oder Workbooks deinstallieren, indem Sie zur Registerkarte **Einzelne Komponenten** aus Schritt 3 wechseln und die Auswahl für bestimmte Komponenten aufheben:

[![](uninstalling-xamarin-images/vs2017-components-sml.png "Deinstallieren einzelner Komponenten")](uninstalling-xamarin-images/vs2017-components.png#lightbox)

Wählen Sie zur vollständigen Deinstallation von Visual Studio 2017 die Option **Deinstallieren** aus dem Hamburgermenü neben der Schaltfläche **Starten**.

[![](uninstalling-xamarin-images/vs2017-uninstall-sml.png "Vollständige Deinstallation von Visual Studio")](uninstalling-xamarin-images/vs2017-uninstall.png#lightbox)

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


## <a name="uninstalling-older-and-unsupported-products"></a>Deinstallieren älterer und nicht unterstützter Produkte

<a name="uninstallvs2015"></a>

### <a name="visual-studio-2015-and-earlier"></a>Visual Studio 2015 und frühere Versionen

Um Visual Studio 2015 vollständig zu deinstallieren, verwenden Sie [die Supportantwort auf visualstudio.com](https://www.visualstudio.com/vs/support/vs2015/uninstall-visual-studio-2015/).

Sie können Xamarin auf einem Windows-Computer über die **Systemsteuerung** deinstallieren. Navigieren Sie zu **Programme und Funktionen** oder **Programme > Programm deinstallieren** wie unten gezeigt:

 [![](uninstalling-xamarin-images/image3.png "Navigieren Sie zu „Programme und Funktionen“ oder „Programme > Programm deinstallieren“, wie hier gezeigt")](uninstalling-xamarin-images/image3.png#lightbox). 

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

<a name="uninstallxamarinstudio"></a>

### <a name="uninstall-xamarin-studio-on-windows"></a>Deinstallieren von Xamarin Studio unter Windows

Sie können Xamarin Studio auf einem Windows-Computer über die **Systemsteuerung** deinstallieren. Navigieren Sie zu **Programme und Funktionen** oder **Programme > Programm deinstallieren**. 

Suchen Sie zur Deinstallation von Xamarin Studio in der Liste der Programme nach **Xamarin Studio 5.x.x**, und klicken Sie anschließend auf die Schaltfläche **Deinstallieren**. 

### <a name="uninstall-xamarin-studio-on-mac"></a>Deinstallieren von Xamarin Studio unter Mac

Der erste Schritt bei der Deinstallation von Xamarin Studio unter Mac besteht darin, die Datei **Xamarin Studio.app** im Verzeichnis **/Anwendungen** zu suchen und in den **Papierkorb** zu ziehen. Alternativ können Sie mit der rechten Maustaste darauf klicken und **In den Papierkorb legen** wie unten gezeigt auswählen:

 [![](uninstalling-xamarin-images/image1.png "Alternativ können Sie mit der rechten Maustaste darauf klicken und „In den Papierkorb legen“ auswählen, wie hier gezeigt wird")](uninstalling-xamarin-images/image1.png#lightbox)

Durch das Löschen dieses App Bundles wird Xamarin Studio entfernt. Im Dateisystem gibt es jedoch möglicherweise noch andere Dateien im Zusammenhang mit Xamarin.

Um alle Spuren von Xamarin Studio zu entfernen, sollten Sie die folgenden Befehle im Terminal ausführen:

```bash
sudo rm -rf "/Applications/Xamarin Studio.app"
rm -rf ~/Library/Caches/XamarinStudio-*
rm -rf ~/Library/Preferences/XamarinStudio-*
rm -rf ~/Library/Logs/XamarinStudio-*
rm -rf ~/Library/XamarinStudio-*
```

## <a name="summary"></a>Zusammenfassung

Dieser Artikel enthält Anweisungen zur vollständigen Deinstallation von Xamarin von einem Mac mithilfe von Terminal-Befehlen. Er bietet auch Anweisungen zum Deinstallieren von Xamarin von einem Windows-Computer über die Option **Programme und Funktionen** (für Visual Studio 2015 und früher) und zum Verwenden des **Visual Studio-Installers** für Visual Studio 2017.


## <a name="related-links"></a>Verwandte Links

- [Deinstallieren des Skripts (Beispiel)](https://raw.githubusercontent.com/MicrosoftDocs/visualstudio-docs/master/mac/resources/uninstall-vsmac.sh)
