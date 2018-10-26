---
title: Meine Datei "Android Resource.designer.cs" wird nicht aktualisiert werden.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 3F7376E3-59CC-4722-AEED-BB50E4D952AA
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/19/2017
ms.openlocfilehash: ba3c2b07e7f35bf9fd84d10b74d034a02ca6a73d
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50106415"
---
# <a name="my-android-resourcedesignercs-file-will-not-update"></a>Meine Datei "Android Resource.designer.cs" wird nicht aktualisiert werden.

> [!NOTE]
> In Xamarin Studio 5.1.4 und höheren Versionen hat dieses Problem behoben wurde. Jedoch, wenn das Problem in Visual Studio für Mac auftritt, melden Sie bitte eine [neuer Fehler](~/cross-platform/troubleshooting/questions/howto-file-bug.md) erstellen Sie mit der Ihre vollständige versionsverwaltung Informationen "und" vollständig "Protokollausgabe.

Ein Fehler in xamarin.Studio die Datei 5.1 beschädigt zuvor CSPROJ-Dateien teilweise oder vollständig löschen Sie den XML-Code in die CSPROJ-Datei. Dies würde dazu führen, dass wichtige Teile des Android-Buildsystem (z. B. Aktualisieren der Android Resource.designer.cs) fehlschlägt. Zum Zeitpunkt der 5.1.4 stabile Version am 15. Juli, dieses Problem wurde behoben. aber in vielen Fällen muss die Datei wie unten beschrieben manuell behoben werden.


## <a name="two-possible-approaches-to-fixing-up-the-project-file"></a>Zwei mögliche Ansätze kennen, um die Projektdatei wird korrigiert.

### <a name="either"></a>Entweder:

1) Ein völlig neues Projekt für die Xamarin.Android-Anwendung erstellen, legen die Projekteigenschaften entsprechend das alte Projekt, und fügen Sie alle Ihre Ressourcen hinzu, Quelldateien usw., die in das Projekt zurück.

### <a name="or"></a>ODER

2) Erstellen Sie eine Sicherungskopie der CSPROJ-Datei für das ursprüngliche Projekt in einem Text-Editor öffnen Sie, und anschließend wieder in die fehlenden Elemente aus einer sauber generierten CSPROJ-Datei hinzufügen.

### <a name="if-this-does-not-solve-the-problem"></a>Wenn dies das Problem nicht behoben wird

Nach dem Experimentieren mit diesen Elementen, können Sie bitte beachten Sie, nachdem die Elemente wieder hinzugefügt, und das Projekt neu erstellt, die Datei "Resource.designer.cs" wird aktualisiert, müssen dann aber möglicherweise immer noch zu schließen und erneut öffnen die Projektmappe rufen Sie die Vervollständigung von Code zur Erkennung von die neuen Typen in Resource.designer.cs enthalten sind. 
