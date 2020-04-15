---
title: Installieren von Xamarin.Android als System-App
description: In diesem Leitfaden werden die Unterschiede zwischen einer System-App und einer Benutzer-App erläutert und wie eine Xamarin.Android-Anwendung als Systemanwendung installiert werden kann. Dieser Leitfaden ist für Autoren von benutzerdefinierten Android ROM-Images vorgesehen. Es wird nicht erläutert, wie ein benutzerdefiniertes ROM-Image erstellt wird.
ms.prod: xamarin
ms.assetid: 0113143B-7D8D-4C4C-B2F5-B966A2E7CE1F
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/15/2018
ms.openlocfilehash: 72cddde86708b5573dc578165354d137c4dc35b6
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2020
ms.locfileid: "76723899"
---
# <a name="installing-xamarinandroid-as-a-system-app"></a>Installieren von Xamarin.Android als System-App

_In diesem Leitfaden werden die Unterschiede zwischen einer System-App und einer Benutzer-App erläutert und wie eine Xamarin.Android-Anwendung als Systemanwendung installiert werden kann. Dieser Leitfaden ist für Autoren von benutzerdefinierten Android ROM-Images vorgesehen. Es wird nicht erläutert, wie ein benutzerdefiniertes ROM-Image erstellt wird._

## <a name="system-app"></a>System-App

Autoren benutzerdefinierter Android ROM-Images oder Hersteller von Android-Geräten möchten vielleicht eine Xamarin.Android-Anwendung als _System-App_ einschließen, wenn Sie einen ROM-Speicher oder ein Gerät verteilen. Bei einer System-App handelt es sich um eine App, die für das Funktionieren des Geräts bzw. für die Bereitstellung von Funktionen vorgesehen ist, die der Autor von benutzerdefinierten ROMs immer zur Verfügung haben möchte.

System-Apps werden im Ordner **/system/app/** installiert (ein schreibgeschütztes Verzeichnis im Dateisystem) und können weder vom Benutzer gelöscht noch verschoben werden, es sei denn, der Benutzer verfügt über Stammzugriff. Im Gegensatz dazu wird eine Anwendung, die vom Benutzer installiert wird (in der Regel über Google Play oder durch Querladen der App) als _Benutzer-App_ bezeichnet. Benutzer-Apps können vom Benutzer gelöscht und in vielen Fällen an einen anderen Speicherort auf dem Gerät verschoben werden (beispielsweise in einen externen Speicher).

System-Apps verhalten sich genauso wie Benutzer-Apps, unterscheiden sich jedoch in folgenden wichtigen Punkten:

- Für System-Apps kann wie bei normalen _Benutzer-Apps_ auch ein Upgrade durchgeführt werden. Da jedoch immer eine Kopie der App unter **/system/app/** vorhanden ist, ist es immer möglich, ein Rollback auf die ursprüngliche Version der Anwendung durchzuführen.

- System-Apps können bestimmte, nur auf das System bezogene Berechtigungen erhalten, die für eine Benutzer-App nicht zur Verfügung stehen. Ein Beispiel für eine „Nur-System“-Berechtigung ist [`BLUETOOTH_PRIVILEGED`](https://developer.android.com/reference/android/Manifest.permission.html#BLUETOOTH_PRIVILEGED), wodurch sich Anwendungen mit Bluetooth-Geräten ohne zusätzliche Benutzerinteraktion koppeln können.

Es ist möglich, eine Xamarin.Android-App als Systemanwendung zu verteilen. Zusätzlich zur Bereitstellung eines Android-Anwendungspakets auf dem benutzerdefinierten ROM gibt es zwei freigegebene Bibliotheken, **libmonodroid.so** und **libmonosgen-2.0.so**, die manuell vom APK zum Dateisystem des ROM-Images kopiert werden müssen. In diesem Leitfaden werden die Schritte erläutert.

## <a name="restrictions"></a>Beschränkungen

Dieser Leitfaden ist für Autoren von benutzerdefinierten Android ROM-Images vorgesehen. Es wird nicht erläutert, wie ein benutzerdefiniertes ROM-Image erstellt wird.

In diesem Leitfaden werden Kenntnisse zum [Packen eines Release-APK für eine Xamarin.Android-Anwendung](~/android/deploy-test/publishing/index.md) und grundlegendes Verständnis der [CPU-Architektur](~/android/app-fundamentals/cpu-architectures.md) für Android-Anwendungen vorausgesetzt.

## <a name="install-a-xamarinandroid-app-as-a-system-app"></a>Installieren einer Xamarin.Android-App als System-App

Die folgenden Schritte beschreiben, wie eine Xamarin.Android-App als System-App installiert wird.

1. **Packen eines Release-APK der Xamarin.Android-App:** Dieser Schritt wird ausführlicher im Leitfaden [Veröffentlichen einer Anwendung](~/android/deploy-test/publishing/index.md) beschrieben.

2. **Extrahieren freigegebener Bibliotheken aus dem APK:** Verwenden eines ZIP-Hilfsprogramms, Öffnen der APK-Datei und Untersuchen der Inhalte des **/lib/** -Ordners. Dieser Ordner enthält dann ein Unterverzeichnis für jede _Anwendungsbinärdatei-Schnittstelle_ (ABI), die von der Anwendung unterstützt wird. Die Inhalte dieses Ordners enthalten alle freigegebenen Bibliotheken, die von der App auf dieser bestimmten ABI benötigt werden:

    ![Screenshot von SO-Dateien im armeabi-v7a-Ordner der „taskypro.zip“-Datei](install-system-app-images/install-system-app-01.png)

   Im obigen Screenshot sehen Sie nur eine unterstützte ABI, (**armeabi-v7a**), die die zwei **SO**-Dateien enthält, die von der App benötigt werden. Beachten Sie, dass nur die ABI-Dateien, die für das Gerät oder die Zielarchitektur des Geräte-ROM geeignet sind, extrahiert werden müssen. Kopieren Sie also keine **SO**-Dateien aus dem **x86**-Ordner auf ein **armeabi-v7a**-Gerät oder -ROM.

3. **Kopieren der SO-Dateien nach /system/lib:** Kopieren Sie **SO**-Dateien, die aus dem Android-Anwendungspaket im vorherigen Schritt kopiert wurden, in den **/system/lib/** -Ordner auf dem benutzerdefinierten ROM.

4. **Kopieren der APK-Datei nach /system/app:** Der letzte Schritt ist, die APK-Datei in den **/system/app**-Ordner auf dem ROM zu kopieren.

## <a name="summary"></a>Zusammenfassung

In diesem Leitfaden wurden die Unterschiede zwischen einer _System-App_ und einer _Benutzer-App_ erläutert, und es wurde erklärt, wie eine Xamarin.Android-Anwendung als System-App installiert werden kann.

## <a name="related-links"></a>Verwandte Links

- [Veröffentlichen einer Anwendung](~/android/deploy-test/publishing/index.md)
- [CPU-Architekturen](~/android/app-fundamentals/cpu-architectures.md)
- [BLUETOOTH_PRIVILEGED](https://developer.android.com/reference/android/Manifest.permission.html#BLUETOOTH_PRIVILEGED)
- [ABI Management (ABI-Verwaltung)](https://developer.android.com/ndk/guides/abis)
