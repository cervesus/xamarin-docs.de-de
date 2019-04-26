---
title: Installieren von Einbetten von .NET
description: 'Dieses Dokument beschreibt die Vorgehensweise: Einbetten von .NET zu installieren. Es wird erläutert, wie führen Sie die Tools von Hand, wie Bindungen generiert automatisch, wie Sie benutzerdefinierte MSBuild-Ziele, und die erforderlichen Schritte für nach der Erstellung zu verwenden.'
ms.prod: xamarin
ms.assetid: 47106AF3-AC6E-4A0E-B30B-9F73C116DDB3
author: chamons
ms.author: chhamo
ms.date: 04/18/2018
ms.openlocfilehash: 2a572748c21d2a640add3346d1162f4b6bdc8e99
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61076611"
---
# <a name="installing-net-embedding"></a>Installieren von Einbetten von .NET

## <a name="installing-net-embedding-from-nuget"></a>Installieren von .NET Einbetten von NuGet

Wählen Sie **hinzufügen > NuGet-Pakete hinzufügen...**  und installieren Sie **Embeddinator-4000** aus der NuGet-Paket-Manager:

![NuGet-Paket-Manager](images/visualstudionuget.png)

Dadurch werden installiert **Embeddinator-4000.exe** und **Objcgen** in die **Pakete/Embeddinator-4000/Tools** Verzeichnis.

Im Allgemeinen muss die neueste Version von der Embeddinator-4000 für das Herunterladen von ausgewählt werden. Objective-C-Unterstützung erfordert 0.4 oder höher.

## <a name="running-manually"></a>Manuelles Ausführen

Nach der Installation des NuGet-Pakets, können Sie das Tool manuell ausführen.

- Öffnen Sie ein Terminal (MacOS) oder eine Eingabeaufforderung (Windows)
- Wechseln Sie zu Ihrer Projektmappenstamm
- Das Tool ist im installiert:
    - **. / Packages/Embeddinator-4000. [VERSION] / Tools/Objcgen** (Objective-C)
    - **. / Packages/Embeddinator-4000. [VERSION]/tools/Embeddinator-4000.exe** (Java/C)
- Unter MacOS **Objcgen** kann direkt ausgeführt werden.
- Auf Windows **Embeddinator-4000.exe** kann direkt ausgeführt werden.
- Unter MacOS **Embeddinator-4000.exe** muss für die Ausführung mit **mono**:
    - `mono ./packages/Embeddinator-4000.[VERSION]/tools/Embeddinator-4000.exe`

Jede Befehlsaufruf benötigen etliche Parameter, die in der Dokumentation zu plattformspezifischen aufgeführt.

## <a name="automatic-binding-generation"></a>Automatische Generierung

Es gibt zwei Ansätze für die automatische Ausführung Einbetten von .NET Teil des Buildprozesses.

- Benutzerdefinierte MSBuild-Ziele
- Schritt nach dem Erstellen

In diesem Dokument beide beschrieben wird, ist ein benutzerdefiniertes MSBuild-Ziel mit bevorzugten. Nachbearbeitungsschritte der Build wird nicht unbedingt ausgeführt werden, wenn Sie über die Befehlszeile zu erstellen.

### <a name="custom-msbuild-targets"></a>Benutzerdefinierte MSBuild-Ziele

Um den Build mit MSbuild-Ziele anpassen möchten, erstellen Sie zunächst eine **Embeddinator-4000.targets** Datei neben Ihrer CSPROJ-Datei dem folgenden ähnelt:

```xml
<Project DefaultTargets="Build" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
    <Target Name="RunEmbeddinator" AfterTargets="AfterBuild" Inputs="$(OutputPath)/$(AssemblyName).dll" Outputs="$(IntermediateOutputPath)/Embeddinator/$(AssemblyName).framework/$(AssemblyName)">
        <Exec Command="" />
    </Target>
</Project>
```

Hier sollte die Befehls Text sich mit einem der Aufrufe Einbetten von .NET aufgelistet, die in der Dokumentation zu plattformspezifischen gefüllt werden.

Um dieses Ziel zu verwenden:

- Schließen Sie das Projekt in Visual Studio 2017 oder Visual Studio für Mac
- Öffnen Sie die Bibliothek Csproj in einem Text-editor
- Fügen Sie diese Zeile im unteren Bereich über die endgültige `</Project>` Zeile:

```xml
 <Import Project="Embeddinator-4000.targets" />
```

- Öffnen Sie Ihr Projekt

### <a name="post-build-steps"></a>Schritt nach dem Erstellen

Die Schritte zum Hinzufügen von Postbuildschritt zum Einbetten von .NET ausführen, hängt von der IDE:

#### <a name="visual-studio-for-mac"></a>Visual Studio für Mac

Wechseln Sie in Visual Studio für Mac zu **Projektoptionen > Erstellen > benutzerdefinierte Befehle** und Hinzufügen einer **nach dem Erstellen** Schritt.

Richten Sie den Befehl in der Dokumentation zu plattformspezifischen aufgeführt.

> [!NOTE]
> Stellen Sie sicher, dass die Versionsnummer zu verwenden, die Sie von NuGet installiert

Wenn Sie beabsichtigen, fortlaufende Entwicklung auf Ausführen, werden die C# -Projekt können Sie auch einen benutzerdefinierten Befehl bereinigt Hinzufügen der **Ausgabe** Verzeichnis vor der Ausführung, Einbetten von .NET:

```shell
rm -Rf '${SolutionDir}/output/'
```

![Benutzerdefinierte Build-Aktion](images/visualstudiocustombuild.png)

> [!NOTE]
> Die generierte Bindung befinden sich im Verzeichnis von der `--outdir` oder `--out` Parameter. Der Name der generierten Bindung kann variieren, da einige Plattformen Einschränkungen auf Paketnamen haben.

#### <a name="visual-studio-2017"></a>Visual Studio 2017

Wir im Wesentlichen das gleiche eingerichtet wird, aber die Menüs in Visual Studio 2017 sind etwas anders. Die Shellbefehle sind auch etwas anders.

Wechseln Sie zu **Projektoptionen > Buildereignisse** und geben Sie den Befehl aufgeführt, die in der Clientplattform-spezifische Dokumentation in der **Postbuildereignis-Befehlszeile** Feld. Zum Beispiel:

![Das in Windows .NET](images/visualstudiowindows.png)
