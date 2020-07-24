---
title: Erstellen einer neuen Bibliothek für mehrere Plattformen für nuget
description: In diesem Dokument wird beschrieben, wie Sie eine Bibliothek für mehrere Plattformen für die Verwendung mit nuget erstellen. Dieses Verfahren eignet sich für Geschäftslogik und Algorithmen, die vollständig in der .net-Basisklassen Bibliothek ausgedrückt werden können und daher auf allen Zielplattformen ohne plattformspezifischen Code ausgeführt werden können.
ms.prod: xamarin
ms.assetid: E7B55354-9BBE-4122-BCE3-3506B79090DD
author: davidortinau
ms.author: daortin
ms.date: 03/23/2017
ms.openlocfilehash: 3226820dddbd6ecb83b87b29ef1991d19104b2a6
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86936980"
---
# <a name="creating-a-new-multiplatform-library-for-nuget"></a>Erstellen einer neuen Bibliothek für mehrere Plattformen für nuget

Wenn Sie ein Multiplatform-Bibliotheksprojekt erstellen, das PCL oder .NET Standard verwendet, bedeutet dies, dass das resultierende nuget zu jedem .net-Projekt hinzugefügt werden kann, das das Ziel Profil unterstützt, einschließlich ASP.NET-Projekten oder Desktop-Apps, die WinForms, WPF oder UWP verwenden.

Die Bibliothek kann nur Code enthalten, der von der ausgewählten PCL oder .NET Standard Profil sowie von allen hinzugefügten nugets unterstützt wird.
Dies eignet sich für Geschäftslogik und Algorithmen, die vollständig in der .net-Basisklassen Bibliothek ausgedrückt werden können.

Eine einzelne Assembly wird erstellt und in ein nuget-Paket integriert.

Wenn Sie später plattformspezifische Funktionen benötigen, [können Sie plattformspezifische Projekte hinzufügen](#add-platforms).

## <a name="steps-to-create-a-multiplatform-library-nuget"></a>Schritte zum Erstellen eines nuget-Bibliothek für mehrere Plattformen

1. Wählen Sie **Datei > neue Projekt Mappe** aus (oder klicken Sie mit der rechten Maustaste auf eine vorhandene Projekt Mappe, und wählen Sie **Hinzufügen >**

2. Wählen Sie im Abschnitt für die **> Bibliothek für mehrere Plattformen** die Option **Multiplattform-Bibliothek**

   [![Konfigurieren der Bibliothek für mehrere Plattformen für eine einzelne Codebasis](single-codebase-images/mulitplatform-library-sml.png)](single-codebase-images/mulitplatform-library.png#lightbox)

3. Geben Sie einen **Namen** und eine **Beschreibung**ein, und wählen Sie **für alle Plattformen Single aus**:

   [![Konfigurieren der Bibliothek für mehrere Plattformen für eine einzelne Codebasis](single-codebase-images/single-configure-sml.png)](single-codebase-images/single-configure.png#lightbox)

4. Schließen Sie den Assistenten ab. In der Projekt Mappe wird ein einzelnes Bibliotheksprojekt erstellt.

5. Klicken Sie mit der rechten Maustaste auf das neue Bibliotheksprojekt, und wählen Sie dann **Optionen**aus. Der Abschnitt **Build > allgemein** ermöglicht das Festlegen des **Ziel Frameworks** – auswählen eines portablen .net-PCL-Profils oder einer .NET Standard Version:

   [![Wählen Sie eine PCL oder .NET Standard für den Bibliothekstyp](single-codebase-images/single-choose-type-sml.png)](single-codebase-images/single-choose-type.png#lightbox)

6. Öffnen Sie außerdem im Fenster " **Projektoptionen** " den Abschnitt " **nuget-Paket > Metadaten** ", und geben Sie die [erforderlichen Metadaten](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md) (sowie alle optionalen Metadaten) ein:

   [![Erforderliche Metadaten eingeben](single-codebase-images/single-metadata-sml.png)](single-codebase-images/single-metadata.png#lightbox)

7. Klicken Sie mit der rechten Maustaste auf das Bibliotheksprojekt, und wählen Sie **nuget-Paket erstellen** (oder die Projekt Mappe erstellen oder bereitstellen) aus, und die **nupkg** -nuget-Paketdatei wird im Ordner **/bin/** gespeichert (abhängig von der Konfiguration entweder Debuggen oder freigeben):

   ![Die nuget-Paketdatei wird abhängig von der Konfiguration im bin-Ordner entweder Debuggen oder Release gespeichert.](single-codebase-images/create-nuget-package.png)

## <a name="verifying-the-output"></a>Überprüfen der Ausgabe

Nuget-Pakete sind auch ZIP-Dateien, sodass es möglich ist, die interne Struktur des generierten Pakets zu überprüfen.

Dieser Screenshot zeigt den Inhalt eines PCL-basierten nuget-– es ist nur eine einzige PCL-Assembly enthalten:

![Im nuget-Paket enthaltene Dateien](single-codebase-images/nuget-output.png)

<a name="add-platforms"></a>

## <a name="adding-platform-specific-code"></a>Hinzufügen von Platt Form spezifischem Code

PCL-basierte Projekte und .NET Standard basierte Projekte dürfen keine plattformspezifischen Verweise enthalten (z. b. IOS-oder Android-Funktionen).

Wenn ein vorhandenes PCL-Projekt oder .NET Standard Projekt erweitert werden muss, um plattformspezifischen Code einzuschließen, klicken Sie hierzu mit der rechten Maustaste auf das Projekt, und wählen **Sie hinzufügen > Plattform-Implementierung hinzu**fügen aus:

[![Menü zur Platt Form Implementierung hinzufügen](single-codebase-images/add-later-sml.png)](single-codebase-images/add-later.png#lightbox)

Ein oder mehrere Platt Form Projekte können der Projekt Mappe hinzugefügt werden, und die vorhandene PCL oder .NET Standard Bibliothek kann optional in ein frei gegebenes Projekt konvertiert werden:

[![Hinzufügen von Platt Form Optionen wie IOS, Android und frei gegebenes Projekt](single-codebase-images/add-later-platforms-sml.png)](single-codebase-images/add-later-platforms-sml.png#lightbox)

Nachdem Sie in ein frei gegebenes Projekt umgerechnet haben, besuchen Sie den Abschnitt **Projektoptionen > nuget-Paket >** Verweisassemblys, 
 [section](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/platform-specific.md) und stellen Sie sicher, dass alle erforderlichen Profile ausgewählt sind (damit das nuget weiterhin mit Projekten kompatibel ist, die zuvor in verwendet wurden).

## <a name="related-links"></a>Verwandte Links

- [Metadatenhandbuch](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md)
