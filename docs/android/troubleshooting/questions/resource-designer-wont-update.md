---
title: Mein Android Resource.designer.cs-Datei wird nicht aktualisiert werden.
ms.topic: article
ms.prod: xamarin
ms.assetid: 3F7376E3-59CC-4722-AEED-BB50E4D952AA
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/19/2017
ms.openlocfilehash: 1b1496f4a6a504c8e991f853c92f937015797aa6
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/12/2018
---
# <a name="my-android-resourcedesignercs-file-will-not-update"></a>Mein Android Resource.designer.cs-Datei wird nicht aktualisiert werden.

> [!NOTE]
> Dieses Problem wurde in Xamarin Studio 5.1.4 und späteren Versionen behoben. Allerdings tritt das Problem in Visual Studio für Mac, bitte der Datei eine [neuen Fehler](~/cross-platform/troubleshooting/questions/howto-file-bug.md) mit Ihrer vollständigen versionsverwaltung-Informationen "und" vollständig "Ausgabeprotokoll erstellen.

Ein Fehler im Xamarin.Studio 5.1 beschädigt zuvor CSPROJ-Dateien durch teilweise oder vollständig löschen den XML-Code in der CSPROJ-Datei. Dies würde dazu führen, dass wichtige Bestandteile der Android-Buildsystem (z. B. Aktualisieren der Android Resource.designer.cs) fehlschlägt. Zum Zeitpunkt der 5.1.4 stabile Version am 15. Juli, dieser Fehler behoben wurde. muss jedoch in vielen Fällen die Projektdatei wie unten beschrieben manuell behoben werden.


## <a name="two-possible-approaches-to-fixing-up-the-project-file"></a>Zwei Möglichkeiten, die Projektdatei Aufbereitung

### <a name="either"></a>Entweder:

1) Erstellen eines brandneuen Xamarin.Android-Anwendungsprojekts, legen Sie die Projekteigenschaften entsprechend das alte Projekt und fügen alle Ressourcen, Quelldateien usw. zurück, in das Projekt.

### <a name="or"></a>ODER

2) Erstellen Sie eine Sicherungskopie der CSPROJ-Datei für das ursprüngliche Projekt, klicken Sie dann in einem Text-Editor zu öffnen Sie, und fügen Sie ihn zurück die fehlenden Elemente aus einer ordnungsgemäß generierten CSPROJ-Datei.

### <a name="if-this-does-not-solve-the-problem"></a>Wenn dies das Problem nicht beheben lässt

Nachdem Ihnen das Experimentieren mit diesen Elementen, bemerken Sie möglicherweise, die nach wieder die Elemente hinzugefügt und das Projekt neu zu erstellen, würde die Resource.designer.cs-Datei zu aktualisieren, müssen dann aber möglicherweise noch schließen und erneut öffnen Sie die Projektmappe zum Abrufen von codevervollständigung erkennen die neuen Typen in Resource.designer.cs enthalten sind. 
