---
title: Wie kann ich anzeigen, welche Bibliotheken in PCL unterstützt werden?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 14FF03BD-AF41-4DB1-B307-2349C13DE7E4
author: asb3993
ms.author: amburns
ms.date: 07/27/2018
ms.openlocfilehash: 7e1017baf7daed68b5e55319a9ce13a4a2df5f2e
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61341264"
---
# <a name="how-can-i-view-what-libraries-are-supported-in-a-pcl"></a>Wie kann ich anzeigen, welche Bibliotheken in PCL unterstützt werden?

- Finden Sie eine Übersicht über die verschiedenen Funktionen, die durch die verschiedenen Zielplattformen der PCL-Ziel unter unterstützt die *unterstützte Funktionen* Teil dieser Seite: [https://docs.microsoft.com/dotnet/standard/cross-platform/cross-platform-development-with-the-portable-class-library](https://docs.microsoft.com/dotnet/standard/cross-platform/cross-platform-development-with-the-portable-class-library)

- Eine weitere Möglichkeit ist die Verwendung der [.NET Portability Analyzer](https://visualstudiogallery.msdn.microsoft.com/1177943e-cfb7-4822-a8a6-e56c7905292b) zu bewerten, ob Ihre vorhandene Bibliothek in ein PCL-Profil konvertiert werden kann.

- Eine dritte Möglichkeit ist, den Inhalt des tatsächlichen Profil zu suchen, die Sie verwenden können. Verwenden z. B. Profil 78, können Sie hier: `C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\.NETPortable\v4.5\Profile\Profile78\` Und alle darin enthaltenen Assemblys anzeigen.

Welche Methode Sie ausgewählt haben, Bitte beachten Sie, dass einige Funktionen, die über NuGet und die Microsoft-BCL-Bibliothek heruntergeladen werden.
