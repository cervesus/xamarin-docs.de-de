---
title: Wie behebe ich den Fehler PathTooLongException?
description: In diesem Artikel wird erläutert, wie die Ausnahme PathTooLongException behoben wird, die beim Kompilieren einer App auftreten kann.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 60EE1C8D-BE44-4612-B3B5-70316D71B1EA
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 05/29/2018
ms.openlocfilehash: ffe88546ff58387865d71268bd64ec05c8aec3c5
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2020
ms.locfileid: "73026788"
---
# <a name="how-do-i-resolve-a-pathtoolongexception-error"></a>Wie behebe ich den Fehler PathTooLongException?

## <a name="cause"></a>Ursache

Die generierten Pfadnamen in einem Xamarin.Android-Projekt können ziemlich lang sein.
Beispielsweise kann während eines Builds ein Pfad wie der folgende generiert werden:

**C:\\Some\\Directory\\Solution\\Project\\obj\\Debug\\__library_projects__\\Xamarin.Forms.Platform.Android\\library_project_imports\\assets**

Unter Windows (maximale Länge eines Pfads: [260 Zeichen](https://msdn.microsoft.com/library/windows/desktop/aa365247.aspx)) kann beim Kompilieren des Projekts **PathTooLongException** ausgelöst werden, wenn ein generierter Pfad die maximale Länge überschreitet. 

## <a name="fix"></a>Korrektur

Die MSBuild-Eigenschaft `UseShortFileNames` ist standardmäßig auf `True` festgelegt, um diesen Fehler zu vermeiden. Wenn diese Eigenschaft auf `True` festgelegt ist, verwendet der Buildprozess kürzere Pfadnamen, um die Wahrscheinlichkeit zu verringern, dass **PathTooLongException** ausgelöst wird.
Wenn `UseShortFileNames` beispielsweise auf `True` festgelegt ist, wird der obige Pfad zu einem Pfad verkürzt, der dem folgenden ähnelt:

**C:\\Some\\Directory\\Solution\\Project\\obj\\Debug\\lp\\1\\jl\\assets**

Um diese Eigenschaft manuell festzulegen, fügen Sie der Datei **.csproj** des Projekts die folgende MSBuild-Eigenschaft hinzu:

```xml
<PropertyGroup>
    <UseShortFileNames>True</UseShortFileNames>
</PropertyGroup>
```

Wenn das Festlegen dieses Flags den Fehler **PathTooLongException** nicht behebt, besteht eine andere Möglichkeit darin, einen [gemeinsamen Zwischenausgabestamm](https://blogs.msdn.microsoft.com/kirillosenkov/2015/04/04/using-a-common-intermediate-and-output-directory-for-your-solution/) für Projekte in Ihrer Projektmappe anzugeben, indem `IntermediateOutputPath` in der Datei **.csproj** des Projekts festgelegt wird. Versuchen Sie, einen relativ kurzen Pfad zu verwenden. Zum Beispiel:

```xml
<PropertyGroup>
    <IntermediateOutputPath>C:\Projects\MyApp</IntermediateOutputPath>
</PropertyGroup>
```

Weitere Informationen zum Festlegen von Buildeigenschaften finden Sie unter [Buildprozess](~/android/deploy-test/building-apps/build-process.md).
