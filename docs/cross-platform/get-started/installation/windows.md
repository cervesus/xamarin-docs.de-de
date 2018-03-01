---
title: Installieren von Xamarin in Visual Studio unter Windows
ms.topic: article
ms.prod: xamarin
ms.assetid: E20D4463-368E-4B60-A059-F50DB8C5552D
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 09/29/2017
ms.openlocfilehash: b68e03251b83192bdc5836af6ea54446ddaad24a
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="installing-xamarin-in-visual-studio-on-windows"></a>Installieren von Xamarin in Visual Studio unter Windows

Da Xamarin jetzt ohne zusätzliche Kosten in allen Editionen von Visual Studio enthalten ist und keine separate Lizenz erforderlich macht, können Sie über den Visual Studio-Installer Xamarin-Tools herunterladen und installieren.

-   [Anforderungen](#requirements)
-   [Installation](#installation)
-   [Hinzufügen von Xamarin zu Visual Studio 2017](#vs2017)
-   [Hinzufügen von Xamarin zu Visual Studio 2015](#vs2015)
-   [Überprüfen der Installation](#verifying)
-   [Nächste Schritte](#nextsteps)


<a name="requirements" />

# <a name="requirements"></a>Anforderungen

Für die Installation von Visual Studio-Tools für Xamarin ist Folgendes erforderlich:

1. Windows 7 oder höher.

2. Visual Studio 2015 oder 2017 (Community, Professional oder Enterprise).

3. Xamarin für Visual Studio.

Beachten Sie, dass Xamarin nicht mit Express-Editionen von Visual Studio verwendet werden kann, da Plug-Ins nicht unterstützt werden.

Weitere Informationen zu den Voraussetzungen für die Installation und Verwendung von Xamarin finden Sie unter [Systemanforderungen](~/cross-platform/get-started/requirements.md).


<a name="installation" />

# <a name="installation"></a>Installation

Xamarin kann im Rahmen einer neuen Visual Studio 2017-Installation installiert werden.
Führen Sie dazu die folgenden Schritte aus:

1. Laden Sie Visual Studio Community, Visual Studio Professional oder Visual Studio Enterprise von der Seite [Visual Studio](https://www.visualstudio.com/vs/) herunter (Downloadlinks sind im unteren Bereich der Seite zu finden).

2. Doppelklicken Sie auf das heruntergeladene Paket, um die Installation zu starten.

3. Wählen Sie auf dem Installationsbildschirm die **Mobile-Entwicklung mit .NET**-Arbeitsauslastung aus: 

    [![Auswahl der Mobile-Entwicklung mit .NET auf dem Bildschirm „Arbeitsauslastung“](windows-images/01-mobile-dev-workload-sml.png)](windows-images/01-mobile-dev-workload.png)

4. Schauen Sie sich, während **Mobile-Entwicklung mit .NET** ausgewählt ist, die **Zusammenfassung** im Bereich auf der rechten Seite an. Hier können Sie mobile Entwicklungsoptionen deaktivieren, die nicht installiert werden sollen. Standardmäßig sind alle im folgenden Screenshot gezeigten Optionen installiert (**Xamarin Workbooks**, **Xamarin Profiler**, **Xamarin Remoted Simulator**,  **Android NDK**, **Android SDK**, **Java SE Development Kit**, **Google Android-Emulator**, **F#-Unterstützung** und **Intel HAXM**):

    ![Zusammenfassungsbereich mit zu installierenden Xamarin-Optionen](windows-images/02-summary.png)

5. Wenn Sie mit der Installation von Visual Studio beginnen möchten, klicken Sie in der rechten unteren Ecke auf die Schaltfläche **Installieren**:

    ![Position der Schaltfläche „Installation“](windows-images/03-click-install.png)

   Je nachdem, welche Edition von Visual Studio Sie installieren, kann der Installationsvorgang viel Zeit in Anspruch nehmen. Sie können die Installation über die Statusleisten überwachen:

    ![Beispielhafter Screenshot der Statusleisten während der Installation](windows-images/04-progress-bars.png)

6. Klicken Sie nach Abschluss der Installation von Visual Studio auf die Schaltfläche **Start**, um Visual Studio zu starten:

    ![Position der Schaltfläche „Start“](windows-images/05-launch.png)


<a name="vs2017" />

## <a name="adding-xamarin-to-visual-studio-2017"></a>Hinzufügen von Xamarin zu Visual Studio 2017

Wenn Visual Studio 2017 bereits installiert ist, können Sie Xamarin hinzufügen, indem Sie den Visual Studio-Installer erneut ausführen, um Arbeitsauslastungen zu ändern (Einzelheiten finden Sie unter [Ändern von Visual Studio](https://docs.microsoft.com/visualstudio/install/modify-visual-studio)). Führen Sie als Nächstes die oben genannten Schritte zum Installieren von Xamarin aus.

Weitere Informationen zum Herunterladen und Installieren von Visual Studio 2017 finden Sie unter [Installieren von Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/install-visual-studio).


<a name="vs2015" />

## <a name="adding-xamarin-to-visual-studio-2015"></a>Hinzufügen von Xamarin zu Visual Studio 2015

Führen Sie die folgenden Schritte aus, um Xamarin.Android zu einer vorhandenen Installation von Visual Studio 2015 hinzuzufügen:

1. Klicken Sie unter Windows mit der rechten Maustaste auf die Schaltfläche **Start**, und wählen Sie **Programme und Funktionen** aus.

2. Klicken Sie mit der rechten Maustaste auf **Microsoft Visual Studio**, und klicken Sie anschließend auf **Ändern**.

3. Wenn das Dialogfeld „Visual Studio-Installer“ angezeigt wird, klicken Sie auf die Schaltfläche **Ändern**.

4. Führen Sie auf der Registerkarte **Funktionen** einen Bildlauf nach unten bis **Plattformübergreifende Mobile-Entwicklung** durch. Klicken Sie auf das Kontrollkästchen neben **C#/.NET (Xamarin)**:

    ![Hinzufügen von C#/.NET Xamarin zu Visual Studio 2015](windows-images/06-add-xamarin.png)

5. Klicken Sie auf die Schaltfläche **UPDATE**, um Xamarin zu Visual Studio hinzuzufügen.


<a name="verifying" />

## <a name="verifying-installation"></a>Überprüfen der Installation

Sie können in Visual Studio 2017 überprüfen, ob Xamarin installiert ist, indem Sie auf das Menü **Hilfe** klicken. Wenn Xamarin installiert ist, müsste Ihnen ein **Xamarin**-Menüelement angezeigt werden, wie im folgenden Screenshot zu sehen ist:

![Im Menü „Hilfe“ angezeigtes Xamarin-Menüelement](windows-images/12-xamarin-menu-item.png)

Wenn Sie eine frühere Version von Visual Studio verwenden, können Sie auf **Hilfe > Microsoft Visual Studio** klicken und einen Bildlauf bis zur Liste der installierten Produkte durchführen. Dort können Sie dann überprüfen, ob Xamarin installiert ist:

![Anzeige mit installierten Visual Studio-Produkten](windows-images/13-xamarin-is-installed.png)

Weitere Informationen zum Suchen von Versionsinformationen finden Sie unter [Where can I find my version information and logs? (Wo finde ich meine Versionsinformationen und Protokolle?)](~/cross-platform/troubleshooting/questions/version-logs.md)

<a name="nextsteps" />

# <a name="next-steps"></a>Nächste Schritte

Mit der Installation von Visual Studio-Tools für Xamarin können Sie Code für Ihre Apps schreiben, müssen jedoch keine weiteren Konfigurationsschritte durchführen, um Ihre Apps zu erstellen und sie auf dem Simulator, Emulator und Gerät bereitzustellen. Lesen Sie die folgenden Anleitungen, um Ihre Installation abzuschließen und um plattformübergreifende Apps zu erstellen.

## <a name="ios"></a>iOS

Weitere ausführliche Informationen finden Sie im Handbuch [Installieren von Xamarin.iOS unter Windows](~/ios/get-started/installation/windows/index.md). 

1. [Install Xamarin.iOS tools on your Mac (Installieren von Xamarin.iOS-Tools auf Ihrem Mac)](~/ios/get-started/installation/windows/index.md#installation)
2. [Configuring your Mac (Konfigurieren Ihres Mac)](~/ios/get-started/installation/windows/index.md#configuration)
3. [iOS Developer Setup (iOS-Entwickler-Setup)](~/ios/get-started/installation/windows/index.md#developersetup) (Zum Ausführen Ihrer Anwendung auf dem Gerät).
4. [Connecting Visual Studio to your Mac build host (Herstellen einer Verbindung von Visual Studio mit Ihrem Mac-Buildhost)](~/ios/get-started/installation/windows/index.md#connectingtomac)
5. [Remoted iOS Simulator (iOS-Remotesimulator)](~/tools/ios-simulator.md)
6. [Einführung in Xamarin.iOS für Visual Studio](~/ios/get-started/installation/windows/introduction-to-xamarin-ios-for-visual-studio.md)

## <a name="android"></a>Android

Weitere ausführliche Informationen finden Sie im Handbuch [Installieren von Xamarin.Android unter Windows](~/android/get-started/installation/windows.md).

1. [Xamarin.Android Configuration (Xamarin.Android-Konfiguration)](~/android/get-started/installation/windows.md#configuration)
2. [Using the Xamarin Android SDK Manager (Verwenden des Xamarin.Android SDK-Managers)](~/android/get-started/installation/android-sdk.md?ide=vs)
3. [Android SDK-Emulator](~/android/get-started/installation/android-emulator/index.md)
4. [Set Up Device for Development (Einrichten eines Geräts für die Entwicklung)](~/android/get-started/installation/set-up-device-for-development.md)
