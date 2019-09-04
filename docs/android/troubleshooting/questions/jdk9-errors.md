---
title: Xamarin. Android und Java Development Kit 9
description: In diesem Artikel wird erläutert, wie Sie Java Development Kit (JDK) 9 oder höher in xamarin. Android auflösen.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 7DCF0985-F77D-4A68-AC54-10C9846E189A
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/29/2018
ms.openlocfilehash: 3f1f49a5f63d8159644bd2bb247287a91c27023a
ms.sourcegitcommit: c9651cad80c2865bc628349d30e82721c01ddb4a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/03/2019
ms.locfileid: "70225554"
---
# <a name="xamarinandroid-and-java-development-kit-9-or-later"></a>Xamarin. Android und Java Development Kit 9 oder höher

_In diesem Artikel wird erläutert, wie Sie Java Development Kit (JDK) 9 oder höher in xamarin. Android auflösen._


## <a name="overview"></a>Übersicht

Xamarin. Android verwendet das Java Development Kit (JDK) für die Integration in die Android SDK zum Entwickeln von Android-Apps und Ausführen des Android-Designers. Die neuesten Versionen des Android SDK (API 24 und höher) erfordern JDK 8 (1,8) oder die Microsoft Mobile openjdk-Vorschau. **Da die von Google verfügbaren Android SDK Tools noch nicht mit JDK 9 kompatibel sind, funktioniert xamarin. Android nicht mit JDK 9 oder höher.**

## <a name="jdk-errors"></a>JDK-Fehler

Wenn Sie versuchen, ein xamarin. Android-Projekt mit einer Version des JDK später als JDK 8 zu erstellen, erhalten Sie einen expliziten Fehler, der anzeigt, dass diese Version von JDK nicht unterstützt wird. Beispiel:

```shell
Building with JDK Version `9.0.4` is not supported. Please install JDK version `1.8.0`. See https://aka.ms/xamarin/jdk9-errors
```

Um diese Fehler zu beheben, müssen Sie JDK 8 (1,8) installieren, wie unter [Gewusst wie Aktualisieren der Java Development Kit (JDK)-Version](~/android/troubleshooting/questions/update-jdk.md)erläutert.
Alternativ dazu können Sie auch die [Microsoft Mobile openjdk-Vorschau](~/android/get-started/installation/openjdk.md) Version installieren. Microsoft Mobile openjdk ersetzt schließlich JDK 8 für die xamarin. Android-Entwicklung.


## <a name="checking-the-jdk-version"></a>Überprüfen der JDK-Version

Sie können überprüfen, welche Version von Java Sie installiert haben, indem Sie den folgenden Befehl eingeben (das `bin` JDK-Verzeichnis muss `PATH`in Ihrem vorhanden sein):

```shell
java -version
```

Wenn JDK 9 installiert ist, wird eine Meldung wie die folgende angezeigt:

```shell
java version "9.0.4"
Java(TM) SE Runtime Environment (build 9.0.4+11)
Java HotSpot(TM) 64-Bit Server VM (build 9.0.4+11, mixed mode)
```

Wenn JDK 9 oder höher installiert ist, müssen Sie Java JDK 8 (1,8) oder die Microsoft Mobile openjdk Preview installieren. Weitere Informationen zum Installieren von JDK 8 finden Sie unter [Gewusst wie Aktualisieren der Version des Java Development Kit (JDK)](~/android/troubleshooting/questions/update-jdk.md). Informationen zum Installieren des Microsoft Mobile openjdk finden Sie unter [Microsoft Mobile openjdk Preview](~/android/get-started/installation/openjdk.md).

Beachten Sie, dass Sie keine spätere Version des JDK deinstallieren müssen. Sie müssen jedoch sicherstellen, dass xamarin JDK 8 verwendet, anstatt eine spätere JDK-Version zu verwenden. Klicken Sie in Visual Studio auf Extras **> Optionen > xamarin > Android-Einstellungen**. Wenn **der Java Development Kit-Speicherort** nicht auf einen JDK 8-Speicherort festgelegt ist (z. **b. C\\: Program Files\\Java\\JDK 1.8.0 _111**), klicken Sie auf **ändern** , und legen Sie ihn auf den Speicherort fest, an dem JDK 8 Navigieren Sie in Visual Studio für Mac zu **Einstellungen > Projekten > SDK-Speicherorte > Android > Java SDK (JDK)** , und klicken Sie auf **Durchsuchen** , um diesen Pfad zu aktualisieren.

## <a name="known-issues-with-jdk-9"></a>Bekannte Probleme mit JDK 9

### <a name="apksigner"></a>apksigner

Es gibt ein bekanntes Problem mit apksigner und JDK 9, bei dem `apksigner.bat` die Datei den `apksigner.jar` mit `-Djava.ext.dirs` aufruft, `-classpath` anstatt JDK 9 erwartet. Es wird empfohlen, JDK 8 (1,8) zu verwenden. Weitere Informationen zum Installieren von JDK 8 finden Sie unter [Gewusst wie Aktualisieren der Java Development Kit (JDK)-Version?](~/android/troubleshooting/questions/update-jdk.md)

Wenn Sie JDK 9 installiert haben, stellen Sie sicher, dass der folgende Pfad für Ihre `PATH` Umgebungsvariable nicht festgelegt ist, weil Sie weiterhin auf JDK 9 verweist:. `C:\ProgramData\Oracle\Java\javapath` Nach dem Entfernen sollte `java-version` in der Befehlszeile JDK 8 angezeigt werden.
