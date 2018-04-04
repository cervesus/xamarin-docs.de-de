---
title: Wo finde ich die .dSYM Datei iOS Absturz Protokolle symbolicate?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: CB8607B9-FFDA-4617-8210-8E43EC512588
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: ce60c19ab0b680e00338f517e5a3f17f725ed329
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="where-can-i-find-the-dsym-file-to-symbolicate-ios-crash-logs"></a>Wo finde ich die .dSYM Datei iOS Absturz Protokolle symbolicate?

Beim Erstellen von iOS-apps aus visual Studio, endet die .dSYM-Datei, die verwendet werden kann, um Absturzberichte symbolicate auf dem Build-Host im Pfad:
```
    /Users/<username>/Library/Caches/Xamarin/mtbs/builds/<appname>/<guid>/bin/iPhone/<configuration>
```

Beachten Sie, dass die `~/Library` Ordner ausgeblendet wird standardmäßig im Finder müssen dies der Fall, wenn der Finder verwenden **wechseln > wechseln Sie zum Ordner** Menü, und geben Sie: `~/Library/Caches/Xamarin/mtbs/builds/` auf den Ordner zu öffnen.  

Alternativ können Sie einblenden der `~/Library` Ordner mithilfe der **Ansichtsoptionen anzeigen** Bereich für den Basisordner. Wenn Sie den Basisordner in der Randleiste im Finder auswählen und verwenden Sie das Menü Finder **Ansicht > Ansichtsoptionen anzeigen** (oder Cmd-j), dann Sie ein Kontrollkästchen zum sehen **Bibliotheksordner anzeigen**.


### <a name="see-also"></a>Siehe auch
- Erweiterte Schritte für iOS symbolicating Absturzberichte: [http://jmillerdev.net/symbolicating-ios-crash-files-xamarin-ios/](http://jmillerdev.net/symbolicating-ios-crash-files-xamarin-ios/)
- [Demystifying iOS stürzt ab, Anwendungsprotokolle](https://www.raywenderlich.com/23704/demystifying-ios-application-crash-logs)
