---
title: OpenJDK-Distribution von Microsoft für die mobile Entwicklung
description: Eine Schrittanleitung für die Konfiguration und Problembehandlung der OpenJDK-Distribution von Microsoft für die mobile Entwicklung.
ms.prod: xamarin
ms.assetid: B5F8503D-F4D1-44CB-8B29-187D1E20C979
ms.technology: xamarin-android
author: vyedin
ms.author: vyedin
ms.date: 07/22/2018
ms.openlocfilehash: a24edbc10d529878092b474df7f186d14049d5e0
ms.sourcegitcommit: f8e22a3b0642179bf44a312e9a2fac0fbad8683c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2019
ms.locfileid: "57239112"
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

Die OpenJDK-Verteilung für die mobile Entwicklung wird automatisch installiert, wenn Sie die Android SDK-Pakete im Visual Studio-Installer für Windows auswählen.

Bei Mac erfolgt die Installation von OpenJDK für die mobile Entwicklung als Teil der Android-Workload für neue Installationen. Benutzer, die Visual Studio für Mac verwenden, werden aufgefordert, das OpenJDK beim Update zu installieren. Die IDE fordert Sie auf, zum neuen JDK zu wechseln und verwendet es ab dem nächsten Neustart.

## <a name="troubleshooting"></a>Problembehandlung

Wenn Probleme mit dem Setup unter Mac oder Windows auftreten, können Sie die folgenden Schritte für ein manuelles Setup durchführen:

Überprüfen Sie, ob OpenJDK am richtigen Speicherort des Computers installiert ist:

- **Mac** &ndash; **$HOME/Library/Developer/Xamarin/jdk/microsoft_dist_openjdk_1.8.0.x**
- **Windows** &ndash; **C:\\Programme\\Android\\jdk\\microsoft_dist_openjdk_1.8.0.x**

Ordnen Sie die IDE dem neuen JDK zu:

- **Mac** &ndash; Klicken Sie auf **Tools > SDK Manager > Speicherorte**, und ändern Sie den Speicherort von **Java SDK (JDK)** in den vollständigen Pfad der OpenJDK-Installation. Im folgenden Beispiel ist dieser Pfad auf **$HOME/Library/Developer/Xamarin/jdk/microsoft_dist_openjdk_1.8.0.9** festgelegt. Möglicherweise verfügen Sie aber über eine neuere Version.

![Festlegen des JDK-Pfads für die Microsoft Mobile OpenJDK-Verteilung für Mac](openjdk-images/vsm.png)

- **Windows** &ndash; Klicken Sie auf **Werkzeuge > Optionen > Xamarin > Android-Einstellungen**, und ändern Sie den **Java Development Kit-Speicherort** in den vollständigen Pfad der OpenJDK-Installation. Im folgenden Beispiel ist dieser Pfad auf **C:\\Programmdateien\\Android\\Jdk\\microsoft_dist_openjdk_1.8.0.9** festgelegt. Möglicherweise verfügen Sie aber über eine neuere Version:

![Festlegen des JDK-Pfads für die Microsoft Mobile OpenJDK-Verteilung für Windows](openjdk-images/vs.png)

## <a name="known-issues"></a>Bekannte Probleme

### <a name="package-openjdkv1regkeyversion18025chipx64-failed-to-install"></a>Package 'OpenJDKV1.RegKey,version=1.8.0.25,chip=x64' failed to install (Das Paket „OpenJDKV1.RegKey,version=1.8.0.25,chip=x64“ konnte nicht installiert werden.)

Dieses Problem kann in einigen Unternehmensumgebungen auftreten. OpenJDK ist bereits auf dem Computer vorhanden: Folgen Sie den oben genannten [Schritten zur Problembehandlung](#troubleshooting), um Ihre IDE auf den richtigen Speicherort zu verweisen. Sie können den Status des Problems [hier](https://developercommunity.visualstudio.com/content/problem/382549/packageidopenjdkv1regkeypackageactioninstallreturn.html) anzeigen.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel haben Sie gelernt, wie Sie Ihre IDE für die Nutzung von OpenJDK von Microsoft für die mobile Entwicklung konfigurieren können und wie Sie eventuell auftretende Probleme behandeln können.
