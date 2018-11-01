---
title: Wie stufe ich ein NuGet-Paket herab?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 2375F833-A630-471E-B8E9-5AD2CB81F264
author: asb3993
ms.author: amburns
ms.date: 05/08/2018
ms.openlocfilehash: 206336cbcdc85e5e2f3f010e947981cb96e7cd1a
ms.sourcegitcommit: 729035af392dc60edb9d99d3dc13d1ef69d5e46c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2018
ms.locfileid: "50674716"
---
# <a name="how-do-i-downgrade-a-nuget-package"></a>Wie stufe ich ein NuGet-Paket herab?

Visual Studio für Mac und Visual Studio haben die Features für ältere Versionen der Pakete auswählen und diese automatisch installiert werden; ähnlich wie das Aktualisieren von funktioniert Paketen. Diese Schritte werden unten beschrieben.

## <a name="visual-studio"></a>Visual Studio

1. Wechseln Sie zu **Tools > NuGet-Paket-Manager > Paket-Manager-Konsole**
2. Legen Sie das Projekt unter **Standardprojekt**
3. Mit der folgenden Syntax:

    > Install-Package [Paketname]-Version [Tab für Version Menü]

Sie können auch kopieren und den exakten Befehl über die Paket NuGet-Seite einfügen. Beispiel für Xamarin.Forms: [https://www.nuget.org/packages/Xamarin.Forms/](https://www.nuget.org/packages/Xamarin.Forms/)

## <a name="visual-studio-for-mac"></a>Visual Studio für Mac

1. Klicken Sie in Ihrem Projekt mit der rechten Maustaste in den Ordner "Pakete", und wählen Sie **Pakete hinzufügen**
2. In der Searchbar können Sie die folgende Syntax für die erforderlichen Pakete zu suchen:

    `[PackageName] version:*`

### <a name="examples"></a>Beispiele 
- Listet alle Xamarin.Forms-Pakete an: 

    `Xamarin.Forms version:`

- Listet alle Pakete von Xamarin.Forms-1.4.x installiert sein: 

    `Xamarin.Forms version:1.4`

*Hinweis: Wenn Sie ein Leerzeichen zwischen hinzufügen `version:` und die Versionsnummer, als ob keine Version angegeben wurde, kann die Suche verhält.*
