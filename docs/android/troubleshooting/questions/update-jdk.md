---
title: Wie aktualisieren kann ich die Java Development Kit (JDK) Version?
description: In diesem Artikel wird veranschaulicht, wie die Java Development Kit (JDK) Version auf Windows- und Mac aktualisieren
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 4b3ac51d-18dd-4034-87b4-4365194e4ece
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/18/2018
ms.openlocfilehash: 979bf4572e0e0865c2254c3e1c2f707c8eecae8d
ms.sourcegitcommit: 57f9a9ba2f199697cb75e7be67f1a372c35a861b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2018
ms.locfileid: "36269659"
---
# <a name="how-do-i-update-the-java-development-kit-jdk-version"></a>Wie aktualisieren kann ich die Java Development Kit (JDK) Version?

_In diesem Artikel wird veranschaulicht, wie die Java Development Kit (JDK) Version auf Windows- und Mac aktualisieren_

## <a name="overview"></a>Übersicht

Xamarin.Android verwendet Java Development Kit (JDK), um die Integration mit dem Android SDK für Android-apps erstellen und Ausführen der Android-Designer. Die neuesten Versionen von Android SDK-API (24 und höher) erfordern JDK 8 (1,8). Wenn Sie noch nicht auf JDK 8 aktualisiert haben, führen Sie diese Schritte zum Installieren und aktivieren Sie ihn:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1.  Laden Sie das JDK 8 (1,8) aus der [Oracle-Website](http://www.oracle.com/technetwork/java/javase/downloads/index.html):

    ![Screenshot des JDK-Downloadseite auf der Oracle-website](update-jdk-images/image1.png)

2.  Wählen Sie die 64-Bit-Version ermöglicht die Darstellung von [benutzerdefinierte Steuerelemente](https://developer.xamarin.com/releases/vs/xamarin.vs_4/xamarin.vs_4.2/#androiddesignercustomcontrols) im Xamarin Android-Designer:

    ![Das Windows X64 JDK-Paket zum Herunterladen von der JDK-Download-Seite auswählen](update-jdk-images/image2.png)

3.  Führen Sie die .exe und Installieren der **Entwicklungstools**:

    ![Installieren der Entwicklungstools JDK-Installer](update-jdk-images/image3.png)

4.  Öffnen Sie Visual Studio, und Aktualisieren der **Java Development Kit Speicherort** , zeigen Sie auf das neue JDK unter **Tools > Optionen > Xamarin > Android-Einstellungen > Java Development Kit-Speicherort > Änderung**:

    [![Path-Einstellung für das JDK Android Einstellung für die IDE-Optionen auf der Seite](update-jdk-images/image4-sml.png)](update-jdk-images/image4.png#lightbox)

Achten Sie darauf, dass Sie Visual Studio neu starten nach dem Aktualisieren des Speicherorts.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

1.  Laden Sie das JDK 8 (1,8) aus der [Oracle-Website](http://www.oracle.com/technetwork/java/javase/downloads/index.html):

    ![Screenshot des JDK-Downloadseite auf der Oracle-website](update-jdk-images/image1.png)

2.  Öffnen Sie die DMG-Datei, und führen Sie das Installationsprogramm .pkg:

    ![Ausführen des JDK auf Mac OS](update-jdk-images/image5.png)

Mac OS wird automatisch die neue Version des JDK als Standard festlegen, indem Sie aktualisieren **/System/Library/Frameworks/JavaVM.framework/Versions/Current**. Sie können dann überprüfen, die der **Java SDK (JDK)** Speicherort auf den erwarteten Standardwert festgelegt ist **/usr** unter **Visual Studio für Mac > Voreinstellungen > Projekte > SDK-Verzeichnissen > Android > Java SDK (JDK) > Speicherort**:

![Festlegen der JDK-Speicherorts auf der Seite Speicherort des Android SDK](update-jdk-images/image6.png)

-----

