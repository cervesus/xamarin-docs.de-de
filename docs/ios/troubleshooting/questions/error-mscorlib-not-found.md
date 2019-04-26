---
title: 'Laufzeitfehler: The assembly mscorlib.dll was not found or could not be loaded (Die Assembly „mscorlib.dll“ wurde nicht gefunden oder konnte nicht geladen werden).'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 1027E16C-2C14-4BB5-AAAB-342F3E28E22E
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 04/03/2018
ms.openlocfilehash: 84bb6815c19bcacb4a9d1bddc44d340d51199c32
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61421982"
---
# <a name="runtime-error-the-assembly-mscorlibdll-was-not-found-or-could-not-be-loaded"></a>Laufzeitfehler: The assembly mscorlib.dll was not found or could not be loaded (Die Assembly „mscorlib.dll“ wurde nicht gefunden oder konnte nicht geladen werden).

```
<Warning>: The assembly mscorlib.dll was not found or could not be loaded.
<Warning>: It should have been installed in the `/Developer/MonoTouch/Source/monotouch/builds/install/target64/lib/mono/2.0/mscorlib.dll' directory.
<Warning>: Service exited with abnormal code: 1
```

Dieses Problem tritt auf, wenn die *ausgeblendete* `.monotouch-32` und `.monotouch-64` Ordner fehlen die `.xcarchive` zum Signieren / IPA-Datei erstellen, die Laufzeitfehler ausgelöst.

