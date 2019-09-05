---
title: Warum integriert Visual Studio mein referenziertes Bibliotheksprojekt nicht in meinen Build?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: b9009db8-e716-43aa-b40e-6f28a8eb1b82
author: conceptdev
ms.author: crdun
ms.date: 12/02/2016
ms.openlocfilehash: 37fa93ef7377456d61d1a5f5de56d5de6b0f3c7f
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70282913"
---
# <a name="why-doesnt-visual-studio-include-my-referenced-library-project-in-my-build"></a>Warum integriert Visual Studio mein referenziertes Bibliotheksprojekt nicht in meinen Build?

Visual Studio verwendet die **Configuration Manager** , um zu bestimmen, welche Projekte in einer Projekt Mappe automatisch in einer bestimmten Build-oder Bereitstellungs Konfiguration enthalten sind.

Für einige Vorlagen, die mit einem referenzierten Bibliotheksprojekt generiert werden, ist die Bibliothek, auf die verwiesen wird, bereits in der Konfiguration enthalten. Andernfalls muss Sie manuell festgelegt werden.

## <a name="how-to-use-the-configuration-manager"></a>Verwenden des Configuration Manager

1. **Build > Configuration Manager** öffnen
2. Wählen Sie die Konfiguration zum Anpassen aus, z. b. **Debug | iPhone**
3. Aktivieren Sie die Kontrollkästchen für die Projekte, die Sie einschließen möchten.

> [!NOTE]
> Ausgegraute Felder werden automatisch behandelt und sollten keine Änderungen erfordern.

Screencast der folgenden Schritte:[http://screencast.com/t/zLoQOpEn](http://screencast.com/t/zLoQOpEn)
