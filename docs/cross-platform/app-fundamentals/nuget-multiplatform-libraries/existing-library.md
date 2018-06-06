---
title: Erstellen ein NuGet aus vorhandenen Bibliotheksprojekte
description: Dieses Dokument beschreibt das Erstellen von NuGet-Paket aus einem vorhandenen Library-Projekt, sodass der Code mit anderen Entwicklern gemeinsam genutzt werden.
ms.prod: xamarin
ms.assetid: EDAC3E5E-DB7D-40A9-AE28-45C52ADA854E
author: asb3993
ms.author: amburns
ms.date: 04/20/2017
ms.openlocfilehash: 7f407b22d1793d585ae40aeae8c2d9b7616784e6
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34779982"
---
# <a name="creating-a-nuget-from-existing-library-projects"></a>Erstellen ein NuGet aus vorhandenen Bibliotheksprojekte

Vorhandene PCL "oder" Standard ".NET Bibliotheken aktiviert werden können, in NuGets über die **Projektoptionen** Fenster:

1. Mit der rechten Maustaste auf das Steuerelementbibliothek-Projekt in der **Lösung Pad** , und wählen Sie **Optionen**.

2. Wechseln Sie zu der **NuGet-Paket > Metadaten** Abschnitt, und geben Sie alle der [erforderlichen Informationen](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md) in der **allgemeine** Registerkarte:

  [![](existing-library-images/existing-metadata-sml.png "Geben Sie die erforderlichen Metadaten")](existing-library-images/existing-metadata.png#lightbox)

3. Optional, [fügen Sie zusätzliche Metadaten](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md) in der **Details** Registerkarte.

4. Sobald die Metadaten konfiguriert ist, können Sie mit der rechten Maustaste auf das Projekt und wählen Sie **NuGet-Pakete erstellen** und **.nupkg** NuGet-Paket-Datei wird gespeichert, der **/bin/** Ordner (Debug oder Release, je nach Konfiguration).

  ![](existing-library-images/create-nuget-package.png "Wählen Sie mit der rechten Maustaste auf NuGet-Pakete erstellen")

5. So erstellen das NuGet-Paket auf _jeder_ erstellen oder bereitstellen, wechseln Sie zu der **NuGet-Paket > Erstellen** Abschnitt und Teilstriche **erstellen Sie ein NuGet-Paket beim Erstellen des Projekts**:

    [![](existing-library-images/existing-tickbox-sml.png "Aktivieren Sie zum Erstellen eines NuGet-Pakets")](existing-library-images/existing-tickbox.png#lightbox)

> [!NOTE]
> Erstellen das NuGet kann während des Erstellungsprozesses Pakets verlangsamen. Wenn Sie dieses Kontrollkästchen nicht aktiviert ist, können Sie ein NuGet-Paket weiterhin manuell zu einem beliebigen Zeitpunkt im Projekt-Kontextmenü (in Schritt 4 oben gezeigt) generieren.

## <a name="verifying-the-output"></a>Überprüfen Sie die Ausgabe

NuGet-Pakete sind auch ZIP-Dateien, daher ist es möglich, die interne Struktur der generierten Paket zu überprüfen.

Diese bildschirmabbildung zeigt den Inhalt von einem PCL-basierte NuGet – nur eine einzelne PCL-Assembly enthalten ist:

![](existing-library-images/nuget-output.png "Dateien in das NuGet-Paket")


## <a name="related-links"></a>Verwandte Links

- [Leitfaden zu Metadaten](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md)
