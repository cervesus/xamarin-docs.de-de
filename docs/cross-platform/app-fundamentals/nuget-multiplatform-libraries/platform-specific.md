---
title: Erstellen neuer plattformspezifischer Bibliotheks Projekte für nuget
description: In diesem Dokument wird beschrieben, wie ein einzelnes nuget-Paket erstellt wird, das plattformspezifischen Code für mehrere Plattformen enthält.
ms.prod: xamarin
ms.assetid: D8BC4906-805F-4AFB-8D1A-88B7BF87E17F
author: davidortinau
ms.author: daortin
ms.date: 03/23/2017
ms.openlocfilehash: 925e08c600c695640c927ada26df376a252b3927
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73016722"
---
# <a name="creating-new-platform-specific-library-projects-for-nuget"></a>Erstellen neuer plattformspezifischer Bibliotheks Projekte für nuget

Multiplattform-Bibliotheks Projekte, die auf bestimmte Plattformen wie IOS und Android abzielen, funktionieren am besten mit freigegebenen Projekten.

Der nuget-Code kann sowohl IOS-als auch Android-spezifischen Code sowie .NET-Code enthalten, der für beides gemeinsam ist.

Mehrere Assemblys werden erstellt und in ein einzelnes nuget-Paket integriert. Mit nuget-Standards wird sichergestellt, dass das Paket allen unterstützten Projekttypen wie xamarin. IOS-und Android-Projekten hinzugefügt werden kann.

## <a name="steps-to-create-a-cross-platform-library-nuget"></a>Schritte zum Erstellen einer plattformübergreifenden Bibliothek für nuget

1. Wählen Sie **Datei > neue Projekt Mappe** aus (oder klicken Sie mit der rechten Maustaste auf eine vorhandene Projekt Mappe, und wählen Sie **Hinzufügen >**

2. Wählen Sie im Abschnitt für die **> Bibliothek für mehrere Plattformen** die Option **Multiplattform-Bibliothek**

    [![](platform-specific-images/mulitplatform-library-sml.png "Configure multi-platform library for a single code base")](platform-specific-images/multiplatform-library.png#lightbox)

3. Geben Sie einen **Namen** und eine **Beschreibung**ein, und wählen Sie **plattformspezifisch**:

    [![](platform-specific-images/specific-configure-sml.png "Configure platform-specific library for iOS and Android")](platform-specific-images/specific-configure.png#lightbox)

4. Durchlaufen Sie den Assistenten. Die folgenden Projekte werden der Projekt Mappe hinzugefügt:

    - **Android-Projekt** – Android-spezifischer Code kann optional zu diesem Projekt hinzugefügt werden.
    - **IOS-Projekt** – IOS-spezifischer Code kann optional zu diesem Projekt hinzugefügt werden.
    - **Nuget-Projekt** – diesem Projekt wird kein Code hinzugefügt. Er verweist auf die anderen Projekte und enthält die Metadatenkonfiguration für die nuget-Paketausgabe.
    - **Gemeinsam genutztes Projekt** – allgemeiner Code sollte diesem Projekt hinzugefügt werden, einschließlich Platt Form spezifischem Code in `#if` Compilerdirektiven.

5. Klicken Sie mit der rechten Maustaste auf das nuget-Projekt, und wählen Sie **Optionen**aus. Öffnen Sie dann den Abschnitt **nuget-Paket > Metadaten** , und geben Sie die [erforderlichen Metadaten](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md) (sowie alle optionalen Metadaten) ein:

    [![](platform-specific-images/specific-metadata-sml.png "Enter required metadata")](platform-specific-images/specific-metadata.png#lightbox)

6. Öffnen Sie im Fenster " **Projektoptionen** " auch den Abschnitt Verweisassemblys, und wählen Sie aus, welche PCL-Profile die freigegebene Bibliothek über "Köder und Switch" unterstützt:

    ![](platform-specific-images/specific-reference-assemblies.png "Also in the Project Options window, open the Reference Assemblies section and choose   which PCL profiles the shared library will support via bait and switch")

    > [!NOTE]
    > "Köder und Switch" bedeutet, dass die PCL-Assemblys nur die API enthalten, die von der Bibliothek verfügbar gemacht wird (der plattformspezifische Code kann nicht enthalten sein). Wenn das nuget einem xamarin-Projekt hinzugefügt wird, werden freigegebene Bibliotheken für die PCL kompiliert, aber die plattformspezifischen Assemblys enthalten den Code, der tatsächlich vom IOS-oder Android-Projekt verwendet wird.

7. Klicken Sie mit der rechten Maustaste auf das Projekt, und wählen Sie **nuget-Paket erstellen** (oder die Projekt Mappe erstellen oder bereitstellen) aus, und die **nupkg** -nuget-Paketdatei wird im Ordner **/bin/** gespeichert (je nach Konfiguration Debuggen oder freigeben).

    ![](platform-specific-images/create-nuget-package.png "NuGet package file will be saved in the bin folder either Debug or Release, depending on configuration")

## <a name="verifying-the-output"></a>Überprüfen der Ausgabe

Nuget-Pakete sind auch ZIP-Dateien, sodass es möglich ist, die interne Struktur des generierten Pakets zu überprüfen.

Dieser Screenshot zeigt den Inhalt eines plattformspezifischen nuget-Inhalts, der IOS und Android unterstützt und zwei Verweisassemblys ausgewählt hat:

![](platform-specific-images/nuget-output.png "Files contained in the NuGet package")

## <a name="related-links"></a>Verwandte Links

- [Leitfaden zu Metadaten](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md)
