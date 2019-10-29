---
title: Meine Android-Datei „Resource.designer.cs“ wird nicht aktualisiert
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 3F7376E3-59CC-4722-AEED-BB50E4D952AA
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/19/2017
ms.openlocfilehash: 4683bbaa5aa48c7b5de5fb9a87a4cd3fbc0aeada
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73019514"
---
# <a name="my-android-resourcedesignercs-file-will-not-update"></a>Meine Android-Datei „Resource.designer.cs“ wird nicht aktualisiert

> [!NOTE]
> Dieses Problem wurde in Xamarin Studio 5.1.4 und höheren Versionen behoben. Wenn das Problem jedoch in Visual Studio für Mac auftritt, melden Sie einen [neuen Fehler](~/cross-platform/troubleshooting/questions/howto-file-bug.md) mit den vollständigen Versionsinformationen und der vollständigen Buildprotokollierung.

Ein Fehler in xamarin. Studio 5,1 zuvor beschädigte csproj-Dateien, indem der XML-Code in der CSPROJ-Datei teilweise oder vollständig gelöscht wurde. Dies würde dazu führen, dass wichtige Teile des Android-Buildsystems (z. b. das Aktualisieren des Android-Resource.Designer.cs) fehlschlagen. Ab dem 15. Juli wurde dieser Fehler korrigiert. in vielen Fällen muss die Projektdatei jedoch manuell repariert werden, wie unten beschrieben.

## <a name="two-possible-approaches-to-fixing-up-the-project-file"></a>Zwei mögliche Ansätze zum Beheben der Projektdatei

**Beides**

1. Erstellen Sie ein neues xamarin. Android-Anwendungsprojekt, legen Sie alle Projekteigenschaften so fest, dass Sie mit dem alten Projekt identisch sind, und fügen Sie alle Ihre Ressourcen, Quelldateien usw. wieder in das Projekt ein.

   **ODER**

2. Erstellen Sie eine Sicherungskopie der CSPROJ-Datei Ihres ursprünglichen Projekts, öffnen Sie Sie in einem Text-Editor, und fügen Sie die fehlenden Elemente aus einer ordnungsgemäß generierten CSPROJ-Datei zurück.

### <a name="if-this-does-not-solve-the-problem"></a>Wenn das Problem hierdurch nicht behoben wird

Nachdem Sie diese Elemente ausprobiert haben, werden Sie möglicherweise bemerken, dass die Resource.Designer.cs-Datei nach dem Hinzufügen der Elemente und der Neuerstellung des Projekts aktualisiert wird. Sie müssen die Projekt Mappe jedoch trotzdem schließen und erneut öffnen, um die Codevervollständigung zu erkennen. die neuen Typen, die in Resource.Designer.cs enthalten sind. 
