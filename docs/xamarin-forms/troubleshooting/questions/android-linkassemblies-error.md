---
title: 'Android Buildfehler: Unerwarteter Fehler der LinkAssemblies-Aufgabe'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: EB3BE685-CB72-48E3-89D7-C845E76B9FA2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/25/2017
ms.openlocfilehash: de6e0b66cac688955d27ba2d0165d5a059d36c38
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
ms.locfileid: "30789539"
---
# <a name="android-build-error--the-linkassemblies-task-failed-unexpectedly"></a>Android Buildfehler: Unerwarteter Fehler der LinkAssemblies-Aufgabe

Möglicherweise folgende Fehlermeldung `The "LinkAssemblies" task failed unexpectedly` beim Erstellen eines Projekts Xamarin.Android, verwendet Formulare. Dies geschieht, wenn der Linker aktiv ist (in der Regel auf ein *Version* Build zum Verringern der Größe des app-Pakets); und es tritt auf, da Android Zielen auf die neueste Framework aktualisiert werden. (Weitere Informationen: [Xamarin.Forms für Android Anforderungen](~/xamarin-forms/get-started/installation.md#android))

Die Lösung des Problems ist, um sicherzustellen, dass die neueste unterstützte Android SDK-Versionen haben, und legen Sie die **Zielframework** auf **neueste installierte Plattform**. Es wird empfohlen, dass Sie festlegen, die **Android Zielversion** auf **Zielframeworkversion verwenden** und die **Mindestversion Android** auf API-15 oder höher. Dies ist die unterstützte Konfiguration angesehen.

## <a name="setting-in-visual-studio-for-mac"></a>In Visual Studio festlegen für Mac

1.  Klicken Sie mit der mit der rechten Maustaste auf das Android-Projekt.
2.  Wechseln Sie zu **erstellen > Allgemein > Zielframework**.
3.  Legen Sie die **Zielframework: neueste installierte Plattform**.
4.  Gehen Sie weiterhin in der Projektoptionen zu **erstellen > Android-Anwendung**.
5.  Legen Sie die **mindestens Android-Version** auf API-Ebene 15 oder höher & der **Ziel Android-Version** zu **Automatic - Framework-Zielversion verwenden**.

## <a name="setting-in-visual-studio"></a>In Visual Studio festlegen

1.  Klicken Sie mit der mit der rechten Maustaste auf das Android-Projekt.
2.  Wechseln Sie zu **Anwendung** in den Projektoptionen.
3.  Legen Sie die **Compile using Android Version** & **Ziel Android-Version** Einstellungen **verwenden neueste Version der Plattform** / **verwenden Kompilieren Sie mit der SDK-Version**.
4.  Legen Sie die **mindestens Android Ziel** auf API-15 oder höher festlegen.

Nachdem Sie diese Einstellungen aktualisiert haben, bereinigen, und erstellen Sie das Projekt, um sicherzustellen, dass die Änderungen übernommen werden neu.
