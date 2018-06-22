---
title: Wie werden ich ein NuGet-Paket heruntergestuft?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 2375F833-A630-471E-B8E9-5AD2CB81F264
author: asb3993
ms.author: amburns
ms.openlocfilehash: 50a96340f8dada802303d6de140812801fdc836d
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/10/2018
ms.locfileid: "33947520"
---
# <a name="how-do-i-downgrade-a-nuget-package"></a>Wie werden ich ein NuGet-Paket heruntergestuft?

Visual Studio für Mac und Visual Studio verfügen über die Funktionen für ältere Versionen der Pakete auswählen und Installieren dieser Komponenten automatisch; ähnlich wie Works aktualisieren Pakete. Diese Schritte werden nachfolgend beschrieben.

## <a name="visual-studio"></a>Visual Studio
1. Wechseln Sie zu **Extras > NuGet-Paket-Manager > Paket-Manager-Konsole**
2. Legen Sie das Projekt unter **Standard-Projekt**
3. Verwenden Sie diese Syntax:

    > Install-Package [PackageName]-Version [Version Menüs Tab]

Sie können auch den exakten Befehl, der das Paket NuGet-Seite kopieren. Beispiel für Xamarin.Forms: [https://www.nuget.org/packages/Xamarin.Forms/](https://www.nuget.org/packages/Xamarin.Forms/)

## <a name="visual-studio-for-mac"></a>Visual Studio für Mac
1. In Ihrem Projekt mit der rechten Maustaste in den Ordner "Pakete", und wählen Sie **Hinzufügen von Paketen**
2. In der Searchbar können Sie die folgende Syntax, für die erforderlichen Pakete gesucht werden soll:

    `[PackageName] version:*`

### <a name="examples"></a>Beispiele 
- Listet alle Xamarin.Forms für Pakete: 

    `Xamarin.Forms version:`
- Listet alle Pakete von Xamarin.Forms 1.4.x installiert sein: 

    `Xamarin.Forms version:1.4`

*Hinweis: Wenn Sie ein Leerzeichen zwischen hinzufügen `version:` & die Versionsnummer, als wären keine Version angegeben wurde, kann die Suche verhält.*

