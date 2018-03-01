---
title: Wo werden die Komponenten auf meinem Computer gespeichert?
ms.topic: article
ms.prod: xamarin
ms.assetid: 5EBB49EE-39E5-428B-866F-9FC1BB215B31
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.openlocfilehash: 9549363d7aeb5ff7a5cfa79eb38443aaa80b7019
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="where-are-the-components-stored-on-my-machine"></a>Wo werden die Komponenten auf meinem Computer gespeichert?

Wenn Sie eine Xamarin-Komponente in ein App-Projekt installieren, ruft dieser an zwei Orten gespeichert:

1. In einem Ordner der Komponenten auf der Stammebene des Ordners "Lösung". Wenn Sie die Komponente aus allen Projekten in der Projektmappe entfernen, wird es aus diesem Ordner ebenfalls entfernt werden.

2. Eine Kopie wird auch in den folgenden Orten gespeichert:
    - Windows: `%LocalAppData%\Xamarin\Cache\Components`
    - Mac: `~/Library/Caches/Xamarin/Components`

Daher eine Komponente aus dem System entfernen möchten, löschen Sie ihn aus Ihrer Projekte und Projektmappen und Cacheordner für die oben genannten.
