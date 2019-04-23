---
ms.openlocfilehash: 5929648b802c27916c0a21142907644781d59d0d
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61193256"
---
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Starten Sie Visual Studio, und erstellen Sie eine neue leere Xamarin.Forms-App mit dem Namen **LocalDatabaseTutorial**. Stellen Sie sicher, dass die App .NET Standard als Mechanismus für freigegebenen Code verwendet.

    > [!IMPORTANT]
    > Für die C#- und XAML-Codeausschnitte in diesem Tutorial ist es erforderlich, dass die Projektmappe **LocalDatabaseTutorial** genannt wird. Die Verwendung eines anderen Namens führt zu Buildfehlern, wenn Sie Code aus diesem Tutorial in die Projektmappe kopieren.

    Weitere Informationen zur .NET Standard-Bibliothek, die erstellt wird, finden Sie unter [Anatomy of a Xamarin.Forms application (Aufbau einer Xamarin.Forms-Anwendung)](~/get-started/first-app/index.md) im Artikel [Xamarin.Forms Quickstart Deep Dive (Weitere Details zum Xamarin.Forms-Schnellstart)](~/get-started/first-app/index.md).

1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt **LocalDatabaseTutorial** und anschließend auf **Manage NuGet Packages...** (NuGet-Pakete verwalten...).

    ![Screenshot: ausgewähltes Menüelement „Manage NuGet Packages...“ (NuGet-Pakete hinzufügen...)](../images/vs/add-nuget-packages.png "Menüelement „Manage NuGet Packages...“ (NuGet-Pakete hinzufügen...)")

1. Klicken Sie im **NuGet-Paket-Manager** auf die Registerkarte **Durchsuchen**, suchen Sie nach dem NuGet-Paket **sqlite-net-pcl**, wählen Sie es aus, und klicken Sie auf die Schaltfläche **Installieren**, um es dem Projekt hinzuzufügen:

    ![Screenshot: SQLite.NET-NuGet-Paket im NuGet-Paket-Manager](../images/vs/add-package.png "SQLite.NET-NuGet-Paket")

    > [!NOTE]
    > Es gibt eine Reihe von NuGet-Paketen mit ähnlichen Namen. Das richtige Paket verfügt über die folgenden Attribute:
    > - **Autor(en):** Frank A. Krueger
    > - **ID:** sqlite-net-pcl
    > - **NuGet-Link:** [sqlite-net-pcl](https://www.nuget.org/packages/sqlite-net-pcl/)  
    >
    > Trotz des Paketnamens kann dieses NuGet-Paket in .NET Standard-Projekten verwendet werden.

    Mit diesem Paket können Datenbankvorgänge in der Anwendung verwendet werden.

1. Erstellen Sie die Projektmappe, um sich zu vergewissern, dass keine Fehler vorliegen.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

1. Starten Sie Visual Studio für Mac, und erstellen Sie eine neue leere Xamarin.Forms-App mit dem Namen **LocalDatabaseTutorial**. Stellen Sie sicher, dass die App .NET Standard als Mechanismus für freigegebenen Code verwendet.

    > [!IMPORTANT]
    > Für die C#- und XAML-Codeausschnitte in diesem Tutorial ist es erforderlich, dass die Projektmappe **LocalDatabaseTutorial** genannt wird. Die Verwendung eines anderen Namens führt zu Buildfehlern, wenn Sie Code aus diesem Tutorial in die Projektmappe kopieren.
    
    Weitere Informationen zur .NET Standard-Bibliothek, die erstellt wird, finden Sie unter [Anatomy of a Xamarin.Forms application (Aufbau einer Xamarin.Forms-Anwendung)](~/get-started/first-app/index.md) im Artikel [Xamarin.Forms Quickstart Deep Dive (Weitere Details zum Xamarin.Forms-Schnellstart)](~/get-started/first-app/index.md).

1. Klicken Sie im **Lösungspad** mit der rechten Maustaste auf das Projekt **LocalDatabaseTutorial** und anschließend auf **Add > Add NuGet Packages...** (Hinzufügen > NuGet-Pakete hinzufügen...).

    ![Screenshot: ausgewähltes Menüelement „Add NuGet Packages...“ (NuGet-Pakete hinzufügen...)](../images/vsmac/add-nuget-packages.png "Menüelement „Add NuGet Packages...“ (NuGet-Pakete hinzufügen...)")

1. Suchen Sie im Fenster **Pakete hinzufügen** nach dem NuGet-Paket **sqlite-net-pcl**, wählen Sie es aus, und klicken Sie auf **Paket hinzufügen**, um es dem Projekt hinzuzufügen:

    ![Screenshot: SQLite.NET-NuGet-Paket im NuGet-Paket-Manager](../images/vsmac/add-package.png "SQLite.NET-NuGet-Paket")

    > [!NOTE]
    > Es gibt eine Reihe von NuGet-Paketen mit ähnlichen Namen. Das richtige Paket verfügt über die folgenden Attribute:
    > - **Autor:** Frank A. Krueger
    > - **ID:** sqlite-net-pcl
    > - **NuGet-Link:** [sqlite-net-pcl](https://www.nuget.org/packages/sqlite-net-pcl/)  
    >
    > Trotz des Paketnamens kann dieses NuGet-Paket in .NET Standard-Projekten verwendet werden.

    Mit diesem Paket können Datenbankvorgänge in der Anwendung verwendet werden.

1. Erstellen Sie die Projektmappe, um sich zu vergewissern, dass keine Fehler vorliegen.
