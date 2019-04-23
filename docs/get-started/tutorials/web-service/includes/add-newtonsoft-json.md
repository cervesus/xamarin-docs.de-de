---
ms.openlocfilehash: aee7c7eb494af76bb450038453c19ee1ed83f2d3
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61388820"
---
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Starten Sie Visual Studio, und erstellen Sie eine neue leere Xamarin.Forms-App mit dem Namen **WebServiceTutorial**. Stellen Sie sicher, dass die App .NET Standard als Mechanismus für freigegebenen Code verwendet.

    > [!IMPORTANT]
    > Für die C#- und XAML-Codeausschnitte in diesem Tutorial ist es erforderlich, dass die Projektmappe **WebServiceTutorial** genannt wird. Die Verwendung eines anderen Namens führt zu Buildfehlern, wenn Sie Code aus diesem Tutorial in die Projektmappe kopieren.

    Weitere Informationen zur .NET Standard-Bibliothek, die erstellt wird, finden Sie unter [Aufbau einer Xamarin.Forms-Anwendung](~/get-started/first-app/index.md) im Artikel [Xamarin.Forms Quickstart Deep Dive (Ausführliche Erläuterungen zum Xamarin.Forms-Schnellstart)](~/get-started/first-app/index.md).

1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt **WebServiceTutorial** und anschließend auf **NuGet-Pakete verwalten...**:

    ![Screenshot: ausgewähltes Menüelement „NuGet-Pakete verwalten...“](../images/vs/add-nuget-packages.png "Menüelement „NuGet-Pakete verwalten...“")

1. Klicken Sie im **NuGet-Paket-Manager** auf die Registerkarte **Durchsuchen**, suchen Sie nach dem NuGet-Paket **Newtonsoft.Json**, wählen Sie es aus, und klicken Sie auf die Schaltfläche **Installieren**, um es dem Projekt hinzuzufügen:

    ![Screenshot: NuGet-Paket „Newtonsoft.Json “ im NuGet-Paket-Manager](../images/vs/add-package.png "NuGet-Paket „Newtonsoft.Json“")

    Mit diesem Paket kann die JSON-Deserialisierung in der Anwendung verwendet werden.

1. Erstellen Sie die Projektmappe, um sich zu vergewissern, dass keine Fehler vorliegen.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

1. Starten Sie Visual Studio für Mac, und erstellen Sie eine neue leere Xamarin.Forms-App mit dem Namen **WebServiceTutorial**. Stellen Sie sicher, dass die App .NET Standard als Mechanismus für freigegebenen Code verwendet.

    > [!IMPORTANT]
    > Für die C#- und XAML-Codeausschnitte in diesem Tutorial ist es erforderlich, dass die Projektmappe **WebServiceTutorial** genannt wird. Die Verwendung eines anderen Namens führt zu Buildfehlern, wenn Sie Code aus diesem Tutorial in die Projektmappe kopieren.

    Weitere Informationen zur .NET Standard-Bibliothek, die erstellt wird, finden Sie unter [Aufbau einer Xamarin.Forms-Anwendung](~/get-started/first-app/index.md) im Artikel [Xamarin.Forms Quickstart Deep Dive (Ausführliche Erläuterungen zum Xamarin.Forms-Schnellstart)](~/get-started/first-app/index.md).

1. Klicken Sie im **Lösungspad** mit der rechten Maustaste auf das Projekt **WebServiceTutorial** und anschließend auf **Add > Add NuGet Packages...** (Hinzufügen > NuGet-Pakete hinzufügen...):

    ![Screenshot: ausgewähltes Menüelement „NuGet-Pakete verwalten...“](../images/vsmac/add-nuget-packages.png "Menüelement „NuGet-Pakete verwalten...“")

1. Suchen Sie im Fenster **Pakete hinzufügen** nach dem NuGet-Paket **Newtonsoft.Json**, wählen Sie es aus, und klicken Sie auf **Paket hinzufügen**, um es dem Projekt hinzuzufügen:

    ![Screenshot: NuGet-Paket „Newtonsoft.Json “ im NuGet-Paket-Manager](../images/vsmac/add-package.png "NuGet-Paket „Newtonsoft.Json“")

    Mit diesem Paket kann die JSON-Deserialisierung in der Anwendung verwendet werden.

1. Erstellen Sie die Projektmappe, um sich zu vergewissern, dass keine Fehler vorliegen.
