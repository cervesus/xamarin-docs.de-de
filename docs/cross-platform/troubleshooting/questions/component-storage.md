---
title: Wo werden die Komponenten auf meinem Computer gespeichert?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5EBB49EE-39E5-428B-866F-9FC1BB215B31
author: asb3993
ms.author: amburns
ms.date: 05/08/2018
ms.openlocfilehash: 4152c8ef7eeba3748d9244e27e48f3f9a2c0019b
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61359099"
---
# <a name="where-are-the-components-stored-on-my-machine"></a>Wo werden die Komponenten auf meinem Computer gespeichert?

Wenn Sie eine Xamarin-Komponente in einem App-Projekt installieren, ruft es an zwei Orten platziert:

1. In einem Ordner der Komponenten auf der Stammebene der Projektmappenordner. Wenn Sie die Komponente aus allen Projekten in der Projektmappe entfernen, wird es von diesem Ordner ebenfalls entfernt.

2. Eine Kopie wird auch in den folgenden Speicherorten gespeichert:
    - Windows: `%LocalAppData%\Xamarin\Cache\Components`
    - Mac: `~/Library/Caches/Xamarin/Components`

Kann zum vollständigen entfernen eine Komponente aus dem System gelöscht werden aus Ihrer Projekte und Projektmappen und aus den oben genannten Cacheordner.
