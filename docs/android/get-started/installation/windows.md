---
title: Windows-Installation
description: "In diesem Leitfaden werden die Schritte für die Installation von Xamarin.Android für Visual Studio unter Windows beschrieben. Ferner wird erläutert, wie Xamarin.Android konfiguriert werden muss, damit Sie Ihre erste Xamarin.Android-Anwendung erstellen können."
ms.topic: article
ms.prod: xamarin
ms.assetid: 2BE4D5AD-D468-B177-8F96-837D084E7DE1
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: 7cf21e75c9ae2f3c27b07cb20f1044779b42b06b
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/12/2018
---
# <a name="windows-installation"></a>Windows-Installation

_In diesem Leitfaden werden die Schritte für die Installation von Xamarin.Android für Visual Studio unter Windows beschrieben. Weiterhin wird erläutert, wie Xamarin.Android konfiguriert werden muss, damit Sie Ihre erste Xamarin.Android-Anwendung erstellen können._


## <a name="overview"></a>Übersicht

Da Xamarin jetzt ohne zusätzliche Kosten in allen Editionen von Visual Studio enthalten ist und keine separate Lizenz erforderlich macht, können Sie über den Visual Studio-Installer Xamarin.Android-Tools herunterladen und installieren.
(Die Schritte für die manuelle Installation und Lizenzierung, die in früheren Versionen von Xamarin.Android ausgeführt werden musste, sind nicht mehr erforderlich.) In diesem Leitfaden lernen Sie Folgendes:

-   Vorgehensweise beim Konfigurieren benutzerdefinierter Speicherorte für Java Development Kit, Android SDK und Android NDK.

-   Vorgehensweise beim Starten des Android SDK-Managers zum Herunterladen und Installieren weiterer Android SDK-Komponenten.

-   Vorgehensweise beim Vorbereiten eines Android-Geräts oder -Emulators für das Debuggen und Testen.

-   Vorgehensweise beim  Erstellen Ihres ersten Xamarin.Android-App-Projekts.

Bis zum Ende dieses Leitfadens werden Sie über eine funktionierende Xamarin.Android-Installation verfügen, die in Visual Studio integriert ist. Sie sind dann bereit, Ihre erste Xamarin.Android-Anwendung zu erstellen.

## <a name="installation"></a>Installation

Ausführliche Informationen zum Installieren von Xamarin für die Verwendung mit Visual Studio unter Windows finden Sie im [Installationshandbuch für Windows](~/cross-platform/get-started/installation/windows.md).


## <a name="configuration"></a>Konfiguration

Xamarin.Android verwendet das Java Development Kit (JDK) und das Android SDK für die Erstellung von Apps. Während der Installation ordnet der Visual Studio-Installer diese Tools in ihren Standardspeicherorten an und konfiguriert die Entwicklungsumgebung mit der entsprechenden Pfadkonfiguration. Sie können diese Speicherorte anzeigen und ändern, indem Sie auf **Tools > Optionen > Xamarin > Android-Einstellungen** klicken:

![Screenshot des Dialogfelds „Xamarin > Android-Einstellungen“](windows-images/07-settings.png)

Diese Standardspeicherorte funktionieren für die meisten Benutzer, ohne dass weitere Änderungen erforderlich sind. Sie müssen Visual Studio jedoch möglicherweise mit benutzerdefinierten Speicherorten für diese Tools konfigurieren (z.B., wenn Sie das Java JDK, Android SDK oder NDK an einem anderen Speicherort installiert haben). Klicken Sie neben einem Pfad, den Sie ändern möchten, auf **Ändern**, und navigieren Sie anschließend zu dem neuen Speicherort.

Xamarin.Android verwendet [JDK 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html), was erforderlich ist, wenn Sie für die API-Ebene 24 oder höher entwickeln (JDK 8 unterstützt auch API-Ebenen älter als 24). Sie können weiterhin [JDK 7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) verwenden, wenn Sie spezifisch für API-Ebene 23 oder früher entwickeln.

> [!IMPORTANT]
> Xamarin.Android unterstützt JDK 9 nicht.


### <a name="android-sdk-manager"></a>Android-SDK-Manager

Android verwendet mehrere Einstellungen für die Android-API-Ebene, um die Kompatibilität Ihrer App über die verschiedenen Android-Versionen zu bestimmen (weitere Informationen zu Android-API-Ebenen finden Sie unter [Understanding Android API Levels](~/android/app-fundamentals/android-api-levels.md) (Grundlegendes zu Android-API-Ebenen)).
Abhängig von Ihrer/Ihren Android-API-Zielebene(n) müssen Sie möglicherweise Android SDK-Komponenten herunterladen und installieren. Darüber hinaus müssen Sie möglicherweise im Android SDK bereitgestellte optionale Tools und Emulator-Images installieren. Verwenden Sie hierzu den **Android SDK-Manager**. Sie können den **Android SDK-Manager** starten, indem Sie auf **Tools > Android > Android SDK-Manager** klicken:

[![Vorgehensweise beim Starten des Android SDK-Managers](windows-images/08-sdk-manager-sml.png)](windows-images/08-sdk-manager.png#lightbox)

In Visual Studio wird standardmäßig der Google Android SDK-Manager installiert:

![Beispielhafter Screenshot des Google Android SDK-Managers](windows-images/09-google-sdk-manager.png)

Sie können über den Google Android SDK-Manager Versionen des Android SDK Tools-Pakets bis zu Version 25.2.3 installieren. Wenn Sie eine höhere Version des Android SDK Tools-Pakets verwenden müssen, müssen Sie jedoch das Xamarin Android SDK-Manager-Plug-in für Visual Studio (in Visual Studio Marketplace verfügbar) installieren. Dies ist erforderlich, da der eigenständige SDK-Manager von Google in Version 25.2.3 des Android SDK Tools-Pakets veraltet war. 

Weitere Informationen zur Verwendung des Xamarin Android SDK-Managers finden Sie unter [Android SDK Setup (Android SDK-Setup)](~/android/get-started/installation/android-sdk.md).


### <a name="android-emulator"></a>Android-Emulator

Wenn Sie nicht über ein physisches Android-Gerät für Testzwecke verfügen, können Sie Ihre App mit einem Android-Emulator testen. Weitere Informationen zum Google Android-Emulator finden Sie unter [Android SDK Emulator (Android SDK-Emulator)](~/android/deploy-test/debugging/android-sdk-emulator/index.md).

Der Google Android-Emulator verwendet Intel HAXM (Hardware Accelerated Execution Manager). Dieser kann mit den von anderen Emulatoren verwendeten Virtualisierungstechnologien in Konflikt stehen. Es gibt folgende drei Hauptvirtualisierungstechnologien:

-   **Hyper-V** (wird vom Visual Studio-Emulator für Android und dem Windows Phone-Emulator verwendet) 

-   **Virtual Box** (wird von Genymotion verwendet)

-   **Intel HAXM** (wird vom Google Android SDK-Emulator verwendet) 

Da die CPU eines Entwicklungscomputers jeweils nur eine Virtualisierungstechnologie unterstützen kann, sollte auf einem Entwicklungscomputer jeweils nur eine eingesetzt werden.

<a name="device" />

### <a name="android-device"></a>Android-Gerät

Wenn Sie über ein physisches Android-Gerät für Testzwecke verfügen, ist dies ein guter Zeitpunkt, um es für die Entwicklung einzurichten. Unter [Set Up Device for Development](~/android/get-started/installation/set-up-device-for-development.md) (Einrichten des Geräts für die Entwicklung) finden Sie Informationen zu der Konfiguration Ihres Android-Geräts für die Entwicklung, dem anschließenden Verbinden mit Ihrem Computer zur Ausführung und dem Debuggen von Xamarin.Android-Anwendungen.


## <a name="create-an-application"></a>Erstellen einer Anwendung

Nachdem Sie Xamarin.Android installiert haben, können Sie Visual Studio starten, um ein neues Projekt zu erstellen. Klicken Sie auf **Datei > Neu > Projekt**, um mit dem Erstellen Ihrer App zu beginnen:

![Erstellen eines neuen Projekts](windows-images/10-new-project.png)

Wählen Sie im Dialogfeld **Neues Projekt** den Eintrag **Android** unter **Vorlagen** aus, und klicken Sie im rechten Bereich auf **Leere App (Android)**. Geben Sie einen Namen für Ihre App ein (im nachfolgenden Screenshot heißt die App **MyApp**), und klicken Sie anschließend auf **OK**:

[![Screenshot des Dialogfelds „Neues Projekt“, Erstellen einer leeren Android-App](windows-images/11-first-app-sml.png)](windows-images/11-first-app.png#lightbox)

Das ist alles! Nun können Sie mit Xamarin.Android Android-Anwendungen erstellen!


## <a name="summary"></a>Zusammenfassung

In diesem Artikel haben Sie gelernt, wie Sie die Xamarin.Android-Plattform unter Windows einrichten und installieren, wie Sie (optional) Visual Studio mit benutzerdefinierten Java JDK- und Android SDK-Installationsspeicherorten konfigurieren; ferner, wie Sie den SDK-Manager für die Installation zusätzlicher Android SDK-Komponenten starten, wie Sie ein Android-Gerät oder einen Android-Emulator einrichten und wie Sie mit der Erstellung Ihrer ersten Anwendung beginnen.

Im nächsten Schritt schauen Sie sich die [Hallo, Android](~/android/get-started/hello-android/index.md)-Tutorials an, um zu lernen, wie eine funktionierende Xamarin.Android-App erstellt wird.


## <a name="related-links"></a>Verwandte Links

- [Herunterladen von Visual Studio](https://www.visualstudio.com/vs/)
- [Installieren von Visual Studio-Tools für Xamarin](~/cross-platform/get-started/installation/windows.md)
- [Systemanforderungen](~/cross-platform/get-started/requirements.md)
- [Android SDK-Setup](~/android/get-started/installation/android-sdk.md)
- [Android SDK-Emulator](~/android/get-started/installation/android-emulator/index.md)
- [Set Up Device for Development (Einrichten eines Geräts für die Entwicklung)](~/android/get-started/installation/set-up-device-for-development.md)
