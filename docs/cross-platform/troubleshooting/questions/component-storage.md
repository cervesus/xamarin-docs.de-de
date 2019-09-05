---
title: Wo werden die Komponenten auf meinem Computer gespeichert?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5EBB49EE-39E5-428B-866F-9FC1BB215B31
author: conceptdev
ms.author: crdun
ms.date: 05/08/2018
ms.openlocfilehash: 7725dbff994ffcef9734ad07c6b506064d9c5b2b
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70285056"
---
# <a name="where-are-the-components-stored-on-my-machine"></a>Wo werden die Komponenten auf meinem Computer gespeichert?

Jedes Mal, wenn Sie eine xamarin-Komponente in einem App-Projekt installieren, wird diese an zwei Stellen platziert:

1. In einem Ordner Komponenten auf der Stamm Ebene Ihres Projektmappenordners. Wenn Sie die Komponente aus allen Projekten in der Projekt Mappe entfernen, wird Sie auch aus diesem Ordner entfernt.

2. Eine Kopie wird auch an den folgenden Speicherorten gespeichert:
    - Windows: `%LocalAppData%\Xamarin\Cache\Components`
    - Mac: `~/Library/Caches/Xamarin/Components`

Um eine Komponente vollständig aus Ihrem System zu entfernen, löschen Sie Sie aus Ihren Projekten/Projektmappen und aus dem obigen Cache Ordner.
