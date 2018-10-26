---
title: Wie kann ich die IPA-Ausgabedateien in den TFS-Ablageordner kopieren?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: B0F1E09E-7315-45BA-B7FF-44D2063EE19C
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/21/2017
ms.openlocfilehash: 74e2f2219dcb0908edce7f109844932639038b25
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50113039"
---
# <a name="how-can-i-copy-ipa-output-files-to-the-tfs-drop-folder"></a>Wie kann ich die IPA-Ausgabedateien in den TFS-Ablageordner kopieren?

Öffnen der `.csproj` -Datei für das iOS-app-Projekt in einem Text-Editor, und klicken Sie dann die folgenden Zeilen am Ende hinzufügen (unmittelbar vor dem schließenden `</Project>` Tag):

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

- Dies ist das gleiche allgemeine Verfahren beschriebenen [kann ich Ändern des Ausgabepfads der IPA-Datei?](~/ios/troubleshooting/questions/ipa-output-path.md). Die zwei wichtige Punkte sind festlegen `$(TF_BUILD_BINARIESDIRECTORY)` als Zielordner, und fügen Sie daher eine zusätzliche Bedingung hinzu `CopyIpa` wird nur ausgeführt, für TFS-builds.

- Eine Beschreibung der `TF_BUILD_BINARIESDIRECTORY` finden Sie unter [vordefinierte Buildvariablen](https://docs.microsoft.com/azure/devops/pipelines/build/variables).

## <a name="additional-references"></a>Zusätzliche Referenzen

- [Dokumentation zur Installation von TFS für die Verwendung mit Xamarin](https://docs.microsoft.com/azure/devops/repos/tfvc/overview)
- [Azure DevOps-Buildaufgabe: Xamarin.Android](https://docs.microsoft.com/azure/devops/pipelines/tasks/build/xamarin-android)
- [Azure DevOps-Buildaufgabe: Xamarin.iOS](https://docs.microsoft.com/azure/devops/pipelines/tasks/build/xamarin-ios)

### <a name="next-steps"></a>Nächste Schritte

Dieses Dokument erläutert das aktuelle Verhalten ab Xamarin 3.11.666 für Visual Studio und Xamarin.iOS 8.10.3 auf dem Mac-buildhost. Weitere Unterstützung benötigen, kontaktieren uns, oder wenn dieses Problem bestehen bleibt, auch nach der Verwendung der oben genannten Informationen finden Sie unter [welche Supportoptionen für Xamarin verfügbar sind?](~/cross-platform/troubleshooting/support-options.md) auf Kontaktoptionen, Vorschläge, Informationen sowie zur einen neuen Bug-Datei bei Bedarf.
