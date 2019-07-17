---
title: Kann ich den Ausgabepfad der IPA-Datei ändern?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: F5E5DCC6-F7CC-48E2-89E8-709E9C269502
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/21/2017
ms.openlocfilehash: 8b0686a91f18b41aa8e2e7db071123c0d96723a0
ms.sourcegitcommit: 32c7cf8b0d00464779e4b0ea43e2fd996632ebe0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/17/2019
ms.locfileid: "68290104"
---
# <a name="can-i-change-the-output-path-of-the-ipa-file"></a>Kann ich den Ausgabepfad der IPA-Datei ändern?

## <a name="for-cycle-7-and-higher"></a>Für den Zyklus 7 und höher
Ja, können Sie benutzerdefinierte MSBuild-Ziele verwenden, um dies zu erreichen. Es ist wahrscheinlich die einfachste Möglichkeit zum Kopieren der `.ipa` Datei, nachdem es erstellt wurde.

Diese Schritte funktionieren für alle iOS-Projekt, die die MSBuild-Build-Engine unter Mac oder Windows verwendet. (Hinweis: alle Unified API-Projekte verwenden, die MSBuild-Build-Engine.)

1. Öffnen der `.csproj` -Datei für das iOS-app-Projekt in einem Text-Editor, und klicken Sie dann die folgenden Zeilen am Ende hinzufügen (unmittelbar vor dem schließenden `</Project>` Tag):
    
    ```
    <PropertyGroup>
           <CreateIpaDependsOn>
           $(CreateIpaDependsOn);
            CopyIpa
           </CreateIpaDependsOn>
    </PropertyGroup>
    
    <Target Name="CopyIpa"
        Condition="'$(OutputType)' == 'Exe'
            And '$(ComputedPlatform)' == 'iPhone'
            And '$(BuildIpa)' == 'true'">
        <Copy
            SourceFiles="$(IpaPackagePath)"
            DestinationFolder="$(OutputPath)"
        />
    </Target>
    ```

2. Legen Sie die DestinationFolder zum gewünschten Ausgabeordner. Wie üblich können Sie MSBuild-Eigenschaften (z. B. $(OutputPath)) innerhalb dieses Argument, wenn Sie möchten.

## <a name="notes"></a>Hinweise
- Die `CreateIpaDependsOn` Eigenschaft wird definiert, der `Xamarin.iOS.Common.targets` Datei, die Teil von Xamarin.iOS. Es verhält sich wie beschrieben in der [Überschreiben vordefinierter Ziele](https://docs.microsoft.com/visualstudio/msbuild/how-to-extend-the-visual-studio-build-process#overriding-predefined-targets) Abschnitt des Artikels [Vorgehensweise: Erweitern des Visual Studio-Buildvorgangs](https://docs.microsoft.com/visualstudio/msbuild/how-to-extend-the-visual-studio-build-process).

- Können Sie eine **verschieben** Task anstelle eines **Kopie** Aufgabe, wenn Sie Ihren bevorzugten. Wenn Sie sich entscheiden, dass die Option, und Sie in Windows erstellen, müssen Sie den Task den vollqualifizierten Namen `<Microsoft.Build.Tasks.Move>` zur Vermeidung eine Mehrdeutigkeit mit dem XamarinVS Buildaufgaben.

## <a name="for-versions-before-xamarin-studio-6005174--xamarin-for-visual-studio-410530"></a>Für ältere Versionen von Xamarin Studio 6.0.0.5174 | Xamarin für Visual Studio 4.1.0.530

Ja, können Sie benutzerdefinierte MSBuild-Ziele verwenden, um dies zu erreichen. Es ist wahrscheinlich die einfachste Möglichkeit zum Kopieren der `.ipa` Datei, nachdem es erstellt wurde.

Diese Schritte funktionieren für alle iOS-Projekt, die die MSBuild-Build-Engine unter Mac oder Windows verwendet. (Hinweis: alle Unified API-Projekte verwenden, die MSBuild-Build-Engine.)

1. Öffnen der `.csproj` -Datei für das iOS-app-Projekt in einem Text-Editor, und klicken Sie dann die folgenden Zeilen am Ende hinzufügen (unmittelbar vor dem schließenden `</Project>` Tag).

    ```csharp
    <PropertyGroup>
        <CreateIpaDependsOn>
            $(CreateIpaDependsOn);
            CopyIpa
        </CreateIpaDependsOn>
    </PropertyGroup>
    
    <Target Name="CopyIpa"
        Condition="'$(OutputType)' == 'Exe'
            And '$(ComputedPlatform)' == 'iPhone'
            And '$(BuildIpa)' == 'true'">
        <Copy
            SourceFiles="$(OutputPath)$(IpaPackageName)"
            DestinationFolder="/Users/macuser/Desktop/"
        />
    </Target>
    ```

2. Legen Sie die `DestinationFolder` zum gewünschten Ausgabeordner. Sie können wie gewohnt verwenden MSBuild-Eigenschaften (z. B. `$(OutputPath)`) innerhalb dieses Argument, wenn Sie möchten.

## <a name="notes"></a>Hinweise
- Die `CreateIpaDependsOn` Eigenschaft wird definiert, der `Xamarin.iOS.Common.targets` Datei, die Teil von Xamarin.iOS. t verhält sich wie beschrieben in der [Überschreiben vordefinierter Ziele](https://docs.microsoft.com/visualstudio/msbuild/how-to-extend-the-visual-studio-build-process#overriding-predefined-targets) Abschnitt des Artikels [Vorgehensweise: Erweitern des Visual Studio-Buildvorgangs](https://docs.microsoft.com/visualstudio/msbuild/how-to-extend-the-visual-studio-build-process).

- Können Sie eine **verschieben** Task anstelle eines **Kopie** Aufgabe, wenn Sie Ihren bevorzugten. Wenn Sie sich entscheiden, dass die Option, und Sie in Windows erstellen, müssen Sie den Task den vollqualifizierten Namen `<Microsoft.Build.Tasks.Move>` zur Vermeidung eine Mehrdeutigkeit mit dem XamarinVS Buildaufgaben.
