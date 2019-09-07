---
title: Erstellen neuer plattformspezifischer Bibliotheks Projekte für nuget
description: In diesem Dokument wird beschrieben, wie ein einzelnes nuget-Paket erstellt wird, das plattformspezifischen Code für mehrere Plattformen enthält.
ms.prod: xamarin
ms.assetid: D8BC4906-805F-4AFB-8D1A-88B7BF87E17F
author: conceptdev
ms.author: crdun
ms.date: 03/23/2017
ms.openlocfilehash: 73f44acad3e30e4301a69e5f2422cd4dd1a3dbf5
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70766563"
---
# <a name="creating-new-platform-specific-library-projects-for-nuget"></a>Erstellen neuer plattformspezifischer Bibliotheks Projekte für nuget

Multiplattform-Bibliotheks Projekte, die auf bestimmte Plattformen wie IOS und Android abzielen, funktionieren am besten mit freigegebenen Projekten.

Der nuget-Code kann sowohl IOS-als auch Android-spezifischen Code sowie .NET-Code enthalten, der für beides gemeinsam ist.

Mehrere Assemblys werden erstellt und in ein einzelnes nuget-Paket integriert. Mit nuget-Standards wird sichergestellt, dass das Paket allen unterstützten Projekttypen wie xamarin. IOS-und Android-Projekten hinzugefügt werden kann.

## <a name="steps-to-create-a-cross-platform-library-nuget"></a>Schritte zum Erstellen einer plattformübergreifenden Bibliothek für nuget

1. Wählen Sie **Datei > neue Projekt Mappe** aus (oder klicken Sie mit der rechten Maustaste auf eine vorhandene Projekt Mappe, und wählen Sie **Hinzufügen >**

2. Wählen Sie im Abschnitt für die **> Bibliothek für mehrere Plattformen** die Option **Multiplattform-Bibliothek**

    [![](platform-specific-images/mulitplatform-library-sml.png "Konfigurieren der Bibliothek für mehrere Plattformen für eine einzelne Codebasis")](platform-specific-images/multiplatform-library.png#lightbox)

3. Geben Sie einen **Namen** und eine **Beschreibung**ein, und wählen Sie **plattformspezifisch**:

    [![](platform-specific-images/specific-configure-sml.png "Konfigurieren einer plattformspezifischen Bibliothek für IOS und Android")](platform-specific-images/specific-configure.png#lightbox)

4. Durchlaufen Sie den Assistenten. Die folgenden Projekte werden der Projekt Mappe hinzugefügt:

    - **Android-Projekt** – Android-spezifischer Code kann optional zu diesem Projekt hinzugefügt werden.
    - **IOS-Projekt** – IOS-spezifischer Code kann optional zu diesem Projekt hinzugefügt werden.
    - **Nuget-Projekt** – diesem Projekt wird kein Code hinzugefügt. Er verweist auf die anderen Projekte und enthält die Metadatenkonfiguration für die nuget-Paketausgabe.
    - **Gemeinsam genutztes Projekt** – allgemeiner Code sollte diesem Projekt hinzugefügt werden, einschließlich Platt Form spezifischem Code in `#if` Compilerdirektiven.

5. Klicken Sie mit der rechten Maustaste auf das nuget-Projekt, und wählen Sie **Optionen**aus. Öffnen Sie dann den Abschnitt **nuget-Paket > Metadaten** , und geben Sie die [erforderlichen Metadaten](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md) (sowie alle optionalen Metadaten) ein:

    [![](platform-specific-images/specific-metadata-sml.png "Erforderliche Metadaten eingeben")](platform-specific-images/specific-metadata.png#lightbox)

6. Öffnen Sie im Fenster " **Projektoptionen** " auch den Abschnitt Verweisassemblys, und wählen Sie aus, welche PCL-Profile die freigegebene Bibliothek über "Köder und Switch" unterstützt:

    ![](platform-specific-images/specific-reference-assemblies.png "Öffnen Sie im Fenster \"Projektoptionen\" den Abschnitt Verweisassemblys, und wählen Sie aus, welche PCL-Profile die freigegebene Bibliothek über Köder und Switch unterstützt")

    > [!NOTE]
    > "Köder und Switch" bedeutet, dass die PCL-Assemblys nur die API enthalten, die von der Bibliothek verfügbar gemacht wird (der plattformspezifische Code kann nicht enthalten sein). Wenn das nuget einem xamarin-Projekt hinzugefügt wird, werden freigegebene Bibliotheken für die PCL kompiliert, aber die plattformspezifischen Assemblys enthalten den Code, der tatsächlich vom IOS-oder Android-Projekt verwendet wird.

7. Klicken Sie mit der rechten Maustaste auf das Projekt, und wählen Sie **nuget-Paket erstellen** (oder die Projekt Mappe erstellen oder bereitstellen) aus, und die **nupkg** -nuget-Paketdatei wird im Ordner **/bin/** gespeichert (je nach Konfiguration Debuggen oder freigeben).

    ![](platform-specific-images/create-nuget-package.png "Die nuget-Paketdatei wird je nach Konfiguration im bin-Ordner entweder Debuggen oder Release gespeichert.")

## <a name="verifying-the-output"></a>Überprüfen der Ausgabe

Nuget-Pakete sind auch ZIP-Dateien, sodass es möglich ist, die interne Struktur des generierten Pakets zu überprüfen.

Dieser Screenshot zeigt den Inhalt eines plattformspezifischen nuget-Inhalts, der IOS und Android unterstützt und zwei Verweisassemblys ausgewählt hat:

![](platform-specific-images/nuget-output.png "Im nuget-Paket enthaltene Dateien")

## <a name="related-links"></a>Verwandte Links

- [Leitfaden zu Metadaten](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md)
