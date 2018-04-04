---
title: Wo werden die Komponenten auf meinem Computer gespeichert?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5EBB49EE-39E5-428B-866F-9FC1BB215B31
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.openlocfilehash: f0dad6e6219d373eaa9f8410aea7d96c81eceb6b
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="where-are-the-components-stored-on-my-machine"></a>Wo werden die Komponenten auf meinem Computer gespeichert?

Wenn Sie eine Xamarin-Komponente in ein App-Projekt installieren, ruft dieser an zwei Orten gespeichert:

1. In einem Ordner der Komponenten auf der Stammebene des Ordners "Lösung". Wenn Sie die Komponente aus allen Projekten in der Projektmappe entfernen, wird es aus diesem Ordner ebenfalls entfernt werden.

2. Eine Kopie wird auch in den folgenden Orten gespeichert:
    - Windows: `%LocalAppData%\Xamarin\Cache\Components`
    - Mac: `~/Library/Caches/Xamarin/Components`

Daher eine Komponente aus dem System entfernen möchten, löschen Sie ihn aus Ihrer Projekte und Projektmappen und Cacheordner für die oben genannten.
