---
title: Wo werden die Komponenten auf meinem Computer gespeichert?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5EBB49EE-39E5-428B-866F-9FC1BB215B31
author: asb3993
ms.author: amburns
ms.openlocfilehash: a53045a6179a26b30d824976d11fd2769a84811e
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/09/2018
---
# <a name="where-are-the-components-stored-on-my-machine"></a>Wo werden die Komponenten auf meinem Computer gespeichert?

Wenn Sie eine Xamarin-Komponente in ein App-Projekt installieren, ruft dieser an zwei Orten gespeichert:

1. In einem Ordner der Komponenten auf der Stammebene des Ordners "Lösung". Wenn Sie die Komponente aus allen Projekten in der Projektmappe entfernen, wird es aus diesem Ordner ebenfalls entfernt werden.

2. Eine Kopie wird auch in den folgenden Orten gespeichert:
    - Windows: `%LocalAppData%\Xamarin\Cache\Components`
    - Mac: `~/Library/Caches/Xamarin/Components`

Daher eine Komponente aus dem System entfernen möchten, löschen Sie ihn aus Ihrer Projekte und Projektmappen und Cacheordner für die oben genannten.
