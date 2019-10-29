---
title: Erstellen einer neuen Bibliothek für mehrere Plattformen für nuget
description: In diesem Dokument wird beschrieben, wie Sie eine Bibliothek für mehrere Plattformen für die Verwendung mit nuget erstellen. Dieses Verfahren eignet sich für Geschäftslogik und Algorithmen, die vollständig in der .net-Basisklassen Bibliothek ausgedrückt werden können und daher auf allen Zielplattformen ohne plattformspezifischen Code ausgeführt werden können.
ms.prod: xamarin
ms.assetid: E7B55354-9BBE-4122-BCE3-3506B79090DD
author: davidortinau
ms.author: daortin
ms.date: 03/23/2017
ms.openlocfilehash: 7dabb420aa094e67fae689f47b3b64a8fe1a6ed4
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73016705"
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

   [![](single-codebase-images/mulitplatform-library-sml.png "Configure multi-platform library for a single code base")](single-codebase-images/mulitplatform-library.png#lightbox)

3. Geben Sie einen **Namen** und eine **Beschreibung**ein, und wählen Sie **für alle Plattformen Single aus**:

   [![](single-codebase-images/single-configure-sml.png "Configure multi-platform library for a single code base")](single-codebase-images/single-configure.png#lightbox)

4. Durchlaufen Sie den Assistenten. In der Projekt Mappe wird ein einzelnes Bibliotheksprojekt erstellt.

5. Klicken Sie mit der rechten Maustaste auf das neue Bibliotheksprojekt, und wählen Sie dann **Optionen**aus. Der Abschnitt **Build > Allgemein** ermöglicht das Festlegen des **Ziel Frameworks** – auswählen eines portablen .net-PCL-Profils oder einer .NET Standard Version:

   [![](single-codebase-images/single-choose-type-sml.png "Choose PCL or .NET Standard for library type")](single-codebase-images/single-choose-type.png#lightbox)

6. Öffnen Sie außerdem im Fenster " **Projektoptionen** " den Abschnitt " **nuget-Paket > Metadaten** ", und geben Sie die [erforderlichen Metadaten](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md) (sowie alle optionalen Metadaten) ein:

   [![](single-codebase-images/single-metadata-sml.png "Enter required metadata")](single-codebase-images/single-metadata.png#lightbox)

7. Klicken Sie mit der rechten Maustaste auf das Bibliotheksprojekt, und wählen Sie **nuget-Paket erstellen** (oder die Projekt Mappe erstellen oder bereitstellen) aus, und die **nupkg** -nuget-Paketdatei wird im Ordner **/bin/** gespeichert (abhängig von der Konfiguration entweder Debuggen oder freigeben):

   ![](single-codebase-images/create-nuget-package.png "The NuGet package file will be saved in the bin folder either Debug or Release, depending on configuration")

## <a name="verifying-the-output"></a>Überprüfen der Ausgabe

Nuget-Pakete sind auch ZIP-Dateien, sodass es möglich ist, die interne Struktur des generierten Pakets zu überprüfen.

Dieser Screenshot zeigt den Inhalt eines PCL-basierten nuget-– es ist nur eine einzige PCL-Assembly enthalten:

![](single-codebase-images/nuget-output.png "Files contained in the NuGet package")

<a name="add-platforms" />

## <a name="adding-platform-specific-code"></a>Hinzufügen von Platt Form spezifischem Code

PCL-basierte Projekte und .NET Standard basierte Projekte dürfen keine plattformspezifischen Verweise enthalten (z. b. IOS-oder Android-Funktionen).

Wenn ein vorhandenes PCL-Projekt oder .NET Standard Projekt erweitert werden muss, um plattformspezifischen Code einzuschließen, klicken Sie hierzu mit der rechten Maustaste auf das Projekt, und wählen **Sie hinzufügen > Plattform-Implementierung hinzu**fügen aus:

[![](single-codebase-images/add-later-sml.png "Add platform implementation menu")](single-codebase-images/add-later.png#lightbox)

Ein oder mehrere Platt Form Projekte können der Projekt Mappe hinzugefügt werden, und die vorhandene PCL oder .NET Standard Bibliothek kann optional in ein frei gegebenes Projekt konvertiert werden:

[![](single-codebase-images/add-later-platforms-sml.png "Add platform options such as iOS, Android, and Shared Project")](single-codebase-images/add-later-platforms-sml.png#lightbox)

Nachdem Sie in ein frei gegebenes Projekt umgerechnet haben, besuchen Sie den [Abschnitt](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/platform-specific.md) **Projektoptionen > nuget-Paket >** Verweisassemblys
, und stellen Sie sicher, dass alle erforderlichen Profile ausgewählt sind (damit das nuget weiterhin mit Projekten kompatibel ist, die es zuvor in verwendet).

## <a name="related-links"></a>Verwandte Links

- [Leitfaden zu Metadaten](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md)
