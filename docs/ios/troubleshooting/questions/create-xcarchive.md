---
title: Ist es möglich, ein. xcarchive-Archiv aus Visual Studio zu erstellen?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 417D84FB-1BA9-4DB9-A683-66E960BA3D0D
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 04/03/2018
ms.openlocfilehash: 1b078b8cb4d1129127997e9fabdd0b128e09c90f
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70769367"
---
# <a name="is-it-possible-to-create-a-xcarchive-archive-from-visual-studio"></a>Ist es möglich, ein. xcarchive-Archiv aus Visual Studio zu erstellen?

## <a name="for-xamarin-4"></a>Für xamarin 4

Ab xamarin 4. x ist es nun möglich, eine `.xcarchive` aus Windows zu erstellen, indem die `ArchiveOnBuild` -Eigenschaft auf `true`festgelegt wird. Verwenden Sie `MSBuild` z. b. in der Befehlszeile:

```bash
msbuild /p:Configuration=Release /p:ServerAddress=10.211.55.2 /p:ServerUser=xamUser /p:Platform=iPhone /p:ArchiveOnBuild=true /t:"Build" MyProject.csproj
```

Die `.xcarchive` wird in das `$HOME/Library/Developer/Xcode/Archives` Verzeichnis auf dem Mac-buildhost platziert, das sowohl von Xcode als auch Xamarin Studio durchsuchen, um zuvor erstellten Archive anzuzeigen.

In diesem [xamarin Forums-Beitrag](https://forums.xamarin.com/discussion/comment/156635/#Comment_156635) finden Sie einige kurze Hinweise `ArchiveOnBuild` zur-Eigenschaft. Weitere Informationen zu den `ServerAddress` -und- `ServerUser` Eigenschaften finden Sie in der Dokumentation zu [xamarin. IOS-Befehlszeilenbuilds unter Windows](~/ios/get-started/installation/windows/connecting-to-mac/index.md) .

* * *

## <a name="for-xamarin-3-and-earlier"></a>Für xamarin 3 und früher

Die Visual Studio-Erweiterung xamarin 3. x stellt keinen Mechanismus zum Erstellen `.xcarchive` von Archiven bereit. Die Logik, die zum Erstellen `.xcarchive` von Archiven in Xamarin Studio auf dem Mac verwendet wird, [wird hier beschrieben](https://bugzilla.xamarin.com/show_bug.cgi?id=35#c5), sodass Sie ggf `.xcarchive` . ihre eigene erstellen können, wenn gewünscht.

Es ist jedoch erwähnenswert, dass Sie keine `.xcarchive` zum Übermitteln an den App Store benötigen. Sie können eine IPA-Datei übermitteln, sofern Sie mit einem App Store-Verteilungs Profil (kein Ad-hoc-Verteilungs Profil) signiert ist.

Tatsächlich können Sie sogar einfach das `.app` Paket (das mit einem App Store-Verteilungs Profil signiert ist) packen und diese `.zip` Datei an den App Store übermitteln.

In beiden Fällen können Sie die Anwendungs Lade Anwendung verwenden, um die APP (anstelle von Xcode) zu übermitteln.
