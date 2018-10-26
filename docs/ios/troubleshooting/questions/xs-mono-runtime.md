---
title: Wie richte ich Mono-Laufzeit-Umgebungsvariablen für iOS-Projekte in Xamarin Studio?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 1176CEA9-C7F1-411B-8F1A-99374E8AFF33
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/31/2017
ms.openlocfilehash: ba74e316f706e5bf22f973de4dad38d94ccd0db9
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50116939"
---
# <a name="how-do-i-set-mono-runtime-environment-variables-for-ios-projects-in-xamarin-studio"></a>Wie richte ich Mono-Laufzeit-Umgebungsvariablen für iOS-Projekte in Xamarin Studio?

Wenn Sie Common Language Runtime-Umgebungsvariablen für Mono festlegen müssen, können festgelegt werden der **Projektoptionen > ausführen > Allgemein** Seite.

Hinweis: Garbagecollection-Umgebungsvariablen für die SGen (MONO\_GC\_PARAMS) festlegen, die auf diese Weise nur werden, beim Starten von Xamarin Studio verwendet wird. Wenn Sie die app auf dem Gerät starten, werden die Einstellungen für Sgen ignoriert. 

Um eine Umgebungsvariable für eine app endgültig festlegen zu können, müssen Sie dies als eine weitere Mtouch-Argument (für alle Konfigurationen) hinzufügen:

```csharp
   --setenv=NAME=VALUE
```

Die Umgebungsvariablen, die festgelegt werden können, finden Sie in der Mono-Manpage: [ http://docs.go-mono.com/?link=man%3amono(1) ](http://docs.go-mono.com/?link=man%3amono(1)) finden Sie im Abschnitt: `ENVIRONMENT VARIABLES`

![](xs-mono-runtime-images/environment-variables.jpg "Festlegen von Umgebungsvariablen für ein Projekt")
