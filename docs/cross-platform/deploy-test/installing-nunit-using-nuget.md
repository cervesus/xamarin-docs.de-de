---
title: Installieren von NUnit 2.6.4 mithilfe von NuGet
description: In diesem Leitfaden wird beschrieben, wie NUnit 3.0 mithilfe von NuGet auf NUnit 2.6.4 herabgestuft wird.
ms.topic: article
ms.prod: xamarin
ms.assetid: 7683F2B8-7FDF-48C4-8E7D-649D4D4E79F0
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/24/2017
ms.openlocfilehash: 9dc50aeec88131a1ce49c7e3357382c019774450
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="installing-nunit-264-using-nuget"></a>Installieren von NUnit 2.6.4 mithilfe von NuGet

_In diesem Leitfaden wird beschrieben, wie NUnit 3.0 mithilfe von NuGet auf NUnit 2.6.4 herabgestuft wird._

Entwickler, die Tests in Visual Studio für Mac oder mit Xamarin.UITest schreiben, sollten [NUnit 2.6.4](http://nunit.org/index.php?p=docHome&r=2.6.4) verwenden, da NUnit 3.0 und höhere Versionen nicht mit Visual Studio für Mac oder Xamarin.UITest kompatibel sind. Beim Versuch, Komponententests mit NUnit 3.0 in Visual Studio für Mac oder Xamarin.UITests auszuführen, tritt ein Fehler auf.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

In diesem Leitfaden wird beschrieben, wie NUnit 2.6.4 mithilfe von NuGet für Visual Studio für Mac installiert wird. Falls nötig wird NUnit 3.0 auch deinstalliert.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

In dieser Anleitung wird beschrieben, wie NUnit 3.0 mithilfe von NuGet in Visual Studio 2015 auf NUnit 2.6.4 herabgestuft wird.

-----

## <a name="requirements"></a>Anforderungen

Für diese Anleitung wird vorausgesetzt, dass Sie eine vorhandene Projektmappe mit einer mobilen Anwendung und einem Testprojekt haben.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

## <a name="installing-nunit-264-in-visual-studio-for-mac"></a>Installieren von NUnit 2.6.4 in Visual Studio für Mac

Die folgenden Schritte beschreiben, wie NUnit 2.6.4. installiert wird.


1. **Paket-Manager öffnen**: Klicken Sie mit der rechten Maustaste auf **Pakete**, und wählen Sie **Pakete hinzufügen** im Popupmenü aus:

    [![](installing-nunit-using-nuget-images/add-packages-xs.png "Klicken Sie mit der rechten Maustaste auf „Pakete“, und wählen Sie „Pakete hinzufügen“ im Popupmenü aus.")](installing-nunit-using-nuget-images/add-packages-xs.png)
    
1. **`NUnit version:2.6.4` suchen**: Visual Studio für Mac deinstalliert NUnit 3.0 (falls erforderlich) und lädt dann NUnit 2.6.4 herunter und installiert es. Geben Sie im Dialogfeld **Pakete hinzufügen** im Feld **Suche** in der oberen rechten Ecke den Text `nunit version:2.6.4` ein. Wählen Sie **NUnit** aus den Suchergebnissen aus, und klicken Sie auf die Schaltfläche **Paket hinzufügen**:

    [![](installing-nunit-using-nuget-images/nunit-search-xs.png "Wählen Sie „NUnit“ aus den Suchergebnissen aus, und klicken Sie auf „Paket hinzufügen“")](installing-nunit-using-nuget-images/nunit-search-xs.png)


Sie können die Versionsnummer des NUnit-Pakets im Lösungspad überprüfen und so sicherstellen, dass NUnit 2.6.4 installiert wurde:

[![](installing-nunit-using-nuget-images/nunit-2-6-4-installed.png "Prüfen Sie die Versionsnummer des NUnit-Pakets im Lösungspad")](installing-nunit-using-nuget-images/nunit-2-6-4-installed.png)

## <a name="summary"></a>Zusammenfassung

Dieser Leitfaden erläutert, wie NUnit 3.0 in Visual Studio für Mac mithilfe der Paket-Manager-Konsole auf NUnit 2.6.4 herabgestuft wird.


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="installing-nunit-264-in-visual-studio"></a>Installieren von NUnit 2.6.4 in Visual Studio

Dieser Abschnitt konzentriert sich auf die Verwendung der _NuGet Paket-Manager-Konsole_ in Visual Studio 2015 zur Deinstallation von NUnit 3.0 und anschließenden Installation von NUnit 2.6.4.


1. **NuGet-Paket-Manager-Konsole starten**: Wählen Sie **Tools > NuGet-Paket-Manager > Paket-Manager-Konsole** aus:

    [![](installing-nunit-using-nuget-images/package-manager-console.png "NuGet-Paket-Manager-Konsole starten: Wählen Sie Tools > NuGet-Paket-Manager > Paket-Manager-Konsole aus")](installing-nunit-using-nuget-images/package-manager-console.png)
    
1. **Version von NUnit überprüfen**: Sie können die Versionsnummer des installierten NUnit überprüfen, indem Sie den Befehl `Get-Package -Project <UITEST PROJECT>` ausführen:

    ```bash
    [PM] Get-Package -Project [TEST PROJECT NAME]
    
        Id                                  Versions                                 ProjectName
        --                                  --------                                 -----------
    NUnit                               {3.0.1}                                  [TEST PROJECT NAME]
    Xamarin.UITest                      {1.2.0}                                  [TEST PROJECT NAME]
    ```

Wenn NUnit 3.0 oder höher angezeigt wird, müssen Sie auf NUnit 2.6.4 herabstufen.

1. **NUnit 3.0 deinstallieren**: Verwenden Sie das Cmdlet `Uninstall-Package`, um NUnit 3.0 zu deinstallieren:

        <PM> Uninstall-Package NUnit -Project <TEST PROJECT NAME>
        Attempting to gather dependencies information for package 'NUnit.3.0.1' with respect to project '<TEST PROJECT NAME>', targeting '.NETFramework,Version=v4.5'
        Resolving actions to uninstall package 'NUnit.3.0.1'
        Resolved actions to uninstall package 'NUnit.3.0.1'
        Removed package 'NUnit.3.0.1' from 'packages.config'
        Successfully uninstalled 'NUnit.3.0.1' from <TEST PROJECT NAME>

1. **NUnit 2.6.4 installieren**: Installieren Sie NUnit 2.6.4 wie im folgenden Codeausschnitt mit dem Cmdlet `Install-Package`:

        <PM> Install-Package NUnit -Version 2.6.4 -Project <TEST PROJECT NAME>
        Attempting to gather dependencies information for package 'NUnit.2.6.4' with respect to project '<TEST PROJECT NAME>', targeting '.NETFramework,Version=v4.5'
        Attempting to resolve dependencies for package 'NUnit.2.6.4' with DependencyBehavior 'Lowest'
        Resolving actions to install package 'NUnit.2.6.4'
        Resolved actions to install package 'NUnit.2.6.4'
        Adding package 'NUnit.2.6.4' to folder 'Z:\Desktop\DowngradeNunit\packages'
        Added package 'NUnit.2.6.4' to folder 'Z:\Desktop\DowngradeNunit\packages'
        Added package 'NUnit.2.6.4' to 'packages.config'
        Successfully installed 'NUnit 2.6.4' to <TEST PROJECT NAME>
    
## <a name="summary"></a>Zusammenfassung

Dieser Leitfaden erläutert, wie NUnit 3.0 in Visual Studio 2015 mithilfe der Paket-Manager-Konsole auf NUnit 2.6.4 herabgestuft wird.

-----

## <a name="related-links"></a>Verwandte Links

- [NUnit 2.6.4](http://nunit.org/index.php?p=docHome&r=2.6.4)
- [NUnit 2.6.4 NuGet-Paket](https://www.nuget.org/packages/NUnit/2.6.4)
