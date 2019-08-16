---
title: Android-Buildfehler – unerwarteter Fehler der linkassemblyaufgabe
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: EB3BE685-CB72-48E3-89D7-C845E76B9FA2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/07/2019
ms.openlocfilehash: 71305dd7287df56036d0298ebfcf8a8cb7c4d3b3
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2019
ms.locfileid: "69528374"
---
# <a name="android-build-error--the-linkassemblies-task-failed-unexpectedly"></a>Android-Buildfehler – unerwarteter Fehler der linkassemblyaufgabe

Möglicherweise wird eine Fehlermeldung `The "LinkAssemblies" task failed unexpectedly` angezeigt, wenn Sie ein xamarin. Android-Projekt mit Formularen verwenden. Dies geschieht, wenn der Linker aktiv ist (in der Regel auf einem *Releasebuild* , um die Größe des App-Pakets zu verringern). Dies liegt daran, dass die Android-Ziele nicht auf das neueste Framework aktualisiert werden. (Weitere Informationen: [Xamarin. Forms für Android-Anforderungen](~/get-started/requirements.md#android))

Beheben Sie dieses Problem, indem Sie sicherstellen, dass Sie über die neuesten unterstützten Android SDK Versionen verfügen, und legen Sie das **Ziel Framework** auf die neueste installierte Plattform fest. Außerdem wird empfohlen, dass Sie die **Android-Zielversion** auf die neueste installierte Plattform und die **Android-Mindestversion** auf API 19 oder höher festlegen. Dies gilt als die unterstützte Konfiguration.

## <a name="setting-in-visual-studio-for-mac"></a>Festlegen in Visual Studio für Mac

1. Klicken Sie mit der rechten Maustaste auf das Android-Projekt, und wählen Sie im Menü **Optionen** aus.
2. Wechseln Sie im Dialogfeld **Projektoptionen** zu **Build > Allgemein**.
3. Festlegen der **Kompilierung mit der Android-Version: (Ziel Framework)**  auf die neueste installierte Plattform.
4. Wechseln Sie im Dialogfeld " **Projektoptionen** " zu **Build > Android-Anwendung**.
5. Legen Sie die **Android-Mindestversion** auf API-Ebene 19 oder höher und die **Android-Zielversion** auf die neueste installierte Plattform fest, die Sie in ausgewählt haben (3).

## <a name="setting-in-visual-studio"></a>Einstellung in Visual Studio

1. Klicken Sie mit der rechten Maustaste auf das Android-Projekt, und wählen Sie im Menü **properies** aus.
2. Wechseln Sie in den Projekteigenschaften zu **Anwendung**.
3. Festlegen der **Kompilierung mit der Android-Version: (Ziel Framework)**  auf die neueste installierte Plattform.
4. Wechseln Sie in den Projekteigenschaften zu **Android-Manifest**.
5. Legen Sie die **Android-Mindestversion** auf API-Ebene 19 oder höher und die **Android-Zielversion** auf die neueste installierte Plattform fest, die Sie in ausgewählt haben (3).

Nachdem Sie diese Einstellungen aktualisiert haben, bereinigen Sie das Projekt, und erstellen Sie es neu, um sicherzustellen, dass Ihre Änderungen übernommen werden.
