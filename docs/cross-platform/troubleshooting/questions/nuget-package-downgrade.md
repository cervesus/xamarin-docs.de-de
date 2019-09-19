---
title: Wie stufe ich ein NuGet-Paket herab?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 2375F833-A630-471E-B8E9-5AD2CB81F264
author: conceptdev
ms.author: crdun
ms.date: 05/08/2018
ms.openlocfilehash: daa8159dd7cac1f727904ca5de08908fd3ca1af9
ms.sourcegitcommit: 6b833f44d5fd8dc7ab7f8546e8b7d383e5a989db
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/18/2019
ms.locfileid: "71105669"
---
# <a name="how-do-i-downgrade-a-nuget-package"></a>Wie stufe ich ein NuGet-Paket herab?

Visual Studio für Mac & Visual Studio über Features verfügen, mit denen ältere Versionen von Paketen ausgewählt und automatisch installiert werden können. ähnlich wie das Aktualisieren von Paketen funktioniert. Diese Schritte werden im folgenden beschrieben.

## <a name="visual-studio"></a>Visual Studio

1. Wechseln Sie zu Extras **> nuget-Paket-Manager > Paket-Manager-Konsole** .
2. Legen Sie das Projekt unter dem **Standard Projekt** fest.
3. Verwenden Sie die folgende Syntax:

    > Install-package [PackageName]-Version [Registerkarte für Versions Menü]

Sie können auch den exakten Befehl auf der nuget-Seite des Pakets kopieren bzw. einfügen. Beispiel für xamarin. Forms:[https://www.nuget.org/packages/Xamarin.Forms/](https://www.nuget.org/packages/Xamarin.Forms/)

## <a name="visual-studio-for-mac"></a>Visual Studio für Mac

1. Klicken Sie im Projekt mit der rechten Maustaste auf den Ordner Pakete & Wählen Sie **Pakete hinzufügen** aus.
2. In der Suchleiste können Sie die folgende Syntax verwenden, um nach den erforderlichen Paketen zu suchen:

    `[PackageName] version:*`

### <a name="examples"></a>Beispiele 

- Listet alle xamarin. Forms-Pakete auf: 

    `Xamarin.Forms version:`

- Listet alle xamarin. Forms 1.4. x-Pakete auf: 

    `Xamarin.Forms version:1.4`

> [!NOTE]
> Wenn Sie ein Leerzeichen zwischen `version:` & der Versionsnummer hinzufügen, verhält sich die Suche so, als ob keine Version angegeben wurde.
