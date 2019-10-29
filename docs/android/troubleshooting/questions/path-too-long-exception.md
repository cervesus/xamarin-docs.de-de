---
title: Gewusst wie Auflösen eines PathTooLongException-Fehlers?
description: In diesem Artikel wird erläutert, wie Sie eine PathTooLongException auflösen, die beim Entwickeln einer APP auftreten kann.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 60EE1C8D-BE44-4612-B3B5-70316D71B1EA
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 05/29/2018
ms.openlocfilehash: ffe88546ff58387865d71268bd64ec05c8aec3c5
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73026788"
---
# <a name="how-do-i-resolve-a-pathtoolongexception-error"></a>Gewusst wie Auflösen eines PathTooLongException-Fehlers?

## <a name="cause"></a>Ursache

Generierte Pfadnamen in einem xamarin. Android-Projekt können recht lang sein.
Beispielsweise könnte während eines Builds ein Pfad wie der folgende generiert werden:

**C:\\einige\\Verzeichnis\\Lösung\\Project\\obj\\Debug\\__library_projects__\\xamarin. Forms. Platform. Android\\library_project_imports\\Assets**

Unter Windows (wobei die maximale Länge für einen Pfad [260 Zeichen](https://msdn.microsoft.com/library/windows/desktop/aa365247.aspx)beträgt), kann eine **PathTooLongException** generiert werden, während das Projekt erstellt wird, wenn ein generierter Pfad die maximale Länge überschreitet. 

## <a name="fix"></a>Korrektur

Die `UseShortFileNames` MSBuild-Eigenschaft ist auf `True` festgelegt, um diesen Fehler standardmäßig zu umgehen. Wenn diese Eigenschaft auf `True`festgelegt wird, verwendet der Buildprozess kürzere Pfadnamen, um die Wahrscheinlichkeit zu verringern, dass eine **PathTooLongException**erzeugt wird.
Wenn `UseShortFileNames` z. b. auf `True`festgelegt ist, wird der obige Pfad zu einem Pfad gekürzt, der dem folgenden ähnelt:

**C:\\einige\\Verzeichnis\\Lösung\\Project\\obj\\\\\\\\\\**

Um diese Eigenschaft manuell festzulegen, fügen Sie die folgende MSBuild-Eigenschaft zur Project **. csproj** -Datei hinzu:

```xml
<PropertyGroup>
    <UseShortFileNames>True</UseShortFileNames>
</PropertyGroup>
```

Wenn durch Festlegen dieses Flags der **PathTooLongException** -Fehler nicht behoben wird, besteht ein anderer Ansatz darin, einen [gemeinsamen zwischen Ausgabe Stamm](https://blogs.msdn.microsoft.com/kirillosenkov/2015/04/04/using-a-common-intermediate-and-output-directory-for-your-solution/) für Projekte in der Projekt Mappe anzugeben, indem `IntermediateOutputPath` in der Datei Project **. csproj** festgelegt wird. Versuchen Sie, einen relativ kurzen Pfad zu verwenden. Beispiel:

```xml
<PropertyGroup>
    <IntermediateOutputPath>C:\Projects\MyApp</IntermediateOutputPath>
</PropertyGroup>
```

Weitere Informationen zum Festlegen von Buildeigenschaften finden Sie unter [Buildprozess](~/android/deploy-test/building-apps/build-process.md).
