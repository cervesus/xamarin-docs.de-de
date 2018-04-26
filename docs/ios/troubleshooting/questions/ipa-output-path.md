---
title: Kann ich den Ausgabepfad der Datei IPA-Datei ändern?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: F5E5DCC6-F7CC-48E2-89E8-709E9C269502
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 9c80a209279a2f032eb6c9efcba1398ca0e267a5
ms.sourcegitcommit: dc882e9631b4ed52596b944a6fbbdde309346943
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/26/2018
---
# <a name="can-i-change-the-output-path-of-the-ipa-file"></a>Kann ich den Ausgabepfad der Datei IPA-Datei ändern?

## <a name="for-cycle-7-and-higher"></a>Zyklus 7 und höher
Ja, können Sie benutzerdefinierte MSBuild-Ziele verwenden, um dies zu erreichen. Die einfachste Option besteht wahrscheinlich so kopieren Sie die `.ipa` Datei, nachdem es erstellt wurde.

Diese Schritte werden für alle iOS-Projekt arbeiten, die das Buildmodul MSBuild auf Mac oder Windows verwendet. (Hinweis: alle einheitliche API-Projekte verwendet das MSBuild-Buildmodul.)

1. Öffnen der `.csproj` Datei für die iOS-app-Projekt in einem Text-Editor, und fügen Sie die folgenden Zeilen am Ende (unmittelbar vor der schließenden `</Project>` Tag):
    
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

2. Legen Sie die DestinationFolder auf den gewünschten Ausgabeordner. Wie üblich können Sie MSBuild-Eigenschaften (z. B. $(OutputPath)) innerhalb dieses Argument, wenn Sie möchten.

## <a name="notes"></a>Hinweise
- Die `CreateIpaDependsOn` Eigenschaft wird definiert, der `Xamarin.iOS.Common.targets` Datei, die Teil des Xamarin.iOS. Verhält sich wie beschrieben unter *überschreiben "DependsOn"-Eigenschaften* auf [ https://msdn.microsoft.com/library/ms366724.aspx ](https://msdn.microsoft.com/library/ms366724.aspx).

- Können Sie eine **verschieben** Aufgabe anstelle eines **Kopie** Aufgabe, wenn Sie bevorzugte. Wenn Sie auswählen, dass die Option, und Sie werden unter Windows erstellen, müssen Sie den eine vollqualifizierte Vorgangsnamen verwenden `<Microsoft.Build.Tasks.Move>` zur Vermeidung eine Mehrdeutigkeit mit dem XamarinVS Buildaufgaben.

## <a name="for-versions-before-xamarin-studio-6005174--xamarin-for-visual-studio-410530"></a>Bei Versionen vor Xamarin Studio 6.0.0.5174 | Xamarin für Visual Studio 4.1.0.530

Ja, können Sie benutzerdefinierte MSBuild-Ziele verwenden, um dies zu erreichen. Die einfachste Option besteht wahrscheinlich so kopieren Sie die `.ipa` Datei, nachdem es erstellt wurde.

Diese Schritte werden für alle iOS-Projekt arbeiten, die das Buildmodul MSBuild auf Mac oder Windows verwendet. (Hinweis: alle einheitliche API-Projekte verwendet das MSBuild-Buildmodul.)

1. Öffnen der `.csproj` Datei für die iOS-app-Projekt in einem Text-Editor, und fügen Sie die folgenden Zeilen am Ende (unmittelbar vor der schließenden `</Project>` Tag).

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

2. Legen Sie die `DestinationFolder` in den gewünschten Ausgabeordner. Sie können MSBuild-Eigenschaften wie gewohnt verwenden (z. B. `$(OutputPath)`) innerhalb dieses Argument, wenn Sie möchten.

## <a name="notes"></a>Hinweise
- Die `CreateIpaDependsOn` Eigenschaft wird definiert, der `Xamarin.iOS.Common.targets` Datei, die Teil des Xamarin.iOS. Verhält sich wie beschrieben unter *überschreiben "DependsOn"-Eigenschaften* auf [ https://msdn.microsoft.com/library/ms366724.aspx ](https://msdn.microsoft.com/library/ms366724.aspx).

- Können Sie eine **verschieben** Aufgabe anstelle eines **Kopie** Aufgabe, wenn Sie bevorzugte. Wenn Sie auswählen, dass die Option, und Sie werden unter Windows erstellen, müssen Sie den eine vollqualifizierte Vorgangsnamen verwenden `<Microsoft.Build.Tasks.Move>` zur Vermeidung eine Mehrdeutigkeit mit dem XamarinVS Buildaufgaben.
