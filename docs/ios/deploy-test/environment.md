---
title: Umgebung
ms.topic: article
ms.prod: xamarin
ms.assetid: 9801644A-89BB-4491-AD28-7F3B97D2CD62
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: c46d3aa151d4a596ca3da881a3c4e5e38bb41cbd
ms.sourcegitcommit: d450ae06065d8f8c80f3588bc5a614cfd97b5a67
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/21/2018
---
# <a name="environment"></a>Umgebung

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

