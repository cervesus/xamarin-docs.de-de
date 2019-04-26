---
title: Ist es möglich, ein xcarchive-Archivs in Visual Studio zu erstellen?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 417D84FB-1BA9-4DB9-A683-66E960BA3D0D
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 04/03/2018
ms.openlocfilehash: 952777b3178b82657c8f64d5882c532c188c7a79
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61422021"
---
# <a name="is-it-possible-to-create-a-xcarchive-archive-from-visual-studio"></a>Ist es möglich, ein xcarchive-Archivs in Visual Studio zu erstellen?

## <a name="for-xamarin-4"></a>Für Xamarin 4

Ab Xamarin 4.x, es ist jetzt möglich, erstellen Sie eine `.xcarchive` von Windows durch Festlegen der `ArchiveOnBuild` Eigenschaft `true`. Verwenden Sie beispielsweise `MSBuild` in der Befehlszeile:

```bash
msbuild /p:Configuration=Release /p:ServerAddress=10.211.55.2 /p:ServerUser=xamUser /p:Platform=iPhone /p:ArchiveOnBuild=true /t:"Build" MyProject.csproj
```

Die `.xcarchive` platziert werden, der `$HOME/Library/Developer/Xcode/Archives` Verzeichnis auf dem Mac-buildhost, die sowohl Xcode und Xamarin Studio zum Anzeigen, die zuvor erstellte Archive zu suchen.

Finden Sie in diesem [Xamarin-Foren Posten](https://forums.xamarin.com/discussion/comment/156635/#Comment_156635) für einige weitere Hinweise zu kurze der `ArchiveOnBuild` Eigenschaft. Finden Sie in der Dokumentation zu [Xamarin.iOS-Befehlszeilenbuilds auf Windows](~/ios/get-started/installation/windows/connecting-to-mac/index.md) Weitere Einzelheiten zu den `ServerAddress` und `ServerUser` Eigenschaften.

* * *

## <a name="for-xamarin-3-and-earlier"></a>Für Xamarin, 3 oder höher

Die Xamarin-3.x-Visual Studio-Erweiterung bietet einen Mechanismus zum erzeugen keine `.xcarchive` archiviert. Dies bedeutet, dass die Logik zum Erstellen `.xcarchive` archiviert in Xamarin Studio unter Mac [wird hier beschrieben](https://bugzilla.xamarin.com/show_bug.cgi?id=35#c5), sodass Sie wahrscheinlich Ihre eigenen erstellen konnte `.xcarchive` manuell, wenn eingesetzt werden.

Es ist erwähnenswert, dass Sie nicht benötigen jedoch eine `.xcarchive` an den App Store übermitteln. Sie können eine IPA-Datei übermitteln, solange sie mit einer App Store-Verteilungsprofil (keine Ad-hoc-Verteilung-Clientprofil) signiert wird.

Sie können sogar noch zippen Sie den `.app` Bundle (die mit einem App Store-Verteilungsprofil signiert ist), und Sende Sie ab `.zip` Datei an den appstore.

In beiden Fällen können Sie den Application Loader-app zum Übermitteln der app (anstelle von Xcode).

