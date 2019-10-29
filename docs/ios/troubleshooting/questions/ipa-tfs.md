---
title: Wie kann ich IPA-Ausgabedateien in den TFS-Ablage Ordner kopieren?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: B0F1E09E-7315-45BA-B7FF-44D2063EE19C
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/21/2017
ms.openlocfilehash: 4493b1a0d06e2f44ee9a11a250395f058baa0548
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73031019"
---
# <a name="how-can-i-copy-ipa-output-files-to-the-tfs-drop-folder"></a>Wie kann ich IPA-Ausgabedateien in den TFS-Ablage Ordner kopieren?

Öffnen Sie die Datei `.csproj` für das IOS-App-Projekt in einem Text-Editor, und fügen Sie dann am Ende die folgenden Zeilen hinzu (unmittelbar vor dem schließenden `</Project>`-Tag):

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
        And '$(BuildIpa)' == 'true'
        And '$(TF_BUILD)' == 'true'">
    <Copy
        SourceFiles="$(OutputPath)$(IpaPackageName)"
        DestinationFolder="$(TF_BUILD_BINARIESDIRECTORY)"
    />
</Target>
```

## <a name="notes"></a>Hinweise

- Dies ist dieselbe allgemeine Methode, die erläutert wird, [kann ich den Ausgabepfad der IPA-Datei ändern?](~/ios/troubleshooting/questions/ipa-output-path.md) Die beiden wichtigen Punkte sind das Festlegen `$(TF_BUILD_BINARIESDIRECTORY)` als Zielordner und das Hinzufügen einer zusätzlichen Bedingung, damit `CopyIpa` nur für TFS-Builds ausgeführt wird.

- Eine Beschreibung `TF_BUILD_BINARIESDIRECTORY` finden Sie unter [vordefinierte Buildvariablen](https://docs.microsoft.com/azure/devops/pipelines/build/variables).

## <a name="additional-references"></a>Weitere Verweise

- [Dokumentation zur Installation von TFS für die Verwendung mit xamarin](https://docs.microsoft.com/azure/devops/repos/tfvc/overview)
- [Azure devops-BuildTask: Xamarin.Android](https://docs.microsoft.com/azure/devops/pipelines/tasks/build/xamarin-android)
- [Azure devops-BuildTask: Xamarin.iOS](https://docs.microsoft.com/azure/devops/pipelines/tasks/build/xamarin-ios)

### <a name="next-steps"></a>Nächste Schritte

In diesem Dokument wird das aktuelle Verhalten von xamarin 3.11.666 für Visual Studio und xamarin. IOS 8.10.3 auf dem Mac-buildhost erläutert. Weitere Unterstützung erhalten Sie bei der Kontaktaufnahme mit uns, oder wenn das Problem weiterhin besteht, auch nachdem Sie die oben genannten Informationen genutzt haben, finden Sie [unter welche Supportoptionen für xamarin verfügbar sind?](~/cross-platform/troubleshooting/support-options.md) Informationen zu Kontaktoptionen, Vorschlägen und wie Sie bei Bedarf einen neuen Fehler melden. .
