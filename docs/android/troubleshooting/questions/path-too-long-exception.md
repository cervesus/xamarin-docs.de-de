---
title: Wie löse ich einen PathTooLongException-Fehler?
description: In diesem Artikel wird erläutert, wie Sie eine PathTooLongException auf, die auftreten können, während der Erstellung einer app.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 60EE1C8D-BE44-4612-B3B5-70316D71B1EA
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 05/29/2018
ms.openlocfilehash: 4cb3e13ebbe3d9e8aed153528a35ab16c92e2145
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61153323"
---
# <a name="how-do-i-resolve-a-pathtoolongexception-error"></a>Wie löse ich einen PathTooLongException-Fehler?

## <a name="cause"></a>Ursache

Generierte Pfadnamen in eine Xamarin.Android-Projekt können ziemlich lang sein.
Beispielsweise kann ein Pfad wie folgt während eines Builds generiert:

**C:\\Some\\Directory\\Solution\\Project\\obj\\Debug\\__library_projects__\\Xamarin.Forms.Platform.Android\\library_project_imports\\assets**

Auf Windows (die maximale Länge für einen Pfad, in denen ist [260 Zeichen](https://msdn.microsoft.com/library/windows/desktop/aa365247.aspx)), eine **PathTooLongException** konnte beim Erstellen des Projekts, ein generierter Pfad überschreitet die maximale Länge erstellt werden. 

## <a name="fix"></a>Korrektur

Xamarin.Android 8.0, ab der `UseShortFileNames` MSBuild-Eigenschaft kann festgelegt werden, um diesen Fehler zu umgehen. Wenn diese Eigenschaft auf festgelegt ist `True` (der Standardwert ist `False`), verwendet der Buildprozess kürzere Pfadnamen reduzieren die Wahrscheinlichkeit, dass erzeugt eine **PathTooLongException**.
Z. B. wenn `UseShortFileNames` nastaven NA hodnotu `True`, oben angegebenen Pfad wird verkürzt, Pfad, der dem folgenden ähnelt:

**"C:"\\einige\\Directory\\Lösung\\Projekt\\Obj\\Debuggen\\Lp\\1\\Jl\\Assets**

Um diese Eigenschaft festzulegen, fügen Sie die folgende MSBuild-Eigenschaft für das Projekt **csproj** Datei:

```xml
<PropertyGroup>
    <UseShortFileNames>True</UseShortFileNames>
</PropertyGroup>
```

Wenn dieses Flag festlegen, wird nicht das Problem behoben die **PathTooLongException** Fehler ein anderer Ansatz ist, an eine [gemeinsamen Zwischenausgabedatei Stamm](https://blogs.msdn.microsoft.com/kirillosenkov/2015/04/04/using-a-common-intermediate-and-output-directory-for-your-solution/) für Projekte in der Projektmappe durch Festlegen `IntermediateOutputPath` in der Projekt **csproj** Datei. Versuchen Sie, einen relativ kurzen Pfad verwenden. Zum Beispiel:

```xml
<PropertyGroup>
    <IntermediateOutputPath>C:\Projects\MyApp</IntermediateOutputPath>
</PropertyGroup>
```

Weitere Informationen zum Festlegen von Build-Eigenschaften finden Sie unter [Buildprozesses](~/android/deploy-test/building-apps/build-process.md).
