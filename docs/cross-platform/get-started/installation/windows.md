---
title: Installieren von Xamarin in Visual Studio 2017
description: In diesem Dokument wird die Installation von Xamarin in Visual Studio 2017 beschrieben. Dabei werden die Voraussetzungen, der Installationsvorgang und das Überprüfen der Installation erläutert.
ms.prod: xamarin
ms.assetid: E20D4463-368E-4B60-A059-F50DB8C5552D
author: asb3993
ms.author: amburns
ms.date: 09/29/2017
ms.openlocfilehash: 6c2fe10b9b29901dfbb6173df131d093fe726bff
ms.sourcegitcommit: 3f2737f8abf9b855edf060474aa222e973abda3f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/28/2018
ms.locfileid: "37066951"
---
# <a name="installing-xamarin-in-visual-studio-2017"></a>Installieren von Xamarin in Visual Studio 2017

<a name="requirements" />

## <a name="requirements"></a>Anforderungen

Für die Installation von Xamarin in Visual Studio 2017 ist Folgendes erforderlich:

1. Windows 7 oder höher.

2. Visual Studio 2017 (Community, Professional oder Enterprise).

3. Xamarin für Visual Studio.

Weitere Informationen zu den Voraussetzungen für die Installation und Verwendung von Xamarin finden Sie unter [Systemanforderungen](~/cross-platform/get-started/requirements.md).

<a name="installation" />

## <a name="installation"></a>Installation

Xamarin kann im Rahmen einer neuen Visual Studio 2017-Installation installiert werden.
Führen Sie dazu die folgenden Schritte aus:

1. Laden Sie Visual Studio 2017 Community, Visual Studio Professional oder Visual Studio Enterprise von der Seite [Visual Studio](https://visualstudio.microsoft.com/vs/) herunter (Downloadlinks sind im unteren Bereich der Seite zu finden).

2. Doppelklicken Sie auf das heruntergeladene Paket, um die Installation zu starten.

3. Wählen Sie auf dem Installationsbildschirm die **Mobile-Entwicklung mit .NET**-Arbeitsauslastung aus: 

    [![Auswahl der Mobile-Entwicklung mit .NET auf dem Bildschirm „Arbeitsauslastung“](windows-images/01-mobile-dev-workload-sml.png)](windows-images/01-mobile-dev-workload.png#lightbox)

4. Schauen Sie sich, während **Mobile-Entwicklung mit .NET** ausgewählt ist, die **Zusammenfassung** im Bereich auf der rechten Seite an. Hier können Sie mobile Entwicklungsoptionen deaktivieren, die nicht installiert werden sollen. Standardmäßig sind alle im folgenden Screenshot gezeigten Optionen installiert (**Xamarin Workbooks**, **Xamarin Profiler**, **Xamarin Remoted Simulator**,  **Android NDK**, **Android SDK**, **Java SE Development Kit**, **Google Android-Emulator**, **F#-Unterstützung** und **Intel HAXM**):

    ![Zusammenfassungsbereich mit zu installierenden Xamarin-Optionen](windows-images/02-summary.png)

5. Wenn Sie mit der Installation von Visual Studio 2017 beginnen möchten, klicken Sie in der rechten unteren Ecke auf die Schaltfläche **Installieren**:

    ![Position der Schaltfläche „Installation“](windows-images/03-click-install.png)

   Je nachdem, welche Edition von Visual Studio 2017 Sie installieren, kann der Installationsvorgang viel Zeit in Anspruch nehmen. Sie können die Installation über die Statusleisten überwachen:

    ![Beispielhafter Screenshot der Statusleisten während der Installation](windows-images/04-progress-bars.png)

6. Klicken Sie nach Abschluss der Installation von Visual Studio 2017 auf die Schaltfläche **Start**, um Visual Studio zu starten:

    ![Position der Schaltfläche „Start“](windows-images/05-launch.png)

<a name="vs2017" />

### <a name="adding-xamarin-to-visual-studio-2017"></a>Hinzufügen von Xamarin zu Visual Studio 2017

Wenn Visual Studio 2017 bereits installiert ist, können Sie Xamarin hinzufügen, indem Sie den Visual Studio 2017-Installer erneut ausführen, um Workloads zu ändern (Einzelheiten finden Sie unter [Ändern von Visual Studio](https://docs.microsoft.com/visualstudio/install/modify-visual-studio)). Führen Sie als Nächstes die oben genannten Schritte zum Installieren von Xamarin aus.

Weitere Informationen zum Herunterladen und Installieren von Visual Studio 2017 finden Sie unter [Installieren von Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/install-visual-studio).


### <a name="verifying-installation"></a>Überprüfen der Installation

Sie können in Visual Studio 2017 überprüfen, ob Xamarin installiert ist, indem Sie auf das Menü **Hilfe** klicken. Wenn Xamarin installiert ist, müsste Ihnen ein **Xamarin**-Menüelement angezeigt werden, wie im folgenden Screenshot zu sehen ist:

![Im Menü „Hilfe“ angezeigtes Xamarin-Menüelement](windows-images/12-xamarin-menu-item.png)

Wenn Sie eine frühere Version von Visual Studio verwenden, können Sie auf **Hilfe > Microsoft Visual Studio** klicken und einen Bildlauf bis zur Liste der installierten Produkte durchführen. Dort können Sie dann überprüfen, ob Xamarin installiert ist:

![Anzeige mit installierten Visual Studio-Produkten](windows-images/13-xamarin-is-installed.png)

Weitere Informationen zum Suchen von Versionsinformationen finden Sie unter [Where can I find my version information and logs? (Wo finde ich meine Versionsinformationen und Protokolle?)](~/cross-platform/troubleshooting/questions/version-logs.md)

<a name="nextsteps" />

## <a name="next-steps"></a>Nächste Schritte

Nach der Installation von Xamarin in Visual Studio 2017 können Sie Code für Ihre Apps schreiben. Sie müssen jedoch weitere Konfigurationsschritte durchführen, um Ihre Apps zu erstellen und sie im Simulator, im Emulator und auf dem Gerät bereitzustellen. Lesen Sie die folgenden Anleitungen, um Ihre Installation abzuschließen und um plattformübergreifende Apps zu erstellen.

### <a name="ios"></a>iOS

Weitere ausführliche Informationen finden Sie im Handbuch [Installieren von Xamarin.iOS unter Windows](~/ios/get-started/installation/windows/index.md). 

1. [Installieren von Visual Studio für Mac](https://docs.microsoft.com/visualstudio/mac/installation)
2. [Connecting Visual Studio to your Mac build host (Herstellen einer Verbindung von Visual Studio mit Ihrem Mac-Buildhost)](~/ios/get-started/installation/windows/connecting-to-mac/index.md)
3. [iOS-Entwickler-Setup](~/ios/get-started/installation/device-provisioning/index.md) (zum Ausführen Ihrer Anwendung auf dem Gerät erforderlich).
5. [Remoted iOS Simulator (iOS-Remotesimulator)](~/tools/ios-simulator.md)
6. [Einführung in Xamarin.iOS für Visual Studio](~/ios/get-started/installation/windows/introduction-to-xamarin-ios-for-visual-studio.md)

### <a name="android"></a>Android

Weitere ausführliche Informationen finden Sie im Handbuch [Installieren von Xamarin.Android unter Windows](~/android/get-started/installation/windows.md).

1. [Xamarin.Android Configuration (Xamarin.Android-Konfiguration)](~/android/get-started/installation/windows.md#configuration)
2. [Using the Xamarin Android SDK Manager (Verwenden des Xamarin.Android SDK-Managers)](~/android/get-started/installation/android-sdk.md?ide=vs)
3. [Android SDK-Emulator](~/android/get-started/installation/android-emulator/index.md)
4. [Set Up Device for Development (Einrichten eines Geräts für die Entwicklung)](~/android/get-started/installation/set-up-device-for-development.md)
