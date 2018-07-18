---
title: Wie kann ich IPA-Ausgabedateien in den TFS-Ablageordner kopieren?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: B0F1E09E-7315-45BA-B7FF-44D2063EE19C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 087a20ea3b573595e6cbd2b40d77de649676391e
ms.sourcegitcommit: dc882e9631b4ed52596b944a6fbbdde309346943
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/26/2018
ms.locfileid: "31883708"
---
# <a name="how-can-i-copy-ipa-output-files-to-the-tfs-drop-folder"></a>Wie kann ich IPA-Ausgabedateien in den TFS-Ablageordner kopieren?

Öffnen der `.csproj` Datei für die iOS-app-Projekt in einem Text-Editor, und fügen Sie die folgenden Zeilen am Ende (unmittelbar vor der schließenden `</Project>` Tag):

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

-   Dies ist das gleiche allgemeine Verfahren erläutert, die auf [kann ich den Ausgabepfad der Datei IPA-Datei ändern?](~/ios/troubleshooting/questions/ipa-output-path.md). Zwei wichtige Punkte sind festzulegende `$(TF_BUILD_BINARIESDIRECTORY)` als Zielordner, und fügen Sie daher eine zusätzliche Bedingung hinzu `CopyIpa` wird nur ausgeführt, für die TFS-builds.

-   Eine Beschreibung der `TF_BUILD_BINARIESDIRECTORY` finden Sie unter [ https://msdn.microsoft.com/library/hh850448.aspx ](https://msdn.microsoft.com/library/hh850448.aspx).

## <a name="additional-references"></a>Zusätzliche Referenzen

- [Dokumentation zum Installieren von TFS für die Verwendung mit Xamarin](https://docs.microsoft.com/vsts/tfvc/overview)
- [TFS-BuildTask: Xamarin.Android](https://docs.microsoft.com/vsts/build-release/tasks/build/xamarin-android)
- [TFS-BuildTask: Xamarin.iOS](https://docs.microsoft.com/vsts/build-release/tasks/build/xamarin-ios)

### <a name="next-steps"></a>Nächste Schritte
Dieses Dokument beschreibt das aktuelle Verhalten ab Xamarin 3.11.666 für Visual Studio und Xamarin.iOS 8.10.3 auf dem Mac Host erstellen. Für weitere Unterstützung zu erhalten, wenden Sie sich an uns, oder bleibt dieses Problem auch nach der Nutzung der oben angegebenen Informationen finden Sie unter [welche Supportoptionen für Xamarin verfügbar sind?](~/cross-platform/troubleshooting/support-options.md) Informationen zu Kontaktoptionen Vorschläge, sowie zur die Datei eines neuen Fehlers bei Bedarf. 



