---
title: Installieren der .net-Einbettung
description: In diesem Dokument wird beschrieben, wie Sie .net-Einbettungen installieren. Es wird erläutert, wie die Tools Hand gesteuert ausgeführt werden, wie Bindungen automatisch generiert werden, wie benutzerdefinierte MSBuild-Ziele verwendet werden und erforderliche Postbuildschritte ausgeführt werden.
ms.prod: xamarin
ms.assetid: 47106AF3-AC6E-4A0E-B30B-9F73C116DDB3
author: chamons
ms.author: chhamo
ms.date: 04/18/2018
ms.openlocfilehash: 47efbaa12475f627b5963cb6613c3441a1d96aac
ms.sourcegitcommit: c9651cad80c2865bc628349d30e82721c01ddb4a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/03/2019
ms.locfileid: "70227840"
---
# <a name="installing-net-embedding"></a>Installieren der .net-Einbettung

## <a name="installing-net-embedding-from-nuget"></a>Installieren von .net-Einbettungen über nuget

Wählen Sie **Hinzufügen > nuget-Pakete hinzufügen...** aus, und installieren Sie **embeddinator-4000** über den nuget-Paket-Manager:

![NuGet-Paket-Manager](images/visualstudionuget.png)

Dadurch werden **Embeddinator-4000. exe** und **objcgen** im Verzeichnis **Packages/embeddinator-4000/Tools** installiert.

Im Allgemeinen sollte die neueste Version von embeddinator-4000 zum Herunterladen ausgewählt werden. Für die Unterstützung von Ziel-C ist 0,4 oder höher erforderlich.

## <a name="running-manually"></a>Manuelles Ausführen

Nachdem Sie das nuget installiert haben, können Sie die Tools von Hand ausführen.

- Öffnen Sie ein Terminal (macOS) oder eine Eingabeaufforderung (Windows).
- Wechseln Sie zum Stammverzeichnis Ihrer Lösung.
- Die Tools werden in installiert:
  - **./Packages/Embeddinator-4000. [Version]/Tools/objcgen** (Ziel-C)
  - **./Packages/Embeddinator-4000. [Version]/Tools/Embeddinator-4000.exe** (Java/C)
- Unter macOS kann **objcgen** direkt ausgeführt werden.
- Unter Windows kann **Embeddinator-4000. exe** direkt ausgeführt werden.
- Unter macOS muss **Embeddinator-4000. exe** mit **Mono**ausgeführt werden:
  - `mono ./packages/Embeddinator-4000.[VERSION]/tools/Embeddinator-4000.exe`

Jeder Befehls Aufruf benötigt eine Reihe von Parametern, die in der plattformspezifischen Dokumentation aufgeführt sind.

## <a name="automatic-binding-generation"></a>Automatische Bindungs Generierung

Es gibt zwei Ansätze zum automatischen Ausführen des .net-Einbettungs Teils des Buildprozesses.

- Benutzerdefinierte MSBuild-Ziele
- Schritte nach dem Build

Obwohl in diesem Dokument beides beschrieben wird, wird die Verwendung eines benutzerdefinierten MSBuild-Ziels bevorzugt. Postbuildschritte werden nicht notwendigerweise ausgeführt, wenn Sie über die Befehlszeile erstellen.

### <a name="custom-msbuild-targets"></a>Benutzerdefinierte MSBuild-Ziele

Wenn Sie den Build mit MSBuild-Zielen anpassen möchten, erstellen Sie zuerst eine Datei " **embeddinator-4000. targets** " neben Ihrem csproj ähnlich der folgenden:

```xml
<Project DefaultTargets="Build" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
    <Target Name="RunEmbeddinator" AfterTargets="AfterBuild" Inputs="$(OutputPath)/$(AssemblyName).dll" Outputs="$(IntermediateOutputPath)/Embeddinator/$(AssemblyName).framework/$(AssemblyName)">
        <Exec Command="" />
    </Target>
</Project>
```

Hier sollte der Text des Befehls mit einem der .net-Einbettungs Aufrufe ausgefüllt werden, die in der plattformspezifischen Dokumentation aufgeführt sind.

So verwenden Sie dieses Ziel:

- Schließen Sie das Projekt in Visual Studio 2017 oder Visual Studio für Mac
- Öffnen der Bibliothek "csproj" in einem Text-Editor
- Fügen Sie diese Zeile unten oberhalb der letzten `</Project>` Zeile hinzu:

```xml
 <Import Project="Embeddinator-4000.targets" />
```

- Erneutes Öffnen des Projekts

### <a name="post-build-steps"></a>Schritte nach dem Build

Die Schritte zum Hinzufügen eines Schritts nach dem Build zum Ausführen der .net-Einbettung variieren abhängig von der IDE:

#### <a name="visual-studio-for-mac"></a>Visual Studio für Mac

Wechseln Sie in Visual Studio für Mac zu den **Projektoptionen > Erstellen Sie > benutzerdefinierte Befehle** , und fügen Sie einen **nach** dem Buildschritt hinzu.

Richten Sie den Befehl ein, der in der plattformspezifischen Dokumentation aufgeführt ist.

> [!NOTE]
> Stellen Sie sicher, dass Sie die von nuget installierte Versionsnummer verwenden.

Wenn Sie eine laufende Entwicklung für das C# Projekt durchführen möchten, können Sie auch einen benutzerdefinierten Befehl hinzufügen, um das **Ausgabe** Verzeichnis vor dem Ausführen der .net-Einbettung zu bereinigen:

```shell
rm -Rf '${SolutionDir}/output/'
```

![Benutzerdefinierte Buildaktion](images/visualstudiocustombuild.png)

> [!NOTE]
> Die generierte Bindung wird in dem Verzeichnis abgelegt, das durch den `--outdir` - `--out` Parameter oder den-Parameter angegeben wird. Der generierte Bindungs Name kann variieren, da bei einigen Plattformen Einschränkungen für Paketnamen vorliegen.

#### <a name="visual-studio-2017"></a>Visual Studio 2017

Wir richten im Grunde dasselbe ein, aber die Menüs in Visual Studio 2017 unterscheiden sich etwas. Die Shellbefehle sind ebenfalls etwas anders.

Wechseln Sie zu **Projektoptionen > Buildereignisse** , und geben Sie den in der plattformspezifischen Dokumentation aufgelisteten Befehl in das **Befehlszeilen Feld Postbuildereignis** ein. Beispiel:

![.Net-Einbettung unter Windows](images/visualstudiowindows.png)
 