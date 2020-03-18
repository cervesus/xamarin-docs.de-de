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
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2020
ms.locfileid: "73019514"
---
# <a name="my-android-resourcedesignercs-file-will-not-update"></a>Meine Android-Datei „Resource.designer.cs“ wird nicht aktualisiert

> [!NOTE]
> Dieses Problem wurde in Xamarin Studio 5.1.4 und höheren Versionen behoben. Wenn das Problem jedoch in Visual Studio für Mac auftreten sollte, erstellen Sie bitte einen [neuen Fehler](~/cross-platform/troubleshooting/questions/howto-file-bug.md), und geben Sie alle Informationen zur Version und zur Buildprotokollausgabe an.

Ein Fehler in Xamarin.Studio 5.1 hat in der Vergangenheit CSPROJ-Dateien beschädigt, indem XML-Code teilweise oder vollständig aus der Datei gelöscht wurde. Dies führte dazu, dass wichtige Komponenten des Android-Buildsystems (wie die Aktualisierung der Resource.designer.cs-Datei) Fehler beinhalteten. Ab dem Release von Version 5.1.4 vom 15. Juli tritt dieser Fehler nicht mehr auf. Allerdings muss die Projektdatei manuell wie nachfolgend beschrieben repariert werden.

## <a name="two-possible-approaches-to-fixing-up-the-project-file"></a>Es gibt zwei Ansätze zum Reparieren der Projektdatei

**Entweder**

1. erstellen Sie ein ganz neues Xamarin.Android-Anwendungsprojekt, passen alle Projekteigenschaften an das alte Projekt an und fügen alle Ressourcen, Quelldateien etc. wieder in das Projekt ein,

   **ODER**

2. Sie erstellen eine Sicherungskopie der ursprünglichen CSPROJ-Projektdatei, öffnen diese in einem Text-Editor und fügen die fehlenden Elemente aus der fehlerfreien CSPROJ-Datei ein.

### <a name="if-this-does-not-solve-the-problem"></a>Gehen Sie wie folgt vor, wenn das Problem dadurch nicht behoben wird:

Wenn Sie mit diesen Elementen experimentiert haben, kann es sein, dass Sie nach dem Hinzufügen der Elemente zum neu erstellten Projekt feststellen, dass die Resource.designer.cs-Datei aktualisiert wird. Dann müssen Sie aber möglicherweise die Projektmappe schließen und wieder öffnen, damit die neuen Elemente, die in der Resource.designer.cs-Datei enthalten sind, von der Codevervollständigung erkannt werden. 
