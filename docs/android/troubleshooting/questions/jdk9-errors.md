---
title: Xamarin.Android und Java Development Kit 9
description: In diesem Artikel wird erläutert, wie Sie Fehler mit Java Development Kit (JDK) 9 oder höher in Xamarin.Android beheben.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 7DCF0985-F77D-4A68-AC54-10C9846E189A
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 08/29/2018
ms.openlocfilehash: 2ea7c9b9f900bc339d183c2f5b317792ebec5232
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2020
ms.locfileid: "73026829"
---
# <a name="xamarinandroid-and-java-development-kit-9-or-later"></a>Xamarin.Android und Java Development Kit 9 oder höher

_In diesem Artikel wird erläutert, wie Sie Fehler mit Java Development Kit (JDK) 9 oder höher in Xamarin.Android beheben._

## <a name="overview"></a>Übersicht

Xamarin.Android verwendet das Java Development Kit (JDK) für die Android SDK-Integration zum Erstellen von Android-Apps und zum Ausführen von Android Designer. Die neuesten Versionen des Android SDK (API 24 und höher) erfordern JDK 8 (1.8) oder die Vorschauversion von Microsoft Mobile OpenJDK. **Da die von Google verfügbaren Android SDK-Tools noch nicht mit JDK 9 kompatibel sind, funktioniert Xamarin.Android nicht mit JDK 9 oder höher.**

## <a name="jdk-errors"></a>JDK-Fehler

Wenn Sie versuchen, ein Xamarin.Android-Projekt mit einer Version des JDK höher als JDK 8 zu erstellen, wird ein expliziter Fehler mit dem Hinweis anzeigt, dass diese JDK-Version nicht unterstützt wird. Zum Beispiel:

```shell
Building with JDK Version `9.0.4` is not supported. Please install JDK version `1.8.0`. See https://aka.ms/xamarin/jdk9-errors
```

Um diese Fehler zu beheben, müssen Sie JDK 8 (1.8), wie in [Wie aktualisiere ich die Java Development Kit-Version (JDK)?](~/android/troubleshooting/questions/update-jdk.md) erläutert, installieren.
Alternativ können Sie die [Vorschauversion von Microsoft Mobile OpenJDK ](~/android/get-started/installation/openjdk.md) installieren. Microsoft Mobile OpenJDK wird irgendwann JDK 8 für die Xamarin.Android-Entwicklung ersetzen.

## <a name="checking-the-jdk-version"></a>Überprüfen der JDK-Version

Sie können feststellen, welche Version von Java installiert ist, indem Sie den folgenden Befehl eingeben (das JDK-`bin` Verzeichnis muss sich in Ihrem `PATH` befinden):

```shell
java -version
```

Wenn JDK 9 installiert ist, wird eine Meldung wie die folgende angezeigt:

```shell
java version "9.0.4"
Java(TM) SE Runtime Environment (build 9.0.4+11)
Java HotSpot(TM) 64-Bit Server VM (build 9.0.4+11, mixed mode)
```

Wenn JDK 9 oder höher installiert ist, müssen Sie Java JDK 8 (1.8) oder die Vorschauversion von Microsoft Mobile OpenJDK installieren. Informationen zum Installieren von JDK 8 finden Sie unter [Wie aktualisiere ich die Java Development Kit-Version (JDK)?](~/android/troubleshooting/questions/update-jdk.md). Informationen zum Installieren von Microsoft Mobile OpenJDK finden Sie unter [Microsoft Mobile OpenJDK (Vorschauversion)](~/android/get-started/installation/openjdk.md).

Beachten Sie, dass Sie eine höhere Version des JDK nicht deinstallieren müssen. Sie müssen jedoch sicherstellen, dass Xamarin JDK 8 und keine höhere JDK-Version verwendet. Klicken Sie in Visual Studio auf **Extras > Optionen > Xamarin > Android-Einstellungen**. Wenn der **Java Development Kit-Speicherort** nicht auf einen JDK 8-Speicherort festgelegt ist (z. B. **C:\\Programme\\Java\\jdk1.8.0_111**), klicken Sie auf **Ändern**, und geben Sie den Speicherort ein, in dem JDK 8 installiert ist. Navigieren Sie in Visual Studio für Mac zu **Einstellungen > Projekte > SDK-Speicherorte > Android > Java SDK (JDK)** und klicken Sie **Durchsuchen**, um diesen Pfad zu aktualisieren.

## <a name="known-issues-with-jdk-9"></a>Bekannte Probleme mit JDK 9

### <a name="apksigner"></a>apksigner

Es gibt ein bekanntes Problem mit apksigner und JDK 9, bei dem die `apksigner.bat`-Datei `apksigner.jar` mit `-Djava.ext.dirs` und nicht wie von JDK 9 erwartet mit `-classpath` aufruft. Es wird empfohlen, JDK 8 (1.8) zu verwenden. Informationen zum Installieren von JDK 8 finden Sie unter [Wie aktualisiere ich die Java Development Kit-Version (JDK)?](~/android/troubleshooting/questions/update-jdk.md).

Wenn Sie JDK 9 installiert haben, darf der folgende Pfad in der `PATH`-Umgebungsvariable nicht festgelegt sein, weil er weiterhin auf JDK 9 verweist: `C:\ProgramData\Oracle\Java\javapath`. Nachdem Sie ihn entfernt haben, sollte `java-version` in der Befehlszeile „JDK 8“ anzeigen.
