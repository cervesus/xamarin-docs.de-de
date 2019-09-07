---
title: Gewusst wie Auflösen eines PathTooLongException-Fehlers?
description: In diesem Artikel wird erläutert, wie Sie eine PathTooLongException auflösen, die beim Entwickeln einer APP auftreten kann.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 60EE1C8D-BE44-4612-B3B5-70316D71B1EA
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 05/29/2018
ms.openlocfilehash: 915f557db7955dc7b8b9f1bc5e014a683740052b
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70760815"
---
# <a name="how-do-i-resolve-a-pathtoolongexception-error"></a>Gewusst wie Auflösen eines PathTooLongException-Fehlers?

## <a name="cause"></a>Ursache

Generierte Pfadnamen in einem xamarin. Android-Projekt können recht lang sein.
Beispielsweise könnte während eines Builds ein Pfad wie der folgende generiert werden:

**C:\\Some\\Directory\\Solution\\Project\\obj\\Debug\\__library_projects__\\Xamarin.Forms.Platform.Android\\library_project_imports\\assets**

Unter Windows (wobei die maximale Länge für einen Pfad [260 Zeichen](https://msdn.microsoft.com/library/windows/desktop/aa365247.aspx)beträgt), kann eine **PathTooLongException** generiert werden, während das Projekt erstellt wird, wenn ein generierter Pfad die maximale Länge überschreitet. 

## <a name="fix"></a>Korrektur

Die `UseShortFileNames` MSBuild-Eigenschaft ist auf `True` festgelegt, um diesen Fehler standardmäßig zu umgehen. Wenn diese Eigenschaft auf `True`festgelegt ist, verwendet der Buildprozess kürzere Pfadnamen, um die Wahrscheinlichkeit zu verringern, dass eine **PathTooLongException**erzeugt wird.
Wenn `UseShortFileNames` z. b. auf `True`festgelegt ist, wird der obige Pfad zu einem Pfad gekürzt, der dem folgenden ähnelt:

**C:\\einige\\Verzeichnis\\Projektmappenprojektobj\\Debuggen\\LP\\1\\JL Assets\\\\\\**

Um diese Eigenschaft manuell festzulegen, fügen Sie die folgende MSBuild-Eigenschaft zur Project **. csproj** -Datei hinzu:

```xml
<PropertyGroup>
    <UseShortFileNames>True</UseShortFileNames>
</PropertyGroup>
```

Wenn durch das Festlegen dieses Flags der **PathTooLongException** -Fehler nicht behoben wird, besteht ein weiterer Ansatz darin, einen [gemeinsamen zwischen Ausgabe Stamm](https://blogs.msdn.microsoft.com/kirillosenkov/2015/04/04/using-a-common-intermediate-and-output-directory-for-your-solution/) für Projekte `IntermediateOutputPath` in der Projekt Mappe anzugeben, indem in der Datei Project **. csproj** festgelegt wird. Versuchen Sie, einen relativ kurzen Pfad zu verwenden. Beispiel:

```xml
<PropertyGroup>
    <IntermediateOutputPath>C:\Projects\MyApp</IntermediateOutputPath>
</PropertyGroup>
```

Weitere Informationen zum Festlegen von Buildeigenschaften finden Sie unter [Buildprozess](~/android/deploy-test/building-apps/build-process.md).
