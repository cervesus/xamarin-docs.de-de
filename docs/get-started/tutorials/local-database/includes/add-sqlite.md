---
ms.openlocfilehash: eda3fa134d318ca787432cf5c5a2115b93c3614e
ms.sourcegitcommit: 898ba8e5140ae32a7df7e07c056aff65f6fe4260
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2020
ms.locfileid: "86277768"
---
# <a name="visual-studio"></a>[Visual Studio](#tab/vswin)

Für dieses Tutorial benötigen Sie das neueste Release von Visual Studio 2019 und die Workload **Mobile-Entwicklung mit .NET**. Außerdem müssen Sie über einen gekoppelten Mac verfügen, um die Tutorial-App unter iOS zu kompilieren. Informationen zur Installation der Xamarin-Plattform finden Sie unter [Installieren von Xamarin](~/get-started/installation/index.md). Informationen zum Verbinden von Visual Studio 2019 mit einem Mac-Buildhost finden Sie unter [Durchführen einer Kopplung mit einem Mac für die Xamarin.iOS-Entwicklung](~/ios/get-started/installation/windows/connecting-to-mac/index.md).

1. Starten Sie Visual Studio, und erstellen Sie eine neue leere Xamarin.Forms-App mit dem Namen **LocalDatabaseTutorial**. Stellen Sie sicher, dass die App .NET Standard als Mechanismus für freigegebenen Code verwendet.

    > [!IMPORTANT]
    > Für die C#- und XAML-Codeausschnitte in diesem Tutorial ist es erforderlich, dass die Projektmappe **LocalDatabaseTutorial** genannt wird. Die Verwendung eines anderen Namens führt zu Buildfehlern, wenn Sie Code aus diesem Tutorial in die Projektmappe kopieren.

    Weitere Informationen zur .NET Standard-Bibliothek, die erstellt wird, finden Sie unter [Aufbau einer Xamarin.Forms-Anwendung](~/get-started/first-app/index.md) im Artikel [Xamarin.Forms Quickstart Deep Dive (Ausführliche Erläuterungen zum Xamarin.Forms-Schnellstart)](~/get-started/first-app/index.md).

1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt **LocalDatabaseTutorial** und anschließend auf **Manage NuGet Packages...** (NuGet-Pakete verwalten...).

    ![Screenshot: Menüelement „Manage NuGet Packages...“ (NuGet-Pakete verwalten...) ausgewählt](../images/vs/add-nuget-packages.png "Menüelement „Add NuGet Packages“ (NuGet-Pakete hinzufügen)")

1. Klicken Sie im **NuGet-Paket-Manager** auf die Registerkarte **Durchsuchen**, suchen Sie nach dem NuGet-Paket **sqlite-net-pcl**, wählen Sie es aus, und klicken Sie auf die Schaltfläche **Installieren**, um es dem Projekt hinzuzufügen:

    ![Screenshot: SQLite.NET-NuGet-Paket im NuGet-Paket-Manager](../images/vs/add-package.png "SQLite.NET-NuGet-Paket")

    > [!NOTE]
    > Es gibt eine Reihe von NuGet-Paketen mit ähnlichen Namen. Das richtige Paket verfügt über die folgenden Attribute:
    > - **ID:** sqlite-net-pcl
    > - **Autor(en):** SQLite-net
    > - **Besitzer:** praeclarum
    > - **Projekt-URL:** https://github.com/praeclarum/sqlite-net
    > - **NuGet-Link:** [sqlite-net-pcl](https://www.nuget.org/packages/sqlite-net-pcl/)
    >
    > Trotz des Paketnamens kann dieses NuGet-Paket in .NET Standard-Projekten verwendet werden.

    Mit diesem Paket können Datenbankvorgänge in der Anwendung verwendet werden.

1. Erstellen Sie die Projektmappe, um sich zu vergewissern, dass keine Fehler vorliegen.

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/vsmac)

Für dieses Tutorial benötigen Sie Visual Studio für Mac (neuestes Release) mit Plattformunterstützung für iOS und Android. Zudem ist Xcode (neuestes Release) erforderlich. Weitere Informationen zur Installation der Xamarin-Plattform finden Sie unter [Installieren von Xamarin](~/get-started/installation/index.md).

1. Starten Sie Visual Studio für Mac, und erstellen Sie eine neue leere Xamarin.Forms-App mit dem Namen **LocalDatabaseTutorial**. Stellen Sie sicher, dass die App .NET Standard als Mechanismus für freigegebenen Code verwendet.

    > [!IMPORTANT]
    > Für die C#- und XAML-Codeausschnitte in diesem Tutorial ist es erforderlich, dass die Projektmappe **LocalDatabaseTutorial** genannt wird. Die Verwendung eines anderen Namens führt zu Buildfehlern, wenn Sie Code aus diesem Tutorial in die Projektmappe kopieren.

    Weitere Informationen zur .NET Standard-Bibliothek, die erstellt wird, finden Sie unter [Aufbau einer Xamarin.Forms-Anwendung](~/get-started/first-app/index.md) im Artikel [Xamarin.Forms Quickstart Deep Dive (Ausführliche Erläuterungen zum Xamarin.Forms-Schnellstart)](~/get-started/first-app/index.md).

1. Klicken Sie im **Lösungspad** mit der rechten Maustaste auf das Projekt **LocalDatabaseTutorial** und anschließend auf **Add > Add NuGet Packages...** (Hinzufügen > NuGet-Pakete hinzufügen...).

    ![Screenshot: Menüelement „Add NuGet Packages“ (NuGet-Pakete hinzufügen) ausgewählt](../images/vsmac/add-nuget-packages.png "Menüelement „Add NuGet Packages“ (NuGet-Pakete hinzufügen)")

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
