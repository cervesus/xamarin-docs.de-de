---
title: Windows-Installation
description: In diesem Leitfaden werden die Schritte für die Installation von Xamarin.Android für Visual Studio unter Windows beschrieben. Ferner wird erläutert, wie Xamarin.Android konfiguriert werden muss, damit Sie Ihre erste Xamarin.Android-Anwendung erstellen können.
ms.prod: xamarin
ms.assetid: 2BE4D5AD-D468-B177-8F96-837D084E7DE1
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/22/2018
ms.openlocfilehash: 32ededcda1fdfc463269c7e4a2db444edab51d22
ms.sourcegitcommit: 1dd7d09b60fcb1bf15ba54831ed3dd46aa5240cb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2019
ms.locfileid: "70119774"
---
# <a name="windows-installation"></a>Windows-Installation

_In diesem Leitfaden werden die Schritte für die Installation von Xamarin.Android für Visual Studio unter Windows beschrieben. Weiterhin wird erläutert, wie Xamarin.Android konfiguriert werden muss, damit Sie Ihre erste Xamarin.Android-Anwendung erstellen können._


## <a name="overview"></a>Übersicht

Da Xamarin jetzt ohne zusätzliche Kosten in allen Editionen von Visual Studio enthalten ist und keine separate Lizenz erforderlich macht, können Sie über den Visual Studio-Installer Xamarin.Android-Tools herunterladen und installieren.
(Die Schritte für die manuelle Installation und Lizenzierung, die in früheren Versionen von Xamarin.Android ausgeführt werden musste, sind nicht mehr erforderlich.) In diesem Leitfaden lernen Sie Folgendes:

- Vorgehensweise beim Konfigurieren benutzerdefinierter Speicherorte für Java Development Kit, Android SDK und Android NDK.

- Vorgehensweise beim Starten des Android SDK-Managers zum Herunterladen und Installieren weiterer Android SDK-Komponenten.

- Vorgehensweise beim Vorbereiten eines Android-Geräts oder -Emulators für das Debuggen und Testen.

- Vorgehensweise beim  Erstellen Ihres ersten Xamarin.Android-App-Projekts.

Bis zum Ende dieses Leitfadens werden Sie über eine funktionierende Xamarin.Android-Installation verfügen, die in Visual Studio integriert ist. Sie sind dann bereit, Ihre erste Xamarin.Android-Anwendung zu erstellen.

## <a name="installation"></a>Installation

Ausführliche Informationen zum Installieren von Xamarin für die Verwendung mit Visual Studio unter Windows finden Sie im [Installationshandbuch für Windows](~/get-started/installation/windows.md).


## <a name="configuration"></a>Konfiguration

Xamarin.Android verwendet das Java Development Kit (JDK) und das Android SDK für die Erstellung von Apps. Während der Installation ordnet der Visual Studio-Installer diese Tools in ihren Standardspeicherorten an und konfiguriert die Entwicklungsumgebung mit der entsprechenden Pfadkonfiguration. Sie können diese Speicherorte anzeigen und ändern, indem Sie auf **Tools > Optionen > Xamarin > Android-Einstellungen** klicken:

![Screenshot des Dialogfelds „Xamarin > Android-Einstellungen“](windows-images/07-settings.png)

Diese Standardspeicherorte funktionieren für die meisten Benutzer, ohne dass weitere Änderungen erforderlich sind. Sie müssen Visual Studio jedoch möglicherweise mit benutzerdefinierten Speicherorten für diese Tools konfigurieren (z.B., wenn Sie das Java JDK, Android SDK oder NDK an einem anderen Speicherort installiert haben). Klicken Sie neben einem Pfad, den Sie ändern möchten, auf **Ändern**, und navigieren Sie anschließend zu dem neuen Speicherort.

Xamarin.Android verwendet [JDK 8](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html), was erforderlich ist, wenn Sie für die API-Ebene 24 oder höher entwickeln (JDK 8 unterstützt auch API-Ebenen älter als 24). Sie können weiterhin [JDK 7](https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) verwenden, wenn Sie spezifisch für API-Ebene 23 oder früher entwickeln.

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

Der [Android-Emulator](https://developer.android.com/studio/run/emulator) kann ein hilfreiches Tool zum Entwickeln und Testen einer Xamarin.Android-App sein. Angenommen, während der Entwicklung ist kein physisches Gerät, z. B. ein Tablet-PC, verfügbar, oder ein Entwickler möchte vor dem Ausführen eines Commits für Code einige Integrationstests auf seinem Computer ausführen.

Das Emulieren eines Android-Geräts auf einem Computer umfasst die folgenden Komponenten:

- **Google Android-Emulator** &ndash; Dies ist ein Emulator, der auf [QEMU](https://www.qemu.org/) basiert und ein virtualisiertes Gerät erstellt, das auf der Arbeitsstation des Entwicklers ausgeführt wird.
- **Emulator-Image** &ndash; Ein _Emulator-Image_ ist eine Vorlage oder eine Spezifikation der Hardware und des Betriebssystems, die virtualisiert werden sollen. Beispielsweise würde ein Emulator-Image die Hardwareanforderungen für ein Nexus 5X mit Android 7.0 und installierten Google Play-Diensten identifizieren. Ein anderes Emulator-Image spezifiziert möglicherweise ein 10"-Tablet mit Android 6.0.
- **Virtuelles Android-Gerät (AVD)** &ndash; Ein _virtuelles Android-Gerät_ ist ein aus einem Emulator-Image erstelltes „emuliertes“ Android-Gerät. Beim Ausführen und Testen von Android-Apps startet Xamarin.Android den Android-Emulator, startet ein bestimmtes AVD, installiert die APK und führt dann die App aus.

Eine erhebliche Verbesserung der Leistung bei der Entwicklung auf x86-Computern kann erreicht werden, indem Sie spezielle für die x86-Architektur optimierte Emulator-Images und eine der beiden Virtualisierungstechnologien verwenden:

1. Hyper-V von Microsoft: Verfügbar auf Computern mit dem April-Update 2018 von Windows 10 oder höher.
2. Hardware Accelerated Execution Manager (HAXM) von Intel: Verfügbar auf x86-Computern mit OS X, macOS oder älteren Versionen von Windows.

Weitere Informationen zum Android-Emulator und zu Hyper-V und HAXM finden Sie im Leitfaden [Hardwarebeschleunigung für verbesserte Leistung des Emulators](~/android/get-started/installation/android-emulator/hardware-acceleration.md).

> [!NOTE]
> In Versionen von Windows vor dem April-Update 2018 von Windows 10, ist HAXM nicht mit Hyper-V kompatibel. In diesem Szenario ist es erforderlich, entweder [Hyper-V zu deaktivieren](~/android/get-started/installation/android-emulator/troubleshooting.md#disable-hyperv) oder die langsameren Emulator-Images ohne x86-Optimierungen zu verwenden.


<a name="device" />

### <a name="android-device"></a>Android-Gerät

Wenn Sie über ein physisches Android-Gerät für Testzwecke verfügen, ist dies ein guter Zeitpunkt, um es für die Entwicklung einzurichten. Unter [Set Up Device for Development](~/android/get-started/installation/set-up-device-for-development.md) (Einrichten des Geräts für die Entwicklung) finden Sie Informationen zu der Konfiguration Ihres Android-Geräts für die Entwicklung, dem anschließenden Verbinden mit Ihrem Computer zur Ausführung und dem Debuggen von Xamarin.Android-Anwendungen.


## <a name="create-an-application"></a>Erstellen einer Anwendung

Nachdem Sie Xamarin.Android installiert haben, können Sie Visual Studio starten, um ein neues Projekt zu erstellen. Klicken Sie auf **Datei > Neu > Projekt**, um mit dem Erstellen Ihrer App zu beginnen:

![Erstellen eines neuen Projekts](windows-images/10-new-project.png)

Wählen Sie im Dialogfeld **Neues Projekt** den Eintrag **Android** unter **Vorlagen** aus, und klicken Sie im rechten Bereich auf **Android-App**. Geben Sie einen Namen für Ihre App ein (im nachfolgenden Screenshot heißt die App **MyApp**), und klicken Sie anschließend auf **OK**:

[![Screenshot des Dialogfelds „Neues Projekt“, Erstellen einer leeren Android-App](windows-images/11-first-app-sml.w157.png)](windows-images/11-first-app.w157.png#lightbox)

Das ist alles! Nun können Sie mit Xamarin.Android Android-Anwendungen erstellen!


## <a name="summary"></a>Zusammenfassung

In diesem Artikel haben Sie gelernt, wie Sie die Xamarin.Android-Plattform unter Windows einrichten und installieren, wie Sie (optional) Visual Studio mit benutzerdefinierten Java JDK- und Android SDK-Installationsspeicherorten konfigurieren; ferner, wie Sie den SDK-Manager für die Installation zusätzlicher Android SDK-Komponenten starten, wie Sie ein Android-Gerät oder einen Android-Emulator einrichten und wie Sie mit der Erstellung Ihrer ersten Anwendung beginnen.

Im nächsten Schritt schauen Sie sich die [Hallo, Android](~/android/get-started/hello-android/index.md)-Tutorials an, um zu lernen, wie eine funktionierende Xamarin.Android-App erstellt wird.


## <a name="related-links"></a>Verwandte Links

- [Herunterladen von Visual Studio](https://visualstudio.microsoft.com/vs/)
- [Installieren von Visual Studio-Tools für Xamarin](~/get-started/installation/windows.md)
- [Systemanforderungen](~/cross-platform/get-started/requirements.md)
- [Android SDK-Setup](~/android/get-started/installation/android-sdk.md)
- [Setup von Android-Emulator](~/android/get-started/installation/android-emulator/index.md)
- [Set Up Device for Development (Einrichten eines Geräts für die Entwicklung)](~/android/get-started/installation/set-up-device-for-development.md)
- [Ausführen von Apps auf dem Android-Emulator](https://developer.android.com/studio/run/emulator#Requirements)
