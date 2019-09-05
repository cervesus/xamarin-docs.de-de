---
title: Erstellen eines nuget aus vorhandenen Bibliotheks Projekten
description: In diesem Dokument wird beschrieben, wie ein nuget-Paket aus einem vorhandenen Bibliotheksprojekt erstellt wird, sodass der Code für andere Entwickler freigegeben werden kann.
ms.prod: xamarin
ms.assetid: EDAC3E5E-DB7D-40A9-AE28-45C52ADA854E
author: conceptdev
ms.author: crdun
ms.date: 04/20/2017
ms.openlocfilehash: f9d49fc4bff91939c9924dc42a11ef31ffd87362
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70289222"
---
# <a name="creating-a-nuget-from-existing-library-projects"></a>Erstellen eines nuget aus vorhandenen Bibliotheks Projekten

Vorhandene PCL-oder .NET Standard Bibliotheken können über das Fenster " **Projektoptionen** " in nugets umgewandelt werden:

1. Klicken Sie mit der rechten Maustaste auf das Bibliotheksprojekt im **Lösungspad** , und wählen Sie **Optionen**aus.

2. Wechseln Sie zum Abschnitt **nuget-Paket > Metadaten** , und geben Sie alle [erforderlichen Informationen](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md) auf der Registerkarte **Allgemein** ein:

   [![](existing-library-images/existing-metadata-sml.png "Erforderliche Metadaten eingeben")](existing-library-images/existing-metadata.png#lightbox)

3. [Fügen Sie optional zusätzliche Metadaten](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md) auf der Registerkarte **Details** hinzu.

4. Nachdem die Metadaten konfiguriert wurden, können Sie mit der rechten Maustaste auf das Projekt klicken und **nuget-Paket erstellen** auswählen, und die **nupkg** -nuget-Paketdatei wird im Ordner **/bin/** (abhängig von der Konfiguration) gespeichert.

   ![](existing-library-images/create-nuget-package.png "Klicken Sie im Kontextmenü auf nuget-Paket erstellen.")

5. Um das nuget-Paket für _jeden_ Build oder jede Bereitstellung zu erstellen, wechseln Sie zum Abschnitt **nuget-Paket >** erstellen, und **Erstellen Sie ein nuget-Paket, wenn Sie das Projekt**erstellen:

    [![](existing-library-images/existing-tickbox-sml.png "Tick zum Erstellen eines nuget-Pakets")](existing-library-images/existing-tickbox.png#lightbox)

> [!NOTE]
> Wenn Sie das nuget-Paket erstellen, kann der Buildprozess verlangsamt werden. Wenn dieses Kontrollkästchen nicht angezeigt wird, können Sie immer noch manuell ein nuget-Paket über das Projektkontext Menü (siehe Schritt 4 oben) generieren.

## <a name="verifying-the-output"></a>Überprüfen der Ausgabe

Nuget-Pakete sind auch ZIP-Dateien, sodass es möglich ist, die interne Struktur des generierten Pakets zu überprüfen.

Dieser Screenshot zeigt den Inhalt eines PCL-basierten nuget-– es ist nur eine einzige PCL-Assembly enthalten:

![](existing-library-images/nuget-output.png "Im nuget-Paket enthaltene Dateien")


## <a name="related-links"></a>Verwandte Links

- [Leitfaden zu Metadaten](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md)
