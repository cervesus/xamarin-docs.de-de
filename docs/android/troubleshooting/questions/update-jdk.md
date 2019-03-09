---
title: Wie aktualisiere ich die Java Development Kit-Version (JDK)?
description: In diesem Artikel wird veranschaulicht, wie zum Aktualisieren der Java Development Kit (JDK) Version unter Windows und Mac.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 4b3ac51d-18dd-4034-87b4-4365194e4ece
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 09/07/2018
ms.openlocfilehash: 290a7deef4fdc10163bdb09881382f011b0dcd32
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2019
ms.locfileid: "57670849"
---
# <a name="how-do-i-update-the-java-development-kit-jdk-version"></a>Wie aktualisiere ich die Java Development Kit-Version (JDK)?

_In diesem Artikel wird veranschaulicht, wie zum Aktualisieren der Java Development Kit (JDK) Version unter Windows und Mac._

## <a name="overview"></a>Übersicht

Xamarin.Android verwendet das Java Development Kit (JDK), um mit dem Android SDK für Android-apps erstellen und Ausführen des Android Designers integrieren. Die neuesten Versionen des Android SDK-API (24 oder höher) erfordern JDK 8 (1.8). Alternativ können Sie installieren die [Microsoft Mobile OpenJDK Preview](~/android/get-started/installation/openjdk.md). Microsoft Mobile OpenJDK wird schließlich JDK 8 für die Entwicklung von Xamarin.Android ersetzt.

Um auf der Microsoft Mobile OpenJDK aktualisieren, finden Sie unter [Microsoft Mobile OpenJDK Preview](~/android/get-started/installation/openjdk.md). Um auf JDK 8 aktualisieren, gehen Sie folgendermaßen vor:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1.  Laden Sie JDK 8 (1.8) herunter, aus der [Oracle-Website](https://www.oracle.com/technetwork/java/javase/downloads/index.html):

    ![Screenshot des JDK-download-Seite auf der Oracle-website](update-jdk-images/image1.png)

2.  Wählen Sie die 64-Bit-Version, der Rendering ermöglichen [benutzerdefinierte Steuerelemente](https://developer.xamarin.com/releases/vs/xamarin.vs_4/xamarin.vs_4.2/#androiddesignercustomcontrols) im Xamarin Android Designer:

    ![Wählen das Windows X64 JDK-Paket zum Herunterladen von der JDK-Download-Seite](update-jdk-images/image2.png)

3.  Führen Sie die .exe aus, und installieren Sie die **Entwicklungstools**:

    ![Installieren die Entwicklungstools im JDK-installer](update-jdk-images/image3.png)

4.  Öffnen Sie Visual Studio, und Aktualisieren der **Java Development Kit-Speicherort** , zeigen Sie auf das neue JDK unter **Tools > Optionen > Xamarin > Android-Einstellungen > Java Development Kit-Speicherort**:

    [![Path-Einstellung für das JDK auf der Seite Einstellungen für Android](update-jdk-images/image4-sml.png)](update-jdk-images/image4.png#lightbox)

Achten Sie darauf, dass Visual Studio neu starten, nach dem Aktualisieren des Speicherorts.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

1.  Laden Sie JDK 8 (1.8) herunter, aus der [Oracle-Website](https://www.oracle.com/technetwork/java/javase/downloads/index.html):

    ![Screenshot des JDK-download-Seite auf der Oracle-website](update-jdk-images/image1.png)

2.  Öffnen Sie die DMG-Datei, und führen Sie das pkg-Installationsprogramm:

    ![Ausführen des JDK-Installationsprogramms unter macOS](update-jdk-images/image5.png)

Mac OS wird automatisch die neue Version des JDK als Standard festgelegt, durch die Aktualisierung **/System/Library/Frameworks/JavaVM.framework/Versions/Current**. Sie können dann überprüfen Sie, dass die **Java SDK (JDK)** Speicherort festgelegt ist, auf den erwarteten Standardwert **"/ usr"** unter **Visual Studio für Mac > Einstellungen > Projekte > SDK-Speicherorte > Android > Standorte > Speicherort für Java-SDK (JDK)**:

[![Festlegen von JDK-Speicherorts in der Registerkarte "Android Speicherorte"](update-jdk-images/image6-sml.png)](update-jdk-images/image6.png#lightbox)

-----

