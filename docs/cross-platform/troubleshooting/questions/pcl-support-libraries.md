---
title: Wie kann ich anzeigen, welche Bibliotheken in PCL unterstützt werden?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 14FF03BD-AF41-4DB1-B307-2349C13DE7E4
author: conceptdev
ms.author: crdun
ms.date: 07/27/2018
ms.openlocfilehash: 31dc5114a04deaf1a35bbd24f71cfa552f61d226
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70290984"
---
# <a name="how-can-i-view-what-libraries-are-supported-in-a-pcl"></a>Wie kann ich anzeigen, welche Bibliotheken in PCL unterstützt werden?

- Einen Überblick über die verschiedenen Funktionen, die von den verschiedenen PCL-Zielplattformen unterstützt werden, finden Sie unter dem Bereich " *unterstützte Features* " auf dieser Seite:[https://docs.microsoft.com/dotnet/standard/cross-platform/cross-platform-development-with-the-portable-class-library](https://docs.microsoft.com/dotnet/standard/cross-platform/cross-platform-development-with-the-portable-class-library)

- Eine andere Möglichkeit ist die Verwendung der [.net-Portabilitäts Analyse](https://visualstudiogallery.msdn.microsoft.com/1177943e-cfb7-4822-a8a6-e56c7905292b) , um zu bewerten, ob Ihre vorhandene Bibliothek in ein PCL-Profil konvertiert werden kann.

- Eine dritte Möglichkeit besteht darin, den Inhalt des eigentlichen Profils zu durchsuchen, das Sie möglicherweise verwenden. Beispielsweise können Sie mit dem Profil 78 die folgenden Schritte durchlaufen: `C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\.NETPortable\v4.5\Profile\Profile78\`Und alle darin enthaltenen Assemblys anzeigen.

Welche Methode Sie ausgewählt haben, beachten Sie, dass einige Funktionen über nuget und die Microsoft BCL-Bibliothek heruntergeladen werden müssen.
