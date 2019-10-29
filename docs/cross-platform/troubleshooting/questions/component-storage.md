---
title: Wo werden die Komponenten auf meinem Computer gespeichert?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5EBB49EE-39E5-428B-866F-9FC1BB215B31
author: davidortinau
ms.author: daortin
ms.date: 05/08/2018
ms.openlocfilehash: 9447cf903c8789078e66082e720eeecfa7bb3e0d
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73014235"
---
# <a name="where-are-the-components-stored-on-my-machine"></a>Wo werden die Komponenten auf meinem Computer gespeichert?

Jedes Mal, wenn Sie eine xamarin-Komponente in einem App-Projekt installieren, wird diese an zwei Stellen platziert:

1. In einem Ordner Komponenten auf der Stamm Ebene Ihres Projektmappenordners. Wenn Sie die Komponente aus allen Projekten in der Projekt Mappe entfernen, wird Sie auch aus diesem Ordner entfernt.

2. Eine Kopie wird auch an den folgenden Speicherorten gespeichert:
    - Windows: `%LocalAppData%\Xamarin\Cache\Components`
    - Mac: `~/Library/Caches/Xamarin/Components`

Um eine Komponente vollständig aus Ihrem System zu entfernen, löschen Sie Sie aus Ihren Projekten/Projektmappen und aus dem obigen Cache Ordner.
