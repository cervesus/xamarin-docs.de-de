---
title: "Wie löse ich einen PathTooLongException-Fehler?"
description: "In diesem Artikel erläutert, wie eine PathTooLongException aufgelöst, die beim Erstellen einer app auftreten können."
ms.topic: article
ms.prod: xamarin
ms.assetid: 60EE1C8D-BE44-4612-B3B5-70316D71B1EA
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/20/2018
ms.openlocfilehash: f4554178648d1257049d1c21cdc3e141acdb3de7
ms.sourcegitcommit: d450ae06065d8f8c80f3588bc5a614cfd97b5a67
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/21/2018
---
# <a name="how-do-i-resolve-a-pathtoolongexception-error"></a>Wie löse ich einen PathTooLongException-Fehler?

## <a name="cause"></a>Ursache

In einem Projekt Xamarin.Android generierten Pfadnamen können recht lang sein.
Ein Pfad wie folgt könnte z. B. während eines Builds generiert:

**"C:"\\einige\\Directory\\Lösung\\Projekt\\Obj\\Debuggen\\__Library_projects__ \\ Xamarin.Forms.Platform.Android\\Library_project_imports\\Bestand**

Unter Windows (steht für die maximale Länge für einen Pfad [260 Zeichen](https://msdn.microsoft.com/library/windows/desktop/aa365247.aspx)), ein **PathTooLongException** beim Erstellen des Projekts ein generierten Pfad überschreitet die maximale Länge erstellt werden konnte. 

## <a name="fix"></a>Problemlösung

Xamarin.Android 8.0, ab der `UseShortFileNames` MSBuild-Eigenschaft kann festgelegt werden, um diesen Fehler zu umgehen. Wenn diese Eigenschaft festgelegt wird, um `True` (die Standardeinstellung ist `False`), verwendet der Buildprozess kürzere Pfadnamen reduziert die Wahrscheinlichkeit, erzeugt eine **PathTooLongException**.
Z. B. wenn `UseShortFileNames` festgelegt ist, um `True`, oben angegebenen Pfad verkürzt, Pfad, der dem folgenden ähnlich ist:

**"C:"\\einige\\Directory\\Lösung\\Projekt\\Obj\\Debuggen\\Lp\\1\\Jl\\Bestand**

Fügen Sie zum Festlegen dieser Eigenschaft die folgenden MSBuild-Eigenschaft für das Projekt **csproj** Datei:

```xml
<PropertyGroup>
    <UseShortFileNames>True</UseShortFileNames>
</PropertyGroup>
```

Weitere Informationen zu den Einstellungseigenschaften für Build, finden Sie unter [Buildprozess](~/android/deploy-test/building-apps/build-process.md).
