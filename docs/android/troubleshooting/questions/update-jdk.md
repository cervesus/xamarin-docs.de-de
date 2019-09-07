---
title: Wie aktualisiere ich die Java Development Kit-Version (JDK)?
description: In diesem Artikel wird veranschaulicht, wie die Java Development Kit (JDK)-Version unter Windows und Mac aktualisiert wird.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 4b3ac51d-18dd-4034-87b4-4365194e4ece
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 09/07/2018
ms.openlocfilehash: d3f4c602f7e581cab74b61072e248a22eede9a22
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70762015"
---
# <a name="how-do-i-update-the-java-development-kit-jdk-version"></a>Wie aktualisiere ich die Java Development Kit-Version (JDK)?

_In diesem Artikel wird veranschaulicht, wie die Java Development Kit (JDK)-Version unter Windows und Mac aktualisiert wird._

## <a name="overview"></a>Übersicht

Xamarin. Android verwendet das Java Development Kit (JDK) für die Integration in die Android SDK zum Entwickeln von Android-Apps und Ausführen des Android-Designers. Die neuesten Versionen des Android SDK (API 24 und höher) erfordern JDK 8 (1,8). Alternativ können Sie die Vorschauversion von [Microsoft Mobile openjdk](~/android/get-started/installation/openjdk.md)installieren. Das Microsoft Mobile openjdk ersetzt schließlich JDK 8 für die xamarin. Android-Entwicklung.

Informationen zum Aktualisieren des Microsoft Mobile openjdk finden Sie unter [Microsoft Mobile openjdk Preview](~/android/get-started/installation/openjdk.md). Führen Sie die folgenden Schritte aus, um auf JDK 8 zu aktualisieren:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Laden Sie JDK 8 (1,8) von der [Oracle-Website](https://www.oracle.com/technetwork/java/javase/downloads/index.html)herunter:

    ![Screenshot der JDK-Downloadseite auf der Oracle-Website](update-jdk-images/image1.png)

2. Wählen Sie die 64-Bit-Version aus, um das Rendering von [benutzerdefinierten Steuerelementen](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/vs/xamarin.vs_4/xamarin.vs_4.2/index.md#androiddesignercustomcontrols) im xamarin Android-Designer zuzulassen:

    ![Auswählen des Windows x64 JDK-Pakets, das von der JDK-Downloadseite heruntergeladen werden soll](update-jdk-images/image2.png)

3. Führen Sie die exe-Version aus, und installieren Sie die **Entwicklungs Tools**:

    ![Installieren der Entwicklungs Tools im JDK-Installer](update-jdk-images/image3.png)

4. Öffnen Sie Visual Studio, und aktualisieren Sie den **Speicherort des Java Development Kit** , sodass Sie auf das neue JDK unter Extras **> Optionen > xamarin > Android-Einstellungen > Java Development Kit-Speicherort**zeigen:

    [![Pfad Einstellung für das JDK auf der Seite mit den Android-Einstellungen](update-jdk-images/image4-sml.png)](update-jdk-images/image4.png#lightbox)

Stellen Sie sicher, dass Sie Visual Studio nach dem Aktualisieren des Standorts neu starten.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

1. Laden Sie JDK 8 (1,8) von der [Oracle-Website](https://www.oracle.com/technetwork/java/javase/downloads/index.html)herunter:

    ![Screenshot der JDK-Downloadseite auf der Oracle-Website](update-jdk-images/image1.png)

2. Öffnen Sie die DMG-Datei, und führen Sie das pkg-Installationsprogramm aus:

    ![Ausführen des JDK-Installers unter macOS](update-jdk-images/image5.png)

Die neue JDK-Version wird von Mac OS automatisch als Standard festgelegt, indem **/System/Library/Frameworks/JavaVM.Framework/Versions/Current**aktualisiert wird. Sie können dann überprüfen, ob der Speicherort des **Java SDK (JDK)** auf den erwarteten Standardwert von **/usr** unter **Visual Studio für Mac > Einstellungen > Projekte > SDK-Speicherorte > Android > Locations > Java SDK (JDK)-Speicherort**festgelegt ist:

[![Festlegen des JDK-Speicher Orts auf der Registerkarte Android-Speicherorte](update-jdk-images/image6-sml.png)](update-jdk-images/image6.png#lightbox)

-----
