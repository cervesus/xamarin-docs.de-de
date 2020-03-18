---
title: Wie aktualisiere ich die Java Development Kit-Version (JDK)?
description: In diesem Artikel wird erklärt, wie die Java Development Kit-Version (JDK) unter Windows und Mac aktualisiert wird.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 4b3ac51d-18dd-4034-87b4-4365194e4ece
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 09/07/2018
ms.openlocfilehash: 0f7499551db7d86d7978b9c3e1f562a2f054c202
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2020
ms.locfileid: "73019528"
---
# <a name="how-do-i-update-the-java-development-kit-jdk-version"></a>Wie aktualisiere ich die Java Development Kit-Version (JDK)?

_In diesem Artikel wird erklärt, wie die Java Development Kit-Version (JDK) unter Windows und Mac aktualisiert wird._

## <a name="overview"></a>Übersicht

Xamarin.Android verwendet das Java Development Kit (JDK) für die Android SDK-Integration zum Erstellen von Android-Apps und zum Ausführen von Android Designer. Die neuesten Versionen des Android SDK (API 24 und höher) erfordern JDK 8 (1.8). Alternativ können Sie die [Vorschauversion von Microsoft Mobile OpenJDK](~/android/get-started/installation/openjdk.md) installieren. Das Microsoft Mobile OpenJDK ersetzt schließlich JDK 8 für die Xamarin.Android-Entwicklung.

Informationen zum Aktualisieren von Microsoft Mobile OpenJDK finden Sie unter [Microsoft Mobile OpenJDK (Vorschauversion)](~/android/get-started/installation/openjdk.md). Führen Sie die folgenden Schritte aus, um auf JDK 8 zu aktualisieren:

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

1. Laden Sie JDK 8 (1.8) von der [Oracle-Website](https://www.oracle.com/technetwork/java/javase/downloads/index.html) herunter:

    ![Screenshot der JDK-Downloadseite auf der Oracle-Website](update-jdk-images/image1.png)

2. Wählen Sie die 64-Bit-Version aus, um das Rendern von [benutzerdefinierten Steuerelementen](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/vs/xamarin.vs_4/xamarin.vs_4.2/index.md#androiddesignercustomcontrols) im Xamarin Android-Designer zu ermöglichen:

    ![Auswählen des Windows x64 JDK-Pakets, das von der JDK-Downloadseite heruntergeladen werden soll](update-jdk-images/image2.png)

3. Führen Sie die EXE-Version aus, und installieren Sie die **Entwicklungstools**:

    ![Installieren der Entwicklungstools im JDK-Installationsprogramm](update-jdk-images/image3.png)

4. Öffnen Sie Visual Studio, und aktualisieren Sie den **Java Development Kit-Speicherort** so, dass er auf das neue JDK unter **Extras > Optionen > Xamarin > Android-Einstellungen > Java Development Kit-Speicherort** weist:

    [![Pfadeinstellung für das JDK auf der Seite für Android-Einstellungen](update-jdk-images/image4-sml.png)](update-jdk-images/image4.png#lightbox)

Stellen Sie sicher, dass Sie Visual Studio nach dem Aktualisieren des Speicherorts neu starten.

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

1. Laden Sie JDK 8 (1.8) von der [Oracle-Website](https://www.oracle.com/technetwork/java/javase/downloads/index.html):

    ![Screenshot der JDK-Downloadseite auf der Oracle-Website](update-jdk-images/image1.png)

2. Öffnen Sie die DMG-Datei, und führen Sie das PKG-Installationsprogramm aus:

    ![Ausführen des JDK-Installationsprogramms unter macOS](update-jdk-images/image5.png)

Die neue JDK-Version wird von Mac OS automatisch als Standardversion festgelegt, indem **/System/Library/Frameworks/JavaVM.Framework/Versions/Current** aktualisiert wird. Sie können dann nochmals überprüfen, ob der Speicherort des **Java SDK (JDK)** auf den erwarteten Standardwert von **/usr** unter **Visual Studio für Mac > Einstellungen > Projekte > SDK-Speicherorte > Android > Speicherorte > Java SDK-Speicherort (JDK)** festgelegt ist:

[![Festlegen des JDK-Speicherorts auf der Registerkarte Android-Speicherorte](update-jdk-images/image6-sml.png)](update-jdk-images/image6.png#lightbox)

-----
