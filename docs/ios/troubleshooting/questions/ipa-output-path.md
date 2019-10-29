---
title: Kann ich den Ausgabepfad der IPA-Datei ändern?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: F5E5DCC6-F7CC-48E2-89E8-709E9C269502
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/21/2017
ms.openlocfilehash: 6df481688bfaa1e23cc56e6e34586f23d8ab9da6
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73031040"
---
# <a name="can-i-change-the-output-path-of-the-ipa-file"></a>Kann ich den Ausgabepfad der IPA-Datei ändern?

## <a name="for-cycle-7-and-higher"></a>Für Cycle 7 und höher
Ja, Sie können hierfür angepasste MSBuild-Ziele verwenden. Die einfachste Option ist, die `.ipa` Datei zu kopieren, nachdem Sie erstellt wurde.

Diese Schritte funktionieren für alle IOS-Projekte, die die MSBuild-Build-Engine unter Mac oder Windows verwenden. (Hinweis: für alle Unified API Projekte wird die MSBuild-Build-Engine verwendet.)

1. Öffnen Sie die Datei `.csproj` für das IOS-App-Projekt in einem Text-Editor, und fügen Sie dann am Ende die folgenden Zeilen hinzu (unmittelbar vor dem schließenden `</Project>`-Tag):

    ```xml
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

2. Legen Sie destinationfolder auf den gewünschten Ausgabeordner fest. Wie üblich können Sie die MSBuild-Eigenschaften (z. b. $ (OutputPath)) in diesem Argument verwenden, wenn Sie möchten.

## <a name="notes"></a>Hinweise

- Die `CreateIpaDependsOn`-Eigenschaft wird in der Datei `Xamarin.iOS.Common.targets` definiert, die Teil von xamarin. IOS ist. Dies verhält sich wie im Abschnitt Überschreiben von [vordefinierten Zielen](https://docs.microsoft.com/visualstudio/msbuild/how-to-extend-the-visual-studio-build-process#overriding-predefined-targets) im Artikel [Vorgehensweise: Erweitern Sie den Visual Studio-Buildprozess](https://docs.microsoft.com/visualstudio/msbuild/how-to-extend-the-visual-studio-build-process).

- Sie können eine Verschiebungs Aufgabe anstelle einer **Kopier** **Aufgabe verwenden** , wenn Sie dies bevorzugen. Wenn Sie diese Option auswählen und unter Windows aufbauen, müssen Sie den voll qualifizierten Aufgaben Namen `<Microsoft.Build.Tasks.Move>` verwenden, um eine Mehrdeutigkeit mit den xamarinvs-Buildaufgaben zu vermeiden.

## <a name="for-versions-before-xamarin-studio-6005174--xamarin-for-visual-studio-410530"></a>Für Versionen vor Xamarin Studio 6.0.0.5174 | Xamarin für Visual Studio 4.1.0.530

Ja, Sie können hierfür angepasste MSBuild-Ziele verwenden. Die einfachste Option ist, die `.ipa` Datei zu kopieren, nachdem Sie erstellt wurde.

Diese Schritte funktionieren für alle IOS-Projekte, die die MSBuild-Build-Engine unter Mac oder Windows verwenden. (Hinweis: für alle Unified API Projekte wird die MSBuild-Build-Engine verwendet.)

1. Öffnen Sie die Datei `.csproj` für das IOS-App-Projekt in einem Text-Editor, und fügen Sie dann am Ende die folgenden Zeilen hinzu (unmittelbar vor dem schließenden `</Project>`-Tag).

    ```xml
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

2. Legen Sie den `DestinationFolder` auf den gewünschten Ausgabeordner fest. Wie üblich können Sie MSBuild-Eigenschaften (wie `$(OutputPath)`) in diesem Argument verwenden, wenn Sie möchten.

## <a name="notes"></a>Hinweise

- Die `CreateIpaDependsOn`-Eigenschaft wird in der Datei `Xamarin.iOS.Common.targets` definiert, die Teil von xamarin. IOS ist. t verhält sich wie im Abschnitt Überschreiben von [vordefinierten Zielen](https://docs.microsoft.com/visualstudio/msbuild/how-to-extend-the-visual-studio-build-process#overriding-predefined-targets) im Artikel [Vorgehensweise: Erweitern Sie den Visual Studio-Buildprozess](https://docs.microsoft.com/visualstudio/msbuild/how-to-extend-the-visual-studio-build-process).

- Sie können eine Verschiebungs Aufgabe anstelle einer **Kopier** **Aufgabe verwenden** , wenn Sie dies bevorzugen. Wenn Sie diese Option auswählen und unter Windows aufbauen, müssen Sie den voll qualifizierten Aufgaben Namen `<Microsoft.Build.Tasks.Move>` verwenden, um eine Mehrdeutigkeit mit den xamarinvs-Buildaufgaben zu vermeiden.
