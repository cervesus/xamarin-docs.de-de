---
title: Ausführungsumgebung für Xamarin.iOS-Apps
description: In diesem Artikel wird beschrieben, wie Sie temporäre und permanente Umgebungsvariablen für eine Xamarin.iOS-App einrichten. Die Variablen können in den Eigenschaften eines Projekts oder als zusätzliche Argumente für das mtouch-Packtool angegeben werden.
ms.prod: xamarin
ms.assetid: 9801644A-89BB-4491-AD28-7F3B97D2CD62
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/05/2017
ms.openlocfilehash: 5296f03cae28e1025c760004c520a2b415ec493d
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2018
ms.locfileid: "39351582"
---
# <a name="execution-environment-for-xamarinios-apps"></a>Ausführungsumgebung für Xamarin.iOS-Apps

Die *Ausführungsumgebung* ist die Menge an Umgebungsvariablen, die die Programmausführung beeinflussen. Umgebungsvariablen können vorübergehend in den Eigenschaften des Projekts oder dauerhaft durch Angabe der zusätzlichen Argumente für das mtouch-Packtool festgelegt werden.

## <a name="temporary-environment-variables"></a>Temporäre Umgebungsvariablen

Temporäre Umgebungsvariablen werden im Fenster **Eigenschaften**/**Optionen** im Abschnitt **Ausführen > Allgemein** festgelegt. Diese Umgebungsvariablen sind nur wirksam, wenn die Anwendung mit Visual Studio für Mac gestartet wird. Sobald die App manuell durch Tippen gestartet wird, werden diese Umgebungsvariablen nicht festgelegt.

## <a name="permanent-environment-variables"></a>Permanente Umgebungsvariablen

Permanente Umgebungsvariablen werden festgelegt, indem Sie zusätzliche Argumente für das mtouch-Packtool angeben. Diese Umgebungsvariablen werden in die ausführbare Datei kompiliert und festgelegt, auch wenn die App nicht über Visual Studio für Mac gestartet wurde.

## <a name="example"></a>Beispiel

```csharp
# log all exceptions to the device log
--setenv:MONO_TRACE=E:all

# to pass multipe environment variables, use --setenv multiple times
--setenv:MONO_TRACE=E:all --setenv:GC_DONT_GC=1
```

