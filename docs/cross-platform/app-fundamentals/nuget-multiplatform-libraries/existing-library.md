---
title: Erstellen eines nuget aus vorhandenen Bibliotheks Projekten
description: In diesem Dokument wird beschrieben, wie ein nuget-Paket aus einem vorhandenen Bibliotheksprojekt erstellt wird, sodass der Code für andere Entwickler freigegeben werden kann.
ms.prod: xamarin
ms.assetid: EDAC3E5E-DB7D-40A9-AE28-45C52ADA854E
author: davidortinau
ms.author: daortin
ms.date: 04/20/2017
ms.openlocfilehash: 30158056b8adbdd5aba4cc311220a22502ea9968
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73016783"
---
# <a name="creating-a-nuget-from-existing-library-projects"></a>Erstellen eines nuget aus vorhandenen Bibliotheks Projekten

Vorhandene PCL-oder .NET Standard Bibliotheken können über das Fenster " **Projektoptionen** " in nugets umgewandelt werden:

1. Klicken Sie mit der rechten Maustaste auf das Bibliotheksprojekt im **Lösungspad** , und wählen Sie **Optionen**aus.

2. Wechseln Sie zum Abschnitt **nuget-Paket > Metadaten** , und geben Sie alle [erforderlichen Informationen](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md) auf der Registerkarte **Allgemein** ein:

   [![](existing-library-images/existing-metadata-sml.png "Enter required metadata")](existing-library-images/existing-metadata.png#lightbox)

3. [Fügen Sie optional zusätzliche Metadaten](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md) auf der Registerkarte **Details** hinzu.

4. Nachdem die Metadaten konfiguriert wurden, können Sie mit der rechten Maustaste auf das Projekt klicken und **nuget-Paket erstellen** auswählen, und die **nupkg** -nuget-Paketdatei wird im Ordner **/bin/** (abhängig von der Konfiguration) gespeichert.

   ![](existing-library-images/create-nuget-package.png "Choose Create NuGet Package from the right-click menu")

5. Um das nuget-Paket für _jeden_ Build oder jede Bereitstellung zu erstellen, wechseln Sie zum Abschnitt **nuget-Paket >** erstellen, und **Erstellen Sie ein nuget-Paket, wenn Sie das Projekt**erstellen:

    [![](existing-library-images/existing-tickbox-sml.png "Tick to create a NuGet package")](existing-library-images/existing-tickbox.png#lightbox)

> [!NOTE]
> Wenn Sie das nuget-Paket erstellen, kann der Buildprozess verlangsamt werden. Wenn dieses Kontrollkästchen nicht angezeigt wird, können Sie immer noch manuell ein nuget-Paket über das Projektkontext Menü (siehe Schritt 4 oben) generieren.

## <a name="verifying-the-output"></a>Überprüfen der Ausgabe

Nuget-Pakete sind auch ZIP-Dateien, sodass es möglich ist, die interne Struktur des generierten Pakets zu überprüfen.

Dieser Screenshot zeigt den Inhalt eines PCL-basierten nuget-– es ist nur eine einzige PCL-Assembly enthalten:

![](existing-library-images/nuget-output.png "Files contained in the NuGet package")

## <a name="related-links"></a>Verwandte Links

- [Leitfaden zu Metadaten](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md)
