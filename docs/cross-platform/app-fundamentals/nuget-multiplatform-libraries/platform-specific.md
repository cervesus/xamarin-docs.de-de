---
title: Erstellen neuer plattformspezifischer Bibliotheks Projekte für nuget
description: In diesem Dokument wird beschrieben, wie ein einzelnes nuget-Paket erstellt wird, das plattformspezifischen Code für mehrere Plattformen enthält.
ms.prod: xamarin
ms.assetid: D8BC4906-805F-4AFB-8D1A-88B7BF87E17F
author: davidortinau
ms.author: daortin
ms.date: 03/23/2017
ms.openlocfilehash: b1c32d76f4f25298a8f0643e2e29707df7775736
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86939234"
---
# <a name="creating-new-platform-specific-library-projects-for-nuget"></a>Erstellen neuer plattformspezifischer Bibliotheks Projekte für nuget

Multiplattform-Bibliotheks Projekte, die auf bestimmte Plattformen wie IOS und Android abzielen, funktionieren am besten mit freigegebenen Projekten.

Der nuget-Code kann sowohl IOS-als auch Android-spezifischen Code sowie .NET-Code enthalten, der für beides gemeinsam ist.

Mehrere Assemblys werden erstellt und in ein einzelnes nuget-Paket integriert. Mit nuget-Standards wird sichergestellt, dass das Paket allen unterstützten Projekttypen wie xamarin. IOS-und Android-Projekten hinzugefügt werden kann.

## <a name="steps-to-create-a-cross-platform-library-nuget"></a>Schritte zum Erstellen einer plattformübergreifenden Bibliothek für nuget

1. Wählen Sie **Datei > neue Projekt Mappe** aus (oder klicken Sie mit der rechten Maustaste auf eine vorhandene Projekt Mappe, und wählen Sie **Hinzufügen >**

2. Wählen Sie im Abschnitt für die **> Bibliothek für mehrere Plattformen** die Option **Multiplattform-Bibliothek**

    [![Konfigurieren der Bibliothek für mehrere Plattformen für eine einzelne Codebasis](platform-specific-images/mulitplatform-library-sml.png)](platform-specific-images/multiplatform-library.png#lightbox)

3. Geben Sie einen **Namen** und eine **Beschreibung**ein, und wählen Sie **plattformspezifisch**:

    [![Konfigurieren einer plattformspezifischen Bibliothek für IOS und Android](platform-specific-images/specific-configure-sml.png)](platform-specific-images/specific-configure.png#lightbox)

4. Schließen Sie den Assistenten ab. Die folgenden Projekte werden der Projekt Mappe hinzugefügt:

    - **Android-Projekt** – Android-spezifischer Code kann optional zu diesem Projekt hinzugefügt werden.
    - **IOS-Projekt** – IOS-spezifischer Code kann optional zu diesem Projekt hinzugefügt werden.
    - **Nuget-Projekt** – diesem Projekt wird kein Code hinzugefügt. Er verweist auf die anderen Projekte und enthält die Metadatenkonfiguration für die nuget-Paketausgabe.
    - **Gemeinsam genutztes Projekt** – allgemeiner Code sollte diesem Projekt hinzugefügt werden, einschließlich Platt Form spezifischem Code in `#if` Compilerdirektiven.

5. Klicken Sie mit der rechten Maustaste auf das nuget-Projekt, und wählen Sie **Optionen**aus. Öffnen Sie dann den Abschnitt **nuget-Paket > Metadaten** , und geben Sie die [erforderlichen Metadaten](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md) (sowie alle optionalen Metadaten) ein:

    [![Erforderliche Metadaten eingeben](platform-specific-images/specific-metadata-sml.png)](platform-specific-images/specific-metadata.png#lightbox)

6. Öffnen Sie im Fenster " **Projektoptionen** " auch den Abschnitt Verweisassemblys, und wählen Sie aus, welche PCL-Profile die freigegebene Bibliothek über "Köder und Switch" unterstützt: **Reference Assemblies**

    ![Öffnen Sie im Fenster "Projektoptionen" den Abschnitt Verweisassemblys, und wählen Sie aus, welche PCL-Profile die freigegebene Bibliothek über Köder und Switch unterstützt](platform-specific-images/specific-reference-assemblies.png)

    > [!NOTE]
    > "Köder und Switch" bedeutet, dass die PCL-Assemblys nur die API enthalten, die von der Bibliothek verfügbar gemacht wird (der plattformspezifische Code kann nicht enthalten sein). Wenn das nuget einem xamarin-Projekt hinzugefügt wird, werden freigegebene Bibliotheken für die PCL kompiliert, aber die plattformspezifischen Assemblys enthalten den Code, der tatsächlich vom IOS-oder Android-Projekt verwendet wird.

7. Klicken Sie mit der rechten Maustaste auf das Projekt, und wählen Sie **nuget-Paket erstellen** (oder die Projekt Mappe erstellen oder bereitstellen) aus, und die **nupkg** -nuget-Paketdatei wird im Ordner **/bin/** gespeichert (je nach Konfiguration Debuggen oder freigeben).

    ![Die nuget-Paketdatei wird je nach Konfiguration im bin-Ordner entweder Debuggen oder Release gespeichert.](platform-specific-images/create-nuget-package.png)

## <a name="verifying-the-output"></a>Überprüfen der Ausgabe

Nuget-Pakete sind auch ZIP-Dateien, sodass es möglich ist, die interne Struktur des generierten Pakets zu überprüfen.

Dieser Screenshot zeigt den Inhalt eines plattformspezifischen nuget-Inhalts, der IOS und Android unterstützt und zwei Verweisassemblys ausgewählt hat:

![Im nuget-Paket enthaltene Dateien](platform-specific-images/nuget-output.png)

## <a name="related-links"></a>Verwandte Links

- [Metadatenhandbuch](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md)
