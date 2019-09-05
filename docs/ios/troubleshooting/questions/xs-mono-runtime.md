---
title: Wie richte ich Mono-Runtime-Umgebungsvariablen für iOS-Projekte in Xamarin Studio ein?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 1176CEA9-C7F1-411B-8F1A-99374E8AFF33
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/31/2017
ms.openlocfilehash: f8e3855b10a20bd4312420f8faf6c68dedde0c67
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70292092"
---
# <a name="how-do-i-set-mono-runtime-environment-variables-for-ios-projects-in-xamarin-studio"></a>Wie richte ich Mono-Runtime-Umgebungsvariablen für iOS-Projekte in Xamarin Studio ein?

Wenn Sie Lauf Zeit Umgebungsvariablen für Mono festlegen müssen, können Sie auf den **Projektoptionen > >** Seite "Allgemein" ausführen festgelegt werden.

Hinweis: Auf diese Weise festgelegte Garbage Collection-Umgebungs\_Variablen\_für Sgen (Mono GC Parametern) werden nur beim Starten von Xamarin Studio verwendet. Wenn Sie die APP vom Gerät starten, werden die Einstellungen für Sgen ignoriert. 

Um eine Umgebungsvariable für eine APP dauerhaft festzulegen, müssen Sie diese als zusätzliches mberührungs-Argument (für alle relevanten Konfigurationen) hinzufügen:

```csharp
   --setenv=NAME=VALUE
```

Informationen zu den Umgebungsvariablen, die festgelegt werden können, finden Sie auf der Mono-man-Seite:  [http://docs.go-mono.com/?link=man%3amono(1)](http://docs.go-mono.com/?link=man%3amono(1))Weitere Informationen finden Sie im Abschnitt:`ENVIRONMENT VARIABLES`

![](xs-mono-runtime-images/environment-variables.jpg "Festlegen von Umgebungsvariablen für ein Projekt")
