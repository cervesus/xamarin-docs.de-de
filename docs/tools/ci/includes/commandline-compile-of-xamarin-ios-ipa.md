---
ms.openlocfilehash: e383bbccd4e76be8a208f5680e5cf21e45a0dbc3
ms.sourcegitcommit: b07e0259d7b30413673a793ebf4aec2b75bb9285
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/26/2019
ms.locfileid: "68511941"
---

In der folgenden Befehlszeile wird ein Releasebuild der Projekt Mappe " **SOLUTION_FILE. sln** " für das iPhone angegeben. Der Speicherort der IPA-Adresse kann festgelegt werden `IpaPackageDir` , indem Sie die-Eigenschaft in der Befehlszeile angeben:

- Auf dem Mac mit **xbuild**:

        xbuild /p:Configuration="Release" \ 
           /p:Platform="iPhone" \ 
           /p:IpaPackageDir="$HOME/Builds" \
           /t:Build MyProject.sln

Der **xbuild** -Befehl befindet sich in der Regel im Verzeichnis **/Library/Frameworks/Mono.Framework/Commands**.

- Unter Windows mit **MSBuild**:

        msbuild /p:Configuration="Release" 
            /p:Platform="iPhone" 
            /p:IpaPackageDir="%USERPROFILE%\Builds" 
            /p:ServerAddress="192.168.1.3" /p:ServerUser="macuser"  
            /t:Build MyProject.sln


von **MSBuild** werden von der `$( )` Befehlszeile über gegebene Ausdrücke nicht automatisch erweitert. Aus diesem Grund wird empfohlen, beim Festlegen `IpaPackageDir` von in der Befehlszeile einen vollständigen Pfad zu verwenden.

Weitere Informationen `IpaPackageDir` zur-Eigenschaft finden Sie in den [Anmerkungen zu dieser Version von IOS 9,8](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/ios/xamarin.ios_9/xamarin.ios_9.8.md#new-msbuild-property-ipapackagedir-to-customize-ipa-output-location) .
