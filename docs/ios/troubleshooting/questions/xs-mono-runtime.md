---
title: Wie richte ich Mono-Runtime-Umgebungsvariablen für iOS-Projekte in Xamarin Studio ein?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 1176CEA9-C7F1-411B-8F1A-99374E8AFF33
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/31/2017
ms.openlocfilehash: fdd208122934b6fa8194a644592d1e23c4000d57
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73030894"
---
# <a name="how-do-i-set-mono-runtime-environment-variables-for-ios-projects-in-xamarin-studio"></a>Wie richte ich Mono-Runtime-Umgebungsvariablen für iOS-Projekte in Xamarin Studio ein?

Wenn Sie Lauf Zeit Umgebungsvariablen für Mono festlegen müssen, können Sie auf den **Projektoptionen > >** Seite "Allgemein" ausführen festgelegt werden.

Hinweis: Garbage Collection-Umgebungsvariablen für Sgen (Mono\_GC\_Parametern) werden auf diese Weise festgelegt und nur beim Starten von Xamarin Studio verwendet. Wenn Sie die APP vom Gerät starten, werden die Einstellungen für Sgen ignoriert. 

Um eine Umgebungsvariable für eine APP dauerhaft festzulegen, müssen Sie diese als zusätzliches mberührungs-Argument (für alle relevanten Konfigurationen) hinzufügen:

```csharp
   --setenv=NAME=VALUE
```

Informationen zu den Umgebungsvariablen, die festgelegt werden können, finden Sie auf der Seite Mono man: [http://docs.go-mono.com/?link=man%3amono(1)](http://docs.go-mono.com/?link=man%3amono(1)) im Abschnitt: `ENVIRONMENT VARIABLES`

![](xs-mono-runtime-images/environment-variables.jpg "Setting environment variables for a project")
