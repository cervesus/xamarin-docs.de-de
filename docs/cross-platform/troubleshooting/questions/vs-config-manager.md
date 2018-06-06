---
title: Integrieren Visual Studio nicht warum sollte ich mein Projekt referenzierte Bibliothek in meinen Build?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: b9009db8-e716-43aa-b40e-6f28a8eb1b82
author: asb3993
ms.author: amburns
ms.date: 12/02/2016
ms.openlocfilehash: d7aeac2f433e8fdf231f5887f1537f15e2bd1976
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34782221"
---
# <a name="why-doesnt-visual-studio-include-my-referenced-library-project-in-my-build"></a>Integrieren Visual Studio nicht warum sollte ich mein Projekt referenzierte Bibliothek in meinen Build?

Visual Studio verwendet die **Configuration Manager** um zu bestimmen, welche Projekte in einer Projektmappe automatisch in einer bestimmten Konfiguration der Erstellung oder Bereitstellung enthalten sind.

Einige Vorlagen, die mit einem Projekt referenzierte Bibliothek generiert werden müssen noch die referenzierte Bibliothek, die in der Konfiguration enthalten; Andernfalls werden Fehler muss jedoch manuell festgelegt werden.

## <a name="how-to-use-the-configuration-manager"></a>Gewusst wie: Verwenden des Konfigurations-Managers

1. Open **erstellen > Configuration Manager**
2. Wählen Sie die Konfiguration zum Anpassen, z. B. **Debuggen | iPhone**
3. Aktivieren Sie die Kontrollkästchen für die Projekte, die Sie einschließen möchten.

> [!NOTE]
> Abgeblendeten Feldern automatisch verarbeitet werden und darf keine Änderungen erforderlich.

Screencast der folgenden Schritte aus: [http://screencast.com/t/zLoQOpEn](http://screencast.com/t/zLoQOpEn)
