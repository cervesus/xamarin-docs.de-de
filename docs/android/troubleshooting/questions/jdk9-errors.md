---
title: Xamarin.Android und JDK 9
description: In diesem Artikel wird erläutert, wie Java Development Kit (JDK) 9-Fehler in Xamarin.Android zu beheben.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 7DCF0985-F77D-4A68-AC54-10C9846E189A
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 8857823884447f22b7bc5535f43369671d3285bc
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="xamarinandroid-and-jdk-9"></a>Xamarin.Android und JDK 9

_In diesem Artikel wird erläutert, wie Java Development Kit (JDK) 9-Fehler in Xamarin.Android zu beheben._


## <a name="overview"></a>Übersicht

Xamarin.Android verwendet Java Development Kit (JDK), um die Integration mit dem Android SDK für Android-apps erstellen und Ausführen der Android-Designer. Die neuesten Versionen von Android SDK-API (24 und höher) erfordern JDK 8 (1,8). **Da die von Google verfügbaren Android SDK-Tools noch nicht mit JDK 9 kompatibel sind, funktioniert die Xamarin.Android nicht mit JDK 9.**

## <a name="jdk-9-errors"></a>JDK 9 Fehler

Wenn Sie versuchen, ein Projekt Xamarin.Android mit JDK 9 installiert zu erstellen, erhalten Sie kein expliziten Fehler, der angibt, dass JDK 9 nicht unterstützt wird.

```shell
Building with JDK Version `9.0.4` is not supported. Please install JDK version `1.8.0`. See https://aka.ms/xamarin/jdk9-errors  
```

Um diesen Fehler zu beheben, müssen Sie JDK 8 (1,8) installieren, wie in beschrieben [wie aktualisiere ich die Java Development Kit (JDK) Version?](~/android/troubleshooting/questions/update-jdk.md).


## <a name="checking-the-jdk-version"></a>Überprüfen der JDK-Version

Sie können überprüfen, um festzustellen, welche Version von Java aus, Sie durch Eingabe des folgenden Befehls installiert haben (das JDK `bin` Verzeichnis muss sich Ihrem `PATH`):

```shell
java -version
```

Wenn JDK 9 installiert haben, wird eine Meldung wie die folgende angezeigt:

```shell
java version "9.0.4"
Java(TM) SE Runtime Environment (build 9.0.4+11)
Java HotSpot(TM) 64-Bit Server VM (build 9.0.4+11, mixed mode)
```

Wenn der JDK 9 installiert ist, müssen Sie Java JDK 8 (1,8) installieren. Informationen über das JDK 8 zu installieren, finden Sie unter [wie aktualisiere ich die Java Development Kit (JDK) Version?](~/android/troubleshooting/questions/update-jdk.md)

## <a name="known-issues-with-jdk-9"></a>Bekannte Probleme mit JDK 9

### <a name="apksigner"></a>apksigner

Es ist ein bekanntes Problem mit Apksigner und JDK 9 in der die `apksigner.bat` Datei ruft der `apksigner.jar` mit `-Djava.ext.dirs` anstelle von `-classpath` JDK 9 erwartet wird. Es wird empfohlen, JDK 8 (1,8) verwenden. Informationen über das JDK 8 zu installieren, finden Sie unter [wie aktualisiere ich die Java Development Kit (JDK) Version?](~/android/troubleshooting/questions/update-jdk.md)

Stellen Sie sicher, dass der folgende Pfad für nicht festgelegt ist, nach der Deinstallation des JDK-9, Ihre `PATH` zeigen Umgebungsvariable, da sie immer noch auf JDK 9: `C:\ProgramData\Oracle\Java\javapath`. Nach dem entfernen, `java -version` sollte in einer Befehlszeile JDK 8 anzeigen.
