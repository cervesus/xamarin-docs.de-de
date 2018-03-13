---
title: Installieren von Xamarin.iOS unter Windows
description: "In diesem Artikel wird das Einrichten von Xamarin.iOS für Visual Studio veranschaulicht. Er umfasst den Installationsprozess für die Xamarin-Erweiterung für Visual Studio und erläutert das Herstellen einer Verbindung mit dem Apple SDK auf dem Mac."
ms.topic: article
ms.prod: xamarin
ms.assetid: abf85d3e-a365-44a2-b1a4-6c572c7f76dd
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/29/2017
ms.openlocfilehash: cfbe2df23317ee3ad11c9970ab892ddcc251b9d6
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="installing-xamarinios-on-windows"></a>Installieren von Xamarin.iOS unter Windows

_In diesem Artikel wird das Einrichten von Xamarin.iOS für Visual Studio veranschaulicht. Er umfasst den Installationsprozess für die Xamarin-Erweiterung für Visual Studio und erläutert das Herstellen einer Verbindung mit dem Apple SDK auf dem Mac._

## <a name="overview"></a>Übersicht

Mit Xamarin.iOS für Visual Studio können iOS-Anwendungen auf Windows-Computern geschrieben und getestet werden, wobei ein Mac, der sich in einem Netzwerk befindet, die Build- und Bereitstellungsdienste bietet.

Wenn Sie in Visual Studio für iOS entwickeln, hat dies mehrere Vorteile:

- Erstellen Sie plattformübergreifende Lösungen für iOS-, Android- und Windows-Anwendungen.
- Verwenden Sie Visual Studio-Tools (z.B. *ReSharper* und *Team Foundation Server*) für all Ihre plattformübergreifenden Projekte, einschließlich iOS-Quellcode.
- Arbeiten Sie mit einer bekannten IDE, und profitieren Sie gleichzeitig von Xamarin.iOS-Bindungen aller APIs von Apple.

Xamarin.iOS für Visual Studio unterstützt Konfigurationen, bei denen Visual Studio auf einem virtuellen Windows-Computer ausgeführt wird, auf einem Mac (mithilfe von Parallels oder VMWare), oder wenn es sich auf einem separaten Computer befindet, der im selben Netzwerk sichtbar ist wie ein Mac. Unabhängig davon, welche Konfiguration für Sie am besten geeignet ist, wird sich Visual Studio mithilfe von SSH schnell und sicher mit dem Mac verbinden.

Dieser Artikel umfasst die Schritte zum Installieren und Konfigurieren der Xamarin.iOS-Tools auf dem Mac- und dem Windows-Computer, als auch die Schritte zum Verbinden mit den Mac-Hosts, damit Entwickler Xamarin.iOS Anwendungen mithilfe von Visual Studio erstellen, debuggen und bereitstellen können.

Das folgende Diagramm zeigt eine einfache Übersicht über den Xamarin.iOS-Entwicklungsworkflow:

[![Der Xamarin.iOS-Entwicklungsworkflow](images/xma2.png)](images/xma2.png#lightbox)

> [!IMPORTANT]
>  Visual Studio startet einen separaten MSBuild-Prozess zum Erstellen der Projekte. Dieser Prozess stellt eine neue Verbindung mit dem Mac her. Dadurch bestehen tatsächlich zwei SSH-Verbindungen zwischen Windows und Mac während des Build-Prozess von Visual Studio. Bei der Verbindungsherstellung über die [Befehlszeile](~/ios/get-started/installation/windows/connecting-to-mac/index.md) wird nur der eine MSBuild-Prozess erstellt. Zur Vereinfachung des Diagramms werden die gesamten Verbindungen durch einen Pfeil dargestellt.

## <a name="requirements"></a>Anforderungen

Mit Xamarin.iOS für Visual Studio können Entwickler Erstaunliches vollbringen: Sie erstellen und debuggen iOS-Anwendungen auf einem Windows-Computer mithilfe der Visual Studio-IDE. Aber Xamarin.iOS für Visual Studio kann das nicht alleine vollbringen: iOS-Anwendungen können nicht ohne den Compiler von Apple erstellt, und sie können auch nicht ohne die Zertifikate und Tools zur Codesignierung von Apple bereitgestellt werden. Daher wird für eine Installation von Xamarin.iOS für Visual Studio eine Verbindung mit einem Mac OS X-Computer im Netzwerk benötigt, der diese Tasks ausführt. Sobald die Xamarin-Tools konfiguriert sind, wird der Prozess so nahtlos wie möglich ausgeführt.


<a name="system-requirements"/>

### <a name="system-requirements"></a>Systemanforderungen

Die Systemanforderungen sind:

### <a name="windows"></a>Windows

1. Windows 7 oder höher.

2. Visual Studio 2015 Professional oder höher.

    a. Wenn Sie über eine Enterprise-Lizenz verfügen, müssen Sie Visual Studio Enterprise installieren.

3. Xamarin für Visual Studio.

Die Xamarin-Tools können nicht mit Express-Editionen von Visual Studio verwendet werden, da Plug-Ins nicht unterstützt werden. Xamarin wird in Visual Studio-Community unterstützt.

### <a name="mac"></a>Mac

1. Ein Mac mit macOS Sierra (10.12) oder höher (es wird jedoch die neueste stabile Version empfohlen).

2. Xamarin iOS SDK. Dies wird installiert, wenn Visual Studio für Mac heruntergeladen wird.

3. Xcode-IDE von Apple und iOS SDK (es wird die neueste stabile Version aus dem Mac App Store empfohlen).

**Der Windows-Computer muss den Mac über das Netzwerk erreichen können.**

### <a name="apple-developer-account"></a>Apple-Entwicklerkonto

Zum Bereitstellen von Anwendungen auf einem Gerät oder um sie an den App Store zu übermitteln, ist ein Apple-Entwicklerkonto erforderlich. Die relevanten Entwicklerzertifikate und Bereitstellungsprofile müssen auf dem Mac im Netzwerk erstellt und installiert werden, bevor Xamarin.iOS für Visual Studio arbeiten kann. Im Artikel zu [Device Provisioning (Gerätebereitstellung)](~/ios/get-started/installation/device-provisioning/index.md) finden Sie Schritte zum Abrufen eines Zertifikats für die Entwicklung und zum Bereitstellen eines Geräts.

## <a name="features"></a>Features 

Xamarin.iOS für Visual Studio ermöglicht das Erstellen, das Bearbeiten und das Bereitstellen von Xamarin.iOS Projekten aus Windows. Dies beinhaltet die folgenden Features:

- Erstellen eines neuen iOS-Projekts

- Bearbeiten von iOS-Projekten und plattformübergreifenden Projektmappen, die auch Xamarin.Android- und UWP-Projekte enthalten.

- Bearbeiten von iOS-Projekten und plattformübergreifenden Projektmappen, die auch Xamarin.Android- und Windows Phone-Projekte enthalten.

- Unterstützung von Storyboard und XIB durch Verwenden des iOS-Designers.

- Bereitstellen und Debuggen von iOS-Anwendungen, in denen die App selbst in einem Simulator ausgeführt wird, oder auf einem Gerät, das mit dem Mac verbunden ist.

- iOS-Simulator unter Windows: Weitere Informationen zur Verwendung des iOS-Simulators unter Windows finden Sie in [diesem Handbuch](~/tools/ios-simulator.md).

<a name="configuring" />

## <a name="configuring-your-mac"></a>Konfigurieren Ihres Mac

<a name="installation"/>

### <a name="installation"></a>Installation

Um Xamarin.iOS-Tools auf Ihrem Mac-Host zu installieren, müssen Sie [Visual Studio für Mac installieren](https://docs.microsoft.com/visualstudio/mac/installation).

Sobald die Software installiert ist, führen Sie die Schritte in den nächsten Abschnitten zum Konfigurieren von Xamarin.iOS unter macOS durch, um zuzulassen, dass Xamarin für Visual Studio eine Verbindung herstellt.

> [!IMPORTANT]
>  Der Windows-Computer muss die gleiche Version von Xamarin.iOS verwenden wie der Mac-Computer, mit dem er verbunden ist. So stellen Sie sicher, dass dies der Fall ist:
>
> - **Visual Studio 2015 und frühere Versionen**: Stellen Sie sicher, dass den gleichen [Updatekanal](https://developer.xamarin.com/recipes/cross-platform/ide/change_updates_channel/) eingestellt haben wie Visual Studio für Mac.
>
> - **Visual Studio 2017, Releaseversion**: Stellen Sie sicher, dass Sie in Visual Studio für Mac den **stabilen** Kanal eingestellt haben.
>
> - **Visual Studio 2017, Vorschauversion**: Stellen Sie sicher, dass Sie in Visual Studio für Mac den **Alpha**-Kanal eingestellt haben.

<a name="configuration" />


### <a name="configuration"></a>Konfiguration

Um auf die Kommunikation zwischen der Xamarin-Erweiterung für Visual Studio und dem Mac zugreifen zu können, müssen Sie die **Remoteanmeldung** auf Ihrem Mac zulassen. Führen Sie dazu die folgenden Schritte aus:

1. Öffnen Sie *Spotlight* (**Cmd-Leerzeichen**), und suchen Sie nach **Remoteanmeldung**, und wählen Sie anschließend das **Freigabe**-Ergebnis aus. Dadurch werden im **Freigabe**-Panel die **Systemeinstellungen** geöffnet.

![Spotlight-Suche für Remoteanmeldung](images/spotlight.png)

2. Aktivieren Sie in der **Dienst**-Liste auf der linken Seite das Kontrollkästchen **Entfernte Anmeldung**, um die Verbindung von Xamarin für Visual Studio zu Ihrem Mac zuzulassen.

![Aktivieren der Option „Remoteanmeldung“ in der Liste der Dienste](images/sharing.png)

3. Stellen Sie sicher, dass die **Remoteanmeldung** so festgelegt ist, dass der Zugriff entweder für **Alle Benutzer** zugelassen wird oder dass Ihr Mac-Benutzername oder Ihre Mac-Benutzergruppe in der Liste der zugelassenen Benutzer auf der rechten Seite enthalten ist.

Ihr Mac sollte nun von Visual Studio erkannt werden, wenn dieser sich im selben Netzwerk befindet.

> [!NOTE]
> Falls Ihre macOS-Firewall so festgelegt ist, dass signierte Anwendungen standardmäßig gesperrt werden, sollten Sie zudem `mono-sgen` zulassen, um eingehende Verbindungen empfangen zu können. In diesem Fall werden Sie in einem Warnungsdialogfeld dazu aufgefordert.

<a name="developersetup"/>

### <a name="ios-developer-setup"></a>iOS-Entwickler-Setup

Für die iOS-Entwicklung ist es wichtig, dass der Mac-Computer mit den entsprechenden Signaturidentitäten konfiguriert ist. Dadurch können Sie Ihre Apps ordnungsgemäß signieren, sodass sie entweder über den App Store oder ad-hoc-verteilt werden können. Folgen Sie dem untenstehenden Link, um Anweisungen zum Einrichten einer Mac für iOS-Entwicklung mit Xamarin zu erhalten:

- [Bereitstellung von Geräten](~/ios/get-started/installation/device-provisioning/index.md?ide=vs)

Sobald Ihr Mac konfiguriert ist, ist es Zeit, Ihren Windows-Computer einzurichten.

<a name="windowsinstallation"/>

## <a name="windows-installation"></a>Windows-Installation

Xamarin kann im Rahmen Ihrer Visual Studio 2017- oder 2015-Installation installiert werden. Weitere Informationen zur Installation von Visual Studio-Tools für Xamarin finden Sie im [Windows-Installationshandbuch](~/cross-platform/get-started/installation/windows.md).

## <a name="installation-complete"></a>Installation abgeschlossen

Nachdem der Installationsvorgang abgeschlossen ist, sind noch einige zusätzliche Schritte erforderlich, damit alles funktioniert:

- [Verbinden von Visual Studio mit dem Mac](#connectingtomac): Visual Studio muss mit dem Mac-Buildhost verbunden sein, bevor Xamarin.iOS-Projekte erstellt werden können.
- [Konfigurieren der Visual Studio-Symbolleiste](#toolbar): Mit der Symbolleiste können Sie problemlos auf Xamarin.iOS-Funktionen in Visual Studio zugreifen.

<a name="connectingtomac" /> 

### <a name="connecting-to-the-mac"></a>Herstellen einer Verbindung mit dem Mac

Es erfolgt eine Verbindung von Xamarin.iOS für Visual Studio mit Ihrem Mac-Buildhost über eine SSH-Verbindung zwischen Computern. Weitere Informationen zur Verbindung finden Sie im [Connecting to Mac (Herstellen einer Verbindung mit Mac)](~/ios/get-started/installation/windows/connecting-to-mac/index.md)-Handbuch.

Um eine Verbindung zu Ihrem Mac herzustellen, führen Sie die folgenden Schritte aus:

- Navigieren Sie zu **Tools > Optionen**, und wählen Sie unter **Xamarin** **iOS-Einstellungen** aus:

  [![Der Bildschirm „iOS-Einstellungen“](images/image2.png)](images/image2.png#lightbox)

- Wenn die Bereitstellung des Mac ordnungsgemäß [konfiguriert](#configuration) wurde, um die **Remoteanmeldung** zu ermöglichen, sollten Sie nun Ihren Mac in der Liste auswählen können:

  [![Das Dialogfeld „Remotehost“](images/xma3.png)](images/xma3.png#lightbox)

- Es erfolgt daraufhin die Aufforderung zur Eingabe der Anmeldeinformationen Ihres Mac-Hosts:

  [![Das Anmeldedialogfeld](images/xma4.png)](images/xma4.png#lightbox)

- Wenn Sie eine Verbindung hergestellt haben, wird das „Verbindung erfolgreich“-Symbol neben dem Computernamen angezeigt:

  [![Das Dialogfeld für den Remotehost mit dem Symbol für „Verbindung erfolgreich“ neben dem Computernamen](images/image6.png)](images/image6.png#lightbox)

Sie werden bei jedem Starten von Visual Studio erneut verbunden.

<a name="toolbar" />

## <a name="visual-studio-toolbar-configuration"></a>Visual Studio-Symbolleistenkonfiguration

Wenn ein iOS-Projekt geöffnet ist, wird die iOS-Symbolleiste standardmäßig angezeigt und muss nicht konfiguriert werden.

Die folgenden Schritte können durchgeführt werden, wenn die iOS-Symbolleiste nicht angezeigt wird.

Um die Symbolleiste zu konfigurieren, öffnen Sie zuerst das **Ansicht > Symbolleisten**-Menü, und stellen Sie sicher, dass der **iOS**-Eintrag ausgewählt ist. Wählen Sie das Menüelement aus, wie in diesem Screenshot gezeigt—es sollte ausgewählt sein, um anzugeben, dass die Symbolleiste angezeigt wird:

[![„Symbolleisten > iOS“ auswählen](images/image31.png)](images/image31.png#lightbox)

### <a name="visual-studio-2015"></a>Visual Studio 2015

In früheren Versionen als Visual Studio 2017 kann es vorkommen, dass die Schaltfläche **Projektmappenplattformen** zur Standardsymbolleiste hinzugefügt werden muss. Dadurch kann ein iOS-Gerät oder der iOS-Simulator beim Debuggen ausgewählt werden. Befolgen Sie dazu die nachstehenden Anweisungen

Klicken Sie auf die Menüschaltfläche rechts von der Standardleiste:

- Wählen Sie **Schaltflächen hinzufügen oder entfernen** aus.
- Wählen Sie **Projektmappenplattformen** aus.

[![Projektmappenplattform auswählen](images/image35.png)](images/image35.png#lightbox)

Die **Standard**- und die **iOS**-Symbolleisten sollten jetzt diesem Screenshot entsprechen:

[![Die Standard- und iOS-Symbolleisten sollten jetzt diesem Screenshot entsprechen](images/image36.png)](images/image36.png#lightbox)

Nachdem die Symbolleistenkonfiguration abgeschlossen ist, können Sie damit beginnen Xamarin iOS für Visual Studio zu verwenden.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde eine ausführliche Anleitung zum Installieren, Konfigurieren und Verwenden von Xamarin iOS für Visual Studio vorgestellt.

Der Artikel hat das Installieren und das Konfigurieren der erforderlichen Tools unter Windows und Mac OS X behandelt.


## <a name="related-links"></a>Verwandte Links

- [Installation](~/cross-platform/get-started/installation/windows.md)
- [Gerätebereitstellung](~/ios/get-started/installation/device-provisioning/index.md)
- [Einführung in Xamarin.iOS für Visual Studio](~/ios/get-started/installation/windows/introduction-to-xamarin-ios-for-visual-studio.md)
- [Connecting a Mac to your Visual Studio environment with XMA (Herstellen einer Verbindung zwischen einem Mac und der Visual Studio-Umgebung mit XMA) (Video)](https://university.xamarin.com/lightninglectures/xamarin-mac-agent)
