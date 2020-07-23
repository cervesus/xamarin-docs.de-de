---
title: Erstellen eines nuget aus vorhandenen Bibliotheks Projekten
description: In diesem Dokument wird beschrieben, wie ein nuget-Paket aus einem vorhandenen Bibliotheksprojekt erstellt wird, sodass der Code für andere Entwickler freigegeben werden kann.
ms.prod: xamarin
ms.assetid: EDAC3E5E-DB7D-40A9-AE28-45C52ADA854E
author: davidortinau
ms.author: daortin
ms.date: 04/20/2017
ms.openlocfilehash: 058904236e100a670e8a38417dcaec805178d6d2
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86936797"
---
# <a name="creating-a-nuget-from-existing-library-projects"></a>Erstellen eines nuget aus vorhandenen Bibliotheks Projekten

Vorhandene PCL-oder .NET Standard Bibliotheken können über das Fenster " **Projektoptionen** " in nugets umgewandelt werden:

1. Klicken Sie mit der rechten Maustaste auf das Bibliotheksprojekt im **Lösungspad** , und wählen Sie **Optionen**aus.

2. Wechseln Sie zum Abschnitt **nuget-Paket > Metadaten** , und geben Sie alle [erforderlichen Informationen](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md) auf der Registerkarte **Allgemein** ein:

   [![Erforderliche Metadaten eingeben](existing-library-images/existing-metadata-sml.png)](existing-library-images/existing-metadata.png#lightbox)

3. [Fügen Sie optional zusätzliche Metadaten](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md) auf der Registerkarte **Details** hinzu.

4. Nachdem die Metadaten konfiguriert wurden, können Sie mit der rechten Maustaste auf das Projekt klicken und **nuget-Paket erstellen** auswählen, und die **nupkg** -nuget-Paketdatei wird im Ordner **/bin/** (abhängig von der Konfiguration) gespeichert.

   ![Klicken Sie im Kontextmenü auf nuget-Paket erstellen.](existing-library-images/create-nuget-package.png)

5. Um das nuget-Paket für _jeden_ Build oder jede Bereitstellung zu erstellen, wechseln Sie zum Abschnitt **nuget-Paket >** erstellen, und **Erstellen Sie ein nuget-Paket, wenn Sie das Projekt**erstellen:

    [![Tick zum Erstellen eines nuget-Pakets](existing-library-images/existing-tickbox-sml.png)](existing-library-images/existing-tickbox.png#lightbox)

> [!NOTE]
> Wenn Sie das nuget-Paket erstellen, kann der Buildprozess verlangsamt werden. Wenn dieses Kontrollkästchen nicht angezeigt wird, können Sie immer noch manuell ein nuget-Paket über das Projektkontext Menü (siehe Schritt 4 oben) generieren.

## <a name="verifying-the-output"></a>Überprüfen der Ausgabe

Nuget-Pakete sind auch ZIP-Dateien, sodass es möglich ist, die interne Struktur des generierten Pakets zu überprüfen.

Dieser Screenshot zeigt den Inhalt eines PCL-basierten nuget-– es ist nur eine einzige PCL-Assembly enthalten:

![Im nuget-Paket enthaltene Dateien](existing-library-images/nuget-output.png)

## <a name="related-links"></a>Ähnliche Themen

- [Metadatenhandbuch](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md)
