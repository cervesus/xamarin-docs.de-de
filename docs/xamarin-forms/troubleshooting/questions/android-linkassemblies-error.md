---
title: Android-Buildfehler – unerwarteter Fehler der linkassemblyaufgabe
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: EB3BE685-CB72-48E3-89D7-C845E76B9FA2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/07/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 9c0403203345ed78fae9f30c623c3e501f24ea0e
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84135901"
---
# <a name="android-build-error--the-linkassemblies-task-failed-unexpectedly"></a>Android-Buildfehler – unerwarteter Fehler der linkassemblyaufgabe

Möglicherweise wird eine Fehlermeldung angezeigt `The "LinkAssemblies" task failed unexpectedly` , wenn Sie ein xamarin. Android-Projekt mit Formularen verwenden. Dies geschieht, wenn der Linker aktiv ist (in der Regel auf einem *Releasebuild* , um die Größe des App-Pakets zu verringern). Dies liegt daran, dass die Android-Ziele nicht auf das neueste Framework aktualisiert werden. (Weitere Informationen: [ Xamarin.Forms Unterstützte Plattformen](~/get-started/supported-platforms.md#android-platform-support))

Beheben Sie dieses Problem, indem Sie sicherstellen, dass Sie über die neuesten unterstützten Android SDK Versionen verfügen, und legen Sie das **Ziel Framework** auf die neueste installierte Plattform fest. Außerdem wird empfohlen, dass Sie die **Android-Zielversion** auf die neueste installierte Plattform und die **Android-Mindestversion** auf API 19 oder höher festlegen. Dies gilt als die unterstützte Konfiguration.

## <a name="setting-in-visual-studio-for-mac"></a>Festlegen in Visual Studio für Mac

1. Klicken Sie mit der rechten Maustaste auf das Android-Projekt, und wählen Sie im Menü **Optionen** aus.
2. Wechseln Sie im Dialogfeld **Projektoptionen** zu **Build > allgemein**.
3. Legen **Sie die Kompilierung mit Android-Version: (Ziel Framework)** auf die neueste installierte Plattform fest.
4. Wechseln Sie im Dialogfeld " **Projektoptionen** " zu **Build > Android-Anwendung**.
5. Legen Sie die **Android-Mindestversion** auf API-Ebene 19 oder höher und die **Android-Zielversion** auf die neueste installierte Plattform fest, die Sie in ausgewählt haben (3).

## <a name="setting-in-visual-studio"></a>Einstellung in Visual Studio

1. Klicken Sie mit der rechten Maustaste auf das Android-Projekt, und wählen Sie im Menü **properies** aus.
2. Wechseln Sie in den Projekteigenschaften zu **Anwendung**.
3. Legen **Sie die Kompilierung mit Android-Version: (Ziel Framework)** auf die neueste installierte Plattform fest.
4. Wechseln Sie in den Projekteigenschaften zu **Android-Manifest**.
5. Legen Sie die **Android-Mindestversion** auf API-Ebene 19 oder höher und die **Android-Zielversion** auf die neueste installierte Plattform fest, die Sie in ausgewählt haben (3).

Nachdem Sie diese Einstellungen aktualisiert haben, bereinigen Sie das Projekt, und erstellen Sie es neu, um sicherzustellen, dass Ihre Änderungen übernommen werden.
