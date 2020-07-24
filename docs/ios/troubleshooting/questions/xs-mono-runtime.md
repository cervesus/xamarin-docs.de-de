---
title: Wie richte ich Mono-Runtime-Umgebungsvariablen für iOS-Projekte in Xamarin Studio ein?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 1176CEA9-C7F1-411B-8F1A-99374E8AFF33
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/31/2017
ms.openlocfilehash: 30585bede5569edfa50ab9f450bcb62e04c8a10d
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86938584"
---
# <a name="how-do-i-set-mono-runtime-environment-variables-for-ios-projects-in-xamarin-studio"></a>Wie richte ich Mono-Runtime-Umgebungsvariablen für iOS-Projekte in Xamarin Studio ein?

Wenn Sie Lauf Zeit Umgebungsvariablen für Mono festlegen müssen, können Sie auf den **Projektoptionen > >** Seite "Allgemein" ausführen festgelegt werden.

Hinweis: auf diese Weise festgelegte Garbage Collection-Umgebungsvariablen für Sgen (Mono \_ GC \_ Parametern) werden nur beim Starten von Xamarin Studio verwendet. Wenn Sie die APP vom Gerät starten, werden die Einstellungen für Sgen ignoriert. 

Um eine Umgebungsvariable für eine APP dauerhaft festzulegen, müssen Sie diese als zusätzliches mberührungs-Argument (für alle relevanten Konfigurationen) hinzufügen:

```csharp
   --setenv=NAME=VALUE
```

Informationen zu den Umgebungsvariablen, die festgelegt werden können, finden Sie in der Mono-man- [http://docs.go-mono.com/?link=man%3amono(1)](http://docs.go-mono.com/?link=man%3amono(1)) Seite:`ENVIRONMENT VARIABLES`

![Festlegen von Umgebungsvariablen für ein Projekt](xs-mono-runtime-images/environment-variables.jpg)
