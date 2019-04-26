---
title: 'Android-Buildfehler: Unerwarteter Fehler bei der LinkAssemblies-Aufgabe'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: EB3BE685-CB72-48E3-89D7-C845E76B9FA2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/07/2019
ms.openlocfilehash: f517aaa770fa7b2f1463954638f0afc95168bf65
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61250740"
---
# <a name="android-build-error--the-linkassemblies-task-failed-unexpectedly"></a>Android-Buildfehler: Unerwarteter Fehler bei der LinkAssemblies-Aufgabe

Sie können eine Fehlermeldung angezeigt `The "LinkAssemblies" task failed unexpectedly` Forms Wenn ein Xamarin.Android-Projekt erstellen, die verwendet wird. Dies geschieht, wenn der Linker aktiv ist (in der Regel auf ein *Version* Build zum Verringern der Größe des app-Pakets), und es tritt auf, da der Android-Ziele, die neueste Framework nicht aktualisiert werden. (Weitere Informationen: [Xamarin.Forms für Android Anforderungen](~/get-started/requirements.md#android))

Die Lösung des Problems ist, um sicherzustellen, dass Sie die neuesten unterstützten Android SDK-Versionen, und legen Sie die **Zielframework** auf die zuletzt installierte Plattform. Es wird empfohlen, dass Sie festlegen, die **Android-Zielversion** auf die zuletzt installierte Plattform und die **Android-Mindestversion** auf API-19 oder höher. Dies gilt als die unterstützte Konfiguration.

## <a name="setting-in-visual-studio-for-mac"></a>Die Einstellung in Visual Studio für Mac

1.  Klicken Sie mit der rechten Maustaste auf das Android-Projekt, und wählen **Optionen** im Menü.
2.  In der **Projektoptionen** wechseln Sie zum Dialogfeld **erstellen > Allgemein**.
3.  Legen Sie die **Kompilieren mit der Android-Version: (Zielframework)**  auf die zuletzt installierte Plattform.
4.  In der **Projektoptionen** wechseln Sie zum Dialogfeld **erstellen > Android-Anwendung**.
5.  Legen Sie die **Minimum Android Version** auf API-Ebene 19 oder höher, und die **Target Android Version** auf die zuletzt installierte Plattform Sie ausgewählt, (3 haben).

## <a name="setting-in-visual-studio"></a>Einstellung in Visual Studio

1.  Klicken Sie mit der rechten Maustaste auf das Android-Projekt, und wählen **Eigenschaften** im Menü.
2.  Wechseln Sie in den Projekteigenschaften zu **Anwendung**.
3.  Legen Sie die **Kompilieren mit der Android-Version: (Zielframework)**  auf die zuletzt installierte Plattform.
4.  Wechseln Sie in den Projekteigenschaften zu **Android-Manifest**.
5.  Legen Sie die **Minimum Android Version** auf API-Ebene 19 oder höher, und die **Target Android Version** auf die zuletzt installierte Plattform Sie ausgewählt, (3 haben).

Nachdem Sie diese Einstellungen aktualisiert haben, bereinigen Sie Sie und Neuerstellen Sie des Projekts, um sicherzustellen, dass die Änderungen übernommen werden.
