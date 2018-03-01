---
title: "Wie richte ich Mono-Runtime-Umgebungsvariablen für iOS-Projekten in Xamarin Studio?"
ms.topic: article
ms.prod: xamarin
ms.assetid: 1176CEA9-C7F1-411B-8F1A-99374E8AFF33
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/31/2017
ms.openlocfilehash: 6032ea89aa54719cc4b0fdde67e67f1ec8fb183b
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="how-do-i-set-mono-runtime-environment-variables-for-ios-projects-in-xamarin-studio"></a>Wie richte ich Mono-Runtime-Umgebungsvariablen für iOS-Projekten in Xamarin Studio?

Wenn Sie Common Language Runtime für Mono Umgebungsvariablen festlegen müssen, können festgelegt werden der **Projektoptionen > ausführen > Allgemein** Seite.

Hinweis: Garbagecollection-Umgebungsvariablen für SGen (MONO\_GC\_PARAMS) festlegen, die auf diese Weise nur werden, beim Starten von Xamarin Studio verwendet wird. Wenn Sie die app vom Gerät zu starten, werden die Einstellungen für Sgen ignoriert. 

Um eine Umgebungsvariable für eine app dauerhaft festzulegen, müssen Sie dies als zusätzliche Mtouch Argument (für alle relevanten Konfigurationen) hinzufügen:

```csharp
   --setenv=NAME=VALUE
```

Die Umgebungsvariablen, die festgelegt werden können, finden Sie auf der Seite "Mono / Man": [http://docs.go-mono.com/?link=man%3amono (1)](http://docs.go-mono.com/?link=man%3amono(1)) finden Sie im Abschnitt: `ENVIRONMENT VARIABLES`

![](xs-mono-runtime-images/environment-variables.jpg "Festlegen von Umgebungsvariablen für ein Projekt")
