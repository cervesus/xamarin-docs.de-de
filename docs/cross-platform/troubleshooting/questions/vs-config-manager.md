---
title: Warum integriert Visual Studio mein referenziertes Bibliotheksprojekt nicht in meinen Build?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: b9009db8-e716-43aa-b40e-6f28a8eb1b82
author: asb3993
ms.author: amburns
ms.date: 12/02/2016
ms.openlocfilehash: d7aeac2f433e8fdf231f5887f1537f15e2bd1976
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61341307"
---
# <a name="why-doesnt-visual-studio-include-my-referenced-library-project-in-my-build"></a>Warum integriert Visual Studio mein referenziertes Bibliotheksprojekt nicht in meinen Build?

Visual Studio verwendet die **Configuration Manager** um zu bestimmen, welche Projekte in einer Projektmappe automatisch in einer bestimmten erstellungs- oder bereitstellungs-Konfiguration enthalten sind.

Einige Vorlagen, die mit einem Projekt referenzierte Bibliothek generiert werden, werden bereits die referenzierte Bibliothek, die in der Konfiguration enthaltenen verfügen. aber Andernfalls müssen sie manuell festgelegt werden.

## <a name="how-to-use-the-configuration-manager"></a>Gewusst wie: Verwenden Sie den Konfigurations-Manager

1. Open **erstellen > Configuration Manager**
2. Wählen Sie die Konfiguration anpassen, z. B. **Debuggen | iPhone**
3. Wählen Sie die Kontrollkästchen für die Projekte, die Sie einschließen möchten.

> [!NOTE]
> Abgeblendeter Felder werden automatisch verarbeitet und keine Änderungen sollten nicht benötigt.

Standbild aus dem für die folgenden Schritte aus: [http://screencast.com/t/zLoQOpEn](http://screencast.com/t/zLoQOpEn)
