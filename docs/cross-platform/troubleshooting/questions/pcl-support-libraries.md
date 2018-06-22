---
title: Wie kann ich anzeigen, welche Bibliotheken in einer PCL unterstützt werden?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 14FF03BD-AF41-4DB1-B307-2349C13DE7E4
author: asb3993
ms.author: amburns
ms.openlocfilehash: 87f65ba2cff2d5990c32aa142f97766a76d6ba05
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/09/2018
ms.locfileid: "33919539"
---
# <a name="how-can-i-view-what-libraries-are-supported-in-a-pcl"></a>Wie kann ich anzeigen, welche Bibliotheken in einer PCL unterstützt werden?

- Sie erhalten einen Überblick über die verschiedenen Funktionen, die von den verschiedenen PCL-Zielplattformen unter unterstützt die *unterstützte Funktionen* Teil dieser Seite: [http://msdn.microsoft.com/library/gg597391.aspx](https://msdn.microsoft.com/library/gg597391.aspx)

- Eine andere Möglichkeit ist die Verwendung der [.NET Portability Analyzer](https://visualstudiogallery.msdn.microsoft.com/1177943e-cfb7-4822-a8a6-e56c7905292b) zu bewerten, ob Ihre vorhandenen Bibliotheksfreigabe in einer PCL-Profil konvertiert werden kann.

- Eine dritte Möglichkeit besteht darin, den Inhalt des aktuellen Profils zu suchen, die Sie verwenden können. Verwenden z. B. Profil 78, Sie möglicherweise finden Sie hier: `C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\.NETPortable\v4.5\Profile\Profile78\` und alle darin enthaltenen Assemblys anzeigen.

Unabhängig davon, welche Methode Sie ausgewählt haben, Bitte beachten Sie, dass einige Funktionen über NuGet und der Microsoft-BCL-Bibliothek heruntergeladen werden.
