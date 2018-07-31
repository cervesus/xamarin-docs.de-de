---
title: 'Laufzeitfehler: die Assembly "mscorlib.dll" wurde nicht gefunden oder konnte nicht geladen werden'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 1027E16C-2C14-4BB5-AAAB-342F3E28E22E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/03/2018
ms.openlocfilehash: 3510ccafed3ed1b57d8ecd1b99ee8d10f952c57c
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2018
ms.locfileid: "39350887"
---
# <a name="runtime-error-the-assembly-mscorlibdll-was-not-found-or-could-not-be-loaded"></a>Laufzeitfehler: die Assembly "mscorlib.dll" wurde nicht gefunden oder konnte nicht geladen werden

```
<Warning>: The assembly mscorlib.dll was not found or could not be loaded.
<Warning>: It should have been installed in the `/Developer/MonoTouch/Source/monotouch/builds/install/target64/lib/mono/2.0/mscorlib.dll' directory.
<Warning>: Service exited with abnormal code: 1
```

Dieses Problem tritt auf, wenn die *ausgeblendete* `.monotouch-32` und `.monotouch-64` Ordner fehlen die `.xcarchive` zum Signieren / IPA-Datei erstellen, die Laufzeitfehler ausgelöst.

