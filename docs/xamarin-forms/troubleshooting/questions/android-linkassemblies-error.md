---
title: 'Android-Buildfehler: Unerwarteter Fehler bei der LinkAssemblies-Aufgabe'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: EB3BE685-CB72-48E3-89D7-C845E76B9FA2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/25/2017
ms.openlocfilehash: 7360ee9064049bcebfd88f0cd36b5938d5337be3
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50105284"
---
# <a name="android-build-error--the-linkassemblies-task-failed-unexpectedly"></a>Android-Buildfehler: Unerwarteter Fehler bei der LinkAssemblies-Aufgabe

Sie können eine Fehlermeldung angezeigt `The "LinkAssemblies" task failed unexpectedly` Forms Wenn ein Xamarin.Android-Projekt erstellen, die verwendet wird. Dies geschieht, wenn der Linker aktiv ist (in der Regel auf ein *Version* Build zum Verringern der Größe des app-Pakets), und es tritt auf, da der Android-Ziele, die neueste Framework nicht aktualisiert werden. (Weitere Informationen: [Xamarin.Forms für Android Anforderungen](~/xamarin-forms/get-started/installation.md#android))

Die Lösung des Problems ist, um sicherzustellen, dass Sie die neuesten unterstützten Android SDK-Versionen, und legen Sie die **Zielframework** zu **zuletzt installierte Plattform verwenden**. Es wird empfohlen, dass Sie festlegen, die **Android-Zielversion** zu **Zielframeworkversion verwenden** und **Android-Mindestversion** auf API 15 oder höher. Dies gilt als die unterstützte Konfiguration.

## <a name="setting-in-visual-studio-for-mac"></a>Die Einstellung in Visual Studio für Mac

1.  Klicken Sie mit der rechten Maustaste auf das Android-Projekt.
2.  Wechseln Sie zu **erstellen > Allgemein > Zielframework**.
3.  Legen Sie die **Zielframework: zuletzt installierte Plattform verwenden**.
4.  Wechseln Sie weiterhin in den Projektoptionen zu **erstellen > Android-Anwendung**.
5.  Legen Sie die **Minimum Android Version** auf API-Ebene 15 oder höher und dem **Target Android Version** zu **automatisch – Zielframeworkversion verwenden**.

## <a name="setting-in-visual-studio"></a>Einstellung in Visual Studio

1.  Klicken Sie mit der rechten Maustaste auf das Android-Projekt.
2.  Wechseln Sie zu **Anwendung** in den Projektoptionen.
3.  Legen Sie die **Kompilieren mit der Android-Version** & **Target Android Version** Einstellungen **neueste Plattform verwenden** / **verwenden Kompilieren mit der SDK-Version**.
4.  Legen Sie die **Android-Mindestversion** festlegen auf API-19 oder höher.

Nachdem Sie diese Einstellungen aktualisiert haben, bereinigen Sie Sie und Neuerstellen Sie des Projekts, um sicherzustellen, dass die Änderungen übernommen werden.
