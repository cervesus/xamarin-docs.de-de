---
title: Ausführungsumgebung für Xamarin.iOS-Apps
description: In diesem Artikel wird beschrieben, wie Sie temporäre und permanente Umgebungsvariablen für eine Xamarin.iOS-App einrichten. Die Variablen können in den Eigenschaften eines Projekts oder als zusätzliche Argumente für das mtouch-Packtool angegeben werden.
ms.prod: xamarin
ms.assetid: 9801644A-89BB-4491-AD28-7F3B97D2CD62
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 06/05/2017
ms.openlocfilehash: e8f08a96d42aa02cb00f37337ecbc2620a8785e4
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70762963"
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
