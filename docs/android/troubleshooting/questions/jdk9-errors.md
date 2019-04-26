---
title: Xamarin.Android- und Java Development Kit 9
description: In diesem Artikel wird erläutert, wie Java Development Kit (JDK), 9 oder höher Fehler in Xamarin.Android aufgelöst wird.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 7DCF0985-F77D-4A68-AC54-10C9846E189A
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/29/2018
ms.openlocfilehash: d8c64ff79367d93e282edd9534ffb98f5bb90c93
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61153327"
---
# <a name="xamarinandroid-and-java-development-kit-9-or-later"></a>Xamarin.Android- und Java Development Kit 9 oder höher

_In diesem Artikel wird erläutert, wie Java Development Kit (JDK), 9 oder höher Fehler in Xamarin.Android aufgelöst wird._


## <a name="overview"></a>Übersicht

Xamarin.Android verwendet das Java Development Kit (JDK), um mit dem Android SDK für Android-apps erstellen und Ausführen des Android Designers integrieren. Die neuesten Versionen des Android SDK-API (24 oder höher) erfordern JDK 8 (1.8) oder der Microsoft Mobile OpenJDK-Vorschau. **Da die von Google verfügbaren Android SDK-Tools noch nicht mit JDK 9 kompatibel sind, funktioniert Xamarin.Android nicht mit JDK 9 oder höher.**

## <a name="jdk-errors"></a>JDK-Fehler

Wenn Sie versuchen, eine Xamarin.Android-Projekt mit einer Version des JDK als JDK 8 zu erstellen, erhalten Sie explizite Fehler, der angibt, dass dieser JDK-Version nicht unterstützt wird. Zum Beispiel:

```shell
Building with JDK Version `9.0.4` is not supported. Please install JDK version `1.8.0`. See https://aka.ms/xamarin/jdk9-errors  
```

Um diese Fehler zu beheben, müssen Sie JDK 8 (1.8) installieren, wie unter [wie aktualisiere ich die Java Development Kit (JDK) Version?](~/android/troubleshooting/questions/update-jdk.md).
Alternativ können Sie installieren die [Microsoft Mobile OpenJDK Preview](~/android/get-started/installation/openjdk.md) der Microsoft Mobile OpenJDK JDK 8 schließlich für die Entwicklung von Xamarin.Android ersetzt.


## <a name="checking-the-jdk-version"></a>Überprüfen die JDK-Version

Sie können überprüfen, welche Version von Java finden Sie mithilfe des folgenden Befehls installiert haben (das JDK `bin` Verzeichnis muss sich Ihrem `PATH`):

```shell
java -version
```

Wenn JDK 9 installiert ist, werden Sie sinngemäß die folgende Meldung angezeigt:

```shell
java version "9.0.4"
Java(TM) SE Runtime Environment (build 9.0.4+11)
Java HotSpot(TM) 64-Bit Server VM (build 9.0.4+11, mixed mode)
```

Wenn JDK 9 oder höher installiert ist, müssen Sie das Java JDK 8 (1.8) oder der Microsoft Mobile OpenJDK Preview installieren. Informationen über das JDK 8 installieren, finden Sie unter [wie aktualisiere ich die Java Development Kit (JDK) Version?](~/android/troubleshooting/questions/update-jdk.md). Informationen zum Installieren von Microsoft Mobile OpenJDK, finden Sie unter [Microsoft Mobile OpenJDK Preview](~/android/get-started/installation/openjdk.md).

Beachten Sie, dass Sie nicht verfügen, um eine höhere Version des JDK zu deinstallieren. Allerdings müssen Sie sicherstellen, dass Xamarin JDK 8 anstelle von einer höheren JDK-Version verwendet wird. Klicken Sie in Visual Studio auf **Tools > Optionen > Xamarin > Android-Einstellungen**. Wenn **Java Development Kit-Speicherort** nicht zu einem JDK 8 Speicherort festgelegt ist (z. B. **C:\\Programmdateien\\Java\\Jdk1.8.0_111**), klicken Sie auf **ändern**  und legen ihn auf den Speicherort, auf dem JDK 8 installiert ist. Wechseln Sie in Visual Studio für Mac zu **Voreinstellungen > Projekte > SDK-Speicherorte > Android > Java-SDK (JDK)** , und klicken Sie auf **Durchsuchen** diesen Pfad zu aktualisieren.

## <a name="known-issues-with-jdk-9"></a>Bekannte Probleme mit JDK 9

### <a name="apksigner"></a>apksigner

Es ist ein bekanntes Problem mit Apksigner und JDK 9 in der die `apksigner.bat` Datei ruft der `apksigner.jar` mit `-Djava.ext.dirs` anstelle von `-classpath` JDK 9 erwartet wird. Es wird empfohlen, um JDK 8 (1.8) verwenden. Informationen über das JDK 8 installieren, finden Sie unter [wie aktualisiere ich die Java Development Kit (JDK) Version?](~/android/troubleshooting/questions/update-jdk.md)

Wenn Sie JDK 9 installiert haben, stellen Sie sicher, dass es sich bei der folgende Pfad nicht festgelegt ist, auf Ihre `PATH` Umgebungsvariablen wie zeigen weiterhin auf JDK 9: `C:\ProgramData\Oracle\Java\javapath`. Nach dem entfernen, `java-version` sollte in einer Befehlszeile JDK 8 anzeigen.
