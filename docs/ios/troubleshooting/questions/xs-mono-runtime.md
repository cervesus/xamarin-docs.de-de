---
title: Wie richte ich Mono-Runtime-Umgebungsvariablen für iOS-Projekte in Xamarin Studio ein?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 1176CEA9-C7F1-411B-8F1A-99374E8AFF33
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/31/2017
ms.openlocfilehash: ba74e316f706e5bf22f973de4dad38d94ccd0db9
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61417661"
---
# <a name="how-do-i-set-mono-runtime-environment-variables-for-ios-projects-in-xamarin-studio"></a>Wie richte ich Mono-Runtime-Umgebungsvariablen für iOS-Projekte in Xamarin Studio ein?

Wenn Sie Common Language Runtime-Umgebungsvariablen für Mono festlegen müssen, können festgelegt werden der **Projektoptionen > ausführen > Allgemein** Seite.

Hinweis: Garbage Collection-Umgebungsvariablen für die SGen (MONO\_GC\_PARAMS) festlegen, die auf diese Weise nur werden, beim Starten von Xamarin Studio verwendet wird. Wenn Sie die app auf dem Gerät starten, werden die Einstellungen für Sgen ignoriert. 

Um eine Umgebungsvariable für eine app endgültig festlegen zu können, müssen Sie dies als eine weitere Mtouch-Argument (für alle Konfigurationen) hinzufügen:

```csharp
   --setenv=NAME=VALUE
```

Um die Umgebungsvariablen anzuzeigen, die festgelegt werden können, finden Sie in der Mono-Manpage:  [http://docs.go-mono.com/?link=man%3amono(1)](http://docs.go-mono.com/?link=man%3amono(1)) Finden Sie im Abschnitt: `ENVIRONMENT VARIABLES`

![](xs-mono-runtime-images/environment-variables.jpg "Festlegen von Umgebungsvariablen für ein Projekt")
