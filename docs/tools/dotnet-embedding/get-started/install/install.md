---
title: Installieren von .NET einbetten
description: Dieses Dokument beschreibt das Einbetten von .NET zu installieren. Es wird erläutert, wie die Tools von Hand ausgeführt wie Bindungen generiert automatisch, wie Sie benutzerdefinierte MSBuild-Ziele und die erforderlichen Postbuildschritte verwenden.
ms.prod: xamarin
ms.assetid: 47106AF3-AC6E-4A0E-B30B-9F73C116DDB3
author: chamons
ms.author: chhamo
ms.date: 4/18/2018
ms.openlocfilehash: 057a1f3f662b2dbe2f8aee277505e1d6e8798084
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34793794"
---
# <a name="installing-net-embedding"></a>Installieren von .NET einbetten

## <a name="installing-net-embedding-from-nuget"></a>Installieren von .NET Einbetten von NuGet

Wählen Sie **hinzufügen > NuGet-Pakete hinzufügen...**  und installieren Sie **Embeddinator 4000** aus dem NuGet-Paket-Manager:

![NuGet-Paket-Manager](images/visualstudionuget.png)

Installieren dieses **Embeddinator 4000.exe** und **Objcgen** in der **Pakete/Embeddinator-4000/Tools** Verzeichnis.

Im Allgemeinen sollten die neueste Version von der Embeddinator-4000 für den Download ausgewählt werden. Objective-C-Unterstützung erfordert 0,4 oder höher.

## <a name="running-manually"></a>Manuelles Ausführen

Nun, dass die NuGet installiert ist, können Sie die Tools manuell ausführen.

- Öffnen Sie ein Terminal (MacOS) oder eine Eingabeaufforderung (Windows)
- Wechseln Sie in Ihre Projektmappenstamm
- Die Tools ist in installiert:
    - **. / Pakete/Embeddinator-4000. [VERSION] / Tools/Objcgen** (Objective-C)
    - **. / Pakete/Embeddinator-4000. [VERSION]/tools/Embeddinator-4000.exe** (Java/C) 
- Auf MacOS **Objcgen** kann direkt ausgeführt werden. 
- Unter Windows **Embeddinator 4000.exe** kann direkt ausgeführt werden.
- Auf MacOS **Embeddinator 4000.exe** mit ausgeführt werden muss **Mono /**: 
    - `mono ./packages/Embeddinator-4000.[VERSION]/tools/Embeddinator-4000.exe`

Jede Befehlsaufruf benötigen eine Anzahl der Parameter in der Dokumentation plattformspezifischen aufgeführt.

## <a name="automatic-binding-generation"></a>Automatische Bindung generation

Es gibt zwei Ansätze zum automatisch .NET einbetten Teil des Buildprozesses ausführen.

- Benutzerdefinierte MSBuild-Ziele
- Postbuildschritte

Während dieses Dokument sowohl beschrieben wird, wird die benutzerdefinierte MSBuild-Ziel mit bevorzugt. POST-Buildschritte wird nicht unbedingt ausgeführt werden, wenn über die Befehlszeile erstellen.

### <a name="custom-msbuild-targets"></a>Benutzerdefinierte MSBuild-Ziele

Zum Anpassen des Builds mit MSbuild-Ziele erstellen Sie zunächst eine **Embeddinator 4000.targets** neben Ihrem Csproj ähnlich der folgenden Datei:

```xml
<Project DefaultTargets="Build" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
    <Target Name="RunEmbeddinator" AfterTargets="AfterBuild" Inputs="$(OutputPath)/$(AssemblyName).dll" Outputs="$(IntermediateOutputPath)/Embeddinator/$(AssemblyName).framework/$(AssemblyName)">
        <Exec Command="" />
    </Target>
</Project>
```

Hier sollte den Befehlstyp Text sich mit einem .NET einbetten Aufrufe in der Dokumentation plattformspezifischen aufgeführt gefüllt werden.

Um dieses Ziel zu verwenden:

- Schließen Sie das Projekt in Visual Studio 2017 oder im Visual Studio für Mac
- Öffnen Sie die Csproj Bibliothek in einem Text-editor
- Fügen Sie diese Zeile am unteren, über die endgültige `</Project>` Zeile:

```xml
 <Import Project="Embeddinator-4000.targets" />
```

- Öffnen Sie das Projekt erneut.

### <a name="post-build-steps"></a>Postbuildschritte

Die Schritte zum Hinzufügen von Postbuildschritt zum Ausführen von .NET einbetten variieren je nach der IDE:

#### <a name="visual-studio-for-mac"></a>Visual Studio für Mac

Wechseln Sie in Visual Studio für Mac zu **Projektoptionen > Erstellen > benutzerdefinierte Befehle** und Hinzufügen einer **nach dem Erstellen** Schritt.

Richten Sie den Befehl in der Clientplattform-spezifische Dokumentation aufgeführt.

> [!NOTE]
> Stellen Sie sicher, dass die Versionsnummer zu verwenden, die Sie von NuGet installiert

Wenn Sie mit der ständigen Entwicklung C#-Projekt auf diese Weise werden, können Sie auch einen benutzerdefinierten Befehl zum Bereinigen Hinzufügen der **Ausgabe** vor dem Ausführen des .NET einbetten Verzeichnis:

```shell
rm -Rf '${SolutionDir}/output/'
```

![Benutzerdefinierte Buildvorgang](images/visualstudiocustombuild.png)

> [!NOTE]
> Die generierte Bindung befinden sich im Verzeichnis angegeben durch die `--outdir` oder `--out` Parameter. Der Name der generierten Bindung kann variieren, wie einige Plattformen auf Paketnamen begrenzt ist.

#### <a name="visual-studio-2017"></a>Visual Studio 2017

Wir werden im Wesentlichen dasselbe eingerichtet, aber die Menüs in Visual Studio 2017 sind etwas anders. Die Shellbefehle sind auch etwas anders.

Wechseln Sie zu **Projektoptionen > Buildereignisse** und geben Sie den Befehl aufgeführt, die in der plattformspezifischen-Dokumentation in der **Postbuildereignis-Befehlszeile** Feld. Zum Beispiel:

![Einbetten von auf Windows .NET](images/visualstudiowindows.png)
