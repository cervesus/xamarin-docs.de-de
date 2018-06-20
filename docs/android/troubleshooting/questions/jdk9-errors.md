---
title: Xamarin.Android und Java Development Kit 9
description: In diesem Artikel wird erläutert, wie Java Development Kit (JDK) 9-Fehler in Xamarin.Android zu beheben.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 7DCF0985-F77D-4A68-AC54-10C9846E189A
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/18/2018
ms.openlocfilehash: 529062f820cd682dc6a9c22f706dbceecef1c836
ms.sourcegitcommit: 57f9a9ba2f199697cb75e7be67f1a372c35a861b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2018
ms.locfileid: "36269672"
---
# <a name="xamarinandroid-and-java-development-kit-9"></a>Xamarin.Android und Java Development Kit 9

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

Beachten Sie, dass Sie keine JDK 9 deinstallieren. Allerdings müssen Sie sicherstellen, dass Xamarin JDK-9, sondern JDK 8 verwendet. Klicken Sie in Visual Studio auf **Extras > Optionen > Xamarin > Android-Einstellungen**. Wenn **Java Development Kit Speicherort** nicht an einen Speicherort für JDK 8 festgelegt ist (z. B. **"c:"\\Programmdateien\\Java\\Jdk1.8.0_111**), klicken Sie auf **ändern**  und legen Sie es auf den Speicherort, auf dem JDK 8 installiert ist. Wechseln Sie in Visual Studio für Mac zu **Voreinstellungen > Projekte > SDK-Verzeichnissen > Android > Java SDK (JDK)** , und klicken Sie auf **Durchsuchen** diesen Pfad zu aktualisieren.

## <a name="known-issues-with-jdk-9"></a>Bekannte Probleme mit JDK 9

### <a name="apksigner"></a>apksigner

Es ist ein bekanntes Problem mit Apksigner und JDK 9 in der die `apksigner.bat` Datei ruft der `apksigner.jar` mit `-Djava.ext.dirs` anstelle von `-classpath` JDK 9 erwartet wird. Es wird empfohlen, JDK 8 (1,8) verwenden. Informationen über das JDK 8 zu installieren, finden Sie unter [wie aktualisiere ich die Java Development Kit (JDK) Version?](~/android/troubleshooting/questions/update-jdk.md)

Wenn Sie JDK 9 installiert haben, stellen Sie sicher, dass der folgende Pfad für nicht festgelegt ist Ihre `PATH` zeigen Umgebungsvariable, da sie immer noch auf JDK 9: `C:\ProgramData\Oracle\Java\javapath`. Nach dem entfernen, `java-version` sollte in einer Befehlszeile JDK 8 anzeigen.
