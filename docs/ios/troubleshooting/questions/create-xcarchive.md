---
title: Ist es möglich, ein. xcarchive-Archiv aus Visual Studio zu erstellen?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 417D84FB-1BA9-4DB9-A683-66E960BA3D0D
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 04/03/2018
ms.openlocfilehash: 27697223735c4074dd508d3f603ef6aa4e47b1be
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73031177"
---
# <a name="is-it-possible-to-create-a-xcarchive-archive-from-visual-studio"></a>Ist es möglich, ein. xcarchive-Archiv aus Visual Studio zu erstellen?

## <a name="for-xamarin-4"></a>Für xamarin 4

Ab xamarin 4. x ist es nun möglich, eine `.xcarchive` aus Windows zu erstellen, indem die `ArchiveOnBuild`-Eigenschaft auf `true`festgelegt wird. Beispielsweise können Sie `MSBuild` in der Befehlszeile verwenden:

```bash
msbuild /p:Configuration=Release /p:ServerAddress=10.211.55.2 /p:ServerUser=xamUser /p:Platform=iPhone /p:ArchiveOnBuild=true /t:"Build" MyProject.csproj
```

Der `.xcarchive` wird in das `$HOME/Library/Developer/Xcode/Archives` Verzeichnis auf dem Mac-buildhost eingefügt, das sowohl von Xcode als auch von Xamarin Studio zum Anzeigen bereits erstellter Archive verwendet werden.

In diesem [xamarin-Forums Beitrag](https://forums.xamarin.com/discussion/comment/156635/#Comment_156635) finden Sie einige kurze Hinweise zur `ArchiveOnBuild`-Eigenschaft. Weitere Informationen zu den Eigenschaften `ServerAddress` und `ServerUser` finden Sie in der Dokumentation zu [xamarin. IOS-Befehlszeilenbuilds unter Windows](~/ios/get-started/installation/windows/connecting-to-mac/index.md) .

* * *

## <a name="for-xamarin-3-and-earlier"></a>Für xamarin 3 und früher

Die Visual Studio-Erweiterung xamarin 3. x stellt keinen Mechanismus zum Erstellen von `.xcarchive` Archiven bereit. Die Logik, die zum Erstellen von `.xcarchive` Archive in Xamarin Studio auf dem Mac verwendet wird, [wird hier beschrieben](https://bugzilla.xamarin.com/show_bug.cgi?id=35#c5), sodass Sie ggf. ihre eigene `.xcarchive` von Hand erstellen können.

Es ist jedoch erwähnenswert, dass Sie keine `.xcarchive` für die Übermittlung an den App Store benötigen. Sie können eine IPA-Datei übermitteln, sofern Sie mit einem App Store-Verteilungs Profil (kein Ad-hoc-Verteilungs Profil) signiert ist.

Tatsächlich können Sie sogar einfach das `.app` Bundle (das mit einem App Store-Verteilungs Profil signiert ist) komprimieren und diese `.zip` Datei an den App Store übermitteln.

In beiden Fällen können Sie die Anwendungs Lade Anwendung verwenden, um die APP (anstelle von Xcode) zu übermitteln.
