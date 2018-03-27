---
title: Deinstallieren von Xamarin
description: Deinstallieren von Xamarin-Produkten auf einem Computer
ms.topic: article
ms.prod: xamarin
ms.assetid: b83a85ec-842a-444c-8f82-c2464eda099b
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 04/08/2017
ms.openlocfilehash: 2c2ba84167924916c3bec27379d33c47e8dab360
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/22/2018
---
# <a name="uninstalling-xamarin"></a>Deinstallieren von Xamarin

> [!IMPORTANT]
> In diesem Artikel wird erläutert, wie Xamarin Studio oder andere Xamarin-Produkte auf einem Mac- oder Windows-Computer deinstalliert werden. Informationen zum Deinstallieren von Visual Studio für Mac finden Sie in der Anleitung [Deinstallieren](https://docs.microsoft.com/visualstudio/mac/uninstall) auf docs.microsoft.com.

Es gibt eine Reihe von Xamarin-Produkten, die eine plattformübergreifende Anwendungsentwicklung ermöglichen. Hierzu gehören sowohl eigenständige Apps wie Xamarin Studio als auch Erweiterungen für andere Apps wie die Xamarin-Erweiterung für Visual Studio.

In dieser Anleitung wird erklärt, wie Sie Xamarin-Funktionen unter macOS oder aus Visual Studio unter Windows entfernen:

1.  [Deinstallieren von Xamarin Studio](#uninstallxamarinstudio)
  1.  [Deinstallation von Mono](#uninstallmono)
  1.  [Deinstallieren von Xamarin.Android](#uninstallandroid)
  1.  [Deinstallieren von Xamarin.iOS](#uninstallios)
  1.  [Deinstallieren von Xamarin.Mac](#uninstallmac)
  2.  [Deinstallieren von Inspector und Workbooks](#uninstallworkbooks)
1.  [Deinstallieren von Xamarin unter Windows](#uninstallwindows)
  1.  [Deinstallieren von Xamarin für Visual Studio 2015 und frühere Versionen](#uninstallvs2015)
  1.  [Deinstallieren von Xamarin für Visual Studio 2017](#uninstallvs2017)
1.  [Deinstallieren von Visual Studio für Mac](#uninstallvsmac)

Wenn Xamarin mit dem Universal Installer erneut installiert werden muss, wird zuerst ein Neustart des Computers empfohlen.

## <a name="uninstalling-xamarin-on-mac"></a>Deinstallieren von Xamarin unter Mac

Mit diesem Handbuch können Sie jedes Produkt einzeln deinstallieren, indem Sie zum entsprechenden Abschnitt navigieren. Wenn Sie die Anleitung bis zum Ende lesen und die notwendigen Schritten ausführen, können Sie das gesamte Xamarin-Toolset deinstallieren.

Hinweise zur Verwendung des [Deinstallationsskripts](http://download.xamarin.com/developer/cross-platform/xamarin_uninstall.sh) finden Sie im Abschnitt [Verwenden des Deinstallationsskripts](#uninstallscript) weiter unten in dieser Anleitung.

<a name="uninstallxamarinstudio" />

### <a name="uninstall-xamarin-studio"></a>Deinstallieren von Xamarin Studio

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

<a name="uninstallmono" />

### <a name="uninstall-mono-sdk-mdk"></a>Deinstallieren des Mono SDK (MDK)

Mono ist eine Open-Source-Implementierung von Microsofts .NET Framework und wird von allen Xamarin-Produkten (Xamarin.iOS, Xamarin.Android und Xamarin.Mac) verwendet, damit diese Plattformen in C# entwickelt werden können.

> [!IMPORTANT]
> Außerhalb von Xamarin gibt es weitere Anwendungen wie Unity, die ebenfalls Mono verwenden. Achten Sie darauf, dass es keine weiteren Abhängigkeiten mit Mono bestehen, bevor Sie es deinstallieren.

Um das Mono-Framework von einem Computer zu entfernen, führen Sie die folgenden Befehle im Terminal aus:

```bash
sudo rm -rf /Library/Frameworks/Mono.framework
sudo pkgutil --forget com.xamarin.mono-MDK.pkg
sudo rm /etc/paths.d/mono-commands
```

<a name="uninstallandroid" />

### <a name="uninstall-xamarinandroid"></a>Deinstallieren von Xamarin.Android

Es gibt eine Anzahl von Elementen, die für die Installation und Verwendung von Xamarin.Android erforderlich sind, z.B. das Android SDK und das Java SDK. Weitere Informationen zu erforderlichen Komponenten finden Sie in der Anleitung [Manuelle Installation](https://docs.microsoft.com/visualstudio/mac/installation/).

Verwenden Sie die folgenden Befehle, um Xamarin.Android zu entfernen:

```bash
sudo rm -rf /Developer/MonoDroid
rm -rf ~/Library/MonoAndroid
sudo pkgutil --forget com.xamarin.android.pkg
sudo rm -rf /Library/Frameworks/Xamarin.Android.framework
```

#### <a name="uninstall-android-sdk-and-java-sdk"></a>Deinstallieren des Android SDK und des Java SDK

Das Android SDK ist für die Entwicklung von Android-Anwendungen erforderlich. Um alle Bestandteile des Android SDKs vollständig zu entfernen, suchen Sie die Datei unter **~/Library/Developer/Xamarin/**, und verschieben Sie sie wie unten gezeigt in den **Papierkorb**:

 [![](uninstalling-xamarin-images/image2.png "Zum vollständigen Entfernen aller Bestandteile des Android SDK , suchen Sie die Datei, und verschieben Sie sie wie hier gezeigt in den Papierkorb")](uninstalling-xamarin-images/image2.png#lightbox)

Das Java SDK (JDK) muss nicht deinstalliert werden, da es unter Mac OS X bereits vorab installiert ist.

<a name="uninstallios" />

### <a name="uninstall-xamarinios"></a>Deinstallieren von Xamarin.iOS

Xamarin.iOS ermöglicht die Entwicklung von iOS-Anwendungen mit C# oder F# für Xamarin Studio auf einem Mac.
Der Xamarin-Buildhost wurde auch in früheren Versionen von Xamarin.iOS automatisch installiert, um die iOS-Entwicklung in Visual Studio zu ermöglichen. Um beides auf einem Computer zu deinstallieren, führen Sie die folgenden Schritte aus:

Führen Sie die folgenden Befehle im Terminal aus, um alle Xamarin.iOS-Dateien aus einem Dateisystem zu entfernen:

```bash
rm -rf ~/Library/MonoTouch
sudo rm -rf /Library/Frameworks/Xamarin.iOS.framework
sudo rm -rf /Developer/MonoTouch
sudo pkgutil --forget com.xamarin.monotouch.pkg
sudo pkgutil --forget com.xamarin.xamarin-ios-build-host.pkg
```

#### <a name="uninstall-the-mac-build-host"></a>Deinstallieren des Mac-Buildhosts

Hinweis: Der Mac-Buildhost wurde möglicherweise bereits entfernt, wenn Sie ein Update auf Xamarin 4 durchgeführt haben. Führen Sie den folgenden Befehl im Terminal aus, um die Buildhost-Anwendung zu entfernen:

```bash
sudo rm -rf "/Applications/Xamarin.iOS Build Host.app"
```

Der Buildhostprozess oder `launchd`-Auftrag wird möglicherweise immer noch ausgeführt oder lauscht unter Umständen an bestimmten Ports.
Sie können den jeweiligen Status durch Ausführen von `launchctl list | grep com.xamarin.mtvs.buildserver` im Terminal überprüfen.

```bash
sudo launchctl unload /Library/LaunchAgents/com.xamarin.mtvs.buildserver.plist
sudo rm -f /Library/LaunchAgents/com.xamarin.mtvs.buildserver.plist
```

<a name="uninstallmac" />

### <a name="uninstall-xamarinmac"></a>Deinstallieren von Xamarin.Mac

Nachdem Sie Xamarin Studio erfolgreich deinstalliert haben, können Sie Xamarin.Mac und die zugehörige Lizenz mithilfe der folgenden beiden Befehle vom Computer entfernen:

```bash
sudo rm -rf /Library/Frameworks/Xamarin.Mac.framework
rm -rf ~/Library/Xamarin.Mac
```

<a name="uninstallworkbooks" />

### <a name="uninstall-workbooks-and-inspector"></a>Deinstallieren von Workbooks und Inspector

Durch den folgenden Bash-Befehl werden Xamarin Inspector und Workbooks (Version 1.2.2 und höher) entfernt:

```bash
sudo /Library/Frameworks/Xamarin.Interactive.framework/Versions/Current/uninstall
```

Informationen zu früheren Versionen finden Sie im Handbuch zur Deinstallation von [Workbooks](~/tools/workbooks/install.md#uninstall-macos).

### <a name="uninstall-the-xamarin-installer"></a>Deinstallieren des Xamarin-Installers

Verwenden Sie die folgenden Befehle, um alle Spuren des Xamarin-Universal-Installer zu entfernen:

```bash
rm -rf ~/Library/Caches/XamarinInstaller/
rm -rf ~/Library/Logs/XamarinInstaller/
rm -rf ~/Library/Preferences/Xamarin/
```

<a name="uninstallscript" />

### <a name="using-the-uninstall-script"></a>Verwenden des Deinstallationsskripts

Durch die Ausführung des [Deinstallationsskripts](http://download.xamarin.com/developer/cross-platform/xamarin_uninstall.sh) wird Xamarin vom Computer entfernt. Gehen Sie zur Verwendung des Deinstallationsskripts wie folgt vor:

1.  Laden Sie das Deinstallationsskript herunter, und notieren Sie sich den Speicherort der heruntergeladenen Datei. Standardmäßig ist für diesen das Verzeichnis **/Downloads** festgelegt.
1.  Öffnen Sie das **Terminal**, und legen Sie als Arbeitsverzeichnis den Speicherort des heruntergeladenen Skripts fest:

        $ cd /location/of/file

1. Machen Sie das Skript ausführbar, und führen Sie es mit **sudo** aus:

        $ chmod +x ./xamarin_uninstall.sh
        $ sudo ./xamarin_uninstall.sh

1. Löschen Sie schließlich das Deinstallationsskript.

Xamarin sollte nun auf Ihrem Computer deinstalliert werden.

<a name="uninstallwindows" />

## <a name="uninstalling-xamarin-on-windows"></a>Deinstallieren von Xamarin unter Windows

<a name="uninstallvs2015" />

### <a name="visual-studio-2015-and-earlier"></a>Visual Studio 2015 und frühere Versionen

Sie können Xamarin auf einem Windows-Computer über die **Systemsteuerung** deinstallieren. Navigieren Sie zu **Programme und Funktionen** oder **Programme > Programm deinstallieren** wie unten gezeigt:

 [![](uninstalling-xamarin-images/image3.png "Navigieren Sie zu „Programme und Funktionen“ > „Programm deinstallieren“, wie hier gezeigt wird")](uninstalling-xamarin-images/image3.png#lightbox) [![](uninstalling-xamarin-images/image3.png "Navigate to Programs and Features or Programs Uninstall a Program as illustrated here")](uninstalling-xamarin-images/image3.png#lightbox)

Suchen Sie zur Deinstallation von Xamarin Studio in der Liste der Programme nach **Xamarin Studio 5.x.x**, und klicken Sie anschließend auf die Schaltfläche **Deinstallieren**. Um die Xamarin-Erweiterung für Visual Studio und die SDKs zu entfernen, suchen Sie in der Liste der Programme nach **Xamarin**, und klicken Sie anschließend auf **Deinstallieren**. Die entsprechenden Dateien sind im folgenden Screenshot zu sehen:

 [![](uninstalling-xamarin-images/image4a.png "Die entsprechenden Dateien sind in diesem Screenshot zu sehen")](uninstalling-xamarin-images/image4a.png#lightbox)

Die folgenden Programme können zur vollständigen Deinstallation aller Xamarin-Komponenten ebenfalls entfernt werden:

-  Android-SDK


  [![](uninstalling-xamarin-images/image5.png "Diese Programme können ebenfalls zur vollständigen Deinstallation aller Xamarin-Komponenten entfernt werden")](uninstalling-xamarin-images/image5.png#lightbox)
-  GTK#


  [![](uninstalling-xamarin-images/image6.png "Diese Programme können ebenfalls zur vollständigen Deinstallation aller Xamarin-Komponenten entfernt werden")](uninstalling-xamarin-images/image6.png#lightbox)
-  Xamarin Universal Installer


 [![](uninstalling-xamarin-images/image7.png "Diese Programme können ebenfalls zur vollständigen Deinstallation aller Xamarin-Komponenten entfernt werden")](uninstalling-xamarin-images/image7.png#lightbox)
-  Java SDK (Beachten Sie beim Entfernen, dass weitere Abhängigkeiten von diesem SDK vorhanden sein können)


 [![](uninstalling-xamarin-images/image8.png "Beachten Sie beim Entfernen des Java SDK, dass weitere Abhängigkeiten von diesem vorhanden sein können")](uninstalling-xamarin-images/image8.png#lightbox)

Befolgen Sie zur vollständigen Deinstallation von Visual Studio die [Anweisungen von Microsoft](https://msdn.microsoft.com/library/mt720585.aspx).


<a name="uninstallvs2017" />

## <a name="visual-studio-2017"></a>Visual Studio 2017

Die Xamarin-Erweiterung für Visual Studio 2017 kann über die Installer-App deinstalliert werden:

1. Verwenden Sie das **Startmenü**, um den **Visual Studio-Installer** zu öffnen.

  [![](uninstalling-xamarin-images/vs2017-01-sml.png "Starten Sie den Visual Studio-Installer")](uninstalling-xamarin-images/vs2017-01.png#lightbox)

1. Klicken Sie auf die Schaltfläche **Ändern**, um die gewünschte Instanz zu ändern.

  [![](uninstalling-xamarin-images/vs2017-02-sml.png "Klicken Sie auf die Schaltfläche „Ändern“")](uninstalling-xamarin-images/vs2017-02.png#lightbox)

1. Heben Sie auf der Registerkarte **Workloads** im Abschnitt **Mobil und Gaming** die Auswahl für **Mobile-Entwicklung mit .NET** auf.

  [![](uninstalling-xamarin-images/vs2017-03-sml.png "Aufheben der Auswahl der Workload „Mobile Entwicklung“")](uninstalling-xamarin-images/vs2017-03.png#lightbox)

1. Klicken Sie unten rechts im Fenster auf die Schaltfläche **Ändern**.
1. Der Installer entfernt nun die Komponenten, für die die Auswahl aufgehoben wurde. Beachten Sie,dass Visual Studio 2017 geschlossen werden muss, bevor der Installer Änderungen vornehmen kann.

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

<a name="uninstallvsmac" />

## <a name="uninstalling-visual-studio-for-mac"></a>Deinstallieren von Visual Studio für Mac

Wenn Sie Visual Studio für Mac deinstallieren, jedoch weiterhin Xamarin Studio verwenden möchten, suchen Sie die Datei **Visual Studio.app** im Verzeichnis **/Anwendungen**, und ziehen Sie sie in den Papierkorb. Alternativ können Sie mit der rechten Maustaste darauf klicken und **In den Papierkorb legen** wie unten gezeigt auswählen:

 [![](uninstalling-xamarin-images/image9.png "Klicken Sie mit der rechten Maustaste auf das Visual Studio-Symbol, und wählen Sie „In den Papierkorb legen“")](uninstalling-xamarin-images/image9.png#lightbox)

Löschen Sie zur vollständigen Deinstallation von Xamarin auf Ihrem Computer zunächst Visual Studio für Mac, und führen Sie anschließend die Schritte im Abschnitt [Deinstallieren von Xamarin Studio](#uninstallxamarinstudio) aus.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde erläutert, wie Sie Xamarin auf einem Mac mithilfe von Terminalbefehlen vollständig deinstallieren. Außerdem wurde beschrieben, wie Sie Xamarin auf einem Windows-Computer mit der Option **Programme und Funktionen** für Visual Studio 2015 und frühere Versionen sowie mit dem **Visual Studio-Installer** für Visual Studio 2017 deinstallieren.


## <a name="related-links"></a>Verwandte Links

- [Deinstallieren des Skripts (Beispiel)](http://download.xamarin.com/developer/cross-platform/xamarin_uninstall.sh)
