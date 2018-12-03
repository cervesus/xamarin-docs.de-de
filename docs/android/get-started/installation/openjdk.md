---
title: OpenJDK-Distribution von Microsoft für die mobile Entwicklung
description: Eine Schrittanleitung für die Konfiguration und Problembehandlung der OpenJDK-Distribution von Microsoft für die mobile Entwicklung.
ms.prod: xamarin
ms.assetid: B5F8503D-F4D1-44CB-8B29-187D1E20C979
ms.technology: xamarin-android
author: vyedin
ms.author: vyedin
ms.date: 07/22/2018
ms.openlocfilehash: aad88ad883dea1e0430a047a71a2c191195efffe
ms.sourcegitcommit: 2f6a5c1abf90fbdb0475fd8a3ce6de3cd7c7d575
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2018
ms.locfileid: "52459901"
---
# <a name="microsofts-mobile-openjdk-distribution"></a>OpenJDK-Distribution von Microsoft für die mobile Entwicklung

_In diesem Leitfaden werden die Schritte für den Wechsel zu einer internen Distribution von OpenJDK beschrieben. Die Verteilung ist für die mobile Entwicklung vorgesehen._

## <a name="overview"></a>Übersicht

Ab Visual Studio 15.9 und Visual Studio für Mac 7.7 verwendet Visual Studio-Tools nicht mehr das Oracle-JDK sondern eine **kompakte OpenJDK-Version, die ausschließlich für die Android-Entwicklung vorgesehen ist**. Dies ist eine erforderliche Migration, da Oracle den Support für die kommerzielle Verteilung des JDK 8 im Jahr 2019 einstellen wird, und das JDK 8 ist eine erforderliche Abhängigkeit für alle Android-Entwicklungen.

Die Vorteile dieses Wechsels sind:

- Es steht Ihnen immer eine OpenJDK-Version zur Verfügung, die für die Android-Entwicklung verwendet werden kann.

- Das Herunterladen des Oracle-JDK 9 oder höher hat keine Auswirkungen auf die Entwicklung.

- Geringere Downloadgröße und weniger Speicherbedarf.

- Keine weiteren Probleme mit Servern und Installationsprogrammen von Drittanbietern.

Wenn Sie früher auf die verbesserte Version umsteigen möchten, stehen Ihnen Builds der Microsoft Mobile OpenJDK-Verteilung zum Testen unter Windows und Mac zur Verfügung. Der Einrichtungsprozess wird im Folgenden beschrieben, und Sie können jederzeit wieder zum Oracle JDK zurückkehren.

## <a name="download"></a>Herunterladen

Die OpenJDK-Verteilung für die mobile Entwicklung wird automatisch installiert, wenn Sie die Android SDK-Pakete im Visual Studio-Installer für Windows auswählen. Die Distribution ist im Update auf Version 15.9 enthalten, wenn das Android SDK ausgewählt ist.

Bei Mac erfolgt die Installation von OpenJDK für die mobile Entwicklung als Teil der Android-Workload für neue Installationen. Benutzer, die Visual Studio für Mac verwenden, werden aufgefordert, das JDK beim Update auf Version 7.7 zu installieren. Die IDE fordert Sie auf, zum neuen JDK zu wechseln und verwendet es ab dem nächsten Neustart.

## <a name="troubleshooting"></a>Problembehandlung

Wenn Probleme mit dem Setup unter Mac oder Windows auftreten, können Sie die folgenden Schritte für ein manuelles Setup durchführen:

Überprüfen Sie, ob OpenJDK am richtigen Speicherort des Computers installiert ist:

- **Mac** &ndash; **$HOME/Library/Developer/Xamarin/jdk/microsoft_dist_openjdk_1.8.0.9**
- **Windows** &ndash; **C:\\Programme\\Android\\jdk\\microsoft_dist_openjdk_1.8.0.9**

Wenn dieser Pfad leer ist, können Sie eines der folgenden Pakete herunterladen:

- **Mac** &ndash; https://dl.xamarin.com/OpenJDK/mac/microsoft-dist-openjdk-1.8.0.9.zip
- **Windows x86** &ndash; https://dl.xamarin.com/OpenJDK/win32/microsoft-dist-openjdk-1.8.0.9.zip
- **Windows x64** &ndash; https://dl.xamarin.com/OpenJDK/win64/microsoft-dist-openjdk-1.8.0.9.zip

Ordnen Sie die IDE dem neuen JDK zu:

- **Mac** &ndash; Klicken Sie auf **Tools > SDK Manager > Speicherorte**, und ändern Sie den Speicherort von **Java SDK (JDK)** in den vollständigen Pfad der OpenJDK-Installation. Im folgenden Beispiel ist dieser Pfad folgendermaßen festgelegt: **$HOME/Library/Developer/Xamarin/jdk/microsoft_dist_openjdk_1.8.0.9**.

![Festlegen des JDK-Pfads für die Microsoft Mobile OpenJDK-Verteilung für Mac](openjdk-images/vsm.png)

- **Windows** &ndash; Klicken Sie auf **Werkzeuge > Optionen > Xamarin > Android-Einstellungen**, und ändern Sie den **Java Development Kit-Speicherort** in den vollständigen Pfad der OpenJDK-Installation. Im folgenden Beispiel ist dieser Pfad folgendermaßen festgelegt **C:\\Programmdateien\\Android\\Jdk\\microsoft_dist_openjdk_1.8.0.9**:

![Festlegen des JDK-Pfads für die Microsoft Mobile OpenJDK-Verteilung für Windows](openjdk-images/vs.png)

## <a name="known-issues"></a>Bekannte Probleme

### <a name="package-openjdkv1regkeyversion1809chipx64-failed-to-install"></a>Paket 'OpenJDKV1.RegKey,version=1.8.0.9,chip=x64' konnte nicht installiert werden

Dieses Problem kann in einigen Unternehmensumgebungen auftreten. OpenJDK ist bereits auf dem Computer vorhanden: Folgen Sie den oben genannten [Schritten zur Problembehandlung](#troubleshooting), um Ihre IDE auf den richtigen Speicherort zu verweisen. Sie können den Status des Problems [hier](https://developercommunity.visualstudio.com/content/problem/382549/packageidopenjdkv1regkeypackageactioninstallreturn.html) anzeigen.

### <a name="unknown-update-type-zip-when-upgrading-visual-studio-for-mac-to-77"></a>„Unbekannter Updatetyp: zip“ beim Aktualisieren von Visual Studio für Mac auf 7.7

Dieses Problem kann beim Versuch auftreten, eine ältere Version von Visual Studio für Mac auf 7.7 zu aktualisieren. Sie können dieses Problem umgehen, indem Sie OpenJDK im Updater deaktivieren, den Rest des Updates installieren und anschließend erneut nach Updates suchen, um das OpenJDK-Paket zu installieren. Sollten Sie weiterhin Probleme haben, können Sie den oben genannten [Schritten zur Problembehandlung](#troubleshooting) folgen, um OpenJDK manuell zu installieren. Bitte melden Sie einen Fehler, und fügen Sie Protokolle hinzu, wenn dieses Problem auftritt.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel haben Sie gelernt, wie Sie Ihre IDE für die Nutzung von OpenJDK von Microsoft für die mobile Entwicklung konfigurieren können und wie Sie eventuell auftretende Probleme behandeln können.
