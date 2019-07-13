---
title: Erstellen von NuGet über vorhandene Bibliotheksprojekte
description: 'Dieses Dokument beschreibt die Vorgehensweise: erstellen ein NuGet-Pakets aus einem vorhandenen Library-Projekt, sodass der Code für andere Entwickler freigegeben werden.'
ms.prod: xamarin
ms.assetid: EDAC3E5E-DB7D-40A9-AE28-45C52ADA854E
author: asb3993
ms.author: amburns
ms.date: 04/20/2017
ms.openlocfilehash: 6e043334d3ca45a573423ebdfdf1ec9149167b55
ms.sourcegitcommit: 7ccc7a9223cd1d3c42cd03ddfc28050a8ea776c2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/13/2019
ms.locfileid: "67864692"
---
# <a name="creating-a-nuget-from-existing-library-projects"></a>Erstellen von NuGet über vorhandene Bibliotheksprojekte

Vorhandener Plc oder .NET Standard-Bibliotheken können aktiviert werden, in der NuGet-Pakete über die **Projektoptionen** Fenster:

1. Mit der rechten Maustaste auf das Bibliotheksprojekt in der **Lösungspad** , und wählen Sie **Optionen**.

2. Wechseln Sie zu der **NuGet-Pakets > Metadaten** aus, und geben Sie alle der [erforderlichen Informationen](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md) in der **allgemeine** Registerkarte:

   [![](existing-library-images/existing-metadata-sml.png "Geben Sie die erforderlichen Metadaten")](existing-library-images/existing-metadata.png#lightbox)

3. Optional [Hinzufügen von zusätzlichen Metadaten](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md) in die **Details** Registerkarte.

4. Sobald die Metadaten konfiguriert ist, können Sie mit der rechten Maustaste auf das Projekt und wählen **NuGet-Paket erstellen** und die **NUPKG** werden im NuGet-Paketdatei gespeichert der **/bin /** Ordner (Debug oder Release, je nach Konfiguration).

   ![](existing-library-images/create-nuget-package.png "Wählen Sie mit der rechten Maustaste im NuGet-Paket erstellen")

5. Erstellen Sie das NuGet-Paket auf _jeder_ erstellen oder bereitstellen, navigieren Sie zu der **NuGet-Paket > Erstellen** Abschnitt und -Tick **erstellen Sie ein NuGet-Paket beim Erstellen des Projekts**:

    [![](existing-library-images/existing-tickbox-sml.png "Aktivieren Sie zum Erstellen eines NuGet-Pakets")](existing-library-images/existing-tickbox.png#lightbox)

> [!NOTE]
> Erstellen des NuGet-Pakets kann den Buildprozess Pakets verlangsamen. Wenn Sie dieses Kontrollkästchen nicht ausgewählt ist, können Sie ein NuGet-Paket weiterhin manuell zu einem beliebigen Zeitpunkt im Kontextmenü "Projekt" (in Schritt 4 oben gezeigt) generieren.

## <a name="verifying-the-output"></a>Überprüfen Sie die Ausgabe

NuGet-Pakete sind auch die ZIP-Dateien, daher ist es möglich, die interne Struktur des generierten Pakets zu überprüfen.

Dieser Screenshot zeigt den Inhalt eines PCL-basierten NuGet – nur eine einzelne PCL-Assembly enthalten ist:

![](existing-library-images/nuget-output.png "Im NuGet-Paket enthaltenen Dateien")


## <a name="related-links"></a>Verwandte Links

- [Leitfaden zu Metadaten](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md)
