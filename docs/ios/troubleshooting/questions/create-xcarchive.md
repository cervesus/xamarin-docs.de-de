---
title: Ist es möglich, ein Archiv .xcarchive aus Visual Studio zu erstellen?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 417D84FB-1BA9-4DB9-A683-66E960BA3D0D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 23af3bf277ab68ffe93df2e1ee8ee64afc01828d
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="is-it-possible-to-create-a-xcarchive-archive-from-visual-studio"></a>Ist es möglich, ein Archiv .xcarchive aus Visual Studio zu erstellen?

## <a name="for-xamarin-4"></a>Für Xamarin 4

Zum Zeitpunkt der Xamarin 4.x, es ist jetzt möglich, erstellen Sie eine `.xcarchive` von Windows durch Festlegen der `ArchiveOnBuild` Eigenschaft `true`. Beispielsweise können `MSBuild` in der Befehlszeile:

```bash
msbuild /p:Configuration=Release /p:ServerAddress=10.211.55.2 /p:ServerUser=xamUser /p:Platform=iPhone /p:ArchiveOnBuild=true /t:"Build" MyProject.csproj
```

Die `.xcarchive` platziert werden, in der `$HOME/Library/Developer/Xcode/Archives` Verzeichnis auf dem Host der Mac-Build, in dem Xcode und Xamarin Studio zum Anzeigen, die zuvor erstellte Archive zu suchen.

Finden Sie in diesem [Xamarin-Foren post](https://forums.xamarin.com/discussion/comment/156635/#Comment_156635) für einige zusätzliche Hinweise zu kurze der `ArchiveOnBuild` Eigenschaft. Finden Sie in der Dokumentation zu [Xamarin.iOS Befehlszeilenbuilds unter Windows](~/ios/get-started/installation/windows/connecting-to-mac/index.md) zusätzliche ausführliche Informationen zu den `ServerAddress` und `ServerUser` Eigenschaften.

* * *

## <a name="for-xamarin-3-and-earlier"></a>Für Xamarin, 3 oder höher

Xamarin-Erweiterung für die 3.x Visual Studio bietet einen Mechanismus zum erzeugen keine `.xcarchive` archiviert. Dies bedeutet, dass die Logik zum Erstellen `.xcarchive` in Xamarin Studio für Mac archiviert [wird hier beschrieben](https://bugzilla.xamarin.com/show_bug.cgi?id=35#c5), sodass Sie wahrscheinlich Erstellen eigener konnte `.xcarchive` manuell, wenn Sie wollten.

Ist Folgendes zu beachten, dass Sie nicht benötigen jedoch eine `.xcarchive` auf den App Store zu übermitteln. Sie können eine IPA-Datei senden, als es mit einem App Store Verteilung-Profil (keine Ad-hoc-Verteilungsprofil) signiert ist.

Tatsächlich, Sie können auch nur Komprimieren der `.app` Paket (die mit einem Profil speichern Verteilung für App signiert ist), und übermitteln, `.zip` Datei auf den app Store.

In beiden Fällen können Sie die app Application Loader beim Übermitteln der app (anstelle von Xcode).

