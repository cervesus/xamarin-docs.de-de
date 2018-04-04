---
title: Erstellen von neuen plattformspezifischen-Bibliotheksprojekte für NuGet
ms.prod: xamarin
ms.assetid: D8BC4906-805F-4AFB-8D1A-88B7BF87E17F
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 0f244e614a40e444139d51a9466ccc7225a7fe68
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="creating-new-platform-specific-library-projects-for-nuget"></a>Erstellen von neuen plattformspezifischen-Bibliotheksprojekte für NuGet

Multiplattform Library-Projekte, die auf bestimmte Plattformen wie iOS und Android, abzielen funktionieren am besten mit Projekten gemeinsam genutzt.

Die NuGet kann sowohl iOS und Android-spezifischen Code als auch .NET Code sowohl enthalten.

Mehrere Assemblys erstellt und in einem einzelnen NuGet-Paket integriert. NuGet-Standards stellen Sie sicher, dass das Paket für alle unterstützten Projekttypen, z. B. Xamarin IOS- und Android-Projekte hinzugefügt werden kann.

## <a name="steps-to-create-a-cross-platform-library-nuget"></a>Schritte zum Erstellen eine plattformübergreifende Bibliothek NuGet

1. Wählen Sie **Datei > neue Projektmappe** (oder klicken Sie mit der rechten Maustaste auf einer vorhandenen Projektmappe, und wählen Sie **hinzufügen > Neues Projekt**).

2. Wählen Sie **Bibliothek für mehrere Plattformen** aus der **mehrere Plattformen > Bibliothek** Abschnitt:

  [![](platform-specific-images/mulitplatform-library-sml.png "Konfigurieren von Multi-Plattform-Bibliothek für einer einzelnen Codebasis")](platform-specific-images/multiplatform-library.png#lightbox)

3. Geben Sie einen **Namen** und **Beschreibung**, und wählen Sie **Clientplattform spezifische**:

  [![](platform-specific-images/specific-configure-sml.png "Konfigurieren Sie die plattformspezifischen-Bibliothek für iOS und Android")](platform-specific-images/specific-configure.png#lightbox)

4. Durchlaufen Sie den Assistenten. Die folgenden Projekte werden der Projektmappe hinzugefügt:

  - **Android-Projekts** – Android-spezifischen Code kann optional auf dieses Projekt hinzugefügt werden.
  - **iOS-Projekt** – iOS-spezifischen Code kann optional auf dieses Projekt hinzugefügt werden.
  - **NuGet-Projekt** – kein Code auf dieses Projekt hinzugefügt. Er verweist auf andere Projekte, und enthält die Metadatenkonfiguration für die Ausgabe des NuGet-Paket.
  - **Freigegebene Projekt** – gemeinsamer Code der einschließlich plattformspezifischen Code innerhalb dieses Projekts hinzugefügt werden sollen `#if` Compilerdirektiven.

5. Mit der rechten Maustaste auf das NuGet-Projekt, und wählen Sie **Optionen**, öffnen Sie dann die **NuGet-Paket > Metadaten** Abschnitt, und geben Sie die [erforderlichen Metadaten](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md) (als sowie ggf. optional Metadaten):

  [![](platform-specific-images/specific-metadata-sml.png "Geben Sie die erforderlichen Metadaten")](platform-specific-images/specific-metadata.png#lightbox)

6. Auch in der **Projektoptionen** geöffnete Fenster die **Verweisassemblys** Abschnitt, und wählen Sie die PCL-Profile, die die freigegebene Bibliothek über "Köder And Switch" unterstützt:

  ![](platform-specific-images/specific-reference-assemblies.png "Auch Sie im Fenster Projektoptionen, öffnen Sie den Abschnitt Verweisassemblys und wählen Sie die PCL-Profile, die die freigegebene Bibliothek über Köder And Switch unterstützt wird")

  > [!NOTE]
> "Köder And Switch" bedeutet, dass die PCL-Assemblys nur enthalten, werden die API, die von der Bibliothek (es kann keine der plattformspezifischen Code enthalten) verfügbar gemacht werden. Bei einem Xamarin-Projekt das NuGet hinzugefügt wird, Generieren von freigegebene Bibliotheken werden anhand der PCL kompiliert, aber die plattformspezifischen Assemblys enthalten den Code, der tatsächlich vom iOS oder Android-Projekt verwendet wird.

7. Mit der rechten Maustaste auf das Projekt, und wählen Sie **NuGet-Pakete erstellen** (oder erstellen oder Bereitstellen der Projektmappe) und die **.nupkg** NuGet-Paket-Datei wird gespeichert, der **/bin/** Ordner ( einer Debug- oder Releasekonfiguration, je nach Konfiguration).

  ![](platform-specific-images/create-nuget-package.png "NuGet-Paket-Datei wird im Ordner "Bin" einer Debug- oder Releasekonfiguration, je nach Konfiguration gespeichert werden")


## <a name="verifying-the-output"></a>Überprüfen Sie die Ausgabe

NuGet-Pakete sind auch ZIP-Dateien, daher ist es möglich, die interne Struktur der generierten Paket zu überprüfen.

Dieser Screenshot zeigt den Inhalt von einem plattformspezifischen NuGet, die IOS- und Android unterstützt, jedoch zwei Verweisassemblys ausgewählt:

![](platform-specific-images/nuget-output.png "Dateien in das NuGet-Paket")


## <a name="related-links"></a>Verwandte Links

- [Leitfaden zu Metadaten](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md)
